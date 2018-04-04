---
title: Tworzenie macOS nowoczesnych aplikacji
description: W tym artykule opisano kilka porad, funkcje i techniki przez dewelopera umożliwia tworzenie aplikacji modern macOS w Xamarin.Mac.
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4eb4ff4a9e4784d816e2cbe8734e0422573cad92
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="building-modern-macos-apps"></a>Tworzenie macOS nowoczesnych aplikacji

_W tym artykule opisano kilka porad, funkcje i techniki przez dewelopera umożliwia tworzenie aplikacji modern macOS w Xamarin.Mac._

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>Tworzenie nowoczesny wygląd z widokami nowoczesnych

Nowoczesny wygląd obejmuje nowoczesny wygląd okna i narzędzi takie jak aplikacja przykładzie pokazano poniżej:

[![](modern-cocoa-apps-images/content08.png "Przykład nowoczesnych aplikacji Mac interfejsu użytkownika")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>Włączenie pełnego rozmiaru zawartości widoków

Aby osiągnąć ten wygląda w aplikacji Xamarin.Mac, będziesz używać Projektanta _pełnego widoku zawartości rozmiar_, co oznacza zawartość rozszerza w obszarach narzędzia i paska tytułu i będzie można automatycznie zaciera macOS.

Aby włączyć tę funkcję w kodzie, Utwórz klasę niestandardowych dla `NSWindowController` i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Set window to use Full Size Content View
            Window.StyleMask = NSWindowStyle.FullSizeContentView;
        }
        #endregion
    }
}
```

Tej funkcji można również włączyć w Konstruktorze interfejsu w środowisku Xcode wybierając okna i sprawdzanie **pełny widok zawartości o rozmiarze**:

[![](modern-cocoa-apps-images/content01.png "Edytowanie głównego storyboard w Konstruktorze interfejsu w środowisku Xcode")](modern-cocoa-apps-images/content01.png#lightbox)

Podczas korzystania z pełnego widoku zawartości rozmiar, deweloper może być konieczne przesunięcia zawartości poniżej obszarów paska tytułu i narzędzia, aby określonej zawartości (na przykład etykiety) nie slajd pod nimi.

Aby skomplikować ten problem, obszarów tytuł i pasek narzędzi może mieć wysokość dynamiczna oparte na akcję, która wykonuje obecnie użytkownika, wersja macOS użytkownik zainstalował i/lub sprzęt Mac, że aplikacja jest uruchomiona na.

W związku z tym po prostu twardego kodowania przesunięcie podczas rozmieszczania interfejsu użytkownika nie będzie działać. Deweloper musisz wykonać dynamicznej podejście.

Dołączył Apple [klucz-wartość zauważalne](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` właściwość `NSWindow` klasy można pobrać bieżącego obszaru zawartości w kodzie. Deweloper może użyć tej wartości ręcznie umieścić wymaganych elementów po zmianie obszar zawartości.

Lepszym rozwiązaniem jest na potrzeby umieść elementy interfejsu użytkownika za pomocą kodu lub interfejsu konstruktora klasy rozmiaru i układu automatycznego.

