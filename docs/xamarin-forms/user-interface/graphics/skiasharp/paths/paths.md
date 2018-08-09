---
title: Ścieżka — podstawy w SkiaSharp
description: W tym artykule Eksploruje obiektu SkiaSharp SKPath łączenia połączonych linii i krzywych i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 3c07614c12fb503638d3d5e63b24eb5367ba691a
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615538"
---
# <a name="path-basics-in-skiasharp"></a>Ścieżka — podstawy w SkiaSharp

_Zapoznaj się z obiektu SkiaSharp SKPath łączenia połączonych linii i krzywych_

Jednym z najważniejszych funkcji ścieżki grafiki jest możliwość definiowania, gdy wiele wierszy powinna być połączona, a jeśli one nie należy łączyć. Różnica może być dość widoczne, jak prezentacja wierzchołki trójkąty dwa:

![](paths-images/connectedlinesexample.png "Dwa trójkąty przedstawiający różnica między wierszami połączonych i niepołączonych")

Ścieżki grafiki jest hermetyzowany przez [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) obiektu. Ścieżka to kolekcja jednej lub więcej *konturów*. Każdy rozkład to zbiór *połączone* prostej linii i krzywych. Konturów nie są połączone ze sobą, ale może być wizualnie nakładają się. Czasami pojedynczego rozkład mogą się nakładać.

Rozkład rozpoczyna się zazwyczaj z wywołaniem do następującej metody `SKPath`:

- `MoveTo` Aby rozpocząć nowy konturu

Argument do tej metody jest pojedynczy punkt można wyrazić jako `SKPoint` wartości lub jako osobne X i Y współrzędne. `MoveTo` Wywołanie ustanawia punkt początku rozkład i po początkowym *bieżący punkt*. Można wywołać poniższych metod, aby kontynuować rozkład z linią lub krzywej w bieżącym punkcie do punktu, określone w metody, która staje się punktem bieżącym:

- `LineTo` Aby dodać prostej do ścieżki
- `ArcTo` Aby dodać łuk, linia na obwód koła lub wielokropka
- `CubicTo` Aby dodać trzeciego stopnia Krzywa Beziera
- `QuadTo` Aby dodać drugiego stopnia Krzywa Beziera
- `ConicTo` Aby dodać wymierne drugiego stopnia Beziera krzywej składanej, która dokładnie umożliwiający renderowanie conic sekcje (wielokropek parabole i hiperbole)

Żadna z tych metod pięć zawierają wszystkie informacje niezbędne do opisania linii lub krzywej. Każda z tych metod pięć działa w połączeniu z bieżącym punkcie ustanowione przez wywołanie metody, bezpośrednio poprzedzających je. Na przykład `LineTo` metoda dodaje do rozkładu linię prostą oparty na bieżącym punkcie, więc parametr `LineTo` jest pojedynczym punktem.

`SKPath` Klasa definiuje również metody, które mają takie same nazwy, jak te metody, ale z `R` na początku:

- `RMoveTo`
- `RLineTo`
- `RArcTo`
- `RCubicTo`
- `RQuadTo`
- `RConicTo`

`R` Oznacza *względną*. Mają one tej samej składni jako odpowiednie metody bez `R` , ale są względne wobec bieżącego punktu. Te są przydatne do rysowania podobne części ścieżki w metodzie, która wywołać wiele razy.

Rozkład kończy się innym wywołaniu `MoveTo` lub `RMoveTo`, zaczynający się nowych rozkład lub wywołanie `Close`, który zamyka ROZKŁAD. `Close` Metoda automatycznie dołącza linię prostą rysowaną od bieżącego punktu do pierwszego punktu rozkład i oznacza ścieżkę jako zamknięty, co oznacza, że będzie ona renderowana bez żadnych pociągnięć.

Przedstawia różnicę między konturów otwarte i zamknięte **dwóch konturów trójkąt** strony, który używa `SKPath` obiektu za pomocą dwóch konturów do renderowania dwa trójkąty. Pierwszy rozkład jest otwarty, i drugi jest zamknięty. Oto [ `TwoTriangleContours` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) klasy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create the path
    SKPath path = new SKPath();

    // Define the first contour
    path.MoveTo(0.5f * info.Width, 0.1f * info.Height);
    path.LineTo(0.2f * info.Width, 0.4f * info.Height);
    path.LineTo(0.8f * info.Width, 0.4f * info.Height);
    path.LineTo(0.5f * info.Width, 0.1f * info.Height);

    // Define the second contour
    path.MoveTo(0.5f * info.Width, 0.6f * info.Height);
    path.LineTo(0.2f * info.Width, 0.9f * info.Height);
    path.LineTo(0.8f * info.Width, 0.9f * info.Height);
    path.Close();

    // Create two SKPaint objects
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Magenta,
        StrokeWidth = 50
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    // Fill and stroke the path
    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

Pierwszy rozkład składa się z wywołania [ `MoveTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.MoveTo/p/System.Single/System.Single/) przy użyciu współrzędnych X i Y, a nie `SKPoint` wartości, następuje trzech wywołań [ `LineTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.LineTo/p/System.Single/System.Single/) do rysowania trzech stronach trójkąt. Rozkład drugi ma tylko dwa wywołania `LineTo` , ale po zakończeniu rozkład wywołaniem [ `Close` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Close()/), który zamyka ROZKŁAD. Różnica polega na tym znaczące:

[![](paths-images/twotrianglecontours-small.png "Potrójna zrzut ekranu przedstawiający stronę dwóch konturów trójkąt")](paths-images/twotrianglecontours-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę dwóch konturów trójkąt")

Jak widać, rozkład pierwszy to oczywiście szereg trzy połączone linie, ale zakończenia nie połączyć z początkiem. Dwa wiersze nakładają się na górze. Drugi rozkład oczywiście jest zamknięty, a było wykonywane przy użyciu jednego mniej `LineTo` wywołuje się, ponieważ `Close` metoda automatycznie dodaje ostatni wiersz, aby zamknąć ROZKŁAD.

`SKCanvas` Definiuje tylko jeden [ `DrawPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawPath/p/SkiaSharp.SKPath/SkiaSharp.SKPaint/) metody, która w tej prezentacji zostanie dwa razy wywołana do wypełnienia i obrysu ścieżki. Wszystkie konturów są wypełnione, nawet te, które nie zostały zamknięte. Na potrzeby wypełniania ścieżki niezamknięty prostej jest zakłada się, że istnieją między punkty początkowy i końcowy konturów. Jeśli usuniesz ostatni `LineTo` z konturem pierwszy lub usuń `Close` wywołanie funkcji z sylwetką drugi każdego rozkład będzie mieć tylko dwa boki mimo wypełnione, tak jakby trójkąt.

`SKPath` definiuje wiele innych metod i właściwości. Następujące metody całego konturów należy dodać do ścieżki, która może być zamknięte lub nie zostały zamknięte w zależności od metody:

- `AddRect`
- [`AddRoundedRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddRoundedRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddCircle`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddCircle/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddOval`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddOval/p/SkiaSharp.SKRect/SkiaSharp.SKPathDirection/)
- [`AddArc`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) Aby dodać krzywą na obwód elipsę
- `AddPath` Aby dodać inną ścieżkę do bieżącej ścieżki
- [`AddPathReverse`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddPathReverse/p/SkiaSharp.SKPath/) Aby dodać inną ścieżkę w odwrotnej kolejności

Należy pamiętać, że `SKPath` obiektu definiuje tylko typy geometryczne &mdash; serii punktów i połączeń. Tylko wtedy, gdy `SKPath` jest połączony z `SKPaint` obiekt jest ścieżka, renderowane przy użyciu określonego koloru, szerokość pociągnięcia i tak dalej. Ponadto należy pamiętać, że `SKPaint` obiekt przekazany do `DrawPath` metody Określa cechy pełną ścieżkę. Jeśli chcesz narysuj coś wymaga kilku kolory, należy użyć oddzielnych ścieżki dla każdego koloru.

Tak, jak wygląd początek i koniec wiersza jest definiowany przez pociągnięcia, wygląd połączenie między dwoma wierszami jest definiowany przez *sprzężenia obrysu*. Należy to określić, ustawiając [ `StrokeJoin` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeJoin/) właściwość `SKPaint` do elementu członkowskiego [ `SKStrokeJoin` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeJoin/) wyliczenia:

- [`Miter`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Miter/) Aby uzyskać wklęsła sprzężenia
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Round/) zaokrąglone sprzężenia
- [`Bevel`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Bevel/) w celu utworzenia sprzężenia pociąć wyłączone

**Sprzężeń obrysu** stronie przedstawiono te trzy obrysu sprzężeń z kodem, podobnie jak **pociągnięć** strony. Jest to `PaintSurface` programu obsługi zdarzeń w [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) klasy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Right
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width - 100;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = 2 * textPaint.FontSpacing;
    string[] strStrokeJoins = { "Miter", "Round", "Bevel" };

    foreach (string strStrokeJoin in strStrokeJoins)
    {
        // Display text
        canvas.DrawText(strStrokeJoin, xText, y, textPaint);

        // Get stroke-join value
        SKStrokeJoin strokeJoin;
        Enum.TryParse(strStrokeJoin, out strokeJoin);

        // Create path
        SKPath path = new SKPath();
        path.MoveTo(xLine1, y - 80);
        path.LineTo(xLine1, y + 80);
        path.LineTo(xLine2, y + 80);

        // Display thick line
        thickLinePaint.StrokeJoin = strokeJoin;
        canvas.DrawPath(path, thickLinePaint);

        // Display thin line
        canvas.DrawPath(path, thinLinePaint);
        y += 3 * textPaint.FontSpacing;
    }
}
```

W tym miejscu jest uruchomiony na trzech platformach program:

[![](paths-images/strokejoins-small.png "Potrójna zrzut ekranu przedstawiający stronę sprzężenia pociągnięcia")](paths-images/strokejoins-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę sprzężenia pociągnięcia")

Sprzężenia ukośnych składa się z sharp punktu, w których łączenie wierszy. Nowi dwa wiersze pod kątem małych sprzężenia skos może stać się bardzo długie. Aby zapobiec sprzężeń ukośnych zbyt długie, długość złączenie ostre jest ograniczone przez wartość [ `StrokeMiter` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeMiter/) właściwość `SKPaint`. Sprzężenia właściwości, które przekracza długość tej została obcięta przestanie ukośne.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
