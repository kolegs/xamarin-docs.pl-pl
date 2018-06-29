---
title: Xamarin Profiler
description: Ten przewodnik opisuje najważniejsze funkcje profilera Xamarin. Wygląda na profilowania, profilowania i kiedy należy użyć i na standardowe przepływu pracy do profilowania aplikacji platformy Xamarin.
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
author: topgenorth
ms.author: toopge
ms.date: 06/03/2018
ms.openlocfilehash: 8882cb9cd84940e12865a730f75e36ecbaf9b6f0
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066679"
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_Ten przewodnik opisuje najważniejsze funkcje profilera Xamarin. Wygląda na profilowania, profilowania i kiedy należy użyć i na standardowe przepływu pracy do profilowania aplikacji platformy Xamarin._

Powodzenie aplikacji zależy od środowiska użytkownika końcowego. Deweloperzy mogą zostały zaimplementowane niektóre funkcje naprawdę świetny w aplikacji, ale jeśli aplikacja jest wolna lub pełnego awarii, użytkownik będzie prawdopodobnie pozbyć się go.

W przeszłości Mono ma wszystkie funkcje zaawansowane, wiersza polecenia profilera zbieranie informacji na temat programom działającym w środowisku wykonawczym Mono wywoływanych [profilera Mono dziennika](http://www.mono-project.com/docs/debug+profile/profile/profiler/). Xamarin Profiler interfejsu graficznego profilera Mono dziennika i obsługuje profilowania systemu Android, iOS, systemu tvOS i aplikacji dla komputerów Mac w systemie Mac i Android, iOS oraz systemu tvOS aplikacji w systemie Windows.

Xamarin Profiler ma liczbę dokumentów, które są dostępne dla profilowania — alokacji, cykle i czasu profilera. W tym przewodniku Eksploruje pomiaru tych dokumentów i sposób ich analizowania aplikacji i wyjaśnia, w rozumieniu dane wyświetlane na poszczególnych ekranach.

W tym przewodniku sprawdza typowych scenariuszy profilowania i wprowadza profilera jako narzędzie można ułatwić analizowanie i zoptymalizować iOS i Android aplikacji.

## <a name="download-and-install"></a>Pobierz i zainstaluj

> [!NOTE]
> Musisz być [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/) subskrybenta, aby odblokować tę funkcję w albo program Visual Studio Enterprise w systemie Windows lub programu Visual Studio dla komputerów Mac na komputerach Mac.

Xamarin Profiler to aplikacja autonomiczna i jest zintegrowana z programu Visual Studio for Mac i Visual Studio włączyć profilowanie z w środowisku IDE.

Pobierz pakiet instalacyjny dla danej platformy:

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/profiler-windows.msi)

Po pobraniu, uruchom Instalatora, aby dodać profilera Xamarin do systemu.

## <a name="profilers-and-profiling"></a>Profilery i profilowania

Profilowanie jest ważne i często ich krok w rozwoju aplikacji. Profilowanie jest formą **analizy dynamicznej program** -analizę program, gdy jest uruchomiona i używany. Profiler jest narzędziem do wyszukiwania danych, które zbiera informacje o czasie złożoność, użycie określonej metody oraz pamięci są przydzielone. Profiler umożliwia głębokość przejść do szczegółów i analizować te metryki w celu określenia obszarów problemów w kodzie.

Podczas projektowania i tworzenia aplikacji, ważne jest, aby nie Optymalizuj przedwcześnie; oznacza to poświęcany czas, tworzenie kodu w obszarach, które rzadko uzyskuje się dostęp. Jest to zasilania profilowania. Profiler zapewnia wgląd w najbardziej najczęściej używanych części kodu podstawowej i pomaga zlokalizować obszary, w którym należy poświęcić czas wprowadzania ulepszenia. Deweloperzy należy zwrócić uwagę, aby zrozumieć, gdzie jest zużywany w większości przypadków w aplikacji i sposobie użycia pamięci przez aplikację.

