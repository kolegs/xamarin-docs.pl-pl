---
title: Animowanie SkiaSharp map bitowych
description: Dowiedz się, jak wykonać animacji mapy bitowej przy sekwencyjnie wyświetlanie serii mapy bitowe i renderowanie animowane pliki GIF.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: charlespetzold
ms.author: chape
ms.date: 07/12/2018
ms.openlocfilehash: 45a009757d84aa98acc41f6cd2bf672c8472c5bb
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615580"
---
# <a name="animating-skiasharp-bitmaps"></a>Animowanie SkiaSharp map bitowych

Aplikacje, które zazwyczaj animowanie grafiki SkiaSharp wywoływać `InvalidateSurface` na `SKCanvasView` według stałej stawki, często na 16 milisekund. Unieważnienie powierzchni wyzwala wywołanie `PaintSurface` program obsługi, aby odświeżyć ekran. Ponieważ wizualizacje są rysowane czasu 60 sekund, wydają się płynnie być animowane.

Jednak w przypadku zbyt złożone, aby być renderowany w milisekundach 16 grafiki animacji może stać się niestabilny. Programistę warto zmniejszyć częstotliwość odświeżania do 30-krotnie lub razy 15 sekund, ale czasami nawet nie jest wystarczająca. Czasami grafiki są tak skomplikowane, że one po prostu nie może być renderowana w czasie rzeczywistym.

Rozwiązanie polega na przygotowania animacji wcześniej przez renderowanie poszczególnych ramek animacji na szereg map bitowych. Aby wyświetlić animacji, należy tylko do wyświetlania tych map bitowych sekwencyjnie czasu 60 sekund. 

Oczywiście prawdopodobnie jest wiele map bitowych, ale jest to jak duży budżetu 3D filmów animowanych zostały wprowadzone. Grafika 3D przestała znacznie zbyt złożone do renderowania w czasie rzeczywistym. Dużo czasu na przetwarzanie wymagane do renderowania każdej ramce. Zobacz podczas oglądania filmu to seria mapy bitowej.

Możesz zrobić coś podobnego na elemencie SkiaSharp. W tym artykule przedstawiono dwa rodzaje animacji mapy bitowej. Pierwszy przykład jest animacji zestawu Mandelbrot:

![Animowanie przykładowe](animating-images/AnimatingSample.png "animowanie próbki")

Drugi przykład pokazuje, jak używać SkiaSharp do renderowania animowany plik GIF.

## <a name="bitmap-animation"></a>Animacja mapy bitowej

Zestaw Mandelbrot jest wizualnie fascynujących, ale computionally długiej. (Omówienie zestawu Mandelbrot ścisłe i matematykę, w tym miejscu użyć, zobacz [działu 20 _tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms_ ](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf) od strony 666. Poniższy opis przy założeniu tę wiedzę w tle).

[ **Animacji Mandelbrot** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/MandelAnima/) próbki do symulacji ciągłe powiększenie stały punktu w zestawie Mandelbrot animacji mapy bitowej. Powiększanie następuje oddalając, a następnie cykl się powtarza, nieskończoność, lub do momentu zakończenia programu. 

Program przygotowuje się na tej animacji, tworząc maksymalnie 50 mapy bitowe, przechowywane w magazynie lokalnym w aplikacji. Każda mapa bitowa obejmuje połowę szerokości i wysokości na płaszczyźnie złożone jako poprzedniego mapy bitowej. (W programie tych map bitowych mówi się, że reprezentują całkowitego _poziomy przybliżenia_.) Mapy bitowe są następnie wyświetlane w kolejności. Skalowanie poszczególnych mapy bitowej jest animowany zapewnienie sprawnego postępu z jednej bitmapy do innego.

Finalnego programu opisanych w rozdziale 20, takich jak _tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms_, obliczenie zestawu Mandelbrot w **animacji Mandelbrot** jest metoda asynchroniczna z ośmiu Parametry. Parametry zawierają punktu centralnego złożone i szerokość i wysokość na płaszczyźnie złożone otaczającego tego punktu centralnego. Następne trzy parametry są szerokość pikseli i wysokość mapy bitowej, który ma zostać utworzony i maksymalną liczbę iteracji do obliczenia cykliczne. `progress` Parametr jest używany do wyświetlania postępu to obliczenie. `cancelToken` Parametr nie jest używany w tym programie:

