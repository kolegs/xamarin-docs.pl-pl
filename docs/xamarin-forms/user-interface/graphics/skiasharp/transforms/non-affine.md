---
title: Affine — przekształcenia
description: Tworzenie perspektyw i efekty stożkowy (zbieżny) przy użyciu trzeciej kolumny macierzy transformacji
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 8c3d39038fbaf5ed6601102a0aa16860c7a5a7a6
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/05/2018
---
# <a name="non-affine-transforms"></a>Affine — przekształcenia

_Tworzenie perspektyw i efekty stożkowy (zbieżny) przy użyciu trzeciej kolumny macierzy transformacji_

Tłumaczenie, skalowanie, obracanie i pochylanie są sklasyfikowane jako *podobne* przekształca. Affine — przekształcenia zachować równoległych. W przypadku równoległego przed transformacji dwa wiersze pozostają równoległych po przekształceniu. Prostokąty są zawsze przekształcić parallelograms.

Jednak SkiaSharp również jest zdolny do innych niż podobne transformacje, które mają możliwość przekształcania prostokąt w dowolnym wypukłych Czworokąt:

![](non-affine-images/nonaffinetransformexample.png "Przekształcone w wypukłych Czworokąt mapy bitowej")

Wypukłych Czworokąt jest pełnego rysunku z wnętrza kąty zawsze mniej niż 180 stopni i stron, które nie między sobą.

Affine — bez przekształca wyniku, gdy trzeciego wiersza macierzy transformacji jest ustawiona na wartość inna niż 0, 0 lub 1. Pełny `SKMatrix` mnożenia jest:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

Formuły przekształcenia wynikowe są:

x "= ScaleX·x + SkewX·y + TransX

y "= SkewY·x + ScaleY·y + TransY

z` = Persp0·x + Persp1·y + Persp2

Podstawowe reguły za pomocą macierzy 3 x 3 dwuwymiarowa transformacji jest czy wszystko pozostaje na płaszczyźnie gdzie Z jest równa 1. O ile `Persp0` i `Persp1` 0, i `Persp2` jest równa 1, transformacja przeniósł współrzędnych Z poza tym płaszczyzny.

Aby przywrócić to dwuwymiarową transformację, współrzędne musi zostać przeniesiona z powrotem do tej płaszczyzny. Kolejny krok jest wymagany. X ", y", i z "wartości musi być podzielona przez z":

x" = x' / z'

y" = y' / z'

z" = z' / z' = 1

Są one nazywane *jednorodnych współrzędne* i zostały zaprojektowane przez mathematician Ferdinand Möbius sierpnia, znacznie lepiej znane dla jego topologii oddity paska Möbius.

Jeśli z "ma wartość 0, wyniki dzielenia we współrzędnych nieskończoną. W rzeczywistości jedną motywacji w Möbius związane z opracowywaniem jednorodnych współrzędne była możliwość reprezentują wartości nieskończone z liczby skończone.

Podczas wyświetlania grafiki, jednak chce się uniknąć renderowania za pomocą współrzędnych, które przetwarzają do wartości nieskończonej. Nie można renderować tych współrzędnych. Wszystko w pobliżu tych współrzędnych będą bardzo duże i prawdopodobnie nie będzie spójna.

W tym równości nie ma wartość z "staje się zero:

z` = Persp0·x + Persp1·y + Persp2

`Persp2` Komórki może być zero lub nie. Jeśli `Persp2` jest zero, a następnie z "wynosi zero (0, 0) punktu i która zazwyczaj nie jest pożądane, ponieważ ten punkt jest bardzo często dwuwymiarowa grafiki. Jeśli `Persp2` nie jest równa zero, nie są tracone z odpisy Jeśli `Persp2` ustala się na 1. Na przykład, jeśli okaże się, że `Persp2` powinien być 5, a następnie można po prostu podzielić wszystkie komórki w macierzy o 5, dzięki czemu `Persp2` równa 1, a wyniki będą takie same.

Z tego względu `Persp2` często jest ustalony na 1, który ma taką samą wartość w macierzy tożsamości.

Ogólnie rzecz biorąc `Persp0` i `Persp1` małej liczby. Na przykład rozpoczniesz z macierzą, ale zestaw `Persp0` do 0,01:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

Przekształcanie formuły są:

x` = x / (0.01·x + 1)

y' = y / (0.01·x + 1)

Teraz można używać tej transformacji renderowania 100 pikseli kwadratowego pola znajduje się w źródle. Oto, jak są przekształcane cztery rogi:

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

Jeśli x to 100, a następnie z "denominator jest 2, więc współrzędne x i y jest dwukrotnie mniejsza. Prawej stronie pola jest krótszy niż po lewej stronie:

![](non-affine-images/nonaffinetransform.png "Okno poddane-affine — przekształcenia")

`Persp` Część nazwy tych komórek odwołuje się do "perspektywy", ponieważ foreshortening sugeruje, że pole jest teraz przechylono z prawej strony, od przeglądarki.

**Perspektywy testu** strony można wypróbować wartości `Persp0` i `Pers1` można uzyskać pewne pojęcie działania. Rozsądne wartości tych komórkach macierzy są tyle mały, który `Slider` w platformy uniwersalnej systemu Windows nie może poprawnie obsługiwać je. Aby uwzględnić problem platformy uniwersalnej systemu Windows, dwa `Slider` elementów w [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) musi zostać zainicjowany do zakresu od -1 do 1:

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

Programy obsługi zdarzeń dla suwaki w [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) pliku CodeBehind dzielenia wartości przez 100, tak aby ich w zakresie między –0.01 i 0,01. Ponadto konstruktora ładuje mapy bitowej:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

`PaintSurface` Oblicza obsługi `SKMatrix` wartość o nazwie `perspectiveMatrix` na podstawie wartości tych dwóch suwaki podzielona przez 100. Jest ona połączona z dwóch tłumaczenie przekształcenia mające umieścić Centrum tej transformacji w Centrum mapy bitowej:

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

[![](non-affine-images/testperspective-small.png "Potrójna zrzut ekranu przedstawiający stronę perspektywy testu")](non-affine-images/testperspective-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę perspektywy testu")

Eksperymentując z suwaków przekonasz się, że wartości poza 0.0066 lub poniżej –0.0066 spowodować obrazu nagle i staną się fractured niespójne. Mapa bitowa transformacji jest kwadratowy 300 pikseli. Jest przekształcana względem jego Centrum, współrzędnych mapy bitowej w zakresie od –150 do 150. Odwołaj, która wartość z "jest:

z` = Persp0·x + Persp1·y + 1

