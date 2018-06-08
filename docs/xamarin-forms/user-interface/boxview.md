---
title: BoxView
description: Użyj kolorowe prostokąt dla dekoracji, grafiki i interakcji.
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2017
ms.openlocfilehash: 356d0effe55638902b6ee599a0d9fb7e9b8ade2d
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848411"
---
# <a name="boxview"></a>BoxView

[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) renderuje prosty prostokąt o określonej szerokości, wysokości i kolor. Można użyć `BoxView` dekoracji podstawowe grafiki i interakcji z użytkownikiem za pośrednictwem touch.

Ponieważ platformy Xamarin.Forms nie dysponuje systemem grafiki wbudowanych wektor `BoxView` pomaga kompensacji. Niektóre programy przykładowe opisane w tym artykule `BoxView` do renderowania grafiki. `BoxView` Można o rozmiarze, aby przypominały wiersza o określonej szerokości i grubości, a następnie obracać przy użyciu dowolnego kąta `Rotation` właściwości.

Mimo że `BoxView` można naśladować proste grafiki, należy zbadać [przy użyciu SkiaSharp w platformy Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) dokładniejsze wymagania grafiki.

W tym artykule omówiono następujące zagadnienia:

- **[Ustawianie koloru BoxView i rozmiaru](#colorandsize)**  &ndash; ustawić `BoxView` właściwości.
- **[Renderowanie tekstu dekoracje](#textdecorations)**  &ndash; użyj `BoxView` renderowania wierszy.
- **[Wyświetlanie kolorów mających BoxView](#listingcolors)**  &ndash; wyświetlić wszystkie kolorów systemu `ListView`.
- **[Odtwarzanie gry życia przez podklasy BoxView](#subclassing)**  &ndash; zaimplementować Słynne automaton komórkową.
- **[Tworzenie zegar cyfrowy](#digitalclock)**  &ndash; symulować wyświetlania Mozaika.
- **[Tworzenie zegar analogowy](#analogclock)**  &ndash; transformacji i animować `BoxView` elementów.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>Ustawienie koloru BoxView i rozmiaru

Bardzo często będzie ustawiony następujących trzech właściwości `BoxView`:

- [`Color`](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) Aby ustawić kolor.
- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) Aby ustawić szerokość `BoxView` w jednostkach niezależnych od urządzenia.
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) Aby ustawić wysokość `BoxView`.

`Color` Właściwość jest typu `Color`; właściwości można ustawić dla każdego `Color` wartość, tym 141 statycznego pola tylko do odczytu z o nazwie kolorów alfabetycznie od `AliceBlue` do `YellowGreen`.

`WidthRequest` i `HeightRequest` właściwości tylko pełnić roli, jeśli `BoxView` jest *nieograniczonego* w układzie. Dotyczy to sytuacji, gdy kontener układu musi wiedzieć, podrzędne jego rozmiar, na przykład kiedy `BoxView` jest elementem podrzędnym o rozmiarze automatycznie komórki w `Grid` układu. A `BoxView` jest również nieograniczonego podczas jego `HorizontalOptions` i `VerticalOptions` właściwości są ustawione na wartości innych niż `LayoutOptions.Fill`. Jeśli `BoxView` jest nieograniczonego, ale `WidthRequest` i `HeightRequest` nie ustawiono właściwości, a następnie szerokości lub wysokości są ustawione na wartości domyślne 40 jednostek lub około 1/4 cala na urządzeniach przenośnych.

`WidthRequest` i `HeightRequest` właściwości są ignorowane, jeśli `BoxView` jest *ograniczonego* w układzie, w których przypadku kontener układu narzuca własny rozmiar `BoxView`. 

A `BoxView` może być ograniczony w jednym wymiarze i nieograniczonego w innym. Na przykład jeśli `BoxView` jest elementem podrzędnym pionowym `StackLayout`, pionowy wymiar `BoxView` jest nieograniczonego i jego poziomy wymiar jest zazwyczaj ograniczone. Ale istnieją wyjątki dla tego wymiaru poziome: Jeśli `BoxView` ma jego `HorizontalOptions` właściwość coś innego niż `LayoutOptions.Fill`, a następnie płaszczyźnie poziomej jest również nieograniczonego. Istnieje również możliwość `StackLayout` do ma nieograniczonego poziome wymiaru, w którym to przypadku `BoxView` będzie również nieograniczonego poziomo.

[ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView) przykładzie wyświetlono co CAL kwadrat nieograniczonego `BoxView` na środku strony:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BasicBoxView"
             x:Class="BasicBoxView.MainPage">

    <BoxView Color="CornflowerBlue"
             WidthRequest="160"
             HeightRequest="160"
             VerticalOptions="Center" 
             HorizontalOptions="Center" />

</ContentPage>
```

W tym miejscu jest wynikiem:

[![Podstawowe BoxView](boxview-images/basicboxview-small.png "BoxView podstawowe")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

Jeśli `VerticalOptions` i `HorizontalOptions` właściwości są usuwane z `BoxView` tagu lub są ustawione na `Fill`, a następnie `BoxView` staje się ograniczona przez rozmiar strony, a rozwijany do wypełnienia strony.

A `BoxView` również może być elementem podrzędnym elementu `AbsoluteLayout`. W takim przypadku lokalizację i rozmiar `BoxView` są ustawiane przy użyciu `LayoutBounds` dołączonych właściwości możliwej do wiązania. `AbsoluteLayout` Opisanej w artykule [ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md).

Zobaczysz przykłady wszystkich tych przypadkach przykładowe programy, które należy wykonać.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>Renderowanie dekoracji tekstu

Można użyć `BoxView` można dodać kilka prostych dekoracje na stronach w postaci wierszy w poziomie i w pionie. [ **TextDecoration** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration) przykładzie pokazano to. Wszystkie elementy wizualne programu są zdefiniowane w **MainPage.xaml** pliku, który zawiera kilka `Label` i `BoxView` elementów w `StackLayout` pokazano poniżej:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:TextDecoration"
             x:Class="TextDecoration.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Black" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ScrollView Margin="15">
        <StackLayout>

            ···

        </StackLayout>
    </ScrollView>
</ContentPage>
```

Wszystkich znaczników, który następuje są elementami podrzędnymi `StackLayout`. Ten kod znaczników składa się z wielu typów ozdobne `BoxView` elementów używanych z `Label` elementu:

[![Dekoracji tekstu](boxview-images/textdecoration-small.png "dekoracji tekstu")](boxview-images/textdecoration-large.png#lightbox "dekoracji tekstu")

Stylowy nagłówka w górnej części strony odbywa się z `AbsoluteLayout` którego elementy podrzędne są cztery `BoxView` elementów i `Label`, wszystkie są przypisane do określonych lokalizacji i rozmiary:

```xaml
<AbsoluteLayout>
    <BoxView AbsoluteLayout.LayoutBounds="0, 10, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="0, 20, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="10, 0, 5, 65" />
    <BoxView AbsoluteLayout.LayoutBounds="20, 0, 5, 65" />
    <Label Text="Stylish Header"
           FontSize="24"
           AbsoluteLayout.LayoutBounds="30, 25, AutoSize, AutoSize"/>
</AbsoluteLayout>
```

W pliku XAML `AbsoluteLayout` następuje `Label` z sformatowanego tekstu, który opisuje `AbsoluteLayout`.

Ciąg tekstowy można podkreślenia, ograniczając jednocześnie `Label` i `BoxView` w `StackLayout` mający jego `HorizontalOptions` wartość coś innego niż `Fill`. Szerokość `StackLayout` następnie podlega szerokość `Label`, który następnie narzuca szerokość ta `BoxView`. `BoxView` Jest przypisane tylko jawne wysokość:

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

Nie można użyć tej metody do underline poszczególnych wyrazów w dłużej ciągów tekstowych lub akapitu.

Istnieje również możliwość użycia `BoxView` tak, aby przypominały HTML `hr` elementu (poziomą). Po prostu let szerokość `BoxView` będzie określany przez jego kontenera nadrzędnego, w tym przypadku jest `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

Na koniec Rysowanie linii pionowej na jednej stronie tekstu akapitu ograniczając jednocześnie `BoxView` i `Label` w poziomym `StackLayout`. W tym przypadku wysokość `BoxView` jest taka sama jak wysokość `StackLayout`, który podlega warunkom wysokość `Label`: 

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
/StackLayout>
```
<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>Wyświetlanie i BoxView kolorów

`BoxView` Jest wygodne wyświetlanie kolorów. Ten program używa `ListView` Aby wyświetlić listę wszystkich publicznego statycznego pola tylko do odczytu z platformy Xamarin.Forms `Color` struktury:

[![Kolory ListView](boxview-images/listviewcolors-small.png "kolory ListView")](boxview-images/listviewcolors-large.png#lightbox "kolory ListView")

[ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/) program zawiera klasę o nazwie `NamedColor`. Konstruktor statyczny uzyskują dostęp wszystkie pola odbicia `Color` struktury i Utwórz `NamedColor` obiekt dla każdego z nich. Są one przechowywane w statycznych `All` właściwości:

```csharp
public class NamedColor
{
    // Instance members.
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    // Static members.
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields ())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof (Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                               (int)(255 * color.R),
                                               (int)(255 * color.G),
                                               (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }
}
```

Elementy wizualne programu są opisane w pliku XAML. `ItemsSource` Właściwość `ListView` ustawiono statycznych `NamedColor.All` właściwości, co oznacza, że `ListView` Wyświetla wszystkie osoby `NamedColor` obiektów: 

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewColors"
             x:Class="ListViewColors.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="10, 20, 10, 0" />
            <On Platform="Android, UWP" Value="10, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ListView SeparatorVisibility="None"
              ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.RowHeight>
            <OnPlatform x:TypeArguments="x:Int32">
                <On Platform="iOS, Android" Value="80" />
                <On Platform="UWP" Value="90" />
            </OnPlatform>
        </ListView.RowHeight>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ContentView Padding="5">
                        <Frame OutlineColor="Accent"
                               Padding="10">
                            <StackLayout Orientation="Horizontal">
                                <BoxView Color="{Binding Color}"
                                         WidthRequest="50"
                                         HeightRequest="50" />
                                <StackLayout>
                                    <Label Text="{Binding FriendlyName}"
                                           FontSize="22"
                                           VerticalOptions="StartAndExpand" />
                                    <Label Text="{Binding RgbDisplay, StringFormat='RGB = {0}'}"
                                           FontSize="16"
                                           VerticalOptions="CenterAndExpand" />
                                </StackLayout>
                            </StackLayout>
                        </Frame>
                    </ContentView>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage> 
```

`NamedColor` Obiekty są formatowane przez `ViewCell` obiekt, który jest ustawiony jako szablon danych `ListView`. Ten szablon obejmuje `BoxView` których `Color` właściwość jest powiązana z `Color` właściwość `NamedColor` obiektu.

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>Gry życia przez podklasy BoxView

Gry użytkowania jest komórkowej automaton opracowana przez mathematician Conway Jan i popularized na stronach *naukowych American* w 1970s. Dobrym wprowadzenie są dostarczane przez artykuł Wikipedia [Conway w grę życia](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

Platformy Xamarin.Forms [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/) program definiuje klasę o nazwie `LifeCell` która pochodzi z `BoxView`. Ta klasa hermetyzuje logiki pojedynczych komórek w grę życia:

```csharp
class LifeCell : BoxView
{
    bool isAlive;

    public event EventHandler Tapped;

    public LifeCell()
    {
        BackgroundColor = Color.White;

        TapGestureRecognizer tapGesture = new TapGestureRecognizer();
        tapGesture.Tapped += (sender, args) =>
        {
            Tapped?.Invoke(this, EventArgs.Empty);
        };
        GestureRecognizers.Add(tapGesture);
    }

    public int Col { set; get; }

    public int Row { set; get; }

    public bool IsAlive
    {
        set
        {
            if (isAlive != value)
            {
                isAlive = value;
                BackgroundColor = isAlive ? Color.Black : Color.White;
            }
        }
        get
        {
            return isAlive;
        }
    }
}
```

`LifeCell` dodaje trzy więcej właściwości do `BoxView`: `Col` i `Row` właściwości przechowywania pozycja komórki w siatce i `IsAlive` właściwość wskazuje jego stan. `IsAlive` Właściwość również określa `Color` właściwość `BoxView` na kolor czarny, jeśli komórka jest aktywne i białe, jeśli komórka nie jest aktywne.

`LifeCell` instaluje również `TapGestureRecognizer` umożliwia użytkownikom przełączyć stan komórki naciskając je. Klasa tłumaczy `Tapped` zdarzenia z aparat rozpoznawania gestów w jego własnej `Tapped` zdarzeń.

**GameOfLife** obejmuje także program `LifeGrid` klasy, która hermetyzuje znacznie logiki gry i `MainPage` klasa, która obsługuje program elementów wizualnych. Obejmują one nakładce, która opisuje reguły gry. Oto program w akcji przedstawiający kilka kilkaset `LifeCell` obiektów na stronie:

[![Gry życia](boxview-images/gameoflife-small.png "gry życia")](boxview-images/gameoflife-large.png#lightbox "gry życia")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>Tworzenie zegar cyfrowy

[ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/) program tworzy 210 `BoxView` elementy, aby symulować punktów stosowane wyświetlania 5-za-7-Mozaika. Czas w trybie pionowa lub pozioma może odczytywać, ale ma on większy w orientacji poziomej:

[![Zegar Mozaika](boxview-images/dotmatrixclock-small.png "zegara Mozaika")](boxview-images/dotmatrixclock-large.png#lightbox "-Mozaika zegara")

Plik XAML nieco więcej niż wystąpienia `AbsoluteLayout` używane zegara:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DotMatrixClock"
             x:Class="DotMatrixClock.MainPage"
             Padding="10"
             SizeChanged="OnPageSizeChanged">

    <AbsoluteLayout x:Name="absoluteLayout"
                    VerticalOptions="Center" />
</ContentPage>
```

Wszystkie inne odbywa się w pliku CodeBehind. Logika wyświetlania Mozaika znacznie zostało uproszczone dzięki definicji tablic kilka opisujące punktów odpowiadające każdej z 10 cyfr i dwukropka:

```csharp
public partial class MainPage : ContentPage
{
    // Total dots horizontally and vertically.
    const int horzDots = 41;
    const int vertDots = 7;

    // 5 x 7 dot matrix patterns for 0 through 9.
    static readonly int[, ,] numberPatterns = new int[10, 7, 5]
    {
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 1, 1}, { 1, 0, 1, 0, 1}, 
            { 1, 1, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 0, 0}, { 0, 1, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0}, 
            { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, 
            { 0, 0, 1, 0, 0}, { 0, 1, 0, 0, 0}, { 1, 1, 1, 1, 1}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 0, 1, 0},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 0, 1, 0}, { 0, 0, 1, 1, 0}, { 0, 1, 0, 1, 0}, { 1, 0, 0, 1, 0}, 
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 0, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0}, { 0, 0, 0, 0, 1}, 
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 1, 0}, { 0, 1, 0, 0, 0}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0}, 
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0}, 
            { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}, 
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 1}, 
            { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 1, 1, 0, 0}
        },
    };

    // Dot matrix pattern for a colon.
    static readonly int[,] colonPattern = new int[7, 2]
    {
        { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }
    };

    // BoxView colors for on and off.
    static readonly Color colorOn = Color.Red;
    static readonly Color colorOff = new Color(0.5, 0.5, 0.5, 0.25);

    // Box views for 6 digits, 7 rows, 5 columns.
    BoxView[, ,] digitBoxViews = new BoxView[6, 7, 5];

    ···

}
```

Te pola zawierają z tablicą trójwymiarową z `BoxView` elementy do przechowywania wzorce kropka sześciu cyfr.

Konstruktor tworzy wszystkie `BoxView` elementy cyfr i dwukropek, a także inicjuje `Color` właściwość `BoxView` elementy dwukropkiem:

```csharp
public partial class MainPage : ContentPage
{

    ···

    public MainPage()
    {
        InitializeComponent();

        // BoxView dot dimensions.
        double height = 0.85 / vertDots;
        double width = 0.85 / horzDots;

        // Create and assemble the BoxViews.
        double xIncrement = 1.0 / (horzDots - 1);
        double yIncrement = 1.0 / (vertDots - 1);
        double x = 0;

        for (int digit = 0; digit < 6; digit++)
        {
            for (int col = 0; col < 5; col++)
            {
                double y = 0;

                for (int row = 0; row < 7; row++)
                {
                    // Create the digit BoxView and add to layout.
                    BoxView boxView = new BoxView();
                    digitBoxViews[digit, row, col] = boxView;
                    absoluteLayout.Children.Add(boxView,
                                                new Rectangle(x, y, width, height),
                                                AbsoluteLayoutFlags.All);
                    y += yIncrement;
                }
                x += xIncrement;
            }
            x += xIncrement;

            // Colons between the hours, minutes, and seconds.
            if (digit == 1 || digit == 3)
            {
                int colon = digit / 2;

                for (int col = 0; col < 2; col++)
                {
                    double y = 0;

                    for (int row = 0; row < 7; row++)
                    {
                        // Create the BoxView and set the color.
                        BoxView boxView = new BoxView
                            {
                                Color = colonPattern[row, col] == 1 ?
                                            colorOn : colorOff
                            };
                        absoluteLayout.Children.Add(boxView,
                                                    new Rectangle(x, y, width, height),
                                                    AbsoluteLayoutFlags.All);
                        y += yIncrement;
                    }
                    x += xIncrement;
                }
                x += xIncrement;
            }
        }

        // Set the timer and initialize with a manual call.
        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimer);
        OnTimer();
    }

    ···

}
```

Ten program używa względne położenie i funkcji zmiany rozmiaru `AbsoluteLayout`. Szerokość i wysokość każdego `BoxView` jest ustawiana wartość ułamkową, w szczególności 85% wynik dzielenia 1 przez liczbę punktów na poziomie i w pionie. Położenie również są ustawione na wartości ułamkowych. 

Ponieważ wszystkie pozycje i rozmiary są względem całkowity rozmiar `AbsoluteLayout`, `SizeChanged` obsługi dla strony należy ustawić tylko `HeightRequest` z `AbsoluteLayout`:

```csharp
public partial class MainPage : ContentPage
{
    
    ···
    
    void OnPageSizeChanged(object sender, EventArgs args)
    {
        // No chance a display will have an aspect ratio > 41:7
        absoluteLayout.HeightRequest = vertDots * Width / horzDots;
    }
    
    ···
    
}
```

Szerokość `AbsoluteLayout` jest ustawiany automatycznie, ponieważ jego rozciąga się do pełnej szerokości strony.

Kod końcowy w `MainPage` klasy przetwarza wywołanie zwrotne czasomierza i kolory punktów każdej cyfry. Definicja tablic wielowymiarowych na początku pliku CodeBehind ułatwia tę logikę najprostszym części programu:

```csharp
public partial class MainPage : ContentPage
{
   
    ···
 
    bool OnTimer()
    {
        DateTime dateTime = DateTime.Now;

        // Convert 24-hour clock to 12-hour clock.
        int hour = (dateTime.Hour + 11) % 12 + 1;

        // Set the dot colors for each digit separately.
        SetDotMatrix(0, hour / 10);
        SetDotMatrix(1, hour % 10);
        SetDotMatrix(2, dateTime.Minute / 10);
        SetDotMatrix(3, dateTime.Minute % 10);
        SetDotMatrix(4, dateTime.Second / 10);
        SetDotMatrix(5, dateTime.Second % 10);
        return true;
    }

    void SetDotMatrix(int index, int digit)
    {
        for (int row = 0; row < 7; row++)
            for (int col = 0; col < 5; col++)
            {
                bool isOn = numberPatterns[digit, row, col] == 1;
                Color color = isOn ? colorOn : colorOff;
                digitBoxViews[index, row, col].Color = color;
            }
    }
}
```
<a name="analogclock" />

## <a name="creating-an-analog-clock"></a>Tworzenie zegar analogowy

Zegar Mozaika mogą wydawać się oczywiste stosowania `BoxView`, ale `BoxView` elementy są także możliwość realizacji zegar analogowy:

[![Zegar BoxView](boxview-images/boxviewclock-small.png "zegara BoxView")](boxview-images/boxviewclock-large.png#lightbox "BoxView zegara")

Wszystkie elementy wizualne na [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/) programu są elementami podrzędnymi `AbsoluteLayout`. Te elementy są skonfigurowane przy użyciu `LayoutBounds` dołączona właściwość i obracać za pomocą `Rotation` właściwości. 

Trzy `BoxView` elementy ręce zegara w pliku XAML, ale nie znajduje się lub o rozmiarze:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BoxViewClock"
             x:Class="BoxViewClock.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <AbsoluteLayout x:Name="absoluteLayout"
                    SizeChanged="OnAbsoluteLayoutSizeChanged">
        
        <BoxView x:Name="hourHand"
                 Color="Black" />
        
        <BoxView x:Name="minuteHand"
                 Color="Black" />
        
        <BoxView x:Name="secondHand"
                 Color="Black" />
    </AbsoluteLayout>
</ContentPage>
```

Konstruktor obiektu pliku CodeBehind tworzy 60 `BoxView` elementów dla znaczników obwodzie zegara:

```csharp
public partial class MainPage : ContentPage
{
      
    ···
 
    BoxView[] tickMarks = new BoxView[60];

    public MainPage()
    {
        InitializeComponent();

        // Create the tick marks (to be sized and positioned later).
        for (int i = 0; i < tickMarks.Length; i++)
        {
            tickMarks[i] = new BoxView { Color = Color.Black };
            absoluteLayout.Children.Add(tickMarks[i]);
        }

        Device.StartTimer(TimeSpan.FromSeconds(1.0 / 60), OnTimerTick);
    }
  
    ···
 
}
```

Rozmiar i położenie wszystkich `BoxView` elementów odbywa się w `SizeChanged` obsługę `AbsoluteLayout`. Nieco struktury wewnętrznej do klasy o nazwie `HandParams` opisuje rozmiar każdego z trzech ręce względem całkowity rozmiar zegara:

```csharp
public partial class MainPage : ContentPage
{
    // Structure for storing information about the three hands.
    struct HandParams
    {
        public HandParams(double width, double height, double offset) : this()
        {
            Width = width;
            Height = height;
            Offset = offset;
        }

        public double Width { private set; get; }   // fraction of radius
        public double Height { private set; get; }  // ditto
        public double Offset { private set; get; }  // relative to center pivot
    }

    static readonly HandParams secondParams = new HandParams(0.02, 1.1, 0.85);
    static readonly HandParams minuteParams = new HandParams(0.05, 0.8, 0.9);
    static readonly HandParams hourParams = new HandParams(0.125, 0.65, 0.9);
 
    ···
 
 }
```

`SizeChanged` Obsługi określa Centrum i promień `AbsoluteLayout`, a następnie rozmiary i umieszcza 60 `BoxView` elementy używane jako znaczniki. `for` Pętli stwierdza, ustawiając `Rotation` właściwości każdego z tych `BoxView` elementów. Na koniec `SizeChanged` obsługi `LayoutHand` wywoływana jest metoda rozmiaru i pozycji trzech ręce zegara:

```csharp
public partial class MainPage : ContentPage
{
 
    ···
 
    void OnAbsoluteLayoutSizeChanged(object sender, EventArgs args)
    {
        // Get the center and radius of the AbsoluteLayout.
        Point center = new Point(absoluteLayout.Width / 2, absoluteLayout.Height / 2);
        double radius = 0.45 * Math.Min(absoluteLayout.Width, absoluteLayout.Height);

        // Position, size, and rotate the 60 tick marks.
        for (int index = 0; index < tickMarks.Length; index++)
        {
            double size = radius / (index % 5 == 0 ? 15 : 30);
            double radians = index * 2 * Math.PI / tickMarks.Length;
            double x = center.X + radius * Math.Sin(radians) - size / 2;
            double y = center.Y - radius * Math.Cos(radians) - size / 2;
            AbsoluteLayout.SetLayoutBounds(tickMarks[index], new Rectangle(x, y, size, size));
            tickMarks[index].Rotation = 180 * radians / Math.PI;
        }

        // Position and size the three hands.
        LayoutHand(secondHand, secondParams, center, radius);
        LayoutHand(minuteHand, minuteParams, center, radius);
        LayoutHand(hourHand, hourParams, center, radius);
    }

    void LayoutHand(BoxView boxView, HandParams handParams, Point center, double radius)
    {
        double width = handParams.Width * radius;
        double height = handParams.Height * radius;
        double offset = handParams.Offset;

        AbsoluteLayout.SetLayoutBounds(boxView,
            new Rectangle(center.X - 0.5 * width,
                          center.Y - offset * height,
                          width, height));

        // Set the AnchorY property for rotations.
        boxView.AnchorY = handParams.Offset;
    }
 
    ···
 
}
```

`LayoutHand` Metoda rozmiary i umieszcza każdego ręcznie, aby wskazywały bezpośrednio do pozycji 12:00. Na końcu metody `AnchorY` właściwość jest ustawiona na pozycji odpowiadającej Centrum zegara. To ustawienie określa środek obrotu.

Ręce są obracane w funkcji wywołanie zwrotne czasomierza:

```csharp
public partial class MainPage : ContentPage
{
 
    ···
     
    bool OnTimerTick()
    {
        // Set rotation angles for hour and minute hands.
        DateTime dateTime = DateTime.Now;
        hourHand.Rotation = 30 * (dateTime.Hour % 12) + 0.5 * dateTime.Minute;
        minuteHand.Rotation = 6 * dateTime.Minute + 0.1 * dateTime.Second;

        // Do an animation for the second hand.
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        secondHand.Rotation = 6 * (dateTime.Second + t);
        return true;
    }
}
```

Z drugiej strony jest traktowane nieco inaczej: zastosowania animacji wyjścia funkcji sterowania tempem aby przenoszenie prawdopodobnie mechaniczne zamiast smooth. Na każdym znaczników drugiej wskazówki pobiera nieco Wstecz i przekroczenia powierzchni miejsca docelowego. Ten niewielki kod dodaje znacznie wzrostu ruchu.

## <a name="conclusion"></a>Wniosek

`BoxView` Może wydawać się proste w pierwszej, ale jako użytkownik w tym samouczku, może być bardzo elastyczne i mogą prawie odtworzenia elementy wizualne są zwykle możliwe tylko w przypadku grafika wektorowa. Aby uzyskać dokładniejsze grafiki, zapoznaj się [przy użyciu SkiaSharp w platformy Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md).


## <a name="related-links"></a>Linki pokrewne

- [Podstawowe BoxView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [Dekoracji tekstu (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [Kolor ListBox (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [Gry życia (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [Zegar Mozaika (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [Zegar BoxView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)
