---
title: Przekształcenie obracania
description: W tym artykule Eksploruje efekty i animacji przy użyciu przekształcenie obracania SkiaSharp i prezentuje to z przykładowym kodzie.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: 514ecd16fedd7d3fda39fe20641cf0ee9ecb119e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244623"
---
# <a name="the-rotate-transform"></a>Przekształcenie obracania

_Poznaj efekty i animacji przy użyciu przekształcenie obracania SkiaSharp_

Obróć przekształcania obiektów graficznych SkiaSharp przerywanie wolnego ograniczenia wyrównanie przy użyciu osi poziomej i pionowej:

![](rotate-images/rotateexample.png "Tekst obrócona wokół środka")

Dla obiektu graficznego wokół punktu (0, 0), SkiaSharp obsługuje zarówno obracanie [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/) — metoda i [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/) metody:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

Koło 360 stopni jest taka sama jak wartość w radianach 2π, więc ułatwia konwersję między dwiema jednostkami. Użyj zależności jest wygodne. Wszystkie funkcje trygonometryczne statycznych [ `Math` ](https://developer.xamarin.com/api/type/System.Math/) klasy za pomocą jednostek radianów.

Obracanie jest prawo zwiększenia kątów. (Mimo że zegara Konwencja obrotu na układ współrzędnych kartezjański obrót w prawo jest zgodna z współrzędne Y zwiększenie będzie w dół.) Ujemne kąty i kąty większa, niż jest to dozwolone 360 stopni.

Formuł przekształcenia obrotu są bardziej złożone niż Przetłumacz oraz skalowalność. Dla kąta α formuły przekształcania są:

x' = x•cos(α) – y•sin(α)   

y` = x•sin(α) + y•cos(α)

**Obróć podstawowe** pokazuje stronę `RotateDegrees` metody. [ `BasicRotate.xaml.cs` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) Pliku wyświetla tekst z jego linii bazowej skupia się na stronie i obraca go na podstawie `Slider` zakres – 360 do 360. Oto odpowiedniej części `PaintSurface` programu obsługi:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Ponieważ obrotu skupia się w lewym górnym rogu obszaru roboczego dla większości kątów ustawiony w tym programie odniosło jest obracany tekst:

[![](rotate-images/basicrotate-small.png "Potrójna zrzut ekranu strony podstawowe Obróć")](rotate-images/basicrotate-large.png#lightbox "Potrójna zrzut ekranu strony Obróć podstawowe")

Bardzo często można obrócić coś skupia się wokół punktu określonego pivot korzystających z tych wersji z [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/System.Single/System.Single/) i [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/System.Single/System.Single/) metod:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**Wyśrodkowany Obróć** strona jest tak jak **podstawowe Obróć** z tą różnicą, że rozszerzona wersja `RotateDegrees` służy do ustawiania środek obrotu do tego samego punktu używana do pozycjonowania tekst:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Teraz tekst obraca się wokół punktu używana do pozycjonowania tekst, który jest Centrum poziomych linii bazowej tekst:

[![](rotate-images/centeredrotate-small.png "Potrójna zrzut ekranu strony wyśrodkowany Obróć")](rotate-images/centeredrotate-large.png#lightbox "Potrójna zrzut ekranu strony wyśrodkowany Obróć")

Tak jak w przypadku wersji wyśrodkowany `Scale` metody, wyśrodkowany wersję `RotateDegrees` wywołanie jest skrót:

```csharp
RotateDegrees (degrees, px, py);
```

Jest to równoważne z następujących czynności:

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Dowiesz się, że można łączyć czasami `Translate` wywołania z `Rotate` wywołania. Na przykład poniżej przedstawiono `RotateDegrees` i `DrawText` odwołuje się **wyśrodkowany Obróć** strony;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees` Wywołanie jest odpowiednikiem dwa `Translate` wywołań i innych niż skupia się `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText` Wywołania do wyświetlania tekstu w określonej lokalizacji jest odpowiednikiem `Translate` wywołania dla tej lokalizacji, a następnie `DrawText` w punkcie (0, 0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

Dwa kolejne `Translate` wywołania anulowania siebie:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

Koncepcyjnie dwa transformacje są stosowane w kolejności przeciwnej sposobu ich wyświetlania w kodzie. `DrawText` Wywołania tekst jest wyświetlany w lewym górnym rogu obszaru roboczego. `RotateDegrees` Wywołania obraca tego tekstu względem lewego górnego rogu. Następnie przy użyciu `Translate` wywołania Przesuwa tekst na środku kanwy.

Zazwyczaj są łączenie obrotu i translacji na kilka sposobów. **Obracany tekst** strony tworzy następujące wyświetlania:

[![](rotate-images/rotatedtext-small.png "Potrójna zrzut ekranu przedstawiający stronę obracany tekst")](rotate-images/rotatedtext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę obracany tekst")

Oto `PaintSurface` obsługi [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) klasy:

```csharp
static readonly string text = "    ROTATE";
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 72
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float yText = yCenter - textBounds.Height / 2 - textBounds.Top;

        for (int degrees = 0; degrees < 360; degrees += 30)
        {
            canvas.Save();
            canvas.RotateDegrees(degrees, xCenter, yCenter);
            canvas.DrawText(text, xCenter, yText, textPaint);
            canvas.Restore();
        }
    }
}

```

`xCenter` i `yCenter` wartości wskazują na środku kanwy. `yText` Wartość jest nieco przesunięcie od tego. To ustawienie Określa współrzędną Y niezbędne umieścić tekst, tak aby był wyśrodkowany naprawdę w pionie na stronie. `for` Pętli następnie ustawia rotacji skupia się na środku kanwy. Obrót jest w odstępach 30 stopni. Tekst jest rysowane przy użyciu `yText` wartość. Liczba odstępów przed wyraz "OBRÓĆ" w `text` empirycznie ustalono wartość do nawiązania połączenia między te ciągi 12 wydają się być dodecagon.

Jednym ze sposobów uprościć ten kod jest zwiększenie kąt obrotu o 30 stopni każdorazowo po wykonaniu pętli `DrawText` wywołania. Eliminuje to potrzebę wywołań `Save` i `Restore`. Zwróć uwagę, że `degrees` zmienna nie jest już używany w treści `for` bloku:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Istnieje również możliwość użycia prostego formę `RotateDegrees` przez prefacing pętli wywołaniem `Translate` wszystko przejście do centrum obszaru roboczego:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Zmodyfikowane `yText` obliczania nie zawiera już `yCenter`. Teraz `DrawText` wywołania Wyśrodkowuje tekst w pionie w górnej części obszaru roboczego.

Ponieważ przekształcenia koncepcyjnie są stosowane przeciwnej sposobu ich wyświetlania w kodzie, często jest możliwe transformacje rozpoczynać się od znaku globalnych, następuje więcej lokalne transformacje. Często jest najprostszym sposobem łączenia obrotu i translacji.

Załóżmy na przykład, aby narysować obiektu graficznego obraca się wokół środka podobnie jak planety obrót wokół osi. Ale ma także obracać wokół środka ekranu, podobnie jak planety, obracanie wokół sun się ten obiekt.

Można to zrobić, umieszczając obiekt w lewym górnym rogu obszaru roboczego, i obracać wokół tym rogu przy użyciu animacji. Następnie wykonuje obiektu poziomie jak wytrząsarce radius. Teraz zastosować drugi obracanie animowany również wokół punktu początkowego. Dzięki temu obiekt obracać wokół prawym górnym rogu. Teraz przełożyć na środku kanwy.

Oto `PaintSurface` programu obsługi, który zawiera te przekształcenie wywołań w odwrotnej kolejności:

```csharp
float revolveDegrees, rotateDegrees;
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Red
    })
    {
        // Translate to center of canvas
        canvas.Translate(info.Width / 2, info.Height / 2);

        // Rotate around center of canvas
        canvas.RotateDegrees(revolveDegrees);

        // Translate horizontally
        float radius = Math.Min(info.Width, info.Height) / 3;
        canvas.Translate(radius, 0);

        // Rotate around center of object
        canvas.RotateDegrees(rotateDegrees);

        // Draw a square
        canvas.DrawRect(new SKRect(-50, -50, 50, 50), fillPaint);
    }
}
```

`revolveDegrees` i `rotateDegrees` pola są animowane. Ten program używa innej animacji technikę platformy Xamarin.Forms `Animation` klasy. (Ta klasa jest opisany w [22 rozdział *tworzenia aplikacji mobilnych za pomocą platformy Xamarin.Forms*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)) `OnAppearing` zastąpienie tworzy dwa `Animation` obiekty z metody wywołania zwrotnego, a następnie wywołuje `Commit` na nich dla czasu trwania animacji:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    new Animation((value) => revolveDegrees = 360 * (float)value).
        Commit(this, "revolveAnimation", length: 10000, repeat: () => true);

    new Animation((value) =>
    {
        rotateDegrees = 360 * (float)value;
        canvasView.InvalidateSurface();
    }).Commit(this, "rotateAnimation", length: 1000, repeat: () => true);
}
```

Pierwszy `Animation` animuje obiektu `revolveDegrees` z zakresu od 0 do 360 stopni ponad 10 sekund. Drugi animuje `rotateDegrees` z zakresu od 0 do 360 stopni co 1 sekundę, a także unieważnia powierzchnię na potrzeby generowania innym wywołaniu `PaintSurface` obsługi. `OnDisappearing` Zastąpienie anuluje tych dwóch animacji:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

**Ugly zegar analogowy** (tzw. ponieważ atrakcyjniejsze zegar analogowy będzie opisana w artykule nowszej) używana obrotu Rysuj znaczniki minutę i godzina zegara i Obróć ręce. Program rysuje zegar przy użyciu dowolnego współrzędnych oparte na koło, który skupia się na punkt (0, 0) z protokołem radius 100. Rozwiń węzeł i tym okrąg na stronie Centrum używa tłumaczenia i skalowania.

`Translate` i `Scale` wywołania dotyczą globalnie zegara, dlatego te są pierwszy ma być wywoływana po inicjowanie `SKPaint` obiektów:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    using (SKPaint fillPaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.Color = SKColors.Black;
        strokePaint.StrokeCap = SKStrokeCap.Round;

        fillPaint.Style = SKPaintStyle.Fill;
        fillPaint.Color = SKColors.Gray;

        // Transform for 100-radius circle centered at origin
        canvas.Translate(info.Width / 2f, info.Height / 2f);
        canvas.Scale(Math.Min(info.Width / 200f, info.Height / 200f));
        ...
    }
}

```csharp
There are 60 marks of two different sizes that must be drawn in a circle around the clock. The `DrawCircle` call draws that circle at the point (0, –90), which relative to the center of the clock corresponds to 12:00. The `RotateDegrees` call increments the rotation angle by 6 degrees after every tick mark. The `angle` variable is used solely to determine if a large circle or a small circle is drawn:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        // Hour and minute marks
        for (int angle = 0; angle < 360; angle += 6)
        {
            canvas.DrawCircle(0, -90, angle % 30 == 0 ? 4 : 2, fillPaint);
            canvas.RotateDegrees(6);
        }
    ...
    }
}
```

Na koniec `PaintSurface` obsługi uzyskuje bieżący czas i oblicza stopni obrotu godzinę, minutę i drugi ręce. Każdej strony jest rysowana na pozycji 12:00, tak, aby kąt obrotu względem który:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        DateTime dateTime = DateTime.Now;

        // Hour hand
        strokePaint.StrokeWidth = 20;
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawLine(0, 0, 0, -50, strokePaint);
        canvas.Restore();

        // Minute hand
        strokePaint.StrokeWidth = 10;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawLine(0, 0, 0, -70, strokePaint);
        canvas.Restore();

        // Second hand
        strokePaint.StrokeWidth = 2;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Second);
        canvas.DrawLine(0, 10, 0, -80, strokePaint);
        canvas.Restore();
    }
}
```

Zegar jest oczywiście funkcjonalności, chociaż ręce są raczej surowego:

[![](rotate-images/uglyanalogclock-small.png "Potrójne zrzut ekranu przedstawiający stronę Ugly analogowy zegara tekstu")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
