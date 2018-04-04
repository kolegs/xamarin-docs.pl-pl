---
title: Projekt interfejsu użytkownika.Storyboard/.XIb-less
description: W tym artykule opisano tworzenie interfejsu użytkownika aplikacji Xamarin.Mac bezpośrednio z kodu C# bez .storyboard plików, pliki .xib lub konstruktora interfejsu.
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 66725b02d3e351e74fa79ae5336a7db3a9f2b534
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="storyboardxib-less-user-interface-design"></a>Projekt interfejsu użytkownika.Storyboard/.XIb-less

_W tym artykule opisano tworzenie interfejsu użytkownika aplikacji Xamarin.Mac bezpośrednio z kodu C# bez .storyboard plików, pliki .xib lub konstruktora interfejsu._


## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, mają dostęp do tego samego elementy interfejsu użytkownika i narzędzi, które Deweloper pracujący *Objective-C* i *Xcode* jest. Zwykle po utworzeniu aplikacji Xamarin.Mac użyjesz konstruktora interfejsu w środowisku Xcode z plikami .storyboard lub .xib można tworzyć i obsługiwać możesz interfejsu użytkownika aplikacji.

Masz również możliwość utworzenia niektórych lub wszystkich interfejsu użytkownika aplikacji Xamarin.Mac bezpośrednio w kodzie języka C#. W tym artykule firma Microsoft będzie obejmują podstawy tworzenia interfejsów użytkownika i elementy interfejsu użytkownika w kodzie języka C#.

