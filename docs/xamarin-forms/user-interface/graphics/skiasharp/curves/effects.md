---
title: Efekty ścieżek w SkiaSharp
description: W tym artykule opisano różne efekty ścieżek SkiaSharp, które zezwalać na ścieżki do użytku z stykają i wypełniając i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: charlespetzold
ms.author: chape
ms.date: 07/29/2017
ms.openlocfilehash: 28f628fb4e8ab77e9c36e6e1972d7269ad0dad4d
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615681"
---
# <a name="path-effects-in-skiasharp"></a>Efekty ścieżek w SkiaSharp

_Odkryj różne efekty ścieżek, zezwalających na ścieżki do zastosowania w przypadku stykają i wypełnianie_

A *efekt ścieżki* jest wystąpieniem [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) klasę, która jest tworzona przy użyciu jednego z ośmiu statyczne `Create` metody. `SKPathEffect` Obiekt jest następnie ustawiany [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) właściwość `SKPaint` obiektu dla różnych interesujących efektów, na przykład stykają wiersza ze ścieżką replikowanego małych:

![](effects-images/patheffectsample.png "Przykładowy łańcuch połączonych")

Efekty ścieżek umożliwiają:

- Wiersz z kropki i kreski pociągnięcia
- Pociągnięcia linii z dowolną ścieżkę wypełnienia
- Wypełnianie obszaru przy użyciu kreskowania wierszy
- Wypełnianie obszaru ze ścieżką fragmentacji
- Upewnij się, ostrych zaokrąglone rogi
- Dodaj losowe "zakłóceń", do linii i krzywych

Ponadto możesz połączyć dwa lub więcej efekty ścieżek.

W tym artykule przedstawiono również sposób użycia `GetFillPath` metody `SKPaint` przekonwertować jednej ścieżki na inną ścieżkę, stosując właściwości `SKPaint`, w tym `StrokeWidth` i `PathEffect`. Skutkuje to kilka interesujących technik, takich jak uzyskanie ścieżki zarys inną ścieżkę. `GetFillPath` jest również przydatne w połączeniu z efekty ścieżek.

## <a name="dots-and-dashes"></a>Kropki i kreski

