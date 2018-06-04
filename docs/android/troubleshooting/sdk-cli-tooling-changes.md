---
title: Zmiany do narzędzi zestawu SDK systemu Android
description: Zmiany w zarządzaniu zestawu SDK systemu Android zainstalowane poziomy interfejsu API i urządzeń Avd.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: b0d9458238c4b3ac9ceeeb7d7ce4e2ca8b0b6de3
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732869"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Zmiany do narzędzi zestawu SDK systemu Android

_Zmiany w zarządzaniu zestawu SDK systemu Android zainstalowane poziomy interfejsu API i urządzeń Avd._

## <a name="changes-to-android-sdk-tooling"></a>Zmiany do narzędzi zestawu SDK systemu Android

W nowszych wersjach narzędzia zestawu SDK dla systemu Android Firma Google usunęła istniejącego menedżera AVD i zestaw SDK na rzecz nowych narzędzi interfejsu wiersza polecenia (interfejsu wiersza polecenia). **Android** program został usunięty i nie będzie działać menedżerów Google graficznego interfejsu użytkownika (graficznego interfejsu użytkownika) w programie Visual Studio for Mac i starsze wersje programu Visual Studio Tools for Xamarin wcześniejszą wersję 25.2.5 narzędzia zestawu SDK systemu Android. Na przykład podjęto próbę użycia **android** program za pomocą wiersza polecenia spowoduje komunikat o błędzie, podobnie do następującej:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

W poniższych sekcjach opisano sposób zarządzania zestawu SDK systemu Android i wirtualnych urządzeń z systemem Android przy użyciu zestawu SDK systemu Android 25.3.0 i nowszych.

### <a name="ui-tools"></a>Narzędzia interfejsu użytkownika

Visual Studio i Visual Studio for Mac zawierają teraz Xamarin zastępujące wycofane menedżerów Google graficznego interfejsu użytkownika:

-   Aby pobrać narzędzia zestawu SDK systemu Android, platform i inne składniki, które są potrzebne do tworzenia aplikacji platformy Xamarin.Android, należy użyć [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md) zamiast starszy Menedżer SDK Google.

-   Aby utworzyć i skonfigurować urządzeń wirtualnych z systemem Android, należy użyć [Menedżera urządzeń Android](~/android/get-started/installation/android-emulator/device-manager.md) zamiast starszych Menedżera emulatora Google.

Te narzędzia są taką samą funkcję jak Google graficzny interfejs menedżerów zastępują one.

### <a name="cli-tools"></a>Narzędzi interfejsu wiersza polecenia

Alternatywnie można użyć interfejsu wiersza polecenia narzędzia do zarządzania i aktualizowanie emulatorów i zestawu SDK systemu Android. Następujące programy tworzą interfejsu wiersza polecenia dla narzędzia zestawu SDK systemu Android teraz:

#### <a name="sdkmanager"></a>sdkmanager

**Dodane w:** narzędzia zestawu SDK systemu Android 25.2.3 (listopada 2016 roku) lub nowszy.

Jest nowy program o nazwie **sdkmanager** w **narzędzia/bin** folderu zestawu SDK systemu Android. To narzędzie jest używane do obsługi zestawu SDK systemu Android w wierszu polecenia. Aby uzyskać więcej informacji na temat stosowania tego narzędzia, zobacz [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Dodane w:** narzędzia zestawu SDK systemu Android 25.3.0 (marzec 2017 r) lub nowszy.

Jest nowy program o nazwie **avdmanager** w **narzędzia/bin** folderu zestawu SDK systemu Android. To narzędzie jest używane do obsługi urządzeń Avd dla Emulator systemu Google Android. Aby uzyskać więcej informacji na temat stosowania tego narzędzia, zobacz [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Zmiana wersji na starszą

Możliwość korzystania z **narzędzia zestawu SDK systemu Android** wersję za pomocą Instalowanie starszej wersji zestawu SDK systemu Android z [witryny dla deweloperów systemu Android](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>Przy użyciu starego graficznego interfejsu użytkownika

Można nadal używać oryginalnego graficznego interfejsu użytkownika, uruchamiając **android** program wewnątrz sieci **narzędzia** tak długo, jak znajduje się w folderze **narzędzia zestawu SDK systemu Android** wersji **25.2.5**  lub niższej.


## <a name="related-links"></a>Linki pokrewne

- [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md)
- [Menedżer urządzeń systemu android](~/android/get-started/installation/android-emulator/device-manager.md)
- [Opis poziomów interfejsu API systemu Android](~/android/app-fundamentals/android-api-levels.md)
- [Informacje o wersji (Google) narzędzia zestawu SDK](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
