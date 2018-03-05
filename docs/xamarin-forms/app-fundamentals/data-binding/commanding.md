---
title: Interfejs polecenia
description: "Implementowanie `Command` właściwości powiązania danych"
ms.topic: article
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 0bc039385a6b2077c3b5fa5114b35b586a14a150
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="the-command-interface"></a>Interfejs polecenia

W ramach architektury Model-View-ViewModel (MVVM) powiązania danych są definiowane pomiędzy właściwości w ViewModel, który zazwyczaj jest klasą pochodzącą z `INotifyPropertyChanged`i właściwości w widoku, który zazwyczaj jest to plik XAML. Czasami aplikacja ma już potrzeby, które wykraczają poza tych powiązań właściwości, gdyż użytkownik zainicjować poleceń, które mają wpływ na coś ViewModel. Te polecenia są zazwyczaj zgłoszony przez kliknięcia przycisków lub palcem podsłuchu i tradycyjnie są przetwarzane w pliku CodeBehind programu obsługi dla `Clicked` zdarzenie `Button` lub `Tapped` zdarzenie `TapGestureRecognizer`. 

Interfejs sterująca umożliwia innym podejściu do wykonania polecenia, który znacznie lepiej nadaje się do architektury MVVM. ViewModel, sam może zawierać polecenia, które są metody, które są wykonywane w reakcji na określonego działania w widoku, takie jak `Button` kliknij. Powiązania danych są definiowane pomiędzy tych poleceń i `Button`.

Aby umożliwić powiązania danych między `Button` oraz ViewModel, `Button` definiuje dwie właściwości:

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) typu [`ICommand`](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/)
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) typu `Object`

Aby używać interfejsu polecenia, należy zdefiniować powiązania danych, którego celem jest `Command` właściwość `Button` której źródłem jest właściwość ViewModel typu `ICommand`. ViewModel zawiera kod skojarzony z tym `ICommand` właściwość, która jest wykonywana po kliknięciu przycisku. Można ustawić `CommandParameter` dowolne dane do rozróżnienia wielu przycisków, jeśli są wszystkie powiązane do tej samej `ICommand` właściwości w ViewModel.

`Command` i `CommandParameter` właściwości są także definiowane przez następujące klasy:

- [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) i w związku z tym [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/), która jest pochodną `MenuItem`
- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) i w związku z tym [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/), która jest pochodną `TextCell`
- [`TapGestureRecognizer`](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)

[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) definiuje [ `SearchCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) właściwości typu `ICommand` i [ `SearchCommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) właściwości. [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) Właściwość [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) jest również typu `ICommand`. 

Tych poleceń mogą być obsługiwane w ramach ViewModel w taki sposób, który nie jest zależny od obiektu konkretnego interfejsu użytkownika w widoku.

## <a name="the-icommand-interface"></a>Interfejs ICommand

[ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) Interfejsu nie jest częścią platformy Xamarin.Forms. Jest on zdefiniowany zamiast tego w [ `System.Windows.Input` ](https://developer.xamarin.com/api/namespace/System.Windows.Input/) przestrzeni nazw i składa się z dwóch metod i jedno zdarzenie:

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

Aby używać interfejsu polecenia, Twoje ViewModel zawiera właściwości typu `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

Klasa, która implementuje również musi odwoływać się ViewModel `ICommand` interfejsu. Ta klasa będzie opisana wkrótce. W widoku `Command` właściwość `Button` jest powiązany z tą właściwością: 

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Gdy użytkownik naciśnie `Button`, `Button` wywołania `Execute` metody w `ICommand` obiekt powiązany z jego `Command` właściwości. To najprostsza część sterująca interfejsu.

`CanExecute` Metody jest bardziej złożony. Gdy powiązanie najpierw jest zdefiniowana na `Command` właściwość `Button`, i po zmianie powiązania danych w jakiś sposób `Button` wywołania `CanExecute` metody w `ICommand` obiektu. Jeśli `CanExecute` zwraca `false`, a następnie `Button` się wyłączy. Oznacza to, że konkretnego polecenia obecnie jest niedostępny lub nieprawidłowy.

`Button` Również dołącza obsługi na `CanExecuteChanged` zdarzenie `ICommand`. Zdarzenie jest generowane z wewnątrz ViewModel. Po uruchomieniu zdarzenia `Button` wywołania `CanExecute` ponownie. `Button` Włącza się, jeśli `CanExecute` zwraca `true` i wyłącza się, jeśli `CanExecute` zwraca `false`.

> [!IMPORTANT]
> Nie używaj `IsEnabled` właściwość `Button` Jeśli używasz interfejsu polecenia.  

## <a name="the-command-class"></a>Klasy poleceń

Jeśli Twoje ViewModel definiuje okreolić, czy typ `ICommand`, ViewModel musi również zawierać lub odwołuje się do klasy, która implementuje `ICommand` interfejsu. Ta klasa musi zawierać lub odwołać `Execute` i `CanExecute` metody i fire `CanExecuteChanged` zdarzenia przy każdym `CanExecute` — metoda może zwracać różne wartości.

Możesz taka klasa może zapisywać, lub można użyć klasy, że ktoś inny został zapisany. Ponieważ `ICommand` jest częścią systemu Microsoft Windows został użyty do lat z aplikacjami MVVM systemu Windows. Używanie klasy systemu Windows, który implementuje `ICommand` umożliwia udostępnianie ViewModels Twojego między aplikacjami systemu Windows i aplikacji platformy Xamarin.Forms.

Jeśli udostępnianie ViewModels między systemem Windows i platformy Xamarin.Forms nie ma znaczenia, a następnie można użyć [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) lub [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) klasy uwzględnione w platformy Xamarin.Forms do zaimplementowania `ICommand`interfejsu. Te klasy umożliwiają określenie organów `Execute` i `CanExecute` metod w konstruktorach klasy. Użyj `Command<T>` korzystając `CommandParameter` powiązane właściwości w celu rozróżnienia wielu widoków do tego samego `ICommand` właściwości i łatwiejsze `Command` klasy, po którym nie jest wymagana.

## <a name="basic-commanding"></a>Steruje podstawowe

**Wpis osoby** strony [ **pokazy powiązania danych** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) program przedstawiono kilka prostych poleceń zaimplementowana w ViewModel.

`PersonViewModel` Definiuje trzy właściwości o nazwie `Name`, `Age`, i `Skills` definiującą osoby. Ta klasa jest *nie* zawierać `ICommand` właściwości:

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    string name;
    double age;
    string skills;

    public event PropertyChangedEventHandler PropertyChanged;

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public double Age
    {
        set { SetProperty(ref age, value); }
        get { return age; }
    }

    public string Skills
    {
        set { SetProperty(ref skills, value); }
        get { return skills; }
    }

    public override string ToString()
    {
        return Name + ", age " + Age;
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

`PersonCollectionViewModel` Pokazano poniżej tworzy nowe obiekty typu `PersonViewModel` i umożliwia użytkownikowi wypełnienie danych. W tym celu klasy definiuje właściwości `IsEditing` typu `bool` i `PersonEdit` typu `PersonViewModel`. Ponadto klasa definiuje trzy właściwości typu `ICommand` i właściwość o nazwie `Persons` typu `IList<PersonViewModel>`: 

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    PersonViewModel personEdit;
    bool isEditing;

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public bool IsEditing
    {
        private set { SetProperty(ref isEditing, value); }
        get { return isEditing; }
    }

    public PersonViewModel PersonEdit
    {
        set { SetProperty(ref personEdit, value); }
        get { return personEdit; }
    }

    public ICommand NewCommand { private set; get; }

    public ICommand SubmitCommand { private set; get; }

    public ICommand CancelCommand { private set; get; }

    public IList<PersonViewModel> Persons { get; } = new ObservableCollection<PersonViewModel>();

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

Skrócona lista nie zawiera konstruktora klasy, czyli gdzie trzech właściwości typu `ICommand` są zdefiniowane, który pojawi się wkrótce. Należy zauważyć, że zmienia się na trzech właściwości typu `ICommand` i `Persons` właściwości nie powodują `PropertyChanged` zdarzenia są uruchomione. Te właściwości są ustawione w przypadku utworzenia klasy i nie należy zmieniać później.

Przed badanie konstruktora `PersonCollectionViewModel` klasy, Przyjrzyjmy się w pliku XAML **wpis osoby** programu. Zawiera `Grid` z jego `BindingContext` ustawioną właściwość `PersonCollectionViewModel`. `Grid` Zawiera `Button` tekstem **nowy** z jego `Command` właściwość powiązana z `NewCommand` właściwości w ViewModel, powiązany formularzem wprowadzania z właściwościami `IsEditing` właściwość, jako także jako właściwości `PersonViewModel`, i powiązana dwa więcej przycisków `SubmitCommand` i `CancelCommand` właściwości ViewModel. Ostatni `ListView` Wyświetla kolekcję już wprowadzone osób:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.PersonEntryPage"
             Title="Person Entry">
    <Grid Margin="10">
        <Grid.BindingContext>
            <local:PersonCollectionViewModel />
        </Grid.BindingContext>
        
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <!-- New Button -->
        <Button Text="New"
                Grid.Row="0"
                Command="{Binding NewCommand}"
                HorizontalOptions="Start" />

        <!-- Entry Form -->
        <Grid Grid.Row="1"
              IsEnabled="{Binding IsEditing}">
            
            <Grid BindingContext="{Binding PersonEdit}">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Label Text="Name: " Grid.Row="0" Grid.Column="0" />
                <Entry Text="{Binding Name}" 
                       Grid.Row="0" Grid.Column="1" />

                <Label Text="Age: " Grid.Row="1" Grid.Column="0" />
                <StackLayout Orientation="Horizontal" 
                             Grid.Row="1" Grid.Column="1">
                    <Stepper Value="{Binding Age}"
                             Maximum="100" />
                    <Label Text="{Binding Age, StringFormat='{0} years old'}"
                           VerticalOptions="Center" />
                </StackLayout>

                <Label Text="Skills: " Grid.Row="2" Grid.Column="0" />
                <Entry Text="{Binding Skills}"
                       Grid.Row="2" Grid.Column="1" />

            </Grid>
        </Grid>

        <!-- Submit and Cancel Buttons -->
        <Grid Grid.Row="2">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>

            <Button Text="Submit"
                    Grid.Column="0"
                    Command="{Binding SubmitCommand}"
                    VerticalOptions="CenterAndExpand" />

            <Button Text="Cancel"
                    Grid.Column="1"
                    Command="{Binding CancelCommand}"
                    VerticalOptions="CenterAndExpand" />
        </Grid>

        <!-- List of Persons -->
        <ListView Grid.Row="3"
                  ItemsSource="{Binding Persons}" />
    </Grid>
</ContentPage>
```

Oto jak to działa: użytkownik naciśnie pierwszy **nowy** przycisku. To umożliwia formularza, ale wyłącza **nowy** przycisku. Użytkownik wprowadza następnie nazwę, wieku i umiejętności. W czasie edycji, użytkownik może nacisnąć **anulować** przycisk, aby rozpocząć od początku. Jeśli wprowadzono nazwę i prawidłowe wieku jest tylko **przesyłania** przycisk włączone. Naciśnięcie przycisku to **przesyłania** przycisk przesyłania osoby do kolekcji, wyświetlane przez `ListView`. Po upływie **anulować** lub **przesyłania** przycisk jest naciśnięty, formularza jest wyczyszczone i **nowy** przycisk jest aktywny ponownie.

Po lewej stronie ekranu dla systemu iOS pokazuje układ przed wprowadzono prawidłowy wiek. Android i platformy uniwersalnej systemu Windows ekrany Pokaż **przesyłania** przycisk włączona po ustawieniu wieku:

[![Wpis osoby](commanding-images/personentry-small.png "wpis osoby")](commanding-images/personentry-large.png "wpis osoby")

Program nie ma żadnych funkcje do edycji istniejących i nie są zapisywane wpisy po wyjściu z strony.

Wszystkie logikę **nowy**, **przesyłania**, i **anulować** obsługi przycisków w `PersonCollectionViewModel` za pośrednictwem definicji `NewCommand`, `SubmitCommand`, i `CancelCommand` właściwości. Konstruktor obiektu `PersonCollectionViewModel` ustawia właściwości te trzy obiekty typu `Command`.  

A [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) z `Command` klasa umożliwia przekazywanie argumentów typu `Action` i `Func<bool>` odpowiadający `Execute` i `CanExecute` metody. Najłatwiej zdefiniować te akcje i funkcje jako lambda funkcji bezpośrednio w `Command` konstruktora. W tym miejscu znajduje się definicja metody `Command` obiekt do `NewCommand` właściwości:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {
        NewCommand = new Command(
            execute: () =>
            {
                PersonEdit = new PersonViewModel();
                PersonEdit.PropertyChanged += OnPersonEditPropertyChanged;
                IsEditing = true;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return !IsEditing;
            });

        ···

    }

    void OnPersonEditPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        (SubmitCommand as Command).ChangeCanExecute();
    }
    
    void RefreshCanExecutes()
    {
        (NewCommand as Command).ChangeCanExecute();
        (SubmitCommand as Command).ChangeCanExecute();
        (CancelCommand as Command).ChangeCanExecute();
    }

    ···

}
```

Po kliknięciu przez użytkownika **nowy** przycisku `execute` funkcja przekazany do `Command` Konstruktor jest wykonywana. Spowoduje to utworzenie nowego `PersonViewModel` obiektów, ustawia program obsługi dla tego obiektu `PropertyChanged` zdarzeń, ustawia `IsEditing` do `true`i wywołuje `RefreshCanExecutes` metody zdefiniowanej po konstruktora.

Oprócz wykonania `ICommand` interfejsu `Command` klasa również definiuje metodę o nazwie `ChangeCanExecute`. Twojej ViewModel powinny wywoływać `ChangeCanExecute` dla `ICommand` właściwość zawsze, gdy nic się coś, co może zmieniać wartość zwracaną `CanExecute` metody. Wywołanie `ChangeCanExecute` powoduje, że `Command` klasy uruchomienie `CanExecuteChanged` metody. `Button` Dołączył obsługi dla tego zdarzenia i odpowiada przez wywołanie metody `CanExecute` ponownie, a następnie włączenie oparty na wartość zwracaną przez tę metodę.

Gdy `execute` metody `NewCommand` wywołania `RefreshCanExecutes`, `NewCommand` właściwości pobiera wywołanie `ChangeCanExecute`i `Button` wywołania `canExecute` metody, która zwraca teraz `false` ponieważ `IsEditing`właściwość jest teraz `true`.

`PropertyChanged` Obsługi nowego `PersonViewModel` obiektu wywołania `ChangeCanExecute` metody `SubmitCommand`. Oto implementowania tej właściwości polecenia:


```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        SubmitCommand = new Command(
            execute: () =>
            {
                Persons.Add(PersonEdit);
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return PersonEdit != null && 
                       PersonEdit.Name != null && 
                       PersonEdit.Name.Length > 1 && 
                       PersonEdit.Age > 0;
            });

        ···
    }

    ···

}
```

`canExecute` Działać w ramach `SubmitCommand` jest wywoływana za każdym razem, gdy jest właściwością zmienione w `PersonViewModel` obiekt edytowany. Zwraca `true` tylko wtedy, gdy `Name` właściwość jest co najmniej jeden znak i `Age` jest większa niż 0. W tym czasie **przesyłania** przycisk staje się dostępny. 

`execute` Działać w ramach **przesyłania** usuwa programu obsługi zmienić właściwości `PersonViewModel`, dodaje obiekt do `Persons` kolekcji i zwraca wszystkie elementy do początkowej warunków.

`execute` Działać w ramach **anulować** przycisk robi wszystko to **przesyłania** execept ma przycisk dodać obiektu do kolekcji:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        CancelCommand = new Command(
            execute: () =>
            {
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return IsEditing;
            }); 
    }

    ···

}
```

