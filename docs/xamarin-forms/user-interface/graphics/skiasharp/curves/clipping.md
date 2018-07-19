---
title: Obcinanie przy użyciu ścieżek i regionów
description: W tym artykule wyjaśniono, jak używać SkiaSharp ścieżki do klipu grafiki do określonych obszarów, a także tworzenie regionów i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: charlespetzold
ms.author: chape
ms.date: 06/16/2017
ms.openlocfilehash: 52e426c8788ca017f36ba49b338b04a64dc0ef3d
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130820"
---
# <a name="clipping-with-paths-and-regions"></a>Obcinanie przy użyciu ścieżek i regionów

_Użyj ścieżki do klipu grafiki do określonych obszarów, a także tworzenie regionów_

Czasami jest konieczne ograniczanie renderowania grafiki do określonego obszaru. Jest to nazywane *wycinka*. Aby uzyskać efekty specjalne, takie jak ten obraz małp widoczne w usłudze dziurką od klucza, można użyć wycinka:

![](clipping-images/clippingsample.png "Małp za pośrednictwem dziurką od klucza")

*Obszaru przycinania* jest obszar ekranu renderowania grafiki. Wszystko, co jest wyświetlany spoza obszaru przycinania nie jest renderowany. Obszar przycinania zazwyczaj jest definiowany przez [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) obiekt, ale można też zdefiniować obszar przycinania za pomocą [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) obiektu. Te dwa typy obiektów w najpierw wydawać się powiązane ponieważ regionie można utworzyć na podstawie ścieżki. Jednak nie można utworzyć ścieżki z regionu i różnią się one bardzo wewnętrznie: ścieżka zawiera ciąg linii i krzywych, gdy region jest definiowany przez serię linii poziomej skanowania.

Na powyższej ilustracji został utworzony przez **małp za pośrednictwem dziurką od klucza** strony. [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) Klasa definiuje ścieżki SVG danych i używa konstruktora, aby załadować mapy bitowej z zasobów programu:

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
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

Mimo że `keyholePath` obiektu opisuje zarys dziurką od klucza, współrzędne są całkowicie dowolnego i odzwierciedlają, jaki był wygodne, gdy dane ścieżki została opracowana. Z tego powodu `PaintSurface` obsługi uzyskuje granicami tej ścieżce i wywołania `Translate` i `Scale` przenieść ścieżkę na środku ekranu oraz zapewnienie niemal przedtem jako ekran:


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

Ale ścieżka nie jest renderowany. Zamiast tego należy następujące przekształcenia, ścieżka jest używana do Ustaw obszar przycinania, za pomocą tej instrukcji:

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface` Obsługi następnie resetuje przekształcenia przy użyciu wywołania do `ResetMatrix` i rysuje mapy bitowej rozszerzenie pełnej wysokości ekranu. Ten kod zakłada, że mapa bitowa jest kwadratowy, czyli tej określonej mapy bitowej. Mapa bitowa jest renderowany tylko w obszarze zdefiniowanym przez ścieżki przycinania:

[![](clipping-images/monkeythroughkeyhole-small.png "Potrójna zrzut ekranu przedstawiający małp za pomocą strony dziurką od klucza")](clipping-images/monkeythroughkeyhole-large.png#lightbox "Potrójna zrzut ekranu przedstawiający małp za pomocą strony dziurką od klucza")

Ścieżki przycięcia podlega przekształcenia obowiązuje podczas `ClipPath` metoda jest wywoływana, a nie do przekształcenia w efekcie gdy obiekt graficzny (takie jak mapy bitowej) zostanie wyświetlona. Ścieżki przycięcia jest częścią stan obszaru roboczego, który jest zapisywany z `Save` metody i przywrócenie z `Restore` metody.

## <a name="combining-clipping-paths"></a>Łącząc ścieżki przycinania

Ściśle rzecz ujmując, w obszarze wycinka nie "sprawdzeniami" `ClipPath` metody. Zamiast tego jest połączona z istniejącą ścieżkę przycinania rozpoczyna się jako prostokąt równym do ekranu. Możesz uzyskać prostokątny granice przy użyciu obszaru przycinania [ `ClipBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipBounds/) właściwości lub [ `ClipDeviceBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipDeviceBounds/) właściwości. `ClipBounds` Właściwość zwraca `SKRect` wartość, która odzwierciedla dowolnego przekształcenia, które mogą obowiązywać. `ClipDeviceBounds` Właściwość zwraca `RectI` wartość. To jest prostokąta o wymiarach liczby całkowitej i opisuje obszaru przycinania w rzeczywistych wymiarów.

Dowolne wywołanie `ClipPath` zmniejsza obszar przycinania, łącząc obszar przycinania, za pomocą nowego obszaru. Pełna składnia [ `ClipPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipPath/p/SkiaSharp.SKPath/SkiaSharp.SKClipOperation/System.Boolean/) metodą jest:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Istnieje również [ `ClipRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRect/p/SkiaSharp.SKRect/SkiaSharp.SKClipOperation/System.Boolean/) metodę, która łączy obszar przycinania z prostokątem:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Domyślnie obszar przycinania wynikowe to część wspólna istniejącego obszaru przycinania i `SKPath` lub `SKRect` określonym w `ClipPath` lub `ClipRect` metody. Jest to zaprezentowane w **klipu Intersect okręgów cztery** strony. `PaintSurface` Obsługi w [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) klasy ponownie używa takie same `SKPath` obiektu do utworzenia czterech nakładających się okręgów, z których każdy zmniejsza obszar przycinania przez kolejne wywołania `ClipPath`:

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

Zachowasz pozostałe jest część wspólną tych kręgów cztery:

[![](clipping-images//fourcircleintersectclip-small.png "Potrójna zrzut ekranu przedstawiający stronę cztery okrąg Intersect klipu")](clipping-images/fourcircleintersectclip-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę cztery okrąg Intersect klipu")

[ `SKClipOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKClipOperation/) Wyliczenie ma tylko dwa elementy członkowskie:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Difference/) Usuwa określoną ścieżkę lub prostokąt z istniejącego obszaru przycinania

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Intersect/) przecina określonej ścieżki lub prostokąt z istniejącego obszaru przycinania

