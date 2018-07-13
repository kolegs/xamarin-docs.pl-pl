---
title: Dziedziczenie stylów w interfejsie Xamarin.Forms
description: Style może dziedziczyć z innymi stylami, aby zmniejszyć dublowania i umożliwiają wielokrotne użycie. W tym artykule opisano sposób przeprowadzenia dziedziczenie stylów aplikacji Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: f8cf3287c6d713d91a0217bd30ca2ee927534aea
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995336"
---
# <a name="style-inheritance-in-xamarinforms"></a>Dziedziczenie stylów w interfejsie Xamarin.Forms

_Style może dziedziczyć z innymi stylami, aby zmniejszyć dublowania i umożliwiają wielokrotne użycie._

## <a name="style-inheritance-in-xaml"></a>Dziedziczenie stylów w XAML

Dziedziczenie stylów odbywa się przez ustawienie [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) właściwości do istniejącego [ `Style` ](xref:Xamarin.Forms.Style). W XAML, jest to osiągane przez ustawienie `BasedOn` właściwości `StaticResource` rozszerzenie znaczników, który odwołuje się do wcześniej utworzonego `Style`. W języku C# odbywa się to przez ustawienie `BasedOn` właściwość `Style` wystąpienia.

Style, które dziedziczą z stylu podstawowego mogą obejmować [ `Setter` ](xref:Xamarin.Forms.Setter) wystąpień dla nowych właściwości lub użyć ich do zastąpienia style z podstawowej stylu. Ponadto style, które dziedziczą z stylu podstawowego muszą wskazywać tego samego typu lub typ, który pochodzi od typu objęte stylu podstawowym. Na przykład, jeśli cele styl podstawowy [ `View` ](xref:Xamarin.Forms.View) wystąpień, style, które są oparte na podstawowej stylu można wskazać `View` wystąpień lub typy, które wynikają z `View` klasy, takie jak [ `Label` ](xref:Xamarin.Forms.Label) i [ `Button` ](xref:Xamarin.Forms.Button) wystąpień.

Poniższy przykład demonstruje *jawne* styl dziedziczenia w strony XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
            </Style>
            <Style x:Key="labelStyle" TargetType="Label"
                   BasedOn="{StaticResource baseStyle}">
                ...
                <Setter Property="TextColor" Value="Teal" />
            </Style>
            <Style x:Key="buttonStyle" TargetType="Button"
                   BasedOn="{StaticResource baseStyle}">
                <Setter Property="BorderColor" Value="Lime" />
                ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelStyle}" />
            ...
            <Button Text="So is the button"
                    Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`baseStyle` Cele [ `View` ](xref:Xamarin.Forms.View) wystąpień i ustawia [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) i [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwości. `baseStyle` Nie ustawiono bezpośrednio na żadną kontrolkę. Zamiast tego `labelStyle` i `buttonStyle` dziedziczyć po nim, ustawianie wartości dodatkowe właściwości możliwej do wiązania. `labelStyle` i `buttonStyle` zostaną następnie zastosowane do [ `Label` ](xref:Xamarin.Forms.Label) wystąpień i [ `Button` ](xref:Xamarin.Forms.Button) wystąpienia, ustawiając ich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości. Skutkuje to wygląd pokazano na poniższych zrzutach ekranu:

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Styl niejawny mogą pochodzić z style jawne, ale jawne styl nie mogą pochodzić z stylu niejawnego.

### <a name="respecting-the-inheritance-chain"></a>Uwzględnienie łańcuch dziedziczenia

Styl może dziedziczyć tylko style na tym samym poziomie lub powyżej, w widoku hierarchii. Oznacza to, że:

- Zasobów na poziomie aplikacji może dziedziczyć tylko z innych zasobów poziomu aplikacji.
- Zasobu z poziomu strony może dziedziczyć z zasobów na poziomie aplikacji i innych zasobów poziomu strony.
- Zasobów z poziomu kontroli może dziedziczyć z zasobów na poziomie aplikacji, zasoby na poziomie strony i innych zasobów poziomu kontroli.

Ten łańcuch dziedziczenia jest przedstawiona w poniższym przykładzie kodu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelStyle" TargetType="Label" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                    <Style x:Key="buttonStyle" TargetType="Button" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

W tym przykładzie `labelStyle` i `buttonStyle` są kontroli poziomu zasobów, podczas gdy `baseStyle` jest zasobu z poziomu strony. Jednakże podczas `labelStyle` i `buttonStyle` dziedziczyć `baseStyle`, nie ma możliwości `baseStyle` odziedziczone `labelStyle` lub `buttonStyle`ze względu na ich odpowiedniej lokalizacji w hierarchii widoku.

## <a name="style-inheritance-in-c35"></a>Dziedziczenie stylów w języku C&#35;

Równoważne C# strony, gdzie [ `Style` ](xref:Xamarin.Forms.Style) wystąpienia zostaną przypisane bezpośrednio do [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości wymaganych punktów kontrolnych, przedstawiono w poniższym przykładzie kodu:

```csharp
public class StyleInheritancePageCS : ContentPage
{
    public StyleInheritancePageCS ()
    {
        var baseStyle = new Style (typeof(View)) {
            Setters = {
                new Setter {
                    Property = View.HorizontalOptionsProperty, Value = LayoutOptions.Center    },
                ...
            }
        };

        var labelStyle = new Style (typeof(Label)) {
            BasedOn = baseStyle,
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Teal    }
            }
        };

        var buttonStyle = new Style (typeof(Button)) {
            BasedOn = baseStyle,
            Setters = {
                new Setter { Property = Button.BorderColorProperty, Value =    Color.Lime },
                ...
            }
        };
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelStyle },
                ...
                new Button { Text = "So is the button", Style = buttonStyle }
            }
        };
    }
}
```

`baseStyle` Cele [ `View` ](xref:Xamarin.Forms.View) wystąpień i ustawia [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) i [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwości. `baseStyle` Nie ustawiono bezpośrednio na żadną kontrolkę. Zamiast tego `labelStyle` i `buttonStyle` dziedziczyć po nim, ustawianie wartości dodatkowe właściwości możliwej do wiązania. `labelStyle` i `buttonStyle` zostaną następnie zastosowane do [ `Label` ](xref:Xamarin.Forms.Label) wystąpień i [ `Button` ](xref:Xamarin.Forms.Button) wystąpienia, ustawiając ich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości.

## <a name="summary"></a>Podsumowanie

Style może dziedziczyć z innymi stylami, aby zmniejszyć dublowania i umożliwiają wielokrotne użycie. Dziedziczenie stylów odbywa się przez ustawienie [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) właściwości do istniejącego [ `Style` ](xref:Xamarin.Forms.Style).


## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Podstawowe style (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Praca ze stylami (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metody ustawiającej](xref:Xamarin.Forms.Setter)
