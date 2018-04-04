---
title: Integrowanie z platformy Xamarin.Forms
description: Utwórz SkiaSharp grafika touch i elementy platformy Xamarin.Forms
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 67c4330d8e446a407dec7792fe5f40cdd9d23c22
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="integrating-with-xamarinforms"></a>Integrowanie z platformy Xamarin.Forms

_Utwórz SkiaSharp grafika touch i elementy platformy Xamarin.Forms_

Grafika SkiaSharp można zintegrować z resztą platformy Xamarin.Forms na kilka sposobów. Można połączyć kanwy SkiaSharp i platformy Xamarin.Forms elementy na tej samej stronie i nawet pozycji platformy Xamarin.Forms na kanwie SkiaSharp:

![](integration-images/integrationexample.png "Wybór koloru za pomocą suwaka")

Inną metodą tworzenia interaktywnych grafiki SkiaSharp w platformy Xamarin.Forms jest touch.
W drugiej stronie [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programu jest uprawniony **naciśnij przycisk przełączania wypełnienia**. Go rysuje Proste kółko dwa sposoby &mdash; bez wypełnienia i z wypełnieniem &mdash; przełączane przez tap. [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) Klasy pokazuje, jak w przypadku modyfikowania SkiaSharp grafiki w odpowiedzi na dane wejściowe użytkownika.

Na tej stronie `SKCanvasView` utworzyć wystąpienia klasy w [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) pliku, który określa również platformy Xamarin.Forms [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) w widoku:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.TapToggleFillPage"
             Title="Tap Toggle Fill">

    <skia:SKCanvasView PaintSurface="OnCanvasViewPaintSurface">
        <skia:SKCanvasView.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnCanvasViewTapped" />
        </skia:SKCanvasView.GestureRecognizers>
    </skia:SKCanvasView>
</ContentPage>
```

Powiadomienie `skia` deklaracji przestrzeni nazw XML.

`Tapped` Obsługę `TapGestureRecognizer` obiektu po prostu przełącza wartość logiczna pól i wywołania [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/) metody `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

Wywołanie `InvalidateSurface` skutecznie generuje wywołania `PaintSurface` obsługi, który używa `showFill` pola, aby wypełnić lub nie wypełnia okręgu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 50
    };
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);

    if (showFill)
    {
        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.Blue;
        canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    }
}
```

`StrokeWidth` Właściwość została ustawiona na 50 dla zaznaczenia różnicy. Najpierw rysowania wewnętrznych i konturu również widoczna szerokość cały wiersz. Domyślnie grafiki danych liczbowych rysowanego później w `PaintSurface` obsługi zdarzeń przesłaniać tych rysowane we wcześniejszej części programu obsługi.

**Eksploruj kolor** strony pokazano, jak można również zintegrować grafiki SkiaSharp z innymi elementami platformy Xamarin.Forms, a także pokazano różnicę między dwie alternatywne metody zdefiniowanie kolorów w SkiaSharp. Statycznych [ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/) metoda tworzy `SKColor` wartość oparta na modelu Hue-nasycenie-jasność:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

Statycznych [ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/) metoda tworzy `SKColor` wartość na podstawie podobne odcień nasycenie modelu:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

W obu przypadkach `h` argument może się wahać od 0 do 360. `s`, `l`, I `v` argumenty należeć do zakresu od 0 do 100. `a` (Alfa lub przezroczystość) argument w zakresie od 0 do 255.

[ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) plików umożliwia utworzenie dwóch `SKCanvasView` obiekty w `StackLayout` side-by-side z `Slider` i `Label` widoków, które umożliwiają użytkownikowi na wybranie HSL i HSV wartości kolorów:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Basics.ColorExplorePage"
             Title="Color Explore">
    <StackLayout>
        <!-- Hue slider -->
        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference hueSlider},
                              Path=Value,
                              StringFormat='Hue = {0:F0}'}" />

        <!-- Saturation slider -->
        <Slider x:Name="saturationSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference saturationSlider},
                              Path=Value,
                              StringFormat='Saturation = {0:F0}'}" />

        <!-- Lightness slider -->
        <Slider x:Name="lightnessSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference lightnessSlider},
                              Path=Value,
                              StringFormat='Lightness = {0:F0}'}" />

        <!-- HSL canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hslCanvasView"
                               PaintSurface="OnHslCanvasViewPaintSurface" />

            <Label x:Name="hslLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>

        <!-- Value slider -->
        <Slider x:Name="valueSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference valueSlider},
                              Path=Value,
                              StringFormat='Value = {0:F0}'}" />

        <!-- HSV canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hsvCanvasView"
                               PaintSurface="OnHsvCanvasViewPaintSurface" />

            <Label x:Name="hsvLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Dwa `SKCanvasView` elementy znajdują się w pojedynczej komórki `Grid` z `Label` z góry do wyświetlania wartości kolorów RGB wynikowe.

[ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) pliku CodeBehind jest stosunkowo proste. Udostępniony `ValueChanged` obsługi dla trzech `Slider` elementów po prostu unieważnia zarówno `SKCanvasView` elementów. `PaintSurface` Obsługi wyczyścić obszar roboczy z kolor wskazywany przez `Slider` elementy, a także ustawić `Label` działo się nad `SKCanvasView` elementy:

```csharp
public partial class ColorExplorePage : ContentPage
{
    public ColorExplorePage()
    {
        InitializeComponent();

        hueSlider.Value = 0;
        saturationSlider.Value = 100;
        lightnessSlider.Value = 50;
        valueSlider.Value = 100;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        hslCanvasView.InvalidateSurface();
        hsvCanvasView.InvalidateSurface();
    }

    void OnHslCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsl((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)lightnessSlider.Value);
        args.Surface.Canvas.Clear(color);

        hslLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }

    void OnHsvCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsv((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)valueSlider.Value);
        args.Surface.Canvas.Clear(color);

        hsvLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }
}
```

W modelach kolor HSL i HSV wartość Hue zakresu od 0 do 360 i wskazuje dominującą hue koloru. Są to tradycyjne kolory siłowe: czerwony, pomarańczowy, żółty, zielony, niebieski, indigo, fioletowo i z powrotem w koło czerwony.

W modelu HSL wartość 0 dla jasności jest zawsze czarny, a wartość 100 zawsze jest białe. Nasycenie wartość wynosi 0, jasność wartości od 0 do 100 są odcieni szarości. Zwiększenie nasycenie dodaje więcej kolorów. Kolory (które są wartości RGB o jeden element równy 255 innego równa 0, a trzeci od 0 do 255) wystąpić, gdy nasycenie wynosi 100, a jasność jest 50.

W modelu HSV kolory wyniku przypadku nasycenie i wartość 100. Kolor jest czarny, gdy wartość jest równa 0, niezależnie od innych ustawień. Odcieni szarości wystąpić, gdy nasycenie jest 0 i wartość w zakresie od 0 do 100.

Ale jest najlepszym sposobem, aby uzyskać pewne pojęcie o dwa modele do eksperymentów z nimi:

[![](integration-images/colorexplore-large.png "Potrójna zrzut ekranu przedstawiający stronę Eksploruj kolor")](integration-images/colorexplore-small.png#lightbox "Potrójna zrzut ekranu strony Eksploruj kolorów")


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
