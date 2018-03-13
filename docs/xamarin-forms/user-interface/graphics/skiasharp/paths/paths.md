---
title: "Podstawowe informacje o ścieżce"
description: "Eksploruj obiektu SkiaSharp SKPath do łączenia połączonych linii i krzywych"
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: f1ce6b62ef13d24148048253700d7b3bff805fad
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="path-basics"></a>Podstawowe informacje o ścieżce

_Eksploruj obiektu SkiaSharp SKPath do łączenia połączonych linii i krzywych_

Jednym z najważniejszych funkcji ścieżki grafiki jest zdefiniowanie po wielu wierszy powinny być połączone, a jeśli ich nie należy łączyć. Różnica może być dość widoczne jako prezentacja wierzchołki trójkąty dwóch:

![](paths-images/connectedlinesexample.png "Dwa trójkąty przedstawiający różnica między liniami połączonych i niepołączonych")

Ścieżki grafiki jest hermetyzowany przez [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) obiektu. Ścieżki to kolekcja jednej lub kilku *konturów*. Każdy rozkład jest kolekcją *połączone* prostej linii i krzywych. Konturów nie są połączone ze sobą, ale może wizualnie zachodziły na siebie. Czasami pojedynczego rozkład mogą nakładać się na samej siebie.

Rozkład zazwyczaj rozpoczyna się od wywołanie następującej metody `SKPath`:

- `MoveTo` Aby rozpocząć nowy rozkład

Argument tej metody jest pojedynczy punkt, który można wyrazić jako `SKPoint` wartość lub jako osobne X i Y współrzędne. `MoveTo` Wywołania ustanawia punkt na początku rozkład i początkowego *bieżącego punktu*. Możesz wywołać następujących metod, aby kontynuować rozkład z wierszem lub krzywej od bieżącego punktu do punktu w metodzie, która staje się punktem bieżącym:

- `LineTo` Aby dodać prostej do ścieżki
- `ArcTo` Aby dodać łuk, znajduje się w wierszu w obwodzie elipsy lub okręgu
- `CubicTo` Aby dodać sześcienny krzywej Beziera
- `QuadTo` Aby dodać kwadratową krzywej Beziera
- `ConicTo` Aby dodać ich rozsądne kwadratową Beziera krzywej składanej, która dokładnie umożliwiający renderowanie conic sekcje (elipsy, parabole i hiperbole)

Żadna z tych metod pięć zawierać wszystkie informacje niezbędne do opisywania linii lub krzywej. Każda z tych metod pięć działa w połączeniu z bieżącego punktu określonego przez wywołanie metody bezpośrednio poprzedzającym go. Na przykład `LineTo` metoda dodaje prostej do konturu na podstawie bieżącego punktu, więc parametru do `LineTo` jest tylko jeden punkt.

`SKPath` Metod, które mają takie same nazwy, jak te metody, ale definiuje także klasy `R` na początku:

- `RMoveTo`
- `RLineTo`
- `RArcTo`
- `RCubicTo`
- `RQuadTo`
- `RConicTo`

`R` Oznacza *względną*. Mają one takiej samej składni jak odpowiednich metod bez `R` , ale są względem bieżącego punktu. Te są przydatne do rysowania podobne części ścieżki w metodzie, który można wywoływać wielokrotnie.

Rozkład kończy się wraz z innym wywołaniu `MoveTo` lub `RMoveTo`, zaczynający rozkład nowych lub wywołanie `Close`, który zamyka rozkładu. `Close` — Metoda automatycznie dołącza prostej od bieżącego punktu do pierwszego punktu rozkład i oznacza ścieżkę jako zamknięte, co oznacza, że będzie renderowany bez żadnych informacji o możliwościach stroke.

Przedstawia różnice między konturów otwarte i zamknięte **dwóch konturów trójkąt** strony, który korzysta z `SKPath` obiektu z dwóch konturów do renderowania, dwie trójkąty. Rozkład pierwszy jest otwarty i drugi jest zamknięty. Oto [ `TwoTriangleContours` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) klasy:

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

Pierwszy rozkład składa się z wywołania [ `MoveTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.MoveTo/p/System.Single/System.Single/) przy użyciu współrzędne X i Y zamiast `SKPoint` wartości, a następnie trzy wywołania [ `LineTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.LineTo/p/System.Single/System.Single/) do rysowania trzy strony trójkąt. Drugi rozkład ma tylko dwie wywołań `LineTo` , ale zakończeniem rozkład wywołaniem [ `Close` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Close()/), który zamyka rozkładu. Różnica polega na tym znaczące:

[![](paths-images/twotrianglecontours-small.png "Potrójna zrzut ekranu przedstawiający stronę dwóch konturów trójkąt")](paths-images/twotrianglecontours-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę dwóch konturów trójkąt")

Jak widać, pierwszy rozkład to oczywiście szereg trzy połączone linie, ale zakończenia nie łączyć się z początku. Dwa wiersze nakładają się na górze. Drugi rozkład oczywiście jest zamknięty, a została realizowane za pomocą jednego mniej `LineTo` wywołuje się, ponieważ `Close` metody automatycznie dodaje końcowego wiersza, aby zamknąć rozkładu.

`SKCanvas` Definiuje tylko jeden [ `DrawPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawPath/p/SkiaSharp.SKPath/SkiaSharp.SKPaint/) metodę, która w tej prezentacji jest wywoływana dwukrotnie do wypełnienia i obrysu ścieżki. Wszystkie konturów są wypełnione, nawet te, które nie zostały zamknięte. W celu wypełnienia ścieżki niezamknięty prostej jest zakłada się, że istnieje między punktu początkowego i końcowego konturów. Po usunięciu ostatniego `LineTo` z konturem pierwszy lub usuń `Close` wywołania z drugiego rozkład każdego rozkład będzie mieć tylko dwa boki ale zostanie wypełnione, tak jakby był on trójkąt.

`SKPath` definiuje wiele innych metod i właściwości. Następujące metody Dodaj całego konturów na ścieżkę, która może być zamknięta lub nie są zamknięte w zależności od metody:

- `AddRect`
- [`AddRoundedRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddRoundedRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddCircle`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddCircle/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddOval`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddOval/p/SkiaSharp.SKRect/SkiaSharp.SKPathDirection/)
- [`AddArc`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) Aby dodać krzywą w obwodzie elipsy
- `AddPath` Aby dodać inną ścieżkę do bieżącej ścieżki
- [`AddPathReverse`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddPathReverse/p/SkiaSharp.SKPath/) Aby dodać inną ścieżkę odwrotnie

Należy pamiętać, że `SKPath` definiuje obiekt tylko geometrii & #x 2014; serii punktów i połączeń. Tylko wtedy, gdy `SKPath` jest połączona z `SKPaint` obiektu jest ścieżką z konkretnym kolor, szerokość pociągnięć i tak dalej. Ponadto należy pamiętać, że `SKPaint` obiekt przekazywany do `DrawPath` metoda definiuje właściwości pełną ścieżkę. Jeśli chcesz narysować coś wymagające kilka kolorów, należy użyć oddzielnych ścieżki dla każdego koloru.

Tak jak wygląd rozpoczęcia i zakończenia wiersza jest definiowana za pomocą koniec obrysu, wygląd połączenie między dwoma liniami jest definiowana za pomocą *sprzężenia obrysu*. Określ to ustawienie [ `StrokeJoin` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeJoin/) właściwość `SKPaint` do elementu członkowskiego [ `SKStrokeJoin` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeJoin/) wyliczenie:

- [`Miter`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Miter/) do sprzężenia wklęsła
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Round/) zaokrąglony sprzężenia
- [`Bevel`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Bevel/) w celu utworzenia sprzężenia kostki wyłączone

**Sprzężenia obrysu** strony przedstawiono te trzy obrysu dołączeń kodu podobne do **Caps obrysu** strony. Jest to `PaintSurface` obsługi zdarzeń w [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) klasy:

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

Oto działająca na platformach trzy program:

[![](paths-images/strokejoins-small.png "Potrójna zrzut ekranu strony tworzy sprzężenie obrysu")](paths-images/strokejoins-large.png#lightbox "Potrójna zrzut ekranu strony tworzy sprzężenie obrysu")

Sprzężenie skosów, skos może składa się z punktem sharp których wiersze nawiązuje połączenie. Nowi dwa wiersze pod kątem małych sprzężenie skosów, skos może może stać się bardzo długie. Aby zapobiec sprzężenia ostre zbyt długie, długość sprzężenie skosów, skos może jest ograniczone przez wartość [ `StrokeMiter` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeMiter/) właściwość `SKPaint`. Sprzężenie skosów, skos może przekraczającą tej długości została obcięta zostać sprzężenia fazy.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
