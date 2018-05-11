---
title: Rozwiązywanie problemów z Emulator systemu Google Android
description: Jak zidentyfikować i rozwiązać problemy Emulator systemu Google Android
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/04/2018
ms.openlocfilehash: 001fc21a519a251715d24b43acfdd4251b5fbc91
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="google-android-emulator-troubleshooting"></a>Rozwiązywanie problemów z Emulator systemu Google Android

W tym artykule omówiono najbardziej typowe komunikaty ostrzegawcze i problemy z Emulator systemu Google Android (i ich rozwiązania).
 
<a name="perfwarn" />

## <a name="performance-warnings"></a>Wydajność — Ostrzeżenia

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Począwszy od programu Visual Studio 2017 wersji 15.4, okno dialogowe ostrzeżenia wydajności mogą być wyświetlane podczas wdrażania aplikacji na emulatorze systemu Android SDK. Poniżej opisano tych okien dialogowych ostrzeżenie.

### <a name="computer-does-not-contain-an-intel-procesor"></a>Komputer nie zawiera Procesor Intel

![Komputer nie zawiera z procesorem Intel](troubleshooting-images/01-no-intel-processor.png)

Jeśli to okno dialogowe jest wyświetlane, komputer nie ma z procesorem Intel, co jest niezbędne do przyspieszania emulatora Android SDK. Jeśli komputer nie ma z procesorem Intel, zaleca się używanie fizycznej urządzenia z systemem Android do tworzenia aplikacji.

### <a name="hyper-v-is-installed-or-active"></a>Funkcja Hyper-V jest zainstalowana lub jest aktywny

![Funkcja Hyper-V jest zainstalowana lub jest aktywny](troubleshooting-images/02-hyper-v-active.png)

