---
title: Menu platformy Xamarin.Mac
description: W tym artykule opisano Praca z menu w aplikacji platformy Xamarin.Mac. Opisano w nim tworzenia i utrzymywania menu i elementów menu w programie Xcode i programu Interface Builder oraz pracy z nimi programistycznie.
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d84910cd5c2bc72a563fb04457532d544aedf571
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780656"
---
# <a name="menus-in-xamarinmac"></a>Menu platformy Xamarin.Mac

_W tym artykule opisano Praca z menu w aplikacji platformy Xamarin.Mac. Opisano w nim tworzenia i utrzymywania menu i elementów menu w programie Xcode i programu Interface Builder oraz pracy z nimi programistycznie._

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tego samego menu Cocoa, które jest deweloperem, pracy w językach Objective-C i Xcode. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, służy program Xcode Interface Builder do tworzenia i obsługi paski menu, menu i elementy menu (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Menu są integralną częścią środowiska użytkownika aplikacji dla komputerów Mac i często pojawiają się w różnych częściach interfejsu użytkownika:

- **Pasek menu aplikacji** — jest to menu głównego, który pojawia się w górnej części ekranu dla każdej aplikacji dla komputerów Mac.
- **Menu kontekstowe** — te są wyświetlane, gdy użytkownik kliknie prawym przyciskiem myszy lub kontroli kliknięć element w oknie.
- **Na pasku stanu** — jest to obszar, z prawej strony paska menu aplikacja, jest wyświetlana w górnej części ekranu (na lewo od zegara paska menu), która zwiększa się po lewej stronie, jak elementy są dodawane do niego.
- **Dokowanie menu** — menu dla każdej aplikacji, w obszarze dock wyświetlany po użytkownik kliknie prawym przyciskiem myszy lub kontroli kliknięcia ikony aplikacji lub po użytkownik lewym przyciskiem myszy ikonę i przechowuje przycisku myszy w dół.
- **Przycisk podręczne i listy rozwijane** — przycisk wyskakującego Wyświetla wybrany element i wyświetla listę opcji, które można wybierać po kliknięciu przez użytkownika. Listy rozwijane jest rodzajem przycisku wyskakujące, zwykle służy do wybierania poleceń specyficznych dla kontekstu bieżącego zadania. Jednocześnie może występować w dowolnym miejscu w oknie.

[![Menu przykład](menu-images/intro01.png "menu przykład")](menu-images/intro01-large.png#lightbox)

W tym artykule omówimy podstawy pracy z paski menu Cocoa, menu i elementów menu w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` atrybutów Umożliwia o komunikacji sieciowej w górę klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

## <a name="the-applications-menu-bar"></a>Pasek menu aplikacji 

W odróżnieniu od działających w systemie operacyjnym Windows, gdzie każdego okna może mieć własną paska menu, dołączono do niego każda aplikacja uruchomiona w systemie macOS ma pasek pojedynczym menu, który uruchamia wzdłuż górnej części ekranu, który jest używany dla każdego okna w tej aplikacji:

[![Pasek menu](menu-images/appmenu01.png "pasek menu")](menu-images/appmenu01-large.png#lightbox)

Elementy na tym pasku menu są aktywowane lub dezaktywowane na podstawie bieżącego kontekstu lub stan aplikacji oraz interfejs użytkownika w danej chwili. Na przykład: Jeśli użytkownik wybierze pole tekstowe, elementy na **Edytuj** menu będą pochodzić włączone, takie jak **kopiowania** i **Wytnij**.

Zgodnie z firmy Apple i domyślnie wszystkie aplikacje z systemem macOS mają standardowy zestaw menu i elementów menu, które pojawiają się na pasku menu aplikacji:

- **Apple menu** — w tym menu zapewnia dostęp do systemu szerokiego elementy, które są dostępne dla użytkownika przez cały czas, niezależnie od tego, w jaki aplikacja jest uruchomiona. Nie można zmodyfikować te elementy przez dewelopera.
- **Menu aplikacji** — to menu wyświetla nazwę aplikacji wytłuszczonym drukiem i pomaga użytkownikowi określić, jakie aplikacja jest obecnie uruchomiona. Zawiera on elementy, które są stosowane do aplikacji jako całości, a nie danego dokumentu lub proces, taki jak zamykanie aplikacji.
- **Menu Plik** — elementy używane do tworzenia, otwieranie lub zapisywanie dokumentów, które Twoja aplikacja działa przy użyciu. Jeśli aplikacja jest oparta na dokumentach, to menu można zmieniona lub usunięta.
- **Menu Edycja** — zawiera polecenia, takie jak **Wytnij**, **kopiowania**, i **Wklej** które są używane do edycji lub modyfikować elementy w interfejsie użytkownika aplikacji.
- **Format menu** — Jeśli aplikacja działa z tekstem, to menu blokad polecenia, aby dostosować formatowanie tekstu.
- **Menu Widok** — zawiera polecenia, które mają wpływ na sposób wyświetlania zawartości (wyświetlane) w interfejsie użytkownika aplikacji.
- **Menu specyficzne dla aplikacji** — są to wszystkie menu, które są specyficzne dla aplikacji (na przykład menu zakładek przeglądarki sieci web). Powinny one zostać wyświetlone między **widoku** i **okna** menu na pasku.
- **Menu okna** — zawiera polecenia do pracy z systemem windows w aplikacji, a także listę bieżącego okna.
- **Menu Pomoc** — Jeśli aplikacja udostępnia pomoc na ekranie, w menu Pomoc powinny być na pasku menu najdalej z prawej strony. 

Aby uzyskać więcej informacji o aplikacji pasek menu i menu standardowe oraz elementy menu, zobacz firmy Apple [Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/).

### <a name="the-default-application-menu-bar"></a>Pasek menu domyślne aplikacji

Gdy utworzysz nowy projekt platformy Xamarin.Mac, automatycznie uzyskują w warstwie standardowa domyślnej aplikacji pasek menu, który ma typowe elementy, które aplikacji z systemem macOS mają (jak opisano w powyższej sekcji). Pasek menu domyślne Twojej aplikacji jest zdefiniowany w **Main.storyboard** pliku (wraz z pozostałej części Interfejsie użytkownika aplikacji) w projekcie w **konsoli rozwiązania**:  

![Wybierz głównego storyboard](menu-images/appmenu02.png "wybierz głównego storyboard")

Kliknij dwukrotnie **Main.storyboard** plik, aby go otworzyć do edycji w program Xcode Interface Builder i zostanie wyświetlony interfejs Edytora menu:

[![Edytowanie interfejsu użytkownika w środowisku Xcode](menu-images/defaultbar01.png "edytowania interfejsu użytkownika w środowisku Xcode")](menu-images/defaultbar01-large.png#lightbox)

W tym miejscu możemy kliknąć elementy takie jak **Otwórz** elementu menu w **pliku** menu edytować i dostosować jego właściwości w **Inspektor atrybuty**:

[![Edycja atrybutów menu](menu-images/defaultbar02.png "Edycja atrybutów menu")](menu-images/defaultbar02-large.png#lightbox)

Przejdziemy do Dodawanie, edytowanie i usuwanie menu i elementy w dalszej części tego artykułu. Dla teraz po prostu chcesz zobaczyć, jakie elementy menu i menu są domyślnie dostępne i jak mogą być automatycznie narażone na kod za pomocą zestawu akcji i gniazd wstępnie zdefiniowane (Aby uzyskać więcej informacji, zobacz nasze [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) dokumentacja).

Na przykład po kliknięciu na **Inspektor połączenia** dla **Otwórz** elementu menu, okaże się, jest automatycznie przewodowej maksymalnie `openDocument:` akcji: 

[![Wyświetlanie akcji dołączone](menu-images/defaultbar03.png "wyświetlanie akcji dołączone")](menu-images/defaultbar03-large.png#lightbox)

Jeśli wybierzesz **pierwszy obiekt odpowiadający w trybie** w **hierarchii interfejsów** i przewiń w dół **Inspektor połączenia**, i zobaczysz definicję `openDocument:` Akcja, **Otwórz** element menu jest dołączony do (oraz kilka innych domyślnej akcji dla aplikacji i nie są automatycznie powiązaną formantów):

[![Wyświetlanie działań wszystkich dołączonych](menu-images/defaultbar04.png "wyświetlanie wszystkich akcji dołączone")](menu-images/defaultbar04-large.png#lightbox) 

Dlaczego jest to istotne? W następnej sekcji zostanie wyświetlony, jak działają te akcje automatycznie definiowane za pomocą innych elementach interfejsu użytkownika Cocoa, aby automatycznie Włącz i Wyłącz elementy menu, a także, zapewnia wbudowanej funkcjonalności dla elementów.

Później będziemy używać tych wbudowanych akcji. Aby włączyć i wyłączyć elementy z kodu i podać własną funkcjonalność, gdy są zaznaczone.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>Funkcje wbudowane menu

Gdyby Uruchom nowo utworzonej aplikacji rozszerzenia Xamarin.Mac przed dodaniem elementy interfejsu użytkownika lub kodu, można zauważyć, że niektóre elementy są automatycznie przewodowej w górę i włączona (z w pełni funkcje automatycznie wbudowanych), takie jak **Zakończ** pozycja **aplikacji** menu:

![Element menu włączone](menu-images/appmenu03.png "elementu menu włączone")

Podczas gdy inne elementy menu, takich jak **Wytnij**, **kopiowania**, i **Wklej** nie są:

![Wyłączyć elementy menu](menu-images/appmenu04.png "wyłączone elementów menu")

Spróbujmy Zatrzymaj aplikację i kliknij dwukrotnie **Main.storyboard** w pliku **konsoli rozwiązania** aby go otworzyć do edycji w programie Xcode zachowania programu Interface Builder. Następnie przeciągnij **widoku tekstu** z **biblioteki** na kontroler widoku okna w **Edytor interfejsu**:

[![Wybieranie widoku tekstu z biblioteki](menu-images/appmenu05.png "wybierając Widok tekstu z biblioteki")](menu-images/appmenu05-large.png#lightbox)

W **edytora ograniczeń** umożliwia przypinanie widoku tekstu do krawędzi okna i ustaw dla niej gdzie wraz ze wzrostem i zmniejsza w zależności od okna, klikając pozycję wszystkie cztery czerwoną I świateł w górnej części edytora i klikając **Dodaj ograniczenia 4** przycisku:

[![Edytowanie ograniczenia](menu-images/appmenu06.png "edytowania ograniczenia")](menu-images/appmenu06-large.png#lightbox)

Zapisz zmiany projektu interfejsu użytkownika, a następnie przełączenie powrotne programu Visual Studio dla komputerów Mac zsynchronizować zmiany z projektu platformy Xamarin.Mac. Teraz uruchom aplikację, wpisz dowolny tekst w widoku tekstu, zaznacz ją i Otwórz **Edytuj** menu:

![Elementy menu są automatycznie włączone/wyłączone](menu-images/appmenu07.png "elementy menu są automatycznie włączone/wyłączone")

Zwróć uwagę sposób, w jaki **Wytnij**, **kopiowania**, i **Wklej** elementy są automatycznie włączone i w pełni funkcjonalne, wszystko to bez konieczności napisania choćby jednego wiersza kodu. 

Co się dzieje w tym miejscu? Należy pamiętać, wbudowane wstępnego zdefiniowania akcji, które pochodzą przewodowej maksymalnie domyślne elementy menu (przedstawionych powyżej), większość Cocoa elementy interfejsu użytkownika, które są częścią z systemem macOS mają wbudowane punkty zaczepienia z określonymi akcjami (takie jak `copy:`). Dlatego gdy są dodawane do okna, aktywne i zaznaczone, odpowiedni element menu lub elementów dołączonych do tej akcji są włączane automatycznie. Jeśli użytkownik wybierze tę pozycję menu, funkcji wbudowanych w element interfejsu użytkownika jest o nazwie i wykonywane, wszystko to bez konieczności interwencji dla deweloperów.

### <a name="enabling-and-disabling-menus-and-items"></a>Włączanie i wyłączanie menu i elementów

Domyślnie za każdym razem, gdy wystąpi zdarzenie użytkownika `NSMenu` automatycznie włącza i wyłącza każdego widoczne menu i menu elementów na podstawie kontekstu aplikacji. Istnieją trzy sposoby, aby włączyć/wyłączyć element:

- **Włączanie automatycznego menu** — element menu jest włączona, jeśli `NSMenu` można znaleźć odpowiedni obiekt, który odpowiada akcji, który element jest przewodowej w usłudze. Na przykład tekst powyższy widok zawierającego wbudowanych zaczepienia `copy:` akcji.
- **Akcje niestandardowe i validateMenuItem:** — dla każdego elementu menu, który jest powiązany z [okna lub Widok akcji niestandardowej kontrolera](#Working-with-Custom-Window-Actions), możesz dodać `validateMenuItem:` akcji i ręcznie Włącz lub Wyłącz elementy menu.
- **Włączanie menu ręczne** — ręcznie ustawić `Enabled` właściwości każdego `NSMenuItem` można włączać lub wyłączać indywidualnie każdy element w menu.

Aby wybrać system, należy ustawić `AutoEnablesItems` właściwość `NSMenu`. `true` odbywa się automatycznie (domyślnie) i `false` jest ręcznie. 

> [!IMPORTANT]
> Jeśli chcesz użyć menu Ręczne włączanie menu pozycji typu Brak, nawet te, które w wartości clientauthtrustmode AppKit klasy, takie jak `NSTextView`, są automatycznie aktualizowane. Będzie odpowiedzialny za włączanie i wyłączanie wszystkich elementów ręcznie w kodzie.

#### <a name="using-validatemenuitem"></a>Za pomocą validateMenuItem

Jak wspomniano powyżej, dla każdego elementu menu, który jest powiązany z [okna lub Akcja niestandardowa kontrolera widoku](#Working-with-Custom-Window-Actions), możesz dodać `validateMenuItem:` akcji i ręcznie Włącz lub Wyłącz elementy menu.

W poniższym przykładzie `Tag` podjęcie decyzji, typ elementu menu, które będą mieć włączone/wyłączone, zostanie użyta właściwość `validateMenuItem:` akcji na podstawie stanu zaznaczonego tekstu w `NSTextView`. `Tag` Ustawiono właściwości w programu Interface Builder dla każdego elementu menu:

![Ustawianie właściwości Tag](menu-images/validate01.png "ustawienie właściwości Tag")

I następującego kodu, które są dodawane do kontrolera widoku:

```csharp
[Action("validateMenuItem:")]
public bool ValidateMenuItem (NSMenuItem item) {

    // Take action based on the menu item type
    // (As specified in its Tag)
    switch (item.Tag) {
    case 1:
        // Wrap menu items should only be available if
        // a range of text is selected
        return (TextEditor.SelectedRange.Length > 0);
    case 2:
        // Quote menu items should only be available if
        // a range is NOT selected.
        return (TextEditor.SelectedRange.Length == 0);
    }

    return true;
}
```

Kiedy ten kod jest uruchamiany i zaznaczono żadnego tekstu w `NSTextView`, zawijania dwa elementy menu są wyłączone (nawet jeśli są one podłączone do akcji kontrolera widoku):

![Elementy wyłączone przedstawiający](menu-images/validate02.png "przedstawiający elementy wyłączone")

Jeśli wybrano fragment tekstu, a następnie ponownie otworzyć menu, elementy menu dwóch zawijania będą dostępne:

![Elementy przedstawiający włączone](menu-images/validate03.png "przedstawiający włączone elementów")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>Włączanie i reagowanie na elementów menu w kodzie

Jak skrócił się powyżej, po prostu, dodając określone elementy interfejsu użytkownika Cocoa do naszego projektu interfejsu użytkownika (na przykład pole tekstowe), kilka elementów menu domyślne zostanie włączona i działa automatycznie, bez konieczności pisania kodu. Dalej Przyjrzyjmy się dodanie swój własny kod C# do projektu naszej platformy Xamarin.Mac w taki sposób, aby włączyć element menu i udostępniają funkcje, gdy użytkownik wybierze je.

Na przykład, załóżmy chcemy, aby użytkownik będzie mógł używać **Otwórz** pozycja **pliku** menu, aby wybrać folder. Ponieważ chcemy, żeby było to funkcja całej aplikacji, a nie tylko zapewniają okna lub elementu interfejsu użytkownika, którą zamierzamy dodać kod do obsługi tego delegata naszej aplikacji.

W **konsoli rozwiązania**, kliknij dwukrotnie `AppDelegate.CS` plik, aby otworzyć do edycji:

![Wybieranie delegata aplikacji](menu-images/appmenu08.png "wybierając delegata aplikacji")

Dodaj następujący kod poniżej `DidFinishLaunching` metody:

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

Teraz uruchom aplikację teraz i Otwórz **pliku** menu: 

![Menu Plik](menu-images/appmenu09.png "menu Plik")

Należy zauważyć, że **Otwórz** element menu jest teraz włączony. Jeśli wybieramy ją pojawi się okno dialogowe Otwórz:

![Otwórz okno dialogowe](menu-images/appmenu10.png "otwarte okna dialogowe")

Po kliknięciu **Otwórz** przycisku, zostanie wyświetlony komunikat z naszych alertu:

![Przykładowy komunikat okna dialogowego](menu-images/appmenu11.png "przykładowy komunikat okna dialogowego")

Tutaj wiersz klucza została `[Export ("openDocument:")]`, informuje `NSMenu` , nasze **elemencie AppDelegate** ma metodę `void OpenDialog (NSObject sender)` który odpowiada na `openDocument:` akcji. Jeśli zapamiętasz z powyższych **Otwórz** element menu jest automatycznie przewodowej w usłudze tej akcji w programu Interface Builder domyślnie:

[![Wyświetlanie akcji dołączone](menu-images/defaultbar03.png "wyświetlanie akcji dołączone")](menu-images/defaultbar03-large.png#lightbox)

Dalej Przyjrzyjmy się reagowanie i tworzenie własnych menu, elementy menu i działań do nich w kodzie.

### <a name="working-with-the-open-recent-menu"></a>Praca z otwartym menu ostatnie

Domyślnie **pliku** menu zawiera **Otwórz ostatni** elementu, który śledzi kilka plików, otwierane z aplikacją. Jeśli tworzysz `NSDocument` na podstawie aplikacji rozszerzenia Xamarin.Mac, to menu będzie obsługiwane dla Ciebie automatycznie. Dla aplikacji rozszerzenia Xamarin.Mac innego typu będzie odpowiedzialny za zarządzanie i reagowanie na ten element menu ręcznie.

Aby ręcznie śledzić **Otwórz ostatni** menu, należy najpierw go poinformować, że nowy plik został otwarty lub zapisać za pomocą następujących:

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

Mimo że aplikacja nie używa `NSDocuments`, nadal używać `NSDocumentController` do utrzymania **Otwórz ostatni** menu, wysyłając `NSUrl` lokalizację pliku do `NoteNewRecentDocumentURL` metody `SharedDocumentController`.

Następnie należy zastąpić `OpenFile` metody delegata aplikacji, aby otworzyć dowolny plik, który użytkownik wybiera z **Otwórz ostatnio** menu. Na przykład:

```csharp
public override bool OpenFile (NSApplication sender, string filename)
{
    // Trap all errors
    try {
        filename = filename.Replace (" ", "%20");
        var url = new NSUrl ("file://"+filename);
        return OpenFile(url);
    } catch {
        return false;
    }
}
```

Zwróć `true` Jeśli plik może być otwarty, inne zwrócić `false` wbudowanych ostrzeżenie pojawi się do użytkownika, że nie można otworzyć pliku.

Ponieważ nazwa pliku i ścieżka zwróciło **Otwórz ostatni** menu może zawierać spacji, musimy prawidłowo ten znak ucieczki przed utworzeniem `NSUrl` lub firma Microsoft otrzymają komunikat o błędzie. Czynność tę możemy wykonać przy użyciu następującego kodu:

```csharp
filename = filename.Replace (" ", "%20");
```

Na koniec utworzymy `NSUrl` wskazującego na plik i użyj metody pomocnika w aplikacji delegować Otwórz nowe okno i ładowania pliku:

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

Ściągać wszystko ze sobą, Spójrzmy na przykład implementacji w **AppDelegate.cs** pliku:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace MacHyperlink
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }

        public override bool OpenFile (NSApplication sender, string filename)
        {
            // Trap all errors
            try {
                filename = filename.Replace (" ", "%20");
                var url = new NSUrl ("file://"+filename);
                return OpenFile(url);
            } catch {
                return false;
            }
        }
        #endregion

        #region Private Methods
        private bool OpenFile(NSUrl url) {
            var good = false;

            // Trap all errors
            try {
                var path = url.Path;

                // Is the file already open?
                for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
                    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
                    if (content != null && path == content.FilePath) {
                        // Bring window to front
                        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
                        return true;
                    }
                }

                // Get new window
                var storyboard = NSStoryboard.FromName ("Main", null);
                var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

                // Display
                controller.ShowWindow(this);

                // Load the text into the window
                var viewController = controller.Window.ContentViewController as ViewController;
                viewController.Text = File.ReadAllText(path);
                viewController.SetLanguageFromPath(path);
                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                viewController.View.Window.RepresentedUrl = url;

                // Add document to the Open Recent menu
                NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);

                // Make as successful
                good = true;
            } catch {
                // Mark as bad file on error
                good = false;
            }

            // Return results
            return good;
        }
        #endregion

        #region actions
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
                    // Open the document in a new window
                    OpenFile (url);
                }
            }
        }
        #endregion
    }
}
```

Na podstawie wymagań aplikacji, nie można użytkownika, aby otworzyć ten sam plik w więcej niż jedno okno, w tym samym czasie. W naszym przykładową aplikację, jeśli użytkownik wybierze otwartego pliku (z **Otwórz ostatnio** lub **Otwórz...** elementy menu), okno, który zawiera plik przekazywane są do przodu.

Aby to osiągnąć, użyliśmy następujący kod w naszym metody pomocniczej:

```csharp
var path = url.Path;

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

Został zaprojektowany naszych `ViewController` klasy utrzymującej ścieżkę do pliku w jego `Path` właściwości. Następnie możemy w pętli poprzez wszystkie aktualnie otwarte okna w aplikacji. Jeśli plik jest otwarty w jednym z systemu windows, wprowadzeniem ich do przodu wszystkich okien przy użyciu:

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

Jeśli nie zostanie znalezione dopasowanie, nowe okno jest otwierany przy użyciu pliku załadowane, a plik jest zapisany w **Otwórz ostatni** menu:

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);

// Load the text into the window
var viewController = controller.Window.ContentViewController as ViewController;
viewController.Text = File.ReadAllText(path);
viewController.SetLanguageFromPath(path);
viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
viewController.View.Window.RepresentedUrl = url;

// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

<a name="Working-with-Custom-Window-Actions" />

### <a name="working-with-custom-window-actions"></a>Praca z akcje niestandardowe okno

Podobnie jak w przypadku wbudowanych **pierwszy obiekt odpowiadający w trybie** akcje, które zaczynają wstępnie przewodowej elementów menu standardowego, możesz utworzyć nowe, niestandardowe akcje i powiązać je z elementami menu w programu Interface Builder.

Najpierw zdefiniuj akcję niestandardową na jednym z kontrolerów okna aplikacji. Na przykład:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

Następnie kliknij dwukrotnie plik scenorysu aplikacji w **konsoli rozwiązania** aby go otworzyć do edycji w programie Xcode zachowania programu Interface Builder. Wybierz **pierwszy obiekt odpowiadający w trybie** w obszarze **sceny aplikacji**, przełącz się do **Inspektor atrybuty**:

![Inspektor atrybuty](menu-images/action01.png "Inspektor atrybutów")

Kliknij przycisk **+** znajdujący się u dołu **Inspektor atrybuty** można dodać nowej akcji niestandardowej:

![Dodawanie nowej akcji](menu-images/action02.png "Dodawanie nowej akcji")

Nadaj mu taką samą nazwę jak akcja niestandardowa, który został utworzony na kontrolerze okna:

![Edytowanie nazwy akcji](menu-images/action03.png "edytowanie nazwy akcji")

Klawisz Control, kliknij i przeciągnij od elementu menu, aby **pierwszy obiekt odpowiadający w trybie** w obszarze **sceny aplikacji**. Na liście podręcznym zaznacz nową akcję właśnie utworzony (`defineKeyword:` w tym przykładzie):

![Dołączanie akcję](menu-images/action04.png "dołączanie akcję")

Zapisz zmiany do scenorysu i wróć do programu Visual Studio dla komputerów Mac zsynchronizować zmiany. Po uruchomieniu aplikacji, element menu, która połączona Akcja niestandardowa kończy się zostanie automatycznie być włączone/wyłączone (na podstawie okna przy użyciu akcji jest otwierany) i wybierając element menu będzie wyzwolić akcję:

[![Testowanie nowej akcji](menu-images/action05.png "testowanie nowej akcji")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>Dodawanie, edytowanie i usuwanie menu

Jak zauważono w poprzednich sekcjach, aplikacja platformy Xamarin.Mac zawiera szereg wstępnie zdefiniowanych domyślnych menu i elementy menu, które określonych kontrolek interfejsu użytkownika automatycznie zostanie uaktywniony i reagowanie na. Zaobserwowaliśmy również sposób dodawania kodu do naszej aplikacji, która będzie również włączyć i odpowiadać na następujące domyślne elementy.

W tej sekcji omówimy usunięcie elementów menu, które nie są nam potrzebne, reorganizowanie menu i dodając nowe menu, elementy menu i akcji.

Kliknij dwukrotnie **Main.storyboard** w pliku **konsoli rozwiązania** aby go otworzyć do edycji:

[![Edytowanie interfejsu użytkownika w środowisku Xcode](menu-images/maint01.png "edytowania interfejsu użytkownika w środowisku Xcode")](menu-images/maint01-large.png#lightbox)

Dla określonych aplikacji platformy Xamarin.Mac nie zamierzamy się przy użyciu domyślnego **widoku** menu, więc użyjemy go usunąć. W **hierarchii interfejsów** wybierz **widoku** element menu, który jest częścią główny pasek menu:

![Wybieranie elementu menu widoku](menu-images/maint02.png "wybraniu elementu menu Widok")

Naciśnij klawisz delete lub backspace, aby usunąć menu. Następnie nie będziemy korzystać z wszystkich elementów w **Format** menu i chcesz przenieść elementy są użyjemy limit jest dostępna z podmenu. W **hierarchii interfejsów** wybierz menu z następującymi elementami:

![Wyróżnianie wielu elementów](menu-images/maint03.png "wyróżnianie wielu elementów")

Przeciągnij elementy podrzędne obiektu nadrzędnego **Menu** podmenu, gdy aktualnie są one:

[![Przeciąganie elementów menu do menu nadrzędnego](menu-images/maint04.png "przeciąganie elementów menu do menu nadrzędnego")](menu-images/maint04-large.png#lightbox)

Menu powinna teraz wyglądać podobnie jak:

[![Elementy w nowej lokalizacji](menu-images/maint05.png "elementów w nowej lokalizacji")](menu-images/maint05-large.png#lightbox)

Dalej przeciągnijmy **tekstu** podmenu się z pod **Format** menu i umieścić go na pasku menu głównego między **Format** i **okna** menu:

[![Z menu tekstowym](menu-images/maint06.png "tekst menu")](menu-images/maint06-large.png#lightbox)

Przejdź wstecz w obszarze **Format** menu i Usuń **czcionki** elementu podmenu. Następnie wybierz pozycję **Format** menu i zmień jego nazwę na "Czcionki":

[![Menu Czcionka](menu-images/maint07.png "menu Czcionka")](menu-images/maint07-large.png#lightbox)

Następnie utwórz niestandardowe menu predefine fraz, które automatycznie będą Pobierz dołączany do tekstu w widoku tekstu po jej wybraniu. W polu wyszukiwania w dolnej części okna **Inspektor biblioteki** typu w menu typu"." Ułatwi znajdowanie i pracować z wszystkich elementów interfejsu użytkownika w menu:

![Inspektor biblioteki](menu-images/maint08.png "Inspektor biblioteki")

Teraz wykonajmy następujące polecenie, aby utworzyć nasze menu:

1. Przeciągnij **element Menu** z **Inspektor biblioteki** na pasku menu między **tekstu** i **okna** menu: 

    ![Wybieranie nowego elementu menu w bibliotece](menu-images/maint10.png "wybierając nowy element menu w bibliotece")
2. Zmień nazwę elementu "Frazy": 

    [![Ustawianie nazwy menu](menu-images/maint09.png "Nazwa menu Ustawienia")](menu-images/maint09-large.png#lightbox)
3. Następnie przeciągnij **Menu** z **Inspektor biblioteki**: 

    ![Wybierając menu z biblioteki](menu-images/maint11.png "wybierając menu z biblioteki")
4. Następnie upuść **Menu** na nowym **element Menu** możemy po prostu utworzyć i zmienić jego nazwę na "Frazy": 

    [![Edytowanie nazwy menu](menu-images/maint12.png "edytowanie nazwy menu")](menu-images/maint12-large.png#lightbox)
5. Teraz Zmieńmy nazwy trzech domyślnych **elementów Menu** "Address", "Data" i "Pozdrowienia": 

    [![Menu fraz](menu-images/maint13.png "menu fraz")](menu-images/maint13-large.png#lightbox)
6. Dodajmy czwarty **element Menu** , przeciągając **element Menu** z **Inspektor biblioteki** i wywołując ją "Podpis": 

    [![Edytowanie nazwy elementu menu](menu-images/maint14.png "edytowanie nazwy elementu menu")](menu-images/maint14-large.png#lightbox)
7. Zapisz zmiany na pasku menu.

Teraz Utwórzmy działań niestandardowych, dzięki czemu nasze nowe elementy menu są widoczne dla kodu C#. W środowisku Xcode przejdźmy do **Asystenta** widoku:

[![Tworzenie wymaganych działań](menu-images/maint15.png "tworzenia wymaganych działań")](menu-images/maint15-large.png#lightbox)

Teraz należy wykonać następujące czynności:

1. Przeciągnij formant z **adres** element menu, aby **AppDelegate.h** pliku.
2. Przełącznik **połączenia** typ **akcji**: 

    [![Wybranie typu akcji](menu-images/maint17.png "wybranie typu akcji")](menu-images/maint17-large.png#lightbox)
3. Wprowadź **nazwa** "phraseAddress" i naciśnij klawisz **Connect** przycisk, aby utworzyć nową akcję: 

    [![Konfigurowanie akcji](menu-images/maint18.png "Konfigurowanie akcji")](menu-images/maint18-large.png#lightbox)
4. Powtórz powyższe kroki dla **data**, **pozdrowienia**, i **podpisu** elementy menu: 

    [![Ukończone akcje](menu-images/maint19.png "ukończonych akcji")](menu-images/maint19-large.png#lightbox)
5. Zapisz zmiany na pasku menu.

Dalej, musimy utworzyć gniazda dla naszych widoku tekstu, dzięki czemu będziemy mogli dostosować jego zawartość z kodu. Wybierz **ViewController.h** w pliku **edytora Asystenta** i utworzyć nowego punktu sprzedaży, o nazwie `documentText`:

[![Tworzenie ujścia](menu-images/maint20.png "Tworzenie gniazda")](menu-images/maint20-large.png#lightbox)

Wróć do programu Visual Studio dla komputerów Mac zsynchronizować zmiany z narzędzia Xcode. Następnie Edytuj **ViewController.cs** plików i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;

using AppKit;
using Foundation;

namespace MacMenus
{
    public partial class ViewController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }

        public string Text {
            get { return documentText.Value; }
            set { documentText.Value = value; }
        } 
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            App.textEditor = this;
        }

        public override void ViewWillDisappear ()
        {
            base.ViewDidDisappear ();

            App.textEditor = null;
        }
        #endregion
    }
}
```

Udostępnia to tekst naszych widoku tekstu poza `ViewController` klasy i informuje delegata aplikacji, gdy okno zyskuje lub traci fokus. Teraz edytować **AppDelegate.cs** plików i przypisz ją wyglądać podobnie do następującego:

```csharp
using AppKit;
using Foundation;
using System;

namespace MacMenus
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public ViewController textEditor { get; set;} = null;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
        #endregion

        #region Custom actions
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

        partial void phrasesAddress (Foundation.NSObject sender) {

            textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
        }

        partial void phrasesDate (Foundation.NSObject sender) {

            textEditor.Text += DateTime.Now.ToString("D");
        }

        partial void phrasesGreeting (Foundation.NSObject sender) {

            textEditor.Text += "Dear Sirs,\n\n";
        }

        partial void phrasesSignature (Foundation.NSObject sender) {

            textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
        }
        #endregion
    }
}
```

W tym miejscu wprowadziliśmy `AppDelegate` częściowe klasy, dzięki czemu możemy użyć akcji i gniazd, zdefiniowanych w programu Interface Builder. Możemy także ujawniać `textEditor` do śledzenia, które okno jest obecnie w trybie koncentracji uwagi.

Następujące metody są używane do obsługi naszych elementy menu i menu niestandardowe:

```csharp
partial void phrasesAddress (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
}

partial void phrasesDate (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += DateTime.Now.ToString("D");
}

partial void phrasesGreeting (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Dear Sirs,\n\n";
}

partial void phrasesSignature (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
}
```

Teraz, jeśli możemy uruchomić aplikację, wszystkie elementy w **frazy** menu zostanie uaktywniona i doda frazy zapewniają do widoku tekstu w przypadku wybrania:

![Przykładem jest uruchomiona aplikacja](menu-images/maint21.png "przykładem jest uruchomiona aplikacja")

Teraz, gdy mamy już podstawowe informacje dotyczące pracy z paska menu aplikacji w dół, Przyjrzyjmy się tworzenie niestandardowego menu kontekstowego.

### <a name="creating-menus-from-code"></a>Tworzenie menu z kodu

Oprócz tworzenia menu i elementów menu za pomocą program Xcode Interface Builder, może być czasy, kiedy aplikacji rozszerzenia Xamarin.Mac musi utworzyć, zmodyfikować lub usunąć menu, podmenu lub elementu menu z kodu.

W poniższym przykładzie utworzono klasę zawierającą informacje na temat elementów menu i podmenu, które zostaną dynamiczne utworzone na bieżąco:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace AppKit.TextKit.Formatter
{
    public class LanguageFormatCommand : NSObject
    {
        #region Computed Properties
        public string Title { get; set; } = "";
        public string Prefix { get; set; } = "";
        public string Postfix { get; set; } = "";
        public List<LanguageFormatCommand> SubCommands { get; set; } = new List<LanguageFormatCommand>();
        #endregion

        #region Constructors
        public LanguageFormatCommand () {

        }

        public LanguageFormatCommand (string title)
        {
            // Initialize
            this.Title = title;
        }

        public LanguageFormatCommand (string title, string prefix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
        }

        public LanguageFormatCommand (string title, string prefix, string postfix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
            this.Postfix = postfix;
        }
        #endregion
    }
}
```

#### <a name="adding-menus-and-items"></a>Dodawanie menu i elementów

Za pomocą tej klasy jest zdefiniowany, następujące procedury będzie analizować zbiór `LanguageFormatCommand`obiektów i rekursywnie tworzyć nowe menu i menu, dodając je do dołu istniejącego menu (utworzonym w programu Interface Builder), który został przekazany w:

```csharp
private void AssembleMenu(NSMenu menu, List<LanguageFormatCommand> commands) {
    NSMenuItem menuItem;

    // Add any formatting commands to the Formatting menu
    foreach (LanguageFormatCommand command in commands) {
        // Add separator or item?
        if (command.Title == "") {
            menuItem = NSMenuItem.SeparatorItem;
        } else {
            menuItem = new NSMenuItem (command.Title);

            // Submenu?
            if (command.SubCommands.Count > 0) {
                // Yes, populate submenu
                menuItem.Submenu = new NSMenu (command.Title);
                AssembleMenu (menuItem.Submenu, command.SubCommands);
            } else {
                // No, add normal menu item
                menuItem.Activated += (sender, e) => {
                    // Apply the command on the selected text
                    TextEditor.PerformFormattingCommand (command);
                };
            }
        }
        menu.AddItem (menuItem);
    }
}
``` 

Dla każdego `LanguageFormatCommand` obiekt, który ma puste `Title` właściwość ta procedura tworzy **element menu Separator** (cienkich szara linia) między sekcjami menu:

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

Jeśli tytuł zostanie podany, zostanie utworzony nowy element menu z tego tytułu:

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

Jeśli `LanguageFormatCommand` obiekt zawiera podrzędny `LanguageFormatCommand` obiektów, w menu podrzędne jest tworzony i `AssembleMenu` metodą jest cyklicznie, wywoływana w celu skompilowania menu:

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

Dla każdego nowego elementu menu, niezawierający podmenu kod zostanie dodany do obsługi elementu menu, zostaną wybrane przez użytkownika:

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>Testowanie tworzenie menu

W przypadku wszystkich powyższych kodu w miejscu Jeśli z następującą kolekcją `LanguageFormatCommand` obiekty zostały utworzone:

```csharp
// Define formatting commands
FormattingCommands.Add(new LanguageFormatCommand("Stong","**","**"));
FormattingCommands.Add(new LanguageFormatCommand("Emphasize","_","_"));
FormattingCommands.Add(new LanguageFormatCommand("Inline Code","`","`"));
FormattingCommands.Add(new LanguageFormatCommand("Code Block","```\n","\n```"));
FormattingCommands.Add(new LanguageFormatCommand("Comment","<!--","-->"));
FormattingCommands.Add (new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Unordered List","* "));
FormattingCommands.Add(new LanguageFormatCommand("Ordered List","1. "));
FormattingCommands.Add(new LanguageFormatCommand("Block Quote","> "));
FormattingCommands.Add (new LanguageFormatCommand ());

var Headings = new LanguageFormatCommand ("Headings");
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 1","# "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 2","## "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 3","### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 4","#### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 5","##### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 6","###### "));
FormattingCommands.Add (Headings);

FormattingCommands.Add(new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Link","[","]()"));
FormattingCommands.Add(new LanguageFormatCommand("Image","![]("-")"));
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[ ![]("-")](linkimagehere/index.md)"));
```

I że kolekcji przekazane do `AssembleMenu` — funkcja (przy użyciu **Format** Menu ustawiony jako podstawy), zostałyby utworzone elementy menu i menu dynamiczne z następujących:

![Nowe elementy menu w działającej aplikacji](menu-images/dynamic01.png "nowych elementów menu w działającej aplikacji")

#### <a name="removing-menus-and-items"></a>Usuwanie menu i elementów

Jeśli musisz usunąć element menu lub menu z interfejsu użytkownika aplikacji, możesz użyć `RemoveItemAt` metody `NSMenu` klasy po prostu polega na przekazaniu mu zero na podstawie indeks elementu do usunięcia.

Na przykład aby usunąć menu i menu elementy utworzone przez powyższe procedury, można użyć następującego kodu:

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

W przypadku powyższego kodu elementy: pierwsze cztery menu są tworzone w program Xcode Interface Builder i aways dostępnych w aplikacji, dzięki czemu nie są usuwane dynamicznie.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>Menu kontekstowe

Menu kontekstowe pojawiają się, gdy użytkownik kliknie prawym przyciskiem myszy lub kontroli kliknięć element w oknie. Domyślnie kilka elementów interfejsu użytkownika, wbudowane w system macOS już ma menu kontekstowych dołączonych do nich (na przykład widoku tekstu). Jednak może być czasy, kiedy chcemy utworzyć własne niestandardowe menu kontekstowe dla elementu interfejsu użytkownika, który dodano do okna.

Umożliwia edytowanie naszych **Main.storyboard** pliku w programie Xcode i Dodaj **okna** okna, aby nasz projekt, ustaw jego **klasy** do "NSPanel" w **Inspektor tożsamości**, Dodaj nowy **Asystenta** elementu do **okna** menu i dołączyć go do nowego przy użyciu okna **Pokaż Segue**:

[![Ustawianie typu segue](menu-images/context01.png "typ segue ustawienia")](menu-images/context01-large.png#lightbox)

Teraz należy wykonać następujące czynności:

1. Przeciągnij **etykiety** z **Inspektor biblioteki** na **panelu** okna i ustaw ich tekst "Property": 

    [![Edytowanie wartości etykiety](menu-images/context03.png "edycji wartości etykiety")](menu-images/context03-large.png#lightbox)
2. Następnie przeciągnij **Menu** z **Inspektor biblioteki** na kontroler widoku Wyświetl hierarchię i zmiany nazwy trzy domyślne menu elementy **dokumentu**, **tekstu**  i **czcionki**:

    [![Elementy menu wymagane](menu-images/context02.png "elementów menu wymagane")](menu-images/context02-large.png#lightbox)
3. Teraz formant podczas przeciągania z **etykieta właściwości** na **Menu**:

    [![Przeciąganie, aby utworzyć segue](menu-images/context04.png "przeciąganie, aby utworzyć segue")](menu-images/context04-large.png#lightbox)
4. W oknie podręcznym wybierz **Menu**: 

    ![Ustawianie typu segue](menu-images/context05.png "typ segue ustawienia")
5. Z **Inspektor tożsamości**, ustaw kontroler widoku klasy "PanelViewController": 

    [![Klasa segue ustawień](menu-images/context10.png "klasa segue ustawień")](menu-images/context10-large.png#lightbox)
6. Przejdź z powrotem do programu Visual Studio dla komputerów Mac do synchronizowania, a następnie wróć do programu Interface Builder.
7. Przełącz się do **edytora Asystenta** i wybierz **PanelViewController.h** pliku.
8. Utwórz akcję dla **dokumentu** element menu o nazwie `propertyDocument`: 

    [![Konfigurowanie akcji](menu-images/context06.png "Konfigurowanie akcji")](menu-images/context06-large.png#lightbox)
9. Powtórz czynności tworzenia dla pozostałych elementów menu: 

    [![Wymagane akcje](menu-images/context07.png "wymagane akcje")](menu-images/context07-large.png#lightbox)
10. Na koniec Utwórz sposobu wykorzystania **etykieta właściwości** o nazwie `propertyLabel`: 

    [![Konfigurowanie ujścia](menu-images/context08.png "Konfigurowanie ujścia")](menu-images/context08-large.png#lightbox)
11. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Edytuj **PanelViewController.cs** pliku i Dodaj następujący kod:

```csharp
partial void propertyDocument (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Document";
}

partial void propertyFont (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Font";
}

partial void propertyText (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Text";
}
```

Teraz możemy uruchomić aplikację, kliknij prawym przyciskiem myszy na etykiecie właściwości w panelu zobaczymy naszych niestandardowego menu kontekstowego. Jeśli zaznaczysz i element menu, zostanie zmieniona wartość etykiety:

![Menu kontekstowe uruchamiania](menu-images/context09.png "menu kontekstowe uruchamiania")

Dalej Przyjrzyjmy się tworzenie menu paska stanu.

## <a name="status-bar-menus"></a>Menu paska stanu

Menu paska stanu wyświetlania zbiór elementów menu stanu, które zapewniają interakcji z lub opinii do użytkownika, takich jak menu lub obraz odzwierciedlający stanu aplikacji. Menu na pasku stanu aplikacji jest włączone i aktywne, nawet wtedy, gdy aplikacja jest uruchomiona w tle. Na pasku stanu całego systemu znajduje się po prawej stronie paska menu aplikacji i jest obecnie dostępna w systemie macOS tylko pasek stanu.

Umożliwia edytowanie naszych **AppDelegate.cs** pliku i upewnij `DidFinishLaunching` wygląd metoda podobne do następującego:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Create a status bar menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Text";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Text");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        PhraseAddress(address);
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        PhraseDate(date);
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        PhraseGreeting(greeting);
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        PhraseSignature(signature);
    };
    item.Menu.AddItem (signature);
}
```

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` daje nam dostęp do pasku stanu całego systemu. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` Tworzy nowy element paska stanu. W tym miejscu utworzymy menu i liczbę elementów menu i dołączanie menu na element paska stanu, który właśnie utworzyliśmy. 

Jeśli możemy uruchomić aplikację, zostanie wyświetlony nowy element paska stanu. Zaznaczenie elementu menu zmiany tekstu w widoku tekstu: 

![Menu na pasku stanu uruchamiania](menu-images/statusbar01.png "menu na pasku stanu uruchamiania")

Następnie Przyjrzyjmy się tworzenie dock niestandardowych elementów menu.

## <a name="custom-dock-menus"></a>Zadokuj niestandardowego menu

Zadokowanie menu pojawia się w aplikacji dla komputerów Mac, gdy użytkownik kliknie prawym przyciskiem myszy lub kontroli — kliknięcia ikony aplikacji w docku:

![Niestandardowy zadokować menu](menu-images/dock01.png "niestandardowego zadokować menu")

Utworzymy niestandardowy zadokowanie menu dla naszej aplikacji, wykonując następujące czynności:

1. W programie Visual Studio dla komputerów Mac, kliknij prawym przyciskiem myszy projekt aplikacji i wybierz pozycję **Dodaj** > **nowy plik...** Okno dialogowe Nowy plik, zaznacz **Xamarin.Mac** > **pustą definicję interfejsu**, na użytek "DockMenu" **nazwa** i kliknij przycisk **New**  przycisk, aby utworzyć nowy **DockMenu.xib** pliku:

    ![Pusta definicja interfejsu dodanie](menu-images/dock02.png "Dodawanie pusta definicja interfejsu")
2. W **konsoli rozwiązania**, kliknij dwukrotnie **DockMenu.xib** plik, aby otworzyć do edycji w środowisku Xcode. Utwórz nową **Menu** z następującymi elementami: **adres**, **data**, **pozdrowienia**, i **podpisu** 

    [![Układ interfejsu użytkownika](menu-images/dock03.png "układ interfejsu użytkownika")](menu-images/dock03-large.png#lightbox)
3. Następnie połącz nasze nowe elementy menu do naszej istniejącej akcji, które utworzyliśmy dla naszych niestandardowego menu w [Dodawanie, edytowanie i usuwanie menu](#Adding,_Editing_and_Deleting_Menus) powyższej sekcji. Przełącz się do **Inspektor połączenia** i wybierz **pierwszy obiekt odpowiadający w trybie** w **hierarchii interfejsów**. Przewiń w dół i Znajdź `phraseAddress:` akcji. Przeciągnij linię od koła na tę akcję, aby **adres** element menu:

    [![Przeciągając przewodowy działania](menu-images/dock04.png "przeciągając przewodowy działania")](menu-images/dock04-large.png#lightbox)
4. Powtórz dla wszystkich innych elementów menu dołączanie ich do ich odpowiednich akcji: 

    [![Wymagane akcje](menu-images/dock05.png "wymagane akcje")](menu-images/dock05-large.png#lightbox)
5. Następnie wybierz pozycję **aplikacji** w **hierarchii interfejsów**. W **Inspektor połączenia**, przeciągnij linię od koła `dockMenu` gniazda do menu, którą właśnie utworzyliśmy:

    [![Przeciąganie przewodowy się ujścia](menu-images/dock06.png "przeciąganie przewodowy się ujścia")](menu-images/dock06-large.png#lightbox)
6. Zapisz zmiany i przejdź z powrotem do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.
7. Kliknij dwukrotnie **Info.plist** plik, aby otworzyć do edycji: 

    [![Edytowanie pliku Info.plist](menu-images/dock07.png "edycji pliku Info.plist")](menu-images/dock07-large.png#lightbox)
8. Kliknij przycisk **źródła** karta w dolnej części ekranu: 

    [![Widok źródła z wybraniu](menu-images/dock08.png "wybraniu widoku źródła")](menu-images/dock08-large.png#lightbox)
9. Kliknij przycisk **Dodaj nowy wpis**kliknij zielony przycisk ze znakiem plus, ustaw nazwę właściwości, aby "AppleDockMenu" i wartość "DockMenu" (nazwa nasz nowy plik .pliki bez rozszerzenia): 

    [![Dodawanie elementu DockMenu](menu-images/dock09.png "dodawania elementu DockMenu")](menu-images/dock09-large.png#lightbox)

Teraz możemy uruchomić aplikację, kliknij prawym przyciskiem myszy jego ikonę w obszarze Dock zostanie wyświetlony nasze nowe elementy menu:

![Przykładem zadokowanie menu uruchamiania](menu-images/dock10.png "przykładem zadokowanie menu uruchamiania")

Wybranie jednego z niestandardowych elementów menu, tekst w naszym widoku tekstu zostaną zmodyfikowane.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>Przycisk podręczne i listy rozwijane

Przycisk wyskakującego Wyświetla wybrany element i wyświetla listę opcji, które można wybierać po kliknięciu przez użytkownika. Listy rozwijane jest rodzajem przycisku wyskakujące, zwykle służy do wybierania poleceń specyficznych dla kontekstu bieżącego zadania. Jednocześnie może występować w dowolnym miejscu w oknie.

Utwórz niestandardowy przycisk podręczne dla naszej aplikacji, wykonując następujące czynności:

1. Edytuj **Main.storyboard** pliku w programie Xcode, a następnie przeciągnij **przycisk menu podręczne** z **Inspektor biblioteki** na **panelu** utworzonych w oknie [menu kontekstowe](#Contextual_Menus) sekcji: 

    [![Dodawanie przycisku menu podręczne](menu-images/popup01.png "Dodawanie przycisku menu podręczne")](menu-images/popup01-large.png#lightbox)
2. Dodaj nowy element menu i ustaw tytułów elementów w menu podręcznym do: **adres**, **data**, **pozdrowienia**, i **podpisu** 

    [![Konfigurowanie elementów menu](menu-images/popup02.png "konfigurowania elementów menu")](menu-images/popup02-large.png#lightbox)
3. Następnie połącz nasze nowe elementy menu do istniejącego akcji, które utworzyliśmy dla naszych niestandardowego menu w [Dodawanie, edytowanie i usuwanie menu](#Adding,_Editing_and_Deleting_Menus) powyższej sekcji. Przełącz się do **Inspektor połączenia** i wybierz **pierwszy obiekt odpowiadający w trybie** w **hierarchii interfejsów**. Przewiń w dół i Znajdź `phraseAddress:` akcji. Przeciągnij linię od koła na tę akcję, aby **adres** element menu: 

    [![Przeciągając przewodowy działania](menu-images/popup03.png "przeciągając przewodowy działania")](menu-images/popup03-large.png#lightbox)
4. Powtórz dla wszystkich innych elementów menu dołączanie ich do ich odpowiednich akcji: 

    [![Wszystkie wymagane akcje](menu-images/popup04.png "wszystkie wymagane akcje")](menu-images/popup04-large.png#lightbox)
5. Zapisz zmiany i przejdź z powrotem do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Teraz możemy uruchomić aplikację, wybierz element z menu podręcznego ulegną zmianie tekstu w naszym widoku tekstu:

![Przykład przedstawiający wyskakujące okienko z systemem](menu-images/popup05.png "przykład przedstawiający wyskakujące okienko z systemem")

Można tworzyć i pracować z listy rozwijane w dokładnie taki sam sposób jak przyciski wyskakujących. Zamiast dołączanie do istniejącej akcji, można utworzyć własne niestandardowe akcje, tak samo, jak robiliśmy nasze menu kontekstowe w [menu kontekstowe](#Contextual_Menus) sekcji.

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z menu i elementów menu w aplikacji platformy Xamarin.Mac. Najpierw zbadaliśmy aplikacji w pasku menu, a następnie tworzenie menu kontekstowe przyjrzeliśmy się obok zbadaliśmy menu paska stanu i niestandardowe zadokować menu. Na koniec pokrótce informacje omówione wyskakujących menu i listy rozwijane.


## <a name="related-links"></a>Linki pokrewne

- [MacMenus (przykład)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Wskazówki dotyczące interfejsu — menu](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [Wprowadzenie do menu aplikacji i wyskakujących list](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
