---
title: Widoki konspektu dla platformy Xamarin.Mac
description: Ten artykuł dotyczy pracy z widoki konspektu w aplikacji platformy Xamarin.Mac. Opisano w nim tworzenia i utrzymywania widoki konspektu w środowisku Xcode i programu Interface Builder oraz pracy z nimi programistycznie.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7228a22eeae3dfdb354ba3693c277251fd2cd821
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780663"
---
# <a name="outline-views-in-xamarinmac"></a>Widoki konspektu dla platformy Xamarin.Mac

_Ten artykuł dotyczy pracy z widoki konspektu w aplikacji platformy Xamarin.Mac. Opisano w nim tworzenia i utrzymywania widoki konspektu w środowisku Xcode i programu Interface Builder oraz pracy z nimi programistycznie._

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tej samej konspektu widoków deweloperem, pracy w *języka Objective-C* i *Xcode* jest. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, można użyć program Xcode _programu Interface Builder_ można tworzyć i obsługiwać własne widoki konspektu (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Wyświetlanie konspektu jest typem tabeli, który umożliwia użytkownikowi rozwinąć lub zwinąć wiersze danych hierarchicznych. Tak jak widoku tabeli widoku konspektu wyświetla dane dla zestawu powiązanych elementów z wierszami reprezentujących poszczególnych elementów i kolumn reprezentujących atrybuty te elementy. W przeciwieństwie do widoku tabeli elementy w widoku konspektu nie są w formie płaskiej listy, są zorganizowane w hierarchii, takie jak pliki i foldery na dysku twardym.

[![](outline-view-images/populate03.png "Uruchamianie przykładowej aplikacji")](outline-view-images/populate03.png#lightbox)

W tym artykule omówiono podstawowe informacje dotyczące pracy z widokami konturu w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` poleceń Umożliwia o komunikacji sieciowej w górę klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Wprowadzenie do konturu widoków

Wyświetlanie konspektu jest typem tabeli, który umożliwia użytkownikowi rozwinąć lub zwinąć wiersze danych hierarchicznych. Tak jak widoku tabeli widoku konspektu wyświetla dane dla zestawu powiązanych elementów z wierszami reprezentujących poszczególnych elementów i kolumn reprezentujących atrybuty te elementy. W przeciwieństwie do widoku tabeli elementy w widoku konspektu nie są w formie płaskiej listy, są zorganizowane w hierarchii, takie jak pliki i foldery na dysku twardym.

Jeśli element w widoku konspektu zawiera inne elementy, można można rozwijać i zwijać przez użytkownika. Element można rozwijać Wyświetla trójkąta, które wskazuje po prawej stronie, gdy element jest zwinięta punktów w dół, gdy element jest rozwinięty. Kliknięcie trójkąta powoduje, że element, aby rozwinąć lub zwinąć.

Wyświetlanie konspektu (`NSOutlineView`) jest podklasą widok tabeli (`NSTableView`) i w efekcie dziedziczy większość swojego zachowania od swojej klasy nadrzędnej. W rezultacie wiele operacji obsługiwanych przez widoku tabeli, takie jak Wybieranie wierszy lub kolumn, zmienianie położenia kolumn przez przeciągnięcie nagłówków kolumn itp., są również obsługiwane przez widok konspektu. Aplikacja platformy Xamarin.Mac ma kontrolę nad tych funkcji, a następnie można skonfigurować parametry widoku konspektu, (albo w kodzie albo programu Interface Builder), aby umożliwiać lub uniemożliwiać pewne operacje.

Wyświetlanie konspektu nie przechowuje danych własne, zamiast tego opiera się na źródle danych (`NSOutlineViewDataSource`) wiersze i kolumny są wymagane na zgodnie z potrzebami.

Zachowanie widoku konspektu można dostosować, podając podklasę delegata wyświetlanie konspektu (`NSOutlineViewDelegate`) do obsługi zarządzania kolumny konspektu, wpisz wybierz funkcje, wybór wiersza i edytowania, niestandardowe śledzenia i widoki niestandardowe dla poszczególnych kolumn i wierszy.

Ponieważ widoku konspektu udostępnia wiele jej działanie oraz funkcje widoku tabeli, możesz chcieć przejść przez naszych [widoki tabel](~/mac/user-interface/table-view.md) dokumentacji, przed kontynuowaniem w tym artykule.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Utworzenie i utrzymywanie widoki konspektu w środowisku Xcode

Kiedy tworzysz nową aplikację cocoa dla platformy Xamarin.Mac, otrzymasz standardowego okna puste, domyślnie. Ten system windows jest zdefiniowany w `.storyboard` pliku automatycznie uwzględnione w projekcie. Aby edytować projektu systemu windows w **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku:

[![](outline-view-images/edit01.png "Wybieranie głównego storyboard")](outline-view-images/edit01.png#lightbox)

Spowoduje to otwarcie projektu okna w program Xcode Interface Builder:

[![](outline-view-images/edit02.png "Edytowanie interfejsu użytkownika w środowisku Xcode")](outline-view-images/edit02.png#lightbox)

Typ `outline` do **Inspektor biblioteki** pola wyszukiwania, aby ułatwić znajdowanie kontrolki widoku konspektu:

[![](outline-view-images/edit03.png "Wybieranie widoku konspektu z biblioteki")](outline-view-images/edit03.png#lightbox)

Przeciągnij widoku konspektu z kontroler widoku w **Edytor interfejsu**, wypełnienia obszaru zawartości kontrolera widoku i ustaw ją na którym zmniejsza i rośnie wraz z okna **edytora ograniczeń**:

[![](outline-view-images/edit04.png "Edytowanie ograniczenia")](outline-view-images/edit04.png#lightbox)

Wybierz widok konspektu **hierarchii interfejsów** oraz następujące właściwości są dostępne w **Inspektor atrybut**:

[![](outline-view-images/edit05.png "Inspektor atrybutu")](outline-view-images/edit05.png#lightbox)

- **Konspekt kolumny** -kolumny tabeli, w którym wyświetlane są dane hierarchiczne.
- **Kolumna konspektu zapisywania** — Jeśli `true`, kolumnie konspektu będzie można automatycznie zapisywany i przywracany między uruchomieniem aplikacji.
- **Wcięcie** -wielkość wcięcia kolumny poniżej rozwinięty element.
- **Wcięcie następuje komórek** — Jeśli `true`, znacznik wcięcia są grupowane razem z komórek.
- **Elementy rozwinięte zapisywania** — Jeśli `true`, stan rozwinięty/zwinięty elementy będzie można automatycznie zapisywany i przywracany między uruchomieniem aplikacji.
- **Zawartości w trybie** — umożliwia korzystanie z widoków albo (`NSView`) lub komórki (`NSCell`) aby wyświetlić dane w wiersze i kolumny. Począwszy od systemu macOS 10.7, skorzystaj z widoków.
- **Liczby zmiennoprzecinkowe grupowanie wierszy** — Jeśli `true`, widok tabeli narysuje pogrupowanych komórek tak, jakby ich liczb zmiennoprzecinkowych.
- **Kolumny** — określa liczbę kolumn wyświetlanych.
- **Nagłówki** — Jeśli `true`, kolumny będą mieć nagłówki.
- **Zmiana kolejności** — Jeśli `true`, użytkownik będzie mógł przeciągnij Zmienianie kolejności kolumn w tabeli.
- **Zmiana rozmiaru** — Jeśli `true`, użytkownik będzie mógł przeciągnij nagłówek kolumny w celu zmiany rozmiaru kolumn.
- **Ustawianie rozmiaru kolumn** — Określa, jak automatycznie rozmiar kolumn w tabeli.
- **Wyróżnij** — formanty, które korzysta z typu wyróżniania w tabeli po wybraniu komórki.
- **Alternatywny wierszy** — Jeśli `true`, nigdy nie innych wierszy mają inny kolor tła.
- **Siatka pozioma** -wybiera typ obramowania wstawiany między komórkami w poziomie.
- **Siatka pionowa** -wybiera typ obramowania nakreślić między komórkami w pionie.
- **Kolor siatki** -Ustawia kolor obramowania komórki.
- **Tło** -Ustawia kolor tła komórek.
- **Wybór** — można kontrolować, jak użytkownik może wybrać komórek w tabeli jako:
    - **Wiele** — Jeśli `true`, użytkownik może wybrać wiele wierszy i kolumn.
    - **Kolumna** — Jeśli `true`, użytkownik może wybrać kolumny.
    - **Wybierz typ** — Jeśli `true`, którą użytkownik może wpisać znaków do zaznaczenia wiersza.
    - **Pusty** — Jeśli `true`, użytkownik nie jest wymagany, aby zaznaczyć wiersz lub kolumnę, tabela pozwala na Brak zaznaczenia w ogóle.
- **Automatyczne zapisywanie** — nazwę i format tabel jest automatycznie Zapisz w obszarze.
- **Informacje o kolumnach** — Jeśli `true`, kolejność i szerokość kolumn zostaną zapisane automatycznie.
- **Podziały** — wybierz, jak komórki obsługuje podziały wierszy.
- **Obcina ostatniego wiersza widocznego** — Jeśli `true`, komórki zostaną obcięte w można dane nie mieści się w jego granice.

> [!IMPORTANT]
> Jeśli nie obsługujesz starszą aplikację platformy Xamarin.Mac, `NSView` oparte na widoki konspektu powinny być używane za pośrednictwem `NSCell` na podstawie widoki tabel. `NSCell` jest uznawany za starszej wersji i może nie być obsługiwana przyszłości.

Wybierz kolumnę tabeli w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **Inspektor atrybut**:

[![](outline-view-images/edit06.png "Inspektor atrybutu")](outline-view-images/edit06.png#lightbox)

- **Tytuł** -Ustawia tytuł kolumny.
- **Wyrównanie** — Ustaw wyrównanie tekstu w komórkach.
- **Czcionka tytułu** -wybiera czcionkę tekstu nagłówka komórki.
- **Klucz sortowania** — jest używany do sortowania danych w kolumnie. Pozostaw puste, jeśli użytkownik nie można sortować w tej kolumnie.
- **Selektor** — jest **akcji** używany do wykonania sortowania. Pozostaw puste, jeśli użytkownik nie można sortować w tej kolumnie.
- **Kolejność** — jest kolejność sortowania dla kolumn danych.
- **Zmiana rozmiaru** -wybiera typ zmiany rozmiaru kolumny.
- **Można edytować** — Jeśli `true`, użytkownik może edytować komórek w tabeli na podstawie komórki.
- **Ukryte** — Jeśli `true`, kolumna jest ukryta.

Można również zmienić rozmiar kolumny, przeciągając jej uchwyt (wyśrodkowane w pionie po prawej stronie kolumny) po lewej lub w prawo.

Teraz wybierz każdej kolumny w widoku tabeli do naszych i nadaj pierwszej kolumny **tytuł** z `Product` i drugi `Details`.

Wybierz widok komórki tabeli (`NSTableViewCell`) w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **Inspektor atrybut**:

[![](outline-view-images/edit07.png "Inspektor atrybutu")](outline-view-images/edit07.png#lightbox)

Są to wszystkie właściwości standardowy widok. Istnieje również możliwość zmiany rozmiaru wierszy dla tej kolumny w tym miejscu.

Komórka widoku tabeli (domyślnie jest to `NSTextField`) w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **Inspektor atrybut**:

[![](outline-view-images/edit08.png "Inspektor atrybutu")](outline-view-images/edit08.png#lightbox)

Będziesz mieć właściwości ustawione w tym miejscu standardowego pola tekstowego. Domyślnie standardowego pola tekstowego służy do wyświetlania danych dla komórek w kolumnie.

Wybierz widok komórki tabeli (`NSTableFieldCell`) w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **Inspektor atrybut**:

[![](outline-view-images/edit09.png "Inspektor atrybutu")](outline-view-images/edit09.png#lightbox)

Najważniejsze funkcje to:

- **Układ** — wybierz, w jaki sposób są ułożone komórek w tej kolumnie.
- **Używa pojedynczego wiersza trybu** — Jeśli `true`, komórki jest ograniczona do jednego wiersza.
- **Pierwszy szerokość układu środowiska uruchomieniowego** — Jeśli `true`, komórki zostanie Preferuj szerokości, ustaw dla niego (ręcznie lub automatycznie) kiedy jest wyświetlana przy pierwszym uruchomieniu aplikacji.
- **Akcja** -Określa, kiedy edycji **akcji** są wysyłane do komórki.
- **Zachowanie** — Określa, czy zawartość komórki jest możliwy lub nie można edytować.
- **Tekst sformatowany** — Jeśli `true`, komórki wyświetlić tekst sformatowany i stylem.
- **Cofnij** — Jeśli `true`, komórki przyjmuje, że jest ona Cofnij zachowanie.

Wybierz widok komórki tabeli (`NSTableFieldCell`) w dolnej części kolumny tabeli w **hierarchii interfejsów**:

[![](outline-view-images/edit11.png "Wybierając widok komórki tabeli")](outline-view-images/edit10.png#lightbox)

Umożliwia to edytowanie widoku komórki tabeli, zostanie użyty jako podstawa _wzorzec_ dla wszystkich komórek utworzone dla danej kolumny.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Dodawanie akcji i gniazd

Po prostu jak inne kontrolki interfejsu użytkownika Cocoa, należy udostępnić nasze widoku konspektu i jest kolumn i komórki do języka C# kodu za pomocą **akcje** i **gniazd** (oparte na funkcje wymagane).

Proces jest taka sama dla każdego elementu widoku konspektu, którą chcemy udostępnić:

1. Przełącz się do **edytora Asystenta** i upewnij się, że `ViewController.h` wybrany plik: 

    [![](outline-view-images/edit11.png "Wybieranie pliku poprawne .h")](outline-view-images/edit11.png#lightbox)
2. Wybierz widok konspektu z **hierarchii interfejsów**, przytrzymując klawisz CTRL i przeciągnij, aby `ViewController.h` pliku.
3. Tworzenie **ujścia** dla widoku konspektu o nazwie `ProductOutline`: 

    [![](outline-view-images/edit13.png "Konfigurowanie źródła")](outline-view-images/edit13.png#lightbox)
4. Tworzenie **gniazd** w kolumnach tabeli o nazwie `ProductColumn` i `DetailsColumn`: 

    [![](outline-view-images/edit14.png "Konfigurowanie źródła")](outline-view-images/edit14.png#lightbox)
5. Zapisać zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Następnie będziemy pisać wyświetlania kodu danych konspektu po uruchomieniu aplikacji.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Wypełnianie widoku konspektu

Z widoku nasze konspektu zaprojektowanych w programu Interface Builder i udostępniane za pośrednictwem **ujścia**, następnie należy utworzyć kod C#, aby je wypełnić.

Po pierwsze Utwórzmy nowy `Product` klasy do przechowywania informacji dla poszczególnych wierszy i grupy produktów sub. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `Product` dla **nazwa** i kliknij przycisk **New** przycisku:

[![](outline-view-images/populate01.png "Tworzenie pustą klasę")](outline-view-images/populate01.png#lightbox)

Wprowadź `Product.cs` wygląd pliku, jak pokazano poniżej:

```csharp
using System;
using Foundation;
using System.Collections.Generic;

namespace MacOutlines
{
    public class Product : NSObject
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        public bool IsProductGroup {
            get { return (Products.Count > 0); }
        }
        #endregion

        #region Constructors
        public Product ()
        {
        }

        public Product (string title, string description)
        {
            this.Title = title;
            this.Description = description;
        }
        #endregion
    }
}
```

Następnie należy utworzyć podklasę klasy `NSOutlineDataSource` do dostarczania danych do naszych konspektu, ponieważ żądanie. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `ProductOutlineDataSource` dla **nazwa** i kliknij przycisk **New** przycisku.

Edytuj `ProductTableDataSource.cs` plików i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDataSource : NSOutlineViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductOutlineDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetChildrenCount (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products.Count;
            } else {
                return ((Product)item).Products.Count;
            }

        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, NSObject item)
        {
            if (item == null) {
                return Products [childIndex];
            } else {
                return ((Product)item).Products [childIndex];
            }
                
        }

        public override bool ItemExpandable (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products [0].IsProductGroup;
            } else {
                return ((Product)item).IsProductGroup;
            }
        
        }
        #endregion
    }
}
```

Ta klasa ma magazyn dla naszych wyświetlanie konspektu elementów i zastępuje `GetChildrenCount` aby zwrócić liczbę wierszy w tabeli. `GetChild` Zwraca określony element nadrzędny lub podrzędny (zgodnie z żądaniem widoku konspektu) i `ItemExpandable` definiuje określony element jako element nadrzędny lub element podrzędny.

Na koniec należy utworzyć podklasę klasy `NSOutlineDelegate` aby zapewnić zachowanie dla naszych konspektu. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `ProductOutlineDelegate` dla **nazwa** i kliknij przycisk **New** przycisku.

Edytuj `ProductOutlineDelegate.cs` plików i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDelegate : NSOutlineViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductOutlineDataSource DataSource;
        #endregion

        #region Constructors
        public ProductOutlineDelegate (ProductOutlineDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)outlineView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Cast item
            var product = item as Product;

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = product.Title;
                break;
            case "Details":
                view.StringValue = product.Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Kiedy tworzymy wystąpienie `ProductOutlineDelegate`, możemy również przekazać w wystąpieniu `ProductOutlineDataSource` zapewniający danych konspektu. `GetView` Metoda jest odpowiedzialna za zwrócenie widoku (dane), aby wyświetlić komórki, które zapewniają kolumn i wierszy. Jeśli to możliwe istniejącemu widokowi zostanie ono użyte ponownie, aby wyświetlić komórkę, jeśli nie należy utworzyć nowy widok.

Aby wypełnić konspektu, możemy edytować `MainWindow.cs` pliku i upewnij `AwakeFromNib` wygląd metoda podobne do następującego:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create data source and populate
    var DataSource = new ProductOutlineDataSource ();

    var Vegetables = new Product ("Vegetables", "Greens and Other Produce");
    Vegetables.Products.Add (new Product ("Cabbage", "Brassica oleracea - Leaves, axillary buds, stems, flowerheads"));
    Vegetables.Products.Add (new Product ("Turnip", "Brassica rapa - Tubers, leaves"));
    Vegetables.Products.Add (new Product ("Radish", "Raphanus sativus - Roots, leaves, seed pods, seed oil, sprouting"));
    Vegetables.Products.Add (new Product ("Carrot", "Daucus carota - Root tubers"));
    DataSource.Products.Add (Vegetables);

    var Fruits = new Product ("Fruits", "Fruit is a part of a flowering plant that derives from specific tissues of the flower");
    Fruits.Products.Add (new Product ("Grape", "True Berry"));
    Fruits.Products.Add (new Product ("Cucumber", "Pepo"));
    Fruits.Products.Add (new Product ("Orange", "Hesperidium"));
    Fruits.Products.Add (new Product ("Blackberry", "Aggregate fruit"));
    DataSource.Products.Add (Fruits);

    var Meats = new Product ("Meats", "Lean Cuts");
    Meats.Products.Add (new Product ("Beef", "Cow"));
    Meats.Products.Add (new Product ("Pork", "Pig"));
    Meats.Products.Add (new Product ("Veal", "Young Cow"));
    DataSource.Products.Add (Meats);

    // Populate the outline
    ProductOutline.DataSource = DataSource;
    ProductOutline.Delegate = new ProductOutlineDelegate (DataSource);

}
```

Jeśli możemy uruchomić aplikację, wyświetlane są następujące:

[![](outline-view-images/populate02.png "W widoku zwiniętym")](outline-view-images/populate02.png#lightbox)

Jeśli możemy rozwinąć węzeł w widoku konspektu, będzie to wyglądać następująco:

[![](outline-view-images/populate03.png "Rozwinięty widok")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Sortowanie według kolumny

Zezwolimy na użytkownika posortować dane w konspekcie, klikając nagłówek kolumny. Po pierwsze, kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w programu Interface Builder. Wybierz `Product` kolumny, wprowadź `Title` dla **klucz sortowania**, `compare:` dla **selektor** i wybierz `Ascending` dla **kolejności**:

[![](outline-view-images/sort01.png "Ustawienie kolejności klucza sortowania")](outline-view-images/sort01.png#lightbox)

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Teraz możemy edytować `ProductOutlineDataSource.cs` pliku i dodaj następujące metody:

```csharp
public void Sort(string key, bool ascending) {

    // Take action based on key
    switch (key) {
    case "Title":
        if (ascending) {
            Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
        } else {
            Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
        }
        break;
    }
}

