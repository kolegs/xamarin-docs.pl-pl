---
title: BoxView zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak używać jako kolorowy prostokąt dla dekoracji, grafikę i interakcję w aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2017
ms.openlocfilehash: 813a913c2c2fb27456c9a489c73b16d5892c4b8d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997055"
---
# <a name="xamarinforms-boxview"></a>BoxView zestawu narzędzi Xamarin.Forms

[`BoxView`](xref:Xamarin.Forms.BoxView) renderuje prosty prostokąt określona szerokość, wysokość i kolor. Możesz użyć `BoxView` dekoracji podstawowe grafiki i interakcji z użytkownikiem za pośrednictwem touch.

Ponieważ Xamarin.Forms nie ma wbudowanych wektor grafiki systemu, `BoxView` pomaga w celu kompensacji. Niektóre z przykładowych programów opisane w tym artykule `BoxView` do renderowania grafiki. `BoxView` Można rozmiar tak, aby przypominały wiersza o określonej szerokości i grubości, a następnie obracać o dowolnym kąta przy użyciu `Rotation` właściwości.

Mimo że `BoxView` można naśladować grafiki proste, możesz chcieć zbadać [przy użyciu SkiaSharp w interfejsie Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) dla bardziej zaawansowanych wymagań grafiki.

W tym artykule omówiono następujące tematy:

- **[Ustawienie BoxView kolor i rozmiar](#colorandsize)**  &ndash; ustaw `BoxView` właściwości.
- **[Dekoracje tekstu renderowania](#textdecorations)**  &ndash; użyj `BoxView` linii renderowania.
- **[Wyświetlanie kolorów BoxView](#listingcolors)**  &ndash; Wyświetl wszystko kolorów systemowych w `ListView`.
- **[Odtwarzanie gry życia przez podklasy BoxView](#subclassing)**  &ndash; zaimplementować sławę niedeterministycznej sieci komórkowej.
- **[Tworzenie zegar cyfrowy](#digitalclock)**  &ndash; symulować wyświetlania Mozaika —.
- **[Tworzenie z zegara analogowy](#analogclock)**  &ndash; Przekształć go, animować i `BoxView` elementów.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>Ustawienie BoxView kolor i rozmiar

Bardzo często będzie zestawu następujących trzech właściwości `BoxView`:

- [`Color`](xref:Xamarin.Forms.BoxView.Color) Aby ustawić jej kolor.
- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) Aby ustawić szerokość `BoxView` w jednostkach niezależnych od urządzenia.
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) Aby ustawić wysokość `BoxView`.

`Color` Właściwość jest typu `Color`; można ustawić właściwości do dowolnego `Color` wartość, tym 141 statycznego pola tylko do odczytu elementu o nazwie kolorów alfabetycznie od `AliceBlue` do `YellowGreen`.

`WidthRequest` i `HeightRequest` właściwości tylko pełnić roli, jeśli `BoxView` jest *nieograniczonego* w układzie. Dotyczy to sytuacji, gdy kontener układu musi wiedzieć, podrzędne jego rozmiar, na przykład, kiedy `BoxView` jest elementem podrzędnym Rozmiar automatyczny komórki w `Grid` układu. A `BoxView` jest również nieograniczonego podczas jego `HorizontalOptions` i `VerticalOptions` właściwości są ustawiane na wartości innych niż `LayoutOptions.Fill`. Jeśli `BoxView` jest nieograniczony, ale `WidthRequest` i `HeightRequest` właściwości nie są ustawione, a następnie szerokość lub wysokość, które są ustawione na wartości domyślne 40 jednostek lub około 1/4 cala na urządzeniach przenośnych.

`WidthRequest` i `HeightRequest` właściwości są ignorowane w przypadku `BoxView` jest *ograniczonego* w układzie, w których przypadku kontener układu narzuca swój własny rozmiar `BoxView`.

Element `BoxView` może być ograniczona w jednym wymiarze i nieograniczonego w innym. Na przykład jeśli `BoxView` jest elementem podrzędnym pionowej `StackLayout`, pionowy wymiar `BoxView` jest nieograniczonego i jego płaszczyźnie poziomej ogólnie jest ograniczony. Ale istnieją wyjątki dla tego wymiaru poziomy: Jeśli `BoxView` ma jego `HorizontalOptions` ustawioną na coś innego niż `LayoutOptions.Fill`, poziomy wymiar jest również nieograniczone. Istnieje również możliwość dla `StackLayout` do masz nieograniczone płaszczyźnie poziomej, w którym to przypadku `BoxView` będzie również poziomo nieograniczone.

[ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView) przykład przedstawia jeden calowy kwadrat nieograniczonego `BoxView` na środku strony:

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

Poniżej przedstawiono wyniki:

[![Podstawowe BoxView](boxview-images/basicboxview-small.png "BoxView podstawowe")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

Jeśli `VerticalOptions` i `HorizontalOptions` właściwości są usuwane z `BoxView` tag lub jest stosowana `Fill`, a następnie `BoxView` staje się zależy od rozmiaru strony, a następnie rozwija do wypełnienia strony.

A `BoxView` również może być obiektem podrzędnym obiektu `AbsoluteLayout`. W tym przypadku, lokalizację i rozmiar `BoxView` są ustawiane przy użyciu `LayoutBounds` dołączonych właściwości możliwej do wiązania. `AbsoluteLayout` Opisanej w artykule [ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md).

Zobaczysz przykłady wszystkich tych przypadkach przykładowych programów, które należy wykonać.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>Renderowanie dekoracje tekstu

Możesz użyć `BoxView` można dodać kilka prostych dekoracje na stronach w formie poziome i pionowe linie. [ **Textdecoration —** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration) przykład pokazuje to. Wszystkie wizualizacje programu są zdefiniowane w **MainPage.xaml** pliku, który zawiera kilka `Label` i `BoxView` elementów w `StackLayout` pokazano poniżej:

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

Wszystkie znaczniki, który następuje po są elementami podrzędnymi `StackLayout`. Ten kod znaczników składa się z kilku typów dekoracyjnych `BoxView` elementy używane z `Label` elementu:

[![Dekoracja tekstu](boxview-images/textdecoration-small.png "dekoracji tekstu")](boxview-images/textdecoration-large.png#lightbox "dekoracji tekstu")

Stylowy nagłówka w górnej części strony jest osiągana za pomocą `AbsoluteLayout` którego elementy podrzędne są cztery `BoxView` elementy i `Label`, wszystkie są przypisane do określonych lokalizacji i rozmiary:

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

W pliku XAML `AbsoluteLayout` następuje `Label` z sformatowany tekst, który opisuje `AbsoluteLayout`.

Ciąg tekstowy może podkreślenie, umieszczając zarówno `Label` i `BoxView` w `StackLayout` zawierający jego `HorizontalOptions` ustawioną na coś innego niż `Fill`. Szerokość `StackLayout` jest określany przez szerokość `Label`, który następnie narzuca tego szerokość `BoxView`. `BoxView` Jest przypisywana tylko jawne height:

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

Nie można Użyj tej techniki, aby podkreślić poszczególnych wyrazów w ramach dłuższego ciągów tekstowych lub akapitu.

Istnieje również możliwość użycia `BoxView` przypominają HTML `hr` — element (poziomą). Po prostu umożliwiają szerokość `BoxView` określane przez jej kontenera nadrzędnego, w tym przypadku jest `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

Na koniec Rysowanie linii pionowej na jednej stronie akapitu tekstu umieszczając zarówno `BoxView` i `Label` poziomej `StackLayout`. W tym przypadku wysokość `BoxView` jest taka sama jak wysokość `StackLayout`, którym podlega wysokość `Label`:

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
</StackLayout>
```
<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>Wyświetlanie listy i BoxView kolorów

`BoxView` Jest wygodne w przypadku wyświetlania kolorów. Ten program używa `ListView` Aby wyświetlić wszystkie publiczne statyczne tylko do odczytu pola Xamarin.Forms `Color` strukturę:

[![Kolory ListView](boxview-images/listviewcolors-small.png "kolory ListView")](boxview-images/listviewcolors-large.png#lightbox "kolory ListView")

[ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/) program obejmuje klasę o nazwie `NamedColor`. Statyczny Konstruktor używa odbicia, aby uzyskać dostęp do wszystkich pól z `Color` struktury i utworzyć `NamedColor` obiekt dla każdego z nich. Są one przechowywane w statycznej `All` właściwości:

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

Wizualizacje programu są opisane w pliku XAML. `ItemsSource` Właściwość `ListView` jest ustawiona na statyczną `NamedColor.All` właściwości, co oznacza, że `ListView` Wyświetla wszystkie osoby `NamedColor` obiektów:

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

`NamedColor` Obiekty są sformatowane według `ViewCell` obiekt, który jest ustawiony jako szablon danych `ListView`. Ten szablon obejmuje `BoxView` którego `Color` właściwość jest powiązana z `Color` właściwość `NamedColor` obiektu.

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>Gry życia przez BoxView podklasy

Gra użytkowania jest komórkowej usługi Automation, opracowana przez mathematician Conway Jan i spopularyzowany na stronach *naukowych American* w 1970s. Wprowadzenie dobre są dostarczane przez artykułu w Wikipedii [potwierdziło gry życia](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

Xamarin.Forms [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/) program definiuje klasę o nazwie `LifeCell` który pochodzi od klasy `BoxView`. Ta klasa hermetyzuje logikę pojedyncze komórki w grze życiu:

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

`LifeCell` dodaje trzy więcej właściwości do `BoxView`: `Col` i `Row` właściwości przechowywania pozycja komórkę w siatce i `IsAlive` właściwość wskazuje jej stan. `IsAlive` Ustawia również właściwość `Color` właściwość `BoxView` na czarny, jeśli komórka jest aktywny i biały, jeśli komórka nie jest aktywny.

`LifeCell` można je również instalować `TapGestureRecognizer` aby umożliwić użytkownikowi do przełączenia stanu komórek, naciskając je. Klasa tłumaczy `Tapped` zdarzenie z aparat rozpoznawania gestów do jego własnej `Tapped` zdarzeń.

**GameOfLife** obejmuje również program `LifeGrid` klasę, która hermetyzuje znaczną część logiki gry i `MainPage` klasa, która obsługuje program wizualizacji. Obejmują one nakładce, która opisuje reguły gry. Poniżej przedstawiono program w działaniu przedstawiający kilka kilkaset `LifeCell` obiektów na stronie:

[![Gra cyklu życia](boxview-images/gameoflife-small.png "gry cyklu życia")](boxview-images/gameoflife-large.png#lightbox "gry cyklu życia")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>Tworzenie zegar cyfrowy

[ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/) program tworzy 210 `BoxView` elementy do symulacji punktów stara wyświetlania Mozaika 5, 7. Może odczytywać czasu w trybie pionowej lub poziomej, ale jest większa w widoku poziomym:

[![Zegar Mozaika](boxview-images/dotmatrixclock-small.png "zegara Mozaika")](boxview-images/dotmatrixclock-large.png#lightbox "Mozaika zegara")

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

Wszystkie inne odbywa się w pliku CodeBehind. Mozaika logikę wyświetlania jest znacznie uproszczone zgodnie z definicją kilka tablic, opisujących, kropki, odpowiadające każdemu z 10 cyfr, a dwukropka:

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

Te pola zawierają z tablicą trójwymiarową `BoxView` elementy do przechowywania wzorce kropka sześć cyfr.

Konstruktor tworzy wszystkie `BoxView` elementy cyfr i dwukropek, a także inicjuje `Color` właściwość `BoxView` elementy dwukropek:

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

Ten program używa pozycjonowanie względne i funkcji zmiany rozmiaru `AbsoluteLayout`. Szerokość i wysokość każdego `BoxView` są ustawione na wartości ułamkowych, w szczególności 85% wynik dzielenia 1 przez liczbę punktów poziome i pionowe. Położenie również są ustawione na wartości ułamkowe.

Ponieważ wszystkie pozycje i rozmiary są względne wobec całkowity rozmiar `AbsoluteLayout`, `SizeChanged` obsługi dla strony należy ustawić tylko `HeightRequest` z `AbsoluteLayout`:

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

Szerokość `AbsoluteLayout` zostanie automatycznie ustawiony, ponieważ jej rozciąga się do pełnej szerokości strony.

Końcowe kodu w `MainPage` klasy przetwarza czasomierza wywołania zwrotnego i kolory punktów każdej cyfry. Definicja tablic wielowymiarowych na początku pliku związanego z kodem ułatwia tę logikę najprostszym części programu:

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

Zegar Mozaika mogą wydawać się oczywiste stosowania `BoxView`, ale `BoxView` elementów również są w stanie zawierającemu zegar analogowy:

[![Zegar BoxView](boxview-images/boxviewclock-small.png "zegara BoxView")](boxview-images/boxviewclock-large.png#lightbox "BoxView zegara")

Wszystkie wizualizacje w [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/) programu są elementami podrzędnymi `AbsoluteLayout`. Te elementy są skonfigurowane przy użyciu `LayoutBounds` dołączona właściwość i obracać za pomocą `Rotation` właściwości.

Trzy `BoxView` elementy ręce zegara tworzone w pliku XAML, ale nie są umieszczone lub wielkości:

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

Konstruktor obiektu pliku związanego z kodem tworzy 60 `BoxView` elementy znaczników obwodzie zegara:

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

Zmiana rozmiaru i pozycjonowania wszystkich `BoxView` elementów odbywa się w `SizeChanged` Obsługa `AbsoluteLayout`. Nieco struktury wewnętrznej do klasy o nazwie `HandParams` opisuje rozmiar każdego z trzech ręce względem całkowity rozmiar zegara:

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

`SizeChanged` Obsługi określa Centrum i promień `AbsoluteLayout`, następnie rozmiarów i umieszcza 60 `BoxView` elementy służące jako znaczniki. `for` Pętli stwierdza, ustawiając `Rotation` właściwości każdego z tych `BoxView` elementów. Na koniec `SizeChanged` obsługi `LayoutHand` metoda jest wywoływana, aby rozmiar i położenie trzy ręce zegara:

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

`LayoutHand` Metoda rozmiarów i umieszcza każdego ręcznie, aby wskazać bezpośrednio do pozycji 12:00. Na końcu metody `AnchorY` właściwość jest ustawiona na pozycji odpowiadającej środek zegara. Oznacza to, środek obrotu.

Wskazówki są obracane w funkcji wywołania zwrotnego czasomierza:

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

Sekund jest traktowany jako nieco inaczej: zastosowania animacji, funkcja sterowania tempem zmian umożliwiają przenoszenie wydają się mechanicznych zamiast smooth. Na każdy takt drugiej wskazówki ściąga nieco ponownie, a następnie przekroczenia powierzchni miejsca docelowego. Ten niewielki kod dodaje wiele realizmu przepływu.

## <a name="conclusion"></a>Wniosek

`BoxView` Mogą wydawać się proste w pierwszym, ale możesz w tym samouczku, może być dość wszechstronne i można niemal odtworzenia elementy wizualne, które są zwykle jest możliwe tylko w przypadku grafika wektorowa. Dla bardziej zaawansowanych grafiki, zapoznaj się z [przy użyciu SkiaSharp w interfejsie Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md).


## <a name="related-links"></a>Linki pokrewne

- [Podstawowe BoxView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [Dekoracji tekstu (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [Kolor ListBox (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [Gra cyklu życia (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [Zegar Mozaika (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [Zegar BoxView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](xref:Xamarin.Forms.BoxView)
