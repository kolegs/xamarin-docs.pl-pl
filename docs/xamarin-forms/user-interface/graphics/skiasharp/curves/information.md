---
title: "Informacje o ścieżce i wyliczenia"
description: "Uzyskaj informacje na temat ścieżek i analizy zawartości"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/12/2017
ms.openlocfilehash: c7edf0c8e563dad25693d184d3a44a3e66466126
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="path-information-and-enumeration"></a>Informacje o ścieżce i wyliczenia

_Uzyskaj informacje na temat ścieżek i analizy zawartości_

[ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) Klasa definiuje kilka właściwości i metody, które umożliwiają uzyskanie informacji o ścieżce. [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) i [ `TightBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.TightBounds/) właściwości (oraz odpowiednich metod) uzyskać metrical wymiary ścieżki. [ `Contains` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Contains/p/System.Single/System.Single/) — Metoda pozwala określić, czy określonego punktu jest do ścieżki.

Czasami jest przydatne do określenia całkowita długość linii i krzywych wchodzące w skład ścieżki. To nie jest algorithmically prostym zadaniem, więc całej klasy o nazwie [ `PathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) jest przeznaczone do niego.

Warto również czasami uzyskania wszystkich rysowania operacji oraz punktów, które tworzą ścieżki. Na początku tej funkcji może wydawać się niepotrzebne: Jeśli program został utworzony ścieżki, program zna już zawartość. Jednak przedstawiono ścieżki również mogą być tworzone przez [efekty ścieżki](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) i konwertując [ciągi na ścieżki](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md). Możesz również uzyskać wszystkie rysowania operacji i punkty wchodzące w skład tych ścieżek. Jedną z możliwości jest zastosowanie transformacji algorytmicznego do wszystkich punktów. Dzięki temu technik, takich jak zawijania tekstu wokół półkulę:

![](information-images/pathenumerationsample.png "Tekst zawijany na półkulę")

## <a name="getting-the-path-length"></a>Pobieranie długości ścieżki

