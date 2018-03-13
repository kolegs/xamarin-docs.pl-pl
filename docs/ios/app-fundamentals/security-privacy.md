---
title: "iOS funkcje zabezpieczeń i prywatności"
description: "W tym artykule omówiono pracy z prywatności i bezpieczeństwa w systemów iOS i ich wpływ na aplikację platformy Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 4747fb73358a60d10832a1e650acd90a5a4274d1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="ios-security-and-privacy-features"></a>iOS funkcje zabezpieczeń i prywatności

_W tym artykule omówiono pracy z prywatności i bezpieczeństwa w systemów iOS i ich wpływ na aplikację platformy Xamarin.iOS._

Apple wprowadziła kilka ulepszeń zarówno zabezpieczeń i prywatności w systemie iOS 10 (i nowsze), ułatwiające developer poprawy zabezpieczeń aplikacji i zapewnienia zachowania poufności użytkownika końcowego. W tym artykule opisano wdrażanie tych funkcji w aplikacji platformy Xamarin.iOS.

Poniższe tematy zostanie omówiona szczegółowo:

- [Ogólne ulepszenia](#General-Enhancements)
- [Uzyskiwanie dostępu do danych użytkownika prywatnych](#Accessing-Private-User-Data)
    - [Ustawienia prywatności kluczy](#Setting-Privacy-Keys)
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Ogólne ulepszenia

Wprowadzono następujące zmiany ogólne zabezpieczeń i prywatności w systemie iOS 10:

- Wspólne dane zabezpieczeń architektura (CDSA) interfejsu API jest przestarzała i powinna zostać zastąpiona w interfejsie API SecKey, aby wygenerować klucze asymetryczne.
- Nowy `NSAllowsArbitraryLoadsInWebContent` klucza można dodać do aplikacji `Info.plist` pliku i umożliwi stron sieci web można prawidłowo załadować podczas zabezpieczeń transportu firmy Apple (ATS) jest nadal chroniona w pozostałej części aplikacji. Aby uzyskać więcej informacji, zobacz nasze [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md) dokumentacji.
- Ponieważ nowego Schowka w systemie iOS 10 i macOS Sierra umożliwia użytkownikowi kopiowanie i wklejanie między urządzeniami, interfejsu API zostały rozszerzone umożliwia Schowka ograniczone do określonego urządzenia i oznaczony znacznikiem czasowym, aby automatycznie wyczyszczona w danym momencie. Ponadto większa nazwane nie są zachowywane i powinna zostać zastąpiona udostępnionych kontenerów stołu montażowego.
- W przypadku wszystkich połączeń SSL/TLS szyfrowania symetrycznego RC4 teraz jest domyślnie wyłączona. Ponadto transportu zabezpieczyć interfejs API nie obsługuje protokołów SSLv3 i zaleca się, że deweloper Zatrzymaj przy użyciu algorytmu SHA-1 i 3DES kryptografii tak szybko, jak to możliwe.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Uzyskiwanie dostępu do danych użytkownika prywatnych

Aplikacje działające w systemie iOS 10 (lub nowszej), muszą deklarować statycznie zamiaru dostęp do określonych funkcji lub informacje o użytkowniku, wprowadzając co najmniej jeden klucz prywatności w ich `Info.plist` pliki, które wyjaśnić użytkownikowi przyczynę aplikacja chce uzyskać dostęp.

> [!IMPORTANT]
> **Uwaga** aplikacje, które nie zapewniają wymagane klucze zostanie dyskretnie przerwane przez system po podjęciu próby dostępu ograniczone funkcje lub informacje o użytkowniku, _bez błędu_! Jeśli aplikacja rozpoczyna się nieoczekiwanie się niepowodzeniem w systemie iOS 10, upewnij się, że wszystkie wymagane `Info.plist` zostały określone.

Następujące prywatności związane z klucze są dostępne:

- **Prywatność — użycie muzyka Apple opis** (`NSAppleMusicUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp biblioteki multimediów użytkownika.
- **Prywatność — Bluetooth urządzenia peryferyjne użycia opis** (`NSBluetoothPeripheralUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp Bluetooth na urządzeniu użytkownika.
- **Prywatność — opis użycia kalendarzy** (`NSCalendarsUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp użytkownika kalendarza.
- **Prywatność — opis użycia aparatu** (`NSCameraUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp aparatu fotograficznego urządzenia.
- **Prywatność — opis użycia kontaktów** (`NSContactsUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp kontaktów użytkownika.
- **Prywatność — opis użycie udziału kondycji** (`NSHealthShareUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp użytkownika dane kondycji. Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania do klasy HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Prywatność — opis użycia aktualizacji kondycji** (`NSHealthUpdateUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja chce edytować dane kondycji użytkownika. Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania do klasy HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Prywatność — opis użycia HomeKit** (`NSHomeKitUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja chce uzyskać dostęp do danych konfiguracji HomeKit użytkownika.
- **Prywatność — zawsze użycia opis lokalizacji** (`NSLocationAlwaysUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja chce zawsze miały dostęp do lokalizacji użytkownika.
- [Przestarzały] **Prywatność — opis użycia lokalizacji** (`NSLocationUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja potrzebuje uzyskiwać dostęp do lokalizacji użytkownika. *Uwaga: Ten klucz jest przestarzała w systemie iOS 8 (lub większą). Użyj `NSLocationAlwaysUsageDescription` lub `NSLocationWhenInUseUsageDescription` zamiast tego.*
- **Prywatność — lokalizacja podczas użycia użycia opis** (`NSLocationWhenInUseUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp z lokalizacją użytkownika jest uruchomiona.
- [Przestarzały] **Prywatność — opis użycie biblioteki multimediów** — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp do biblioteki multimediów użytkownika. *Uwaga: Ten klucz jest przestarzała w systemie iOS 8 (lub większą). Użyj `NSAppleMusicUsageDescription` zamiast tego.*
- **Prywatność — opis użycia mikrofon** (`NSMicrophoneUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp mikrofon urządzeń.
- **Prywatność — opis obciążenie ruchu** (`NSMotionUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp przyspieszeniomierza urządzenia.
- **Prywatność — opis użycie biblioteki fotografii** (`NSPhotoLibraryUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp biblioteki zdjęcia użytkownika.
- **Prywatność — opis użycia przypomnienia** (`NSRemindersUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp użytkownika przypomnienia.
- **Prywatność — opis użycia Siri** (`NSSiriUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja chce wysyłać dane użytkownika do Siri.
- **Prywatność — opis użycia rozpoznawania mowy** (`NSSpeechRecognitionUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja chce wysyłać dane użytkownika do serwerów rozpoznawania mowy firmy Apple.
- **Prywatność — opis użycia dostawcy TV** (`NSVideoSubscriberAccountUsageDescription`) — umożliwia deweloperowi opisują, dlaczego aplikacja próbuje uzyskać dostęp konta dostawcy telewizji.

Aby uzyskać więcej informacji na temat pracy z `Info.plist` kluczy, zobacz firmy Apple [informacji właściwości listy z informacjami o kluczach](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Ustawienia prywatności kluczy

Następujący przykładowy uzyskiwania dostępu do HomeKit w systemie iOS 10 (i nowsze), należy dodać dewelopera `NSHomeKitUsageDescription` klucza w aplikacji `Info.plist` plików i podać ciąg zadeklarowanie, dlaczego aplikacja potrzebuje dostępu do bazy danych HomeKit użytkownika. Ten ciąg zostanie przedstawiony użytkownikowi przy pierwszym uruchomieniu aplikacji:

[![](security-privacy-images/info01.png "Przykład NSHomeKitUsageDescription alertu")](security-privacy-images/info01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS dla bieżącej Visual Studio nie obsługuje edycji ulepszenie zabezpieczeń `Info.plist` ustawień z poziomu środowiska IDE, dlatego poniższe obejście będzie wymagane:

1. Otwórz `Info.plist` plik w edytorze tekstu zewnętrznych.
2. Przed ostatnią `</dict>` węzła, Dodaj następującego węzła: `<key>NSHomeKitUsageDescription</key>`
3. Dodaj następujące węzeł, aby podać opis wymagane: `<string>Allows the app to control HomeKit enabled devices.</string>`
4. `Info.plist` Pliku powinna wyglądać następująco: 

    [![](security-privacy-images/info02vs.png "Plik Info.plist powinien wyglądać tak")](security-privacy-images/info02vs.png#lightbox)
4. Zapisz zmiany w pliku.
5. Wróć do programu Visual Studio i skompiluj ponownie przez aplikację.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby ustawić dowolnego klucza prywatności, wykonaj następujące czynności:

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
2. W dolnej części ekranu, przełącz się do **źródła** widoku.
3. Dodaj nową **wpis** do listy.
4. Z listy rozwijanej wybierz klucz prywatności (takich jak **prywatność — opis użycia HomeKit**): 

    [![](security-privacy-images/info02.png "Wybierz klucz prywatności")](security-privacy-images/info02.png#lightbox)
5. Wprowadź opis dlaczego aplikacja próbuje uzyskać dostęp informacje podane w funkcji lub użytkownika: 

    [![](security-privacy-images/info03.png "Wprowadź opis")](security-privacy-images/info03.png#lightbox)
6. Zapisz zmiany w pliku.

-----

> [!IMPORTANT]
> **Uwaga:** w przykładzie podane powyżej, nie można ustawić `NSHomeKitUsageDescription` klucza w `Info.plist` pliku spowoduje aplikacji _awarie w trybie dyskretnym_ (zamknięte przez system w czasie wykonywania) bez błąd uruchomienia w systemie iOS 10 (lub większe).

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego zabezpieczeń i prywatności zmian że Apple ma iOS 10 i ich wpływ na aplikację platformy Xamarin.iOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
