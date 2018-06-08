---
title: Przycisk platformy Xamarin.Forms
description: Przycisk odpowiada naciśnij lub kliknij przycisk kierujący aplikacji do wykonania określonego zadania.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/01/2018
ms.openlocfilehash: 1fed439ecb4bd79bd84974ea1397ca0ed1336b62
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847956"
---
# <a name="xamarinforms-button"></a>Przycisk platformy Xamarin.Forms

_Przycisk odpowiada naciśnij lub kliknij przycisk kierujący aplikacji do wykonania określonego zadania._ 

[ `Button` ](xref:Xamarin.Forms.Button) Jest najbardziej podstawową sterowania interaktywnego we wszystkich platformy Xamarin.Forms. `Button` Zwykle wyświetla krótki ciąg tekstowy wskazujący polecenia, ale można również wyświetlić obraz mapy bitowej lub kombinacji tekstu i obrazów. Użytkownik naciśnie `Button` z palcem lub kliknięciu myszą do zainicjowania tego polecenia.

Większość tematów omówiony poniżej odpowiadają stronom w [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) próbki.

## <a name="handling-button-clicks"></a>Obsługa przycisku kliknie

`Button` definiuje [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) zdarzenie, które jest wywoływane po naciśnięciu `Button` za pomocą wskaźnika myszy lub linii papilarnych. Zdarzenie jest wywoływane po zwolnieniu przycisku myszy lub linii papilarnych z powierzchni `Button`. `Button` Musi mieć jego [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) ustawioną właściwość `true` dla niego odpowiedzieć podsłuchu. 

**Podstawowe kliknij przycisk** w obszarze [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) przykładowych pokazano, jak utworzyć wystąpienia `Button` w kodzie XAML i obsługi jej `Clicked` zdarzeń. **BasicButtonClickPage.xaml** plik zawiera `StackLayout` zarówno `Label` i `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.BasicButtonClickPage"
             Title="Basic Button Click">
    <StackLayout>
        
        <Label x:Name="label"
               Text="Click the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand" 
               HorizontalOptions="Center" />

        <Button Text="Click to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Clicked="OnButtonClicked" />
     
    </StackLayout>
</ContentPage>
```

`Button` Zwykle zajmują to miejsce, który jest dozwolony dla niego. Na przykład, jeśli nie zostanie ustawiona `HorizontalOptions` właściwość `Button` do czegoś innego niż `Fill`, `Button` zajmie całą szerokość jego elementu nadrzędnego.

