---
title: Typy wypełnienia ścieżki
description: Odnajdywanie efekty różnych możliwe typy wypełnienia ścieżki SkiaSharp
ms.topic: article
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 16471c10028c2e4bc91e1cfc1bddeed153680d49
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2018
---
# <a name="the-path-fill-types"></a>Typy wypełnienia ścieżki

_Odnajdywanie efekty różnych możliwe typy wypełnienia ścieżki SkiaSharp_

Mogą nakładać się na dwóch konturów w ścieżce, a wiersze, które składają się na jednym rozkład mogą nakładać się na. Każdy obszar objętego potencjalnie może zostać wypełniony, ale nie należy wypełnić spójne obszary. Oto przykład:

![](fill-types-images/filltypeexample.png "Odnosi się do pięciu w formie gwiazdek częściowo filles")

Masz niewielką kontrolę nad tym. Algorytm wypełnianie podlega warunkom [ `SKFillType` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.FillType/) właściwość `SKPath`, która wartość jest członkiem [ `SKPathFillType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathFillType/) wyliczenie:

- [`Winding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.Winding/), wartość domyślna
- [`EvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.EvenOdd/)
- [`InverseWinding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseWinding/)
- [`InverseEvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseEvenOdd/)

Algorytmy zawijania i parzyste określenia, czy wszystkie obszaru jest wypełnione i nie wypełnione na podstawie wierszu hipotetyczny pochodzą z tego obszaru do nieskończoności. Ten wiersz przecina jeden lub więcej wierszy granic, które składają się na ścieżce. Tryb zawijania Jeśli liczba granic linii w jednym kierunku saldo liczbę linii w innym kierunku, a następnie obszaru nie jest wypełnione. W przeciwnym razie wartość obszaru jest wypełnione. Algorytm parzyste wypełnia obszar, jeśli liczba granic wierszy jest nieparzysta.

Z wielu ścieżek rutynowych zawijania algorytm często wypełnia objętego obszary ścieżki. Algorytm parzyste tworzy zazwyczaj bardziej interesującego wyników.

Klasycznym przykładem jest wskazywana przez pięć gwiazdy, jak pokazano w **Five-Pointed gwiazdy** strony. [FivePointedStarPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml) pliku tworzy dwa `Picker` widoków, aby wybrać ścieżkę wypełnienia typu i czy ścieżka jest malowania wypełnione i/lub w jakiej kolejności:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.FivePointedStarPage"
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
            <Picker.Items>
                <x:String>Winding</x:String>
                <x:String>EvenOdd</x:String>
                <x:String>InverseWinding</x:String>
                <x:String>InverseEvenOdd</x:String>
            </Picker.Items>
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
            <Picker.Items>
                <x:String>Fill only</x:String>
                <x:String>Stroke only</x:String>
                <x:String>Stroke then Fill</x:String>
                <x:String>Fill then Stroke</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Plik CodeBehind używa zarówno `Picker` wartości do rysowania gwiazdy odnosi się do pięciu:

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
        FillType = (SKPathFillType)Enum.Parse(typeof(SKPathFillType),
                        fillTypePicker.Items[fillTypePicker.SelectedIndex])
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

    switch (drawingModePicker.SelectedIndex)
    {
        case 0:
            canvas.DrawPath(path, fillPaint);
            break;

        case 1:
            canvas.DrawPath(path, strokePaint);
            break;

        case 2:
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case 3:
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

Zwykle typ wypełnienia ścieżki narusza tylko wypełnienia i nie pociągnięć, ale dwa `Inverse` tryby wpływają na, wypełnienia i pociągnięć. Do wypełnienia, dwa `Inverse` typy wypełnienia oppositely obszarów, dzięki czemu obszar poza gwiazdy zostanie wypełniony. Dla pociągnięć, dwa `Inverse` typy kolor wszystkim poza pociągnięć. Używanie tych typów odwrotny wypełnienia można efektów niektórych nieparzysta, jak pokazano na zrzucie ekranu z systemem iOS:

[![](fill-types-images/fivepointedstar-small.png "Potrójna zrzut ekranu przedstawiający stronę gwiazdkę Five-Pointed")](fill-types-images/fivepointedstar-large.png#lightbox "Potrójna zrzut ekranu strony Five-Pointed gwiazdy")

Android i Windows przenośnych zrzuty ekranu Pokaż typowe skutki parzyste i zawijania, ale kolejność obrysu i wypełnienia wpływa na wyniki.

Algorytm zawijania jest zależna od kierunku rysowania linii. Zwykle podczas tworzenia ścieżki, można sterować tym kierunku jak określić, że wiersze są rysowane z jednego miejsca do innego. Jednak `SKPath` klasy definiuje również metody, takie jak `AddRect` i `AddCircle` który rysowania całego konturów. Aby kontrolować, jak te obiekty są rysowane, metody to parametr typu [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/), która zawiera dwa elementy członkowskie:

- [`Clockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.Clockwise/)
- [`CounterClockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.CounterClockwise/)

Metody w `SKPath` zawierające `SKPathDirection` parametru nadaj mu wartość domyślną równą `Clockwise`.

**Nakładających się okręgi** strony tworzy ścieżki z czterech okręgi nakładające się z typem wypełnienia parzyste ścieżki:

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

Jest obraz interesujące utworzony z co najmniej kodu:

[![](fill-types-images/overlappingcircles-small.png "Potrójna zrzut ekranu przedstawiający stronę nakładających się okręgi")](fill-types-images/overlappingcircles-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę okręgi nakładających się")


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
