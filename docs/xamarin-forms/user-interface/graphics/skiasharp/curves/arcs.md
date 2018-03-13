---
title: "Trzy sposoby narysować łuk"
description: "Dowiedz się, jak użyć SkiaSharp, aby zdefiniować Łuki na trzy różne sposoby"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: charlespetzold
ms.author: chape
ms.date: 05/10/2017
ms.openlocfilehash: 390c8f4634ea38ecb93e3f21175db00fef27b8e4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="three-ways-to-draw-an-arc"></a>Trzy sposoby narysować łuk

_Dowiedz się, jak użyć SkiaSharp, aby zdefiniować Łuki na trzy różne sposoby_

Łuk jest krzywej w obwodzie elipsy, takie jak zaokrąglone części znaku nieskończoności:

![](arcs-images/arcsample.png "Infinity logowania")

Pomimo uproszczenia tej definicji, nie istnieje sposób aby zdefiniować funkcję łuk rysunku, która spełnia co należy i w związku z tym nie konsensu między systemami grafiki najlepszy sposób, aby narysować łuk. Z tego powodu `SKPath` klasy nie ogranicza się do tylko jeden z nich.

`SKPath` definiuje `AddArc` metoda, pięciu różnych `ArcTo` metody i względem dwóch `RArcTo` metody. Te metody dzielą się na trzy kategorie, reprezentujący trzech bardzo różne podejścia do określania łuk. Używanego jeden zależy od informacji dostępnych do definiowania łuk i sposobu ten łuk wpasowania grafiki, która rysowania.

## <a name="the-angle-arc"></a>Łuk kąta

Podejście łuk kąt do rysowania Łuki wymaga określenia prostokąt zakresem elipsy. Wskazuje łuk w obwodzie tego elipsy kąty od środka elipsy co początku łuk i jej długość. Dwie różne metody Rysowanie łuków kąta. Są to [ `AddArc` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) — metoda i [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKRect/System.Single/System.Single/System.Boolean/) metody:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Te metody są takie same jak Android [ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/) i [ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/) metody. IOS [ `AddArc` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArc/p/System.Boolean/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) jest podobny, ale jest ograniczony do Łuki w obwodzie koło zamiast uogólnione w celu elipsy.

Obie metody rozpoczynać się od `SKRect` wartość, która definiuje lokalizacji i rozmiaru elipsy:

![](arcs-images/anglearcoval.png "Oval, rozpoczynającego łuk kąta")

Łuk jest częścią obwodu tego elipsy.

`startAngle` Argument jest prawo kąta w stopniach względem linii poziomej pochodzą z Centrum wielokropka z prawej strony. `sweepAngle` Argument jest względem `startAngle`. Poniżej przedstawiono `startAngle` i `sweepAngle` wartości 60 do 100 stopni, odpowiednio:

![](arcs-images/anglearcangles.png "Kąty definiujące łuk kąta")

Łuk rozpoczyna się od Kąt początkowy. Długość jego podlega kąta odchylenia:

![](arcs-images/anglearchighlight.png "Łuk wyróżnione kąta")

Dodawany do ścieżki z krzywej `AddArc` lub `ArcTo` metoda jest po prostu część elipsy obwód, pokazane na czerwono:

![](arcs-images/anglearc.png "Kąt łuk samodzielnie")

`startAngle` Lub `sweepAngle` argumentów może być ujemna: łuk jest prawo do wartości dodatnie `sweepAngle` i zegara dla wartości ujemnych.

Jednak `AddArc` jest *nie* zdefiniuj rozkład zamknięte. Jeśli wywołujesz `LineTo` po `AddArc`, jest rysowana linia od końca łuk do punktu w `LineTo` — metoda i tym samym dotyczy `ArcTo`.

`AddArc` automatycznie uruchamia nowe rozkład i jest funkcjonalnym odpowiednikiem wywołania `ArcTo` z ostatnim argumentem `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Nosi ostatni argument `forceMoveTo`, i efektywnie powoduje `MoveTo` wywołać na początku łuk. Która rozpoczyna się nowych ROZKŁAD. Oznacza to inaczej z ostatnim argumentem `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Ta wersja `ArcTo` rysuje od bieżącej pozycji na początku łuk. Oznacza to, że łuk można gdzieś środku rozkład większy.