[![Visual Studio dla edytora kodu Mac](xibless-ui-images/intro01.png "programu Visual Studio dla edytora kodu Mac")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>Przełączanie okno, aby użyć kodu

Podczas tworzenia nowej aplikacji Xamarin.Mac Cocoa, otrzymasz standardowe okno puste, domyślnie. Ten system windows jest zdefiniowany w **Main.storyboard** (lub tradycyjnie **MainWindow.xib**) pliku automatycznie dołączony do projektu. Obejmuje to także **ViewController.cs** pliku, który zarządza Widok główny aplikacji (lub ponownie tradycyjnie **MainWindow.cs** i **MainWindowController.cs** pliku).

Aby przełączyć się do okna Xibless dla aplikacji, wykonaj następujące czynności:

1. Otwórz aplikację, którą chcesz zaprzestać używania `.stroyboard` lub .xib plików można zdefiniować interfejs użytkownika w programie Visual Studio dla komputerów Mac.
2. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **Main.storyboard** lub **MainWindow.xib** plik i wybierz **Usuń**: 

    ![Usunięcie głównego storyboard lub okna](xibless-ui-images/switch01.png "usunięcie głównego storyboard lub okna")
3. Z **Usuń okna dialogowego**, kliknij przycisk **usunąć** przycisk, aby całkowicie usunąć .storyboard lub .xib z projektu: 

    ![Potwierdzenie usunięcia](xibless-ui-images/switch02.png "potwierdzenie usunięcia")

Teraz musisz zmodyfikować **MainWindow.cs** pliku do definiowania układu okna i zmodyfikować **ViewController.cs** lub **MainWindowController.cs** pliku w celu utworzenia wystąpienie naszych `MainWindow` klasy, ponieważ firma Microsoft nie korzystają już pliku .storyboard lub .xib.

Nowoczesne aplikacje Xamarin.Mac używających Scenorys dla interfejsu użytkownika nie może zawierać automatycznie **MainWindow.cs**, **ViewController.cs** lub **MainWindowController.cs** plików. Zgodnie z wymaganiami, po prostu Dodaj nowy pusty C# klasę do projektu (**Dodaj** > **nowego pliku...**   >  **Ogólne** > **pustą klasę**) i nadaj mu nazwę taka sama jak brakujący plik. 


### <a name="defining-the-window-in-code"></a>Definiowanie okna w kodzie

Następnie Edytuj **MainWindow.cs** pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindow : NSWindow
    {
        #region Private Variables
        private int NumberOfTimesClicked = 0;
        #endregion

        #region Computed Properties
        public NSButton ClickMeButton { get; set;}
        public NSTextField ClickMeLabel { get ; set;}
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }

        public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
            // Define the user interface of the window here
            Title = "Window From Code";

            // Create the content view for the window and make it fill the window
            ContentView = new NSView (Frame);

            // Add UI elements to window
            ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
                AutoresizingMask = NSViewResizingMask.MinYMargin
            };
            ContentView.AddSubview (ClickMeButton);

            ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
                BackgroundColor = NSColor.Clear,
                TextColor = NSColor.Black,
                Editable = false,
                Bezeled = false,
                AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
                StringValue = "Button has not been clicked yet."
            };
            ContentView.AddSubview (ClickMeLabel);
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Wireup events
            ClickMeButton.Activated += (sender, e) => {
                // Update count
                ClickMeLabel.StringValue = (++NumberOfTimesClicked == 1) ? "Button clicked one time." : string.Format("Button clicked {0} times.",NumberOfTimesClicked);
            };
        }
        #endregion

    }
}
```

Omówimy niektóre z kluczowych elementów.

Po pierwsze, dodano kilka _właściwości obliczanej_ który będzie działać tak jak gniazda (tak, jakby okna został utworzony w pliku .storyboard lub .xib):

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

To zapewni nam dostęp do elementów interfejsu użytkownika, które zamierzamy do wyświetlenia w oknie. Ponieważ nie jest on zwiększony okna z pliku .storyboard lub .xib, potrzebujemy sposób utworzyć (jak zajmiemy się tym później w `MainWindowController` klasy). Jest to nowa metoda Konstruktor jest:

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

To gdzie Zapoznamy zaprojektować układ okna i umieścić wszelkie potrzebne do utworzenia interfejsu użytkownika wymagane elementy interfejsu użytkownika. Zanim wszystkie elementy interfejsu użytkownika można dodać do okna, musi on _widok zawartości_ zawiera elementy:

```csharp
ContentView = new NSView (Frame);
```

Spowoduje to utworzenie widok zawartości, który będzie Wypełnianie okna. Teraz możemy dodać nasze pierwszego elementu interfejsu użytkownika `NSButton`, w oknie:

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

Najpierw należy pamiętać, w tym miejscu jest, że w przeciwieństwie do systemu iOS, system macOS używa notacji matematyczne do definiowania układ współrzędnych okna. Dlatego punktem początkowym jest w dolnym rogu po lewej stronie okna o zwiększenie wartości prawo i kierunku prawym górnym rogu okna. Gdy utworzymy nowe `NSButton`, możemy uwzględnienie to definiujemy jego położenie i rozmiar na ekranie.

`AutoresizingMask = NSViewResizingMask.MinYMargin` Właściwości informuje przycisku chcemy pozostać w tej samej lokalizacji, w górnej części okna, gdy okno pionie zmieni się rozmiar. Ponownie, są wymagane, ponieważ (0,0) w lewym dolnym rogu okna.

Na koniec `ContentView.AddSubview (ClickMeButton)` dodaje metody `NSButton` do widoku zawartości, którą będzie wyświetlany na ekranie, gdy aplikacja jest uruchamiania i okno.

Obok etykiety jest dodawany do okna, która będzie wyświetlana liczba który `NSButton` zostanie kliknięta: 

```csharp
ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
    BackgroundColor = NSColor.Clear,
    TextColor = NSColor.Black,
    Editable = false,
    Bezeled = false,
    AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
    StringValue = "Button has not been clicked yet."
};
ContentView.AddSubview (ClickMeLabel);
``` 

Ponieważ system macOS nie ma określonego _etykiety_ elementu interfejsu użytkownika dodano specjalnie styl, nie można edytować `NSTextField` do działania jako etykieta. Jak przycisk przed rozmiar i położenie uwzględnia tylko tym (0,0) znajduje się w lewym dolnym rogu okna. `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` Używa właściwości **lub** operatora, aby połączyć dwie `NSViewResizingMask` funkcji. Spowoduje to etykieta pozostać w tej samej lokalizacji, w górnej części okna, gdy okno pionie zmieni się rozmiar i zmniejszyć rozmiar i zwiększa się szerokość jako poziomie rozmiaru okna.

Ponownie `ContentView.AddSubview (ClickMeLabel)` dodaje metody `NSTextField` do widoku zawartości, tak że będą wyświetlane na ekranie po uruchomieniu aplikacji i otworzyć okno.


### <a name="adjusting-the-window-controller"></a>Dopasowywanie kontrolera okna

Od czasu projektowania `MainWindow` jest już załadowany z pliku .storyboard lub .xib, trzeba będzie dostosować do kontrolera okna. Edytuj **MainWindowController.cs** pliku i zapewnić ich wyglądać następująco:

```csharp
using System;