Korzystanie z [ `PathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) metoda została opisana w artykule [ **kropki i kreski**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md). Pierwszy argument metody jest tablica zawierająca parzystą liczbą dwie lub więcej wartości przemian długości kresek i długość przerwy między kresek:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

Te wartości są *nie* szerokość pociągnięcia. Na przykład, jeśli szerokość pociągnięcia wynosi 10 i chcesz, aby wiersz składa się z kwadratowy kresek i odstępów kwadratowy, ustaw `intervals` tablicy do {10, 10}. `phase` Argument wskazuje, gdzie we wzorze kreskowym wiersz rozpoczyna się. W tym przykładzie wiersz, który ma zaczynać się od kwadratowy przerwy, ustawić `phase` do 10.

Dotyczy zakończenia kresek `StrokeCap` właściwość `SKPaint`. Szerokości szerokiego obrysu często zdarza się, aby ustawić tę właściwość na `SKStrokeCap.Round` zaokrąglić zakończenia kresek. W tym przypadku wartości w `intervals` czy tablica *nie* dodatkowe długość wyniku zaokrąglania, co oznacza, że cykliczne kropka wymaga określenia pliku szerokość równa zero. Szerokość pociągnięcia, 10, aby utworzyć linię z i przerwy między punktami średnica okręgu kropek, należy użyć `intervals` tablicy {0, 20}.

**Animowane tekstu kropkowana** strona jest podobna do **opisane tekstu** strony opisaną w artykule [ **integrowanie tekstu i grafiki** ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) w to, że wyświetlane wyróżnione znaki tekstowe, ustawiając `Style` właściwość `SKPaint` obiekt `SKPaintStyle.Stroke`. Ponadto **animowane tekstu kropkowana** używa `SKPathEffect.CreateDash` pozwala to konspektu notacji wygląd, a także animuje program `phase` argument `SKPathEffect.CreateDash` metody kropki wydaje się, że podróży wokół tekstu znaki. Poniżej znajduje się Strona w trybie poziomym:

[![](effects-images/animateddottedtext-small.png "Potrójna zrzut ekranu przedstawiający stronę animowane tekstu z kropkami")](effects-images/animateddottedtext-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę animowane kropkowana tekstu")

[ `AnimatedDottedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Klasy rozpoczyna się od definicji kilka stałych i zastępuje również `OnAppearing` i `OnDisappearing` metody animacji:

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

`PaintSurface` Obsługi zaczyna się od utworzenia `SKPaint` obiektu, aby wyświetlić tekst. `TextSize` Właściwość jest uwzględniany w oparciu o szerokości ekranu:

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
            SKRect textBounds = new SKRect();
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

Pod koniec metody `SKPathEffect.CreateDash` metoda jest wywoływana przy użyciu `dashArray` która jest zdefiniowana jako pole i animowany `phase` wartość. `SKPathEffect` Wystąpienia jest równa `PathEffect` właściwość `SKPaint` obiektu, aby wyświetlić tekst.

Alternatywnie, można ustawić `SKPathEffect` obiekt `SKPaint` obiektu przed pomiaru tekst i Przygotuj się na stronie. W takim przypadku należy jednak animowane kropki i kreski spowodować pewne zmiany rozmiaru tekstu renderowanego i tekst zwykle nieco wibracje. (Wypróbuj)

Można także zauważyć, że jako kropki animowany wokół znaków tekstu jest pewnego momentu w każdym zamkniętej krzywej gdzie kropki wydaje się, że pop pojęcie istnienia. To, gdzie ścieżką, która definiuje konspektu znak rozpoczyna się i kończy. Jeśli długość ścieżki nie jest całkowitą wielokrotnością długość (w tym przypadku 20 pikseli) wzorze kreskowym tylko część tego wzorca może składać się na końcu ścieżki.

Można zmienić długość wzorze kreskowym, aby dopasować długość ścieżki, ale wymaga określająca długość ścieżki, metody, która jest opisane w przyszłym artykule.

**Dot / kreski Morph** program animuje wzorze kreskowym, sama tak, aby kreski wydaje się, że podzielić kropek, które są łączone w celu kresek formularz ponownie:

[![](effects-images/dotdashmorph-small.png "Potrójna zrzut ekranu przedstawiający stronę Morph Dash kropka")](effects-images/dotdashmorph-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Morph Dash kropka")

[ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Klasy zastąpienia `OnAppearing` i `OnDisappearing` metod, podobnie jak jak poprzedni program, ale definiuje klasę `SKPaint` obiektu jako pola:

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

`PaintSurface` Obsługi tworzy eliptycznego ścieżkę, w zależności od rozmiaru strony i wykonuje długie sekcji kodu, który ustawia `dashArray` i `phase` zmiennych. Jako zmienna animowany `t` z zakresu od 0 do 1, `if` bloki Podziel tego czasu do czterech kwartałów i w każdym z tych kwartałów `tsub` również zakresu od 0 do 1. Na samym końcu program tworzy `SKPathEffect` i ustawia ją na `SKPaint` obiektu do rysowania.

## <a name="from-path-to-path"></a>Ze ścieżki do ścieżki

[ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) Metody `SKPaint` jest przekształcany jedną ścieżkę zgodnie z ustawieniami w `SKPaint` obiektu. Aby zobaczyć, jak to działa, należy zastąpić `canvas.DrawPath` wywołania poprzedni program poniższym kodem:

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

W tym nowym kodzie `GetFillPath` wywołać konwertuje `ellipsePath` (jest to po prostu elipsę) do `newPath`, wyświetlony przy użyciu `newPaint`. `newPaint` Obiekt zostanie utworzony przy użyciu wszystkie domyślne ustawienia właściwości z tą różnicą, że `Style` właściwość jest ustawiona na podstawie na atrybut typu wartość logiczna zwraca wartość z `GetFillPath`.

Wizualizacje są identyczne, z wyjątkiem koloru, który jest ustawiony w `ellipsePaint` , ale nie `newPaint`. Zamiast prosty wielokropek zdefiniowane w `ellipsePath`, `newPath` zawiera wiele konturów ścieżki, które definiują szeregu kropki i kreski. Jest to wynik zastosowania różnych właściwości obiektu `ellipsePaint` — `StrokeWidth`, `StrokeCap`, i `PathEffect` — aby `ellipsePath` i umieszczenie Ścieżka wynikowa w `newPath`. `GetFillPath` Metoda zwraca wartość logiczną wskazującą, czy ścieżka docelowa jest do wypełnienia; w tym przykładzie wartość zwracana jest `true` do wypełniania ścieżki.

Spróbuj zmienić `Style` w `newPaint` do `SKPaintStyle.Stroke` a zobaczysz konturów ścieżki poszczególnych opisane za pomocą jednego pikseli szerokości linii.

## <a name="stroking-with-a-path"></a>Kreskowanie przy użyciu ścieżki

[ `SKPathEffect.Create1DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create1DPath/p/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPath1DPathEffectStyle/) Metoda jest podobna do `SKPathEffect.CreateDash` z tą różnicą, że możesz określić ścieżkę, a nie wzór z kresek i luk. Ta ścieżka jest replikowana wielokrotnie obrysu wiersza lub krzywej.

Składnia jest następująca:

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

> [!IMPORTANT]
> Należy uważać: Brak przeciążenia `Create1DPath` zdefiniowanego za pomocą argumentu typu wyliczenia `SkPath1DPathEffect` z małych 'k'. Ta nazwa jest błąd, a w związku z tym wyliczenia i metody są przestarzałe, ale jest bardzo proste przestarzałe metody stać się częścią kodu i trudno zobaczyć dokładnie, co jest nieprawidłowy.

Ogólnie rzecz biorąc, ścieżki, który jest przekazywany do `Create1DPath` będą małe i wyśrodkowany wokół punktu (0, 0). `advance` Parametr wskazuje odległości od centrów ścieżki, jako ścieżkę są replikowane w wierszu. Ten argument jest zwykle ustawiane na przybliżone szerokość ścieżki. `phase` Argument pełni rolę tej samej tutaj go, jak w `CreateDash` metody.

[ `SKPath1DPathEffectStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath1DPathEffectStyle/) Ma trzy elementy członkowskie:

- [`Translate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Translate/)
- [`Rotate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Rotate/)
- [`Morph`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Morph/)

`Translate` Elementu członkowskiego powoduje, że ścieżka do jest zachowywana w tym samym orientacji są replikowane wzdłuż linii lub krzywej. Aby uzyskać `Rotate`, ścieżka obraca się zależnie od tangens krzywej. Ścieżka ma jego normalnej orientację poziome linie. `Morph` jest podobny do `Rotate` z tą różnicą, że sama ścieżka jest również krzywych do dopasowania krzywiznę wiersza jest malowania.

**1 D ścieżki efekt** strona przedstawia te trzy opcje. [ **OneDimensionalPathEffectPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) plik definiuje selektora zawierający trzy elementy odpowiadające trzy elementy członkowskie wyliczenia:

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

[ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) pliku związanego z kodem definiuje trzy `SKPathEffect` obiektów jako pola. Są one wszystkie tworzone przy użyciu `SKPathEffect.Create1DPath` z `SKPath` obiektów utworzonych za pomocą `SKPath.ParseSvgPathData`. Pierwsza to proste okno, druga jest kształt rombu, a trzeci jest prostokąta. Są one używane do zademonstrowania style trzy wpływu:

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

`PaintSurface` Obsługi tworzy się wokół w pętli, która uzyskuje dostęp do selektora, aby określić, które krzywej Beziera `PathEffect` powinien być używany do obrysu. Trzy opcje — `Translate`, `Rotate`, i `Morph` — są wyświetlane od lewej do prawej:

[![](effects-images/1dpatheffect-small.png "Potrójna zrzut ekranu przedstawiający stronę efekt ścieżki 1D")](effects-images/1dpatheffect-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę efekt ścieżki 1 D")

Ścieżka określona w `SKPathEffect.Create1DPath` metody zawsze jest wypełnione. Ścieżka określona w `DrawPath` metody jest zawsze malowania, jeśli `SKPaint` obiekt ma jego `PathEffect` właściwością w celu ścieżki 1 D. Należy zauważyć, że `pathPaint` nie ma obiektu `Style` ustawienie, które zazwyczaj wartość domyślna to `Fill`, ale ścieżka jest malowania niezależnie od tego.

Pole używane w `Translate` przykładem jest kwadratowy, 20 pikseli i `advance` argument ma wartość 24. Różnica ta powoduje, że przerwa między polami, gdy wiersz jest około poziomej lub pionowej, ale pola nakładają się na siebie nieco linia jest skos, ponieważ po przekątnej pola jest 28.3 pikseli.

Kształt rombu w `Rotate` przykład jest również 20 pikseli szerokości. `advance` Zostaje ustawiony na 20, tak aby punkty w dalszym ciągu touch jako rombu jest obracana wraz ze krzywiznę linii.

Kształt prostokąta w `Morph` przykład to 50 pikseli z `advance` ustawienie 55 się mały odstęp między prostokątami, zgodnie z ich zgięte wokół krzywej Beziera.

Jeśli `advance` argument jest mniejszy niż rozmiar ścieżkę, a następnie replikowane ścieżki może pokrywać się. Może to spowodować, że interesujące efekty. **Łańcuch połączonych** strony prezentuje serię nakładających się okręgów, które wydają się podobne łańcuch połączonych zawiesza się szczególne kształtu sieci jezdnej:

[![](effects-images/linkedchain-small.png "Potrójna zrzut ekranu przedstawiający stronę łańcuch połączonych")](effects-images/linkedchain-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę połączonego łańcucha")

Przejrzyj ścisłe i zostaną wyświetlone te nie są faktycznie okręgi. Każde łącze w łańcuchu jest dwa łuki, rozmiar i umieszczony, więc one wydaje się, że skupionej sąsiednimi łącza.

Łańcuch lub kabla dystrybucja wag jednolitego zawiesza się w formularzu sieci jezdnej. Arch wbudowane w formie odwróconą sieci jezdnej korzyści płynące z równomierną dystrybucję ciśnienia z wagą arch. Sieci jezdnej ma pozornie prosty opis matematyczne:

y = XI COSH(x / a)

*Cosh* jest funkcją cosinus hiperboliczny. Dla *x* równa 0, *cosh* wynosi zero i *y* jest równe *a*. To Centrum sieci jezdnej. Podobnie jak *cosinus* funkcji *cosh* jest nazywany *nawet*, co oznacza, że *cosh(–x)* jest równa *cosh(x)*, i wartości osi x rosną zwiększenia argumenty dodatnia lub ujemna. Wartości te opisują krzywych, które formie boków sieci jezdnej.

Znajdowanie odpowiednich wartości *a* dopasowania sieci jezdnej wymiary strony telefonu nie ma bezpośredniego obliczeń. Jeśli *w* i *h* są szerokość i wysokość prostokąta, wartość optymalne *a* spełnia następujące równanie:

COSH (w/2 /) = 1 + h /

Następującą metodę w [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) klasa obejmuje ten równości, odwołując się do dwóch wyrażeń po lewej stronie i po prawej stronie znaku równości, jako `left` i `right`. Dla małych wartości *a*, `left` jest większa niż `right`; w przypadku dużych wartości *a*, `left` jest mniejsza niż `right`. `while` Pętli zawęża w optymalnych wartości *a*:

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

`SKPath` Obiekt łącza zostanie utworzony w konstruktorze klasy i wynikowe `SKPathEffect` obiekt jest następnie ustawiany `PathEffect` właściwość `SKPaint` obiekt, który jest przechowywany jako pola:

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

Głównym zadaniem `PaintSurface` obsługi polega na utworzeniu ścieżkę sieci jezdnej sam. Po ustaleniu optymalnego *a* i przechowywanie ich w `optA` zmiennej, również należy go obliczyć przesunięcie od góry okna. Następnie może wzrosnąć zbiór `SKPoint` wartości dla sieci jezdnej, przekształcając, ścieżki i rysowanie ścieżki przy użyciu utworzonej wcześniej `SKPaint` obiektu:

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

Ten program Określa ścieżkę używaną w `Create1DPath` zapewnienie jego (0, 0) do punktu w środku. Zajmuje to nieco uzasadnione ponieważ (0, 0) punktu ścieżki jest powiązana z wiersza lub krzywej, że jest ona adorning. Jednak można użyć innych — a ich tematyka (0, 0) punktu, aby uzyskać pewne efekty specjalne.

**Taśmy taśmy** strony powoduje utworzenie ścieżki przypominającą taśmy prostokątnych taśmy z krzywej górnej i dolnej, to ma rozmiar, wymiary okna. Ta ścieżka jest malowania przy użyciu prostego `SKPaint` 20 pikseli szerokości i kolor szary obiektów, a następnie malowania ponownie z inną `SKPaint` obiekt z `SKPathEffect` obiekt odwołuje się do ścieżki podobny mały zasobnika:

[![](effects-images/conveyorbelt-small.png "Potrójna zrzut ekranu przedstawiający stronę taśmy taśmy")](effects-images/conveyorbelt-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę taśmy taśmy")

(0, 0) punktu ścieżki zasobnika jest uchwyt, więc kiedy `phase` argument jest animowany, zasobników wydaje się, że koncentrują się wokół taśmy taśmy, być może tworzenie zakresów się wodą u dołu i zrzucanie ją u góry.

[ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) Klasa implementuje animacji przy użyciu zastąpień `OnAppearing` i `OnDisappearing` metody. Ścieżka do pakietu jest zdefiniowany w Konstruktorze strony:

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

Kod tworzenia zasobnika kończy się z dwóch przekształceń, wchodzące w zasobniku nieco większy a obróceniu go. Stosowanie tych przekształceń było łatwiejsze dostosowywanie wszystkie współrzędne w poprzednim kodzie.

`PaintSurface` Obsługi rozpoczyna się od definicji ścieżkę taśmy taśmy sam. Jest to po prostu pary linii i parę rozdzielana okręgów, które są rysowane przy użyciu 20 pikseli na poziomie wiersza szary ciemny:

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

Logika rysowania taśmy taśmy nie działa w trybie poziomym.

Zasobników powinien rozłożone o 200 pikseli od siebie na taśmie taśmy. Jednak pas taśmy prawdopodobnie nie jest wielokrotnością liczby 200 pikseli długi, co oznacza, które jako `phase` argument `SKPathEffect.Create1DPath` jest animowany, zasobników zostanie wyświetlona w i poza istnienia.

Z tego powodu, najpierw jest obliczana wartość o nazwie `length` czyli długość taśmy taśmy. Ponieważ taśmy taśmy składa się z proste i rozdzielana okręgów, jest to proste obliczenia. Następnie liczba przedziałów jest obliczana przez podzielenie `length` przez 200. Ta jest zaokrąglana do najbliższej liczby całkowitej, a następnie jest numer podzielony na `length`. Wynik jest odstępy na całkowitą liczbę przedziałów. `phase` Argument jest po prostu ułamek tego.

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

Podobnie jak w poprzednim przykładzie `GetFillPath`, zobaczysz, że wyniki są takie same, z wyjątkiem kolor. Po wykonaniu `GetFillPath`, `newPath` obiekt zawiera wiele kopii ścieżki zasobnik, każdy umieszczony w tym samym narzędzia spot, animacji umieszczony ich w momencie zgłoszenia wywołania.

## <a name="hatching-an-area"></a>Kreskowaniu obszar

[ `SKPathEffect.Create2DLines` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DLine/p/System.Single/SkiaSharp.SKMatrix/) Metoda wypełni obszar za pomocą równoległych, często nazywane *kreskowania wiersze*. Metoda ma następującą składnię:

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width` Argument określa grubość linii kreskowania. `matrix` Parametru jest kombinacją obrotu skalowania i opcjonalnych. Współczynnik skalowania wskazuje przyrost pikseli, korzystającą z Skia celu wiersze kreskowania miejsca. Rozdzielenie między liniami jest współczynnik skalowania minus `width` argumentu. Jeśli współczynnik skalowania jest mniejsza niż lub równa `width` wartość, będzie istnieć bez spacji między wierszami kreskowania i obszaru pojawi się do wypełnienia. Określ taką samą wartość dla skalowania w poziomie i w pionie.

Domyślnie kreskowania wiersze są poziomy. Jeśli `matrix` parametr zawiera obrotu, wiersze kreskowania są obracane zgodnie ze wskazówkami zegara.

**Kreskowania wypełnienia** strony pokazuje, w tym celu ścieżki. [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) Klasa definiuje trzy skutki ścieżkę jako pola, pierwszy do Kreskowanie poziome linie o szerokości 3 pikseli z współczynnik skalowania wskazującym, że są one rozmieszczone 6 pikseli od siebie. Rozdzielenie między wierszami, dlatego jest 3 pikseli. Drugi efekt ścieżki jest Kreskowanie pionowe linie szerokość pikseli 6 rozmieszczonych 24 pikseli od siebie, (oddzielenie więc 18 pikseli), a trzecia będzie dla Kreskowanie ukośne linii 12 pikseli szeroki rozmieszczonych 36 pikseli od siebie.

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
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

Zwróć uwagę, macierzy `Multiply` metody. Ponieważ poziome i pionowe czynniki skalowania są takie same, kolejność pomnożyć macierzy skalowanie i obrót nie ma znaczenia.

`PaintSurface` Program obsługi używa efekty tych trzech ścieżek w trzech różnych kolorach, w połączeniu z `fillPaint` aby wypełnić prostokąt zaokrąglony o rozmiarze do dopasowania do strony. `Style` Właściwość ustawioną na `fillPaint` jest zignorowana; gdy `SKPaint` obiekt zawiera efekt ścieżki, utworzone na podstawie `SKPathEffect.Create2DLine`, obszar jest wypełniany niezależnie od tego:

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

Jeśli przyjrzymy się dokładnie wyniki, zobaczysz, czy wiersze kreskowania czerwonej i niebieskiej nie są ograniczone dokładnie do prostokąt zaokrąglony. (To jest prawdopodobnie cechy podstawowy kod Skia). Jeśli jest to niewystarczające, alternatywnym podejściem jest wyświetlany dla wierszy Kreskowanie ukośne w kolorze zielonym: prostokąt zaokrąglony jest używany jako przycinania i linie kreskowania są wyświetlane na całej stronie.

`PaintSurface` Obsługi stwierdza, za pomocą wywołania, można po prostu obrysu zaokrąglony prostokąt, tak aby było widać rozbieżność przy użyciu kreskowania czerwonej i niebieskiej wiersze:

[![](effects-images/hatchfill-small.png "Potrójna zrzut ekranu przedstawiający stronę kreskowania wypełnienia")](effects-images/hatchfill-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę kreskowania wypełnienia")

Android ekranu tak naprawdę nie wygląda tak jak: skalowanie na zrzucie ekranu spowodowała alokowania elastycznego czerwone linie i alokowania elastycznego można skonsolidować w pozornie szersze czerwone linie oraz szersze przestrzenie.

## <a name="filling-with-a-path"></a>Wypełnianie ścieżki

[ `SKPathEffect.Create2DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DPath/p/SkiaSharp.SKMatrix/SkiaSharp.SKPath/) Umożliwia wypełnienie obszar ścieżki, który jest replikowany w pionie i poziomie, obowiązują fragmentacji obszaru:

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix` Skalowania czynniki wskazują poziome i pionowe odstępy replikowanych ścieżki. Ale nie można obrócić ścieżki za pomocą tego `matrix` argument; Jeśli chcesz, aby ścieżka okresowych Obróć samej ścieżce za pomocą `Transform` metody zdefiniowanej przez `SKPath`.

Ścieżka replikowanych zwykle jest powiązana z lewym i górnym krawędzi ekranu, a nie w obszarze są wypełnione. Może zmienić to zachowanie, dostarczając czynniki tłumaczenia zakresu od 0 do skalowania czynników, które określają przesunięcia poziome i pionowe z lewym i górnym stron.

**Wypełnienia kafelka ścieżki** strony pokazuje, w tym celu ścieżki. Ścieżki używany w obszarze fragmentacji jest zdefiniowana jako pole w [ `PathFileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) klasy. Współrzędne poziome i pionowe zakresie od od – 40 do 40, co oznacza, że ta ścieżka jest 80 pikseli kwadrat:

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

W `PaintSurface` obsługi `SKPathEffect.Create2DPath` wywołania ustawia 64 spowodować Kafelki kwadratowy 80-pikselowe nakładanie się poziome i pionowe odstępy. Na szczęście ścieżkę podobny element układanki wśród, dobrze meshing przy użyciu sąsiadujących kafelków:

[![](effects-images/pathtilefill-small.png "Potrójna zrzut ekranu strony wypełnienia kafelka ścieżki")](effects-images/pathtilefill-large.png#lightbox "Potrójna zrzut ekranu strony wypełnienia kafelka ścieżki")

Skalowanie od oryginalnego zrzut ekranu powoduje, że niektóre jego zniekształcenia, szczególnie na ekranie dla systemu Android.

Należy zauważyć, że te Kafelki zawsze wyświetlane cały i nigdy nie są obcinane. W pierwszych dwóch zrzutów ekranu nie jest jeszcze widoczna, to obszar jest wypełniony prostokąt zaokrąglony. Obciąć tych kafelków do określonego obszaru, należy użyć ścieżki przycięcia.

Spróbuj ustawienie `Style` właściwość `SKPaint` obiekt `Stroke`, a zobaczysz poszczególnych kafelków opisane zamiast wypełnione.

## <a name="rounding-sharp-corners"></a>Zaokrąglanie narożników Sharp

**Zaokrąglone Heptagon** program znajdujące się w [ **trzy sposoby narysowania łuku** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) artykułu umożliwia arcus tangens krzywą punkty dwustronna siedmiu rysunku. **Innego Heptagon zaokrąglone** stronie znajdują się znacznie łatwiejsze podejście stosowane efekt ścieżki, utworzone na podstawie [ `SKPathEffect.CreateCorner` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCorner/p/System.Single/) metody:

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

Mimo że jeden argument jest o nazwie `radius` musi równa połowie promienia narożnika żądaną. (Jest to cecha podstawowy kod Skia).

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

W tym celu można użyć stykają lub wypełnianie na podstawie `Style` właściwość `SKPaint` obiektu. Poniżej przedstawiono na wszystkich trzech platformach:

[![](effects-images/anotherroundedheptagon-small.png "Potrójna zrzut ekranu strony innego Heptagon zaokrąglone")](effects-images/anotherroundedheptagon-large.png#lightbox "Potrójna zrzut ekranu strony innego Heptagon zaokrąglone")

Zobaczysz, czy ten zaokrąglone heptagon jest taka sama jak wcześniej program. Jeśli potrzebujesz bardziej wiarygodne promienia narożnika naprawdę to 100, a nie 50 określonych w `SKPathEffect.CreateCorner` wywołanie, usuniesz można komentarz końcowej instrukcji w programie i zobacz okrąg 100-radius nakładają się w prawym górnym rogu.

## <a name="random-jitter"></a>Losowe zakłócenia

Czasami nieskazitelnych proste linie grafiki w komputerze nie są dość ma i nieco losowość jest pożądane. W takim przypadku można spróbować [ `SKPathEffect.CreateDiscrete` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDiscrete/p/System.Single/System.Single/System.UInt32/) metody:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

W tym celu ścieżki służy stykają lub wypełnieniem formularza. Wiersze są podzielone na segmenty połączonych — przybliżony długość jest określona przez `segLength` — i rozszerzać w różnych kierunkach. Stopień odchylenia od oryginalnego wiersza jest określona przez `deviation`.

Ostatni argument jest używany do generowania pseudolosową sekwencja używana dla efektu inicjator. Efekt zakłócenia będzie wyglądać nieco inaczej dla różnych wartości początkowe. Argument ma wartość domyślną równą zero, co oznacza, że efekt jest taki sam, przy każdym uruchomieniu programu. Jeśli chcesz korzystać z różnych zakłócenia zawsze wtedy, gdy ekran jest odświeżana, można ustawić inicjatora `Millisecond` właściwość `DataTime.Now` wartości (na przykład).


**Zakłóceń eksperymentować** strona pozwala na eksperymentowanie z różnymi wartościami w stykają prostokąt:

[![](effects-images/jitterexperiment-small.png "Zrzut ekranu przedstawiający stronę zakłóceń eksperymentu potrójne")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

Program jest straightfoward. [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) plików są tworzone wystąpienia dwóch `Slider` elementy i `SKCanvasView`:

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

`PaintSurface` Obsługi w [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) pliku związanego z kodem jest wywoływana zawsze wtedy, gdy `Slider` wartość zmienia się. Wywołuje `SKPathEffect.CreateDiscrete` przy użyciu dwóch `Slider` wartości i użyty do obrysu prostokąt:

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

Efekt ten można stosować do wypełniania, w którym to przypadku konspektu wypełnionego obszaru podlega te losowe odchylenia. **Zakłóceń tekstu** strony, który demonstruje sposób użycia tego efektu ścieżki do wyświetlania tekstu. Większość kodu w `PaintSurface` program obsługi [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) klasy jest poświęcona rozmiary i wyśrodkowanie tekstu:

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
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        // Calculate offsets to center the text on the screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

W tym miejscu jest uruchomiona w trybie poziomym na wszystkich trzech platformach:

[![](effects-images/jittertext-small.png "Zrzut ekranu przedstawiający stronę zakłóceń tekstu potrójne")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>Ścieżka konspekt

Już przedstawiono dwa przykłady nieco [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) metody `SKPaint`, która występuje także w [przeciążenia](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/):

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale)
```

Wymagane są tylko pierwsze dwa argumenty. Metoda uzyskuje dostęp do ścieżki odwołuje się `src` argument, modyfikuje dane ścieżki na podstawie właściwości pociągnięcia w `SKPaint` obiektu (w tym `PathEffect` właściwości), a następnie zapisuje wyniki w `dst` ścieżki. `resScale` Parametr umożliwia zmniejszenie dokładności, aby utworzyć mniejsze ścieżkę docelową, a `cullRect` argument może wyeliminować konturów poza prostokąta.

Jeden podstawowe użycie tej metody nie wiąże się efekty ścieżek w ogóle. Jeśli `SKPaint` obiekt ma jego `Style` właściwością `SKPaintStyle.Stroke`i wykonuje *nie* mają jego `PathEffect` wartość to `GetFillPath` tworzy ścieżkę, która reprezentuje *konspektu*ścieżki źródłowego tak, jakby było zostały malowania za pomocą właściwości programu paint.

Na przykład jeśli `src` ścieżka jest proste koła o promieniu 500 i `SKPaint` obiektu określa szerokość pociągnięć 100, a następnie `dst` ścieżki staje się dwie koncentrycznych, jeden z protokołem radius w 450, a drugie z protokołem radius, 550. Metoda jest wywoływana `GetFillPath` ponieważ wypełnianie to `dst` ścieżki jest taka sama jak stykają `src` ścieżki. Ale metodę tę można również stosować `dst` ścieżkę, aby zobaczyć kontur ścieżki.

**Naciśnij, aby konspektu, ścieżka** pokazuje to. `SKCanvasView` i `TapGestureRecognizer` są tworzone w [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) pliku. [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) pliku związanego z kodem definiuje trzy `SKPaint` obiektów jako pola, dwa stykają z obrysu szerokości 100 i 20 i trzeci na wypełnianie:

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

Jeśli nie zostało nacisnął ekranu, `PaintSurface` program obsługi używa `blueFill` i `redThickStroke` malowanie obiekty do renderowania na ścieżce cyklicznej:

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

Okrąg wypełniony i malowania, jak można oczekiwać:

[![](effects-images/taptooutlinethepathnormal-small.png "Potrójna zrzut ekranu przedstawiający normalna strona naciśnij do konturu ścieżki")](effects-images/taptooutlinethepathnormal-large.png#lightbox "Potrójna zrzut ekranu przedstawiający normalna strona naciśnij do konturu ścieżki")

Po wybraniu ekranu `outlineThePath` jest ustawiona na `true`i `PaintSurface` obsługi tworzy świeży `SKPath` obiektu i używa, jako ścieżkę docelową, w wywołaniu `GetFillPath` na `redThickStroke` paint obiektu. Tej ścieżki docelowej jest wypełniony i malowania z `redThinStroke`, dając w następującym:

[![](effects-images/taptooutlinethepathoutlined-small.png "Potrójna zrzut ekranu przedstawiający konturu strony naciśnij do konturu Path")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "Potrójna zrzut ekranu przedstawiający konturu strony naciśnij do konturu ścieżki")

Dwa kółka w kolorze czerwonym jasno wskazywać, że oryginalna ścieżka cykliczne został przekształcony w dwóch konturów cykliczne.

Ta metoda może być przydatne w przypadku tworzenia ścieżek na potrzeby `SKPathEffect.Create1DPath` metody. Ścieżki, które określisz w tych metodach są wypełnione zawsze, gdy ścieżki są replikowane. Jeśli nie chcesz pełną ścieżkę do wypełnienia należy dokładnie zdefiniować konturów.

Na przykład w **łańcuch połączonych** przykładowe linki zostały zdefiniowane przy użyciu serii cztery łuki, każda para znajdowały się na dwóch promienie w konturze obszar ścieżki do wypełnienia. Jest możliwe, Zastąp kod w `LinkedChainPage` klasy, aby zrobić to nieco inaczej.

Najpierw należy zdefiniować `linkRadius` stałe:

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath` Jest obecnie tylko dwa łuki oparte na tym pojedynczej usługi radius z żądaną start kąty i kąta odchylenia:

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

`outlinePath` Obiekt jest następnie odbiorcy konturu `linkPath` kiedy jest malowania przy użyciu właściwości określonych w `strokePaint`.

Inny przykład, przy użyciu tej metody jest wyłączany dalej dla ścieżki używany w `SKPathEffect.Create2DPath` metody.

## <a name="combining-path-effects"></a>Łącząc efekty ścieżek

Dwie metody końcowego statyczne tworzenie `SKPathEffect` są [ `SKPathEffect.CreateSum` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateSum/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/) i [ `SKPathEffect.CreateCompose` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCompose/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

Obie te metody łączenia dwóch efekty ścieżek do utworzenia efektu złożonej ścieżki. `CreateSum` Metoda tworzy efekt ścieżki, podobne do efektów dwie ścieżki stosowane osobno, podczas gdy `CreateCompose` stosuje jeden efektu ścieżki ( `inner`) a następnie stosuje `outer` do tego.

Napotkano już sposób, w jaki `GetFillPath` metody `SKPaint` można przekonwertować jednej ścieżki na inną ścieżkę na podstawie `SKPaint` właściwości (w tym `PathEffect`), nie powinien być to *zbyt* tajemniczymi, w jaki sposób `SKPaint`obiektu można wykonać tej operacji dwukrotnie z efekty dwóch ścieżek, określone w `CreateSum` lub `CreateCompose` metody.

Jednym z zastosowań oczywiste `CreateSum` polega na zdefiniowaniu `SKPaint` obiekt, który wypełnia ścieżką efekt jedną ścieżkę i obrysy ścieżki przy użyciu inny efekt ścieżki. Jest to zaprezentowane w **koty w ramce** próbki, które wyświetla tablicę koty wewnątrz ramki z krawędziami Pofałdowany:

[![](effects-images/catsinframe-small.png "Potrójna zrzut ekranu przedstawiający stronę koty w ramce")](effects-images/catsinframe-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę koty w ramce")

[ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) Klasy rozpoczyna się od definicji kilka pól. Mogą rozpoznawać pierwsze pole z [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) klasy z [ **dane ścieżki SVG** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) artykułu. Drugi ścieżki opiera się na wykres liniowy i łuk dla wzorca St Jacques ramki:

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

`catPath` Można używać w `SKPathEffect.Create2DPath` metoda Jeśli `SKPaint` obiektu `Style` właściwość jest ustawiona na `Stroke`. Jednak jeśli `catPath` służy bezpośrednio w tym programie, następnie cały nagłówek cat zostaną wypełnione i wąsów nawet nie będzie widoczny. (Wypróbuj) Jest niezbędne do uzyskania zarys tej ścieżce i używania analizują w `SKPathEffect.Create2DPath` metody.

Konstruktor wykonuje to zadanie. Najpierw dotyczy dwóch przekształceń do `catPath` przenieść (0, 0) wskaż Centrum i skalowanie w dół, rozmiar. `GetFillPath` pobiera wszystkie konspekty konturów w `outlinedCatPath`, a obiekt jest używany w `SKPathEffect.Create2DPath` wywołania. Skalowanie uwzględnia `SKMatrix` wartość są nieco większy niż poziomy i pionowy rozmiar cat dostarczyć niewielkie bufor między kafelków, gdy czynniki tłumaczenia były pochodne nieco empirically tak, aby pełna cat jest widoczna w lewego górnego rogu ramki:

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

Następnie wywołuje konstruktor `SKPathEffect.Create1DPath` Pofałdowany ramki. Należy zauważyć, że szerokość ścieżki jest 100 pikseli, ale zaliczki 75 pikseli, aby replikowanych ścieżki jest nakładających się wokół ramki. Końcowe zestawienie Konstruktor wywołuje `SKPathEffect.CreateSum` połączyć efekty dwóch ścieżek i ustawiono wynik `SKPaint` obiektu.

Zezwala na tę pracę `PaintSurface` obsługi to bardzo proste. Tylko musi definiować prostokąt i narysuj pole tekstowe za pomocą `framePaint`:

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

Algorytmy za efekty ścieżek zawsze powodować pełną ścieżkę używane na potrzeby stykają ani wypełnianie mają być wyświetlane, która może spowodować, że niektóre elementy wizualne znajdować się poza prostokąta. `ClipRect` Wywołać przed `DrawRect` wywołanie umożliwia wizualizacje tak, aby być znacznie bardziej przejrzysty. (Wypróbuj bez przycinania!)

Jest często używa się `SKPathEffect.CreateCompose` można dodać niektórych zakłócenia w innym celu ścieżki. Bez obaw można eksperymentować na własną rękę, ale w tym miejscu jest nieco inny przykład:

**Linie przerywane kreskowania** wypełnia elipsę przy użyciu kreskowania wierszy, które są linia przerywana. Większość pracy w [ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) klasy jest wykonywane bezpośrednio w definicji pola. Te pola określają efektów kreskowania i wpływu dash. Są one definiowane jako `static` ponieważ są one następnie przywoływane w `SKPathEffect.CreateCompose` wywołania `SKPaint` definicji:

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
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

`PaintSurface` Konieczność obsługi zawiera tylko standardowe koszty oraz jedno wywołanie `DrawOval`:

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

Jak już znasz, wiersze kreskowania nie precyzyjnie ograniczone do wewnętrznego obszaru, a w tym przykładzie zawsze rozpoczynają się po lewej stronie przy użyciu całej dash:

[![](effects-images/dashedhatchlines-small.png "Potrójna zrzut ekranu przedstawiający stronę Kreskowanie wiersze kreskowania")](effects-images/dashedhatchlines-large.png#lightbox "Potrójna zrzut ekranu strony Kreskowanie kreskowania wierszy")

Teraz, gdy wiesz efekty ścieżek, z zakresu od prostego kropki i kreski, aby kombinacje dziwne, użyć swojej wyobraźni i zobacz, jakie można utworzyć.



## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
