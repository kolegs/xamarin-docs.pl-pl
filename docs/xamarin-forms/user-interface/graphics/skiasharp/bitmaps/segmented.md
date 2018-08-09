---
title: Segmenty wyświetlanie map bitowych SkiaSharp
description: Wyświetlić mapę bitową SkiaSharp obszaru niektórych konfiguracji, a niektóre obszary nie są.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79AE2033-C41C-4447-95A6-76D22E913D19
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: e5bfa076a8746abd6275e9d7a8393c7c8ab53294
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615239"
---
# <a name="segmented-display-of-skiasharp-bitmaps"></a>Segmenty wyświetlanie map bitowych SkiaSharp

Skiasharp — `SKCanvas` obiektu definiuje metodę o nazwie `DrawBitmapNinePatch` i dwie metody o nazwie `DrawBitmapLattice` , które są bardzo podobne. Zarówno te metody renderowania mapy bitowej do rozmiaru prostokąta docelowego, ale zamiast równomiernie rozciąganie mapy bitowej, wyświetlanie części mapy bitowej w jego rozmiar w pikselach i stretch innych części mapę bitową tak, aby zmieścił się prostokąt:

![Przykłady segmentowanych](segmented-images/SegmentedSample.png "Segmentowanych próbki")

Te metody są zazwyczaj używane do renderowania mapy bitowe, które stanowią część obiektów interfejsu użytkownika, takich jak przyciski. Podczas projektowania przycisku, zazwyczaj chcesz rozmiar przycisku, aby być na podstawie zawartości przycisku, ale prawdopodobnie ma obramowanie przycisku do tej samej szerokości, niezależnie od zawartości na przycisku. Jest to idealne rozwiązanie zastosowanie `DrawBitmapNinePatch`.

`DrawBitmapNinePatch` jest szczególnym przypadkiem `DrawBitmapLattice` , ale jest łatwiejsze z dwóch metod w celu jego wykorzystanie i zrozumienie.

## <a name="the-nine-patch-display"></a>Wyświetlanie dziewięć poprawki 

Model [ `DrawBitmapNinePatch` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapNinePatch/p/SkiaSharp.SKBitmap/SkiaSharp.SKRectI/SkiaSharp.SKRect/SkiaSharp.SKPaint/) dzieli mapy bitowej na prostokąty dziewięć:

![Poprawka dziewięć](segmented-images/NinePatch.png "dziewięć poprawki")

Prostokąty na cztery rogi są wyświetlane w ich wielkości pikseli. Jako strzałki wskazują, innych obszarów na krawędziach mapy bitowej konfiguracji poziomo lub pionowo do obszaru prostokąta docelowego. Prostokątem na środku jest rozciągana w poziomie i w pionie. 

Jeśli w prostokąta docelowego, aby wyświetlić nawet dowiedzą wymiarami pikseli jest za mało miejsca, są skalowane w dół do dostępny rozmiar i od niczego, ale są wyświetlane cztery rogi.

Aby podzielić mapy bitowej na tych prostokąty dziewięć, jest tylko należy określić prostokątem na środku. To jest składnia `DrawBitmapNinePatch` metody:

```csharp
canvas.DrawBitmapNinePatch(bitmap, centerRectangle, destRectangle, paint);
```

Prostokąt Centrum jest określana względem mapy bitowej. Jest `SKRectI` wartość (liczba całkowita wersję `SKRect`) i wszystkich współrzędnych i rozmiarów znajdują się w jednostkach pikseli. Prostokąta docelowego jest określana względem wyświetlanej powierzchni. `paint` Argument jest opcjonalny.

**Wyświetlania poprawki dziewięciu** strony w [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) najpierw próbki statyczny Konstruktor do tworzenia publiczna właściwość statyczna typu `SKBitmap`:

```csharp
public partial class NinePatchDisplayPage : ContentPage
{
    static NinePatchDisplayPage()
    {
        using (SKCanvas canvas = new SKCanvas(FiveByFiveBitmap))
        using (SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 10
        })
        {
            for (int x = 50; x < 500; x += 100)
                for (int y = 50; y < 500; y += 100)
                {
                    canvas.DrawCircle(x, y, 40, paint);
                }
        }
    }

    public static SKBitmap FiveByFiveBitmap { get; } = new SKBitmap(500, 500);
    ···
}
```

