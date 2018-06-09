---
title: Manipulacje Touch
description: W tym artykule wyjaśniono, jak użyć macierzy transformacji do zaimplementowania przeciąganie touch, punkty zaciskające i obrotu i pokazuje to z przykładowym kodzie.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/03/2018
ms.openlocfilehash: a53fe287e74070adb22c2a7c67d4b7cc10b35d3e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244289"
---
# <a name="touch-manipulations"></a>Manipulacje Touch

_Użyj macierzy przekształca do zaimplementowania przeciąganie touch, punkty zaciskające i obrotu_

W środowiskach wielodotyku, takich jak te na urządzeniach przenośnych użytkownicy często używać swoich palców do manipulowania obiektami na ekranie. Typowe gestów takich jak przeciąganie linii papilarnych jednego i uszczypnięcia palca dwa można przenieść i skalowanie obiektów lub nawet Obróć je. Gestów te zwykle są implementowane za pomocą macierzy transformacji, a w tym artykule przedstawiono, jak to zrobić.

![](touch-images/touchmanipulationsexample.png "Poddane translacji, skalowanie i obrót mapy bitowej")

## <a name="manipulating-one-bitmap"></a>Manipulowanie jeden mapy bitowej

**Touch manipulowania** strony pokazuje manipulacje touch na jednym mapy bitowej.
Ten przykład wykorzystuje efekt śledzenia touch przedstawione w artykule [wywoływanie zdarzeń od efekty](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Inne pliki zapewniają obsługę **Touch manipulowania** strony. Pierwsza to [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) wyliczenia, co oznacza różne typy manipulacji touch zaimplementowana przez kod widać będzie:

```csharp
enum TouchManipulationMode
{
    None,
    PanOnly,
    IsotropicScale,     // includes panning
    AnisotropicScale,   // includes panning
    ScaleRotate,        // implies isotropic scaling
    ScaleDualRotate     // adds one-finger rotation
}
```

`PanOnly` jest przeciągania palca jednej, która jest implementowana przy użyciu translacji. Wszystkie kolejne opcje także zawierać przesuwanie, ale obejmują dwoma palcami: `IsotropicScale` jest operacją uszczypnięcia, który daje w obiekcie jednakowo skalowania w poziomie i w pionie kierunkach. `AnisotropicScale` Umożliwia skalowanie nierówne.

`ScaleRotate` Opcja dotyczy dwóch palca skalowanie i obrót. Skalowanie jest izotropowego. Implementowanie obrotu dwa palca z anizotropowej skalowanie jest problemy, ponieważ przeniesień palca są zasadniczo takie same.

`ScaleDualRotate` Opcja dodaje jeden palca obrotu. Przeciąganego obiektu pojedynczego palca przeciąga obiektu, jest najpierw obrócona wokół środka tak, aby Centrum obiektu linii z wektora przeciągania.

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) plik zawiera `Picker` z elementami członkowskimi `TouchManipulationMode` wyliczenie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.TouchManipulationPage"
             Title="Touch Manipulation">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker Title="Touch Mode"
                Grid.Row="0"
                SelectedIndexChanged="OnTouchModePickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>None</x:String>
                <x:String>PanOnly</x:String>
                <x:String>IsotropicScale</x:String>
                <x:String>AnisotropicScale</x:String>
                <x:String>ScaleRotate</x:String>
                <x:String>ScaleDualRotate</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                4
            </Picker.SelectedIndex>
        </Picker>

        <Grid BackgroundColor="White"
              Grid.Row="1">

            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>
    </Grid>
</ContentPage>
```

W dolnej jest `SKCanvasView` i `TouchEffect` dołączony do pojedynczych komórek `Grid` który umieszcza go.

[ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) ma pliku CodeBehind `bitmap` pole, ale nie jest typu `SKBitmap`. Typ jest `TouchManipulationBitmap` (klasa pojawi się później):

```csharp
public partial class TouchManipulationPage : ContentPage
{
    TouchManipulationBitmap bitmap;
    ...