Domyślnie `Button` jest prostokątny, ale można nadać it zaokrąglone narożniki za pomocą [ `CornerRadius` ](xref:Xamarin.Forms.Button.CornerRadius) właściwości, zgodnie z poniższym opisem w sekcji [ **wygląd przycisku** ](#button-appearance).

[ `Text` ](xref:Xamarin.Forms.Button.Text) Właściwość określa tekst wyświetlany w `Button`. [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) Zdarzeń ustawiono na program obsługi zdarzeń o nazwie `OnButtonClicked`. Ten program obsługi znajduje się w pliku CodeBehind **BasicButtonClickPage.xaml.cs**:

```csharp
public partial class BasicButtonClickPage : ContentPage
{
    public BasicButtonClickPage ()
    {
        InitializeComponent ();
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        await label.RelRotateTo(360, 1000);
    }
}
```

Gdy `Button` jest dotknięciu `OnButtonClicked` metoda jest wykonywana. `sender` Argument jest `Button` obiekt odpowiedzialny dla tego zdarzenia. Umożliwia to dostęp `Button` obiektu, albo w celu rozróżnienia między wieloma `Button` udostępnianie takie same obiekty `Clicked` zdarzeń.

W szczególności ten `Clicked` obsługi wywołuje funkcję animacji, który przełącza `Label` 360 stopni w 1000 milisekund. W tym miejscu jest uruchomiony na urządzeniach z systemem Android i iOS, a także jako aplikacja systemu Windows platformy Uniwersalnej na pulpicie systemu Windows 10 program:

[![Kliknij przycisk podstawowe](button-images/BasicButtonClick.png "kliknij przycisk podstawowe")](button-images/BasicButtonClick-Large.png#lightbox "kliknij przycisk podstawowe")

Zwróć uwagę, że `OnButtonClicked` metoda zawiera `async` modyfikator ponieważ `await` jest używany w ramach programu obsługi zdarzeń. A `Clicked` wymaga obsługi zdarzeń `async` modyfikator tylko wtedy, gdy treść program obsługi używa `await`.

Renderuje każdej platformy `Button` w określonego rodzaju. W [ **przycisk wygląd** ](#button-appearance) sekcji, zobaczysz ustawiania kolorów i upewnij `Button` obramowania widoczna dla bardziej dostosowanego wyglądu. `Button` implementuje [ `IFontElement` ](xref:Xamarin.Forms.Internals.IFontElement) interfejsu, aby zawierał [ `FontFamily` ](xref:Xamarin.Forms.Button.FontFamily), [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize), i [ `FontAttributes` ](xref:Xamarin.Forms.Button.FontAttributes)właściwości.

## <a name="creating-a-button-in-code"></a>Tworzenie przycisku w kodzie

Często można utworzyć wystąpienia `Button` w języku XAML, ale można również utworzyć `Button` w kodzie. Może to być wygodne, gdy aplikacja potrzebuje do utworzenia wielu przycisków na podstawie danych, która jest wyliczalny z `foreach` pętli.

**Kliknij przycisk kod** strony pokazano, jak utworzyć stronę, która jest funkcjonalnym odpowiednikiem **podstawowe kliknij przycisk** strony, ale wyłącznie w języku C#:

```csharp
public class CodeButtonClickPage : ContentPage
{
    public CodeButtonClickPage ()
    {
        Title = "Code Button Click";

        Label label = new Label
        {
            Text = "Click the Button below",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };

        Button button = new Button
        {
            Text = "Click to Rotate Text!",
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };
        button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);

        Content = new StackLayout
        {
            Children =
            {
                label,
                button
            }
        };
    }
}
```

Wszystko jest realizowane w Konstruktorze tej klasy. Ponieważ `Clicked` program obsługi jest tylko jednej instrukcji, które długie, będzie można dołączyć do zdarzenia bardzo prosty:

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

Oczywiście istnieje także możliwość zdefiniowania programu obsługi zdarzeń jako osobne metody (podobnie jak `OnButtonClick` metody w **podstawowe kliknij przycisk**) i dołączenie tej metody w zdarzeniu:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>Wyłączenie przycisku

Czasami aplikacji jest w określonym stanie w przypadku, gdy określonego `Button` kliknij nie jest prawidłową operacją. W takich przypadkach `Button` powinny być wyłączone przez ustawienie jej `IsEnabled` właściwości `false`. Przykład klasycznego `Entry` kontroli dla nazwy pliku wraz z otwartego pliku `Button`: `Button` powinna być włączona tylko wtedy, gdy wpisywać tekst `Entry`. Można użyć `DataTrigger` dla tego zadania, jak pokazano w [ **wyzwalaczy danych** ](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) artykułu.

## <a name="using-the-command-interface"></a>Przy użyciu interfejsu polecenia

Istnieje możliwość aplikacji odpowiedź na `Button` podsłuchu bez obsługi `Clicked` zdarzeń. `Button` Implementuje mechanizm alternatywny powiadomienia o nazwie _polecenia_ lub _droższe_ interfejsu. Ten krok składa się z dwóch właściwości:

- [`Command`](xref:Xamarin.Forms.Button.Command) typu [ `ICommand` ](xref:System.Windows.Input.ICommand), interfejsem zdefiniowanym w [ `System.Windows.Input` ](xref:System.Windows.Input) przestrzeni nazw.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) Właściwość typu [ `Object` ](xref:System.Object).

Ta metoda jest szczególnie odpowiedniego połączenia z programem wiązania danych, a zwłaszcza w przypadku implementowania architektury Model-View-ViewModel (MVVM). W tych tematach omówiono w artykułach [powiązania danych](~/xamarin-forms/app-fundamentals/data-binding/index.md), [z powiązania danych z modelem MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), i [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md).

W aplikacji MVVM ViewModel definiuje właściwości typu `ICommand` następnie podłączone do XAML `Button` elementy z powiązaniami danych. Definiuje również platformy Xamarin.Forms [ `Command` ]((xref:Xamarin.Forms.Command`1)) i [ `Command<T>` ](xref:Xamarin.Forms.Command`1) klas, które implementują `ICommand` interfejsu i ułatwić ViewModel Definiowanie właściwości typu `ICommand`.

Steruje jest opisany bardziej szczegółowo w artykule [ **interfejsu polecenia** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) , ale **podstawowe przycisk polecenia** strony [  **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) pokazano podejście podstawowe.

`CommandDemoViewModel` Klasy jest bardzo prosty ViewModel, który definiuje właściwość typu `double` o nazwie `Number`, a dwie właściwości typu `ICommand` o nazwie `MultiplyBy2Command` i `DivideBy2Command`:

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    double number = 1;

    public event PropertyChangedEventHandler PropertyChanged;

    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(() => Number *= 2);

        DivideBy2Command = new Command(() => Number /= 2);
    }

    public double Number
    {
        set
        {
            if (number != value)
            {
                number = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Number"));
            }
        }
        get
        {
            return number;
        }
    }

    public ICommand MultiplyBy2Command { private set; get; }

    public ICommand DivideBy2Command { private set; get; }
}
```

Dwa `ICommand` właściwości są inicjowane w Konstruktorze tej klasy z dwóch obiektów typu `Command`. `Command` Konstruktorów oferuje nieco (nazywane `execute` argumentów konstruktora) który podwaja albo połowy `Number` właściwości.

**BasicButtonCommand.xaml** plików zestawów jego `BindingContext` na wystąpienie `CommandDemoViewModel`. `Label` Elementu i dwa `Button` elementów zawiera powiązań z trzech właściwości w `CommandDemoViewModel`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.BasicButtonCommandPage"
             Title="Basic Button Command">
    
    <ContentPage.BindingContext>
        <local:CommandDemoViewModel />
    </ContentPage.BindingContext>
    
    <StackLayout>
        <Label Text="{Binding Number, StringFormat='Value is now {0}'}"
               FontSize="Large"
               VerticalOptions="CenterAndExpand" 
               HorizontalOptions="Center" />

        <Button Text="Multiply by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding MultiplyBy2Command}" />

        <Button Text="Divide by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding DivideBy2Command}" />
    </StackLayout>
</ContentPage>
```

Jako dwa `Button` elementy widoczne są wybierane, polecenia zostaną wykonane, oraz liczbę zmienia wartość:

[![Polecenie przycisk podstawowe](button-images/BasicButtonCommand.png "podstawowe przycisku polecenia")](button-images/BasicButtonCommand-Large.png#lightbox)

Zaletą tej metody za pośrednictwem `Clicked` obsługi jest, że całą logikę obejmujące funkcjonalność ta strona znajduje się w ViewModel zamiast pliku CodeBehind osiągnięcia lepszej separacji interfejsu użytkownika z logiką biznesową.

Istnieje również możliwość `Command` obiekty do kontrolowania Włączanie i wyłączanie `Button` elementów. Załóżmy na przykład, aby ograniczyć zakres wartości liczbowe między 2<sup>10</sup> i 2<sup>&ndash;10</sup>. Inną funkcję można dodać do konstruktora (nazywane `canExecute` argument) zwracającą `true` Jeśli `Button` powinno być włączone. Oto modyfikację `CommandDemoViewModel` konstruktora:

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    ···
    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(
            execute: () =>
            {
                Number *= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number < Math.Pow(2, 10));

        DivideBy2Command = new Command(
            execute: () =>
            {
                Number /= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number > Math.Pow(2, -10));
    }
    ···
}
```

Wywołania `ChangeCanExecute` metody `Command` są niezbędne, aby `Command` można wywołać metody `canExecute` — metoda i określić, czy `Button` powinny być wyłączone lub nie. Dzięki tej zmianie kod jako numer osiągnie limit, `Button` jest wyłączone:

[![Polecenie przycisk podstawowe - zmodyfikowane](button-images/BasicButtonCommandModified.png "podstawowe przycisk polecenia - zmodyfikowane")](button-images/BasicButtonCommandModified-Large.png#lightbox)

Istnieje możliwość co najmniej dwóch `Button` elementy, które można powiązać do tej samej `ICommand` właściwości. `Button` Elementów można rozróżnić za pomocą [ `CommandParameter` ](xref:Xamarin.Forms.Button.CommandParameter) właściwość `Button`. W takim przypadku należy używać ogólnych [ `Command<T>` ](xref:Xamarin.Forms.Command`1) klasy. `CommandParameter` Obiektu są następnie przekazywane jako argument `execute` i `canExecute` metody. Ta technika jest wyświetlany w obszarze szczegółów [ **podstawowe droższe** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) sekcji [ **interfejsu polecenia** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) artykułu.

[ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) próbki również korzysta z tej techniki w jego `MainPage` klasy. **MainPage.xaml** plik zawiera `Button` dla każdej strony przykładu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.MainPage"
             Title="Button Demos">
    <ScrollView>
        <FlexLayout Direction="Column"
                    JustifyContent="SpaceEvenly"
                    AlignItems="Center">

            <Button Text="Basic Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonClickPage}" />

            <Button Text="Code Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:CodeButtonClickPage}" />

            <Button Text="Basic Button Command"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonCommandPage}" />

            <Button Text="Press and Release Button"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:PressAndReleaseButtonPage}" />

            <Button Text="Button Appearance"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ButtonAppearancePage}" />

            <Button Text="Toggle Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ToggleButtonDemoPage}" />

            <Button Text="Image Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ImageButtonDemoPage}" />

        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Każdy `Button` ma jego `Command` właściwość powiązana z właściwości o nazwie `NavigateCommand`i `CommandParameter` ustawiono [ `Type` ](xref:System.Type) obiektu odpowiadającego do jednej z klas strony w projekcie.