Kod, jak w następującym przykładzie może służyć do pozycji elementy interfejsu użytkownika przy użyciu klasy rozmiar i AutoLayout w widoku Appcontroller:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        #region Computed Properties
        public NSLayoutConstraint topConstraint { get; set; }
        #endregion

        ...

        #region Override Methods
        public override void UpdateViewConstraints ()
        {
            // Has the constraint already been set?
            if (topConstraint == null) {
                // Get the top anchor point
                var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
                var topAnchor = contentLayoutGuide.TopAnchor;

                // Found?
                if (topAnchor != null) {
                    // Assemble constraint and activate it
                    topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
                    topConstraint.Active = true;
                }
            }

            base.UpdateViewConstraints ();
        }
        #endregion
    }
}
```

Ten kod tworzy magazyn dla górnego ograniczenia, które będą stosowane do etykiety (`ItemTitle`) aby upewnić się, że nie dokumentu w obszarze tytuł i pasek narzędzi:

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

Przez zastąpienie kontroler widoku `UpdateViewConstraints` metody dewelopera można testu, jeśli wymagane ograniczenie już został utworzony i utworzyć ją w razie potrzeby.

Jeśli nowe ograniczenie musi zostać utworzony, `ContentLayoutGuide` właściwości formantu, który ma być ograniczone jest dostępne i rzutowanie do okna `NSLayoutGuide`:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

Właściwość TopAnchor `NSLayoutGuide` jest dostępny i jeśli jest dostępny, jest używany do tworzenia nowego ograniczenia z odpowiednią wielkość przesunięcia i nowe ograniczenie jest aktywowane ją zastosować:

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>Włączanie prostsze paski narzędzi

Normalne macOS okna zawiera standard, który uruchamia paska tytułu u górnej krawędzi okna. Jeśli okno zawiera również pasek narzędzi, zostanie wyświetlony w obszarze ten obszar paska tytułu:

[![](modern-cocoa-apps-images/content02.png "Standardowych narzędzi Mac")](modern-cocoa-apps-images/content02.png#lightbox)

Jeśli przy użyciu narzędzi prostsze, obszar tytułu zniknie i pasek narzędzi zostanie przesunięty w określonej pozycji na pasku tytułu, w linii z Zamknij okno, przyciski:

[![](modern-cocoa-apps-images/content03.png "Prostsze narzędzi Mac")](modern-cocoa-apps-images/content03.png#lightbox)

Włączono zoptymalizowane paska narzędzi przez zastąpienie `ViewWillAppear` metody `NSViewController` i co wyglądać podobnie do następującego:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

W tym celu jest zazwyczaj używana w przypadku _aplikacji Shoebox_ (jedno okno aplikacji), takich jak map, kalendarza, uwagi i preferencjach systemowych. 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>Za pomocą kontrolerów akcesoriów widoku

W zależności od projektu aplikacji, deweloper może być również konieczne uzupełniają paska tytułu są obszaru z kontrolera widoku dodatek wyświetlany w prawej poniżej obszar paska tytułu/narzędzia zapewnienie kontekstowe kontrolki użytkownika na podstawie działania one obecnie zajmujących się:

[![](modern-cocoa-apps-images/content04.png "Przykładowy dodatek kontrolera widoku")](modern-cocoa-apps-images/content04.png#lightbox)

Kontroler widoku akcesoriów zostaną automatycznie rozmyciu i dopasowane przez system bez interwencji developer.

Aby dodać kontroler widoku akcesoriów, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji.
2. Przeciągnij **kontroler niestandardowy widok** do okna hierarchii: 

    [![](modern-cocoa-apps-images/content05.png "Dodawanie nowego kontrolera widoku niestandardowego")](modern-cocoa-apps-images/content05.png#lightbox)
3. Układ widoku akcesoriów interfejsu użytkownika: 

    [![](modern-cocoa-apps-images/content06.png "Projektowanie nowego widoku")](modern-cocoa-apps-images/content06.png#lightbox)
4. Udostępnianie akcesoriów widoku w postaci **gniazda** oraz wszelkie inne **akcje** lub **gniazda** jego interfejsu użytkownika: 

    [![](modern-cocoa-apps-images/content07.png "Dodawanie wymaganych gniazda")](modern-cocoa-apps-images/content07.png#lightbox)
5. Zapisz zmiany.
6. Wróć do programu Visual Studio for Mac zsynchronizować zmiany.

Edytuj `NSWindowController` i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }
        #endregion
    }
}
```

Klucz wskazuje tego kodu są, gdzie widoku jest ustawiony jako widok niestandardowy, który został zdefiniowany w Konstruktorze interfejsu i udostępniany jako **gniazda**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

I `LayoutAttribute` definiuje _gdzie_ urządzenia będą wyświetlane:

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

Ponieważ system macOS teraz jest zlokalizowana w pełni, `Left` i `Right` `NSLayoutAttribute` właściwości są przestarzałe i powinna zostać zastąpiona `Leading` i `Trailing`.

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>Przy użyciu systemu Windows z kartami

Ponadto macOS system może dodać kontrolery widoku akcesoriów do okna aplikacji. Na przykład, aby utworzyć na kartach systemu Windows, gdy kilka aplikacji systemu Windows są scalane w jednym oknie wirtualnego:

[![](modern-cocoa-apps-images/content08.png "Przykład okno Mac z kartami")](modern-cocoa-apps-images/content08.png#lightbox)

Zazwyczaj dewelopera, musisz podjąć akcję ograniczone użycie Windows kartach w swoich aplikacjach Xamarin.Mac, system obsługi je automatycznie w następujący sposób:

- System Windows będzie automatycznie kartach, kiedy `OrderFront` wywołania metody.
- System Windows będzie automatycznie podczas Untabbed `OrderOut` wywołania metody.
- W kodzie wszystkich kartach systemu windows nadal są traktowane jako "visible" jednak żadnych kart z systemem innym niż przedniej są ukryte przez system za pomocą CoreGraphics.
- Użyj `TabbingIdentifier` właściwość `NSWindow` do grupy systemu Windows, w kartach.
- Jeśli jest `NSDocument` zależności aplikacji, niektóre z tych funkcji zostanie włączona automatycznie (na przykład przycisk plus dodawany do paska kartę) bez żadnych działań developer.
- Z systemem innym niż`NSDocument` aplikacji można włączyć przycisk "plus" w grupie kart, aby dodać nowy dokument przez zastąpienie `GetNewWindowForTab` metody `NSWindowsController`.

Łącząc wszystkie elementy, `AppDelegate` aplikacji, którą chce używać systemu Windows z kartami może wyglądać następująco:

```csharp
using AppKit;
using Foundation;

namespace MacModern
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewDocumentNumber { get; set; } = 0;
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

        #region Custom Actions
        [Export ("newDocument:")]
        public void NewDocument (NSObject sender)
        {
            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow (this);
        } 
        #endregion
    }
}
```

Gdzie `NewDocumentNumber` właściwość przechowuje informacje o liczbę nowych dokumentów utworzonych i `NewDocument` metoda tworzy nowy dokument i wyświetla je.

`NSWindowController` Następnie może wyglądać jak:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Application Access
        /// <summary>
        /// A helper shortcut to the app delegate.
        /// </summary>
        /// <value>The app.</value>
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void SetDefaultDocumentTitle ()
        {
            // Is this the first document?
            if (App.NewDocumentNumber == 0) {
                // Yes, set title and increment
                Window.Title = "Untitled";
                ++App.NewDocumentNumber;
            } else {
                // No, show title and count
                Window.Title = $"Untitled {App.NewDocumentNumber++}";
            }
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Prefer Tabbed Windows
            Window.TabbingMode = NSWindowTabbingMode.Preferred;
            Window.TabbingIdentifier = "Main";

            // Set default window title
            SetDefaultDocumentTitle ();

            // Set window to use Full Size Content View
            // Window.StyleMask = NSWindowStyle.FullSizeContentView;

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }

        public override void GetNewWindowForTab (NSObject sender)
        {
            // Ask app to open a new document window
            App.NewDocument (this);
        }
        #endregion
    }
}
```

Gdzie statycznych `App` właściwość zapewnia skrót na uzyskanie dostępu do `AppDelegate`. `SetDefaultDocumentTitle` Metoda ustawia nowy tytuł dokumentów na podstawie liczby nowych dokumentów utworzonych.

Poniższy kod, informuje system macOS preferowany aplikacji do korzystania z kart i udostępnia ciąg, który umożliwia aplikacji systemu Windows, można podzielić na kartach:

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

I następującą metodę zastąpienie dodaje przycisk plus do kart, który zostanie utworzony nowy dokument, po kliknięciu przez użytkownika:

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>Przy użyciu animacji Core

Podstawowe animacji jest aparat renderowania grafiki wysokiego zasilanie, który jest wbudowana w system macOS. Aby móc korzystać z procesora GPU (przetwarzania graficzny) dostępne w nowoczesnych macOS sprzętu zamiast uruchamiania operacji grafiki na Procesor, co może zmniejszyć maszynę został zoptymalizowany Core animacji.

`CALayer`, Dostarczone przez animację Core, może być używany dla zadania, takie jak szybka i płynne przewijanie i animacji. Interfejs użytkownika aplikacji powinien składać się z wielu widoków podrzędnych i warstwy w pełni korzystać z animacji Core.

A `CALayer` obiektu zawiera wiele właściwości, które umożliwiają deweloperom kontroli, co przedstawiono na ekranie użytkownikowi takich jak:

- `Content` — Może być `NSImage` lub `CGImage` zapewnia zawartość warstwy.
- `BackgroundColor` — Ustawia kolor tła warstwy jako `CGColor`
- `BorderWidth` — Ustawia szerokość obramowania.
- `BorderColor` — Ustawia kolor obramowania.

Korzystanie z grafiki Core w Interfejsie użytkownika aplikacji, muszą używać _kopii warstwy_ widoków, które Apple sugeruje, że deweloper zawsze należy włączyć w widoku zawartości okna. W ten sposób wszystkie widoki podrzędnych automatycznie odziedziczą kopii warstwy również.

Ponadto Apple sugeruje korzystanie z widoków kopii warstwy w przeciwieństwie do dodawania nowego `CALayer` jako podwarstwę ponieważ system automatycznie obsłuży niektóre z wymaganych ustawień (takich jak zasoby wymagane przez wyświetlenie siatkówki).

Warstwy kopii można włączyć, ustawiając `WantsLayer` z `NSView` do `true` lub wewnątrz konstruktora interfejsu Xcode firmy w obszarze **inspektora efekty widoku** sprawdzając **warstwy animacji Core**:

[![](modern-cocoa-apps-images/content09.png "Widok inspektora efekty")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>Odświeżanie widoków z warstwy

Innym ważnym krokiem podczas ustawiania jest korzystanie z widoków kopii warstwy w aplikacji Xamarin.Mac `LayerContentsRedrawPolicy` z `NSView` do `OnSetNeedsDisplay` w `NSViewController`. Na przykład:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Jeśli projektanta nie ustawić tę właściwość, widok będzie narysowania przy każdej zmianie jego pochodzenie ramki, który nie jest wymagana ze względu na wydajność. Z tą właściwością ustawioną `OnSetNeedsDisplay` dewelopera ręcznie, należy ustawić `NeedsDisplay` do `true` Aby wymusić ponowne, jednak zawartość.

Gdy widok jest oznaczony jako zmieniony, system sprawdza `WantsUpdateLayer` właściwości widoku. Jeśli zmienna zwraca `true` , a następnie `UpdateLayer` metoda jest wywoływana, else `DrawRect` wywoływana jest metoda widoku, aby zaktualizować zawartość widoku.

Apple ma poniższe sugestie dla aktualizowanie zawartości widoków, gdy jest to wymagane:

- Preferuje Apple przy użyciu `UpdateLater` za pośrednictwem `DrawRect` zawsze, gdy to możliwe, ponieważ zapewnia wydajność znaczne zwiększenie wydajności.
- Używać tego samego `layer.Contents` dla podobne elementy interfejsu użytkownika.
- Apple również preferuje developer utworzenie ich interfejsu użytkownika przy użyciu standardowych widoków, takich jak `NSTextField`, ponownie możliwe.

Aby użyć `UpdateLayer`, tworzenie niestandardowej klasy dla `NSView` i wprowadź kod wyglądać następująco:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView
    {
        #region Computed Properties
        public override bool WantsLayer {
            get { return true; }
        }

        public override bool WantsUpdateLayer {
            get { return true; }
        }
        #endregion

        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void DrawRect (CoreGraphics.CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

        }

        public override void UpdateLayer ()
        {
            base.UpdateLayer ();

            // Draw view 
            Layer.BackgroundColor = NSColor.Red.CGColor;
        }
        #endregion
    }
}
```

