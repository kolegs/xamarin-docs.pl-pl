---
title: Aplikacja Xamarin Live Player
description: W tym dokumencie opisano aplikację, która może służyć do podgląd zmian kodu na żywo na urządzeniu aplikacji Xamarin Live Player. Omówiono w nim instalacji, przykłady, dzienniki, ustawienia, zarządzanie urządzeniami i nie tylko.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: topgenorth
ms.author: toopge
ms.date: 08/08/2017
ms.openlocfilehash: b3166aa440cbe2981d597771b360373fadc6451b
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780672"
---
# <a name="xamarin-live-player-app"></a>Aplikacja Xamarin Live Player

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

Po zainstalowaniu aplikacji na telefonie, postępuj zgodnie z [instrukcje instalacji](~/tools/live-player/install.md) połączyć się z komputera. Wypróbuj jedną z [przykładowe aplikacje](~/tools/live-player/samples.md) Aby przygotować go do pracy.

Uruchamianie aplikacji Xamarin Live Player wygląda następująco:

![Zrzut ekranu na żywo odtwarzacza systemu Android przez aplikację](player-images/app-android-sml.png)

Po naciśnięciu klawisza **parowania z programu Visual Studio**, używanie aparatu do skanowania kodu kreskowego przedstawiający na komputerze:

![Zrzut ekranu przedstawiający skanera kodów kreskowych dla systemu Android](player-images/scan-android-sml.png)

Jeśli połączenie zostanie nawiązane, powinien zostać uruchomiony kod na urządzeniu niemal natychmiast (takie jak [przykład Kalkulator](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)):

![Przykładowa aplikacja Kalkulator działające na urządzeniu](player-images/basic-calculator-sml.png)

## <a name="options"></a>Opcje

Kliknij przycisk informacji **(i)** w dolnej części aplikacji, aby wyświetlić **opcje** menu:

[![Zrzut ekranu przedstawiający menu Opcje](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Dzienniki

Wyświetl dzienniki do diagnozowania problemów.

### <a name="settings"></a>Ustawienia

- Przełącz wyświetlanie błędów kompilacji i środowiska uruchomieniowego.
- Informacje o wersji.
- Wyślij informacje zwrotne.

[![Zrzut ekranu przedstawiający ustawienia](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>Zarządzanie urządzeniami

Aby połączyć urządzenie po raz pierwszy, postępuj zgodnie z instrukcjami [wymagania dotyczące instalacji](~/tools/live-player/install.md). Można skojarzyć wiele urządzeń i zarządzanie nimi za pomocą środowiska IDE.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

W programie Visual Studio, wybierz **Narzędzia > Xamarin Live Player > Zarządzanie urządzeniami...**

![Zarządzanie urządzeniami okna](player-images/manage-tools-menu-vs.png)

To okno umożliwia wykonaj następujące czynności:

- Para nowego urządzenia przez zeskanowanie kodu
- Alternatywnie parowania urządzenia, wpisując kod wyświetlany na jego ekranie
- Usuń istniejące urządzenia z listy

W tym oknie można również uzyskać dostęp z listy urządzeń.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

W programie Visual Studio dla komputerów Mac, wybierz **Narzędzia > Zarządzanie urządzeniami (Xamarin Live Player)...**

![Zarządzanie urządzeniami okna](player-images/manage-tools-menu.png)

To okno umożliwia wykonaj następujące czynności:

- Para nowego urządzenia przez zeskanowanie kodu
- Alternatywnie parowania urządzenia, wpisując kod wyświetlany na jego ekranie
- Usuń istniejące urządzenia z listy

![Zarządzanie urządzeniami okna](player-images/manage.png)

W tym oknie można również przejść, korzystając z listy urządzeń:

![Wybierz platformy Xamarin Live Player urządzeń z listy urządzeń](player-images/manage-device-menu.png)

-----

Jeśli wystąpią Zobacz wszystkie problemy [ograniczenia i rozwiązywanie problemów z](~/tools/live-player/troubleshooting.md).

## <a name="related-links"></a>Linki pokrewne

- [Ograniczenia](~/tools/live-player/limitations.md)
- [Rozwiązywanie problemów](~/tools/live-player/troubleshooting.md)
- [Przykłady aplikacji Xamarin Live Player](samples.md)
