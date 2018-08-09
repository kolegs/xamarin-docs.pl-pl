---
title: Malowanie palcami w SkiaSharp
description: W tym artykule wyjaśniono, jak używać palcami do rysowania na kanwie SkiaSharp w aplikacji platformy Xamarin.Forms i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: b0f28cd3e8a928a6da3169dee96ec089178a64e2
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615824"
---
# <a name="finger-painting-in-skiasharp"></a>Malowanie palcami w SkiaSharp

_Użyj palców do rysowania na kanwie._

`SKPath` Obiekt może być stale aktualizowane i wyświetlane. Ta funkcja umożliwia ścieżki służący do rysowania interaktywne, takie jak w programie kolorowa.

![](finger-paint-images/fingerpaintsample.png "Wykonywanie w malowanie palcami")

Obsługi wprowadzania dotykowego, w interfejsie Xamarin.Forms nie pozwala na śledzenie poszczególnych palców na ekranie, więc efekt śledzenia touch Xamarin.Forms został opracowany do obsługi dodatkowych touch. Ten efekt jest opisany w artykule [ **wywoływanie zdarzenia z efektów**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). Przykładowy program [ **Touch śledzenia wpływu pokazy** ](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) zawiera dwie strony, które używają SkiaSharp, w tym kolorowa program.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) rozwiązanie obejmuje to zdarzenie śledzenia touch. Projekt biblioteki .NET Standard obejmuje `TouchEffect` klasy `TouchActionType` wyliczenia, `TouchActionEventHandler` delegować i `TouchActionEventArgs` klasy. Każdy z projektów platformy obejmują `TouchEffect` klasy dla danej platformy; zawiera także projekt dla systemu iOS `TouchRecognizer` klasy.

**Finger Paint** strony w **SkiaSharpFormsDemos** to uproszczony implementacja malowanie palcami. Nie zezwalaj na wybieranie koloru lub obrysu szerokości, go nie ma możliwości Usuń obszar roboczy i oczywiście nie można zapisać kompozycji.

[ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml) pliku umieszcza `SKCanvasView` w jednej komórki `Grid` i dołącza `TouchEffect` , `Grid`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Paths.FingerPaintPage"
             Title="Finger Paint">

    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Dołączanie `TouchEffect` bezpośrednio do `SKCanvasView` nie działa w ramach wszystkich platform.

[ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs) pliku związanego z kodem definiuje dwie kolekcje do przechowywania `SKPath` obiektów, a także `SKPaint` obiektu do renderowania tych ścieżek:

```csharp
public partial class FingerPaintPage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };

    public FingerPaintPage()
    {
        InitializeComponent();
    }
    ...
}
```

Jako sugerowanej nazwy `inProgressPaths` słownika przechowuje ścieżek, które obecnie są wprowadzane przez co najmniej jeden palców. Klucz ze słownika jest identyfikator touch, który towarzyszy zdarzenia dotykowe. `completedPaths` Pole jest kolekcją ścieżek, które zostały zakończone podczas obliczania palcem rysowanie ścieżki podniesiony z poziomu ekranu.

`TouchAction` Obsługi zarządza te dwie kolekcje. Gdy palcem najpierw dotyka ekranu nowy `SKPath` jest dodawany do `inProgressPaths`. Przemieszcza się w tej linii papilarnych, dodatkowe punkty zostaną dodane do ścieżki. Po zwolnieniu finger ścieżka jest przekazywana do `completedPaths` kolekcji. Można malować jednocześnie z wielu palców. Po każdej zmianie na ścieżki lub kolekcji `SKCanvasView` zostaje unieważniony:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                           (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }
}
```

Punkty, towarzyszący zdarzeń śledzenia touch są współrzędne Xamarin.Forms; te muszą zostać przekonwertowane na współrzędne SkiaSharp, które są pikseli. Jest celem `ConvertToPixel` metody.

`PaintSurface` Obsługi następnie po prostu renderuje obie kolekcje ścieżek. Wcześniej ukończone ścieżki zostaną wyświetlone poniżej ścieżki w toku:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ,,,
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (SKPath path in completedPaths)
        {
            canvas.DrawPath(path, paint);
        }

        foreach (SKPath path in inProgressPaths.Values)
        {
            canvas.DrawPath(path, paint);
        }
    }
    ...
}
```

Twoje obrazy finger ograniczona tylko Twojej talent:

[![](finger-paint-images/fingerpaint-small.png "Potrójna zrzut ekranu przedstawiający stronę Finger Paint")](finger-paint-images/fingerpaint-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę malowanie linii papilarnych")


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Pokazy efekt Touch śledzenia (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [Wywoływanie zdarzenia z efektów](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
