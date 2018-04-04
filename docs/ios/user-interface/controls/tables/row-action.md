---
title: Praca z akcjami wiersza
description: Ten przewodnik przedstawia sposób tworzenia działań niestandardowych Przejdź wierszy tabeli z UISwipeActionsConfiguration lub UITableViewRowAction
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: c257406f3ad81e8144b47e099c9a00f3fdae30cb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-row-actions"></a>Praca z akcjami wiersza

_Ten przewodnik przedstawia sposób tworzenia działań niestandardowych Przejdź wierszy tabeli z UISwipeActionsConfiguration lub UITableViewRowAction_

![Prezentacja działania Przejdź w wierszach](row-action-images/action02.png)

iOS udostępnia dwa sposoby wykonania akcji w tabeli: `UISwipeActionsConfiguration` i `UITableViewRowAction`.

`UISwipeActionsConfiguration` została wprowadzona w systemie iOS 11 i służy do definiowania zestawu działań, które powinny zająć umieść podczas swipes użytkownika _w żadnym kierunku_ wiersza w widoku tabeli. To zachowanie jest podobne do natywnej Mail.app 

`UITableViewRowAction` Klasa jest używana do definiowania akcję, która będzie miała miejsce, gdy swipes użytkownika w lewo poziomie wiersza w widoku tabeli.
Na przykład, gdy edycji tabeli, szybko przesuwając w lewo na wiersz wyświetla **usunąć** przycisk domyślny. Dołączenie wielu wystąpień `UITableViewRowAction` klasy do `UITableView`, można zdefiniować wiele akcji niestandardowych, każde z nich własny tekst, formatowanie i zachowania.


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

Istnieją trzy kroki wymagane do wykonania akcji przejdź z `UISwipeActionsConfiguration`:

1. Zastąpienie `GetLeadingSwipeActionsConfiguration` i/lub `GetTrailingSwipeActionsConfiguration` metody. Te metody zwracają `UISwipeActionsConfiguration`. 
2. Utwórz wystąpienie `UISwipeActionsConfiguration` ma zostać zwrócona. Ta klasa pobiera tablicę `UIContextualAction`.
3. Utwórz `UIContextualAction`.

Te są objaśnione szczegółowo w poniższych sekcjach.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. Implementacja metody SwipeActionsConfigurations

`UITableViewController` (, a także `UITableViewSource` i `UITableViewDelegate`) zawiera dwie metody: `GetLeadingSwipeActionsConfiguration` i `GetTrailingSwipeActionsConfiguration`, które są używane do implementowania działań Przejdź w wierszu widok tabeli. Początkowe działania Przejdź odwołuje się do przejdź z lewej strony ekranu w języku od lewej do prawej i z prawej strony ekranu w języku od prawej do lewej. 

