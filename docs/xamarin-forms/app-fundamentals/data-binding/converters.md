---
title: "Konwertery wartości powiązania"
description: "Rzutowania lub konwersji wartości w ramach powiązanie danych"
ms.topic: article
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: aaa4c93eda9edb0eb5d568b3470c02352bdb7467
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="binding-value-converters"></a>Konwertery wartości powiązania

Powiązania danych zazwyczaj przesyłanie danych z właściwości source właściwość target, a w niektórych przypadkach z właściwością target właściwości source. To przeniesienie jest proste, gdy właściwości źródłowa i docelowa są tego samego typu lub gdy jeden typ można przekonwertować na typ za pośrednictwem niejawnej konwersji. Gdy nie jest wielkość liter, musi nastąpić konwersji typu.

W [ **ciągu formatowania** ](string-formatting.md) artykułu, widać, jak używasz `StringFormat` właściwości powiązania danych, aby przekonwertować dowolnego typu ciąg. Dla innych typów konwersji, należy napisać niektórych wyspecjalizowany kod w klasie, który implementuje [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) interfejsu. (Platformy uniwersalnej systemu Windows zawiera podobne klasę o nazwie [ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) w `Windows.UI.Xaml.Data` przestrzeni nazw, ale to `IValueConverter` w `Xamarin.Forms` przestrzeni nazw.) Klasy, które implementują `IValueConverter` są nazywane *konwertery wartości*, ale są one również często określone jako *powiązanie konwertery* lub *powiązanie konwertery wartości*.

## <a name="the-ivalueconverter-interface"></a>Interfejsu IValueConverter

Załóżmy, że chcesz zdefiniować powiązania danych, w którym właściwość źródłowa jest typu `int` , ale właściwość docelowa jest `bool`. Ma to powiązanie danych do tworzenia `false` wartość, gdy źródłem liczba całkowita jest równa 0, i `true` inaczej.  

Można to zrobić z klasy, która implementuje `IValueConverter` interfejsu:

```csharp
public class IntToBoolConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value != 0;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? 1 : 0;
    }
}
```

Ustawienia wystąpienia tej klasy do [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Converter/) właściwość `Binding` klasy lub [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Converter/) właściwość `Binding` — rozszerzenie znaczników. Ta klasa staje się częścią wiązania danych.

`Convert` Metoda jest wywoływana, gdy dane są przenoszone od źródła do obiektu docelowego w `OneWay` lub `TwoWay` powiązania. `value` Parametr jest obiektu lub wartości ze źródła danych powiązania. Metoda musi zwracać wartość typu elementu docelowego powiązania danych. Metoda pokazane rzutowania `value` parametr `int` i porównuje ją z 0 dla `bool` zwracają wartość.

`ConvertBack` Metoda jest wywoływana, gdy dane są przenoszone z miejsca docelowego do źródła w `TwoWay` lub `OneWayToSource` powiązania. `ConvertBack` wykonuje odwrotną konwersję: przyjmuje, że parametr `value` jest równy `bool` od celu i konwertuje go na `int` wartość zwracaną dla źródła.

Jeśli powiązanie danych obejmuje również `StringFormat` ustawienie konwerter wartości jest wywoływana przed wynik jest sformatowany jako ciąg.

**Włączyć przyciski** strony [ **pokazy powiązania danych** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) przykładzie pokazano sposób użycia tego konwertera wartości w powiązaniu danych. `IntToBoolConverter` Zostanie uruchomiony w słowniku zasobów strony. Następnie odwołuje się on z `StaticResource` — rozszerzenie znaczników można ustawić `Converter` właściwości powiązania danych dwa. Często zdarza się, aby udostępnić konwertery dane między wiele powiązań danych na stronie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.EnableButtonsPage"
             Title="Enable Buttons">
    <ContentPage.Resources>
        <ResourceDictionary>
            <local:IntToBoolConverter x:Key="intToBool" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <Entry x:Name="entry1"
               Text=""
               Placeholder="enter search term"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Search"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry1},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />

        <Entry x:Name="entry2"
               Text=""
               Placeholder="enter destination"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Submit"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry2},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />
    </StackLayout>
