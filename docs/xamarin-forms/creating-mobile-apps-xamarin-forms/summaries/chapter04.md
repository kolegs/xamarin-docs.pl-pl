---
title: Podsumowanie rozdziałów 4. Przewijanie stosu
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziału 4. Przewijanie stosu'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 49f2d96fb7f95ab880d5cfafa420afbbe933c1ad
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156720"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Podsumowanie rozdziałów 4. Przewijanie stosu

W tym rozdziale przede wszystkim jest poświęcona wprowadzenie do koncepcji *układ*, który jest ogólny termin dla klas i technik, które korzysta z zestawu narzędzi Xamarin.Forms do organizowania wyświetlania wielu widoków na stronie.

Układ obejmuje kilka klas, które wynikają z [ `Layout` ](xref:Xamarin.Forms.Layout) i [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1). W tym rozdziale koncentruje się na [ `StackLayout` ](xref:Xamarin.Forms.StackLayout).

> [!NOTE]
> [ `FlexLayout` ](~/xamarin-forms/user-interface/layouts/flex-layout.md) Wprowadzona w Xamarin.Forms 3.0 mogą być używane w sposób podobny do `StackLayout` , ale z większą elastyczność.

Również wprowadzone w tym rozdziale są [ `ScrollView` ](xref:Xamarin.Forms.ScrollView), [ `Frame` ](xref:Xamarin.Forms.Frame), i [ `BoxView` ](xref:Xamarin.Forms.BoxView) klasy.

## <a name="stacks-of-views"></a>Stosy widoków

[`StackLayout`](xref:Xamarin.Forms.StackLayout) pochodzi od klasy `Layout<View>` i dziedziczy [ `Children` ](xref:Xamarin.Forms.Layout`1) właściwości typu `IList<View>`. Dodaj wiele elementów w widoku do tej kolekcji, a `StackLayout` wyświetli je w stosie poziomej lub pionowej.

Ustaw [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) właściwość `StackLayout` do elementu członkowskiego [ `StackOrientation` ](xref:Xamarin.Forms.StackOrientation) wyliczenia, albo [ `Vertical` ](xref:Xamarin.Forms.StackOrientation.Vertical) lub [ `Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal). Wartość domyślna to `Vertical`.

Ustaw [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing) właściwość `StackLayout` do `double` wartością do określenia odstępy między elementami podrzędnymi. Wartość domyślna to 6.

