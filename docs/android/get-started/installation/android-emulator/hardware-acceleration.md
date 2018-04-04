---
title: Przyspieszanie sprzętowe emulatora systemu android
description: Jak przygotować komputer do maksymalnej wydajności emulatora Android SDK
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/22/2017
ms.openlocfilehash: f074bca7571188b14a36bd4e6c59a6fdf8df9339
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Przyspieszanie sprzętowe emulatora systemu android

Ponieważ jest zbyt wolno bez przyspieszenia sprzętowego, Intel w emulatorze systemu Android SDK HAXM (menedżera wykonywania przyspieszony sprzętu) jest zalecanym sposobem znacząco poprawić wydajność emulatora Android SDK.


## <a name="haxm-overview"></a>Omówienie HAXM

HAXM jest aparatem Wirtualizacja sprzętowa (funkcja hypervisor), który używa Intel Virtualization Technology (VT) w celu przyspieszenia emulacji aplikacji systemu Android na komputerze hosta. W połączeniu z systemem Android x86 emulatora obrazy, pod warunkiem Intel i oficjalna Android SDK Manager HAXM umożliwia szybsze emulacji systemu Android w systemach z obsługą VT. Jeśli tworzysz maszynę procesor CPU Intel VT funkcję, można skorzystać z HAXM aby znacznie przyspieszyć emulatora Android SDK (Jeśli nie masz pewności, czy Procesora obsługuje VT, zobacz [ustalić, czy Twoje procesora obsługuje Intel Technologii wirtualizacji](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

Emulatora Android SDK automatycznie korzysta z HAXM gdy jest ona dostępna. Po wybraniu **x86**— na podstawie urządzenia wirtualnego (zgodnie z opisem w [konfiguracji i używania](~/android/deploy-test/debugging/android-sdk-emulator/index.md)), że urządzenie wirtualne użyje HAXM przyspieszanie sprzętowe. Przed użyciem emulatora Android SDK po raz pierwszy, jest dobrym rozwiązaniem, aby sprawdzić, czy HAXM jest zainstalowany i dostępny dla emulatora Android SDK.

> [!NOTE]
> Nie można uruchomić HAXM na maszynie wirtualnej.


## <a name="verifying-haxm-installation"></a>Weryfikowanie instalacji HAXM

Można sprawdzić, czy HAXM jest dostępna, wyświetlając **uruchamianie emulatora systemu Android** okno podczas uruchamiania emulatora. Aby uruchomić emulatora Android SDK, wykonaj następujące czynności:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uruchom Menedżera emulatora systemu Android, klikając **Narzędzia > Android > Android Emulator Manager**:

    [![Lokalizacja elementu menu Menedżera emulatora systemu android](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Jeśli widzisz **ostrzeżenie wydajności** okno dialogowe podobne do następujących, a następnie HAXM jeszcze nie jest zainstalowane lub prawidłowo skonfigurowane na tym komputerze:

    ![Okno dialogowe z ostrzeżeniem wydajności HAXM nie jest gotowy](hardware-acceleration-images/win/11-perf-warn.png)

   Jeśli **ostrzeżenie wydajności** jest wyświetlane okno dialogowe następująco, zobacz [ostrzeżeń dotyczących wydajności](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) Aby zidentyfikować przyczynę i rozwiązać pierwotny problem.

3. Wybierz **x86** obrazu (na przykład **Visual Studio\_android 23\_x86\_phone**), kliknij przycisk **Start**, następnie kliknij przycisk  **Uruchamianie**:

    ![Uruchamianie emulatora Android SDK z domyślnego obrazu urządzenia wirtualnego](hardware-acceleration-images/win/02-start-default-avd.png)

4. Obserwuj **uruchamianie emulatora systemu Android** okno dialogowe podczas uruchamiania emulatora. Jeśli HAXM jest zainstalowany, zostanie wyświetlony komunikat **HAX działa i emulatora działa w trybie szybkiego virt** opisane w tym zrzut ekranu:

    ![HAXM jest wyświetlane jako dostępne w oknie dialogowym Uruchamianie emulatora systemu Android](hardware-acceleration-images/win/03-haxm-detected.png)

   Jeśli ten komunikat nie jest wyświetlony, HAXM prawdopodobnie nie jest zainstalowany. Na przykład poniżej przedstawiono zrzut ekranu komunikatu, że zostaną wyświetlone Jeśli HAXM nie jest dostępny:

    ![HAXM nie jest dostępny na tym komputerze](hardware-acceleration-images/win/04-haxm-error.png)

   Jeśli HAXM nie jest dostępny na komputerze, wykonaj kroki w następnej sekcji, aby zainstalować HAXM.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom Menedżera emulatora systemu Android, klikając **Narzędzia > Google Emulator Manager**:

    [![Lokalizacja elementu menu Menedżera emulatora systemu android](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Jeśli widzisz **ostrzeżenie wydajności** okno dialogowe podobne do następujących, a następnie HAXM jeszcze nie jest zainstalowane lub prawidłowo skonfigurowane na tym komputerze:

    ![Okno dialogowe z ostrzeżeniem wydajności HAXM nie jest gotowy](hardware-acceleration-images/mac/04-avd-warning.png)

   Jeśli **ostrzeżenie wydajności** jest wyświetlane okno dialogowe następująco, zobacz [ostrzeżeń dotyczących wydajności](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) Aby zidentyfikować przyczynę i rozwiązać pierwotny problem.

3. Wybierz **x86** obrazu (na przykład **Android\_akcelerowanego\_x86**), kliknij przycisk **Start**, następnie kliknij przycisk **uruchamianie**:

    [![Uruchamianie emulatora Android SDK z domyślnego obrazu urządzenia wirtualnego](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. Obserwuj **uruchamianie emulatora systemu Android** okno dialogowe podczas uruchamiania emulatora. Jeśli HAXM jest zainstalowany, zostanie wyświetlony komunikat **HAX działa i emulatora działa w trybie szybkiego virt** opisane w tym zrzut ekranu:

    ![HAXM jest wyświetlane jako dostępne w oknie dialogowym Uruchamianie emulatora systemu Android](hardware-acceleration-images/mac/03-haxm-detected.png)

   Jeśli HAXM nie jest dostępny na komputerze (na przykład, jeśli zostanie wyświetlony komunikat o błędzie, takich jak _Sprawdź, czy Intel HAXM jest propertly zainstalowanych i można go użyć_), wykonaj kroki w następnej sekcji, aby zainstalować HAXM.


-----

<a name="install-haxm" />

## <a name="installing-haxm"></a>Instalowanie HAXM

Jeśli nie zostanie uruchomiony emulator, HAXM może być można zainstalować ręcznie. HAXM zainstalować pakiety dla systemów Windows i macOS są dostępne z [menedżera wykonywania przyspieszony sprzętu Intel](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) strony. Aby pobrać i zainstalować HAXM ręcznie, wykonaj następujące kroki:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Z witryny sieci Web firmy Intel, Pobierz najnowszą [HAXM wirtualizacji aparat](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) Instalatora dla systemu Windows. Zaletą pobrać Instalatora HAXM bezpośrednio z witryny sieci Web Intel jest to, że można mieć pewność, korzystania z najnowszej wersji.

   Alternatywnie za pomocą Menedżera zestawu SDK można pobrać Instalatora HAXM (w Menedżerze zestawu SDK, kliknij **Narzędzia > Dodatki > akceleratora Emulator Intel x86 (Instalator HAXM)**). Zestaw SDK systemu Android zwykle pobiera Instalator HAXM w następującej lokalizacji:

   **C:\\Program Files (x86)\\Android\\zestawu sdk systemu android\\dodatki\\intel\\sprzętu\_akcelerowanego\_wykonywania\_Manager**

   Należy pamiętać, że Menedżer zestawu SDK nie instaluje HAXM jedynie pobiera Instalator HAXM do powyższej lokalizacji; nadal trzeba uruchomić go ręcznie.

2. Uruchom **intelhaxm android.exe** można uruchomić Instalator HAXM. Zaakceptuj wartości domyślne w oknach dialogowych Instalatora:

   ![Okno Ustawienia przyspieszony menedżera wykonywania programu Intel sprzętu](hardware-acceleration-images/win/05-haxm-installer.png)

Jeśli zostanie wyświetlony następujący okna dialogowego błędu (_ten komputer nie obsługuje technologią Intel Virtualization Technology (VT-x) lub jest on wyłącznie używany przez funkcję Hyper-V_), a następnie funkcji Hyper-V musi być wyłączona, aby można było zainstalować HAXM:

![Nie można zainstalować HAXM z powodu konfliktu funkcji Hyper-V](hardware-acceleration-images/win/06-cant-install-haxm.png)

W następnej sekcji objaśniono sposób wyłączania funkcji Hyper-V.

<a name="disable-hyperv" />

## <a name="disabling-hyper-v"></a>Wyłączanie funkcji Hyper-V

Jeśli używasz systemu Windows z włączoną funkcją Hyper-V, należy ją wyłączyć i uruchom ponownie komputer, aby zainstalować i używać HAXM. Można wyłączyć funkcji Hyper-V, z Panelu sterowania, wykonaj następujące czynności:

1. W polu wyszukiwania systemu Windows wprowadź **programy i** kliknięcie **programy i funkcje** wynik wyszukiwania.

2. W Panelu sterowania **programy i funkcje** okna dialogowego, kliknij przycisk **Włącz lub wyłącz funkcje systemu Windows**:

    ![Włącz lub wyłącz funkcje systemu Windows](hardware-acceleration-images/win/07-turn-windows-features.png)

3. Usuń zaznaczenie pola wyboru **funkcji Hyper-V** i ponowne uruchomienie komputera:

    ![Wyłączanie funkcji Hyper-V w oknie dialogowym funkcje systemu Windows](hardware-acceleration-images/win/08-uncheck-hyper-v.png)

Aby wyłączyć funkcji Hyper-v można użyć następującego polecenia cmdlet programu Powershell

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM i Microsoft Hyper-V nie może być jednocześnie aktywne w tym samym czasie. Niestety nie jest obecnie nie można przełączać się między funkcją Hyper-V i HAXM bez ponownego uruchamiania komputera. Jeśli chcesz użyć [programu Visual Studio Emulator for Android](~/android/deploy-test/debugging/visual-studio-android-emulator.md) (która jest zależna od funkcji Hyper-V), będzie mógł użyć emulatora Android SDK bez ponownego uruchamiania komputera. Sposób użycia funkcji Hyper-V jak również HAXM jest utworzenie instalacji wielokrotnych zgodnie z objaśnieniem w [tworzenia nie funkcji hypervisor wpisu](https://blogs.msdn.microsoft.com/virtual_pc_guy/2008/04/14/creating-a-no-hypervisor-boot-entry/).

W niektórych przypadkach przy użyciu powyższych kroków nie powiedzie się w wyłączenie funkcji Hyper-V, po włączeniu ochrony urządzeń i ochrona poświadczeń. Jeśli nie można wyłączyć funkcji Hyper-V (lub wydaje się być wyłączona, ale nadal niepowodzenia instalacji HAXM), wykonaj kroki w następnej sekcji, aby wyłączyć urządzenie zabezpieczenia i ochrona poświadczeń.

<a name="disable-devguard" />

## <a name="disabling-device-guard"></a>Wyłączanie ochrony urządzeń

Ochrona urządzeń i ochrona poświadczeń mogą uniemożliwić funkcji Hyper-V jest wyłączona na komputerach z systemem Windows. Często jest to problem maszyn przyłączonych do domeny, które są konfigurowane i kontrolowane przez jego organizację.
W systemie Windows 10, wykonaj następujące kroki, aby sprawdzić, czy **ochrony urządzeń** działa:

1. W **Windows Search**, typ **informacje o systemie** uruchomić **informacje o systemie** aplikacji.

2. W **Podsumowanie systemu**, wyglądu, aby sprawdzić, czy **zabezpieczenia oparte na urządzeniu zabezpieczenia wirtualizacji** jest istnieje i jest w **systemem** stanu:

   [![Ochrona urządzeń jest obecna i uruchomiona](hardware-acceleration-images/win/09-device-guard-sml.png)](hardware-acceleration-images/win/09-device-guard.png#lightbox)

Jeśli jest włączona ochrona urządzeń, wykonaj następujące kroki, aby ją wyłączyć:

1. Upewnij się, że **funkcji Hyper-V** jest wyłączone (w obszarze **Włącz lub wyłącz funkcje systemu Windows**) zgodnie z opisem w poprzedniej sekcji.

2. W polu wyszukiwania systemu Windows wpisz **gpedit** i wybierz **edycji zasad grupy** wynik wyszukiwania. Spowoduje to uruchomienie **Edytora lokalnych zasad grupy**.

3. W **Edytora lokalnych zasad grupy**, przejdź do **Konfiguracja komputera > Szablony administracyjne > System > ochrony urządzeń**:

   [![Ochrona urządzeń w Edytorze lokalnych zasad grupy](hardware-acceleration-images/win/10-group-policy-editor-sml.png)](hardware-acceleration-images/win/10-group-policy-editor.png#lightbox)

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

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Z witryny sieci Web firmy Intel, Pobierz najnowszą [HAXM wirtualizacji aparat](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) Instalator macOS.

2. Uruchom Instalatora HAXM. Zaakceptuj wartości domyślne w oknach dialogowych Instalatora:

   [![Okno Ustawienia przyspieszony menedżera wykonywania programu Intel sprzętu](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----
