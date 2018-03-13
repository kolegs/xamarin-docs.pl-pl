---
title: "Obrotów 3D"
description: "Affine — przekształcenia umożliwia obracanie 2D obiektów w przestrzeni 3D."
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: a959278b5de72792b23e46372b1333362bed91c8
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="3d-rotations"></a>Obrotów 3D

_Affine — przekształcenia umożliwia obracanie 2D obiektów w przestrzeni 3D._

Jeden typowych aplikacji z systemem innym niż affine — przekształcenia symuluje obrót obiektu 2D w przestrzeni 3D:

![](3d-rotation-images/3drotationsexample.png "Ciąg tekstowy obracać w przestrzeni 3D")

To zadanie polega na współpracę z trójwymiarowy obrotów i następnie wynikających z systemem innym niż — affine — `SKMatrix` przekształcania, który wykonuje te obrotów 3D.

Trudno jest opracowanie to `SKMatrix` przekształcenia działają jedynie w ramach dwóch wymiarów. Zadanie staje się znacznie łatwiejsze, gdy ta macierz 3 x 3 pochodzi z macierzy 4-na-4 używane w grafiki 3D. Obejmuje SkiaSharp [ `SKMatrix44` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.PreConcat/p/SkiaSharp.SKMatrix44/) klasy dla tego celu, ale niektóre tła w grafiki 3D jest niezbędna dla opis obrotów 3D i macierzy transformacji 4-na-4.

Trójwymiarowy układ współrzędnych dodaje trzecią osią o nazwie Z. koncepcyjnie, osi Z prostopadle do ekranu. Współrzędna punktów w przestrzeni 3D są oznaczone trzech cyfr: (x, y, z). W 3D współrzędnych używane w tym artykule, zwiększenie wartości X są po prawej stronie i zwiększa wartości Y przestaną działać, tak jak w przypadku dwóch wymiarów. Zwiększanie pozytywne wartości Z wyjść ze ekranu. Pochodzi lewego górnego rogu, tak jak w przypadku 2D grafiki. Ekran można traktować jako płaszczyzna XY z osi Z prostopadle do tej warstwy.

Jest to po lewej stronie układ współrzędnych. Jeśli punkt wskazującym dla użytkownika po lewej stronie w kierunku dodatnią X współrzędne (z prawej strony), a palca środka w kierunku Y zwiększenie współrzędne (wyłączone), następnie z thumb punktów w kierunek narastania współrzędnych Z — rozszerzenie się od ekran.

W grafiki 3D przekształceń są oparte na macierz 4 przez 4. W tym miejscu jest macierzą 4-na-4:

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

W pracy z macierzy 4-na-4, rozwiązaniem jest określenie komórki numerów wierszy i kolumn:

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

Jednak SkiaSharp `Matrix44` klasy jest nieco inny. Jedynym sposobem, aby ustawić lub pobrać wartości pojedynczych komórek w `SKMatrix44` za pomocą [ `Item` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Item/p/System.Int32/System.Int32/) indeksatora. Indeksy wierszy i kolumn są liczony od zera, a nie na podstawie jednej, a wiersze i kolumny są zamienione. Komórki M14 na powyższym diagramie jest dostępny przy użyciu indeksatora `[3, 0]` w `SKMatrix44` obiektu.

W systemie grafiki 3D 3D punkt (x, y, z) jest konwertowana na 1 na 4 macierzy pomnożenie przez macierzy transformacji 4-na-4:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Odpowiednikiem 2D przekształca tego odbywają się trzech wymiarów, transformacje 3D uznaje się, że ma miejsce w czterema wymiarami. Czwarty wymiar jest nazywany W i przestrzeni 3D założono, że istnieje w przestrzeni 4D, gdzie W współrzędne są podawane równa 1. Przekształcanie formuły są następujące:

x' = M11·x + M21·y + M31·z + M41

y' = M12·x + M22·y + M32·z + M42

z' = M13·x + M23·y + M33·z + M43

w' = M14·x + M24·y + M34·z + M44

Jest oczywiste z formuły przekształcenia który komórki `M11`, `M22`, `M33` czynnik skalowania w X, Y i Z instrukcjami, i `M41`, `M42`, i `M43` czynników tłumaczenia X, Y i Z wskazówki.

Aby przekonwertować te współrzędne przestrzeni 3D, gdzie p jest równa 1, x ", y", i z "współrzędnych są wszystkie rozdzielonych w":

