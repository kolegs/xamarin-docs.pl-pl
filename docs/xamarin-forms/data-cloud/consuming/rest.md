---
title: "Korzystanie z usługi sieci RESTful Web"
description: "Integracja usługi sieci web do aplikacji jest typowym scenariuszem. W tym artykule pokazano, jak korzystać z usługą sieci web RESTful aplikacji platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: f8b748ad1b57218d1e8aab11bdc1037cf3cfa14c
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="consuming-a-restful-web-service"></a>Korzystanie z usługi sieci RESTful Web

_Integracja usługi sieci web do aplikacji jest typowym scenariuszem. W tym artykule pokazano, jak korzystać z usługą sieci web RESTful aplikacji platformy Xamarin.Forms._

Representational State (Transfer REST) jest architektury stylu do tworzenia usług sieci web. Żądania REST są wysyłane za pośrednictwem protokołu HTTP, przy użyciu tego samego polecenia HTTP, korzystających z przeglądarki sieci web do pobierania strony sieci web i wysyłać dane do serwerów. Zlecenia, które są:

- **Pobierz** — ta operacja jest używana do pobierania danych z usługi sieci web.
- **POST** — ta operacja jest używana do utworzenia nowego elementu danych w usłudze sieci web.
- **Umieść** — ta operacja jest używane do aktualizowania elementu danych w usłudze sieci web.
- **POPRAWKA** — ta operacja jest używane do aktualizowania elementu danych w usłudze sieci web przez opisujące zestaw instrukcji dotyczących jak element powinien być modyfikowany. Tego zlecenia nie jest używany w przykładowej aplikacji.
- **Usuń** — ta operacja jest używana do usuwania elementu danych w usłudze sieci web.

Usługa sieci Web interfejsów API REST jest zgodna są nazywane interfejsy API RESTful i są definiowane przy użyciu:

- Podstawowy identyfikator URI.
- Metody HTTP, takich jak GET, POST, PUT, PATCH lub DELETE.
- Typ nośnika dla danych, takich jak JavaScript Object Notation (JSON).

