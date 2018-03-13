---
title: "Tryb wiązania"
description: "Sterowanie przepływem danych między źródłem a celem"
ms.topic: article
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 887dc3cf710fb75d05d02af179bc218c15d31f97
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="binding-mode"></a>Tryb wiązania

W [poprzednim artykule](basic-bindings.md), **zamiast kodu — wiązanie** i **zamiast XAML — wiązanie** stron umieszczony `Label` z jego `Scale` właściwości powiązany z `Value` właściwość `Slider`. Ponieważ `Slider` wartość początkowa to 0, to spowodowane `Scale` właściwość `Label` na wartość 0, a nie na 1 i `Label` zniknęła.

W [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) próbki, **wstecznego powiązania** strony jest podobny do programów w poprzednim artykule, z wyjątkiem tego, że powiązanie danych został zdefiniowany w `Slider` , a nie na `Label`:

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

Na początku, to rozwiązanie może wydawać się wstecz: teraz `Label` jest źródłem powiązania danych i `Slider` jest elementem docelowym. Odwołania do powiązania `Opacity` właściwość `Label`, która ma wartość domyślną 1.

Zgodnie z oczekiwaniami może `Slider` jest ustawiana na wartość 1, z początkowej `Opacity` wartość `Label`. Jest to przedstawione na zrzucie ekranu z systemem iOS, po lewej stronie:

[![Wstecznego powiązania](binding-mode-images/reversebinding-small.png "wstecznego powiązania")](binding-mode-images/reversebinding-large.png#lightbox "wstecznego powiązania")

Ale może być oczekiwano, że `Slider` nadal działa, jak pokazują zrzuty ekranu Android i platformy uniwersalnej systemu Windows. Prawdopodobnie sugeruje, że powiązanie danych działa lepiej, gdy `Slider` jest cel wiązania zamiast `Label` ponieważ inicjowanie działa tak, jak firma Microsoft może spodziewać się.

Różnica między **wstecznego powiązania** próbki i wcześniejszych próbek obejmuje *tryb wiązania*.

## <a name="the-default-binding-mode"></a>Domyślny tryb powiązania

Określono tryb powiązanie z elementem członkowskim o [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) wyliczenie: 

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) 
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) &ndash; dane są zapisywane obu kierunkach między źródłem a celem
- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) &ndash; dane są zapisywane ze źródła do docelowego
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) &ndash; dane przechodzą z miejsca docelowego do źródła

Dla każdej właściwości powiązania ma domyślny tryb, który jest ustawiana po utworzeniu można powiązać właściwości powiązania i która jest dostępna z [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) właściwość `BindableProperty` obiektu. To domyślny tryb powiązanie wskazuje tryb obowiązywać, gdy ta właściwość jest element docelowy powiązanie danych.

Domyślny tryb powiązanie dla większości właściwości `Rotation`, `Scale`, i `Opacity` jest `OneWay`. Gdy te właściwości są elementy docelowe wiązania danych, ze źródła jest ustawiona właściwość target.

Jednakże domyślny tryb powiązania `Value` właściwość `Slider` jest `TwoWay`. Oznacza to, że w przypadku `Value` właściwość jest element docelowy wiązania danych, a następnie element docelowy jest ustawiany w źródle (normalnie), ale źródła znajduje się również z elementem docelowym. Jest to, co umożliwia `Slider` ustawiono z początkowej `Opacity` wartość.

To powiązanie dwukierunkowe może wydawać się, aby utworzyć nieskończoną pętlę, ale który nie jest realizowane. Właściwości nie można sygnalizować zmiany właściwości chyba, że faktycznie zmiany właściwości. Zapobiega to nieskończoną pętlę.

### <a name="two-way-bindings"></a>Dwukierunkowego powiązania

Najbardziej właściwości ma domyślny tryb powiązania z `OneWay` , lecz następujące właściwości mają domyślny tryb powiązania z `TwoWay`:

- `Date` Właściwość `DatePicker`
- `Text` Właściwość `Editor`, `Entry`, `SearchBar`, i `EntryCell`
- `IsRefreshing` Właściwość `ListView`
- `SelectedItem` Właściwość `MultiPage`
- `SelectedIndex` i `SelectedItem` właściwości `Picker`
- `Value` Właściwość `Slider` i `Stepper`
- `IsToggled` Właściwość `Switch` 
- `On` Właściwość `SwitchCell`
- `Time` Właściwość `TimePicker`

