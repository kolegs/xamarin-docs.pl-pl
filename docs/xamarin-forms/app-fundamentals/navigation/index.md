---
title: Nawigacji platformy Xamarin.Forms
description: W tym przewodniku opisano sposób przeprowadzenia nawigacji w aplikacji platformy Xamarin.Forms. Platformy Xamarin.Forms udostępnia wiele zastosowań nawigacji innej strony, w zależności od używanego typu strony.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 90aedee42af7ed1788110e832fb3b435d870ee77
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241959"
---
# <a name="xamarinforms-navigation"></a>Nawigacji platformy Xamarin.Forms

_Platformy Xamarin.Forms udostępnia wiele zastosowań nawigacji innej strony, w zależności od używanego typu strony._

![](images/page-types.png "Typy stron platformy Xamarin.Forms")

## <a name="hierarchical-navigationhierarchicalmd"></a>[Nawigacja hierarchiczna](hierarchical.md)

[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Klasa udostępnia środowisko hierarchiczna nawigacji, gdy użytkownik jest powinni poruszać się po stronach przodu i do tyłu, zgodnie z potrzebami. Klasa implementuje nawigacji jako ostatni na, wytworzenia stos [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) obiektów.

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Platformy Xamarin.Forms [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) składa się z listy kart i większy obszar szczegółów, z każdej karcie ładowania zawartości w obszarze szczegółów.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Platformy Xamarin.Forms [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) jest strony, który użytkownicy mogą szybko przesuń strony do przechodzenia przez strony zawartości, takich jak galerii.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Platformy Xamarin.Forms [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) jest strona, która zarządza dwie strony powiązane informacje — prezentuje elementy strony wzorcowej oraz strony szczegółów, który przedstawia szczegółowe informacje dotyczące elementów na stronie głównej.

## <a name="modal-pagesmodalmd"></a>[Strony modalne](modal.md)

Platformy Xamarin.Forms także zapewnia obsługę modalne stron. Modalne strony zachęca użytkowników do ukończenia zadania niezależne, który nie może być opuszczeniu do czasu ukończenia zadania lub anulowane.

## <a name="displaying-pop-upspop-upsmd"></a>[Wyświetlanie wyskakujących okienek](pop-ups.md)

Platformy Xamarin.Forms zawiera dwa elementy interfejsu użytkownika podręcznego konta podobne: alert i arkusza akcji. Te elementy interfejsu może służyć do pytania użytkowników proste i prowadzą użytkowników przez zadania.
