---
title: Wprowadzenie do stylów
description: Style umożliwiają wyświetlanie elementów wizualnych do dostosowania. Style są definiowane dla określonego typu i zawierają wartości dla właściwości dostępne dla tego typu.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 453c4d6edafd6493272f8ca0435fcc86e2f3b2f7
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="introduction-to-styles"></a>Wprowadzenie do stylów

_Style umożliwiają wyświetlanie elementów wizualnych do dostosowania. Style są definiowane dla określonego typu i zawierają wartości dla właściwości dostępne dla tego typu._

Aplikacje platformy Xamarin.Forms często zawierają wiele formantów, które mają identyczne wyglądu. Na przykład aplikacja może mieć wielu [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień, które mają te same opcje czcionki i opcje układu, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Poniższy przykład kodu pokazuje odpowiedniej strony utworzone w języku C#:

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        Icon = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

Każdy [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpienie ma kontrolowanie wyglądu tekstu wyświetlanego przez wartości właściwości identyczne `Label`. Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

[![](introduction-images/no-styles.png "Etykieta wygląd bez style")](introduction-images/no-styles-large.png#lightbox "etykietę wygląd bez style")

Ustawianie wygląd każdego pojedynczego formantu może być powtarzane i błąd podatnych na błędy. Zamiast tego stylu można utworzyć, która definiuje wygląd, a następnie stosowane do wymaganych kontrolek.

## <a name="creating-a-style"></a>Tworzenie stylu

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Klasy grup kolekcji wartości właściwości do jednego obiektu, który można zastosować do wielu wystąpień elementu wizualnego. Umożliwia zmniejszenie liczby powtarzających się fragmentów kodu i umożliwia wygląd aplikacji w taki sposób, aby łatwiej można zmienić.

Mimo że style są przeznaczone głównie dla aplikacji opartych na języku XAML, mogą one również tworzone w języku C#:

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia utworzone w języku XAML są zazwyczaj definiowane w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) przypisany do [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekcji formantu, strony, lub do [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) kolekcji aplikacji.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia utworzone w języku C# są zazwyczaj definiowane w klasie strony lub w klasie mogą uzyskiwać globalnie.

Wybieranie miejsce definiowania [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wpływu, których można użyć:

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia zdefiniowane na poziomie formantu można stosować do formantu i jego elementów podrzędnych.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia zdefiniowane na poziomie strony można stosować do strony i jego elementów podrzędnych.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia zdefiniowane na poziomie aplikacji mogą być stosowane w całej aplikacji.

Każdy [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienie zawiera co najmniej jedną kolekcję [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) obiektów z każdym `Setter` o [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) i [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/). `Property` Jest nazwą właściwości możliwej do wiązania elementu styl ma zastosowanie i `Value` jest wartość, która jest stosowana do właściwości.

Każdy [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) można instancji *jawne*, lub *niejawne*:

- *Jawne* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia jest zdefiniowany przez określenie [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) i `x:Key` wartość, a przez ustawienie elementu docelowego [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości `x:Key` odwołania. Aby uzyskać więcej informacji na temat *jawne* style, zobacz [jawne style](~/xamarin-forms/user-interface/styles/explicit.md).
- *Niejawne* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia jest zdefiniowany, określając tylko [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/). `Style` Wystąpienia następnie zostaną automatycznie zastosowane do wszystkich elementów tego typu. Uwaga tej podklasy `TargetType` automatycznie nie mają `Style` zastosowane. Aby uzyskać więcej informacji na temat *niejawne* style, zobacz [niejawne style](~/xamarin-forms/user-interface/styles/implicit.md).

Podczas tworzenia [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/), [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) właściwości jest zawsze wymagane. Poniższy kod przedstawia przykład *jawne* styl (Uwaga `x:Key`) utworzone w języku XAML:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Aby zastosować `Style`, obiekt docelowy musi być [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) odpowiadającego [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) wartość właściwości `Style`, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Style niżej w hierarchii widoku pierwszeństwo określone wyższy się. Na przykład ustawienie [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) stanowiąca [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) do `Red` w aplikacji będą zastępowane poziom styl poziomu strony, która ustawia `Label.TextColor` do `Green`. Podobnie styl poziomu strony zostaną zastąpione przez stylu poziomu formantu. Ponadto jeśli `Label.TextColor` jest ustawiona, bezpośrednio na właściwości formantu, to ma pierwszeństwo przed wszystkie style.

Artykuły w tej sekcji prezentacja i wyjaśniono, jak utworzyć i zastosować *jawne* i *niejawne* style, jak tworzyć stylów globalnych, styl dziedziczenia, odpowiadanie na zmiany stylu w czasie wykonywania i sposobu użycia w wbudowane style z platformy Xamarin.Forms.

> [!NOTE]
> **Co to jest StyleId?**
>
> Przed 2.2 platformy Xamarin.Forms [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) właściwość została użyta w celu identyfikowania poszczególnych elementów w aplikacji do identyfikacji podczas testowania interfejsu użytkownika, a w aparaty motywu, takich jak Pixate. Jednak wprowadziła 2.2 platformy Xamarin.Forms [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) właściwość, która została zastąpiona [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) właściwości. Aby uzyskać więcej informacji, zobacz [automatyzacji platformy Xamarin.Forms testowanie za pomocą Xamarin.UITest i chmury testowej](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md).

## <a name="summary"></a>Podsumowanie

Aplikacje platformy Xamarin.Forms często zawierają wiele formantów, które mają identyczne wyglądu. Ustawianie wygląd każdego pojedynczego formantu może być powtarzane i błąd podatnych na błędy. Zamiast tego style mogą być tworzone umożliwiające dostosowanie wyglądu formantu przez grupowanie i ustawienia właściwości dostępnych dla typu formantu.


## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metody ustawiającej](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
