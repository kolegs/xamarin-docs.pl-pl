---
title: Praca z tabelami w Projektancie systemu iOS
description: "W poprzednich sekcjach możemy przedstawione tworzenie przy użyciu tabel. W tym celu sekcji piątego i końcowych Zapoznamy agregacji, co ma dowiedzieliśmy się do tej pory i utworzyć aplikację podstawowych kwestii listy przy użyciu scenorysu."
ms.topic: article
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: c59ddde44b0e47122865c55a7964707f106d2691
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-tables-in-the-ios-designer"></a>Praca z tabelami w Projektancie systemu iOS

Scenorys WYSIWYG służą do tworzenia aplikacji systemu iOS i są obsługiwane w programie Visual Studio na Mac i systemu Windows. Aby uzyskać więcej informacji na Scenorys dotyczą [wprowadzenie do Scenorys](~/ios/user-interface/storyboards/index.md) dokumentu. Scenorys można również edytować komórki układów *w* tabeli, które upraszcza tworzenie tabel i komórek

Podczas konfigurowania właściwości widoku tabeli w systemie iOS projektanta, istnieją dwa typy zawartości komórki, możesz wybrać spośród: **dynamiczne** lub **statycznych** prototypu zawartości.

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>Zawartość dynamiczna prototypu

A `UITableView` z prototypem zawartości zwykle jest przeznaczony do wyświetlania listy danych gdzie komórki prototypu (lub komórek, jak można zdefiniować więcej niż jeden) są ponownie używane dla każdego elementu na liście. Komórki nie można utworzyć wystąpienia, są uzyskiwane w `GetView` metody wywołując `DequeueReusableCell` metoda jego `UITableViewSource`.

 <a name="Static_Content" />


## <a name="static-content"></a>Zawartość statyczna

`UITableView`s zawartość statyczna umożliwia tabel należy planować w prawo na powierzchnię projektu. Komórki można przeciągnąć do tabeli i dostosowane przez zmianę właściwości i dodawanie formantów.

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>Tworzenie aplikacji opartych na scenorysu

