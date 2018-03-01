---
title: "Wypełnianie tabeli z danymi"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: fb0e4341d8d8ad0719f35c691add9bad1d3f85a8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="populating-a-table-with-data"></a>Wypełnianie tabeli z danymi

Aby dodać wierszy do `UITableView` należy wdrożyć `UITableViewSource` podklasy i zastąpienie wywołuje metody, które wyświetlić tabeli do wypełnienia sam.

Ten przewodnik obejmuje:

- Tworzenie podklas UITableViewSource
- Ponowne użycie komórki
- Dodawanie indeksu
- Dodawanie nagłówków i stopek


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>Tworzenie podklas UITableViewSource

A `UITableViewSource` podklasy jest przypisany do każdego `UITableView`. Widok tabeli zapytania klasę źródła, aby określić sposób renderowania sam (na przykład, ile wierszy są wymagane i wysokość każdego wiersza, jeśli jest inna niż wartość domyślna). Przede wszystkim źródło udostępnia każdy widok komórki wypełniane przy użyciu danych.

Istnieją tylko dwa obowiązkowe metody wymagane do tworzenia tabel dane:

-   **RowsInSection** — zwracany [ `nint` ](http://developer.xamarin.com/guides/cross-platform/macios/nativetypes/) liczby całkowitej liczby wierszy danych powinien być wyświetlany w tabeli.
-   **GetCell** — zwracany `UITableCellView` wypełniane przy użyciu danych odpowiedni indeks wiersza przekazywany do metody.


Przykładowy plik BasicTable **TableSource.cs** ma najprostsza możliwa implementację elementu `UITableViewSource`. Widoczne we fragmencie kodu poniżej akceptuje tablicy ciągów w celu wyświetlenia w tabeli i zwraca domyślny styl komórki zawierające każdy ciąg:

```csharp
public class TableSource : UITableViewSource {

        string[] TableItems;
        string CellIdentifier = "TableCell";

        public TableSource (string[] items)
        {
            TableItems = items;
        }

        public override nint RowsInSection (UITableView tableview, nint section)
        {
            return TableItems.Length;
        }

        public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewCell cell = tableView.DequeueReusableCell (CellIdentifier);
            string item = TableItems[indexPath.Row];

            //---- if there are no cells to reuse, create a new one
            if (cell == null)
            { cell = new UITableViewCell (UITableViewCellStyle.Default, CellIdentifier); }

            cell.TextLabel.Text = item;

            return cell;
        }
}
```

A `UITableViewSource` można użyć dowolnej struktury danych z tablicy prostego ciągu (jak pokazano w tym przykładzie) <> listy lub innych kolekcji. Implementacja `UITableViewSource` metody izoluje tabeli podstawowej struktury danych.

Aby użyć tego podklasy, Utwórz tablicy ciągów, aby utworzyć źródło, a następnie przypisać je do wystąpienia `UITableView`:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    table = new UITableView(View.Bounds); // defaults to Plain style
    string[] tableItems = new string[] {"Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers"};
    table.Source = new TableSource(tableItems);
    Add (table);
}
```

Tabeli wynikowej wygląda następująco:

 [ ![](populating-a-table-with-data-images/image3.png "Przykładowa tabela uruchomiona")](populating-a-table-with-data-images/image3.png)

Większość tabel umożliwia użytkownikowi touch wiersza, aby go zaznaczyć, a następnie wykonać inną akcję (np. odtwarzanie utworu, wywoływania kontaktu lub przedstawiający inny ekran). Aby to osiągnąć, istnieje kilka rzeczy, które należy wykonać. Najpierw utwórz AlertController, aby wyświetlić komunikat, gdy użytkownik kliknie na wiersz, dodając następujące polecenie, aby `RowSelected` metody:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

Następnie utwórz wystąpienie kontrolera widoku:

```csharp
HomeScreen owner;
```
Dodaj Konstruktor do własnej klasy UITableViewSource, która przyjmuje kontroler widoku jako parametr i zapisuje go w polu:

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```
Zmodyfikuj metodę ViewDidLoad, gdy klasa UITableViewSource jest tworzona do przekazania `this` odwołania:

```csharp
table.Source = new TableSource(tableItems, this);
```
Na koniec ponownie Twoje `RowSelected` metoda, wywołanie `PresentViewController` w pamięci podręcznej pola:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


Teraz użytkownik może touch wiersza i pojawi się alert:



 [ ![](populating-a-table-with-data-images/image4.png "Wybrany alert wiersza")](populating-a-table-with-data-images/image4.png)


## <a name="cell-reuse"></a>Ponowne użycie komórki

W tym przykładzie istnieją elementy tylko sześć, więc nie ma żadnych ponownemu komórki wymagane. Wyświetlając setkami lub tysiącami wierszy, jednak byłoby odpady pamięci do utworzenia setek lub tysięcy `UITableViewCell` obiekty, jeśli tylko kilka mieści się na ekranie naraz.

Aby uniknąć tej sytuacji, gdy komórki zniknie z ekranu, jego widoku jest umieszczony w kolejce do ponownego użycia. Jako użytkownik przewija widok, wywołuje tabeli `GetCell` żądania nowych widoków do wyświetlenia, — do ponownego użycia istniejącej komórki (który nie jest on wyświetlany) po prostu Wywołaj `DequeueReusableCell` metody. Jeśli komórka jest dostępny do ponownego użycia, które zostaną zwrócone, w przeciwnym razie zwrócona wartość null i kod, należy utworzyć nowe wystąpienie komórki.

Następujący fragment kodu w przykładzie pokazano wzorca:

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

`cellIdentifier` Skutecznie tworzy osobne kolejki dla różnych typów komórki. W tym przykładzie, że wszystkie komórki wygląd samego ustalony tak tylko jeden identyfikator jest używany. Gdyby różnego rodzaju komórki każdego nich ciąg identyfikatora różnych zarówno w przypadku utrzymującego i zleconą z kolejki ponownego użycia.

### <a name="cell-reuse-in-ios-6"></a>Ponowne użycie komórki w iOS 6 +

iOS 6 dodać wzorzec ponownemu komórki podobne do wprowadzenia jednego z widokami kolekcja. Mimo że istniejącej struktury ponownemu pokazanym powyżej jest nadal obsługiwane wstecz zgodności, to nowy wzorzec jest preferowana, ponieważ eliminuje konieczność sprawdzania wartości null w komórce.

Za pomocą wzorca nowych aplikacji rejestruje klasy komórki lub xib mają być używane przez wywołanie każdej `RegisterClassForCellReuse` lub `RegisterNibForCellReuse` w Konstruktorze kontrolera. A następnie, kiedy dequeueing komórki w `GetCell` metody, po prostu wywołanie `DequeueReusableCell` przekazywanie identyfikator zarejestrowany dla klasy komórki lub xib i ścieżka indeksu.

Na przykład następujący kod rejestruje klasy niestandardowej komórki w UITableViewController:

```csharp
public class MyTableViewController : UITableViewController
{
  static NSString MyCellId = new NSString ("MyCellId");

  public MyTableViewController ()
  {
    TableView.RegisterClassForCellReuse (typeof(MyCell), MyCellId);
  }
  ...
}
```

W klasie MyCell zarejestrowany komórki można usuniętej w `GetCell` metody `UITableViewSource` bez konieczności sprawdzania bardzo null, jak pokazano poniżej:

```csharp
class MyTableSource : UITableViewSource
{
  public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
  {
    // if cell is not available in reuse pool, iOS will create one automatically
    // no need to do null check and create cell manually
    var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

    // do whatever you need to with cell, such as assigning properties, etc.

    return cell;
  }
}
```

Należy pamiętać, jeśli nowy wzorzec ponownemu z klasy niestandardowej komórki, musisz wdrożyć Konstruktor, który pobiera `IntPtr`, jak pokazano poniżej fragment, w przeciwnym razie Objective-C nie można skonstruować wystąpienia klasy komórki:

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

