---
title: Manipulacje za pomocą dotyku
description: W tym artykule wyjaśniono, jak wdrożyć przeciągając touch, uszczypnięć i obrót za pomocą przekształcenia macierzowe i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/03/2018
ms.openlocfilehash: e2c1529980681ed1013c53343c2d077297352b95
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615395"
---
# <a name="touch-manipulations"></a>Manipulacje za pomocą dotyku

_Użyj macierzy przekształcenia do zaimplementowania przeciągając touch, uszczypnięć i obrót_

W środowiskach Wielodotyk, takich jak te na urządzeniach przenośnych użytkownicy często używają swoich palców do manipulowania obiektami na ekranie. Typowe gestów takich jak przeciągnij jeden finger i uszczypnięcia dwóch palców można przenieść i skalowanie obiektów lub nawet przenosić je. Następujących gestów są zazwyczaj implementowane za pomocą macierzy transformacji, a w tym artykule dowiesz się, jak to zrobić.

![](touch-images/touchmanipulationsexample.png "Mapy bitowej poddane translacji, skalowanie i obrót")

## <a name="manipulating-one-bitmap"></a>Manipulowanie jeden mapy bitowej

**Touch manipulowania** strony pokazuje manipulacje dotykowej w postaci bitmapy.
W tym przykładzie użyto efektu touch śledzenia znajdujące się w artykule [wywoływanie zdarzenia z efektów](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Kilka innych plików zapewnia pomoc techniczną dla **Touch manipulowania** strony. Pierwsza to [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) wyliczenia, co oznacza różne rodzaje manipulowania touch implementowana przez kod, będą wyświetlane:

```csharp
enum TouchManipulationMode
{
    None,
    PanOnly,
    IsotropicScale,     // includes panning
    AnisotropicScale,   // includes panning
    ScaleRotate,        // implies isotropic scaling
    ScaleDualRotate     // adds one-finger rotation
}
```

`PanOnly` jest przeciągnij palcem jeden, który jest implementowany przy użyciu translacji. Kolejne opcje także obejmują panoramowaniem, ale obejmują dwóch palców: `IsotropicScale` jest operacją uszczypnięcia, powstałego w obiekcie równie skalowanie w poziomie i pionie kierunkach. `AnisotropicScale` Umożliwia skalowanie nierówne.

`ScaleRotate` Opcja jest przeznaczona dla dwóch palców skalowanie i obrót. Skalowanie jest izotropowego. Implementowanie obrotu dwóch palców ze skalowaniem anizotropowego jest problematyczne, ponieważ przepływy finger są zasadniczo takie same.

`ScaleDualRotate` Opcja dodaje obrotu co finger. Przeciągany obiekt pojedynczej linii papilarnych przeciągnie obiektu, jest najpierw obracany wokół środka tak, aby Centrum obiektu linii z przeciągania wektora.

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) plik zawiera `Picker` z elementami członkowskimi `TouchManipulationMode` wyliczenia:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.TouchManipulationPage"
             Title="Touch Manipulation">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker Title="Touch Mode"
                Grid.Row="0"
                SelectedIndexChanged="OnTouchModePickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>None</x:String>
                <x:String>PanOnly</x:String>
                <x:String>IsotropicScale</x:String>
                <x:String>AnisotropicScale</x:String>
                <x:String>ScaleRotate</x:String>
                <x:String>ScaleDualRotate</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                4
            </Picker.SelectedIndex>
        </Picker>

        <Grid BackgroundColor="White"
              Grid.Row="1">

            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>
    </Grid>
</ContentPage>
```

W dolnej jest `SKCanvasView` i `TouchEffect` dołączone do pojedynczych komórek `Grid` która ją obejmuje.

[ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) ma pliku CodeBehind `bitmap` pole, ale nie jest typu `SKBitmap`. Typ jest `TouchManipulationBitmap` (klasa pojawi się wkrótce):

```csharp
public partial class TouchManipulationPage : ContentPage
{
    TouchManipulationBitmap bitmap;
    ...

