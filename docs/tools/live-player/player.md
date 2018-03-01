---
title: "Aplikacja odtwarzacza na żywo Xamarin"
description: "Edytuj i testowania aplikacji w czasie rzeczywistym na urządzenia z systemem iOS lub Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/10/2017
ms.openlocfilehash: 6ae0439566387e22d939ede83e6b4cb5c1a5a8fe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-live-player-app"></a>Aplikacja odtwarzacza na żywo Xamarin

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

## <a name="get-the-app"></a>Pobierz aplikację

Xamarin Player na żywo jest dostępnej w sklepie iOS App Store i Google Play:

[ ![Pobieranie ze sklepu z aplikacjami systemu iOS](player-images/app-store-badge.svg)](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?ls=1&mt=8) [ ![dostępne w witrynie Google Play](player-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)



## <a name="using-the-app"></a>Przy użyciu aplikacji

Po zainstalowaniu aplikacji na telefonie, wykonaj [instrukcje instalacji](~/tools/live-player/install.md) do podłączenia do komputera. Wypróbuj jedną z [przykładowe aplikacje](~/tools/live-player/samples.md) można pobrać go do pracy.

Uruchamianie aplikacji platformy Xamarin Player na żywo wygląda następująco (w systemach iOS i Android odpowiednio):

![Zrzut ekranu aplikacji systemu iOS Player na żywo](player-images/app-iphone-sml.png) ![Na żywo Player Android zrzut ekranu aplikacji](player-images/app-android-sml.png)

Po naciśnięciu **pary dla programu Visual Studio**, użyć aparatu do skanowania kodów kreskowych, przedstawiający na komputerze:

![Zrzut ekranu przedstawiający skanera kodów kreskowych z systemem iOS](player-images/scan-iphone-sml.png) ![Zrzut ekranu przedstawiający skanera kodów kreskowych systemu Android](player-images/scan-android-sml.png)

Jeśli połączenie zostanie nawiązane, kod należy uruchomić na urządzenia niemal natychmiast (na przykład przykład kalkulatora):

![Przykładowa aplikacja Kalkulator działającej na urządzeniu](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>Opcje

Kliknij przycisk informacji **(i)** w dolnej części aplikacji, aby wyświetlić **opcje** menu:

![Zrzut ekranu przedstawiający menu opcji](player-images/options.png)

### <a name="logs"></a>Dzienniki

Wyświetl dzienniki do diagnozowania problemów.

### <a name="settings"></a>Ustawienia

* Przełącz wyświetlanie błędów kompilacji i środowiska wykonawczego.
* Informacje o wersji.
* Wyślij opinię.

![Zrzut ekranu, ustawienia](player-images/settings.png)

## <a name="managing-devices"></a>Zarządzanie urządzeniami

Aby połączyć z urządzenia po raz pierwszy, postępuj zgodnie z instrukcjami [wymagania dotyczące instalacji](~/tools/live-player/install.md). Można skojarzyć wielu urządzeń (na przykład iOS i Android) i zarządzać nimi za pomocą środowiska IDE.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W programie Visual Studio, wybierz **Narzędzia > Xamarin Live Player > Zarządzanie urządzeniami...**

![Zarządzanie urządzeniami okna](player-images/manage-tools-menu-vs.png)

To okno umożliwia wykonaj następujące czynności:

- Para nowe urządzenie przez zeskanowanie kodu
- Można również skojarzyć urządzenia, wpisując kod wyświetlany na ekranie jego
- Usuń istniejące urządzenia z listy

Można również dostęp do tego okna z listy urządzeń.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W programie Visual Studio dla komputerów Mac, wybierz **Narzędzia > Zarządzanie urządzeniami (Xamarin Live Player)...**

![Zarządzanie urządzeniami okna](player-images/manage-tools-menu.png)

To okno umożliwia wykonaj następujące czynności:

- Para nowe urządzenie przez zeskanowanie kodu
- Można również skojarzyć urządzenia, wpisując kod wyświetlany na ekranie jego
- Usuń istniejące urządzenia z listy

![Zarządzanie urządzeniami okna](player-images/manage.png)

Można również dostęp do tego okna z listy urządzeń:

![Wybierz z listy urządzeń Xamarin Live Player urządzeń](player-images/manage-device-menu.png)

-----

Jeśli wystąpią można znaleźć żadnych problemów [ograniczenia i rozwiązywanie problemów z](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>Linki pokrewne

- [Ograniczenia](~/tools/live-player/limitations.md)
- [Rozwiązywanie problemów](~/tools/live-player/troubleshooting.md)
- [Przykłady na żywo Player Xamarin](~/tools/livehttps://developer.xamarin.com/samples.md)
