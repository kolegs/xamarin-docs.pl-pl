---
title: "Rozwiązywanie problemów"
description: "Ten artykuł obejmuje znać problemy mogą wystąpić podczas pracy z obsługą systemu tvOS na platformie Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 278b9e782073a26dc04bac9418613ea4c09db445
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Rozwiązywanie problemów

_Ten artykuł obejmuje znać problemy mogą wystąpić podczas pracy z obsługą systemu tvOS na platformie Xamarin._

<a name="Known-Issues" />

## <a name="known-issues"></a>Znane problemy

Bieżącej wersji programu Xamarin na obsługę systemu tvOS ma następujące znane problemy:

- **Mono Framework** — Mono 4.3 Cryptography.ProtectedData nie powiedzie się odszyfrować dane z Mono 4.2. W związku z tym pakietów NuGet nie będzie można przywrócić z powodu błędu `Data unprotection failed` , gdy chronionego źródła NuGet jest skonfigurowany.
    - **Obejście** — w programie Visual Studio dla komputerów Mac, musisz dodać z powrotem dowolnego pakietu NuGet źródłem tego uwierzytelnianie hasła przed ponowne próby przywrócenia pakietów.
- **Visual Studio for Mac z F # dodatku** — błąd podczas tworzenia szablonu programu F # dla systemu Android w systemie Windows. To nadal powinny działać poprawnie na komputerach Mac.
- **Xamarin.Mac** — podczas uruchamiania projektu szablonu ujednoliconego Xamarin.Mac z elementem docelowym Framework ustawioną `Unsupported`, menu podręcznego `Could not connect to the debugger` mogą być wyświetlane.
    - **Potencjalne rozwiązania** — obniżyć wersji Mono framework dostępne w naszym stabilna kanału.
- **Xamarin Visual Studio & Xamarin.iOS** — w przypadku wdrażania aplikacji WatchKit w programie Visual studio, błąd `The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist` mogą być wyświetlane.

