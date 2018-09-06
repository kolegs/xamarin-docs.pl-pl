---
title: Listy źródeł na platformie Xamarin.Mac
description: Ten artykuł dotyczy pracy z listy źródeł w aplikacji platformy Xamarin.Mac. Opisano w nim tworzenie i utrzymywanie listy źródeł w środowisku Xcode i programu Interface Builder i interakcji z nimi w kodzie języka C#.
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: de51395192ab009f5c7fa338a4414ba553610b8c
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780623"
---
# <a name="source-lists-in-xamarinmac"></a>Listy źródeł na platformie Xamarin.Mac

_Ten artykuł dotyczy pracy z listy źródeł w aplikacji platformy Xamarin.Mac. Opisano w nim tworzenie i utrzymywanie listy źródeł w środowisku Xcode i programu Interface Builder i interakcji z nimi w kodzie języka C#._

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tej samej źródła Wyświetla deweloperem, pracy w *języka Objective-C* i *Xcode* jest. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, można użyć program Xcode _programu Interface Builder_ Aby utworzyć i obsługiwać swoje listy źródłowej (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Lista źródeł to specjalny rodzaj widoku konspektu, używane do wyświetlania źródła akcji, takich jak pasek boczny w programie Finder lub programu iTunes.

[![](source-list-images/source05.png "Przykład listy źródłowej")](source-list-images/source05.png#lightbox)

W tym artykule omówimy podstawy pracy z listami źródła w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` poleceń Umożliwia o komunikacji sieciowej w górę klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>Wprowadzenie do listy źródeł

Jak wspomniano powyżej, lista źródeł jest specjalny rodzaj widoku konspektu używane do wyświetlania źródła akcji, takich jak pasek boczny w programie Finder lub programu iTunes. Lista źródeł jest typem tabeli, który umożliwia użytkownikowi rozwinąć lub zwinąć wiersze danych hierarchicznych. W przeciwieństwie do widoku tabeli elementy na liście źródłowej nie są w formie płaskiej listy, są zorganizowane w hierarchii, takie jak pliki i foldery na dysku twardym. Jeśli element na liście źródło zawiera inne elementy, można można rozwijać i zwijać przez użytkownika.

Lista źródeł jest specjalnie ze stylem widoku konspektu (`NSOutlineView`), który sam jest podklasą widok tabeli (`NSTableView`) i w efekcie dziedziczy większość swojego zachowania od swojej klasy nadrzędnej. W rezultacie wiele operacji obsługiwanych przez wyświetlanie konspektu, są również obsługiwane przez listy źródeł. Aplikacja platformy Xamarin.Mac ma kontrolę nad tych funkcji, a następnie można skonfigurować parametrów listy źródeł, (albo w kodzie albo programu Interface Builder), aby umożliwiać lub uniemożliwiać pewne operacje.

Lista źródeł nie przechowuje swoich danych, zamiast tego opiera się na źródle danych (`NSOutlineViewDataSource`) wiersze i kolumny są wymagane na zgodnie z potrzebami.

Zachowanie listę źródła można dostosować, podając podklasę delegata wyświetlanie konspektu (`NSOutlineViewDelegate`) na potrzeby obsługi typu konspektu, aby wybrać funkcje, element wyboru i edytowania, niestandardowe śledzenia i widoki niestandardowe dla poszczególnych elementów.

Ponieważ listy źródeł udostępni wiele jej działanie oraz funkcje widok tabeli i konspektu, możesz chcieć przejść przez naszych [widoki tabel](~/mac/user-interface/table-view.md) i [widoki konspektu](~/mac/user-interface/outline-view.md) dokumentacji, przed kontynuowaniem z tego artykułu.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>Praca z listy źródeł

Lista źródeł to specjalny rodzaj widoku konspektu, używane do wyświetlania źródła akcji, takich jak pasek boczny w programie Finder lub programu iTunes. W odróżnieniu od widoki konspektu przed definiujemy naszej listy źródeł w programu Interface Builder Utwórzmy klasy zapasowy platformie Xamarin.Mac.

Po pierwsze Utwórzmy nowy `SourceListItem` klasy do przechowywania danych z naszej listy źródłowej. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `SourceListItem` dla **nazwa** i kliknij przycisk **New** przycisku:

[![](source-list-images/source01.png "Dodawanie pustą klasę")](source-list-images/source01.png#lightbox)

Wprowadź `SourceListItem.cs` wygląd pliku, jak pokazano poniżej: 

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListItem: NSObject, IEnumerator, IEnumerable
    {
        #region Private Properties
        private string _title;
        private NSImage _icon;
        private string _tag;
        private List<SourceListItem> _items = new List<SourceListItem> ();
        #endregion

        #region Computed Properties
        public string Title {
            get { return _title; }
            set { _title = value; }
        }

        public NSImage Icon {
            get { return _icon; }
            set { _icon = value; }
        }

        public string Tag {
            get { return _tag; }
            set { _tag=value; }
        }
        #endregion

        #region Indexer
        public SourceListItem this[int index]
        {
            get
            {
                return _items[index];
            }

            set
            {
                _items[index] = value;
            }
        }

        public int Count {
            get { return _items.Count; }
        }

        public bool HasChildren {
            get { return (Count > 0); }
        }
        #endregion

        #region Enumerable Routines
        private int _position = -1;

        public IEnumerator GetEnumerator()
        {
            _position = -1;
            return (IEnumerator)this;
        }

        public bool MoveNext()
        {
            _position++;
            return (_position < _items.Count);
        }

        public void Reset()
        {_position = -1;}

        public object Current
        {
            get 
            { 
                try 
                {
                    return _items[_position];
                }

                catch (IndexOutOfRangeException)
                {
                    throw new InvalidOperationException();
                }
            }
        }
        #endregion

        #region Constructors
        public SourceListItem ()
        {
        }

        public SourceListItem (string title)
        {
            // Initialize
            this._title = title;
        }

        public SourceListItem (string title, string icon)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
        }

        public SourceListItem (string title, string icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
        }

        public SourceListItem (string title, NSImage icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon, string tag)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
        }

        public SourceListItem (string title, NSImage icon, string tag, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
            this.Clicked = clicked;
        }
        #endregion

        #region Public Methods
        public void AddItem(SourceListItem item) {
            _items.Add (item);
        }

        public void AddItem(string title) {
            _items.Add (new SourceListItem (title));
        }

        public void AddItem(string title, string icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, string icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, NSImage icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon, string tag) {
            _items.Add (new SourceListItem (title, icon, tag));
        }

        public void AddItem(string title, NSImage icon, string tag, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, tag, clicked));
        }

        public void Insert(int n, SourceListItem item) {
            _items.Insert (n, item);
        }

        public void RemoveItem(SourceListItem item) {
            _items.Remove (item);
        }

        public void RemoveItem(int n) {
            _items.RemoveAt (n);
        }

        public void Clear() {
            _items.Clear ();
        }
        #endregion

        #region Events
        public delegate void ClickedDelegate();
        public event ClickedDelegate Clicked;

        internal void RaiseClickedEvent() {
            // Inform caller
            if (this.Clicked != null)
                this.Clicked ();
        }
        #endregion
    }
}
```

W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `SourceListDataSource` dla **nazwa** i kliknij przycisk **New** przycisku. Wprowadź `SourceListDataSource.cs` wygląd pliku, jak pokazano poniżej:

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDataSource : NSOutlineViewDataSource
    {
        #region Private Variables
        private SourceListView _controller;
        #endregion

        #region Public Variables
        public List<SourceListItem> Items = new List<SourceListItem>();
        #endregion

        #region Constructors
        public SourceListDataSource (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Properties
        public override nint GetChildrenCount (NSOutlineView outlineView, Foundation.NSObject item)
        {
            if (item == null) {
                return Items.Count;
            } else {
                return ((SourceListItem)item).Count;
            }
        }

        public override bool ItemExpandable (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, Foundation.NSObject item)
        {
            if (item == null) {
                return Items [(int)childIndex];
            } else {
                return ((SourceListItem)item) [(int)childIndex]; 
            }
        }

        public override NSObject GetObjectValue (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            return new NSString (((SourceListItem)item).Title);
        }
        #endregion

        #region Internal Methods
        internal SourceListItem ItemForRow(int row) {
            int index = 0;

            // Look at each group
            foreach (SourceListItem item in Items) {
                // Is the row inside this group?
                if (row >= index && row <= (index + item.Count)) {
                    return item [row - index - 1];
                }

                // Move index
                index += item.Count + 1;
            }

            // Not found 
            return null;
        }
        #endregion
    }
}
```

Zapewni to dane w naszej listy źródłowej.

W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `SourceListDelegate` dla **nazwa** i kliknij przycisk **New** przycisku. Wprowadź `SourceListDelegate.cs` wygląd pliku, jak pokazano poniżej:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDelegate : NSOutlineViewDelegate
    {
        #region Private variables
        private SourceListView _controller;
        #endregion

        #region Constructors
        public SourceListDelegate (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Methods
        public override bool ShouldEditTableColumn (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            return false;
        }

        public override NSCell GetCell (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            nint row = outlineView.RowForItem (item);
            return tableColumn.DataCellForRow (row);
        }

        public override bool IsGroupItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            NSTableCellView view = null;

            // Is this a group item?
            if (((SourceListItem)item).HasChildren) {
                view = (NSTableCellView)outlineView.MakeView ("HeaderCell", this);
            } else {
                view = (NSTableCellView)outlineView.MakeView ("DataCell", this);
                view.ImageView.Image = ((SourceListItem)item).Icon;
            }

            // Initialize view
            view.TextField.StringValue = ((SourceListItem)item).Title;

            // Return new view
            return view;
        }

        public override bool ShouldSelectItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return (outlineView.GetParent (item) != null);
        }

        public override void SelectionDidChange (NSNotification notification)
        {
            NSIndexSet selectedIndexes = _controller.SelectedRows;

            // More than one item selected?
            if (selectedIndexes.Count > 1) {
                // Not handling this case
            } else {
                // Grab the item
                var item = _controller.Data.ItemForRow ((int)selectedIndexes.FirstIndex);

                // Was an item found?
                if (item != null) {
                    // Fire the clicked event for the item
                    item.RaiseClickedEvent ();

                    // Inform caller of selection
                    _controller.RaiseItemSelected (item);
                }
            }
        }
        #endregion
    }
}
```

Zapewni to zachowanie naszej listy źródłowej.

Na koniec w **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy plik...** Wybierz **ogólne** > **pustą klasę**, wprowadź `SourceListView` dla **nazwa** i kliknij przycisk **New** przycisku. Wprowadź `SourceListView.cs` wygląd pliku, jak pokazano poniżej:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }
        
        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

Spowoduje to utworzenie niestandardowych, wielokrotnego użytku podklasa `NSOutlineView` (`SourceListView`), firma Microsoft można użyć do listy źródeł na dysku w dowolnej aplikacji platformy Xamarin.Mac, które firma Microsoft.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Utworzenie i utrzymywanie listy źródeł w środowisku Xcode

Teraz możemy projektowanie listy źródłowej w programu Interface Builder. Kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w programu Interface Builder i przeciągnij widoku złożonego z **Inspektor biblioteki**, dodaj go do kontrolera widoku i ustaw dla niej zmiany rozmiaru w widoku w **ograniczenia edytora** :

[![](source-list-images/source00.png "Edytowanie ograniczenia")](source-list-images/source00.png#lightbox)

Następnie przeciągnij listy źródeł, z **Inspektor biblioteki**, dodaj go do lewej strony widoku złożonego i ustaw dla niej zmiany rozmiaru w widoku w **Edytor ograniczenia**:

[![](source-list-images/source02.png "Edytowanie ograniczenia")](source-list-images/source02.png#lightbox)

Następnie przełącz się do **widoku tożsamości**, wybierz z listy źródeł i zmień ją na **klasy** do `SourceListView`:

[![](source-list-images/source03.png "Ustawianie nazwy klasy")](source-list-images/source03.png#lightbox)

Na koniec Utwórz **ujścia** dla naszej listy źródła o nazwie `SourceList` w `ViewController.h` pliku:

[![](source-list-images/source04.png "Konfigurowanie źródła")](source-list-images/source04.png#lightbox)

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>Podczas wypełniania listy źródłowej

Umożliwia edytowanie `RotationWindow.cs` pliku w programie Visual Studio dla komputerów Mac i przypisz ją firmy `AwakeFromNib` wygląd metoda podobne do następującego:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Populate source list
    SourceList.Initialize ();

    var library = new SourceListItem ("Library");
    library.AddItem ("Venues", "house.png", () => {
        Console.WriteLine("Venue Selected");
    });
    library.AddItem ("Singers", "group.png");
    library.AddItem ("Genre", "cards.png");
    library.AddItem ("Publishers", "box.png");
    library.AddItem ("Artist", "person.png");
    library.AddItem ("Music", "album.png");
    SourceList.AddItem (library);

    // Add Rotation 
    var rotation = new SourceListItem ("Rotation"); 
    rotation.AddItem ("View Rotation", "redo.png");
    SourceList.AddItem (rotation);

    // Add Kiosks
    var kiosks = new SourceListItem ("Kiosks");
    kiosks.AddItem ("Sign-in Station 1", "imac");
    kiosks.AddItem ("Sign-in Station 2", "ipad");
    SourceList.AddItem (kiosks);

    // Display side list
    SourceList.ReloadData ();
    SourceList.ExpandItem (null, true);

}
```

`Initialize ()` Metody muszą zostać wywołane z naszej listy źródłowej **ujścia** _przed_ wszystkie elementy są dodawane do niego. Dla każdej grupy elementów możemy utworzyć element nadrzędny, a następnie dodaj elementy podrzędne do tego elementu grupy. Każda grupa jest dodawane do listy źródeł kolekcji `SourceList.AddItem (...)`. Ostatnie dwa wiersze Załaduj dane do listy źródeł i rozwija wszystkie grupy:

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

Wreszcie edytować `AppDelegate.cs` pliku i upewnij `DidFinishLaunching` wygląd metoda podobne do następującego:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

Jeśli możemy uruchomić aplikację, że zostanie wyświetlony:

[![](source-list-images/source05.png "Uruchamianie przykładowej aplikacji")](source-list-images/source05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z listami źródła w aplikacji platformy Xamarin.Mac. Widzieliśmy, jak tworzyć i obsługiwać, źródła list w program Xcode Interface Builder i sposób pracy z listami źródła w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacOutlines (przykład)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Widoki konspektu](~/mac/user-interface/outline-view.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do konturu widoków](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
