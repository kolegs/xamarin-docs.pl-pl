---
title: Dane ścieżki SVG
description: Zdefiniuj ścieżek przy użyciu ciągów tekstowych w formacie Skalowalna grafika wektorowa
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: charlespetzold
ms.author: chape
ms.date: 05/24/2017
ms.openlocfilehash: fe9699894224d9a33b3a79e9b5bcd4cd41c635dd
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/05/2018
---
# <a name="svg-path-data"></a>Dane ścieżki SVG

_Zdefiniuj ścieżek przy użyciu ciągów tekstowych w formacie Skalowalna grafika wektorowa_

`SKPath` Klasa obsługuje definicji obiektów całej ścieżki z ciągów tekstowych w formacie ustanowionych przez specyfikację skalowalne wektor grafiki SVG. W dalszej części tego artykułu zobaczysz, jak może reprezentować całej ścieżce, takie jak ta, w ciągu tekstowym:

![](path-data-images/pathdatasample.png "Przykładowe ścieżki zdefiniowane z SVG ścieżki danych")

SVG jest język dla stron sieci web do programowania grafiki opartych na języku XML. Ponieważ SVG musi zezwalać na ścieżki do zdefiniowania znaczników zamiast szereg wywołania funkcji, standardowe SVG obejmuje bardzo krótkie sposób określenia ścieżki grafiki cały ciąg tekstowy.

