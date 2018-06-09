---
title: Korzystanie z usługi sieci Web platformy ASP.NET (ASMX)
description: W tym artykule pokazano, jak korzystać z usługi ASMX SOAP z aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 6ec8168a8da64dbf3dfeb805856a4d91c9ec78ca
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242066"
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>Korzystanie z usługi sieci Web platformy ASP.NET (ASMX)

_ASMX umożliwia tworzenie usług sieci web, które wysyłanie wiadomości przy użyciu obiektu dostępu protokołu SOAP (Simple). SOAP jest protokołem niezależne od platformy i języka umożliwiające tworzenie i uzyskiwanie dostępu do usług sieci web. Korzystającym z usług ASMX nie trzeba niczego wiedzieć o platformie, model obiektów lub język programowania używany do wdrażania usługi. Tylko muszą zrozumieć sposób wysyłania i odbierania wiadomości protokołu SOAP. W tym artykule pokazano, jak korzystać z usługi ASMX SOAP z aplikacji platformy Xamarin.Forms._

Wiadomości SOAP jest dokument XML zawierający następujące elementy:

- Element główny o nazwie *koperty* identyfikującym dokumentu w formacie XML jako wiadomości protokołu SOAP.
- Opcjonalny *nagłówka* element, który zawiera informacje specyficzne dla aplikacji, takie jak dane uwierzytelniania. Jeśli *nagłówka* elementu musi być pierwszym elementem podrzędnym *koperty* elementu.
- Wymaganą *treści* element, który zawiera przeznaczone dla adresata komunikatu protokołu SOAP.
- Opcjonalny *błędów* element, który służy do wskazywania komunikaty o błędach. Jeśli *błędów* element jest obecny, musi być elementem podrzędnym *treści* elementu.

SOAP mogą pracować nad wiele protokołów transportu, w tym HTTP, SMTP, TCP i UDP. Jednak usługa ASMX może wykonywać operacje tylko za pośrednictwem protokołu HTTP. Platformy Xamarin obsługuje standardowe implementacje protokołu SOAP 1.1 za pośrednictwem protokołu HTTP, a to obsługuje wiele standardowych konfiguracji usługi ASMX.

Instrukcje na temat konfigurowania usługi ASMX można znaleźć w pliku readme, który towarzyszy przykładowej aplikacji. Jednak po uruchomieniu aplikacji przykładowej, go połączą się z usługą ASMX hostowanej Xamarin, która umożliwia dostęp tylko do odczytu do danych, jak pokazano na poniższym zrzucie ekranu:

![](asmx-images/portal.png "Przykładowa aplikacja")

