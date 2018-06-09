---
title: Uzyskiwanie dostępu do danych zdalnych
description: W tym rozdziale opisano, jak aplikacji mobilnej eShopOnContainers uzyskuje dostęp do danych z konteneryzowanych mikrousług.
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: a140560731cc68dd85c97dc5a89aedcb32abd405
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242092"
---
# <a name="accessing-remote-data"></a>Uzyskiwanie dostępu do danych zdalnych

Wiele nowoczesnych rozwiązań opartych na sieci web wprowadzić korzystanie z usług sieci web, znajdujących się na serwerach sieci web, umożliwiają korzystanie z funkcji zdalnego klienta aplikacji. Operacje, które udostępnia usługi sieci web stanowi interfejs API sieci web.

Aplikacje klienta powinno być możliwe korzystanie z interfejsu API sieci web bez znajomości sposobu implementacji danych lub operacji, które ujawnia interfejsu API. Wymaga to, że interfejs API przestrzega wspólne normy umożliwiające Usługa klienta sieci web i aplikacji uzgodnić formatów danych użycia i struktury danych, które są wymieniane między aplikacjami klienta i usługi sieci web.

## <a name="introduction-to-representational-state-transfer"></a>Wprowadzenie do Representational State Transfer

Representational State (Transfer REST) jest architektury styl tworzenia systemów rozproszonych, w oparciu o hipermedialnych. Główną zaletą modelu REST jest jest oparta na standardach otwarty i nie jest powiązana implementacji modelu lub aplikacje klienckie, które do niego dostęp do określonej implementacji. W związku z tym usługi sieci web REST można zaimplementować za pomocą programu Microsoft ASP.NET Core MVC, a aplikacje klienckie można rozwijać przy użyciu dowolnego języka oraz zestawu narzędzi, które można wygenerować żądania HTTP i przeanalizować odpowiedzi HTTP.

REST model wykorzystuje schemat nawigacji do reprezentowania obiektów i usług w sieci określonych jako zasoby. Systemy, które implementuje REST zwykle używają protokołu HTTP do przesyłania żądań dostępu do tych zasobów. W takich systemach aplikacji klient przesyła żądanie w formie identyfikatora URI identyfikujący zasób, a metoda HTTP (na przykład GET, POST, PUT i DELETE), która wskazuje operacji do wykonania dla tego zasobu. Treść żądania HTTP zawiera wszystkie dane wymagane do wykonania tej operacji.

> [!NOTE]
> REST definiuje bezstanowe żądanie model. W związku z tym żądań HTTP musi być niezależna i mogą wystąpić w dowolnej kolejności.

Odpowiedź z REST żądanie sprawia, że użycie standardowe kody stanu HTTP. Na przykład żądania, która zwraca prawidłowe dane powinna zawierać kod odpowiedzi HTTP 200 (OK), gdy żądanie nie może znaleźć lub usunąć określonego zasobu powinna zwrócić odpowiedzi, który zawiera kod stanu HTTP 404 (nie znaleziono).

RESTful interfejsu API sieci web udostępnia zestaw połączonych zasobów i udostępnia podstawowych operacji, które umożliwiają aplikacji do manipulowania tych zasobów i łatwe przechodzenie między nimi. Z tego powodu identyfikatory URI, który stanowi interfejs API RESTful typowe w sieci web są ukierunkowane na dane, które udostępnia ona i użyj funkcji podana przy użyciu protokołu HTTP do działania na tych danych.

Dane uwzględnione przez aplikację klienta w żądaniu HTTP i powiązanych komunikatów odpowiedzi z serwera sieci web może być przedstawiony w różnych formatach, znany jako typów nośników. Gdy aplikacja kliencka wysyła żądanie, która zwraca dane w treści wiadomości, można określić typy nośnika może obsłużyć w `Accept` nagłówka żądania. Jeśli serwer sieci web obsługuje ten typ nośnika, może odpowiadać za pomocą odpowiedzi, który zawiera `Content-Type` nagłówek, który określa format danych w treści wiadomości. Następnie jest odpowiedzialny za aplikacji klienta, można przeanalizować komunikatu odpowiedzi i odpowiednio interpretuj wyniki w treści wiadomości.

