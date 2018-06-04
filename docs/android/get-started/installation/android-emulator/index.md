---
title: Instalator Emulator systemu Google Android
description: Emulator systemu Google Android można uruchomić w wielu konfiguracjach, aby symulować różnych urządzeń. W tym przewodniku opisano sposób przygotowania emulatora systemu Android do testowania aplikacji.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: e5ba2cc23ea9751ca60644d3eb5b7e3f31bbb6bb
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732534"
---
# <a name="google-android-emulator-setup"></a>Instalator Emulator systemu Google Android

_W tym przewodniku opisano sposób przygotowania Emulator systemu Google Android do testowania aplikacji._


## <a name="overview"></a>Omówienie

Emulator systemu Google Android można uruchomić w wielu konfiguracjach, aby symulować różnych urządzeń. Każdy konfiguracja jest nazywana _urządzenia wirtualnego_. Wdrażanie i testowanie aplikacji w emulatorze, wybierz wstępnie skonfigurowane lub niestandardowych urządzenia wirtualnego, która symuluje fizycznych Android urządzeniami, takimi jak telefon węzła lub pikseli.

Sekcje wymienione poniżej opisano, jak w celu przyspieszenia emulator systemu Google Android maksymalną wydajność, jak tworzyć i dostosowywać urządzeń wirtualnych przy użyciu Menedżera urządzeń systemu Android i dostosowywanie właściwości profilu urządzenia wirtualnego. Ponadto sekcji dotyczącej rozwiązywania problemów opisano typowe problemy z instalacją i rozwiązania problemu.

## <a name="sections"></a>Sekcje

### <a name="hardware-acceleration-for-emulator-performanceandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Przyspieszanie sprzętowe emulatora wydajności](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Jak przygotować komputer do maksymalnej wydajności emulatora systemu Android.
Ponieważ Emulator systemu Google Android mogą być zbyt wolno bez przyspieszenia sprzętowego, zaleca się włączyć przyspieszanie sprzętowe na komputerze przed użyciem tego emulatora.

### <a name="managing-virtual-devices-with-the-android-device-managerandroidget-startedinstallationandroid-emulatordevice-managermd"></a>[Zarządzanie urządzeniami wirtualnego przy użyciu Menedżera urządzeń systemu Android](~/android/get-started/installation/android-emulator/device-manager.md)

Jak tworzyć i dostosowywać urządzeń wirtualnych przy użyciu Menedżera urządzeń systemu Android.

### <a name="editing-android-virtual-device-propertiesandroidget-startedinstallationandroid-emulatordevice-propertiesmd"></a>[Edytowanie właściwości urządzenia wirtualnego systemu Android](~/android/get-started/installation/android-emulator/device-properties.md)

Jak edytować właściwości profilu wirtualnego urządzenia z systemem Android za pomocą Menedżera urządzeń systemu Android.

### <a name="troubleshooting-emulator-setup-problemsandroidget-startedinstallationandroid-emulatortroubleshootingmd"></a>[Rozwiązywanie problemów z emulatorem problemy z instalacją](~/android/get-started/installation/android-emulator/troubleshooting.md)

Jak zdiagnozować i rozwiązać problemy Menedżera urządzeń systemu Android, które mogą wystąpić podczas konfigurowania emulatora systemu Android.


Po skonfigurowaniu emulatora systemu Android, zobacz [debugowania za pomocą Emulator systemu Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md) informacji o sposobie uruchamiania emulatora i użyć jej do testowania i debugowania aplikacji.


> [!NOTE]
> Począwszy od wersji narzędzia zestawu SDK systemu Android **26.0.1** i później, firma Google usunęła obsługę istniejącego menedżera AVD/pakiet SDK na rzecz ich nowych narzędzi interfejsu wiersza polecenia (interfejsu wiersza polecenia). Z powodu tej zmiany amortyzacja Xamarin zestawu SDK lub urządzeniu menedżerów są teraz używane zamiast menedżerów zestawu SDK lub urządzeniu Google dla narzędzi dla systemu Android 26.0.1 i nowszych. Aby uzyskać więcej informacji o Menedżerze zestawu SDK platformy Xamarin, zobacz [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md).

