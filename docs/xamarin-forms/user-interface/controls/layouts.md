---
title: Układy platformy Xamarin.Forms
description: Układy platformy Xamarin.Forms są używane do tworzenia formantów interfejsu użytkownika do struktury visual.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: ca48ad6fb7d5aae53f972f6e3b5257919d140a8d
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/25/2018
ms.locfileid: "34546167"
---
# <a name="xamarinforms-layouts"></a>Układy platformy Xamarin.Forms

_Układy platformy Xamarin.Forms są używane do tworzenia formantów interfejsu użytkownika do struktury visual._

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) i [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) podtypów specjalnych widoków, które działają jak kontenery widoki i układy innych występują następujące klasy w platformy Xamarin.Forms. `Layout` Sama klasa pochodzi od [ `View` ](views.md). A `Layout` zależnych zwykle zawiera logikę można ustawić położenie i rozmiar elementów podrzędnych w aplikacjach platformy Xamarin.Forms.

[![Typy układ platformy Xamarin.Forms](layouts-images/layouts-sml.png "typy układ platformy Xamarin.Forms")](layouts-images/layouts.png#lightbox "typy układ platformy Xamarin.Forms")

Klasy, które pochodzą z `Layout` można podzielić na dwie kategorie:

## <a name="layouts-with-single-content"></a>Układy z pojedynczą zawartość

Te klasy pochodzić od [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), który definiuje [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) i [ `IsClippedToBounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.IsClippedToBounds/) właściwości.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) zawiera pojedynczy element potomny ustawionej z [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) właściwości. `Content` Właściwość można ustawić dla każdego `View` pochodnej, także inne `Layout` pochodnych. `ContentView` przede wszystkim jest używany jako element strukturalnych i służy jako klasę podstawową do [ `Frame` ](#frame).<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) | [![Przykład ContentView](layouts-images/ContentView.png "przykład ContentView")](layouts-images/ContentView-Large.png#lightbox "ContentView przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Klatka

|     |     |
| --- | --- |
| [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) Pochodną klasy [ `ContentView` ](#contentView) i wyświetla prostokątne ramki wokół jego podrzędny. `Frame` ma wartość domyślną [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) o wartości 20, a także określa [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/), [ `CornerRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.CornerRadius/), i [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/)właściwości.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) | [![Ramka przykład](layouts-images/Frame.png "ramki przykład")](layouts-images/Frame-Large.png#lightbox "ramki przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) jest w stanie przewijanie jego zawartość. Ustaw [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) właściwości widoku lub zbyt duży układu na ekranie. (Zawartość `ScrollView` jest bardzo często [ `StackLayout` ](#stackLayout).) Ustaw [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) Właściwość wskazująca, czy przewijanie powinna być pionowy, poziomy lub oba.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) / [przewodnik](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Przykład ScrollView](layouts-images/ScrollView.png "przykład ScrollView")](layouts-images/ScrollView-Large.png#lightbox "ScrollView przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) Wyświetla zawartość z szablon formantu i jest klasą bazową dla [ `ContentView` ](#contentView).<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) / [przewodnik](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Przykład TemplatedView](layouts-images/TemplatedView.png "przykład TemplatedView")](layouts-images/TemplatedView.png#lightbox "TemplatedView przykład") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) Menedżer układu, widoki szablonem, używane w ramach [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) do oznaczania, gdy zostanie wyświetlona zawartość, która ma być przedstawiona.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) / [przewodnik](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Przykład ContentPresenter](layouts-images/ContentPresenter.png "przykład ContentPresenter")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter przykład") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Układy z wieloma elementami podrzędnymi.

Te klasy pochodzić od [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/).

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) Określa położenie elementów podrzędnych na stosie, albo w poziomie lub pionie na podstawie [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) właściwości. [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) Właściwość kontroluje odstęp między elementami podrzędnymi oraz ma wartość domyślną 6.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) / [przewodnik](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)| [![Przykład StackLayout](layouts-images/StackLayout.png "przykład StackLayout")](layouts-images/StackLayout-Large.png#lightbox "StackLayout przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Siatka

|     |     |
| --- | --- |
| [`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) Określa położenie jego elementy podrzędne w siatce wierszy i kolumn. Położenie elementu podrzędnego jest sygnalizowany [dołączone właściwości](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/), [ `Column` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/), [ `RowSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/), i [ `ColumnSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/).<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) / [przewodnik](~/xamarin-forms/user-interface/layouts/grid.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Przykład siatka](layouts-images/Grid.png "przykład siatka")](layouts-images/Grid-Large.png#lightbox "siatki — przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) Określa położenie elementów podrzędnych w określonych lokalizacjach względem jego elementu nadrzędnego. Położenie elementu podrzędnego jest sygnalizowany [dołączone właściwości](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/) i [ `LayoutFlags` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/). `AbsoluteLayout` Przydaje się do animacji pozycji widoków.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) / [przewodnik](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Przykład AbsoluteLayout](layouts-images/AbsoluteLayout.png "przykład AbsoluteLayout")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) z [związane z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) Określa położenie elementów podrzędnych względem `RelativeLayout` samej siebie lub swoich elementów równorzędnych. Położenie elementu podrzędnego jest sygnalizowany [dołączone właściwości](~/xamarin-forms/xaml/attached-properties.md) ustawiono obiektów typu [ `Constraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/) i [ `BoundsConstraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/).<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) / [przewodnik](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Przykład RelativeLayout](layouts-images/RelativeLayout.png "przykład RelativeLayout")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) opiera się na CSS [elastyczne modułu układu pole](http://www.w3.org/TR/css-flexbox-1/), powszechnie znane jako _flex układu_ lub _pola elastycznego_. `FlexLayout` Definiuje właściwości sześciu i pięć dołączone właściwości powiązania, które zezwalają na elementy podrzędne mają być ułożone lub opakowane wiele opcji wyrównania i orientacji.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.FlexLayout) / [przewodnik](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/) | [![Przykład FlexLayout](layouts-images/FlexLayout.png "przykład FlexLayout")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Przykładowe FormsGallery platformy Xamarin.Forms](https://developer.xamarin.com/samples/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
