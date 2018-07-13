---
title: Style niejawne w interfejsie Xamarin.Forms
description: Styl niejawny to taki, który jest używany przez wszystkie formanty TargetType tego samego, bez konieczności każdy formant, aby odwołać stylu.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 277be51c242521f52e9b1e162226ae8137e7b133
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995517"
---
# <a name="implicit-styles-in-xamarinforms"></a>Style niejawne w interfejsie Xamarin.Forms

_Styl niejawny to taki, który jest używany przez wszystkie formanty TargetType tego samego, bez konieczności każdy formant, aby odwołać stylu._

## <a name="creating-an-implicit-style-in-xaml"></a>Tworzenie stylu niejawnego w XAML

Aby zadeklarować [ `Style` ](xref:Xamarin.Forms.Style) na poziomie strony [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) musi zostać dodany do strony i następnie co najmniej jeden `Style` deklaracje mogą być zawarte w `ResourceDictionary`. A `Style` wykonano *niejawne* , nie określając `x:Key` atrybutu. Styl zostaną zastosowane do elementów wizualnych, które odpowiadają `TargetType` dokładnie tak, ale nie do elementów, które są uzyskiwane z `TargetType` wartość.

Poniższy kod przedstawia przykład *niejawne* styl zadeklarowanych w XAML na stronie `ResourceDictionary`i zastosowane do strony [ `Entry` ](xref:Xamarin.Forms.Entry) wystąpień:

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

[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Definiuje pojedyncze *niejawne* styl, który jest stosowany do strony [ `Entry` ](xref:Xamarin.Forms.Entry) wystąpień. `Style` Służy do wyświetlania niebieski tekst w żółte tło podczas ustawiania również inne opcje wygląd. `Style` Zostanie dodany do strony [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) bez określania `x:Key` atrybutu. W związku z tym `Style` jest stosowane do wszystkich `Entry` wystąpień niejawnie, jak pasują [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) właściwość `Style` dokładnie. Jednak `Style` nie ma zastosowania do `CustomEntry` wystąpienie, będące podklasami `Entry`. Skutkuje to wygląd pokazano na poniższych zrzutach ekranu:

[![](implicit-images/implicit-styles.png "Przykład niejawna style")](implicit-images/implicit-styles-large.png#lightbox "przykład style niejawne")

Ponadto, czwarta [ `Entry` ](xref:Xamarin.Forms.Entry) zastępuje [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) i [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) właściwości stylu niejawnego do różnych `Color`wartości.

### <a name="creating-an-implicit-style-at-the-control-level"></a>Tworzenie stylu niejawnego na podczas kontroli

Oprócz tworzenia *niejawne* style na poziomie strony, ich można również utworzyć na poziomie kontroli, jak pokazano w poniższym przykładzie kodu:

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

W tym przykładzie *niejawne* [ `Style` ](xref:Xamarin.Forms.Style) jest przypisany do [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) zbiór [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)kontroli. *Niejawne* następnie można zastosować styl do kontrolki i jego elementy podrzędne.

Informacje o tworzeniu style w aplikacji [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), zobacz [style globalne](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-implicit-style-in-c35"></a>Tworzenie stylu niejawnego w języku C&#35;

[`Style`](xref:Xamarin.Forms.Style) wystąpienia mogą być dodawane do strony [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekcji w języku C#, tworząc nowe [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), a następnie dodając `Style` wystąpień do `ResourceDictionary`, jak pokazano na Poniższy przykład kodu:

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

Konstruktor definiuje pojedyncze *niejawne* styl, który jest stosowany do strony [ `Entry` ](xref:Xamarin.Forms.Entry) wystąpień. `Style` Służy do wyświetlania niebieski tekst w żółte tło podczas ustawiania również inne opcje wygląd. `Style` Zostanie dodany do strony [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) bez określania `key` ciągu. W związku z tym `Style` jest stosowane do wszystkich `Entry` wystąpień niejawnie, jak pasują [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) właściwość `Style` dokładnie. Jednak `Style` nie ma zastosowania do `CustomEntry` wystąpienie, będące podklasami `Entry`.

## <a name="summary"></a>Podsumowanie

*Niejawne* styl to taki, który jest używany przez wszystkie elementy wizualne w tej samej [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), bez konieczności każdy formant, aby odwołać stylu. A `Style` wykonano *niejawne* , nie określając `x:Key` atrybutu. Zamiast tego `x:Key` atrybut automatycznie staną się wartość [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) właściwości.



## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Podstawowe style (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Praca ze stylami (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metody ustawiającej](xref:Xamarin.Forms.Setter)
