---
title: "Debugowanie wielu procesów"
ms.topic: article
ms.prod: xamarin
ms.assetid: 852F8AB1-F9E2-4126-9C8A-12500315C599
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: 9454c65298dbb5417f91765f541d22ae1d6137d7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="multi-process-debugging"></a>Debugowanie wielu procesów

Jest bardzo często dla nowoczesnych rozwiązań zostało utworzonych w programie Visual Studio dla komputerów Mac mają wiele projektów tego różnych platformach docelowych. Na przykład rozwiązanie może mieć projekt aplikacji mobilnej, który korzysta z danych dostarczonych przez projekt usługi sieci web. Podczas opracowywania tego rozwiązania, deweloper może być wymagane oba projekty równoczesne działanie Rozwiązywanie problemów. Począwszy od [9 cyklu wersji programu Xamarin](https://releases.xamarin.com/stable-release-cycle-9/), jest obecnie możliwe dla programu Visual Studio dla komputerów Mac do debugowania wiele procesów uruchomionych w tym samym czasie. Dzięki temu można ustawić punktów przerwania, Sprawdź zmienne i wyświetlić więcej niż jednym projekcie uruchomionych wątków. Jest to nazywane _debugowanie wielu procesów_. 

W tym przewodniku będzie omówiono niektóre zmiany wprowadzone w programie Visual Studio dla komputerów Mac do obsługi debugowanie wielu procesów, sposobu konfigurowania rozwiązania debugowanie wielu procesów i jak można dołączyć do istniejących procesów z programem Visual Studio dla komputerów Mac.

## <a name="requirements"></a>Wymagania

Debugowanie wielu procesów wymaga programu Visual Studio dla komputerów Mac.

## <a name="ide-changes"></a>Zmiany środowiska IDE

Aby pomóc deweloperom z wieloma procesem debugowania, Visual Studio for Mac przeszła niektóre zmiany w interfejsie użytkownika. Visual Studio for Mac zawiera zaktualizowaną **narzędzi debugowania**i nowy **konfiguracji rozwiązania** sekcji **opcje rozwiązania** folderu. Ponadto **konsoli wątków** będą teraz wyświetlane uruchamiania procesów i wątków dla każdego procesu. Visual Studio for Mac będą również wyświetlane wiele tablety debugowania, po jednej dla każdego procesu dla niektórych elementów, takich jak **danych wyjściowych aplikacji**.

### <a name="solution-configurations"></a>Konfiguracje rozwiązania

Domyślnie program Visual Studio for Mac spowoduje wyświetlenie pojedynczego projektu w **konfiguracji rozwiązania** obszar paska narzędzi debugowania. Po zainicjowaniu sesji debugowania, jest to projekt, który uruchomi i Dołącz debuger do programu Visual Studio for Mac.

Aby uruchomić i debugowanie wielu procesów w programie Visual Studio dla komputerów Mac, jest niezbędne do utworzenia _konfiguracji rozwiązania_. Konfiguracja rozwiązania opisano, jakie projektów w rozwiązaniu powinny być włączone po zainicjowaniu sesji debugowania jednym kliknięciem **Start** przycisk lub podczas &#8984; &#8617; (**Wprowadź Cmd**) zostanie naciśnięty. Poniższy zrzut ekranu jest stanowi przykład rozwiązania w programie Visual Studio dla komputerów Mac, który ma wiele konfiguracji rozwiązania:

![](multi-process-debugging-images/mpd01-xs.png "Rozwiązania z wielu konfiguracji rozwiązania")

### <a name="parts-of-the-debug-toolbar"></a>Części paska narzędzi debugowania

Na pasku narzędzi debugowania została zmieniona umożliwia konfigurację rozwiązania, należy wybrać za pomocą menu podręcznego. Ten zrzut ekranu przedstawia części paska narzędzi debugowania:

![](multi-process-debugging-images/mpd02-xs.png "Części paska narzędzi debugowania")

1. **Konfiguracja rozwiązania** — można ustawić konfiguracji rozwiązania, klikając konfiguracji rozwiązania na pasku narzędzi debugowania i wybierając z menu podręcznego konfiguracji:

    ![](multi-process-debugging-images/mpd03-xs.png "Przykład okna podręcznego o konfiguracje rozwiązania")

2. **Tworzenie docelowego** -identyfikuje element docelowy kompilacji dla projektów. To jest takie same jak w poprzednich wersjach programu Visual Studio dla komputerów Mac.
3. **Wartości docelowe urządzeń** — po wybraniu tej opcji urządzenia, które rozwiązanie będzie uruchamiany na. Można zidentyfikować osobne urządzenia lub emulatora dla każdego projektu.:

    ![](multi-process-debugging-images/mpd04-xs.png "Menu podręczne przedstawiający urządzeń dla projektu")

### <a name="multiple-debug-pads"></a>Wiele tablety debugowania

Po uruchomieniu konfiguracji rozwiązania wielu część programu Visual Studio for Mac tablety pojawi się wiele razy, po jednej dla każdego procesu. Na przykład poniższy zrzut ekranu zawiera dwa **danych wyjściowych aplikacji** podkładki dla rozwiązania, które działa dwa projekty:

![](multi-process-debugging-images/mpd05-xs.png "Konsola danych wyjściowych dla konfiguracji rozwiązania")

### <a name="multiple-processes-and-the-active-thread"></a>Wiele procesów i _aktywnego wątku_

Po napotkaniu punktu przerwania w jednym procesie tego procesu wstrzyma wykonywania, podczas gdy inne procesy nadal działać. W przypadku jednego procesu Visual Studio for Mac można łatwo wyświetlić informacje, takie jak wątków, zmienne lokalne, danych wyjściowych aplikacji w jednym zestawie konsole. Jednak w przypadku wielu procesów z wielu punktów przerwania i potencjalnie wiele wątków można okaże utrudnione deweloperowi radzenia sobie z informacjami z sesji debugowania, która próbuje go wyświetlać wszystkie informacje ze wszystkich wątków (i procesów) na raz.

Aby rozwiązać ten problem, Visual Studio dla komputerów Mac w czasie będą wyświetlane tylko dane z jednego wątku, jest to nazywane _aktywnego wątku_. Pierwszy wątek, który wstrzymuje w punkcie przerwania jest uważany za _aktywnego wątku_. Aktywnego wątku jest wątku, który jest celem dewelopera uwagi. Debugowanie poleceń, takich jak **Step Over** &#8679; &#8984; O (Shift-Cmd-O), które zostaną wystawione do aktywnego wątku.

**Konsoli wątku** będą wyświetlane informacje dotyczące wszystkich procesów i wątków, które podlegają kontroli w konfiguracji rozwiązania i podaj wizualnych jest aktywnym wątkiem:

![](multi-process-debugging-images/mpd06-xs.png "Konsola wątku dla konfiguracji rozwiązania")

Wątki są pogrupowane według proces, który jest hostem je. Nazwa projektu i Identyfikatora wątku active zostaną wyświetlone pogrubioną czcionką, a strzałka wskazująca w prawo pojawią się w odstępu obok aktywnego wątku. W poprzednim zrzucie ekranu **wątku #1** w **identyfikator 48703 procesu** (**FirstProject**), jest aktywnym wątkiem.

Debugowanie wielu procesów, jest możliwość active wątku, aby wyświetlić informacje dla tego procesu (lub wątku) debugowania za pomocą **konsoli wątku**. Aby przełączyć się aktywny wątku, wybierz żądany wątku **konsoli wątku** , a następnie kliknij go dwukrotnie w.

#### <a name="stepping-through-code-when-multiple-projects-are-stopped"></a>Krokowe wykonywanie kodu po wielu projektów zostały zatrzymane.

Gdy dwie (lub więcej) projektów ma punktów podziału, Visual Studio for Mac wstrzyma oba procesy. Możliwe jest tylko **Step Over** kodu w aktywnym wątkiem. Inny proces zostanie wstrzymana, aż do zmiany zakresu umożliwia debugera tak, aby przełączyć fokus z aktywnym wątkiem. Rozważmy na przykład poniższy zrzut ekranu programu Visual Studio for Mac debugowania dwa projekty:

![](multi-process-debugging-images/mpd09-xs.png  "Visual Studio for Mac debugowania dwa projekty")

Na tym zrzucie ekranu pokazano poszczególnych rozwiązań ma własny punkt przerwania. Gdy debugowanie się rozpocznie, pierwszy punkt przerwania występujących znajduje się na **wiersz 10** z `MainClass` w **SecondProject**. Ponieważ oba projekty punktów przerwania, każdy proces jest zatrzymywane. Po napotkaniu punktu przerwania każdego wywołania elementu **Step Over** spowoduje, że Visual Studio dla komputerów Mac do Przekrocz nad kodu w aktywnym wątkiem.

Krokowe wykonywanie kodu jest ograniczona do aktywnego wątku, więc programu Visual Studio for Mac będzie krok za pomocą wiersza kodu w czasie, podczas tego procesu jest wstrzymany.

Przy użyciu poprzedni zrzut ekranu przedstawia przykład podczas `for` pętli zakończeniu programu Visual Studio dla komputerów Mac umożliwiałyby **FirstProject** do momentu zakończenia punktu przerwania w **wiersz 11** w `MainClass` został Napotkano. Dla każdego **Step Over** poleceń, debuger będzie wiersz po wierszu w dojściu **FirstProject**, dopóki wewnętrzne algorytmy heurystyczne programu Visual Studio dla komputerów Mac przełącznika aktywnego wątku z powrotem do  **SecondProject**.

Jeśli tylko jeden z projektów ma ustawić punkt przerwania, a następnie tylko tego procesu będzie można wstrzymać. Inny projekt będzie kontynuował działanie aż do wstrzymane przez dewelopera lub punkt przerwania został dodany.

### <a name="pausing-and-resuming-a-processes"></a>Wstrzymywanie i wznawianie procesów

Istnieje możliwość wstrzymać lub wznowić proces proces prawym przyciskiem myszy i wybierając **wstrzymać** lub **wznowić** z menu kontekstowego:

![](multi-process-debugging-images/mpd08-xs.png "Wstrzymywanie i wznawianie w konsoli wątku")

Wygląd paska narzędzi debugowania zmieni się w zależności od stanu projektów, które są debugowane. Uruchamiając wiele projektów, narzędzi debugowania będą wyświetlane zarówno **Wstrzymaj** i **Wznów** przycisk w przypadku, gdy istnieje co najmniej jeden projekt systemem i wstrzymana jednego projektu:

![](multi-process-debugging-images/mpd07-xs.png  "Debugowanie paska narzędzi")

Kliknięcie przycisku **wstrzymać** przycisk **narzędzi debugowania** wstrzyma wszystkie procesy, które są debugowane, podczas klikania **wznowić** przyciski wznowi wszystkie procesy wstrzymana .

### <a name="debugging-a-second-project"></a>Debugowanie drugi projektu

Istnieje również możliwość debugowania projektu, drugi po rozpoczęciu pierwszego projektu przez program Visual Studio dla komputerów Mac. Po rozpoczęciu pierwszego projektu **kliknij prawym przyciskiem myszy* na projekt w **konsoli rozwiązania** i wybierz **rozpocząć debugowanie elementu**:

![](multi-process-debugging-images/mpd13-xs.png  "Rozpocznij debugowanie elementu")

## <a name="creating-a-solution-configuration"></a>Tworzenie konfiguracji rozwiązania

A _konfiguracji rozwiązania_ informuje Visual Studio dla komputerów Mac, jakie projektu do uruchomienia podczas sesji debugowania jest intiated z **Start** przycisku. Może istnieć więcej niż jedną konfigurację rozwiązania na rozwiązanie. Dzięki temu można określić, jakie projekty są uruchamiane podczas debugowania projektu.

Aby utworzyć nową konfigurację rozwiązania w Xamaring Studio:

1. Otwórz **opcje rozwiązania** okno dialogowe w Visual Studio for Mac, a następnie wybierz **Uruchom > konfiguracje**:

    ![](multi-process-debugging-images/mpd10-xs.png "Konfiguracja rozwiązania w oknie dialogowym Opcje rozwiązania")

2. Polecenie **nowy** przycisku i wprowadź nazwę dla nowej konfiguracji rozwiązania, a następnie kliknij przycisk **Utwórz**. Nowa konfiguracja rozwiązania będą wyświetlane w **konfiguracje** okno:

    ![](multi-process-debugging-images/mpd11-xs.png  "Nazewnictwo nowej konfiguracji rozwiązania")

3. Wybierz z listy konfiguracji nową konfigurację uruchamiania. **Opcje rozwiązania** okna dialogowego spowoduje wyświetlenie każdego projektu w rozwiązaniu. Sprawdź każdego projektu, który ma być uruchamiany po zainicjowaniu sesji debugowania:

    ![](multi-process-debugging-images/mpd12-xs.png "Wybieranie projektu, aby uruchomić")

**MultipleProjects** konfiguracji rozwiązania będzie teraz wyświetlany w **narzędzi debugowania**, co umożliwia deweloperom jednocześnie debugować dwa projekty.

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano debugowanie wielu procesów w programie Visual Studio dla komputerów Mac. Niektóre zmiany do środowiska IDE, aby zapewnić obsługę debugowania wieloprocesowa objęte, a opisano niektóre skojarzone zachowania.

## <a name="related-links"></a>Linki pokrewne

- [Informacje o wersji cyklu Xamarin 9](https://releases.xamarin.com/stable-release-cycle-9/)