</ContentPage>
```

Jeśli konwerter wartości jest używana na wielu stronach w aplikacji, można utworzyć wystąpienie go w słowniku zasobów w **App.xaml** pliku.

**Włączyć przyciski** strony przedstawiono popularne potrzebne podczas `Button` wykonuje operację na podstawie tekstu, który użytkownik wpisze w `Entry` widoku. Jeśli nic nie zostało wpisane w `Entry`, `Button` powinny być wyłączone. Każdy `Button` zawiera wiązania z danymi na jego `IsEnabled` właściwości. Źródło danych — wiązanie jest `Length` właściwość `Text` właściwości odpowiadającego `Entry`. Jeśli `Length` właściwość nie jest zwraca konwerter wartości 0, `true` i `Button` jest włączona:

[![Włącz przyciski](converters-images/enablebuttons-small.png "Włącz przyciski")](converters-images/enablebuttons-large.png "Włącz przyciski")

Zwróć uwagę, że `Text` właściwości w każdym `Entry` jest inicjowana na pusty ciąg. `Text` Jest właściwość `null` domyślnie i danych powiązania nie będzie działać w takiej sytuacji.

Niektóre konwertery wartości napisanych specjalnie dla aplikacji, podczas gdy inne zostały uogólnione. Jeśli wiesz, że konwerter wartości będzie można używać tylko w `OneWay` powiązań, a następnie `ConvertBack` można po prostu zwracany przez metodę `null`.

`Convert` Metod przedstawionych powyżej niejawnie, przy założeniu, że `value` argument jest typu `int` i zwracana wartość musi być typu `bool`. Podobnie `ConvertBack` metody, przy założeniu, że `value` argument jest typu `bool` i jest zwracana wartość `int`. Jeśli nie jest wielkość liter, wystąpi wyjątek czasu wykonywania.

Konwertery wartości więcej uogólnione i zaakceptować kilku różnych typów danych może zapisywać. `Convert` i `ConvertBack` można użyć metody `as` lub `is` operatory o `value` parametr, lub można wywołać `GetType` tego parametru, aby ustalić jego typ, a następnie wykonaj coś odpowiednie. Oczekiwany typ zwracanej wartości w każdej z metod jest określany przez `targetType` parametru. Czasami konwertery wartości są używane z powiązaniami danych typów inny element docelowy; Konwerter wartości można użyć `targetType` argumentu, aby dokonać konwersji do poprawnego typu.

Jeśli konwersja wykonywana jest różne dla różnych kultur, użyj `culture` parametru w tym celu. `parameter` Argument `Convert` i `ConvertBack` omówione w dalszej części tego artykułu.

## <a name="binding-converter-properties"></a>Powiązanie właściwości konwertera

Klasy konwertera wartości może mieć właściwości i parametrów ogólnych. Ten konwerter konkretną wartość konwertuje `bool` ze źródła do obiektu typu `T` dla obiekt docelowy:

```csharp
public class BoolToObjectConverter<T> : IValueConverter
{
    public T TrueObject { set; get; }

