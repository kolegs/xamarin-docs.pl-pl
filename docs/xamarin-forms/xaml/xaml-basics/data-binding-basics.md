---
title: Część 4. Podstawowe informacje dotyczące powiązania danych
description: Powiązania danych Zezwalaj na właściwości dwa obiekty połączone, tak aby zmiany w jednym powoduje zmianę w innym. Jest to bardzo przydatne narzędzie, a podczas wiązania danych można definiować wyłącznie w kodzie XAML zawiera skróty i wygody. W rezultacie jest jednym z najważniejszych rozszerzeń znaczników w platformy Xamarin.Forms powiązania.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 342288C3-BB4C-4924-B178-72E112D777BA
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: a8adc0c16043048ec919f5a0f9f7c5ce25f08ef9
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733038"
---
# <a name="part-4-data-binding-basics"></a>Część 4. Podstawowe informacje dotyczące powiązania danych

_Powiązania danych Zezwalaj na właściwości dwa obiekty połączone, tak aby zmiany w jednym powoduje zmianę w innym. Jest to bardzo przydatne narzędzie, a podczas wiązania danych można definiować wyłącznie w kodzie XAML zawiera skróty i wygody. W rezultacie jest jednym z najważniejszych rozszerzeń znaczników w platformy Xamarin.Forms powiązania._

## <a name="data-bindings"></a>Powiązania danych

Powiązania danych połączenia właściwości dwóch obiektów o nazwie *źródła* i *docelowej*. W kodzie, wymagane są dwa kroki: `BindingContext` musi być ustawiona właściwość obiektu docelowego do obiektu źródłowego i `SetBinding` — metoda (często używane w połączeniu z `Binding` klasy) musi zostać wywołana można powiązać właściwości tego obiektu docelowego obiekt do właściwości obiektu źródłowego.

Właściwość target musi być właściwości możliwej do wiązania, co oznacza, że obiekt docelowy musi pochodzić od `BindableObject`. Dokumentację platformy Xamarin.Forms w trybie online wskazuje właściwości, które można powiązać właściwości. Właściwość `Label` takich jak `Text` jest skojarzony z właściwości możliwej do wiązania `TextProperty`.

W znaczniku, należy również wykonać tego samego dwa kroki, które są wymagane w kodzie, z wyjątkiem `Binding` — rozszerzenie znaczników ma miejsce z `SetBinding` wywołania i `Binding` klasy.

Jednak podczas definiowania powiązania danych w języku XAML, istnieje wiele sposobów, aby ustawić `BindingContext` obiektu docelowego. Czasami jest ustawiona w pliku CodeBehind czasami przy użyciu `StaticResource` lub `x:Static` — rozszerzenie znaczników i czasami jako zawartość `BindingContext` znaczniki elementu właściwości.

Powiązania są najczęściej używane Aby połączyć elementy wizualne programu z właściwego modelu danych, zazwyczaj w realizacji architektura MVVM (Model-View-ViewModel) zgodnie z opisem w [część 5. Z danych powiązania z modelem MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), ale inne scenariusze są możliwe.

## <a name="view-to-view-bindings"></a>Widok na widok powiązania

Można zdefiniować powiązania danych, aby powiązać właściwości z dwóch widoków na tej samej stronie. W takim przypadku należy ustawić `BindingContext` użycia obiektu docelowego `x:Reference` — rozszerzenie znaczników.

Oto pliku XAML, który zawiera `Slider` i dwa `Label` widoki, z których jeden jest obracana przez `Slider` wartość i drugiego, w którym wyświetlana `Slider` wartość:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderBindingsPage"
             Title="Slider Bindings Page">

    <StackLayout>
        <Label Text="ROTATION"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider` Zawiera `x:Name` atrybut, który odwołuje się do niego dwa `Label` widoków przy użyciu `x:Reference` — rozszerzenie znaczników.

`x:Reference` Powiązanie rozszerzenia definiuje właściwość o nazwie `Name` można ustawić na nazwę elementu, do którego istnieje odwołanie, w tym przypadku `slider`. Jednak `ReferenceExtension` klasy definiującej `x:Reference` definiuje również — rozszerzenie znaczników `ContentProperty` atrybutu dla `Name`, co oznacza, że nie jest jawnie wymagane. Tylko dla różnych pierwszy `x:Reference` obejmuje "Name =", ale nie w drugim:

```csharp
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