Czy `NavigateCommand` właściwość jest typu `ICommand` i jest zdefiniowany w pliku związanym z kodem:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Konstruktor inicjuje `NavigateCommand` właściwości `Command<Type>` obiektu, ponieważ `Type` jest typem `CommandParameter` obiektu ustawione w pliku XAML. Oznacza to, że `execute` metoda ma argumentu typu `Type` , który odpowiada to `CommandParameter` obiektu. Funkcja tworzy stronę, a następnie przechodzi do niego.

Należy zauważyć, że Konstruktor stwierdza, ustawiając jego `BindingContext` do samej siebie. Jest to niezbędne do właściwości w pliku XAML, aby powiązać `NavigateCommand` właściwości.

## <a name="pressing-and-releasing-the-button"></a>Naciśnięcie klawisza i zwolnieniem przycisku

Oprócz `Clicked` zdarzenia `Button` definiuje również [ `Pressed` ](xref:Xamarin.Forms.Button.Pressed) i [ `Released` ](xref:Xamarin.Forms.Button.Released) zdarzenia. `Pressed` Zdarzenie wystąpi, gdy palcem naciśnie w `Button`, lub przycisk myszy jest wciśnięty z umieść wskaźnik `Button`. `Released` Zdarzeń występuje po zwolnieniu przycisku myszy lub linii papilarnych. Ogólnie rzecz biorąc `Clicked` zdarzenie jest również uruchamiane w tym samym czasie jako `Released` zdarzeń, ale jeśli wskaźnika myszy lub palca slajdów od powierzchni `Button` przed został wydany, `Clicked` zdarzenia nie mogą wystąpić.

