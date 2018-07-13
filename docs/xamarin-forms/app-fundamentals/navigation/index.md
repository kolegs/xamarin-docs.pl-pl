---
title: Nawigacja zestawu narzędzi Xamarin.Forms
description: W tym przewodniku objaśniono sposób przeprowadzania nawigacji w aplikacji platformy Xamarin.Forms. Zestaw narzędzi Xamarin.Forms oferuje pewną liczbę innej strony nawigacji środowisk, w zależności od użytej strony.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 202f044ebd7dd5b110b94d2aa60eeb7151150607
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994731"
---
# <a name="xamarinforms-navigation"></a>Nawigacja zestawu narzędzi Xamarin.Forms

_Zestaw narzędzi Xamarin.Forms oferuje pewną liczbę innej strony nawigacji środowisk, w zależności od użytej strony._

![](images/page-types.png "Typy stron zestawu narzędzi Xamarin.Forms")

## <a name="hierarchical-navigationhierarchicalmd"></a>[Nawigacja hierarchiczna](hierarchical.md)

[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Klasa udostępnia środowisko Nawigacja hierarchiczna, gdzie użytkownik jest powinni poruszać się po stronach przodu i do tyłu, zgodnie z potrzebami. Klasa implementuje nawigacji jako ostatni na wejściu, first-out (LIFO) stos [ `Page` ](xref:Xamarin.Forms.Page) obiektów.

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) składa się z listy kart i większy obszar szczegółów, o każdej karcie ładowanie zawartości w obszarze szczegółów.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) to strona, który użytkownicy mogą szybko przesuń bok przechodzenie przez strony zawartości, takich jak galerii.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) to strona, która zarządza dwie strony z powiązanymi informacjami — strony wzorcowej, która przedstawia elementy i stronę szczegółów, która przedstawia szczegółowe informacje na temat elementów na stronie wzorcowej.

## <a name="modal-pagesmodalmd"></a>[Strony modalne](modal.md)

Zestaw narzędzi Xamarin.Forms obsługuje również strony modalne. Strony modalne zaleca użytkownikom ukończenie niezależna zadanie, które nie mogą być opuszczeniu aż zadanie jest ukończone lub anulowane.

## <a name="displaying-pop-upspop-upsmd"></a>[Wyświetlanie wyskakujących okienek](pop-ups.md)

Zestaw narzędzi Xamarin.Forms zawiera dwa elementy interfejsu użytkownika podręcznego dokonywania podobne: alert i arkuszu akcji. Te elementy interfejsu może służyć do pytania użytkownikom proste i prowadzą użytkowników przez zadania.
