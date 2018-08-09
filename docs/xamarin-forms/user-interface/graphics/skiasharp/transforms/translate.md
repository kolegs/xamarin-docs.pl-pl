---
title: Przekład — przekształcenie
description: Artykuł examiens jak shift grafiki SkiaSharp w aplikacjach Xamarin.Forms za pomocą przekład — przekształcenie i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 02361b5b2d00015ce168c075dc19522b6c04e446
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615449"
---
# <a name="the-translate-transform"></a>Przekład — przekształcenie

_Dowiedz się, jak shift SkiaSharp grafiki za pomocą przekład — przekształcenie_

To najprostszy typ przekształcenia w SkiaSharp *tłumaczenie* lub *tłumaczenia* przekształcania. Ta transformacja przenosi obiektów graficznych w kierunku poziomym i pionowym. W tym sensie tłumaczenie jest najbardziej niepotrzebne transformacji, ponieważ zwykle można osiągnąć ten sam efekt, po prostu zmieniając współrzędne, których używasz do rysowania funkcji. Podczas renderowania ścieżki, jednak wszystkie współrzędne są hermetyzowane w ścieżce, więc jest znacznie łatwiejsze, stosując przekład — przekształcenie do przesunięcia pełną ścieżkę.

Tłumaczenie jest również przydatne podczas tworzenia animacji i efekty prosty tekst:

![](translate-images/translateexample.png "Tekst w tle, grawerowanie i płaskorzeźba za pomocą tłumaczenia")

[ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) Method in Class metoda `SKCanvas` zawiera dwa parametry, które powodują obiektów graficznych później rysowane są przesuwane w poziomie i w pionie:

```csharp
public void Translate (Single dx, Single dy)
```

Tych argumentów może być ujemna. Sekundy [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/SkiaSharp.SKPoint/) metoda łączy wartości dwóch tłumaczenia w jednym `SKPoint` wartość:

```csharp
public void Translate (SKPoint point)
```

**Zgromadzonych tłumaczenie** strony [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) przykładowy program pokazuje, że wiele wywołań z `Translate` kumulują metody. [ `AccumulatedTranslate` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) Klasy Wyświetla 20 wersje tego samego prostokąt, w każdej z nich przesunięcie od poprzedniego prostokąt wystarczą, dzięki czemu mogą rozciągnąć wzdłuż przekątnej. Oto `PaintSurface` program obsługi zdarzeń:

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

Kolejne prostokąty ścieknie niżej na tej stronie:

[![](translate-images/accumulatedtranslate-small.png "Potrójna zrzut ekranu przedstawiający stronę zgromadzonych tłumaczenie")](translate-images/accumulatedtranslate-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę zgromadzonych tłumaczenie")

Jeśli czynniki skumulowana tłumaczenia `dx` i `dy`, i określ rysowania funkcji jest (`x`, `y`), obiekt graficzny jest renderowany w momencie a następnie (`x'`, `y'`), gdzie:

x' = x + dx

y "=, y + dy

Są to znane jako *Przekształcanie formuły* tłumaczenia. Z wartościami domyślnymi `dx` i `dy` nowej `SKCanvas` to 0.

Często jest na użytek przekład — przekształcenie efekty i podobne techniki jako **efekty tłumaczenie tekstu** pokazuje strony. Oto odpowiednia część `PaintSurface` obsługi w [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) klasy:

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

W każdym trzy przykłady `Translate` nosi nazwę do wyświetlania tekstu, aby przesunąć go w lokalizacji określonej przez `x` i `y` zmiennych. Następnie tekst jest wyświetlany ponownie w innym kolorem, bez zmiany tłumaczenia:

[![](translate-images/translatetexteffects-small.png "Potrójna zrzut ekranu przedstawiający stronę efekty tłumaczenie tekstu")](translate-images/translatetexteffects-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę efekty tłumaczenie tekstu")

Każdego z trzech przykładach przedstawiono inną metodę Negacja `Translate` wywołania:

Pierwszy przykład wywołuje po prostu `Translate` ponownie, ale z wartości ujemnych. Ponieważ `Translate` kumulują się wywołania, ta sekwencja wywołań przywraca po prostu łączna liczba tłumaczeń do wartości zero.

Drugi przykład wywołuje [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix()/). To powoduje, że wszystkie przekształcenia powrócić do stanu domyślnego.

Trzeci przykład zapisuje stan z `SKCanvas` obiektu z wywołaniem [ `Save` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Save()/) i następnie przywraca stan z wywołaniem [ `Restore` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Restore/). Jest to najbardziej wszechstronny sposób do manipulowania przekształceń dla szeregu operacji rysowania. Te `Save` i `Restore` wywołuje funkcję, takich jak stos: możesz wywołać `Save` wiele czasu, a następnie wywołania `Restore` w odwrotnej kolejności, aby powrócić do poprzedniego stanu. `Save` Metoda zwraca liczbę całkowitą, a następnie można przekazać tego liczbę całkowitą [ `RestoreToCount` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RestoreToCount/) skutecznie wywołać `Restore` wiele razy. [ `SaveCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.SaveCount/) Właściwość zwraca liczbę stanów obecnie zapisane na stosie.

Jednak nie trzeba martwić się o przekształceń przesunięcia z jedno wywołanie elementu `PaintSurface` obsługi do następnego. Każde nowe wywołanie `PaintSurface` oferuje świeży `SKCanvas` obiektu przy użyciu domyślnego transformacji.

Innym typowym zastosowaniem `Translate` przekształcenie jest renderowanie obiekt wizualny, który został pierwotnie utworzony za pomocą współrzędnych, które są wygodne do rysowania. Na przykład możesz chcieć określić współrzędne zegar analogowy z Centrum w momencie (0, 0). Następnie można użyć transformacji do wyświetlania go znajdzie. Jest to zaprezentowane w [**tablicy Hendecagram**] strony. [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs) Klasy zaczyna się od utworzenia `SKPath` obiekt wskazywany przez 11 star. `HendecagramPath` Obiekt jest zdefiniowany jako publiczne, statyczne i tylko do odczytu, tak aby był on dostępny z innych programów demonstracyjnych. Jest on tworzony w konstruktorze statycznym:

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

Jeśli środku gwiazdy znajduje punkt (0, 0), wszystkie punkty gwiazdy znajdują się na kółko wokół tego punktu. Każdy punkt jest kombinacją wartości funkcji sinus i cosinus kąta, która zwiększa się o 5/11ths 360 stopni. (Istnieje również możliwość utworzenia wskazywany przez 11 star, zwiększając przez 2/11, 3/11ths lub 4/11 koła.) Promień koła tego jest ustawiony jako 100.

Jeśli ta ścieżka jest renderowany bez jakichkolwiek plików transformacji, Centrum zostanie umieszczony w lewym górnym rogu `SKCanvas`i tylko jednej czwartej go będzie widoczny. `PaintSurface` Program obsługi `HendecagramPage` zamiast tego używa `Translate` do kafelka kanwy przy użyciu wielu kopii Star, każdy z nich losowo pokolorowane:

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

Poniżej przedstawiono wyniki:

[![](translate-images/hendecagramarray-small.png "Potrójna zrzut ekranu przedstawiający stronę tablicy Hendecagram")](translate-images/hendecagramarray-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Hendecagram tablicy")

Animacje często obejmują transformacje. **Animacji Hendecagram** strony przenosi star wskazywany przez 11 w okręgu. [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) Klasy rozpoczyna się od niektórych pól i przesłonięć o `OnAppearing` i `OnDisappearing` metody, aby uruchomić i zatrzymać czasomierz Xamarin.Forms:

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

`angle` Pola jest animowany z zakresu od 0 do 360 stopni co 5 sekund. `PaintSurface` Program obsługi używa `angle` właściwości na dwa sposoby: do określenia odcień koloru `SKColor.FromHsl` metody i jako argument do `Math.Sin` i `Math.Cos` metod określających lokalizacji gwiazdy:

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

`PaintSurface` Obsługi zdarzeń wywołuje `Translate` metodę dwa razy, aby najpierw przełożyć na środku kanwy, a następnie przełożyć na obwód koła skupia się wokół (0, 0). Promień koła jest ustawiana na możliwie jak największy jednocześnie zachowując gwiazdki w obrębie strony:

[![](translate-images/hendecagramanimation-small.png "Potrójna zrzut ekranu przedstawiający stronę animacji Hendecagram")](translate-images/hendecagramanimation-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Hendecagram animacji")

Należy zauważyć gwiazdki zachowuje tę samą orientację, zgodnie z jego opiera się funkcjonowanie wokół środka strony. Obrotu nie jest w ogóle. To zadanie dla obrót — przekształcenie.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
