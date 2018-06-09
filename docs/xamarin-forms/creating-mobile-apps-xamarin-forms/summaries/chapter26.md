---
title: Podsumowanie rozdział 26. Układy niestandardowe
description: 'Tworzenie aplikacji mobilnych za pomocą platformy Xamarin.Forms: Podsumowanie rozdział 26. Układy niestandardowe'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 1c8fec34c0bc7f38d360f76122d851ae653ce15e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241174"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Podsumowanie rozdział 26. Układy niestandardowe

Platformy Xamarin.Forms obejmuje kilka klas pochodnych [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout`, a
* `RelativeLayout`.

W tym rozdziale opisano sposób tworzenia własnych klas, które pochodzą z `Layout<View>`.

## <a name="an-overview-of-layout"></a>Omówienie układu

Nie istnieje scentralizowane system obsługujący układ platformy Xamarin.Forms. Każdy element jest odpowiedzialny za określenie, co powinna być własną rozmiar i sposób renderowania się w określonym obszarze.

### <a name="parents-and-children"></a>Elementów nadrzędnych i podrzędnych

Każdy element, który ma elementy podrzędne jest odpowiedzialny za pozycjonowanie tych elementów podrzędnych w niej samej. Jest nadrzędny, który określa, jaki rozmiar jego elementy podrzędne, które powinny być oparte na rozmiar jest dostępny i chce się rozmiar elementu podrzędnego.

### <a name="sizing-and-positioning"></a>Ustawianie rozmiaru i pozycjonowania

Układu rozpoczyna się w górnej części drzewa wizualnego ze stroną, a następnie obejmującego wszystkie gałęzie. Najważniejsze publicznej metody w układzie jest [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) zdefiniowane przez `VisualElement`. Każdy element, który jest elementem nadrzędnym do innych elementów wywołań `Layout` dla każdego z jego elementów podrzędnych umożliwiają dziecka, rozmiar i położenie względem samego w formie [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) wartości. Te `Layout` wywołania propagację za pośrednictwem drzewa wizualnego.

Wywołanie `Layout` jest wymagany dla elementu są wyświetlane na ekranie i powoduje, że można ustawić następujące właściwości tylko do odczytu. Są one zgodne z `Rectangle` przekazywany do metody:

- [`Bounds`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) typu `Rectangle`
- [`X`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.X/) typu `double`
- [`Y`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Y/) typu `double`
- [`Width`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) typu `double`
- [`Height`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) typu `double`

Przed `Layout` wywołać, `Height` i `Width` mają wartości zasymulować &ndash;1.

Wywołanie `Layout` wyzwala również wywołań następujące metody chronione:

- [`SizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.SizeAllocated/p/System.Double/System.Double/), które wywołuje
- [`OnSizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeAllocated/p/System.Double/System.Double/), która może zostać zastąpiona.

Ponadto następujące zdarzenie jest wywoływane:

- [`SizeChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)

`OnSizeAllocated` Metoda zostanie przesłonięta przez `Page` i `Layout`, które są tylko dwa klas platformy Xamarin.Forms, który może mieć elementów podrzędnych. Wywołania przesłoniętej metody

- [`UpdateChildrenLayout`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.UpdateChildrenLayout()/) Aby uzyskać `Page` pochodne i [ `UpdateChildrenLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.UpdateChildrenLayout()/) dla `Layout` pochodne, które wywołuje
- [`LayoutChildren`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) Aby uzyskać `Page` pochodne i [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) dla `Layout` pochodnych.

`LayoutChildren` następnie wywołuje `Layout` dla każdego z jego elementów podrzędnych. Jeśli co najmniej jeden element podrzędny ma nową `Bounds` ustawienia, a następnie uruchamiane następujące zdarzenie:

- [`LayoutChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.LayoutChanged/) Aby uzyskać `Page` pochodne i [ `LayoutChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Layout.LayoutChanged/) dla `Layout` pochodne

### <a name="constraints-and-size-requests"></a>Rozmiar żądania i ograniczenia

Dla `LayoutChildren` inteligentnie wywołać `Layout` na wszystkie elementy podrzędne muszą znać *preferowanych* lub *żądany* rozmiar dla elementów podrzędnych. W związku z tym wywołań `Layout` dla każdego elementu podrzędnego są zazwyczaj poprzedzone wywołań

- [`GetSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)

Po publikacji księgi `GetSizeRequest` metody została przestarzałe i zastępowane

