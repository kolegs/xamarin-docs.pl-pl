---
title: Integrowanie tekstu i grafiki
description: Informacje o określaniu rozmiar ciągu tekstowego renderowanych tekst jest integrowana z SkiaSharp grafiki
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 1607fe31785b6793175dfb61e1e12e34429aa089
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2018
---
# <a name="integrating-text-and-graphics"></a>Integrowanie tekstu i grafiki

_Informacje o określaniu rozmiar ciągu tekstowego renderowanych tekst jest integrowana z SkiaSharp grafiki_

W tym artykule przedstawiono sposób pomiaru tekstu, prawdopodobnie Skalowanie tekstu do określonego rozmiaru i integracji z innymi grafiki tekst:

![](text-images/textandgraphicsexample.png "Tekst otoczony prostokątów")

SkiaSharp `Canvas` klasa zawiera również metody, aby narysować prostokąt ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) i prostokąt z zaokrąglonymi narożnikami ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). Te metody wymagają prostokąt określone jako `SKRect` wartość.

**Tekstu w ramce** krótki ciąg tekstowy na stronie i obudowy z ramką składa z para prostokątów zaokrąglonych Wyśrodkowuje strony. [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) Klasy pokazuje, jak jest wykonywane.

W SkiaSharp użyjesz `SKPaint` klasy zestawu atrybutów tekstu i czcionek, ale można również wykorzystać do uzyskania renderowany rozmiarem tekstu. Na początku następujące `PaintSurface` obsługi zdarzeń wymaga dwóch różnych `MeasureText` metody. Pierwszy [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) wywołania ma prostą `string` argument i zwraca szerokość piksela tekstu na podstawie bieżącej czcionki atrybutów. Następnie program oblicza nowy `TextSize` właściwość `SKPaint` obiektu oparte na tym renderowanych szerokość bieżącego `TextSize` właściwości i szerokości obszaru wyświetlania. Ta wartość jest przeznaczona do ustawienia `TextSize` tak, aby tekst ciąg, który ma być renderowany 90% szerokości ekranu:

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
    SKRect textBounds;
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

Drugi [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) wywołanie `SKRect` argumentu, więc uzyska zarówno szerokość i wysokość renderowanych tekstu. `Height` Właściwości tego `SKRect` wartość zależy od obecności wielkimi literami, wydłużenia górne i dolne w ciągu tekstowym. Różne `Height` wartości są raportowane dla tekstu ciągów "programu Microsoft Operations Manager", "kot" i "kot", np.

`Left` i `Top` właściwości `SKRect` struktury wskazują współrzędne górnego lewego rogu renderowanego tekstu, jeżeli tekst jest wyświetlany przez `DrawText` wywołania z pozycji 0 X i Y. Na przykład, gdy ten program jest uruchomiony w symulatorze iPhone 7 `TextSize` jest przypisywana wartość 90.6254 wyniku obliczania po pierwszym wywołaniu `MeasureText`. `SKRect` Wartość uzyskane z drugie wywołanie `MeasureText` ma następujące wartości właściwości:

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

Należy pamiętać, że współrzędne X i Y należy przekazać do `DrawText` metody Określ lewej stronie tekstu w linii bazowej. `Top` Wartość oznacza, że tekst rozszerza 68 pikseli powyżej tej linii bazowej i (odjęcie 68 it z 88) 20 pikseli poniżej linii bazowej. `Left` Wartość 6 oznacza, że tekst rozpoczyna się 6 pikseli w prawo w wartości X `DrawText` wywołania. Umożliwia to normalne odstępów między znakami. Jeśli chcesz wyświetlić tekst prawidłowo osadzone w lewym górnym rogu ekranu, Przekaż negatywów tych `Left` i `Top` wartości jak współrzędne X i Y `DrawText`, w tym przykładzie &ndash;6 i 68.

`SKRect` Struktury definiuje kilka przydatną właściwości i metody, niektóre z nich są używane w pozostałej części `PaintSurface` programu obsługi. `MidX` i `MidY` wartości wskazują współrzędne Centrum prostokąta. (W tym przykładzie iPhone 7, te wartości są 338.4107 i &ndash;24.) W poniższym kodzie użyto tych wartości obliczania najprostszym współrzędnych do środka tekstu na wyświetlaczu:

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

`PaintSurface` Obsługi zawiera dwa wywołania `DrawRoundRect`, które wymagają argumenty `SKRect`. To `SKRect` wartość przypomina pewnością `SKRect` wartość uzyskane z `MeasureText` metody, ale nie mogą być takie same. Po pierwsze, musi on być nieco większa zaokrąglony prostokąt nie Rysuj nad krawędzi tekstu, a po drugie, musi on zostać przesunięty w przestrzeni, aby `Left` i `Top` wartości odpowiadają w lewym górnym rogu, w którym ma być prostokąta ustawiony. Te dwa zadania są realizowane przez `Offset` i `Inflate` metody zdefiniowane przez `SKRect`:

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

Po tym, że pozostała metody jest proste. Tworzy innego `SKPaint` obiektu obramowania i wywołania `DrawRoundRect` dwa razy. Drugie wywołanie używa prostokąt zwiększony o innym 10 pikseli. Pierwsze wywołanie określa promień narożnika 20 pikseli; drugi ma promień narożnika 30 pikseli, więc ich wydają się być równoległe:

 [![](text-images/framedtext-small.png "Potrójna zrzut ekranu strony tekstu w ramce")](text-images/framedtext-large.png#lightbox "Potrójna zrzut ekranu strony tekstu w ramce")

Można włączyć swój telefon lub symulator bok tekstu i zwiększyć rozmiar ramki.

Jeśli wymagane jest tylko część tekstu na ekranie Centrum, możesz zrobić to około bez pomiaru tekst przez ustawienie `TextAlign` właściwość `SKPaint` do `SKTextAlign.Center`. Współrzędna X, określ w `DrawText` metoda następnie wskazuje, gdzie znajduje się wyśrodkowanego tekstu. W przypadku przekazania punkt środkowy ekranu, aby `DrawText` metody tekst będzie wyśrodkowany w poziomie i *niemal* wyśrodkowane w pionie ponieważ linii bazowej zostaną wyśrodkowane w pionie.

Sam tekst może być znacznie traktowana jak graficzną opcję. Jedną z opcji proste jest wyświetlanie konspektu znaków, a nie normalne wyświetlania wypełniony:

[![](text-images/outlinedtext-small.png "Potrójne zrzut ekranu strony opisane tekst")](text-images/outlinedtext-large.png#lightbox "potrójne zrzut ekranu strony opisane tekstu")

Ma to zmieniając po prostu normalnej `Style` właściwość `SKPaint` obiektu na podstawie ustawień domyślnych `SKPaintStyle.Fill` do `SKPaintStyle.Stroke` i określając szerokość pociągnięć. `PaintSurface` Obsługi **opisane tekst** strony pokazuje, jak jest wykonywane:

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
    SKRect textBounds;
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 W ciągu ostatnich kilku artykuły które masz teraz pokazano sposób użycia SkiaSharp Rysowanie tekstu, okręgi, elipsy i prostokąty zaokrąglone. Następnym krokiem jest [SkiaSharp wierszy i ścieżki](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) , w którym wyjaśniono, jak do rysowania linii połączonych w ścieżce grafiki.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
