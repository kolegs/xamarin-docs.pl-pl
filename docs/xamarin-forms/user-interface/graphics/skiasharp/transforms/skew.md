---
title: Pochylenie — przekształcenie
description: W tym artykule wyjaśniono, jak utworzyć Wychylny obiektów graficznych w SkiaSharp pochylenie — przekształcenie i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: charlespetzold
ms.author: chape
ms.date: 03/20/2017
ms.openlocfilehash: 951fc02dfff1721c1391c5d0c8a21452a156cfdb
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615356"
---
# <a name="the-skew-transform"></a>Pochylenie — przekształcenie

_Zobacz, jak utworzyć Wychylny obiektów graficznych w SkiaSharp pochylenie — przekształcenie_

W SkiaSharp pochylenie — przekształcenie można przechylać graficzne obiekty, takie jak w tle na tej ilustracji:

![](skew-images/skewexample.png "Przykładem pochylanie programu pochylanie tekst z cieniem")

Pochylenia przekształcający parallelograms prostokąty niesymetryczne wielokropka jest jednak nadal elipsę.

Chociaż Xamarin.Forms definiuje właściwości przesunięcia, skalowania i możliwość wymiany, nie ma odpowiedniej właściwości w interfejsie Xamarin.Forms dla przesunięcia czasowego.

[ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/System.Single/System.Single/) Metody `SKCanvas` przyjmuje dwa argumenty w poziomie i pionie pochylanie:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

Sekundy [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/SkiaSharp.SKPoint/) metoda łączy te argumenty w jednym `SKPoint` wartość:

```csharp
public void Skew (SKPoint skew)
```

Jest mało prawdopodobne, że należy używać jednej z tych dwóch metod w izolacji.

**Pochylanie eksperymentować** strona umożliwia wypróbowanie niedokładność wartości od –10 do 10. Ciąg tekstowy jest umieszczany w lewym górnym rogu strony, przy użyciu niesymetryczność wartości z dwóch `Slider` elementów. Oto `PaintSurface` obsługi w [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) klasy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        canvas.Skew((float)xSkewSlider.Value, (float)ySkewSlider.Value);
        canvas.DrawText(text, 0, -textBounds.Top, textPaint);
    }
}
```

Wartości `xSkew` argument shift dolnej części tekstu odpowiednią dla wartości dodatnich lub w lewo dla wartości ujemnych. Wartości `ySkew` w dół po prawej stronie tekstu dla wartości dodatnich lub dla wartości ujemnych:

[![](skew-images/skewexperiment-small.png "Potrójna zrzut ekranu przedstawiający stronę pochylanie eksperymentu")](skew-images/skewexperiment-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę pochylanie eksperymentu")

Jeśli `xSkew` jest negacją `ySkew`, wynik jest obrotu, ale także skalowanych, nieco, ponieważ wskazuje, wyświetlanie platformy uniwersalnej systemu Windows.

Formuły przekształcenia są następujące:

x "= x + xSkew XI y

y "= ySkew XI x + y

Na przykład dla wartości dodatnich `xSkew` wartość przekształcony `x'` wartość zwiększa się jako `y` zwiększa się. To, co powoduje, że pochylenia.

Jeśli trójkąt 200 pikseli szerokości i 100 pikseli znajduje się za pomocą jego lewego górnego rogu w momencie (0, 0) i jest renderowany przy użyciu `xSkew` wartość 1.5, z następującymi wynikami równoległobok:

![](skew-images/skeweffect.png "Efekt pochylenie — przekształcenie na prostokącie")

Współrzędne dolnej krawędzi mają `y` wartości 100, więc jest przesuwany w prawo 150 pikseli.

Wartości niezerowe wiązania `xSkew` lub `ySkew`tylko punkt (0, 0) pozostają bez zmian. Ten punkt jest uznawana za środek pochylanie. Jeśli potrzebujesz środek pochylanie jako coś innego (które zwykle tak jest), nie `Skew` metodę, która stanowi, że. Musisz jawnie połączyć `Translate` wywołań za pomocą `Skew` wywołania. Aby wyśrodkować pochylanie na `px` i `py`, następujące wywołania:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Formuły złożone przekształcenia są:

x "= x + xSkew XI (y — py)

y "= ySkew XI (x-px) + y

Jeśli `ySkew` wynosi zero, a wartość różna od zera tylko określać `xSkew`, następnie `px` wartość nie jest używana. Wartość nie ma znaczenia i podobnie dla `ySkew` i `py`.

Może być czujesz bardziej określenie niesymetryczność jako wartość kąta pochylenie, takich jak kąt α na tym rysunku:

![](skew-images/skewangleeffect.png "Wskazane temat prostokąt o kąt pochylanie pochylenie — przekształcenie")

Stosunek shift 150 pikseli, aby w pionie 100 pikseli jest tangens kąta, w tym przykładzie 56.3 stopni.

