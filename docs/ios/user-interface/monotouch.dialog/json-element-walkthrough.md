---
title: Tworzenie interfejsu użytkownika w rozszerzeniu Xamarin.iOS przy użyciu formatu JSON
description: Elementu MonoTouch.Dialog (góra MT.) D) obsługuje dynamiczne generowanie interfejsu użytkownika za pośrednictwem danych JSON. W tym samouczku omówiono sposób używania JSONElement do tworzenia interfejsu użytkownika z formatu JSON, które jest dołączone do aplikacji, lub ładowane z zdalnego adresu Url.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 94cef78bb7eedc03192071f17af765ebb702e260
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038498"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>Tworzenie interfejsu użytkownika w rozszerzeniu Xamarin.iOS przy użyciu formatu JSON

_Elementu MonoTouch.Dialog (góra MT.) D) obsługuje dynamiczne generowanie interfejsu użytkownika za pośrednictwem danych JSON. W tym samouczku omówiono sposób używania JSONElement do tworzenia interfejsu użytkownika z formatu JSON, które jest dołączone do aplikacji, lub ładowane z zdalnego adresu Url._

PATRZ HASŁO MT. D obsługuje tworzenie interfejsów użytkownika, zadeklarowany w formacie JSON. Gdy elementy są deklarowane przy użyciu formatu JSON, patrz hasło MT. D utworzy skojarzonych elementów dla Ciebie automatycznie. Za pomocą pliku JSON można załadować z pliku lokalnego przeanalizowany `JsonObject` wystąpienie lub nawet zdalnego adresu Url.

PATRZ HASŁO MT. D obsługuje pełnego zakresu funkcji, które są dostępne w interfejsie API elementy, korzystając z formatu JSON. Na przykład aplikacji na następującym zrzucie ekranu jest całkowicie zadeklarowany, za pomocą formatu JSON:

[![](json-element-walkthrough-images/01-load-from-file.png "Na przykład aplikacji, w tym zrzucie ekranu jest całkowicie zadeklarowany za pomocą formatu JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox) [![](json-element-walkthrough-images/01-load-from-file.png "na przykład aplikacji, w tym zrzucie ekranu jest całkowicie zadeklarowany za pomocą JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

Wróćmy do przykładu z [elementów interfejsu API wskazówki](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) samouczek, pokazujący sposób dodać ekran szczegółów zadania przy użyciu formatu JSON.

## <a name="setting-up-mtd"></a>Konfigurowanie patrz hasło MT. D

PATRZ HASŁO MT. D jest rozpowszechniana z platformy Xamarin.iOS. Aby go użyć, kliknij prawym przyciskiem myszy **odwołania** węzła Xamarin.iOS projektu w programie Visual Studio 2017 lub Visual Studio dla komputerów Mac i Dodaj odwołanie do **1 elementu MonoTouch.Dialog** zestawu. Następnie należy dodać `using MonoTouch.Dialog` instrukcji w źródle kodu zgodnie z potrzebami.

## <a name="json-walkthrough"></a>Przewodnik JSON

W przykładzie w tym przewodniku pozwala utworzyć zadania. Gdy zadanie jest zaznaczona na pierwszym ekranie, zostanie wyświetlony ekran szczegółów, jak pokazano:

 [![](json-element-walkthrough-images/03-task-list.png "Gdy zadanie jest zaznaczona na pierwszym ekranie, ekran szczegółów zostanie wyświetlony pokazany")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>Tworzenie za pomocą pliku JSON

W tym przykładzie załadujemy plik JSON z pliku w projekcie o nazwie `task.json`. PATRZ HASŁO MT. D oczekuje JSON, aby były zgodne ze składni, która odzwierciedla interfejsu API elementów. Podobnie jak przy użyciu interfejsu API elementów z kodu, korzystając z formatu JSON, firma Microsoft zadeklarować sekcje, a wśród tych sekcji dodamy elementów. Aby zadeklarować sekcje i elementy w formacie JSON, używamy ciągi "sekcji" i "elementów" odpowiednio jako klucze. Dla każdego elementu, typ elementu skojarzonego można ustawić przy użyciu `type` klucza. Dla każdej właściwości elementów ustawiono o nazwie właściwości jako klucz.

Na przykład następujący kod JSON opisano sekcje i elementów, aby uzyskać szczegóły zadania:

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

Ogłoszenie powyższego kodu JSON zawiera identyfikator dla każdego elementu. Każdy element może zawierać identyfikator do odwoływania się do niego w czasie wykonywania. Zobaczymy, jak jest on używany w chwili, gdy samouczków pokazano, jak załadować dane JSON w kodzie.

## <a name="loading-the-json-in-code"></a>Ładowanie za pomocą pliku JSON w kodzie

Po zdefiniowaniu JSON należy załadować je do patrz hasło MT. D przy użyciu `JsonElement` klasy. Zakładając, że plik o JSON utworzone powyżej została dodana do projektu z sample.json nazwy i podana Akcja kompilacji, treści, ładowanie `JsonElement` jest równie proste co wywołanie następujący wiersz kodu:

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Ponieważ dodajemy to na żądanie każdorazowo, że zadanie jest tworzone, możemy zmodyfikować przycisk kliknięto z wcześniejszego przykładu interfejsu API elementów w następujący sposób:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

## <a name="accessing-elements-at-runtime"></a>Uzyskiwanie dostępu do elementów w czasie wykonywania

Pamiętaj, że dodaliśmy identyfikatora do oba te elementy gdy firma Microsoft zadeklarowana w pliku JSON. Używamy właściwości identyfikatora umożliwiają dostęp do każdego elementu w czasie wykonywania, aby zmodyfikować jego właściwości w kodzie. Na przykład poniższy kod odwołuje się do elementów wejścia i datę można ustawić wartości z obiektu zadania:

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

## <a name="loading-json-from-a-url"></a>Trwa ładowanie JSON z adresu url

PATRZ HASŁO MT. D obsługuje również dynamiczne ładowanie JSON z zewnętrznego adresu Url, po prostu przekazując adres Url do konstruktora obiektu `JsonElement`. PATRZ HASŁO MT. D rozwinie hierarchii zadeklarowana w kodzie JSON na żądanie, jak przechodzenie między ekranami. Rozważmy na przykład plik JSON, takich jak poniżej znajduje się w folderze głównym serwera lokalnego w przeglądarce:

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

Firma Microsoft może załadować to przy użyciu `JsonElement` zgodnie z poniższym kodem:

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

W czasie wykonywania plik będą pobierane i analizowane za patrz hasło MT. D, gdy użytkownik przechodzi do drugiego widoku, jak pokazano na poniższym zrzucie ekranu:

 [![](json-element-walkthrough-images/04-json-web-example.png "Plik zostanie pobrana i analizowane za patrz hasło MT. D, gdy użytkownik przechodzi do drugiego widoku")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak utworzyć za pomocą interfejsu o patrz hasło MT. D z formatu JSON. Jego pokazano, jak załadować zawarte w pliku z aplikacją, a także z zdalnego adresu Url w formacie JSON. On również pokazano, jak uzyskać dostęp do elementów opisanych w notacji JSON w czasie wykonywania.

## <a name="related-links"></a>Linki pokrewne

- [MTDJsonDemo (przykład)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [Zrzut ekranu — Miguel de Icaza tworzy ekran logowania dla systemu iOS przy użyciu elementu MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Zrzut ekranu — łatwe tworzenie dla systemu iOS interfejsy użytkownika przy użyciu elementu MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Wprowadzenie do elementu MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Elementy interfejsu API wskazówki](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Przewodnik interfejsu API odbicia](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Okno dialogowe MonoTouch w witrynie Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikacji](https://github.com/migueldeicaza/TweetStation)
- [Informacje o klasach UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Informacje o klasach UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
