---
title: Rotacji 3D w SkiaSharp
description: W tym artykule wyjaśniono, jak za pomocą przekształcenia nieafiniczne obrócić 2D obiekty w przestrzeni 3D i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 53102b735b4b64bff4456e5e252f2342d0c4002f
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130897"
---
# <a name="3d-rotations-in-skiasharp"></a>Rotacji 3D w SkiaSharp

_Przekształcenia nieafiniczne umożliwiają obracanie 2D obiekty w przestrzeni 3D._

Jednym z typowych zastosowań przekształcenia nieafiniczne symuluje obrót 2D obiektu w przestrzeni 3D:

![](3d-rotation-images/3drotationsexample.png "Ciąg tekstowy, obracać w przestrzeni 3D")

To zadanie polega na pracy z rotacji trójwymiarowych i następnie wyprowadzanie nieafiniczne `SKMatrix` transformacji, który wykonuje te rotacji 3D.

Trudno jest opracowanie to `SKMatrix` przekształcenie działają wyłącznie w ramach dwóch wymiarów. Zadanie staje się znacznie łatwiejsze, jeśli ta macierz 3 x 3 jest tworzony na podstawie macierzy 4, 4, używane w grafiki 3D. Skiasharp — obejmuje [ `SKMatrix44` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.PreConcat/p/SkiaSharp.SKMatrix44/) klasy dla tego celu, ale niektóre tło Grafika 3D jest niezbędne do zrozumienia rotacji 3D i macierzy transformacji 4 na 4.

Trójwymiarowy współrzędnych dodaje trzeci osi, o nazwie Z. pod względem koncepcyjnym, osi Z pod kątem prostym do ekranu. Współrzędna punktów w przestrzeni 3D są oznaczone za pomocą trzech liczb: (x, y, z). W 3D współrzędnych używane w tym artykule, zwiększenie wartości X są po prawej stronie i zwiększenie wartości Y są wyłączane, podobnie jak w przypadku dwóch wymiarów. Zwiększenie wartości dodatnich Z wyjść na ekranie. Pochodzi lewym górnym rogu, podobnie jak w przypadku grafika 2D. Ekran można traktować jako płaszczyzna XY z osi Z prostopadle do tej warstwy.

Jest to po lewej stronie współrzędnych. Jeśli wskażesz palec dla Twojego po lewej stronie ekranu, w kierunku dodatnie X współrzędne (z prawej strony) i służy do koordynowania palca środka w kierunku rosnących Y (w dół), następnie swoje thumb punktów w kierunku rosnących współrzędne Z — rozszerzanie się z ekran.

Grafika 3D przekształcenia zależą macierzy 4 na 4. W tym miejscu jest macierzą 4-na-4:

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

W pracy z macierzy 4, 4, jest wygodne zidentyfikować komórki z ich numery wierszy i kolumn:

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

Jednak SkiaSharp `Matrix44` klasy jest nieco inny. Jedynym sposobem, aby ustawić lub pobrać wartości do pojedynczych komórek `SKMatrix44` polega na użyciu [ `Item` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Item/p/System.Int32/System.Int32/) indeksatora. Wskaźniki wierszy i kolumn jest liczony od zera, a nie na podstawie jednego, a wiersze i kolumny zostały zamienione. Komórka M14 na powyższym diagramie odbywa się za pomocą indeksatora `[3, 0]` w `SKMatrix44` obiektu.

W systemie Grafika 3D punktu 3W (x, y, z) jest konwertowana na 1 na 4 macierzy mnożenia wartości przez macierzy transformacji 4-na-4:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Odpowiednikiem 2D przekształca, jakie mają mieć miejsce w trzech wymiarach, przekształcenia 3D są uznawane za miejsce w czterech wymiarów. Czwarty wymiar jest nazywana W, a przestrzeni 3D założono, że istnieje w przestrzeni 4D, gdzie W współrzędne są podawane równa 1. Formuły przekształcenia są następujące:

x' = M11·x + M21·y + M31·z + M41

y' = M12·x + M22·y + M32·z + M42

z' = M13·x + M23·y + M33·z + M43

w' = M14·x + M24·y + M34·z + M44

Jest oczywiste, z poziomu formuł transformacji, komórki `M11`, `M22`, `M33` czynnik skalowania w X, Y i Z instrukcjami i `M41`, `M42`, i `M43` czynników tłumaczenia X, Y i Z wskazówki.

Aby przekonwertować te współrzędne 3D miejsce, gdzie W jest równa 1, x ", y", i z "współrzędnych są wszystkie podzielone według w":

x"= x" / w "

y"= y" / w "

z"= z" / w "

w"= w" / w "= 1

Tym dzielenie przez odczytu zapisu "zawiera perspektywę w przestrzeni 3D. Jeśli w "jest równa 1, a następnie perspektywy nie występuje.

