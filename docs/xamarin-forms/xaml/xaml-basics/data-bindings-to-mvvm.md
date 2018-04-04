---
title: Część 5. Z danych powiązania z modelem MVVM
description: Wzorzec architektury Model-View-ViewModel (MVVM) został opracowany z XAML pamiętać. Wzorzec wymusza rozdzielenie trzy warstwy oprogramowania — interfejsu użytkownika XAML, nazywany widoku; danych o nazwie modelu; i pośredniczący między widoku i modelu o nazwie ViewModel. Widok i ViewModel często są połączone za pośrednictwem powiązania danych zdefiniowanych w pliku XAML. BindingContext widoku jest zwykle wystąpienia ViewModel.
ms.prod: xamarin
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 95cd79a4bd6da47757cfeb12a2862ccb5a66fee2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>Część 5. Z danych powiązania z modelem MVVM

_Wzorzec architektury Model-View-ViewModel (MVVM) został opracowany z XAML pamiętać. Wzorzec wymusza rozdzielenie trzy warstwy oprogramowania — interfejsu użytkownika XAML, nazywany widoku; danych o nazwie modelu; i pośredniczący między widoku i modelu o nazwie ViewModel. Widok i ViewModel często są połączone za pośrednictwem powiązania danych zdefiniowanych w pliku XAML. BindingContext widoku jest zwykle wystąpienia ViewModel._

## <a name="a-simple-viewmodel"></a>Proste ViewModel

Jako wprowadzenie do ViewModels najpierw Przyjrzyjmy się program bez ani jednego.
Wcześniej pokazano, jak do definiowania nowych deklaracja obszaru nazw XML, aby zezwolić na pliku XAML klasy odwołania w innych zestawów. W tym miejscu to program, który definiuje deklaracji przestrzeni nazw XML dla `System` przestrzeni nazw:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Program może użyć `x:Static` uzyskać bieżącą datę i godzinę z statycznych `DateTime.Now` właściwości i ustawić `DateTime` do wartości `BindingContext` na `StackLayout`:

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext` jest to bardzo specjalne właściwość: podczas ustawiania `BindingContext` w elemencie jest dziedziczona przez wszystkie elementy podrzędne tego elementu. Oznacza to, że wszystkie elementy podrzędne elementu `StackLayout` mają ten sam `BindingContext`, i mogą zawierać proste powiązania właściwości tego obiektu.

W **One-Shot DateTime** program, dwa elementy podrzędne zawiera powiązań z właściwości tego `DateTime` wartość, ale dwa inne elementy podrzędne zawierają powiązania, które prawdopodobnie brakuje ścieżki powiązania. Oznacza to, że `DateTime` jest używana wartość samego `StringFormat`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.OneShotDateTimePage"
             Title="One-Shot DateTime Page">

    <StackLayout BindingContext="{x:Static sys:DateTime.Now}"
                 HorizontalOptions="Center"
                 VerticalOptions="Center">

        <Label Text="{Binding Year, StringFormat='The year is {0}'}" />
        <Label Text="{Binding StringFormat='The month is {0:MMMM}'}" />
        <Label Text="{Binding Day, StringFormat='The day is {0}'}" />
        <Label Text="{Binding StringFormat='The time is {0:T}'}" />

    </StackLayout>
</ContentPage>
```

Oczywiście duży problem jest, że data i godzina są zestaw po po pierwszym utworzeniu strony i nigdy nie zmiany:

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "Widok wyświetlanie daty i godziny")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "widoku wyświetlanie daty i godziny")

Plik XAML może wyświetlać zegara, który zawsze wyświetla bieżący czas, ale musi on kodu pomagać. Podczas planowania pod względem MVVM, modelu i ViewModel są klasy napisanych w całości w kodzie. Widok jest często pliku XAML, który odwołuje się do właściwości zdefiniowane w ViewModel za pośrednictwem powiązania danych.

Właściwego modelu jest ignorujących z ViewModel i odpowiednie ViewModel jest ignorujących widoku. Jednak bardzo często programisty dostosowanie typy danych udostępnianych przez ViewModel z typami danych skojarzonych z interfejsami określonego użytkownika. Na przykład jeśli Model uzyskuje dostęp do bazy danych, która zawiera znak 8-bitową ciągi ASCII, ViewModel potrzebny do konwersji między te ciągi do ciągów Unicode w celu uwzględnienia wyłączne użycie znaków Unicode w interfejsie użytkownika.

