---
title: Wprowadzenie do systemu iOS 10
description: "W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w systemie iOS 10 dla deweloperów platformy Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: d5618ad4477cadfe8977faa9616f5ab6201f14ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-10"></a>Wprowadzenie do systemu iOS 10

_W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w systemie iOS 10 dla deweloperów platformy Xamarin.iOS._

## <a name="introducing-ios-10"></a>Wprowadzenie do systemu iOS 10

Z nowego iOS 10 SDK, Apple dołączył nowych interfejsów API i usług, które umożliwiają deweloperom tworzenie nowej kategorii aplikacji i funkcji. Aplikacja systemu iOS można rozszerzyć teraz aplikacji wiadomości, Siri, Phone i map umożliwiają korzystanie z funkcji zaawansowanych, atrakcyjne dla użytkownika końcowego, który był wcześniej niedostępne.

Aby uzyskać więcej informacji w systemie iOS 10, zobacz firmy Apple [iOS + aplikacji](https://developer.apple.com/ios/) dokumentacji.

## <a name="whats-new-in-ios-10"></a>Nowości w systemie iOS 10

Apple został dodany kilka nowych interfejsów API i usług w systemie iOS 10 wraz z wielu ulepszenia istniejących funkcji, w tym:


## <a name="adapting-to-the-true-tone-display"></a>Dostosowania do wyświetlenia True sygnału

Technologia True wyświetlić sygnału firmy Apple używa czujnika światła w urządzeniu z systemem iOS można dynamicznie zmieniać, kolor i intensywność wyświetlania odpowiadające warunkom oświetlenia. iOS 10 udostępnia nowe [UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31) klucz, który można dodać do aplikacji `Info.plist` plików i określa, jak True sygnału stosuje przesunięcie standardowe kolorów. 

Dostępne są następujące wartości:

- `UIWhitePointAdaptivityStyleStandard` **Domyślna** -Użyj standardowego adaptivity biały punktu.
- `UIWhitePointAdaptivityStyleReading` -Użyta do aplikacji zorientowanych na odczyt.
- `UIWhitePointAdaptivityStyleGame` -Użyta do fokus gry aplikacji.
- `UIWhitePointAdaptivityStyleVideo` -Użyta do aplikacji fokus wideo.
- `UIWhitePointAdaptivityStylePhoto` -Użyta do fokus fotografii aplikacji gdzie koloru ma większe znaczenie niż środowiska punktu biały dopasowania.

<a name="app-extensions" />

## <a name="app-extensions"></a>Rozszerzenia aplikacji

Apple udostępnia kilka nowych punktów rozszerzenia aplikacji w systemie iOS 10:

- Wywołanie katalogu
- Intencje i opcji interfejsu użytkownika
- Komunikaty
- Zawartość powiadomień
- Usługi powiadomień
- Pakiet naklejce

Ponadto 3 klawiatury rozszerzeniami aplikacji ma następujące udoskonalenia:

- Nowy `DocumentInputMode` właściwość `UITextDocumentProxy` klasy można ustalić języka dokumentu i Zezwalaj na rozszerzenie klawiatury były wyrównane z tego języka.
- Nowe `HandleInputModeList` metoda umożliwia wyświetlanie menu selektora klawiatury systemu w odpowiedzi na klucz globu trwa dotknięciu rozszerzenia klawiatury.

Aby uzyskać więcej informacji, zobacz nasze [wprowadzenie do rozszerzeń](~/ios/platform/extensions.md), [integracji aplikacji komunikat](~/ios/platform/message-app-integration/index.md), [wprowadzenie do aktywnego sugestie](~/ios/platform/search/proactive-suggestions.md), [ Wprowadzenie do SiriKit](~/ios/platform/sirikit/index.md), [wprowadzenie do powiadomień użytkownika](~/ios/platform/user-notifications/index.md) i firmy Apple [przewodnik programowania w języku rozszerzenia aplikacji](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

## <a name="app-search-enhancements"></a>Ulepszenia wyszukiwania aplikacji

Spotlight Core w systemie iOS 10 udostępnia kilka rozszerzeń takich jak do wyszukiwania aplikacji:

- **Popularne z Linkiem bezpośrednim Crowdsourced (o prywatności różnicowa)** — zapewnia sposób promowania zawartość aplikacji z linkiem bezpośrednim w wynikach wyszukiwania.
- **Wyszukiwanie w aplikacji** — Użyj nowych `CSSearchQuery` klasy, aby zapewnić możliwość wyszukiwania uwagi w aplikacji podobnie jak działają aplikacje poczty, wiadomości i notatki.
- **Wyszukaj kontynuacji** — umożliwia użytkownikowi rozpocząć wyszukiwanie Spotlight lub Safari, a następnie otwórz aplikację i kontynuować tego wyszukiwania.
- **Wizualizacja wyników weryfikacji** -firmy Apple [aplikacji wyszukiwania interfejsu API sprawdzania poprawności narzędzie](https://search.developer.apple.com/appsearch-validation-tool) podczas preforming testy są obecnie wyświetlane wizualną reprezentację znaczników witryny sieci Web, a następnie połączenie bezpośrednie.
- **Komunikat aplikacji do udostępniania obrazu** — umożliwia popularnych obrazów w aplikacji dostępne w celu udostępniania w komunikatach (za pośrednictwem rozszerzenia aplikacji wiadomości) do jego wyświetlenia na wyszukiwanie Spotlight.

Aby dowiedzieć się więcej, zobacz nasze [aplikacji wyszukiwania ulepszenia](~/ios/platform/search/app-search-enhancements.md) przewodnik.

## <a name="apple-pay-enhancements"></a>Ulepszenia płatności firmy Apple

Apple wprowadziła kilka ulepszeń Apple Pay w systemie iOS 10, który umożliwia użytkownikowi dokonanie bezpieczne płatności z witryn sieci Web i dzięki współdziałaniu z Siri i map.

Z systemem iOS 10 kilka nowych interfejsów API dodano pracy z systemem iOS i watchOS do obsługi sieci dynamiczne płatności i nowego środowiska testowego piaskownicy.

Ponadto w ramach PassKit został rozszerzony w celu obsługi Apple Pay poza `UIKit` oraz w celu umożliwienia wystawców kart do prezentowania ich karty w swoich aplikacjach.

Aby dowiedzieć się więcej, zobacz nasze [firmy Apple należy zwrócić ulepszenia](~/ios/platform/apple-pay.md) przewodnik.

## <a name="alternate-app-icons"></a>Ikony aplikacji alternatywnej

Apple został dodany kilka ulepszeń w systemach iOS 10.3 Zezwalaj aplikacji na zarządzanie jego ikonę:

 - `ApplicationIconBadgeNumber` -Pobiera lub ustawia identyfikator ikony aplikacji w Springboard.
 - `SupportsAlternateIcons` -Jeśli `true` aplikacja ma inny zestaw ikon.
 - `AlternateIconName` -Zwraca nazwę alternatywnego ikony aktualnie wybrane lub `null` Jeśli za pomocą ikony podstawowego.
 - `SetAlternameIconName` — Użyj tej metody, aby przełączyć się ikona aplikacji do danego ikona alternatywny.

Aby dowiedzieć się więcej, zobacz nasze [alternatywny ikony aplikacji](~/ios/app-fundamentals/images-icons/alternate-app-icons.md) przewodnik.


## <a name="introduction-to-callkit"></a>Wprowadzenie do CallKit

Nowy interfejs API CallKit w systemie iOS 10 umożliwia aplikacjom VOIP integrują się z iPhone interfejsu użytkownika i znanych interfejs i środowisko użytkownika końcowego. W tym interfejsie API, użytkownicy mogą wyświetlać i interakcyjnie VOIP wywołania z ekranem blokady urządzenia z systemem iOS i zarządzać kontaktami przy użyciu aplikacji Phone **ulubione** i **ostatnich** widoków.

Ponadto interfejs API CallKit zapewnia możliwość tworzenia Extensions aplikacji, które można skojarzyć numeru telefonu z nazwą (identyfikator wywołującego) lub stwierdzić, że systemu, jeśli liczba może być zablokowany (wywołanie blokowania).

Aby dowiedzieć się więcej, zobacz nasze [wprowadzenie do Callkit](~/ios/platform/callkit.md) przewodnik.

## <a name="message-app-integration"></a>Integracja aplikacji wiadomości

10 systemu iOS umożliwia włączenie rozszerzenia aplikacji wiadomości w rozwiązaniu Xamarin.iOS, która integruje się z **wiadomości** aplikacji i przedstawia nowych funkcji dla użytkownika. Rozszerzenie można wysyłać tekstu, nalepek, plików multimedialnych i interaktywne wiadomości. Dostępne są dwa typy rozszerzenia aplikacji komunikat:

- **Pakiety naklejce** — zawiera kolekcję nalepek, które użytkownik może dodać do komunikatu. Naklejce pakiety mogą być tworzone bez pisania żadnego kodu.
- **iMessage aplikacji** — może ona powodować niestandardowy interfejs użytkownika w aplikacji Messages na wybranie nalepek, wprowadzania tekstu, w tym plików multimedialnych (z konwersje typów opcjonalne) i tworzeniem, edytowaniem i wysyłanie komunikatów interakcji.

Aby dowiedzieć się więcej, zobacz nasze [integracji aplikacji komunikat](~/ios/platform/message-app-integration/index.md) przewodnik.

## <a name="news-publisher-enhancements"></a>Ulepszenia wydawcy wiadomości

Z systemem iOS 10 Apple zezwala wszystkim z głównych magazyny i nowych organizacji autorów blogów i niezależnych wydawców, aby zarejestrować się i produktu i dostarczania zawartości na aplikacji wiadomości firmy Apple. Aby dowiedzieć się więcej, zobacz firmy Apple [zasobów wiadomości](https://newsresources.apple.com/) dokumentacji.

## <a name="providing-haptic-feedback"></a>Przekazywanie opinii dotykowych

Na telefonie iPhone 7 i iPhone 7 Plus, Apple dołączył nowe haptics odpowiedzi, które zapewniają dodatkowe sposoby fizycznie udziału użytkownika. Nowe opcje opinii dotykiem umożliwia pobrać uwagi użytkownika i wzmocnienia ich działania.

Kilka wbudowanych elementów interfejsu użytkownika już reagowanie dotykowych takich jak selektorami, przełączniki i suwaki. iOS 10 teraz dodaje możliwość programowo wyzwolenia haptics przy użyciu konkretnych podklasą klasy `UIFeedbackGenerator` klasy.

Aby dowiedzieć się więcej, zobacz nasze [dostarczanie dotykowych opinii](~/ios/user-interface/ios-ui/haptic-feedback.md) przewodnik.

## <a name="proactive-suggestions"></a>Sugestie aktywne

iOS 10 przedstawia nowe sposoby pobudzenie engagement dla aplikacji dzięki systemowi aktywnego wyświetlane przydatne informacje automatycznie dla użytkownika w odpowiednim czasie. Tak jak w przypadku systemu iOS 9 podać możliwość dodawania wyszukiwania bezpośrednich do aplikacji przy użyciu Spotlight, przekazaniem i sugestie Siri z systemem iOS 10 aplikacji mogą uwidaczniać funkcje, które są widoczne dla użytkownika przez system z w następujących lokalizacjach:

- Przełącznik aplikacji
- Ekran blokady
- CarPlay
- Mapy
- Używanie programu Siri interakcji
- Sugestie QuickType 

Aplikacja udostępnia tę funkcję do systemu przy użyciu kolekcji technologii, takich jak [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), znaczników sieci web, Core Spotlight, MapKit, Media Player i UIKit.

Aby dowiedzieć się więcej, zobacz nasze [wprowadzenie do aktywnego sugestie](~/ios/platform/search/proactive-suggestions.md) przewodnik.

## <a name="request-app-review"></a>Przegląd żądania aplikacji

Jesteś nowym użytkownikiem iOS 10.3 `RequestReview()` metody umożliwia aplikacji systemu iOS poprosić użytkownika o szybkości lub zapoznaj się z nim. Tę metodę można wywołać w dowolnym momencie, w których warto środowisko użytkownika, proces przeglądu jest postanowieniom i obsługiwane zasadom sklepu. W związku z tym ta metoda może lub nie może wyświetlić alert i nigdy nie powinna być wywoływana w odpowiedzi na akcję użytkownika, takich jak naciśnięcie przycisku.

Aby dowiedzieć się więcej, zobacz nasze [żądania przeglądu aplikacji](~/ios/platform/request-app-review.md) przewodnik.

## <a name="security-and-privacy-enhancements"></a>Bezpieczeństwo i prywatność ulepszenia

Apple wprowadziła kilka ulepszeń zarówno zabezpieczeń i prywatności w systemie iOS 10 pomoże developer poprawy zabezpieczeń aplikacji i zapewnienia zachowania poufności użytkownika końcowego.

W związku z tym aplikacje działające w systemie iOS 10 (lub późniejszy) statycznie musi deklarować ich próba dostępu do określonych funkcji lub informacje użytkownika, wprowadzając jeden lub więcej określonych kluczy prywatności w ich `Info.plist` pliki, które wyjaśnić użytkownikowi przyczynę aplikacja chce uzyskać dostęp.

Aby dowiedzieć się więcej, zobacz nasze [zabezpieczeń i prywatności rozszerzenia](~/ios/app-fundamentals/security-privacy.md) przewodnik.

## <a name="sirikit"></a>SiriKit

Nowość w systemach iOS 10, SiriKit umożliwia aplikacji platformy Xamarin.iOS do świadczenia usług, które są dostępne dla użytkownika za pomocą Siri na urządzeniu z systemem iOS. Ta funkcja znajduje się w co najmniej jednego rozszerzenia aplikacji przy użyciu nowego **intencje** i **intencje interfejsu użytkownika** struktury.

SiriKit obsługuje następujące domeny usługi:

- Audio i wideo telefonicznej.
- Rezerwacji jazdy.
- Zarządzanie ćwiczeń.
- Do obsługi komunikatów.
- Wyszukiwanie zdjęcia.
- Wysyłanie lub odbieranie płatności.

Gdy użytkownik wysyła żądanie Siri odwołania rozszerzenia aplikacji usług, SiriKit wysyła rozszerzenia **zamiar** obiektu, który opisuje żądanie użytkownika wraz z danymi pomocniczych. Rozszerzenie aplikacji następnie generuje odpowiednie **odpowiedzi** obiektu dla danego **zamiar**, opisujące, jak rozszerzenie może obsłużyć żądania.

Podczas gdy Siri zazwyczaj obsługuje wszystkie interakcji z użytkownikiem, można użyć rozszerzenia aplikacji **interfejsu użytkownika zamiar** framework do prezentowania rozbudowane, niestandardowego interfejsu użytkownika wyposażony w aplikacji do znakowania i dodatkowe informacje.

Aby dowiedzieć się więcej, zobacz nasze [wprowadzenie do SiriKit](~/ios/platform/sirikit/index.md) przewodnik.

## <a name="speech-recognition"></a>Rozpoznawanie mowy

iOS 10 obejmuje nowy interfejs API mowy, który umożliwia aplikacji do obsługi rozpoznawania mowy i wykonać transkrypcji mowy (strumienie na żywo lub nagrania audio) na tekst.

Ponieważ rozpoznawanie mowy wymaga transmisji i tymczasowego przechowywania danych na serwerach firmy Apple, aplikacja _musi_ zażądać uprawnień do wykonania rozpoznawania, umieszczając w niej `NSSpeechRecognitionUsageDescription` klucza w jego `Info.plist` plik i wywoływania `SFSpeechRecognizer.RequestAutorization` metody.

Aby dowiedzieć się więcej, zobacz nasze [wprowadzenie do rozpoznawania mowy](~/ios/platform/speech.md) przewodnik.

## <a name="user-notifications"></a>Powiadomienia użytkowników

Jesteś nowym użytkownikiem iOS 10, powiadomienie użytkownika pozwala dostarczania i obsługi powiadomień lokalnych i zdalnych. Przy użyciu tego framework, aplikacji lub rozszerzenia aplikacji można zaplanować dostarczania lokalnego powiadomienia, określając zestaw warunków, takich jak lokalizacji lub godzinę.

Ponadto, aplikacji lub rozszerzenia można odbierać (i potencjalnie modyfikować) powiadomień lokalnych i zdalnych jako zostaną dostarczone do urządzeń z systemem iOS przez użytkownika.

Nowa struktura interfejsu użytkownika powiadomienie użytkownika umożliwia aplikacji lub rozszerzenia aplikacji do dostosowywania wyglądu powiadomień lokalnych i zdalnych, kiedy mają być przedstawiane dla użytkownika.

Aby dowiedzieć się więcej, zobacz nasze [Framework powiadomienia użytkownika](~/ios/platform/user-notifications/index.md) przewodnik.

## <a name="video-subscriber-account"></a>Konto subskrybenta wideo

Nowe dla systemu iOS 10, w ramach konta subskrybenta wideo umożliwia aplikacjom tej pomocy technicznej uwierzytelniony przesyłania strumieniowego lub wideo na żądanie do uwierzytelniania za pomocą kabla lub satelitarnej dostawcy TV przy użyciu pojedynczej-logowania dla użytkownika końcowego.

## <a name="wide-color"></a>Kolor międzynarodowe

iOS 10 rozszerza do obsługi formatów pikseli rozszerzony zakres i całej gamy przestrzenie w całym systemie, w tym platform, takich jak Core grafiki, Core obrazu systemu operacyjnego i AVFoundation. Obsługa urządzeń z przedstawia szeroką kolor dalsze jest ułatwiony dostarczając to zachowanie przez cały stos całej grafiki.

Ponadto [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) został zmodyfikowany do pracy w nowym rozszerzony **sRGB** kolorów, ułatwiając mieszać kolorów w one zakresami szeroki kolor bez utraty znaczących wydajności.

Apple oferuje następujące najlepsze rozwiązania podczas pracy z szeroką kolorów:

- [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/) teraz używa sRGB przestrzeń kolorów i nie będą clamp wartości `0.0` do `1.0` zakresu. Jeśli aplikacja opiera się na poprzednie ograniczenie zachowanie, trzeba będzie można zmodyfikować dla systemu iOS 10.
- Podczas wykonywania niestandardowej środowiska rysowania zostanie skonfigurowany dla przestrzeń kolorów sRGB `UIView` rysowania na urządzeniu iPad Pro.
- Jeśli aplikacja przeprowadza niestandardowe renderowanie `UIImages`, używając nowego [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) klasę, aby określić używanie formatów rozszerzony zakres lub standard-range.
- Podaj przetwarzania obrazów za pomocą API niskiego poziomu, takich jak grafiki rdzeni lub systemu operacyjnego, deweloper należy używać rozszerzonej zakresu kolorów miejsca i piksela formacie, który obsługuje wartości zmiennoprzecinkowych 16-bitowych. W przypadku, gdy to konieczne, deweloper będzie musiał ręcznie clamp wartości składnika kolorów.
- Podstawowe grafiki, Core obrazu i oferować nowych metod konwersji między dwoma przestrzenie systemu operacyjnego wydajności programów do cieniowania.

Aby dowiedzieć się więcej, zobacz nasze [wprowadzenie do szerokości kolor](~/ios/platform/wide-color.md) przewodnik.

## <a name="widget-enhancements"></a>Ulepszenia widżetu

Apple wprowadziła kilka ulepszeń systemu Widget, aby upewnić się, że elementy widget wygląda świetnie na tle, wszelkie istniejące na nowe iOS 10 ekranu blokady. [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) właściwości jest już przestarzałe i zostało zastąpione przez nowe [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) lub [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect) właściwości. Ponadto elementy widget zawierają teraz [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) właściwość, która umożliwia deweloperowi opisano, jak dużo zawartości jest dostępny i umożliwia użytkownikowi rozwijanie i zwijanie zawartości.

Aby dowiedzieć się więcej, zobacz nasze [wyszukiwania i Home rozszerzenia elementu Widget ekranu](~/ios/platform/search/widgets.md) przewodnik.

## <a name="additional-framework-changes"></a>Zmiany dodatkowe Framework

Oprócz framework najważniejszych zmian i dodatków wymienionych powyżej Apple wprowadził wiele dodatkowych framework drobne zmiany w systemie iOS 10.

Aby dowiedzieć się więcej, zobacz nasze [dodatkowych zmian Framework](~/ios/platform/introduction-to-ios10/additional-framework-changes.md) przewodnik.

## <a name="deprecated-apis"></a>Przestarzałe interfejsy API

Następujące interfejsy API są przestarzałe w systemie iOS 10:

- `CKDiscoverAllContactsOperation`, `CKDiscoveredUserInfo`, `CKDiscoverUserInfosOperation` i `CKFetchRecordChangesOperation` klasy są przestarzałe w CloudKit dla systemu iOS 10. Użyj [CKDiscoverAllUserIdentitiesOperation](https://developer.xamarin.com/api/type/CloudKit.CKDiscoverUserIdentitiesOperation/), [CKUserIdentity](https://developer.xamarin.com/api/type/CloudKit.CKUserIdentity/) i [CKFetchRecordZoneChangesOperation](https://developer.xamarin.com/api/type/CloudKit.CKFetchRecordZoneChangesOperation/) klasy (które obsługuje udostępnianie rekordów) zamiast tego.
- Kilka [CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) interfejsów API (np. na podstawie strefy i oparte na zapytaniach subskrypcje) są przestarzałe. Użyj [CKRecordZoneSubscription](https://developer.xamarin.com/api/type/CloudKit.CKRecordZoneSubscription/) i [CKQuerySubscription](https://developer.xamarin.com/api/type/CloudKit.CKQuerySubscription/) interfejsów API zamiast tego.
- [NSPersistentStoreCoordnator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) symbole związane z uniwersalnych zawartości są przestarzałe.
- `ADBannerView`, `ADInterstitialAd` i powiązanych symbole w [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) klasy są przestarzałe.
- [SKUniform](https://developer.apple.com/reference/spritekit/skuniform) symbole powiązane do wartości zmiennoprzecinkowych zostały wycofane.
- `UILocalNotification`, `UIMutableUserNotificationAction`, `UIMutableUserNotificationCategory`, `UIUserNotificationAction`, `UIUserNotificationCategory` i `UIUserNotificationSettings` klasy UIKit są przestarzałe. Użyj [powiadomienia użytkownika](#User-Notifications) framework zamiast tego.
- `HandleActionForLocalNotification`, `HandleActionForRemoteNotification`, `DidReceiveLocalNotification` i `DidReceiveRemoteNotification` WatchKit metody są przestarzałe. Użyj `HandleActionForNotification` i `DidReceiveNotification` metody zamiast tego.
- `DidReceiveLocalNotification` i `DidReceiveRemoteNotification` metody [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) są przestarzałe. Utwórz wystąpienie [UNUserNotificationCenterDelegate](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate) czy implementuje odpowiednich metod i przypisz go do `Delegate` właściwość [UNUserNotificationCenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter) obiektu.
- **Aplikacji Centrum gier** przestarzałe i usunięty z systemu iOS. Jeśli aplikacja korzysta z GameKit, jego _musi_ przedstawić własny interfejs wyświetlanie GameKit funkcji, np. tablice wyników.

Zobacz firmy Apple [systemu iOS 9.3 różnic interfejsu API dla systemu iOS 10.0](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html) dokumentację, aby uzyskać pełną listę deprecations.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Nowości w systemie iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
