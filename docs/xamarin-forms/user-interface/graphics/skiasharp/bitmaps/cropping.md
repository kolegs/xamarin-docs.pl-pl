---
title: Przycinanie SkiaSharp map bitowych
description: Dowiedz się, jak projektować interfejs użytkownika interaktywnego desribing prostokąt przycinania za pomocą SkiaSharp.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 0A79AB27-C69F-4376-8FFE-FF46E4783F30
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 3dd9011d19e77f52d1fe89a37e4d992c23c72ab1
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615551"
---
# <a name="cropping-skiasharp-bitmaps"></a>Przycinanie SkiaSharp map bitowych

[ **Tworzenie i mapy bitowe SkiaSharp rysowania** ](drawing.md) artykule opisano sposób `SKBitmap` obiekt może być przekazywany do `SKCanvas` konstruktora. Wszystkie metody rysowania o nazwie na tym grafiki powoduje, że kanwy renderowany na mapę bitową. Obejmują one rysowania metody `DrawBitmap`, co oznacza, że ta technika umożliwia transfer część lub całość jednym mapy bitowej do innej mapy bitowej, być może przy użyciu transformacji zastosowanych.

Możesz użyć tej techniki dla przycinanie mapy bitowej, wywołując [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) metody z prostokątami źródłowe i docelowe:

```csharp
canvas.DrawBitmap(bitmap, sourceRect, destRect);
```

Jednak aplikacje, które implementują przycinanie często zawierają interfejs użytkownika interaktywnego wybierz prostokąt przycinania:

![Przycinanie przykładowe](cropping-images/CroppingSample.png "przycinanie próbki")

Ten artykuł koncentruje się na tym interfejsie.

## <a name="encapsulating-the-cropping-rectangle"></a>Zawieranie prostokąt przycinania

Jest przydatne do izolowania część logiki przycinania w klasie o nazwie `CroppingRectangle`. Parametry Konstruktora obejmują maksymalny prostokąt, który jest zwykle rozmiar mapy bitowej przycinanie, i opcjonalnie współczynnik proporcji. Konstruktor najpierw definiuje początkowe prostokąt przycinania, co czyni go jest publiczna w `Rect` właściwości typu `SKRect`. Ta początkowa prostokąt przycinania jest 80% szerokość i wysokość prostokąta mapy bitowej, ale jest następnie korygowany, jeśli jest określony współczynnik proporcji:

```csharp
class CroppingRectangle
{
    ···
    SKRect maxRect;             // generally the size of the bitmap
    float? aspectRatio;

    public CroppingRectangle(SKRect maxRect, float? aspectRatio = null)
    {
        this.maxRect = maxRect;
        this.aspectRatio = aspectRatio;

        // Set initial cropping rectangle
        Rect = new SKRect(0.9f * maxRect.Left + 0.1f * maxRect.Right,
                          0.9f * maxRect.Top + 0.1f * maxRect.Bottom,
                          0.1f * maxRect.Left + 0.9f * maxRect.Right,
                          0.1f * maxRect.Top + 0.9f * maxRect.Bottom);

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            SKRect rect = Rect;
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;
                rect.Left = (maxRect.Width - width) / 2;
                rect.Right = rect.Left + width;
            }
            else
            {
                float height = rect.Width / aspect;
                rect.Top = (maxRect.Height - height) / 2;
                rect.Bottom = rect.Top + height;
            }

            Rect = rect;
        }
    }
    
    public SKRect Rect { set; get; }
    ···
}
```

Jeden fragment przydatne informacje, `CroppingRectangle` sprawia, że dostępna jest także szereg `SKPoint` odpowiadający dowiedzą o prostokąt przycinania w kolejności, w lewym górnym, prawym górnym rogu, prawej dolnej i lewej dolnej wartości:

```csharp
class CroppingRectangle
{
    ···
    public SKPoint[] Corners
    {
        get
        {
            return new SKPoint[]
            {
                new SKPoint(Rect.Left, Rect.Top),
                new SKPoint(Rect.Right, Rect.Top),
                new SKPoint(Rect.Right, Rect.Bottom),
                new SKPoint(Rect.Left, Rect.Bottom)
            };
        }
    }
    ···
}
```

Ta tablica jest używana w następującą metodę, która jest wywoływana `HitTest`. `SKPoint` Parametru jest punktem odpowiadający touch palca, lub kliknij przycisk myszy. Metoda ta zwraca indeks (0, 1, 2 lub 3) odpowiadający rogu dotknięciu wskaźnik myszy lub linii papilarnych, w odległości podane przez `radius` parametru: 