W proste przykłady MVVM (takich jak pokazano poniżej) często nie zostanie wzór na wszystkich wzorzec obejmuje tylko widoku i ViewModel związane z powiązaniami danych.

Oto ViewModel zegar z tylko jedną właściwość o nazwie `DateTime`, ale która aktualizuje `DateTime` właściwości w ciągu sekundy:

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    class ClockViewModel : INotifyPropertyChanged
    {
        DateTime dateTime;

        public event PropertyChangedEventHandler PropertyChanged;

        public ClockViewModel()
        {
            this.DateTime = DateTime.Now;

            Device.StartTimer(TimeSpan.FromSeconds(1), () =>
                {
                    this.DateTime = DateTime.Now;
                    return true;
                });
        }

        public DateTime DateTime
        {
            set
            {
                if (dateTime != value)
                {
                    dateTime = value;

                    if (PropertyChanged != null)
                    {
                        PropertyChanged(this, new PropertyChangedEventArgs("DateTime"));
                    }
                }
            }
            get
            {
                return dateTime;
            }
        }
    }
}
```

Zazwyczaj zaimplementować ViewModels `INotifyPropertyChanged` interfejsu, co oznacza, że klasa generowane `PropertyChanged` zdarzeń przy każdej zmianie jednej z jego właściwości. Mechanizm powiązania danych platformy Xamarin.Forms dołącza obsługi tej `PropertyChanged` zdarzeń, więc może otrzymać powiadomienie, gdy zmienia się właściwość i Zachowaj docelowego, zaktualizować przy użyciu nowej wartości.

Zegar oparte na tym ViewModel może być tak proste, jak to:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ClockPage"
             Title="Clock Page">

    <Label Text="{Binding DateTime, StringFormat='{0:T}'}"
           FontSize="Large"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Label.BindingContext>
            <local:ClockViewModel />
        </Label.BindingContext>
    </Label>
</ContentPage>
```

Powiadomienie jak `ClockViewModel` ustawiono `BindingContext` z `Label` przy użyciu tagów elementu właściwości. Alternatywnie można utworzyć wystąpienia `ClockViewModel` w `Resources` kolekcji i ustaw ją na `BindingContext` za pośrednictwem `StaticResource` — rozszerzenie znaczników. Lub instancje ViewModel można tworzyć pliku CodeBehind.

`Binding` — Rozszerzenie znaczników w `Text` właściwość `Label` formatów `DateTime` właściwości. Oto wyświetlania:

[![](data-bindings-to-mvvm-images/clock.png "Widok wyświetlanie daty i godziny za pośrednictwem ViewModel")](data-bindings-to-mvvm-images/clock-large.png#lightbox "widoku wyświetlanie daty i godziny za pośrednictwem ViewModel")

Istnieje również możliwość dostęp do poszczególnych właściwości `DateTime` właściwości ViewModel rozdzielając właściwości okresów:

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>Interakcyjne MVVM

MVVM jest dość często używane z powiązania danych dwukierunkowe interakcyjny widok oparte na odpowiedni model danych.

Oto klasę o nazwie `HslViewModel` konwertująca `Color` wartości do `Hue`, `Saturation`, i `Luminosity` wartości i na odwrót:

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    public class HslViewModel : INotifyPropertyChanged
    {
        double hue, saturation, luminosity;
        Color color;

        public event PropertyChangedEventHandler PropertyChanged;

        public double Hue
        {
            set
            {
                if (hue != value)
                {
                    hue = value;
                    OnPropertyChanged("Hue");
                    SetNewColor();
                }
            }
            get
            {
                return hue;
            }
        }

        public double Saturation
        {
            set
            {
                if (saturation != value)
                {
                    saturation = value;
                    OnPropertyChanged("Saturation");
                    SetNewColor();
                }
            }
            get
            {
                return saturation;
            }
        }

        public double Luminosity
        {
            set
            {
                if (luminosity != value)
                {
                    luminosity = value;
                    OnPropertyChanged("Luminosity");
                    SetNewColor();
                }
            }
            get
            {
                return luminosity;
            }
        }

        public Color Color
        {
            set
            {
                if (color != value)
                {
                    color = value;
                    OnPropertyChanged("Color");

                    Hue = value.Hue;
                    Saturation = value.Saturation;
                    Luminosity = value.Luminosity;
                }
            }
            get
            {
                return color;
            }
        }

        void SetNewColor()
        {
            Color = Color.FromHsla(Hue, Saturation, Luminosity);
        }

        protected virtual void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Zmienia się na `Hue`, `Saturation`, i `Luminosity` Przyczyna właściwości `Color` właściwości do zmiany, a zmiany `Color` powoduje, że trzech właściwości do zmiany. To może się wydawać nieskończoną pętlę, z wyjątkiem tego, że klasa nie wywołania `PropertyChanged` zdarzeń, chyba że rzeczywiste zmiany właściwości. Spowoduje to umieszczenie punktu końcowego do inaczej fluktuacje sprzężenia zwrotnego.

Następujący plik XAML zawiera `BoxView` których `Color` właściwość jest powiązana z `Color` właściwość ViewModel i trzech `Slider` i trzy `Label` widoków powiązany z `Hue`, `Saturation`i `Luminosity` właściwości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.HslColorScrollPage"
             Title="HSL Color Scroll Page">
    <ContentPage.BindingContext>
        <local:HslViewModel Color="Aqua" />
    </ContentPage.BindingContext>

    <StackLayout Padding="10, 0">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Hue, Mode=TwoWay}" />

        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Saturation, Mode=TwoWay}" />

        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Luminosity, Mode=TwoWay}" />
    </StackLayout>