Jeżeli wymienisz cztery `SKClipOperation.Intersect` argumentów `FourCircleIntersectClipPage` klasy `SKClipOperation.Difference`, zostaną wyświetlone następujące czynności:

[![](clipping-images//fourcircledifferenceclip-small.png "Potrójna zrzut ekranu strony klipu Intersect okrąg cztery za pomocą operacji różnicowej")](clipping-images/fourcircledifferenceclip-large.png#lightbox "Potrójna zrzut ekranu strony klipu Intersect okrąg cztery za pomocą operacji różnicowej")

Czterech nakładających się okręgów zostały usunięte z obszaru przycinania.

**Operacje klipu** strony ilustruje różnicę między te dwie operacje przy użyciu tylko pary koła. Koło po lewej stronie zostanie dodany do obszaru przycinania za pomocą domyślnego działania klipu `Intersect`, podczas gdy drugi okrąg po prawej stronie zostanie dodany do obszaru przycinania z operacją klipu wskazywanym przez tekst etykiety:

[![](clipping-images//clipoperations-small.png "Potrójna zrzut ekranu przedstawiający stronę operacje klipu")](clipping-images/clipoperations-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę operacje klipu")

[ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) Klasy definiuje dwa `SKPaint` obiektów jako pola, a następnie dzieli ekranu na dwa obszary prostokątny. Te obszary są różne w zależności od tego, czy numer telefonu jest w trybie pionowa lub pozioma. `DisplayClipOp` Klasy następnie wyświetla tekst i wywołania `ClipPath` ze ścieżkami dwóch okrąg do zilustrowania każdej operacji klipu:

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

Wywoływanie `DrawPaint` zwykle powoduje, że cały obszar roboczy, trzeba napełniać, `SKPaint` obiektu, ale w tym przypadku metoda po prostu umożliwia malowanie wewnątrz obszaru przycinania.

## <a name="exploring-regions"></a>Poznawanie regionów

Jeśli zostały przedstawione w dokumentacji interfejsu API `SKCanvas`, być może Zauważyłeś, przeciążenia `ClipPath` i `ClipRect` metod, które są podobne do metod opisanych powyżej, ale zamiast tego ma parametr o nazwie [ `SKRegionOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegionOperation/) zamiast `SKClipOperation`. `SKRegionOperation` ma sześć elementy członkowskie, co zapewnia nieco większą elastyczność podczas łączenia ścieżek do obszarów wycinka formularza:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Difference/)

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Intersect/)

- [`Union`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Union/)

- [`XOR`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.XOR/)

- [`ReverseDifference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.ReverseDifference/)

- [`Replace`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Replace/)

Jednak przeciążeń `ClipPath` i `ClipRect` z `SKRegionOperation` parametry są przestarzałe i nie można ich używać.

Można nadal używać `SKRegionOperation` wyliczenie, ale wymaga zdefiniowania obszar przycinania na podstawie [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) obiektu.

Nowo utworzone `SKRegion` obiektu opisuje pusty obszar. Pierwsze wywołanie do obiektu jest zazwyczaj [ `SetRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetRect/p/SkiaSharp.SKRectI/) tak, aby region opisują prostokątny obszar. Parametr `SetRect` jest `SKRectI` wartość &mdash; prostokąt wartością właściwości Liczba całkowita. Następnie możesz wywołać [ `SetPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetPath/p/SkiaSharp.SKPath/SkiaSharp.SKRegion/) z `SKPath` obiektu. Spowoduje to utworzenie region, który jest taki sam jak wewnątrz ścieżki, ale przycinane do początkowego prostokątny obszar.

`SKRegionOperation` Wyliczenie pochodzi wyłącznie do gry podczas wywoływania jednego z [ `Op` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.Op/p/SkiaSharp.SKRegion/SkiaSharp.SKRegionOperation/) przeciążenia metody, taką jak ta:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

Regionie, w którym wprowadzasz `Op` wywołanie jest połączony z regionu, określony jako parametr w oparciu o `SKRegionOperation` elementu członkowskiego. Podczas ostatecznie uzyskasz region odpowiedni dla wycinka, można ustawić, jako obszar przycinania za pomocą kanwy [ `ClipRegion` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRegion/p/SkiaSharp.SKRegion/SkiaSharp.SKClipOperation/) metody `SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

Poniższy zrzut ekranu przedstawia wycinka obszarów, na podstawie operacji sześciu regionów. Po lewej stronie koła to region, który `Op` metoda jest wywoływana w i odpowiednie okrąg jest to region, przekazywane do `Op` metody:

[![](clipping-images//regionoperations-small.png "Potrójna zrzut ekranu przedstawiający stronę operacje Region")](clipping-images/regionoperations-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę operacje Region")

Czy te wszystkie możliwości łączenia tych dwóch kręgów? Należy wziąć pod uwagę obraz wynikowy pod postacią połączenia trzy składniki, które same w sobie są widoczne w `Difference`, `Intersect`, i `ReverseDifference` operacji. Całkowita liczba kombinacji jest dwa do potęgi trzeciej lub 8. Dwa, na których brakuje są oryginalne region (co powoduje wywołanie nie `Op` w ogóle) i całkowicie pusty regionu.

Jest trudniejsze do użytku regionów przycinania, ponieważ trzeba najpierw utworzyć ścieżkę, a następnie region z tej ścieżki, a następnie łączyć wiele regionów. Ogólną strukturę **operacje Region** strony jest bardzo podobny do **operacje klipu** ale [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) klasy dzieli ekranu na sześć obszarów i Pokazuje dodatkowej pracy, które są wymagane do użycia regiony dla tego zadania:

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

W tym miejscu jest różnica między `ClipPath` metody i `ClipRegion` metody:

> [!IMPORTANT]
> W odróżnieniu od `ClipPath` metody `ClipRegion` metoda nie ma wpływu przekształcenia.

Zrozumienie uzasadnienie tę różnicę, to zrozumieć, jakie regionie. Jeśli już wiesz, jak operacji klipu ani operacji regionu może być implementowane wewnętrznie, prawdopodobnie wydaje się bardzo skomplikowane. Kilka bardzo złożonych ścieżek są łączone i kontur Ścieżka wynikowa jest prawdopodobnie okropnej konsolidatorze.

Ale to zadanie jest znacznie uproszczone w przypadku poszczególnych ścieżek na serię linii poziomej skanowania, takich jak stara rury odkurzający telewizorów. Każdy wiersz skanowania jest po prostu linii poziomej punkt początkowy i punkt końcowy. Na przykład koła o promieniu 10 może być rozłożone na 20 linii poziomej skanowania, z których każdy rozpoczyna się w lewej części koła i kończy się w prawej części. Łącząc dwa okręgi z każdej operacji region staje się bardzo prosty ponieważ jest on po prostu kwestią badanie rozpoczęcia i zakończenia współrzędne każdej pary odpowiednie wiersze skanowania.

Co to jest region: serię linii poziomej skanowania, która definiuje obszar.

Jednak gdy obszar jest ograniczona do szeregu skanowania linii, te skanowania, które wiersze są oparte na wymiarze piksela. Ściśle rzecz ujmując region nie jest obiektem grafiki wektorowej. Jest z natury skompresowany monochromatyczną mapę bitową niż do ścieżki. W związku z tym regionów nie, i bez utraty jakości i z tego powodu nie są przekształcane stosowania obszarów wycinka.

Jednakże można zastosować przekształcenia do regionów do celów rysowania. **Region Paint** program wyraźnie pokazuje wewnętrzny charakter regionów. [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) Tworzy klasę `SKRegion` na podstawie obiektu `SKPath` okręgu 10 jednostek usługi radius. Przekształcenie następnie rozwija ten okrąg do wypełnienia strony:

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

`DrawRegion` Wywołanie wypełnia regionu w kolorze pomarańczowym, gdy `DrawPath` wywołanie obrysy oryginalnej ścieżce w kolorze niebieskim dla porównania:

[![](clipping-images//regionpaint-small.png "Potrójna zrzut ekranu przedstawiający stronę Region Paint")](clipping-images/regionpaint-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Paint regionu")

Region jest wyraźnie szereg dyskretnych współrzędnych.

Nie ma potrzeby używanie przekształceń w połączeniu z obszary wycinka, mogą używać regionów dla wycinka, jak **czterech — liścia koniczyna** pokazuje strony. [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) Klasy tworzy złożonego region z czterech regionów cykliczne, ustawia ten region złożonego jako obszar przycinania i następnie rysuje szereg 360 proste linie pochodzących od środka strony:

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

Tak naprawdę nie wygląda koniczyna czterech — typu liść, ale jest obraz, który może być trudny do renderowania bez przycinania:

[![](clipping-images//fourleafclover-small.png "Potrójna zrzut ekranu przedstawiający stronę koniczyna typu liść — czterech")](clipping-images/fourleafclover-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę koniczyna typu liść — czterech")


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
