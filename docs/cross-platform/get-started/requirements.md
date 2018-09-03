---
title: Wymagania systemowe
description: Ten dokument zawiera listę wymagań systemowych dotyczących tworzenia aplikacji za pomocą platformy Xamarin na komputerach Mac i komputerach z systemem Windows. Zawiera również linki do instrukcji instalacji.
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
author: conceptdev
ms.author: crdun
ms.date: 07/24/2018
ms.openlocfilehash: 422eb24b86ba14ff4e5362db8aeec5775fab5833
ms.sourcegitcommit: aa16f267c59725cc88bd84b049544ecfbec297ac
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43263511"
---
# <a name="system-requirements"></a>Wymagania systemowe

Produkty Xamarin działają na systemach iOS i Android dzięki zestawom SDK dla platformy od firm Apple i Google, zatem wymagania systemowe są zgodne z wymaganiami tych systemów. Na tej stronie przedstawiono zgodność systemów dla platformy Xamarin oraz zalecane środowisko deweloperskie i wersje zestawów SDK.

Zapoznaj się z [instrukcjami instalacji](#installation-instructions), aby uzyskać więcej informacji o uzyskiwaniu oprogramowania i wymaganych zestawach SDK.

## <a name="development-environments"></a>Środowiska deweloperskie

W tej tabeli przedstawiono, które platformy można tworzyć przy użyciu różnych kombinacji narzędzi deweloperskich i systemów operacyjnych:

[!include[](~/cross-platform/includes/development-environment.md)]

> [!NOTE]
> Aby tworzyć aplikacje systemu iOS na komputerach z systemem Windows, potrzebny jest [komputer Mac dostępny w sieci](~/ios/get-started/installation/windows/connecting-to-mac/index.md), służący do zdalnego kompilowania i debugowania. To rozwiązanie działa również w przypadku programu Visual Studio uruchomionego na maszynie wirtualnej z systemem Windows działającej na komputerze Mac.

## <a name="macos-requirements"></a>Wymagania dotyczące systemu macOS

Tworzenie aplikacji platformy Xamarin przy użyciu komputera Mac wymaga następujących wersji zestawów SDK / oprogramowania. Sprawdź wersję swojego systemu operacyjnego i postępuj zgodnie z instrukcjami dla [instalatora platformy Xamarin](#installation-instructions).

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Środowisko Xcode można zainstalować (i zaktualizować), odwiedzając stronę [developer.apple.com](https://developer.apple.com/xcode/download/) lub za pośrednictwem sklepu Mac App Store.

### <a name="testing--debugging-on-macos"></a>Testowanie i debugowanie w systemie macOS

- Aplikacje mobilne platformy Xamarin można wdrożyć na potrzeby testowania i debugowania na urządzeniach fizycznych przy użyciu kabla USB (aplikacje zegarka Apple Watch są najpierw wdrażane na sparowanym telefonie iPhone).
- Aplikacje Xamarin.Mac można przetestować bezpośrednio na komputerze programisty.

[!include[](~/cross-platform/includes/macos-testing.md)]

> [!WARNING]
> Nadchodząca wersja platformy Xamarin.Mac 4.8 będzie obsługiwała tylko system macOS w wersji 10.9 lub wyższej.
> Poprzednie wersje platformy Xamarin.Mac obsługiwały system macOS w wersji 10.7 lub wyższej, ale starsze wersje systemu macOS nie posiadają odpowiedniej infrastruktury TLS do obsługi protokołu TLS 1.2. W przypadku systemu macOS 10.7 lub macOS 10.8 należy użyć platformy Xamarin.Mac w wersji 4.6 lub wcześniejszej.

## <a name="windows-requirements"></a>Wymagania dotyczące systemu Windows

Tworzenie aplikacji platformy Xamarin przy użyciu komputera z systemem Windows wymaga następujących wersji zestawów SDK / oprogramowania.
Sprawdź wersję swojego systemu operacyjnego (i upewnij się, że nie używasz wersji *Express* programu Visual Studio — jeśli tak, rozważ aktualizację do wersji *Community*).
W instalatorze programu Visual Studio 2017 dostępna jest opcja automatycznej instalacji platformy Xamarin (**Opracowywanie aplikacji mobilnych za pomocą środowiska .NET**).

[!include[](~/cross-platform/includes/windows-requirements.md)]

> [!NOTE]
> - Platforma Xamarin dla programu Visual Studio obsługuje dowolną wersję programu Visual Studio 2017 (Community, Professional i Enterprise).
> - Tworzenie aplikacji Xamarin.Forms dla platformy uniwersalnej systemu Windows (UWP) wymaga systemu Windows 10 i programu Visual Studio 2017.

### <a name="testing--debugging-on-windows"></a>Testowanie i debugowanie w systemie Windows

Aplikacje mobilne platformy Xamarin można wdrożyć na potrzeby testowania i debugowania na urządzeniach fizycznych przy użyciu kabla USB lub bezprzewodowo (urządzenia z systemem iOS muszą być połączone z komputerem Mac, nie z komputerem z uruchomionym programem Visual Studio).

[!include[](~/cross-platform/includes/windows-testing.md)]

## <a name="installation-instructions"></a>Instrukcje instalacji

Najnowszą wersję platformy Xamarin dla systemu macOS można pobrać ze strony [xamarin.com/download](http://xamarin.com/download). W przypadku systemu Windows postępuj zgodnie z instrukcjami instalacji programu [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).

Pełna lista bieżących wersji produktów jest dostępna na [stronie bieżących wersji](http://developer.xamarin.com/releases/current/). Na tej stronie przedstawiono również wersje poszczególnych produktów (oraz linki do informacji o wersji) dla kanałów beta i alfa.

Szczegółowe instrukcje [instalacji](~/cross-platform/get-started/installation/index.md) dla każdej z platform są dostępne tutaj:

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

Dostępne są również dodatkowe informacje o [wymaganiach platformy Xamarin.Forms i obsługiwanych platformach](~/xamarin-forms/get-started/installation.md).

## <a name="related-links"></a>Linki pokrewne

- [Pobierz platformę Xamarin](https://visualstudio.microsoft.com/xamarin/)
- [Bieżące wersje](https://developer.xamarin.com/releases/current/)