</ContentPage>
```

Powiązanie w każdym `Label` jest ustawieniem domyślnym `OneWay`. Wymaga tylko do wyświetlania wartości. Ale powiązanie w każdym `Slider` jest `TwoWay`. Dzięki temu `Slider` zostać zainicjowany z ViewModel. Zwróć uwagę, że `Color` właściwość jest ustawiona na `Blue` po ViewModel zostanie uruchomiony. Jednak zmiana `Slider` musi również ustawić nową wartość dla właściwości w ViewModel, który następnie oblicza nowy kolor.

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "Przy użyciu powiązań danych dwustronny MVVM")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "przy użyciu powiązań danych dwustronny MVVM")

## <a name="commanding-with-viewmodels"></a>Steruje z ViewModels

W wielu przypadkach wzorzec MVVM jest ograniczony do manipulowania elementów danych: obiekty danych w ViewModel równoległe obiektów interfejsu użytkownika w widoku.

Jednak czasami widok musi zawierać przyciski wyzwalające różnego rodzaju akcje w ViewModel. Ale nie może zawierać ViewModel `Clicked` programy obsługi przycisków ponieważ czy który umożliwia powiązanie ViewModel do modelu konkretnego interfejsu użytkownika.

Aby umożliwić ViewModels do bardziej niezależne od obiektów interfejsu użytkownika, ale nadal możliwe metody do wywołania w ViewModel, *polecenia* istnieje interfejsu. Ten interfejs polecenia jest obsługiwany przez następujące elementy w platformy Xamarin.Forms:

-  `Button`
-  `MenuItem`
-  `ToolbarItem`
-  `SearchBar`
-  `TextCell` (a tym samym również `ImageCell`)
-  `ListView`
-  `TapGestureRecognizer`

Z wyjątkiem produktów `SearchBar` i `ListView` elemencie te elementy zdefiniowane dwie właściwości:

-  `Command` typu  `System.Windows.Input.ICommand`
-  `CommandParameter` typu  `Object`

`SearchBar` Definiuje `SearchCommand` i `SearchCommandParameter` właściwości, gdy `ListView` definiuje `RefreshCommand` właściwości typu `ICommand`.

`ICommand` Interfejs definiuje dwie metody i jedno zdarzenie:

-  `void Execute(object arg)`
-  `bool CanExecute(object arg)`
-  `event EventHandler CanExecuteChanged`

ViewModel można zdefiniować właściwości typu `ICommand`. Następnie możesz powiązać te właściwości, aby `Command` właściwości każdego `Button` lub innego elementu lub prawdopodobnie widok niestandardowy, który implementuje ten interfejs. Opcjonalnie możesz ustawić `CommandParameter` właściwości do identyfikowania poszczególnych `Button` obiektów (lub inne elementy) powiązanych do tej właściwości ViewModel. Wewnętrznie `Button` wywołania `Execute` metoda przy każdym naciśnięciu `Button`, przechodzącą do `Execute` metoda jego `CommandParameter`.

`CanExecute` — Metoda i `CanExecuteChanged` zdarzenia są używane w przypadku których `Button` naciśnij może być aktualnie jest nieprawidłowy, w którym to przypadku `Button` należy wyłączyć dla samej siebie. `Button` Wywołania `CanExecute` podczas `Command` zdefiniowanych właściwości i w razie `CanExecuteChanged` zdarzenie jest wywoływane. Jeśli `CanExecute` zwraca `false`, `Button` wyłączy się i nie generuje `Execute` wywołania.

Aby uzyskać pomoc przy dodawaniu ViewModels Twojego są droższe, platformy Xamarin.Forms definiuje dwie klasy, które implementują `ICommand`: `Command` i `Command<T>` gdzie `T` jest typem argumenty `Execute` i `CanExecute`. Te dwie klasy definiują kilka konstruktorów plus `ChangeCanExecute` metodę, która ViewModel można wywołać, aby wymusić `Command` obiektu uruchomienie `CanExecuteChanged` zdarzeń.

Oto ViewModel dla prostego klawiatury, przewidziany do wprowadzania numerów telefonów. Zwróć uwagę, że `Execute` i `CanExecute` metody są zdefiniowane jako lambda funkcji bezpośrednio w Konstruktorze:

```csharp
using System;
using System.ComponentModel;
using System.Windows.Input;
using Xamarin.Forms;

