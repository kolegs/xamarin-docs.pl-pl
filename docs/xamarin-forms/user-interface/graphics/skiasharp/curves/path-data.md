---
title: Dane ścieżki SVG w SkiaSharp
description: W tym artykule opisano sposób definiowania ścieżek skiasharp — korzystanie z ciągów tekstowych w formacie skalowalnej grafice wektorowej i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: charlespetzold
ms.author: chape
ms.date: 05/24/2017
ms.openlocfilehash: f3c06198ae9e677c667c9216b3ace8784a6056b2
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615330"
---
# <a name="svg-path-data-in-skiasharp"></a>Dane ścieżki SVG w SkiaSharp

_Zdefiniuj ścieżek przy użyciu ciągi tekstowe w formacie Skalowalna grafika wektorowa_

`SKPath` Klasa obsługuje definicji obiektów pełną ścieżkę z ciągami tekstowymi w formacie ustanowione przez specyfikację skalowalne grafiki wektorowej (SVG). W dalszej części tego artykułu zobaczysz, jak mogą reprezentować całą ścieżkę, taką jak ta w ciągu tekstowym:

![](path-data-images/pathdatasample.png "Ścieżka próbki, zdefiniowana za pomocą dane ścieżki SVG")

SVG jest oparty na składni XML grafiki języka programowania dla stron sieci web. Ponieważ SVG muszą zezwalać na ścieżki zdefiniowanych w znaczników, a nie w serii wywołań funkcji, SVG standardowa obejmuje to bardzo zwięzły sposób określania ścieżki grafiki całego jako ciąg tekstowy.

