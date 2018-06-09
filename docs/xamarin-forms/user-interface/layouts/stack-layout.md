---
title: StackLayout platformy Xamarin.Forms
description: W tym artykule opisano sposób użycia klasy StackLayout platformy Xamarin.Forms do prezentowania kolekcji widoków za pośrednictwem jednego wymiaru.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 6e278c466c352ad19575cd3a84d6e38e14ec2587
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244600"
---
# <a name="xamarinforms-stacklayout"></a>StackLayout platformy Xamarin.Forms

`StackLayout` organizuje widoków w wierszu jednowymiarowa ("stosu"), poziomo czy pionowo. Wyświetla `StackLayout` może ustalać w oparciu miejsca w układzie przy użyciu opcji układu. Pozycjonowanie jest określana przez kolejność, według której widoki zostały dodane do układu i opcje układu widoki.

[![](stack-layout-images/layouts-sml.png "Układy platformy Xamarin.Forms")](stack-layout-images/layouts.png#lightbox "układów platformy Xamarin.Forms")

## <a name="purpose"></a>Cel

`StackLayout` jest mniej złożona niż innych widoków. Proste interfejsy liniowej można utworzyć przez dodanie tylko widoki `StackLayout`i bardziej złożonej interfejsy utworzonych przez nich zagnieżdżania.

## <a name="usage--behavior"></a>Użycie & zachowanie

### <a name="spacing"></a>Odstępy

Domyślnie `StackLayout` doda margines 6px między widokami. To kontrolowane lub ustawić dla bez marginesu przez ustawienie `Spacing` właściwość StackLayout. Poniżej pokazano, jak ustawić odstępy i wpływu opcje odstępów różne:

W języku XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout Spacing="10" x:Name="layout">
      <Button Text="StackLayout" VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView Color="Yellow" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView Color="Green" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" Color="Blue" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

W języku C#:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { Text = "StackLayout", VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var yellowBox = new BoxView { Color = Color.Yellow, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var greenBox = new BoxView { Color = Color.Green, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var blueBox = new BoxView { Color = Color.Blue, VerticalOptions = LayoutOptions.FillAndExpand,
      HorizontalOptions = LayoutOptions.FillAndExpand, HeightRequest = 75 };

    layout.Children.Add(button);
    layout.Children.Add(yellowBox);
    layout.Children.Add(greenBox);
    layout.Children.Add(blueBox);
    layout.Spacing = 10;
    Content = layout;
  }
}
```

Odstępy = 0:

![](stack-layout-images/spacing-zero.png "StackLayout z odstępami = 0")

Odstępy 10:

![](stack-layout-images/spacing-ten.png "StackLayout z odstępami = 10")

### <a name="sizing"></a>Zmiana rozmiaru

Rozmiar widoku w StackLayout zależy od tego, zarówno wysokość i szerokość żądań i opcje układu. `StackLayout` będzie wymuszać dopełnienia. Następujące `LayoutOption`s spowoduje, że widoków zająć tyle miejsca jest dostępne z układu:

- **CenterAndExpand** &ndash; Wyśrodkowuje widoku w układzie i rozwija aby zająć tyle miejsca zapewni jego układ.
- **EndAndExpand** &ndash; umieszcza widok na końcu układu (u dołu lub prawej krawędzi granicy) i rozwija aby zająć tyle miejsca zapewni jego układu.
- **FillAndExpand** &ndash; umieszcza widoku, dzięki czemu nie ma żadnych uzupełnienie i zajmuje tyle miejsca układ zapewni on.
- **StartAndExpand** &ndash; umieszcza widok na początku układ i zajmuje tyle miejsca zapewni nadrzędnego.

Aby uzyskać więcej informacji, zobacz [rozszerzenia](~/xamarin-forms/user-interface/layouts/layout-options.md#expansion).

### <a name="positioning"></a>Pozycjonowanie

Widoki StackLayout może być umieszczony i zmieniać ich rozmiar przy użyciu `LayoutOptions`. Każdy widok można przydzielić `VerticalOptions` i `HorizontalOptions`, określające, jak widoki będzie pozycjonują się względem układu. Następujące wstępnie zdefiniowane `LayoutOptions` są dostępne:

- **Centrum** &ndash; Wyśrodkowuje widoku w układzie.
- **Końcowy** &ndash; umieszcza widok na końcu układu (u dołu lub prawej krawędzi granicy).
- **Wypełnij** &ndash; umieszcza widok, aby nie dopełnienia.
- **Uruchom** &ndash; umieszcza widok na początku układu.

Poniższy kod przedstawia opcje układu ustawienia:

W języku XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout x:Name="layout">
      <Button VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

W języku C#:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var oneBox = new BoxView { VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var twoBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var threeBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };

    layout.Children.Add(button);
    layout.Children.Add(oneBox);
    layout.Children.Add(twoBox);
    layout.Children.Add(threeBox);
    Content = layout;
  }
}
```

Aby uzyskać więcej informacji, zobacz [wyrównanie](~/xamarin-forms/user-interface/layouts/layout-options.md#alignment).

## <a name="exploring-a-complex-layout"></a>Poznawanie złożonego układu

Układy mieć najważniejsze wady i zalety tworzenia konkretnego układów. W tej serii artykułów układu Przykładowa aplikacja została utworzona z ten sam układ strony implementowane przy użyciu trzech różnych układów.

Należy wziąć pod uwagę następujące XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.StackLayoutPage"
BackgroundColor="Maroon"
Title="StackLayouts">
    <ContentPage.Content>
    <ScrollView>
      <StackLayout Spacing="0" Padding="0" BackgroundColor="Maroon">
        <BoxView HorizontalOptions="FillAndExpand" HeightRequest="100"
          VerticalOptions="Start" Color="Gray" />
        <Button BorderRadius="30" HeightRequest="60" WidthRequest="60"
          BackgroundColor="Red" HorizontalOptions="Center" VerticalOptions="Start" />
        <StackLayout HeightRequest="100" VerticalOptions="Start" HorizontalOptions="FillAndExpand" Spacing="20" BackgroundColor="Maroon">
          <Label Text="User Name" FontSize="28" HorizontalOptions="Center"
            VerticalOptions="Center" FontAttributes="Bold" />
          <Entry Text="Bio + Hashtags" TextColor="White"
            BackgroundColor="Maroon" HorizontalOptions="FillAndExpand" VerticalOptions="CenterAndExpand" />
        </StackLayout>
        <StackLayout Orientation="Horizontal" HeightRequest="50" BackgroundColor="White" Padding="5">
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="Start">
            <BoxView BackgroundColor="Black" WidthRequest="40" HeightRequest="40"  HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="EndAndExpand">
            <BoxView BackgroundColor="Maroon" WidthRequest="40" HeightRequest="40" HorizontalOptions="Start" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Age:" TextColor="White" HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="35" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Interests:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100"/>
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin, C#, .NET, Mono..." TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
      </StackLayout>
    </ScrollView>
    </ContentPage.Content>
</ContentPage>

```

Powyższy kod wyniki w układzie następujące:

![](stack-layout-images/stack.png "StackLayout złożonych")

Zwróć uwagę, że `StackLayouts`s są zagnieżdżone, ponieważ w niektórych przypadkach zagnieżdżania układów mogą być łatwiejsze niż przedstawienie wszystkich elementów w ten sam układ. Ponadto, ponieważ `StackLayout` nie obsługuje elementów nakładające się Strona nie ma niceties układu znaleźć niektórych dla innych układów na stronach.



## <a name="related-links"></a>Linki pokrewne

- [LayoutOptions](~/xamarin-forms/user-interface/layouts/layout-options.md)
- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
