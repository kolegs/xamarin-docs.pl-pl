---
title: Zdalny symulator systemu iOS dla systemu Windows
description: Zdalny symulator systemu Windows dla systemu iOS służy do testowania aplikacji w symulatorze iOS wyświetlany w systemie Windows z programu Visual Studio 2017 r.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: topgenorth
ms.author: toopge
ms.date: 05/11/2018
ms.openlocfilehash: b07cc24e63f4aa3ce4451e3bdb5819f1df1058c6
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34149810"
---
# <a name="remoted-ios-simulator-for-windows"></a>Zdalny symulator systemu iOS dla systemu Windows

Zdalny symulator systemu Windows dla systemu iOS służy do testowania aplikacji w symulatorze iOS wyświetlany w systemie Windows z programu Visual Studio 2017 r.

[![](ios-simulator-images/hero-sml.png "symulatora systemu iOS uruchomiony w systemie Windows")](ios-simulator-images/hero.png#lightbox)

## <a name="getting-started"></a>Wprowadzenie

Zdalny iOS Simulator dla systemu Windows jest instalowana automatycznie w ramach platformy Xamarin w programie Visual Studio 2017 r. Aby go użyć, wykonaj następujące kroki:

1. [Sparuj Visual 2017 do hosta usługi kompilacji Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. W programie Visual Studio 2017 r Rozpocznij debugowanie projektu systemu iOS lub systemu tvOS. Zdalny iOS Simulator dla systemu Windows pojawi się na komputerze z systemem Windows.

## <a name="simulator-window"></a>Symulator okna

Pasek narzędzi w górnej części okna symulatora zawiera wiele przydatne przyciski:

- **Macierzysty** — symuluje przycisku Strona główna na urządzeniu z systemem iOS
- **Zablokuj** — blokuje symulatora (Przejdź do odblokowania)
- **Zrzut ekranu** — zrzut ekranu przedstawiający symulator jest zapisywany
- [**Ustawienia** ](#settings) — Wyświetla klawiatury, lokalizacji i inne ustawienia
- [**Inne opcje** ](#other-options) — wyświetlenie różnych opcji symulator, takich jak obracanie i potrząsania powoduje gestów

    [![](ios-simulator-images/maps-app-sml.png "przykład mapuje symulatora systemu iOS")](ios-simulator-images/maps-app.png#lightbox)

## <a name="settings"></a>Ustawienia

Kliknięcie przycisku paska narzędzi koło zębate ikonę otwiera **ustawienia** okno:

[![](ios-simulator-images/settings-sml.png "Ustawienia symulatora systemu iOS")](ios-simulator-images/settings.png#lightbox)

Te ustawienia umożliwiają włączenia klawiatury sprzętu, wybierz lokalizację, która ma być urządzenia raportu (statyczne i przenoszenie lokalizacje są obsługiwane), włączyć funkcję Touch ID i zresetować zawartość i ustawienia symulatora.

## <a name="other-options"></a>Inne opcje

Przycisk wielokropka ujawnia inne opcje, takie jak obracanie, gestów potrząsania powoduje i ponowny rozruch. Te same opcje można wyświetlić jako listę, prawym przyciskiem myszy w oknie symulatora:

[![](ios-simulator-images/more-sml.png "dodatkowe ustawienia symulatora systemu iOS")](ios-simulator-images/more.png#lightbox)

## <a name="touchscreen-support"></a>Obsługa ekranu dotykowego

Większość nowoczesnych komputerów z systemem Windows mają ekranów dotykowych. Ponieważ zdalny iOS Simulator dla systemu Windows obsługuje interakcje touch, należy przetestować aplikacji za pomocą tego samego uszczypnięcia, przejdź i gestów dotykowych wielu linii papilarnych, korzystających z urządzeń fizycznych z systemem iOS.

Podobnie zdalny symulator systemu Windows dla systemu iOS traktuje wprowadzania danych piórem systemu Windows jako dane wejściowe ołówka firmy Apple.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Wyłączanie zdalny symulator systemu Windows dla systemu iOS

Aby wyłączyć zdalny iOS Simulator dla systemu Windows, przejdź do **Narzędzia > Opcje > Xamarin > Ustawienia systemu iOS** i usuń zaznaczenie pola wyboru **zdalnego symulator systemu Windows**.

[![](ios-simulator-images/options-sml.png "pole wyboru, aby użyć symulatora")](ios-simulator-images/options.png#lightbox)

Wyłączeniu tej opcji, debugowanie otwiera w narzędziu iOS Simulator na połączonych Mac kompilacji hosta.