    public TouchManipulationPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.MountainClimbers.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            SKBitmap bitmap = SKBitmap.Decode(skStream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

Tworzy konstruktora `TouchManipulationBitmap` przekazania do konstruktora obiektu `SKBitmap` uzyskany z zasobu osadzonego. Konstruktor stwierdza, ustawiając `Mode` właściwość `TouchManager` właściwość `TouchManipulationBitmap` obiektu do elementu członkowskiego `TouchManipulationMode` wyliczenia.

`SelectedIndexChanged` Obsługę `Picker` ustawia to również `Mode` właściwości:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            TouchManipulationMode mode;
            Enum.TryParse(picker.Items[picker.SelectedIndex], out mode);
            bitmap.TouchManager.Mode = mode;
        }
    }
    ...
}
```

`TouchAction` Obsługi `TouchEffect` wystąpienia w wywołaniach pliku XAML dwie metody w `TouchManipulationBitmap` o nazwie `HitTest` i `ProcessTouchEvent`:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    List<long> touchIds = new List<long>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (bitmap.HitTest(point))
                {
                    touchIds.Add(args.Id);
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    break;
                }
                break;

            case TouchActionType.Moved:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    touchIds.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Jeśli `HitTest` metoda zwraca `true` &mdash; co oznacza, że palcem ma dotknięciu ekranu w obszarze zajmowane przez mapy bitowej &mdash; touch ID jest dodawana do `TouchIds` kolekcji. Ten identyfikator reprezentuje sekwencję zdarzeń touch dla tej linii papilarnych, dopóki palca wind na ekranie. Jeśli wiele palców touch mapę bitową, a następnie `touchIds` kolekcja zawiera identyfikator touch, dla każdej linii papilarnych.

`TouchAction` Również wywołuje program obsługi `ProcessTouchEvent` klasy w `TouchManipulationBitmap`. Jest to, gdy niektóre (ale nie wszystkie) rzeczywistych dotykowego przetwarzanie zachodzi.

[ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) Klasy to klasa otoki dla `SKBitmap` zawiera kod, aby renderować mapy bitowej i przetwarzania zdarzeń touch. Działa w połączeniu z więcej uogólniony kodu w `TouchManipulationManager` klasy (która pojawi się wkrótce).

`TouchManipulationBitmap` Zapisuje konstruktora `SKBitmap` i tworzy dwie właściwości `TouchManager` właściwości typu `TouchManipulationManager` i `Matrix` właściwości typu `SKMatrix`:

```csharp
class TouchManipulationBitmap
{
    SKBitmap bitmap;
    ...

    public TouchManipulationBitmap(SKBitmap bitmap)
    {
        this.bitmap = bitmap;
        Matrix = SKMatrix.MakeIdentity();

        TouchManager = new TouchManipulationManager
        {
            Mode = TouchManipulationMode.ScaleRotate
        };
    }

    public TouchManipulationManager TouchManager { set; get; }