```csharp
static class Mandelbrot
{
    public static Task<BitmapInfo> CalculateAsync(Complex center,
                                                  double width, double height,
                                                  int pixelWidth, int pixelHeight,
                                                  int iterations,
                                                  IProgress<double> progress,
                                                  CancellationToken cancelToken)
    {
        return Task.Run(() =>
        {
            int[] iterationCounts = new int[pixelWidth * pixelHeight];
            int index = 0;

            for (int row = 0; row < pixelHeight; row++)
            {
                progress.Report((double)row / pixelHeight);
                cancelToken.ThrowIfCancellationRequested();

                double y = center.Imaginary + height / 2 - row * height / pixelHeight;

                for (int col = 0; col < pixelWidth; col++)
                {
                    double x = center.Real - width / 2 + col * width / pixelWidth;
                    Complex c = new Complex(x, y);

                    if ((c - new Complex(-1, 0)).Magnitude < 1.0 / 4)
                    {
                        iterationCounts[index++] = -1;
                    }
                    // http://www.reenigne.org/blog/algorithm-for-mandelbrot-cardioid/
                    else if (c.Magnitude * c.Magnitude * (8 * c.Magnitude * c.Magnitude - 3) < 3.0 / 32 - c.Real)
                    {
                        iterationCounts[index++] = -1;
                    }
                    else
                    {
                        Complex z = 0;
                        int iteration = 0;

                        do
                        {
                            z = z * z + c;
                            iteration++;
                        }
                        while (iteration < iterations && z.Magnitude < 2);

                        if (iteration == iterations)
                        {
                            iterationCounts[index++] = -1;
                        }
                        else
                        {
                            iterationCounts[index++] = iteration;
                        }
                    }
                }
            }
            return new BitmapInfo(pixelWidth, pixelHeight, iterationCounts);
        }, cancelToken);
    }
}
```

Metoda ta zwraca obiekt typu `BitmapInfo` zawierające informacje dotyczące tworzenia mapy bitowej:

```csharp
class BitmapInfo
{
    public BitmapInfo(int pixelWidth, int pixelHeight, int[] iterationCounts)
    {
        PixelWidth = pixelWidth;
        PixelHeight = pixelHeight;
        IterationCounts = iterationCounts;
    }

    public int PixelWidth { private set; get; }

    public int PixelHeight { private set; get; }

    public int[] IterationCounts { private set; get; }
}
```

**Animacji Mandelbrot** plik XAML zawiera dwie `Label` widoków, `ProgressBar`, a `Button` , jak również `SKCanvasView`:

```csharp
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="MandelAnima.MainPage"
             Title="Mandelbrot Animation">

    <StackLayout>
        <Label x:Name="statusLabel"
               HorizontalTextAlignment="Center" />
        <ProgressBar x:Name="progressBar" />

        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     Padding="5">
            <Label x:Name="storageLabel"
                   VerticalOptions="Center" />

            <Button x:Name="deleteButton"
                    Text="Delete All"
                    HorizontalOptions="EndAndExpand" 
                    Clicked="OnDeleteButtonClicked" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Plik związany z kodem rozpoczyna się od definicji trzy kluczowe stałe i tablicę map bitowych:

```csharp
public partial class MainPage : ContentPage
{
    const int COUNT = 10;           // The number of bitmaps in the animation.
                                    // This can go up to 50!

    const int BITMAP_SIZE = 1000;   // Program uses square bitmaps exclusively

    // Uncomment just one of these, or define your own
    static readonly Complex center = new Complex(-1.17651152924355, 0.298520986549558);
    //   static readonly Complex center = new Complex(-0.774693089457127, 0.124226621261617);
    //   static readonly Complex center = new Complex(-0.556624880053304, 0.634696788141351);