Te właściwości określonym są zdefiniowane jako `TwoWay` bardzo dobre przyczyny: 

W przypadku używania powiązań danych z architekturą aplikacji Model-View-ViewModel (MVVM) Klasa ViewModel jest źródła danych powiązania i widoku, który składa się z widoków, takich jak `Slider`, elementów docelowych powiązania danych. Podobne powiązania MVVM **wstecznego powiązania** próbki więcej niż w poprzednich przykładach powiązania. Jest bardzo prawdopodobne, czy chcesz, aby każdy widok na stronie, aby można było zainicjować o wartości odpowiadających im właściwości w ViewModel, ale zmiany widoku również wpływać na właściwość ViewModel.

Właściwości z trybów powiązanie domyślnej `TwoWay` są te właściwości, które są najbardziej mogą być używane w scenariuszach MVVM.

### <a name="one-way-to-source-bindings"></a>Jednorazowe-Way-do-powiązania źródła

Właściwości tylko do odczytu ma domyślny tryb powiązania z `OneWayToSource`. Istnieje tylko jedna właściwość można powiązać odczytu/zapisu, która ma domyślny tryb powiązania z `OneWayToSource`:

- `SelectedItem` Właściwość `ListView`

Decyzja jest powiązanie na `SelectedItem` powinno spowodować ustawienie źródle powiązania właściwości. Przykładem w dalszej części tego artykułu zastąpienia tego zachowania.

## <a name="viewmodels-and-property-change-notifications"></a>ViewModels i powiadomień o zmianie właściwości

**Prosty selektor kolorów** strony zademonstrowano użycie prostego ViewModel. Powiązania danych Zezwalaj użytkownikowi na wybranie koloru przy użyciu trzech `Slider` elementy odcień, nasycenie i jasność.

ViewModel jest źródłem powiązania danych. Jest ViewModel *nie* zdefiniować właściwości, ale implementuje mechanizm powiadomień, który umożliwia infrastruktura powiązań otrzymywać powiadomienia, gdy wartość właściwości. Jest to mechanizm powiadamiania [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) interfejs, który definiuje jedną właściwość o nazwie [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/). Klasa, która implementuje ten interfejs zwykle generowane zdarzenie, gdy jeden z jego właściwości publiczne zmienia wartość. Zdarzenie nie trzeba uruchamiany, jeśli właściwość nigdy nie zmieni się. ( `INotifyPropertyChanged` Interfejs również jest implementowany przez `BindableObject` i `PropertyChanged` zdarzenie jest wywoływane zawsze, gdy zmienia się wartość właściwości możliwej do wiązania.)

`HslColorViewModel` Klasy definiuje właściwości pięć: `Hue`, `Saturation`, `Luminosity`, i `Color` właściwości są powiązane. Gdy jeden z trzech wartości zmian, składniki koloru `Color` ponownego obliczania właściwości i `PropertyChanged` zdarzenia są generowane dla wszystkich czterech właściwości:

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

Gdy `Color` zmiany właściwości, statycznych `GetNearestColorName` metody w `NamedColor` klasy (zawarte w **DataBindingDemos** rozwiązania) uzyskuje najbliższy kolor nazwany i ustawia `Name` właściwości. To `Name` właściwość ma prywatnej `set` dostępu, więc nie można ustawić z poza klasą.

Gdy ViewModel jest ustawiona jako źródło powiązania, infrastruktura powiązań dołącza program obsługi do `PropertyChanged` zdarzeń. W ten sposób powiązania można otrzymywać powiadomienia o zmianach właściwości, a następnie można ustawić właściwości obiektu docelowego ze zmienionymi wartościami.

**Prosty selektor kolorów** tworzy plik XAML `HslColorViewModel` w słowniku zasobów i inicjuje strony `Color` właściwości. `BindingContext` Właściwość `Grid` ustawiono `StaticResource` powiązanie rozszerzenia, aby odwołać tego zasobu:

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

`BoxView`, `Label`i trzy `Slider` widoków dziedziczy kontekstu powiązania z `Grid`. Widoki te są wszystkie elementy docelowe powiązania, które odwołują się do właściwości źródła w ViewModel. Dla `Color` właściwość `BoxView`i `Text` właściwość `Label`, powiązania danych są `OneWay`: właściwości w widoku są ustawiane przy użyciu właściwości w ViewModel.

