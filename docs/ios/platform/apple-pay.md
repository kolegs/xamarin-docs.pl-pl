---
title: Apple Pay w rozszerzeniu Xamarin.iOS
description: Tym przewodniku opisano sposób konfigurowania środowiska platformy Xamarin.iOS do użytku z programem Apple Pay na płacenie za towary fizyczne, takie jak żywności, Rozrywka i grupach za pośrednictwem aplikacji. Zawiera informacje na temat wymagany, identyfikatory, certyfikaty i uprawnień.
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: f2a38a4305aa11c78f3e4e35265a86dc71642777
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351668"
---
# <a name="apple-pay-in-xamarinios"></a>Apple Pay w rozszerzeniu Xamarin.iOS

_Tym przewodniku opisano sposób konfigurowania środowiska platformy Xamarin.iOS do użytku z programem Apple Pay na płacenie za towary fizyczne, takie jak żywności, Rozrywka i grupach za pośrednictwem aplikacji. Zawiera informacje na temat wymagany, identyfikatory, certyfikaty i uprawnień._

Apple Pay został wprowadzony wraz z systemem iOS 8, umożliwiając użytkownikom na płacenie za towary fizyczne, takie jak żywności, Rozrywka i grupach za pomocą ich urządzeń z systemem iOS. Jest ona dostępna na urządzeniach iPhone 6 i iPhone 6 Plus, a także mogą być parowane ze Apple Watch za zakupy w sklepie. W przypadku użycia na telefonie iPhone, używa funkcji Touch ID jako sposób, aby potwierdzić, a następnie autoryzować transakcji użytkownika kredytowej lub debetowej.

## <a name="requirements"></a>Wymagania

Apple Pay dostępną tylko w systemie iOS 8 lub nowszym i dlatego wymaga co najmniej Xcode 6.

Wymagane do integracji Apple Pay z Twoją aplikacją są również następujące elementy:

 - Platforma procesora płatności
 - Identyfikator handlowca
 - Certyfikat usługi Apple Pay
 - Uprawnienie Apple Pay

W tym dokumencie będzie wyglądać na te elementy, które bardziej szczegółowo.

## <a name="differences-between-apple-pay-and-iap"></a>Różnice między Apple Pay i IAP

Główną różnicą między Apple Pay i *zakupy w aplikacjach* (IAP) odnoszą się do produktów, które sprzedaży. *Fizyczne* towary są sprzedawane za pośrednictwem Apple Pay; żywności, zakwaterowanie i rozrywka fizyczne (takie jak bilety kinowych) należą do tego. Z kolei sprzedaje IAP *wirtualnego* towarów, takich jak premium lub dodatkowej zawartości i subskrypcje — Pomyśl dodatkowe miesięcy usługa przesyłania strumieniowego lub dodatkowe życie w grze.

Struktury używane są również Najważniejszą różnicą; [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) jest używana do Apple Pay, podczas gdy [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) zapewnia platformę interfejsu API do IAP.

Za pomocą Apple Pay Apple [stany](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) czy go "są pobierane opłaty użytkowników, temu handlowcy mogą tworzyć lub deweloperów firmy Apple Pay na użytek płatności". W odróżnieniu od IAP ma 30% opłata za każdą transakcję. Ponadto, za pomocą Apple Pay, transakcja nie przechodzi przez firmy Apple na wszystkich, zamiast tego przechodzi on przez platformę płatności.

## <a name="using-a-payment-processor-platform"></a>Przy użyciu platformy procesora płatności

Jednym z podstawowych części Apple Pay jest przetwarzanie płatności. Choć jest możliwe zrobić to samodzielnie, wymaga znaczących wiedzę na temat kryptografii
- jak wyjaśniono w firmy Apple [przetwarzania płatności przewodnik](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html).
Platform przetwarzania płatności, z drugiej strony, obsługi tych operacjach dla Ciebie, co pozwala skoncentrować się na kompilowaniu swojej aplikacji.

Dostępne są dwie opcje:

- **STRIPE** — Zarejestruj się pod adresem [Stripe.com](https://stripe.com/) dostęp do swoich interfejsów API.

- **JudoPay** — zapoznaj się z ich [Xamarin przykładowego kodu w serwisie github](https://github.com/Judopay/Xamarin-Sample-App)i zarejestrować się bezpłatnie [JudoPay.com](https://www.judopay.com/).

## <a name="provisioning-for-apple-pay"></a>Inicjowanie obsługi administracyjnej Apple Pay

Konfigurowanie aplikacji do używania Apple Pay wymaga ustawienia w portalu dla deweloperów firmy Apple i w aplikacji. Istnieje kilka kroków, które powinien być używany do pomyślnego zainicjowania obsługi aplikacji Apple pay:

1. Utwórz identyfikator handlowca:
    - Postępuj zgodnie z instrukcjami [tutaj](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Utwórz identyfikator aplikacji dzięki możliwości zastosowania płacić i Dodaj handlowca do niego:
    - Postępuj zgodnie z instrukcjami [tutaj](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. Wygenerowanie certyfikatu dla Identyfikatora handlowca:
    - Postępuj zgodnie z instrukcjami [tutaj](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. Generuj profil aprowizacji przy użyciu nowo utworzony identyfikator aplikacji:
    - Postępuj zgodnie z instrukcjami [tutaj](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Dodaj Apple Pay uprawnienia:
    - Wybierz uprawnienie Apple płatność zgodnie z opisem [tutaj](~/ios/deploy-test/provisioning/entitlements.md), albo ręcznie Dodaj parę klucza i wartości do pliku z [tutaj](~/ios/deploy-test/provisioning/entitlements.md)

## <a name="working-with-apple-pay"></a>Praca z Apple Pay

Apple wprowadziła kilka ulepszeń usługi Apple Pay w systemie iOS 10, która pozwala użytkownikowi dokonanie bezpieczne płatności z witryn sieci Web i za pomocą interakcji z Siri oraz Maps.

Z systemem iOS 10 kilka nowych interfejsów API dodano współpracujące z systemów iOS, jak i systemu watchOS do obsługi płatności dynamiczne sieci i nowego środowiska testowego piaskownicy.

### <a name="apple-pay-website-integration"></a>Integracja z witryny sieci Web firmy Apple Pay

Nowe z systemem iOS 10, deweloper można zastosować Apple Pay bezpośrednio do ich witryn sieci Web przy użyciu **ApplePay JS**. Użytkownicy przeglądający witrynę sieci Web w przeglądarce Safari w systemie iOS lub macOS można wprowadzać płatności z Apple Pay weryfikowanie transakcji na swoim telefonie iPhone lub Apple Watch. Aby uzyskać więcej informacji, zobacz firmy Apple [odwołanie struktury ApplePay JP](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>Ulepszenia PassKit Framework

W systemie iOS 10, została rozwinięta PassKit framework obsługuje Apple Pay poza `UIKit` oraz o zezwolenie wystawców kart prezentować swoje własne karty w swoich aplikacjach.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>Obsługa Apple Pay poza UIKit

Za pomocą [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) i [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), aplikacja może obsługiwać te same funkcje, które są dostarczane przez [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) bez użycia UIKit. Chociaż ten nowy interfejs API jest wymagany do obsługi Apple Pay, Apple Watch (i w określonych intencji także), jest opcjonalny w innych sytuacjach (np. istniejące aplikacje). Jednak Apple sugeruje, przejście do nowego interfejsu API tak szybko, jak to możliwe zapewnienie obsługi szerokiej gamy Apple Pay we wszystkich aplikacji dla deweloperów przy użyciu jednej bazy kodu. Aby uzyskać więcej informacji na temat intencje i używanie programu siri po integracji, zobacz nasze [wprowadzenie do zestawu SiriKit](~/ios/platform/sirikit/index.md) dokumentacji.

#### <a name="presenting-issuer-cards-from-within-apps"></a>Umożliwienie korzystania z karty wystawcy w aplikacjach

Z systemem iOS 10 zostały dodane nowe funkcje do framework PassKit, dzięki czemu wystawców kart do przedstawienia ich karty w ramach ich własnych aplikacji. Deweloper może dodać `PKPaymentButtonTypeInStore` obiektu klasy UIButton do interfejsu użytkownika aplikacji, który zostanie wyświetlony przycisk Apple Pay dla karty.

`PresentPaymentPass` Metody [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) klasy może również służyć do programowe wyświetlanie karty.

### <a name="new-payment-network-support"></a>Obsługa nowych sieci płatności

Nowe z systemem iOS 10, aplikacja może automatycznie obsługuje nową sieć płatności po jej udostępnieniu bez developer konieczności zmodyfikować, ponownie skompilować aplikację i ponownie prześlij go do Store aplikacji.

Nowy [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) metody `PKPaymentNetwork` klasa umożliwia aplikacji do odnajdywania sieci, które są dostępne na urządzeniu użytkownika w czasie wykonywania. Ponadto [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) właściwość została rozszerzona, aby wykonać nazwę dostawcy płatności jako argument. Przy użyciu tych metod, aplikacja może automatycznie obsługują żadną siecią, która obsługuje dostawcy płatności.

Aby uzyskać więcej informacji, zobacz nasze [konfiguracji płatności firmy Apple](~/ios/platform/apple-pay.md) i firmy Apple [przewodnik płacić Apple](https://developer.apple.com/apple-pay/).

### <a name="new-testing-environment"></a>Nowe środowisko testowe

Z systemem iOS 10 Apple wprowadziła nowy środowisku testowym, który umożliwia deweloperom aprowizować kart płatniczych testu bezpośrednio na urządzeniu z systemem iOS. Nowego środowiska testowania zwraca testu zaszyfrowane dane o płatności do aplikacji.

Aby włączyć nowe środowisko testowe, wykonaj następujące czynności:

1. Tworzenie usługi iCloud testowania nowego konta w usłudze iTunes Connect.
2. Zaloguj się na urządzeniu z systemem iOS przy użyciu nowego konta testowania.
3. Ustaw odpowiedni region, aby przetestować aplikację w.
4. Użyj jednej z kart płatniczych testu z [przewodnik płacić Apple](https://developer.apple.com/apple-pay/) do dokonywania płatności.

> [!IMPORTANT]
> Przełączając konta usługi iCloud, urządzenie automatycznie przełączą się do nowego środowiska testowego. Jednak nadal Apple **wymaga** aplikację, aby ją przetestować przy użyciu rzeczywistego karty w środowisku produkcyjnym przed przesłaniem do usługi iTunes App Store.

## <a name="summary"></a>Podsumowanie

W tym artykule rozważyliśmy różne elementy wymagane do użycia usługi Apple Pay wiadomości w aplikacji. Zobaczyliśmy, jak utworzyć identyfikator handlowca i sposobie ich użycia w ramach **plik Entitlements.plist**, który ma zostać zmodyfikowana ręcznie.

## <a name="related-links"></a>Linki pokrewne

- [Zakupy w aplikacjach](~/ios/platform/in-app-purchasing/index.md)
- [Wprowadzenie do PassKit](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (przykład)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