`Pressed` i `Released` zdarzenia nie są często używane, ale można ich wykorzystanie do celów specjalnych, jak pokazano w **naciśnij i przycisk** strony. Plik XAML zawiera `Label` i `Button` z procedurami obsługi dołączonymi do `Pressed` i `Released` zdarzenia:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.PressAndReleaseButtonPage"
             Title="Press and Release Button">
    <StackLayout>

        <Label x:Name="label"
               Text="Press and hold the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand" 
               HorizontalOptions="Center" />

        <Button Text="Press to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Pressed="OnButtonPressed"
                Released="OnButtonReleased" />

    </StackLayout>
</ContentPage>
```

Animuje pliku CodeBehind `Label` podczas `Pressed` zdarzenie występuje, ale wstrzymuje obrót podczas `Released` zdarzenie:

```csharp
public partial class PressAndReleaseButtonPage : ContentPage
{
    bool animationInProgress = false;
    Stopwatch stopwatch = new Stopwatch();

    public PressAndReleaseButtonPage ()
    {
        InitializeComponent ();
    }

    void OnButtonPressed(object sender, EventArgs args)
    {
        stopwatch.Start();
        animationInProgress = true;

        Device.StartTimer(TimeSpan.FromMilliseconds(16), () =>
        {
            label.Rotation = 360 * (stopwatch.Elapsed.TotalSeconds % 1);

            return animationInProgress;
        });
    }

