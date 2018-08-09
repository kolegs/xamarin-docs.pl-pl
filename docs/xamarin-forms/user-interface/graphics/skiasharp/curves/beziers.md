---
title: Trzy rodzaje krzywych Beziera
description: W tym artykule wyjaśniono, jak używać SkiaSharp do renderowania krzywych Beziera trzeciego stopnia, kwadratową i conic w aplikacjach Xamarin.Forms i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: charlespetzold
ms.author: chape
ms.date: 05/25/2017
ms.openlocfilehash: 0ad722f22cf5ed8dc06fdf0d1e063d285e2ddb2f
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615343"
---
# <a name="three-types-of-bzier-curves"></a>Trzy rodzaje krzywych Beziera

_Zbadaj, jak na potrzeby skiasharp — krzywe Beziera trzeciego stopnia kwadratową i conic renderowania_

Po Pierre Beziera (1910 — 1999), francuska engineer w firmie automotive Renault, która korzystała z krzywej projektu wspomaganej komputerowo treści samochodu nosi nazwę krzywej Beziera.

Krzywych Beziera wiadomo, że dobrze nadaje się do interaktywności: są one również behaved &mdash; innymi słowy, nie ma singularities, które powodują krzywej nieskończoną lub niewygodne &mdash; i zwykle aesthetically czytelnych . Wskazano znaków czcionek oparte na komputerze zwykle są definiowane za pomocą krzywych Beziera:

![](beziers-images/beziersample.png "Przykładowe krzywej Beziera")

Artykułu w Wikipedii na [krzywej Beziera](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) zawiera pewne przydatne informacje. Termin *krzywej Beziera* faktycznie odwołuje się do rodziny krzywych podobne. Skiasharp — obsługuje trzy typy krzywych Beziera, nazywanych *trzeciego stopnia*, *drugiego stopnia*i *conic*. Conic jest także znana jako *wymierne umożliwiającej obliczanie kwadratów*.

## <a name="the-cubic-bzier-curve"></a>Krzywą Beziera trzeciego stopnia

Wartości cubic jest typem, większość programistów traktować gdy podmiot krzywych Beziera krzywej Beziera.

Możesz dodać krzywą Beziera trzeciego stopnia do `SKPath` przy użyciu [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) metody z trzema `SKPoint` parametrów lub [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/System.Single/) przeciążenia z oddzielnym `x` i `y` parametry:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Krzywa rozpoczyna się w bieżącym punkcie ROZKŁAD. Pełne krzywą Beziera trzeciego stopnia jest definiowany przez cztery punkty:

- punkt początkowy: bieżący punkt w rozkład, lub (0, 0), jeśli `MoveTo` nie została wywołana.
- pierwszy punkt kontrolny: `point1` w `CubicTo` wywołania
- drugi punkt kontrolny: `point2` w `CubicTo` wywołania
- punkt końcowy: `point3` w `CubicTo` wywołania

Wynikowe krzywej rozpoczyna się od punktu początkowego i kończy się w punkcie końcowym. Krzywa ogólnie nie przechodzi przez punkty kontrolne dwóch; Zamiast tego działają dużo pól podobnego do ściągania krzywej do nich.