    public SKMatrix Matrix { set; get; }
    ...
}
```

To `Matrix` właściwości jest skumulowany przekształcania wynikające z działania touch. Jak można zauważyć, każde zdarzenie touch zostanie rozwiązany w macierzy, która jest następnie łączony z `SKMatrix` wartości przechowywanej przez `Matrix` właściwości.

`TouchManipulationBitmap` Obiektu rysuje jego `Paint` metody. Argument jest `SKCanvas` obiektu. To `SKCanvas` mogą już mieć transformacji zastosować dla niego, więc `Paint` łączy metody `Matrix` właściwości skojarzone z mapy bitowej do istniejących transformacji i przywraca kanwę, po zakończeniu:

```csharp
class TouchManipulationBitmap
{
    ...
    public void Paint(SKCanvas canvas)
    {
        canvas.Save();
        SKMatrix matrix = Matrix;
        canvas.Concat(ref matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();
    }
    ...
}
```

`HitTest` Metoda zwraca `true` Jeśli użytkownik dotyka ekranu w punkcie w granicach mapy bitowej. Jako użytkownik modyfikuje mapy bitowej, mapy bitowej może obracać lub nawet (przy użyciu kombinacji anizotropowej skalowanie i obrót) można w kształcie równoległobok. Użytkownik może obawy, że `HitTest` metody należy zaimplementować raczej złożonych geometrii analityczne w takiej sytuacji.

Dostępna jest jednak skrótu:

Określenie, czy punkt znajduje się w granicach przekształcone prostokąt jest taka sama jak określenie, czy odwrotny przekształcony punkt znajduje się w granicach Nieprzekształcony prostokąta. Jest znacznie łatwiejsze obliczeń i może używać wygodne `Contains` metody zdefiniowanej przez `SKRect`:

```csharp
class TouchManipulationBitmap
{
    ...
    public bool HitTest(SKPoint location)
    {
        // Invert the matrix
        SKMatrix inverseMatrix;

        if (Matrix.TryInvert(out inverseMatrix))
        {
            // Transform the point using the inverted matrix
            SKPoint transformedPoint = inverseMatrix.MapPoint(location);

            // Check if it's in the untransformed bitmap rectangle
            SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
            return rect.Contains(transformedPoint);
        }
        return false;
    }
    ...
}
```

Druga metoda publiczna w `TouchManipulationBitmap` jest `ProcessTouchEvent`. Gdy ta metoda jest wywoływana, już ustalono że zdarzeń touch należy do tej określonej mapy bitowej. Metoda obsługuje słownik [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) obiektów, które jest po prostu poprzedniego punktu i nowy punkt każdej palca:

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Oto słownika i `ProcessTouchEvent` metody:

```csharp
class TouchManipulationBitmap
{
    ...
    Dictionary<long, TouchManipulationInfo> touchDictionary =
        new Dictionary<long, TouchManipulationInfo>();
    ...
    public void ProcessTouchEvent(long id, TouchActionType type, SKPoint location)
    {
        switch (type)
        {
            case TouchActionType.Pressed:
                touchDictionary.Add(id, new TouchManipulationInfo
                {
                    PreviousPoint = location,
                    NewPoint = location
                });
                break;

            case TouchActionType.Moved:
                TouchManipulationInfo info = touchDictionary[id];
                info.NewPoint = location;
                Manipulate();
                info.PreviousPoint = info.NewPoint;
                break;

            case TouchActionType.Released:
                touchDictionary[id].NewPoint = location;
                Manipulate();
                touchDictionary.Remove(id);
                break;

            case TouchActionType.Cancelled:
                touchDictionary.Remove(id);
                break;
        }
    }
    ...
}
```

W `Moved` i `Released` zdarzenia, wywołania metody `Manipulate`. W tych godzinach `touchDictionary` zawiera jeden lub więcej `TouchManipulationInfo` obiektów. Jeśli `touchDictionary` zawiera jeden element, istnieje duże prawdopodobieństwo, że `PreviousPoint` i `NewPoint` wartości są równe i reprezentują przepływu palcem. Jeśli wiele palców są dotknięcie mapy bitowej, słownik zawiera więcej niż jeden element, ale tylko jeden z tych elementów zawiera różne `PreviousPoint` i `NewPoint` wartości. Wszystkie pozostałe mają takie same `PreviousPoint` i `NewPoint` wartości.

Jest to ważne: `Manipulate` metody można założyć, że przetwarzania przepływu palca tylko jeden. W tym połączeniu żaden z innych palcami nie jest przenoszenie, i ich naprawdę przechodzenia (jak prawdopodobnie), tych przepływów będą przetwarzane w przyszłych wywołaniach `Manipulate`.

`Manipulate` Metody najpierw kopiuje słownik do tablicy jako udogodnienie. Ignoruje innym niż dwóch pierwszych wpisów. Jeśli więcej niż dwoma palcami podjęto próbę manipulowania mapy bitowej, inne są ignorowane. `Manipulate` jest ostatnim członkiem `TouchManipulationBitmap`:

```csharp
class TouchManipulationBitmap
{
    ...
    void Manipulate()
    {
        TouchManipulationInfo[] infos = new TouchManipulationInfo[touchDictionary.Count];
        touchDictionary.Values.CopyTo(infos, 0);
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();

        if (infos.Length == 1)
        {
            SKPoint prevPoint = infos[0].PreviousPoint;
            SKPoint newPoint = infos[0].NewPoint;
            SKPoint pivotPoint = Matrix.MapPoint(bitmap.Width / 2, bitmap.Height / 2);

            touchMatrix = TouchManager.OneFingerManipulate(prevPoint, newPoint, pivotPoint);
        }
        else if (infos.Length >= 2)
        {
            int pivotIndex = infos[0].NewPoint == infos[0].PreviousPoint ? 0 : 1;
            SKPoint pivotPoint = infos[pivotIndex].NewPoint;
            SKPoint newPoint = infos[1 - pivotIndex].NewPoint;
            SKPoint prevPoint = infos[1 - pivotIndex].PreviousPoint;

            touchMatrix = TouchManager.TwoFingerManipulate(prevPoint, newPoint, pivotPoint);
        }

        SKMatrix matrix = Matrix;
        SKMatrix.PostConcat(ref matrix, touchMatrix);
        Matrix = matrix;
    }
}
```

Jeśli jeden palca jest manipulowanie mapy bitowej, `Manipulate` wywołania `OneFingerManipulate` metody `TouchManipulationManager` obiektu. Dla dwoma palcami wywołuje `TwoFingerManipulate`. Argumenty metody te są takie same: `prevPoint` i `newPoint` palca, które porusza się reprezentować argumentów. Ale `pivotPoint` argument różni się w dwóch wywołania:

Do manipulacji palca jeden `pivotPoint` Centrum mapy bitowej. To umożliwić obrotu jeden palca. Do manipulacji palca dwa zdarzenia wskazuje przepływu tylko jeden palca, dzięki czemu `pivotPoint` jest palca, który nie jest przenoszona.

W obu przypadkach `TouchManipulationManager` zwraca `SKMatrix` wartość, która metoda łączy się z bieżącym `Matrix` właściwości który `TouchManipulationPage` używany do renderowania mapy bitowej.

`TouchManipulationManager` uogólniony i korzysta z żadnych innych plików z wyjątkiem `TouchManipulationMode`. Można użyć tej klasy bez zmian w aplikacjach. Definiuje właściwości jednego typu `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


Jednak prawdopodobnie należy unikać `AnisotropicScale` opcji. Jest bardzo łatwe przy użyciu tej opcji do manipulowania mapę bitową tak, aby jednym z czynników skalowania wynosi zero. Dzięki temu mapy bitowej są usuwane z procesów, nigdy nie do zwrócenia. Jeśli naprawdę konieczne anizotropowej skalowania, chcesz zwiększyć logiki, aby uniknąć niepożądane wyniki.

`TouchManipulationManager` wykorzystuje wektorów, ale ponieważ nie istnieje żadne `SKVector` struktury SkiaSharp, `SKPoint` zamiast niego jest używana. `SKPoint` operator odejmowania i wynik może być traktowana jako wektor obsługuje. Logika tylko specyficzne dla wektora, który wymagany do dodania jest `Magnitude` obliczenie:

```csharp
class TouchManipulationManager
{
    ...
    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
}
```

Zawsze, gdy został wybrany obracanie, metod manipulowania palca jednego i dwa palca najpierw obsługi obrót. W przypadku wykrycia obracania składnika obrotu skutecznie zostaną usunięte. Co to jest jest interpretowany jako przesuwać i skalowania.

Oto `OneFingerManipulate` metody. Jeden palca obrotu nie została włączona, a następnie logiki jest proste &mdash; po prostu używa poprzedniego punktu i nowy punkt do utworzenia wektora o nazwie `delta` odpowiada dokładnie tłumaczenia. Z obrotu palca jeden włączony metoda wykorzystuje kąty z punktu obrotu (center mapy bitowej) do poprzedniego punktu i nowy punkt do utworzenia macierzy obrotu:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }

    public SKMatrix OneFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        if (Mode == TouchManipulationMode.None)
        {
            return SKMatrix.MakeIdentity();
        }

        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint delta = newPoint - prevPoint;

        if (Mode == TouchManipulationMode.ScaleDualRotate)  // One-finger rotation
        {
            SKPoint oldVector = prevPoint - pivotPoint;
            SKPoint newVector = newPoint - pivotPoint;

            // Avoid rotation if fingers are too close to center
            if (Magnitude(newVector) > 25 && Magnitude(oldVector) > 25)
            {
                float prevAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                // Calculate rotation matrix
                float angle = newAngle - prevAngle;
                touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                // Effectively rotate the old vector
                float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                oldVector.X = magnitudeRatio * newVector.X;
                oldVector.Y = magnitudeRatio * newVector.Y;

                // Recalculate delta
                delta = newVector - oldVector;
            }
        }

        // Multiply the rotation matrix by a translation matrix
        SKMatrix.PostConcat(ref touchMatrix, SKMatrix.MakeTranslation(delta.X, delta.Y));

        return touchMatrix;
    }
    ...
}
```

W `TwoFingerManipulate` metody punktu obrotu to pozycja palca, który nie jest przenoszona w takim przypadku określonej platformy touch. Obrót jest bardzo podobny do obrotu jeden palca, a następnie wektor o nazwie `oldVector` (na podstawie w poprzednim punkcie) jest uwzględniany obrót. Pozostały ruch jest interpretowana jako skalowanie:

```csharp
class TouchManipulationManager
{
    ...
    public SKMatrix TwoFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint oldVector = prevPoint - pivotPoint;
        SKPoint newVector = newPoint - pivotPoint;

