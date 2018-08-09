---
title: Przekształcenia Nieafiniczne
description: W tym artykule opisano sposób tworzenia perspektyw i efekty stożkowy (zbieżny) przy użyciu trzecia kolumna macierzy transformacji i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 13f2a1160d012a6b7720bd84340a1cdd0f991535
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615655"
---
# <a name="non-affine-transforms"></a>Przekształcenia Nieafiniczne

_Tworzenie perspektyw i efekty stożkowy (zbieżny) przy użyciu trzecia kolumna macierzy transformacji_

Przesunięcia, skalowania, obrotu i pochylanie są klasyfikowane jako *przekształceniem afinicznym* przekształcenia. Affine — przekształcenia zachować równoległych. W przypadku równoległego przed Przekształcanie dwóch wierszy pozostają równoległego po transformacji. Prostokąty zawsze są przekształcane na parallelograms.

Jednak SkiaSharp również jest zdolny do przekształcenia nieafiniczne, które mają możliwość przekształcania prostokąt w dowolnym wypukłe kwadratowe:

![](non-affine-images/nonaffinetransformexample.png "Mapy bitowej przekształcane wypukłe kwadratowe")

Wypukłe kwadratowe jest rysunku pełnego z wnętrza kąty zawsze mniejsza niż 180 stopni i stron, które nie przekraczają siebie nawzajem.

Affine — inne niż przekształca wynik, podczas trzeciego wiersza macierzy transformacji jest ustawiona na wartość inna niż 0, 0 i 1. Pełny `SKMatrix` mnożenie jest:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

Formuły przekształcenie wynikowe są:

x "= ScaleX·x + SkewX·y + TransX

y "= SkewY·x + ScaleY·y + TransY

z` = Persp0·x + Persp1·y + Persp2

Podstawowe reguły za pomocą macierzy 3 x 3 dwuwymiarową transformacji jest, że wszystko, co pozostanie na płaszczyźnie gdzie Z jest równa 1. Chyba że `Persp0` i `Persp1` to 0, a `Persp2` jest równa 1, przekształcenia przeniósł współrzędne Z poza tym płaszczyzny.

Aby przywrócić to dwuwymiarowa transformacja, współrzędne musi przeniesiony z powrotem do tej warstwy. Kolejny krok jest wymagany. X ", y", i z "wartości muszą zostać podzielone według z":

x" = x' / z'

y" = y' / z'

z" = z' / z' = 1

Są to znane jako *jednorodnych współrzędne* i zostały opracowane przez mathematician Ferdinand Möbius sierpnia, znacznie lepiej znane pod nazwą dla jego topologii oddity paska Möbius.

Jeśli z "ma wartość 0, wyniki dzielenia we współrzędnych nieskończone. W rzeczywistości jeden z zresztą Möbius firmy do tworzenia jednorodnych współrzędne był zdolność do reprezentowania wartości nieskończonej z liczby skończone.

Podczas wyświetlania grafiki, jednak chcesz uniknąć renderowania za pomocą współrzędnych, które przetwarzają do wartości nieskończonej. Nie można renderować tych współrzędnych. Wszystko, czego spowodowanych antyaliasingiem w pobliżu tych współrzędnych będą bardzo duże i prawdopodobnie nie będzie spójny.

W tym równania, nie ma wartość z "staje się zerem:

z` = Persp0·x + Persp1·y + Persp2

`Persp2` Komórki może być zero lub nie. Jeśli `Persp2` jest zero, a następnie z "ma wartość zero dla punktu (0, 0) i która zazwyczaj nie jest pożądane, ponieważ ten punkt jest bardzo częsty w grafiki dwuwymiarowej. Jeśli `Persp2` nie jest równa zero, jeśli występuje bez utraty ogólności `Persp2` jest ustalony na 1. Na przykład, jeśli stwierdzisz, że `Persp2` powinien być 5, a następnie można po prostu podzielić wszystkie komórki w macierzy, 5, co sprawia, że `Persp2` wynosi 1, a wynik będzie taki sam.

Z tego względu `Persp2` często jest ustalony na 1, która jest taka sama wartość w macierzy tożsamości.

Ogólnie rzecz biorąc `Persp0` i `Persp1` są małymi liczbami. Załóżmy na przykład, rozpoczynać macierz tożsamości, ale zestaw `Persp0` do 0,01:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

