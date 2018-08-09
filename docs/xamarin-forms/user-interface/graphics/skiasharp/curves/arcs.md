---
title: Trzy sposoby narysowania łuku
description: W tym artykule wyjaśniono, jak zdefiniować Łuki na trzy różne sposoby przy użyciu SkiaSharp i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: charlespetzold
ms.author: chape
ms.date: 05/10/2017
ms.openlocfilehash: e862a663b35124c1470ae5239c93409c298b19ba
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615408"
---
# <a name="three-ways-to-draw-an-arc"></a>Trzy sposoby narysowania łuku

_Dowiedz się, jak zdefiniować Łuki na trzy różne sposoby przy użyciu SkiaSharp_

Łuk jest krzywej w obwodzie elipsy, takie jak zaokrąglone części znaku nieskończoności:

![](arcs-images/arcsample.png "Logowanie w nieskończoność")

Pomimo uproszczenia tej definicji nie istnieje sposób, aby zdefiniować funkcję łuk rysunku, który spełnia wszystkie potrzeby i w związku z tym, nie zgody między systemami grafiki w najlepszym sposobem narysowania łuku. Z tego powodu `SKPath` klasy nie ogranicza się do tylko jednej metody.

`SKPath` definiuje `AddArc` metody, pięć różnych `ArcTo` metody i dwiema względny `RArcTo` metody. Metody te można podzielić na trzy kategorie, reprezentujący trzech bardzo różne podejścia do określania łuku. Używane, w którym zależy od informacji dostępnych do definiowania łuk i jak ten łuk pracy i wpasowania grafiki, która rysowania.

## <a name="the-angle-arc"></a>Kąt łuk

Kąt łuk podejście do rysowania Łuki wymaga określenia prostokąt, który granic elipsę. Łuk na obwód to wielokropka jest wskazywany przez kąty z Centrum elipsy, dzięki czemu początku łuk i jego długość. Dwie różne metody narysuj Łuki kąta. Są to [ `AddArc` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) metody i [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKRect/System.Single/System.Single/System.Boolean/) metody:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Te metody są takie same jak Android [ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/) i [ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/) metody. IOS [ `AddArc` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArc/p/System.Boolean/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) metody są podobne, ale jest ograniczony do Łuki na obwód koła zamiast uogólniony, aby elipsę.

Obie metody zaczynają się od `SKRect` wartość, która definiuje lokalizacji i rozmiaru elipsy:

![](arcs-images/anglearcoval.png "Elipsa, zaczynającą łuk kąt")

Łuk jest częścią obwód to elipsy.

`startAngle` Argument jest prawo kąt w stopniach względem linii poziomej rysowany z Centrum wielokropka z prawej strony. `sweepAngle` Argument jest względem `startAngle`. Poniżej przedstawiono `startAngle` i `sweepAngle` wartości 60-100 stopni, odpowiednio:

![](arcs-images/anglearcangles.png "Kąty, które definiują łuk kąt")

Pod kątem uruchamiania rozpocznie się łuku. Jego długość jest regulowane przez kąta odchylenia:

![](arcs-images/anglearchighlight.png "Kąt wyróżnione łuk")

Krzywa dodawany do ścieżki z `AddArc` lub `ArcTo` metodą jest po prostu część obwód elipsy, w tym miejscu wyświetlane na czerwono:

![](arcs-images/anglearc.png "Kąt łuk samodzielnie")

`startAngle` Lub `sweepAngle` argumentów może być ujemna: łuk jest do ruchu wskazówek zegara dla wartości dodatnich z `sweepAngle` i zegara dla wartości ujemnych.

Jednak `AddArc` jest *nie* definiowania zamkniętych ROZKŁAD. Jeśli wywołasz `LineTo` po `AddArc`, linia jest rysowana od końca łuk do punktu w `LineTo` metody i tym samym dotyczy `ArcTo`.

