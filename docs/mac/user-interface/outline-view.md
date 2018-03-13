---
title: Widoki konspektu
description: "W tym artykule omówiono pracy z widokami konspektu w aplikacji Xamarin.Mac. Opisuje tworzenie i utrzymywanie konspektu widoków w programie Xcode i kompilatora interfejsu oraz pracy z nimi programistycznie."
ms.topic: article
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: dbbd10af046c0a8421e06e675364f92405b2317f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="outline-views"></a>Widoki konspektu

_W tym artykule omówiono pracy z widokami konspektu w aplikacji Xamarin.Mac. Opisuje tworzenie i utrzymywanie konspektu widoków w programie Xcode i kompilatora interfejsu oraz pracy z nimi programistycznie._

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tej samej konspektu widoków używająca *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć w środowisku Xcode _konstruktora interfejsu_ do tworzenia i obsługi widoków konspektu (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Wyświetlanie konspektu jest typem tabeli, która pozwala użytkownikowi rozwiń lub Zwiń wierszy hierarchiczne dane. Tak jak widok tabeli widoku konspektu przedstawia dane dotyczące zbiór elementów powiązanych z wierszami reprezentujących poszczególnych elementów i kolumn reprezentujących atrybuty tych elementów. W przeciwieństwie do widoku tabeli elementów w widoku konspektu nie znajdują się w płaskiej listy, są zorganizowane w hierarchii, takie jak pliki i foldery na dysku twardym.

[![](outline-view-images/populate03.png "Uruchom przykładową aplikację")](outline-view-images/populate03.png#lightbox)

W tym artykule firma Microsoft będzie kroki te obejmują podstawy Praca z widokami konspektu w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` poleceń używany do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Wprowadzenie do widoków konspektu

Wyświetlanie konspektu jest typem tabeli, która pozwala użytkownikowi rozwiń lub Zwiń wierszy hierarchiczne dane. Tak jak widok tabeli widoku konspektu przedstawia dane dotyczące zbiór elementów powiązanych z wierszami reprezentujących poszczególnych elementów i kolumn reprezentujących atrybuty tych elementów. W przeciwieństwie do widoku tabeli elementów w widoku konspektu nie znajdują się w płaskiej listy, są zorganizowane w hierarchii, takie jak pliki i foldery na dysku twardym.

Jeśli element w widoku konspektu znajdują się inne elementy, można rozwinięte lub zwinięte przez użytkownika. Element rozwijania wyświetli trójkąta, który wskazuje z prawej strony, gdy element jest zwinięty i punktów w dół po rozwinięciu elementu. Kliknięcie trójkąta powoduje, że element, aby rozwinąć lub zwinąć.

Wyświetlanie konspektu (`NSOutlineView`) jest podklasą widoku tabeli (`NSTableView`) i w efekcie dziedziczy znacznie jego działania od swojej klasy nadrzędnej. W związku z tym wiele operacji obsługiwane przez widoku tabeli, takich jak zaznaczania wierszy lub kolumn, zmienianie pozycji kolumn przez przeciągnięcie nagłówków kolumn itp., również są obsługiwane przez wyświetlanie konspektu. Aplikacja Xamarin.Mac ma kontrolę nad tych funkcji i parametry widoku konspektu, (albo w kodzie lub konstruktora interfejsu) można skonfigurować do Zezwalaj lub nie zezwalaj pewnych operacji.

Wyświetlanie konspektu nie przechowuje swoich danych, zamiast tego jest zależne od źródła danych (`NSOutlineViewDataSource`) wiersze i kolumny są wymagane na zgodnie z potrzebami.

Zachowanie widoku konspektu można dostosować, podając podklasą delegata widoku konspektu (`NSOutlineViewDelegate`) do obsługi zarządzania kolumny konspektu, wpisz, aby wybrać funkcje, zaznaczenie wiersza i edytowanie niestandardowych śledzenia i widoków niestandardowych dla poszczególnych kolumn i wierszy.

Ponieważ widoku konspektu udostępnia znacznie jego zachowania i funkcji w widoku tabeli, należy przejść przez naszych [widoków tabel](~/mac/user-interface/table-view.md) dokumentacji, przed kontynuowaniem w tym artykule.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Tworzenie i obsługę konspektu widoków w programie Xcode

Podczas tworzenia nowej aplikacji Xamarin.Mac Cocoa, otrzymasz standardowe okno puste, domyślnie. Ten system windows jest zdefiniowany w `.storyboard` pliku automatycznie dołączony do projektu. Aby edytować w projekcie systemu windows **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku:

[![](outline-view-images/edit01.png "Wybieranie głównego storyboard")](outline-view-images/edit01.png#lightbox)

Spowoduje to otwarcie okna projektu w Konstruktorze interfejsu w środowisku Xcode:

[![](outline-view-images/edit02.png "Edytowanie interfejsu użytkownika w środowisku Xcode")](outline-view-images/edit02.png#lightbox)

Typ `outline` do **Inspector biblioteki** pole wyszukiwania, aby ułatwić znajdowanie kontrolki widoku konspektu:

[![](outline-view-images/edit03.png "Wybranie widoku konspektu z biblioteki")](outline-view-images/edit03.png#lightbox)

Przeciągnij widoku konspektu kontrolera widoku w **Edytor interfejsu**, stał się wypełnienia obszaru zawartości kontroler widoku i ustaw ją na którym zmniejsza i płynne okna w **edytora ograniczeń**:

[![](outline-view-images/edit04.png "Edytowanie ograniczenia")](outline-view-images/edit04.png#lightbox)

Wybierz widok konspektu **hierarchii interfejsów** oraz następujące właściwości są dostępne w **inspektora atrybutu**:

[![](outline-view-images/edit05.png "Inspektor atrybutu")](outline-view-images/edit05.png#lightbox)

- **Konspekt kolumny** -kolumnie tabeli, w którym są wyświetlane dane hierarchicznej.
- **Kolumna konspektu AutoSave** — Jeśli `true`, kolumnie konspektu będzie można automatycznie zapisywany i przywracany między jest uruchamiana aplikacja.
- **Wcięcie** -wielkość wcięcia kolumn w obszarze rozwinięty element.
- **Wcięcie następuje komórek** — Jeśli `true`, znacznik wcięcia są grupowane razem z komórek.
- **Elementy rozwinięty AutoSave** — Jeśli `true`, stanu rozwinięte zwinięte elementów będzie można automatycznie zapisywany i przywracany między jest uruchamiana aplikacja.
- **Zawartości tryb** — umożliwia użycie albo widoków (`NSView`) lub komórki (`NSCell`) do wyświetlania danych w wiersze i kolumny. Począwszy od macOS 10,7, należy użyć widoków.
- **Pojawia się grupy wierszy** — Jeśli `true`, widok tabeli narysuje grupowanych komórek tak, jakby ich liczby zmiennoprzecinkowe.
- **Kolumny** — definiuje liczbę kolumn wyświetlanych.
- **Nagłówki** — Jeśli `true`, kolumny ma nagłówki.
- **Ponowne porządkowanie** — Jeśli `true`, użytkownik będzie mógł przeciągnij kolejności kolumn w tabeli.
- **Zmiana rozmiaru** — Jeśli `true`, użytkownik będzie można przeciągnięcie nagłówków kolumn, aby zmienić rozmiar kolumn.
- **Ustawianie rozmiaru kolumn** — Określa, jak automatycznie rozmiar kolumn w tabeli.
- **Wyróżnij** -formanty typu wyróżnianie tabeli używa zaznaczenie komórki.
- **Alternatywny wierszy** — Jeśli `true`, kiedykolwiek drugiego wiersza będzie mieć inny kolor tła.
- **Siatki poziomej** -wybiera typ obramowania wstawiany w poziomie między komórkami.
- **Siatka pionowa** -wybiera typ obramowania rysowany pionowo między komórkami.
- **Kolor siatki** -Ustawia kolor obramowania komórki.
- **Tło** -Ustawia kolor tła komórki.
- **Wybór** — pozwala na kontrolowanie, jak użytkownik może wybrać komórek w tabeli jako:
    - **Wiele** — Jeśli `true`, użytkownik może wybrać wiele wierszy i kolumn.
    - **Kolumna** — Jeśli `true`, użytkownik może wybrać kolumn.
    - **Wybierz typ** — Jeśli `true`, użytkownik może wpisać znaków do zaznaczenia wiersza.
    - **Pusty** — Jeśli `true`, użytkownik nie jest wymagany do wybrania wierszy lub kolumn, tabeli zezwala na Brak zaznaczenia w ogóle.
- **Automatyczne zapisywanie** -nazwę formatu tabel jest automatycznie zapisać pod.
- **Informacje dotyczące kolumn** — Jeśli `true`, kolejność i szerokość kolumn zostaną automatycznie zapisane.
- **Podziały** — wybierz, jak komórki obsługuje podziały wiersza.
- **Obcina ostatniego wiersza widocznego** — Jeśli `true`, komórki zostanie obcięty w można danych mieści się w jej granicach.

> [!IMPORTANT]
> Jeśli nie obsługujesz starszych aplikacji Xamarin.Mac `NSView` opartych na widokach konspektu powinien być używany przez `NSCell` na podstawie widoków tabel. `NSCell` uznana za przestarzałą i może nie być obsługiwana idąc dalej.

Wybierz kolumnę tabeli w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **inspektora atrybutu**:

[![](outline-view-images/edit06.png "Inspektor atrybutu")](outline-view-images/edit06.png#lightbox)

- **Tytuł** -Ustawia tytuł kolumny.
- **Wyrównanie** — Ustawianie wyrównania tekstu w komórkach.
- **Tytuł czcionki** -wybiera czcionkę tekstu nagłówka komórki.
- **Klucz sortowania** — jest używane do sortowania danych w kolumnie klucza. Pozostaw to pole puste, jeśli użytkownik nie może sortować tej kolumny.
- **Selektor** — jest **akcji** używane w celu wykonania sortowania. Pozostaw to pole puste, jeśli użytkownik nie może sortować tej kolumny.
- **Kolejność** — jest porządek sortowania dla kolumn danych.
- **Zmiana rozmiaru** -wybiera typ zmiany rozmiaru kolumny.
- **Można edytować** — Jeśli `true`, użytkownik może edytować komórki w tabeli na podstawie komórki.
- **Ukryte** — Jeśli `true`, kolumna jest ukryta.

Można również zmienić rozmiar kolumny przez przeciągnięcie jego uchwytu (wyśrodkowane w pionie w prawej kolumnie) w lewo lub w prawo.

Teraz wybierz każdej kolumny w widoku naszych tabeli i nadaj pierwszej kolumny **tytuł** z `Product` i drugi `Details`.

Wybierz widok komórki tabeli (`NSTableViewCell`) w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **inspektora atrybutu**:

[![](outline-view-images/edit07.png "Inspektor atrybutu")](outline-view-images/edit07.png#lightbox)

Są to wszystkie właściwości standardowy. Istnieje również możliwość zmiany rozmiaru wierszy dla tej kolumny w tym miejscu.

Zaznacz komórkę widoku tabeli (domyślnie jest to `NSTextField`) w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **inspektora atrybutu**:

[![](outline-view-images/edit08.png "Inspektor atrybutu")](outline-view-images/edit08.png#lightbox)

Będziesz mieć właściwości standardowego pola tekstowego do określonego w tym miejscu. Domyślnie standardowego pola tekstowego służy do wyświetlania danych dla komórek w kolumnie.

Wybierz widok komórki tabeli (`NSTableFieldCell`) w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **inspektora atrybutu**:

[![](outline-view-images/edit09.png "Inspektor atrybutu")](outline-view-images/edit09.png#lightbox)

Najważniejsze funkcje są:

- **Układ** — wybierz, w jaki sposób układ komórek w tej kolumnie.
- **Tryb pojedynczy wiersz** — Jeśli `true`, komórka jest ograniczone do jednego wiersza.
- **Pierwszy szerokość układu środowiska uruchomieniowego** — Jeśli `true`, preferowane szerokości ustawionej dla niego (ręcznie lub automatycznie) gdy komórka jest wyświetlany przy pierwszym uruchomieniu aplikacji.
- **Akcja** -sterować Edycja **akcji** jest wysyłane do komórki.
- **Zachowanie** — definiuje, jeśli komórka jest możliwy lub nie można edytować.
- **Tekst sformatowany** — Jeśli `true`, komórki można wyświetlić tekst sformatowany i styl.
- **Cofnij** — Jeśli `true`, komórki przyjmuje na siebie odpowiedzialność zostaje cofnąć zachowanie.

Wybierz widok komórki tabeli (`NSTableFieldCell`) u dołu w kolumnie tabeli **hierarchii interfejsów**:

[![](outline-view-images/edit11.png "Wybranie widoku komórki tabeli")](outline-view-images/edit10.png#lightbox)

Dzięki temu można edytować komórki tabeli widoku używany jako bazowy _wzorzec_ we wszystkich komórkach utworzone dla podanej kolumny.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Dodawanie działań i gniazda

Tylko innych formantu interfejsu użytkownika Cocoa, musimy udostępnić nasze widoku konspektu i kolumny, a aby C# kodu za pomocą **akcje** i **gniazda** (oparta na funkcjach wymaganych).

Proces jest taka sama dla każdego elementu widoku konspektu, która ma zostać udostępniona:

1. Przełącz się do **Edytor Asystenta** i upewnij się, że `ViewController.h` wybrany plik: 

    [![](outline-view-images/edit11.png "Wybieranie pliku prawidłowe .h")](outline-view-images/edit11.png#lightbox)
2. Wybierz widok konspektu z **hierarchii interfejsów**, przytrzymując klawisz CTRL i przeciągnij, aby `ViewController.h` pliku.
3. Utwórz **gniazda** wywoływanych widoku konspektu `ProductOutline`: 

    [![](outline-view-images/edit13.png "Konfigurowanie gniazda")](outline-view-images/edit13.png#lightbox)
4. Utwórz **gniazda** w kolumnach tabel o nazwie `ProductColumn` i `DetailsColumn`: 

    [![](outline-view-images/edit14.png "Konfigurowanie gniazda")](outline-view-images/edit14.png#lightbox)
5. Możesz zapisać i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Następnie firma Microsoft będzie zapisywać wyświetlania kodu niektórych danych kontur po uruchomieniu aplikacji.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Wypełnianie widoku konspektu

Z naszych widoku konspektu zaprojektowane w Konstruktorze interfejsu i udostępniane za pośrednictwem funkcji **gniazda**, następnie należy utworzyć kodu C#, aby wypełnić go.

Najpierw utwórz nową `Product` klasy do przechowywania informacji dla poszczególnych wierszy i grupy produktów sub. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `Product` dla **nazwa** i kliknij przycisk **nowy** przycisk:

[![](outline-view-images/populate01.png "Tworzenie klasy pusty")](outline-view-images/populate01.png#lightbox)

Wprowadź `Product.cs` wygląd pliku podobne do poniższych:

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

Następnie należy utworzyć podklasy `NSOutlineDataSource` do dostarczania danych do naszej konspektu jak żądanie. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `ProductOutlineDataSource` dla **nazwa** i kliknij przycisk **nowy** przycisku.

Edytuj `ProductTableDataSource.cs` pliku i zapewnić ich wyglądać następująco:

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

Ta klasa ma magazyn dla elementów naszych widoku konspektu i zastępuje `GetChildrenCount` do zwrócenia liczbę wierszy w tabeli. `GetChild` Zwraca z określonym elementem nadrzędnym lub podrzędnym (zgodnie z żądaniem widoku konspektu) i `ItemExpandable` definiuje określony element jako element nadrzędny lub element podrzędny.

Na koniec należy utworzyć podklasy `NSOutlineDelegate` przewidzieć zachowania naszych konspektu. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `ProductOutlineDelegate` dla **nazwa** i kliknij przycisk **nowy** przycisku.

Edytuj `ProductOutlineDelegate.cs` pliku i zapewnić ich wyglądać następująco:

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

Kiedy możemy utworzyć wystąpienia `ProductOutlineDelegate`, będziemy również przekazać wystąpienia `ProductOutlineDataSource` który dostarcza dane do konturu. `GetView` Metoda jest odpowiedzialna za zwracanie widoku (dane), aby wyświetlić komórki dla podać kolumn i wierszy. Jeśli to możliwe istniejącego widoku zostanie ono użyte ponownie, aby wyświetlić komórkę, jeśli nie należy utworzyć nowy widok.

Aby wypełnić konturu, umożliwia edytowanie `MainWindow.cs` plików i upewnij `AwakeFromNib` wygląd metody podobne do poniższych:

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

Czy możemy uruchomić aplikację, wyświetlane są następujące:

[![](outline-view-images/populate02.png "W widoku zwiniętym")](outline-view-images/populate02.png#lightbox)

Jeśli firma Microsoft rozwinąć węzeł w widoku konspektu, będzie mieć następującą postać:

[![](outline-view-images/populate03.png "Widoku rozwiniętego")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Sortowanie według kolumny

Załóżmy Zezwalaj użytkownikowi na posortuj dane w konspekcie przez kliknięcie nagłówka kolumny. Po pierwsze, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu. Wybierz `Product` kolumny, wprowadź `Title` dla **klucza sortowania**, `compare:` dla **selektora** i wybierz `Ascending` dla **kolejności**:

[![](outline-view-images/sort01.png "Ustawienie kolejności klucza sortowania")](outline-view-images/sort01.png#lightbox)

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Teraz załóżmy Edytuj `ProductOutlineDataSource.cs` pliku i dodaj następujące metody:

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

`Sort` Metody umożliwiają sortowanie danych w źródle danych, na podstawie danego `Product` pola klasy w porządku rosnącym lub malejącym. Zastąpione `SortDescriptorsChanged` metoda zostanie wywołana za każdym razem, gdy użycie kliknie nagłówek kolumny. Zostaną przekazane **klucza** wartość ustawioną w konstruktora interfejsu i kolejność sortowania dla tej kolumny.

Jeśli firma Microsoft może uruchomić aplikację, a następnie kliknij nagłówek kolumny, wiersze zostaną posortowane według tej kolumny:

[![](outline-view-images/sort02.png "Przykład danych wyjściowych posortowana")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Zaznaczenie wiersza

Jeśli chcesz zezwolić użytkownikowi wybierz pojedynczy wiersz, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu. Wybierz widok konspektu **hierarchii interfejsów** i usuń zaznaczenie pola wyboru **wiele** checkbox w **inspektora atrybutu**:

[![](outline-view-images/select01.png "Inspektor atrybutu")](outline-view-images/select01.png#lightbox)

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.


Następnie Edytuj `ProductOutlineDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Dzięki temu użytkownikowi na wybranie dowolnego pojedynczego wiersza w widoku konspektu. Zwraca `false` dla `ShouldSelectItem` dla żadnego elementu, który nie ma użytkownik będzie mógł wybrać lub `false` dla każdego elementu, jeśli nie chcesz, aby użytkownik będzie mógł wybrać wszystkie elementy.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Wybieranie wielu wierszy

Jeśli chcesz zezwolić użytkownikowi zaznaczyć wiele wierszy, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu. Wybierz widok konspektu **hierarchii interfejsów** i sprawdź **wiele** checkbox w **inspektora atrybutu**:

[![](outline-view-images/select02.png "Inspektor atrybutu")](outline-view-images/select02.png#lightbox)

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.


Następnie Edytuj `ProductOutlineDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Dzięki temu użytkownikowi na wybranie dowolnego pojedynczego wiersza w widoku konspektu. Zwraca `false` dla `ShouldSelectRow` dla żadnego elementu, który nie ma użytkownik będzie mógł wybrać lub `false` dla każdego elementu, jeśli nie chcesz, aby użytkownik będzie mógł wybrać wszystkie elementy.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Typ do zaznaczenia wiersza

Jeśli chcesz zezwolić użytkownikowi na wpisz znak z widokiem konspektu wybrane i wybierz pierwszy wiersz, który ma tego znaku, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu. Wybierz widok konspektu **hierarchii interfejsów** i sprawdź **wybierz typ** checkbox w **inspektora atrybutu**:

[![](outline-view-images/type01.png "Edytowanie wiersza typu")](outline-view-images/type01.png#lightbox)

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Teraz załóżmy Edytuj `ProductOutlineDelegate.cs` pliku i dodaj następującą metodę:

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

`GetNextTypeSelectMatch` Ma metodę danego `searchString` i zwraca element pierwszy `Product` z tego ciągu w jego `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Ponowne porządkowanie kolumn

Jeśli chcesz zezwolić użytkownikowi na przeciąganie Zmienianie kolejności kolumn w widoku konspektu, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu. Wybierz widok konspektu **hierarchii interfejsów** i sprawdź **Reordering** checkbox w **inspektora atrybutu**:

[![](outline-view-images/reorder01.png "Inspektor atrybutu")](outline-view-images/reorder01.png#lightbox)

Jeśli firma Microsoft daje wartość **Autosave** właściwości i wyboru **informacji o kolumnie** pola, wszystkie zmiany wykonujemy do układu tabeli zostaną automatycznie zapisane firmie Microsoft i przywrócić przy następnym aplikacji jest uruchamiane.

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Teraz załóżmy Edytuj `ProductOutlineDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Metoda powinna zwrócić `true` dla każdej kolumny interesujące zezwolenie na można zmienić kolejności przeciągania do `newColumnIndex`, przeciwnym razie zwracany `false`;

Jeśli możemy uruchomić aplikację, możemy przeciągnij nagłówki kolumn wokół, aby zmienić kolejność kolumn naszych:

[![](outline-view-images/reorder02.png "Przykład opcję kolumn")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Edytowanie komórek

Jeśli chcesz zezwolić użytkownikowi edytowanie wartości danego komórki, Edytuj `ProductOutlineDelegate.cs` plików i zmień `GetViewForItem` metody w następujący sposób:

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

Teraz w możemy uruchomić aplikację, użytkownik może edytować komórki w widoku tabeli:

[![](outline-view-images/editing01.png "Przykład edycji w komórkach")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>Używanie obrazów w widokach konspektu

Aby umieścić obraz jako część komórki w `NSOutlineView`, musisz zmienić, jak dane są zwracane przez wyświetlanie konspektu `NSTableViewDelegate's` `GetView` metodę `NSTableCellView` zamiast typowe `NSTextField`. Na przykład:

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

Aby uzyskać więcej informacji, zobacz [przy użyciu obrazów z widokami konspektu](~/mac/app-fundamentals/image.md) sekcji naszych [Praca z obrazu](~/mac/app-fundamentals/image.md) dokumentacji.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Widoki konspektu powiązania danych

Przy użyciu techniki kluczy i wartości kodowania i powiązania danych w aplikacji Xamarin.Mac, można znacznie zmniejszyć ilość kodu, które trzeba zapisać i zachować do wypełnienia i pracować z nimi elementy interfejsu użytkownika. Masz również zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) od z przodu kończyć interfejsu użytkownika (_Model-View-Controller_), prowadzących do łatwiejsze w obsłudze, bardziej elastyczne aplikacji Projekt.

Kodowanie klucz-wartość (KVC) to mechanizm służący do uzyskiwania dostępu do właściwości obiektu pośrednio, za pomocą kluczy (specjalnie sformatowana ciągi) do identyfikowania właściwości zamiast dostępu do nich za pośrednictwem zmienne wystąpienia lub metody dostępu (`get/set`). Implementując kluczy i wartości kodowania zgodne metod dostępu do aplikacji Xamarin.Mac, uzyskasz dostęp do innych funkcji macOS, takich jak obserwowania klucz-wartość (KVO), wiązanie danych podstawowych danych, Cocoa powiązania i scriptability.

Aby uzyskać więcej informacji, zobacz [powiązania danych widoku konspektu](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) sekcji naszych [powiązania danych i kluczy i wartości kodowania](~/mac/app-fundamentals/databinding.md) dokumentacji.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się praca z widokami konspektu w aplikacji Xamarin.Mac. Firma Microsoft przedstawiono różne typy i używa widoków konspektu, jak tworzyć i obsługiwać konspektu widoków w Konstruktorze interfejsu w środowisku Xcode i sposobu pracy z widokami konspektu w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacOutlines (przykład)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (przykład)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Listy źródeł](~/mac/user-interface/source-list.md)
- [Powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do widoków konspektu](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
