---
title: Uzyskiwanie dostępu do danych zdalnych
description: W tym rozdziale wyjaśniono, jak aplikacja mobilna w ramach aplikacji eShopOnContainers uzyskuje dostęp do danych z konteneryzowanych mikrousług.
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 009a4025bc9df6f657964b7e97e559635ef0a929
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996168"
---
# <a name="accessing-remote-data"></a>Uzyskiwanie dostępu do danych zdalnych

Wiele nowoczesnych rozwiązań opartych na sieci web wprowadzić korzystanie z usług sieci web hostowanych na serwerach sieci web w celu zapewnienia funkcji zdalnego klienta aplikacji. Operacje, które uwidacznia Usługa sieci web stanowi interfejs API sieci web.

Aplikacje klienckie powinno być możliwe korzystanie z interfejsu API sieci web nie wiedząc o tym, jak zaimplementować danych lub operacji, które uwidacznia interfejs API. Wymaga to, że interfejs API przestrzega wspólnych standardów, które umożliwiają Usługa klienta sieci web i aplikacji uzgodnić formatów danych użycia i struktury danych, które są wymieniane między aplikacjami klienta i usługę sieci web.

## <a name="introduction-to-representational-state-transfer"></a>Wprowadzenie do technologii Representational State Transfer

Representational State Transfer (REST) jest to styl architektoniczny dotyczący tworzenia systemów rozproszonych opartych na hipermediach. Główną zaletą modelu REST jest ma opartą na otwartych standardach i nie jest powiązana implementacji modelu lub aplikacje klienckie, mających do niej dostęp do wszelkich konkretnej implementacji. W związku z tym usługa internetowa REST może być wdrażany za pomocą programu Microsoft ASP.NET Core MVC, a aplikacje klienckie mogą być opracowywać zawartość za pomocą dowolnego języka i zestawu narzędzi, który można wygenerować żądania HTTP i analizowanie odpowiedzi HTTP.

W modelu REST wykorzystuje schemat nawigacji do reprezentowania obiektów i usług za pośrednictwem sieci, określane jako zasoby. Systemy, które zazwyczaj implementują REST używają protokołu HTTP do przesyłania żądań dostępu do tych zasobów. W tych systemach aplikacja kliencka przesyła żądanie w formie identyfikatora URI, który identyfikuje zasób i metodę HTTP (np. GET, POST, PUT i DELETE), która wskazuje operacji do wykonania na taki zasób. Treść żądania HTTP zawiera wszystkie dane wymagane do wykonania tej operacji.

> [!NOTE]
> REST definiuje z bezstanowego modelu żądań. Dlatego żądania HTTP muszą być niezależne i mogą występować w dowolnej kolejności.

Odpowiedź REST żądania sprawia, że użycie standardowych kodów stanu HTTP. Na przykład żądania, która zwraca prawidłowe dane powinna zawierać kod odpowiedzi HTTP 200 (OK), gdy żądanie, w której nie można odnaleźć lub usunąć określony zasób powinien zwrócić odpowiedź, która zawiera kod stanu HTTP 404 (nie znaleziono).

RESTful internetowy interfejs API udostępnia zestaw zasobów, połączonych i zapewnia podstawowe operacje, które umożliwiają aplikacji do manipulowania tymi zasobami i łatwe przechodzenie między nimi. Z tego powodu identyfikatory URI, które stanowią typowe internetowego interfejsu API RESTful są ukierunkowane na dane, które udostępnia ona i użyj urządzenia dostarczane przy użyciu protokołu HTTP do działania na tych danych.

Dane zawarte przez aplikację klienta w żądaniu HTTP i odpowiednich komunikatów odpowiedzi z serwera sieci web może być przedstawiony w różnych formatach, znane jako typy nośnika. Gdy aplikacja kliencka wysyła żądanie, która zwraca dane w treści wiadomości, można określić typy nośników, może obsługiwać w `Accept` nagłówku żądania. Jeśli serwer sieci web obsługuje ten typ nośnika, mogą odpowiedzieć odpowiedź, która zawiera `Content-Type` nagłówek, który określa format danych w treści wiadomości. Jest ona odpowiedzialność aplikacji klienckiej, można przeanalizować komunikat odpowiedzi i odpowiednio interpretacji wyników w treści komunikatu.

Aby uzyskać więcej informacji na temat REST, zobacz [projekt interfejsu API](/azure/architecture/best-practices/api-design/) i [implementacji interfejsu API](/azure/architecture/best-practices/api-implementation/).

## <a name="consuming-restful-apis"></a>Korzystanie z interfejsów API RESTful

