---
title: "Wycinka przy użyciu ścieżek i regiony"
description: "Używanie ścieżek do obiektów graficznych do określonych obszarów, a także tworzenie regionów"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: charlespetzold
ms.author: chape
ms.date: 06/16/2017
ms.openlocfilehash: e84bce5d4280ded801ed58999a2570d3c6bd327e
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="clipping-with-paths-and-regions"></a>Wycinka przy użyciu ścieżek i regiony

_Używanie ścieżek do obiektów graficznych do określonych obszarów, a także tworzenie regionów_

Czasami jest konieczne ograniczanie Renderowanie grafiki do określonego obszaru. Jest to nazywane *wycinka*. Wycinka służącego do efektów specjalnych, takich jak ten obraz małp, poprzez dziurką od klucza:

![](clipping-images/clippingsample.png "Małp za pośrednictwem dziurką od klucza")

*Obszar przycinania* jest obszar ekranu renderowania grafiki. Wszystko, co jest wyświetlane poza obszarem wycinka nie jest renderowany. Obszar przycinania są zazwyczaj zdefiniowane przez [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) obiekt, ale można również zdefiniować obszar przycinania za pomocą [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) obiektu. Te dwa typy obiektów w najpierw prawdopodobnie pokrewne ponieważ regionie można utworzyć na podstawie ścieżki. Jednak nie można utworzyć ścieżki z regionu i są bardzo różnych wewnętrznie: ścieżka zawiera ciąg linii i krzywych, gdy region jest definiowana za pomocą serii linii poziomych skanowania.

Powyższy obraz został utworzony przez **małp za pośrednictwem dziurką od klucza** strony. [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) Klasy ścieżki przy użyciu danych pliku SVG definiuje i używa konstruktora załadować mapy bitowej z zasobów programu:

```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    SKBitmap bitmap;
    SKPath keyholePath = SKPath.ParseSvgPathData(
        "M 300 130 L 250 350 L 450 350 L 400 130 A 70 70 0 1 0 300 130 Z");

    public MonkeyThroughKeyholePage()
    {
        Title = "Monkey through Keyhole";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

Mimo że `keyholePath` obiektu opisuje konturu dziurką od klucza, współrzędne są całkowicie dowolnego i uwzględniać, jaka była wygodny, gdy dane ścieżki została opracowana. Z tego powodu `PaintSurface` obsługi uzyskuje granice tego ścieżką i wywołania `Translate` i `Scale` można przenieść ścieżki do Centrum ekranu, a także niemal tak wysokie jako ekran:


```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transform to center and enlarge clip path to window height
        SKRect bounds;
        keyholePath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.98f * info.Height / bounds.Height);
        canvas.Translate(-bounds.MidX, -bounds.MidY);

        // Set the clip path
        canvas.ClipPath(keyholePath);

        // Reset transforms
        canvas.ResetMatrix();

        // Display monkey to fill height of window but maintain aspect ratio
        canvas.DrawBitmap(bitmap,
            new SKRect((info.Width - info.Height) / 2, 0,
                       (info.Width + info.Height) / 2, info.Height));
    }
}
```

Ale ścieżka nie jest renderowany. Zamiast tego następujące przekształcenia, ścieżka jest używana do ustawiania obszar przycinania z tej instrukcji:

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface` Obsługi następnie resetuje przekształcenia wywołaniem `ResetMatrix` i rysuje mapy bitowej rozszerzenie wysokości pełnego ekranu. Ten kod zakłada, że mapy bitowej kwadratowych, czyli tej konkretnej mapy bitowej. Mapy bitowej jest renderowany tylko w obszarze zdefiniowanym przez ścieżki przycinania:

