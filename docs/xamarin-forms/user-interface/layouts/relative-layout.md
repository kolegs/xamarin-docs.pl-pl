---
title: RelativeLayout platformy Xamarin.Forms
description: W tym artykule opisano sposób użycia klasy RelativeLayout platformy Xamarin.Forms można utworzyć pakietu, które Skaluj do rozmiaru ekranu.
ms.prod: xamarin
ms.assetid: 2530BCB8-01B8-4C4F-BF14-CA53659F1B5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 712092e58a7a7358ba1fa808614822c7988e6105
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245058"
---
# <a name="xamarinforms-relativelayout"></a>RelativeLayout platformy Xamarin.Forms

`RelativeLayout` Służy do pozycji i rozmiaru widoków względem właściwości układu lub element równorzędny widoków. W odróżnieniu od `AbsoluteLayout`, `RelativeLayout` nie ma koncepcji ruchomej zakotwiczenia i nie ma urządzeń do rozmieszczania elementów do dołu lub prawej krawędzi układu. `RelativeLayout` obsługuje położenia elementów poza granice własny.

[![](relative-layout-images/layouts-sml.png "Układy platformy Xamarin.Forms")](relative-layout-images/layouts.png#lightbox "układów platformy Xamarin.Forms")

## <a name="purpose"></a>Cel

`RelativeLayout` można umieścić widoków na ekranie względem ogólny układ lub z innymi widokami.

![](relative-layout-images/flag.png "Eksploracja RelativeLayout")

## <a name="usage"></a>Użycie

### <a name="understanding-constraints"></a>Opis ograniczeń

Pozycjonowanie i rozmiary widoku w `RelativeLayout` odbywa się z ograniczeniami. Wyrażenie ograniczenia może zawierać następujące informacje:

- **Typ** &ndash; czy ograniczenie jest względem nadrzędnego, lub do innego widoku.
- **Właściwość** &ndash; właściwości, które ma być używana jako podstawa dla ograniczenia.
- **Współczynnik** &ndash; współczynnik stosowany do wartości właściwości.
- **Stała** &ndash; wartość przesunięcia wartości.
- **ElementName** &ndash; nazwę widoku, który jest ograniczenie względem.

W języku XAML, ograniczenia są wyrażane jako `ConstraintExpression`s. Rozważmy następujący przykład:

```xaml
<BoxView Color="Green" WidthRequest="50" HeightRequest="50"
    RelativeLayout.XConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Width,
                             Factor=0.5,
                             Constant=-100}"
    RelativeLayout.YConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Height,
                             Factor=0.5,
                             Constant=-100}" />
```

W języku C# ograniczenia wyrażane są nieco inaczej, przy użyciu funkcji zamiast wyrażenia w widoku. Ograniczenia są określone jako argumenty w układzie `Add` metody:

```csharp
layout.Children.Add(box, Constraint.RelativeToParent((parent) =>
    {
      return (.5 * parent.Width) - 100;
    }),
    Constraint.RelativeToParent((parent) =>
    {
        return (.5 * parent.Height) - 100;
    }),
    Constraint.Constant(50), Constraint.Constant(50));
```

Należy uwzględnić następujące aspekty układu powyżej:

- `x` i `y` ograniczenia są określane za pomocą ich własnych ograniczenia.
- W języku C# ograniczenia względne są definiowane jako funkcje. Pojęcia, takich jak `Factor` nie istnieje, ale może być zaimplementowany ręcznie.
- Pola `x` współrzędnych jest zdefiniowany jako pełnej szerokości elementu nadrzędnego, -100.
- Pola `y` współrzędnych jest zdefiniowany jako połowie wysokości elementu nadrzędnego, -100.

> [!NOTE]
> Ze względu na sposób ograniczeń jest możliwość bardziej złożonych układów w języku C# nie można określić języka XAML.

Obu powyższych przykładach definiowanie ograniczeń jako `RelativeToParent` &ndash; oznacza to, że ich wartości są względem elementu nadrzędnego. Istnieje również możliwość zdefiniowania ograniczenia jako względem innego widoku. To umożliwia układów bardziej intuicyjne (do deweloperów), a można wprowadzić bardziej dostrzegalne celem kodu układu.

Należy wziąć pod uwagę układu, w której jeden element musi być niższa od innego 20 pikseli. Jeśli oba elementy są zdefiniowane z wartości stałych, może mieć niższy jego `Y` ograniczenie zdefiniowane jako stała, która jest większa niż 20 pikseli `Y` ograniczenie elementu wyższy. Takie podejście znajduje się krótki, jeśli wyższej element znajduje się za pomocą w stosunku tak, aby nie jest znana rozmiar pikseli. W takim przypadku ograniczający element na podstawie pozycji inny element jest bardziej niezawodną metodą:

```xaml
<RelativeLayout>
    <BoxView Color="Red" x:Name="redBox"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent,
            Property=Height,Factor=.15,Constant=0}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=1,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.8,Constant=0}" />
    <BoxView Color="Blue"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=Y,Factor=1,Constant=20}"
        RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=X,Factor=1,Constant=20}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=.5,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.5,Constant=0}" />
</RelativeLayout>
```

Aby osiągnąć ten sam układ w języku C#:

```csharp
layout.Children.Add (redBox, Constraint.RelativeToParent ((parent) => {
        return parent.X;
    }), Constraint.RelativeToParent ((parent) => {
        return parent.Y * .15;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .8;
    }));
layout.Children.Add (blueBox, Constraint.RelativeToView (redBox, (Parent, sibling) => {
        return sibling.X + 20;
    }), Constraint.RelativeToView (blueBox, (parent, sibling) => {
        return sibling.Y + 20;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width * .5;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .5;
    }));
```

To spowoduje utworzenie następujących danych wyjściowych z pozycji niebieskie pole ustalone _względną_ do położenia czerwonym prostokątem:

![](relative-layout-images/red-blue-box.png "RelativeLayout z BoxViews czerwona i niebieska")

### <a name="sizing"></a>Zmiana rozmiaru

Widoki, którego układ określa się `RelativeLayout` są dostępne dwie opcje do określania ich rozmiar:

- `HeightRequest & WidthRequest`
- `RelativeLayout.WidthConstraint` & `RelativeLayout.HeightConstraint`

`HeightRequest` i `WidthRequest` określić zamierzone wysokość i szerokość widoku, ale mogą zostać zastąpione przez układów zgodnie z potrzebami. `WidthConstraint` i `HeightConstraint` obsługuje ustawiania wysokość i szerokość jako wartość właściwości układu lub inny widok lub jako wartość stałą.

## <a name="exploring-a-complex-layout"></a>Poznawanie złożonego układu
Układy mieć najważniejsze wady i zalety tworzenia konkretnego układów. W tej serii artykułów układu Przykładowa aplikacja została utworzona z ten sam układ strony implementowane przy użyciu trzech różnych układów.

Należy wziąć pod uwagę następujące XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.RelativeLayoutPage"
BackgroundColor="Maroon"
Title="RelativeLayout">
    <ContentPage.Content>
    <ScrollView>
      <RelativeLayout>
        <BoxView Color="Gray" HeightRequest="100"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Button BorderRadius="35" x:Name="imageCircleBack"
            BackgroundColor="Maroon" HeightRequest="70" WidthRequest="70" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.5, Constant = -35}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=70}" />
        <Button BorderRadius="30" BackgroundColor="Red" HeightRequest="60"
            WidthRequest="60" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=imageCircleBack, Property=X, Factor=1,Constant=5}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=75}" />
        <Label Text="User Name" FontAttributes="Bold" FontSize="26"
            HorizontalTextAlignment="Center" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=140}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Entry Text="Bio + Hashtags" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=180}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <RelativeLayout BackgroundColor="White" RelativeLayout.YConstraint="
            {ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=220}" HeightRequest="60" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" >
            <BoxView BackgroundColor="Black" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=5}" />
            <BoxView BackgroundColor="Maroon" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=}" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=60}" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=55}" />
        </RelativeLayout>
        <RelativeLayout Padding="5,0,0,0">
          <Label FontSize="14" Text="Age:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=305}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=280}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Interests:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=345}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=320}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
            LineBreakMode="WordWrap"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=395}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
            BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=370}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
      </RelativeLayout>
    </ScrollView>
  </ContentPage.Content>
</ContentPage>
```

Powyższy kod wyniki w układzie następujące:

![](relative-layout-images/relative.png "RelativeLayout złożonych")

Zwróć uwagę, że `RelativeLayouts`s są zagnieżdżone, ponieważ w niektórych przypadkach zagnieżdżania układów mogą być łatwiejsze niż przedstawienie wszystkich elementów w ten sam układ. Również powiadomienie, że niektóre elementy są `RelativeToView`, ponieważ umożliwia łatwiejsze i bardziej intuicyjne układu po relacje między widokami przewodnik rozmieszczania.


## <a name="related-links"></a>Linki pokrewne

- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
