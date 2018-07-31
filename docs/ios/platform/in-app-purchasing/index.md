---
title: Zakup w rozszerzeniu Xamarin.iOS w aplikacji
description: W tym dokumencie opisano sposób sprzedawać cyfrowego produktów i usług, za pomocą interfejsów API StoreKit. Łączy on przewodniki z instrukcjami, które mówią o konfiguracji, produktów w użyciu, produktów innych niż usprawnia, transakcje, subskrypcji i więcej.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 102ff2f11cc2f3d536e3ce9dd595a881f370f764
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351424"
---
# <a name="in-app-purchasing-in-xamarinios"></a>Zakup w rozszerzeniu Xamarin.iOS w aplikacji

aplikacje dla systemu iOS mogą sprzedawać cyfrowego produktów lub usług, za pomocą StoreKit — zestaw interfejsów API dostarczonych przez system iOS, które komunikują się z serwerami firmy Apple do przeprowadzenia transakcji finansowych z użytkownikiem za pośrednictwem podanie identyfikatora Apple ID. Interfejsy API StoreKit dotyczy głównie podczas pobierania informacji o produkcie i przeprowadzania transakcji — Brak nie składników interfejsu użytkownika. Aplikacje, które implementują zakupu w aplikacji należy tworzyć interfejsy użytkownika i śledzić zakupione elementy kodu niestandardowego, aby zapewnić wymagane produktów lub usług dla użytkownika.

Dostarczanie funkcji zakupu w aplikacji wymaga kilku kroków:

-  **Konfigurowanie aplikacji** — profilu inicjowania obsługi administracyjnej aplikacji musi być poprawnie skonfigurowany.
-  **Tworzenie produktów** — opisy produktów i cen, które muszą zostać utworzone w portalu usługi iTunes Connect.
-  **Implementowanie StoreKit** — interfejs API StoreKit musi zostać wdrożone zgodnie z typów produkty sprzedawane.
-  **Tworzenie interfejsu użytkownika i same produkty** — produkty muszą być zaimplementowane, łącznie z mechanizmów śledzenia każdego zakupu i wykonywania kopii zapasowej/przywracania je stosownie do.
-  **Monitorowanie sprzedaży i uzyskania środków** — Użyj informacjom zawartym w usłudze iTunes Connect, aby monitorować trendy sprzedaży i śledzić przychodów.

W tym dokumencie wyjaśniono, jak wykonać te kroki zapewnienie, że zakupy w aplikacji przy użyciu rozszerzenia Xamarin.iOS.

## <a name="requirements"></a>Wymagania

Obsługa zakupów w aplikacji możesz korzystać Xamarin.iOS 5.0 lub nowszym z Xcode 7 lub nowszym.

## <a name="contents"></a>Spis treści

 * [Konfiguracja i podstawowe informacje dotyczące zakupów w aplikacji](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [Omówienie StoreKit i pobierania informacji o produkcie](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [Zakup produktów, które można wykorzystać](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [Zakup produktów, których nie można wykorzystać](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [Transakcje i weryfikacja](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [Subskrypcje i raportowanie](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>Podsumowanie

Ten artykuł ma wprowadzono koncepcję zakupy w aplikacjach, opisano jak skonfigurować aplikację, aby z niej korzystać i przedstawione przykłady użycia platformy Xamarin.iOS. Oceniono:

-  **Portal Aprowizowania dla systemu iOS** — wskazówki dotyczące włączania aplikacji zakupu funkcji.
-  **iTunes Connect** — konfigurowania produktów do sprzedaży w Twojej aplikacji.
-  **Store Kit** — omówienie klas używany do tworzenia funkcji zakupu w aplikacji.
-  **Kodowanie aplikacji zakupu** — przykłady sposobu tworzenia zakupu w aplikacji do aplikacji platformy Xamarin.iOS.
-  **Raportowanie** — omówienie statystyk, które są dostępne za pośrednictwem programu iTunes Connect.


## <a name="related-links"></a>Linki pokrewne

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [W Podręczniku programowania zakup aplikacji](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect — przewodnik dewelopera](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Odwołanie do zestawu platformy Store](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [Identyfikatory produktu zakupu w aplikacji, pytania i odpowiedzi](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Uwaga techniczna zakupu w aplikacji](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [Przesyłanie pierwszy App Store](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [Centrum zasobów usługi App Store](https://developer.apple.com/appstore/index.html)
- [Porady dotyczące przesyłania App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Wytyczne dotyczące sklepu App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Zarządzanie aplikacjami](https://developer.apple.com/appstore/resources/managing/index.html)
