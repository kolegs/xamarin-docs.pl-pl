---
title: Aktualizowanie aplikacji w systemach iOS 11
description: Poznawanie nowych funkcji systemu IOS 11
ms.topic: article
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: cced7cc3d1b0579c36d598ef0d05da872478c8dc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="updating-your-app-to-ios-11"></a>Aktualizowanie aplikacji w systemach iOS 11

_Poznawanie nowych funkcji systemu IOS 11_

W systemie iOS 11 Apple wprowadziła procesu Connect zaktualizowane iTunes, nowe zmiany wizualne i architektura aktualizacji. Ten przewodnik opisuje każdego z tych zmian umożliwia uzyskiwanie aktualizacji dla systemu iOS 11 dla swojej aplikacji platformy Xamarin.iOS.

## <a name="architecture-changesarchitecture-changesmd"></a>[Architektura zmiany](architecture-changes.md)

Jednym z najważniejszych zmian, które należy zwrócić uwagę z systemem iOS 11 jest amortyzacja obsługa 32-bitowych aplikacji, zgodnie z opisem w [firmy Apple](https://developer.apple.com/news/?id=06282017b) naciśnij wersji.

Ten przewodnik przeprowadzi Cię przez aktualizowanie aplikacji dla wersji 64-bitowych.

## <a name="visual-design-updatesvisual-designmd"></a>[Projekt Visual aktualizacji](visual-design.md)

W systemie iOS 11 Firma Apple wprowadziła nowe zmiany wizualne w tym aktualizacje na pasku nawigacyjnym, pasek wyszukiwania i widoków tabel. Dodatkowo udoskonalono aby umożliwić większą elastyczność za pośrednictwem marginesy i pełny ekran zawartości. Te zmiany zostały uwzględnione w tym przewodniku.

## <a name="app-store-changesapp-store-changesmd"></a>[Zmiany w sklepie z aplikacjami](app-store-changes.md)

IOS App Store miał pełną one, które nie tylko umożliwia wydajne Przejdź magazynu, ale można też, Deweloper, aby promować aplikację dla użytkowników. Promocje te obejmują aktualizacje na zakupy w aplikacji i aktualizacji do strony produktu. iOS 11 dodaje również aktualizacje dotyczące sposób komunikowania się z użytkownikami, jak dodać ikonę Twojej aplikacji i jak zwolnić aplikacji publicznie.

## <a name="app-icon-updates"></a>Aktualizacje ikona aplikacji

> [!NOTE]
> Ikony aplikacji powinny teraz być dostarczona przez _katalogu zasobów_. 

Informacji na temat używania katalogi zasobów można znaleźć [ikonę aplikacji sklepu](~/ios/app-fundamentals/images-icons/app-store-icon.md) przewodnik. Aby uzyskać pomoc dotyczącą migracji ikon z Info.plist do katalogu zasobów, zobacz [migrowania Info.plist do zawartości katalogów](~/ios/app-fundamentals/images-icons/app-icons.md) przewodnik.

Ikona wymagany w katalogu zasobów o nazwie **sklepu z aplikacjami** i powinna być 1024 x 1024 rozmiar. Apple ma już wspomniano, czy ikona magazynu aplikacji w katalogu zasobów nie może być przezroczysty, ani zawierać kanału alfa.

![Aplikacja przechowywać położenie ikony w katalogu zasobów.](images/image1.png)

## <a name="related-links"></a>Linki pokrewne

- [Nowości w systemie iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Strona produktu zaktualizowane sklepu App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualizowanie aplikacji dla systemu iOS 11 (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/204/)