Plik XAML **pochylanie eksperymentu kąt** strona jest podobna do **pochylanie kąt** strony, chyba że `Slider` elementy do zakresu od – 90 do 90 stopni. [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) Pliku związanego z kodem wyśrodkowanie tekstu na stronie i używa `Translate` można ustawić środek pochylanie do środkowej części strony. Krótki `SkewDegrees` metoda w dolnej części kodu konwertuje kąty fałszować wartości:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;

        canvas.Translate(xCenter, yCenter);
        SkewDegrees(canvas, xSkewSlider.Value, ySkewSlider.Value);
        canvas.Translate(-xCenter, -yCenter);
        canvas.DrawText(text, xText, yText, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

Jak kąt zbliża się do 90 stopni dodatnie lub ujemne, tangens zbliża się do nieskończoności, ale kąty do około 80 stopni, lub tak nadają się do:

[![](skew-images/skewangleexperiment-small.png "Potrójna zrzut ekranu przedstawiający stronę pochylanie eksperymentu kąt")](skew-images/skewangleexperiment-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę pochylanie eksperymentu kąt")

Małe ujemna pochylenie w poziomie można naśladować nachylone lub kursywą tekstu, jako **nachylone tekstu** pokazuje strony. [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) Klasy pokazuje, jak to zrobić:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Maroon,
        TextAlign = SKTextAlign.Center,
        TextSize = info.Width / 8       // empirically determined
    })
    {
        canvas.Translate(info.Width / 2, info.Height / 2);
        SkewDegrees(canvas, -20, 0);
        canvas.DrawText(Title, 0, 0, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

`TextAlign` Właściwość `SKPaint` ustawiono `Center`. Bez jakichkolwiek plików transformacji `DrawText` Wywołaj za pomocą współrzędnych (0, 0) czy umieść tekst między znakami dwóch środka linii bazowej w lewym górnym rogu. `SkewDegrees` Pochyla tekstu w poziomie 20 stopni względem punktu odniesienia. `Translate` Wywołania przenosi do środka linii bazowej tekstu na środku kanwy:

[![](skew-images/obliquetext-small.png "Potrójna zrzut ekranu przedstawiający stronę nachylone tekstu")](skew-images/obliquetext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę nachylone tekstu")

**Pochylanie tekst w tle** strony pokazuje, jak użyć kombinacji skali pochylenia i w pionie 45 stopni, aby wprowadzić tekst w tle, który można przechylać od tekstu. Oto odpowiednie części `PaintSurface` procedury obsługi:

```csharp
using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = info.Width / 6;   // empirically determined

    // Common to shadow and text
    string text = "Shadow";
    float xText = 20;
    float yText = info.Height / 2;

    // Shadow
    textPaint.Color = SKColors.LightGray;
    canvas.Save();
    canvas.Translate(xText, yText);
    canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
    canvas.Scale(1, 3);
    canvas.Translate(-xText, -yText);
    canvas.DrawText(text, xText, yText, textPaint);
    canvas.Restore();

    // Text
    textPaint.Color = SKColors.Blue;
    canvas.DrawText(text, xText, yText, textPaint);
}
```

Kopii w tle jest wyświetlane najpierw, a następnie tekst:

[![](skew-images/skewshadowtext1-small.png "Potrójna zrzut ekranu przedstawiający stronę pochylanie tekst w tle")](skew-images/skewshadowtext1-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę pochylanie tekst z cieniem")

Współrzędna pionowy przekazany do `DrawText` metoda wskazuje położenie tekstu względem punktu odniesienia. To tę samą współrzędną pionowy, używany do środka pochylanie. Ta technika nie będzie działać, jeśli ciąg tekstowy zawiera wydłużeń. Na przykład słowo "quirky", "Cienia" i tutaj subsitute użytkownika wynik:

[![](skew-images/skewshadowtext2-small.png "Potrójna zrzut ekranu strony pochylanie tekst w tle wyrazami alternatywnych, z wydłużeń")](skew-images/skewshadowtext2-large.png#lightbox "Potrójna zrzut ekranu strony pochylanie tekst w tle wyrazami alternatywnych, z wydłużeń")

W tle i tekst nadal są wyrównane w linii bazowej, ale efekt po prostu jest nieprawidłowy. Aby rozwiązać ten problem, należy uzyskać granice tekstu:

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

`Translate` Wywołania, które muszą zostać dostosowane przez wysokość wydłużeń:

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

Teraz cień rozszerza się w dolnej części tych wydłużeń:

[![](skew-images/skewshadowtext3-small.png "Potrójna zrzut ekranu strony pochylanie tekst w tle za pomocą dostosowania do wydłużeń")](skew-images/skewshadowtext3-large.png#lightbox "Potrójna zrzut ekranu strony pochylanie tekst w tle za pomocą dostosowania do wydłużenia dolne")


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
