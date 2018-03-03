---
title: "Ścieżki i tekst"
description: "Eksploruj przecięcia ścieżek i tekstu"
ms.topic: article
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: bf382f380876e85db46226fb3586382f20d630f2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="paths-and-text"></a>Ścieżki i tekst

_Eksploruj przecięcia ścieżek i tekstu_

W systemach nowoczesnych grafiki czcionki tekstu są kolekcjami elementów zawiera znak, zazwyczaj definiowane przez kwadratową krzywych Beziera. W związku z tym wiele systemów nowoczesnych grafiki obejmują możliwość konwersji znaków w ścieżce grafiki. 

Widzisz już, że można obrysu konturów znaki tekstu oraz jak wypełnić je. Dzięki temu można wyświetlić te zawiera znak z konkretnym grubość i nawet efektu ścieżki, zgodnie z opisem w [ **efekty ścieżki** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) artykułu. Ale jest również możliwe do przekonwertowania ciągu znaków na `SKPath` obiektu. Oznacza to, że obramowanie tekstu mogą być używane dla wycinka techniki, które zostały opisane w [ **wycinka przy użyciu ścieżek i regiony** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md) artykułu. 

Oprócz obrysu konspektu znak za pomocą efektu ścieżki, można również utworzyć ścieżkę, którą efekty, które są oparte na ścieżkach są uzyskiwane z ciągów znaków, i można nawet łączyć dwa efekty:

![](text-paths-images/pathsandtextsample.png "Efekt ścieżki tekstu")

W [poprzednim artykule](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) pokazaliśmy, jak [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) metody `SKPaint` można uzyskać konspekt ścieżki wypełnionej. Ta metoda służy również ze ścieżkami pochodną zawiera znak.

