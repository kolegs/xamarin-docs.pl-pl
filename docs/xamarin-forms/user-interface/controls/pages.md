---
title: Strony platformy Xamarin.Forms
description: "Strony platformy Xamarin.Forms reprezentują ekranów i platform aplikacji mobilnej."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 35822dbbb7d5694e7f1f0a3f35f10df404206af9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-pages"></a>Strony platformy Xamarin.Forms

_Strony platformy Xamarin.Forms reprezentują ekranów i platform aplikacji mobilnej._

<style>.tableimg {max-width: Brak! ważne;}</style>

## <a name="pages"></a>Strony

[ `Page` ](http://iosapi.xamarin.com/?link=T%3aXamarin.Forms.Page) Klasy jest element wizualny, który zajmuje większość lub wszystkie ekranu i zawiera pojedynczy element potomny. A `Xamarin.Forms.Page` reprezentuje kontrolera widoku w systemie iOS lub Windows Phone strony. W systemie Android każdej stronie zajmuje ekranu, takie jak działania, ale są strony platformy Xamarin.Forms *nie* działań.

 [ ![](pages-images/pages-sml.png "Typy stron platformy Xamarin.Forms")](pages-images/pages.png "strony platformy Xamarin.Forms")

<br clear="all" />

Obsługuje platformy Xamarin.Forms:

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <tr>
  <thead>
    <th>
      <strong>Typ</strong>
    </th>
    <th>
      <strong>Opis elementu</strong>
    </th>
    <th style="min-width:400px">
      <strong>Zrzut ekranu</strong>
    </th>
  </thead></tr>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a>
    </td>
    <td align="center" valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">wartość ContentPage</a> wyświetla pojedynczy <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a>, często kontener, takich jak <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a> lub <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentPageDemoPage.cs"><img src="pages-images/ContentPage.png" title="Przykład wartość ContentPage" class="tableimg">
    </a></td>
  </tr><tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">strony</a> który zarządza panelami informacji.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/MasterDetailPageDemoPage.cs"><img src="pages-images/MasterDetailPage.png" title="Przykład MasterDetailPage" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">strony</a> który zarządza nawigacji i środowisko użytkownika stosu innych stron.  
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/NavigationPageDemoPage.cs"><img src="pages-images/NavigationPage.png" title="Przykład NavigationPage" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">strony</a> umożliwiający nawigację między elementami podrzędnymi stron przy użyciu karty.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TabbedPageDemoPage.cs"><img src="pages-images/TabbedPage.png" title="Przykład TabbedPage" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">strony</a> który wyświetla zawartość pełnego ekranu z szablon formantu i klasa podstawowa dla <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">wartość ContentPage</a>.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="pages-images/TemplatedPage.png" title="Przykład TemplatedPage" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">strony</a> stosowanie gestów Przejdź między podstrony, takich jak galerii.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CarouselPageDemoPage.cs"><img src="pages-images/CarouselPage.png" title="Przykład CarouselPage" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Galeria platformy Xamarin.Forms (przykład)](https://developer.xamarin.com/samples/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](http://iosapi.xamarin.com/?link=N%3aXamarin.Forms)
