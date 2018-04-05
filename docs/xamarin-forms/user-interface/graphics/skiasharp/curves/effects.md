---
title: Efekty ścieżki
description: Odnajdywanie różnego rodzaju zezwalających na ścieżki do zastosowania w przypadku używana do malowania i wypełnianie efektów ścieżki
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: charlespetzold
ms.author: chape
ms.date: 07/29/2017
ms.openlocfilehash: 47f5a6fdcfb6ee795f84ca8e19c0954b68a2fae9
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/05/2018
---
# <a name="path-effects"></a>Efekty ścieżki

_Odnajdywanie różnego rodzaju zezwalających na ścieżki do zastosowania w przypadku używana do malowania i wypełnianie efektów ścieżki_

A *efekt ścieżki* jest wystąpieniem [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) klasy, która zostanie utworzona z jedną statyczną osiem `Create` metody. `SKPathEffect` Następnie ustawiono obiektu [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) właściwość `SKPaint` obiektu dla różnych interesujące efekty, na przykład używana do malowania wiersza ze ścieżką replikowanego małych:

![](effects-images/patheffectsample.png "Przykładowy łańcuch powiązanych")

Ścieżka efekty umożliwiają:

- Obrysu linii z kropki i łączniki
- Obrysu linii z dowolną ścieżkę wypełniony
- Wypełnianie obszaru z kreskowania linii
- Wypełnianie obszaru ze ścieżką ułożonymi sąsiadująco
- Upewnij się, ostrych zaokrąglone narożniki
- Dodaj losowe "zakłóceń", do linii i krzywych

Ponadto można połączyć dwóch lub więcej efektów ścieżki.

W tym artykule przedstawiono również sposób użycia `GetFillPath` metody `SKPaint` przekonwertować jednej ścieżki na inną ścieżkę, stosując właściwości `SKPaint`, takie jak `StrokeWidth` i `PathEffect`. Powoduje to niektóre ciekawe technik, takich jak uzyskanie ścieżkę, która jest konspekt inną ścieżkę. `GetFillPath` jest również przydatne w połączeniu z efekty ścieżki.

## <a name="dots-and-dashes"></a>Kropki i łączniki

