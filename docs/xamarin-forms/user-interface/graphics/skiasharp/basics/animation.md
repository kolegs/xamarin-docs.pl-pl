---
title: Podstawowa Animacja w SkiaSharp
description: W tym artykule wyjaśniono, jak animować grafiki SkiaSharp w aplikacjach Xamarin.Forms i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 0ba3d86f52d2e6907f32450d87f30280ade95d3f
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615642"
---
# <a name="basic-animation-in-skiasharp"></a>Podstawowa Animacja w SkiaSharp

_Dowiedz się, jak animować grafiki SkiaSharp_

Można animować grafiki SkiaSharp w interfejsie Xamarin.Forms, powodując `PaintSurface` bardzo często wywoływanej metody każdym rysowania grafiki nieco inaczej. Oto animacji przedstawiony w dalszej części tego artykułu koncentrycznych, które pozornie z Centrum:

![](animation-images/animationexample.png "Kilka koncentrycznych pozornie powiększający się z Centrum")

**Pulsating elipsy** strony w [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program animuje dwie osie elipsy, tak aby wydaje się być pulsating, a nawet kontrolowania szybkość to pulsację:


[ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) pliku tworzy rozwiązanie Xamarin.Forms `Slider` i `Label` do wyświetlenia bieżącej wartości suwaka. Jest to typowe sposób integrowania `SKCanvasView` z innymi widokami Xamarin.Forms:

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

Tworzy plik związany z kodem `Stopwatch` obiektu, która będzie służyć jako zegara wysokiej precyzji. `OnAppearing` Zastąpienia zestawy `pageIsActive` pole `true` i wywołuje metodę o nazwie `AnimationLoop`. `OnDisappearing` Zastąpienie zestawów, które `pageIsActive` pole `false`:

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

`AnimationLoop` Uruchomieniu metody `Stopwatch` i następnie pętli podczas `pageIsActive` jest `true`. Jest to zasadniczo "wejścia w nieskończoną pętlę", gdy strona jest aktywna, ale nie powoduje zawieszanie ponieważ pętli stwierdza, przy użyciu wywołania do programu `Task.Delay` z `await` operatora, który umożliwia innych części funkcji programu. Argument `Task.Delay` powoduje jego ukończenie po 30/1 sekundę. Definiuje szybkość klatek animacji.

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

`while` Pętli rozpoczyna się od czasu cyklu, od uzyskiwania `Slider`. Jest to czas w sekundach, na przykład 5. Druga instrukcja oblicza wartość `t` dla *czasu*. Aby uzyskać `cycleTime` 5, `t` wzrasta z zakresu od 0 do 1, co 5 sekund. Argument `Math.Sin` funkcji w drugiej instrukcji z zakresu od 0 do 2π co 5 sekund. `Math.Sin` Funkcja zwraca wartość z zakresu od 0 do 1 z tyłu do 0, a następnie do &ndash;1 i 0 co 5 sekund, ale z wartościami, które zmieniają wolniej, jeśli wartość to 1 lub -1. Wartość 1 jest dodawany, więc wartości są zawsze dodatnią, a następnie go podzieleniu przez 2, aby wartości z zakresu od ½, 1, aby ½ na 0, aby ½, ale wolniej, gdy wartość jest równa około 1 i 0. To jest przechowywany w `scale` pola i `SKCanvasView` zostaje unieważniony.

`PaintSurface` Metoda wykorzystuje to `scale` wartość do obliczenia dwie osie elipsy:

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

Metoda oblicza maksymalną radius, w zależności od rozmiaru obszaru wyświetlania, a minimalny promień oparte na maksymalnej usługi radius. `scale` Wartość jest animowany od 0 do 1 i z powrotem na 0, więc metoda używa, aby obliczyć `xRadius` i `yRadius` zakresów adresów, między `minRadius` i `maxRadius`. Te wartości są używane do rysowania i wypełnij elipsę:

[![](animation-images/pulsatingellipse-small.png "Potrójna zrzut ekranu przedstawiający stronę pulsujące elipsy")](animation-images/pulsatingellipse-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę pulsujące wielokropka")

Należy zauważyć, że `SKPaint` obiekt zostanie utworzony w `using` bloku. Wiele klas SkiaSharp, takich jak `SKPaint` pochodzi od klasy `SKObject`, który pochodzi od klasy `SKNativeObject`, który implementuje [ `IDisposable` ](xref:System.IDisposable) interfejsu. `SKPaint` zastępuje `Dispose` metodę, aby zwolnić niezarządzane zasoby.

 Umieszczenie `SKPaint` w `using` bloku, masz pewność, że `Dispose` nosi nazwę na końcu bloku zwolnienie tych niezarządzanych zasobów. Jest to spowodowane mimo to pamięć używana przez `SKPaint` obiektu zostanie zwolniony przez moduł odśmiecania pamięci platformy .NET, ale w kodzie animacji, zaleca się nieco aktywne w zwalnianie pamięci w bardziej uporządkowany sposób.

 W tym przypadku lepszym rozwiązaniem byłoby utworzyć dwa `SKPaint` obiekty raz, a następnie zapisz je jako pola.

Co to jest **rozwijanie okręgów** jest animacji. [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) Klasy rozpoczyna się od definicji kilka pól, w tym `SKPaint` obiektu:

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

Ten program używa innego podejścia do animacji, w oparciu o Xamarin.Forms `Device.StartTimer`. `t` Pola jest animowany z zakresu od 0 do 1 co `cycleTime` MS:

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

`PaintSurface` Obsługi rysuje 5 koncentrycznych o promieniach animowany. Jeśli `baseRadius` zmienna jest równa 100, następnie `t` jest animowany z zakresu od 0 do 1, promień narożników wzrostu pięć okręgów z zakresu od 0 do 100, 100, 200, 200, 300, 300 i 400 i 400 lub 500. Dla większości okręgów `strokeWidth` to 50, ale dla pierwszego okrąg, `strokeWidth` animuje z zakresu od 0 do 50. Dla większości okręgów kolor jest niebieskie, ale koła ostatni kolor jest animowany od niebieskiego na przezroczyste:

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

Wynik jest, że obraz wygląda takie same, gdy `t` jest równa 0, gdy `t` jest równa 1 oraz okręgów wydaje się, że nadal rozszerzanie zawsze:

[![](animation-images/expandingcircles-small.png "Potrójna zrzut ekranu przedstawiający stronę rozwijanie okręgów")](animation-images/expandingcircles-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę rozwijanie okręgów")


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
