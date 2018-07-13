---
title: Podsumowanie rozdziałów 26. Układy niestandardowe
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziałów 26. Układy niestandardowe'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 1fc42207d26f2e2154c7bd6634cc90fead4b0b17
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998937"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Podsumowanie rozdziałów 26. Układy niestandardowe

Zestaw narzędzi Xamarin.Forms obejmuje kilka klas pochodnych [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout`, a
* `RelativeLayout`.

W tym rozdziale opisano sposób tworzenia własnych klas, które wynikają z `Layout<View>`.

## <a name="an-overview-of-layout"></a>Omówienie układu

Nie ma żadnych scentralizowanym systemie, który obsługuje układ zestawu narzędzi Xamarin.Forms. Każdy element jest odpowiedzialny za sprawdzenie co swój własny rozmiar powinien być i sposób renderowania sam w określonym obszarze.

### <a name="parents-and-children"></a>Elementów nadrzędnych i podrzędnych

Każdy element, który ma elementy podrzędne jest odpowiedzialny za pozycjonowanie te elementy podrzędne w sobie. Jest nadrzędnym, który określa, jaki rozmiar jego elementy podrzędne powinien być oparty na podstawie rozmiaru ma dostępne i rozmiar elementu podrzędnego nie chce być.

### <a name="sizing-and-positioning"></a>Dostosuj rozmiar i położenie

Układ rozpoczyna się w górnej części drzewa wizualnego ze stroną, a następnie przechodzi przez wszystkie gałęzie. Najważniejszą metodą publicznych w układzie jest [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) definicją `VisualElement`. Każdy element, który jest elementem nadrzędnym, aby inne wywołania elementów `Layout` dla każdego z jego elementów podrzędnych, aby dać elementu podrzędnego, rozmiar i położenie względem samej w formie [ `Rectangle` ](xref:Xamarin.Forms.Rectangle) wartości. Te `Layout` wywołania propagować przez drzewo wizualne.

Wywołanie `Layout` jest wymagany dla elementu na ekranie i powoduje, że można ustawić następujące właściwości tylko do odczytu. Są one zgodne z `Rectangle` przekazywany do metody:

- [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds) tego typu `Rectangle`
- [`X`](xref:Xamarin.Forms.VisualElement.X) tego typu `double`
- [`Y`](xref:Xamarin.Forms.VisualElement.Y) tego typu `double`
- [`Width`](xref:Xamarin.Forms.VisualElement.Width) tego typu `double`
- [`Height`](xref:Xamarin.Forms.VisualElement.Height) tego typu `double`

Przed `Layout` wywołać, `Height` i `Width` mają wartości makiety &ndash;1.

Wywołanie `Layout` wyzwala również wywołania chronionego następujących metod:

- [`SizeAllocated`](xref:Xamarin.Forms.VisualElement.SizeAllocated(System.Double,System.Double)), która wywołuje metodę
- [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated(System.Double,System.Double)), który może zostać zastąpiona.

Na koniec jest wyzwalane zdarzenie następujące:

- [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

`OnSizeAllocated` Metoda zostanie przesłonięta przez `Page` i `Layout`, które są tylko dwie klasy w interfejsie Xamarin.Forms, który może mieć elementów podrzędnych. Wywołuje zastąpione — metoda

- [`UpdateChildrenLayout`](xref:Xamarin.Forms.Page.UpdateChildrenLayout) Aby uzyskać `Page` pochodnych i [ `UpdateChildrenLayout` ](xref:Xamarin.Forms.Layout.UpdateChildrenLayout) dla `Layout` pochodne, które wywołuje
- [`LayoutChildren`](xref:Xamarin.Forms.Page.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) Aby uzyskać `Page` pochodnych i [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) dla `Layout` pochodnych.

`LayoutChildren` następnie wywołuje `Layout` dla każdego z elementów podrzędnych elementu. Jeśli co najmniej jeden element podrzędny ma nową `Bounds` ustawienia, a następnie uruchamiane następujące zdarzenie:

- [`LayoutChanged`](xref:Xamarin.Forms.Page.LayoutChanged) Aby uzyskać `Page` pochodnych i [ `LayoutChanged` ](xref:Xamarin.Forms.Layout.LayoutChanged) dla `Layout` pochodne

### <a name="constraints-and-size-requests"></a>Ograniczenia i rozmiar żądania

Dla `LayoutChildren` inteligentnie wywołać `Layout` na wszystkich jego obiektów podrzędnych, trzeba znać *preferowanych* lub *żądaną* rozmiar dla dzieci. W związku z tym wywołania `Layout` dla każdego elementu podrzędnego są zazwyczaj poprzedzone wywołania

- [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))

Po publikacji księgi `GetSizeRequest` metoda została przestarzały i zastąpiony

- [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))

`Measure` Obsługuje metody [ `Margin` ](xref:Xamarin.Forms.View.Margin) właściwości i zawiera argument typu [ `MeasureFlag` ](xref:Xamarin.Forms.MeasureFlags), która zawiera dwa elementy członkowskie:

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None) Aby nie dołączać marginesów

W przypadku wielu elementów `GetSizeRequest` lub `Measure` uzyskuje dostęp do natywnych rozmiar elementu z jego renderowania. Obie metody mają parametry dla szerokości i wysokości *ograniczenia*. Na przykład `Label` użyje ograniczenie szerokości, aby określić, jak opakowywać wiele wierszy tekstu.

Zarówno `GetSizeRequest`i `Measure` zwraca wartość typu [ `SizeRequest` ](xref:Xamarin.Forms.SizeRequest), który ma dwie właściwości:

- [`Request`](xref:Xamarin.Forms.SizeRequest.Request) tego typu `Size`
- [`Minimum`](xref:Xamarin.Forms.SizeRequest.Minimum) tego typu `Size`

Bardzo często te dwie wartości są takie same, a `Minimum` wartości zwykle można je zignorować.

`VisualElement` definiuje również chronione metody podobnej do `GetSizeRequest` który jest wywoływany z `GetSizeRequest`:

- [`OnSizeRequest`](xref:Xamarin.Forms.VisualElement.OnSizeRequest(System.Double,System.Double)) Zwraca `SizeRequest` wartość

Ta metoda jest teraz przestarzały i zastąpiony:

- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))

Każda klasa, która pochodzi od klasy `Layout` lub `Layout<T>` przesłonięcie `OnSizeRequest` lub `OnMeasure`. Jest to, gdzie klasa układ określa swój własny rozmiar, który jest zazwyczaj na podstawie rozmiaru jego elementy podrzędne, które uzyskuje się przez wywołanie metody `GetSizeRequest` lub `Measure` na elementy podrzędne. Przed i po wywołaniu `OnSizeRequest` lub `OnMeasure`, `GetSizeRequest` lub `Measure` sprawia, że dopasowania na podstawie następujących właściwości:

- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)typu `double`, ma wpływ na `Request` właściwość `SizeRequest`
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) typu `double`, ma wpływ na `Request` właściwość `SizeRequest`
- [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest) typu `double`, ma wpływ na `Minimum` właściwość `SizeRequest`
- [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest) typu `double`, ma wpływ na `Minimum` właściwość `SizeRequest`

### <a name="infinite-constraints"></a>Nieskończona ograniczenia

Ograniczenie Argumenty przekazane do `GetSizeRequest` (lub `Measure`) i `OnSizeRequest` (lub `OnMeasure`) mogą być nieskończone (czyli wartości `Double.PositiveInfinity`). Jednak `SizeRequest` zwrócone przez te metody nie może zawierać nieskończonej wymiarów.

Ograniczenia nieskończonej wskazują, czy żądany rozmiar powinny odzwierciedlać wielkość fizyczną elementu. Pionowa `StackLayout` wywołania `GetSizeRequest` (lub `Measure`) na jego elementy podrzędne z ograniczeniem wysokość nieskończone. Układ poziomy stosu wywołań `GetSizeRequest` (lub `Measure`) na jego elementy podrzędne o ograniczenie szerokości nieskończone. `AbsoluteLayout` Wywołania `GetSizeRequest` (lub `Measure`) na jego elementy podrzędne z nieskończoną ograniczeniami szerokość i wysokość.

### <a name="peeking-inside-the-process"></a>Wgląd do wewnątrz procesu

[ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) Wyświetla ograniczenia i rozmiar żądania informacje dotyczące układu prostego.

## <a name="deriving-from-layoutview"></a>Wyprowadzanie z układu<View>

Klasa układu niestandardowego pochodzi z `Layout<View>`. Ma dwa obowiązki:

- Zastąp `OnMeasure` do wywołania `Measure` na wszystkich układu elementów podrzędnych. Zwraca żądany rozmiar samego układu
- Zastąp `LayoutChildren` do wywołania `Layout` na wszystkich układu elementów podrzędnych

`for` Lub `foreach` pętlę w te zastąpienia należy pominąć wszystkie podrzędne którego `IsVisible` właściwość jest ustawiona na `false`.

Wywołanie `OnMeasure` nie ma żadnej gwarancji. `OnMeasure` nie zostanie wywołany, jeśli element nadrzędny układ jest dotyczących rozmiaru układu (na przykład, układ, który wypełnia strony). Z tego powodu `LayoutChildren` nie można polegać na rozmiary podrzędnych uzyskanego podczas `OnMeasure` wywołania. Bardzo często `LayoutChildren` samego wywołania `Measure` na elementy podrzędne w układzie, lub możesz zaimplementować pewnego rodzaju rozmiar pamięci podręcznej logikę (omówione później).

### <a name="an-easy-example"></a>Przykładem proste

[ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) sample zawiera uproszczona [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) klasy i pokaz jego użycia.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Poziome i pionowe pozycjonowanie uproszczony

Jedno z zadań, `VerticalStack` należy wykonać podczas `LayoutChildren` zastąpienia. Metoda używa elementu podrzędnego `HorizontalOptions` właściwości, aby ustalić położenie elementu podrzędnego w jego miejsce w `VerticalStack`. Zamiast tego można wywołać statyczną metodę [ `Layout.LayoutChildIntoBoundingRect` ](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle)). Ta metoda wywołuje `Measure` na element podrzędny i używa jej `HorizontalOptions` i `VerticalOptions` właściwości do elementu podrzędnego w obrębie prostokąta określonej pozycji.

### <a name="invalidation"></a>Unieważnieniu

Często zmiany właściwości elementu ma wpływ na sposób wyświetlania tego elementu w układzie. Układ musi zostać unieważnione wyzwolenie nowego układu.

`VisualElement` definiuje metodę chronionych [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure), zazwyczaj nazywana przez program obsługi dowolnej właściwości, które można powiązać zmienić właściwości którego zmiana wpływa na wielkość elementu. `InvalidateMeasure` Generowane metody [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) zdarzeń.

`Layout` Klasa definiuje o nazwie podobnej metody chronionej [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout), który `Layout` utworów zależnych, powinny wywoływać o wszystkich zmianach, który wpływa na sposób umieszcza i rozmiarów jego obiektów podrzędnych.

### <a name="some-rules-for-coding-layouts"></a>Niektóre reguły kodowania układów

1. Właściwości zdefiniowane przez `Layout<T>` pochodnych należy utworzyć kopię, przez które można powiązać właściwości i procedury obsługi zmiany właściwości powinny wywoływać `InvalidateLayout`.

2. A `Layout<T>` utworów zależnych, definiujący dołączonych właściwości możliwej do wiązania powinien przesłonić [ `OnAdded` ](xref:Xamarin.Forms.Layout`1.OnAdded*) można dodać procedury obsługi zmiany właściwości do jego elementów podrzędnych i [ `OnRemoved` ](xref:Xamarin.Forms.Layout`1.OnRemoved*) które zostaną usunięte Program obsługi. Procedura obsługi powinna Sprawdź, czy zmiany w tych dołączonych właściwości możliwej do wiązania i odpowiadać przez wywołanie metody `InvalidateLayout`.

3. A `Layout<T>` utworów zależnych, implementujący pamięci podręcznej rozmiarów podrzędne powinien przesłonić `InvalidateLayout` i [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) i wyczyścić pamięć podręczną, gdy te metody są wywoływane.

### <a name="a-layout-with-properties"></a>Układ z właściwościami

[ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) założono, że wszystkie jego elementy podrzędne jest taki sam rozmiar i elementy podrzędne w jednym wierszu (lub kolumny) jest zawijany do Dalej. Definiuje on `Orientation` właściwości, takich jak `StackLayout`, i `ColumnSpacing` i `RowSpacing` właściwości, takie jak `Grid`, i buforuje ją rozmiary podrzędnych.

[ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) przykładowy umieszcza `WrapLayout` w `ScrollView` do wyświetlania zdjęcia z archiwum.

### <a name="no-unconstrained-dimensions-allowed"></a>Nie mogą nieograniczonego wymiarów!

[ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki jest przeznaczony do wyświetlania wszystkich elementów podrzędnych w sobie. Dlatego nie może obsłużyć z nieograniczonego wymiarami i zgłasza wyjątek, jeśli okaże się jeden.

[ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) w przykładzie pokazano `UniformGridLayout`:

[![Potrójna zrzut ekranu przedstawiający Siatka zdjęć](images/ch26fg08-small.png "jednolitego Układ tabelaryczny")](images/ch26fg08-large.png#lightbox "jednolitego układu siatki")

### <a name="overlapping-children"></a>Nakładające się elementów podrzędnych

A `Layout<T>` utworów zależnych mogą nakładać się na jego elementy podrzędne. Jednak elementy podrzędne są renderowane w kolejności ich w `Children` kolekcji, a nie kolejność, w której ich `Layout` metody są wywoływane.

`Layout` Klasy definiuje dwie metody, które pozwalają na przenoszenie elementu podrzędnego w obrębie kolekcji:

- [`LowerChild`](xref:Xamarin.Forms.Layout.LowerChild(Xamarin.Forms.View)) Aby przenieść element podrzędny do początku kolekcji
- [`RaiseChild`](xref:Xamarin.Forms.Layout.RaiseChild(Xamarin.Forms.View)) Aby przenieść element podrzędny do końca kolekcji

W przypadku nakładających się dzieci elementy podrzędne końca kolekcji wizualnie wyświetlane na podstawie elementów podrzędnych na początku kolekcji.

[ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Biblioteka definiuje dołączoną właściwość do wskazania kolejności renderowania i tym samym zezwolić jednemu z jego elementy podrzędne mają być wyświetlane na innych. [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) przykład pokazuje, to:

[![Potrójna zrzut ekranu przedstawiający uczniów karty plik siatki](images/ch26fg10-small.png "nakładających się dzieci układ")](images/ch26fg10-large.png#lightbox "nakładających się dzieci układu")

### <a name="more-attached-bindable-properties"></a>Więcej dołączonych właściwości możliwej do wiązania

[ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Biblioteka definiuje właściwości możliwej do wiązania dołączone do określenia dwóch `Point` wartości i grubość i obsługuje `BoxView` elementów przypominają wiersze.

[ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) próbki, aby narysować sześcianu 3D.

### <a name="layout-and-layoutto"></a>Układ i LayoutTo

A `Layout<T>` utworów zależnych, można wywołać `LayoutTo` zamiast `Layout` animować układu. [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) Klasy dzieje i [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) w przykładzie pokazano go.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 26 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Przykłady rozdział 26](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Tworzenie niestandardowych układów](~/xamarin-forms/user-interface/layouts/custom.md)
