---
title: Integrowanie tekstu i grafiki
description: W tym artykule wyjaśniono, jak określić rozmiar ciągu tekstowego renderowanych do integracji tekstu z grafiką SkiaSharp w aplikacjach Xamarin.Forms i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: be1524029ada79896f83517c3b439f2ad0e2c6d9
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615164"
---
# <a name="integrating-text-and-graphics"></a>Integrowanie tekstu i grafiki

_Zobacz, jak określić rozmiar ciągu tekstowego renderowanych zintegrować tekstu z grafiką SkiaSharp_

W tym artykule pokazano, jak mierzyć tekstu, prawdopodobnie skalowania tekstu do określonego rozmiaru i integrowanie tekstu z innych graficznych:

![](text-images/textandgraphicsexample.png "Tekst otoczony prostokątów")

Skiasharp — `Canvas` klasa zawiera także metody, aby narysować prostokąt ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) i prostokąta z zaokrąglonymi rogami ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). Te metody wymagają prostokąt można zdefiniować jako `SKRect` wartość.

**Tekstu w ramce** strony centra krótki ciąg tekstowy na stronie i otacza go za pomocą ramki składa się z pary zaokrąglonych. [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) Klasy pokazuje, jak to się robi.

W SkiaSharp `SKPaint` klasy zestawu atrybutów tekstu i czcionek, ale również służy do uzyskiwania renderowanych rozmiar tekstu. Początek następujące `PaintSurface` program obsługi zdarzeń wywołuje dwie różne `MeasureText` metody. Pierwszy [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) wywołanie ma prostą `string` argument i zwraca szerokość piksela tekstu na podstawie bieżącej czcionki atrybutów. Następnie obliczana nową `TextSize` właściwość `SKPaint` obiektu oparte na tym renderowanych szerokość bieżącego `TextSize` właściwość i szerokości obszaru wyświetlania. Ta wartość jest przeznaczona do zestawu `TextSize` tak, aby ciąg znaków do renderowania 90% szerokości ekranu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string str = "Hello SkiaSharp!";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Chocolate
    };

    // Adjust TextSize property so text is 90% of screen width
    float textWidth = textPaint.MeasureText(str);
    textPaint.TextSize = 0.9f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

Drugi [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) wywołanie ma `SKRect` argument, dzięki czemu uzyskuje szerokość i wysokość tekstu renderowanego. `Height` Właściwości tego `SKRect` wartość zależy od obecności wielkimi literami, wydłużenia górne i dolne w ciągu tekstowym. Różne `Height` wartości są zgłaszane dla tekstu ciągi "programu Microsoft Operations Manager", "cat" i "dog", na przykład.

`Left` i `Top` właściwości `SKRect` struktury wskazują współrzędne lewego górnego rogu renderowanego tekstu, jeśli tekst jest wyświetlany przez `DrawText` wywołanie z pozycji 0 X i Y. Na przykład, gdy ten program jest uruchomiona na symulatora telefonu iPhone 7, `TextSize` jest przypisywana wartość 90.6254 wyniku obliczenia po pierwszym wywołaniu `MeasureText`. `SKRect` Uzyskaną z drugie wywołanie `MeasureText` ma następujące wartości właściwości:

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

Należy pamiętać, że współrzędne X i Y możesz przekazać do `DrawText` metoda Określ po lewej stronie tekstu w linii bazowej. `Top` Wartość wskazuje, że tekst rozszerza 68 piksele powyżej tej linii bazowej i (odjęcie 68 it przy użyciu 88) 20 pikseli poniżej linii bazowej. `Left` Wartość 6 Wskazuje, czy tekst rozpoczyna się 6 pikseli na prawo od wartości X w `DrawText` wywołania. Umożliwia to normalne odstępy między znakami. Jeśli chcesz wyświetlić tekst prawidłowo osadzone w lewym górnym rogu ekranu, należy przekazać negatywów tych `Left` i `Top` wartości jako współrzędne X i Y `DrawText`, w tym przykładzie &ndash;6 i 68.

