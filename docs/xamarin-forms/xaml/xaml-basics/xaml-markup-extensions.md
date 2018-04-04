---
title: Część 3. Rozszerzenia znaczników XAML
description: Rozszerzenia znaczników XAML stanowi ważną funkcją w języku XAML, który umożliwia właściwości można ustawić obiektów lub wartości, które odwołuje się pośrednio z innych źródeł. Rozszerzenia znaczników XAML są szczególnie ważne w przypadku udostępniania obiektów i odwołuje się do stałych używanych w całej aplikacji, ale ich największy narzędzie znajduje się w powiązania danych.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: charlespetzold
ms.author: chape
ms.date: 3/27/2018
ms.openlocfilehash: 104a3adb5d59bc7feafa3c993290247b749ce312
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="part-3-xaml-markup-extensions"></a>Część 3. Rozszerzenia znaczników XAML

_Rozszerzenia znaczników XAML stanowi ważną funkcją w języku XAML, który umożliwia właściwości można ustawić obiektów lub wartości, które odwołuje się pośrednio z innych źródeł. Rozszerzenia znaczników XAML są szczególnie ważne w przypadku udostępniania obiektów i odwołuje się do stałych używanych w całej aplikacji, ale ich największy narzędzie znajduje się w powiązania danych._

## <a name="xaml-markup-extensions"></a>Rozszerzenia znaczników XAML

Ogólnie rzecz biorąc Użyj XAML można ustawić właściwości obiektu do jawnej wartości, takich jak ciąg, liczbę, elementu członkowskiego wyliczenia lub ciąg, który jest konwertowany na wartość w tle.

Czasami jednak właściwości zamiast tego należy odwoływać się wartości zdefiniowanych w innym miejscu lub które mogą wymagać małego przetwarzania kodu w czasie wykonywania. W tym celu XAML *rozszerzenia znaczników* są dostępne.

Te rozszerzenia znaczników XAML nie są rozszerzenia XML. XAML jest całkowicie prawne XML. Są nazywane "rozszerzenia" ponieważ obsługiwanych przez kod klas, które implementują `IMarkupExtension`. Można pisać własne rozszerzenia znacznika niestandardowego.

W wielu przypadkach rozszerzenia znaczników XAML rozpoznawalnych natychmiast w plikach XAML ponieważ pojawią się one zgodnie z ustawieniami atrybutu rozdzielone nawiasów klamrowych: {i}, ale czasami rozszerzenia znaczników znajdował się w znaczniku jako elementy z konwencjonalnej.

## <a name="shared-resources"></a>Udostępnione zasoby

Niektóre strony XAML zawiera kilka widoków z tej samej wartości właściwości. Na przykład wiele ustawienia właściwości tych `Button` obiekty są takie same:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

    </StackLayout>
</ContentPage>
```

Jedną z tych właściwości muszą być zmienione, warto wprowadzić zmiany tylko raz, a nie trzy razy. Gdyby ten kod, będą prawdopodobnie stosowane stałe i statycznych obiektów w trybie tylko do odczytu aby zapewnić spójny i łatwy do modyfikowania takich wartości.

W języku XAML, jedno rozwiązanie popularnych jest przechowywanie takich wartości lub obiektów w *słownik zasobów*. `VisualElement` Klasa definiuje właściwość o nazwie `Resources` typu `ResourceDictionary`, która jest słownik z kluczami typu `string` i wartości typu `object`. Można umieścić obiektów w tym słowniku i odwoływać je z kodu znaczników w XAML.

Aby użyć słownika zasobów na stronie, zawierać pary `Resources` znaczniki elementu właściwości. Jest najodpowiedniejszym umieścić je w górnej części strony:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>

    </ContentPage.Resources>
    ...
</ContentPage>
```