Profilowanie jest przydatne w przypadku wszystkich typów programowanie, ale jest to szczególnie istotne w przenośnych programowanie. Zoptymalizowanego kodu jest bardziej widoczne na platformach mobilnych niż na komputerach stacjonarnych i Powodzenie aplikacji zależy od doskonałych i zoptymalizowany kod, który działa wydajnie.

## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin Profiler udostępnia deweloperom sposób do profilu aplikacji w programie Visual Studio dla komputerów Mac lub Visual Studio. Profiler zbiera i wyświetla informacje o aplikacji, które mogą następnie używane przez dewelopera, do analizy zachowania aplikacji. Istnieje wiele różnych sposobów do profilu aplikacji przy użyciu profilera Xamarin, czyli profilowania pamięci i statystyczne próbkowania. Są prowadzone za pośrednictwem alokacji i czasu profilera instruments odpowiednio.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Obecnie profilera Xamarin może służyć do testowania aplikacji platformy Xamarin.iOS, Xamarin.Android i Xamarin.Mac dla komputerów Mac (za pomocą programu Visual Studio for Mac). Profiler jest procesem oddzielne z IDE i tak, oprócz uruchamianie z programu Visual Studio dla komputerów Mac, może służyć jako aplikacja autonomiczna zbadanie .exe i `.mlpd` pliki, które zostały utworzone z [profilera mono dziennika](http://www.mono-project.com/docs/debug+profile/profile/profiler/).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Obecnie profilera Xamarin może służyć do testowania aplikacji platformy Xamarin.Android w systemie Windows (za pomocą programu Visual Studio i Visual Studio for Mac). Profiler jest procesem oddzielne z IDE i tak, oprócz uruchamianie z programu Visual Studio, może służyć jako aplikacja autonomiczna zbadanie .exe i `.mlpd` pliki, które zostały utworzone z [profilera mono dziennika](http://www.mono-project.com/docs/debug+profile/profile/profiler/) .

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>Obsługa profilera

Obsługa platformy Xamarin Profiler jest dostępna na następujących platformach:

 - Visual Studio for Mac (system macOS — z licencji Enterprise)
    - Android
        - Urządzenia i emulatora
    - iOS
        - Urządzenia i symulatora
    - systemu tvOS (czas dokumentu nie jest obsługiwane)
        - Urządzenia i symulatora
    - Mac

 - Visual Studio (tylko **Enterprise** wersji)
    - Android
        - Urządzenia i emulatora
    - iOS [Experimental]
        - Urządzenia i symulatora
    - tvOS
        - Urządzenia i symulatora

Należy pamiętać, że możesz **tylko** profilu **debugowania** konfiguracji.

## <a name="profiler-basics"></a>Podstawy profilera

W tej sekcji wprowadzono części Profiler platformy Xamarin i tours jego funkcje.

### <a name="allow-profiling-in-your-app"></a>Zezwalaj na profilowania w aplikacji

Zanim można pomyślnie profilu aplikacji, należy umożliwić profilowania w aplikacji Opcje projektu.

 - iOS:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  **Tworzenie > iOS debugowania > Włącz profilowanie**

  ![](images/ios-options-mac.png "okno dialogowe Opcje programu Visual Studio for Mac systemu iOS")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **Właściwości > kompilacji systemu iOS > Włącz profilowanie**

  ![](images/ios-project-options-vs.png "okno dialogowe Opcje programu Visual Studio iOS")

