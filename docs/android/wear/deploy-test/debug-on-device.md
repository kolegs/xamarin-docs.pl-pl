---
title: "Debugowanie na urządzeniu zużycie"
description: "W tym artykule wyjaśniono, jak debugowanie aplikacji platformy Xamarin.Android zużycia zużycia urządzenia."
ms.topic: article
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c12764610d0fd9834914b8114818b2ccd7d7def0
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="debug-on-a-wear-device"></a>Debugowanie na urządzeniu zużycie

_W tym artykule wyjaśniono, jak debugowanie aplikacji platformy Xamarin.Android zużycia zużycia urządzenia._


## <a name="overview"></a>Omówienie

Jeśli masz urządzenie Android nosić, takich jak Android Smartwatch nosić, aplikację można uruchomić na urządzeniu, a nie przy użyciu emulatora. (Jeśli nie są jeszcze zapoznać się z procesem wdrażania i uruchamiania aplikacji systemu Android nosić, zobacz [nosić Witaj,](~/android/wear/get-started/hello-wear.md).)

## <a name="prepare-the-wear-device"></a>Przygotuj urządzenie zużycia:

Aby włączyć debugowanie na urządzeniu z systemem Android nosić, wykonaj następujące kroki:

1.  Otwórz **ustawienia** w systemie Android zużycia urządzenia.

2.  Przewiń w dół menu i naciśnij **o**.

3.  Wybierz numer kompilacji 7 razy.

4.  Na **ustawienia** menu, wybierz **opcje dewelopera**.

5.  Upewnij się, że **debugowania ADB** jest włączona.


## <a name="debugging-over-usb"></a>Debugowanie USB

Jeśli zużycia urządzenia ma USB port, możesz można połączyć się z komputerem zużycia urządzenia, wdrażać do niego i uruchomienia/debugowania aplikacji jako jak przy użyciu telefonie z systemem Android (Aby uzyskać więcej informacji, zobacz [debugowania na urządzeniu](~/android/deploy-test/debugging/debug-on-device.md)).


## <a name="debugging-over-bluetooth"></a>Debugowanie przez sieć Bluetooth

Jeśli urządzenie zużycia nie ma portu USB, można wdrożyć aplikację do zużycia urządzenia przez sieć Bluetooth przez routingu danych wyjściowych debugowania aplikacji systemu Android numerem telefonu, który jest podłączony do komputera. 

### <a name="prepare-your-phone"></a>Przygotowanie telefonu

Aby przygotować telefonu do połączeń Bluetooth na urządzeniu zużycia, wykonaj następujące kroki: 

1.  Jeśli nie zostało to jeszcze zrobione, skonfiguruj telefon do programowania aplikacji platformy Xamarin.Android zgodnie z objaśnieniem w [ustawić się urządzenia na potrzeby programowania](~/android/get-started/installation/set-up-device-for-development.md).

2.  Pobierz i zainstaluj bezpłatną [Android nosić](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) aplikację ze sklepu Google Play.

### <a name="connect-the-device"></a>Podłącz urządzenie

Następujące kroki umożliwiają podłączenie zużycia na Twój telefon:

1.  Na telefonie który działa jako pośredniczącego Bluetooth (skonfigurowanych powyżej), uruchomić aplikację systemu Android nosić. 

2.  Wybierz **ustawienia** ikony.

3.  Włącz **debugowania przez sieć Bluetooth**. Powinny pojawić się następujące stany wyświetlane na ekranie aplikacji systemu Android nosić:

        Host: disconnected
        Target: connected

4.  Podłącz telefon do komputera za pośrednictwem USB. Na komputerze wprowadź następujące polecenia:

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    Jeśli port 4444 nie jest dostępny, można użyć innych dostępny port, do których masz dostęp. 

    **Uwaga**: po ponownym uruchomieniu programu Visual Studio lub Visual Studio dla komputerów Mac, należy uruchomić następujące polecenia, aby ponownie nawiązanie połączenia z urządzeniem zużycia.

5.  Urządzenie zużycia monit, upewnij się, że umożliwi **debugowania ADB**. W aplikacji systemu Android nosić powinien zostać wyświetlony stan zmiany do:

        Host: connected
        Target: connected

6.  Po wykonaniu powyższych czynności systemem `adb devices` pokazuje stan telefonu i urządzenia z systemem Android nosić:

        List of devices attached
        127.0.0.1:4444    device
        019ad61df0a69399  device

W tym momencie można wdrożyć aplikację na urządzeniu zużycia.

<a name="screenshots" />

### <a name="taking-screenshots"></a>Biorąc zrzuty ekranu

Zrzut ekranu zużycia urządzenia może potrwać, wprowadzając następujące polecenie: 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

Kopiowanie zrzut ekranu na komputer, wpisując następujące polecenie:

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

Usuń zrzut ekranu na urządzeniu, wprowadzając następujące polecenie:

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>Odinstalowywanie aplikacji

Z zużycia urządzenia można odinstalować aplikację, wprowadzając następujące polecenie:

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

Na przykład, aby usunąć aplikację z nazwą pakietu `com.xamarin.weartest`, wprowadź następujące polecenie:

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Aby uzyskać więcej informacji o debugowaniu urządzenia Android nosić przez sieć Bluetooth, zobacz [debugowania przez sieć Bluetooth](https://developer.android.com/training/wearables/apps/bt-debugging.html).


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>Debugowania aplikacji zużycia z aplikacji phone pomocnika

Aplikacje dla systemu android zużycia są dostarczane z aplikacją telefonów z systemem Android pomocnika do dystrybucji w witrynie Google Play (Aby uzyskać więcej informacji, zobacz [Praca z opakowania](~/android/wear/deploy-test/packaging.md)). Jednak nadal opracowywania aplikacji zużycia i jego aplikacji pomocnika oddzielnie. Po zwolnieniu aplikacji do sklepu Google Play aplikację zużycia zostanie wchodzących w skład aplikacji pomocnika i instalowane automatycznie, jeśli to możliwe.

Debugowanie aplikacji zużycia z aplikacją uzupełniający: 

1.  Tworzenie i wdrażanie aplikacji pomocnika z numerem telefonu.

2.  Kliknij prawym przyciskiem myszy projekt zużycia oraz ustaw go jako domyślny projekt rozpoczęcia.

3.  Wdrażanie projektu zużycia wearable urządzenia.

4.  Uruchom i debugowanie aplikacji zużycia urządzenia.

 
## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak skonfigurować dla zużycia debugowania w programie Visual Studio za pomocą funkcji Bluetooth urządzenia Android nosić i debugowanie aplikacji zużycia za pomocą aplikacji phone pomocnika. Dostępne również wspólnej porady debugowania do debugowania aplikacji zużycia, za pośrednictwem połączenia Bluetooth.