`canExecute` Metoda zwraca `true` w dowolnym momencie `PersonViewModel` jest edytowany.

Te techniki mogą być dostosowywane do bardziej złożonymi scenariuszami: właściwość `PersonCollectionViewModel` może być powiązane z `SelectedItem` właściwości `ListView` do edycji istniejących elementów i **usunąć** przycisk mogą zostać dodane do usunięcia te elementy.

Nie trzeba definiować `execute` i `canExecute` metod jako funkcje lambda. Możesz zapisać je jako zwykłych prywatnych metod w ViewModel i odwoływać je w `Command` konstruktorów. Jednak takie podejście zwykle powoduje wiele metod, które odwołują się tylko raz w ViewModel.

## <a name="using-command-parameters"></a>Przy użyciu parametrów polecenia

Czasami jest wygodne dla jednego lub więcej przycisków (lub inne obiekty interfejsu użytkownika) identyczny `ICommand` właściwości w ViewModel. W takim przypadku należy użyć `CommandParameter` właściwości do rozróżniania między przyciskami. 

Można nadal używać `Command` klasy tych udostępnionych `ICommand` właściwości. Definiuje klasę [konstruktora alternatywnego](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action%7BSystem.Object%7D/System.Func%7BSystem.Object,System.Boolean%7D/) która akceptuje `execute` i `canExecute` metody z parametrami typu `Object`. Jest to sposób `CommandParameter` jest przekazywana do tych metod.

