---
title: "Zmiany do narzędzi zestawu SDK systemu Android"
description: "Zmiany w zarządzaniu zestawu SDK systemu Android zainstalowane poziomy interfejsu API i urządzeń Avd."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 69e9f08870a01c056951700978d07277af5edfa8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="changes-to-the-android-sdk-tooling"></a>Zmiany do narzędzi zestawu SDK systemu Android

_Zmiany w zarządzaniu zestawu SDK systemu Android zainstalowane poziomy interfejsu API i urządzeń Avd._

## <a name="changes-to--android-sdk-tooling"></a>Zmiany do narzędzi zestawu SDK systemu Android

W nowszych wersjach narzędzia zestawu SDK dla systemu Android, firma Google usunęła istniejących menedżerów AVD i zestaw SDK na rzecz nowych _interfejsu wiersza polecenia_ narzędzi (CLI). Pierwsza **android** program został usunięty i menedżerów graficznego interfejsu użytkownika (graficznego interfejsu użytkownika) w programie Visual Studio for Mac i starszych wersji programu Xamarin dla Visual Studio nie będzie dłużej działać poza narzędzia zestawu SDK systemu Android w wersji.


![Android menu IDE w programie Visual Studio](sdk-cli-tooling-changes-images/android-ide-menu.png)

Podjęto próbę użycia **android** program za pomocą wiersza polecenia spowoduje komunikat o błędzie, podobnie do następującej:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

W związku z tym należy użyć narzędzi interfejsu wiersza polecenia do zarządzania i aktualizowanie emulatorów i zestawu SDK systemu Android.

### <a name="cli-tools"></a>Narzędzi interfejsu wiersza polecenia

Następujące programy tworzą interfejsu wiersza polecenia dla narzędzia zestawu SDK systemu Android teraz:

#### <a name="sdkmanager"></a>sdkmanager

**Dodane w:** narzędzia zestawu SDK systemu Android 25.2.3 (listopada 2016 roku) lub nowszy.

Jest nowy program o nazwie **sdkmanager** w **narzędzia/bin** folderu zestawu SDK systemu Android. To narzędzie jest używane do obsługi zestawu SDK systemu Android w wierszu polecenia. Aby uzyskać więcej informacji na temat stosowania tego narzędzia, zobacz [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Dodane w:** narzędzia zestawu SDK systemu Android 25.3.0 (marzec 2017 r) lub nowszy.

Jest nowy program o nazwie **avdmanager** w **narzędzia/bin** folderu zestawu SDK systemu Android. To narzędzie służy do obsługi AVD dla Emulator systemu Google Android. Aby uzyskać więcej informacji na temat stosowania tego narzędzia, zobacz [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Zmiana wersji na starszą

Możliwość korzystania z **narzędzia zestawu SDK systemu Android** wersję za pomocą Instalowanie starszej wersji zestawu SDK systemu Android z [witryny dla deweloperów systemu Android](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>Przy użyciu starego graficznego interfejsu użytkownika

Można nadal używać oryginalnego graficznego interfejsu użytkownika, uruchamiając **android** program wewnątrz sieci **narzędzia** tak długo, jak znajduje się w folderze **narzędzia zestawu SDK systemu Android** wersji **25.2.5**  lub niższej.


## <a name="related-links"></a>Linki pokrewne

- [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md)
- [Opis poziomów interfejsu API systemu Android](~/android/app-fundamentals/android-api-levels.md)
- [Informacje o wersji (Google) narzędzia zestawu SDK](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
