---
title: Interfejs polecenia zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak implementować właściwość polecenia przy użyciu powiązania danych zestawu narzędzi Xamarin.Forms. Interfejs sterująca zapewnia podejście alternatywne do wykonywania poleceń, która znacznie lepiej nadaje się do architektury MVVM.
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: b18d042e34146a72b488da9017648a430c9cd353
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996376"
---
# <a name="the-xamarinforms-command-interface"></a>Interfejs polecenia zestawu narzędzi Xamarin.Forms

W ramach architektury Model-View-ViewModel (MVVM) powiązania danych są zdefiniowane między właściwościami w ViewModel, który jest zazwyczaj klasa, która pochodzi od klasy `INotifyPropertyChanged`, właściwości i w widoku, który zazwyczaj jest to plik XAML. Czasami aplikacja ma wymagania, które wykraczają poza te powiązania właściwość, wymagając od użytkownika zainicjować poleceń, które wpływają na element ViewModel. Te polecenia są zazwyczaj sygnalizowane, kliknięć przycisków lub palcem podsłuchu i tradycyjnie są przetwarzane w pliku związanym z kodem w obsłudze dla `Clicked` zdarzenia `Button` lub `Tapped` zdarzenia `TapGestureRecognizer`.

Interfejs sterująca zapewnia podejście alternatywne do wykonywania poleceń, która znacznie lepiej nadaje się do architektury MVVM. ViewModel, sama może zawierać polecenia, które są metodami, które są wykonywane w reakcji na określone działania w widoku, takie jak `Button` kliknij. Powiązania danych są definiowane pomiędzy tych poleceń i `Button`.

Aby zezwolić na powiązanie danych między `Button` oraz ViewModel, `Button` definiuje dwie właściwości:

- [`Command`](xref:Xamarin.Forms.Button.Command) tego typu <xref:System.Windows.Input.ICommand>
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) tego typu `Object`

Aby korzystać z interfejsu polecenia, należy zdefiniować powiązanie danych, który jest przeznaczony dla `Command` właściwość `Button` której źródłem jest właściwością w ViewModel typu `ICommand`. ViewModel zawiera kod skojarzony z tym `ICommand` właściwość, która jest wykonywana po kliknięciu przycisku. Możesz ustawić `CommandParameter` do dowolnych danych rozróżnienie między wiele przycisków, jeśli są one wszystkie powiązane do tej samej `ICommand` właściwość ViewModel.

`Command` i `CommandParameter` właściwości również są definiowane przez następujące klasy:

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) i w związku z tym [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem), która jest pochodną `MenuItem`
- [`TextCell`](xref:Xamarin.Forms.TextCell) i w związku z tym [ `ImageCell` ](xref:Xamarin.Forms.ImageCell), która jest pochodną `TextCell`
- [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)

[`SearchBar`](xref:Xamarin.Forms.SearchBar) definiuje [ `SearchCommand` ](xref:Xamarin.Forms.SearchBar.SearchCommand) właściwości typu `ICommand` i [ `SearchCommandParameter` ](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) właściwości. [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand) Właściwość [ `ListView` ](xref:Xamarin.Forms.ListView) jest również typu `ICommand`.

Te polecenia mogą być obsługiwane w ramach ViewModel w taki sposób, który nie są zależne od obiektów interfejsu użytkownika określonego w widoku.

## <a name="the-icommand-interface"></a>Interfejs ICommand

<xref:System.Windows.Input.ICommand> Interfejs nie jest częścią zestawu narzędzi Xamarin.Forms. Jest on zdefiniowany zamiast tego w [System.Windows.Input](xref:System.Windows.Input) przestrzeni nazw i składa się z dwóch metod oraz jedno zdarzenie:

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