Należy również uwzględnić jawnie `ResourceDictionary` tagów:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Teraz wartości różnych typów obiektów i mogą być dodawane do słownika zasobów. Te typy muszą być tworzone jako wystąpienia. Klasy abstrakcyjne nie można na przykład. Te typy również musi mieć publicznego konstruktora bez parametrów. Każdy element wymaga klucza słownika określony za pomocą `x:Key` atrybutu. Na przykład:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Te dwie pozycje są wartościami typu struktury `LayoutOptions`i każda ma unikatowy klucz i jednego lub dwóch właściwości ustawione. W kodzie i znacznika jest bardziej wspólnego do użycia statycznego pola `LayoutOptions`, ale w tym miejscu jest wygodniejsze do ustawiania właściwości.

Teraz należy ustawić `HorizontalOptions` i `VerticalOptions` właściwości tych przycisków dla tych zasobów i który wykonuje się za pomocą `StaticResource` rozszerzenie znaczników w XAML:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

`StaticResource` — Rozszerzenie znaczników zawsze rozdzielana w nawiasach klamrowych i zawiera klucz słownika.

Nazwa `StaticResource` odróżnia go od `DynamicResource`, który obsługuje również platformy Xamarin.Forms. `DynamicResource` dotyczy klucze słownikowe związanych z wartościami, które może zmieniać się w czasie wykonywania, gdy `StaticResource` uzyskuje dostęp do elementów ze słownika tylko raz podczas zbudowanych elementów na stronie.

Aby uzyskać `BorderWidth` właściwości, należy go przechowywać w słowniku wartość o podwójnej precyzji. XAML wygodnie definiuje znaczniki dla typowych danych, takich jak `x:Double` i `x:Int32`:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

        <x:Double x:Key="borderWidth">
            3
        </x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

Nie trzeba umieścić go na trzy wiersze. Ten wpis słownika dla tego kąt obrotu przyjmuje tylko jeden wiersz w górę:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

         <x:Double x:Key="borderWidth">
            3
         </x:Double>

        <x:Double x:Key="rotationAngle">-15</x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

Te dwa zasoby może być przywoływany w taki sam sposób jak `LayoutOptions` wartości:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

Dla zasobów typu `Color`, można użyć tego samego reprezentacji ciągu używanych podczas bezpośrednie przypisywanie atrybutów tych typów. Typy konwerterów są wywoływane po utworzeniu zasobu. Oto zasobu typu `Color`:

```xaml
<Color x:Key="textColor">Red</Color>
```

Często programy zestaw `FontSize` właściwości do elementu członkowskiego `NamedSize` wyliczenia, takich jak `Large`. `FontSizeConverter` Działa w tle, aby przekonwertować go na wartość zależny od platformy przy użyciu klasy `Device.GetNamedSized` metody. Jednak podczas definiowania zasobów rozmiar czcionki, warto więcej używać wartości liczbowe wyświetlane jako `x:Double` typu:

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

Teraz wszystkie właściwości z wyjątkiem `Text` są definiowane przez ustawienia zasobów:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

Istnieje również możliwość użycia `OnPlatform` w słowniku zasobów, aby określić różne wartości dla platformy. Oto jak `OnPlatform` obiekt może być częścią słownik zasobów dla różnych kolorów:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

Zwróć uwagę, że `OnPlatform` pobiera zarówno `x:Key` atrybutu, ponieważ jest to obiekt w słowniku i `x:TypeArguments` atrybutu, ponieważ jest on klasą szablonową. `iOS`, `Android`, I `UWP` atrybuty są konwertowane na `Color` wartości po zainicjowaniu obiektu.

Oto końcowego pełny plik XAML z trzy przyciski uzyskiwanie dostępu do udostępnionych wartości sześciu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />

            <x:Double x:Key="borderWidth">3</x:Double>

            <x:Double x:Key="rotationAngle">-15</x:Double>

            <OnPlatform x:Key="textColor"
                        x:TypeArguments="Color">
                <On Platform="iOS" Value="Red" />
                <On Platform="Android" Value="Aqua" />
                <On Platform="UWP" Value="#80FF80" />
            </OnPlatform>

            <x:String x:Key="fontSize">Large</x:String>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do that!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do the other thing!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

    </StackLayout>