Ponadto w tym artykule przedstawiono innego przecięcia ścieżek i tekst: [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metody `SKCanvas` można wyświetlić ciąg tekstowy, dzięki czemu linii bazowej tekstu następuje ścieżki.

## <a name="text-to-path-conversion"></a>Tekst do konwersji ścieżki

[ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/) Metody `SKPaint` konwertuje ciąg znaków na `SKPath` obiektu:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x` i `y` argumenty wskazują początek linii bazowej z lewej strony tekstu. Odtwarzania tego samego elementu role w tym miejscu jako w `DrawText` metody `SKCanvas`. W ścieżce linia bazowa lewej tekst ma współrzędne (x, y). 

`GetTextPath` — Metoda jest zbyt obszerne, jeśli chcesz tylko do wypełnienia lub obrysu wynikowe ścieżki. Normalnej `DrawText` metoda pozwala to zrobić. `GetTextPath` Metoda jest bardziej użyteczna w przypadku innych zadań dotyczących ścieżki.

Jeden z tych zadań jest wycinka. **Tekst wycinka** strony tworzy ścieżkę wycinka zawiera znak Word 'CODE'. Ta ścieżka jest rozciągany tak, aby rozmiar strony do Przytnij mapę bitową, który zawiera obraz **wycinka tekst** kod źródłowy:

[![](text-paths-images/clippingtext-small.png "Potrójna zrzut ekranu przedstawiający stronę wycinka tekst")](text-paths-images/clippingtext-large.png "Potrójna zrzut ekranu przedstawiający stronę wycinka tekstu")

[ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) Konstruktora klasy ładuje mapę bitową, która jest przechowywana jako osadzony zasób w **nośnika** folderu rozwiązania:

```csharp
public class ClippingTextPage : ContentPage
{
    SKBitmap bitmap;

    public ClippingTextPage()
    {
        Title = "Clipping Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.PageOfCode.png";
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

`PaintSurface` Obsługi rozpoczyna się od utworzenia `SKPaint` odpowiednie dla tekstu obiektu. `Typeface` Właściwość jest ustawiona, jak również `TextSize`, mimo że dla konkretnej aplikacji `TextSize` właściwość jest całkowicie arbirtrary. Ponadto istnieje nie `Style` ustawienie.

`TextSize` i `Style` ustawienia właściwości nie są konieczne ponieważ to `SKPaint` obiekt jest używany wyłącznie do `GetTextPath` wywołanie przy użyciu ciągu tekstowego "Kod". Program obsługi następnie mierzy wynikowe `SKPath` obiektu i stosuje trzy transformacje wyśrodkuj go i skalować ją do rozmiaru strony. Następnie można ustawić ścieżkę jako ścieżka wycinka:

```csharp
public class ClippingTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Blue);

        using (SKPaint paint = new SKPaint())
        {
            paint.Typeface = SKTypeface.FromFamilyName(null, SKTypefaceStyle.Bold);
            paint.TextSize = 10;

            using (SKPath textPath = paint.GetTextPath("CODE", 0, 0))
            {
                // Set transform to center and enlarge clip path to window height
                SKRect bounds;
                textPath.GetTightBounds(out bounds);

                canvas.Translate(info.Width / 2, info.Height / 2);
                canvas.Scale(info.Width / bounds.Width, info.Height / bounds.Height);
                canvas.Translate(-bounds.MidX, -bounds.MidY);

                // Set the clip path
                canvas.ClipPath(textPath);
            }
        }

        // Reset transforms
        canvas.ResetMatrix();

        // Display bitmap to fill window but maintain aspect ratio
        SKRect rect = new SKRect(0, 0, info.Width, info.Height);
        canvas.DrawBitmap(bitmap, 
            rect.AspectFill(new SKSize(bitmap.Width, bitmap.Height)));
    }
}
```

Po ścieżce wycinka jest ustawiona, można wyświetlić mapę bitową i zostanie obcięta zawiera znak. Zwróć uwagę na [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/) metody `SKRect` obliczającej prostokąta do wypełniania strony przy zachowaniu współczynnika proporcji.

**Efekt ścieżki tekstu** strony konwertuje pojedynczy znak ścieżki do utworzenia efektu ścieżki 1 D. Obiekt malowania począwszy ta ścieżka jest następnie używany do obrysu konturu większej wersji tego samego znaku:

[![](text-paths-images/textpatheffect-small.png "Potrójna zrzut ekranu przedstawiający stronę efekt ścieżki tekstu")](text-paths-images/textpatheffect-large.png "Potrójna zrzut ekranu przedstawiający stronę efekt ścieżki tekstu")

Dużo pracy w [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) klasa występuje w polach i konstruktora. Dwa `SKPaint` obiektów zdefiniowana jako pola służą do dwóch różnych celów: pierwszy (o nazwie `textPathPaint`) służy do konwertowania handlowego "i" z `TextSize` 50 do ścieżki dla efektu ścieżki 1 D. Druga (`textPaint`) służy do wyświetlania większej wersji handlowego "i" począwszy tej ścieżki. Z tego powodu `Style` tego paint drugi obiekt został ustawiony na `Stroke`, ale `StrokeWidth` właściwość nie jest ustawiona, ponieważ tej właściwości nie jest konieczne, gdy za pomocą efektu ścieżki 1 D:

```csharp
public class TextPathEffectPage : ContentPage
{
    const string character = "@";
    const float littleSize = 50;

    SKPathEffect pathEffect;

    SKPaint textPathPaint = new SKPaint
    {
        TextSize = littleSize
    };

    SKPaint textPaint = new SKPaint 
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black
    };

    public TextPathEffectPage()
    {
        Title = "Text Path Effect";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Get the bounds of textPathPaint
        SKRect textPathPaintBounds;
        textPathPaint.MeasureText(character, ref textPathPaintBounds);

        // Create textPath centered around (0, 0)
        SKPath textPath = textPathPaint.GetTextPath(character, 
                                                    -textPathPaintBounds.MidX,
                                                    -textPathPaintBounds.MidY);
        // Create the path effect
        pathEffect = SKPathEffect.Create1DPath(textPath, littleSize, 0,
                                               SKPath1DPathEffectStyle.Translate);
    }
    ...
}
```

Konstruktor najpierw używa `textPathPaint` obiektu do mierzenia handlowego "i" z `TextSize` 50. Negatywów współrzędnych center tego prostokąta są następnie przekazywane do `GetTextPath` metodę, aby przekonwertować go do ścieżki. Ścieżka wynikowa ma (0, 0) punkt w Centrum znak, który jest idealny dla efektu ścieżki 1D.

Może być uważasz, że `SKPathEffect` można ustawić obiektu utworzonego na końcu konstruktora `PathEffect` właściwości `textPaint` zamiast zapisane jako pole. Ale ten nie włączenia się bardzo dobrze działa, ponieważ jego zniekształcony wyniki `MeasureText` wywołanie w `PaintSurface` obsługi:

```csharp
public class TextPathEffectPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set textPaint TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Do not measure the text with PathEffect set!
        SKRect textBounds;
        textPaint.MeasureText(character, ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Set the PathEffect property and display text
        textPaint.PathEffect = pathEffect;
        canvas.DrawText(character, xText, yText, textPaint);
    }
}
```

Czy `MeasureText` wywołanie służy do środka znaków na tej stronie. Aby uniknąć problemów, `PathEffect` właściwość jest ustawiona na obiekt paint po tekst został zmierzony, ale przed wyświetleniem.

## <a name="outlines-of-character-outlines"></a>Zawiera znak konspektów

Zwykle [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) metody `SKPaint` konwertuje jedną ścieżkę do innego, stosując właściwości paint głównie obrysu width i ścieżkę efekt. Użyte bez efekty ścieżki `GetFillPath` skutecznie tworzy ścieżki, który zawiera inną ścieżkę. To została przedstawiona w **naciśnij, aby konspektu, ścieżka** strony [ **efekty ścieżki** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) artykułu.

Możesz także wywołać `GetFillPath` w ścieżce zwrócony z `GetTextPath` , ale na początku może nie być całkowicie się, jakie chcesz wyglądu.

**Przedstawiono konspektu znak** strony pokazuje techniki. Odpowiedni kod znajduje się w `PaintSurface` obsługi [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) klasy.

Konstruktor, który rozpoczyna się od utworzenia `SKPaint` obiektu o nazwie `textPaint` z `TextSize` właściwości na podstawie rozmiaru strony. To jest konwertowana na ścieżkę przy użyciu `GetTextPath` metody. Współrzędna argumenty `GetTextPath` skutecznie Centrum ścieżki na ekranie:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint())
    {
        // Set Style for the character outlines
        textPaint.Style = SKPaintStyle.Stroke;

        // Set TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Measure the text
        SKRect textBounds;
        textPaint.MeasureText("@", ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Get the path for the character outlines
        using (SKPath textPath = textPaint.GetTextPath("@", xText, yText))
        {
            // Create a new path for the outlines of the path
            using (SKPath outlinePath = new SKPath())
            {
                // Convert the path to the outlines of the stroked path
                textPaint.StrokeWidth = 25;
                textPaint.GetFillPath(textPath, outlinePath);

                // Stroke that new path
                using (SKPaint outlinePaint = new SKPaint())
                {
                    outlinePaint.Style = SKPaintStyle.Stroke;
                    outlinePaint.StrokeWidth = 5;
                    outlinePaint.Color = SKColors.Red;

                    canvas.DrawPath(outlinePath, outlinePaint);
                }
            }
        }
    }
}
```

`PaintSurface` Obsługi utworzy nową ścieżkę o nazwie `outlinePath`. Staje się on w ścieżce docelowej w wywołaniu `GetFillPath`. `StrokeWidth` Właściwości 25 przyczyny `outlinePath` do opisywania konturu ścieżki 25 pikseli na poziomie używana do malowania znaki tekstu. Ta ścieżka zebrane na czerwono szerokość pociągnięć 5:

[![](text-paths-images/characteroutlineoutlines-small.png "Potrójna zrzut ekranu strony zawiera konspektu znak")](text-paths-images/characteroutlineoutlines-large.png "Potrójna zrzut ekranu strony opisanych konspektu znaków")

Przeglądanie i pojawi się nakładania się gdzie konturu ścieżki sprawia, że sharp rogu. Są to pozostałości normalne tego procesu.

## <a name="text-along-a-path"></a>Tekst na ścieżce

Tekst jest zwykle wyświetlany na poziomie linii bazowej. Tekst można obracać do uruchamiania w pionie lub przekątnej linii bazowej jest jednak nadal prostą.

Brak sytuacji, gdy ma się uruchom krzywej tekst. To jest celem [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metody `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

Tekst określony w pierwszym argumencie dotyczące uruchamiania na ścieżce określony jako drugi argument. Możesz rozpocząć tekstowych w miejscach występowania przesunięcia od początku ścieżki z `hOffset` argumentu. Zwykle ścieżka formularzy linii bazowej tekstu: wydłużenia górne tekst znajdują się na jednej stronie ścieżki, a tekst wydłużeń dolnych z drugiej strony. Jednak można przesunąć linii bazowej tekstu ze ścieżki z `vOffset` argumentu.

Ta metoda ma możliwość wytyczne dotyczące ustawienie `TextSize` właściwość `SKPaint` aby tekst o rozmiarze doskonale do uruchomienia od początku ścieżki na końcu. Czasami może ustalić ten rozmiar tekstu samodzielnie. Innym razem, musisz być opisane w artykule przyszłych za pomocą funkcji pomiaru ścieżki.

**Cykliczne tekst** program zawijania tekstu wokół okręgu. Jest łatwy do określenia obwód koła, dzięki czemu łatwiej rozmiar tekstu, aby dokładnie dopasować. `PaintSurface` Obsługi [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) klasy oblicza Promień okrągłego, zależnie od rozmiaru strony. Staje się tym koło `circularPath`:

```csharp
public class CircularTextPage : ContentPage
{
    const string text = "xt in a circle that shapes the te";
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circularPath = new SKPath())
        {
            float radius = 0.35f * Math.Min(info.Width, info.Height);
            circularPath.AddCircle(info.Width / 2, info.Height / 2, radius);

            using (SKPaint textPaint = new SKPaint())
            {
                textPaint.TextSize = 100;
                float textWidth = textPaint.MeasureText(text);
                textPaint.TextSize *= 2 * 3.14f * radius / textWidth;

                canvas.DrawTextOnPath(text, circularPath, 0, 0, textPaint);
            }
        }
    }
}
```

`TextSize` Właściwość `textPaint` następnie zostanie zmieniona tak, aby szerokość tekstu zgodne obwód koła:

[![](text-paths-images/circulartext-small.png "Potrójna zrzut ekranu przedstawiający stronę cykliczne tekst")](text-paths-images/circulartext-large.png "Potrójna zrzut ekranu przedstawiający stronę cykliczne tekstu")

Tekst został wybrany do nieco cykliczne można również: wyraz "okrąg" jest zarówno podmiotu zdania i obiekt frazę przyimkowych. 

## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
