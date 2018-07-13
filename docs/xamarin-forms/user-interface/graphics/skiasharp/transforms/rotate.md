---
title: Obrót — przekształcenie
description: W tym artykule przedstawiono wpływ i animacji, które można zrobić za pomocą SkiaSharp obrót — przekształcenie i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: cbb34fb4887fc3fa086fa9912d25addebd9b13f2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995003"
---
# <a name="the-rotate-transform"></a>Obrót — przekształcenie

_Poznaj efekty i animacji, które można zrobić za pomocą SkiaSharp obrót — przekształcenie_

Obróć przekształcania obiektów graficznych SkiaSharp uwolnienie się od ograniczenia wyrównania z osiami poziome i pionowe:

![](rotate-images/rotateexample.png "Tekst obrócona wokół środka")

Do rotacji obiekt graficzny wokół punktu (0, 0), SkiaSharp obsługuje zarówno [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/) metody i [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/) metody:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

Okrąg 360 stopni jest taka sama jak wartość w radianach 2π, dzięki czemu można łatwo przekonwertować między dwiema jednostkami. Użyj, która kwota jest wygodne. Wszystkie funkcje trygonometryczne statycznej [ `Math` ](xref:System.Math) klasy użycia jednostek radianów.

Obrót jest zwiększenia kąty obrotu w prawo. (Mimo że przeciwnie do ruchu wskazówek zegara, zgodnie z Konwencją obrotu w układzie współrzędnych formułuje obrót w prawo wokół jest zgodna z współrzędne Y zwiększenie przechodząc w dół.) Ujemna kąty i kąty większy, niż jest dopuszczalne 360 stopni.

Formuły transformacji dla obrotu są bardziej skomplikowane niż te dotyczące translate i skalowania. Dla kąta α formuły przekształcenia są:

x' = x•cos(α) – y•sin(α)   