-----

 - Android:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  **Tworzenie > Android debugowania > Włącz Developer Instrumentacji**

  ![Okno dialogowe Opcje systemu android w programie Visual Studio dla komputerów Mac](images/android-project-options.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **Tworzenie > Android debugowania > Włącz Developer Instrumentacji**

  ![Okno dialogowe Opcje systemu android w programie Visual Studio dla komputerów Mac](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>Uruchamianie profilera

Xamarin Profiler może uruchomić ze środowiskiem IDE, gdy są profilowania systemu iOS lub Android aplikacji lub jako aplikacja autonomiczna.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="launching-from-visual-studio-for-mac"></a>Uruchamianie z programu Visual Studio dla komputerów Mac

1. Najpierw upewnij się, aplikacja załadowany w programie Visual Studio dla komputerów Mac i wybierz konfigurację debugowania (ustawienie domyślne).
2. Przejdź do **Uruchom > Rozpocznij profilowanie**w programie Visual Studio dla komputerów Mac, lub **Analizuj > profilera Xamarin** w programie Visual Studio, aby otworzyć profilera, jak pokazano na poniższym diagramie:

  ![](images/start-profiling-xs.png "Uruchamianie programu profilującego z programu Visual Studio dla komputerów Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="launching-from-visual-studio"></a>Uruchamianie z programu Visual Studio

1.  Najpierw upewnij się, aplikacja załadowany w programie Visual Studio i wybierz konfigurację debugowania (ustawienie domyślne), jako określonej powyżej.
2.  Przejdź do **Analizuj > profilera Xamarin** w programie Visual Studio, aby otworzyć profilera, jak pokazano na poniższym diagramie:

![Uruchamianie programu profilującego z programu Visual Studio](images/start-profiling-vs.png)

-----

Elementy menu są niewidoczne, zapoznaj się [przewodnik rozwiązywania problemów](~/tools/profiler/troubleshooting.md).

Uruchamia profilera i automatycznie uruchamia profilowanie aplikacji.

Profiler może służyć do pomiaru wydajności i pamięci. Powoduje to osiągnięcie za pośrednictwem alokacji i czasu profilera dokumentów, których firma Microsoft może zapoznać się szczegółowo w następnej sekcji.

#### <a name="saving-and-loading-profiler-sessions"></a>Zapisywanie i ładowanie sesji profilera

Aby zapisać sesję profilowania w dowolnym momencie, wybierz **Plik > Zapisz jako...**  na pasku Menu profilera. Spowoduje to zapisanie pliku w _mlpd_ format, format specjalnych, wysokiej skompresowanych danych profilowania.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po zakończeniu instalacji profilera Xamarin można znaleźć w folderze aplikacji, jak pokazano na poniższym zrzucie ekranu:

![](images/applications.png "Otwórz autonomiczny profilera z komputerów Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po zakończeniu instalacji aplikacji platformy Xamarin profilera znajduje się w katalogu aplikacji:

![](images/applications-vs.png "Otwórz autonomiczny profilera z systemu Windows ")

-----

Można załadować *.mlpd* plików do profilera, otwierając aplikacja autonomiczna, wybierając **wybierz docelowy** i ładowania pliku.

Aby uzyskać więcej informacji, zobacz [Generowanie plików .mlpd ](~/tools/profiler/troubleshooting.md#gen_mlpd).

## <a name="profiler-features"></a>Funkcje profilera

Xamarin Profiler składa się z pięciu sekcji, jak przedstawiono poniżej:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](images/profiler-mac-sml.png "Profiler sekcje w programie Visual Studio dla komputerów Mac")](images/profiler-mac.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/profiler-vs.png "Profiler sekcje w programie Visual Studio")](images/profiler-vs.png#lightbox)

-----

- **Pasek narzędzi** — znajduje się w górnej części profilera, zapewnia to opcje uruchamiania/zatrzymywania profilowania, wybierz proces docelowy, Wyświetl czas działania aplikacji i wybierz widokami złożonymi tworzące aplikacji profilera.
- **Instrumentacja listy** — ta lista zawiera wszystkie narzędzia ładowany w sesji profilowania.
- **Wykreślenia wykresu** — te wykresy poziomie dotyczą odpowiednich dokumentów na liście dokumentu. Suwaka (wyświetlone poniżej Profiler czasu) może służyć do zmiany skali.
- **Instrumentacja obszaru szczegółów** -zawiera dane wyświetlane przez wybrany widok bieżącego dokumentu. Zajmiemy tych widoków bardziej szczegółowo w poniższej sekcji.
- **Widok inspektora** — zawiera sekcje, które można wybrać przez formant segmentu. Sekcje są zależne od dokumentu zaznaczone i obejmuje: ustawienia konfiguracji, statystyki, informacje śledzenia stosu i ścieżkę do katalogów głównych.

### <a name="allocations"></a>Alokacje

Dokument alokacji zawiera szczegółowe informacje dotyczące obiektów w aplikacji, jak są one tworzone i w ramach odzyskiwania pamięci.

W górnej części profiler jest wykres alokacji, który przedstawia ilość pamięci przydzielona w regularnych odstępach czasu podczas profilowania. Obecnie wykres alokacji jest łącznej liczby przydziałów i nie Rozmiar sterty w danym momencie. W tym sensie nigdy nie zostanie umieszczona w dół, tylko zwiększy. W tym obiektów przydzielony na stosie. W zależności od wersji środowiska uruchomieniowego, używane wykres może różnić się — nawet w przypadku tej samej aplikacji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](images/allocations1.png "Alokacje dokumentu")](images/allocations1.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/allocations1-vs.png "Alokacje dokumentu")](images/allocations1-vs.png#lightbox)

-----

Istnieją różne widoki danych w dokumencie alokacji, które umożliwiają deweloperom analizowanie jak przy użyciu i zwolnić pamięć, ich aplikacji. Poniżej opisano następujące widoki:

 -   **Alokacje** — to zostanie wyświetlona lista wszystkich alokacji i grupuje je według nazwy klasy. To omówienie doskonałą klas i metod, jak często są one używane i zbiorowe rozmiar klasy używane. Dwukrotne kliknięcie klasy wyświetli pamięć przydzielona: 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations3.png "Karta alokacji")](images/allocations3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations2-vs.png "Karta alokacji")](images/allocations2-vs.png#lightbox)

-----

Widok inspektora alokacji zawiera opcje filtrowania i grupowania obiektów, zapewniając statystyk na pamięć przydzielona i alokacji górny, a także widoki ślad stosu i ścieżka do katalogu głównego.

 -   **Drzewie wywołań** — to wyświetla drzewie wywołań całego wszystkich wątków w aplikacji i zawiera informacje na temat pamięć przydzielona w każdym węźle. Gdy element jest zaznaczony na liście, wszystkie węzły równorzędne będą wyświetlane w kolorze szarym. Można rozwinąć drzewo lub kliknij dwukrotnie element, aby przejść do niego. Podczas wyświetlania tego widoku danych, wyświetlania widoku inspektora ustawienia można zmienić sposób są one przedstawiane w. Obecnie dostępne są dwie opcje:
    1.  **Odwrócony drzewa wywołań** — to uwzględnia ślad stosu od góry do dołu. Jest to opcję widoku wygodny jak wskazuje najgłębszym metody gdzie Procesor ma zostały wydatków jego czas.
    2.  **Oddzielne przez wątek** — ta opcja umożliwia organizowanie drzewo wywołań przez wątek.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations2.png "Karta drzewa wywołań")](images/allocations2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations3-vs.png "Karta drzewa wywołań")](images/allocations3-vs.png#lightbox)

-----

 -   **Migawki** — w tym okienku Wyświetla informacje o migawki pamięci. Aby wygenerować te podczas profilowania aktywnej aplikacji, kliknij przycisk _aparatu_ przycisku w pasku narzędzi w każdym punkcie, którą chcesz jakie pamięci jest zachowywana i wydane. Następnie możesz kliknąć każdej migawki, aby zapoznać się z tym, co dzieje się pod maską. Należy pamiętać, że migawki mogą być wykonywane tylko po na żywo, profilowania aplikacji. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations4.png "Karta migawki")](images/allocations4.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations4-vs.png "Karta migawki")](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>Profiler czasu

Dokument Profiler czasu mierzy dokładnie czas, jaki jest poświęcony w każdej metodzie aplikacji. Aplikacja jest wstrzymana w regularnych odstępach czasu i ślad stosu jest uruchamiany na każdym aktywnego wątku. Każdy wiersz w obszarze szczegółów dokumentu zawiera ścieżki wykonywania, która została po.

Wykres skrzynkowy jak pokazano na zrzucie ekranu poniżej, wyświetla liczbę próbek odebranych przez aplikację podczas działania:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Czas Instrumentacji profilera](images/time1.png)](images/time1.png#lightbox) 

[![Czas Instrumentacji profilera — lista próbek](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Czas Instrumentacji profilera](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![Czas Instrumentacji profilera — lista próbek](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----

- **Drzewie wywołań** — przedstawia ilość czasu przeznaczonego w każdej z metod:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/time2.png "Czas Instrumentacji profilera — drzewo wywołań")](images/time2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/time2-vs.png "Czas Instrumentacji profilera — drzewo wywołań")](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>Cykle

Przy użyciu języka C# i F # zarządzanego kodu może być dość często i Niestety bardzo łatwo utworzyć odwołania do obiektów, które nigdy nie zostaną usunięte. Niniejszy dokument umożliwia wskazanie tych obiektów, a następnie Wyświetl cykle, do których odwołuje się do aplikacji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Cykle dokumentu](images/cycles.m751-sml.png)](images/cycles.m751.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Cykle dokumentu](images/cycles-vs-sml.png)](images/cycles-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>Profilowanie aplikacji

Obecnie można sprofilować tylko domyślnej konfiguracji debugowania.

Jeśli profil aplikacji z dowolnej innej konfiguracji, zostanie wyświetlone następujące okno dialogowe komunikat:


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Profilowanie okna dialogowego błędu](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/image1vs.png "Profilowanie okna dialogowego błędu")](images/image1vs.png#lightbox) 

-----

Wybierz **aktualizacji** aby kontynuować.

### <a name="sgen-garbage-collector-and-profiling"></a>Moduł zbierający elementy bezużyteczne SGen i profilowania

[SGen](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) modułu zbierającego elementy bezużyteczne jest używany dla wszystkich platform Xamarin.

SGen jest pokoleniowej GC, który przydziela obiektów aplikacji do trzech stosów — szkółki, sterty główne i dużych przestrzeni obiektów. Umożliwi to lepszą wykonanie operacji wyrzucania elementów bezużytecznych. SGen — jest obecnie GC domyślny dla platformy Xamarin.Android oraz Xamarin.iOS Unified aplikacji.

Aplikacji platformy Xamarin.iOS przy użyciu klasycznego interfejsu API używany GC Boehm — zachowawcze, pokoleniowej modułu zbierającego elementy bezużyteczne. Jak jest zachowawcze, jest mniej prawdopodobne zwolnić dostępną pamięć, co może prowadzić do wyników niedokładne przy użyciu profilera. Z tego powodu wagi alokacji nie można używać z modułu zbierającego elementy bezużyteczne Boehm.

Gdy pojawi się okno dialogowe komunikat Jeśli aplikacja korzysta z wykazem Globalnym Boehm, Xamarin nie zaleca przełączenie istniejącej aplikacji systemu iOS, która używać Boehm do SGen bez zachować ostrożność podczas badania i testowania dokładnego. Xamarin nie zaleca również przełączanie do SGen do profilowania, a następnie przełączać się ponownie, zgodnie z tymi wynikami, nie będą umożliwiać dokładne wzorce użycia pamięci.

Aby uzyskać więcej informacji dotyczących zarządzania pamięcią, zapoznaj się [pamięci i najlepsze rozwiązania w zakresie wydajności](~/cross-platform/deploy-test/memory-perf-best-practices.md) przewodnik.

## <a name="summary"></a>Podsumowanie

W tym przewodniku analizujemy jakie profilowania jest i jak jest korzystne do deweloperów. Następnie wprowadzono profilera Xamarin, niektóre historii oraz informacji w jej działania. Na koniec możemy toured funkcje profilera Xamarin i zbadane, alokacji i czasu profilera narzędzia.

## <a name="related-links"></a>Linki pokrewne

- [Dobre praktyki dotyczące pamięci i wydajność](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Uwagi do wersji](https://developer.xamarin.com/releases/profiler/preview/)
