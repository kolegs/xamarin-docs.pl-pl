---
title: Styl dziedziczenia
description: "Style może dziedziczyć z innych style, aby zmniejszyć dublowania i włączanie ponownego użycia."
ms.topic: article
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: e57f19d1eb66e22badb418d4584f5654904c7ade
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="style-inheritance"></a>Styl dziedziczenia

_Style może dziedziczyć z innych style, aby zmniejszyć dublowania i włączanie ponownego użycia._

## <a name="style-inheritance-in-xaml"></a>Styl dziedziczenia w języku XAML

Styl dziedziczenia odbywa się przez ustawienie [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) właściwości do istniejącej [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/). W języku XAML, jest to osiągane przez ustawienie `BasedOn` właściwości `StaticResource` rozszerzenie znaczników, który odwołuje się do wcześniej utworzonego `Style`. W języku C#, jest to osiągane przez ustawienie `BasedOn` właściwości `Style` wystąpienia.

Style, które dziedziczą z stylu podstawowego mogą obejmować [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) wystąpień dla nowych właściwości lub użyj ich, aby przesłonić stylów ze stylu podstawowej. Ponadto style, które dziedziczą z stylu podstawowego muszą wskazywać tego samego typu lub typu pochodzącego od typu objęci podstawowy styl. Na przykład, jeśli cele podstawowy styl [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) wystąpień, style, które są oparte na stylu podstawowym celem może być `View` wystąpień lub typy pochodzące z `View` klas, takich jak [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) i [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wystąpień.

Poniższy kod przedstawia *jawne* styl dziedziczenia w strony XAML:

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

`baseStyle` Celów [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) wystąpień i ustawia [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) i [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości. `baseStyle` Nie ustawiono bezpośrednio na żaden formant. Zamiast tego `labelStyle` i `buttonStyle` dziedziczyć po nim, ustawienie wartości dodatkowe właściwości możliwej do wiązania. `labelStyle` i `buttonStyle` zostaną zastosowane do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień i [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wystąpienia, ustawiając ich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości. Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Niejawne styl mogą pochodzić z jawnym styl, ale styl jawne nie może dziedziczyć po stylu niejawne.

### <a name="respecting-the-inheritance-chain"></a>Przestrzeganie łańcuch dziedziczenia

Styl może dziedziczyć tylko z style na tym samym poziomie lub powyżej, w hierarchii widoku. Oznacza to, że:

- Zasobów na poziomie aplikacji może dziedziczyć tyko z innych zasobów poziomu aplikacji.
- Strona zasobów na poziomie może dziedziczyć z zasobów na poziomie aplikacji i innych zasobów poziomu strony.
- Zasobów poziomu kontroli może dziedziczyć z zasobów na poziomie aplikacji, zasoby poziomu strony i innych zasobów poziom kontroli.

Ten łańcuch dziedziczenia przedstawiono w poniższym przykładzie kodu:

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

W tym przykładzie `labelStyle` i `buttonStyle` są kontroli poziomu zasobów, podczas gdy `baseStyle` jest zasobów na poziomie strony. Jednakże podczas `labelStyle` i `buttonStyle` dziedziczyć `baseStyle`, nie jest możliwe w dla `baseStyle` odziedziczone `labelStyle` lub `buttonStyle`, z powodu ich odpowiednich lokalizacji w hierarchii widoku.

## <a name="style-inheritance-in-c35"></a>Styl dziedziczenia w języku C&#35;

Odpowiednik C# strony, gdzie [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia są przypisywane bezpośrednio do [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości wymagane formantów, przedstawiono w poniższym przykładzie kodu:

```csharp
public class StyleInheritancePageCS : ContentPage
{
    public StyleInheritancePageCS ()
    {
        var baseStyle = new Style (typeof(View)) {
            Setters = {
                new Setter {
                    Property = View.HorizontalOptionsProperty, Value = LayoutOptions.Center },
                ...
            }
        };

        var labelStyle = new Style (typeof(Label)) {
            BasedOn = baseStyle,
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Teal }
            }
        };

        var buttonStyle = new Style (typeof(Button)) {
            BasedOn = baseStyle,
            Setters = {
                new Setter { Property = Button.BorderColorProperty, Value = Color.Lime },
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

`baseStyle` Celów [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) wystąpień i ustawia [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) i [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości. `baseStyle` Nie ustawiono bezpośrednio na żaden formant. Zamiast tego `labelStyle` i `buttonStyle` dziedziczyć po nim, ustawienie wartości dodatkowe właściwości możliwej do wiązania. `labelStyle` i `buttonStyle` zostaną zastosowane do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień i [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wystąpienia, ustawiając ich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości.

## <a name="summary"></a>Podsumowanie

Style może dziedziczyć z innych style, aby zmniejszyć dublowania i włączanie ponownego użycia. Styl dziedziczenia odbywa się przez ustawienie [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) właściwości do istniejącej [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/).


## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style podstawowe (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Praca z style (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metody ustawiającej](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
