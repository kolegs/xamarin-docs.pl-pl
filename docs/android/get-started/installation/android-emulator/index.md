---
title: Ustawienia emulatora systemu android
description: W wielu konfiguracjach, aby symulować różnych urządzeń można uruchomić emulatora systemu Android. W tym przewodniku opisano sposób przygotowania emulatora systemu Android do testowania aplikacji.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: f281227ae6ee17548e9c4653d52c7ae6d2bfff2d
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935038"
---
# <a name="android-emulator-setup"></a>Ustawienia emulatora systemu android

_W tym przewodniku opisano sposób przygotowania emulatora systemu Android do testowania aplikacji._


## <a name="overview"></a>Omówienie

W wielu konfiguracjach, aby symulować różnych urządzeń można uruchomić emulatora systemu Android. Każdy konfiguracja jest nazywana _urządzenia wirtualnego_. Wdrażanie i testowanie aplikacji w emulatorze, wybierz wstępnie skonfigurowane lub niestandardowych urządzenia wirtualnego, która symuluje fizycznych Android urządzeniami, takimi jak telefon węzła lub pikseli.

Sekcje wymienione poniżej opisano, jak w celu przyspieszenia emulatora systemu Android dla maksymalnej wydajności, jak tworzyć i dostosowywać urządzeń wirtualnych przy użyciu Menedżera urządzeń systemu Android i dostosowywanie właściwości profilu urządzenia wirtualnego. Ponadto sekcji dotyczącej rozwiązywania problemów opisano typowe problemy z emulatora i obejścia.

## <a name="sections"></a>Sekcje

### <a name="hardware-acceleration-for-emulator-performanceandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Przyspieszanie sprzętowe emulatora wydajności](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Jak przygotować komputer do maksymalnej wydajności emulatora systemu Android.
Ponieważ emulatora systemu Android mogą być zbyt wolno bez przyspieszenia sprzętowego, zaleca się włączyć przyspieszanie sprzętowe na komputerze przed skorzystaniem z emulatorem.

### <a name="managing-virtual-devices-with-the-android-device-managerandroidget-startedinstallationandroid-emulatordevice-managermd"></a>[Zarządzanie urządzeniami wirtualnego przy użyciu Menedżera urządzeń systemu Android](~/android/get-started/installation/android-emulator/device-manager.md)

Jak tworzyć i dostosowywać urządzeń wirtualnych przy użyciu Menedżera urządzeń systemu Android.

### <a name="editing-android-virtual-device-propertiesandroidget-startedinstallationandroid-emulatordevice-propertiesmd"></a>[Edytowanie właściwości urządzenia wirtualnego systemu Android](~/android/get-started/installation/android-emulator/device-properties.md)

Jak edytować właściwości profilu urządzenia wirtualnego za pomocą Menedżera urządzeń systemu Android.

### <a name="android-emulator-troubleshootingandroidget-startedinstallationandroid-emulatortroubleshootingmd"></a>[Rozwiązywanie problemów z emulatora systemu android](~/android/get-started/installation/android-emulator/troubleshooting.md)

W tym artykule najbardziej typowe komunikaty ostrzegawcze i problemy występujące podczas uruchamiania emulatora systemu Android są opisane, wraz z rozwiązania i wskazówki.

Po skonfigurowaniu emulatora systemu Android, zobacz [debugowania na emulatorze systemu Android](~/android/deploy-test/debugging/debug-on-emulator.md) informacji o sposobie uruchamiania emulatora i użyć jej do testowania i debugowania aplikacji.


> [!NOTE]
> Począwszy od wersji narzędzia zestawu SDK systemu Android **26.0.1** i później, firma Google usunęła obsługę istniejącego menedżera AVD/pakiet SDK na rzecz ich nowych narzędzi interfejsu wiersza polecenia (interfejsu wiersza polecenia). Z powodu tej zmiany amortyzacja Xamarin zestawu SDK lub urządzeniu menedżerów są teraz używane zamiast menedżerów zestawu SDK lub urządzeniu Google dla narzędzi dla systemu Android 26.0.1 i nowszych. Aby uzyskać więcej informacji o Menedżerze zestawu SDK platformy Xamarin, zobacz [Definiowanie zestawu Android SDK dla platformy Xamarin.Android](~/android/get-started/installation/android-sdk.md).