namespace XamlSamples
{
    class KeypadViewModel : INotifyPropertyChanged
    {
        string inputString = "";
        string displayText = "";
        char[] specialChars = { '*', '#' };

        public event PropertyChangedEventHandler PropertyChanged;

        // Constructor
        public KeypadViewModel()
        {
            AddCharCommand = new Command<string>((key) =>
                {
                    // Add the key to the input string.
                    InputString += key;
                });

            DeleteCharCommand = new Command(() =>
                {
                    // Strip a character from the input string.
                    InputString = InputString.Substring(0, InputString.Length - 1);
                },
                () =>
                {
                    // Return true if there's something to delete.
                    return InputString.Length > 0;
                });
        }

        // Public properties
        public string InputString
        {
            protected set
            {
                if (inputString != value)
                {
                    inputString = value;
                    OnPropertyChanged("InputString");
                    DisplayText = FormatText(inputString);

                    // Perhaps the delete button must be enabled/disabled.
                    ((Command)DeleteCharCommand).ChangeCanExecute();
                }
            }

            get { return inputString; }
        }

        public string DisplayText
        {
            protected set
            {
                if (displayText != value)
                {
                    displayText = value;
                    OnPropertyChanged("DisplayText");
                }
            }
            get { return displayText; }
        }

        // ICommand implementations
        public ICommand AddCharCommand { protected set; get; }

        public ICommand DeleteCharCommand { protected set; get; }

        string FormatText(string str)
        {
            bool hasNonNumbers = str.IndexOfAny(specialChars) != -1;
            string formatted = str;

            if (hasNonNumbers || str.Length < 4 || str.Length > 10)
            {
            }
            else if (str.Length < 8)
            {
                formatted = String.Format("{0}-{1}",
                                          str.Substring(0, 3),
                                          str.Substring(3));
            }
            else
            {
                formatted = String.Format("({0}) {1}-{2}",
                                          str.Substring(0, 3),
                                          str.Substring(3, 3),
                                          str.Substring(6));
            }
            return formatted;
        }

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Ta ViewModel przy założeniu, że `AddCharCommand` właściwość jest powiązana z `Command` właściwość kilka przycisków (lub jakikolwiek inny interfejs polecenia), z których każdy jest identyfikowany przez `CommandParameter`. Znaki, aby dodać tych przycisków `InputString` właściwość, która będzie sformatowany jako numer telefonu dla `DisplayText` właściwości.

Jest też druga właściwość typu `ICommand` o nazwie `DeleteCharCommand`. To jest powiązany z przycisku Wstecz spacji, ale przycisku powinna być wyłączona, jeśli nie są znaki do usunięcia.

Klawiaturze następujące jest nie jako wizualnie zaawansowane opcje jak jest to możliwe. Zamiast tego została zmniejszona znaczników do minimum, aby zademonstrować sposób użycia interfejsu polecenia:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.KeypadPage"
             Title="Keypad Page">

