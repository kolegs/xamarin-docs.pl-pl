---
title: Debugowanie zużycie emulatora systemu Android
description: Te artykuły wyjaśniono, jak debugowanie aplikacji na emulatorze platformy Xamarin.Android zużycia.
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2018
ms.openlocfilehash: baa8df87caf2c05d7b6202d5160c930e51656e10
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36934983"
---
# <a name="debug-android-wear-on-an-emulator"></a>Debugowanie zużycie emulatora systemu Android

_Te artykuły wyjaśniono, jak debugowanie aplikacji na emulatorze platformy Xamarin.Android zużycia._

## <a name="debug-wear-on-emulator-overview"></a>Debugowanie zużycie emulatora — omówienie

Tworzenie aplikacji systemu Android nosić wymaga działania aplikacji, albo na sprzęcie fizycznym lub przy użyciu emulatorem ani symulatorem. Przy użyciu sprzętu jest najlepszym rozwiązaniem, ale nie zawsze najbardziej praktyczne. W wielu przypadkach może być łatwiejsze i bardziej ekonomiczne symulować/emulować sprzętu dla systemu Android nosić przy użyciu emulatora, zgodnie z poniższym opisem. Jeśli nie są jeszcze zapoznać się z procesem wdrażania i uruchamiania aplikacji systemu Android nosić, zobacz [nosić Witaj,](~/android/wear/get-started/hello-wear.md).

## <a name="configure-the-android-emulator"></a>Skonfiguruj emulatora systemu Android

Aby uruchomić aplikację zużycia emulatora, należy zainstalować emulatora Android SDK systemu Android i jest skonfigurowana do używania systemu Android. Uzyskać ogólną emulatora Android SDK instalacji i konfiguracji, zobacz [Konfiguracja emulatora systemu Android](~/android/get-started/installation/android-emulator/index.md).

Podczas tworzenia zużycia urządzenia wirtualnego, wybierz profil urządzenia Android nosić (takich jak **Android zużycia kwadratowe**). Zwiększa wydajność, użyj zużycia **x86** procesora CPU/ABI, jak pokazano w poniższym przykładzie:

[![Przykładowa konfiguracja urządzenia wirtualnego zużycie](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)


## <a name="launch-the-wear-virtual-device"></a>Uruchamianie zużycia urządzenia wirtualnego 

Po utworzeniu urządzenia wirtualnego systemu Android nosić, można ją z menu rozwijanego urządzenia w IDE przed rozpoczęciem debugowania. Jeśli urządzenia wirtualnego nie jest dostępna w urządzeniu rozwijanego, sprawdź, czy projekt Android *nosić* projektu aplikacji (nie projekt aplikacji systemu Android) oraz że jego docelowy poziom interfejsu API jest ustawiona na tego samego interfejsu API na poziomie co urządzenie wirtualne. Na przykład:

[![Wybranie w menu programu Visual Studio urządzenia AVD nosić](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Po uruchomieniu emulatora systemu Android Xamarin.Android nastąpi wdrożenie aplikacji zużycia na emulator. Emulator uruchamia aplikację z obrazem konfigurację urządzenia wirtualnego.

Nie można zaskoczeniem, jeśli widzisz taki (lub inny ekran międzysegmentowe) na początku. Emulator czujki może zająć trochę czasu, aby uruchomić: 

![Obejrzyj emulatora są wyświetlane tylko kilka minut...](debug-on-emulator-images/please-wait.png)

Emulator może pozostać uruchomiony; nie jest konieczne zamknąć ją i uruchom go ponownie na każdym razem, gdy aplikacja jest uruchamiana.

 
## <a name="summary"></a>Podsumowanie
 
W tym przewodniku wyjaśniono sposób konfigurowania emulatora systemu Android dla rozwoju zużycia i uruchomić urządzenie wirtualne zużycia do debugowania.
