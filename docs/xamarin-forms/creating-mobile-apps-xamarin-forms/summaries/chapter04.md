---
title: Podsumowanie rozdziału 4. Przewijanie stosu
description: 'Tworzenie aplikacji mobilnych za pomocą platformy Xamarin.Forms: Podsumowanie rozdziału 4. Przewijanie stosu'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 1af3632d226ce894c1d856f665d6482f45d61f16
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241145"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Podsumowanie rozdziału 4. Przewijanie stosu

W tym rozdziale przede wszystkim jest przeznaczony do wprowadzenia koncepcji *układu*, która jest ogólny termin dla klasy i metody używane do organizowania wyświetlania wiele widoków na stronie platformy Xamarin.Forms.

Układ obejmuje kilka klas, które pochodzą z [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) i [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/). W tym rozdziale skupiono się na [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

Wprowadzono również w tym rozdziale są [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/), [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/), i [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) klasy.

## <a name="stacks-of-views"></a>Stosy widoków

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) pochodną `Layout<View>` i dziedziczy [ `Children` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) właściwości typu `IList<View>`. Dodaj wiele elementów widoku do tej kolekcji i `StackLayout` wyświetla je w stosie pozioma lub pionowa.

Ustaw [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) właściwość `StackLayout` do elementu członkowskiego [ `StackOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackOrientation/) wyliczenia, albo [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Vertical/) lub [ `Horizontal`](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Horizontal/). Wartość domyślna to `Vertical`.

Ustaw [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) właściwość `StackLayout` do `double` wartość, aby określić odstęp między elementami podrzędnymi. Wartość domyślna to 6.

W kodzie, można dodać elementy do `Children` Kolekcja `StackLayout` w `for` lub `foreach` pętli, jak pokazano w [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) przykładowych albo można zainicjować `Children` kolekcji z listą poszczególnych widoków jako wykazały w [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Elementy podrzędne muszą pochodzić od `View` , ale może zawierać inne `StackLayout` obiektów.

## <a name="scrolling-content"></a>Przewijanie zawartości

Jeśli `StackLayout` zawiera zbyt wiele elementów podrzędnych wyświetlanych na stronie, które można wprowadzić `StackLayout` w [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) umożliwia przewijanie.

Ustaw [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) właściwość `ScrollView` do widoku, aby przewijać. Jest to często `StackLayout`, ale może być dowolnym widoku.

Ustaw [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) właściwość `ScrollView` do elementu członkowskiego [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/) właściwość [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Vertical/), [ `Horizontal` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Horizontal/), lub [ `Both` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Both/). Wartość domyślna to `Vertical`. Jeśli zawartość `ScrollView` jest `StackLayout`, dwa orientacje powinny być zgodne.

[ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) przykładzie przedstawiono użycie `ScrollView` i `StackLayout` do wyświetlenia dostępnych kolorów. Próbki także przedstawiono sposób użycia odbicia .NET można uzyskać wszystkie publiczne statyczne właściwościami i polami `Color` struktury bez konieczności jawnie umieszczone na liście.

## <a name="the-expands-option"></a>Opcja Expands

Gdy `StackLayout` stosy zajmuje jej funkcji podrzędnych każdego elementu podrzędnego określonego miejsca w ramach całkowitej wysokości `StackLayout` to zależy od rozmiaru dziecka i ustawienia z jego `HorizontalOptions` i `VerticalOptions` właściwości. Te właściwości są przypisane wartości typu [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/).

`LayoutOptions` Struktury definiuje dwie właściwości:

- [`Alignment`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) typu wyliczenia [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/) z czterech członków [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), i [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/)
- [`Expands`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) typu `bool`

Dla wygody `LayoutOptions` struktury definiuje również osiem statycznego pola tylko do odczytu typu `LayoutOptions` które obejmują wszystkie kombinacje dwa wystąpienia właściwości:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

Obejmuje następujące dyskusji `StackLayout` w orientacji pionowej domyślne. Poziomu `StackLayout` jest analogiczna.

Dla pionowym `StackLayout`, `HorizontalOptions` ustawienie określa poziomie położenie elementu podrzędnego względem szerokości elementu `StackLayout`. `Alignment` Ustawienie `Start`, `Center`, lub `End` powoduje, że podrzędnych się poziomo nieograniczonego. Określa szerokość własnego elementu podrzędnego, a znajduje się w lewej, center lub prawej `StackLayout`. `Fill` Opcja powoduje, że podrzędnych na poziomie można ograniczyć i wypełnia szerokość `StackLayout`.

Dla pionowym `StackLayout`każdego elementu podrzędnego jest pionowo nieograniczonego i pobiera pionowym gniazdo w zależności od wysokość dziecka, w którym to przypadku `VerticalOptions` ustawienie nie ma znaczenia.

Jeśli pionową `StackLayout` jest nieograniczonego&mdash;czyli jeśli jego `VerticalOptions` jest ustawienie `Start`, `Center`, lub `End`, następnie wysokość `StackLayout` jest całkowita wysokość jego elementów podrzędnych.

Jednak jeśli pionową `StackLayout` pionowo jest ograniczane&mdash;jeśli jego `VerticalOptions` jest ustawienie `Fill` &mdash;następnie wysokość `StackLayout` będzie wysokość jego kontenera, który może być większa niż suma wysokość jego elementów podrzędnych. Jeśli tak jest, a ma co najmniej jeden element podrzędny `VerticalOptions` ustawienie `Expands` flagę `true`, następnie dodatkowe miejsce w `StackLayout` przydzielania jednakowo wszystkie te elementy podrzędne z `Expands` flagę `true`. Całkowita wysokość elementu podrzędnego następnie będzie równa wysokość `StackLayout`i `Alignment` częścią `VerticalOptions` ustawienie określa, jak dziecka zostanie umieszczone w jego miejscu.

To jest przedstawiona w [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) próbki.

## <a name="frame-and-boxview"></a>Ramka i BoxView

Te dwa widoki prostokątne są często używane na potrzeby prezentacji.

[ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) Widoku są wyświetlane prostokątne ramki wokół innego widoku, które mogą być takie jak układ `StackLayout`. `Frame` dziedziczy [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) właściwość z [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) ustawionej dla widoku do wyświetlenia w ramach `Frame`. `Frame` Jest niewidoczny domyślnie. Ustaw następujące trzy właściwości, aby dostosować wygląd ramki:

- [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/) Właściwości, aby go wyświetlić. Często można ustawić `OutlineColor` do `Color.Accent` Jeśli nie znasz podstawowy schemat kolorów.
- [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/) Ustawioną właściwość `true` do wyświetlenia czarny cienia na urządzeniach z systemem iOS.
- Ustaw [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) właściwości `Thickness` wartość oddzielone spacjami ramki i ramki jego zawartości. Wartość domyślna to 20 jednostek ze wszystkich stron.

`Frame` Ma domyślne `HorizontalOptions` i `VerticalOptions` wartości `LayoutOptions.Fill`, co oznacza, że `Frame` spowoduje wypełnienie jego kontenera. Z innymi ustawieniami rozmiar `Frame` opiera się na rozmiar jego zawartości.

`Frame` Jest przedstawiona w [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) próbki.

[ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Wyświetla prostokątny obszar koloru określony przez jego [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) właściwości.

Jeśli `BoxView` jest ograniczone (jego `HorizontalOptions` i `VerticalOptions` właściwości mają ich ustawień domyślnych `LayoutOptions.Fill`), `BoxView` wypełnienia dostępnego miejsca dla niego. Jeśli `BoxView` jest nieograniczonego (z `HorizontalOptions` i `LayoutOptions` ustawienia `Start`, `Center`, lub `End`), ma wymiar domyślny kwadratu 40 jednostki. A `BoxView` może być ograniczony w jednym wymiarze i nieograniczonego w innym.

Często, będzie można ustawić [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) i [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) właściwości `BoxView` umożliwiają określonego rozmiaru. Jest to zilustrowane [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) próbki.

Można użyć kilku wystąpień `StackLayout` połączyć `BoxView` i kilka `Label` wystąpień w `Frame` do wyświetlania określonego koloru, a następnie umieść każdy z tych widoków w `StackLayout` w `ScrollView` utworzyć atrakcyjne Lista kolorów wyświetlany w [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) próbki:

[![Potrójna zrzut ekranu przedstawiający bloki kolorów](images/ch04fg11-small.png "listy kolory")](images/ch04fg11-large.png#lightbox "listy kolorów")

## <a name="a-scrollview-in-a-stacklayout"></a>ScrollView w StackLayout?

Umieszczanie `StackLayout` w `ScrollView` wspólne, ale odkładanie `ScrollView` w `StackLayout` jest również czasami. Teoretycznie ten nie powinny być możliwe ponieważ pionowej elementy podrzędne `StackLayout` są nieograniczonego pionowo. Ale `ScrollView` pionowo ograniczone. Musi ona zawierać określonej wysokości tak, aby następnie można określić rozmiaru podrzędnej przewijania.

Lewy jest nadanie `ScrollView` podrzędnym `StackLayout` `VerticalOptions` ustawienie `FillAndExpand`. To jest przedstawiona w [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) próbki.

**BlackCat** próbki również pokazano, jak zdefiniować i uzyskiwać dostęp do zasobów programu, które są osadzone w przenośnej biblioteki klas (PCL). Można to osiągnąć również z udostępnionych projektów zasobów (protokoły SAP), ale proces jest nieco trudniejszy jako [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) przykładzie pokazano.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 4 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Przykłady rozdział 4](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Przykłady rozdział 4 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
