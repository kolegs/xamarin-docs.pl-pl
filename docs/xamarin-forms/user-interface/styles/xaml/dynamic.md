---
title: Style dynamiczne
description: Style nie odpowiadają na zmiany właściwości i pozostają niezmienione na czas trwania aplikacji. Na przykład po przypisaniu stylu elementu wizualnego, jeśli jedno wystąpienie metody ustawiającej zmodyfikowanych, usuniętych lub nowe wystąpienie metody ustawiającej dodane, zmiany nie będzie stosowane do elementu wizualnego. Jednak aplikacje może odpowiadać na zmiany stylu dynamicznie w czasie wykonywania za pomocą dynamicznej zasobów.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: c484bdc90ec039a8d70209deabbe283cf7100610
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="dynamic-styles"></a>Style dynamiczne

_Style nie odpowiadają na zmiany właściwości i pozostają niezmienione na czas trwania aplikacji. Na przykład po przypisaniu stylu elementu wizualnego, jeśli jedno wystąpienie metody ustawiającej zmodyfikowanych, usuniętych lub nowe wystąpienie metody ustawiającej dodane, zmiany nie będzie stosowane do elementu wizualnego. Jednak aplikacje może odpowiadać na zmiany stylu dynamicznie w czasie wykonywania za pomocą dynamicznej zasobów._

