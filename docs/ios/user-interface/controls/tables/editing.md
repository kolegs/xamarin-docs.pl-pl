---
title: Edytowanie
ms.topic: article
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 1ea4489cd6f9839d5d32c97aa7ded41e4f15538a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="editing"></a>Edytowanie

Funkcje edycji tabeli są włączone przez zastąpienie metody `UITableViewSource` podklasy. Najprostsza zachowanie edycji jest gestu przejdź do usunięcia, który może być zaimplementowany z zastąpienie pojedynczej metody.
Bardziej złożone edytowanie (takie jak przenoszenie wierszy) może zostać wykonane z tabelą w trybie edycji.

W tym przewodniku sprawdza następujące czynności:

- [Przejdź do usunięcia](#Swipe_to_Delete)
- [Tryb edycji](#Edit_Mode)
- [Styl edycji wstawiania wiersza](#row_insertion_editing_style)

<a name="Swipe_to_delete" />

## <a name="swipe-to-delete"></a>Przejdź do usunięcia

Przejdź do usunięcia funkcji jest gestu fizycznych w systemie iOS, które użytkownicy będą. 

 [ ![](editing-images/image10.png "Przykład przejdź do usunięcia")](editing-images/image10.png)

Istnieją trzy zastąpień — metoda, które mają wpływ na gestu przejdź do wyświetlenia **usunąć** przycisku w komórce:

-   **CommitEditingStyle** — wykrywa źródło tabeli, jeśli ta metoda zostanie przesłonięta i automatycznie włącza gestu przejdź do usunięcia. Implementacja metody powinny wywoływać `DeleteRows` na `UITableView` spowodować komórek znikają, a także usunąć dane z modelu (na przykład tablicy, słownika lub bazy danych). 
-   **CanEditRow** — Jeśli CommitEditingStyle jest wyłączona, wszystkie wiersze są zakłada się, że można edytować. Jeśli ta metoda jest zaimplementowana i zwraca wartość false (w niektórych wierszach określonych lub wszystkich wierszy) to gestu przejdź do usunięcia nie będą dostępne w tej komórce. 
-   **TitleForDeleteConfirmation** — opcjonalnie określa tekst **usunąć** przycisku. Jeśli ta metoda nie jest zaimplementowana tekst przycisku będzie "Delete". 


Te metody są implementowane w `TableSource` klas w następujący sposób:

```csharp
public override void CommitEditingStyle (UITableView tableView, UITableViewCellEditingStyle editingStyle, Foundation.NSIndexPath indexPath)
{
    switch (editingStyle) {
        case UITableViewCellEditingStyle.Delete:
            // remove the item from the underlying data source
            tableItems.RemoveAt(indexPath.Row);
            // delete the row from the table
            tableView.DeleteRows (new NSIndexPath[] { indexPath }, UITableViewRowAnimation.Fade);
            break;
        case UITableViewCellEditingStyle.None:
            Console.WriteLine ("CommitEditingStyle:None called");
            break;
    }
}
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override string TitleForDeleteConfirmation (UITableView tableView, NSIndexPath indexPath)
{   // Optional - default text is 'Delete'
    return "Trash (" + tableItems[indexPath.Row].SubHeading + ")";
}
```

W tym przykładzie `UITableViewSource` zostały zaktualizowane do używania `List<TableItem>` (zamiast tablicy ciągów) jako źródła danych, ponieważ obsługuje dodawanie i usuwanie elementów z kolekcji.

<a name="Edit_mode" />

## <a name="edit-mode"></a>Tryb edycji

Gdy tabela jest w trybie edycji użytkownik widzi red widżet "stop" w każdym wierszu, który ujawnia przycisk Usuń, po dotknięciu. W tabeli przedstawiono również ikona "uchwytu", aby wskazać, że można przeciągnąć wiersza, aby zmienić kolejność.
**TableEditMode** próbki implementuje te funkcje, jak pokazano.

 [ ![](editing-images/image11.png "Przykładowe TableEditMode implementuje te funkcje, jak pokazano")](editing-images/image11.png)

Istnieje kilka różnych metod na `UITableViewSource` które wpływają na zachowanie w trybie edycji tabeli:

-   **CanEditRow** — czy każdy wiersz może być edytowany. Zwraca wartość FAŁSZ, aby zapobiegać zarówno przejdź do usunięcia i usunięcia w trybie edycji. 
-   **CanMoveRow** — return true na włączanie "uchwyt" lub wartość FAŁSZ, aby zapobiec przenoszenia. 
-   **EditingStyleForRow** — gdy tabela jest w trybie edycji wartość zwrócona przez tę metodę Określa, czy wartość komórki ikony czerwonego usunięcia lub zielonego dodać ikony. Zwraca `UITableViewCellEditingStyle.None` Jeśli wiersza nie można modyfikować. 
-   **MoveRow** — nazywany po przeniesieniu wiersza, aby można zmodyfikować struktury danych, aby pasują do danych, ponieważ jest on wyświetlany w tabeli. 


Implementacji dla pierwszych trzech metod jest względnie proste do przodu — Jeśli nie chcesz używać `indexPath` Aby zmienić zachowanie określonych wierszy, tylko kodowania zwracany wartości dla całej tabeli.

```csharp
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you don't allow re-ordering
}
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    return UITableViewCellEditingStyle.Delete; // this example doesn't support Insert
}
```

`MoveRow` Implementacja jest nieco bardziej skomplikowane, ponieważ należy ją zmienić struktury danych, aby pasowały do nowych. Ponieważ dane są zaimplementowane jako `List` poniższy kod umożliwia usunięcie elementu danych w starej lokalizacji i wstawia go w nowej lokalizacji. Jeśli dane są przechowywane w tabeli bazy danych SQLite kolumna "order" (na przykład), ta metoda zamiast tego należy wykonywać niektórych operacji SQL, aby zmienić kolejność numerów w tej kolumnie.

```csharp
public override void MoveRow (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    var item = tableItems[sourceIndexPath.Row];
    var deleteAt = sourceIndexPath.Row;
    var insertAt = destinationIndexPath.Row;
    
    // are we inserting 
    if (destinationIndexPath.Row < sourceIndexPath.Row) {
        // add one to where we delete, because we're increasing the index by inserting
        deleteAt += 1;
    } else {
        // add one to where we insert, because we haven't deleted the original yet
        insertAt += 1;
    }
    tableItems.Insert (insertAt, item);
    tableItems.RemoveAt (deleteAt);
}
```

Na koniec, aby uzyskać trybu edycji tabeli do, **Edytuj** przycisk musi wywołać `SetEditing` podobny

```csharp
table.SetEditing (true, true);
```

i po zakończeniu edycji, **gotowe** przycisk należy wyłączyć tryb edycji:

```csharp
table.SetEditing (false, true);
```

<a name="Edit_mode_–_row_insertion_editing_style" />

## <a name="row-insertion-editing-style"></a>Style edycji wstawiania wiersza

Wstawiania wierszy z tabeli jest interfejsem użytkownika rzadko — jest najlepszym przykładem w aplikacjach iOS standardowe **Edytuj kontakt** ekranu. Ten zrzut ekranu przedstawia sposób działania funkcji wstawiania wiersza — w edycji tryb Brak kolejnych wierszy która (po kliknięciu) wstawia dodatkowe wiersze do danych. Po zakończeniu edycji, tymczasowy **(Dodaj nowy)** wiersza zostanie usunięty.

 [ ![](editing-images/image12.png "Po zakończeniu edycji tymczasowy Dodawanie nowego wiersza zostanie usunięty.")](editing-images/image12.png)

Istnieje kilka różnych metod na `UITableViewSource` które wpływają na zachowanie w trybie edycji tabeli. Te metody są zaimplementowane w następujący sposób w przykładowym kodzie:

-   **EditingStyleForRow** — zwraca `UITableViewCellEditingStyle.Delete` wierszy zawierających dane i zwraca `UITableViewCellEditingStyle.Insert` dla ostatniego wiersza (który zostanie dodany do zachowywać się jak przycisk Wstaw). 
-   **CustomizeMoveTarget** — gdy użytkownik jest przenoszona komórki wartość zwrócona przez tę metodę opcjonalne można zastąpić, wybór lokalizacji. Oznacza to, że możesz uniemożliwić ich "usunięcie" komórki w określonych miejscach — np. w tym przykładzie uniemożliwiający każdy wiersz przenoszony po **(Dodaj nowy)** wiersza. 
-   **CanMoveRow** — return true na włączanie "uchwyt" lub wartość FAŁSZ, aby zapobiec przenoszenia. W tym przykładzie ostatni wiersz ma "uchwyt" ukryty, ponieważ jest on przeznaczony do serwera jako przycisk Wstaw widoczny tylko. 


Możemy również dodać dwie metody niestandardowe Dodaj wiersz "insert", a następnie usuń ją ponownie, gdy nie jest już wymagane. Są one nazywane z **Edytuj** i **gotowe** przyciski:

-   **WillBeginTableEditing** — gdy **Edytuj** znajduje się przycisk dotknięciu go wywołania `SetEditing` mają zostać umieszczone w tabeli w trybie edycji. Powoduje metody WillBeginTableEditing, gdzie wyświetli **(Dodaj nowy)** wiersza na koniec tabeli jako przycisk Wstaw. 
-   **DidFinishTableEditing** — po dotknięciu jest przycisk Gotowe `SetEditing` zostanie ponownie wywołany, aby wyłączyć tryb edycji. Przykład kodu usuwa **(Dodaj nowy)** wiersz z tabeli po edycji nie jest już wymagane. 


Te zastąpienia metody są implementowane w przykładowym pliku **TableEditModeAdd/Code/TableSource.cs**:

```csharp
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    if (tableView.Editing) {
        if (indexPath.Row == tableView.NumberOfRowsInSection (0) - 1)
            return UITableViewCellEditingStyle.Insert;
        else
            return UITableViewCellEditingStyle.Delete;
    } else // not in editing mode, enable swipe-to-delete for all rows
        return UITableViewCellEditingStyle.Delete;
}
public override NSIndexPath CustomizeMoveTarget (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath proposedIndexPath)
{
    var numRows = tableView.NumberOfRowsInSection (0) - 1; // less the (add new) one
    if (proposedIndexPath.Row >= numRows)
        return NSIndexPath.FromRowSection(numRows - 1, 0);
    else
        return proposedIndexPath;
} 
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return indexPath.Row < tableView.NumberOfRowsInSection (0) - 1;
}
```

Te dwie metody niestandardowe są używane do dodawania i usuwania **(Dodaj nowy)** wiersza tryb edytując tabeli jest włączone lub wyłączone:

```csharp
public void WillBeginTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // insert the 'ADD NEW' row at the end of table display
    tableView.InsertRows (new NSIndexPath[] { 
            NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0), 0) 
        }, UITableViewRowAnimation.Fade);
    // create a new item and add it to our underlying data (it is not intended to be permanent)
    tableItems.Add (new TableItem ("(add new)"));
    tableView.EndUpdates (); // applies the changes
}
public void DidFinishTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // remove our 'ADD NEW' row from the underlying data
    tableItems.RemoveAt ((int)tableView.NumberOfRowsInSection (0) - 1); // zero based :)
    // remove the row from the table display
    tableView.DeleteRows (new NSIndexPath[] { NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0) - 1, 0) }, UITableViewRowAnimation.Fade);
    tableView.EndUpdates (); // applies the changes
}
```

Ponadto ten kod tworzy **Edytuj** i **gotowe** przycisków z wyrażenia lambda, które włączyć lub wyłączyć tryb edycji, gdy są dotknięte:

```csharp
done = new UIBarButtonItem(UIBarButtonSystemItem.Done, (s,e)=>{
    table.SetEditing (false, true);
    NavigationItem.RightBarButtonItem = edit;
    tableSource.DidFinishTableEditing(table);
});
edit = new UIBarButtonItem(UIBarButtonSystemItem.Edit, (s,e)=>{
    if (table.Editing)
        table.SetEditing (false, true); // if we've half-swiped a row
    tableSource.WillBeginTableEditing(table);
    table.SetEditing (true, true);
    NavigationItem.LeftBarButtonItem = null;
    NavigationItem.RightBarButtonItem = done;
});
```

Ten wzorzec interfejsu użytkownika wstawiania wiersza nie jest używana bardzo często, jednak można również użyć `UITableView.BeginUpdates` i `EndUpdates` metody w celu animowania wstawiania lub usuwania komórek w tabeli. Zasadę pod kątem korzystania z tych metod jest, że różnica w wartości zwracanych przez `RowsInSection` między `BeginUpdates` i `EndUpdates` wywołania musi odpowiadać net liczbę komórek dodać/usunąć z `InsertRows` i `DeleteRows` metody. Jeśli podstawowego źródła danych nie ulega zmianie, aby dopasować wstawienia/usuwania w tabeli widoku wystąpi błąd.


## <a name="related-links"></a>Linki pokrewne

- [WorkingWithTables (przykład)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