</ContentPage>
```

Zrzuty ekranu Sprawdź stylów spójne i style zależny od platformy:

[![](xaml-markup-extensions-images/sharedresources.png "Kontrolek")](xaml-markup-extensions-images/sharedresources-large.png#lightbox "kontrolek")

Chociaż przeważnie do definiowania `Resources` kolekcji w górnej części strony, należy pamiętać, że `Resources` właściwość jest zdefiniowana przez `VisualElement`, i może zawierać `Resources` kolekcje na inne elementy na stronie. Na przykład, dodaj je do `StackLayout` w tym przykładzie:

```xaml
<StackLayout>
    <StackLayout.Resources>
        <ResourceDictionary>
            <Color x:Key="textColor">Blue</Color>
        </ResourceDictionary>
    </StackLayout.Resources>
    ...
</StackLayout>
```

Dowiesz się, kolor tekstu przycisków jest teraz niebieski. Zasadniczo, gdy analizator XAML napotka `StaticResource` — rozszerzenie znaczników, wyszukuje w górę drzewa wizualnego i używa pierwszego `ResourceDictionary` napotkania zawierające tego klucza.

Jednym z najbardziej typowych obiekty przechowywane w słownikach zasobów jest platformy Xamarin.Forms `Style`, który definiuje kolekcję ustawień właściwości. Style zostały omówione w artykule [style](~/xamarin-forms/user-interface/styles/index.md).

Czasami deweloperzy nowego w języku XAML zastanawiasz się one można umieścić element wizualny takich jak `Label` lub `Button` w `ResourceDictionary`. Chociaż z pewnością możliwe jest, go nie ma sensu wiele. Celem `ResourceDictionary` jest udostępnienie obiektów. Nie można udostępnić elementu wizualnego. To samo wystąpienie nie może występować dwa razy na jednej stronie.

## <a name="the-xstatic-markup-extension"></a>X: Static — rozszerzenie znaczników

Pomimo podobieństwa ich nazw `x:Static` i `StaticResource` bardzo różnią się. `StaticResource` Zwraca obiekt ze słownika zasobów podczas `x:Static` uzyskuje dostęp do jednej z następujących czynności:

- statyczne pole publiczne
- publiczna właściwość statyczna
- publiczne pola stałej 
- elementu członkowskiego wyliczenia. 

`StaticResource` — Rozszerzenie znaczników jest obsługiwany przez implementacje XAML, które definiują słownik zasobów, podczas gdy `x:Static` jest wewnętrzna część XAML, co `x` prefiksu prezentuje.

Oto kilka przykładów, które pokazują, jak `x:Static` można jawnie odwoływać się pola statyczne i elementy członkowskie wyliczenia:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

Do tej pory nie jest to bardzo ogromnych możliwości. Ale `x:Static` — rozszerzenie znaczników można także odwoływać statycznego pola lub właściwości, z własnego kodu. Na przykład, w tym miejscu jest `AppConstants` klasę, która zawiera niektórych pól statycznych, które możesz chcieć użyć na wielu stronach w aplikacji:

```csharp
using System;
using Xamarin.Forms;

namespace XamlSamples
{
    static class AppConstants
    {
        public static readonly Thickness PagePadding;

        public static readonly Font TitleFont;

        public static readonly Color BackgroundColor = Color.Aqua;

        public static readonly Color ForegroundColor = Color.Brown;

