---
title: "Rozwiązywanie problemów z emulatorem zestawu SDK systemu android"
description: "Jak zidentyfikować i rozwiązać problemy z emulatora Android SDK"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 486df3bbee3f8af511140e2d287f9f95571c7b3d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="android-sdk-emulator-troubleshooting"></a>Rozwiązywanie problemów z emulatorem zestawu SDK systemu android

W tym artykule omówiono najbardziej typowe komunikaty ostrzegawcze i problemy z emulatora Android SDK (i ich rozwiązania).
 
<a name="perfwarn" />

## <a name="performance-warnings"></a>Wydajność — Ostrzeżenia

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Począwszy od programu Visual Studio 2017 wersji 15.4, okno dialogowe ostrzeżenia wydajności mogą być wyświetlane podczas wdrażania aplikacji na emulatorze systemu Android SDK. Poniżej opisano tych okien dialogowych ostrzeżenie.

### <a name="computer-does-not-contain-an-intel-procesor"></a>Komputer nie zawiera Procesor Intel

![Komputer nie zawiera z procesorem Intel](troubleshooting-images/01-no-intel-processor.png)

Jeśli to okno dialogowe jest wyświetlane, komputer nie ma z procesorem Intel, co jest niezbędne do przyspieszania emulatora Android SDK. Jeśli komputer nie ma z procesorem Intel, zaleca się używanie fizycznej urządzenia z systemem Android do tworzenia aplikacji.

### <a name="hyper-v-is-installed-or-active"></a>Funkcja Hyper-V jest zainstalowana lub jest aktywny

![Funkcja Hyper-V jest zainstalowana lub jest aktywny](troubleshooting-images/02-hyper-v-active.png)

Jeśli to okno dialogowe jest wyświetlane, funkcji Hyper-V jest zainstalowana lub jest aktywny i musi być wyłączona. [Wyłączanie funkcji Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv) wyjaśniono, jak rozwiązać ten problem. 

### <a name="haxm-is-not-installed"></a>HAXM jest niezainstalowany

![HAXM nie jest zainstalowany.](troubleshooting-images/03-haxm-not-installed.png)

To okno dialogowe wskazuje, że komputer jest wyposażony w procesor Intel, funkcji Hyper-V jest wyłączona, ale HAXM nie jest zainstalowany.
[Instalowanie HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) opisano kroki dotyczące instalowania HAXM.

### <a name="haxm-process-not-running"></a>Proces HAXM nie działa

![Proces HAXM nie jest uruchomiony](troubleshooting-images/04-haxm-process-not-running.png)

To okno dialogowe jest wyświetlane, gdy komputer jest wyposażony w procesor Intel, funkcji Hyper-V jest wyłączona, Intel HAXM jest zainstalowany, ale proces HAXM nie jest uruchomiony. Aby rozwiązać ten problem, Otwórz okno wiersza polecenia, a następnie wprowadź następujące polecenie:

```cmd
sc query intelhaxm
```

Jeśli jest uruchomiony proces HAXM, powinny być widoczne dane wyjściowe podobne do następującego:

```cmd
SERVICE_NAME: intelhaxm
    TYPE               : 1  KERNEL_DRIVER
    STATE              : 4  RUNNING
                            (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
    WIN32_EXIT_CODE    : 0  (0x0)
    SERVICE_EXIT_CODE  : 0  (0x0)
    CHECKPOINT         : 0x0
    WAIT_HINT          : 0x0
```


Jeśli **stanu** nie jest ustawiony na **systemem**, zobacz [sposób użycia sprzętu Intel przyspieszony menedżera wykonywania](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) Aby rozwiązać ten problem.


### <a name="other-failures"></a>Inne błędy

![Inne błędy](troubleshooting-images/05-other-failure.png)