Aby uzyskać więcej informacji na temat REST, zobacz [projekt interfejsu API](/azure/architecture/best-practices/api-design/) i [implementacji interfejsu API](/azure/architecture/best-practices/api-implementation/).

## <a name="consuming-restful-apis"></a>Korzystanie z interfejsów API RESTful

Aplikacja mobilna eShopOnContainers używa wzorca Model-View-ViewModel (MVVM), a elementy modelu reprezentuje wzorzec jednostek domeny używane w aplikacji. Klasy kontrolera i repozytorium w aplikacji odwołanie eShopOnContainers zaakceptować i zwracać wiele z tych obiektów modelu. W związku z tym są używane jako obiekty transfer danych (DTOs) zawierających dane, które jest przekazywane między aplikacjami mobilnymi i mikrousług konteneryzowanych. Główne zaletą używania DTOs do przekazywania danych do i odbierać dane z usługi sieci web jest poprzez przekazanie większej ilości danych w jednym wywołaniu zdalnym, aplikację można zmniejszyć liczbę wywołań zdalnych, które należy wprowadzić.

### <a name="making-web-requests"></a>Tworzenie żądania sieci Web

Używa aplikacji mobilnej eShopOnContainers `HttpClient` klasa do tworzenia żądań za pośrednictwem protokołu HTTP z JSON używany jako typ nośnika. Ta klasa udostępnia funkcje asynchronicznego wysyłania żądań HTTP i odbierania odpowiedzi HTTP z identyfikatora URI zidentyfikowane zasobów. `HttpResponseMessage` Klasa reprezentuje komunikat odpowiedzi HTTP odebrane z interfejsu API REST, po dokonaniu żądania HTTP. Zawiera informacje o odpowiedzi, w tym kod stanu, nagłówki i wszelkie treści. `HttpContent` Klasa reprezentuje treści HTTP i nagłówków zawartości, takich jak `Content-Type` i `Content-Encoding`. Zawartość może zostać odczytany przy użyciu dowolnego `ReadAs` metod, takich jak `ReadAsStringAsync` i `ReadAsByteArrayAsync`, zależnie od formatu danych.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>Tworzenie żądania GET

