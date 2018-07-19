---
title: Ścieżki i tekst w SkiaSharp
description: W tym artykule przedstawiono część wspólną SkiaSharp ścieżki i tekst i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: e7ce6994541ae947fa714d3c67acbc5d5d816975
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130783"
---
# <a name="paths-and-text-in-skiasharp"></a>Ścieżki i tekst w SkiaSharp

_Zapoznaj się z przecięcia ścieżki i tekst_

W systemach nowoczesnych grafiki czcionki tekstu są kolekcjami konturów znak, zazwyczaj zdefiniowane przez drugiego stopnia krzywych Beziera. W związku z tym wiele systemów nowoczesnych grafiki obejmują funkcji, aby skonwertować znaki tekstu do ścieżki grafiki.

Możesz już możliwość przekonania się, można obrysu konturów znaki tekstowe oraz jak je Wypełnij. Dzięki temu można wyświetlić tych opisanych znaków z określonego grubość i nawet efekt ścieżki, zgodnie z opisem w [ **efekty ścieżek** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) artykułu. Ale jest również możliwe do przekonwertowania na ciąg znaków do `SKPath` obiektu. Oznacza to, że obramowanie tekstu mogą być używane dla wycinka przy użyciu technik, które zostały opisane w [ **obcinanie przy użyciu ścieżek i regionów** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md) artykułu.

Oprócz obrysu konspektu znaków przy użyciu efektu ścieżki, można również utworzyć ścieżkę, którą efektów, które są oparte na ścieżkach są uzyskiwane z ciągów znaków, i można nawet łączyć dwa skutki:

![](text-paths-images/pathsandtextsample.png "Efekt ścieżki tekstu")

W [poprzednim artykule](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) pokazaliśmy, jak [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) metody `SKPaint` można uzyskać zarys ścieżki wypełnionej. Ta metoda służy również ze ścieżkami pochodzące z opisanych znaków.

