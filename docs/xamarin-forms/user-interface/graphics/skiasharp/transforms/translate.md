---
title: "Przekształcanie Przetłumacz"
description: "Używanie transformacji Przetłumacz przesunięcie SkiaSharp grafiki"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 491c82406dafceb876ddbb4a0a7204447b95f57d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="the-translate-transform"></a>Przekształcanie Przetłumacz

_Używanie transformacji Przetłumacz przesunięcie SkiaSharp grafiki_

Najprostsza typ przekształcenie w SkiaSharp jest *tłumaczenie* lub *tłumaczenia* transformacji. Tej transformacji przewiduje obiektów graficznych w poziomie i w pionie kierunkach. W tym sensie tłumaczenia jest najbardziej niepotrzebnych przekształcania, ponieważ ten sam efekt można zwykle osiągnąć, zmieniając po prostu współrzędne używane w funkcji rysowania. Podczas renderowania ścieżki, jednak wszystkie współrzędne znajdują się w ścieżce, dlatego jest znacznie łatwiejsze, zastosowanie transformacji Przetłumacz, które mają zostać przesunięte na całej ścieżce.

Tłumaczenie jest również przydatne do animacji i efekty prosty tekst:

![](translate-images/translateexample.png "Cień tekstu, grawerowanie i płaskorzeźba z tłumaczenia")

[ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) Metoda `SKCanvas` zawiera dwa parametry, które powodują obiektów graficznych następnie narysowanego lekkie w poziomie i w pionie:

```csharp
public void Translate (Single dx, Single dy)
```

Tych argumentów może być ujemny. Drugi [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/SkiaSharp.SKPoint/) metody łączy dwa translacji wartości w jednej `SKPoint` wartość:

```csharp
public void Translate (SKPoint point)
```

**Zgromadzonych tłumaczenie** strony [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) przedstawiono przykładowy program wiele wywołań z `Translate` kumulują metody. [ `AccumulatedTranslate` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) Klasa Wyświetla 20 wersje tego samego prostokąta, każdej z nich przesunięcie od poprzedniego prostokąt tylko wystarczająco tak rozciągają się wraz z przekątnej. Oto `PaintSurface` obsługi zdarzeń:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Color = SKColors.Black;
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = 3;

        int rectangleCount = 20;
        SKRect rect = new SKRect(0, 0, 250, 250);
        float xTranslate = (info.Width - rect.Width) / (rectangleCount - 1);
        float yTranslate = (info.Height - rect.Height) / (rectangleCount - 1);

        for (int i = 0; i < rectangleCount; i++)
        {
            canvas.DrawRect(rect, strokePaint);
            canvas.Translate(xTranslate, yTranslate);
        }
    }
}
```

Kolejne prostokąty ścieknie w dół strony:

[![](translate-images/accumulatedtranslate-small.png "Potrójna zrzut ekranu strony zgromadzonych tłumaczenie")](translate-images/accumulatedtranslate-large.png "Potrójna zrzut ekranu strony zgromadzonych tłumaczenia")

Jeśli czynniki skumulowany tłumaczenia `dx` i `dy`, i określić w funkcji rysowania punkt znajduje się (`x`, `y`), a następnie obiektu graficznego jest renderowany w punkcie (`x'`, `y'`), gdzie:

x' = x + dx

y' = y + dy

Są one nazywane *przekształcenie formuły* translacji. Wartości domyślne `dx` i `dy` nowej `SKCanvas` to 0.

Często do używania przekształcenia Przetłumacz efekty cienia i podobnych technik jako **tłumaczenie efektów tekstowych** pokazuje strony. Oto odpowiedniej części `PaintSurface` obsługi w [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) klasy:

```csharp
float textSize = 150;