    public TouchManipulationPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.MountainClimbers.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            SKBitmap bitmap = SKBitmap.Decode(stream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

Tworzy konstruktora `TouchManipulationBitmap` obiektu i przekazanie do konstruktora `SKBitmap` uzyskane z zasobu osadzonego. Konstruktor stwierdza, ustawiając `Mode` właściwość `TouchManager` właściwość `TouchManipulationBitmap` obiektu do elementu członkowskiego `TouchManipulationMode` wyliczenia.

`SelectedIndexChanged` Obsługa `Picker` spowoduje także ustawienie to `Mode` właściwości:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            TouchManipulationMode mode;
            Enum.TryParse(picker.Items[picker.SelectedIndex], out mode);
            bitmap.TouchManager.Mode = mode;
        }
    }
    ...
}
```

`TouchAction` Program obsługi `TouchEffect` tworzone w wywołaniach pliku XAML dwie metody w `TouchManipulationBitmap` o nazwie `HitTest` i `ProcessTouchEvent`:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    List<long> touchIds = new List<long>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (bitmap.HitTest(point))
                {
                    touchIds.Add(args.Id);
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    break;
                }
                break;

            case TouchActionType.Moved:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    touchIds.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Jeśli `HitTest` metoda zwraca `true` &mdash; co oznacza, że palcem ma dotknięciu ekranu w ramach obszar zajmowany przez mapę bitową &mdash; użycia funkcji touch ID jest dodawana do `TouchIds` kolekcji. Ten identyfikator reprezentuje sekwencję zdarzeń touch tej linii papilarnych, dopóki finger wind z ekranu. Jeśli wiele palców touch mapy bitowej, a następnie `touchIds` kolekcja zawiera użycia funkcji touch ID dla poszczególnych finger.

`TouchAction` Również wywołuje procedurę obsługi `ProcessTouchEvent` klasy w `TouchManipulationBitmap`. To jest, gdy niektóre (ale nie wszystkie) dotykowego prawdziwe przetwarzanie odbywa się.

[ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) Klasy jest klasą otoki dla `SKBitmap` zawierający kod do renderowania mapę bitową i przetwarzania zdarzeń touch. Działa w połączeniu z bardziej powszechny kodu w `TouchManipulationManager` klasy (która pojawi się wkrótce).

`TouchManipulationBitmap` Zapisuje Konstruktor `SKBitmap` i tworzy dwie właściwości `TouchManager` właściwości typu `TouchManipulationManager` i `Matrix` właściwości typu `SKMatrix`:

```csharp
class TouchManipulationBitmap
{
    SKBitmap bitmap;
    ...

    public TouchManipulationBitmap(SKBitmap bitmap)
    {
        this.bitmap = bitmap;
        Matrix = SKMatrix.MakeIdentity();

        TouchManager = new TouchManipulationManager
        {
            Mode = TouchManipulationMode.ScaleRotate
        };
    }

    public TouchManipulationManager TouchManager { set; get; }

