---
title: Suwak zestawu narzędzi Xamarin.Forms
description: Suwak Xamarin.Forms jest poziomy pasek, które mogą być zmieniane przez użytkownika, które można wybierać wartość typu double ciągłego zakresu. W tym artykule opisano sposób użycia klasy suwak, aby wybrać wartość z zakresu wartości ciągłe.
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/10/2018
ms.openlocfilehash: 2ba4ffa1bcaee5f95fbd963cd48e694569ec7850
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986099"
---
# <a name="xamarinforms-slider"></a>Suwak zestawu narzędzi Xamarin.Forms

_Użyj suwaka służąca do wybierania z zakresu wartości ciągłe._

Xamarin.Forms [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) jest poziomy pasek, które mogą być zmieniane przez użytkownika, aby wybrać `double` wartość z ciągłego zakresu.

`Slider` Definiuje trzy właściwości typu `double`:

- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) to minimum zakresu, z wartością domyślną równą 0.
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) to maksimum zakresu, z wartością domyślną 1.
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) wartość suwaka należą do zakresu od `Minimum` i `Maximum` i ma wartość domyślną równą 0.

Wszystkie trzy właściwości są wspierane przez `BindableProperty` obiektów. `Value` Właściwość ma domyślny tryb powiązania z `BindingMode.TwoWay`, co oznacza, że nadaje się jako źródło powiązania w aplikacji, która używa [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architektury.

> [!WARNING]
> Wewnętrznie `Slider` zapewnia, że `Minimum` jest mniejsza niż `Maximum`. Jeśli `Minimum` lub `Maximum` nigdy nie są ustawione tak, aby `Minimum` jest nie mniejsza niż `Maximum`, zgłaszany jest wyjątek. Zobacz [ **środki ostrożności** ](#precautions) sekcji poniżej, aby uzyskać więcej informacji na temat ustawień dotyczących `Minimum` i `Maximum` właściwości.

`Slider` Przekształca wynik dane `Value` właściwości, tak aby można ją między `Minimum` i `Maximum`włącznie. Jeśli `Minimum` właściwość jest ustawiona na wartość większą niż `Value` właściwości `Slider` ustawia `Value` właściwość `Minimum`. Podobnie jeśli `Maximum` jest ustawiona na wartość mniej niż `Value`, następnie `Slider` ustawia `Value` właściwość `Maximum`.

`Slider` definiuje [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) zdarzenia, które jest wywoływane, gdy `Value` zmiany, za pośrednictwem manipulację użytkownika `Slider` lub gdy program ustawia `Value` właściwość bezpośrednio. A `ValueChanged` zdarzenie również jest wywoływane, gdy `Value` właściwość jest traktowany jak opisano w poprzednim akapicie.

[ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) Obiektu, który towarzyszy `ValueChanged` zdarzenie ma dwie właściwości, oba typu `double`: [ `OldValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.OldValue/) i [ `NewValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.NewValue/). W tym czasie jest wyzwalane zdarzenie, wartość `NewValue` jest taka sama jak `Value` właściwość `Slider` obiektu.

> [!WARNING]
> Nie należy używać opcji nieograniczone układzie poziomym `Center`, `Start`, lub `End` z `Slider`. W systemach Android i platformy uniwersalnej systemu Windows `Slider` zwija na pasku o zerowej długości, a także w systemach iOS, pasek jest bardzo mały. Zachowaj wartość domyślną `HorizontalOptions` ustawienie `Fill`i nie używaj szerokości `Auto` podczas przełączania `Slider` w `Grid` układu.

`Slider` Definiuje również kilka właściwości, które wpływają na jego wygląd:

- [`MinimumTrackColor`](xref:Xamarin.Forms.Slider.MinimumTrackColorProperty) znajduje się pasek koloru po lewej stronie przycisku suwaka.
- [`MaximumTrackColor`](xref:Xamarin.Forms.Slider.MaximumTrackColorProperty) znajduje się pasek koloru po prawej stronie przycisku suwaka.
- [`ThumbColor`](xref:Xamarin.Forms.Slider.ThumbColorProperty) jest to kolor przycisku suwaka. Ta właściwość nie jest obsługiwana na platformie Universal Windows.
- [`ThumbImage`](xref:Xamarin.Forms.Slider.ThumbImageProperty) obraz dla przycisku suwaka typu [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource). Ta właściwość nie jest obsługiwana na platformie Universal Windows.

> [!NOTE]
> `ThumbColor` i `ThumbImage` właściwości wzajemnie się wykluczają. Jeśli obie te właściwości są ustawione, `ThumbImage` właściwości mają wyższy priorytet.

## <a name="basic-slider-code-and-markup"></a>Podstawowy kod suwaka i znaczników

[ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) przykładowe zaczyna się od trzech stron, które są funkcjonalnie identyczny, ale są implementowane w na różne sposoby. Pierwsza strona używa tylko kod C#, drugi za program obsługi zdarzeń w kodzie XAML, a trzeci ma możliwość uniknąć programu obsługi zdarzeń za pomocą powiązania danych w pliku XAML.

### <a name="creating-a-slider-in-code"></a>Tworzenie suwaka w kodzie

**Podstawową kodu suwaka** strony w [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) przykład pokazuje pokaz, aby utworzyć `Slider` oraz dwóch `Label` obiektów w kodzie:

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

`Slider` Jest inicjowany mieć `Maximum` właściwość 360. `ValueChanged` Program obsługi `Slider` używa `Value` właściwość `slider` obiektu, aby ustawić `Rotation` właściwości pierwszego `Label` i używa `String.Format` metody z `NewValue` właściwość argumenty zdarzeń, aby ustawić `Text` właściwość drugiej `Label`. Te dwie metody, aby uzyskać bieżącą wartość `Slider` są wymienne.

Poniżej przedstawiono program działających w systemach iOS, Android i Windows platformy Uniwersalnej urządzeniach:

[![Kod podstawowy suwaka](slider-images/BasicSliderCode.png "kodu podstawowego suwaka")](slider-images/BasicSliderCode-Large.png#lightbox)

Drugi `Label` Wyświetla tekst "(niezainicjowana)" do momentu `Slider` jest przetwarzany, który przypadków pierwszy `ValueChanged` zdarzeń do uruchomienia. Zauważ, że liczba miejsc dziesiętnych, które są wyświetlane różne dla trzech platformach. Te różnice są związane z implementacji platformy `Slider` zostały one omówione w dalszej części tego artykułu, w sekcji [różnice dotyczące Platform implementacji](#implementations).

### <a name="creating-a-slider-in-xaml"></a>Tworzenie suwaka w XAML

**Podstawowe XAML suwaka** strony funkcjonalnie jest taka sama jak **podstawową kodu suwaka** ale zaimplementowany głównie w XAML:

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

Plik związany z kodem zawiera program obsługi dla `ValueChanged` zdarzeń:

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

Istnieje również możliwość obsługi zdarzeń w celu uzyskania `Slider` , jest uruchamiana zdarzeń za pośrednictwem `sender` argumentu. `Value` Właściwość zawiera bieżącą wartość:

```csharp
double value = ((Slider)sender).Value;
```

Jeśli `Slider` obiektu otrzymanych nazwy w pliku XAML z `x:Name` atrybutu (na przykład "suwaka"), a następnie program obsługi zdarzeń może odwoływać się do tego obiektu bezpośrednio:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Suwak powiązania danych

**Podstawowe powiązania suwaka** strona przedstawia sposób zapisania niemal równoważne program, który eliminuje `Value` programu obsługi zdarzeń za pomocą [powiązanie danych](~/xamarin-forms/app-fundamentals/data-binding/index.md):

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

`Rotation` Właściwości pierwszego `Label` jest powiązany z `Value` właściwość `Slider`, ponieważ jest `Text` właściwość drugiej `Label` z `StringFormat` specyfikacji. **Podstawowe powiązania suwaka** strony funkcje nieco inaczej dwóch poprzednich stron: po raz pierwszy zostanie wyświetlona strona, drugi `Label` wyświetla ciąg tekstowy, z wartością. Jest to korzyść wynikająca z użycia powiązanie danych. Do wyświetlania tekstu bez powiązania danych, musisz zainicjować specjalnie `Text` właściwość `Label` lub symulować uruchomieniu którego z `ValueChanged` zdarzeń, wywołując program obsługi zdarzeń z konstruktora klasy.

<a name="precautions" />

## <a name="precautions"></a>Środki ostrożności

Wartość `Minimum` właściwość zawsze musi być mniejsza niż wartość `Maximum` właściwości. Poniższy kod powoduje, że fragment kodu `Slider` Aby zgłosić wyjątek:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

Kompilator języka C# generuje kod, który ustawia te dwie właściwości w kolejności, i kiedy `Minimum` właściwość jest ustawiona na 10, jest większa niż wartość domyślna `Maximum` wartość 1. Można uniknąć wyjątek w tym przypadku ustawiając `Maximum` właściwość pierwszy:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Ustawienie `Maximum` 20 nie stanowi to problemu, ponieważ jest on większy niż domyślna `Minimum` ustawienie 0. Gdy `Minimum` jest ustawiona, wartość jest mniejsza niż `Maximum` o wartości 20.

Ten sam problem występuje w XAML. Ustawianie właściwości w kolejności, który zapewnia, że `Maximum` jest zawsze większa niż `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Możesz ustawić `Minimum` i `Maximum` wartości ujemne, ale tylko w kolejności gdzie `Minimum` jest zawsze mniejsza niż `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value` Właściwość zawsze jest większa niż lub równa `Minimum` wartości i mniejsza niż lub równa `Maximum`. Jeśli `Value` jest ustawiona na wartość spoza zakresu, wartość będzie go to tego zmusić należeć do zakresu, ale nie jest wyjątek. Na przykład, ten kod pozwoli *nie* zgłosić wyjątek:

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Zamiast tego `Value` właściwość jest traktowany jak `Maximum` wartość 1.

Poniżej przedstawiono fragment kodu powyżej:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Gdy `Minimum` jest ustawiony na 10, następnie `Value` jest również ustawiona na 10.

Jeśli `ValueChanged` programu obsługi zdarzeń została dołączona w czasie, `Value` właściwość jest traktowany jak coś innego niż jego wartość domyślna 0, a następnie `ValueChanged` jest wyzwalane zdarzenie. Poniżej przedstawiono fragment XAML:

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Gdy `Minimum` jest ustawiony na 10, `Value` jest również ustawiona na 10, a `ValueChanged` jest wyzwalane zdarzenie. Taka sytuacja może wystąpić, zanim został skonstruowany pozostałej części strony, a program obsługi może próbować odwoływać się do innych elementów na stronie, które nie zostały jeszcze utworzone. Możesz chcieć dodać kod do `ValueChanged` program obsługi, który sprawdza, czy `null` wartości innych elementów na stronie. Lub możesz ustawić `ValueChanged` programu obsługi zdarzeń po `Slider` wartości zostały zainicjowane.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>Różnice dotyczące platform implementacji

Zrzuty ekranu, przedstawionej wcześniej, wyświetlanie wartości `Slider` wprowadzając szereg różnych separatorów dziesiętnych. Dotyczy to jak `Slider` jest zaimplementowana na platformach Android i platformy uniwersalnej systemu Windows.

### <a name="the-android-implementation"></a>Implementacja systemu Android

Implementacja systemu Android `Slider` opiera się na Android [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) i zawsze ustawia [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) właściwości ustawiono wartość 1000. Oznacza to, że `Slider` w systemie Android zawiera tylko 1,001 wartości dyskretnych. Jeśli ustawisz `Slider` mieć `Minimum` 0 i `Maximum` 5000, a następnie jako `Slider` jest przetwarzany `Value` właściwość ma wartości 0, 5, 10, 15 i tak dalej.

### <a name="the-uwp-implementation"></a>Implementacja platformy uniwersalnej systemu Windows

Implementacja platformy uniwersalnej systemu Windows `Slider` zależy od platformy UWP [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) kontroli. `StepFrequency` Właściwości platformy UWP `Slider` jest ustawiony na różnicę tej `Maximum` i `Minimum` właściwości podzielona przez 10, ale nie jest większa niż 1.

Na przykład domyślny zakres od 0 do 1 `StepFrequency` właściwość ma wartość 0,1. Jako `Slider` jest przetwarzany `Value` właściwość jest ograniczona do 0, 0.1, 0.2, 0,3, 0,4, 0,5, Update 0.6, 0,7, 0,8, 0,9 i 1.0. (Jest to widoczne na ostatniej stronie w [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) próbki.) Gdy różnica między `Maximum` i `Minimum` właściwości jest 10 lub nowszy, `StepFrequency` jest ustawiona na 1 i `Value` właściwość posiada wartości całkowitych.

Ponadto [ `ThumbColor` ](xref:Xamarin.Forms.Slider.ThumbColorProperty) i [ `ThumbImage` ](xref:Xamarin.Forms.Slider.ThumbImageProperty) właściwości nie są obsługiwane na platformy uniwersalnej systemu Windows.

### <a name="the-stepslider-solution"></a>Rozwiązanie StepSlider

Bardziej wszechstronna `StepSlider` została omówiona w [działu 27. Niestandardowe programy renderujące](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) książki *tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms*. `StepSlider` Przypomina `Slider` , ale dodaje `Steps` właściwości, aby określić liczbę wartości z zakresu od `Minimum` i `Maximum`.

## <a name="sliders-for-color-selection"></a>Suwaki wybór koloru

Końcowe dwie strony w programie [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) przykładowe obaj użytkownicy za pomocą trzech `Slider` wystąpień do wyboru kolorów. Pierwsza strona obsługuje wszystkie interakcje w pliku związanym z kodem, podczas gdy druga strona przedstawia sposób użycia powiązanie danych z ViewModel.

### <a name="handling-sliders-in-the-code-behind-file"></a>Obsługa suwaki w pliku związanym z kodem

**Suwaki koloru RGB** tworzy stronę `BoxView` do wyświetlania kolorów, trzy `Slider` wystąpień, wybór składników czerwonego, zielonego i niebieskiego koloru i trzech `Label` elementów do wyświetlania tych kolorów wartości:

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

A `Style` oferuje wszystkie trzy `Slider` elementów w zakresie od 0 do 255. `Slider` Elementy współużytkować ten sam `ValueChanged` obsługi, który jest implementowany w pliku związanym z kodem:

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

Pierwszy zestawy sekcji `Text` właściwości jednego z `Label` wystąpień krótki ciąg tekstowy określająca wartość `Slider` w formacie szesnastkowym. Następnie wszystkie trzy `Slider` wystąpieniach używanych do tworzenia `Color` wartość ze składników RGB:

[![Suwaki koloru RGB](slider-images/RgbColorSliders.png "suwaki kolorów RGB")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Powiązywanie suwak ViewModel

**HSL suwaków** strona przedstawia sposób użycia ViewModel do wykonywania obliczeń użyte do utworzenia `Color` wartości z wartości hue, nasycenia i jasności. Wszystkie modele widoków, takich jak `HSLColorViewModel` klasy implementuje `INotifyPropertyChanged` interfejsu i generowane `PropertyChanged` zdarzenie zawsze wtedy, gdy zmieni się jedna z właściwości:

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

Modele widoków i `INotifyPropertyChanged` interfejsu zostały omówione w artykule [powiązanie danych](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**HslColorSlidersPage.xaml** plik `HslColorViewModel` i ustawia ją na stronę `BindingContext` właściwości. Dzięki temu wszystkie elementy w pliku XAML, aby powiązać z właściwościami w ViewModel:

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

Jako `Slider` elementy są zmieniane, `BoxView` i `Label` elementy zostaną zaktualizowane na podstawie ViewModel:

[![HSL suwaków](slider-images/HslColorSliders.png "suwaków HSL")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat` Składnika `Binding` — rozszerzenie znaczników jest ustawiona dla formatu "F2" do wyświetlania dwóch miejsc po przecinku. (Ciąg formatowania w powiązań danych jest omówione w artykule [formatowanie ciągów](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) Jednak program w wersji platformy uniwersalnej systemu Windows jest ograniczona do wartości 0, 0.1, 0.2... 0,9 i 1.0. Jest to bezpośrednio w wyniku wykonania platformy UWP `Slider` zgodnie z powyższym opisem w sekcji [różnice dotyczące Platform implementacji](#implementations).

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe pokazy suwaka](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [Suwak interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)