```csharp
class CroppingRectangle
{
    ···
    public int HitTest(SKPoint point, float radius)
    {
        SKPoint[] corners = Corners;

        for (int index = 0; index < corners.Length; index++)
        {
            SKPoint diff = point - corners[index];
                
            if ((float)Math.Sqrt(diff.X * diff.X + diff.Y * diff.Y) < radius)
            {
                return index;
            }
        }

        return -1;
    }
    ···
}
```

Jeśli punkt dotykowej lub myszy nie jest w ramach `radius` jednostek dowolnego zakątka, metoda zwraca &ndash;1.

W ostatnim metody `CroppingRectangle` nosi nazwę `MoveCorner`, która jest wywoływana w odpowiedzi, ani myszy przepływu. Dwa parametry wskazuje indeks rogu przenoszone i nową lokalizację tego rogu. W pierwszej połowie metody dostosowuje prostokąt przycinania na podstawie nowej lokalizacji, w prawym górnym rogu, ale zawsze w granicach `maxRect`, czyli rozmiar mapy bitowej. Tę logikę również uwzględnia `MINIMUM` pola, aby uniknąć zwijanie prostokąt przycinania na nic:

```csharp
class CroppingRectangle
{
    const float MINIMUM = 10;   // pixels width or height
    ···
    public void MoveCorner(int index, SKPoint point)
    {
        SKRect rect = Rect;

        switch (index)
        {
            case 0: // upper-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 1: // upper-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 2: // lower-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;

            case 3: // lower-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;
        }

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;

                switch (index)
                {
                    case 0:
                    case 3: rect.Left = rect.Right - width; break;
                    case 1:
                    case 2: rect.Right = rect.Left + width; break;
                }
            }
            else
            {
                float height = rect.Width / aspect;

                switch (index)
                {
                    case 0:
                    case 1: rect.Top = rect.Bottom - height; break;
                    case 2:
                    case 3: rect.Bottom = rect.Top + height; break;
                }
            }
        }

        Rect = rect;
    }
}
```

W drugiej połowie metody dopasowuje dla opcjonalne współczynnik proporcji.

Należy pamiętać, że wszystkie elementy w tej klasie znajduje się w jednostkach pikseli.

## <a name="a-canvas-view-just-for-cropping"></a>Widok kanwy tylko na potrzeby przycinania

`CroppingRectangle` Klasa właśnie została przedstawiona jest używana przez `PhotoCropperCanvasView` klasy, która jest pochodną `SKCanvasView`. Ta klasa jest odpowiedzialny za wyświetlanie mapy bitowej i prostokąt przycinania, a także do obsługi dotykowej lub myszy zdarzeń zmiany prostokąt przycinania.

`PhotoCropperCanvasView` Konstruktor wymaga mapy bitowej. Współczynnik proporcji jest opcjonalne. Konstruktor tworzy wystąpienie obiektu typu `CroppingRectangle` na podstawie tej mapy bitowej i współczynnik proporcji i zapisuje go jako pola:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        this.bitmap = bitmap;

        SKRect bitmapRect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
        croppingRect = new CroppingRectangle(bitmapRect, aspectRatio);
        ···
    }
    ···
}
```

Ponieważ ta klasa jest pochodną `SKCanvasView`, nie trzeba zainstalować program obsługi `PaintSurface` zdarzeń. Zamiast tego można zastąpić jej `OnPaintSurface` metody. Metoda Wyświetla mapę bitową i korzysta z kilku `SKPaint` obiektów zapisane jako pola, aby narysować prostokąt przycinania bieżący:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    const int CORNER = 50;      // pixel length of cropper corner
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;
    ···
    // Drawing objects
    SKPaint cornerStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 10
    };

    SKPaint edgeStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 2
    };
    ···
    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        base.OnPaintSurface(args);

        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Gray);

        // Calculate rectangle for displaying bitmap 
        float scale = Math.Min((float)info.Width / bitmap.Width, (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect bitmapRect = new SKRect(x, y, x + scale * bitmap.Width, y + scale * bitmap.Height);
        canvas.DrawBitmap(bitmap, bitmapRect);

        // Calculate a matrix transform for displaying the cropping rectangle
        SKMatrix bitmapScaleMatrix = SKMatrix.MakeIdentity();
        bitmapScaleMatrix.SetScaleTranslate(scale, scale, x, y);

        // Display rectangle
        SKRect scaledCropRect = bitmapScaleMatrix.MapRect(croppingRect.Rect);
        canvas.DrawRect(scaledCropRect, edgeStroke);

        // Display heavier corners
        using (SKPath path = new SKPath())
        {
            path.MoveTo(scaledCropRect.Left, scaledCropRect.Top + CORNER);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Left + CORNER, scaledCropRect.Top);

            path.MoveTo(scaledCropRect.Right - CORNER, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top + CORNER);

            path.MoveTo(scaledCropRect.Right, scaledCropRect.Bottom - CORNER);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Right - CORNER, scaledCropRect.Bottom);

            path.MoveTo(scaledCropRect.Left + CORNER, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom - CORNER);

            canvas.DrawPath(path, cornerStroke);
        }

        // Invert the transform for touch tracking
        bitmapScaleMatrix.TryInvert(out inverseBitmapMatrix);
    }
    ···
}
```

