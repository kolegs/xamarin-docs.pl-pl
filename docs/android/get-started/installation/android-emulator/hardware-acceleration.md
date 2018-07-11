---
title: Przyspieszanie sprzętowe emulatora wydajności
description: W tym artykule wyjaśniono, jak używać funkcji przyspieszenia sprzętowego komputera, aby zmaksymalizować wydajność emulatora systemu Android.
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: c2bef2c614d4cc0655deb9732ccefec223a8318a
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848470"
---
# <a name="hardware-acceleration-for-emulator-performance"></a>Przyspieszanie sprzętowe emulatora wydajności

_W tym artykule wyjaśniono, jak używać funkcji przyspieszenia sprzętowego komputera, aby zmaksymalizować wydajność emulatora systemu Android._

## <a name="overview"></a>Omówienie

Program Visual Studio ułatwia deweloperom testowanie i debugowanie aplikacji platformy Xamarin.Android przy użyciu emulatora systemu Android w sytuacjach, gdy urządzenie z systemem Android jest niedostępna lub niepraktyczne.
Jednak emulatora systemu Android jest uruchamiany zbyt wolno, jeśli przyspieszanie sprzętowe nie jest dostępny na komputerze, na którym działa. Może znacząco zwiększyć wydajność emulatora systemu Android przy użyciu sprzętu, który docelowej x86 obrazów specjalne urządzenia wirtualnego w połączeniu z jednym z dwóch technologii wirtualizacji:

1. **Firmy Microsoft Hyper-V i platformie funkcji Hypervisor**. Funkcji Hyper-V jest funkcją wirtualizacji systemu Windows, która umożliwia uruchamianie systemów komputerowych zwirtualizowanych na fizycznym komputerze hosta. Jest to technologii wirtualizacji zalecane do użytku przyspieszanie emulatora systemu Android. Aby dowiedzieć się więcej na temat funkcji Hyper-V, zobacz [funkcji Hyper-V w systemie Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).

2. **Firmy Intel przyspieszane menedżera wykonywania (HAXM)**. 
   Technologia HAXM jest aparatem wirtualizacji dla komputerów z systemem procesory Intel.
   Jest to aparat wirtualizacji zalecana dla komputerów, które nie można uruchomić funkcji Hyper-V.

Emulator systemu Android automatycznie spowoduje, że korzystanie z przyspieszenia sprzętowego, jeśli są spełnione poniższe kryteria:

-   Przyspieszanie sprzętowe jest dostępny i włączony na komputerze deweloperskim.

-   Emulator jest uruchomiony obraz emulatora utworzone specjalnie do **x86**— na podstawie urządzenia wirtualnego.

Aby uzyskać informacji na temat uruchamiania i debugowania przy użyciu emulatora systemu Android, zobacz [debugowanie w emulatorze systemu Android](~/android/deploy-test/debugging/debug-on-emulator.md).

## <a name="hyper-v"></a>Funkcja Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Obsługa funkcji Hyper-V jest obecnie w wersji zapoznawczej.

Deweloperzy, którzy korzystają z systemu Windows 10 (aktualizacja z kwietnia 2018 r. lub nowszej) zdecydowanie zaleca się umożliwia przyspieszenie emulatora systemu Android firmy Microsoft Hyper-V. Emulator systemu Android za pomocą funkcji Hyper-V:

1. **Aktualizacja systemu Windows 10 kwietnia 2018 aktualizacja (kompilacja 1803) lub nowszym**.
   Aby sprawdzić, która wersja systemu Windows jest uruchomiona, kliknij na pasku wyszukiwania Cortany i wpisz **o**. Wybierz **informacje o komputerze** w wynikach wyszukiwania. Przewiń w dół **o** okno dialogowe **specyfikacje Windows** sekcji. **Wersji** powinien być co najmniej w wersji 1803:

    [![Specyfikacje Windows](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Włącz platformę Windows funkcji Hypervisor**.
   Na pasku wyszukiwania Cortany wpisz **Windows Włącz lub wyłącz funkcje**.
   Przewiń w dół **funkcji Windows** okna dialogowego i upewnij się, że **platformie funkcji Hypervisor Windows** jest włączona:

    [![Platforma Hypervisor Windows włączone](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

   Włączanie **platformie funkcji Hypervisor Windows** automatycznie włącza funkcji Hyper-V. To dobry pomysł, aby ponownie uruchomić Windows po wprowadzeniu tej zmiany.

3. **Zainstaluj [Visual Studio 15.8 w wersji zapoznawczej 1 lub nowszym](https://visualstudio.microsoft.com/vs/preview/)**.
   Ta wersja programu Visual Studio zapewnia obsługę środowiska IDE do uruchamiania emulatora systemu Android za pomocą funkcji Hyper-V.
 
4. **Zainstaluj pakiet emulatora systemu Android 27.2.7 lub nowszym**. Aby zainstalować ten pakiet, przejdź do **Narzędzia > Android > Menedżer zestawów SDK** w programie Visual Studio. Wybierz **narzędzia** kartę i upewnij się, że Emulator systemu Android w wersji co najmniej 27.2.7. Upewnij się również, że wersja Android SDK Tools jest 26.1.1 lub nowszej:

    [![Zestawy android SDK i narzędzi okna dialogowego](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Jeśli wersja emulatora jest co najmniej 27.2.7, ale mniej niż 27.3.1, poniższe obejście jest wymagana do używania funkcji Hyper-V:

    1.  W **C:\\użytkowników\\_username_\\.android** folderze utwórz plik o nazwie **advancedFeatures.ini** (jeśli go nie już istnieje).

    2.  Dodaj następujący wiersz do **advancedFeatures.ini**:
        ```
        WindowsHypervisorPlatform = on
        ```


### <a name="known-issues"></a>Znane problemy

-   Jeśli nie można zaktualizować do wersji emulatora 27.2.7 lub później, po uaktualnieniu do programu Visual Studio w wersji zapoznawczej, może być konieczne bezpośrednio zainstalować [Instalatora w wersji zapoznawczej](http://aka.ms/hyperv-emulator-dl) umożliwiające nowszych wersji emulatora.

-   Wydajność może być ograniczona, korzystając z niektórych firmy Intel i procesory AMD procesorów.

-   Aplikacja dla systemu android może potrwać nietypowe ilość czasu ładowania na wdrożenie.

-   Błąd dostępu rozwiązanie MMIO sporadycznie może uniemożliwić rozruchu w emulatorze systemu Android. Ponowne uruchamianie w emulatorze powinno rozwiązać ten problem.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Obsługa funkcji Hyper-V wymaga systemu Windows 10. Zobacz [wymagania funkcji Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements) Aby uzyskać więcej informacji.

-----

## <a name="haxm"></a>APARAT HAXM

Technologia HAXM jest aparat wirtualizacji sprzętowej (funkcja hypervisor), który używa Intel Virtualization Technology (VT) w celu przyspieszenia emulacji aplikacji dla systemu Android na komputerze hosta. Przy użyciu technologii haxm i umożliwiający w połączeniu z systemem Android x86 obrazy emulatora dostarczane przez firmę Intel, umożliwia szybsze Android emulacji w systemach z obsługą VT.

Jeśli opracowujesz na maszynie z procesora CPU Intel VT możliwości możesz korzystać z zalet aparatu HAXM, aby znacznie szybciej rozpocząć pracę emulatora systemu Android (Jeśli nie masz pewności, czy Twoje procesor CPU obsługuje VT, zobacz [jest Mój procesor obsługuje Intel Virtualization Technologia? ](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Nie można uruchomić emulatora accelerated maszyny Wirtualnej wewnątrz innej maszyny Wirtualnej, np. maszyna wirtualna hostowana przez VirtualBox, programu VMWare lub platformy Docker. Należy uruchomić emulatora systemu Android [bezpośrednio na danym urządzeniu systemu](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Przed rozpoczęciem korzystania z emulatora systemu Android po raz pierwszy, jest dobry pomysł, aby sprawdzić, czy aparat HAXM jest zainstalowanych i dostępnych do emulatora systemu Android do użycia.

### <a name="verifying-haxm-installation"></a>Weryfikowanie instalacji aparatu HAXM

Można sprawdzić, czy aparat HAXM jest dostępna, wyświetlając **uruchamianie emulatora systemu Android** okna podczas uruchamiania emulatora. Można uruchomić emulatora systemu Android, wykonaj następujące czynności:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uruchom Menedżera urządzeń Android, klikając **Narzędzia > Android > Menedżer urządzeń Android**:

    [![Lokalizacja elementu menu systemu android Device Manager](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Jeśli widzisz **ostrzeżenie dotyczące wydajności** okno dialogowe podobny do poniższego, a następnie aparatu HAXM jest nie zostały jeszcze zainstalowane lub prawidłowo skonfigurowany na komputerze:

    ![Okno dialogowe z ostrzeżeniem wydajności aparatu HAXM nie jest gotowy](hardware-acceleration-images/win/11-perf-warn.png)

   Jeśli **ostrzeżenie dotyczące wydajności** okno dialogowe, takich jak to jest wyświetlane, zobacz [ostrzeżeń dotyczących wydajności](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) Aby zidentyfikować przyczynę i rozwiązać pierwotny problem.

3. Wybierz **x86** obrazu (na przykład **VisualStudio\_android-23\_x86\_phone**) i kliknij przycisk **Start**:

    ![Uruchamianie emulatora systemu Android przy użyciu domyślnego obrazu urządzenia wirtualnego](hardware-acceleration-images/win/02-start-default-avd.png)

4. Poszukaj **uruchamianie emulatora systemu Android** okna dialogowego podczas uruchamiania emulatora. Jeśli zainstalowano aparatu HAXM, zostanie wyświetlony komunikat, **działa HAX i emulatora działa w trybie Szybkie virt** pokazany na tym zrzucie ekranu:

    ![Technologia HAXM jest wyświetlane jako dostępne w oknie dialogowym Uruchamianie emulatora systemu Android](hardware-acceleration-images/win/03-haxm-detected.png)

   Jeśli ten komunikat nie jest widoczny, technologia HAXM prawdopodobnie nie jest zainstalowana. Na przykład poniżej przedstawiono zrzut ekranu przedstawiający komunikat informujący o tym, że może się pojawić, jeśli aparat HAXM nie jest dostępna:

    ![Technologia HAXM nie jest dostępne na tej maszynie](hardware-acceleration-images/win/04-haxm-error.png)

   Jeśli aparat HAXM nie jest dostępny na komputerze, wykonaj kroki w następnej sekcji, do zainstalowania aparatu HAXM.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom Menedżera urządzeń Android, klikając **Narzędzia > Menedżer urządzeń**:

    [![Lokalizacja elementu menu systemu android Device Manager](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Jeśli widzisz **ostrzeżenie dotyczące wydajności** okno dialogowe podobny do poniższego, a następnie aparatu HAXM jest nie zostały jeszcze zainstalowane lub prawidłowo skonfigurowany na komputerze:

    ![Okno dialogowe z ostrzeżeniem wydajności aparatu HAXM nie jest gotowy](hardware-acceleration-images/mac/04-avd-warning.png)

   Jeśli **ostrzeżenie dotyczące wydajności** okno dialogowe, takich jak to jest wyświetlane, zobacz [ostrzeżeń dotyczących wydajności](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) Aby zidentyfikować przyczynę i rozwiązać pierwotny problem.

3. Wybierz **x86** obrazu (na przykład **Android\_Accelerated\_x86**) i kliknij przycisk **Odtwórz**:

    [![Uruchamianie emulatora systemu Android przy użyciu domyślnego obrazu urządzenia wirtualnego](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. Poszukaj **uruchamianie emulatora systemu Android** okna dialogowego podczas uruchamiania emulatora. Jeśli zainstalowano aparatu HAXM, zostanie wyświetlony komunikat, **działa HAX i emulatora działa w trybie Szybkie virt** pokazany na tym zrzucie ekranu:

    ![Technologia HAXM jest wyświetlane jako dostępne w oknie dialogowym Uruchamianie emulatora systemu Android](hardware-acceleration-images/mac/03-haxm-detected.png)

   Jeśli aparat HAXM nie jest dostępny na komputerze (na przykład, jeśli zostanie wyświetlony komunikat o błędzie, takich jak _upewnij się, Intel HAXM jest propertly zainstalowane i można używać_), wykonaj kroki w następnej sekcji, aby zainstalować aparatu HAXM.

-----

<a name="install-haxm" />

### <a name="installing-haxm"></a>Trwa instalowanie aparatu HAXM

Nie można uruchomić emulatora, może być można zainstalować ręcznie aparatu HAXM. Aparat HAXM instalowania pakietów dla systemu macOS i Windows są dostępne z [menedżera wykonywania Accelerated sprzętu Intel](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) strony. Aby pobrać i zainstalować ręcznie aparatu HAXM, wykonaj następujące kroki:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. W witrynie internetowej firmy Intel, Pobierz najnowszy [aparat wirtualizacji aparatu HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) Instalator Windows. Zaletą pobrać Instalatora aparatu HAXM bezpośrednio z witryny sieci Web Intel jest, że możesz mieć pewność, z przy użyciu najnowszej wersji.

   Alternatywnie można użyć Menedżera zestawów SDK można pobrać Instalatora aparatu HAXM (Menedżer zestawów SDK kliknij **Narzędzia > Dodatki > Intel x86 emulatora Accelerator (Instalatora aparatu HAXM)**). Zwykle zestawu Android SDK do pobrania Instalatora aparatu HAXM, w następującej lokalizacji:

   **C:\\Program Files (x86)\\Android\\zestawu sdk systemu android\\dodatki\\intel\\sprzętu\_Accelerated\_wykonywania\_Menedżera**

   Należy pamiętać, że Menedżer zestawów SDK nie instaluje aparatu HAXM jedynie pobiera Instalatora aparatu HAXM do powyższej lokalizacji; nadal trzeba uruchomić je ręcznie.

2. Uruchom **intelhaxm android.exe** można uruchomić Instalatora aparatu HAXM. Zaakceptuj wartości domyślne w oknach dialogowych Instalatora:

   ![Okno Ustawienia Menedżera wykonywania Accelerated sprzęt firmy Intel](hardware-acceleration-images/win/05-haxm-installer.png)

## <a name="hardware-acceleration-and-amd-cpus"></a>Przyspieszanie sprzętowe oraz procesory AMD

Ponieważ emulator systemu Android obsługuje obecnie przyspieszanie sprzętowe AMD [tylko w systemie Linux](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies), przyspieszanie sprzętowe nie jest dostępna dla procesory AMD komputerów z systemem Windows.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. W witrynie internetowej firmy Intel, Pobierz najnowszy [aparat wirtualizacji aparatu HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) Instalatora dla systemu macOS.

2. Uruchom Instalatora aparatu HAXM. Zaakceptuj wartości domyślne w oknach dialogowych Instalatora:

   [![Okno Ustawienia Menedżera wykonywania Accelerated sprzęt firmy Intel](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>Linki pokrewne

* [Uruchamianie aplikacji w emulatorze systemu Android](https://developer.android.com/studio/run/emulator)