Jednak przy użyciu `CommandParameter`, najłatwiej może używać ogólnych [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) klasę, aby określić typ obiektu ustawioną `CommandParameter`. `execute` i `canExecute` metody, które określisz ma parametry typu.

**Dziesiętną klawiatury** strony przedstawiono ta technika, pokazując implementowania klawiatury do wprowadzenia liczby dziesiętne. `BindingContext` Dla `Grid` jest `DecimalKeypadViewModel`. `Entry` Właściwość ta ViewModel jest powiązana z `Text` właściwość `Label`. Wszystkie `Button` obiekty są powiązane z różnych poleceń w ViewModel: `ClearCommand`, `BackspaceCommand`, i `DigitCommand`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.DecimalKeypadPage"
             Title="Decimal Keyboard">

    <Grid WidthRequest="240"
          HeightRequest="480"
          ColumnSpacing="2"
          RowSpacing="2"
          HorizontalOptions="Center"
          VerticalOptions="Center">

        <Grid.BindingContext>
            <local:DecimalKeypadViewModel />
        </Grid.BindingContext>
        
        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Button">
                    <Setter Property="FontSize" Value="32" />
                    <Setter Property="BorderWidth" Value="1" />
                    <Setter Property="BorderColor" Value="Black" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Label Text="{Binding Entry}"
               Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3"
               FontSize="32"
               LineBreakMode="HeadTruncation"
               VerticalTextAlignment="Center"
               HorizontalTextAlignment="End" />

        <Button Text="CLEAR"
                Grid.Row="1" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding ClearCommand}" />

        <Button Text="&#x21E6;"
                Grid.Row="1" Grid.Column="2" 
                Command="{Binding BackspaceCommand}" />

        <Button Text="7"
                Grid.Row="2" Grid.Column="0" 
                Command="{Binding DigitCommand}"
                CommandParameter="7" />

        <Button Text="8"
                Grid.Row="2" Grid.Column="1" 
                Command="{Binding DigitCommand}"
                CommandParameter="8" />
        
        <Button Text="9"
                Grid.Row="2" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="9" />

        <Button Text="4"
                Grid.Row="3" Grid.Column="0" 
                Command="{Binding DigitCommand}"
                CommandParameter="4" />

        <Button Text="5"
                Grid.Row="3" Grid.Column="1" 
                Command="{Binding DigitCommand}"
                CommandParameter="5" />

        <Button Text="6"
                Grid.Row="3" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="6" />

        <Button Text="1"
                Grid.Row="4" Grid.Column="0" 
                Command="{Binding DigitCommand}"
                CommandParameter="1" />

        <Button Text="2"
                Grid.Row="4" Grid.Column="1" 
                Command="{Binding DigitCommand}"
                CommandParameter="2" />

        <Button Text="3"
                Grid.Row="4" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="3" />

        <Button Text="0"
                Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding DigitCommand}"
                CommandParameter="0" />

        <Button Text="&#x00B7;"
                Grid.Row="5" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="." />
    </Grid>