    void OnButtonReleased(object sender, EventArgs args)
    {
        animationInProgress = false;
        stopwatch.Stop();
    }
}
```

W wyniku `Label` tylko obraca zablokowaniu palcem z `Button`i zatrzymuje po zwolnieniu palca:

[![Naciśnij i zwolnij przycisk](button-images/PressAndReleaseButton.png "naciśnij i zwolnij przycisk")](button-images/PressAndReleaseButton-Large.png)

Tego rodzaju zachowanie ma aplikacji dotyczące gier: palcem dniach `Button` może uniemożliwić na obiekt ekranu przenieść w określonym kierunku. 

<a name="button-appearance" />

## <a name="button-appearance"></a>Wygląd przycisku

`Button` Dziedziczy lub definiuje kilka właściwości, które mają wpływ na jego wygląd:

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) kolor jest `Button` tekstu
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) jest to kolor tła do tekstu
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) jest to kolor otaczającego obszar `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) rodziny czcionek są używane dla tekstu
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) rozmiar tekstu
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) Wskazuje, czy tekst jest pogrubiony lub kursywa
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) Szerokość obramowania 
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) Zaokrągla liczbę w narożnikach

Efekty sześć tych właściwości (z wyłączeniem `FontFamily` i `FontAttributes`) przedstawiono w części **wygląd przycisku** strony. Inna właściwość [ `Image` ](xref:Xamarin.Forms.Button.Image), została szczegółowo opisana w sekcji [ **przy użyciu map bitowych z przyciskiem**](#image-button).

Wszystkie widoki i danych powiązania w **wygląd przycisku** strony są zdefiniowane w pliku XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ButtonAppearancePage"
             Title="Button Appearance">
    <StackLayout>
        <Button x:Name="button"
                Text="Button"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                TextColor="{Binding Source={x:Reference textColorPicker},
                                    Path=SelectedItem.Color}"
                BackgroundColor="{Binding Source={x:Reference backgroundColorPicker},
                                          Path=SelectedItem.Color}"
                BorderColor="{Binding Source={x:Reference borderColorPicker},
                                      Path=SelectedItem.Color}" />

        <StackLayout BindingContext="{x:Reference button}"
                     Padding="10">
            
            <Slider x:Name="fontSizeSlider"
                    Maximum="48"
                    Minimum="1"
                    Value="{Binding FontSize}" />

            <Label Text="{Binding Source={x:Reference fontSizeSlider},
                                  Path=Value,
                                  StringFormat='FontSize = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="borderWidthSlider"
                    Minimum="-1"
                    Maximum="12"
                    Value="{Binding BorderWidth}" />
            
            <Label Text="{Binding Source={x:Reference borderWidthSlider}, 
                                  Path=Value,
                                  StringFormat='BorderWidth = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="cornerRadiusSlider"
                    Minimum="-1"
                    Maximum="24"
                    Value="{Binding CornerRadius}" />

            <Label Text="{Binding Source={x:Reference cornerRadiusSlider}, 
                                  Path=Value,
                                  StringFormat='CornerRadius = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.Resources>
                    <Style TargetType="Label">
                        <Setter Property="VerticalOptions" Value="Center" />
                    </Style>
                </Grid.Resources>

                <Label Text="Text Color:"
                       Grid.Row="0" Grid.Column="0" />

                <Picker x:Name="textColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="0" Grid.Column="1" />

                <Label Text="Background Color:"
                       Grid.Row="1" Grid.Column="0" />

                <Picker x:Name="backgroundColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="1" Grid.Column="1" />

                <Label Text="Border Color:"
                       Grid.Row="2" Grid.Column="0" />

                <Picker x:Name="borderColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="2" Grid.Column="1" />
            </Grid>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

