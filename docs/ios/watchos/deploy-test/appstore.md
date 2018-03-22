---
title: "Wdrażanie do sklepu z aplikacjami"
description: "Wdrażanie aplikacji czujki do sklepu z aplikacjami"
ms.topic: article
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c5b89570fdd3df80d39c6621fcd12a23babed9ee
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="deploying-to-the-app-store"></a>Wdrażanie do sklepu z aplikacjami

> [!IMPORTANT]
> Należy zapoznać się z tematem [firmy Apple Watch zestawu przesyłanie przewodnik](https://developer.apple.com/app-store/watch/)i zobacz [Rozwiązywanie problemów](#Troubleshooting) sekcji może mieć problemy.

- Upewnij się, że masz:
  - [**Profile inicjowania obsługi dystrybucji** ](#provisioning) utworzone dla projektów.
  - **Cel wdrożenia** (`MinimumOSVersion`) dla systemu iOS nadrzędnego zestawu aplikacji do **8.2** lub wcześniej (8.3 nie jest obsługiwane).

- W [ **iTunes Connect**](#iTunes_Connect):

  - Utwórz wpis aplikacji dla systemu iOS (lub Dodaj **nowej wersji** do istniejącej aplikacji).
  - Dodaj wyrażenie kontrolne ikony, jak i zrzuty ekranu.

- Następnie w [programu Visual Studio for Mac](#xamarin_studio) (Visual Studio nie jest obecnie obsługiwana):

  - Kliknij prawym przyciskiem myszy aplikacji systemu iOS i wybierz polecenie **Ustaw jako projekt startowy**.
  - Zmień na **sklepu z aplikacjami** konfiguracji.
  - Użyj **archiwum** funkcji utworzyć archiwum aplikacji.

- Na koniec, przełącz się do [Xcode 6.2 +](#xcode)

  - Przejdź do **okna > organizatora** i wybierz polecenie **archiwa**.
  - Wybierz z listy aplikacji i archiwum.
  - (Opcjonalnie) **Sprawdzania poprawności...**  archiwum.
  - **Prześlij...**  archiwum i wykonaj kroki, aby przekazać do iTunes połączenia do przeglądu i zatwierdzania.

Wskazówek określonych powiązane z tymi poniżej. Zobacz [Rozwiązywanie problemów](#Troubleshooting) sekcji, jeśli masz problemy.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>Profile aprowizacji dystrybucji

Tworzenie wdrożenia sklepu z aplikacjami, należy utworzyć **profil inicjowania obsługi administracyjnej dystrybucji** dla każdego Identyfikatora aplikacji w rozwiązaniu.

Jeśli masz identyfikator aplikacji, symbol wieloznaczny *tylko jeden profil inicjowania obsługi będzie wymagane*; ale jeśli masz oddzielne Identyfikatora aplikacji dla każdego projektu, należy profilu inicjowania obsługi administracyjnej dla każdego Identyfikatora aplikacji:

![](appstore-images/provisioningprofile-distribution-sml.png "Profil dystrybucji magazynu aplikacji")

Po utworzeniu wszystkich trzech profilów wyświetlone na liście. Pamiętaj, aby pobrać i zainstalować każdą z nich (przez dwukrotne kliknięcie go):

![](appstore-images/provisioningprofiles-sml.png "Listy dostępnych profilów")

Możesz sprawdzić, profilu inicjowania obsługi administracyjnej w **opcje projektu** wybierając **kompilacji > iOS podpisywania pakietu** ekranu i wybierając **sklepu AppStore | iPhone** Konfiguracja.

**Profilu inicjowania obsługi administracyjnej** listy zostaną wyświetlone wszystkie pasujących profilów — pasujących profilów utworzone na tej liście rozwijanej powinny być widoczne.

![](appstore-images/options-selectprofile-sml.png "Okno dialogowe podpisywania pakietu systemu iOS")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

Postępuj zgodnie z [omówienie dystrybucji aplikacji](~/ios/deploy-test/app-distribution/index.md), w szczególności:

- [Konfigurowanie aplikacji w iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikowanie w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

Podczas konfigurowania aplikacji w iTunes Connect, nie zapomnij dodać ikona monitorowania i zrzuty ekranu:

![](appstore-images/itunesconnect-watch-sml.png "Ikona monitorowania i zrzuty ekranu w iTunes Connect")

Plik ikony powinny być pikseli 1024 x 1024 i będzie miał maskę cykliczne zastosować dla niego pojawi się. Ikona nie powinna mieć kanał alfa.

Co najmniej jeden zrzut ekranu jest wymagana, maksymalnie pięć mogą zostać przekazane.
Powinny być pikseli 312 x 390 i prezentacja aplikacji czujki w akcji.
Symulator czujki 42mm umożliwia zrzutów ekranu w tym rozmiarze.


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. Upewnij się, że aplikacja systemu iOS jest projekt startowy. Jeśli nie, kliknij prawym przyciskiem myszy, aby ustawić ją:

  ![](appstore-images/xs-startup.png "Ustawienia projektu startowego")

2. Wybierz **sklepu AppStore** Konfiguracja kompilacji:

  ![](appstore-images/xs-appstore.png "Konfiguracja kompilacji sklepu z aplikacjami")

3. Wybierz **kompilacji > archiwum** element menu, aby rozpocząć proces archiwum:

  ![](appstore-images/xs-archive.png "W menu kompilacji")

Można również wybrać **Widok > archiwa...**  element menu, aby zobaczyć archiwum, które zostały utworzone wcześniej.

  ![](appstore-images/xs-archives-sml.png "Wyświetl archiwa")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode automatycznie wyświetli archiwa utworzone w programie Visual Studio dla komputerów Mac.

1. Uruchom środowisko Xcode i wybierz polecenie **okna > organizatora**:

  ![](appstore-images/xc-organizer.png "Menu okna")

2. Przełącz się do **archiwa** i wybierz archiwum, który został utworzony za pomocą programu Visual Studio dla komputerów Mac:

  ![](appstore-images/xc-archives.png "Na karcie archiwa")

3. Opcjonalnie **sprawdzania poprawności...**  archiwum, a następnie wybierz **przesyłania...**  do przekazania aplikacji do iTunes Connect.

4. Wybierz zespół deweloperów (Jeśli użytkownik należy do więcej niż jeden), a następnie potwierdź przesyłania:

  ![](appstore-images/xc-submit1.png "Programowanie sekcji zespołu")

5. Odwiedź stronę iTunes Połącz ponownie, aby zobaczyć przekazanego pliku binarnego. Przejdź do strony konfiguracji aplikacji i wybierz **wersja wstępna** z górnego menu, aby wyświetlić **kompilacje** listy:

  [![](appstore-images/itc-prerelease-sml.png "Strona konfiguracji aplikacji w iTunes Connect")](appstore-images/itc-prerelease.png#lightbox)

Następnie można przesłać do zatwierdzenia aplikacji na **wersji** strony. Zapoznaj się [omówienie dystrybucji aplikacji systemu iOS](~/ios/deploy-test/app-distribution/index.md) Aby uzyskać więcej informacji.


## <a name="troubleshooting"></a>Rozwiązywanie problemów

Poniżej przedstawiono niektóre błędy, które mogą wystąpić podczas przekazywania do sklepu z aplikacjami i czynności, które należy wykonać, aby je rozwiązać.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>Opcja menu archiwum nie jest widoczny w programie Visual Studio dla komputerów Mac

Postępuj zgodnie z [powyższe kroki](#xamarin_studio) w celu skonfigurowania rozwiązania do archiwizacji. Jeśli projekt startowy nie zostały prawidłowo skonfigurowane, upewnij się, że konfiguracja kompilacji najpierw ustaw debugowania i wydania przed podjęciem próby zmiany projekt startowy. A następnie ustaw dla konfiguracji kompilacji z powrotem do **sklepu AppStore**.

### <a name="invalid-icon"></a>Ikona elementów nieprawidłowych

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcons27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Postępuj zgodnie z [instrukcje dotyczące usuwania kanału alfa](~/ios/watchos/troubleshooting.md) z ikony.

### <a name="cfbundleversion-mismatch"></a>Niezgodność CFBundleVersion

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

Wszystkie projekty w rozwiązaniu — aplikacja systemu iOS, obejrzyj rozszerzenia i aplikacji czujki — powinien być używany ten sam numer wersji. Edytuj każdego **Info.plist** pliku tak, aby dokładnie numer wersji.

### <a name="missing-icons"></a>Brak ikon

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

Postępuj zgodnie z instrukcjami [Praca z ikonami](~/ios/watchos/app-fundamentals/icons.md) do dodania do projektu aplikacji czujki wymagane obrazy.

### <a name="missing-icon"></a>Brak ikony

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Upewnij się, masz najnowszą wersję programu Visual Studio dla komputerów Mac, a Twoje **AppIcons.appiconset** zawiera pełen zestaw obrazów. Jeśli są nadal występuje błąd, Wyświetl źródło **Contents.json** aby potwierdzić zawiera wpis dla wszystkich wymaganych obrazów. Alternatywnie, po upewnieniu się, używasz najnowszej wersji programu Xamarin, Usuń i Utwórz ponownie **AppIcons.appiconset**.

> [!IMPORTANT]
> Jest znaną usterką w programie Visual Studio obsługi dla komputerów Mac czujki ikony: oczekuje 88 x 88 piksel obrazu  **29x29@3x**  obrazu (która powinna być pikseli 87 x 87).


Nie można rozwiązać ten problem w programie Visual Studio dla komputerów Mac — albo zasób obrazu w środowisku Xcode edycji lub ręcznie edytować **Contents.json** plików (do dopasowania [w tym przykładzie](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).



### <a name="invalid-watchkit-support"></a>Nieprawidłowa obsługa WatchKit

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

Ten komunikat może być umieszczony podczas sprawdzania poprawności i przesyłanie, lub automatyczne wiadomości e-mail z programu iTunes Connect po przesyłania wydaje się pomyślnie.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> Należy **archiwum** aplikacji w programie Visual Studio for Mac i następnie przełączasz xcode 6.2 + do sprawdzania poprawności i przekazać do iTunes Connect.


Skorzystaj z kanału Xamarin stabilny i Xcode 6.2 +.



### <a name="invalid-provisioning-profile"></a>Nieprawidłowy profil inicjowania obsługi administracyjnej

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**Profile inicjowania obsługi dystrybucji** należy podać dla wszystkich trzech projektów w rozwiązaniu aplikacji czujki: aplikacja systemu iOS, rozszerzenie czujki i aplikacja Czujka — albo jawnie (trzy profile) lub za pośrednictwem profilu jeden symbol wieloznaczny. Sprawdź, czy profile inicjowania obsługi administracyjnej znajdują się w Centrum deweloperów systemu iOS i zostały pobrane i dodane do Twojej Mac.

### <a name="invalid-code-signing-entitlements"></a>Nieprawidłowy kod podpisywania uprawnień

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Upewnij się, do profilu inicjowania obsługi administracyjnej są konfiguracji prawidłowo w Centrum deweloperów firmy Apple i zostały pobrane i ich instalacji. Sprawdź również są ustawione dla komputerów Mac na okna właściwości dla każdego projektu w programie Visual Studio.

### <a name="invalid-architecture"></a>Nieprawidłowa architektura

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Obejrzyj aplikacje można dodawać tylko [Unified API (64-bitowy)](~/cross-platform/macios/unified/index.md) aplikacji platformy Xamarin.iOS.
Kliknij prawym przyciskiem myszy projekt aplikacji systemu iOS, a następnie przejdź do **Opcje > kompilacji > kompilacji systemu iOS > Zaawansowane** i upewnij się, że **architektur obsługiwanych** dla telefonu iPhone sklepu AppStore Konfiguracja obejmuje **ARM64** (np.) **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>Ten pakiet jest nieprawidłowy.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

Aplikacji systemu iOS nadrzędny musi mieć MinimumOSVersion ustawiony na "8.2" lub wcześniej.

### <a name="non-public-api-usage"></a>Niepubliczne użycia interfejsu API

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Upewnij się, że używasz najnowszej wersji narzędzia Xcode i Xamarin firmy.
Kod nie powinien uzyskać dostępu do żadnych niepublicznych interfejsów API.

### <a name="build-error-mt5309"></a>Błąd MT5309 kompilacji

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

Ten błąd jest prawdopodobnie wynik użytkownika o zmieniono jego nazwę instalacji Xcode z **Xcode.app**. Na przykład ten błąd wystąpi w przypadku zmiany nazwy instalację do **XCode 6.2.app**.



## <a name="related-links"></a>Linki pokrewne

- [Przewodnik przesyłanie WatchKit firmy Apple](https://developer.apple.com/app-store/watch/)