    public SKMatrix Matrix { set; get; }
    ...
}
```

To `Matrix` właściwość jest skumulowana transformacji, wynikające z całą aktywność touch. Jak można zauważyć, każde zdarzenie touch zostanie rozwiązany w macierzy, który następnie jest połączona z `SKMatrix` wartość przechowywana `Matrix` właściwości.

`TouchManipulationBitmap` Obiektu rysuje samą siebie jego `Paint` metody. Argument jest `SKCanvas` obiektu. To `SKCanvas` mogą już mieć przekształcenie zastosowane, więc `Paint` łączy metody `Matrix` właściwości skojarzone z mapy bitowej do przekształcenia istniejących i przywraca kanwy, po zakończeniu:

```csharp
class TouchManipulationBitmap
{
    ...
    public void Paint(SKCanvas canvas)
    {
        canvas.Save();
        SKMatrix matrix = Matrix;
        canvas.Concat(ref matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();
    }
    ...
}
```

`HitTest` Metoda zwraca `true` Jeśli użytkownik dotyka ekranu w punkcie w granicach mapy bitowej. Jako użytkownik modyfikuje mapy bitowej, mapy bitowej może obracać lub nawet (przy użyciu kombinacji anizotropowego skalowania i obracania) można w kształcie równoległobok. Może być obawy, że `HitTest` metoda musi implementować dość złożone geometrii analityczne w takiej sytuacji.

Jednak skrót jest:

Określenie, czy punkt znajduje się w granicach przekształcone prostokąta jest taka sama jak ustalania, czy punkt przekształcone odwrotność znajduje się w granicach Nieprzekształcony prostokąta. Jest znacznie łatwiejsze obliczeń i mogą być używane wygodnym `Contains` metody zdefiniowanej przez `SKRect`:

```csharp
class TouchManipulationBitmap
{
    ...
    public bool HitTest(SKPoint location)
    {
        // Invert the matrix
        SKMatrix inverseMatrix;

        if (Matrix.TryInvert(out inverseMatrix))
        {
            // Transform the point using the inverted matrix
            SKPoint transformedPoint = inverseMatrix.MapPoint(location);

            // Check if it's in the untransformed bitmap rectangle
            SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
            return rect.Contains(transformedPoint);
        }
        return false;
    }
    ...
}
```

Druga metoda publiczna w `TouchManipulationBitmap` jest `ProcessTouchEvent`. Gdy ta metoda jest wywoływana, już ustalono czy zdarzenia dotykowe należy do tej konkretnej mapy bitowej. Metoda przechowuje słownik [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) obiektów, które jest po prostu poprzedniego punktu i nowy punkt każdej linii papilarnych:

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Oto słownika i `ProcessTouchEvent` sama metoda:

```csharp
class TouchManipulationBitmap
{
    ...
    Dictionary<long, TouchManipulationInfo> touchDictionary =
        new Dictionary<long, TouchManipulationInfo>();
    ...
    public void ProcessTouchEvent(long id, TouchActionType type, SKPoint location)
    {
        switch (type)
        {
            case TouchActionType.Pressed:
                touchDictionary.Add(id, new TouchManipulationInfo
                {
                    PreviousPoint = location,
                    NewPoint = location
                });
                break;

            case TouchActionType.Moved:
                TouchManipulationInfo info = touchDictionary[id];
                info.NewPoint = location;
                Manipulate();
                info.PreviousPoint = info.NewPoint;
                break;

            case TouchActionType.Released:
                touchDictionary[id].NewPoint = location;
                Manipulate();
                touchDictionary.Remove(id);
                break;

            case TouchActionType.Cancelled:
                touchDictionary.Remove(id);
                break;
        }
    }
    ...
}
```

W `Moved` i `Released` zdarzenia, wywołania metody `Manipulate`. W tych godzinach `touchDictionary` zawiera jeden lub więcej `TouchManipulationInfo` obiektów. Jeśli `touchDictionary` zawiera jeden element, jest prawdopodobne, `PreviousPoint` i `NewPoint` wartości są różne i reprezentuje przepływ palcem. Jeśli wiele palców są dotknięcie mapy bitowej, słownik zawiera więcej niż jeden element, ale tylko jeden z tych elementów są różne w różnych `PreviousPoint` i `NewPoint` wartości. Wszystkie pozostałe mają równe `PreviousPoint` i `NewPoint` wartości.

Jest to ważne: `Manipulate` metody, można założyć, że przetwarzania przepływu finger tylko jeden. Podczas tego wywołania żadne inne palców przemieszczają się i one naprawdę przechodzenia (co jest prawdopodobne), będą przetwarzane tych przepływów w przyszłych wywołaniach `Manipulate`.

`Manipulate` Metoda najpierw kopiuje słownika jako tablicę dla wygody. Ignoruje coś innego niż pierwsze dwie pozycje. Jeśli więcej niż dwóch palców próbuje manipulowania mapy bitowej, pozostałe są ignorowane. `Manipulate` jest ostateczny członkiem `TouchManipulationBitmap`:

```csharp
class TouchManipulationBitmap
{
    ...
    void Manipulate()
    {
        TouchManipulationInfo[] infos = new TouchManipulationInfo[touchDictionary.Count];
        touchDictionary.Values.CopyTo(infos, 0);
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();

        if (infos.Length == 1)
        {
            SKPoint prevPoint = infos[0].PreviousPoint;
            SKPoint newPoint = infos[0].NewPoint;
            SKPoint pivotPoint = Matrix.MapPoint(bitmap.Width / 2, bitmap.Height / 2);

            touchMatrix = TouchManager.OneFingerManipulate(prevPoint, newPoint, pivotPoint);
        }
        else if (infos.Length >= 2)
        {
            int pivotIndex = infos[0].NewPoint == infos[0].PreviousPoint ? 0 : 1;
            SKPoint pivotPoint = infos[pivotIndex].NewPoint;
            SKPoint newPoint = infos[1 - pivotIndex].NewPoint;
            SKPoint prevPoint = infos[1 - pivotIndex].PreviousPoint;

            touchMatrix = TouchManager.TwoFingerManipulate(prevPoint, newPoint, pivotPoint);
        }

        SKMatrix matrix = Matrix;
        SKMatrix.PostConcat(ref matrix, touchMatrix);
        Matrix = matrix;
    }
}
```

Jeśli jeden palca jest manipulowania mapy bitowej, `Manipulate` wywołania `OneFingerManipulate` metody `TouchManipulationManager` obiektu. Dla dwóch palców wywoływanych przez nią `TwoFingerManipulate`. Argumenty do metody te są takie same: `prevPoint` i `newPoint` argumenty reprezentują finger, który jest przenoszenie. Ale `pivotPoint` argumentu jest inny dla dwóch wywołań:

Do manipulacji finger jeden `pivotPoint` Centrum mapy bitowej. Ma to na dla obrotu co finger. W przypadku manipulowania finger dwa zdarzenia wskazuje przepływu finger tylko jeden tak, aby `pivotPoint` jest finger, która nie jest przenoszona.

W obu przypadkach `TouchManipulationManager` zwraca `SKMatrix` wartość, która metoda łączy się z bieżącym `Matrix` właściwości, `TouchManipulationPage` używa do renderowania mapy bitowej.

`TouchManipulationManager` jest uogólniona i korzysta z żadnych innych plików, z wyjątkiem `TouchManipulationMode`. Można użyć tej klasy bez zmian we własnych aplikacjach. Definiuje jedną właściwość typu `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


Jednakże, prawdopodobnie należy unikać `AnisotropicScale` opcji. Jest bardzo łatwe przy użyciu tej opcji do manipulowania mapy bitowej, tak aby jednym z czynników skalowania staje się zerem. Dzięki temu mapa bitowa, są usuwane z kontakt nigdy do niej powrócić. Jeśli naprawdę potrzebujesz anizotropowego skalowania, należy poprawić logikę w celu uniknięcia niepożądane wyniki.

`TouchManipulationManager` użyto wektorów, ale ponieważ ma nie `SKVector` strukturze SkiaSharp, `SKPoint` zamian jest używana. `SKPoint` obsługuje operator odejmowania, a także wynik może być traktowana jako wektor. Logika tylko specyficzne dla wektora, wymagana w celu dodania `Magnitude` obliczeń:

```csharp
class TouchManipulationManager
{
    ...
    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
}
```

Zawsze, gdy wybrano obrotu, metodach jeden finger i dwóch palców manipulacji obsługiwać obrót najpierw. W przypadku wykrycia obracania składnika obrotu skutecznie zostaną usunięte. Co jeszcze pozostało jest interpretowany jako kadrowania i skalowania.

Oto `OneFingerManipulate` metody. Jeśli obrotu co palca nie zostało włączone, a następnie logika jest prosta &mdash; po prostu używa poprzedniego punktu i nowy punkt do utworzenia wektora o nazwie `delta` odpowiada dokładnie tłumaczenia. Dzięki obrotu finger jeden włączony metoda używa kąty od punktu obrotu (Centrum mapy bitowej) do wcześniejszego punktu i nowy punkt do konstruowania macierz obrotu:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }

    public SKMatrix OneFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        if (Mode == TouchManipulationMode.None)
        {
            return SKMatrix.MakeIdentity();
        }

        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint delta = newPoint - prevPoint;

        if (Mode == TouchManipulationMode.ScaleDualRotate)  // One-finger rotation
        {
            SKPoint oldVector = prevPoint - pivotPoint;
            SKPoint newVector = newPoint - pivotPoint;

            // Avoid rotation if fingers are too close to center
            if (Magnitude(newVector) > 25 && Magnitude(oldVector) > 25)
            {
                float prevAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                // Calculate rotation matrix
                float angle = newAngle - prevAngle;
                touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                // Effectively rotate the old vector
                float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                oldVector.X = magnitudeRatio * newVector.X;
                oldVector.Y = magnitudeRatio * newVector.Y;

                // Recalculate delta
                delta = newVector - oldVector;
            }
        }

        // Multiply the rotation matrix by a translation matrix
        SKMatrix.PostConcat(ref touchMatrix, SKMatrix.MakeTranslation(delta.X, delta.Y));

        return touchMatrix;
    }
    ...
}
```

W `TwoFingerManipulate` metody punktu obrotu jest pozycja finger, która nie jest przenoszona w tym przypadku danego touch. Obrót jest bardzo podobny do rotacji finger jeden, a następnie nazwę wektora `oldVector` (oparte na poprzedni punkt) jest dostosowywany dla obrotu. Pozostały ruch jest interpretowany jako skalowania:

```csharp
class TouchManipulationManager
{
    ...
    public SKMatrix TwoFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint oldVector = prevPoint - pivotPoint;
        SKPoint newVector = newPoint - pivotPoint;