    SKBitmap[] bitmaps = new SKBitmap[COUNT];   // array of bitmaps
    ···
}
```

W pewnym momencie będzie prawdopodobnie chcesz zmienić `COUNT` wartość 50, aby wyświetlić pełną gamę animacji. Wartości powyżej 50 są przydatne. Wokół poziom powiększenia 48, lub tak rozpoznawanie liczby zmiennoprzecinkowe podwójnej precyzji staje się niewystarczające w obliczeniach Mandelbrot zestawu. Ten problem, opisanej na stronie 684 _tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms_.

`center` Wartość jest bardzo ważne. Jest to fokus powiększenia animacji. Trzy wartości w pliku są używane trzy końcowego zrzuty ekranu w rozdziale 20 _tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms_ na stronie 684, ale możesz eksperymentować z programu, w tym rozdziale, aby skorzystać z jednej z własnymi wartościami. 

**Animacji Mandelbrot** przykładowe przechowuje te `COUNT` mapy bitowe, w magazynie lokalnym aplikacji. 50 mapy bitowe wymagają ponad 20 MB miejsca na urządzeniu z systemem, więc warto wiedzieć, ile pamięci masowej są zajmuje tych map bitowych, i w pewnym momencie możesz chcieć usunąć wszystkie. To jest cel te dwie metody w dolnej części `MainPage` klasy:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void TallyBitmapSizes()
    {
        long fileSize = 0;

        foreach (string filename in Directory.EnumerateFiles(FolderPath()))
        {
            fileSize += new FileInfo(filename).Length;
        }

        storageLabel.Text = $"Total storage: {fileSize:N0} bytes";
    }

    void OnDeleteButtonClicked(object sender, EventArgs args)
    {
        foreach (string filepath in Directory.EnumerateFiles(FolderPath()))
        {
            File.Delete(filepath);
        }

        TallyBitmapSizes();
    }
}
```

Gdy program jest animowanie tych tych samych mapy bitowe, ponieważ program przechowuje je w pamięci, można usunąć mapy bitowe, w magazynie lokalnym. Ale przy następnym uruchomieniu programu, konieczne będzie ponowne utworzenie mapy bitowej.

Zawierają mapy bitowe, przechowywane w magazynie lokalnym aplikacji `center` wartość dla ich nazw plików, więc jeśli zmienisz `center` ustawienie istniejącej mapy bitowe nie zostaną zastąpione w magazynie i będzie w dalszym ciągu zajmują miejsce.

Poniżej przedstawiono metody, `MainPage` używa do konstruowania nazwy plików, a także `MakePixel` metoda definiowanie wartości pikseli oparta na składnikach:

```csharp
public partial class MainPage : ContentPage
{
    ···
    // File path for storing each bitmap in local storage
    string FolderPath() => 
        Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);

    string FilePath(int zoomLevel) => 
        Path.Combine(FolderPath(),
                     String.Format("R{0}I{1}Z{2:D2}.png", center.Real, center.Imaginary, zoomLevel));

    // Form bitmap pixel for Rgba8888 format
    uint MakePixel(byte alpha, byte red, byte green, byte blue) =>
        (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

`zoomLevel` Parametr `FilePath` z zakresu od 0 do `COUNT` stałej minus 1.

`MainPage` Konstruktor wywołuje `LoadAndStartAnimation` metody:

```csharp
public partial class MainPage : ContentPage
{
    ···
    public MainPage()
    {
        InitializeComponent();

        LoadAndStartAnimation();
    }
    ···
}
```

`LoadAndStartAnimation` Metoda jest odpowiedzialna za uzyskiwanie dostępu do magazynu lokalnego aplikacji można załadować żadnych mapy bitowe, które może być utworzone, gdy program został wcześniej uruchomiony. W pętli `zoomLevel` wartości z zakresu od 0 do `COUNT`. Jeśli plik istnieje, ładuje je do `bitmaps` tablicy. W przeciwnym razie wymagane do utworzenia mapy bitowej dla konkretnej `center` i `zoomLevel` wartości przez wywołanie metody `Mandelbrot.CalculateAsync`. Ta metoda uzyskuje liczby iteracji dla każdego piksela, którego ta metoda konwertuje kolorów:

```csharp
public partial class MainPage : ContentPage
{
    ···
    async void LoadAndStartAnimation()
    {
        // Show total bitmap storage
        TallyBitmapSizes();

        // Create progressReporter for async operation
        Progress<double> progressReporter =
            new Progress<double>((double progress) => progressBar.Progress = progress);

        // Create (unused) CancellationTokenSource for async operation
        CancellationTokenSource cancelTokenSource = new CancellationTokenSource();

        // Loop through all the zoom levels
        for (int zoomLevel = 0; zoomLevel < COUNT; zoomLevel++)
        {
            // If the file exists, load it
            if (File.Exists(FilePath(zoomLevel)))
            {
                statusLabel.Text = $"Loading bitmap for zoom level {zoomLevel}";

                using (Stream stream = File.OpenRead(FilePath(zoomLevel)))
                {
                    bitmaps[zoomLevel] = SKBitmap.Decode(stream);
                }
            }
            // Otherwise, create a new bitmap
            else
            {
                statusLabel.Text = $"Creating bitmap for zoom level {zoomLevel}";

                CancellationToken cancelToken = cancelTokenSource.Token;

                // Do the (generally lengthy) Mandelbrot calculation 
                BitmapInfo bitmapInfo =
                    await Mandelbrot.CalculateAsync(center,
                                                    4 / Math.Pow(2, zoomLevel),
                                                    4 / Math.Pow(2, zoomLevel),
                                                    BITMAP_SIZE, BITMAP_SIZE,
                                                    (int)Math.Pow(2, 10), progressReporter, cancelToken);

                // Create bitmap & get pointer to the pixel bits
                SKBitmap bitmap = new SKBitmap(BITMAP_SIZE, BITMAP_SIZE, SKColorType.Rgba8888, SKAlphaType.Opaque);
                IntPtr basePtr = bitmap.GetPixels();

                // Set pixel bits to color based on iteration count
                for (int row = 0; row < bitmap.Width; row++)
                    for (int col = 0; col < bitmap.Height; col++)
                    {
                        int iterationCount = bitmapInfo.IterationCounts[row * bitmap.Width + col];
                        uint pixel = 0xFF000000;            // black

                        if (iterationCount != -1)
                        {
                            double proportion = (iterationCount / 32.0) % 1;
                            byte red = 0, green = 0, blue = 0;

                            if (proportion < 0.5)
                            {
                                red = (byte)(255 * (1 - 2 * proportion));
                                blue = (byte)(255 * 2 * proportion);
                            }
                            else
                            {
                                proportion = 2 * (proportion - 0.5);
                                green = (byte)(255 * proportion);
                                blue = (byte)(255 * (1 - proportion));
                            }

                            pixel = MakePixel(0xFF, red, green, blue);
                        }

                        // Calculate pointer to pixel
                        IntPtr pixelPtr = basePtr + 4 * (row * bitmap.Width + col);

                        unsafe     // requires compiling with unsafe flag
                        {
                            *(uint*)pixelPtr.ToPointer() = pixel;
                        }
                    }

                // Save as PNG file
                SKData data = SKImage.FromBitmap(bitmap).Encode();

                try
                {
                    File.WriteAllBytes(FilePath(zoomLevel), data.ToArray());
                }
                catch
                {
                    // Probably out of space, but just ignore
                }

                // Store in array
                bitmaps[zoomLevel] = bitmap;

                // Show new bitmap sizes
                TallyBitmapSizes();
            }

            // Display the bitmap
            bitmapIndex = zoomLevel;
            canvasView.InvalidateSurface();
        }

        // Now start the animation
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }
    ···
}
```

Zwróć uwagę, program przechowuje te mapy bitowe, w magazynie lokalnym aplikacji, a nie w bibliotece zdjęć urządzenia. Biblioteki .NET Standard 2.0 umożliwia przy użyciu znanej `File.OpenRead` i `File.WriteAllBytes` metod dla tego zadania.

Po wszystkich map bitowych zostały utworzone lub ładowany do pamięci, metoda uruchamia `Stopwatch` obiektów i wywołuje `Device.StartTimer`. `OnTimerTick` Metoda jest wywoływana co 16 milisekund.

`OnTimerTick` oblicza `time` wartość w milisekundach, z zakresu od 0 do 6000 godzin `COUNT`, który apportions sześć sekund do wyświetlania każdej mapy bitowej. `progress` Wartość używa `Math.Sin` wartości do utworzenia sinusoidalnego animacji, który będzie przebiegać wolniej na początku cyklu i wolniejszych na końcu, ponieważ odwraca kierunek. 

`progress` Wartość z zakresu od 0 do `COUNT`. Oznacza to, że część całkowitą `progress` jest indeks `bitmaps` tablicy podczas część ułamkowa `progress` wskazuje poziom powiększenia tej określonej mapy bitowej. Te wartości są przechowywane w `bitmapIndex` i `bitmapProgress` pól i wyświetlane przez `Label` i `Slider` w pliku XAML. `SKCanvasView` Zostaje unieważniony, aby zaktualizować wyświetlane mapy bitowej:

```csharp
public partial class MainPage : ContentPage
{
    ···
    Stopwatch stopwatch = new Stopwatch();      // for the animation
    int bitmapIndex;
    double bitmapProgress = 0;
    ···
    bool OnTimerTick()
    {
        int cycle = 6000 * COUNT;       // total cycle length in milliseconds

        // Time in milliseconds from 0 to cycle
        int time = (int)(stopwatch.ElapsedMilliseconds % cycle);

        // Make it sinusoidal, including bitmap index and gradation between bitmaps
        double progress = COUNT * 0.5 * (1 + Math.Sin(2 * Math.PI * time / cycle - Math.PI / 2));

        // These are the field values that the PaintSurface handler uses
        bitmapIndex = (int)progress;
        bitmapProgress = progress - bitmapIndex;

        // It doesn't often happen that we get up to COUNT, but an exception would be raised
        if (bitmapIndex < COUNT)
        {
            // Show progress in UI
            statusLabel.Text = $"Displaying bitmap for zoom level {bitmapIndex}";
            progressBar.Progress = bitmapProgress;

            // Update the canvas
            canvasView.InvalidateSurface();
        }

        return true;
    }
    ···
}
```

Na koniec `PaintSurface` program obsługi `SKCanvasView` oblicza prostokąta docelowego, aby wyświetlić mapę bitową tak duże, jak to możliwe przy zachowaniu współczynnika proporcji. Prostokąta źródłowego opiera się na `bitmapProgress` wartość. `fraction` Wartość obliczona tutaj z zakresu od 0, gdy `bitmapProgress` jest 0, aby wyświetlić całą mapę bitową, gdy osiągnie 0,25 `bitmapProgress` 1 do wyświetlania pół szerokość i wysokość mapy bitowej, efektywnie powiększyć:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        if (bitmaps[bitmapIndex] != null)
        {
            // Determine destination rect as square in canvas
            int dimension = Math.Min(info.Width, info.Height);
            float x = (info.Width - dimension) / 2;
            float y = (info.Height - dimension) / 2;
            SKRect destRect = new SKRect(x, y, x + dimension, y + dimension);

            // Calculate source rectangle based on fraction:
            //  bitmapProgress == 0: full bitmap
            //  bitmapProgress == 1: half of length and width of bitmap
            float fraction = 0.5f * (1 - (float)Math.Pow(2, -bitmapProgress));
            SKBitmap bitmap = bitmaps[bitmapIndex];
            int width = bitmap.Width;
            int height = bitmap.Height;
            SKRect sourceRect = new SKRect(fraction * width, fraction * height, 
                                           (1 - fraction) * width, (1 - fraction) * height);

            // Display the bitmap
            canvas.DrawBitmap(bitmap, sourceRect, destRect);
        }
    }
    ···
}
```