`AddArc` automatycznie uruchamia nowy rozkład i jest funkcjonalnym odpowiednikiem wywołania `ArcTo` z ostatnim argumentem `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Nosi ostatni argument `forceMoveTo`, i skutecznie powoduje `MoveTo` wywołania na początku łuku. Która rozpoczyna się nowej ROZKŁAD. To znaczy nie w przypadku ostatniego argumentu `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Ta wersja `ArcTo` rysuje linię z bieżącego położenia na początku łuku. Oznacza to, że łuk może być gdzieś środku większych ROZKŁAD.

**Łuk kąt** strona pozwala na początek i kąta odchylenia za pomocą dwóch suwaków. Plik XAML są tworzone wystąpienia dwóch `Slider` elementy i `SKCanvasView`. `PaintCanvas` Obsługi w [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) pliku Rysuje elipsę i łuk za pomocą dwóch `SKPaint` obiekty zdefiniowane jako pola:

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

Jak widać, Kąt początkowy i kąta odchylenia może potrwać od wartości ujemnych:

[![](arcs-images/anglearc-small.png "Potrójna zrzut ekranu przedstawiający stronę łuk kąt")](arcs-images/anglearc-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę kąt łuk")

Algorithmically najprościej jest to podejście do generowania łuk, oraz łatwe do wyprowadzenia równania parametryczne, które opisują łuku. Wiedząc, rozmiar i położenie elipsy oraz kąty początkową i odchylenia, początkowy i punkt końcowy, łuku można obliczyć przy użyciu prostego trygonometryczne:

x = oval. MidX + (oval. Szerokość / 2) * cos(angle)

y = oval. MidY + (oval. Wysokość / 2) * sin(angle)

`angle` Wartość jest albo `startAngle` lub `startAngle + sweepAngle`.

Użycie dwóch kąty, aby zdefiniować łuk najlepiej stosować w przypadkach, gdy wiadomo, usługi angular długość przewidzianą do rysowania, na przykład, aby wykresu kołowego łuku. **Rozkładane wykresu kołowego** strona przedstawia to. [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) Klasa używa Wewnętrzna klasa do definiowania niektórych danych metalowych i kolorów:

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

`PaintSurface` Obsługi najpierw przetwarza w pętli elementów do obliczania `totalValues` numer. Od tego jego rozmiar każdego elementu jest określany jako części całości i przekonwertować, kąt:

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

Nowy `SKPath` obiekt jest tworzony dla każdego wycinka koła. Ścieżka składa się z wiersza z Centrum, a następnie `ArcTo` do rysowania łuk, a inny powrót do wyników Centrum z `Close` wywołania. Ten program wyświetla wycinki koła "rozsunięty", przenosząc je wszystkie się z Centrum 50 pikseli. To zadanie wymaga wektora w kierunku punktu środkowego kąta odchylenia każdy wycinek:

[![](arcs-images/explodedpiechart-small.png "Potrójna zrzut ekranu przedstawiający stronę wykres kołowy rozsunięty")](arcs-images/explodedpiechart-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę wykres kołowy rozsunięty")

Aby zobaczyć, jak to wygląda bez "masowego", po prostu komentarz `Translate` wywołania:

[![](arcs-images/explodedpiechartunexploded-small.png "Potrójna zrzut ekranu przedstawiający stronę wykres kołowy rozsunięty bez masowego")](arcs-images/explodedpiechartunexploded-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę wykres kołowy rozsunięty bez masowego")

## <a name="the-tangent-arc"></a>Arcus tangens

Drugi typ łuk obsługiwane przez `SKPath` jest *arcus tangens*, tzw. ponieważ łuk obwód koła stycznej na dwie połączone linie.

Arcus tangens zostanie dodany do ścieżki z wywołaniem [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) metody przy użyciu dwóch `SKPoint` parametrów, lub [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) przeciążenia z oddzielnym `Single` parametrów punkty:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

To `ArcTo` metoda jest podobna do PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) funkcji (strona 532 w dokumencie PDF) i systemu iOS [ `AddArcToPoint` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArcToPoint/p/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) metody.

`ArcTo` Metoda obejmuje trzy punkty:

- Bieżące polecenie sylwetką lub punkt (0, 0), jeśli `MoveTo` nie została wywołana.
- Pierwszy argument punktu `ArcTo` wywołano metodę *rogu punktu*
- Drugi argument punktu `ArcTo`, co jest nazywane *docelowy punkt*:

![](arcs-images/tangentarcthreepoints.png "Trzy punkty, które zaczynają się arcus tangens")

Te trzy punkty zdefiniować dwa połączone linie:

![](arcs-images/tangentarcconnectinglines.png "Linie łączące trzech punktów arcus tangens")

W przypadku trzy punkty colinear &mdash; oznacza to, jeśli znajdują się one w tej samej prostej &mdash; łuk nie będą pobierane.

`ArcTo` Metoda obejmuje także `radius` parametru. Określa promień koła:

![](arcs-images/tangentarccircle.png "Koło arcus tangens")

Arcus tangens to nie jest uogólniona dla elipsę.

Jeśli dwa wiersze spełniają pod kątem dowolnego, ten okrąg można wstawiać między te wiersze, tak, aby stycznej do dwóch wierszach:

![](arcs-images/tangentarctangentcircle.png "Okrąg arcus tangens między dwoma wierszami")

Krzywa, który jest dodawany do rozkładu nie w ogóle albo punktów określonych w `ArcTo` metody. Składa się z linię prostą rysowaną od bieżącego punktu pierwszy punkt stycznej i kończący się w momencie stycznej drugi łuk:

![](arcs-images/tangentarchighlight.png "Wyróżnione arcus tangens między dwoma wierszami")

Poniżej przedstawiono końcowy prostej i arc, który jest dodawany do rozkładu:

![](arcs-images/tangentarc.png "Wyróżnione arcus tangens między dwoma wierszami")

Rozkład może być kontynuowane z drugiego punktu stycznej.

**Arcus tangens** strona pozwala na eksperymentowanie z arcus tangens. Jest to pierwszy kilku stron, które wynikają z [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs), który definiuje kilka przydatną `SKPaint` obiektów i wykonuje `TouchPoint` przetwarzania:

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

`TangentArcPage` Klasa pochodzi od `InteractivePage`. Konstruktor w [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) plik jest odpowiedzialny za tworzenie wystąpień i Inicjowanie `touchPoints` tablicy i ustawienie `baseCanvasView` (w `InteractivePage`) do `SKCanvasView` tworzone wystąpienie obiektu w [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) pliku:

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

`PaintSurface` Program obsługi używa `ArcTo` metodę, aby narysować łuk oparte na punkty dotykowe i `Slider`, ale również algorithmically oblicza koła na podstawie kąt:

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

Oto **arcus tangens** strony uruchomionej na wszystkich trzech platformach:

[![](arcs-images/tangentarc-small.png "Potrójna zrzut ekranu przedstawiający stronę arcus tangens")](arcs-images/tangentarc-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę arcus tangens")

Arcus tangens to idealne rozwiązanie w przypadku tworzenia zaokrąglone narożniki, takich jak prostokąt zaokrąglony. Ponieważ `SKPath` zawiera już `AddRoundedRect` metody **zaokrąglone Heptagon** strony pokazuje sposób użycia `ArcTo` zaokrąglania narożników dwustronna siedmiu wielokąta. (Kod jest uogólniony dla dowolnego wielokąta regularnych).

`PaintSurface` Program obsługi [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) klasy zawiera jeden `for` pętli do obliczania współrzędne siedem wierzchołki heptagon oraz drugiego do obliczania pośrednie siedem boki z tych wierzchołki. Te punkty środkowe są następnie używane do konstruowania ścieżki:

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

W tym miejscu jest uruchomiony na trzech platformach program:

[![](arcs-images/roundedheptagon-small.png "Potrójna zrzut ekranu przedstawiający stronę zaokrąglone Heptagon")](arcs-images/roundedheptagon-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Heptagon zaokrąglone")

## <a name="the-elliptical-arc"></a>Łuk eliptyczny

Łuk eliptyczny zostanie dodany do ścieżki z wywołaniem [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/SkiaSharp.SKPoint/) metodę, która ma dwa `SKPoint` parametrów lub [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/System.Single/System.Single/) przeciążenia z oddzielnych X i Y współrzędnych:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Łuk eliptyczny jest zgodna z [łuk eliptyczny](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) zawarte w skalowalnych grafiki wektorowej (SVG) i platformy uniwersalnej Windows [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) klasy.

Te `ArcTo` metody narysowania łuku między dwoma punktami, co jest bieżącym punkcie sylwetką, oraz ostatni parametr do `ArcTo` — metoda ( `xy` parametr lub szczegółowych `x` i `y` parametry):

![](arcs-images/ellipticalarcpoints.png "Dwa punkty, które zdefiniowane łuk eliptyczny")

Pierwszy parametr punktu `ArcTo` — metoda (`r`, lub `rx` i `ry`) nie jest punktem na wszystkich, ale zamiast tego Określa promień narożników elipsę; poziome i pionowe

![](arcs-images/ellipticalarcellipse.png "Elipsy, który zdefiniowany łuk eliptyczny")

`xAxisRotate` Parametr jest liczbę stopni do ruchu wskazówek zegara, aby obrócić tego elipsy:

![](arcs-images/ellipticalarctiltedellipse.png "Elipsa Wychylny, który zdefiniowany łuk eliptyczny")

Jeśli ta Wychylny elipsy następnie jest ustawione, tak, aby go dotyka dwa punkty, punkty połączone przez dwa różne łuki:

![](arcs-images/ellipticalarcellipse1.png "Pierwszy zestaw eliptyczne Łuki")

Te dwa łuki można odróżnić na dwa sposoby: łuk najważniejsze jest większy niż łuk dolnej i jak łuku od lewej do prawej, najważniejsze łuku zgodnie z ruchem wskazówek podczas dolnej łuku w kierunku.

Istnieje również możliwość dopasowania elipsy między dwoma punktami w inny sposób:

![](arcs-images/ellipticalarcellipse2.png "Drugi zestaw eliptyczne Łuki")

Teraz ma mniejszy łuk u góry, który jest wstawiany zgodnie ze wskazówkami zegara i większych łuk w dolnej części, który jest wstawiany przeciwnie do ruchu wskazówek zegara.

Te dwa punkty połączonymi w związku z tym łuk definicją Wychylny elipsy, łącznie z czterech sposobów:

![](arcs-images/ellipticalarccolors.png "Wszystkie cztery eliptyczne Łuki")

Te cztery Łuki są rozróżniane na podstawie czterech kombinacji [ `SKPathArcSize` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathArcSize/) i [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/) argumentów typu wyliczenia dla `ArcTo` metody:

- czerwony: SKPathArcSize.Large i SKPathDirection.Clockwise
- zielony: SKPathArcSize.Small i SKPathDirection.Clockwise
- niebieski: SKPathArcSize.Small i SKPathDirection.CounterClockwise
- amarantowy: SKPathArcSize.Large i SKPathDirection.CounterClockwise

Jeśli wielokropek Wychylny nie jest wystarczająco duży, aby dopasować między dwoma punktami jest równomiernie skalowana, dopóki nie jest wystarczająco duży. Tylko dwa łuki unikatowy połączyć dwa punkty w takiej sytuacji. Te można rozróżnić za pomocą `SKPathDirection` parametru.

Mimo że takie podejście do definiowania łuk brzmi złożonych na pierwszym encounter, jest tylko podejście, które umożliwia zdefiniowanie łuk o obrócony wielokropek, a często jest to najłatwiejsza metoda gdy konieczny do integracji łuki z innymi częściami ROZKŁAD.

**Łuk eliptyczny** strona pozwala interaktywnie ustawić dwa punkty i rozmiarem i rotacją elipsy. `EllipticalArcPage` Klasa pochodzi od `InteractivePage`i `PaintSurface` obsługi w [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) pliku związanego z kodem rysuje cztery łuki:

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

W tym miejscu jest uruchomiona na trzech platformach:

[![](arcs-images/ellipticalarc-small.png "Potrójna zrzut ekranu przedstawiający stronę łuk eliptyczny")](arcs-images/ellipticalarc-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę łuk eliptyczny")

**Nieskończoności łuk** strona używa łuk eliptyczny do rysowania znakiem nieskończoność. Znak nieskończoności opiera się na dwa okręgi z promienie 100 jednostek rozdzielone 100 jednostek:

![](arcs-images/infinitycircles.png "Dwa okręgi")

Dwa wiersze, które wzajemnie wykraczania poza granice są stycznej do obu okręgów:

![](arcs-images/infinitycircleslines.png "Dwa okręgi z styczne")

Podpisz nieskończoności jest kombinacją części tych kręgów i dwa wiersze. Aby użyć łuk eliptyczny do rysowania logowania nieskończoność, można określić współrzędne, w których stycznej do okręgów dwa wiersze.

Należy utworzyć odpowiednie prostokąt w jeden z okręgów:

![](arcs-images/infinitytriangle.png "Dwa okręgi styczne i osadzone okręgu")

Promień koła jest 100 jednostek i przeciwprostokątnej trójkąta jest 150 jednostek, więc kąt α arcus sinus (odwrotność sinusa), 100 podzielona przez 150 lub 41.8 stopni. Długość bok trójkąta jest 150 razy cosinus 41.8 stopni lub 112, które również mogą być obliczane przez twierdzenie Pitagorasa.

Współrzędne punktu stycznej można następnie obliczyć, korzystając z tych informacji:

x = 112·cos(41.8) = 83

y = 112·sin(41.8) = 75

Cztery punkty stycznej są niezbędne do rysowania znakiem nieskończoności wyśrodkowany w punkcie (0, 0), za pomocą promień koła 100:

![](arcs-images/infinitycoordinates.png "Dwa okręgi z współrzędne i Styczne")

`PaintSurface` Obsługi w [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) klasy umieszcza znak nieskończoność, aby (0, 0) punkt jest umieszczany w środku strony i skaluje się ścieżkę do rozmiaru ekranu:

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

Kod używa `Bounds` właściwość `SKPath` do określania wymiarów sinus nieskończoności skalowania do rozmiaru obszaru roboczego:

[![](arcs-images/arcinfinity-small.png "Potrójna zrzut ekranu przedstawiający stronę nieskończoności łuk")](arcs-images/arcinfinity-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę nieskończoności łuk")

Wynik wydaje się nieco małe, co sugeruje, że `Bounds` właściwość `SKPath` zgłasza rozmiar większy niż ścieżki.

Wewnętrznie Skia przybliża łuk przy użyciu wielu krzywych Beziera drugiego stopnia. Krzywe te (jak zobaczysz w następnej sekcji) zawierają punkty kontrolne, które określają, jak jest rysowana krzywej, ale nie są częścią renderowanych krzywej. `Bounds` Właściwość zawiera te punkty kontrolne.

Aby uzyskać większego rozmiaru, użyj `TightBounds` właściwość, która nie obejmuje punktów kontrolnych. Poniżej przedstawiono program uruchomiony w trybie poziomym i przy użyciu `TightBounds` właściwość, aby uzyskać ścieżką granic:

[![](arcs-images/arcinfinitytightbounds-small.png "Potrójna zrzut ekranu strony nieskończoności łuku z granicami ścisłej")](arcs-images/arcinfinitytightbounds-large.png#lightbox "Potrójna zrzut ekranu strony nieskończoności łuku z granicami ścisłej")

Mimo że połączeń między łuki i proste linie są ze sobą matematycznie smooth, zmiana łuk prostej mogą wydawać się nieco nagłego. Lepsze logowania nieskończoności jest przedstawiona w następnej strony.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