        static AppConstants()
        {
            switch (Device.RuntimePlatform)
            {
                case Device.iOS:
                    PagePadding = new Thickness(5, 20, 5, 0);
                    TitleFont = Font.SystemFontOfSize(35, FontAttributes.Bold);
                    break;

                case Device.Android:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(40, FontAttributes.Bold);
                    break;
                    
                case Device.UWP:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(50, FontAttributes.Bold);
                    break;
            }
        }
    }
}
```

Aby odwołać pola statyczne tej klasy w pliku XAML, konieczne będzie jakiś sposób, aby wskazać, w pliku XAML, w którym znajduje się ten plik. W tym z deklaracji przestrzeni nazw XML.

Odwołaj się, że pliki XAML utworzona w ramach standardowego szablonu platformy Xamarin.Forms XAML zawiera dwa deklaracje przestrzeni nazw XML: jeden do uzyskiwania dostępu do klasy platformy Xamarin.Forms i drugi dla odwołania do tagów i atrybutów wewnętrzne w języku XAML:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

Będziesz potrzebować dodatkowe deklaracje przestrzeni nazw XML, dostęp do innych klas. Każdy dodatkowego deklaracji przestrzeni nazw XML definiuje nowy prefiks. Dostęp do klas lokalne dla udostępnionych aplikacji PCL, takich jak `AppConstants`, XAML programistów często używają prefiks `local`. Deklaracja przestrzeni nazw musi wskazywać nazwę przestrzeni nazw CLR (środowisko uruchomieniowe języka wspólnego), znany także jako nazwa przestrzeni nazw .NET, którego nazwa jest wyświetlana w języku C# `namespace` definicji lub `using` dyrektywy:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

Można również zdefiniować deklaracji przestrzeni nazw XML dla przestrzeni nazw .NET w zestawu, który odwołuje się do PCL. Na przykład, w tym miejscu jest `sys` prefiks dla platformy .NET standard `System` przestrzeni nazw, który znajduje się w **mscorlib** zestawu raz umieszczenia "Wspólnej obiekt biblioteki wykonawczej Microsoft", ale teraz oznacza "wersje językowe standardowe Typowe obiektu Biblioteka środowiska uruchomieniowego." Ponieważ jest to inny zestaw, należy również określić nazwę zestawu, w tym przypadku **mscorlib**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Należy zauważyć, że słowo kluczowe `clr-namespace` następuje dwukropek, a następnie nazwę obszaru nazw .NET, a następnie średnikami, słowo kluczowe `assembly`, znak równości i nazwy zestawu.

Tak, następuje dwukropek `clr-namespace` , ale następuje znak równości `assembly`. Składnia została zdefiniowana w ten sposób celowo: deklaracje przestrzeni nazw XML najbardziej odwoływać się identyfikator URI, który rozpoczyna się nazwa schematu URI, takich jak `http`, który jest zawsze z dwukropkiem. `clr-namespace` Część ten ciąg jest przeznaczona do naśladować Konwencji.

Obie te deklaracje przestrzeni nazw znajdują się w **StaticConstantsPage** próbki. Zwróć uwagę, że `BoxView` wymiary są ustawione na `Math.PI` i `Math.E`, ale skalowana przez współczynnik 100:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.StaticConstantsPage"
             Title="Static Constants Page"
             Padding="{x:Static local:AppConstants.PagePadding}">

    <StackLayout>
       <Label Text="Hello, XAML!"
              TextColor="{x:Static local:AppConstants.BackgroundColor}"
              BackgroundColor="{x:Static local:AppConstants.ForegroundColor}"
              Font="{x:Static local:AppConstants.TitleFont}"
              HorizontalOptions="Center" />

      <BoxView WidthRequest="{x:Static sys:Math.PI}"
               HeightRequest="{x:Static sys:Math.E}"
               Color="{x:Static local:AppConstants.ForegroundColor}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="100" />
    </StackLayout>
</ContentPage>
```

