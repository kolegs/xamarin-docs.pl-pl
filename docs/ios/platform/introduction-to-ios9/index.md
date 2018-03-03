---
title: Wprowadzenie do systemu iOS 9
description: "W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w systemie iOS 9 dla deweloperów platformy Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 5b26989603695cfb309fba5a5318f7ef4d2460e2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-9"></a>Wprowadzenie do systemu iOS 9

_W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w systemie iOS 9 dla deweloperów platformy Xamarin.iOS._

![](images/ios9-sml.png "Logo systemu iOS 9")

Apple został dodany kilka nowych interfejsów API i usług z systemem iOS 9 wraz z wielu ulepszenia istniejących funkcji.

## <a name="3d-touch"></a>3D Touch

Nowy system iOS 9 i telefonów iPhone 6s i telefonów iPhone 6s Plus, 3D Touch dodaje gestów poufnych wykorzystania aplikacji systemu iOS. 3W Touch iPhone aplikacja jest teraz można nie tylko stwierdzić, że użytkownik jest dotknięcie ekranu urządzenia go można znaczeniu nacisku wywiera użytkownika i odpowiadać na wykorzystanie różnych poziomów.

3D Touch zapewnia następujące funkcje do aplikacji:

- **Wykorzystania czułości** — aplikacje można teraz miar jak twarde lub światła użytkownika zachodzi ekranu i podejmij korzystać z tych informacji. Na przykład aplikacja rysowania wprowadzić wierszu grubszy lub cieńsza oparte na jaką użytkownik zachodzi ekranu.
- **Podgląd i wyświetl** -aplikację teraz można pozwolić użytkownikowi na interakcję z jego dane bez konieczności Przejdź poza ich bieżący kontekst. Naciskając twardych na ekranie, można ich *wgląd* są zainteresowani (takich jak Podgląd wiadomości) dla tego elementu. Naciskając trudniejsze, mogą one *Pop* do elementu.
- **Szybkie akcje** — Pomyśl o szybkie akcji, takich jak menu kontekstowe, które mogą zostać zdjęte ze stosu w górę po kliknięciu elementu w aplikacji komputerowej. Przy użyciu szybkie akcje, można dodać wspólne, szybkie i łatwe do skrótów dostęp do funkcji w aplikacji z głównej ikony ekranu na urządzeniu z systemem iOS.

Aby dowiedzieć się więcej, zobacz nasze [wprowadzenie do technologii 3D Touch](~/ios/platform/3d-touch.md) przewodnik.

## <a name="app-transport-security"></a>Zabezpieczenia transportu aplikacji