**Łuk kąt** strona pozwala określić uruchomienia i odchylenia kąty za pomocą suwaków dwa. Plik XAML tworzy dwa `Slider` elementów i `SKCanvasView`. `PaintCanvas` Obsługi w [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) pliku rysuje zarówno oval, jak i za pomocą dwóch łuk `SKPaint` obiektów zdefiniowanych jako pola:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
    float startAngle = (float)startAngleSlider.Value;
    float sweepAngle = (float)sweepAngleSlider.Value;

    canvas.DrawOval(rect, outlinePaint);

    using (SKPath path = new SKPath())
    {
        path.AddArc(rect, startAngle, sweepAngle);
        canvas.DrawPath(path, arcPaint);
    }
}
```

Jak widać, zarówno Kąt początkowy, jak i kąta odchylenia można wykonywać na wartości ujemnych.

[![](arcs-images/anglearc-small.png "Potrójna zrzut ekranu przedstawiający stronę łuk kąt")](arcs-images/anglearc-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę łuk kąta")

Takie podejście do generowania łuk jest algorithmically najprostszą i ułatwia pochodzi równania parametryczne, które opisują łuk. Znajomość rozmiar i położenie elipsy i kąty rozpoczęcia i odchylenia, punktu początkowego i końcowego łuku oblicza się przy użyciu prostego trygonometryczne:

x = oval. MidX + (oval. Szerokość / 2) * cos(angle)

y = oval. MidY + (oval. Wysokość / 2) * sin(angle)

`angle` Wartość to `startAngle` lub `startAngle + sweepAngle`.

Korzystanie z dwóch kąty do definiowania łuk jest najlepsze w przypadku, gdy znasz kątowego długość przewidzianą do rysowania, na przykład, aby wykresu kołowego łuku. **Rozsunięty wykres kołowy** strony pokazano to. [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) Klasa używana Wewnętrzna klasa do definiowania niektórych danych metalowych i kolory:

```csharp
class ChartData
{
    public ChartData(int value, SKColor color)
    {
        Value = value;
        Color = color;
    }

    public int Value { private set; get; }

    public SKColor Color { private set; get; }
}

ChartData[] chartData =
{
    new ChartData(45, SKColors.Red),
    new ChartData(13, SKColors.Green),
    new ChartData(27, SKColors.Blue),
    new ChartData(19, SKColors.Magenta),
    new ChartData(40, SKColors.Cyan),
    new ChartData(22, SKColors.Brown),
    new ChartData(29, SKColors.Gray)
};

```

`PaintSurface` Obsługi najpierw pętli elementów do obliczenia `totalValues` numer. Od tego go określić rozmiar każdego elementu jako część całkowitej i przekonwertować który kąta:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int totalValues = 0;

    foreach (ChartData item in chartData)
    {
        totalValues += item.Value;
    }

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float explodeOffset = 50;
    float radius = Math.Min(info.Width / 2, info.Height / 2) - 2 * explodeOffset;
    SKRect rect = new SKRect(center.X - radius, center.Y - radius,
                             center.X + radius, center.Y + radius);

    float startAngle = 0;

    foreach (ChartData item in chartData)
    {
        float sweepAngle = 360f * item.Value / totalValues;

        using (SKPath path = new SKPath())
        using (SKPaint fillPaint = new SKPaint())
        using (SKPaint outlinePaint = new SKPaint())
        {
            path.MoveTo(center);
            path.ArcTo(rect, startAngle, sweepAngle, false);
            path.Close();

            fillPaint.Style = SKPaintStyle.Fill;
            fillPaint.Color = item.Color;

            outlinePaint.Style = SKPaintStyle.Stroke;
            outlinePaint.StrokeWidth = 5;
            outlinePaint.Color = SKColors.Black;

            // Calculate "explode" transform
            float angle = startAngle + 0.5f * sweepAngle;
            float x = explodeOffset * (float)Math.Cos(Math.PI * angle / 180);
            float y = explodeOffset * (float)Math.Sin(Math.PI * angle / 180);

            canvas.Save();
            canvas.Translate(x, y);

            // Fill and stroke the path
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, outlinePaint);
            canvas.Restore();
        }

        startAngle += sweepAngle;
    }
}
```

Nowy `SKPath` obiekt jest tworzony dla każdego wycinka koła. Ścieżka zawiera wiersza z Centrum, a następnie `ArcTo` do rysowania łuk, a inny powrót do Centrum wyników z `Close` wywołania. Ten program wyświetla wycinków koła "rozsunięty", przenosząc je wszystkie wylogowanie z Centrum 50 pikseli. To zadanie wymaga wektor w kierunku punkt środkowy kąta odchylenia każdy wycinek:

[![](arcs-images/explodedpiechart-small.png "Potrójna zrzut ekranu przedstawiający stronę Rozsunięty wykres kołowy")](arcs-images/explodedpiechart-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Rozsunięty wykres kołowy")

Aby wyświetlić jego wyglądu bez "masowego", po prostu komentarz `Translate` wywołania:

[![](arcs-images/explodedpiechartunexploded-small.png "Potrójna zrzut ekranu przedstawiający stronę Rozsunięty wykres kołowy bez rozłożenie")](arcs-images/explodedpiechartunexploded-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Rozsunięty wykres kołowy bez masowego")

## <a name="the-tangent-arc"></a>Arcus tangens

Drugi typ łuk obsługiwane przez `SKPath` jest *arcus tangens*, tzw. ponieważ łuk jest obwód koła stycznej na dwa połączone linie.

Arcus tangens zostanie dodany do ścieżki z wywołania [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) metody przy użyciu dwóch `SKPoint` parametry, lub [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) przeciążenia z oddzielnym `Single` parametrów punkty:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

To `ArcTo` metoda jest podobna do PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) — funkcja (strona 532 dokumentu PDF) oraz iOS [ `AddArcToPoint` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArcToPoint/p/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) metody.

`ArcTo` Metoda obejmuje trzy punkty:

- Bieżące polecenie rozkład lub punkt (0, 0), jeśli `MoveTo` nie została wywołana.
- Pierwszy argument punktu `ArcTo` wywołano metodę *rogu punktu*
- Drugi argument punktu `ArcTo`o nazwie *docelowego punktu*:

![](arcs-images/tangentarcthreepoints.png "Trzy punkty rozpoczynające stycznej łuk")

Te trzy punkty definiują dwa połączone linie:

![](arcs-images/tangentarcconnectinglines.png "Linie łączące trzy punkty stycznej łuk")

Jeśli trzy wskazuje, czy colinear & #x 2014; oznacza to, gdy znajdują się one w tej samej prostej & #x 2014; Łuk nie będą pobierane.

`ArcTo` Zawiera również metody `radius` parametru. Określa promień okręgu:

![](arcs-images/tangentarccircle.png "Stycznej łuku okręgu")

Arcus tangens to nie jest uogólniona dla elipsy.

Jeśli dwa wiersze spełniają pod kątem żadnych, że koło można wstawiać między te wiersze tak, aby tangens do obu wierszy:

![](arcs-images/tangentarctangentcircle.png "Stycznej łuku okręgu między dwoma liniami")

Krzywej, który jest dodawany do konturu nie touch albo punktów określonych w `ArcTo` metody. Składa się z prostej od bieżącego punktu pierwszego punktu stycznej i łuk kończy się w drugim punkcie stycznej:

![](arcs-images/tangentarchighlight.png "Wyróżnione łuk stycznej między dwoma liniami")

Poniżej przedstawiono końcowego prostej i łuk, który jest dodawany do konturu:

![](arcs-images/tangentarc.png "Wyróżnione łuk stycznej między dwoma liniami")

Rozkład może być kontynuowane z drugiego punktu stycznej.

**Arcus tangens** strony można wypróbować arcus tangens. Jest to pierwszy kilka stron, które pochodzą z [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/InteractivePage.cs), który definiuje kilka przydatną `SKPaint` obiekty i wykonuje `TouchPoint` przetwarzania:

```csharp
public class InteractivePage : ContentPage
{
    protected SKCanvasView baseCanvasView;
    protected TouchPoint[] touchPoints;

    protected SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3
    };

    protected SKPaint redStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 15
    };

    protected SKPaint dottedStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    };

    protected void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = baseCanvasView.CanvasSize.Width / (float)baseCanvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            baseCanvasView.InvalidateSurface();
        }
    }
}
```

`TangentArcPage` Pochodną klasy `InteractivePage`. Konstruktor w [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) plik jest odpowiedzialny za tworzenie wystąpień i Inicjowanie `touchPoints` tablicy i ustawienie `baseCanvasView` (w `InteractivePage`) do `SKCanvasView` wystąpienie obiektu w [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) pliku:

```csharp
public partial class TangentArcPage : InteractivePage
{
    public TangentArcPage()
    {
        touchPoints = new TouchPoint[3];

        for (int i = 0; i < 3; i++)
        {
            TouchPoint touchPoint = new TouchPoint
            {
                Center = new SKPoint(i == 0 ? 100 : 500,
                                     i != 2 ? 100 : 500)
            };
            touchPoints[i] = touchPoint;
        }

        InitializeComponent();

        baseCanvasView = canvasView;
        radiusSlider.Value = 100;
    }

    void sliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

`PaintSurface` Program obsługi używa `ArcTo` metodę, by narysować łuk oparte na punktach touch i `Slider`, ale również algorithmically oblicza okręgu na podstawie kąt:

```csharp
public partial class TangentArcPage : InteractivePage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw the two lines that meet at an angle
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.LineTo(touchPoints[1].Center);
            path.LineTo(touchPoints[2].Center);
            canvas.DrawPath(path, dottedStrokePaint);
        }

        // Draw the circle that the arc wraps around
        float radius = (float)radiusSlider.Value;

        SKPoint v1 = Normalize(touchPoints[0].Center - touchPoints[1].Center);
        SKPoint v2 = Normalize(touchPoints[2].Center - touchPoints[1].Center);

        double dotProduct = v1.X * v2.X + v1.Y * v2.Y;
        double angleBetween = Math.Acos(dotProduct);
        float hypotenuse = radius / (float)Math.Sin(angleBetween / 2);
        SKPoint vMid = Normalize(new SKPoint((v1.X + v2.X) / 2, (v1.Y + v2.Y) / 2));
        SKPoint center = new SKPoint(touchPoints[1].Center.X + vMid.X * hypotenuse,
                                     touchPoints[1].Center.Y + vMid.Y * hypotenuse);

        canvas.DrawCircle(center.X, center.Y, radius, this.strokePaint);

        // Draw the tangent arc
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.ArcTo(touchPoints[1].Center, touchPoints[2].Center, radius);
            canvas.DrawPath(path, redStrokePaint);
        }

        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
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
}
```

Oto **arcus tangens** strony uruchomionej na wszystkich platformach trzy:

[![](arcs-images/tangentarc-small.png "Potrójna zrzut ekranu przedstawiający stronę arcus tangens")](arcs-images/tangentarc-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę arcus tangens")

Na urządzeniu z systemem Windows Mobile trzy punkty są prawie colinear i łuk jest bardzo mała.

Arcus tangens jest idealny dla tworzenia zaokrąglonymi narożnikami, takich jak zaokrąglony prostokąt. Ponieważ `SKPath` zawiera już `AddRoundedRect` metody **zaokrąglona Heptagon** strony przedstawiają sposób użycia `ArcTo` zaokrąglania narożników dwustronnych siedmiu wielokąta. (Kod uogólniony żadnych regularne wielokąta.)

`PaintSurface` Obsługi [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) klasy zawiera jeden `for` pętli do obliczenia współrzędne siedmiu wierzchołki heptagon oraz drugiego obliczyć pośrednie siedmiu stron z nich wierzchołków. Te punkty środkowe są następnie używane do utworzenia ścieżki:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float cornerRadius = 100;
    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPoint[] vertices = new SKPoint[numVertices];
    SKPoint[] midPoints = new SKPoint[numVertices];

    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    // Coordinates of the midpoints of the sides connecting the vertices
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        int prevVertex = (vertex + numVertices - 1) % numVertices;
        midPoints[vertex] = new SKPoint((vertices[prevVertex].X + vertices[vertex].X) / 2,
                                        (vertices[prevVertex].Y + vertices[vertex].Y) / 2);
    }

    // Create the path
    using (SKPath path = new SKPath())
    {
        // Begin at the first midpoint
        path.MoveTo(midPoints[0]);

        for (int vertex = 0; vertex < numVertices; vertex++)
        {
            SKPoint nextMidPoint = midPoints[(vertex + 1) % numVertices];

            // Draws a line from the current point, and then the arc
            path.ArcTo(vertices[vertex], nextMidPoint, cornerRadius);

            // Connect the arc with the next midpoint
            path.LineTo(nextMidPoint);
        }
        path.Close();

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);
        }
    }
}

```