y` = x•sin(α) + y•cos(α)

**Podstawowa, Obróć** pokazuje strony `RotateDegrees` metody. [ `BasicRotate.xaml.cs` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) Plik wyświetla tekst z linią bazową, ich tematyka koncentruje się na stronie i obraca się ona na podstawie `Slider` z szerokim zakresem – 360 do 360. Oto odpowiednia część `PaintSurface` procedury obsługi:

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

Ponieważ obrotu skupia się na lewym górnym rogu kanwy dla większości kąty, w tym programie tekst jest obracany mieściły się na ekranie:

[![](rotate-images/basicrotate-small.png "Potrójna zrzut ekranu przedstawiający stronę podstawowa, Obróć")](rotate-images/basicrotate-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę podstawowa, Obróć")

Bardzo często należy obrócić coś skupia się wokół punktu obrotu określony za pomocą te wersje [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/System.Single/System.Single/) i [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/System.Single/System.Single/) metody:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**Wyśrodkowany Obróć** strona jest podobnie jak **podstawowa, Obróć** z tą różnicą, że rozszerzona wersja `RotateDegrees` służy do ustawiania środek obrotu do tego samego punktu, które są używane, aby umieścić tekst:

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

Teraz tekst obraca się wokół punktu do określania położenia tekst, który jest środka linii bazowej tekstu:

[![](rotate-images/centeredrotate-small.png "Potrójna zrzut ekranu strony a ich tematyka Obróć")](rotate-images/centeredrotate-large.png#lightbox "Potrójna zrzut ekranu strony a ich tematyka Obróć")

Tak jak w przypadku wersji wyśrodkowany `Scale` metody, wyśrodkowany wersję `RotateDegrees` wywołanie jest skrót:

```csharp
RotateDegrees (degrees, px, py);
```

To jest odpowiednikiem następujących czynności:

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Dowiesz się, że czasami można łączyć `Translate` wywołań za pomocą `Rotate` wywołania. Na przykład, w tym miejscu są `RotateDegrees` i `DrawText` wywołuje **wyśrodkowany Obróć** stronie;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees` Wywołanie jest równoważne z dwóch `Translate` wywołań i innych — a ich tematyka `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText` Wywołania do wyświetlania tekstu w danej lokalizacji jest odpowiednikiem `Translate` wywołać wobec tej lokalizacji, a następnie `DrawText` w momencie (0, 0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

Dwa kolejne znaki `Translate` wywołania anulować siebie:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

Model dwóch transformacje są stosowane w kolejności przeciwnej jak pojawiają się w kodzie. `DrawText` Wywołania tekst jest wyświetlany w lewym górnym rogu kanwy. `RotateDegrees` Wywołanie obraca się ten tekst względem lewego górnego rogu. A następnie `Translate` wywołanie przenosi tekst na środku kanwy.

Są zazwyczaj łączyć rotacji i tłumaczenia na kilka sposobów. **Tekst obrócony** strony tworzy następujące:

[![](rotate-images/rotatedtext-small.png "Potrójna zrzut ekranu przedstawiający stronę tekst obrócony")](rotate-images/rotatedtext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę tekst obrócony")

Oto `PaintSurface` program obsługi [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) klasy:

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

`xCenter` i `yCenter` wartości wskazują na środku kanwy. `yText` Wartość jest nieco przesunięcie przy jego użyciu. Oznacza to niezbędne umieścić tekst tak, aby go jest naprawdę wyśrodkowane w pionie na stronie współrzędną Y. `for` Pętli jest następnie Ustawia rotację tematyka koncentruje się na środku kanwy. Obrót znajduje się w przyrostach co 30 stopni. Tekst jest rysowany przy użyciu `yText` wartość. Liczba spacji przed słowem "OBRÓĆ" w `text` wartość ustalono empirically, aby nawiązać połączenie między te ciągi tekstowe 12 wydają się być dodecagon.

Jednym ze sposobów, aby uprościć ten kod jest zwiększenie kąt obrotu w stopniach 30 każdorazowo po wykonaniu pętli `DrawText` wywołania. Eliminuje to potrzebę wywołania `Save` i `Restore`. Należy zauważyć, że `degrees` zmienna nie jest już używany w treści `for` bloku:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Istnieje również możliwość użycia prosty formularz `RotateDegrees` przez prefacing pętli z wywołaniem `Translate` przenieść wszystkiego na środku kanwy:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Zmodyfikowanego `yText` obliczeń nie zawiera już `yCenter`. Teraz `DrawText` wywołanie wyśrodkowanie tekstu w pionie w górnej części kanwy.

Transformacje są koncepcyjnie stosowane przeciwnej jak pojawiają się w kodzie, dlatego jest często można rozpoczynać się bardziej globalnego przekształceń, następuje więcej lokalne transformacje. Często jest najprostszym sposobem łączenia rotacji i tłumaczenia.

Na przykład załóżmy, że chcemy narysować obiekt graficzny, obraca się wokół jej środka, podobnie jak w świecie z rotacji wokół osi. Ale ma również obracać wokół środka ekranu, podobnie jak globalnej z obrotowymi słońce się ten obiekt.

Można to zrobić, ustawiając obiekt w lewym górnym rogu kanwy i Obróć w tym rogiem przy użyciu animacji. Następnie wykonuje translację elementu obiektu w poziomie, takich jak wytrząsarce usługi radius. Teraz je zastosować rotację animowany drugi, również wokół punktu początkowego. To sprawia, że obiekt koncentrują się na prawym górnym rogu. Teraz Przetłumacz na środku kanwy.

Oto `PaintSurface` program obsługi, który zawiera te Przekształcanie wywołań w odwrotnej kolejności:

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

`revolveDegrees` i `rotateDegrees` pola są animowane. Ten program używa technika inną animację, oparte na Xamarin.Forms `Animation` klasy. (Ta klasa jest opisana w [22 rozdział *tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)) `OnAppearing` zastąpienie tworzy dwa `Animation` obiektów przy użyciu metod wywołania zwrotnego, a następnie wywołuje `Commit` na nich na czas trwania animacji:

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

Pierwszy `Animation` animuje obiektu `revolveDegrees` z zakresu od 0 do 360 stopni ponad 10 sekund. Drugi animuje `rotateDegrees` z zakresu od 0 do 360 stopni co 1 sekundę, a także unieważnia powierzchni, aby wygenerować inne wywołanie `PaintSurface` programu obsługi. `OnDisappearing` Zastąpienie anuluje tych dwóch animacji:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

**Nieładnego zegar analogowy** (tzw. ponieważ bardziej atrakcyjne zegar analogowy są opisane w artykule nowszych) jest używana obrotu Rysuj znaczniki minutę i godzina zegara i Obróć ręce. Program rysuje zegara przy użyciu dowolnego układ współrzędnych oparty na okręgu, który skupia się w momencie (0, 0) z protokołem radius 100. Aby rozwinąć ten okrąg na stronie Centrum używa przesunięcia i skalowania.

`Translate` i `Scale` wywołania stosowane globalnie do zegar, dzięki czemu te są tymi pierwszy do wywołania po inicjowaniu `SKPaint` obiektów:

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

Na koniec `PaintSurface` obsługi uzyskuje bieżącego czasu i oblicza stopni obrotu godzinę, minutę i drugi ręce. Każdej strony jest rysowana w położeniu 12:00, tak, względem którego kąt obrotu:

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

Zegar jest bez obaw funkcjonalności, mimo że wskazówki są zamiast surowych:

[![](rotate-images/uglyanalogclock-small.png "Zrzut ekranu przedstawiający stronę Nieładnego tekst zegara analogowy potrójne")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
