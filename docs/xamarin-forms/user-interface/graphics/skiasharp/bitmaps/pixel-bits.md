---
title: Uzyskiwanie dostępu do usługi bits pikseli SkiaSharp
description: Poznaj różne techniki do uzyskiwania dostępu i modyfikowania bitów pikseli SkiaSharp mapy bitowej.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DBB58522-F816-4A8C-96A5-E0236F16A5C6
author: charlespetzold
ms.author: chape
ms.date: 07/11/2018
ms.openlocfilehash: 51252a1c18602c704b87f7b01efe7cc5a7549ac1
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39131059"
---
# <a name="accessing-skiasharp-pixel-bits"></a>Uzyskiwanie dostępu do usługi bits pikseli SkiaSharp

Jak przedstawiono w artykule [ **bitmap SkiaSharp zapisywanie w plikach**](saving.md), mapy bitowe są zazwyczaj przechowywane w plikach w formacie skompresowany, takich jak JPEG lub PNG. Skiasharp — mapa bitowa, przechowywane w pamięci w constrast, nie będzie skompresowany. Jest on przechowywany jako sekwencyjną serię pikseli. Ten format nieskompresowanych ułatwia transfer map bitowych na powierzchni ekranu.

Blok pamięci zajmowane przez mapę bitową SkiaSharp jest zorganizowana w bardzo prosty sposób: rozpoczyna się od pierwszego wiersza pikseli od lewej do prawej, a następnie kontynuuje tworzenie drugiego wiersza. Bitmap kolorowych każdego piksela składa się z czterech bajtów, co oznacza, że przestrzeni całkowitej ilości pamięci wymaganej przez mapę bitową jest cztery razy wynikiem jego szerokość i wysokość.

W tym artykule opisano, jak aplikacja można uzyskać dostęp do tych pikselach bezpośrednio uzyskując dostęp do bloku pamięci pikseli mapy bitowej, ani pośrednio. W niektórych przypadkach program może być do analizowania piksele obrazu i utworzenia histogramu pewnego rodzaju. Częściej aplikacje można skonstruować obraz systemu, tworząc algorithmically piksele, które tworzą mapy bitowej:

![Piksel Bits przykłady](pixel-bits-images/PixelBitsSample.png "pikseli Bits próbki")

## <a name="the-techniques"></a>Techniki

Skiasharp — oferuje kilka technik do uzyskania dostępu do mapy bitowej pikseli bitów. Który z nich wybierzesz zwykle stanowi kompromis między kodowania, wydajności i wygody, (który jest powiązany z konserwacji i łatwość debugowania). W większości przypadków, użyjemy jednego z następujących metod i właściwości działania `SKBitmap` do uzyskiwania dostępu do mapy bitowej w pikselach:

- `GetPixel` i `SetPixel` metody pozwalają uzyskać lub ustawić kolor piksela.
- `Pixels` Właściwości pobiera tablicę kolorów pikseli dla całego mapy bitowej lub ustawia tablicę kolorów.
- `GetPixels` Zwraca adres pamięci pikseli, używany przez mapy bitowej.
- `SetPixels` zastępuje adres pamięci pikseli, używany przez mapy bitowej.

Można potraktować pierwsze dwie techniki jako "wysokiego poziomu", a drugi dwa jako "poziom niski." Istnieją inne metody i właściwości, których można używać, ale są to najbardziej przydatna.

Aby możliwe było zobaczyć różnice w wydajności między te techniki [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) aplikacja zawiera stronę o nazwie **gradientu mapy bitowej** , Tworzy mapę bitową z pikseli, łączące czerwonej i niebieskiej odcieni do Tworzenie gradientu. Program tworzy ośmiu różnych kopii tej mapy bitowej, wszystkie przy użyciu różnych technik do ustawiania pikseli mapy bitowej. Każda z tych osiem mapy bitowe jest tworzony w oddzielnych metodach również określający krótki opis tekstowy techniki i oblicza czas wymagany do zestawu wszystkich pikseli. Każda metoda pętli logiki ustawienie w pikselach 100 razy można pobrać lepiej oszacować wydajność. 

### <a name="the-setpixel-method"></a>Metoda Element SetPixel

Jeśli potrzebujesz ustawić lub pobrać kilka poszczególnych pikseli, [ `SetPixel` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.SetPixel/p/System.Int32/System.Int32/SkiaSharp.SKColor/) i [ `GetPixel` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.GetPixel/p/System.Int32/System.Int32/) metody są idealnym rozwiązaniem. Dla każdego z tych dwóch metod należy określić wierszy i kolumn liczby całkowitej. Bez względu na format pikseli, te dwie metody pozwalają uzyskać lub ustawić pikseli jako `SKColor` wartość:

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

