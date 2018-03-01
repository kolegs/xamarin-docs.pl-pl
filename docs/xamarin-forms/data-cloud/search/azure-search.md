---
title: "Wyszukiwanie danych za pomocą usługi Azure Search"
description: "Wyszukiwanie Azure to usługa w chmurze, który zapewnia indeksowanie i badania możliwości przekazywane dane. Spowoduje to usunięcie wymagania dotyczące infrastruktury i złożoności algorytmu wyszukiwania tradycyjnie skojarzone z wdrożeniem funkcji wyszukiwania w aplikacji. W tym artykule przedstawiono sposób korzystania z biblioteki usługi Microsoft Azure wyszukiwania Integrowanie usługi Azure Search w aplikacji platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
ms.openlocfilehash: bf6b9f8aaa07e934a1e707b85ecaa24e4f3d99bf
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="searching-data-with-azure-search"></a>Wyszukiwanie danych za pomocą usługi Azure Search

_Wyszukiwanie Azure to usługa w chmurze, który zapewnia indeksowanie i badania możliwości przekazywane dane. Spowoduje to usunięcie wymagania dotyczące infrastruktury i złożoności algorytmu wyszukiwania tradycyjnie skojarzone z wdrożeniem funkcji wyszukiwania w aplikacji. W tym artykule przedstawiono sposób korzystania z biblioteki usługi Microsoft Azure wyszukiwania Integrowanie usługi Azure Search w aplikacji platformy Xamarin.Forms._

## <a name="overview"></a>Omówienie

Dane są przechowywane w usłudze Azure Search jako indeksów oraz dokumentów. *Indeksu* jest magazynem danych, które mogą być wyszukiwane przez usługę Azure Search i jest podobny do tabeli bazy danych. A *dokumentu* jest pojedynczą jednostkę danych z możliwością wyszukiwania w indeksie i jest podobny do wiersza bazy danych. Podczas przekazywania dokumentów i przesyłania zapytań wyszukiwań do usługi Azure Search, żądania są wysyłane do konkretnego indeksu w usłudze wyszukiwania.

Każde żądanie, wprowadzone do usługi Azure Search musi zawierać nazwę usługi i klucz interfejsu API. Istnieją dwa typy klucz interfejsu API:

- *Klucze administratora* przyznać pełne prawa do wszystkich operacji. W tym zarządzanie usługą, tworzenia i usuwania indeksów i źródeł danych.
- *Klucze zapytań* przyznać dostęp tylko do odczytu do indeksów oraz dokumentów i powinna być używana przez aplikacje, które wysyłają żądania wyszukiwania.

Najczęściej wykonywane żądania do usługi Azure Search jest wykonywanie zapytań. Istnieją dwa typy zapytania, które można przesłać:

- A *wyszukiwania* zapytania wyszukiwania dla jednego lub więcej elementów we wszystkich polach można wyszukiwać w indeksie. Zapytania wyszukiwania są tworzone przy użyciu składni uproszczoną lub składnia zapytań Lucene. Aby uzyskać więcej informacji, zobacz [prosta składnia zapytań w usłudze Azure Search](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/), i [składnia zapytań Lucene w usłudze Azure Search](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/).
- A *filtru* zapytania ocenia wyrażenie logiczne przez wszystkie pola możliwego do indeksu. Filtr zapytania są tworzone przy użyciu podzbiór języka filtru OData. Aby uzyskać więcej informacji, zobacz [OData składnia wyrażeń dla usługi Azure Search](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/).

Zapytania wyszukiwania i filtrów kwerendy można osobno lub razem. Razem zapytanie filtru zostanie zastosowana jako pierwsza do całego indeksu, a następnie zapytania wyszukiwania jest wykonywane na wynikach zapytania filtru.

