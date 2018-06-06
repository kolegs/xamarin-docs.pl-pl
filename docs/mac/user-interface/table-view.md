---
title: Widoki tabel w Xamarin.Mac
description: Ten artykuł dotyczy pracy z widoków tabel w aplikacji Xamarin.Mac. Opisuje tworzenie widoków tabel w Xcode i kompilatora interfejsu i interakcji z nimi w kodzie.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: da26810869f23b8861ffb4409248c56bff12a521
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793233"
---
# <a name="table-views-in-xamarinmac"></a>Widoki tabel w Xamarin.Mac

_Ten artykuł dotyczy pracy z widoków tabel w aplikacji Xamarin.Mac. Opisuje tworzenie widoków tabel w Xcode i kompilatora interfejsu i interakcji z nimi w kodzie._

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tej samej tabeli widoków używająca *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć w środowisku Xcode _konstruktora interfejsu_ do tworzenia i obsługi widoków tabeli (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Widok tabeli wyświetla dane w formacie tabelarycznym zawierający co najmniej jedną kolumnę informacji w wielu wierszach. Na podstawie typu tworzonego widoku tabeli, użytkownik można sortować według kolumny, uporządkować kolumny, dodawanie kolumn, usunąć kolumny lub edytowanie danych zawartych w tabeli.

[![](table-view-images/intro01.png "Przykładowa tabela")](table-view-images/intro01.png#lightbox)

W tym artykule firma Microsoft będzie kroki te obejmują podstawy Praca z widokami tabeli w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` poleceń używany do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Wprowadzenie do widoków tabel

Widok tabeli wyświetla dane w formacie tabelarycznym zawierający co najmniej jedną kolumnę informacji w wielu wierszach. Widoki tabel są wyświetlane wewnątrz widoków przewijania (`NSScrollView`), a począwszy od macOS 10,7, można użyć dowolnego `NSView` zamiast komórek (`NSCell`) do wyświetlenia wierszy i kolumn. Inaczej mówiąc, można nadal używać `NSCell` , zazwyczaj należy podklasy `NSTableCellView` i tworzenie niestandardowych wierszy i kolumn.

Własne dane przechowywane w widoku tabeli, zamiast tego jest zależne od źródła danych (`NSTableViewDataSource`) wiersze i kolumny są wymagane na zgodnie z potrzebami.

Zachowanie widoku tabeli można dostosować, podając podklasą delegata widok tabeli (`NSTableViewDelegate`) do obsługi zarządzania kolumny tabeli, wpisz, aby wybrać funkcje, zaznaczenie wiersza i edytowanie niestandardowych śledzenia i widoków dla poszczególnych kolumn i Liczba wierszy.

Trwa tworzenie widoków tabel, Apple sugeruje następujące czynności:

* Zezwalaj użytkownikowi na sortować, klikając nagłówki kolumn w tabeli.
* Utwórz nagłówki kolumn, które są rzeczowniki lub frazy rzeczownik krótkich, które opisują dane wyświetlane w tej kolumnie.

Aby uzyskać więcej informacji, zobacz [zawartości widoków](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Tworzenie i obsługę widoków tabel w środowisku Xcode

Podczas tworzenia nowej aplikacji Xamarin.Mac Cocoa, otrzymasz standardowe okno puste, domyślnie. Ten system windows jest zdefiniowany w `.storyboard` pliku automatycznie dołączony do projektu. Aby edytować w projekcie systemu windows **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku:

[![](table-view-images/edit01.png "Wybieranie głównego storyboard")](table-view-images/edit01.png#lightbox)

Spowoduje to otwarcie okna projektu w Konstruktorze interfejsu w środowisku Xcode:

[![](table-view-images/edit02.png "Edytowanie interfejsu użytkownika w środowisku Xcode")](table-view-images/edit02.png#lightbox)

Typ `table` do **Inspector biblioteki** pole wyszukiwania, aby ułatwić znajdowanie kontrolki widoku tabeli:

[![](table-view-images/edit03.png "Wybranie widoku tabeli z biblioteki")](table-view-images/edit03.png#lightbox)

Przeciągnij widoku tabeli kontrolera widoku w **Edytor interfejsu**, stał się wypełnienia obszaru zawartości kontroler widoku i ustaw ją na którym zmniejsza i płynne okna w **edytora ograniczeń**:

[![](table-view-images/edit04.png "Edytowanie ograniczenia")](table-view-images/edit04.png#lightbox)

Wybierz widok tabeli **hierarchii interfejsów** oraz następujące właściwości są dostępne w **inspektora atrybutu**:

[![](table-view-images/edit05.png "Inspektor atrybutu")](table-view-images/edit05.png#lightbox)

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
> Jeśli nie obsługujesz starszych aplikacji Xamarin.Mac `NSView` opartych na widokach tabeli powinien być używany przez `NSCell` na podstawie widoków tabel. `NSCell` uznana za przestarzałą i może nie być obsługiwana idąc dalej.

Wybierz kolumnę tabeli w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **inspektora atrybutu**:

[![](table-view-images/edit06.png "Inspektor atrybutu")](table-view-images/edit06.png#lightbox)

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

[![](table-view-images/edit07.png "Inspektor atrybutu")](table-view-images/edit07.png#lightbox)

Są to wszystkie właściwości standardowy. Istnieje również możliwość zmiany rozmiaru wierszy dla tej kolumny w tym miejscu.

Zaznacz komórkę widoku tabeli (domyślnie jest to `NSTextField`) w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **inspektora atrybutu**:

[![](table-view-images/edit08.png "Inspektor atrybutu")](table-view-images/edit08.png#lightbox)

Będziesz mieć właściwości pola tekstowego standardowych można ustawić tutaj. Domyślnie standardowego pola tekstowego służy do wyświetlania danych dla komórek w kolumnie.

Wybierz widok komórki tabeli (`NSTableFieldCell`) w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **inspektora atrybutu**:

[![](table-view-images/edit09.png "Inspektor atrybutu")](table-view-images/edit09.png#lightbox)

Najważniejsze funkcje są:

- **Układ** — wybierz, w jaki sposób układ komórek w tej kolumnie.
- **Tryb pojedynczy wiersz** — Jeśli `true`, komórka jest ograniczone do jednego wiersza.
- **Pierwszy szerokość układu środowiska uruchomieniowego** — Jeśli `true`, preferowane szerokości ustawionej dla niego (ręcznie lub automatycznie) gdy komórka jest wyświetlany przy pierwszym uruchomieniu aplikacji.
- **Akcja** -sterować Edycja **akcji** jest wysyłane do komórki.
- **Zachowanie** — definiuje, jeśli komórka jest możliwy lub nie można edytować.
- **Tekst sformatowany** — Jeśli `true`, komórki można wyświetlić tekst sformatowany i styl.
- **Cofnij** — Jeśli `true`, komórki przyjmuje na siebie odpowiedzialność zostaje cofnąć zachowanie.

Wybierz widok komórki tabeli (`NSTableFieldCell`) u dołu w kolumnie tabeli **hierarchii interfejsów**:

[![](table-view-images/edit10.png "Wybranie widoku komórki tabeli")](table-view-images/edit10.png#lightbox)

Dzięki temu można edytować komórki tabeli widoku używany jako bazowy _wzorzec_ we wszystkich komórkach utworzone dla podanej kolumny.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Dodawanie działań i gniazda

Tylko innych formantu interfejsu użytkownika Cocoa, musimy udostępnić nasze widoku tabeli i kolumny, a aby C# kodu za pomocą **akcje** i **gniazda** (oparta na funkcjach wymaganych).

Proces jest taka sama dla każdego elementu widoku tabeli, która ma zostać udostępniona:

1. Przełącz się do **Edytor Asystenta** i upewnij się, że `ViewController.h` wybrany plik: 

    [![](table-view-images/edit11.png "Edytor Asystenta")](table-view-images/edit11.png#lightbox)
2. Wybierz widok tabeli z **hierarchii interfejsów**, przytrzymując klawisz CTRL i przeciągnij, aby `ViewController.h` pliku.
3. Utwórz **gniazda** dla widoku tabeli o nazwie `ProductTable`: 

    [![](table-view-images/edit13.png "Konfigurowanie gniazda")](table-view-images/edit13.png#lightbox)
4. Utwórz **gniazda** w kolumnach tabel o nazwie `ProductColumn` i `DetailsColumn`: 

    [![](table-view-images/edit14.png "Konfigurowanie gniazda")](table-view-images/edit14.png#lightbox)
5. Możesz zapisać i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Następnie firma Microsoft będzie zapisu wyświetlania kodu niektórych danych z tabeli po uruchomieniu aplikacji.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Wypełnianie tabeli widoku

Z naszych widoku tabeli zaprojektowane w Konstruktorze interfejsu i udostępniane za pośrednictwem funkcji **gniazda**, następnie należy utworzyć kodu C#, aby wypełnić go.

Najpierw utwórz nową `Product` klasy do przechowywania informacji w poszczególnych wierszach. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `Product` dla **nazwa** i kliknij przycisk **nowy** przycisk:

[![](table-view-images/populate01.png "Tworzenie klasy pusty")](table-view-images/populate01.png#lightbox)

Wprowadź `Product.cs` wygląd pliku podobne do poniższych:

```csharp
using System;

namespace MacTables
{
    public class Product
    {
        #region Computed Propoperties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
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

Następnie należy utworzyć podklasy `NSTableDataSource` do dostarczania danych do naszej tabeli jako żądanie. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `ProductTableDataSource` dla **nazwa** i kliknij przycisk **nowy** przycisku.

Edytuj `ProductTableDataSource.cs` pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDataSource : NSTableViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Products.Count;
        }
        #endregion
    }
}

```

Ta klasa ma magazyn dla elementów naszych widoku tabeli i zastępuje `GetRowCount` do zwrócenia liczbę wierszy w tabeli.

Na koniec należy utworzyć podklasy `NSTableDelegate` przewidzieć zachowania naszych tabeli. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `ProductTableDelegate` dla **nazwa** i kliknij przycisk **nowy** przycisku.

Edytuj `ProductTableDelegate.cs` pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDelegate: NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductTableDataSource DataSource;
        #endregion

        #region Constructors
        public ProductTableDelegate (ProductTableDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = DataSource.Products [(int)row].Title;
                break;
            case "Details":
                view.StringValue = DataSource.Products [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Kiedy możemy utworzyć wystąpienia `ProductTableDelegate`, będziemy również przekazać wystąpienia `ProductTableDataSource` udostępniająca danych dla tabeli. `GetViewForItem` Metoda jest odpowiedzialna za zwracanie widoku (dane), aby wyświetlić komórki dla podać kolumn i wierszy. Jeśli to możliwe istniejącego widoku zostanie ono użyte ponownie, aby wyświetlić komórkę, jeśli nie należy utworzyć nowy widok.

Aby wypełnić tabeli, umożliwia edytowanie `ViewController.cs` plików i upewnij `AwakeFromNib` wygląd metody podobne do poniższych:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);
}
```

Czy możemy uruchomić aplikację, wyświetlane są następujące:

[![](table-view-images/populate02.png "Uruchom przykładową aplikację")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Sortowanie według kolumny

Teraz można zezwolić użytkownikowi na posortuj dane w tabeli, klikając nagłówek kolumny. Po pierwsze, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu. Wybierz `Product` kolumny, wprowadź `Title` dla **klucza sortowania**, `compare:` dla **selektora** i wybierz `Ascending` dla **kolejności**:

[![](table-view-images/sort01.png "Ustawienie klucza sortowania")](table-view-images/sort01.png#lightbox)

Wybierz `Details` kolumny, wprowadź `Description` dla **klucza sortowania**, `compare:` dla **selektora** i wybierz `Ascending` dla **kolejności**:

[![](table-view-images/sort02.png "Ustawienie klucza sortowania")](table-view-images/sort02.png#lightbox)

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Teraz załóżmy Edytuj `ProductTableDataSource.cs` pliku i dodaj następujące metody:

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
    case "Description":
        if (ascending) {
            Products.Sort ((x, y) => x.Description.CompareTo (y.Description));
        } else {
            Products.Sort ((x, y) => -1 * x.Description.CompareTo (y.Description));
        }
        break;
    }

}

public override void SortDescriptorsChanged (NSTableView tableView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    if (oldDescriptors.Length > 0) {
        // Update sort
        Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    } else {
        // Grab current descriptors and update sort
        NSSortDescriptor[] tbSort = tableView.SortDescriptors; 
        Sort (tbSort[0].Key, tbSort[0].Ascending); 
    }
            
    // Refresh table
    tableView.ReloadData ();
}
```

`Sort` Metody umożliwiają sortowanie danych w źródle danych, na podstawie danego `Product` pola klasy w porządku rosnącym lub malejącym. Zastąpione `SortDescriptorsChanged` metoda zostanie wywołana za każdym razem, gdy użycie kliknie nagłówek kolumny. Zostaną przekazane **klucza** wartość ustawioną w konstruktora interfejsu i kolejność sortowania dla tej kolumny.

Jeśli firma Microsoft może uruchomić aplikację, a następnie kliknij nagłówek kolumny, wiersze zostaną posortowane według tej kolumny:

[![](table-view-images/sort03.png "Uruchom przykładową aplikację")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Zaznaczenie wiersza

Jeśli chcesz zezwolić użytkownikowi wybierz pojedynczy wiersz, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu. Wybierz widok tabeli **hierarchii interfejsów** i usuń zaznaczenie pola wyboru **wiele** checkbox w **inspektora atrybutu**:

[![](table-view-images/select01.png "Inspektor atrybutu")](table-view-images/select01.png#lightbox)

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.


Następnie Edytuj `ProductTableDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Dzięki temu użytkownikowi na wybranie dowolnego pojedynczego wiersza w widoku tabeli. Zwraca `false` dla `ShouldSelectRow` dla żadnego wiersza, który nie ma użytkownik będzie mógł wybrać lub `false` dla każdego wiersza, jeśli nie chcesz, aby użytkownik będzie mógł wybrać wszystkie wiersze.

Widok tabeli (`NSTableView`) zawiera następujące metody do pracy z zaznaczenie wiersza:

- `DeselectRow(nint)` -Odznacza danego wiersza w tabeli.
- `SelectRow(nint,bool)` -Wybiera danego wiersza. Przekaż `false` dla drugiego parametru wybrać tylko jeden wiersz jednocześnie.
- `SelectedRow` -Zwraca bieżący wiersz zaznaczony w tabeli.
- `IsRowSelected(nint)` -Zwraca `true` zaznaczenie danego wiersza.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Wybieranie wielu wierszy

Jeśli chcesz zezwolić użytkownikowi zaznaczyć wiele wierszy, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu. Wybierz widok tabeli **hierarchii interfejsów** i sprawdź **wiele** checkbox w **inspektora atrybutu**:

[![](table-view-images/select02.png "Inspektor atrybutu")](table-view-images/select02.png#lightbox)

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.


Następnie Edytuj `ProductTableDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Dzięki temu użytkownikowi na wybranie dowolnego pojedynczego wiersza w widoku tabeli. Zwraca `false` dla `ShouldSelectRow` dla żadnego wiersza, który nie ma użytkownik będzie mógł wybrać lub `false` dla każdego wiersza, jeśli nie chcesz, aby użytkownik będzie mógł wybrać wszystkie wiersze.

Widok tabeli (`NSTableView`) zawiera następujące metody do pracy z zaznaczenie wiersza:

- `DeselectAll(NSObject)` -Odznacza wszystkie wiersze w tabeli. Użyj `this` jako pierwszy parametr do wysłania w obiekcie podczas wyboru. 
- `DeselectRow(nint)` -Odznacza danego wiersza w tabeli.
- `SelectAll(NSobject)` -Wybiera wszystkie wiersze w tabeli. Użyj `this` jako pierwszy parametr do wysłania w obiekcie podczas wyboru.
- `SelectRow(nint,bool)` -Wybiera danego wiersza. Przekaż `false` dla drugiego parametru wyczyść zaznaczenie i wybierz tylko jeden wiersz, Przekaż `true` rozszerzyć zaznaczenie i dołączenie tego wiersza.
- `SelectRows(NSIndexSet,bool)` -Wybiera danego zestawu wierszy. Przekaż `false` dla drugiego parametru wyczyść zaznaczenie, a następnie wybierz tylko tych wierszy, Przekaż `true` rozszerzyć zaznaczenie i Dołącz te wiersze.
- `SelectedRow` -Zwraca bieżący wiersz zaznaczony w tabeli.
- `SelectedRows` -Zwraca `NSIndexSet` zawierający indeksy zaznaczone wiersze.
- `SelectedRowCount` -Zwraca liczbę wybranych wierszy.
- `IsRowSelected(nint)` -Zwraca `true` zaznaczenie danego wiersza.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Typ do zaznaczenia wiersza

Jeśli chcesz zezwolić użytkownikowi na wpisz znak z widoku tabeli wybrane i wybierz pierwszy wiersz, który ma tego znaku, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu. Wybierz widok tabeli **hierarchii interfejsów** i sprawdź **wybierz typ** checkbox w **inspektora atrybutu**:

[![](table-view-images/type01.png "Ustawienie typu zaznaczenia")](table-view-images/type01.png#lightbox)

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Teraz załóżmy Edytuj `ProductTableDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override nint GetNextTypeSelectMatch (NSTableView tableView, nint startRow, nint endRow, string searchString)
{
    nint row = 0;
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains(searchString)) return row;

        // Increment row counter
        ++row;
    }

    // If not found select the first row
    return 0;
}
```

`GetNextTypeSelectMatch` Ma metodę danego `searchString` i zwraca wiersz pierwszy `Product` z tego ciągu w jego `Title`.

Jeśli firma Microsoft może uruchomić aplikację, a następnie wpisz znak, zostanie wybrany wiersz:

[![](table-view-images/type02.png "Uruchom przykładową aplikację")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Ponowne porządkowanie kolumn

Jeśli chcesz zezwolić użytkownikowi na przeciąganie Zmienianie kolejności kolumn w widoku tabeli, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu. Wybierz widok tabeli **hierarchii interfejsów** i sprawdź **Reordering** checkbox w **inspektora atrybutu**:

[![](table-view-images/reorder01.png "Inspektor atrybutu")](table-view-images/reorder01.png#lightbox)

Jeśli firma Microsoft daje wartość **Autosave** właściwości i wyboru **informacji o kolumnie** pola, wszystkie zmiany wykonujemy do układu tabeli zostaną automatycznie zapisane firmie Microsoft i przywrócić przy następnym aplikacji jest uruchamiane.

Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Teraz załóżmy Edytuj `ProductTableDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Metoda powinna zwrócić `true` dla każdej kolumny interesujące zezwolenie na można zmienić kolejności przeciągania do `newColumnIndex`, przeciwnym razie zwracany `false`;

Jeśli możemy uruchomić aplikację, możemy przeciągnij nagłówki kolumn wokół, aby zmienić kolejność kolumn naszych:

[![](table-view-images/reorder02.png "Przykład kolejnos'ci kolumn")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Edytowanie komórek

Jeśli chcesz zezwolić użytkownikowi edytowanie wartości danego komórki, Edytuj `ProductTableDelegate.cs` plików i zmień `GetViewForItem` metody w następujący sposób:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{
    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = true;

        view.EditingEnded += (sender, e) => {
                    
            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.Tag].Title = view.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.Tag].Description = view.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Teraz w możemy uruchomić aplikację, użytkownik może edytować komórki w widoku tabeli:

[![](table-view-images/editing01.png "Przykład edytowania komórki")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>Używanie obrazów w widoku tabeli

Aby uwzględnić obrazu jako część komórki w `NSTableView`, musisz zmienić, jak dane są zwracane przez widok tabeli `NSTableViewDelegate's` `GetViewForItem` metodę `NSTableCellView` zamiast typowe `NSTextField`. Na przykład:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
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
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Aby uzyskać więcej informacji, zobacz [przy użyciu obrazów za pomocą widoków tabel](~/mac/app-fundamentals/image.md) sekcji naszych [Praca z obrazu](~/mac/app-fundamentals/image.md) dokumentacji.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Dodawanie przycisku Usuń na wiersz

Na podstawie wymagań aplikacji, może być zmieniana gdzie należy podać przycisku akcji dla każdego wiersza w tabeli. Na przykład, załóżmy rozwiń przykład widoku tabeli utworzone powyżej, aby uwzględnić **usunąć** przycisk w każdym wierszu.

Po pierwsze, Edytuj `Main.storyboard` w Konstruktorze interfejsu w programie Xcode, wybierz widok tabeli i zwiększyć liczbę kolumn do trzech (3). Następnie należy zmienić **tytuł** nowej kolumny do `Action`:

[![](table-view-images/delete01.png "Edytowanie nazwy kolumny")](table-view-images/delete01.png#lightbox)

Zapisz zmiany do scenorysu i wróć do programu Visual Studio for Mac zsynchronizować zmiany.

Następnie Edytuj `ViewController.cs` pliku i dodaj następującą metodę publicznego:

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

W tym samym pliku, zmodyfikuj tworzenia nowego obiektu delegowanego widok tabeli wewnątrz `ViewDidLoad` metody w następujący sposób:

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

Teraz, Edytuj `ProductTableDelegate.cs` plików obejmują prywatnego połączenia kontrolera widoku i podejmowania kontrolera jako parametru podczas tworzenia nowego wystąpienia delegata:

```csharp
#region Private Variables
private ProductTableDataSource DataSource;
private ViewController Controller;
#endregion

#region Constructors
public ProductTableDelegate (ViewController controller, ProductTableDataSource datasource)
{
    this.Controller = controller;
    this.DataSource = datasource;
}
#endregion
```

Następnie dodaj poniższe nowej metody prywatnej do klasy:

```csharp
private void ConfigureTextField (NSTableCellView view, nint row)
{
    // Add to view
    view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
    view.AddSubview (view.TextField);

    // Configure
    view.TextField.BackgroundColor = NSColor.Clear;
    view.TextField.Bordered = false;
    view.TextField.Selectable = false;
    view.TextField.Editable = true;

    // Wireup events
    view.TextField.EditingEnded += (sender, e) => {

        // Take action based on type
        switch (view.Identifier) {
        case "Product":
            DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
            break;
        case "Details":
            DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
            break;
        }
    };

    // Tag view
    view.TextField.Tag = row;
}
```

Trwa to wszystkie konfiguracje widoku tekstu, które zostały wcześniej wykonywana w `GetViewForItem` — metoda i umieszcza je w jednej, który można wywołać lokalizacji (od momentu ostatniej kolumny w tabeli nie zawiera widoku tekstu, ale przycisku).

Na koniec Edytuj `GetViewForItem` — metoda i zapewnić ich wyglądać następująco:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();

        // Configure the view
        view.Identifier = tableColumn.Title;

        // Take action based on title
        switch (tableColumn.Title) {
        case "Product":
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Details":
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Action":
            // Create new button
            var button = new NSButton (new CGRect (0, 0, 81, 16));
            button.SetButtonType (NSButtonType.MomentaryPushIn);
            button.Title = "Delete";
            button.Tag = row;

            // Wireup events
            button.Activated += (sender, e) => {
                // Get button and product
                var btw = sender as NSButton;
                var product = DataSource.Products [(int)btn.Tag];

                // Configure alert
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
                    MessageText = $"Delete {product.Title}?",
                };
                alert.AddButton ("Cancel");
                alert.AddButton ("Delete");
                alert.BeginSheetForResponse (Controller.View.Window, (result) => {
                    // Should we delete the requested row?
                    if (result == 1001) {
                        // Remove the given row from the dataset
                        DataSource.Products.RemoveAt((int)btn.Tag);
                        Controller.ReloadTable ();
                    }
                });
            };

            // Add to view
            view.AddSubview (button);
            break;
        }

    }

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tag.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        view.TextField.Tag = row;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        view.TextField.Tag = row;
        break;
    case "Action":
        foreach (NSView subview in view.Subviews) {
            var btw = subview as NSButton;
            if (btw != null) {
                btn.Tag = row;
            }
        }
        break;
    }

    return view;
}
```

Oto kilka fragmentów kodu, tym bardziej szczegółowo. Pierwsza strona, jeśli jest to nowy `NSTableViewCell` jest tworzony akcji pochodzi oparte na nazwę kolumny. Dla dwóch pierwszych kolumn (**produktu** i **szczegóły**), nowy `ConfigureTextField` metoda jest wywoływana.

Dla **akcji** kolumny, nowy `NSButton` jest tworzony i dodawany do komórki jako widok podrzędny:

```csharp
// Create new button
var button = new NSButton (new CGRect (0, 0, 81, 16));
button.SetButtonType (NSButtonType.MomentaryPushIn);
button.Title = "Delete";
button.Tag = row;
...

