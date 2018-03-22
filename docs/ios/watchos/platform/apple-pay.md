---
title: Apple Pay na watchOS
description: "W tym artykule omówiono ulepszenia Apple wprowadziła Apple Pay w watchOS 3 oraz sposób ich wdrażania w Xamarin.iOS dla Apple Watch."
ms.topic: article
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 591f6f53c9e787ee9499b2a1a3cc812f7e72749a
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="apple-pay-on-watchos"></a>Apple Pay na watchOS

Apple wprowadziła kilka ulepszeń Apple Pay w watchOS 3, który dodaje obsługę płatności w aplikacji. Dzięki temu użytkownikowi bezpiecznego Podaj płatności i skontaktuj się z informacji zapłacenia bezpośrednio z Apple Watch towarów fizycznych i usług.


## <a name="about-apple-pay-enhancements"></a>Temat udoskonaleń dotyczących płatności firmy Apple

Jako Stated powyżej Apple wprowadziła kilka ulepszeń Apple Pay w watchOS 3, które umożliwiają bezpieczne płatności i skontaktuj się z informacji zapłacenia bezpośrednio z Apple Watch towarów fizycznych i usług. Rozszerzenia te są dostarczane przez modyfikacje w ramach PassKit.

IOS 10 i watchOS 3 kilka nowych interfejsów API dodano pracy z systemem iOS i watchOS do obsługi sieci dynamiczne płatności i nowego środowiska testowego piaskownicy.

## <a name="passkit-framework-enhancements"></a>Ulepszenia PassKit Framework

W systemie iOS 10, PassKit framework zostały rozszerzone do obsługi Apple Pay poza `UIKit` oraz w celu umożliwienia wystawców kart do prezentowania ich karty w swoich aplikacjach. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>Obsługa płatności Apple poza UIKit

Za pomocą [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) i [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), aplikacja może obsługiwać te same funkcje udostępniane przez [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) bez użycia UIKit. Ten nowy interfejs API jest wymagana do obsługi Apple Pay na Apple Watch (i w określonej lokalizacji docelowych również), jest opcjonalna w innych sytuacjach (np. istniejące aplikacje). Jednak Apple sugeruje przeniesienie na nowy interfejs API jak najszybciej zapewnia Apple Pay we wszystkich aplikacji dewelopera na bazie pojedynczy kod. Aby uzyskać więcej informacji o lokalizacji docelowych i używanie programu Siri integracji, zobacz nasze [wprowadzenie do SiriKit](~/ios/platform/sirikit/index.md) dokumentacji.

### <a name="presenting-issuer-cards-from-within-apps"></a>Wystawcy kart z aplikacji prezentacji

IOS 10 i watchOS 3 nowe funkcje zostały dodane do struktury PassKit umożliwiająca wystawców kart do prezentowania ich karty płatności w swoich aplikacjach. Deweloper może dodać `PKPaymentButtonTypeInStore` UIButton do interfejsu użytkownika aplikacji spowoduje wyświetlenie przycisku Apple Pay dla karty.

`PresentPaymentPass` Metody [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) klasy może również służyć do programowane wyświetlanie karty.

## <a name="new-payment-network-support"></a>Nowa funkcja obsługi sieci płatności

New iOS 10 i watchOS 3, aplikacja może automatycznie obsługują nową sieć płatności po ich udostępnieniu bez developer konieczności modyfikowania, ponownie skompilować aplikację i ponownie prześlij go do sklepu z aplikacjami.

Nowy [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) metody `PKPaymentNetwork` klasy, dzięki czemu aplikacja do odnajdywania sieci, które są dostępne na urządzeniu użytkownika w czasie wykonywania. Ponadto [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) właściwość została rozszerzona, aby wykonać nazwy dostawcy płatności jako argumentu. Za pomocą tych metod, aplikację można automatycznie obsługują dowolnej sieci obsługującej dostawcę płatności.

Aby uzyskać więcej informacji, zobacz nasze [zapłacić konfiguracji firmy Apple](~/ios/platform/apple-pay.md) i firmy Apple [firmy Apple należy zwrócić przewodnik](https://developer.apple.com/apple-pay/).

## <a name="new-testing-environment"></a>Nowe środowisko testowania

W przypadku systemu iOS 10 i watchOS 3 Firma Apple wprowadziła nowego środowiska testowego, który umożliwia deweloperowi udostępnić płatności testowe bezpośrednio na urządzeniu z systemem iOS. Nowego środowiska testowego następnie zwraca testu zaszyfrowanych danych płatności w aplikacji.

Aby włączyć nowego środowiska testowego, wykonaj następujące czynności:

1. Utwórz iCloud testowania nowego konta w iTunes Connect.
2. Zaloguj się do urządzenia z systemem iOS z nowym kontem testowych.
3. Ustaw odpowiedni region do testowania aplikacji.
4. Użyj jednej z kart płatniczych testu z [firmy Apple należy zwrócić przewodnik](https://developer.apple.com/apple-pay/) dokonanie płatności.

> [!NOTE]
> Przełączając iCloud kont, urządzenie zostanie automatycznie przełączona do nowego środowiska testowego. Jednak nadal Apple **wymaga** aplikacji ma zostać przetestowana, do rzeczywistych karty w środowisku produkcyjnym przed ich przesłaniem do iTunes App Store.

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego ulepszenia Apple wprowadziła Apple Pay w watchOS 3 oraz sposób ich wdrażania w platformy Xamarin.iOS.