Najlepszym sposobem, aby można było uzyskać pewne pojęcie dla krzywej Beziera trzeciego stopnia jest eksperymentowanie w usłudze. To jest celem **krzywą Beziera** strony, która jest pochodną `InteractivePage`. [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) plik `SKCanvasView` i `TouchEffect`. [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) pliku związanego z kodem tworzy cztery `TouchPoint` obiektów w jego konstruktorze. `PaintSurface` Tworzy program obsługi zdarzeń `SKPath` do renderowania krzywej Beziera, oparte na cztery `TouchPoint` obiektów, a także rysuje kropkowana styczne z punktów kontrolnych do punktów końcowych:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with cubic Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.CubicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     touchPoints[3].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[2].Center.X,
                    touchPoints[2].Center.Y,
                    touchPoints[3].Center.X,
                    touchPoints[3].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
       touchPoint.Paint(canvas);
    }
}
```

W tym miejscu jest uruchomiona na wszystkich trzech platformach:

[![](beziers-images/beziercurve-small.png "Potrójna zrzut ekranu przedstawiający stronę krzywą Beziera")](beziers-images/beziercurve-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę krzywą Beziera")

Matematycznych krzywa jest wielomianu trzeciego stopnia. Krzywa przecina prostej w trzech miejscach co najwyżej. W momencie rozpoczęcia krzywa jest zawsze stycznej, a w tym samym kierunku co linię prostą rysowaną od początku wskaż pierwszy punkt kontrolny. W punkcie końcowym krzywa jest zawsze stycznej, a w tym samym kierunku co polecenie linię prostą rysowaną od drugiego punktu końcowego.

Krzywą Beziera trzeciego stopnia zawsze jest ograniczone przez kwadratowe wypukłe, łączenie cztery punkty. Jest to nazywane *wypukłe kadłuba*. Jeśli punkty kontrolne znajduje się na prostej między początkowego i punktu końcowego, krzywej Beziera renderowany jako linia prosta. Ale krzywej mogą również przechodzić, tak jak pokazano na trzecie zrzut ekranu.

Rozkład ścieżki może zawierać wiele połączonych krzywych Beziera trzeciego stopnia, ale połączenie między dwie krzywe Beziera trzeciego stopnia będzie smooth, tylko wtedy, gdy trzy następujące kwestie są colinear (czyli znajduje się na prostej):

- drugi punkt kontrolny krzywej pierwszy
- punkt końcowy krzywej pierwszy, która jest również punkt początkowy, drugi krzywej
- pierwszy punkt kontrolny krzywej drugiego

W następnym artykule na [ **dane ścieżki SVG** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) odkryjesz funkcji do jej obsługi ułatwiają realizację definicji smooth połączonych krzywych Beziera.

Czasami jest użyteczne informacje bazowego równania parametryczne, które renderują krzywą Beziera trzeciego stopnia. Aby uzyskać *t* od 0 do 1, równania parametryczne są następujące:

x(t) = (1 — t) ³x₀ + z programu 3t (1 — t) ²x₁ + 3t² (1 — t) x₂ + t³x₃

y(t) = (1 — t) ³y₀ + z programu 3t (1 — t) ²y₁ + 3t² (1 — t) y₂ + t³y₃

Wykładnik najwyższy 3 potwierdza, że są one polynomials trzeciego stopnia. Można łatwo sprawdzić, że w przypadku `t` jest równa 0, punktem jest (x₀ y₀), który jest punkt początkowy i kiedy `t` jest równa 1, punkt jest (x₃ y₃), który jest punktem końcowym. W pobliżu punkt początkowy (niskiej wartości `t`), pierwszy punkt kontrolny (x₁, y₁) ma silne efektu i niemal punktu końcowego (wysokiej wartości t ") drugi punkt kontrolny (x₂, y₂) jest stosowany efekt silne.

## <a name="bzier-curve-approximation-to-circular-arcs"></a>Przybliżenie krzywej Beziera na łuki okręgu

Czasami wygodne jest na potrzeby renderowania łuku okręgu krzywej Beziera. Krzywą Beziera trzeciego stopnia przybliżyć łuku okręgu bardzo dobrze maksymalnie ćwierć okręgu cztery połączonych krzywych Beziera umożliwiają definiowanie całego okrąg. W dwóch artykuły opublikowane ponad 25 lat temu omówiono takich wyników:

> Tor Dokken, et al, "Dobre zbliżenia okręgów krzywych Beziera ciągłe krzywiznę" *komputera wspomagane geometrycznych Projekt 7* (1990), 33 41.

> Michael Goldapp, "Zbliżenia Łuki okręgu przez trzeciego stopnia Polynomials" *komputera wspomagane geometrycznych Projekt 8* (1991), 227 238.

Na poniższym diagramie przedstawiono cztery punkty etykietą `pto`, `pt1`, `pt2`, i `pt3` Definiowanie krzywej Beziera (pokazane na czerwono), zbliżonym łuku okręgu:

![](beziers-images/bezierarc45.png "Przybliżenie łuku okręgu z krzywej Beziera")

Wiersze z początkowy i punkt końcowy do punkty kontrolne są tangens koła i krzywej Beziera i mają długość *L*. Pierwszego artykułu wymienionych powyżej wskazuje, że najlepsze krzywej Beziera przybliża łuku okręgu podczas tak długo *L* jest obliczana w następujący sposób:

G = 4 x tan(α / 4) / 3

Na ilustracji przedstawiono pod kątem 45 stopni, dlatego L równa 0.265. W kodzie ta wartość będzie mnożona przez żądaną promień koła.

**Łuku okręgu krzywej Beziera** strony można eksperymentować, definiując krzywej Beziera zbliżenie łuku okręgu kątów wynik może mieć maksymalnie 180 stopni. [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) plik `SKCanvasView` i `Slider` służąca do wybierania kąta. `PaintSurface` Programu obsługi zdarzeń w [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) pliku związanego z kodem używa transformacji, aby ustawić punkt (0, 0) w środku kanwy. Rysuje okrąg tematyka koncentruje się na prowadzące do porównania, a następnie oblicza punktów kontrolnych dwóch dla krzywej Beziera:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 3;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate length of control point line
    float length = radius * 4 * (float)Math.Tan(Math.PI * angle / 180 / 4) / 3;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the end points
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point3 = new SKPoint(radius * sin, radius * cos);

    // Find the control points
    SKPoint point0Normalized = Normalize(point0);
    SKPoint point1 = point0 + new SKPoint(length * point0Normalized.Y,
                                          -length * point0Normalized.X);

    SKPoint point3Normalized = Normalize(point3);
    SKPoint point2 = point3 + new SKPoint(-length * point3Normalized.Y,
                                          length * point3Normalized.X);

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);
    canvas.DrawCircle(point3.X, point3.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point3.X, point3.Y, point2.X, point2.Y, dottedStroke);

    // Draw the Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.CubicTo(point1, point2, point3);
        canvas.DrawPath(path, redStroke);
    }
}

// Vector methods
SKPoint Normalize(SKPoint v)
{
    float magnitude = Magnitude(v);
    return new SKPoint(v.X / magnitude, v.Y / magnitude);
}

float Magnitude(SKPoint v)
{
    return (float)Math.Sqrt(v.X * v.X + v.Y * v.Y);
}

```