public override void SortDescriptorsChanged (NSOutlineView outlineView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    outlineView.ReloadData ();
}
```

`Sort` Metody umożliwiają sortowanie danych w źródle danych, na podstawie danego `Product` pola klasy w porządku rosnącym lub malejącym. Zastąpione `SortDescriptorsChanged` metoda zostanie wywołana, za każdym razem, gdy użycie kliknięciu nagłówka kolumny. Zostaną przekazane **klucz** wartość, która jest ustawiany w programu Interface Builder i porządek sortowania dla tej kolumny.

Jeśli firma Microsoft może uruchomić aplikację, a następnie kliknij nagłówek kolumny, wiersze będą sortowane według tej kolumny:

[![](outline-view-images/sort02.png "Przykład posortowanych danych wyjściowych")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Zaznaczenie wiersza

Jeśli chcesz umożliwić użytkownikowi wybrać pojedynczy wiersz, kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w programu Interface Builder. Wybierz widok konspektu **hierarchii interfejsów** i usuń zaznaczenie pola wyboru **wielu** pola wyboru w **Inspektor atrybut**:

[![](outline-view-images/select01.png "Inspektor atrybutu")](outline-view-images/select01.png#lightbox)

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.


Następnie Edytuj `ProductOutlineDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Umożliwi to użytkownikowi na wybranie dowolnego pojedynczego wiersza w widoku konspektu. Zwróć `false` dla `ShouldSelectItem` dla dowolny element, którego nie chcesz, użytkownik będzie mógł wybrać lub `false` dla każdego elementu, jeśli nie chcesz, aby użytkownik będzie mógł wybrać wszystkie elementy.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Wybór wielokrotny wiersza