> [!NOTE]
> W systemie iOS 9 i większości aplikacji zabezpieczeń transportu (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych. Ponieważ ATS jest domyślnie włączone w aplikacje dla systemu iOS 9, wszystkie połączenia będą podlegać ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.
> ATS można można korzystać z, jeśli nie jest możliwe użycie `HTTPS` protokołu i zabezpieczania komunikacji dla zasobów w Internecie. Można to osiągnąć przez zaktualizowanie aplikacji **Info.plist** pliku. Aby uzyskać więcej informacji, zobacz [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Korzystanie z usługi sieci Web

Usługa ASMX udostępnia następujące operacje:

|Operacja|Opis|Parametry|
|--- |--- |--- |
|GetTodoItems|Pobierz listę elementów do wykonania|
|CreateTodoItem|Utwórz nowy element zadania do wykonania|TodoItem serializacji XML|
|EditTodoItem|Aktualizuj element do wykonania|TodoItem serializacji XML|
|DeleteTodoItem|Usuń element do wykonania|TodoItem serializacji XML|

Aby uzyskać więcej informacji na temat modelu danych używany w aplikacji, zobacz [modelowania danych](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Przykładowa aplikacja zużywa usługa hostowana Xamarin ASMX, która umożliwia dostęp tylko do odczytu do usługi sieci web. W związku z tym operacje, które tworzenie, aktualizowanie i usuwanie danych nie ma wpływu czy dane używane w aplikacji. Jednak jest dostępna w wersji pełnić rolę hosta usługi ASMX **TodoASMXService** folderu w towarzyszący przykładowej aplikacji. Ta wersja pełnić rolę hosta zezwoleń usługi ASMX pełne tworzenia, aktualizacji, do odczytu i usuwania dostęp do danych.

A *proxy* musi zostać wygenerowany w celu korzystania z usługi ASMX, dzięki czemu aplikacji połączyć się z usługą. Serwer proxy jest tworzony przez odbierającą metadanych usługi definiujący metody i skojarzona usługa konfiguracji. Te metadane jest widoczna w formularzu sieci Web Services Description Language (WSDL) dokument, który jest generowany przez usługę sieci web. Serwer proxy jest konstruowany przez dodawanie odwołania sieci web dla usługi sieci web do projektów specyficzne dla platformy.

Klasy wygenerowany serwer proxy, podaj metody służący do konsumowania usługi sieci web, które używają wzorca projektowego asynchronicznego programowania modelu (APM). W tym wzorcu operacji asynchronicznej jest zaimplementowany jako dwie metody o nazwie *BeginOperationName* i *EndOperationName*, który rozpoczęcia i zakończenia operacji asynchronicznej.

*BeginOperationName* metody rozpoczyna operację asynchroniczną i zwraca obiekt, który implementuje `IAsyncResult` interfejsu. Po wywołaniu *BeginOperationName*, aplikacja może kontynuować wykonywania instrukcji w wątku wywołującym, gdy operacja asynchroniczna odbywa się w wątku puli wątków.

Dla każdego wywołania *BeginOperationName*, również powinny wywoływać aplikacji *EndOperationName* Aby uzyskać wyniki operacji. Wartość zwracana *EndOperationName* jest ten sam typ zwracany przez metodę usługi sieci web synchronicznego. Na przykład `EndGetTodoItems` metoda zwraca kolekcję `TodoItem` wystąpień. *EndOperationName* zawiera również metody `IAsyncResult` parametr, który powinien być ustawiony na wystąpienie zwrócony przez odpowiedniego wywołania *BeginOperationName* metody.

Zadania biblioteki równoległych (TPL) może uprościć proces zużywa pary metod begin/end APM hermetyzując operacji asynchronicznych w tym samym `Task` obiektu. Ta hermetyzacja są dostarczane przez wielu przeładowań `TaskFactory.FromAsync` metody.

Aby uzyskać więcej informacji na temat APM zobacz [Model programowania asynchronicznego](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) i [TPL i tradycyjnych .NET Framework asynchronicznego programowania](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) w witrynie MSDN.

### <a name="creating-the-todoservice-object"></a>Tworzenie obiektu TodoService

Udostępnia klasy wygenerowany serwer proxy `TodoService` klasy, która jest używana do komunikacji z usługą ASMX za pośrednictwem protokołu HTTP. Zawiera funkcję do wywoływania metody usługi sieci web, wystąpienie usługi określonych operacji asynchronicznych z identyfikatora URI. Aby uzyskać więcej informacji na temat operacji asynchronicznych, zobacz [Przegląd pomocy technicznej Async](~/cross-platform/platform/async.md).

`TodoService` Wystąpienia zadeklarowano na poziomie klasy tak, że obiekt znajduje się na tak długo, jak aplikacja musi korzystać z usługi ASMX, jak pokazano w poniższym przykładzie kodu:

```csharp
public class SoapService : ISoapService
{
  ASMXService.TodoService asmxService;
  ...

  public SoapService ()
  {
    asmxService = new ASMXService.TodoService (Constants.SoapUrl);
  }
  ...
}
```

`TodoService` Konstruktor przyjmuje parametr opcjonalny ciąg, który określa adres URL wystąpienia usługi ASMX. Umożliwia aplikacji nawiązać połączenia z różnymi wystąpieniami usługi ASMX, pod warunkiem, że istnieje wiele wystąpień opublikowane.

### <a name="creating-data-transfer-objects"></a>Tworzenie obiektów transferu danych

Przykładowa aplikacja korzysta z `TodoItem` klasy do modelu danych. Do przechowywania `TodoItem` elementu w usłudze sieci web, muszą najpierw zostać przekonwertowane na serwer proxy generowany `TodoItem` typu. Jest to osiągane przez `ToASMXServiceTodoItem` metody, jak pokazano w poniższym przykładzie:

```csharp
ASMXService.TodoItem ToASMXServiceTodoItem (TodoItem item)
{
  return new ASMXService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Ta metoda po prostu tworzy nowe `ASMService.TodoItem` wystąpienie i ustawia dla właściwości identyczne z każdej właściwości `TodoItem` wystąpienia.

Podobnie, gdy dane są pobierane z usługi sieci web, należy go przekonwertować z serwera proxy generowany `TodoItem` typ `TodoItem` wystąpienia. Jest to realizowane przy użyciu `FromASMXServiceTodoItem` metody, jak pokazano w poniższym przykładzie:

```csharp
static TodoItem FromASMXServiceTodoItem (ASMXService.TodoItem item)
{
  return new TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Ta metoda po prostu pobiera dane z serwera proxy generowany `TodoItem` wpisz i ustawia go w nowo utworzonej `TodoItem` wystąpienia.

### <a name="retrieving-data"></a>Pobieranie danych

`TodoService.BeginGetTodoItems` i `TodoService.EndGetTodoItems` metody są używane do wywoływania `GetTodoItems` operacji udostępniony przez usługę sieci web. Te metody asynchroniczne są hermetyzowane w `Task` obiektów, jak pokazano w poniższym przykładzie:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromASMXServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` Metoda tworzy `Task` , który jest wykonywany `TodoService.EndGetTodoItems` raz — metoda `TodoService.BeginGetTodoItems` metody zakończeniu z `null` parametr wskazujący, że żadne dane nie jest przekazywany do `BeginGetTodoItems` delegowanie. Na koniec wartości `TaskCreationOptions` wyliczenie Określa, czy domyślne zachowanie dla tworzenia i uruchamiania zadań ma być używany.

`TodoService.EndGetTodoItems` Metoda zwraca tablicę `ASMXService.TodoItem` wystąpienia, które jest następnie konwertowana na `List` z `TodoItem` wystąpień do wyświetlenia.

### <a name="creating-data"></a>Tworzenie danych

`TodoService.BeginCreateTodoItem` i `TodoService.EndCreateTodoItem` metody są używane do wywoływania `CreateTodoItem` operacji udostępniony przez usługę sieci web. Te metody asynchroniczne są hermetyzowane w `Task` obiektów, jak pokazano w poniższym przykładzie:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Metoda tworzy `Task` , który jest wykonywany `TodoService.EndCreateTodoItem` raz — metoda `TodoService.BeginCreateTodoItem` metody zakończeniu z `todoItem` trwa danych, która została przekazana do parametru `BeginCreateTodoItem` pełnomocnika, aby określić `TodoItem` ma zostać utworzony przez usługę sieci web. Na koniec wartości `TaskCreationOptions` wyliczenie Określa, czy domyślne zachowanie dla tworzenia i uruchamiania zadań ma być używany.

Zwraca usługi sieci web `SoapException` Jeśli go nie powiedzie się utworzyć `TodoItem`, który jest obsługiwany przez aplikację.

### <a name="updating-data"></a>Aktualizowanie danych

`TodoService.BeginEditTodoItem` i `TodoService.EndEditTodoItem` metody są używane do wywoływania `EditTodoItem` operacji udostępniony przez usługę sieci web. Te metody asynchroniczne są hermetyzowane w `Task` obiektów, jak pokazano w poniższym przykładzie:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Metoda tworzy `Task` , który jest wykonywany `TodoService.EndEditTodoItem` raz — metoda `TodoService.BeginCreateTodoItem` metody zakończeniu z `todoItem` trwa danych, która została przekazana do parametru `BeginEditTodoItem` pełnomocnika, aby określić `TodoItem` zostać zaktualizowane przez usługę sieci web. Na koniec wartości `TaskCreationOptions` wyliczenie Określa, czy domyślne zachowanie dla tworzenia i uruchamiania zadań ma być używany.

Zwraca usługi sieci web `SoapException` Jeśli go nie może zlokalizować lub zaktualizować `TodoItem`, który jest obsługiwany przez aplikację.

### <a name="deleting-data"></a>Usuwanie danych

`TodoService.BeginDeleteTodoItem` i `TodoService.EndDeleteTodoItem` metody są używane do wywoływania `DeleteTodoItem` operacji udostępniony przez usługę sieci web. Te metody asynchroniczne są hermetyzowane w `Task` obiektów, jak pokazano w poniższym przykładzie:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Metoda tworzy `Task` , który jest wykonywany `TodoService.EndDeleteTodoItem` raz — metoda `TodoService.BeginDeleteTodoItem` metody zakończeniu z `id` trwa danych, która została przekazana do parametru `BeginDeleteTodoItem` pełnomocnika, aby określić `TodoItem` do usunięcia przez usługę sieci web. Na koniec wartości `TaskCreationOptions` wyliczenie Określa, czy domyślne zachowanie dla tworzenia i uruchamiania zadań ma być używany.

Zwraca usługi sieci web `SoapException` Jeśli go nie może zlokalizować lub usunąć `TodoItem`, który jest obsługiwany przez aplikację.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób wykorzystywania usługi sieci web ASMX z aplikacji platformy Xamarin.Forms. ASMX zapewnia możliwość tworzenia usług sieci web, który wysyła wiadomości za pośrednictwem protokołu HTTP przy użyciu protokołu SOAP. Korzystającym z usług ASMX nie trzeba niczego wiedzieć o platformie, model obiektów lub język programowania używany do wdrażania usługi. Tylko muszą zrozumieć sposób wysyłania i odbierania wiadomości protokołu SOAP.


## <a name="related-links"></a>Linki pokrewne

- [TodoASMX (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