// Add to view
view.AddSubview (button);
```

Przycisk `Tag` właściwość jest używana do przechowywania numer wiersza, który jest obecnie przetwarzane. Ta liczba będzie można użyć później, gdy użytkownik zażąda wiersz usunięty z przycisku `Activated` zdarzeń:

```csharp
// Wireup events
button.Activated += (sender, e) => {
    // Get button and product
    var btw = sender as NSButton;
    var product = DataSource.Products [(int)btn.Tag];

    // Configure alert
    var alert = new NSAlert () {
        AlertStyle = NSAlertStyle.Informational,
        InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
        MessageText = $"Delete {product.Title}?",
    };
    alert.AddButton ("Cancel");
    alert.AddButton ("Delete");
    alert.BeginSheetForResponse (Controller.View.Window, (result) => {
        // Should we delete the requested row?
        if (result == 1001) {
            // Remove the given row from the dataset
            DataSource.Products.RemoveAt((int)btn.Tag);
            Controller.ReloadTable ();
        }
    });
};
```

Na początku programu obsługi zdarzeń uzyskujemy przycisku i produktu, który znajduje się w wierszu danej tabeli. Alert jest przedstawiony w usłudze użytkownika potwierdzenie usunięcia wiersza. Jeśli użytkownik wybierze można usunąć wiersza, danego wiersza zostanie usunięty ze źródła danych i załadowaniu tabeli:

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Ponadto jeśli komórkę tabeli w widoku jest ponownie używany zamiast tworzenia nowego, następujący kod konfiguruje go na podstawie kolumny przetwarzanych:

```csharp
// Setup view based on the column selected
switch (tableColumn.Title) {
case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tag.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    view.TextField.Tag = row;
    break;
case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    view.TextField.Tag = row;
    break;
case "Action":
    foreach (NSView subview in view.Subviews) {
        var btw = subview as NSButton;
        if (btw != null) {
            btn.Tag = row;
        }
    }
    break;
}

