---
title: Tworzenie aplikacji platformy Xamarin.iOS przy użyciu interfejsu API elementów
description: W tym artykule opiera się na informacje znajdujące się we wprowadzeniu do okna dialogowego MonoTouch artykułu. Przedstawia informacje o przewodnik, który przedstawia sposób użycia elementu MonoTouch.Dialog (góra MT.) D) elementów interfejsu API umożliwiają szybkie rozpoczęcie tworzenia aplikacji za pomocą patrz hasło MT. D.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: dcd6f1260be3414c515010c2fd615910c7b5c054
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038345"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>Tworzenie aplikacji platformy Xamarin.iOS przy użyciu interfejsu API elementów

_W tym artykule opiera się na informacje znajdujące się we wprowadzeniu do okna dialogowego MonoTouch artykułu. Przedstawia informacje o przewodnik, który przedstawia sposób użycia elementu MonoTouch.Dialog (góra MT.) D) elementów interfejsu API umożliwiają szybkie rozpoczęcie tworzenia aplikacji za pomocą patrz hasło MT. D._

W tym przewodniku użyjemy patrz hasło MT. D elementów interfejsu API Tworzenie stylu wzorzec / szczegół aplikacji, która wyświetla listę zadań. Gdy użytkownik wybierze <span class="ui"> + </span> przycisk na pasku nawigacyjnym zostanie dodany nowy wiersz do tabeli dla tego zadania. Zaznaczając wiersz spowoduje przejście do ekranu szczegółów, który pozwala zaktualizować opis zadania i Data ukończenia, jak przedstawiono poniżej:

 [![](elements-api-walkthrough-images/01-task-list-app.png "Zaznaczając wiersz spowoduje przejście do ekranu szczegółów, który pozwala zaktualizować opis zadania i Data ukończenia")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 ## <a name="setting-up-mtd"></a>Konfigurowanie patrz hasło MT. D

PATRZ HASŁO MT. D jest rozpowszechniana z platformy Xamarin.iOS. Aby go użyć, kliknij prawym przyciskiem myszy **odwołania** węzła Xamarin.iOS projektu w programie Visual Studio 2017 lub Visual Studio dla komputerów Mac i Dodaj odwołanie do **1 elementu MonoTouch.Dialog** zestawu. Następnie należy dodać `using MonoTouch.Dialog` instrukcji w źródle kodu zgodnie z potrzebami.

## <a name="elements-api-walkthrough"></a>Przewodnik elementów interfejsu API

W [wprowadzenie do okna dialogowego MonoTouch](~/ios/user-interface/monotouch.dialog/index.md) artykułu, firma Microsoft zdobyte pełny opis różnych części MT. D. Utwórzmy umieszczania ich wszystko ze sobą w aplikacji za pomocą interfejsu API elementów.

## <a name="setting-up-the-multi-screen-application"></a>Konfigurowanie aplikacji wieloma ekranami

Aby rozpocząć proces tworzenia ekranu, tworzy elementu MonoTouch.Dialog `DialogViewController`, a następnie dodaje `RootElement`.

Aby utworzyć aplikację z wieloma ekranami przy użyciu elementu MonoTouch.Dialog, musimy:

1.  Utwórz `UINavigationController.`
1.  Utwórz `DialogViewController.`
1.  Dodaj `DialogViewController` jako katalog główny  `UINavigationController.` 
1.  Dodaj `RootElement` do  `DialogViewController.`
1.  Dodaj `Sections` i `Elements` do  `RootElement.` 

### <a name="using-a-uinavigationcontroller"></a>Za pomocą UINavigationController

Aby utworzyć aplikację stylu nawigacji, musimy utworzyć `UINavigationController`, a następnie dodaj go jako `RootViewController` w `FinishedLaunching` metody `AppDelegate`. Aby `UINavigationController` pracować elementu MonoTouch.Dialog, dodamy `DialogViewController` do `UINavigationController` jak pokazano poniżej:

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

Powyższy kod tworzy wystąpienie `RootElement` i przekazuje je do `DialogViewController`. `DialogViewController` Ma zawsze `RootElement` u góry hierarchii. W tym przykładzie `RootElement` jest tworzona przy użyciu ciągu "Lista zadań do wykonania,", która służy jako tytuł na pasku nawigacyjnym kontrolera nawigacji. W tym momencie uruchomiona jest aplikacja przedstawiałoby ekranu poniżej:

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Uruchamianie aplikacji spowoduje wyświetlenie ekranu pokazane tutaj")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

Zobaczmy, jak używać tego elementu MonoTouch.Dialog hierarchicznej struktury `Sections` i `Elements` można dodać więcej ekranów.

### <a name="creating-the-dialog-screens"></a>Tworzenie ekranów w oknie dialogowym

A `DialogViewController` jest `UITableViewController` podklasy używający elementu MonoTouch.Dialog dodawania ekranów. Elementu MonoTouch.Dialog tworzy ekrany, dodając `RootElement` do `DialogViewController`, ponieważ był widoczny powyżej. `RootElement` Może mieć `Section` wystąpień, które reprezentują sekcji tabeli.
Sekcje składają się z elementów, inne sekcje, lub nawet innych `RootElements`. Przez zagnieżdżanie `RootElements`, elementu MonoTouch.Dialog automatycznie tworzy aplikację stylu nawigacji, jak firma Microsoft będzie wyświetlane obok.

### <a name="using-dialogviewcontroller"></a>Za pomocą DialogViewController

`DialogViewController`, Trwa `UITableViewController` ma podklasę, `UITableView` jako jej widok. W tym przykładzie chcemy dodać elementy do tabeli każdorazowo <span class="ui"> + </span> naciśnięcia przycisku. Ponieważ `DialogViewController` została dodana do `UINavigationController`, możemy użyć `NavigationItem`firmy `RightBarButton` właściwości do dodania <span class="ui"> + </span> przycisk, jak pokazano poniżej:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Po utworzeniu `RootElement` wcześniej, firma Microsoft przekazano go pojedynczej `Section` wystąpienia, aby firma Microsoft może dodać elementy jako <span class="ui"> + </span> naciśnięcia przycisku przez użytkownika. Do wykonania w tym w przypadku obsługi dla przycisku, możemy użyć następującego kodu:

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

Ten kod tworzy nową `Task` obiektu każdego naciśnięcia przycisku. Poniżej pokazano proste wdrażanie `Task` klasy:

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

Zadania podrzędnego `Name` właściwość jest używana do tworzenia `RootElement`firmy podpis wraz ze zmienną licznika o nazwie `n` , są zwiększane dla każdego nowego zadania. Elementu MonoTouch.Dialog jest przekształcany elementy wierszy, które są dodawane do `TableView` podczas każdego `taskElement` zostanie dodany.

## <a name="presenting-and-managing-dialog-screens"></a>Prezentowanie i zarządzanie nimi ekrany okna dialogowego

Użyliśmy `RootElement` tak, aby elementu MonoTouch.Dialog spowoduje to automatyczne utworzenie nowego ekranu, aby uzyskać szczegółowe informacje z każdego zadania i przejście do niej, gdy zostanie wybrany wiersz.

Ekran szczegółów zadania, sama składa się z dwóch części; Każda z tych sekcji zawiera jeden element. Pierwszy element jest tworzona na podstawie `EntryElement` zapewnienie można edytować wierszy dla tego zadania `Description` właściwości. Po wybraniu elementu, zostanie wyświetlony klawiatury do edycji tekstu, jak pokazano poniżej:

 [![](elements-api-walkthrough-images/03-create-task.png "Po wybraniu elementu klawiatury do edycji tekstu, zostanie wyświetlony pokazany")](elements-api-walkthrough-images/03-create-task.png#lightbox)

Druga sekcja zawiera `DateElement` pozwala nam zarządzać zadania podrzędnego `DueDate` właściwości. Wybranie daty automatycznie ładuje selektor daty, jak pokazano:

 [![](elements-api-walkthrough-images/04-date-picker.png "Wybranie daty automatycznie ładuje selektor daty, jako")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

W obu `EntryElement` i `DateElement` przypadków (lub dowolnego elementu wprowadzania danych na elementu MonoTouch.Dialog), wszelkie zmiany wartości zostaną automatycznie zachowane. Możemy to wykazać edycji datę oraz kierując się i z powrotem na ekranie głównym i różne szczegóły zadania, w których wartości w ekrany szczegółów są zachowywane.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawione wskazówki, które pokazano, jak za pomocą interfejsu API elementów elementu MonoTouch.Dialog. On objęty podstawowe kroki, aby tworzyć aplikacje wieloma ekranami z patrz hasło MT. D, takich jak używać `DialogViewController` oraz sposób dodawania elementów i sekcji, do tworzenia ekranów. Ponadto go pokazano, jak używać patrz hasło MT. W połączeniu z D `UINavigationController`.

## <a name="related-links"></a>Linki pokrewne

- [MTDWalkthrough (przykład)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [Zrzut ekranu — Miguel de Icaza tworzy ekran logowania dla systemu iOS przy użyciu elementu MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Zrzut ekranu — łatwe tworzenie dla systemu iOS interfejsy użytkownika przy użyciu elementu MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Wprowadzenie do elementu MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Przewodnik interfejsu API odbicia](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Przewodnik elementu JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Okno dialogowe MonoTouch w witrynie Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikacji](https://github.com/migueldeicaza/TweetStation)
- [Informacje o klasach UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Informacje o klasach UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