        if (Mode == TouchManipulationMode.ScaleRotate ||
            Mode == TouchManipulationMode.ScaleDualRotate)
        {
            // Find angles from pivot point to touch points
            float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
            float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

            // Calculate rotation matrix
            float angle = newAngle - oldAngle;
            touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

            // Effectively rotate the old vector
            float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
            oldVector.X = magnitudeRatio * newVector.X;
            oldVector.Y = magnitudeRatio * newVector.Y;
        }

        float scaleX = 1;
        float scaleY = 1;

        if (Mode == TouchManipulationMode.AnisotropicScale)
        {
            scaleX = newVector.X / oldVector.X;
            scaleY = newVector.Y / oldVector.Y;

        }
        else if (Mode == TouchManipulationMode.IsotropicScale ||
                 Mode == TouchManipulationMode.ScaleRotate ||
                 Mode == TouchManipulationMode.ScaleDualRotate)
        {
            scaleX = scaleY = Magnitude(newVector) / Magnitude(oldVector);
        }

        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
        {
            SKMatrix.PostConcat(ref touchMatrix,
                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y));
        }

        return touchMatrix;
    }
    ...
}
```

Można zauważyć jest nie jawnego tłumaczenia w przypadku tej metody. Jednakże, zarówno `MakeRotation` i `MakeScale` metody są oparte na punkt obrotu i zawiera niejawne tłumaczenia. Jeśli używasz dwóch palców na mapę bitową i przeciągając je w tym samym kierunku `TouchManipulation` otrzyma szereg zdarzeń touch przełączanie pomiędzy dwoma palcami. Za każdym finger przenosi względem innych, skalowanie lub obrót wyniki, ale jest ujemna przez inne finger przepływu, a wynik jest tłumaczenia.

Tylko pozostała część **Touch manipulowania** strona jest `PaintSurface` obsługi w `TouchManipulationPage` pliku CodeBehind. Powoduje to wywołanie `Paint` metody `TouchManipulationBitmap`, którego dotyczy macierzy reprezentujący działania skumulowana touch:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    MatrixDisplay matrixDisplay = new MatrixDisplay();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        bitmap.Paint(canvas);

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(bitmap.Matrix);

        matrixDisplay.Paint(canvas, bitmap.Matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));
    }
}
```

