---
title: Konwertery wartości powiązania zestawu narzędzi Xamarin.Forms
description: W tym artykule opisano sposób rzutowania lub przekonwertować wartości w ramach zestawu narzędzi Xamarin.Forms wiązania danych poprzez implementację konwertera wartości (czyli znany także jako powiązania konwertera lub powiązania konwertera wartości).
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 28892692133020de1fa5a6eb007bb3f9bcf2612b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997488"
---
# <a name="xamarinforms-binding-value-converters"></a>Konwertery wartości powiązania zestawu narzędzi Xamarin.Forms

Powiązania danych zazwyczaj transfer danych z właściwością źródła do właściwości docelowej, a w niektórych przypadkach zapewniając z właściwości obiektu docelowego właściwości source. To przeniesienie jest bardzo proste, gdy właściwości źródłowe i docelowe są tego samego typu, lub gdy jeden typ może być konwertowany do innych typów za pośrednictwem niejawną konwersję. Gdy nie jest to przypadek, musi nastąpić konwersji typu.

W [ **formatowanie ciągów** ](string-formatting.md) artykule pokazano, jak można użyć `StringFormat` właściwości powiązania danych, aby przekonwertować dowolny typ ciągu. Dla innych typów konwersji, należy napisać niektóre wyspecjalizowany kod w klasie, która implementuje [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) interfejsu. (Platforma uniwersalna Windows zawiera podobne klasę o nazwie [ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) w `Windows.UI.Xaml.Data` przestrzeni nazw, ale to `IValueConverter` znajduje się w `Xamarin.Forms` przestrzeni nazw.) Klas, które implementują `IValueConverter` są nazywane *konwertery wartości*, ale one są często nazywane *powiązanie konwertery* lub *konwertery wartości powiązania*.

## <a name="the-ivalueconverter-interface"></a>Interfejsu IValueConverter

Załóżmy, że chcesz zdefiniować powiązania danych, w którym właściwość źródłowa jest typu `int` , ale właściwość docelowa jest `bool`. Chcesz, aby to powiązanie danych do tworzenia `false` wartości, gdy źródłem liczba całkowita jest równa 0, a `true` inaczej.  

Można to zrobić za pomocą klasy, która implementuje `IValueConverter` interfejsu:

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

Ustaw wystąpienia tej klasy, aby [ `Converter` ](xref:Xamarin.Forms.Binding.Converter) właściwość `Binding` klasy lub [ `Converter` ](xref:Xamarin.Forms.Xaml.BindingExtension.Converter) właściwość `Binding` — rozszerzenie znaczników. Ta klasa staje się częścią powiązanie danych.

`Convert` Metoda jest wywoływana, gdy dane są przenoszone ze źródła do docelowego w `OneWay` lub `TwoWay` powiązania. `value` Parametru jest obiekt lub wartość ze źródła powiązanie danych. Metoda musi zwracać wartość typu docelowego powiązanie danych. Metoda pokazane tutaj rzutowania `value` parametr `int` i porównuje ją z 0 dla `bool` zwracają wartość.

`ConvertBack` Metoda jest wywoływana, gdy dane są przenoszone z docelowym źródłem w `TwoWay` lub `OneWayToSource` powiązania. `ConvertBack` wykonuje odwrotną konwersję: przyjmuje, że parametr `value` jest równy `bool` od celu i konwertuje go na `int` wartość zwracaną dla źródła.

Jeśli powiązanie danych obejmuje również `StringFormat` ustawienie konwertera wartości jest wywoływany przed sformatowaniem wynik jako ciąg.

**Włącz przyciski** strony w [ **pokazy powiązania danych** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) przykład pokazuje, jak używać tego konwertera wartości w powiązaniu danych. `IntToBoolConverter` Zostanie uruchomiony w słowniku zasobów strony. Następnie jest wskazywane poprzez `StaticResource` — rozszerzenie znaczników można ustawić `Converter` właściwości powiązania danych dwa. Często zdarza się na udostępnianie danych konwertery między wiele powiązań danych na stronie:

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

