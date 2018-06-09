---
title: Korzystanie z bazą danych dokumentów platformy Azure rozwiązania Cosmos bazy danych
description: W tym artykule opisano sposób korzystania z rozwiązania Cosmos Azure DB .NET Standard biblioteki klienta do integracji z bazą danych dokumentów bazy danych Azure rozwiązania Cosmos w aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 6e797eaad98f6fac66876aaebecd7ae53ad9dbab
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242508"
---
# <a name="consuming-an-azure-cosmos-db-document-database"></a>Korzystanie z bazą danych dokumentów platformy Azure rozwiązania Cosmos bazy danych

_Baza danych dokumentów bazy danych Azure rozwiązania Cosmos jest bazą danych NoSQL, który zapewnia małe opóźnienia dostępu do dokumentów JSON, oferty usługi szybkie, wysokiej dostępności i skalowalności bazy danych dla aplikacji, które wymagają bezproblemowego skalowania i globalnej replikacji. W tym artykule opisano sposób korzystania z rozwiązania Cosmos Azure DB .NET Standard biblioteki klienta do integracji z bazą danych dokumentów bazy danych Azure rozwiązania Cosmos w aplikacji platformy Xamarin.Forms._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure rozwiązania Cosmos bazy danych, przez [Xamarin University](https://university.xamarin.com/)**

Konto bazy danych dokumentów bazy danych Azure rozwiązania Cosmos można alokować przy użyciu subskrypcji platformy Azure. Każde konto bazy danych może mieć zero lub więcej baz danych. Bazy danych dokumentów w usłudze Azure DB rozwiązania Cosmos jest kontenerem logicznym dla kolekcji dokumentów i użytkowników.

Bazy danych Azure DB rozwiązania Cosmos dokumentu może zawierać zero lub więcej kolekcji dokumentów. Każda kolekcja dokumentu może mieć poziomu wydajności różnych, dzięki czemu więcej przepustowości może być określony dla często używanych kolekcje i mniej przepływność dla kolekcji rzadko używane.

Każda kolekcja dokument składa się z zero lub więcej dokumentów JSON. Dokumenty w kolekcji są bez schematu, a więc nie trzeba udostępniać tej samej struktury lub pól. Gdy dokumenty są dodawane do kolekcji dokumentów, DB rozwiązania Cosmos automatycznie indeksuje je i będą one dostępne do zbadania.

Do celów programistycznych dokumentów bazy danych mogą być używane przez emulator. Przy użyciu emulatora, aplikacje można opracowany i przetestowany lokalnie, bez tworzenia subskrypcji platformy Azure lub ponoszenia kosztów. Aby uzyskać więcej informacji o emulatorze, zobacz [lokalnie programowania z użyciem emulatora usługi Azure DB rozwiązania Cosmos](/azure/cosmos-db/local-emulator/).

W tym artykule i towarzyszące przykładowej aplikacji przedstawiono aplikację listy Todo, gdzie zadania są przechowywane w bazie danych dokumentów bazy danych Azure rozwiązania Cosmos. Aby uzyskać więcej informacji na temat przykładowej aplikacji, zobacz [opis próbki](~/xamarin-forms/data-cloud/walkthrough.md).

Aby uzyskać więcej informacji na temat bazy danych Azure rozwiązania Cosmos, zobacz [Azure rozwiązania Cosmos DB dokumentacji](/azure/cosmos-db/).

## <a name="setup"></a>Konfiguracja

Integrowanie bazą danych dokumentów bazy danych Azure rozwiązania Cosmos w aplikacji platformy Xamarin.Forms proces wygląda następująco:

1. Utwórz konto DB rozwiązania Cosmos. Aby uzyskać więcej informacji, zobacz [utworzyć konto bazy danych Azure rozwiązania Cosmos](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. Dodaj [biblioteki klienta usługi Azure rozwiązania Cosmos DB .NET Standard](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) pakiet NuGet do projektów platformy w rozwiązaniu platformy Xamarin.Forms.
1. Dodaj `using` dyrektywy `Microsoft.Azure.Documents`, `Microsoft.Azure.Documents.Client`, i `Microsoft.Azure.Documents.Linq` obszarów nazw do klasy, które będą uzyskiwać dostęp do konta DB rozwiązania Cosmos.

Po wykonaniu tych kroków, można skonfigurować i wykonywać żądania w bazie danych dokumentów bibliotece klienta usługi Azure rozwiązania Cosmos DB .NET Standard.

> [!NOTE]
> Rozwiązania Cosmos Azure DB .NET Standard biblioteki klienta można zainstalować tylko do projektów platformy, a nie w projekcie przenośnej biblioteki klasy (PCL). W związku z tym Przykładowa aplikacja jest udostępnione dostępu do projektu (SAP) aby uniknąć zduplikowania kodu. Jednak `DependencyService` klasa może być używana w projekcie PCL do wywołania rozwiązania Cosmos Azure DB .NET Standard kod biblioteki klienta zawartych w projektach specyficzne dla platformy.

## <a name="consuming-the-azure-cosmos-db-account"></a>Korzystanie z konta bazy danych Azure rozwiązania Cosmos

`DocumentClient` Typu hermetyzuje punktu końcowego, poświadczeń i zasady połączenia używane do uzyskania dostępu do konta bazy danych rozwiązania Cosmos Azure i służy do konfigurowania i wykonywać żądania względem konta. Poniższy przykładowy kod przedstawia sposób tworzenia wystąpienia tej klasy:

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

Należy podać rozwiązania Cosmos DB Uri oraz primary key, aby `DocumentClient` konstruktora. Można je uzyskać z portalu Azure. Aby uzyskać więcej informacji, zobacz [Połącz z kontem usługi Azure DB rozwiązania Cosmos](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect).

### <a name="creating-a-database"></a>Tworzenie bazy danych

Dokument bazy danych jest kontenerem logicznym dla kolekcji dokumentów i użytkowników i może być utworzone w portalu Azure lub programistycznie przy użyciu `DocumentClient.CreateDatabaseIfNotExistsAsync` metody:

```csharp
public async Task CreateDatabase(string databaseName)
{
  ...
  await client.CreateDatabaseIfNotExistsAsync(new Database
  {
    Id = databaseName
  });
  ...
}
```

`CreateDatabaseIfNotExistsAsync` Określa metodę `Database` obiekt jako argumentu z `Database` obiektu, określając nazwę bazy danych jako jego `Id` właściwości. `CreateDatabaseIfNotExistsAsync` Metoda tworzy bazy danych, jeśli nie istnieje lub przywraca bazę danych, jeśli już istnieje. Jednak przykładowej aplikacji ignoruje wszystkie dane zwrócone przez `CreateDatabaseIfNotExistsAsync` metody.

> [!NOTE]
> `CreateDatabaseIfNotExistsAsync` Metoda zwraca `Task<ResourceResponse<Database>>` obiekt i kod stanu odpowiedzi można sprawdzić czy baza danych została utworzona, czy istniejąca baza danych została zwrócona.

### <a name="creating-a-document-collection"></a>Trwa tworzenie kolekcji dokumentów

Kolekcja dokumentów jest kontenerem dokumentów JSON i może być utworzone w portalu Azure lub programistycznie przy użyciu `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` metody:

```csharp
public async Task CreateDocumentCollection(string databaseName, string collectionName)
{
  ...
  // Create collection with 400 RU/s
  await client.CreateDocumentCollectionIfNotExistsAsync(
    UriFactory.CreateDatabaseUri(databaseName),
    new DocumentCollection
    {
      Id = collectionName
    },
    new RequestOptions
    {
      OfferThroughput = 400
    });
  ...
}
```

`CreateDocumentCollectionIfNotExistsAsync` Metoda wymaga dwóch argumentów obowiązkowych — Nazwa bazy danych jest określony jako `Uri`, a `DocumentCollection` obiektu. `DocumentCollection` Obiekt reprezentuje kolekcję dokument, którego nazwa jest określona z `Id` właściwości. `CreateDocumentCollectionIfNotExistsAsync` Metoda tworzy kolekcji dokumentów, jeśli nie istnieje lub zwraca kolekcję dokumentów, jeśli już istnieje. Jednak przykładowej aplikacji ignoruje wszystkie dane zwrócone przez `CreateDocumentCollectionIfNotExistsAsync` metody.

> [!NOTE]
> `CreateDocumentCollectionIfNotExistsAsync` Metoda zwraca `Task<ResourceResponse<DocumentCollection>>` obiekt i kod stanu odpowiedzi można sprawdzić czy utworzono kolekcję dokumentów, czy został zwrócony istniejącą kolekcję dokumentów.

Opcjonalnie `CreateDocumentCollectionIfNotExistsAsync` metody można również określić `RequestOptions` obiektu, który hermetyzuje opcje, które można określić dla żądań wystawiony dla konto bazy danych rozwiązania Cosmos. `RequestOptions.OfferThroughput` Właściwość jest używana do definiowania poziomu wydajności kolekcji dokumentów, a w przykładowej aplikacji, jest równa 400 jednostek żądań na sekundę. Ta wartość powinna zwiększenia lub zmniejszenia w zależności od tego, czy kolekcja często lub rzadko uzyskuje się.

> [!IMPORTANT]
> Należy pamiętać, że `CreateDocumentCollectionIfNotExistsAsync` metody utworzy nową kolekcję z zarezerwowaną przepływnością, co ma implikacje cenowe.

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>Pobieranie dokumentu kolekcji dokumentów

Zawartość kolekcji dokumentów mogą zostać pobrane przez tworzenie i wykonywanie zapytań dokumentu. Dokument zapytania jest tworzony z `DocumentClient.CreateDocumentQuery` metody:

```csharp
public async Task<List<TodoItem>> GetTodoItemsAsync()
{
  ...
  var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
            .AsDocumentQuery();
  while (query.HasMoreResults)
  {
    Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
  }
  ...
}
```

To zapytanie asynchronicznie pobiera wszystkie dokumenty z określonej kolekcji, a następnie umieszcza dokumentów w `List<TodoItem>` kolekcji do wyświetlenia.

`CreateDocumentQuery<T>` Określa metodę `Uri` argument, który reprezentuje kolekcję, do której należy wysłać zapytanie dla dokumentów. W tym przykładzie `collectionLink` zmienna jest polem poziomie klasy, które określa `Uri` reprezentujący kolekcji dokumentów do pobierania dokumentów z:

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

`CreateDocumentQuery<T>` Metoda tworzy kwerendę, która jest wykonywana synchronicznie, a zwraca `IQueryable<T>` obiektu. Jednak `AsDocumentQuery` metoda konwertuje `IQueryable<T>` do obiektu `IDocumentQuery<T>` obiektów, które mogą być wykonywane asynchronicznie. Asynchroniczne zapytanie jest wykonywane z `IDocumentQuery<T>.ExecuteNextAsync` metodę, która pobiera następnej strony wyników z bazy danych dokumentów z `IDocumentQuery<T>.HasMoreResults` Właściwość wskazująca, czy istnieją dodatkowe wyniki mają być zwracane przez zapytanie.

Dokumenty mogą być filtrowane po stronie serwera, umieszczając w niej `Where` klauzuli w zapytaniu, którego dotyczy predykat filtrowania kwerenda dotycząca kolekcji dokumentów:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

To zapytanie pobiera wszystkie dokumenty z kolekcji, których `Done` właściwości jest równa `false`.

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>Wstawianie dokumentu do kolekcji dokumentów

Dokumenty są zdefiniowane przez użytkownika zawartość JSON i można wstawiać do kolekcji dokumentów o `DocumentClient.CreateDocumentAsync` metody:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

`CreateDocumentAsync` Określa metodę `Uri` argumentu, który reprezentuje kolekcję dokumentu powinny zostać wstawione do, i `object` argument, który reprezentuje dokument do wstawienia.

### <a name="replacing-a-document-in-a-document-collection"></a>Zastępowanie dokumentu w kolekcji dokumentów

Dokumenty mogą być zastępowane w kolekcji dokumentów o `DocumentClient.ReplaceDocumentAsync` metody:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

`ReplaceDocumentAsync` Określa metodę `Uri` argumentu, który reprezentuje dokument w kolekcji, która powinna zostać zastąpiona, i `object` argument, który reprezentuje dane zaktualizowanego dokumentu.

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>Usunięcie dokumentu z kolekcji dokumentów

Dokument może być usunięty z kolekcji dokumentów o `DocumentClient.DeleteDocumentAsync` metody:

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

`DeleteDocumentAsync` Określa metodę `Uri` argumentu, który reprezentuje dokument w kolekcji, która powinna zostać usunięta.

### <a name="deleting-a-document-collection"></a>Usuwanie kolekcji dokumentów

Kolekcja dokumentów może być usunięty z bazy danych o `DocumentClient.DeleteDocumentCollectionAsync` metody:

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

`DeleteDocumentCollectionAsync` Określa metodę `Uri` argumentu, który reprezentuje kolekcję dokumentów do usunięcia. Należy pamiętać, że wywołanie tej metody spowoduje również usunięcie dokumentów przechowywanych w kolekcji.

### <a name="deleting-a-database"></a>Usuwanie bazy danych

Bazy danych może być usunięty z rozwiązania Cosmos DB bazy danych przy użyciu adresu e-mail `DocumentClient.DeleteDatabaesAsync` metody:

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

`DeleteDatabaseAsync` Określa metodę `Uri` argumentu, który reprezentuje bazy danych do usunięcia. Należy pamiętać, że wywołanie tej metody spowoduje również usunięcie kolekcji dokumentów przechowywanych w bazie danych i dokumenty zapisane w kolekcjach dokumentów.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak używać rozwiązania Cosmos Azure DB .NET Standard biblioteki klienta do integracji z bazą danych dokumentów bazy danych Azure rozwiązania Cosmos w aplikacji platformy Xamarin.Forms. Baza danych dokumentów bazy danych Azure rozwiązania Cosmos jest bazą danych NoSQL, który zapewnia małe opóźnienia dostępu do dokumentów JSON, oferty usługi szybkie, wysokiej dostępności i skalowalności bazy danych dla aplikacji, które wymagają bezproblemowego skalowania i globalnej replikacji.


## <a name="related-links"></a>Linki pokrewne

- [Bazy danych rozwiązania Cosmos Azure ToDo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)
- [Dokumentacja rozwiązania Cosmos Azure DB](/azure/cosmos-db/)
- [Biblioteka klienta usługi Azure rozwiązania Cosmos DB .NET Standard](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Interfejs API Azure Cosmos DB](https://docs.microsoft.com/en-us/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)

