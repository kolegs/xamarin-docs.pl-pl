---
title: Suwak platformy Xamarin.Forms
description: Suwak platformy Xamarin.Forms jest poziomy pasek, może operować przez użytkownika, aby wybrać wartość typu double z ciągły zakres. W tym artykule opisano sposób użycia klasy suwaka można wybrać wartość z zakresu wartości ciągłe.
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/16/2018
ms.openlocfilehash: 33c26abe2de017b6d8070053baf917cdd7a0dfc6
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245810"
---
# <a name="xamarinforms-slider"></a>Suwak platformy Xamarin.Forms

_Użyj suwaka do zaznaczania na podstawie zakresu wartości ciągłe._

Platformy Xamarin.Forms [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) jest poziomy pasek, który może manipulować użytkownikowi na wybranie `double` wartość z zakresu ciągłe.

`Slider` Definiuje trzy właściwości typu `double`:

- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) to minimum zakresu, wartość domyślna 0.
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) jest maksymalną zakresu, wartości domyślnej 1.
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) wartość suwaka można dostosować w zakresie między `Minimum` i `Maximum` i ma wartość domyślną równą 0.

Wszystkie trzy właściwości bazują na `BindableProperty` obiektów. `Value` Właściwość ma domyślny tryb powiązania z `BindingMode.TwoWay`, która oznacza, że jest odpowiedni jako źródło powiązania w aplikacji, która używa [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architektury.

> [!WARNING]
> Wewnętrznie `Slider` upewnia się, że `Minimum` jest mniejsza niż `Maximum`. Jeśli `Minimum` lub `Maximum` kiedykolwiek są ustawione, aby `Minimum` jest nie mniejszy niż `Maximum`, zgłoszony wyjątek. Zobacz [ **środki ostrożności** ](#precautions) sekcji poniżej, aby uzyskać więcej informacji o ustawieniu `Minimum` i `Maximum` właściwości.

`Slider` Przekształca wynik dane `Value` właściwości, tak aby była między `Minimum` i `Maximum`włącznie. Jeśli `Minimum` właściwość jest ustawiona na wartość większa niż `Value` właściwość `Slider` ustawia `Value` właściwości `Minimum`. Podobnie jeśli `Maximum` jest ustawiona na wartość mniej niż `Value`, następnie `Slider` ustawia `Value` właściwości `Maximum`.

`Slider` definiuje [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) zdarzenie, które jest wywoływane, gdy `Value` zmian, albo przez użytkownika manipulowanie `Slider` lub gdy program ustawia `Value` właściwości bezpośrednio. A `ValueChanged` zdarzenie jest również uruchamiane podczas `Value` właściwości jest traktowany jak opisano w poprzednim akapicie.

[ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) Obiektu, który towarzyszy `ValueChanged` zdarzenie ma dwie właściwości, zarówno typu `double`: [ `OldValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.OldValue/) i [ `NewValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.NewValue/). W tym czasie jest generowane zdarzenie, wartość `NewValue` jest taka sama jak `Value` właściwość `Slider` obiektu.

> [!WARNING]
> Nie należy używać opcji nieograniczonego układ poziomy `Center`, `Start`, lub `End` z `Slider`. Zarówno dla systemu Android, jak i platformy uniwersalnej systemu Windows `Slider` zwija pasek o zerowej długości, a w systemie iOS, pasek jest bardzo krótki. Zachowaj ustawienie domyślne `HorizontalOptions` ustawienie `Fill`i nie używaj szerokości `Auto` podczas umieszczania `Slider` w `Grid` układu.

## <a name="basic-slider-code-and-markup"></a>Podstawowy kod suwaka i znaczników

[ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) próbki rozpoczyna się od trzech stron, które są identyczne funkcjonalnie, ale są wykonywane na różne sposoby. Pierwsza strona używa tylko kodu C#, drugi używa XAML z obsługi zdarzeń w kodzie, a trzeci jest w stanie uniknąć programu obsługi zdarzeń za pomocą powiązania danych w pliku XAML.

### <a name="creating-a-slider-in-code"></a>Tworzenie suwaka w kodzie

**Podstawowy kod suwaka** strony [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) pokazano Pokaż, aby utworzyć `Slider` i dwa `Label` obiektów w kodzie:

```csharp
public class BasicSliderCodePage : ContentPage
{
    public BasicSliderCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Slider slider = new Slider
        {
            Maximum = 360
        };
        slider.ValueChanged += (sender, args) =>
        {
            rotationLabel.Rotation = slider.Value;
            displayLabel.Text = String.Format("The Slider value is {0}", args.NewValue);
        };

        Title = "Basic Slider Code";
        Padding = new Thickness(10, 0);
        Content = new StackLayout
        {
            Children =
            {
                rotationLabel,
                slider,
                displayLabel
            }
        };
    }
}
```

`Slider` Zainicjowano mają `Maximum` właściwości 360. `ValueChanged` Obsługi `Slider` używa `Value` właściwość `slider` obiekt, aby ustawić `Rotation` właściwości pierwszego `Label` i używa `String.Format` metody z `NewValue` właściwości argumenty zdarzeń, aby ustawić `Text` właściwości drugiego `Label`. Te dwie metody, aby uzyskać bieżącą wartość `Slider` są wymienne.

Oto programu uruchomionego w systemach iOS, Android i Windows platformy Uniwersalnej urządzeń:

[![Suwak podstawowy kod](slider-images/BasicSliderCode.png "kod podstawowe suwaka")](slider-images/BasicSliderCode-Large.png#lightbox)

Drugi `Label` Wyświetla tekst "(niezainicjowane)" do `Slider` zmieniany, który przypadków pierwszy `ValueChanged` zdarzeń do uruchomienia. Zauważ, że liczba miejsc dziesiętnych, które są wyświetlane jest różne dla trzech platform. Te różnice są związane z implementacji platformy `Slider` i zostały omówione w dalszej części tego artykułu w sekcji [platformy różnice implementacji](#implementations).

### <a name="creating-a-slider-in-xaml"></a>Tworzenie suwaka w języku XAML

**Podstawowe XAML suwaka** strony funkcjonalnie jest taka sama jak **podstawowy kod suwaka** ale zaimplementowanym głównie w języku XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderXamlPage"
             Title="Basic Slider XAML"
             Padding="10, 0">
    <StackLayout>
        <Label x:Name="rotatingLabel"
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider Maximum="360"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Plik CodeBehind zawiera programu obsługi `ValueChanged` zdarzeń:

```csharp
public partial class BasicSliderXamlPage : ContentPage
{
    public BasicSliderXamlPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double value = args.NewValue;
        rotatingLabel.Rotation = value;
        displayLabel.Text = String.Format("The Slider value is {0}", value);
    }
}
```

Istnieje również możliwość obsługi zdarzeń w celu uzyskania `Slider` który jest wyzwoleniem zdarzenia za pośrednictwem `sender` argumentu. `Value` Właściwość zawiera bieżąca wartość:

```csharp
double value = ((Slider)sender).Value;
```

Jeśli `Slider` obiektu określono nazwę w pliku XAML z `x:Name` atrybutu (na przykład "suwaka"), a następnie program obsługi zdarzeń można odwołać obiektu bezpośrednio:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Suwak powiązanie danych

**Podstawowe powiązania suwaka** strona przedstawia sposób zapisania niemal równoważne program, który eliminuje `Value` obsługi zdarzeń za pomocą [powiązania danych](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderBindingsPage"
             Title="Basic Slider Bindings"
             Padding="10, 0">
    <StackLayout>
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference slider},
                                  Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360" />

        <Label x:Name="displayLabel"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The Slider value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Rotation` Właściwości pierwszego `Label` jest powiązany z `Value` właściwość `Slider`, ponieważ jest `Text` właściwości drugiego `Label` z `StringFormat` specyfikacji. **Podstawowe powiązania suwaka** strony funkcje nieco inaczej z dwóch poprzednich stron: po wyświetleniu strony, drugi `Label` wyświetla ciąg tekstowy o wartości. Jest to korzyść wynikająca używanie powiązania danych. Do wyświetlania tekstu bez powiązania danych, należy zainicjować specjalnie `Text` właściwość `Label` lub symulować uruchamiania programu `ValueChanged` zdarzenia przez wywołanie metody obsługi zdarzeń z konstruktora klasy.

<a name="precautions" />

## <a name="precautions"></a>Środki ostrożności

Wartość `Minimum` właściwość zawsze musi być mniejsza niż wartość `Maximum` właściwości. Poniższy kod przyczyny fragment `Slider` Aby zgłosić wyjątek:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

Kompilator języka C# generuje kod, który ustawia te dwie właściwości w sekwencji, i kiedy `Minimum` właściwości jest równa 10, jest większy niż domyślne `Maximum` wartość 1. Można uniknąć wyjątek w takim przypadku przez ustawienie `Maximum` właściwości pierwszy:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Ustawienie `Maximum` 20 nie stanowi to problemu, ponieważ jest większy niż domyślne `Minimum` ustawienie 0. Gdy `Minimum` jest ustawiona wartość jest mniejsza niż `Maximum` o wartości 20.

Ten sam problem występuje w języku XAML. Ustawianie właściwości w kolejności, która zapewnia, że `Maximum` zawsze jest większa niż `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Można ustawić `Minimum` i `Maximum` wartości ujemne, ale tylko w określonym porządku gdzie `Minimum` jest zawsze mniej niż `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value` Właściwość zawsze jest większa niż lub równa `Minimum` wartość i mniejsza niż lub równa `Maximum`. Jeśli `Value` jest ustawiony na wartość spoza zakresu, będzie można przekształcić wartość należeć do zakresu, ale nie wystąpił wyjątek. Na przykład, ten kod będzie *nie* zgłaszał wyjątku:

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Zamiast tego `Value` właściwości jest traktowany jak `Maximum` wartość 1.

Oto fragment kodu pokazano powyżej:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Gdy `Minimum` jest ustawiony na 10, następnie `Value` również jest ustawiony na 10.

Jeśli `ValueChanged` dołączono program obsługi zdarzeń w czasie który `Value` właściwości jest traktowany jak coś innego niż jego wartość domyślna 0, a następnie `ValueChanged` zdarzenie jest wywoływane. Poniżej przedstawiono fragment XAML:

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Gdy `Minimum` jest ustawiony na 10, `Value` również jest ustawiony na 10, a `ValueChanged` zdarzenie jest wywoływane. Taka sytuacja może wystąpić, zanim skonstruowane pozostałej części strony, a program obsługi może podejmować wielokrotne próby odwoływać się inne elementy na stronie, które nie zostały jeszcze utworzone. Możesz dodać kod do `ValueChanged` obsługi, która sprawdza, czy `null` wartości innych elementów na stronie. Lub możesz ustawić `ValueChanged` obsługi zdarzeń po `Slider` wartości zostały zainicjowane.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>Różnice implementacji platformy

Zrzuty ekranu przedstawiona wcześniej wyświetlić wartość `Slider` z różną liczbę miejsc dziesiętnych. Dotyczy to jak `Slider` jest zaimplementowany w platformach systemów Android i platformy uniwersalnej systemu Windows.

### <a name="the-android-implementation"></a>Implementacja systemu Android

Implementacja systemu Android `Slider` opiera się na Android [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) i zawsze ustawia [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) właściwości do 1000. Oznacza to, że `Slider` w systemie Android ma tylko 1,001 wartości dyskretnych. Jeśli ustawisz `Slider` mają `Minimum` 0 i `Maximum` 5000, a następnie jako `Slider` zmieniany, `Value` właściwość ma wartości 0, 5, 10, 15 i tak dalej.

### <a name="the-uwp-implementation"></a>Implementacja platformy uniwersalnej systemu Windows

Implementacja platformy uniwersalnej systemu Windows `Slider` opiera się na platformy uniwersalnej systemu Windows [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) formantu. `StepFrequency` Właściwości platformy uniwersalnej systemu Windows `Slider` ma ustawioną wartość różnicy `Maximum` i `Minimum` właściwości podzielona przez 10, ale nie większą niż 1.

Na przykład dla domyślnego zakresu od 0 do 1 `StepFrequency` właściwość ma wartość 0,1. Jako `Slider` zmieniany, `Value` właściwość jest ograniczona do 0, 0,1, 0,2, 0,3, 0,4, 0,5, 0,6, 0,7, 0,8, 0,9 i 1.0. (Jest to widoczne na ostatniej stronie w [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) próbki.) Gdy różnica między `Maximum` i `Minimum` właściwości jest następnie 10 lub nowszy, `StepFrequency` jest ustawiona na 1 i `Value` właściwość ma wartości całkowitej.

### <a name="the-stepslider-solution"></a>Rozwiązanie StepSlider

Bardziej elastyczne `StepSlider` została szczegółowo opisana w [działu 27. Niestandardowe moduły renderowania](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) książki *tworzenia aplikacji mobilnych za pomocą platformy Xamarin.Forms*. `StepSlider` Jest podobny do `Slider` , ale dodaje `Steps` właściwości, aby określić liczbę wartości między `Minimum` i `Maximum`.

## <a name="sliders-for-color-selection"></a>Suwaki wybór kolorów

Ostatni dwie strony w [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) przykładowa jednocześnie używać trzech `Slider` wystąpień wybór kolorów. Pierwsza strona obsługuje wszystkie interakcje w pliku CodeBehind podczas drugiej stronie przedstawia sposób użycia powiązanie danych z ViewModel.

### <a name="handling-sliders-in-the-code-behind-file"></a>Obsługa suwaki w pliku związanym z kodem

**Suwaki kolorów RGB** tworzy stronę `BoxView` do wyświetlenia kolor, trzy `Slider` instancje, aby wybrać składniki czerwony, zielonemu i niebieskiemu koloru i trzech `Label` elementy do wyświetlania tych kolorów wartości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.RgbColorSlidersPage"
             Title="RGB Color Sliders">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="Maximum" Value="255" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView x:Name="boxView"
                 Color="Black"
                 VerticalOptions="FillAndExpand" />

        <Slider x:Name="redSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="redLabel" />

        <Slider x:Name="greenSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="greenLabel" />

        <Slider x:Name="blueSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="blueLabel" />
    </StackLayout>
</ContentPage>
```

A `Style` zapewnia wszystkie trzy `Slider` elementy w zakresie od 0 do 255. `Slider` Elementy mają takie same wspólne `ValueChanged` obsługi, która jest zaimplementowana w pliku związanym z kodem:

```csharp
public partial class RgbColorSlidersPage : ContentPage
{
    public RgbColorSlidersPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (sender == redSlider)
        {
            redLabel.Text = String.Format("Red = {0:X2}", (int)args.NewValue);
        }
        else if (sender == greenSlider)
        {
            greenLabel.Text = String.Format("Green = {0:X2}", (int)args.NewValue);
        }
        else if (sender == blueSlider)
        {
            blueLabel.Text = String.Format("Blue = {0:X2}", (int)args.NewValue);
        }

        boxView.Color = Color.FromRgb((int)redSlider.Value,
                                      (int)greenSlider.Value,
                                      (int)blueSlider.Value);
    }
}
```

Pierwszy zestawy sekcji `Text` właściwości jednego z `Label` wystąpień krótki ciąg tekstowy wartości `Slider` w formacie szesnastkowym. Następnie wszystkie trzy `Slider` wystąpienia są dostępne do utworzenia `Color` wartości RGB składników:

[![Suwaki kolorów RGB](slider-images/RgbColorSliders.png "suwaki kolorów RGB")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Powiązanie suwak ViewModel

**HSL suwaków** strona przedstawia sposób użycia ViewModel do obliczeń użyty do utworzenia `Color` wartość z wartości odcień, nasycenie i jasność. Wszystkie ViewModels, takich jak `HSLColorViewModel` klasa implementuje `INotifyPropertyChanged` interfejsu i uruchamiany `PropertyChanged` zdarzeń przy każdej zmianie jedna z właściwości:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;

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
            }
        }
        get
        {
            return color;
        }
    }
}
```

ViewModels i `INotifyPropertyChanged` interfejsu omówiono w artykule [powiązania danych](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**HslColorSlidersPage.xaml** tworzy plik `HslColorViewModel` i ustawia ją na stronie `BindingContext` właściwości. Dzięki temu wszystkie elementy w pliku XAML w celu powiązania właściwości w ViewModel:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SliderDemos"
             x:Class="SliderDemos.HslColorSlidersPage"
             Title="HSL Color Sliders">

    <ContentPage.BindingContext>
        <local:HslColorViewModel Color="Chocolate" />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</ContentPage>
```

Jako `Slider` elementy są manipulacje `BoxView` i `Label` elementy są aktualizowane z ViewModel:

[![HSL suwaków](slider-images/HslColorSliders.png "suwaków HSL")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat` Składnika `Binding` — rozszerzenie znaczników ustawiono dla formatu "F2" wyświetlanie dwóch miejsc po przecinku. (Ciąg formatowania powiązania danych została szczegółowo opisana w artykule [ciągu formatowania](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) Jednak wersja platformy uniwersalnej systemu Windows program jest ograniczony do wartości 0, 0,1, 0,2... 0,9 i 1.0. To jest bezpośrednio w wyniku wykonania platformy uniwersalnej systemu Windows `Slider` zgodnie z powyższym opisem w sekcji [platformy różnice implementacji](#implementations).

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe pokazy suwaka](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [Suwak interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)