    <Grid HorizontalOptions="Center"
          VerticalOptions="Center">
        <Grid.BindingContext>
            <local:KeypadViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
        </Grid.ColumnDefinitions>

        <!-- Internal Grid for top row of items -->
        <Grid Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>

            <Frame Grid.Column="0"
                   OutlineColor="Accent">
                <Label Text="{Binding DisplayText}" />
            </Frame>

            <Button Text="&#x21E6;"
                    Command="{Binding DeleteCharCommand}"
                    Grid.Column="1"
                    BorderWidth="0" />
        </Grid>

        <Button Text="1"
                Command="{Binding AddCharCommand}"
                CommandParameter="1"
                Grid.Row="1" Grid.Column="0" />

        <Button Text="2"
                Command="{Binding AddCharCommand}"
                CommandParameter="2"
                Grid.Row="1" Grid.Column="1" />

        <Button Text="3"
                Command="{Binding AddCharCommand}"
                CommandParameter="3"
                Grid.Row="1" Grid.Column="2" />

        <Button Text="4"
                Command="{Binding AddCharCommand}"
                CommandParameter="4"
                Grid.Row="2" Grid.Column="0" />

        <Button Text="5"
                Command="{Binding AddCharCommand}"
                CommandParameter="5"
                Grid.Row="2" Grid.Column="1" />

        <Button Text="6"
                Command="{Binding AddCharCommand}"
                CommandParameter="6"
                Grid.Row="2" Grid.Column="2" />

        <Button Text="7"
                Command="{Binding AddCharCommand}"
                CommandParameter="7"
                Grid.Row="3" Grid.Column="0" />

        <Button Text="8"
                Command="{Binding AddCharCommand}"
                CommandParameter="8"
                Grid.Row="3" Grid.Column="1" />

        <Button Text="9"
                Command="{Binding AddCharCommand}"
                CommandParameter="9"
                Grid.Row="3" Grid.Column="2" />

        <Button Text="*"
                Command="{Binding AddCharCommand}"
                CommandParameter="*"
                Grid.Row="4" Grid.Column="0" />

        <Button Text="0"
                Command="{Binding AddCharCommand}"
                CommandParameter="0"
                Grid.Row="4" Grid.Column="1" />

        <Button Text="#"
                Command="{Binding AddCharCommand}"
                CommandParameter="#"
                Grid.Row="4" Grid.Column="2" />
    </Grid>
</ContentPage>
```

`Command` Właściwości pierwszego `Button` wyświetlany w tym znaczników jest powiązany z `DeleteCharCommand`; pozostałe są powiązane z `AddCharCommand` z `CommandParameter` który jest taki sam, jak znak, który znajduje się w `Button` twarzy na obrazie. Oto program w akcji:

[![](data-bindings-to-mvvm-images/keypad.png "Kalkulator za pomocą poleceń i MVVM")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "Kalkulator za pomocą poleceń i MVVM")

### <a name="invoking-asynchronous-methods"></a>Wywoływania metod asynchronicznych

Polecenia można także wywoływać metod asynchronicznych. Jest to realizowane za pośrednictwem `async` i `await` słów kluczowych podczas określania `Execute` metody:

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

Oznacza to, że `DownloadAsync` jest metoda `Task` i powinny być oczekiwane:

```csharp
async Task DownloadAsync ()
{
    await Task.Run (() => Download ());
}

void Download ()
{
    ...
}
```

## <a name="implementing-a-navigation-menu"></a>Implementowanie Menu nawigacji

[XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/) program, który zawiera kod źródłowy w tej serii artykułów używa ViewModel jego strony głównej. Ten ViewModel jest definicją klasy krótkich z trzech właściwości o nazwie `Type`, `Title`, i `Description` zawierające typ przykładowe strony, tytuł i krótki opis. Ponadto ViewModel definiuje właściwości statycznej o nazwie `All` będący kolekcją wszystkich stron w programie:

```csharp
public class PageDataViewModel
{
    public PageDataViewModel(Type type, string title, string description)
    {
        Type = type;
        Title = title;
        Description = description;
    }

    public Type Type { private set; get; }

    public string Title { private set; get; }

    public string Description { private set; get; }