Zawiera przykłady tematów przedstawionych powyżej w **BasicTable** próbki połączone z tym artykule.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>Dodawanie indeksu

Indeks ułatwia użytkownikom przewijać długich list, zwykle w kolejności alfabetycznej mimo że może indeksować według niezależnie od kryteriów ma. **BasicTableIndex** próbki ładuje znacznie dłużej listę elementów z pliku, aby pokazać indeksu. Każdy element w indeksie odpowiada "sekcji" w tabeli.

 [ ![](populating-a-table-with-data-images/image5.png "Wyświetlanie indeksu")](populating-a-table-with-data-images/image5.png)

Do obsługi danych tabeli należy grupowane, więc tworzy próbki BasicTableIndex "sections" `Dictionary<>` z tablicy ciągów za pomocą pierwszą literę każdego elementu jako klucz słownika:

```csharp
indexedTableItems = new Dictionary<string, List<string>>();
foreach (var t in items) {
    if (indexedTableItems.ContainsKey (t[0].ToString ())) {
        indexedTableItems[t[0].ToString ()].Add(t);
    } else {
        indexedTableItems.Add (t[0].ToString (), new List<string>() {t});
    }
}
keys = indexedTableItems.Keys.ToArray ();
```

`UITableViewSource` Podklasy następnie wymaga następujących metod dodane lub zmodyfikowane w celu użycia `Dictionary<>` :

-   **NumberOfSections** — ta metoda jest opcjonalne, domyślnie tabeli zakłada jedną sekcję. Podczas wyświetlania indeksu ta metoda powinna zwrócić liczba elementów w indeksie (na przykład 26 Jeśli indeks zawiera wszystkie litery alfabetu angielskiego).
-   **RowsInSection** — zwraca liczbę wierszy w danej sekcji.
-   **SectionIndexTitles** — zwraca tablicę ciągów, które będą używane do wyświetlania indeksu. W przykładowym kodzie zwraca tablicę litery.


Zaktualizowano metod w przykładowym pliku **BasicTableIndex/TableSource.cs** wyglądać następująco:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return keys.Length;
}
public override nint RowsInSection (UITableView tableview, nint section)
{
    return indexedTableItems[keys[section]].Count;
}
public override string[] SectionIndexTitles (UITableView tableView)
{
    return keys;
}
```

Indeksy zwykle są używane tylko przy użyciu stylu zwykły tabeli.


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>Dodawanie nagłówków i stopek

Nagłówki i stopki może służyć do grupowania wizualnie wierszy w tabeli. Struktura danych, wymagane jest bardzo podobne do dodawania indeksu — `Dictionary<>` naprawdę dobrze działa. Zamiast używać alfabetu do grupowania komórek, w tym przykładzie pozwala grupować warzyw według typu botaniczne.
Dane wyjściowe wyglądają następująco:

 [ ![](populating-a-table-with-data-images/image6.png "Przykładowe nagłówki i stopki")](populating-a-table-with-data-images/image6.png)

Aby wyświetlić nagłówki i stopki `UITableViewSource` podklasy wymaga tych dodatkowych metod:

-   **TitleForHeader** — zwraca tekst, który ma być używana jako nagłówka
-   **TitleForFooter** — zwraca tekst, który ma być używana jako stopki.


Zaktualizowano metod w przykładowym pliku **BasicTableHeaderFooter/Code/TableSource.cs** wyglądać następująco:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    return keys[section];
}
public override string TitleForFooter (UITableView tableView, nint section)
{
    return indexedTableItems[keys[section]].Count + " items";
}
```

Można dostosować wygląd nagłówku i stopce z widokiem obiekt, za pomocą `GetViewForHeader` i `GetViewForFooter` metoda zastępuje na `UITableViewSource`.


## <a name="related-links"></a>Linki pokrewne

- [WorkingWithTables (przykład)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
