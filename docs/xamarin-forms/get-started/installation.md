---
title: Wymagania dotyczące platformy Xamarin.Forms
description: Platforma i rozwoju wymagania systemowe dla platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: 75e6d25f95a0a3f18c83fe73f67ad4a7797f0924
ms.sourcegitcommit: c024f29ff730ae20c15e99bfe0268a0e1c9d41e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2018
ms.locfileid: "34470333"
---
# <a name="xamarinforms-requirements"></a>Wymagania dotyczące platformy Xamarin.Forms

_Platforma i rozwoju wymagania systemowe dla platformy Xamarin.Forms._

Zapoznaj się [instalacji](~/cross-platform/get-started/installation/index.md) artykuł, aby zapoznać się z omówieniem instalacji i konfiguracji rozwiązania, które są stosowane na platformach.

## <a name="target-platforms"></a>Platformy docelowe

Aplikacji platformy Xamarin.Forms mogą być napisane dla następujących systemów operacyjnych:

- System iOS 8 lub nowszy
- Android 4.0.3 (API 15) lub nowszy ([szczegółowe](#android))
- Platforma uniwersalna systemu Windows 10 systemu Windows ([szczegółowe](#windows10))

Zakłada się, że deweloperzy mają znajomość [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) i [udostępnionych projektów](~/cross-platform/app-fundamentals/shared-projects.md).

### <a name="additional-platform-support"></a>Obsługa dodatkowych platform.

Stan tych platform jest dostępna w [GitHub platformy Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support):

- Samsung Tizen
- macOS
- GTK#
- WPF

### <a name="platforms-from-earlier-versions"></a>Platformy z wcześniejszymi wersjami

Tych platform nie są obsługiwane w przypadku korzystania z platformy Xamarin.Forms 3.0:

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*

### <a name="android"></a>Android

Powinien mieć najnowsze narzędzia zestawu SDK systemu Android i interfejs API systemu Android platforma jest zainstalowana. Można aktualizować do najnowszych wersji przy użyciu [Android SDK Manager](~/android/get-started/installation/android-sdk.md).

Ponadto wersji docelowej/kompilacji dla projektów w systemie Android **musi** można ustawić *Użyj zainstalowana najnowsza wersja platformy*. Jednak minimalna wersja może należeć do interfejsu API 15 dzięki czemu można kontynuować do obsługi urządzeń, które używają systemu Android to 4.0.3 i nowszych. Te wartości są ustawiane **opcje projektu**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Opcje projektu > aplikacji > właściwości aplikacji**

![](installation-images/options-android-vs-sml.png "Opcje kompilacji systemu android w programie Visual Studio")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**Tworzenie > Ogólne**

![](installation-images/options-general-sml.png "Tworzenie > Ogólne")

**Tworzenie > aplikacji systemu Android**

![](installation-images/options-android-sml.png "Tworzenie > aplikacji systemu Android")

-----

## <a name="development-system-requirements"></a>Wymagania systemowe programowanie

System macOS i systemu Windows mogą być opracowane aplikacji platformy Xamarin.Forms. Jednak systemu Windows i programu Visual Studio są wymagane do tworzenia aplikacji wersji systemu Windows.

## <a name="mac-system-requirements"></a>Wymagania systemowe dla komputerów Mac

Visual Studio for Mac można użyć do opracowywania aplikacji platformy Xamarin.Forms na OS X El Capitan (10.11) lub nowszej. Do opracowywania aplikacji systemu iOS, firma Microsoft zaleca, o co najmniej 10 SDK i 8 Xcode zainstalowane z systemem iOS.

> [!NOTE]
>  Aplikacje systemu Windows nie może opracowany na macOS.

## <a name="windows-system-requirements"></a>Wymagania dotyczące systemu Windows

Aplikacje platformy Xamarin.Forms dla systemów iOS i Android mogą być wbudowane w żadnej instalacji systemu Windows, która obsługuje programowanie Xamarin. Wymaga programu Visual Studio 2017 lub nowszej działa w systemie Windows 7 lub nowszej. Mac sieci jest wymagana dla opracowywania aplikacji systemu iOS.

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>Platforma uniwersalna systemu Windows (UWP)

Tworzenie aplikacji platformy Xamarin.Forms dla platformy uniwersalnej systemu Windows wymaga:

- Windows 10 (aktualizacja twórców spadek zalecane)

- Visual Studio 2017

- [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

Projekty platformy UWP znajdują się w rozwiązania platformy Xamarin.Forms utworzone w programie Visual Studio 2017, ale nie rozwiązania utworzone w programie Visual Studio dla komputerów Mac.
Możesz [dodać Windows platformy Uniwersalnej aplikacji](~/xamarin-forms/platform/windows/installation/index.md) do istniejącego rozwiązania platformy Xamarin.Forms w dowolnym momencie.
