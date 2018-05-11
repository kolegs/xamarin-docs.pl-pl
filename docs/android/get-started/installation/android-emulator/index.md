---
title: Ustawienia emulatora systemu android
description: W tej sekcji opisano sposób przygotowania Emulator systemu Google Android do testowania aplikacji. Wyjaśniono, jak przyspieszanie emulatora o maksymalnej wydajności i przedstawia sposób tworzenia i dostosowywania urządzeń wirtualnych przy użyciu Menedżera emulatora.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/25/2018
ms.openlocfilehash: 215e298068b7a3a23b2e469e923f172c8303bbcb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="android-emulator-setup"></a>Ustawienia emulatora systemu android

_W tej sekcji opisano sposób przygotowania Emulator systemu Google Android do testowania aplikacji. Wyjaśniono, jak przyspieszanie emulatora o maksymalnej wydajności i przedstawia sposób tworzenia i dostosowywania urządzeń wirtualnych przy użyciu Menedżera emulatora._


## <a name="overview"></a>Omówienie

W wielu konfiguracjach, aby symulować różnych urządzeń można uruchomić emulatora Google Android SDK. Każdy z tych konfiguracji jest tworzony jako _urządzenia wirtualnego_. W tym przewodniku dowiesz się, jak przyspieszanie emulatora systemu Android w celu poprawy wydajności i tworzenie urządzenia wirtualnego za pomocą platformy Xamarin Android Emulator Manager lub starszej wersji Menedżera emulatora Google.


> [!NOTE]
> Począwszy od wersji narzędzia zestawu SDK systemu Android **26.0.1** i później, firma Google usunęła obsługę istniejącego menedżera AVD/pakiet SDK na rzecz ich nowych narzędzi interfejsu wiersza polecenia (interfejsu wiersza polecenia). Z powodu tej zmiany amortyzacja Xamarin zestawu SDK lub urządzeniu menedżerów są teraz używane zamiast menedżerów SDK/emulatora Google dla narzędzi dla systemu Android 26.0.1 i nowszych. (Aby uzyskać więcej informacji o Menedżerze zestawu SDK platformy Xamarin, zobacz [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md)).


## <a name="sections"></a>Sekcje

### <a name="hardware-accelerationandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Przyspieszanie sprzętowe](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Jak przygotować komputer do maksymalnej wydajności Emulator systemu Google Android. Ponieważ Emulator systemu Google Android mogą być zbyt wolno bez przyspieszenia sprzętowego, zaleca się włączenie przyspieszanie sprzętowe na komputerze przed użyciem Emulator systemu Google Android.

### <a name="xamarin-android-device-managerandroidget-startedinstallationandroid-emulatorxamarin-device-managermd"></a>[Menedżer urządzeń Xamarin Android](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)

Jak tworzyć i dostosowywać urządzeń wirtualnych Emulator systemu Google Android przy użyciu Menedżera urządzeń Android Xamarin. **Menedżera urządzeń Xamarin Android**, obecnie w wersji zapoznawczej, ma zastąpić starszego Menedżera emulatora Google. Jeśli ma być przeznaczona dla systemu Android Oreo 8.0 lub później, należy użyć Menedżera urządzeń Xamarin Android zamiast Menedżera emulatora Google.

### <a name="google-emulator-managerandroidget-startedinstallationandroid-emulatorgoogle-emulator-managermd"></a>[Menedżer emulatora Google](~/android/get-started/installation/android-emulator/google-emulator-manager.md)

Jak tworzyć i dostosowywać urządzeń wirtualnych Emulator systemu Google Android przy użyciu starszej wersji Menedżera emulatora Google. Możesz kontynuować uruchamianie Emulator systemu Google Android przy użyciu oryginalnego Google emulatora Menedżera poprzez na wersję narzędzia zestawu SDK systemu Android 25.2.5 lub niższy.

Po skonfigurowaniu emulatora Android SDK, zobacz [Emulator systemu Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md) informacji o sposobie uruchamiania emulatora i użyć jej do testowania i debugowania aplikacji.
