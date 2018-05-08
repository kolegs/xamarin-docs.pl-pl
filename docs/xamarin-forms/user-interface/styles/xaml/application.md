---
title: Style globalne
description: Style mogą być udostępniane globalnie przez dodanie ich do słownika zasobów aplikacji. Pozwala to uniknąć duplikowania style na stronach lub formantów.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: a505720e5fef8fe9e9ef82d03e53370210772f45
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="global-styles"></a>Style globalne

_Style mogą być udostępniane globalnie przez dodanie ich do słownika zasobów aplikacji. Pozwala to uniknąć duplikowania style na stronach lub formantów._

## <a name="creating-a-global-style-in-xaml"></a>Tworzenie stylu globalnych w języku XAML

Domyślnie wszystkie aplikacje platformy Xamarin.Forms utworzonych na podstawie szablonu używają **aplikacji** klasy do zaimplementowania [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) podklasy. Aby zadeklarować [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) na poziomie aplikacji w aplikacji [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) przy użyciu kodu XAML, domyślnie **aplikacji** klasy muszą zostać zastąpione XAML **Aplikacji** klasy i skojarzone kodem. Aby uzyskać więcej informacji, zobacz [Praca z klasy aplikacji](~/xamarin-forms/app-fundamentals/application-class.md).

Poniższy kod przedstawia przykład [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) zadeklarowane na poziomie aplikacji:

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

To [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definiuje jedną *jawne* stylu, `buttonStyle`, który będzie służyć do ustawienia wyglądu [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wystąpień. Jednak może być style globalne *jawne* lub *niejawne*.

Poniższy przykład kodu pokazuje stosowania strony XAML `buttonStyle` do strony [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wystąpień:

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

Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

[![](application-images/application-styles-1.png "Przykład style globalne")](application-images/application-styles-1-large.png#lightbox "przykład style globalne")

Informacje o tworzeniu style na stronie [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), zobacz [jawne style](~/xamarin-forms/user-interface/styles/explicit.md) i [niejawne style](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="overriding-styles"></a>Zastępowanie style

Style niżej w hierarchii widoku pierwszeństwo określone wyższy się. Na przykład ustawienie [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) stanowiąca [ `Button.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/) do `Red` w aplikacji będą zastępowane poziom styl poziomu strony, która ustawia `Button.TextColor` do `Green`. Podobnie styl poziomu strony zostaną zastąpione przez stylu poziomu formantu. Ponadto jeśli `Button.TextColor` jest ustawiona, bezpośrednio na właściwości formantu, to będzie miało pierwszeństwo przed wszystkie style. Pierwszeństwo tego przedstawiono w poniższym przykładzie kodu:

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

Oryginalna `buttonStyle`, zdefiniowanych na poziomie aplikacji, jest zastępowana `buttonStyle` wystąpienia zdefiniowane na poziomie strony. Ponadto styl poziomie strony jest zastępowana poziom kontroli `buttonStyle`. W związku z tym [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wyświetlane są wystąpienia z niebieskim, jak pokazano na poniższych zrzutach ekranu:

[![](application-images/application-styles-2.png "Zastępowanie przykład style")](application-images/application-styles-2-large.png#lightbox "zastępowanie przykład style")

## <a name="creating-a-global-style-in-c35"></a>Tworzenie stylu globalnych w języku C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia można dodać do aplikacji [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekcji w języku C#, tworząc nowe [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), a następnie dodając `Style` wystąpień do `ResourceDictionary`, jako przedstawiono w poniższym przykładzie:

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,   Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

Konstruktor definiuje jedną *jawne* styl stosowania [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wystąpień w całej aplikacji. *Jawne* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia są dodawane do [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) przy użyciu [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) metoda określania `key`ciąg do odwoływania się do `Style` wystąpienia. `Style` Wystąpienia następnie mogą być stosowane do wszystkich formantów poprawnego typu w aplikacji. Jednak może być style globalne *jawne* lub *niejawne*.

Poniższy przykład kodu pokazuje, C# strony stosowania `buttonStyle` do strony [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wystąpień:

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

`buttonStyle` Jest stosowany do [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wystąpień przez ustawienie ich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości oraz określa wygląd `Button` wystąpień.

## <a name="summary"></a>Podsumowanie

Style mogą być udostępniane globalnie przez dodanie ich do aplikacji [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Pozwala to uniknąć duplikowania style na stronach lub formantów.



## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style podstawowe (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Praca z style (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metody ustawiającej](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
