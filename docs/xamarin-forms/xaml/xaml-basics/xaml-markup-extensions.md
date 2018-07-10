---
title: Część 3. Rozszerzenia znaczników w XAML
description: Rozszerzeń struktury znaczników XAML stanowi ważną funkcję w XAML, który umożliwia właściwości można ustawić do obiektów lub wartości, do których odwołują się bezpośrednio z innych źródeł.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: charlespetzold
ms.author: chape
ms.date: 3/27/2018
ms.openlocfilehash: 6fcb051d2c24c7da169106b06ad5ebfc91edafa6
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935619"
---
# <a name="part-3-xaml-markup-extensions"></a>Część 3. Rozszerzenia znaczników w XAML

_Rozszerzeń struktury znaczników XAML stanowi ważną funkcję w XAML, który umożliwia właściwości można ustawić do obiektów lub wartości, do których odwołują się bezpośrednio z innych źródeł. Rozszerzeń struktury znaczników XAML są szczególnie ważne w przypadku udostępniania obiektów i odwoływanie się do stałych używanych w całej aplikacji, ale mogą znaleźć ich najlepsze narzędzia w powiązań danych._

## <a name="xaml-markup-extensions"></a>Rozszerzenia znaczników w XAML

Ogólnie rzecz biorąc umożliwia XAML właściwości obiektu jawnie ustawione wartości, takich jak ciąg, liczba lub ciąg, który jest konwertowany na wartość w tle elementu członkowskiego wyliczenia.

Czasami jednak właściwości zamiast musi odwoływać się wartości zdefiniowanych gdzieś else lub co może wymagać nieco przetwarzania przez kod w czasie wykonywania. Do tych celów, XAML *— rozszerzenia znaczników* są dostępne.

Tych rozszerzeń struktury znaczników XAML nie są rozszerzenia XML. XAML jest całkowicie prawne XML. Są nazywane "rozszerzeniami", ponieważ są one wspierane przez kod w klasach, które implementują `IMarkupExtension`. Można napisać własne rozszerzenia niestandardowych znaczników.

W wielu przypadkach rozpoznawalny w plikach XAML są rozszerzeń struktury znaczników XAML, ponieważ są wyświetlane zgodnie z ustawieniami atrybut rozdzielone nawiasów klamrowych: {i}, ale czasami rozszerzenia znaczników, są wyświetlane w znacznikach jako konwencjonalne elementy.

## <a name="shared-resources"></a>Udostępnione zasoby

Niektóre strony XAML zawiera kilka widoków z właściwościami, te same wartości. Na przykład wiele ustawień właściwości dla tych `Button` obiekty są takie same:

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

Jedną z tych właściwości musi zostać zmieniona, warto wprowadzić zmiany tylko raz, a nie trzy razy. Gdyby ten kod, będą prawdopodobnie stosowane stałe i statycznych obiektów tylko do odczytu do zapewnienia spójnego i można z łatwością zmodyfikować takich wartości.

W XAML, jednym z popularnych rozwiązań jest do przechowywania tych wartości lub obiekty w *słownik zasobów*. `VisualElement` Klasa definiuje właściwość o nazwie `Resources` typu `ResourceDictionary`, czyli słownika przy użyciu kluczy typu `string` i wartości typu `object`. Można umieścić obiekty w tym słowniku, a następnie odwoływać się do nich z kodu znaczników w XAML.

Aby użyć słownika zasobów, na stronie, Dołącz parę `Resources` tagi element właściwości. Najwygodniej umieścić je w górnej części strony jest:

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

Jest również jawnie dołączyć znak `ResourceDictionary` tagi:

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

Wartości różnych typów obiektów i może być dodany do słownika zasobów. Te typy muszą być tworzone jako wystąpienia. Klasy abstrakcyjne, nie można na przykład. Te typy muszą również mieć publicznego konstruktora bez parametrów. Każdy element wymaga klucza słownika określony za pomocą `x:Key` atrybutu. Na przykład:

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