`col` Argumentu musi należeć do zakresu od 0 do jednego mniejsza niż `Width` właściwości mapy bitowej, i `row` z zakresu od 0 do jednego mniejsza niż `Height` właściwości.

Poniżej przedstawiono metody **gradientu mapy bitowej** określająca zawartość mapy bitowej przy użyciu `SetPixel` metody. Mapa bitowa jest 256 x 256 pikseli i `for` pętle są zakodowane przy użyciu zakres wartości:

```csharp
public class GradientBitmapPage : ContentPage
{
    const int REPS = 100;
        
    Stopwatch stopwatch = new Stopwatch();
    ···
    SKBitmap FillBitmapSetPixel(out string description, out int milliseconds)
    {
        description = "SetPixel";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        for (int rep = 0; rep < REPS; rep++)
            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    bitmap.SetPixel(col, row, new SKColor((byte)col, 0, (byte)row));
                }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
}
```

Kolor dla każdego piksela ma składnik czerwony równa kolumny mapy bitowej i równych składnik niebieski wiersza. Wynikowe mapy bitowej jest czarny w lewym górnym rogu, czerwony w prawym górnym rogu, niebieski w lewej dolnej i amarantowy w dolnym rogu przy użyciu gradientów w innym miejscu.

`SetPixel` Metoda jest wywoływana 65 536 razy i niezależnie od tego, jak wydajne ta metoda może być, zazwyczaj nie jest dobry pomysł, aby wprowadzić tę liczbę wywołań interfejsu API, jeśli dostępny jest alternatywą. Na szczęście istnieje kilka rozwiązań alternatywnych.

### <a name="the-pixels-property"></a>Właściwość pikseli

`SKBitmap` definiuje [ `Pixels` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Pixels/) właściwość, która zwraca tablicę `SKColor` wartości całą mapę bitową. Można również użyć `Pixels` można ustawić tablicę wartości kolorów mapy bitowej:

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

Piksele są rozmieszczone w tablicy, począwszy pierwszy wiersz, od lewej do prawej, następnie drugiego wiersza i tak dalej. Całkowita liczba kolorów w tablicy jest równa iloczyn wysokość i szerokość mapy bitowej.

Mimo, że właściwość ta wydaje się być efektywne, należy pamiętać, który pikseli są kopiowane z mapy bitowej do tablicy, a także z tablicy do mapy bitowej i pikseli są konwertowane z i do `SKColor` wartości.

Poniżej przedstawiono metody `GradientBitmapPage` klasę, która ustawia za pomocą mapy bitowej `Pixels` właściwości. Metoda przydziela `SKColor` tablicę wymagany rozmiar, ale mógł użyć `Pixels` właściwości do utworzenia tablicy:

```csharp
SKBitmap FillBitmapPixelsProp(out string description, out int milliseconds)
{
    description = "Pixels property";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    SKColor[] pixels = new SKColor[256 * 256]; 

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                pixels[256 * row + col] = new SKColor((byte)col, 0, (byte)row);
            }

    bitmap.Pixels = pixels;

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Należy zauważyć, że indeks `pixels` tablicy musi być obliczana na podstawie `row` i `col` zmiennych. Wiersz jest mnożony przez liczbę pikseli w każdym wierszu (256 w tym przypadku), a następnie dodać kolumny.

`SKBitmap` definiuje również podobny `Bytes` właściwość, która zwraca tablicę bajtów w całej mapy bitowej, ale jest bardziej złożona, dla pełnego kolorach.

### <a name="the-getpixels-pointer"></a>Wskaźnik GetPixels

Potencjalnie jest najbardziej zaawansowanych technik dostępu pikseli mapy bitowej do [ `GetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.GetPixels()/), nie należy mylić z `GetPixel` metody lub `Pixels` właściwości. Natychmiast zauważysz różnicy z `GetPixels` , zwraca coś nie są bardzo popularne w programowaniu w języku C#:

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

.NET [ `IntPtr` ](xref:System.IntPtr) typu reprezentuje wskaźnik. Jest on nazywany `IntPtr` ponieważ długość liczby całkowitej na macierzystego procesora komputera, na którym program jest uruchamiany, zazwyczaj 32-bitowy lub 64 bity długości. `IntPtr` , `GetPixels` Zwraca jest adresem rzeczywiste bloku pamięci, która używa obiektu mapy bitowej do przechowywania jego pikseli. 