Usługi sieci web rESTful zazwyczaj używają komunikatów JSON do zwracania danych do klienta. JSON to format wymiany danych tekstowych, że ładunki compact i tworzy, których wynikiem zmniejszyć wymagania dotyczące przepustowości podczas wysyłania danych. Przykładowa aplikacja korzysta z typu open source [NewtonSoft JSON.NET biblioteki](http://www.newtonsoft.com/json) do serializowania i deserializowania wiadomości.

Łatwość REST pomogła była podstawowej metody dostępu do usług sieci web w aplikacjach mobilnych.

Instrukcje dotyczące konfigurowania usługi REST można znaleźć w pliku readme, który towarzyszy przykładowej aplikacji. Jednak po uruchomieniu aplikacji przykładowej, jego połączy do usługi hostowanej platformy Xamarin REST, która umożliwia dostęp tylko do odczytu do danych, jak pokazano na poniższym zrzucie ekranu:

![](rest-images/portal.png "Przykładowa aplikacja")

> [!NOTE]
> W systemie iOS 9 i większości aplikacji zabezpieczeń transportu (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych. Ponieważ ATS jest domyślnie włączone w aplikacje dla systemu iOS 9, wszystkie połączenia będą podlegać ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.
>
>ATS można można korzystać z, jeśli nie jest możliwe użycie **HTTPS** protokołu i zabezpieczania komunikacji dla zasobów w Internecie. Można to osiągnąć przez zaktualizowanie aplikacji **Info.plist** pliku. Aby uzyskać więcej informacji, zobacz [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Korzystanie z usługi sieci Web

Usługa REST jest zapisywany przy użyciu platformy ASP.NET Core i zapewnia następujące operacje:

<table>
  <thead>
    <tr>
      <th>Operacja</th>
      <th>Metoda HTTP</th>
      <th>Względny identyfikator URI</th>
      <th>Parametry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Pobierz listę elementów do wykonania</td>
      <td>GET</td>
      <td>/API/todoitems /</td>
      <td></td>
    </tr>
    <tr>
      <td>Utwórz nowy element zadania do wykonania</td>
      <td>POST</td>
      <td>/API/todoitems /</td>
      <td>W formacie JSON <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>Aktualizuj element do wykonania</td>
      <td>UMIEŚĆ</td>
      <td>/API/todoitems /</td>
      <td>W formacie JSON <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>Usuń element do wykonania</td>
      <td>DELETE</td>
      <td>/API/todoitems / {id}</td>
      <td></td>
    </tr>
  </tbody>
</table>

Większość identyfikatory URI obejmują `TodoItem` identyfikator w ścieżce. Na przykład, aby usunąć `TodoItem` których identyfikator jest `6bb8a868-dba1-4f1a-93b7-24ebce87e243`, klient wysyła żądanie usunięcia `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243`. Aby uzyskać więcej informacji na temat modelu danych używany w przykładowej aplikacji, zobacz [modelowania danych](~/xamarin-forms/data-cloud/walkthrough.md).

Gdy strukturę interfejsu API sieci Web otrzymuje żądanie kieruje żądanie do akcji. Te akcje są po prostu publicznej metody `TodoItemsController` klasy. Platformę korzysta tabeli routingu do ustalenia akcję do wywołania w odpowiedzi na żądanie, które przedstawiono w poniższym przykładzie:

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

Tabela routingu zawiera szablon trasy, i gdy strukturę interfejsu API sieci Web otrzymuje żądanie HTTP, go próbuje dopasować identyfikatora URI dla szablonu trasy w tabeli routingu. Jeśli odpowiadającego mu się, że klient odbierze błąd 404 (nie znaleziono) nie można odnaleźć trasy. Jeśli zostanie znaleziony pasującej trasy, interfejs API sieci Web wybiera kontroler i akcję w następujący sposób:

- Aby odnaleźć kontrolera, interfejsu API sieci Web dodaje "controller" z wartością *{controller}* zmiennej.
- Aby znaleźć akcję, interfejsu API sieci Web sprawdza metodę HTTP i analizuje akcji kontrolera, które są oznaczone jako atrybut tej samej metody HTTP.
- *{Id}* zmiennej symbol zastępczy jest zamapowana do parametru akcji.

Usługa REST korzysta z uwierzytelniania podstawowego. Aby uzyskać więcej informacji, zobacz [uwierzytelniania usługi sieci RESTful web](~/xamarin-forms/data-cloud/authentication/rest.md). Aby uzyskać więcej informacji o routingu ASP.NET Web API, zobacz [routingu na platformie ASP.NET Web API](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) w witrynie sieci Web programu ASP.NET. Aby uzyskać więcej informacji dotyczących tworzenia usługi REST przy użyciu platformy ASP.NET Core, zobacz [tworzenia usługi wewnętrznej bazy danych dla natywnych aplikacji mobilnej](/aspnet/core/mobile/native-mobile-backend/).

> [!NOTE]
> Przykładowa aplikacja korzysta z usługi hostowanej Xamarin REST, który umożliwia dostęp tylko do odczytu do usługi sieci web. W związku z tym operacje, które tworzenie, aktualizowanie i usuwanie danych nie ma wpływu czy dane używane w aplikacji. Jednak jest dostępna w wersji pełnić rolę hosta usługi REST **TodoRESTService** folderu w odpowiednim [przykładowy kod](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/).
> Jeśli na serwerze usługi REST, samodzielnie, pozwala na pełną tworzenia, aktualizacji, do odczytu i usuwania dostęp do danych.

`HttpClient` Klasa jest używana do wysyłania i odbierania żądań za pośrednictwem protokołu HTTP. Zawiera funkcję wysyłania żądań HTTP i odbierania odpowiedzi HTTP z identyfikatora URI zidentyfikowane zasobów. Każde żądanie jest wysyłany jako operację asynchroniczną. Aby uzyskać więcej informacji na temat operacji asynchronicznych, zobacz [Przegląd pomocy technicznej Async](~/cross-platform/platform/async.md).

`HttpResponseMessage` Klasa reprezentuje komunikat odpowiedzi HTTP, otrzymał od usługi sieci web po dokonaniu żądania HTTP. Zawiera informacje o odpowiedzi, w tym kod stanu, nagłówki i wszelkie treści. `HttpContent` Klasa reprezentuje treści HTTP i nagłówków zawartości, takich jak `Content-Type` i `Content-Encoding`. Zawartość może zostać odczytany przy użyciu dowolnego `ReadAs` metod, takich jak `ReadAsStringAsync` i `ReadAsByteArrayAsync`, zależnie od formatu danych.

### <a name="creating-the-httpclient-object"></a>Tworzenie obiektu HTTPClient

`HttpClient` Wystąpienia zadeklarowano na poziomie klasy tak, że obiekt znajduje się na tak długo, jak aplikacja musi żądania HTTP, jak pokazano w poniższym przykładzie:

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    client = new HttpClient ();
    client.MaxResponseContentBufferSize = 256000;
  }
  ...
}
```

`HttpClient.MaxResponseContentBufferSize` Właściwość jest używana, aby określić maksymalną liczbę bajtów do zbuforowania podczas odczytywania zawartość komunikatu odpowiedzi HTTP. Domyślny rozmiar tej właściwości jest maksymalny rozmiar całkowitą. W związku z tym właściwość ma ustawioną wartość mniejszą ze względów bezpieczeństwa, aby ograniczyć ilość danych, która aplikacja będzie akceptować odpowiedzi z usługi sieci web.

### <a name="retrieving-data"></a>Pobieranie danych

`HttpClient.GetAsync` Metoda służy do wysyłania żądania GET do określonego przez identyfikator URI usługi sieci web i będzie otrzymywać odpowiedź z usługi sieci web, jak pokazano w poniższym przykładzie:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));
  ...
  var response = await client.GetAsync (uri);
  if (response.IsSuccessStatusCode) {
      var content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

Kod stanu HTTP są wysyłane przez usługę REST `HttpResponseMessage.IsSuccessStatusCode` właściwości, aby wskazać, czy żądanie HTTP powodzeniem lub niepowodzeniem. Dla tej operacji REST usługi wysyła w odpowiedzi, który wskazuje, że żądanie zakończyło się pomyślnie i że żądane informacje są w odpowiedzi kod stanu HTTP 200 (OK).

Jeśli operacja HTTP zakończyło się pomyślnie, jest do odczytu treści odpowiedzi, do wyświetlenia. `HttpResponseMessage.Content` Właściwość reprezentuje zawartość odpowiedzi HTTP i `HttpContent.ReadAsStringAsync` metoda asynchronicznie zapisuje zawartość HTTP na ciąg. Ta zawartość jest następnie konwertowana z formatu JSON do `List` z `TodoItem` wystąpień.

### <a name="creating-data"></a>Tworzenie danych

`HttpClient.PostAsync` Metoda służy do wysyłania żądania POST do usługi sieci web, określony przez identyfikator URI, a następnie do odbierania odpowiedzi z usługi sieci web, jak pokazano w poniższym przykładzie kodu:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));

  ...
  var json = JsonConvert.SerializeObject (item);
  var content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem) {
    response = await client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully saved.");

  }
  ...
}
```

`TodoItem` Wystąpienia jest konwertowana na ładunek JSON do wysłania do usługi sieci web. Ł adunek zostaje następnie osadzony w treści zawartość HTTP, które zostaną wysłane do usługi sieci web przed żądań z `PostAsync` metody.

Kod stanu HTTP są wysyłane przez usługę REST `HttpResponseMessage.IsSuccessStatusCode` właściwości, aby wskazać, czy żądanie HTTP powodzeniem lub niepowodzeniem. Typowe dla tej operacji są:

- **201 (utworzono)** — żądanie spowodowało zwrócenie nowego zasobu tworzona przed wysłaniem odpowiedzi.
- **400 (nieprawidłowe żądanie)** — żądanie nie jest rozpoznawany przez serwer.
- **409 (konflikt)** — nie można przeprowadzać żądania z powodu konfliktu na serwerze.

### <a name="updating-data"></a>Aktualizowanie danych

`HttpClient.PutAsync` Metoda służy do wysyłania żądania PUT do określonego przez identyfikator URI usługi sieci web i będzie otrzymywać odpowiedź z usługi sieci web, jak pokazano w poniższym przykładzie kodu:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await client.PutAsync (uri, content);
  ...
}
```
Działanie `PutAsync` metoda jest taki sam jak `PostAsync` metodę, która jest używana do tworzenia danych w usłudze sieci web. Jednak inne możliwe odpowiedzi wysyłane przez usługę sieci web.

Kod stanu HTTP są wysyłane przez usługę REST `HttpResponseMessage.IsSuccessStatusCode` właściwości, aby wskazać, czy żądanie HTTP powodzeniem lub niepowodzeniem. Typowe dla tej operacji są:

- **(Brak zawartości) 204** — pomyślnie przetworzył żądania i odpowiedzi jest celowo puste.
- **400 (nieprawidłowe żądanie)** — żądanie nie jest rozpoznawany przez serwer.
- **404 (nie znaleziono)** — żądany zasób nie istnieje na serwerze.

### <a name="deleting-data"></a>Usuwanie danych

`HttpClient.DeleteAsync` Metoda służy do wysyłania żądania usunięcia do określonego przez identyfikator URI usługi sieci web i będzie otrzymywać odpowiedź z usługi sieci web, jak pokazano w poniższym przykładzie:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/{0}
  var uri = new Uri (string.Format (Constants.RestUrl, id));
  ...
  var response = await client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully deleted.");
  }
  ...
}
```

Kod stanu HTTP są wysyłane przez usługę REST `HttpResponseMessage.IsSuccessStatusCode` właściwości, aby wskazać, czy żądanie HTTP powodzeniem lub niepowodzeniem. Typowe dla tej operacji są:

- **(Brak zawartości) 204** — pomyślnie przetworzył żądania i odpowiedzi jest celowo puste.
- **400 (nieprawidłowe żądanie)** — żądanie nie jest rozpoznawany przez serwer.
- **404 (nie znaleziono)** — żądany zasób nie istnieje na serwerze.

## <a name="summary"></a>Podsumowanie

W tym artykule zbadane, jak korzystać z usługi sieci web RESTful, z poziomu aplikacji platformy Xamarin.Forms przy użyciu `HttpClient` klasy. Łatwość REST pomogła była podstawowej metody dostępu do usług sieci web w aplikacjach mobilnych.


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie usług zaplecza dla natywnych aplikacji mobilnych](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
