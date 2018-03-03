---
title: "Wskazówki: Tworzenie przy użyciu elementu JSON interfejsu użytkownika"
description: "MonoTouch.Dialog (patrz hasło MT. D) obsługuje dynamiczne generowanie interfejsu użytkownika za pomocą danych JSON. W tym samouczku zostanie omówiony sposób użycia JSONElement do tworzenia interfejsu użytkownika z formatu JSON, które jest dołączone do aplikacji, lub ładowane z zdalnego adresu Url."
ms.topic: article
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 63faa0c46abfb509a6834efa647f23ad0ed7f454
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough-using-a-json-element-to-create-a-user-interface"></a>Wskazówki: Tworzenie przy użyciu elementu JSON interfejsu użytkownika

_MonoTouch.Dialog (patrz hasło MT. D) obsługuje dynamiczne generowanie interfejsu użytkownika za pomocą danych JSON. W tym samouczku zostanie omówiony sposób użycia JSONElement do tworzenia interfejsu użytkownika z formatu JSON, które jest dołączone do aplikacji, lub ładowane z zdalnego adresu Url._


PATRZ HASŁO MT. D obsługuje tworzenie interfejsów użytkownika zadeklarowany w formacie JSON. Gdy elementy zadeklarowane za pomocą formatu JSON, patrz hasło MT. D utworzy skojarzonych elementów można automatycznie. JSON mogą być ładowane z pliku lokalnego przeanalizowany `JsonObject` wystąpienia lub nawet zdalnego adresu Url.

PATRZ HASŁO MT. D obsługuje pełny zakres funkcji, które są dostępne w interfejsie API elementy, korzystając z formatu JSON. Na przykład w aplikacji na poniższym zrzucie ekranu przy użyciu całkowicie jest zadeklarowany:

[ ![](json-element-walkthrough-images/01-load-from-file.png "Na przykład aplikacji na tym zrzucie ekranu pokazano całkowicie zadeklarowano przy użyciu") ](json-element-walkthrough-images/01-load-from-file.png) [ ![ ] (json-element-walkthrough-images/02-load-from-file-details.png "na przykład aplikacji na tym zrzucie ekranu pokazano jest całkowicie zadeklarowane za pomocą JSON")](json-element-walkthrough-images/02-load-from-file-details.png)

Umożliwia ponowne przeanalizowanie przykład z [elementów interfejsu API wskazówki](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) Samouczek przedstawiający sposób dodawania ekran szczegółów zadania przy użyciu formatu JSON.

## <a name="json-walkthrough"></a>Wskazówki JSON

Przykładem tego przewodnika pozwala utworzyć zadania. Po wybraniu zadania na pierwszym ekranie zostanie wyświetlony ekran szczegółów, jak pokazano:

 [ ![](json-element-walkthrough-images/03-task-list.png "Po wybraniu zadania na pierwszym ekranie ekranu szczegółów przedstawiono, jak pokazano")](json-element-walkthrough-images/03-task-list.png)

## <a name="creating-the-json"></a>Tworzenie JSON

Na przykład firma Microsoft będzie załadować JSON z pliku w projekcie o nazwie `task.json`. PATRZ HASŁO MT. D oczekuje JSON, aby był zgodny ze składnią, która odzwierciedla elementów interfejsu API. Podobnie jak przy użyciu interfejsu API elementy z kodu, korzystając z formatu JSON, możemy zadeklarować sekcje i wśród tych sekcji dodamy elementów. Aby zadeklarować sekcje i elementów w formacie JSON, używamy ciągów "sekcji" i "elementów" odpowiednio jako klucze. Dla każdego elementu typu skojarzony element ustawić przy użyciu `type` klucza. Nazwa właściwości ustawiono dla każdej właściwości elementów jako klucz.

Na przykład następujące JSON opisano sekcje i elementów dla szczegółów zadań:

```csharp
{
    "title": "Task",
    "sections": [
        {
          "elements" : [
            {
                "id" : "task-description",
                "type": "entry",
                "placeholder": "Enter task description"
            },
            {
                "id" : "task-duedate",
                "type": "date",
                "caption": "Due Date",
                "value": "00:00"
            }
         ]
        }
    ]
  }
```