Te dwa elementy są wartości typu struktury `LayoutOptions`i każdy z nich ma unikatowy klucz i jednego lub dwóch właściwości ustawione. W kodzie i znaczników jest znacznie bardziej powszechne, aby użyć statycznego pola elementu `LayoutOptions`, ale poniżej przedstawiono bardziej wygodne do ustawiania właściwości.

Teraz ustaw `HorizontalOptions` i `VerticalOptions` właściwości tych przycisków do tych zasobów, a zostanie to zrobione za pomocą `StaticResource` — rozszerzenie znaczników XAML:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

`StaticResource` — Rozszerzenie znaczników zawsze była rozdzielana za pomocą nawiasów klamrowych i zawiera klucz ze słownika.

Nazwa `StaticResource` odróżnia go od `DynamicResource`, który obsługuje również zestaw narzędzi Xamarin.Forms. `DynamicResource` dla kluczy słownika związanych z wartościami, które mogą ulec zmianie w czasie wykonywania, podczas gdy `StaticResource` uzyskuje dostęp do elementów ze słownika tylko raz, kiedy są konstruowane elementów na stronie.

Aby uzyskać `BorderWidth` właściwość, należy go przechowywać wartość o podwójnej precyzji w słowniku. XAML wygodnie definiuje znaczniki dla popularnych typów danych, takich jak `x:Double` i `x:Int32`:

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

Te dwa zasoby mogą być przywoływane w taki sam sposób jak `LayoutOptions` wartości:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

Dla zasobów typu `Color`, można użyć tego samego ciągów reprezentujących, których używasz, przypisując bezpośrednio atrybuty z tych typów. Typy konwerterów są wywoływane, gdy zasób jest tworzony. Oto zasób typu `Color`:

```xaml
<Color x:Key="textColor">Red</Color>
```

Często programy zestaw `FontSize` właściwości do elementu członkowskiego `NamedSize` wyliczenie, takie jak `Large`. `FontSizeConverter` Działa w tle, aby przekonwertować go na wartość zależny od platformy za pomocą klasy `Device.GetNamedSized` metody. Jednak podczas definiowania zasobów rozmiar czcionki, więcej sensu użycie wartości liczbowych, pokazano tutaj jako `x:Double` typu:

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

Istnieje również możliwość użycia `OnPlatform` w ciągu słownika zasobów, aby zdefiniować różne wartości dla platform. Oto jak `OnPlatform` obiekt może być częścią słownik zasobów dla różnych kolorów:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

Należy zauważyć, że `OnPlatform` pobiera zarówno `x:Key` atrybutu, ponieważ jest to obiekt w słowniku i `x:TypeArguments` atrybutu, ponieważ jest on klasą rodzajową. `iOS`, `Android`, I `UWP` atrybuty są konwertowane na `Color` wartości, gdy obiekt jest zainicjowany.

Poniżej przedstawiono końcowy kompletny plik XAML z trzy przyciski, uzyskiwanie dostępu do sześciu udostępnionych wartości:

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

Zrzuty ekranu Sprawdź stylu spójne i stylu zależny od platformy:

[![](xaml-markup-extensions-images/sharedresources.png "Kontrolek")](xaml-markup-extensions-images/sharedresources-large.png#lightbox "kontrolek")

Chociaż przeważnie do definiowania `Resources` kolekcji w górnej części strony, należy pamiętać, który `Resources` właściwość jest zdefiniowana przez `VisualElement`, i może mieć `Resources` kolekcje w przypadku innych elementów na stronie. Na przykład spróbuj dodać je do `StackLayout` w tym przykładzie:

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

Dowiesz się, że kolor tekstu, przycisków jest teraz niebieski. Zasadniczo, zawsze, gdy XAML analizator składni napotka `StaticResource` — rozszerzenie znaczników, wyszukuje w górę drzewa wizualnego i używa pierwszego `ResourceDictionary` napotka zawierające ten klucz.

Jednym z najbardziej powszechne typy obiektów przechowywanych w słownikach zasobów jest Xamarin.Forms `Style`, która definiuje zbiór ustawień właściwości. Style są omówione w artykule [style](~/xamarin-forms/user-interface/styles/index.md).

Czasami deweloperów jesteś nowym użytkownikiem XAML zastanawiasz się, jeśli są takie jak umieścić element wizualny `Label` lub `Button` w `ResourceDictionary`. Choć jest możliwe z pewnością, nie ma sensu wiele. Celem `ResourceDictionary` jest udostępnienie obiektów. Nie można współużytkować elementu wizualnego. To samo wystąpienie nie może pojawić się dwa razy na jednej stronie.

## <a name="the-xstatic-markup-extension"></a>X: Static — rozszerzenie znaczników

Pomimo podobieństw ich nazw `x:Static` i `StaticResource` bardzo różnią się. `StaticResource` Zwraca obiekt ze słownika zasobów podczas `x:Static` uzyskuje dostęp do jednego z następujących czynności:

- publiczne pole statyczne
- publiczna właściwość statyczna
- Pole publiczne stałe
- element członkowski wyliczenia.

`StaticResource` — Rozszerzenie znaczników jest obsługiwany przez implementacje XAML, definiujące słownika zasobów, podczas gdy `x:Static` jest wewnętrzna część XAML, jako `x` prefiksu prezentuje.

Poniżej przedstawiono kilka przykładów, które pokazują, jak `x:Static` jawnie można odwoływać się do pól statycznych i elementów członkowskich wyliczenia:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

Do tej pory nie jest to bardzo imponujące. Ale `x:Static` — rozszerzenie znaczników można także odwoływać się do właściwości lub pola statyczne z własnego kodu. Na przykład Oto `AppConstants` klasę, która zawiera niektóre pola statyczne, które możesz chcieć użyć na wielu stronach w całej aplikacji:

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

Aby odwoływać się do pola statyczne tej klasy w pliku XAML, konieczne będzie jakiś sposób, aby wskazać, w pliku XAML, w którym znajduje się ten plik. Można to zrobić za pomocą deklaracji przestrzeni nazw XML.

Pamiętaj, że pliki XAML, utworzone w ramach standardowego szablonu XAML zestawu narzędzi Xamarin.Forms zawierają dwie deklaracje przestrzeni nazw XML: jeden do uzyskiwania dostępu do klasy zestawu narzędzi Xamarin.Forms i inny wpis dla odwołania do znaczniki i atrybuty nierozerwalnie związane z XAML:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

Potrzebna będzie dodatkowe deklaracje przestrzeni nazw XML, dostęp do innych klas. Każdy dodatkowy deklaracji przestrzeni nazw XML definiuje nowy prefiks. Dostęp do klas lokalnych do aplikacji udostępnionej biblioteki .NET Standard, takich jak `AppConstants`, XAML programistów często używają prefiksu `local`. Deklaracja przestrzeni nazw musi wskazywać nazwy obszaru nazw środowiska CLR (środowisko uruchomieniowe języka wspólnego), znany także jako nazwa przestrzeni nazw .NET, jest to nazwa, która pojawia się w języku C# `namespace` definicja lub obiekcie `using` dyrektywy:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

Można również zdefiniować deklaracje przestrzeni nazw XML dla przestrzeni nazw .NET w każdym zestawie, który odwołuje się do biblioteki .NET Standard. Na przykład Oto `sys` prefiks dla programu standard .NET `System` przestrzeni nazw, który znajduje się w **mscorlib** zestawu, która po umieszczenia "Wspólne biblioteki obiektów Microsoft środowiska uruchomieniowego", ale teraz oznacza "wersje językowe standardowy Typowe obiekt biblioteki wykonawczej." Ponieważ jest to innego zestawu, należy także określić nazwę zestawu, w tym przypadku **mscorlib**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Należy zauważyć, że słowa kluczowego `clr-namespace` następuje dwukropek, a następnie nazwę przestrzeni nazw .NET, a następnie średnikami, słowo kluczowe `assembly`, znak równości i nazwę zestawu.

Tak, następuje dwukropek `clr-namespace` , ale następuje po znaku równości `assembly`. Składnia został zdefiniowany w tym sposób celowo: deklaracje przestrzeni nazw XML najbardziej odwoływać się do identyfikatora URI, który rozpoczyna się nazwa schematu identyfikatora URI, takich jak `http`, który jest zawsze następuje dwukropek. `clr-namespace` Część ten ciąg jest przeznaczona do naśladowania tej Konwencji.

Oba te deklaracje przestrzeni nazw znajdują się w **StaticConstantsPage** próbki. Należy zauważyć, że `BoxView` wymiary są ustawione na `Math.PI` i `Math.E`, ale skalowanych o 100:

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

 [![](xaml-markup-extensions-images/staticconstants.png "Kontrolek przy użyciu x: Static — rozszerzenie znaczników")](xaml-markup-extensions-images/staticconstants-large.png#lightbox "kontrolek przy użyciu x: Static — rozszerzenie znaczników")

## <a name="other-standard-markup-extensions"></a>Inne rozszerzenia standardowych znaczników

Kilka rozszerzenia znaczników są nierozerwalnie związane z XAML i obsługiwane w plikach XAML zestawu narzędzi Xamarin.Forms. Niektóre z nich nie są bardzo często używane, ale są niezbędne, gdy będą potrzebne:

-  Jeśli właściwość ma innej niż `null` chcesz ustawić ją na wartość domyślną, ale `null`, ustaw ją na `{x:Null}` — rozszerzenie znaczników.
-  Jeśli właściwość jest typu `Type`, można je przypisać do `Type` przy użyciu rozszerzenia znaczników `{x:Type someClass}`.
-  Tablice można zdefiniować przy użyciu XAML `x:Array` — rozszerzenie znaczników. To rozszerzenie znaczników ma wymaganego atrybutu o nazwie `Type` oznacza typ elementów w tablicy.
- `Binding` — Rozszerzenie znaczników jest omówiona w [część 4. Podstawowe informacje dotyczące powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="the-constraintexpression-markup-extension"></a>Rozszerzenie znaczników ConstraintExpression

Rozszerzenia znaczników może mieć właściwości, ale nie są ustawione takie jak atrybuty XML. W rozszerzeniem znacznika ustawienia właściwości są oddzielone przecinkami i cudzysłowy są wyświetlane w nawiasy klamrowe.

To może można zilustrować na przykładzie rozszerzenie znaczników zestawu narzędzi Xamarin.Forms, o nazwie `ConstraintExpression`, który jest używany z `RelativeLayout` klasy. Można określić lokalizacji i rozmiaru widok podrzędny, jako stałej lub względem elementu nadrzędnego lub innych nazwany widok. Składnia `ConstraintExpression` pozwala ustawić położenie i rozmiar widoku przy użyciu `Factor` razy właściwość innego widoku, a także a `Constant`. Nic bardziej złożone niż wymaga kodu.

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

Być może najważniejszych lekcji, które powinny zająć od tego przykładu jest składni rozszerzenia znaczników: znaki cudzysłowu musi znajdować się w obrębie nawiasów klamrowych rozszerzenie znaczników. Podczas wpisywania rozszerzenie znaczników w pliku XAML, jest naturalnym do wartości właściwości, należy ująć w znaki cudzysłowu. Siłowym możliwość przesłania!

W tym miejscu jest uruchomiony program:

[![](xaml-markup-extensions-images/relativelayout.png "Względna układu za pomocą ograniczeń")](xaml-markup-extensions-images/relativelayout-large.png#lightbox "względne układu za pomocą ograniczeń")

## <a name="summary"></a>Podsumowanie

Rozszerzeń struktury znaczników XAML, które są wyświetlane w tym miejscu podaj istotne wsparcie dla plików XAML. Ale być może jest najbardziej przydatna rozszerzenie znaczników w XAML `Binding`, które zostało omówione w następnej części tej serii [część 4. Podstawowe informacje dotyczące powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).



## <a name="related-links"></a>Linki pokrewne

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Part 1. Część 1. Wprowadzenie do języka XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Part 2. Część 2. Podstawowa składnia języka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Part 4. Część 4. Powiązania danych — podstawy](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Part 5. Dane powiązanie z modelem MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
