---
title: Style
description: Styl tekstu w platformy Xamarin.Forms
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: f38e4bc9ecd66c1dc33e53fa5c9046ff363802e6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="styles"></a>Style

_Styl tekstu w platformy Xamarin.Forms_


Style umożliwia dostosowanie wyglądu etykiet, wpisy i edytory. Style może być zdefiniowany tylko raz i używany przez wiele widoków, ale stylu można używać tylko z widokami jednego typu.
Style można przydzielić `Key` i stosowane selektywnie przy użyciu określonego formantu `Style` właściwości.

W tym artykule omówiono następujące tematy:

- **[Wbudowane style](#Built-In_Styles)**  &ndash; przy użyciu wbudowanych stylów styl tekstowy widoków w całej aplikacji.
- **[Style niestandardowe](#Custom_Styles)**  &ndash; style niestandardowe pozwalają zdefiniować, kiedy wbudowanych opcji nie ma wystarczającej ilości.
- **[Stosowanie stylów](#Applying_Styles)**  &ndash; stosowanie stylów wbudowanych i niestandardowych widoków.
- **[Ułatwienia dostępu](#Accessibility)**  &ndash; upewnij się, że tekst szanuje ustawienia ułatwień dostępu.

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>Wbudowane style

Platformy Xamarin.Forms zawiera kilka [wbudowanych](http://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) style dla typowych scenariuszy:

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

W języku C#, wbudowane style są wybierane w `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "Przykład style urządzenia")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>Style niestandardowe

Style składają się z metody ustawiające i zawierać metody ustawiające właściwości jest równa wartości właściwości.

W języku C# styl niestandardowy etykiety na czerwono o rozmiarze 30 będą zdefiniowane w następujący sposób:

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

W języku XAML:

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

Należy pamiętać, że zasoby (w tym wszystkie style) są zdefiniowane w ramach `ContentPage.Resources`, który jest elementem równorzędnym im bardziej znanych `ContentPage.Content` elementu.

![](styles-images/customstyle.png "Przykład style niestandardowe")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>Stosowanie stylów

Po utworzeniu stylu, można je stosować do Dopasowanie widoku jego `TargetType`.

W języku XAML, style niestandardowe są stosowane do widoków, podając ich `Style` właściwości o `StaticResource` odwołuje się do żądany styl — rozszerzenie znaczników:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

W języku C# style albo można zastosować bezpośrednio do widoku lub dodane do i pobrać ze strony `ResourceDictionary`. Aby dodać bezpośrednio:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Aby dodać i pobrać ze strony `ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

Wbudowane style są stosowane inaczej, ponieważ muszą odpowiadać na ustawienia ułatwień dostępu. Aby zastosować wbudowane style w języku XAML, `DynamicResource` — rozszerzenie znaczników jest używany:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

W języku C#, wbudowane style są wybierane w `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>Ułatwienia dostępu

Wbudowane style istnieje ułatwiające przestrzegać preferencje dostępności. W przypadku stosowania wbudowane style rozmiary czcionek automatycznie spowoduje zwiększenie, jeśli użytkownik konfiguruje odpowiednio ich preferencje dostępności.

Rozważmy poniższy przykład przedstawia tej samej stronie stylem wbudowane style z ustawienia ułatwień dostępu, włączyć i wyłączyć widoki:

Wyłączone:

![](styles-images/pre-access.png "Style urządzenia z wyłączonymi ułatwień dostępu")

Włączone:

![](styles-images/post-access.png "Style urządzenia z włączoną ułatwień dostępu")

W celu zapewnienia dostępności, upewnij się, czy wbudowane style są używane jako podstawa dla dowolnego style związanych z tekstem w Twojej aplikacji i że używasz style spójnie. Zobacz [style](~/xamarin-forms/user-interface/styles/index.md) uzyskać więcej informacji dotyczących rozszerzenie i Praca z style w zasadzie.


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie aplikacji mobilnych za pomocą platformy Xamarin.Forms, rozdziale 12](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [Style](~/xamarin-forms/user-interface/styles/index.md)
- [Tekst (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