```

Dla **akcji** kolumny, dopóki są skanowane wszystkie widoki Sub `NSButton` zostanie znaleziony, a następnie jest `Tag` właściwość zostało zaktualizowane do punktu w bieżącym wierszu.

Z tych zmian w miejsce, gdy aplikacja jest uruchamiana każdy wiersz będzie mieć **usunąć** przycisk:

[![](table-view-images/delete02.png "Widok tabeli usuwanie przycisków")](table-view-images/delete02.png#lightbox)

Po kliknięciu przez użytkownika **usunąć** przycisku alertu pojawi się monitem o usunąć podanego wiersza:

[![](table-view-images/delete03.png "W przypadku wystąpienia alertu usuwania wiersza")](table-view-images/delete03.png#lightbox)

Jeśli użytkownik wybierze delete, wiersz zostaną usunięte i zostanie narysowany ponownie tabeli:

[![](table-view-images/delete04.png "Po usunięciu wiersza tabeli")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Widoki tabeli powiązania danych

Przy użyciu techniki kluczy i wartości kodowania i powiązania danych w aplikacji Xamarin.Mac, można znacznie zmniejszyć ilość kodu, które trzeba zapisać i zachować do wypełnienia i pracować z nimi elementy interfejsu użytkownika. Masz również zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) od z przodu kończyć interfejsu użytkownika (_Model-View-Controller_), prowadzących do łatwiejsze w obsłudze, bardziej elastyczne aplikacji Projekt.

Kodowanie klucz-wartość (KVC) to mechanizm służący do uzyskiwania dostępu do właściwości obiektu pośrednio, za pomocą kluczy (specjalnie sformatowana ciągi) do identyfikowania właściwości zamiast dostępu do nich za pośrednictwem zmienne wystąpienia lub metody dostępu (`get/set`). Implementując kluczy i wartości kodowania zgodne metod dostępu do aplikacji Xamarin.Mac, uzyskasz dostęp do innych funkcji macOS, takich jak obserwowania klucz-wartość (KVO), wiązanie danych podstawowych danych, Cocoa powiązania i scriptability.

Aby uzyskać więcej informacji, zobacz [powiązania danych widoku tabeli](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) sekcji naszych [powiązania danych i kluczy i wartości kodowania](~/mac/app-fundamentals/databinding.md) dokumentacji.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się praca z widokami tabeli w aplikacji Xamarin.Mac. Firma Microsoft przedstawiono różne typy i używa widoków tabel, jak tworzyć i obsługiwać widoków tabel w Konstruktorze interfejsu w środowisku Xcode i sposobu pracy z widoków tabel w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacTables (przykład)](https://developer.xamarin.com/samples/mac/MacTables/)
- [MacImages (przykład)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Widoki konspektu](~/mac/user-interface/outline-view.md)
- [Listy źródeł](~/mac/user-interface/source-list.md)
- [Powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [NSTableView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [NSTableViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
