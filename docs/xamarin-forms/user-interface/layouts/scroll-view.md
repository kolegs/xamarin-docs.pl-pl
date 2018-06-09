---
title: ScrollView platformy Xamarin.Forms
description: W tym artykule opisano sposób użycia klasy ScrollView platformy Xamarin.Forms do prezentowania układów, który nie mieści się na tylko jedną ekranu i zawartości zwolnić miejsce na klawiaturze.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2016
ms.openlocfilehash: 72897013842d464ff9d46825e2b111efbaeb79b8
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245237"
---
# <a name="xamarinforms-scrollview"></a>ScrollView platformy Xamarin.Forms

[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) zawiera układy i umożliwia przewijania niewidocznej. `ScrollView` jest również stosowane do umożliwienia widoków, które można automatycznie przełączona na widoczną część ekranu, gdy jest pokazywany klawiatury.

[![](scroll-view-images/layouts-sml.png "Układy platformy Xamarin.Forms")](scroll-view-images/layouts.png#lightbox "układów platformy Xamarin.Forms")

W tym artykule omówiono:

- **[Cel](#Purpose)**  &ndash; cel `ScrollView` i gdy jest używana.
- **[Użycie](#Usage)**  &ndash; sposób użycia `ScrollView` w praktyce.
- **[Właściwości](#Properties)**  &ndash; właściwości publiczne, które mogą odczytywać i modyfikować.
- **[Metody](#Methods)**  &ndash; metody publiczne, które mogą być wywoływane do przewijania widoku.
- **[Zdarzenia](#Events)**  &ndash; zdarzenia, które mogą służyć do nasłuchiwania na zmiany w widoku stanów.

## <a name="purpose"></a>Cel

`ScrollView` może służyć do upewnij się, że większych widokach są wyświetlane prawidłowo na mniejsze telefonów. Na przykład układu, która działa na telefonie iPhone 6s może zostać obcięty na telefonie iPhone 4s. Przy użyciu `ScrollView` umożliwiałyby przyciętą części układu, który będzie wyświetlany na ekranie mniejsze.

## <a name="usage"></a>Użycie

> [!NOTE]
> `ScrollView`s nie powinien być zagnieżdżone. Ponadto `ScrollView`s nie powinny być zagnieżdżane z innym kontrolki udostępniające przewijanie, tak samo, jak `ListView` i `WebView`.

`ScrollView` przedstawia `Content` właściwość, którą można ustawić jednego widoku ani układu. Należy wziąć pod uwagę w tym przykładzie układ z bardzo dużych boxView, a następnie `Entry`:

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

W języku C#:

```csharp
var scroll = new ScrollView();
Content = scroll;
var stack = new StackLayout();
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,    HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

Zanim użytkownik przewija widok w dół, tylko `BoxView` jest widoczna:

![](scroll-view-images/scroll-start.png "BoxView w ScrollView")

Zwróć uwagę, że gdy użytkownik uruchomi wprowadzania tekstu w `Entry`, Przewija widok, aby widoczne na ekranie:

![](scroll-view-images/scroll-end.png "Wpis w ScrollView")

## <a name="properties"></a>Właściwości

ScrollView ma następujące właściwości:

- **Zawartości** &ndash; pobiera lub ustawia widok w celu wyświetlenia w `ScrollView`.
- **[ContentSize](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/)**  &ndash; tylko do odczytu, pobiera zawartość, która ma składników szerokości i wysokości rozmiaru. Jest to właściwość powiązania
- **[Orientacja](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)**  &ndash; jest [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/), czyli może być ustawiony na wyliczenie `Horizontal`, `Vertical`, lub `Both`.
- **ScrollX** &ndash; tylko do odczytu, pobiera bieżącą pozycję przewijania w wymiarze X.
- **ScrollY** &ndash; tylko do odczytu, pobiera bieżącą pozycję przewijania w wymiarze Y.

## <a name="methods"></a>Metody

`ScrollView` udostępnia `ScrollToAsync` metodę, która może służyć do przewijania widoku przy użyciu współrzędnych albo określenie określonego widoku, które powinny być widoczne.

Korzystając z współrzędne, określ `x` i `y` współrzędne, wraz z wartość boolean wskazującą, czy powinny być animowane przewijania:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

Podczas przewijania na dany element `ScrollToPosition` wyliczenie Określa, gdzie w widoku element będzie wyświetlany jako:

- **Centrum** &ndash; Przewija elementu do środka widoczną część widoku.
- **Końcowy** &ndash; Przewija element na końcu widoczną część widoku.
- **MakeVisible** &ndash; Przewija elementu, tak aby była widoczna w widoku.
- **Uruchom** &ndash; Przewija element na początku widoczną część widoku.

`IsAnimated` Właściwość określa, jak być przewijane widoku. Gdy ma wartość true, smooth animacji będzie używany, zamiast natychmiast przenoszenia zawartości do widoku.

## <a name="events"></a>Zdarzenia

`ScrollView` przedstawia tylko jedno zdarzenie `Scrolled`. `Scrolled` jest wywoływane, gdy widok zakończył przewijania. Program obsługi zdarzeń dla `Scrolled` przyjmuje `ScrolledEventArgs`, który ma `ScrollX` i `ScrollY` właściwości. Następujące pokazano, jak zaktualizować etykiety w bieżącym położeniu przewijania `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Należy pamiętać, że położenie przewijania może być ujemna, ze względu na wpływ podskokiem podczas przewijania na końcu listy.


## <a name="related-links"></a>Linki pokrewne

- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
