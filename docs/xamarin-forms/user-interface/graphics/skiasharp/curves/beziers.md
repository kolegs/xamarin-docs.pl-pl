---
title: Trzy typy krzywych Beziera
description: W tym artykule wyjaśniono, jak używać SkiaSharp do renderowania sześcienny kwadratową i conic krzywych Beziera w aplikacji platformy Xamarin.Forms i pokazuje to z przykładowym kodzie.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: charlespetzold
ms.author: chape
ms.date: 05/25/2017
ms.openlocfilehash: 4a1b86035f9ce31b6e9fafac06cd0090a516b542
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244009"
---
# <a name="three-types-of-bzier-curves"></a>Trzy typy krzywych Beziera

_Eksploruj sposób użycia SkiaSharp do renderowania sześcienny kwadratową i conic krzywych Beziera_

Po Pierre Beziera (1910 — 1999), francuskim inżynierem motoryzacyjnych firmy Renault, przy krzywej dotyczące projektowania przy pomocy komputera organów samochodu nosi nazwę krzywej Beziera.

Krzywe Beziera wiadomo, jest odpowiednie do interaktywnego projektowania: znajdują się również behaved &mdash; innymi słowy, nie ma singularities powodujących krzywej do nieskończonej lub niewygodna &mdash; i zwykle aesthetically czytelnych . Zawiera znak czcionek oparte na komputerze są zazwyczaj definiowane przy użyciu krzywych Beziera:

![](beziers-images/beziersample.png "Przykładowe krzywej Beziera")

Artykuł Wikipedia na [krzywej Beziera](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) zawiera niektóre przydatne informacje. Termin *krzywej Beziera* faktycznie odwołuje się do rodziny krzywych podobne. SkiaSharp obsługuje trzy rodzaje krzywych Beziera, nazywanych *sześcienny*, *kwadratową*i *conic*. Conic jest także znana jako *wymierna kwadratowe*.

## <a name="the-cubic-bzier-curve"></a>Trzeciego krzywej Beziera

Cubic jest typem krzywej Beziera większość deweloperów traktować gdy podmiot krzywych Beziera.

Można dodać krzywą trzeciego stopnia Beziera do `SKPath` przy użyciu [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) metody z trzema `SKPoint` parametry, lub [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/System.Single/) przeciążenia z oddzielnym `x` i `y` parametry:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Krzywej rozpoczyna się od bieżącego punktu rozkładu. Zakończenie sześcienny krzywą Beziera jest zdefiniowane przez cztery punkty:

- Uruchom punkt: bieżącego punktu w rozkład, lub (0, 0), jeśli `MoveTo` nie została wywołana.
- pierwszy punkt kontrolny: `point1` w `CubicTo` wywołania
- drugi punkt kontrolny: `point2` w `CubicTo` wywołania
- punkt końcowy: `point3` w `CubicTo` wywołania

Wynikowe krzywej zaczyna się od punktu początkowego i kończy się w punkcie końcowym. Krzywej generalnie nie przechodzi przez punkty kontrolne dwóch; Zamiast tego funkcjonują dużo pól podobnego do ściągnięcia krzywej do nich.

Aby uzyskać pewne pojęcie sześcienny krzywej Beziera najlepiej przez eksperymenty. To jest celem **krzywej Beziera** strony, która jest pochodną `InteractivePage`. [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) tworzy plik `SKCanvasView` i `TouchEffect`. [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) plik CodeBehind tworzy cztery `TouchPoint` obiektów w jego konstruktora. `PaintSurface` Tworzy program obsługi zdarzeń `SKPath` do renderowania krzywej Beziera, oparte na czterech `TouchPoint` obiektów, a także pobiera kropkowanej stycznej wiersze z punktów kontrolnych do punktów końcowych:

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

W tym miejscu jest uruchomiona na wszystkich platformach trzy:

[![](beziers-images/beziercurve-small.png "Potrójna zrzut ekranu przedstawiający stronę krzywej Beziera")](beziers-images/beziercurve-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę krzywej Beziera")

Matematycznie krzywa jest wielomianu trzeciego stopnia. Krzywej co najwyżej przecina prostej na trzy punkty. W momencie rozpoczęcia krzywa jest zawsze stycznej, a w tym samym kierunku co prostej od początku wskaż pierwszy punkt kontrolny. W punkcie końcowym krzywa jest zawsze stycznej, a w tym samym kierunku co polecenie prostej z drugiego punktu końcowego.

Trzeciego krzywej Beziera zawsze jest ograniczone przez jest Czworokąt wypukłych łączenie cztery punkty. Ta metoda jest wywoływana *wypukłych powłoki*. Jeśli punkty kontrolne znajduje się na prostej między początkowy i punkt końcowy, krzywej Beziera renderuje jako prostej. Ale krzywej mogą również przechodzić, jako trzeci zrzut ekranu przedstawia.

Rozkład ścieżki może zawierać wiele połączonych sześcienny krzywych Beziera, ale połączenie między dwoma sześcienny krzywych Beziera będzie smooth, tylko wtedy, gdy colinear następujące trzy punkty (czyli znajduje się na prostej):

- drugi punkt kontrolny krzywej pierwszy
- punkt końcowy pierwszego krzywej, która jest również punkt początkowy krzywej drugi
- pierwszy punkt kontrolny krzywej drugi

W artykule na [ **danych ścieżki SVG** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) dowiesz się możliwość jej obsługi ułatwiają definicji smooth połączonych krzywych Beziera.

Czasami jest to użyteczne informacje podstawowej równania parametryczne, które renderowania sześcienny krzywej Beziera. Aby uzyskać *t* od 0 do 1, parametrycznych równania są następujące:

x(t) = (1 – t) ³x₀ + 3t (1 – t) ²x₁ + 3t² (1 – t) x₂ + t³x₃

y(t) = (1 – t) ³y₀ + 3t (1 – t) ²y₁ + 3t² (1 – t) y₂ + t³y₃

Wykładnik najwyższy 3 potwierdza, że są one polynomials trzeciego stopnia. Łatwo sprawdzić, że w przypadku `t` jest równe 0, punkt znajduje się (x₀ y₀), która jest punkt początkowy i kiedy `t` jest równa 1, punkt znajduje się (x₃ y₃), który jest punktem końcowym. Pobliżu punkt początkowy (dla wartości niski `t`), pierwszy punkt kontrolny (x₁, y₁) ma silne efektu, a punkt końcowy w pobliżu (wysokiej wartości 't') drugi punkt kontrolny (x₂, y₂) ma wpływ silne.

## <a name="bzier-curve-approximation-to-circular-arcs"></a>Zbliżenia krzywej Beziera na łuki okręgu

Czasami jest to łatwe w użyciu krzywej Beziera na łuk okręgu renderowania. Sześcienny krzywej Beziera można przybliżona łuku okręgu bardzo dobrze maksymalnie koło kwartału tak cztery połączonych krzywych Beziera można zdefiniować całego okręgu. Zbliżenia opisanej w artykułach dwóch opublikowane ponad 25 lat temu:

> Tor Dokken, i inni "Dobrej zbliżenia okręgi krzywych Beziera łuku ciągłego" *komputera wspomagane geometrycznych Projekt 7* (1990), 33 41.

> Jan Goldapp, "Zbliżenia Łuki okręgu przez sześcienny Polynomials" *komputera wspomagane geometrycznych Projekt 8* (1991), 227 238.

Na poniższym diagramie przedstawiono cztery punkty etykietą `pto`, `pt1`, `pt2`, i `pt3` Definiowanie krzywej Beziera (zaznaczone na czerwono), który jest zbliżony łuku okręgu:

![](beziers-images/bezierarc45.png "Zbliżenia łuku okręgu z krzywej Beziera")

Wiersze z punktu początkowego i końcowego do punktów kontrolnych są tangens okręgu i krzywej Beziera i mają długość *L*. Pierwszego artykułu wymienionych powyżej wskazuje, że najważniejsze krzywej Beziera przybliża łuku okręgu podczas tej długości *L* jest obliczane w następujący sposób:

L = 4 x tan(α / 4) / 3

Na ilustracji przedstawiono pod kątem 45 stopni, dlatego L równa 0.265. W kodzie ta wartość będzie mnożona przez żądaną promień okręgu.

**Łuku okręgu krzywej Beziera** strony można wypróbować Definiowanie krzywej Beziera na łuk okręgu kątów zakresu do 180 stopni w przybliżeniu. [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) tworzy plik `SKCanvasView` i `Slider` wybierania kąta. `PaintSurface` Obsługi zdarzeń w [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) plik CodeBehind używa transformacji ustawioną na środku kanwy punkt (0, 0). Rysowanie okręgu skupia się na prowadzące do porównania, a następnie oblicza punkty kontrolny krzywej Beziera:

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

Punkt początkowy i końcowy (`point0` i `point3`) są obliczane w zależności od normalnego równania parametrycznych okręgu. Ponieważ okręgu skupia się na (0, 0), punkty te mogą także traktowane jako wektory promieniowego od środka okręgu do obwodu. Punkty kontrolne są w wierszach stycznych kółko, dzięki czemu są one prostopadle do tych promieniowego wektory. Wektor pod kątem prostym jest po prostu oryginalnego wektora z współrzędne X i Y miejscami i jeden z nich wprowadzone ujemna.

Oto działająca na platformach trzy, z trzema różnymi kątami program:

[![](beziers-images/beziercirculararc-small.png "Potrójna zrzut ekranu przedstawiający stronę łuku okręgu krzywej Beziera")](beziers-images/beziercirculararc-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę łuku okręgu krzywej Beziera")

Należy dokładnie przejrzeć trzeci zrzut ekranu i zobaczysz, że krzywej Beziera będzie szczególnie odbiega od Półkolista, gdy kąt jest 180 stopni, ale ekranu dla systemu iOS pokazuje, że prawdopodobnie dopasowania koło kwartału tylko drobne kąt wynosi 90 stopni.

Obliczanie współrzędne punktów kontrolnych dwóch jest dość łatwe, gdy kółko kwartału jest orientacji w następujący sposób:

![](beziers-images/bezierarc90.png "Zbliżenia kwartału koło z krzywej Beziera")

Jeśli promień okręgu wynosi 100, następnie *L* jest 55 i która jest liczbą łatwe do zapamiętania.

**Squaring okręgu** cyfrę koło kwadrat animuje strony. Okręgu jest w przybliżeniu cztery krzywych Beziera których współrzędne są wyświetlane w pierwszej kolumnie tej definicji tablicy w [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) klasy:

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

Druga kolumna zawiera współrzędne cztery krzywych Beziera definiujące kwadratowy, w których obszar około jest taka sama jak obszaru okręgu. (Rysowania kwadrat z *dokładnie* obszar jako danego koło jest klasycznego przesyłał geometrycznych problemu [squaring okręgu](https://en.wikipedia.org/wiki/Squaring_the_circle).) Do renderowania kwadrat za pomocą krzywych Beziera, punktów kontrolnych dwóch dla każdej krzywej są takie same, i są colinear z punktu początkowego i końcowego, więc krzywej Beziera jest renderowane jako prostej.

Trzecia kolumna tablicy jest interpolowane wartości dla animacji. Strona ustawia czasomierza dla 16 MS i `PaintSurface` obsługi jest wywołana w tym szybkość:

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

Punkty są interpolowane na podstawie wartości sinusoidally OSCYLUJĄCA `t`. Interpolowane punkty są następnie używane do utworzenia serii cztery połączonych krzywych Beziera. Oto animacji działająca na platformach trzy, wskaźnik postępu z okręgu do kwadratu:

[![](beziers-images/squaringthecircle-small.png "Potrójna zrzut ekranu przedstawiający Squaring strony koło")](beziers-images/squaringthecircle-large.png#lightbox "Potrójna zrzut ekranu przedstawiający Squaring strony okręgu")

Takie animacji byłoby możliwe bez krzywych algorithmically wystarczająco elastyczny, aby być renderowane jako zarówno proste, jak i łuki okręgu.

**Nieskończoności Beziera** strony korzysta również możliwość przybliżona łuku okręgu krzywej Beziera. Oto `PaintSurface` programu obsługi [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) klasy:

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

Może to być dobry wykonywania do wykreślenia tych współrzędnych na papier, aby zobaczyć, w jaki sposób są powiązane. Znak nieskończoności skupia się na punkt (0, 0), a dwie pętle ma środkami (–150, 0) i (150, 0) i promienie 100. W serii `CubicTo` polecenia, można zobaczyć X współrzędne punktów kontrolnych wykonywania na wartości –95 i –205 (te wartości są –150 plus lub minus 55), 205 i 95 (150 plus lub minus 55), a także 250 i –250 do prawej i lewej stronie. Jedynym wyjątkiem jest podczas logowania nieskończoności przecina w Centrum. W takim przypadku punkty kontrolne mają współrzędne z połączeniem 50 i – 50, porządkowanie krzywej w środku.

W tym miejscu jest znak nieskończoności na wszystkich platformach trzy:

[![](beziers-images/bezierinfinity-small.png "Potrójna zrzut ekranu przedstawiający stronę nieskończoności Beziera")](beziers-images/bezierinfinity-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę nieskończoności Beziera")

Jest nieco łatwiejsze do Centrum niż znak nieskończoności renderowana przez **nieskończoności łuk** strony z [ **trzech sposobów, aby narysować łuk** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) artykułu.

## <a name="the-quadratic-bzier-curve"></a>Kwadratową krzywej Beziera

Kwadratową krzywej Beziera ma punkt kontrolny tylko jeden i krzywej jest definiowana za pomocą trzech punktów: punkt początkowy, punkt kontrolny i punktu końcowego. Parametrycznych równania są bardzo podobne do trzeciego krzywej Beziera, z wyjątkiem tego, że najwyższy wykładnik jest 2, tak aby krzywą kwadratową wielomianu:

x(t) = (1 – t) ²x₀ + 2t (1 – t) x₁ + t²x₂

y(t) = (1 – t) ²y₀ + 2t (1 – t) y₁ + t²y₂

Aby dodać krzywą kwadratową Beziera do ścieżki, użyj [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) metody lub [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/System.Single/System.Single/System.Single/System.Single/) przeciążenia z oddzielnym `x` i `y` współrzędne:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Metody dodać krzywą z bieżącej pozycji do `point2` z `point1` jako punkt kontrolny.

Możesz eksperymentować z kwadratową krzywych Beziera z **było dodać krzywą kwadratową** strony, która jest bardzo podobny do **krzywej Beziera** strony, lecz ma ona tylko trzy punkty touch. Oto `PaintSurface` obsługi w [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) pliku CodeBehind:

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

I w tym miejscu jest uruchomiona na wszystkich platformach trzy:

[![](beziers-images/quadraticcurve-small.png "Potrójna zrzut ekranu przedstawiający stronę było dodać krzywą kwadratową")](beziers-images/quadraticcurve-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę było dodać krzywą kwadratową")

Wiersze przerywana tangens na krzywą na punkt początkowy i punkt końcowy oraz że spełniają na punkt kontrolny.

Kwadratową Beziera jest prawidłowa, jeśli potrzebujesz krzywą ogólny kształt, ale wolisz wygodę punkt kontrolny tylko jeden zamiast dwóch. Kwadratową Beziera renderuje wydajniej żadnych innych krzywej, dlatego ona jest używana wewnętrznie w Skia do renderowania eliptycznej łuków.

Jednak kształt kwadratową krzywej Beziera nie jest eliptycznej, dlatego wiele Béziers kwadratową są wymagane do przybliżonego określenia łuku. Kwadratową Beziera zamiast tego jest segmentem parabola.

## <a name="the-conic-bzier-curve"></a>Conic krzywej Beziera

Conic krzywej Beziera &mdash; znanej także jako wymierna kwadratową krzywej Beziera &mdash; jest stosunkowo niedawne dodanie do rodziny krzywych Beziera. Podobnie jak kwadratową krzywej Beziera wymierna kwadratową krzywej Beziera obejmuje punkt początkowy punkt końcowy i punkt kontrolny jeden. Ale wymaga wymierna kwadratową krzywej Beziera *wagi* wartość. Jest to *wymierna* kwadratową, ponieważ parametrycznych formuły wymagają wskaźników.

Parametrycznych równania X i Y to współczynnik, które współużytkują tego samego denominator. Oto równanie denominator dla *t* zakresu od 0 do 1 i wartość wagi *w*:

d(t) = (1 – t) ² + 2wt(1 – t) + t²

Teoretycznie wymierna kwadratowe może obejmować trzech oddzielnych wag, po jednej dla każdego z trzech warunków, ale może zostać uproszczona do tylko jednej wartości wagi na bliski termin.

Parametrycznych równania współrzędne X i Y są podobne do wskaźnika równania kwadratową Beziera, z tą różnicą, że określenie bliski obejmuje również wartość wagi, a wyrażenie jest podzielona przez dzielnik:

x(t) = ((1 – t) ²x₀ + 2wt (1 – t) x₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t) ²y₀ + 2wt (1 – t) y₁ + t²y₂)) ÷ d(t)

Wymierna kwadratową krzywych Beziera są również nazywane *conics* ponieważ dokładnie reprezentują segmentów dowolnej sekcji conic &mdash; hiperbole, parabole elipsy i kółka.

Aby dodać krzywą kwadratową Beziera wymierna do ścieżki, użyj [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) metody lub [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) przeciążenia z oddzielnym `x` i `y` współrzędne:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

Zwróć uwagę, ostatecznych `weight` parametru.

**Conic krzywej** strony można wypróbować te krzywych. `ConicCurvePage` Pochodną klasy `InteractivePage`. [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) tworzy plik `Slider` wybierz wartość wagi z zakresu od -2 i 2. [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) plik CodeBehind tworzy trzy `TouchPoint` obiekty i `PaintSurface` obsługi po prostu renderuje wynikowe krzywej liniami stycznej do formantu punkty:

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

W tym miejscu jest uruchomiona na wszystkich platformach trzy:

[![](beziers-images/coniccurve-small.png "Potrójna zrzut ekranu przedstawiający stronę krzywej Conic")](beziers-images/coniccurve-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Conic krzywej")

Jak widać, punkt kontrolny jest prawdopodobnie ściągnięcia krzywej kierunku więcej, kiedy waga jest wyższy. Waga wynosi zero, krzywej staje się linii prostej punkt początkowy do punktu końcowego.

Teoretycznie są dozwolone wag ujemna i spowodować krzywej zakrzywia *optymalizacji* z punkt kontrolny. Jednak przeprowadzi -1 lub poniżej Przyczyna denominator parametrycznych dolne do ujemna dla określonej wartości *t*. Prawdopodobnie z tego powodu wagi ujemna są ignorowane w `ConicTo` metody. **Conic krzywej** program umożliwia ustawienie wagi ujemna, ale jak widać Eksperymentując masy ujemnej mają ten sam efekt co wagę zero i prostej do renderowania.

Jest bardzo proste pochodzić punkt kontrolny i wagi do użycia `ConicTo` metodę, by narysować łuku okręgu do (z wyjątkiem) Półkolista. Na poniższym diagramie stycznej wiersze z punktu początkowego i końcowego spełnia na punkt kontrolny.

![](beziers-images/conicarc.png "Renderowanie conic łuk łuk okręgu")

Można określić odległość punkt kontrolny od środka okręgu trygonometryczne: jest radius rozdzielonych cosinus kąta połowa α okręgu. Rysowanie łuku okręgu między początkową i punktów końcowych, należy ustawić wagę do tego samego cosinus połowa kąta. Należy zauważyć, że jeśli kąt jest 180 stopni, w następnie stycznej wierszy nigdy nie spełnia a waga wynosi zero. Jednak dla kątów mniej niż 180 stopni, obliczenia działa prawidłowo.

**Conic łuku okręgu** strony pokazano to. [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) tworzy plik `Slider` wybierania kąta. `PaintSurface` Obsługi w [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) pliku CodeBehind oblicza punkt kontrolny i wagi:

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

Jak widać, nie ma żadnej visual różnicy między `ConicTo` ścieżka zaznaczone na czerwono i podstawowej okręgu wyświetlane dla odwołania:

[![](beziers-images/coniccirculararc-small.png "Potrójna zrzut ekranu przedstawiający stronę Conic łuku okręgu")](beziers-images/coniccirculararc-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Conic łuk okręgu")

Ale kąt 180 stopni i matematyce kończyć się niepowodzeniem.

Niefortunne jest w tym przypadku który `ConicTo` nie obsługuje ujemna wag, ponieważ teoretycznie (oparte na parametrycznych równania) można wykonać z innym wywołaniu okręgu `ConicTo` z tego samego punkty, ale wartość ujemną wagi. Dzięki temu tworzenie całych koło z tylko dwa `ConicTo` krzywych oparte na dowolny kąt między (z wyjątkiem) zero stopni do 180 stopni.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