        if (Mode == TouchManipulationMode.ScaleRotate ||
            Mode == TouchManipulationMode.ScaleDualRotate)
        {
            // Find angles from pivot point to touch points
            float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
            float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

            // Calculate rotation matrix
            float angle = newAngle - oldAngle;
            touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

            // Effectively rotate the old vector
            float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
            oldVector.X = magnitudeRatio * newVector.X;
            oldVector.Y = magnitudeRatio * newVector.Y;
        }

        float scaleX = 1;
        float scaleY = 1;

        if (Mode == TouchManipulationMode.AnisotropicScale)
        {
            scaleX = newVector.X / oldVector.X;
            scaleY = newVector.Y / oldVector.Y;

        }
        else if (Mode == TouchManipulationMode.IsotropicScale ||
                 Mode == TouchManipulationMode.ScaleRotate ||
                 Mode == TouchManipulationMode.ScaleDualRotate)
        {
            scaleX = scaleY = Magnitude(newVector) / Magnitude(oldVector);
        }

        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
        {
            SKMatrix.PostConcat(ref touchMatrix,
                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y));
        }

        return touchMatrix;
    }
    ...
}
```

Można zauważyć, że nie bez jawnego translacji w ramach tej metody. Jednakże, oba `MakeRotation` i `MakeScale` metody są oparte na punktu obrotu i zawiera niejawne tłumaczenia. Jeśli używasz dwoma palcami na mapę bitową i przeciągając je w tym samym kierunku `TouchManipulation` otrzyma szereg zdarzeń touch przełączanie pomiędzy dwoma palcami. Jak każdy palca przenosi względem innych, skalowanie i obrót wyników, ale jest on zanegowane przez inne palca przepływu, a wynik jest translacja.

Tylko pozostałą część **Touch manipulowania** strona jest `PaintSurface` obsługi w `TouchManipulationPage` pliku CodeBehind. Powoduje to wywołanie `Paint` metody `TouchManipulationBitmap`, którego dotyczy macierzy reprezentujący skumulowany touch działania:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    MatrixDisplay matrixDisplay = new MatrixDisplay();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        bitmap.Paint(canvas);

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(bitmap.Matrix);

        matrixDisplay.Paint(canvas, bitmap.Matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));
    }
}
```