`SKRect` Struktury definiuje kilka przydatną właściwości i metody, z których niektóre są używane w dalszej części `PaintSurface` programu obsługi. `MidX` i `MidY` wartości wskazują współrzędne środek prostokąta. (W tym przykładzie iPhone 7, te wartości są 338.4107 i &ndash;24.) W poniższym kodzie użyto tych wartości najprostszym obliczania współrzędne, aby wyśrodkować tekst na ekranie:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(str, xText, yText, textPaint);
    ...
}
```

`PaintSurface` Obsługi stwierdza, przy użyciu dwóch wywołań `DrawRoundRect`, które wymagają argumentów `SKRect`. To `SKRect` wartość przypomina bez obaw `SKRect` uzyskaną z `MeasureText` metody, ale nie może być taka sama. Najpierw musi to być nieco większy prostokąt zaokrąglony nie Rysuj za pośrednictwem krawędzi tekstu, a po drugie, potrzebny jest przesuwany w miejscu, aby `Left` i `Top` wartości odpowiadają lewego górnego rogu, w którym ma być prostokąta umieszczony. Te dwa zadania są wykonywane przez `Offset` i `Inflate` metody zdefiniowane przez `SKRect`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Create a new SKRect object for the frame around the text
    SKRect frameRect = textBounds;
    frameRect.Offset(xText, yText);
    frameRect.Inflate(10, 10);

    // Create an SKPaint object to display the frame
    SKPaint framePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5,
        Color = SKColors.Blue
    };

    // Draw one frame
    canvas.DrawRoundRect(frameRect, 20, 20, framePaint);

    // Inflate the frameRect and draw another
    frameRect.Inflate(10, 10);
    framePaint.Color = SKColors.DarkBlue;
    canvas.DrawRoundRect(frameRect, 30, 30, framePaint);
}
```

Poniżej pozostałą część metody jest proste. Tworzy, kolejny `SKPaint` obiekt obramowanie i wywołania `DrawRoundRect` dwa razy. Drugie wywołanie używa prostokąt zwiększony o innym 10 pikseli. Pierwsze wywołanie określa promienia narożnika 20 pikseli; drugi ma promienia narożnika 30 pikseli, więc one wydaje się być równoległego:

 [![](text-images/framedtext-small.png "Potrójna zrzut ekranu przedstawiający stronę tekstu w ramce")](text-images/framedtext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę tekstu w ramce")

Obrócić telefon lub symulatora w bok, aby wyświetlić tekst i ramki zwiększają swoich rozmiarów.

Jeśli wymagane jest tylko część tekstu na ekranie center, możesz zrobić to około bez pomiaru tekstu, ustawiając `TextAlign` właściwość `SKPaint` do `SKTextAlign.Center`. Współrzędna X w `DrawText` metoda następnie wskazuje, gdzie jest umieszczony środka tekstu. W przypadku przekazania punktu środkowego ekranu, aby `DrawText` metody, tekst będzie wyśrodkowany w poziomie i *niemal* wyśrodkowane w pionie ponieważ linii bazowej zostaną wyśrodkowane w pionie.

Sam tekst mogą być traktowane wiele opcji graficznych. Jedną z opcji proste jest wyświetlanie konspektu znaki tekstowe, a nie na normalnej wypełniony wyświetlania:

[![](text-images/outlinedtext-small.png "Potrójny zrzut ekranu strony opisane tekstu")](text-images/outlinedtext-large.png#lightbox "Potroiliśmy zrzut ekranu strony opisane tekstu")

Ma to po prostu zmieniając normalną `Style` właściwość `SKPaint` obiektu na podstawie ustawień domyślnych `SKPaintStyle.Fill` do `SKPaintStyle.Stroke` i określając szerokość pociągnięcia. `PaintSurface` Program obsługi **opisane tekstu** strony pokazuje, jak to zrobić:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "OUTLINE";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 1,
        FakeBoldText = true,
        Color = SKColors.Blue
    };

    // Adjust TextSize property so text is 95% of screen width
    float textWidth = textPaint.MeasureText(text);
    textPaint.TextSize = 0.95f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 W ciągu ostatnich kilku artykułach użytkownik, do których masz teraz już, jak rysowanie tekstu za pomocą SkiaSharp kół, wielokropek i prostokąty zaokrąglony. Następnym krokiem jest [skiasharp — linie i ścieżki](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) w którym nauczysz do rysowania linii połączonych w ścieżce grafiki.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
