---
title: Ustawienia odtwarzacza na żywo Xamarin
description: Edytuj i testowania aplikacji w czasie rzeczywistym na urządzenia z systemem iOS lub Android
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: topgenorth
ms.author: toopge
ms.date: 11/22/2017
ms.openlocfilehash: 0f343e253a57de33d7e19e648862e6d11fa5af5f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="xamarin-live-player-setup"></a>Ustawienia odtwarzacza na żywo Xamarin

Xamarin Live Player umożliwia wprowadzanie zmian na żywo do aplikacji i mają te zmiany zostaną uwzględnione na żywo na urządzeniu. Wykonywania kodu w aplikacji platformy Xamarin Player na żywo — nie ma potrzeby konfigurowania emulatory lub wdrażanie przy użyciu kabli! W tym artykule opisano, jak skonfigurować Xamarin Player na żywo.

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

## <a name="1-get-the-app"></a>1. Pobierz aplikację

# <a name="androidtabandroid"></a>[Android](#tab/android)

Xamarin Player na żywo jest dostępna dla systemu Android ze sklepu Google Play:

[ ![Dostępne w witrynie Google Play](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Dla urządzeń z systemem Android bez Google Play Xamarin odtwarzacz na żywo jest dostępna za pośrednictwem [HockeyApp](https://aka.ms/xlp-hockeyapp) dystrybucji. Ponadto Podgląd wczesne kompilacje dla systemu Android mogą być instalowane bezpośrednio z witryny Google Play przez zgody na korzystanie z [otwarte w wersji beta programu](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

Firma Microsoft zachęca użytkowników do dołączenia do [aplikacji odtwarzacza Live Xamarin _z systemem iOS w wersji zapoznawczej_ ](https://aka.ms/liveplayeralpha) można uzyskiwać dostęp do najnowszych ulepszeń za pośrednictwem TestFlight.

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2. Pobierz program Visual Studio 2017 r.

Xamarin Player na żywo wymaga:

- 15.4 2017 r w usłudze Visual Studio lub nowszej.
- Visual Studio komputer i urządzenia w tej samej sieci Wi-Fi.

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Za pomocą platformy Xamarin Player na żywo po raz pierwszy

1. Otwórz **programu Visual Studio 2017**.
2. Przejdź do **Narzędzia > Opcje...**  i wybierz **Xamarin > innych** kartę.
3. Znaczników **włączyć Player na żywo Xamarin**:

  ![Zaznacz pole Włącz Player Xamarin na żywo w oknie Opcje](install-images/vs2017-options.png)

2. Utwórz lub Otwórz projektu Xamarin (lub [próbki](~/tools/live-player/samples.md)).
3. Wybierz **Live Player** na liście urządzeń:

  ![Lista urządzeń zawiera opcję Xamarin Live Player](install-images/devices-empty-windows.png)

  * Jeśli urządzenie zostało już przeprowadzone, będą dostępne jako opcję.
  * W przeciwnym razie zostanie wyświetlony monit, aby skojarzyć urządzenia, gdy jest to wymagane.
4. Naciśnij klawisz **Uruchom** przycisku lub wybierz jedną z następujących opcji z **Uruchom** lub prawym przyciskiem myszy:

  - **Uruchom bez debugowania** — można edytować aplikacji i zobacz zmian na urządzeniu (aplikacja zostanie ponownie uruchomiony jako zmian i zapisać plik).
  - **Rozpocznij debugowanie** — można ustawić punktów przerwania i sprawdzić zmiennych, ale nie można edytować kodu.

  Można także wybrać **Narzędzia > Xamarin Live Player > Live uruchom bieżący widok**, które umożliwia edytowanie aplikacji i zobacz zmian na urządzeniu. Bieżący widok jest wyświetlany (zamiast ekranie głównym aplikacji).

5. Jeśli urządzenie jest już sparowana i aplikacja Xamarin Player na żywo działa na urządzeniu, kod będzie wykonywał razu!

  Jeśli urządzenie nie jest parowania, kod QR pojawi się z instrukcjami, aby skojarzyć urządzenia:

  ![Para okno urządzenia](install-images/manage-empty-windows.png)

  Jeśli urządzenie nie można skontaktować się z parowania, może wystąpić błąd.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2. Pobierz program Visual Studio dla komputerów Mac

Xamarin Player na żywo wymaga:

- OS X 10.11 macOS 10.12 lub nowszego
- Visual Studio for Mac
- Mac i urządzeń w tej samej sieci Wi-Fi

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Za pomocą platformy Xamarin Player na żywo po raz pierwszy

1. Otwórz **programu Visual Studio for Mac**.
2. Przejdź do **programu Visual Studio > Preferencje...**  i wybierz **projekty > Player Live Xamarin (wersja zapoznawcza)** kartę.
3. Znaczników **włączyć Player na żywo Xamarin**:

  [![Zaznacz pole Włącz Player Xamarin na żywo w oknie Opcje](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

2. Utwórz lub Otwórz projektu Xamarin (lub [próbki](~/tools/live-player/samples.md)).
3. Wybierz **Live Player** na liście urządzeń.

  ![Lista urządzeń zawiera opcję Xamarin Live Player](install-images/devices.png)

  * Jeśli urządzenie zostało już przeprowadzone, będą dostępne jako opcję.
  * W przeciwnym razie zostanie wyświetlony monit, aby skojarzyć urządzenia, gdy jest to wymagane.
  * Wybierz **Xamarin na żywo Player urządzenia...**  do zarządzania urządzeniami, mają być używane z Xamarin Player na żywo.

4. Naciśnij klawisz **Uruchom** przycisku lub wybierz jedną z następujących opcji z **Uruchom** lub prawym przyciskiem myszy:

  ![Uruchamianie poleceń menu](install-images/run-menu.png)

  - **Uruchom bez debugowania** — można edytować aplikacji i zobacz zmian na urządzeniu (aplikacja zostanie ponownie uruchomiony jako zmian i zapisać plik).
  - **Rozpocznij debugowanie** — można ustawić punktów przerwania i sprawdzić zmiennych, ale nie można edytować kodu.
  - **Uruchom bieżący widok na żywo** — można edytować aplikacji i zobacz zmian na urządzeniu. Bieżący widok jest wyświetlany (zamiast ekranie głównym aplikacji).

5. Jeśli urządzenie jest już sparowana i aplikacja Xamarin Player na żywo działa na urządzeniu, kod będzie wykonywał razu!

  Jeśli urządzenie nie jest sparowana, kod QR pojawi się z instrukcjami skojarzyć urządzenia:

  ![Para okno urządzenia](install-images/manage-empty.png)

  Jeśli urządzenie nie można skontaktować się z parowania, pojawi się błąd:

  ![Nie można połączyć się komunikat o błędzie urządzenia](install-images/error-cannot-connect.png)


-----

Jeśli występują problemy lub nie można połączyć, zobacz [ograniczenia i rozwiązywanie problemów z](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>Linki pokrewne

- [Ograniczenia](~/tools/live-player/limitations.md)
- [Rozwiązywanie problemów](~/tools/live-player/troubleshooting.md)
- [Przykłady na żywo Player Xamarin](~/tools/live-player/samples.md)