<a name="Using-Modern-Drag-and-Drop" />

## <a name="using-modern-drag-and-drop"></a>Przy użyciu nowoczesnych przeciąganie i upuszczanie

Przedstawienie modern przeciąganie i upuszczanie środowisko użytkownika powinna przyjąć dewelopera _Flocking przeciągania_ w swoich aplikacji operacji przeciągania i upuszczania. Przeciągnij Flocking jest, gdzie każdego pojedynczego pliku lub początkowo przeciąganie elementu jest wyświetlany jako pojedynczy element, który stad (grupa razem pod kursorem wraz z liczbą liczba elementów), jako użytkownik kontynuuje operację przeciągania.

Jeśli użytkownik kończy operację przeciągania, poszczególne elementy będą Unflock i wróć do ich oryginalnych lokalizacji.

Poniższy przykładowy kod umożliwia przeciągania Flocking na widok niestandardowy:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView, INSDraggingSource, INSDraggingDestination
    {
        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void MouseDragged (NSEvent theEvent)
        {
            // Create group of string to be dragged
            var string1 = new NSDraggingItem ((NSString)"Item 1");
            var string2 = new NSDraggingItem ((NSString)"Item 2");
            var string3 = new NSDraggingItem ((NSString)"Item 3");

            // Drag a cluster of items
            BeginDraggingSession (new [] { string1, string2, string3 }, theEvent, this);
        }
        #endregion
    }
}
```

Osiągnięto flocking efekt, wysyłając każdy element przeciąganego `BeginDraggingSession` metody `NSView` jako osobny element w tablicy.

Podczas pracy z `NSTableView` lub `NSOutlineView`, użyj `PastboardWriterForRow` metody `NSTableViewDataSource` klasy można uruchomić operacji przeciąganie:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDataSource: NSTableViewDataSource
    {
        #region Constructors
        public ContentsTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override INSPasteboardWriting GetPasteboardWriterForRow (NSTableView tableView, nint row)
        {
            // Return required pasteboard writer
            ...
            
            // Pasteboard writer failed
            return null;
        }
        #endregion
    }
}
```

Dzięki temu deweloper zapewnienie osoby `NSDraggingItem` dla każdego elementu w tabeli, która jest przeciągany w przeciwieństwie do starszych — metoda `WriteRowsWith` , które zapisują wszystkie wiersze w jednej grupie obszar roboczy.

Podczas pracy z `NSCollectionViews`, ponownie użyj `PasteboardWriterForItemAt` metody, w przeciwieństwie do `WriteItemsAt` rozpocznie się podczas przeciągania.

Deweloper powinien zawsze należy unikać umieszczania dużych plików w obszarze roboczym. Jesteś nowym użytkownikiem macOS Sierra, _ze zobowiązania pliku_ umożliwiają deweloperom umieść odwołań do danego plików w obszarze roboczym, które później zostaną spełnione, gdy użytkownik zakończy operacji usuwania za pomocą nowej `NSFilePromiseProvider` i `NSFilePromiseReceiver` klasy.

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>Przy użyciu nowoczesnych zdarzenia śledzenia

