---
title: Emulator wiersza polecenia
ms.topic: article
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: b8e8ec3de3afccab15d54f666974af678c1b8c33
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="command-line-emulator"></a>Emulator wiersza polecenia


## <a name="running-the-android-emulator-from-the-command-line"></a>Uruchamianie emulatora systemu Android z wiersza polecenia

Aby włączyć uruchamianie emulatora systemu Android z wiersza polecenia, można użyć narzędzia "emulatora", udostępniane przez zestaw SDK systemu Android. To narzędzie można uruchomić emulatora Terminal na OS X lub z wiersza polecenia na komputerze z systemem Windows.

Aby uruchomić określonego emulatora systemu Android, uruchom następujące polecenie z katalogu narzędzia w lokalizacji zestawu SDK systemu android (na przykład C:\android-sdk-windows\tools):

W systemie Windows

```cmd
emulator.exe -avd NameOfYourEmulator -partition-size 512
```

Na macOS

```bash
./emulator -avd NameOfYourEmulator -partition-size 512
```

Przyczyna wymagające rozmiar partycji jest umożliwienie emulator ma wystarczająco dużo miejsca, aby pobrać platformy Xamarin.Android zainstalowany na emulatorze domyślnie jest mały rozmiar emulatora.

Można znaleźć więcej informacji na temat dodatkowych parametrów tutaj — witryny systemu Android [http://developer.android.com/guide/developing/tools/emulator.html](http://developer.android.com/guide/developing/tools/emulator.html)