Kod w `CroppingRectangle` klasy Określa prostokąt przycinania na rozmiar mapy bitowej w pikselach. Jednakże wyświetlanie mapy bitowej przy `PhotoCropperCanvasView` klasy jest skalowana zależnie od rozmiaru obszaru wyświetlania. `bitmapScaleMatrix` Obliczeniowe w `OnPaintSurface` zastąpienia mapy z pikseli mapy bitowej do rozmiaru i położenia mapy bitowej, tak jak jest wyświetlana. Ta macierz jest następnie używany do przekształcania prostokąt przycinania, aby mogły zostać wyświetlone względem mapy bitowej.

Ostatni wiersz `OnPaintSurface` zastąpienie przyjmuje odwrotność `bitmapScaleMatrix` i zapisuje go jako `inverseBitmapMatrix` pola. Służy do przetwarzania touch.

A `TouchEffect` jako pole jest tworzone wystąpienie obiektu, a Konstruktor dołącza obsługę do `TouchAction` zdarzenia, ale `TouchEffect` musi zostać dodane do `Effects` zbiór _nadrzędnego_ z `SKCanvasView`pochodnych, więc, że użytkownika wykonane `OnParentSet` zastąpienia:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    const int RADIUS = 100;     // pixel radius of touch hit-test
    ···
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;

    // Touch tracking 
    TouchEffect touchEffect = new TouchEffect();
    struct TouchPoint
    {
        public int CornerIndex { set; get; }
        public SKPoint Offset { set; get; }
    }

    Dictionary<long, TouchPoint> touchPoints = new Dictionary<long, TouchPoint>();
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        ···
        touchEffect.TouchAction += OnTouchEffectTouchAction;
    }
    ···
    protected override void OnParentSet()
    {
        base.OnParentSet();

        // Attach TouchEffect to parent view
        Parent.Effects.Add(touchEffect);
    }
    ···
    void OnTouchEffectTouchAction(object sender, TouchActionEventArgs args)
    {
        SKPoint pixelLocation = ConvertToPixel(args.Location);
        SKPoint bitmapLocation = inverseBitmapMatrix.MapPoint(pixelLocation);

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Convert radius to bitmap/cropping scale
                float radius = inverseBitmapMatrix.ScaleX * RADIUS;

                // Find corner that the finger is touching
                int cornerIndex = croppingRect.HitTest(bitmapLocation, radius);

                if (cornerIndex != -1 && !touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = new TouchPoint
                    {
                        CornerIndex = cornerIndex,
                        Offset = bitmapLocation - croppingRect.Corners[cornerIndex]
                    };

                    touchPoints.Add(args.Id, touchPoint);
                }
                break;

            case TouchActionType.Moved:
                if (touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = touchPoints[args.Id];
                    croppingRect.MoveCorner(touchPoint.CornerIndex, 
                                            bitmapLocation - touchPoint.Offset);
                    InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchPoints.ContainsKey(args.Id))
                {
                    touchPoints.Remove(args.Id);
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Xamarin.Forms.Point pt)
    {
        return new SKPoint((float)(CanvasSize.Width * pt.X / Width),
                           (float)(CanvasSize.Height * pt.Y / Height));
    }
}
```

Zdarzenia dotykowe przetwarzane przez `TouchAction` obsługi znajdują się w jednostkach niezależnych od urządzenia. Te najpierw muszą zostać przekonwertowane na pikseli przy użyciu `ConvertToPixel` metody w dolnej części tej klasy, a następnie konwertowana do `CroppingRectangle` jednostek przy użyciu `inverseBitmapMatrix`.

Aby uzyskać `Pressed` zdarzenia, `TouchAction` obsługi zdarzeń wywołuje `HitTest` metody `CroppingRectangle`. Jeśli zostanie zwrócona indeksu innych niż &ndash;1, następnie jednym z jej narożników prostokąta kadrowania jest podlegający manipulowaniu. Czy indeks i przesunięcie punktu rzeczywiste touch w prawym górnym rogu znajduje się w `TouchPoint` obiektu i dodane do `touchPoints` słownika.

Aby uzyskać `Moved` zdarzeń, `MoveCorner` metody `CroppingRectangle` jest wywoływana, aby przenieść róg, przy użyciu możliwości dostosowania do współczynnik proporcji.

W dowolnym momencie za pomocą programu `PhotoCropperCanvasView` mogą uzyskiwać dostęp do `CroppedBitmap` właściwości. Ta właściwość używa `Rect` właściwość `CroppingRectangle` do utworzenia nowej mapy bitowej o rozmiarze przycięte. Wersja `DrawBitmap` ze źródłowym i docelowym prostokąty następnie wyodrębnia podzbiór oryginalnego mapy bitowej:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public SKBitmap CroppedBitmap
    {
        get
        {
            SKRect cropRect = croppingRect.Rect;
            SKBitmap croppedBitmap = new SKBitmap((int)cropRect.Width, 
                                                  (int)cropRect.Height);
            SKRect dest = new SKRect(0, 0, cropRect.Width, cropRect.Height);
            SKRect source = new SKRect(cropRect.Left, cropRect.Top, 
                                       cropRect.Right, cropRect.Bottom);

            using (SKCanvas canvas = new SKCanvas(croppedBitmap))
            {
                canvas.DrawBitmap(bitmap, source, dest);
            }

            return croppedBitmap;
        }
    }
    ···
}
```

