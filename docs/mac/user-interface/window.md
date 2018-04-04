---
title: Windows
description: W tym artykule omówiono pracy z systemem windows i panele w aplikacji Xamarin.Mac. Opisuje tworzenia systemu windows i panele w środowisku Xcode i interfejs konstruktora, ładowaniem ich z scenorys i .xib plików i pracy z nimi programistycznie.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: f45bc69b74d98c7b9130f2caeaee91b184c38d87
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="windows"></a>Windows

_W tym artykule omówiono pracy z systemem windows i panele w aplikacji Xamarin.Mac. Opisuje tworzenia systemu windows i panele w środowisku Xcode i interfejs konstruktora, ładowaniem ich z scenorys i .xib plików i pracy z nimi programistycznie._

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, mają dostęp do tego samego systemu Windows i płyty, które osoba używająca *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć w środowisku Xcode _konstruktora interfejsu_ do tworzenia i obsługi sieci systemu Windows i panele (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

W oparciu o jego przeznaczenie, aplikacji Xamarin.Mac może stanowić co najmniej jednego systemu Windows na ekranie do zarządzania i koordynowania informacje w nim wyświetlane oraz współpracuje z. Podstawowe funkcje okna są:

1. Aby zapewnić obszar widoki i formanty może być umieszczone i zarządzane.
2. Aby zaakceptować i reagowania na zdarzenia w odpowiedzi na interakcję użytkownika z klawiatury i myszy.

Systemu Windows mogą być używane w stanie niemodalne (takich jak edytor tekstu, który może mieć wielu dokumentów otwartych jednocześnie) lub modalne (na przykład okno eksportu, który musi być ukryty, zanim będzie można kontynuować aplikacji).

Panele to specjalny rodzaj okna (podklasy elementu bazowego `NSWindow` klasy), zwykle obsługujących funkcję pomocnicze w aplikacji, takich jak narzędzia systemu windows, takich jak inspektorzy formatu tekstu i selektora kolorów systemu.

[![](window-images/intro01.png "Edytowanie okna w środowisku Xcode")](window-images/intro01.png#lightbox)

W tym artykule omówione zostaną następujące czynności podstawowe informacje dotyczące pracy z systemem Windows i panele w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` poleceń używany do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Wprowadzenie do systemu Windows

Jak już wspomniano, okno obszar, w którym widoki i formanty można umieścić i zarządzane i reaguje na zdarzenia w oparciu o interakcji z użytkownikiem (za pośrednictwem klawiatury lub myszy).

Zgodnie z firmy Apple istnieją pięć typów główne systemu Windows w macOS aplikacji:

- **Okna dokumentów** — okno dokumentu zawiera dane użytkownika opartych na plikach, takie jak arkusz kalkulacyjny lub dokument tekstowy.
- **Okno aplikacji** -okna aplikacji jest w głównym oknie aplikacji, która nie jest na podstawie dokumentu (takie jak aplikacja kalendarza na komputerze Mac).
- **Panel** — panelu pojawia się powyżej innych oknach i oferuje narzędzia, lub Otwórz formantów, które użytkownicy mogą pracować z, gdy są dokumenty. W niektórych przypadkach może być przezroczyste panelu (np. podczas pracy z dużych obrazów).
- **Okno dialogowe** — okno dialogowe zostanie wyświetlone w odpowiedzi na akcję użytkownika i zwykle zapewnia użytkownikom sposoby może zakończyć akcji. Okno dialogowe wymaga odpowiedzi od użytkownika może zostać zamknięty. (Zobacz [Praca z okien dialogowych](~/mac/user-interface/dialog.md))
- **Alerty** — alert jest specjalnym rodzajem okna dialogowego, który jest wyświetlany, gdy występuje poważny problem (np. Błąd) lub jako ostrzeżenie (takich jak przygotowywanie do usunięcia pliku). Ponieważ alert jest okno dialogowe, wymagany jest również odpowiedzi użytkownika przed jego zamknięciem. (Zobacz [Praca z alertami](~/mac/user-interface/alert.md))

Aby uzyskać więcej informacji, zobacz [systemu Windows o](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>Main, klucz i nieaktywne systemu Windows

System Windows w aplikacji Xamarin.Mac może wyglądać i zachowywać się inaczej w zależności od jak użytkownik jest obecnie interakcji z nimi. Przedniej dokumentu lub okna aplikacji, która jest aktualnie fokus uwagi użytkownika jest nazywany _okno główne_. W większości przypadków tego okna będzie również _okna klucza_ (okno obecnie akceptuje dane wejściowe użytkownika). Ale nie jest to zawsze wielkość liter, na przykład selektora kolorów może być otwarty i w oknie klucz, w którym użytkownik prowadzi interakcję z zmiany stanu elementu okno dokumentu (który nadal będzie okno główne).

Główne i klucz Windows (jeśli są osobne) zawsze są aktywne, _nieaktywne Windows_ są otwarte okna, które nie są na pierwszym planie. Na przykład aplikacja edytora tekstu może mieć więcej niż jeden dokument otwarty naraz tylko okno główne będzie aktywny, inni będą nieaktywne. 

Aby uzyskać więcej informacji, zobacz [systemu Windows o](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Nazewnictwo systemu Windows

Można wyświetlić w oknie paska tytułu i po wyświetleniu tytuł zazwyczaj jest to nazwa aplikacji, nazwy dokumentu wykorzystywanej na lub funkcja okna (np. inspektora). Niektóre aplikacje nie wyświetlaj pasek tytułu, ponieważ rozpoznawalnych przez procesów, a nie Praca z dokumentami.

Apple Sugeruj następujących wytycznych:

- Korzystać z nazwy aplikacji, tytuł okna głównego, niebędącego dokumentem. 
- Nazwa nowe okno dokumentu `untitled`. Pierwszy nowego dokumentu nie append numer do tytułu (takie jak `untitled 1`). Jeśli użytkownik tworzy nowy dokument przed zapisaniem i tytułowych pierwszy, wywołaj okno `untitled 2`, `untitled 3`itp.

Aby uzyskać więcej informacji, zobacz [nazewnictwa Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>Windows pełnego ekranu

W macOS, okna aplikacji można przejść ukrywanie pełnoekranowy wszystko na pasku Menu aplikacji (co może być ujawniony przez Przesuń kursor w górnej części ekranu) w tym utrudnień wolnego interakcję z nią jest zawartości.

Apple sugeruje następujących wytycznych:

- Ustal, czy warto okna przejść pełny ekran. Aplikacji, które mają krótki interakcji (na przykład Kalkulator) nie należy podać tryb pełnoekranowy.
- Wyświetl pasek narzędzi, jeśli zadanie pełnego ekranu wymaga go. Zwykle pasek narzędzi jest ukryty w trybie pełnoekranowym.
- Okno pełnego ekranu powinien mieć wszyscy użytkownicy funkcje wymagane do ukończenia tego zadania.
- Jeśli to możliwe należy unikać wyszukiwania interakcji, podczas, gdy użytkownik jest w trybie pełnoekranowym.
- Skorzystaj z miejsca na ekranie zwiększona bez przesuwania fokus poza głównym zadaniem.

Aby uzyskać więcej informacji, zobacz [systemu Windows w trybie pełnoekranowym](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>Paneli

Panel jest pomocnicze okna, zawierający formanty, opcje, które mają wpływ na aktywny dokument lub (np. system próbnika kolorów):

[![](window-images/panel01.png "Panel kolorów")](window-images/panel01.png#lightbox)

Panele mogą być _specyficzny dla aplikacji_ lub _ogólnosystemowe_. Panele specyficzny dla aplikacji float w górnej części okna dokumentów aplikacji i znikają w przypadku aplikacji znajduje się w tle. Panele ogólnosystemowe (takich jak **czcionki** panelu), float na wszystkie otwarte okna, niezależnie od aplikacji. 

Apple sugeruje następujących wytycznych:

- Ogólnie rzecz biorąc, za pomocą panelu standardowe, przezroczyste panele powinien być używany tylko oszczędnie i dla graficznie zadania.
- Należy wziąć pod uwagę, aby umożliwić użytkownikom łatwy dostęp do formantów ważne lub informacji, które bezpośrednio wpływa na ich zadań za pomocą panelu.
- Ukryj i Pokaż panele zgodnie z potrzebami.
- Panele zawsze powinna zawierać paska tytułu.
- Panele nie może zawierać przycisk Minimalizuj aktywne.

#### <a name="inspectors"></a>Inspektorzy

Większość nowoczesnych aplikacji macOS przedstawia pomocniczego kontrolki i opcje, które mają wpływ na aktywny dokument lub zaznaczenie jako _inspektorzy_ które są częścią okno główne (takich jak **stron** aplikacji pokazano poniżej), zamiast przy użyciu panelu systemu Windows:

[![](window-images/panel02.png "Przykład Inspektora")](window-images/panel02.png#lightbox)

Aby uzyskać więcej informacji, zobacz [panele](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) i naszych [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) Przykładowa aplikacja dla pełnej implementacji **Interfejsu inspektora** w aplikacji Xamarin.Mac.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>Tworzenie i obsługę systemu Windows w środowisku Xcode

Podczas tworzenia nowej aplikacji Xamarin.Mac Cocoa, otrzymasz standardowe okno puste, domyślnie. Ten system windows jest zdefiniowany w `.storyboard` pliku automatycznie dołączony do projektu. Aby edytować w projekcie systemu windows **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku:

[![](window-images/edit01.png "Wybieranie głównego storyboard")](window-images/edit01.png#lightbox)

Spowoduje to otwarcie okna projektu w Konstruktorze interfejsu w środowisku Xcode:

[![](window-images/edit02.png "Edytowanie interfejsu użytkownika w środowisku Xcode")](window-images/edit02.png#lightbox)

W **inspektora atrybutu**, istnieje kilka właściwości, które służą do definiowania i kontrolowania okna:

- **Tytuł** -to jest tekst, który będzie wyświetlany w pasek tytułu okna.
- **Automatyczne zapisywanie** — jest to _klucza_ który będzie używany do IDENTYFIKATORA okna po jego położenie ustawienia i są automatycznie zapisywane.
- **Pasek tytułu** — okno Wyświetla pasek tytułu.
- **Ujednolicone tytuł i narzędzi** — Jeśli okno zawiera narzędzi, powinny być częścią paska tytułu.
- **Pełny widok zawartości o rozmiarze** — umożliwia obszar zawartości okna, aby być poniżej paska tytułu.
- **Cień** — okno ma cień.
- **Teksturę** -teksturą systemu windows można użyć efekty (na przykład popularność) i mogą być przenoszone między przez przeciąganie ich treści.
- **Zamknij** — okno ma przycisk Zamknij.
- **Minimalizowanie** — okno ma przycisk Minimalizuj.
- **Zmień rozmiar** — okno ma rozmiar formantu.
- **Przycisk paska narzędzi** — okno ma przycisk Pokaż/Ukryj.
- **Umożliwiająca przywrócenie** — jest ustawienia automatycznie zapisywany i przywracany i pozycja okna.
- **Widoczne na uruchamianie** — okno automatycznie przedstawiono kiedy `.xib` ładowania pliku.
- **Ukryj na dezaktywować** — jest okno ukryte po przejściu tło w aplikacji.
- **Zwolnij po zamknięciu** -okna wyczyszczone z pamięci, gdy jest ono zamknięte.
- **Zawsze wyświetlana etykietki** -są stale wyświetlany etykietki narzędzi.
- **Ponownie oblicza pętli widoku** -kolejności Wyświetl obliczenia przed narysowaniem okna.
- **Spacje**, **Exposé** i **funkcja włączania i wyłączania** — wszystkie Definiowanie zachowania okna w tych środowiskach macOS. 
- **Pełny ekran** — Określa, jeśli to okno można wprowadzić w trybie pełnoekranowym. 
- **Animacja** — Określa typ animacji dostępne dla okna.
- **Wygląd** -steruje wyglądem okna. Obecnie brak wygląd tylko jeden Akwamaryna.

Zobacz firmy Apple [wprowadzenie do systemu Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) i [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) dokumentację, aby uzyskać więcej szczegółowych informacji.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>Ustawienie domyślne rozmiar i położenie

Aby ustawić początkowe położenie okna, a jego rozmiar, przełącz się do **inspektora rozmiar**:

[![](window-images/edit07.png "Domyślny rozmiar i położenie")](window-images/edit07.png#lightbox)

W tym miejscu można ustawić początkową wartość rozmiaru okna, nadaj minimalnego i maksymalnego rozmiaru, Ustaw początkową lokalizację na ekranie i kontrolować obramowania okna.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>Ustawianie kontrolera niestandardowego okna głównego

Aby można było utworzyć gniazda i akcji, aby ujawnić elementy interfejsu użytkownika do kodu C#, aplikacji Xamarin.Mac należy być przy użyciu kontrolera niestandardowego okna.

Wykonaj następujące czynności:

1. Otwórz scenorysu aplikacji w Konstruktorze interfejsu w środowisku Xcode.
2. Wybierz `NSWindowController` na powierzchni projektu.
3. Przełącz się do **inspektora tożsamości** Wyświetl i wprowadź `WindowController` jako **Nazwa klasy**: 

    [![](window-images/windowcontroller01.png "Ustawienie nazwy klasy")](window-images/windowcontroller01.png#lightbox)
4. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji.
5. A `WindowController.cs` plik zostanie dodany do projektu w **Eksploratora rozwiązań** w programie Visual Studio dla komputerów Mac: 

    [![](window-images/windowcontroller02.png "Wybiera kontroler systemu windows")](window-images/windowcontroller02.png#lightbox)
6. Otwórz ponownie scenorysu w Konstruktorze interfejsu w środowisku Xcode.
7. `WindowController.h` Plik będzie dostępna do użycia: 

    [![](window-images/windowcontroller03.png "Edytowanie pliku WindowController.h")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>Dodawanie elementów interfejsu użytkownika

Aby zdefiniować zawartości okna, przeciągnij formanty z **inspektora biblioteki** na **Edytor interfejsu**. Zobacz nasze [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) dokumentację, aby uzyskać więcej informacji na temat tworzenia i włączania formantów przy użyciu konstruktora interfejsu.

Na przykład załóżmy przeciągnij pasek narzędzi **inspektora biblioteki** na oknie **Edytor interfejsu**:

[![](window-images/edit03.png "Wybieranie narzędzi z biblioteki")](window-images/edit03.png#lightbox)

Następnie przeciągnij **widoku tekstu** i rozmiar, aby wypełnił obszar w pasku narzędzi:

[![](window-images/edit04.png "Dodawanie widoku tekstu")](window-images/edit04.png#lightbox)

Ponieważ chcemy **widoku tekstu** Aby zmniejszyć rozmiar i powiększania jako zmiany rozmiaru okna, umożliwia przełączanie do **edytora ograniczeń** i dodaj następujące ograniczenia:

[![](window-images/edit05.png "Edytowanie ograniczenia")](window-images/edit05.png#lightbox)

Klikając dla **czerwony I świateł** w górnej części edytora i kliknięcie przycisku **dodać ograniczenia 4**, firma Microsoft informuje widoku tekstu, aby trzymać danego X, Y współrzędne i zwiększać i zmniejszać poziomie i w pionie jako Zmieniono rozmiar okna.

Ponadto umożliwia udostępnianie **widoku tekstu** kodu za pomocą **gniazda** (upewnieniu się wybrać `ViewController.h` pliku):

[![](window-images/edit06.png "Konfigurowanie gniazda")](window-images/edit06.png#lightbox)

Zapisz zmiany i wrócić do programu Visual Studio for Mac synchronizację w programie Xcode.

Aby uzyskać więcej informacji na temat pracy z **gniazda** i **akcje**, zobacz nasze [gniazda i akcji](~/mac/get-started/hello-mac.md#Outlets_and_Actions) dokumentacji.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>Standardowe okno przepływu pracy

Oknie, Utwórz i pracować z aplikacji Xamarin.Mac proces jest zasadniczo taki sam, jak co możemy właśnie wykonano powyżej:

1. Dla nowego systemu windows, które nie są domyślnie automatycznie dodawane do projektu należy dodać nową definicję okna do projektu. To zostanie dokładnie omówione szczegółowo poniżej.
2. Kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć projekt okna do edycji w Konstruktorze interfejsu w środowisku Xcode.
3. Przeciągnij nowe okno do projektu interfejsu użytkownika a Podłączanie okna do głównego okna przy użyciu _Segues_ (Aby uzyskać więcej informacji, zobacz [Segues](~/mac/platform/storyboards/indepth.md#Segues) sekcji naszych [Praca z Scenorys](~/mac/platform/storyboards/indepth.md) dokumentacji).
3. Ustaw właściwości wymagane okna **inspektora atrybutu** i **inspektora rozmiar**.
4. Przeciągnij formanty wymagane do tworzenia interfejsu i skonfigurować je w **inspektora atrybutu**.
5. Użyj **inspektora rozmiar** do obsługi zmiana rozmiaru dla elementów interfejsu użytkownika.
6. Udostępnianie okna elementy interfejsu użytkownika do kodu C# za pomocą **gniazda** i **akcje**.
7. Zapisz zmiany i wrócić do programu Visual Studio for Mac synchronizację w programie Xcode.

Teraz, gdy mamy podstawowa okna utworzonego przyjrzymy typowe procesy Xamarin.Mac aplikacji jest podczas pracy z systemem windows. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>Wyświetlanie okna domyślne

Domyślnie nowa aplikacja Xamarin.Mac automatycznie wyświetli okno zdefiniowane w `MainWindow.xib` pliku po uruchomieniu:

[![](window-images/display01.png "Okno przykład uruchomiony")](window-images/display01.png#lightbox)

Ponieważ firma Microsoft zmodyfikowane projektu przedział powyżej, teraz zawiera domyślne narzędzi i **widoku tekstu** formantu. Poniższa sekcja w `Info.plist` plik jest odpowiedzialny za wyświetlania tego okna:

[![](window-images/display00.png "Edytowanie Info.plist")](window-images/display00.png#lightbox)

**Interfejsu Main** listy rozwijanej jest używany do wybierania scenorysu, który będzie używany jako głównej aplikacji interfejsu użytkownika (w tym przypadku `Main.storyboard`).

Kontroler widoku jest automatycznie dodawany do projektu, aby kontrolować tego Windows Main, która zostanie wyświetlona (wraz z jego widoku głównego). Jest on zdefiniowany w `ViewController.cs` plików i dołączane do **właścicielem pliku** w Konstruktorze interfejsu w obszarze **inspektora tożsamości**:

[![](window-images/display02.png "Ustawienie właściciela pliku")](window-images/display02.png#lightbox)

W naszym okna, chcielibyśmy ma tytuł `untitled` podczas jego otwierania więc warto zastąpienie `ViewWillAppear` metody w `ViewController.cs` do wyglądać następująco:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> Konfigurujemy ustawienia wartość okna `Title` właściwości w `ViewWillAppear` zamiast metody `ViewDidLoad` metody ponieważ, podczas gdy widok może zostać załadowane do pamięci, go nie jeszcze pełni utworzono wystąpienia. Jeśli podjęto próbę uzyskania dostępu `Title` właściwości w `ViewDidLoad` — metoda może pobrać `null` wyjątek od okna nie zostało wykonane, a przewodowej w górę do właściwości jeszcze.

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>Programowo zamknięcie okna

Może to być razy, na których mają być programowane zamykanie okna aplikacji Xamarin.Mac, oprócz posiadania użytkownika kliknij przycisk okna **zamknąć** przycisku lub przy użyciu elementu menu. System macOS zawiera dwa różne sposoby, aby zamknąć `NSWindow` programowo: `PerformClose` i `Close`.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

Wywoływanie `PerformClose` metody `NSWindow` symuluje kliknięciu okna **Zamknij** przycisk na chwilę wyróżnianie przycisku i zamknięcie okna.

Jeśli aplikacja korzysta `NSWindow`w `WillClose` zdarzeń, zostanie wygenerowany, przed zamknięciem okna. Jeśli zdarzenie zwraca `false`, a następnie okno nie zostanie zamknięte. Jeśli okno nie ma **Zamknij** przycisk lub nie można zamknąć z jakiejkolwiek przyczyny, system operacyjny będzie emitować dźwięk alertu.

Na przykład:

```csharp
MyWindow.PerformClose(this);
```

Czy próbować zamknąć `MyWindow` `NSWindow` wystąpienia. Jeśli powiedzie się, okno zostanie zamknięte, przeciwnym razie dźwięk alertu zostanie wyemitowany i będzie pozostanie otwarte.

<a name="Close" />

### <a name="close"></a>Zamknięcie

Wywoływanie `Close` metody `NSWindow` nie symuluje kliknięciu okna **Zamknij** przycisk przez wyróżnianie na chwilę przycisku, po prostu zamyka okna.

Okno musi być widoczny zostanie zamknięty i `NSWindowWillCloseNotification` powiadomień zostaną opublikowane w Centrum powiadomień domyślne zamykania okna.

`Close` Metoda różni się na dwa sposoby ważne z `PerformClose` metody:

1. Nie próbuje podnieść `WillClose` zdarzeń.
2. Nie zasymulować, klikając użytkownika **Zamknij** przycisk przez wyróżnianie na chwilę przycisku.

Na przykład:

```csharp
MyWindow.Close();
```

Czy zamknąć `MyWindow` `NSWindow` wystąpienia.

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>Zawartości zmodyfikowane systemu Windows

W macOS, Apple udostępnił sposób poinformowania użytkownika o który zawartość okna (`NSWindow`) został zmodyfikowany przez użytkownika i należy zapisać. Jeśli zawartość zmodyfikowanego okna małych czarna kropka będą wyświetlane w jego **Zamknij** elementu widget:

[![](window-images/close01.png "Okno ze znacznikiem zmodyfikowane")](window-images/close01.png#lightbox)

Jeśli użytkownik próbuje zamknąć okna lub aplikacji Mac, gdy istnieją niezapisane zmiany w zawartości okna, powinni przedstawić [okno dialogowe](~/mac/user-interface/dialog.md) lub [modalny arkusz](~/mac/user-interface/dialog.md) i umożliwia użytkownikowi zapisać swoje zmiany pierwszy:

[![](window-images/close02.png "Zapisywanie arkusza wyświetlane po zamknięciu okna")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>Okno ze zmianami oznaczanie

Aby oznaczyć jako posiadające zmodyfikował zawartość okna, należy użyć poniższego kodu:

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

I po zapisaniu zmian, usuń zaznaczenie przy użyciu flagi zmodyfikowane:

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>Trwa zapisywanie zmian przed zamknięciem okna

Aby obejrzeć użytkownikowi zamknięcie okna, dzięki czemu ich wcześniej zapisać zmodyfikowanej zawartości, należy utworzyć podklasy `NSWindowDelegate` i zastąp jego `WindowShouldClose` metody. Na przykład:

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

Aby dołączyć wystąpienie tego obiektu delegowanego do okna, należy użyć poniższego kodu:

```csharp
// Set delegate
Window.Delegate = new EditorWidowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>Trwa zapisywanie zmian przed zamknięciem aplikacji

Na koniec Xamarin.Mac aplikacji należy sprawdzić wszelkie jej systemu Windows zawierają zmodyfikowanej zawartości i umożliwia użytkownikowi zapisać zmiany przed zamknięciem. Aby to zrobić, Edytuj Twojej `AppDelegate.cs` pliku, Zastąp `ApplicationShouldTerminate` — metoda i zapewnić ich wyglądać następująco:

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

## <a name="working-with-multiple-windows"></a>Praca z wieloma systemu Windows

Większość aplikacji dla komputerów Mac na podstawie dokumentu można edytować wielu dokumentów, w tym samym czasie. Na przykład edytor tekstu może mieć wiele plików Otwórz do edycji w tym samym czasie. Domyślnie ma naszej nowej aplikacji Xamarin.Mac **pliku** menu z **nowy** elementu automatycznie przewodowej zapasowych `newDocument:` **akcji**.

Zamierzamy aktywować nowy element i umożliwia użytkownikowi otwieranie wielu kopii naszych okno główne, aby edytować wiele dokumentów jednocześnie.

Umożliwia edytowanie naszych `AppDelegate.cs` i dodaj następujące właściwości obliczanej:

```csharp
public int UntitledWindowCount { get; set;} =1;
```

Użyjemy można śledzić liczbę niezapisanych plików, dlatego firma Microsoft można przesłać opinię użytkownikowi (na wytycznymi firmy Apple, zgodnie z opisem powyżej).

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

Ten kod tworzy nową wersję kontrolera okna, ładuje nowe okno główne i okno klucza i ustawia go tytuł. Teraz możemy uruchomić aplikację i zaznaczenie **nowy** z **pliku** menu zostaną otwarte i wyświetlone nowe okno edytora:

[![](window-images/display04.png "Dodano nowe okno bez tytułu")](window-images/display04.png#lightbox)

Jeśli możemy otworzyć **Windows** menu widać automatycznie śledzenia i naszych okien obsługi aplikacji:

[![](window-images/display05.png "Widows menu")](window-images/display05.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z menu w aplikacji Xamarin.Mac, zobacz nasze [Praca z menu](~/mac/user-interface/menu.md) dokumentacji.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>Pobieranie aktualnie aktywne okno

W aplikacji Xamarin.Mac, który można otworzyć wiele okien (dokumenty) są czasami będą potrzebne można pobrać bieżącego, znajdujących się na górze okna (okno klucza). Poniższy kod zwróci okna klucza:

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

Może zostać wywołany w dowolnej klasy lub metody, która musi uzyskać dostęp do okna bieżącej, klucza. Jeśli nie jest okno aktualnie otwarte, będzie zwracać `null`.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>Uzyskiwanie dostępu do wszystkich aplikacji systemu Windows

Mogą wystąpić razy wymagających dostępu do wszystkich systemu windows, że aplikacja Xamarin.Mac ma obecnie otwarty. Na przykład aby sprawdzić, czy użytkownik chce otworzyć plik jest już otwarty w istniejącym okna.

`NSApplication.SharedApplication` Przechowuje `Windows` właściwość, która zawiera tablicę wszystkie otwarte okna w aplikacji. Można iteracja tej tablicy, aby uzyskać dostęp do wszystkich aplikacji bieżącego systemu Windows. Na przykład:

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

W przykładowym kodzie możemy rzutowanie każdego zwróconego okna niestandardowe `ViewController` klasy w aplikacji i wartość niestandardowego testowania `Path` właściwości do ścieżki pliku użytkownik chce otworzyć. Jeśli plik jest już otwarty, możemy są dostarczają tego okna na pierwszym planie.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>Dostosowywanie rozmiaru okna w kodzie

Brak razy, gdy aplikacja potrzebuje zmiany rozmiaru okna w kodzie. Aby rozmiar i położenie okna, można dostosować w `Frame` właściwości. Zmieniając rozmiar okna, zazwyczaj należy również dostosować jego źródła, aby zachować okna w tej samej lokalizacji z powodu jego macOS współrzędnych.

W przeciwieństwie do systemu iOS, której lewy górny róg reprezentuje (0,0) system macOS używa mathematic współrzędnych gdzie dolny róg po lewej stronie ekranu reprezentuje (0,0). W systemie iOS współrzędne zwiększa przesuwana do prawej. Współrzędne w macOS, zwiększyć wartość do góry po prawej. 

Poniższy przykładowy kod zmienia rozmiar okna:

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> Podczas dostosowywania systemu windows rozmiar i położenie w kodzie, musisz upewnij się, że przestrzegać minimalny i maksymalny rozmiar ustawionych w Konstruktorze interfejsu. To nie automatycznie honorowane i będzie można wyświetlić okno większy lub mniejszy od tych ograniczeń.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>Monitorowanie zmian rozmiaru okna

Mogą wystąpić razy gdzie należy monitorować zmiany w rozmiarze okna wewnątrz aplikacji Xamarin.Mac. Na przykład, aby odświeżyć zawartość do nowego rozmiaru.

Aby monitorować zmiany rozmiaru, należy najpierw upewnić się, czy użytkownik przypisał niestandardowej klasy kontrolera okna w Konstruktorze interfejsu w środowisku Xcode. Na przykład `MasterWindowController` poniżej:

[![](window-images/resize01.png "Inspektor tożsamości")](window-images/resize01.png#lightbox)

Następnie edytuj niestandardowe klasy okna kontrolera i monitor `DidResize` zdarzeń w oknie kontrolera, aby otrzymywać powiadomienia o zmianach bieżący rozmiar. Na przykład:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

Opcjonalnie można użyć `DidEndLiveResize` zdarzeń, aby tylko otrzymywać powiadomienia, gdy użytkownik zakończy zmianę rozmiaru okna. Na przykład:

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

## <a name="setting-a-windows-title-and-represented-file"></a>Ustawienie okna jego tytuł i reprezentowany pliku

Podczas pracy z systemem windows, które reprezentują dokumenty, `NSWindow` ma `DocumentEdited` właściwości Jeśli ustawiono `true` Wyświetla małej kropki w przycisk Zamknij, aby przyznać użytkownikowi wskazanie, że plik został zmodyfikowany i powinien zostać zapisany przed zamknięciem.

Umożliwia edytowanie naszych `ViewController.cs` plik, a następnie dokonaj następujących zmian:

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

Możemy też monitorowanie `WillClose` zdarzenia w oknie i sprawdzanie stanu `DocumentEdited` właściwości. Jeśli jest `true` musimy umożliwiają użytkownikowi zapisać zmiany w pliku. Jeśli firma Microsoft uruchomienia aplikacji, wprowadź tekst, zostanie wyświetlony kropki (.):

[![](window-images/file01.png "Zmienione okna")](window-images/file01.png#lightbox)

Spróbujemy zamknąć okno, uzyskujemy alertu:

[![](window-images/file02.png "Zapisz wyświetlanie okna dialogowego")](window-images/file02.png#lightbox)

Firma Microsoft ładowania dokumentu z pliku możemy ustawić tytuł okna w pliku nazwy przy użyciu `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` — metoda (zakładając, że `path` jest ciąg reprezentujący otwierany plik). Ponadto można ustawić adres URL pliku przy użyciu `window.RepresentedUrl = url;` metody.

Jeśli adres URL wskazuje typ pliku znane przez system operacyjny, pojawi się jego ikonę na pasku tytułu. Gdy użytkownik kliknie prawym przyciskiem myszy ikonę, wyświetlany jest ścieżka do pliku.

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

Teraz możemy uruchomienia aplikacji, wybierz **Otwórz...**  z **pliku** menu, wybierz plik tekstowy z **Otwórz** okno dialogowe i otwórz go:

[![](window-images/file03.png "Otwarte okno dialogowe")](window-images/file03.png#lightbox)

Plik, który będzie wyświetlany i tytuł zostanie ustawiony z ikoną pliku:

[![](window-images/file04.png "Zawartość pliku załadowany")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>Dodawanie nowego okna do projektu

Jako uzupełnienie okna głównego dokumentu aplikacji Xamarin.Mac może być konieczne do wyświetlania innych typów systemu windows użytkownika, takie jak preferencje lub Inspektora paneli.

Aby dodać nowe okno, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu w środowisku Xcode.
2. Przeciągnij nowy **kontrolera okna** z **biblioteki** i upuść ją na **powierzchni projektowej**:

    [![](window-images/new01.png "Wybiera nowy kontroler okna w bibliotece")](window-images/new01.png#lightbox)
3. W **inspektora tożsamości**, wprowadź `PreferencesWindow` dla **identyfikator scenorysu**: 

    [![](window-images/new02.png "Ustawienie Identyfikatora scenorysu")](window-images/new02.png#lightbox)
5. Projekt interfejsu: 

    [![](window-images/new03.png "Projektowanie interfejsu użytkownika")](window-images/new03.png#lightbox)
6. Otwórz Menu aplikacji (`MacWindows`), wybierz pozycję **Preferencje...** , Przytrzymując klawisz CTRL i przeciągnij, aby nowe okno: 

    [![](window-images/new05.png "Tworzenie segue")](window-images/new05.png#lightbox)
7. Wybierz **Pokaż** w menu podręcznym.
6. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Jeśli firma Microsoft wykonywania kodu i wybierz **Preferencje...**  z **Menu aplikacja**, wyświetli się okno:

[![](window-images/new04.png "Menu Preferencje próbki")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>Praca z paneli

Jak już wspomniano w chwili rozpoczęcia tego artykułu, panelu pojawia się powyżej innych oknach i udostępnia narzędzia lub kontrolki, które użytkownicy mogą pracować z, gdy są otwarte dokumenty. 

Podobnie jak okna Tworzenie i pracować z aplikacji Xamarin.Mac innego typu proces jest zasadniczo taki sam:

1. Dodaj nową definicję okna do projektu.
2. Kliknij dwukrotnie `.xib` plik, aby otworzyć projekt okna do edycji w Konstruktorze interfejsu w środowisku Xcode.
2. Ustaw właściwości wymagane okna **inspektora atrybutu** i **inspektora rozmiar**.
4. Przeciągnij formanty wymagane do tworzenia interfejsu i skonfigurować je w **inspektora atrybutu**.
5. Użyj **inspektora rozmiar** do obsługi zmiana rozmiaru dla elementów interfejsu użytkownika.
6. Udostępnianie okna elementy interfejsu użytkownika do kodu C# za pomocą **gniazda** i **akcje**.
7. Zapisz zmiany i wrócić do programu Visual Studio for Mac synchronizację w programie Xcode.

W **inspektora atrybutu**, masz następujące opcje specyficzne dla paneli:

[![](window-images/panel03.png "Inspektor atrybutu")](window-images/panel03.png#lightbox)

- **Styl** — umożliwiają dostosowanie styl panelu z: regularne panelu (prawdopodobnie standardowe okno), narzędzia panelu (ma mniejszy paska tytułu), HUD Panel (jest przezroczysta i na pasku tytułu jest częścią tła).
- **Uaktywnianie z systemem innym niż** — Określa, w panelu staje się oknem klucza.
- **Dokument modalne** — Jeśli dokument modalne, panelu tylko przesunie się nad aplikacji systemu windows, przeciwnym razie jest ona wyświetlana przede wszystkim.


Aby dodać nowy Panel, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy plik.** .
2. W oknie dialogowym Nowy plik, wybierz **Xamarin.Mac** > **Cocoa okna z kontrolera**:

    [![](window-images/panels00.png "Dodawanie nowego kontrolera okna")](window-images/panels00.png#lightbox)
3. Wprowadź `DocumentPanel` dla **nazwa** i kliknij przycisk **nowy** przycisku.
4. Kliknij dwukrotnie `DocumentPanel.xib` plik, aby otworzyć do edycji w Konstruktorze interfejsu: 

    [![](window-images/new02.png "Edytowanie pannel")](window-images/new02.png#lightbox)
5. Usuń istniejące okno i przeciągnij Panel z **inspektora biblioteki** w **Edytor interfejsu**: 

    [![](window-images/panels01.png "Usuwanie istniejącego okna")](window-images/panels01.png#lightbox)
6. Utworzenie punktu zaczepienia panelu do **właścicielem pliku*-**okna*- **gniazda**: 

    [![](window-images/panels02.png "Przeciąganie można przewodowo zapasowej panelu")](window-images/panels02.png#lightbox)
7. Przełącz się do **inspektora tożsamości** i ustaw klasy panelu `DocumentPanel`: 

    [![](window-images/panels03.png "Ustawienie panelu — klasa")](window-images/panels03.png#lightbox)
6. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.
7. Edytuj `DocumentPanel.cs` plików i zmień definicję klasy do następującego: 

    `public partial class DocumentPanel : NSPanel`
8. Zapisz zmiany w pliku.

Edytuj `AppDelegate.cs` plików i upewnij `DidFinishLaunching` wygląd metody podobne do poniższych:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

Czy możemy uruchomić aplikację, zostanie wyświetlony panelu:

[![](window-images/panels04.png "Panel w uruchomionej aplikacji")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> Panel systemu Windows została zastąpiona przez firmę Apple i powinna zostać zastąpiona **interfejsy inspektora**. Pełny przykład tworzenia **inspektora** w aplikacji Xamarin.Mac, zobacz nasze [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) przykładowej aplikacji.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowy widok w pracy z systemem Windows i panele aplikacją Xamarin.Mac. Firma Microsoft przedstawiono różne typy i korzysta z systemu Windows i panele, tworzenie i obsługa systemu Windows i panele w Konstruktorze interfejsu w środowisku Xcode i sposobu pracy z systemem Windows i panele w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacWindows (przykład)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (przykład)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Praca z menu](~/mac/user-interface/menu.md)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do systemu Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
