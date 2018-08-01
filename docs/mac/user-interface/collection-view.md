---
title: Kolekcja widoków Xamarin.Mac
description: W tym artykule opisano pracy z widokiem kolekcji w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i obsługa kolekcji widoków w programie Xcode i kompilatora interfejsu oraz pracy z nimi programistycznie.
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/24/2017
ms.openlocfilehash: 8e354b1ce273b10758a7d8c1361055b972839943
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792560"
---
# <a name="collection-views-in-xamarinmac"></a>Kolekcja widoków Xamarin.Mac

_W tym artykule opisano pracy z widokiem kolekcji w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i obsługa kolekcji widoków w programie Xcode i kompilatora interfejsu oraz pracy z nimi programistycznie._

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, deweloper ma dostęp do tej samej AppKit widok kolekcji formantów, które osoba używająca *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, deweloperów używa w środowisku Xcode _konstruktora interfejsu_ można tworzyć i obsługiwać widoki kolekcji.

A `NSCollectionView` wyświetlana siatka zorganizowane przy użyciu widoków podrzędnych `NSCollectionViewLayout`. Każdy widok podrzędny w siatce jest reprezentowana przez `NSCollectionViewItem` który zarządza ładowania zawartości widoku z `.xib` pliku.

[![Uruchom przykładową aplikację](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

W tym artykule opisano podstawy Praca z widokami kolekcja w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które są używane w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` poleceń używany do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>Widoki kolekcji — informacje

Głównym celem widok kolekcji (`NSCollectionView`) jest wizualnie ułożyć grupy obiektów w sposób zorganizowany przy użyciu kolekcji układu widoku (`NSCollectionViewLayout`), z poszczególnych obiektów (`NSCollectionViewItem`) uzyskiwanie własną widoku w większej kolekcji. Kolekcja widoków pracy przy użyciu technik wiązania danych i kluczy i wartości kodowania i, należy przeczytać [powiązania danych i kluczy i wartości kodowania](~/mac/app-fundamentals/databinding.md) dokumentacji, przed kontynuowaniem w tym artykule.

Widok kolekcji ma nie standardowego, wbudowanego widok elementu kolekcji (np. konspektu lub widoku tabeli nie), więc dewelopera jest odpowiedzialny za projektowanie i implementowanie _widoku prototypu_ za pomocą innych mechanizmów kontroli AppKit, np. pola obrazu , Pola tekstu etykiety, itp. Ten widok prototypu będzie używany do wyświetlania i pracować z każdym elementem zarządzany przez widok kolekcji i są przechowywane w `.xib` pliku.

Widok kolekcji, ponieważ dewelopera jest odpowiedzialny za wygląd i działanie elementu widoku kolekcji, ma wbudowaną obsługę wyróżnienie z zaznaczonego elementu w siatce. Implementowanie ta funkcja zostanie omówiona w tym artykule.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>Definiowanie modelu danych

Przed powiązania danych Widok kolekcji w interfejsie konstruktora, kluczy i wartości kodowania (KVC) / klasy zgodne obserwowania klucz-wartość (KVO) musi być zdefiniowana w aplikacji Xamarin.Mac do działania jako _modelu danych_ dla wiązania. Model danych zawiera wszystkie dane, która będzie wyświetlana w kolekcji i odbiera wszelkie modyfikacje dane użytkownika w interfejsie użytkownika podczas uruchamiania aplikacji.

Przykładowy aplikację, która zarządza grupy pracowników, następujące klasy może służyć do definiowania modelu danych:

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

Powiązanie danych z widokiem kolekcji jest bardzo dużo podobnego powiązania z widokiem tabeli jako `NSCollectionViewDataSource` zapewnia dane dla kolekcji. Ponieważ widok kolekcji nie ma formatu wyświetlania predefiniowanych, więcej pracy jest wymagana, aby przekazać opinię interakcji użytkownika oraz śledzenie wybór użytkownika.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>Tworzenie prototypów komórki

Ponieważ widok kolekcji nie ma domyślnego prototyp komórki, deweloper należy dodać co najmniej jeden `.xib` pliki do aplikacji Xamarin.Mac, aby określić układ i zawartość poszczególnych komórek.

Wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Dodaj** > **nowego pliku...**
2. Wybierz **Mac** > **kontrolera widoku**, nadaj mu nazwę (takich jak `EmployeeItem` w tym przykładzie) i kliknij przycisk **nowy** przycisk, aby utworzyć: 

    ![Dodawanie nowego kontrolera widoku](collection-view-images/proto01.png)

    Spowoduje to dodanie `EmployeeItem.cs`, `EmployeeItemController.cs` i `EmployeeItemController.xib` plik do projektu tego rozwiązania.
3. Kliknij dwukrotnie `EmployeeItemController.xib` plik, aby otworzyć do edycji w Konstruktorze interfejsu w środowisku Xcode.
4. Dodaj `NSBox`, `NSImageView` i dwa `NSLabel` formantów do widoku i zaprojektuj w następujący sposób:

    ![Projektowanie układu prototypu komórki](collection-view-images/proto02.png)
5. Otwórz **Edytor Asystenta** i Utwórz **gniazda** dla `NSBox` , dzięki czemu może służyć do wskazywania stanu zaznaczenia komórki:

    ![Udostępnianie NSBox w gniazda.](collection-view-images/proto03.png)
6. Wróć do **standardowego edytora** i wybierz widok obrazu.
7. W **powiązanie inspektora**, wybierz pozycję **powiązać do** > **właścicielem pliku** , a następnie wprowadź **ścieżka klucza modelu** z `self.Person.Icon`:

    ![Powiązanie ikony](collection-view-images/proto04.png)
8. Wybierz pierwsza etykieta w **powiązanie Inspektor**, wybierz pozycję **powiązania do** > **właściciela pliku** , a następnie wprowadź **ścieżka klucza modelu**z `self.Person.Name`:

    ![Nazwa powiązania](collection-view-images/proto05.png)
9. Wybierz etykietę, drugi i w **powiązanie inspektora**, wybierz pozycję **powiązać do** > **właścicielem pliku** , a następnie wprowadź **ścieżka klucza modelu**z `self.Person.Occupation`:

    ![Powiązanie zawód](collection-view-images/proto06.png)
10. Zapisać zmiany w `.xib` pliku, a następnie wróć do programu Visual Studio, aby zsynchronizować zmiany.

Edytuj `EmployeeItemController.cs` pliku i zapewnić ich wyglądać następująco:

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

Spojrzenie na ten kod szczegółowo, klasa dziedziczy po klasie `NSCollectionViewItem` tak może działać jako prototyp komórki widok kolekcji. `Person` Właściwość udostępnia klasy, który został użyty do tworzenia powiązań danych widoku obrazu i etykiety w środowisku Xcode. Jest to wystąpienie `PersonModel` utworzone powyżej.

`BackgroundColor` Właściwości to skrót do `NSBox` formantu `FillColor` który będzie używany do określenia stanu zaznaczenia komórki. Przez zastąpienie `Selected` właściwość `NSCollectionViewItem`, poniższy kod ustawia lub usuwa ten stan zaznaczenia:

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

### <a name="creating-the-collection-view-data-source"></a>Tworzenie widoku źródła danych kolekcji

Widok źródła danych kolekcji (`NSCollectionViewDataSource`) zawiera wszystkie dane dla widoku kolekcji i tworzy i wypełnia komórkę widoku kolekcji (przy użyciu `.xib` prototypu) zgodnie z wymogami dla każdego elementu w kolekcji.

Dodaj nową klasę projektu, wywołać ją `CollectionViewDataSource` i zapewnić ich wyglądać następująco:

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

Spojrzenie na ten kod szczegółowo, klasa dziedziczy po klasie `NSCollectionViewDataSource` i udostępnia listę `PersonModel` wystąpienia za pomocą jego `Data` właściwości.

Ponieważ ta kolekcja ma tylko jedną sekcję, kod zastępuje `GetNumberOfSections` — metoda i zawsze zwraca `1`. Ponadto `GetNumberofItems` metoda zostanie przesłonięta w zwraca liczbę elementów w `Data` listy właściwości.

`GetItem` Metoda jest wywoływana, gdy nowe komórki jest wymagany i wygląda podobnie do następującej:

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

`MakeItem` Wywoływana jest metoda widoku kolekcji do utworzenia lub zwracać wielokrotnego użytku wystąpienie `EmployeeItemController` i jego `Person` właściwość jest ustawiona na element będzie wyświetlany w komórce żądanej. 

`EmployeeItemController` Musi być zarejestrowana w kolekcji kontroler widoku wcześniej przy użyciu następującego kodu:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

**Identyfikator** (`EmployeeCell`) używane w `MakeItem` wywołać _musi_ zgodna z nazwą kontrolera widoku, który został zarejestrowany z widokiem kolekcji. W tym kroku zostanie omówiona szczegółowo poniżej.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>Wybór elementu obsługi

Do obsługi deselection elementów w kolekcji, a wybór `NSCollectionViewDelegate` będzie wymagane. Ponieważ w tym przykładzie będzie można za pomocą wbudowanego `NSCollectionViewFlowLayout` typu układu `NSCollectionViewDelegateFlowLayout` konieczności określoną wersję tego obiektu delegowanego.

Dodaj nową klasę w projekcie wywołać ją `CollectionViewDelegate` i zapewnić ich wyglądać następująco:

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

`ItemsSelected` i `ItemsDeselected` metody są zastępowane i używany do ustawiania i czyszczenia `PersonSelected` właściwości kontrolera widoku, który jest obsługa widok kolekcji, gdy użytkownik zaznaczy lub odznacza elementu. Będzie on wyświetlany szczegółowo poniżej.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Tworzenie widoku kolekcji konstruktora interfejsu

Wszystkie wymagane elementy pomocnicze w miejscu mogą być edytowane głównego storyboard i widok kolekcji do niej dodać.

Wykonaj następujące czynności:

1. Kliknij dwukrotnie `Main.Storyboard` w pliku **Eksploratora rozwiązań** go otworzyć do edycji w środowisku Xcode do konstruktora interfejsu.
2. Przeciągnij widok kolekcji do widoku głównego i ono widoku:

    ![Dodawanie widok kolekcji do układu](collection-view-images/collection01.png)
3. Z widokiem kolekcji zaznaczone Aby przypiąć go do widoku podczas jego rozmiar jest zmieniany za pomocą edytora ograniczenia:

    ![Dodanie ograniczeń](collection-view-images/collection02.png)
4. Upewnij się, że widok kolekcji jest zaznaczona w **powierzchni projektowej** (i nie **z obramowaniem przewijania widoku** lub **widok klipu** zawierający go), przełącz się do  **Edytor Asystenta** i Utwórz **gniazda** widoku kolekcji:

    ![Dodanie ograniczeń](collection-view-images/collection03.png)
5. Zapisz zmiany i wróć do programu Visual Studio do synchronizacji.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>W jednym wszystkie wspólne

Wszystkie elementy towarzyszące teraz została wprowadzona w miejscu z klasy do działania jako model danych (`PersonModel`), `NSCollectionViewDataSource` został dodany do dostarczania danych, `NSCollectionViewDelegateFlowLayout` został utworzony w celu obsługi Wybór elementu i `NSCollectionView` został dodany do scenorysu Main i udostępniany jako gniazda (`EmployeeCollection`).

Ostatnim krokiem jest Edytuj kontroler widoku, który zawiera widok kolekcji i Przełącz wszystkie elementy są ze sobą w celu wypełnienia kolekcji i obsługiwać wybór elementu.

Edytuj `ViewController.cs` pliku i zapewnić ich wyglądać następująco:

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

Biorąc szczegółowo, obejrzyj ten kod `Datasource` właściwość jest zdefiniowana, aby pomieścić wystąpienia `CollectionViewDataSource` który będzie dostarczać dane dla widoku kolekcji. A `PersonSelected` właściwość jest zdefiniowana, aby pomieścić `PersonModel` reprezentujących obecnie wybranego elementu w widoku kolekcji. Ta właściwość jest również zgłasza `SelectionChanged` zdarzenie, gdy zmieni się zaznaczenie.

`ConfigureCollectionView` Klasa jest używana do rejestrowania kontroler widoku, który działa jako prototyp komórki z widokiem kolekcji przy użyciu następującego wiersza:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

Zwróć uwagę, że **identyfikator** (`EmployeeCell`) używany do rejestrowania dopasowań prototypu jedną o nazwie w `GetItem` metody `CollectionViewDataSource` zdefiniowanych powyżej:

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

Ponadto typ kontrolera widoku **musi** zgodna z nazwą `.xib` pliku, który definiuje prototyp **dokładnie**. W tym przykładzie `EmployeeItemController` i `EmployeeItemController.xib`.

Rzeczywisty układ elementów w kolekcji widoku jest kontrolowany przez klasę układ widoku kolekcji i można zmienić dynamicznie w czasie wykonywania, przypisując nowe wystąpienie do `CollectionViewLayout` właściwości. Zmienianie tej właściwości aktualizuje wygląd widok kolekcji bez zmiany animacji.

Apple dostarczany dwa typy wbudowane układu z widokiem kolekcji, która obsłuży najbardziej typowych zastosowań: `NSCollectionViewFlowLayout` i `NSCollectionViewGridLayout`. Jeśli dewelopera wymagane formatu niestandardowego, takich jak tworzenie elementów w koło, mogą utworzyć niestandardowego wystąpienia `NSCollectionViewLayout` i zastępowanie metody wymagane do osiągnięcia pożądanych efektów.

W tym przykładzie użyto domyślny układ przepływu, więc tworzy wystąpienie `NSCollectionViewFlowLayout` klasy i konfiguruje w następujący sposób:

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

`ItemSize` Właściwość określa rozmiar każdego pojedynczych komórek w kolekcji. `SectionInset` Właściwość definiuje marginesy od krawędzi komórki zostanie rozmieszczona w kolekcji. `MinimumInteritemSpacing` Określa minimalny odstęp między elementami i `MinimumLineSpacing` Określa minimalny odstęp między wierszami w kolekcji.

Układ jest przypisany do widoku kolekcji i wystąpienia `CollectionViewDelegate` jest dołączony do obsługi Wybór elementu:

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

`PopulateWithData` Metoda tworzy nowe wystąpienie klasy `CollectionViewDataSource`, wypełnia danych, dołącza go do kolekcji widoku i wywołania `ReloadData` metody w celu wyświetlenia elementów:

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

`ViewDidLoad` Metoda zostanie przesłonięta i wywołuje `ConfigureCollectionView` i `PopulateWithData` metody wyświetlania dla użytkownika końcowego widok kolekcji:

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

W tym artykule trwało szczegółowe przyjrzeć się praca z widokami kolekcja w aplikacji Xamarin.Mac. Najpierw należy go przeglądał udostępnianie klasy C# do języka Objective-C za pomocą kluczy i wartości kodowania (KVC) i klucz-wartość obserwowania (KVO). Następnie go pokazano, jak użyć klasy zgodne KVO i danych powiązać go z widokiem kolekcji w Konstruktorze interfejsu w środowisku Xcode. Na koniec go pokazano, jak wchodzić w interakcje z widokiem kolekcji w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacCollectionNew (przykład)](https://developer.xamarin.com/samples/mac/MacCollectionNew/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