x" = x' / w'

y" = y' / w'

z" = z' / w'

w"= w" / w "= 1

Ten podział według w "zawiera perspektywę w przestrzeni 3D. Jeśli w "jest równa 1, a następnie perspektywy nie występuje.

Obrotów w przestrzeni 3D może być bardzo skomplikowane, ale najprostszym obrotów są wokół osi X, Y i Z. Obrót o kąt α wokół osi X jest w tej macierzy:

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

Wartości X nie zmieniają się poddawane tej transformacji. Obrót wokół osi Y pozostawia wartości Y bez zmian:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Obrót wokół osi Z jest taki sam jak grafiki 2D:

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

Kierunek obrotu technicznego przez skrętności dla układu współrzędnych. To system dla leworęcznych, tak więc jeśli punktu stronie przycisku suwaka użytkownika po lewej stronie kierunku zwiększenie wartości osi określonego — w prawo obrót wokół osi X, dół dla obrót wokół osi Y i kierunku możesz uzyskać obrót wokół osi Z — następnie krzywej yo inne palcami wskazuje kierunek dodatnią kąty obrotu.

`SKMatrix44` został uogólniony static [ `CreateRotation` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotation/p/System.Single/System.Single/System.Single/System.Single/) i [ `CreateRotationDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotationDegrees/p/System.Single/System.Single/System.Single/System.Single/) metod, które umożliwiają określenie osi, wokół którego obrót występuje:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

Dla obrót wokół osi X należy ustawić pierwszych trzech argumentów do 1, 0, 0. W przypadku obrót wokół osi Y ustawienie dla nich wartości 0, 1, 0 i ustaw je dla obrót wokół osi Z, 0, 0, 1.

Czwartej kolumnie 4-na-4 jest perspektywy. `SKMatrix44` Nie ma żadnych metod tworzenia transformacji perspektywy, ale można utworzyć ręcznie przy użyciu następującego kodu:

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

Przyczyna Nazwa argumentu `depth` będą widoczne wkrótce. Ten kod tworzy macierzy.

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

Formuły przekształcenia spowodować następujące obliczenia w ":

w "— z = / głębokość + 1

Służy to zmniejszenie współrzędne X i Y, gdy wartości z jest mniejsza od zera (koncepcyjnie za płaszczyzna XY) i zwiększyć współrzędne X i Y dla dodatnie wartości Z. Kiedy jest równa Współrzędna `depth`, następnie w "wynosi zero, a współrzędne stają się nieskończona. Systemy trójwymiarowych obrazów są wbudowane wokół metaphor aparatu fotograficznego i `depth` wartość reprezentuje odległość aparatu ze źródła układu współrzędnych. Jeśli obiektu graficznego ma Z koordynować oznacza to `depth` jednostek ze źródła, jest koncepcyjnie dotknięcie obiektyw kamery i staje się nieskończenie wielki.

Należy pamiętać, że prawdopodobnie należy używać to `perspectiveMatrix` wartość w połączeniu z macierzy obrotu. Jeśli obiekt graphics za ma współrzędne X i Y przekracza `depth`, następnie obrót tego obiektu w przestrzeni 3D prawdopodobnie obejmuje współrzędnych Z większą niż `depth`. Należy unikać to! Podczas tworzenia `perspectiveMatrix` chcesz ustawić `depth` wystarczająco duży dla wszystkich współrzędnych w obiekcie grafiki, niezależnie od tego, jaki jest obracany wartość. To zapewni, że nigdy nie będzie żadnych dzielenie przez zero.

Łączenie obrotów 3D i perspektywy wymaga mnożenie macierzy 4-na-4 ze sobą. W tym celu `SKMatrix44` definiuje metody łączenia. Jeśli `A` i `B` są `SKMatrix44` obiektów, a następnie ustawia następujący kod równości x B:

```csharp
A.PostConcat(B);
```

W przypadku macierzy transformacji 4-na-4 w systemie grafiki 2D zastosowano 2D obiektów. Te obiekty są płaski i są zakłada się, że Z współrzędne zero. Mnożenia transformacji jest nieco łatwiejsze niż w przypadku transformacji przedstawiona wcześniej:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Tę wartość 0 z wyników w formułach przekształcania, które nie wymagają żadnych komórek w trzecim wierszu macierzy:

x' = M11·x + M21·y + M41

y' = M12·x + M22·y + M42

z' = M13·x + M23·y + M43

w' = M14·x + M24·y + M44

Ponadto z "Współrzędna jest bez znaczenia tutaj również. Po wyświetleniu obiektu 3D w systemie grafiki 2D jest zwinięty obiektowi dwuwymiarowa ignorując wartości współrzędnych Z. Przekształcanie formuły są naprawdę tylko te dwa:

x" = x' / w'

y" = y' / w'

Oznacza to, że trzeciego wiersza *i* trzeciej kolumny macierzy 4-na-4 można zignorować.

Ale jeśli jest to, dlaczego jest nawet konieczne macierzy 4-na-4 w pierwszej kolejności?

Mimo że trzeciego wiersza i trzecia kolumna 4-na-4 nie mają znaczenia dla transformacji dwuwymiarowa, trzeci wiersz i kolumnę *czy* pełnić rolę przed że w przypadku różnych `SKMatrix44` pomnożenie wartości. Załóżmy na przykład, należy pomnożyć obrót wokół osi Y o transformacja perspektywy:

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

W produkcie komórki `M14` zawiera teraz wartość perspektywy. Jeśli chcesz dotyczą tej macierzy obiekty 2D trzeciego wiersza i kolumny są eliminowane przekonwertować go do macierzy 3 na 3:

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

Teraz może służyć do przekształcania punktu 2D:

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

Przekształcanie formuły są:

x' = cos(α)·x

y' = y

z' = (sin(α)/depth)·x + 1

Teraz podzielić wszystko przez z ":

x"= cos ·x (α) / ((sin (α) / głębokość) ·x + 1)

y"= y / ((sin (α) / głębokość) ·x + 1)

Gdy 2D obiekty są obracane dodatnią kąt wokół osi Y, a następnie dodatnią wartości X recede do tła podczas ujemne wartości X pochodzą na pierwszy plan. Wartości X wydaje się, aby przenieść bliżej osi Y (która podlega wartości cosinus) jako współrzędne najbardziej od osi Y staje się mniejszy lub większy, ponieważ są one przenoszone od przeglądarki lub bliżej.

Korzystając z `SKMatrix44`, wykonywać wszystkie obrotu 3W i operacje perspektywy przez pomnożenie różnych `SKMatrix44` wartości. Następnie można wyodrębnić dwuwymiarowa macierzy 3 x 3 z 4-na-4 przy użyciu macierzy [ `Matrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Matrix/) właściwość `SKMatrix44` klasy. Ta właściwość zwraca znane `SKMatrix` wartość.