W tym miejscu jest uruchomiony na wszystkich trzech platformach program:

[![Animacja Mandelbrot](animating-images/MandelbrotAnimation.png "Mandelbrot animacji")](animating-images/MandelbrotAnimation-Large.png#lightbox)

## <a name="gif-animation"></a>Animacja GIF

Specyfikacja Graphics (Interchange Format GIF) zawiera funkcję, która umożliwia jeden plik GIF zawiera wiele kolejnych ramek scen, które mogą być wyświetlane w odstępie czasu, często w pętli. Pliki te są znane jako _animowane pliki GIF_. Przeglądarki sieci Web może odtwarzać animowane pliki GIF i SkiaSharp umożliwia aplikacji można wyodrębnić ramek z animowany plik GIF i wyświetlić je sekwencyjnie.

[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) przykład obejmuje zasób animowany plik GIF o nazwie **Newtons_cradle_animation_book_2.gif** utworzony przez DemonDeLuxe i pobierane z [firmy Newton — podstawki ](https://en.wikipedia.org/wiki/Newton%27s_cradle) strony w witrynie Wikipedia. **Animowany plik GIF** strona zawiera plik XAML, który zawiera te informacje i tworzy `SKCanvasView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.AnimatedGifPage"
             Title="Animated GIF">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="GIF file by DemonDeLuxe from Wikipedia Newton's Cradle page"
               Grid.Row="1"
               Margin="0, 5"
               HorizontalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Plik związany z kodem nie jest uogólniona do odtwarzania na dowolnym animowany plik GIF. Ignoruje niektóre informacje, które są dostępne, w szczególności licznik powtórzeń i po prostu odtwarza animowany plik GIF w pętli. 

Użycie SkisSharp do wyodrębnienia ramek animowany plik GIF nie wydaje się być udokumentowane w dowolnym miejscu, więc opis kodu, która jest bardziej szczegółowe niż zwykle:

Dekodowanie animowany plik GIF występuje w Konstruktorze strony i wymaga `Stream` obiekt odwołuje się do mapy bitowej służącej do tworzenia `SKManagedStream` obiektu a następnie [ `SKCodec` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodec/) obiektu. [ `FrameCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.FrameCount/) Właściwość wskazuje liczbę ramek, które tworzą animacji. 

Te klatki, po pewnym czasie są zapisywane jako poszczególnych mapy bitowe, więc używa konstruktora `FrameCount` do przydzielania tablicy typu `SKBitmap` także dwa `int` tablic na czas trwania każdej ramce oraz (do jej obsługi ułatwiają realizację logiki animacji) skumulowany czas trwania.

[ `FrameInfo` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.FrameInfo/) Właściwość `SKCodec` klasy jest tablicą [ `SKCodecFrameInfo` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodecFrameInfo/) wartości, po jednym dla każdej ramki, ale jedynym elementem tego programu, pobiera z tej struktury jest [ `Duration` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodecFrameInfo.Duration/) ramki w milisekundach.

`SKCodec` Definiuje właściwość o nazwie [ `Info` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.Info/) typu [ `SKImageInfo` ](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/), ale ta `SKImageInfo` wartość wskazuje (co najmniej dla tego obrazu) to typ koloru `SKColorType.Index8`, co oznacza, że każdego piksela jest indeks typ koloru. Aby uniknąć bothering z tabele kolorów, program używa [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Width/) i [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Height/) informacji z tej struktury, do jego konstruowania jest właścicielem kolorowych `ImageInfo` wartość. Każdy `SKBitmap` jest tworzony przy jego użyciu.

`GetPixels` Metody `SKBitmap` zwraca `IntPtr` odwołania do bitów pikseli tej mapy bitowej. Tych bitów na piksel nie został ustawiony. Czy `IntPtr` jest przekazywane do jednego z [ `GetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCodec.GetPixels/p/SkiaSharp.SKImageInfo/System.IntPtr/SkiaSharp.SKCodecOptions/) metody `SKCodec`. Tej metody kopiowania ramkę z pliku GIF do obszaru pamięci odwołuje się `IntPtr`. [ `SKCodecOptions` ](https://developer.xamarin.com/api/constructor/SkiaSharp.SKCodecOptions.SKCodecOptions/p/System.Int32/System.Boolean/) Konstruktor określa liczbę ramek:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;
    ···

    public AnimatedGifPage ()
    {
        InitializeComponent ();

        string resourceID = "SkiaSharpFormsDemos.Media.Newtons_cradle_animation_book_2.gif";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        using (SKCodec codec = SKCodec.Create(skStream))
        {
            // Get frame count and allocate bitmaps
            int frameCount = codec.FrameCount;
            bitmaps = new SKBitmap[frameCount];
            durations = new int[frameCount];
            accumulatedDurations = new int[frameCount];

            // Note: There's also a RepetitionCount property of SKCodec not used here

            // Loop through the frames
            for (int frame = 0; frame < frameCount; frame++)
            {
                // From the FrameInfo collection, get the duration of each frame
                durations[frame] = codec.FrameInfo[frame].Duration;

                // Create a full-color bitmap for each frame
                SKImageInfo imageInfo = code.new SKImageInfo(codec.Info.Width, codec.Info.Height);
                bitmaps[frame] = new SKBitmap(imageInfo);

                // Get the address of the pixels in that bitmap
                IntPtr pointer = bitmaps[frame].GetPixels();

                // Create an SKCodecOptions value to specify the frame
                SKCodecOptions codecOptions = new SKCodecOptions(frame, false);

                // Copy pixels from the frame into the bitmap
                codec.GetPixels(imageInfo, pointer, codecOptions);
            }

            // Sum up the total duration
            for (int frame = 0; frame < durations.Length; frame++)
            {
                totalDuration += durations[frame];
            }

            // Calculate the accumulated durations 
            for (int frame = 0; frame < durations.Length; frame++)
            {
                accumulatedDurations[frame] = durations[frame] + 
                    (frame == 0 ? 0 : accumulatedDurations[frame - 1]);
            }
        }
    }
    ···
}
```

Pomimo `IntPtr` wartości nie `unsafe` kod jest wymagany, ponieważ `IntPtr` nigdy nie jest konwertowany na wartość wskaźnika języka C#.

Po wyodrębnieniu każdej klatce, Konstruktor sumuje się czasów trwania wszystkie ramki, a następnie inicjuje innej tablicy przy użyciu zebranych czasów trwania.

W pozostałej części pliku związanego z kodem jest dedykowany animacji. `Device.StartTimer` Metody są używane do uruchamiania czasomierza pracę i `OnTimerTick` używa wywołań zwrotnych `Stopwatch` obiektu, aby określić czas w milisekundach. Zapętlenie przez tablicę skumulowany czas trwania jest wystarczające, aby znaleźć bieżącej ramki:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;

    Stopwatch stopwatch = new Stopwatch();
    bool isAnimating;

    int currentFrame;
    ···
    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        int msec = (int)(stopwatch.ElapsedMilliseconds % totalDuration);
        int frame = 0;

        // Find the frame based on the elapsed time
        for (frame = 0; frame < accumulatedDurations.Length; frame++)
        {
            if (msec < accumulatedDurations[frame])
            {
                break;
            }
        }

        // Save in a field and invalidate the SKCanvasView.
        if (currentFrame != frame)
        {
            currentFrame = frame;
            canvasView.InvalidateSurface();
        }

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);
            
        // Get the bitmap and center it
        SKBitmap bitmap = bitmaps[currentFrame];
        canvas.DrawBitmap(bitmap,info.Rect, BitmapStretch.Uniform);
    }
}
```

Każdorazowo `currentframe` zmiany zmiennych `SKCanvasView` zostaje unieważniony i pojawi się nowej ramki:

[![Animowany obraz GIF](animating-images/AnimatedGif.png "animowany plik GIF")](animating-images/AnimatedGif-Large.png#lightbox)

Oczywiście należy uruchomić program sobie, aby zobaczyć animację.

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Mandelbrot animacji (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/MandelAnima/)
