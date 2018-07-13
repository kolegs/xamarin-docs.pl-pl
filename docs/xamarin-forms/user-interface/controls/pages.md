---
title: Strony zestawu narzędzi Xamarin.Forms
description: Strony zestawu narzędzi Xamarin.Forms reprezentują ekrany aplikacji mobilnych dla wielu platform. W tym artykule wymieniono stron, które znajdują się w interfejsie Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: fea1db6c65e533692601439f4712d15371740644
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995901"
---
# <a name="xamarinforms-pages"></a>Strony zestawu narzędzi Xamarin.Forms

_Strony zestawu narzędzi Xamarin.Forms reprezentują ekrany aplikacji mobilnych dla wielu platform._

Wszystkie typy stron, które zostały opisane poniżej pochodzi od Xamarin.Forms [ `Page` ](xref:Xamarin.Forms.Page) klasy. Te elementy wizualne zajmują wszystkich lub większości ekranu. A `Page` obiekt reprezentuje `ViewController` w systemie iOS i `Page` platformie Universal Windows. W systemie Android, każda strona trwa maksymalnie ekran, tak jak `Activity`, ale strony zestawu narzędzi Xamarin.Forms *nie* `Activity` obiektów.

[ ![](pages-images/pages-sml.png "Typy stron Xamarin.Forms")](pages-images/pages.png#lightbox "typy stron zestawu narzędzi Xamarin.Forms")

## <a name="pages"></a>Strony

Zestaw narzędzi Xamarin.Forms obsługuje następujące typy strony:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage) to najprostszy i najbardziej typowe rodzaj strony. Ustaw [ `Content` ](xref:Xamarin.Forms.ContentPage.Content) właściwości do pojedynczego [ `View` ](views.md) obiektu, który jest w większości przypadków [ `Layout` ](layouts.md) takich jak [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), lub [ `ScrollView` ](layouts.md#scrollView).<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.ContentPage) | [![Przykład ContentPage](pages-images/ContentPage.png "przykład ContentPage")](pages-images/ContentPage-Large.png#lightbox "przykład ContentPage")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) zarządza dwa okienka informacji. Ustaw [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) właściwości na stronie ogólnie wyświetlanie listy lub menu. Ustaw [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) właściwości na stronie przedstawiający wybrany element ze strony wzorcowej. [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) Właściwość kontroluje, czy strona wzorca lub szczegółów jest widoczna.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.MasterDetailPage) / [przewodnik](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![Przykład MasterDetailPage](pages-images/MasterDetailPage.png "przykład MasterDetailPage")](pages-images/MasterDetailPage-Large.png#lightbox "przykład MasterDetailPage")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) z [związanym z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Zarządza Nawigacja wśród innych stron przy użyciu architektury opartej na stosie. Korzystając z nawigowania po stronach w aplikacji, wystąpienia części strony głównej powinien zostać przekazany do konstruktora obiektu `NavigationPage` obiektu.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.NavigationPage) / [przewodnik](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [przykładowe 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/), [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), i [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![Przykład NavigationPage](pages-images/NavigationPage.png "przykład NavigationPage")](pages-images/NavigationPage-Large.png#lightbox "przykład NavigationPage")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) z [kodu = za zaporą](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) pochodzi z abstrakcyjnej [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) klasy i umożliwia nawigacji między podrzędny, stron za pomocą karty. Ustaw [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) właściwości w kolekcji, stron lub zestaw [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) właściwości w kolekcji obiektów danych i [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) Właściwość [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) opisujące, jak każdy obiekt ma być przedstawiane.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.TabbedPage) / [przewodnik](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [przykładowe 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) i [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![Przykład TabbedPage](pages-images/TabbedPage.png "przykład TabbedPage")](pages-images/TabbedPage-Large.png#lightbox "przykład TabbedPage")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) pochodzi z abstrakcyjnej [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) klasy i umożliwia nawigacji między podrzędny, stron za pośrednictwem szybko przesuwając palcem. Ustaw [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) właściwości w kolekcji [ `ContentPage` ](#contentPage) obiektów lub zestawem [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) właściwości w kolekcji obiektów danych i [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) właściwości [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) opisujące, jak każdy obiekt ma być przedstawiane.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.CarouselPage) / [przewodnik](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [przykładowe 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) i [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![Przykład CarouselPage](pages-images/CarouselPage.png "przykład CarouselPage")](pages-images/CarouselPage-Large.png#lightbox "przykład CarouselPage")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) Wyświetla zawartość w pełnym ekranie przy użyciu szablonu kontrolki i jest klasą bazową dla [ `ContentPage` ](#contentPage).<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.TemplatedPage) / [przewodnik](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Przykład TemplatedPage](pages-images/TemplatedPage.png "przykład TemplatedPage")](pages-images/TemplatedPage.png "przykład TemplatedPage") |
|     |     |

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do zestawu narzędzi Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Przykładowe FormsGallery zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
