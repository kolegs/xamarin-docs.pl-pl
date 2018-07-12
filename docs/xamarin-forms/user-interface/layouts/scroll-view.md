---
title: ScrollView zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak korzystać z zestawu narzędzi Xamarin.Forms ScrollView klasy do przedstawienia układów, który nie mieści się na tylko jeden ekran i zawartości zwolnić miejsce na klawiaturze.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 814b74ce04269d18b9a280ada74204c1c86621e1
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38985981"
---
# <a name="xamarinforms-scrollview"></a>ScrollView zestawu narzędzi Xamarin.Forms

[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) zawiera układy i umożliwia przewijanie poza ekranem. `ScrollView` Umożliwia również widoki automatycznego przenoszenia do widocznej części ekranu, gdy klawiatura jest są wyświetlane.

[![](scroll-view-images/layouts-sml.png "Układy platformy Xamarin.Forms")](scroll-view-images/layouts.png#lightbox "układy platformy Xamarin.Forms")

W tym artykule omówiono:

- **[Cel](#Purpose)**  &ndash; cel `ScrollView` i gdy jest używany.
- **[Użycie](#Usage)**  &ndash; sposób używania `ScrollView` w praktyce.
- **[Właściwości](#Properties)**  &ndash; właściwości publiczne, które mogą odczytywać i modyfikować.
- **[Metody](#Methods)**  &ndash; metody publiczne, które mogą być wywoływane do przewijania widoku.
- **[Zdarzenia](#Events)**  &ndash; zdarzenia, które mogą służyć do nasłuchiwania na zmiany stanów widoku.

## <a name="purpose"></a>Cel

`ScrollView` może służyć do upewnij się, że większych widokach są wyświetlane na telefonach mniejsze. Na przykład układ, który działa na telefonie iPhone 6s mogą zostać obcięte na telefonie iPhone 4s. Za pomocą `ScrollView` pozwoliłoby obciętych części układu, które mają być wyświetlane na ekranie mniejsze.

## <a name="usage"></a>Użycie

> [!NOTE]
> `ScrollView`s nie powinny być zagnieżdżane. Ponadto `ScrollView`s nie powinny być zagnieżdżane z innymi formantami, które zapewniają przewijanie, takie jak `ListView` i `WebView`.

`ScrollView` udostępnia `Content` właściwość można ustawić w jednym widoku ani układu. Rozważmy następujący przykład układ z boxView bardzo duże, a następnie `Entry`:

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

Zanim użytkownik przewija widok w dół, tylko `BoxView` jest widoczny:

![](scroll-view-images/scroll-start.png "BoxView w ScrollView")

Należy zauważyć, że gdy użytkownik zacznie wpisywać tekst w `Entry`, widok przewija się, aby zachować widoczność na ekranie:

![](scroll-view-images/scroll-end.png "Wpis w ScrollView")

## <a name="properties"></a>Właściwości

`ScrollView` definiuje następujące właściwości:

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) pobiera [ `Size` ](xref:Xamarin.Forms.Size) wartość, która reprezentuje rozmiar zawartości.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) Pobiera lub ustawia [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) wartość wyliczenia, która reprezentuje przewijany kierunek `ScrollView`.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) pobiera `double` reprezentujący bieżące położenie przewijania X.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) pobiera `double` reprezentujący bieżące położenie przewijania osi Y.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) Pobiera lub ustawia [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) wartość, która reprezentuje gdy poziomy pasek przewijania jest widoczny.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) Pobiera lub ustawia [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) wartość, która reprezentuje gdy pionowy pasek przewijania jest widoczny.

## <a name="methods"></a>Metody

`ScrollView` udostępnia `ScrollToAsync` metody, która może służyć do przewijania widoku przy użyciu współrzędnych lub określenie określonego widoku, które powinny być widoczne.

Podczas używania współrzędne, określić `x` i `y` współrzędne, oraz wartość boolean wskazującą, czy powinien być animowane przewijania:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

Podczas przewijania do konkretnego elementu `ScrollToPosition` określa wyliczenie, gdzie w widoku element będzie wyświetlany jako:

- **Centrum** &ndash; Przewija element do środka widoczne części widoku.
- **Koniec** &ndash; Przewija element na końcu widoczne części widoku.
- **MakeVisible** &ndash; Przewija elementu, tak aby nie jest widoczny w widoku.
- **Rozpocznij** &ndash; Przewija elementu do początku widoczne części widoku.

`IsAnimated` Właściwość określa, jak być przewijane widoku. Gdy wartość true, płynne animacje będzie używany, zamiast natychmiast przenoszenie zawartości do widoku.

## <a name="events"></a>Zdarzenia

`ScrollView` Definiuje tylko jedno zdarzenie, `Scrolled`. `Scrolled` jest wywoływane, gdy widok zostało zakończone, przewijania. Program obsługi zdarzeń dla `Scrolled` przyjmuje `ScrolledEventArgs`, który ma `ScrollX` i `ScrollY` właściwości. Następujące pokazuje, jak zaktualizować etykiety w bieżącym położeniu przewijania `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Należy pamiętać, że położenie przewijania mogą być ujemne, ze względu na wpływ odrzucenie podczas przewijania na końcu listy.


## <a name="related-links"></a>Linki pokrewne

- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