`PaintSurface` Obsługi stwierdza, wyświetlając `MatrixDisplay` przedstawiający macierzy skumulowana touch obiektu:

[![](touch-images/touchmanipulation-small.png "Potrójna zrzut ekranu przedstawiający stronę Touch manipulowania")](touch-images/touchmanipulation-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę manipulowania Touch")

## <a name="manipulating-multiple-bitmaps"></a>Manipulowanie wiele map bitowych

Jedną z korzyści wynikające z izolowania takich jak dotyk przetwarzania kodu w klasach `TouchManipulationBitmap` i `TouchManipulationManager` jest możliwość ponownego użycia tych klas w programie, który umożliwia użytkownikowi manipulowania wiele map bitowych.

**Widok punktowy mapy bitowej** strony pokazuje, jak to zrobić. Zamiast definiować pola typu `TouchManipulationBitmap`, [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) klasa definiuje `List` mapy bitowej obiektów:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    List<TouchManipulationBitmap> bitmapCollection =
        new List<TouchManipulationBitmap>();
    ...
    public BitmapScatterViewPage()
    {
        InitializeComponent();

        // Load in all the available bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;
        string[] resourceIDs = assembly.GetManifestResourceNames();
        SKPoint position = new SKPoint();

        foreach (string resourceID in resourceIDs)
        {
            if (resourceID.EndsWith(".png") ||
                resourceID.EndsWith(".jpg"))
            {
                using (Stream stream = assembly.GetManifestResourceStream(resourceID))
                {
                    SKBitmap bitmap = SKBitmap.Decode(stream);
                    bitmapCollection.Add(new TouchManipulationBitmap(bitmap)
                    {
                        Matrix = SKMatrix.MakeTranslation(position.X, position.Y),
                    });
                    position.X += 100;
                    position.Y += 100;
                }
            }
        }
    }
    ...
}
```

Konstruktor ładuje we wszystkich map bitowych dostępne jako zasoby osadzone i doda je do `bitmapCollection`. Należy zauważyć, że `Matrix` właściwość jest inicjowana na każdym `TouchManipulationBitmap` obiektu, więc narożników lewego górnego każdą mapę bitową są Przesunięcie x 100 pikseli.

`BitmapScatterView` Strony musi również obsługiwać zdarzenia dotykowe dla wielu map bitowych. Zamiast definiować `List` dotykowego obecnie zmieniane identyfikatory `TouchManipulationBitmap` obiektów, ten program wymaga słownika:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    Dictionary<long, TouchManipulationBitmap> bitmapDictionary =
       new Dictionary<long, TouchManipulationBitmap>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                for (int i = bitmapCollection.Count - 1; i >= 0; i--)
                {
                    TouchManipulationBitmap bitmap = bitmapCollection[i];

                    if (bitmap.HitTest(point))
                    {
                        // Move bitmap to end of collection
                        bitmapCollection.Remove(bitmap);
                        bitmapCollection.Add(bitmap);

                        // Do the touch processing
                        bitmapDictionary.Add(args.Id, bitmap);
                        bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                        canvasView.InvalidateSurface();
                        break;
                    }
                }
                break;

            case TouchActionType.Moved:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    bitmapDictionary.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Zwróć uwagę sposób, w jaki `Pressed` logikę pętli `bitmapCollection` w odwrotnej kolejności. Mapy bitowe często nakładać się na siebie nawzajem. Mapy bitowe, w dalszej części kolekcji wizualnie znajdować się na górze mapy bitowe wcześniej w kolekcji. W przypadku wielu map bitowych pod palcem, który naciśnie na ekranie znajdujące się najwyżej jeden musi być jeden, który jest przetwarzany przez ten finger.

Ponadto należy zauważyć, że `Pressed` logiki tak, aby wizualnie przemieszczał się na górze stosu innych mapy bitowe przenosi tej mapy bitowej na końcu kolekcji.

W `Moved` i `Released` zdarzenia, `TouchAction` obsługi zdarzeń wywołuje `ProcessingTouchEvent` method in Class metoda `TouchManipulationBitmap` podobnie jak w przypadku starszych program.

Na koniec `PaintSurface` obsługi zdarzeń wywołuje `Paint` metoda każdego `TouchManipulationBitmap` obiektu:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (TouchManipulationBitmap bitmap in bitmapCollection)
        {
            bitmap.Paint(canvas);
        }
    }
}
```