Początkowy i punkt końcowy (`point0` i `point3`) są obliczane w oparciu o normalnym równania parametryczne koła. Ponieważ koła skupia się na (0, 0), te punkty mogą również być traktowane jako promieniowego wektorów od środka okręgu do obwodu. Punkty kontrolne są w wierszach, stycznych do okrąg, dzięki czemu można je pod kątem prostym do tych wektorów promieniowego. Wektor pod kątem prostym jest po prostu oryginalnego wektora za pomocą współrzędnych X i Y zamienione i jeden z nich wprowadzone ujemna.

W tym miejscu jest uruchomiony na trzech platformach, z trzema różnymi kątami program:

[![](beziers-images/beziercirculararc-small.png "Potrójna zrzut ekranu przedstawiający stronę łuku okręgu krzywej Beziera")](beziers-images/beziercirculararc-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę łuku okręgu krzywej Beziera")

Przyjrzyj się bliżej trzeci zrzut ekranu, a zobaczysz, że krzywej Beziera będzie przede wszystkim odbiega od promieniu, kąt ma wartość 180 stopni, kiedy ekran dla systemu iOS pokazuje, że wydaje się, aby obejmował okrąg kwartału, po prostu dobrym rozwiązaniem kąta wynosi 90 stopni.

Obliczanie współrzędne punktów kontrolnych dwóch jest całkiem proste, gdy ćwierć okręgu jest ustawiony w następujący sposób:

![](beziers-images/bezierarc90.png "Przybliżenie ćwierć okręgu z krzywej Beziera")

Jeśli promień koła wynosi 100, następnie *L* jest 55 i który jest liczbą łatwe do zapamiętania.

**Squaring koła** strony animuje rysunek między kwadratu i okrąg. Koło jest w przybliżeniu za cztery krzywych Beziera, którego współrzędne są wyświetlane w pierwszej kolumnie tę definicję tablicy w [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) klasy:

```csharp
public class SquaringTheCirclePage : ContentPage
{
    SKPoint[,] points =
    {
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() },
        { new SKPoint(  55,  100), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,   55), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,    0), new SKPoint(   125,      0), new SKPoint() },
        { new SKPoint( 100,  -55), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(  55, -100), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(   0, -100), new SKPoint(     0,   -125), new SKPoint() },
        { new SKPoint( -55, -100), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,  -55), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,    0), new SKPoint(  -125,      0), new SKPoint() },
        { new SKPoint(-100,   55), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint( -55,  100), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() }
    };
    ...
}
```

Druga kolumna zawiera współrzędne cztery krzywych Beziera, które definiują kwadratowych, w których obszar około jest taka sama jak pole koła. (Rysunek kwadrat z *dokładnie* obszar jako okrąg danego to klasyczne przesyłał geometrycznych problem [squaring koła](https://en.wikipedia.org/wiki/Squaring_the_circle).) Renderowania kwadrat za pomocą krzywych Beziera, punktów kontrolnych dwóch dla każdego krzywej są takie same, i są colinear za pomocą początkowego i punktu końcowego, więc krzywej Beziera jest renderowany jako linię prostą.

Trzecia kolumna tablicy jest interpolowane wartości dla animacji. Strony ustawia czasomierz 16 milisekund i `PaintSurface` program obsługi jest wywoływany po tym kursie:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    canvas.Translate(info.Width / 2, info.Height / 2);
    canvas.Scale(Math.Min(info.Width / 300, info.Height / 300));

    // Interpolate
    TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
    float t = (float)(timeSpan.TotalSeconds % 3 / 3);   // 0 to 1 every 3 seconds
    t = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;     // 0 to 1 to 0 sinusoidally

    for (int i = 0; i < 13; i++)
    {
        points[i, 2] = new SKPoint(
            (1 - t) * points[i, 0].X + t * points[i, 1].X,
            (1 - t) * points[i, 0].Y + t * points[i, 1].Y);
    }

    // Create the path and draw it
    using (SKPath path = new SKPath())
    {
        path.MoveTo(points[0, 2]);

        for (int i = 1; i < 13; i += 3)
        {
            path.CubicTo(points[i, 2], points[i + 1, 2], points[i + 2, 2]);
        }
        path.Close();

        canvas.DrawPath(path, cyanFill);
        canvas.DrawPath(path, blueStroke);
    }
}
```

Punkty są interpolowane na podstawie wartości sinusoidally OSCYLUJĄCA `t`. Interpolowane punkty są następnie używane do konstruowania serię czterech połączonych krzywych Beziera. Oto animacji działających na trzech platformach, pokazujący postęp okrąg kwadrat:

[![](beziers-images/squaringthecircle-small.png "Potrójna zrzut ekranu przedstawiający Squaring strony okrąg")](beziers-images/squaringthecircle-large.png#lightbox "Potrójna zrzut ekranu przedstawiający Squaring strony okręgu")

Takie animacji byłoby możliwe bez krzywych algorithmically wystarczająco elastyczny, aby być renderowana jako Łuki okręgu i proste.

**Nieskończoności Beziera** strony wykorzystuje także możliwość określenie w przybliżeniu łuku okręgu krzywej Beziera. Oto `PaintSurface` programu obsługi z [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) klasy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.MoveTo(0, 0);                                // Center
        path.CubicTo(  50,  -50,   95, -100,  150, -100); // To top of right loop
        path.CubicTo( 205, -100,  250,  -55,  250,    0); // To far right of right loop
        path.CubicTo( 250,   55,  205,  100,  150,  100); // To bottom of right loop
        path.CubicTo(  95,  100,   50,   50,    0,    0); // Back to center  
        path.CubicTo( -50,  -50,  -95, -100, -150, -100); // To top of left loop
        path.CubicTo(-205, -100, -250,  -55, -250,    0); // To far left of left loop
        path.CubicTo(-250,   55, -205,  100, -150,  100); // To bottom of left loop
        path.CubicTo( -95,  100,  -50,   50,    0,    0); // Back to center
        path.Close();

        SKRect pathBounds = path.Bounds;
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.9f * Math.Min(info.Width / pathBounds.Width,
                                     info.Height / pathBounds.Height));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 5;

            canvas.DrawPath(path, paint);
        }
    }
}
```

Może być wspaniałe do wykreślenia te współrzędne na papierze, aby zobaczyć, jak są ze sobą powiązane. Znak nieskończoności skupia się wokół punktu (0, 0), a dwa pętli ma centra (–150, 0) i (150, 0) i promienie 100. W serii `CubicTo` polecenia, możesz zobaczyć X współrzędne punktów kontrolnych, tworzenia wartości –95 i –205 (te wartości są –150 plus lub minus 55), 205 i 95 (150 plus lub minus 55), a także 250 i –250 do prawej i lewej stronie. Jedynym wyjątkiem jest, gdy znak nieskończoności przecina w Centrum. W takim przypadku punkty kontrolne mają współrzędnych przy użyciu kombinacji 50 i – 50, porządkowanie krzywej w pobliżu środka.

Oto logowania nieskończoność na wszystkich trzech platformach:

[![](beziers-images/bezierinfinity-small.png "Potrójna zrzut ekranu przedstawiający stronę nieskończoności Beziera")](beziers-images/bezierinfinity-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę nieskończoności Beziera")

Jest nieco gładsze kierunku środka znak nieskończoności renderowany przez **nieskończoności łuk** strony [ **trzy sposoby narysowania łuku** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) artykułu.

## <a name="the-quadratic-bzier-curve"></a>Krzywą Beziera drugiego stopnia

Krzywą Beziera drugiego stopnia ma punkt kontrolny tylko jeden, a krzywej jest definiowany przez trzech punktów: punkt początkowy, punkt kontrolny i punktu końcowego. Równania parametryczne są bardzo podobne do krzywej Beziera trzeciego stopnia, z tą różnicą, że wykładnik najwyższy wynosi 2, dlatego krzywą kwadratową wielomianu:

x(t) = (1 — t) ²x₀ + 2t (1 — t) x₁ + t²x₂

y(t) = (1 — t) ²y₀ + 2t (1 — t) y₁ + t²y₂

Aby dodać krzywą Beziera drugiego stopnia do ścieżki, użyj [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) metody lub [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/System.Single/System.Single/System.Single/System.Single/) przeciążenia z oddzielnym `x` i `y` współrzędnych:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Metody dodać krzywą od aktualnej pozycji, aby `point2` z `point1` jako punkt kontrolny.

Możesz eksperymentować z drugiego stopnia krzywych Beziera z **krzywą kwadratową** strony, która jest bardzo podobny do **krzywą Beziera** strony, ale ma ona punkty dotykowe: tylko trzy. Oto `PaintSurface` obsługi w [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) pliku związanego z kodem:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with quadratic Bezier
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.QuadTo(touchPoints[1].Center,
                    touchPoints[2].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

A tutaj jest uruchomiona na wszystkich trzech platformach:

[![](beziers-images/quadraticcurve-small.png "Potrójna zrzut ekranu przedstawiający stronę krzywą kwadratową")](beziers-images/quadraticcurve-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę krzywą kwadratową")

Linii kropkowanej są tangens na krzywą na punkt początkowy i punkt końcowy, a końców punkt kontrolny.

Beziera drugiego stopnia jest dobre, jeśli potrzebujesz krzywej ogólny kształt, ale wolisz wygodę punkt kontrolny tylko jeden zamiast dwóch. Beziera drugiego stopnia renderuje efektywniej niż wszystkie inne krzywej, dlatego jest ono używane wewnętrznie w Skia do renderowania eliptyczne Łuki.

Kształt krzywą Beziera drugiego stopnia nie jest jednak elipsy, dlatego wiele Béziers drugiego stopnia jest konieczne określenie w przybliżeniu łuk eliptyczny. Beziera drugiego stopnia zamiast tego jest segment parabola.

## <a name="the-conic-bzier-curve"></a>Conic krzywej Beziera

Conic krzywej Beziera &mdash; znany także jako wymierne drugiego stopnia krzywej Beziera &mdash; jest stosunkowo ostatnio dodana do rodziny krzywych Beziera. Podobnie jak krzywą Beziera drugiego stopnia wymierne krzywą Beziera drugiego stopnia obejmuje punkt początkowy punkt końcowy i jeden punkt. Ale wymaga wykonania wymierne krzywą Beziera drugiego stopnia *wagi* wartość. Jest on nazywany *wymierne* drugiego stopnia, ponieważ parametrycznych formuły obejmują wskaźniki.

Równania parametryczne X i Y są wskaźniki, które współużytkują ten sam mianownik. Oto równanie mianownik dla *t* zakresu od 0 do 1 i wartość wagi *w*:

d(t) = (1 — t) ² + 2wt(1 – t) + t²

Teoretycznie wymierne umożliwiającej obliczanie kwadratów może obejmować trzech oddzielnych wag, jednej dla każdego z trzech warunków, ale może być uproszczone do tylko jednej wartości wagi na bliski termin.

Równania parametryczne, dla współrzędnych X i Y są podobne do równania parametryczne Beziera drugiego stopnia, z tą różnicą, że określenie bliski obejmuje również wartość wagi, a wyrażenie jest dzielona przez mianownik:

x(t) = ((1 – t) ²x₀ + 2wt (1 — t) x₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t) ²y₀ + 2wt (1 — t) y₁ + t²y₂)) ÷ d(t)

Wymierne krzywych Beziera drugiego stopnia są również nazywane *conics* ponieważ mogą one reprezentować dokładnie segmentów dowolną sekcję conic &mdash; hiperbole, parabole, wielokropek i koła.

Aby dodać wymierne krzywą Beziera drugiego stopnia do ścieżki, użyj [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) metody lub [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) przeciążenia z oddzielnym `x` i `y` współrzędnych:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

Zwróć uwagę, końcowe `weight` parametru.

**Conic krzywej** strony można eksperymentować z tymi krzywych. `ConicCurvePage` Klasa pochodzi od `InteractivePage`. [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) plik `Slider` zaznacz wartość wagi z zakresu od -2 i 2. [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) pliku związanego z kodem tworzy trzy `TouchPoint` obiektów, a `PaintSurface` obsługi po prostu renderuje wynikowe krzywej przy użyciu funkcji tangens wierszy do kontrolki punkty:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with conic curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.ConicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     (float)weightSlider.Value);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

W tym miejscu jest uruchomiona na wszystkich trzech platformach:

[![](beziers-images/coniccurve-small.png "Potrójna zrzut ekranu przedstawiający stronę Conic krzywej")](beziers-images/coniccurve-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Conic krzywej")

Jak widać, punkt kontrolny wydaje się, aby ściągnąć krzywej kierunku więcej, kiedy waga jest wyższy. Waga wynosi zero, krzywej staje się linię prostą rysowaną od punktu początkowego do punktu końcowego.

Teoretycznie wagi ujemne są dozwolone i spowodować krzywej do zakrzywia *natychmiast* z punkt kontrolny. Jednak przeprowadzi -1 lub pod Przyczyna mianownik w równania parametryczne do ujemna określonej wartości *t*. Prawdopodobnie z tego powodu wagi ujemne są ignorowane w `ConicTo` metody. **Conic krzywej** program pozwala ustawić ujemnych wagi, ale jak widać, eksperymentując ujemna wagi mają taki sam skutek jak wagę zero i prostej do renderowania.

Jest bardzo proste do wyprowadzenia punkt kontrolny i waga do użycia `ConicTo` metodę, aby narysować łuku okręgu do (z wyjątkiem) promieniu. Na poniższym diagramie styczne od początkowego i punktu końcowego spełnia na punkt kontrolny.

![](beziers-images/conicarc.png "Renderowanie conic łuk łuk okręgu")

Trygonometryczne można użyć, aby określić odległość punkt kontrolny z Centrum koła: to promień koła podzielona przez cosinus kąta połowa α. Aby narysować łuku okręgu między początkowego i punktu końcowego, należy ustawić wagę tego cosinus tej samej połowa kąta. Zwróć uwagę, następnie, że jeśli kąt jest 180 stopni, nigdy nie spełniają styczne i wagi wynosi zero. Jednak dla kąty mniejsza niż 180 stopni, obliczenia, działa prawidłowo.

**Conic łuku okręgu** strona przedstawia to. [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) plik `Slider` służąca do wybierania kąta. `PaintSurface` Obsługi w [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) oblicza pliku związanego z kodem, punkt kontrolny i wagę:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 4;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the points and weight
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point1 = new SKPoint(0, radius / cos);
    SKPoint point2 = new SKPoint(radius * sin, radius * cos);
    float weight = cos;

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point2.X, point2.Y, point1.X, point1.Y, dottedStroke);

    // Draw the conic
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.ConicTo(point1, point2, weight);
        canvas.DrawPath(path, redStroke);
    }
}
```

Jak widać, nie ma visual różnic między `ConicTo` ścieżki, pokazane na czerwono i podstawowych koła wyświetlane odwołania:

[![](beziers-images/coniccirculararc-small.png "Potrójna zrzut ekranu przedstawiający stronę Conic łuku okręgu")](beziers-images/coniccirculararc-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Conic łuk okręgu")

Ale Ustaw kąt 180 stopni i matematykę kończyć się niepowodzeniem.

Niefortunne jest w tym przypadku, `ConicTo` nie obsługuje ujemnych wagi, ponieważ teoretycznie (oparte na równania parametryczne) można wykonać koło z innym wywołaniu `ConicTo` przy użyciu tych samych punktów, ale wartość ujemną wagi. To umożliwia utworzenie całego okrąg tylko dwa `ConicTo` krzywych oparte na każdego kąta między (z wyjątkiem) zero stopnie oraz 180 stopni.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