    static PageDataViewModel()
    {
        All = new List<PageDataViewModel>
        {
            // Part 1. Getting Started with XAML
            new PageDataViewModel(typeof(HelloXamlPage), "Hello, XAML",
                                  "Display a Label with many properties set"),

            new PageDataViewModel(typeof(XamlPlusCodePage), "XAML + Code",
                                  "Interact with a Slider and Button"),

            // Part 2. Essential XAML Syntax
            new PageDataViewModel(typeof(GridDemoPage), "Grid Demo",
                                  "Explore XAML syntax with the Grid"),

            new PageDataViewModel(typeof(AbsoluteDemoPage), "Absolute Demo",
                                  "Explore XAML syntax with AbsoluteLayout"),

            // Part 3. XAML Markup Extensions
            new PageDataViewModel(typeof(SharedResourcesPage), "Shared Resources",
                                  "Using resource dictionaries to share resources"),

            new PageDataViewModel(typeof(StaticConstantsPage), "Static Constants",
                                  "Using the x:Static markup extensions"),

            new PageDataViewModel(typeof(RelativeLayoutPage), "Relative Layout",
                                  "Explore XAML markup extensions"),

            // Part 4. Data Binding Basics
            new PageDataViewModel(typeof(SliderBindingsPage), "Slider Bindings",
                                  "Bind properties of two views on the page"),

            new PageDataViewModel(typeof(SliderTransformsPage), "Slider Transforms",
                                  "Use Sliders with reverse bindings"),

            new PageDataViewModel(typeof(ListViewDemoPage), "ListView Demo",
                                  "Use a ListView with data bindings"),

            // Part 5. From Data Bindings to MVVM
            new PageDataViewModel(typeof(OneShotDateTimePage), "One-Shot DateTime",
                                  "Obtain the current DateTime and display it"),

            new PageDataViewModel(typeof(ClockPage), "Clock",
                                  "Dynamically display the current time"),

            new PageDataViewModel(typeof(HslColorScrollPage), "HSL Color Scroll",
                                  "Use a view model to select HSL colors"),

            new PageDataViewModel(typeof(KeypadPage), "Keypad",
                                  "Use a view model for numeric keypad logic")
        };
    }

    public static IList<PageDataViewModel> All { private set; get; } 
}
```

W pliku XAML `MainPage` definiuje `ListBox` których `ItemsSource` właściwość jest ustawiona na który `All` właściwości i który zawiera `TextCell` do wyświetlania `Title` i `Description` właściwości każdej strony:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage"
             Padding="5, 0"
             Title="XAML Samples">

    <ListView ItemsSource="{x:Static local:PageDataViewModel.All}"
              ItemSelected="OnListViewItemSelected">
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding Title}"
                          Detail="{Binding Description}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Strony są wyświetlane w listę:

[![](data-bindings-to-mvvm-images/mainpage.png "Przewijanej listy stron")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "przewijanej listy stron")

Program obsługi w pliku CodeBehind jest wyzwalane, gdy użytkownik wybiera element. Ustawia program obsługi `SelectedItem` właściwość `ListBox` do `null` tworzy wybranej strony i przechodzi do niej:

```csharp
private async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
{
    (sender as ListView).SelectedItem = null;

    if (args.SelectedItem != null)
    {
        PageDataViewModel pageData = args.SelectedItem as PageDataViewModel;
        Page page = (Page)Activator.CreateInstance(pageData.Type);
        await Navigation.PushAsync(page);
    }
}
```

## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/DYRLcqG2BAY]

**Xamarin rozwijać 2016: Uproszczona obsługa platformy Xamarin.Forms i biblioteki Prism modelem MVVM**

## <a name="summary"></a>Podsumowanie

XAML jest zaawansowanym narzędziem do definiowania interfejsy użytkownika w aplikacjach platformy Xamarin.Forms, zwłaszcza w przypadku wiązania danych i MVVM są używane. Wynik jest czysta, elegancki i potencjalnie biorąc reprezentacja interfejsu użytkownika z obsługą tła w kodzie.


## <a name="related-links"></a>Linki pokrewne

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Part 1. Część 1. Wprowadzenie do języka XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Part 2. Część 2. Podstawowa składnia języka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Part 3. Część 3. Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Part 4. Część 4. Powiązania danych — podstawy](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
