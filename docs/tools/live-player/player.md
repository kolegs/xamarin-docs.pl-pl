---
title: Aplikacja odtwarzacza na żywo Xamarin
description: Edytuj i testowania aplikacji w czasie rzeczywistym na urządzenia z systemem iOS lub Android
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: topgenorth
ms.author: toopge
ms.date: 05/10/2017
ms.openlocfilehash: 2e2a3e80c121688f9a8bfff0afe4184f19fc6619
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="xamarin-live-player-app"></a>Aplikacja odtwarzacza na żywo Xamarin

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

## <a name="get-the-app"></a>Pobierz aplikację

# <a name="androidtabandroid"></a>[Android](#tab/android)

Xamarin Player na żywo jest dostępna dla systemu Android ze sklepu Google Play:

[![Dostępne w witrynie Google Play](images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Dla urządzeń z systemem Android bez Google Play Xamarin odtwarzacz na żywo jest dostępna za pośrednictwem [HockeyApp](https://aka.ms/xlp-hockeyapp) dystrybucji. Ponadto Podgląd wczesne kompilacje dla systemu Android mogą być instalowane bezpośrednio z witryny Google Play przez zgody na korzystanie z [otwarte w wersji beta programu](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

Firma Microsoft zachęca użytkowników do dołączenia do [aplikacji odtwarzacza Live Xamarin _z systemem iOS w wersji zapoznawczej_ ](https://aka.ms/liveplayeralpha) można uzyskiwać dostęp do najnowszych ulepszeń za pośrednictwem TestFlight.

-----

## <a name="using-the-app"></a>Przy użyciu aplikacji

Po zainstalowaniu aplikacji na telefonie, wykonaj [instrukcje instalacji](~/tools/live-player/install.md) do podłączenia do komputera. Wypróbuj jedną z [przykładowe aplikacje](~/tools/live-player/samples.md) można pobrać go do pracy.

Uruchamianie aplikacji platformy Xamarin Player na żywo wygląda następująco (w systemach iOS i Android odpowiednio):

![Zrzut ekranu aplikacji systemu iOS Player na żywo](player-images/app-iphone-sml.png) ![Na żywo Player Android zrzut ekranu aplikacji](player-images/app-android-sml.png)

Po naciśnięciu **pary dla programu Visual Studio**, użyć aparatu do skanowania kodów kreskowych, przedstawiający na komputerze:

![Zrzut ekranu przedstawiający skanera kodów kreskowych z systemem iOS](player-images/scan-iphone-sml.png) ![Zrzut ekranu przedstawiający skanera kodów kreskowych systemu Android](player-images/scan-android-sml.png)

Jeśli połączenie zostanie nawiązane, kod powinien prawie natychmiast uruchomić na urządzeniu (takich jak [próbki kalkulatora](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)):

![Przykładowa aplikacja Kalkulator działającej na urządzeniu](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>Opcje

Kliknij przycisk informacji **(i)** w dolnej części aplikacji, aby wyświetlić **opcje** menu:

[![Zrzut ekranu przedstawiający menu opcji](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Dzienniki

Wyświetl dzienniki do diagnozowania problemów.

### <a name="settings"></a>Ustawienia

- Przełącz wyświetlanie błędów kompilacji i środowiska wykonawczego.
- Informacje o wersji.
- Wyślij opinię.

[![Zrzut ekranu, ustawienia](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>Zarządzanie urządzeniami

Aby połączyć z urządzenia po raz pierwszy, postępuj zgodnie z instrukcjami [wymagania dotyczące instalacji](~/tools/live-player/install.md). Można skojarzyć wielu urządzeń (na przykład iOS i Android) i zarządzać nimi za pomocą środowiska IDE.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

W programie Visual Studio, wybierz **Narzędzia > Xamarin Live Player > Zarządzanie urządzeniami...**

![Zarządzanie urządzeniami okna](player-images/manage-tools-menu-vs.png)

To okno umożliwia wykonaj następujące czynności:

- Para nowe urządzenie przez zeskanowanie kodu
- Można również skojarzyć urządzenia, wpisując kod wyświetlany na ekranie jego
- Usuń istniejące urządzenia z listy

Można również dostęp do tego okna z listy urządzeń.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

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
- [Przykłady na żywo Player Xamarin](samples.md)