using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = textSize;
    textPaint.FakeBoldText = true;

    float x = 10;
    float y = textSize;

    // Shadow
    canvas.Translate(10, 10);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("SHADOW", x, y, textPaint);
    canvas.Translate(-10, -10);
    textPaint.Color = SKColors.Pink;
    canvas.DrawText("SHADOW", x, y, textPaint);

    y += 2 * textSize;

    // Engrave
    canvas.Translate(-5, -5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("ENGRAVE", x, y, textPaint);
    canvas.ResetMatrix();
    textPaint.Color = SKColors.White;
    canvas.DrawText("ENGRAVE", x, y, textPaint);

    y += 2 * textSize;

    // Emboss
    canvas.Save();
    canvas.Translate(5, 5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("EMBOSS", x, y, textPaint);
    canvas.Restore();
    textPaint.Color = SKColors.White;
    canvas.DrawText("EMBOSS", x, y, textPaint);
}
```

W każdej z trzech przykładach `Translate` nosi nazwę do wyświetlania tekstu do przesunięcia ją z lokalizacji określonej przez `x` i `y` zmiennych. Następnie tekst jest wyświetlany ponownie w innym kolorze bez zmiany tłumaczenia:

[![](translate-images/translatetexteffects-small.png "Potrójna zrzut ekranu przedstawiający stronę tłumaczenie efektów tekstowych")](translate-images/translatetexteffects-large.png "Potrójna zrzut ekranu przedstawiający stronę tłumaczenie efektów tekstowych")

Każdego z trzech przykładach przedstawiono inną metodę Negacja `Translate` wywołania:

Wywołuje po prostu pierwszym przykładzie `Translate` ponownie, ale wartości ujemnych. Ponieważ `Translate` wywołania są aktualizacjami zbiorczymi, ta sekwencja wywołania po prostu przywraca całkowita tłumaczenia domyślne wartości zero.

Drugim przykładzie wywołuje [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix()/). Powoduje to, że wszystkie transformacje powrócić do stanu domyślnego.

Przykład trzeci zapisuje stan z `SKCanvas` obiektu o wywołaniu [ `Save` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Save()/) , a następnie przywraca stan wywołaniem [ `Restore` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Restore/). Jest to najbardziej wszechstronny sposób do manipulowania przekształceń dla operacje rysowania serii. Te `Save` i `Restore` wywołania funkcji, takich jak stosu: można wywołać `Save` wiele czasu, a następnie wywołania `Restore` w odwrotnej kolejności, aby powrócić do poprzedniego stanu. `Save` Metoda zwraca liczbę całkowitą i można przekazać tej liczby całkowitej w celu [ `RestoreToCount` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RestoreToCount/) skutecznie wywołać `Restore` wiele razy. [ `SaveCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.SaveCount/) Właściwość zwraca liczbę stanów obecnie zapisane na stosie.

Jednak nie trzeba martwić transformacje przenoszonych z jednego wywołania `PaintSurface` obsługi do następnego. Każde nowe wywołanie `PaintSurface` zapewnia świeża `SKCanvas` obiektu przy użyciu domyślnego transformacji.

Inne typowe zastosowanie `Translate` jest transformacji renderowania obiekt wizualny, który pierwotnie utworzono za pomocą współrzędnych względnych w dogodnym do rysowania. Na przykład można określić współrzędne zegar analogowy z Centrum w punkcie (0, 0). Następnie można użyć transformacji do wyświetlenia jego miejscu. To jest przedstawiona w [**tablicy Hendecagram**] strony. [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs) Klasy rozpoczyna się od utworzenia `SKPath` obiektu odnosi się do 11 gwiazdki. `HendecagramPath` Obiektu jest zdefiniowana jako publiczne, statyczne i tylko do odczytu, aby był on dostępny z innych programów pokazu. Jest on tworzony w konstruktorze statycznym:

```csharp
public class HendecagramArrayPage : ContentPage
{
    ...
    public static readonly SKPath HendecagramPath;

    static HendecagramArrayPage()
    {
        // Create 11-pointed star
        HendecagramPath = new SKPath();
        for (int i = 0; i < 11; i++)
        {
            double angle = 5 * i * 2 * Math.PI / 11;
            SKPoint pt = new SKPoint(100 * (float)Math.Sin(angle),
                                    -100 * (float)Math.Cos(angle));
            if (i == 0)
            {
                HendecagramPath.MoveTo(pt);
            }
            else
            {
                HendecagramPath.LineTo(pt);
            }
        }
        HendecagramPath.Close();
    }
}
```