using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindowController : NSWindowController
    {
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindowController (NSCoder coder) : base (coder)
        {
        }

        public MainWindowController () : base ("MainWindow")
        {
            // Construct the window from code here
            CGRect contentRect = new CGRect (0, 0, 1000, 500);
            base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);

            // Simulate Awaking from Nib
            Window.AwakeFromNib ();
        }

        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }

        public new MainWindow Window {
            get { return (MainWindow)base.Window; }
        }
    }
}

```

Let omówiono kluczowe elementy tej zmiany.

Po pierwsze, definiujemy nowe wystąpienie klasy `MainWindow` klasy i przypisz go do kontrolera podstawowego okna `Window` właściwości:

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

Definiujemy lokalizacji okna ekranu z `CGRect`. Podobnie jak współrzędnych okna ekranu definiuje (0,0) jako dolny róg po lewej stronie. Następnie definiujemy styl okna przy użyciu **lub** operatora, aby połączyć dwie lub więcej `NSWindowStyle` funkcje:

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

Następujące `NSWindowStyle` funkcje są dostępne:

- **Bez obramowania** -okna nie odniesie brak obramowania.
- **Zatytułowany** — okno ma paska tytułu.
- **Zamykanych** — okno ma przycisk Zamknij i może zostać zamknięty.
- **Miniaturizable** — okno ma przycisk Miniaturize i może być minimalny.
- **Zmienny rozmiar** — Zmień rozmiar przycisku i być zmienny rozmiar okna.
- **Narzędzie** — okno jest oknem styl narzędzia (panel).
- **DocModal** — okno jest panelu, będzie dokumentu modalne zamiast modalne systemowe. 
- **NonactivatingPanel** — Jeśli okno jest panelu, go nie będzie nawiązywane głównego okna.
- **TexturedBackground** — okno będzie miał teksturą tło.
- **Nieskalowanego** -okna nie będzie skalowana.
- **UnifiedTitleAndToolbar** -dołączy obszarów tytuł i paska narzędzi okna.
- **Hud** -okna, które będą wyświetlane jako ekran projekcyjny wskazuje pozycję panelu.
- **FullScreenWindow** -okna można wprowadzić w trybie pełnoekranowym.
- **FullSizeContentView** — widok zawartości okna znajduje się za tytuł i obszar paska narzędzi.

Definiowanie właściwości ostatnich dwóch _buforowanie typu_ okna i jeśli zostanie odłożone rysunku okna. Aby uzyskać więcej informacji na temat `NSWindows`, można znaleźć pod adresem firmy Apple [wprowadzenie do systemu Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) dokumentacji.

Na koniec, ponieważ nie jest on zwiększony okna z pliku .storyboard lub .xib, musimy symulować w naszym **MainWindowController.cs** przez wywołanie systemu windows `AwakeFromNib` metody:

```csharp
Window.AwakeFromNib ();
```

Dzięki temu będzie się, że na kod tylko okna, takich jak standardowe okno załadowanego z pliku .storyboard lub .xib.


### <a name="displaying-the-window"></a>Wyświetlanie okna

Przy użyciu pliku .storyboard lub .xib usunięte i **MainWindow.cs** i **MainWindowController.cs** pliki zmodyfikowane, należy używać okna tak samo, jak normalnym oknie, które zostały utworzone w W środowisku Xcode interfejsu konstruktora z plikiem .xib.

Następujące będzie utworzyć nowe wystąpienie klasy okna i jego kontrolera i wyświetlanie okna na ekranie:

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

W tym momencie Jeśli aplikacja jest uruchamiana i kliknięciu przycisku kilka razy, poniżej zostanie wyświetlony:

![Uruchom przykładową aplikację](xibless-ui-images/run01.png "Uruchom przykładową aplikację")


## <a name="adding-a-code-only-window"></a>Dodawanie okna tylko kodu

Jeśli chcemy dodać do istniejącej aplikacji Xamarin.Mac kod tylko, okno xibless, kliknij prawym przyciskiem myszy projekt w **konsoli rozwiązania** i wybierz **Dodaj** > **nowy plik.** . W **nowy plik** wybierz w oknie dialogowym **Xamarin.Mac** > **Cocoa okna z kontrolera**, jak pokazano poniżej:

![Dodawanie nowego kontrolera okna](xibless-ui-images/add01.png "dodawania nowego kontrolera okna") 

Podobnie jak wcześniej, będzie usunąć domyślny plik .storyboard lub .xib z projektu (w tym przypadku **SecondWindow.xib**) i postępuj zgodnie z instrukcjami [przełączanie okno, aby użyć kodu](#Switching_a_Window_to_use_Code) sekcji powyżej, aby obejmować Definicja okna kodu.


## <a name="adding-a-ui-element-to-a-window-in-code"></a>Dodawanie elementu interfejsu użytkownika do okna w kodzie

Czy okno został utworzony w kodzie lub załadowanego z pliku .storyboard lub .xib, mogą wystąpić razy gdy chcemy, aby dodać element interfejsu użytkownika do okna z kodu. Na przykład:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

Powyższy kod tworzy nową `NSButton` i dodaje go do `MyWindow` wystąpienia okna do wyświetlenia. Zasadniczo elementów interfejsu użytkownika, które można definiować w środowisku Xcode konstruktora interfejsu w pliku .storyboard lub .xib można tworzyć w kodzie i wyświetlane w oknie.


## <a name="defining-the-menu-bar-in-code"></a>Definiowanie na pasku menu w kodzie

Ze względu na bieżące ograniczenia w Xamarin.Mac, nie zaleca się utworzenie aplikacji Xamarin.Mac Menu paska —`NSMenuBar`— w kodzie, ale nadal używać **Main.storyboard** lub **MainMenu.xib** pliku definiować go. Inaczej mówiąc, można dodawać i usuwać elementy Menu i menu w kodzie języka C#.

Na przykład edytować **AppDelegate.cs** plików i wprowadź `DidFinishLaunching` wygląd metody podobne do następujących:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    // Create a Status Bar Menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Phrases";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Phrases");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        Console.WriteLine("Address Selected");
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        Console.WriteLine("Date Selected");
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        Console.WriteLine("Greetings Selected");
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        Console.WriteLine("Signature Selected");
    };
    item.Menu.AddItem (signature);
}
```

Powyższe tworzy menu paska stanu z kodu i wyświetla je, gdy aplikacja jest uruchamiana. Aby uzyskać więcej informacji na temat pracy z menu, zobacz nasze [menu](~/mac/user-interface/menu.md) dokumentacji.


## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowy widok w tworzeniu interfejsu użytkownika aplikacji Xamarin.Mac w kodzie C#, a nie za pomocą konstruktora interfejsu w środowisku Xcode z plikami .storyboard lub .xib.



## <a name="related-links"></a>Linki pokrewne

- [MacXibless (przykład)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Windows](~/mac/user-interface/window.md)
- [Menu](~/mac/user-interface/menu.md)
- [System macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Wprowadzenie do systemu Windows](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
