---
title: Skalowanie — przekształcenie
description: Artykuł Thhis Eksploruje SkiaSharp skalowanie — przekształcenie skalowania obiektów do różnych rozmiarów i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: 94105cbb83e4c6eb3558ca3fc55e505ab41f28fe
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615606"
---
# <a name="the-scale-transform"></a>Skalowanie — przekształcenie

_Odkryj SkiaSharp skalowanie — przekształcenie skalowania obiektów do różnych rozmiarów_

Jak przedstawiono w [Przekształcanie tłumaczenie](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md) artykule przekład — przekształcenie można przenieść obiekt graficzny z jednej lokalizacji do innej. Z kolei skalowanie — Przekształcenie zmienia rozmiar obiektu graficznego:

![](scale-images/scaleexample.png "Wysokość programu word, skalowana w rozmiarze")

Skalowanie — przekształcenie często powoduje, że współrzędne grafiki przenieść, ponieważ są one większe.

Wcześniej był wyświetlany dwie formuły przekształcenia, które opisują wpływu czynników tłumaczenia `dx` i `dy`:

x' = x + dx

y "=, y + dy

Skalowanie czynników `sx` i `sy` są mnożenia zamiast dodatku:

x' = sx · x

y "= XI sy y

Wartości domyślne czynników translate to 0. wartości domyślne współczynników skali to 1.

`SKCanvas` Klasa definiuje cztery `Scale` metody. Pierwszy [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/) jest metoda przypadków, gdy ten sam skalowanie w pionie i wziąć pod uwagę:

```csharp
public void Scale (Single s)
```

Jest to nazywane *izotropowego* skalowanie &mdash; skalowania oznacza to taka sama w obu kierunkach. Skalowanie izotropowego zachowuje współczynnik proporcji obiektu.

Drugi [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) metody umożliwia określenie różnych wartości skalowanie w pionie i w poziomie:

```csharp
public void Scale (Single sx, Single sy)
```

Skutkuje to *anizotropowego* skalowania.
Trzeci [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/SkiaSharp.SKPoint/) metoda obejmuje dwa czynniki skalowania, w ramach pojedynczej `SKPoint` wartość:

```csharp
public void Scale (SKPoint size)
```

Czwarty `Scale` metody są opisane wkrótce.

**Podstawowe skalowania** pokazuje strony `Scale` metody. [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) plik XAML zawiera dwa `Slider` elementy, które pozwalają wybrać poziome i pionowe czynniki skalowania od 0 do 10. [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) pliku związanego z kodem używa tych wartości w celu wywołania `Scale` przed wyświetleniem prostokąt zaokrąglony malowania linia przerywana i dopasowana jakiś tekst w lewym górnym rogu rogu kanwy:

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

Być może zastanawiasz się: jak skalowania czynniki wpływają na wartość zwracana z `MeasureText` metody `SKPaint`? Odpowiedź brzmi: wcale. `Scale` jest to metoda `SKCanvas`. Nie dotyczy ono niczego, możesz zrobić z `SKPaint` obiektu, dopóki ten obiekt jest używany do renderowania coś na kanwie.

Jak widać, wszystko, co jest rysowana po `Scale` proporcjonalnie wywołać zwiększa:

[![](scale-images/basicscale-small.png "Potrójna zrzut ekranu przedstawiający stronę podstawowe Skala")](scale-images/basicscale-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę podstawowe Skala")

Tekst, szerokość linii kreskowanej długość kreski, w tym wierszu Zaokrąglanie narożników i marginesu 10 pikseli między lewym i górnym krawędzi kanwy i prostokąt zaokrąglony podlegają wszystkich tych samych czynników skalowania.

> [!IMPORTANT]
> Platforma Universal Windows nie jest poprawnie renderowana anisotropicly skalowany tekst.

Anizotropowe, skalowanie powoduje, że szerokość pociągnięcia przestanie różnych wierszy powiązana z poziome i pionowe osi. (Jest to również widoczne z pierwszą ilustracją na tej stronie.) Jeśli nie chcesz szerokość pociągnięcia dotyczyły czynniki skalowania równa 0 i będzie zawsze szerokości niezależnie od tego jednego piksela `Scale` ustawienie.

Skalowanie jest określana względem lewego górnego rogu kanwy. Może to być dokładnie co chcesz zrobić, ale może nie być. Załóżmy, że chcesz umieścić tekst i prostokąt w innym miejscu na kanwie, a użytkownik chce przeskalujesz ją względem jej środka. W takim przypadku można użyć wersji czwarty [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/System.Single/System.Single/) metody, która zawiera dwa dodatkowe parametry do określenia środek skalowania:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px` i `py` parametry definiują punkt, który jest czasem nazywany *skalowanie Centrum* , ale w skiasharp — dokumentacja jest nazywany *punkt obrotu*. Jest to punkt względem lewego górnego rogu kanwy, która nie ma wpływu skalowania. Skalowanie wszystkich występuje względem tego Centrum.

[ **Skali, a ich tematyka** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) strona przedstawia, jak to działa. `PaintSurface` Program obsługi jest podobny do **podstawowe skalowania** programu, chyba że `margin` wartość jest obliczana na wyśrodkowanie tekstu w poziomie, co oznacza, że program działa najlepiej w orientacji pionowej:

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

Znajduje się w lewym górnym rogu prostokąt zaokrąglony `margin` pikseli od lewej strony kanwy i `margin` pikseli od góry. Dwa ostatnie argumenty do `Scale` metody są ustawione na te wartości oraz szerokość i wysokość tekstu, który jest również szerokość i wysokość prostokąt zaokrąglony. Oznacza, że wszystkie skalowanie względem środka tego prostokąta:

[![](scale-images/centeredscale-small.png "Potrójna zrzut ekranu przedstawiający stronę Skala wyśrodkowany")](scale-images/centeredscale-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Skala wyśrodkowana")

`Slider` Elementy w tym programie mają zakres &ndash;10 do 10. Jak widać, ujemne wartości (na przykład w systemie Android ekranu w Centrum) skalowanie w pionie spowodować, że obiekty do przerzucenia wzdłuż osi poziomej, przechodzący przez środek skalowania. Ujemne wartości (takie jak ekran platformy uniwersalnej systemu Windows po prawej stronie) skalowanie w poziomie spowodować, że obiekty do przerzucenia wokół osi pionowej, przechodzący przez środek skalowania.

Ta wersja czwarty `Scale` metodą jest faktycznie skrótu. Warto zobaczyć, jak to działa, zastępując `Scale` metody, w tym kodzie następującym kodem:

```csharp
canvas.Translate(-px, -py);
```

Są to negatywów współrzędnych punktu obrotu.

Teraz ponownie uruchom program. Zobaczysz, że prostokąt i tekst są przesuwane w tak, aby Centrum znajduje się w lewym górnym rogu kanwy. Ledwie mogli je zobaczyć. Suwaki nie działają oczywiście, ponieważ program nie skalować w ogóle.

Teraz Dodaj podstawowe `Scale` wywołań (bez skalowania center) *przed* , `Translate` wywołania:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Jeśli znasz tego ćwiczenia w innych programowania systemów grafiki, może być uważasz, że jest nieprawidłowy, ale nie jest. Skia obsługuje wywołania kolejne przekształcenie nieco inaczej od co można zapoznać się z.

Za pomocą następujących po sobie `Scale` i `Translate` wywołań, środek prostokąt zaokrąglony nadal znajduje się w lewym górnym rogu, ale można ją teraz skalować względem lewego górnego rogu kanwy, która jest również środek prostokąt zaokrąglony.

Teraz, aby ten `Scale` wywołania Dodaj kolejny `Translate` wywołać wyśrodkowywania wartościami:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

To jest przenoszony skalowanych wynik do oryginalnego położenia. Te trzy wywołania są równoważne:

```csharp
canvas.Scale(sx, sy, px, py);
```

Poszczególne transformacje są stwarzane tak, aby formuły całkowita przekształcenia:

 x "= XI sx (x-px) + piks.

 y "= XI sy (y — py) + py

Należy pamiętać, że z wartościami domyślnymi `sx` i `sy` to 1. To proste przekonanie sobie, że punkt obrotu (px, py) nie jest on przekształcany przez te formuły. Pozostaje w tym samym położeniu względem obszaru roboczego.

Łącząc `Translate` i `Scale` wywołań, kolejność ma znaczenie. Jeśli `Translate` przychodzi po `Scale`, współczynniki translacji skutecznie są skalowane przy skalowania czynników. Jeśli `Translate` poprzedzającej `Scale`, czynniki tłumaczenia nie mogą być skalowane. Proces ten staje się nieco bardziej zrozumiały (chociaż bardziej matematyczne) kiedy został wprowadzony przedmiotem macierzy transformacji.

`SKPath` Klasy definiuje tylko do odczytu [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) właściwość, która zwraca `SKRect` definiowanie w zakresie współrzędne w ścieżce. Na przykład, gdy `Bounds` właściwości są uzyskiwane ze ścieżki hendecagram utworzony wcześniej `Left` i `Top` właściwości prostokąta są-około 100 `Right` i `Bottom` właściwości około 100, a `Width` i `Height` właściwości są około 200. (Większość rzeczywistych wartości, które są nieco mniej ponieważ punktów gwiazdek są definiowane przez koła o promieniu 100, ale górnego punktu jest równoległy osi poziomej lub pionowej).

Dostępność tych informacji oznacza, że powinno być możliwe pochodzić skalowania i tłumaczyć czynników umożliwiających skalowanie ścieżki do rozmiaru obszaru roboczego. [ **Anizotropowego skalowanie** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) strona przedstawia to star wskazywany przez 11. *Anizotropowego* skalowania oznacza, że jest nierówne w poziomie i pionie kierunkach, co oznacza, że gwiazdka nie zostaną zachowane jego oryginalny współczynnik proporcji. W tym miejscu jest odpowiedni kod w `PaintSurface` procedury obsługi:

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

`pathBounds` Prostokąt uzyskane w górnej części tego kodu, a następnie używany później przy użyciu szerokości i wysokości kanwy w `Scale` wywołania. Wywołanie przez siebie skalowane współrzędne ścieżkę renderowanych przez `DrawPath` wywołań, ale gwiazdka będzie wyśrodkowana w prawym górnym rogu kanwy. Musi ona przesunięty w dół i w lewo. To zadanie z `Translate` wywołania. Te dwie właściwości `pathBounds` są-około 100, więc czynniki tłumaczenia są około 100. Ponieważ `Translate` wywołanie jest po `Scale` wywołać, te wartości są skalowane skutecznie skalowania czynniki, co ich przenieść środku gwiazdy w środku kanwy:

[![](scale-images/anisotropicscaling-small.png "Potrójna zrzut ekranu przedstawiający stronę skalowanie Anizotropowego")](scale-images/anisotropicscaling-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Anizotropowego skalowania")

Inny sposób należy rozważać `Scale` i `Translate` wywołania ma na celu określenie efekt w odwrotnej kolejności: `Translate` wywołanie przenosi ścieżki, staje się ona w pełni widoczny, ale zorientowana w lewym górnym rogu kanwy. `Scale` Metoda następnie sprawia, że ten star większe względem lewego górnego rogu.

W rzeczywistości wydaje się, że rejestr star to nieco większy niż obszar roboczy. Problem polega na szerokość pociągnięcia. `Bounds` Właściwość `SKPath` wskazuje wymiary współrzędne zakodowane w ścieżce i to, co jest używana do skalowania. Po wyrenderowaniu ścieżki o szerokości określonej obrysu renderowanych ścieżki jest większa niż obszar roboczy.

Aby rozwiązać ten problem, musisz kompensuje który. Jedno z podejść łatwy w tym programie jest dodanie następujących po prawej stronie instrukcji przed `Scale` wywołania:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Zwiększa to `pathBounds` prostokąt 1,5 raza więcej jednostek ze wszystkich czterech stron. Jest to rozsądne rozwiązanie tylko wtedy, gdy jest zaokrąglana sprzężenia pociągnięcia. Sprzężenia skos może być dłuższa i jest trudne do obliczenia.

Można również użyć podobne techniki z tekstem, jako **Anizotropowego tekstu** pokazuje strony. Oto odpowiednia część `PaintSurface` programu obsługi z [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) klasy:

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

Jest podobnej logiki, a tekst jest rozszerzany, aby rozmiar strony oparte na tekst granice prostokątowi zwróconemu z `MeasureText` (czyli nieco większy niż rzeczywisty tekst):

[![](scale-images/anisotropictext-small.png "Potrójna zrzut ekranu przedstawiający stronę testową Anizotropowego")](scale-images/anisotropictext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę testową Anizotropowego")

Jeśli potrzebujesz zachować współczynnik proporcji obiektów graficznych, należy użyć izotropowego skalowania. **Izotropowego skalowanie** strona przedstawia to gwiazdki wskazywany przez 11. Koncepcyjnie kroki do wyświetlania obiekt graficzny w środku strony ze skalowaniem izotropowego są:

- Wykonuje translację elementu roboczego obiektu graficznego do lewego górnego rogu.
- Skalowanie obiektu, w oparciu o co najmniej wymiary strony poziome i pionowe podzielona przez wymiary obiektu graficznego.
- Wykonuje translację elementu środek skalowanego obiektu do środkowej części strony.

[ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) Wykonuje następujące kroki w kolejności odwrotnej względem przed wyświetleniem gwiazdy:

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

Ten kod wyświetla również gwiazdka dziesięć razy, każdym razem, zmniejszając skalowanie wziąć pod uwagę przy 10% i stopniowo zmienia kolor z kolor czerwony, niebieski:

[![](scale-images/isotropicscaling-small.png "Potrójna zrzut ekranu przedstawiający stronę izotropowego skalowanie")](scale-images/isotropicscaling-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę izotropowego skalowania")


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
