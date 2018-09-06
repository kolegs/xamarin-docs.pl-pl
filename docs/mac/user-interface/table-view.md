---
title: Widoki tabel na platformie Xamarin.Mac
description: Ten artykuł dotyczy pracy z widoki tabel w aplikacji platformy Xamarin.Mac. Opisuje tworzenie widoków tabel w środowisku Xcode i programu Interface Builder i interakcji z nimi w kodzie.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 68b52fb4b7a3a65b45fcbecdc865bc64d9865fd9
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780639"
---
# <a name="table-views-in-xamarinmac"></a>Widoki tabel na platformie Xamarin.Mac

_Ten artykuł dotyczy pracy z widoki tabel w aplikacji platformy Xamarin.Mac. Opisuje tworzenie widoków tabel w środowisku Xcode i programu Interface Builder i interakcji z nimi w kodzie._

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tej samej tabeli widoków deweloperem, pracy w *języka Objective-C* i *Xcode* jest. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, można użyć program Xcode _programu Interface Builder_ Aby utworzyć i obsługiwać swoje widoki tabel (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Widok tabeli wyświetla dane w formacie tabelarycznym, zawierający co najmniej jedną kolumnę informacji w wielu wierszach. Na podstawie typu widoku tabeli tworzony, użytkownika można sortować według kolumny, reorganizowanie kolumn, dodawanie kolumn, Usuń kolumny lub edytować danych zawartych w tabeli.

[![](table-view-images/intro01.png "Przykład tabeli")](table-view-images/intro01.png#lightbox)

W tym artykule omówiono podstawowe informacje dotyczące pracy z widokami tabeli w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` poleceń Umożliwia o komunikacji sieciowej w górę klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Wprowadzenie do widoku tabeli

Widok tabeli wyświetla dane w formacie tabelarycznym, zawierający co najmniej jedną kolumnę informacji w wielu wierszach. Widoki tabel są wyświetlane w widokach przewijania (`NSScrollView`) i począwszy od systemu macOS 10.7, możesz użyć dowolnej `NSView` zamiast komórek (`NSCell`) do wyświetlenia, wierszy i kolumn. Inaczej mówiąc, możesz nadal używać `NSCell` jednak będzie najczęściej podklasy `NSTableCellView` i utworzyć niestandardowe wierszy i kolumn.

Widok tabeli nie przechowuje swoich danych, zamiast tego opiera się na źródle danych (`NSTableViewDataSource`) wiersze i kolumny są wymagane na zgodnie z potrzebami.

Zachowanie widoku tabeli, można dostosować, podając podklasę delegata widok tabeli (`NSTableViewDelegate`) aby obsługiwać Zarządzanie kolumnami tabeli, wpisz wybierz funkcje, wybór wiersza i edytowania, niestandardowe śledzenia i widoków dla poszczególnych kolumn i Liczba wierszy.

Tworząc widoki tabel, Apple sugeruje następujące czynności:

* Umożliwia użytkownikowi sortowanie tabeli, klikając nagłówki kolumn.
* Utwórz nagłówków kolumn, które są rzeczowniki lub frazy rzeczownik krótki, które opisują dane są wyświetlane w tej kolumnie.

Aby uzyskać więcej informacji, zobacz [widoki zawartości](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Utworzenie i utrzymywanie widoki tabel w środowisku Xcode

Kiedy tworzysz nową aplikację cocoa dla platformy Xamarin.Mac, otrzymasz standardowego okna puste, domyślnie. Ten system windows jest zdefiniowany w `.storyboard` pliku automatycznie uwzględnione w projekcie. Aby edytować projektu systemu windows w **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku:

[![](table-view-images/edit01.png "Wybieranie głównego storyboard")](table-view-images/edit01.png#lightbox)

Spowoduje to otwarcie projektu okna w program Xcode Interface Builder:

[![](table-view-images/edit02.png "Edytowanie interfejsu użytkownika w środowisku Xcode")](table-view-images/edit02.png#lightbox)

Typ `table` do **Inspektor biblioteki** pola wyszukiwania, aby ułatwić znajdowanie kontrolki widok tabeli:

[![](table-view-images/edit03.png "Wybierając widok tabeli z biblioteki")](table-view-images/edit03.png#lightbox)

Przeciągnij widok tabeli na kontroler widoku w **Edytor interfejsu**, wypełnienia obszaru zawartości kontrolera widoku i ustaw ją na którym zmniejsza i rośnie wraz z okna **edytora ograniczeń**:

[![](table-view-images/edit04.png "Edytowanie ograniczenia")](table-view-images/edit04.png#lightbox)

Wybierz widok tabeli **hierarchii interfejsów** oraz następujące właściwości są dostępne w **Inspektor atrybut**:

[![](table-view-images/edit05.png "Inspektor atrybutu")](table-view-images/edit05.png#lightbox)

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
> Jeśli nie obsługujesz starszą aplikację platformy Xamarin.Mac, `NSView` oparte na widoki tabel powinny być używane za pośrednictwem `NSCell` na podstawie widoki tabel. `NSCell` jest uznawany za starszej wersji i może nie być obsługiwana przyszłości.

Wybierz kolumnę tabeli w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **Inspektor atrybut**:

[![](table-view-images/edit06.png "Inspektor atrybutu")](table-view-images/edit06.png#lightbox)

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

[![](table-view-images/edit07.png "Inspektor atrybutu")](table-view-images/edit07.png#lightbox)

Są to wszystkie właściwości standardowy widok. Istnieje również możliwość zmiany rozmiaru wierszy dla tej kolumny w tym miejscu.

Komórka widoku tabeli (domyślnie jest to `NSTextField`) w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **Inspektor atrybut**:

[![](table-view-images/edit08.png "Inspektor atrybutu")](table-view-images/edit08.png#lightbox)

Będziesz mieć wszystkie właściwości, pola tekstu standardowego, aby ustawić w tym miejscu. Domyślnie standardowego pola tekstowego służy do wyświetlania danych dla komórek w kolumnie.

Wybierz widok komórki tabeli (`NSTableFieldCell`) w **hierarchii interfejsów** oraz następujące właściwości są dostępne w **Inspektor atrybut**:

[![](table-view-images/edit09.png "Inspektor atrybutu")](table-view-images/edit09.png#lightbox)

Najważniejsze funkcje to:

- **Układ** — wybierz, w jaki sposób są ułożone komórek w tej kolumnie.
- **Używa pojedynczego wiersza trybu** — Jeśli `true`, komórki jest ograniczona do jednego wiersza.
- **Pierwszy szerokość układu środowiska uruchomieniowego** — Jeśli `true`, komórki zostanie Preferuj szerokości, ustaw dla niego (ręcznie lub automatycznie) kiedy jest wyświetlana przy pierwszym uruchomieniu aplikacji.
- **Akcja** -Określa, kiedy edycji **akcji** są wysyłane do komórki.
- **Zachowanie** — Określa, czy zawartość komórki jest możliwy lub nie można edytować.
- **Tekst sformatowany** — Jeśli `true`, komórki wyświetlić tekst sformatowany i stylem.
- **Cofnij** — Jeśli `true`, komórki przyjmuje, że jest ona Cofnij zachowanie.

Wybierz widok komórki tabeli (`NSTableFieldCell`) w dolnej części kolumny tabeli w **hierarchii interfejsów**:

[![](table-view-images/edit10.png "Wybierając widok komórki tabeli")](table-view-images/edit10.png#lightbox)

Umożliwia to edytowanie widoku komórki tabeli, zostanie użyty jako podstawa _wzorzec_ dla wszystkich komórek utworzone dla danej kolumny.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Dodawanie akcji i gniazd

Po prostu jak inne kontrolki interfejsu użytkownika Cocoa, należy udostępnić nasze widok tabeli i jest kolumn i komórki do języka C# kodu za pomocą **akcje** i **gniazd** (oparte na funkcje wymagane).

Proces jest taka sama dla każdego elementu w widoku tabeli, którą chcemy udostępnić:

1. Przełącz się do **edytora Asystenta** i upewnij się, że `ViewController.h` wybrany plik: 

    [![](table-view-images/edit11.png "Edytor Asystenta ustawień")](table-view-images/edit11.png#lightbox)
2. Wybierz widok tabeli z **hierarchii interfejsów**, przytrzymując klawisz CTRL i przeciągnij, aby `ViewController.h` pliku.
3. Tworzenie **ujścia** dla widoku tabeli o nazwie `ProductTable`: 

    [![](table-view-images/edit13.png "Konfigurowanie źródła")](table-view-images/edit13.png#lightbox)
4. Tworzenie **gniazd** w kolumnach tabeli o nazwie `ProductColumn` i `DetailsColumn`: 

    [![](table-view-images/edit14.png "Konfigurowanie źródła")](table-view-images/edit14.png#lightbox)
5. Zapisać zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Następnie będziemy pisać wyświetlania kodu niektóre dane w tabeli po uruchomieniu aplikacji.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Wypełnianie widok tabeli

Za pomocą naszych widok tabeli zaprojektowanych w programu Interface Builder i udostępniane za pośrednictwem **ujścia**, następnie należy utworzyć kod C#, aby je wypełnić.

Po pierwsze Utwórzmy nowy `Product` klasy do przechowywania informacji w poszczególnych wierszach. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `Product` dla **nazwa** i kliknij przycisk **New** przycisku:

[![](table-view-images/populate01.png "Tworzenie pustą klasę")](table-view-images/populate01.png#lightbox)

Wprowadź `Product.cs` wygląd pliku, jak pokazano poniżej:

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

Następnie należy utworzyć podklasę klasy `NSTableDataSource` do dostarczania danych do tabeli, ponieważ żądanie. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `ProductTableDataSource` dla **nazwa** i kliknij przycisk **New** przycisku.

Edytuj `ProductTableDataSource.cs` plików i przypisz ją wyglądać podobnie do następującego:

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

Ta klasa magazyn dla elementów widoku naszej tabeli i zastępuje `GetRowCount` aby zwrócić liczbę wierszy w tabeli.

Na koniec należy utworzyć podklasę klasy `NSTableDelegate` zapewnienie zachowanie tabeli. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **Nowy plik ...**  Wybierz **ogólne** > **pustą klasę**, wprowadź `ProductTableDelegate` dla **nazwa** i kliknij przycisk **New** przycisku.

Edytuj `ProductTableDelegate.cs` plików i przypisz ją wyglądać podobnie do następującego:

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

Kiedy tworzymy wystąpienie `ProductTableDelegate`, możemy również przekazać w wystąpieniu `ProductTableDataSource` zapewniający danych dla tabeli. `GetViewForItem` Metoda jest odpowiedzialna za zwrócenie widoku (dane), aby wyświetlić komórki, które zapewniają kolumn i wierszy. Jeśli to możliwe istniejącemu widokowi zostanie ono użyte ponownie, aby wyświetlić komórkę, jeśli nie należy utworzyć nowy widok.

Aby wypełnić tabelę, umożliwia edytowanie `ViewController.cs` pliku i upewnij `AwakeFromNib` wygląd metoda podobne do następującego:

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

Jeśli możemy uruchomić aplikację, wyświetlane są następujące:

[![](table-view-images/populate02.png "Uruchom przykładową aplikację")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Sortowanie według kolumny

Zezwolimy na użytkownika posortować dane w tabeli, klikając nagłówek kolumny. Po pierwsze, kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w programu Interface Builder. Wybierz `Product` kolumny, wprowadź `Title` dla **klucz sortowania**, `compare:` dla **selektor** i wybierz `Ascending` dla **kolejności**:

[![](table-view-images/sort01.png "Ustawienie klucza sortowania")](table-view-images/sort01.png#lightbox)

Wybierz `Details` kolumny, wprowadź `Description` dla **klucz sortowania**, `compare:` dla **selektor** i wybierz `Ascending` dla **kolejności**:

[![](table-view-images/sort02.png "Ustawienie klucza sortowania")](table-view-images/sort02.png#lightbox)

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Teraz możemy edytować `ProductTableDataSource.cs` pliku i dodaj następujące metody:

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

`Sort` Metody umożliwiają sortowanie danych w źródle danych, na podstawie danego `Product` pola klasy w porządku rosnącym lub malejącym. Zastąpione `SortDescriptorsChanged` metoda zostanie wywołana, za każdym razem, gdy użycie kliknięciu nagłówka kolumny. Zostaną przekazane **klucz** wartość, która jest ustawiany w programu Interface Builder i porządek sortowania dla tej kolumny.

Jeśli firma Microsoft może uruchomić aplikację, a następnie kliknij nagłówek kolumny, wiersze będą sortowane według tej kolumny:

[![](table-view-images/sort03.png "Uruchamianie przykładowej aplikacji")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Zaznaczenie wiersza

Jeśli chcesz umożliwić użytkownikowi wybrać pojedynczy wiersz, kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w programu Interface Builder. Wybierz widok tabeli **hierarchii interfejsów** i usuń zaznaczenie pola wyboru **wielu** pola wyboru w **Inspektor atrybut**:

[![](table-view-images/select01.png "Inspektor atrybutu")](table-view-images/select01.png#lightbox)

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.


Następnie Edytuj `ProductTableDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Umożliwi to użytkownikowi na wybranie dowolnego pojedynczego wiersza w widoku tabeli. Zwróć `false` dla `ShouldSelectRow` dla żadnego wiersza, który nie chcesz, użytkownik będzie mógł wybrać lub `false` dla każdego wiersza, jeśli nie chcesz, aby użytkownik będzie mógł wybrać żadnych wierszy.

Widok tabeli (`NSTableView`) zawiera następujące metody do pracy z wybór wiersza:

- `DeselectRow(nint)` -Usuwa zaznaczenie danego wiersza w tabeli.
- `SelectRow(nint,bool)` -Wybiera w danym wierszu. Przekaż `false` jako drugi parametr wybrać tylko jeden wiersz w danym momencie.
- `SelectedRow` -Zwraca bieżący wiersz, który wybrano w tabeli.
- `IsRowSelected(nint)` -Zwraca `true` zaznaczenie danego wiersza.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Wybór wielokrotny wiersza

Jeśli chcesz zezwolić na użytkownika wybrać wiele wierszy, kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w programu Interface Builder. Wybierz widok tabeli **hierarchii interfejsów** i sprawdź **wielu** pola wyboru w **Inspektor atrybut**:

[![](table-view-images/select02.png "Inspektor atrybutu")](table-view-images/select02.png#lightbox)

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.


Następnie Edytuj `ProductTableDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Umożliwi to użytkownikowi na wybranie dowolnego pojedynczego wiersza w widoku tabeli. Zwróć `false` dla `ShouldSelectRow` dla żadnego wiersza, który nie chcesz, użytkownik będzie mógł wybrać lub `false` dla każdego wiersza, jeśli nie chcesz, aby użytkownik będzie mógł wybrać żadnych wierszy.

Widok tabeli (`NSTableView`) zawiera następujące metody do pracy z wybór wiersza:

- `DeselectAll(NSObject)` -Odznacza wszystkie wiersze w tabeli. Użyj `this` jako pierwszy parametr wysłać w obiekcie, wykonując, wybierając. 
- `DeselectRow(nint)` -Usuwa zaznaczenie danego wiersza w tabeli.
- `SelectAll(NSobject)` -Wybiera wszystkie wiersze w tabeli. Użyj `this` jako pierwszy parametr wysłać w obiekcie, wykonując, wybierając.
- `SelectRow(nint,bool)` -Wybiera w danym wierszu. Przekaż `false` dla drugiego parametru wyczyść zaznaczenie i wybrać tylko jeden wiersz, przekazać `true` rozszerzenie zaznaczenia i dołączyć ten wiersz.
- `SelectRows(NSIndexSet,bool)` -Wybiera danego zestawu wierszy. Przekaż `false` dla drugiego parametru wyczyść zaznaczenie, a następnie wybierz tylko te wiersze, przekazać `true` rozszerzenie zaznaczenia i dodaj następujące wiersze.
- `SelectedRow` -Zwraca bieżący wiersz, który wybrano w tabeli.
- `SelectedRows` -Zwraca `NSIndexSet` zawierających indeksy zaznaczone wiersze.
- `SelectedRowCount` — Zwraca liczbę wybranych wierszy.
- `IsRowSelected(nint)` -Zwraca `true` zaznaczenie danego wiersza.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Typ, aby zaznaczyć wiersz

Jeśli chcesz umożliwić użytkownikowi wpisz znak z widoku tabeli, zostanie wybrana, a następnie wybierz pierwszy wiersz, który ma ten znak, kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w programu Interface Builder. Wybierz widok tabeli **hierarchii interfejsów** i sprawdź **wybierz typ** pola wyboru w **Inspektor atrybut**:

[![](table-view-images/type01.png "Typ wyboru ustawienia")](table-view-images/type01.png#lightbox)

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Teraz możemy edytować `ProductTableDelegate.cs` pliku i dodaj następującą metodę:

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

`GetNextTypeSelectMatch` Metoda przyjmuje danego `searchString` i zwraca wiersz pierwszy `Product` z tego ciągu w jego `Title`.

Jeśli firma Microsoft może uruchomić aplikację, a następnie wpisz znak, zostanie wybrany wiersz:

[![](table-view-images/type02.png "Uruchom przykładową aplikację")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Zmienianie kolejności kolumn

Jeśli chcesz zezwolić użytkownikowi na przeciąganie kolejności kolumn w widoku tabeli, kliknij dwukrotnie `Main.storyboard` plik, aby go otworzyć do edycji w programu Interface Builder. Wybierz widok tabeli **hierarchii interfejsów** i sprawdź **Reordering** pola wyboru w **Inspektor atrybut**:

[![](table-view-images/reorder01.png "Inspektor atrybutu")](table-view-images/reorder01.png#lightbox)

Jeśli firma Microsoft daje wartość **zapisywania** właściwości i sprawdź **informacji o kolumnie** pola, wszelkie zmiany, udostępnimy do układu tabeli zostaną automatycznie zapisane dla nas i przywrócić ją przy następnym zostanie uruchomiony.

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Teraz możemy edytować `ProductTableDelegate.cs` pliku i dodaj następującą metodę:

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Metoda powinna zwrócić `true` dla każdej kolumny, które chcesz zezwolić na to przeciągania zmieniana w `newColumnIndex`, inne zwrot `false`;

Jeśli możemy uruchomić aplikację, aby zmienić kolejność kolumn naszych można przeciągnąć nagłówków kolumn wokół:

[![](table-view-images/reorder02.png "Przykład zmieniono kolejność kolumn")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Edytowanie komórki

Jeśli chcesz umożliwić użytkownikowi edytowanie wartości dana komórka, edytowanie `ProductTableDelegate.cs` pliku, a następnie zmień `GetViewForItem` metody w następujący sposób:

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

Jeśli możemy uruchomić aplikację, użytkownik może edytować komórki w widoku tabeli:

[![](table-view-images/editing01.png "Przykładem edytowania komórki")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>Używanie obrazów w widoku tabeli

Do dołączenia obrazu komórki w `NSTableView`, musisz zmienić, jak dane są zwracane przez widok tabeli `NSTableViewDelegate's` `GetViewForItem` metodę `NSTableCellView` zamiast typowej `NSTextField`. Na przykład:

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

Aby uzyskać więcej informacji, zobacz [przy użyciu obrazów z widokami tabeli](~/mac/app-fundamentals/image.md) części naszych [pracy przy użyciu obrazu](~/mac/app-fundamentals/image.md) dokumentacji.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Dodawanie przycisku Usuń wiersz

Na podstawie wymagań aplikacji, mogą wystąpić sytuacje, gdzie należy podać przycisku akcji dla każdego wiersza w tabeli. Na przykład, możemy rozwinąć w przykładzie w widoku tabeli utworzonej powyżej, aby uwzględnić **Usuń** przycisku w każdym wierszu.

Po pierwsze Przeprowadź edycję `Main.storyboard` w program Xcode Interface Builder, wybierz widok tabeli i zwiększyć liczbę kolumn do trzech (3). Następnie należy zmienić **tytuł** nowej kolumny do `Action`:

[![](table-view-images/delete01.png "Edytowanie nazwy kolumny")](table-view-images/delete01.png#lightbox)

Zapisz zmiany do scenorysu i wróć do programu Visual Studio dla komputerów Mac zsynchronizować zmiany.

Następnie Edytuj `ViewController.cs` pliku i dodaj następującą metodę publiczny:

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

W tym samym pliku, zmodyfikować Tworzenie nowej tabeli widoku delegat wewnątrz `ViewDidLoad` metody w następujący sposób:

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

Teraz, edytować `ProductTableDelegate.cs` pliku do uwzględnienia prywatnego połączenia z kontrolera widoku, a przełączanie kontrolera jako parametr, tworząc nowe wystąpienie delegata:

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

Następnie dodaj następującą nową metodę prywatnych do klasy:

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

Spowoduje to przejście wszystkich konfiguracji widoku tekstu, które zostały wcześniej wykonywana w `GetViewForItem` metody i umieszcza je w jednym, możliwy do wywołania lokalizacji (ponieważ jest to ostatnia kolumna tabeli nie zawiera widok tekstu, ale przycisk).

Wreszcie edytować `GetViewForItem` metody i przypisz ją wyglądać podobnie do następującego:

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

Przyjrzyjmy się kilku sekcji kodu, tym bardziej szczegółowo. Pierwsze, jeśli nowy `NSTableViewCell` jest tworzona akcji pochodzi oparte na nazwę kolumny. Pierwszych dwóch kolumn (**produktu** i **szczegóły**), nowy `ConfigureTextField` metoda jest wywoływana.

Dla **akcji** kolumny, nową `NSButton` zostanie utworzony i dodany do komórki jako widok podrzędny:

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

Przycisk `Tag` właściwość jest używana do przechowywania numer wiersza, który jest aktualnie przetwarzany. Ta liczba będzie później używane podczas użytkownik zażąda wiersz usunięty z przycisku `Activated` zdarzeń:

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

Na początku procedury obsługi zdarzeń uzyskujemy przycisku i produkt, który znajduje się w wierszu danej tabeli. Następnie alertu są prezentowane użytkownikowi potwierdzeniu operacji usuwania wiersza. Jeśli użytkownik zdecyduje usunąć wiersz, danym wierszu zostanie usunięty ze źródła danych i załadowaniu tabeli:

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Na koniec Jeśli komórka widoku tabeli zostanie on ponownie użyty, zamiast tworzenia nowego, poniższy kod konfiguruje go na podstawie kolumny przetwarzane:

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

Dla **akcji** kolumny, aż do są skanowane wszystkie widoki Sub `NSButton` zostanie znaleziony, a następnie jest `Tag` właściwość zostanie zaktualizowany do punktu w bieżącym wierszu.

Za pomocą tych zmian w miejscu, gdy aplikacja jest uruchamiana każdy wiersz będzie mieć **Usuń** przycisku:

[![](table-view-images/delete02.png "Widok tabeli za pomocą przycisków usunięcia")](table-view-images/delete02.png#lightbox)

Kiedy użytkownik kliknie **Usuń** przycisku, zostanie wyświetlony alert pytaniem, aby usunąć wiersz danego:

[![](table-view-images/delete03.png "Alert dotyczący Usuń wiersz")](table-view-images/delete03.png#lightbox)

Jeśli użytkownik wybierze delete, wiersz zostaną usunięte i zostanie narysowany ponownie tabeli:

[![](table-view-images/delete04.png "Tabela po usunięciu wiersza")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Widoki tabel powiązania danych

Przy użyciu technik kodowania pary klucz-wartość i powiązania danych w aplikacji platformy Xamarin.Mac, może znacznie skrócić ilość kodu, który trzeba napisać i obsługiwać do wypełniania i pracować z elementami interfejsu użytkownika. Masz także zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) z usługi frontonu zakończenia interfejsu użytkownika (_Model-View-Controller_), prowadzącego do łatwiejsza Obsługa bardziej elastycznych aplikacji Projekt.

Kodowanie klucz-wartość (KVC) to mechanizm służący do uzyskiwania dostępu do właściwości obiektu pośrednio, za pomocą klucze (specjalnie sformatowanego ciągi), aby zidentyfikować właściwości zamiast uzyskanie do nich dostępu za pośrednictwem zmienne wystąpienia lub metody dostępu (`get/set`). Wdrażając kodowanie klucz-wartość zgodne metod dostępu do aplikacji platformy Xamarin.Mac, możesz uzyskać dostęp do innych funkcji systemu macOS, takich jak obserwowania pary klucz-wartość (KVO), powiązań danych, danych podstawowych, cocoa dla powiązania i scriptability.

Aby uzyskać więcej informacji, zobacz [powiązanie danych widoku tabeli](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) części naszych [powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md) dokumentacji.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z widoki tabel w aplikacji platformy Xamarin.Mac. Firma dostrzegła różnych typów i używa widoku tabeli, jak tworzyć i obsługiwać widoki tabel w program Xcode Interface Builder i sposób pracy z widoki tabel w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacTables (przykład)](https://developer.xamarin.com/samples/mac/MacTables/)
- [MacImages (przykład)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Widoki konspektu](~/mac/user-interface/outline-view.md)
- [Listy źródeł](~/mac/user-interface/source-list.md)
- [Powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [NSTableView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [NSTableViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
