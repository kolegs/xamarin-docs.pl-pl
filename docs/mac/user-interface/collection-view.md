---
title: Widoki kolekcji dla platformy Xamarin.Mac
description: W tym artykule opisano pracy za pomocą widoków kolekcji w aplikacji platformy Xamarin.Mac. Obejmuje on tworzenia i utrzymywania widoki kolekcji w programie Xcode i programu Interface Builder oraz pracy z nimi programistycznie.
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/24/2017
ms.openlocfilehash: c8b4b5ff8bebf5fbbded410ae84d1aefcca2d6cc
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780610"
---
# <a name="collection-views-in-xamarinmac"></a>Widoki kolekcji dla platformy Xamarin.Mac

_W tym artykule opisano pracy za pomocą widoków kolekcji w aplikacji platformy Xamarin.Mac. Obejmuje on tworzenia i utrzymywania widoki kolekcji w programie Xcode i programu Interface Builder oraz pracy z nimi programistycznie._

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, deweloper ma dostęp do tej samej strukturze AppKit widok kolekcji formantów, które osoba używająca *języka Objective-C* i *Xcode* jest. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, programistka używa program Xcode _programu Interface Builder_ do tworzenia i obsługi widoki kolekcji.

A `NSCollectionView` Wyświetla siatkę organizować przy użyciu widoków podrzędnych `NSCollectionViewLayout`. Każdy widok podrzędny w siatce jest reprezentowany przez `NSCollectionViewItem` która zarządza ładowanie zawartości widoku z `.xib` pliku.

