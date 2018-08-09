---
title: Wyświetlanie map bitowych SkiaSharp
description: Dowiedz się, jak wyświetlać SkiaSharp mapy bitowej w pikselach rozmiarów i rozszerzyć, aby wypełnić prostokąty przy zachowaniu współczynnika proporcji.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8E074F8D-4715-4146-8CC0-FD7A8290EDE9
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: cbe3166c4edb147f7179f2c719901b382db8ec80
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615317"
---
# <a name="displaying-skiasharp-bitmaps"></a>Wyświetlanie map bitowych SkiaSharp

Temat map bitowych SkiaSharp została wprowadzona w artykule  **[podstawy mapy bitowej w SkiaSharp](../basics/bitmaps.md)**. Ten artykuł wykazało, że trzy sposoby mapy bitowe obciążenia i wyświetlanie map bitowych na trzy sposoby. W tym artykule przeglądy technik, które można załadować mapy bitowe i lepiej jest przenoszony do użytkowania `DrawBitmap` metody `SKCanvas`.

![Wyświetlanie przykładowych](displaying-images/DisplayingSample.png "wyświetlanie przykładowych")

`DrawBitmapLattice` i `DrawBitmapNinePatch` metody zostały omówione w artykule  **[Segmentowanych wyświetlania mapy bitowe SkiaSharp](segmented.md)**.

Przykłady na tej stronie są z **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikacji. Z tej aplikacji na stronie głównej wybierz **mapy bitowe SkiaSharp**, a następnie przejdź do **wyświetlanie map bitowych** sekcji.

## <a name="loading-a-bitmap"></a>Trwa ładowanie mapy bitowej

Używane przez aplikację SkiaSharp ogólnie mapy bitowej pochodzi z jednego z trzech różnych źródeł:

- Ponad Internet
- Z zasobu osadzonego w pliku wykonywalnym
- Z biblioteki zdjęć użytkownika

Użytkownik może również dla aplikacji skiasharp — można utworzyć nowej mapy bitowej, a następnie narysować w nim lub algorithmically ustawić bity mapy bitowej. Techniki te zostały omówione w artykułach **[tworzenia i rysowania na mapy bitowe SkiaSharp](drawing.md)** i **[uzyskiwania dostępu do pikseli mapy bitowej SkiaSharp](pixel-bits.md)**.

W poniższych przykładach kodu trzy ładowania mapy bitowej klasy zakłada się, że zawiera pole o typie `SKBitmap`:

```csharp
SKBitmap bitmap;
```

Jako artykuł **[podstawy mapy bitowej w SkiaSharp](../basics/bitmaps.md)** stwierdzono, jest najlepszym sposobem, aby załadować mapy bitowej za pośrednictwem Internetu z [ `HttpClient` ](xref:System.Net.Http.HttpClient) klasy. Jedno wystąpienie tej klasy można zdefiniować jako pola:

```csharp
HttpClient httpClient = new HttpClient();
```

Korzystając z `HttpClient` z systemem iOS i aplikacji dla systemu Android, będziesz chciał ustawiania właściwości projektu, zgodnie z opisem w dokumentach na  **[zabezpieczeń TLS (Transport Layer) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)**.

Kod, który używa `HttpClient` często obejmuje `await` operatora, więc musi znajdować się w `async` metody:

```csharp
try
{
    using (Stream stream = await httpClient.GetStreamAsync("https:// ··· "))
    using (MemoryStream memStream = new MemoryStream())
    {
        await stream.CopyToAsync(memStream);
        memStream.Seek(0, SeekOrigin.Begin);

        bitmap = SKBitmap.Decode(stream);
        ···
    };
}
catch
{
    ···
}
```

Należy zauważyć, że `Stream` uzyskanego z `GetStreamAsync` jest kopiowana do `MemoryStream`. System android uniemożliwia `Stream` z `HttpClient` do przetworzenia przez wątek główny, z wyjątkiem w metod asynchronicznych. 

[ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/System.IO.Stream/) Jest dużo pracy: `Stream` przekazany obiekt odwołuje się do bloku pamięci zawierające całą mapę bitową w jednym z typowych formatów pliku mapy bitowej, zazwyczaj JPEG, PNG lub GIF. `Decode` Metody należy określić format, a następnie Zdekoduj SkiaSharp własnego wewnętrznego mapa bitowa plik mapy bitowej.