Dwie strony w tym artykule, użyj tej samej mapy bitowej. Mapa bitowa jest 500 pikseli w kształcie kwadratu i składa się z tablicy 25 okręgów, ten sam rozmiar, zajmuje obszar kwadratowy 100 pikseli:

![Koło siatki](segmented-images/CircleGrid.png "koło siatki")

Tworzy konstruktora wystąpienia programu `SKCanvasView` z `PaintSurface` program obsługi, który używa `DrawBitmapNinePatch` do wyświetlania mapy bitowej rozciągnięte do powierzchni całej zawartości ekranu:

```csharp
public class NinePatchDisplayPage : ContentPage
{
    ···
    public NinePatchDisplayPage()
    {
        Title = "Nine-Patch Display";

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

        SKRectI centerRect = new SKRectI(100, 100, 400, 400);
        canvas.DrawBitmapNinePatch(FiveByFiveBitmap, centerRect, info.Rect);
    }
}
```

`centerRect` Prostokąt obejmuje centralne tablicy okręgów 16. Okręgów w rogach są wyświetlane w ich wymiarów, a cała reszta jest rozciągany tak, w związku z tym:

[![Wyświetlanie dziewięć Patch](segmented-images/NinePatchDisplay.png "wyświetlania dziewięć poprawki")](segmented-images/NinePatchDisplay-Large.png#lightbox)

Na stronie platformy uniwersalnej systemu Windows ma miejsce szerokość 500 pikseli i dlatego wyświetla wiersze górny i dolny jako serię okręgów, tego samego rozmiaru. W przeciwnym razie konfiguracji okręgów, które nie znajdują się w rogach na wielokropek w formularzu.

Otrzymano nieoczekiwany wyświetlania obiektów składający się z kombinacji okręgi i elipsy spróbuj Definiowanie prostokąta center, które zachodzą wierszy i kolumn okręgów:

```csharp
SKRectI centerRect = new SKRectI(150, 150, 350, 350);
```

## <a name="the-lattice-display"></a>Wyświetlanie struktury

Dwa `DrawBitmapLattice` metody są podobne do `DrawBitmapNinePatch`, ale są one lotniczych dowolną liczbę podziałów poziomej lub pionowej. Przegrody te są definiowane przez tablice liczb całkowitych, odpowiadający pikseli. 

[ `DrawBitmapLattice` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapLattice/p/SkiaSharp.SKBitmap/System.Int32[]/System.Int32[]/SkiaSharp.SKRect/SkiaSharp.SKPaint/) Metody z parametrami dla tych tablice liczb całkowitych nie wydaje się działać. [ `DrawBitmapLattice` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapLattice/p/SkiaSharp.SKBitmap/SkiaSharp.SKLattice/SkiaSharp.SKRect/SkiaSharp.SKPaint/) Metody z parametrem typu `SKLattice` pracy, która jest używana w przykładach pokazano poniżej.

[ `SKLattice` ](https://developer.xamarin.com/api/type/SkiaSharp.SKLattice/) Struktury definiuje cztery właściwości:

- [`XDivs`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.XDivs/), tablicy liczb całkowitych
- [`YDivs`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.YDivs/), tablicy liczb całkowitych
- [`Flags`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.Flags/), tablicę `SKLatticeFlags`, typ wyliczenia
- [`Bounds`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.Bounds/) typu `Nullable<SKRectI>` do określenia prostokąta źródłowego opcjonalne, w ramach mapy bitowej

`XDivs` Tablicy dzieli szerokość mapy bitowej na pionowe paski. Usuń pierwsze rozciąga się od piksela 0 po lewej stronie `XDivs[0]`. Ten pasek jest renderowany w jego szerokość w pikselach. Rozciąga się od drugiego paska `XDivs[0]` do `XDivs[1]`i jest rozciągana. Trzeci paska rozciąga się od `XDivs[1]` do `XDivs[2]` i jest wyświetlana w jego szerokość w pikselach. Ostatni pasek rozciąga się od ostatniego elementu w tablicy do prawej krawędzi mapy bitowej. Jeśli tablica ma parzystą liczbę elementów, a następnie pojawi się w jego szerokość w pikselach. W przeciwnym razie jest rozciągana. Całkowita liczba pionowe paski to jedna więcej niż liczba elementów w tablicy.

`YDivs` Tablica jest podobna. Wysokość tablicy jest podzielony na poziome paski.

Razem `XDivs` i `YDivs` tablicy podzielić prostokąty mapy bitowej. Liczba prostokąty jest równy iloczyn liczby poziome paski i liczba pionowe paski.

Zgodnie z dokumentacją Skia `Flags` tablica zawiera najpierw jednego elementu dla każdego prostokąta, górny wiersz prostokąty, a następnie drugiego wiersza i tak dalej. `Flags` Tablica jest tego typu [ `SKLatticeFlags` ](https://developer.xamarin.com/api/type/SkiaSharp.SKLatticeFlags/), wyliczenie z następujących elementów członkowskich:

- `Default` wartość 0
- `Transparent` z wartością 1

Jednak te flagi prawdopodobnie nie działać zgodnie z mają i najlepiej je ignorować. Ale nie należy ustawiać `Flags` właściwość `null`. Ustaw ją na tablicę `SKLatticeFlags` wartości wystarczająco duży, aby obejmować łączną liczbę prostokąty.

**Kratowych dziewięciu poprawki** stronie używa `DrawBitmapLattice` zdaje się `DrawBitmapNinePatch`. Używa tego samego map bitowych utworzonych w `NinePatchDisplayPage`:

```csharp
public class LatticeNinePatchPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeNinePatchPage ()
    {
        Title = "Lattice Nine-Patch";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    `
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 400 };
        lattice.YDivs = new int[] { 100, 400 };
        lattice.Flags = new SKLatticeFlags[9]; 

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

Zarówno `XDivs` i `YDivs` właściwości są ustawiane w tablicach zaledwie dwóch liczb całkowitych, dzielenie mapy bitowej na trzy paski w poziomie i w pionie: od piksela 0 piksel (renderowany w rozmiar w pikselach), 100 od pikseli od 100 do 400 pikseli (rozciągnięte), a od piksela 400 pikseli 500 (rozmiar w pikselach). Razem `XDivs` i `YDivs` zdefiniować łącznie 9 prostokątów, czyli rozmiar elementu `Flags` tablicy. Po prostu utworzenie tablicy jest wystarczające, aby utworzyć tablicę `SKLatticeFlags.Default` wartości.

Wyświetlanie jest taka sama jak poprzedni program:

[![Kratowych dziewięć Patch](segmented-images/LatticeNinePatch.png "kratowych dziewięć poprawki")](segmented-images/LatticeNinePatch-Large.png#lightbox)

**Wyświetlania kratowych** strony dzieli mapy bitowej w prostokątach, 16:

```csharp
public class LatticeDisplayPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeDisplayPage()
    {
        Title = "Lattice Display";

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

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 200, 400 };
        lattice.YDivs = new int[] { 100, 300, 400 };

        int count = (lattice.XDivs.Length + 1) * (lattice.YDivs.Length + 1);
        lattice.Flags = new SKLatticeFlags[count];

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

`XDivs` i `YDivs` tablice są nieco inny, powodując wyświetlany obraz do potrzeb nie może być dość możliwie symetryczna poprzednich przykładach:

[![Wyświetlanie struktury](segmented-images/LatticeDisplay.png "kratowych wyświetlania")](segmented-images/LatticeDisplay-Large.png#lightbox)

Dla systemu iOS i Android obrazów po lewej stronie mniejsze okręgów — zostaną zrenderowane w ich wielkości pikseli. Cała reszta jest rozciągana.

**Wyświetlania kratowych** strony uogólnia tworzenie `Flags` tablicy, dzięki czemu możesz eksperymentować `XDivs` i `YDivs` łatwiejsze. W szczególności warto zobaczyć, co się stanie, gdy wartość pierwszego elementu `XDivs` lub `YDivs` tablicy na 0. 

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