W ramach SkiaSharp ten format jest nazywany "SVG ścieżki danych." Format jest też obsługiwana na opartych na języku XAML Windows środowiska programowania, w tym Windows Presentation Foundation i platformy uniwersalnej systemu Windows, gdy jest znany jako [składnia znacznika ścieżki](https://msdn.microsoft.com/library/ms752293%28v=vs.110%29.aspx) lub [Przenieś i rysowanie składni polecenia](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Służy również jako format wymiana obrazów grafiki wektorowej, szczególnie w przypadku plików tekstowych, takich jak XML.

SkiaSharp definiuje dwie metody słowami `SvgPathData` w nazwach:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Statycznych [ `ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) metoda konwertuje ciąg na `SKPath` obiektu, gdy [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) konwertuje `SKPath` obiektu na ciąg.

Oto ciągu SVG gwiazdy odnosi się do pięciu skupia się na punkt (0, 0) z protokołem radius 100:

```csharp
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Polecenia, które kompilacji są litery `SKPath` obiektu. `M` Wskazuje `MoveTo` wywołać, `L` jest `LineTo`, i `Z` jest `Close` zamknąć ROZKŁAD. Każda para zapewnia współrzędną X i Y punktu. Zwróć uwagę, że `L` polecenia następuje wiele punktów rozdzielonych przecinkami. W serii współrzędnych i punkty, przecinkami i odstępu są traktowane identycznie. Niektóre programistów chce umieścić przecinkami między współrzędne X i Y, a nie między punktami, ale przecinków lub spacji są tylko wymagane, aby uniknąć niejednoznaczności. Jest to idealne prawnych:

```csharp
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

Składnia danych ścieżki SVG formalnie opisano w [8.3 sekcji specyfikacji SVG](http://www.w3.org/TR/SVG11/paths.html#PathData). Poniżej przedstawiono podsumowanie:

## <a name="moveto"></a>**MoveTo**

```csharp
M x y
```

Nowy rozkład w ścieżce rozpoczyna się przez ustawienie bieżącego położenia. Ścieżka danych zawsze powinny rozpoczynać się od `M` polecenia.

## <a name="lineto"></a>**LineTo**

```csharp
L x y ...
```

To polecenie dodaje prostej (lub wierszy) do ścieżki i ustawia nowe bieżącej pozycji na końcu ostatniego wiersza. Możesz wykonać `L` z wiele par *x* i *y* współrzędnych.

## <a name="horizontal-lineto"></a>**LineTo pozioma**

```csharp
H x ...
```

To polecenie dodaje linii poziomej do ścieżki i ustawia nowe bieżącej pozycji do końca wiersza. Możesz wykonać `H` polecenie z wieloma *x* współrzędne, ale nie ma sensu wiele.

## <a name="vertical-line"></a>**Linii pionowej**

```csharp
V y ...
```

To polecenie dodaje linii pionowej do ścieżki i ustawia nowe bieżącej pozycji do końca wiersza.

## <a name="close"></a>**Zamknij**

```csharp
Z
```

`C` Polecenie zamyka rozkład przez dodanie prostej od bieżącej pozycji na początku rozkładu.

## <a name="arcto"></a>**ArcTo**

Polecenie, aby dodać łuku do konturu jest najbardziej złożonych polecenia w specyfikacji całej ścieżki danych SVG. Jest tylko polecenie, w którym numery może reprezentować coś innego niż Współrzędna wartości:

```csharp
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*Rx* i *ry* parametry są poziome i pionowe promienie elipsy. *Kąt obrotu* jest prawo stopni.

Ustaw *dużych flagi łuk* 1 w przypadku dużych łuk lub 0 dla małych łuk.

Ustaw *flagi odchylenia* 1 wskazówek zegara i 0 dla zegara.

Łuku do punktu (*x*, *y*), które stają się nowych bieżącego położenia.

## <a name="cubicto"></a>**CubicTo**

```csharp
C x1 y1 x2 y2 x3 y3 ...
```

To polecenie dodaje sześcienny krzywej Beziera z bieżącej pozycji do (*x3*, *y3*), które stają się nowych bieżącego położenia. Punkty (*x1*, *y1*) i (*x2*, *y2*) są punkty kontrolne.

Można określić wiele krzywych Beziera przez jeden `C` polecenia. Liczba punktów musi być wielokrotnością liczby 3.

Istnieje również polecenie "smooth" krzywej Beziera:

```csharp
S x2 y2 x3 y3 ...
```

To polecenie należy wykonać regularne polecenia Beziera (chociaż ściśle nie są wymagane). Polecenie Beziera smooth oblicza pierwszy punkt kontrolny tak, aby odbicia drugiego punktu kontrolnego z poprzednich Beziera wokół ich wzajemne punktu. Te trzy punkty w związku z tym są colinear i smooth jest połączenie między dwie krzywe Beziera.

## <a name="quadto"></a>**QuadTo**

```csharp
Q x1 y1 x2 y2 ...
```

Dla krzywych Beziera kwadratową liczba punktów musi być wielokrotnością liczby 2. Punkt kontrolny jest (*x1*, *y1*) i punkt końcowy (i nowych bieżącego położenia) (*x2*, *y2*)

Istnieje również polecenia smooth było dodać krzywą kwadratową:

```csharp
T x2 y2 ...
```

Punkt kontrolny jest obliczana na podstawie punkt kontrolny od poprzedniego krzywą kwadratową.

Tych poleceń są również dostępne w wersjach "względna" gdzie współrzędnych punktów względem bieżącego położenia. Te polecenia względną rozpoczynać się od małych liter, na przykład `c` zamiast `C` względną wersji sześcienny polecenia Beziera.

Jest to zakres definicji danych ścieżki SVG. Nie ma żadnych zakładzie powtarzalnych grupy poleceń lub do wykonania dowolnego typu obliczenia. Polecenia dla `ConicTo` lub inne typy specyfikacji łuk nie są dostępne.

Statycznych [ `SKPath.ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) metoda oczekuje prawidłowy ciąg polecenia SVG. Jeśli zostanie wykryty błąd żadnych składni, metoda zwraca `null`. To wskazuje błąd tylko.

[ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) Metoda jest przydatna do uzyskania SVG ścieżki danych z istniejącego `SKPath` obiektu do przenoszenia do innego programu, lub do przechowywania w formacie pliku tekstowym, takich jak XML. ( `ToSvgPathData` — Metoda nie jest prezentowana w przykładowym kodzie w tym artykule.) Czy *nie* oczekiwać `ToSvgPathData` zwraca ciąg odpowiadający dokładnie wywołania metody, które utworzone ścieżki. W szczególności dowiesz się, że Łuki są konwertowane na wielu `QuadTo` poleceń, i sposobu ich wyświetlania w ścieżce danych zwróconych z `ToSvgPathData`.

**Ścieżki danych Hello** stronę zaklęć limit słowo "HELLO" przy użyciu danych ścieżki SVG. Zarówno `SKPath` i `SKPaint` obiekty są zdefiniowane jako pola w [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) klasy:

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

Ścieżka Definiowanie ciąg tekstowy rozpoczyna się w lewym górnym rogu w punkcie (0, 0). Litera każdego jest 50 jednostek szerokości i wysokości 100 jednostek i litery są rozdzielone przez inny 25 jednostek, które oznacza, że całą ścieżkę 350 jednostki szerokości.

"H" tekst "Hello" składa się z trzech konturów jednego wiersza, gdy dwie krzywe Beziera sześcienny połączonych jest "E". Zwróć uwagę, że `C` polecenia następuje sześciu punktów i mieć dwa punkty kontrolne współrzędne Y –10 i 110 umieszczane poza zakresem współrzędną Y innego litery. "L" jest dwa połączone linie, podczas gdy "O' jest elipsy jest odwzorowywany z `A` polecenia.

Zwróć uwagę, że `M` polecenie rozpoczęcia ostatniego rozkład ustawia pozycję w punkcie (350, 50), czyli środka w pionie po lewej stronie z "O'. Określone przez następujące pierwszej liczby `A` polecenia elipsy ma promień w poziomie 25 i promień w pionie 50. Punkt końcowy jest określane przez ostatnich pary liczb znajdujących się w `A` polecenia, które reprezentuje punkt (300, 49.9). Celowo nieco różni się od punktu początkowego. Jeśli punkt końcowy jest równe punkt początkowy, nie można renderować łuk. Rysowanie elipsy pełnej, należy ustawić punkt końcowy Zamknij, aby (ale nie jest równa) punkt początkowy, lub użyj co najmniej dwóch `A` polecenia każdej części pełną elipsy.

Możesz chcieć Dodaj następującą instrukcję do konstruktora strony, a następnie ustaw punkt przerwania, aby zbadać wynikowy ciąg:

```csharp
string str = helloPath.ToSvgPathData();
```

Dowiesz się, że łuk zostało zastąpione długich serii `Q` polecenia fragmentaryczne zbliżenia łuk przy użyciu kwadratową krzywych Beziera.

`PaintSurface` Obsługi uzyskuje ścisłej granice ścieżki, która nie zawiera punktów kontrolnych dla "E" i "O' krzywych. Trzy przekształcenia Przenieś Centrum ścieżki do punktu (0, 0), skalowanie ścieżkę do rozmiaru obszaru roboczego (ale biorąc pod uwagę także szerokość pociągnięć), a następnie przesuń Centrum ścieżki na środku kanwy:

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

Ścieżka wypełnia obszaru roboczego, który wygląda bardziej przystępne wyświetlony w trybie krajobraz:

[![](path-data-images/pathdatahello-small.png "Potrójna zrzut ekranu przedstawiający stronę ścieżki danych Hello")](path-data-images/pathdatahello-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę ścieżki danych Hello")

**Ścieżki danych Cat** przypomina strony. Obiekt ścieżki i paint zarówno zdefiniowano jako pola w [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) klasy:

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

Nagłówek cat jest koło, a tutaj jest on renderowany przy użyciu dwóch `A` poleceń, z których każdy rysuje Półkolista. Zarówno `A` polecenia dla nagłówek określają poziome i pionowe promienie 100. Pierwszy łuk zaczyna się od (240, 100) i kończy się na (240, 300), które stają się punkt początkowy dla drugiego łuku, która kończy się w (240, 100).

Dwa oczu również są renderowane przy użyciu dwóch `A` poleceń i tak jak w przypadku jego head, drugi `A` polecenie kończy się w tym samym punkcie jako pierwszego uruchomienia `A` polecenia. Jednak te pary `A` poleceń nie definiują elipsy. Z każdego łuku jest 40 jednostki oraz promień 40 jednostki, co oznacza, że te Łuki nie są pełne semicircles.

`PaintSurface` Obsługi wykonuje transformacji podobnych jako powyższego przykładu, ale ustawia jeden `Scale` współczynnik współczynnik proporcji i udostępniają małego margines, więc jego wąsów nie ruszaj krawędzi ekranu:

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

Oto programu uruchomionego na wszystkich platformach trzy:

[![](path-data-images/pathdatacat-small.png "Potrójna zrzut ekranu przedstawiający stronę ścieżki danych Cat")](path-data-images/pathdatacat-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę ścieżki danych Cat")

Zwykle, gdy `SKPath` obiektu jest zdefiniowana jako pole, konturów ścieżka musi być zdefiniowana w konstruktorze lub innej metody. Podczas korzystania z danych ścieżki SVG, jednak przedstawiono można określić wyłącznie w definicji pola ścieżka.

Wcześniej **Ugly zegar analogowy** przykładowa w [ **Obróć przekształcenie** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) artykułu wyświetlany ręce zegara jako prostych linii. **Pretty analogowy zegara** poniższy program zastępuje te wiersze z `SKPath` obiektów zdefiniowanych jako pola w [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) klasy wraz z programem `SKPaint` obiektów:

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

Teraz ręce godzinowe i minutowe ma ujęta obszary, tak aby tych ręce różne od siebie, rysowania z czarnym konspektu i za pomocą wypełnienia szarego `handStrokePaint` i `handFillPaint` obiektów.

W starszych **Ugly zegar analogowy** przykładowe, trochę okręgi, który oznaczony godziny i minuty były rysowane w pętli. W tym **Pretty analogowy zegara** przykładowa służy zupełnie innego podejścia: są znaki godzinowe i minutowe kropkowana linii z `minuteMarkPaint` i `hourMarkPaint` obiektów:

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

[ **Kropki i kreski** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) przewodniku opisano sposób korzystania `SKPathEffect.CreateDash` metodę, aby utworzyć linię kropkowaną. Pierwszy argument jest `float` tablica, która zazwyczaj ma dwa elementy: pierwszy element ma długość kresek, a drugi element odstęp między łączników na pauzy. Gdy `StrokeCap` właściwość jest ustawiona na `SKStrokeCap.Round`, następnie zaokrąglony końców kreska skutecznie wydłużyć długość kreski o szerokości obrysu po obu stronach kreska. W związku z tym ustawienie pierwszy element tablicy 0 tworzy linię kropkowaną.

Odległość między tych punktów jest objęte drugiego elementu tablicy. Jak można zauważyć wkrótce, te dwie `SKPaint` obiekty służą do rysowania okręgi z protokołem radius 90 jednostek. Obwód tego koło w związku z tym jest 180π, co oznacza, że znaczniki 60 minut musi występować co 3π jednostki, która jest wartością drugi w `float` tablicy w `minuteMarkPaint`. Znaczniki 12 godzin musi występować co 15π jednostki, która jest wartością w ciągu sekundy `float` tablicy.

`PrettyAnalogClockPage` Klasy ustawia zegar unieważnienie powierzchni co 16 milisekund i `PaintSurface` obsługi jest wywoływana w tym stawki. Definicje wcześniejszych `SKPath` i `SKPaint` obiektów umożliwiają bardzo czysty kod rysowania:

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

Coś specjalne odbywa się z drugiej strony, jednak. Ponieważ zegar jest aktualizowany co 16 milisekund `Millisecond` właściwość `DateTime` wartość potencjalnie może być używana do animowania odchylenia drugi strony zamiast, który umożliwia przeniesienie w odrębny przechodzi z drugiej na sekundę. Ale ten kod nie zezwala na ruch być smooth. Zamiast tego używa platformy Xamarin.Forms [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) i [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) animacji łatwiejszym funkcje dla innego rodzaju ruchu. Te funkcje łatwiejszym spowodować używane do przenoszenia w sposób jerkier &mdash; ponownie ściąganie, trochę przed powoduje przeniesienie, a następnie nieco nadmiernie premia miejsca docelowego, efekt to Niestety nie można odtworzyć te statycznych zrzuty ekranu:

[![](path-data-images/prettyanalogclock-small.png "Potrójna zrzut ekranu przedstawiający stronę Pretty analogowy zegara")](path-data-images/prettyanalogclock-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Pretty analogowy zegara")


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
