---
title: Windows na platformie Xamarin.Mac
description: Ten artykuł dotyczy pracy z systemem windows i paneli w aplikacji platformy Xamarin.Mac. Opisuje tworzenie systemu windows i paneli w środowisku Xcode i programu Interface Builder, ładowania ich ze scenorysami oraz .pliki XIb i pracy z nimi programistycznie.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: b60b8a6a7c56347d6abf71f8c5149ddd556d3da8
ms.sourcegitcommit: ee66db647ae9d94b54b1c5d9093075a620d0c6b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43116373"
---
# <a name="windows-in-xamarinmac"></a>Windows na platformie Xamarin.Mac

_Ten artykuł dotyczy pracy z systemem windows i paneli w aplikacji platformy Xamarin.Mac. Opisuje tworzenie systemu windows i paneli w środowisku Xcode i programu Interface Builder, ładowania ich ze scenorysami oraz .pliki XIb i pracy z nimi programistycznie._

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, mają dostęp do tej samej Windows i panele, który deweloper pracy w *języka Objective-C* i *Xcode* jest. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, można użyć program Xcode _programu Interface Builder_ do tworzenia i obsługi Windows i panele (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Oparte na jego przeznaczenie, aplikacji rozszerzenia Xamarin.Mac cechuje się co najmniej Windows na ekranie do zarządzania i koordynacji informacje wyświetlane, a w programach. Podstawowe funkcje okna są:

1. Aby zapewnić obszar, w którym widoków i kontrolek mogą być wprowadzane i zarządzane.
2. Aby zaakceptować i reagowania na zdarzenia w odpowiedzi na interakcję użytkownika z klawiaturą i myszą.

Windows może być używany w stanie niemodalne (takich jak edytor tekstu, który może mieć wiele dokumentów, Otwórz na raz) lub modalne (np. Eksport okno dialogowe, które muszą być ukryty przed kontynuowaniem aplikacji).

Panele to specjalny rodzaj okna (podklasę elementu bazowego `NSWindow` klasy), które zwykle służą pomocnicze w ramach funkcji w aplikacji, takich jak okna narzędzia, takie jak inspektorzy format tekstu i selektora kolorów systemu.

[![](window-images/intro01.png "Okno w środowisku Xcode edycji")](window-images/intro01.png#lightbox)

W tym artykule omówiono podstawowe informacje dotyczące pracy z Windows i paneli w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` poleceń Umożliwia o komunikacji sieciowej w górę klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Wprowadzenie do Windows

Jak wspomniano powyżej, okno to obszar, w którym widoków i kontrolek mogą być wprowadzane i zarządzane i reaguje na zdarzenia w oparciu o interakcji z użytkownikiem (za pośrednictwem klawiatury lub myszy).

Zgodnie z firmy Apple istnieje pięć typów głównego systemu Windows w systemie macOS aplikacji:

- **Okno dokumentu** — okno dokumentu zawiera dane użytkownika opartych na plikach, takie jak arkusz kalkulacyjny lub dokument tekstowy.
- **Okna aplikacji** -okna aplikacji jest głównego okna aplikacji, która nie jest na podstawie dokumentu (na przykład aplikacja kalendarza na komputerze Mac).
- **Panel** — panel liczby zmiennoprzecinkowe powyżej innych oknach oraz udostępnia narzędzia, lub otworzyć formantów, które użytkownicy mogą pracować z, gdy są dokumenty. W niektórych przypadkach może być półprzezroczyste panelu (takie jak podczas pracy z grafikami duże).
- **Okno dialogowe** — okno dialogowe pojawia się w odpowiedzi na akcję użytkownika i zazwyczaj zawiera sposoby użytkowników można ukończyć akcji. Okno dialogowe wymaga odpowiedź od użytkownika może zostać zamknięty. (Zobacz [Praca z okien dialogowych](~/mac/user-interface/dialog.md))
- **Alerty** — alert jest specjalnym typem okna dialogowego wyświetlanego, gdy występuje poważny problem, (na przykład błąd) lub jako ostrzeżenie (takich jak przygotowywanie do usunięcia pliku). Ponieważ alert jest okno dialogowe, również wymaga odpowiedź użytkownika przed jego zamknięciem. (Zobacz [Praca z alertami](~/mac/user-interface/alert.md))

Aby uzyskać więcej informacji, zobacz [o Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>Main, klucz i nieaktywne Windows

Windows w aplikacji platformy Xamarin.Mac można wyglądają i zachowują się inaczej w zależności od sposobu użytkownik jest aktualnie interakcji z nimi. Przedniej dokumentu lub okna aplikacji, która jest aktualnie fokus uwagi użytkownika jest nazywany _okno główne_. W większości przypadków okno to będzie również _okna klucz_ (okno, które są aktualnie akceptowane dane wejściowe użytkownika). Ale nie jest to wymagane, na przykład selektora kolorów może być otwarte i się w oknie klucz, w którym użytkownik prowadzi interakcję z, aby zmienić stan elementu w oknie dokumentu, (które nadal będzie okno główne).

Główne i klucz Windows (jeśli są one niezależne) zawsze są aktywne, _nieaktywne Windows_ są otwarte okna, które nie są na pierwszym planie. Na przykład aplikacja edytora tekstu może mieć więcej niż jeden dokument otwarty naraz tylko okno główne będzie aktywny, wszystkie inne będą nieaktywne. 

Aby uzyskać więcej informacji, zobacz [o Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Windows nazewnictwa

Można wyświetlić w oknie paska tytułu i tytuł jest wyświetlany, zazwyczaj jest to nazwa aplikacji, nazwy dokumentu, w której są wykonywane prace lub funkcję okna (np. inspektor). Niektóre aplikacje nie wyświetlaj pasek tytułu, ponieważ są rozpoznawalne przez kontakt, a nie Praca z dokumentami.

Apple zasugerować następujące wytyczne:

- Użyj nazwy aplikacji tytułu okna głównego, niebędącego dokumentem. 
- Nadaj nazwę nowe okno dokumentu `untitled`. Pierwszy nowego dokumentu nie dołączenia liczby do tytułu (takie jak `untitled 1`). Jeśli użytkownik tworzy inny nowy dokument przed zapisaniem i warianty pierwszy, wywołanie tego okna `untitled 2`, `untitled 3`itp.

Aby uzyskać więcej informacji, zobacz [nazewnictwa Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>Windows pełnego ekranu

W systemie macOS, okno aplikacji można przejść ukrywanie pełny ekran wszystko, co łącznie z paska Menu aplikacji (co może być ujawniony, przenosząc kursor do górnej części ekranu) zapewnienie naruszania zawartości jest bezpłatna interakcji z nią.

Apple sugeruje następujące wytyczne:

- Ustal, czy sens dla okno do pełnego ekranu. Aplikacje, które zapewniają krótki interakcji (na przykład Kalkulator) nie należy podać tryb pełnoekranowy.
- Pokaż pasek narzędzi, jeśli zadanie pełnego ekranu go wymaga. Zazwyczaj pasek narzędzi jest ukryty w trybie pełnoekranowym.
- Pełnoekranowe okno mają wszyscy użytkownicy funkcje potrzebne do ukończenia zadania.
- Jeśli to możliwe należy unikać interakcji wyszukiwania, gdy użytkownik jest w trybie pełnego ekranu.
- Z zalet zwiększonej ekranowego bez przesuwania fokus kursora głównym zadaniem.

Aby uzyskać więcej informacji, zobacz [Windows pełnoekranowym](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>Panele

Panel jest okno pomocnicze, który zawiera formanty i opcje, które mają wpływ na aktywnego dokumentu lub wybór (na przykład system selektor kolorów):

[![](window-images/panel01.png "Panel kolorów")](window-images/panel01.png#lightbox)

Panele mogą być albo _specyficzny dla aplikacji_ lub _ogólnosystemowe_. Panele specyficzny dla aplikacji float w górnej części okna dokumentu aplikacji i zniknąć, gdy aplikacja znajduje się w tle. Panele ogólnosystemowe (takie jak **czcionki** panelu), float, na podstawie wszystkie otwarte okna, niezależnie od aplikacji. 

Apple sugeruje następujące wytyczne:

- Ogólnie rzecz biorąc, za pomocą panelu standardowe, panele przezroczyste powinien być używany tylko rzadko i zadania intensywnie korzystające z grafiki.
- Należy rozważyć użycie panelu, aby zapewnić użytkownikom łatwy dostęp do ważnych formantów lub informacje, które bezpośrednio wpływa na ich zadań.
- Ukryj i Pokaż panele zgodnie z potrzebami.
- Panele zawsze powinna zawierać paska tytułu.
- Panele nie powinien zawierać przycisk Minimalizuj active.

#### <a name="inspectors"></a>Inspektorzy

Większość nowoczesnych aplikacji systemu macOS przedstawia pomocniczego kontrolki i opcje, które mają wpływ na aktywnego dokumentu lub zaznaczenia jako _inspektorzy_ będących częścią okno główne (takich jak **stron** aplikacji poniżej), Zamiast korzystać z panelu Windows:

[![](window-images/panel02.png "Inspektor przykład")](window-images/panel02.png#lightbox)

Aby uzyskać więcej informacji, zobacz [panele](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) i naszej [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) Przykładowa aplikacja dla pełnego wdrożenia **Inspektor interfejsu** w aplikacji platformy Xamarin.Mac.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>Utworzenie i utrzymywanie Windows w środowisku Xcode

Kiedy tworzysz nową aplikację cocoa dla platformy Xamarin.Mac, otrzymasz standardowego okna puste, domyślnie. Ten system windows jest zdefiniowany w `.storyboard` pliku automatycznie uwzględnione w projekcie. Aby edytować projektu systemu windows w **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku:

[![](window-images/edit01.png "Wybieranie głównego storyboard")](window-images/edit01.png#lightbox)

Spowoduje to otwarcie projektu okna w program Xcode Interface Builder:

[![](window-images/edit02.png "Edytowanie interfejsu użytkownika w środowisku Xcode")](window-images/edit02.png#lightbox)

W **Inspektor atrybutu**, istnieje kilka właściwości, które służą do definiowania i kontrolowania okna:

- **Tytuł** — jest to tekst, który będzie wyświetlany w pasek tytułu okna.
- **Automatyczne zapisywanie** — jest to _klucz_ posłuży IDENTYFIKATORA okna, gdy jego pozycji ustawienia i są automatycznie zapisywane.
- **Pasek tytułu** — jest wyświetlane okno paska tytułu.
- **Ujednolicone tytuł i narzędzi** — Jeśli okno zawiera pasek narzędzi, powinna być częścią paska tytułu.
- **Pełny widok zawartości o rozmiarze** — umożliwia obszar zawartości okna poniżej paska tytułu.
- **W tle** — okno ma cień.
- **Tekstury** -teksturą systemu windows można użyć efekty (na przykład popularność) i można przenosić, przeciągając jego treści.
- **Zamknij** — okno ma przycisk Zamknij.
- **Minimalizuj** — okno ma przycisk minimalizacji.
- **Zmień rozmiar** — okno ma zmieniania rozmiaru formantu.
- **Przycisk paska narzędzi** — okno ma przycisk paska narzędzi Pokaż/Ukryj.
- **Umożliwiająca przywrócenie** -położenie okna i ustawienia automatycznie zapisywany i przywracany.
- **Widoczne na uruchamianie** — okno automatycznie przedstawiono kiedy `.xib` ładowania pliku.
- **Ukryj na dezaktywować** — okno kryje po przejściu tło w aplikacji.
- **Zwolnij po zamknięciu** -okna usuwane z pamięci, gdy jest ono zamknięte.
- **Zawsze wyświetlanie etykietek narzędzi** — są stale wyświetlany etykietki narzędzi.
- **Ponownie oblicza pętli widoku** -polega na kolejności Wyświetl ponownie obliczone przed narysowaniem okna.
- **Miejsca do magazynowania**, **Exposé** i **funkcja włączania i wyłączania** -decydować, jak okna zachowuje się w tych środowiskach systemu macOS. 
- **Pełny ekran** — Określa, w tym oknie można wprowadzić w trybie pełnoekranowym. 
- **Animacja** — Określa typ animacji dostępne dla okna.
- **Wygląd** -kontrolują wygląd okna. Obecnie istnieje tylko jeden wygląd, akwamaryna.

Zobacz firmy Apple [wprowadzenie do Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) i [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) dokumentacji, aby uzyskać więcej informacji.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>Ustawienie domyślnym rozmiarem i lokalizacją

Aby ustawić położenie początkowe elementu okna i kontrolować jego rozmiar, przełącz się do **Inspektor rozmiar**:

[![](window-images/edit07.png "Domyślny rozmiar i położenie")](window-images/edit07.png#lightbox)

W tym miejscu można ustawić początkowy rozmiar okna, nadaj mu minimalny i maksymalny rozmiar, Ustaw początkową lokalizację na ekranie i kontrolować obramowania okna.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>Ustawienia kontrolera niestandardowe okno główne

Aby można było utworzyć gniazd i akcje, aby uwidocznić elementy interfejsu użytkownika do kodu w języku C#, musisz używać kontroler okna niestandardowe aplikacji rozszerzenia Xamarin.Mac.

Wykonaj następujące czynności:

1. Otwórz Scenorys aplikacji w program Xcode Interface Builder.
2. Wybierz `NSWindowController` powierzchni projektowej.
3. Przełącz się do **Inspektor tożsamości** wyświetlić, a następnie wprowadź `WindowController` jako **Nazwa klasy**: 

    [![](window-images/windowcontroller01.png "Ustawianie nazwy klasy")](window-images/windowcontroller01.png#lightbox)
4. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji.
5. A `WindowController.cs` plik zostanie dodany do projektu w **Eksploratora rozwiązań** w programie Visual Studio dla komputerów Mac: 

    [![](window-images/windowcontroller02.png "Wybiera kontroler systemu windows")](window-images/windowcontroller02.png#lightbox)
6. Otwórz Scenorys w program Xcode Interface Builder.
7. `WindowController.h` Plik będzie dostępny do użytku: 

    [![](window-images/windowcontroller03.png "Edytowanie pliku WindowController.h")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>Dodawanie elementów interfejsu użytkownika

Aby zdefiniować zawartości okna, przeciągnij formanty z **Inspektor biblioteki** na **Edytor interfejsu**. Zobacz nasze [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) dokumentacji, aby uzyskać więcej informacji o używaniu programu Interface Builder do tworzenia i włączania kontrolek.

Na przykład przeciągnijmy narzędzi z **Inspektor biblioteki** na oknie **Edytor interfejsu**:

[![](window-images/edit03.png "Wybieranie paska narzędzi z biblioteki")](window-images/edit03.png#lightbox)

Następnie przeciągnij **widoku tekstu** i rozmiar, aby wypełnił obszar pod paskiem narzędzi:

[![](window-images/edit04.png "Dodawanie widoku tekstu")](window-images/edit04.png#lightbox)

Ponieważ chcemy **widoku tekstu** zmniejszać i zwiększać zgodnie ze zmianami wielkości okna, przejdźmy do **edytora ograniczeń** i dodaj następujące ograniczenia:

[![](window-images/edit05.png "Edytowanie ograniczenia")](window-images/edit05.png#lightbox)

Klikając pozycję dla **czerwoną I świateł** u góry edytora i kliknięcie przycisku **Dodaj ograniczenia 4**, kolejne parametry widok tekstu, aby się działać zgodnie z danym X, Y współrzędnych i zwiększać i zmniejszać, poziomo i pionowo jako zmiany rozmiaru okna.

Na koniec możemy uwidocznić **widoku tekstu** o kodowaniu za pomocą **ujścia** (upewniając się wybrać `ViewController.h` plików):

[![](window-images/edit06.png "Konfigurowanie źródła")](window-images/edit06.png#lightbox)

Zapisz zmiany i przejdź z powrotem do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Aby uzyskać więcej informacji na temat pracy z usługą **gniazd** i **akcje**, zobacz nasze [źródła i akcji](~/mac/get-started/hello-mac.md#outlets-and-actions) dokumentacji.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>Przepływ pracy standardowego okna

W oknie, który można utworzyć i pracować w aplikacji platformy Xamarin.Mac proces jest zasadniczo taki sam jak co to po prostu wykonaliśmy powyżej:

1. Dla nowego systemu windows, które nie są domyślnie dodawane automatycznie do projektu Dodaj nową definicję okna do projektu. To zostanie dokładnie omówione poniżej.
2. Kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć projekt okna do edycji w program Xcode Interface Builder.
3. Przeciągnij nowe okno do projektowania interfejsu użytkownika i podłączyć okna w głównym oknie, używając _przejścia_ (Aby uzyskać więcej informacji, zobacz [przejścia](~/mac/platform/storyboards/indepth.md#Segues) części naszych [Praca ze Scenorysami](~/mac/platform/storyboards/indepth.md) dokumentacji).
3. Ustaw właściwości wymagane okna **Inspektor atrybut** i **Inspektor rozmiar**.
4. Przeciągnij w kontrolkach wymagany do kompilowania interfejsu i skonfigurować je w **Inspektor atrybutu**.
5. Użyj **Inspektor rozmiar** do obsługi rozmiaru dla elementów interfejsu użytkownika.
6. Udostępnianie elementów interfejsu użytkownika w oknie kodu C# za pomocą **gniazd** i **akcje**.
7. Zapisz zmiany i przejdź z powrotem do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Teraz, gdy podstawowe okna utworzonego, omówimy typowe procesy Xamarin.Mac, aplikacja wykonuje podczas pracy z systemem windows. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>Wyświetlanie okna domyślne

Domyślnie nowa aplikacja platformy Xamarin.Mac automatycznie wyświetli okno zdefiniowane w `MainWindow.xib` plików po uruchomieniu:

[![](window-images/display01.png "Przykładowe okno")](window-images/display01.png#lightbox)

Ponieważ możemy zmodyfikować wygląd tego okna, powyżej, teraz obejmuje domyślne narzędzi i **widoku tekstu** kontroli. W sekcji następujące `Info.plist` plik jest odpowiedzialny za wyświetlania tego okna:

[![](window-images/display00.png "Edytowanie pliku Info.plist")](window-images/display00.png#lightbox)

**Interfejsu Main** listy rozwijanej jest używany do wybierania Scenorysem, która będzie służyć jako głównej aplikacji interfejsu użytkownika (w tym przypadku `Main.storyboard`).

Kontroler widoku jest automatycznie dodawane do projektu, aby sterować tym Windows Main, wyświetlanego (wraz z jego widoku głównego). Jest on zdefiniowany w `ViewController.cs` plików i dołączane do **właściciel pliku** w programu Interface Builder w obszarze **Inspektor tożsamości**:

[![](window-images/display02.png "Ustawianie właściciela pliku")](window-images/display02.png#lightbox)

Dla naszych okna, prosimy o poświęcenie ma tytuł `untitled` podczas jego otwierania więc zastąpienie `ViewWillAppear` method in Class metoda `ViewController.cs` do wyglądać podobnie do następującego:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> Ustawiamy wartość okna `Title` właściwość `ViewWillAppear` zamiast metody `ViewDidLoad` metody ponieważ gdy widok może zostać załadowane do pamięci, go nie jeszcze w pełni utworzono wystąpienia. Jeśli firma Microsoft próbował uzyskać dostęp do `Title` właściwości `ViewDidLoad` otrzymamy wynik metody `null` wyjątek, ponieważ okna nie zostały wykonane, a przewodowej w górę do właściwości jeszcze.

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>Programowe zamknięcie okna

Może to być razy, które chcesz programowe zamykanie okna aplikacji rozszerzenia Xamarin.Mac, oprócz posiadania użytkownika kliknij okno **Zamknij** przycisku lub przy użyciu elementu menu. System macOS oferuje dwa różne sposoby, aby zamknąć `NSWindow` programowo: `PerformClose` i `Close`.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

Wywoływanie `PerformClose` metody `NSWindow` symuluje użytkownika, klikając przycisk w oknie **Zamknij** przycisk chwilowo Wyróżnienie przycisku, a następnie zamknięcie okna.

Jeśli aplikacja implementuje `NSWindow`firmy `WillClose` zdarzeń, zostanie wygenerowany, przed zamknięciem okna. Jeśli zdarzenie podaje `false`, a następnie okno nie zostanie zamknięte. Jeśli okno nie ma **Zamknij** przycisk lub nie można zamknąć z jakiejkolwiek przyczyny, system operacyjny zostanie wyemitowany dźwięk alertu.

Na przykład:

```csharp
MyWindow.PerformClose(this);
```

Podejmował próbę Zamknij `MyWindow` `NSWindow` wystąpienia. Jeśli pomyślnie, okno zostanie zamknięte, a inne dźwięk alertu zostanie wyemitowany i spowoduje pozostanie otwarte.

<a name="Close" />

### <a name="close"></a>Zamknięcie

Wywoływanie `Close` metody `NSWindow` nie symuluje użytkownika, klikając przycisk w oknie **Zamknij** przycisk wyróżniając chwilowo przycisku, po prostu na zamknięcie okna.

Okno musi być widoczny zostanie zamknięty i `NSWindowWillCloseNotification` powiadomień zostaną opublikowane w Centrum powiadomień domyślny okna zamykane.

`Close` Metoda różni się na dwa istotne sposoby z `PerformClose` metody:

1. Nie spróbuje podnieść `WillClose` zdarzeń.
2. Nie zasymulować, klikając użytkownika **Zamknij** przycisku przez chwilowo Wyróżnienie przycisku.

Na przykład:

```csharp
MyWindow.Close();
```

Czy aby zamknąć `MyWindow` `NSWindow` wystąpienia.

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>Windows zmodyfikowanej zawartości

W systemie macOS, Apple oferuje sposób poinformowania użytkownika, zawartość okna (`NSWindow`) został zmodyfikowany przez użytkownika i należy zapisać. Jeśli okno zawiera zmodyfikowanej zawartości, mały czarna kropka będą wyświetlane w jego **Zamknij** elementu widget:

[![](window-images/close01.png "Okno programu przy użyciu zmodyfikowanych znacznika")](window-images/close01.png#lightbox)

Jeśli użytkownik próbuje zamknąć okna lub aplikacji Mac, gdy istnieją niezapisane zmiany w zawartości okna, powinni przedstawić [okno dialogowe](~/mac/user-interface/dialog.md) lub [modalny arkusz](~/mac/user-interface/dialog.md) i umożliwić użytkownikowi zapisać swoich zmian, pierwszy:

[![](window-images/close02.png "Zapisywanie arkusza, które są wyświetlane po zamknięciu okna")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>Oznaczanie okna jako zmodyfikowane

Aby oznaczyć jako posiadające zmodyfikował zawartość okna, użyj następującego kodu:

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

I po zapisaniu zmian można wyczyścić przy użyciu flagi zmodyfikowane:

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>Zapisywanie zmian przed zamknięciem okna

Aby obejrzeć dla użytkownika, zamknąć okno i umożliwia im wcześniej zapisać zmodyfikowanej zawartości, należy utworzyć podklasę klasy `NSWindowDelegate` i zastąpić jej `WindowShouldClose` metody. Na przykład:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWidowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            // is the window dirty?
            if (Window.DocumentEdited) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Critical,
                    InformativeText = "Save changes to document before closing window?",
                    MessageText = "Save Document",
                };
                alert.AddButton ("Save");
                alert.AddButton ("Lose Changes");
                alert.AddButton ("Cancel");
                var result = alert.RunSheetModal (Window);

                // Take action based on resu;t
                switch (result) {
                case 1000:
                    // Grab controller
                    var viewController = Window.ContentViewController as ViewController;

                    // Already saved?
                    if (Window.RepresentedUrl != null) {
                        var path = Window.RepresentedUrl.Path;

                        // Save changes to file
                        File.WriteAllText (path, viewController.Text);
                        return true;
                    } else {
                        var dlg = new NSSavePanel ();
                        dlg.Title = "Save Document";
                        dlg.BeginSheet (Window, (rslt) => {
                            // File selected?
                            if (rslt == 1) {
                                var path = dlg.Url.Path;
                                File.WriteAllText (path, viewController.Text);
                                Window.DocumentEdited = false;
                                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                                viewController.View.Window.RepresentedUrl = dlg.Url;
                                Window.Close();
                            }
                        });
                        return true;
                    }
                    return false;
                case 1001:
                    // Lose Changes
                    return true;
                case 1002:
                    // Cancel
                    return false;
                }
            }

            return true;
        }
        #endregion
    }
}
```

Użyj poniższego kodu, aby dołączyć wystąpienie tego obiektu delegowanego do okna:

```csharp
// Set delegate
Window.Delegate = new EditorWidowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>Zapisywanie zmian przed zamknięciem aplikacji

Na koniec aplikacji rozszerzenia Xamarin.Mac należy sprawdzić dowolnego z jego Windows zawierają zmodyfikowanej zawartości i umożliwia użytkownikowi zapisać zmiany przed zamknięciem. Aby to zrobić, Edytuj swoje `AppDelegate.cs` pliku, Zastąp `ApplicationShouldTerminate` metody i przypisz ją wyglądać podobnie do następującego:

```csharp
public override NSApplicationTerminateReply ApplicationShouldTerminate (NSApplication sender)
{
    // See if any window needs to be saved first
    foreach (NSWindow window in NSApplication.SharedApplication.Windows) {
        if (window.Delegate != null && !window.Delegate.WindowShouldClose (this)) {
            // Did the window terminate the close?
            return NSApplicationTerminateReply.Cancel;
        }
    }

    // Allow normal termination
    return NSApplicationTerminateReply.Now;
}
```

<a name="Working_with_Multiple_Windows" />

## <a name="working-with-multiple-windows"></a>Praca z wieloma Windows

Większość aplikacji dla komputerów Mac na podstawie dokumentu można edytować wiele dokumentów, w tym samym czasie. Na przykład edytor tekstu może mieć wiele plików Otwórz do edycji, w tym samym czasie. Domyślnie ma naszej nowej aplikacji platformy Xamarin.Mac **pliku** menu z **New** elementu automatycznie przewodowej w usłudze `newDocument:` **akcji**.

Zamierzamy aktywować nowy element i umożliwić użytkownikowi otwieranie wielu kopii naszych okno główne, aby edytować wiele dokumentów jednocześnie.

Umożliwia edytowanie naszych `AppDelegate.cs` pliku i dodaj następujące obliczoną właściwość:

```csharp
public int UntitledWindowCount { get; set;} =1;
```

Użyjemy można śledzić liczbę niezapisanych plików, więc możemy Prześlij opinię do użytkownika (na wytycznymi firmy Apple, zgodnie z powyższym omówieniem).

Następnie dodaj następującą metodę:

```csharp
[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Ten kod tworzy nową wersję kontrolera okna, ładuje nowe okno, sprawia, że główny i oknie klucz i ustawia jego tytuł. Teraz, jeśli możemy uruchomić aplikację i wybierzesz **New** z **pliku** nowe okno edytora będą otwierane i wyświetlane menu:

[![](window-images/display04.png "Dodano nowe okno bez tytułu")](window-images/display04.png#lightbox)

Jeśli otwieramy **Windows** menu widać aplikacja jest automatycznie śledzenia i naszych otwarte okna obsługi:

[![](window-images/display05.png "Widows menu")](window-images/display05.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z menu w aplikacji platformy Xamarin.Mac, zobacz nasze [Praca z menu](~/mac/user-interface/menu.md) dokumentacji.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>Wprowadzenie do aktualnie aktywnego okna

W aplikacji platformy Xamarin.Mac, który można otworzyć wiele okien (dokumenty) istnieją razy, gdy konieczne będzie pobieranie aktualnych i najwyższego poziomu okna (okno klucza). Poniższy kod zwróci okna klucza:

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

Może ona zostać wywołana w dowolnej klasy lub metody, która musi uzyskać dostęp do aktualnych i klucza okna. Jeśli okno nie jest obecnie otwarty, to zostanie zwrócona `null`.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>Uzyskiwanie dostępu do wszystkich aplikacji Windows

Możliwe czasy których trzeba uzyskiwać dostęp do wszystkich systemu windows, że Twoja aplikacja platformy Xamarin.Mac obecnie ma open. Na przykład aby sprawdzić, czy użytkownik chce, aby otworzyć plik jest już otwarty w oknie istniejącej.

`NSApplication.SharedApplication` Przechowuje `Windows` właściwości, które zawiera tablicę wszystkie otwarte okna w swojej aplikacji. Można wykonać iterację za pośrednictwem tej tablicy, uzyskać dostęp do wszystkich okien bieżącej aplikacji. Na przykład:

```csharp
// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

W przykładowym kodzie możemy rzutowanie każdego zwróconego okna niestandardowe `ViewController` klasy w naszej aplikacji i wartość niestandardowego testowania `Path` ścieżkę do pliku, użytkownik chce, aby otworzyć właściwości. Jeśli plik jest już otwarty, możemy także tego okna do przodu.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>Dostosowywanie rozmiaru okna w kodzie

Istnieją terminy, gdy aplikacja potrzebuje rozmiar okna w kodzie. Aby rozmiar i położenie okna, możesz dostosować firmy `Frame` właściwości. Zmieniając rozmiar okna, zazwyczaj należy również dostosować jego źródła, aby zachować okna w tej samej lokalizacji, ze względu na układ współrzędnych dla systemu macOS.

Inaczej niż w przypadku której lewy górny róg reprezentuje (0,0) dla systemu iOS macOS używa matematycznych współrzędnych, gdzie reprezentuje (0,0) w dolnym lewym dolnym rogu ekranu. W systemie iOS współrzędne zwiększa przesuwana kierunku po prawej stronie. W systemie macOS współrzędne wzrost wartość do góry z prawej strony. 

Poniższy przykład kodu zmienia rozmiar okna:

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> Podczas dostosowywania systemu windows, rozmiar i lokalizacja w kodzie, musisz upewnij się, że należy przestrzegać minimalny i maksymalny rozmiar, ustawione w programu Interface Builder. To nie automatycznie uznawane i będzie można wyświetlić okno większe lub mniejsze niż te limity.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>Monitorowanie zmian rozmiaru okna

Możliwe czasy gdzie należy monitorować zmiany rozmiaru okna wewnątrz aplikacji platformy Xamarin.Mac. Na przykład, aby odświeżyć zawartość do nowego rozmiaru.

Aby monitorować zmiany rozmiaru, należy najpierw upewnić się, czy użytkownik przypisał klasę niestandardową dla kontrolera okna w program Xcode Interface Builder. Na przykład `MasterWindowController` poniżej:

[![](window-images/resize01.png "Inspektor tożsamości")](window-images/resize01.png#lightbox)

Następnie edytuj niestandardowe klasy kontroler okna i monitor `DidResize` zdarzeń w oknie kontrolera Aby otrzymywać powiadomienia o zmianach rozmiarów na żywo. Na przykład:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

Opcjonalnie możesz użyć `DidEndLiveResize` zdarzeń tylko zgłaszane po użytkownik zakończył, zmiana rozmiaru okna. Na przykład:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

        Window.DidEndLiveResize += (sender, e) => {
        // Do something after the user's finished resizing
        // the window
    };
}
```

<a name="Setting_a_Window’s_Title_and_Represented_File" />

## <a name="setting-a-windows-title-and-represented-file"></a>Okno w tytule i reprezentowane pliku

Podczas pracy z systemem windows, które reprezentują dokumenty, `NSWindow` ma `DocumentEdited` że jeśli właściwością `true` Wyświetla małej kropki w przycisk Zamknij, aby przyznać użytkownikowi wskazanie, że plik został zmodyfikowany i powinny zostać zapisane przed zamknięciem.

Umożliwia edytowanie naszych `ViewController.cs` plik i dokonaj następujących zmian:

```csharp
public bool DocumentEdited {
    get { return View.Window.DocumentEdited; }
    set { View.Window.DocumentEdited = value; }
}
...

public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";

    View.Window.WillClose += (sender, e) => {
        // is the window dirty?
        if (DocumentEdited) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to give the user the ability to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    };
}

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Show when the document is edited
    DocumentEditor.TextDidChange += (sender, e) => {
        // Mark the document as dirty
        DocumentEdited = true;
    };

    // Overriding this delegate is required to monitor the TextDidChange event
    DocumentEditor.ShouldChangeTextInRanges += (NSTextView view, NSValue[] values, string[] replacements) => {
        return true;
    };

}
```

Możemy również monitorowanie `WillClose` zdarzeń w oknie i sprawdzając stan `DocumentEdited` właściwości. Jeśli jest `true` musimy przyznać użytkownikowi możliwości, aby zapisać zmiany w pliku. Jeśli firma Microsoft uruchomić naszą aplikację i wprowadź jakiś tekst, zostanie wyświetlony kropki (.):

[![](window-images/file01.png "Zmieniono okna")](window-images/file01.png#lightbox)

Podejmowane są próby zamknij okno, uzyskujemy alertu:

[![](window-images/file02.png "Zapisz wyświetlania okna dialogowego")](window-images/file02.png#lightbox)

Ładujemy dokumentu z pliku możemy ustawić tytuł okna w pliku przy użyciu `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` — metoda (biorąc pod uwagę, że `path` jest ciągiem reprezentującym otwierany plik). Ponadto można ustawić adres URL pliku przy użyciu `window.RepresentedUrl = url;` metody.

Jeśli adres URL wskazuje typ pliku, znane przez system operacyjny, pojawi się jego ikonę na pasku tytułu. Jeśli użytkownik kliknie prawym przyciskiem myszy na ikonie, pojawią się ścieżkę do pliku.

Umożliwia edytowanie `AppDelegate.cs` pliku i dodaj następującą metodę:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = true;
    dlg.CanChooseDirectories = false;

    if (dlg.RunModal () == 1) {
        // Nab the first file
        var url = dlg.Urls [0];

        if (url != null) {
            var path = url.Path;

            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow(this);

            // Load the text into the window
            var viewController = controller.Window.ContentViewController as ViewController;
            viewController.Text = File.ReadAllText(path);
                    viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
            viewController.View.Window.RepresentedUrl = url;

        }
    }
}
```

Teraz Jeśli możemy uruchomić naszą aplikację, wybierz **Otwórz...**  z **pliku** menu, wybierz plik tekstowy z **Otwórz** okna dialogowego pole, a następnie otwórz go:

[![](window-images/file03.png "Otwarte okno dialogowe")](window-images/file03.png#lightbox)

Plik, który będzie wyświetlany i tytuł zostanie ustawiony z ikoną pliku:

[![](window-images/file04.png "Załadowano zawartość pliku")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>Dodawanie nowego okna do projektu

Oprócz okna głównego dokumentu aplikacji rozszerzenia Xamarin.Mac może być konieczne mają być wyświetlane inne rodzaje systemu windows użytkownika, takie jak preferencje lub inspektor paneli.

Aby dodać nowe okno, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w program Xcode Interface Builder.
2. Przeciągnij nowy **kontroler okna** z **biblioteki** i upuść je na **powierzchni projektowej**:

    [![](window-images/new01.png "Wybiera nowy kontroler okna w bibliotece")](window-images/new01.png#lightbox)
3. W **Inspektor tożsamości**, wprowadź `PreferencesWindow` dla **identyfikator scenorysu**: 

    [![](window-images/new02.png "Ustawianie Identyfikatora scenorysu")](window-images/new02.png#lightbox)
5. Projektowanie interfejsu użytkownika: 

    [![](window-images/new03.png "Projektowanie interfejsu użytkownika")](window-images/new03.png#lightbox)
6. Otwórz Menu aplikacji (`MacWindows`), wybierz opcję **Preferencje...** , Przytrzymując klawisz CTRL i przeciągnij go do nowego okna: 

    [![](window-images/new05.png "Tworzenie segue")](window-images/new05.png#lightbox)
7. Wybierz **Pokaż** z menu podręcznego.
6. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Jeśli możemy uruchomić kod i wybierz pozycję **Preferencje...**  z **Menu aplikacji**, zostanie wyświetlone okno:

[![](window-images/new04.png "Menu Preferencje próbki")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>Praca z paneli

Jak wspomniano na początku tego artykułu, panel pojawia się nad innymi oknami i zapewnia narzędzia lub kontrolki, które użytkownicy mogą pracować z, gdy są otwarte dokumenty. 

Podobnie jak dowolny inny typ okna, który można utworzyć i pracować w aplikacji platformy Xamarin.Mac proces jest zasadniczo taki sam:

1. Dodaj nową definicję okna do projektu.
2. Kliknij dwukrotnie `.xib` plik, aby otworzyć projekt okna do edycji w program Xcode Interface Builder.
2. Ustaw właściwości wymagane okna **Inspektor atrybut** i **Inspektor rozmiar**.
4. Przeciągnij w kontrolkach wymagany do kompilowania interfejsu i skonfigurować je w **Inspektor atrybutu**.
5. Użyj **Inspektor rozmiar** do obsługi rozmiaru dla elementów interfejsu użytkownika.
6. Udostępnianie elementów interfejsu użytkownika w oknie kodu C# za pomocą **gniazd** i **akcje**.
7. Zapisz zmiany i przejdź z powrotem do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

W **Inspektor atrybutu**, masz następujące opcje specyficzne dla panele:

[![](window-images/panel03.png "Inspektor atrybutu")](window-images/panel03.png#lightbox)

- **Styl** — pozwalają dostosować styl panelu z: regularne panelu (wygląda standardowego okna), narzędzia panelu (ma mniejszy paska tytułu), HUD Panel (jest przezroczysty i na pasku tytułu jest częścią tła).
- **Uaktywnianie nie** — Określa, w panelu staje się oknem klucza.
- **Dokumentowanie modalne** — Jeśli dokument modalne, panel tylko przesunie się nad aplikacji systemu windows, przeciwnym razie jest ona wyświetlana przede wszystkim.


Aby dodać nowy Panel, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nad projektem i wybierz **Dodaj** > **nowy plik...** .
2. W oknie dialogowym Nowy plik, wybierz **Xamarin.Mac** > **okno Cocoa z kontrolerem**:

    [![](window-images/panels00.png "Dodawanie nowych kontroler okna")](window-images/panels00.png#lightbox)
3. Wprowadź `DocumentPanel` dla **nazwa** i kliknij przycisk **New** przycisku.
4. Kliknij dwukrotnie `DocumentPanel.xib` plik, aby go otworzyć do edycji w programu Interface Builder: 

    [![](window-images/new02.png "Edytowanie pannel")](window-images/new02.png#lightbox)
5. Usuń istniejące okno i przeciągnij panelu z poziomu **Inspektor biblioteki** w **Edytor interfejsu**: 

    [![](window-images/panels01.png "Usuwanie istniejącego okna")](window-images/panels01.png#lightbox)
6. Podłączać panelu **właściciel pliku** - **okna** - **ujścia**: 

    [![](window-images/panels02.png "Przeciągając przewodowy Panel")](window-images/panels02.png#lightbox)
7. Przełącz się do **Inspektor tożsamości** i ustaw klasy panelu `DocumentPanel`: 

    [![](window-images/panels03.png "Klasa panelu ustawień")](window-images/panels03.png#lightbox)
6. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.
7. Edytuj `DocumentPanel.cs` pliku i zmień definicję klasy do następującego: 

    `public partial class DocumentPanel : NSPanel`
8. Zapisz zmiany w pliku.

Edytuj `AppDelegate.cs` pliku i upewnij `DidFinishLaunching` wygląd metoda podobne do następującego:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

Jeśli możemy uruchomić aplikację, zostanie wyświetlony panel:

[![](window-images/panels04.png "Panel w uruchomionej aplikacji")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> Panel Windows została zastąpiona przez firmę Apple i powinien zostać zamieniony **inspektora interfejsów**. Aby uzyskać pełny przykład tworzenia **Inspektor** w aplikacji platformy Xamarin.Mac, zobacz nasze [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) przykładową aplikację.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z Windows i paneli w aplikacji platformy Xamarin.Mac. Firma dostrzegła różnych typów i korzysta z systemu Windows i paneli, jak tworzyć i obsługiwać Windows i paneli w program Xcode Interface Builder i sposób pracy z Windows i paneli w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacWindows (przykład)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (przykład)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Praca z menu](~/mac/user-interface/menu.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