- [`Measure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)

`Measure` Metody bierze pod uwagę [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) właściwości i zawiera argument typu [ `MeasureFlag` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MeasureFlags/), która zawiera dwa elementy członkowskie:

- [`IncludeMargins`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.IncludeMargins/)
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.None/) Aby nie dołączać marginesów

W przypadku wielu elementów `GetSizeRequest` lub `Measure` uzyskuje macierzysty rozmiar elementu z jego renderowania. Obie metody mają parametry dla szerokości i wysokości *ograniczenia*. Na przykład `Label` użyje ograniczenie szerokości, aby określić sposób zawijania wiele wierszy tekstu.

Zarówno `GetSizeRequest`i `Measure` zwracać wartość typu [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/), który ma dwie właściwości:

- [`Request`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Request/) typu `Size`
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Minimum/) typu `Size`

Bardzo często te dwie wartości są takie same i `Minimum` wartości zwykle można zignorować.

`VisualElement` definiuje również metoda chroniona, podobnie jak `GetSizeRequest` który jest wywoływany z `GetSizeRequest`:

- [`OnSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeRequest/p/System.Double/System.Double/) Zwraca `SizeRequest` wartość

Tej metody jest teraz przestarzałe i zastępowane:

- [`OnMeasure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)

Każda klasa, która jest pochodną `Layout` lub `Layout<T>` przesłonięcie `OnSizeRequest` lub `OnMeasure`. Jest to, gdy klasa układu określa własnej rozmiarze, który jest zazwyczaj na podstawie rozmiaru jego elementów podrzędnych, których uzyskuje się poprzez wywołanie `GetSizeRequest` lub `Measure` na elementy podrzędne. Przed i po wywołaniu `OnSizeRequest` lub `OnMeasure`, `GetSizeRequest` lub `Measure` sprawia, że dopasowania na podstawie następujących właściwości:

- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)typu `double`, ma wpływ na `Request` właściwości `SizeRequest`
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) typu `double`, ma wpływ na `Request` właściwości `SizeRequest`
- [`MinimumWidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumWidthRequest/) typu `double`, ma wpływ na `Minimum` właściwości `SizeRequest`
- [`MinimumHeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumHeightRequest/) typu `double`, ma wpływ na `Minimum` właściwości `SizeRequest`

### <a name="infinite-constraints"></a>Nieskończone ograniczenia

Ograniczenie argumentów przekazanych do `GetSizeRequest` (lub `Measure`) i `OnSizeRequest` (lub `OnMeasure`) mogą być nieskończone (tj. wartości `Double.PositiveInfinity`). Jednak `SizeRequest` zwrócony z tych metod nie może zawierać nieskończone wymiarów.

Ograniczenia nieskończone wskazania, że żądany rozmiar powinien odzwierciedlają wielkość fizyczną elementu. Pionowym `StackLayout` wywołania `GetSizeRequest` (lub `Measure`) na elementy podrzędne z ograniczeniem nieskończone wysokość. Układ poziomy stosu wywołań `GetSizeRequest` (lub `Measure`) na jego elementy podrzędne z ograniczenie szerokości nieskończoną. `AbsoluteLayout` Wywołania `GetSizeRequest` (lub `Measure`) na jego elementy podrzędne z nieskończone ograniczenia szerokości i wysokości.

### <a name="peeking-inside-the-process"></a>Wgląd w procesie

[ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) Wyświetla ograniczenia i rozmiar żądania informacji o prostym układzie.

## <a name="deriving-from-layoutview"></a>Wyprowadzanie z układu<View>

Pochodną klasy niestandardowego układu `Layout<View>`. Składa się z dwóch obowiązki:

- Zastąpienie `OnMeasure` do wywołania `Measure` na wszystkich układ elementów podrzędnych. Zwraca żądany rozmiar dla samej siebie układu
- Zastąpienie `LayoutChildren` do wywołania `Layout` na wszystkich układ elementów podrzędnych

`for` Lub `foreach` pętli w te zastąpienia należy pominąć dziecka, którego `IsVisible` właściwość jest ustawiona na `false`.

Wywołanie `OnMeasure` nie jest gwarantowana. `OnMeasure` nie będzie można wywołać, jeśli element nadrzędny układ jest dotyczące rozmiaru układu (na przykład, układu, które wypełnia strony). Z tego powodu `LayoutChildren` nie zależą od rozmiarów podrzędny uzyskany podczas `OnMeasure` wywołania. Bardzo często `LayoutChildren` sam Wywołaj `Measure` na układ elementów podrzędnych, lub można zaimplementować określonego rodzaju rozmiar buforowanie logikę (omówiona później).

### <a name="an-easy-example"></a>Prosty przykład

[ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) próbka zawiera uproszczona [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) klasy i pokaz jej użycia.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Poziome i pionowe pozycjonowanie uproszczony

Jedno z zadań który `VerticalStack` należy wykonać podczas `LayoutChildren` zastąpienia. Metoda korzysta z tego elementu podrzędnego `HorizontalOptions` właściwości w celu określenia położenie elementu podrzędnego w jego miejsce w `VerticalStack`. Zamiast tego można wywołać metody statycznej [ `Layout.LayoutChildIntoBoundingRect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/). Ta metoda wywołuje `Measure` podrzędnych i używa jej `HorizontalOptions` i `VerticalOptions` właściwości położenie elementu podrzędnego w obrębie określonego prostokąta.

