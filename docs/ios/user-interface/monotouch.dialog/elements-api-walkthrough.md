---
title: Wskazówki — tworzenie aplikacji przy użyciu interfejsu API elementów
description: W tym artykule opisano informacje przedstawione w wprowadzenie do okna dialogowego MonoTouch artykułu. Stanowi wskazówki, która przedstawia sposób użycia MonoTouch.Dialog (patrz hasło MT. D) elementów interfejsu API umożliwiają szybkie rozpoczęcie tworzenia aplikacji z patrz hasło MT. D.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e4fbf744c6f967d09e0033212024c2e2398fb768
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---creating-an-application-using-the-elements-api"></a>Wskazówki — tworzenie aplikacji przy użyciu interfejsu API elementów

_W tym artykule opisano informacje przedstawione w wprowadzenie do okna dialogowego MonoTouch artykułu. Stanowi wskazówki, która przedstawia sposób użycia MonoTouch.Dialog (patrz hasło MT. D) elementów interfejsu API umożliwiają szybkie rozpoczęcie tworzenia aplikacji z patrz hasło MT. D._

W ramach tego przewodnika użyjemy patrz hasło MT. D elementów interfejsu API można utworzyć stylu główny szczegółowy aplikacji, która wyświetla listę zadań. Gdy użytkownik wybierze <span class="ui"> + </span> przycisk na pasku nawigacyjnym zostanie dodany nowy wiersz do tabeli dla zadania. Wybranie wiersza spowoduje przejście do ekranu szczegółów, która pozwala zaktualizować opis zadania i Data ukończenia, jak przedstawiono poniżej:

 [![](elements-api-walkthrough-images/01-task-list-app.png "Wybranie wiersza spowoduje przejście do ekranu szczegółów, która pozwala zaktualizować opis zadania i Data ukończenia")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 <a name="Elements_API_Walkthrough" />


## <a name="elements-api-walkthrough"></a>Elementy interfejsu API wskazówki

W [wprowadzenie do okna dialogowego MonoTouch](~/ios/user-interface/monotouch.dialog/index.md) artykułu, firma Microsoft uzyskuje pełny opis różnych części patrz hasło MT. D. Teraz umieść je z wszystkich elementów do aplikacji za pomocą interfejsu API elementów.

 <a name="Setting_up_the_Multi-Screen_Application" />


## <a name="setting-up-the-multi-screen-application"></a>Konfigurowanie aplikacji usługi ekranu

Aby rozpocząć proces tworzenia ekranu, tworzy MonoTouch.Dialog `DialogViewController`, a następnie dodaje `RootElement`.

Aby utworzyć aplikację usługi ekranu z MonoTouch.Dialog, musimy:

1.  Utwórz  `UINavigationController.`
1.  Utwórz  `DialogViewController.`
1.  Dodaj `DialogViewController` jako katalogu głównego  `UINavigationController.` 
1.  Dodaj `RootElement` do  `DialogViewController.`
1.  Dodaj `Sections` i `Elements` do  `RootElement.` 


 <a name="Using_A_UINavigationController" />


### <a name="using-a-uinavigationcontroller"></a>Przy użyciu UINavigationController

Aby utworzyć aplikację stylu nawigacji, należy utworzyć `UINavigationController`, a następnie dodaj go jako `RootViewController` w `FinishedLaunching` metody `AppDelegate`. Aby `UINavigationController` współpracować MonoTouch.Dialog, dodamy `DialogViewController` do `UINavigationController` w sposób przedstawiony poniżej:

```csharp
public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
        _window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        _rootElement = new RootElement ("To Do List"){new Section ()};

        // code to create screens with MT.D will go here …

        _rootVC = new DialogViewController (_rootElement);
        _nav = new UINavigationController (_rootVC);
        _window.RootViewController = _nav;
        _window.MakeKeyAndVisible ();
            
        return true;
}
```

Powyższy kod tworzy wystąpienie `RootElement` i przekazuje je do `DialogViewController`. `DialogViewController` Ma zawsze `RootElement` u góry hierarchii. W tym przykładzie `RootElement` jest tworzony z ciągiem "Lista zadań do wykonania,", która służy jako tytuł w kontrolerze nawigacji na pasku nawigacyjnym. W tym momencie uruchamiania aplikacji przedstawia ekranu pokazano poniżej:

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Uruchamianie aplikacji przedstawi ekranu pokazano tutaj")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

Zobaczmy, jak używać MonoTouch.Dialog w strukturę hierarchiczną `Sections` i `Elements` można dodać więcej ekranów.

 <a name="Creating_the_Dialog_Screens" />


### <a name="creating-the-dialog-screens"></a>Tworzenie ekranów w oknie dialogowym

A `DialogViewController` jest `UITableViewController` podklasy używającej MonoTouch.Dialog dodawania ekranów. MonoTouch.Dialog tworzy ekrany przez dodanie `RootElement` do `DialogViewController`, jak widzieliśmy powyżej. `RootElement` Może mieć `Section` wystąpień, które reprezentują sekcji tabela.
Sekcje składają się z elementów, inne sekcje, a nawet innych `RootElements`. Przez zagnieżdżanie `RootElements`, jak zajmiemy się obok MonoTouch.Dialog automatycznie tworzy aplikację stylu nawigacji.

 <a name="Using_DialogViewController" />


### <a name="using-dialogviewcontroller"></a>Przy użyciu DialogViewController

`DialogViewController`, Trwa `UITableViewController` podklasa, ma `UITableView` jako jego widoku. W tym przykładzie chcemy dodać elementy do tabeli zawsze <span class="ui"> + </span> dotknięciu jest przycisk. Ponieważ `DialogViewController` został dodany do `UINavigationController`, możemy użyć `NavigationItem`w `RightBarButton` właściwości, aby dodać <span class="ui"> + </span> przycisku, jak pokazano poniżej:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Gdy utworzyliśmy `RootElement` wcześniej, możemy przekazany on pojedynczy `Section` , aby firma Microsoft może dodać elementów jako <span class="ui"> + </span> przycisku jest dotknięciu przez użytkownika. W przypadku tego celu obsługi dla przycisku, możemy użyć poniższego kodu:

```csharp
_addButton.Clicked += (sender, e) => {
                
        ++n;
                
        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
        var taskElement = new RootElement (task.Name){
                new Section () {
                        new EntryElement (task.Name, 
                                "Enter task description", task.Description)
                },
                new Section () {
                        new DateElement ("Due Date", task.DueDate)
                }
        };
        _rootElement [0].Add (taskElement);
};
```

Ten kod tworzy nową `Task` obiektu zawsze jest dotknięciu przycisku. Poniżej pokazano prosty wykonania `Task` klasy:

```csharp
public class Task
{   
        public Task ()
        {
        }
        
        public string Name { get; set; }
        
        public string Description { get; set; }

        public DateTime DueDate { get; set; }
}
```

 []()

Zadania `Name` właściwość jest używana do tworzenia `RootElement`na podpis wraz ze zmienną licznik o nazwie `n` który jest zwiększany dla każdego nowego zadania. MonoTouch.Dialog zmieni się elementy w wierszach, które są dodawane do `TableView` podczas każdego `taskElement` został dodany.

 <a name="Presenting_and_Managing_Dialog_Screens" />


## <a name="presenting-and-managing-dialog-screens"></a>Przedstawienie i zarządzanie ekrany okna dialogowego

My używamy `RootElement` tak, aby MonoTouch.Dialog będzie automatycznie twórz nowe ekranu dla każdego zadania szczegóły i przejdź do niego, gdy zostanie wybrany wiersz.

Sam ekran szczegółów zadań składa się z dwóch części; Każda z tych sekcji zawiera pojedynczy element. Pierwszy element jest tworzona na podstawie `EntryElement` zapewnienie można edytować wiersza zadania `Description` właściwości. Po wybraniu elementu klawiatury do edycji tekstu przedstawiono, jak pokazano poniżej:

 [![](elements-api-walkthrough-images/03-create-task.png "Po wybraniu elementu klawiatury do edycji tekstu przedstawiono, jak pokazano")](elements-api-walkthrough-images/03-create-task.png#lightbox)

Druga sekcja zawiera `DateElement` pozwala nam Zarządzanie zadaniami `DueDate` właściwości. Automatyczne zaznaczanie daty ładuje wyboru daty, jak pokazano:

 [![](elements-api-walkthrough-images/04-date-picker.png "Automatyczne zaznaczanie daty ładuje formant wyboru daty jako")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

W obu `EntryElement` i `DateElement` przypadków (lub dla każdego elementu wprowadzania danych w MonoTouch.Dialog), wszystkie zmiany wartości są zachowywane automatycznie. Firma Microsoft może pokazują edycji daty, a następnie i z powrotem nawigowanie między ekranu głównego i różnych szczegółów zadania, gdy wartości na ekranach szczegółów są zachowywane.

 <a name="Summary" />


## <a name="summary"></a>Podsumowanie

W tym artykule przedstawione wskazówki, które pokazano, jak za pomocą interfejsu API MonoTouch.Dialog elementów. Pasuje do podstawowe kroki, aby utworzyć aplikację usługi ekranu z patrz hasło MT. D, łącznie ze sposobem użycia `DialogViewController` oraz sposobu dodawania elementów i sekcji, aby utworzyć ekranów. Ponadto go pokazano, jak używać patrz hasło MT. D w połączeniu z `UINavigationController`.


## <a name="related-links"></a>Linki pokrewne

- [MTDWalkthrough (przykład)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [Tworzy Miguel de Icaza Screencast - ekran logowania dla systemu iOS z MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Screencast — łatwe tworzenie iOS interfejsy użytkownika z MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Wprowadzenie do MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Wskazówki interfejsu API odbicia](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Wskazówki elementu JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Okno dialogowe MonoTouch w witrynie Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikacji](https://github.com/migueldeicaza/TweetStation)
- [Odwołania do klasy UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Odwołania do klasy UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
