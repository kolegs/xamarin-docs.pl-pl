---
title: Wprowadzenie do systemu iOS 8
description: Z systemem iOS 8 Apple udostępnił nadmiar nowe struktury i interfejsy API do wzbudzania i doskonale dopasowanych deweloperów. W tym przewodniku możemy wprowadzić te nowe interfejsy API i zobacz korzyści z systemem iOS 8 zarówno deweloperzy i użytkownicy.
ms.prod: xamarin
ms.assetid: 33AD66C0-3743-49FE-9DCE-88ED3A16BA63
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 2f57547356adcbafd01851bc54e42a14454ccd6a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30781119"
---
# <a name="introduction-to-ios-8"></a>Wprowadzenie do systemu iOS 8

_Z systemem iOS 8 Apple udostępnił nadmiar nowe struktury i interfejsy API do wzbudzania i doskonale dopasowanych deweloperów. W tym przewodniku możemy wprowadzić te nowe interfejsy API i zobacz korzyści z systemem iOS 8 zarówno deweloperzy i użytkownicy._

System iOS 7 wizualnie zmienić interfejsu użytkownika dla całego systemu iOS z deweloperzy i użytkowników ma pochodzić oczekiwać z pierwszego iPhone systemu operacyjnego. System IOS 8 kontynuuje to zapewniając wiele platform dla deweloperów, dzięki czemu użytkownicy mogą kontrolować niemal każdego aspektu ich życia prostej z ich iPhone. Na przykład kondycji i przydatności może być analizowane za pomocą *HealthKit*, kody dostępu są przestarzała z uwierzytelniania biometrycznego przy użyciu *LocalAuthentication*, *rozszerzeń aplikacji*Otwórz kanał komunikacji między aplikacjami 3 stron, i *HomeKit* możliwość przekształcenie domu w głównej w przyszłości. 

Jeśli o delighting użytkownicy systemu iOS 7, system iOS 8 koncentruje się na delighting deweloperom całego zakresu tych tasty nowych narzędzi. 

W tym przewodniku przedstawiono nowe interfejsy API dla deweloperów platformy Xamarin.iOS.  

Istnieje kilka interfejsów API, które są przestarzałe w systemie iOS 8, które opisano szczegółowo na końcu tego dokumentu.

## <a name="requirements"></a>Wymagania

Poniższe elementy są wymagane do tworzenia aplikacji dla systemu iOS 8 w programie Visual Studio dla komputerów Mac:

- **Xcode 7 i iOS 8 lub nowszego** — najnowsze Xcode i interfejsów API systemu iOS firmy Apple muszą być zainstalowana i skonfigurowana na komputerze dewelopera.
- **Visual Studio for Mac** — najnowszej wersji programu Visual Studio for Mac powinna być zainstalowana i skonfigurowana na urządzeniu użytkownika.
- **System iOS 8 urządzenie lub symulator** — urządzenia z systemem iOS z najnowszą wersją systemu IOS 8 do testowania.

## <a name="home-and-leisure"></a>Domowych i

System iOS 8 pomogła mocno roślin bezpośrednio do serca domu HomeKit i HealthKit firmy Apple i urządzeń z systemem iOS. W tej sekcji przedstawiono, jak obu tych nowych struktur pracy i jak mogą zostać włączone do aplikacji platformy Xamarin.iOS.

## <a name="homekit"></a>HomeKit

Kontrolowanie urządzeń z urządzenia iPhone nie jest aplikacją nowych technologii; wiele produktów połączone głównej można sterować za pośrednictwem aplikacji systemu iOS. Jednak HomeKit teraz trwa to kolejny krok podwyższania poziomu wspólnego protokołu dla urządzeń z macierzystego automatyzacji oraz udostępniania niektórych producentów, takie jak iHome, Philips i Honeywell publiczny interfejs API. Dla użytkownika, oznacza to, że kontrolować niemal każdego aspektu ich głównej bezproblemowo z wewnątrz jednej aplikacji. Jest istotny dla nich wiedzieć, że używają żarówka Philips Hue lub alarm zagnieżdżania. Użytkownicy mogą również łańcucha inteligentne wiele procesów macierzystego w "Sceny".

Z HomeKit aplikacje innych producentów i używanie programu Siri może odnajdywać Akcesoria i dodaj je do ich bazy danych osobowych konfiguracji macierzystego, edytować i operować na danych, a komunikować się z Akcesoria i ich usługi do wykonywania akcji.

### <a name="configuration"></a>Konfiguracja

Na poniższym diagramie przedstawiono podstawowe hierarchii konfiguracji Akcesoria HomeKit:

![](introduction-to-ios8-images/image1.png "Ten diagram przedstawia podstawowe hierarchii konfiguracji Akcesoria HomeKit")
 
