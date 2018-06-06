---
title: pliki .xib w Xamarin.Mac
description: Ten artykuł dotyczy pracy z plikami .xib utworzone w Konstruktorze interfejsu w programie Xcode, można tworzyć i obsługiwać interfejsy użytkownika dla aplikacji Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3ef536ddb19ed60975368bd022e57c34c6f473dc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792277"
---
# <a name="xib-files-in-xamarinmac"></a>pliki .xib w Xamarin.Mac

_Ten artykuł dotyczy pracy z plikami .xib utworzone w Konstruktorze interfejsu w programie Xcode, można tworzyć i obsługiwać interfejsy użytkownika dla aplikacji Xamarin.Mac._

> [!NOTE]
> Jest to preferowany sposób tworzenia interfejsu użytkownika dla aplikacji Xamarin.Mac z scenorys. Ta dokumentacja została pozostawiona w miejscu przyczyn historycznych i pracy z projektów Xamarin.Mac starszej. Aby uzyskać więcej informacji, zobacz nasze [wprowadzenie do scenorysu](~/mac/platform/storyboards/index.md) dokumentacji.

## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, mają dostęp do tego samego elementy interfejsu użytkownika i narzędzi, które Deweloper pracujący *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć w środowisku Xcode _konstruktora interfejsu_ tworzenie i obsługa interfejsów użytkownika (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Plik .xib jest używany przez system macOS do zdefiniowania elementów interfejsu użytkownika aplikacji (np. menu, Windows, widoki, etykiet, pola tekstowe), które są tworzone i obsługiwane graficznie w Konstruktorze interfejsu w środowisku Xcode.