`PaintSurface` Obsługi stwierdza, wyświetlając `MatrixDisplay` przedstawiający macierzy skumulowany touch obiektu:

[![](touch-images/touchmanipulation-small.png "Potrójna zrzut ekranu przedstawiający stronę Touch manipulowania")](touch-images/touchmanipulation-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę manipulowania Touch")

## <a name="manipulating-multiple-bitmaps"></a>Manipulowanie wiele map bitowych

Jedną z zalet izolowania takich jak touch przetwarzania kodu w klasach `TouchManipulationBitmap` i `TouchManipulationManager` jest możliwość ponownego użycia tych klas w programie, który umożliwia użytkownikowi manipulowania wiele map bitowych.

**Mapy bitowej punktowy widoku** strony pokazano, jak to zrobić. Zamiast definicji pola typu `TouchManipulationBitmap`, [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) klasa definiuje `List` obiektów mapy bitowej:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    List<TouchManipulationBitmap> bitmapCollection =
        new List<TouchManipulationBitmap>();
    ...
    public BitmapScatterViewPage()
    {
        InitializeComponent();

        // Load in all the available bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;
        string[] resourceIDs = assembly.GetManifestResourceNames();
        SKPoint position = new SKPoint();

        foreach (string resourceID in resourceIDs)
        {
            if (resourceID.EndsWith(".png") ||
                resourceID.EndsWith(".jpg"))
            {
                using (Stream stream = assembly.GetManifestResourceStream(resourceID))
                using (SKManagedStream skStream = new SKManagedStream(stream))
                {
                    SKBitmap bitmap = SKBitmap.Decode(skStream);
                    bitmapCollection.Add(new TouchManipulationBitmap(bitmap)
                    {
                        Matrix = SKMatrix.MakeTranslation(position.X, position.Y),
                    });
                    position.X += 100;
                    position.Y += 100;
                }
            }
        }
    }
    ...
}
```

Konstruktor ładuje we wszystkich map bitowych dostępna jako zasoby osadzone, a następnie dodanie ich do `bitmapCollection`. Zwróć uwagę, że `Matrix` właściwość jest inicjowana na każdym `TouchManipulationBitmap` obiekt, w lewym górnym rogu każdej mapy bitowej są Przesunięcie x 100 pikseli.

`BitmapScatterView` Strony musi również obsługę zdarzeń touch dla wielu map bitowych. Zamiast definiowanie `List` dotykowego obecnie manipulować identyfikatory `TouchManipulationBitmap` obiekty, ten program wymaga słownika:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    Dictionary<long, TouchManipulationBitmap> bitmapDictionary =
       new Dictionary<long, TouchManipulationBitmap>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                for (int i = bitmapCollection.Count - 1; i >= 0; i--)
                {
                    TouchManipulationBitmap bitmap = bitmapCollection[i];

                    if (bitmap.HitTest(point))
                    {
                        // Move bitmap to end of collection
                        bitmapCollection.Remove(bitmap);
                        bitmapCollection.Add(bitmap);

                        // Do the touch processing
                        bitmapDictionary.Add(args.Id, bitmap);
                        bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                        canvasView.InvalidateSurface();
                        break;
                    }
                }
                break;

            case TouchActionType.Moved:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    bitmapDictionary.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Powiadomienie jak `Pressed` logiki pętli `bitmapCollection` odwrotnie. Mapy bitowe często nakładają się na siebie. Mapy bitowe później w kolekcji wizualnie znajdować się na górze map bitowych wcześniej w kolekcji. W przypadku wielu map bitowych w obszarze palca, które naciśnie na ekranie znajdujące się najwyżej jeden musi mieć jedną, która steruje się przy tym palca.

Ponadto należy zauważyć, że `Pressed` logiki wizualnie był przenoszony do góry stosu innych map bitowych przenosi tej mapy bitowej na końcu kolekcji.

W `Moved` i `Released` zdarzenia, `TouchAction` wywołań obsługi `ProcessingTouchEvent` metoda `TouchManipulationBitmap` podobnie jak program wcześniej.

Na koniec `PaintSurface` wywołań obsługi `Paint` metody każdego `TouchManipulationBitmap` obiektu:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (TouchManipulationBitmap bitmap in bitmapCollection)
        {
            bitmap.Paint(canvas);
        }
    }
}
```