Poniższy przykład (z [TableSwipeActions](https://developer.xamarin.com/samples/monotouch/TableSwipeActions) przykładowe) pokazuje Implementowanie wiodące konfiguracji Przejdź. Dwie akcje są tworzone na podstawie kontekstowe akcje, które opisano szczegółowo [poniżej](#create-uicontextualaction). Czynności te są następnie przekazany do nowo utworzonym [ `UISwipeActionsConfiguration` ](#create-uiswipeactionsconfigurations), które jest używane jako wartości zwracane.


```csharp
public override UISwipeActionsConfiguration GetLeadingSwipeActionsConfiguration(UITableView tableView, NSIndexPath indexPath)
{
    //UIContextualActions
    var definitionAction = ContextualDefinitionAction(indexPath.Row);
    var flagAction = ContextualFlagAction(indexPath.Row);

    //UISwipeActionsConfiguration
    var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction });
    
    leadingSwipe.PerformsFirstActionWithFullSwipe = false;
    
    return leadingSwipe;
}  
```

<a name="create-uiswipeactionsconfigurations" />

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. Utwórz wystąpienie `UISwipeActionsConfiguration`

Utwórz wystąpienie `UISwipeActionsConfiguration` za pomocą `FromActions` metody w celu dodania nowej tablicy `UIContextualAction`s, jak pokazano w poniższy fragment kodu:

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

Należy pamiętać, że kolejność wyświetlania akcji zależy sposób przekazywania do macierzy. Na przykład powyższy kod wiodące swipes Wyświetla akcje w ten sposób:

![początkowe działania Przejdź wyświetlany na wiersza tabeli](row-action-images/action03.png)

Dla spacje końcowe swipes akcje będą wyświetlane, jak pokazano na poniższej ilustracji:

![końcowe czynności przejdź wyświetlany na wiersza tabeli](row-action-images/action04.png)

Następujący fragment kodu również sprawia, że użycie nowego `PerformsFirstActionWithFullSwipe` właściwości. Domyślnie ta właściwość ma wartość true, co oznacza, że pierwszą akcją w tablicy będą się zdarzyć, gdy użytkownik całkowicie swipes w wierszu. Jeśli akcja, która nie jest destrukcyjnego (na przykład "Delete", to nie być idealne zachowanie i w związku z tym należy ją ustawić na `false`.

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>Utwórz `UIContextualAction`

Kontekstowe akcji faktycznie tworzy się akcja, która będzie wyświetlana, gdy użytkownik swipes wiersza tabeli.

Aby zainicjować akcję, należy podać `UIContextualActionStyle`, title oraz a `UIContextualActionHandler`. `UIContextualActionHandler` Przyjmuje trzy parametry: akcji, widok wyświetlanych w akcji i obsługi zakończenia:

```csharp
public UIContextualAction ContextualFlagAction(int row)
{
    var action = UIContextualAction.FromContextualActionStyle
                    (UIContextualActionStyle.Normal,
                        "Flag",
                        (FlagAction, view, success) => {
                            var alertController = UIAlertController.Create($"Report {words[row]}?", "", UIAlertControllerStyle.Alert);
                            alertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, null)); 
                            alertController.AddAction(UIAlertAction.Create("Yes", UIAlertActionStyle.Destructive, null));
                            PresentViewController(alertController, true, null);
                            
                            success(true);
                        });

    action.Image = UIImage.FromFile("feedback.png");
    action.BackgroundColor = UIColor.Blue;

    return action;
}
```

Można edytować różne właściwości visual, takie jak kolor tła lub obraz akcji. Fragment kodu powyżej przedstawiono Dodawanie obrazu do akcji i ustawienie jej kolor tła na niebieski.

Po utworzeniu akcje kontekstowe mogą używać do inicjowania `UISwipeActionsConfiguration` w `GetLeadingSwipeActionsConfiguration` metody.

## <a name="uitableviewrowaction"></a>UITableViewRowAction

Aby zdefiniować co najmniej jednej akcji niestandardowych wiersza dla `UITableView`, konieczne będzie utworzenie wystąpienia `UITableViewDelegate` klasy i zastąpić `EditActionsForRow` — metoda. Na przykład:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Foundation;
using UIKit;

namespace BasicTable
{
    public class TableDelegate : UITableViewDelegate
    {
        #region Constructors
        public TableDelegate ()
        {
        }

        public TableDelegate (IntPtr handle) : base (handle)
        {
        }

        public TableDelegate (NSObjectFlag t) : base (t)
        {
        }

        #endregion

        #region Override Methods
        public override UITableViewRowAction[] EditActionsForRow (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewRowAction hiButton = UITableViewRowAction.Create (
                UITableViewRowActionStyle.Default,
                "Hi",
                delegate {
                    Console.WriteLine ("Hello World!");
                });
            return new UITableViewRowAction[] { hiButton };
        }
        #endregion
    }
}
```

Statycznych `UITableViewRowAction.Create` metoda jest używana do tworzenia nowego `UITableViewRowAction` który wyświetli **Hi** przycisku swipes użytkownika lewo poziomo na wiersz w tabeli. Później nowe wystąpienie klasy `TableDelegate` jest tworzony i dołączona do `UITableView`. Na przykład:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

Podczas uruchamiania powyższej kodu i swipes użytkownika w lewo na wiersz tabeli **Hi** przycisk będzie wyświetlany zamiast **usunąć** przycisku, który jest wyświetlany domyślnie:

[![](row-action-images/action01.png "Przycisk Hi będzie wyświetlany zamiast przycisk Usuń")](row-action-images/action01.png#lightbox)

Jeśli użytkownik naciska **Hi** przycisku `Hello World!` zostanie zapisany do konsoli w programie Visual Studio dla komputerów Mac lub Visual Studio, gdy aplikacja jest uruchamiana w trybie debugowania.



## <a name="related-links"></a>Linki pokrewne

- [TableSwipeActions (przykład)](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)
- [WorkingWithTables (przykład)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
