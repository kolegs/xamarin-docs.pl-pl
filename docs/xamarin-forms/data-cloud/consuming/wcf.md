---
title: Korzystanie z usługi sieci Web Windows Communication Foundation (WCF)
description: W tym artykule pokazano, jak korzystać z usługi WCF obiektu dostępu protokołu SOAP (Simple) z aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 05092a3648ac4c37dfd8d712184176a544979ede
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241207"
---
# <a name="consuming-a-windows-communication-foundation-wcf-web-service"></a>Korzystanie z usługi sieci Web Windows Communication Foundation (WCF)

_WCF jest strukturą ujednoliconego firmy Microsoft do tworzenia aplikacji korzystających z usług. Umożliwia ona deweloperom tworzenie bezpieczne, niezawodne, transakcyjne i interoperacyjne aplikacji rozproszonej. W tym artykule pokazano, jak korzystać z usługi WCF obiektu dostępu protokołu SOAP (Simple) z aplikacji platformy Xamarin.Forms._

Usługi WCF zawiera opis usługi z wieloma różnymi umowami, takich jak:

- **Kontrakty danych** — zdefiniuj struktury danych, które stanowią podstawę dla treści wiadomości.
- **Kontrakty komunikatu** — redagowanie wiadomości z istniejących kontraktów danych.
- **Odporność kontrakty** — Zezwalaj na niestandardowe błędach SOAP, należy określić.
- **Umowy o świadczenie usług** — Określ operacje, które obsługują usługi i komunikaty wymagane do interakcji z każdej operacji. Określają również zachowanie żadnych błędów niestandardowych, które mogą być skojarzone z operacjami w każdej usłudze.