Aplikacja mobilna w ramach aplikacji eShopOnContainers używa wzorca Model-View-ViewModel (MVVM) i elementy modelu stanowi wzorca jednostki domeny używanych w aplikacji. Klasy kontrolera i repozytorium, w ramach aplikacji eShopOnContainers aplikacji odwołanie akceptują i zwracają, wiele z tych obiektów modelu. W związku z tym są one używane jako obiektów transferu danych (dto), których przechowywane są wszystkie dane, które są przekazywane między konteneryzowane mikrousługi i aplikacji mobilnych. Główną zaletą przy użyciu dto do przekazywania danych do i odbierania danych z usługi sieci web to poprzez przekazanie większej ilości danych w pojedynczym wywołaniu zdalny, aplikacja może zmniejszyć liczbę wywołań zdalnych, które należy podjąć.

### <a name="making-web-requests"></a>Tworzenie żądania sieci Web

Zastosowań aplikacji mobilnej w ramach aplikacji eShopOnContainers `HttpClient` klasy do wysyłania żądań za pośrednictwem protokołu HTTP przy użyciu formatu JSON jako typ nośnika. Ta klasa udostępnia funkcje asynchronicznego wysyłania żądań HTTP i odbierania odpowiedzi HTTP z identyfikatora URI identyfikowane zasobów. `HttpResponseMessage` Klasa reprezentuje komunikat odpowiedzi HTTP otrzymany z interfejsu API REST, po dokonaniu żądania HTTP. Zawiera informacje o odpowiedzi, łącznie z kodem stanu, nagłówki i wszelkie treści. `HttpContent` Klasa reprezentuje ciała dokumentu HTTP i nagłówków zawartości, takich jak `Content-Type` i `Content-Encoding`. Zawartość może zostać odczytany przy użyciu dowolnej z `ReadAs` metod, takich jak `ReadAsStringAsync` i `ReadAsByteArrayAsync`, w zależności od formatu danych.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>Żądania GET

