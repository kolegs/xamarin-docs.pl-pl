---
title: Wymagania dotyczące platformy Xamarin.Forms
description: Platforma i rozwoju wymagania systemowe dla platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2017
ms.openlocfilehash: e62c82b351bab759192a4fe879a3b63754cdf0af
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-requirements"></a>Wymagania dotyczące platformy Xamarin.Forms

_Platforma i rozwoju wymagania systemowe dla platformy Xamarin.Forms._

Zapoznaj się [instalacji](~/cross-platform/get-started/installation/index.md) artykuł, aby zapoznać się z omówieniem instalacji i konfiguracji rozwiązania, które są stosowane na platformach.

## <a name="target-platforms"></a>Platformy docelowe

Aplikacji platformy Xamarin.Forms mogą być napisane dla następujących systemów operacyjnych:

-  System iOS 8 lub nowszy
-  Android 4.0.3 (API 15) lub nowszy ([szczegółowe](#android))
-  Platforma uniwersalna systemu Windows 10 systemu Windows ([szczegółowe](#windows10))
-  Windows 8.1 / Windows Phone 8.1 WinRT ([szczegółowe](#windows))
-  *Windows Phone Silverlight 8 (przestarzały)*

Zakłada się, że deweloperzy mają znajomość [przenośnej biblioteki klas](~/cross-platform/app-fundamentals/pcl.md) i [udostępnionych projektów](~/cross-platform/app-fundamentals/shared-projects.md).

<a name="android" />

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


<a name="windows10" />

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Projekty platformy uniwersalnej systemu Windows do systemu Windows 10 nie są dodawane po utworzeniu rozwiązania na macOS. Aby uzyskać instrukcje, jak dodać te projekty do istniejącego rozwiązania, zobacz [Dodawanie Windows platformy Uniwersalnej aplikacji](~/xamarin-forms/platform/windows/installation/universal.md).


<a name="windows" />

### <a name="windows-81--windows-phone-81-winrt"></a>Windows 8.1 / Windows Phone 8.1 WinRT

Windows 8.1 / Windows Phone 8.1 WinRT projekty nie są dodawane po utworzeniu rozwiązania na macOS. Aby uzyskać instrukcje, jak dodać te projekty do istniejącego rozwiązania, zobacz [Dodawanie aplikacji Windows Phone](~/xamarin-forms/platform/windows/installation/phone.md) i [Dodawanie aplikacji dla systemu Windows](~/xamarin-forms/platform/windows/installation/tablet.md).


## <a name="development-system-requirements"></a>Wymagania systemowe programowanie

System macOS i systemu Windows mogą być opracowane aplikacji platformy Xamarin.Forms. Jednak systemu Windows i programu Visual Studio są wymagane do tworzenia aplikacji wersji systemu Windows.

## <a name="mac-system-requirements"></a>Wymagania systemowe Mac

Visual Studio for Mac można użyć do opracowywania aplikacji platformy Xamarin.Forms na OS X El Capitan (10.11) lub nowszej. Do opracowywania aplikacji systemu iOS, firma Microsoft zaleca, o co najmniej 10 SDK i 8 Xcode zainstalowane z systemem iOS.

> [!NOTE]
>  Aplikacje systemu Windows nie może opracowany na macOS.

## <a name="windows-system-requirements"></a>Wymagania dotyczące systemu Windows

Aplikacje platformy Xamarin.Forms dla systemów iOS i Android mogą być wbudowane w żadnej instalacji systemu Windows, która obsługuje programowanie Xamarin. Wymaga programu Visual Studio 2015 lub nowsza działa w systemie Windows 7 lub nowszej. Mac sieci jest wymagana dla opracowywania aplikacji systemu iOS.

### <a name="universal-windows-platform-uwp"></a>Platforma uniwersalna systemu Windows (UWP)

Tworzenie aplikacji platformy Xamarin.Forms dla platformy uniwersalnej systemu Windows wymaga:

* Windows 10 (aktualizacja twórców spadek zalecane)

* Zaleca się programu Visual Studio 2017 r.

* [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

Projekty platformy UWP znajdują się w rozwiązaniach platformy Xamarin.Forms utworzone w programie Visual Studio 2015 i Visual Studio 2017 r.
Możesz również [dodać Windows platformy Uniwersalnej aplikacji](~/xamarin-forms/platform/windows/installation/universal.md) do istniejącego rozwiązania platformy Xamarin.Forms.