W kodzie, można dodać elementów do `Children` zbiór `StackLayout` w `for` lub `foreach` pętli, jak pokazano w [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) próbki, lub można zainicjować `Children` kolekcji z listą pojedyncze widoki jako wykazały w [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Elementy podrzędne muszą pochodzić od `View` , ale mogą zawierać inne `StackLayout` obiektów.

## <a name="scrolling-content"></a>Przewijanie zawartości

Jeśli `StackLayout` zawiera zbyt wiele elementów podrzędnych do wyświetlenia na stronie, można umieścić `StackLayout` w [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) umożliwia przewijanie.

Ustaw [ `Content` ](xref:Xamarin.Forms.ScrollView.Content) właściwość `ScrollView` do widoku, aby przewijać. Jest to często `StackLayout`, ale może to być dowolny widok.

Ustaw [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation) właściwość `ScrollView` do elementu członkowskiego [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) właściwości [ `Vertical` ](xref:Xamarin.Forms.ScrollOrientation.Vertical), [ `Horizontal` ](xref:Xamarin.Forms.ScrollOrientation.Horizontal), lub [ `Both` ](xref:Xamarin.Forms.ScrollOrientation.Both). Wartość domyślna to `Vertical`. Jeśli zawartość `ScrollView` jest `StackLayout`, dwa orientacje powinny być zgodne.

[ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) przykład demonstruje użycie `ScrollView` i `StackLayout` do wyświetlenia dostępnych kolorów. W przykładzie pokazano również jak uzyskać właściwości statyczne publiczne i pola za pomocą odbicia w .NET `Color` struktury, bez konieczności jawnego umieszczać je na liście.

## <a name="the-expands-option"></a>Opcja Expands

Gdy `StackLayout` stosach jej funkcji podrzędnych każdego elementu podrzędnego zajmuje określonego miejsca w ramach całkowita wysokość `StackLayout` to zależy od rozmiaru elementu podrzędnego i ustawienia jej `HorizontalOptions` i `VerticalOptions` właściwości. Te właściwości mają przypisane wartości typu [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/).

`LayoutOptions` Struktury definiuje dwie właściwości:

- [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) typem wyliczenia [ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment) z czterema składowymi [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start), [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center), [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End), i [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) tego typu `bool`

Dla Twojej wygody `LayoutOptions` struktury definiuje również osiem statycznego pola tylko do odczytu typu `LayoutOptions` , uwzględniający wszystkie kombinacje właściwości dwa wystąpienia:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

Obejmuje następujące dyskusji `StackLayout` w orientacji pionowej domyślne. Poziome `StackLayout` jest analogiczna.

Pionowo `StackLayout`, `HorizontalOptions` ustawienie określa, jak element podrzędny jest umieszczony w poziomie do szerokości `StackLayout`. `Alignment` Ustawienie `Start`, `Center`, lub `End` powoduje, że podrzędnych się poziomo nieograniczone. Element podrzędny określa własnej szerokości i znajduje się w lewej, center lub po prawej stronie `StackLayout`. `Fill` Opcja powoduje, że podrzędne w poziomie będą ograniczane i wypełnia szerokość `StackLayout`.

Pionowo `StackLayout`każdego elementu podrzędnego jest pionowo nieograniczonego i pobiera pionowej gniazdo w zależności od wysokości elementu podrzędnego, w którym to przypadku `VerticalOptions` ustawienie nie ma znaczenia.

Jeśli w pionie `StackLayout` jest nieograniczonego&mdash;to znaczy jeśli jego `VerticalOptions` ustawienie jest `Start`, `Center`, lub `End`, następnie wysokość `StackLayout` jest całkowita wysokość jego elementów podrzędnych.

Jednak jeśli pionowe `StackLayout` w pionie jest ograniczony&mdash;jeśli jego `VerticalOptions` jest ustawienie `Fill` &mdash;następnie wysokość `StackLayout` będzie wysokość jego kontenera, która może być większa niż suma wysokość jego elementów podrzędnych. Jeśli tak jest rzeczywiście i ma co najmniej jeden element podrzędny `VerticalOptions` ustawienie z `Expands` flagę `true`, następnie dodatkowe miejsce w `StackLayout` jest przydzielany równomiernie między wszystkie te elementy podrzędne przy użyciu `Expands` flagę `true`. Całkowita wysokość elementu podrzędnego następnie będzie równa wysokość `StackLayout`i `Alignment` wchodzi w skład `VerticalOptions` ustawienie określa, jak element podrzędny zostanie umieszczone w jego miejscu.

Jest to zaprezentowane w [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) próbki.

## <a name="frame-and-boxview"></a>Ramka i BoxView

Te dwa widoki prostokątny są często używane do celów prezentacji.

[ `Frame` ](xref:Xamarin.Forms.Frame) Widoku wyświetla prostokątny ramkę wokół innego widoku, który może być takie jak układ `StackLayout`. `Frame` dziedziczy [ `Content` ](xref:Xamarin.Forms.ContentView.Content) właściwość [ `ContentView` ](xref:Xamarin.Forms.ContentView) ustawionym widoku mają być wyświetlane w ramach `Frame`. `Frame` Jest niewidoczny, domyślnie. Ustaw następujące trzy właściwości, aby dostosować wygląd ramki:

- [ `OutlineColor` ](xref:Xamarin.Forms.Frame.OutlineColor) Właściwość, aby stał się widoczny. Często można ustawić `OutlineColor` do `Color.Accent` kiedy nie wiesz, podstawowych schemat kolorów.
- [ `HasShadow` ](xref:Xamarin.Forms.Frame.HasShadow) Właściwość może być ustawiona na `true` do wyświetlania czarnym tle na urządzeniach z systemem iOS.
- Ustaw [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) właściwości `Thickness` wartość oddzielone spacjami ramki i ramki jego zawartości. Wartość domyślna to 20 jednostek ze wszystkich stron.

`Frame` Ma wartości domyślnej `HorizontalOptions` i `VerticalOptions` wartości `LayoutOptions.Fill`, co oznacza, że `Frame` będzie wypełnienia jego kontenera. Z innymi ustawieniami rozmiar `Frame` zależy od rozmiaru jego zawartości.

`Frame` Została przedstawiona w [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) próbki.

[ `BoxView` ](xref:Xamarin.Forms.BoxView) Wyświetla prostokątny obszar kolor określony przez jego [ `Color` ](xref:Xamarin.Forms.BoxView.Color) właściwości.

Jeśli `BoxView` jest ograniczony (jego `HorizontalOptions` i `VerticalOptions` właściwości mają ustawień domyślnych z `LayoutOptions.Fill`), `BoxView` wypełnia ilość miejsca dostępnego dla niego. Jeśli `BoxView` jest nieograniczonego (przy użyciu `HorizontalOptions` i `LayoutOptions` ustawienia `Start`, `Center`, lub `End`), posiada wymiar domyślny kwadratu 40 jednostek. Element `BoxView` może być ograniczona w jednym wymiarze i nieograniczonego w innym.

Często będziesz ustawisz [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) i [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) właściwości `BoxView` celu nadania mu określonego rozmiaru. Jest to zilustrowane przez [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) próbki.

Można użyć kilka wystąpień `StackLayout` połączyć `BoxView` i kilka `Label` wystąpienia w `Frame` wyświetlane określonego koloru, a następnie składane w każdym z tych widoków w `StackLayout` w `ScrollView` do tworzenia atrakcyjnych Lista kolorów objętego [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) próbki:

[![Potrójna zrzut ekranu przedstawiający bloki kolorów](images/ch04fg11-small.png "listy kolorów")](images/ch04fg11-large.png#lightbox "listy kolorów")

## <a name="a-scrollview-in-a-stacklayout"></a>ScrollView w StackLayout?

Umieszczenie `StackLayout` w `ScrollView` jest wspólnego, ale odkładanie `ScrollView` w `StackLayout` również czasami wygodnie jest. Teoretycznie ten nie powinny być możliwe ponieważ pionowej elementy podrzędne `StackLayout` są nieograniczone pionowo. Ale `ScrollView` pionowo ograniczone. Musisz nadać określonej wysokości tak, aby następnie można określić rozmiar jego podrzędny przewijania.

Lewy jest zapewnienie `ScrollView` podrzędnym `StackLayout` `VerticalOptions` ustawienie `FillAndExpand`. Jest to zaprezentowane w [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) próbki.

**BlackCat** przykład pokazuje również, jak zdefiniować i uzyskiwać dostęp do zasobów programu, które są osadzone w bibliotece udostępnionej. Można to osiągnąć również przy użyciu udostępnionych zasobów (protokoły SAP), ale proces ten jest nieco trudniejszy jako [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) w przykładzie pokazano.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 4 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Przykłady rozdział 4](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Przykłady rozdział 4 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