Oto działająca na platformach trzy program:

[![](arcs-images/roundedheptagon-small.png "Potrójna zrzut ekranu przedstawiający stronę zaokrąglona Heptagon")](arcs-images/roundedheptagon-large.png#lightbox "Potrójna zrzut ekranu strony Heptagon zaokrąglona")

## <a name="the-elliptical-arc"></a>Łuku

Łuku zostanie dodany do ścieżki z wywołania [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/SkiaSharp.SKPoint/) metodę, która ma dwa `SKPoint` parametry, lub [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/System.Single/System.Single/) przeciążenia z oddzielnych X i Y współrzędne:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Łuku jest zgodna z [łuku](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) objętych skalowalne wektor grafiki SVG i platformy uniwersalnej systemu Windows [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) klasy.

Te `ArcTo` metody narysować łuk między dwoma punktami, co stanowi bieżącego punktu konturu, a ostatnim parametrem na `ArcTo` — metoda ( `xy` parametr lub szczegółowych `x` i `y` parametry):

![](arcs-images/ellipticalarcpoints.png "Dwa punkty, które zdefiniowano łuku")

Pierwszy parametr punktu `ArcTo` — metoda (`r`, lub `rx` i `ry`) nie jest punktem w ogóle, ale zamiast tego Określa promień poziome i pionowe elipsy;

![](arcs-images/ellipticalarcellipse.png "Elipsy zdefiniowanego łuku")

`xAxisRotate` Parametr to liczba stopni do ruchu wskazówek zegara Obróć tego elipsy:

![](arcs-images/ellipticalarctiltedellipse.png "Elipsy Wychylny zdefiniowanego łuku")

Jeśli ta Wychylny elipsy następnie znajduje się tak, aby go dotyka dwa punkty, punkty są połączone przez dwa różne łuki:

![](arcs-images/ellipticalarcellipse1.png "Pierwszy zestaw wycinkami elips")

Te dwa łuki można rozróżnić na dwa sposoby: łuk top jest większy niż łuk dolnej i jako łuku od lewej do prawej, top łuku wskazówek zegara podczas dolnej łuku w ruchu wskazówek zegara.

Istnieje również możliwość dopasowania elipsy między dwoma punktami w inny sposób:

![](arcs-images/ellipticalarcellipse2.png "Drugi zestaw wycinkami elips")

Teraz jest mniejszy łuk w górnej części, która jest rysowane w prawo i większy łuk na dole, który ma zostać narysowana przeciwnie do ruchu wskazówek zegara.

Te dwa punkty połączonymi z tego powodu łuk definiowany przez Wychylny elipsy w całości cztery sposoby:

![](arcs-images/ellipticalarccolors.png "Wszystkie cztery wycinkami elips")

Te cztery Łuki można rozróżnić za cztery kombinacje [ `SKPathArcSize` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathArcSize/) i [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/) wyliczenie argumentów typu dla `ArcTo` metody:

- czerwony: SKPathArcSize.Large i SKPathDirection.Clockwise
- zielony: SKPathArcSize.Small i SKPathDirection.Clockwise
- niebieski: SKPathArcSize.Small i SKPathDirection.CounterClockwise
- amarantowy: SKPathArcSize.Large i SKPathDirection.CounterClockwise

Jeśli Wychylny elipsy nie jest wystarczająco duży, aby zmieścić między dwoma punktami, jest on skalowany jednolicie, dopóki nie jest wystarczająco duży. Tylko dwa łuki unikatowy w takim przypadku Połącz dwa punkty. Te można rozróżnić z `SKPathDirection` parametru.

Mimo że takie podejście do definiowania łuk dźwięki złożonych na podczas pierwszego potyczki, jest tylko podejście, które umożliwia zdefiniowanie łuk o obrócony elipsy i jest często najłatwiejszy gdy konieczny do integracji z innymi składnikami rozkład łuków.

**Łuku** strona umożliwia interakcyjne ustawić dwa punkty i rozmiarem i rotacją elipsy. `EllipticalArcPage` Pochodną klasy `InteractivePage`i `PaintSurface` obsługi w [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) pliku CodeBehind rysuje cztery łuków:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        int colorIndex = 0;
        SKPoint ellipseSize = new SKPoint((float)xRadiusSlider.Value,
                                          (float)yRadiusSlider.Value);
        float rotation = (float)rotationSlider.Value;

        foreach (SKPathArcSize arcSize in Enum.GetValues(typeof(SKPathArcSize)))
            foreach (SKPathDirection direction in Enum.GetValues(typeof(SKPathDirection)))
            {
                path.MoveTo(touchPoints[0].Center);
                path.ArcTo(ellipseSize, rotation,
                           arcSize, direction,
                           touchPoints[1].Center);

                strokePaint.Color = colors[colorIndex++];
                canvas.DrawPath(path, strokePaint);
                path.Reset();
            }
    }

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}