W artykule [ **ścieżek i tekst** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) pokazano, jak używać [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metodę, by narysować ciąg tekstowy, którego linii bazowej następuje ciągu ścieżki. Ale co zrobić, jeśli chcesz rozmiaru tekst, tak aby zmieścił ścieżka dokładnie? Dla Rysowanie tekstu wokół koło, łatwo jest, ponieważ obwód koła jest prosty do obliczenia. Jednak nie jest tak proste obwód elipsy lub długość krzywej Beziera. 

[ `SKPathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) Klasy może pomóc. [Konstruktor](https://developer.xamarin.com/api/constructor/SkiaSharp.SKPathMeasure.SKPathMeasure/p/SkiaSharp.SKPath/System.Boolean/System.Single/) akceptuje `SKPath` argument, a [ `Length` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPathMeasure.Length/) właściwości ujawnia jej długość.

To jest przedstawiona w **długość ścieżki** próbki, która jest oparta na **krzywej Beziera** strony. [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) plik pochodzi z `InteractivePage` i zawiera interfejs touch:

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

[ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) pliku CodeBehind umożliwia przeniesienie cztery punkty touch do definiowania punktów końcowych i kontrolowania punktów krzywej Beziera trzeciego stopnia. Ciąg tekstowy, zdefiniuj trzy pola `SKPaint` obiekt i obliczeniowej szerokości tekstu:

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

`baseTextWidth` Pole jest szerokości tekstu, na podstawie `TextSize` ustawienie 10.

`PaintSurface` Obsługi rysuje krzywej Beziera i następnie rozmiar tekst, który mieści się wraz z jego długość pełnej:

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

`Length` Właściwość nowo utworzony `SKPathMeasure` obiektu uzyskuje długość ścieżki. To jest podzielona przez `baseTextWidth` wartość, (czyli szerokości tekstu, na podstawie rozmiaru tekstu, 10), a następnie pomnożona przez rozmiar tekst podstawowy 10. Wynik jest nowy rozmiar tekstu do wyświetlania tekstu w tej ścieżce:

[![](information-images/pathlength-small.png "Potrójna zrzut ekranu przedstawiający stronę długość ścieżki")](information-images/pathlength-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę długość ścieżki")

Zgodnie z krzywej Beziera pobiera dłuższy lub krótszy, widać Zmień rozmiar tekstu.

## <a name="traversing-the-path"></a>Przechodzenie przez ścieżkę

`SKPathMeasure` należy nie tylko miary długość ścieżki. Na dowolną wartość z zakresu między zero a długość ścieżki `SKPathMeasure` obiektu w tym momencie uzyskać pozycji na ścieżkę i tangens na krzywą ścieżki. Tangens jest dostępna jako wektor w formie `SKPoint` obiektu lub jako rotacji hermetyzowane w `SKMatrix` obiektu. Poniżej przedstawiono metody `SKPathMeasure` który uzyskania tych informacji w różnych i elastyczny sposób:

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

**Potoku połowie motocyklem** rysunku USB na motocyklem, który wydaje się, które wywołują i z powrotem krzywej Beziera sześcienny animuje strony:

[![](information-images/unicyclehalfpipe-small.png "Potrójna zrzut ekranu strony potoku połowie motocyklem")](information-images/unicyclehalfpipe-large.png#lightbox "Potrójna zrzut ekranu strony motocyklem połowie potoku")

`SKPaint` Obiekt używany do używana do malowania zarówno połowa kreski pionowej, jak i motocyklem jest zdefiniowany jako pole w [ `UnicycleHalfPipePage` ]() klasy. Jest także zdefiniowana `SKPath` obiektu dla motocyklem:

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

Klasa zawiera standard przesłonięcia `OnAppearing` i `OnDisappearing` metody dla animacji. `PaintSurface` Obsługi tworzy ścieżki dla potoku połowie, a następnie go rysuje. `SKPathMeasure` Obiekt jest tworzony na podstawie na tej ścieżce:

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

`PaintSurface` Obsługi oblicza wartość `t` skojarzoną z zakresu od 0 do 1 co pięć sekund. Następnie używa `Math.Cos` funkcji konwersji, która wartość `t` który zakresu od 0 do 1 i z powrotem na 0, gdzie 0 odpowiada motocyklem na początku u góry po lewej, gdy 1 odpowiada motocyklem u góry po prawej. Funkcja cosinus powoduje, że szybkość najwolniejsze w górnej części potoku i najszybsze u dołu.

Należy zauważyć, że ta wartość `t` długość ścieżki dla pierwszego argumentu musi zostać pomnożona wartość `GetMatrix`. Macierz jest następnie stosowany do `SKCanvas` obiektu dla ścieżki motocyklem rysowania.

## <a name="enumerating-the-path"></a>Wyliczanie ścieżki

Osadzone dwa rodzaje `SKPath` umożliwiają wyliczyć zawartości ścieżki. Te klasy są [ `SKPath.Iterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+Iterator/) i [ `SKPath.RawIterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+RawIterator/). Dwie klasy są bardzo podobne, ale `SKPath.Iterator` można wyeliminować elementów w ścieżce o zerowej długości lub bliski o zerowej długości. `RawIterator` Jest używany w poniższym przykładzie.

Można uzyskać obiektu typu `SKPath.RawIterator` przez wywołanie metody [ `CreateRawIterator` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CreateRawIterator()/) metody `SKPath`. Wyliczanie za pośrednictwem ścieżki odbywa się wielokrotnie wywołując [ `Next` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.Next/p/SkiaSharp.SKPoint[]/) metody. Przekazywania do niej tablicę cztery `SKPoint` wartości:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next` Metoda zwraca element członkowski [ `SKPathVerb` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathVerb/) wyliczenia. Te wartości wskazują polecenie rysowania określonego w ścieżce. Liczba prawidłowych punktów wstawione w tablicy zależy od tego zlecenia:

- [`Move`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Move/) z jednego miejsca
- [`Line`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Line/) z dwa punkty.
- [`Cubic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Cubic/) cztery punkty.
- [`Quad`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Quad/) trzy punkty.
- [`Conic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Conic/) trzy punkty (a także wywołać [ `ConicWeight` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.ConicWeight/) metody wagi)
- [`Close`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Close/) z jednego punktu
- [`Done`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Done/)

`Done` Zlecenie wskazuje, że wyliczenia jest pełny.

Należy zauważyć, że istnieją nie `Arc` zleceń. Oznacza to, że wszystkie łuki są konwertowane na krzywe Beziera w przypadku dodania do ścieżki.

Niektóre informacje w `SKPoint` tablicy jest nadmiarowy. Na przykład jeśli `Move` następuje zlecenie `Line` zlecenie, a następnie pierwszy z dwóch punktów towarzyszą `Line` jest taka sama jak `Move` punktu. W praktyce dublowanie jest bardzo przydatne. Gdy `Cubic` zlecenie, towarzyszy wszystkie cztery punkty definiujące krzywą Beziera trzeciego stopnia. Nie trzeba zachować bieżące położenie ustala zlecenie poprzedniej.

Zlecenie problemy, jednak jest `Close`. To polecenie Rysuje prostą od bieżącego położenia na początku rozkład ustanowić wcześniej przez `Move` polecenia. W idealnym przypadku `Close` zlecenie powinno dostarczyć te dwa punkty, a nie tylko jeden punkt. Co to jest pogarsza się wraz z oznacza, że punkt towarzyszącego `Close` zlecenie jest zawsze (0, 0). Oznacza to, że podczas wyliczania za pomocą ścieżki, prawdopodobnie musisz zachować `Move` punktu i bieżącego położenia.

Statycznych [ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) klasa zawiera kilka metod, które przekonwertować trzy typy krzywych Beziera na serię niewielki rozmiar proste, które przybliżona krzywej. (Parametrycznych formuły zostały przedstawione w artykule [ **trzy typy krzywych Beziera**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) `Interpolate` Metody dzieli prostej do wielu krótkich wierszy, które są tylko jedną jednostkę długości:

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

Wszystkie te metody są przywoływane przez metodę rozszerzenie `CloneWithTransform` pokazano poniżej. Ta metoda klonów ścieżki wyliczania polecenia ścieżki i utworzenie nowej ścieżki na podstawie danych. Jednak nowej ścieżki składa się tylko z `MoveTo` i `LineTo` wywołania. Krzywe i proste zostały zredukowane do serii niewielki rozmiar wierszy.

Podczas wywoływania metody `CloneWithTransform`, przekazać do metody `Func<SKPoint, SKPoint>`, która to funkcja z `SKPaint` parametr, który zwraca `SKPoint` wartość. Ta funkcja jest wywoływana dla każdego punktu zastosowanie niestandardowych transformacji algorytmicznego:

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

Ponieważ sklonowany ścieżka zostanie zmniejszona do niewielki rozmiar linii prostej, funkcji przekształcenia ma możliwości konwersji proste krzywych.

Powiadomienie, że metoda zachowuje pierwszy punkt każdej rozkład w zmiennej o nazwie `firstPoint` i bieżące położenie po każdym poleceniu Rysowanie polecenia w zmiennej `lastPoint`. Są one niezbędne do utworzenia końcowego zamknięcia wiersza w przypadku `Close` napotkano zlecenia.

**GlobularText** w przykładzie użyto tę metodę rozszerzenie pozornie tekstem półkulę efektu 3D:

[![](information-images/globulartext-small.png "Potrójna zrzut ekranu strony tekstu Globular")](information-images/globulartext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Globular tekstu")

[ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) Konstruktora klasy wykonuje tej transformacji. Tworzy `SKPaint` obiekt tekstu, a następnie uzyskuje `SKPath` obiekt z `GetTextPath` metody. Jest to ścieżka przekazany do `CloneWithTransform` — metoda rozszerzenia wraz z funkcji przekształcenia: 

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

Funkcja transformacji najpierw oblicza dwóch wartości o nazwie `longitude` i `latitude` zakresie od — π/2 u góry i po lewej stronie tekstu do π/2 w prawo i w dolnej części tekstu. Te wartości z zakresu nie jest wizualnie zadowalające, więc ich zostały zredukowane przez pomnożenie przez wartość 0,75. (Spróbuj kodu bez te dostosowania. Tekst staje się zbyt zasłoniętej na północ i południowo Kijki i zbyt alokowania na stronie). Trójwymiarowy współrzędnych kulistego są konwertowane na dwuwymiarowa `x` i `y` współrzędne przez standardowe formuły.

Nowa ścieżka jest przechowywana jako pola. `PaintSurface` Obsługi jedynie trzeba Wyśrodkowanie i skalować ścieżki do jego wyświetlenia na ekranie:

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

Jest to metoda bardzo elastyczne. Jeśli tablica efekty ścieżki opisane w [ **efekty ścieżki** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) artykułu dość nie obejmować coś mieli świadomość, należy włączyć to sposób wypełniania luk.

## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