Istnieją różnice między usług sieci Web platformy ASP.NET (ASMX) i WCF, ale ważne jest, aby zrozumieć, że WCF obsługuje te same możliwości udostępniające ASMX — wiadomości SOAP za pośrednictwem protokołu HTTP. Aby uzyskać więcej informacji na temat konsumowania usługi ASMX, zobacz [korzystanie z platformy ASP.NET sieci Web usług (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

Ogólnie rzecz biorąc platformy Xamarin obsługuje samej podzbiór po stronie klienta WCF, który jest dostarczany z środowisko uruchomieniowe Silverlight. Obejmuje to najczęściej używane kodowanie i protokół implementacje WCF — transportu kodowany tekst wiadomości SOAP za pośrednictwem protokołu HTTP przy użyciu protokołu `BasicHttpBinding` klasy. Ponadto obsługa usług WCF wymaga użycia narzędzia dostępne tylko w środowisku systemu Windows, aby wygenerować serwera proxy.

Instrukcje dotyczące konfigurowania usługi WCF można znaleźć w pliku readme, który towarzyszy przykładowej aplikacji. Jednak po uruchomieniu aplikacji przykładowej go połączą się z usługą WCF hostowanej Xamarin, która umożliwia dostęp tylko do odczytu do danych, jak pokazano na poniższym zrzucie ekranu:

![](wcf-images/portal.png "Przykładowa aplikacja")

> [!NOTE]
> W systemie iOS 9 i większości aplikacji zabezpieczeń transportu (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych. Ponieważ ATS jest domyślnie włączone w aplikacje dla systemu iOS 9, wszystkie połączenia będą podlegać ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.
> ATS można można korzystać z, jeśli nie jest możliwe użycie `HTTPS` protokołu i zabezpieczania komunikacji dla zasobów w Internecie. Można to osiągnąć przez zaktualizowanie aplikacji **Info.plist** pliku. Aby uzyskać więcej informacji, zobacz [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Korzystanie z usługi sieci Web

Usługa WCF oferuje następujące operacje:

|Operacja|Opis|Parametry|
|--- |--- |--- |
|GetTodoItems|Pobierz listę elementów do wykonania|
|CreateTodoItem|Utwórz nowy element zadania do wykonania|TodoItem serializacji XML|
|EditTodoItem|Aktualizuj element do wykonania|TodoItem serializacji XML|
|DeleteTodoItem|Usuń element do wykonania|TodoItem serializacji XML|

Aby uzyskać więcej informacji na temat modelu danych używany w aplikacji, zobacz [modelowania danych](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Przykładowa aplikacja korzysta z usługi WCF hostowanej Xamarin, która umożliwia dostęp tylko do odczytu do usługi sieci web. W związku z tym operacje, które tworzenie, aktualizowanie i usuwanie danych nie ma wpływu czy dane używane w aplikacji. Jednak jest dostępna w wersji pełnić rolę hosta usługi ASMX **TodoWCFService** folderu w towarzyszący przykładowej aplikacji. Ta wersja pełnić rolę hosta zezwoleń usługi WCF pełna tworzenia, aktualizacji, do odczytu i usuwania dostęp do danych.

A *proxy* musi zostać wygenerowany użycie usługi WCF, dzięki czemu aplikacji połączyć się z usługą. Serwer proxy jest tworzony przez odbierającą metadanych usługi definiują metody i skojarzona usługa konfiguracji. Te metadane jest widoczna w formularzu sieci Web Services Description Language (WSDL) dokument, który jest generowany przez usługę sieci web. Serwer proxy mogą być tworzone przy użyciu dostawcy odwołanie usług sieci Web WCF firmy Microsoft w programie Visual Studio 2017 można dodać odwołania do usługi dla usługi sieci web do biblioteki .NET Standard. Zamiast tworzenia proxy przy użyciu dostawcy odwołanie usług sieci Web WCF firmy Microsoft w programie Visual Studio 2017 jest za pomocą narzędzia narzędzie metadanych elementu ServiceModel (svcutil.exe). Aby uzyskać więcej informacji, zobacz [narzędzie narzędzia metadanych elementu ServiceModel (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Klasy wygenerowany serwer proxy podania metod służący do konsumowania usługi sieci web, które korzystają z wzorca projektowego asynchronicznego programowania modelu (APM). W tym wzorcu operacji asynchronicznej jest zaimplementowany jako dwie metody o nazwie *BeginOperationName* i *EndOperationName*, który rozpoczęcia i zakończenia operacji asynchronicznej.

*BeginOperationName* metody rozpoczyna operację asynchroniczną i zwraca obiekt, który implementuje `IAsyncResult` interfejsu. Po wywołaniu *BeginOperationName*, aplikacja może kontynuować wykonywania instrukcji w wątku wywołującym, gdy operacja asynchroniczna odbywa się w wątku puli wątków.

Dla każdego wywołania *BeginOperationName*, również powinny wywoływać aplikacji *EndOperationName* Aby uzyskać wyniki operacji. Wartość zwracana *EndOperationName* jest ten sam typ zwracany przez metodę usługi sieci web synchronicznego. Na przykład `EndGetTodoItems` metoda zwraca kolekcję `TodoItem` wystąpień. *EndOperationName* zawiera również metody `IAsyncResult` parametr, który powinien być ustawiony na wystąpienie zwrócony przez odpowiedniego wywołania *BeginOperationName* metody.

Zadania biblioteki równoległych (TPL) może uprościć proces zużywa pary metod begin/end APM hermetyzując operacji asynchronicznych w tym samym `Task` obiektu. Ta hermetyzacja są dostarczane przez wielu przeładowań `TaskFactory.FromAsync` metody.

Aby uzyskać więcej informacji na temat APM zobacz [Model programowania asynchronicznego](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) i [TPL i tradycyjnych .NET Framework asynchronicznego programowania](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) w witrynie MSDN.

### <a name="creating-the-todoserviceclient-object"></a>Tworzenie obiektu TodoServiceClient

Udostępnia klasy wygenerowany serwer proxy `TodoServiceClient` klasy, która jest używana do komunikacji z usługą WCF za pośrednictwem protokołu HTTP. Zawiera funkcję do wywoływania metody usługi sieci web, wystąpienie usługi określonych operacji asynchronicznych z identyfikatora URI. Aby uzyskać więcej informacji na temat operacji asynchronicznych, zobacz [Przegląd pomocy technicznej Async](~/cross-platform/platform/async.md).

`TodoServiceClient` Wystąpienia zadeklarowano na poziomie klasy tak, że obiekt znajduje się na tak długo, jak aplikacja musi korzystać z usługi WCF, jak pokazano w poniższym przykładzie:

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

`TodoServiceClient` Wystąpienie jest skonfigurowane z powiązaniem informacji i adresu punktu końcowego. Powiązanie służy do określania transportu, kodowanie i szczegóły protokołu wymagane dla aplikacji i usług komunikować się ze sobą. `BasicHttpBinding` Określa, że kodowany tekst wiadomości SOAP będą wysyłane za pośrednictwem protokołu transportu HTTP. Określanie adresu punktu końcowego umożliwia aplikacjom nawiązać połączenia z różnymi wystąpieniami usługi WCF, pod warunkiem, że istnieje wiele wystąpień opublikowane.

Aby uzyskać więcej informacji o konfigurowaniu odwołania do usługi, zobacz [Konfigurowanie odwołania do usługi](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="creating-data-transfer-objects"></a>Tworzenie obiektów transferu danych

Przykładowa aplikacja korzysta z `TodoItem` klasy do modelu danych. Do przechowywania `TodoItem` elementu w usłudze sieci web, muszą najpierw zostać przekonwertowane na serwer proxy generowany `TodoItem` typu. Jest to osiągane przez `ToWCFServiceTodoItem` metody, jak pokazano w poniższym przykładzie:

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Ta metoda po prostu tworzy nowe `TodoWCFService.TodoItem` wystąpienie i ustawia dla właściwości identyczne z każdej właściwości `TodoItem` wystąpienia.

Podobnie, gdy dane są pobierane z usługi sieci web, należy go przekonwertować z serwera proxy generowany `TodoItem` typ `TodoItem` wystąpienia. Jest to realizowane przy użyciu `FromWCFServiceTodoItem` metody, jak pokazano w poniższym przykładzie:

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
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

`TodoServiceClient.BeginGetTodoItems` i `TodoServiceClient.EndGetTodoItems` metody są używane do wywoływania `GetTodoItems` operacji udostępniony przez usługę sieci web. Te metody asynchroniczne są hermetyzowane w `Task` obiektów, jak pokazano w poniższym przykładzie:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` Metoda tworzy `Task` , który jest wykonywany `TodoServiceClient.EndGetTodoItems` raz — metoda `TodoServiceClient.BeginGetTodoItems` metody zakończeniu z `null` parametr wskazujący, że żadne dane nie jest przekazywany do `BeginGetTodoItems` delegowanie. Na koniec wartości `TaskCreationOptions` wyliczenie Określa, czy domyślne zachowanie dla tworzenia i uruchamiania zadań ma być używany.

`TodoServiceClient.EndGetTodoItems` Metoda zwraca `ObservableCollection` z `TodoWCFService.TodoItem` wystąpienia, które jest następnie konwertowana na `List` z `TodoItem` wystąpień do wyświetlenia.

### <a name="creating-data"></a>Tworzenie danych

`TodoServiceClient.BeginCreateTodoItem` i `TodoServiceClient.EndCreateTodoItem` metody są używane do wywoływania `CreateTodoItem` operacji udostępniony przez usługę sieci web. Te metody asynchroniczne są hermetyzowane w `Task` obiektów, jak pokazano w poniższym przykładzie:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Metoda tworzy `Task` , który jest wykonywany `TodoServiceClient.EndCreateTodoItem` raz — metoda `TodoServiceClient.BeginCreateTodoItem` metody zakończeniu z `todoItem` trwa danych, która została przekazana do parametru `BeginCreateTodoItem` pełnomocnika, aby określić `TodoItem` ma zostać utworzony przez usługę sieci web. Na koniec wartości `TaskCreationOptions` wyliczenie Określa, czy domyślne zachowanie dla tworzenia i uruchamiania zadań ma być używany.

Zwraca usługi sieci web `FaultException` Jeśli go nie powiedzie się utworzyć `TodoItem`, który jest obsługiwany przez aplikację.

### <a name="updating-data"></a>Aktualizowanie danych

`TodoServiceClient.BeginEditTodoItem` i `TodoServiceClient.EndEditTodoItem` metody są używane do wywoływania `EditTodoItem` operacji udostępniony przez usługę sieci web. Te metody asynchroniczne są hermetyzowane w `Task` obiektów, jak pokazano w poniższym przykładzie:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Metoda tworzy `Task` , który jest wykonywany `TodoServiceClient.EndEditTodoItem` raz — metoda `TodoServiceClient.BeginCreateTodoItem` metody zakończeniu z `todoItem` trwa danych, która została przekazana do parametru `BeginEditTodoItem` pełnomocnika, aby określić `TodoItem` zostać zaktualizowane przez usługę sieci web. Na koniec wartości `TaskCreationOptions` wyliczenie Określa, czy domyślne zachowanie dla tworzenia i uruchamiania zadań ma być używany.

Zwraca usługi sieci web `FaultException` Jeśli go nie może zlokalizować lub zaktualizować `TodoItem`, który jest obsługiwany przez aplikację.

### <a name="deleting-data"></a>Usuwanie danych

`TodoServiceClient.BeginDeleteTodoItem` i `TodoServiceClient.EndDeleteTodoItem` metody są używane do wywoływania `DeleteTodoItem` operacji udostępniony przez usługę sieci web. Te metody asynchroniczne są hermetyzowane w `Task` obiektów, jak pokazano w poniższym przykładzie:

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

`Task.Factory.FromAsync` Metoda tworzy `Task` , który jest wykonywany `TodoServiceClient.EndDeleteTodoItem` raz — metoda `TodoServiceClient.BeginDeleteTodoItem` metody zakończeniu z `id` trwa danych, która została przekazana do parametru `BeginDeleteTodoItem` pełnomocnika, aby określić `TodoItem` do usunięcia przez usługę sieci web. Na koniec wartości `TaskCreationOptions` wyliczenie Określa, czy domyślne zachowanie dla tworzenia i uruchamiania zadań ma być używany.

Zwraca usługi sieci web `FaultException` Jeśli go nie może zlokalizować lub usunąć `TodoItem`, który jest obsługiwany przez aplikację.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób wykorzystywania usługi WCF SOAP z aplikacji platformy Xamarin.Forms. Ogólnie rzecz biorąc platformy Xamarin obsługuje samej podzbiór po stronie klienta WCF, który jest dostarczany z środowisko uruchomieniowe Silverlight. Obejmuje to najczęściej używane kodowanie i protokół implementacje WCF — transportu kodowany tekst wiadomości SOAP za pośrednictwem protokołu HTTP przy użyciu protokołu `BasicHttpBinding` klasy. Ponadto obsługa usług WCF wymaga użycia narzędzia dostępne tylko w środowisku systemu Windows, aby wygenerować serwera proxy.


## <a name="related-links"></a>Linki pokrewne

- [TodoWCF (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