## <a name="hosting-the-photo-cropper-canvas-view"></a>Hostowanie widoku kanwy przycinania zdjęć

Za pomocą tych dwóch klas przycinania logika obsługi **przycinanie zdjęcie** strony w **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikacja ma bardzo mało pracy do wykonania. Tworzy plik XAML `Grid` hosta `PhotoCropperCanvasView` i **gotowe** przycisku:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoCroppingPage"
             Title="Photo Cropping">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid x:Name="canvasViewHost"
              Grid.Row="0"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="1"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

`PhotoCropperCanvasView` Nie może być utworzona w pliku XAML, ponieważ wymaga parametru typu `SKBitmap`.

Zamiast tego `PhotoCropperCanvasView` zostanie uruchomiony w Konstruktorze pliku związanego z kodem przy użyciu jednej z mapy bitowe zasobów:

```csharp
public partial class PhotoCroppingPage : ContentPage
{
    PhotoCropperCanvasView photoCropper;
    SKBitmap croppedBitmap;

    public PhotoCroppingPage ()
    {
        InitializeComponent ();

        SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        photoCropper = new PhotoCropperCanvasView(bitmap);
        canvasViewHost.Children.Add(photoCropper);
    }

    void OnDoneButtonClicked(object sender, EventArgs args)
    {
        croppedBitmap = photoCropper.CroppedBitmap;

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(croppedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Użytkownik może następnie manipulować prostokąt przycinania:

[![Photo przycinania 1](cropping-images/PhotoCropping1.png "Photo przycinania 1")](cropping-images/PhotoCropping1-Large.png#lightbox)

Po zdefiniowaniu dobre prostokąt przycinania kliknij **gotowe** przycisku. `Clicked` Obsługi uzyskuje przycięty mapę bitową z `CroppedBitmap` właściwość `PhotoCropperCanvasView`i zastępuje całą zawartość strony z nową `SKCanvasView` obiekt, który wyświetla ten przycięty mapę bitową:

[![Photo przycinania 2](cropping-images/PhotoCropping2.png "Photo przycinania 2")](cropping-images/PhotoCropping2-Large.png#lightbox)

Spróbuj ustawić drugi argument `PhotoCropperCanvasView` do 1.78f (na przykład):

```csharp
photoCropper = new PhotoCropperCanvasView(bitmap, 1.78f);
```

Zobaczysz prostokąt przycinania, ograniczony do 16-9 współczynnik proporcji, charakterystycznych dla telewizji o wysokiej rozdzielczości.

<a name="tile-division" />

## <a name="dividing-a-bitmap-into-tiles"></a>Dzielenie map bitowych na kafelkach

Wersja zestawu narzędzi Xamarin.Forms sławę układanki 14-15 pojawiła się 22 rozdział książki [ _tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms_ ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) i można je pobrać jako [  **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle). Jednak układanki staje się fajne (i często trudniejsze) gdy jest on oparty na obraz z biblioteki zdjęć.

Ta wersja układanki 14-15 jest częścią **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikacji i składa się z szeregu stron pod tytułem **Układanka zdjęć —**.

**PhotoPuzzlePage1.xaml** pliku składa się z `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage1"
             Title="Photo Puzzle">
    
    <Button Text="Pick a photo from your library"
            VerticalOptions="CenterAndExpand" 
            HorizontalOptions="CenterAndExpand"
            Clicked="OnPickButtonClicked"/>
    
