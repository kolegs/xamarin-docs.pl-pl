---
title: Style jawne w interfejsie Xamarin.Forms
description: Style jawne to taki, który jest wybiórczo zastosować do kontrolek, ustawiając ich ponownego obliczenia właściwości stylu. W tym artykule wyjaśniono, jak używać jawnych style w aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: fba00120ed9f5c74bec7622ae1914c43533e8579
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998573"
---
# <a name="explicit-styles-in-xamarinforms"></a>Style jawne w interfejsie Xamarin.Forms

_Style jawne to taki, który jest wybiórczo zastosować do kontrolek, ustawiając ich ponownego obliczenia właściwości stylu._

## <a name="creating-an-explicit-style-in-xaml"></a>Tworzenie stylu jawne w XAML

Aby zadeklarować [ `Style` ](xref:Xamarin.Forms.Style) na poziomie strony [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) musi zostać dodany do strony i następnie co najmniej jeden `Style` deklaracje mogą być zawarte w `ResourceDictionary`. A `Style` wykonano *jawne* , zapewniając zadeklarowaniem `x:Key` atrybut, który nadaje jej opisu klucza w `ResourceDictionary`. *Jawne* następnie należy zastosować styl do określonych elementów wizualnych, ustawiając ich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości.

Poniższy kod przedstawia przykład *jawne* style zadeklarowanych w XAML na stronie `ResourceDictionary` i zastosowane do strony [ `Label` ](xref:Xamarin.Forms.Label) wystąpień:

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

[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Definiuje trzy *jawne* style, które są stosowane na stronę [ `Label` ](xref:Xamarin.Forms.Label) wystąpień. Każdy `Style` służy do wyświetlania tekstu w innym kolorze podczas ustawiania również czcionki opcje układu rozmiarze i poziomie i w pionie. Każdy `Style` jest stosowany do innego `Label` , ustawiając jego [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości za pomocą `StaticResource` — rozszerzenie znaczników. Skutkuje to wygląd pokazano na poniższych zrzutach ekranu:

[![](explicit-images/explicit-styles.png "Przykład style jawne")](explicit-images/explicit-styles-large.png#lightbox "przykład style jawne")

Ponadto końcowe [ `Label` ](xref:Xamarin.Forms.Label) ma [ `Style` ](xref:Xamarin.Forms.Style) stosowane do niego, ale również zastępuje [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) właściwości różnych `Color`wartość.

### <a name="creating-an-explicit-style-at-the-control-level"></a>Tworzenie stylu jawne na podczas kontroli

Oprócz tworzenia *jawne* style na poziomie strony, ich można również utworzyć na poziomie kontroli, jak pokazano w poniższym przykładzie kodu:

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

W tym przykładzie *jawne* [ `Style` ](xref:Xamarin.Forms.Style) wystąpienia zostaną przypisane do [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) zbiór [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) kontroli. Następnie można zastosować style formantu i jego elementów podrzędnych.

Informacje o tworzeniu style w aplikacji [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), zobacz [style globalne](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-explicit-style-in-c35"></a>Tworzenie stylu jawne w języku C&#35;

[`Style`](xref:Xamarin.Forms.Style) wystąpienia mogą być dodawane do strony [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekcji w języku C#, tworząc nowe [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), a następnie dodając `Style` wystąpień do `ResourceDictionary`, jak pokazano na Poniższy przykład kodu:

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

Konstruktor definiuje trzy *jawne* style, które są stosowane na stronę [ `Label` ](xref:Xamarin.Forms.Label) wystąpień. Każdy *jawne* [ `Style` ](xref:Xamarin.Forms.Style) jest dodawany do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) przy użyciu [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) metody, określając `key` ciągu do odwoływania się do `Style` wystąpienia. Każdy `Style` jest stosowany do innego `Label` , ustawiając ich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości.

Jednak nie ma żadnych dodatkowych zalet za pomocą [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) tutaj. Zamiast tego [ `Style` ](xref:Xamarin.Forms.Style) wystąpienia można przypisać bezpośrednio do [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości wymagane elementy wizualne i `ResourceDictionary` mogła zostać usunięta, jak pokazano w następującym przykład kodu:

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

Konstruktor definiuje trzy *jawne* style, które są stosowane na stronę [ `Label` ](xref:Xamarin.Forms.Label) wystąpień. Każdy `Style` służy do wyświetlania tekstu w innym kolorze podczas ustawiania również czcionki opcje układu rozmiarze i poziomie i w pionie. Każdy `Style` jest stosowany do innego `Label` , ustawiając jego [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości. Ponadto końcowe `Label` ma `Style` stosowane do niego, ale również zastępuje `TextColor` inną właściwość `Color` wartość.

## <a name="summary"></a>Podsumowanie

A [ `Style` ](xref:Xamarin.Forms.Style) wykonano *jawne* , zapewniając zadeklarowaniem `x:Key` atrybutu, a następnie selektywnie zastosowanie go do formantów, ustawiając ich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości.



## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Podstawowe style (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Praca ze stylami (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metody ustawiającej](xref:Xamarin.Forms.Setter)
