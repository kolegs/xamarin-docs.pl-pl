---
title: Zdalny iOS Simulator (dla systemu Windows)
description: "Test i debugowania aplikacji systemu iOS w całości w programie Visual Studio w systemie Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/07/2017
ms.openlocfilehash: 20e8d843b73fca5eb49da21a7f834ebbb52f4aa9
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="remoted-ios-simulator-for-windows"></a>Zdalny iOS Simulator (dla systemu Windows)

_Test i debugowania aplikacji systemu iOS w całości w programie Visual Studio w systemie Windows_

[![](ios-simulator-images/hero-sml.png "symulatora systemu iOS uruchomiony w systemie Windows")](ios-simulator-images/hero.png#lightbox)

## <a name="download-and-install"></a>Pobierz i zainstaluj

Pobierz [Instalator](https://dl.xamarin.com/xamarin-simulator/Xamarin.Simulator.Installer.msi) i zainstalować na komputerze z systemem Windows. Visual Studio Tools dla platformy Xamarin powinno być już zainstalowane.

> [!NOTE]
> Przy użyciu zdalnego systemu iOS Simulator wymaga programu Visual Studio sieciowych Mac za pomocą platformy Xamarin zainstalowane.

## <a name="getting-started"></a>Wprowadzenie

Aby użyć symulatora zdalnego systemu iOS:

1. Upewnij się, że program Visual Studio został podłączony do komputera Mac co najmniej raz, przed rozpoczęciem zdalnego symulatora systemu iOS.
2. Upewnij się, aplikacja systemu iOS lub systemu tvOS jest **projekt startowy** i Rozpocznij debugowanie.

Możesz wyłączyć symulatorze zdalnego systemu iOS z **Narzędzia > Opcje > Xamarin > Ustawienia systemu iOS** usuwając zaznaczenie pola wyboru pole **zdalnego symulator systemu Windows** pokazano poniżej:

[![](ios-simulator-images/options-sml.png "pole wyboru, aby użyć symulatora")](ios-simulator-images/options.png#lightbox)

Na komputerze Mac w podłączonych następnie otworzy symulatora systemu iOS. Zaznacz tę opcję, aby włączyć symulatora zdalnego systemu iOS.

## <a name="features"></a>Funkcje

Zdalne symulatora systemu iOS zapewnia sposób do testowania i debugowania aplikacji systemu iOS w symulatorze całkowicie z programu Visual Studio w systemie Windows.

### <a name="simulator-window"></a>Symulator okna

Pasek narzędzi okna obejmuje kilka przycisków wchodzić w interakcje z symulatora:

- **Macierzysty** — symuluje przycisku Strona główna na urządzeniu.
- **Zablokuj** — blokuje symulatora (użytkownik może szybko Przesuń do odblokowania).
- **Zrzut ekranu** — zrzut ekranu symulatora jest zapisywany na dysku.
- [**Ustawienia** ](#settings) — konfigurowanie klawiatury i lokalizacji.
- Inne [ **opcje** ](#options) — różne opcje symulatora są dostępne, takie jak obracanie, potrząsania powoduje lub wywołanie innych stanów w symulatorze. Jeśli niektóre opcje są zasłonięty, możliwy z ikoną wielokropka, wyświetlane na pasku narzędzi lub klikając prawym przyciskiem myszy w oknie.

    [![](ios-simulator-images/maps-app-sml.png "przykład mapuje symulatora systemu iOS")](ios-simulator-images/maps-app.png#lightbox)


### <a name="settings"></a>Ustawienia

Ikona "narzędzia" otwiera **ustawienia** okno:

[![](ios-simulator-images/settings-sml.png "Ustawienia symulatora systemu iOS")](ios-simulator-images/settings.png#lightbox)

Dzięki temu można włączyć klawiatury sprzętu w symulatorze i wybierz polecenie lokalizacji jest zgłaszany na urządzeniu (w tym statycznego lokalizacji lub inne opcje przenoszenia lokalizacji).



### <a name="other-options"></a>Inne opcje

Kliknij prawym przyciskiem myszy w oknie symulator, aby wyświetlić wszystkie opcje dostępne w symulatorze, takie jak obracanie wyzwalania gest potrząsania powoduje i ponownego uruchamiania symulatora:

[![](ios-simulator-images/more-sml.png "dodatkowe ustawienia symulatora systemu iOS")](ios-simulator-images/more.png#lightbox)

### <a name="touchscreen-support"></a>Obsługa ekranu dotykowego

Większość nowoczesnych komputerów z systemem Windows mają ekranów dotykowych i symulatora zdalnego systemu iOS umożliwia touch okna symulatora testowanie interakcji użytkowników w aplikacji systemu iOS.

Obejmuje to punkty zaciskające, szybko przesuwając i gestów dotykowych palca wielu - rzeczy, które wcześniej może być łatwo badać tylko na fizycznych urządzeniach.

Obsługa Pióro w systemie Windows jest również translacji na dane wejściowe Apple ołówka w symulatorze.

<!--
<a name="knownissues" />

# Known Issues

 - Apple Watch devices may show in the Visual Studio device list, but are not yet supported.
 - Launching in **Release** mode may also start Apple’s simulator on the networked Mac.
 - Closing the remote iOS Simulator on Windows will not immediately stop debugging in Visual Studio. Stop debugging manually from the menu or the red button.
 - Opening too many different simulators simultaneously will produce unexpected results.
 - Exception of type `Foundation.NSErrorException` may be thrown while launching Simulators. Workaround is to kill csproxy (server process) on the Mac host and re-deploy to the simulator.
 - Performance may be slower when using Xcode 8
-->
