---
title: Skopiuj i Wklej platformy Xamarin.Mac
description: Ten artykuł dotyczy pracy z obszar roboczy, aby zapewnić kopiowania i Wklej w aplikacji platformy Xamarin.Mac. Pokazuje, jak pracować z typami danych w warstwie standardowa, które mogą być współużytkowane między wiele aplikacji i sposób obsługi danych niestandardowych w ramach danej aplikacji.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 728e0264f7da8f3adfef360dd473772dd7e28a11
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780647"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Skopiuj i Wklej platformy Xamarin.Mac

_Ten artykuł dotyczy pracy z obszar roboczy, aby zapewnić kopiowania i Wklej w aplikacji platformy Xamarin.Mac. Pokazuje, jak pracować z typami danych w warstwie standardowa, które mogą być współużytkowane między wiele aplikacji i sposób obsługi danych niestandardowych w ramach danej aplikacji._

## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tego samego stołu montażowego (kopiowanie i wklejanie) wsparcia dla deweloperów pracujących w języku Objective-C ma.

W tym artykule firma Microsoft będzie obejmował dwa główne sposoby używania obszar roboczy w aplikacji platformy Xamarin.Mac:

1. **Standardowe typy danych** — ponieważ stołu montażowego operacje są zazwyczaj wykonywane między dwiema aplikacjami niepowiązanych, żadna aplikacja zna typy danych obsługującą przez innych. Aby zmaksymalizować potencjał do udostępniania, obszar roboczy może zawierać wiele reprezentacji danego elementu (przy użyciu standardowego zestawu standardowe typy danych), dzięki temu aplikacja odbierająca komunikaty i wybierz wersję, która najlepiej nadaje się do swoich potrzeb.
2. **Dane niestandardowe** — do obsługi kopiowania i wklejania danych złożonych w Twojej platformy Xamarin.Mac, można zdefiniować typ danych niestandardowych, który będzie obsługiwany przez obszar roboczy. Na przykład w aplikacji rysowania wektor umożliwiająca użytkownikowi kopiowanie i wklejanie kształtów złożonych, które składają się z wielu typów danych i punktów.

