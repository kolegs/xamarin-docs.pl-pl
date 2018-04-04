---
title: Debugowanie
description: Jak do testowania i debugowania aplikacji platformy Xamarin.Android
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/16/2018
ms.openlocfilehash: 429a369ddcd11829920f9fb932a737d1a53cec10
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="debugging"></a>Debugowanie

## <a name="debugging-overview"></a>Informacje o debugowaniu

Tworzenie aplikacji systemu Android wymaga działania aplikacji, albo na sprzęcie fizycznym lub przy użyciu emulatorem ani symulatorem. Przy użyciu sprzętu jest najlepszym rozwiązaniem, ale nie zawsze najbardziej praktyczne. W wielu przypadkach może być łatwiejsze i bardziej ekonomiczne symulować/emulować sprzętu z systemem Android przy użyciu jednej z emulatory opisane poniżej.


### <a name="android-sdk-emulatorandroiddeploy-testdebuggingandroid-sdk-emulatorindexmd"></a>[Emulator zestawu SDK systemu Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

Artykuły te wyjaśniają zasady korzystania z emulatorem domyślnym, który został dostarczony wraz z zestawu SDK systemu Android. Emulator tego jest dostępna dla programu Visual Studio dla systemu Windows i programu Visual Studio dla komputerów Mac.

### <a name="visual-studio-android-emulatorandroiddeploy-testdebuggingvisual-studio-android-emulatormd"></a>[Emulator programu Visual Studio dla systemu Android](~/android/deploy-test/debugging/visual-studio-android-emulator.md)

W tym artykule opisano sposób debugowania i testowania aplikacji platformy Xamarin.Android przy użyciu emulatora systemu Android, które są wbudowane w program Visual Studio 2015. Emulator tego jest dobrym rozwiązaniem, jeśli używasz programu Visual Studio 2015 i nie ma potrzeby profilów urządzeń niestandardowych.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[Debugowanie na urządzeniu](~/android/deploy-test/debugging/debug-on-device.md)

W tym artykule przedstawiono sposób konfigurowania fizycznego urządzenia z systemem Android, dzięki czemu można wdrożyć aplikacji platformy Xamarin.Android do niego bezpośrednio z programu Visual Studio lub Visual Studio lub komputerów Mac.

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Dziennik debugowania systemu Android](~/android/deploy-test/debugging/android-debug-log.md)

Jeden często deweloperzy lewy umożliwia debugowanie aplikacji przy użyciu `Console.WriteLine`. Jednak na platformie przenośnych, takich jak Android nie konsoli. Urządzenia z systemem android zawiera dziennik, który prawdopodobnie należy wykorzystać podczas pisania aplikacji. Jest to czasami określane jako **logcat** z powodu polecenie wpisane w celu ich pobrania. W tym artykule opisano sposób użycia **logcat**.

> [!WARNING]
> Należy pamiętać, że **Xamarin Android Player** jest przestarzała. Aby uzyskać więcej informacji, zobacz [anonsu, w tym wpisie w blogu](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/).
