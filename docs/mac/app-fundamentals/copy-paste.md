---
title: Skopiuj i Wklej w Xamarin.Mac
description: Ten artykuł dotyczy pracy z obszar roboczy, aby zapewnić kopiowania i wklejania w aplikacji Xamarin.Mac. Widoczny jest sposób pracy z typy standardowych danych, które może być współużytkowana wielu aplikacji oraz sposobu obsługi danych niestandardowych w danej aplikacji.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: becdec771949584919595c84b13ae9e05bfd377b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791900"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Skopiuj i Wklej w Xamarin.Mac

_Ten artykuł dotyczy pracy z obszar roboczy, aby zapewnić kopiowania i wklejania w aplikacji Xamarin.Mac. Widoczny jest sposób pracy z typy standardowych danych, które może być współużytkowana wielu aplikacji oraz sposobu obsługi danych niestandardowych w danej aplikacji._

## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tego samego stołu montażowego (kopiowanie i wklejanie) obsługują ma Deweloper pracujący w języku Objective-C.

W tym artykule firma Microsoft będzie obejmował dwa podstawowe sposoby użycia obszar roboczy w aplikacji Xamarin.Mac:

1. **Standardowe typy danych** — ponieważ stołu montażowego operacje są zazwyczaj wykonywane między dwiema aplikacjami niepowiązanymi ze sobą, żadna aplikacja zna typy danych, które obsługuje innych. Aby zmaksymalizować możliwości udostępniania, obszar roboczy może zawierać wiele reprezentacje danego elementu (przy użyciu standardowego zestawu popularnych typów danych), to umożliwić odbierającą aplikację, aby wybrać wersję, która jest najbardziej odpowiednie dla swoich potrzeb.
2. **Niestandardowe dane** — umożliwia kopiowanie i wklejanie złożonych danych w sieci Xamarin.Mac można określić typu danych niestandardowych, który będzie obsługiwany przez obszar roboczy. Na przykład aplikacji rysowania wektor który umożliwia użytkownikowi kopiowanie i wklejanie kształtów złożonych, które składają się z wielu typów danych i punkty.