Możesz przekonwertować `IntPtr` w języku C# wskaźnika typu, używając [ `ToPointer` ](xref:System.IntPtr.ToPointer) metody. Składnia języka C# wskaźnik jest taki sam jak C i C++:

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

`ptr` Zmienna jest typu _wskaźnik bajt_. To `ptr` zmienny zezwala na dostęp do poszczególnych bajtów pamięci, które są używane do przechowywania mapy bitowej w pikselach. Kod umożliwia Odczytaj bajt z tej pamięci lub zapisywać typu byte do pamięci:

```sharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

W tym kontekście gwiazdka jest języka C# _operatora pośredniego_ i jest używana do odwołania zawartości pamięci wskazywany przez `ptr`. Początkowo `ptr` punktów do pierwszego bajtu piksela pierwszego wiersza mapy bitowej, ale można wykonywać operacji arytmetycznych na `ptr` zmiennej, aby przenieść go do innych miejsc w mapie bitowej.

Wadą jest to, że możesz użyć tej funkcji `ptr` zmiennej tylko w bloku kodu oznaczone `unsafe` — słowo kluczowe. Ponadto zestaw musi mieć flagę co bloki niebezpieczne. Odbywa się we właściwościach projektu.

Za pomocą wskaźników w języku C# jest bardzo zaawansowane, ale także niezwykle niebezpiecznych. Musisz należy zachować ostrożność, że nie dostęp do pamięci powyżej wskaźnik powinna się odwoływać. Dlatego użyj wskaźnika jest skojarzony z wyrazem "niebezpieczny".

Poniżej przedstawiono metody `GradientBitmapPage` klasy, która używa `GetPixels` metody. Zwróć uwagę `unsafe` blok, który obejmuje cały kod przy użyciu wskaźnika bajtów:

```csharp
SKBitmap FillBitmapBytePtr(out string description, out int milliseconds)
{
    description = "GetPixels byte ptr";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            byte* ptr = (byte*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (byte)(col);   // red
                    *ptr++ = 0;             // green
                    *ptr++ = (byte)(row);   // blue
                    *ptr++ = 0xFF;          // alpha
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Gdy `ptr` zmiennej najpierw jest uzyskiwana z `ToPointer` metody wskazuje na pierwszy bajt skrajnie po lewej stronie piksel pierwszy wiersz mapy bitowej. `for` Pętli `row` i `col` zostały skonfigurowane tak, aby `ptr` można zwiększyć za pomocą `++` operator po ustawieniu poszczególne bajty każdego piksela. Dla 99 pętli za pośrednictwem pikseli `ptr` musi być ustawione ponownie na początku mapy bitowej.

Każdego piksela jest cztery bajty pamięci, dzięki czemu poszczególne bajty musi zostać ustawione osobno. W tym miejscu w kodzie założono, że bajtów są w kolejności czerwony, zielony, niebieski i alfa, który jest zgodny z `SKColorType.Rgba8888` koloru typu. Może się odwołać, czy jest to domyślny typ kolorów dla systemów iOS i Android, ale nie dla platformy uniwersalnej Windows. Domyślnie, platformy uniwersalnej systemu Windows tworzy map bitowych o `SKColorType.Bgra8888` koloru typu. Z tego powodu wyniki powinny być widoczne niektóre różnych na tej platformie!

Można rzutować wartości zwróconej z `ToPointer` do `uint` wskaźnika zamiast `byte` wskaźnika. Dzięki temu całą pikseli, można uzyskać dostęp w jednej instrukcji. Stosowanie `++` operatora ten wskaźnik zwiększa go przez cztery bajty, aby wskazywał pikseli dalej:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    SKBitmap FillBitmapUintPtr(out string description, out int milliseconds)
    {
        description = "GetPixels uint ptr";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        IntPtr pixelsAddr = bitmap.GetPixels();

        unsafe
        {
            for (int rep = 0; rep < REPS; rep++)
            {
                uint* ptr = (uint*)pixelsAddr.ToPointer();

                for (int row = 0; row < 256; row++)
                    for (int col = 0; col < 256; col++)
                    {
                        *ptr++ = MakePixel((byte)col, 0, (byte)row, 0xFF);
                    }
            }
        }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    uint MakePixel(byte red, byte green, byte blue, byte alpha) =>
            (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

Piksel można ustawić przy użyciu `MakePixel` metody, która tworzy pikseli liczby całkowitej ze składników czerwony, zielony, niebieski i alfa. Należy pamiętać, że `SKColorType.Rgba8888` format ma piksel, porządkowanie następująco:

AA RR GG BB

Jednak jest liczba całkowita, odpowiadający tych bajtów:

AABBGGRR

Najmniej znaczący bajt liczby całkowitej znajduje się najpierw zgodnie z architektury little-endian. To `MakePixel` metody nie będą działać poprawnie dla map bitowych o `Bgra8888` koloru typu.

`MakePixel` Metody jest oznaczony za pomocą [ `MethodImplOptions.AggressiveInlining` ](xref:System.Runtime.CompilerServices.MethodImplOptions) opcję, aby zachęcić kompilatora, aby uniknąć konieczności to oddzielnych metodach, ale zamiast tego można skompilować kod gdy metoda jest wywoływana. Należy to poprawić wydajność.

Co ciekawe `SKColor` struktury definiuje konwersja jawna z `SKColor` na liczbę całkowitą bez znaku, co oznacza, że `SKColor` wartości mogą być tworzone i konwersja na `uint` mogą być używane zamiast `MakePixel`:

```csharp
SKBitmap FillBitmapUintPtrColor(out string description, out int milliseconds)
{
    description = "GetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            uint* ptr = (uint*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (uint)new SKColor((byte)col, 0, (byte)row);
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Tylko pytaniem jest: format liczby całkowitej, które mają `SKColor` wartość kolejności atrybutu `SKColorType.Rgba8888` koloru typu, lub `SKColorType.Bgra8888` kolorów, typ, czy jest to coś innego całkowicie? Odpowiedź na to pytanie jest będą wkrótce ujawnione.

### <a name="the-setpixels-method"></a>Metoda SetPixels

`SKBitmap` definiuje również metodę o nazwie [ `SetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.SetPixels/p/System.IntPtr/), które wywołujesz następująco:

```csharp
bitmap.SetPixels(intPtr);
```

Pamiętamy `GetPixels` uzyskuje `IntPtr` odwołuje się do bloku pamięci używany przez mapy bitowej do przechowywania jego pikseli. `SetPixels` Wywołania _zastępuje_ tego bloku pamięci za pomocą bloku pamięci, odwołuje się `IntPtr` określony jako `SetPixels` argumentu. Mapa bitowa zwalnia blok pamięci, których używano wcześniej. Przy następnym `GetPixels` jest wywoływana, uzyskuje bloku pamięci, ustawić za pomocą `SetPixels`.

Na początku wydaje się tak, jakby `SetPixels` udostępnia już moc i wydajność niż `GetPixels` będąc mniej wygodne. Za pomocą `GetPixels` uzyskać blok pamięci mapy bitowej i uzyskać do niego dostęp. Za pomocą `SetPixels` przydzielić i dostęp do pamięci i następnie ustaw ją jako blok pamięci mapy bitowej.

Jednak przy użyciu `SetPixels` oferuje składni przewagę: umożliwia dostęp do bitów pikseli mapy bitowej przy użyciu tablicy. Poniżej przedstawiono metody `GradientBitmapPage` , pokazano tej techniki. Metoda najpierw definiuje tablicy bajtowej wielowymiarowych odpowiadający bajtów mapy bitowej w pikselach. Pierwszy wymiar to wiersz, drugiego wymiaru kolumny i corresonds trzeciego wymiaru cztery składniki każdego piksela:

```csharp
SKBitmap FillBitmapByteBuffer(out string description, out int milliseconds)
{
    description = "SetPixels byte buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    byte[,,] buffer = new byte[256, 256, 4];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col, 0] = (byte)col;   // red
                buffer[row, col, 1] = 0;           // green
                buffer[row, col, 2] = (byte)row;   // blue
                buffer[row, col, 3] = 0xFF;        // alpha
            }
    
    unsafe
    {
        fixed (byte* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Następnie po napełnieniu tablicy pikseli, `unsafe` bloku i `fixed` instrukcja jest używane do uzyskania wskaźnika bajtów, który wskazuje na tej tablicy. Następnie może być rzutowany ten wskaźnik bajtów `IntPtr` do przekazania do `SetPixels`.

Tablica, która tworzenia nie musi być tablicą bajtów. Może być tablicę liczb całkowitych z tylko dwoma wymiarami dla wierszy i kolumn:

```csharp
SKBitmap FillBitmapUintBuffer(out string description, out int milliseconds)
{
    description = "SetPixels uint buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = MakePixel((byte)col, 0, (byte)row, 0xFF);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

`MakePixel` Metoda jest ponownie używana do łączenia składników koloru w pikselach 32-bitowych.

Tylko dla zachowania jego kompletności, Oto ten sam kod, ale z `SKColor` wartość rzutowane na liczbę całkowitą bez znaku:

```csharp
SKBitmap FillBitmapUintBufferColor(out string description, out int milliseconds)
{
    description = "SetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = (uint)new SKColor((byte)col, 0, (byte)row);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

### <a name="comparing-the-techniques"></a>Porównanie metod

Konstruktor obiektu **kolor gradientu** strona wywołuje z metod powyżej i zapisuje wyniki:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    string[] descriptions = new string[8];
    SKBitmap[] bitmaps = new SKBitmap[8];
    int[] elapsedTimes = new int[8];

    SKCanvasView canvasView;

    public GradientBitmapPage ()
    {
        Title = "Gradient Bitmap";

        bitmaps[0] = FillBitmapSetPixel(out descriptions[0], out elapsedTimes[0]);
        bitmaps[1] = FillBitmapPixelsProp(out descriptions[1], out elapsedTimes[1]);
        bitmaps[2] = FillBitmapBytePtr(out descriptions[2], out elapsedTimes[2]);
        bitmaps[4] = FillBitmapUintPtr(out descriptions[4], out elapsedTimes[4]);
        bitmaps[6] = FillBitmapUintPtrColor(out descriptions[6], out elapsedTimes[6]);
        bitmaps[3] = FillBitmapByteBuffer(out descriptions[3], out elapsedTimes[3]);
        bitmaps[5] = FillBitmapUintBuffer(out descriptions[5], out elapsedTimes[5]);
        bitmaps[7] = FillBitmapUintBufferColor(out descriptions[7], out elapsedTimes[7]);

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

Konstruktor stwierdza, tworząc `SKCanvasView` Aby wyświetlić wynikowe map bitowych. `PaintSurface` Obsługi dzieli powierzchni na osiem wywołań i prostokąty `Display` do wyświetlenia każdego z nich:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        int width = info.Width;
        int height = info.Height;

        canvas.Clear();

        Display(canvas, 0, new SKRect(0, 0, width / 2, height / 4));
        Display(canvas, 1, new SKRect(width / 2, 0, width, height / 4));
        Display(canvas, 2, new SKRect(0, height / 4, width / 2, 2 * height / 4));
        Display(canvas, 3, new SKRect(width / 2, height / 4, width, 2 * height / 4));
        Display(canvas, 4, new SKRect(0, 2 * height / 4, width / 2, 3 * height / 4));
        Display(canvas, 5, new SKRect(width / 2, 2 * height / 4, width, 3 * height / 4));
        Display(canvas, 6, new SKRect(0, 3 * height / 4, width / 2, height));
        Display(canvas, 7, new SKRect(width / 2, 3 * height / 4, width, height));
    }

    void Display(SKCanvas canvas, int index, SKRect rect)
    {
        string text = String.Format("{0}: {1:F1} msec", descriptions[index], 
                                    (double)elapsedTimes[index] / REPS);

        SKRect bounds = new SKRect();

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.TextSize = (float)(12 * canvasView.CanvasSize.Width / canvasView.Width);
            textPaint.TextAlign = SKTextAlign.Center;
            textPaint.MeasureText("Tly", ref bounds);

            canvas.DrawText(text, new SKPoint(rect.MidX, rect.Bottom - bounds.Bottom), textPaint);
            rect.Bottom -= bounds.Height;
            canvas.DrawBitmap(bitmaps[index], rect, BitmapStretch.Uniform);
        }
    }
}
```

Aby umożliwić kompilatorowi optymalizację kodu, ta strona został uruchomiony w **wersji** trybu. Poniżej przedstawiono tę stronę systemem symulatora telefonu iPhone 8 MacBook Pro, telefon z systemem Android 5 Nexus i Surface Pro 3 systemem Windows 10. Ze względu na różnice sprzętu uniknąć porównanie razy wydajności między urządzeniami, ale patrzą względne czas na poszczególnych urządzeniach:

[![Mapa bitowa gradientu](pixel-bits-images/GradientBitmap.png "gradientu mapy bitowej")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

Oto tabela, która konsoliduje czas wykonywania (w milisekundach):

| interfejs API       | Typ danych | iOS  | Android | Platforma UWP  |
| --------- | --------- | ----:| -------:| ----:|
| Element SetPixel  |           | 3.17 |   10.77 | 3.49 |
| Pikseli    |           | 0,32 |    1,23 | 0,07 |
| GetPixels | byte      | 0,09 |    0,24 | 0,10 |
|           | uint      | 0,06 |    0.26 | 0.05 |
|           | SKColor   | 0.29 |    0,99. | 0,07 |
| SetPixels | byte      | 1.33 |    6.78 | 0,11 |
|           | uint      | 0,14 |    0.69 | 0,06 |
|           | SKColor   | 0.35 |    1.93 | 0,10 |

Zgodnie z oczekiwaniami, wywołanie `SetPixel` 65 536 razy jest co najmniej sposób effeicient, ustaw pikseli mapy bitowej. Wypełnianie `SKColor` tablicy i ustawienie `Pixels` właściwość jest znacznie lepsze i jeszcze porównuje favorably z niektórymi z `GetPixels` i `SetPixels` technik. Praca z `uint` wartości pikseli jest zwykle szybsze niż ustawienie oddzielnych `byte` składników i konwertowania `SKColor` wartości na liczbę całkowitą bez znaku dodaje pewne nadmiarowe obciążenie do procesu.

Warto również porównanie różnych gradientów: pierwsze wiersze dla wszystkich trzech platformach są takie same i wyświetlić gradient, ponieważ ma. Oznacza to, że `SetPixel` metody i `Pixels` właściwości poprawnie utworzyć w pikselach kolorów, niezależnie od tego, podstawowy format pikseli.

Następne dwa wiersze dla systemów iOS i Android zrzuty ekranu są także takie same, co potwierdza, że małym `MakePixel` metoda jest poprawnie zdefiniowana dla domyślnej `Rgba8888` format pikseli dla tych platform.

Dolny wiersz z systemem iOS i Android zrzutów ekranu jest wstecznie, co oznacza, że liczba całkowita bez znaku uzyskane przez rzutowanie `SKColor` wartość ma postać:

AARRGGBB

Kolejność bajtów jest:

AA BB GG ZASOBU

Jest to `Bgra8888` porządkowanie zamiast `Rgba8888` kolejności. `Brga8888` Format jest ustawieniem domyślnym dla platformy Universal Windows, dlatego gradienty w ostatnim wierszu tego zrzutu ekranu są takie same jak pierwszy wiersz. Ale środkowej dwa wiersze są nieprawidłowe, ponieważ zakłada, że kod tworzenia tych map bitowych `Rgba8888` kolejności.

Jeśli chcesz użyć tego samego kodu do uzyskiwania dostępu do bitów pikseli na wszystkich trzech platformach, możesz jawnie utworzyć `SKBitmap` za pomocą `Rgba8888` lub `Bgra8888` formatu. Jeśli chcesz rzutować `SKColor` wartości do mapy bitowej w pikselach, należy użyć `Bgra8888`.

## <a name="random-access-of-pixels"></a>Losowym pikseli

`FillBitmapBytePtr` i `FillBitmapUintPtr` metody **gradientu mapy bitowej** strony korzystali ze `for` pętli przeznaczone do wypełnienia mapy bitowej sekwencyjnie od górny wiersz, aby dolny wiersz, a w każdym wierszu, od lewej do prawej. Piksel można ustawić za pomocą tej samej instrukcji, która zwiększa wskaźnika. 

Czasami zachodzi konieczność dostępu piksele losowo, a nie po kolei. Jeśli używasz `GetPixels` podejście, należy obliczyć wskaźniki oparte na wierszy i kolumn. Jest to zaprezentowane w **sinus tęczowego** strony, która tworzy mapę bitową pokazująca tęczowego w postaci jednego cyklu krzywej sinus. 

Kolory tęczowego są najłatwiej utworzyć przy użyciu modelu kolorów HSL (odcień, nasycenie, jasność). `SKColor.FromHsl` Metoda tworzy `SKColor` wartości przy użyciu wartości hue, z zakresu od 0 do 360 (na przykład kąty okrąg, ale przechodząc od czerwony, zielony i niebieski i z powrotem na czerwony) i nasycenia i jasności od 0 do 100. W przypadku kolorów tęczowego nasycenie powinna być równa maksymalnie 100 i jasność do środkowego 50.

**Sinus tęczowego** tworzy obraz wiersze mapy bitowej w pętli, a następnie w pętli 360 hue wartości. Od każdej wartości odcienia, który jest obliczana kolumny mapy bitowej, który również opiera się na wartość funkcji sinus:

```csharp
public class RainbowSinePage : ContentPage
{
    SKBitmap bitmap;

    public RainbowSinePage()
    {
        Title = "Rainbow Sine";

        bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);

        unsafe
        {
            // Pointer to first pixel of bitmap
            uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();

            // Loop through the rows
            for (int row = 0; row < bitmap.Height; row++)
            {
                // Calculate the sine curve angle and the sine value
                double angle = 2 * Math.PI * row / bitmap.Height;
                double sine = Math.Sin(angle);

                // Loop through the hues
                for (int hue = 0; hue < 360; hue++)
                {
                    // Calculate the column
                    int col = (int)(360 + 360 * sine + hue);

                    // Calculate the address
                    uint* ptr = basePtr + bitmap.Width * row + col;

                    // Store the color value
                    *ptr = (uint)SKColor.FromHsl(hue, 100, 50);
                }
            }
        }

        // Create the SKCanvasView
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

Należy zauważyć, że Konstruktor tworzy mapę bitową na podstawie `SKColorType.Bgra8888` formatu:

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

Dzięki temu programowi na używanie konwersji `SKColor` wartości w `uint` pikseli bez obaw. Mimo że nie ona pełnić rolę w tym określonego programu, zawsze można użyć `SKColor` konwersji można ustawić pikseli, należy także określić `SKAlphaType.Unpremul` ponieważ `SKColor` nie wstępnego mnożenia przez wartość alfa odpowiadającą jej składników kolorów.

Następnie używa konstruktora `GetPixels` metodę, aby uzyskać wskaźnik do pierwszego piksel mapy bitowej:

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

Dla określonego wiersza i kolumny, wartość przesunięcia muszą zostać dodane do `basePtr`. To przesunięcie jest wiersz razy szerokość mapy bitowej plus kolumny:

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

`SKColor` Wartość jest przechowywana w pamięci, za pomocą tego wskaźnika:

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

W `PaintSurface` program obsługi `SKCanvasView`, mapa bitowa jest rozciągany tak, aby wypełnił obszar wyświetlania:

[![Sinus tęczowego](pixel-bits-images/RainbowSine.png "tęczowego sinus")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>Z jednej bitmapy do innego

Bardzo wielu zadań przetwarzania obrazów obejmują modyfikowanie pikseli, które są przekazywane z jedną mapę bitową do innego. Ta technika jest przedstawiona w **dopasowanie koloru** strony. Strona ładuje się jeden z zasobów mapy bitowej i następnie można zmodyfikować obraz za pomocą trzech `Slider` widoki:

[![Kolor korekty](pixel-bits-images/ColorAdjustment.png "dostosowanie kolorów")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

Dla każdego koloru piksela pierwszy `Slider` dodaje wartość z zakresu od 0 do 360, aby odcień, ale następnie używa modulo operator, który ma być Zachowaj wynik od 0 do 360, efektywnie przesunięcie kolory wzdłuż widma (tak jak pokazano na zrzucie ekranu platformy uniwersalnej systemu Windows). Drugi `Slider` umożliwia wybranie mnożenia współczynnik zakresu od 0,5 do 2 mają dotyczyć nasycenie, a trzeci `Slider` jest taka sama dla jasności, jak pokazano na zrzucie ekranu systemu Android.

Program przechowuje dwa mapy bitowe, oryginalnym źródłową mapę bitową o nazwie `srcBitmap` i skorygowany docelową mapę bitową o nazwie `dstBitmap`. Każdorazowo `Slider` zostanie przeniesiony, wszystkie nowe pikseli jest obliczana `dstBitmap`. Oczywiście, użytkownicy będą eksperymentować, przenosząc `Slider` widoków bardzo szybko, więc chcesz, aby uzyskać najlepszą wydajność, można zarządzać. Obejmuje to `GetPixels` metoda bitmap źródłowym i docelowym. 

**Dopasowanie koloru** strony nie kontroluje format koloru źródłowego i docelowego mapy bitowej. Zamiast tego zawiera logikę nieco `SKColorType.Rgba8888` i `SKColorType.Bgra8888` formatów. Źródłowe i docelowe mogą być różne formaty i program będą nadal działać.

W tym miejscu to program, z wyjątkiem kluczowym `TransferPixels` metody, która przesyła piksele tworzą źródła do miejsca docelowego. Ustawia konstruktora `dstBitmap` równa `srcBitmap`. `PaintSurface` Wyświetla obsługi `dstBitmap`:

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    SKBitmap srcBitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKBitmap dstBitmap;

    public ColorAdjustmentPage()
    {
        InitializeComponent();

        dstBitmap = new SKBitmap(srcBitmap.Width, srcBitmap.Height);
        OnSliderValueChanged(null, null);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        float hueAdjust = (float)hueSlider.Value;
        hueLabel.Text = $"Hue Adjustment: {hueAdjust:F0}";

        float saturationAdjust = (float)Math.Pow(2, saturationSlider.Value);
        saturationLabel.Text = $"Saturation Adjustment: {saturationAdjust:F2}";

        float luminosityAdjust = (float)Math.Pow(2, luminositySlider.Value);
        luminosityLabel.Text = $"Luminosity Adjustment: {luminosityAdjust:F2}";

        TransferPixels(hueAdjust, saturationAdjust, luminosityAdjust);
        canvasView.InvalidateSurface();
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(dstBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

`ValueChanged` Obsługa `Slider` widoków oblicza wartości dostosowania i wywołania `TransferPixels`.

Cały `TransferPixels` metoda jest oznaczona jako `unsafe`. Rozpoczyna się od uzyskiwanie wskaźników bajtów z bitami pikseli, zarówno map bitowych, a następnie w pętli wszystkich wierszy i kolumn. Z źródłową mapę bitową metoda uzyskuje czterech bajtów dla każdego piksela. Mogą to być albo `Rgba8888` lub `Bgra8888` zamówienia. Sprawdzanie dla typ koloru, zezwala `SKColor` wartość ma zostać utworzony. Składniki HSL są następnie wyodrębniany, dostosować i używana do odtworzenia `SKColor` wartość. W zależności od tego, czy jest docelową mapę bitową `Rgba8888` lub `Bgra8888`, bajty są przechowywane w bitmp miejsce docelowe:

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    ···
    unsafe void TransferPixels(float hueAdjust, float saturationAdjust, float luminosityAdjust)
    {
        byte* srcPtr = (byte*)srcBitmap.GetPixels().ToPointer();
        byte* dstPtr = (byte*)dstBitmap.GetPixels().ToPointer();

        int width = srcBitmap.Width;       // same for both bitmaps
        int height = srcBitmap.Height;

        SKColorType typeOrg = srcBitmap.ColorType;
        SKColorType typeAdj = dstBitmap.ColorType;

        for (int row = 0; row < height; row++)
        {
            for (int col = 0; col < width; col++)
            {
                // Get color from original bitmap
                byte byte1 = *srcPtr++;         // red or blue
                byte byte2 = *srcPtr++;         // green
                byte byte3 = *srcPtr++;         // blue or red
                byte byte4 = *srcPtr++;         // alpha

                SKColor color = new SKColor();

                if (typeOrg == SKColorType.Rgba8888)
                {
                    color = new SKColor(byte1, byte2, byte3, byte4);
                }
                else if (typeOrg == SKColorType.Bgra8888)
                {
                    color = new SKColor(byte3, byte2, byte1, byte4);
                }

                // Get HSL components
                color.ToHsl(out float hue, out float saturation, out float luminosity);

                // Adjust HSL components based on adjustments
                hue = (hue + hueAdjust) % 360;
                saturation = Math.Max(0, Math.Min(100, saturationAdjust * saturation));
                luminosity = Math.Max(0, Math.Min(100, luminosityAdjust * luminosity));

                // Recreate color from HSL components
                color = SKColor.FromHsl(hue, saturation, luminosity);

                // Store the bytes in the adjusted bitmap
                if (typeAdj == SKColorType.Rgba8888)
                {
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Alpha;
                }
                else if (typeAdj == SKColorType.Bgra8888)
                {
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Alpha;
                }
            }
        }
    }
    ···
}
```

Istnieje prawdopodobieństwo, że wydajność tej metody można jeszcze bardziej poprawić przez utworzenie oddzielnych metody dla różnych kombinacji typów kolorów mapy bitowej źródłowe i docelowe i uniknąć, sprawdzając typ dla każdego piksela. Innym rozwiązaniem jest wielu `for` pętli `col` zmiennej na podstawie koloru typu.

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