Aby korzystać z interfejsu polecenia, Twoja ViewModel zawiera właściwości typu `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

Klasa, która implementuje również musi odwoływać się ViewModel `ICommand` interfejsu. Ta klasa opisane zostaną wkrótce. W widoku `Command` właściwość `Button` jest powiązany z tej właściwości:

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Gdy użytkownik naciśnie `Button`, `Button` wywołania `Execute` method in Class metoda `ICommand` obiekt powiązany z jego `Command` właściwości. To najprostsza część sterująca interfejsu.

`CanExecute` Metody jest bardziej złożona. Jeśli wiązanie najpierw jest zdefiniowana na `Command` właściwość `Button`, i zmianie powiązanie danych w jakiś sposób `Button` wywołania `CanExecute` method in Class metoda `ICommand` obiektu. Jeśli `CanExecute` zwraca `false`, a następnie `Button` wyłączy. Oznacza to, że określonego polecenia jest obecnie niedostępna lub nieprawidłowa.

`Button` Również dołącza obsługę na `CanExecuteChanged` zdarzenia `ICommand`. Zdarzenie jest uruchamiane z w ramach ViewModel. Po uruchomieniu zdarzenia `Button` wywołania `CanExecute` ponownie. `Button` Włącza się, jeśli `CanExecute` zwraca `true` i wyłączy, jeśli `CanExecute` zwraca `false`.

> [!IMPORTANT]
> Nie używaj `IsEnabled` właściwość `Button` Jeśli używasz interfejsu polecenia.  

## <a name="the-command-class"></a>Klasy poleceń

Gdy Twoja ViewModel definiuje okreolić, czy typ `ICommand`, ViewModel musi również zawierać lub odwoływać się do klasy, która implementuje `ICommand` interfejsu. Ta klasa musi zawierać lub odwoływać się do `Execute` i `CanExecute` metody i fire `CanExecuteChanged` zdarzenie zawsze wtedy, gdy `CanExecute` metoda może zwrócić inną wartość.

Taka klasa można napisać samodzielnie, lub można użyć klasy, który ktoś został zapisany. Ponieważ `ICommand` jest częścią programu Microsoft Windows, został on użyty przez wiele lat, za pomocą aplikacji Windows MVVM. Używanie klasy Windows, który implementuje `ICommand` umożliwia udostępnianie swoje modele widoków między aplikacjami Windows i aplikacje Xamarin.Forms.

Jeśli udostępnianie modele widoków między Windows i zestawu narzędzi Xamarin.Forms nie ma znaczenia, a następnie można użyć [ `Command` ](xref:Xamarin.Forms.Command) lub [ `Command<T>` ](xref:Xamarin.Forms.Command`1) klasy uwzględnione w interfejsie Xamarin.Forms do zaimplementowania `ICommand`interfejsu. Klasy te umożliwiają określenie organów `Execute` i `CanExecute` metod w konstruktorach klasy. Użyj `Command<T>` zastosowania `CommandParameter` właściwości w celu rozróżnienia między wieloma widokami powiązany do tej samej `ICommand` właściwość i prostszej `Command` klasy, gdy nie jest to wymagane.

## <a name="basic-commanding"></a>Podstawowe polecenia

**Wpis osoby** strony w [ **pokazy powiązania danych** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) program pokazuje kilka prostych poleceń zaimplementowane w ViewModel.

`PersonViewModel` Definiuje trzy właściwości o nazwie `Name`, `Age`, i `Skills` definiują osoby. Ta klasa jest *nie* zawierać `ICommand` właściwości:

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

`PersonCollectionViewModel` Pokazane poniżej tworzy nowe obiekty typu `PersonViewModel` i umożliwia użytkownikowi wypełnianie danych. W tym celu klasy definiuje właściwości `IsEditing` typu `bool` i `PersonEdit` typu `PersonViewModel`. Ponadto klasy definiuje trzy właściwości typu `ICommand` i właściwość o nazwie `Persons` typu `IList<PersonViewModel>`:

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

Skrócona lista nie zawiera konstruktora klasy, czyli gdzie trzy właściwości typu `ICommand` są zdefiniowane, które pojawią się wkrótce. Należy zauważyć, że zmiany do trzech właściwości typu `ICommand` i `Persons` właściwości nie powodują `PropertyChanged` zdarzenia są wywoływane. Te właściwości są gotowi, gdy tworzona jest najpierw klasy i nie należy zmieniać po tej dacie.

Przed zbadaniem konstruktora `PersonCollectionViewModel` klasy, Przyjrzyjmy się plik XAML dla **wpis osoby** program. Zawiera on `Grid` z jego `BindingContext` właściwością `PersonCollectionViewModel`. `Grid` Zawiera `Button` z tekstem **nowy** z jego `Command` właściwość powiązana z `NewCommand` właściwość w ViewModel, powiązana formularza zgłoszenia z właściwościami `IsEditing` właściwości jako również jako właściwości `PersonViewModel`, a dwa przyciski więcej powiązany z `SubmitCommand` i `CancelCommand` właściwości ViewModel. Końcowe `ListView` Wyświetla zbiór osób już wprowadzone:

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

