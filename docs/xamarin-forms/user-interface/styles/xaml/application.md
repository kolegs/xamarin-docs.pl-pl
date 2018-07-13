---
title: Style globalne w interfejsie Xamarin.Forms
description: Style mogą być udostępniane globalnie, dodając je do słownika zasobów aplikacji. Pomaga to uniknąć duplikowania style różnych stron i formantów.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: e7b2a37b868ea03ca626ffd2dcddb006a235b0cc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995404"
---
# <a name="global-styles-in-xamarinforms"></a>Style globalne w interfejsie Xamarin.Forms

_Style mogą być udostępniane globalnie, dodając je do słownika zasobów aplikacji. Pomaga to uniknąć duplikowania style różnych stron i formantów._

## <a name="creating-a-global-style-in-xaml"></a>Tworzenie stylu globalne w XAML

Domyślnie wszystkie aplikacje Xamarin.Forms tworzone na podstawie szablonu używają **aplikacji** klasy do zaimplementowania [ `Application` ](xref:Xamarin.Forms.Application) podklasę. Aby zadeklarować [ `Style` ](xref:Xamarin.Forms.Style) na poziomie aplikacji w aplikacji w witrynie [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) przy użyciu XAML, domyślnie **aplikacji** klasy muszą zostać zastąpione w XAML **Aplikacji** klasy i skojarzone związanym z kodem. Aby uzyskać więcej informacji, zobacz [pracy przy użyciu klasy aplikacji](~/xamarin-forms/app-fundamentals/application-class.md).

Poniższy kod przedstawia przykład [ `Style` ](xref:Xamarin.Forms.Style) zadeklarowane na poziomie aplikacji:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.App">
    <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BorderColor" Value="Lime" />
                <Setter Property="BorderRadius" Value="5" />
                <Setter Property="BorderWidth" Value="5" />
                <Setter Property="WidthRequest" Value="200" />
                <Setter Property="TextColor" Value="Teal" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

To [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) definiuje pojedyncze *jawne* stylu `buttonStyle`, która będzie służyć do ustawić wygląd [ `Button` ](xref:Xamarin.Forms.Button) wystąpień. Jednak może być style globalne *jawne* lub *niejawne*.

Poniższy przykład kodu pokazuje, stosując strony XAML `buttonStyle` na stronę [ `Button` ](xref:Xamarin.Forms.Button) wystąpień:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Skutkuje to wygląd pokazano na poniższych zrzutach ekranu:

[![](application-images/application-styles-1.png "Przykład style globalne")](application-images/application-styles-1-large.png#lightbox "przykład style globalne")

Informacje o tworzeniu style na stronie [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), zobacz [style jawne](~/xamarin-forms/user-interface/styles/explicit.md) i [niejawna style](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="overriding-styles"></a>Zastępowanie style

Style niżej w hierarchii widoku pierwszeństwo identyczne ze zdefiniowanymi wyższej w górę. Na przykład ustawienie [ `Style` ](xref:Xamarin.Forms.Style) określająca [ `Button.TextColor` ](xref:Xamarin.Forms.Button.TextColor) do `Red` na poziomie aplikacji, poziom zostaną zastąpione przez styl poziomu strony, która ustawia `Button.TextColor` do `Green`. Podobnie styl z poziomu strony zostaną zastąpione przez styl z poziomu kontroli. Ponadto jeśli `Button.TextColor` jest ustawiona, bezpośrednio we właściwości kontrolki, to ma pierwszeństwo wszelkie style. Priorytet tej przedstawionej w poniższym przykładzie kodu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                ...
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="buttonStyle" TargetType="Button">
                        ...
                        <Setter Property="TextColor" Value="Blue" />
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Oryginalny `buttonStyle`, zdefiniowanych na poziomie aplikacji, jest zastępowany przez `buttonStyle` wystąpienia zdefiniowane na poziomie strony. Ponadto styl poziomu strony jest zastępowany przez poziom kontroli `buttonStyle`. W związku z tym [ `Button` ](xref:Xamarin.Forms.Button) wyświetlane są wystąpienia z niebieskim, jak pokazano na poniższych zrzutach ekranu:

[![](application-images/application-styles-2.png "Zastępowanie przykład style")](application-images/application-styles-2-large.png#lightbox "zastępowanie przykład style")

## <a name="creating-a-global-style-in-c35"></a>Tworzenie stylu globalne w języku C&#35;

[`Style`](xref:Xamarin.Forms.Style) w aplikacji można dodać wystąpienia [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekcji w języku C#, tworząc nowe [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), a następnie dodając `Style` wystąpień do `ResourceDictionary`, jako pokazano w poniższym przykładzie kodu:

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,    Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

Konstruktor definiuje pojedyncze *jawne* styl stosowania do [ `Button` ](xref:Xamarin.Forms.Button) wystąpień w całej aplikacji. *Jawne* [ `Style` ](xref:Xamarin.Forms.Style) wystąpienia są dodawane do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) przy użyciu [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) metody, określając `key`ciągu do odwoływania się do `Style` wystąpienia. `Style` Wystąpienia można następnie zastosować funkcje na żadną kontrolkę poprawnego typu w aplikacji. Jednak może być style globalne *jawne* lub *niejawne*.

Poniższy przykład kodu pokazuje, C# strony stosowanie `buttonStyle` na stronę [ `Button` ](xref:Xamarin.Forms.Button) wystąpień:

```csharp
public class ApplicationStylesPageCS : ContentPage
{
    public ApplicationStylesPageCS ()
    {
        ...
        Content = new StackLayout {
            Children = {
                new Button { Text = "These buttons", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "are demonstrating", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "application styles", Style = (Style)Application.Current.Resources ["buttonStyle"]
                }
            }
        };
    }
}
```

`buttonStyle` Jest stosowany do [ `Button` ](xref:Xamarin.Forms.Button) wystąpienia, ustawiając ich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości i określa wygląd `Button` wystąpień.

## <a name="summary"></a>Podsumowanie

Style mogą być udostępniane globalnie, dodając je do aplikacji [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Pomaga to uniknąć duplikowania style różnych stron i formantów.



## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Podstawowe style (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Praca ze stylami (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metody ustawiającej](xref:Xamarin.Forms.Setter)