[![Uruchamianie przykładowej aplikacji](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

W tym artykule opisano podstawy pracy z widokami kolekcja w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które są używane w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` poleceń Umożliwia o komunikacji sieciowej w górę klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>Widoki kolekcji — informacje

Głównym celem widok kolekcji (`NSCollectionView`) jest wizualnie rozmieścić grupy obiektów w sposób zorganizowany widok w układzie kolekcji (`NSCollectionViewLayout`), za pomocą poszczególnych obiektów (`NSCollectionViewItem`) pobieranie własnego widoku w większej kolekcji. Widoki kolekcji pracy za pomocą technik wiązania danych i kodowanie klucz-wartość i jako takie, należy przeczytać [powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md) dokumentacji, przed kontynuowaniem w tym artykule.

Widok kolekcji ma nie standardowego, wbudowanego widoku elementu kolekcji (np. konspekt lub widok tabeli nie), dzięki czemu deweloper jest odpowiedzialny za projektowanie i implementowanie _widoku prototypu_ przy użyciu innych kontrolek AppKit, takie jak pola obrazu , Pól tekstowych, etykiety, itp. Ten widok prototypu będzie służyć do wyświetlania i pracować każdy element zarządzany przez widok kolekcji i są przechowywane w `.xib` pliku.

Ponieważ deweloper jest odpowiedzialny za wygląd i działanie elementu widoku kolekcji, widok kolekcji nie ma wbudowanej obsługi wyróżnianie wybranego elementu w siatce. Implementowanie ta funkcja zostanie omówione w tym artykule.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>Zdefiniowanie modelu danych

Przed powiązania danych Widok kolekcji w programu Interface Builder pary klucz-wartość kodowania (KVC) / klasy zgodnej obserwowania pary klucz-wartość (KVO) musi być zdefiniowany w aplikacji platformy Xamarin.Mac, aby pełnić rolę _modelu danych_ dla wiązania. Model danych zawiera wszystkie dane, która będzie wyświetlana w kolekcji i odbiera wszelkie modyfikacje danych, który użytkownik dokona w interfejsie użytkownika podczas uruchamiania aplikacji.

Przykładowy aplikację, która zarządza grupy pracowników, następującej klasy może służyć do definiowania modelu danych:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon
        {
            get
            {
                if (isManager)
                {
                    return NSImage.ImageNamed("IconGroup");
                }
                else
                {
                    return NSImage.ImageNamed("IconUser");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

`PersonModel` Modelu danych, które będą używane w dalszej części tego artykułu.

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>Praca z widokiem kolekcji

Powiązanie danych z widokiem kolekcji jest bardzo podobny wiązania za pomocą widoku tabeli, jak `NSCollectionViewDataSource` jest używana do dostarczania danych dla kolekcji. Ponieważ widok kolekcji nie ma format wyświetlania wstępnie zdefiniowane, więcej pracy jest wymagane, aby przekazać opinię interakcji użytkownika oraz śledzenie wybór użytkownika.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>Tworzenie prototypów komórki

Ponieważ widok kolekcji nie ma prototypu komórki domyślne, deweloper należy dodać co najmniej jedno `.xib` plików do aplikacji platformy Xamarin.Mac, aby zdefiniować układ i zawartość poszczególnych komórek.

Wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Dodaj** > **nowy plik...**
2. Wybierz **Mac** > **kontrolera widoku**, nadaj jej nazwę (taką jak `EmployeeItem` w tym przykładzie) i kliknij przycisk **New** przycisk, aby utworzyć: 

    ![Dodawanie nowego kontrolera widoku](collection-view-images/proto01.png)

    Spowoduje to dodanie `EmployeeItem.cs`, `EmployeeItemController.cs` i `EmployeeItemController.xib` plik do projektu rozwiązania.
3. Kliknij dwukrotnie `EmployeeItemController.xib` plik, aby go otworzyć do edycji w program Xcode Interface Builder.
4. Dodaj `NSBox`, `NSImageView` oraz dwóch `NSLabel` formantów do widoku i zaprojektuj w następujący sposób:

    ![Projektowanie układu prototypu komórki](collection-view-images/proto02.png)
5. Otwórz **edytora Asystenta** i utworzyć **ujścia** dla `NSBox` , dzięki czemu może służyć do wskazywania stanu zaznaczenia komórki:

    ![Udostępnianie NSBox w gniazda.](collection-view-images/proto03.png)
6. Wróć do **Edytor standardowy** i wybierz widok obrazu.
7. W **powiązanie Inspektor**, wybierz opcję **powiązania do** > **właściciel pliku** i wprowadź **ścieżka klucza modelu** z `self.Person.Icon`:

    ![Powiązanie ikony](collection-view-images/proto04.png)
8. Wybierz pierwszą etykietę i w **powiązanie Inspektor**, wybierz opcję **powiązania do** > **właściciel pliku** i wprowadź **ścieżka klucza modelu**z `self.Person.Name`:

    ![Nazwa powiązania](collection-view-images/proto05.png)
9. Wybierz drugą etykietę i w **powiązanie Inspektor**, wybierz opcję **powiązania do** > **właściciel pliku** i wprowadź **ścieżka klucza modelu**z `self.Person.Occupation`:

    ![Powiązanie zawodu](collection-view-images/proto06.png)
10. Czy zapisać zmiany `.xib` pliku, a następnie wróć do programu Visual Studio, aby zsynchronizować zmiany.

Edytuj `EmployeeItemController.cs` plików i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// The Employee item controller handles the display of the individual items that will
    /// be displayed in the collection view as defined in the associated .XIB file.
    /// </summary>
    public partial class EmployeeItemController : NSCollectionViewItem
    {
        #region Private Variables
        /// <summary>
        /// The person that will be displayed.
        /// </summary>
        private PersonModel _person;
        #endregion

        #region Computed Properties
        // strongly typed view accessor
        public new EmployeeItem View
        {
            get
            {
                return (EmployeeItem)base.View;
            }
        }

        /// <summary>
        /// Gets or sets the person.
        /// </summary>
        /// <value>The person that this item belongs to.</value>
        [Export("Person")]
        public PersonModel Person
        {
            get { return _person; }
            set
            {
                WillChangeValue("Person");
                _person = value;
                DidChangeValue("Person");
            }
        }

        /// <summary>
        /// Gets or sets the color of the background for the item.
        /// </summary>
        /// <value>The color of the background.</value>
        public NSColor BackgroundColor {
            get { return Background.FillColor; }
            set { Background.FillColor = value; }
        }

        /// <summary>
        /// Gets or sets a value indicating whether this <see cref="T:MacCollectionNew.EmployeeItemController"/> is selected.
        /// </summary>
        /// <value><c>true</c> if selected; otherwise, <c>false</c>.</value>
        /// <remarks>This also changes the background color based on the selected state
        /// of the item.</remarks>
        public override bool Selected
        {
            get
            {
                return base.Selected;
            }
            set
            {
                base.Selected = value;

                // Set background color based on the selection state
                if (value) {
                    BackgroundColor = NSColor.DarkGray;
                } else {
                    BackgroundColor = NSColor.LightGray;
                }
            }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public EmployeeItemController(IntPtr handle) : base(handle)
        {
            Initialize();
        }

        // Called when created directly from a XIB file
        [Export("initWithCoder:")]
        public EmployeeItemController(NSCoder coder) : base(coder)
        {
            Initialize();
        }

        // Call to load from the XIB/NIB file
        public EmployeeItemController() : base("EmployeeItem", NSBundle.MainBundle)
        {
            Initialize();
        }

        // Added to support loading from XIB/NIB
        public EmployeeItemController(string nibName, NSBundle nibBundle) : base(nibName, nibBundle) {

            Initialize();
        }

        // Shared initialization code
        void Initialize()
        {
        }
        #endregion
    }
}
```

Patrząc ten kod szczegółowo, klasa dziedziczy po `NSCollectionViewItem` , dzięki czemu może działać jako prototyp dla komórki widoku kolekcji. `Person` Właściwość udostępnia klasy, który został użyty do tworzenia powiązań danych Wyświetlanie obrazów i etykiety w środowisku Xcode. To wystąpienie `PersonModel` utworzonego powyżej.

`BackgroundColor` Właściwość to skrót umożliwiający `NSBox` kontrolki `FillColor` który będzie używany do wyświetlania stanu zaznaczenia komórki. Przez zastąpienie `Selected` właściwość `NSCollectionViewItem`, poniższy kod ustawia lub czyści ten stan zaznaczenia:

```csharp
public override bool Selected
{
    get
    {
        return base.Selected;
    }
    set
    {
        base.Selected = value;

        // Set background color based on the selection state
        if (value) {
            BackgroundColor = NSColor.DarkGray;
        } else {
            BackgroundColor = NSColor.LightGray;
        }
    }
}
```

<a name="Creating-the-Collection-View-Data-Source"/>

### <a name="creating-the-collection-view-data-source"></a>Tworzenie źródła danych widoku kolekcji

Źródło danych widoku kolekcji (`NSCollectionViewDataSource`) zawiera wszystkie dane dla widoku kolekcji i utworzenie i wypełnienie komórki widoku kolekcji (przy użyciu `.xib` prototypu) zgodnie z wymaganiami dla każdego elementu w kolekcji.

Dodaj nową klasę projekt, wywołaj ją `CollectionViewDataSource` i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view data source provides the data for the collection view.
    /// </summary>
    public class CollectionViewDataSource : NSCollectionViewDataSource
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent collection view.
        /// </summary>
        /// <value>The parent collection view.</value>
        public NSCollectionView ParentCollectionView { get; set; }

        /// <summary>
        /// Gets or sets the data that will be displayed in the collection.
        /// </summary>
        /// <value>A collection of PersonModel objects.</value>
        public List<PersonModel> Data { get; set; } = new List<PersonModel>();
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDataSource"/> class.
        /// </summary>
        /// <param name="parent">The parent collection that this datasource will provide data for.</param>
        public CollectionViewDataSource(NSCollectionView parent)
        {
            // Initialize
            ParentCollectionView = parent;

            // Attach to collection view
            parent.DataSource = this;

        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Gets the number of sections.
        /// </summary>
        /// <returns>The number of sections.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        public override nint GetNumberOfSections(NSCollectionView collectionView)
        {
            // There is only one section in this view
            return 1;
        }

        /// <summary>
        /// Gets the number of items in the given section.
        /// </summary>
        /// <returns>The number of items.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="section">The Section number to count items for.</param>
        public override nint GetNumberofItems(NSCollectionView collectionView, nint section)
        {
            // Return the number of items
            return Data.Count;
        }

        /// <summary>
        /// Gets the item for the give section and item index.
        /// </summary>
        /// <returns>The item.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPath">Index path specifying the section and index.</param>
        public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
        {
            var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
            item.Person = Data[(int)indexPath.Item];

            return item;
        }
        #endregion
    }
}
```

Patrząc ten kod szczegółowo, klasa dziedziczy po `NSCollectionViewDataSource` i udostępnia listę `PersonModel` wystąpień za pośrednictwem jego `Data` właściwości.

Ponieważ ta kolekcja ma tylko jedną sekcję, ten kod zastępuje `GetNumberOfSections` metody i zawsze zwraca `1`. Ponadto `GetNumberofItems` metoda zostanie przesłonięta w zwraca liczbę elementów w `Data` listy właściwości.

`GetItem` Metoda jest wywoływana za każdym razem nową komórkę jest wymagany i wygląda podobnie do następującej:

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

`MakeItem` Zostanie wywołana metoda widok kolekcji, można utworzyć lub powrócić do ponownego użycia wystąpienia `EmployeeItemController` i jego `Person` właściwość jest ustawiona na element będzie wyświetlany w żądany komórki. 

`EmployeeItemController` Muszą być zarejestrowane przy użyciu kontrolera widoku kolekcji wcześniej przy użyciu następującego kodu:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

**Identyfikator** (`EmployeeCell`) używane w `MakeItem` wywołania _musi_ zgodna z nazwą kontrolera widoku, który został zarejestrowany przy użyciu widoku kolekcji. W tym kroku zostaną omówione szczegółowo poniżej.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>Wybór elementu obsługi

Do obsługi wybór i deselection elementów w kolekcji, `NSCollectionViewDelegate` będą wymagane. Ponieważ w tym przykładzie będzie można za pomocą wbudowanego `NSCollectionViewFlowLayout` typ układu `NSCollectionViewDelegateFlowLayout` określoną wersję ten delegat będzie wymagane.

Dodaj nową klasę do projektu, wywołaj ją `CollectionViewDelegate` i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view delegate handles user interaction with the elements of the 
    /// collection view for the Flow-Based layout type.
    /// </summary>
    public class CollectionViewDelegate : NSCollectionViewDelegateFlowLayout
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent view controller.
        /// </summary>
        /// <value>The parent view controller.</value>
        public ViewController ParentViewController { get; set; }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDelegate"/> class.
        /// </summary>
        /// <param name="parentViewController">Parent view controller.</param>
        public CollectionViewDelegate(ViewController parentViewController)
        {
            // Initialize
            ParentViewController = parentViewController;
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Handles one or more items being selected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being selected.</param>
        public override void ItemsSelected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = (int)paths[0].Item;

            // Save the selected item
            ParentViewController.PersonSelected = ParentViewController.Datasource.Data[index];

        }

        /// <summary>
        /// Handles one or more items being deselected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being deselected.</param>
        public override void ItemsDeselected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = paths[0].Item;

            // Clear selection
            ParentViewController.PersonSelected = null;
        }
        #endregion
    }
}
``` 

`ItemsSelected` i `ItemsDeselected` metody są zastąpione i używany do ustawiania i czyszczenia `PersonSelected` właściwości kontrolera widoku, który jest obsługa widok kolekcji, gdy użytkownik zaznacza lub usuwa ich zaznaczenie elementu. Będzie ona widoczna poniżej.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Tworzenie widoku kolekcji w programu Interface Builder

Wszystkie wymagane elementy pomocnicze w miejscu mogą być edytowane głównego storyboard i widokiem kolekcji dodawanych do niego.

Wykonaj następujące czynności:

1. Kliknij dwukrotnie `Main.Storyboard` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji w programie Xcode zachowania programu Interface Builder.
2. Przeciągnij widok kolekcji do widoku głównego i zmień rozmiar, tak aby wypełniała widoku:

    ![Dodawanie widoku kolekcji do układu](collection-view-images/collection01.png)
3. Wybrany widok kolekcji za pomocą edytora ograniczenie Aby przypiąć go do widoku podczas zmiany jego rozmiaru:

    ![Dodawanie ograniczenia](collection-view-images/collection02.png)
4. Upewnij się, że w jest wybrany widok kolekcji **powierzchni projektowej** (i nie **obramowaniem widoku przewijania** lub **widok klipu** zawierający go), przejdź do  **Edytor Asystenta** i utworzyć **ujścia** widoku kolekcji:

    ![Dodawanie ograniczenia](collection-view-images/collection03.png)
5. Zapisz zmiany i powrócić do programu Visual Studio do synchronizacji.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>W jednym razem wszystkie

Wszystkie elementy pomocnicze teraz została wprowadzona w miejscu z klasą jako model danych (`PersonModel`), `NSCollectionViewDataSource` została dodana do dostarczania danych, `NSCollectionViewDelegateFlowLayout` został utworzony w celu obsługi zaznaczenie elementu i `NSCollectionView` została dodana do Main Storyboard i udostępniane jako sposobu (`EmployeeCollection`).

Ostatnim krokiem jest Edytuj kontrolera widoku, który zawiera widok kolekcji i przenieść wszystkie elementy są ze sobą, aby wypełnić kolekcji i obsługiwać zaznaczenie elementu.

Edytuj `ViewController.cs` plików i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using AppKit;
using Foundation;
using CoreGraphics;

namespace MacCollectionNew
{
    /// <summary>
    /// The View controller controls the main view that houses the Collection View.
    /// </summary>
    public partial class ViewController : NSViewController
    {
        #region Private Variables
        private PersonModel _personSelected;
        private bool shouldEdit = true;
        #endregion

        #region Computed Properties
        /// <summary>
        /// Gets or sets the datasource that provides the data to display in the 
        /// Collection View.
        /// </summary>
        /// <value>The datasource.</value>
        public CollectionViewDataSource Datasource { get; set; }

        /// <summary>
        /// Gets or sets the person currently selected in the collection view.
        /// </summary>
        /// <value>The person selected or <c>null</c> if no person is selected.</value>
        [Export("PersonSelected")]
        public PersonModel PersonSelected
        {
            get { return _personSelected; }
            set
            {
                WillChangeValue("PersonSelected");
                _personSelected = value;
                DidChangeValue("PersonSelected");
                RaiseSelectionChanged();
            }
        }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.ViewController"/> class.
        /// </summary>
        /// <param name="handle">Handle.</param>
        public ViewController(IntPtr handle) : base(handle)
        {
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Called after the view has finished loading from the Storyboard to allow it to
        /// be configured before displaying to the user.
        /// </summary>
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();

            // Initialize Collection View
            ConfigureCollectionView();
            PopulateWithData();
        }
        #endregion

        #region Private Methods
        /// <summary>
        /// Configures the collection view.
        /// </summary>
        private void ConfigureCollectionView()
        {
            EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");

            // Create a flow layout
            var flowLayout = new NSCollectionViewFlowLayout()
            {
                ItemSize = new CGSize(150, 150),
                SectionInset = new NSEdgeInsets(10, 10, 10, 20),
                MinimumInteritemSpacing = 10,
                MinimumLineSpacing = 10
            };
            EmployeeCollection.WantsLayer = true;

            // Setup collection view
            EmployeeCollection.CollectionViewLayout = flowLayout;
            EmployeeCollection.Delegate = new CollectionViewDelegate(this);

        }

        /// <summary>
        /// Populates the Datasource with data and attaches it to the collection view.
        /// </summary>
        private void PopulateWithData()
        {
            // Make datasource
            Datasource = new CollectionViewDataSource(EmployeeCollection);

            // Build list of employees
            Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
            Datasource.Data.Add(new PersonModel("Amy Burns", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Joel Martinez", "Web & Infrastructure"));
            Datasource.Data.Add(new PersonModel("Kevin Mullins", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Mark McLemore", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Tom Opgenorth", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Larry O'Brien", "API Docs Manager", true));
            Datasource.Data.Add(new PersonModel("Mike Norman", "API Documentor"));

            // Populate collection view
            EmployeeCollection.ReloadData();
        }
        #endregion

        #region Events
        /// <summary>
        /// Selection changed delegate.
        /// </summary>
        public delegate void SelectionChangedDelegate();

        /// <summary>
        /// Occurs when selection changed.
        /// </summary>
        public event SelectionChangedDelegate SelectionChanged;

        /// <summary>
        /// Raises the selection changed event.
        /// </summary>
        internal void RaiseSelectionChanged() {
            // Inform caller
            if (this.SelectionChanged != null) SelectionChanged();
        }
        #endregion
    }
}
```

Biorąc przyjrzeć się ten kod szczegółowo `Datasource` właściwość jest zdefiniowana na potrzeby przechowywania wystąpienie `CollectionViewDataSource` , będzie dostarczać dane dla widoku kolekcji. A `PersonSelected` właściwość jest zdefiniowana na potrzeby przechowywania `PersonModel` reprezentujący aktualnie wybranego elementu w widoku kolekcji. Ta właściwość jest także zgłasza `SelectionChanged` zdarzenie, gdy zmieni się zaznaczenie.

`ConfigureCollectionView` Klasy jest używane do rejestrowania kontrolera widoku, który działa jako prototyp komórki z widokiem kolekcji przy użyciu następującego wiersza:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

Należy zauważyć, że **identyfikator** (`EmployeeCell`) używane do rejestrowania dopasowania prototypu o nazwach w `GetItem` metody `CollectionViewDataSource` zdefiniowanych powyżej:

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

Ponadto typ kontrolera widoku **musi** zgodna z nazwą `.xib` pliku, który definiuje prototypu **dokładnie**. W tym przykładzie `EmployeeItemController` i `EmployeeItemController.xib`.

Układ elementów w widoku kolekcji jest kontrolowany przez klasę kolekcji widok układu i można zmienić dynamicznie w czasie wykonywania przez przypisanie nowego wystąpienia `CollectionViewLayout` właściwości. Zmienianie tej właściwości aktualizacji wygląd widok kolekcji bez animowanie zmian.

Apple jest dostarczany dwa typy wbudowane układ z widokiem kolekcji, która będzie obsługiwać najbardziej typowych zastosowań: `NSCollectionViewFlowLayout` i `NSCollectionViewGridLayout`. Jeśli Deweloper wymaganego formatu niestandardowego, takich jak rozmieszczania elementów w okręgu, mogą utworzyć niestandardowego wystąpienia `NSCollectionViewLayout` i zastąpienia metody wymagane do osiągnięcia pożądanych efektów.

W tym przykładzie użyto domyślnego układu przepływu, dlatego powoduje utworzenie wystąpienia `NSCollectionViewFlowLayout` klasy i konfiguruje go w następujący sposób:

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

`ItemSize` Właściwość definiuje rozmiar każdego pojedyncze komórki w kolekcji. `SectionInset` Właściwość definiuje marginesy od krawędzi komórki zostaną wyświetlone w kolekcji. `MinimumInteritemSpacing` Określa minimalny odstęp między elementami i `MinimumLineSpacing` definiuje minimalny odstęp między wierszami w kolekcji.

Układ jest przypisany do przeglądania kolekcji i wystąpienie `CollectionViewDelegate` jest dołączony do obsługi Wybór elementu:

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

`PopulateWithData` Metoda tworzy nowe wystąpienie klasy `CollectionViewDataSource`, wypełnia ją z danymi, dołącza go do widoku kolekcji i wywołania `ReloadData` metodę, aby wyświetlić elementy:

```csharp
private void PopulateWithData()
{
    // Make datasource
    Datasource = new CollectionViewDataSource(EmployeeCollection);

    // Build list of employees
    Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
    ...

    // Populate collection view
    EmployeeCollection.ReloadData();
}
```

`ViewDidLoad` Metoda zostanie przesłonięta a wywołuje `ConfigureCollectionView` i `PopulateWithData` metody służące do wyświetlania końcowego widoku kolekcji dla użytkownika:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    // Initialize Collection View
    ConfigureCollectionView();
    PopulateWithData();
}
```

<a name="Summary"/>

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z widoki kolekcji w aplikacji platformy Xamarin.Mac. Po pierwsze znaleziono w znajdowaniu klasy C# do języka Objective-C przy użyciu kodowania pary klucz-wartość (KVC) i obserwowania pary klucz-wartość (KVO). Następnie pokazano, jak używać klasy zgodne KVO i powiązać go z widokami kolekcja w program Xcode Interface Builder danych. Na koniec pokazano sposób interakcji z widokami kolekcja w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacCollectionNew (przykład)](https://developer.xamarin.com/samples/mac/MacCollectionNew/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