[![Przykład uruchomionej aplikacji](copy-paste-images/intro01.png "przykład uruchomionej aplikacji")](copy-paste-images/intro01-large.png#lightbox)

W tym artykule omówimy podstawy pracy z obszaru wklejania w aplikacji platformy Xamarin.Mac obsługuje kopiowania i wklejania. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` atrybutów Umożliwia podłączanie klas języka C# do języka Objective-C obiektów i interfejsu użytkownika elementów.

## <a name="getting-started-with-the-pasteboard"></a>Wprowadzenie do obszaru roboczego

Obszar roboczy wyświetla standardowy mechanizm wymiany danych w ramach danej aplikacji lub między aplikacjami. Zwykle dla obszaru wklejania w aplikacji platformy Xamarin.Mac jest używane do obsługi kopiowania i wklejania, jednak wiele innych operacji są również obsługiwane (na przykład przeciągania i upuszczania oraz usług w aplikacji).

Szybko można na wyższy, użyjemy można uruchomić przy użyciu prostego, praktyczne wprowadzenie do korzystania z większa w aplikacji platformy Xamarin.Mac. Później firma Microsoft udostępni szczegółowe wyjaśnienie działania obszar roboczy i metod.

W tym przykładzie utworzymy aplikację prostych dokumentów na podstawie, które zarządzają okno zawierające widok obrazu. Użytkownik będzie do kopiowania i wklejania obrazów między dokumentami w aplikacji oraz lub z innych aplikacji lub wiele okien w tej samej aplikacji.

### <a name="creating-the-xamarin-project"></a>Tworzenie projektu platformy Xamarin

Najpierw pobierzemy Tworzenie nowej aplikacji platformy Xamarin.Mac dokumentów na podstawie, firma Microsoft można Dodawanie kopiowania i Wklej obsługę.

Wykonaj następujące czynności:

1. Uruchom program Visual Studio dla komputerów Mac i kliknij przycisk **nowy projekt...**  łącza.
2. Wybierz **Mac** > **aplikacji** > **aplikacja cocoa dla**, następnie kliknij przycisk **dalej** przycisku: 

    [![Tworzenie nowego projektu aplikacji Cocoa](copy-paste-images/sample01.png "tworzenia nowego projektu aplikacja Cocoa")](copy-paste-images/sample01-large.png#lightbox)
3. Wprowadź `MacCopyPaste` dla **Nazwa projektu** i zachować wszystkie inne jako domyślny. Kliknij przycisk Dalej: 

    [![Ustawianie nazwy projektu](copy-paste-images/sample01a.png "ustawienie nazwy projektu")](copy-paste-images/sample01a-large.png#lightbox)

4. Kliknij przycisk **Utwórz** przycisku: 

    [![Trwa Potwierdzanie ustawienia nowego projektu](copy-paste-images/sample02.png "potwierdzenie ustawienia nowego projektu")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>Dodawanie interfejsu NSDocument

Następnie dodamy niestandardowy `NSDocument` klasy, który będzie pełnił rolę magazynu tło dla aplikacji interfejsu użytkownika. Będzie ona zawierać pojedynczy widok obrazu i wiedzieć, jak skopiować obraz z widoku w obszarze roboczym domyślne i jak wykonać obrazu z obszaru wklejania domyślne i wyświetlania ich w widoku obrazu.

Kliknij prawym przyciskiem myszy nad projektem platformy Xamarin.Mac w **konsoli rozwiązania** i wybierz **Dodaj** > **nowy plik...** :

![Dodawanie interfejsu NSDocument dla projektu](copy-paste-images/sample03.png "dodanie interfejsu NSDocument do projektu")

Wprowadź `ImageDocument` dla **nazwa** i kliknij przycisk **New** przycisku. Edytuj **ImageDocument.cs** klasy i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using AppKit;
using Foundation;
using ObjCRuntime;

namespace MacCopyPaste
{
    [Register("ImageDocument")]
    public class ImageDocument : NSDocument
    {
        #region Computed Properties
        public NSImageView ImageView {get; set;}

        public ImageInfo Info { get; set; } = new ImageInfo();

        public bool ImageAvailableOnPasteboard {
            get {
                // Initialize the pasteboard
                NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
                Class [] classArray  = { new Class ("NSImage") };

                // Check to see if an image is on the pasteboard
                return pasteboard.CanReadObjectForClasses (classArray, null);
            }
        }
        #endregion

        #region Constructor
        public ImageDocument ()
        {
        }
        #endregion

        #region Public Methods
        [Export("CopyImage:")]
        public void CopyImage(NSObject sender) {

            // Grab the current image
            var image = ImageView.Image;

            // Anything to process?
            if (image != null) {
                // Get the standard pasteboard
                var pasteboard = NSPasteboard.GeneralPasteboard;

                // Empty the current contents
                pasteboard.ClearContents();

                // Add the current image to the pasteboard
                pasteboard.WriteObjects (new NSImage[] {image});

                // Save the custom data class to the pastebaord
                pasteboard.WriteObjects (new ImageInfo[] { Info });

                // Using a Pasteboard Item
                NSPasteboardItem item = new NSPasteboardItem();
                string[] writableTypes = {"public.text"};

                // Add a data provier to the item
                ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
                var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

                // Save to pasteboard
                if (ok) {
                    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
                }
            }

        }

        [Export("PasteImage:")]
        public void PasteImage(NSObject sender) {

            // Initialize the pasteboard
            NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
            Class [] classArray  = { new Class ("NSImage") };

            bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
                NSImage image = (NSImage)objectsToPaste[0];

                // Display the new image
                ImageView.Image = image;
            }

            Class [] classArray2 = { new Class ("ImageInfo") };
            ok = pasteboard.CanReadObjectForClasses (classArray2, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
                ImageInfo info = (ImageInfo)objectsToPaste[0];

            }

        }
        #endregion
    }
}
```

Spójrzmy na kilka kodu poniżej.

Poniższy kod zawiera właściwości do sprawdzania istnienia danych obrazu w obszarze roboczym domyślne, jeśli obraz jest dostępny, `true` jest zwracany w przeciwnym razie `false`:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

Poniższy kod kopiuje obraz z widoku dołączonego obrazu, w obszarze roboczym domyślne:

```csharp
[Export("CopyImage:")]
public void CopyImage(NSObject sender) {

    // Grab the current image
    var image = ImageView.Image;

    // Anything to process?
    if (image != null) {
        // Get the standard pasteboard
        var pasteboard = NSPasteboard.GeneralPasteboard;

        // Empty the current contents
        pasteboard.ClearContents();

        // Add the current image to the pasteboard
        pasteboard.WriteObjects (new NSImage[] {image});

        // Save the custom data class to the pastebaord
        pasteboard.WriteObjects (new ImageInfo[] { Info });

        // Using a Pasteboard Item
        NSPasteboardItem item = new NSPasteboardItem();
        string[] writableTypes = {"public.text"};

        // Add a data provider to the item
        ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
        var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

        // Save to pasteboard
        if (ok) {
            pasteboard.WriteObjects (new NSPasteboardItem[] { item });
        }
    }

}
```

I następujący kod wklejania obrazów z obszaru wklejania domyślne i wyświetla go w widoku dołączonego obrazu (Jeśli obszar roboczy nie zawiera prawidłowego obrazu):

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0]
    }
}
```

Z tym dokumentem w miejscu utworzymy interfejsu użytkownika dla aplikacji platformy Xamarin.Mac.

### <a name="building-the-user-interface"></a>Tworzenie interfejsu użytkownika

Kliknij dwukrotnie **Main.storyboard** plik, aby otworzyć go w środowisku Xcode. Następnie dodaj pasek narzędzi i obraz oraz i skonfigurować je w następujący sposób:

[![Pasek narzędzi edycji](copy-paste-images/sample04.png "edycji na pasku narzędzi")](copy-paste-images/sample04-large.png#lightbox)

Dodaj kopię i Wklej **element paska narzędzi obrazów** po lewej stronie, na pasku narzędzi. Będziemy używać je jako skróty do kopiowania i wklejania z menu Edycja. Następnie dodaj cztery **elementów paska narzędzi obrazów** prawą stronę paska narzędzi. Użyjemy tych do wypełniania obraz dobrze w przypadku niektórych domyślnych obrazów.

Aby uzyskać więcej informacji na temat pracy z pasków narzędzi, zobacz nasze [pasków narzędzi](~/mac/user-interface/toolbar.md) dokumentacji.

Następnie możemy również udostępnić następujące gniazd i akcje w przypadku naszych elementów paska narzędzi i obrazu:

[![Tworzenie gniazda i działań](copy-paste-images/sample05.png "Tworzenie gniazda i działań")](copy-paste-images/sample05-large.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z gniazda i akcji, zobacz [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) części naszych [Witaj, Mac](~/mac/get-started/hello-mac.md) dokumentacji.

### <a name="enabling-the-user-interface"></a>Włączanie interfejsu użytkownika

Za pomocą interfejsu użytkownika tworzone w Xcode i naszych elementu interfejsu użytkownika, udostępniane za pośrednictwem gniazd i akcji należy dodać kod, aby włączyć interfejs użytkownika. Kliknij dwukrotnie **ImageWindow.cs** w pliku **konsoli rozwiązania** i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCopyPaste
{
    public partial class ImageWindow : NSWindow
    {
        #region Private Variables
        ImageDocument document;
        #endregion

        #region Computed Properties
        [Export ("Document")]
        public ImageDocument Document {
            get {
                return document;
            }
            set {
                WillChangeValue ("Document");
                document = value;
                DidChangeValue ("Document");
            }
        }

        public ViewController ImageViewController {
            get { return ContentViewController as ViewController; }
        }

        public NSImage Image {
            get {
                return ImageViewController.Image;
            }
            set {
                ImageViewController.Image = value;
            }
        }
        #endregion

        #region Constructor
        public ImageWindow (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create a new document instance
            Document = new ImageDocument ();

            // Attach to image view
            Document.ImageView = ImageViewController.ContentView;
        }
        #endregion

        #region Public Methods
        public void CopyImage (NSObject sender)
        {
            Document.CopyImage (sender);
        }

        public void PasteImage (NSObject sender)
        {
            Document.PasteImage (sender);
        }

        public void ImageOne (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image01.jpg");

            // Set image info
            Document.Info.Name = "city";
            Document.Info.ImageType = "jpg";
        }

        public void ImageTwo (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image02.jpg");

            // Set image info
            Document.Info.Name = "theater";
            Document.Info.ImageType = "jpg";
        }

        public void ImageThree (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image03.jpg");

            // Set image info
            Document.Info.Name = "keyboard";
            Document.Info.ImageType = "jpg";
        }

        public void ImageFour (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image04.jpg");

            // Set image info
            Document.Info.Name = "trees";
            Document.Info.ImageType = "jpg";
        }
        #endregion
    }
}
```