`Button` w górnej części strony ma trzy jego `Color` właściwości powiązany z `Picker` elementów w dolnej części strony. Elementy w `Picker` elementy są kolory z `NamedColor` klasy dołączony do projektu. Trzy `Slider` elementy zawierają dwukierunkowego powiązania z `FontSize`, `BorderWidth`, i `CornerRadius` właściwości `Button`.

Ten program umożliwia eksperymentów z kombinacji tych właściwości:

[![Przycisk wygląd](button-images/ButtonAppearance.png "wygląd przycisku")](button-images/ButtonAppearance-Large.png)

Aby wyświetlić `Button` obramowania, musisz ustawić `BorderColor` do czegoś innego niż `Default`i `BorderWidth` na wartość dodatnią.

W systemach iOS, można zauważyć, że szerokości obramowania dużych mającym do wnętrza `Button` zakłócać wyświetlanie tekstu. Jeśli chcesz użyć obramowanie z systemem iOS `Button`, prawdopodobnie należy do rozpoczęcia i zakończenia `Text` właściwości z funkcją miejsca do przechowywania jego widoczność.

Na platformy uniwersalnej systemu Windows wybierając `CornerRadius` przekraczający połowę wysokości `Button` zgłasza wyjątek.

## <a name="creating-a-toggle-button"></a>Tworzenie przycisk przełącznika

Istnieje możliwość podklasy `Button` , która działa jak wyłącznik: naciśnij przycisk raz przycisk przełączania na i wybierz go ponownie, aby przełączyć go wyłączyć.

Następujące `ToggleButton` pochodną klasy `Button` i definiuje nowego zdarzenia o nazwie `Toggled` i właściwości typu Boolean o nazwie `IsToggled`. Są to te same właściwości dwóch zdefiniowane przez platformy Xamarin.Forms [ `Switch` ](xref:Xamarin.Forms.Switch):

```csharp
class ToggleButton : Button
{
    public event EventHandler<ToggledEventArgs> Toggled;

    public static BindableProperty IsToggledProperty =
        BindableProperty.Create("IsToggled", typeof(bool), typeof(ToggleButton), false,
                                propertyChanged: OnIsToggledChanged);

    public ToggleButton()
    {
        Clicked += (sender, args) => IsToggled ^= true;
    }

    public bool IsToggled
    {
        set { SetValue(IsToggledProperty, value); }
        get { return (bool)GetValue(IsToggledProperty); }
    }

    protected override void OnParentSet()
    {
        base.OnParentSet();
        VisualStateManager.GoToState(this, "ToggledOff");
    }

    static void OnIsToggledChanged(BindableObject bindable, object oldValue, object newValue)
    {
        ToggleButton toggleButton = (ToggleButton)bindable;
        bool isToggled = (bool)newValue;

        // Fire event
        toggleButton.Toggled?.Invoke(toggleButton, new ToggledEventArgs(isToggled));

        // Set the visual state
        VisualStateManager.GoToState(toggleButton, isToggled ? "ToggledOn" : "ToggledOff");
    }
}
```

`ToggleButton` Konstruktor dołącza program obsługi do `Clicked` zdarzenie, którego nie można zmienić wartości `IsToggled` właściwości. `OnIsToggledChanged` Uruchamiany metody `Toggled` zdarzeń. 

Ostatni wiersz `OnIsToggledChanged` wywołania metod statycznych `VisualStateManager.GoToState` metody tekstem dwa ciągi "ToggledOn" i "ToggledOff". Możesz przeczytać temat tej metody i jak aplikacja może odpowiadać na stany wizualne w artykule [ **platformy Xamarin.Forms Visual State Manager**](~/xamarin-forms/user-interface/visual-state-manager.md). 

Ponieważ `ToggleButton` sprawia, że wywołanie `VisualStateManager.GoToState`, ta sama klasa nie musi zawierać żadnych dodatkowych urządzeń, aby zmienić wygląd przycisku na podstawie jego `IsToggled` stanu. Oznacza to odpowiedzialność XAML, który jest hostem `ToggleButton`. 

