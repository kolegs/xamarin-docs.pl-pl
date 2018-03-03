---
title: "Płatności firmy Apple"
description: "Ten przewodnik opisuje Konfigurowanie środowiska platformy Xamarin.iOS do użycia z programem Apple Pay płacisz za towarów fizycznych, takich jak żywności, rozrywce i członkostwa za pośrednictwem aplikacji. Zawiera informacje dotyczące wymaganych identyfikatorów, certyfikaty i uprawnień."
ms.topic: article
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 0ac2a19e9020113df273897a8ec2c86ee1763ec2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="apple-pay"></a>Płatności firmy Apple

_Ten przewodnik opisuje Konfigurowanie środowiska platformy Xamarin.iOS do użycia z programem Apple Pay płacisz za towarów fizycznych, takich jak żywności, rozrywce i członkostwa za pośrednictwem aplikacji. Zawiera informacje dotyczące wymaganych identyfikatorów, certyfikaty i uprawnień._


Apple Pay został wprowadzony wraz z systemem iOS 8, umożliwiając użytkownikom opłacać towarów fizycznych, takich jak żywności, rozrywce i członkostwa za pomocą ich urządzeń z systemem iOS. Jest ona dostępna na telefonie iPhone 6 i iPhone 6 Plus, a także mogą łączyć się z Apple Watch dotyczące zakupów w sklepie. Gdy jest używany na telefonie iPhone, używa funkcji Touch ID jako sposób potwierdzić i autoryzować transakcji użytkownika kredytową lub debetową.


## <a name="requirements"></a>Wymagania

Apple Pay znajduje się tylko w systemie iOS 8 lub nowszym i dlatego wymaga co najmniej Xcode 6.

Następujące elementy są również wymagane do integracji Apple Pay w swojej aplikacji:

 - Platforma procesora płatności
 - Identyfikator handlowe
 - Certyfikat Apple Pay
 - Apple Pay uprawnień

Ten dokument będzie wyglądać na te elementy bardziej szczegółowo.

## <a name="differences-between-apple-pay-and-iap"></a>Różnice między a IAP firmy Apple

Główną różnicą między Apple Pay i *zakupu w aplikacji* (IAP) odnoszą się do produktów, które sprzedaży. *Fizyczny* towarów są sprzedanych za pośrednictwem Apple Pay; żywności, zakwaterowania i rozrywka fizyczne (na przykład biletów kinowych) należą do nich to. Z kolei sprzedaje IAP *wirtualnego* towarów, takich jak premium lub dodatkowe zawartości i subskrypcje — Pomyśl miesięcy dodatkowe usługi przesyłania strumieniowego lub dodatkowe życie w grę.

Struktury używane są również Najważniejsza różnica; [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) jest używana na potrzeby Apple Pay, podczas gdy [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) zapewnia platformę interfejsu API IAP.

Z Apple Pay Apple [stanów](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) czy go "nalicza użytkowników, sprzedawców lub deweloperów firmy Apple płatności na potrzeby płatności". Z kolei IAP ma opłat 30% dla każdej transakcji. Ponadto z Apple Pay, transakcja nie przechodzi przez firmy Apple w ogóle, natomiast przechodzi ona przez platformę płatności.


## <a name="using-a-payment-processor-platform"></a>Przy użyciu platformy procesora płatności

Jednym z podstawowych części Apple Pay jest przetwarzanie płatności. Mimo że jest to możliwe zrobić to samodzielnie, wymaga znaczących wiedzę na temat kryptografii
- jak wyjaśniono w firmy Apple [przetwarzania płatności przewodnik](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html).
Platform przetwarzania płatności, z drugiej strony, obsługiwać te operacje dla Ciebie, umożliwiając skoncentrować się na tworzeniu aplikacji.

Dostępne są dwie opcje:

- **Aplikacja STRIPE** -utworzyć konto w [Stripe.com](https://stripe.com/) dostępu do ich interfejsów API.

- **JudoPay** — zapoznaj się z ich [Xamarin przykładowy kod w serwisie github](https://github.com/Judopay/Xamarin-Sample-App)i zarejestruj pod adresem [JudoPay.com](https://www.judopay.com/).


## <a name="provisioning-for-apple-pay"></a>Inicjowanie obsługi płatności firmy Apple

Konfigurowanie aplikacji przy użyciu Apple Pay wymaga ustawienia w portalu dla deweloperów firmy Apple i aplikacji. Istnieje wiele kroków, które powinny być zachowana w celu pomyślnie udostępniania aplikacji dla płatności firmy Apple:

1. Utwórz identyfikator handlowe:
    - Postępuj zgodnie z instrukcjami [tutaj](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Utwórz identyfikator aplikacji dzięki możliwości zastosowania płatności i Dodaj sprzedawcy do niej:
    - Postępuj zgodnie z instrukcjami [tutaj](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. Wygeneruj certyfikat dla Identyfikatora sprzedawcy:
    - Postępuj zgodnie z instrukcjami [tutaj](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. Generuj profil inicjowania obsługi administracyjnej z nowo utworzony identyfikator aplikacji:
    - Postępuj zgodnie z instrukcjami [tutaj](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Dodaj Apple Pay uprawnień:
    - Wybierz uprawnienia Apple płatności zgodnie z opisem [tutaj](~/ios/deploy-test/provisioning/entitlements.md), lub ręcznie Dodaj parę klucza i wartości do pliku z [tutaj](~/ios/deploy-test/provisioning/entitlements.md)


## <a name="working-with-apple-pay"></a>Praca z płatności firmy Apple

Apple wprowadziła kilka ulepszeń Apple Pay w systemie iOS 10, który umożliwia użytkownikowi dokonanie bezpieczne płatności z witryn sieci Web i dzięki współdziałaniu z Siri i map.

Z systemem iOS 10 kilka nowych interfejsów API dodano pracy z systemem iOS i watchOS do obsługi sieci dynamiczne płatności i nowego środowiska testowego piaskownicy.


### <a name="apple-pay-website-integration"></a>Integracja z witryny sieci Web firmy Apple płatności

Nowość w systemach iOS 10, deweloper można zastosować Apple Pay bezpośrednio w swoich witryn sieci Web przy użyciu **ApplePay JS**. Użytkownicy przeglądający witrynę sieci Web z przeglądarki Safari w systemie iOS lub macOS wprowadzić płatności z Apple Pay weryfikując transakcji na telefonie iPhone lub Apple Watch. Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania Framework ApplePay JP](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>Ulepszenia PassKit Framework

W systemie iOS 10, PassKit framework zostały rozszerzone do obsługi Apple Pay poza `UIKit` oraz w celu umożliwienia wystawców kart prezentować swoje karty z ich aplikacji.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>Obsługa płatności Apple poza UIKit

Za pomocą [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) i [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), aplikacja może obsługiwać te same funkcje udostępniane przez [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) bez użycia UIKit. Ten nowy interfejs API jest wymagana do obsługi Apple Pay na Apple Watch (i w określonej lokalizacji docelowych również), jest opcjonalna w innych sytuacjach (np. istniejące aplikacje). Jednak Apple sugeruje przeniesienie na nowy interfejs API jak najszybciej zapewnia Apple Pay we wszystkich aplikacji dewelopera na bazie pojedynczy kod. Aby uzyskać więcej informacji o lokalizacji docelowych i używanie programu Siri integracji, zobacz nasze [wprowadzenie do SiriKit](~/ios/platform/sirikit/index.md) dokumentacji.

#### <a name="presenting-issuer-cards-from-within-apps"></a>Wystawcy kart z aplikacji prezentacji

Z systemem iOS 10 zostały dodane nowe funkcje do struktury PassKit umożliwiająca wystawców kart do prezentowania ich karty w swoich aplikacjach. Deweloper może dodać `PKPaymentButtonTypeInStore` UIButton do interfejsu użytkownika aplikacji spowoduje wyświetlenie przycisku Apple Pay dla karty.

`PresentPaymentPass` Metody [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) klasy może również służyć do programowane wyświetlanie karty.

### <a name="new-payment-network-support"></a>Nowa funkcja obsługi sieci płatności

Nowość w systemach iOS 10, aplikacja może automatycznie obsługują nową sieć płatności po ich udostępnieniu bez developer konieczności modyfikowania, ponownie skompilować aplikację i ponownie prześlij go do sklepu z aplikacjami.

Nowy [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) metody `PKPaymentNetwork` klasy, dzięki czemu aplikacja do odnajdywania sieci, które są dostępne na urządzeniu użytkownika w czasie wykonywania. Ponadto [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) właściwość została rozszerzona, aby wykonać nazwy dostawcy płatności jako argumentu. Za pomocą tych metod, aplikację można automatycznie obsługują dowolnej sieci obsługującej dostawcę płatności.

Aby uzyskać więcej informacji, zobacz nasze [zapłacić konfiguracji firmy Apple](~/ios/platform/apple-pay.md) i firmy Apple [firmy Apple należy zwrócić przewodnik](https://developer.apple.com/apple-pay/).

### <a name="new-testing-environment"></a>Nowe środowisko testowania

Z systemem iOS 10 Firma Apple wprowadziła nowego środowiska testowego, który umożliwia deweloperowi udostępnić płatności testowe bezpośrednio na urządzeniu z systemem iOS. Nowego środowiska testowego następnie zwraca testu zaszyfrowanych danych płatności w aplikacji.

Aby włączyć nowego środowiska testowego, wykonaj następujące czynności:

1. Utwórz iCloud testowania nowego konta w iTunes Connect.
2. Zaloguj się do urządzenia z systemem iOS z nowym kontem testowych.
3. Ustaw odpowiedni region do testowania aplikacji.
4. Użyj jednej z kart płatniczych testu z [firmy Apple należy zwrócić przewodnik](https://developer.apple.com/apple-pay/) dokonanie płatności.

> [!IMPORTANT]
>  **Uwaga:** przełączając iCloud kont, urządzenie zostanie automatycznie przełączona do nowego środowiska testowego. Jednak nadal Apple **wymaga** aplikacji ma zostać przetestowana, do rzeczywistych karty w środowisku produkcyjnym przed ich przesłaniem do iTunes App Store.

## <a name="summary"></a>Podsumowanie

W tym artykule firma Microsoft zbadane różne elementy wymagane do używania Apple Pay w Twojej aplikacji. Analizujemy jak utworzyć identyfikator handlowych i sposobie ich użycia w ramach **Entitlements.plist**, która ma zostać zmodyfikowana ręcznie.


## <a name="related-links"></a>Linki pokrewne

- [Zakupy w aplikacjach](~/ios/platform/in-app-purchasing/index.md)
- [Wprowadzenie do usługi PassKit](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (przykład)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
