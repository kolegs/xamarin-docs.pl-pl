---
title: Korzystanie z rozszerzeń struktury znaczników XAML
description: W tym artykule wyjaśniono, jak ulepszyć możliwości i elastyczność XAML, umożliwiając atrybutów elementów, należy ustawić z różnych źródeł przy użyciu rozszerzeń struktury znaczników XAML zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: a630d7c2acb95b7551c9f5f870078a0efcfc075c
ms.sourcegitcommit: ecdc031e9e26bbbf9572885531ee1f2e623203f5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393675"
---
# <a name="consuming-xaml-markup-extensions"></a>Korzystanie z rozszerzeń struktury znaczników XAML

Rozszerzeń struktury znaczników XAML zwiększanie możliwości i elastyczność XAML, umożliwiając atrybutów elementów, należy ustawić z różnych źródeł. Kilka rozszerzeń struktury znaczników XAML są częścią specyfikacji XAML 2009. Te pakiety są wyświetlane w plikach XAML z zwyczajowego `x` prefiks przestrzeni nazw i są czasami określane za pomocą tego prefiksu. Te ustawienia zostały opisane w poniższych sekcjach:

- [`x:Static`](#static) &ndash; odwoływać się do właściwości statycznych, pól ani elementów członkowskich wyliczenia.
- [`x:Reference`](#reference) &ndash; odwołanie o nazwie elementów na stronie.
- [`x:Type`](#type) &ndash; Ustaw atrybut `System.Type` obiektu.
- [`x:Array`](#array) &ndash; Skonstruuj tablicę obiektów określonego typu.
- [`x:Null`](#null) &ndash; Ustaw atrybut `null` wartość.

Dodatkowe rozszerzenia znaczników w XAML w przeszłości są obsługiwane przez inne implementacje XAML i są również obsługiwane przez zestaw narzędzi Xamarin.Forms. Te ustawienia zostały opisane w pełnym w innych artykułach:

- `StaticResource` &ndash; odwołują się do obiektów ze słownika zasobów, zgodnie z opisem w artykule [ **słowniki zasobów**](~/xamarin-forms/xaml/resource-dictionaries.md).
- `DynamicResource` &ndash; Odpowiadanie na zmiany w obiektach w słowniku zasobów, zgodnie z opisem w artykule [ **style dynamiczne**](~/xamarin-forms/user-interface/styles/dynamic.md).
- `Binding` &ndash; nawiązanie połączenia między właściwościami dwa obiekty, zgodnie z opisem w artykule [ **powiązanie danych**](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- `TemplateBinding` &ndash; wykonuje wiązanie danych z szablonu kontrolki, zgodnie z opisem w artykule [ **powiązanie z szablonu kontrolki**](/guides/xamarin-forms/application-fundamentals/templates/control-templates/template-binding/).

[ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) Układ korzysta z rozszerzeń niestandardowych znaczników [ `ConstraintExpression` ](xref:Xamarin.Forms.ConstraintExpression). Tego rozszerzenia znacznika jest opisany w artykule [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md).

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static — Rozszerzenie znaczników

`x:Static` — Rozszerzenie znaczników jest obsługiwana przez [ `StaticExtension` ](xref:Xamarin.Forms.Xaml.StaticExtension) klasy. Klasa ma jedną właściwość o nazwie [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member) typu `string` równa nazwę publicznej — stała, właściwość statyczna, pole statyczne lub element członkowski wyliczenia.

Typowym sposobem użycia `x:Static` jest najpierw zdefiniować klasę z niektórych stałe i zmienne statyczne, takie jak tym niewielki `AppConstants` klasy w [ **wyrażeń MarkupExtension** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) program:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X: Static pokaz** strony pokazano kilka sposobów wykorzystania `x:Static` — rozszerzenie znaczników. Tworzy wystąpienie najpełniejszych podejście `StaticExtension` klasy między `Label.FontSize` tagi element właściwości:

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

Umożliwia także analizator XAML `StaticExtension` klasy, aby stosować skrót `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

To może być jeszcze bardziej uproszczone, ale zmiany wprowadza pewne nowej składni: składa się z umieszczenie `StaticExtension` klas i składowych w nawiasy klamrowe. Wyrażenie wynikowe ustawiono bezpośrednio do `FontSize` atrybutu:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Należy zauważyć, że istnieją *nie* cudzysłowy wewnątrz nawiasu klamrowego. `Member` Właściwość `StaticExtension` nie jest już atrybut XML. Zamiast tego jest częścią wyrażenia dla rozszerzenia znaczników.

Tak samo jak można skrócić `x:StaticExtension` do `x:Static` go użyć jako elementu obiektu, również można na skrócić je w wyrażeniu w nawiasy klamrowe:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension` Klasa ma `ContentProperty` atrybut odwołuje się do właściwości `Member`, która oznacza tę właściwość jako właściwość zawartości domyślnej klasy. Dla rozszerzeń struktury znaczników XAML wyrażona za pomocą nawiasów klamrowych, można wyeliminować `Member=` część wyrażenia:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Jest to najczęściej używany typ `x:Static` — rozszerzenie znaczników.

**Statyczne pokaz** strona zawiera dwie inne przykłady. Tag główny plik XAML zawiera deklaracji przestrzeni nazw XML dla platformy .NET `System` przestrzeni nazw:

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Dzięki temu `Label` rozmiar czcionki, należy ustawić pole statyczne `Math.PI`. Który skutkuje zamiast małego tekstu, więc `Scale` właściwość jest ustawiona na `Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

W ostatnim przykładzie są wyświetlane `Device.RuntimePlatform` wartość. `Environment.NewLine` Właściwość statyczna służy do wstawiania znaków nowego wiersza, między tymi dwoma `Span` obiektów:

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

Poniżej przedstawiono przykładowe uruchomione na wszystkich trzech platformach:

[![x: Static pokaz](consuming-images/staticdemo-small.png "pokaz x: Static")](consuming-images/staticdemo-large.png#lightbox "pokaz x: Static")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference — Rozszerzenie znaczników

`x:Reference` — Rozszerzenie znaczników jest obsługiwana przez [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) klasy. Klasa ma jedną właściwość o nazwie [ `Name` ](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) typu `string` równa nazwa elementu na stronie, któremu nadano nazwę zawierającą `x:Name`. To `Name` właściwość jest właściwość content `ReferenceExtension`, więc `Name=` nie jest wymagane podczas `x:Reference` pojawia się w nawiasach klamrowych.

`x:Reference` — Rozszerzenie znaczników jest używane wyłącznie w przypadku powiązania danych, które są opisane bardziej szczegółowo w artykule [ **powiązanie danych**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**X: Reference pokaz** stronie znajdują się dwa przypadki użycia `x:Reference` przy użyciu powiązania danych pierwszego, gdy jest używana do ustawiania `Source` właściwość `Binding` obiekt i drugi, gdy jest używana do ustawiania `BindingContext` Właściwość powiązań danych dwa:

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

Zarówno `x:Reference` wyrażeń użyj skróconej wersji `ReferenceExtension` Nazwa klasy obsługi i eliminowanie `Name=` część wyrażenia. W pierwszym przykładzie `x:Reference` — rozszerzenie znaczników jest osadzony w `Binding` — rozszerzenie znaczników. Należy zauważyć, że `Source` i `StringFormat` ustawienia są oddzielone przecinkami. W tym miejscu jest uruchomiony na wszystkich trzech platformach program:

[![x: Reference pokaz](consuming-images/referencedemo-small.png "pokaz x: Reference")](consuming-images/referencedemo-large.png#lightbox "pokaz x: Reference")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type — Rozszerzenie znaczników

`x:Type` — Rozszerzenie znaczników jest odpowiednikiem XAML języka C# [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) — słowo kluczowe. Nie jest obsługiwany przez [ `TypeExtension` ](xref:Xamarin.Forms.Xaml.TypeExtension) klasy, która definiuje jedną właściwość o nazwie [ `TypeName` ](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) typu `string` , jest ustawiona na nazwę klasy lub struktury. `x:Type` Zwraca rozszerzenia znaczników [ `System.Type` ](xref:System.Type) obiekt tej klasy lub struktury. `TypeName` Właściwość content jest `TypeExtension`, więc `TypeName=` nie jest wymagane podczas `x:Type` pojawia się za pomocą nawiasów klamrowych.

W ramach zestawu narzędzi Xamarin.Forms, istnieje kilka właściwości, które mają argumentów typu `Type`. Przykłady obejmują [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) właściwość `Style`i [x: typearguments —](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) atrybutu można określić argumentów w klasach ogólnych. Jednak wykonuje analizatora XAML `typeof` operacji automatycznie, a `x:Type` — rozszerzenie znaczników nie jest używany w tych przypadkach.

Jedno miejsce gdzie `x:Type` *jest* wymagane jest `x:Array` — rozszerzenie znaczników, który jest opisany w [następnej sekcji](#array).

`x:Type` — Rozszerzenie znaczników jest również przydatne podczas tworzenia menu, w którym każdy element menu odnosi się do obiektu określonego typu. Możesz skojarzyć `Type` obiekt z każdego elementu menu, a następnie utwórz wystąpienie obiektu, gdy zaznaczony zostanie element menu.

Jest to jak menu nawigacji w `MainPage` w **— rozszerzenia znaczników** program działa. **MainPage.xaml** plik zawiera `TableView` z każdym `TextCell` odpowiadający określonej strony w programie:

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

Oto strona główna otwierania w **— rozszerzenia znaczników**:

[![Główny strony](consuming-images/mainpage-small.png "Main strony")](consuming-images/mainpage-large.png#lightbox "Main strony")

Każdy `CommandParameter` właściwość jest ustawiona na `x:Type` rozszerzenie znaczników, który odwołuje się do jednego z innych stron. `Command` Właściwość jest powiązana z właściwością o nazwie `NavigateCommand`. Ta właściwość jest zdefiniowana w `MainPage` pliku związanego z kodem:

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

`NavigateCommand` Właściwość `Command` obiekt, który implementuje wykonuje polecenie przy użyciu argumentu typu `Type` &mdash; wartość `CommandParameter`. Metoda używa `Activator.CreateInstance` do utworzenia wystąpienia strony i następnie przechodzi do niego. Konstruktor stwierdza, ustawiając `BindingContext` strony do samego siebie, co pozwala `Binding` na `Command` do pracy. Zobacz [ **powiązanie danych** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) artykułu i szczególnie [ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) artykuł, aby uzyskać więcej informacji na temat tego typu kodu.

**X: Type pokaz** strona używa podobne techniki tworzenia wystąpienia elementów zestawu narzędzi Xamarin.Forms oraz dodanie ich do `StackLayout`. Plik XAML początkowo składa się z trzech `Button` elementów przy użyciu ich `Command` właściwości ustawione na `Binding` i `CommandParameter` właściwości ustawione na typy trzy widoki Xamarin.Forms:

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

Plik związany z kodem definiuje i inicjuje `CreateCommand` właściwości:

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

Metoda, która jest wykonywane, kiedy `Button` naciśnięciu tworzy nowe wystąpienie argumentu, ustawia jego `VerticalOptions` właściwości i dodaje go do `StackLayout`. Trzy `Button` elementy następnie udostępnić stronie dynamicznie utworzoną widoki:

[![x: Type pokaz](consuming-images/typedemo-small.png "pokaz x: Type")](consuming-images/typedemo-large.png#lightbox "pokaz x: Type")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array — Rozszerzenie znaczników

`x:Array` — Rozszerzenie znaczników pozwala zdefiniować tablicę w znacznikach. Nie jest obsługiwany przez [ `ArrayExtension` ](xref:Xamarin.Forms.Xaml.ArrayExtension) klasy, która definiuje dwie właściwości:

- `Type` typu `Type`, która wskazuje typ elementów w tablicy.
- `Items` typu `IList`, który stanowi kolekcję same elementy. Jest to właściwość content `ArrayExtension`.

`x:Array` — Rozszerzenie znaczników sam nie jest wyświetlana w nawiasy klamrowe. Zamiast tego `x:Array` tagiem początkowym i końcowym ograniczania listy elementów. Ustaw `Type` właściwość `x:Type` — rozszerzenie znaczników.

**X: Array pokaz** strona przedstawia sposób użycia `x:Array` do dodawania elementów do `ListView` , ustawiając `ItemsSource` właściwości tablicy:

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

`ViewCell` Tworzy prostą `BoxView` dla każdego wpisu kolorów:

[![x: Array pokaz](consuming-images/arraydemo-small.png "pokaz x: Array")](consuming-images/arraydemo-large.png#lightbox "pokaz x: Array")

Istnieje kilka sposobów, aby określić poszczególnych `Color` elementów w tej tablicy. Możesz użyć `x:Static` — rozszerzenie znaczników:

```xaml
<x:Static Member="Color.Blue" />
```

Alternatywnie można użyć `StaticResource` można pobrać koloru ze słownika zasobów:

```xaml
<StaticResource Key="myColor" />
```

Pod koniec tego artykułu zobaczysz niestandardowego rozszerzenia znaczników XAML, który również tworzy nową wartość koloru:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

Podczas definiowania tablic popularnych typów, takich jak ciągów lub liczby, za pomocą tagów na liście [ **przekazywania argumentów konstruktora** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) artykuł, aby ograniczyć wartości.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null — Rozszerzenie znaczników

`x:Null` — Rozszerzenie znaczników jest obsługiwana przez [ `NullExtension` ](xref:Xamarin.Forms.Xaml.NullExtension) klasy. Nie ma właściwości i jest po prostu XAML odpowiednikiem języka C# [ `null` ](/dotnet/csharp/language-reference/keywords/null/) — słowo kluczowe.

`x:Null` — Rozszerzenie znaczników jest rzadko potrzebne i rzadko używane, ale jeśli znajdziesz na potrzeby się dobrze, że istnieje.

**X: Null pokaz** strona przedstawia jeden scenariusz przypadku `x:Null` może być łatwo. Załóżmy, że należy zdefiniować ukrytego `Style` dla `Label` zawierającej `Setter` określająca `FontFamily` właściwość na nazwę rodziny zależny od platformy:

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

A następnie użytkownik stwierdza, że dla jednego z `Label` elementów, chcesz, aby wszystkie ustawienia właściwości w niejawny `Style` z wyjątkiem `FontFamily`, który ma być wartością domyślną. Można zdefiniować inny `Style` do tego celu, ale prostszej metody jest po prostu ustaw `FontFamily` właściwości określonych `Label` do `x:Null`, jak pokazano w Centrum `Label`.

W tym miejscu jest uruchomiony na trzech platformach program:

[![x: Null pokaz](consuming-images/nulldemo-small.png "pokaz x: Null")](consuming-images/nulldemo-large.png#lightbox "pokaz x: Null")

Informacja dotycząca tego cztery `Label` elementy mają czcionki serif, ale Centrum `Label` ma domyślną czcionkę sans-serif.

## <a name="define-your-own-markup-extensions"></a>Zdefiniowanie własnego rozszerzenia znaczników

Jeśli wystąpiły na potrzeby rozszerzenia znaczników XAML, który nie jest dostępny w interfejsie Xamarin.Forms, możesz to zrobić [Utwórz swoje własne](creating.md).


## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia znaczników (przykład)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [XAML znaczników rozszerzenia rozdziału z książki zestawu narzędzi Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Słowniki zasobów](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Style dynamiczne](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Powiązanie danych](~/xamarin-forms/app-fundamentals/data-binding/index.md)