Formuły przekształcenia są:

x` = x / (0.01·x + 1)

y "= y / (0.01·x + 1)

Teraz za pomocą tej transformacji renderowania 100 pikseli kwadratowego pola umieszczane w źródle. Poniżej przedstawiono, jak są przekształcane dowiedzą:

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

Jeśli x to 100, a następnie z "mianownik wynosi 2, dzięki czemu współrzędne x i y są wydajnie filtrach. Po prawej stronie pola staje się mniej niż po lewej stronie:

![](non-affine-images/nonaffinetransform.png "Pole poddane przekształcenia nieafiniczne")

`Persp` Część z tych nazw komórki odwołuje się do "w perspektywie", ponieważ foreshortening sugeruje, że pole jest teraz pochylony z po prawej stronie od przeglądarki.

**Punktu widzenia testu** strona pozwala na eksperymentowanie z wartościami `Persp0` i `Pers1` Aby uzyskać pewne pojęcie dla działania. Rozsądne wartości tych komórek macierzy są tak mały, który `Slider` platformie Universal Windows nie może poprawnie obsłużyć je. Aby uwzględnić problem platformy uniwersalnej systemu Windows, dwa `Slider` elementów w [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) musi zostać zainicjowany do zakresu od – 1 do 1:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.TestPerspectivePage"
             Title="Test Perpsective">
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
                    <Setter Property="Minimum" Value="-1" />
                    <Setter Property="Maximum" Value="1" />
                    <Setter Property="Margin" Value="20, 0" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="persp0Slider"
                Grid.Row="0"
                ValueChanged="OnPersp0SliderValueChanged" />

        <Label x:Name="persp0Label"
               Text="Persp0 = 0.0000"
               Grid.Row="1" />

        <Slider x:Name="persp1Slider"
                Grid.Row="2"
                ValueChanged="OnPersp1SliderValueChanged" />

        <Label x:Name="persp1Label"
               Text="Persp1 = 0.0000"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Programy obsługi zdarzeń dla suwaki w [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) pliku związanego z kodem dzielnikiem wartości 100 tak, aby ich w zakresie od –0.01 i 0,01. Ponadto Konstruktor ładuje mapy bitowej:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    SKBitmap bitmap;

    public TestPerspectivePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnPersp0SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp0Label.Text = String.Format("Persp0 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }

    void OnPersp1SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp1Label.Text = String.Format("Persp1 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }
    ...
}
```

`PaintSurface` Oblicza obsługi `SKMatrix` wartość o nazwie `perspectiveMatrix` na podstawie wartości tych dwóch suwaki podzielona przez 100. Jest ona połączona z dwóch tłumaczenie przekształcenia mające umieścić środek przekształcenia w środku mapy bitowej:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Calculate perspective matrix
        SKMatrix perspectiveMatrix = SKMatrix.MakeIdentity();
        perspectiveMatrix.Persp0 = (float)persp0Slider.Value / 100;
        perspectiveMatrix.Persp1 = (float)persp1Slider.Value / 100;

        // Center of screen
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);
        SKMatrix.PostConcat(ref matrix, perspectiveMatrix);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(xCenter, yCenter));

        // Coordinates to center bitmap on canvas
        float x = xCenter - bitmap.Width / 2;
        float y = yCenter - bitmap.Height / 2;

        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

Poniżej przedstawiono niektóre przykładowe obrazy:

[![](non-affine-images/testperspective-small.png "Potrójna zrzut ekranu przedstawiający stronę z punktu widzenia testu")](non-affine-images/testperspective-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę z punktu widzenia testu")

Jak wypróbowanie suwaki przekonasz się, że wartości poza 0.0066 lub poniżej –0.0066 spowodować obrazu nagle stanie się fractured i niespójne. Mapa bitowa przekształcanego jest kwadratowy 300 pikseli. Jest przekształcana względem środka, więc współrzędne mapy bitowej do zakresu od –150 do 150. Pamiętaj, że wartość z "jest:

z` = Persp0·x + Persp1·y + 1

Jeśli `Persp0` lub `Persp1` jest większa niż 0.0066 lub poniżej –0.0066, następnie jest zawsze niektóre współrzędnych mapy bitowej, który skutkuje z "o wartości zero. Dzięki któremu dzielenie przez zero i renderowanie staje się zakorkowane. Korzystając z przekształcenia nieafiniczne, chcesz uniknąć renderowania z współrzędne, które powodują dzielenie przez zero.

Ogólnie rzecz biorąc, nie będzie można ustawienie `Persp0` i `Persp1` w izolacji. Należy również często można ustawić inne komórki w macierzy, aby osiągnąć niektórych rodzajów przekształcenia nieafiniczne.

Jest jeden taki przekształcenia nieafiniczne *stożkowy (zbieżny) przekształcenie*. Ten typ przekształcenia nieafiniczne zachowuje wymiary prostokąta, ale zwężający z jednej strony:

![](non-affine-images/tapertransform.png "Pole poddane przekształcenie stożkowy (zbieżny)")

[ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) Klasy wykonuje obliczenie uogólnionego przekształcenia nieafiniczne na podstawie tych parametrów:

- prostokątne rozmiar obrazu, przekształcanego,
- wskazuje stronę prostokąt, który zwężający, wyliczenie
- inny wyliczenia, która wskazuje, jak zwężający, i
- zakres zbieżności.

Oto kod:

```csharp
enum TaperSide { Left, Top, Right, Bottom }

enum TaperCorner { LeftOrTop, RightOrBottom, Both }

static class TaperTransform
{
    public static SKMatrix Make(SKSize size, TaperSide taperSide, TaperCorner taperCorner, float taperFraction)
    {
        SKMatrix matrix = SKMatrix.MakeIdentity();

        switch (taperSide)
        {
            case TaperSide.Left:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp0 = (taperFraction - 1) / size.Width;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Top:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp1 = (taperFraction - 1) / size.Height;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Right:
                matrix.ScaleX = 1 / taperFraction;
                matrix.Persp0 = (1 - taperFraction) / (size.Width * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        break;
                }
                break;

            case TaperSide.Bottom:
                matrix.ScaleY = 1 / taperFraction;
                matrix.Persp1 = (1 - taperFraction) / (size.Height * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        break;
                }
                break;
        }
        return matrix;
    }
}
```

Ta klasa jest używana w **stożkowy (zbieżny) Przekształcanie** strony. Plik XAML są tworzone wystąpienia dwóch `Picker` elementy, aby wybrać wartości wyliczenia i `Slider` dotyczące wybierania ułamek stożkowy (zbieżny). [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) Obsługi łączy stożkowy (zbieżny) przekształcenia przy użyciu dwóch tłumaczenie przekształceń, które umożliwiają przekształcanie względem lewego górnego rogu mapy bitowej:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    TaperSide taperSide = (TaperSide)taperSidePicker.SelectedIndex;
    TaperCorner taperCorner = (TaperCorner)taperCornerPicker.SelectedIndex;
    float taperFraction = (float)taperFractionSlider.Value;

    SKMatrix taperMatrix =
        TaperTransform.Make(new SKSize(bitmap.Width, bitmap.Height),
                            taperSide, taperCorner, taperFraction);

    // Display the matrix in the lower-right corner
    SKSize matrixSize = matrixDisplay.Measure(taperMatrix);

    matrixDisplay.Paint(canvas, taperMatrix,
        new SKPoint(info.Width - matrixSize.Width,
                    info.Height - matrixSize.Height));

    // Center bitmap on canvas
    float x = (info.Width - bitmap.Width) / 2;
    float y = (info.Height - bitmap.Height) / 2;

    SKMatrix matrix = SKMatrix.MakeTranslation(-x, -y);
    SKMatrix.PostConcat(ref matrix, taperMatrix);
    SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(x, y));

    canvas.SetMatrix(matrix);
    canvas.DrawBitmap(bitmap, x, y);
}
```

Oto kilka przykładów:

[![](non-affine-images/tapertransform-small.png "Potrójna zrzut ekranu przedstawiający stronę przekształcenia stożkowy (zbieżny)")](non-affine-images/tapertransform-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę przekształcenia stożkowy (zbieżny)")

Innym typem uogólnionego przekształcenia nieafiniczne jest obrotu 3W, które przedstawiono w kolejnym artykule [rotacji 3D](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md).

Przekształcenia nieafiniczne można przekształcić prostokąta do dowolnego wypukłe kwadratowe. Wskazuje na **Pokaż affine — inne niż macierzy** strony. Jest bardzo podobne do **Pokaż przekształceniem Afinicznym macierzy** strony [przekształcenia macierzy](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md) artykułu z tą różnicą, że ma ona czwarty `TouchPoint` obiekt do manipulowania czwarty rogu mapy bitowej:

[![](non-affine-images/shownonaffinematrix-small.png "Potrójna zrzut ekranu przedstawiający stronę pokazać affine — inne niż macierzy")](non-affine-images/shownonaffinematrix-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę pokazać affine — inne niż macierzy")

Tak długo, jak nie spróbujesz kąt wnętrza jednej z jej narożników mapy bitowej większa niż 180 stopni lub wprowadź dwa boki między sobą pomyślnie obliczana przekształcenia przy użyciu tej metody z [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) klasy:

```csharp
static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL, SKPoint ptLR)
{
    // Scale transform
    SKMatrix S = SKMatrix.MakeScale(1 / size.Width, 1 / size.Height);

    // Affine transform
    SKMatrix A = new SKMatrix
    {
        ScaleX = ptUR.X - ptUL.X,
        SkewY = ptUR.Y - ptUL.Y,
        SkewX = ptLL.X - ptUL.X,
        ScaleY = ptLL.Y - ptUL.Y,
        TransX = ptUL.X,
        TransY = ptUL.Y,
        Persp2 = 1
    };

    // Non-Affine transform
    SKMatrix inverseA;
    A.TryInvert(out inverseA);
    SKPoint abPoint = inverseA.MapPoint(ptLR);
    float a = abPoint.X;
    float b = abPoint.Y;

    float scaleX = a / (a + b - 1);
    float scaleY = b / (a + b - 1);

    SKMatrix N = new SKMatrix
    {
        ScaleX = scaleX,
        ScaleY = scaleY,
        Persp0 = scaleX - 1,
        Persp1 = scaleY - 1,
        Persp2 = 1
    };

    // Multiply S * N * A
    SKMatrix result = SKMatrix.MakeIdentity();
    SKMatrix.PostConcat(ref result, S);
    SKMatrix.PostConcat(ref result, N);
    SKMatrix.PostConcat(ref result, A);

    return result;
}
```

W celu ułatwienia obliczeń ta metoda uzyskuje łączna liczba transformacji jest produktem w trzech oddzielnych przekształceń, które są w tym miejscu symbolized przy użyciu strzałek pokazujący, jak te przekształcenia zmodyfikować dowiedzą o mapy bitowej:

(0, 0) → → (0, 0) → (0, 0) (x 0, y0) (w lewym górnym)

(0, H) → → (0, 1) → (0, 1) (x1 y1) (lewy dolny róg)

(W, 0) → → (1, 0) → (1, 0) (x 2, y2) (prawym górnym rogu)

(W, H) → → (, b) → (1, 1) (x 3, y3) (prawy dolny róg)

Współrzędne końcowe po prawej stronie są cztery punkty skojarzone z punkty dotykowe cztery. Są to ostateczne współrzędne narożników mapy bitowej.

Sz i reprezentują szerokość i wysokość mapy bitowej. Pierwszy transformacji (`S`) po prostu skaluje mapy bitowej do 1-pikselowe kwadrat. Drugi przekształcenie jest przekształcenia nieafiniczne `N`, a trzecia będzie affine — przekształcenia `A`. Tego affine — przekształcenia opiera się na trzy punkty, dzięki czemu ma tak jak wcześniej affine — [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) metody i nie obejmują czwarty wiersz mający (, (b) punkt.

`a` i `b` wartości są obliczane tak, aby trzeci przekształcenie przekształceniem afinicznym. Kod uzyskuje odwrotność affine — przekształcenia, a następnie użyty do mapowania w prawym dolnym rogu. To punkt (, b).

Innym zastosowaniem przekształcenia nieafiniczne jest do naśladowania trójwymiarowej grafiki. W następnym artykule [rotacji 3D](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md) Zobacz jak wymienić grafiki dwuwymiarowej w przestrzeni 3D.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