Spójrzmy na ten kod poniżej.

Po pierwsze, możemy ujawnić wystąpienie `ImageDocument` klasę, która utworzone powyżej:

```csharp
private ImageDocument _document;
...

[Export ("Document")]
public ImageDocument Document {
    get { return _document; }
    set {
        WillChangeValue ("Document");
        _document = value;
        DidChangeValue ("Document");
    }
}
```

Za pomocą `Export`, `WillChangeValue` i `DidChangeValue`, mamy Instalatora `Document` właściwości, aby umożliwić kodowanie klucz-wartość i powiązania danych w środowisku Xcode.

Możemy również ujawnić obrazu z obrazu, oraz dodaliśmy do naszego interfejsu użytkownika w środowisku Xcode z następującymi właściwościami:

```csharp
public ViewController ImageViewController {
    get { return ContentViewController as ViewController; }
}

public NSImage Image {
    get {
        return ImageViewController.Image;
    }
    set {
        ImageViewController.Image = value;
    }
}
```

Gdy okno główne jest ładowany i wyświetlane, możemy utworzyć wystąpienia naszej `ImageDocument` klasy i dołączyć obraz w Interfejsie użytkownika oraz do niego następujący kod:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create a new document instance
    Document = new ImageDocument ();

    // Attach to image view
    Document.ImageView = ImageViewController.ContentView;
}
```

Na koniec w odpowiedzi na użytkownika, klikając elementy paska narzędzi kopiowania i wklejania, nazywamy wystąpienie `ImageDocument` klasy wykonują rzeczywistą pracę:

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Włączanie menu Plik i Edytuj

Włączone jest ostatnim zadaniem, które musimy **New** element menu z **pliku** menu (w celu utworzenia nowych wystąpień nasze główne okno) oraz w celu umożliwienia **Wytnij**, **kopiowania**  i **Wklej** menu elementów z **Edytuj** menu.

Aby włączyć **New** menu elementów, Edytuj **AppDelegate.cs** pliku i Dodaj następujący kod:

```csharp
public int UntitledWindowCount { get; set;} =1;
...

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

Aby uzyskać więcej informacji, zobacz [Praca z wieloma Windows](~/mac/user-interface/window.md) części naszych [Windows](~/mac/user-interface/window.md) dokumentacji.

Aby włączyć **Wytnij**, **kopiowania** i **Wklej** elementów menu Edycja **AppDelegate.cs** pliku i Dodaj następujący kod:

```csharp
[Export("copy:")]
void CopyImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);
}

[Export("cut:")]
void CutImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);

    // Clear the existing image
    window.Image = null;
}

[Export("paste:")]
void PasteImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Paste the image from the clipboard
    window.Document.PasteImage (sender);
}
```

Dla każdego elementu menu, możemy uzyskać okna bieżącego, najwyższego poziomu, klucza i Rzutuj ją na naszych `ImageWindow` klasy:

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

W tym miejscu nazywamy `ImageDocument` wystąpienie klasy tego okna do obsługi kopiowania i wklejania działań. Na przykład: 

```csharp
window.Document.CopyImage (sender);
```

Ma być uruchamiany tylko **Wytnij**, **kopiowania** i **Wklej** elementów menu była dostępna, jeśli istnieje obraz danych w obszarze roboczym domyślne lub obraz oraz bieżące aktywne okno.

Dodajmy **EditMenuDelegate.cs** plik do projektu rozszerzenia Xamarin.Mac i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using AppKit;

namespace MacCopyPaste
{
    public class EditMenuDelegate : NSMenuDelegate
    {
        #region Override Methods
        public override void MenuWillHighlightItem (NSMenu menu, NSMenuItem item)
        {
        }