**Obrotu 3W** strony umożliwia eksperymentować obrotu 3W. [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) pliku tworzy cztery suwaki, aby ustawić obrót wokół osi X, Y i Z i do ustawiania wartości głębokości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.Rotation3DPage"
             Title="Rotation 3D">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
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
                    <Setter Property="Maximum" Value="360" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="xRotateSlider"
                Grid.Row="0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference xRotateSlider},
                              Path=Value,
                              StringFormat='X-Axis Rotation = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="yRotateSlider"
                Grid.Row="2"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference yRotateSlider},
                              Path=Value,
                              StringFormat='Y-Axis Rotation = {0:F0}'}"
               Grid.Row="3" />

        <Slider x:Name="zRotateSlider"
                Grid.Row="4"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zRotateSlider},
                              Path=Value,
                              StringFormat='Z-Axis Rotation = {0:F0}'}"
               Grid.Row="5" />

        <Slider x:Name="depthSlider"
                Grid.Row="6"
                Maximum="2500"
                Minimum="250"
                ValueChanged="OnSliderValueChanged" />

        <Label Grid.Row="7"
               Text="{Binding Source={x:Reference depthSlider},
                              Path=Value,
                              StringFormat='Depth = {0:F0}'}" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="8"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Zwróć uwagę, że `depthSlider` jest inicjowany z `Minimum` wartość 250. Oznacza to, że obiekt 2D jest obracana tutaj ma współrzędne X i Y, które są ograniczone do koło zdefiniowane przez radius 250 pikseli, wokół punktu początkowego. Współrzędna wartości mniejszej niż 250 zawsze spowoduje obracania tego obiektu w przestrzeni 3D.

[ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) ładuje plik CodeBehind mapy bitowej 300 pikseli kwadratowych:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
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

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Jeśli transformacja 3D skupia się na tej mapy bitowej, następnie współrzędne X i Y zakresie –150 i 150, gdy rogi są 212 pikseli z Centrum, więc wszystko, co znajduje się w 250 pikseli.

`PaintSurface` Tworzy program obsługi `SKMatrix44` obiektów oparte na suwakach i mnoży je ze sobą przy użyciu `PostConcat`. `SKMatrix` Wartość wyodrębniony z końcowym `SKMatrix44` obiektu jest otoczone przez tłumaczenie przekształceń do środka obrotu w Centrum ekranu:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
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

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
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

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Use 3D matrix for 3D rotations and perspective
        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, (float)xRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, (float)yRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, (float)zRotateSlider.Value));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / (float)depthSlider.Value;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the bitmap
        canvas.SetMatrix(matrix);
        float xBitmap = xCenter - bitmap.Width / 2;
        float yBitmap = yCenter - bitmap.Height / 2;
        canvas.DrawBitmap(bitmap, xBitmap, yBitmap);
    }
}
```

Gdy wypróbowanie czwarty suwaka można zauważyć ustawień głębokość różnych nie przenoś obiekt dalsze od przeglądarki, ale zamiast tego zmienić zakres efekt perspektywy:

[![](3d-rotation-images/rotation3d-small.png "Potrójna zrzut ekranu przedstawiający stronę 3D obrotu")](3d-rotation-images/rotation3d-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę obrotu 3W")

**Animowany obrotu 3W** używa również `SKMatrix44` do animowania ciągu tekstowego w przestrzeni 3D. `textPaint` Obiekt ustawiony jako pole służy do określenia granicami wartości tekstowej w Konstruktorze:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    SKCanvasView canvasView;
    float xRotationDegrees, yRotationDegrees, zRotationDegrees;
    string text = "SkiaSharp";
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        TextSize = 100,
        StrokeWidth = 3,
    };
    SKRect textBounds;

    public AnimatedRotation3DPage()
    {
        Title = "Animated Rotation 3D";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Measure the text
        textPaint.MeasureText(text, ref textBounds);
    }
    ...
}
```

`OnAppearing` Zastąpienie definiuje trzy platformy Xamarin.Forms `Animation` obiektów do animowania `xRotationDegrees`, `yRotationDegrees`, i `zRotationDegrees` pól w różnym tempie. Zwróć uwagę, że okresy te animacje są ustawione na zapisują numery — 5 sekund, 7 sekund i 11 sekund — tak ogólna kombinacja tylko jest powtarzany co 385 sekund, czyli więcej niż 10 minut:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();

        new Animation((value) => xRotationDegrees = 360 * (float)value).
            Commit(this, "xRotationAnimation", length: 5000, repeat: () => true);

        new Animation((value) => yRotationDegrees = 360 * (float)value).
            Commit(this, "yRotationAnimation", length: 7000, repeat: () => true);

        new Animation((value) =>
        {
            zRotationDegrees = 360 * (float)value;
            canvasView.InvalidateSurface();
        }).Commit(this, "zRotationAnimation", length: 11000, repeat: () => true);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        this.AbortAnimation("xRotationAnimation");
        this.AbortAnimation("yRotationAnimation");
        this.AbortAnimation("zRotationAnimation");
    }
    ...
}
```

Jak poprzedni program `PaintCanvas` tworzy program obsługi `SKMatrix44` wartości obracanie i perspektywy i mnoży je razem:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Scale so text fits
        float scale = Math.Min(info.Width / textBounds.Width,
                               info.Height / textBounds.Height);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(scale, scale));

        // Calculate composite 3D transforms
        float depth = 0.75f * scale * textBounds.Width;

        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, xRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, yRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, zRotationDegrees));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / depth;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the text
        canvas.SetMatrix(matrix);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;
        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

Ta obrotu 3W jest ujęty w kilka przekształceń 2W Aby przenieść środek obrotu centrum ekranu i skalowania rozmiar ciągu tekstowego tak, aby szerokość ekranu:

[![](3d-rotation-images/animatedrotation3d-small.png "Potrójna zrzut ekranu przedstawiający stronę 3D animowany obrotu")](3d-rotation-images/animatedrotation3d-large.png#lightbox "Potrójna zrzut ekranu strony animowany obrotu 3W")


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
