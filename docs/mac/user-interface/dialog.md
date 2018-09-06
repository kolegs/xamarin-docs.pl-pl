---
title: Okna dialogowe platformie Xamarin.Mac
description: Ten artykuł dotyczy pracy z okien dialogowych i okna modalne w aplikacji platformy Xamarin.Mac. Opisuje tworzenie modalnych okien w programie Xcode i interfejs builder pracę w standardowych oknach dialogowych i korzystania z tych kontrolek w kodzie języka C#.
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 2f28b52b4904b73f97cd9da575e90ef583e443da
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780632"
---
# <a name="dialogs-in-xamarinmac"></a>Okna dialogowe platformie Xamarin.Mac

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tych samych okien dialogowych i modalne Windows, deweloper pracy w *języka Objective-C* i *Xcode* jest. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, można użyć program Xcode _programu Interface Builder_ Aby utworzyć i zachować swoje modalne Windows (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Okno dialogowe pojawia się w odpowiedzi na akcję użytkownika i zwykle zapewnia, że użytkownicy sposobów można ukończyć akcji. Okno dialogowe wymaga odpowiedź od użytkownika może zostać zamknięty.

Windows może być używany w stanie niemodalne (takich jak edytor tekstu, który może mieć wiele dokumentów, Otwórz na raz) lub modalne (np. Eksport okno dialogowe, które muszą być ukryty przed kontynuowaniem aplikacji).

[![](dialog-images/dialog03.png "Otwarte okno dialogowe")](dialog-images/dialog03.png#lightbox)

W tym artykule omówiono podstawowe informacje dotyczące pracy z oknami dialogowymi i modalne Windows w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` poleceń Umożliwia o komunikacji sieciowej w górę klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>Wprowadzenie do okien dialogowych

Okno dialogowe pojawia się w odpowiedzi na akcję użytkownika (na przykład zapisanie pliku) i umożliwia użytkownikom ukończyć tę akcję. Okno dialogowe wymaga odpowiedź od użytkownika może zostać zamknięty.

Zgodnie z firmy Apple istnieją trzy sposoby na wyświetlenie okna dialogowego:

- **Dokumentowanie modalne** -dokumentu modalne okno dialogowe zapobiega wykonywania jakichkolwiek innych czynności w ramach danego dokumentu, dopóki nie jest on odrzucony przez użytkownika.
- **Modalne aplikacji** — aplikacja modalne okno dialogowe uniemożliwia użytkownikowi wchodzenie w interakcje z aplikacją, dopóki nie jest on odrzucony.
- **Niemodalne** niemodalne okno umożliwia użytkownikom zmianę ustawień w oknie dialogowym podczas nadal interakcji z okna dokumentu.

### <a name="modal-window"></a>Okno modalne

Wszystkie standardowe `NSWindow` może służyć jako niestandardowe okno dialogowe, wyświetlając go w trybie modalnym:

[![](dialog-images/modal01.png "Przykładowe okno modalne")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>Arkusze modalne okno dialogowe dokumentu

A _arkusza_ jest modalne okno dialogowe, który jest dołączony do danego dokumentu okna, uniemożliwiając użytkownikom interakcji z oknem, dopóki ich zamknąć okno dialogowe. Arkusz jest dołączony do okna, z którego wynika, i tylko jeden arkusz może być otwarty w oknie, w dowolnym momencie.

[![](dialog-images/sheet08.png "Przykład arkusza modalne")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>Preferencje Windows

Okno preferencji jest niemodalne okno dialogowe, które zawiera ustawienia aplikacji, które użytkownik zmienia się rzadko. Preferencje Windows często zawierają pasek narzędzi, który umożliwia użytkownikowi przełączać się między różnymi grupami ustawień:

[![](dialog-images/dialog02.png "Przykład preferencji okna")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>Otwórz okno dialogowe

Otwórz okno dialogowe zapewnia użytkownikom spójny sposób, aby znaleźć i otworzyć element w aplikacji:

[![](dialog-images/dialog03.png "Otwórz okno dialogowe")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>Drukowanie i okna dialogowe Ustawienia strony

macOS zawiera standardowe drukowania i strony Instalatora okien dialogowych, które aplikacja może wyświetlać, dzięki czemu użytkownicy mają spójnego drukowanie środowisko w każdej aplikacji, których używają.

Okno dialogowe drukowania mogą być wyświetlane jako zarówno bezpłatne przestawne okno dialogowe:

[![](dialog-images/print01.png "Okno dialogowe drukowania")](dialog-images/print01.png#lightbox)

Lub mogą być wyświetlane jako arkusz:

[![](dialog-images/print02.png "Drukowanie arkuszy")](dialog-images/print02.png#lightbox)

Okno dialogowe Ustawienia strony mogą być wyświetlane jako zarówno bezpłatne przestawne okno dialogowe:

[![](dialog-images/print03.png "Okno dialogowe Ustawienia strony")](dialog-images/print03.png#lightbox)

Lub mogą być wyświetlane jako arkusz:

[![](dialog-images/print04.png "Arkusz konfiguracji strony")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>Zapisz okien dialogowych

Okno dialogowe Zapisz zapewnia użytkownikom spójny sposób, aby zapisać element w aplikacji. Okno dialogowe Zapisz ma dwa stany: **minimalny** (znany także jako zwinięty):

[![](dialog-images/save01.png "Zapisywanie okna dialogowego")](dialog-images/save01.png#lightbox)

I **rozwinięte** stanu:

[![](dialog-images/save02.png "Okno dialogowe zapisywania rozwinięty")](dialog-images/save02.png#lightbox)

**Minimalny** Zapisz z okna dialogowego, również mogą być wyświetlane jako arkusz:

[![](dialog-images/save03.png "Minimalny Zapisz arkusz")](dialog-images/save03.png#lightbox)

Jak **rozwinięte** Zapisz okno dialogowe:

[![](dialog-images/save04.png "Zapisz arkusz rozwinięty")](dialog-images/save04.png#lightbox)

Aby uzyskać więcej informacji, zobacz [okien dialogowych](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Dodawanie modalny do projektu

Oprócz okna głównego dokumentu aplikacji rozszerzenia Xamarin.Mac może być konieczne mają być wyświetlane inne rodzaje systemu windows użytkownika, takie jak preferencje lub inspektor paneli.

Aby dodać nowe okno, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, otwórz `Main.storyboard` plik do edycji w program Xcode Interface Builder.
2. Przeciągnij nowy **kontrolera widoku** do powierzchni projektu:

    [![](dialog-images/new01.png "Wybiera kontroler widoku z biblioteki")](dialog-images/new01.png#lightbox)
3. W **Inspektor tożsamości**, wprowadź `CustomDialogController` dla **Nazwa klasy**: 

    [![](dialog-images/new02.png "Ustawianie nazwy klasy")](dialog-images/new02.png#lightbox)
4. Przejdź z powrotem do programu Visual Studio dla komputerów Mac, będzie możliwe jego synchronizacji z narzędziem Xcode i utworzyć `CustomDialogController.h` pliku.
5. Wróć do programu Xcode i projektować interfejs użytkownika: 

    [![](dialog-images/new03.png "Projektowanie interfejsu użytkownika w środowisku Xcode")](dialog-images/new03.png#lightbox)
6. Tworzenie **modalne Segue** z okna głównego aplikacji na nowy kontroler widoku, przeciągając kontrolki z elementu interfejsu użytkownika, który spowoduje otwarcie okna dialogowego do okna dialogowego. Przypisz **identyfikator** `ModalSegue`: 

    [![](dialog-images/new06.png "Modalne segue")](dialog-images/new06.png#lightbox)
6. O komunikacji sieciowej w górę dowolne **akcje** i **gniazd**: 

    [![](dialog-images/new04.png "Konfigurowanie akcji")](dialog-images/new04.png#lightbox)
6. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Wprowadź `CustomDialogController.cs` wygląd pliku, jak pokazano poniżej:

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

Ten kod udostępnia kilka właściwości, aby ustawić tytuł i opis w oknie dialogowym i kilka zdarzeń, aby reagować na oknie dialogowym są anulowane lub zaakceptowane.

Następnie Edytuj `ViewController.cs` pliku, Zastąp `PrepareForSegue` metody i przypisz ją wyglądać podobnie do następującego:

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

Ten kod inicjalizuje segue, który zdefiniowaliśmy w program Xcode Interface Builder na naszych okna dialogowego i konfiguruje tytuł i opis. Obsługuje ona również wybór użytkownika sprawia, że w oknie dialogowym.

Firma Microsoft można uruchomić aplikację i wyświetlić niestandardowe okno dialogowe:

[![](dialog-images/new05.png "Przykładowe okno dialogowe")](dialog-images/new05.png#lightbox)

Aby uzyskać więcej informacji na temat korzystania z systemu windows w aplikacji platformy Xamarin.Mac, zobacz nasze [pracy z Windows](~/mac/user-interface/window.md) dokumentacji.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Tworzenie niestandardowych arkusza

A _arkusza_ jest modalne okno dialogowe, który jest dołączony do danego dokumentu okna, uniemożliwiając użytkownikom interakcji z oknem, dopóki ich zamknąć okno dialogowe. Arkusz jest dołączony do okna, z którego wynika, i tylko jeden arkusz może być otwarty w oknie, w dowolnym momencie. 

Utwórz arkusz niestandardowe platformie Xamarin.Mac, możemy wykonać następujące czynności:

1. W **Eksploratora rozwiązań**, otwórz `Main.storyboard` plik do edycji w program Xcode Interface Builder.
2. Przeciągnij nowy **kontrolera widoku** do powierzchni projektu:

    [![](dialog-images/new01.png "Wybiera kontroler widoku z biblioteki")](dialog-images/new01.png#lightbox)
2. Projektowanie interfejsu użytkownika:

    [![](dialog-images/sheet01.png "Projekt interfejsu użytkownika")](dialog-images/sheet01.png#lightbox)
3. Tworzenie **Segue arkusza** z okna głównego nowy kontroler widoku: 

    [![](dialog-images/sheet02.png "Wybieranie typu segue arkusza")](dialog-images/sheet02.png#lightbox)
4. W **Inspektor tożsamości**, nazwę kontrolera widoku **klasy** `SheetViewController`: 

    [![](dialog-images/sheet03.png "Ustawianie nazwy klasy")](dialog-images/sheet03.png#lightbox)
5. Zdefiniuj dowolny potrzebne **gniazd** i **akcje**: 

    [![](dialog-images/sheet04.png "Definiowanie wymagane punkty i akcje")](dialog-images/sheet04.png#lightbox)
6. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji.

Następnie Edytuj `SheetViewController.cs` plików i przypisz ją wyglądać podobnie do następującego:

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

Następnie Edytuj `ViewController.cs` plik, Edytuj `PrepareForSegue` metody i przypisz ją wyglądać podobnie do następującego:

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

Jeśli firma Microsoft może uruchomić aplikację, a następnie otwórz kartę, zostanie dołączony do okna:

[![](dialog-images/sheet08.png "Przykład arkusza")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Tworzenie okna dialogowego preferencji

Zanim firma Zaprojektuj widok preferencji programu Interface Builder musimy dodać typ segue niestandardowe do obsługi przełączania z preferencji. Dodaj nową klasę do projektu, a następnie wywołaj ją `ReplaceViewSeque `. Edytuj klasy i przypisz ją wyglądać następująco:

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

Za pomocą segue niestandardowy, utworzony możemy dodać nowe okno w program Xcode Interface Builder, aby obsłużyć nasz Preferencje.

Aby dodać nowe okno, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, otwórz `Main.storyboard` plik do edycji w program Xcode Interface Builder.
2. Przeciągnij nowy **kontroler okna** do powierzchni projektu:

    [![](dialog-images/pref01.png "Wybierz kontroler okna z biblioteki")](dialog-images/pref01.png#lightbox)
3. Rozmieść okna w pobliżu **pasek Menu** projektanta:

    [![](dialog-images/pref02.png "Dodawanie nowego okna")](dialog-images/pref02.png#lightbox)
4. Tworzenie kopii dołączonych kontrolera widoku, ponieważ w widoku preferencji będzie karty:

    [![](dialog-images/pref03.png "Dodawanie wymaganych kontrolerów widoku")](dialog-images/pref03.png#lightbox)
5. Przeciągnij nowy **kontrolera narzędzi** z **biblioteki**:

    [![](dialog-images/pref04.png "Wybierz kontroler paska narzędzi z biblioteki")](dialog-images/pref04.png#lightbox)
6. I upuść je na okna na powierzchni projektowej:

    [![](dialog-images/pref05.png "Dodawanie nowego kontrolera paska narzędzi")](dialog-images/pref05.png#lightbox)
7. Układ projektu paska narzędzi:

    [![](dialog-images/pref06.png "Układ paska narzędzi")](dialog-images/pref06.png#lightbox)
8. Klawisz Control, kliknij i przeciągnij każdego z nich **przycisku paska narzędzi** do widoków, które zostały utworzone powyżej. Wybierz **niestandardowe** segue typu:

    [![](dialog-images/pref07.png "Ustawianie typu segue")](dialog-images/pref07.png#lightbox)
9. Wybierz nowy Segue i ustaw **klasy** do `ReplaceViewSegue`:

    [![](dialog-images/pref08.png "Klasa segue ustawień")](dialog-images/pref08.png#lightbox)
10. W **projektanta Menubar** na powierzchni projektowej, w Menu aplikacji wybierz **Preferencje...** klawisz control, kliknij i przeciągnij okno Preferencje, aby utworzyć **Pokaż** segue:

    [![](dialog-images/pref09.png "Ustawianie typu segue")](dialog-images/pref09.png#lightbox)
11. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji.

Jeśli możemy uruchomić kod i wybierz pozycję **Preferencje...**  z **Menu aplikacji**, zostanie wyświetlone okno:

[![](dialog-images/pref10.png "Przykładowe okno preferencji")](dialog-images/pref10.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z Windows i paski narzędzi, zobacz nasze [Windows](~/mac/user-interface/window.md) i [pasków narzędzi](~/mac/user-interface/toolbar.md) dokumentacji.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Zapisywanie i ładowanie preferencje

W typowym macOS aplikacji po użytkownik wprowadza zmiany do któregokolwiek elementu preferencji użytkownika aplikacji, zmiany zostaną zapisane automatycznie. Najprostszym sposobem obsługi to w przypadku aplikacji platformy Xamarin.Mac jest utworzyć jedną klasę, aby zarządzać wszystkimi preferencje użytkownika i udostępnij go całego systemu.

Najpierw dodaj nową `AppPreferences` klasy do projektu i dziedziczy `NSObject`. Preferencje, będzie przeznaczony do stosowania [powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md) który spowoduje, że proces tworzenia i utrzymywania Preferencje formularzy dużo prostsze. Ponieważ preferencje będzie składać się z krótkim proste typy danych, należy użyć wbudowanych w `NSUserDefaults` do przechowywania i pobierania wartości.

Edytuj `AppPreferences.cs` plików i przypisz ją wyglądać podobnie do następującego:

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

Ta klasa zawiera kilka procedur Pomocnika `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`, itp., aby ułatwić pracę z `NSUserDefaults` łatwiejsze. Ponadto ponieważ `NSUserDefaults` nie ma wbudowanych sposób obsługi `NSColors`, `NSColorToHexString` i `NSColorFromHexString` metody są używane do konwersji kolorów do ciągów szesnastkowych opartego na sieci web (`#RRGGBBAA` gdzie `AA` alfa przezroczystości jest), które można łatwo przechowywane i pobierane.

W `AppDelegate.cs` plików, Utwórz wystąpienie obiektu **AppPreferences** obiektu, który będzie używany w całej aplikacji:

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

### <a name="wiring-preferences-to-preference-views"></a>Preferencje okablowania z widokami preferencji

Następnie połącz klasy preferencji do elementów interfejsu użytkownika w oknie preferencji i widoki utworzone powyżej. W programu Interface Builder wybierz kontroler widoku preferencji, a następnie przełącz się do **Inspektor tożsamości**, utworzyć klasę niestandardową dla kontrolera: 

[![](dialog-images/prefs12.png "Inspektor tożsamości")](dialog-images/prefs12.png#lightbox)

Przełącz się do programu Visual Studio dla komputerów Mac zsynchronizować zmiany i otwórz nowo utworzony klasę do edycji. Wprowadź klasę wyglądać następująco:

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

Należy zauważyć, że ta klasa została wykonana tutaj dwie rzeczy: po pierwsze, istnieje Pomocnika `App` właściwości, które umożliwiają uzyskiwanie dostępu do **elemencie AppDelegate** łatwiejsze. Drugi `Preferences` właściwość udostępnia globalną **AppPreferences** klasy powiązanie danych z żadną kontrolkę interfejsu użytkownika jest umieszczana w ramach tego widoku.

Następnie dwukrotnie kliknij plik scenorysu, otwórz go ponownie w Konstruktorze interfejsu (i zobaczyć zmiany wprowadzone przed chwilą powyżej). Przeciągnij żadną kontrolkę interfejsu użytkownika, które są wymagane do tworzenia interfejsu preferencje do widoku. Dla każdego formantu, przełącz się do **powiązanie Inspektor** i powiąż go z poszczególnych właściwości **AppPreference** klasy:

[![](dialog-images/prefs13.png "Inspektor powiązania")](dialog-images/prefs13.png#lightbox)

Powtórz powyższe kroki dla wszystkich zespołów (kontrolerów widoku) i wymaganych właściwości preferencji.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>Stosowanie preferencji zmienia się na wszystkie otwarte Windows

Jak wspomniano powyżej, w typowych systemu macOS, aplikacji, gdy użytkownik wprowadza zmiany do któregokolwiek elementu preferencji użytkownika aplikacji, te zmiany zostaną zapisane automatycznie i zastosowane do wszystkich oknach użytkownika, może być otwarty w aplikacji.

Ten proces, do wykonania bez problemów i w sposób niewidoczny dla użytkownika końcowego z minimalną ilością pracy kodowania umożliwi starannego planowania i projektowania Preferencje aplikacji i systemu windows.

W oknie, które będą korzystać z aplikacji preferencje, dodaj następującą właściwość pomocnika do jego zawartości kontrolera widoku, aby upewnić się, uzyskiwanie dostępu do naszych **elemencie AppDelegate** łatwiejsze:

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

Następnie Dodaj klasę do konfigurowania zawartości lub zachowania, w oparciu o preferencje użytkownika:

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

Musisz wywołać tej metody konfiguracji po pierwszym otwarciu okna, aby upewnić się, że spełnia on preferencje użytkownika:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

Następnie Edytuj `AppDelegate.cs` pliku i dodaj następującą metodę do zastosowania żadnych preferencji zmienia się na wszystkie otwarte okna:

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

Następnie dodaj `PreferenceWindowDelegate` klasy do projektu i przypisz ją wyglądać podobnie do następującego:

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

Spowoduje to wysłanie do wszystkich otwartych Windows po zamknięciu okna preferencji zmiany preferencji.

Na koniec edytować kontroler okna preferencji i dodanie delegatu utworzone powyżej:

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

W przypadku wszystkich tych zmian w miejscu, jeśli użytkownik edytuje Preferencje aplikacji i zamyka okno preferencji zmiany zostaną zastosowane do wszystkich otwartych Windows:

[![](dialog-images/prefs14.png "Przykładowe okno preferencji")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>Otwórz okno dialogowe

Otwórz okno dialogowe umożliwia użytkownikom spójny sposób, aby znaleźć i otworzyć element w aplikacji. Aby wyświetlić okno dialogowe Otwórz w aplikacji platformy Xamarin.Mac, użyj następującego kodu:

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

W powyższym kodzie otwieramy nowe okno dokumentu, aby wyświetlić zawartość pliku. Należy zamienić na ten kod za pomocą funkcji jest wymagana przez aplikację.

Następujące właściwości są dostępne podczas pracy z `NSOpenPanel`:

- **CanChooseFiles** — Jeśli `true` użytkownik może wybrać pliki.
- **CanChooseDirectories** — Jeśli `true` użytkownik może wybrać katalogów.
- **AllowsMultipleSelection** — Jeśli `true` użytkownik może wybrać więcej niż jeden plik jednocześnie.
- **ResolveAliases** — Jeśli `true` wybranie i alias, rozwiązuje to ścieżka oryginalnego pliku.
- **AllowedFileTypes** — jest tablicą ciągów z typów plików, które użytkownik może wybrać jako rozszerzenie albo lub _identyfikator UTI_. Wartość domyślna to `null`, co umożliwia dowolny plik do otwarcia.

`RunModal ()` Metoda wyświetla otwarte okno dialogowe i umożliwia użytkownikowi wybranie plików lub katalogów (określone przez właściwości) i zwraca `1` Jeśli użytkownik kliknie **Otwórz** przycisku.

Otwórz okno dialogowe zwraca wybranych plików lub katalogów użytkownika jako tablicę adresów URL w `URL` właściwości.

Jeśli firma Microsoft jest uruchomienie programu i wybierz pozycję **Otwórz...**  elementu z **pliku** zostanie wyświetlone menu z następujących czynności: 

[![](dialog-images/dialog03.png "Otwarte okno dialogowe")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>Drukowanie i okna dialogowe Ustawienia strony

macOS zawiera standardowe drukowania i strony Instalatora okien dialogowych, które aplikacja może wyświetlać, dzięki czemu użytkownicy mają spójnego drukowanie środowisko w każdej aplikacji, których używają.

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

Jeśli ustawimy `ShowPrintAsSheet` właściwości `false`, uruchom aplikację i wyświetlić okno dialogowe drukowania, wyświetlane są następujące:

[![](dialog-images/print01.png "Okno dialogowe drukowania")](dialog-images/print01.png#lightbox)

Jeśli ustawiona `ShowPrintAsSheet` właściwości `true`, uruchom aplikację i wyświetlić okno dialogowe drukowania, wyświetlane są następujące:

[![](dialog-images/print02.png "Drukowanie arkuszy")](dialog-images/print02.png#lightbox)

Poniższy kod wyświetli okno dialogowe układu strony:

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

Jeśli ustawimy `ShowPrintAsSheet` właściwości `false`, uruchom aplikację i wyświetlić okno dialogowe Układ wydruku, wyświetlane są następujące:

[![](dialog-images/print03.png "Okno dialogowe Ustawienia strony")](dialog-images/print03.png#lightbox)

Jeśli ustawiona `ShowPrintAsSheet` właściwości `true`, uruchom aplikację i wyświetlić okno dialogowe Układ wydruku, wyświetlane są następujące:

[![](dialog-images/print04.png "Arkusz konfiguracji strony")](dialog-images/print04.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z drukowania i okna dialogowe Ustawienia strony, zobacz firmy Apple [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) i [wprowadzenie do drukowania](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) dokumentacja.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>Zapisz okna dialogowego

Okno dialogowe Zapisz zapewnia użytkownikom spójny sposób, aby zapisać element w aplikacji.

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

`AllowedFileTypes` Właściwość jest tablicą ciągów typów plików, które użytkownik może wybrać, aby zapisać plik jako. Typ pliku można określić jako rozszerzenie lub _identyfikator UTI_. Wartość domyślna to `null`, która zezwala na wszystkie typy plików do użycia.

Jeśli ustawimy `ShowSaveAsSheet` właściwości `false`, uruchom aplikację i wybierz **Zapisz jako...**  z **pliku** menu wyświetlane są następujące:

[![](dialog-images/save01.png "Zapisywanie okno dialogowe")](dialog-images/save01.png#lightbox)

Użytkownika można rozwinąć w oknie dialogowym:

[![](dialog-images/save02.png "Rozwinięty Zapisz — okno dialogowe")](dialog-images/save02.png#lightbox)

Jeśli ustawimy `ShowSaveAsSheet` właściwości `true`, uruchom aplikację i wybierz **Zapisz jako...**  z **pliku** menu wyświetlane są następujące:

[![](dialog-images/save03.png "Zapisywanie arkusza")](dialog-images/save03.png#lightbox)

Użytkownika można rozwinąć w oknie dialogowym:

[![](dialog-images/save04.png "Zapisz arkusz rozwinięty")](dialog-images/save04.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z okna dialogowego Zapisywanie, zobacz firmy Apple [NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) dokumentacji.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z Windows modalne, arkusze i system standardowych oknach dialogowych w aplikacji platformy Xamarin.Mac. Widzieliśmy różne typy i używa modalnego Windows, arkuszy i oknach dialogowych, w jaki sposób tworzyć i obsługiwać modalne Windows i arkusze w środowisku Xcode użytkownika programu Interface Builder i sposób pracy z Windows modalne, arkusze i okien dialogowych w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacWindows (przykład)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Menu](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [Paski narzędzi](~/mac/user-interface/toolbar.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Wprowadzenie do arkuszy](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Wprowadzenie do drukowania](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
