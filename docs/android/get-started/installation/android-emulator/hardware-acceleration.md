---
title: Przyspieszanie sprzętowe emulatora systemu android
description: Jak przygotować komputer do maksymalnej wydajności Emulator systemu Google Android
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/10/2018
ms.openlocfilehash: b5c20eb9f40bb4c4981d6b60b9fd4bc75fd29336
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Przyspieszanie sprzętowe emulatora systemu android

Emulator systemu Google Android jest zbyt niska bez przyspieszanie sprzętowe. Użytkownik może znacząco zwiększyć wydajność Emulator systemu Google Android przy użyciu emulatora specjalne hardward obrazów, sprzętem docelowej x86 i jeden z dwóch technologii wirtualizacji:

1. **Firmy Microsoft Hyper-V i platformy Hypervisor** &ndash; funkcji Hyper-V jest składnikiem wirtualizacji, który jest dostępny w systemie Windows 10, umożliwiającą uruchomionych systemów komputerowych wirtualnego na hoście fizycznym. Jest to technologii wirtualizacji zalecane przyspieszonego obrazów Emulator systemu Google Android. Aby dowiedzieć się więcej o funkcji Hyper-V, zapoznaj się [funkcję Hyper-V systemu Windows 10 przewodnik](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).
2. **Firmy Intel sprzętu przyspieszyć wykonywanie Manager (HAXM)** &ndash; jest aparatem wirtualizacji dla komputerów z systemem procesory Intel. Jest to aparat wirtualizacji zalecane dla deweloperów, którzy nie mogli użyć funkcji Hyper-V.

Android SDK Manager automatycznie wprowadzi Użyj sprzętowego przyspieszania, gdy jest ona dostępna jest uruchomiona obraz emulatora specjalnie z myślą o **x86**— na podstawie urządzenie wirtualne (zgodnie z opisem w [konfiguracji i użycia ](~/android/deploy-test/debugging/android-sdk-emulator/index.md)).

## <a name="hyper-v-overview"></a>Omówienie funkcji Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Obsługa funkcji Hyper-V jest obecnie w przeglądzie.

Deweloperzy, którzy korzystają z systemu Windows 10 (kwiecień 2018 aktualizacja) zdecydowanie zaleca się stosowanie firmy Microsoft Hyper-V. Visual Studio Tools dla platformy Xamarin ułatwiają deweloperom testowanie i debugowanie aplikacji platformy Xamarin.Android w sytuacjach, gdy urządzenia z systemem Android jest niedostępny lub niepraktyczne.

Aby rozpocząć używanie funkcji Hyper-V i Emulator systemu Google Android:

1. **Aktualizacja systemu Windows 10 kwietnia 2018 aktualizacji (kompilacja 1803)** &ndash; Aby sprawdzić, która wersja systemu Windows jest uruchomiona, kliknij Cortana pasek wyszukiwania i wpisz **o**. Wybierz **o Twoim komputerze** w wynikach wyszukiwania. Przewiń w dół **o** okna dialogowego, do **specyfikacje Windows** sekcji. **Wersji** powinien być co najmniej 1803:

    [![Specyfikacje systemu Windows](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Włączenia funkcji Hyper-V i platformy funkcji Hypervisor systemu Windows** &ndash; pasek w wyszukiwania Cortana, typ **Włącz lub wyłącz funkcje systemu Windows**.
   Przewiń w dół **funkcje systemu Windows** okna dialogowego i upewnij się, że **platformy funkcji Hypervisor systemu Windows** jest włączona.

    [![Funkcja Hyper-V i włączone platforma funkcji Hypervisor systemu Windows](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

    Może być konieczne ponowne uruchomienie systemu Windows po włączeniu funkcji Hyper-V i platformy funkcji Hypervisor systemu Windows.

3. **Zainstaluj [programu Visual Studio 15.8 Preview 1](https://aka.ms/hyperv-emulator-dl)**  &ndash; tej wersji programu Visual Studio zapewnia obsługę środowiska IDE uruchamianie Emulator systemu Google Android z obsługą funkcji Hyper-V.

4. **Zainstaluj pakiet emulator systemu Google Android 27.2.7 lub nowszej** &ndash; Aby zainstalować ten pakiet, przejdź do **Narzędzia > Android > Android SDK Manager** w programie Visual Studio. Wybierz **narzędzia** i upewnij się, składnik emulatora systemu Android jest co najmniej wersji 27.2.7.

    [![Okno dialogowe narzędzia i zestawy SDK systemu android](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Jeśli wersja emulatora systemu Android jest mniejsza niż 27.3.1, zastosuj krok dodatkowe obejście wyjaśniono w **znane problemy** (dalej).


### <a name="known-issues"></a>Znane problemy

-   Jeśli wersja emulatora jest co najmniej 27.2.7, ale mniej niż 27.3.1, poniższe obejście jest wymagany do użycia funkcji Hyper-V:
    1.  W **C:\\użytkowników\\_username_\\.android** folderu, Utwórz plik o nazwie **advancedFeatures.ini** w przeciwnym razie już istnieje.
    2.  Dodaj następujący wiersz do **advancedFeatures.ini**:
        ```
        WindowsHypervisorPlatform = on
        ```

-   Wydajność może się zmniejszyć, korzystając z określonych Intel i procesory AMD procesorów.

-   Android aplikacji może potrwać nietypowe ilość czasu potrzebna na załadowanie na wdrożenie.

-   Błąd dostępu do rozwiązanie MMIO sporadycznie może uniemożliwiać rozruchu w emulatorze systemu Android. Ponowne uruchamianie w emulatorze powinno rozwiązać ten problem.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Obsługa funkcji Hyper-V wymaga systemu Windows 10. Zobacz [wymagania dotyczące funkcji Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements) więcej szczegółów.

-----

## <a name="haxm-overview"></a>Omówienie HAXM

HAXM jest aparatem Wirtualizacja sprzętowa (funkcja hypervisor), który używa Intel Virtualization Technology (VT) w celu przyspieszenia emulacji aplikacji systemu Android na komputerze hosta. W połączeniu z systemem Android x86 emulatora obrazy, pod warunkiem Intel i oficjalna Android SDK Manager HAXM umożliwia szybsze emulacji systemu Android w systemach z obsługą VT. 

Jeśli tworzysz maszynę procesor CPU Intel VT funkcję, można skorzystać z HAXM aby znacznie przyspieszyć Emulator systemu Google Android (Jeśli nie masz pewności, czy Procesora obsługuje VT, zobacz [ustalić, czy Twoje procesora obsługuje Intel Technologii wirtualizacji](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Nie można uruchomić emulatora przyspieszony maszyny Wirtualnej w innej maszyny Wirtualnej, np. maszyna wirtualna hostowana przez VirtualBox, serwery VMWare lub Docker. Należy uruchomić emulator systemu Google Android [bezpośrednio na danym urządzeniu systemu](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Przed użyciem Emulator systemu Google Android po raz pierwszy, jest dobrym rozwiązaniem, aby sprawdzić, czy HAXM jest zainstalowany i dostępny dla Emulator systemu Google Android.

### <a name="verifying-haxm-installation"></a>Weryfikowanie instalacji HAXM

Można sprawdzić, czy HAXM jest dostępna, wyświetlając **uruchamianie emulatora systemu Android** okno podczas uruchamiania emulatora. Aby uruchomić Emulator systemu Google Android, wykonaj następujące czynności:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uruchom Menedżera emulatora systemu Android, klikając **Narzędzia > Android > Android Emulator Manager**:

    [![Lokalizacja elementu menu Menedżera emulatora systemu android](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Jeśli widzisz **ostrzeżenie wydajności** okno dialogowe podobne do następujących, a następnie HAXM jeszcze nie jest zainstalowane lub prawidłowo skonfigurowane na tym komputerze:

    ![Okno dialogowe z ostrzeżeniem wydajności HAXM nie jest gotowy](hardware-acceleration-images/win/11-perf-warn.png)

   Jeśli **ostrzeżenie wydajności** jest wyświetlane okno dialogowe następująco, zobacz [ostrzeżeń dotyczących wydajności](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) Aby zidentyfikować przyczynę i rozwiązać pierwotny problem.

3. Wybierz **x86** obrazu (na przykład **Visual Studio\_android 23\_x86\_phone**), kliknij przycisk **Start**, następnie kliknij przycisk  **Uruchamianie**:

    ![Począwszy od domyślnego obrazu urządzenia wirtualnego Emulator systemu Google Android](hardware-acceleration-images/win/02-start-default-avd.png)

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

    [![Począwszy od domyślnego obrazu urządzenia wirtualnego Emulator systemu Google Android](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. Obserwuj **uruchamianie emulatora systemu Android** okno dialogowe podczas uruchamiania emulatora. Jeśli HAXM jest zainstalowany, zostanie wyświetlony komunikat **HAX działa i emulatora działa w trybie szybkiego virt** opisane w tym zrzut ekranu:

    ![HAXM jest wyświetlane jako dostępne w oknie dialogowym Uruchamianie emulatora systemu Android](hardware-acceleration-images/mac/03-haxm-detected.png)

   Jeśli HAXM nie jest dostępny na komputerze (na przykład, jeśli zostanie wyświetlony komunikat o błędzie, takich jak _Sprawdź, czy Intel HAXM jest propertly zainstalowanych i można go użyć_), wykonaj kroki w następnej sekcji, aby zainstalować HAXM.


-----

<a name="install-haxm" />

### <a name="installing-haxm"></a>Instalowanie HAXM

Jeśli nie zostanie uruchomiony emulator, HAXM może być można zainstalować ręcznie. HAXM zainstalować pakiety dla systemów Windows i macOS są dostępne z [menedżera wykonywania przyspieszony sprzętu Intel](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) strony. Aby pobrać i zainstalować HAXM ręcznie, wykonaj następujące kroki:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Z witryny sieci Web firmy Intel, Pobierz najnowszą [HAXM wirtualizacji aparat](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) Instalatora dla systemu Windows. Zaletą pobrać Instalatora HAXM bezpośrednio z witryny sieci Web Intel jest to, że można mieć pewność, korzystania z najnowszej wersji.

   Alternatywnie za pomocą Menedżera zestawu SDK można pobrać Instalatora HAXM (w Menedżerze zestawu SDK, kliknij **Narzędzia > Dodatki > akceleratora Emulator Intel x86 (Instalator HAXM)**). Zestaw SDK systemu Android zwykle pobiera Instalator HAXM w następującej lokalizacji:

   **C:\\Program Files (x86)\\Android\\zestawu sdk systemu android\\dodatki\\intel\\sprzętu\_akcelerowanego\_wykonywania\_Manager**

   Należy pamiętać, że Menedżer zestawu SDK nie instaluje HAXM jedynie pobiera Instalator HAXM do powyższej lokalizacji; nadal trzeba uruchomić go ręcznie.

2. Uruchom **intelhaxm android.exe** można uruchomić Instalator HAXM. Zaakceptuj wartości domyślne w oknach dialogowych Instalatora:

   ![Okno Ustawienia przyspieszony menedżera wykonywania programu Intel sprzętu](hardware-acceleration-images/win/05-haxm-installer.png)

## <a name="hardware-acceleration-and-amd-cpus"></a>Przyspieszanie sprzętowe i procesorów AMD

Ponieważ emulator systemu Android firmy Google obecnie obsługuje przyspieszanie sprzętowe AMD [tylko w systemie Linux](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies), przyspieszanie sprzętowe nie jest dostępna dla procesory AMD komputery z systemem Windows.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Z witryny sieci Web firmy Intel, Pobierz najnowszą [HAXM wirtualizacji aparat](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) Instalator macOS.

2. Uruchom Instalatora HAXM. Zaakceptuj wartości domyślne w oknach dialogowych Instalatora:

   [![Okno Ustawienia przyspieszony menedżera wykonywania programu Intel sprzętu](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>Linki pokrewne

* [Uruchamianie aplikacji na emulatorze systemu Android](https://developer.android.com/studio/run/emulator)