`Binding` — Rozszerzenie znaczników sam może mieć kilka właściwości, tak jak `BindingBase` i `Binding` klasy. `ContentProperty` Dla `Binding` jest `Path`, ale "Ścieżka =" część rozszerzenia znaczników można pominąć, jeśli ścieżka jest pierwszy element `Binding` — rozszerzenie znaczników. W pierwszym przykładzie przedstawiono "Ścieżka =", ale drugi przykład pominie:

```csharp
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Właściwości mogą znajdować się na jeden wiersz lub podzielony na wiele wierszy:

```csharp
Text="{Binding Value, 
               StringFormat='The angle is {0:F0} degrees'}"
```

Czy, niezależnie od jest wygodne.

Powiadomienie `StringFormat` właściwości w ciągu sekundy `Binding` — rozszerzenie znaczników. W platformy Xamarin.Forms, powiązania nie wykonuj żadnych niejawne konwersje typów, a jeśli konieczne jest wyświetlenie obiekt z systemem innym niż ciągu jako ciąg znaków musi Podaj konwerter typów, lub użyj `StringFormat`. W tle, statycznych `String.Format` metoda służy do implementowania `StringFormat`. To potencjalnie problem, ponieważ nawiasy klamrowe, które są również używane do ograniczania rozszerzenia znaczników wymagają specyfikacji formatowania .NET. Spowoduje to utworzenie ryzyko wprowadzenia w błąd analizatora języka XAML. Aby tego uniknąć, należy umieścić w pojedynczym cudzysłowie cały ciąg formatowania:

```csharp
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

W tym miejscu jest uruchomiony program:

