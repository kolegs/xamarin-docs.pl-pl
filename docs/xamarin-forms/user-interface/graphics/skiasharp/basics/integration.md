---
title: Integracja z zestawem narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak utworzyć skiasharp — grafika touch i elementy zestawu narzędzi Xamarin.Forms i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 35aede1a541d0ff62f6a4a5b57256c389e5a8640
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997533"
---
# <a name="integrating-with-xamarinforms"></a>Integracja z zestawem narzędzi Xamarin.Forms

_Tworzenia grafiki SkiaSharp, które reagują na dotyk i elementy zestawu narzędzi Xamarin.Forms_

Skiasharp — grafika można zintegrować z pozostałą częścią zestawu narzędzi Xamarin.Forms na kilka sposobów. Można połączyć kanwy SkiaSharp i zestawu narzędzi Xamarin.Forms elementów na tej samej stronie i nawet pozycji Xamarin.Forms elementów na kanwie SkiaSharp:

![](integration-images/integrationexample.png "Wybór koloru za pomocą suwaka")

Innym sposobem tworzenia interaktywnego grafiki SkiaSharp w interfejsie Xamarin.Forms jest za pomocą dotyku.
Na drugiej stronie w [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program jest upoważniona **naciśnij przełącznik wypełnienia**. Jego Rysowanie prostego okręgu na dwa sposoby &mdash; bez wypełnienia i z wypełnieniem &mdash; przełączać klikając naciśnij. [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) Klasy pokazuje, jak można zmienić grafikę SkiaSharp w odpowiedzi na dane wejściowe użytkownika.

Na tej stronie `SKCanvasView` utworzyć wystąpienia klasy w [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) pliku, który ustawia również rozwiązanie Xamarin.Forms [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) w widoku:

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

Zwróć uwagę `skia` deklarację przestrzeni nazw XML.

`Tapped` Obsługa `TapGestureRecognizer` obiektu po prostu przełącza wartość pola logicznych i wywołania [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/) metody `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

Wywołanie `InvalidateSurface` skutecznie generuje wywołanie `PaintSurface` obsługi, który używa `showFill` pola do wypełnienia lub nie wypełniać pola koła:

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

`StrokeWidth` Właściwości ustawiono 50 akcentowania różnica. Widać również szerokość całego wiersza przez najpierw rysowania wewnętrznych i konspektu. Domyślnie, rysunki rysowane w dalszej części grafiki `PaintSurface` program obsługi zdarzeń zasłaniać tych rysowane we wcześniejszej części programu obsługi.

**Eksplorowanie kolor** strony pokazuje, jak można również zintegrować SkiaSharp grafiki z innymi elementami platformy Xamarin.Forms i przedstawia również różnicę między dwie alternatywne metody definiowania kolorów w SkiaSharp. Statyczne [ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/) metoda tworzy `SKColor` wartości na podstawie Hue-nasycenie-jasności modelu:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

Statyczne [ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/) metoda tworzy `SKColor` wartości na podstawie podobne odcień, nasycenie, wartości modelu:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

W obu przypadkach `h` argumentu z zakresu od 0 do 360. `s`, `l`, I `v` argumenty do zakresu od 0 do 100. `a` (Alfa lub nieprzezroczystości) argumentu z zakresu od 0 do 255.

[ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) pliku tworzy dwa `SKCanvasView` obiekty w `StackLayout` side-by-side przy użyciu `Slider` i `Label` widoków, które umożliwiają użytkownikowi na wybranie HSL i HSV wartości kolorów:

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

Dwa `SKCanvasView` elementy znajdują się w jednej komórki `Grid` z `Label` "siedzieć" na górze do wyświetlania wartość wynikowa koloru RGB.

[ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) pliku związanego z kodem jest stosunkowo prosta. Wspólnie `ValueChanged` obsługi dla trzech `Slider` elementy po prostu unieważnia zarówno `SKCanvasView` elementów. `PaintSurface` Obsługi wyczyść kanwy przy użyciu koloru wskazywanym przez `Slider` elementów, a także ustawić `Label` znajdują się w górnej części `SKCanvasView` elementy:

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

W modelach kolor HSL i HSV wartość Hue zakresu od 0 do 360 i wskazuje dominujący odcień koloru. Tradycyjne kolory tęczowego: czerwony, pomarańczowy, żółty, zielony, niebieski, indigo, violet i z powrotem w okręgu w systemie red.

W modelu HSL wartość 0 dla jasności zawsze jest czarny, a wartość 100 zawsze biały. Wartość nasycenie wynosi 0, jasność wartości z zakresu od 0 do 100 są odcieni szarości. Zwiększenie nasycenie dodaje więcej kolorów. Kolory, (które wartości RGB o jeden element równy 255, inny równa 0, a trzeci od 0 do 255) występują, gdy nasycenie wynosi 100, a jasność to 50.

W modelu HSV kolory wyniku przypadku nasycenie i wartość 100. Kolor jest czarny, gdy wartość jest równa 0, niezależnie od innych ustawień. Odcienie szarości wystąpić, gdy nasycenie jest 0 i wartość z zakresu od 0 do 100.

Jednak jest najlepszym sposobem, aby można było uzyskać pewne pojęcie obu modeli, aby eksperymentować z nimi:

[![](integration-images/colorexplore-large.png "Potrójna zrzut ekranu strony Poznaj kolor")](integration-images/colorexplore-small.png#lightbox "Potrójna zrzut ekranu strony Poznaj kolorów")


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
