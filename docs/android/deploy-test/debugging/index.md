---
title: Profilowanie platformy Xamarin.Android na urządzeniach i emulatory
description: Jak do testowania i debugowania aplikacji platformy Xamarin.Android
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 1ed6ec57365c5d3a861dd3fd947a2ad195ce5357
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935389"
---
# <a name="debugging"></a>Debugowanie

W tej sekcji omówiono sposób debugowania aplikacji platformy Xamarin.Android na urządzeniach lub emulatory.

## <a name="debugging-overview"></a>Informacje o debugowaniu

Tworzenie aplikacji systemu Android wymaga działania aplikacji, albo na sprzęcie fizycznym lub przy użyciu emulatora. Przy użyciu sprzętu jest najlepszym rozwiązaniem, ale nie zawsze najbardziej praktyczne. W wielu przypadkach może być łatwiejsze i bardziej ekonomiczne symulować/emulować sprzętu z systemem Android przy użyciu jednej z emulatory opisane poniżej.

### <a name="debugging-on-the-android-emulatorandroiddeploy-testdebuggingdebug-on-emulatormd"></a>[Debugowanie na emulatorze systemu Android](~/android/deploy-test/debugging/debug-on-emulator.md)

W tym artykule opisano sposób uruchamiania emulatora systemu Android w programie Visual Studio i uruchom aplikację w urządzenie wirtualne.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[Debugowanie na urządzeniu](~/android/deploy-test/debugging/debug-on-device.md)

W tym artykule przedstawiono sposób konfigurowania fizycznego urządzenia z systemem Android, dzięki czemu można wdrożyć aplikacji platformy Xamarin.Android do niego bezpośrednio z programu Visual Studio lub Visual Studio lub komputerów Mac.

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Dziennik debugowania systemu Android](~/android/deploy-test/debugging/android-debug-log.md)

Jeden często deweloperzy lewy umożliwia debugowanie aplikacji przy użyciu `Console.WriteLine`. Jednak na platformie przenośnych, takich jak Android nie konsoli. Urządzenia z systemem android zawiera dziennik, który prawdopodobnie należy wykorzystać podczas pisania aplikacji. Jest to czasami określane jako **logcat** z powodu polecenie wpisane w celu ich pobrania. W tym artykule opisano sposób użycia **logcat**.

> [!WARNING]
> Należy pamiętać, że **Xamarin Android Player** jest przestarzała. Aby uzyskać więcej informacji, zobacz [anonsu, w tym wpisie w blogu](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/). Ponadto **Visual Studio Emulator systemu Android** jest przestarzałe począwszy od programu Visual Studio 2017 r.
