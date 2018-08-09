---
title: Informacje o ścieżce i wyliczenia
description: W tym artykule wyjaśniono, jak uzyskać informacje na temat ścieżek SkiaSharp i wyliczenia zawartości i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 09/12/2017
ms.openlocfilehash: 65c614e9a6eb26bc0d027a4a67bec19b036d0a70
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615278"
---
# <a name="path-information-and-enumeration"></a>Informacje o ścieżce i wyliczenia

_Pobieranie informacji na temat ścieżek i wyliczenia zawartości_

[ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) Klasa definiuje kilka właściwości i metod, które umożliwiają uzyskanie informacji o ścieżce. [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) i [ `TightBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.TightBounds/) właściwości (i powiązanych metod) uzyskać metrical wymiary ścieżki. [ `Contains` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Contains/p/System.Single/System.Single/) Metody umożliwia określenie, czy określony punkt w ścieżce.

Czasami jest to przydatne na potrzeby określania łączna długość wszystkich linii i krzywych, które tworzą ścieżki. To nie jest algorithmically prostym zadaniem, więc cała klasa o nazwie [ `PathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) została poświęcona.

Jest to również czasami przydatne uzyskać wszystkie rysowania operacji i punkty, które tworzą ścieżki. Na początku tej funkcji mogą wydawać się niepotrzebne: Jeśli program został utworzony w ścieżce, program już zna zawartość. Jednak wiesz, że ścieżki mogą być tworzone przez [efekty ścieżek](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) oraz przez Przekształcanie [ciągi na ścieżki](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md). Możesz również uzyskać wszystkie rysowania operacji i punkty, które tworzą te ścieżki. Jedną z możliwości jest zastosowanie transformacji konsolidatorze do wszystkich punktów. Dzięki temu technik, takich jak zawijania tekstu w całym półkulę:

![](information-images/pathenumerationsample.png "Tekst zawinięty na półkuli")

## <a name="getting-the-path-length"></a>Pobieranie długości ścieżki

