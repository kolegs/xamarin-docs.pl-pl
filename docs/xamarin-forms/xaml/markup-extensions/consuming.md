---
title: Korzystanie z rozszerzeń znaczników XAML
description: Użyj rozszerzenia znaczników XAML dostępne w platformy Xamarin.Forms
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: 25eada483e8bd2ce95cb3101dfe873ea38b283ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-xaml-markup-extensions"></a>Korzystanie z rozszerzeń znaczników XAML

Rozszerzenia znaczników XAML zwiększanie możliwościach i elastyczności XAML, zezwalając atrybuty elementu ustawiono z różnych źródeł. Kilka rozszerzeń znaczników XAML są częścią specyfikacji języka XAML 2009. Są one wyświetlane w plikach XAML z zwyczajowe `x` prefiks przestrzeni nazw i są powszechnie z tym prefiksem. Te ustawienia zostały opisane w poniższych sekcjach:

- [`x:Static`](#static) &ndash; odwołania pola, statycznej właściwości lub elementy członkowskie wyliczenia.
- [`x:Reference`](#reference) &ndash; odwołanie o nazwie elementów na stronie.
- [`x:Type`](#type) &ndash; Ustaw dla atrybutu `System.Type` obiektu.
- [`x:Array`](#array) &ndash; konstruować tablicę obiektów określonego typu.
- [`x:Null`](#null) &ndash; Ustaw dla atrybutu `null` wartość.

Trzy inne rozszerzenia znaczników XAML w przeszłości są obsługiwane przez inne implementacje XAML, a także są obsługiwane przez platformy Xamarin.Forms. Te ustawienia zostały opisane w pełnym w innych artykułach:

- `StaticResource` &ndash; odwoływać się do obiektów ze słownika zasobów, zgodnie z opisem w artykule [ **słowniki zasobów**](~/xamarin-forms/xaml/resource-dictionaries.md).
- `DynamicResource` &ndash; Odpowiadanie na zmiany w obiektach w słowniku zasobów, zgodnie z opisem w artykule [ **dynamiczne style**](~/xamarin-forms/user-interface/styles/dynamic.md).
- `Binding` &ndash; nawiązanie połączenia między właściwościami dwa obiekty, zgodnie z opisem w artykule [ **powiązania danych**](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- `TemplateBinding` &ndash; wykonuje powiązanie danych z szablonu kontrolki, zgodnie z opisem w artykule [**powiązanie z szablonu kontroli**] / przewodniki/xamarin-/ aplikacji — podstawowe informacje na temat/szablonów/sterowania — szablony/szablonu — wiązanie formularzy /)

[ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) Układu użyto rozszerzenia niestandardowego znacznika [ `ConstraintExpression` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/). Tego rozszerzenia znacznika jest opisana w artykule [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md).

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static — Rozszerzenie znaczników

`x:Static` — Rozszerzenie znaczników jest obsługiwana przez [ `StaticExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) klasy. Klasa ma tylko jedną właściwość o nazwie [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) typu `string` ustawioną nazwę stała publicznej, statycznej właściwości, pola statycznego lub element członkowski wyliczenia.

Typowy sposób użycia `x:Static` jest najpierw zdefiniować klasy z niektóre stałe i zmienne statyczne, takie jak tym niewielki rozmiar `AppConstants` klasy w [ **wyrażenia MarkupExtension** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) program:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X: Static pokaz** strony przedstawiono kilka sposobów użycia `x:Static` — rozszerzenie znaczników. Tworzy wystąpienie najpełniejszych podejście `StaticExtension` klas między `Label.FontSize` znaczniki elementu właściwości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.StaticDemoPage"
             Title="x:Static Demo">
    <StackLayout Margin="10, 0">
        <Label Text="Label No. 1">
            <Label.FontSize>
                <x:StaticExtension Member="local:AppConstants.NormalFontSize" />
            </Label.FontSize>
        </Label>

        ···

    </StackLayout>
</ContentPage>
```

Umożliwia również analizator XAML `StaticExtension` klasę, aby stosować skrót `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Może to uprościć jeszcze bardziej, ale niektóre nowej składni wprowadza zmiany: składa się z umieszczanie `StaticExtension` klasy i element członkowski w nawiasy klamrowe. Wynikowe wyrażenie ustawiono bezpośrednio do `FontSize` atrybutu:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Należy zauważyć, że istnieją *nie* cudzysłów wewnątrz nawiasów klamrowych. `Member` Właściwość `StaticExtension` nie jest już atrybutu XML. Zamiast tego jest częścią wyrażenia rozszerzenia znaczników.

Jak można skrócić `x:StaticExtension` do `x:Static` używania go jako elementu obiektu, można również skrócić go w wyrażeniu w nawiasy klamrowe:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension` Klasa ma `ContentProperty` atrybut odwołuje się do właściwości `Member`, która oznacza tę właściwość jako właściwość content domyślnej klasy. Rozszerzenia znaczników XAML wyrażone w nawiasach klamrowych, można wyeliminować `Member=` część wyrażenia:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

To jest najczęściej używana forma `x:Static` — rozszerzenie znaczników.

**Statycznych pokaz** strona zawiera dwa inne przykłady. Tag główny pliku XAML zawiera deklaracji przestrzeni nazw XML dla programu .NET `System` przestrzeni nazw:

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Dzięki temu `Label` rozmiar czcionki, należy ustawić pola statycznego `Math.PI`. Która powoduje raczej małego tekstu, więc `Scale` właściwość jest ustawiona na `Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

Wyświetla końcowy przykład `Device.RuntimePlatform` wartość. `Environment.NewLine` Właściwość statyczna służy do wstawiania znaków nowego wiersza, między tymi dwoma `Span` obiektów:

```xaml
<Label HorizontalTextAlignment="Center"
       FontSize="{x:Static local:AppConstants.NormalFontSize}">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Runtime Platform: " />
            <Span Text="{x:Static sys:Environment.NewLine}" />
            <Span Text="{x:Static Device.RuntimePlatform}" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Oto przykładowe działająca na wszystkich platformach trzy:

[![x: Static pokaz](consuming-images/staticdemo-small.png "pokaz x: Static")](consuming-images/staticdemo-large.png#lightbox "pokaz x: Static")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference — Rozszerzenie znaczników

`x:Reference` — Rozszerzenie znaczników jest obsługiwana przez [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) klasy. Klasa ma tylko jedną właściwość o nazwie [ `Name` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.ReferenceExtension.Name/) typu `string` ustawioną nazwę elementu na stronie została podana nazwa z `x:Name`. To `Name` właściwość jest właściwość content `ReferenceExtension`, więc `Name=` nie jest wymagana podczas `x:Reference` pojawia się w nawiasach klamrowych.

`x:Reference` — Rozszerzenie znaczników jest używane wyłącznie w przypadku powiązań danych, które są opisane bardziej szczegółowo w artykule [ **powiązania danych**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**X: Reference pokaz** strona zawiera dwa zastosowania `x:Reference` z powiązaniami danych pierwszego, gdy służy do ustawiania `Source` właściwość `Binding` obiektu, a drugi służy do ustawienia `BindingContext` Właściwość powiązania dwóch danych:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ReferenceDemoPage"
             x:Name="page"
             Title="x:Reference Demo">

    <StackLayout Margin="10, 0">

        <Label Text="{Binding Source={x:Reference page},
                              StringFormat='The type of this page is {0}'}"
               FontSize="18"
               VerticalOptions="CenterAndExpand"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="Center" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='{0:F0}&#x00B0; rotation'}"
               Rotation="{Binding Value}"
               FontSize="24"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

Zarówno `x:Reference` wyrażenia użyj skróconej wersji `ReferenceExtension` Nazwa klasy i eliminowania `Name=` część wyrażenia. W pierwszym przykładzie `x:Reference` — rozszerzenie znaczników jest osadzony w `Binding` — rozszerzenie znaczników. Zwróć uwagę, że `Source` i `StringFormat` ustawienia są oddzielone przecinkami. Oto programu uruchomionego na wszystkich platformach trzy:

[![x: Reference pokaz](consuming-images/referencedemo-small.png "pokaz x: Reference")](consuming-images/referencedemo-large.png#lightbox "pokaz x: Reference")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type — Rozszerzenie znaczników

`x:Type` — Rozszerzenie znaczników jest odpowiednikiem XAML języka C# [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) — słowo kluczowe. Nie jest obsługiwany przez [ `TypeExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) klasy, która definiuje jedną właściwość o nazwie [ `TypeName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.TypeExtension.TypeName/) typu `string` która jest ustawiona na nazwę klasy lub struktury. `x:Type` Zwraca rozszerzenia znaczników [ `System.Type` ](https://developer.xamarin.com/api/type/System.Type/) obiektu tej klasy lub struktury. `TypeName` Właściwość content jest `TypeExtension`, więc `TypeName=` nie jest wymagana podczas `x:Type` pojawia się w nawiasach klamrowych.

W ramach platformy Xamarin.Forms, istnieje kilka właściwości, które mają argumentów typu `Type`. Przykłady obejmują [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) właściwość `Style`i [x: typearguments —](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) atrybut służy do określania argumentów w klasach ogólnych. Jednak wykonuje działanie analizatora składni języka XAML `typeof` operacji automatycznie oraz `x:Type` — rozszerzenie znaczników nie jest używany w tych przypadkach.

W jednym miejscu gdzie `x:Type` *jest* wymagane jest z `x:Array` — rozszerzenie znaczników, który jest opisany w [następnej sekcji](#array).

`x:Type` — Rozszerzenie znaczników jest również przydatne podczas tworzenia menu, w którym każdy element menu odnosi się do obiektu określonego typu. Możesz skojarzyć `Type` obiekt z każdym elementem menu, a następnie utwórz wystąpienie obiektu, gdy element menu zostanie zaznaczony.

Jest to sposób menu nawigacji w `MainPage` w **rozszerzenia znaczników** program działa. **MainPage.xaml** plik zawiera `TableView` z każdym `TextCell` odpowiadający określonej strony w programie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.MainPage"
             Title="Markup Extensions"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection>
                <TextCell Text="x:Static Demo"
                          Detail="Access constants or statics"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:StaticDemoPage}" />

                <TextCell Text="x:Reference Demo"
                          Detail="Reference named elements on the page"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ReferenceDemoPage}" />

                <TextCell Text="x:Type Demo"
                          Detail="Associate a Button with a Type"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:TypeDemoPage}" />

                <TextCell Text="x:Array Demo"
                          Detail="Use an array to fill a ListView"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ArrayDemoPage}" />

                ···                          

        </TableRoot>
    </TableView>
</ContentPage>
```

Oto otwierania strony głównej **rozszerzenia znaczników**:

[![Główne strony](consuming-images/mainpage-small.png "Main strony")](consuming-images/mainpage-large.png#lightbox "Main strony")

Każdy `CommandParameter` właściwość jest ustawiona na `x:Type` rozszerzenie znaczników, który odwołuje się do jednego z innych stron. `Command` Właściwość jest powiązana z właściwości o nazwie `NavigateCommand`. Ta właściwość jest zdefiniowana w `MainPage` pliku CodeBehind:

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

`NavigateCommand` Właściwość jest `Command` obiekt, który implementuje wykonuje polecenie z argumentem typu `Type` &mdash; wartość `CommandParameter`. W metodzie `Activator.CreateInstance` można utworzyć wystąpienia na stronie, a następnie przechodzi do niego. Konstruktor stwierdza, ustawiając `BindingContext` strony do samej siebie, co pozwala `Binding` na `Command` do pracy. Zobacz [ **powiązania danych** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) artykuł, a szczególnie [ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) artykułu, aby uzyskać więcej informacji o tym typie kodu.

**X: Type pokaz** strona używa technika podobne do utworzenia wystąpienia elementy platformy Xamarin.Forms oraz dodanie ich do `StackLayout`. Plik XAML początkowo składa się z trzech `Button` elementy z ich `Command` właściwości `Binding` i `CommandParameter` właściwości mają typy trzy widoki platformy Xamarin.Forms:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.TypeDemoPage"
             Title="x:Type Demo">

    <StackLayout x:Name="stackLayout"
                 Padding="10, 0">

        <Button Text="Create a Slider"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Slider}" />

        <Button Text="Create a Stepper"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Stepper}" />

        <Button Text="Create a Switch"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Switch}" />
    </StackLayout>
</ContentPage>
```

Plik CodeBehind definiuje i inicjuje `CreateCommand` właściwości:

```csharp
public partial class TypeDemoPage : ContentPage
{
    public TypeDemoPage()
    {
        InitializeComponent();

        CreateCommand = new Command<Type>((Type viewType) =>
        {
            View view = (View)Activator.CreateInstance(viewType);
            view.VerticalOptions = LayoutOptions.CenterAndExpand;
            stackLayout.Children.Add(view);
        });

        BindingContext = this;
    }

    public ICommand CreateCommand { private set; get; }
}
```

Metoda, która jest wykonywane, kiedy `Button` naciśnięciu tworzy nowe wystąpienie argumentu, ustawia jego `VerticalOptions` właściwości i dodaje go do `StackLayout`. Trzy `Button` elementy następnie udostępnić stronie utworzony dynamicznie widoków:

[![x: Type pokaz](consuming-images/typedemo-small.png "pokaz x: Type")](consuming-images/typedemo-large.png#lightbox "pokaz x: Type")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array — Rozszerzenie znaczników

`x:Array` — Rozszerzenie znaczników można zdefiniować tablicy w znaczniku. Nie jest obsługiwany przez [ `ArrayExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) klasy, która definiuje dwie właściwości:

- `Type` typu `Type`, która wskazuje typ elementów w tablicy.
- `Items` typu `IList`, która jest kolekcją same elementy. Jest to właściwość content `ArrayExtension`.

`x:Array` — Rozszerzenie znaczników sam nie jest wyświetlana w nawiasach klamrowych. Zamiast tego `x:Array` tagiem początkowym i końcowym ograniczyć listę elementów. Ustaw `Type` właściwości `x:Type` — rozszerzenie znaczników.

**X: Array pokaz** strona przedstawia sposób użycia `x:Array` do dodawania elementów do `ListView` przez ustawienie `ItemsSource` właściwości do tablicy:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ArrayDemoPage"
             Title="x:Array Demo Page">
    <ListView Margin="10">
        <ListView.ItemsSource>
            <x:Array Type="{x:Type Color}">
                <Color>Aqua</Color>
                <Color>Black</Color>
                <Color>Blue</Color>
                <Color>Fuchsia</Color>
                <Color>Gray</Color>
                <Color>Green</Color>
                <Color>Lime</Color>
                <Color>Maroon</Color>
                <Color>Navy</Color>
                <Color>Olive</Color>
                <Color>Pink</Color>
                <Color>Purple</Color>
                <Color>Red</Color>
                <Color>Silver</Color>
                <Color>Teal</Color>
                <Color>White</Color>
                <Color>Yellow</Color>
            </x:Array>
        </ListView.ItemsSource>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <BoxView Color="{Binding}"
                             Margin="3" />    
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>        
```

`ViewCell` Tworzy prosty `BoxView` dla każdego wpisu kolorów:

[![x: Array pokaz](consuming-images/arraydemo-small.png "pokaz x: Array")](consuming-images/arraydemo-large.png#lightbox "pokaz x: Array")

Istnieje kilka sposobów, aby określić poszczególne `Color` elementów w tej macierzy. Można użyć `x:Static` — rozszerzenie znaczników:

```xaml
<x:Static Member="Color.Blue" />
```

Możesz też użyć `StaticResource` można pobrać koloru ze słownika zasobów:

```xaml
<StaticResource Key="myColor" />
```

Na końcu tego artykułu zobaczysz niestandardowego rozszerzenia znaczników XAML tworzący nową wartość kolorów:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

Podczas definiowania tablice popularnych typów ciągów lub numery, użyj tagów na liście [ **przekazywanie argumentów konstruktora** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) artykuł, aby ograniczyć wartości.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null — Rozszerzenie znaczników

`x:Null` — Rozszerzenie znaczników jest obsługiwana przez [ `NullExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) klasy. Nie ma właściwości oraz jest po prostu odpowiednikiem XAML języka C# [ `null` ](/dotnet/csharp/language-reference/keywords/null/) — słowo kluczowe.

`x:Null` — Rozszerzenie znaczników jest rzadko potrzebne i rzadko używane, ale jeśli znajdziesz potrzebę będzie Cieszymy się, że czy ona istnieje.

**X: Null pokaz** strony przedstawiono scenariusz podczas `x:Null` może być łatwo. Załóżmy, że należy zdefiniować niejawny `Style` dla `Label` zawierającą `Setter` stanowiąca `FontFamily` właściwość na nazwę rodziny zależny od platformy:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.NullDemoPage"
             Title="x:Null Demo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="48" />
                <Setter Property="FontFamily">
                    <Setter.Value>
                        <OnPlatform x:TypeArguments="x:String">
                            <On Platform="iOS" Value="Times New Roman" />
                            <On Platform="Android" Value="serif" />
                            <On Platform="UWP" Value="Times New Roman" />
                        </OnPlatform>
                    </Setter.Value>
                </Setter>
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.Content>
        <StackLayout Padding="10, 0">
            <Label Text="Text 1" />
            <Label Text="Text 2" />

            <Label Text="Text 3"
                   FontFamily="{x:Null}" />

            <Label Text="Text 4" />
            <Label Text="Text 5" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>   
```

Następnie użytkownik stwierdza, że dla jednego z `Label` elementów, ustawienia właściwości w niejawnych `Style` z wyjątkiem `FontFamily`, który ma być wartością domyślną. Można zdefiniować innego `Style` do tego celu, ale prostsze jest po prostu określenie `FontFamily` właściwości określonej `Label` do `x:Null`, jak to pokazano w Centrum `Label`.

Oto działająca na platformach trzy program:

[![x:Null Demo](consuming-images/nulldemo-small.png "x:Null Demo")](consuming-images/nulldemo-large.png#lightbox "x:Null Demo")

O tym, że cztery `Label` elementy mają czcionki serif, ale Centrum `Label` ma sans-serif domyślnej czcionki.

## <a name="define-your-own-markup-extensions"></a>Zdefiniowanie własnego rozszerzenia znaczników

Jeśli zostały napotkane potrzebę rozszerzenie znaczników XAML, który nie jest dostępna w platformy Xamarin.Forms, możesz [Utwórz swój własny](creating.md).


## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia znaczników (przykład)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Rozdział rozszerzeń znaczników XAML z książki platformy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Słowniki zasobów](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Style dynamiczne](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Powiązanie danych](~/xamarin-forms/app-fundamentals/data-binding/index.md)
