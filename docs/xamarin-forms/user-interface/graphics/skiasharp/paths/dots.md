---
title: Kropki i kreski SkiaSharp
description: W tym artykule przedstawiono, jak skutecznie stawisz niewymagającego rysunku kropkowane i kreskowane wierszy w SkiaSharp i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: a28e4bbaae28befd91278ac5c2b9e7c9c0b522b9
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615421"
---
# <a name="dots-and-dashes-in-skiasharp"></a>Kropki i kreski SkiaSharp

_Główna niewymagającego rysunku kropkowane i kreskowane wierszy w SkiaSharp_

Skiasharp — umożliwia narysowanie wierszy, które nie są stałe, ale zamiast tego składają się z kropki i kreski:

![](dots-images/dottedlinesample.png "Linia kropkowana")

Tego *efekt ścieżki*, który jest wystąpieniem [ `SKPathEffect` ](xref:SkiaSharp.SKPathEffect) klasę, która jest ustawiona na [ `PathEffect` ](xref:SkiaSharp.SKPaint.PathEffect) właściwość `SKPaint`. Można utworzyć ścieżki efekt (lub łączenia efekty ścieżek) przy użyciu jednej z metod tworzenia statycznych, zdefiniowane przez `SKPathEffect`. (`SKPathEffect` jest jeden z sześciu efektów obsługiwane przez SkiaSharp; inne są opisane w sekcji [ **efekt SkiaSharp**](../effects/index.md).)

Rysowanie linii kropkowanej lub przerywanej, należy użyć [ `SKPathEffect.CreateDash` ](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) metody statycznej. Istnieją dwa argumenty: jest to pierwsze tablicę `float` wartości, które wskazują długości kropki i kreski i długość odstępów między nimi. Ta tablica musi mieć parzysta liczba elementów, a powinien być co najmniej dwa elementy. (Może mieć zero elementów w tablicy, ale linia ciągła, którego wynikiem.) Czy istnieją dwa elementy, pierwszy długość kropka lub kreska, a drugą jest długość luki przed następnym kropka lub kreska. Jeśli istnieje więcej niż dwa elementy, a następnie znajdują się w następującej kolejności: kreski, długość, długość przerwy, długość kreski, długość przerwy i tak dalej.

Ogólnie rzecz biorąc należy wprowadzić wielokrotnością szerokość pociągnięcia długości dash i przerwy. Jeśli szerokość pociągnięcia 10 pikseli, na przykład, następnie tablicy {10, 10} będzie narysuj linię kropkowaną których kropki i luki mają taką samą długość jak grubość pociągnięć.

Jednak `StrokeCap` ustawienie `SKPaint` obiekt ma również wpływ na te kropki i kreski. Jak można zauważyć wkrótce, który ma wpływ na elementy tej tablicy.

Kropkowana i wiersze przerywaną, co przedstawiono na **kropki i kreski** strony. [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) plików są tworzone wystąpienia dwóch `Picker` widoków, jeden dla umożliwiające wybranie pociągnięcia i drugą do tablicy dash wybierz:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.DotsAndDashesPage"
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
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKStrokeCap}">
                    <x:Static Member="skia:SKStrokeCap.Butt" />
                    <x:Static Member="skia:SKStrokeCap.Round" />
                    <x:Static Member="skia:SKStrokeCap.Square" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>10, 10</x:String>
                    <x:String>30, 10</x:String>
                    <x:String>10, 10, 30, 10</x:String>
                    <x:String>0, 20</x:String>
                    <x:String>20, 20</x:String>
                    <x:String>0, 20, 20, 20</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                PaintSurface="OnCanvasViewPaintSurface"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

 Pierwsze trzy elementy `dashArrayPicker` przyjęto założenie, że szerokość pociągnięcia jest 10 pikseli. {10, 10} tablica dotyczy linię kropkowaną {30, 10} dotyczy linię przerywaną, co i {10, 10, 30, 10} dotyczy wiersza kropki i kreski. (Pozostałych trzech zostanie dokładnie omówione wkrótce.)

[ `DotsAndDashesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) Zawiera plik związany z kodem `PaintSurface` program obsługi zdarzeń i kilku procedur pomocnika do uzyskiwania dostępu do `Picker` widoki:

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
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem,
        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint); 
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string str = (string)picker.SelectedItem;
    string[] strs = str.Split(new char[] { ' ', ',' }, StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

W poniższych zrzutach ekranu na końcu z lewej strony ekranu w systemie iOS pokazuje linię kropkowaną:

[![](dots-images/dotsanddashes-small.png "Potrójna zrzut ekranu przedstawiający stronę kropki i kreski")](dots-images/dotsanddashes-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę kropki i kreski")

Jednak dla systemu Android ekranu również powinien Pokaż linię kropkowaną przy użyciu tablicy {10, 10}, ale zamiast tego wiersza jest pełny. Co się stało? Problem polega na to, czy ekranu dla systemu Android ma również ustawienie caps obrysu `Square`. Spowoduje to rozszerzenie wszystkich łączników o połowę szerokości obrysu, powoduje wypełniają luki.

Aby ominąć ten problem, korzystając z pociągnięcia z `Square` lub `Round`, należy zmniejszyć długości dash w tablicy długości obrysu (czasami skutkuje dash długość 0) i zwiększenia długości przerwy przez długość pociągnięcia. Jest to, jak trzy końcowej kreski tablic w `Picker` zostały obliczone w pliku XAML:

- {10, 10} staje się {0, 20} dla linia kropkowana
- {30, 10} staje się {20, 20} dla linii kreskowanej
- {10, 10, 30, 10} staje się {0, 20, 20, 20} dla linię kropkowaną i kreskowane

Pokazuje ekran platformy uniwersalnej systemu Windows, kropkami, które linia przerywana linii pociągnięcia limit z `Round`. `Round` Grubości linii pociągnięcia często zapewnia najlepszy wygląd, kropki i kreski.

Do tej pory nie wymieniono stało się z drugim parametrem `SKPathEffect.CreateDash` metody. Ten parametr nosi nazwę `phase` i odwołuje się do przesunięcia w ramach wzorzec kropki i kreski do początku wiersza. Na przykład, jeśli tablica dash jest {10, 10} i `phase` wynosi 10, a następnie wiersz zaczyna się od przerwa, a nie pojedynczego znaku kropki.

Jeden ciekawszych zastosowań `phase` parametr znajduje się w animacji. **Animowane spirali** strona jest podobna do **spirali Archimedean** strony, chyba że [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/AnimatedSpiralPage.cs) animuje klasy `phase` przy użyciu parametru Zestaw narzędzi Xamarin.Forms `Device.Timer` metody:


```csharp
public class AnimatedSpiralPage : ContentPage
{
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
    ···  
}
```

Oczywiście trzeba będzie uruchamiany program, aby zobaczyć animacji:

[![](dots-images/animatedspiral-small.png "Potrójna zrzut ekranu przedstawiający stronę animowane spirali")](dots-images/animatedspiral-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę spirali animowane")

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
