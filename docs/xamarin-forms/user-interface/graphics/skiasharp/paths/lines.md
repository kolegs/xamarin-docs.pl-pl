---
title: Linie i obrysu CAP
description: "Dowiedz się, jak używać SkiaSharp do rysowania linii z informacji o różnych możliwościach"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 341d850709ff27f4dc397cee3bb2fc5f73c0ec3c
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="lines-and-stroke-caps"></a>Linie i obrysu CAP

_Dowiedz się, jak używać SkiaSharp do rysowania linii z informacji o różnych możliwościach_

SkiaSharp renderowania pojedynczy wiersz jest bardzo różnią się od renderowania szereg połączone linie proste. Nawet podczas rysowania pojedynczych wierszy, jednak często jest to wymagane, aby dać wiersze określonego grubość i szersze wiersza, im ważniejsze staje się wygląd końca linii o nazwie *koniec obrysu*:

![](lines-images/strokecapsexample.png "Opcje caps trzy obrysu")

Na rysowanie linii pojedynczego `SKCanvas` definiuje prosty [ `DrawLine` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawLine/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) którego argumenty wskazuje początkową i końcową współrzędne wiersz z metody `SKPaint` obiektu:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

Domyślnie `StrokeWidth` właściwość nowo skonkretyzowanym `SKPaint` obiektu wynosi 0, który ma ten sam efekt co wartość 1 w renderowania grubość linii jeden piksel. Pojawia się bardzo alokowania na urządzeniach wysokiej rozdzielczości, takich jak telefony, więc prawdopodobnie należy ustawić `StrokeWidth` większej wartości. Jednak po uruchomieniu Rysowanie linii grubość może być zmieniany, który zgłasza innego problemu: sposób rozpoczęcia i zakończenia tych grubości linii będą renderowane?

Wygląd rozpoczęcia i zakończenia wierszy jest nazywany *zakończenie linii* lub w Skia, *koniec obrysu*. Wyraz "zakończenia" w tym kontekście, który odwołuje się do typu hat &mdash; coś, która znajduje się na końcu linii. Możesz ustawić [ `StrokeCap` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeCap/) właściwość `SKPaint` obiekt na jedno z następujących członków [ `SKStrokeCap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeCap/) wyliczenie:

- [`Butt`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Butt/) (ustawienie domyślne)
- [`Square`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)

Najlepiej są one przedstawiane za pomocą przykładowy program. Druga sekcja strony głównej [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) program zaczyna się od strony zatytułowany **Caps obrysu** na podstawie [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) klasy. Ta strona definiuje `PaintSurface` obsługi zdarzeń, który przetwarza w pętli trzech elementów członkowskich `SKStrokeCap` wyliczenia, wyświetlana nazwa elementu członkowskiego wyliczenia i rysowanie linii za pomocą tego koniec obrysu:

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

Dla każdego członka `SKStrokeCap` wyliczenia, obsługę rysuje dwa wiersze, jeden z grubość pociągnięć 50 pikseli i innego wiersza znajduje się w górnej części z grubość pociągnięć 2 pikseli. Wiersz jest za zadanie zilustrowanie geometrycznych rozpoczęcia i zakończenia, niezależnie od grubości linii i koniec obrysu wiersza:

[![](lines-images/strokecaps-small.png "Potrójna zrzut ekranu przedstawiający stronę Caps obrysu")](lines-images/strokecaps-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Caps obrysu")

Jak widać, `Square` i `Round` caps obrysu skutecznie wydłużyć wiersza o połowę szerokości obrysu na początku wiersza i ponownie na końcu. To rozszerzenie staje się ważne, gdy konieczne jest Określ wymiary obiektu odtwarzane grafiki.

`SKCanvas` Klasa zawiera również inna metoda na rysowanie wielu wierszy jest nieco szczególna:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points` Parametr jest tablicą `SKPoint` wartości i `mode` jest elementem członkowskim [ `SKPointMode` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPointMode/) wyliczenia, który zawiera trzy składniki:

- [`Points`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Points/) do poszczególnych punktów renderowania
- [`Lines`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Lines/) Aby połączyć każda para punkty
- [`Polygon`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Polygon/) Aby połączyć wszystkie kolejne punkty

**Wiele wierszy** strona przedstawia tę metodę. [ `MultipleLinesPage` Pliku XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) tworzy dwa `Picker` członkiem wybierz widoki, które pozwalają `SKPointMode` wyliczenie i elementem członkowskim `SKStrokeCap` wyliczenie:

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

`SelectedIndexChanged` Obsługi zarówno `Picker` widoków po prostu unieważnia `SKCanvasView` obiektu:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Ten program obsługi musi sprawdzić obecność `SKCanvasView` obiektu, ponieważ program obsługi zdarzeń jest pierwszym wywoływane, gdy `SelectedIndex` właściwość `Picker` jest ustawiony na 0 w pliku XAML występuje przed `SKCanvasView` zostały utworzone.

`PaintSurface` Obsługi uzyskuje dostęp do metody rodzajowej uzyskiwania dwie wybrane elementy z `Picker` widoki i konwertowania ich na wartości wyliczenia:

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

Zrzut ekranu przedstawia różne `Picker` wybrane elementy na trzy platformach:

[![](lines-images/multiplelines-small.png "Potrójna zrzut ekranu przedstawiający stronę wiele wierszy")](lines-images/multiplelines-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę wielu wierszy")

IPhone, w lewym pokazuje sposób `SKPointMode.Points` powoduje, że element członkowski wyliczenia `DrawPoints` do renderowania każdego z punktów w `SKPoint` tablicy jako kwadrat, jeśli jest zakończenie linii `Butt` lub `Square`. Okręgi są renderowane w przypadku zakończenia wiersza `Round`.

Jeśli natomiast używasz `SKPointMode.Lines`, jak pokazano na ekranie systemu Android w Centrum, `DrawPoints` metody rysuje każda para `SKPoint` wartości, przy użyciu określonego wiersza centralnych zasad dostępu, w tym przypadku `Round`.

Urządzenie przenośne z systemem Windows przedstawia wynik `SKPointMode.Polygon` wartość. Wiersz jest rysowany między kolejnych punktów w tablicy, ale bardzo ściśle dostępne, można zobaczyć te wiersze nie są połączone. Każdy z tych osobnych wierszach początek i koniec z centralnych zasad dostępu określonego wiersza. W przypadku wybrania `Round` caps, mogą być wyświetlane linie połączenia, ale ich nie naprawdę połączenia.

Czy wiersze są połączony lub niepołączony jest niezwykle istotne aspektów pracy z ścieżki grafiki.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
