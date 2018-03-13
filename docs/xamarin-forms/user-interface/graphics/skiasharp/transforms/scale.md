---
title: "Przekształcanie skali"
description: "Odnajdywanie przekształcenia skali SkiaSharp skalowania obiektów do różnych rozmiarach"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: feecfc923903a20332bf3a1a188ab9d7cd2ce1c0
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="the-scale-transform"></a>Przekształcanie skali

_Odnajdywanie przekształcenia skali SkiaSharp skalowania obiektów do różnych rozmiarach_

Jak przedstawiono w [wykonuje przekształcenie](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md) artykułu, przekształcanie Przetłumacz można przenieść obiektu graficznego z jednej lokalizacji do innej. Z kolei przekształcenia skali zmienia rozmiar obiektu graficznego:

![](scale-images/scaleexample.png "Wysokość wyraz skalowany w rozmiarze")

Przekształcenie skali powoduje również często współrzędne grafiki można przenieść, ponieważ są one większe.

Wcześniej był wyświetlany formuły przekształcenia opisujące skutków czynników tłumaczenia `dx` i `dy`:

x' = x + dx

y' = y + dy

Skalowanie czynników `sx` i `sy` są mnożenia zamiast dodatku:

x' = sx · x

y "= sy · y

Wartości domyślne czynników Przetłumacz to 0; wartości domyślne czynników skali to 1.

`SKCanvas` Klasa definiuje cztery `Scale` metody. Pierwszy [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/) metoda jest dla przypadków, gdy ma tego samego skalowanie w pionie i współczynnika:

```csharp
public void Scale (Single s)
```

Jest to nazywane *izotropowego* skalowanie & #x 2014; skalowanie który jest taka sama w obu kierunkach. Skalowanie izotropowego zachowuje współczynnik proporcji obiektu.

Drugi [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) metoda pozwala określić różne wartości skalowanie w pionie i w poziomie:

```csharp
public void Scale (Single sx, Single sy)
```

Powoduje to *anizotropowych* skalowania.
Trzeci [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/SkiaSharp.SKPoint/) metody łączy dwa czynniki skalowania w jednej `SKPoint` wartość:

```csharp
public void Scale (SKPoint size)
```

Czwarta `Scale` metoda będzie opisana wkrótce.

**Podstawowe skali** pokazuje stronę `Scale` metody. [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) pliku XAML zawiera dwa `Slider` elementy, które pozwalają wybierz poziome i pionowe czynniki skalowania od 0 do 10. [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) pliku CodeBehind używa tych wartości do wywołania `Scale` przed wyświetlanie zaokrąglony prostokąt malowania linia przerywana i dopasowana tekst w lewym górnym narożnik obszaru roboczego:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] {  7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        canvas.Scale((float)xScaleSlider.Value,
                     (float)yScaleSlider.Value);

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);

        float margin = 10;
        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Być może zastanawiasz się: jak skalowania czynniki wpływają na wartość zwracana z `MeasureText` metody `SKPaint`? Odpowiedź brzmi: wcale. `Scale` jest to metoda `SKCanvas`. Nie dotyczy żadnych z `SKPaint` obiektu, dopóki nie użyjesz tego obiektu do renderowania coś na kanwie.

Jak widać, wszystko rysowane po `Scale` proporcjonalnie wywołać zwiększa:

[![](scale-images/basicscale-small.png "Potrójna zrzut ekranu strony podstawowej skali")](scale-images/basicscale-large.png#lightbox "Potrójna zrzut ekranu strony podstawowej skali")

Tekst, szerokość linii kreskowanej długość kreski w tym wierszu zaokrąglania narożników i marginesu 10 pikseli od lewej lub górnej krawędzi obszaru roboczego i zaokrąglony prostokąt podlegają wszystkie tego samego czynniki skalowania.

> [!IMPORTANT]
> Platforma uniwersalna systemu Windows nie jest poprawnie renderowana anisotropicly skalowanego tekstu.

Anizotropowych skalowanie przyczyny grubość zostać różnych wierszy wyrównane Osie poziome i pionowe. (Dotyczy również widoczna od pierwszego obrazu na tej stronie). Jeśli nie chcesz grubość dotyczyć czynniki skalowania równa 0 i będzie zawsze równa jeden piksel szeroki niezależnie od tego `Scale` ustawienie.

Skalowanie jest określana względem lewego górnego rogu obszaru roboczego. Może to być dokładnie co ma, ale może nie być. Załóżmy, że chcesz umieścić tekst i prostokąt gdzieś w obszarze roboczym, a użytkownik chce go skalować względem jego center. W takim przypadku można użyć wersji czwarty [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/System.Single/System.Single/) metodę, która obejmuje dwóch dodatkowych parametrów, aby określić Centrum skalowania:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px` i `py` parametry definiują punkt, który jest czasami nazywany *skalowanie center* , ale w SkiaSharp dokumentacji jest określana jako *punktu przestawnego*. To jest punkt względem lewego górnego rogu obszaru roboczego, który nie ma wpływu na skalowanie. Skalowanie wszystkich występuje względem tego Centrum.

[ **Wyśrodkowany skali** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) strony pokazuje, jak to działa. `PaintSurface` Obsługi jest podobny do **podstawowe skali** programu z wyjątkiem `margin` wartość jest obliczana do środka tekstu w poziomie, co oznacza, że program działa najlepiej w trybie portret:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);
        float margin = (info.Width - textBounds.Width) / 2;

        float sx = (float)xScaleSlider.Value;
        float sy = (float)yScaleSlider.Value;
        float px = margin + textBounds.Width / 2;
        float py = margin + textBounds.Height / 2;

        canvas.Scale(sx, sy, px, py);

        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Znajduje się w lewym górnym rogu zaokrąglony prostokąt `margin` pikseli z lewej strony obszaru roboczego i `margin` pikseli od góry. Dwa ostatnie argumenty do `Scale` metody ustawienie tych wartości oraz szerokość i wysokość tekst, który jest także szerokość i wysokość prostokąta zaokrąglone. Oznacza to, że wszystkie skalowania ma względem Centrum tego prostokąta:

[![](scale-images/centeredscale-small.png "Potrójna zrzut ekranu przedstawiający stronę wyśrodkowany skali")](scale-images/centeredscale-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę wyśrodkowany skali")

`Slider` Elementy w tym programie mają zakres & #x 2013; 10 do 10. Jak widać, wartości ujemnych pionowego, skalowania (na przykład w systemie Android ekranu w Centrum) powodują obiektów do przerzucenia wzdłuż osi poziomej, który przechodzi przez Centrum skalowania. Wartości ujemne poziomego skalowania (taki jak ekranu systemu Windows po prawej stronie) powodują obiektów do przerzucenia wokół osi pionowej, który przechodzi przez Centrum skalowania.

Ta wersja czwarty `Scale` metoda jest rzeczywiście skrótu. Możesz zobaczyć, jak to działa przez zastąpienie `Scale` metody w tym kodzie następującym kodem:

```csharp
canvas.Translate(-px, -py);
```

Są to negatywów współrzędnych punktu przestawnego.

Teraz ponownie uruchomić program. Zobaczysz, że prostokąt i tekst są przesuwane tak, aby Centrum znajduje się w lewym górnym rogu obszaru roboczego. Zauważalny go. Suwaki nie działają oczywiście, ponieważ teraz program nie skalować w ogóle.

Teraz dodać podstawowe `Scale` wywołań (bez skalowania center) *przed* który `Translate` wywołania:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Jeśli znasz tego ćwiczenia w innych programowania systemów grafiki, może być uważasz, że jest nieprawidłowy, ale nie jest. Skia obsługuje transformacji kolejne wywołania nieco inaczej z co można zapoznać się z.

Z kolejnych `Scale` i `Translate` wywołań, Centrum zaokrąglony prostokąt nadal znajduje się w lewym górnym rogu, ale teraz można go skalować względem lewego górnego rogu obszaru roboczego, który jest także Centrum zaokrąglony prostokąt.

Teraz, przed którym `Scale` wywołania dodać kolejne `Translate` wywołać wyśrodkowywania wartościami:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Spowoduje to przeniesienie skalowana wynik wstecz do oryginalnego położenia. Te trzy wywołania są równoważne:

```csharp
canvas.Scale(sx, sy, px, py);
```

Poszczególne przekształceń są ich tak, aby formuła całkowita transformacji jest:

 x "= sx · (x — pikseli) + pikseli

 y "= sy · (y — py) + py

Należy pamiętać, że wartości domyślne `sx` i `sy` 1. To proste przekonać się, że punktu obrotu (pikseli, py) nie jest on przekształcany przez te formuły. Pozostaje w tej samej lokalizacji względem obszaru roboczego.

Podczas łączenia `Translate` i `Scale` wywołań, kolejność ma znaczenie. Jeśli `Translate` po `Scale`, czynniki tłumaczenia skutecznie są skalowane czynniki skalowania. Jeśli `Translate` znajduje się przed `Scale`, nie mogą być skalowane czynniki tłumaczenia. Ten proces staje się nieco jaśniejszy (Chociaż więcej matematyczne) gdy wprowadzono przedmiotem macierzy transformacji.

`SKPath` Klasa definiuje tylko do odczytu [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) właściwości, która zwraca `SKRect` definiujący stopień współrzędne w ścieżce. Na przykład, jeśli `Bounds` właściwości są uzyskiwane ze ścieżki hendecagram utworzony wcześniej, `Left` i `Top` właściwości prostokąta są-około 100 `Right` i `Bottom` właściwości około 100 i `Width` i `Height` właściwości są około 200. (Większość rzeczywiste wartości są mniej małego ponieważ punktów gwiazdek są definiowane przez koło z protokołem radius 100, ale górnego punktu jest równoległy osi poziomej lub pionowej).

Dostępność tych informacji oznacza, że powinno być możliwe pochodzi skali i tłumaczyć czynniki odpowiedniego skalowania ścieżki do rozmiaru obszaru roboczego. [ **Anizotropowej skalowanie** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) strony pokazano to z gwiazdką odnosi się do 11. *Anizotropowej* skali oznacza, że jest nierówne w poziomie i w pionie kierunkach, co oznacza, że gwiazdy nie zachowa jego oryginalny współczynnik proporcji. W tym miejscu jest odpowiedni kod `PaintSurface` obsługi:

```csharp
SKPath path = HendecagramPage.HendecagramPath;
SKRect pathBounds = path.Bounds;

using (SKPaint fillPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Pink
})
using (SKPaint strokePaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 3,
    StrokeJoin = SKStrokeJoin.Round
})
{
    canvas.Scale(info.Width / pathBounds.Width,
                 info.Height / pathBounds.Height);
    canvas.Translate(-pathBounds.Left, -pathBounds.Top);

    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

`pathBounds` Prostokąt uzyskane w górnej części tego kodu, a następnie używana później szerokość i wysokość obszaru roboczego w `Scale` wywołania. Czy wywołanie samodzielnie są skalowane współrzędne ścieżki, gdy jest on renderowany przy `DrawPath` wywołanie, ale gwiazdy będzie wyśrodkowany w prawym górnym rogu obszaru roboczego. Musi zostać przesunięty w dół i w lewo. To zadanie z `Translate` wywołania. Te dwie właściwości `pathBounds` są-około 100, więc czynniki tłumaczenia około 100. Ponieważ `Translate` po wywołaniu `Scale` wywołać, te wartości są skalowane skutecznie czynniki skalowania, co ich przenieść w środku kanwy środku gwiazdy:

[![](scale-images/anisotropicscaling-small.png "Potrójna zrzut ekranu strony skalowanie anizotropowych")](scale-images/anisotropicscaling-large.png#lightbox "Potrójna zrzut ekranu strony anizotropowych skalowania")

Należy zwrócić uwagę w inny sposób `Scale` i `Translate` wywołania jest można ustalić skutek w odwrotnej kolejności: `Translate` wywołania przewiduje ścieżka staje się ona pełni widoczny, ale orientacji w lewym górnym rogu obszaru roboczego. `Scale` — Metoda następnie sprawia, że ten gwiazdkę większy względem lewego górnego rogu.

W rzeczywistości wygląda na to, że gwiazdy jest nieco większy niż obszar roboczy. Problem polega na szerokość pociągnięć. `Bounds` Właściwość `SKPath` wskazuje wymiary współrzędne zakodowane w ścieżce i jest program używa skalowania go. Po wyrenderowaniu ścieżki z danego grubość renderowanych ścieżki jest większa niż obszar roboczy.

Aby rozwiązać ten problem należy odpowiednio do tego. Jednym z podejść łatwy w tym programie ma Dodaj następujące uprawnienia instrukcji przed `Scale` wywołania:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Powoduje to zwiększenie `pathBounds` prostokąt 1,5 jednostek ze wszystkich czterech stron. Jest to rozsądne rozwiązanie tylko wtedy, gdy jest zaokrąglana obrysu sprzężenia. Sprzężenie skosów, skos może może być dłuższa i jest trudne do obliczenia.

Podobne technika tekstem, można także używać jako **anizotropowych tekst** pokazuje strony. Oto odpowiedniej części `PaintSurface` programu obsługi [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) klasy:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 0.1f,
    StrokeJoin = SKStrokeJoin.Round
})
{
    SKRect textBounds = new SKRect();
    textPaint.MeasureText("HELLO", ref textBounds);

    // Inflate bounds by the stroke width
    textBounds.Inflate(textPaint.StrokeWidth / 2,
                       textPaint.StrokeWidth / 2);

    canvas.Scale(info.Width / textBounds.Width,
                 info.Height / textBounds.Height);
    canvas.Translate(-textBounds.Left, -textBounds.Top);

    canvas.DrawText("HELLO", 0, 0, textPaint);
}
```

Jest podobne logiki i tekst rozwijany do rozmiaru strony oparte na prostokątne granice tekst zwrócony z `MeasureText` (czyli nieco większy niż rzeczywisty tekst):

[![](scale-images/anisotropictext-small.png "Potrójna zrzut ekranu przedstawiający stronę testową anizotropowej")](scale-images/anisotropictext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę testową anizotropowych")

Jeśli chcesz zachować współczynnik proporcji obiektów graficznych chcesz użyć izotropowego skalowania. **Izotropowego skalowanie** strony pokazano to odnosi się do 11 gwiazdki. Koncepcyjnie kroki do wyświetlania obiektu graficznego na środku strony z izotropowego skalowanie są:

- Tłumaczenie Centrum obiektu graficznego do lewego górnego rogu.
- Skalowanie obiektu, w oparciu o minimalnym wymiary strony poziome i pionowe rozdzielonych wymiary obiektu graficznego.
- Tłumaczenie Centrum skalowany obiekt do środka strony.

[ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) Wykonuje te czynności w kolejności odwrotnej przed wyświetleniem gwiazdy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPath path = HendecagramArrayPage.HendecagramPath;
    SKRect pathBounds = path.Bounds;

    using (SKPaint fillPaint = new SKPaint())
    {
        fillPaint.Style = SKPaintStyle.Fill;

        float scale = Math.Min(info.Width / pathBounds.Width,
                               info.Height / pathBounds.Height);

        for (int i = 0; i <= 10; i++)
        {
            fillPaint.Color = new SKColor((byte)(255 * (10 - i) / 10),
                                          0,
                                          (byte)(255 * i / 10));
            canvas.Save();
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(scale);
            canvas.Translate(-pathBounds.MidX, -pathBounds.MidY);
            canvas.DrawPath(path, fillPaint);
            canvas.Restore();

            scale *= 0.9f;
        }
    }
}
```

Kod wyświetla również gwiazdy dziesięć razy, zawsze zmniejszenie skalowanie dwuetapowego przy 10% i stopniowo zmiana koloru z kolor czerwony, niebieski:

[![](scale-images/isotropicscaling-small.png "Potrójna zrzut ekranu przedstawiający stronę izotropowego skalowanie")](scale-images/isotropicscaling-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę izotropowego skalowania")


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
