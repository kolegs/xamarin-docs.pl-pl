---
title: Typy wypełnień ścieżek
description: W tym artykule sprawdza różne efekty możliwe przy użyciu typy wypełnień ścieżek SkiaSharp i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: d16f6f6023c1db0223d5d5863e19116147f948d1
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615473"
---
# <a name="the-path-fill-types"></a>Typy wypełnień ścieżek

_Odkryj różne efekty możliwego typy wypełnień ścieżek SkiaSharp_

Dwa konturów w ścieżce może pokrywać się i wierszy, które tworzą pojedynczy rozkład może pokrywać się. Każdy obszar ujęty potencjalnie mogą zostać uzupełnione, ale może nie chcesz wypełnić spójne obszary. Oto przykład:

![](fill-types-images/filltypeexample.png "Wskazywany przez pięć gwiazdek częściowo filles")

Masz trochę kontrolę nad tym. Algorytm wypełnianie jest regulowane przez [ `SKFillType` ](xref:SkiaSharp.SKPath.FillType) właściwość `SKPath`, który ustawia do elementu członkowskiego [ `SKPathFillType` ](xref:SkiaSharp.SKPathFillType) wyliczenia:

- `Winding`, wartość domyślna
- `EvenOdd`
- `InverseWinding`
- `InverseEvenOdd`

Algorytmy zawijania i parzyste ustalenia, czy dowolny obszar ujęty jest wypełniony lub nie jest wypełniony zależnie od linią hipotetyczny w obszarze poza zakresem. Ten wiersz przecina jeden lub więcej wierszy granic, które tworzą ścieżki. Tryb zawijania Jeśli liczba granic wykresów w jednym kierunku równowadze się liczba wierszy w innym kierunku, a następnie obszaru nie jest wypełnione. W przeciwnym razie obszar jest wypełniony. Algorytm parzyste wypełnia obszar, jeśli liczba granic wierszy jest nieparzysta.

Za pomocą wielu ścieżek rutynowych zawijania algorytm często wypełnia ujęty obszary ścieżki. Algorytm parzyste generuje zazwyczaj bardziej interesujące wyniki.

Klasycznym przykładem jest wskazywany przez pięć gwiazdkę, jak pokazano w **Five-Pointed gwiazdki** strony. [ **FivePointedStarPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml) plików są tworzone wystąpienia dwóch `Picker` widoków, aby wybrać ścieżkę wypełnienia typu i czy ścieżka jest malowania wypełnione lub w jakiej kolejności:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.FivePointedStarPage"
             Title="Five-Pointed Star">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="fillTypePicker"
                Title="Path Fill Type"
                Grid.Row="0"
                Grid.Column="0"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPathFillType}">
                    <x:Static Member="skia:SKPathFillType.Winding" />
                    <x:Static Member="skia:SKPathFillType.EvenOdd" />
                    <x:Static Member="skia:SKPathFillType.InverseWinding" />
                    <x:Static Member="skia:SKPathFillType.InverseEvenOdd" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="drawingModePicker"
                Title="Drawing Mode"
                Grid.Row="0"
                Grid.Column="1"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Fill only</x:String>
                    <x:String>Stroke only</x:String>
                    <x:String>Stroke then Fill</x:String>
                    <x:String>Fill then Stroke</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2"
                                PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Plik związany z kodem będą wykorzystywane serwery `Picker` wartości, aby narysować gwiazdkę wskazywany przez pięć:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPath path = new SKPath
    {
        FillType = (SKPathFillType)fillTypePicker.SelectedItem
    };
    path.MoveTo(info.Width / 2, info.Height / 2 - radius);

    for (int i = 1; i < 5; i++)
    {
        // angle from vertical
        double angle = i * 4 * Math.PI / 5;
        path.LineTo(center + new SKPoint(radius * (float)Math.Sin(angle), 
                                        -radius * (float)Math.Cos(angle)));
    }
    path.Close();

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 50,
        StrokeJoin = SKStrokeJoin.Round
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    switch ((string)drawingModePicker.SelectedItem)
    {
        case "Fill only":
            canvas.DrawPath(path, fillPaint);
            break;

        case "Stroke only":
            canvas.DrawPath(path, strokePaint);
            break;

        case "Stroke then Fill":
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case "Fill then Stroke":
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

Normalnie, typ wypełnienia ścieżki powinna mieć wpływu na wypełnienia i nie pociągnięć ale dwa `Inverse` tryby wpływają na wypełnienia i pociągnięć. Do wypełnienia, dwa `Inverse` typy wypełniać oppositely obszarów, tak aby obszar poza gwiazdka jest wypełnione. Dla pociągnięcia, dwa `Inverse` typy kolor wszystkim, z wyjątkiem stroke. Używanie tych typów odwrotność wypełnienia można efektów niektóre nieparzysta, tak jak pokazano na zrzucie ekranu systemu iOS:

[![](fill-types-images/fivepointedstar-small.png "Potrójna zrzut ekranu przedstawiający stronę Five-Pointed Star")](fill-types-images/fivepointedstar-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Five-Pointed Star")

Dla systemów Android i platformy uniwersalnej systemu Windows zrzuty ekranu Pokaż typowe efektów parzyste i zawijania, ale kolejność obrysu i wypełnienia ma również wpływ na wyniki.

Algorytm zawijania zależy od kierunku rysowania linii. Zwykle podczas tworzenia ścieżki, można sterować tym kierunku podobnie jak określić, czy wiersze są pobierane z jednym punktem na inny. Jednak `SKPath` klasa definiuje również metod, takich jak `AddRect` i `AddCircle` , narysuj całego konturów. Aby kontrolować, jak te obiekty są rysowane, metody zawierają parametr typu [ `SKPathDirection` ](xref:SkiaSharp.SKPathDirection), która zawiera dwa elementy członkowskie:

- `Clockwise`
- `CounterClockwise`

Metody w `SKPath` zawierające `SKPathDirection` parametr nadaj wartość domyślną `Clockwise`.

**Nakładających się okręgów** strony tworzy ścieżkę z czterech nakładających się okręgów z typem wypełnienia parzyste ścieżki:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(info.Width, info.Height) / 4;

    SKPath path = new SKPath
    {
        FillType = SKPathFillType.EvenOdd
    };

    path.AddCircle(center.X - radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X - radius / 2, center.Y + radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y + radius / 2, radius);

    SKPaint paint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    canvas.DrawPath(path, paint);

    paint.Style = SKPaintStyle.Stroke;
    paint.StrokeWidth = 10;
    paint.Color = SKColors.Magenta;

    canvas.DrawPath(path, paint);
}
```

Chodzi o interesujących obraz utworzony przy minimalnej ilości kodu:

[![](fill-types-images/overlappingcircles-small.png "Potrójna zrzut ekranu przedstawiający stronę nakładających się okręgów")](fill-types-images/overlappingcircles-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę nakładających się okręgów")


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