        public override void NeedsUpdate (NSMenu menu)
        {
            // Get list of menu items
            NSMenuItem[] Items = menu.ItemArray ();

            // Get the key window and determine if the required images are available
            var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
            var hasImage = (window != null) && (window.Image != null);
            var hasImageOnPasteboard = (window != null) && window.Document.ImageAvailableOnPasteboard;

            // Process every item in the menu
            foreach(NSMenuItem item in Items) {
                // Take action based on the menu title
                switch (item.Title) {
                case "Cut":
                case "Copy":
                case "Delete":
                    // Only enable if there is an image in the view
                    item.Enabled = hasImage;
                    break;
                case "Paste":
                    // Only enable if there is an image on the pasteboard
                    item.Enabled = hasImageOnPasteboard;
                    break;
                default:
                    // Only enable the item if it has a sub menu
                    item.Enabled = item.HasSubmenu;
                    break;
                }
            }
        }
        #endregion
    }
}
```

Ponownie, możemy uzyskać bieżące okien najwyższego poziomu oraz jego `ImageDocument` wystąpienia klasy, aby zobaczyć, czy istnieje danych wymagane obrazu. Następnie użyjemy `MenuWillHighlightItem` metodę, aby włączyć lub wyłączyć poszczególne elementy na podstawie tego stanu.

Edytuj **AppDelegate.cs** pliku i upewnij `DidFinishLaunching` wygląd metoda podobne do następującego:
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

Po pierwsze możemy wyłączyć automatyczne włączanie i wyłączanie elementów menu w menu Edycja. Następnie dołączamy wystąpienie `EditMenuDelegate` klasę, która utworzone powyżej.

Aby uzyskać więcej informacji, zobacz nasze [menu](~/mac/user-interface/menu.md) dokumentacji.

### <a name="testing-the-app"></a>Testowanie aplikacji

Wszystko w miejscu możemy przystąpić do testowania aplikacji. Tworzenie i uruchomić aplikację i jest wyświetlany interfejs główne:

![Uruchomiona jest aplikacja](copy-paste-images/run01.png "uruchomiona jest aplikacja")

Po otwarciu menu Edycja, należy pamiętać, że **Wytnij**, **kopiowania** i **Wklej** są wyłączone, ponieważ nie ma żadnego obrazu na obrazie, dobrze lub w obszarze roboczym domyślne:

![Otwieranie menu Edycja](copy-paste-images/run02.png "otwierając menu Edycja")

Jeśli możesz również dodać obraz do obrazu i ponownie otworzyć menu Edycja, zostanie ono włączone elementy:

![Wyświetlanie elementów menu Edycja są włączone](copy-paste-images/run03.png "wyświetlanie elementów menu Edycja są włączone")

Jeśli kopiowanie obrazu i wybierz pozycję **New** z menu Plik tego obrazu można wkleić w nowym oknie:

![Wklejanie obrazu w nowym oknie](copy-paste-images/run04.png "wklejania obrazu w nowym oknie")

W poniższych sekcjach przeniesiemy szczegółowe omówienie pracy z obszaru wklejania w aplikacji platformy Xamarin.Mac.

## <a name="about-the-pasteboard"></a>O obszarze roboczym

W systemie macOS (wcześniej znane jako OS X) obszar roboczy (`NSPasteboard`) umożliwia obsługę kilku serwer przetwarza, takich jak kopiowanie i wklejanie, przeciąganie i upuszczanie i usługi aplikacji. W poniższych sekcjach przeniesiemy dokładniej poznać kilka podstawowych pojęć stołu montażowego.

### <a name="what-is-a-pasteboard"></a>Co to jest obszar roboczy?

`NSPasteboard` Klasa udostępnia standardowy mechanizm wymiany informacji między aplikacjami lub w ramach danej aplikacji. Główna funkcja obszar roboczy jest obsługi operacji kopiowania i wklejania:

1. Gdy użytkownik zaznaczy element w aplikacji i używa **Wytnij** lub **kopiowania** element menu reprezentacje co najmniej jeden wybrany element są umieszczane w obszarze roboczym.
2. Gdy użytkownik używa **Wklej** elementu menu (w ramach tej samej aplikacji lub inną), wersja danych, która może obsłużyć jest kopiowane z obszaru roboczego i dodana do aplikacji.

Mniej oczywistych zastosowania stołu montażowego obejmują Znajdź przeciągania przeciąganie i upuszczanie i operacje usługi aplikacji:

- Gdy użytkownik zainicjuje operację przeciągania, przeciągnij danych jest kopiowany do obszarze roboczym. Jeśli operacja przeciągania kończy się listy na inną aplikację, tej aplikacji kopiuje dane z obszaru roboczego.
- Usługi tłumaczenia dane, które ma zostać poddany translacji są kopiowane do obszar roboczy za pomocą żądania aplikacji. Usługi aplikacji, pobiera dane z obszaru roboczego, jest translacji, a następnie past danych z powrotem w obszarze roboczym.

W najprostszej postaci większa są używane do przenoszenia danych w danej aplikacji lub między aplikacjami i do nich istnieje w obszarze specjalnych pamięci globalnej poza procesem aplikacji. Pojęcia związane z większa są łatwo grasps, istnieje kilka bardziej złożonych szczegółowe informacje, które muszą być rozważone. Będą one opisane poniżej.

### <a name="named-pasteboards"></a>Większa o nazwie

Obszar roboczy może być publicznym lub prywatnym i może służyć do różnych celów, w ramach aplikacji lub między wieloma aplikacjami. System macOS udostępnia kilka standardowych większa, każdy z użyciem określonych, dobrze zdefiniowany:

- `NSGeneralPboard` — W obszarze roboczym domyślny dla **Wytnij**, **kopiowania** i **Wklej** operacji.
- `NSRulerPboard` — Obsługuje **Wytnij**, **kopiowania** i **Wklej** operacji na **linijki**.
- `NSFontPboard` — Obsługuje **Wytnij**, **kopiowania** i **Wklej** operacji na `NSFont` obiektów.
- `NSFindPboard` — Obsługuje specyficzne dla aplikacji Znajdź paneli, które mogą udostępniać tekst wyszukiwania.
- `NSDragPboard` — Obsługuje **przeciąganie i upuszczanie** operacji.

W większości sytuacji użyjesz jeden większa zdefiniowane przez system. Jednak może to być sytuacje, w których konieczna utworzyć własne większa. W takich sytuacjach można używać `FromName (string name)` metody `NSPasteboard` klasy w celu utworzenia niestandardowych obszar roboczy o podanej nazwie.

Ewentualnie możesz wywołać `CreateWithUniqueName` metody `NSPasteboard` klasy w celu utworzenia unikatowej nazwie obszar roboczy.

### <a name="pasteboard-items"></a>Stołu montażowego elementów

Każda część danych, które aplikacja zapisuje obszar roboczy jest uważany za _elementu obszar roboczy_ i obszar roboczy może zawierać wiele elementów w tym samym czasie. W ten sposób aplikacja może zapisu wielu wersji danych kopiowane do obszaru roboczego, (na przykład zwykły tekst i tekstu sformatowanego) i pobieranie aplikacji mogą odczytać tylko te dane, mógł on przetwarzać (np. tylko zwykły tekst).

### <a name="data-representations-and-uniform-type-identifiers"></a>Reprezentacje danych i identyfikatory uniform typów

Stołu montażowego operacje trwają zwykle aplikacji między dwoma (lub więcej), które nie znają wzajemnie lub typów danych każdy może obsłużyć. Jak wspomniano w poprzedniej sekcji, aby zmaksymalizować potencjał do udostępniania informacji, obszar roboczy może zawierać wiele reprezentacji danych są skopiowane i wklejone.

Każdy reprezentacja jest identyfikowany za pomocą jednolitego typ identyfikator (identyfikator UTI), czyli nic więcej niż prosty ciąg, który unikatowo identyfikuje typ daty jest prezentowana (Aby uzyskać więcej informacji, zobacz firmy Apple [jednolitego Przegląd identyfikatorów typu ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) dokumentacji). 

Jeśli tworzysz niestandardowy typ danych (na przykład rysowania obiektu w wektorze aplikacji rysowania), możesz utworzyć własny identyfikator UTI, aby jednoznacznie zidentyfikować je w kopiowania i wklejania.

Gdy aplikacja przygotowuje wkleić dane skopiowane z obszaru roboczego, musi znaleźć reprezentacji, która najlepiej odpowiada jego możliwości (jeśli istnieje). Zwykle będzie najszerszym typ dostępne (na przykład sformatowany tekst aplikacji word przetwarzania), nastąpi powrót do formularzy najprostszym dostępne jako wymagany (zwykły tekst dla prostego edytora tekstów).

<a name="Promised_Data" />

### <a name="promised-data"></a>Uzgodnionego danych

Ogólnie rzecz biorąc należy podać liczbę reprezentacje danych, kopiowane jak to możliwe, aby zmaksymalizować udostępniane między aplikacjami. Jednak ze względu na ograniczenia czasu lub pamięć, może być niepraktyczne faktycznie zapisu każdego typu danych w obszarze roboczym.

W takiej sytuacji można umieścić pierwszy reprezentacji danych w obszarze roboczym i aplikacja odbierająca może żądać różnych reprezentacji, które mogą być wygenerowany na bieżąco tuż przed operacji wklejania.

Podczas początkowego elementu, który możesz umieścić w obszarze roboczym, należy określić co najmniej jeden z dostępnych oświadczenia są dostarczane przez obiekt, który jest zgodny z `NSPasteboardItemDataProvider` interfejsu. Te obiekty oferują dodatkowe oświadczenia na żądanie, zgodnie z żądaniem aplikacji odbierającej.

### <a name="change-count"></a>Liczba zmian

Każdy obszar roboczy przechowuje _liczba zmian_ przyrosty każdego nowego właściciela czasu jest zadeklarowana. Aplikację można określić, czy zawartość w obszarze roboczym uległy zmianie od czasu ostatniego go zbadać, sprawdzając wartość, liczba zmian.

Użyj `ChangeCount` i `ClearContents` metody `NSPasteboard` klasy, aby zmodyfikować dany obszar roboczy, liczba zmian.

## <a name="copying-data-to-a-pasteboard"></a>Kopiowanie danych do obszaru roboczego

Wykonuje operację kopiowania pierwszego uzyskiwania dostępu do obszaru roboczego, czyszczenie dowolnym istniejącą zawartość i zapisywanie tyle reprezentacje danych, ponieważ są wymagane do obszaru roboczego.

Na przykład:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

Zazwyczaj po prostu napiszesz do ogólnego obszar roboczy, ponieważ wykonaliśmy w powyższym przykładzie. Każdy obiekt, który możesz wysłać do `WriteObjects` metoda *musi* są zgodne z `INSPasteboardWriting` interfejsu. Kilka wbudowanych klas (takie jak `NSString`, `NSImage`, `NSURL`, `NSColor`, `NSAttributedString`, i `NSPasteboardItem`) automatycznie dopasowują się do tego interfejsu.

Jeśli piszesz klasy danych niestandardowych do obszaru roboczego musi być zgodna z `INSPasteboardWriting` interfejs lub być ujęte w instancji `NSPasteboardItem` klasy (zobacz [niestandardowych typów danych](#Custom_Data_Types) sekcji poniżej).

## <a name="reading-data-from-a-pasteboard"></a>Odczytywanie danych z obszaru roboczego

Jak wspomniano powyżej, aby zmaksymalizować możliwości udostępniania danych między aplikacjami, wiele reprezentacji skopiowane dane mogą być zapisane na obszarze roboczym. Jest odbieranie aplikacji, aby wybrać wersję najszerszym możliwe jego możliwości (jeśli istnieje).

### <a name="simple-paste-operation"></a>Operacja wklejania prosty

Odczytywanie danych z stołu montażowego przy użyciu `ReadObjectsForClasses` metody. Wymaga dwóch parametrów:

1. Tablica `NSObject` na podstawie typu klasy, które mają być odczytywane w obszarze roboczym. Należy zamówić to z typem danych najczęściej żądanej najpierw ze wszystkich pozostałych typów malejąco preferencji.
2. Słownik zawierający dodatkowe ograniczenia (np. ograniczenie do określonych typów zawartości na adres URL) lub pusty słownik, jeśli nie dodatkowe ograniczenia są wymagane.

Metoda zwraca tablicę elementów, które spełniają kryteria, które firma Microsoft przekazanej, dlatego zawiera co najwyżej taką samą liczbę typów danych, które są wymagane. jest również możliwe, że żaden z żądanych typów oraz zostanie zwrócona pusta tablica.

Na przykład, poniższy kod sprawdza, czy `NSImage` istnieje w ogólne obszar roboczy i wyświetla go w Studnia obrazu, jeśli istnieje:

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0];
            }
}
```

