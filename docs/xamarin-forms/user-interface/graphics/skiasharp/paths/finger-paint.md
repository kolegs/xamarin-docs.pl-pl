---
title: Malowania w SkiaSharp
description: W tym artykule wyjaśniono, jak używać palców do rysowania na kanwie SkiaSharp w aplikacji platformy Xamarin.Forms i pokazuje to z przykładowym kodzie.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: f4c3d2ef2f6d1253f58b95559ef83af291f87b03
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243782"
---
# <a name="finger-painting-in-skiasharp"></a>Malowania w SkiaSharp

_Użyj palcami do rysowania na kanwie._

`SKPath` Obiekt może być stale aktualizowane i wyświetlane. Ta funkcja umożliwia ścieżki służący do rysowania interaktywne, takie jak w programie finger-painting.

![](finger-paint-images/fingerpaintsample.png "Ćwiczenie w malowania")

Obsługa touch w platformy Xamarin.Forms nie zezwala na śledzenia poszczególnych palców na ekranie, więc efekt touch śledzenia platformy Xamarin.Forms został opracowany do obsługi dodatkowych touch. W tym celu jest opisana w artykule [ **wywoływanie zdarzeń od efekty**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). Przykładowy program [ **pokazy efekt śledzenia Touch** ](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) zawiera dwie strony, które używają SkiaSharp, w tym finger-painting program.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) rozwiązanie zawiera to zdarzenie śledzenia touch. Zawiera standardowe .NET projektu biblioteki `TouchEffect` klasy `TouchActionType` wyliczenia, `TouchActionEventHandler` delegować i `TouchActionEventArgs` klasy. Każdy z projektów platformy obejmują `TouchEffect` klasy dla tej platformy; zawiera także projektu iOS `TouchRecognizer` klasy.

**Paint palca** strony **SkiaSharpFormsDemos** jest uproszczoną implementację malowania. Nie zezwalaj na wybieranie koloru lub obrysu szerokość go nie ma możliwości wyczyścić obszar roboczy, a oczywiście nie można zapisać kompozycji.

[ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml) pliku naraża `SKCanvasView` w komórce jednym `Grid` i dołącza `TouchEffect` w tym `Grid`:

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

[ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs) plik CodeBehind definiuje dwie kolekcje do przechowywania `SKPath` obiektów, a także `SKPaint` obiektu do renderowania tych ścieżek:

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

Jako sugerowanej nazwy `inProgressPaths` słownika przechowuje ścieżek, które obecnie są wprowadzane przez co najmniej jeden palców. Klucz słownika jest identyfikator touch dołączona zdarzenia touch. `completedPaths` Pole to zbiór ścieżek, które zostały zakończone podczas obliczania palcem rysowania ścieżki unosiło na ekranie.

`TouchAction` Te dwie kolekcje zarządza program obsługi. Gdy palcem najpierw krawędzi ekranu, nowy `SKPath` jest dodawany do `inProgressPaths`. Podczas przenoszenia tej linii papilarnych, dodatkowe punkty są dodawane do ścieżki. Po wydaniu palca ścieżka jest przenoszona do `completedPaths` kolekcji. Można malować jednocześnie z wieloma palców. Po każdej zmianie do ścieżek lub kolekcje `SKCanvasView` unieważnienia:

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

Punkty towarzyszące zdarzenia śledzenia touch są współrzędne platformy Xamarin.Forms; te muszą zostać przekonwertowane na współrzędne SkiaSharp, które są pikseli. Jest celem `ConvertToPixel` metody.

`PaintSurface` Obsługi następnie po prostu renderuje obie kolekcje ścieżki. Wcześniej zakończone ścieżki zostaną wyświetlone poniżej ścieżki w toku:

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

Twoje obrazy palca tylko są ograniczone przez użytkownika talenty:

[![](finger-paint-images/fingerpaint-small.png "Potrójna zrzut ekranu przedstawiający stronę Paint palca")](finger-paint-images/fingerpaint-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Paint palca")


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Pokazy efekt Touch śledzenia (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [Wywoływanie zdarzeń z efekty](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