Rozmiar wynikowe `BoxView` względem ekranu jest zależny od platformy:

 [![](xaml-markup-extensions-images/staticconstants.png "Formanty przy użyciu x: Static — rozszerzenie znaczników")](xaml-markup-extensions-images/staticconstants-large.png#lightbox "formantów za pomocą x: Static — rozszerzenie znaczników")

## <a name="other-standard-markup-extensions"></a>Rozszerzenia innych standardowych znaczników

Kilka rozszerzeń znaczników są wewnętrzne w języku XAML i obsługiwane w plikach XAML platformy Xamarin.Forms. Niektóre z nich nie są często używane, ale są niezbędne, gdy są potrzebne:

-  Jeśli właściwość jest niż `null` chcesz ustawić ją na wartość domyślną, ale `null`, ustaw ją na `{x:Null}` — rozszerzenie znaczników.
-  Jeśli właściwość jest typu `Type`, można je przypisać do `Type` przy użyciu rozszerzenia znacznika `{x:Type someClass}`.
-  Tablice można zdefiniować przy użyciu języka XAML `x:Array` — rozszerzenie znaczników. Tego rozszerzenia znacznika ma wymaganego atrybutu o nazwie `Type` wskazujące typ elementów w tablicy.
- `Binding` — Rozszerzenie znaczników została szczegółowo opisana w [część 4. Podstawowe informacje o powiązaniu danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="the-constraintexpression-markup-extension"></a>Rozszerzenie znaczników ConstraintExpression

Rozszerzenia znaczników może mieć właściwości, ale nie są ustawione takie jak atrybutów XML. W rozszerzeniu znaczników ustawienia właściwości są oddzielone przecinkami, a znaki cudzysłowu są wyświetlane w nawiasach klamrowych.

Może być to zilustrowane z rozszerzeniem znacznika platformy Xamarin.Forms o nazwie `ConstraintExpression`, który jest używany z `RelativeLayout` klasy. Można określić lokalizacji ani rozmiaru widok podrzędny, jako stałej lub względem elementu nadrzędnego lub w innym widoku o nazwie. Składnia `ConstraintExpression` pozwala ustawić pozycji i rozmiaru widoku przy użyciu `Factor` razy właściwość innego widoku, a także a `Constant`. Bardziej złożone niż niczego wymaga kodu.

Oto przykład:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.RelativeLayoutPage"
             Title="RelativeLayout Page">

    <RelativeLayout>

        <!-- Upper left -->
        <BoxView Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Upper right -->
        <BoxView Color="Green"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Lower left -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />
        <!-- Lower right -->
        <BoxView Color="Yellow"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />

        <!-- Centered and 1/3 width and height of parent -->
        <BoxView x:Name="oneThird"
                 Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"  />

        <!-- 1/3 width and height of previous -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=X}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Y}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Height,
                                            Factor=0.33}"  />
    </RelativeLayout>
</ContentPage>
```

Prawdopodobnie najważniejszych lekcja powinno zająć od tego przykładu jest składnia rozszerzenia znacznika: znaki cudzysłowu musi występować w nawiasy klamrowe rozszerzenia znacznika. Podczas wpisywania rozszerzenia znacznika w pliku XAML, jest naturalna do wartości właściwości, należy ująć w cudzysłów. Oprzeć możliwość przesłania!

W tym miejscu jest uruchomiony program:

[![](xaml-markup-extensions-images/relativelayout.png "Względne układu za pomocą ograniczenia")](xaml-markup-extensions-images/relativelayout-large.png#lightbox "względną układu za pomocą ograniczenia")

## <a name="summary"></a>Podsumowanie

Rozszerzenia znaczników XAML pokazane zapewniają obsługę ważnych plików XAML. Ale być może jest najbardziej przydatna rozszerzenie znaczników w XAML `Binding`, która została opisana w następnej części tej serii [część 4. Podstawowe informacje o powiązaniu danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).



## <a name="related-links"></a>Linki pokrewne

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Part 1. Część 1. Wprowadzenie do języka XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Part 2. Część 2. Podstawowa składnia języka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Part 4. Część 4. Powiązania danych — podstawy](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Part 5. Z danych powiązanie z modelem MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