</ContentPage>
```

Przyciski 11 10 cyfr i dziesiętnego udziału powiązanie z `DigitCommand`. `CommandParameter` Rozróżnia tych przycisków. Wartości `CommandParameter` zazwyczaj jest taka sama jak tekst wyświetlany przez przycisk z wyjątkiem dziesiętnym, który do celów jasności jest wyświetlany znak kropki w środkowym.

Oto program w akcji:

[![Decimal klawiatury](commanding-images/decimalkeyboard-small.png "dziesiętną klawiatury")](commanding-images/decimalkeyboard-large.png "dziesiętną klawiatury")

Należy zauważyć, że przycisk dziesiętnego wszystkie trzy zrzuty ekranu jest wyłączona, ponieważ podanej liczby zawiera już separatorem dziesiętnym. 

`DecimalKeypadViewModel` Definiuje `Entry` właściwości typu `string` (która jest jedyną właściwością, która wyzwala `PropertyChanged` zdarzeń) oraz trzy właściwości typu `ICommand`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{
    string entry = "0";

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public string Entry
    {
        private set
        {
            if (entry != value)
            {
                entry = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Entry"));
            }
        }
        get
        {
            return entry;
        }
    }

    public ICommand ClearCommand { private set; get; }

    public ICommand BackspaceCommand { private set; get; }

    public ICommand DigitCommand { private set; get; }
}
```

