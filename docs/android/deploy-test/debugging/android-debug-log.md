---
title: Dziennik debugowania dla systemu android
ms.topic: article
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/19/2018
ms.openlocfilehash: ec6536ee9bdd5f25a7f9ef90e5cf052717b23143
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2018
---
# <a name="android-debug-log"></a>Dziennik debugowania dla systemu android

Jeden często deweloperzy lewy umożliwia debugowanie aplikacji jest do łączenia się `Console.WriteLine`. Jednak na platformie przenośnych, takich jak Android nie konsoli. Urządzenia z systemem android zawiera dziennik używanego podczas pisania aplikacji. Jest to czasami określane jako _logcat_ z powodu polecenie wpisz ją pobrać. Użyj **dziennika debugowania** narzędzia, aby wyświetlić zarejestrowane dane.

## <a name="android-debug-log-overview"></a>Omówienie dziennika debugowania dla systemu android

**Dziennika debugowania** narzędzie zapewnia sposób wyświetlania danych wyjściowych dziennika podczas debugowania aplikacji. Dziennik debugowania obsługuje następujące urządzenia:

-   Fizyczny telefony, tablety i wearables.
-   Urządzenie Android wirtualne uruchomione na Emulator systemu Google Android. 

> [!NOTE]
> **Dziennika debugowania** narzędzie nie obsługuje Xamarin Player na żywo.


## <a name="accessing-the-debug-log-from-visual-studio"></a>Uzyskiwanie dostępu do dzienników debugowania w programie Visual Studio

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby otworzyć **dziennika urządzenia** narzędzia, kliknij przycisk **dziennika urządzenia (logcat)** ikonę na pasku narzędzi:

[![Lokalizacja dziennika urządzenia narzędzia na pasku narzędzi](android-debug-log-images/vswin-01-logcat-sml.png)](android-debug-log-images/vswin-01-logcat.png#lightbox)

Alternatywnie uruchamianie **dziennika urządzenia** narzędzie z jednego z następujących opcji menu:

-   **Widok > innych okien > dziennika urządzenia**
-   **Narzędzia > Android > dziennika urządzenia**

Poniższy zrzut ekranu przedstawia różne części **narzędzia debugowania** okno:

[![Części okna narzędzia debugowania](android-debug-log-images/vswin-03-features-sml.png)](android-debug-log-images/vswin-03-features.png#lightbox)

-   **Selektor urządzenia** &ndash; wybiera, które urządzenia fizycznego lub działającego emulatora do monitorowania.

-   **Pozycje dziennika** &ndash; tabeli komunikatów dziennika z logcat.

-   **Wyczyść wpisy dziennika** &ndash; usuwa wszystkie bieżące wpisy dziennika z tabeli.

-   **Odtwórz/Wstrzymaj** &ndash; przełącza między aktualizowania lub wstrzymując wyświetlanie nowych wpisów dziennika.

-   **Zatrzymaj** &ndash; zatrzymuje wyświetlanie nowych wpisów dziennika.

-   **Pole wyszukiwania** &ndash; ciągów wyszukiwania wprowadź w tym polu, aby filtrować pod kątem podzbiór pozycje dziennika.


Gdy **dziennika debugowania** okna narzędzia jest wyświetlana, użyj menu rozwijanego urządzenia, aby wybrać urządzeniu z systemem Android do monitorowania:

[![Lokalizacja selektora urządzenia](android-debug-log-images/vswin-02-devices-combo-sml.png)](android-debug-log-images/vswin-02-devices-combo.png#lightbox)

Po wybraniu urządzenia **dziennika urządzenia** narzędzie automatycznie dodaje wpisy dziennika z uruchomionej aplikacji &ndash; te wpisy dziennika są wyświetlane w tabeli Wpisy dziennika. Przełączanie między urządzeniami zatrzymuje i uruchamia rejestrowania urządzenia. Należy pamiętać, że projekt systemu Android muszą być ładowane przed dowolnego urządzenia będą wyświetlane w selektorze urządzenia. Jeśli urządzenie nie ma w selektorze urządzenia, sprawdź, czy jest dostępne w menu rozwijanym urządzenia Visual Studio obok **Start** przycisku.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby otworzyć **dziennika urządzenia**, kliknij przycisk **Widok > konsole > urządzenia dziennika**:

[![Lokalizacja dziennika urządzenia elementu menu](android-debug-log-images/vsmac-01-logcat-sml.png)](android-debug-log-images/vsmac-01-logcat.png#lightbox)

Poniższy zrzut ekranu przedstawia różne części **narzędzia debugowania** okno:

[![Funkcje okna narzędzia debugowania](android-debug-log-images/vsmac-03-features-sml.png)](android-debug-log-images/vsmac-03-features.png#lightbox)

-   **Selektor urządzenia** &ndash; wybiera, które urządzenia fizycznego lub działającego emulatora do monitorowania.

-   **Pozycje dziennika** &ndash; tabeli komunikatów dziennika z logcat.

-   **Wyczyść wpisy dziennika** &ndash; usuwa wszystkie bieżące wpisy dziennika z tabeli.

-   **Pole wyszukiwania** &ndash; ciągów wyszukiwania wprowadź w tym polu, aby filtrować pod kątem podzbiór pozycje dziennika.

-   **Pokaż komunikaty** &ndash; Włącza wyświetlanie wiadomości informacyjnych.

-   **Pokaż ostrzeżenia** &ndash; Włącza wyświetlanie ostrzeżenia (ostrzeżenia są oznaczone kolorem żółtym).

-   **Pokaż błędy** &ndash; Włącza wyświetlanie komunikatów o błędach (komunikaty ostrzegawcze są wyświetlane na czerwono).

-   **Połącz ponownie** &ndash; ponownie nawiąże połączenie z urządzeniem i odświeża wpisu dziennika.

-   **Dodaj znacznik** &ndash; wstawia komunikat znacznika (takich jak `--- Marker N ---`) po najnowsze wpis dziennika, gdy _N_ jest licznika, który rozpoczyna się od 1 i zwiększa 1 miarę dodawania nowych znaczników.

Po wyświetleniu okna narzędzia debugowania dziennika umożliwia urządzenia menu rozwijanego wybierz urządzenie z systemem Android do monitorowania:

[![Lokalizacja selektora urządzenia](android-debug-log-images/vsmac-02-devices-combo-sml.png)](android-debug-log-images/vsmac-02-devices-combo.png#lightbox)

Po wybraniu urządzenia **dziennika urządzenia** narzędzie automatycznie dodaje wpisy dziennika z uruchomionej aplikacji &ndash; te wpisy dziennika są wyświetlane w tabeli Wpisy dziennika. Przełączanie między urządzeniami zatrzymuje i uruchamia rejestrowania urządzenia. Należy pamiętać, że projekt systemu Android muszą być ładowane przed dowolnego urządzenia będą wyświetlane w selektorze urządzenia. Jeśli urządzenie nie ma w selektorze urządzenia, sprawdź, czy jest dostępne w menu rozwijanym urządzenia Visual Studio obok **Start** przycisku.

-----


## <a name="accessing-from-the-command-line"></a>Uzyskiwanie dostępu z poziomu wiersza polecenia

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Innym rozwiązaniem jest wyświetlanie dzienników debugowania przy użyciu wiersza polecenia. Otwórz okno wiersza polecenia i przejdź do folderu narzędzi platformy zestawu SDK systemu Android (zazwyczaj folderu narzędzi platformy zestawu SDK znajduje się w **C:\\Program Files (x86)\\Android\\zestawu sdk systemu android\\ narzędzia platformy**).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Innym rozwiązaniem jest wyświetlanie dzienników debugowania przy użyciu wiersza polecenia. Otwórz okno terminala i przejdź do folderu narzędzi platformy zestawu SDK systemu Android (zazwyczaj folderu narzędzi platformy zestawu SDK znajduje się w **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/platform-tools**).

-----

Jeśli tylko pojedyncze urządzenie (fizyczne urządzenia lub emulatora) jest dołączony, dziennik można wyświetlić, wprowadzając następujące polecenie:

```shell
$ adb logcat
```

Jeśli więcej niż jedno urządzenie jest podłączone, urządzenia muszą być jawnie określone. Na przykład **logcat -d adb** Wyświetla dziennik tylko fizyczne urządzenia podłączone, podczas gdy **logcat -e adb** przedstawia dziennik uruchomionym emulatorem tylko.

Kolejne polecenia nie można znaleźć, wprowadzając **adb** i odczytywania wiadomości pomocy.


## <a name="writing-to-the-debug-log"></a>Zapisywanie w dzienniku debugowania

Komunikaty mogą być zapisywane **debugowania dziennika** przy użyciu metody [Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) klasy.
Na przykład: 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

To generuje dane wyjściowe podobne do następujących:

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```

## <a name="interesting-messages"></a>Interesujące wiadomości

Podczas odczytywania dziennika (i szczególnie gdy dostarczanie dziennik wstawki do innych użytkowników), perusing pliku dziennika w całości często jest zbyt skomplikowane.
Aby ułatwić nawigowania komunikatów dziennika, należy rozpocząć do odszukania wpisu dziennika, podobny do następującego:

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

W szczególności Wyszukaj wiersz zgodny z wyrażeniem regularnym, zawierający nazwę pakietu aplikacji:

```shell
^I.*ActivityManager.*Starting: Intent
```

Jest to wiersz odpowiadający uruchomienia działania i *większość* (ale nie wszystkie) z następujących komunikatów odnoszą się do aplikacji.

Zwróć uwagę, że każdy komunikat zawiera identyfikator procesu (pid) proces generowania wiadomości. W powyższych `ActivityManager` komunikatu, proces `12944` generowany komunikat. Aby ustalić, który proces jest procesem debugowanej aplikacji, wyszukaj **mono. MonoRuntimeProvider** komunikat: 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

Ten komunikat pochodzi z procesu, który został uruchomiony. Wszystkie kolejne komunikaty, które zawierają ten identyfikator PID procesu pochodzić z tego samego procesu.