Dla elementu interfejsu użytkownika (takich jak `NSButton`) który dodano do obszaru tytuł lub pasek narzędzi, użytkownik powinien mieć możliwość kliknij element, a jego zdarzenia Normal (na przykład wyświetlanie okna podręcznego). Jednak ponieważ element znajduje się również w obszarze tytuł lub pasek narzędzi, użytkownik powinien mieć możliwość kliknij i przeciągnij element, aby przenieść okno również.

W tym celu w kodzie, Utwórz klasę niestandardowego elementu (takich jak `NSButton`) i Przesłoń `MouseDown` zdarzeń w następujący sposób:

```csharp
public override void MouseDown (NSEvent theEvent)
{
    var shouldCallSuper = false;

    Window.TrackEventsMatching (NSEventMask.LeftMouseUp, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Handle event as normal
        stop = true;
        shouldCallSuper = true;
    });

    Window.TrackEventsMatching(NSEventMask.LeftMouseDragged, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Pass drag event to window
        stop = true;
        Window.PerformWindowDrag (evt);
    });

    // Call super to handle mousedown
    if (shouldCallSuper) {
        base.MouseDown (theEvent);
    }
}
```

Ten kod zawiera `TrackEventsMatching` metody `NSWindow` dołączonego do elementu interfejsu użytkownika do przechwycenia `LeftMouseUp` i `LeftMouseDragged` zdarzenia. Aby uzyskać `LeftMouseUp` zdarzenia, elementu interfejsu użytkownika odpowiada normalnie. Dla `LeftMouseDragged` zdarzeń, zdarzenia są przekazywane do `NSWindow`w `PerformWindowDrag` metodę, aby przenieść okno na ekranie.

Wywoływanie `PerformWindowDrag` metody `NSWindow` klasy zapewnia następujące korzyści:

- Zezwala na okno, aby przenieść, nawet wtedy, gdy aplikacja jest zawieszona (takie jak czas przetwarzania głębokie pętli).
- Przełączanie miejsca będzie działać zgodnie z oczekiwaniami.
- Pasek spacje będzie wyświetlany jako standardowa.
- Okno przyciągania i wyrównanie pracy normalnego.

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>Przy użyciu nowoczesnych kontenera kontrolki widoku

System macOS Sierra zawiera wiele ulepszeń nowoczesnych istniejących formantów kontenera widok dostępny w poprzedniej wersji systemu operacyjnego.

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>Ulepszenia widoku tabeli

Deweloper zawsze należy używać nowego `NSView` na podstawie wersji kontenera kontrolki widoku, takie jak `NSTableView`. Na przykład:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }
        #endregion
    }
}
```

Dzięki temu akcje niestandardowe wiersza tabeli jest dołączony do danego wierszy w tabeli (takich jak szybko przesuwając od prawej do usunięcia wiersza). Aby włączyć to zachowanie, należy zastąpić `RowActions` metody `NSTableViewDelegate`:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }

        public override NSTableViewRowAction [] RowActions (NSTableView tableView, nint row, NSTableRowActionEdge edge)
        {
            // Take action based on the edge
            if (edge == NSTableRowActionEdge.Trailing) {
                // Create row actions
                var editAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Regular, "Edit", (action, rowNum) => {
                    // Handle row being edited
                    ...
                });

                var deleteAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Destructive, "Delete", (action, rowNum) => {
                    // Handle row being deleted
                    ...
                });

                // Return actions
                return new [] { editAction, deleteAction };
            } else {
                // No matching actions
                return null;
            }
        }
        #endregion
    }
}
```

Statycznych `NSTableViewRowAction.FromStyle` służy do tworzenia nowej akcji wiersza tabeli następujące style:

- `Regular` -Wykonuje standardowych działań nieszkodliwe takich jak edytowanie zawartości wiersza.
- `Destructive` -Wykonuje destrukcyjnego akcji, takich jak usuwanie wiersza z tabeli. Te akcje będzie renderowany z czerwonym tła.

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>Ulepszenia widoku przewijania 

Korzystając z widoku przewijania (`NSScrollView`) bezpośrednio lub jako część innego formantu (takie jak `NSTableView`), zawartość widoku przewijania można przesunąć w obszarach tytuł i paska narzędzi w aplikacji Xamarin.Mac przy użyciu widoków i nowoczesny wygląd.

W związku z tym pierwszego elementu w obszarze zawartości przewijania widoku może częściowo zasłonięty przez obszar tytuł i paska narzędzi.

