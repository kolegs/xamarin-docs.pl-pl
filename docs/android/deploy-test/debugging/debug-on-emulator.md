---
title: Debugowanie na emulatorze systemu Android
description: W tym przewodniku opisano sposób uruchomienia i debugowania aplikacji w programie Visual Studio przy użyciu emulatora systemu Android.
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 9e0eade7a2e033838f78f24270ec2bf9d4abc171
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935791"
---
# <a name="debugging-on-the-android-emulator"></a>Debugowanie na emulatorze systemu Android

_W tym przewodniku dowiesz się, jak uruchomić urządzenie wirtualne w emulatorze systemu Android w celu debugowania i testowania aplikacji._

## <a name="overview"></a>Omówienie

Emulatora systemu Android (instalowane jako część **Mobile development z platformą .NET** obciążenia), mogą być uruchamiane w różnych konfiguracjach, aby symulować różne urządzenia z systemem Android. Każdy z tych konfiguracji jest tworzony jako _urządzenia wirtualnego_. W tym przewodniku dowiesz się, jak Uruchom emulator w Visual Studio i uruchomić aplikację w urządzenie wirtualne. Aby uzyskać informacje o konfigurowaniu emulatora systemu Android i tworzenia nowych urządzeń wirtualnych, zobacz [Konfiguracja emulatora systemu Android](~/android/get-started/installation/android-emulator/index.md).


## <a name="using-a-pre-configured-virtual-device"></a>Za pomocą wstępnie skonfigurowanych urządzeń wirtualnych

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio obejmuje wstępnie skonfigurowane urządzeń wirtualnych, które są wyświetlane w menu rozwijanym urządzenia. Na przykład na poniższym zrzucie ekranu Visual Studio 2017 kilka wstępnie skonfigurowanych urządzeń wirtualnych są dostępne:

-   **Visual Studio\_android 23\_arm\_telefonu**

-   **Visual Studio\_android 23\_arm\_typu tablet**

-   **Visual Studio\_android 23\_x86\_telefonu** 

-   **VisualStudio\_android-23\_x86\_tablet** 