### <a name="requesting-multiple-data-types"></a>Żądanie wiele typów danych

Na podstawie typu aplikacji rozszerzenia Xamarin.Mac tworzony, może być może obsługiwać wiele reprezentacji danych wklejanych. W takiej sytuacji istnieją dwa scenariusze podczas pobierania danych w obszarze roboczym:

1. Wywołanie jednej `ReadObjectsForClasses` metody i zapewniając tablica wszystkich oświadczenia, które pragną (w kolejności preferencji).
2. Skierowanie wielu wywołań do `ReadObjectsForClasses` metoda monitowania o różnych tablicę typów każdorazowo.

Zobacz **prostych operacji wklejania** sekcji powyżej, aby uzyskać szczegółowe informacje na temat pobierania danych z obszaru roboczego.

### <a name="checking-for-existing-data-types"></a>Sprawdzanie dla istniejących typów danych

Istnieją momenty, które możesz chcieć sprawdzanie, czy obszar roboczy zawiera reprezentację danych bez faktycznego odczytu danych w obszarze roboczym (np. włączenie **Wklej** element menu tylko wtedy, gdy istnieje prawidłowe dane).

Wywołaj `CanReadObjectForClasses` metoda obszar roboczy, aby zobaczyć, czy zawiera danego typu.

Na przykład, poniższy kod określa, czy ogólne obszar roboczy zawiera `NSImage` wystąpienie:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