Jeśli `Persp0` lub `Persp1` jest większa niż 0.0066 lub poniżej –0.0066, oznacza to, że zawsze niektórych współrzędnych mapy bitowej, która powoduje z "wartość zero. Powodujący dzielenia przez zero, a bałagan staje się renderowanie. Korzystając z systemem innym niż affine — przekształcenia chce się uniknąć, renderowania operację na współrzędne, które powodują dzielenie przez zero.

Ogólnie rzecz biorąc, nie na ustawienie `Persp0` i `Persp1` w izolacji. Należy również często można ustawić innych komórek w macierzy do osiągnięcia określonych typów innych niż affine — przekształcenia.

Jedno takie przekształcenie podobne jest *stożkowy (zbieżny) transformacji*. Ten typ innych niż affine — przekształcenia zachowuje wymiary prostokąta, ale zwężający po jednej stronie:

![](non-affine-images/tapertransform.png "Okno poddane transformacji stożkowy (zbieżny)")

[ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) Klasa wykonuje uogólniony obliczania-affine — przekształcenia, na podstawie tych parametrów:

- prostokątne rozmiar obrazu transformacji,
- Wyliczenie wskazuje stronę prostokąt zwężający,
- innego wyliczenia, która wskazuje, jak zwężający, i
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

Ta klasa jest używana w **stożkowy (zbieżny) przekształcenie** strony. Plik XAML tworzy dwa `Picker` elementy, aby wybrać wartości wyliczenia i `Slider` dotyczące wybierania ułamek stożkowy (zbieżny). [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) Łączy obsługi transformacji stożkowy (zbieżny) przy użyciu dwóch tłumaczenie transformacje dokonanie przekształcenia względem lewego górnego rogu mapy bitowej:

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

[![](non-affine-images/tapertransform-small.png "Potrójna zrzut ekranu przedstawiający stronę przekształcenie stożkowy (zbieżny)")](non-affine-images/tapertransform-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę przekształcenie stożkowy (zbieżny)")

Innym typem uogólniony-affine — przekształcenia jest obrotu 3W, które przedstawiono w kolejnym artykule [obrotów 3D](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md).

Affine — przekształcenia można przekształcić prostokąta do dowolnego wypukłych Czworokąt. Wskazuje na **macierzy affine — nie pokazuj** strony. Jest bardzo podobny do **Pokaż podobne macierzy** strony z [macierzy transformacji](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md) artykułu z tą różnicą, że ma ona czwarty `TouchPoint` obiekt do manipulowania czwarty rogu mapy bitowej:

[![](non-affine-images/shownonaffinematrix-small.png "Potrójna zrzut ekranu przedstawiający stronę macierzy affine — nie pokazuj")](non-affine-images/shownonaffinematrix-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę macierzy affine — nie pokazuj")

Tak długo, jak nie próbie kąta wewnątrz jednej narożników mapy bitowej większa niż 180 stopni, lub utworzyć dwa boki między sobą pomyślnie jest obliczana przy użyciu tej metody z transformacji [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) klasy:

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

W celu ułatwienia obliczeń ta metoda uzyskuje całkowita przekształcenia jako produkt trzech oddzielnych plików transformacji, które są w tym miejscu symbolized z strzałki wskazujące, jak te transformacje zmodyfikować cztery rogi mapy bitowej:

(0, 0) → → (0, 0) → (0, 0) (x 0, y0) (lewego górnego)

(0, H) → → (0, 1) → (0, 1) (x1 y1) (lewy dolny róg)

(W, 0) → → (1, 0) → (1, 0) (x 2, y2) (w prawym górnym)

(W, H) → → (, b) → (1, 1) (x 3, y3) (prawy dolny róg)

Współrzędne końcowego po prawej są cztery punkty skojarzone z touch cztery punkty. Są to ostateczne współrzędne narożników mapy bitowej.

Sz i reprezentują szerokość i wysokość mapy bitowej. Pierwszy transformacji (`S`) po prostu skaluje mapy bitowej do kwadratu 1 piksela. Drugi przekształcenie jest-affine — przekształcenia `N`, a trzeci affine — przekształcenia `A`. Dzięki ma podobnie jak wcześniej podobne tego affine — przekształcenia opiera się na trzy punkty [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) metody i nie może dotyczyć czwartego wiersza z (, (b) punktu.

`a` i `b` wartości są obliczane, aby trzeci przekształcenie jest podobne. Kod uzyskuje odwrotność affine — przekształcenia, a następnie użyty do mapowania prawym dolnym rogu. To punkt (, b).

Użycie innego-affine — przekształcenia jest naśladować trójwymiarowych obrazów. W kolejnym artykule [obrotów 3D](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md) poznać sposób Obróć dwuwymiarowa grafiki 3D miejsca.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