Jeśli środku gwiazdy znajduje się punkt (0, 0), wszystkie punkty gwiazdy są okrąg otaczającego tego punktu. Każdy punkt jest kombinacją wartości sinus i cosinus kąta, który zwiększa się 5/11ths 360 stopni. (Istnieje również możliwość tworzenie gwiazdkę odnosi się do 11 przez zwiększenie przez 2/11, 3/11ths lub 4/11 okręgu.) Promień tego okręgu jest ustawiony jako 100.

Jeśli ta ścieżka jest renderowany bez transformacje, Centrum zostanie umieszczony w lewym górnym rogu `SKCanvas`i tylko kwartału jego będą widoczne. `PaintSurface` Obsługi `HendecagramPage` użyje `Translate` na kafelku obszaru roboczego z wielu kopii gwiazdy, każda z nich losowo pokolorowane:

```csharp
public class HendecagramArrayPage : ContentPage
{
    Random random = new Random();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            for (int x = 100; x < info.Width + 100; x += 200)
                for (int y = 100; y < info.Height + 100; y += 200)
                {
                    // Set random color
                    byte[] bytes = new byte[3];
                    random.NextBytes(bytes);
                    paint.Color = new SKColor(bytes[0], bytes[1], bytes[2]);

                    // Display the hendecagram
                    canvas.Save();
                    canvas.Translate(x, y);
                    canvas.DrawPath(HendecagramPath, paint);
                    canvas.Restore();
                }
        }
    }
}

```

W tym miejscu jest wynikiem:

[![](translate-images/hendecagramarray-small.png "Potrójna zrzut ekranu przedstawiający stronę tablicy Hendecagram")](translate-images/hendecagramarray-large.png "Potrójna zrzut ekranu przedstawiający stronę Hendecagram tablicy")

Animacje obejmują często transformacji. **Animacji Hendecagram** strony przenosi gwiazdy odnosi się do 11 okręgu. [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) Klasy rozpoczyna się od niektórych pól i zastąpień o `OnAppearing` i `OnDisappearing` metody, aby uruchomić i zatrzymać czasomierza platformy Xamarin.Forms:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    const double cycleTime = 5000;      // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float angle;

    public HendecagramAnimationPage()
    {
        Title = "Hedecagram Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            angle = (float)(360 * t);
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

`angle` Pole jest animowany z zakresu od 0 do 360 stopni co 5 sekund. `PaintSurface` Program obsługi używa `angle` właściwości na dwa sposoby: Aby określić hue koloru w `SKColor.FromHsl` metody i jako argument `Math.Sin` i `Math.Cos` metod określających lokalizacji gwiazdy:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.Translate(info.Width / 2, info.Height / 2);
        float radius = (float)Math.Min(info.Width, info.Height) / 2 - 100;

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColor.FromHsl(angle, 100, 50);

            float x = radius * (float)Math.Sin(Math.PI * angle / 180);
            float y = -radius * (float)Math.Cos(Math.PI * angle / 180);
            canvas.Translate(x, y);
            canvas.DrawPath(HendecagramPage.HendecagramPath, paint);
        }
    }
}
```

`PaintSurface` Wywołań obsługi `Translate` metody dwa razy, aby najpierw przełożyć na środku kanwy, a następnie przełożyć na obwód koła skupia się wokół (0, 0). Promień okrągłego ustawiono być możliwie jak największy, jednocześnie zachowując gwiazdy w obrębie strony:

[![](translate-images/hendecagramanimation-small.png "Potrójna zrzut ekranu przedstawiający stronę animacji Hendecagram")](translate-images/hendecagramanimation-large.png "Potrójna zrzut ekranu przedstawiający stronę Hendecagram animacji")

Zwróć uwagę, że problemem go na środku strony gwiazdy utrzymuje tę samą orientację. Nie Obróć wcale. To zadanie obracania transformacji.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