`Value` Właściwość `Slider`, jednak jest `TwoWay`. Dzięki temu każda `Slider` należy ustawić z ViewModel, a także do ViewModel ustawiono z każdej `Slider`. 

Po pierwszym uruchomieniu programu `BoxView`, `Label`i trzy `Slider` elementy są wszystkie zestaw na podstawie ViewModel oparte na początkowej `Color` właściwość ViewModel zostało uruchomione. Przedstawiono to w zrzut ekranu dla systemu iOS po lewej stronie:

[![Wybór koloru proste](binding-mode-images/simplecolorselector-small.png "selektor kolorów proste")](binding-mode-images/simplecolorselector-large.png#lightbox "selektor kolorów proste")

Jak manipulowania suwaki, `BoxView` i `Label` są aktualizowane w związku z tym, jak pokazano na zrzutach ekranu Android i platformy uniwersalnej systemu Windows.

Tworzenie wystąpień ViewModel w słowniku zasobów jest jeden typowym podejściem. Istnieje również możliwość utworzenia wystąpienia ViewModel w tagach element właściwości dla `BindingContext` właściwości. W **prosty selektor kolorów** XAML pliku, spróbuj usunąć `HslColorViewModel` ze słownika zasobów i ustaw ją na `BindingContext` właściwość `Grid` podobnie do następującej:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>
        
    ···

</Grid>
```

Kontekst powiązania można ustawić w różny sposób. Czasami plik CodeBehind tworzy ViewModel i ustawia ją na `BindingContext` właściwości strony. Są to wszystkie ważne metod.

## <a name="overriding-the-binding-mode"></a>Zastępowanie tryb wiązania

Jeśli domyślny tryb powiązania dla właściwości docelowej nie jest odpowiedni dla powiązania danych, istnieje możliwość zastąpienia go przez ustawienie [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) właściwość `Binding` (lub [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Mode/) właściwość `Binding` — rozszerzenie znaczników) do jednego z elementów członkowskich `BindingMode` wyliczenia.

Jednak ustawienie `Mode` właściwości `TwoWay` nie zawsze działa zgodnie z oczekiwaniami może. Na przykład, spróbuj zmodyfikować **powiązania XAML alternatywna** pliku XAML, aby uwzględnić `TwoWay` w definicji powiązania:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Można się spodziewać który `Slider` czy zainicjowany do wartości początkowej `Scale` właściwość, która jest 1, ale który nie jest realizowane. Gdy `TwoWay` powiązania jest zainicjowany, element docelowy jest ustawiany ze źródła, co oznacza, że `Scale` właściwość jest ustawiona na `Slider` domyślna wartość 0. Podczas `TwoWay` powiązania jest ustawiona na `Slider`, a następnie `Slider` jest ustawiany w źródle.

Można ustawić trybu powiązania `OneWayToSource` w **powiązania XAML alternatywna** próbki:

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

Przydatne stosowania Zastępowanie domyślnego trybu powiązania z `TwoWay` obejmuje `SelectedItem` właściwość `ListView`. Jest to domyślny tryb powiązania `OneWayToSource`. Podczas wiązania z danymi jest ustawiona na `SelectedItem` właściwości, aby odwołać właściwość źródła w ViewModel tej właściwości source przybiera wartość z `ListView` zaznaczenia. Jednak w pewnych okolicznościach, można także `ListView` zostać zainicjowany z ViewModel.

**Ustawień przykładowych** strony pokazuje tej metody. Ta strona reprezentuje prostych implementacji ustawień aplikacji, które bardzo często są definiowane w ViewModel, takich jak ta `SampleSettingsViewModel` pliku:

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

Każde ustawienie aplikacji jest właściwością, która jest zapisywany do słownika właściwości platformy Xamarin.Forms w metodę o nazwie `SaveState` i załadowany z tego słownika w konstruktorze. W dolnej części tej klasy są dwie metody, które pomagają usprawnić ViewModels i należy je mniej podatne na błędy. `OnPropertyChanged` Metoda u dołu ma parametr opcjonalny, który ma ustawioną właściwość wywołującego. Pozwala to uniknąć błędów pisowni, określając nazwę właściwości jako ciąg. 

`SetProperty` Metody w klasie jest nawet więcej: porównuje wartość, która jest ustawiona we właściwości z wartością przechowywane jako pole i tylko wywołuje `OnPropertyChanged` po dwóch wartości nie są takie same. 

`SampleSettingsViewModel` Klasa definiuje dwie właściwości kolor tła: `BackgroundNamedColor` właściwość jest typu `NamedColor`, która klasa znajduje się również w **DataBindingDemos** rozwiązania. `BackgroundColor` Właściwość jest typu `Color`i są uzyskiwane z `Color` właściwość `NamedColor` obiektu.

`NamedColor` Klasy używa odbicia .NET wyliczyć wszystkie statyczne publiczne pola platformy Xamarin.Forms `Color` struktury i przechowywać je z ich nazwy w kolekcji dostępny z statycznych `All` właściwości:

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

`App` Klasy w **DataBindingDemos** projekt definiuje właściwość o nazwie `Settings` typu `SampleSettingsViewModel`. Ta właściwość jest inicjowana po `App` tworzenia wystąpienia klasy i `SaveState` metoda jest wywoływana, gdy `OnSleep` metoda jest wywoływana:

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

Aby uzyskać więcej informacji na temat metod cyklem życia aplikacji, zobacz artykuł [ **cykl życia aplikacji**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Niemal wszystkich innych urządzeń jest obsługiwane w **SampleSettingsPage.xaml** pliku. `BindingContext` Strony jest ustawiany za pomocą `Binding` — rozszerzenie znaczników: źródło powiązania jest statycznych `Application.Current` właściwość, która jest wystąpieniem programu `App` klasy w projekcie i `Path` ma ustawioną wartość `Settings` Właściwość, która jest `SampleSettingsViewModel` obiektu:

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

Wszystkie elementy podrzędne strony dziedziczy kontekstu powiązania. Większość pozostałych powiązaniach na tej stronie mają właściwości w `SampleSettingsViewModel`. `BackgroundColor` Właściwość jest używana do ustawiania `BackgroundColor` właściwość `StackLayout`i `Entry`, `DatePicker`, `Switch`, i `Stepper` właściwości są powiązane z właściwościami w ViewModel.

`ItemsSource` Właściwość `ListView` ustawiono statycznych `NamedColor.All` właściwości. Wypełnia to `ListView` ze wszystkimi `NamedColor` wystąpień. Dla każdego elementu w `ListView`, kontekst powiązania dla elementu ustawiono `NamedColor` obiektu. `BoxView` i `Label` w `ViewCell` są powiązane z właściwościami w `NamedColor`.

`SelectedItem` Właściwość `ListView` jest typu `NamedColor`i jest powiązany z `BackgroundNamedColor` właściwości `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Domyślny tryb powiązania `SelectedItem` jest `OneWayToSource`, który ustawia właściwość ViewModel z wybranego elementu. `TwoWay` Tryb umożliwia `SelectedItem` zostać zainicjowany z ViewModel. 

Jednakże, gdy `SelectedItem` jest ustawiony w ten sposób `ListView` nie jest przewijane automatycznie do zaznaczonego elementu. Niezbędne jest trochę kodu w pliku związanym z kodem:

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

Zrzut ekranu dla systemu iOS po lewej stronie zawiera program po pierwszym uruchomieniu. Konstruktor w `SampleSettingsViewModel` inicjuje kolor tła na kolor biały i jest wybranej `ListView`:

[![Ustawienia przykładowe](binding-mode-images/samplesettings-small.png "ustawienia przykładowe")](binding-mode-images/samplesettings-large.png#lightbox "ustawienia przykładowe")

Inne dwa zrzuty ekranu pokazują zmienionych ustawień. Eksperymentując z tej strony, pamiętaj, aby umieścić w stan uśpienia lub zakończenia działania na urządzeniu lub emulatorze, który jest uruchomiony program. Nie spowoduje zakończenie programu Visual Studio debugger `OnSleep` zastąpienia w `App` klasy do wywołania.

W kolejnym artykule zobaczysz sposobu określania [ **ciągu formatowania** ](string-formatting.md) powiązań danych, które są ustawione na `Text` właściwość `Label`.


## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