[![](clipping-images/monkeythroughkeyhole-small.png "Potrójna zrzut ekranu przedstawiający małp za pośrednictwem strony dziurką od klucza")](clipping-images/monkeythroughkeyhole-large.png#lightbox "Potrójna zrzut ekranu przedstawiający małp za pośrednictwem strony dziurką od klucza")

Ścieżka wycinka podlega przekształcenia obowiązywać po `ClipPath` metoda jest wywoływana, a nie do przekształcenia w mocy po obiektu graficznego (na przykład mapy bitowej) są wyświetlane. Ścieżka wycinka jest częścią stanie roboczym, który zostanie zapisany z `Save` — metoda i przywrócenie historii z `Restore` metody.

## <a name="combining-clipping-paths"></a>Łączenie ścieżki przycinania

Mówiąc ściślej, obszar przycinania nie "ustawieniem" `ClipPath` metody. Zamiast tego jest połączony z istniejącą ścieżką wycinka rozpoczyna się jako prostokąt o rozmiarze równa ekranu. Można uzyskać za pomocą obszaru przycinania prostokątne granice [ `ClipBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipBounds/) właściwości lub [ `ClipDeviceBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipDeviceBounds/) właściwości. `ClipBounds` Zwraca `SKRect` wartość, która odzwierciedla żadnego przekształca, które mogą obowiązywać. `ClipDeviceBounds` Zwraca `RectI` wartość. Jest to prostokąt z liczbą całkowitą wymiarów i opisano obszar przycinania w rzeczywistych wymiarów.

Wszelkie wywołanie `ClipPath` zmniejsza obszar przycinania łącząc obszar przycinania z nowego obszaru. Pełna składnia [ `ClipPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipPath/p/SkiaSharp.SKPath/SkiaSharp.SKClipOperation/System.Boolean/) metoda jest:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Istnieje również [ `ClipRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRect/p/SkiaSharp.SKRect/SkiaSharp.SKClipOperation/System.Boolean/) metodę, która łączy obszaru przycinania z prostokąta:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Domyślnie obszar wynikowe wycinka jest przecięcie istniejącego obszaru przycinania i `SKPath` lub `SKRect` określonym w `ClipPath` lub `ClipRect` metody. To jest przedstawiona w **cztery okręgi Intersect klip** strony. `PaintSurface` Obsługi w [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) klasy ponownie używa takie same `SKPath` obiekt, aby utworzyć cztery nakładające się okręgi, z których każdy zmniejsza obszar przycinania za pośrednictwem kolejnych wywołań `ClipPath`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float size = Math.Min(info.Width, info.Height);
    float radius = 0.4f * size;
    float offset = size / 2 - radius;

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    using (SKPath path = new SKPath())
    {
        path.AddCircle(-offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(-offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColors.Blue;
            canvas.DrawPaint(paint);
        }
    }
}
```

Pozostała jest przecięcie te cztery koła:

[![](clipping-images//fourcircleintersectclip-small.png "Potrójna zrzut ekranu strony klip Intersect koło cztery")](clipping-images/fourcircleintersectclip-large.png#lightbox "Potrójna zrzut ekranu strony klip cztery Intersect koło")

[ `SKClipOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKClipOperation/) Wyliczenie ma tylko dwa elementy członkowskie:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Difference/) Usuwa określoną ścieżkę lub prostokąt z istniejącego obszaru przycinania

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Intersect/) przecina określonej ścieżki lub prostokąt z istniejącego obszaru przycinania

Jeśli musisz zastąpić cztery `SKClipOperation.Intersect` argumentów `FourCircleIntersectClipPage` klasy z `SKClipOperation.Difference`, pojawi się następujące:

[![](clipping-images//fourcircledifferenceclip-small.png "Potrójna zrzut ekranu przedstawiający stronę klip cztery Intersect koło z operacji różnicowej")](clipping-images/fourcircledifferenceclip-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę klip cztery Intersect koło z operacji różnicowej")

Cztery nakładające się okręgi zostały usunięte z obszaru przycinania.

**Operacji klip** strony pokazano różnicę między tych dwóch operacji o po prostu pary okręgi. Koło po lewej stronie zostanie dodany do obszaru przycinania z domyślnego działania klip `Intersect`, podczas gdy koła po prawej stronie dodawanej do obszaru przycinania z operacją przycinania oznaczonego etykietą:

[![](clipping-images//clipoperations-small.png "Potrójna zrzut ekranu strony Operacje klip")](clipping-images/clipoperations-large.png#lightbox "Potrójna zrzut ekranu strony Operacje Clip")

[ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) Klasa definiuje dwie `SKPaint` obiekty jako pola, a następnie dzieli ekranu na dwa obszary prostokątne. Te obszary są różne w zależności od tego, czy telefon jest w trybie pionowa lub pozioma. `DisplayClipOp` Klasy następnie wyświetla tekst i wywołania `ClipPath` ze ścieżkami dwóch koło w celu zilustrowania każdej operacji klip:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;

    foreach (SKClipOperation clipOp in Enum.GetValues(typeof(SKClipOperation)))
    {
        // Portrait mode
        if (info.Height > info.Width)
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width, y + info.Height / 2), clipOp);
            y += info.Height / 2;
        }
        // Landscape mode
        else
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width / 2, y + info.Height), clipOp);
            x += info.Width / 2;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKClipOperation clipOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(clipOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    canvas.Save();

    using (SKPath path1 = new SKPath())
    {
        path1.AddCircle(xCenter - radius / 2, yCenter, radius);
        canvas.ClipPath(path1);

        using (SKPath path2 = new SKPath())
        {
            path2.AddCircle(xCenter + radius / 2, yCenter, radius);
            canvas.ClipPath(path2, clipOp);

            canvas.DrawPaint(fillPaint);
        }
    }

    canvas.Restore();
}
```

Wywoływanie `DrawPaint` zwykle powoduje, że cały obszar roboczy do wypełnienia z tym `SKPaint` obiektu, ale w takim przypadku metoda po prostu umożliwia malowanie wewnątrz obszaru przycinania.

## <a name="exploring-regions"></a>Eksploracja regionów

Jeśli zostały przedstawione w dokumentacji interfejsu API `SKCanvas`, można zauważyć przeciążeń `ClipPath` i `ClipRect` metod, które są podobne do metod opisanych powyżej, ale zamiast tego ma parametr o nazwie [ `SKRegionOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegionOperation/) zamiast `SKClipOperation`. `SKRegionOperation` ma sześć członków nieco większą elastyczność podczas łączenia ścieżek do formularza wycinka obszarów:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Difference/)

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Intersect/)

- [`Union`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Union/)

- [`XOR`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.XOR/)