Powiadomienie powyżej JSON zawiera identyfikator dla każdego elementu. Każdy element może zawierać identyfikator, aby odwołać się w czasie wykonywania. Zajmiemy się tym jak jest on używany w momencie, gdy zostanie przedstawiony sposób załadować JSON w kodzie.

 <a name="Loading_the_JSON_in_Code" />


## <a name="loading-the-json-in-code"></a>Ładowanie JSON w kodzie

Po zdefiniowaniu JSON, musimy załadować do patrz hasło MT. Przy użyciu D `JsonElement` klasy. Zakładając, że plik o JSON, które zostały utworzone powyżej został dodany do projektu o nazwie sample.json i podana Akcja kompilacji zawartości ładowania `JsonElement` jest tak proste, jak wywołanie następującego kodu:

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Ponieważ firma Microsoft jest dodawany na żądanie zawsze się, że zadanie jest tworzone, możemy zmodyfikować przycisk kliknięty z wcześniejszego przykładu elementów interfejsu API w następujący sposób:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

 <a name="Accessing_Elements_at_Runtime" />


## <a name="accessing-elements-at-runtime"></a>Uzyskiwanie dostępu do elementów w czasie wykonywania

Odwołaj się, że dodano identyfikatora do obu elementów podczas został zadeklarowany w pliku JSON. Używamy właściwości id do każdego elementu w czasie wykonywania, aby zmodyfikować jego właściwości w kodzie. Na przykład następujący kod odwołuje się do zapisu i Data elementy do ustawiania wartości z obiektu zadania:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        taskElement.Caption = task.Name;

        var description = taskElement ["task-description"] as EntryElement;

        if (description != null) {
                description.Caption = task.Name;
                description.Value = task.Description;       
        }

        var duedate = taskElement ["task-duedate"] as DateElement;

        if (duedate != null) {                
                duedate.DateValue = task.DueDate;
        }
        _rootElement [0].Add (taskElement);
};
```

 <a name="Loading_JSON_from_a_Url" />


## <a name="loading-json-from-a-url"></a>Ładowanie JSON z adresu Url

PATRZ HASŁO MT. D obsługuje również dynamiczne ładowanie JSON z zewnętrznego adresu Url, przekazując po prostu adres Url do konstruktora obiektu `JsonElement`. PATRZ HASŁO MT. D rozwinie hierarchii zadeklarowany w formacie JSON na żądanie, jak przechodzić między ekranów. Rozważmy na przykład pliku JSON, takie jak poniżej znajduje się w katalogu głównym serwera sieci web lokalnego:

```csharp
{
    "type": "root",
    "title": "home",
    "sections": [
       {
         "header": "Nested view!",
         "elements": [
           {
             "type": "boolean",
             "caption": "Just a boolean",
             "id": "first-boolean",
             "value": false
           },
           {
             "type": "string",
             "caption": "Welcome to the nested controller"
           }
         ]
       }
     ]
}
```

Firma Microsoft może ładować to przy użyciu `JsonElement` zgodnie z poniższym kodem:

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

W czasie wykonywania plik zostanie pobrana i analizowane przez patrz hasło MT. D, gdy użytkownik przechodzi do drugiego widoku, jak pokazano na poniższym zrzucie ekranu:

 [ ![](json-element-walkthrough-images/04-json-web-example.png "Plik zostanie pobrana i analizowane przez patrz hasło MT. D, gdy użytkownik przechodzi do drugiego widoku")](json-element-walkthrough-images/04-json-web-example.png)

 <a name="Summary" />


## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak utworzyć przy użyciu interfejsu z patrz hasło MT. D z formatu JSON. Go pokazano, jak załadować zawarty w pliku z aplikacją, a także z zdalnego adresu Url w formacie JSON. On również pokazano, jak dostęp do elementów, które opisano w formacie JSON w czasie wykonywania.


## <a name="related-links"></a>Linki pokrewne

- [MTDJsonDemo (przykład)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [Tworzy Miguel de Icaza Screencast - ekran logowania dla systemu iOS z MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Screencast — łatwe tworzenie iOS interfejsy użytkownika z MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Wprowadzenie do MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Elementy interfejsu API wskazówki](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Wskazówki interfejsu API odbicia](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Okno dialogowe MonoTouch w witrynie Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikacji](https://github.com/migueldeicaza/TweetStation)
- [Odwołania do klasy UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Odwołania do klasy UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