    public T FalseObject { set; get; }

    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? TrueObject : FalseObject;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return ((T)value).Equals(TrueObject);
    }
}
```

**Wskaźniki przełącznika** strony pokazano, jak może służyć do wyświetlania wartości `Switch` widoku. Wspólne dla wystąpienia konwertery wartości jako zasoby w słowniku zasobów, ale ta strona przedstawia zamiast: każdego konwerter wartości zostanie uruchomiony między `Binding.Converter` znaczniki elementu właściwości. `x:TypeArguments` Wskazuje argumentów ogólnych i `TrueObject` i `FalseObject` są ustawione na obiekty tego typu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SwitchIndicatorsPage"
             Title="Switch Indicators">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>

            <Style TargetType="Switch">
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Subscribe?" />
            <Switch x:Name="switch1" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch1}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Of course!"
                                                         FalseObject="No way!" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Allow popups?" />
            <Switch x:Name="switch2" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Yes"
                                                         FalseObject="No" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
                <Label.TextColor>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Color"
                                                         TrueObject="Green"
                                                         FalseObject="Red" />
                        </Binding.Converter>
                    </Binding>
                </Label.TextColor>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Learn more?" />
            <Switch x:Name="switch3" />
            <Label FontSize="18"
                   VerticalOptions="Center">
                <Label.Style>
                    <Binding Source="{x:Reference switch3}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Style">
                                <local:BoolToObjectConverter.TrueObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Indubitably!" />
                                        <Setter Property="FontAttributes" Value="Italic, Bold" />
                                        <Setter Property="TextColor" Value="Green" />
                                    </Style>                                    
                                </local:BoolToObjectConverter.TrueObject>

                                <local:BoolToObjectConverter.FalseObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Maybe later" />
                                        <Setter Property="FontAttributes" Value="None" />
                                        <Setter Property="TextColor" Value="Red" />
                                    </Style>
                                </local:BoolToObjectConverter.FalseObject>
                            </local:BoolToObjectConverter>
                        </Binding.Converter>
                    </Binding>
                </Label.Style>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

W ciągu ostatnich trzech `Switch` i `Label` pary argumentów ogólnych ustawiono `Style`i całego `Style` obiekty są dostępne dla wartości `TrueObject` i `FalseObject`. Te zastąpienia niejawne styl `Label` ustawiona w słowniku zasobów, więc właściwości w stylu są jawnie przypisane do `Label`. Przełączanie `Switch` powoduje, że odpowiednie `Label` aby odzwierciedlić zmianę:

[![Wskaźniki przełączania](converters-images/switchindicators-small.png "Wskaźniki przełącznika")](converters-images/switchindicators-large.png "Wskaźniki przełączania")


Istnieje również możliwość użycia [ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md) implementować podobne zmiany w interfejsie użytkownika oparte na innych widoków.

## <a name="binding-converter-parameters"></a>Wiązania parametrów konwertera

`Binding` Klasa definiuje [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.ConverterParameter/) właściwości oraz `Binding` definiuje również — rozszerzenie znaczników [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.ConverterParameter/) właściwości. Jeśli ta właściwość jest ustawiona, wartość jest przekazywana do `Convert` i `ConvertBack` metod jako `parameter` argumentu. Nawet jeśli wystąpienia konwerter wartości jest współużytkowane przez kilka powiązania danych, `ConverterParameter` mogą być różne do wykonywania konwersji nieco inny.

Korzystanie z `ConverterParameter` przedstawionej w programie, wybór kolorów. W takim przypadku `RgbColorViewModel` ma trzy właściwości typu `double` o nazwie `Red`, `Green`, i `Blue` używaną do utworzenia `Color` wartość:

```csharp
public class RgbColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Red
    {
        set
        {
            if (color.R != value)
            {
                Color = new Color(value, color.G, color.B);
            }
        }
        get
        {
            return color.R;
        }
    }

    public double Green
    {
        set
        {
            if (color.G != value)
            {
                Color = new Color(color.R, value, color.B);
            }
        }
        get
        {
            return color.G;
        }
    }

    public double Blue
    {
        set
        {
            if (color.B != value)
            {
                Color = new Color(color.R, color.G, value);
            }
        }
        get
        {
            return color.B;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Red"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Green"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Blue"));
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

`Red`, `Green`, I `Blue` właściwości zakresu od 0 do 1. Jednak preferowane, że składniki mają być wyświetlane jako wartości szesnastkowe dwucyfrowym.

Aby wyświetlić je jako wartości szesnastkowe w języku XAML, ich musi być pomnożona przez 255, przekonwertować na liczbę całkowitą, a następnie sformatowane specyfikację "X2" w `StringFormat` właściwości. Pierwsze dwa zadania (mnożenia wartości przez 255 i konwertowanie na liczbę całkowitą) są obsługiwane przez konwerter wartości. Aby konwerter wartości, ponieważ uogólniony jak to możliwe, można określić mnożnik z `ConverterParameter` właściwości, co oznacza, że wprowadza `Convert` i `ConvertBack` metod jako `parameter` argumentu:

```csharp
public class DoubleToIntConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)Math.Round((double)value * GetParameter(parameter));
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value / GetParameter(parameter);
    }

    double GetParameter(object parameter)
    {
        if (parameter is double)
            return (double)parameter;

        else if (parameter is int)
            return (int)parameter;

        else if (parameter is string)
            return double.Parse((string)parameter);

        return 1;
    }
}
```

`Convert` Konwertuje `double` do `int` podczas pomnożenie przez `parameter` wartości; `ConvertBack` dzieli liczb całkowitych `value` argument przez `parameter` i zwraca `double` wynik. (W programie, pokazano poniżej, konwerter wartości jest używany tylko w połączeniu z ciągu formatowania, więc `ConvertBack` nie jest używany.)

Typ `parameter` argument prawdopodobnie może się różnić w zależności od tego, czy powiązanie danych jest zdefiniowana w kodzie lub XAML. Jeśli `ConverterParameter` właściwość `Binding` jest ustawiona w kodzie, prawdopodobnie będzie mieć ustawioną wartość liczbowa:

```csharp
binding.ConverterParameter = 255;
```

`ConverterParameter` Właściwość jest typu `Object`, dlatego kompilator języka C# interpretuje literału 255 jako liczba całkowita i ustawia właściwość z tą wartością.

W języku XAML, jednak `ConverterParameter` prawdopodobnie można ustawić w następujący sposób:

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

255 wygląda jak numer, ale, ponieważ `ConverterParameter` jest typu `Object`, analizator XAML traktuje 255 jako ciąg.

Z tego powodu konwerter wartości przedstawionych powyżej zawiera osobne `GetParameter` metoda obsługująca przypadków dla `parameter` jest typu `double`, `int`, lub `string`.  

**Selektor kolorów RGB** tworzy stronę `DoubleToIntConverter` w słowniku zasobów zgodnie z definicją dwie niejawne style:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.RgbColorSelectorPage"
             Title="RGB Color Selector">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <local:DoubleToIntConverter x:Key="doubleToInt" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:RgbColorViewModel Color="Gray" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Red}" />
            <Label Text="{Binding Red,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0:X2}'}" />

            <Slider Value="{Binding Green}" />
            <Label Text="{Binding Green,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0:X2}'}" />

            <Slider Value="{Binding Blue}" />
            <Label>
                <Label.Text>
                    <Binding Path="Blue"
                             StringFormat="Blue = {0:X2}"
                             Converter="{StaticResource doubleToInt}">
                        <Binding.ConverterParameter>
                            <x:Double>255</x:Double>
                        </Binding.ConverterParameter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Wartości `Red` i `Green` właściwości są wyświetlane z `Binding` — rozszerzenie znaczników. `Blue` Właściwości, jednak tworzy `Binding` klasy, aby zademonstrować sposób jawnego `double` ustawioną wartość `ConverterParameter` właściwości.

W tym miejscu jest wynikiem:

[![Selektor kolorów RGB](converters-images/rgbcolorselector-small.png "Selektor kolorów RGB")](converters-images/rgbcolorselector-large.png "Selektor kolorów RGB")



## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