Korzystanie z [ `PathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) metoda została opisana w artykule [ **kropki i kreski**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md). Pierwszy argument metody jest tablicą zawierającą parzystej liczby dwa lub więcej wartości przemian długości kresek i długości luki pomiędzy kresek:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

Te wartości są *nie* szerokość pociągnięć. Na przykład, jeśli szerokość pociągnięć to 10, i ma wiersz składa kwadratowy kresek i odstępów kwadratowych, ustaw `intervals` tablicy do {10, 10}. `phase` Argument wskazuje, gdzie we wzorze wiersz rozpoczyna się. W tym przykładzie, jeśli chcesz, aby wiersz, który ma rozpoczynać się od kwadratowy przerwy, ustaw `phase` do 10.

Dotyczy zakończenia kresek `StrokeCap` właściwość `SKPaint`. Międzynarodowe obrysu szerokości, jest często ustawić tę właściwość na `SKStrokeCap.Round` zostać zaokrąglona zakończenia łączników na pauzy. W tym przypadku wartości w `intervals` tablicy *nie* dodatkowe długość wyniku zaokrąglenia, co oznacza, że kropką cykliczne wymaga określenia szerokość zero. Grubość 10, aby utworzyć linię o i przerwy między punktami średnica okręgu kropkami, użyć `intervals` tablicę {0, 20}.

**Animowany tekst kropkami** strona jest podobna do **opisane tekst** strony opisaną w artykule [ **integrowanie tekstu i grafiki** ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) w Wyświetlanie opisane znaki tekstu przez ustawienie `Style` właściwość `SKPaint` do obiektu `SKPaintStyle.Stroke`. Ponadto **animowany tekst kropkami** używa `SKPathEffect.CreateDash` umożliwiają to konspektu kropkowanej wygląd, a także animuje program `phase` argument `SKPathEffect.CreateDash` metodę, aby dokonać kropki wydaje się przesyłanie wokół tekstu znaki. Oto stronę w trybie krajobraz:

[![](effects-images/animateddottedtext-small.png "Potrójna zrzut ekranu strony animowany tekst kropkami")](effects-images/animateddottedtext-large.png#lightbox "Potrójna zrzut ekranu strony animowany kropkami tekstu")

[ `AnimatedDottedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Klasy rozpoczyna się od definicji niektóre stałe, a także zastępuje `OnAppearing` i `OnDisappearing` metody dla animacji:

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    const string text = "DOTTED";
    const float strokeWidth = 10;
    static readonly float[] dashArray = { 0, 2 * strokeWidth };

    SKCanvasView canvasView;
    bool pageIsActive;

    public AnimatedDottedTextPage()
    {
        Title = "Animated Dotted Text";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;

        Device.StartTimer(TimeSpan.FromSeconds(1f / 60), () =>
        {
            canvasView.InvalidateSurface();
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

`PaintSurface` Obsługi rozpoczyna się od utworzenia `SKPaint` obiektu do wyświetlania tekstu. `TextSize` Właściwości zostanie zmieniona w oparciu o szerokości ekranu:

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create an SKPaint object to display the text
        using (SKPaint textPaint = new SKPaint
            {
                Style = SKPaintStyle.Stroke,
                StrokeWidth = strokeWidth,
                StrokeCap = SKStrokeCap.Round,
                Color = SKColors.Blue,
            })
        {
            // Adjust TextSize property so text is 95% of screen width
            float textWidth = textPaint.MeasureText(text);
            textPaint.TextSize *= 0.95f * info.Width / textWidth;

            // Find the text bounds
            SKRect textBounds;
            textPaint.MeasureText(text, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Animate the phase; t is 0 to 1 every second
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 1 / 1);
            float phase = -t * 2 * strokeWidth;

            // Create dotted line effect based on dash array and phase
            using (SKPathEffect dashEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                // Set it to the paint object
                textPaint.PathEffect = dashEffect;

                // And draw the text
                canvas.DrawText(text, xText, yText, textPaint);
            }
        }
    }
}
```

Pod koniec metodę `SKPathEffect.CreateDash` metoda jest wywoływana przy użyciu `dashArray` zdefiniowanej jako pola i animowany `phase` wartość. `SKPathEffect` Ustawiono wystąpienie `PathEffect` właściwość `SKPaint` obiektu do wyświetlania tekstu.

Alternatywnie, można ustawić `SKPathEffect` do obiektu `SKPaint` obiektu przed pomiaru tekst i wyśrodkowania go na stronie. W takim przypadku jednak animowany kropki i kreski spowodować niektóre zmiany rozmiaru renderowanego tekstu i zwykle Włącz wibrację nieco tekst. (Wypróbuj!)

Należy także zauważyć, czy jako animowany punktów wokół znaków znajduje się punkt niektórych każdego zamkniętej krzywej gdzie prawdopodobnie punktów pop i istnienia. To, których ścieżką, która definiuje konspektu znak rozpoczyna się i kończy się. Jeśli długość ścieżki nie jest całkowitą wielokrotnością długość (w tym przypadku 20 pikseli) wzorze kreskowym, gdzie tylko część tego wzorca może składać się na końcu ścieżki.

Można zmienić długość wzorze dopasowywane do długości ścieżki, ale która wymaga określenia długości ścieżki, techniki, które można w przyszłości artykułu.

**Kropka / pauzę Morph** program animuje wzorze sama tak, aby łączniki prawdopodobnie podział punktów, które Połącz ponownie w łączniki formularza:

[![](effects-images/dotdashmorph-small.png "Potrójna zrzut ekranu przedstawiający stronę Morph Dash kropka")](effects-images/dotdashmorph-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Morph Dash kropka")

[ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Klasy zastąpienia `OnAppearing` i `OnDisappearing` metody tak samo jak jak poprzedni program, ale definiuje klasę `SKPaint` obiektu jako pola:

```csharp
public class DotDashMorphPage : ContentPage
{
    const float strokeWidth = 30;
    static readonly float[] dashArray = new float[4];

    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint ellipsePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = strokeWidth,
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create elliptical path
        using (SKPath ellipsePath = new SKPath())
        {
            ellipsePath.AddOval(new SKRect(50, 50, info.Width - 50, info.Height - 50));

            // Create animated path effect
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 3 / 3);
            float phase = 0;

            if (t < 0.25f)  // 1, 0, 1, 2 --> 0, 2, 0, 2
            {
                float tsub = 4 * t;
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2 * tsub;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2;
            }
            else if (t < 0.5f)  // 0, 2, 0, 2 --> 1, 2, 1, 0
            {
                float tsub = 4 * (t - 0.25f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2 * (1 - tsub);
                phase = strokeWidth * tsub;
            }
            else if (t < 0.75f) // 1, 2, 1, 0 --> 0, 2, 0, 2
            {
                float tsub = 4 * (t - 0.5f);
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2 * tsub;
                phase = strokeWidth * (1 - tsub);
            }
            else               // 0, 2, 0, 2 --> 1, 0, 1, 2
            {
                float tsub = 4 * (t - 0.75f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2 * (1 - tsub);
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2;
            }

            using (SKPathEffect pathEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                ellipsePaint.PathEffect = pathEffect;
                canvas.DrawPath(ellipsePath, ellipsePaint);
            }
        }
    }
}
```

`PaintSurface` Obsługi tworzy eliptycznej ścieżki na podstawie rozmiaru strony i wykonuje dużo części kodu, który ustawia `dashArray` i `phase` zmiennych. Jako zmienną animowany `t` w zakresie od 0 do 1, `if` bloki podzielić tego czasu do czterech kwartałów i w każdym z tych kwartałów `tsub` również zakresu od 0 do 1. Na końcu, program tworzy `SKPathEffect` i ustawia ją na `SKPaint` obiektu do rysowania.

## <a name="from-path-to-path"></a>Ze ścieżki do ścieżki

[ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) Metody `SKPaint` włącza jedną ścieżkę do innego na podstawie ustawień w `SKPaint` obiektu. Aby zobaczyć, jak to działa, należy zastąpić `canvas.DrawPath` wywołania w poprzednim program poniższym kodem:

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

W tym nowy kod `GetFillPath` wywołać konwertuje `ellipsePath` (która jest po prostu elipsę) do `newPath`, wyświetlone z `newPaint`. `newPaint` Obiekt jest tworzony przy użyciu wszystkich ustawień właściwości domyślnych z wyjątkiem `Style` ustawiono na podstawie właściwości na wartość logiczną wartością zwracaną z `GetFillPath`.

Elementy wizualne są identyczne z wyjątkiem kolor, który jest ustawiony w `ellipsePaint` , ale nie `newPaint`. Zamiast prosty wielokropek zdefiniowane w `ellipsePath`, `newPath` zawiera wiele konturów ścieżki definiujące serii kropki i łączniki. Jest to wynik zastosowania różne właściwości `ellipsePaint` — `StrokeWidth`, `StrokeCap`, i `PathEffect` — do `ellipsePath` i wprowadzenie Ścieżka wynikowa `newPath`. `GetFillPath` Metoda zwraca wartość Boolean wskazującą, czy ścieżka docelowa jest do wypełnienia; w tym przykładzie jest zwracana wartość `true` do wypełniania ścieżki.

Spróbuj zmienić `Style` w `newPaint` do `SKPaintStyle.Stroke` i zobaczysz konturów poszczególnych ścieżki z jednego pikseli szerokości linii.

## <a name="stroking-with-a-path"></a>Używana do malowania ze ścieżką

[ `SKPathEffect.Create1DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create1DPath/p/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPath1DPathEffectStyle/) Metody jest podobny do `SKPathEffect.CreateDash` z tą różnicą, że należy określić ścieżkę zamiast wzorzec kresek i odstępów. Ta ścieżka jest replikowana wielokrotnie obrysu wiersza lub krzywej.

Składnia jest następująca:

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

> [!IMPORTANT]
> Uważaj: Brak przeciążenia `Create1DPath` zdefiniowanego z wyliczenia argumentu typu `SkPath1DPathEffect` z małe "k." Ta nazwa jest błąd, a w związku z tym wyliczenia i metody są przestarzałe, ale jest bardzo proste metody przestarzałych stały się częścią kodu i jest trudne zobaczyć dokładnie, co jest nieprawidłowe.

W ogólności, ścieżkę, którą można przekazać do `Create1DPath` będą małe i wyśrodkowany wokół punktu (0, 0). `advance` Parametr wskazuje odległość od centrów ścieżkę jako ścieżkę są replikowane w wierszu. Zazwyczaj argument ten ma przybliżonej szerokość ścieżki. `phase` Tego samego elementu role w tym miejscu ponieważ jest w pełni argument `CreateDash` metody.

[ `SKPath1DPathEffectStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath1DPathEffectStyle/) Ma trzy elementy członkowskie:

- [`Translate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Translate/)
- [`Rotate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Rotate/)
- [`Morph`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Morph/)

`Translate` Ścieżki pozostają w tym samym orientacji są replikowane wzdłuż linii lub krzywej powoduje, że element członkowski. Aby uzyskać `Rotate`, ścieżka jest obracana oparte na tangens krzywej. Ścieżka ma jego normalnej orientację poziome linie. `Morph` przypomina `Rotate` z tą różnicą, że sama ścieżka jest również krzywych odpowiadające łuku wiersza jest malowania.

**1 D ścieżki efekt** strony przedstawiono te trzy opcje. [ **OneDimensionalPathEffectPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) plik definiuje selektora, zawierający trzy elementy odpowiadające trzy elementy członkowskie wyliczenia:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.OneDimensionalPathEffectPage"
             Title="1D Path Effect">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="effectStylePicker"
                Title="Effect Style"
                Grid.Row="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Translate</x:String>
                <x:String>Rotate</x:String>
                <x:String>Morph</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1" />
    </Grid>
</ContentPage>
```

[ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) plik CodeBehind definiuje trzy `SKPathEffect` obiektów jako pola. Te są wszystkie utworzone za pomocą `SKPathEffect.Create1DPath` z `SKPath` obiektów utworzonych przy użyciu `SKPath.ParseSvgPathData`. Pierwsza to proste pole, kształtu romb jest drugi i trzeci jest prostokąt. Są one używane do zaprezentowania style trzy wpływu:

```csharp
public partial class OneDimensionalPathEffectPage : ContentPage
{
    SKPathEffect translatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 -10 L 10 -10, 10 10, -10 10 Z"),
                                  24, 0, SKPath1DPathEffectStyle.Translate);

    SKPathEffect rotatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 0 L 0 -10, 10 0, 0 10 Z"),
                                  20, 0, SKPath1DPathEffectStyle.Rotate);

    SKPathEffect morphPathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -25 -10 L 25 -10, 25 10, -25 10 Z"),
                                  55, 0, SKPath1DPathEffectStyle.Morph);

    SKPaint pathPaint = new SKPaint
    {
        Color = SKColors.Blue
    };

    public OneDimensionalPathEffectPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath path = new SKPath())
        {
            path.MoveTo(new SKPoint(0, 0));
            path.CubicTo(new SKPoint(2 * info.Width, info.Height),
                         new SKPoint(-info.Width, info.Height),
                         new SKPoint(info.Width, 0));

            switch (effectStylePicker.Items[effectStylePicker.SelectedIndex])
            {
                case "Translate":
                    pathPaint.PathEffect = translatePathEffect;
                    break;

                case "Rotate":
                    pathPaint.PathEffect = rotatePathEffect;
                    break;

                case "Morph":
                    pathPaint.PathEffect = morphPathEffect;
                    break;
            }

            canvas.DrawPath(path, pathPaint);
        }
    }
}
```

`PaintSurface` Krzywej Beziera, pętle się wokół, która uzyskuje dostęp do wyboru, aby określić, które tworzy program obsługi `PathEffect` powinien być używany do obrysu. Trzy opcje — `Translate`, `Rotate`, i `Morph` — są wyświetlane od lewej do prawej:

[![](effects-images/1dpatheffect-small.png "Potrójna zrzut ekranu przedstawiający stronę efekt ścieżki 1D")](effects-images/1dpatheffect-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę efekt ścieżki 1 D")

Ścieżka określona w `SKPathEffect.Create1DPath` metody zawsze jest wypełnione. Ścieżka określona w `DrawPath` metody jest zawsze malowania, jeśli `SKPaint` obiekt ma jego `PathEffect` właściwość efektu ścieżki 1 D. Zwróć uwagę, że `pathPaint` nie ma obiektu `Style` ustawienie, które normalnie domyślnie `Fill`, ale ścieżka jest malowania niezależnie od.

Pole używane w `Translate` przykładem jest kwadratowy, 20 pikseli i `advance` argument ma wartość 24. Ta różnica powoduje, że odstęp między polami, gdy wiersz jest około pozioma lub pionowa, ale pola nakładają się nieco linia jest ukośnych, ponieważ przekątnej pola jest 28.3 pikseli. 

Kształt romb `Rotate` przykładem jest również 20 pikseli szerokości. `advance` Ustawiono 20, dzięki czemu punkty nadal touch jako romb jest obracana wraz ze łuku wiersza.

Prostokąt w `Morph` przykładem jest 50 pikseli szerokości z `advance` ustawienie 55 aby mały odstęp między prostokątów one zgięte wokół krzywej Beziera.

Jeśli `advance` argument jest mniejszy niż rozmiar ścieżki, a następnie replikowane ścieżki mogą nakładać się na. Może to spowodować interesujące efekty. **Połączonego łańcucha** serii nakładających się okręgi, które wydają się tak, aby przypominały łańcuch połączonych zawiesza się w kształcie charakterystyczne sieci jezdnej zostanie wyświetlona strona:

[![](effects-images/linkedchain-small.png "Potrójna zrzut ekranu przedstawiający stronę połączonego łańcucha")](effects-images/linkedchain-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę połączonego łańcucha")

Znajdź ścisłe i pojawi się te nie są faktycznie okręgi. Każde łącze w łańcuchu jest dwa łuki o rozmiarze i znajduje się więc wydawać się one nawiązać połączenia z sąsiadujących łącza.

Łańcuch lub kabla dystrybucja wag uniform zawiesza się w formularzu sieci jezdnej. Architektura wbudowane w formie odwrócony sieci jezdnej korzysta z rozkładając wykorzystania z wagą arch. Sieci jezdnej ma pozornie prosty opis matematyczne:

y = · COSH(x / a)

*Cosh* funkcji cosinus hiperboliczny jest. Dla *x* równa 0, *cosh* wynosi zero i *y* jest równe *a*. To Centrum sieci jezdnej. Podobnie jak *cosinus* funkcji *cosh* jest określany jako *nawet*, co oznacza, że *cosh(–x)* jest równe *cosh(x)*, i zwiększyć wartości zwiększenia argumenty dodatnią lub ujemną. Te wartości opisano krzywych, które tworzą stron sieci jezdnej.

Znajdowanie odpowiednich wartości *a* dopasowania sieci jezdnej wymiary strony telefonu nie ma bezpośredniego obliczeń. Jeśli *w* i *h* są szerokość i wysokość prostokąta, wartość optymalne *a* spełnia następujące równanie:

cosh(w / 2 / a) = 1 + h / a

W następujące metody [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) klasy zawiera tego równości, odwołując się do dwóch wyrażeń po lewej i prawej znak równości jako `left` i `right`. Dla małych wartości *a*, `left` jest większa niż `right`; w przypadku dużych wartości *a*, `left` jest mniejsza niż `right`. `while` Pętli zawęża w optymalnych wartości *a*:

```csharp
float FindOptimumA(float width, float height)
{
    Func<float, float> left = (float a) => (float)Math.Cosh(width / 2 / a);
    Func<float, float> right = (float a) => 1 + height / a;

    float gtA = 1;         // starting value for left > right
    float ltA = 10000;     // starting value for left < right

    while (Math.Abs(gtA - ltA) > 0.1f)
    {
        float avgA = (gtA + ltA) / 2;

        if (left(avgA) < right(avgA))
        {
            ltA = avgA;
        }
        else
        {
            gtA = avgA;
        }
    }

    return (gtA + ltA) / 2;
}
```

`SKPath` Obiekt łącza jest tworzony w Konstruktorze tej klasy, a wynikowe `SKPathEffect` następnie ustawiono obiektu `PathEffect` właściwość `SKPaint` obiektu, który jest przechowywany jako pola:

```csharp
public class LinkedChainPage : ContentPage
{
    const float linkRadius = 30;
    const float linkThickness = 5;

    Func<float, float, float> catenary = (float a, float x) => (float)(a * Math.Cosh(x / a));

    SKPaint linksPaint = new SKPaint
    {
        Color = SKColors.Silver
    };

    public LinkedChainPage()
    {
        Title = "Linked Chain";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the individual links
        SKRect outer = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
        SKRect inner = outer;
        inner.Inflate(-linkThickness, -linkThickness);

        using (SKPath linkPath = new SKPath())
        {
            linkPath.AddArc(outer, 55, 160);
            linkPath.ArcTo(inner, 215, -160, false);
            linkPath.Close();

            linkPath.AddArc(outer, 235, 160);
            linkPath.ArcTo(inner, 395, -160, false);
            linkPath.Close();

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(linkPath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);
        }
    }
    ...
}
```

Głównym zadaniem `PaintSurface` program obsługi jest można utworzyć ścieżki dla sieci jezdnej samej siebie. Po ustaleniu optymalnego *a* i przechowywanie ich w `optA` zmiennej, również należy go obliczyć przesunięcie od góry okna. Następnie go utworzy się kolekcja `SKPoint` wartości dla sieci jezdnej, który przekształcić ścieżkę i rysowanie ścieżki z utworzonej wcześniej `SKPaint` obiektu:

```csharp
public class LinkedChainPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        // Width and height of catenary
        int width = info.Width;
        float height = info.Height - linkRadius;

        // Find the optimum 'a' for this width and height
        float optA = FindOptimumA(width, height);

        // Calculate the vertical offset for that value of 'a'
        float yOffset = catenary(optA, -width / 2);

        // Create a path for the catenary
        SKPoint[] points = new SKPoint[width];

        for (int x = 0; x < width; x++)
        {
            points[x] = new SKPoint(x, yOffset - catenary(optA, x - width / 2));
        }

        using (SKPath path = new SKPath())
        {
            path.AddPoly(points, false);

            // And render that path with the linksPaint object
            canvas.DrawPath(path, linksPaint);
        }
    }
    ...
}
```

Ten program Określa ścieżkę używaną w `Create1DPath` mieć jego (0, 0) punkt w Centrum. Wydaje się to uzasadnione ponieważ (0, 0) punktu ścieżki jest wyrównana z wierszem lub krzywej, który jest ona adorning. Jednak można użyć z systemem innym niż — wyśrodkowany (0, 0) w punkcie niektóre efekty specjalne.

**Taśmy taśmy** strony tworzy ścieżki przypominającą taśmy podłużnych taśmy z krzywej górnej i dolnej to jest dopasowywany do wymiary okna. Ta ścieżka jest malowania przy użyciu prostego `SKPaint` 20 pikseli szerokości i kolorowe szary obiektów, a następnie malowania ponownie z inną `SKPaint` obiekt z `SKPathEffect` odwołuje się do ścieżki przypominającą zasobnika małego obiektu:

[![](effects-images/conveyorbelt-small.png "Potrójna zrzut ekranu przedstawiający stronę taśmy taśmy")](effects-images/conveyorbelt-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę taśmy taśmy")

(0, 0) punktu ścieżki zasobnika jest uchwytu, tak po wybraniu `phase` argument jest animowany, zasobników prawdopodobnie obracać wokół taśmy taśmy, prawdopodobnie tworzenie zakresów się wody u dołu i zrzucanie go na początku.

[ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) Klasa implementuje animacji z zastąpieniami o `OnAppearing` i `OnDisappearing` metody. Ścieżka zasobnika jest zdefiniowany w Konstruktorze strony:

```csharp
public class ConveyorBeltPage : ContentPage
{
    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint conveyerPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 20,
        Color = SKColors.DarkGray
    };

    SKPath bucketPath = new SKPath();

    SKPaint bucketsPaint = new SKPaint
    {
        Color = SKColors.BurlyWood,
    };

    public ConveyorBeltPage()
    {
        Title = "Conveyor Belt";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the bucket starting with the handle
        bucketPath.AddRect(new SKRect(-5, -3, 25, 3));

        // Sides
        bucketPath.AddRoundedRect(new SKRect(25, -19, 27, 18), 10, 10, 
                                  SKPathDirection.CounterClockwise);
        bucketPath.AddRoundedRect(new SKRect(63, -19, 65, 18), 10, 10, 
                                  SKPathDirection.CounterClockwise);

        // Five slats
        for (int i = 0; i < 5; i++)
        {
            bucketPath.MoveTo(25, -19 + 8 * i);
            bucketPath.LineTo(25, -13 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small, 
                             SKPathDirection.CounterClockwise, 65, -13 + 8 * i);
            bucketPath.LineTo(65, -19 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small, 
                             SKPathDirection.Clockwise, 25, -19 + 8 * i);
            bucketPath.Close();
        }

        // Arc to suggest the hidden side
        bucketPath.MoveTo(25, -17);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small, 
                         SKPathDirection.Clockwise, 65, -17);
        bucketPath.LineTo(65, -19);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small, 
                         SKPathDirection.CounterClockwise, 25, -19);
        bucketPath.Close();

        // Make it a little bigger and correct the orientation
        bucketPath.Transform(SKMatrix.MakeScale(-2, 2));
        bucketPath.Transform(SKMatrix.MakeRotationDegrees(90));
    }
    ...
```

Kod tworzenia zasobników kończy się z dwóch transformacje, wchodzące w zasobniku nieco większy a włączyć ją w bok. Te transformacji jest łatwiejsze niż dostosowywania wszystkie współrzędne w poprzednim kodzie. 

`PaintSurface` Obsługi rozpoczyna się od definicji ścieżki dla samej siebie taśmy taśmy. Jest to po prostu pary linii i parę częściowo okręgi, które są rysowane linią szary ciemny 20 pikseli na poziomie:

```csharp
public class ConveyorBeltPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float width = info.Width / 3;
        float verticalMargin = width / 2 + 150;

        using (SKPath conveyerPath = new SKPath())
        {
            // Straight verticals capped by semicircles on top and bottom
            conveyerPath.MoveTo(width, verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large, 
                               SKPathDirection.Clockwise, 2 * width, verticalMargin);
            conveyerPath.LineTo(2 * width, info.Height - verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large, 
                               SKPathDirection.Clockwise, width, info.Height - verticalMargin);
            conveyerPath.Close();

            // Draw the conveyor belt itself
            canvas.DrawPath(conveyerPath, conveyerPaint);

            // Calculate spacing based on length of conveyer path
            float length = 2 * (info.Height - 2 * verticalMargin) +
                           2 * ((float)Math.PI * width / 2);

            // Value will be somewhere around 200
            float spacing = length / (float)Math.Round(length / 200);

            // Now animate the phase; t is 0 to 1 every 2 seconds
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 2 / 2);
            float phase = -t * spacing;

            // Create the buckets PathEffect
            using (SKPathEffect bucketsPathEffect = 
                        SKPathEffect.Create1DPath(bucketPath, spacing, phase, 
                                                  SKPath1DPathEffectStyle.Rotate))
            {
                // Set it to the Paint object and draw the path again
                bucketsPaint.PathEffect = bucketsPathEffect;
                canvas.DrawPath(conveyerPath, bucketsPaint);
            }
        }
    }
}
```

Logikę rysowania taśmy taśmy nie działa w trybie krajobraz.

Pakiety powinien rozmieszczonych o 200 pikseli od siebie na taśmie taśmy. Jednak taśmy taśmy prawdopodobnie nie jest wielokrotnością liczby 200 pikseli długi, co oznacza, które jako `phase` argumentu `SKPathEffect.Create1DPath` jest animowany, pakiety zostaną pop do i z istnienia. 

Z tego powodu najpierw jest obliczana wartość o nazwie `length` czyli długość taśmy taśmy. Ponieważ taśmy taśmy zawiera proste i częściowo okręgi, to proste obliczenia. Następnie Liczba zasobników, jest obliczany przez podzielenie `length` przez 200. To jest zaokrąglana do najbliższej liczby całkowitej, a następnie jest numer podzielony na `length`. Wynik jest odstępy dla całkowitej liczby zasobników. `phase` Argument jest po prostu ułamek tego.

## <a name="from-path-to-path-again"></a>Ze ścieżki do ścieżki ponownie

W dolnej części `DrawSurface` obsługi w **taśmy taśmy**, komentarz `canvas.DrawPath` wywołania i zastąp go następującym kodem:

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

Tak jak w poprzednim przykładzie `GetFillPath`, zobaczysz, że wyniki są takie same, z wyjątkiem kolor. Po wykonaniu `GetFillPath`, `newPath` obiekt zawiera wiele kopii ścieżki zasobnik, każdy znajduje się w tym samym miejscu że animacja się je w momencie wywołania.

## <a name="hatching-an-area"></a>Kreskowaniu obszaru

[ `SKPathEffect.Create2DLines` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DLine/p/System.Single/SkiaSharp.SKMatrix/) Metody wypełnia obszar o równoległych, często nazywane *kreskowania linii*. Metoda ma następującą składnię:

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width` Argument określa szerokość pociągnięć kreskowania linii. `matrix` Parametr jest kombinacją skalowania i opcjonalne obrotu. Czynnik skalowania wskazuje przyrost pikseli, używaną do obszar wierszy kreskowania Skia. Rozdzielenie wierszy jest czynnik skalowania minus `width` argumentu. Jeśli czynnik skalowania jest mniejsza lub równa `width` wartość, nie będą bez spacji między liniami kreskowania i obszar pojawi się zostać wypełnione. Określ taką samą wartość skalowania w poziomie i w pionie. 

Domyślnie kreskowania wiersze są poziomej. Jeśli `matrix` parametr zawiera obracanie, linie kreskowania są obracane wskazówek zegara.

**Kreskowania wypełnienia** strony ilustruje efekt tej ścieżki. [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) Klasa definiuje trzy skutki ścieżkę jako pola, pierwszy kreskowania poziomych linii o szerokości 3 pikseli z skalowania wskazujący współczynnik, że są rozmieszczone 6 pikseli od siebie. Rozdzielenie między wierszami w związku z tym jest 3 pikseli. Drugi ścieżka powoduje dla linii pionowej kreskowania szerokość 6 pikseli rozmieszczonych 24 pikseli od siebie, (tak oddzielenie jest 18 pikseli), a trzeci jest ukośnych kreskowania linii 12 pikseli szerokości rozmieszczonych w odległości 36 pikseli od siebie. 

```csharp
public class HatchFillPage : ContentPage
{
    SKPaint fillPaint = new SKPaint();

    SKPathEffect horzLinesPath = SKPathEffect.Create2DLine(3, SKMatrix.MakeScale(6, 6));

    SKPathEffect vertLinesPath = SKPathEffect.Create2DLine(6, 
        Multiply(SKMatrix.MakeRotationDegrees(90), SKMatrix.MakeScale(24, 24)));

    SKPathEffect diagLinesPath = SKPathEffect.Create2DLine(12, 
        Multiply(SKMatrix.MakeScale(36, 36), SKMatrix.MakeRotationDegrees(45)));

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target;
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

Zwróć uwagę, macierzy `Multiply` metody. Ponieważ czynniki skalowania poziome i pionowe są takie same, kolejność pomnożyć macierzy skalowanie i obrót nie ma znaczenia.

`PaintSurface` Program obsługi używa tych skutków trzy ścieżki z trzech różnych kolorach w połączeniu z `fillPaint` do wypełnienia zaokrąglony prostokąt o rozmiarze do dopasowania do strony. `Style` Ustawiona w `fillPaint` jest zignorowana; gdy `SKPaint` obiekt zawiera efektu ścieżki utworzone na podstawie `SKPathEffect.Create2DLine`, obszar jest wypełniany niezależnie od tego:

```csharp
public class HatchFillPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath roundRectPath = new SKPath())
        {
            // Create a path 
            roundRectPath.AddRoundedRect(
                new SKRect(50, 50, info.Width - 50, info.Height - 50), 100, 100);

            // Horizontal hatch marks
            fillPaint.PathEffect = horzLinesPath;
            fillPaint.Color = SKColors.Red;
            canvas.DrawPath(roundRectPath, fillPaint); 

            // Vertical hatch marks
            fillPaint.PathEffect = vertLinesPath;
            fillPaint.Color = SKColors.Blue;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Diagonal hatch marks -- use clipping
            fillPaint.PathEffect = diagLinesPath;
            fillPaint.Color = SKColors.Green;

            canvas.Save();
            canvas.ClipPath(roundRectPath);
            canvas.DrawRect(new SKRect(0, 0, info.Width, info.Height), fillPaint);
            canvas.Restore();

            // Outline the path
            canvas.DrawPath(roundRectPath, strokePaint);
        }
    }
    ...
}
```

Jeśli przyjrzymy się uważnie wyniki, zobaczysz, czy wiersze kreskowania czerwona i niebieska nie są ograniczone dokładnie do zaokrąglony prostokąt. (Jest to prawdopodobnie cech kodu Skia.) Jeśli jest niewystarczające, informacje o innym podejściu jest wyświetlany na zielono linii ukośnych kreskowania: zaokrąglony prostokąt jest używany jako ścieżki przycinania i linie kreskowania są wyświetlane na całej stronie.

`PaintSurface` Obsługi stwierdza, w wyniku wywołania po prostu obrysu zaokrąglony prostokąt, dzięki czemu niezgodności z wierszami kreskowania czerwona i niebieska:

[![](effects-images/hatchfill-small.png "Potrójna zrzut ekranu przedstawiający stronę kreskowania wypełnienia")](effects-images/hatchfill-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę kreskowania wypełnienia")

Android ekranu naprawdę nie wygląda jak: skalowanie na zrzucie ekranu spowodowała cienkie czerwoną linie i alokowania skonsolidować na pozornie szerszej czerwoną linie oraz szersze przestrzenie.

## <a name="filling-with-a-path"></a>Wypełnianie ścieżki

[ `SKPathEffect.Create2DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DPath/p/SkiaSharp.SKMatrix/SkiaSharp.SKPath/) Skutkuje umożliwia wypełnienie obszaru ścieżki, które są replikowane w poziomie i w pionie, ustawianie widoku kafelków obszar:

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix` Skalowania czynniki wskazują odstępy w poziomie i pionie replikowanych ścieżki. Ale nie można obrócić za pomocą tej ścieżki `matrix` argumentu; Jeśli chcesz, aby ścieżka rotacji Obróć sama ścieżka przy użyciu `Transform` metody zdefiniowanej przez `SKPath`. 

Zreplikowane ścieżki zwykle jest wyrównany do lewej lub górnej krawędzi ekranu, a nie w obszarze wypełniany. To zachowanie można przesłonić, zapewniając czynniki tłumaczenie między 0 a skalowania czynniki, które określają przesunięcia w poziomie i w pionie z lewej i górnej krawędzi.

**Wypełnienia kafelka ścieżki** strony ilustruje efekt tej ścieżki. Nie zdefiniowano ścieżki używany w obszarze przy podziale jako pola w [ `PathFileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) klasy. Współrzędne poziome i pionowe zakres z od – 40 do 40, co oznacza, że ta ścieżka jest kwadratowy 80 następującą liczbę pikseli: 

```csharp
public class PathTileFillPage : ContentPage
{
    SKPath tilePath = SKPath.ParseSvgPathData(
        "M -20 -20 L 2 -20, 2 -40, 18 -40, 18 -20, 40 -20, " + 
        "40 -12, 20 -12, 20 12, 40 12, 40 40, 22 40, 22 20, " + 
        "-2 20, -2 40, -20 40, -20 8, -40 8, -40 -8, -20 -8 Z");
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Red;

            using (SKPathEffect pathEffect =
                   SKPathEffect.Create2DPath(SKMatrix.MakeScale(64, 64), tilePath))
            {
                paint.PathEffect = pathEffect;

                canvas.DrawRoundRect(
                    new SKRect(50, 50, info.Width - 50, info.Height - 50), 
                    100, 100, paint);
            }
        }
    }
}
```

W `PaintSurface` obsługi `SKPathEffect.Create2DPath` wywołania ustawia odstępy w poziomie i w pionie 64 spowodować 80 piksel kwadratowe Kafelki nakładanie się. Na szczęście ścieżka podobny element układanki, meshing dobrze z sąsiadujące Kafelki:

[![](effects-images/pathtilefill-small.png "Potrójna zrzut ekranu strony wypełnienia kafelka ścieżki")](effects-images/pathtilefill-large.png#lightbox "Potrójna zrzut ekranu strony wypełnienia kafelka ścieżki")

Skalowanie z oryginalnego zrzut ekranu powoduje, że niektóre zakłócenia, szczególnie na ekranie systemu Android.

Zwróć uwagę, że te Kafelki zawsze wyświetlane całego i nigdy nie są obcinane. Z wyjątkiem na ekranie systemu Windows 10 Mobile nie jest jeszcze manipulacji, że obszar wypełniany jest zaokrąglony prostokąt. Jeśli ma zostać obcięta te Kafelki do określonego obszaru, użyj ścieżki przycinania.

Spróbuj ustawienie `Style` właściwość `SKPaint` do obiektu `Stroke`, i zobaczysz Kafelki poszczególnych opisanych zamiast wypełnione.

## <a name="rounding-sharp-corners"></a>Zaokrąglania narożników Sharp

**Zaokrąglona Heptagon** program przedstawionych w [ **trzech sposobów, aby narysować łuk** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) artykułu umożliwiają arcus tangens krzywą punkty dwustronnych siedmiu rysunku. **Innego zaokrąglona Heptagon** strona zawiera to znacznie łatwiejsze podejście, które używa efektu ścieżki utworzone na podstawie [ `SKPathEffect.CreateCorner` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCorner/p/System.Single/) — metoda:

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

Chociaż jeden argument ma nazwę `radius` należy skonfigurować urządzenie do połowy promień narożnika żądany. (Jest to cecha Skia kodu).

Oto `PaintSurface` obsługi w [ `AnotherRoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) klasy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);
    SKPoint[] vertices = new SKPoint[numVertices];
    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    float cornerRadius = 100;

    // Create the path
    using (SKPath path = new SKPath())
    {
        path.AddPoly(vertices, true);

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            // Set argument to half the desired corner radius!
            paint.PathEffect = SKPathEffect.CreateCorner(cornerRadius / 2);

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);

            // Uncomment DrawCircle call to verify corner radius
            float offset = cornerRadius / (float)Math.Sin(Math.PI * (numVertices - 2) / numVertices / 2);
            paint.Color = SKColors.Green;
            // canvas.DrawCircle(vertices[0].X, vertices[0].Y + offset, cornerRadius, paint);
        }
    }
}
```

W tym celu można użyć z używana do malowania lub wypełnianie na podstawie `Style` właściwość `SKPaint` obiektu. Poniżej przedstawiono na wszystkich platformach trzy:

[![](effects-images/anotherroundedheptagon-small.png "Potrójna zrzut ekranu strony innego zaokrąglona Heptagon")](effects-images/anotherroundedheptagon-large.png#lightbox "Potrójna zrzut ekranu strony innego Heptagon zaokrąglona")

Zobaczysz, że to zaokrąglony heptagon jest identyczny z wcześniej program. Jeśli potrzebujesz bardziej wiarygodne promień narożnika naprawdę to 100 zamiast 50 określone w `SKPathEffect.CreateCorner` wywołanie, użytkownik może usuń znaczniki komentarza końcowej instrukcji w programie i Zobacz koło 100-radius nałożoną na prawym górnym rogu.

## <a name="random-jitter"></a>Losowe zakłócenia

Czasami uzyskuje doskonałą linii prostych grafiki komputera nie są jeszcze ma i wymagane jest nieco losowości. W takim przypadku można spróbować [ `SKPathEffect.CreateDiscrete` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDiscrete/p/System.Single/System.Single/System.UInt32/) metody:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

Można użyć w tym celu Ścieżka używana do malowania lub wypełnianie. Wiersze są oddzielone w segmentach połączonych — przybliżonej długość jest określona przez `segLength` — i rozszerzyć w różnych kierunkach. Zakres odchylenia od oryginalnego wiersza jest określona przez `deviation`. 

Ostatni argument jest inicjator służący do generowania sekwencja pseudolosowego używane dla efektu. Efekt zakłócenia będzie wyglądać nieco inaczej w różnych ds. Argument ma wartość domyślną równą zero, co oznacza, że efekt jest taki sam, przy każdym uruchomieniu programu. Jeśli chcesz korzystać z różnych zakłócenia zawsze, gdy jest odowieżany ekranu, można ustawić inicjatora `Millisecond` właściwość `DataTime.Now` wartości (na przykład).


**Zakłóceń eksperymentu** strony można wypróbować różne wartości w prostokącie używana do malowania:

[![](effects-images/jitterexperiment-small.png "Potrójne zrzut ekranu przedstawiający stronę zakłóceń eksperymentu")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

Program jest straightfoward. [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) pliku tworzy dwa `Slider` elementów i `SKCanvasView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.JitterExperimentPage"
             Title="Jitter Experiment">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Label">
                    <Setter Property="HorizontalTextAlignment" Value="Center" />
                </Style>

                <Style TargetType="Slider">
                    <Setter Property="Margin" Value="20, 0" />
                    <Setter Property="Minimum" Value="0" />
                    <Setter Property="Maximum" Value="100" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="segLengthSlider"
                Grid.Row="0"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference segLengthSlider},
                              Path=Value,
                              StringFormat='Segment Length = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="deviationSlider"
                Grid.Row="2"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference deviationSlider},
                              Path=Value,
                              StringFormat='Deviation = {0:F0}'}"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

