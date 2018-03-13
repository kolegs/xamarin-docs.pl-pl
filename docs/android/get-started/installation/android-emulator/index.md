---
title: Ustawienia emulatora systemu android
description: "W tej sekcji opisano sposób przygotowania emulatora Android SDK do testowania aplikacji. Wyjaśniono, jak przyspieszanie emulatora o maksymalnej wydajności i przedstawia sposób tworzenia i dostosowywania urządzeń wirtualnych przy użyciu Menedżera emulatora."
ms.topic: article
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/25/2018
ms.openlocfilehash: 55f5cf22718713fdcf11c49e0993f47c2f5a6f1d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="android-emulator-setup"></a>Ustawienia emulatora systemu android

_W tej sekcji opisano sposób przygotowania emulatora Android SDK do testowania aplikacji. Wyjaśniono, jak przyspieszanie emulatora o maksymalnej wydajności i przedstawia sposób tworzenia i dostosowywania urządzeń wirtualnych przy użyciu Menedżera emulatora._


## <a name="overview"></a>Omówienie

W wielu konfiguracjach, aby symulować różnych urządzeń można uruchomić emulatora Google Android SDK. Każdy z tych konfiguracji jest tworzony jako _urządzenia wirtualnego_. W tym przewodniku dowiesz się, jak przyspieszanie emulatora systemu Android w celu poprawy wydajności i tworzenie urządzenia wirtualnego za pomocą platformy Xamarin Android Emulator Manager lub starszej wersji Menedżera emulatora Google.


> [!NOTE]
> Począwszy od wersji narzędzia zestawu SDK systemu Android **26.0.1** i później, firma Google usunęła obsługę istniejącego menedżera AVD/pakiet SDK na rzecz ich nowych narzędzi interfejsu wiersza polecenia (interfejsu wiersza polecenia). Z powodu tej zmiany amortyzacja Xamarin zestawu SDK lub urządzeniu menedżerów są teraz używane zamiast menedżerów SDK/emulatora Google dla narzędzi dla systemu Android 26.0.1 i nowszych. (Aby uzyskać więcej informacji o Menedżerze zestawu SDK platformy Xamarin, zobacz [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md)).


## <a name="sections"></a>Sekcje

### <a name="hardware-accelerationandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Przyspieszanie sprzętowe](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Jak przygotować komputer do maksymalnej wydajności emulatora Android SDK. Ponieważ emulatora Android SDK może być zbyt wolno bez przyspieszenia sprzętowego, zaleca się włączyć przyspieszanie sprzętowe na komputerze przed użyciem emulatora Android SDK.

### <a name="xamarin-android-device-managerandroidget-startedinstallationandroid-emulatorxamarin-device-managermd"></a>[Menedżer urządzeń Xamarin Android](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)

Jak tworzyć i dostosowywać emulatora Android SDK urządzeń wirtualnych przy użyciu Menedżera urządzeń Android Xamarin. **Menedżera urządzeń Xamarin Android**, obecnie w wersji zapoznawczej, ma zastąpić starszego Menedżera emulatora Google. Jeśli ma być przeznaczona dla systemu Android Oreo 8.0 lub później, należy użyć Menedżera urządzeń Xamarin Android zamiast Menedżera emulatora Google.

### <a name="google-emulator-managerandroidget-startedinstallationandroid-emulatorgoogle-emulator-managermd"></a>[Menedżer emulatora Google](~/android/get-started/installation/android-emulator/google-emulator-manager.md)

Jak tworzyć i dostosowywać emulatora Android SDK urządzeń wirtualnych przy użyciu starszej wersji Menedżera emulatora Google. Możesz kontynuować uruchamianie Emulator systemu Google Android przy użyciu oryginalnego Google emulatora Menedżera poprzez na wersję narzędzia zestawu SDK systemu Android 25.2.5 lub niższy.

Po skonfigurowaniu emulatora Android SDK, zobacz [emulatora Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md) informacji o sposobie uruchamiania emulatora i użyć jej do testowania i debugowania aplikacji.