### <a name="reading-urls-from-the-pasteboard"></a>Odczytywanie adresów URL w obszarze roboczym

Na podstawie funkcji danej aplikacji rozszerzenia Xamarin.Mac, może być wymagane do odczytywania adresy URL z obszaru roboczego, ale tylko wtedy, gdy spełniają określony zestaw kryteriów (takich jak wskazujące pliki lub adresy URL na określony typ danych). W takiej sytuacji można określić dodatkowe kryteria wyszukiwania za pomocą drugiego parametru `CanReadObjectForClasses` lub `ReadObjectsForClasses` metody.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>Niestandardowe typy danych

Istnieją terminy, gdy trzeba będzie zapisać własnych typach niestandardowych w obszarze roboczym aplikacji platformy Xamarin.Mac. Na przykład rysowanie wektora aplikację, która umożliwia użytkownikowi kopiowanie i wklejanie Rysowanie obiektów.

W takiej sytuacji należy projektować klasę niestandardową danych, tak aby dziedziczył z `NSObject` i zgodne kilka interfejsów (`INSCoding`, `INSPasteboardWriting` i `INSPasteboardReading`). Opcjonalnie możesz użyć `NSPasteboardItem` do hermetyzacji danych, które mają zostać skopiowane lub wklejone.

Obie te opcje zostaną objęte szczegółowo poniżej.

### <a name="using-a-custom-class"></a>Za pomocą niestandardowej klasy

W tej sekcji firma Microsoft zostanie rozwijanie aplikacji prosty przykład, utworzony na początku tego dokumentu i Dodawanie niestandardowej klasy do śledzenia informacji o obrazie, który firma Microsoft jest kopiowanie i wklejanie między oknami.

Dodaj nową klasę do projektu, a następnie wywołaj ją **ImageInfo.cs**. Edytuj plik i przypisz ją wyglądać następująco:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfo")]
    public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
    {
        #region Computed Properties
        [Export("name")]
        public string Name { get; set; }

        [Export("imageType")]
        public string ImageType { get; set; }
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfo ()
        {
        }
        
        public ImageInfo (IntPtr p) : base (p)
        {
        }

        [Export ("initWithCoder:")]
        public ImageInfo(NSCoder decoder) {

            // Decode data
            NSString name = decoder.DecodeObject("name") as NSString;
            NSString type = decoder.DecodeObject("imageType") as NSString;

            // Save data
            Name = name.ToString();
            ImageType = type.ToString ();
        }
        #endregion

        #region Public Methods
        [Export ("encodeWithCoder:")]
        public void EncodeTo (NSCoder encoder) {

            // Encode data
            encoder.Encode(new NSString(Name),"name");
            encoder.Encode(new NSString(ImageType),"imageType");
        }

        [Export ("writableTypesForPasteboard:")]
        public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
            string[] writableTypes = {"com.xamarin.image-info", "public.text"};
            return writableTypes;
        }

        [Export ("pasteboardPropertyListForType:")]
        public virtual NSObject GetPasteboardPropertyListForType (string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSKeyedArchiver.ArchivedDataWithRootObject(this);
            case "public.text":
                return new NSString(string.Format("{0}.{1}", Name, ImageType));
            }

            // Failure, return null
            return null;
        }

        [Export ("readableTypesForPasteboard:")]
        public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
            string[] readableTypes = {"com.xamarin.image-info", "public.text"};
            return readableTypes;
        }

        [Export ("readingOptionsForType:pasteboard:")]
        public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSPasteboardReadingOptions.AsKeyedArchive;
            case "public.text":
                return NSPasteboardReadingOptions.AsString;
            }

            // Default to property list
            return NSPasteboardReadingOptions.AsPropertyList;
        }

        [Export ("initWithPasteboardPropertyList:ofType:")]
        public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return new ImageInfo();
            case "public.text":
                return new ImageInfo();
            }

            // Failure, return null
            return null;
        }
        #endregion
    }
}
    
