---
title: Podstawowe animacji w SkiaSharp
description: W tym artykule opisano sposób animować SkiaSharp grafiki w aplikacji platformy Xamarin.Forms i pokazano to z przykładowym kodzie.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 08583a62719927b900c6aeede1b3b4398ed803de
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243346"
---
# <a name="basic-animation-in-skiasharp"></a>Podstawowe animacji w SkiaSharp

_Jak animować SkiaSharp grafiki_

Można animować SkiaSharp grafiki w platformy Xamarin.Forms, powodując `PaintSurface` bardzo często wywoływanej metody zawsze rysowania grafiki nieco inaczej. Oto animacji pokazano w dalszej części tego artykułu koncentrycznych pozornie rozszerzające z Centrum:

![](animation-images/animationexample.png "Kilka koncentrycznych pozornie rozszerzanie Centrum")

**Pulsating elipsy** strony [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program animuje dwóch osiach elipsy, dzięki czemu wydaje się być pulsating, a nawet można kontrolować Liczba ta pulsację:


[ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) pliku tworzy platformy Xamarin.Forms `Slider` i `Label` Aby wyświetlić aktualną wartość suwaka. To jest typowe sposób na zintegrowanie `SKCanvasView` z innymi widokami platformy Xamarin.Forms:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.PulsatingEllipsePage"
             Title="Pulsating Ellipse">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Slider x:Name="slider"
                Grid.Row="0"
                Maximum="10"
                Minimum="0.1"
                Value="5"
                Margin="20, 0" />

        <Label Grid.Row="1"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Cycle time = {0:F1} seconds'}"
               HorizontalTextAlignment="Center" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Tworzy plik CodeBehind `Stopwatch` obiektu jako obiektu clock wysokiej precyzji. `OnAppearing` Zastąpienia zestawy `pageIsActive` do `true` i wywołuje metodę o nazwie `AnimationLoop`. `OnDisappearing` Zastąpienie zestawów `pageIsActive` do `false`:

```csharp
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float scale;            // ranges from 0 to 1 to 0

public PulsatingEllipsePage()
{
    InitializeComponent();
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    AnimationLoop();
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    pageIsActive = false;
}
```

`AnimationLoop` Uruchamia metody `Stopwatch` , a następnie pętle podczas `pageIsActive` jest `true`. Jest zasadniczo "nieskończoną pętlę", gdy strona jest aktywna, ale nie powoduje program zawieszenie, ponieważ zawiera pętli w wyniku wywołania `Task.Delay` z `await` operatora, który umożliwia inne części funkcji programu. Argument `Task.Delay` powoduje jej zakończenie po 30/1 sekundę. Określa szybkość klatek animacji.

```csharp
async Task AnimationLoop()
{
    stopwatch.Start();

    while (pageIsActive)
    {
        double cycleTime = slider.Value;
        double t = stopwatch.Elapsed.TotalSeconds % cycleTime / cycleTime;
        scale = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;
        canvasView.InvalidateSurface();
        await Task.Delay(TimeSpan.FromSeconds(1.0 / 30));
    }

    stopwatch.Stop();
}

```

`while` Pętli rozpoczyna się od czasu cyklu od uzyskania `Slider`. Jest to czas w sekundach, na przykład 5. Druga instrukcja oblicza wartość `t` dla *czas*. Aby uzyskać `cycleTime` 5, `t` zwiększa z zakresu od 0 do 1 co 5 sekund. Argument `Math.Sin` funkcji w drugim instrukcji w zakresie od 0 do 2π co 5 sekund. `Math.Sin` Funkcja zwraca wartość z zakresu od 0 do 1 wstecz na 0, a następnie &ndash;1 i 0 co 5 sekund, ale z wartości, które zmieniają wolniej, gdy wartość jest bliski 1 lub -1. Wartość 1 jest dodawana, więc wartości są zawsze dodatnią, a następnie scenariusz jest podzielony przez 2, więc wartości z zakresu od ½, 1, aby ½ na 0, aby ½, ale wolniej, gdy wartość jest równa około 1 i 0. To jest przechowywane w `scale` pola i `SKCanvasView` zostało unieważnione.

`PaintSurface` Metoda używa to `scale` wartość do obliczenia dwóch osiach elipsy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float maxRadius = 0.75f * Math.Min(info.Width, info.Height) / 2;
    float minRadius = 0.25f * maxRadius;

    float xRadius = minRadius * scale + maxRadius * (1 - scale);
    float yRadius = maxRadius * scale + minRadius * (1 - scale);

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.Color = SKColors.Blue;
        paint.StrokeWidth = 50;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);

        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.SkyBlue;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
    }
}
```

Metoda oblicza maksymalną radius, zależnie od rozmiaru obszaru wyświetlania, a minimalny promień oparte na maksymalną radius. `scale` Wartość jest animowany pomiędzy 0 a 1 i z powrotem na 0, dlatego metoda użyty do obliczenia `xRadius` i `yRadius` waha się między `minRadius` i `maxRadius`. Te wartości są używane do rysowania i wypełnienia elipsy:

[![](animation-images/pulsatingellipse-small.png "Potrójna zrzut ekranu przedstawiający stronę pulsujące elipsy")](animation-images/pulsatingellipse-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę pulsujące wielokropka")

Zwróć uwagę, że `SKPaint` obiekt jest tworzony w `using` bloku. Wiele klas SkiaSharp, takich jak `SKPaint` pochodną `SKObject`, pochodzący od `SKNativeObject`, który implementuje [ `IDisposable` ](https://developer.xamarin.com/api/type/System.IDisposable/) interfejsu. `SKPaint` zastępuje `Dispose` metodę, aby zwolnić zasoby niezarządzane.

 Umieszczanie `SKPaint` w `using` bloku upewnia się, że `Dispose` nazywa się na końcu bloku, aby zwolnić te zasoby niezarządzane. Dzieje się to mimo to użycie pamięci przez `SKPaint` obiektu zostanie zwolniona przez moduł zbierający elementy bezużyteczne .NET, ale w kodzie animacji, zaleca się nieco aktywne w zwolnić pamięć, w sposób bardziej uporządkowany.

 W tym przypadku lepszym rozwiązaniem byłoby utworzyć dwa `SKPaint` obiekty raz, a następnie zapisz je jako pola.

Co to jest **rozszerzanie okręgi** jest animacji. [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) Klasy rozpoczyna się od definicji kilka pól, w tym `SKPaint` obiektu:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    const double cycleTime = 1000;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float t;
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke
    };

    public ExpandingCirclesPage()
    {
        Title = "Expanding Circles";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ...
}
```