</ContentPage>
```

Plik związany z kodem implementuje `Clicked` program obsługi, który używa `IPhotoLibrary` usługi zależności, aby umożliwić użytkownikowi wybrać zdjęcia z biblioteki zdjęć:

```csharp
public partial class PhotoPuzzlePage1 : ContentPage
{
    public PhotoPuzzlePage1 ()
    {
        InitializeComponent ();
    }

    async void OnPickButtonClicked(object sender, EventArgs args)
    {
        IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
        using (Stream stream = await photoLibrary.PickPhotoAsync())
        {
            if (stream != null)
            {
                SKBitmap bitmap = SKBitmap.Decode(stream);

                await Navigation.PushAsync(new PhotoPuzzlePage2(bitmap));
            }
        }
    }
}
```

Metoda następnie powoduje przejście do `PhotoPuzzlePage2`, przekazując do Konstruktor wybranej mapy bitowej.

Istnieje możliwość, że zdjęcie wybrane z biblioteki nie jest zorientowany pojawiły się w bibliotece zdjęć, ale jest obracany czy nogami. (Jest to szczególnie problem z urządzeń z systemem iOS). Z tego powodu `PhotoPuzzlePage2` umożliwia obrócić obraz, który ma odpowiednią orientację. Plik XAML zawiera trzy przyciski **90&#x00B0; po prawej stronie** (to znaczy zgodnie ze wskazówkami zegara), **90&#x00B0; po lewej stronie** (do ruchu wskazówek zegara), a **gotowe**.

Plik związany z kodem implementuje logikę obrotu mapy bitowej, pokazano w artykule  **[tworzenia i rysowania na mapy bitowe SkiaSharp](drawing.md#rotating-bitmaps)**. Użytkownik może obracać obraz 90 stopni w prawo lub lewo dowolną liczbę razy: 

```csharp
public partial class PhotoPuzzlePage2 : ContentPage
{
    SKBitmap bitmap;