Oto jak to działa: użytkownik naciśnie pierwszy **New** przycisku. To umożliwia formularza zgłoszenia, lecz wyłączenie **New** przycisku. Użytkownik wprowadza następnie nazwę, wiek i umiejętności. W dowolnym momencie podczas edycji, użytkownik może nacisnąć **anulować** przycisk, aby zacząć od początku. Po wprowadzeniu nazwy i prawidłowy wiek jest tylko **przesyłania** przycisk włączone. Naciśnięcie tego **przesyłania** przycisk przesyła osoby do kolekcji wyświetlanych przez `ListView`. Po wywołaniu **anulować** lub **przesyłania** przycisk jest wciśnięty, formularza zgłoszenia jest czyszczona i **nowy** przycisk jest włączony, ponownie.

Po lewej stronie ekranu w systemie iOS zawiera układ, zanim wprowadzono prawidłowy wiek. Dla systemów Android i platformy uniwersalnej systemu Windows ekrany show **przesyłania** przycisk włączone po ustawieniu wiek:

[![Wpis osoby](commanding-images/personentry-small.png "wpis osoby")](commanding-images/personentry-large.png#lightbox "wpis osoby")

Program nie ma żadnych funkcji do edycji istniejących wpisów i nie są zapisywane wpisy, gdy przejdziesz do innej strony.

Całą logikę dla **New**, **przesyłania**, i **anulować** przyciski odbywa się w `PersonCollectionViewModel` za pośrednictwem definicji `NewCommand`, `SubmitCommand`, i `CancelCommand` właściwości. Konstruktor obiektu `PersonCollectionViewModel` ustawia te trzy właściwości obiektów typu `Command`.  

A [Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) z `Command` klasy umożliwia przekazywanie argumentów typu `Action` i `Func<bool>` odpowiadający `Execute` i `CanExecute` metody. Najłatwiej jest definiowana jako wyrażenie lambda funkcji bezpośrednio w te akcje i funkcje `Command` konstruktora. Poniżej przedstawiono definicję `Command` dla obiektu `NewCommand` właściwości:

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

Kiedy użytkownik kliknie **nowy** przycisku `execute` funkcji są przekazywane do `Command` Konstruktor jest wykonywany. Spowoduje to utworzenie nowego `PersonViewModel` obiektu, ustawia program obsługi dla tego obiektu `PropertyChanged` zdarzeń, ustawia `IsEditing` do `true`i wywołuje `RefreshCanExecutes` metody zdefiniowanej po konstruktora.

Oprócz wykonywania `ICommand` interfejsu `Command` klasa definiuje również metodę o nazwie `ChangeCanExecute`. Twoje ViewModel powinny wywoływać `ChangeCanExecute` dla `ICommand` właściwość zawsze wtedy, gdy nic się stanie, które mogą ulec zmianie wartość zwracaną przez `CanExecute` metody. Wywołanie `ChangeCanExecute` powoduje, że `Command` klasy ognia `CanExecuteChanged` metody. `Button` Dołączył obsługi dla tego zdarzenia i odpowiada przez wywołanie metody `CanExecute` ponownie, a następnie włączenie oparty na wartość zwracana przez tę metodę.

Gdy `execute` metody `NewCommand` wywołania `RefreshCanExecutes`, `NewCommand` właściwości pobiera wywołanie `ChangeCanExecute`i `Button` wywołania `canExecute` metody, która zwraca teraz `false` ponieważ `IsEditing`właściwość jest obecnie `true`.

`PropertyChanged` Obsługi nowego `PersonViewModel` obiektu wywołania `ChangeCanExecute` metody `SubmitCommand`. Poniżej przedstawiono sposób implementacji właściwości polecenia:


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

`canExecute` Działać w ramach `SubmitCommand` jest wywoływana za każdym razem, gdy jest właściwością zmienione w `PersonViewModel` obiektu edytowany. Zwraca `true` tylko wtedy, gdy `Name` właściwość jest co najmniej jeden znak i `Age` jest większa niż 0. W tym czasie **przesyłania** przycisk staje się dostępny.

`execute` Działać w ramach **przesyłania** usuwa obsługi zmiany właściwości z `PersonViewModel`, dodaje obiekt do `Persons` kolekcji i zwraca wszystkie elementy do warunków początkowych.

`execute` Działać w ramach **anulować** przycisk robi to wszystko, **przesyłania** execept ma przycisk Dodaj obiekt do kolekcji:

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

Techniki te można dostosować tak, aby bardziej złożonych scenariuszy: właściwość `PersonCollectionViewModel` może być powiązane z `SelectedItem` właściwość `ListView` do edycji istniejących elementów i **Usuń** przycisk może zostać dodany do usunięcia te elementy.

Nie trzeba zdefiniować `execute` i `canExecute` metod jako funkcje lambda. Możesz zapisać je jako zwykłe metody prywatne w ViewModel i odwoływać się do nich w `Command` konstruktorów. Jednak to podejście zazwyczaj powodują powstanie wielu metod, do których odwołują się tylko raz w ViewModel.

## <a name="using-command-parameters"></a>Przy użyciu parametrów polecenia

Czasami jest to wygodne dla jednego lub więcej przycisków (lub innych obiektów interfejsu użytkownika) współużytkować ten sam `ICommand` właściwość ViewModel. W tym przypadku używasz `CommandParameter` właściwości w celu rozróżnienia między przyciskami.

Będzie można kontynuować używanie `Command` klasę tymi wspólnymi `ICommand` właściwości. Definiuje klasę [alternatywny Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action{System.Object},System.Func{System.Object,System.Boolean})) akceptujący `execute` i `canExecute` metody z parametrami typu `Object`. Jest to sposób, w jaki `CommandParameter` jest przekazywana do tych metod.

