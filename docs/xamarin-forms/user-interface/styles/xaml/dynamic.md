---
title: Style dynamiczne w interfejsie Xamarin.Forms
description: W tym artykule wyjaśniono, jak aplikacja Xamarin.Forms można odpowiedzieć zmian stylów dynamicznie w czasie wykonywania przy użyciu dynamicznych zasobów.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: cedf9e3daed9a2d5f8bfa0962bf66510748b592a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997149"
---
# <a name="dynamic-styles-in-xamarinforms"></a>Style dynamiczne w interfejsie Xamarin.Forms

_Style nie reagować na zmiany właściwości i pozostają bez zmian w czasie trwania operacji aplikacji. Na przykład po przypisaniu stylu element wizualny, jeśli jedno wystąpienie metody ustawiającej zmodyfikowanych, usuniętych lub nowe wystąpienie metody ustawiającej dodane, zmiany nie zostanie zastosowany do elementu wizualnego. Jednak aplikacje może reagować na zmiany wprowadzone w stylu dynamicznie w czasie wykonywania przy użyciu dynamicznych zasobów._

`DynamicResource` — Rozszerzenie znaczników jest podobny do `StaticResource` — rozszerzenie znaczników w obu użyć klucza słownika można pobrać wartość z zakresu od [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Jednakże podczas `StaticResource` przeprowadza wyszukiwanie pojedynczego słownika, `DynamicResource` z łączem do klucz ze słownika. W związku z tym jeśli wpis skojarzoną z kluczem w słowniku zostanie zastąpiony, zmiana zostanie zastosowana do elementu wizualnego. Dzięki temu zmiany stylów środowiska uruchomieniowego ma zostać wykonane w aplikacji.

Poniższy przykład kodu demonstruje *dynamiczne* style na stronie XAML:

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

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar) Wystąpień użyj `DynamicResource` — rozszerzenie znaczników do odwołania [ `Style` ](xref:Xamarin.Forms.Style) o nazwie `searchBarStyle`, który nie jest zdefiniowany w XAML. Jednak ponieważ [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości `SearchBar` wystąpienia są ustawiane przy użyciu `DynamicResource`, Brak klucza słownika nie powoduje wyjątek.

Zamiast tego w pliku związanym z kodem, Konstruktor tworzy [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) wpis z kluczem `searchBarStyle`, jak pokazano w poniższym przykładzie kodu:

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

Gdy `OnButtonClicked` programu obsługi zdarzeń jest wykonywane, `searchBarStyle` będą przełączać się między `blueSearchBarStyle` i `greenSearchBarStyle`. Skutkuje to wygląd pokazano na poniższych zrzutach ekranu:

[![](dynamic-images/dynamic-style-blue.png "Niebieski dynamiczne stylu przykładu")](dynamic-images/dynamic-style-blue-large.png#lightbox "niebieski dynamiczne stylu przykładu")
[![](dynamic-images/dynamic-style-green.png "zielony dynamiczne stylu przykładu") ] (dynamic-images/dynamic-style-green-large.png#lightbox "Zielony przykład Style dynamiczne")

Poniższy przykład kodu demonstruje odpowiednich stron w języku C#:

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
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button    }
        };
    }
    ...
}
```

W języku C# [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) wystąpień użyj [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource*) metodę, aby odwoływać się do `searchBarStyle`. `OnButtonClicked` Kod procedury obsługi zdarzeń jest taka sama, jak w przykładzie XAML i po wykonaniu `searchBarStyle` będą przełączać się między `blueSearchBarStyle` i `greenSearchBarStyle`.

<a name="dynamic-style-inheritance">

## <a name="dynamic-style-inheritance"></a>Dziedziczenie stylów dynamiczne

Wyprowadzanie styl z dynamicznych styl nie można osiągnąć przy użyciu [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) właściwości. Zamiast tego [ `Style` ](xref:Xamarin.Forms.Style) klasa zawiera [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) właściwość, która może być ustawiona na klucz ze słownika o wartości dynamiczne mogą ulec zmianie.

Poniższy przykład kodu demonstruje *dynamiczne* styl dziedziczenia w strony XAML:

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

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar) Wystąpień użyj `StaticResource` — rozszerzenie znaczników k odkazu [ `Style` ](xref:Xamarin.Forms.Style) o nazwie `tealSearchBarStyle`. To `Style` ustawia niektóre dodatkowe właściwości i używa [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) właściwości w celu odwołania `searchBarStyle`. `DynamicResource` — Rozszerzenie znaczników nie jest wymagana, ponieważ `tealSearchBarStyle` nie ulegną zmianie, z wyjątkiem `Style` pochodzi od klasy. W związku z tym `tealSearchBarStyle` z łączem do `searchBarStyle` i ulega zmianie po zmianie stylu podstawowym.

W pliku związanym z kodem, tworzy konstruktora [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) wpis z kluczem `searchBarStyle`, ponieważ na poprzednim przykładzie, który pokazano style dynamiczne. Gdy `OnButtonClicked` programu obsługi zdarzeń jest wykonywane, `searchBarStyle` będą przełączać się między `blueSearchBarStyle` i `greenSearchBarStyle`. Skutkuje to wygląd pokazano na poniższych zrzutach ekranu:

[![](dynamic-images/dynamic-style-inheritance-blue.png "Niebieski przykład dziedziczenia Style dynamiczne")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "niebieski przykład dziedziczenia Style dynamiczne")
[![](dynamic-images/dynamic-style-inheritance-green.png "zielony Style dynamiczne Przykład dziedziczenia")](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox "zielony przykład dziedziczenia Style dynamiczne")

Poniższy przykład kodu demonstruje odpowiednich stron w języku C#:

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

`tealSearchBarStyle` Przypisano bezpośrednio do [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwość [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) wystąpień. To `Style` ustawia niektóre dodatkowe właściwości, a następnie używa [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) właściwości w celu odwołania `searchBarStyle`. [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource*) Metoda nie jest wymagane w tym miejscu ponieważ `tealSearchBarStyle` nie ulegną zmianie, z wyjątkiem `Style` pochodzi od klasy. W związku z tym `tealSearchBarStyle` z łączem do `searchBarStyle` i ulega zmianie po zmianie stylu podstawowym.

## <a name="summary"></a>Podsumowanie

Style nie reagować na zmiany właściwości i pozostają bez zmian w czasie trwania operacji aplikacji. Jednak aplikacje może reagować na zmiany wprowadzone w stylu dynamicznie w czasie wykonywania przy użyciu dynamicznych zasobów. Ponadto *dynamiczne* style mogą pochodzić z z [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) właściwości.



## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style dynamiczne (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Praca ze stylami (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metody ustawiającej](xref:Xamarin.Forms.Setter)