Przycisk odpowiadający `ClearCommand` jest zawsze włączona i ustawia wpis "0" do:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {
        ClearCommand = new Command(
            execute: () =>
            {
                Entry = "0";
                RefreshCanExecutes();
            });

        ···
    
    }

    void RefreshCanExecutes()
    {
        ((Command)BackspaceCommand).ChangeCanExecute();
        ((Command)DigitCommand).ChangeCanExecute();
    }

    ···
    
}
```

Ponieważ przycisku jest zawsze włączone, nie jest konieczne określanie `canExecute` argument `Command` konstruktora.

Logikę wprowadzania liczb i backspacing jest nieco trudnych, ponieważ jeśli nie wprowadzono żadnych cyfr, a następnie `Entry` właściwość jest ciągiem "0". Jeśli użytkownik wpisze więcej zera, a następnie `Entry` nadal zawiera co najmniej jeden zero. Jeśli użytkownik wpisze innych cyfrę, że cyfry zastępuje zero. Jednak jeśli użytkownik wpisze separatorem dziesiętnym przed dowolną cyfrę, następnie `Entry` to "0". ciąg.

**Backspace** przycisk jest aktywny tylko wtedy, gdy długość wpisu jest większa niż 1 lub jeśli `Entry` nie równa się ciąg "0":

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        BackspaceCommand = new Command(
            execute: () =>
            {
                Entry = Entry.Substring(0, Entry.Length - 1);
                if (Entry == "")
                {
                    Entry = "0";
                }
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return Entry.Length > 1 || Entry != "0";
            });

        ···

    }

    ···
    
}
```

Logikę `execute` działać w ramach **Backspace** przycisk upewnia się, że `Entry` jest co najmniej jeden ciąg "0".