Jeśli to okno dialogowe jest wyświetlane, funkcji Hyper-V jest zainstalowana lub jest aktywny i musi być wyłączona. [Wyłączanie funkcji Hyper-V](#disable-hyperv) wyjaśniono, jak rozwiązać ten problem.

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

Począwszy od programu Visual Studio dla komputerów Mac kompilacji 7.2 (kompilacja 559), okno dialogowe ostrzeżenia wydajności mogą być wyświetlane podczas wdrażania aplikacji na emulatorze systemu Android firmy Google. Poniżej opisano tych okien dialogowych ostrzeżenie.

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

Wiele typowych problemów Emulator systemu Google Android można rozwiązać przez konfigurowanie zmiany do komputera lub zainstalowanie dodatkowego oprogramowania. W poniższych sekcjach opisano te problemy i dostarczanie rozwiązań.


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

Jeśli Emulator systemu Google Android nie zostanie prawidłowo uruchomiona, zazwyczaj jest to spowodowane przez problemy z HAXM. Problemy z HAXM są często wynik powoduje konflikt z innych technologii wirtualizacji, nieprawidłowe ustawienia lub nieaktualny sterownika HAXM.

<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>Konflikty wirtualizacji HAXM

HAXM mogą powodować konflikt z innymi technologiami używające funkcji wirtualizacji, takich jak funkcja Hyper-V, ochrona urządzeń z systemem Windows i oprogramowanie antywirusowe:

- **Funkcja Hyper-V** &ndash; Jeśli używasz systemu Windows z włączoną funkcją Hyper-V, postępuj zgodnie z instrukcjami [wyłączenie funkcji Hyper-V](#disable-hyperv).

- **Ochrona urządzeń** &ndash; urządzenia zabezpieczenia i ochrona poświadczeń można zapobiec funkcji Hyper-V jest wyłączona na komputerach z systemem Windows. Aby wyłączyć urządzenie zabezpieczenia i ochrona poświadczeń, zobacz [wyłączenie ochrony urządzeń](#disable-devguard).

- **Oprogramowanie antywirusowe** &ndash; Jeśli używasz oprogramowania antywirusowego, który używa wirtualizacji sprzętowej (na przykład Avast), wyłączyć lub odinstalować tego oprogramowania, ponowne uruchomienie komputera i spróbuj ponownie emulatora Android SDK.


#### <a name="incorrect-bios-settings"></a>Ustawienia systemu BIOS niepoprawne

Jeśli używasz HAXM na komputerach z systemem Windows, HAXM nie będzie działać, jeśli nie włączono virtualization technology (Intel VT-x) w systemie BIOS. Jeśli VT-x jest wyłączona, otrzymasz błąd podobny do następującego podczas próby uruchomienia Emulator systemu Google Android:

**Niniejszy komputer spełnia wymagania dotyczące HAXM, ale technologią Intel Virtualization Technology (VT-x) nie jest włączony.**

Aby rozwiązać ten problem, przeprowadź rozruch komputera w systemie BIOS, Włącz VT-x i SLAT (drugi poziom NAT), a następnie ponownie uruchom komputer ponownie w systemie Windows.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Jeśli Emulator systemu Google Android nie zostanie prawidłowo uruchomiona, zazwyczaj jest to spowodowane przez problemy z HAXM. Problemy z HAXM są często wynik powoduje konflikt z innych technologii wirtualizacji, nieprawidłowe ustawienia lub nieaktualny sterownika HAXM. Spróbuj ponownie zainstalować sterownik HAXM, wykonując kroki szczegółowo w [instalowanie HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

<a name="disable-hyperv" />

### <a name="disabling-hyper-v"></a>Wyłączanie funkcji Hyper-V

Jeśli używasz systemu Windows z włączoną funkcją Hyper-V, należy ją wyłączyć i uruchom ponownie komputer, aby zainstalować i używać HAXM. Można wyłączyć funkcji Hyper-V, z Panelu sterowania, wykonaj następujące czynności:

1. W polu wyszukiwania systemu Windows wprowadź **programy i** kliknięcie **programy i funkcje** wynik wyszukiwania.

2. W Panelu sterowania **programy i funkcje** okna dialogowego, kliknij przycisk **Włącz lub wyłącz funkcje systemu Windows**:

    ![Włącz lub wyłącz funkcje systemu Windows](troubleshooting-images/win/07-turn-windows-features.png)

3. Usuń zaznaczenie pola wyboru **funkcji Hyper-V** i ponowne uruchomienie komputera:

    ![Wyłączanie funkcji Hyper-V w oknie dialogowym funkcje systemu Windows](troubleshooting-images/win/08-uncheck-hyper-v.png)

Aby wyłączyć funkcji Hyper-v można użyć następującego polecenia cmdlet programu Powershell

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM i Microsoft Hyper-V nie może być jednocześnie aktywne w tym samym czasie. Niestety nie jest obecnie nie można przełączać się między funkcją Hyper-V i HAXM bez ponownego uruchamiania komputera. Jeśli chcesz użyć [programu Visual Studio Emulator for Android](~/android/deploy-test/debugging/visual-studio-android-emulator.md) (która jest zależna od funkcji Hyper-V), będzie mógł użyć emulatora Android SDK bez ponownego uruchamiania komputera. Sposób użycia funkcji Hyper-V jak również HAXM jest utworzenie instalacji wielokrotnych zgodnie z objaśnieniem w [tworzenia nie funkcji hypervisor wpisu](https://blogs.msdn.microsoft.com/virtual_pc_guy/2008/04/14/creating-a-no-hypervisor-boot-entry/).

W niektórych przypadkach przy użyciu powyższych kroków nie powiedzie się w wyłączenie funkcji Hyper-V, po włączeniu ochrony urządzeń i ochrona poświadczeń. Jeśli nie można wyłączyć funkcji Hyper-V (lub wydaje się być wyłączona, ale nadal niepowodzenia instalacji HAXM), wykonaj kroki w następnej sekcji, aby wyłączyć urządzenie zabezpieczenia i ochrona poświadczeń.

<a name="disable-devguard" />

#### <a name="disabling-device-guard"></a>Wyłączanie ochrony urządzeń

Ochrona urządzeń i ochrona poświadczeń mogą uniemożliwić funkcji Hyper-V jest wyłączona na komputerach z systemem Windows. Często jest to problem maszyn przyłączonych do domeny, które są konfigurowane i kontrolowane przez jego organizację.
W systemie Windows 10, wykonaj następujące kroki, aby sprawdzić, czy **ochrony urządzeń** działa:

1. W **Windows Search**, typ **informacje o systemie** uruchomić **informacje o systemie** aplikacji.

2. W **Podsumowanie systemu**, wyglądu, aby sprawdzić, czy **zabezpieczenia oparte na urządzeniu zabezpieczenia wirtualizacji** jest istnieje i jest w **systemem** stanu:

   [![Ochrona urządzeń jest obecna i uruchomiona](troubleshooting-images/win/09-device-guard-sml.png)](troubleshooting-images/win/09-device-guard.png#lightbox)

Jeśli jest włączona ochrona urządzeń, wykonaj następujące kroki, aby ją wyłączyć:

1. Upewnij się, że **funkcji Hyper-V** jest wyłączone (w obszarze **Włącz lub wyłącz funkcje systemu Windows**) zgodnie z opisem w poprzedniej sekcji.

2. W polu wyszukiwania systemu Windows wpisz **gpedit** i wybierz **edycji zasad grupy** wynik wyszukiwania. Spowoduje to uruchomienie **Edytora lokalnych zasad grupy**.

3. W **Edytora lokalnych zasad grupy**, przejdź do **Konfiguracja komputera > Szablony administracyjne > System > ochrony urządzeń**:

   [![Ochrona urządzeń w Edytorze lokalnych zasad grupy](troubleshooting-images/win/10-group-policy-editor-sml.png)](troubleshooting-images/win/10-group-policy-editor.png#lightbox)

4. Zmień **włączyć na zabezpieczenia wirtualizacji na podstawie** do **wyłączone** (jak pokazano powyżej) i zamknąć **Edytora lokalnych zasad grupy**.

5. W polu wyszukiwania systemu Windows wpisz **cmd**. Gdy **wiersza polecenia** pojawia się w wynikach wyszukiwania kliknij prawym przyciskiem myszy **wiersza polecenia** i wybierz **Uruchom jako Administrator**.

6. Skopiuj i wklej poniższe polecenia w oknie wiersza polecenia (Jeśli dysk **Z:** w użyć, wybierz na nieużywanej literze dysku zamiast tego użyć):

        mountvol Z: /s
        copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
        bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
        bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
        mountvol Z: /d

7. Uruchom ponownie komputer. Na ekranie rozruchu powinien zostać wyświetlony monit o podobne do poniższych:

   **Czy chcesz wyłączyć ochrona poświadczeń**

   Naciśnij klawisz wskazanych, aby wyłączyć Guard poświadczeń w.

8. Po ponownym uruchomieniu komputera, sprawdź ponownie, aby upewnić się, że funkcja Hyper-V jest wyłączona (co opisano w poprzednich krokach).

Jeśli nadal funkcji Hyper-V nie zostanie wyłączony, zasad na komputerze przyłączonym do domeny mogą uniemożliwiać wyłączenie ochrony urządzeń lub ochrona poświadczeń. W takim przypadku możesz poprosić o wyjątek od administratora domeny, można zrezygnować z ochrona poświadczeń. Alternatywnie można użyć komputera, który nie jest przyłączony do domeny do użycia HAXM.


# <a name="visual-studiotabvsmac"></a>[Visual Studio](#tab/vsmac)

Funkcji Hyper-V nie jest dostępna w OS X lub macOS.

-----

