---
title: Zezwalaj użytkownikowi na Xamarin.Mac
description: W tym artykule omówiono Praca z menu w aplikacji Xamarin.Mac. Opisuje tworzenie i utrzymywanie menu i elementów menu w środowisku Xcode i kompilatora interfejsu oraz pracy z nimi programistycznie.
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: cb89d1df60bafe14dcc989666f0eeb5d757e4017
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792924"
---
# <a name="menus-in-xamarinmac"></a>Zezwalaj użytkownikowi na Xamarin.Mac

_W tym artykule omówiono Praca z menu w aplikacji Xamarin.Mac. Opisuje tworzenie i utrzymywanie menu i elementów menu w środowisku Xcode i kompilatora interfejsu oraz pracy z nimi programistycznie._

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tego samego menu Cocoa, które jest dewelopera pracy w języku Objective C i Xcode. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć konstruktora interfejsu w środowisku Xcode do tworzenia i obsługi paski menu, w menu i w menu elementów (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Menu są integralną częścią środowisko użytkownika aplikacji Mac i często pojawiają się w różnych częściach interfejsu użytkownika:

- **Pasek menu aplikacji** -to jest wyświetlany w górnej części ekranu dla każdej aplikacji dla komputerów Mac menu głównego.
- **Menu kontekstowe** — są one wyświetlane, gdy użytkownik kliknie prawym przyciskiem myszy lub kliknięcia formantu elementu w oknie.
- **Pasek stanu** — jest to obszar, z prawej strony paska menu aplikacji jest wyświetlany w górnej części ekranu (do lewej strony zegara paska menu), który zwiększa się po lewej stronie w miarę elementy zostaną dodane do niego.
- **Dokowanie menu** -menu dla każdej aplikacji w doku wyświetlony gdy użytkownik kliknie prawym przyciskiem myszy lub kliknięcia formantu ikonę aplikacji lub gdy użytkownik lewym przyciskiem myszy ikonę i przechowuje przycisku myszy w dół.
- **Przycisk wyskakujących i listy rozwijane** — przycisk wyskakujących Wyświetla wybrany element i wyświetla listę opcji wyboru po kliknięciu przez użytkownika. Listy rozwijane to typ przycisku wyskakujących zwykle służy do wybierania polecenia specyficzne dla kontekstu bieżącego zadania. Jednocześnie może występować w dowolnym miejscu w oknie.

[![Przykład menu](menu-images/intro01.png "przykład menu")](menu-images/intro01-large.png#lightbox)

W tym artykule omówione zostaną następujące czynności podstawowe informacje dotyczące pracy z paski menu Cocoa, menu i elementów menu w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` atrybutów używany do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

## <a name="the-applications-menu-bar"></a>Pasek menu aplikacji 

W przeciwieństwie do aplikacji działających na systemu operacyjnego, w którym co okno może mieć własną paska menu do niego dołączony każda aplikacja uruchomiona na macOS ma paska menu pojedynczego wzdłuż górnej części ekranu, który jest używany dla każdego okna w tej aplikacji:

[![Pasek menu](menu-images/appmenu01.png "pasek menu")](menu-images/appmenu01-large.png#lightbox)

Elementy tego paska menu są aktywowane lub dezaktywowane na podstawie bieżącego kontekstu lub stan aplikacji i interfejs użytkownika w danym momencie. Na przykład: Jeśli użytkownik wybierze pola tekstowego, elementy na **Edytuj** menu będzie pochodził włączone, takich jak **kopiowania** i **Wytnij**.

Zgodnie z firmy Apple i domyślnie wszystkie aplikacje macOS mają standardowy zestaw menu i elementów menu, które są wyświetlane na pasku menu aplikacji:

- **Apple menu** — w tym menu zapewnia dostęp do systemu szeroki elementów, które są dostępne dla użytkownika zawsze, niezależnie od tego, jakie aplikacja jest uruchomiona. Nie można zmodyfikować te elementy przez dewelopera.
- **Menu aplikacji** — w tym menu wyświetla nazwę aplikacji pogrubione i pomaga użytkownikowi określenie, jakie aplikacja jest obecnie uruchomiona. Zawiera elementy, które mają zastosowanie do aplikacji jako całości, a nie danego dokumentu lub procesu, takie jak kończenie działania aplikacji.
- **Menu Plik** — elementy umożliwiają tworzenie, otwieranie lub zapisywanie dokumentów, który aplikacja działa z. Jeśli aplikacja nie jest oparte na dokument, to menu można zmienić nazwy lub usunąć.
- **Menu Edycja** -przechowuje poleceń, takich jak **Wytnij**, **kopiowania**, i **Wklej** używane do edycji lub modyfikowanie elementów w interfejsie użytkownika aplikacji.
- **Format menu** — Jeśli aplikacja działa z tekstem, to menu poleceń blokad, aby dostosować formatowanie tekstu.
- **Menu Widok** — zawiera polecenia, które mają wpływ na sposób wyświetlania zawartości (wyświetlanie) w interfejsie użytkownika aplikacji.
- **Menu specyficzne dla aplikacji** — są to wszystkie menu, które są specyficzne dla aplikacji (np. menu zakładki w przeglądarce sieci web). Powinny one występować między **widoku** i **okna** menu na pasku.
- **Menu okna** — zawiera polecenia do pracy z systemem windows w aplikacji, a także listę bieżącego okna.
- **Menu Pomoc** — Jeśli aplikacja zawiera pomoc na ekranie, w menu Pomoc powinien być na pasku menu prawej krawędzi. 

Aby uzyskać więcej informacji o aplikacji paska menu i menu standardowe i elementów menu, zobacz firmy Apple [Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/).

### <a name="the-default-application-menu-bar"></a>Domyślny pasek menu aplikacji

Podczas tworzenia nowego projektu Xamarin.Mac automatycznie pobrać standardowe, domyślnej aplikacji paskiem menu zawierającego typowe elementy, które aplikacji macOS mają (jak opisano w powyższej sekcji). Pasek menu domyślnej aplikacji jest zdefiniowany w **Main.storyboard** pliku (wraz z resztą Interfejsie użytkownika aplikacji) w obszarze projektu w **konsoli rozwiązania**:  

![Wybierz głównego storyboard](menu-images/appmenu02.png "wybierz głównego storyboard")

Kliknij dwukrotnie **Main.storyboard** plik, aby go otworzyć do edycji w konstruktora interfejsu w środowisku Xcode i wyświetlony interfejs Edytora menu:

[![Edytowanie interfejsu użytkownika w środowisku Xcode](menu-images/defaultbar01.png "edycji interfejsu użytkownika w środowisku Xcode")](menu-images/defaultbar01-large.png#lightbox)

W tym miejscu możemy kliknąć elementów takich jak **Otwórz** elementu menu w **pliku** menu edytować i dostosować jego właściwości w **inspektora atrybutów**:

[![Edytowanie atrybutów menu](menu-images/defaultbar02.png "edycji atrybutów menu")](menu-images/defaultbar02-large.png#lightbox)

Możemy uzyskać na dodawanie, edytowanie i usuwanie menu i elementów w dalszej części tego artykułu. Dla teraz po prostu chcesz zobaczyć, jakie elementy menu i menu są domyślnie dostępne i sposób ich być automatycznie narażone kodu przez zestaw wstępnie zdefiniowanych gniazda i akcje (Aby uzyskać więcej informacji, zobacz nasze [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) dokumentacja).

Na przykład po kliknięciu pozycji na **inspektora połączenia** dla **Otwórz** możemy stwierdzić, jest automatycznie przewodowej do elementu menu `openDocument:` akcji: 

[![Wyświetlanie akcji dołączonych](menu-images/defaultbar03.png "wyświetlanie dołączonych akcji")](menu-images/defaultbar03-large.png#lightbox)

W przypadku wybrania **pierwszy obiekt odpowiadający w trybie** w **hierarchii interfejsów** i przewiń w dół **inspektora połączenia**, i zobaczysz definicję `openDocument:` Akcja który **Otwórz** element menu jest dołączony do (wraz z kilka innych domyślnej akcji dla aplikacji i nie są automatycznie powiązaną klasą formantów):

[![Wyświetlanie wszystkich dołączonych akcje](menu-images/defaultbar04.png "wyświetlanie wszystkich dołączonych akcji")](menu-images/defaultbar04-large.png#lightbox) 

Dlaczego jest to istotne? W następnej sekcji zostanie wyświetlony, jak te akcje zdefiniowane automatycznie współpracuje z inne elementy interfejsu użytkownika Cocoa, które automatycznie Włączanie i wyłączanie elementów menu, a także, podaj wbudowanej funkcji dla elementów.

Później będziemy używać tych działań wbudowanych do włączenia i wyłączenia elementów z kodu i udostępnia własnej funkcji po jej wybraniu.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>Funkcje wbudowane menu

Jeśli uruchomienie nowo utworzonej aplikacji Xamarin.Mac przed dodaniem elementów interfejsu użytkownika ani kodu innych, można zauważyć, że niektóre elementy są automatycznie przewodowej w górę i włączona (z pełnej funkcjonalności automatycznie wbudowane), takich jak **Zakończ** elementu **aplikacji** menu:

![Element menu włączone](menu-images/appmenu03.png "element menu włączone")

Połączenie innych elementów menu, takich jak **Wytnij**, **kopiowania**, i **Wklej** nie są:

![Elementy menu wyłączone](menu-images/appmenu04.png "wyłączone elementy menu")

Załóżmy Zatrzymaj aplikację i kliknij dwukrotnie **Main.storyboard** w pliku **konsoli rozwiązania** go otworzyć do edycji w środowisku Xcode do konstruktora interfejsu. Następnie przeciągnij **widoku tekstu** z **biblioteki** na okna kontrolera widoku w **Edytor interfejsu**:

[![Wybranie widoku tekstu z biblioteki](menu-images/appmenu05.png "wybranie widoku tekstu z biblioteki")](menu-images/appmenu05-large.png#lightbox)

W **edytora ograniczeń** umożliwia przypiąć widoku tekstu do krawędzi okna i ustaw dla niej gdzie rozwoju i zmniejsza okno z klikając wszystkie cztery czerwony I świateł w górnej części edytora i klikając **dodać ograniczenia 4** przycisk:

[![Edytowanie ograniczenia](menu-images/appmenu06.png "Edytowanie ograniczenia")](menu-images/appmenu06-large.png#lightbox)

Zapisz zmiany w projekcie interfejsu użytkownika i przełączenia programu Visual Studio for Mac zsynchronizować zmiany z projektu Xamarin.Mac. Teraz uruchom aplikację, wpisz dowolny tekst w widoku tekstu, zaznacz go i Otwórz **Edytuj** menu:

![Elementy menu są automatycznie włączone/wyłączone](menu-images/appmenu07.png "elementy menu są automatycznie włączone/wyłączone")

Powiadomienie jak **Wytnij**, **kopiowania**, i **Wklej** elementy są automatycznie włączone i funkcjonalnej, wszystko to bez napisania jakiegokolwiek wiersza kodu. 

Co się dzieje w tym miejscu? Należy pamiętać, wbudowane wstępnie akcje, które pochodzą przewodowej elementów menu domyślne (przedstawionych powyżej), większość Cocoa elementy interfejsu użytkownika, które są częścią macOS mają wbudowane punkty zaczepienia określone działania (takie jak `copy:`). Dlatego podczas dodawania do okna, aktywna i wybrać odpowiedniego elementu menu lub elementy dołączony do tego działania są włączane automatycznie. Jeśli użytkownik wybierze tę pozycję menu, funkcji wbudowanych w elementu interfejsu użytkownika jest nazywane i wykonywane bez interwencji developer.

### <a name="enabling-and-disabling-menus-and-items"></a>Włączanie i wyłączanie menu i elementy

Domyślnie za każdym razem, gdy wystąpi zdarzenie użytkownika `NSMenu` automatycznie włącza i wyłącza każdego widoczne menu i menu elementu na podstawie kontekstu aplikacji. Istnieją trzy sposoby, aby włączyć lub wyłączyć elementu:

- **Włączanie automatycznego menu** — element menu jest włączona, jeśli `NSMenu` można znaleźć odpowiedniego obiektu, który odpowiada na akcję, która element jest przewodowej w górę do. Na przykład widoku tekstu powyżej mająca wbudowanych haku do `copy:` akcji.
- **Akcje niestandardowe i validateMenuItem:** — dowolny element menu, który jest powiązany z [okna lub Widok akcji niestandardowej kontrolera](#Working-with-Custom-Window-Actions), można dodać `validateMenuItem:` akcji i ręcznie włączyć lub wyłączyć elementów menu.
- **Włączanie menu ręczne** -ręcznie ustawić `Enabled` właściwości każdego `NSMenuItem` Aby włączyć lub wyłączyć każdego elementu w menu indywidualnie.

Aby wybrać system, należy ustawić `AutoEnablesItems` właściwość `NSMenu`. `true` odbywa się automatycznie (domyślnie) i `false` jest ręcznie. 

> [!IMPORTANT]
> Jeśli chcesz użyć menu ręczne włączenie, żaden z menu elementów, nawet te kontrolowane przez AppKit klas takich jak `NSTextView`, są automatycznie aktualizowane. Będzie odpowiedzialny za włączanie i wyłączanie wszystkie elementy ręcznie w kodzie.

#### <a name="using-validatemenuitem"></a>Przy użyciu validateMenuItem

Jak wspomniano powyżej, dla każdego elementu menu, który jest powiązany z [okna lub Akcja niestandardowa kontrolera widoku](#Working-with-Custom-Window-Actions), możesz dodać `validateMenuItem:` akcji i ręcznie włączyć lub wyłączyć elementów menu.

W poniższym przykładzie `Tag` właściwości będzie służyć do określania, typ elementu menu, które będą mieć włączone/wyłączone przez `validateMenuItem:` akcji na podstawie stanu zaznaczonego tekstu w `NSTextView`. `Tag` Właściwość została ustawiona w Konstruktorze interfejs dla każdego elementu menu:

![Ustawienie właściwości tagu](menu-images/validate01.png "ustawienie właściwości tagu")

I następujący kod, dodano do kontrolera widoku:

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

Kiedy uruchamiać ten kod, a nie zaznaczono żadnych tekstu w `NSTextView`, zawijania dwa elementy menu są wyłączone (nawet jeśli są one podłączone do akcji kontrolera widoku):

![Wyświetlanie wyłączone elementów](menu-images/validate02.png "przedstawiający elementy wyłączone")

Jeśli wybrano fragment tekstu, a następnie ponownie otworzyć menu, zawijania dwóch elementów menu będą dostępne:

![Włączono wyświetlanie elementów](menu-images/validate03.png "przedstawiający włączone elementów")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>Włączanie i odpowiada na żądania elementów menu w kodzie

Jak możemy jak już wspomniano powyżej, po prostu, dodając określone elementy interfejsu użytkownika Cocoa do naszej projektu interfejsu użytkownika (np. w polu tekstowym), kilka elementów menu domyślny zostanie włączona i działa automatycznie, bez konieczności pisania kodu. Następny Przyjrzyjmy się dodawanie swój własny kod C# do naszej projektu Xamarin.Mac, aby włączyć element menu i zapewniać funkcje, gdy użytkownik wybierze opcję.

Na przykład let powiedzieć chcemy użytkownik będzie mógł używać **Otwórz** elementu **pliku** menu, aby wybrać inny folder. Ponieważ chcemy, aby była funkcja całej aplikacji, a nie tylko zapewnia okna lub elementu interfejsu użytkownika, zamierzamy Dodaj kod, aby obsłużyć to delegatem naszej aplikacji.

W **konsoli rozwiązania**, kliknij dwukrotnie `AppDelegate.CS` plik, aby otworzyć do edycji:

![Wybieranie delegata aplikacji](menu-images/appmenu08.png "wybranie delegata aplikacji")

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

Zwróć uwagę, że **Otwórz** element menu jest teraz włączony. Jeśli mamy zaznacz go, pojawi się okno dialogowe Otwórz:

![Otwórz okno dialogowe](menu-images/appmenu10.png "Otwórz okno dialogowe")

Po kliknięciu pozycji **Otwórz** przycisku, zostanie wyświetlony komunikat z naszych alertu:

![Przykład komunikatu dialogu](menu-images/appmenu11.png "przykładowy komunikat okna dialogowego")

W tym miejscu wiersz klucza została `[Export ("openDocument:")]`, informuje o tym, `NSMenu` który naszych **AppDelegate** ma metodę `void OpenDialog (NSObject sender)` który odpowiada na `openDocument:` akcji. Jeśli zapamiętania z powyższych **Otwórz** element menu jest automatycznie przewodowej zestawienia z tej akcji domyślny konstruktor interfejsu:

[![Wyświetlanie dołączonych akcje](menu-images/defaultbar03.png "wyświetlanie dołączonych akcje")](menu-images/defaultbar03-large.png#lightbox)

Następny Przyjrzyjmy się tworzenie własnej menu, elementy menu i akcje i odpowiada do nich w kodzie.

### <a name="working-with-the-open-recent-menu"></a>Praca z menu ostatnie Otwórz

Domyślnie **pliku** zawiera menu **Otwórz ostatni** elementu, który przechowuje informacje o ostatnich kilka pliki otwierane z aplikacją. Jeśli tworzysz `NSDocument` na podstawie Xamarin.Mac aplikacji, to menu będzie zostać obsłużone automatycznie. Dla aplikacji Xamarin.Mac innego typu będzie odpowiedzialny za zarządzanie i reagowanie na ten element menu ręcznie.

Aby ręcznie obsługi **Otwórz ostatni** menu, musisz najpierw informują o tym że nowy plik został otwarty lub zapisany przy użyciu następujących:

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

Mimo że aplikacja nie używa `NSDocuments`, nadal używać `NSDocumentController` do obsługi **Otwórz ostatni** menu, wysyłając `NSUrl` lokalizację pliku do `NoteNewRecentDocumentURL` metody `SharedDocumentController`.

Następnie należy zastąpić `OpenFile` metody delegata aplikacji do otwierania plików użytkownik wybiera z **Otwórz ostatnie** menu. Na przykład:

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

Zwraca `true` przypadku plik może być otwarty, przeciwnym razie zwraca `false` i wbudowane ostrzeżenie będzie widoczny dla użytkownika, że nie można otworzyć pliku.

Ponieważ nazwa pliku i ścieżka zwrócona z **Otwórz ostatni** menu może zawierać spacji, musimy poprawnie ten znak ucieczki przed utworzeniem `NSUrl` lub firma Microsoft będzie wyświetlany komunikat o błędzie. Przejdziemy następującym kodem:

```csharp
filename = filename.Replace (" ", "%20");
```

Na koniec Utwórz `NSUrl` wskazującego do pliku i użyj metody pomocnika w aplikacji przekazać Otwórz nowe okno i załaduj plik do niej:

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

Do scalenia wszystko, Spójrzmy na przykład implementacja **AppDelegate.cs** pliku:

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

Na podstawie wymagań aplikacji, nie możesz użytkownika do otwarcia tego samego pliku w więcej niż jedno okno w tym samym czasie. W naszym przykładową aplikację, jeśli użytkownik wybierze pliku, który jest już otwarty (albo z **Otwórz ostatnie** lub **Otwórz...** elementy menu), okna zawierającego plik jest przeniesiony do przodu.

W tym celu użyliśmy następującego kodu w naszym metody pomocniczej:

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

Firma Microsoft zaprojektowane naszych `ViewController` klasy utrzymującej ścieżkę do pliku w jego `Path` właściwości. Następnie Pętla przez wszystkie aktualnie otwarte okna w aplikacji. Jeśli plik jest już otwarty w jednym z systemu windows, zostanie przywrócony na początku wszystkich okien przy użyciu:

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

Jeśli nie znaleziono, nowe okno jest otwarty z plikiem załadowane i plik jest odnotowany w **Otwórz ostatni** menu:

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

### <a name="working-with-custom-window-actions"></a>Praca z działań niestandardowych okien

Podobnie jak wbudowane **pierwszy obiekt odpowiadający w trybie** akcje, które zostały wstępnie przewodowej elementów menu standardowego, możesz utworzyć nowe, akcje niestandardowe i okablować ich do elementów menu w Konstruktorze interfejsu.

Najpierw należy zdefiniować niestandardowej akcji na jednym z kontrolerów okna aplikacji. Na przykład:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

Następnie kliknij dwukrotnie plik scenorysu aplikacji w **konsoli rozwiązania** go otworzyć do edycji w środowisku Xcode do konstruktora interfejsu. Wybierz **pierwszy obiekt odpowiadający w trybie** w obszarze **sceny aplikacji**, następnie przejdź do **inspektora atrybutów**:

![Atrybuty inspektora](menu-images/action01.png "inspektora atrybutów")

Kliknij przycisk **+** przycisk w dolnej części **inspektora atrybuty** do dodania nowych akcji niestandardowej:

![Dodawanie nowej akcji](menu-images/action02.png "Dodawanie nowej akcji")

Nadaj taką samą nazwę jak utworzony na kontrolerze okna akcji niestandardowej:

![Edytowanie nazwy akcji](menu-images/action03.png "edycji nazwy akcji")

Formant kliknij i przeciągnij od elementu menu **pierwszy obiekt odpowiadający w trybie** w obszarze **sceny aplikacji**. Na liście podręcznym zaznacz nową akcję właśnie utworzony (`defineKeyword:` w tym przykładzie):

![Dołączanie akcji](menu-images/action04.png "Dołączanie akcji")

Zapisz zmiany do scenorysu i wróć do programu Visual Studio for Mac zsynchronizować zmiany. Po uruchomieniu aplikacji element menu, która połączona Akcja niestandardowa w celu zostanie automatycznie być włączone/wyłączone (na podstawie okna z akcją okna) i wybraniu elementu menu będzie wyzwalać poza akcji:

[![Testowanie nowej akcji](menu-images/action05.png "testowanie nowej akcji")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>Dodawanie, edytowanie i usuwanie menu

Jak możemy jak już wspomniano w poprzedniej sekcji, aplikacja Xamarin.Mac zawiera wstępnie ustawiony numer domyślny menu i elementów menu, które automatycznie aktywować i odpowiadać na określonych formantów interfejsu użytkownika. Zaobserwowano również sposób dodawania kodu do naszej aplikacji, która będzie także włączyć i odpowiadać na te elementy domyślne.

W tej sekcji zajmiemy usuwanie elementów menu, które nie są nam potrzebne, reorganizacja menu i dodawanie nowych menu, elementy menu i akcje.

Kliknij dwukrotnie **Main.storyboard** w pliku **konsoli rozwiązania** go otworzyć do edycji:

[![Edytowanie interfejsu użytkownika w środowisku Xcode](menu-images/maint01.png "edycji interfejsu użytkownika w środowisku Xcode")](menu-images/maint01-large.png#lightbox)

Dla określonej aplikacji Xamarin.Mac nie zamierzamy się przy użyciu domyślnego **widoku** menu tak zamierzamy go usunąć. W **hierarchii interfejsów** wybierz **widoku** element menu, który jest częścią na pasku menu głównym:

![Wybranie elementu menu Widok](menu-images/maint02.png "wybraniu elementu menu Widok")

Naciśnij klawisz delete lub backspace do usunięcia z menu. Następnie firma Microsoft nie mają być używane wszystkie elementy w **Format** menu i my chcesz przenieść elementy, firma Microsoft będzie używany limit z menu podrzędne. W **hierarchii interfejsów** wybierz następujące pozycje menu:

![Wyróżnianie wielu elementów](menu-images/maint03.png "wyróżnianie wielu elementów")

Przeciągnij elementy podrzędne nadrzędnego **Menu** podmenu gdy aktualnie są:

[![Przeciąganie elementów menu do menu nadrzędnego](menu-images/maint04.png "przeciąganie elementów menu do menu nadrzędnego")](menu-images/maint04-large.png#lightbox)

Z menu powinna wyglądać tak jak:

[![Elementy w nowej lokalizacji](menu-images/maint05.png "elementów w nowej lokalizacji")](menu-images/maint05-large.png#lightbox)

Dalej umożliwia przeciągnij **tekst** podmenu się zgodnie z **Format** menu i umieścić go na pasku menu głównego między **Format** i **okna** menu:

[![Menu tekst](menu-images/maint06.png "tekst menu")](menu-images/maint06-large.png#lightbox)

Przejdźmy pod **Format** menu i Usuń **czcionki** elementu podmenu. Następnie wybierz pozycję **Format** menu i zmień jego nazwę na "Font":

[![Menu Czcionka](menu-images/maint07.png "menu Czcionka")](menu-images/maint07-large.png#lightbox)

Następnie utwórz niestandardowe menu predefine wyrażeń, które automatycznie zostanie uzyskać dołączany na tekst w widoku tekstu po jej wybraniu. W polu wyszukiwania w dół na **inspektora biblioteki** typu "menu". Ułatwi znajdowanie i pracy z wszystkich elementów menu interfejsu użytkownika:

![Biblioteka inspektora](menu-images/maint08.png "inspektora biblioteki")

Teraz załóżmy wykonaj następujące polecenie, aby utworzyć naszych menu:

1. Przeciągnij **element Menu** z **inspektora biblioteki** na pasku menu między **tekst** i **okna** menu: 

    ![Wybranie nowego elementu menu w bibliotece](menu-images/maint10.png "wybranie nowego elementu menu w bibliotece")
2. Zmień nazwę elementu "Frazy": 

    [![Ustawienie nazwy menu](menu-images/maint09.png "ustawienie nazwy menu")](menu-images/maint09-large.png#lightbox)
3. Następnie przeciągnij **Menu** z **inspektora biblioteki**: 

    ![Wybranie menu z biblioteki](menu-images/maint11.png "wybierając menu z biblioteki")
4. Następnie upuścić **Menu** na nowym **element Menu** możemy właśnie utworzony i zmień jego nazwę na "Frazy": 

    [![Edytowanie nazwy menu](menu-images/maint12.png "zmiany jego nazwy menu")](menu-images/maint12-large.png#lightbox)
5. Teraz możemy zmienić trzy domyślne **elementów Menu** "Address", "Date" i "Pozdrowienia": 

    [![Menu fraz](menu-images/maint13.png "fraz menu")](menu-images/maint13-large.png#lightbox)
6. Dodajmy czwarty **element Menu** przeciągając **element Menu** z **inspektora biblioteki** i wywoływanie jej "Podpisu": 

    [![Nazwa elementu menu Edycja](menu-images/maint14.png "edycji Nazwa elementu menu")](menu-images/maint14-large.png#lightbox)
7. Zapisać zmiany na pasku menu.

Teraz Utwórzmy działań niestandardowych, dzięki czemu nasze nowe elementy menu są widoczne dla kodu C#. W środowisku Xcode umożliwia przełączanie do **Asystenta** widoku:

[![Tworzenie wymagane akcje](menu-images/maint15.png "tworzenie wymagane akcje")](menu-images/maint15-large.png#lightbox)

Teraz należy wykonać następujące czynności:

1. Przeciągnij formant z **adres** elementu menu **AppDelegate.h** pliku.
2. Przełącznik **połączenia** typ **akcji**: 

    [![Wybranie typu akcji](menu-images/maint17.png "wybranie typu akcji")](menu-images/maint17-large.png#lightbox)
3. Wprowadź **nazwa** "phraseAddress" i naciśnij klawisz **Connect** przycisk, aby utworzyć nową akcję: 

    [![Konfigurowanie akcji](menu-images/maint18.png "Konfigurowanie akcji")](menu-images/maint18-large.png#lightbox)
4. Powtórz powyższe kroki dla **data**, **pozdrowienia**, i **podpisu** elementów menu: 

    [![Ukończonych akcji](menu-images/maint19.png "ukończonych akcji")](menu-images/maint19-large.png#lightbox)
5. Zapisać zmiany na pasku menu.

Następnie należy utworzyć gniazda dla naszych widoku tekstu tak, aby firma Microsoft może dostosować zawartość z kodu. Wybierz **ViewController.h** w pliku **Edytor Asystenta** i utworzenie nowego punktu sprzedaży o nazwie `documentText`:

[![Tworzenie gniazda](menu-images/maint20.png "Tworzenie gniazda")](menu-images/maint20-large.png#lightbox)

Wróć do programu Visual Studio for Mac zsynchronizować zmiany z Xcode. Następnie Edytuj **ViewController.cs** pliku i zapewnić ich wyglądać następująco:

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

Udostępnia to tekst naszych widoku tekstu poza `ViewController` klasy i informuje o delegata aplikacji, gdy okno uzyskuje lub utraci fokus. Teraz edytować **AppDelegate.cs** pliku i zapewnić ich wyglądać następująco:

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

W tym miejscu wprowadziliśmy `AppDelegate` częściowym klasy, dzięki czemu możemy użyć akcji i gniazda, zdefiniowane w Konstruktorze interfejsu. Możemy również ujawniać `textEditor` do śledzenia, które okno jest aktualnie fokus.

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

Teraz, jeśli możemy uruchomić aplikację, wszystkie elementy w **frazy** menu będzie aktywny i doda podać hasło do widoku tekstu, w przypadku wybrania:

![Przykładem aplikacji uruchomionej](menu-images/maint21.png "Przykładem aplikacji")

Teraz, gdy mamy podstawy pracy z paska menu aplikacji w dół, Przyjrzyjmy się tworzenie niestandardowych menu kontekstowego.

### <a name="creating-menus-from-code"></a>Tworzenie menu z kodu

Oprócz tworzenia menu i elementów menu z konstruktora interfejsu w programie Xcode, może być razy, gdy aplikacja Xamarin.Mac potrzebuje do tworzenia, modyfikowania lub usuwania menu, podmenu lub elementu menu z kodu.

W poniższym przykładzie utworzono klasę do przechowywania informacji o elementów menu i podmenu, które będzie można dynamicznie utworzyć na bieżąco:

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

#### <a name="adding-menus-and-items"></a>Dodawanie menu i elementy

Z tą klasą zdefiniowane następujące procedury będzie analizować Kolekcja `LanguageFormatCommand`obiektów i rekursywnie tworzenia nowego menu i elementów menu przez dodanie ich do dolnej części istniejącego menu (utworzone w Konstruktorze interfejsu), który został przekazany w:

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

Dla każdego `LanguageFormatCommand` obiektu, który ma pusty `Title` właściwości, ta procedura powoduje utworzenie **element menu separatora** (cienkich szara linia) między menu sekcje:

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

Jeśli podany tytuł jest tworzony nowy element menu z tym tytułem:

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

Jeśli `LanguageFormatCommand` obiekt zawiera podrzędny `LanguageFormatCommand` obiekty podmenu jest tworzony i `AssembleMenu` metoda jest rekursywnie wywołuje budować tego menu:

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

Dla dowolnego nowego elementu menu, który nie ma podmenu kod został dodany do obsługi elementu menu wybierane przez użytkownika:

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>Testowanie tworzenie menu

Biorąc pod uwagę powyżej kodu w miejscu Jeśli po pobraniu `LanguageFormatCommand` obiekty zostały utworzone:

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

I że kolekcji przekazywane do `AssembleMenu` — funkcja (z **Format** Menu ustawić jako podstawa), może zostać utworzone następujące menu dynamiczne i elementów menu:

![Nowych elementów menu w uruchomionej aplikacji](menu-images/dynamic01.png "nowych elementów menu w uruchomionej aplikacji")

#### <a name="removing-menus-and-items"></a>Usuwanie menu i elementy

Jeśli musisz usunąć element menu lub menu z interfejsu użytkownika aplikacji, możesz użyć `RemoveItemAt` metody `NSMenu` klasy po prostu przez nadanie mu od zera na podstawie indeks elementu do usunięcia.

Na przykład aby usunąć menu i menu elementy utworzone przez powyższej procedury, można użyć poniższego kodu:

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

W przypadku kodu powyżej pierwsze cztery menu elementów są tworzone w konstruktora interfejsu i aways dostępne w aplikacji w programie Xcode, więc nie są usuwane dynamicznie.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>Menu kontekstowe

Menu kontekstowe są wyświetlane, gdy użytkownik kliknie prawym przyciskiem myszy lub kliknięcia formantu elementu w oknie. Domyślnie kilka elementów interfejsu użytkownika, wbudowane w system macOS już mieć menu kontekstowe dołączony do ich (na przykład widoku tekstu). Jednak może być razy, gdy chcemy, aby utworzyć własne niestandardowe menu kontekstowe dla elementu interfejsu użytkownika, które dodano do okna.

Umożliwia edytowanie naszych **Main.storyboard** w środowisku Xcode i Dodaj **okna** okno naszych projekt ustawić jej **klasy** do "NSPanel" w **tożsamości inspektora**, Dodaj nową **Asystenta** elementu do **okna** menu i dołączenie go do nowego przy użyciu okna **Pokaż Segue**:

[![Ustawienie typu segue](menu-images/context01.png "segue typ ustawienia")](menu-images/context01-large.png#lightbox)

Teraz należy wykonać następujące czynności:

1. Przeciągnij **etykiety** z **inspektora biblioteki** na **panelu** okna i Ustaw tekst w nim "Właściwości": 

    [![Edytowanie etykiety wartość](menu-images/context03.png "edycji wartości etykiety")](menu-images/context03-large.png#lightbox)
2. Następnie przeciągnij **Menu** z **inspektora biblioteki** na kontroler widoku w widoku hierarchii i elementy menu domyślne zmiany nazwy trzech **dokumentu**, **tekstu**  i **czcionki**:

    [![Elementy menu wymagane](menu-images/context02.png "elementów menu wymagane")](menu-images/context02-large.png#lightbox)
3. Teraz kontrolki przeciągania z **etykiety właściwości** na **Menu**:

    [![Przeciąganie, aby utworzyć segue](menu-images/context04.png "przeciąganie, aby utworzyć segue")](menu-images/context04-large.png#lightbox)
4. W oknie podręcznym wybierz **Menu**: 

    ![Ustawienie typu segue](menu-images/context05.png "segue typ ustawienia")
5. Z **inspektora tożsamości**, ustaw kontroler widoku klasy "PanelViewController": 

    [![Ustawienie klasy segue](menu-images/context10.png "ustawienie klasy segue")](menu-images/context10-large.png#lightbox)
6. Wrócić do programu Visual Studio dla komputerów Mac do synchronizacji, a następnie wróć do konstruktora interfejsu.
7. Przełącz się do **Edytor Asystenta** i wybierz **PanelViewController.h** pliku.
8. Utwórz działanie dla **dokumentu** wywołuje element menu `propertyDocument`: 

    [![Konfigurowanie akcji](menu-images/context06.png "Konfigurowanie akcji")](menu-images/context06-large.png#lightbox)
9. Powtórz tworzenia akcji dla pozostałych elementów menu: 

    [![Wymagane akcje](menu-images/context07.png "wymagane akcje")](menu-images/context07-large.png#lightbox)
10. Na koniec Utwórz gniazda dla **etykiety właściwości** o nazwie `propertyLabel`: 

    [![Konfigurowanie gniazda](menu-images/context08.png "Konfigurowanie gniazda")](menu-images/context08-large.png#lightbox)
11. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

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

Teraz możemy uruchomić aplikację, kliknij prawym przyciskiem myszy na etykiecie właściwości w panelu możemy zostanie wyświetlone menu kontekstowe naszych niestandardowe. Jeśli firma Microsoft wybierz i elementu menu zostanie zmieniona wartość etykiety:

![Menu kontekstowe systemem](menu-images/context09.png "menu kontekstowe uruchomiona")

Następny Przyjrzyjmy się tworzenie menu paska stanu.

## <a name="status-bar-menus"></a>Menu paska stanu

Menu paska stanu wyświetlany kolekcji elementów menu stanie, zapewniające interakcji z lub opinii użytkowników, takich jak menu lub obraz odzwierciedlające stan aplikacji. Menu na pasku stanu aplikacji jest włączone i aktywne nawet wtedy, gdy aplikacja jest uruchomiona w tle. Pasek stanu systemowe znajduje się po prawej stronie paska menu, aplikacji i jest obecnie dostępna w macOS tylko paska stanu.

Umożliwia edytowanie naszych **AppDelegate.cs** plików i upewnij `DidFinishLaunching` wygląd metody podobne do poniższych:

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

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` daje dostęp do paska stanu całego systemu. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` Tworzy nowy element paska stanu. Z tego miejsca możemy utworzyć menu i liczbę elementów menu i Dołącz menu do element paska stanu, które właśnie utworzyliśmy. 

Jeśli możemy uruchomić aplikację, zostanie wyświetlony nowy element paska stanu. Wybranie elementu z menu zmieni się tekst w widoku tekstu: 

![Menu na pasku stanu systemem](menu-images/statusbar01.png "systemem menu paska stanu")

Następnie Przyjrzyjmy się tworzenie niestandardowych dokowania elementów menu.

## <a name="custom-dock-menus"></a>Menu niestandardowe doku.

Menu dokowania pojawia się w aplikacji dla komputerów Mac, gdy użytkownik kliknie prawym przyciskiem myszy lub kliknięcia formantu ikonę aplikacji w doku:

![Niestandardowy dock menu](menu-images/dock01.png "niestandardowego dokowanie menu")

Umożliwia tworzenie menu dokowania niestandardowe dla aplikacji w następujący sposób:

1. W programie Visual Studio for Mac, kliknij prawym przyciskiem myszy projekt i wybierz aplikacji **Dodaj** > **nowego pliku...** W oknie dialogowym Nowy plik wybierz **Xamarin.Mac** > **pusta definicja interfejsu**, użyj "DockMenu" dla **nazwa** i kliknij przycisk **nowy**  przycisk, aby utworzyć nowy **DockMenu.xib** pliku:

    ![Dodawanie pusta definicja interfejsu](menu-images/dock02.png "Dodawanie pusta definicja interfejsu")
2. W **konsoli rozwiązania**, kliknij dwukrotnie **DockMenu.xib** plik, aby otworzyć do edycji w środowisku Xcode. Utwórz nową **Menu** z następującymi elementami: **adres**, **data**, **pozdrowienia**, i **podpisu** 

    [![Układ interfejsu użytkownika](menu-images/dock03.png "układ interfejsu użytkownika")](menu-images/dock03-large.png#lightbox)
3. Następnie możemy nawiązać połączenia z naszych nowych elementów menu istniejące działania utworzonymi dla naszego menu niestandardowe w [Dodawanie, edytowanie i usuwanie menu](#Adding,_Editing_and_Deleting_Menus) powyższej sekcji. Przełącz się do **inspektora połączenia** i wybierz **pierwszy obiekt odpowiadający w trybie** w **hierarchii interfejsów**. Przewiń w dół i Znajdź `phraseAddress:` akcji. Przeciągnij linię z okręgu tego działania **adres** element menu:

    [![Przeciąganie można przewodowo działaniami](menu-images/dock04.png "przeciąganie można przewodowo działaniami")](menu-images/dock04-large.png#lightbox)
4. Powtórz dla wszystkich innych elementów menu dołączać do ich odpowiednich akcji: 

    [![Wymagane akcje](menu-images/dock05.png "wymagane akcje")](menu-images/dock05-large.png#lightbox)
5. Następnie wybierz pozycję **aplikacji** w **hierarchii interfejsów**. W **inspektora połączenia**, przeciągnij linię z okręgu `dockMenu` gniazda do menu właśnie utworzyliśmy:

    [![Przeciąganie zachowuje się gniazda](menu-images/dock06.png "przeciąganie zachowuje się gniazda")](menu-images/dock06-large.png#lightbox)
6. Zapisz zmiany i wrócić do programu Visual Studio for Mac synchronizację w programie Xcode.
7. Kliknij dwukrotnie **Info.plist** plik, aby otworzyć do edycji: 

    [![Edytowanie pliku Info.plist](menu-images/dock07.png "edytowania pliku Info.plist")](menu-images/dock07-large.png#lightbox)
8. Kliknij przycisk **źródła** u dołu ekranu: 

    [![Wybranie widoku źródła](menu-images/dock08.png "wybraniu widoku źródła")](menu-images/dock08-large.png#lightbox)
9. Kliknij przycisk **Dodaj nowy wpis**kliknij zielony oraz przycisk, ustaw nazwę właściwości, aby "AppleDockMenu" i wartością "DockMenu" (nazwa naszego nowego pliku .xib bez rozszerzenia): 

    [![Dodawanie elementu DockMenu](menu-images/dock09.png "Dodawanie elementu DockMenu")](menu-images/dock09-large.png#lightbox)

Teraz możemy uruchomić aplikację, kliknij prawym przyciskiem myszy odpowiednią ikonę w doku naszych nowych elementów menu zostanie wyświetlony:

![Przykład menu dokowania systemem](menu-images/dock10.png "przykładem menu dokowania uruchomiona")

Jeśli firma Microsoft wybierz jeden z niestandardowych elementów z menu tekst w naszym widoku tekstu zostaną zmodyfikowane.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>Przycisk wyskakujących i listy rozwijane

Przycisk wyskakujących Wyświetla wybrany element i wyświetla listę opcji wyboru po kliknięciu przez użytkownika. Listy rozwijane to typ przycisku wyskakujących zwykle służy do wybierania polecenia specyficzne dla kontekstu bieżącego zadania. Jednocześnie może występować w dowolnym miejscu w oknie.

Utwórz niestandardowy przycisk wyskakującego dla aplikacji w następujący sposób:

1. Edytuj **Main.storyboard** pliku w programie Xcode i przeciągnij **przycisk menu podręczne** z **inspektora biblioteki** na **panelu** utworzyliśmy w oknie [menu kontekstowe](#Contextual_Menus) sekcji: 

    [![Dodawanie przycisku menu podręczne](menu-images/popup01.png "Dodawanie przycisku menu podręczne")](menu-images/popup01-large.png#lightbox)
2. Dodaj nowy element menu i ustaw tytuły elementy w menu podręcznym do: **adres**, **data**, **pozdrowienia**, i **podpisu** 

    [![Konfigurowanie elementów menu](menu-images/popup02.png "Konfigurowanie elementy menu")](menu-images/popup02-large.png#lightbox)
3. Następnie możemy nawiązać połączenia z naszych nowych elementów menu Akcje istniejącego utworzonymi dla naszego menu niestandardowe w [Dodawanie, edytowanie i usuwanie menu](#Adding,_Editing_and_Deleting_Menus) powyższej sekcji. Przełącz się do **inspektora połączenia** i wybierz **pierwszy obiekt odpowiadający w trybie** w **hierarchii interfejsów**. Przewiń w dół i Znajdź `phraseAddress:` akcji. Przeciągnij linię z okręgu tego działania **adres** element menu: 

    [![Przeciąganie można przewodowo działaniami](menu-images/popup03.png "przeciąganie można przewodowo działaniami")](menu-images/popup03-large.png#lightbox)
4. Powtórz dla wszystkich innych elementów menu dołączać do ich odpowiednich akcji: 

    [![Wszystkie wymagane akcje](menu-images/popup04.png "wszystkie wymagane akcje")](menu-images/popup04-large.png#lightbox)
5. Zapisz zmiany i wrócić do programu Visual Studio for Mac synchronizację w programie Xcode.

Teraz możemy uruchomić aplikację, wybierz element z menu podręcznego ulegnie zmianie tekstu w naszym widoku tekstu:

![Przykład podręcznego systemem](menu-images/popup05.png "przykładem podręcznego uruchomiona")

Można tworzyć i pracować z listy rozwijane w dokładnie sam sposób jak przyciski podręczne. Zamiast dołączanie do istniejących działań, można utworzyć własne niestandardowe akcje tak samo, jak robiliśmy dla naszych menu kontekstowego w [menu kontekstowe](#Contextual_Menus) sekcji.

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się praca z menu i elementów menu w aplikacji Xamarin.Mac. Najpierw paska menu aplikacji, możemy zbadane, a następnie analizujemy tworzenie menu kontekstowe, następnie możemy zbadać stan paska menu i niestandardowe dock menu. Ponadto firma Microsoft objęte menu podręcznego i listy rozwijane.


## <a name="related-links"></a>Linki pokrewne

- [MacMenus (przykład)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Human Interface Guidelines — menu](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [Wprowadzenie do menu aplikacji i listy wyskakujących](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