Usługa Azure Search obsługuje również podczas pobierania sugestii oparte na dane wejściowe wyszukiwania. Aby uzyskać więcej informacji, zobacz [zapytania sugestię](#suggestions).

## <a name="setup"></a>Konfiguracja

Integrowanie usługi Azure Search w aplikacji platformy Xamarin.Forms proces wygląda następująco:

1. Tworzenie usługi Azure Search. Aby uzyskać więcej informacji, zobacz [Tworzenie usługi Azure Search przy użyciu portalu Azure](/azure/search/search-create-service-portal/).
1. Usuń Silverlight jako platforma docelowa z rozwiązania platformy Xamarin.Forms przenośnej biblioteki klasy (PCL). Można to zrobić, zmieniając profilu PCL do dowolnego profilu, która obsługuje wiele platform, ale nie obsługuje programu Silverlight, takich jak profil 151 lub 92.
1. Dodaj [Biblioteka programu Microsoft Azure Search](https://www.nuget.org/packages/Microsoft.Azure.Search) pakiet NuGet do projektu PCL w rozwiązaniu platformy Xamarin.Forms.

Po wykonaniu tych kroków, interfejsu API z biblioteki Microsoft Search umożliwia zarządzanie źródłami danych i indeksów wyszukiwania, przekazywanie i zarządzania dokumentami oraz wykonywanie zapytań.

## <a name="creating-the-azure-search-index"></a>Tworzenie indeksu usługi Azure Search

Schematu indeksu musi być zdefiniowany mapowanego struktury danych, które mają być wyszukiwane. Może to być zakończonej w portalu Azure lub programistycznie przy użyciu `SearchServiceClient` klasy. Ta klasa zarządza połączeniami z usługi Azure Search i może służyć do tworzenia indeksu. Poniższy przykładowy kod przedstawia sposób tworzenia wystąpienia tej klasy:

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

`SearchServiceClient` Przeładowania konstruktora przyjmuje nazwę usługi wyszukiwania i `SearchCredentials` obiekt jako argumenty, z `SearchCredentials` zawijania obiektu *klucz administratora* dla usługi Azure Search. *Klucz administratora* jest wymagany do tworzenia indeksu.

> [!NOTE]
>  Pojedynczy `SearchServiceClient` wystąpienia powinny być używane w aplikacji w celu uniknięcia otwarcia zbyt wielu połączeń do usługi Azure Search.

Indeks jest definiowana za pomocą `Index` obiektów, jak pokazano w poniższym przykładzie:

```csharp
static void CreateSearchIndex()
{
  var index = new Index()
  {
    Name = Constants.Index,
    Fields = new[]
    {
      new Field("id", DataType.String) { IsKey = true, IsRetrievable = true },
      new Field("name", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("location", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("details", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSearchable = true },
      new Field("imageUrl", DataType.String) { IsRetrievable = true }
    },
    Suggesters = new[]
    {
      new Suggester("nameSuggester", SuggesterSearchMode.AnalyzingInfixMatching, new[] { "name" })
    }
  };

  searchClient.Indexes.Create(index);
}
```

`Index.Name` Właściwość powinna być ustawiona na nazwę indeksu i `Index.Fields` właściwość powinna być ustawiona na tablicę `Field` obiektów. Każdy `Field` wystąpienia Określa nazwę, typ i żadnych właściwości, które określają sposób używania pola. Te właściwości obejmują:

- `IsKey` — Wskazuje, czy pole jest klucz indeksu. Tylko jedno pole w indeksie, typu `DataType.String`, muszą być wyznaczone jako pole klucza.
- `IsFacetable` — Wskazuje, czy jest możliwe przeprowadzenie nawigacji aspektowej dla tego pola. Wartość domyślna to `false`.
- `IsFilterable` — Wskazuje, czy pole może być używane w zapytaniach filtrów. Wartość domyślna to `false`.
- `IsRetrievable` — Wskazuje, czy pole może pobierać w wynikach wyszukiwania. Wartość domyślna to `true`.
- `IsSearchable` — Wskazuje, czy pole jest uwzględniane w wynikach wyszukiwania pełnotekstowego. Wartość domyślna to `false`.
- `IsSortable` — Wskazuje, czy pole może być używana w `OrderBy` wyrażenia. Wartość domyślna to `false`.

> [!NOTE]
> Wprowadzanie zmian do indeksu po jego wdrożeniu obejmuje ponowne utworzenie i ponownie załadowanie danych.

`Index` Obiektu można opcjonalnie określić `Suggesters` właściwość, która definiuje pola w indeksie, który ma być używany do obsługi automatycznego zakończenia lub kwerendy sugestii wyszukiwania. `Suggesters` Właściwość powinna być ustawiona na tablicę `Suggester` obiektów, które definiują pola, które są używane do tworzenia wyniki sugestii dotyczących wyszukiwania.

Po utworzeniu `Index` obiektu o utworzeniu indeksu przez wywołanie metody `Indexes.Create` na `SearchServiceClient` wystąpienia.

> [!NOTE]
> Podczas tworzenia indeksu z aplikacji, które muszą być przechowywane w reakcji, użyj `Indexes.CreateAsync` metody.

Aby uzyskać więcej informacji, zobacz [utworzyć indeks usługi Azure Search przy użyciu zestawu .NET SDK](/azure/search/search-create-index-dotnet/).

## <a name="deleting-the-azure-search-index"></a>Usuwanie indeksu usługi Azure Search

Indeks może zostać usunięta przez wywołanie `Indexes.Delete` na `SearchServiceClient` wystąpienie:

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Przekazywanie danych do indeksu usługi Azure Search

Po zdefiniowaniu indeksu, dane mogą być przekazywane do niego przy użyciu jednej z dwóch modeli:

- **Model ściągania** — danych jest okresowo pozyskanych z usługi Azure DocumentDB, bazy danych SQL Azure, magazyn obiektów Blob Azure lub programu SQL Server obsługiwanych w maszynie wirtualnej platformy Azure.
- **Wypychanie modelu** — programowo wysłaniu danych do indeksu. Jest to model przyjęte w tym artykule.

A `SearchIndexClient` wystąpienia należy utworzyć w celu importowania danych do indeksu. Można to zrobić przez wywołanie metody `SearchServiceClient.Indexes.GetClient` metody, jak pokazano w poniższym przykładzie:

```csharp
static void UploadDataToSearchIndex()
{
  var indexClient = searchClient.Indexes.GetClient(Constants.Index);

  var monkeyList = MonkeyData.Monkeys.Select(m => new
  {
    id = Guid.NewGuid().ToString(),
    name = m.Name,
    location = m.Location,
    details = m.Details,
    imageUrl = m.ImageUrl
  });

  var batch = IndexBatch.New(monkeyList.Select(IndexAction.Upload));
  try
  {
    indexClient.Documents.Index(batch);
  }
  catch (IndexBatchException ex)
  {
    // Sometimes when the Search service is under load, indexing will fail for some
    // documents in the batch. Compensating actions like delaying and retrying should be taken.
    // Here, the failed document keys are logged.
    Console.WriteLine("Failed to index some documents: {0}",
      string.Join(", ", ex.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
  }
}
```

Dane do zaimportowania do indeksu jest dostarczana jako `IndexBatch` obiektu, który hermetyzuje kolekcję `IndexAction` obiektów. Każdy `IndexAction` wystąpienia zawiera dokument i właściwości, które informują usługę Azure Search akcję do wykonania w dokumencie. W powyższym przykładzie kodu `IndexAction.Upload` akcji jest określony, której wynikiem jest dokumentu wstawiane do indeksu, jeśli jest nowy, lub zastąpiony, jeśli już istnieje. `IndexBatch` Obiektu zostanie przesłany do indeksu przez wywołanie metody `Documents.Index` metoda `SearchIndexClient` obiektu. Informacje o innych akcji indeksowania, zobacz [Wybieranie akcji indeksowania do użycia](/azure/search/search-import-data-dotnet#ii-decide-which-indexing-action-to-use).

> [!NOTE]
> Pojedyncze żądanie indeksowania może obejmować tylko 1000 dokumentów.

Należy pamiętać, że w powyższym przykładzie kodu `monkeyList` Kolekcja zostanie utworzona jako anonimowy obiekt z kolekcji `Monkey` obiektów. Spowoduje to utworzenie danych `id` pól i rozpoznawany jako mapowanie przypadków Pascal `Monkey` nazw właściwości do camelcase wyszukiwania nazw pól w indeksie. Alternatywnie to mapowanie może być również wykonywane przez dodanie `[SerializePropertyNamesAsCamelCase]` atrybutu `Monkey` klasy.

Aby uzyskać więcej informacji, zobacz [przekazywanie danych do usługi Azure Search przy użyciu zestawu .NET SDK](/azure/search/search-import-data-dotnet/).

## <a name="querying-the-azure-search-index"></a>Wykonywanie zapytania dotyczącego indeksu usługi Azure Search

A `SearchIndexClient` można utworzyć wystąpienia, aby tworzenie zapytań względem indeksu. Gdy aplikacja wykonuje zapytania, zaleca się postępuj zgodnie z zasadą najniższych uprawnień i Utwórz `SearchIndexClient` bezpośrednio, przekazywanie *klucz zapytania* jako argument. Dzięki temu, że użytkownicy mają dostęp tylko do odczytu do indeksów oraz dokumentów. Takie podejście jest przedstawiona w poniższym przykładzie kodu:

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

`SearchIndexClient` Przeładowania konstruktora przyjmuje nazwę usługi wyszukiwania, nazwę indeksu i `SearchCredentials` obiekt jako argumenty, z `SearchCredentials` zawijania obiektu *klucz zapytania* dla usługi Azure Search.

### <a name="search-queries"></a>Zapytania wyszukiwania

Indeks może być badana przez wywołanie metody `Documents.SearchAsync` metoda `SearchIndexClient` wystąpienie, jak pokazano w poniższym przykładzie:

```csharp
async Task AzureSearch(string text)
{
  Monkeys.Clear();

  var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text);
  foreach (SearchResult<Monkey> result in searchResults.Results)
  {
    Monkeys.Add(new Monkey
    {
      Name = result.Document.Name,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SearchAsync` Metoda przyjmuje argument tekstowy wyszukiwania i opcjonalne `SearchParameters` obiekt, który może służyć do uściślenia zapytania. Zapytania wyszukiwania jest określony jako argument tekstowy wyszukiwania, gdy zapytanie filtru można określić przez ustawienie `Filter` właściwość `SearchParameters` argumentu. Poniższy przykład kodu pokazuje, że oba typy zapytań:

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

To zapytanie filtru jest stosowana do całego indeksu i usuwa dokumenty z wyników gdzie `location` pole nie jest równa Chin i nie jest równa Wietnam. Po filtrowaniu zapytania wyszukiwania odbywa się w wynikach zapytania filtru.

> [!NOTE]
> Aby filtrować bez wyszukiwania, należy przekazać `*` jako argument tekst wyszukiwania.

`SearchAsync` Metoda zwraca `DocumentSearchResult` obiekt zawierający wyniki zapytania. Ten obiekt jest wyliczyć z każdym `Document` obiekt tworzony jako `Monkey` obiektu i dodane do `Monkeys` `ObservableCollection` do wyświetlenia. Poniższe zrzuty ekranu Pokaż wyszukiwania zapytania wyniki zwrócone z usługi Azure Search:

![](azure-search-images/search.png "Wyniki wyszukiwania")

Aby uzyskać więcej informacji na temat wyszukiwania i filtrowania, zobacz [tworzenie zapytań względem indeksu usługi Azure Search przy użyciu zestawu .NET SDK](/azure/search/search-query-dotnet/).

<a name="suggestions" />

### <a name="suggestion-queries"></a>Sugestia zapytań

Usługa wyszukiwanie Azure umożliwia sugestie wymagane na podstawie wyszukiwania zapytania, wywołując `Documents.SuggestAsync` metoda `SearchIndexClient` wystąpienia. Zostało to przedstawione w poniższym przykładzie kodu:

```csharp
async Task AzureSuggestions(string text)
{
  Suggestions.Clear();

  var parameters = new SuggestParameters()
  {
    UseFuzzyMatching = true,
    HighlightPreTag = "[",
    HighlightPostTag = "]",
    MinimumCoverage = 100,
    Top = 10
  };

  var suggestionResults =
    await indexClient.Documents.SuggestAsync<Monkey>(text, "nameSuggester", parameters);

  foreach (var result in suggestionResults.Results)
  {
    Suggestions.Add(new Monkey
    {
      Name = result.Text,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SuggestAsync` Metoda przyjmuje argument tekstowy wyszukiwania, nazwę sugestora do użycia (która jest zdefiniowana w indeksie), i opcjonalny `SuggestParameters` obiekt, który może służyć do uściślenia zapytania. `SuggestParameters` Wystąpienia ustawia następujące właściwości:

- `UseFuzzyMatching` — wartość `true`, usługi Azure Search znajdzie sugestie, nawet jeśli występuje znak podstawione lub brak wyszukiwanie w tekście.
- `HighlightPreTag` — tag, który jest dołączany na początku na trafień uwag.
- `HighlightPostTag` — tag, który jest dołączany do trafień uwag.
- `MinimumCoverage` — reprezentuje procent indeksu, który musi być objęta sugestię zapytania dla zapytania jako informację o powodzeniu. Wartość domyślna to 80.
- `Top` — Liczba sugestii do pobrania. Go musi być liczbą całkowitą z zakresu od 1 do 100 z wartości domyślnej równej 5.

Ogólny efekt jest, że top 10 wyników z indeksu zostaną zwrócone z wyróżnianie trafień, a wyniki obejmują dokumentów, które obejmują podobnie wpisana terminy wyszukiwania.

`SuggestAsync` Metoda zwraca `DocumentSuggestResult` obiekt zawierający wyniki zapytania. Ten obiekt jest wyliczyć z każdym `Document` obiekt tworzony jako `Monkey` obiektu i dodane do `Monkeys` `ObservableCollection` do wyświetlenia. Poniższe zrzuty ekranu pokazują sugestię wyniki zwrócone z usługi Azure Search:

![](azure-search-images/suggest.png "Wyniki sugestii")

Należy pamiętać, że w przykładowej aplikacji `SuggestAsync` metoda jest wywoływana tylko, gdy użytkownik zakończy wprowadzanie wyszukiwanego terminu. Jednak go mogą służyć do obsługi zapytań wyszukiwania funkcja automatycznego uzupełniania, wykonując na poszczególnych przycisków.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób korzystania z biblioteki usługi Microsoft Azure wyszukiwania Integrowanie usługi Azure Search w aplikacji platformy Xamarin.Forms. Wyszukiwanie Azure to usługa w chmurze, który zapewnia indeksowanie i badania możliwości przekazywane dane. Spowoduje to usunięcie wymagania dotyczące infrastruktury i złożoności algorytmu wyszukiwania tradycyjnie skojarzone z wdrożeniem funkcji wyszukiwania w aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [Usługa Azure Search (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureSearch/)
- [Dokumentacja usługi Azure Search](/azure/search/)
- [Biblioteka wyszukiwania Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.Search/)
