---
title: Wprowadzenie do zestawu narzędzi Xamarin.Forms style
description: Style umożliwiają elementami wizualnymi, aby dostosować wygląd. Style są zdefiniowane dla określonego typu i zawierają wartości dla właściwości dostępne dla tego typu.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 8f84c960f17f56fce2a1bba143a215ce930f6f4e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996113"
---
# <a name="introduction-to-xamarinforms-styles"></a>Wprowadzenie do zestawu narzędzi Xamarin.Forms style

_Style umożliwiają elementami wizualnymi, aby dostosować wygląd. Style są zdefiniowane dla określonego typu i zawierają wartości dla właściwości dostępne dla tego typu._

Aplikacje Xamarin.Forms często zawierają wiele formantów, które mają identyczne wygląd. Na przykład aplikacja może mieć wiele [ `Label` ](xref:Xamarin.Forms.Label) wystąpień, które mają te same opcje czcionki i opcje układu, jak pokazano w poniższym przykładzie kodu XAML:

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

Poniższy przykład kodu pokazuje odpowiednich stron, które są tworzone w języku C#:

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

Każdy [ `Label` ](xref:Xamarin.Forms.Label) wystąpienie posiada wartości właściwości identyczne kontrolowanie wyglądu tekstu wyświetlanego przez `Label`. Skutkuje to wygląd pokazano na poniższych zrzutach ekranu:

[![](introduction-images/no-styles.png "Etykieta wygląd bez style")](introduction-images/no-styles-large.png#lightbox "etykiety wygląd bez style")

Ustawienie wygląd każdego pojedynczego formantu może być powtarzane i występowania błędów. Zamiast tego stylu można utworzyć, która definiuje wygląd, a następnie stosowane do wymaganych punktów kontrolnych.

## <a name="creating-a-style"></a>Tworzenie stylu

[ `Style` ](xref:Xamarin.Forms.Style) Klasy grupy to zbiór wartości właściwości w jeden obiekt, który można zastosować do wielu wystąpień elementu wizualnego. To ułatwia ograniczenie powtarzających się fragmentów kodu oraz umożliwia wygląd aplikacji w taki sposób, aby łatwiej można zmienić.

Mimo że style zostały zaprojektowane głównie dla aplikacji opartych na XAML, one również można utworzyć w języku C#:

- [`Style`](xref:Xamarin.Forms.Style) wystąpienia utworzone w XAML są zazwyczaj zdefiniowane w [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) przypisany do [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekcji danej kontrolki, na stronie lub [ `Resources` ](xref:Xamarin.Forms.Application.Resources) kolekcji aplikacji.
- [`Style`](xref:Xamarin.Forms.Style) wystąpienia utworzone w języku C# są zazwyczaj definiowane w klasie strony lub w klasie, która może globalnie dostępna.

Wybieranie miejsce definiowania [ `Style` ](xref:Xamarin.Forms.Style) na środowisko, w których można użyć:

- [`Style`](xref:Xamarin.Forms.Style) wystąpienia zdefiniowane na poziomie kontroli można zastosować tylko do formantu i jego elementów podrzędnych.
- [`Style`](xref:Xamarin.Forms.Style) wystąpienia zdefiniowane na poziomie strony będzie stosowany tylko do strony i jego elementów podrzędnych.
- [`Style`](xref:Xamarin.Forms.Style) wystąpienia zdefiniowane na poziomie aplikacji mogą być stosowane w całej aplikacji.

Każdy [ `Style` ](xref:Xamarin.Forms.Style) wystąpienie zawiera zbiór co najmniej jeden [ `Setter` ](xref:Xamarin.Forms.Setter) obiektów, z każdym `Setter` o [ `Property` ](xref:Xamarin.Forms.Setter.Property) i [`Value`](xref:Xamarin.Forms.Setter.Value). `Property` Jest nazwą właściwości możliwej do wiązania elementu zastosowano ten styl, a `Value` jest wartością, która jest stosowana do właściwości.

Każdy [ `Style` ](xref:Xamarin.Forms.Style) wystąpienie może być *jawne*, lub *niejawne*:

- *Jawne* [ `Style` ](xref:Xamarin.Forms.Style) wystąpienia jest zdefiniowany, określając [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) i `x:Key` wartości, a przez ustawienie elementu docelowego [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwość `x:Key` odwołania. Aby uzyskać więcej informacji na temat *jawne* style, zobacz [style jawne](~/xamarin-forms/user-interface/styles/explicit.md).
- *Niejawne* [ `Style` ](xref:Xamarin.Forms.Style) wystąpienia jest zdefiniowany, określając tylko [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType). `Style` Wystąpienia zostaną automatycznie zastosowane do wszystkich elementów tego typu. Uwaga tej podklasy `TargetType` nie mają automatycznie `Style` stosowane. Aby uzyskać więcej informacji na temat *niejawne* style, zobacz [niejawna style](~/xamarin-forms/user-interface/styles/implicit.md).

Podczas tworzenia [ `Style` ](xref:Xamarin.Forms.Style), [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) właściwości jest zawsze wymagana. Poniższy kod przedstawia przykład *jawne* stylu (Uwaga `x:Key`) utworzone w XAML:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Aby zastosować `Style`, obiekt docelowy musi być [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) odpowiadający [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) wartość właściwości `Style`, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Style niżej w hierarchii widoku pierwszeństwo identyczne ze zdefiniowanymi wyższej w górę. Na przykład ustawienie [ `Style` ](xref:Xamarin.Forms.Style) określająca [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) do `Red` na poziomie aplikacji, poziom zostaną zastąpione przez styl poziomu strony, która ustawia `Label.TextColor` do `Green`. Podobnie styl z poziomu strony zostaną zastąpione przez styl z poziomu kontroli. Ponadto jeśli `Label.TextColor` jest ustawiona, bezpośrednio we właściwości kontrolki, to ma pierwszeństwo przed wszystkie style.

Artykuły w tej sekcji pokazują i wyjaśniono, jak utworzyć i zastosować *jawne* i *niejawne* style, jak utworzyć style globalne, stylu dziedziczenie, jak reagować na zmiany wprowadzone w stylu w czasie wykonywania i sposobu używania style wbudowane uwzględnione w interfejsie Xamarin.Forms.

> [!NOTE]
> **Co to jest StyleId?**
>
> Przed Xamarin.Forms 2.2 [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) właściwość była używana do identyfikowania poszczególnych elementów w aplikacji do identyfikacji w testowanie interfejsu użytkownika i aparatów motywu, takich jak Pixate. Jednak wprowadził Xamarin.Forms 2.2 [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) właściwość, która została zastąpiona [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) właściwości. Aby uzyskać więcej informacji, zobacz [Xamarin.Forms zautomatyzować testowanie za pomocą Xamarin.UITest i usługa Test Cloud](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md).

## <a name="summary"></a>Podsumowanie

Aplikacje Xamarin.Forms często zawierają wiele formantów, które mają identyczne wygląd. Ustawienie wygląd każdego pojedynczego formantu może być powtarzane i występowania błędów. Zamiast tego stylów mogą być tworzone umożliwiające dostosowanie wyglądu formantu przez grupowanie i właściwości ustawień dostępnych w kontrolce o typie.


## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Styl](xref:Xamarin.Forms.Style)
- [Metody ustawiającej](xref:Xamarin.Forms.Setter)