    public PhotoPuzzlePage2 (SKBitmap bitmap)
    {
        this.bitmap = bitmap;

        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnRotateRightButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(bitmap.Height, 0);
            canvas.RotateDegrees(90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnRotateLeftButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(0, bitmap.Width);
            canvas.RotateDegrees(-90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        await Navigation.PushAsync(new PhotoPuzzlePage3(bitmap));
    }
}
```

Po kliknięciu przez użytkownika **gotowe** przycisku `Clicked` obsługi przechodzi do `PhotoPuzzlePage3`, przekazując końcowego obrócony mapy bitowej w Konstruktorze strony.

`PhotoPuzzlePage3` Umożliwia zdjęcie można przycięcia. Program wymaga kwadratowy mapy bitowej podzielić na 4-na-4 siatkę kafelków.

**PhotoPuzzlePage3.xaml** plik zawiera `Label`, `Grid` hosta `PhotoCropperCanvasView`, a inny **gotowe** przycisku:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage3"
             Title="Photo Puzzle">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Label Text="Crop the photo to a square"
               Grid.Row="0"
               FontSize="Large"
               HorizontalTextAlignment="Center"
               Margin="5" />

        <Grid x:Name="canvasViewHost"
              Grid.Row="1"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="2"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

Tworzy plik związany z kodem `PhotoCropperCanvasView` z mapą bitową przekazywany do jej konstruktora. Należy zauważyć, że 1 jest przekazywany jako drugi argument `PhotoCropperCanvasView`. Prostokąt przycinania, być kwadratowe wymusza na ten współczynnik proporcji 1:

```csharp
public partial class PhotoPuzzlePage3 : ContentPage
{
    PhotoCropperCanvasView photoCropper;

    public PhotoPuzzlePage3(SKBitmap bitmap)
    {
        InitializeComponent ();

        photoCropper = new PhotoCropperCanvasView(bitmap, 1f);
        canvasViewHost.Children.Add(photoCropper);
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        SKBitmap croppedBitmap = photoCropper.CroppedBitmap;
        int width = croppedBitmap.Width / 4;
        int height = croppedBitmap.Height / 4;

        ImageSource[] imgSources = new ImageSource[15];

        for (int row = 0; row < 4; row++)
        {
            for (int col = 0; col < 4; col++)
            {
                // Skip the last one!
                if (row == 3 && col == 3)
                    break;

                // Create a bitmap 1/4 the width and height of the original
                SKBitmap bitmap = new SKBitmap(width, height);
                SKRect dest = new SKRect(0, 0, width, height);
                SKRect source = new SKRect(col * width, row * height, (col + 1) * width, (row + 1) * height);

                // Copy 1/16 of the original into that bitmap
                using (SKCanvas canvas = new SKCanvas(bitmap))
                {
                    canvas.DrawBitmap(croppedBitmap, source, dest);
                }

                imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
            }
        }

        await Navigation.PushAsync(new PhotoPuzzlePage4(imgSources));
    }
}
```

**Gotowe** obsługi przycisk uzyskuje szerokość i wysokość przycięty mapy bitowej (te dwie wartości powinna być taka sama) i jest podzielony na 15 oddzielne mapy bitowe, z których każdy jest 1/4 szerokość i wysokość oryginalnego. (Ostatni możliwe 16 mapy bitowe nie zostanie utworzony.) `DrawBitmap` Metody z prostokąta źródłowego i docelowego umożliwia mapy bitowej można tworzyć w oparciu o podzbiór większych mapy bitowej.

## <a name="converting-to-xamarinforms-bitmaps"></a>Konwertowanie map bitowych zestawu narzędzi Xamarin.Forms

W `OnDoneButtonClicked` metody tablicy utworzone dla 15 mapy bitowe jest typu [ `ImageSource` ](xref:Xamarin.Forms.ImageSource):

```csharp
ImageSource[] imgSources = new ImageSource[15];
```

`ImageSource` jest platformy Xamarin.Forms typu podstawowego, który hermetyzuje mapę bitową. Na szczęście SkiaSharp umożliwia konwersji z mapy bitowe SkiaSharp mapy bitowe zestawu narzędzi Xamarin.Forms. **SkiaSharp.Views.Forms** definiuje zestaw [ `SKBitmapImageSource` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKBitmapImageSource/) klasę pochodzącą od `ImageSource` , ale można tworzyć w oparciu o SkiaSharp `SKBitmap` obiektu. `SKBitmapImageSource` nawet definiuje konwersje między `SKBitmapImageSource` i `SKBitmap`i jak `SKBitmap` obiekty są przechowywane w tablicy jako zestawu narzędzi Xamarin.Forms map bitowych:

```csharp
imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
```

Ta tablica mapy bitowe jest przekazywany jako konstruktor do `PhotoPuzzlePage4`. Ta strona jest całkowicie zestawu narzędzi Xamarin.Forms i nie używa żadnych SkiaSharp. Jest bardzo podobne do [ **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle), dzięki czemu nie będzie można opisać w tym miejscu, ale wyświetla wybranych zdjęć podzielony na 15 kafelków kwadrat:

[![Photo układanki 1](cropping-images/PhotoPuzzle1.png "Photo układanki 1")](cropping-images/PhotoPuzzle1-Large.png#lightbox)

Naciśnięcie klawisza **losowo** przycisk miesza się wszystkie Kafelki:

[![Photo układanki 2](cropping-images/PhotoPuzzle2.png "Photo układanki 2")](cropping-images/PhotoPuzzle2-Large.png#lightbox)

Możesz teraz umieścić je w odpowiedniej kolejności. Wszystkie Kafelki na tym samym wierszu lub kolumnie jako pusty kwadrat można wybierać przenieś je do pusty kwadrat. 

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
