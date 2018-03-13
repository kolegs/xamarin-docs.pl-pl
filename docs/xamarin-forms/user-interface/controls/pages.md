---
title: Strony platformy Xamarin.Forms
description: "Strony platformy Xamarin.Forms reprezentują ekranów i platform aplikacji mobilnej."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 5f979d2dbb894107d8d606ec1f41de44c294cdd3
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="xamarinforms-pages"></a>Strony platformy Xamarin.Forms

_Strony platformy Xamarin.Forms reprezentują ekranów i platform aplikacji mobilnej._

Wszystkie typy strony, które są opisane poniżej pochodzi od platformy Xamarin.Forms [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) klasy. Te elementy wizualne zajmują wszystkie lub większość ekranu. A `Page` obiekt reprezentuje `ViewController` w systemie iOS i `Page` w platformy uniwersalnej systemu Windows. W systemie Android każdej stronie zajmuje ekranu, takich jak `Activity`, ale stron platformy Xamarin.Forms *nie* `Activity` obiektów.

[ ![](pages-images/pages-sml.png "Typy stron platformy Xamarin.Forms")](pages-images/pages.png#lightbox "strony platformy Xamarin.Forms")

## <a name="pages"></a>Strony

Platformy Xamarin.Forms obsługuje następujące typy strony:

<a name="contentPage" />

### <a name="contentpage"></a>Wartość ContentPage

|     |     | 
| --- | --- | 
| [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) to najprostszy i najbardziej typowych typ strony. Ustaw [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentPage.Content/) właściwości pojedynczej [ `View` ](views.md) obiektu, który jest najczęściej [ `Layout` ](layouts.md) takich jak [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), lub [ `ScrollView` ](layouts.md#scrollView).<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) | [![Przykład wartość ContentPage](pages-images/ContentPage.png "przykładzie wartość ContentPage")](pages-images/ContentPage-Large.png#lightbox "przykładzie wartość ContentPage")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     | 
| --- | --- | 
| A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) zarządza panelami informacji. Ustaw [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) właściwości do strony zazwyczaj wyświetlanie listy lub menu. Ustaw [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) właściwości do strony przedstawiający wybrany element z strony wzorcowej. [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) Właściwość kontroluje, czy strona głównego lub szczegółów jest widoczna.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) / [przewodnik](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![Przykład MasterDetailPage](pages-images/MasterDetailPage.png "przykład MasterDetailPage")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) z [związane z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     | 
| --- | --- | 
| [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Zarządza nawigacji między innych stron przy użyciu architektury opartego na stosie. Korzystając z nawigacji strony w aplikacji, wystąpienie strony głównej powinien zostać przekazany do konstruktora obiektu `NavigationPage` obiektu.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) / [przewodnik](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [próbkowania 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/), [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), i [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![Przykład NavigationPage](pages-images/NavigationPage.png "przykład NavigationPage")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) z [kodu = za](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     | 
| --- | --- | 
| [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) pochodzi z klasy abstrakcyjnej [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) klasy i umożliwia nawigacji między podrzędnych strony za pomocą karty. Ustaw [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) właściwości to zbiór stron lub zestawu [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) właściwości w kolekcji obiektów danych i [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) dla właściwości [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) opisujące, w jaki sposób ma być przedstawiane każdego obiektu.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) / [przewodnik](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [próbkowania 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) i [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![Przykład TabbedPage](pages-images/TabbedPage.png "przykład TabbedPage")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     | 
| --- | --- | 
| [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) pochodzi z klasy abstrakcyjnej [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) klasy i umożliwia nawigacji między podrzędnych stron za pośrednictwem szybko przesuwając palca. Ustaw [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) właściwości w kolekcji [ `ContentPage` ](#contentPage) obiektów lub zestawu [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) właściwości w kolekcji obiektów danych i [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) właściwości [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) opisujące, w jaki sposób ma być przedstawiane każdego obiektu.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) / [przewodnik](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [próbkowania 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) i [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![Przykład CarouselPage](pages-images/CarouselPage.png "przykład CarouselPage")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     | 
| --- | --- | 
| [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) Wyświetla zawartość pełnego ekranu przy użyciu szablonu formantu i jest klasą bazową dla [ `ContentPage` ](#contentPage).<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) / [przewodnik](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Przykład TemplatedPage](pages-images/TemplatedPage.png "przykład TemplatedPage")](pages-images/TemplatedPage.png "TemplatedPage przykład") |
|     |     |

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Przykładowe FormsGallery platformy Xamarin.Forms](https://developer.xamarin.com/samples/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