Kod w pętli kolekcji i wyświetla map bitowych od początku kolekcji na końcu stosu:

[![](touch-images/bitmapscatterview-small.png "Potrójna zrzut ekranu strony mapy bitowej punktowy widoku")](touch-images/bitmapscatterview-large.png#lightbox "Potrójna zrzut ekranu strony widoku punktowy mapy bitowej")

## <a name="single-finger-scaling"></a>Skalowanie jednym palca

Operacja skalowania zwykle wymaga gestem uszczypnięcia przy użyciu dwoma palcami. Istnieje możliwość wdrożenia skalowania palcem pojedynczego dzięki użyciu linii papilarnych, Przenieś narożników mapy bitowej.

To jest przedstawiona w **pojedynczego skali rogu palca** strony. Ponieważ w tym przykładzie użyto nieco inny typ skalowania, że wprowadzonym w `TouchManipulationManager` klasa, nie używa tej klasy lub `TouchManipulationBitmap` klasy. Zamiast tego całą logikę touch znajduje się w pliku CodeBehind. Jest to logiki nieco prostsze niż zwykle, ponieważ śledzi tylko jeden palca w czasie i po prostu ignoruje wszystkie dodatkowej palców, które mogą być dotknięcie ekranu.

[ **SingleFingerCornerScale.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml) tworzy stronę `SKCanvasView` klasy i tworzy `TouchEffect` obiektu dla śledzenia zdarzeń touch:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.SingleFingerCornerScalePage"
             Title="Single Finger Corner Scale">

    <Grid BackgroundColor="White"
          Grid.Row="1">

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction"   />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

[ **SingleFingerCornerScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs) załadowaniu pliku zasobu mapy bitowej z **nośnika** katalogu i wyświetla je przy użyciu `SKMatrix` obiekt zdefiniowany jako pola:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();
    ···

    public SingleFingerCornerScalePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.SetMatrix(currentMatrix);
        canvas.DrawBitmap(bitmap, 0, 0);
    }
    ···
}
```

To `SKMatrix` obiektu jest modyfikowany przez logikę touch, pokazano poniżej.

W pozostałej części pliku CodeBehind jest `TouchEffect` obsługi zdarzeń. Rozpoczyna konwertując bieżącą lokalizację palca do `SKPoint` wartości. Aby uzyskać `Pressed` typ akcji programu obsługi sprawdza, czy nie inne palca zachodzi ekranu, oraz że palca znajduje się w granicach mapy bitowej.

Jest kluczową kwestią kod `if` instrukcji obejmujące dwóch wywołań `Math.Pow` metody. Ta matematyczne umożliwia sprawdzenie, czy lokalizacja palca poza elipsę wypełnia mapy bitowej. Jeśli tak, to operacji skalowania. Zbliża się jeden z narożników mapy bitowej palca, a punktu obrotu jest ustalił, że jest przeciwny prawym górnym rogu. Jeśli w tym elipsy palca, jest regularnie przesuwania:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();

    // Information for translating and scaling
    long? touchId = null;
    SKPoint pressedLocation;
    SKMatrix pressedMatrix;

    // Information for scaling
    bool isScaling;
    SKPoint pivotPoint;
    ···

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Track only one finger
                if (touchId.HasValue)
                    return;

                // Check if the finger is within the boundaries of the bitmap
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = currentMatrix.MapRect(rect);
                if (!rect.Contains(point))
                    return;

                // First assume there will be no scaling
                isScaling = false;

                // If touch is outside interior ellipse, make this a scaling operation
                if (Math.Pow((point.X - rect.MidX) / (rect.Width / 2), 2) +
                    Math.Pow((point.Y - rect.MidY) / (rect.Height / 2), 2) > 1)
                {
                    isScaling = true;
                    float xPivot = point.X < rect.MidX ? rect.Right : rect.Left;
                    float yPivot = point.Y < rect.MidY ? rect.Bottom : rect.Top;
                    pivotPoint = new SKPoint(xPivot, yPivot);
                }

                // Common for either pan or scale
                touchId = args.Id;
                pressedLocation = point;
                pressedMatrix = currentMatrix;
                break;

            case TouchActionType.Moved:
                if (!touchId.HasValue || args.Id != touchId.Value)
                    return;

                SKMatrix matrix = SKMatrix.MakeIdentity();

                // Translating
                if (!isScaling)
                {
                    SKPoint delta = point - pressedLocation;
                    matrix = SKMatrix.MakeTranslation(delta.X, delta.Y);
                }
                // Scaling
                else
                {
                    float scaleX = (point.X - pivotPoint.X) / (pressedLocation.X - pivotPoint.X);
                    float scaleY = (point.Y - pivotPoint.Y) / (pressedLocation.Y - pivotPoint.Y);
                    matrix = SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);
                }

                // Concatenate the matrices
                SKMatrix.PreConcat(ref matrix, pressedMatrix);
                currentMatrix = matrix;
                canvasView.InvalidateSurface();
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = null;
                break;
        }
    }
}
```