Jeśli chcesz zezwolić na użytkownika wybrać wiele wierszy, kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w programu Interface Builder. Wybierz widok konspektu **hierarchii interfejsów** i sprawdź **wielu** pola wyboru w **Inspektor atrybut**:

[![](outline-view-images/select02.png "Inspektor atrybutu")](outline-view-images/select02.png#lightbox)

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.


Następnie Edytuj `ProductOutlineDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Umożliwi to użytkownikowi na wybranie dowolnego pojedynczego wiersza w widoku konspektu. Zwróć `false` dla `ShouldSelectRow` dla dowolny element, którego nie chcesz, użytkownik będzie mógł wybrać lub `false` dla każdego elementu, jeśli nie chcesz, aby użytkownik będzie mógł wybrać wszystkie elementy.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Typ, aby zaznaczyć wiersz

Jeśli chcesz umożliwić użytkownikowi wpisz znak z widokiem konspektu wybrane, a następnie wybierz pierwszy wiersz, który ma ten znak, kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w programu Interface Builder. Wybierz widok konspektu **hierarchii interfejsów** i sprawdź **wybierz typ** pola wyboru w **Inspektor atrybut**:

[![](outline-view-images/type01.png "Edytowanie typu wiersza")](outline-view-images/type01.png#lightbox)

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Teraz możemy edytować `ProductOutlineDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override NSObject GetNextTypeSelectMatch (NSOutlineView outlineView, NSObject startItem, NSObject endItem, string searchString)
{
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains (searchString)) {
            return product;
        }
    }

    // Not found
    return null;
}
```

`GetNextTypeSelectMatch` Metoda przyjmuje danego `searchString` i zwraca element pierwszy `Product` z tego ciągu w jego `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Zmienianie kolejności kolumn