W ramach SkiaSharp ten format jest określany jako "Dane ścieżki SVG —." Format jest też obsługiwana na Windows XAML środowisk opartych na programowania, w tym Windows Presentation Foundation i uniwersalnej platformy Windows, gdzie jest nazywane [składni znacznikowania ścieżki](https://msdn.microsoft.com/library/ms752293%28v=vs.110%29.aspx) lub [Przenieś i narysuj składni polecenia](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Może również służyć jako format wymiany obrazy grafiki wektorowej, szczególnie w przypadku plików tekstowych, takich jak XML.

Skiasharp — definiuje dwie metody słowami `SvgPathData` w nazwach:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Statyczne [ `ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) metoda konwertuje ciąg na `SKPath` obiektu, podczas gdy [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) konwertuje `SKPath` obiekt na ciąg.

W tym miejscu jest ciągiem SVG wskazywany przez pięć gwiazdki, a ich tematyka w punkcie (0, 0) z protokołem radius 100:

```csharp
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Litery są poleceniami, które są kompilowane `SKPath` obiektu. `M` Wskazuje `MoveTo` wywołać, `L` jest `LineTo`, i `Z` jest `Close` rozkładu zamknąć. Każda para numerów zawiera współrzędne X i Y punktu. Należy zauważyć, że `L` polecenia następuje wiele punktów rozdzielonych przecinkami. W serii współrzędnych i punkty, przecinki oraz białe znaki są traktowane identycznie. Niektórych programistów chce umieścić przecinkami, między współrzędne X i Y, a nie między punktami, ale przecinków lub spacji są tylko wymagane, aby uniknąć niejednoznaczności. Jest to idealne prawne:

```csharp
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

Składnia dane ścieżki SVG jest formalnie udokumentowane w [8.3 sekcji specyfikacji SVG](http://www.w3.org/TR/SVG11/paths.html#PathData). Poniżej przedstawiono podsumowanie:

## <a name="moveto"></a>**MoveTo**

```csharp
M x y
```

Nowe konturu w ścieżce rozpoczyna się przez ustawienie bieżącego położenia. Dane ścieżki powinna zawsze zaczynać się `M` polecenia.

## <a name="lineto"></a>**LineTo**

```csharp
L x y ...
```

To polecenie dodaje prostej (lub wierszy) do ścieżki i ustawia nowe bieżącej pozycji na końcu ostatniego wiersza. Możesz wykonać `L` polecenia wiele pary *x* i *y* współrzędnych.

## <a name="horizontal-lineto"></a>**LineTo poziome**

```csharp
H x ...
```

To polecenie dodaje linii poziomej do ścieżki i ustawia nowe bieżącej pozycji do końca wiersza. Możesz wykonać `H` polecenia z wieloma *x* współrzędne, ale nie większego sensu.

## <a name="vertical-line"></a>**Linia pionowa**

```csharp
V y ...
```

To polecenie dodaje linii pionowej do ścieżki i ustawia nowe bieżącej pozycji do końca wiersza.

## <a name="close"></a>**Zamknij**

```csharp
Z
```

`C` Polecenia zamyka sylwetką, dodając prostej z bieżącego położenia na początku ROZKŁAD.

## <a name="arcto"></a>**ArcTo**

Polecenie, aby dodać łuk eliptyczny do rozkładu jest najbardziej złożone polecenia w całej specyfikacji dane ścieżki SVG. Jest tylko polecenie, w którym liczb może reprezentować coś innego niż wartości współrzędnych:

```csharp
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*Rx* i *ry* parametry są poziome i pionowe promienie elipsy. *Kąt obrotu* jest obrotu w prawo w stopniach.

Ustaw *dużych flagi łuk* 1 w przypadku dużych łuk lub 0 w przypadku małych łuku.

Ustaw *czyszczenia flagi* 1 dla zgodnie ze wskazówkami zegara i na 0 w przypadku przeciwnie do ruchu wskazówek zegara.

Łuku w punkcie (*x*, *y*), która staje się nowe bieżącej pozycji.

## <a name="cubicto"></a>**CubicTo**

```csharp
C x1 y1 x2 y2 x3 y3 ...
```

To polecenie dodaje krzywą Beziera trzeciego stopnia od aktualnej pozycji, aby (*x3*, *y3*), która staje się nowe bieżącej pozycji. Punkty (*x1*, *y1*) i (*x2*, *y2*) są punkty kontrolne.

Można określić wiele krzywych Beziera przez jeden `C` polecenia. Liczba punktów musi być wielokrotnością liczby 3.

Dostępna jest również "smooth" polecenie krzywej Beziera:

```csharp
S x2 y2 x3 y3 ...
```

To polecenie należy stosować regularne polecenia Beziera (mimo że nie jest właściwie ma wymagane). Polecenie Beziera smooth oblicza pierwszy punkt kontrolny, tak, aby odbicia drugi punkt kontrolny z poprzednich Beziera w całym ich wzajemne punktu. Te trzy punkty są zatem colinear i połączenia między dwie krzywe Beziera jest bezproblemowe.

## <a name="quadto"></a>**QuadTo**

```csharp
Q x1 y1 x2 y2 ...
```

Dla krzywych Beziera drugiego stopnia punktów musi być wielokrotnością liczby 2. Punkt kontrolny jest (*x1*, *y1*) i punkt końcowy (i nowego bieżącej pozycji) (*x2*, *y2*)

Dostępna jest również polecenia smooth krzywą kwadratową:

```csharp
T x2 y2 ...
```

Punkt kontrolny jest obliczany na podstawie punkt kontrolny z poprzednim krzywą kwadratową.

Te polecenia są również dostępne w wersji "względna", gdzie punkty współrzędnych są względne wobec bieżącego położenia. Te polecenia względnej zaczynać się od małymi literami, na przykład `c` zamiast `C` względne wersji polecenie Beziera trzeciego stopnia.

Jest to zakres definicji dane ścieżki SVG. Nie ma możliwości dla powtarzających się grup poleceń lub do wykonywania obliczeń dowolnego typu. Polecenia dla `ConicTo` lub innych rodzajów specyfikacje łuk nie są dostępne.

Statyczne [ `SKPath.ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) metoda oczekuje prawidłowy ciąg polecenia SVG. Jeśli zostanie wykryty błąd wszelkie składni, metoda zwraca `null`. To wskazanie tylko błąd.

[ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) Metoda jest przydatna do uzyskania dane ścieżki SVG z istniejącego `SKPath` obiektu przenieść do innego programu lub przechowywać w formacie pliku tekstowego, takich jak XML. ( `ToSvgPathData` Metoda nie jest pokazana w przykładowym kodzie, w tym artykule.) Czy *nie* oczekiwać `ToSvgPathData` zwraca ciąg odpowiadający dokładnie wywołania metody, które tworzone ścieżki. W szczególności, dowiesz się, że Łuki są konwertowane na wielu `QuadTo` polecenia, i sposobu ich wyświetlania w ścieżce danych zwracanych przez `ToSvgPathData`.

**Ścieżki danych Hello** strony zaklęć się słowo "Cześć" dane ścieżki SVG. Zarówno `SKPath` i `SKPaint` obiekty są zdefiniowane jako pola w [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) klasy:

```csharp
public class PathDataHelloPage : ContentPage
{
    SKPath helloPath = SKPath.ParseSvgPathData(
        "M 0 0 L 0 100 M 0 50 L 50 50 M 50 0 L 50 100" +                // H
        "M 125 0 C 60 -10, 60 60, 125 50, 60 40, 60 110, 125 100" +     // E
        "M 150 0 L 150 100, 200 100" +                                  // L
        "M 225 0 L 225 100, 275 100" +                                  // L
        "M 300 50 A 25 50 0 1 0 300 49.9 Z");                           // O

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ...
}
```

Ścieżka Definiowanie ciąg tekstowy rozpoczyna się od lewego górnego rogu w momencie (0, 0). Każdej litery jest maksymalnie 50 jednostek szerokości i wysokości 100 jednostek i litery są oddzielone innego 25 jednostek, który oznacza, że całą ścieżkę 350 jednostki szerokości.

"H" "Hello" składa się z trzech konturów on-line, "E" jest na dwie krzywe Beziera trzeciego stopnia połączonych. Należy zauważyć, że `C` polecenia następuje sześć punktów i dwóch punktów kontrolnych ma współrzędne Y –10 i 110, która umieszcza je poza zakresem współrzędne Y pozostałe litery. "L" jest dwie połączone linie, podczas gdy ' O' jest elipsy, który jest renderowany przy użyciu `A` polecenia.

Należy zauważyć, że `M` polecenie, które zaczyna się ostatni rozkład Ustawia położenie punktu (350, 50), czyli środka w pionie po lewej stronie strony ' O'. Wskazane przez pierwszy następujące numery `A` polecenia elipsy ma promień 25 w poziomie i pionie promieniu 50. Punkt końcowy jest wskazywany przez ostatnią parę liczb znajdujących się w `A` polecenia, które reprezentuje punkt (300, 49.9). Celowo nieco różni się od punktu początkowego. Punkt końcowy jest równe punkt początkowy, łuk nie będzie renderowana. Aby narysować elipsę pełną, należy ustawić punkt końcowy Zamknij, aby (ale nie równa się) punkt początkowy, lub należy użyć co najmniej dwóch `A` polecenia każdej części pełną elipsy.

Możesz chcieć dodać następującą instrukcję do konstruktora strony, a następnie ustaw punkt przerwania, aby zbadać wynikowy ciąg:

```csharp
string str = helloPath.ToSvgPathData();
```

Dowiesz się, że została zastąpiona łuk długiego szeregu `Q` poleceń fragmentaryczne zbliżenia łuk przy użyciu drugiego stopnia krzywych Beziera.

`PaintSurface` Obsługi uzyskuje ścisłej granice ścieżki, która nie obejmuje punkty kontrolne dla "E" i "O' krzywych. Trzy przekształcenia przenoszenie środek ścieżka do punktu (0, 0), skalowanie ścieżki do rozmiaru obszaru roboczego (ale biorąc pod uwagę także szerokość pociągnięcia) i następnie przenieść środek ścieżka na środku kanwy:

```csharp
public class PathDataHelloPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect bounds;
        helloPath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(info.Width / (bounds.Width + paint.StrokeWidth),
                     info.Height / (bounds.Height + paint.StrokeWidth));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(helloPath, paint);
    }
}
```

Ścieżka wypełnia obszar roboczy, który będzie bardziej przystępne, podczas wyświetlania w orientacji poziomej:

[![](path-data-images/pathdatahello-small.png "Potrójna zrzut ekranu przedstawiający stronę ścieżki danych Hello")](path-data-images/pathdatahello-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę ścieżki danych Hello")

**Ścieżki danych Cat** strona jest podobna. Obiekty ścieżki i paint są definiowane jako pola w [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) klasy:

```csharp
public class PathDataCatPage : ContentPage
{
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 5
    };
    ...
}
```

Nagłówek cat jest okrąg, a w tym miejscu jest on renderowany przy użyciu dwóch `A` poleceń, z których każdy rysuje promieniu. Zarówno `A` polecenia do głowy zdefiniować poziome i pionowe promienie 100. Pierwszy łuk rozpoczyna się od (240, 100) i kończy się (240, 300), które stają się punkt początkowy dla drugi łuk, kończące w (240, 100).

Dwa oczy również są renderowane przy użyciu dwóch `A` polecenia i podobnie jak w przypadku jego head, drugi `A` polecenie kończy się w tym samym punkcie jako początek pierwszy `A` polecenia. Jednak te pary `A` poleceń nie definiują elipsę. Z każdego łuku jest 40 jednostek oraz promień 40 jednostek, co oznacza, że te Łuki nie są pełne semicircles.

`PaintSurface` Program obsługi wykonuje transformacji podobne jak w poprzednim przykładzie, ale określa pojedynczy `Scale` współczynnik, aby zachować współczynnik proporcji i udostępniają margines mały, więc wąsów cat don't touch stronach ekranu:

```csharp
public class PathDataCatPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        SKRect bounds;
        catPath.GetBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(0.9f * Math.Min(info.Width / bounds.Width,
                                     info.Height / bounds.Height));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(catPath, paint);
    }
}
```

W tym miejscu jest uruchomiony na wszystkich trzech platformach program:

[![](path-data-images/pathdatacat-small.png "Potrójna zrzut ekranu przedstawiający stronę ścieżki danych Cat")](path-data-images/pathdatacat-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę ścieżki danych Cat")

Zwykle, gdy `SKPath` obiekt jest zdefiniowany jako pole, konturów ścieżka musi być zdefiniowany w konstruktorze lub innej metody. Korzystając z dane ścieżki SVG, jednak wiesz że ścieżce można określić wyłącznie w definicji pola.

Wcześniej **Nieładnego zegara analogowy** próbki w [ **Przekształcanie Obróć** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) artykułu wyświetlane ręce zegara jako prosty wierszy. **Dość analogowy zegara** poniższy program zastępuje te wiersze z `SKPath` obiekty zdefiniowane jako pola w [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) klasy wraz z `SKPaint` obiektów:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    // Clock hands pointing straight up
    SKPath hourHandPath = SKPath.ParseSvgPathData(
        "M 0 -60 C   0 -30 20 -30  5 -20 L  5   0" +
                "C   5 7.5 -5 7.5 -5   0 L -5 -20" +
                "C -20 -30  0 -30  0 -60 Z");

    SKPath minuteHandPath = SKPath.ParseSvgPathData(
        "M 0 -80 C   0 -75  0 -70  2.5 -60 L  2.5   0" +
                "C   2.5 5 -2.5 5 -2.5   0 L -2.5 -60" +
                "C 0 -70  0 -75  0 -80 Z");

    SKPath secondHandPath = SKPath.ParseSvgPathData(
        "M 0 10 L 0 -80");

    // SKPaint objects
    SKPaint handStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2,
        StrokeCap = SKStrokeCap.Round
    };

    SKPaint handFillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Gray
    };
    ...
}
```