Ten program używa innego podejścia do animacji, w oparciu o platformy Xamarin.Forms `Device.StartTimer`. `t` Pole jest animowany z zakresu od 0 do 1 co `cycleTime` w milisekundach:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            t = (float)(stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }
            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`PaintSurface` Obsługi rysuje 5 koncentrycznych z animowany promień. Jeśli `baseRadius` zmiennej wynosi 100, następnie `t` jest animowany z zakresu od 0 do 1, promień wzrost pięć okręgi z zakresu od 0 do 100, 200 do 100, 200 i 300, 300 i 400 i 400 lub 500. Dla większości kółka `strokeWidth` jest 50, ale w pierwszym okrąg, `strokeWidth` animuje z zakresu od 0 do 50. Dla większości kółka kolor jest niebieski, ale dla ostatniego okręgu kolor jest animowany z blue na przezroczyste:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
        float baseRadius = Math.Min(info.Width, info.Height) / 12;

        for (int circle = 0; circle < 5; circle++)
        {
            float radius = baseRadius * (circle + t);

            paint.StrokeWidth = baseRadius / 2 * (circle == 0 ? t : 1);
            paint.Color = new SKColor(0, 0, 255,
                (byte)(255 * (circle == 4 ? (1 - t) : 1)));

            canvas.DrawCircle(center.X, center.Y, radius, paint);
        }
    }
}
```

Wynik jest, że obraz wygląda takie same, gdy `t` jest równe 0, gdy `t` jest równa 1 oraz kółka prawdopodobnie Kontynuuj rozwijanie nieskończona:

[![](animation-images/expandingcircles-small.png "Potrójna zrzut ekranu przedstawiający stronę rozszerzanie okręgi")](animation-images/expandingcircles-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę rozszerzanie okręgi")


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