`CatalogService` Klasa jest używana do zarządzania proces pobierania danych z mikrousług katalogu. W `RegisterDependencies` method in Class metoda `ViewModelLocator` klasy `CatalogService` klasa jest rejestrowana jako mapowanie typu względem `ICatalogService` typu za pomocą kontenera iniekcji zależności Autofac. Następnie, gdy wystąpienie klasy `CatalogViewModel` klasa jest tworzona, jego Konstruktor akceptuje `ICatalogService` typ, który jest rozpoznawany jako Autofac, zwrócenia wystąpienia `CatalogService` klasy. Aby uzyskać więcej informacji na temat wstrzykiwanie zależności zobacz [wprowadzenie do wstrzykiwania zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Rysunek 10-1 przedstawia interakcję klas, które odczytują dane wykazu z mikrousług katalogu do wyświetlania przez `CatalogView`.

[![](accessing-remote-data-images/catalogdata.png "Pobieranie danych z mikrousług katalogu")](accessing-remote-data-images/catalogdata-large.png#lightbox "pobieranie danych z mikrousług katalogu")

**Rysunek 10-1**: pobieranie danych z mikrousług katalogu

Gdy `CatalogView` przejście, `OnInitialize` method in Class metoda `CatalogViewModel` nosi nazwę klasy. Ta metoda pobiera dane wykazu z mikrousług katalogu, jak pokazano w poniższym przykładzie kodu:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

Ta metoda wywołuje `GetCatalogAsync` metody `CatalogService` wystąpienia, który został wprowadzony w `CatalogViewModel` przez Autofac. Poniższy kod przedstawia przykład `GetCatalogAsync` metody:

```csharp
public async Task<ObservableCollection<CatalogItem>> GetCatalogAsync()  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.CatalogEndpoint);  
    builder.Path = "api/v1/catalog/items";  
    string uri = builder.ToString();  

    CatalogRoot catalog = await _requestProvider.GetAsync<CatalogRoot>(uri);  
    ...  
    return catalog?.Data.ToObservableCollection();            
}
```

Ta metoda tworzy identyfikator URI, który identyfikuje zasób żądania, które będą wysyłane do i używa `RequestProvider` klasy, aby wywołać metodę GET HTTP do zasobu przed zwróceniem wyników do `CatalogViewModel`. `RequestProvider` Klasy zawiera funkcję, która przesyła żądanie w formie identyfikatora URI, który identyfikuje zasób, metodę HTTP, która wskazuje operacji do wykonania na taki zasób i treści, która zawiera wszystkie dane wymagane do wykonania tej operacji. Aby uzyskać informacje o tym, jak `RequestProvider` klasy są wstrzykiwane do `CatalogService class`, zobacz [wprowadzenie do wstrzykiwania zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Poniższy kod przedstawia przykład `GetAsync` method in Class metoda `RequestProvider` klasy:

```csharp
public async Task<TResult> GetAsync<TResult>(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    HttpResponseMessage response = await httpClient.GetAsync(uri);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>   
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Ta metoda wywołuje `CreateHttpClient` metody, która zwraca wystąpienie `HttpClient` klasy przy użyciu zestawu odpowiednie nagłówki. Następnie przesyła żądanie GET asynchroniczne do zasobu wskazywanego przez identyfikator URI odpowiedzi są przechowywane w `HttpResponseMessage` wystąpienia. `HandleResponse` Następnie wywoływana jest metoda, która zgłasza wyjątek, jeśli odpowiedź nie zawiera Powodzenie kod stanu HTTP. Następnie jest do odczytu odpowiedzi jako ciąg znaków, konwersji z formatu JSON do `CatalogRoot` obiektu, a następnie wróciło, aby `CatalogService`.

`CreateHttpClient` Pokazano w poniższym przykładzie kodu:

```csharp
private HttpClient CreateHttpClient(string token = "")  
{  
    var httpClient = new HttpClient();  
    httpClient.DefaultRequestHeaders.Accept.Add(  
        new MediaTypeWithQualityHeaderValue("application/json"));  

    if (!string.IsNullOrEmpty(token))  
    {  
        httpClient.DefaultRequestHeaders.Authorization =   
            new AuthenticationHeaderValue("Bearer", token);  
    }  
    return httpClient;  
}
```

Ta metoda tworzy nowe wystąpienie klasy `HttpClient` klasy i zestawy `Accept` nagłówka żadnych żądań wysyłanych przez `HttpClient` wystąpienia do `application/json`, co oznacza, że oczekuje, że zawartość odpowiedzi formatowana przy użyciu formatu JSON. Następnie, jeśli token dostępu został przekazany jako argument do `CreateHttpClient` metody, jest ona dodawana do `Authorization` nagłówka żadnych żądań wysyłanych przez `HttpClient` wypadku prefiksem ciągu `Bearer`. Aby uzyskać więcej informacji o autoryzacji, zobacz [autoryzacji](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Gdy `GetAsync` method in Class metoda `RequestProvider` klasy wywołania `HttpClient.GetAsync`, `Items` method in Class metoda `CatalogController` klasy w projekcie Catalog.API zostanie wywołana, która została przedstawiona w poniższym przykładzie kodu:

```csharp
[HttpGet]  
[Route("[action]")]  
public async Task<IActionResult> Items(  
    [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)  
{  
    var totalItems = await _catalogContext.CatalogItems  
        .LongCountAsync();  

    var itemsOnPage = await _catalogContext.CatalogItems  
        .OrderBy(c=>c.Name)  
        .Skip(pageSize * pageIndex)  
        .Take(pageSize)  
        .ToListAsync();  

    itemsOnPage = ComposePicUri(itemsOnPage);  
    var model = new PaginatedItemsViewModel<CatalogItem>(  
        pageIndex, pageSize, totalItems, itemsOnPage);             

    return Ok(model);  
}
```

Ta metoda pobiera dane wykazu z bazy danych SQL za pomocą platformy EntityFramework i zwraca je jako wiadomości odpowiedzi, która zawiera kod stanu HTTP sukcesu i zbiór JSON w formacie `CatalogItem` wystąpień.

#### <a name="making-a-post-request"></a>Żądania POST

`BasketService` Klasa jest używana do zarządzania pobierania danych i aktualizowania procesu przy użyciu mikrousług koszyka. W `RegisterDependencies` method in Class metoda `ViewModelLocator` klasy `BasketService` klasa jest rejestrowana jako mapowanie typu względem `IBasketService` typu za pomocą kontenera iniekcji zależności Autofac. Następnie, gdy wystąpienie klasy `BasketViewModel` klasa jest tworzona, jego Konstruktor akceptuje `IBasketService` typ, który jest rozpoznawany jako Autofac, zwrócenia wystąpienia `BasketService `klasy. Aby uzyskać więcej informacji na temat wstrzykiwanie zależności zobacz [wprowadzenie do wstrzykiwania zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Rysunek 10-2 przedstawia interakcję klas, które wysyłają koszyka wyświetlanych danych przez `BasketView`, aby mikrousług koszyka.

[![](accessing-remote-data-images/basketdata.png "Wysyłanie danych do mikrousług koszyka")](accessing-remote-data-images/basketdata-large.png#lightbox "wysyłania danych do mikrousług koszyka")

**Rysunek 10-2**: wysyłanie danych do mikrousług koszyka

Po dodaniu elementu do koszyka `ReCalculateTotalAsync` method in Class metoda `BasketViewModel` nosi nazwę klasy. Ta metoda aktualizuje wartość łączna liczba elementów w koszyku i wysyła dane koszyka do mikrousług koszyka, jak pokazano w poniższym przykładzie kodu:

```csharp
private async Task ReCalculateTotalAsync()  
{  
    ...  
    await _basketService.UpdateBasketAsync(new CustomerBasket  
    {  
        BuyerId = userInfo.UserId,   
        Items = BasketItems.ToList()  
    }, authToken);  
}
```

Ta metoda wywołuje `UpdateBasketAsync` metody `BasketService` wystąpienia, który został wprowadzony w `BasketViewModel` przez Autofac. Metoda pokazano `UpdateBasketAsync` metody:

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

Ta metoda tworzy identyfikator URI, który identyfikuje zasób żądania, które będą wysyłane do i używa `RequestProvider` klasy, aby wywołać metodę POST HTTP do zasobu przed zwróceniem wyników do `BasketViewModel`. Należy zauważyć, że token dostępu uzyskany z IdentityServer podczas procesu uwierzytelniania jest wymagane do autoryzowania żądania do mikrousług koszyka. Aby uzyskać więcej informacji o autoryzacji, zobacz [autoryzacji](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Poniższy przykład kodu przedstawia jedną z `PostAsync` metody `RequestProvider` klasy:

```csharp
public async Task<TResult> PostAsync<TResult>(  
    string uri, TResult data, string token = "", string header = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    ...  
    var content = new StringContent(JsonConvert.SerializeObject(data));  
    content.Headers.ContentType = new MediaTypeHeaderValue("application/json");  
    HttpResponseMessage response = await httpClient.PostAsync(uri, content);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>  
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Ta metoda wywołuje `CreateHttpClient` metody, która zwraca wystąpienie `HttpClient` klasy przy użyciu zestawu odpowiednie nagłówki. Następnie przesyła żądanie POST asynchronicznych do zasobu wskazywanego przez identyfikator URI przy użyciu danych serializacji koszyka, są wysyłane w formacie JSON i odpowiedzi są przechowywane w `HttpResponseMessage` wystąpienia. `HandleResponse` Następnie wywoływana jest metoda, która zgłasza wyjątek, jeśli odpowiedź nie zawiera Powodzenie kod stanu HTTP. Następnie jest do odczytu odpowiedzi jako ciąg znaków, konwersji z formatu JSON do `CustomerBasket` obiektu, a następnie wróciło, aby `BasketService`. Aby uzyskać więcej informacji na temat `CreateHttpClient` metody, zobacz [wprowadzania żądań UZYSKAĆ](#making_a_get_request).

Gdy `PostAsync` method in Class metoda `RequestProvider` klasy wywołania `HttpClient.PostAsync`, `Post` method in Class metoda `BasketController` klasy w projekcie Basket.API zostanie wywołana, która została przedstawiona w poniższym przykładzie kodu:

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

Ta metoda używa wystąpienia `RedisBasketRepository` klasy do utrwalenia danych koszyka do pamięci podręcznej Redis i zwraca go jak sformatowane komunikat odpowiedzi, Powodzenie kod stanu HTTP i JSON `CustomerBasket` wystąpienia.

#### <a name="making-a-delete-request"></a>Żądania DELETE

Rysunek 10-3. pokazuje interakcje występujące klas, które usuwania danych koszyk z mikrousług koszyka, `CheckoutView`.

![](accessing-remote-data-images/checkoutdata.png "Usuwania danych z mikrousług koszyka")

**Rysunek 10-3**: usunięcie danych z mikrousług koszyka

Po wywołaniu rozpoczęcie procesu realizowania zamówienia `CheckoutAsync` method in Class metoda `CheckoutViewModel` nosi nazwę klasy. Ta metoda tworzy nowe zamówienie przed wyczyszczeniem koszyka, jak pokazano w poniższym przykładzie kodu:

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

Ta metoda wywołuje `ClearBasketAsync` metody `BasketService` wystąpienia, który został wprowadzony w `CheckoutViewModel` przez Autofac. Metoda pokazano `ClearBasketAsync` metody:

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

Ta metoda tworzy identyfikator URI, który identyfikuje zasób, który będą wysyłane żądania do i używa `RequestProvider` klasy, aby wywołać metodę DELETE HTTP dla zasobu. Należy zauważyć, że token dostępu uzyskany z IdentityServer podczas procesu uwierzytelniania jest wymagane do autoryzowania żądania do mikrousług koszyka. Aby uzyskać więcej informacji o autoryzacji, zobacz [autoryzacji](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Poniższy kod przedstawia przykład `DeleteAsync` method in Class metoda `RequestProvider` klasy:

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

Ta metoda wywołuje `CreateHttpClient` metody, która zwraca wystąpienie `HttpClient` klasy przy użyciu zestawu odpowiednie nagłówki. Następnie przesyła je do zasobu wskazywanego przez identyfikator URI żądania asynchronicznego DELETE. Aby uzyskać więcej informacji na temat `CreateHttpClient` metody, zobacz [wprowadzania żądań UZYSKAĆ](#making_a_get_request).

Gdy `DeleteAsync` method in Class metoda `RequestProvider` klasy wywołania `HttpClient.DeleteAsync`, `Delete` method in Class metoda `BasketController` klasy w projekcie Basket.API zostanie wywołana, która została przedstawiona w poniższym przykładzie kodu:

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

Ta metoda używa wystąpienia `RedisBasketRepository` klasy do usuwania z pamięci podręcznej redis Cache danych koszyka.

## <a name="caching-data"></a>Buforowanie danych

Można zwiększyć wydajność aplikacji, buforując rzadziej używanych danych do szybkiego magazynu znajdującego się blisko aplikacji. Jeśli szybkiego magazynu znajduje się bliżej aplikacji niż oryginalne źródło, a następnie buforowanie może znacznie poprawić odpowiedzi czasu podczas pobierania danych.

Najbardziej typowe rodzaj buforowania jest buforowania read-through, gdzie aplikacja pobiera dane poprzez odwołanie do pamięci podręcznej. Jeśli dane nie znajduje się w pamięci podręcznej, ma pobierane z magazynu danych i dodane do pamięci podręcznej. Aplikacje można zaimplementować buforowania read-through ze wzorcem odkładania do pamięci podręcznej. Ten wzorzec Określa, czy element jest aktualnie w pamięci podręcznej. Jeśli element nie znajduje się w pamięci podręcznej, ma odczytu z magazynu danych i dodane do pamięci podręcznej. Aby uzyskać więcej informacji, zobacz [odkładania do pamięci podręcznej](/azure/architecture/patterns/cache-aside/) wzorca.

> [!TIP]
> Buforowanie danych, które są często odczytywane i które zmieniają się rzadko. Te dane można dodać do pamięci podręcznej na żądanie po raz pierwszy, że jest ono pobierane przez aplikację. Oznacza to, że aplikacja musi pobrać dane tylko jeden raz z magazynu danych, a kolejny dostęp może być zrealizowany przy użyciu pamięci podręcznej.

Aplikacje rozproszone, takie jak w ramach aplikacji eShopOnContainers odwoływać się do aplikacji, powinno zapewniać jednego lub obu następujących pamięci podręczne:

-   Udostępnionej pamięci podręcznej, który może zostać oceniony przez wiele procesów lub maszyn.
-   Prywatnej pamięci podręcznej przechowującej dane lokalnie na urządzeniu, uruchamiając aplikację.

Aplikacja mobilna w ramach aplikacji eShopOnContainers używa prywatnej pamięci podręcznej przechowującej dane lokalnie na urządzeniu, na którym jest uruchomione wystąpienie aplikacji. Aby uzyskać informacji o pamięci podręcznej używane przez aplikację odwołania w ramach aplikacji eShopOnContainers, zobacz [Mikrousługi .NET: Architektura aplikacji kontenerowych nimi .NET](https://aka.ms/microservicesebook).

> [!TIP]
> Pomyśl o pamięci podręcznej jako o przejściowym magazynie danych, który może zniknąć w dowolnym momencie. Upewnij się, że dane są przechowywane w oryginalnym magazynie danych, a także pamięci podręcznej. Następnie są zminimalizować prawdopodobieństwo utraty danych, jeśli pamięć podręczna jest niedostępna.

### <a name="managing-data-expiration"></a>Zarządzanie wygasaniem danych

Jest to niepraktyczne, można oczekiwać, że dane w pamięci podręcznej będzie zawsze spójne z oryginalnymi danymi. Dane w oryginalnym magazynie danych mogą ulec zmianie po został pamięci podręcznej, co powoduje dane w pamięci podręcznej staną się przestarzałe. W związku z tym aplikacje powinny implementować strategię, dzięki której będą upewniać się, że dane w pamięci podręcznej są możliwie aktualne, ale można wykryć i obsługiwać sytuacje, które powstają, gdy dane w pamięci podręcznej stało się przestarzałe. Najbardziej buforowania mechanizmów włączenie pamięci podręcznej skonfigurować wygasanie danych i tym samym zmniejszenie okresu, dla którego dane mogą być nieaktualne.

> [!TIP]
> Ustawienia wygasania domyślny czas podczas konfigurowania pamięci podręcznej. Wiele pamięci podręcznych implementuje wygasania, które unieważniają dane i usuwa go z pamięci podręcznej, jeśli nie jest on dostępny w wybranym okresie. Jednak należy uważać podczas wybierania okres ważności. Jeśli zostanie podjęta zbyt krótki, dane będą wygasać zbyt szybko, a zostanie ona zredukowana korzyści wynikające z pamięci podręcznej. Jeśli staje się zbyt długa zagrożeń danych stają się nieaktualne. W związku z tym czas wygaśnięcia powinny odpowiadać wzorca dostępu aplikacji używających danych.

Po wygaśnięciu danych w pamięci podręcznej, powinny zostać usunięte z pamięci podręcznej i aplikacja musi pobrać dane z oryginalnych danych przechowywania i umieść go z powrotem do pamięci podręcznej.

Istnieje również możliwość, że pamięć podręczna może zapełnić danych może pozostawać zbyt długiego okresu. W związku z tym, żądania do dodawania nowych elementów do pamięci podręcznej może być konieczne usunięcie niektórych elementów w ramach procesu nazywanego *eksmisji*. Usługi pamięci podręcznej zazwyczaj eksmitują dane na zasadzie najmniejszych Najdawniej używane. Istnieją inne zasady eksmisji, w tym ostatnio używane, a pierwszy wejściu — pierwszy na wyjściu. Aby uzyskać więcej informacji, zobacz [wskazówki dotyczące pamięci podręcznej](/azure/architecture/best-practices/caching/).

<a name="caching_images" />

### <a name="caching-images"></a>Pamięć podręczna obrazów

Aplikacja mobilna w ramach aplikacji eShopOnContainers zużywa obrazy produktów zdalny, korzystające z pamięci podręcznej. Te obrazy są wyświetlane przez [ `Image` ](xref:Xamarin.Forms.Image) kontroli i `CachedImage` kontroli dostarczone przez [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) biblioteki.

Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) kontrolka obsługuje buforowanie pobrane zdjęcia. Buforowanie jest domyślnie włączona i będzie przechowywać obraz lokalnie przez 24 godziny. Ponadto można skonfigurować czas wygaśnięcia [ `CacheValidity` ](xref:Xamarin.Forms.UriImageSource.CacheValidity) właściwości. Aby uzyskać więcej informacji, zobacz [pobrany obraz buforowania](~/xamarin-forms/user-interface/images.md#Image_Caching).

Firmy FFImageLoading `CachedImage` formant jest zamiennikiem Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) kontrolki, zapewniając dodatkowe właściwości, które włączyć dodatkowe funkcje. Przez tę funkcję formant udostępnia funkcję buforowania można skonfigurować podczas obsługi błędów i ładowanie symboli zastępczych obrazu. Poniższy przykład kodu pokazuje, jak korzysta z aplikacji mobilnej w ramach aplikacji eShopOnContainers `CachedImage` w kontrolce `ProductTemplate`, który jest używany przez szablon danych [ `ListView` ](xref:Xamarin.Forms.ListView) w kontrolce `CatalogView`:

```xaml
<ffimageloading:CachedImage
    Grid.Row="0"
    Source="{Binding PictureUri}"     
    Aspect="AspectFill">
    <ffimageloading:CachedImage.LoadingPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="default_campaign" />
            <On Platform="UWP" Value="Assets/default_campaign.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.LoadingPlaceholder>
    <ffimageloading:CachedImage.ErrorPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="noimage" />
            <On Platform="UWP" Value="Assets/noimage.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.ErrorPlaceholder>
</ffimageloading:CachedImage>
```

`CachedImage` Kontrolować zestawy `LoadingPlaceholder` i `ErrorPlaceholder` właściwości specyficzne dla platformy obrazów. `LoadingPlaceholder` Właściwość określa obraz wyświetlany podczas obraz określony przez `Source` właściwości jest pobierana, a `ErrorPlaceholder` właściwość określa obraz, który ma być wyświetlana, jeśli wystąpi błąd podczas próby pobrania obrazu określony przez `Source` właściwości.

Jak wskazuje nazwa, `CachedImage` kontroli zapisuje w pamięci podręcznej obrazów zdalnego na urządzeniu przez czas określony przez wartość `CacheDuration` właściwości. Wartość tej właściwości nie jest jawnie ustawiona wartość domyślna w ciągu 30 dni jest stosowany.

## <a name="increasing-resilience"></a>Zwiększa odporność.

Wszystkie aplikacje, które komunikują się ze zdalnymi usługami i zasobami muszą być wrażliwe na błędy przejściowe. Błędy przejściowe obejmują chwilową utratę łączności sieciowej z usługami, tymczasową niedostępność usługi lub przekroczenia limitu czasu, gdy usługa jest zajęta. Błędy te zostaną często usunięte automatycznie, a jeśli akcja zostanie powtórzona z odpowiednim opóźnieniem jest prawdopodobnie się powiedzie.

Błędy przejściowe mogą mieć duży wpływ na postrzegany jakości aplikacji, nawet wtedy, gdy jej ma została dokładnie przetestowana pod kątem wszystkich dających się przewidzieć okoliczności. Zapewnienie niezawodnego działania aplikacji, która komunikuje się z usługami zdalnego, musi mieć możliwość wykonaj następujące czynności:

-   Wykrywanie błędów, gdy wystąpi i ustalić, czy mogą być przejściowe błędy.
-   Spróbuj ponownie wykonać operację, jeśli wykryje, że błąd jest prawdopodobnie przejściowy i śledzić liczbę podejmowanych prób ponownego wykonania operacji.
-   Użyj strategii ponawiania odpowiednie i określa liczbę ponownych prób, opóźnienie między kolejnymi próbami i jakie akcje do wykonania po nieudanej próbie.

Ta obsługa błędu przejściowego można osiągnąć dzięki zawijaniu wszystkie próby uzyskania dostępu do zdalnej usługi w kodzie, który implementuje wzorzec ponawiania.

### <a name="retry-pattern"></a>Wzorzec ponawiania

Jeśli aplikacja wykryje błąd, gdy próbuje wysłać żądanie do usługi zdalnej, go obsłużyć błąd w dowolnym z następujących sposobów:

-   Ponawianie operacji. Aplikację można ponowić próbę żądania zakończonego niepowodzeniem natychmiast.
-   Ponawianie operacji z opóźnieniem. Aplikacja ma oczekiwać odpowiednią ilość czasu przed ponowieniem próby żądania.
-   Anulowanie operacji. Aplikacja powinna anulować operację i zgłosić wyjątek.

Strategia ponawiania powinny być dostosowane do potrzeb biznesowych aplikacji. Na przykład należy zoptymalizować liczbę ponownych prób i interwał na ponawianej operacji ponawiania prób. Jeśli operacja stanowi część interakcji z użytkownikiem, Interwał ponowień powinien być krótki, a tylko kilku ponowień prób podjęto próbę, aby uniknąć konieczności oczekiwania na odpowiedź przez użytkowników. Jeśli operacja się część długotrwałe przepływu pracy, gdzie anulowanie lub ponowne uruchomienie przepływu pracy jest kosztowne lub czasochłonne, należy poczekać dłużej między próbami i ponowić próbę więcej razy.

> [!NOTE]
> Strategia skuteczną ponów próbę, z minimalnym opóźnieniem między próbami i dużą liczbę ponownych prób, może to obniżyć usługę zdalną, która jest uruchomiona, Zamknij, aby lub osiągnięto maksymalną. Ponadto strategia ponawiania prób może wpłynąć na czas odpowiedzi aplikacji stale próbą wykonania operacji kończącej się niepowodzeniem.

Jeśli żądanie nadal kończy się niepowodzeniem po określonej liczbie ponownych prób, jest lepszym rozwiązaniem dla aplikacji, aby uniknąć dalszych żądań do tego samego zasobu i zgłoszenie błędu. Następnie po upływie Ustaw aplikacja może wykonać co najmniej jednego żądania do zasobu, aby zobaczyć, jeśli zakończą się one pomyślnie. Aby uzyskać więcej informacji, zobacz [wzorzec wyłącznika](#circuit_breaker_pattern).

> [!TIP]
> Nigdy nie Implementuj mechanizmu nieskończonego ponawiania prób. Użyj skończoną liczbę ponownych prób lub zaimplementuj [wyłącznik](/azure/architecture/patterns/circuit-breaker/) wzorca, aby umożliwić usłudze do odzyskania.

Aplikacji mobilnej w ramach aplikacji eShopOnContainers nie implementuje obecnie wzorca ponawiania prób w przypadku wysyłania żądań sieci web typu RESTful. Jednak `CachedImage` kontrolki, dostarczone przez [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) biblioteka obsługuje obsługi błędów przejściowych, ponawianie próby ładowania obrazu. W przypadku niepowodzenia ładowania obrazu zostaną wprowadzone dalsze próby. Liczba prób jest określona przez `RetryCount` właściwości, a liczba ponownych nastąpi po chwili określony przez `RetryDelay` właściwości. Jeśli wartości tej właściwości nie są jawnie ustawiona, domyślne wartości zostają zastosowane — 3- `RetryCount` właściwość i 250ms dla `RetryDelay` właściwości. Aby uzyskać więcej informacji na temat `CachedImage` sterowania, zobacz [pamięci podręcznej obrazów](#caching_images).

Aplikacja referencyjna w ramach aplikacji eShopOnContainers implementacji wzorca ponawiania prób. Aby uzyskać więcej informacji, w tym omówienie sposób łączenia ze wzorca do ponawiania `HttpClient` klasy, zobacz [Mikrousługi .NET: Architektura aplikacji kontenerowych nimi .NET](https://aka.ms/microservicesebook).

Aby uzyskać więcej informacji na temat wzorca ponawiania, zobacz [ponów](/azure/architecture/patterns/retry/) wzorca.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>Wzorzec wyłącznika

W niektórych sytuacjach błędów może wystąpić z powodu przewidywanego zdarzenia, które trwać dłużej, aby rozwiązać problem. Te błędy mogą należeć do zakresu od częściowej utraty łączności do całkowitej awarii usługi. W takich przypadkach jest sensu dla aplikacji ponowić próbę wykonania operacji, która najprawdopodobniej nie powiedzie się, a zamiast tego należy przyjąć, że operacja zakończyła się niepowodzeniem i odpowiednie obsłużenie tego błędu.

Wzorzec wyłącznika może uniemożliwić aplikacji wielokrotne powtarzanie prób wykonania można wykonać operacji, która prawdopodobnie się nie powiedzie, a także aplikacji wykryć, czy błąd został naprawiony.

> [!NOTE]
> Celem wzorzec wyłącznika różni się od wzorca ponawiania. Wzorzec ponawiania umożliwia aplikacji ponowić próbę wykonania operacji w założeniu, że się powiedzie. Wzorzec wyłącznika zapobiega aplikacji wykonywanie operacji, która prawdopodobnie się nie powiedzie.

Wyłącznik działa jako serwer proxy dla operacji, które może zakończyć się niepowodzeniem. Serwer proxy, należy monitorować liczbę ostatnich awarii, które miały miejsce i dzięki tym informacjom można zdecydować, czy zezwala na tę operację, aby kontynuować, lub natychmiast zwrócony wyjątek.

W ramach aplikacji eShopOnContainers aplikacji mobilnej nie implementuje obecnie wzorzec wyłącznika. Jednak ramach aplikacji eShopOnContainers robi. Aby uzyskać więcej informacji, zobacz [Mikrousługi .NET: Architektura aplikacji kontenerowych nimi .NET](https://aka.ms/microservicesebook).

> [!TIP]
> Połącz wzorców ponawiania prób i wyłącznik. Aplikację można połączyć wzorców ponawiania prób i wyłącznik przy użyciu wzorca ponawiania do wywołania operacji za pośrednictwem wyłącznika. Jednak Logika ponawiania powinna uwzględniać ewentualne wyjątki zwracane przez wyłącznik i przerwać ponawianie prób, jeśli wyłącznik wskazuje, że błąd nie jest przejściowy.

Aby uzyskać więcej informacji na temat wzorzec wyłącznika, zobacz [wyłącznik](/azure/architecture/patterns/circuit-breaker/) wzorca.

## <a name="summary"></a>Podsumowanie

Wiele nowoczesnych rozwiązań opartych na sieci web wprowadzić korzystanie z usług sieci web hostowanych na serwerach sieci web w celu zapewnienia funkcji zdalnego klienta aplikacji. Operacje, które uwidacznia Usługa sieci web stanowi interfejs API sieci web i aplikacje klienckie powinno być możliwe korzystanie z interfejsu API sieci web nie wiedząc o tym, jak zaimplementować danych lub operacji, które uwidacznia interfejs API.

Można zwiększyć wydajność aplikacji, buforując rzadziej używanych danych do szybkiego magazynu znajdującego się blisko aplikacji. Aplikacje można zaimplementować buforowania read-through ze wzorcem odkładania do pamięci podręcznej. Ten wzorzec Określa, czy element jest aktualnie w pamięci podręcznej. Jeśli element nie znajduje się w pamięci podręcznej, ma odczytu z magazynu danych i dodane do pamięci podręcznej.

Podczas komunikacji z interfejsów API sieci web, aplikacje muszą być wrażliwe na błędy przejściowe. Błędy przejściowe obejmują chwilową utratę łączności sieciowej z usługami, tymczasową niedostępność usługi lub przekroczenia limitu czasu, gdy usługa jest zajęta. Błędy te zostaną często usunięte automatycznie, a jeśli akcja zostanie powtórzona z odpowiednim opóźnieniem, to może się powieść. Dlatego aplikacje powinna opakować wszystkie próby uzyskania dostępu do internetowego interfejsu API w kodzie, który implementuje mechanizmem obsługi błędów przejściowych.


## <a name="related-links"></a>Linki pokrewne

- [Pobierz książkę elektroniczną (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [ramach aplikacji eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
