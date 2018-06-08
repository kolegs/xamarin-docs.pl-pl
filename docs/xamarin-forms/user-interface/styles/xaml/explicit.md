---
title: Jawne style
description: Jawne styl jest wybiórczo zastosować do formantów przez ustawienie właściwości w stylu.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 9f9e87ae0fd9d609cef56123e9052d85941bda51
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848190"
---
# <a name="explicit-styles"></a>Jawne style

_Jawne styl jest wybiórczo zastosować do formantów przez ustawienie właściwości w stylu._

## <a name="creating-an-explicit-style-in-xaml"></a>Tworzenie jawne stylu w kodzie XAML

Aby zadeklarować [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) na poziomie strony [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) musi zostać dodany do strony, a następnie jedną lub więcej `Style` deklaracje może być uwzględniony w `ResourceDictionary`. A `Style` staje się *jawne* , zapewniając jego deklaracji `x:Key` atrybut, który zapewnia jej opisową klucza w `ResourceDictionary`. *Jawne* style następnie muszą być stosowane do określonych elementów wizualnych przez ustawienie ich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości.

Poniższy kod przedstawia przykład *jawne* style zadeklarowany w języku XAML na stronie `ResourceDictionary` i zastosowane do strony [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Definiuje trzy *jawne* style, które są stosowane do strony [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień. Każdy `Style` służy do wyświetlania tekstu w innym kolorze podczas ustawiania również czcionki opcje układu rozmiarze i poziomie i w pionie. Każdy `Style` jest stosowany do innej `Label` przez ustawienie jej [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości za pomocą `StaticResource` — rozszerzenie znaczników. Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

[![](explicit-images/explicit-styles.png "Przykład jawne style")](explicit-images/explicit-styles-large.png#lightbox "przykład jawne style")

Ponadto ostatecznych [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ma [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) zastosować dla niego, ale również zastępuje [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) właściwości do różnych `Color`wartość.

### <a name="creating-an-explicit-style-at-the-control-level"></a>Tworzenie stylu jawne podczas kontroli poziomu

Oprócz tworzenia *jawne* style na poziomie strony, ich można także utworzyć na poziomie kontroli, jak pokazano w poniższym przykładzie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

W tym przykładzie *jawne* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia są przypisane do [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Kolekcja [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) formantu. Następnie można zastosować style do formantu i jego elementów podrzędnych.

Informacje o tworzeniu style w aplikacji [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), zobacz [stylów globalnych](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-explicit-style-in-c35"></a>Tworzenie stylu jawne w języku C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia mogą być dodawane do strony sieci [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekcji w języku C#, tworząc nowe [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), a następnie dodając `Style` wystąpień do `ResourceDictionary`, jak pokazano w Poniższy przykład kodu:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red    }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label {    Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

Konstruktor definiuje trzy *jawne* style, które są stosowane do strony [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień. Każdego *jawne* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) jest dodawany do [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) przy użyciu [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) metody, określając `key` ciąg do odwoływania się do `Style` wystąpienia. Każdy `Style` jest stosowany do innej `Label` przez ustawienie ich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości.

Jednak nie ma żadnych dodatkowych zalet przy użyciu [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) tutaj. Zamiast tego [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) można przypisać bezpośrednio do wystąpienia [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości wymaganych elementów wizualnych i `ResourceDictionary` mogła zostać usunięta, jak pokazano w następującym przykład kodu:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

Konstruktor definiuje trzy *jawne* style, które są stosowane do strony [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień. Każdy `Style` służy do wyświetlania tekstu w innym kolorze podczas ustawiania również czcionki opcje układu rozmiarze i poziomie i w pionie. Każdy `Style` jest stosowany do innej `Label` przez ustawienie jej [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości. Ponadto ostatecznych `Label` ma `Style` zastosować dla niego, ale również zastępuje `TextColor` właściwości z innym `Color` wartość.

## <a name="summary"></a>Podsumowanie

A [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) staje się *jawne* , zapewniając jego deklaracji `x:Key` atrybutu, a następnie selektywnie zastosowania go do formantów przez ustawienie ich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości.



## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style podstawowe (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Praca z style (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metody ustawiającej](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
