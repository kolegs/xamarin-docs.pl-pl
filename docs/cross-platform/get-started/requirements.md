---
title: Wymagania systemowe
description: "Wymagania wstępne dotyczące korzystania z platformy Xamarin"
ms.topic: article
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 08/28/2017
ms.openlocfilehash: 4a53053ebef88bf831b7749fa82f3444ecc26723
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="system-requirements"></a>Wymagania systemowe

_Wymagania wstępne dotyczące korzystania z platformy Xamarin_

Produkty Xamarin opierają się na platformie zestawów SDK z firmy Apple i Google do docelowych z systemem iOS lub Android, więc nasze wymagania systemowe odpowiada szkodliwego. Ta strona przedstawia zgodność systemu dla platformy Xamarin i środowisko projektowe zalecane i wersji zestawu SDK.

- [Środowisk deweloperskich](#devenv)
- [System macOS wymagania](#mac)
- [Wymagania dotyczące systemu Windows](#windows)

Odwiedź stronę [instrukcje dotyczące instalacji](#install) Aby uzyskać więcej informacji na temat uzyskiwania oprogramowania i wymaganych zestawów SDK.

<a name="devenv" />

## <a name="development-environments"></a>Środowisk deweloperskich

Ten przedstawia tabeli platformy, których można wbudować o różnych programowanie narzędzia & systemu operacyjnego:

[!include[](~/cross-platform/includes/development-environment.md)]


> [!NOTE]
> Umożliwiające tworzenie dla systemu iOS na komputerach z systemem Windows musi być [dostępny w sieci komputera Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md), kompilacji zdalnej i debugowanie. Ta metoda działa również Jeśli masz program Visual Studio na komputerze Mac z systemem wewnątrz maszyny Wirtualnej systemu Windows.

<a name="mac" />

## <a name="macos-requirements"></a>System macOS wymagania

Używanie komputera Mac do tworzenia aplikacji platformy Xamarin wymaga następujących wersji oprogramowania/pakiet SDK. Sprawdź wersję systemu operacyjnego i postępuj zgodnie z instrukcjami dotyczącymi [Instalator Xamarin](#install).

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Xcode można instalować (i zaktualizować) na [developer.apple.com](https://developer.apple.com/xcode/download/) lub za pośrednictwem sklepu Mac App Store.

### <a name="testing--debugging-on-macos"></a>Testowanie i debugowanie na macOS

Aplikacje mobilne platformy Xamarin można wdrożyć na fizycznych urządzeniach przez połączenie USB testowanie i debugowanie (Xamarin.Mac aplikacji można przetestować bezpośrednio na komputerze dewelopera; Apple Watch aplikacje są wdrażane najpierw sparowanego iPhone).

[!include[](~/cross-platform/includes/macos-testing.md)]


<a name="windows" />

## <a name="windows-requirements"></a>Wymagania dotyczące systemu Windows

Używanie komputerem z systemem Windows do tworzenia aplikacji platformy Xamarin wymaga następujących wersji oprogramowania/pakiet SDK.
Sprawdź wersję systemu operacyjnego (i upewnij się, że nie używasz *Express* wersji programu Visual Studio — Jeśli tak, może być konieczna aktualizacja do *społeczności* edition).
Visual Studio 2015 i instalatorów 2017 zawierają opcję automatycznie Zainstaluj program Xamarin.

[!include[](~/cross-platform/includes/windows-requirements.md)]


> [!NOTE]
>
>* Xamarin dla Visual Studio obsługuje wszystkie programu Visual Studio 2015 lub 2017 (Community, Professional i Enterprise).
>
>* Utworzenie aplikacji platformy Xamarin.Forms dla uniwersalnych platformy systemu Windows (UWP) wymaga systemu Windows 10 z programu Visual Studio 2015 lub 2017 r.


### <a name="testing--debugging-on-windows"></a>Testowanie i debugowanie w systemie Windows

Aplikacje mobilne platformy Xamarin można wdrożyć na fizycznych urządzeniach przez połączenie USB testowanie i debugowanie (iOS, które urządzenia musi być podłączony do komputera Mac, nie komputera z uruchomionym Visual Studio).

[!include[](~/cross-platform/includes/windows-testing.md)]


> [!NOTE]
>
>* [Pobierz emulator Windows Phone 8.1](https://www.microsoft.com/en-us/download/details.aspx?id=43719).
>* Emulator Windows Phone 10 jest dołączony do programu Visual Studio 2015 platformy uniwersalnej systemu Windows SDK.

<a name="install" />

## <a name="installation-instructions"></a>Instrukcje dotyczące instalacji

Można pobrać najnowszą wersję platformy Xamarin dla macOS z [xamarin.com/download](http://xamarin.com/download). W systemie Windows, należy wykonać [programu Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/install-visual-studio) instrukcje dotyczące instalacji.

Pełna lista naszych bieżącej wersji produktu jest dostępny na [bieżącej strony wersjach](http://developer.xamarin.com/releases/current/). Ta strona omówiono również wersji określonego produktu (oraz łączy się z informacjami o wersji) dla naszych beta i kanałów alfa.

Określone [instalacji](~/cross-platform/get-started/installation/index.md) instrukcje dotyczące każdej platformy są dostępne w tym miejscu:

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

Istnieje również dodatkowe informacje o [platformy Xamarin.Forms wymagania dotyczące obsługiwanych platform](~/xamarin-forms/get-started/installation.md).


## <a name="related-links"></a>Linki pokrewne

- [Pobierz Xamarin](https://xamarin.com/download/)
- [Bieżące wersje](https://developer.xamarin.com/releases/current/)