`DigitCommand` Właściwość jest powiązana z przycisków, 11, z których każdy określa się `CommandParameter` właściwości. `DigitCommand` Można ustawić na wystąpienie zwykłej `Command` klasy, ale na łatwiejsze w `Command<T>` klasy ogólnej. Korzystając z języka XAML, sterująca interfejsu `CommandParameter` właściwości są zazwyczaj ciągów, a jest to typ argument rodzajowy. `execute` i `canExecute` następnie mają argumenty typu `string`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        DigitCommand = new Command<string>(
            execute: (string arg) =>
            {
                Entry += arg;
                if (Entry.StartsWith("0") && !Entry.StartsWith("0."))
                {
                    Entry = Entry.Substring(1);
                }
                RefreshCanExecutes();
            },
            canExecute: (string arg) =>
            {
                return !(arg == "." && Entry.Contains("."));
            });
    }

    ···
    
}
```

`execute` Metody dołącza argument ciągu `Entry` właściwości. Jednak jeśli wynik rozpoczyna się od zero (ale nie zero i separatorem dziesiętnym) następnie tej początkowej zero należy usunąć przy użyciu `Substring` funkcji.

`canExecute` Metoda zwraca `false` tylko wtedy, gdy argument jest dziesiętnym (co oznacza, że dziesiętnego zostanie naciśnięty) i `Entry` zawiera już separatorem dziesiętnym. 

Wszystkie `execute` wywołania metody `RefreshCanExecutes`, które następnie wywołuje `ChangeCanExecute` dla obu `DigitCommand` i `ClearCommand`. Dzięki temu, że dziesiętnego i przyciski backspace są włączone lub wyłączone na podstawie bieżącej sekwencji cyfr podana.

## <a name="adding-commands-to-existing-views"></a>Dodawanie poleceń do istniejących widoków

Jeśli chcesz korzystać z interfejsu sterująca z widoków, które nie obsługują tego, istnieje możliwość zachowanie platformy Xamarin.Forms, który konwertuje zdarzenie na polecenie. Jest to opisane w artykule [ **EventToCommandBehavior wielokrotnego użytku**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md).

## <a name="asynchronous-commanding-for-navigation-menus"></a>Asynchroniczne, wydawanie poleceń dla menu nawigacji

Steruje jest wygodną metodą wykonania menu nawigacji, takim jak w [ **pokazy powiązania danych** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) sam program. W tym miejscu jest częścią **MainPage.xaml**:


```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MainPage"
             Title="Data Binding Demos"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection Title="Basic Bindings">

                <TextCell Text="Basic Code Binding"
                          Detail="Define a data-binding in code"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicCodeBindingPage}" />

                <TextCell Text="Basic XAML Binding"
                          Detail="Define a data-binding in XAML"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicXamlBindingPage}" />

                <TextCell Text="Alternative Code Binding"
                          Detail="Define a data-binding in code without a BindingContext"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:AlternativeCodeBindingPage}" />

                ···

            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

Podczas korzystania z XAML, są droższe `CommandParameter` właściwości zwykle są ustawione na ciągi. W takim przypadku jednak używane rozszerzenie znaczników w XAML, aby `CommandParameter` jest typu `System.Type`.

Każdy `Command` właściwość jest powiązana z właściwości o nazwie `NavigateCommand`. Czy właściwość jest zdefiniowana w pliku CodeBehind **MainPage.xaml.cs**:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(
            async (Type pageType) =>
            {
                Page page = (Page)Activator.CreateInstance(pageType);
                await Navigation.PushAsync(page);
            });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Ustawia konstruktora `NavigateCommand` właściwości `execute` metodę, która tworzy `System.Type` parametr, a następnie przechodzi do niego. Ponieważ `PushAsync` wywołanie wymaga `await` operatora, `execute` — metoda musi być oznaczone jako asynchroniczne. Jest to realizowane przy użyciu `async` — słowo kluczowe przed listy parametrów. 

Ustawia również konstruktora `BindingContext` strony do samej siebie, aby odwołać powiązania `NavigateCommand` w tej klasie.

Kolejność kodu w tym konstruktorze sprawia, że różnica: `InitializeComponent` XAML do przeanalizowania powoduje wywołanie, ale w tym czasie powiązania właściwości o nazwie `NavigateCommand` nie może zostać rozpoznane, ponieważ `BindingContext` ma ustawioną wartość `null`. Jeśli `BindingContext` jest ustawiony w Konstruktorze *przed* `NavigateCommand` jest ustawiona, a następnie powiązanie mogą zostać rozwiązane, kiedy `BindingContext` jest ustawiona, ale w tym czasie `NavigateCommand` jest nadal `null`. Ustawienie `NavigateCommand` po `BindingContext` nie odniesie żadnego skutku dla powiązania ponieważ zmiana `NavigateCommand` nie wyzwalać `PropertyChanged` zdarzeń i powiązania nie może ustalić, który `NavigateCommand` jest obecnie prawidłowe.

Ustawienie obu `NavigateCommand` i `BindingContext` (w dowolnej kolejności) przed wywołaniem do `InitializeComponent` będą działać, ponieważ oba składniki powiązania są ustawiane podczas definicji powiązania napotka działanie analizatora składni języka XAML. 

Czasami może być kłopotliwe powiązań danych, ale jak przedstawiono w tej serii artykułów, są wydajne i elastyczne i znacznie pomoc do zorganizowania kodu oddzielając podstawowej logiki w interfejsie użytkownika.



## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
