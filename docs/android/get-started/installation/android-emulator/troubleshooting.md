---
title: Rozwiązywanie problemów z emulatora systemu android
description: W tym artykule wyjaśniono, jak zdiagnozować i rozwiązać problemy, które mogą wystąpić w przypadku korzystania z emulatora systemu Android.
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 1d13a3dae509fea4a2e955c4ad206a81a57e75ed
ms.sourcegitcommit: 6433b424410a850f504e0f934bbb5baf8f093e49
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067337"
---
# <a name="android-emulator-troubleshooting"></a>Rozwiązywanie problemów z emulatora systemu android

_W tym artykule najbardziej typowe komunikaty ostrzegawcze i problemy występujące podczas konfigurowania i uruchamiania emulatora systemu Android zostały opisane, wraz z rozwiązania i wskazówki._

<a name="perfwarn" />

## <a name="performance-warnings"></a>Wydajność — Ostrzeżenia

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Począwszy od programu Visual Studio 2017 w wersji 15.4, okno dialogowe z ostrzeżeniem wydajności może być wyświetlany w przypadku wdrażania aplikacji w emulatorze systemu Android. Poniżej opisano te okna ostrzeżeń.

### <a name="computer-does-not-contain-an-intel-procesor"></a>Komputer nie zawiera Procesor Intel

![Komputer nie zawiera z procesorem Intel](troubleshooting-images/01-no-intel-processor.png)

Po wyświetleniu tego okna dialogowego na komputerze nie ma z procesorem Intel, co jest niezbędne do przyspieszenia emulatora Android SDK. Jeśli komputer nie ma na procesorze Intel, zaleca się używanie fizycznego urządzenia z systemem Android do tworzenia aplikacji.

### <a name="hyper-v-is-installed-or-active"></a>Funkcji Hyper-V jest zainstalowana lub jest aktywny

![Funkcji Hyper-V jest zainstalowana lub jest aktywny](troubleshooting-images/02-hyper-v-active.png)

