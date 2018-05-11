---
title: Uruchomiony Emulator systemu Google Android
description: Debugowanie aplikacji za pomocą Emulator systemu Google Android
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 0e290b24c0d7a98b1abaf647fe76e56867042645
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="running-the-google-android-emulator"></a>Uruchomiony Emulator systemu Google Android

W tym przewodniku dowiesz się, jak uruchomić urządzenie wirtualne w Emulator systemu Google Android do debugowania i testowania aplikacji.

## <a name="using-a-pre-configured-virtual-device"></a>Za pomocą wstępnie skonfigurowanych urządzeń wirtualnych

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio obejmuje wstępnie skonfigurowane urządzeń wirtualnych, które są wyświetlane w menu rozwijanym urządzenia. Na przykład na poniższym zrzucie ekranu Visual Studio 2017 kilka wstępnie skonfigurowanych urządzeń wirtualnych są dostępne:

-   **Visual Studio\_android 23\_arm\_telefonu**

-   **Visual Studio\_android 23\_arm\_typu tablet**

-   **Visual Studio\_android 23\_x86\_telefonu** 

-   **VisualStudio\_android-23\_x86\_tablet** 

[![Urządzenia wirtualne](running-the-emulator-images/win/01-virtual-devices-sml.png)](running-the-emulator-images/win/01-virtual-devices.png#lightbox)

Zazwyczaj należy wybrać **VisualStudio\_android 23\_x86\_phone** urządzenie wirtualne do testowania i debugowania aplikacji telefonicznej. Jeśli jeden z tych wstępnie skonfigurowanych urządzeń wirtualnych odpowiada wymaganiom, (tj. pasuje do elementu docelowego Twojej aplikacji poziom interfejsu API), przejdź do [uruchamianie w emulatorze](#launching) celu rozpoczęcia uruchamiania aplikacji w emulatorze. (Jeśli nie znasz jeszcze z poziomami interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).)

Jeśli projekt platformy Xamarin.Android korzysta z poziomu platformy docelowej, która jest niezgodna z dostępnych urządzeń wirtualnych, menu rozwijane spowoduje wyświetlenie listy bezużyteczne urządzeń wirtualnych w obszarze **nieobsługiwany urządzeń**. Na przykład następujący projekt ma ustawioną platformy docelowej **nugacie 7.1 systemu Android (interfejs API 25)**, który jest niezgodny z **Android 6.0** urządzeń wirtualnych, które znajdują się domyślnie:

[![Niezgodne urządzenie wirtualne](running-the-emulator-images/win/02-incompatible-level-sml.png)](running-the-emulator-images/win/02-incompatible-level.png#lightbox)

Możesz kliknąć **zmienić docelowy z systemem Android co najmniej** Aby zmienić projekt obiektu minimalna wersja systemu Android, aby było poziom interfejsu API dostępnych urządzeń wirtualnych. Alternatywnie można użyć **Android Emulator Manager** do tworzenia nowych urządzeń wirtualnych, które obsługują docelowego interfejsu API poziomu, jak wyjaśniono dalej w [Konfigurowanie urządzenia wirtualnego](#virtualdevice). Aby można było skonfigurować urządzenia wirtualne Nowy poziom interfejsu API, należy najpierw zainstalować odpowiednie obrazów systemu dla tego interfejsu API poziomu &ndash; wyjaśnienie jest zawarte w następnej sekcji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac obejmuje wstępnie skonfigurowane urządzeń wirtualnych, które są wyświetlane w menu rozwijanym urządzenia. Na przykład na poniższym zrzucie ekranu, dostępne są dwa wstępnie skonfigurowanych urządzeń wirtualnych:

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[![Urządzenia wirtualne](running-the-emulator-images/mac/01-virtual-devices-sml.png)](running-the-emulator-images/mac/01-virtual-devices.png#lightbox)

Zazwyczaj należy wybrać **Android\_akcelerowanego\_x86** urządzenie wirtualne do testowania i debugowania aplikacji telefonicznej. Jeśli to wstępnie skonfigurowane wirtualne urządzenie spełnia wymagania dotyczące (tj. pasuje do elementu docelowego Twojej aplikacji poziom interfejsu API), przejdź do [uruchamianie w emulatorze](#launching) celu rozpoczęcia uruchamiania aplikacji w emulatorze. (Jeśli nie znasz jeszcze z poziomami interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).)

-----

## <a name="creating-custom-virtual-devices"></a>Tworzenie niestandardowych urządzeń wirtualnych

Aby utworzyć niestandardowe urządzeń wirtualnych, musi Użyj Menedżera urządzeń Xamarin Android lub starszej wersji Menedżera emulatora Google, który jest częścią zestawu SDK systemu Android. Aby uzyskać więcej informacji na temat tworzenia i dostosowywania urządzeń wirtualnych, zobacz [Menedżera urządzeń Xamarin Android](~/android/get-started/installation/android-emulator/xamarin-device-manager.md).
Jeśli wolisz użyć starszego Menedżera emulatora Google, zobacz [Menedżera emulatora Google](~/android/get-started/installation/android-emulator/google-emulator-manager.md).

Należy pamiętać, że jeśli opracowujesz Oreo 8.0 dla systemu Android za pomocą Menedżera urządzeń Xamarin Android.

<a name="launching" />

## <a name="launching-the-emulator"></a>Uruchamianie emulatora

W górnej części IDE jest menu rozwijanego, który może służyć do wybrania **debugowania** lub **wersji** tryb. Wybieranie **debugowania** dołącza debuger do procesu aplikacji uruchomiony emulator. 

Po wybraniu urządzenia wirtualnego z menu rozwijanego urządzenia, wybierz opcję **debugowania** lub **wersji** tryb, następnie kliknij przycisk **odtwarzanie** przycisk, aby uruchomić aplikację:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Debug i Release tryby, przycisk Odtwórz](running-the-emulator-images/win/17-debug-release-sml.png)](running-the-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Debug i Release tryby, przycisk Odtwórz](running-the-emulator-images/mac/16-debug-release-sml.png)](running-the-emulator-images/mac/16-debug-release.png#lightbox)

-----

Po uruchomieniu emulatora systemu Android platformy Xamarin.Android nastąpi wdrożenie aplikacji na emulatorze. Emulator uruchamia aplikację z obrazem konfigurację urządzenia wirtualnego. Zrzut ekranu z Emulator systemu Google Android jest wyświetlony poniżej (emulator działa pustą aplikację o nazwie **moja_aplikacja**):

![Pusta aplikacja uruchomionym emulatorem](running-the-emulator-images/emulator-running.png)

Emulator może pozostać uruchomiony; nie jest konieczne zamknąć ją i uruchom go ponownie na każdym razem, gdy aplikacja jest uruchamiana. Podczas pierwszego uruchomienia aplikacji platformy Xamarin.Android w emulatorze, środowiska wykonawczego platformy Xamarin.Android udostępnione dla poziomu docelowych interfejsu API jest zainstalowany, wraz z nazwą aplikacji. Instalacja środowiska uruchomieniowego może zająć kilka minut, więc prosimy o cierpliwość. Instalacja środowiska uruchomieniowego odbywa się tylko wtedy, gdy emulator wdrożeniu pierwszej aplikacji platformy Xamarin.Android &ndash; kolejne wdrożenia są szybsze, ponieważ tylko aplikacja jest kopiowany do emulatora.

Aby uzyskać więcej informacji na temat używania Emulator systemu Google Android zobacz następujące tematy Android Developer:

-   [Na ekranie](https://developer.android.com/studio/run/emulator.html#navigate)

-   [Wykonywanie podstawowych zadań w emulatorze](https://developer.android.com/studio/run/emulator.html#tasks)

-   [Praca z rozszerzonego kontrolki, ustawienia i pomocy](https://developer.android.com/studio/run/emulator.html#extended)