Jeśli konwerter wartości jest używany na wielu stronach w aplikacji, można utworzyć wystąpienie go w słowniku zasobów w **App.xaml** pliku.

**Włącz przyciski** strony pokazuje często konieczne, kiedy `Button` wykonuje operację na tekst wpisywany przez użytkownika na podstawie `Entry` widoku. Jeśli nic nie zostało wpisane w `Entry`, `Button` powinna być wyłączona. Każdy `Button` zawiera powiązanie danych w jego `IsEnabled` właściwości. Źródło powiązania danych jest `Length` właściwość `Text` właściwości odpowiadającego `Entry`. Jeśli to `Length` właściwość nie jest 0, funkcja zwraca konwerter wartości `true` i `Button` jest włączona:

[![Włącz przyciski](converters-images/enablebuttons-small.png "Włącz przyciski")](converters-images/enablebuttons-large.png#lightbox "Włącz przyciski")

Należy zauważyć, że `Text` właściwość w każdym `Entry` jest inicjowana na pusty ciąg. `Text` Właściwość `null` domyślnie i dane wiązania nie będą działać w takiej sytuacji.

Niektóre konwertery wartości zostały napisane specjalnie dla określonej aplikacji, podczas gdy inne osoby zostały uogólnione. Jeśli wiesz, że konwertera wartości będą używane tylko w `OneWay` powiązania, a następnie `ConvertBack` po prostu może zwracać metoda `null`.

`Convert` Metoda powyżej niejawnie założono, że `value` argument jest typu `int` i zwracana wartość musi być typu `bool`. Podobnie `ConvertBack` metoda zakłada, że `value` argument jest typu `bool` i wartość zwracana jest `int`. Jeśli nie jest to przypadek, wystąpi wyjątek czasu wykonywania.

Można napisać konwertery wartości bardziej była uogólniona i mogą zaakceptować kilka różnych typów danych. `Convert` i `ConvertBack` metod można użyć `as` lub `is` operatory o `value` parametr, lub może wywołać `GetType` dla tego parametru można określić coś, jego typ, a następnie wykonaj odpowiednie. Oczekiwany typ wartości zwracanej z każdą z tych metod jest nadawana przez `targetType` parametru. Czasami konwertery wartości są używane wraz z powiązań danych z inną docelową typów; można użyć konwertera wartości `targetType` argumentu, aby przeprowadzić konwersję do poprawnego typu.

Jeśli konwersja jest wykonywana jest różne dla różnych kultur, użyj `culture` parametru, w tym celu. `parameter` Argument `Convert` i `ConvertBack` została omówiona w dalszej części tego artykułu.

## <a name="binding-converter-properties"></a>Właściwości konwerter powiązania

Klasy konwertera wartości może mieć właściwości i parametrów ogólnych. Konwertuje tego konwertera określoną wartość `bool` ze źródła do obiektu typu `T` dla obiektu docelowego:

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

**Wskaźników o przełącznikiem** strony pokazuje, jak może służyć do wyświetlania wartości `Switch` widoku. Wspólne dla wystąpienia konwertery wartości jako zasoby w słowniku zasobów, ale ta strona pokazuje alternatywa: konkretyzacji każdego konwertera wartości między `Binding.Converter` tagi element właściwości. `x:TypeArguments` Wskazuje argument rodzajowy i `TrueObject` i `FalseObject` są ustawione na obiekty tego typu:

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

W ciągu ostatnich trzech `Switch` i `Label` pary argument rodzajowy ustawiono `Style`i całe `Style` obiekty są dostarczane dla wartości `TrueObject` i `FalseObject`. Te zastąpienia styl niejawny `Label` ustawiony w słowniku zasobów, więc właściwości w stylu są jawnie przypisane do `Label`. Przełączanie `Switch` powoduje, że odpowiednie `Label` w celu odzwierciedlenia zmiany:

[![Wskaźniki przełączania](converters-images/switchindicators-small.png "Wskaźniki przełącznika")](converters-images/switchindicators-large.png#lightbox "Wskaźniki przełączania")


Istnieje również możliwość użycia [ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md) zaimplementować podobne zmiany w interfejsie użytkownika opartych na innych widokach.

## <a name="binding-converter-parameters"></a>Parametry konwerter powiązania

`Binding` Klasa definiuje [ `ConverterParameter` ](xref:Xamarin.Forms.Binding.ConverterParameter) właściwości i `Binding` definiuje również — rozszerzenie znaczników [ `ConverterParameter` ](xref:Xamarin.Forms.Xaml.BindingExtension.ConverterParameter) właściwości. Jeśli ta właściwość jest ustawiona, a następnie wartość jest przekazywana do `Convert` i `ConvertBack` metod jako `parameter` argumentu. Nawet wtedy, gdy wystąpienie konwertera wartości jest współużytkowana przez wiele powiązań danych, `ConverterParameter` może różnić się do wykonywania konwersji nieco inny.

Korzystanie z `ConverterParameter` jest przedstawiona w programie, wybór koloru. W tym przypadku `RgbColorViewModel` ma trzy właściwości typu `double` o nazwie `Red`, `Green`, i `Blue` używaną do konstruowania `Color` wartość:

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

`Red`, `Green`, I `Blue` właściwości zakresu od 0 do 1. Jednak najpierw, że składniki mają być wyświetlane jako dwucyfrowych wartości szesnastkowych.

Aby wyświetlić je jako wartości szesnastkowych XAML, muszą być pomnożona przez 255, konwertowana na liczbę całkowitą i następnie sformatowane przy użyciu specyfikacji "X2" `StringFormat` właściwości. Pierwsze dwa zadania (mnożenia wartości przez 255 i konwertowanie na liczbę całkowitą) może być obsługiwany przez konwerter wartości. Aby konwertera wartości jako uogólniony, jak to możliwe, można określić za pomocą mnożnik `ConverterParameter` właściwości, co oznacza, że wprowadza `Convert` i `ConvertBack` metod jako `parameter` argumentu:

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

`Convert` Konwertuje `double` do `int` podczas mnożenia wartości przez `parameter` wartość; `ConvertBack` dzieli liczbę całkowitą `value` argumentu przez `parameter` i zwraca `double` wynik. (W programie, pokazano poniżej, konwertera wartości jest używana tylko w połączeniu z ciągu formatowania, więc `ConvertBack` nie jest używana.)

Typ `parameter` argument prawdopodobnie może się różnić w zależności od tego, czy powiązanie danych jest zdefiniowana w kodzie lub XAML. Jeśli `ConverterParameter` właściwość `Binding` jest ustawiony w kodzie, prawdopodobnie należy ustawić na wartość liczbową:

```csharp
binding.ConverterParameter = 255;
```

`ConverterParameter` Właściwość jest typu `Object`, dzięki czemu kompilator języka C# interpretuje słowa kluczowe literału 255 jako liczba całkowita, a właściwość do tej wartości.

W XAML, jednak `ConverterParameter` prawdopodobnie należy ustawić w następujący sposób:

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

255 wygląda jak numer, ale, ponieważ `ConverterParameter` typu `Object`, analizator składni XAML traktuje 255 jako ciąg.

Z tego powodu konwertera wartości przedstawionych powyżej zawiera oddzielny `GetParameter` metoda, która obsługuje przypadków `parameter` typu `double`, `int`, lub `string`.  

**Selektor kolorów RGB** tworzy stronę `DoubleToIntConverter` w słowniku zasobów zgodnie z definicją dwóch niejawna style:

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

Wartości `Red` i `Green` właściwości są wyświetlane z `Binding` — rozszerzenie znaczników. `Blue` Własności, jednak tworzy `Binding` klasę wykazać w sposób jawny `double` może być równa wartości `ConverterParameter` właściwości.

Poniżej przedstawiono wyniki:

[![Selektor kolorów RGB](converters-images/rgbcolorselector-small.png "Selektor kolorów RGB")](converters-images/rgbcolorselector-large.png#lightbox "Selektor kolorów RGB")



## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