Gdy to okno dialogowe jest wyświetlane, funkcji Hyper-V jest zainstalowana lub jest aktywny i musi zostać wyłączona. [Wyłączanie funkcji Hyper-V](#disable-hyperv) wyjaśnia, jak rozwiązać ten problem.

### <a name="haxm-is-not-installed"></a>Technologia HAXM jest niezainstalowany

![Technologia HAXM nie jest zainstalowana.](troubleshooting-images/03-haxm-not-installed.png)

To okno dialogowe wskazuje, czy komputer ma procesor Intel, funkcji Hyper-V jest wyłączona, ale technologia HAXM nie jest zainstalowana.
[Trwa instalowanie aparatu HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) w tym artykule opisano kroki wymagane do zainstalowania aparatu HAXM.

### <a name="haxm-process-not-running"></a>Proces aparatu HAXM nie jest uruchomiona

![Proces aparatu HAXM nie jest uruchomiony](troubleshooting-images/04-haxm-process-not-running.png)

To okno dialogowe jest wyświetlane, gdy komputer ma procesor Intel, funkcji Hyper-V jest wyłączona, Intel HAXM jest zainstalowany, ale proces aparatu HAXM nie jest uruchomiony. Aby rozwiązać ten problem, Otwórz okno wiersza polecenia i wpisz następujące polecenie:

```cmd
sc query intelhaxm
```

Jeśli jest uruchomiony proces aparatu HAXM, powinny pojawić się dane wyjściowe podobne do następujących:

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

Jeśli `STATE` nie jest ustawiony na `RUNNING`, zobacz [sposób użycia sprzętu Intel Accelerated menedżera wykonywania](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) Aby rozwiązać ten problem.


### <a name="other-failures"></a>Inne błędy

![Inne błędy](troubleshooting-images/05-other-failure.png)

To okno dialogowe jest wyświetlane, gdy komputer ma procesor Intel, funkcji Hyper-V jest wyłączona, Intel HAXM jest zainstalowany, jest uruchomiony proces aparatu HAXM, ale emulator uruchomienie nie powiedzie się z nieznanej przyczyny.
Aby rozwiązać ten problem, zobacz [sposób użycia sprzętu Intel Accelerated menedżera wykonywania](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Wyłączanie ostrzeżeń dotyczących wydajności

Jeśli użytkownik nie ma być wyświetlany ostrzeżeń dotyczących wydajności, można je wyłączyć. W programie Visual Studio, kliknij przycisk **Narzędzia > Opcje > Xamarin > Ustawienia systemu Android** i Wyłącz **Ostrzegaj, jeśli przyspieszanie AVD nie jest obsługiwane (HAXM)** opcji:

[![Wyłączanie ostrzeżenia przyspieszanie AVD](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Począwszy od programu Visual Studio dla komputerów Mac w wersji 7.2 (kompilacja 559), okno dialogowe z ostrzeżeniem wydajności może być wyświetlany w przypadku wdrażania aplikacji w emulatorze systemu Android. Poniżej opisano te okna ostrzeżeń.

### <a name="haxm-is-not-installed"></a>Technologia HAXM jest niezainstalowany

![Technologia HAXM nie jest zainstalowana.](troubleshooting-images/03-haxm-not-installed.png)

To okno dialogowe wskazuje, że technologia HAXM nie jest zainstalowany.
[Trwa instalowanie aparatu HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) w tym artykule opisano kroki wymagane do zainstalowania aparatu HAXM.

### <a name="haxm-process-not-running"></a>Proces aparatu HAXM nie jest uruchomiona

![Proces aparatu HAXM nie jest uruchomiony](troubleshooting-images/04-haxm-process-not-running.png)

To okno dialogowe jest wyświetlane, gdy proces aparatu HAXM nie jest uruchomiony. Aby uzyskać szczegółowe informacje rozwiązać ten problem, zobacz [sposób użycia sprzętu Intel Accelerated menedżera wykonywania](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) Aby rozwiązać ten problem.

### <a name="other-failures"></a>Inne błędy

![Inne błędy](troubleshooting-images/05-other-failure.png)

To okno dialogowe jest wyświetlane, gdy emulator uruchomienie nie powiedzie się z nieznanej przyczyny. Aby rozwiązać ten problem, zobacz [sposób użycia sprzętu Intel Accelerated menedżera wykonywania](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) Aby rozwiązać ten problem.

-----

## <a name="deployment-issues"></a>Problemy z wdrażaniem

Jeśli wystąpi błąd o błąd instalacji pliku APK na emulator lub niepowodzenie uruchomienia mostka debugowania systemu Android (**adb**), sprawdź, czy zestaw SDK systemu Android może połączyć się z emulatora. Aby to zrobić, wykonaj następujące kroki:

1. Uruchom emulator w **Menedżera urządzeń Android** (Wybierz urządzenie wirtualne, a następnie kliknij przycisk **Start**).

2. Otwórz wiersz polecenia i przejdź do folderu, w którym **adb** jest zainstalowany. Na przykład na Windows, może to być na: **C:\\Program Files (x86)\\Android\\zestawu sdk systemu android\\narzędzi platformy\\adb.exe**.

3. Wpisz następujące polecenie:

   ```shell
   adb devices
   ```

4. Jeśli emulator jest dostępny z zestawu SDK systemu Android, emulator powinna zostać wyświetlona na liście podłączonych urządzeń. Na przykład:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Jeśli emulator nie ma na tej liście, uruchom **Menedżer zestawów SDK**, zastosuj wszystkie aktualizacje, a następnie spróbuj ponownie uruchomić emulator.


## <a name="haxm-issues"></a>Problemy z aparatu HAXM

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Jeśli Emulator systemu Android nie zostanie uruchomiona poprawnie, jest to często spowodowane przez problemy z technologią HAXM. Problemy z technologii haxm i umożliwiający często są wynikiem powoduje konflikt z innych technologii wirtualizacji, ustawienia niepoprawne lub nieaktualne sterownika aparatu HAXM.

<a name="virt-conflicts" />

### <a name="haxm-virtualization-conflicts"></a>Konflikty wirtualizacji aparatu HAXM

Aparat HAXM, mogą powodować konflikt z innymi technologiami, korzystających z wirtualizacji, takich jak oprogramowanie antywirusowe, funkcji Hyper-V i funkcja Windows Device Guard:

- **Funkcji Hyper-V** &ndash; korzystania z wersji Windows przed **Windows update 10 kwietnia 2018 r. (kompilacja 1803)** i funkcji Hyper-V jest włączona, postępuj zgodnie z instrukcjami w [wyłączenie funkcji Hyper-V](#disable-hyperv).

- **Funkcja Device Guard** &ndash; funkcji Device Guard i Credential Guard może uniemożliwić funkcji Hyper-V jest wyłączona na komputerach Windows. Aby wyłączyć funkcję Device Guard i Credential Guard, zobacz [wyłączenie funkcji Device Guard](#disable-devguard).

- **Oprogramowanie antywirusowe** &ndash; korzystający z oprogramowania antywirusowego, który używa wirtualizacji sprzętowej (na przykład Avast), wyłączyć lub odinstalować tego oprogramowania, ponowne uruchomienie komputera i spróbuj ponownie emulatora Android SDK.


### <a name="incorrect-bios-settings"></a>Ustawienia systemu BIOS niepoprawne

Korzystając z aparatu HAXM na komputera z systemem Windows, aparatu HAXM nie będzie działać, o ile nie włączono virtualization technology (Intel VT-x) w systemie BIOS. Jeśli VT-x jest wyłączona, otrzymasz błąd podobny do następującego podczas próby uruchomienia emulatora systemu Android:

**Ten komputer spełnia wymagania dotyczące aparatu HAXM, ale technologią Intel Virtualization Technology (VT-x) nie jest włączona.**

Aby rozwiązać ten problem, przeprowadź rozruch komputera w systemie BIOS, Włącz VT-x i SLAT (drugi poziom Translacja), a następnie ponowne uruchomienie komputera do Windows.

<a name="disable-hyperv" />

### <a name="disabling-hyper-v"></a>Wyłączanie funkcji Hyper-V

Jeśli używasz wersji systemu Windows przed **systemu Windows 10 kwietnia 2018 r aktualizacji (kompilacja 1803)** i funkcji Hyper-V jest włączona, należy wyłączyć funkcję Hyper-V i ponownie uruchomić komputer, aby zainstalować i korzystać z aparatu HAXM. Jeśli używasz **systemu Windows 10 kwietnia 2018 aktualizacja (kompilacja 1803)** lub nowszej, wersji emulatora systemu Android 27.2.7 lub później użyć funkcji Hyper-V (zamiast HAXM) dla przyspieszenia sprzętowego, więc nie można wyłączyć funkcji Hyper-V.

Można wyłączyć funkcji Hyper-V, z poziomu Panelu sterowania, wykonaj następujące czynności:

1. W polu wyszukiwania Windows wprowadź **programów i** kliknięcie **programy i funkcje** wynik wyszukiwania.

2. W Panelu sterowania **programy i funkcje** okno dialogowe, kliknij przycisk **Windows Włącz lub wyłącz funkcje**:

    ![Włącz lub wyłącz funkcje Windows](troubleshooting-images/win/07-turn-windows-features.png)

3. Usuń zaznaczenie pola wyboru **funkcji Hyper-V** i ponowne uruchomienie komputera:

    ![Wyłączanie funkcji Hyper-V w oknie dialogowym funkcji Windows](troubleshooting-images/win/08-uncheck-hyper-v.png)

Alternatywnie służy następujące polecenie cmdlet programu Powershell do wyłączenia funkcji Hyper-V:

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Aparat Intel HAXM i Microsoft Hyper-V nie mogą być jednocześnie aktywne w tym samym czasie. Niestety nie istnieje żaden sposób przełączania między funkcją Hyper-V i technologii haxm i umożliwiający bez ponownego uruchamiania komputera. 

W niektórych przypadkach wykonując powyższe kroki nie powiedzie się po włączeniu funkcji Device Guard i Credential Guard, wyłączania funkcji Hyper-V. Jeśli nie możesz wyłączyć funkcję Hyper-V (lub wydaje się być wyłączona, ale instalacja aparatu HAXM nadal kończy się niepowodzeniem), wykonaj kroki w następnej sekcji, aby wyłączyć funkcję Device Guard i Credential Guard.

<a name="disable-devguard" />

### <a name="disabling-device-guard"></a>Wyłączanie funkcji Device Guard

Funkcja Device Guard i Credential Guard można zapobiec funkcji Hyper-V jest wyłączona na komputerach Windows. Często jest to problem dla maszyn przyłączone do domeny, które są konfigurowane i kontrolowane przez organizacji będącej właścicielem.
W systemie Windows 10, wykonaj następujące kroki, aby sprawdzić, czy **funkcji Device Guard** działa:

1. W **Windows Search**, typ **informacje o systemie** można uruchomić **informacje o systemie** aplikacji.

2. W **Podsumowanie systemu**, wygląd, aby sprawdzić, czy **zabezpieczenia oparte na wirtualizacji Guard urządzenia** istnieje i znajduje się w **systemem** stanu:

   [![Device Guard jest obecna i uruchomiona](troubleshooting-images/win/09-device-guard-sml.png)](troubleshooting-images/win/09-device-guard.png#lightbox)

Jeśli funkcja Device Guard jest włączona, umożliwia ją wyłączyć następujące czynności:

1. Upewnij się, że **funkcji Hyper-V** jest wyłączona (w obszarze **Włącz lub wyłącz funkcje Windows**) zgodnie z opisem w poprzedniej sekcji.

2. Wpisz w polu Windows Search **gpedit** i wybierz **edycji zasad grupy** wynik wyszukiwania. Spowoduje to uruchomienie **Edytora lokalnych zasad grupy**.

3. W **Edytora lokalnych zasad grupy**, przejdź do **Konfiguracja komputera > Szablony administracyjne > System > Device Guard**:

   [![Funkcja Device Guard w Edytorze lokalnych zasad grupy](troubleshooting-images/win/10-group-policy-editor-sml.png)](troubleshooting-images/win/10-group-policy-editor.png#lightbox)

4. Zmiana **włączyć na zabezpieczenia oparte na wirtualizacji** do **wyłączone** (jak pokazano powyżej) i zamknąć okno **Edytora lokalnych zasad grupy**.

5. Wpisz w polu Windows Search **cmd**. Gdy **polecenia** pojawia się w wynikach wyszukiwania, kliknij prawym przyciskiem myszy **wiersza polecenia** i wybierz **Uruchom jako Administrator**.

6. Skopiuj i wklej następujące polecenia w oknie wiersza polecenia (Jeśli dysk **Z:** w użyć, wybierz na nieużywanej literze dysku do użycia zamiast kodu):

        mountvol Z: /s
        copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
        bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
        bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
        mountvol Z: /d

7. Uruchom ponownie komputer. Na ekranie rozruchowy powinien zostać wyświetlony monit, jak pokazano poniżej:

   **Czy chcesz wyłączyć Credential Guard**

   Naciśnij klawisz wskazany wyłączyć Credential Guard, ponieważ zostanie wyświetlony monit.

8. Po ponownym uruchomieniu komputera, sprawdź ponownie, aby upewnić się, że funkcja Hyper-V jest wyłączona (zgodnie z opisem w poprzednich krokach).

Jeśli nadal funkcji Hyper-V nie zostanie wyłączony, zasad na komputerze przyłączonym do domeny może uniemożliwić wyłączenie funkcji Device Guard lub Credential Guard. W takim przypadku możesz zażądać zwolnienia z administratora domeny, aby możliwe było zrezygnować z Credential Guard. Alternatywnie można użyć komputera, który nie jest przyłączone do domeny do użycia aparatu HAXM.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Jeśli Emulator systemu Android nie zostanie uruchomiona poprawnie, jest to często spowodowane przez problemy z technologią HAXM. Problemy z technologii haxm i umożliwiający często są wynikiem powoduje konflikt z innych technologii wirtualizacji, ustawienia niepoprawne lub nieaktualne sterownika aparatu HAXM. Spróbuj ponownie zainstalować sterownika aparatu HAXM, wykonując kroki szczegółowo opisane w [Instalowanie aparatu HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----

