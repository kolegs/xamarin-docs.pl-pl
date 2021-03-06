---
title: Dokumentacja formantów DataPages
description: W tym artykule przedstawiono formantów, które są dostępne w pakiecie DataPages NuGet zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: c907d55f09d334e167c831a19f9d0edc4c97732f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38866525"
---
# <a name="datapages-controls-reference"></a>Dokumentacja formantów DataPages

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji zapoznawczej")

> [!IMPORTANT]
> Wymaga DataPages [motyw Xamarin.Forms](~/xamarin-forms/user-interface/themes/index.md) odwołania do renderowania.


Do pakietu DataPages Nuget Xamarin.Forms zawiera wiele elementów sterujących, które można wykorzystać powiązanie źródła danych.

Aby użyć tych kontrolek w XAML, upewnij się, przestrzeń nazw została uwzględniona, na przykład zobacz `xmlns:pages` deklaracji poniżej:

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

Poniższe przykłady zawierają `DynamicResource` odwołania, które należałoby istnieje w słowniku zasobów projektu do pracy. Istnieje również przykład sposobu tworzenia [kontrolki niestandardowej](#custom)

## <a name="built-in-controls"></a>Wbudowanych formantów

* [Duży obraz](#heroimage)
* [ListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>Duży obraz

`HeroImage` Kontrolka ma cztery właściwości:

* Tekst
* Szczegóły
* ImageSource
* Aspekt

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "Duży obraz sterowania w systemie Android") ![](controls-images/heroimage-dark-android.png "duży obraz sterowania w systemie Android")

**iOS**

![](controls-images/heroimage-light-ios.png "Duży obraz sterowania w systemie iOS") ![](controls-images/heroimage-dark-ios.png "duży obraz sterowania w systemie iOS")


<a name="listitem" />

### <a name="listitem"></a>ListItem

`ListItem` Układ formantu jest podobny do natywnych dla systemów iOS i Android listy lub tabeli wiersze, jednak może również służyć jako regularne widok. W przykładzie kodu poniżej przedstawiono hostowanej wewnątrz `StackLayout`, ale także mogą być używane w kontrolkach listy scolling powiązanych z danymi.

Istnieje pięć właściwości:

* Tytuł
* Szczegóły
* ImageSource
* PlaceholdImageSource
* Aspekt

```xaml
<StackLayout Spacing="0">
    <pages:ListItemControl
        Detail="Xamarin"
        ImageSource="{ DynamicResource UserImage }"
        Title="Miguel de Icaza"
        PlaceholdImageSource="{ DynamicResource IconImage }"
    />
```

Te zrzuty ekranu Pokaż `ListItem` na iOS i Android przy użyciu motywy jasny i ciemny:

**Android**

![](controls-images/listitem-light-android.png "Kontroli wyszczególnij w systemie Android") ![](controls-images/listitem-dark-android.png "kontroli wyszczególnij w systemie Android")

**iOS**

![](controls-images/listitem-light-ios.png "Kontroli wyszczególnij w systemie iOS") ![](controls-images/listitem-dark-ios.png "kontroli wyszczególnij w systemie iOS")


## <a name="custom-control-example"></a>Przykład formantu niestandardowego

Instrukcje opisane w tym niestandardowych `CardView` formant jest tak, aby przypominały natywnych CardView dla systemu Android.

Będzie ona zawierać trzy właściwości:

* Tekst
* Szczegóły
* ImageSource

Celem jest niestandardowy formant, który będzie wyglądać jak poniższy kod (należy pamiętać, że niestandardowe `xmlns:local` jest wymagany, odwołuje się do bieżącego zestawu):

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

Powinien on wyglądać podobnie zrzuty ekranu poniżej z użyciem odpowiadający wbudowanych motywy jasny i ciemny motyw kolorów:

**Android**

![](controls-images/cardview-light-android.png "Kontrolka niestandardowa CardView w systemie Android") ![](controls-images/cardview-dark-android.png "kontrolkę niestandardową CardView w systemie Android")

**iOS**

![](controls-images/cardview-light-ios.png "Kontrolka niestandardowa CardView w systemie iOS") ![](controls-images/cardview-dark-ios.png "CardView kontrolkę niestandardową w systemie iOS")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>Tworzenie niestandardowych CardView

1. [Podklasy DataView](#1)
2. [Zdefiniuj czcionki, układ i marginesów](#2)
3. [Tworzenie stylów dla formantu elementów podrzędnych](#3)
4. [Tworzenie szablonu układ formantu](#4)
5. [Dodawanie zasobów specyficznych dla motywów](#5)
6. [Ustaw ControlTemplate dla klasy CardView](#6)
7. [Dodawanie formantu do strony](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1. Podklasy DataView

C# podklasę `DataView` definiuje właściwości możliwej do wiązania dla kontrolki.

```csharp
public class CardView : DataView
{
    public static readonly BindableProperty TextProperty =
        BindableProperty.Create ("Text", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Text
    {
        get { return (string)GetValue (TextProperty); }
        set { SetValue (TextProperty, value); }
    }

    public static readonly BindableProperty DetailProperty =
        BindableProperty.Create ("Detail", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Detail
    {
        get { return (string)GetValue (DetailProperty); }
        set { SetValue (DetailProperty, value); }
    }

    public static readonly BindableProperty ImageSourceProperty =
        BindableProperty.Create ("ImageSource", typeof (ImageSource), typeof (CardView), null, BindingMode.TwoWay);

    public ImageSource ImageSource
    {
        get { return (ImageSource)GetValue (ImageSourceProperty); }
        set { SetValue (ImageSourceProperty, value); }
    }

    public CardView()
    {
    }
}
```

<a name="2" />

#### <a name="2-define-font-layout-and-margins"></a>2. Zdefiniuj czcionki, układ i marginesów

Projektanta formantów może ustalić te wartości jako część projektu interfejsu użytkownika dla formantu niestandardowego. W przypadku, gdy wymaganych specyfikacjach specyficzne dla platformy `OnPlatform` element jest używany.

Należy zauważyć, że niektóre wartości odnoszą się do `StaticResource`s — te są definiowane w [kroku 5](#5).

```xml
<!-- CARDVIEW FONT SIZES -->
<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewTextFontSize">
        <On Platform="iOS, Android" Value="15" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewDetailFontSize">
        <On Platform="iOS, Android" Value="13" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewTextTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewTextTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewTextTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewTextlMargin">
        <On Platform="iOS" Value="12,10,12,4" />
        <On Platform="Android" Value="20,0,20,5" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewDetailTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewDetailTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewDetailTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewDetailMargin">
        <On Platform="iOS" Value="12,0,10,12" />
        <On Platform="Android" Value="20,0,20,20" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewBackgroundColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewBackgroundColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewBackgroundColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewShadowSize">
        <On Platform="iOS" Value="2" />
        <On Platform="Android" Value="5" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewCornerRadius">
        <On Platform="iOS" Value="0" />
        <On Platform="Android" Value="4" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewShadowColor">
        <On Platform="iOS, Android" Value="#CDCDD1" />
</OnPlatform>
```

<a name="3" />

#### <a name="3-create-styles-for-the-controls-children"></a>3. Tworzenie stylów dla formantu elementów podrzędnych

Odwołać się do wszystkich elementów, które są zdefiniowane utworzone elementy podrzędne, które będą używane w formancie niestandardowym:

```xml
<!-- EXPLICIT STYLES (will be Classes) -->
<Style TargetType="Label" x:Key="CardViewTextStyle">
    <Setter Property="FontSize" Value="{ StaticResource CardViewTextFontSize }" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewTextTextColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewTextlMargin }" />
    <Setter Property="HorizontalTextAlignment" Value="Start" />
</Style>

<Style TargetType="Label" x:Key="CardViewDetailStyle">
    <Setter Property="HorizontalTextAlignment" Value="Start" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewDetailTextColor }" />
    <Setter Property="FontSize" Value="{ StaticResource CardViewDetailFontSize }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewDetailMargin }" />
</Style>

<Style TargetType="Image" x:Key="CardViewImageImageStyle">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="Center" />
    <Setter Property="WidthRequest" Value="220"/>
    <Setter Property="HeightRequest" Value="165"/>
</Style>
```

<a name="4" />

#### <a name="4-create-the-control-layout-template"></a>4. Tworzenie szablonu układ formantu

Wygląd formantu niestandardowego jest jawnie zadeklarowana w szablonie kontrolki za pomocą zasobów zdefiniowanych powyżej:

```xml
<!--- CARDVIEW -->
<ControlTemplate x:Key="CardViewControlControlTemplate">
  <StackLayout
    Spacing="0"
    BackgroundColor="{ TemplateBinding BackgroundColor }"
  >

    <!-- CARDVIEW IMAGE -->
    <Image
      Source="{ TemplateBinding ImageSource }"
      HorizontalOptions="FillAndExpand"
      VerticalOptions="StartAndExpand"
      Aspect="AspectFill"
      Style="{ StaticResource CardViewImageImageStyle }"
    />

    <!-- CARDVIEW TEXT -->
    <Label
      Text="{ TemplateBinding Text }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewTextStyle }"
    />


    <!-- CARDVIEW DETAIL -->
    <Label
      Text="{ TemplateBinding Detail }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewDetailStyle }" />

  </StackLayout>

</ControlTemplate>
```

<a name="5" />

#### <a name="5-add-the-theme-specific-resources"></a>5. Dodawanie zasobów specyficznych dla motywów

Ponieważ jest to formant niestandardowy, należy dodać zasoby, które pasował do motywu używasz słownik zasobów:

##### <a name="light-theme-colors"></a>Kolory motywu jasny

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>Kolory motywu ciemny

```xaml
<!-- CARD VIEW COLORS -->
            <Color x:Key="iOSCardViewBackgroundColor">#404040</Color>
            <Color x:Key="AndroidCardViewBackgroundColor">#404040</Color>

            <Color x:Key="AndroidCardViewTextTextColor">#FFFFFF</Color>
            <Color x:Key="iOSCardViewTextTextColor">#FFFFFF</Color>

            <Color x:Key="AndroidCardViewDetailTextColor">#B5B4B9</Color>
            <Color x:Key="iOSCardViewDetailTextColor">#B5B4B9</Color>
```

<a name="6" />

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. Ustaw ControlTemplate dla klasy CardView

Na koniec upewnij się, klasa C# utworzone w [kroku 1](#1) używa zdefiniowane w szablonie kontrolki [kroku 4](#4) przy użyciu `Style` `Setter` — element

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7. Dodawanie formantu do strony

`CardView` Kontroli można teraz dodać do strony. W poniższym przykładzie pokazano jej hostowanych w `StackLayout`:

```xaml
<StackLayout Spacing="0">
  <local:CardView
    Margin="12,6"
    ImageSource="{ DynamicResource CardViewImage }"
    Text="CardView Text"
    Detail="CardView Detail"
  />
</StackLayout>

```
