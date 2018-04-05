---
title: Kropki i łączniki
description: Zawiłościami Rysowanie linii kropkowanej i kreskowane w SkiaSharp
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 46ab21aa5156a6deab5952f165917cc299b500ac
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/05/2018
---
# <a name="dots-and-dashes"></a>Kropki i łączniki

_Zawiłościami Rysowanie linii kropkowanej i kreskowane w SkiaSharp_

SkiaSharp umożliwia rysowanie linii, które nie są stałe, ale zamiast tego składają się z kropki i kreski:

![](dots-images/dottedlinesample.png "Linia kropkowana")

W tym z *efekt ścieżki*, który jest wystąpieniem [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) klasy, która zostanie ustawiona na [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) właściwość `SKPaint`. Można utworzyć ścieżki efekt (lub łączenie ścieżki efekty) przy użyciu statycznych `Create` metody zdefiniowane przez `SKPathEffect`.

Rysowanie linii kropkowanej lub przerywanej, należy użyć [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) metody statycznej. Istnieją dwa argumenty: jest to pierwszy tablicę `float` wartości, które wskazują długości kropki i kreski i długość odstępów między nimi. Ta tablica musi mieć parzysta liczba elementów, a powinien być co najmniej dwa elementy. (Może być zerowy elementów w tablicy, ale linia ciągła, którego wynikiem). Czy istnieją dwa elementy, pierwszy długość kropki i kreski, a drugą jest wartość długości odstęp przed dalej kropką ani kreską. Jeśli istnieje więcej niż dwa elementy, a następnie znajdują się w następującej kolejności: kreski-długość, długość luki, długość kreski, długość luki i tak dalej.

Ogólnie rzecz biorąc będzie ma być wielokrotnością szerokość pociągnięć kreska i przerwę długości. Jeśli szerokość pociągnięć 10 pikseli, na przykład następnie tablicy {10, 10} zostanie Rysuj linię kropkowaną skutkującej taką samą długość jak grubość pociągnięć luki i kropek.

Jednak `StrokeCap` ustawienie `SKPaint` obiektu dotyczy także te kropki i łączniki. Jak można zauważyć wkrótce, który ma wpływ na elementy tej tablicy.

Kropkami i linie przerywane przedstawiono na **kropki i kreski** strony. [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) pliku tworzy dwa `Picker` widoków, jeden dla możliwości wybierania koniec obrysu i drugą do tablicy dash wybierz:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.DotsAndDashesPage"
             Title="Dots and Dashes">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>10, 10</x:String>
                <x:String>30, 10</x:String>
                <x:String>10, 10, 30, 10</x:String>
                <x:String>0, 20</x:String>
                <x:String>20, 20</x:String>
                <x:String>0, 20, 20, 20</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

Pierwsze trzy elementy `dashArrayPicker` założono, że szerokość pociągnięć jest 10 pikseli. {10, 10} tablicy dotyczy linię kropkowaną {30, 10} jest linię kropkowaną i {10, 10, 30, 10} jest dla wiersza kropki i kreski. (Inne trzech zostanie dokładnie omówione wkrótce.)

[ `DotsAndDashesPage` Pliku CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) zawiera `PaintSurface` obsługi zdarzeń i kilka procedury pomocnika do uzyskiwania dostępu do `Picker` widoków:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = (SKStrokeCap)Enum.Parse(typeof(SKStrokeCap),
                        strokeCapPicker.Items[strokeCapPicker.SelectedIndex]),

        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }

    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string[] strs = picker.Items[picker.SelectedIndex].Split(new char[] { ' ', ',' },
                                                             StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

Na poniższych zrzutach ekranu na ekranie z systemem iOS przy lewej zostanie wyświetlona linia kropkowana:

[![](dots-images/dotsanddashes-small.png "Potrójna zrzut ekranu przedstawiający stronę kropki i kreski")](dots-images/dotsanddashes-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę kropki i łączniki")

Jednak Android ekranu również powinien pokazać kropkowana, przy użyciu tablicy {10, 10}, ale zamiast tego wiersz jest pełny. Co się stało? Problem jest Android ekranu również ustawienie caps obrysu `Square`. Spowoduje to rozszerzenie wszystkie łączniki o połowę szerokości obrysu, powoduje zapełnić luk.

Aby ominąć ten problem, korzystając z obrysu centralnych zasad dostępu z `Square` lub `Round`, musi zmniejszenie długości dash w tablicy długości obrysu (czasem spowodować, że długość kreski 0) i zwiększenie długości przerwy długość obrysu. Jest to, jak końcowego trzech kreska tablic w `Picker` zostały obliczone w pliku XAML:

- {10, 10} staje się {0, 20} dla linia kropkowana
- {30, 10} staje się {20, 20} dla linii kreskowanej
- {10, 10, 30, 10} staje się {0, 20, 20, 20} dla linię kropkowaną i kreskowane

Nagrania ekranu systemu Windows z kropkami i linii dla linii kreskowanej cap z `Round`. `Round` Koniec obrysu często sprawia, że najważniejsze kropki i kreski w wierszach grubości.

Do tej pory wprowadzono nie wymieniono drugiego parametru `SKPathEffect.CreateDash` metody. Ten parametr ma nazwę `phase` i odnosi się do przesunięcie w wzorcu kropki i kreski do początku wiersza. Na przykład, jeśli tablica dash to {10, 10} i `phase` wynosi 10, a następnie wiersz rozpoczyna się od przerwę zamiast kropką.

Jeden interesujące stosowania `phase` parametr jest animacji. **Animowany spirali** strona jest podobna do **spirali Archimedean** strony, z wyjątkiem [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs) animuje klasy `phase` parametru. Strona przedstawiono również innego podejścia do animacji. Przykład wcześniejszych [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs) używane `Task.Delay` metody kontrolowania animacji. W tym przykładzie użyto zamiast tego platformy Xamarin.Forms `Device.Timer` metody:


```csharp
const double cycleTime = 250;       // in milliseconds

SKCanvasView canvasView;
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float dashPhase;

public AnimatedSpiralPage()
{
    Title = "Animated Spiral";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    stopwatch.Start();

    Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
    {
        double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
        dashPhase = (float)(10 * t);
        canvasView.InvalidateSurface();

        if (!pageIsActive)
        {
            stopwatch.Stop();
        }

        return pageIsActive;
    });
}
```

Oczywiście będzie konieczne aktualnie ma uruchomiony program, aby zobaczyć animacji:

[![](dots-images/animatedspiral-small.png "Potrójna zrzut ekranu przedstawiający stronę animowany spirali")](dots-images/animatedspiral-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę animowany spirali")

Teraz przedstawiono sposób Rysowanie linii i zdefiniuj krzywych przy użyciu parametrów równania. Sekcja do opublikowania później będzie obejmować różne rodzaje krzywych który `SKPath` obsługuje.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