Nowość w systemach iOS 9, zabezpieczeń transportu aplikacji (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji. ATS zapewnia cała komunikacja z Internetem odpowiadają bezpieczne połączenie najlepszych rozwiązań, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych bezpośrednio przy użyciu aplikacji lub biblioteki, która zajmuje go.

Ponieważ ATS jest domyślnie włączona w aplikacji skompilowany dla systemu iOS 9 i OS X 10.11 (El Capitan), wszystkie połączenia przy użyciu [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) lub [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) będzie pracował Wymagania dotyczące zabezpieczeń ATS. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.

Aby dowiedzieć się więcej na temat ATS, zobacz nasze [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md) przewodnik.

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>Wielozadaniowości dla urządzeń iPad

Z systemem iOS 9 Apple została dodana obsługa wielozadaniowości sprzęt określonego urządzenia iPad z systemem dwie aplikacje w tym samym czasie. W związku z tym aplikacji platformy Xamarin.iOS już można założyć, że są one tylko aplikacja uruchomiona w danym momencie lub których mają dostęp do pełnego ekranu lub zasobów urządzenia.

Wielozadaniowości dla tabletu iPad jest obsługiwany przez następujące funkcje:

- **Slajd za pośrednictwem** — zezwala użytkownikowi na tymczasowo uruchomić drugi aplikacji dla systemu iOS na slajdzie limit panelu (albo po prawej lub lewej stronie ekranu, w zależności od kierunku języka), który obejmuje około 25% głównej aplikacji aktualnie uruchomione. Slajd za pośrednictwem jest dostępna tylko na iPad Pro, iPad lotniczego, iPad 2 lotniczego, iPad Mini 2, iPad Mini 3 lub 4 dla urządzenia iPad.
- **Widok podzielony** — na urządzeniu iPad obsługiwane (iPad lotniczego 2, 4 dla urządzeń iPad i Pro tylko iPad), użytkownik może Wybierz drugi aplikacji i uruchom go side-by-side z aktualnie uruchomionej aplikacji w trybie ekranu podziału. Użytkownik może określić procent ekranu głównego, który zajmuje każdej aplikacji.
- **Obraz na rysunku** — dla aplikacji, które odtwarzania zawartości wideo, filmów może teraz zostać odtworzone w oknie ruchome i o zmiennych rozmiarach, które pojawia się na inne aplikacje uruchomione na urządzeniu z systemem iOS. Użytkownik ma pełną kontrolę nad rozmiar i położenie tego okna. Obraz na rysunku jest dostępna tylko na iPad Pro, iPad lotniczego, iPad 2 lotniczego, iPad Mini 2, iPad Mini 3 lub 4 dla urządzenia iPad.

Aby dowiedzieć się więcej na temat nowych możliwości wielozadaniowości systemu IOS 9, zobacz nasze [wielozadaniowości dla tabletu iPad](~/ios/platform/multitasking.md) przewodnik.

## <a name="new-contacts-and-contacts-ui-frameworks"></a>Nowe kontakty i kontakty struktur interfejsu użytkownika

Wraz z wprowadzeniem systemu iOS 9, Apple wydała dwóch nowych struktur, [kontaktów](https://developer.xamarin.com/api/namespace/Contacts/) i [ContactsUI](https://developer.xamarin.com/api/namespace/ContactsUI/), który zastąpić istniejące książki adresowej i interfejsu użytkownika książki adresowej struktury używane przez system iOS 8 i przed.

Te nowe, zorientowany obiektowo struktur omówienia następujących zagadnień:

- **Kontakty** — Xamarin.iOS zapewnia dostęp do informacji kontaktowych dla użytkownika. Ponieważ większość aplikacji wymaga tylko dostęp tylko do odczytu, platforma został zoptymalizowany dla wątku dostępu bezpiecznej, tylko do odczytu.
- **ContactsUI** — udostępnia interfejs użytkownika platformy Xamarin.iOS elementów do wyświetlenia, edytować, wybierz i tworzenie kontaktów na urządzeniach z systemem iOS.

Aby uzyskać więcej informacji, zobacz nasze [kontakty i interfejsu użytkownika kontaktów](~/ios/platform/contacts.md) dokumentacji.


## <a name="new-search-apis"></a>Nowe interfejsy API wyszukiwania

Wyszukiwanie została rozszerzona w systemie iOS 9, aby zapewnić doskonałe nowe sposoby na dostęp do informacji w aplikacji platformy Xamarin.iOS. Przy użyciu nowych interfejsów API wyszukiwania, możesz wprowadzić zawartości aplikacji za pośrednictwem Spotlight oraz Safari wyniki wyszukiwania, przekazaniem i używanie programu Siri przypomnienia i sugestie dotyczące wyszukiwania. Dzięki temu użytkownicy szybki dostęp do działań i informacji głęboko w aplikacji.

Ponadto nowe interfejsy API wyszukiwania ułatwiają integrowanie wyszukiwania w aplikacji bez obsługi wdrożenia poprzedniego wyszukiwania. W związku z tym Apple oświadczeń. zazwyczaj zajmuje kilka godzin, aby aplikacja systemu iOS 9 zawartość ogólnie można wyszukiwać w wyszukiwaniu aplikacji.

Aby uzyskać więcej informacji, zobacz nasze [ulepszenia wyszukiwania](~/ios/platform/search/index.md) dokumentacji.

## <a name="new-stack-view"></a>Nowy widok stosu

Formant widoku stosu ([UIStackView](https://developer.xamarin.com/api/type/UIKit.UIStackView/)) korzysta z możliwości automatycznego układu oraz klasy wielkości Zarządzanie stos widoków podrzędnych (poziomej lub pionowej) odpowiadający dynamicznie na rozmiar orientację i ekranu urządzenia z systemem iOS.

Za pomocą formantu widoku stosu, ilość pracy wymagane do układu interfejs użytkownika jest znacznie ograniczone. Układ wszystkich widoków podrzędnych dołączone do widoku stosu są zarządzane automatycznie w oparciu o developer zdefiniowane właściwości, takie jak osi, dystrybucji i wyrównanie odstępów.

Aby uzyskać więcej informacji, zobacz nasze [wprowadzenie do widoku stosu](~/ios/user-interface/controls/uistackview.md) dokumentacji.


## <a name="collection-view-changes"></a>Przeglądanie zmian w kolekcji

W systemie iOS 9, widok kolekcji ([UICollectionView](https://developer.xamarin.com/api/type/UIKit.UICollectionView/)) teraz obsługuje przeciągnij zmiany kolejności elementów poza pole przez dodanie nowych aparat rozpoznawania gestów domyślne i kilka nowych metod pomocniczych.

Przy użyciu tych nowych metod, można łatwo zaimplementować przeciągania do zmiany kolejności w widoku kolekcji i dostępna jest opcja Dostosowywanie wyglądu elementy w każdym etapie procesu opcję.

Aby dowiedzieć się więcej o zmianach widok kolekcji dla systemu iOS 9, zobacz nasze [przeglądanie zmian w kolekcji](~/ios/user-interface/controls/uicollectionview.md) przewodnik.

## <a name="game-enhancements"></a>Ulepszenia gier

Z systemem iOS 9 Apple wprowadziła kilka ulepszeń technologicznych interfejsów API gier, które ułatwiają wdrażanie grafikę gier i audio w aplikacji platformy Xamarin.iOS. Dotyczy to zarówno łatwość programowania za pośrednictwem struktury wysokiego poziomu i wykorzystanie mocy procesora GPU urządzenie iOS ulepszone szybkości i możliwości graficzne ulepszeń niskiego poziomu.

W tym GameplayKit, ReplayKit, Model we/wy, MetalKit i programów do cieniowania wydajności systemu operacyjnego oraz nowego, ulepszone funkcje systemu operacyjnego, SceneKit i SpriteKit.

Aby uzyskać więcej informacji, zobacz nasze [ulepszenia gry](~/ios/platform/gaming/index.md) dokumentacji.

## <a name="homekit-framework-changes"></a>HomeKit Framework zmiany

[HomeKit](https://developer.xamarin.com/api/namespace/HomeKit/) framework, wprowadzone w systemie iOS 8, umożliwia konfigurowanie i kontrolować różne akcesoria HomeKit włączone (takie jak świateł zautomatyzowane blokady drzwi i obiekty otwierające drzwi garaż) z aplikacji platformy Xamarin.iOS. Oprócz łatwe do instalacji i konfiguracji, Akcesoria HomeKit można kontrolować za pomocą rozmowy polecenia Siri.

Apple iOS 9, ma łatwiejsza Instalatora, rozwinięty typów Akcesoria obsługiwane i podać więcej interakcje akcesoriów (takie jak kontrolowanie akcesoriów zdalnie przy użyciu usługi iCloud).

Aby uzyskać więcej informacji, zobacz nasze [wprowadzenie do HomeKit](~/ios/platform/homekit.md), [HomeKitIntro iOS Przykładowa aplikacja](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/) i firmy Apple [HomeKit](https://developer.apple.com/homekit/) dokumentacji.

## <a name="handoff-framework-changes"></a>Programowi handoff Framework zmiany

Programowi handoff (znanej także jako ciągłości) została wprowadzona przez firmy Apple w systemie iOS 8 i OS X Yosemite (10.10) jako sposób dla użytkownika uruchomić działanie na jednym urządzeniu (z systemem iOS lub Mac) i kontynuować tego samego działania na innym urządzeniu (określone przez użytkownika iClou d konta).

Programowi handoff została rozszerzona w systemie iOS 9 w celu obsługi nowych, udoskonalone funkcje wyszukiwania. Aby uzyskać więcej informacji, zobacz nasze [ulepszenia wyszukiwania](~/ios/platform/search/index.md) dokumentacji. Aby uzyskać więcej informacji na temat używania przekazaniem, zobacz nasze [wprowadzenie do przekazaniem](~/ios/platform/handoff.md) dokumentacji.

## <a name="new-extension-points"></a>Nowe punkty rozszerzenia

W systemie iOS 8, Firma Apple wprowadziła rozszerzenia — bibliotek, które są przedstawiane przez system operacyjny w kontekstach standardowe, takich jak w Centrum powiadomień, gdy użytkownik zażąda klawiatura lub podczas edycji zdjęcie.

Z systemem iOS 9, Apple stanowi rozszerzenie obsługi rozszerzenia podając kilka nowych _punktów rozszerzenia_ Definiowanie zasad użytkowania i zapewnić interfejsów API do pracy w danym obszarze w następujący sposób:

- **Nowy punkt rozszerzenia jednostki Audio** — Użyj tego punktu rozszerzenia, aby podać efekty audio, instrumentów muzycznych, generatory dźwięku, itp. do użytku w innych aplikacjach hosta Audio jednostki (na przykład GarageBand). Ten punkt rozszerzenia umożliwia również sprzedaży _Audio jednostki_ (audio wtyczki) ze sklepu App Store.
- **Nowy punkt rozszerzenia konserwacji indeksu** — korzystanie z tego punktu rozszerzenia obsługi indeksowanie danych aplikacji bez konieczności wznowienia aplikacji.
- **Nowe punkty rozszerzenia sieci** (wymagają specjalnych uprawnień od firmy Apple):
    - **Rozszerzenie dostawcy serwera Proxy aplikacji** — korzystanie z tego punktu rozszerzenia do zaimplementowania niestandardowego przezroczysty sieci po stronie klienta serwera proxy.
    - **Filtrowanie danych dostawcy / rozszerzenia dostawcy formant filtru** — korzystać z tych punktów rozszerzenia do zaimplementowania zawartość dynamiczna sieci filtrowania na urządzeniu.
    - **Rozszerzenie dostawcy tunelu pakietów** — korzystanie z tego punktu rozszerzenia do zaimplementowania niestandardowego tunneling protocol po stronie klienta sieci VPN.
- **Nowe punkty rozszerzenia Safari**:
    - **Blokowanie rozszerzenia zawartości** — użycie tego punktu rozszerzenia, aby zdefiniować listę zablokowanych zawartości, które nie będą wyświetlane, gdy użytkownik jest przeglądanie sieci web.
    - **Udostępnione linki rozszerzenia** — użycie tego punktu rozszerzenia, aby umożliwić wyświetlanie zawartości aplikacji w łącza udostępnione przez przeglądarkę Safari.

Aby uzyskać więcej informacji, zobacz nasze [wprowadzenie do rozszerzeń](~/ios/platform/extensions.md) i firmy Apple [przewodnik programowania w języku rozszerzenie aplikacji](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) dokumentacji.

## <a name="keychain-enhancements"></a>Ulepszenia łańcucha kluczy

W systemie iOS 9 Apple ma rozszerzone łańcucha kluczy w celu zapewnienia nowy typ klucza szyfrowania enklawę zabezpieczenia i więcej opcji ochrony elementu w następujący sposób:

- Nowe ograniczenie funkcji Touch ID, która unieważnia elementy łańcucha kluczy modyfikacji bazy danych linii papilarnych.
- Nowe ograniczenia, które umożliwia tylko tworzenie pozycji listy kontroli dostępu za pomocą funkcji Touch ID i kod dostępu.
- Nowy kontekst uwierzytelniania, która pozwala na wywołanie uwierzytelniania niezależnie od `SecItem` wywołania.
- Dostęp do entropii formantu listy (przy użyciu opcji hasło aplikacji) do szyfrowania elementu łańcucha kluczy dostarczony do aplikacji.
- Obsługa generowania i przy użyciu kluczy w bezpiecznym enklawę (za pośrednictwem `kSecAttrTokenIDSecureEnclave` atrybutu).

Aby uzyskać więcej informacji, zobacz nasze [wprowadzenie do funkcji Touch ID](~/ios/platform/touchid.md) dokumentacji.


## <a name="right-to-left-language-support"></a>Obsługa języków od prawej do lewej

W systemie iOS 9 Apple wprowadził umożliwienie korzystania z interfejsem użytkownika odwrócony łatwiejsze niż kiedykolwiek zapewniając pełną obsługę języków od prawej do lewej. Obejmuje to następujące elementy:

- Standardowe [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) kontrolek będzie automatycznie Przerzuć na podstawie ustawień regionalnych i językowych urządzenia iOS od prawej do lewej.
- [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) zawiera klasy atrybutów, które umożliwiają definiowanie sposobu danego widoku powinna występować, jeśli odwrócony od prawej do lewej.
- Funkcja przerzucania obrazu programowo przy użyciu [FlipsForRightToLeftLayoutDirection](https://developer.xamarin.com/api/property/UIKit.UIImage.FlipsForRightToLeftLayoutDirection/) właściwość [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/) klasy.

Aby uzyskać więcej informacji, zobacz firmy Apple [języków wspierające od prawej do lewej](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) dokumentacji.



## <a name="additional-framework-changes"></a>Zmiany dodatkowe Framework

Oprócz najważniejszych zmian, które firma Microsoft ma wymienionego powyżej Apple wprowadził zmiany i usprawnienia kilka istniejących struktur dla systemu iOS 9, takie jak następujące:

- Struktury AV Foundation
- AVKit Framework
- CloudKit Framework
- Struktury Foundation
- Handoff Framework
- HealthKit Framework
- HomeKit Framework
- Framework uwierzytelniania lokalnych
- MapKit Framework
- PassKit Framework
- Safari usługi Framework
- UIKit Framework

Aby uzyskać więcej informacji, zobacz nasze [zmiany Framework dodatkowe systemu iOS 9](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) dokumentacji.

## <a name="deprecated-apis-and-functions"></a>Przestarzałe interfejsy API i funkcje

Apple jest przestarzała, następujące funkcje w systemie iOS 9 i interfejsów API:

- **Adres książki & interfejsu użytkownika książki adresowej** -API te zostały zastąpione przez struktury kontaktów i skontaktuj się z interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz nasze [kontakty i interfejsu użytkownika kontaktów](~/ios/platform/contacts.md) dokumentacji.
- **CBCentralManager** - `RetrievePeripherals` i `RetrieveConnectedPeripherals` metody `CBCentralManager` klasy zostały usunięte z systemu iOS 9. Wywołanie tych metod spowoduje aplikacji do awarii podczas parowania akcesoriów lub uruchamiania aplikacji.
- **FetchAllChanges** - `FetchAllChanges` z `CKFetchRecordChangesOperation` klasy został amortyzacji i zostanie usunięte w systemie iOS 9.
- **Odtwarzacz** -Media Player framework jest przestarzała w systemie iOS 9. Zamiast tego użyj AVKit lub interfejsów API Foundation AV.

Aby uzyskać pełną listę określonych deprecations interfejsu API, zobacz firmy Apple [iOS 9.0 Diffs interfejsu API](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) dokumentacji.

## <a name="ios-9-sample-apps"></a>System iOS 9 przykładowe aplikacje

Niektóre mamy [przykłady 9 specyficzne dla systemu iOS](https://developer.xamarin.com/samples/ios/iOS9/) rozpoczęcie pracy:

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://developer.xamarin.com/samples/monotouch/ios9/MetalPerformanceShadersHelloWorld/)
- [MusicMotion](https://developer.xamarin.com/samples/monotouch/ios9/MusicMotion/)
- [PhotoProgress](https://developer.xamarin.com/samples/monotouch/ios9/PhotoProgress/)
- [SegueCatalog](https://developer.xamarin.com/samples/monotouch/ios9/SegueCatalog/)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

Sprawdź również iOS części te przykłady (dostępne wersje systemu Mac OS X pomocnika!):

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Wprowadzenie do technologii 3D Touch](~/ios/platform/3d-touch.md)
- [Zabezpieczenia transportu aplikacji](~/ios/app-fundamentals/ats.md)
- [Wielozadaniowość dla iPadów](~/ios/platform/multitasking.md)
- [Kontakty i kontakty interfejsu użytkownika](~/ios/platform/contacts.md)
- [Nowe interfejsy API wyszukiwania](~/ios/platform/search/index.md)
- [Wprowadzenie do stosu widoku](~/ios/user-interface/controls/uistackview.md)
- [Przeglądanie zmian w kolekcji](~/ios/user-interface/controls/uicollectionview.md)
- [Ulepszenia gier](~/ios/platform/gaming/index.md)
- [Wprowadzenie do HomeKit](~/ios/platform/homekit.md)
- [Wprowadzenie do przekazaniem](~/ios/platform/handoff.md)
- [Dodatkowe zmiany struktury systemu iOS 9](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [Rozwiązywanie problemów](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [Nowości w systemie iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Aktualizowanie aplikacji platformy Xamarin.iOS do iOS9 (klip wideo)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
