---
title: Niejawne style
description: "Niejawne styl jest taki, który jest używany przez wszystkie formanty sam element TargetType, bez konieczności styl odwołanie do każdego formantu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 0600a4ca1f26fd034679619c1427821e9c7a12b8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="implicit-styles"></a>Niejawne style

_Niejawne styl jest taki, który jest używany przez wszystkie formanty sam element TargetType, bez konieczności styl odwołanie do każdego formantu._

## <a name="creating-an-implicit-style-in-xaml"></a>Tworzenie stylu niejawne w języku XAML

Aby zadeklarować [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) na poziomie strony [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) musi zostać dodany do strony, a następnie jedną lub więcej `Style` deklaracje może być uwzględniony w `ResourceDictionary`. A `Style` staje się *niejawne* , nie określając `x:Key` atrybutu. Styl będą następnie stosowane do elementów wizualnych, które pasują `TargetType` dokładnie, ale nie dla elementów, które pochodzą od `TargetType` wartość.

Poniższy kod przedstawia przykład *niejawne* styl zadeklarowany w języku XAML na stronie `ResourceDictionary`i zastosowane do strony [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) wystąpień:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Entry">
                <Setter Property="HorizontalOptions" Value="Fill" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BackgroundColor" Value="Yellow" />
                <Setter Property="FontAttributes" Value="Italic" />
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Entry Text="These entries" />
            <Entry Text="are demonstrating" />
            <Entry Text="implicit styles," />
            <Entry Text="and an implicit style override" BackgroundColor="Lime" TextColor="Red" />
            <local:CustomEntry Text="Subclassed Entry is not receiving the style" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Definiuje jedną *niejawne* styl stosowany do strony [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) wystąpień. `Style` Służy do wyświetlania niebieski tekst na tle żółty, podczas ustawiania również inne opcje wyglądu. `Style` Zostanie dodany do strony [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) bez określania `x:Key` atrybutu. W związku z tym `Style` jest stosowane do wszystkich `Entry` niejawnie wystąpienia, ponieważ są one zgodne [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) właściwość `Style` dokładnie. Jednak `Style` nie ma zastosowania do `CustomEntry` wystąpienia, która jest podklasą `Entry`. Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

[![](implicit-images/implicit-styles.png "Przykład niejawne style")](implicit-images/implicit-styles-large.png "przykład niejawne style")

Ponadto czwarty [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) zastępuje [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) i [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) właściwości stylu niejawne do różnych `Color`wartości.

### <a name="creating-an-implicit-style-at-the-control-level"></a>Tworzenie stylu niejawne podczas kontroli poziomu

Oprócz tworzenia *niejawne* style na poziomie strony, ich można także utworzyć na poziomie kontroli, jak pokazano w poniższym przykładzie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style TargetType="Entry">
                        <Setter Property="HorizontalOptions" Value="Fill" />
                        ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Entry Text="These entries" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

W tym przykładzie *niejawne* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) jest przypisany do [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Kolekcja [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)formantu. *Niejawne* styl następnie można zastosować do formantu i jego elementów podrzędnych.

Informacje o tworzeniu style w aplikacji [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), zobacz [stylów globalnych](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-implicit-style-in-c35"></a>Tworzenie stylu niejawne w & 35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia mogą być dodawane do strony sieci [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekcji w języku C#, tworząc nowe [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), a następnie dodając `Style` wystąpień do `ResourceDictionary`, jak pokazano w Poniższy przykład kodu:

```csharp
public class ImplicitStylesPageCS : ContentPage
{
    public ImplicitStylesPageCS ()
    {
        var entryStyle = new Style (typeof(Entry)) {
            Setters = {
                ...
                new Setter { Property = Entry.TextColorProperty, Value = Color.Blue }
            }
        };

        ...
        Resources = new ResourceDictionary ();
        Resources.Add (entryStyle);

        Content = new StackLayout {
            Children = {
                new Entry { Text = "These entries" },
                new Entry { Text = "are demonstrating" },
                new Entry { Text = "implicit styles," },
                new Entry { Text = "and an implicit style override", BackgroundColor = Color.Lime, TextColor = Color.Red },
                new CustomEntry  { Text = "Subclassed Entry is not receiving the style" }
            }
        };
    }
}
```

Konstruktor definiuje jedną *niejawne* styl stosowany do strony [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) wystąpień. `Style` Służy do wyświetlania niebieski tekst na tle żółty, podczas ustawiania również inne opcje wyglądu. `Style` Zostanie dodany do strony [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) bez określania `key` ciągu. W związku z tym `Style` jest stosowane do wszystkich `Entry` niejawnie wystąpienia, ponieważ są one zgodne [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) właściwość `Style` dokładnie. Jednak `Style` nie ma zastosowania do `CustomEntry` wystąpienia, która jest podklasą `Entry`.

## <a name="summary"></a>Podsumowanie

*Niejawne* styl to taki, który jest używany przez wszystkie elementy wizualne tego samego [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), bez konieczności styl odwołanie do każdego formantu. A `Style` staje się *niejawne* , nie określając `x:Key` atrybutu. Zamiast tego `x:Key` atrybutu automatycznie stanie się wartość [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) właściwości.



## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style podstawowe (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Praca z style (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metody ustawiającej](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
