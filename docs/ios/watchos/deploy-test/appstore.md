---
title: Wdrażanie aplikacji systemu watchOS Store aplikacji
description: W tym dokumencie opisano sposób wdrażania aplikacji systemu watchOS za pomocą platformy Xamarin, aby Store aplikacji. Zajmuje się profile aprowizacji dystrybucji i iTunes Connect, a także zawiera wskazówki dotyczące rozwiązywania problemów.
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 90058f5074759fdded5d259004cb40c0cb7ea212
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276070"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>Wdrażanie aplikacji systemu watchOS Store aplikacji

> [!IMPORTANT]
> Należy przejrzeć [firmy Apple Watch Kit przesyłania Guide](https://developer.apple.com/app-store/watch/)i zobacz [Rozwiązywanie problemów](#Troubleshooting) sekcja może mieć problemy.

- Upewnij się, że masz:
  - [**Profile aprowizacji dystrybucji** ](#provisioning) utworzone dla Twoich projektów.
  - **Cel wdrożenia** (`MinimumOSVersion`) dla systemu iOS nadrzędnej ustawienie aplikacji **8.2** lub starszy (8.3 nie jest obsługiwane).

- W [ **iTunes Connect**](#iTunes_Connect):

  - Utwórz wpis aplikacji dla systemu iOS (lub Dodaj **nowej wersji** do istniejącej aplikacji).
  - Dodaj ikonę wyrażenie kontrolne i zrzuty ekranu.

- Następnie w [programu Visual Studio dla komputerów Mac](#xamarin_studio) (Visual Studio nie jest obecnie obsługiwany):

  - Kliknij prawym przyciskiem myszy aplikację systemu iOS, a następnie wybierz **Ustaw jako projekt startowy**.
  - Zmień **App Store** konfiguracji.
  - Użyj **archiwum** funkcji utworzyć archiwum aplikacji.

- Na koniec Przełącz się do [Xcode 6.2 +](#xcode)

  - Przejdź do **okna > organizatorem** i wybierz polecenie **archiwa**.
  - Wybierz aplikację i archiwum na liście.
  - (Opcjonalnie) **Sprawdzania poprawności...**  archiwum.
  - **Prześlij...**  archiwum i wykonaj kroki, aby przekazać do usługi iTunes Connect do przeglądu i zatwierdzania.

Przeczytaj określonych wskazówki związane z tych elementów, które są poniżej. Zobacz [Rozwiązywanie problemów](#Troubleshooting) sekcji, jeśli masz problemy.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>Profile aprowizacji dystrybucji

Tworzenie wdrożenia App Store, musisz utworzyć **profilu aprowizacji dystrybucji** dla każdego Identyfikatora aplikacji w rozwiązaniu.

Jeśli używany jest symbol wieloznaczny Identyfikatora aplikacji *tylko jeden profil inicjowania obsługi będzie wymagana*; ale jeśli masz odrębny identyfikator aplikacji dla każdego projektu, a następnie należy profilu aprowizacji dla każdego Identyfikatora aplikacji:

![](appstore-images/provisioningprofile-distribution-sml.png "Profil dystrybucji Store aplikacji")

Po utworzeniu wszystkie trzy profile, które zostaną wyświetlone na liście. Pamiętaj, aby pobrać i zainstalować każdego z nich (klikając go dwukrotnie):

![](appstore-images/provisioningprofiles-sml.png "Listy dostępnych profilów")

Możesz sprawdzić profilu aprowizacji w **opcje projektu** , wybierając **kompilacji > podpisywanie pakietu systemu iOS** ekranu i wybierając polecenie **sklepu z aplikacjami | iPhone** Konfiguracja.

**Profilu aprowizacji** liście zostaną wyświetlone wszystkie pasujące profile — powinien zostać wyświetlony zgodnych profilów, utworzone na tej liście rozwijanej.

![](appstore-images/options-selectprofile-sml.png "Okno dialogowe podpisywanie zbiorów systemu iOS")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

Postępuj zgodnie z [Przegląd dystrybucji aplikacji](~/ios/deploy-test/app-distribution/index.md), w szczególności:

- [Konfigurowanie aplikacji w usłudze iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikowanie w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

Podczas konfigurowania aplikacji w usłudze iTunes Connect, nie należy zapominać dodać ikonę wyrażenie kontrolne i zrzuty ekranu:

![](appstore-images/itunesconnect-watch-sml.png "Ikona monitorowania i zrzuty ekranu w usłudze iTunes Connect")

Plik ikony powinny być 1024 x 1024 piksele i będzie miał maskę cykliczne stosowane do niego, gdy jest on wyświetlany. Ikona nie powinna mieć kanał alfa.

Co najmniej jeden zrzut ekranu jest wymagane, maks. pięć mogą zostać przekazane.
Powinny być 312 x 390 pikseli i prezentacja aplikacji Watch w działaniu.
Symulator watch 42mm umożliwia wykonywać zrzuty ekranu w tym rozmiarze.


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. Upewnij się, że aplikacja dla systemu iOS jest projekt startowy. W przeciwnym razie kliknij prawym przyciskiem myszy, aby ustawić go:

  ![](appstore-images/xs-startup.png "Ustawienia projektu startowego")

2. Wybierz **sklepu z aplikacjami** Konfiguracja kompilacji:

  ![](appstore-images/xs-appstore.png "Konfiguracja kompilacji sklepu z aplikacjami")

3. Wybierz **kompilacji > archiwum** element menu, aby rozpocząć proces archiwum:

  ![](appstore-images/xs-archive.png "W menu kompilacji")

Można również wybrać **Widok > archiwa...**  elementu menu, aby wyświetlić archiwów, które zostały utworzone wcześniej.

  ![](appstore-images/xs-archives-sml.png "Wyświetl archiwa")

<a name="xcode" />

## <a name="xcode"></a>Środowisko Xcode

Środowisko Xcode będzie automatycznie wskazywać archiwa utworzone w programie Visual Studio dla komputerów Mac.

1. Uruchom program Xcode i wybierz polecenie **okna > organizatorem**:

  ![](appstore-images/xc-organizer.png "W menu Okno")

2. Przełącz się do **archiwa** kartę, a następnie wybierz archiwum, który został utworzony za pomocą programu Visual Studio dla komputerów Mac:

  ![](appstore-images/xc-archives.png "Na karcie archiwa")

3. Opcjonalnie **sprawdzania poprawności...**  archiwum, następnie wybierz **przesyłania...**  do przekazania aplikacji do usługi iTunes Connect.

4. Wybierz zespół programistyczny (Jeśli użytkownik należy do więcej niż jeden), a następnie potwierdź przesyłania:

  ![](appstore-images/xc-submit1.png "Sekcja team development")

5. Można znaleźć w usłudze iTunes Connect ponownie, aby zobaczyć przekazanego pliku binarnego. Przejdź do strony konfiguracji aplikacji i wybierz **wersję wstępną** z górnego menu, aby wyświetlić **kompilacje** listy:

  [![](appstore-images/itc-prerelease-sml.png "Na stronie konfiguracji aplikacji w usłudze iTunes Connect")](appstore-images/itc-prerelease.png#lightbox)

Później możesz przesłać aplikację do zatwierdzenia na **wersji** strony. Zapoznaj się [Przegląd dystrybucji aplikacji dla systemu iOS](~/ios/deploy-test/app-distribution/index.md) Aby uzyskać więcej informacji.


## <a name="troubleshooting"></a>Rozwiązywanie problemów

Poniżej przedstawiono pewne błędy, które mogą wystąpić podczas przesyłania Store aplikacji i czynności, które można wykonać, aby je rozwiązać.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>Opcja menu archiwum nie jest widoczny w programie Visual Studio dla komputerów Mac

Postępuj zgodnie z [powyższe kroki](#xamarin_studio) skonfigurować rozwiązanie do archiwizacji. Jeśli projekt startowy nie może zostać ustawiona poprawnie, upewnij się, że konfiguracja kompilacji ustawiono najpierw Debuguj lub Uwolnij zanim spróbujesz zmienić projekt startowy. A następnie ustaw konfigurację kompilacji z powrotem na **sklepu z aplikacjami**.

### <a name="invalid-icon"></a>Ikona elementów nieprawidłowych

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcon27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Postępuj zgodnie z [instrukcje dotyczące usuwania kanał alfa](~/ios/watchos/troubleshooting.md) z ikony.

### <a name="cfbundleversion-mismatch"></a>Niezgodność znaków CFBundleVersion

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

Wszystkie projekty w rozwiązaniu — aplikacja systemu iOS, rozszerzenia Watch i aplikacja na zegarek — powinien używać tego samego numeru wersji. Każdy edytowanie **Info.plist** pliku numer wersji jest dokładnie zgodny.

### <a name="missing-icons"></a>Brak ikon

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

Postępuj zgodnie z instrukcjami w [Praca z ikonami](~/ios/watchos/app-fundamentals/icons.md) dodać wymagane obrazy do projektu aplikacji Watch.

### <a name="missing-icon"></a>Brak ikony

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Upewnij się, masz najnowszą wersję programu Visual Studio dla komputerów Mac, które Twoja **AppIcon.appiconset** zawiera kompletny zestaw obrazów. Jeśli nadal widzisz ten błąd, Wyświetl źródło **Contents.json** aby upewnić się, zawiera on wpis dla wymagane obrazy. Alternatywnie, po upewnieniu się, używasz najnowszej wersji programu Xamarin, usunięcie i ponowne utworzenie **AppIcon.appiconset**.

> [!IMPORTANT]
> Jest to znana Usterka w programie Visual Studio obsługi komputerów Mac Obejrzyj ikony: oczekuje, że obraz 88 x 88 pikseli dla **29x29@3x** obrazu (który powinna być pikseli 87 x 87).


Nie można rozwiązać ten problem w programie Visual Studio for Mac — albo Edytuj zasób obrazu w środowisku Xcode lub ręcznie edytować **Contents.json** plików (do dopasowania [w tym przykładzie](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).



### <a name="invalid-watchkit-support"></a>Nieprawidłowa obsługa WatchKit

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

Ten komunikat może się pojawić podczas sprawdzania poprawności i przesyłania lub w automatyczne wiadomości e-mail z usługi iTunes Connect po najwyraźniej pomyślnego wysłania.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> Należy najpierw **archiwum** aplikacji w programie Visual Studio dla systemów Mac i następnie przełączasz xcode 6.2 + zweryfikować i Przekaż do usługi iTunes Connect.


Skorzystaj z kanału stabilne platformy Xamarin i programu Xcode 6.2 +.



### <a name="invalid-provisioning-profile"></a>Nieprawidłowy profil inicjowania obsługi administracyjnej

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**Profile aprowizacji dystrybucji** muszą być dostarczone dla wszystkich trzech projektach w rozwiązaniu aplikacji watch: aplikacja systemu iOS i rozszerzenia Watch, aplikacja na zegarek — albo jawnie (trzy profile) lub za pośrednictwem profilu pojedynczego symbolu wieloznacznego. Sprawdź, czy profile inicjowania obsługi administracyjnej znajdują się w Centrum deweloperów systemu iOS oraz że pobrane i dodane do Twojego komputera Mac.

### <a name="invalid-code-signing-entitlements"></a>Nieprawidłowy kod podpisywania uprawnień

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Upewnij się, profilów aprowizacji są konfiguracji prawidłowo w Centrum deweloperów firmy Apple i że pobrane i ich instalacji. Również Sprawdź, czy są skonfigurowane w programie Visual Studio dla komputerów Mac w oknie właściwości dla każdego projektu.

### <a name="invalid-architecture"></a>Nieprawidłowa architektura

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Możesz tylko dodawać aplikacje dla zegarków [ujednoliconego interfejsu API (wersja 64-bitowa)](~/cross-platform/macios/unified/index.md) aplikacji platformy Xamarin.iOS.
Kliknij prawym przyciskiem myszy na projekt aplikacji systemu iOS, a następnie przejdź do **Opcje > kompilacji > kompilacja systemu iOS > karta Zaawansowane** i upewnij się, że **obsługiwane architektury** dla telefonu iPhone sklepu z aplikacjami Konfiguracja obejmuje **ARM64** (np.) **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>Ten pakiet jest nieprawidłowy.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

Aplikacja dla systemu iOS nadrzędny musi mieć MinimumOSVersion "8.2" lub wcześniej.

### <a name="non-public-api-usage"></a>Użycie interfejsu API niepubliczne

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Upewnij się, że używasz najnowszej wersji narzędzia Xcode i Xamarin.
Twój kod nie mają do dowolnych interfejsów API bez publicznego.

### <a name="build-error-mt5309"></a>Tworzenie MT5309 błąd

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

Ten błąd jest prawdopodobnie wynik Twojego o zmieniono jego nazwę instalację środowiska Xcode z **Xcode.app**. Na przykład ten błąd wystąpi, jeśli zmienisz instalacji **XCode 6.2.app**.



## <a name="related-links"></a>Linki pokrewne

- [Przewodnik przesyłania WatchKit firmy Apple](https://developer.apple.com/app-store/watch/)
