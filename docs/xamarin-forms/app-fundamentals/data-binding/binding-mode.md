---
title: Tryb powiązania zestawu narzędzi Xamarin.Forms
description: W tym artykule opisano sposób sterowania przepływem informacji w elemencie źródłowym i docelowym przy użyciu trybu powiązania, który jest określony za pomocą jest członkiem wyliczenia BindingMode. Dla każdej możliwej do wiązania właściwości ma domyślny tryb powiązania, co oznacza tryb obowiązuje w przypadku tej właściwości jest ona lokalizacją docelową powiązanie danych.
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: a6eaf08d17f70c43f451361e27555a09c39f26a9
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935670"
---
# <a name="xamarinforms-binding-mode"></a>Tryb powiązania zestawu narzędzi Xamarin.Forms

W [poprzednim artykule](basic-bindings.md), **powiązanie kodu zamiast** i **powiązanie XAML alternatywna** stron polecane `Label` z jego `Scale` właściwości powiązany z `Value` właściwość `Slider`. Ponieważ `Slider` początkowa wartość to 0, to spowodowane `Scale` właściwość `Label` należy ustawić na 0, a nie 1, a `Label` zniknął.

W [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) próbki, **odwrotnego powiązania** strony jest podobny do programów w poprzednim artykule, z tą różnicą, że powiązanie danych jest zdefiniowany w `Slider` , a nie na `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.ReverseBindingPage"
             Title="Reverse Binding">
    <StackLayout Padding="10, 0">

        <Label x:Name="label"
               Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                VerticalOptions="CenterAndExpand"
                Value="{Binding Source={x:Reference label},
                                Path=Opacity}" />
    </StackLayout>
</ContentPage>
```

Na początku, to może wydawać się wstecz: teraz `Label` jest źródłem powiązanie danych oraz `Slider` jest elementem docelowym. Odwołań do powiązań `Opacity` właściwość `Label`, który ma wartość domyślną 1.

Zgodnie z oczekiwaniami, `Slider` jest ustawiana na wartość 1, z początkowego `Opacity` wartość `Label`. Jest to przedstawione na zrzucie ekranu systemu iOS po lewej stronie:

[![Odwróć powiązania](binding-mode-images/reversebinding-small.png "odwrotnego powiązania")](binding-mode-images/reversebinding-large.png#lightbox "odwrotnego powiązania")

Jednak może być oczekiwano, że `Slider` będzie nadal działać, ponieważ pokazują, zrzuty ekranu dla systemów Android i platformy uniwersalnej systemu Windows. Zajmuje to nieco sugeruje, że powiązanie danych działa lepiej, gdy `Slider` jest cel wiążący zamiast `Label` ponieważ inicjalizacja działa jak moglibyśmy oczekiwać.

Różnica między **odwrotnego powiązania** próbki i wcześniejszych przykładów obejmuje *tryb powiązania*.

## <a name="the-default-binding-mode"></a>Domyślny tryb powiązania

Określono tryb powiązanie z elementem członkowskim [ `BindingMode` ](xref:Xamarin.Forms.BindingMode) wyliczenia:

- [`Default`](xref:Xamarin.Forms.BindingMode.Default)
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) &ndash; dane są umieszczane obu kierunkach między źródłem a celem
- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) &ndash; dane są umieszczane ze źródła do docelowego
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; dane są umieszczane w docelowym ze źródłem
- [`OneTime`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; dane będą wysyłane ze źródła do docelowego, ale tylko wtedy, gdy `BindingContext` zmiany (nowe przy użyciu zestawu narzędzi Xamarin.Forms 3.0)

Dla każdej możliwej do wiązania właściwości ma domyślny tryb, który jest ustawiony, po utworzeniu właściwości możliwej do wiązania powiązania i który jest dostępny z [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) właściwość `BindableProperty` obiektu. To domyślny tryb powiązania wskazuje tryb obowiązuje, gdy ta właściwość jest ona lokalizacją docelową powiązanie danych.

Domyślny tryb powiązania większość właściwości, takie jak `Rotation`, `Scale`, i `Opacity` jest `OneWay`. Jeśli te właściwości są cele powiązanie danych, właściwość docelowego ma wartość ze źródła.

Jednakże domyślny tryb powiązania `Value` właściwość `Slider` jest `TwoWay`. Oznacza to, że w przypadku `Value` właściwości jest celu wiązania danych, a następnie element docelowy jest ustawiony (standardowe) ze źródła, ale źródła znajduje się również z obiektu docelowego. Jest to, co umożliwia `Slider` należy ustawić z początkowego `Opacity` wartość.

To powiązanie dwukierunkowe mogą wydawać się utworzyć wejścia w nieskończoną pętlę, ale nie mają miejsce. Właściwości możliwe do wiązania sygnalizuje zmianę właściwości, chyba że faktycznie zmienia właściwość. Zapobiega to nieskończoną pętlę.

### <a name="two-way-bindings"></a>Powiązania dwukierunkowego

Najbardziej możliwej do wiązania właściwości mają domyślny tryb powiązania z `OneWay` , ale następujące właściwości ma domyślny tryb powiązania z `TwoWay`:

- `Date` Właściwość `DatePicker`
- `Text` Właściwość `Editor`, `Entry`, `SearchBar`, i `EntryCell`
- `IsRefreshing` Właściwość `ListView`
- `SelectedItem` Właściwość `MultiPage`
- `SelectedIndex` i `SelectedItem` właściwości `Picker`
- `Value` Właściwość `Slider` i `Stepper`
- `IsToggled` Właściwość `Switch`
- `On` Właściwość `SwitchCell`
- `Time` Właściwość `TimePicker`

Te właściwości określonego są definiowane jako `TwoWay` bardzo dobre przyczyny:

Podczas używania powiązań danych przy użyciu architektury Model-View-ViewModel (MVVM) w aplikacji, klas ViewModel jest źródle powiązanie danych oraz widoku, który składa się z widoków, takich jak `Slider`, są cele powiązanie danych. Powiązania MVVM przypominają **odwrotnego powiązania** przykładowe więcej niż powiązania w poprzednich przykładach. Jest bardzo prawdopodobne, że chcesz, aby każdy widok na stronie, aby można zainicjować przy użyciu wartości odpowiednich właściwości ViewModel, ale zmiany w widoku również wpływać na właściwość ViewModel.

Właściwości, za pomocą domyślnego powiązania tryby `TwoWay` tych właściwości, które są najbardziej mogą być używane w scenariuszach MVVM.

### <a name="one-way-to-source-bindings"></a>Powiązania jeden-Way-do Source

Tylko do odczytu, które można powiązać właściwości mają domyślny tryb powiązania z `OneWayToSource`. Istnieje tylko jeden właściwości możliwej do wiązania odczytu/zapisu, która ma domyślny tryb powiązania z `OneWayToSource`:

- `SelectedItem` Właściwość `ListView`

Uzasadnienie jest powiązaniu w elemencie `SelectedItem` właściwość powinna być rozwiązywana we ustawienie źródło wiążące. Przykładem w dalszej części tego artykułu zastąpienia tego zachowania.

### <a name="one-time-bindings"></a>Jednorazowe powiązania

Kilka właściwości mają domyślny tryb powiązania z `OneTime`. Są to:

- `IsTextPredictionEnabled` Właściwość `Entry`
- `Text`, `BackgroundColor`, i `Style` właściwości `Span`.

Docelowe właściwości z trybu wiązania `OneTime` są aktualizowane tylko wtedy, gdy zmieni się kontekstu powiązania. Dla powiązania na te właściwości obiektu docelowego upraszcza to infrastruktura powiązań, ponieważ nie jest niezbędne do monitorowania zmian właściwości źródła.

## <a name="viewmodels-and-property-change-notifications"></a>Modele widoków i powiadomień o zmianie właściwości

**Prosty selektor kolorów** strony zademonstrowano użycie prostego ViewModel. Powiązania danych Zezwalaj użytkownikowi na wybranie koloru przy użyciu trzech `Slider` elementy na odcień, nasycenie i jasność.

ViewModel jest źródłem powiązanie danych. Jest ViewModel *nie* zdefiniować właściwości możliwej do wiązania, ale go zaimplementować mechanizm powiadomień, który umożliwia infrastruktura powiązań otrzymywać powiadomienia po zmianie wartości właściwości. Ten mechanizm powiadomień jest [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) interfejs, który definiuje jedną właściwość o nazwie [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged). Klasa, która implementuje ten interfejs jest ogólnie wyzwala zdarzenie, gdy jeden z jego właściwości publiczne zmienia wartość. Zdarzenie nie trzeba być uruchomione, jeśli właściwość nigdy się nie zmienia. ( `INotifyPropertyChanged` Interfejsu jest również implementowana przez `BindableObject` i `PropertyChanged` zdarzenie jest wywoływane zawsze wtedy, gdy zmieni się wartość właściwości możliwej do wiązania.)

`HslColorViewModel` Klasy definiuje właściwości pięć: `Hue`, `Saturation`, `Luminosity`, i `Color` właściwości są powiązane. Gdy jeden z trzech kolorów składniki wartość zmiany `Color` ponownego obliczania właściwości i `PropertyChanged` zdarzenia są uruchamiane wszystkie cztery właściwości:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

Podczas `Color` zmiany właściwości, statycznej `GetNearestColorName` method in Class metoda `NamedColor` klasy (dołączone do dodatków **DataBindingDemos** rozwiązania) uzyskuje najbliższego kolor nazwany i ustawia `Name` właściwości. To `Name` właściwość ma prywatnej `set` dostępu, więc nie można ustawić z poza klasy.

Gdy ViewModel jest ustawiona jako źródło powiązania, infrastruktura powiązań dołącza obsługę do `PropertyChanged` zdarzeń. W ten sposób powiązania może zostać poinformowany o zmiany właściwości, a następnie można ustawić właściwości obiektu docelowego ze zmienionymi wartościami.

Jednak gdy właściwość target (lub `Binding` definicji na właściwość docelowa) ma `BindingMode` z `OneTime`, nie jest konieczne infrastruktura powiązań dołączyć program obsługi na `PropertyChanged` zdarzeń. Właściwość docelowa jest aktualizowany tylko wtedy, gdy `BindingContext` zmiany, a nie samej właściwości źródła zmiany.

**Prosty selektor kolorów** plik XAML `HslColorViewModel` w słowniku zasobów i inicjuje strony `Color` właściwości. `BindingContext` Właściwość `Grid` ustawiono `StaticResource` powiązanie rozszerzenie, aby odwoływać się do tego zasobu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SimpleColorSelectorPage">

    <ContentPage.Resources>
        <ResourceDictionary>
            <local:HslColorViewModel x:Key="viewModel"
                                     Color="MediumTurquoise" />

            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <Grid BindingContext="{StaticResource viewModel}">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <BoxView Color="{Binding Color}"
                 Grid.Row="0" />

        <StackLayout Grid.Row="1"
                     Margin="10, 0">

            <Label Text="{Binding Name}"
                   HorizontalTextAlignment="Center" />

            <Slider Value="{Binding Hue}" />

            <Slider Value="{Binding Saturation}" />

            <Slider Value="{Binding Luminosity}" />
        </StackLayout>
    </Grid>
</ContentPage>
```

`BoxView`, `Label`i trzy `Slider` widoków dziedziczy kontekstu powiązania z `Grid`. Widoki te są wszystkie elementy docelowe powiązania odwołujące się do właściwości źródła w ViewModel. Dla `Color` właściwość `BoxView`i `Text` właściwość `Label`, powiązań danych są `OneWay`: właściwości w widoku są ustawiane przy użyciu właściwości w ViewModel.

`Value` Właściwość `Slider`, jest jednak `TwoWay`. Dzięki temu każda `Slider` należy ustawić z ViewModel, a także do ViewModel należy ustawić każdego z nich `Slider`.

Po uruchomieniu program `BoxView`, `Label`i trzy `Slider` elementy są gotowi z ViewModel, w oparciu o początkowej `Color` właściwością po utworzeniu wystąpienia ViewModel. Jest to przedstawione na zrzucie ekranu systemu iOS po lewej stronie:

[![Selektor kolorów proste](binding-mode-images/simplecolorselector-small.png "selektor kolorów proste")](binding-mode-images/simplecolorselector-large.png#lightbox "selektor kolorów prosty")

Ponieważ manipulowanie suwaki, `BoxView` i `Label` są aktualizowane w związku z tym, jak pokazano na zrzutach ekranu dla systemów Android i platformy uniwersalnej systemu Windows.

Utworzenie wystąpienia ViewModel w słowniku zasobów jest jeden typowym podejściem. Istnieje również możliwość utworzenia wystąpienia ViewModel w tagach element właściwości dla `BindingContext` właściwości. W **prosty selektor kolorów** XAML pliku, spróbuj usunąć `HslColorViewModel` ze słownika zasobów i ustaw ją na `BindingContext` właściwość `Grid` następująco:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>

    ···

</Grid>
```

Różne sposoby, można ustawić kontekstu powiązania. Czasami pliku związanego z kodem ViewModel tworzy i ustawia ją na `BindingContext` właściwości strony. Są prawidłowymi podejścia.

## <a name="overriding-the-binding-mode"></a>Zastępowanie tryb powiązania

Jeśli domyślny tryb powiązania na właściwość docelowa nie jest odpowiedni dla powiązania danych, istnieje możliwość zastąpienia go przez ustawienie [ `Mode` ](xref:Xamarin.Forms.BindingBase.Mode) właściwość `Binding` (lub [ `Mode` ](xref:Xamarin.Forms.Xaml.BindingExtension.Mode) właściwość `Binding` — rozszerzenie znaczników) do jednego z elementów członkowskich `BindingMode` wyliczenia.

Jednak ustawienie `Mode` właściwości `TwoWay` nie zawsze działa zgodnie z oczekiwaniami. Na przykład spróbuj zmodyfikować **powiązania XAML alternatywna** pliku XAML, aby uwzględnić `TwoWay` w definicji powiązania:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Można się spodziewać, `Slider` będzie inicjowane na wartość początkową `Scale` właściwość, która jest 1, ale nie mają miejsce. Gdy `TwoWay` powiązania jest zainicjowany, element docelowy jest ustawiony ze źródła, co oznacza, że `Scale` właściwość jest ustawiona na `Slider` domyślna wartość 0. Gdy `TwoWay` powiązania jest ustawiona na `Slider`, a następnie `Slider` jest początkowo ustawiona jako ze źródła.

Można ustawić trybu wiązania `OneWayToSource` w **powiązania XAML alternatywna** próbki:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

Teraz `Slider` jest inicjowany do 1 (domyślna wartość `Scale`) ale manipulowania `Slider` nie ma wpływu na `Scale` właściwości, więc nie jest to bardzo przydatne.

Bardzo przydatne stosowania Zastępowanie domyślnego trybu powiązania z `TwoWay` obejmuje `SelectedItem` właściwość `ListView`. Jest to domyślny tryb powiązania `OneWayToSource`. Gdy powiązanie danych jest ustawiona na `SelectedItem` właściwości w celu odwołania właściwość źródła w ViewModel tej właściwości źródła zostanie ustawiony z `ListView` zaznaczenia. Jednak w niektórych przypadkach można także `ListView` z ViewModel.

**Ustawień przykładowych** strony pokazano tej techniki. Ta strona przedstawia proste wdrażanie ustawienia aplikacji, które bardzo często są zdefiniowane w ViewModel, na przykład to `SampleSettingsViewModel` pliku:

```csharp
public class SampleSettingsViewModel : INotifyPropertyChanged
{
    string name;
    DateTime birthDate;
    bool codesInCSharp;
    double numberOfCopies;
    NamedColor backgroundNamedColor;

    public event PropertyChangedEventHandler PropertyChanged;

    public SampleSettingsViewModel(IDictionary<string, object> dictionary)
    {
        Name = GetDictionaryEntry<string>(dictionary, "Name");
        BirthDate = GetDictionaryEntry(dictionary, "BirthDate", new DateTime(1980, 1, 1));
        CodesInCSharp = GetDictionaryEntry<bool>(dictionary, "CodesInCSharp");
        NumberOfCopies = GetDictionaryEntry(dictionary, "NumberOfCopies", 1.0);
        BackgroundNamedColor = NamedColor.Find(GetDictionaryEntry(dictionary, "BackgroundNamedColor", "White"));
    }

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public DateTime BirthDate
    {
        set { SetProperty(ref birthDate, value); }
        get { return birthDate; }
    }

    public bool CodesInCSharp
    {
        set { SetProperty(ref codesInCSharp, value); }
        get { return codesInCSharp; }
    }

    public double NumberOfCopies
    {
        set { SetProperty(ref numberOfCopies, value); }
        get { return numberOfCopies; }
    }

    public NamedColor BackgroundNamedColor
    {
        set
        {
            if (SetProperty(ref backgroundNamedColor, value))
            {
                OnPropertyChanged("BackgroundColor");
            }
        }
        get { return backgroundNamedColor; }
    }

    public Color BackgroundColor
    {
        get { return BackgroundNamedColor?.Color ?? Color.White; }
    }

    public void SaveState(IDictionary<string, object> dictionary)
    {
        dictionary["Name"] = Name;
        dictionary["BirthDate"] = BirthDate;
        dictionary["CodesInCSharp"] = CodesInCSharp;
        dictionary["NumberOfCopies"] = NumberOfCopies;
        dictionary["BackgroundNamedColor"] = BackgroundNamedColor.Name;
    }

    T GetDictionaryEntry<T>(IDictionary<string, object> dictionary, string key, T defaultValue = default(T))
    {
        return dictionary.ContainsKey(key) ? (T)dictionary[key] : defaultValue;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Każde ustawienie aplikacji jest właściwością, który jest zapisywany do słownika właściwości zestawu narzędzi Xamarin.Forms w metodę o nazwie `SaveState` i załadowany z tego słownika w konstruktorze. W dolnej części tej klasy są dwie metody, które pomagają uprościć modele widoków były mniej podatny na błędy. `OnPropertyChanged` Metoda u dołu ma parametr opcjonalny jest ustawiony do wywoływania właściwości. Pozwala to uniknąć błędów pisowni, określając nazwę właściwości jako ciąg.

`SetProperty` Metody w klasie ma jeszcze większe: porównuje wartość, która jest ustawiana na właściwość z wartością przechowywaną jako pole, a tylko wywołuje `OnPropertyChanged` gdy dwie wartości nie są równe.

`SampleSettingsViewModel` Klasa definiuje dwie właściwości koloru tła: `BackgroundNamedColor` właściwość jest typu `NamedColor`, której klasą znajdują się również w **DataBindingDemos** rozwiązania. `BackgroundColor` Właściwość jest typu `Color`i są uzyskiwane z `Color` właściwość `NamedColor` obiektu.

`NamedColor` Klasa używa odbicia .NET można wyliczyć wszystkie pola publiczne statyczne w Xamarin.Forms `Color` struktury i przechowywać je przy użyciu ich nazw, w kolekcji dostępne z statycznej `All` właściwości:

```csharp
public class NamedColor : IEquatable<NamedColor>, IComparable<NamedColor>
{
    // Instance members
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    public bool Equals(NamedColor other)
    {
        return Name.Equals(other.Name);
    }

    public int CompareTo(NamedColor other)
    {
        return Name.CompareTo(other.Name);
    }

    // Static members
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof(Color))
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
        all.Sort();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }

    public static NamedColor Find(string name)
    {
        return ((List<NamedColor>)All).Find(nc => nc.Name == name);
    }

    public static string GetNearestColorName(Color color)
    {
        double shortestDistance = 1000;
        NamedColor closestColor = null;

        foreach (NamedColor namedColor in NamedColor.All)
        {
            double distance = Math.Sqrt(Math.Pow(color.R - namedColor.Color.R, 2) +
                                        Math.Pow(color.G - namedColor.Color.G, 2) +
                                        Math.Pow(color.B - namedColor.Color.B, 2));

            if (distance < shortestDistance)
            {
                shortestDistance = distance;
                closestColor = namedColor;
            }
        }
        return closestColor.Name;
    }
}
```

`App` Klasy w **DataBindingDemos** projektu definiuje właściwość o nazwie `Settings` typu `SampleSettingsViewModel`. Ta właściwość jest inicjowana po `App` tworzenia wystąpienia klasy i `SaveState` metoda jest wywoływana, gdy `OnSleep` metoda jest wywoływana:

```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        Settings = new SampleSettingsViewModel(Current.Properties);

        MainPage = new NavigationPage(new MainPage());
    }

    public SampleSettingsViewModel Settings { private set; get; }

    protected override void OnStart()
    {
        // Handle when your app starts
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Settings.SaveState(Current.Properties);
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
    }
}
```

Aby uzyskać więcej informacji na temat metody cyklu życia aplikacji, zobacz artykuł [ **cykl życia aplikacji**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Prawie wszystko, co jeszcze jest obsługiwana w **SampleSettingsPage.xaml** pliku. `BindingContext` Strony można ustawić przy użyciu `Binding` — rozszerzenie znaczników: źródło wiążące jest statyczna `Application.Current` właściwość, która jest wystąpienie elementu `App` klasy w projekcie i `Path` jest ustawiona na `Settings` Właściwość, która jest `SampleSettingsViewModel` obiektu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SampleSettingsPage"
             Title="Sample Settings"
             BindingContext="{Binding Source={x:Static Application.Current},
                                      Path=Settings}">

    <StackLayout BackgroundColor="{Binding BackgroundColor}"
                 Padding="10"
                 Spacing="10">

        <StackLayout Orientation="Horizontal">
            <Label Text="Name: "
                   VerticalOptions="Center" />

            <Entry Text="{Binding Name}"
                   Placeholder="your name"
                   HorizontalOptions="FillAndExpand"
                   VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Birth Date: "
                   VerticalOptions="Center" />

            <DatePicker Date="{Binding BirthDate}"
                        HorizontalOptions="FillAndExpand"
                        VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Do you code in C#? "
                   VerticalOptions="Center" />

            <Switch IsToggled="{Binding CodesInCSharp}"
                    VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Number of Copies: "
                   VerticalOptions="Center" />

            <Stepper Value="{Binding NumberOfCopies}"
                     VerticalOptions="Center" />

            <Label Text="{Binding NumberOfCopies}"
                   VerticalOptions="Center" />
        </StackLayout>

        <Label Text="Background Color:" />

        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
                  VerticalOptions="FillAndExpand"
                  RowHeight="40">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     HeightRequest="32"
                                     WidthRequest="32"
                                     VerticalOptions="Center" />

                            <Label Text="{Binding FriendlyName}"
                                   FontSize="24"
                                   VerticalOptions="Center" />
                        </StackLayout>                        
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Wszystkie elementy podrzędne strony dziedziczy kontekstu powiązania. Większość pozostałych powiązaniach na tej stronie są do właściwości w `SampleSettingsViewModel`. `BackgroundColor` Właściwość jest używana do ustawiania `BackgroundColor` właściwość `StackLayout`i `Entry`, `DatePicker`, `Switch`, i `Stepper` właściwości są powiązane z innymi właściwościami w ViewModel.

`ItemsSource` Właściwość `ListView` jest ustawiona na statyczną `NamedColor.All` właściwości. To wypełnia `ListView` ze wszystkimi `NamedColor` wystąpień. Dla każdego elementu w `ListView`, kontekstu powiązania dla elementu jest ustawiona na `NamedColor` obiektu. `BoxView` i `Label` w `ViewCell` są powiązane z właściwościami w `NamedColor`.

`SelectedItem` Właściwość `ListView` typu `NamedColor`i jest powiązana z `BackgroundNamedColor` właściwość `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Domyślny tryb powiązania `SelectedItem` jest `OneWayToSource`, która ustawia właściwość ViewModel z wybranego elementu. `TwoWay` Tryb umożliwia `SelectedItem` z ViewModel.

