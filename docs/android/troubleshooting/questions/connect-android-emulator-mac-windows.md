---
title: Możliwe do nawiązania połączenia z systemem Android emulatorów działa na komputerze Mac z maszyny Wirtualnej systemu Windows?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2018
ms.openlocfilehash: f94c0966dd67940fc201df84a311db422d77b542
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935204"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Możliwe do nawiązania połączenia z systemem Android emulatorów działa na komputerze Mac z maszyny Wirtualnej systemu Windows?

Aby połączyć się emulatora systemu Android, uruchamiając na komputerze Mac z maszyny wirtualnej systemu Windows, należy użyć następujące czynności:

1.  Uruchom emulator na komputerach Mac.

2.  Kasowanie `adb` serwera dla komputerów Mac:

    ```bash
    adb kill-server
    ```

3.  Należy zwrócić uwagę, emulator nasłuchuje 2 porty TCP dla sprzężenia zwrotnego interfejsu sieciowego:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    Nieparzystą port jest używany do nawiązania połączenia `adb`. Zobacz też [ http://developer.android.com/tools/devices/emulator.html#emulatornetworking ](http://developer.android.com/tools/devices/emulator.html#emulatornetworking).

4.  _Opcja 1_: Użyj [ `nc` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html) do przodu przychodzących pakietów TCP zewnętrznie odebranych na porcie 5555 (lub innego portu chcesz) do portu nieparzystą interfejsu sprzężenia zwrotnego (**127.0.0.1 5555** w tym przykładzie), a do przekazywania pakietów wychodzących z powrotem inny sposób:

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    Tak długo, jak `nc` polecenia pozostanie uruchomione w okno terminalu, pakiety zostaną przekazane zgodnie z oczekiwaniami. CTRL-C można wpisać w oknie terminalu, aby zakończyć `nc` polecenia po zakończeniu instalacji przy użyciu emulatora.

    (Opcja 1 jest zazwyczaj łatwiejsze niż opcja 2, zwłaszcza jeśli **preferencjach systemowych > Zabezpieczenia i prywatność > zapory** jest włączone.) 

    _Opcja 2_: Użyj [ `pfctl` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html) przekierować pakiety TCP z portu `5555` (lub innego portu chcesz) na [udostępniony sieci](http://kb.parallels.com/en/4948) interfejs do portu nieparzystą w interfejsu sprzężenia zwrotnego (`127.0.0.1:5555` w tym przykładzie):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    To polecenie ustawia port przekazywania przy użyciu `pf packet filter` usługi systemowej. Podziały wierszy są ważne. Pamiętaj zachować je podczas kopiowania i wklejania. Należy również dostosować nazwę interfejsu z *vmnet8* Jeśli używasz równoleżników. `vmnet8` Nazwa specjalną *urządzenie NAT* dla *udostępniony sieci* trybu w VMWare Fusion. Prawdopodobnie odpowiedni interfejs sieciowy w rozwiązaniu Parallels [vnic0](http://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5.  Podłącz do emulatora z komputera z systemem Windows:

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    Zastąp "ip — adres z —-mac" przy użyciu adresu IP, Mac, na przykład wymienione przez `ifconfig vmnet8 | grep 'inet '`. W razie potrzeby Zastąp `5555` z innego portu, które chcesz z kroku 4\. (Uwaga: Aby uzyskać dostęp z wiersza polecenia do `adb` odbywa się za pośrednictwem [ **Narzędzia > Android > Android wiersza polecenia Adb** ](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) w programie Visual Studio.)

### <a name="alternate-technique-using-ssh"></a>Alternatywny przy użyciu techniki `ssh`

Jeśli włączono _logowania zdalnego_ dla komputerów Mac, wówczas można używać `ssh` przekierowanie do nawiązania połączenia w emulatorze portów.

1.  Zainstaluj klienta SSH w systemie Windows. Jedną z opcji jest zainstalowanie [Git dla systemu Windows](https://git-for-windows.github.io/). `ssh` Polecenia będzie dostępna w **Git Bash** wiersza polecenia.

2.  Wykonaj kroki 1 – 3 z powyższych Uruchom emulator, kill `adb` serwera dla komputerów Mac i określić portów emulatora.

3.  Uruchom `ssh` w systemie Windows, aby skonfigurować przekierowania portów dwukierunkowej między portów lokalnych w systemie Windows (`localhost:15555` w tym przykładzie) i portu nieparzystą emulatora interfejsu sprzężenia zwrotnego Mac (`127.0.0.1:5555` w tym przykładzie):

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Zastąp `mac-username` z nazwy użytkownika Mac wymienionych przez `whoami`. Zastąp `ip-address-of-the-mac` przy użyciu adresu IP dla komputerów Mac.

4.  Podłącz do emulatora przy użyciu portu lokalnego w systemie Windows:

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Uwaga: jeden łatwy sposób uzyskać dostęp z wiersza polecenia do `adb` odbywa się za pośrednictwem [ **Narzędzia > Android > Android wiersza polecenia Adb** w programie Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

Mała Uwaga: korzystając z portu `5555` dla portu lokalnego `adb` podejrzenie, że emulator działa lokalnie w systemie Windows. To nie powoduje żadnych problemów w programie Visual Studio, ale w programie Visual Studio for Mac powoduje wyjście natychmiast po uruchamiania aplikacji.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>Przy użyciu techniki alternatywne `adb -H` nie jest jeszcze obsługiwana

Teoretycznie innym rozwiązaniem jest używany `adb`przez wbudowane możliwości nawiązywania połączenia z `adb` serwer z uruchomioną na komputerze zdalnym (na przykład zobacz [ http://stackoverflow.com/a/18551325 ](http://stackoverflow.com/a/18551325)).
Ale rozszerzeń platformy Xamarin.Android IDE nie udostępniają obecnie sposób konfigurowania tej opcji.

## <a name="contact-information"></a>Informacje kontaktowe

W tym dokumencie omówiono bieżące zachowanie wersji marca 2016 r. Techniki opisane w tym dokumencie nie jest częścią stabilna suite testowania dla platformy Xamarin, więc może spowodować przerwanie w przyszłości.

Jeśli można zauważyć, że techniki już nie działa lub Jeśli zauważysz innych błędów w dokumencie, możesz także dodać do dyskusji w następujących wątku forum: [ http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm ](http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
Dziękujemy!