[![](data-binding-basics-images/sliderbinding.png "Widok na widok powiązania")](data-binding-basics-images/sliderbinding-large.png#lightbox "powiązania widoku do widoku ")

## <a name="the-binding-mode"></a>Tryb wiązania 

Jeden widok może mieć powiązania danych w przypadku kilku jego właściwości. Jednak każdy widok może mieć tylko jeden `BindingContext`, dlatego wiele powiązań danych w tym widoku musi wszystkie odwołania właściwości tego samego obiektu.

Rozwiązanie to i inne problemy wymaga `Mode` właściwości, która jest ustawiona na członkiem `BindingMode` wyliczenie:

- `Default` 
- `OneWay` — wartości są przekazywane ze źródła do obiektu docelowego
- `OneWayToSource` — wartości są przekazywane z elementem docelowym ze źródłem
- `TwoWay` — wartości zostaną przeniesione obu kierunkach między źródłem a celem

Następujący program pokazuje jedno użycie wspólnej `OneWayToSource` i `TwoWay` powiązanie trybów. Cztery `Slider` widoki są przeznaczone do sterowania `Scale`, `Rotate`, `RotateX`, i `RotateY` właściwości `Label`. Początkowo wygląda tak, jakby tych czterech właściwości `Label` powinna być cele wiązania danych, ponieważ każdy jest ustawiany `Slider`. Jednak `BindingContext` z `Label` może być tylko jeden obiekt, a istnieją cztery różne suwaki.

Z tego powodu wszystkie powiązania są ustawiane w pozornie wstecz sposobów: `BindingContext` każdego z czterech suwaki ustawiono `Label`, i powiązania są ustawione na `Value` właściwości suwaki. Za pomocą `OneWayToSource` i `TwoWay` tryby, te `Value` właściwości można ustawić właściwości źródła, które są `Scale`, `Rotate`, `RotateX`, i `RotateY` właściwości `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderTransformsPage"
             Padding="5"
             Title="Slider Transforms Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <!-- Scaled and rotated Label -->
        <Label x:Name="label"
               Text="TEXT"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <!-- Slider and identifying Label for Scale -->
        <Slider x:Name="scaleSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="1" Grid.Column="0"
                Maximum="10"
                Value="{Binding Scale, Mode=TwoWay}" />

        <Label BindingContext="{x:Reference scaleSlider}"
               Text="{Binding Value, StringFormat='Scale = {0:F1}'}"
               Grid.Row="1" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for Rotation -->
        <Slider x:Name="rotationSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="2" Grid.Column="0"
                Maximum="360"
                Value="{Binding Rotation, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationSlider}"
               Text="{Binding Value, StringFormat='Rotation = {0:F0}'}"
               Grid.Row="2" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationX -->
        <Slider x:Name="rotationXSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="3" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationX, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationXSlider}"
               Text="{Binding Value, StringFormat='RotationX = {0:F0}'}"
               Grid.Row="3" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationY -->
        <Slider x:Name="rotationYSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="4" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationY, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationYSlider}"
               Text="{Binding Value, StringFormat='RotationY = {0:F0}'}"
               Grid.Row="4" Grid.Column="1"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Wiązania na trzech `Slider` widoki są `OneWayToSource`, co oznacza że `Slider` wartość powoduje zmianę we właściwości jego `BindingContext`, która jest `Label` o nazwie `label`. Te trzy `Slider` widoków powodują zmianę `Rotate`, `RotateX`, i `RotateY` właściwości `Label`.

Jednak powiązania dla `Scale` jest właściwość `TwoWay`. Jest to spowodowane `Scale` właściwość ma wartość domyślną 1 i przy użyciu `TwoWay` powiązanie przyczyny `Slider` początkowa wartość ustawiono na 1, a nie na 0. Jeśli zostały tego powiązania `OneWayToSource`, `Scale` właściwości początkowo może być równa 0 z `Slider` wartość domyślna. `Label` Nie będzie widoczny i które mogą powodować dezorientację użytkownika.

 [![](data-binding-basics-images/slidertransforms.png "Wstecz powiązania")](data-binding-basics-images/slidertransforms-large.png#lightbox "wstecz powiązania")

## <a name="bindings-and-collections"></a>Powiązania i kolekcji

Nic nie przedstawiono możliwości powiązania XAML i danych lepszym rozwiązaniem niż szablonem `ListView`.

`ListView` definiuje `ItemsSource` właściwość typu `IEnumerable`, i wyświetla elementy w tej kolekcji. Te elementy mogą być obiekty dowolnego typu. Domyślnie `ListView` używa `ToString` metody poszczególnych elementów do wyświetlenia tego elementu. Jest to po prostu to, czego potrzebujesz, ale w wielu przypadkach `ToString` zwraca tylko klasy w pełni kwalifikowaną nazwę obiektu.

Jednak elementów w `ListView` kolekcji można wyświetlić wszelkie sposób za pośrednictwem *szablonu*, która obejmuje klasą pochodzącą z `Cell`. Szablon został sklonowany dla każdej pozycji `ListView`, i powiązania danych, które zostały ustawione w szablonie są przenoszone do poszczególnych klony.

Bardzo często, należy utworzyć niestandardowe komórki dla tych elementów przy użyciu `ViewCell` klasy. Ten proces jest nieco lepiej w kodzie, ale w języku XAML staje się bardzo proste.

XamlSamples projektu zawiera klasy o nazwie `NamedColor`. Każdy `NamedColor` obiekt ma `Name` i `FriendlyName` właściwości typu `string`, a `Color` właściwości typu `Color`. Ponadto `NamedColor` 141 statycznego pola tylko do odczytu typu `Color` odpowiadający kolorów zdefiniowanych w platformy Xamarin.Forms `Color` klasy. Statyczny Konstruktor tworzy `IEnumerable<NamedColor>` kolekcji zawierającej `NamedColor` obiekty odpowiadające te pola statyczne i przypisuje go do jego publiczne statyczne `All` właściwości.

Ustawianie statycznego `NamedColor.All` właściwości `ItemsSource` z `ListView` łatwo przy użyciu `x:Static` — rozszerzenie znaczników:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

Wyświetlanie wynikowych ustanawia elementy są naprawdę typu `XamlSamples.NamedColor`:

[![](data-binding-basics-images/listview1.png "Tworzenie powiązania z kolekcją")](data-binding-basics-images/listview1-large.png#lightbox "tworzenia powiązania z kolekcją")

Nie jest wiele informacji, ale `ListView` jest przewijany i zaznaczania.

Aby zdefiniować szablon dla elementów, należy rozbicie `ItemTemplate` właściwość jako elementu właściwości i ustaw ją na `DataTemplate`, które następnie odwołania `ViewCell`. Aby `View` właściwość `ViewCell` można określić układ co najmniej jeden widok do wyświetlania każdego elementu. Poniżej przedstawiono prosty przykład:

```xaml
<ListView ItemsSource="{x:Static local:NamedColor.All}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.View>
                    <Label Text="{Binding FriendlyName}" />
                </ViewCell.View>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

`Label` Element jest ustawiony na wartość `View` właściwość `ViewCell`. ( `ViewCell.View` Tagi nie są wymagane, ponieważ `View` właściwość jest właściwość content `ViewCell`.) Wyświetla tego znacznika `FriendlyName` właściwości każdego `NamedColor` obiektu:

[![](data-binding-basics-images/listview2.png "Powiązanie do kolekcji zawierającej obiekt DataTemplate")](data-binding-basics-images/listview2-large.png#lightbox "powiązanie do kolekcji zawierającej obiekt DataTemplate")

Znacznie lepiej. Teraz wszystkie potrzebne jest świerk się szablon elementu więcej informacji i Kolor rzeczywisty. Aby zapewnić obsługę tego szablonu, niektóre wartości i obiekty zostały określone w słowniku zasobów strony:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <OnPlatform x:Key="boxSize"
                        x:TypeArguments="x:Double">
                <On Platform="iOS, Android, UWP" Value="50" />
            </OnPlatform>

            <OnPlatform x:Key="rowHeight"
                        x:TypeArguments="x:Int32">
                <On Platform="iOS, Android, UWP" Value="60" />
            </OnPlatform>

            <local:DoubleToIntConverter x:Key="intConverter" />

        </ResourceDictionary>
    </ContentPage.Resources>

    <ListView ItemsSource="{x:Static local:NamedColor.All}"
              RowHeight="{StaticResource rowHeight}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <StackLayout Padding="5, 5, 0, 5"
                                 Orientation="Horizontal"
                                 Spacing="15">

                        <BoxView WidthRequest="{StaticResource boxSize}"
                                 HeightRequest="{StaticResource boxSize}"
                                 Color="{Binding Color}" />

                        <StackLayout Padding="5, 0, 0, 0"
                                     VerticalOptions="Center">

                            <Label Text="{Binding FriendlyName}"
                                   FontAttributes="Bold"
                                   FontSize="Medium" />

                            <StackLayout Orientation="Horizontal"
                                         Spacing="0">
                                <Label Text="{Binding Color.R,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat='R={0:X2}'}" />

                                <Label Text="{Binding Color.G,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', G={0:X2}'}" />

                                <Label Text="{Binding Color.B,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', B={0:X2}'}" />
                            </StackLayout>
                        </StackLayout>
                    </StackLayout>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Zwróć uwagę na `OnPlatform` do definiowania rozmiar `BoxView` i wysokość `ListView` wierszy. Chociaż wartości dla wszystkich platform trzy są takie same, znaczników można łatwo dostosowane do innych wartości w celu dopasowania wyświetlania. 

## <a name="binding-value-converters"></a>Konwertery wartości powiązania

Poprzedni **pokaz ListView** pliku XAML Wyświetla poszczególne `R`, `G`, i `B` właściwości platformy Xamarin.Forms `Color` struktury. Te właściwości są typu `double` i z zakresu od 0 do 1. Jeśli chcesz wyświetlić wartości szesnastkowych, po prostu nie można użyć `StringFormat` z "X2" specyfikacji formatowania. Które działa tylko dla liczb całkowitych i oprócz, `double` wartości muszą zostać pomnożona przez 255.

Mały problem został rozwiązany z *konwerter wartości*, nazywany również *konwertera powiązania*. Jest to klasa implementująca `IValueConverter` interfejsu, co oznacza, że ma ona dwie metody o nazwie `Convert` i `ConvertBack`. `Convert` Metoda jest wywoływana, gdy wartość jest przenoszona z źródłowego do docelowego; `ConvertBack` wywoływana jest metoda transferów element docelowy źródła w `OneWayToSource` lub `TwoWay` powiązania:

```csharp
using System;
using System.Globalization;
using Xamarin.Forms;

namespace XamlSamples
{
    class DoubleToIntConverter : IValueConverter
    {
        public object Convert(object value, Type targetType,
                              object parameter, CultureInfo culture)
        {
            double multiplier;

            if (!Double.TryParse(parameter as string, out multiplier))
                multiplier = 1;

            return (int)Math.Round(multiplier * (double)value);
        }

        public object ConvertBack(object value, Type targetType,
                                  object parameter, CultureInfo culture)
        {
            double divider;

            if (!Double.TryParse(parameter as string, out divider))
                divider = 1;

            return ((double)(int)value) / divider;
        }
    }
}
```

`ConvertBack` — Metoda nie ma elementu role w tym programie ponieważ powiązania są tylko jednym ze sposobów ze źródła do docelowego. 

Konwerter powiązania z odwołuje się powiązanie `Converter` właściwości. Konwerter powiązanie również może akceptować określony za pomocą parametru `ConverterParameter` właściwości. W przypadku niektórych wszechstronność jest jak określono mnożnik. Konwerter powiązanie sprawdza parametru konwertera dla prawidłowej `double` wartość.

Konwerter zostanie uruchomiony w słowniku zasobów, aby mogły być udostępniane między wiele powiązań:

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

Powiązania danych trzy odwołania to pojedyncze wystąpienie. Zwróć uwagę, że `Binding` — rozszerzenie znaczników zawiera osadzony `StaticResource` — rozszerzenie znaczników:

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

W tym miejscu jest wynikiem:

[![](data-binding-basics-images/listview3.png "Powiązanie do kolekcji zawierającej obiekt DataTemplate i konwertery")](data-binding-basics-images/listview3-large.png#lightbox "tworzenia powiązania z kolekcją DataTemplate i konwertery")

`ListView` Jest bardzo zaawansowane podczas obsługi zmian, które mogą wystąpić dynamicznie w danych źródłowych, ale tylko wtedy, jeśli wykonanie pewnych dodatkowych kroków. Jeśli kolekcja elementów przypisanych `ItemsSource` właściwość `ListView` zmiany w czasie wykonywania — które, jeśli elementy mogą być dodawane do lub usunięty z kolekcji — użyj `ObservableCollection` klasy dla tych elementów. `ObservableCollection` implementuje `INotifyCollectionChanged` interfejsu i `ListView` zainstaluje program obsługi `CollectionChanged` zdarzeń.

Jeśli zmiana właściwości same elementy w czasie wykonywania, a następnie elementów w kolekcji należy zaimplementować `INotifyPropertyChanged` interfejsu i sygnału zmiany wartości właściwości przy użyciu `PropertyChanged` zdarzeń. To jest przedstawiona w następnej części tej serii [część 5. Powiązanie z modelem MVVM danych](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Podsumowanie

Powiązania danych Podaj zaawansowany mechanizm łączenie właściwości między dwoma obiektami na stronie lub między obiektami visual i podstawowych danych. Jednak po rozpoczęciu pracy ze źródłami danych aplikacji wzorzec architektury popularnych aplikacji zaczyna wyłaniać jako przydatne modelu. Ten temat znajdują się w [część 5. Z danych powiązania z modelem MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).



## <a name="related-links"></a>Linki pokrewne

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Part 1. Wprowadzenie do języka XAML (przykład)](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Part 2. Istotne składni języka XAML (przykład)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Part 3. Rozszerzenia znaczników XAML (przykład)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Part 5. Z danych powiązanych z modelem MVVM (przykład)](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
