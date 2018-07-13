---
title: Style tekstu zestawu narzędzi Xamarin.Forms
description: W tym artykule opisano sposób stylów tekstu w aplikacjach Xamarin.Forms. Style może być zdefiniowana raz i używane przez wiele widoków, ale stylu należy używać tylko z widokami jednego typu.
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 73aa3115e92d1e3954f5ae3eb8dcb84abf9d9efb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998771"
---
# <a name="xamarinforms-text-styles"></a>Style tekstu zestawu narzędzi Xamarin.Forms

_Style tekstu w interfejsie Xamarin.Forms_

Style można dostosować wygląd etykiety, wpisy i edytorów. Style może być zdefiniowana raz i używane przez wiele widoków, ale stylu należy używać tylko z widokami jednego typu.
Style można podać `Key` i stosowane selektywnie przy użyciu określonej kontrolki `Style` właściwości.

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>Style wbudowane

Zestaw narzędzi Xamarin.Forms zawiera kilka [wbudowanych](xref:Xamarin.Forms.Device.Styles) stylów dla typowych scenariuszy:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

Aby zastosować jedną z wbudowanych stylów, użyj `DynamicResource` — rozszerzenie znaczników do określania stylu:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

W języku C# style wbudowane są wybrane z `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "Przykład style urządzenia")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>Style niestandardowe

Style składają się z metody ustawiające i metod ustawiających składają się z właściwości jest równa wartości właściwości.

W języku C# własny styl etykiety na czerwono o rozmiarze 30 będą zdefiniowane w następujący sposób:

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

W XAML:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="LabelStyle" TargetType="Label">
            <Setter Property="TextColor" Value="Red"/>
            <Setter Property="FontSize" Value="30"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<ContentPage.Content>
    <StackLayout>
        <Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
    </StackLayout>
</ContentPage.Content>
```

Należy pamiętać, że zasoby (w tym wszystkie style) są zdefiniowane w ramach `ContentPage.Resources`, który jest elementem równorzędnym węzła lepiej `ContentPage.Content` elementu.

![](styles-images/customstyle.png "Przykładowe niestandardowe style")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>Stosowanie stylów

Po utworzeniu stylu można go zastosować do Dopasowanie widoku jego `TargetType`.

W XAML, niestandardowe style są stosowane do widoków przez przesłanie ich `Style` właściwość o `StaticResource` odwołuje się do żądanego stylu — rozszerzenie znaczników:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

W języku C# style można być stosowane bezpośrednio do widoku lub dodane do i pobrać ze strony `ResourceDictionary`. Aby dodać bezpośrednio:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Aby dodać i pobrania na stronie `ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

Style wbudowane są stosowane w różny sposób, ponieważ muszą one odpowiadać na ustawienia ułatwień dostępu. Aby zastosować style wbudowane w XAML, `DynamicResource` jest używane rozszerzenie znaczników:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

W języku C# style wbudowane są wybrane z `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>Ułatwienia dostępu

Style wbudowane ma ułatwić przestrzegają preferencje ułatwień dostępu. W przypadku stosowania style wbudowane rozmiary czcionek spowoduje zwiększenie automatycznie, jeżeli użytkownik ustawi odpowiednio swoje preferencje ułatwień dostępu.

Rozważmy następujący przykład tej samej strony stylem wbudowane style, przy użyciu ustawień ułatwień dostępu, włączone i wyłączone widoki:

Wyłączone:

![](styles-images/pre-access.png "Style urządzenia z wyłączoną ułatwień dostępu")

Włączone:

![](styles-images/post-access.png "Style urządzenia z włączoną ułatwień dostępu")

Aby zapewnić dostępność, upewnij się, że style wbudowane są używane jako podstawa dla dowolnego stylów związane z tekstem w aplikacji i że używasz style spójne. Zobacz [style](~/xamarin-forms/user-interface/styles/index.md) więcej informacji na temat rozszerzania i pracą z nimi przy użyciu stylów ogólnie rzecz biorąc.


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms rozdziale 12](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [Style](~/xamarin-forms/user-interface/styles/index.md)
- [Tekst (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Styl](xref:Xamarin.Forms.Style)
