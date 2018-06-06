---
title: Instalowanie platformy Xamarin.iOS w systemie Windows
description: Ten dokument zawiera opis sposobu konfigurowania maszynę z systemem Windows, Mac hosta kompilacji i para systemu Windows do komputera Mac do tworzenia aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: 2bff37aba9b961b7308bf261377951dc96bd8e34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786067"
---
# <a name="installing-xamarinios-on-windows"></a>Instalowanie platformy Xamarin.iOS w systemie Windows

_W tym artykule opisano sposób konfigurowania komputera z systemem Windows i Mac kompilacji hosta do tworzenia aplikacji platformy Xamarin.iOS._

## <a name="overview"></a>Omówienie

Tworzenie aplikacji platformy Xamarin.iOS przy użyciu programu Visual Studio 2017 w systemie Windows, będą potrzebne:
 
-  Maszynę z systemem Windows z programu Visual Studio 2017 r zainstalowane. Może to być fizyczne lub maszyny wirtualnej.
    - [Wymagania dotyczące systemu Windows](~/cross-platform/get-started/requirements.md#windows-requirements)
    
-  Dostępne w sieci Mac instalacji narzędzi kompilacji i Xamarin.iOS firmy Apple. Visual Studio 2017 uzyskuje dostęp do tego komputera przy użyciu połączenia sieciowego za pomocą narzędzi kompilacji firmy Apple, które są wymagane do kompilowania aplikacji natywnej systemu iOS. 
    - [Wymagania systemowe Mac](~/cross-platform/get-started/requirements.md#macos-requirements)

## <a name="setup"></a>Konfiguracja

Do konfigurowania Xamarin.iOS Programowanie w Visual Studio 2017, wykonaj następujące kroki:

1. Konfigurowanie systemu Windows (Instalowanie programu Visual Studio 2017 r)

    Xamarin.iOS współpracuje z wersji programu Visual Studio 2017 Community, Professional i Enterprise, w przypadku autonomicznych lub maszyny wirtualnej.
    
    - [Zainstaluj program Visual Studio 2017](~/cross-platform/get-started/installation/windows.md).

2. Konfigurowanie komputerów Mac (Zainstaluj Xcode i programu Visual Studio for Mac)

    Do tworzenia, debugowania i podpisywania aplikacji systemu iOS do dystrybucji, Visual Studio 2017 musi mieć dostęp do sieci na hoście kompilacji Mac skonfigurowano narzędzia dla deweloperów firmy Apple zarówno (Xcode) i platformy Xamarin.iOS.

    - [Pobierz i zainstaluj program Xcode ze sklepu Mac App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12). 
    - [Zainstaluj program Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/installation), co spowoduje również zainstalowanie platformy Xamarin.iOS.

    > [!NOTE] 
    > Jeśli użytkownik nie chce zainstalować program Visual Studio dla komputerów Mac, począwszy od [programu Visual Studio 2017 wersji 15,6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning), Visual Studio 2017 mogą automatycznie konfigurować hosta kompilacji Mac przy użyciu oprogramowania, które są niezbędne do tworzenia aplikacji platformy Xamarin.iOS . Aby uzyskać więcej informacji, zobacz [Mac automatycznego inicjowania obsługi administracyjnej](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning).

3. Para Mac (łączenie programu Visual Studio 2017 do komputera Mac)

    Dla programu Visual Studio 2017 za pomocą narzędzi kompilacji systemu iOS dla komputerów Mac dwa komputery muszą łączyć się za pośrednictwem sieci.

    - [Odczytać pary podręczniku Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób konfigurowania komputera z systemem Windows i jego skojarzony host kompilacji Mac do tworzenia aplikacji platformy Xamarin.iOS.

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do rozwiązania Xamarin.iOS dla programu Visual Studio](introduction-to-xamarin-ios-for-visual-studio.md)
- [Konfigurowanie programu Visual Studio 2017 r.](config-options.md)
- [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md)