```

W poniższych sekcjach przeniesiemy szczegółowy widok tej klasy.

#### <a name="inheritance-and-interfaces"></a>Dziedziczenie i interfejsy

Klasy niestandardowe dane było zapisywane do lub odczytywanie obszar roboczy, musi być zgodna z `INSPastebaordWriting` i `INSPasteboardReading` interfejsów. Ponadto musi on dziedziczyć z `NSObject` i również są zgodne z `INSCoding` interfejsu:

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

Klasa musi być również wystawiony języka Objective-C przy użyciu `Register` dyrektywy i musi uwidaczniać wszelkie wymagane właściwości lub metody za pomocą `Export`. Na przykład:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Firma Microsoft jest ujawniany dwa pola danych, które ta klasa będzie zawierać — nazwa obrazu i jego typ (jpg, png, itp.). 

Aby uzyskać więcej informacji, zobacz [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentacji opisano `Register` i `Export` atrybutów Umożliwia podłączanie klas języka C# do języka Objective-C obiektów i interfejsu użytkownika elementów.

#### <a name="constructors"></a>Konstruktorów

Dwa konstruktory (prawidłowo uwidaczniany w języku Objective-C) jest wymagana w przypadku klasy Nasze niestandardowe dane tak, aby mogły zostać odczytane z obszaru roboczego:

```csharp
[Export ("init")]
public ImageInfo ()
{
}

[Export ("initWithCoder:")]
public ImageInfo(NSCoder decoder) {

    // Decode data
    NSString name = decoder.DecodeObject("name") as NSString;
    NSString type = decoder.DecodeObject("imageType") as NSString;

    // Save data
    Name = name.ToString();
    ImageType = type.ToString ();
}
```

Po pierwsze, możemy ujawnić _pusty_ konstruktora w obszarze domyślną metodę języka Objective-C `init`.

Następnie możemy ujawnić `NSCoding` zgodne Konstruktor, który będzie używany do utworzenia nowego wystąpienia obiektu w obszarze roboczym, podczas wklejania w obszarze nazwy eksportowanych `initWithCoder`.

Ten konstruktor przyjmuje `NSCoder` (tworzony przez `NSKeyedArchiver` podczas są zapisywane w obszarze roboczym), wyodrębnia dane sparowane klucz/wartość i zapisuje go do pola właściwości klasy danych.

#### <a name="writing-to-the-pasteboard"></a>Zapisywanie w obszarze roboczym

Przez zgodną `INSPasteboardWriting` interfejsu, należy udostępnić dwie metody i opcjonalnie Trzecia metoda tak, aby klasa można zapisać w obszarze roboczym.

Po pierwsze dlatego trzeba poinformować, obszar roboczy dane typu oświadczenia, które mogą być zapisywane niestandardowej klasy:

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Każdy reprezentacja jest identyfikowany za pomocą jednolitego typ identyfikator (identyfikator UTI), czyli nic więcej niż prosty ciąg, który unikatowo identyfikuje typ danych jest prezentowana (Aby uzyskać więcej informacji, zobacz firmy Apple [jednolitego Przegląd identyfikatorów typu ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) dokumentacji).

Dla naszych formatu niestandardowego, jesteśmy w stanie tworzyć własną identyfikator UTI: "com.xamarin.image-info" (Pamiętaj, że znajduje się w odwrotnej notacji, podobnie jak identyfikator aplikacji). Klasy Nasze również jest w stanie zapisywanie standardowego ciągu w obszarze roboczym (`public.text`). 

Następnie należy utworzyć obiekt w żądany format, który faktycznie pobiera zapisywane w obszarze roboczym:

```csharp
[Export ("pasteboardPropertyListForType:")]
public virtual NSObject GetPasteboardPropertyListForType (string type) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSKeyedArchiver.ArchivedDataWithRootObject(this);
    case "public.text":
        return new NSString(string.Format("{0}.{1}", Name, ImageType));
    }

    // Failure, return null
    return null;
}
```

Dla `public.text` typu, firma Microsoft jest zwracany prosta, sformatowane `NSString` obiektu. Dla niestandardowego `com.xamarin.image-info` typu, użyto `NSKeyedArchiver` i `NSCoder` interfejsu do zakodowania klasy danych niestandardowych do archiwum sparowane klucz/wartość. Firma Microsoft będzie należy zaimplementować następującą metodę do faktycznie obsługi kodowania:

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Pary pojedynczych klucz/wartość są zapisywane do kodera i zostaną zdekodowane przy użyciu drugi Konstruktor, który dodaliśmy powyżej.

Opcjonalnie możemy dodać następującą metodę, aby zdefiniować żadnych opcji, podczas zapisywania danych w obszarze roboczym:

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

Obecnie tylko `WritingPromised` opcja jest dostępna i powinien być używany podczas danego typu jest tylko rzeczywiście i bez faktycznego są zapisywane w obszarze roboczym. Aby uzyskać więcej informacji, zobacz [rzeczywiście danych](#Promised_Data) powyższej sekcji.

Za pomocą tych metod w miejscu poniższy kod może służyć do zapisywania klasy Nasze niestandardowe obszar roboczy:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>Odczyt z obszaru roboczego

Przez zgodną `INSPasteboardReading` interfejsu, należy udostępnić trzy metody, tak aby klasy niestandardowe dane mogą być odczytywane w obszarze roboczym.

Najpierw należy sprawdzić w obszarze roboczym dane typu oświadczenia, które niestandardowej klasy mogą odczytywać ze Schowka:

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

Ponownie te są definiowane jako identyfikatory Uti proste i są te same typy zdefiniowane w **zapisywanie w obszarze roboczym** powyższej sekcji.

Następnie należy sprawdzić w obszarze roboczym _jak_ każdego z typami UTI będzie można odczytać przy użyciu następującej metody:

```csharp
[Export ("readingOptionsForType:pasteboard:")]
public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSPasteboardReadingOptions.AsKeyedArchive;
    case "public.text":
        return NSPasteboardReadingOptions.AsString;
    }

    // Default to property list
    return NSPasteboardReadingOptions.AsPropertyList;
}
```

Dla `com.xamarin.image-info` typu, kolejne parametry obszar roboczy do zdekodowania pary klucz/wartość, utworzonej za pomocą `NSKeyedArchiver` podczas zapisywania klasy obszar roboczy, wywołując `initWithCoder:` Konstruktor, który dodano do klasy.

Na koniec należy dodać następującą metodę do odczytywania innych reprezentacje danych identyfikatorów UTI w obszarze roboczym:

```csharp
[Export ("initWithPasteboardPropertyList:ofType:")]
public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

    // Take action based on the requested type
    switch (type) {
    case "public.text":
        return new ImageInfo();
    }

    // Failure, return null
    return null;
}
```

W przypadku wszystkich tych metod w miejscu klasy niestandardowe dane mogą być odczytywane z obszar roboczy, używając następującego kodu:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
var classArrayPtrs = new [] { Class.GetHandle (typeof(ImageInfo)) };
NSArray classArray = NSArray.FromIntPtrs (classArrayPtrs);

// NOTE: Sending messages directly to the base Objective-C API because of this defect:
// https://bugzilla.xamarin.com/show_bug.cgi?id=31760
// Check to see if image info is on the pasteboard
ok = bool_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("canReadObjectForClasses:options:"), classArray.Handle, IntPtr.Zero);

if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = NSArray.ArrayFromHandle<Foundation.NSObject>(IntPtr_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("readObjectsForClasses:options:"), classArray.Handle, IntPtr.Zero));
    ImageInfo info = (ImageInfo)objectsToPaste[0];
}
```

