---
title: Opis próbki
description: W tym temacie przedstawiono wskazówki przykładowej aplikacji platformy Xamarin.Forms, który demonstruje sposób komunikowania się z usługami sieci web inną. Podczas każdej usługi sieci web używa oddzielnych przykładowej aplikacji, są podobne i wspólnych klas.
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 703441e3fc58beeb33e519f3781387a59c1c1cef
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="understanding-the-sample"></a>Opis próbki

_W tym temacie przedstawiono wskazówki przykładowej aplikacji platformy Xamarin.Forms, który demonstruje sposób komunikowania się z usługami sieci web inną. Podczas każdej usługi sieci web używa oddzielnych przykładowej aplikacji, są podobne i wspólnych klas._

Przykładową aplikację listy zadań do wykonania opisanych poniżej umożliwia pokazują, jak można uzyskać dostępu do różnych rodzajów zapleczy usług sieci web z platformy Xamarin.Forms. Udostępnia ona funkcje:

- Wyświetlanie listy zadań.
- Dodawanie, edytowanie i usuwanie zadań.
- Ustaw stan zadania na "gotowe".
- Czytaj pola nazwy i uwagi dotyczące tego zadania.

We wszystkich przypadkach zadania są przechowywane w wewnętrznej bazie danych, który jest dostępny za pośrednictwem usługi sieci web.

Gdy aplikacja jest uruchamiana, zostanie wyświetlona strona zawiera listę wszystkich zadań pobrane z usługi sieci web, która umożliwia użytkownikowi utworzenie nowego zadania. Kliknięcie zadania przechodzi aplikacji do drugiej strony, w którym zadanie może można edytować, zapisane, usunięte i używany. Końcowe aplikacji jest pokazany poniżej:

![](walkthrough-images/app-example-1.png "Aplikacji ToDo — pierwsza strona")
![](walkthrough-images/app-example-2.png "Todo aplikacji — druga strona")

Poszczególne tematy w tym przewodniku Link pobierania do *różnych* wersja aplikacji, który demonstruje określonego typu wewnętrznej bazy danych usługi sieci web. Pobierz odpowiedni przykładowy kod na stronie odnoszących się do każdego stylu usługi sieci web.

## <a name="understanding-the-application-anatomy"></a>Opis Anatomia aplikacji

Projekt PCL dla każdej aplikacji przykładowej składa się z trzech głównych folderów:

|Folder|Cel|
|--- |--- |
|Dane|Zawiera klasy i interfejsy używane do zarządzania elementami danych i komunikować się z usługą sieci web. Co najmniej obejmuje `TodoItemManager` klasy, która jest uwidaczniany za pośrednictwem właściwości w `App` klasy do wywołania operacji usługi sieci web.|
|Modele|Zawiera klasy modelu danych dla aplikacji. Co najmniej obejmuje `TodoItem` klasy, która modele pojedynczego elementu danych używanych przez aplikację. Folder można także ewentualne dodatkowe klasy używane do modelu danych użytkownika.|
|Widoki|Zawiera strony dla aplikacji. Zazwyczaj składa się z `TodoListPage` i `TodoItemPage` klasy i wszystkie dodatkowe klasy używane na potrzeby uwierzytelniania.|

Projekt PCL dla każdej aplikacji również składa się z kilku ważnych plików:

|Plik|Cel|
|--- |--- |
|Constants.CS|`Constants` Klasy, która określa wszelkie stałe używane przez aplikację do komunikacji z usługą sieci web. Te stałe wymagają aktualizacji do uzyskania dostępu do usługi zaplecza osobiste utworzone przez dostawcę.|
|ITextToSpeech.cs|`ITextToSpeech` Interfejs, który określa, że `Speak` metoda musi być dostarczona przez wszystkie klasy implementującej.|
|TODO.CS|`App` Klasy, która odpowiada za tworzenie wystąpień zarówno pierwszej strony wyświetlanej przez aplikację na każdej platformie i `TodoItemManager` klasy, która jest używana do wywołania operacji usługi sieci web.|

### <a name="viewing-pages"></a>Przeglądanie stron

Większość aplikacji przykładowej zawiera co najmniej dwie strony:

- **TodoListPage** — ta strona zostanie wyświetlona lista `TodoItem` wystąpień i ikona znaczników Jeśli `TodoItem.Done` jest właściwość `true`. Kliknięcie elementu przechodzi do `TodoItemPage`. Ponadto można tworzyć nowe elementy, klikając *+* symbolu.
- **TodoItemPage** — ta strona Wyświetla szczegóły wybranego `TodoItem`i umożliwia jego można edytowane, zapisane, usunięte i używany.

Ponadto niektóre przykładowe aplikacje zawierają dodatkowe strony, które są używane do zarządzania procesem uwierzytelniania użytkownika.

### <a name="modeling-the-data"></a>Modelowanie danych

Każdy Przykładowa aplikacja korzysta `TodoItem` klasy do modelu danych wyświetlane i wysyłane do usługi sieci web dla magazynu. Poniższy kod przedstawia przykład `TodoItem` klasy:

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

`ID` Właściwość jest używana do unikatowo identyfikować każdą `TodoItem` wystąpienia i jest używany przez poszczególnych usług sieci web w celu zidentyfikowania danych można zaktualizować lub usunąć.

### <a name="invoking-web-service-operations"></a>Wywoływanie operacji usługi sieci Web

Operacje usług sieci Web są dostępne za pośrednictwem `TodoItemManager` klasy i wystąpienia klasy jest możliwy za pośrednictwem `App.TodoManager` właściwości. `TodoItemManager` Klasa udostępnia następujące metody do wywołania operacji usługi sieci web:

- **GetTasksAsync** — ta metoda służy do wypełniania `ListView` kontrolować na `TodoListPage` z `TodoItem` wystąpień pobrane z usługi sieci web.
- **SaveTaskAsync** — ta metoda służy do tworzenia lub aktualizowania `TodoItem` wystąpienie usługi sieci web.
- **DeleteTaskAsync** — ta metoda służy do usuwania `TodoItem` wystąpienie usługi sieci web.

Ponadto niektóre przykładowe aplikacje zawierają dodatkowe metody w `TodoItemManager` klasy, które są używane do zarządzania procesem uwierzytelniania użytkownika.

Zamiast bezpośrednio wywoływać operacje usługi sieci web `TodoItemManager` metod wywoływania metod dla zależnych klasy, która jest wstrzykuje `TodoItemManager` konstruktora. Na przykład, jeden przykładowej aplikacji injects `RestService` klasy do `TodoItemManager` konstruktora do implementacji, która używa interfejsów API REST uzyskują dostęp do danych.

### <a name="translating-text-to-speech"></a>Tłumaczenie tekstu na mowę

Większość aplikacji przykładowej zawierają tekst na mowę (TTS) funkcji porozmawiać wartości `TodoItem.Name` i `TodoItem.Notes` właściwości. Jest to osiągane przez `OnSpeakActivated` obsługi zdarzeń w `TodoItemPage` klasy, jak pokazano w poniższym przykładzie:

```csharp
void OnSpeakActivated (object sender, EventArgs e)
{
    var todoItem = (TodoItem)BindingContext;
    App.Speech.Speak(todoItem.Name + " " + todoItem.Notes);
}
```

Ta metoda po prostu wywołuje `Speak` metodę, która jest zaimplementowana przez poszczególnych platform `Speech` klasy. Każdy `Speech` klasa implementuje `ITextToSpeech` interfejsu i uruchamiania specyficzne dla platformy kod tworzy wystąpienie `Speech` klasy, która jest możliwy za pośrednictwem `App.Speech` właściwości.

## <a name="summary"></a>Podsumowanie

W tym temacie podano wskazówki przykładowej aplikacji platformy Xamarin.Forms służący do pokazują, jak łączyć się z usługami innej witryny sieci web. Podczas każdej usługi sieci web używa oddzielnych przykładowej aplikacji, wszystkie bazują na tej samej logiki interfejsu użytkownika i business zgodnie z powyższym opisem — różni się tylko w sieci web usługi danych mechanizm magazynowania.


## <a name="related-links"></a>Linki pokrewne

- [Wersja ASMX (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX)
- [Wersja usługi WCF (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF)
- [Wersja REST (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)
- [Wersja platformy Azure (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure)
