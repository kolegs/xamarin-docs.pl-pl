---
title: Procedury kontroli aplikacji na żywo
description: Ten dokument zawiera opis sposobu kontroli aplikacji za pomocą Inspektora Xamarin. Zawiera omówienie również ograniczeń tego narzędzia inspektora Xamarin.
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: 67cc6b42901521226322d964514f19b4b639148b
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268826"
---
# <a name="inspecting-live-applications"></a>Procedury kontroli aplikacji na żywo

Kontroli aplikacji na żywo jest dostępna dla klientów korporacyjnych.

1. Otwórz dowolną [obsługiwane projektu aplikacji](~/tools/inspector/install.md#supported-platforms) w Visual Studio for Mac lub Visual Studio.
1. Uruchom aplikację w trybie debugowania.
1. Kliknij przycisk **inspekcję** przycisku w pasku narzędzi IDE (w programie Visual Studio **inspekcję bieżącej aplikacji...**  element menu jest również dostępna z **narzędzia** lub **debugowania** menu).

[![](inspect-images/mac-heres-the-button.png "Kliknij przycisk Inspekcja, na pasku narzędzi IDE")](inspect-images/mac-heres-the-button.png#lightbox)

Nowe okno klienta inspektora Xamarin zostanie otwarty z monit świeżych REPL.

[![](inspect-images/inspector-0.7.0-map-inspect-small.png "Zostanie otwarte nowe okno klienta inspektora Xamarin, świeże monitu REPL")](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

Gdy pojawi się to okno, masz C# monitu interakcyjnego używanej do wykonywania i ocena wyrażeń i C# instrukcje. Co sprawia, że to unikatowy jest, że kod jest obliczane w kontekście procesu docelowego. W takim przypadku możemy są wyświetlane kodu uruchamiania aplikacji dla systemu iOS wyświetlane.

Wszelkie zmiany wprowadzone do stanu aplikacji są naprawdę procesów docelowych, dzięki czemu można używać C#, aby zmienić aplikacji na żywo, lub możesz sprawdzić stan aplikacji na żywo.

Na przykład w systemach iOS, firma Microsoft może być zlokalizować naszych UIApplication klasy delegata, która jest naszym głównym sterownika (gdzie są przechowywane dużo stan aplikacji):

    var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
    del.Database.GetAllCustomers ()
    ...
    del.Database.AddCustomer (...)

(Należy pamiętać, że każdego przesłania występuje w edytorze wielowierszowy. `Shift + Enter` zostanie utworzony nowy wiersz, i `Cmd + Enter` (`Ctrl + Enter` w systemie Windows) będzie przesłać kodu do oceny. `Enter` automatycznie przesyła gdy będzie to bezpieczne.)

To wygodny sposób na uzyskanie dostępu do elementów wizualnych aplikacji przy użyciu przycisku "Inspekcja". Po naciśnięciu klawisza, można wybrać elementu interfejsu użytkownika, klikając w aplikacji. Zmienna `selectedView` zostanie przypisana do punktu do rzeczywistego elementu na ekranie. Na zrzucie ekranu powyżej, widać, jak firma Microsoft dostęp do, a następnie edytować `selectedView.BarTintColor` na `UISearchBar` wybranych liczników firma Microsoft.

Dynamiczne drzewo wizualne jest również przydatne. Reprezentuje bieżącą migawką hierarchii widoku. Możesz wybrać wiersze, aby ustawić `selectedView` w REPL i wartości właściwości widoku. Dla komputerów Mac zakłócają wizualizacji rozsunięty 3W warstwowego widoków. W systemie Windows można edytować wartości właściwości widoku wizualnie.

## <a name="known-limitations"></a>Znane ograniczenia

 - Wybór widoku jest obsługiwana tylko na ekranie głównym.
 - Edycja siatki właściwości nie jest dostępna dla komputerów Mac, a w systemie Windows jest ograniczony do kilku typów danych. Użyj REPL do bardziej zaawansowanych edycji.
 - Tak długo, jak inspektora addin/rozszerzenie jest zainstalowane i włączone w środowiskiem IDE, możemy są iniekcję kodu do aplikacji za każdym razem, gdy rozpoczyna się ona w trybie debugowania. Jeśli zauważysz wszelkie nietypowe zachowanie w aplikacji, skontaktuj się z spróbuj wyłączenie lub odinstalowywania dodatku inspektora/rozszerzenia, restartowanie IDE i ponowne sprawdzanie. I sprawdź [pliku usterki](~/tools/inspector/install.md#reporting-bugs) aby poinformować nas!
 - Jeśli sprawdzenie elementu interfejsu użytkownika powoduje, że aby mimo to zmienić, skontaktuj się z [Daj nam znać](~/tools/inspector/install.md#reporting-bugs), ponieważ może to wskazywać na błąd.