To okno dialogowe jest wyświetlane, gdy komputer jest wyposażony w procesor Intel, funkcji Hyper-V jest wyłączona, Intel HAXM jest zainstalowany, jest uruchomiony proces HAXM, ale emulator nie powiedzie się z nieznanej przyczyny.
Aby rozwiązać ten problem, zobacz [sposób użycia sprzętu Intel przyspieszony menedżera wykonywania](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Wyłączanie ostrzeżeń dotyczących wydajności

Jeśli użytkownik nie ma być wyświetlany ostrzeżeń dotyczących wydajności, można je wyłączyć. W programie Visual Studio, kliknij przycisk **Narzędzia > Opcje > Xamarin > Ustawienia systemu Android** i Wyłącz **Ostrzegaj, jeśli przyspieszenie AVD nie jest obsługiwane (HAXM)** opcji:

[![Wyłączanie AVD przyspieszenie ostrzeżenia](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Począwszy od programu Visual Studio dla komputerów Mac kompilacji 7.2 (kompilacja 559), okno dialogowe ostrzeżenia wydajności mogą być wyświetlane podczas wdrażania aplikacji na emulatorze systemu Android SDK. Poniżej opisano tych okien dialogowych ostrzeżenie.

### <a name="haxm-is-not-installed"></a>HAXM jest niezainstalowany

![HAXM nie jest zainstalowany.](troubleshooting-images/03-haxm-not-installed.png)

To okno dialogowe wskazuje, że HAXM nie jest zainstalowany.
[Instalowanie HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) opisano kroki dotyczące instalowania HAXM.

### <a name="haxm-process-not-running"></a>Proces HAXM nie działa

![Proces HAXM nie jest uruchomiony](troubleshooting-images/04-haxm-process-not-running.png)

To okno dialogowe jest wyświetlane, gdy proces HAXM nie jest uruchomiony. Aby uzyskać szczegółowe informacje rozwiązać ten problem, zobacz [sposób użycia sprzętu Intel przyspieszony menedżera wykonywania](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) Aby rozwiązać ten problem.

### <a name="other-failures"></a>Inne błędy

![Inne błędy](troubleshooting-images/05-other-failure.png)

To okno dialogowe jest wyświetlane, gdy emulator nie powiedzie się z nieznanej przyczyny. Aby rozwiązać ten problem, zobacz [sposób użycia sprzętu Intel przyspieszony menedżera wykonywania](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) Aby rozwiązać ten problem.

-----


## <a name="solutions-to-common-problems"></a>Rozwiązania typowych problemów

Konfigurowanie zmiany do komputera lub zainstalować dodatkowe oprogramowanie, można rozwiązać wiele typowych problemów emulatora Android SDK. W poniższych sekcjach opisano te problemy i dostarczanie rozwiązań.


### <a name="deployment-issues"></a>Problemy z wdrażaniem

Jeśli wystąpi błąd o błąd instalacji APK na emulatorze lub niepowodzenie uruchomienia mostka debugowania systemu Android (**adb**), sprawdź, czy zestaw SDK systemu Android nawiązać połączenie z emulatora. Aby to zrobić, wykonaj następujące kroki:

1. Uruchom emulator w **Android Virtual Device (AVD) Manager** (Wybierz urządzenie wirtualne, a następnie kliknij przycisk **Start**).

2. Otwórz wiersz polecenia i przejdź do folderu, gdzie **adb** jest zainstalowany. Na przykład w systemie Windows, może to być na: **C:\\Program Files (x86)\\Android\\zestawu sdk systemu android\\narzędzi platformy\\adb.exe**.

3. Wpisz następujące polecenie:

   ```shell
   adb devices
   ```

4. Jeśli emulator jest dostępny z zestawu SDK systemu Android, emulator powinny być wyświetlane na liście podłączonych urządzeń. Na przykład:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Jeśli emulator nie ma na liście, należy uruchomić **Android SDK Manager**, zastosuj wszystkie aktualizacje, a następnie spróbuj ponownie uruchomić emulatora.



### <a name="haxm-issues"></a>Problemy z HAXM

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Jeśli emulatora Android SDK nie zostanie prawidłowo uruchomiona, zazwyczaj jest to spowodowane przez problemy z HAXM. Problemy z HAXM są często wynik powoduje konflikt z innych technologii wirtualizacji, nieprawidłowe ustawienia lub nieaktualny sterownika HAXM.

<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>Konflikty wirtualizacji HAXM

HAXM mogą powodować konflikt z innymi technologiami używające funkcji wirtualizacji, takich jak funkcja Hyper-V, ochrona urządzeń z systemem Windows i oprogramowanie antywirusowe:

- **Funkcja Hyper-V** &ndash; Jeśli używasz systemu Windows z włączoną funkcją Hyper-V, postępuj zgodnie z instrukcjami [wyłączenie funkcji Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv).

- **Ochrona urządzeń** &ndash; urządzenia zabezpieczenia i ochrona poświadczeń można zapobiec funkcji Hyper-V jest wyłączona na komputerach z systemem Windows. Aby wyłączyć urządzenie zabezpieczenia i ochrona poświadczeń, zobacz [wyłączenie ochrony urządzeń](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-devguard).

- **Oprogramowanie antywirusowe** &ndash; Jeśli używasz oprogramowania antywirusowego, który używa wirtualizacji sprzętowej (na przykład Avast), wyłączyć lub odinstalować tego oprogramowania, ponowne uruchomienie komputera i spróbuj ponownie emulatora Android SDK.


#### <a name="incorrect-bios-settings"></a>Ustawienia systemu BIOS niepoprawne

Jeśli używasz HAXM na komputerach z systemem Windows, HAXM nie będzie działać, jeśli nie włączono virtualization technology (Intel VT-x) w systemie BIOS. Jeśli VT-x jest wyłączona, otrzymasz błąd podobny do następującego podczas próby uruchomienia emulatora Android SDK:

**Niniejszy komputer spełnia wymagania dotyczące HAXM, ale technologią Intel Virtualization Technology (VT-x) nie jest włączony.**

Aby rozwiązać ten problem, przeprowadź rozruch komputera w systemie BIOS, Włącz VT-x i SLAT (drugi poziom NAT), a następnie ponownie uruchom komputer ponownie w systemie Windows.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Jeśli emulatora Android SDK nie zostanie prawidłowo uruchomiona, zazwyczaj jest to spowodowane przez problemy z HAXM. Problemy z HAXM są często wynik powoduje konflikt z innych technologii wirtualizacji, nieprawidłowe ustawienia lub nieaktualny sterownika HAXM. Spróbuj ponownie zainstalować sterownik HAXM, wykonując kroki szczegółowo w [instalowanie HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----
