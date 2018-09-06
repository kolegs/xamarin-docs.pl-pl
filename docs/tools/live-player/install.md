---
title: Xamarin Live Player Instalatora
description: W tym dokumencie opisano sposób konfigurowania aplikacji Xamarin Live Player i przy jego użyciu dokonaj edycji na żywo do uruchomionej aplikacji.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: topgenorth
ms.author: toopge
ms.date: 08/08/2018
ms.openlocfilehash: 10fdc09ec946ff57b87ad4749cf6edaf04b5f7e8
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780607"
---
# <a name="xamarin-live-player-setup"></a>Xamarin Live Player Instalatora

Aplikacja Xamarin Live Player umożliwia dokonaj edycji na żywo do aplikacji i mają te zmiany zostaną uwzględnione na żywo na urządzeniu. Kod jest wykonywany wewnątrz aplikacji Xamarin Live Player — nie ma potrzeby konfigurowania emulatorów lub wdrożyć przy użyciu kabli! W tym artykule opisano sposób konfigurowania aplikacji Xamarin Live Player.

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

## <a name="1-get-the-android-app"></a>1. Pobierz aplikację dla systemu Android

Aplikacja Xamarin Live Player jest dostępna dla systemu Android ze sklepu Google Play:

[ ![Dostępne w witrynie Google Play](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Dla urządzeń z systemem Android bez sklepu Google Play środowiska Xamarin Live Player jest dostępna za pośrednictwem [HockeyApp](https://aka.ms/xlp-hockeyapp) dystrybucji. Ponadto wczesną wersję zapoznawczą kompilacji dla systemu Android można zainstalować bezpośrednio ze sklepu Google Play za zgody na korzystanie z [otwarty w wersji beta](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2. Pobierz program Visual Studio 2017

Wymaga aplikacji Xamarin Live Player:

- Visual Studio 2017 15.4 lub nowszej.
- Komputer Visual Studio i urządzenia na tej samej sieci Wi-Fi.

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Po raz pierwszy przy użyciu aplikacji Xamarin Live Player

1. Otwórz **programu Visual Studio 2017**.
2. Przejdź do **Narzędzia > Opcje...**  i wybierz **Xamarin > inne** kartę.
3. Znaczników **Włącz Xamarin Live Player**:

    ![Zaznacz pole Włącz Xamarin Live Player w oknie Opcje](install-images/vs2017-options.png)

4. Utwórz lub Otwórz projekt Xamarin (lub [przykładowe](~/tools/live-player/samples.md)).
5. Wybierz **Live Player** na liście urządzeń:

    ![Lista urządzeń zawiera opcję aplikacji Xamarin Live Player](install-images/devices-empty-windows.png)

    - Jeśli urządzenie zostało już przeprowadzone, będzie on dostępny jako opcja.
    - W przeciwnym razie zostanie wyświetlony monit parowania urządzenia, gdy jest to wymagane.

6. Naciśnij klawisz **Uruchom** przycisku lub wybierz jedną z następujących opcji z **Uruchom** lub kliknij prawym przyciskiem myszy menu:

    - **Rozpocznij bez debugowania** — możesz edytować aplikację i zobacz zmianach na urządzeniu (aplikacja zostanie ponownie uruchomiony jako zmian i zapisać pliku).
    - **Rozpocznij debugowanie** — można ustawić punktów przerwania i sprawdzanie zmiennych, ale nie można edytować kodu.

    Można także wybrać **Narzędzia > Xamarin Live Player > Live bieżącego widoku uruchamiania**, która pozwala edytować aplikację i zobacz zmianach na urządzeniu. Bieżący widok jest wyświetlany (zamiast ekranie głównym aplikacji).

7. Jeśli urządzenie jest już sparowana i aplikacja Xamarin Live Player jest uruchomiona na urządzeniu, ten kod będzie wykonywał razu!

    Jeśli urządzenie nie jest sparowana, kod QR pojawi się z instrukcjami parowania urządzenia:

    ![Para okno urządzenia](install-images/manage-empty-windows.png)

    Jeśli nie można skontaktować się z urządzenia do parowania, może wystąpić błąd.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2. Pobierz program Visual Studio dla komputerów Mac

Wymaga aplikacji Xamarin Live Player:

- OS X 10.11, z systemem macOS 10.12 lub nowszej
- Visual Studio for Mac
- Komputer Mac i urządzeń w tej samej sieci Wi-Fi

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Po raz pierwszy przy użyciu aplikacji Xamarin Live Player

1. Otwórz **programu Visual Studio dla komputerów Mac**.
2. Przejdź do **programu Visual Studio > Preferencje...**  i wybierz **projektów > Xamarin Live Player (wersja zapoznawcza)** kartę.
3. Znaczników **Włącz Xamarin Live Player**:

    [![Zaznacz pole Włącz Xamarin Live Player w oknie Opcje](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

4. Utwórz lub Otwórz projekt Xamarin (lub [przykładowe](~/tools/live-player/samples.md)).
5. Wybierz **Live Player** na liście urządzeń.

    ![Lista urządzeń zawiera opcję aplikacji Xamarin Live Player](install-images/devices.png)

    - Jeśli urządzenie zostało już przeprowadzone, będzie on dostępny jako opcja.
    - W przeciwnym razie zostanie wyświetlony monit parowania urządzenia, gdy jest to wymagane.
    - Wybierz **Xamarin Live Player urządzenia...**  do zarządzania urządzeniami, o których chcesz użyć przy użyciu aplikacji Xamarin Live Player.

6. Naciśnij klawisz **Uruchom** przycisku lub wybierz jedną z następujących opcji z **Uruchom** lub kliknij prawym przyciskiem myszy menu:

    ![Uruchamianie poleceń menu](install-images/run-menu.png)

    - **Rozpocznij bez debugowania** — możesz edytować aplikację i zobacz zmianach na urządzeniu (aplikacja zostanie ponownie uruchomiony jako zmian i zapisać pliku).
    - **Rozpocznij debugowanie** — można ustawić punktów przerwania i sprawdzanie zmiennych, ale nie można edytować kodu.
    - **Bieżącego widoku uruchamiania na żywo** — możesz edytować aplikację i zobacz zmianach na urządzeniu. Bieżący widok jest wyświetlany (zamiast ekranie głównym aplikacji).

7. Jeśli urządzenie jest już sparowana i aplikacja Xamarin Live Player jest uruchomiona na urządzeniu, ten kod będzie wykonywał razu!

    Jeśli urządzenie nie jest sparowana, kod QR pojawi się przy użyciu instrukcji dotyczących parowania urządzenia:

    ![Para okno urządzenia](install-images/manage-empty.png)

    Jeśli nie można skontaktować się z urządzenia do parowania, pojawi się błąd:

    ![Nie można nawiązać połączenia z komunikat o błędzie urządzenia](install-images/error-cannot-connect.png)

-----

Jeśli występują problemy związane, lub nie można połączyć, zobacz [ograniczenia i rozwiązywanie problemów z](~/tools/live-player/troubleshooting.md).

## <a name="related-links"></a>Linki pokrewne

- [Ograniczenia](~/tools/live-player/limitations.md)
- [Rozwiązywanie problemów](~/tools/live-player/troubleshooting.md)
- [Przykłady aplikacji Xamarin Live Player](~/tools/live-player/samples.md)
