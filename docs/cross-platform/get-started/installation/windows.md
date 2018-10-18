---
title: Instalowanie środowiska Xamarin w programie Visual Studio 2017
description: W tym dokumencie opisano sposób instalacji Xamarin w programie Visual Studio 2017. Omówiono w nim wymagania dotyczące procesu instalacji i weryfikowanie instalacji.
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
author: conceptdev
ms.author: crdun
ms.date: 08/28/2018
ms.openlocfilehash: cd87c33394b21ce6a9338d239de8f013cc8ec1fa
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "37066955"
---
# <a name="installing-xamarin-in-visual-studio-2017"></a>Instalowanie środowiska Xamarin w programie Visual Studio 2017

<a name="requirements" />

Sprawdź [wymagania systemowe](~/cross-platform/get-started/requirements.md) przed przystąpieniem do wykonywania.

## <a name="installation"></a>Instalacja

[!include[](~/cross-platform/includes/install-xamarin-windows.md)]

W programie Visual Studio 2017, sprawdź, czy platformy Xamarin jest zainstalowany, klikając **pomocy** menu. Jeśli zainstalowano program Xamarin, powinien zostać wyświetlony **Xamarin** elementu menu, jak pokazano na tym zrzucie ekranu:

![Element menu platformy Xamarin w menu Pomoc](windows-images/12-xamarin-menu-item.png "Xamarin element menu w menu Pomoc")

Możesz również kliknąć **Pomoc > informacje programu Microsoft Visual Studio** i przewijaj listę zainstalowanych produktów, aby zobaczyć, czy zainstalowano program Xamarin:

![Program Visual Studio 2015 zainstalowane produkty ekranu](windows-images/13-xamarin-is-installed.png "Visual Studio Visual 2015 zainstalowane produkty ekranu")

Aby uzyskać więcej informacji na temat lokalizowania informacji o wersji, zobacz [gdzie mogę znaleźć Moje informacje o wersji i dzienniki?](~/cross-platform/troubleshooting/questions/version-logs.md)

## <a name="next-steps"></a>Następne kroki

Instalowanie środowiska Xamarin w programie Visual Studio 2017 pozwala rozpocząć pisanie kodu dla aplikacji, ale wymaga dodatkowej konfiguracji do tworzenia i wdrażania aplikacji w symulatorze, emulator i urządzeń. Można znaleźć w następujących Przewodnikach w celu ukończenia instalacji programu i zacznij tworzyć aplikacje dla wielu platform.

### <a name="ios"></a>iOS

Aby uzyskać więcej informacji, zobacz [instalowania rozszerzenia Xamarin.iOS na Windows](~/ios/get-started/installation/windows/index.md) przewodnik. 

1. [Zainstaluj program Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/installation)
2. [Visual Studio nawiązać połączenie z hostem kompilacji komputera Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
3. [Konfiguracja dla deweloperów z systemem iOS](~/ios/get-started/installation/device-provisioning/index.md) — jest to wymagane do uruchamiania aplikacji na urządzeniu
5. [Zdalny symulator systemu iOS](~/tools/ios-simulator/index.md)
6. [Wprowadzenie do rozwiązania Xamarin.iOS dla programu Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### <a name="android"></a>Android

Aby uzyskać więcej informacji, zobacz [instalowanie platformy Xamarin.Android na Windows](~/android/get-started/installation/windows.md) przewodnik.

1. [Xamarin.Android Configuration](~/android/get-started/installation/windows.md#configuration)
2. [Przy użyciu Menedżera zestawów SDK platformy Xamarin Android](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Emulator zestawu SDK systemu Android](~/android/get-started/installation/android-emulator/index.md)
4. [Konfigurowanie urządzenia na potrzeby programowania](~/android/get-started/installation/set-up-device-for-development.md)