Możliwość wymiany w przestrzeni 3D mogą być całkiem złożone, ale najprostszy obroty są wokół osi X, Y i Z. Obrót wokół osi X, kąt α jest tej macierzy:

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

Wartości X pozostają takie same poddawane tej transformacji. Obrót wokół osi Y pozostawia wartości Y bez zmian:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Obrót wokół osi Z, jest taka sama, jak grafika 2D:

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

Kierunek obrotu jest implikowany przez skrętności dla układu współrzędnych. To leworęczni systemu, więc jeśli zmienisz ręki na zwiększenie wartości określonej osi po lewej stronie przycisku suwaka — do prawej obrót wokół osi X, dół, aby obrót wokół osi Y i kierunku możesz uzyskać obrót wokół osi Z — następnie krzywej yo inne palcami wskazuje kierunek dodatnią kąty obrotu.

`SKMatrix44` został uogólniony statyczne [ `CreateRotation` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotation/p/System.Single/System.Single/System.Single/System.Single/) i [ `CreateRotationDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotationDegrees/p/System.Single/System.Single/System.Single/System.Single/) metod, które pozwalają na określenie osi, wokół którego występuje obrót:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

W przypadku rotacji wokół osi X należy ustawić pierwszych trzech argumentów do 1, 0, 0. Obrót wokół osi Y ustaw je na 0, 1, 0 i ustawić je dla obrotu wokół osi Z, 0, 0, 1.

Czwarta kolumna 4 na przez 4 to dla perspektywy. `SKMatrix44` Nie ma żadnych metod tworzenia przekształcenia perspektywy, ale możesz utworzyć je samodzielnie, używając następującego kodu:

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

Przyczyna nazwę argumentu `depth` będzie widoczne wkrótce. Ten kod tworzy macierzy:

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

Formuły przekształcenie spowodować następujące obliczenia w ":

w "= — z / głębokość + 1

Służy to zmniejszenie współrzędne X i Y, gdy wartości z jest mniejsza od zera (koncepcyjnie poza płaszczyzną XY) oraz zwiększenie współrzędne X i Y dla wartości dodatnich elementu Z. Kiedy jest równa Współrzędna `depth`, następnie w "wynosi zero, a współrzędne stają się nieskończone. Systemy trójwymiarowej grafiki są zbudowane wokół metaphor kamery, a `depth` wartość reprezentuje odległość aparatu ze źródła w układzie współrzędnych. Jeśli obiekt graficzny ma Z koordynować to znaczy `depth` jednostki ze źródła, koncepcyjnie zachodzi obiektywu kamery i staje się nieskończenie duży.

Należy pamiętać, że należy prawdopodobnie używać to `perspectiveMatrix` wartość w połączeniu z macierzy obrotu. Jeśli obiekt graficzny, za zawiera współrzędne X lub Y przekracza `depth`, może obejmować Z współrzędne większe niż jest obrót tego obiektu w przestrzeni 3D `depth`. Należy unikać to! Podczas tworzenia `perspectiveMatrix` chcesz ustawić `depth` wartość wystarczająco duży dla wszystkich współrzędnych w obiekcie grafiki, niezależnie od tego, jaki jest obracany. Zapewnia to, że nigdy nie jest żadnych dzielenie przez zero.

Łączenie rotacji 3D i perspektywy wymaga mnożenie macierzy 4 na 4 ze sobą. W tym celu `SKMatrix44` definiuje metody łączenia. Jeśli `A` i `B` są `SKMatrix44` obiektów, a następnie poniższy kod ustawia równe × B:

```csharp
A.PostConcat(B);
```

Macierzy transformacji 4 na 4 jest używany w systemie grafika 2D, jest stosowana do obiektów 2D. Te obiekty są płaskie i są zakłada się, że masz współrzędne Z zerową. Mnożenie transformacji jest nieco prostsze niż przekształcenie przedstawionej wcześniej:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Tę wartość 0 z wyników w formułach przekształcenia, które nie wymagają żadnych komórek w trzecim rzędzie macierzy:

x' = M11·x + M21·y + M41

y' = M12·x + M22·y + M42

z' = M13·x + M23·y + M43

w "= M14·x + M24·y + M44

Ponadto z "Współrzędna jest bez znaczenia tutaj również. Po wyświetleniu w systemie grafika 2D obiektu 3D, ignorując wartości Współrzędna Z jest zwinięta jak dwuwymiarowy obiekt. Formuły przekształcenia są tylko te dwa:

x"= x" / w "

y"= y" / w "

Oznacza to, że trzecim rzędzie *i* trzecia kolumna macierzy 4, 4, można je zignorować.

Ale jeśli jest to, dlaczego jest nawet konieczne macierzy 4 na 4 w pierwszej kolejności?

Mimo że trzeciego wiersza i trzecia kolumna 4 na przez 4 są nieodpowiednie dla przekształceń dwuwymiarowy, trzeci wierszy i kolumn *czy* pełniące rolę przed że w przypadku różnych `SKMatrix44` pomnożenie wartości. Załóżmy na przykład, należy pomnożyć obrót wokół osi Y z zadaniami transformacji perspektywy:

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

W produkcie komórki `M14` zawiera teraz wartość perspektywy. Jeśli dotyczą tej macierzy obiekty 2D trzeciego wiersza i kolumny są eliminowane i przekształcać je do macierzy 3 na 3:

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

Teraz może służyć do przekształcania punkt 2D:

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

Formuły przekształcenia są:

x' = cos(α)·x

y' = y

z "= (sin (α) / głębokość) ·x + 1

Teraz dzielnikiem wszystkie elementy z':

x"= cos ·x (α) / ((sin (α) / głębokość) ·x + 1)

y"= y / ((sin (α) / głębokość) ·x + 1)

Gdy 2D obiekty są obracane dodatnią kąt wokół osi Y, a następnie dodatnie wartości X recede tło podczas ujemnych wartości X Przyjdź na pierwszym planie. Wartości X wydawać się, aby Zbliż do osi Y (która jest regulowane przez wartość funkcji cosinus) jako najbardziej od osi Y staje się mniejszy lub większy od przeglądarki przechodzące lub bliżej.

Korzystając z `SKMatrix44`, wykonaj obrotu 3D i operacje perspektywy przez pomnożenie różnych `SKMatrix44` wartości. Następnie można wyodrębnić dwuwymiarowej macierzy 3 x 3 z 4-na-4 macierzy przy użyciu [ `Matrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Matrix/) właściwość `SKMatrix44` klasy. Ta właściwość zwraca powszechnie znane `SKMatrix` wartość.