Aby rozwiązać ten problem, Apple dodał dwie nowe właściwości do `NSScrollView` klasy:

- `ContentInsets` — Umożliwia deweloperowi podaj `NSEdgeInsets` obiekt definiujący przesunięcia, które zostaną zastosowane do góry widoku przewijania.
- `AutomaticallyAdjustsContentInsets` -Jeśli `true` widoku przewijania będzie automatycznie obsługiwał `ContentInsets` dla deweloperów.

Za pomocą `ContentInsets` deweloper może dostosować start przewijania widoku umożliwiające włączenie Akcesoria takich jak:

- Wskaźnik sortowania, takich jak ta pokazana w aplikacji poczty.
- Pole wyszukiwania.
- Przycisk odświeżania lub aktualizacji.

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>Automatycznie Rozmieść i lokalizacja w nowoczesnych aplikacji

Apple dołączył kilku technologii w środowisku Xcode, które umożliwiają deweloperom łatwe tworzenie aplikacji międzynarodowych macOS. Xcode teraz umożliwia deweloperowi oddzielenia tekstu dla użytkownika z projektu interfejsu użytkownika aplikacji w jego pliki scenorysu i udostępnia narzędzia do obsługi rozdzielenie w przypadku zmiany interfejsu użytkownika.

Aby uzyskać więcej informacji, zobacz firmy Apple [przystosowywanie do warunków międzynarodowych i lokalizacja przewodnik](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html).

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>Wdrażanie podstawowego internacjonalizacji 

Zaimplementowanie internacjonalizacji Base dewelopera zapewniają pojedynczy plik scenorysu będzie odpowiadać Interfejsie użytkownika aplikacji i rozdzielać wszystkich ciągów dla użytkownika. 

Gdy deweloper tworzy plik scenorysu (lub pliki) definiującą interfejsu użytkownika aplikacji, zostanie utworzona w internacjonalizacji Base (język mówi dewelopera).

Następnie dewelopera można wyeksportować lokalizacje i ciągów internacjonalizacji Base (w projekcie interfejsu użytkownika scenorysu), które można przetłumaczyć wielu języków.

Później te lokalizacje mogą być importowane i Xcode wygeneruje pliki językowe ciągu dla scenorysu.

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>Implementowanie automatycznego układu do obsługi lokalizacji

Ponieważ zlokalizowane wersje ciągu wartości mogą mieć znacząco różnych rozmiarach i/lub odczytywania kierunek, deweloper należy używać automatycznego układu do położenia i rozmiaru interfejsu użytkownika aplikacji w pliku scenorysu.

Apple sugerują, wykonując następujące czynności:

- **Usuń stałej szerokości ograniczenia** — wszystkie widoki tekstowe powinny mieć możliwość zmiany rozmiaru na podstawie ich zawartości. Stała szerokość widok może przyciąć ich zawartość w określonych języków.
- **Użyj rozmiarów zawartość wewnętrzna** — domyślna tekstowych widoków zostanie automatycznie Dopasuj dopasowana do ich zawartości. Dla widoku tekstowych, które nie są poprawnie zmiany rozmiaru, wybierz je w Konstruktorze interfejsu w programie Xcode, a następnie wybierz **Edytuj** > **rozmiar do dopasowania zawartości**.
- **Zastosuj wiodące i końcowe atrybuty** — nie można zmienić kierunek tekstu oparte na języku użytkownika, użyj nowych `Leading` i `Trailing` atrybutów ograniczenia zamiast istniejących `Right` i `Left` atrybuty. `Leading` i `Trailing` automatycznie Dostosuj w zależności od kierunku języków.
- **Widoki numeru PIN do widoków sąsiadujących ze sobą** — dzięki temu widoki zmienia położenie i zmiany rozmiaru w widokach wokół nich zmiany w odpowiedzi na język wybrany.
- **Nie należy ustawiać co najmniej systemu Windows i/lub maksymalne rozmiary** — Zezwalaj na systemu Windows, aby zmienić rozmiar, zgodnie z językiem wybranym zmienia rozmiar obszarów ich zawartości.
- **Testowanie stale zmiany układu** — podczas tworzenia w aplikacji należy podawać stale w różnych językach. Zobacz firmy Apple [testowania międzynarodowych Your app](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) dokumentację, aby uzyskać więcej szczegółowych informacji.
- **Użyj NSStackViews do numeru Pin widoków ze sobą**  -  `NSStackViews` umożliwia ich zawartość do przesunięcia i rozwój w sposób przewidywalny i zawartość zmienić rozmiar oparte na języku, który został wybrany.

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Lokalizowanie w Konstruktorze interfejsu w środowisku Xcode

