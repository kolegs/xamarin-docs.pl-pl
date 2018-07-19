---
title: Tworzenie i rysowania na SkiaSharp map bitowych
description: Dowiedz się, jak utworzyć SkiaSharp mapy bitowe, a następnie narysuj na tych map bitowych w przez utworzenie obszaru roboczego na ich podstawie.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 79BD3266-D457-4E50-BDDF-33450035FA0F
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: fa32b2bdb95044c8171542ff4156ec3c15747372
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39131064"
---
# <a name="creating-and-drawing-on-skiasharp-bitmaps"></a>Tworzenie i rysowania na SkiaSharp map bitowych

Wiesz, jak aplikacja może załadować mapy bitowe, w sieci Web z zasobów aplikacji oraz z biblioteki zdjęć użytkownika. Istnieje również możliwość tworzenia nowej mapy bitowe, w ramach aplikacji. Najprostsza metoda polega na jednym z konstruktorów z [ `SKBitmap` ](https://developer.xamarin.com/api/constructor/SkiaSharp.SKBitmap.SKBitmap/p/System.Int32/System.Int32/System.Boolean/):

```csharp
SKBitmap bitmap = new SKBitmap(width, height);
```

`width` i `height` parametry są liczbami całkowitymi i Określ wymiary mapy bitowej w pikselach. Ten konstruktor tworzy mapę bitową kolorowych cztery bity na piksel: jeden bajt na czerwony, zielony, niebieski i składniki alfa (nieprzezroczystość). 

Po utworzeniu nowej mapy bitowej, musisz pobrać coś na powierzchni mapy bitowej. Ogólnie rzecz biorąc wykonać to w jednym z dwóch sposobów:

- Rysowanie w mapy bitowej przy użyciu standardu `Canvas` rysowania metody.
- Bezpośredni dostęp bitów pikseli.

W tym artykule przedstawiono pierwszą metodę:

![Próbki](drawing-images/DrawingSample.png "próbki")

Drugie podejście zostało omówione w artykule [ **uzyskiwania dostępu do pikseli mapy bitowej SkiaSharp**](pixel-bits.md).

## <a name="drawing-on-the-bitmap"></a>Rysowanie na mapę bitową

Rysowanie na powierzchni mapy bitowej jest taka sama jak rysunek na wyświetlanie wideo. Aby rysować na ekranie wideo, należy uzyskać `SKCanvas` obiektu z `PaintSurface` argumenty zdarzeń. Aby narysować na mapę bitową, należy utworzyć `SKCanvas` przy użyciu [ `SKCanvas` ](https://developer.xamarin.com/api/constructor/SkiaSharp.SKCanvas.SKCanvas/p/SkiaSharp.SKBitmap/) Konstruktor:

```csharp
SKCanvas canvas = new SKCanvas(bitmap);
```

Po zakończeniu rysowania na mapę bitową może likwidowane `SKCanvas` obiektu. Z tego powodu `SKCanvas` ogólnie wywoływany jest konstruktor `using` instrukcji:

```csharp
using (SKCanvas canvas = new SKCanvas(bitmap))
{
    ··· // call drawing function
}
```

Następnie można wyświetlić mapę bitową. W późniejszym czasie, można utworzyć nowy program `SKCanvas` takie same mapy bitowej, a na nim rysować niektórych innych na podstawie obiektu.

**Mapy bitowej Hello** strony w **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikacja zapisuje tekst "Hello, mapy bitowej!" na mapę bitową i następnie wyświetla, które mapy bitowej wiele razy.  

Konstruktor obiektu `HelloBitmapPage` zaczyna się od utworzenia `SKPaint` obiektu do wyświetlania tekstu. Wymiary ciągu tekstowego, a następnie tworzy mapę bitową z tych wymiarów. Następnie tworzy `SKCanvas` obiektu oparte na wywołania tej mapy bitowej `Clear`, a następnie wywołuje `DrawText`. Zawsze jest dobrym pomysłem jest wywołanie `Clear` przy użyciu nowej mapy bitowej ponieważ nowo utworzonej mapy bitowej może zawierać dane losowe.

Konstruktor stwierdza, tworząc `SKCanvasView` obiektu, aby wyświetlić mapę bitową:

```csharp
public partial class HelloBitmapPage : ContentPage
{
    const string TEXT = "Hello, Bitmap!";
    SKBitmap helloBitmap;

    public HelloBitmapPage()
    {
        Title = TEXT;

        // Create bitmap and draw on it
        using (SKPaint textPaint = new SKPaint { TextSize = 48 })
        {
            SKRect bounds = new SKRect();
            textPaint.MeasureText(TEXT, ref bounds);

            helloBitmap = new SKBitmap((int)bounds.Right,
                                       (int)bounds.Height);

            using (SKCanvas bitmapCanvas = new SKCanvas(helloBitmap))
            {
                bitmapCanvas.Clear();
                bitmapCanvas.DrawText(TEXT, 0, -bounds.Top, textPaint);
            }
        }

        // Create SKCanvasView to view result 
        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Aqua);

        for (float y = 0; y < info.Height; y += helloBitmap.Height)
            for (float x = 0; x < info.Width; x += helloBitmap.Width)
            {
                canvas.DrawBitmap(helloBitmap, x, y);
            }
    }
}
```

`PaintSurface` Obsługi powoduje wyświetlenie mapy bitowej wielokrotnie w wiersze i kolumny wyświetlania. Należy zauważyć, że `Clear` method in Class metoda `PaintSurface` program obsługi ma argument `SKColors.Aqua`, który kolory tła powierzchni ekranu:

[![Witaj, mapy bitowej! ] (drawing-images/HelloBitmap.png "Witaj, mapy bitowej!")](drawing-images/HelloBitmap-Large.png#lightbox)

Wygląd Akwamaryna tła wykaże, że mapy bitowej jest przejrzysty, z wyjątkiem tekst.

## <a name="clearing-and-transparency"></a>Czyszczenie i przezroczystości

Wyświetlanie **mapy bitowej Hello** strony pokazuje, że mapy bitowej programu, który został utworzony jest przejrzysty, z wyjątkiem czarny tekst. Dlatego akwamarynową powierzchni ekranu pokazuje za pośrednictwem.

W dokumentacji `Clear` metody `SKCanvas` opisano je przy użyciu instrukcji: "Zastępuje wszystkie piksele w klipie bieżącego obszaru roboczego". Użycie słowa "zastępuje", co spowoduje wyświetlenie ważną cechą z następujących metod: wszelkie rysowania metody `SKCanvas` dodać coś do istniejących wyświetlanej powierzchni. `Clear` Metody _Zastąp_ nowości już istnieje.

`Clear` istnieje już w dwóch różnych wersjach: 

- [ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear/p/SkiaSharp.SKColor/) Metody z `SKColor` parametr zamienia piksele wyświetlanej powierzchni pikseli tego koloru.

- [ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) Metoda bez parametrów zastępuje pikseli z [ `SKColors.Empty` ](https://developer.xamarin.com/api/property/SkiaSharp.SKColors.Empty/) koloru, który jest kolorem, w którym wszystkie składniki (czerwony, zielony, niebieski i alfa) są ustawione na zero. Kolor ten jest czasami określane jako "przezroczysty czarny."

Wywoływanie `Clear` bez argumentów w nowej mapy bitowej aktywuje całą mapę bitową w całkowicie przezroczysty. Nic później rysowane na mapę bitową zwykle jest nieprzezroczysta lub częściowo nieprzezroczystości.

W tym miejscu jest coś, co do wypróbowania: W **mapy bitowej Hello** strony, Zastąp `Clear` metody stosowane do `bitmapCanvas` nowym:

```csharp
bitmapCanvas.Clear(new SKColor(255, 0, 0, 128));
```

Kolejność `SKColor` parametry konstruktora jest czerwony, zielony, niebieski i alfa, gdzie każda wartość może wynosić od 0 do 255. Należy pamiętać, że wartość alfa 0 jest przejrzysty, podczas wartość alfa 255 jest nieprzezroczysta.

Wartość (255, 0, 0, 128) Czyści pikseli mapy bitowej na czerwony pikseli przy użyciu nieprzezroczystości 50%. Oznacza, że tła mapy bitowej półprzezroczyste. Półprzezroczyste czerwony tła mapy bitowej łączy się z Akwamaryna tła powierzchni ekranu do utworzenia na szarym tle.

Spróbuj ustawić kolor tekstu przezroczysty czarny, umieszczając następujące przypisania `SKPaint` inicjatora:

```csharp
Color = new SKColor(0, 0, 0, 0)
```

Może się wydawać, że ten tekst przezroczyste utworzyć w pełni przezroczyste obszary mapy bitowej za pomocą którego można zobaczyć Akwamaryna tła powierzchni ekranu. Ale oznacza to nie tak. Tekst jest rysowana na podstawie co znajduje się już w mapie bitowej. Przezroczysty tekst nie będą widoczne w ogóle.

Nie `Draw` metody nigdy nie tworzy mapę bitową zwiększenie przejrzystości. Tylko `Clear` można to zrobić.

## <a name="bitmap-color-types"></a>Typy kolorów mapy bitowej

Najprostsze `SKBitmap` Konstruktor pozwala na określenie liczby całkowitej szerokość i wysokość pikseli mapy bitowej. Inne `SKBitmap` konstruktory są bardziej złożone. Te konstruktory wymaga argumentów, dwóch typów wyliczenia: [ `SKColorType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColorType/) i [ `SKAlphaType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKAlphaType/). Użyj innych konstruktory [ `SKImageInfo` ](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/) struktury, która konsoliduje te informacje.

`SKColorType` Wyliczenie ma 9 elementów członkowskich. Każda z tych składowych w tym artykule opisano sposób przechowywania pikseli mapy bitowej:

- `Unknown`
- `Alpha8` &mdash; każdego piksela jest 8 bitów, reprezentujący wartość alfa z w pełni przezroczyste całkowicie nieprzezroczyste
- `Rgb565` &mdash; każdego piksela jest 16 bitów 5 bitów dla czerwonej i niebieskiej i 6 dla zielony
- `Argb4444` &mdash; każdego piksela jest 16 bitów i 4 dla alpha, czerwony, zielony i niebieski
- `Rgba8888` &mdash; każdego piksela jest 32-bitowy, 8 na czerwony, zielony, niebieski i alfa
- `Bgra8888` &mdash; każdego piksela jest 32-bitowy, 8 niebieski, zielony, koloru czerwonego i alfa
- `Index8` &mdash; każdego piksela wynosi 8 bitów, a reprezentuje indeks [`SKColorTable`](https://developer.xamarin.com/api/type/SkiaSharp.SKColorTable/)
- `Gray8` &mdash; każdego piksela jest 8 bitów reprezentujący odcień szarości z czarny na biały
- `RgbaF16` &mdash; każdego piksela jest 64-bitowy z czerwony, zielony, niebieski i alfa w formacie zmiennoprzecinkowych 16-bitowych

Dwa formaty, w przypadku każdego piksela na 32 piksele (4 bajty) są często nazywane _kolorowych_ formatów. Wiele innych formatów daty od czasu, gdy wideo wyświetla się nie stanie pełnego koloru. Mapy bitowe ograniczone koloru były właściwe dla tych ekranów i dozwolone mapy bitowe zajmuje mniej miejsca w pamięci. 

Te dni programiści prawie zawsze używać pełnej kolorach i nie odblokowane w innych formatach. Wyjątek stanowi `RgbaF16` formatu, który umożliwia większą rozdzielczość koloru niż nawet formaty kolorowych. Jednak ten format jest używany do celów specjalnych, takich jak obrazowanie medyczne i nie sensu w przypadku użycia za pomocą standardowych Wyświetla pełny kolor.

Ta seria artykułów będzie ograniczać się do `SKBitmap` kolor formatów używanych domyślnie, gdy nie `SKColorType` element członkowski jest określić dojście. Ten format domyślny opiera się na podstawowej platformy. Na platformach obsługiwanych przez zestaw narzędzi Xamarin.Forms jest domyślny typ koloru:

- `Rgba8888` dla systemów iOS i Android
- `Bgra8888` dla platformy uniwersalnej systemu Windows

Jedyna różnica polega na kolejności 4 bajty w pamięci, a to tylko staje się problem podczas uzyskiwania dostępu bezpośrednio bitów pikseli. Nie będzie to nabierają znaczenia, aż dojdziesz do tego artykułu [ **uzyskiwania dostępu do pikseli mapy bitowej SkiaSharp**](pixel-bits.md).

`SKAlphaType` Wyliczenie ma cztery elementy członkowskie:

- `Unknown`
- `Opaque` &mdash; Mapa bitowa ma bez przezroczystości
- `Premul` &mdash; składniki kolorów są wstępnie pomnożona przez składnik alfa
- `Unpremul` &mdash; kolor składniki nie są wstępnie pomnożona przez składnik alfa

Oto piksel 4-bajtowych czerwony mapy bitowej o 50% przezroczystości, za pomocą bajtów wyświetlane w kolejności czerwony, zielony, niebieski, alfa:

0xFF 0x00 0x00 0x80

Po wyrenderowaniu mapy bitowej zawierających półprzezroczyste piksele na powierzchni ekranu składników koloru każdego piksela mapy bitowej muszą pomnożona przez wartość alfa odpowiadającą tej pikseli i należy pomnożyć składników koloru w odpowiedniej piksel wyświetlanej powierzchni przez 255 minus wartość alfa. Następnie można łączyć dwóch pikseli. Mapy bitowej możliwą do wyrenderowania szybciej, jeśli składniki kolorów w pikselach mapy bitowej zostały już wstępnie pomnożoną przez wartość alfa. Ten sam piksela czerwony zostaną zapisane następująco w postaci wstępnie przemnożonego:

0x80 0x00 0x00 0x80

Ta poprawa wydajności jest dlaczego `SkiaSharp` mapy bitowe, domyślnie są tworzone za pomocą `Premul` formatu. Ale ponownie, staje się trzeba wiedzieć, tylko wtedy, gdy dostępu i manipulowania bitów na piksel.

## <a name="drawing-on-existing-bitmaps"></a>Rysowanie w istniejącej mapy bitowej

Nie jest niezbędne do utworzenia nowej mapy bitowej do rysowania na nim. Można też rysować istniejącej mapy bitowej. 

**Moustache małp** strona używa jej konstruktora załadować **MonkeyFace.png** obrazu. Następnie tworzy `SKCanvas` obiektu oparte na tej mapy bitowej i używa `SKPaint` i `SKPath` obiektów, aby narysować moustache na nim:

```csharp
public partial class MonkeyMoustachePage : ContentPage
{
    SKBitmap monkeyBitmap;

    public MonkeyMoustachePage()
    {
        Title = "Monkey Moustache";

        monkeyBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MonkeyFace.png");

        // Create canvas based on bitmap
        using (SKCanvas canvas = new SKCanvas(monkeyBitmap))
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 24;
                paint.StrokeCap = SKStrokeCap.Round;

                using (SKPath path = new SKPath())
                {
                    path.MoveTo(380, 390);
                    path.CubicTo(560, 390, 560, 280, 500, 280);

                    path.MoveTo(320, 390);
                    path.CubicTo(140, 390, 140, 280, 200, 280);

                    canvas.DrawPath(path, paint);
                }
            }
        }

        // Create SKCanvasView to view result 
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
        canvas.DrawBitmap(monkeyBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Konstruktor stwierdza, tworząc `SKCanvasView` którego `PaintSurface` obsługi po prostu wyświetla wynik:

[![Małp Moustache](drawing-images/MonkeyMoustache.png "małp Moustache")](drawing-images/MonkeyMoustache-Large.png#lightbox)

## <a name="copying-and-modifying-bitmaps"></a>Kopiowanie i modyfikowanie map bitowych

Metody `SKCanvas` służące do rysowania na mapę bitową obejmują `DrawBitmap`. Oznacza to, że można narysować jedną mapę bitową na innym, zwykle modyfikowanie w jakiś sposób.

Najbardziej wszechstronny sposobem modyfikowania mapę bitową do uzyskiwania dostępu do usługi bits rzeczywiste pikseli, temat omówione w artykule  **[SkiaSharp uzyskiwania dostępu do mapy bitowej pikseli](pixel-bits.md)**. Ale istnieje wiele innych technik do modyfikowania mapy bitowe, które nie wymagają dostępu do usługi bits pikseli.

Poniższe mapy bitowej dołączone **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikacji to 360 pikseli szerokości i 480 pikseli wysokości:

![Górski Climbers](drawing-images/MountainClimbers.jpg "Climbers górski")

Załóżmy, że nie otrzymali uprawnienia z małp po lewej stronie, aby opublikować to zdjęcie. Rozwiązanie polega na zasłaniać małp twarzy, wykorzystując technikę o nazwie _pixelization_. Piksele na powierzchni są zastępowane bloki koloru, dlatego nie można utworzyć funkcje. Bloki koloru są zazwyczaj uzyskiwane z oryginalnego obrazu poprzez uśrednienie kolorów pikseli odpowiadający tych bloków. Ale nie trzeba wykonywać tej uśrednianie samodzielnie. Zdarza się to automatycznie podczas kopiowania mapy bitowej na mniejszy wymiar pikseli. 

Po lewej stronie małp twarzy zajmuje około 72 pikseli kwadratowy obszar za pomocą lewego górnego rogu w momencie (112, 238). Przyjrzyjmy zastąpić ten obszar kwadratowy 72 pikseli z tablicą 9 na 9 kolorowe bloki, z których każdy jest kwadratowy pikseli 8 przez 8.

**Pixelize obraz** strona ładuje się w tej mapy bitowej i najpierw tworzy niewielki rozmiar piksela 9 kwadratowy mapy bitowej o nazwie `faceBitmap`. Jest to miejsce docelowe kopiowania po prostu małp twarzy. Prostokąta docelowego jest kwadratowy po prostu 9-pikseli, ale prostokąta źródłowego jest kwadratowy 72 pikseli. Każdy blok 8 przez 8 pikseli źródła są konsolidowane w dół, aby tylko jeden piksel przy średniej kolorów.

Następnym krokiem jest do skopiowania oryginalnego mapy bitowej do nowej mapy bitowej o tym samym rozmiarze, o nazwie `pixelizedBitmap`. Niewielki rozmiar `faceBitmap` zostaną skopiowane, za pomocą prostokąta docelowego kwadratowy 72 pikseli, aby każdy piksel `faceBitmap` jest rozwinięta, 8-krotnością rozmiar:

```csharp
public class PixelizedImagePage : ContentPage
{
    SKBitmap pixelizedBitmap;

    public PixelizedImagePage ()
    {
        Title = "Pixelize Image";

        SKBitmap originalBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        // Create tiny bitmap for pixelized face
        SKBitmap faceBitmap = new SKBitmap(9, 9);

        // Copy subset of original bitmap to that
        using (SKCanvas canvas = new SKCanvas(faceBitmap))
        {
            canvas.Clear();
            canvas.DrawBitmap(originalBitmap,
                              new SKRect(112, 238, 184, 310),   // source
                              new SKRect(0, 0, 9, 9));          // destination

        }

        // Create full-sized bitmap for copy
        pixelizedBitmap = new SKBitmap(originalBitmap.Width, originalBitmap.Height);

        using (SKCanvas canvas = new SKCanvas(pixelizedBitmap))
        {
            canvas.Clear();

            // Draw original in full size
            canvas.DrawBitmap(originalBitmap, new SKPoint());

            // Draw tiny bitmap to cover face
            canvas.DrawBitmap(faceBitmap, 
                              new SKRect(112, 238, 184, 310));  // destination
        }

        // Create SKCanvasView to view result
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
        canvas.DrawBitmap(pixelizedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Konstruktor stwierdza, tworząc `SKCanvasView` do wyświetlania wyniku:

[![Obraz pixelize](drawing-images/PixelizeImage.png "Pixelize obrazu")](drawing-images/PixelizeImage-Large.png#lightbox)

<a name="rotating-bitmaps" />

## <a name="rotating-bitmaps"></a>Obracanie map bitowych

Inne typowe zadanie jest obracanie map bitowych. Jest to szczególnie przydatne podczas pobierania map bitowych z biblioteki zdjęć iPhone lub iPad. Chyba że urządzenie odbyła się w szczególności orientacji, gdy została wykonana zdjęcia, obraz jest prawdopodobnie nogami lub w bok.

Włączanie mapy bitowej nogami wymaga tworzenia innej mapy bitowej w taki sam rozmiar jak pierwsza, a następnie ustawić przekształcenie do Obróć o 180 stopni podczas kopiowania z pierwszym do drugiego. We wszystkich przykładach w tej sekcji `bitmap` jest `SKBitmap` obiekt, który należy obrócić:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(180, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

W przypadku rotacji o 90 stopni, należy utworzyć mapę bitową, który jest inny rozmiar niż oryginalna przez zamianę szerokość i wysokość. Na przykład w przypadku oryginalnej mapy bitowej 1200 pikseli szerokości i wysokości 800 pikseli, obrócony bitmapa jest szerokości 800 pikseli i 1200 pikseli szerokości. Ustaw przesunięcie i obrót, tak aby obracać wokół jego lewego górnego rogu i następnie przesunięte do widoku mapy bitowej. (Należy pamiętać, że `Translate` i `RotateDegrees` metody są wywoływane w kolejności przeciwnej sposób ich stosowania.) Poniżej przedstawiono kod związany z rotacją 90 stopni w prawo:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(bitmap.Height, 0);
    canvas.RotateDegrees(90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```                        

A Oto podobną funkcję do rotacji 90 stopni przeciwnie do ruchu wskazówek:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(0, bitmap.Width);
    canvas.RotateDegrees(-90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Te dwie metody są używane w **Układanka zdjęć —** stron, które opisano w artykule [ **przycinanie mapy bitowe SkiaSharp**](cropping.md#tile-division).

Program, który umożliwia użytkownikowi obracanie bitmapę o 90 stopni musi zawierać implementację tylko jedną funkcję do rotacji o 90 stopni. Użytkownik może następnie obracać w dowolnym przyrostu o 90 stopni przy wielokrotnego wykonywania tej funkcji, co.

Program można również obrócić mapy bitowej za względu na wielkość. Proste jedno z podejść jest zmodyfikowanie funkcja, która obraca się o 180 stopni, zastępując 180 uogólnionego `angle` zmiennej:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(angle, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Jednak w przypadku ogólnych tę logikę będzie przyciąć, wyłącz narożników obrócony mapy bitowej. Lepszym rozwiązaniem jest do obliczania rozmiaru obrócony mapy bitowej przy użyciu trygonometryczne, aby uwzględnić te rogi. 

Ta trygonometryczne jest wyświetlany w **Rotator mapy bitowej** strony. Tworzy plik XAML `SKCanvasView` i `Slider` , należą do zakresu od 0 do 360 stopni przy użyciu `Label` przedstawiający bieżąca wartość:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapRotatorPage"
             Title="Bitmap Rotator">
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="slider"
                Maximum="360"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Rotate by {0:F0}&#x00B0;'}"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Plik związany z kodem ładuje zasób mapy bitowej i zapisuje go jako statyczne pole tylko do odczytu o nazwie `originalBitmap`. Mapa bitowa wyświetlane w `PaintSurface` program obsługi jest `rotatedBitmap`, która jest początkowo ustawiona `originalBitmap`:

```csharp
public partial class BitmapRotatorPage : ContentPage
{
    static readonly SKBitmap originalBitmap = 
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.Banana.jpg");

    SKBitmap rotatedBitmap = originalBitmap;

    public BitmapRotatorPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(rotatedBitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double angle = args.NewValue;
        double radians = Math.PI * angle / 180;
        float sine = (float)Math.Abs(Math.Sin(radians));
        float cosine = (float)Math.Abs(Math.Cos(radians));
        int originalWidth = originalBitmap.Width;
        int originalHeight = originalBitmap.Height;
        int rotatedWidth = (int)(cosine * originalWidth + sine * originalHeight);
        int rotatedHeight = (int)(cosine * originalHeight + sine * originalWidth);

        rotatedBitmap = new SKBitmap(rotatedWidth, rotatedHeight);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear(SKColors.LightPink);
            canvas.Translate(rotatedWidth / 2, rotatedHeight / 2);
            canvas.RotateDegrees((float)angle);
            canvas.Translate(-originalWidth / 2, -originalHeight / 2);
            canvas.DrawBitmap(originalBitmap, new SKPoint());
        }

        canvasView.InvalidateSurface();
    }
}
```

`ValueChanged` Program obsługi `Slider` wykonuje operacje, które Utwórz nową `rotatedBitmap` oparte na kąt obrotu. Nową wysokość i szerokość są oparte na absolute values sinusy i cosinusów oryginalnego szerokości i wysokości. Przekształcenia do rysowania oryginalnego mapy bitowej w obrócony mapy bitowej przenieść oryginalnego Centrum mapy bitowej do źródła, a następnie obróć go przez określoną liczbę stopni, a następnie przełożyć tego Centrum do środka obrócony mapy bitowej. ( `Translate` i `RotateDegrees` metody są wywoływane w kolejności przeciwnej niż sposób stosowania.)

Zwróć uwagę na `Clear` metody tła `rotatedBitmap` jasnoróżowy. To jest wyłącznie zilustrowanie rozmiar `rotatedBitmap` na ekranie:

[![Mapa bitowa Rotator](drawing-images/BitmapRotator.png "mapy bitowej Rotator")](drawing-images/BitmapRotator-Large.png#lightbox)

Obrócony mapy bitowej jest wystarczająco duży, aby obejmować cały bitową oryginalnej, ale nie może być większa.

## <a name="flipping-bitmaps"></a>Przerzucanie map bitowych

Nosi nazwę innej operacji wykonywanych na mapy bitowe _przerzucanie_. Model mapy bitowej obraca się w trzech wymiarach wokół osi pionowej lub osi poziomej za pośrednictwem Centrum mapy bitowej. Przerzucanie pionowy tworzy obraz lustrzany.

**Ramię mapy bitowej** strony w **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** demonstates aplikacji tych procesów. Plik XAML zawiera `SKCanvasView` oraz dwa przyciski dla Przerzucanie w pionie i w poziomie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapFlipperPage"
             Title="Bitmap Flipper">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        
        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Flip Vertical"
                Grid.Row="1" Grid.Column="0"
                Margin="0, 10"
                Clicked="OnFlipVerticalClicked" />

        <Button Text="Flip Horizontal"
                Grid.Row="1" Grid.Column="1"
                Margin="0, 10"
                Clicked="OnFlipHorizontalClicked" />
    </Grid>
</ContentPage>
```

Plik związany z kodem implementuje te dwie operacje w `Clicked` programy obsługi przycisków: 

```csharp
public partial class BitmapFlipperPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public BitmapFlipperPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnFlipVerticalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(-1, 1, bitmap.Width / 2, 0);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnFlipHorizontalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(1, -1, 0, bitmap.Height / 2);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }
}
```

Przerzucenie w pionie odbywa się przez skalowania przekształcenia o poziomie współczynnik skalowania &ndash;1. Centrum skalowania jest środka w pionie mapy bitowej. Przerzucenie w poziomie jest skalowanie przekształcenia o pionowy współczynnik skalowania &ndash;1. 

Jak widać w odwróconej litery na koszuli małp Przerzucanie nie jest taka sama jak obrotu. Ale tak jak pokazano na zrzucie ekranu platformy uniwersalnej systemu Windows, po prawej stronie, przerzucanie zarówno w pionie i poziomie jest taka sama jak obrót o 180 stopni:

[![Mapa bitowa Flipper](drawing-images/BitmapFlipper.png "ramię Bitmap")](drawing-images/BitmapFlipper-Large.png#lightbox)

Inne typowe zadanie, które mogą być obsługiwane za pomocą podobne techniki jest przycinanie mapę bitową do podzbioru prostokątny. Jest to opisane w następnym artykule [ **przycinanie mapy bitowe SkiaSharp**](cropping.md).

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