[![Przykład uruchomionej aplikacji](xib-images/intro01.png "przykładem uruchomionej aplikacji")](xib-images/intro01-large.png#lightbox)

W tym artykule omówione zostaną następujące czynności podstawowe informacje dotyczące pracy z plikami .xib w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw go omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` atrybutów umożliwiają połączenie klas C# do obiektów języka Objective C i interfejsu użytkownika elementy.


## <a name="introduction-to-xcode-and-interface-builder"></a>Wprowadzenie do programów Xcode i konstruktora interfejsu

W ramach Xcode Apple utworzył narzędzie Konstruktor interfejsu, który służy do wizualnego tworzenia interfejsu użytkownika w projektancie. Xamarin.Mac fluently integruje się z konstruktora interfejsu, pozwala na tworzenie interfejsu użytkownika z tym samym narzędziami, które wykonują użytkowników języka Objective-C.


### <a name="components-of-xcode"></a>Składniki Xcode

Po otwarciu pliku .xib w środowisku Xcode z programu Visual Studio dla komputerów Mac, zostanie ona otwarta z **Nawigatora projektu** po lewej stronie, **hierarchii interfejsów** i **Edytor interfejsu** w środku , a **właściwości i narzędzia** sekcji po prawej stronie:

[![Składniki interfejsu użytkownika Xcode](xib-images/xcode03.png "składniki interfejsu użytkownika Xcode")](xib-images/xcode03-large.png#lightbox)

Spójrzmy na jakie każdego z tych Xcode sekcje czy i jak użyjesz ich do tworzenia aplikacji Xamarin.Mac interfejsu.


#### <a name="project-navigation"></a>Projekt nawigacji

Po otwarciu pliku .xib do edycji w programie Xcode, Visual Studio for Mac tworzy plik projektu Xcode w tle informacji o zmianach między sobą i Xcode. Później po możesz przełączyć się do programu Visual Studio dla komputerów Mac w programie Xcode, zmiany wprowadzone w tym projekcie są synchronizowane z projektu Xamarin.Mac przez program Visual Studio dla komputerów Mac.

**Nawigacji projektu** sekcja umożliwia nawigowanie między wszystkie pliki wchodzące w skład to _podkładki_ projektu Xcode. Zwykle, tylko będzie interesować na tej liście, takich jak pliki .xib **MainMenu.xib** i **MainWindow.xib**.


#### <a name="interface-hierarchy"></a>Interfejs hierarchii

**Hierarchii interfejsów** sekcji pozwala łatwo uzyskiwać dostęp do wielu właściwości klucza interfejsu użytkownika takich jak jej **symbole zastępcze** i głównym **okna**. Ta sekcja umożliwia również dostęp pojedyncze elementy (widoki), które należy interfejsu użytkownika i Dostosuj sposób, że są zagnieżdżone przeciągając je w hierarchii.


#### <a name="interface-editor"></a>Interfejs edytora

**Edytor interfejsu** sekcja zawiera obszar, w którym można graficznie układu użytkowników Interface. Będzie przeciągnij elementy z **biblioteki** sekcji **właściwości i narzędzia** sekcji, aby utworzyć projekt. Elementy interfejsu użytkownika (widoki) podczas dodawania do powierzchni projektowej, będzie można dodać do **hierarchii interfejsów** sekcji w kolejności, w jakiej występują w **Edytor interfejsu**.


#### <a name="properties--utilities"></a>Właściwości i narzędzia

**Właściwości i narzędzia** sekcja jest podzielona na dwie sekcje główne, które firma Microsoft będzie działać, **właściwości** (nazywanych również inspektorzy) i **biblioteki**:

![Inspektora właściwości](xib-images/xcode04.png "Inspektora właściwości")

Początkowo w tej sekcji jest prawie pusta, jednak w przypadku wybrania elementu w **Edytor interfejsu** lub **hierarchii interfejsów**, **właściwości** sekcji zostaną wypełnione. informacje dotyczące danego elementu i właściwości, które można dostosować.

W ramach **właściwości** sekcji, są różne 8 *karty inspektora*, jak pokazano na poniższej ilustracji:

[![Przegląd wszystkich inspektorzy](xib-images/xcode05.png "przegląd wszystkich inspektorzy")](xib-images/xcode05-large.png#lightbox)

Od lewej do prawej są następujące karty:

-   **Plik inspektora** — Inspector plików zawiera informacje o pliku, takie jak nazwa pliku i lokalizację pliku Xib, który jest edytowany.
-   **Szybką pomoc** — karta szybki pomocy zawiera pomocy kontekstowej oparte na wybranej w środowisku Xcode.
-   **Tożsamość inspektora** — inspektora tożsamości zawiera informacje o wybrany formant/widok.
-   **Atrybuty inspektora** — atrybuty inspektora umożliwia dostosowanie różne atrybuty wybrany formant/widok.
-   **Rozmiar inspektora** — rozmiar inspektora pozwala na kontrolowanie rozmiaru i zmienianie rozmiaru zachowanie kontroli/widok.
-   **Połączenia inspektora** — inspektora połączeń przedstawiono połączenia gniazda i akcji formanty. Zajmiemy się gniazda i akcji w chwilę.
-   **Powiązania inspektora** — inspektora powiązania służy do konfigurowania kontrolek, tak aby ich wartości są automatycznie powiązane z modelami danych.
-   **Wyświetl inspektora efekty** — inspektora efekty widok służy do określenia wpływu na formanty, takie jak animacji.

W **biblioteki** sekcji, można znaleźć kontrolki i obiekty umieszcza graficznie w projektancie do tworzenia interfejsu użytkownika:

![Przykład inspektora biblioteki](xib-images/xcode06.png "przykład inspektora biblioteki")

Teraz, po zapoznaniu się z Xcode IDE i kompilatora interfejsu, Przyjrzyjmy się użyciem jej do utworzenia interfejsu użytkownika.


## <a name="creating-and-maintaining-windows-in-xcode"></a>Tworzenie i obsługę systemu windows w środowisku Xcode

Preferowaną metodą tworzenia interfejsu użytkownika aplikacji Xamarin.Mac jest Scenorys (zobacz nasze [wprowadzenie do scenorysu](~/mac/platform/storyboards/index.md) dokumentacji, aby uzyskać więcej informacji) i w związku z tym wszelkie nowy projekt uruchomiona wykona Xamarin.Mac Scenorys używają domyślnie.

Aby przełączyć się do przy użyciu .xib opartej na interfejsie użytkownika, wykonaj następujące czynności:

1. Otwórz program Visual Studio dla komputerów Mac i Utwórz nowy projekt Xamarin.Mac.
2. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowego pliku...**
3. Wybierz **Mac** > **kontrolera Windows**:

    ![Dodawanie nowego kontrolera okna](xib-images/setup00.png "dodawania nowego kontrolera okna")
4. Wprowadź `MainWindow` dla nazwy i kliknij przycisk **nowy** przycisk:

    ![Dodanie nowego okna głównego](xib-images/setup01.png "dodanie nowego okna głównego")
5. Kliknij prawym przyciskiem myszy projekt ponownie i wybierz **Dodaj** > **nowego pliku...**
6. Wybierz **Mac** > **Menu głównego**:

    ![Dodawanie nowego Menu główne](xib-images/setup02.png "Dodawanie nowego Menu główne")
7. Pozostaw nazwę jako `MainMenu` i kliknij przycisk **nowy** przycisku.
8. W **konsoli rozwiązania** wybierz **Main.storyboard** pliku, kliknij prawym przyciskiem myszy i wybierz **Usuń**:

    ![Wybieranie głównego storyboard](xib-images/setup03.png "wybranie głównego storyboard")
9. W oknie dialogowym usunięcia, kliknij przycisk **usunąć** przycisk:

    ![Potwierdzenie usunięcia](xib-images/setup04.png "potwierdzenie usunięcia")
10. W **konsoli rozwiązania**, kliknij dwukrotnie **Info.plist** plik, aby otworzyć do edycji.
11. Wybierz `MainMenu` z **interfejsu Main** listy rozwijanej:

    [![Ustawienie poziomu menu głównego](xib-images/setup05.png "ustawienie menu głównego")](xib-images/setup05-large.png#lightbox)
12. W **konsoli rozwiązania**, kliknij dwukrotnie **MainMenu.xib** plik, aby otworzyć do edycji w Konstruktorze interfejsu w środowisku Xcode.
13. W **inspektora biblioteki**, typ `object` w polu wyszukiwania przeciągnij nowy **obiektu** na powierzchnię projektu:

    [![Edytowanie menu głównego](xib-images/setup06.png "edycji menu głównego")](xib-images/setup06-large.png#lightbox)
14. W **inspektora tożsamości**, wprowadź `AppDelegate` dla **klasy**:

    [![Wybieranie delegata aplikacji](xib-images/setup07.png "wybranie delegata aplikacji")](xib-images/setup07-large.png#lightbox)
15. Wybierz **właścicielem pliku** z **hierarchii interfejsów**, przełącz się do **inspektora połączenia** i przeciągnij linię z delegata do `AppDelegate` **Obiektu** po prostu dodane do projektu:

    [![Podłączanie delegata aplikacji](xib-images/setup08.png "Podłączanie delegata aplikacji")](xib-images/setup08-large.png#lightbox)
16. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac.

Wszystkie wprowadzone zmiany w miejscu, należy edytować **AppDelegate.cs** pliku i zapewnić ich wyglądać następująco:

```csharp
using AppKit;
using Foundation;

namespace MacXib
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public MainWindowController mainWindowController { get; set; }

        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
            mainWindowController = new MainWindowController ();
            mainWindowController.Window.MakeKeyAndOrderFront (this);
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

Teraz okno główne aplikacji jest zdefiniowany w **.xib** pliku automatycznie dołączony do projektu podczas dodawania kontrolera okna. Aby edytować projektu systemu windows, w **konsoli rozwiązania**, kliknij dwukrotnie **MainWindow.xib** pliku:

![Wybieranie pliku MainWindow.xib](xib-images/edit01.png "zaznaczenie pliku MainWindow.xib")

Spowoduje to otwarcie okna projektu w Konstruktorze interfejsu w środowisku Xcode:

[![Edytowanie MainWindow.xib](xib-images/edit02.png "edycji MainWindow.xib")](xib-images/edit02-large.png#lightbox)


### <a name="standard-window-workflow"></a>Standardowe okno przepływu pracy

Dla każdego okna Tworzenie i pracować z aplikacji Xamarin.Mac proces jest zasadniczo taki sam:

1. Dla nowego systemu windows, które nie są domyślnie automatycznie dodawane do projektu należy dodać nową definicję okna do projektu.
2. Kliknij dwukrotnie plik .xib, aby otworzyć projekt okna do edycji w Konstruktorze interfejsu w środowisku Xcode.
3. Ustaw właściwości wymagane okna **inspektora atrybutu** i **inspektora rozmiar**.
4. Przeciągnij formanty wymagane do tworzenia interfejsu i skonfigurować je w **inspektora atrybutu**.
5. Użyj **inspektora rozmiar** do obsługi zmiana rozmiaru dla elementów interfejsu użytkownika.
6. Udostępnianie okna elementy interfejsu użytkownika do kodu C# za pomocą akcji i gniazda.
7. Zapisz zmiany i wrócić do programu Visual Studio for Mac synchronizację w programie Xcode.


### <a name="designing-a-window-layout"></a>Projektowanie układu okna

Proces rozmieszczania interfejsu użytkownika w Konstruktorze interfejsu jest zasadniczo taki sam dla każdego elementu, który zostanie dodany:

1. Znajdź żądanego formantu w **inspektora biblioteki** i przeciągnij go do **Edytor interfejsu** i umieść go.
2. Ustaw właściwości wymagane okna **inspektora atrybutu**.
3. Użyj **inspektora rozmiar** do obsługi zmiana rozmiaru dla elementów interfejsu użytkownika.
4. Jeśli używasz niestandardowej klasy, ustaw go w **inspektora tożsamości**.
5. Udostępnianie elementów interfejsu użytkownika do kodu C# za pomocą akcji i gniazda.
6. Zapisz zmiany i wrócić do programu Visual Studio for Mac synchronizację w programie Xcode.

Na przykład:

1. W programie Xcode, przeciągnij **przycisk** z **biblioteki**:

    [![Wybranie przycisku z biblioteki](xib-images/xcode07.png "wybierając przycisk z biblioteki")](xib-images/xcode07-large.png#lightbox)
2. Upuść ten przycisk na **okna** w **Edytor interfejsu**:

    [![Dodawanie przycisku do okna](xib-images/xcode08.png "Dodawanie przycisku do okna")](xib-images/xcode08-large.png#lightbox)
3. Polecenie **tytuł** właściwości w **inspektora atrybutu** i Zmień tytuł przycisku do `Click Me`:

    ![Ustawianie atrybutów przycisk](xib-images/xcode09.png "Ustawianie atrybutów przycisku")
4. Przeciągnij **etykiety** z **biblioteki**:

    [![Wybranie etykiety w bibliotece](xib-images/xcode10.png "wybranie etykiety w bibliotece")](xib-images/xcode10-large.png#lightbox)
5. Upuść etykiety na **okna** obok przycisku w **Edytor interfejsu**:

    [![Dodawanie etykiet do okna](xib-images/xcode11.png "Dodawanie etykiet do okna")](xib-images/xcode11-large.png#lightbox)
6. Wystarczy pobrać uchwytu na etykiecie po prawej stronie, a następnie przeciągnij go do czasu jego pobliżu krawędzi okna:

    [![Zmiana rozmiaru etykiety](xib-images/xcode12.png "zmiana rozmiaru etykiety")](xib-images/xcode12-large.png#lightbox)
7. Oznaczone etykietą nadal wybrany w **Edytor interfejsu**, przełącz się do **inspektora rozmiar**:

    ![Zaznaczając rozmiar inspektora](xib-images/xcode13.png "zaznaczając rozmiar Inspektora")
8. W **automatyczna zmiana rozmiaru pola** kliknij **Dim nawiasu czerwony** po prawej stronie i **Dim czerwona strzałka pozioma** w Centrum:

    ![Edytowanie właściwości automatyczna zmiana rozmiaru](xib-images/xcode14.png "edytowania właściwości automatyczna zmiana rozmiaru")
9. Dzięki temu, że etykieta zostanie rozciągają się, aby zwiększyć lub zmniejszyć, ponieważ zmieniono rozmiar okna w uruchomionej aplikacji. **Nawiasy czerwony** oraz górnej i lewej strony **automatyczna zmiana rozmiaru pola** pole Poinformuj etykiety, aby zostać zatrzymane do danego X i Y lokalizacji.
10. Zapisz zmiany w interfejsie użytkownika

Jak zostały Zmienianie rozmiaru i przenoszenia kontrolek wokół, powinien zauważenia czy konstruktora interfejsu zapewnia wskazówki przydatne przystawki, oparte na [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Niniejsze wytyczne pomoże Ci tworzenie wysokiej jakości aplikacji, które będą miały znanych wyglądu i działania dla użytkowników komputerów Mac.

W **hierarchii interfejsów** sekcji, zwróć uwagę, jak pokazano układ i hierarchię elementów wchodzące w skład naszych interfejsu użytkownika:

![Zaznaczenie elementu w hierarchii interfejsów](xib-images/xcode15.png "zaznaczenie elementu w hierarchii interfejsów")

W tym miejscu można wybrać elementy do edycji lub przeciągnij, aby zmienić kolejność elementów interfejsu użytkownika, jeśli to konieczne. Na przykład jeśli element interfejsu użytkownika są objęte przez inny element, można go przeciągnij w dół na liście, aby stał się najwyżej elementu w oknie.

Aby uzyskać więcej informacji na temat pracy z systemem Windows w aplikacji Xamarin.Mac, zobacz nasze [Windows](~/mac/user-interface/window.md) dokumentacji.


## <a name="exposing-ui-elements-to-c-code"></a>Udostępnianie elementów interfejsu użytkownika do kodu C#

Po zakończeniu rozmieszczania wyglądu i działania interfejsu użytkownika w Konstruktorze interfejsu należy uwidaczniać elementy interfejsu użytkownika, dzięki czemu są one dostępne z kodu C#. Aby to zrobić, należy używać działań i gniazda.


### <a name="setting-a-custom-main-window-controller"></a>Ustawianie kontrolera niestandardowego okna głównego

Aby można było utworzyć gniazda i akcji, aby ujawnić elementy interfejsu użytkownika do kodu C#, aplikacji Xamarin.Mac należy być przy użyciu kontrolera niestandardowego okna.

Wykonaj następujące czynności:

1. Otwórz scenorysu aplikacji w Konstruktorze interfejsu w środowisku Xcode.
2. Wybierz `NSWindowController` na powierzchni projektu.
3. Przełącz się do **inspektora tożsamości** Wyświetl i wprowadź `WindowController` jako **Nazwa klasy**:

    [![Edytowanie nazwy klasy](xib-images/windowcontroller01.png "edycji Nazwa klasy")](xib-images/windowcontroller01-large.png#lightbox)
4. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji.
5. A **WindowController.cs** plik zostanie dodany do projektu w **konsoli rozwiązania** w programie Visual Studio dla komputerów Mac:

    ![Nazwa nowej klasy w Visual Studio dla komputerów Mac](xib-images/windowcontroller02.png "nową nazwę klasy w Visual Studio dla komputerów Mac")
6. Otwórz ponownie scenorysu w Konstruktorze interfejsu w środowisku Xcode.
7. **WindowController.h** pliku będą dostępne do użycia:

    [![Odpowiedniego pliku .h w środowisku Xcode](xib-images/windowcontroller03.png "odpowiedniego pliku .h w środowisku Xcode")](xib-images/windowcontroller03-large.png#lightbox)


### <a name="outlets-and-actions"></a>Gniazda i akcji

Dlatego co to są gniazda i akcje? W programowaniu tradycyjnych interfejs użytkownika platformy .NET, formantu w interfejsie użytkownika jest automatycznie widoczne jako właściwość, gdy jest ona dodawana. Elementy działają inaczej w Mac, po prostu Dodawanie formantu do widoku nie ona łatwiej dostępna dla kodu. Deweloper musi ujawniać jawnie elementu interfejsu użytkownika do kodu. W kolejności to zrobić, Apple daje dwie opcje:

-  **Gniazda** — gniazda są podobne do właściwości. Jeśli okablować kontrolą gniazda jest uwidaczniany w kodzie za pomocą właściwości, więc można wykonywać czynności takie, jak dołączyć procedury obsługi zdarzeń, wywoływać metod w go, np.
-  **Akcje** — akcje są analogiczne do polecenia wzorzec na platformie WPF. Na przykład jeśli akcja jest wykonywana na formancie, powiedz kliknij przycisk, formantu zostanie automatycznie wywołania metody w kodzie. Akcje są wydajne i wygodne, ponieważ można okablować się wiele formantów do tego samego działania.

W środowisku Xcode gniazda i działania są dodawane bezpośrednio w kodzie za pomocą *przeciąganie kontroli*. W szczególności, oznacza to, aby utworzyć gniazda lub akcji, wybranie który chcesz dodać gniazda lub akcji, przytrzymaj naciśnięty element kontroli **kontroli** znajdującego się na klawiaturze, a następnie przeciągnij formant bezpośrednio w kodzie.

Dla deweloperów Xamarin.Mac oznacza to, przeciągnij w pliki szczątkowe Objective-C, które odpowiadają w pliku C# której chcesz utworzyć gniazda lub akcji. Visual Studio for Mac utworzony plik o nazwie **MainWindow.h** jako część podkładki projektu Xcode wygenerowane za pomocą konstruktora interfejsu:

[![Przykładowy plik .h w środowisku Xcode](xib-images/xcode16.png "przykładowy plik .h w środowisku Xcode")](xib-images/xcode16-large.png#lightbox)

Ten plik .h stub odzwierciedla **MainWindow.designer.cs** jest automatycznie dodawany do projektu Xamarin.Mac podczas tworzenia nowego `NSWindow` jest tworzony. Ten plik będzie używane do synchronizowania zmiany wprowadzone przez konstruktora interfejsu i jest, gdzie utworzymy akcji i gniazda, aby elementy interfejsu użytkownika są widoczne dla kodu C#.


#### <a name="adding-an-outlet"></a>Dodawanie gniazda

Opis podstawowych co to są gniazda i akcji, Przyjrzyjmy się tworzenie gniazda do udostępnienia elementu interfejsu użytkownika do kodu C#.

Wykonaj następujące czynności:

1. W środowisku Xcode w prawej top górny róg ekranu, kliknij przycisk **dwukrotnie okrąg** przycisk, aby otworzyć **Edytor Asystenta**:

    [![Wybieranie edytora Asystenta](xib-images/outlet01.png "wybranie Edytor Asystenta")](xib-images/outlet01-large.png#lightbox)
2. Xcode nastąpi przełączenie do trybu widok podzielony z **Edytor interfejsu** po jednej stronie i **edytora kodu** z drugiej strony.
3. Należy zauważyć, że środowisko Xcode jest pobierany automatycznie **MainWindowController.m** w pliku **edytora kodu**, która jest nieprawidłowa. Czy pamiętasz z naszych omówione gniazda i akcje są powyżej, trzeba mieć **MainWindow.h** wybrane.
4. W górnej części **edytora kodu** kliknij **łącze automatyczne** i wybierz **MainWindow.h** pliku:

    [![Wybieranie pliku .h poprawne](xib-images/outlet02.png "Wybieranie pliku prawidłowe .h")](xib-images/outlet02-large.png#lightbox)
5. Xcode teraz powinny mieć poprawny plik wybrane:

    [![Wybrany plik poprawne](xib-images/outlet03.png "właściwy plik wybrane")](xib-images/outlet03-large.png#lightbox)
6. **Ostatnim krokiem było bardzo ważne!** Jeśli nie masz poprawnego pliku wybrane, nie można utworzyć gniazda i akcje lub ich będzie ona widoczna dla niewłaściwego klasy w języku C#!
7. W **Edytor interfejsu**, naciśnij i przytrzymaj **kontroli** klucza na klawiaturze i kliknij i przeciągnij etykiety, które zostały utworzone powyżej do edytora kodu właśnie poniżej `@interface MainWindow : NSWindow { }` kodu:

    [![Przeciąganie, aby utworzyć nowe gniazda](xib-images/outlet04.png "przeciąganie, aby utworzyć nowe gniazda")](xib-images/outlet04-large.png#lightbox)
8. Zostanie wyświetlone okno dialogowe. Pozostaw **połączenia** ustawioną gniazda, a następnie wprowadź `ClickedLabel` dla **nazwa**:

    [![Ustawianie właściwości gniazda](xib-images/outlet05.png "ustawienie właściwości gniazda")](xib-images/outlet05-large.png#lightbox)
9. Kliknij przycisk **Connect** przycisk, aby utworzyć gniazda:

    ![Ukończono gniazda](xib-images/outlet06.png "ujścia ukończone")
10. Zapisz zmiany w pliku.


#### <a name="adding-an-action"></a>Dodawanie operacji

Następnie Przyjrzyjmy się akcji do udostępnienia interakcji z użytkownikiem z elementu interfejsu użytkownika do kodu C#.

Wykonaj następujące czynności:

1. Upewnij się, że jesteśmy w **Edytor Asystenta** i **MainWindow.h** plik jest widoczny w **edytora kodu**.
2. W **Edytor interfejsu**, naciśnij i przytrzymaj **kontroli** klucza na klawiaturze i kliknij i przeciągnij przycisku, które zostały utworzone powyżej do edytora kodu właśnie poniżej `@property (assign) IBOutlet NSTextField *ClickedLabel;` kodu:

    [![Przeciąganie, aby utworzyć akcję](xib-images/action01.png "przeciąganie, aby utworzyć akcję")](xib-images/action01-large.png#lightbox)
3. Zmień **połączenia** typu akcji:

    [![Wybierz typ akcji](xib-images/action02.png "wybierz typ akcji")](xib-images/action02-large.png#lightbox)
4. Wprowadź `ClickedButton` jako **nazwa**:

    [![Konfigurowanie akcji](xib-images/action03.png "Konfigurowanie akcji")](xib-images/action03-large.png#lightbox)
5. Kliknij przycisk **Connect** przycisk, aby utworzyć akcję:

    ![Ukończono akcji](xib-images/action04.png "ukończonych akcji")
6. Zapisz zmiany w pliku.

Z interfejsu użytkownika przewodowej w pionie i ujawniony dla kodu C# należy przełączyć się do programu Visual Studio dla komputerów Mac i pozwól mu przeprowadzić synchronizację zmian z Xcode i kompilatora interfejsu.


### <a name="writing-the-code"></a>Pisanie kodu

Utworzyć interfejsu użytkownika i jego elementów interfejsu użytkownika do kodu za pomocą akcji i gniazda można przystąpić do pisania kodu do programu do życia. Na przykład otwórz **MainWindow.cs** plik do edycji przez dwukrotne kliknięcie w **konsoli rozwiązania**:

[![Plik MainWindow.cs](xib-images/code01.png "MainWindow.cs pliku")](xib-images/code01-large.png#lightbox)

I Dodaj następujący kod, aby `MainWindow` klasy do pracy z gniazda próbki utworzoną wcześniej:

```csharp
private int numberOfTimesClicked = 0;
...

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Należy pamiętać, że `NSLabel` jest dostępny w języku C# o nazwie bezpośredniego czy przypisano go w środowisku Xcode podczas tworzenia ujściu w środowisku Xcode, w tym przypadku jest to `ClickedLabel`. Są dostępne wszystkie metody lub właściwości obiektu narażonych taki sam sposób jak normalne klas C#.

> [!IMPORTANT]
> Należy użyć `AwakeFromNib`, zamiast innej metody, takie jak `Initialize`, ponieważ `AwakeFromNib` jest nazywany _po_ system operacyjny został załadowany i utworzyć wystąpienia interfejsu użytkownika z pliku .xib. Jeśli próbujesz dostępu formantu etykiety przed pliku .xib do pełni załadowaniu i wystąpienia, jak `NullReferenceException` błąd ponieważ formantu etykiety nie zostałyby jeszcze utworzone.

Następnie dodaj następujące klasy częściowej do `MainWindow` klasy:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Ten kod dołącza do akcji, który został utworzony w programie Xcode i kompilatora interfejsu i zostanie wywołana w dowolnym momencie użytkownik kliknie przycisk.

Niektóre elementy interfejsu użytkownika automatycznie utworzone w akcji, na przykład na pasku Menu domyślne elementów takich jak **Otwórz...**  elementu menu (`openDocument:`). W **konsoli rozwiązania**, kliknij dwukrotnie **AppDelegate.cs** plik, aby otworzyć do edycji i Dodaj następujący kod poniżej `DidFinishLaunching` metody:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

W tym miejscu wiersz klucza jest `[Export ("openDocument:")]`, informuje o tym `NSMenu` który **AppDelegate** ma metodę `void OpenDialog (NSObject sender)` który odpowiada na `openDocument:` akcji.

Aby uzyskać więcej informacji na temat pracy z menu, zobacz nasze [menu](~/mac/user-interface/menu.md) dokumentacji.


## <a name="synchronizing-changes-with-xcode"></a>Synchronizowanie zmian z Xcode

Po przełączeniu do programu Visual Studio dla komputerów Mac w programie Xcode, wszystkie zmiany wprowadzone w programie Xcode automatycznie zostaną zsynchronizowane z projektu Xamarin.Mac.

W przypadku wybrania **MainWindow.designer.cs** w **konsoli rozwiązania** będzie można zobaczyć, jak naszym gniazda i akcji ma zostały przewodowej się w naszym kodzie C#:

[![Synchronizowanie zmian z Xcode](xib-images/sync01.png "synchronizowanie zmian z Xcode")](xib-images/sync01-large.png#lightbox)

Powiadomienie jak dwie definicje w **MainWindow.designer.cs** pliku:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Wiersz w górę z definicjami w **MainWindow.h** plików w środowisku Xcode:

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Jak widać, wykrywa zmiany w pliku .h programu Visual Studio for Mac i automatycznie synchronizuje te zmiany w odpowiednich **. Designer.cs narzędzie** pliku do udostępnienia ich do aplikacji. Należy również zauważyć, że **MainWindow.designer.cs** jest częściowej klasy, dzięki czemu nie ma programu Visual Studio for Mac, można zmodyfikować **MainWindow.cs** który zastąpiłaby wszelkie zmiany, które zostały wprowadzone do klasy.

Zwykle nie należy otworzyć **MainWindow.designer.cs** samodzielnie, jego został przedstawiony tutaj wyłącznie w celach edukacyjnych.

> [!IMPORTANT]
> W większości przypadków programu Visual Studio for Mac automatycznie Zobacz wszystkie zmiany wprowadzone w programie Xcode i zsynchronizować je do projektu Xamarin.Mac. W wystąpieniu wyłączenia synchronizacji nie jest realizowane automatycznie przejdź do programów Xcode i je do programu Visual Studio dla komputerów Mac ponownie. Zwykle będzie to rozpocząć poza cyklu synchronizacji.


## <a name="adding-a-new-window-to-a-project"></a>Dodawanie nowego okna do projektu

Jako uzupełnienie okna głównego dokumentu aplikacji Xamarin.Mac może być konieczne do wyświetlania innych typów systemu windows użytkownika, takie jak preferencje lub Inspektora paneli. Podczas dodawania nowego okna do projektu należy zawsze używać **Cocoa okna z kontrolera** opcja, ponieważ dzięki temu proces ładowania okna z .xib pliku łatwiejsze.

Aby dodać nowe okno, wykonaj następujące czynności:

1. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy plik.** .
2. W oknie dialogowym Nowy plik, wybierz **Xamarin.Mac** > **Cocoa okna z kontrolera**:

    ![Dodawanie nowego kontrolera okna](xib-images/new01.png "dodawania nowego kontrolera okna")
3. Wprowadź `PreferencesWindow` dla **nazwa** i kliknij przycisk **nowy** przycisku.
4. Kliknij dwukrotnie **PreferencesWindow.xib** plik, aby otworzyć do edycji w Konstruktorze interfejsu:

    [![Edytowanie okna w środowisku Xcode](xib-images/new02.png "edycji okna w środowisku Xcode")](xib-images/new02-large.png#lightbox)
5. Projekt interfejsu:

    [![Projektowanie układu windows](xib-images/new03.png "projektowanie układu systemu windows")](xib-images/new03-large.png#lightbox)
6. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Dodaj następujący kod do **AppDelegate.cs** do wyświetlenia nowego okna:

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

`var preferences = new PreferencesWindowController ();` Wiersz tworzy nowe wystąpienie klasy kontrolera okna, który ładuje okna z pliku .xib i nadyma go. `preferences.Window.MakeKeyAndOrderFront (this);` Wiersz zawiera nowe okno do użytkownika.

Jeżeli możesz uruchomić kod i wybierz **Preferencje...**  z **Menu aplikacja**, wyświetli się okno:

![Przykładowa aplikacja uruchomiona](xib-images/new04.png "systemem przykładowej aplikacji")

Aby uzyskać więcej informacji na temat pracy z systemem Windows w aplikacji Xamarin.Mac, zobacz nasze [Windows](~/mac/user-interface/window.md) dokumentacji.


## <a name="adding-a-new-view-to-a-project"></a>Dodawanie nowego widoku do projektu

Istnieją sytuacje, gdy będzie łatwiej podzielić projekt z okna na kilka plików .xib łatwiejsze w zarządzaniu. Na przykład, takich jak przełączania z zawartość okna głównego podczas wybierania elementem paska narzędzi w **okna Preferencje** albo wymienić zawartości w odpowiedzi na **listy źródeł** zaznaczenia.

Podczas dodawania nowego widoku do projektu należy zawsze używać **Cocoa widoku z kontrolerem** opcja, ponieważ dzięki temu proces ładowania widoku z .xib pliku łatwiejsze.

Aby dodać nowy widok, wykonaj następujące czynności:

1. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy plik.** .
2. W oknie dialogowym Nowy plik, wybierz **Xamarin.Mac** > **Cocoa widoku z kontrolerem**:

    ![Dodawanie nowego widoku](xib-images/view01.png "dodanie nowego widoku")
3. Wprowadź `SubviewTable` dla **nazwa** i kliknij przycisk **nowy** przycisku.
4. Kliknij dwukrotnie **SubviewTable.xib** plik, aby otworzyć do edycji w Konstruktorze interfejsu i projektowanie interfejsu użytkownika:

    [![Projektowanie nowego widoku w programie Xcode](xib-images/view02.png "projektowania nowego widoku w środowisku Xcode")](xib-images/view02-large.png#lightbox)
5. Okablować wszystkie wymagane akcje i gniazda.
6. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Następnie Edytuj **SubviewTable.cs** i Dodaj następujący kod, aby **AwakeFromNib** pliku do wypełnienia nowego widoku po załadowaniu go:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));
    DataSource.Sort ("Title", true);

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);

    // Auto select the first row
    ProductTable.SelectRow (0, false);
}
```

Dodaj typ wyliczeniowy do projektu, aby śledzić widoku jest obecnie wyświetlana. Na przykład **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

Edytuj plik .xib zostanie korzystanie z widoku i wyświetlanie okna. Dodaj **widok niestandardowy** pełniący funkcję kontenera widoku po załadowaniu do pamięci przez kod w języku C# i Ujawnij do gniazda mu `ViewContainer`:

[![Tworzenie wymagane gniazda](xib-images/view03.png "tworzenie wymagane gniazda")](xib-images/view03-large.png#lightbox)

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Następnie przeprowadź edycję pliku .cs okna, w którym będzie wyświetlany nowy widok (na przykład **MainWindow.cs**) i Dodaj następujący kod:

```csharp
private SubviewType ViewType = SubviewType.None;
private NSViewController SubviewController = null;
private NSView Subview = null;
...

private void DisplaySubview(NSViewController controller, SubviewType type) {

    // Is this view already displayed?
    if (ViewType == type) return;

    // Is there a view already being displayed?
    if (Subview != null) {
        // Yes, remove it from the view
        Subview.RemoveFromSuperview ();

        // Release memory
        Subview = null;
        SubviewController = null;
    }

    // Save values
    ViewType = type;
    SubviewController = controller;
    Subview = controller.View;

    // Define frame and display
    Subview.Frame = new CGRect (0, 0, ViewContainer.Frame.Width, ViewContainer.Frame.Height);
    ViewContainer.AddSubview (Subview);
}
```

Kiedy należy wyświetlić nowy widok załadowanego z pliku .xib w kontenerze okna ( **widok niestandardowy** dodanych wcześniej), ta dojścia kodu usunięcie dowolnego istniejącego widoku i zamienienie go do nowego. Wygląda, aby zobaczyć, masz już widok wyświetlany, jeśli więc spowoduje usunięcie go z ekranu. Następnie zajmuje widok, który został przekazany w (jak załadowany z View Controller) zmieniany mieści się w obszarze zawartości i dodaje go do zawartości do wyświetlenia.

Aby wyświetlić nowy widok, należy użyć poniższego kodu:

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

Tworzy nowe wystąpienie klasy kontrolera widoku nowego widoku do wyświetlenia, spowoduje to jego typ (określoną przez wyliczenia dodane do projektu) i korzysta z `DisplaySubview` metody dodany do klasy okna do faktycznie wyświetlania widoku. Na przykład:

[![Przykładowa aplikacja uruchomiona](xib-images/view04.png "systemem przykładowej aplikacji")](xib-images/view04-large.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z systemem Windows w aplikacji Xamarin.Mac, zobacz nasze [Windows](~/mac/user-interface/window.md) i [okna](~/mac/user-interface/dialog.md) dokumentacji.


## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się praca z plikami .xib w aplikacji Xamarin.Mac. Widzieliśmy różne typy i używa plików .xib, aby utworzyć interfejs użytkownika, w jaki sposób można tworzyć i obsługiwać .xib plików w środowisku Xcode interfejsu konstruktora oraz sposób pracy z plikami .xib w kodzie języka C#.


## <a name="related-links"></a>Linki pokrewne

- [MacImages (przykład)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Menu](~/mac/user-interface/menu.md)
- [Okna dialogowe](~/mac/user-interface/dialog.md)
- [Praca z obrazami](~/mac/app-fundamentals/image.md)
- [System macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