Kod w pętli kolekcji i wyświetla wiele map bitowych z początku kolekcji na końcu:

[![](touch-images/bitmapscatterview-small.png "Potrójna zrzut ekranu przedstawiający stronę widok punktowy mapy bitowej")](touch-images/bitmapscatterview-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę widok punktowy mapy bitowej")

## <a name="single-finger-scaling"></a>Skalowanie pojedynczej palca

Operacja skalowania jest ogólnie wymaga gest uszczypnięcia, używając dwóch palców. Jednak jest możliwe zaimplementowanie skalowanie za pomocą pojedynczej linii papilarnych dzięki finger przenieść narożników mapy bitowej.

Jest to zaprezentowane w **pojedynczego skalowania rogu Finger** strony. Ponieważ w tym przykładzie użyto nieco inny rodzaj skalowania, który wdrożony `TouchManipulationManager` klasy, nie używa tej klasy lub `TouchManipulationBitmap` klasy. Zamiast tego całą logikę touch znajduje się w pliku związanym z kodem. Jest to nieco prostsza logiki niż zwykle, ponieważ śledzi tylko jeden palca naraz, a po prostu ignoruje dodatkowej palców, które mogą być dotykanie ekranu.

[ **SingleFingerCornerScale.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml) tworzy stronę `SKCanvasView` klasy i tworzy `TouchEffect` obiektu do śledzenia zdarzeń dotyku:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.SingleFingerCornerScalePage"
             Title="Single Finger Corner Scale">

    <Grid BackgroundColor="White"
          Grid.Row="1">

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction"   />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

[ **SingleFingerCornerScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs) plik ładuje zasób mapy bitowej z **Media** katalogu i wyświetla je przy użyciu `SKMatrix` obiektu zdefiniowany jako pole:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();
    ···

    public SingleFingerCornerScalePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.SetMatrix(currentMatrix);
        canvas.DrawBitmap(bitmap, 0, 0);
    }
    ···
}
```

To `SKMatrix` obiekt zostanie zmodyfikowany przez logikę touch, pokazano poniżej.

W pozostałej części pliku związanego z kodem jest `TouchEffect` programu obsługi zdarzeń. Jego rozpoczyna się od bieżącej lokalizacji palcem do konwertowania `SKPoint` wartość. Dla `Pressed` typ akcji programu obsługi sprawdza, czy nie inne finger zachodzi ekranu, i że finger znajduje się w granicach mapy bitowej.

Kluczowym elementem kodu jest `if` instrukcji obejmujących dwie wywołania `Math.Pow` metody. Ten matematyczne sprawdza, czy lokalizacja finger poza elipsy, który wypełnia mapy bitowej. Jeśli tak, to operacji skalowania. Finger zbliża się jeden z jej narożników mapy bitowej, a punktu obrotu jest określona, że jest przeciwny prawym górnym rogu. Jeśli palcem w tym elipsy, jest normalnej przesuwania:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();

    // Information for translating and scaling
    long? touchId = null;
    SKPoint pressedLocation;
    SKMatrix pressedMatrix;

    // Information for scaling
    bool isScaling;
    SKPoint pivotPoint;
    ···

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Track only one finger
                if (touchId.HasValue)
                    return;

                // Check if the finger is within the boundaries of the bitmap
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = currentMatrix.MapRect(rect);
                if (!rect.Contains(point))
                    return;

                // First assume there will be no scaling
                isScaling = false;

                // If touch is outside interior ellipse, make this a scaling operation
                if (Math.Pow((point.X - rect.MidX) / (rect.Width / 2), 2) +
                    Math.Pow((point.Y - rect.MidY) / (rect.Height / 2), 2) > 1)
                {
                    isScaling = true;
                    float xPivot = point.X < rect.MidX ? rect.Right : rect.Left;
                    float yPivot = point.Y < rect.MidY ? rect.Bottom : rect.Top;
                    pivotPoint = new SKPoint(xPivot, yPivot);
                }

                // Common for either pan or scale
                touchId = args.Id;
                pressedLocation = point;
                pressedMatrix = currentMatrix;
                break;

            case TouchActionType.Moved:
                if (!touchId.HasValue || args.Id != touchId.Value)
                    return;

                SKMatrix matrix = SKMatrix.MakeIdentity();

                // Translating
                if (!isScaling)
                {
                    SKPoint delta = point - pressedLocation;
                    matrix = SKMatrix.MakeTranslation(delta.X, delta.Y);
                }
                // Scaling
                else
                {
                    float scaleX = (point.X - pivotPoint.X) / (pressedLocation.X - pivotPoint.X);
                    float scaleY = (point.Y - pivotPoint.Y) / (pressedLocation.Y - pivotPoint.Y);
                    matrix = SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);
                }

                // Concatenate the matrices
                SKMatrix.PreConcat(ref matrix, pressedMatrix);
                currentMatrix = matrix;
                canvasView.InvalidateSurface();
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = null;
                break;
        }
    }
}
```