Po Twój kod wywołuje `SKBitmap.Decode`, prawdopodobnie spowoduje unieważnienie `CanvasView` tak, aby `PaintSurface` obsługi można wyświetlić nowo załadowanych mapy bitowej.

Druga metoda można załadować mapy bitowej przez dołączenie mapy bitowej jako zasobu osadzonego w bibliotece programu .NET Standard odwołują się projekty poszczególnych platform. Identyfikator jest przekazywany do zasobu [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) metody. Tego Identyfikatora zasobu składa się z nazwy zestawu, nazwę folderu i nazwa pliku zasobu, oddzielone kropkami:

```csharp
string resourceID = "assemblyName.folderName.fileName";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    bitmap = SKBitmap.Decode(stream);
    ···
}
```

Pliki map bitowych mogą być również przechowywane jako zasoby w projekcie poszczególnych platform dla systemów iOS, Android i platformy uniwersalnej Windows (UWP). Podczas ładowania tych map bitowych wymaga jednak wykonania kodu, który znajduje się w projekcie platformy.

Jest trzeci sposobem uzyskiwania mapę bitową z biblioteki obrazów przez użytkownika. W poniższym kodzie użyto usługi zależności, który znajduje się w **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikacji. **SkiaSharpFormsDemo** zawiera Biblioteka .NET Standard `IPhotoLibrary` interfejsu, a każdy z projektów platformy zawiera `PhotoLibrary` klasę, która implementuje ten interfejs.

```csharp
IPhotoicturePicker picturePicker = DependencyService.Get<IPhotoLibrary>();

using (Stream stream = await picturePicker.GetImageStreamAsync())
{
    if (stream != null)
    {
        bitmap = SKBitmap.Decode(stream);
        ···
    }
}
```

Ogólnie rzecz biorąc, również unieważnia takiego kodu `CanvasView` tak, aby `PaintSurface` obsługi można wyświetlić nowej mapy bitowej.

`SKBitmap` Klasa definiuje kilka użytecznych właściwości, w tym [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) i [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/), które ujawniają wymiary mapy bitowej, jak również wiele metod, takich jak metody tworzenia mapy bitowe, skopiuj je i udostępnić bitów pikseli. 

## <a name="displaying-in-pixel-dimensions"></a>Wyświetlanie w wymiarach