Godzinowe i minutowe ręce teraz mieć ujęte obszarów, tak aby te ćwiczenia różne od siebie nawzajem, rysowania za pomocą zarówno czarny konspektu i za pomocą wypełnienia szarego `handStrokePaint` i `handFillPaint` obiektów.

We wcześniejszych przykładach **Nieładnego zegara analogowy** próbki, małym kół, oznaczony godziny i minuty były rysowane w pętli. W tym **dość analogowy zegara** przykładowe służy zupełnie innego podejścia: znaki godzinowe i minutowe są linii kropkowanej z `minuteMarkPaint` i `hourMarkPaint` obiektów:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    SKPaint minuteMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 3 * 3.14159f }, 0)
    };

    SKPaint hourMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 6,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 15 * 3.14159f }, 0)
    };
    ...
}
```

[ **Kropki i kreski** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) przewodniku omówiono sposób korzystania `SKPathEffect.CreateDash` metodę, aby utworzyć linię przerywaną. Pierwszy argument jest `float` tablica, która zazwyczaj ma dwa elementy: pierwszy element jest długością kresek, a drugi element stanowi lukę między kresek. Gdy `StrokeCap` właściwość jest ustawiona na `SKStrokeCap.Round`, następnie zaokrąglone zakończenia kreska skutecznie wydłużyć długość kreski przez szerokość pociągnięcia po obu stronach się kreska. W związku z tym ustawienie pierwszy element tablicy 0 tworzy linię kropkowaną.

Odległość między tych punktów jest regulowane przez drugiego elementu tablicy. Jak zobaczysz wkrótce, te dwie `SKPaint` obiekty służą do rysowania kół z protokołem radius, 90 jednostek. Obwód koła to jest zatem 180π, co oznacza, że znaczniki 60 minut musi znajdować się co 3π jednostki, która jest wartością drugiego w `float` tablicy w `minuteMarkPaint`. Znaczniki dwanaście godzin musi znajdować się co 15π jednostek, która jest wartością w drugim `float` tablicy.

`PrettyAnalogClockPage` Klasy ustawia czasomierza do unieważnienia powierzchni co 16 milisekund i `PaintSurface` program obsługi jest wywoływany po tym kursie. Definicje wcześniejsze `SKPath` i `SKPaint` obiektów umożliwiają bardzo czystym kodu rysowania:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Transform for 100-radius circle in center
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / 200, info.Height / 200));

        // Draw circles for hour and minute marks
        SKRect rect = new SKRect(-90, -90, 90, 90);
        canvas.DrawOval(rect, minuteMarkPaint);
        canvas.DrawOval(rect, hourMarkPaint);

        // Get time
        DateTime dateTime = DateTime.Now;

        // Draw hour hand
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawPath(hourHandPath, handStrokePaint);
        canvas.DrawPath(hourHandPath, handFillPaint);
        canvas.Restore();

        // Draw minute hand
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawPath(minuteHandPath, handStrokePaint);
        canvas.DrawPath(minuteHandPath, handFillPaint);
        canvas.Restore();

        // Draw second hand
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        canvas.Save();
        canvas.RotateDegrees(6 * (dateTime.Second + (float)t));
        canvas.DrawPath(secondHandPath, handStrokePaint);
        canvas.Restore();
    }
}
```

Efekty specjalne odbywa się przy użyciu drugiej wskazówki, jednak. Ponieważ zegar jest aktualizowany co 16 milisekund `Millisecond` właściwość `DateTime` wartość potencjalnie można animować odchylenia drugi strony zamiast jednego, który przesuwa dyskretnych przechodzi z sekundy na sekundę. Jednak ten kod nie zezwala na przenoszenie jako bezproblemowe. Zamiast tego używa Xamarin.Forms [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) i [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) animacji ułatwianie funkcje inny rodzaj przepływu. Te funkcje easingu spowodować drugiej wskazówki przenieść w sposób jerkier &mdash; ponownie ściąganie, trochę przed przemieszczał się, a następnie nieco nadmiernie rozwiązywania problemów miejsca docelowego, efekt oznacza Niestety nie można odtworzyć te statyczne zrzuty ekranu:

[![](path-data-images/prettyanalogclock-small.png "Potrójna zrzut ekranu przedstawiający stronę dość analogowy zegara")](path-data-images/prettyanalogclock-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę dość analogowy zegara")


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
