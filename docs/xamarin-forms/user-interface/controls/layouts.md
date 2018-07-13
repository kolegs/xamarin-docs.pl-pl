---
title: Układy platformy Xamarin.Forms
description: Układy platformy Xamarin.Forms są używane do tworzenia kontrolek interfejsu użytkownika w visual struktury. W tym artykule wymieniono układy uwzględnione w interfejsie Xamarin.Forms.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 224eb2ee3958e5979382a3dc5e70110fdce51879
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994605"
---
# <a name="xamarinforms-layouts"></a>Układy platformy Xamarin.Forms

_Układy platformy Xamarin.Forms są używane do tworzenia kontrolek interfejsu użytkownika w visual struktury._

[ `Layout` ](xref:Xamarin.Forms.Layout) i [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) klasy w interfejsie Xamarin.Forms to wyspecjalizowane podtypów widoki, które działają jak kontenery, widoki i inne układów. `Layout` Sama klasa pochodzi od klasy [ `View` ](views.md). A `Layout` utworów zależnych zwykle zawiera logikę, aby ustawić położenie i rozmiar elementów podrzędnych w aplikacjach Xamarin.Forms.

[![Typy układ zestawu narzędzi Xamarin.Forms](layouts-images/layouts-sml.png "typy układ Xamarin.Forms")](layouts-images/layouts.png#lightbox "typy układ zestawu narzędzi Xamarin.Forms")

Klasy, które wynikają z `Layout` można podzielić na dwie kategorie:

## <a name="layouts-with-single-content"></a>Układy z pojedynczą zawartość

Te klasy pochodzić od [ `Layout` ](xref:Xamarin.Forms.Layout), która definiuje [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) i [ `IsClippedToBounds` ](xref:Xamarin.Forms.Layout.IsClippedToBounds) właściwości.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView) zawiera pojedynczy element podrzędny, która została ustawiona za pomocą [ `Content` ](xref:Xamarin.Forms.ContentView.Content) właściwości. `Content` Właściwość można ustawić na dowolny `View` utworów zależnych, włączając inne `Layout` pochodnych. `ContentView` jest najczęściej używany jako element strukturalnych i służy jako klasę bazową do [ `Frame` ](#frame).<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.ContentView) | [![Przykład ContentView](layouts-images/ContentView.png "przykład ContentView")](layouts-images/ContentView-Large.png#lightbox "przykład ContentView")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Klatka

|     |     |
| --- | --- |
| [ `Frame` ](xref:Xamarin.Forms.Frame) Klasa pochodzi od [ `ContentView` ](#contentView) i wyświetla prostokątny ramkę wokół jego podrzędny. `Frame` ma wartość domyślną [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) wartość 20, a także określa [ `OutlineColor` ](xref:Xamarin.Forms.Frame.OutlineColor), [ `CornerRadius` ](xref:Xamarin.Forms.Frame.CornerRadius), i [ `HasShadow` ](xref:Xamarin.Forms.Frame.HasShadow)właściwości.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Frame) | [![Przykład ramek](layouts-images/Frame.png "ramki przykład")](layouts-images/Frame-Large.png#lightbox "ramki przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView) jest w stanie przewijanie jego zawartość. Ustaw [ `Content` ](xref:Xamarin.Forms.ScrollView.Content) właściwości widoku lub układu jest zbyt duży na ekranie. (Zawartość `ScrollView` jest bardzo często [ `StackLayout` ](#stackLayout).) Ustaw [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation) właściwości, aby wskazać, jeśli przewijania powinien być pionowa, pozioma i / lub.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.ScrollView) / [przewodnik](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Przykład ScrollView](layouts-images/ScrollView.png "przykład ScrollView")](layouts-images/ScrollView-Large.png#lightbox "przykład ScrollView")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) Wyświetla zawartość za pomocą szablonu kontrolki i jest klasą bazową dla [ `ContentView` ](#contentView).<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.TemplatedView) / [przewodnik](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Przykład TemplatedView](layouts-images/TemplatedView.png "przykład TemplatedView")](layouts-images/TemplatedView.png#lightbox "przykład TemplatedView") |
|     |     |

### <a name="contentpresenter"></a>Element ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) to Menedżer układu dla widoków oparte na szablonach, używane w ramach [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) do oznaczania, gdzie pojawia się zawartość, która jest przedstawiane.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.ContentPresenter) / [przewodnik](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Przykład ContentPresenter](layouts-images/ContentPresenter.png "przykład ContentPresenter")](layouts-images/ContentPresenter.png#lightbox "przykład ContentPresenter") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Układy z wieloma elementami podrzędnymi

Te klasy pochodzić od [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1).

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout) Określa położenie elementów podrzędnych na stosie, albo w poziomie lub pionie na podstawie [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) właściwości. [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing) Właściwość kontroluje odstępy między elementami podrzędnymi oraz ma wartość domyślną 6.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.StackLayout) / [przewodnik](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)| [![Przykład StackLayout](layouts-images/StackLayout.png "przykład StackLayout")](layouts-images/StackLayout-Large.png#lightbox "przykład StackLayout")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Siatka

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid) umieszczenie jego elementów podrzędnych siatki wierszy i kolumn. Położenie elementów podrzędnych jest sygnalizowany [dołączonych właściwości](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](xref:Xamarin.Forms.Grid.RowProperty), [ `Column` ](xref:Xamarin.Forms.Grid.ColumnProperty), [ `RowSpan` ](xref:Xamarin.Forms.Grid.RowSpanProperty), i [ `ColumnSpan` ](xref:Xamarin.Forms.Grid.ColumnSpanProperty).<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Grid) / [przewodnik](~/xamarin-forms/user-interface/layouts/grid.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Przykład Grid](layouts-images/Grid.png "przykład Grid")](layouts-images/Grid-Large.png#lightbox "siatki — przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) Określa położenie elementów podrzędnych w określonych lokalizacjach względem jego elementu nadrzędnego. Położenie elementów podrzędnych jest sygnalizowany [dołączonych właściwości](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) i [ `LayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty). `AbsoluteLayout` Jest przydatne w przypadku animowanie położenia widoków.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.AbsoluteLayout) / [przewodnik](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Przykład AbsoluteLayout](layouts-images/AbsoluteLayout.png "przykład AbsoluteLayout")](layouts-images/AbsoluteLayout-Large.png#lightbox "przykład AbsoluteLayout")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) z [związanym z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) Określa położenie elementów podrzędnych względem `RelativeLayout` sam lub jego elementów równorzędnych. Położenie elementów podrzędnych jest sygnalizowany [dołączonych właściwości](~/xamarin-forms/xaml/attached-properties.md) ustawionych dla obiektów typu [ `Constraint` ](xref:Xamarin.Forms.Constraint) i [ `BoundsConstraint` ](xref:Xamarin.Forms.Constraint).<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.RelativeLayout) / [przewodnik](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Przykład RelativeLayout](layouts-images/RelativeLayout.png "przykład RelativeLayout")](layouts-images/RelativeLayout-Large.png#lightbox "przykład RelativeLayout")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) zależy od CSS [elastyczne modułu układ pole](http://www.w3.org/TR/css-flexbox-1/), powszechnie znane jako _flex układ_ lub _flex-box_. `FlexLayout` Definiuje sześć możliwej do wiązania i pięciu dołączonych właściwości możliwej do wiązania, zezwalających na elementy podrzędne, skumulowany lub ujęte w nawiasy wiele opcji wyrównania i orientacji.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.FlexLayout) / [przewodnik](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/) | [![Przykład FlexLayout](layouts-images/FlexLayout.png "przykład FlexLayout")](layouts-images/FlexLayout-Large.png#lightbox "przykład FlexLayout")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do zestawu narzędzi Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Przykładowe FormsGallery zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