Skiasharp — [ `Canvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) klasa definiuje cztery `DrawBitmap` metody. Te metody umożliwiają mapy bitowe mają być wyświetlane w dwóch całkowicie różne sposoby: 

- Określanie `SKPoint` wartość (lub osobnych `x` i `y` wartości) Wyświetla mapę bitową w jego rozmiar w pikselach. Piksele mapy bitowej są mapowane bezpośrednio na piksele obrazu wideo.
- Określenie prostokąt powoduje mapy bitowej do być rozciągnięty do rozmiaru i kształtu prostokąta. 

Wyświetlanie mapy bitowej w jego rozmiar w pikselach przy użyciu [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKPoint/SkiaSharp.SKPaint/) z `SKPoint` parametru lub [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) z oddzielnym `x` i `y` parametry:

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

Te dwie metody są funkcjonalnie identyczny. Określony punkt wskazuje lokalizację, w lewym górnym rogu mapy bitowej względem obszaru roboczego. Ponieważ rozdzielczości pikseli urządzeń przenośnych jest tak duży, map bitowych mniejsze zwykle pojawiają się bardzo niewielki rozmiar na tych urządzeniach.

Opcjonalny `SKPaint` parametr umożliwia wyświetlanie mapy bitowej przy użyciu programu blend tryby lub filtrowanie efekty. Będą one pokazano, w dalsze artykuły.

**Wymiary** strony w **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** przykładowy program wyświetla zasób mapy bitowej, który jest 320 pikseli szerokości i wysokości 240 pikseli:

```csharp
public class PixelDimensionsPage : ContentPage
{
    SKBitmap bitmap;

    public PixelDimensionsPage()
    {
        Title = "Pixel Dimensions";

        // Load the bitmap from a resource
        string resourceID = "SkiaSharpFormsDemos.Media.Banana.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        // Create the SKCanvasView and set the PaintSurface handler
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

        float x = (info.Width - bitmap.Width) / 2;
        float y = (info.Height - bitmap.Height) / 2;

        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

`PaintSurface` Obsługi centrów mapy bitowej, obliczając `x` i `y` wartości na podstawie wymiary powierzchni ekranu i wymiary mapy bitowej w pikselach:

[![Wymiary](displaying-images/PixelDimensions.png "rozmiar w pikselach")](displaying-images/PixelDimensions-Large.png#lightbox)

Gdy aplikacja chce wyświetlać mapę bitową w jego lewym górnym rogu, po prostu przejdzie współrzędnych (0, 0). 

## <a name="a-method-for-loading-resource-bitmaps"></a>Metoda do ładowania zasobów map bitowych

Wiele przykładów pojawi się należy załadować mapy bitowej zasobów. Statyczne `BitmapExtensions` klasy w **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** rozwiązanie zawiera metodę, aby pomóc w:

```csharp
static class BitmapExtensions
{
    public static SKBitmap LoadBitmapResource(Type type, string resourceID)
    {
        Assembly assembly = type.GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            return SKBitmap.Decode(stream);
        }
    }
    ···
}
```

Zwróć uwagę `Type` parametru. Może to być `Type` obiekt skojarzony z dowolnego typu w zestawie, który przechowuje zasób mapy bitowej.

To `LoadBitmapResource` metoda będzie używana w wszystkich kolejnych próbek, które wymagają zasoby mapy bitowej.

## <a name="stretching-to-fill-a-rectangle"></a>Rozciąganie, aby wypełnić prostokąt

`SKCanvas` Klasa definiuje również [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) metodę, która powoduje wyświetlenie mapy bitowej do prostokąt, a druga [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) metodę, która renderuje podzbiór prostokątny mapy bitowej do prostokąta.

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

W obu przypadkach mapy bitowej jest rozciągany tak, aby wypełnić prostokąt o nazwie `dest`. W drugiej metodzie `source` prostokąt można wybrać podzestaw mapy bitowej. `dest` Prostokąt stanie się względem urządzenia wyjściowego; `source` prostokąta jest określana względem mapy bitowej.

**Wypełnić prostokąt** strony pokazuje pierwszy te dwie metody, wyświetlając tego samego mapy bitowej pełnią we wcześniejszym przykładzie w prostokącie taki sam rozmiar obszaru roboczego: 

```csharp
public class FillRectanglePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public FillRectanglePage ()
    {
        Title = "Fill Rectangle";

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

        canvas.DrawBitmap(bitmap, info.Rect);
    }
}
```

Zwróć uwagę na użycie nowej `BitmapExtensions.LoadBitmapResource` metodę, aby ustawić `SKBitmap` pola. Prostokąta docelowego są uzyskiwane z [ `Rect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Rect/) właściwość `SKImageInfo`, który opisuje rozmiar powierzchni ekranu:

[![Wypełnij prostokąt](displaying-images/FillRectangle.png "wypełnić prostokąt")](displaying-images/FillRectangle-Large.png#lightbox)

Jest to zazwyczaj _nie_ co chcesz. Obraz jest zniekształcony przez rozciągnięcia inaczej w kierunku poziomym i pionowym. Podczas wyświetlania mapy bitowej w coś innego niż jego rozmiar w pikselach, zazwyczaj chcesz zachować oryginalny współczynnik proporcji mapy bitowej.

## <a name="stretching-while-preserving-the-aspect-ratio"></a>Rozciąganie przy zachowaniu współczynnika proporcji

Rozciąganie mapy bitowej przy zachowaniu współczynnika proporcji jest również nazywany procesem _jednolitego skalowanie_. Czy termin sugeruje konsolidatorze podejście. Jedno z możliwych rozwiązań jest wyświetlany w **jednolitego skalowanie** strony:

```csharp
public class UniformScalingPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(UniformScalingPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public UniformScalingPage()
    {
        Title = "Uniform Scaling";

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

        float scale = Math.Min((float)info.Width / bitmap.Width, 
                               (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect destRect = new SKRect(x, y, x + scale * bitmap.Width, 
                                           y + scale * bitmap.Height);

        canvas.DrawBitmap(bitmap, destRect);
    }
}
```

`PaintSurface` Oblicza obsługi `scale` współczynnika, który jest co najmniej stosunek szerokości i wysokości wyświetlania do mapy bitowej szerokość i wysokość. `x` i `y` można obliczyć wartości do wyrównywania skalowanych mapy bitowej w ramach szerokości i wysokości wyświetlania. Prostokąta docelowego ma lewego górnego rogu `x` i `y` i prawym dolnym rogu te wartości oraz przeskalowano szerokość i wysokość mapy bitowej:

[![Skalowanie jednolitego](displaying-images/UniformScaling.png "jednolitego skalowania")](displaying-images/UniformScaling-Large.png#lightbox)

Aby wyświetlić mapę bitową rozciągnięte do tego obszaru, należy włączyć obrócić telefon:

[![Jednolity pozioma skalowanie](displaying-images/UniformScaling-Landscape.png "jednolitego skalowanie pozioma")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

Zaletą używania to `scale` współczynnik staje się oczywisty, gdy chcesz zaimplementować nieco innego algorytmu. Załóżmy, że chcesz zachować współczynnik proporcji mapy bitowej, ale również wypełnić prostokąta docelowego. Przycinanie część obrazu jest jedynym sposobem jest to możliwe, ale możesz zaimplementować tego algorytmu, po prostu zmieniając `Math.Min` do `Math.Max` w powyższym kodzie. Poniżej przedstawiono wyniki: 

[![Jednolite alternatywna skalowanie](displaying-images/UniformScaling-Alternative.png "jednolitego skalowanie alternatywna")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

Współczynnik proporcji mapy bitowej zostaną zachowane, ale są obcinane obszary do lewej i prawej strony mapy bitowej.

## <a name="a-versatile-bitmap-display-function"></a>Funkcja wyświetlania wszechstronna mapy bitowej

Środowiska programistyczne oparte na XAML (na przykład platformy UWP i Xamarin.Forms) mają możliwość zwiększania lub zmniejszania rozmiaru mapy bitowej przy zachowaniu ich proporcji. Chociaż SkiaSharp nie zawiera tej funkcji, można zaimplementować je samodzielnie. `BitmapExtensions` Klasy uwzględnione w [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) aplikacji przedstawia sposób. Klasa definiuje dwa nowe `DrawBitmap` metod, które wykonują obliczenia współczynnik proporcji. Te nowe metody są metody rozszerzenia `SKCanvas`.

Nowy `DrawBitmap` metody to parametr typu `BitmapStretch`, wyliczenie zdefiniowane w **BitmapExtensions.cs** pliku:

```csharp
public enum BitmapStretch
{
    None,
    Fill,
    Uniform,
    UniformToFill,
    AspectFit = Uniform,
    AspectFill = UniformToFill
}
```

`None`, `Fill`, `Uniform`, I `UniformToFill` elementy członkowskie są takie same, jak na platformie UWP [ `Stretch` ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) wyliczenia. Podobnych Xamarin.Forms [ `Aspect` ](xref:Xamarin.Forms.Aspect) wyliczenie definiuje składowe `Fill`, `AspectFit`, i `AspectFill`.

**Jednolitego skalowanie** strony powyżej centra mapy bitowej w obrębie prostokąta, ale może być inne opcje, takie jak mapy bitowej przy lewej lub prawej krawędzi prostokąta, lub górny lub dolny pozycjonowania. Jest celem `BitmapAlignment` wyliczenia:

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

Ustawienia wyrównania nie mają żadnego skutku, gdy jest używane z `BitmapStretch.Fill`.

Pierwszy `DrawBitmap` funkcji rozszerzenia zawiera prostokąta docelowego, ale nie prostokąta źródłowego. Wartości domyślne są zdefiniowane tak, aby Chcąc mapy bitowej, a ich tematyka, wystarczy określić `BitmapStretch` elementu członkowskiego:

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect dest, 
                                  BitmapStretch stretch, 
                                  BitmapAlignment horizontal = BitmapAlignment.Center, 
                                  BitmapAlignment vertical = BitmapAlignment.Center, 
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * bitmap.Width, scale * bitmap.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, display, paint);
        }
    }
    ···
}
```

Głównym celem tej metody jest można obliczyć współczynnik skalowania o nazwie `scale` który jest następnie stosowane do mapy bitowej szerokość i wysokość podczas wywoływania `CalculateDisplayRect` metody. Jest to metoda, który oblicza prostokąta do wyświetlania mapy bitowej wyrównanie w pionie i w oparciu:

```csharp
static class BitmapExtensions
{
    ···
    static SKRect CalculateDisplayRect(SKRect dest, float bmpWidth, float bmpHeight, 
                                       BitmapAlignment horizontal, BitmapAlignment vertical)
    {
        float x = 0;
        float y = 0;

        switch (horizontal)
        {
            case BitmapAlignment.Center:
                x = (dest.Width - bmpWidth) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                x = dest.Width - bmpWidth;
                break;
        }

        switch (vertical)
        {
            case BitmapAlignment.Center:
                y = (dest.Height - bmpHeight) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                y = dest.Height - bmpHeight;
                break;
        }

        x += dest.Left;
        y += dest.Top;

        return new SKRect(x, y, x + bmpWidth, y + bmpHeight);
    }
}
```

`BitmapExtensions` Klasa zawiera dodatkowy `DrawBitmap` sposobu określenia podzbiór mapę bitową z prostokąta źródłowego. Ta metoda jest podobna do pierwszej, z tą różnicą, że współczynnik skalowania jest obliczana na podstawie `source` prostokąt, a następnie stosowane do `source` prostokąt w wywołaniu `CalculateDisplayRect`:

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect source, SKRect dest,
                                  BitmapStretch stretch,
                                  BitmapAlignment horizontal = BitmapAlignment.Center,
                                  BitmapAlignment vertical = BitmapAlignment.Center,
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, source, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / source.Width, dest.Height / source.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / source.Width, dest.Height / source.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * source.Width, scale * source.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, source, display, paint);
        }
    }
    ···
}
```

Pierwsza z tych dwóch nowych `DrawBitmap` metody została przedstawiona w **skalowanie tryby** strony. Plik XAML zawiera trzy `Picker` elementy, które umożliwiają wybieranie elementów członkowskich z `BitmapStretch` i `BitmapAlignment` wyliczeń:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.ScalingModesPage"
             Title="Scaling Modes">

    <Grid Padding="10">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="Stretch:"
               Grid.Row="1" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="stretchPicker"
                Grid.Row="1" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapStretch}">
                    <x:Static Member="local:BitmapStretch.None" />
                    <x:Static Member="local:BitmapStretch.Fill" />
                    <x:Static Member="local:BitmapStretch.Uniform" />
                    <x:Static Member="local:BitmapStretch.UniformToFill" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Horizontal Alignment:"
               Grid.Row="2" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="horizontalPicker" 
                Grid.Row="2" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Vertical Alignment:"
               Grid.Row="3" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="verticalPicker" 
                Grid.Row="3" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

Plik związany z kodem po prostu unieważnia `CanvasView` gdy którykolwiek `Picker` element został zmieniony. `PaintSurface` Obsługi uzyskuje dostęp do tych trzech `Picker` widoków do wywoływania `DrawBitmap` — metoda rozszerzenia:

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public ScalingModesPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, dest, stretch, horizontal, vertical);
    }
}
```

Poniżej przedstawiono niektóre kombinacje opcje:

[![Skalowanie tryby](displaying-images/ScalingModes.png "skalowanie tryby")](displaying-images/ScalingModes-Large.png#lightbox)

**Podzbioru prostokąt** strona ma niemal tym samym pliku XAML **skalowanie tryby**, ale plik związany z kodem definiuje podzbiór prostokątny mapy bitowej, określone przez `SOURCE` pola: 

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");

    static readonly SKRect SOURCE = new SKRect(94, 12, 212, 118);

    public RectangleSubsetPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, SOURCE, dest, stretch, horizontal, vertical);
    }
}
```

To źródło prostokąt izoluje małp head, jak pokazano w tych zrzuty ekranu:

[![Prostokąt podzbioru](displaying-images/RectangleSubset.png "podzbioru prostokąt")](displaying-images/RectangleSubset-Large.png#lightbox)

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