### <a name="using-a-nspasteboarditem"></a>Za pomocą NSPasteboardItem

Może to być czasy, kiedy należy napisać własne elementy do obszaru roboczego, który gwarantuje tworzenia niestandardowej klasy lub chcesz udostępnić dane w formacie common, tylko zgodnie z potrzebami. W takich sytuacjach można użyć `NSPasteboardItem`.

A `NSPasteboardItem` zapewnia precyzyjną kontrolę nad danymi, które są zapisywane w obszarze roboczym i jest przeznaczony do tymczasowego dostępu — jej powinny zostać usunięte z po jego zostały zapisane w obszarze roboczym.

#### <a name="writing-data"></a>Zapisywanie danych

Niestandardowe dane, aby zapisać `NSPasteboardItem` musisz podać niestandardowy `NSPasteboardItemDataProvider`. Dodaj nową klasę do projektu, a następnie wywołaj ją **ImageInfoDataProvider.cs**. Edytuj plik i przypisz ją wyglądać następująco:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfoDataProvider")]
    public class ImageInfoDataProvider : NSPasteboardItemDataProvider
    {
        #region Computed Properties
        public string Name { get; set;}
        public string ImageType { get; set;}
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfoDataProvider ()
        {
        }

        public ImageInfoDataProvider (string name, string imageType)
        {
            // Initialize
            this.Name = name;
            this.ImageType = imageType;
        }

        protected ImageInfoDataProvider (NSObjectFlag t){
        }

        protected internal ImageInfoDataProvider (IntPtr handle){

        }
        #endregion

        #region Override Methods
        [Export ("pasteboardFinishedWithDataProvider:")]
        public override void FinishedWithDataProvider (NSPasteboard pasteboard)
        {
            
        }

        [Export ("pasteboard:item:provideDataForType:")]
        public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
        {

            // Take action based on the type
            switch (type) {
            case "public.text":
                // Encode the data to string 
                item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
                break;
            }

        }
        #endregion
    }
}
```

Ile My mieliśmy z klasą danych niestandardowych, należy wykonać `Register` i `Export` dyrektywy w celu udostępnienia jej Objective-C. Klasa musi dziedziczyć `NSPasteboardItemDataProvider` , którą należy wdrożyć `FinishedWithDataProvider` i `ProvideDataForType` metody.

Użyj `ProvideDataForType` metodę dostarczania danych, które zostaną opakowane w `NSPasteboardItem` w następujący sposób:

```csharp
[Export ("pasteboard:item:provideDataForType:")]
public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
{

    // Take action based on the type
    switch (type) {
    case "public.text":
        // Encode the data to string 
        item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
        break;
    }

}
```

W tym przypadku możemy przechowywania dwóch rodzajów informacji o naszych obrazów (nazwy i ImageType) i zapisywanie tych prosty ciąg (`public.text`).

Typ zapisywać dane obszar roboczy, użyj następującego kodu:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Using a Pasteboard Item
NSPasteboardItem item = new NSPasteboardItem();
string[] writableTypes = {"public.text"};

// Add a data provider to the item
ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

// Save to pasteboard
if (ok) {
    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
}
```

#### <a name="reading-data"></a>Odczyt danych

Do odczytywania danych z kopii obszar roboczy, użyj następującego kodu:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
Class [] classArray  = { new Class ("NSImage") };

bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
    NSImage image = (NSImage)objectsToPaste[0];

    // Do something with data
    ...
}
            
Class [] classArray2 = { new Class ("ImageInfo") };
ok = pasteboard.CanReadObjectForClasses (classArray2, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
    
    // Do something with data
    ...
}
```

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z obszaru wklejania w aplikacji platformy Xamarin.Mac obsługuje kopiowania i wklejania. Po pierwsze wprowadzono prosty przykład, aby zapoznać się z operacjami standardowa większa. Następnie, jaki zajęło szczegółowy widok w obszarze roboczym i jak odczytywać i zapisywać dane z niego. Na koniec go przyjrzano się za pomocą niestandardowego typu danych do obsługi kopiowania i wklejania złożone typy danych w obrębie aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [MacCopyPaste (przykład)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Przewodnik programowania w stołu montażowego](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [System macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