`Moved` Typ akcji oblicza macierzy odpowiadający działania touch od momentu finger naciśnięcia ekranu do tego czasu. Tej macierzy oraz w macierzy go łączy obowiązuje w czasie finger naciskanie mapy bitowej. Operacja skalowania jest zawsze względem rogu przeciwnej ten, który korzysta z finger.

Dla małych i prostokątnych map bitowych elipsę posługiwanie się nimi może zajmować większość mapę bitową i pozostaw bardzo niewielki obszarów w rogach skalowania mapy bitowej. Preferować nieco inne podejścia, w tym przypadku możesz zastąpić ten całą `if` blok, który ustawia `isScaling` do `true` przy użyciu tego kodu:

```csharp
float halfHeight = rect.Height / 2;
float halfWidth = rect.Width / 2;

// Top half of bitmap
if (point.Y < rect.MidY)
{
    float yRelative = (point.Y - rect.Top) / halfHeight;

    // Upper-left corner
    if (point.X < rect.MidX - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Bottom);
    }
    // Upper-right corner
    else if (point.X > rect.MidX + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Bottom);
    }
}
// Bottom half of bitmap
else
{
    float yRelative = (point.Y - rect.MidY) / halfHeight;

    // Lower-left corner
    if (point.X < rect.Left + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Top);
    }
    // Lower-right corner
    else if (point.X > rect.Right - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Top);
    }
}
```

Ten kod skutecznie dzieli obszaru mapy bitowej do wnętrza romb i cztery trójkąty w rogach. Pozwala to znacznie większych obszarów w rogach i skalowanie mapy bitowej.

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Wywoływanie zdarzenia z efektów](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
