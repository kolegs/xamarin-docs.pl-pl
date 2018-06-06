---
title: Okna dialogowe w Xamarin.Mac
description: Ten artykuł dotyczy pracy z okien dialogowych i modalnych okien aplikacji Xamarin.Mac. Opisuje tworzenie okna modalne Xcode i interfejsu konstruktora, Praca z standardowych oknach dialogowych i interakcji z tych kontrolek w kodzie języka C#.
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7d9a93c8503d7e25f098e871378a22455b597e90
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792697"
---
# <a name="dialogs-in-xamarinmac"></a>Okna dialogowe w Xamarin.Mac

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tych samych okien dialogowych i okna modalne który używająca *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć w środowisku Xcode _konstruktora interfejsu_ do tworzenia i obsługi modalnych okien (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Okno dialogowe jest wyświetlany w odpowiedzi na akcję użytkownika i zazwyczaj zapewnia użytkownikom sposoby może zakończyć akcji. Okno dialogowe wymaga odpowiedzi od użytkownika może zostać zamknięty.

Systemu Windows mogą być używane w stanie niemodalne (takich jak edytor tekstu, który może mieć wielu dokumentów otwartych jednocześnie) lub modalne (na przykład okno eksportu, który musi być ukryty, zanim będzie można kontynuować aplikacji).

[![](dialog-images/dialog03.png "Otwarte okno dialogowe")](dialog-images/dialog03.png#lightbox)

W tym artykule omówione zostaną następujące czynności podstawowe informacje dotyczące pracy z okien dialogowych i okna modalne w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` poleceń używany do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>Wprowadzenie do okien dialogowych

Okno dialogowe jest wyświetlany w odpowiedzi na akcję użytkownika (np. Zapisywanie pliku) i umożliwia użytkownikom do wykonania tej akcji. Okno dialogowe wymaga odpowiedzi od użytkownika może zostać zamknięty.

Zgodnie z firmy Apple istnieją trzy sposoby wyświetlenia okna dialogowego:

- **Dokument modalne** -A dokumentu modalnego okna dialogowego uniemożliwia wykonanie dodatkowych czynności w ramach danego dokumentu, dopóki nie zostanie on odrzucony.
- **Modalne aplikacji** — aplikacji modalnego okna dialogowego uniemożliwia interakcji z aplikacją, dopóki nie zostanie on odrzucony.
- **Niemodalny** okna dialogowego niemodalny A pozwala użytkownikom na zmianę ustawienia w oknie dialogowym podczas nadal interakcji z okna dokumentu.

### <a name="modal-window"></a>Modalne okna

Wszystkie standardowe `NSWindow` mogą być używane jako okno dialogowe dostosowane, wyświetlając trybie modalnym:

[![](dialog-images/modal01.png "Przykład modalny")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>Arkusze modalnego okna dialogowego dokumentu

A _arkusza_ jest modalne okno dialogowe jest dołączony do okna danego dokumentu, uniemożliwia użytkownikom interakcji z okna do czasu ich zamknąć okno dialogowe. Arkusz jest dołączony do okna, z którego wynika, i tylko jeden arkusz można otworzyć okna w dowolnym momencie.

[![](dialog-images/sheet08.png "Przykład arkusza modalne")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>Preferencje systemu Windows

Okno Preferencje jest niemodalne okno dialogowe, które zawiera ustawienia aplikacji, które użytkownik zmieni się rzadko. Preferencje systemu Windows często zawierają paska narzędzi przez użytkownika w celu przełączania się między różnymi grupami ustawień:

[![](dialog-images/dialog02.png "Przykładowe okno preferencji")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>Otwórz okno dialogowe

Otwórz okno dialogowe umożliwia użytkownikom spójny sposób, aby znaleźć i otworzyć element w aplikacji:

[![](dialog-images/dialog03.png "Otwórz okno dialogowe")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>Drukowanie i okna dialogowe Ustawienia strony

System macOS zawiera standardowe drukowania i okna Ustawienia strony, których aplikacja może wyświetlać, dzięki czemu użytkownicy mogą mieć zgodne drukowanie doświadczenie w każdej aplikacji, których używają.

Okno dialogowe drukowania mogą być wyświetlane jako zarówno bezpłatne przestawne okno dialogowe:

[![](dialog-images/print01.png "Okno dialogowe drukowania")](dialog-images/print01.png#lightbox)

Lub mogą być wyświetlane jako arkusz:

[![](dialog-images/print02.png "Arkusz wydruku")](dialog-images/print02.png#lightbox)

Okno dialogowe Ustawienia strony mogą być wyświetlane jako zarówno bezpłatne przestawne okno dialogowe:

[![](dialog-images/print03.png "Okno dialogowe Ustawienia strony")](dialog-images/print03.png#lightbox)

Lub mogą być wyświetlane jako arkusz:

[![](dialog-images/print04.png "Arkusz Ustawienia strony")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>Zapisz okien dialogowych

Okno dialogowe Zapisz umożliwia użytkownikom spójny sposób, aby zapisać element w aplikacji. Okno dialogowe Zapisz ma dwa stany: **minimalnego** (znanej także jako zwinięte):

[![](dialog-images/save01.png "Zapisywanie okna dialogowego")](dialog-images/save01.png#lightbox)

I **rozwinięty** stanu:

[![](dialog-images/save02.png "Okno dialogowe zapisywania rozszerzonych")](dialog-images/save02.png#lightbox)

**Minimalnego** dialogowe Zapisz mogą być także wyświetlane jako arkusz:

[![](dialog-images/save03.png "Minimalny zapisać arkusza")](dialog-images/save03.png#lightbox)

Jak **rozwinięty** dialogowe Zapisz:

[![](dialog-images/save04.png "Rozwinięte zapisać arkusza")](dialog-images/save04.png#lightbox)

Aby uzyskać więcej informacji, zobacz [okna](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Dodawanie do projektu modalny

Jako uzupełnienie okna głównego dokumentu aplikacji Xamarin.Mac może być konieczne do wyświetlania innych typów systemu windows użytkownika, takie jak preferencje lub Inspektora paneli.

Aby dodać nowe okno, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, otwórz `Main.storyboard` plik do edycji w Konstruktorze interfejsu w środowisku Xcode.
2. Przeciągnij nowy **kontrolera widoku** na powierzchnię projektu:

    [![](dialog-images/new01.png "Wybiera kontroler widoku z biblioteki")](dialog-images/new01.png#lightbox)
3. W **inspektora tożsamości**, wprowadź `CustomDialogController` dla **Nazwa klasy**: 

    [![](dialog-images/new02.png "Ustawienie nazwy klasy")](dialog-images/new02.png#lightbox)
4. Wrócić do programu Visual Studio dla komputerów Mac, Zezwalaj na synchronizację w programie Xcode i utworzyć `CustomDialogController.h` pliku.
5. Wróć do Xcode i projekt interfejsu: 

    [![](dialog-images/new03.png "Projektowanie interfejsu użytkownika w środowisku Xcode")](dialog-images/new03.png#lightbox)
6. Utwórz **Segue modalne** z okna głównego aplikacji na nowy kontroler widoku, przeciągając kontroli z elementu interfejsu użytkownika, który zostanie otwarte okno dialogowe do okna dialogowego. Przypisz **identyfikator** `ModalSegue`: 

    [![](dialog-images/new06.png "Modalne segue")](dialog-images/new06.png#lightbox)
6. Podczas transmisji w górę żadnego **akcje** i **gniazda**: 

    [![](dialog-images/new04.png "Konfigurowanie akcji")](dialog-images/new04.png#lightbox)
6. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Wprowadź `CustomDialogController.cs` wygląd pliku podobne do poniższych:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class CustomDialogController : NSViewController
    {
        #region Private Variables
        private string _dialogTitle = "Title";
        private string _dialogDescription = "Description";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string DialogTitle {
            get { return _dialogTitle; }
            set { _dialogTitle = value; }
        }

        public string DialogDescription {
            get { return _dialogDescription; }
            set { _dialogDescription = value; }
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public CustomDialogController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial title and description
            Title.StringValue = DialogTitle;
            Description.StringValue = DialogDescription;
        }
        #endregion

        #region Private Methods
        private void CloseDialog() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptDialog (Foundation.NSObject sender) {
            RaiseDialogAccepted();
            CloseDialog();
        }

        partial void CancelDialog (Foundation.NSObject sender) {
            RaiseDialogCanceled();
            CloseDialog();
        }
        #endregion

        #region Events
        public EventHandler DialogAccepted;

        internal void RaiseDialogAccepted() {
            if (this.DialogAccepted != null)
                this.DialogAccepted (this, EventArgs.Empty);
        }

        public EventHandler DialogCanceled;

        internal void RaiseDialogCanceled() {
            if (this.DialogCanceled != null)
                this.DialogCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Ten kod zawiera kilka właściwości można ustawić tytuł i opis tego okna i kilka zdarzeń reagowanie do okna dialogowego, które są anulowane lub akceptowane.

Następnie Edytuj `ViewController.cs` pliku, Zastąp `PrepareForSegue` — metoda i zapewnić ich wyglądać następująco:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    }
}
```

Ten kod inicjuje segue, który zdefiniowanego w Konstruktorze interfejsu w środowisku Xcode do naszej okna dialogowego i ustawia tytuł i opis. Obsługuje ona również wybór wprowadzone przez użytkownika w oknie dialogowym.

Możemy uruchomić aplikację i wyświetla okno dialogowe niestandardowych:

[![](dialog-images/new05.png "Przykładowe okno dialogowe")](dialog-images/new05.png#lightbox)

Aby uzyskać więcej informacji na temat korzystania z systemu windows w aplikacji Xamarin.Mac, zobacz nasze [pracy z systemem Windows](~/mac/user-interface/window.md) dokumentacji.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Tworzenie niestandardowych arkusza

A _arkusza_ jest modalne okno dialogowe jest dołączony do okna danego dokumentu, uniemożliwia użytkownikom interakcji z okna do czasu ich zamknąć okno dialogowe. Arkusz jest dołączony do okna, z którego wynika, i tylko jeden arkusz można otworzyć okna w dowolnym momencie. 

Aby utworzyć arkusz niestandardowe w Xamarin.Mac, teraz wykonać następujące czynności:

1. W **Eksploratora rozwiązań**, otwórz `Main.storyboard` plik do edycji w Konstruktorze interfejsu w środowisku Xcode.
2. Przeciągnij nowy **kontrolera widoku** na powierzchnię projektu:

    [![](dialog-images/new01.png "Wybiera kontroler widoku z biblioteki")](dialog-images/new01.png#lightbox)
2. Projektowanie interfejsu użytkownika:

    [![](dialog-images/sheet01.png "Projektowanie interfejsu użytkownika")](dialog-images/sheet01.png#lightbox)
3. Utwórz **Segue arkusza** z okna głównego na nowy kontroler widoku: 

    [![](dialog-images/sheet02.png "Wybieranie typu segue arkusza")](dialog-images/sheet02.png#lightbox)
4. W **inspektora tożsamości**, nazwy kontrolera widoku **klasy** `SheetViewController`: 

    [![](dialog-images/sheet03.png "Ustawienie nazwy klasy")](dialog-images/sheet03.png#lightbox)
5. Zdefiniuj wszelkie potrzebne **gniazda** i **akcje**: 

    [![](dialog-images/sheet04.png "Definiowanie wymagane gniazda i akcji")](dialog-images/sheet04.png#lightbox)
6. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji.

Następnie Edytuj `SheetViewController.cs` pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class SheetViewController : NSViewController
    {
        #region Private Variables
        private string _userName = "";
        private string _password = "";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string UserName {
            get { return _userName; }
            set { _userName = value; }
        }

        public string Password {
            get { return _password;}
            set { _password = value;}
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public SheetViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial values
            NameField.StringValue = UserName;
            PasswordField.StringValue = Password;

            // Wireup events
            NameField.Changed += (sender, e) => {
                UserName = NameField.StringValue;
            };
            PasswordField.Changed += (sender, e) => {
                Password = PasswordField.StringValue;
            };
        }
        #endregion

        #region Private Methods
        private void CloseSheet() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptSheet (Foundation.NSObject sender) {
            RaiseSheetAccepted();
            CloseSheet();
        }

        partial void CancelSheet (Foundation.NSObject sender) {
            RaiseSheetCanceled();
            CloseSheet();
        }
        #endregion

        #region Events
        public EventHandler SheetAccepted;

        internal void RaiseSheetAccepted() {
            if (this.SheetAccepted != null)
                this.SheetAccepted (this, EventArgs.Empty);
        }

        public EventHandler SheetCanceled;

        internal void RaiseSheetCanceled() {
            if (this.SheetCanceled != null)
                this.SheetCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Następnie Edytuj `ViewController.cs` plik, Edytuj `PrepareForSegue` — metoda i zapewnić ich wyglądać następująco:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    case "SheetSegue":
        var sheet = segue.DestinationController as SheetViewController;
        sheet.SheetAccepted += (s, e) => {
            Console.WriteLine ("User Name: {0} Password: {1}", sheet.UserName, sheet.Password);
        };
        sheet.Presentor = this;
        break;
    }
}
```

Jeśli firma Microsoft może uruchomić aplikację, a następnie otwórz kartę, zostanie dołączona do okna:

[![](dialog-images/sheet08.png "Przykład arkusza")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Tworzenie okna dialogowego preferencji

Zanim firma Microsoft układ widoku preferencji konstruktora interfejsu musimy dodać typ segue niestandardowego do obsługi przełączania z preferencji. Dodaj nową klasę do projektu i nadaj mu `ReplaceViewSeque `. Edytuj klasy i zapewnić ich wyglądać następująco:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacWindows
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Is there a source?
            if (source == null) {
                // No, get the current key window
                var window = NSApplication.SharedApplication.KeyWindow;

                // Swap the controllers
                window.ContentViewController = destination;

                // Release memory
                window.ContentViewController?.RemoveFromParentViewController ();
            } else {
                // Swap the controllers
                source.View.Window.ContentViewController = destination;

                // Release memory
                source.RemoveFromParentViewController ();
            }
        
        }
        #endregion

    }

}
```

Z segue niestandardowe, utworzone możemy Dodaj nowe okno w Konstruktorze interfejsu w środowisku Xcode do obsługi naszych Preferencje.

Aby dodać nowe okno, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, otwórz `Main.storyboard` plik do edycji w Konstruktorze interfejsu w środowisku Xcode.
2. Przeciągnij nowy **kontrolera okna** na powierzchnię projektu:

    [![](dialog-images/pref01.png "Wybierz kontroler okno z biblioteki")](dialog-images/pref01.png#lightbox)
3. Rozmieść okna w pobliżu **paska Menu** projektanta:

    [![](dialog-images/pref02.png "Dodawanie nowego okna")](dialog-images/pref02.png#lightbox)
4. Utwórz kopie dołączonego kontrolera widoku, ponieważ w widoku preferencji będzie karty:

    [![](dialog-images/pref03.png "Dodawanie wymaganych kontrolerów widoku")](dialog-images/pref03.png#lightbox)
5. Przeciągnij nowy **kontrolera narzędzi** z **biblioteki**:

    [![](dialog-images/pref04.png "Wybierz kontroler narzędzi z biblioteki")](dialog-images/pref04.png#lightbox)
6. I upuść ją na okna na powierzchnię projektu:

    [![](dialog-images/pref05.png "Dodawanie nowego kontrolera paska narzędzi")](dialog-images/pref05.png#lightbox)
7. Układ projektu paska narzędzi:

    [![](dialog-images/pref06.png "Układ paska narzędzi")](dialog-images/pref06.png#lightbox)
8. Formant kliknij i przeciągnij z każdej **przycisku paska narzędzi** do widoków utworzone powyżej. Wybierz **niestandardowy** segue typu:

    [![](dialog-images/pref07.png "Ustawienie typu segue")](dialog-images/pref07.png#lightbox)
9. Wybierz nowe Segue i ustaw **klasy** do `ReplaceViewSegue`:

    [![](dialog-images/pref08.png "Klasa segue ustawień")](dialog-images/pref08.png#lightbox)
10. W **projektanta Menubar** na powierzchni projektu aplikacji wybierz z Menu **Preferencje...** , przytrzymując klawisz CTRL i przeciągnij, aby okna Preferencje, aby utworzyć **Pokaż** segue:

    [![](dialog-images/pref09.png "Ustawienie typu segue")](dialog-images/pref09.png#lightbox)
11. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji.

Jeśli firma Microsoft wykonywania kodu i wybierz **Preferencje...**  z **Menu aplikacja**, wyświetli się okno:

[![](dialog-images/pref10.png "Przykład preferencje okna")](dialog-images/pref10.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z systemem Windows i paski narzędzi, zobacz nasze [Windows](~/mac/user-interface/window.md) i [paski narzędzi](~/mac/user-interface/toolbar.md) dokumentacji.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Zapisywanie i ładowanie preferencji

W typowych macOS aplikacji gdy użytkownik wprowadza zmiany do dowolnego z preferencjami użytkownika aplikacji te zmiany są zapisywane automatycznie. Najprostszym sposobem obsługi to w aplikacji Xamarin.Mac jest utworzyć jedną klasę do zarządzania wszystkimi preferencji użytkownika i udostępnij go całego systemu.

Najpierw Dodaj nowy `AppPreferences` klas do projektu i dziedziczyć `NSObject`. Preferencje będzie przeznaczony do użycia [powiązania danych i kluczy i wartości kodowania](~/mac/app-fundamentals/databinding.md) który spowoduje, że proces tworzenia i utrzymywania Preferencje formularzy znacznie prostsze. Ponieważ preferencje będzie składać się z małej ilości proste typy danych, użyć wbudowanych w `NSUserDefaults` do przechowywania i pobierania wartości.

Edytuj `AppPreferences.cs` pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    [Register("AppPreferences")]
    public class AppPreferences : NSObject
    {
        #region Computed Properties
        [Export("DefaultLangauge")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLangauge", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLangauge");
                SaveInt ("DefaultLangauge", value, true);
                DidChangeValue ("DefaultLangauge");
            }
        }

        [Export("SmartLinks")]
        public bool SmartLinks {
            get { return LoadBool ("SmartLinks", true); }
            set {
                WillChangeValue ("SmartLinks");
                SaveBool ("SmartLinks", value, true);
                DidChangeValue ("SmartLinks");
            }
        }

        // Define any other required user preferences in the same fashion
        ...

        [Export("EditorBackgroundColor")]
        public NSColor EditorBackgroundColor {
            get { return LoadColor("EditorBackgroundColor", NSColor.White); }
            set {
                WillChangeValue ("EditorBackgroundColor");
                SaveColor ("EditorBackgroundColor", value, true);
                DidChangeValue ("EditorBackgroundColor");
            }
        }
        #endregion

        #region Constructors
        public AppPreferences ()
        {
        }
        #endregion

        #region Public Methods
        public int LoadInt(string key, int defaultValue) {
            // Attempt to read int
            var number = NSUserDefaults.StandardUserDefaults.IntForKey(key);

            // Take action based on value
            if (number == null) {
                return defaultValue;
            } else {
                return (int)number;
            }
        }
            
        public void SaveInt(string key, int value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetInt(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public bool LoadBool(string key, bool defaultValue) {
            // Attempt to read int
            var value = NSUserDefaults.StandardUserDefaults.BoolForKey(key);

            // Take action based on value
            if (value == null) {
                return defaultValue;
            } else {
                return value;
            }
        }

        public void SaveBool(string key, bool value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetBool(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public string NSColorToHexString(NSColor color, bool withAlpha) {
            //Break color into pieces
            nfloat red=0, green=0, blue=0, alpha=0;
            color.GetRgba (out red, out green, out blue, out alpha);

            // Adjust to byte
            alpha *= 255;
            red *= 255;
            green *= 255;
            blue *= 255;

            //With the alpha value?
            if (withAlpha) {
                return String.Format ("#{0:X2}{1:X2}{2:X2}{3:X2}", (int)alpha, (int)red, (int)green, (int)blue);
            } else {
                return String.Format ("#{0:X2}{1:X2}{2:X2}", (int)red, (int)green, (int)blue);
            }
        }

        public NSColor NSColorFromHexString (string hexValue)
        {
            var colorString = hexValue.Replace ("#", "");
            float red, green, blue, alpha;

            // Convert color based on length
            switch (colorString.Length) {
            case 3 : // #RGB
                red = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(0, 1)), 16) / 255f;
                green = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(1, 1)), 16) / 255f;
                blue = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(2, 1)), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 6 : // #RRGGBB
                red = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 8 : // #AARRGGBB
                alpha = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                red = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(6, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, alpha);
            default :
                throw new ArgumentOutOfRangeException(string.Format("Invalid color value '{0}'. It should be a hex value of the form #RBG, #RRGGBB or #AARRGGBB", hexValue));
            }
        }

        public NSColor LoadColor(string key, NSColor defaultValue) {

            // Attempt to read color
            var hex = NSUserDefaults.StandardUserDefaults.StringForKey(key);

            // Take action based on value
            if (hex == null) {
                return defaultValue;
            } else {
                return NSColorFromHexString (hex);
            }
        }

        public void SaveColor(string key, NSColor color, bool sync) {
            // Save to default
            NSUserDefaults.StandardUserDefaults.SetString(NSColorToHexString(color,true), key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }
        #endregion
    }
}
```

Ta klasa zawiera kilka procedury Pomocnika `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`, itp., aby praca z `NSUserDefaults` łatwiejsze. Ponadto, ponieważ `NSUserDefaults` nie ma wbudowanej sposób obsługi `NSColors`, `NSColorToHexString` i `NSColorFromHexString` metody są używane do konwersji kolorów do ciągów szesnastkowych opartych na sieci web (`#RRGGBBAA` gdzie `AA` jest alfa przezroczystości) które może być łatwo przechowywane i pobierane.

W `AppDelegate.cs` plików, Utwórz wystąpienie **AppPreferences** obiektu, który będzie używany w całej aplikacji:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace SourceWriter
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;

        public AppPreferences Preferences { get; set; } = new AppPreferences();
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion
        
        ...
```

<a name="Wiring-Preferences-to-Preference-Views" />

### <a name="wiring-preferences-to-preference-views"></a>Preferencje połączeń z widokami preferencji

Następnie podłącz preferencji klasy do elementów interfejsu użytkownika w oknie preferencji i tworzyć widoki powyżej. W Konstruktorze interfejsu, wybierz kontroler widoku preferencji i przejdź do **inspektora tożsamości**, Utwórz niestandardowe klasę dla kontrolera: 

[![](dialog-images/prefs12.png "Inspektor tożsamości")](dialog-images/prefs12.png#lightbox)

Przełącz się do programu Visual Studio for Mac zsynchronizować zmiany, a następnie otwórz nowo utworzonej klasy do edycji. Wprowadź klasy wyglądać następująco:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class EditorPrefsController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        [Export("Preferences")]
        public AppPreferences Preferences {
            get { return App.Preferences; }
        }
        #endregion

        #region Constructors
        public EditorPrefsController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Należy zauważyć, że ta klasa została wykonana w tym miejscu dwie czynności: najpierw jest Pomocnika `App` właściwości, aby podczas uzyskiwania dostępu do **AppDelegate** łatwiejsze. Drugi, `Preferences` właściwość udostępnia globalny **AppPreferences** klasy wiązania danych za pomocą jakichkolwiek formantów interfejsu użytkownika umieszczana w tym widoku.

Kliknij dwukrotnie następnie plik scenorysu, otwórz go ponownie w konstruktora interfejsu (i zobaczyć zmiany dokonane po prostu powyżej). Przeciągnij wszystkie kontrolki interfejsu użytkownika wymagane do tworzenia interfejsu Preferencje w widoku. Dla każdego formantu, przełącz się do **powiązanie inspektora** i powiąż poszczególnych właściwości **AppPreference** klasy:

[![](dialog-images/prefs13.png "Inspektor powiązania")](dialog-images/prefs13.png#lightbox)

Powtórz powyższe kroki dla wszystkich paneli (Widok kontrolery) i wymaganych właściwości preferencji.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>Stosowanie preferencji zmienia się na wszystkie otwarte okna

Jak już wspomniano w typowych macOS aplikacji, gdy użytkownik wprowadza zmiany do dowolnego preferencje użytkownika aplikacji, te zmiany są automatycznie zapisywane i stosowane do wszystkich okien użytkownika, może być otwarty w aplikacji.

Ten proces sprawnie i w przezroczysty sposób dla użytkownika końcowego z minimalną ilością kodowania pracy umożliwi zachować ostrożność podczas planowania i projektowania preferencji aplikacji i systemu windows.

Dla dowolnego okna, które będą korzystać z aplikacji preferencji, dodaj następującą właściwość pomocnika do jego zawartości kontrolera widoku, aby podczas uzyskiwania dostępu do naszej **AppDelegate** łatwiejsze:

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

Następnie Dodaj klasę, aby skonfigurować zawartości lub zachowania zgodnie z preferencjami użytkownika:

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

Należy wywołać metodę konfiguracji po pierwszym otwarciu okna do upewnij się, że spełnia on preferencje użytkownika:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

Następnie Edytuj `AppDelegate.cs` i dodaj następującą metodę do zastosowania żadnego zmienia się na wszystkie otwarte okna:

```csharp
public void UpdateWindowPreferences() {

    // Process all open windows
    for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
        var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
        if (content != null ) {
            // Reformat all text
            content.ConfigureEditor ();
        }
    }

}
```

Następnie dodaj `PreferenceWindowDelegate` klas do projektu i przydzielić mu wyglądać następująco:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWidowDelegate : NSWindowDelegate
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public PreferenceWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            
            // Apply any changes to open windows
            App.UpdateWindowPreferences();

            return true;
        }
        #endregion
    }
}
```

Spowoduje to zmiany preferencji przesyłany do wszystkich okien po zamknięciu okna preferencji.

Na koniec Edytuj kontrolera okna preferencji i dodać delegata utworzone powyżej:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class PreferenceWindowController : NSWindowController
    {
        #region Constructors
        public PreferenceWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Initialize
            Window.Delegate = new PreferenceWidowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

Wszystkie wprowadzone zmiany w miejscu Jeśli użytkownik edytuje Preferencje aplikacji i zamyka okno preferencji zmiany będą dotyczyć wszystkich okien:

[![](dialog-images/prefs14.png "Przykład preferencje okna")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>Otwórz okno dialogowe

Otwórz okno dialogowe umożliwia użytkownikom spójny sposób, aby znaleźć i otworzyć element w aplikacji. Aby wyświetlić okno dialogowe Otwórz w aplikacji Xamarin.Mac, należy użyć poniższego kodu:

```csharp
var dlg = NSOpenPanel.OpenPanel;
dlg.CanChooseFiles = true;
dlg.CanChooseDirectories = false;
dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

if (dlg.RunModal () == 1) {
    // Nab the first file
    var url = dlg.Urls [0];

    if (url != null) {
        var path = url.Path;

        // Create a new window to hold the text
        var newWindowController = new MainWindowController ();
        newWindowController.Window.MakeKeyAndOrderFront (this);

        // Load the text into the window
        var window = newWindowController.Window as MainWindow;
        window.Text = File.ReadAllText(path);
        window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
        window.RepresentedUrl = url;

    }
}
```

W powyższym kodzie możemy są otworzyć nowe okno dokumentu, aby wyświetlić zawartość pliku. Należy zamienić na ten kod z funkcjami jest wymagane przez aplikację.

Dostępne są następujące właściwości, podczas pracy z `NSOpenPanel`:

- **CanChooseFiles** — Jeśli `true` użytkownik może wybrać pliki.
- **CanChooseDirectories** — Jeśli `true` użytkownik może wybrać katalogów.
- **AllowsMultipleSelection** — Jeśli `true` użytkownik może wybrać więcej niż jeden plik w czasie.
- **ResolveAliases** — Jeśli `true` wybierając i aliasu, jest on rozpoznawane jako ścieżka oryginalnego pliku.
- **AllowedFileTypes** — jest tablicą ciągów typów plików, które użytkownik może wybrać jako albo rozszerzenie lub _UTI_. Wartość domyślna to `null`, dzięki czemu każdy plik ma zostać otwarty.

`RunModal ()` Metoda Wyświetla okno dialogowe Otwórz i Zezwalaj użytkownikowi na wybranie plików lub katalogów (określoną przez właściwości) i zwraca `1` gdy użytkownik kliknie **Otwórz** przycisku.

Otwórz okno dialogowe Zwraca wybrane pliki lub katalogi użytkownika w postaci tablicy adresów URL w `URL` właściwości.

Jeśli firma Microsoft uruchomić program i wybrać **Otwórz...**  elementu z **pliku** zostanie wyświetlone menu następujące czynności: 

[![](dialog-images/dialog03.png "Otwarte okno dialogowe")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>Drukowanie i okna dialogowe Ustawienia strony

System macOS zawiera standardowe drukowania i okna Ustawienia strony, których aplikacja może wyświetlać, dzięki czemu użytkownicy mogą mieć zgodne drukowanie doświadczenie w każdej aplikacji, których używają.

Poniższy kod wyświetli standardowe okno dialogowe drukowania:

```csharp
public bool ShowPrintAsSheet { get; set;} = true;
...

[Export ("showPrinter:")]
void ShowDocument (NSObject sender) {
    var dlg = new NSPrintPanel();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet(new NSPrintInfo(),this,this,null,new IntPtr());
    } else {
        if (dlg.RunModalWithPrintInfo(new NSPrintInfo()) == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}

```

Jeśli firma Microsoft `ShowPrintAsSheet` właściwości `false`, uruchom aplikację i wyświetla okno dialogowe drukowania, wyświetlane są następujące:

[![](dialog-images/print01.png "Okno dialogowe drukowania")](dialog-images/print01.png#lightbox)

Jeśli wartość `ShowPrintAsSheet` właściwości `true`, uruchom aplikację i wyświetla okno dialogowe drukowania, wyświetlane są następujące:

[![](dialog-images/print02.png "Arkusz wydruku")](dialog-images/print02.png#lightbox)

Poniższy kod wyświetli okno dialogowe Układ strony:

```csharp
[Export ("showLayout:")]
void ShowLayout (NSObject sender) {
    var dlg = new NSPageLayout();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet (new NSPrintInfo (), this);
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}
```

Jeśli firma Microsoft `ShowPrintAsSheet` właściwości `false`, uruchom aplikację i wyświetla okno dialogowe Układ wydruku, wyświetlane są następujące:

[![](dialog-images/print03.png "Okno dialogowe Ustawienia strony")](dialog-images/print03.png#lightbox)

Jeśli wartość `ShowPrintAsSheet` właściwości `true`, uruchom aplikację i wyświetla okno dialogowe Układ wydruku, wyświetlane są następujące:

[![](dialog-images/print04.png "Arkusz Ustawienia strony")](dialog-images/print04.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z drukowania i okna dialogowe Ustawienia strony, zobacz firmy Apple [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) i [wprowadzenie do drukowania](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) dokumentacja.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>Zapisz okna dialogowego

Okno dialogowe Zapisz umożliwia użytkownikom spójny sposób, aby zapisać element w aplikacji.

Poniższy kod wyświetli standardowe okno dialogowe Zapisz:

```csharp
public bool ShowSaveAsSheet { get; set;} = true;
...

[Export("saveDocumentAs:")]
void ShowSaveAs (NSObject sender)
{
    var dlg = new NSSavePanel ();
    dlg.Title = "Save Text File";
    dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

    if (ShowSaveAsSheet) {
        dlg.BeginSheet(mainWindowController.Window,(result) => {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        });
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    }

}
```

`AllowedFileTypes` Właściwości jest tablicą ciągów typów plików, które użytkownik może wybrać, aby zapisać plik jako. Typ pliku może być określone jako rozszerzenie lub _UTI_. Wartość domyślna to `null`, co umożliwia dowolny typ pliku do użycia.

Jeśli firma Microsoft `ShowSaveAsSheet` właściwości `false`, uruchom aplikację i wybierz **Zapisz jako...**  z **pliku** menu, wyświetlane są następujące:

[![](dialog-images/save01.png "Zapisywanie — okno dialogowe")](dialog-images/save01.png#lightbox)

Użytkownika można rozszerzyć okna dialogowego:

[![](dialog-images/save02.png "Zapisz rozwinięte — okno dialogowe")](dialog-images/save02.png#lightbox)

Jeśli firma Microsoft `ShowSaveAsSheet` właściwości `true`, uruchom aplikację i wybierz **Zapisz jako...**  z **pliku** menu, wyświetlane są następujące:

[![](dialog-images/save03.png "Zapisywanie arkusza")](dialog-images/save03.png#lightbox)

Użytkownika można rozszerzyć okna dialogowego:

[![](dialog-images/save04.png "Rozwinięte zapisać arkusza")](dialog-images/save04.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z okna dialogowego Zapisywanie, zobacz firmy Apple [NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) dokumentacji.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowy widok okna modalne, arkuszy i standardowym systemie okien dialogowych w aplikacji Xamarin.Mac. Widzieliśmy różne typy i używa okna modalne, arkuszy i okna dialogowe, w jaki sposób można tworzyć i obsługiwać modalnych okien i arkuszy w środowisku Xcode's konstruktora interfejsu i sposobu pracy z okna modalne, arkusze i okna dialogowe w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacWindows (przykład)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Menu](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [Paski narzędzi](~/mac/user-interface/toolbar.md)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do systemu Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Wprowadzenie do arkuszy](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Wprowadzenie do drukowania](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