`CatalogService` Klasa jest używana do zarządzania procesu pobierania danych z katalogu mikrousługi. W `RegisterDependencies` metody w `ViewModelLocator` klasy, `CatalogService` klasy jest zarejestrowany jako typ mapowania przed `ICatalogService` typu o Autofac kontenera iniekcji zależności. Następnie, gdy wystąpienie klasy `CatalogViewModel` jego konstruktor klasy jest tworzony, akceptuje `ICatalogService` wpisz Autofac rozwiązanie, które zwracanie wystąpienia `CatalogService` klasy. Aby uzyskać więcej informacji na temat iniekcji zależności, zobacz [wprowadzenie do iniekcji zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Rysunek 10-1 pokazuje interakcje klasy, które będzie odczytywać dane wykazu mikrousługi katalogu do wyświetlania przez `CatalogView`.

[![](accessing-remote-data-images/catalogdata.png "Pobieranie danych z wykazu mikrousługi")](accessing-remote-data-images/catalogdata-large.png#lightbox "pobieranie danych z mikrousługi katalogu")

**Rysunek 10-1**: pobieranie danych z mikrousługi katalogu

Gdy `CatalogView` przejście, `OnInitialize` metoda `CatalogViewModel` nosi nazwę klasy. Ta metoda pobiera dane wykazu z mikrousługi katalogu, jak pokazano w poniższym przykładzie:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

Ta metoda wywołuje `GetCatalogAsync` metody `CatalogService` wystąpienie, które zostało dodane do `CatalogViewModel` przez Autofac. Poniższy kod przedstawia przykład `GetCatalogAsync` metody:

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

Ta metoda tworzy identyfikator URI, który identyfikuje żądanie zostanie wysłane do zasobu, a następnie używa `RequestProvider` klasę, aby wywołać metodę GET HTTP na zasobie, przed zwróceniem wyników do `CatalogViewModel`. `RequestProvider` Klasa zawiera funkcje, które przesyła żądanie w formie identyfikatora URI, który identyfikuje zasób, metoda HTTP, która wskazuje operacji do wykonania dla tego zasobu i treści, która zawiera wszystkie dane wymagane do wykonania tej operacji. Aby uzyskać informacje o sposobie `RequestProvider` wstrzykuje się klasy `CatalogService class`, zobacz [wprowadzenie do iniekcji zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Poniższy kod przedstawia przykład `GetAsync` metoda `RequestProvider` klasy:

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

Ta metoda wywołuje `CreateHttpClient` metodę, która zwraca wystąpienie klasy `HttpClient` z nagłówków odpowiednie wartości. Następnie przesyła żądanie GET asynchroniczne do zasobu wskazywanego przez identyfikator URI, odpowiedzi są przechowywane w `HttpResponseMessage` wystąpienia. `HandleResponse` Następnie wywoływana jest metoda, która zgłasza wyjątek, jeśli odpowiedź nie zawiera Powodzenie kod stanu HTTP. Następnie jest do odczytu odpowiedzi jako ciąg przekonwertowany z formatu JSON do `CatalogRoot` obiektu i powrót do `CatalogService`.

`CreateHttpClient` Metody przedstawiono w poniższym przykładzie kodu:

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

Ta metoda tworzy nowe wystąpienie klasy `HttpClient` klasy i zestawy `Accept` nagłówka żądania przez `HttpClient` wystąpienie do `application/json`, co oznacza, że oczekuje, że treść odpowiedzi zostanie sformatowany przy użyciu. Następnie, jeśli token dostępu został przekazany jako argument `CreateHttpClient` metody, jest ona dodawana do `Authorization` nagłówka żądania przez `HttpClient` wystąpienia poprzedzona ciągiem `Bearer`. Aby uzyskać więcej informacji na temat autoryzacji, zobacz [autoryzacji](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Gdy `GetAsync` metody w `RequestProvider` klasy wywołania `HttpClient.GetAsync`, `Items` metody w `CatalogController` wywołaniu klasy w projekcie Catalog.API, które przedstawiono w poniższym przykładzie kodu:

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

Ta metoda pobiera dane katalogu z bazy danych SQL za pomocą platformy EntityFramework i zwraca go jako wiadomości odpowiedzi, która zawiera kod stanu HTTP sukcesu i sformatowany zbiór JSON `CatalogItem` wystąpień.

#### <a name="making-a-post-request"></a>Tworzenie żądania POST

`BasketService` Klasa jest używana do zarządzania pobierania danych i zaktualizować procesu, który z koszyka mikrousługi. W `RegisterDependencies` metody w `ViewModelLocator` klasy, `BasketService` klasy jest zarejestrowany jako typ mapowania przed `IBasketService` typu o Autofac kontenera iniekcji zależności. Następnie, gdy wystąpienie klasy `BasketViewModel` jego konstruktor klasy jest tworzony, akceptuje `IBasketService` wpisz Autofac rozwiązanie, które zwracanie wystąpienia `BasketService `klasy. Aby uzyskać więcej informacji na temat iniekcji zależności, zobacz [wprowadzenie do iniekcji zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Rysunek 10-2 przedstawia interakcje klasy wysyłających dane koszyka, wyświetlane przez `BasketView`, aby mikrousługi koszyka.

[![](accessing-remote-data-images/basketdata.png "Wysyłanie danych do koszyka mikrousługi")](accessing-remote-data-images/basketdata-large.png#lightbox "wysyłania danych do mikrousługi koszyka")

**Rysunek 10-2**: wysyłanie danych do mikrousługi koszyka

Gdy element zostanie dodany do koszyka `ReCalculateTotalAsync` metoda `BasketViewModel` nosi nazwę klasy. Ta metoda aktualizacji łączną liczbę elementów w koszyku i wysyła dane koszyka mikrousługi koszyka, jak pokazano w poniższym przykładzie:

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

Ta metoda wywołuje `UpdateBasketAsync` metody `BasketService` wystąpienie, które zostało dodane do `BasketViewModel` przez Autofac. Poniżej przedstawiono metody `UpdateBasketAsync` metody:

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

Ta metoda tworzy identyfikator URI, który identyfikuje żądanie zostanie wysłane do zasobu, a następnie używa `RequestProvider` klasy można wywołać metody POST HTTP na zasobie, przed zwróceniem wyników do `BasketViewModel`. Należy pamiętać, że token dostępu pochodzi IdentityServer podczas procesu uwierzytelniania jest wymagane do autoryzowania żądań do koszyka mikrousługi. Aby uzyskać więcej informacji na temat autoryzacji, zobacz [autoryzacji](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Poniższy przykładowy kod przedstawia jedną z `PostAsync` metod w `RequestProvider` klasy:

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

Ta metoda wywołuje `CreateHttpClient` metodę, która zwraca wystąpienie klasy `HttpClient` z nagłówków odpowiednie wartości. Następnie przesyła żądanie POST asynchroniczne do zasobu wskazywanego przez identyfikator URI, z koszyka serializacji danych są wysyłane w formacie JSON i odpowiedzi są przechowywane w `HttpResponseMessage` wystąpienia. `HandleResponse` Następnie wywoływana jest metoda, która zgłasza wyjątek, jeśli odpowiedź nie zawiera Powodzenie kod stanu HTTP. Następnie, jest do odczytu odpowiedzi jako ciąg przekonwertowany z formatu JSON do `CustomerBasket` obiektu i powrót do `BasketService`. Aby uzyskać więcej informacji na temat `CreateHttpClient` metody, zobacz [wprowadzania żądanie GET](#making_a_get_request).

Gdy `PostAsync` metody w `RequestProvider` klasy wywołania `HttpClient.PostAsync`, `Post` metody w `BasketController` wywołaniu klasy w projekcie Basket.API, które przedstawiono w poniższym przykładzie kodu:

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

Ta metoda używa wystąpienia `RedisBasketRepository` klasy do utrwalenia danych koszyka do pamięci podręcznej Redis i zwraca go jako sformatowane komunikat odpowiedzi, Powodzenie kod stanu HTTP i JSON `CustomerBasket` wystąpienia.

#### <a name="making-a-delete-request"></a>Żądanie usunięcia wprowadzania

Rysunek 10-3 przedstawia interakcje klasy, które usuwania danych koszyka z mikrousługi koszyka `CheckoutView`.

![](accessing-remote-data-images/checkoutdata.png "Usuwania danych z koszyka mikrousługi")

**Rysunek 10-3**: usuwanie danych z koszyka mikrousługi

Po wywołaniu realizacji `CheckoutAsync` metoda `CheckoutViewModel` nosi nazwę klasy. Ta metoda tworzy nową kolejność, przed wyczyszczeniem koszyka, jak pokazano w poniższym przykładzie:

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

Ta metoda wywołuje `ClearBasketAsync` metody `BasketService` wystąpienie, które zostało dodane do `CheckoutViewModel` przez Autofac. Poniżej przedstawiono metody `ClearBasketAsync` metody:

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

Ta metoda tworzy identyfikator URI, który identyfikuje żądanie zostanie wysłane do zasobu, a następnie używa `RequestProvider` klasę, aby wywołać metodę DELETE HTTP w zasobie. Należy pamiętać, że token dostępu pochodzi IdentityServer podczas procesu uwierzytelniania jest wymagane do autoryzowania żądań do koszyka mikrousługi. Aby uzyskać więcej informacji na temat autoryzacji, zobacz [autoryzacji](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Poniższy kod przedstawia przykład `DeleteAsync` metoda `RequestProvider` klasy:

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

Ta metoda wywołuje `CreateHttpClient` metodę, która zwraca wystąpienie klasy `HttpClient` z nagłówków odpowiednie wartości. Następnie przesyła on asynchroniczne żądanie usunięcia zasobu określonego przez identyfikator URI. Aby uzyskać więcej informacji na temat `CreateHttpClient` metody, zobacz [wprowadzania żądanie GET](#making_a_get_request).

Gdy `DeleteAsync` metody w `RequestProvider` klasy wywołania `HttpClient.DeleteAsync`, `Delete` metody w `BasketController` wywołaniu klasy w projekcie Basket.API, które przedstawiono w poniższym przykładzie kodu:

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

Ta metoda używa wystąpienia `RedisBasketRepository` klasy do usuwania danych koszyka z pamięci podręcznej Redis.

## <a name="caching-data"></a>Buforowanie danych

Można zwiększyć wydajność aplikacji przez buforowanie często używane dane do szybkiego magazynu, który znajduje się Zamknij, aby aplikacja. Jeśli szybkiego magazynu znajduje się bliżej aplikacji niż oryginalne źródło, a następnie buforowanie może znacznie poprawić odpowiedź podczas pobierania danych.

Najczęściej buforowania jest read-through buforowania, gdy aplikacja pobiera dane za pomocą odwołań do pamięci podręcznej. Jeśli dane nie będzie w pamięci podręcznej, ma ona pobrana z magazynu danych i dodana do pamięci podręcznej. Aplikacje można zaimplementować read-through buforowanie przy użyciu wzorca Zarezerwuj pamięci podręcznej. Ten wzorzec Określa, czy element jest obecnie w pamięci podręcznej. Jeśli element nie znajduje się w pamięci podręcznej, ma odczytu z magazynu danych i dodana do pamięci podręcznej. Aby uzyskać więcej informacji, zobacz [Zarezerwuj pamięć podręczna](/azure/architecture/patterns/cache-aside/) wzorca.

> [!TIP]
> Dane pamięci podręcznej, która jest często odczytywane i który zmienia się rzadko. Te dane można dodać do pamięci podręcznej na żądanie po raz pierwszy, że jest ono pobierane przez aplikację. Oznacza to, że aplikacja musi pobrać dane tylko jeden raz z magazynu danych, a dostęp do tego może być spełnione przy użyciu pamięci podręcznej.

Aplikacje rozproszone, takie jak eShopOnContainers zawierają odwołania do aplikacji, powinien zawierać jedną lub obie następujące pamięci podręcznych:

-   Udostępnionej pamięci podręcznej, które są dostępne dla wielu procesów lub maszyny.
-   Prywatne pamięci podręcznej, gdzie danych jest przechowywanych lokalnie na urządzeniu, na którym uruchomiono aplikację.

Aplikacji mobilnej eShopOnContainers używa prywatnego pamięci podręcznej, gdzie danych jest przechowywanych lokalnie na urządzeniu, na którym jest uruchomione wystąpienie aplikacji. Informacje używane przez aplikację odwołanie eShopOnContainers pamięci podręcznej, zobacz [Mikrousług .NET: Architektura konteneryzowanych aplikacji .NET](https://aka.ms/microservicesebook).

> [!TIP]
> Pamięć podręczna można traktować jako magazyn danych przejściowy może zniknąć w dowolnym momencie. Upewnij się, że dane są przechowywane w oryginalnego magazynu danych, a także pamięci podręcznej. Prawdopodobieństwo utraty danych są następnie zminimalizowany, jeśli pamięć podręczna jest niedostępny.

### <a name="managing-data-expiration"></a>Zarządzanie Data wygaśnięcia

Jest to niemożliwe do oczekiwane dane z pamięci podręcznej zawsze będzie zgodne z danymi. Dane w magazynie danych w oryginalnej mogą ulec zmianie po zostały pamięci podręcznej, powodując buforowane dane do nieodświeżone. W związku z tym aplikacje należy zaimplementować strategię można mieć pewność, że dane w pamięci podręcznej jest jak to możliwe, aktualny, ale można wykryć i obsługi sytuacji, które powstają, gdy dane w pamięci podręcznej jest nieodświeżona. Najbardziej buforowania mechanizmów włączyć pamięć podręczną umożliwiać wygasić dane, a więc skrócić okres, dla których dane mogą być nieaktualne.

> [!TIP]
> Ustawienia okresu ważności domyślny czas podczas konfigurowania pamięci podręcznej. Wiele pamięci podręcznych implementuje wygaśnięcia, która unieważnia dane i usuwa go z pamięci podręcznej, jeśli nie jest on dostępny w określonym przedziale. Jednak należy uważać podczas wybierania okresu wygaśnięcia. Jeśli jest on zbyt krótki, zbyt szybko wygaśnie danych i zmniejszy korzyści buforowania. Jeśli jest on zbyt długi, ryzyka danych stają się stare. W związku z tym czas wygaśnięcia powinna odpowiadać wzorca dostępu dla aplikacji, które używają danych.

Po danych z pamięci podręcznej wygasa, powinna zostać usunięta z pamięci podręcznej i aplikacji musi pobrać dane z oryginalnych danych przechowywania i umieść go do pamięci podręcznej.

Istnieje również możliwość, że bufor może zapełnić danych może pozostawać zbyt długiego okresu. W związku z tym żądań do dodawania nowych elementów do pamięci podręcznej może być konieczne usunięcie niektórych elementów w ramach procesu nazywanego *wykluczenia*. Usługi buforowania zwykle wykluczenie danych na podstawie najmniej niedawno używane. Istnieją inne zasady wykluczenia, w tym ostatnio używane, a pierwszy w pierwszym limit. Aby uzyskać więcej informacji, zobacz [buforowanie wskazówki](/azure/architecture/best-practices/caching/).

<a name="caching_images" />

### <a name="caching-images"></a>Buforowanie obrazów

Aplikacja mobilna eShopOnContainers zużywa obrazy produktu zdalnego, które korzystają z pamięci podręcznej. Te obrazy są wyświetlane przez [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) kontroli i `CachedImage` kontroli, podana przez [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) biblioteki.

Platformy Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) sterowanie obsługuje buforowanie pobrany obrazów. Buforowanie jest włączone domyślnie i zapisze obraz lokalnie przez 24 godziny. Ponadto można skonfigurować czas wygaśnięcia [ `CacheValidity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) właściwości. Aby uzyskać więcej informacji, zobacz [pobrany obraz buforowanie](~/xamarin-forms/user-interface/images.md#Image_Caching).

W FFImageLoading `CachedImage` formantu nie zastępuje platformy Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) kontroli, zapewniając dodatkowe właściwości, które umożliwiają dodatkowych funkcji. Wśród tej funkcji formantu udostępnia funkcję buforowania można skonfigurować podczas obsługi błędów i ładowanie symboli zastępczych obrazu. Poniższy przykład kodu pokazuje, jak korzysta z aplikacji mobilnej eShopOnContainers `CachedImage` kontroli w `ProductTemplate`, który jest używany przez szablon danych [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kontroli w `CatalogView`:

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

`CachedImage` Kontrolować zestawy `LoadingPlaceholder` i `ErrorPlaceholder` właściwości specyficzne dla platformy obrazów. `LoadingPlaceholder` Właściwość określa obraz do wyświetlenia podczas obraz określony przez `Source` właściwości jest pobierana i `ErrorPlaceholder` właściwość określa obrazu, który będzie wyświetlany, jeśli wystąpi błąd podczas próby pobrania obrazu określony przez `Source` właściwości.

Jak wskazuje nazwę, `CachedImage` kontroli buforuje zdalnego obrazów na urządzeniu przez czas określony przez wartość `CacheDuration` właściwości. Wartość tej właściwości nie jest jawnie ustawiona, jest stosowana wartość domyślna wynosi 30 dni.

## <a name="increasing-resilience"></a>Zwiększa odporność

Wszystkie aplikacje, które komunikują się ze zdalnej usługi i zasoby musi być wrażliwe na błędów przejściowych. Błędów przejściowych obejmują chwilowo utratę łączności sieciowej z usługami, tymczasowej niedostępności usługi lub przekroczenia limitu czasu, który wystąpić, gdy usługa jest zajęta. Często są rozwiązywane te błędy, a jeśli akcja jest powtarzany opóźnieniem odpowiedniego może się powieść.

Błędów przejściowych może mieć duży wpływ na postrzegany jakości aplikacji, nawet wtedy, gdy ma została dokładnie przetestowana we wszystkich okolicznościach przewidywalne. W celu zapewnienia niezawodnego działania aplikacji, która komunikuje się z usługi zdalnej, musi mieć możliwość wykonaj następujące czynności:

-   Wykryj błędów, kiedy występuje i sprawdzić, czy usterek są może być przejściowy.
-   Spróbuj ponownie wykonać operację, gdy ustali, że ten błąd jest może być przejściowy i śledzenie liczby została podjęta ponowna próba wykonania operacji.
-   Użyć odpowiednich strategii, które określa liczbę ponownych prób, opóźnienie między kolejnymi próbami i akcje do wykonania po nieudanej próby.

Obsługa tego błędu przejściowego można osiągnąć zawijania wszystkie próby dostępu do zdalnej usługi w kodzie, który implementuje wzorzec ponów próbę.

### <a name="retry-pattern"></a>Spróbuj ponownie wzorca

Jeśli aplikacja wykryje błąd przy próbie Wyślij żądanie do zdalnej usługi, może obsługiwać awarii w dowolnej z następujących sposobów:

-   Ponawianie operacji. Aplikację można ponowić próbę żądania niepowodzenie natychmiast.
-   Ponawianie operacji z opóźnieniem. Aplikacja ma oczekiwać na odpowiednią ilość czasu, przed podjęciem próby wykonania żądania.
-   Anulowanie operacji. Aplikację należy anulować operację i Zgłoś wyjątek.

Strategia ponownych prób powinien dopasowane do dopasowania wymagań biznesowych aplikacji. Na przykład należy zoptymalizować liczby ponownych prób, a następnie spróbuj ponownie interwał działania podejmowane próby. Jeśli operacja jest częścią interakcji z użytkownikiem, interwał ponawiania powinna być krótki i próbował uniknąć oczekiwania na odpowiedź użytkowników tylko kilka prób. Jeśli operacja jest długo działające przepływu pracy, gdzie anulowanie i ponowne uruchomienie przepływu pracy jest kosztowna lub czasochłonne, jest już oczekiwania między próbami i ponów próbę wykonania więcej razy.

> [!NOTE]
> Agresywne strategii z minimalnym opóźnieniem między próbami i dużą liczbę ponownych prób, może to obniżyć usługi zdalnej, który jest uruchomiony, Zamknij, aby lub osiągnięto maksymalną liczbę. Ponadto strategii ponawiania może wpłynąć na czas odpowiedzi aplikacji Jeśli stale próby wykonania operacji się niepowodzeniem.

Jeśli żądanie jest nadal nie po upływie liczby ponownych prób, lepiej jest dla aplikacji, aby uniknąć dalszych żądań do tego samego zasobu i zgłoś błąd. Następnie po upływie ustawione, aplikacja może wykonać co najmniej jedno żądanie do zasobu, aby zobaczyć, czy są one powiodło się. Aby uzyskać więcej informacji, zobacz [wzorzec wyłącznika](#circuit_breaker_pattern).

> [!TIP]
> Nigdy nie implementuje mechanizm ponawiania nieskończona. Użyj skończoną liczbę ponownych prób, lub zaimplementuj [wyłącznika](/azure/architecture/patterns/circuit-breaker/) wzorzec, aby zezwalać na usługi do odzyskania.

EShopOnContainers aplikacji mobilnej nie implementuje obecnie wzorzec ponownych prób w przypadku wysyłania żądań sieci web RESTful. Jednak `CachedImage` kontroli, podana przez [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) biblioteka obsługuje obsługi błędu przejściowego przez ponowienie ładowania obrazu. W przypadku niepowodzenia podczas ładowania obrazu zostaną podjęte dalsze próby. Liczba prób jest określona przez `RetryCount` właściwości oraz liczbę ponownych prób nastąpi z opóźnieniem określony przez `RetryDelay` właściwości. Jeśli wartości tych właściwości nie są jawnie ustawiona, domyślne wartości są stosowane — 3- `RetryCount` właściwości i 250ms dla `RetryDelay` właściwości. Aby uzyskać więcej informacji na temat `CachedImage` sterowania, zobacz [buforowanie obrazów](#caching_images).

Aplikacja referencyjna eShopOnContainers implementują wzorzec ponów próbę. Aby uzyskać więcej informacji, łącznie z omówieniem jak połączyć wzorzec ponownych prób z `HttpClient` , zobacz [Mikrousług .NET: Architektura konteneryzowanych aplikacji .NET](https://aka.ms/microservicesebook).

Aby uzyskać więcej informacji o wzorcu ponownych prób, zobacz [ponów](/azure/architecture/patterns/retry/) wzorca.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>Wzorzec wyłącznika

W niektórych sytuacjach błędów może wystąpić z powodu przewidywanego zdarzenia, które trwać dłużej, aby rozwiązać problem. Te błędy mogą należeć do zakresu z częściowa utraty połączenia do całkowitej awarii usługi. W takich przypadkach jest bezcelowe dla aplikacji ponowić próbę wykonania operacji, który prawdopodobnie nie powiedzie się, a zamiast tego należy przyjąć, że operacja nie powiodła się i odpowiednio je obsłużyć tego błędu.

Wzorzec wyłącznika uniemożliwi aplikacji wielokrotne próby wykonania operacji, który prawdopodobnie ulegnie awarii, umożliwiając również aplikacji na wykrywanie, czy ten błąd został rozwiązany.

> [!NOTE]
> Celem wzorzec wyłącznika różni się od wzorca ponów próbę. Wzorzec ponawiania umożliwia aplikacji ponowić próbę wykonania operacji w oczekiwania, który będzie powiodło się. Wzorzec wyłącznika uniemożliwia aplikacjom wykonywania operacji, który prawdopodobnie nie powiedzie się.

Wyłącznik działa jako serwer proxy dla operacji, które może zakończyć się niepowodzeniem. Serwer proxy należy monitorować liczbę Niedawne awarie, które wystąpiły, a dzięki tym informacjom można zdecydować, czy zezwala na tę operację, aby kontynuować, lub natychmiast zwrócił elementu exception.

Aplikacji mobilnej eShopOnContainers aktualnie nie implementuje wzorca wyłącznika. Jednak wykonuje eShopOnContainers. Aby uzyskać więcej informacji, zobacz [Mikrousług .NET: Architektura konteneryzowanych aplikacji .NET](https://aka.ms/microservicesebook).

> [!TIP]
> Łączenie ponawiania i wyłącznika wzorce. Aplikację można połączyć wzorce ponawiania i wyłącznika za pomocą wzorca ponownych prób do wywołania operacji za pomocą wyłącznika. Jednak Logika ponawiania powinny być wrażliwe na wszystkie wyjątki zwrócony przez wyłącznik i Porzuć ponownych prób, jeśli wyłącznik wskazuje, że nie jest przejściowy błąd.

Aby uzyskać więcej informacji na temat wzorzec wyłącznika zobacz [wyłącznika](/azure/architecture/patterns/circuit-breaker/) wzorca.

## <a name="summary"></a>Podsumowanie

Wiele nowoczesnych rozwiązań opartych na sieci web wprowadzić korzystanie z usług sieci web, znajdujących się na serwerach sieci web, umożliwiają korzystanie z funkcji zdalnego klienta aplikacji. Operacje, które udostępnia usługi sieci web stanowi interfejs API sieci web i aplikacje klienckie powinno być możliwe korzystanie z interfejsu API sieci web bez znajomości sposobu implementacji danych lub operacji, które udostępnia interfejs API.

Można zwiększyć wydajność aplikacji przez buforowanie często używane dane do szybkiego magazynu, który znajduje się Zamknij, aby aplikacja. Aplikacje można zaimplementować read-through buforowanie przy użyciu wzorca Zarezerwuj pamięci podręcznej. Ten wzorzec Określa, czy element jest obecnie w pamięci podręcznej. Jeśli element nie znajduje się w pamięci podręcznej, ma odczytu z magazynu danych i dodana do pamięci podręcznej.

Podczas komunikacji z interfejsów API sieci web, aplikacje muszą mieć wrażliwe na błędów przejściowych. Błędów przejściowych obejmują chwilowo utratę łączności sieciowej z usługami, tymczasowej niedostępności usługi lub przekroczenia limitu czasu, który wystąpić, gdy usługa jest zajęta. Często są rozwiązywane te błędy, a jeśli akcja jest powtarzany opóźnieniem odpowiednie, jest prawdopodobnie się powiedzie. W związku z tym aplikacji powinna być zawijana, wszystkie próby dostępu do sieci web interfejsu API w kodzie, który implementuje mechanizmu obsługi błędu przejściowego.


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