`Moved` Macierzy odpowiadającego działaniu touch od czasu palca naciśnięty ekranu poprzedzającym teraz oblicza typ akcji. Tej macierzy z macierzy go łączy obowiązująca w momencie palca naciskanie mapy bitowej. Operacja skalowania jest zawsze względem rogu przeciwnej który dotknięciu palca.

Dla małych lub podłużnych map bitowych wewnętrzne elipsy może zajmować większość mapy bitowej i pozostawić bardzo mała obszarów w narożnikach skalowania mapy bitowej. Można wybrać nieco inny sposób, w tym przypadku można zastąpić ten całą `if` bloku, który ustawia `isScaling` do `true` o tym kodzie:

```csharp
float halfHeight = rect.Height / 2;
float halfWidth = rect.Width / 2;

// Top half of bitmap
if (point.Y < rect.MidY)
{
    float yRelative = (point.Y - rect.Top) / halfHeight;

    // Upper-left corner
    if (point.X < rect.MidX - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Bottom);
    }
    // Upper-right corner
    else if (point.X > rect.MidX + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Bottom);
    }
}
// Bottom half of bitmap
else
{
    float yRelative = (point.Y - rect.MidY) / halfHeight;

    // Lower-left corner
    if (point.X < rect.Left + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Top);
    }
    // Lower-right corner
    else if (point.X > rect.Right - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Top);
    }
}
```

Ten kod skutecznie dzieli obszar mapy bitowej do kształtu romb wewnętrzne i cztery trójkąty w narożnikach. Pozwala to znacznie większych obszarów w narożnikach do pobrania i skalować mapy bitowej.

## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Wywoływanie zdarzeń z efekty](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