Raport wszelkie błędy, można znaleźć na [Bugzilla](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

## <a name="troubleshooting"></a>Rozwiązywanie problemów

W poniższych sekcjach wymieniono znane problemy występujące podczas korzystania z systemu tvOS 9 z Xamarin.tvOS i rozwiązanie tych problemów:

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>Nieprawidłowy plik wykonywalny — plik wykonywalny nie zawiera kodu bitowego

Podczas próby wysłania Xamarin.tvOS aplikacji do sklepu z aplikacjami firmy Apple TV, może być wyświetlony komunikat o błędzie w formie _"Nieprawidłowy plik wykonywalny — plik wykonywalny nie zawiera kodu bitowego"_.

Aby rozwiązać ten problem, wykonaj następujące czynności:

1. W programie Visual Studio for Mac, kliknij prawym przyciskiem myszy plik projektu Xamarin.tvOS w **Eksploratora rozwiązań** i wybierz **opcje**.
2. Wybierz **systemu tvOS kompilacji** i upewnij się, że znajdują się na **wersji** konfiguracji: 

    [ ![](troubleshooting-images/ts01.png "Wybierz opcje kompilacji systemu tvOS")](troubleshooting-images/ts01.png)
3. Dodaj `--bitcode=asmonly` do **mtouch dodatkowe argumenty** a następnie kliknij przycisk **OK** przycisku.
4. Skompiluj ponownie aplikację w **wersji** konfiguracji.

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>Sprawdzenia, czy Twojego systemu tvOS Bitcode zawiera aplikację

Upewnić się, że kompilowania aplikacji Xamarin.tvOS zawiera kodu bitowego, Otwórz aplikację terminala i wprowadź następujące informacje:

```csharp
otool -l /path/to/your/tv.app/tv
```

W danych wyjściowych Wyszukaj następujące czynności:

```csharp
Section
  sectname __bundle
   segname __LLVM
      addr 0x0000000100001000
      size 0x000000000000124f
    offset 4096
     align 2^0 (1)
    reloff 0
    nreloc 0
     flags 0x00000000
 reserved1 0
 reserved2 0
```

`addr` i `size` będą różne, ale inne pola powinny być identyczne.

Należy upewnić się, że trzecich statyczne (`.a`) bibliotek używasz zostały skompilowane względem systemu tvOS biblioteki (nie biblioteki z systemem iOS) i czy zawiera także informacje kodu bitowego.

Dla aplikacji lub bibliotek, które zawierają nieprawidłowy kodu bitowego `size` będzie większa niż jeden. Istnieje kilka sytuacji, gdzie biblioteki mogą zawierać znacznik kodu bitowego jeszcze zawiera nieprawidłowy kodu bitowego. Na przykład:

**Nieprawidłowy kodu bitowego**

```csharp
 $ otool -arch arm64 libLibrary.a | grep __bitcode -A 3
   sect name __bitcode
   segname __LLVM
      add 0x0000000000000670
      size 0x0000000000000001
```

**Nieprawidłowa kodu bitowego** 

```csharp
$ otool -l -arch arm64 libDownloadableAgent-tvos.a |grep __bitcode -A 3
   sectname __bitcode
   segname __LLVM
      addr 0x000000000001d2d0
      size 0x0000000000045440
```

Różnice w `size` między dwie biblioteki w tym przykładzie wymienionych uruchamia powyżej. Biblioteka musi zostać wygenerowany z kompilacji archiwum Xcode z kodu bitowego włączone (ustawienie Xcode `ENABLE_BITCODE`) jako rozwiązanie tego problemu rozmiar.

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Aplikacje, które zawierają tylko wycinek arm64 na liście UIRequiredDeviceCapabilities w pliku Info.plist musi mieć również "arm64"

Podczas przesyłania aplikacji do sklepu Apple TV aplikacji dla publikacji, może być błąd pojawia się w postaci:

_"Aplikacje, które zawierają tylko wycinek arm64 musi mieć również"arm64"na liście UIRequiredDeviceCapabilities w pliku Info.plist"_

W takim przypadku Edytowanie użytkownika `Info.plist` i upewnij się, że ma ona następujące klucze:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
    <string>arm64</string>
</array>
```

Ponownie skompilować aplikację dla wersji i ponownie prześlij do iTunes Connect.

### <a name="task-mtouch-execution----failed"></a>"MTouch"--nie można wykonać zadania

Jeśli korzystasz z 3 biblioteki firm (na przykład MonoGame) i Twojej wersji kompilacji nie powiodło się z szeregu długo komunikaty o błędach kończy się rozszerzeniem `Task "MTouch" execution -- FAILED`, spróbuj dodać `-gcc_flags="-framework OpenAL"` do Twojej **touch dodatkowe argumenty**:

[ ![](troubleshooting-images/mtouch01.png "Wykonywanie zadań MTouch")](troubleshooting-images/mtouch01.png)

Należy także uwzględnić `--bitcode=asmonly` w **touch dodatkowe argumenty**, ustawiono opcje konsolidatora **wszystkie łącza** i wykonaj czystą kompilacji.

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS-90471 error. Brak dużych ikon

Jeśli pojawi się komunikat w postaci "ITMS 90471 błąd. Dużych ikon brakuje"podczas próby wysłania Xamarin.tvOS aplikacji do sklepu Apple TV aplikacji dla wersji, należy wykonać następujące działania:

1. Upewnij się, wprowadzono zasoby dużych ikon w Twojej `Assets.car` plików, które zostały utworzone za pomocą [ikony aplikacji](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons) dokumentacji.
2. Upewnij się, że objęty `Assets.car` plik z [Praca z obrazów i ikon](~/ios/tvos/app-fundamentals/icons-images.md) dokumentacji w pakiecie użytkownika końcowego aplikacji.

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>Nieprawidłowy pakiet — aplikację, która obsługuje kontrolery gier muszą również obsługiwać Apple TV zdalnego

lub 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>Nieprawidłowy pakiet — Apple TV aplikacji za pomocą GameController framework muszą zawierać klucz GCSupportedGameControllers w pliku Info.plist aplikacji

Kontrolery gier może służyć do zwiększenia gry i zapewnienia w pewnym sensie immersyjną w grę. One mogą służyć do kontrolowania standardowy interfejs Apple TV, więc użytkownik nie ma na celu przełączania się między zdalnego i kontroler.

Jeśli przesyłasz Xamarin.tvOS aplikacji z obsługą kontrolera gier do sklepu Apple TV, a komunikat o błędzie jest pobierany w postaci:


_Odnaleźliśmy co najmniej jeden problem z Twojej ostatnie dostarczania "app name". Firmy zakończyło się pomyślnie, ale chcesz rozwiązać następujące problemy w sieci dostarczania dalej:_

_Nieprawidłowy pakiet — aplikację, która obsługuje kontrolery gier muszą również obsługiwać Apple TV zdalnego._

lub 

_Nieprawidłowy pakiet — Apple TV aplikacji za pomocą GameController framework muszą zawierać klucz GCSupportedGameControllers w pliku Info.plist aplikacji._

Rozwiązanie to dodanie obsługi zdalnego Siri (`GCMicroGamepad`) do swojej aplikacji `Info.plist` pliku. Profil kontrolera gry Micro został dodany przez firmę Apple do zdalnego Siri. Na przykład obejmują następujące klucze:

```xml
<key>GCSupportedGameControllers</key>  
  <array>  
    <dict>  
      <key>ProfileName</key>  
      <string>ExtendedGamepad</string>  
    </dict>  
    <dict>  
      <key>ProfileName</key>  
      <string>MicroGamepad</string>  
    </dict>  
  </array>  
<key>GCSupportsControllerUserInteraction</key>  
<true/>
```

> [!IMPORTANT]
> **Uwaga:** kontrolery gier Bluetooth są opcjonalne zakupu, które może uniemożliwić użytkownikom końcowym, aplikacja nie zmusza użytkownika do kupić. Jeśli aplikacja obsługuje kontrolery gier również musi obsługiwać zdalne Siri tak, aby gry jest użyteczny przez wszystkich użytkowników Apple TV.

Aby uzyskać więcej informacji, zobacz nasze [Praca z kontrolery gier](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers) sekcji naszych [Siri zdalnego oraz kontrolerów Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) dokumentacji.

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>Platforma docelowa niezgodne:. NetPortable, Version = 4.5, profil = Profile78

Podczas próby dołączenia przenośnej biblioteki klas (PCL) do projektu Xamarin.tvOS może otrzymać komunikat celu utworzenia:

_Platforma docelowa niezgodne:. NetPortable, Version = 4.5, profil = Profile78_

Aby rozwiązać ten problem, Dodaj plik XML o nazwie ` Xamarin.TVOS.xml` o następującej treści:

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

Do następującej ścieżki:

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

Należy pamiętać, że numer profilu w ścieżce musi być zgodna z liczbą profilu PCL.

Z tego pliku w miejscu można pomyślnie dodać do projektu Xamarin.tvOS pliku PCL.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