### <a name="invalidation"></a>Unieważnienie

Często zmiany w elemencie właściwości ma wpływ na sposób wyświetlania tego elementu w układzie. Układ musi być nieważne do wyzwolenia nowy układ.

`VisualElement` definiuje metodę chronionych [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/), zwykle nazywany przez program obsługi zmiany właściwości dowolnej można powiązać właściwości której zmiana wpływa na wielkość elementu. `InvalidateMeasure` Uruchamiany metody [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) zdarzeń.

`Layout` Klasa definiuje o nazwie podobnej metody chronionych [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/), który `Layout` pochodnych powinny wywoływać dla każdej zmiany, która wpływa na sposób określa położenie i rozmiar jego elementów podrzędnych.

### <a name="some-rules-for-coding-layouts"></a>Niektóre reguły kodowania układów

1. Właściwości zdefiniowane przez `Layout<T>` pochodne powinien był zabezpieczony za pomocą właściwości i zmienić właściwości obsługi powinny wywoływać `InvalidateLayout`.

2. A `Layout<T>` zależnych definiujący dołączonych właściwości powinny zastępować [ `OnAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnAdded/p/T/) można dodać obsługi zmienić właściwości do jego elementów podrzędnych i [ `OnRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnRemoved/p/T/) które zostaną usunięte Program obsługi. Program obsługi powinien Sprawdź, czy zmiany w tych dołączonych właściwości i odpowiadać przez wywołanie metody `InvalidateLayout`.

3. A `Layout<T>` pochodne implementujące pamięci podręcznej rozmiarów podrzędnych powinny zastępować `InvalidateLayout` i [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) i wyczyść pamięć podręczną wywołanego tych metod.

### <a name="a-layout-with-properties"></a>Układ z właściwości

[ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) zakłada, że wszystkie elementy podrzędne są takie same rozmiaru i opakowuje dzieci w jednym wierszu (lub kolumny) do Dalej. Definiuje `Orientation` właściwość jak `StackLayout`, i `ColumnSpacing` i `RowSpacing` właściwości, takie jak `Grid`, i buforuje ją rozmiary podrzędnych.

[ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) przykładowe naraża `WrapLayout` w `ScrollView` do wyświetlania zdjęcia z archiwum.

### <a name="no-unconstrained-dimensions-allowed"></a>Nie dozwolone nieograniczonego wymiarów!

[ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka jest przeznaczona do wyświetlania wszystkich jego obiektów podrzędnych w siebie. W związku z tym nie można uwzględniać nieograniczonego wymiarów i zgłasza wyjątek, jeśli okaże się jeden.

[ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) przykładzie pokazano `UniformGridLayout`:

[![Potrójna zrzut ekranu przedstawiający Siatka zdjęć](images/ch26fg08-small.png "Uniform Układ tabelaryczny")](images/ch26fg08-large.png#lightbox "Uniform układ siatki")

### <a name="overlapping-children"></a>Nakładające się elementy podrzędne

A `Layout<T>` pochodne mogą nakładać się na jego elementów podrzędnych. Jednak elementy podrzędne są wyświetlane z ich kolejność, w `Children` kolekcji, a nie w kolejności ich `Layout` metody są wywoływane.

`Layout` Klasy definiuje dwie metody, które pozwalają na przenoszenie elementu podrzędnego w kolekcji:

- [`LowerChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LowerChild/p/Xamarin.Forms.View/) Aby przenieść element podrzędny na początku kolekcji
- [`RaiseChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.RaiseChild/p/Xamarin.Forms.View/) Aby przenieść element podrzędny do końca kolekcji

Nakładające się podrzędnych elementów podrzędnych na końcu kolekcji są wyświetlane wizualnie nad elementów podrzędnych na początku kolekcji.

[ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Biblioteka definiuje właściwości dołączonej do wskazywania kolejności renderowania i w związku z tym zezwolić jednemu z jego elementy podrzędne wyświetlany u góry innych. [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) przykładzie pokazano to:

[![Potrójna zrzut ekranu przedstawiający siatki pliku karty uczniowie](images/ch26fg10-small.png "nakładających się dzieci układu")](images/ch26fg10-large.png#lightbox "nakładających się dzieci układu")

### <a name="more-attached-bindable-properties"></a>Więcej dołączone właściwości

[ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Biblioteka definiuje dołączonych właściwości do określania dwóch `Point` wartości i grubość i manipuluje `BoxView` elementy, aby przypominały wierszy.

[ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) próbki użyty do rysowania sześcianu 3W.

### <a name="layout-and-layoutto"></a>Układ i LayoutTo

A `Layout<T>` pochodnej można wywołać `LayoutTo` zamiast `Layout` do animowania układu. [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) Klasy dzieje się tak i [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) przykładzie pokazano go.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 26 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Przykłady rozdział 26](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Tworzenie niestandardowych układów](~/xamarin-forms/user-interface/layouts/custom.md)