`DynamicResource` — Rozszerzenie znaczników jest podobny do `StaticResource` — rozszerzenie znaczników w obu użycia klucza słownika można pobrać wartości z [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Jednakże podczas `StaticResource` przeprowadza wyszukiwanie jednego słownika `DynamicResource` z łączem do klucza słownika. W związku z tym jeśli wpis słownika skojarzoną z kluczem zostanie zastąpiony, zmiana zostanie zastosowana do elementu wizualnego. Dzięki temu zmiany stylu środowiska uruchomieniowego w aplikacji.

Poniższy przykład kodu pokazuje *dynamiczne* style w strony XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle"
                   TargetType="SearchBar"
                   BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle"
                   TargetType="SearchBar">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Placeholder="These SearchBar controls"
                       Style="{DynamicResource searchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Wystąpienia użyj `DynamicResource` — rozszerzenie znaczników odwołanie do [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) o nazwie `searchBarStyle`, który nie jest zdefiniowany w języku XAML. Jednak ponieważ [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości `SearchBar` wystąpienia są ustawiane przy użyciu `DynamicResource`, Brak klucza słownika nie zwróci wyjątek.

Zamiast tego w pliku CodeBehind konstruktora tworzy [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) wpisu z kluczem `searchBarStyle`, jak pokazano w poniższym przykładzie:

```csharp
public partial class DynamicStylesPage : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPage ()
    {
        InitializeComponent ();
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
    }

    void OnButtonClicked (object sender, EventArgs e)
    {
        if (originalStyle) {
            Resources ["searchBarStyle"] = Resources ["greenSearchBarStyle"];
            originalStyle = false;
        } else {
            Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
            originalStyle = true;
        }
    }
}
```

Gdy `OnButtonClicked` program obsługi zdarzeń jest wykonywane, `searchBarStyle` nastąpi przełączenie między `blueSearchBarStyle` i `greenSearchBarStyle`. Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

[![](dynamic-images/dynamic-style-blue.png "Niebieski dynamiczne stylu przykładu")](dynamic-images/dynamic-style-blue-large.png#lightbox "niebieski dynamiczne stylu przykładu")
[![](dynamic-images/dynamic-style-green.png "Green dynamiczne stylu przykładu") ] (dynamic-images/dynamic-style-green-large.png#lightbox "Green dynamiczne stylu przykładu")

Poniższy przykład kodu pokazuje odpowiedniej strony w języku C#:

```csharp
public class DynamicStylesPageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        ...
        var searchBar1 = new SearchBar { Placeholder = "These SearchBar controls" };
        searchBar1.SetDynamicResource (VisualElement.StyleProperty, "searchBarStyle");
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button  }
        };
    }
    ...
}
```

W języku C# [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) wystąpienia użyj [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) metodę, aby odwołać `searchBarStyle`. `OnButtonClicked` Kod obsługi zdarzenia jest taki sam, na przykład XAML, a po wykonaniu `searchBarStyle` nastąpi przełączenie między `blueSearchBarStyle` i `greenSearchBarStyle`.

<a name="dynamic-style-inheritance">

## <a name="dynamic-style-inheritance"></a>Styl dynamiczne dziedziczenia

Pochodny stylu dynamiczne stylu nie może zostać osiągnięty przy użyciu [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) właściwości. Zamiast tego [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) klasa zawiera [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) właściwość, która może mieć wartości klucza słownika którego wartość może dynamicznie zmieniać.

Poniższy przykład kodu pokazuje *dynamiczne* styl dziedziczenia w strony XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle" TargetType="SearchBar" BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle" TargetType="SearchBar">
              ...
            </Style>
            <Style x:Key="tealSearchBarStyle" TargetType="SearchBar" BaseResourceKey="searchBarStyle">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Text="These SearchBar controls" Style="{StaticResource tealSearchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Wystąpienia użyj `StaticResource` — rozszerzenie znaczników odwołanie do [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) o nazwie `tealSearchBarStyle`. To `Style` ustawia niektóre dodatkowe właściwości i używa [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) właściwości odwołanie do `searchBarStyle`. `DynamicResource` — Rozszerzenie znaczników nie jest wymagana, ponieważ `tealSearchBarStyle` nie zmieni się, z wyjątkiem `Style` dziedziczy. W związku z tym `tealSearchBarStyle` z łączem do `searchBarStyle` i ulega zmianie po zmianie stylu bazowego.

W pliku CodeBehind tworzy konstruktora [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) wpisu z kluczem `searchBarStyle`, jak na poprzednim przykładzie wykazały, dynamiczne style. Gdy `OnButtonClicked` program obsługi zdarzeń jest wykonywane, `searchBarStyle` nastąpi przełączenie między `blueSearchBarStyle` i `greenSearchBarStyle`. Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

[![](dynamic-images/dynamic-style-inheritance-blue.png "Niebieski dynamiczne stylu dziedziczenia przykładu")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "niebieski dynamiczne stylu dziedziczenia przykładu")
[![](dynamic-images/dynamic-style-inheritance-green.png "Green styl dynamiczne Przykład dziedziczenia")](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox "Green przykład dziedziczenia styl dynamiczne")

Poniższy przykład kodu pokazuje odpowiedniej strony w języku C#:

```csharp
public class DynamicStylesInheritancePageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesInheritancePageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var tealSearchBarStyle = new Style (typeof(SearchBar)) {
            BaseResourceKey = "searchBarStyle",
            ...
        };
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = {
                new SearchBar { Text = "These SearchBar controls", Style = tealSearchBarStyle },
                ...
            }
        };
    }
    ...
}
```

`tealSearchBarStyle` Przypisano bezpośrednio do [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwość [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) wystąpień. To `Style` ustawia niektóre dodatkowe właściwości, a następnie używa [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) właściwości odwołanie do `searchBarStyle`. [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) — Metoda nie jest wymagane w tym miejscu ponieważ `tealSearchBarStyle` nie zmieni się, z wyjątkiem `Style` dziedziczy. W związku z tym `tealSearchBarStyle` z łączem do `searchBarStyle` i ulega zmianie po zmianie stylu bazowego.

## <a name="summary"></a>Podsumowanie

Style nie odpowiadają na zmiany właściwości i pozostają niezmienione na czas trwania aplikacji. Jednak aplikacje może odpowiadać na zmiany stylu dynamicznie w czasie wykonywania za pomocą dynamicznej zasobów. Ponadto *dynamiczne* style mogą pochodzić z z [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) właściwości.



## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style dynamiczna (na przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Praca z style (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metody ustawiającej](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