Przykład StoryboardTable zawiera prostej aplikacji główny szczegółowy używającej obu typów UITableView w scenorysu. W pozostałej części tej sekcji opisano sposób tworzenia przykład listy zadań do wykonania w małych, w którym będzie wyglądać następująco po zakończeniu:

 [ ![Przykład ekranów](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png)

Interfejs użytkownika zostanie skompilowany scenorysu, a oba ekrany użyje UITableView. Korzysta z ekranu głównego *zawartości prototypu* układu wiersza i szczegóły ekran używa *zawartość statyczną* można utworzyć formularza wprowadzania danych przy użyciu układy niestandardowe komórki.

## <a name="walkthrough"></a>Wskazówki

Utwórz nowe rozwiązanie Visual Studio przy użyciu **(Utwórz) o nowy projekt... > pojedynczego widoku App(C#)**i nadaj mu _StoryboardTables_.

 [ ![Utwórz okno dialogowe nowego projektu](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png)

Rozwiązanie zostanie otwarty z niektórych plików języka C# i `Main.storyboard` plik już utworzony. Kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć go w systemie iOS projektanta.

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>Modyfikowanie scenorysu

Scenorysu będzie można edytować w trzy kroki:

-  Najpierw układu wymaganych wyświetlania kontrolerów i ustawiania ich właściwości.
-  Następnie utwórz interfejsu użytkownika poprzez przeciągnięcie i upuszczenie obiekty do widoku
-  Na koniec Dodaj wymagane klasy UIKit do każdego widoku i nadaj nazwę różnych formantów, dlatego może być przywoływany w kodzie.


Po zakończeniu scenorysu kodu można dodać do wszystkich elementów.

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>Układ kontrolerów widoku

Pierwsza zmiana do scenorysu jest usunięcie istniejącego widoku szczegółów i zastępowanie UITableViewController. Wykonaj następujące kroki:

1.  Zaznacz na pasku w dolnej części kontroler widoku i usuń go.
2.  Przeciągnij **kontrolera nawigacji** i **tabeli kontrolera widoku** na scenorysu z przybornika. 
3.  Utwórz segue kontroler widoku głównego na drugi kontroler widoku tabeli, który został dodany. Aby utworzyć segue, sterowania i przeciągnij *z komórki szczegółów* do nowo dodanego UITableViewController. Wybierz opcję **Pokaż*** w obszarze **Segue wybór**. 
4.  Wybierz nowe segue utworzony i nadaj mu identyfikator odwołania to segue w kodzie. Polecenie segue, a następnie wprowadź `TaskSegue` dla **identyfikator** w **konsoli właściwości**, podobnie do następującej:    
  [ ![Nazewnictwo segue w panelu Właściwość](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png) 

5. Skonfiguruj dwa widoki tabel zaznaczając je i przy użyciu konsoli do właściwości. Upewnij się wybrać widok i nie kontrolera widoku — konspekt dokumentu ułatwiające wybór.

6.  Kontroler widoku głównego, należy zmienić **zawartości: dynamiczne prototypy** (widok na powierzchni projektu będzie mieć etykietę **prototypu zawartości** ):

    [ ![Ustawienie właściwości zawartości dynamicznej prototypów](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png)

7.  Zmień nowego **UITableViewController** jako **zawartości: statyczne komórek**. 


8. Nowe UITableViewController musi mieć swoją nazwę klasy i Ustaw identyfikator. Wybierz widok kontroler i typ _TaskDetailViewController_ dla **klasy** w **konsoli właściwości** — spowoduje to utworzenie nowego `TaskDetailViewController.cs` pliku w rozwiązaniu Konsola. Wprowadź **StoryboardID** jako _szczegółów_, jak pokazano w poniższym przykładzie. To będzie można później załadować tego widoku w kodzie języka C#:  

    [ ![Ustawienie Identyfikatora scenorysu](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png)

9. Scenorysu powierzchni projektu powinna wyglądać następująco (tytuł elementu nawigacji głównego kontrolera widoku został zmieniony na "Kwestii tablicy"):

    [ ![Dzięki powierzchni projektowej](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>Tworzenie interfejsu użytkownika

Teraz, widoków i segues są skonfigurowane, elementy interfejsu użytkownika musi zostać dodany.

#### <a name="root-view-controller"></a>Kontroler widoku głównego

Najpierw należy zaznaczyć komórkę prototypu w kontroler widoku głównego i ustawić **identyfikator** jako _taskcell_, jak pokazano poniżej. Można pobrać wystąpienia tej UITableViewCell ten będzie używany w dalszej części kodu:

 [ ![Ustawianie identyfikatora komórki](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png)

Następnie musisz utworzyć przycisku, który doda nowe zadania, jak przedstawiono poniżej:

[ ![pasek element przycisku w pasku nawigacyjnym](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png)

Wykonaj następujące czynności: 

-  Przeciągnij **element przycisku paska** z przybornika do _po prawej stronie paska nawigacyjnego_.
-  W **konsoli właściwości**w obszarze **element przycisku paska** wybierz **identyfikator: Dodaj** (aby stał się  *+*  oraz przycisk). 
-  Nadaj mu nazwę, aby można go zidentyfikować w kodzie w późniejszym terminie. Należy pamiętać, że konieczne będzie nadaj nazwę klasy kontrolera widoku głównego (na przykład **ItemViewController**) można ustawić nazwy element przycisku paska.


#### <a name="taskdetail-view-controller"></a>Kontroler widoku TaskDetail

Widok szczegółów wymaga wiele pracy. Komórki w widoku tabeli muszą być przeciągnięto widoku i następnie wypełniane przy użyciu etykiet, widoki tekstu i przycisków. Poniższy zrzut ekranu przedstawia Zakończono interfejsu użytkownika z dwóch części. Jedną sekcję ma trzy komórki, trzy etykiety, pól tekstowych i jeden przełącznik, podczas gdy druga sekcja ma jedną komórkę z dwóch przycisków:

 [ ![Układ widoku szczegółów](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png)

Dostępne są następujące kroki, aby utworzyć pełną układ:

Wybierz widok tabeli i Otwórz **konsoli właściwości**. Zaktualizuj następujące właściwości:

-  **Sekcje**: _2_ 
-  **Styl**: _pogrupowane_
-  **Separator**: _None_
-  **Wybór**: _Brak zaznaczenia_

Wybierz pierwsza sekcja i w obszarze **właściwości > sekcji Widok tabeli** zmienić **wierszy** do _3_, jak pokazano poniżej:


 [ ![Ustawienie pierwsza sekcja trzy wiersze](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png)

Dla każdej komórki Otwórz **konsoli właściwości** i ustaw:

-  **Styl**: _niestandardowe_
-  **Identyfikator**: Wybierz unikatowy identyfikator dla każdej komórki (np.) "_tytuł_","_uwagi_","_gotowe_").
-  Przeciągnij formanty wymagane do tworzenia układu wyświetlany na zrzucie ekranu (umieścić **UILabel**, **UITextField** i **UISwitch** na odpowiednich komórek i ustawić etykiety odpowiednio tj. Tytuł, uwagi i gotowe).


W drugiej sekcji, ustaw **wierszy** do _1_ i przechwycić uchwyt zmiany rozmiaru dolnej krawędzi komórki, aby była większa.

-  **Identyfikator zestawu**: unikatowa wartość (np.) "Zapisz"). 
-  **Ustawianie tła**: _wyczyść kolor_ .
-  Przeciągnij dwóch przycisków komórki i odpowiednio ustawione ich tytuły (tj. _zapisać_ i _usunąć_), jak pokazano poniżej:

   [ ![Ustawianie dwóch przycisków w dolnej części](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png)

Na tym etapie może również chcesz ustawić ograniczenia na komórek i kontroli w celu zapewnienia adaptacyjnego układu.

### <a name="adding-uikit-class-and-naming-controls"></a>Dodawanie klasy UIKit i nazewnictwa formantów

Istnieje kilka ostatnie kroki tworzenia naszych scenorysu. Najpierw możemy musi nadać każdego z naszych formanty nazwę w polu **tożsamości > nazwa** aby można było używać w kodzie później. Nazwa je w następujący sposób:

-  **Tytuł UITextField** : _TitleText_
-  **Uwagi dotyczące UITextField** : _NotesText_
-  **UISwitch** : _DoneSwitch_
-  **Usuń UIButton** : _DeleteButton_
-  **Zapisz UIButton** : _SaveButton_


<a name="Adding_Code" />

## <a name="adding-code"></a>Dodawanie kodu

W pozostałej części pracy będzie realizowane w Visual Studio Mac lub Windows w języku C#. Należy pamiętać, że nazwy właściwości używane w kodzie odzwierciedlają w powyższym wskazówki.

Najpierw chcemy utworzyć `Chores` klasy, która zapewnia łatwy sposób do pobierania i ustawiania wartości Identyfikatora, nazwy, uwagi i gotowe Boolean, dzięki czemu możemy użyć tych wartości w całej aplikacji.

W Twojej `Chores` klasy, Dodaj następujący kod:

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

Następnie należy utworzyć `RootTableSource` klasy, która dziedziczy `UITableViewSource`. 

Różnica między tą i widoku tabeli scenorysu z systemem innym niż jest to, że `GetView` — metoda nie potrzebuje do utworzenia wystąpienia dowolnego komórek — `theDequeueReusableCell` metoda zawsze zwraca wystąpienia prototypu komórki (z pasującym identyfikatorem).

Poniższy kod jest z `RootTableSource.cs` pliku:

```csharp
public class RootTableSource : UITableViewSource
{
// there is NO database or storage of Tasks in this example, just an in-memory List<>
Chores[] tableItems;
string cellIdentifier = "taskcell"; // set in the Storyboard

    public RootTableSource(Chores[] items)
    {
        tableItems = items;
    }

public override nint RowsInSection(UITableView tableview, nint section)
{
  return tableItems.Length;
}

public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
  // in a Storyboard, Dequeue will ALWAYS return a cell, 
  var cell = tableView.DequeueReusableCell(cellIdentifier);
  // now set the properties as normal
  cell.TextLabel.Text = tableItems[indexPath.Row].Name;
  if (tableItems[indexPath.Row].Done)
    cell.Accessory = UITableViewCellAccessory.Checkmark;
  else
    cell.Accessory = UITableViewCellAccessory.None;
  return cell;
}
public Chores GetItem(int id)
{
  return tableItems[id];
}
```

Aby użyć `RootTableSource` klasy, Utwórz nową kolekcję w `ItemViewController`w konstruktora:

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

W `ViewWillAppear` przekazywane kolekcję w źródle i przypisać do tabeli w widoku:

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

Po uruchomieniu aplikacji teraz ekranu głównego będzie teraz załadować i wyświetlić listę dwa zadania. Gdy zadanie jest dotknięciu segue zdefiniowane przez scenorysu spowoduje wyświetlenie ekranu szczegółów, ale nie będą wyświetlane wszystkie dane w tej chwili.

Aby "send parametr" segue, Przesłoń `PrepareForSegue` metodę i ustawić właściwości `DestinationViewController` ( `TaskDetailViewController` w tym przykładzie). Klasa kontrolera widoku docelowego zostanie utworzone, ale nie jest jeszcze wyświetlane użytkownikowi — to oznacza, że można ustawić właściwości dla klasy, ale nie modyfikować wszystkie kontrolki interfejsu użytkownika:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
    {
      if (segue.Identifier == "TaskSegue") { // set in Storyboard
        var navctlr = segue.DestinationViewController as TaskDetailViewController;
        if (navctlr != null) {
          var source = TableView.Source as RootTableSource;
          var rowPath = TableView.IndexPathForSelectedRow;
          var item = source.GetItem(rowPath.Row);
          navctlr.SetTask (this, item); // to be defined on the TaskDetailViewController
        }
      }
    }
```

W `TaskDetailViewController` `SetTask` — metoda przypisuje jego parametrów do właściwości, więc może być przywoływany w ViewWillAppear. Nie można zmodyfikować właściwości formantu w `SetTask` ponieważ nie istnieje podczas `PrepareForSegue` nosi nazwę:

```csharp
Chore currentTask {get;set;}
    public ItemViewController Delegate {get;set;} // will be used to Save, Delete later

public override void ViewWillAppear (bool animated)
    {
      base.ViewWillAppear (animated);
      TitleText.Text = currentTask.Name;
      NotesText.Text = currentTask.Notes;
      DoneSwitch.On = currentTask.Done;
    }

    // this will be called before the view is displayed
    public void SetTask (ItemViewController d, Chore task) {
      Delegate = d;
      currentTask = task;
    }
```

Segue teraz otworzyć ekran szczegółów i wyświetlać informacje wybranego zadania. Niestety nie jest implementacji **zapisać** i **usunąć** przycisków. Przed wdrożeniem przycisków, Dodaj te metody **ItemViewController.cs** do aktualizacji danych podstawowych i zamknąć ekran szczegółów:

```csharp
public void SaveTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
        NavigationController.PopViewController(true);
}

public void DeleteTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
  chores.Remove(oldTask);
        NavigationController.PopViewController(true);
}
```

Następnie należy dodać przycisku `TouchUpInside` program obsługi zdarzeń do `ViewDidLoad` metody **TaskDetailViewController.cs**. `Delegate` Odwołania do właściwości do `ItemViewController` został utworzony w szczególności firma Microsoft może wywołać `SaveTask` i `DeleteTask`, które Zamknij to okno widoku w ramach działania:

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

Ostatni element pozostałych funkcjonalność do tworzenia jest tworzenie nowych zadań. W **ItemViewController.cs** Dodaj metodę, która tworzy nowe zadania i otwiera widok szczegółów. Można utworzyć wystąpienia widoku z użytku scenorysu `InstantiateViewController` metody z `Identifier` dla tego widoku — w tym przykładzie, który ma być "szczegóły":

```csharp
public void CreateTask () 
    {
      // first, add the task to the underlying data
      var newId = chores[chores.Count - 1].Id + 1;
      var newChore = new Chore{Id = newId};
      chores.Add (newChore);

      // then open the detail view to edit it
      var detail = Storyboard.InstantiateViewController("detail") as TaskDetailViewController;
      detail.SetTask (this, newChore);
      NavigationController.PushViewController (detail, true);
    }
```

Na koniec okablować przycisk na pasku nawigacyjnym w **ItemViewController.cs**w `ViewDidLoad` metody w celu wywołania go:

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

Na tym kończy się przykład scenorysu — Zakończono aplikacji prawdopodobnie to:

[ ![Zakończono aplikacji](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png)

W przykładzie pokazano:

-  Tworzenie tabeli z prototypem zawartości gdzie są zdefiniowane komórek do ponownego użycia wyświetlić listę danych. 
-  Tworzenie tabeli z zawartość statyczna służy do tworzenia formularza danych wejściowych. Obejmuje zmiany stylu tabeli i dodanie sekcji, komórek i formantów interfejsu użytkownika. 
-  Jak utworzyć segue i zastąpić `PrepareForSegue` metodę, aby powiadomić widok elementu docelowego wymaga parametrów. 
-  Ładowanie widoków scenorysu bezpośrednio z `Storyboard.InstantiateViewController` metody.



## <a name="related-links"></a>Linki pokrewne

- [StoryboardTable (przykład)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Wprowadzenie do scenorysu](~/ios/user-interface/storyboards/index.md)
