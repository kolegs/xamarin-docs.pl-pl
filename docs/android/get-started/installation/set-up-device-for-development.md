---
title: Konfigurowanie urządzeń środowiska deweloperskiego
description: W tym artykule będzie omawiać temat skonfigurować urządzenia z systemem Android i połączyć się z komputerem, tak aby urządzenie może służyć do uruchamiania i debugowanie aplikacji platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: 9116A3AA-EA00-56AF-AE70-BAEEC045EF11
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: 16716db67067f07166fa35df7e539cdf3ed1de5e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30768285"
---
# <a name="set-up-device-for-development"></a>Konfigurowanie urządzeń środowiska deweloperskiego

_W tym artykule będzie omawiać temat skonfigurować urządzenia z systemem Android i połączyć się z komputerem, tak aby urządzenie może służyć do uruchamiania i debugowanie aplikacji platformy Xamarin.Android._

Przez teraz prawdopodobnie w tym samouczku dużą nowej aplikacji uruchomionych na emulatorze systemu Android i chcesz zobaczyć będzie działać na urządzeniu z systemem Android obiektowi błyszczący. Poniżej przedstawiono kroki związane z łączeniem z urządzenia do komputera do debugowania:

1.  **Włącz debugowanie na urządzeniu** — domyślnie nie będzie możliwe do debugowania aplikacji na urządzeniu z systemem Android.

2.  **Zainstaluj sterowniki USB** — ten krok nie jest niezbędne, aby komputery OS X. Komputery z systemem Windows mogą wymagać zainstalowania sterowników USB.

3.  **Podłącz urządzenie do komputera** — ostatni krok obejmuje podłączania urządzeń do komputera przez USB lub Wi-Fi.

Każdy z tych kroków zostanie omówiona bardziej szczegółowo w poniższych sekcjach.


## <a name="enable-debugging-on-the-device"></a>Włącz debugowanie na urządzeniu

Istnieje możliwość użycia dowolne urządzenie z systemem Android do testowania aplikacji systemu Android. Jednak urządzenia muszą zostać prawidłowo skonfigurowane przed debugowania mogą wystąpić. Czynności procedury są nieco inne w zależności od wersji Android uruchomionej na urządzeniu.


### <a name="android-40-to-android-41"></a>Android 4.0 dla systemu Android 4.1

Debugowanie jest włączone dla systemu Android 4.0.x do 4.1.x systemu Android, wykonując następujące czynności:

1.  Przejdź do **ustawienia** ekranu.
2.  Wybierz **opcje dewelopera** .
3.  Zaznacz **debugowanie USB** opcji.

Ten zrzut ekranu przedstawia **opcje dewelopera** ekranu na urządzeniu z systemem Android 4.0.3:

[![Opcje dewelopera](set-up-device-for-development-images/developer-options-sml.png)](set-up-device-for-development-images/developer-options.png#lightbox)


### <a name="android-42-and-higher"></a>System android 4.2 lub nowszej

Począwszy od systemu Android 4.2 i nowszym, **opcje dewelopera** domyślnie ukryty. Aby było to możliwe, przejdź do **Ustawienia > informacje o telefonie**, a następnie naciśnij pozycję **numer kompilacji** elementu siedem razy, aby wyświetlić **opcje dewelopera** kartę:

[![Element numeru kompilacji](set-up-device-for-development-images/about-phone-sml.png)](set-up-device-for-development-images/about-phone.png#lightbox)

Raz **opcje dewelopera** karta jest dostępna w obszarze **Ustawienia > System**, otwórz go, aby wyświetlić ustawienia dewelopera:

[![Ekran ustawień dewelopera](set-up-device-for-development-images/developer3.png)](set-up-device-for-development-images/developer3.png#lightbox)

Jest to miejsce, aby włączyć opcje dewelopera, takich jak debugowanie USB i pozostać w trybie aktywności.


## <a name="install-usb-drivers"></a>Zainstaluj sterowniki USB

Ten krok nie jest konieczne dla systemu OS X. Podłącz urządzenie do komputera Mac za pomocą kabla USB.

Może być konieczne zainstalowanie pewnych dodatkowych sterowników przed komputerem z systemem Windows będą rozpoznawać urządzenia z systemem Android połączonymi USB.

> [!NOTE]
> Są kroki, aby skonfigurować urządzenie węzła Google i znajdują się jako odwołanie. Kroki dla określonego urządzenia może się różnić, ale zastosują się do tego samego typu. Jeśli masz problem wyszukiwania w Internecie dla danego urządzenia.

Uruchom **android.bat** aplikacji w **\tools [ścieżka instalacji zestawu SDK systemu Android]** katalogu. Domyślnie Instalator platformy Xamarin.Android umieści zestawu SDK systemu Android w następującej lokalizacji na komputerze z systemem Windows:

    C:\Users\[username]\AppData\Local\Android\android-sdk


### <a name="download-the-usb-drivers"></a>Pobieranie sterowników USB

Urządzenia z węzła Google (z wyjątkiem węzła Galaxy) wymagają sterownik USB Google. Sterownik węzła Galaxy jest [dystrybuowana Samsung](http://www.samsung.com/us/support/downloads/).
Wszystkie urządzenia z systemem Android należy używać [sterownik USB ich odpowiednich producenta](http://developer.android.com/tools/extras/oem-usb.html#Drivers).

Zainstaluj **sterownik USB Google** pakietu, uruchamiając narzędzie Android SDK Manager i rozwijając **dodatki** folderu, co wynika wykonaj zrzut ekranu:

[![Pakiet sterowników USB Google wybrany](set-up-device-for-development-images/usbdriverpackage.png)](set-up-device-for-development-images/usbdriverpackage.png#lightbox)

Sprawdź **sterownik USB Google** i kliknij **zainstalować** przycisku.
Pliki sterowników są pobierane w następującej lokalizacji:

    [Android SDK install path]\extras\google\usb\_driver

Domyślna ścieżka instalacji platformy Xamarin.Android to:

    C:\Users\[username]\AppData\Local\Android\android-sdk\extras\google\usb_driver



### <a name="installing-the-usb-driver"></a>Instalowanie sterownika USB

Po pobraniu sterowniki USB, należy je zainstalować.
Aby zainstalować sterowniki w systemie Windows 7:

1.  Podłącz urządzenie do komputera za pomocą kabla USB.

2.  Kliknij prawym przyciskiem myszy na komputerze z pulpitu lub w Eksploratorze Windows, a następnie wybierz **Zarządzaj** .

3.  Wybierz **urządzeń** w okienku po lewej stronie.

4.  Znajdź i rozwiń **inne urządzenia** w okienku po prawej stronie.

5.  Kliknij prawym przyciskiem myszy nazwę urządzenia i wybierz **aktualizacji sterowników** .
    Spowoduje to uruchomienie Kreatora aktualizacji sprzętu.

6.  Wybierz **Przeglądaj komputer oprogramowanie sterownika** i kliknij przycisk **dalej** .

7.  Kliknij przycisk **Przeglądaj** , aby zlokalizować folder sterownik USB (sterownik Google USB znajduje się w **\extras\google\usb_driver [ścieżka instalacji zestawu SDK systemu Android]**.

8.  Kliknij przycisk **dalej** do zainstalowania sterownika.


### <a name="installing-unverified-drivers-in-windows-8"></a>Instalowanie sterowników niezweryfikowane w systemie Windows 8

Aby zainstalować sterownik niezweryfikowane w systemie Windows 8 może wymagać dodatkowych czynności. W poniższych krokach opisano sposób instalowania sterowników dla węzła Galaxy:

1.  **Dostęp do systemu Windows 8 zaawansowane opcje rozruchu** — ten krok polega na ponowne uruchomienie komputera, aby uzyskać dostęp zaawansowane opcje rozruchu. Uruchom wpisz w wierszu polecenia i uruchom ponownie komputer za pomocą następującego polecenia:

        shutdown.exe /r /o

2.  **Podłącz urządzenie** -Podłącz urządzenie do komputera

3.  **Uruchom Menedżera urządzeń** — wykonywania **devmgmt.msc**; powinna zostać wyświetlona urządzenie na liście z żółtym trójkącie nad nim.

4.  **Instalowanie sterowników urządzeń** -zainstalować sterowniki urządzeń, jak opisano powyżej.



## <a name="connect-the-device-to-the-computer"></a>Podłącz urządzenie do komputera

Ostatnim krokiem jest Podłącz urządzenie do komputera. Istnieją dwa sposoby, w tym celu:

-   **Kabel USB** — jest to najprostszy i najbardziej typowych sposób. Po prostu Podłącz kabel USB do urządzenia, a następnie do komputera.

-   **Wi-Fi** — umożliwia łączenie urządzenia z systemem Android do komputera bez za pośrednictwem sieci Wi-Fi za pomocą kabla USB. Ta metoda wymaga nieco więcej wysiłku, ale mogą być przydatne, gdy istnieje nie kabla USB lub urządzenie ma daleko kabla USB. Łączenie za pośrednictwem sieci Wi-Fi zostanie omówiona w następnej sekcji.


### <a name="connecting-over-wifi"></a>Połączenie za pośrednictwem sieci Wi-Fi

Domyślnie [mostka debugowania Android](http://developer.android.com/tools/help/adb.html) (*ADB*) jest skonfigurowany do komunikowania się z urządzenia z systemem Android przez połączenie USB. Istnieje możliwość ponownej konfiguracji do używania protokołu TCP/IP zamiast USB. Aby to zrobić, zarówno urządzenia, jak i na komputerze musi być w tej samej sieci Wi-Fi. Aby skonfigurować środowisko do debugowania za pośrednictwem problem WiF następujące kroki w wierszu polecenia:

1.  Określ adres IP urządzenia z systemem Android. Aby odszukać poza adres IP jest Sprawdź w obszarze **Ustawienia > sieci Wi-Fi** , a następnie naciśnij przycisk na urządzenie jest podłączone do sieci Wi-Fi. Pojawi się ekran Ustawienia pokazywane są informacje dotyczące połączenia sieciowego, podobnie jak co występuje na poniższym zrzucie ekranu:

    ![Adres IP](set-up-device-for-development-images/ip-settings.png)

    W niektórych wersjach systemu Android nie liście nie adres IP, ale zamiast tego można znaleźć w obszarze **Ustawienia > informacje o telefonie > Stan**.

2.  Podłącz urządzenie z systemem Android do komputera przez połączenie USB.

3.  Następnie uruchom ponownie ADB tak it przy użyciu protokołu TCP na porcie 5555. W wierszu polecenia wpisz następujące polecenie:

        adb tcpip 5555

    Po uruchomieniu tego polecenia, komputer nie będzie nasłuchiwać na urządzeniach, które są połączone przez połączenie USB.

4.  Odłączyć kabel USB łączenie urządzenia z komputera.

5.  Skonfiguruj ADB, dzięki czemu będą łączyć się urządzenie z systemem Android na porcie, który został określony w kroku 1:

        adb connect 192.168.1.28:5555

    Po zakończeniu tego polecenia Android urządzenie jest podłączone do komputera za pośrednictwem sieci Wi-Fi.

Po zakończeniu debugowanie za pośrednictwem sieci Wi-Fi, jest możliwe Resetowanie ADB z powrotem do trybu USB przy użyciu następującego polecenia:

    adb usb

Użytkownik może zażądać ADB, aby wyświetlić listę urządzeń, które są podłączone do komputera. Niezależnie od tego, jak urządzenia są połączone należy wydać następujące polecenie w wierszu polecenia jest podłączony co:

    adb devices


## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób konfigurowania urządzenia z systemem Android do tworzenia aplikacji, należy włączyć debugowanie na urządzeniu. Uwzględniane jak Podłącz urządzenie do komputera za pomocą USB lub Wi-Fi.


## <a name="related-links"></a>Linki pokrewne

- [Android mostka debugowania](http://developer.android.com/tools/help/adb.html)
- [Za pomocą urządzeń sprzętowych](http://developer.android.com/tools/device.html)
- [Samsung pobranych plików](http://www.samsung.com/us/support/downloads/)
- [Sterowniki USB przez producenta OEM](http://developer.android.com/tools/extras/oem-usb.html#Drivers)
- [Google USB Driver](http://developer.android.com/sdk/win-usb.html)
- [Deweloperzy XDA: Windows 8 - rozwiązanie problemu ze sterownikiem ADB/funkcję fastboot](http://forum.xda-developers.com/showthread.php?t=1583801)
