---
title: Przekształcanie pochylenia
description: Zobacz, jak utworzyć Wychylny obiektów graficznych w SkiaSharp pochylenia przekształcenie
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: charlespetzold
ms.author: chape
ms.date: 03/20/2017
ms.openlocfilehash: 39547ebaf301a9b6dca6a90cb5ede831b19862cf
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/05/2018
---
# <a name="the-skew-transform"></a>Przekształcanie pochylenia

_Zobacz, jak utworzyć Wychylny obiektów graficznych w SkiaSharp pochylenia przekształcenie_

W SkiaSharp pochylenia przekształcenia można przechylać obiektów graficznych, takich jak cień do tego obrazu:

![](skew-images/skewexample.png "Przykład pochylanie z programu pochylanie cienia tekstu")

Pochylenie przekształca prostokąty w parallelograms spowodowałoby zafałszowanie elipsy jest jednak nadal elipsy.

Chociaż platformy Xamarin.Forms definiuje właściwości tłumaczenia, skalowanie i obrotów, nie ma odpowiedniej właściwości w platformy Xamarin.Forms dla zegara.

[ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/System.Single/System.Single/) Metody `SKCanvas` przyjmuje dwa argumenty w poziomie i pionie pochylanie:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

Drugi [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/SkiaSharp.SKPoint/) metody łączy tych argumentów w jednej `SKPoint` wartość:

```csharp
public void Skew (SKPoint skew)
```

Jednak jest mało prawdopodobne, że należy używać jednej z tych dwóch metod w izolacji.

**Pochylanie eksperymentu** strona umożliwia eksperymentować zegara wartości od –10 do 10. Ciąg tekstowy znajduje się w lewym górnym rogu strony, wartościami pochylenia uzyskane z dwóch `Slider` elementów. Oto `PaintSurface` obsługi w [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) klasy:

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

Wartości `xSkew` argumentu przesunięcia w dolnej części tekstu prawa dla wartości dodatnie lub pozostać dla wartości ujemnych. Wartości `ySkew` w dół po prawej stronie tekstu dla wartości dodatnie lub dla wartości ujemnych:

[![](skew-images/skewexperiment-small.png "Potrójna zrzut ekranu przedstawiający stronę pochylanie eksperymentu")](skew-images/skewexperiment-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę pochylanie eksperymentu")

Jeśli `xSkew` jest ujemna z `ySkew`, wynik jest obracanie, ale również skalować nieco jako wskazuje wyświetlania systemu Windows.

Przekształcanie formuły są następujące:

x "= x + xSkew · y

y "= ySkew · x + y

Na przykład w przypadku dodatnią `xSkew` wartości, po przekształceniu `x'` wartość zwiększa jako `y` zwiększa. To, co powoduje pochylenia.

Jeśli trójkąt 200 pikseli szerokości i wysokości 100 pikseli znajduje się z jego lewego górnego rogu w punkcie (0, 0) i jest odwzorowywany z `xSkew` wartość 1.5, równoległobok następujące wyniki:

![](skew-images/skeweffect.png "Efekt pochylenia transformacji w prostokącie")

Współrzędne dolnej krawędzi ma `y` wartości od 100, dzięki czemu przesunięte 150 pikseli z prawej strony.

Dla wartości innej niż zero `xSkew` lub `ySkew`tylko punkt (0, 0) jest taka sama. Ten punkt jest uznawana za środek pochylanie. Czy potrzebujesz środek pochylanie jako inny (co jest zazwyczaj sytuacją), nie `Skew` metoda, która zapewnia, że. Musisz jawnie połączyć `Translate` wywołania z `Skew` wywołania. Aby wyśrodkować pochylanie na `px` i `py`, wywołań następujące:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Formuły złożone przekształcenia są:

x "= x + xSkew · (y — py)

y "= ySkew · (x — pikseli) + y

Jeśli `ySkew` wynosi zero, a tylko w przypadku określania wartość niezerową `xSkew`, następnie `px` wartość nie jest używana. Wartość nie ma znaczenia i podobnie dla `ySkew` i `py`.

Może uznać wygodniejsze Określanie pochylenia jako kąt nachylenia, takich jak kąt α na tym diagramie:

![](skew-images/skewangleeffect.png "Wskazane wpływ pochylenia przekształcenia na prostokąt z pochylanie kąta")

Stosunek shift 150 pikseli na pionowy 100 pikseli jest tangens kąta, w tym przykładzie 56.3 stopni.

Plik XAML **pochylanie eksperymentu kąt** strona jest podobna do **pochylanie kąt** strony z wyjątkiem `Slider` elementy należeć do zakresu od – 90 do 90 stopni. [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) Pliku CodeBehind Wyśrodkowuje tekst na stronie i używa `Translate` można ustawić środka pochylanie na środku strony. Krótki `SkewDegrees` metoda w dolnej części kodu konwertuje kąty fałszować wartości:

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

Jako wartość kąta zbliża się do 90 stopni dodatnie lub ujemne, tangens zbliża się do nieskończoności, ale kąty do około 80 stopni lub dlatego nadają się do:

[![](skew-images/skewangleexperiment-small.png "Potrójna zrzut ekranu przedstawiający stronę pochylanie eksperymentu kąt")](skew-images/skewangleexperiment-large.png#lightbox "Potrójna zrzut ekranu strony pochylanie eksperymentu kąta")

Mała ujemna zegara poziomy można naśladować nachylone lub kursywą tekstu, jako **nachylone tekst** pokazuje strony. [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) Klasy pokazuje, jak jest wykonywane:

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

`TextAlign` Właściwość `SKPaint` ma ustawioną wartość `Center`. Bez transformacje `DrawText` wywołania z współrzędne (0, 0) czy umieścić tekst w Centrum poziomych linii bazowej w lewym górnym rogu. `SkewDegrees` Pochyla tekst poziomo 20 stopni względem linii bazowej. `Translate` Wywołania przenosi Centrum poziomych linii bazowej tekstu do centrum obszaru roboczego:

[![](skew-images/obliquetext-small.png "Potrójna zrzut ekranu przedstawiający stronę nachylone tekst")](skew-images/obliquetext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę nachylone tekstu")

**Pochylanie cienia tekstu** strony pokazano, jak dokonanie Cień tekstu, który można przechylać przeciwną tekst jest użycie kombinacji skali pochylenia i w pionie 45 stopni. Poniżej przedstawiono istotne część `PaintSurface` obsługi:

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

Kopii w tle jest najpierw wyświetlany, a następnie tekst:

[![](skew-images/skewshadowtext1-small.png "Potrójna zrzut ekranu przedstawiający stronę pochylanie cienia tekstu")](skew-images/skewshadowtext1-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę pochylanie cienia tekstu")

Współrzędna pionowy przekazany do `DrawText` metody wskazuje pozycję tekstu względem linii bazowej. To jest tę samą współrzędną pionowy używany do środka pochylanie. Ta metoda nie będzie działać, jeśli ciąg tekstowy zawiera wydłużeń dolnych. Na przykład subsitute wyraz "quirky", "Cienia" i miejsca jego wynik:

[![](skew-images/skewshadowtext2-small.png "Potrójna zrzut ekranu strony pochylanie cienia tekstu z alternatywnych word z wydłużeń dolnych")](skew-images/skewshadowtext2-large.png#lightbox "Potrójna zrzut ekranu strony pochylanie cienia tekstu z alternatywnych word z wydłużenia dolne")

Cień i tekst jest wyrównywany w linii bazowej, ale wpływ właśnie jest nieprawidłowy. Aby go rozwiązać, należy uzyskać granice tekstu:

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

`Translate` Wywołania muszą być zmienione przez wysokość dolnej:

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

Teraz cień rozciąga się od dołu tych wydłużeń dolnych:

[![](skew-images/skewshadowtext3-small.png "Potrójna zrzut ekranu przedstawiający stronę pochylanie cienia tekstu korekt dla wydłużeń dolnych")](skew-images/skewshadowtext3-large.png#lightbox "Potrójna zrzut ekranu strony pochylanie cienia tekstu korekt dla wydłużenia dolne")


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
