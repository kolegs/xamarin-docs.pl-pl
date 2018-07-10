---
title: dla systemu iOS, funkcje zabezpieczeń i prywatności
description: W tym dokumencie opisano funkcje zabezpieczeń i prywatności systemu IOS i opisano, jak z nich korzystać z platformy Xamarin.iOS. Zajmuje się aktualizacje wprowadzone w systemie iOS 10 oraz sposób dostępu do prywatnych danych użytkowników.
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 1a28bf394d29c09bfd264f03e0eea6c8b582f271
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854756"
---
# <a name="ios-security-and-privacy-features"></a>dla systemu iOS, funkcje zabezpieczeń i prywatności

_Ten artykuł dotyczy pracy z zabezpieczeń i prywatności w systemie iOS i ich wpływ na aplikację platformy Xamarin.iOS._

Apple wprowadziła kilka dodatkowych funkcji zabezpieczeń i prywatności w systemie iOS 10 (lub nowszym), które pomogą developer zwiększające bezpieczeństwo swoich aplikacji i upewnij się, ochrony prywatności użytkowników końcowych. W tym artykule opisano wdrażanie tych funkcji w aplikacji platformy Xamarin.iOS.
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Ogólne ulepszenia

Wprowadzono następujące zmiany ogólnego bezpieczeństwa i ochrony prywatności w systemie iOS 10:

- Wspólne dane zabezpieczeń architektury (CDSA) interfejsu API jest przestarzały i powinien zostać zamieniony API SecKey do generowania kluczy asymetrycznych.
- Nowy `NSAllowsArbitraryLoadsInWebContent` klucza, można dodać do aplikacji **Info.plist** pliku i umożliwi stron sieci web załadować się poprawnie, gdy Apple Transport Security (ATS) jest nadal chroniona przez pozostałą część aplikacji. Aby uzyskać więcej informacji, zobacz nasze [App Transport Security](~/ios/app-fundamentals/ats.md) dokumentacji.
- Ponieważ nowe Schowka w systemie iOS 10 i macOS Sierra umożliwia użytkownikowi kopiowanie i wklejanie między urządzeniami, interfejs API został rozszerzony w celu umożliwienia Schowka ograniczone do określonego urządzenia i oznaczony sygnaturą czasową zostaje wyczyszczona automatycznie w danym momencie. Ponadto większa o nazwie nie są zachowywane i powinien zostać zamieniony udostępnionych stołu montażowego kontenerów.
- Dla wszystkich połączeń SSL/TLS szyfrowania symetrycznego RC4 teraz jest domyślnie wyłączona. Ponadto transportu zabezpieczanie interfejsu API nie obsługuje już protokołu SSLv3 i zaleca się, że deweloper zatrzymana, tak szybko, jak to możliwe przy użyciu algorytmu SHA-1 i 3DES kryptografii.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Uzyskiwanie dostępu do prywatnych danych użytkowników

Aplikacje działające w systemie iOS 10 (lub późniejszy) statycznie musi deklarować ich zamiar dostępu do określonych funkcji lub informacje o użytkowniku, wprowadzając co najmniej jeden klucz prywatności w ich **Info.plist** pliki, które wyjaśniają, dlaczego aplikacja chce uzyskać użytkownikowi dostęp.

> [!IMPORTANT]
> Aplikacje, które nie zapewniają wymagane klucze dyskretnie przestaną obowiązywać przez system po podjęciu próby dostępu do jednego z ograniczeniami funkcji lub informacje o użytkowniku, _bez błędów_! Jeśli aplikacja zostanie uruchomiony, niespodziewanie kończy się niepowodzeniem w systemie iOS 10, upewnij się, że wszystkie wymagane **Info.plist** , które zostały określone.

Następujące zachowania związane z kluczami są dostępne:

- **Prywatność — użycie Apple Music opis** (`NSAppleMusicUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskać dostęp biblioteki multimediów użytkownika.
- **Prywatność — opis użycia usługi peryferyjnych Bluetooth** (`NSBluetoothPeripheralUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce, aby dostęp do funkcji Bluetooth na urządzeniu użytkownika.
- **Prywatność — opis użycia kalendarzy** (`NSCalendarsUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskać dostęp użytkownika kalendarza.
- **Prywatność — opis użycia aparatu** (`NSCameraUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja potrzebuje dostępu do aparatu fotograficznego urządzenia.
- **Prywatność — opis użycia kontaktów** (`NSContactsUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskiwać dostęp do kontaktów użytkownika.
- **Prywatność — opis użycia usługi udostępniania kondycji** (`NSHealthShareUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskać dostęp do danych kondycji użytkownika. Aby uzyskać więcej informacji, zobacz firmy Apple [odwołań do klas HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Prywatność — opis użycia aktualizacji kondycji** (`NSHealthUpdateUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce edytować dane kondycji przez użytkownika. Aby uzyskać więcej informacji, zobacz firmy Apple [odwołań do klas HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Prywatność — opis użycia usługi HomeKit** (`NSHomeKitUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskiwać dostęp do danych konfiguracji HomeKit użytkownika.
- **Prywatność — zawsze opis użycia lokalizacji** (`NSLocationAlwaysUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce, aby zawsze mieć dostęp do lokalizacji użytkownika.
- [Przestarzałe] **Prywatność — opis użycia lokalizacji** (`NSLocationUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskać dostęp do lokalizacji użytkownika. *Uwaga: Ten klucz jest przestarzała w systemie iOS 8 (i nowszych). Użyj `NSLocationAlwaysUsageDescription` lub `NSLocationWhenInUseUsageDescription` zamiast tego.*
- **Prywatność — gdy w użyciu opis użycia lokalizacji** (`NSLocationWhenInUseUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskać dostęp do lokalizacji użytkownika, gdy jest on uruchomiony.
- [Przestarzałe] **Prywatność — opis użycia biblioteki multimediów** — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskać dostęp do biblioteki multimediów użytkownika. *Uwaga: Ten klucz jest przestarzała w systemie iOS 8 (i nowszych). Użyj `NSAppleMusicUsageDescription` zamiast tego.*
- **Prywatność — opis użycia mikrofonu** (`NSMicrophoneUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskać dostęp do mikrofonu urządzenia.
- **Prywatność — opis użycia funkcji ruchu** (`NSMotionUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskać dostęp przyspieszeniomierza urządzenia.
- **Prywatność — opis użycia biblioteki zdjęć** (`NSPhotoLibraryUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskać dostęp biblioteki zdjęć użytkownika.
- **Prywatność — opis użycia przypomnień** (`NSRemindersUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskać dostęp użytkownika przypomnienia.
- **Prywatność — opis użycia usługi Siri** (`NSSiriUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce wysyłać dane użytkownika Siri.
- **Prywatność — opis użycia usługi rozpoznawania mowy** (`NSSpeechRecognitionUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce wysyłać dane użytkownika do serwerów rozpoznawania mowy firmy Apple.
- **Prywatność — opis użycia usługi dostawcy TV** (`NSVideoSubscriberAccountUsageDescription`) — umożliwia deweloperowi Opisz, dlaczego aplikacja chce uzyskać dostęp konta dostawcy TV.

Aby uzyskać więcej informacji na temat pracy z **Info.plist** kluczy, zobacz firmy Apple [informacji właściwości listy kluczach](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Ustawienia prywatności kluczy

Wykonaj poniższy przykład uzyskiwania dostępu do "HomeKit" w systemie iOS 10 (lub nowszym), deweloper będzie konieczne dodanie `NSHomeKitUsageDescription` klucza w aplikacji **Info.plist** plik, a następnie podaj parametry deklarowania, dlaczego aplikacja chce uzyskać dostęp przez użytkownika zestaw HomeKit Baza danych. Ten ciąg zostanie ona wyświetlona użytkownikowi przy pierwszym uruchomieniu aplikacji:

![Przedstawia przykładowy alert NSHomeKitUsageDescription](security-privacy-images/info01.png "przedstawia przykładowy alert NSHomeKitUsageDescription")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Obecnie Xamarin.iOS dla programu Visual Studio nie obsługuje edytowania **Info.plist** edytorze manifestu klucze prywatności z w ramach domyślnego dla systemu iOS. Zamiast tego należy używać ogólnych Edytor PList więc wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy **Info.plist** w pliku **Eksploratora rozwiązań** i wybierz **Otwórz za pomocą...** .
2. Wybierz **Edytor PList ogólny** z listy programów, aby otworzyć plik, następnie kliknij przycisk **OK**.

    ![Wybierz edytor PList ogólny](security-privacy-images/InfoEditorSelectionVs.png "wybierz edytor PList ogólny")
3. Kliknij przycisk **+** przycisku w ostatnim wierszu w edytorze Aby dodać nowy wpis do listy. To będzie nazywane "Custom", typ właściwości to równa `String` i wartość pustą.
4. Kliknij nazwę właściwości, a pojawi się listy rozwijanej.
5. Z listy rozwijanej wybierz klucz prywatności (takie jak **prywatność — opis użycia usługi HomeKit**): 

    ![Wybierz klucz prywatności](security-privacy-images/InfoPListEditorSelectKey.png "wybierz klucz prywatności")
6. Wprowadź opis w kolumnie wartość Dlaczego aplikacja chce uzyskać dostęp do informacji w danej funkcji lub użytkownika: 

    ![Wprowadź opis](security-privacy-images/InfoPListSetValue.png "wprowadź opis")
7. Zapisz zmiany w pliku.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby ustawić klawiszy ochrony prywatności, wykonaj następujące czynności:

1. Kliknij dwukrotnie **Info.plist** w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji.
2. W dolnej części ekranu, przełącz się do **źródła** widoku.
3. Dodaj nową **wpis** do listy.
4. Z listy rozwijanej wybierz klucz prywatności (takie jak **prywatność — opis użycia usługi HomeKit**): 

    ![Wybierz klucz prywatności](security-privacy-images/info02.png "wybierz klucz prywatności")
5. Wprowadź opis dlaczego aplikacja chce uzyskać dostęp do informacji w danej funkcji lub użytkownika: 

    ![Wprowadź opis](security-privacy-images/info03.png "wprowadź opis")
6. Zapisz zmiany w pliku.

-----

> [!IMPORTANT]
> W przykładzie podanej powyżej, można ustawić `NSHomeKitUsageDescription` w **Info.plist** pliku mogłoby spowodować jej _dyskretnie awarie_ (zamknięte przez system w czasie wykonywania) bez błędów, gdy są uruchomione w systemie iOS 10 ( lub nowszego).

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

Ten artykuł ma objętych usługą, zabezpieczenia i prywatność zmian, Apple ma w systemie iOS 10 i ich wpływ na aplikację platformy Xamarin.iOS.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