[![Przykład uruchomionej aplikacji](copy-paste-images/intro01.png "przykład uruchomionej aplikacji")](copy-paste-images/intro01-large.png#lightbox)

W tym artykule omówione zostaną następujące czynności podstawowe informacje dotyczące pracy z obszar roboczy w Xamarin.Mac aplikacji do obsługi kopiowania i wklejania. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` atrybutów umożliwiają połączenie klas C# do obiektów języka Objective C i interfejsu użytkownika elementy.

## <a name="getting-started-with-the-pasteboard"></a>Wprowadzenie do korzystania z obszaru roboczego

Obszar roboczy przedstawia informacje o standardowych mechanizm wymiany danych w ramach danej aplikacji lub między aplikacjami. Zwykle dla obszar roboczy w aplikacji Xamarin.Mac jest używane do obsługi kopiowania i wklejania, jednak wiele innych operacji są również obsługiwane (na przykład przeciągania i upuszczania i usługi aplikacji).

Aby szybko możesz wyłączyć ziemi, zamierzamy rozpoczynać się od prostego, praktyczne wprowadzenie przy użyciu większa w aplikacji Xamarin.Mac. Później firma Microsoft udostępni szczegółowe wyjaśnienie działania obszar roboczy i metod.

Na przykład firma Microsoft będzie można tworzenia prostych dokumentów na podstawie aplikacji zarządzającej okno zawierające widok obrazu. Użytkownik będzie mógł skopiować i wkleić obrazy między dokumentów w aplikacji oraz do lub z innych aplikacji lub wiele okien w tej samej aplikacji.

### <a name="creating-the-xamarin-project"></a>Tworzenie projektu Xamarin

Po pierwsze zamierzamy utworzyć nową aplikację Xamarin.Mac dokumentów na podstawie czy zostanie możemy Dodawanie Kopiuj i Wklej obsługę.

Wykonaj następujące czynności:

1. Uruchom program Visual Studio for Mac i kliknij przycisk **nowy projekt...**  łącza.
2. Wybierz **Mac** > **aplikacji** > **aplikacji Cocoa**, następnie kliknij przycisk **dalej** przycisk: 

    [![Tworzenie nowego projektu aplikacji Cocoa](copy-paste-images/sample01.png "tworzenia nowego projektu aplikacji Cocoa")](copy-paste-images/sample01-large.png#lightbox)
3. Wprowadź `MacCopyPaste` dla **Nazwa projektu** i zachować wszystkie inne jako domyślny. Kliknij przycisk Dalej: 

    [![Ustawienie nazwy projektu](copy-paste-images/sample01a.png "ustawienie nazwy projektu")](copy-paste-images/sample01a-large.png#lightbox)

4. Kliknij przycisk **Utwórz** przycisk: 

    [![Potwierdzenie nowego ustawienia projektu](copy-paste-images/sample02.png "potwierdzenie nowego ustawienia projektu")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>Dodaj NSDocument

Następnie dodamy niestandardowe `NSDocument` klasy, który będzie pełnił rolę magazynu tła dla interfejsu użytkownika aplikacji. Go będzie zawierać pojedynczy widok obrazu i dowiedzieć się, jak kopiowanie obrazu z wgląd w obszarze roboczym domyślne i jak pobrać obrazu z obszar roboczy domyślne i wyświetl ją w widoku obrazu.

Kliknij prawym przyciskiem myszy na projekt Xamarin.Mac w **konsoli rozwiązania** i wybierz **Dodaj** > **nowy plik.** :

![Dodawanie do projektu NSDocument](copy-paste-images/sample03.png "Dodawanie NSDocument do projektu")

Wprowadź `ImageDocument` dla **nazwa** i kliknij przycisk **nowy** przycisku. Edytuj **ImageDocument.cs** klasy i zapewnić ich wyglądać następująco:

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

Spójrzmy na części kodu szczegółowo poniżej.

Poniższy kod zawiera właściwość, aby przetestować obecność dane obrazu w obszarze roboczym domyślne, jeśli obraz jest dostępny, `true` jest zwracany w przeciwnym razie `false`:

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

Poniższy kod kopiuje obraz z widoku obrazów dołączonych w obszarze roboczym domyślne:

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

I następujący kod wkleja obrazu z obszar roboczy domyślne i wyświetla go w widoku dołączonych obrazów (Jeśli obszar roboczy zawiera nieprawidłowy obraz):

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

Niniejszy dokument w miejscu utworzymy interfejsu użytkownika dla aplikacji Xamarin.Mac.

### <a name="building-the-user-interface"></a>Tworzenie interfejsu użytkownika

Kliknij dwukrotnie **Main.storyboard** plik, aby otworzyć go w programie Xcode. Następnie dodaj pasek narzędzi i obraz również i skonfigurować w następujący sposób:

[![Pasek narzędzi edycji](copy-paste-images/sample04.png "narzędzi edycji")](copy-paste-images/sample04-large.png#lightbox)

Dodaj kopię i Wklej **element paska narzędzi obrazu** po lewej stronie paska narzędzi. Będziemy używać je jako skróty do kopiowania i wklejania w menu Edycja. Następnie dodaj cztery **elementów paska narzędzi obrazu** z prawej strony paska narzędzi. Te będą używane do wypełnienia obrazu z niektóre domyślne obrazy.

Aby uzyskać więcej informacji na temat pracy z paski narzędzi, zobacz nasze [paski narzędzi](~/mac/user-interface/toolbar.md) dokumentacji.

Następnie umożliwia również uwidacznia następujące punkty i akcji dla naszych elementów paska narzędzi i obrazu:

[![Tworzenie punktów i akcje](copy-paste-images/sample05.png "Tworzenie gniazda i akcji")](copy-paste-images/sample05-large.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z gniazda i akcji, zobacz [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcji naszych [Hello, Mac](~/mac/get-started/hello-mac.md) dokumentacji.

### <a name="enabling-the-user-interface"></a>Włączenie interfejsu użytkownika

Z naszych utworzone w programie Xcode i naszych elementu interfejsu użytkownika udostępniane za pośrednictwem funkcji gniazda i akcji interfejsu użytkownika musimy Dodaj kod, aby włączyć interfejsu użytkownika. Kliknij dwukrotnie **ImageWindow.cs** w pliku **konsoli rozwiązania** i zapewnić ich wyglądać następująco:

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

Spójrzmy na ten kod szczegółowo poniżej.

Po pierwsze, uwidaczniamy wystąpienia `ImageDocument` klasy, która utworzyliśmy powyżej:

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

Za pomocą `Export`, `WillChangeValue` i `DidChangeValue`, mamy Instalator `Document` właściwości, aby umożliwić kluczy i wartości kodowania i powiązania danych w środowisku Xcode.

Możemy również ujawniać obrazu z obrazu, oraz dodaliśmy do naszego interfejsu użytkownika w środowisku Xcode z następującej właściwości:

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

Gdy okno główne jest załadowana i wyświetlona, możemy utworzyć wystąpienia naszych `ImageDocument` klasy i dołączyć obrazu interfejsu użytkownika oraz do niego z następującym kodem:

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

Na koniec w odpowiedzi na kliknięcie na kopiowanie i wklejanie elementów paska narzędzi przez użytkownika, nazywamy wystąpienie `ImageDocument` klasy wykonują rzeczywistą pracę:

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Włączanie menu Plik i Edycja

Ostatnim zadaniem, które są konieczne jest, Włącz **nowy** element menu z **pliku** menu (w celu utworzenia nowego wystąpienia nasze główne okno) oraz w celu umożliwienia **Wytnij**, **kopiowania**  i **Wklej** elementów menu z **Edytuj** menu.

Aby włączyć **nowy** menu elementu, Edytuj **AppDelegate.cs** pliku i Dodaj następujący kod:

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

Aby uzyskać więcej informacji, zobacz [Praca z wielu okien](~/mac/user-interface/window.md) sekcji naszych [Windows](~/mac/user-interface/window.md) dokumentacji.

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

Dla każdego elementu menu możemy pobrać okna bieżącego, znajdujących się na górze, klucza i rzutować go na naszych `ImageWindow` klasy:

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

Z tego miejsca nazywamy `ImageDocument` wystąpienia klasy tego okna do obsługi kopiowania i wklejania akcje. Na przykład: 

```csharp
window.Document.CopyImage (sender);
```

Chcemy tylko **Wytnij**, **kopiowania** i **Wklej** elementów menu był dostępny w przypadku obrazu danych w obszarze roboczym domyślnego obrazu oraz bieżące aktywne okno.

Dodajmy **EditMenuDelegate.cs** plików do projektu Xamarin.Mac i zapewnić ich wyglądać następująco:

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

Ponownie, możemy uzyskać bieżące, znajdujących się na górze okna oraz jego `ImageDocument` wystąpienia klasy, czy dane wymagane obrazu istnieje. Następnie użyjemy `MenuWillHighlightItem` metody, aby włączyć lub wyłączyć każdego elementu na podstawie tego stanu.

Edytuj **AppDelegate.cs** plików i upewnij `DidFinishLaunching` wygląd metody podobne do poniższych:
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

Po pierwsze możemy wyłączyć automatyczne włączanie i wyłączanie elementów menu w menu Edycja. Następnie możemy Dołącz wystąpienia `EditMenuDelegate` klasy, która utworzyliśmy powyżej.

Aby uzyskać więcej informacji, zobacz nasze [menu](~/mac/user-interface/menu.md) dokumentacji.

### <a name="testing-the-app"></a>Testowanie aplikacji

Wszystko w miejscu możemy przystąpić do testowania aplikacji. Tworzenie i uruchamianie aplikacji i wyświetlany jest interfejs głównego:

![Uruchamianie aplikacji](copy-paste-images/run01.png "uruchamiania aplikacji")

Jeśli możesz otworzyć menu Edycja, należy pamiętać, że **Wytnij**, **kopiowania** i **Wklej** są wyłączone, ponieważ nie ma żadnego obrazu w obrazie również lub w obszarze roboczym domyślne:

![Otwieranie menu Edycja](copy-paste-images/run02.png "Otwieranie menu Edycja")

Jeśli również dodać obraz do obrazu i ponownie otwórz menu Edycja, zostanie ono włączone elementy:

![Wyświetlanie elementów menu Edycja są włączone](copy-paste-images/run03.png "wyświetlanie elementów menu Edycja są włączone")

Jeśli kopiowanie obrazu i wybierz **nowy** z menu Plik można wkleić tego obrazu w nowym oknie:

![Wklejanie obrazu w nowym oknie](copy-paste-images/run04.png "wklejania obrazu w nowym oknie")

W poniższych sekcjach przeniesiemy szczegółowe przyjrzeć się praca z obszar roboczy w aplikacji Xamarin.Mac.

## <a name="about-the-pasteboard"></a>O obszarze roboczym

W macOS (wcześniej znane jako OS X) obszar roboczy (`NSPasteboard`) umożliwia obsługę kilku serwer przetwarza takich jak kopiowanie i wklejanie, przeciągnij i upuść i usługi aplikacji. W poniższych sekcjach przeniesiemy bliższe spojrzenie na kilka kluczowych założeń stołu montażowego.

### <a name="what-is-a-pasteboard"></a>Co to jest obszar roboczy?

`NSPasteboard` Klasa udostępnia standardowy mechanizm wymiany informacji pomiędzy aplikacjami lub w ramach danej aplikacji. Główna funkcja obszar roboczy jest obsługa operacji kopiowania i wklejania:

1. Gdy użytkownik wybiera element w aplikacji i używa **Wytnij** lub **kopiowania** element menu reprezentacje co najmniej jednego wybranego elementu są umieszczane w obszarze roboczym.
2. Gdy użytkownik używa **Wklej** elementu menu (w ramach tej samej aplikacji lub inny), wersja danych, który może obsługiwać jest skopiowany z obszaru roboczego i dodać do aplikacji.

Mniej oczywista zastosowania stołu montażowego obejmują Znajdź, przeciągnij, przeciągnij i upuść, a operacje usługi aplikacji:

- Gdy użytkownik inicjuje operacji przeciągania, dane przeciągania są kopiowane do obszaru roboczego. Jeśli operacja przeciągania kończy listy na inną aplikację, danej aplikacji kopiuje dane w obszarze roboczym.
- Dla usługi tłumaczeniowe danych ma zostać poddany translacji jest kopiowany do obszar roboczy za pomocą żądania aplikacji. Usługa aplikacji pobiera dane z obszaru roboczego, jest translacja, a następnie past danych kopii w obszarze roboczym.

W najprostszej postaci większa są używane do przenoszenia danych wewnątrz danej aplikacji lub między aplikacjami i do nich istnieje w obszarze specjalne pamięci globalnej poza procesu aplikacji. Pojęcia związane z większa są łatwo grasps, istnieje kilka bardziej złożonych szczegółowe informacje, które należy wziąć pod uwagę. Te zostanie omówiona szczegółowo poniżej.

### <a name="named-pasteboards"></a>Większa o nazwie

Obszar roboczy może być publicznych lub prywatnych i może służyć do różnych celów w aplikacji lub między wieloma aplikacjami. System macOS udostępnia kilka standardowych większa, każdy z użyciem określonych, dobrze zdefiniowany:

- `NSGeneralPboard` — W obszarze roboczym domyślny dla **Wytnij**, **kopiowania** i **Wklej** operacji.
- `NSRulerPboard` — Obsługuje **Wytnij**, **kopiowania** i **Wklej** operacji na **linijki**.
- `NSFontPboard` — Obsługuje **Wytnij**, **kopiowania** i **Wklej** operacji na `NSFont` obiektów.
- `NSFindPboard` — Obsługuje specyficzne dla aplikacji Znajdź panele, które można udostępniać tekst do wyszukania.
- `NSDragPboard` — Obsługuje **przeciągnij i upuść** operacji.

W większości sytuacji będzie używany jeden większa zdefiniowane w systemie. Może to się zdarzyć, które wymagają utworzenia własnych większa. W takich przypadkach można użyć `FromName (string name)` metody `NSPasteboard` klasy w celu utworzenia niestandardowych obszar roboczy o podanej nazwie.

Opcjonalnie możesz wywołać `CreateWithUniqueName` metody `NSPasteboard` klasy w celu utworzenia unikatowej nazwie obszar roboczy.

### <a name="pasteboard-items"></a>Elementy stołu montażowego

Każda aplikacja zapisuje obszar roboczy danych jest uznawany za _elementu obszar roboczy_ i obszar roboczy może zawierać wielu elementów w tym samym czasie. W ten sposób aplikacji można zapisać wiele wersji danych do obszaru roboczego (na przykład zwykłego tekstu i tekst sformatowany) i pobierania aplikacji mogą odczytać tylko dane do przetwarzania (na przykład tylko zwykły tekst).

### <a name="data-representations-and-uniform-type-identifiers"></a>Identyfikatory uniform typu i reprezentacje danych

Operacje stołu montażowego zwykle zajmują aplikacji między dwoma (lub więcej), które nie korzystają z nie siebie lub typów danych każdy może obsłużyć. Jak już wspomniano w poprzedniej sekcji, aby zmaksymalizować możliwości udostępniania tych informacji, obszar roboczy może zawierać wiele reprezentacje skopiować i wkleić danych.

Każdy reprezentacja jest identyfikowany za pośrednictwem jednolitego typ identyfikator (UTI), które nic nie więcej niż prosty ciąg, który unikatowo identyfikuje typ daty zgłoszenia (Aby uzyskać więcej informacji, zobacz firmy Apple [Uniform Przegląd identyfikatorów typu ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) dokumentacji). 

Jeśli tworzysz niestandardowy typ danych (na przykład rysowania obiektu w wektorowej aplikacji), można tworzyć własne UTI, aby jednoznacznie zidentyfikować je w kopiowania i wklejania.

Przygotowuje aplikacji można wkleić skopiowanych z obszar roboczy danych, musi znaleźć reprezentacja, która najlepiej odpowiada jego możliwości (jeśli istnieje dowolne). Zazwyczaj są to najszerszym typ dostępne (na przykład sformatowany tekst dla aplikacji tekstów), nastąpi powrót do najprostszym formularzy dostępne jako wymagany (zwykły tekst edytora prosty tekst).

<a name="Promised_Data" />

### <a name="promised-data"></a>Uzgodnionej danych

Ogólnie rzecz biorąc należy podać tyle reprezentacje danych kopiowane, jak to możliwe, aby zmaksymalizować udostępniane między aplikacjami. Jednak ze względu na ograniczenia pamięci lub czasu, może być niemożliwe do faktycznie zapisać poszczególnych typów danych w obszarze roboczym.

W takiej sytuacji możesz umieścić pierwszy reprezentację danych w obszarze roboczym i aplikacja odbierająca może wysłać żądanie dotyczące różnych reprezentacji, które mogą być generowane na bieżąco tuż przed operacji wklejania.

Po umieszczeniu początkowego elementu w obszarze roboczym należy określić, że co najmniej jeden z dostępnych reprezentacje są dostarczane przez obiekt, który odpowiada `NSPasteboardItemDataProvider` interfejsu. Te obiekty określi dodatkowe reprezentacje na żądanie, zgodnie z żądaniem przez aplikację odbierającą.

### <a name="change-count"></a>Zmień liczbę

Każdy obszar roboczy obsługuje _zmiany liczby_ zadeklarowano czasu zwiększa każdego nowego właściciela. Aplikacja może określić, czy zawartość w obszarze roboczym zostały zmienione od czasu ostatniego ją zbadać weryfikując wartość tego licznika zmiany.

Użyj `ChangeCount` i `ClearContents` metody `NSPasteboard` klasy, aby zmodyfikować liczbę zmian dany obszar roboczy.

## <a name="copying-data-to-a-pasteboard"></a>Kopiowanie danych do obszaru roboczego

Wykonuje operacji kopiowania pierwszy podczas uzyskiwania dostępu do obszaru roboczego, wyczyszczenie wszelkich istniejącej zawartości i zapisywanie tyle reprezentacje danych w razie potrzeby do obszaru roboczego.

Na przykład:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

Zwykle po prostu napiszesz do ogólnych obszar roboczy, jak firma Microsoft to zostało zrobione w powyższym przykładzie. Każdy obiekt, który możesz wysłać do `WriteObjects` metody *musi* odpowiadają `INSPasteboardWriting` interfejsu. Kilka wbudowanych klas (takich jak `NSString`, `NSImage`, `NSURL`, `NSColor`, `NSAttributedString`, i `NSPasteboardItem`) automatycznie są zgodne z tym interfejsie.

Jeśli piszesz klasy danych niestandardowych do obszaru roboczego musi być zgodna z `INSPasteboardWriting` interfejsu lub być ujęte w wystąpieniu `NSPasteboardItem` klasy (zobacz [niestandardowe typy danych](#Custom_Data_Types) sekcji poniżej).

## <a name="reading-data-from-a-pasteboard"></a>Odczyt danych z obszaru roboczego

Jak wspomniano powyżej, aby zmaksymalizować możliwości udostępniania danych między aplikacjami, wiele reprezentacje skopiowanych danych może zostać zapisany obszar roboczy. Jest odbierania aplikacji wybierz wersję najszerszym możliwe jej możliwości, (jeśli istnieje dowolne).

### <a name="simple-paste-operation"></a>Operacja wklejania proste

Odczytywanie danych z stołu montażowego przy użyciu `ReadObjectsForClasses` metody. Trzeba będzie dwa parametry:

1. Tablica `NSObject` na podstawie typu klasy, które mają być odczytywane w obszarze roboczym. Należy zamówić to z najbardziej odpowiedniego typu danych, wszelkie pozostałe typy według preferencji.
2. Słownik zawierający dodatkowe ograniczenia (takie jak ograniczanie do określonych typów zawartości, adres URL) lub pusty słownik, jeśli są wymagane nie dodatkowe ograniczenia.

Metoda zwraca tablicę elementów, które spełniają kryteria, które firma Microsoft przekazano, dlatego zawiera maksymalnie taką samą liczbę typów danych, które są wymagane. Istnieje również możliwość, że żaden z żądanych typów nie jest obecny i zostanie zwrócony pustą tablicę.

Na przykład poniższy kod sprawdza, czy `NSImage` istnieje w obszarze roboczym ogólne i wyświetla je w obrazie dobrze, jeśli tak:

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

### <a name="requesting-multiple-data-types"></a>Żąda wiele typów danych

Na podstawie typu aplikacji Xamarin.Mac tworzona, może istnieć możliwość obsługi wielu reprezentacje danych wklejanych. W takim przypadku istnieją dwa scenariusze pobierania danych z obszaru roboczego:

1. Wywoływania jednej `ReadObjectsForClasses` — metoda i zapewnianie tablicę wszystkie oświadczenia, które chcesz (w preferowanej kolejności).
2. Wiele wywołań do `ReadObjectsForClasses` metody pytania o różnych tablicę typów zawsze.

Zobacz **prostych operacji wklejania** sekcji powyżej, aby uzyskać więcej informacji na temat pobierania danych z obszaru roboczego.

### <a name="checking-for-existing-data-types"></a>Wyszukiwanie istniejących typów danych

Istnieją momenty, które możesz chcieć sprawdzanie, czy obszar roboczy zawiera reprezentację danej danych bez faktycznie odczytywania danych z obszaru roboczego (np. włączenie **Wklej** element menu tylko wtedy, gdy istnieje prawidłowe dane).

Wywołanie `CanReadObjectForClasses` metody obszar roboczy, aby zobaczyć, czy zawiera danego typu.

Na przykład następujący kod określa, czy ogólne obszar roboczy zawiera `NSImage` wystąpienie:

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

Na podstawie funkcji danej aplikacji Xamarin.Mac, może być wymagane do odczytania adresów URL z obszaru roboczego, ale tylko wtedy, gdy spełniają danego określone kryteria (na przykład skierowana do plików lub adresy URL określonego typu danych). W takim przypadku można określić dodatkowe kryteria wyszukiwania przy użyciu drugiego parametru `CanReadObjectForClasses` lub `ReadObjectsForClasses` metody.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>Niestandardowe typy danych

Brak godziny, kiedy należy zapisać własnych niestandardowych typów w obszarze roboczym z aplikacji Xamarin.Mac. Na przykład wektorowej aplikację, która umożliwia użytkownikowi kopiowanie i wklejanie rysowania obiektów.

W takiej sytuacji należy zaprojektować niestandardowe klasy danych tak, aby go dziedziczy `NSObject` i odpowiada kilka interfejsów (`INSCoding`, `INSPasteboardWriting` i `INSPasteboardReading`). Opcjonalnie można użyć `NSPasteboardItem` celu hermetyzacji dane mają zostać skopiowane lub wkleić.

Obie te opcje zostanie omówiona szczegółowo poniżej.

### <a name="using-a-custom-class"></a>Za pomocą niestandardowej klasy

W tej sekcji możemy zostanie rozszerzając aplikacji prosty przykład utworzony na początku tego dokumentu i Dodawanie niestandardowej klasy do śledzenia informacji na temat obrazu, który mamy są kopiowanie i wklejanie między systemem windows.

Dodaj nową klasę w projekcie i nadaj mu **ImageInfo.cs**. Przeprowadź edycję pliku i przydzielić mu wyglądać następująco:

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

W poniższych sekcjach przeniesiemy szczegółowe przyjrzeć się tej klasy.

#### <a name="inheritance-and-interfaces"></a>Interfejsy i dziedziczenie

Przed klasy danych niestandardowych można zapisywania lub odczytywać obszar roboczy, musi być zgodna z `INSPastebaordWriting` i `INSPasteboardReading` interfejsów. Ponadto muszą dziedziczyć `NSObject` , a także są zgodne z `INSCoding` interfejsu:

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

Klasa musi być również wystawiony Objective-C za pomocą `Register` i dyrektywa musi ujawniać wszelkie wymagane właściwości lub metody za pomocą `Export`. Na przykład:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Firma Microsoft jest ujawniany przez dwa pola danych, które ta klasa będzie zawierać — nazwa obrazu i jego typ (jpg, png, itp.). 

Aby uzyskać więcej informacji, zobacz [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentacji opisano `Register` i `Export` atrybutów umożliwiają połączenie klas C# do obiektów języka Objective C i interfejsu użytkownika elementy.

#### <a name="constructors"></a>Konstruktorów

Dwa konstruktory (prawidłowo ujawniony dla języka Objective-C) jest wymagana w przypadku klasy Nasze danych niestandardowych, dzięki czemu mogą być odczytywane w obszarze roboczym:

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

Po pierwsze, uwidaczniamy _pusty_ konstruktora w obszarze domyślną metodę Objective-C `init`.

Następnie uwidaczniamy `NSCoding` zgodne Konstruktor, który będzie używany do utworzenia nowego wystąpienia obiektu w obszarze roboczym podczas wklejania pod nazwą wyeksportowanego `initWithCoder`.

Ten konstruktor ma `NSCoder` (tworzony przez `NSKeyedArchiver` gdy zapisany w obszarze roboczym), wyodrębnia dane klucza i wartości łączyć i zapisuje je do pól właściwości klasy danych.

#### <a name="writing-to-the-pasteboard"></a>Zapisywanie w obszarze roboczym

Przez odpowiadające `INSPasteboardWriting` interfejsu, musimy ujawnia dwie metody i opcjonalnie trzecią metodę, dzięki czemu klasy mogą być zapisywane na obszar roboczy.

Najpierw należy sprawdzić obszar roboczy danych typu oświadczenia, które mogą być zapisywane niestandardowej klasy:

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Każdy reprezentacja jest identyfikowany za pośrednictwem jednolitego typ identyfikator (UTI), które nic nie więcej niż prosty ciąg, który unikatowo identyfikuje typ danych jest prezentowana (Aby uzyskać więcej informacji, zobacz firmy Apple [Uniform Przegląd identyfikatorów typu ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) dokumentacji).

Dla naszych formatu niestandardowego Tworzenie własnej UTI: "com.xamarin.image-info" (należy pamiętać, że znajduje się w odwrotnej notacji podobnie jak identyfikator aplikacji). Klasy Nasze również jest w stanie zapisywanie standardowego ciągu w obszarze roboczym (`public.text`). 

Następnie należy utworzyć obiekt żądany format, który faktycznie pobiera zapisywane w obszarze roboczym:

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

Dla `public.text` typu, możemy zwróconego prosty, sformatowany `NSString` obiektu. Niestandardowe `com.xamarin.image-info` typu używamy `NSKeyedArchiver` i `NSCoder` interfejs do kodowania klasy danych niestandardowych do archiwum łączyć klucza i wartości. Musimy zaimplementować następującą metodę do faktycznie obsługi kodowanie:

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Pary klucz/wartość poszczególnych są zapisywane do kodera i będzie można dekodować przy użyciu drugi Konstruktor, który dodaliśmy powyżej.

Opcjonalnie możemy dodać następującą metodę do definiowania żadnych opcji podczas zapisywania danych w obszarze roboczym:

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

Obecnie tylko `WritingPromised` opcja jest dostępna i powinien być używany podczas danego typu jest tylko zapewnianą i faktycznie nie są zapisywane w obszarze roboczym. Aby uzyskać więcej informacji, zobacz [zapewnianą danych](#Promised_Data) powyższej sekcji.

Z tych metod w miejscu można można zapisać naszej klasy niestandardowej w obszarze roboczym następujący kod:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>Odczytywanie z obszaru roboczego

Przez odpowiadające `INSPasteboardReading` interfejsu, musimy ujawnia trzy metody, dzięki czemu klasy niestandardowe dane mogą być odczytywane w obszarze roboczym.

Najpierw należy sprawdzić w obszarze roboczym, jakie dane typu oświadczenia, które niestandardowej klasy mogą odczytywać ze Schowka:

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

Ponownie te są zdefiniowane jako UTIs proste i są te same typy zdefiniowane w **zapisywanie w obszarze roboczym** powyższej sekcji.

Następnie należy sprawdzić w obszarze roboczym _jak_ poszczególnych typów UTI będą odczytywane przy użyciu następujących metod:

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

Dla `com.xamarin.image-info` typu, firma Microsoft informuje obszar roboczy w celu zdekodowania parę klucza i wartości, które utworzono z `NSKeyedArchiver` podczas zapisywania klasy obszar roboczy, wywołując `initWithCoder:` Konstruktor, który dodano do klasy.

Na koniec musimy Dodaj następującą metodę do odczytu innych reprezentacje UTI danych w obszarze roboczym:

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

### <a name="using-a-nspasteboarditem"></a>Przy użyciu NSPasteboardItem

Może to być razy, gdy należy napisać własne elementy do obszaru roboczego, który gwarantuje Tworzenie niestandardowej klasy lub chcesz podać dane w formacie wspólne, tylko zgodnie z potrzebami. W takich sytuacjach można użyć `NSPasteboardItem`.

A `NSPasteboardItem` zapewnia precyzyjną kontrolę nad danymi, które są zapisywane w obszarze roboczym i jest przeznaczony do tymczasowego dostępu — go powinna zostać usunięta z po jego zostały zapisane w obszarze roboczym.

#### <a name="writing-data"></a>Zapisywanie danych

Niestandardowe dane, aby zapisać `NSPasteboardItem` musisz podać niestandardowy `NSPasteboardItemDataProvider`. Dodaj nową klasę w projekcie i nadaj mu **ImageInfoDataProvider.cs**. Przeprowadź edycję pliku i przydzielić mu wyglądać następująco:

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

Jak robiliśmy przy użyciu klasy danych niestandardowych, należy użyć `Register` i `Export` dyrektywy je ujawnić Objective-c. Musi dziedziczyć po klasie `NSPasteboardItemDataProvider` , którą należy wdrożyć `FinishedWithDataProvider` i `ProvideDataForType` metody.

Użyj `ProvideDataForType` metodę w celu zapewnienia dane, które zostaną `NSPasteboardItem` w następujący sposób:

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

W takim przypadku możemy przechowywane informacje o naszych obrazu (nazwy i ImageType) i zapisywanie tych prosty ciąg (`public.text`).

Typ zapisać dane obszar roboczy, użyj następującego kodu:

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

Odczytywanie danych z obszaru roboczego, użyj następującego kodu:

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

W tym artykule trwało szczegółowe przyjrzeć się praca z obszar roboczy w Xamarin.Mac aplikacji do obsługi kopiowania i wklejania. Wprowadzona go prosty przykład, aby zapoznać się z operacjami standardowe większa. Następnie zajęło szczegółowy widok w obszarze roboczym i sposobu odczytywania i zapisywania danych z niego. Na koniec znaleziono za pomocą niestandardowego typu danych do obsługi kopiowanie i wklejanie złożone typy danych w aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [MacCopyPaste (przykład)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Przewodnik programowania w języku stołu montażowego](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [System macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