**Obrót 3D** stronie pozwala na eksperymentowanie z obrotu 3W. [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) pliku tworzy cztery suwaki, aby ustawić obrót wokół osi X, Y i Z i do ustawiania wartości głębi:

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

Należy zauważyć, że `depthSlider` jest inicjowany za pomocą `Minimum` wartości 250. Oznacza to, że obiekt 2D jest obracana tutaj ma współrzędne X i Y, które są ograniczone do koło z definicją w promieniu 250 pikseli, wokół źródła. Wartości współrzędnych mniej niż 250 zawsze spowoduje żadnych obrotu tego obiektu w przestrzeni 3D.

[ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) pliku związanego z kodem ładuje mapy bitowej o rozmiarze 300 pikseli kwadrat:

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
        {
            bitmap = SKBitmap.Decode(stream);
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

Jeśli przekształcenia 3D skupia się na tej mapy bitowej, następnie współrzędne X i Y zakresie –150 i 150, rogi są 212 pikseli z Centrum, więc wszystko, co znajduje się w piksel 250.

`PaintSurface` Tworzy program obsługi `SKMatrix44` obiektów oparty na fragmentatorach i mnoży je ze sobą przy użyciu `PostConcat`. `SKMatrix` Wartość wyodrębniony z końcowym `SKMatrix44` obiektu jest otoczone przez tłumaczenie przekształceń do środka obrotu w środku ekranu:

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
        {
            bitmap = SKBitmap.Decode(stream);
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

Podczas eksperymentowania za pomocą suwaka czwarty widać nie przenoś obiektu dalsze od obserwatora ustawień głębokość różnych, ale zamiast tego zmienić zakres efekt perspektywy:

[![](3d-rotation-images/rotation3d-small.png "Potrójna zrzut ekranu przedstawiający stronę obrót 3D")](3d-rotation-images/rotation3d-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę obrót 3D")

**Animowane obrót 3D** używa również `SKMatrix44` animować ciąg tekstowy w przestrzeni 3D. `textPaint` Obiektu ustawione jako pole służy do określenia granicami wartości tekstowej w Konstruktorze:

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

`OnAppearing` Zastąpienie definiuje trzy Xamarin.Forms `Animation` obiektów, aby animować `xRotationDegrees`, `yRotationDegrees`, i `zRotationDegrees` pól w różnym tempie. Należy zauważyć, że okresy te animacji są ustawione na zainicjowanie numery — 5 sekund, 7 sekundach, a 11 sekundach — więc ogólną kombinacji tylko powtarza się co 385 sekund, czyli więcej niż 10 minut:

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

Jak poprzedni program `PaintCanvas` tworzy program obsługi `SKMatrix44` wartości rotacji i perspektywy i mnoży je ze sobą:

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

Ta obrotu 3W jest ujęty w kilka przekształceń 2D, aby przenieść środek obrotu na środku ekranu, a także skalowanie rozmiaru ciąg tekstowy, tak aby był szerokość ekranu:

[![](3d-rotation-images/animatedrotation3d-small.png "Potrójna zrzut ekranu przedstawiający stronę 3D animowane obrotu")](3d-rotation-images/animatedrotation3d-large.png#lightbox "Potrójna zrzut ekranu strony animowane obrót 3D")


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
