---
title: Praca z uprawnieniami
description: Uprawnienia są możliwości aplikacji specjalnych i uprawnienia zabezpieczeń do aplikacji, które są poprawnie skonfigurowane do korzystania z nich.
ms.prod: xamarin
ms.assetid: 8A3961A2-02AB-4228-A41D-06CB4108D9D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: f158ab7e51eb7610566ed052b326fecf016add8a
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="working-with-entitlements"></a>Praca z uprawnieniami

_Uprawnienia są możliwości aplikacji specjalnych i uprawnienia zabezpieczeń do aplikacji, które są poprawnie skonfigurowane do korzystania z nich._

W systemie iOS, aplikacje działać w _piaskownicy_, która oferuje zestaw reguł, które ograniczają dostęp między aplikacji i niektóre zasoby systemowe lub dane użytkownika. _Uprawnień do_ są używane do żądania, że system rozwiń piaskownicy, aby udzielić aplikacji dodatkowe możliwości.

Aby rozszerzyć możliwości aplikacji, należy podać uprawnienia w pliku Entitlements.plist aplikacji. Tylko pewnych funkcji, można ją rozszerzyć, i są wyświetlane na liście w [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/index.md) prowadzą i opisem [poniżej](#keyreference). Uprawnienia są przekazywane w systemie jako para klucza i wartości, a zwykle tylko jeden z nich jest wymagany na możliwości. Określone klucze i wartości, które zostały opisane w [uprawnienia z informacjami o kluczach](#keyreference) dalszej części tego przewodnika.
Program Visual Studio dla komputerów Mac i Visual Studio udostępniają wyczyść interfejs dodawania uprawnień w aplikacji platformy Xamarin.iOS przy użyciu edytora Entitlements.plist.
W tym przewodniku przedstawiono Edytor Entitlements.plist i jak z niego korzystać. Gwarantuje to również odwołanie wszystkich uprawnień, które mogą zostać dodane do projektu systemu iOS dla każdej funkcji.

## <a name="entitlements-and-provisioning"></a>Uprawnień i udostępniania


Plik Entitlements.plist służy do określania uprawnień i jest używany do podpisywania pakietu aplikacji.

Jednak niektóre dodatkowe inicjowania obsługi administracyjnej jest wymagane, aby upewnić się, że dana aplikacja jest poprawnie podpisany kod. Profil informacyjny używany musi zawierać identyfikator aplikacji, z możliwością wymagane włączone. Aby uzyskać informacje o tym, zapoznaj się [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik.

> [!IMPORTANT]
> Plik Entitlements.plist pomaga wypełnić prawidłowe właściwości dla aplikacji za pomocą funkcji, ale nie można wygenerować profilu inicjowania obsługi administracyjnej, ponieważ nie jest połączony z konta dewelopera firmy Apple. Należy także wygenerować profilu inicjowania obsługi administracyjnej, wdrażanie i dystrybucja aplikacji za pomocą portalu dla deweloperów.

## <a name="set-entitlements-in-a-xamarinios-project"></a>Ustawianie uprawnień w projekcie platformy Xamarin.iOS

Oprócz wybranie i skonfigurowanie usług wymaganych aplikacji, określając identyfikator aplikacji, uprawnienia muszą być również skonfigurowane w projekcie platformy Xamarin.iOS edytując **Info.plist** i  **Entitlements.plist** plików.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby skonfigurować uprawnienia w programie Visual Studio dla komputerów Mac, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **Info.plist** plik, aby otworzyć do edycji.
2. W **iOS aplikacji docelowej** , wprowadź nazwę aplikacji i wprowadź **identyfikator pakietu** utworzony podczas definiowania identyfikator aplikacji:

  ![](entitlements-images/servicexs01.png "Wprowadź identyfikator pakietu")

3. Zapisać zmiany w **Info.plist** pliku.
4. W **Eksploratora rozwiązań**, kliknij dwukrotnie **Entitlements.plist** plik, aby otworzyć do edycji:

    ![](entitlements-images/servicexs02.png "Uprawnienia do edycji")

5. Wybierz i skonfiguruj wszystkie uprawnienia wymagane dla aplikacji platformy Xamarin.iOS, tak aby były zgodne z Instalatora, która została zdefiniowana, jeśli został utworzony identyfikator aplikacji.
6. Zapisać zmiany w **Entitlements.plist** pliku.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby skonfigurować uprawnienia w programie Visual Studio, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Info.plist**, wybierz pozycję **Otwórz za pomocą...** i **Edytor listy właściwości** plik, aby otworzyć do edycji.
2. W **iOS aplikacji docelowej** , wprowadź nazwę aplikacji i wprowadź **identyfikator pakietu** utworzony podczas definiowania identyfikator aplikacji:

  ![](entitlements-images/servicevs01.png "Ustawianie identyfikatora pakietu")

3. Zapisać zmiany w **Info.plist** pliku.
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Entitlements.plist** pliku, wybierz opcję **Otwórz za pomocą...** i **Edytor listy właściwości** go otworzyć do edycji:

    ![](entitlements-images/servicevs02.png "Uprawnienia do edycji")

    Alternatywnie dwukrotne kliknięcie **Entitlements.plist** plik zostanie otwarty w edytorze XML źródła, którego będzie można ustawić wartości właściwości i klucz uprawnienia zgodnie z opisem w [uprawnień do odwołania](#keyreference)poniższej sekcji.

5. Wybierz i skonfiguruj wszystkie uprawnienia wymagane dla aplikacji platformy Xamarin.iOS, tak aby były zgodne z Instalatora, która została zdefiniowana, jeśli został utworzony identyfikator aplikacji.
6. Zapisać zmiany w **Entitlements.plist** pliku.


-----

<a name="add-new" />

## <a name="adding-a-new-entitlementsplist-file"></a>Dodanie nowego pliku Entitlements.plist

Uprawnienia są dodawane do aplikacji za pomocą pliku Entitlements.plist. Ten plik znajduje się w projektach Xamarin.iOS domyślnie, ale może być brak starsze projektów.

Aby dodać plik Entitlements.plist do Twojej platformy Xamarin.iOS, należy wykonać następujące czynności:

1.  Kliknij prawym przyciskiem myszy w pliku projektu, a następnie przejdź do **Dodaj > Nowy plik...** :

    ![Dodaj menu kontekstowe plików](entitlements-images/image1.png)
2.  W oknie dialogowym Nowy plik wybierz **systemu iOS > listę właściwości** i nadaj mu nazwę uprawnień:

    ![Okno dialogowe nowego pliku](entitlements-images/image2.png)

<a name="keyreference" />

## <a name="entitlement-key-reference"></a>Uprawnienia klucza odwołania

Klucze uprawnienia można dodać za pomocą panelu Źródło Entitlements.plist edytora. Wymagane klucze będą zwykle można dodać przy użyciu edytora Entitlements.plist, ale są wymienione poniżej odwołania.

### <a name="wallet"></a>Portfela

*   **Opis elementu**: wcześniej znany jako Passbook, portfela to aplikacja, która przechowuje i którymi zarządza przebiegów. Przekazuje te mogą być kart kredytowych, kart magazynu, wstępu lub biletów.

    - **Identyfikator typu — dostęp próbny**
        * **Klucze**: com.apple.developer.pass typu identyfikatorów
        * **Ciąg**: `$(TeamIdentifierPrefix)*`

* **Uwagi dotyczące**:
    - To spowoduje włączenie aplikacji zezwala na przekazywanie wszystkich typów. Ograniczenie aplikacji i dopuszcza tylko podzbiór typów przebiegu zespołu, ustaw wartość ciągu: `$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

    Gdzie jest przekazać identyfikator, który został utworzony przez pass.$(CFBundleIdentifier) [powyżej](~/ios/platform/passkit.md)

<a name="icloud" />

### <a name="icloud"></a>usługi iCloud

*   **Opis elementu**: iCloud zapewnia użytkownicy systemu iOS wygodne i prosty sposób, aby przechowywać zawartość i udostępniać je między urządzeniami. Istnieją cztery metody deweloperzy mogą używać iCloud zapewnienie środków magazynu dla użytkowników: magazyn kluczy i wartości, magazynu UIDocument CoreData i przy użyciu CloudKit bezpośrednio w celu zapewnienia magazynu dla poszczególnych plików i katalogów. Aby uzyskać więcej informacji o tych dotyczą wprowadzenie do przewodnika iCloud.

    - **CloudKit & menty w ramach usługi iCloud**
        - **Keys**: com.apple.developer.ubiquity-container-identifiers
        - **Ciąg**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`
    - **iCloud KeyValue Storage**
        - **Key**: com.apple.developer.ubiquity-kvstore-identifier
        - **Ciąg**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`

* **Uwagi dotyczące**:
    - `$(TeamIdentifierPrefix)` Ciąg mogą znajdować się na rejestrowanie, aby developer.apple.com i odwiedziny **Member Center > Twoje konto > Podsumowanie konta dewelopera** uzyskać identyfikator zespołu (lub poszczególne identyfikator dla deweloperów jednego). Będzie to ciąg znaków 10 (A93A5CM278 na przykład).
    - `$(CFBundleIdentifier)` Rozpoczyna się od ciągu `iCloud` i jest ustawiona, gdy kontener iCloud została utworzona zgodnie z harmonogramem kroki opisane w [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) przewodnik.
    - $`(TeamIdentifierPrefix)` i `$(CFBundleIdentifier)` symbole zastępcze mogą być używane i zostanie zamieniony poprawne wartości podczas kompilacji.

> [!IMPORTANT]
> Apple [udostępnia narzędzia](https://developer.apple.com/support/allowing-users-to-manage-data/) aby pomóc deweloperom poprawnie obsługiwać interfejsów Unii Europejskiej ogólne dane ochrony rozporządzenia (GDPR).

### <a name="app-groups"></a>Grupy aplikacji

- **Opis elementu**: grupa aplikacji umożliwia różnych aplikacji (lub aplikacji i jej rozszerzenia) dostęp do lokalizacji magazynu udostępnionego pliku.

    - **Key**: com.apple.security.application-groups
    - **Ciąg**: group.$(CFBundleIdentifier)

<a name="apple-pay" />

### <a name="apple-pay"></a>Płatności firmy Apple

- **Opis elementu**: płatności Apple umożliwia użytkownikom opłacać towarów fizycznych za pośrednictwem urządzeń z systemem iOS.
    - **Klucz**: com.apple.developer.in-app płatności
    - **Ciąg**: merchant.your.mechantid

### <a name="push-notifications"></a>Powiadomienia wypychane

- **Klucz**: punktach dostępu do środowiska
- **Ciąg**: `production` lub `development`

### <a name="siri"></a>Używanie programu Siri

- **Opis elementu**: SiriKit umożliwia aplikacji systemu iOS do świadczenia usług, które są dostępne dla Siri i map aplikacji na urządzeniu z systemem iOS przy użyciu rozszerzeń aplikacji i nowych lokalizacji docelowych i opcji interfejsu użytkownika platformy. Aby uzyskać więcej informacji zapoznaj się wprowadzenie do przewodnika SiriKit.
    - **Klucz**: com.apple.developer.siri

### <a name="personal-vpn"></a>Osobista sieć VPN

- **Klucz**: com.apple.developer.networking.vpn.api
- **Ciąg**: Zezwalaj na sieci vpn

### <a name="keychain-sharing"></a>Udostępnianie łańcucha kluczy

- **Opis elementu**: udostępnianie łańcucha kluczy umożliwia deweloperom aplikacji udostępnianie haseł, które są przechowywane w łańcuchu kluczy urządzenie z innym aplikacjom opracowywanym przez tego samego zespołu. Można ograniczyć dostęp przez przekazanie identyfikator grupy dostępu łańcucha kluczy w ciągu.
    - **Key**: keychain-access-groups
    - **Ciąg**: $(AppIdentifierPrefix) $(CFBundleIdentifier)

### <a name="inter-app-audio"></a>Dźwięk Inter-App

- **Opis elementu**: Audio między aplikacji umożliwia deweloperom strumień audio między aplikacjami.
    - **Key**: inter-app-audio
    - **Wartość logiczna**: tak

### <a name="associated-domains"></a>Skojarzone domen

- **Opis elementu**: skojarzony domen, które powinny być traktowane jako łącza uniwersalnych powinien zostać przekazany z tych uprawnień. Można zaimplementować uniwersalnych łącza umożliwiają bezpośrednich połączeń między aplikacjami a witryny sieci Web. Należy podać wpis do każdej domeny, która obsługuje Twojej aplikacji i każdego wpisu powinny rozpoczynać się od `applinks:`
    - **Klucz**: com.apple.developer.associated domen
    - **Ciąg**: webcredentials:example.com

### <a name="data-protection"></a>Ochrona danych

- **Opis elementu**: włączenie ochrony danych używa wbudowane szyfrowanie sprzętu do przechowywania poufnych danych używany w aplikacji w postaci zaszyfrowanej. Poziom ochrony jest domyślnie przeprowadzenie ochrony (plików dostępnych tylko kiedy urządzenie zostanie odblokowane, a następnie).
    - **Key**: com.apple.developer.default-data-protection
    - **Ciąg**: NSFileProtectionComplete

### <a name="homekit"></a>HomeKit

- **Opis elementu**: HomeKit framework zapewnia platformę służącą do konfigurowania, konfigurowanie i zarządzanie obsługiwanych urządzeniach Automatyzacja domu — wszystkie urządzenia z systemem iOS. Aby uzyskać więcej informacji na temat używania HomeKit dotyczą wprowadzenie do przewodnika HomeKit.
    - **Key**: com.apple.developer.homekit
    - **Wartość logiczna**: tak

### <a name="healthkit"></a>HealthKit

- **Opis elementu**: HealthKit to struktura wprowadzone w systemie iOS 8 udostępnia magazyn danych scentralizowane, skoordynowanej i bezpieczny do informacji dotyczących kondycji. Aby uzyskać więcej informacji na temat używania HealthKit dotyczą wprowadzenie do przewodnika HealthKit.
    - **Key**: com.apple.developer.healthkit
    - **Wartość logiczna**: tak

### <a name="wireless-accessory-configuration"></a>Konfiguracja zasobów bezprzewodowych

- **Opis elementu**: przy użyciu konfiguracji akcesoriów sieci bezprzewodowej umożliwia aplikacji skonfigurowanie akcesoriów Wi-Fi MFI
    - **Key**: com.apple.external-accessory.wireless-configuration
    - **Wartość logiczna**: tak

## <a name="summary"></a>Podsumowanie

W tym przewodniku wprowadzono uprawnień i sposób ich użycia w programie Visual Studio dla komputerów Mac i w programie Visual Studio. Dostępne również odwołanie par klucz/wartość dla każdej funkcji.