**Pokaz przycisk przełączania** strona zawiera dwa wystąpienia `ToggleButton`, w tym Visual State Manager kod znaczników, który ustawia `Text`, `BackgroundColor`, i `TextColor` przycisku oparte na stan wizualny: 

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ToggleButtonDemoPage"
             Title="Toggle Button Demo">
    
    <ContentPage.Resources>
        <Style TargetType="local:ToggleButton">
            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            <Setter Property="HorizontalOptions" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <local:ToggleButton Toggled="OnItalicButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Italic Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>
                    
                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Italic On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <local:ToggleButton Toggled="OnBoldButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Bold Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>
                    
                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Bold On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <Label x:Name="label"
               Text="Just a little passage of some sample text that can be formatted in italic or boldface by toggling the two buttons."
               FontSize="Large"
               HorizontalTextAlignment="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

`Toggled` Programów obsługi zdarzeń znajdują się w pliku CodeBehind. Są one odpowiedzialne za ustawienie `FontAttributes` właściwość `Label` oparte na stanie przyciski:

```csharp
public partial class ToggleButtonDemoPage : ContentPage
{
    public ToggleButtonDemoPage ()
    {
        InitializeComponent ();
    }

    void OnItalicButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Italic;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Italic;
        }
    }

    void OnBoldButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Bold;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Bold;
        }
    }
}
```

Oto programu uruchomionego w systemach iOS, Android i platformy uniwersalnej systemu Windows:

[![Przełącz przycisk pokaz](button-images/ToggleButtonDemo.png "Przełącz przycisk pokaz")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>Przy użyciu mapy bitowe przycisków

`Button` Klasa definiuje [ `Image` ](xref:Xamarin.Forms.Button.Image) właściwość, która służy do wyświetlania obrazu mapy bitowej na `Button`, samodzielnie lub w połączeniu z tekstem. Można również określić układ tekstowych i obrazów.

`Image` Właściwość jest typu [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource), co oznacza, że map bitowych muszą być przechowywane jako zasoby w projektach poszczególnych platform, a nie w .NET Standard projektu biblioteki. 

Każdej z platform obsługiwanych przez platformy Xamarin.Forms umożliwia obrazów, które mają być przechowywane w różnych rozmiarach dla różnych pikseli rozdzielczości różnych urządzeń, których aplikacja może działać na. Te wiele map bitowych o nazwie lub przechowywane w taki sposób, system operacyjny może wybrać najlepsze dopasowanie podczas urządzenia wideo rozdzielczości ekranu. 

Dla mapy bitowej na `Button`najlepszy rozmiar jest zwykle między 32- i 64 jednostki niezależnych od urządzenia, w zależności od wielkości ma być. Obrazy używane w tym przykładzie są oparte na rozmiar 48 jednostek niezależnych od urządzenia.

W projekcie systemu iOS **zasobów** folder zawiera trzy rozmiary tego obrazu:

- Mapy bitowej 48-piksel kwadratowe, przechowywane jako **/Resources/MonkeyFace.png**
- Mapy bitowej 96 piksel kwadratowe, przechowywane jako **/Resource/MonkeyFace@2x.png**
- Mapy bitowej 144 piksel kwadratowe, przechowywane jako **/Resource/MonkeyFace@3x.png**

Zostały podane wszystkie trzy map bitowych **Akcja kompilacji** z **BundleResource**.

Dla projektu systemu Android, mapy bitowe wszystkie mają taką samą nazwę, ale są one przechowywane w różnych podfolderach **zasobów** folderu:

- Mapy bitowej 72 piksel kwadratowe, przechowywane jako **/Resources/drawable-hdpi/MonkeyFace.png**
- Mapy bitowej 96 piksel kwadratowe, przechowywane jako **/Resources/drawable-xhdpi/MonkeyFace.png**
- Mapy bitowej 144 piksel kwadratowe, przechowywane jako **/Resources/drawable-xxhdpi/MonkeyFace.png**
- Mapy bitowej 192 piksel kwadratowe, przechowywane jako **/Resources/drawable-xxxhdpi/MonkeyFace.png**

Te zostały podane **Akcja kompilacji** z **AndroidResource**.

W projekcie platformy UWP, mapy bitowe mogą być przechowywane w dowolnym miejscu w projekcie, ale zazwyczaj są przechowywane w folderze niestandardowych lub **zasoby** istniejącego folderu. Projekt platformy uniwersalnej systemu Windows zawiera te map bitowych:

- Mapy bitowej 48-piksel kwadratowe, przechowywane jako **/Assets/MonkeyFace.scale-100.png**
- Mapy bitowej 96 piksel kwadratowe, przechowywane jako **/Assets/MonkeyFace.scale-200.png**
- Mapy bitowej 192 piksel kwadratowe, przechowywane jako **/Assets/MonkeyFace.scale-400.png**

One wszystkich określono **Akcja kompilacji** z **zawartości**.

Można określić sposób `Text` i `Image` właściwości są ustawione na `Button` przy użyciu [ `ContentLayout` ](xref:Xamarin.Forms.Button.ContentLayout) właściwość `Button`. Ta właściwość jest typu [ `ButtonContentLayout` ](xref:Xamarin.Forms.Button.ButtonContentLayout), które jest klasą osadzony w `Button`. [Konstruktor](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double)) ma dwa argumenty:

- Członek [ `ImagePosition` ](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) wyliczenie: `Left`, `Top`, `Right`, lub `Bottom` wskazującą sposób wyświetlania mapy bitowej względem tekstu.
- A `double` wartość odstępu między mapy bitowej i tekst.

Wartości domyślne są `Left` i 10 jednostek. Dwie właściwości tylko do odczytu z `ButtonContentLayout` o nazwie [ `Position` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) i [ `Spacing` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) Podaj wartości tych właściwości.

W kodzie, można utworzyć `Button` i ustaw `ContentLayout` właściwości w następujący sposób:

```csharp
Button button = new Button
{
    Text = "button text",
    Image = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

W języku XAML należy określić tylko element członkowski wyliczenia lub odstępy lub zarówno w dowolnej kolejności oddzielonych przecinkami:

```xaml
<Button Text="button text"
        Image="image filename"
        ContentLayout="Right, 20" />
```

**Obrazu przycisku Pokaz** strona używa `OnPlatform` do określenia innej nazwy plików dla systemu iOS, Android i platformy uniwersalnej systemu Windows pliki map bitowych. Jeśli chcesz użyć tej samej nazwy pliku dla wszystkich trzech platform i unikaj używania `OnPlatform`, konieczne będzie przechowywać map bitowych platformy uniwersalnej systemu Windows w katalogu głównym projektu.

Pierwszy `Button` na **obrazu przycisku Pokaz** strony zestawy `Image` właściwości, ale nie `Text` właściwości:

```xaml
<Button>
    <Button.Image>
        <OnPlatform x:TypeArguments="FileImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.Image>
</Button>
```

Mapy bitowe platformy uniwersalnej systemu Windows są przechowywane w katalogu głównym projektu, ten kod znaczników mogą znacznie uproszczone:

```xaml
<Button Image="MonkeyFace.png" />
```

Aby uniknąć dużo występują powtarzające znaczników w **ImageButtonDemo.xaml** pliku niejawny `Style` jest także zdefiniowana, aby ustawić `Image` właściwości. To `Style` jest automatycznie stosowana do pięciu innych `Button` elementów. Oto pełny plik XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">
        
        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="Image">
                    <OnPlatform x:TypeArguments="FileImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.Image>
                <OnPlatform x:TypeArguments="FileImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.Image>
        </Button>

        <Button Text="Default" />

        <Button Text="Left - 10"
                ContentLayout="Left, 10" />

        <Button Text="Top - 10"
                ContentLayout="Top, 10" />

        <Button Text="Right - 20"
                ContentLayout="Right, 20" />

        <Button Text="Bottom - 20" 
                ContentLayout="Bottom, 20" />
    </FlexLayout>
</ContentPage>
```

Cztery końcowego `Button` elementy użycie `ContentLayout` właściwości w celu określenia pozycji i odstępy tekstu i mapy bitowej:

[![Obraz przycisku Pokaz](button-images/ImageButtonDemo.png "obrazu przycisku Pokaz")](button-images/ImageButtonDemo-Large.png#lightbox)

Teraz przedstawiono różne sposoby, które może obsłużyć `Button` zdarzenia i zmień `Button` wyglądu.

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe ButtonDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)
- [Przycisk interfejsu API](xref:Xamarin.Forms.Button)
