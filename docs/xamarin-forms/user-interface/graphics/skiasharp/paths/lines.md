---
title: Linie i zakończenia pociągnięć
description: W tym artykule wyjaśniono, jak używać SkiaSharp Rysowanie linii za pomocą różnych pociągnięć w aplikacjach Xamarin.Forms i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 000bf24c1b06baab892f0b165c8b9eeebebce49d
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615265"
---
# <a name="lines-and-stroke-caps"></a>Linie i zakończenia pociągnięć

_Dowiedz się, jak używać SkiaSharp Rysowanie linii za pomocą różnych pociągnięć_

SkiaSharp renderowanie pojedynczy wiersz jest bardzo różnią się od renderowania szereg połączone linie w proste. Nawet podczas rysowania pojedynczej linii, jednak często jest to wymagane, aby dać wiersze szerokość pociągnięcia określonego i szerszy wiersza, niezwykle ważne staje się wygląd końca linii o nazwie *pociągnięcia*:

![](lines-images/strokecapsexample.png "Opcje caps trzy pociągnięcia")

Rysowania pojedynczej linii `SKCanvas` definiuje prosty [ `DrawLine` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawLine/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) metody, w której argumenty wskazują początkową i końcową współrzędne wiersz z `SKPaint` obiektu:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

Domyślnie `StrokeWidth` nowo utworzona właściwość `SKPaint` obiektu ma wartość 0, która ma taki sam skutek jak wartość 1 w renderowania linię o jeden piksel w grubości. Ta opcja ma nazwę bardzo alokowania elastycznego na urządzeniach o wysokiej rozdzielczości, takich jak telefony, więc prawdopodobnie będziesz chciał ustaw `StrokeWidth` większej wartości. Po rozpoczęciu Rysowanie linii o sporej liczbie grubości, który wywołuje inny problem, ale: jak rozpoczyna się i kończy się tych grubości linii będą renderowane?