Apple udostępnił kilka funkcji w Konstruktorze interfejsu w programie Xcode, że deweloper może użyć podczas projektowania lub edycji w Interfejsie użytkownika aplikacji do obsługi lokalizacji. **Kierunek tekstu** sekcji **inspektora atrybutu** umożliwia deweloperowi zawierają wskazówki na sposób wykorzystania i aktualizowane na wybierz widok tekstowy kierunek (takie jak `NSTextField`):

[![](modern-cocoa-apps-images/content10.png "Opcje kierunek tekstu")](modern-cocoa-apps-images/content10.png#lightbox)

Istnieją trzy możliwe wartości **kierunek tekstu**:

- **Fizyczne** — układ jest oparta na ciąg znaków przypisany do formantu.
- **Od lewej do prawej** — zawsze wymusza układzie od lewej do prawej.
- **Od prawej do lewej** — układ zawsze będzie zmuszony do prawej do lewej.

Istnieją dwa możliwe wartości **układu**:

- **Od lewej do prawej** — układ jest zawsze od lewej do prawej.
- **Od prawej do lewej** — układ jest zawsze od prawej do lewej.

Zwykle te nie powinny być zmieniane, chyba że wyraźnego związku jest wymagana.

**Dublowany** właściwości informuje system, aby odwrócić właściwości określonego formantu (na przykład położenie obrazu komórki). Ma trzy możliwe wartości:

- **Automatycznie** — pozycja automatycznie zmieniać w oparciu o kierunek język wybrany.
- **W prawej do lewej strony interfejsu** — pozycja będzie można zmienić tylko w prawej do lewej na podstawie języków.
- **Nigdy nie** — nigdy nie zmienia położenie.

Jeśli określono dewelopera **Center**, **Wyjustuj** lub **pełne** wyrównanie zawartości widoku opartego na tekście, nigdy nie będą odwrócony na podstawie język wybrany.

Przed macOS Sierra formanty utworzone w kodzie może nie być dublowane automatycznie. Deweloper nie może obsłużyć dublowania za pomocą kodu podobnie do następującej:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Setting a button's mirroring based on the layout direction
    var button = new NSButton ();
    if (button.UserInterfaceLayoutDirection == NSUserInterfaceLayoutDirection.LeftToRight) {
        button.Alignment = NSTextAlignment.Right;
        button.ImagePosition = NSCellImagePosition.ImageLeft;
    } else {
        button.Alignment = NSTextAlignment.Left;
        button.ImagePosition = NSCellImagePosition.ImageRight;
    }
}
```

Gdzie `Alignment` i `ImagePosition` są określane na podstawie `UserInterfaceLayoutDirection` formantu.

System macOS Sierra dodaje kilka nowych konstruktorów wygody (za pomocą statycznych `CreateButton` — metoda) czy podjąć kilka parametrów (takie jak tytuł, obrazu i akcji) i automatycznie zostanie poprawnie duplikatów. Na przykład:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>Za pomocą wystąpień systemu

System macOS nowoczesnych aplikacji może przyjąć nowy ciemny wygląd interfejsu, który dobrze aplikacji do tworzenia, edytowania i prezentacji obrazu:

[![](modern-cocoa-apps-images/content11.png "Przykład ciemny UI okna Mac")](modern-cocoa-apps-images/content11.png#lightbox)

Można to zrobić przed okna są prezentowane, dodając jeden wiersz kodu. Na przykład:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        ...
    
        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Apply the Dark Interface Appearance
            View.Window.Appearance = NSAppearance.GetAppearance (NSAppearance.NameVibrantDark);

            ...
        }
        #endregion
    }
}
```

Statycznych `GetAppearance` metody `NSAppearance` klasy jest używany do pobierania nazwanego wygląd z systemu (w tym przypadku `NSAppearance.NameVibrantDark`).

Apple ma poniższe sugestie dotyczące korzystania z wystąpień systemu:

- Preferuj nazwane kolory przed zapisane na stałe wartości (takie jak `LabelColor` i `SelectedControlColor`).
- Jeśli to możliwe, należy użyć stylu formantu standardowego systemu.

Dla użytkowników, którzy mają włączone funkcje ułatwień dostępu z poziomu aplikacji preferencjach systemowych aplikacji macOS, który używa wystąpień systemu będzie automatycznie działać poprawnie. W związku z tym Apple sugeruje, że deweloper zawsze należy używać systemu wyglądu w swoich aplikacjach macOS.

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>Projektowanie UI z Scenorys

Scenorys umożliwiają deweloperom nie tylko projektowy przepływu poszczególne elementy interfejsu użytkownika aplikacji, ale w celu wizualizacji i projektowanie interfejsu użytkownika, jak i hierarchii danego elementów.