Jednak gdy `SelectedItem` jest ustawiana w ten sposób `ListView` nie automatycznie przewijać w celu wyświetlenia wybranego elementu. Niezbędne jest niewielkiej ilości kodu w pliku związanym z kodem:

```csharp
public partial class SampleSettingsPage : ContentPage
{
    public SampleSettingsPage()
    {
        InitializeComponent();

        if (colorListView.SelectedItem != null)
        {
            colorListView.ScrollTo(colorListView.SelectedItem,
                                   ScrollToPosition.MakeVisible,
                                   false);
        }
    }
}
```

Zrzut ekranu z systemem iOS, po lewej stronie zawiera program podczas pierwszego uruchomienia. Konstruktor w `SampleSettingsViewModel` inicjuje kolor tła na biały i to, co jest wybrane w `ListView`:

[![Przykładowe ustawienia](binding-mode-images/samplesettings-small.png "przykładowe ustawienia")](binding-mode-images/samplesettings-large.png#lightbox "przykładowe ustawienia")

Inne dwa zrzuty ekranu Pokaż ustawienia zmieniony. Eksperymentując z tą stroną, pamiętaj przełączyć w tryb uśpienia lub zakończenie go na urządzeniu lub w emulatorze, czy jest uruchomiony program. Kończenie programu, używając debugera programu Visual Studio nie będzie powodował `OnSleep` zastąpić w `App` klasy do wywołania.

W następnym artykule pokazano, jak określić [ **formatowanie ciągów** ](string-formatting.md) nad wiązaniami danych, które są ustawione na `Text` właściwość `Label`.


## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