Aby rozpocząć pracę z HomeKit, deweloperzy muszą upewnij się, że ich profilu inicjowania obsługi administracyjnej ma wybranej usługi HomeKit. Apple oferuje także deweloperom dodatku HomeKit symulatora dodatku programu dla Xcode. Znajdują się w [Centrum deweloperów firmy Apple](https://developer.apple.com/downloads/index.action)w obszarze `Hardware IO Tools for Xcode`. 

Aby uzyskać więcej informacji, zobacz nasze [HomeKit](~/ios/platform/homekit.md) przewodnik.

## <a name="healthkit"></a>HealthKit

HealthKit to platforma wprowadzona w systemie iOS 8, która zapewnia scentralizowane, skoordynowanej i bezpieczny magazyn danych informacji dotyczących kondycji. System operacyjny zapewnia prywatności i bezpieczeństwa informacji o kondycji oraz za pomocą aplikacji kondycji pulpitu nawigacyjnego dla użytkownika. Za zgodą użytkownika aplikacje można odczytu i zapisu szerokiej gamy informacji o kondycji.

Aby uzyskać więcej informacji na temat używania tego w aplikacji platformy Xamarin.iOS dotyczą [wprowadzenie do HealthKit](~/ios/platform/healthkit.md) przewodnik.

## <a name="extending-iphone-functionality"></a>Rozszerzanie funkcjonalności iPhone
Z iOS8 deweloperzy podano jest znacznie większą kontrolę nad kto może używać ich aplikacji i zwiększenia możliwości bardziej otwarte komunikacji między aplikacje innych producentów. Funkcje, takie jak selektora dokumentu i rozszerzeń aplikacji otwórz world możliwości jak aplikacje mogą być używane w ekosystemie firmy Apple.

### <a name="app-extensions"></a>Rozszerzenia aplikacji
Rozszerzenia aplikacji, aby oversimplify, służą do aplikacji innych firm do komunikowania się ze sobą. Utrzymanie wysokiego poziomu zabezpieczeń, standardów i utrzymanie integralności aplikacji w trybie piaskownicy, ta komunikacja nie jest realizowane bezpośrednio między aplikacjami. Zamiast tego jest przeprowadzony przez *rozszerzenia* w środku.

Pierwszym krokiem tworzenia rozszerzenia aplikacji jest określenie punktu prawidłowym rozszerzeniem — jest to ważne w celu zapewnienia zachowania i dostępności poprawne interfejsów API. Aby utworzyć rozszerzenia aplikacji w programie Visual Studio dla komputerów Mac, należy go dodać do istniejącej aplikacji przez dodanie nowego projektu do rozwiązania.

W **nowy projekt** okno dialogowe, przejdź do **C#** > **iOS** > **Unified API**  >   **Rozszerzenia**, jak pokazano na poniższym zrzucie ekranu:

![](introduction-to-ios8-images/image2.png "Tworzenie nowego rozszerzenia")
 
Okno dialogowe Nowy projekt zapewnia siedem nowych szablonów projektu do tworzenia rozszerzeń aplikacji i omówiono poniżej. Należy zauważyć, że wiele rozszerzeń odnoszą się do innych nowych interfejsów API w systemie iOS, takie jak selektora dokumentu:

- **Akcja** — dzięki temu deweloperzy mogą tworzyć unikatowe akcji niestandardowej przyciski umożliwiające użytkownikom wykonuje pewne zadania
- **Niestandardowe klawiatury** — umożliwia deweloperom dodać do zakresu wbudowane klawiatury firmy Apple przez dodanie ich własnych niestandardowych jeden. Popularne klawiatury Swype używa go do przełączenia ich klawiatury dla systemu iOS.
- **Dokument selektora** — zawiera kontrolera widoku selektora dokumentu, który umożliwia użytkownikom dostęp do plików spoza aplikacji piaskownicy.
- **Dokument selektora plików dostawca** — zapewnia bezpieczne przechowywanie plików za pomocą selektora dokumentu.
- **Edycja fotografii** — ta rozszerza na filtry i narzędzi w aplikacji zdjęć umożliwić użytkownikom większą kontrolę i więcej opcji podczas edytowania swoich zdjęć, już dostarczonymi przez firmę Apple do edycji.
- **Dzisiaj** — daje aplikacji możliwość wyświetlania elementów widget w sekcji dzisiaj Centrum powiadomień.

Aby uzyskać więcej informacji na temat używania rozszerzeń aplikacji w programie Xamarin dotyczą [wprowadzenie do rozszerzeń aplikacji](~/ios/platform/extensions.md) przewodnik.

### <a name="touch-id"></a>Touch ID

Touch ID została wprowadzona w systemie iOS 7 jako metody uwierzytelniania użytkownika — podobnie jak kod dostępu. Jednak została ograniczona do odblokowywania urządzenia, przy użyciu sklepu z aplikacjami za pomocą programu iTunes i iCloud łańcucha kluczy tylko uwierzytelniania 

Istnieją teraz dwa sposoby użycia funkcji Touch ID jako mechanizmu uwierzytelniania w aplikacjach systemu iOS 8 przy użyciu lokalnego interfejsu API uwierzytelniania. Obecnie nie jest możliwe zdalne uwierzytelnianie za pomocą uwierzytelniania lokalnego.

Po pierwsze ułatwia on istniejące usługi łańcucha kluczy za pomocą nowego łańcucha kluczy listy kontroli dostępu (ACL). Dane łańcucha kluczy można odblokować za pomocą pomyślne uwierzytelnienie użytkowników linii papilarnych.

Po drugie LocalAuthentication udostępnia dwie metody uwierzytelniania aplikacji lokalnie. Programiści powinni używać `CanEvaluatePolicy` ustalenie, czy urządzenie jest w stanie akceptowania funkcji Touch ID, a następnie `EvaluatePolicy` rozpocząć operację uwierzytelniania.

Aby uzyskać więcej informacji na funkcję Touch ID i informacje na temat integracji aplikacji platformy Xamarin.iOS dotyczą [wprowadzenie czytnik TouchID](~/ios/platform/touchid.md) przewodników.

### <a name="document-picker"></a>Selektor dokumentu

Ponownie Wycofaj dokumentu selektora współpracuje z dysku iCloud użytkowników umożliwia użytkownikowi otwieranie plików, które zostały utworzone w innej aplikacji, importowanie i manipulowania nimi oraz eksportowanie ich. Spowoduje to utworzenie intuicyjne przepływu pracy i w związku z tym znacznie lepiej, dla użytkowników. Synchronizowanie iCloud przyjmuje jednym kroku dalsze — wszystkie zmiany wprowadzone w jednej aplikacji również będzie można odzwierciedlać spójnie na Twoich urządzeniach.

Aby dowiedzieć się więcej o selektora dokumentu bardziej szczegółowo i Dowiedz się, jak zintegrować ją z aplikacji platformy Xamarin.iOS dotyczą [wprowadzenie do selektora dokumentu](~/ios/platform/document-picker.md) przewodnik.

### <a name="handoff"></a>Handoff

Programowi handoff, będący częścią większej funkcji ciągłości przyjmuje stanowi kolejny krok do integrowania OS X oraz iOS. W tym AirDrop i platform, umożliwia wykonywanie wywołań iPhone, programu SMS na iPad i komputerów Mac oraz ulepszenia w tethering z urządzenia iPhone.

Przekazaniem współpracuje z systemem iOS 8 i Yosemite i wymaga konta iCloud zalogowania się do różnych urządzeń, czy chcesz użyć. Powinien działać z wstępnie zainstalowane aplikacje firmy Apple, w tym Safari, iWork mapy, kalendarzy i kontakty.

Aby uzyskać więcej informacji, zobacz nasze [programowi Handoff](~/ios/platform/handoff.md) przewodnik.

## <a name="unified-storyboards"></a>Ujednolicone Scenorys
System iOS 8 zawiera nowy łatwiej używać mechanizmu służący do tworzenia interfejsu użytkownika — ujednoliconą scenorysu. Pojedynczy scenorysu, obejmujące wszystkie sprzętu różnych rozmiarów ekranu można tworzyć widoki szybkie i elastyczny w stylu "projektowania raz, korzystać z wielu" wartość true.

Przed iOS8, deweloperzy używane `UIInterfaceOrientation` do rozróżniania między trybami pionowej i poziomej i `UIInterfaceIdiom` do rozróżniania między urządzeniami z systemem iOS. W iOS8 nie jest już utworzyć oddzielne scenorys dla urządzenia iPhone i iPad — Orientacja i urządzenia są określane za pomocą *klasy wielkości*.

Każde urządzenie jest zdefiniowane przez klasę rozmiaru w pionie i osi poziomej i istnieją dwa typy klas rozmiar w systemie iOS 8:

- **Regularne** -to jest o rozmiarze większym ekranem (takich jak iPad) lub gadżet, który daje wyobrażenie o dużym rozmiarze (na przykład UIScrollView
- **Compact** -to jest przeznaczona dla mniejszych urządzeń (na przykład iPhone). Ten rozmiar uwzględnia orientacji urządzenia.

Jeśli używane są ze sobą dwa pojęcia, wynikiem jest siatki 2 x 2, który definiuje różne rozmiary możliwości, które mogą być używane w obu kierunkach różne, jak pokazano na poniższym diagramie:

![](introduction-to-ios8-images/image3.png "Diagram reprezentujący siatki 2 x 2, który definiuje różne rozmiary możliwości, które mogą być używane w orientacji różne")
 
Aby uzyskać więcej informacji na temat klas rozmiar dotyczą [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md).

## <a name="photo-kit"></a>Zdjęcie Kit
Zdjęcie Kit to nowe struktury, która umożliwia aplikacjom zapytania Biblioteka obrazów systemu oraz tworzenie niestandardowych interfejsów użytkownika do wyświetlania i modyfikowania jego zawartość. Zawiera liczbę klas reprezentujących obrazu i zasoby wideo, a także kolekcji zasobów, takich jak albumów i folderów.

Aby uzyskać więcej informacji, zobacz nasze [PhotoKit](~/ios/platform/photokit.md) przewodnik.

## <a name="games"></a>gry

<a name="scenekit" />

### <a name="scene-kit"></a>Zestaw sceny

Zestaw sceny jest wykres scenę 3D interfejsu API, które upraszcza pracy z grafiki 3D. Została wprowadzona w systemie OS X 10.8 i teraz nadszedł iOS 8. Z zestawem sceny tworzenie wizualizacji 3W bez ramek i zwykłych grach 3W nie wymaga doświadczenia w OpenGL. Opierając się na wspólne pojęcia wykres sceny, zestaw sceny abstracts optymalizacji złożoności OpenGL i interfejsy OpenGL ES, dzięki czemu bardzo łatwo dodać 3D zawartości do aplikacji. Jednak w przypadku eksperta OpenGL sceny zestawu ma doskonałą pomoc techniczną dla poleceń bezpośrednio z również OpenGL. Ponadto zawiera wiele funkcji, które stanowią uzupełnienie grafiki 3D, takie jak fizycznych, a bardzo dobrze integruje się z kilku innych platform firmy Apple, takich jak zestaw Sprite, obrazu Core i Core animacji.

Aby uzyskać więcej informacji, zobacz nasze [SceneKit](~/ios/platform/gaming/scenekit.md) dokumentacji.

<a name="spritekit" />

### <a name="sprite-kit"></a>Zestaw Sprite

Zestaw Sprite, gier 2W platformę od firmy Apple, posiada niektóre ciekawe nowe funkcje w systemie iOS 8 i OS X Yosemite. Obejmuje to integrację z zestawu sceny, obsługuje programów do cieniowania oświetlenia, cieni, ograniczenia, generowania mapy normalnej i ulepszenia fizycznych. W szczególności nowe funkcje fizycznych ułatwiają bardzo realistyczne efektów gier.

Aby uzyskać więcej informacji, zobacz nasze [SpriteKit](~/ios/platform/gaming/spritekit.md) dokumentacji.

## <a name="other-changes"></a>Inne zmiany
Oraz najważniejszych zmian w systemie iOS 8, które są opisane powyżej Apple zaktualizował dodatkowo wielu istniejących struktur. Te szczegóły są przedstawione poniżej:

- **[Podstawowe obrazu](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)**  — Apple rozwinął po jego platforma przetwarzania obrazu, dodając lepszą obsługę wykrywania prostokątne regionów i kodów QR wewnątrz obrazów. Jan Bluestein Eksploruje to jego blogu post prawo [wykrywania obrazu w systemie iOS 8](http://blog.xamarin.com/image-detection-in-ios-8/)

## <a name="deprecated-apis"></a>Przestarzałe interfejsy API
Z wszystkich usprawnienia wprowadzone w systemie iOS 8 przestarzałe ma kilka interfejsów API. Niektóre z nich są szczegółowo opisane poniżej.

- **[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)**  — metody i właściwości używana do rejestracji powiadomień zdalnego ma przestarzały. Są to registerForRemoteNotificationTypes i enabledRemoteNotificationTypes.
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)**  — cech i rozmiar klasy zastąpienia metody i właściwości używana do opisu interfejsu orientacji. Zapoznaj się [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) Aby uzyskać więcej informacji na temat sposobu ich używać.

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)**  — ta funkcja została zastąpiona przez UISearchController w iOS8.

## <a name="summary"></a>Podsumowanie
W tym artykule analizujemy niektóre nowe funkcje wprowadzone w systemie iOS 8 firmy Apple.



## <a name="related-links"></a>Linki pokrewne

- [UIKitEnhancements (przykład)](https://developer.xamarin.com/samples/monotouch/ios8/UIKitEnhancements)
- [Wprowadzenie do rozszerzeń aplikacji](~/ios/platform/extensions.md)
- [Wprowadzenie do CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)
- [Wprowadzenie do selektora dokumentu](~/ios/platform/document-picker.md)
- [Wprowadzenie do HealthKit](~/ios/platform/healthkit.md)
- [Wprowadzenie do formantów ręczne aparatu](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Wprowadzenie do czytnika TouchID](~/ios/platform/touchid.md)
- [Wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md)