Jednak przy użyciu `CommandParameter`, najłatwiej musieli używać ogólnych [ `Command<T>` ](xref:Xamarin.Forms.Command`1) klasy, aby określić typ obiektu, ustaw `CommandParameter`. `execute` i `canExecute` metody, należy określić, które mają parametry tego typu.

**Dziesiętna klawiatury** strony pokazano tej techniki, przedstawiając sposób implementacji klawiaturę do wprowadzania liczb dziesiętnych. `BindingContext` Dla `Grid` jest `DecimalKeypadViewModel`. `Entry` Właściwość ta ViewModel jest powiązana z `Text` właściwość `Label`. Wszystkie `Button` obiekty są powiązane z różnymi poleceniami w ViewModel: `ClearCommand`, `BackspaceCommand`, i `DigitCommand`:

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

Przyciski 11 dla 10 cyfr i separator dziesiętny udostępnianie powiązanie z `DigitCommand`. `CommandParameter` Rozróżnia tych przycisków. Wartość równa `CommandParameter` jest zwykle taka sama jak tekst wyświetlany przez przycisk z wyjątkiem punktu dziesiętnego, wyświetlanych na potrzeby w celu uściślenia przy użyciu znaku kropki środkowej.

Poniżej przedstawiono program w akcji:

[![Klawiatura dziesiętną](commanding-images/decimalkeyboard-small.png "dziesiętna klawiatury")](commanding-images/decimalkeyboard-large.png#lightbox "dziesiętna klawiatury")

Należy zauważyć, że przycisk punktu dziesiętnego, wszystkie trzy zrzuty ekranu jest wyłączona, ponieważ podanej liczby już zawiera separator dziesiętny.

`DecimalKeypadViewModel` Definiuje `Entry` właściwości typu `string` (która jest tylko właściwość, która wyzwala `PropertyChanged` zdarzeń) i trzech właściwości typu `ICommand`:

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

Przycisk odpowiadający `ClearCommand` jest zawsze włączona i po prostu ustawia wpis "0" do:

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

Ponieważ ten przycisk jest zawsze włączone, nie jest konieczne określenie `canExecute` argument `Command` konstruktora.

Logika do wprowadzania liczb i backspacing jest trudna, ponieważ jeśli zostały wprowadzone nie cyfry, a następnie `Entry` właściwość jest ciąg "0". Jeśli użytkownik wpisze więcej zer, a następnie `Entry` nadal zawiera tylko jedno zero. Jeśli użytkownik wpisze jakąkolwiek inną cyfrę, ta cyfra zamienia zero. Jednak jeśli użytkownik wpisze separator dziesiętny przed jakąkolwiek inną cyfrę, następnie `Entry` jest ciąg "0.".

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

Logikę `execute` działać w ramach **Backspace** przycisku masz pewność, że `Entry` jest co najmniej jeden ciąg "0".

`DigitCommand` Właściwość jest powiązana z przycisków, 11, z których każdy identyfikowany za pomocą `CommandParameter` właściwości. `DigitCommand` Mógł zostać ustawiony na wystąpienie normalnej `Command` klasy, ale jego łatwiejszy w obsłudze `Command<T>` klasy ogólnej. W przypadku korzystania z interfejsu sterująca z XAML, `CommandParameter` właściwości są zwykle ciągów i jest to typ argument ogólny. `execute` i `canExecute` następnie mają argumenty typu `string`:

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

`execute` Metoda dołącza argument typu string `Entry` właściwości. Jednakże, jeśli wynik, który rozpoczyna się od zera (ale nie zero i separator dziesiętny) następnie tej początkowej zero musi zostać usunięta za pomocą `Substring` funkcji.

`canExecute` Metoda zwraca `false` tylko wtedy, gdy argument jest przecinka dziesiętnego (wskazuje, czy punkt dziesiętny jest naciskana) i `Entry` już zawiera punkt dziesiętny.

Wszystkie `execute` wywołanie metody `RefreshCanExecutes`, która następnie wywołuje metodę `ChangeCanExecute` dla obu `DigitCommand` i `ClearCommand`. Daje to gwarancję, że punkt dziesiętny i backspace przyciski są włączone lub wyłączone na podstawie bieżącego sekwencji cyfr podana.

## <a name="adding-commands-to-existing-views"></a>Dodawanie poleceń do istniejących widoków

Jeśli chcesz sterująca interfejs za pomocą widoków, które nie obsługują tego, jest możliwe użycie zachowania zestawu narzędzi Xamarin.Forms, które konwertuje zdarzenie na polecenia. Jest to opisane w artykule [ **EventToCommandBehavior wielokrotnego użytku, do**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md).

## <a name="asynchronous-commanding-for-navigation-menus"></a>Asynchroniczne, polecenia menu nawigacji

Polecenia jest wygodne w przypadku implementowania menu nawigacji, takie jak w [ **pokazy powiązania danych** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) samego programu. Poniżej przedstawiono część **MainPage.xaml**:


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

Korzystając z polecenia przy użyciu XAML, `CommandParameter` właściwości są zwykle ustawiane na ciągi. W tym przypadku, jednak rozszerzenie znaczników XAML jest używana, aby `CommandParameter` typu `System.Type`.

Każdy `Command` właściwość jest powiązana z właściwością o nazwie `NavigateCommand`. Czy właściwość jest zdefiniowana w pliku związanym z kodem **MainPage.xaml.cs**:

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

Ustawia konstruktora `NavigateCommand` właściwości `execute` metody, która tworzy wystąpienie `System.Type` parametru i następnie przechodzi do niego. Ponieważ `PushAsync` wywołanie wymaga `await` operatora `execute` metoda musi mieć flagę jako asynchroniczne. Jest to realizowane przy `async` — słowo kluczowe przed listą parametrów.

Ustawia również konstruktora `BindingContext` strony do samego siebie, aby odwoływać się do powiązania `NavigateCommand` w tej klasie.

Kolejność kodu w tym konstruktorze referacie: `InitializeComponent` wywołanie powoduje, że XAML, który ma być analizowany, ale w tym czasie powiązania z właściwością o nazwie `NavigateCommand` nie może zostać rozpoznane, ponieważ `BindingContext` ustawiono `null`. Jeśli `BindingContext` jest ustawiony w Konstruktorze *przed* `NavigateCommand` jest ustawiona, a następnie powiązanie może zostać rozwiązany, kiedy `BindingContext` jest ustawiona, ale w tym czasie `NavigateCommand` jest nadal `null`. Ustawienie `NavigateCommand` po `BindingContext` będzie mieć wpływu na powiązania, ponieważ zmiana `NavigateCommand` nie zostanie wyzwolony `PropertyChanged` zdarzeń i powiązania nie wie, że `NavigateCommand` jest obecnie prawidłowe.

Ustawienie oba `NavigateCommand` i `BindingContext` (w dowolnej kolejności) przed wywołaniem do `InitializeComponent` będą działać, ponieważ oba składniki powiązania są ustawiane w chwili XAML analizator składni napotka definicji powiązania.

Powiązania danych czasami może być trudne, ale jak przedstawiono w tej serii artykułów, są zaawansowane i wszechstronne i znacznie ułatwiają organizowanie kodu, oddzielając podstawowej logiki z poziomu interfejsu użytkownika.



## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