Jeśli chcesz zezwolić użytkownikowi na przeciąganie kolejności kolumn w widoku konspektu, kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w programu Interface Builder. Wybierz widok konspektu **hierarchii interfejsów** i sprawdź **Reordering** pola wyboru w **Inspektor atrybut**:

[![](outline-view-images/reorder01.png "Inspektor atrybutu")](outline-view-images/reorder01.png#lightbox)

Jeśli firma Microsoft daje wartość **zapisywania** właściwości i sprawdź **informacji o kolumnie** pola, wszelkie zmiany, udostępnimy do układu tabeli zostaną automatycznie zapisane dla nas i przywrócić ją przy następnym zostanie uruchomiony.

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Teraz możemy edytować `ProductOutlineDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Metoda powinna zwrócić `true` dla każdej kolumny, które chcesz zezwolić na to przeciągania zmieniana w `newColumnIndex`, inne zwrot `false`;

Jeśli możemy uruchomić aplikację, aby zmienić kolejność kolumn naszych można przeciągnąć nagłówków kolumn wokół:

[![](outline-view-images/reorder02.png "Przykład opcję kolumn")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Edytowanie komórki

Jeśli chcesz umożliwić użytkownikowi edytowanie wartości dana komórka, edytowanie `ProductOutlineDelegate.cs` pliku, a następnie zmień `GetViewForItem` metody w następujący sposób:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.StringValue;
            break;
        case "Details":
            prod.Description = view.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = product.Title;
        break;
    case "Details":
        view.StringValue = product.Description;
        break;
    }

    return view;
}
```

Jeśli możemy uruchomić aplikację, użytkownik może edytować komórki w widoku tabeli:

[![](outline-view-images/editing01.png "Przykład edytowanie komórki")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>Używanie obrazów w widoki konspektu

Do dołączenia obrazu komórki w `NSOutlineView`, musisz zmienić, jak dane są zwracane przez widok konspektu `NSTableViewDelegate's` `GetView` metodę `NSTableCellView` zamiast typowej `NSTextField`. Na przykład:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Aby uzyskać więcej informacji, zobacz [przy użyciu obrazów za pomocą widoki konspektu](~/mac/app-fundamentals/image.md) części naszych [pracy przy użyciu obrazu](~/mac/app-fundamentals/image.md) dokumentacji.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Widoki konspektu powiązania danych

Przy użyciu technik kodowania pary klucz-wartość i powiązania danych w aplikacji platformy Xamarin.Mac, może znacznie skrócić ilość kodu, który trzeba napisać i obsługiwać do wypełniania i pracować z elementami interfejsu użytkownika. Masz także zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) z usługi frontonu zakończenia interfejsu użytkownika (_Model-View-Controller_), prowadzącego do łatwiejsza Obsługa bardziej elastycznych aplikacji Projekt.

Kodowanie klucz-wartość (KVC) to mechanizm służący do uzyskiwania dostępu do właściwości obiektu pośrednio, za pomocą klucze (specjalnie sformatowanego ciągi), aby zidentyfikować właściwości zamiast uzyskanie do nich dostępu za pośrednictwem zmienne wystąpienia lub metody dostępu (`get/set`). Wdrażając kodowanie klucz-wartość zgodne metod dostępu do aplikacji platformy Xamarin.Mac, możesz uzyskać dostęp do innych funkcji systemu macOS, takich jak obserwowania pary klucz-wartość (KVO), powiązań danych, danych podstawowych, cocoa dla powiązania i scriptability.

Aby uzyskać więcej informacji, zobacz [powiązanie danych widoku konspektu](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) części naszych [powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md) dokumentacji.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z widoki konspektu w aplikacji platformy Xamarin.Mac. Firma dostrzegła różnych typów i używa widoki konspektu, jak tworzyć i obsługiwać widoki konspektu w program Xcode Interface Builder i sposób pracy z widoki konspektu w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacOutlines (przykład)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (przykład)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Listy źródeł](~/mac/user-interface/source-list.md)
- [Powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do konturu widoków](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