```

W tym miejscu jest uruchomiona na trzy platformach:

[![](arcs-images/ellipticalarc-small.png "Potrójna zrzut ekranu przedstawiający stronę łuku")](arcs-images/ellipticalarc-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę łuku")

**Nieskończoności łuk** strona używa łuku do rysowania znak nieskończoności. Znak nieskończoności opiera się na dwóch koła z promień 100 jednostek oddzielone 100 jednostek:

![](arcs-images/infinitycircles.png "Dwa okręgi")

Dwa wiersze przekraczania wzajemnie są stycznej do obu okręgi:

![](arcs-images/infinitycircleslines.png "Dwa okręgi liniami stycznej")

Znak nieskończoności jest kombinacją części tych okręgi i dwa wiersze. Aby użyć łuku do rysowania znak nieskończoności, muszą być określone współrzędne, w których dwa wiersze są stycznej do kółka.

Utworzyć prawy prostokąt w jednym z kółka:

![](arcs-images/infinitytriangle.png "Dwa okręgi z liniami stycznej i osadzone okręgu")

100 jednostek jest promień okręgu i przeciwprostokątnej trójkąta jest 150 jednostki, więc kąt α jest sinus (sinus) podzielony przez 150 lub 41.8 stopni 100. Długość boku trójkąta jest 150 razy cosinus 41.8 stopni lub 112, które również mogą być obliczane przez twierdzenie Pitagorasa.

Współrzędne punktu stycznej można obliczyć za pomocą tych informacji:

x = 112·cos(41.8) = 83

y = 112·sin(41.8) = 75

Cztery punkty stycznej są niezbędne do rysowania znak nieskończoności skupia się na punkt (0, 0) z Promień okrągłego 100:

![](arcs-images/infinitycoordinates.png "Dwa okręgi z liniami stycznej i współrzędnych")

`PaintSurface` Obsługi w [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) klasa pozycje znak nieskończoności, aby (0, 0) punkt znajduje się na środku strony i skaluje ścieżkę do rozmiaru ekranu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.LineTo(83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.CounterClockwise, 83, -75);
        path.LineTo(-83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.Clockwise, -83, -75);
        path.Close();

        // Use path.TightBounds for coordinates without control points
        SKRect pathBounds = path.Bounds;

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / pathBounds.Width,
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

W kodzie użyto `Bounds` właściwość `SKPath` do określania wymiarów sinus nieskończoności skalowania go do rozmiaru obszaru roboczego:

[![](arcs-images/arcinfinity-small.png "Potrójna zrzut ekranu przedstawiający stronę nieskończoności łuk")](arcs-images/arcinfinity-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę nieskończoności łuk")

Wynik wydaje się nieco mały, które sugeruje, że `Bounds` właściwość `SKPath` raportuje o rozmiarze większym niż ścieżki.

Wewnętrznie Skia przybliża łuk przy użyciu wielu kwadratową krzywych Beziera. Krzywe te (jak zobaczysz w następnej sekcji) zawiera punkty kontrolne, regulują sposób rysowania krzywej, które nie są częścią krzywej renderowany. `Bounds` Właściwość zawiera te punkty kontrolne.

Aby uzyskać dopasowanie zwiększenie poziomu, użyj `TightBounds` właściwość, która nie obejmuje punktów kontrolnych. Oto program działa w trybie krajobraz i przy użyciu `TightBounds` właściwości można uzyskać ścieżki granic:

[![](arcs-images/arcinfinitytightbounds-small.png "Potrójna zrzut ekranu strony nieskończoności łuku z granicami ścisłej")](arcs-images/arcinfinitytightbounds-large.png#lightbox "Potrójna zrzut ekranu strony nieskończoności łuku z granicami ścisłej")

Mimo że połączenia między łuki i proste są ze sobą matematycznie smooth, zmiana z łuk na prostej mogą wydawać się nieco niespodziewane. Znak nieskończoności lepiej jest podana w następnej strony.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