W artykule [ **ścieżki i tekst** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) pokazaliśmy, jak używać [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metodę, aby narysować ciąg tekstowy, którego punkt odniesienia następuje kurs ścieżki. Ale co zrobić, jeśli chcesz rozmiar tekstu, tak aby dokładnie pasował ścieżkę? Rysowanie tekstu w całym koła, aby uzyskać to proste, ponieważ obwód koła jest łatwy do obliczenia. Ale obwód elipsę lub długość krzywej Beziera nie jest tak proste.

[ `SKPathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) Klasa może pomagać. [Konstruktor](https://developer.xamarin.com/api/constructor/SkiaSharp.SKPathMeasure.SKPathMeasure/p/SkiaSharp.SKPath/System.Boolean/System.Single/) akceptuje `SKPath` argument, a [ `Length` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPathMeasure.Length/) właściwości, co spowoduje wyświetlenie jego długość.

Jest to zaprezentowane w **długość ścieżki** próbki, która jest oparta na **krzywą Beziera** strony. [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) plik pochodzi z `InteractivePage` oraz interfejsem dotykowym:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Curves.PathLengthPage"
                       Title="Path Length">
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</local:InteractivePage>
```

[ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) pliku związanego z kodem umożliwia przeniesienie cztery punkty dotykowe do definiowania punktów końcowych i kontrolowania punktów krzywej Beziera trzeciego stopnia. Trzy pola zdefiniować ciąg tekstowy, `SKPaint` obiektu, a obliczony szerokość tekstu:

```csharp
public partial class PathLengthPage : InteractivePage
{
    const string text = "Compute length of path";

    static SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Black,
        TextSize = 10,
    };

    static readonly float baseTextWidth = textPaint.MeasureText(text);
    ...
}
```

`baseTextWidth` Pole jest szerokość tekstu, na podstawie `TextSize` ustawienie 10.

`PaintSurface` Obsługi rysuje krzywej Beziera i następnie rozmiar tekstu, aby dopasować wzdłuż jego długość pełnej:

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

        // Get path length
        SKPathMeasure pathMeasure = new SKPathMeasure(path, false, 1);

        // Find new text size
        textPaint.TextSize = pathMeasure.Length / baseTextWidth * 10;

        // Draw text on path
        canvas.DrawTextOnPath(text, path, 0, 0, textPaint);
    }
    ...
}
```

`Length` Właściwości nowo utworzonej `SKPathMeasure` obiektu uzyskuje długość ścieżki. To jest dzielona przez `baseTextWidth` wartości (czyli szerokości tekstu, w oparciu o rozmiar tekstu, 10), a następnie jest mnożony przez rozmiar tekstu podstawowego 10. W wyniku powstaje nowy rozmiar tekstu do wyświetlania tekstu w tej ścieżce:

[![](information-images/pathlength-small.png "Potrójna zrzut ekranu przedstawiający stronę długość ścieżki")](information-images/pathlength-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę długość ścieżki")

Ponieważ krzywej Beziera pobiera dłuższy lub krótszy, możesz zobaczyć zmiany rozmiaru tekstu.

## <a name="traversing-the-path"></a>Przechodzenie przez ścieżkę

`SKPathMeasure` można zrobić więcej niż tylko miary długość ścieżki. Dla dowolnej wartości z zakresu między zero a długość ścieżki `SKPathMeasure` obiektu można uzyskać w tym momencie pozycji na ścieżkę i tangens na krzywą ścieżki. Tangens jest dostępna jako wektor w formie `SKPoint` obiektu lub jako rotację hermetyzowane w `SKMatrix` obiektu. Poniżej przedstawiono metody `SKPathMeasure` które podaje te informacje w zróżnicowane i elastyczny sposób:

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

[ `SKPathMeasureMatrixFlags` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasureMatrixFlags/) Są:

- [`GetPosition`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPosition/)
- [`GetTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)
- [`GetPositionAndTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)

**Monocyklem pół-potoku** strony animuje kreska na monocyklem, który wydaje się i z powrotem krzywej Beziera trzeciego stopnia:

[![](information-images/unicyclehalfpipe-small.png "Potrójna zrzut ekranu przedstawiający stronę monocyklem pół-potoku")](information-images/unicyclehalfpipe-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę monocyklem pół-potoku")

`SKPaint` Obiekt używany do stykają połowa kreski pionowej i monocyklem jest zdefiniowana jako pole w [ `UnicycleHalfPipePage` ]() klasy. Jest również definiowany `SKPath` obiekt monocyklem:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };

    SKPath unicyclePath = SKPath.ParseSvgPathData(
        "M 0 0" +
        "A 25 25 0 0 0 0 -50" +
        "A 25 25 0 0 0 0 0 Z" +
        "M 0 -25 L 0 -100" +
        "A 15 15 0 0 0 0 -130" +
        "A 15 15 0 0 0 0 -100 Z" +
        "M -25 -85 L 25 -85");
    ...
}
```

Klasa zawiera standardowe zastąpień `OnAppearing` i `OnDisappearing` metody dla animacji. `PaintSurface` Obsługi tworzy ścieżki dla potoku połowie, a następnie pobiera ona. `SKPathMeasure` Następnie obiekt zostanie utworzony w oparciu o tę ścieżkę:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath pipePath = new SKPath())
        {
            pipePath.MoveTo(50, 50);
            pipePath.CubicTo(0, 1.25f * info.Height,
                             info.Width - 0, 1.25f * info.Height,
                             info.Width - 50, 50);

            canvas.DrawPath(pipePath, strokePaint);

            using (SKPathMeasure pathMeasure = new SKPathMeasure(pipePath))
            {
                float length = pathMeasure.Length;

                // Animate t from 0 to 1 every three seconds
                TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
                float t = (float)(timeSpan.TotalSeconds % 5 / 5);

                // t from 0 to 1 to 0 but slower at beginning and end
                t = (float)((1 - Math.Cos(t * 2 * Math.PI)) / 2);

                SKMatrix matrix;
                pathMeasure.GetMatrix(t * length, out matrix,
                                      SKPathMeasureMatrixFlags.GetPositionAndTangent);

                canvas.SetMatrix(matrix);
                canvas.DrawPath(unicyclePath, strokePaint);
            }
        }
    }
}
```

`PaintSurface` Obsługi oblicza wartość `t` skojarzoną z zakresu od 0 do 1 co pięć sekund. Następnie używa `Math.Cos` funkcji konwersji, wartość `t` , zakresów adresów z zakresu od 0 do 1 i powrót do 0, gdzie 0 odpowiada monocyklem na początku po lewej stronie, gdy 1 odpowiada monocyklem w prawym górnym rogu. Funkcja cosinus powoduje, że szybkość najwolniejsze w górnej części potoku i najszybszy u dołu.

Należy zauważyć, że ta wartość `t` musi być pomnożona przez długość ścieżki dla pierwszego argumentu `GetMatrix`. Macierz jest następnie stosowane do `SKCanvas` obiektu ścieżki monocyklem rysowania.

## <a name="enumerating-the-path"></a>Wyliczanie ścieżki

Osadzone dwóch klas `SKPath` umożliwiają wyliczanie zawartości ścieżki. Te klasy są [ `SKPath.Iterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+Iterator/) i [ `SKPath.RawIterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+RawIterator/). Dwie klasy są bardzo podobne, ale `SKPath.Iterator` mogą wyeliminować elementy w ścieżce o zerowej długości lub w pobliżu o zerowej długości. `RawIterator` Jest używany w poniższym przykładzie.

Można uzyskać obiektu typu `SKPath.RawIterator` przez wywołanie metody [ `CreateRawIterator` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CreateRawIterator()/) metody `SKPath`. Wyliczanie przez ścieżkę odbywa się przez wielokrotne wywoływanie [ `Next` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.Next/p/SkiaSharp.SKPoint[]/) metody. Przekaż do niego tablicę czterech `SKPoint` wartości:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next` Metoda zwraca element członkowski [ `SKPathVerb` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathVerb/) wyliczenia. Te wartości wskazują określonego polecenia rysowania w ścieżce. Nieprawidłowa punktów wstawione w tablicy zależy od tego zlecenia:

- [`Move`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Move/) za pomocą pojedynczego punktu
- [`Line`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Line/) dwa punkty
- [`Cubic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Cubic/) cztery punkty.
- [`Quad`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Quad/) trzy punkty.
- [`Conic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Conic/) trzy punkty (a także wywołać [ `ConicWeight` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.ConicWeight/) metodę waga)
- [`Close`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Close/) z jednym punktem
- [`Done`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Done/)

`Done` Zlecenie wskazuje, że wyliczanie zostało zakończone.

Należy zauważyć, że istnieją nie `Arc` zleceń. Oznacza to, że wszystkie łuki są konwertowane na krzywe Beziera w przypadku dodania do ścieżki.

Niektóre informacje w `SKPoint` tablicy jest nadmiarowe. Na przykład jeśli `Move` następuje zlecenie `Line` zlecenie, a następnie pierwszy dwa momenty, które towarzyszyć `Line` jest taka sama jak `Move` punktu. W praktyce tę nadmiarowość jest bardzo przydatne. Gdy uzyskujesz `Cubic` zlecenie, dołączono do niego wszystkie cztery punkty, które definiują krzywą Beziera trzeciego stopnia. Nie potrzebujesz zachować bieżącą pozycję ustanowione przez poprzednie polecenie.

Jest jednak problematyczne zlecenie `Close`. To polecenie rysuje linię prostą od bieżącej pozycji do stanu sprzed rozkład ustanowione wcześniej przez `Move` polecenia. W idealnym przypadku `Close` czasownik powinien udostępniać te dwa punkty, a nie tylko jeden punkt. Co to jest niższa jest to, że towarzyszący punktu `Close` zlecenie jest zawsze (0, 0). Oznacza to, że podczas wyliczania za pośrednictwem ścieżki, prawdopodobnie musisz zachować `Move` punkt i bieżącej pozycji.

Statyczne [ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) klasa zawiera kilka metod, które konwertują trzy rodzaje krzywych Beziera na serię określenie w przybliżeniu krzywej niewielki rozmiar proste linie. (Parametric formuły zostały przedstawione w artykule [ **trzech typów krzywych Beziera**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) `Interpolate` Metoda dzieli prostej w liczne krótkich wierszy, które są tylko jedną jednostkę długości:

```csharp
static class PathExtensions
{
    ...
    static SKPoint[] Interpolate(SKPoint pt0, SKPoint pt1)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * pt0.X + t * pt1.X;
            float y = (1 - t) * pt0.Y + t * pt1.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenCubic(SKPoint pt0, SKPoint pt1, SKPoint pt2, SKPoint pt3)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2) + Length(pt2, pt3));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * (1 - t) * pt0.X +
                        3 * t * (1 - t) * (1 - t) * pt1.X +
                        3 * t * t * (1 - t) * pt2.X +
                        t * t * t * pt3.X;
            float y = (1 - t) * (1 - t) * (1 - t) * pt0.Y +
                        3 * t * (1 - t) * (1 - t) * pt1.Y +
                        3 * t * t * (1 - t) * pt2.Y +
                        t * t * t * pt3.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenQuadratic(SKPoint pt0, SKPoint pt1, SKPoint pt2)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenConic(SKPoint pt0, SKPoint pt1, SKPoint pt2, float weight)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float denominator = (1 - t) * (1 - t) + 2 * weight * t * (1 - t) + t * t;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * weight * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * weight * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            x /= denominator;
            y /= denominator;
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

Wszystkie te metody są wywoływane z metody rozszerzenia `CloneWithTransform` pokazano poniżej. Ta metoda klony ścieżkę przez wyliczanie polecenia ścieżki i skonstruowanie nową ścieżkę, w oparciu o dane. Jednak nowa ścieżka składa się tylko z `MoveTo` i `LineTo` wywołania. Krzywe i proste linie zostały zredukowane serii niewielki rozmiar wierszy.

Podczas wywoływania `CloneWithTransform`, przekazać do metody `Func<SKPoint, SKPoint>`, czyli funkcji z `SKPaint` parametr, który zwraca `SKPoint` wartość. Ta funkcja jest wywoływana dla każdego punktu zastosować niestandardowe przekształcenia konsolidatorze:

```csharp
static class PathExtensions
{
    public static SKPath CloneWithTransform(this SKPath pathIn, Func<SKPoint, SKPoint> transform)
    {
        SKPath pathOut = new SKPath();

        using (SKPath.RawIterator iterator = pathIn.CreateRawIterator())
        {
            SKPoint[] points = new SKPoint[4];
            SKPathVerb pathVerb = SKPathVerb.Move;
            SKPoint firstPoint = new SKPoint();
            SKPoint lastPoint = new SKPoint();

            while ((pathVerb = iterator.Next(points)) != SKPathVerb.Done)
            {
                switch (pathVerb)
                {
                    case SKPathVerb.Move:
                        pathOut.MoveTo(transform(points[0]));
                        firstPoint = lastPoint = points[0];
                        break;

                    case SKPathVerb.Line:
                        SKPoint[] linePoints = Interpolate(points[0], points[1]);

                        foreach (SKPoint pt in linePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[1];
                        break;

                    case SKPathVerb.Cubic:
                        SKPoint[] cubicPoints = FlattenCubic(points[0], points[1], points[2], points[3]);

                        foreach (SKPoint pt in cubicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[3];
                        break;

                    case SKPathVerb.Quad:
                        SKPoint[] quadPoints = FlattenQuadratic(points[0], points[1], points[2]);

                        foreach (SKPoint pt in quadPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Conic:
                        SKPoint[] conicPoints = FlattenConic(points[0], points[1], points[2], iterator.ConicWeight());

                        foreach (SKPoint pt in conicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Close:
                        SKPoint[] closePoints = Interpolate(lastPoint, firstPoint);

                        foreach (SKPoint pt in closePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        firstPoint = lastPoint = new SKPoint(0, 0);
                        pathOut.Close();
                        break;
                }
            }
        }
        return pathOut;
    }
    ...
}
```

Funkcja transformacji, ponieważ sklonowany ścieżki jest ograniczona do niewielki rozmiar linii prostych, ma możliwość konwertowania proste linie na krzywe.

Zwróć uwagę, że metoda zachowuje pierwszy punkt każdej konturu w zmiennej o nazwie `firstPoint` i bieżącej pozycji po każdym poleceniu rysowania poleceń w zmiennej `lastPoint`. Są one niezbędne do utworzenia zamknięcie końcowej wiersza w przypadku `Close` zlecenie zostanie osiągnięty.

**GlobularText** próbki ta metoda rozszerzenia pozornie tekstem półkulę w efekt 3W:

[![](information-images/globulartext-small.png "Potrójna zrzut ekranu przedstawiający stronę tekstu Globular")](information-images/globulartext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Globular tekstu")

[ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) Konstruktora klasy wykonuje tej transformacji. Tworzy `SKPaint` obiektu dla tekstu, a następnie uzyskuje `SKPath` obiektu z `GetTextPath` metody. Jest to ścieżka przekazany do `CloneWithTransform` — metoda rozszerzenia wraz z funkcji przekształcenia:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;

    public GlobularTextPage()
    {
        Title = "Globular Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.Typeface = SKTypeface.FromFamilyName("Times New Roman");
            textPaint.TextSize = 100;

            using (SKPath textPath = textPaint.GetTextPath("HELLO", 0, 0))
            {
                SKRect textPathBounds;
                textPath.GetBounds(out textPathBounds);

                globePath = textPath.CloneWithTransform((SKPoint pt) =>
                {
                    double longitude = (Math.PI / textPathBounds.Width) *
                                            (pt.X - textPathBounds.Left) - Math.PI / 2;
                    double latitude = (Math.PI / textPathBounds.Height) *
                                            (pt.Y - textPathBounds.Top) - Math.PI / 2;

                    longitude *= 0.75;
                    latitude *= 0.75;

                    float x = (float)(Math.Cos(latitude) * Math.Sin(longitude));
                    float y = (float)Math.Sin(latitude);

                    return new SKPoint(x, y);
                });
            }
        }
    }
    ...
}
```

Funkcja transformacji najpierw oblicza dwie wartości o nazwie `longitude` i `latitude` zakresu od – π/2 u góry i po lewej stronie tekstu do π/2 w prawo i w dolnej części tekstu. Zakres te wartości nie jest wizualnie zadowalające, więc one zostały zredukowane przez pomnożenie przez wartość 0,75. (Wypróbuj kod bez te dostosowania. Tekst staje się zbyt zasłoniętej północnej i południowo-biegunów i zbyt alokowania elastycznego na stronie). Te trójwymiarowej kulistego współrzędne są konwertowane do dwuwymiarową `x` i `y` współrzędne przez standardowe formuły.

Nowa ścieżka są przechowywane jako pole. `PaintSurface` Obsługi jedynie musi Centrum i skalować ścieżki, aby wyświetlić go na ekranie:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint pathPaint = new SKPaint())
        {
            pathPaint.Style = SKPaintStyle.Fill;
            pathPaint.Color = SKColors.Blue;
            pathPaint.StrokeWidth = 3;
            pathPaint.IsAntialias = true;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(0.45f * Math.Min(info.Width, info.Height));     // radius
            canvas.DrawPath(globePath, pathPaint);
        }
    }
}
```

Jest to bardzo różnorodnych technika. Jeśli tablica efekty ścieżek opisanego w [ **efekty ścieżek** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) artykułu dość nie obejmują coś uznało, należy włączyć to sposób, aby uzupełnić luki.

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