Wygląd rozpoczyna się i końców wierszy jest nazywany *zakończenia wiersza* lub Skia, *pociągnięcia*. Słowo "limit", w tym kontekście, który odwołuje się do typu hat &mdash; coś, co znajduje się na końcu wiersza. Możesz ustawić [ `StrokeCap` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeCap/) właściwość `SKPaint` obiektu do jednej z następujących członków [ `SKStrokeCap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeCap/) wyliczenia:

- [`Butt`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Butt/) (ustawienie domyślne)
- [`Square`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)

Zostały one najlepiej zilustrowane konkretnymi przykładowy program. W drugiej części strony głównej [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program zaczyna się od stronę zatytułowaną **pociągnięć** na podstawie [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) klasy. Ta strona definiuje `PaintSurface` obsługi zdarzeń, który przetwarza trzech członków w pętli `SKStrokeCap` wyliczenia, wyświetlając nazwę elementu członkowskiego wyliczenia i rysowanie linii za pomocą tego pociągnięcia:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Center
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width / 2;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = textPaint.FontSpacing;

    foreach (SKStrokeCap strokeCap in Enum.GetValues(typeof(SKStrokeCap)))
    {
        // Display text
        canvas.DrawText(strokeCap.ToString(), xText, y, textPaint);
        y += textPaint.FontSpacing;

        // Display thick line
        thickLinePaint.StrokeCap = strokeCap;
        canvas.DrawLine(xLine1, y, xLine2, y, thickLinePaint);

        // Display thin line
        canvas.DrawLine(xLine1, y, xLine2, y, thinLinePaint);
        y += 2 * textPaint.FontSpacing;
    }
}
```

Dla każdego elementu członkowskiego `SKStrokeCap` wyliczenie, program obsługi rysuje dwa wiersze, jeden z grubość obrysu 50 pikseli i innego wiersza umieszczone na górze z grubość obrysu 2 pikseli. Ten drugi wiersz jest za zadanie zilustrowanie geometrycznych początku i końca wiersza, które są niezależne od grubości linii i pociągnięcia:

[![](lines-images/strokecaps-small.png "Potrójna zrzut ekranu przedstawiający stronę pociągnięć")](lines-images/strokecaps-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę pociągnięć")

Jak widać, `Square` i `Round` pociągnięć skutecznie wydłużyć wiersza o połowę szerokości obrysu na początku wiersza i ponownie na końcu. To rozszerzenie staje się ważne, gdy jest to konieczne określić wymiary obiektu graficznego renderowany.

`SKCanvas` Klasa zawiera także innej metody rysowania wiele wierszy, który jest nieco szczególna:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points` Parametr jest tablicą `SKPoint` wartości i `mode` jest elementem członkowskim [ `SKPointMode` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPointMode/) wyliczenia, która ma trzy elementy członkowskie:

- [`Points`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Points/) Aby renderować poszczególnych punktów
- [`Lines`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Lines/) do łączenia z każdej pary punktów
- [`Polygon`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Polygon/) Aby połączyć wszystkie kolejne punkty

**Wiele wierszy** strona przedstawia tę metodę. [ `MultipleLinesPage` Pliku XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) są tworzone wystąpienia dwóch `Picker` widoków, które pozwalają wybrać członkiem `SKPointMode` wyliczenie i członkiem `SKStrokeCap` wyliczenia:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.MultipleLinesPage"
             Title="Multiple Lines">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="pointModePicker"
                Title="Point Mode"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Points</x:String>
                <x:String>Lines</x:String>
                <x:String>Polygon</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

`SelectedIndexChanged` Obsługi dla obu `Picker` widoków po prostu unieważnia `SKCanvasView` obiektu:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Ten program obsługi wymaga pod kątem istnienia `SKCanvasView` obiektu, ponieważ program obsługi zdarzeń jest pierwszy wywoływane, gdy `SelectedIndex` właściwość `Picker` jest ustawiona na 0 w pliku XAML występuje przed `SKCanvasView` został uruchomiony.

`PaintSurface` Obsługi uzyskuje dostęp do metody ogólnej dla dwóch wybranych pozycji od uzyskiwania `Picker` widoków i konwertowania go na potrzeby wartości wyliczenia:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create an array of points scattered through the page
    SKPoint[] points = new SKPoint[10];

    for (int i = 0; i < 2; i++)
    {
        float x = (0.1f + 0.8f * i) * info.Width;

        for (int j = 0; j < 5; j++)
        {
            float y = (0.1f + 0.2f * j) * info.Height;
            points[2 * j + i] = new SKPoint(x, y);
        }
    }

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.DarkOrchid,
        StrokeWidth = 50,
        StrokeCap = GetPickerItem<SKStrokeCap>(strokeCapPicker)
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = GetPickerItem<SKPointMode>(pointModePicker);
    canvas.DrawPoints(pointMode, points, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }
    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}
```

Na zrzucie ekranu przedstawiono szereg `Picker` wybrane opcje na trzech platformach:

[![](lines-images/multiplelines-small.png "Potrójna zrzut ekranu przedstawiający stronę wiele wierszy")](lines-images/multiplelines-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę wiele wierszy")

IPhone w po lewej stronie pokazuje sposób, w jaki `SKPointMode.Points` powoduje, że element członkowski wyliczenia `DrawPoints` do każdego z punktów w renderowania `SKPoint` tablicy jako kwadrat w przypadku zakończenia wiersza `Butt` lub `Square`. Okręgów są renderowane w przypadku zakończenia wiersza `Round`.

Jeśli zamiast tego użyć `SKPointMode.Lines`, jak pokazano na ekranie dla systemu Android w Centrum `DrawPoints` metoda rysuje linię między sąsiednimi `SKPoint` przy użyciu limitu określonego wiersza, w tym przypadku `Round`.

Zrzut ekranu platformy uniwersalnej systemu Windows zawiera wynik `SKPointMode.Polygon` wartość. Linia jest rysowana między kolejnymi punktami w tablicy, ale można spojrzeć ściśle, zobaczysz, że następujące wiersze nie są połączone. Każda z tych osobnych wierszach rozpoczynający się i kończący limitu określonego wiersza. Jeśli wybierzesz `Round` limity, wiersze mogą być wyświetlane jest połączony, ale są naprawdę nie nawiązano.

Czy wiersze są połączony lub niepołączony jest kluczowym aspektem pracy ze ścieżki grafiki.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