`PaintSurface` Obsługi w [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) pliku CodeBehind jest wywoływana przy każdym `Slider` wartość zmienia się. Wywołuje `SKPathEffect.CreateDiscrete` przy użyciu dwóch `Slider` wartości i użyty do obrysu prostokąt:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float segLength = (float)segLengthSlider.Value;
    float deviation = (float)deviationSlider.Value;

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke; 
        paint.StrokeWidth = 5;
        paint.Color = SKColors.Blue;

        using (SKPathEffect pathEffect = SKPathEffect.CreateDiscrete(segLength, deviation))
        {
            paint.PathEffect = pathEffect;

            SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

W tym celu można użyć do wypełniania, jak również w takim przypadku podlega te odchylenia losowe konspektu wypełnienia obszaru. **Zakłóceń tekst** pokazuje strony za pomocą tego efektu ścieżki do wyświetlania tekstu. Większość kodu w `PaintSurface` obsługi [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) klasy jest przeznaczony do zmiany rozmiaru i wyśrodkowania tekst:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "FUZZY";

    using (SKPaint textPaint = new SKPaint())
    {
        textPaint.Color = SKColors.Purple;
        textPaint.PathEffect = SKPathEffect.CreateDiscrete(3f, 10f);

        // Adjust TextSize property so text is 95% of screen width
        float textWidth = textPaint.MeasureText(text);
        textPaint.TextSize *= 0.95f * info.Width / textWidth;

        // Find the text bounds
        SKRect textBounds;
        textPaint.MeasureText(text, ref textBounds);

        // Calculate offsets to center the text on the screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

W tym miejscu działa w trybie krajobraz na wszystkich platformach trzy:

[![](effects-images/jittertext-small.png "Potrójne zrzut ekranu przedstawiający stronę zakłóceń tekstu")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>Ścieżka określająca zakres

Już przedstawiono dwa przykłady małego [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) metody `SKPaint`, która występuje także w [przeciążenia](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/):

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale)
```

Tylko pierwsze dwa argumenty są wymagane. Metoda uzyskuje dostęp do ścieżki odwołuje się `src` argumentu, modyfikuje dane ścieżki na podstawie właściwości stroke w `SKPaint` obiektu (w tym `PathEffect` właściwości), a następnie zapisuje wyniki w `dst` ścieżki. `resScale` Parametr umożliwia zmniejszenie dokładność, aby utworzyć mniejsze ścieżkę docelową i `cullRect` argument można wyeliminować konturów poza prostokątem.

Jedno podstawowe użycie tej metody nie wiąże się z efekty ścieżki w ogóle. Jeśli `SKPaint` obiekt ma jego `Style` ustawioną właściwość `SKPaintStyle.Stroke`i wykonuje *nie* ma jego `PathEffect` wartość to `GetFillPath` tworzy ścieżki, która reprezentuje *konspektu*ścieżki źródłowej tak, jakby było zostały malowania za pomocą właściwości programu paint.

Na przykład jeśli `src` ścieżka jest proste kółko promienia 500 i `SKPaint` obiektu określa szerokość pociągnięć 100, a następnie `dst` ścieżka staje się dwie koncentrycznych, jeden z promień 450, a druga z protokołem radius 550. Metoda jest wywoływana `GetFillPath` ponieważ wypełnianie to `dst` ścieżka jest taka sama jak używana do malowania `src` ścieżki. Ale można również obrysu `dst` ścieżkę, aby zobaczyć zawiera ścieżkę.

**Naciśnij, aby konspektu, ścieżka** pokazano to. `SKCanvasView` i `TapGestureRecognizer` wystąpienia są tworzone w [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) pliku. [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) plik CodeBehind definiuje trzy `SKPaint` obiektów jako pola, dwa dla używana do malowania z obrysu szerokości 100 i 20 i trzeci na wypełnianie:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    bool outlineThePath = false;

    SKPaint redThickStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 100
    };

    SKPaint redThinStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 20
    };

    SKPaint blueFill = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    public TapToOutlineThePathPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewTapped(object sender, EventArgs args)
    {
        outlineThePath ^= true;
        (sender as SKCanvasView).InvalidateSurface();
    }
    ...
}
```

Jeśli nie zostały nacisnął ekranu, `PaintSurface` program obsługi używa `blueFill` i `redThickStroke` malowanie obiektów do renderowania cykliczne ścieżki:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circlePath = new SKPath())
        {
            circlePath.AddCircle(info.Width / 2, info.Height / 2,
                                 Math.Min(info.Width / 2, info.Height / 2) - 
                                 redThickStroke.StrokeWidth);

            if (!outlineThePath)
            {
                canvas.DrawPath(circlePath, blueFill);
                canvas.DrawPath(circlePath, redThickStroke);
            }
            else
            {
                using (SKPath outlinePath = new SKPath())
                {
                    redThickStroke.GetFillPath(circlePath, outlinePath);

                    canvas.DrawPath(outlinePath, blueFill);
                    canvas.DrawPath(outlinePath, redThinStroke);
                }
            }
        }
    }
}
```

Okręgu jest wypełniony i malowania zgodnie z regułami:

[![](effects-images/taptooutlinethepathnormal-small.png "Potrójna zrzut ekranu przedstawiający normalne strony wybierz do konturu Path")](effects-images/taptooutlinethepathnormal-large.png#lightbox "Potrójna zrzut ekranu przedstawiający normalne strony wybierz do konturu ścieżki")

Po wybraniu ekranu, `outlineThePath` ma ustawioną wartość `true`i `PaintSurface` obsługi tworzy świeża `SKPath` obiektu i użyty jako ścieżkę docelową w wywołaniu `GetFillPath` na `redThickStroke` paint obiektu. Tej ścieżki docelowej jest wypełniony i malowania z `redThinStroke`, co w następujących czynności:

[![](effects-images/taptooutlinethepathoutlined-small.png "Potrójna zrzut ekranu strony wybierz do konturu Path obramowane")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "Potrójna zrzut ekranu strony wybierz do konturu Path obramowane")

Dwa czerwone kółka wyraźnie wskazuje, że oryginalna ścieżka cykliczne został przekonwertowany do dwóch konturów cykliczne.

Ta metoda może być przydatne w przypadku tworzenia ścieżek do użycia na potrzeby `SKPathEffect.Create1DPath` metody. Zawsze są wypełnione ścieżek, które określisz w tych metod, gdy ścieżki są replikowane. Jeśli nie chcesz pełną ścieżkę do wypełnienia, należy zdefiniować dokładnie konturów.

Na przykład w **połączonego łańcucha** przykładowa łącza zostały zdefiniowane z serii cztery łuki, każda para znajdowały się na dwóch promień do konspektu ścieżki do wypełnienia obszaru. Istnieje możliwość Zastąp kod w `LinkedChainPage` klasę, aby zrobić to nieco inaczej.

Najpierw należy do ponownego zdefiniowania `linkRadius` stałe:

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath` Jest obecnie tylko dwa łuki oparte na tym pojedynczego serwera radius z żądaną start kąty i odchylenia kątów:

```csharp
using (SKPath linkPath = new SKPath())
{
    SKRect rect = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
    linkPath.AddArc(rect, 55, 160);
    linkPath.AddArc(rect, 235, 160);

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = linkThickness;

        using (SKPath outlinePath = new SKPath())
        {
            strokePaint.GetFillPath(linkPath, outlinePath);

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(outlinePath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);

        }

    }
}
```

`outlinePath` Obiekt jest następnie odbiorcy konturu `linkPath` po jest malowania z właściwości określone w `strokePaint`. 

Innym przykładem ta technika jest powtarzający się dalej dla ścieżki używany w `SKPathEffect.Create2DPath` metody. 

## <a name="combining-path-effects"></a>Łączenie efekty ścieżki

Dwie metody końcowego statyczne tworzenie `SKPathEffect` są [ `SKPathEffect.CreateSum` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateSum/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/) i [ `SKPathEffect.CreateCompose` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCompose/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

Obie te metody łączyć dwa efekty ścieżki do utworzenia efektu złożony ścieżki. `CreateSum` Metoda tworzy efekt ścieżki, który jest podobny do efekty dwie ścieżki oddzielnie, podczas `CreateCompose` stosuje się jeden efekt ścieżki ( `inner`), a następnie stosuje `outer` do tego.

Napotkano już jak `GetFillPath` metody `SKPaint` można przekonwertować jednej ścieżki do innej ścieżki na podstawie `SKPaint` właściwości (w tym `PathEffect`) więc nie powinny być *zbyt* brzmieć tajemniczo, w jaki sposób `SKPaint`obiektu można wykonać tej operacji dwukrotnie z skutków ścieżki dwóch określonych w `CreateSum` lub `CreateCompose` metody.

Jedno użycie oczywiste `CreateSum` jest określenie `SKPaint` obiekt, który wypełnia ścieżki z mocą jedną ścieżkę i obrysy ścieżki począwszy innej ścieżki. To jest przedstawiona w **kotów w ramce** próbki, które wyświetla tablicę kotów w ramce z Pofałdowany krawędzi:

[![](effects-images/catsinframe-small.png "Potrójna zrzut ekranu przedstawiający stronę kotów w ramce")](effects-images/catsinframe-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę kotów w ramce")

[ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) Klasy rozpoczyna się od definicji kilka pól. Rozpoznają pierwsze pole z [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) klasę z [ **danych ścieżki SVG** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) artykułu. Drugi ścieżka jest oparta na wiersz i łuk dla wzorca St Jacques ramki:

```csharp
public class CatsInFramePage : ContentPage
{
    // From PathDataCatPage.cs
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint catStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5
    };

    SKPath scallopPath = 
        SKPath.ParseSvgPathData("M 0 0 L 50 0 A 60 60 0 0 1 -50 0 Z");

    SKPaint framePaint = new SKPaint
    {
        Color = SKColors.Black
    };
    ...
}
```

`catPath` Mogą być używane w `SKPathEffect.Create2DPath` metody Jeśli `SKPaint` obiektu `Style` właściwość jest ustawiona na `Stroke`. Jednak jeśli `catPath` jest używana bezpośrednio przez ten program, następnie cały nagłówek cat zostaną wypełnione i wąsów nawet nie będzie wyświetlany. (Wypróbuj!) Należy uzyskać konspektu tej ścieżki i że konspekt w `SKPathEffect.Create2DPath` metody.

Konstruktor wykonuje to zadanie. Najpierw dotyczy dwóch przekształceń do `catPath` przenoszenia (0, 0) wskaż Centrum i skali jego rozmiar. `GetFillPath` uzyskuje konturów konturów w `outlinedCatPath`, a obiekt jest używany w `SKPathEffect.Create2DPath` wywołania. Skalowanie uwzględnia `SKMatrix` wartości są nieco większe niż poziomej i pionowy rozmiar cat dostarczyć mały bufor między Kafelki, gdy były czynniki tłumaczenia pochodnych nieco empirycznie tak, aby nie jest widoczna pełna cat lewego górnego rogu ramki:

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    public CatsInFramePage()
    {
        Title = "Cats in Frame";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Move (0, 0) point to center of cat path
        catPath.Transform(SKMatrix.MakeTranslation(-240, -175));

        // Now catPath is 400 by 250
        // Scale it down to 160 by 100
        catPath.Transform(SKMatrix.MakeScale(0.40f, 0.40f));

        // Get the outlines of the contours of the cat path
        SKPath outlinedCatPath = new SKPath();
        catStroke.GetFillPath(catPath, outlinedCatPath);

        // Create a 2D path effect from those outlines
        SKPathEffect fillEffect = SKPathEffect.Create2DPath(
            new SKMatrix { ScaleX = 170, ScaleY = 110,
                           TransX = 75, TransY = 80,
                           Persp2 = 1 },
            outlinedCatPath);

        // Create a 1D path effect from the scallop path
        SKPathEffect strokeEffect = 
            SKPathEffect.Create1DPath(scallopPath, 75, 0, SKPath1DPathEffectStyle.Rotate);

        // Set the sum the effects to frame paint
        framePaint.PathEffect = SKPathEffect.CreateSum(fillEffect, strokeEffect);
    }
    ...
}
```

Wywołuje konstruktor `SKPathEffect.Create1DPath` Pofałdowany ramki. Zwróć uwagę, że szerokość ścieżki jest 100 pikseli, ale wcześniejszym jest 75 pikseli, dzięki czemu nakłada replikowanych ścieżki wokół ramki. Końcowe zestawienie wywołania konstruktora `SKPathEffect.CreateSum` Aby połączyć efekty dwie ścieżki, a następnie ustaw dla wyniku `SKPaint` obiektu.

Zezwala na tę pracę `PaintSurface` obsługi ma być dość proste. Wymaga tylko do definiowania prostokąt i rysowanie za pomocą `framePaint`:

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect rect = new SKRect(50, 50, info.Width - 50, info.Height - 50);
        canvas.ClipRect(rect);
        canvas.DrawRect(rect, framePaint);
    }
}
```

Algorytmy za skutki ścieżki zawsze powodować całej ścieżki używana do malowania lub wypełnianie mają być wyświetlane, co może spowodować, że niektóre elementy wizualne znajdujące się poza prostokątem. `ClipRect` Wywołać przed `DrawRect` wywołania umożliwia wizualnych się znacznie oczyszczarki. (Wypróbuj bez przycinania)

Często, aby użyć `SKPathEffect.CreateCompose` można dodać niektóre zakłócenia w innym celu ścieżki. Oczywiście możesz eksperymentować samodzielnie, ale w tym miejscu jest nieco inny przykład:

**Linie przerywane kreskowania** wypełnia elipsy kreskowania linii, które są oznaczone kreskowaną. Większość pracy w [ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) klasy jest wykonywane bezpośrednio w definicji pola. Te pola zdefiniuj efekt łączniki i efekt kreskowania. Są one zdefiniowane jako `static` ponieważ są one następnie przywoływany w `SKPathEffect.CreateCompose` wywołanie w `SKPaint` definicji:

```csharp
public class DashedHatchLinesPage : ContentPage
{
    static SKPathEffect dashEffect = 
        SKPathEffect.CreateDash(new float[] { 30, 30 }, 0);

    static SKPathEffect hatchEffect = SKPathEffect.Create2DLine(20,
        Multiply(SKMatrix.MakeScale(60, 60), 
                 SKMatrix.MakeRotationDegrees(45)));

    SKPaint paint = new SKPaint()
    {
        PathEffect = SKPathEffect.CreateCompose(dashEffect, hatchEffect),
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target;
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

`PaintSurface` Konieczność obsługi zawierają tylko standardowe koszty oraz jednego wywołania `DrawOval`:

```csharp
public class DashedHatchLinesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawOval(info.Width / 2, info.Height / 2, 
                        0.45f * info.Width, 0.45f * info.Height, 
                        paint);
    }
    ...
}
```

Jak już znasz, kreskowania linii nie są dokładnie ograniczone do wewnętrznych obszaru, a w tym przykładzie zawsze podawać po lewej stronie z łącznikiem całego:

[![](effects-images/dashedhatchlines-small.png "Potrójna zrzut ekranu przedstawiający stronę linii kreskowanej kreskowania")](effects-images/dashedhatchlines-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę kreskowane kreskowania linii")

Teraz, w tym samouczku efekty ścieżki, od prostego kropki i łączniki do kombinacji dziwne, użyj wyobraźni i zobacz, można utworzyć.



## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
