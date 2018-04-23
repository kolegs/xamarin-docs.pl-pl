---
title: Konfigurowanie programu Visual Studio 2017 r.
description: W tym artykule opisano różne opcje konfiguracji platformy Xamarin.iOS dla programu Visual Studio 2017 r.
ms.prod: xamarin
ms.assetid: 22D82244-890D-4325-B3CC-C0AC49130BCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: ee08cf7d68bd9d10026f1c15d4438077349fe367
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2018
---
# <a name="configuring-visual-studio-2017"></a>Konfigurowanie programu Visual Studio 2017 r.

_W tym artykule opisano różne opcje konfiguracji platformy Xamarin.iOS dla programu Visual Studio._

## <a name="using-matching-xamarinios-versions"></a>Przy użyciu zgodnych wersji platformy Xamarin.iOS

Visual Studio 2017 musi używać tej samej wersji platformy Xamarin.iOS zainstalowanego na hoście kompilacji Mac. Aby upewnić się, to PRAWDA:

 - Jeśli używasz programu Visual Studio 2017 wybierz **stabilna** kanału aktualizacji w programie Visual Studio dla komputerów Mac.

 - Jeśli używasz programu Visual Studio 2017 podglądu, wybierz **alfa** kanału aktualizacji w programie Visual Studio dla komputerów Mac.

> [!NOTE]
> Począwszy od [programu Visual Studio 2017 wersji 15,6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning), Visual Studio 2017 automatycznie wykrywa, czy host kompilacji Mac używa tej samej wersji platformy Xamarin.iOS jako systemu Windows. W przypadku niezgodności wersji programu Visual Studio 2017 oferuje zdalnie zainstalować odpowiednią wersję na hoście Mac kompilacji. Aby uzyskać więcej informacji, zapoznaj się [Mac automatycznego inicjowania obsługi administracyjnej](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning) sekcji [pary Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) przewodnik.

## <a name="ios-toolbar"></a>pasek narzędzi dla systemu iOS

Gdy projekt dla systemu iOS jest otwarty w programie Visual Studio 2017, narzędzi dla systemu iOS powinny być widoczne.  Domyślnie zawiera cztery przyciski, które są przydatne do tworzenia aplikacji platformy Xamarin.iOS:

![Pasek narzędzi z systemem iOS w programie Visual Studio 2017](config-options-images/ios-toolbar.png "narzędzi dla systemu iOS w programie Visual Studio 2017")

- **Para Mac** — otwiera pary do okna dialogowego Mac. Włączone, gdy projekt dla systemu iOS jest otwarty w programie Visual Studio 2017 r.
- **Pokaż symulatora systemu iOS** — host kompilacji na Mac, przesuwa na wierzch symulatora systemu iOS. Włączone, gdy projekt dla systemu iOS jest otwarty w programie Visual Studio 2017 r.
- **Urządzenie dziennika** — wyświetlenie okna, które pozwala sprawdzać dzienniki urządzenia. Włączone, gdy projekt dla systemu iOS jest otwarty w programie Visual Studio 2017 r.
- **Pokaż plik IPA na serwerze kompilacji** — powoduje otwarcie okna na hoście kompilacji Mac przedstawiający lokalizację pliku IPA aplikacji. Włączone po zakończeniu kompilacji, dla którego utworzono .ipa.

Jeśli nie ma tego paska narzędzi, otwórz **widoku** menu programu Visual Studio 2017 r i wybierz polecenie **paski narzędzi > iOS**:

![Włączanie narzędzi dla systemu iOS](config-options-images/ios-toolbar-enable.png "włączenie narzędzi dla systemu iOS")

## <a name="solution-platforms-drop-down-menu"></a>Menu rozwijane platformy rozwiązania

**Platformy rozwiązania** listy rozwijanej można wybrać, czy następnej kompilacji będzie obowiązywać fizyczne urządzenie lub symulator.

Aby upewnić się, to menu rozwijane jest widoczny na standardowym pasku narzędzi:

- W programie Visual Studio 2017 r kliknij strzałkę w dół na prawej krawędzi standardowym pasku narzędzi.
- Wybierz **Dodaj lub usuń przyciski** 
- Upewnij się, że **platformy rozwiązania** zaznaczono element:

![Włączanie menu rozwijanego platformy rozwiązania](config-options-images/solution-platforms-enable.png "włączanie menu rozwijanego platformy rozwiązania")

Po otwarciu projektu iOS **standardowe** i **iOS** paski narzędzi powinien teraz wyglądać Poniższy zrzut ekranu:

![Paski narzędzi Standardowy i iOS](config-options-images/toolbars.png "Standard i iOS paski narzędzi")