Na koniec, w tym artykule przedstawiono część wspólną innej ścieżki i tekst: [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metody `SKCanvas` umożliwia wyświetlanie ciąg tekstowy, tak aby linii bazowej tekstu następuje ścieżki.

## <a name="text-to-path-conversion"></a>Tekst do konwersji ścieżki

[ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/) Metody `SKPaint` konwertuje ciąg znaków `SKPath` obiektu:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x` i `y` argumenty wskazanie punktu początkowego linii bazowej po lewej stronie tekstu. Grają tę samą rolę tutaj jako w `DrawText` metody `SKCanvas`. W ścieżce linię bazową po lewej stronie tekstu będzie miał współrzędne (x, y).

`GetTextPath` Metoda jest obszerne, jeśli chcesz tylko do wypełnienia lub Ścieżka wynikowa obrysu. Normalną `DrawText` metoda umożliwia to zrobić. `GetTextPath` Metoda jest bardziej użyteczna w przypadku innych zadań z użyciem ścieżki.

Jedną z tych zadań jest przycinania. **Przycinania tekstu** strony tworzy przycinania, oparte na opisanych znak wyrazu "Kod". Ta ścieżka jest rozciągany tak, aby rozmiar strony, kiedy należy przyciąć mapy bitowej, który zawiera obraz **przycinanie tekstu** kod źródłowy:

[![](text-paths-images/clippingtext-small.png "Potrójna zrzut ekranu przedstawiający stronę przycinanie tekstu")](text-paths-images/clippingtext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę przycinanie tekstu")

[ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) Konstruktora klasy ładuje mapę bitową, która jest przechowywana jako zasób osadzony w **Media** folderu rozwiązania:

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
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

`PaintSurface` Obsługi zaczyna się od utworzenia `SKPaint` obiektu odpowiedni tekst. `Typeface` Właściwość jest ustawiona, jak również `TextSize`, mimo że dla tej konkretnej aplikacji `TextSize` właściwość jest wyłącznie arbirtrary. Należy również zauważyć, jest nie `Style` ustawienie.

`TextSize` i `Style` ustawienia właściwości nie są konieczne ponieważ to `SKPaint` obiekt jest używany wyłącznie na potrzeby `GetTextPath` wywołać za pomocą ciągu tekstowego "CODE". Program obsługi mierzy następnie wynikowe `SKPath` obiektu i stosuje przekształcenia trzy wyśrodkuj go i skalować go do rozmiaru strony. Ścieżka może być następnie ustawiona jako ścieżki przycięcia:

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

Po ustawieniu przycinania mapy bitowej mogą być wyświetlane, i zostanie obcięta opisanych znaków. Zwróć uwagę na [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/) metody `SKRect` obliczającej prostokąta do wypełniania strony przy zachowaniu współczynnika proporcji.

**Efekt ścieżki tekstu** strony konwertuje znak pojedynczego handlowe "i" do ścieżki do utworzenia efektu ścieżki 1 D. Obiekt malowania z mocą ta ścieżka jest następnie używany do obrysu zarys większą wersję tego samego znaku:

[![](text-paths-images/textpatheffect-small.png "Potrójna zrzut ekranu przedstawiający stronę efekt ścieżki tekstu")](text-paths-images/textpatheffect-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę efekt ścieżki tekstu")

Większość pracy w [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) klasy odbywa się w polach i konstruktora. Dwa `SKPaint` obiekty zdefiniowane pola są używane do dwóch różnych celów: pierwszy (o nazwie `textPathPaint`) służy do konwertowania handlowe "i" za pomocą `TextSize` 50 do ścieżki dla efektu ścieżki 1 D. Drugi (`textPaint`) służy do wyświetlania większą wersję handlowe "i" począwszy tej ścieżki. Z tego powodu `Style` ten drugi farby obiekt jest ustawiony na `Stroke`, ale `StrokeWidth` właściwość nie jest ustawiona, ponieważ ta właściwość nie jest konieczne, korzystając z mocą ścieżki 1 D:

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
        SKRect textPathPaintBounds = new SKRect();
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

Konstruktor najpierw zastosowano `textPathPaint` obiektu do mierzenia handlowe "i" za pomocą `TextSize` 50. Negatywów współrzędnych Centrum tego prostokąta są następnie przekazywane do `GetTextPath` metody do skonwertowania tekstu do ścieżki. Ścieżka wynikowa ma (0, 0) do punktu w środku znaku, co jest idealnym rozwiązaniem dla efektu ścieżki 1D.

Może się wydawać, że `SKPathEffect` obiekt utworzony na koniec Konstruktor może być równa `PathEffect` właściwość `textPaint` zamiast zapisywane jako pole. Ale to, aby wyłączyć limit nie działa bardzo dobrze, ponieważ jego zakłócona wyniki `MeasureText` wywołania `PaintSurface` obsługi:

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
        SKRect textBounds = new SKRect();
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

Czy `MeasureText` wywołanie jest używane do Centrum znak na stronie. Aby uniknąć problemów, `PathEffect` właściwość jest ustawiona na obiekt programu paint, po tekst został zmierzony, ale przed wyświetleniem.

## <a name="outlines-of-character-outlines"></a>Wskazano konspektów znaków

Zwykle [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) metody `SKPaint` konwertuje jedną ścieżkę do innego za pomocą właściwości programu paint, głównie obrysu szerokości i ścieżki efekt. Gdy jest używana bez efekty ścieżek `GetFillPath` skutecznie tworzą ścieżki, które stanowią zarys inną ścieżkę. To była przedstawiona w **naciśnij, aby konspektu, ścieżka** strony w [ **efekty ścieżek** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) artykułu.

Można również wywołać `GetFillPath` na ścieżce zwróciło `GetTextPath` , ale na początku może nie być całkowitej pewności przebieg wyglądać w następujący sposób.

**Przedstawiono zarys znak** strony pokazano techniki. Odpowiedni kod znajduje się w `PaintSurface` program obsługi [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) klasy.

Konstruktor, który zaczyna się od utworzenia `SKPaint` obiektu o nazwie `textPaint` z `TextSize` właściwości na podstawie rozmiaru strony. To jest konwertowany na ścieżki przy użyciu `GetTextPath` metody. Współrzędna argumenty `GetTextPath` skutecznie Centrum ścieżkę na ekranie:

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
        SKRect textBounds = new SKRect();
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

`PaintSurface` Obsługi utworzy nową ścieżkę o nazwie `outlinePath`. Staje się on ścieżkę docelową, w wywołaniu `GetFillPath`. `StrokeWidth` Właściwość 25 przyczyny `outlinePath` do opisania konspektu ścieżki 25 pikseli na poziomie stykają znaki tekstowe. Ta ścieżka jest następnie wyświetlana w kolorze czerwonym szerokość pociągnięcia, 5:

[![](text-paths-images/characteroutlineoutlines-small.png "Potrójna zrzut ekranu przedstawiający stronę przedstawiono zarys znak")](text-paths-images/characteroutlineoutlines-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę przedstawiono zarys znaków")

Przyjrzyj się bliżej i zostanie wyświetlony nakładania się gdzie konturu ścieżki zapewniają rogu sharp. Są to normalne pozostałości tego procesu.

## <a name="text-along-a-path"></a>Tekst na ścieżce

Tekst jest zwykle wyświetlany na poziomie linii bazowej. Tekst można obracać do uruchomienia po przekątnej czy w pionie, ale linia bazowa jest nadal prostej.

Brak sytuacji, kiedy chcesz tekstu do uruchomienia po łuku. To jest celem [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metody `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

Do uruchomienia na ścieżce określonej jako drugi argument składa się z tekstem określonym w pierwszym argumencie. Możesz rozpocząć tekstowych w miejscach występowania przesunięcie od początku ścieżki z `hOffset` argumentu. Zwykle ścieżka formularzy linii bazowej tekstu: wydłużenia tekst dolnego są na jednej stronie ścieżki i tekst wydłużeń z drugiej strony. Ale można przesunąć linii bazowej tekstu ze ścieżki z `vOffset` argumentu.

Ta metoda ma możliwość zawiera wskazówek na temat ustawień dotyczących `TextSize` właściwość `SKPaint` aby tekst o rozmiarach doskonale do uruchomienia od początku ścieżki na końcu. Czasami można określić ten rozmiar tekstu na własną rękę. Czasami konieczne będzie można opisać w przyszłym artykule za pomocą funkcji pomiaru ścieżki.

**Cykliczne tekstu** program zawija tekst wokół okrąg. To proste ustalenie obwód koła, dzięki czemu można łatwo rozmiar tekstu, aby dokładnie dopasować. `PaintSurface` Program obsługi [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) klasy oblicza promień koła na podstawie rozmiaru strony. Ten okrąg staje się `circularPath`:

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

`TextSize` Właściwość `textPaint` jest następnie dostosować tak, aby szerokość tekstu odpowiada obwód koła:

[![](text-paths-images/circulartext-small.png "Potrójna zrzut ekranu przedstawiający stronę cykliczne tekstu")](text-paths-images/circulartext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę cykliczne tekstu")

Sam tekst został wybrany do nieco cykliczne można również: słowo "koło" jest zarówno podmiotu zdania i obiekt frazy przyimkowych.

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