Kontrolery umożliwiają deweloperom Zbieranie elementów na jednostkę układ i abstrakcyjny Segues i Usuń typowe "przyklej kodu" wymagane do przeniesienia w całej hierarchii widoku:

[![](modern-cocoa-apps-images/content12.png "Edytowanie interfejsu użytkownika w programie Xcode konstruktora interfejsu")](modern-cocoa-apps-images/content12.png#lightbox)

Aby uzyskać więcej informacji, zobacz nasze [wprowadzenie do scenorysu](~/mac/platform/storyboards/index.md) dokumentacji.

Istnieje wiele wystąpień, w którym danego sceny zdefiniowane w scenorysu wymaga danych z poprzedniej scenę w widoku hierarchii. Apple ma poniższe sugestie do przekazywania informacji między sceny:

- Dependancies danych należy zawsze kaskadową dół hierarchii.
- Unikaj hardcoding strukturalnych dependancies interfejsu użytkownika, jak ogranicza elastyczność interfejsu użytkownika.
- Użyj interfejsów języka C#, aby podać dependancies danych typu ogólnego.

Kontroler widoku, który działa jako źródło Segue, można zastąpić `PrepareForSegue` metody i nie inicjowanie przed Segue wymagane (takich jak przekazywanie danych) jest wykonywana do wyświetlenia docelowego kontrolera widoku. Na przykład:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

Aby uzyskać więcej informacji, zobacz nasze [Segues](~/mac/platform/storyboards/indepth.md#Segues) dokumentacji.

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>Propagowanie akcje

Oparte na projekt aplikacji macOS, mogą wystąpić razy podczas najlepsze obsługi akcji dla formantu interfejsu użytkownika może znajdować się w innym miejscu w hierarchii interfejsu użytkownika. Dotyczy to zwykle menu i elementów Menu, które znajdują się w ich własnych sceny oddzielnie od pozostałych Interfejsie użytkownika aplikacji.

Aby obsłużyć taką sytuację, deweloper można utworzyć akcji niestandardowej i przekazać akcji zapasowej łańcucha obiektu odpowiadającego w trybie. Aby uzyskać więcej informacji zobacz nasze [Praca z akcji niestandardowych okna](~/mac/user-interface/menu.md) dokumentacji.

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>Funkcje nowoczesnych Mac

Apple ma obejmuje kilka funkcji dla użytkownika system macOS Sierra umożliwiają deweloperom optymalne wykorzystanie platforma Mac, takich jak:

- **NSUserActivity** — umożliwia to aplikacji w celu opisania działania, które użytkownik jest obecnie używana w. `NSUserActivity` został utworzony do obsługi przekazaniem, gdy działanie uruchomione na jednym z urządzeń użytkownika można pobrana i nadal na innym urządzeniu. `NSUserActivity` działania jest taka sama w macOS, ponieważ w systemie iOS, dlatego należy Zobacz nasze [wprowadzenie do przekazaniem](~/ios/platform/handoff.md) więcej szczegółów zawiera dokumentacja systemu iOS.
- **Używanie programu Siri na Mac** -Siri używa bieżącego działania (`NSUserActivity`) zapewnienie kontekstu z poleceniami, które użytkownik może wydać.
- **Stan przywracania** — w przypadku aplikacji na macOS, a następnie kontynuować relaunches go, aplikacja będzie automatycznie zamykany użytkownika można zwrócić do jego poprzedniego stanu. Deweloper można użyć interfejsu API przywracania stanu do kodowania i przywracania przejściowej stanów interfejsu użytkownika, przed wyświetleniem interfejsu użytkownika dla użytkownika. Jeśli aplikacja jest `NSDocument` na podstawie, Przywracanie stanu odbywa się automatycznie. Aby włączyć Przywracanie stanu dla z systemem innym niż`NSDocument` aplikacji, ustaw `Restorable` z `NSWindow` klasa do `true`.
- **Dokumenty w chmurze** — przed macOS Sierra aplikacji musiał jawnie wyrazić zgodę na Praca z dokumentami w iCloud użytkownika dysku. W macOS Sierra użytkownika **pulpitu** i **dokumenty** folderów może być zsynchronizowane z ich iCloud dysku automatycznie przez system. W związku z tym lokalne kopie dokumenty mogą zostać usunięte, aby zwolnić miejsce na komputerze użytkownika. `NSDocument` aplikacje oparte na zostanie automatycznie obsłużyć tej zmiany. Wszystkie typy aplikacji, będą musieli używać `NSFileCoordinator` synchronizację odczytywanie i zapisywanie dokumentów.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego kilka porad, funkcje i techniki przez dewelopera umożliwia tworzenie aplikacji modern macOS w Xamarin.Mac.



## <a name="related-links"></a>Linki pokrewne

- [System macOS — przykłady](https://developer.xamarin.com/samples/mac/)
