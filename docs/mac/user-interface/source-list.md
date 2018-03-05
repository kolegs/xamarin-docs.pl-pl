---
title: "Źródło listy"
description: "Ten artykuł dotyczy pracy z listy źródła w aplikacji Xamarin.Mac. Opisuje tworzenie i utrzymywanie list źródła w środowisku Xcode i konstruktora interfejsu i interakcji z nimi w kodzie języka C#."
ms.topic: article
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 1cc74fb30e59ecd5f6be3cf3e1c84f60cd5ca0a6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="source-lists"></a>Źródło listy

_Ten artykuł dotyczy pracy z listy źródła w aplikacji Xamarin.Mac. Opisuje tworzenie i utrzymywanie list źródła w środowisku Xcode i konstruktora interfejsu i interakcji z nimi w kodzie języka C#._

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tego samego źródła listy, które osoba używająca *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć w środowisku Xcode _konstruktora interfejsu_ Aby utworzyć i zachować swoje listy źródłowej (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Źródło listy jest specjalny typ widoku konspektu używane do wyświetlania źródła akcji, takich jak paska bocznego w iTunes lub wyszukiwania.

[ ![](source-list-images/source05.png "Przykład listy źródłowej")](source-list-images/source05.png)

W tym artykule omówione zostaną następujące czynności podstawowe informacje o pracy z listami źródła w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` poleceń używany do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>Wprowadzenie do listy źródłowej

Jak już wspomniano, lista źródła jest specjalny typ widoku konspektu używany do wyświetlenia źródła akcji, takich jak paska bocznego w iTunes lub wyszukiwania. Lista źródeł jest typem tabeli, która pozwala użytkownikowi rozwiń lub Zwiń wiersze danych hierarchicznej. W przeciwieństwie do widoku tabeli elementów na liście źródło nie znajdują się w płaskiej listy, są zorganizowane w hierarchii, takie jak pliki i foldery na dysku twardym. Jeśli element na liście źródło zawiera inne elementy, można rozwinięte lub zwinięte przez użytkownika.

Lista źródła jest specjalnie styl widoku konspektu (`NSOutlineView`), które jest podklasą widoku tabeli (`NSTableView`) i w efekcie dziedziczy znacznie jego działania od swojej klasy nadrzędnej. W związku z tym wiele operacji obsługiwane przez wyświetlanie konspektu, również są obsługiwane przez listy źródłowej. Aplikacja Xamarin.Mac ma kontrolę nad tych funkcji i parametry listy źródeł, (albo w kodzie lub konstruktora interfejsu) można skonfigurować do Zezwalaj lub nie zezwalaj pewnych operacji.

Lista źródeł nie przechowuje swoich danych, zamiast tego jest zależne od źródła danych (`NSOutlineViewDataSource`) wiersze i kolumny są wymagane na zgodnie z potrzebami.

Zachowanie listę źródła można dostosować, podając podklasą delegata widoku konspektu (`NSOutlineViewDelegate`) na potrzeby obsługi typu konspektu, aby wybrać funkcje, element wybór i edytowania, niestandardowe śledzenia i widoków niestandardowych dla poszczególnych elementów.

Ponieważ listy źródeł udostępnia znacznie jego zachowania i funkcji z widoku tabeli i widoku konspektu, należy przejść przez naszych [widoków tabel](~/mac/user-interface/table-view.md) i [widoków konspektu](~/mac/user-interface/outline-view.md) dokumentacji, aby móc kontynuować z tego artykułu.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>Praca z listy źródłowej

Źródło listy jest specjalny typ widoku konspektu używane do wyświetlania źródła akcji, takich jak paska bocznego w iTunes lub wyszukiwania. W przeciwieństwie do widoków konspektu przed definiujemy naszej listy źródła w Konstruktorze interfejsu Utwórzmy klasy zapasowy w Xamarin.Mac.

Najpierw utwórz nową `SourceListItem` klasy do przechowywania danych dla naszej listy źródłowej. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `SourceListItem` dla **nazwa** i kliknij przycisk **nowy** przycisk:

[ ![](source-list-images/source01.png "Dodawanie klasy pusty")](source-list-images/source01.png)

Wprowadź `SourceListItem.cs` wygląd pliku podobne do poniższych: 

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

W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `SourceListDataSource` dla **nazwa** i kliknij przycisk **nowy** przycisku. Wprowadź `SourceListDataSource.cs` wygląd pliku podobne do poniższych:

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

To będzie dostarczać dane dla naszej listy źródłowej.

W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `SourceListDelegate` dla **nazwa** i kliknij przycisk **nowy** przycisku. Wprowadź `SourceListDelegate.cs` wygląd pliku podobne do poniższych:

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

Ponadto w **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowego pliku...** Wybierz **ogólne** > **pustą klasę**, wprowadź `SourceListView` dla **nazwa** i kliknij przycisk **nowy** przycisku. Wprowadź `SourceListView.cs` wygląd pliku podobne do poniższych:

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

Spowoduje to utworzenie niestandardowego, wielokrotnego użytku podklasą klasy `NSOutlineView` (`SourceListView`) można używana do kierowania listy źródeł w dowolnej aplikacji Xamarin.Mac, że firma Microsoft.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Tworzenie i obsługę list źródła w środowisku Xcode

Teraz Przyjrzyjmy projektowania naszej listy źródła w Konstruktorze interfejsu. Kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu i przeciągnij podzielony widok z **inspektora biblioteki**, dodaj go do kontrolera widoku i ustaw dla niej zmiany rozmiaru w widoku w **Edytor ograniczenia** :

[ ![](source-list-images/source00.png "Edytowanie ograniczenia")](source-list-images/source00.png)

Następnie przeciągnij listę źródła z **inspektora biblioteki**, dodaj go do lewej strony podzielony widok oraz ustaw go zmiany rozmiaru w widoku w **Edytor ograniczenia**:

[ ![](source-list-images/source02.png "Edytowanie ograniczenia")](source-list-images/source02.png)

Następnie przełącz się do **widoku tożsamości**, wybierz z listy źródeł i zmień ją na **klasy** do `SourceListView`:

[ ![](source-list-images/source03.png "Ustawienie nazwy klasy")](source-list-images/source03.png)

Na koniec Utwórz **gniazda** wywoływanych z naszej listy źródłowej `SourceList` w `ViewController.h` pliku:

[ ![](source-list-images/source04.png "Konfigurowanie gniazda")](source-list-images/source04.png)

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>Wypełnianie listy źródłowej

Umożliwia edytowanie `RotationWindow.cs` pliku w Visual Studio dla komputerów Mac i przekształcenie go w `AwakeFromNib` wygląd metody podobne do poniższych:

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

`Initialize ()` Metoda musi być wywoływany z naszej listy źródłowej **gniazda** _przed_ wszystkie elementy są dodawane do niego. Dla każdej grupy elementów możemy utworzyć element nadrzędny, a następnie dodaj elementów podrzędnych do elementu grupy. Każda grupa jest dodawane do listy źródłowej kolekcji `SourceList.AddItem (...)`. Ostatnie dwa wiersze ładowania dla listy źródeł danych i rozwija wszystkie grupy:

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

Na koniec Edytuj `AppDelegate.cs` plików i upewnij `DidFinishLaunching` wygląd metody podobne do poniższych:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

Czy możemy uruchomić aplikację, poniżej zostanie wyświetlony:

[ ![](source-list-images/source05.png "Uruchom przykładową aplikację")](source-list-images/source05.png)

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się praca z listami źródła w aplikacji Xamarin.Mac. Widzieliśmy, jak tworzyć i obsługiwać źródła list w programie Xcode przez interfejs konstruktora i sposobu pracy z listami źródła w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Widoki konspektu](~/mac/user-interface/outline-view.md)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do widoków konspektu](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