- [`ReverseDifference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.ReverseDifference/)

- [`Replace`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Replace/)

Jednak przeciążeń `ClipPath` i `ClipRect` z `SKRegionOperation` parametry są przestarzałe i nie można ich używać.

Można nadal używać `SKRegionOperation` wyliczenia, ale wymaga zdefiniowania obszar przycinania na podstawie [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) obiektu.

Nowo utworzone `SKRegion` obiektu opisuje pustym obszarem. Zazwyczaj jest pierwsze wywołanie w obiekcie [ `SetRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetRect/p/SkiaSharp.SKRectI/) tak, aby obszar opisano prostokątny obszar. Parametr `SetRect` jest `SKRectI` wartość &mdash; wartość prostokąt z właściwościami liczby całkowitej. Następnie można wywołać [ `SetPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetPath/p/SkiaSharp.SKPath/SkiaSharp.SKRegion/) z `SKPath` obiektu. Spowoduje to utworzenie region, który jest taki sam jak wewnątrz ścieżki, ale przycinane do początkowej prostokątny obszar.

`SKRegionOperation` Wyliczenie pochodzi wyłącznie do gry podczas wywoływania jednej z [ `Op` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.Op/p/SkiaSharp.SKRegion/SkiaSharp.SKRegionOperation/) przeciążenia metody, taką jak:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

Obszar wprowadzasz `Op` wywołanie jest połączona z określonego jako parametru na podstawie regionu `SKRegionOperation` elementu członkowskiego. Na koniec otrzymasz region odpowiednie dla wycinka, można ustawić który jako obszar przycinania za pomocą obszaru roboczego [ `ClipRegion` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRegion/p/SkiaSharp.SKRegion/SkiaSharp.SKClipOperation/) metody `SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

Poniższy zrzut ekranu przedstawia na podstawie operacji region sześć obszarów wycinka. Po lewej stronie okrąg ma wartość region który `Op` wywoływana jest metoda, a prawa okrąg region przekazany do `Op` metody:

[![](clipping-images//regionoperations-small.png "Potrójna zrzut ekranu strony Operacje Region")](clipping-images/regionoperations-large.png#lightbox "Potrójna zrzut ekranu strony Operacje regionu")

Czy te wszystkie możliwości łączenia tych dwóch okręgi? Należy wziąć pod uwagę Wynikowy obraz jako kombinację trzech składników, które same są widoczne w `Difference`, `Intersect`, i `ReverseDifference` operacji. Całkowita liczba kombinacji jest trzeci zasilania, co najmniej dwóch osiem. Dwa, które nie są spełnione są regionu oryginalnego (którego wynikiem wywołania nie `Op` w ogóle) i całkowicie pusty region.

Jest trudniej regionów na potrzeby wycinka, ponieważ trzeba najpierw utworzyć ścieżkę, a następnie region z tej ścieżki, a następnie połączenie wielu regionach. Ogólna struktura **operacji Region** strony jest bardzo podobny do **operacji klip** , ale [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) klasy dzieli ekranu na sześć obszarów i przedstawia dodatkowej pracy wymagane do używania regiony dla tego zadania:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;
    float width = info.Height > info.Width ? info.Width / 2 : info.Width / 3;
    float height = info.Height > info.Width ? info.Height / 3 : info.Height / 2;

    foreach (SKRegionOperation regionOp in Enum.GetValues(typeof(SKRegionOperation)))
    {
        DisplayClipOp(canvas, new SKRect(x, y, x + width, y + height), regionOp);

        if ((x += width) >= info.Width)
        {
            x = 0;
            y += height;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKRegionOperation regionOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(regionOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    SKRectI recti = new SKRectI((int)rect.Left, (int)rect.Top,
                                (int)rect.Right, (int)rect.Bottom);

    using (SKRegion wholeRectRegion = new SKRegion())
    {
        wholeRectRegion.SetRect(recti);

        using (SKRegion region1 = new SKRegion(wholeRectRegion))
        using (SKRegion region2 = new SKRegion(wholeRectRegion))
        {
            using (SKPath path1 = new SKPath())
            {
                path1.AddCircle(xCenter - radius / 2, yCenter, radius);
                region1.SetPath(path1);
            }

            using (SKPath path2 = new SKPath())
            {
                path2.AddCircle(xCenter + radius / 2, yCenter, radius);
                region2.SetPath(path2);
            }

            region1.Op(region2, regionOp);

            canvas.Save();
            canvas.ClipRegion(region1);
            canvas.DrawPaint(fillPaint);
            canvas.Restore();
        }
    }
}
```

Oto istotną różnicą między `ClipPath` — metoda i `ClipRegion` metody:

> [!IMPORTANT]
> W odróżnieniu od `ClipPath` metody `ClipRegion` — metoda nie ma wpływu na transformacji.

Zrozumienie uzasadnienie tę różnicę, jest zrozumieć, jakie region jest. Jeśli już wiesz, jak klip operacje lub operacje regionie można zaimplementować wewnętrznie, prawdopodobnie wydaje się bardzo skomplikowane. Wiele ścieżek bardzo złożonych są łączone i konturu Ścieżka wynikowa prawdopodobnie algorytmicznego nightmare.

Jednak tego zadania jest znacznie uproszczone w przypadku poszczególnych ścieżek do serii linii poziomych skanowania, takich jak stosowane probówki próżni telewizorach. Każdy wiersz skanowania jest po prostu linii poziomej punkt początkowy i punkt końcowy. Na przykład koło z protokołem radius 10 może być rozłożone na 20 skanowania poziome linie, z których każdy rozpoczyna się w lewej części okręgu i kończy się w prawej części. Łączenie dwa okręgi z żadnej operacji region staje się bardzo proste, ponieważ jest po prostu kwestią badanie początkową i końcową współrzędne każda para odpowiednie wiersze skanowania.

Co to jest obszarem: serii linii poziomych skanowania, która definiuje obszar.

Jednak gdy obszar zostanie zmniejszona do serii skanowanie linii, te skanowania wierszy na podstawie których wymiaru piksela. Mówiąc ściślej region nie jest obiektem grafiki wektorowej. Jest charakteru bliżej skompresowany monochromatyczny mapę bitową niż do ścieżki. W rezultacie regionów nie, i bez utraty wierności i z tego powodu nie są przekształcony użycie obszarów wycinka.

Jednak można stosować przekształceń do regionów dla celów malowania. **Region Paint** program żywe ilustruje wewnętrzny rodzaj regionów. [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) Tworzy klasy `SKRegion` na podstawie obiektu `SKPath` koła radius 10 jednostki. Następnie transformacji rozwija tego okręgu do wypełnienia strony:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int radius = 10;

    // Create circular path
    using (SKPath circlePath = new SKPath())
    {
        circlePath.AddCircle(0, 0, radius);

        // Create circular region
        using (SKRegion circleRegion = new SKRegion())
        {
            circleRegion.SetRect(new SKRectI(-radius, -radius, radius, radius));
            circleRegion.SetPath(circlePath);

            // Set transform to move it to center and scale up
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(Math.Min(info.Width / 2, info.Height / 2) / radius);

            // Fill region
            using (SKPaint fillPaint = new SKPaint())
            {
                fillPaint.Style = SKPaintStyle.Fill;
                fillPaint.Color = SKColors.Orange;

                canvas.DrawRegion(circleRegion, fillPaint);
            }

            // Stroke path for comparison
            using (SKPaint strokePaint = new SKPaint())
            {
                strokePaint.Style = SKPaintStyle.Stroke;
                strokePaint.Color = SKColors.Blue;
                strokePaint.StrokeWidth = 0.1f;

                canvas.DrawPath(circlePath, strokePaint);
            }
        }
    }
}
```

`DrawRegion` Wywołania wypełnia regionu w kolorze pomarańczowym, gdy `DrawPath` wywołania obrysy oryginalnej ścieżki na niebiesko do porównania:

[![](clipping-images//regionpaint-small.png "Potrójna zrzut ekranu przedstawiający stronę Region Paint")](clipping-images/regionpaint-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Paint regionu")

Region jest wyraźnie szereg odrębny współrzędnych.

Jeśli nie trzeba transformacje używane na obszary wycinka, możesz użyć regionów dla wycinka, jako **czterech — liścia koniczyna** pokazuje stronę. [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) Klasy tworzy złożonego region z czterech cykliczną regionami, ustawia tego regionu złożonego jako obszar przycinania i następnie rysuje szereg 360 proste pochodzących od środka strony:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float xCenter = info.Width / 2;
    float yCenter = info.Height / 2;
    float radius = 0.24f * Math.Min(info.Width, info.Height);

    using (SKRegion wholeScreenRegion = new SKRegion())
    {
        wholeScreenRegion.SetRect(new SKRectI(0, 0, info.Width, info.Height));

        using (SKRegion leftRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion rightRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion topRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion bottomRegion = new SKRegion(wholeScreenRegion))
        {
            using (SKPath circlePath = new SKPath())
            {
                // Make basic circle path
                circlePath.AddCircle(xCenter, yCenter, radius);

                // Left leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, 0));
                leftRegion.SetPath(circlePath);

                // Right leaf
                circlePath.Transform(SKMatrix.MakeTranslation(2 * radius, 0));
                rightRegion.SetPath(circlePath);

                // Make union of right with left
                leftRegion.Op(rightRegion, SKRegionOperation.Union);

                // Top leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, -radius));
                topRegion.SetPath(circlePath);

                // Combine with bottom leaf
                circlePath.Transform(SKMatrix.MakeTranslation(0, 2 * radius));
                bottomRegion.SetPath(circlePath);

                // Make union of top with bottom
                bottomRegion.Op(topRegion, SKRegionOperation.Union);

                // Exclusive-OR left and right with top and bottom
                leftRegion.Op(bottomRegion, SKRegionOperation.XOR);

                // Set that as clip region
                canvas.ClipRegion(leftRegion);

                // Set transform for drawing lines from center
                canvas.Translate(xCenter, yCenter);

                // Draw 360 lines
                for (double angle = 0; angle < 360; angle++)
                {
                    float x = 2 * radius * (float)Math.Cos(Math.PI * angle / 180);
                    float y = 2 * radius * (float)Math.Sin(Math.PI * angle / 180);

                    using (SKPaint strokePaint = new SKPaint())
                    {
                        strokePaint.Color = SKColors.Green;
                        strokePaint.StrokeWidth = 2;

                        canvas.DrawLine(0, 0, x, y, strokePaint);
                    }
                }
            }
        }
    }
}
```

Naprawdę nie wygląda jak czterech — liścia koniczyny, ale jest obraz, który może być trudne do renderowania bez wycinka:

[![](clipping-images//fourleafclover-small.png "Potrójna zrzut ekranu strony czterech — liścia koniczyna")](clipping-images/fourleafclover-large.png#lightbox "Potrójna zrzut ekranu strony czterech — liść koniczyny")


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