[![Urządzenia wirtualne](debug-on-emulator-images/win/01-virtual-devices-sml.png)](debug-on-emulator-images/win/01-virtual-devices.png#lightbox)

Zazwyczaj należy wybrać **VisualStudio\_android 23\_x86\_phone** urządzenie wirtualne do testowania i debugowania aplikacji telefonicznej. Jeśli jeden z tych wstępnie skonfigurowanych urządzeń wirtualnych odpowiada wymaganiom, (tj. pasuje do elementu docelowego Twojej aplikacji poziom interfejsu API), przejdź do [uruchamianie w emulatorze](#launching) celu rozpoczęcia uruchamiania aplikacji w emulatorze. (Jeśli nie znasz jeszcze z poziomami interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).)

Jeśli projekt platformy Xamarin.Android korzysta z poziomu platformy docelowej, która jest niezgodna z dostępnych urządzeń wirtualnych, menu rozwijane wymieniono bezużyteczne urządzeń wirtualnych w obszarze **nieobsługiwany urządzeń**. Na przykład następujący projekt ma ustawioną platformy docelowej **nugacie 7.1 systemu Android (interfejs API 25)**, który jest niezgodny z **Android 6.0** urządzeń wirtualnych, które są wymienione w tym przykładzie:

[![Niezgodne urządzenie wirtualne](debug-on-emulator-images/win/02-incompatible-level-sml.png)](debug-on-emulator-images/win/02-incompatible-level.png#lightbox)

Możesz kliknąć **zmienić docelowy z systemem Android co najmniej** Aby zmienić projekt obiektu minimalna wersja systemu Android, aby było poziom interfejsu API dostępnych urządzeń wirtualnych. Alternatywnie można użyć [Menedżera urządzeń Android](~/android/get-started/installation/android-emulator/device-manager.md) do tworzenia nowych urządzeń wirtualnych, które obsługują docelowego interfejsu API poziomu.
Aby można było skonfigurować urządzenia wirtualne Nowy poziom interfejsu API, należy najpierw zainstalować odpowiedni obrazów systemu dla tego poziomu interfejsu API (zobacz [Definiowanie zestawu Android SDK dla platformy Xamarin.Android](~/android/get-started/installation/android-sdk.md)).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac obejmuje wstępnie skonfigurowane urządzeń wirtualnych, które są wyświetlane w menu rozwijanym urządzenia. Na przykład na poniższym zrzucie ekranu, dostępne są dwa wstępnie skonfigurowanych urządzeń wirtualnych:

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[![Urządzenia wirtualne](debug-on-emulator-images/mac/01-virtual-devices-sml.png)](debug-on-emulator-images/mac/01-virtual-devices.png#lightbox)

Zazwyczaj należy wybrać **Android\_akcelerowanego\_x86** urządzenie wirtualne do testowania i debugowania aplikacji telefonicznej. Jeśli to wstępnie skonfigurowane wirtualne urządzenie spełnia wymagania dotyczące (tj. pasuje do elementu docelowego Twojej aplikacji poziom interfejsu API), przejdź do [uruchamianie w emulatorze](#launching) celu rozpoczęcia uruchamiania aplikacji w emulatorze. (Jeśli nie znasz jeszcze z poziomami interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).)

-----

## <a name="editing-virtual-devices"></a>Edytowanie urządzeń wirtualnych

Aby zmodyfikować urządzeń wirtualnych (lub utworzyć nowe), należy użyć [Menedżera urządzeń Android](~/android/get-started/installation/android-emulator/device-manager.md).


<a name="launching" />

## <a name="launching-the-emulator"></a>Uruchamianie emulatora

W górnej części programu Visual Studio jest menu rozwijanego, który może służyć do wybrania **debugowania** lub **wersji** tryb. Wybieranie **debugowania** powoduje, że debuger można dołączyć do procesu aplikacji uruchomionych w emulatorze, po uruchomieniu aplikacji. Wybieranie **wersji** tryb wyłącza debugera (ale nadal można uruchomić instrukcje dziennik aplikacji i używana do debugowania). Po wybraniu urządzenia wirtualnego z menu rozwijanego urządzenia, wybierz opcję **debugowania** lub **wersji** trybie, a następnie kliknij przycisk Odtwórz, aby uruchomić aplikację:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Debug i Release tryby, przycisk Odtwórz](debug-on-emulator-images/win/17-debug-release-sml.png)](debug-on-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Debug i Release tryby, przycisk Odtwórz](debug-on-emulator-images/mac/16-debug-release-sml.png)](debug-on-emulator-images/mac/16-debug-release.png#lightbox)

-----

Po uruchomieniu emulatora platformy Xamarin.Android nastąpi wdrożenie aplikacji na emulatorze. Emulator uruchamia aplikację z obrazem konfigurację urządzenia wirtualnego. Zrzut ekranu emulatora systemu Android jest wyświetlony poniżej. W tym przykładzie jest to emulator działa pustą aplikację o nazwie **moja_aplikacja**:

![Pusta aplikacja uruchomionym emulatorem](debug-on-emulator-images/emulator-running.png)

Emulator może pozostać uruchomione: nie jest konieczne zamknąć ją i oczekuje na ponowne uruchomienie aplikacji zawsze jest uruchomiony. Podczas pierwszego uruchomienia aplikacji platformy Xamarin.Android w emulatorze, środowiska wykonawczego platformy Xamarin.Android udostępnione dla poziomu docelowych interfejsu API jest zainstalowany, wraz z nazwą aplikacji. Instalacja środowiska uruchomieniowego może zająć kilka minut, więc prosimy o cierpliwość. Instalacja środowiska uruchomieniowego odbywa się tylko wtedy, gdy emulator wdrożeniu pierwszej aplikacji platformy Xamarin.Android &ndash; kolejne wdrożenia są szybsze, ponieważ tylko aplikacja jest kopiowany do emulatora.

<a name="quick-boot" />

## <a name="quick-boot"></a>Szybkie rozruchu

Nowszych wersji emulatora systemu Android obejmują funkcję _szybkie rozruchu_ który uruchamia emulatora w zaledwie kilka sekund. Po zamknięciu emulator pobiera migawkę stanu urządzenia wirtualnego, aby go można szybko przywrócić z tego stanu po ponownym uruchomieniu.
Aby uzyskać dostęp do tej funkcji, potrzebne następujące elementy:

-   Emulator systemu android w wersji 27.0.2 lub nowszej
-   Wersja narzędzia zestawu SDK systemu android 26.1.1 lub nowszy

Po zainstalowaniu wersji wymienionych powyżej emulatora i narzędzia zestawu SDK funkcja szybkiego rozruchu jest domyślnie włączona. 

Podczas pierwszego uruchomienia zimnych urządzenia wirtualnego odbywa się bez poprawy szybkości, ponieważ nie ma jeszcze utworzyć migawki:

![Zimnych rozruchu zrzut ekranu](debug-on-emulator-images/cold-boot.png)

Po zakończeniu poza emulator szybkie rozruchu zapisuje stan emulatora migawki:

![Zapisywanie stanu podczas zamykania](debug-on-emulator-images/saving-state.png)

Uruchamia kolejne urządzenie wirtualne jest znacznie szybsze, ponieważ emulator po prostu przywraca stan jaką zamknięte emulator.

![Podczas ładowania stanu po ponownym uruchomieniu komputera](debug-on-emulator-images/loading-state.png)


## <a name="troubleshooting"></a>Rozwiązywanie problemów

Porady i rozwiązania typowych problemów z emulatora, zobacz [Rozwiązywanie problemów z systemem Android Emulator](~/android/get-started/installation/android-emulator/troubleshooting.md).


## <a name="summary"></a>Podsumowanie

W tym przewodniku wyjaśniono proces konfigurowania emulatora systemu Android do uruchomienia i testowania aplikacji platformy Xamarin.Android. Go opisane kroki dotyczące uruchamiania emulatora za pomocą wstępnie skonfigurowanych urządzeń wirtualnych, a zapewniała kroki dotyczące wdrażania aplikacji na emulatorze z programu Visual Studio. 

Aby uzyskać więcej informacji o korzystaniu z emulatora systemu Android zobacz następujące tematy Android Developer:

-   [Na ekranie](https://developer.android.com/studio/run/emulator.html#navigate)

-   [Wykonywanie podstawowych zadań w emulatorze](https://developer.android.com/studio/run/emulator.html#tasks)

-   [Praca z rozszerzonego kontrolki, ustawienia i pomocy](https://developer.android.com/studio/run/emulator.html#extended)

-   [Uruchamianie emulatora szybkie rozruchu](https://developer.android.com/studio/run/emulator#quickboot)
