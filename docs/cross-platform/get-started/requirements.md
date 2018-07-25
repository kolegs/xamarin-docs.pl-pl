---
title: Wymagania systemowe
description: Ten dokument zawiera listę wymagań systemowych dotyczących tworzenia aplikacji za pomocą platformy Xamarin na komputerach Mac i Windows. Łączy on również instrukcje dotyczące instalacji.
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
author: conceptdev
ms.author: crdun
ms.date: 07/24/2018
ms.openlocfilehash: 6d16f01965b6b3bcba35cf14d4000f53a4400653
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241981"
---
# <a name="system-requirements"></a>Wymagania systemowe

Produkty platformy Xamarin korzystają z możliwości platformy, zestawy SDK firmy Apple i Google do docelowego z systemem iOS lub Android, dzięki czemu nasze wymagania dotyczące systemu dopasowania w swoim otoczeniu. Ta strona przedstawia zgodność systemu dla platformy Xamarin i Środowisko deweloperskie zalecane i wersji zestawu SDK.

- [Środowiska deweloperskie](#devenv)
- [wymagania dotyczące systemu macOS](#mac)
- [Wymagania dotyczące Windows](#windows)

Odwiedź stronę [instrukcje dotyczące instalacji](#install) więcej informacji na temat uzyskiwania oprogramowania i wymaganych zestawów SDK.

<a name="devenv" />

## <a name="development-environments"></a>Środowiska deweloperskie

Tabeli przedstawiono, które platformy mogą być wbudowane w rozwoju różnych kombinacji narzędzi & systemu operacyjnego:

[!include[](~/cross-platform/includes/development-environment.md)]


> [!NOTE]
> Aby programować dla systemu iOS na komputerach Windows musi być [dostępne w sieci komputera Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)dla zdalnej kompilacji i debugowania. Ta metoda działa również w przypadku programu Visual Studio na komputerze Mac z systemem wewnątrz maszyny Wirtualnej z systemem Windows.

<a name="mac" />

## <a name="macos-requirements"></a>wymagania dotyczące systemu macOS

Używanie komputera Mac do tworzenia aplikacji platformy Xamarin wymaga następujących wersji oprogramowania/zestaw SDK. Sprawdź swoją wersję systemu operacyjnego i postępuj zgodnie z instrukcjami dotyczącymi [Instalatora platformy Xamarin](#install).

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Środowisko Xcode można instalować (i zaktualizować) na [developer.apple.com](https://developer.apple.com/xcode/download/) lub za pośrednictwem Mac App Store.

### <a name="testing--debugging-on-macos"></a>Testowanie i debugowanie w systemie macOS

Aplikacje mobilne platformy Xamarin można wdrożyć na urządzeniach fizycznych przy użyciu kabla USB, testowanie i debugowanie (Xamarin.Mac mogą być testowane aplikacje bezpośrednio na komputerze deweloperskim; Apple Watch aplikacje są wdrażane najpierw sparowane iPhone).

[!include[](~/cross-platform/includes/macos-testing.md)]

<a name="windows" />

## <a name="windows-requirements"></a>Wymagania dotyczące Windows

Do tworzenia aplikacji platformy Xamarin przy użyciu komputera Windows wymaga następujących wersji oprogramowania/zestaw SDK.
Sprawdź swoją wersję systemu operacyjnego (i upewnij się, że nie używasz *Express* wersji programu Visual Studio — w takim przypadku należy wziąć pod uwagę aktualizacji do *społeczności* wersji).
Instalator programu Visual Studio 2017 obejmuje opcję do automatycznej instalacji Xamarin (**opracowywania aplikacji mobilnych przy użyciu platformy .NET**).

[!include[](~/cross-platform/includes/windows-requirements.md)]

> [!NOTE]
>
>- Platforma Xamarin dla programu Visual Studio obsługuje dowolnego programu Visual Studio 2017 (Community, Professional i Enterprise).
>
>- Do tworzenia aplikacji Xamarin.Forms dla uniwersalnej platformy Windows (UWP) wymaga systemu Windows 10 za pomocą programu Visual Studio 2017.

### <a name="testing--debugging-on-windows"></a>Testowanie i debugowanie na Windows

Aplikacje mobilne platformy Xamarin można wdrożyć na urządzeniach fizycznych przy użyciu kabla USB, testowanie i debugowanie (iOS, które urządzenia muszą być podłączone do komputera Mac, nie komputera z systemem Visual Studio).

[!include[](~/cross-platform/includes/windows-testing.md)]

<a name="install" />

## <a name="installation-instructions"></a>Instrukcje dotyczące instalacji

Najnowsza wersja platformy Xamarin dla systemu macOS można pobrać z [xamarin.com/download](http://xamarin.com/download). Windows, postępuj zgodnie z [programu Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) instrukcje dotyczące instalacji.

Pełna lista naszej bieżącej wersji produktu jest dostępna na [bieżącej strony z wersjami](http://developer.xamarin.com/releases/current/). Na tej stronie przedstawiono również wersje poszczególnych produktów (i łączy się z informacjami o wersji) w wersji beta i kanałów alfa.

Określone [instalacji](~/cross-platform/get-started/installation/index.md) instrukcje dotyczące każdej platformy są dostępne tutaj:

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

Istnieje również dodatkowe informacje na temat [Xamarin.Forms wymagania dotyczące obsługiwanych platform](~/xamarin-forms/get-started/installation.md).

## <a name="related-links"></a>Linki pokrewne

- [Pobierz program Xamarin](https://visualstudio.microsoft.com/xamarin/)
- [Bieżące wersje](https://developer.xamarin.com/releases/current/)
