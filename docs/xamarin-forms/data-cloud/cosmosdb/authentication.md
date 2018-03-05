---
title: "Uwierzytelnianie użytkowników z bazą danych dokumentów platformy Azure rozwiązania Cosmos bazy danych"
description: "Bazy dokumentów w usłudze Azure DB rozwiązania Cosmos obsługuje partycjonowanych kolekcje, które mogą znajdować się na różnych serwerach i partycji, podczas obsługi nieograniczony Magazyn oraz przepustowość. W tym artykule wyjaśniono, jak połączyć kontroli dostępu z kolekcji podzielone na partycje, dzięki czemu użytkownik może uzyskiwać dostęp tylko do swoich własnych dokumentów w aplikacji platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: a16f72e6aaee93aa313aff0aba23887b51acf701
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="authenticating-users-with-an-azure-cosmos-db-document-database"></a>Uwierzytelnianie użytkowników z bazą danych dokumentów platformy Azure rozwiązania Cosmos bazy danych

_Bazy dokumentów w usłudze Azure DB rozwiązania Cosmos obsługuje partycjonowanych kolekcje, które mogą znajdować się na różnych serwerach i partycji, podczas obsługi nieograniczony Magazyn oraz przepustowość. W tym artykule wyjaśniono, jak połączyć kontroli dostępu z kolekcji podzielone na partycje, dzięki czemu użytkownik może uzyskiwać dostęp tylko do swoich własnych dokumentów w aplikacji platformy Xamarin.Forms._

## <a name="overview"></a>Omówienie

Podczas tworzenia kolekcję partycjonowaną, należy określić klucz partycji i dokumenty z tym samym kluczem partycji będą przechowywane w tej samej partycji. W związku z tym określanie tożsamości użytkownika jako klucza partycji spowoduje kolekcję partycjonowaną, który będzie przechowywane tylko dokumenty dla tego użytkownika. Dzięki temu również bazą danych dokumentów bazy danych rozwiązania Cosmos Azure mogą być skalowane wraz liczbę użytkowników, a elementy zwiększyć.

Musi być przyznany dostęp do kolekcji i model kontroli dostępu do interfejsu API usługi DocumentDB definiują dwa typy konstrukcji dostępu:

- **Klucze główne** włączyć pełny dostęp administracyjny do wszystkich zasobów w ramach konta bazy danych rozwiązania Cosmos i są tworzone po utworzeniu konta DB rozwiązania Cosmos.
- **Tokeny zasobów** przechwytywania relacji między użytkownikiem bazy danych i uprawnień, użytkownik ma dla określonego zasobu rozwiązania Cosmos bazy danych, takich jak kolekcji lub dokumentu.

Udostępnianie klucz główny otwartego rozwiązania Cosmos DB możliwości złośliwego lub brak dbałości o ich użycia. Jednak tokenów zasobów DB rozwiązania Cosmos mechanizmu bezpiecznego zezwolenie komputerom klienckim do odczytu, zapisu i usuwania określonych zasobów w ramach konta bazy danych rozwiązania Cosmos zgodnie z przyznawanych uprawnień.

Typowe metody z żądaniem, generowanie i dostarczanie tokenów zasobów dla aplikacji mobilnej jest użycie zasobu brokera tokenu. Na poniższym diagramie przedstawiono ogólne omówienie sposobu Przykładowa aplikacja korzysta z zasobu brokera tokenu do zarządzania dostępem do danych w bazie danych dokumentów:

![](authentication-images/documentdb-authentication.png "Proces uwierzytelniania bazy danych dokumentu")

Broker token zasobu jest usługi interfejsu API sieci Web warstwy środkowej, hostowana w usłudze Azure App Service, która posiada klucza głównego konta bazy danych rozwiązania Cosmos. Przykładowa aplikacja używa brokera tokenu zasobów do zarządzania dostępem do danych w bazie danych dokumentów w następujący sposób:

1. Podczas logowania aplikacji platformy Xamarin.Forms kontaktuje się z usługi aplikacji Azure do zainicjowania przepływ uwierzytelniania.
1. Usługa aplikacji Azure wykonuje przepływ uwierzytelniania OAuth z serwisem Facebook. Po zakończeniu przebieg uwierzytelniania w aplikacji platformy Xamarin.Forms odbiera token dostępu.
1. Aplikacja platformy Xamarin.Forms używa tokenu dostępu dla żądania tokenu zasobów z zasobu brokera tokenu.
1. Broker tokenu zasobów używa tokenu dostępu, aby zażądać tożsamość użytkownika z usługi Facebook. Tożsamość użytkownika jest następnie używany do żądania tokenu zasobu z bazy danych rozwiązania Cosmos, używany do przyznania dostępu odczytu i zapisu do kolekcji partycjonowanych uwierzytelnionego użytkownika.
1. Aplikacja platformy Xamarin.Forms używa tokenu zasobów bezpośredni dostęp do zasobów DB rozwiązania Cosmos uprawnienia określone przez token zasobu.

> [!NOTE]
> Po wygaśnięciu tokenu zasobów dokumentu kolejnych żądań bazy danych będą otrzymywać 401 nieautoryzowane wyjątek. W tym momencie aplikacji platformy Xamarin.Forms powinien przywrócenia tożsamości i uzyskać nowy token zasobu.

Aby uzyskać więcej informacji na temat partycjonowania rozwiązania Cosmos bazy danych, zobacz [partycji i skali w usłudze Azure DB rozwiązania Cosmos](/azure/cosmos-db/partition-data/). Aby uzyskać więcej informacji na temat rozwiązania Cosmos DB kontroli dostępu, zobacz [zabezpieczanie dostępu do danych DB rozwiązania Cosmos](/azure/cosmos-db/secure-access-to-data/) i [kontrola dostępu w usługach interfejsu API usługi DocumentDB](/rest/api/documentdb/access-control-on-documentdb-resources/).

## <a name="setup"></a>Konfiguracja

Proces zintegrować zasobu brokera tokenu aplikacji platformy Xamarin.Forms wygląda następująco:

1. Utwórz konto rozwiązania Cosmos bazy danych, które będzie używane kontroli dostępu. Aby uzyskać więcej informacji, zobacz [rozwiązania Cosmos bazy danych konfiguracji](#cosmosdb_configuration).
1. Utwórz usługę aplikacji Azure do hostowania brokera token zasobu. Aby uzyskać więcej informacji, zobacz [konfiguracji usługi aplikacji Azure](#app_service_configuration).
1. Tworzenie aplikacji usługi Facebook, do uwierzytelniania. Aby uzyskać więcej informacji, zobacz [konfiguracji aplikacji usługi Facebook](#facebook_configuration).
1. Konfigurowanie usługi aplikacji Azure do łatwego uwierzytelniania z serwisem Facebook. Aby uzyskać więcej informacji, zobacz [konfiguracji uwierzytelniania usługi aplikacji Azure](#app_service_authentication_configuration).
1. Konfigurowanie przykładowej aplikacji platformy Xamarin.Forms do komunikowania się z usługi aplikacji Azure i DB rozwiązania Cosmos. Aby uzyskać więcej informacji, zobacz [konfiguracji aplikacji platformy Xamarin.Forms](#forms_application_configuration).

<a name="cosmosdb_configuration" />

### <a name="cosmos-db-configuration"></a>Konfiguracja rozwiązania cosmos bazy danych

Proces tworzenia konto rozwiązania Cosmos bazy danych, które będzie używane kontroli dostępu jest następujący:

1. Utwórz konto DB rozwiązania Cosmos. Aby uzyskać więcej informacji, zobacz [Tworzenie konta bazy danych rozwiązania Cosmos](/azure/cosmos-db/documentdb-dotnetcore-get-started#step-1-create-a-documentdb-account).
1. W ramach konta rozwiązania Cosmos bazy danych, należy utworzyć nową kolekcję o nazwie `UserItems`, określając klucz partycji `/userid`.

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Konfiguracja usługi aplikacji Azure

Proces hostingu broker token zasobu w usłudze Azure App Service jest następujący:

1. W portalu Azure Utwórz nową aplikację sieci web usługi aplikacji. Aby uzyskać więcej informacji, zobacz [tworzenie aplikacji sieci web w środowisku usługi aplikacji](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/).
1. W portalu Azure Otwórz blok ustawień aplikacji dla aplikacji sieci web, a następnie dodaj następujące ustawienia:
    - `accountUrl` — wartość powinna być adres URL rozwiązania Cosmos DB konta z bloku klucze konto bazy danych rozwiązania Cosmos.
    - `accountKey` — wartość powinna być DB rozwiązania Cosmos klucz główny programu (podstawowe lub pomocnicze) za pomocą bloku klucze konta DB rozwiązania Cosmos.
    - `databaseId` — wartość powinna być nazwą bazy danych DB rozwiązania Cosmos.
    - `collectionId` — wartość powinna być nazwą kolekcji rozwiązania Cosmos bazy danych (w tym przypadku `UserItems`).
    - `hostUrl` — wartość powinna być adres URL aplikacji sieci web za pomocą bloku Przegląd konta usługi aplikacji.

    Poniższy zrzut ekranu przedstawia tej konfiguracji:

    [![](authentication-images/azure-web-app-settings.png "Ustawienia aplikacji sieci Web usługi aplikacji")](authentication-images/azure-web-app-settings-large.png "ustawień aplikacji sieci Web usługi aplikacji")

1. Publikuj rozwiązanie tokenu brokera zasobów w aplikacji sieci web w usłudze Azure App Service.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Konfiguracja aplikacji usługi Facebook

Proces tworzenia aplikacji usługi Facebook, do uwierzytelniania jest następujący:

1. Tworzenie aplikacji usługi Facebook. Aby uzyskać więcej informacji, zobacz [rejestracji i konfiguracji aplikacji](https://developers.facebook.com/docs/apps/register) w Centrum deweloperów usługi Facebook.
1. Dodaj produkt Facebook logowania do aplikacji. Aby uzyskać więcej informacji, zobacz [dodać Facebook logowania do aplikacji lub witryny sieci Web](https://developers.facebook.com/docs/facebook-login) w Centrum deweloperów usługi Facebook.
1. Skonfiguruj Facebook logowania w następujący sposób:
  - Włącz klienta OAuth logowania.
  - Włączanie logowania OAuth sieci Web.
  - Ustaw OAuth prawidłowy identyfikator URI przekierowania w na identyfikator URI aplikacji sieci web usługi aplikacji, z `/.auth/login/facebook/callback` dołączane.

  Poniższy zrzut ekranu przedstawia tej konfiguracji:

  ![](authentication-images/facebook-oauth-settings.png "Ustawienia uwierzytelniania OAuth logowania usługi Facebook")

Aby uzyskać więcej informacji, zobacz [zarejestrować aplikację w usłudze Facebook](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook).

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Konfiguracja uwierzytelniania usługi aplikacji Azure

Proces konfigurowania aplikacji usługi jest łatwe uwierzytelnianie w następujący sposób:

1. W portalu Azure przejdź do aplikacji sieci web usługi aplikacji.
1. W portalu Azure Otwórz uwierzytelnianie / autoryzacja bloku i wykonaj następującą konfigurację:
  - Uwierzytelnianie usługi aplikacji powinna być włączona.
  - Działanie podejmowane w przypadku nieuwierzytelnionego żądania powinien mieć ustawioną **logowania się za pomocą usługi Facebook**.

  Poniższy zrzut ekranu przedstawia tej konfiguracji:

  [![](authentication-images/app-service-authentication-settings.png "Ustawienia uwierzytelniania aplikacji sieci Web usługi aplikacji")](authentication-images/app-service-authentication-settings-large.png "ustawienia uwierzytelniania aplikacji sieci Web usługi aplikacji")

Aplikacja sieci web usługi aplikacji powinien również być skonfigurowany do komunikowania się z aplikacjami usługi Facebook, aby umożliwić przepływ uwierzytelniania. Można to zrobić przez wybranie dostawcy tożsamości usługi Facebook i wprowadzając **identyfikator aplikacji** i **klucz tajny aplikacji** wartości z ustawień aplikacji Facebook w Centrum deweloperów usługi Facebook. Aby uzyskać więcej informacji, zobacz [informacje dodać Facebook dla aplikacji](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application).

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Konfiguracja aplikacji platformy Xamarin.Forms

Proces konfigurowania przykładowej aplikacji platformy Xamarin.Forms wygląda następująco:

1. Otwórz rozwiązanie platformy Xamarin.Forms.
1. Otwórz `Constants.cs` i zaktualizuj wartości następujące ograniczenia:
  - `EndpointUri` — wartość powinna być adres URL rozwiązania Cosmos DB konta z bloku klucze konto bazy danych rozwiązania Cosmos.
  - `DatabaseName` — wartość powinna być nazwą bazy danych dokumentu.
  - `CollectionName` — wartość powinna być nazwą kolekcji dokumentów bazy danych (w tym przypadku `UserItems`).
  - `ResourceTokenBrokerUrl` — wartość powinna być adres URL aplikacji sieci web token brokera zasobów z bloku Przegląd konta usługi aplikacji.

## <a name="initiating-login"></a>Inicjowanie logowania

Przykładowa aplikacja inicjuje proces logowania przy użyciu Xamarin.Auth do przekierowania przeglądarki do adresu URL dostawcy tożsamości, jak pokazano w poniższym przykładzie kodzie:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

Powoduje to, że przepływ uwierzytelniania OAuth inicjowanie między usługą aplikacji Azure i usługi Facebook, która zostanie wyświetlona strona logowania usługi Facebook:

![](authentication-images/login.png "Facebook Login")

Nazwy logowania można anulować, naciskając klawisz **anulować** przycisk w systemie iOS lub naciskając klawisz **ponownie** przycisk w systemie Android, to w takim przypadku użytkownik pozostanie nieuwierzytelnione i interfejs użytkownika dostawcy tożsamości usunąć na ekranie.

Aby uzyskać więcej informacji o Xamarin.Auth, zobacz [uwierzytelnianie użytkowników przy użyciu dostawcy tożsamości](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="obtaining-a-resource-token"></a>Uzyskiwanie tokenu zasobów

Po pomyślnym uwierzytelnieniu `WebRedirectAuthenticator.Completed` generowane zdarzenie. Poniższy przykład kodu pokazuje, obsługa tego zdarzenia:

```csharp
auth.Completed += async (sender, e) =>
{
  if (e.IsAuthenticated && e.Account.Properties.ContainsKey("token"))
  {
    var easyAuthResponseJson = JsonConvert.DeserializeObject<JObject>(e.Account.Properties["token"]);
    var easyAuthToken = easyAuthResponseJson.GetValue("authenticationToken").ToString();

    // Call the ResourceBroker to get the resource token
    using (var httpClient = new HttpClient())
    {
      httpClient.DefaultRequestHeaders.Add("x-zumo-auth", easyAuthToken);
      var response = await httpClient.GetAsync(Constants.ResourceTokenBrokerUrl + "/api/resourcetoken/");
      var jsonString = await response.Content.ReadAsStringAsync();
      var tokenJson = JsonConvert.DeserializeObject<JObject>(jsonString);
      resourceToken = tokenJson.GetValue("token").ToString();
      UserId = tokenJson.GetValue("userid").ToString();

      if (!string.IsNullOrWhiteSpace(resourceToken))
      {
        client = new DocumentClient(new Uri(Constants.EndpointUri), resourceToken);
        ...
      }
      ...
    }
  }
};
```

Wynikiem pomyślnym uwierzytelnieniu jest token dostępu, który jest dostępny `AuthenticatorCompletedEventArgs.Account` właściwości. Token dostępu jest wyodrębniany i używany w żądania GET do zasobu brokera tokenu `resourcetoken` interfejsu API.

`resourcetoken` Interfejsu API używa tokenu dostępu, aby zażądać tożsamość użytkownika z usługi Facebook, który z kolei jest używany do żądania tokenu zasobu z bazy danych rozwiązania Cosmos. Jeśli istnieje już dokument prawidłowego uprawnienia dla użytkownika w bazie danych dokumentów, zostanie pobrana i zawierające token zasobu dokumentu JSON jest zwracana do aplikacji platformy Xamarin.Forms. Jeśli dokument prawidłowego uprawnienia nie istnieje dla użytkownika, użytkowników i uprawnienia jest tworzony w bazie danych dokumentów, a token zasobu jest wyodrębnione z dokumentu uprawnień i powrót do aplikacji platformy Xamarin.Forms w dokumencie JSON.

> [!NOTE]
> Użytkownik bazy danych dokumentu jest zasobem skojarzonego z bazą danych dokumentów, a każda baza danych może zawierać zero lub więcej użytkowników. Uprawnienia bazy danych dokumentu jest zasobem skojarzonych z użytkownikiem bazy danych dokumentów, a każdy użytkownik może zawierać zero lub więcej uprawnień. Zasób uprawnienia zapewnia dostęp do tokenów zabezpieczających wymaga użytkownika podczas próby uzyskania dostępu do zasobu, takie jak dokumentu.

Jeśli `resourcetoken` pomyślnie ukończy interfejsu API, wysyła kod stanu HTTP 200 (OK) w odpowiedzi, wraz z dokumentu JSON zawierający token zasobu. Następujące dane JSON pokazuje komunikat typowe pomyślnej odpowiedzi:

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

`WebRedirectAuthenticator.Completed` Obsługi zdarzeń odczytuje odpowiedzi z `resourcetoken` interfejsu API i wyodrębnia token zasobu oraz identyfikator użytkownika. Token zasobu są następnie przekazywane jako argument `DocumentClient` konstruktora, który hermetyzuje punktu końcowego, poświadczeń i umożliwiają dostęp do bazy danych rozwiązania Cosmos zasady połączeń i służy do konfigurowania i wykonywać żądania względem DB rozwiązania Cosmos. Token zasobu są wysyłane z każdym żądaniem bezpośredni dostęp do zasobu, a także wskazuje, czy udzielono dostępu do odczytu i zapisu do kolekcji partycjonowanych uwierzytelnionych użytkowników.

## <a name="retrieving-documents"></a>Podczas pobierania dokumentów

Podczas pobierania dokumentów tylko należących do uwierzytelnionego użytkownika można osiągnąć, tworząc kwerendy dokumentu, która zawiera identyfikator użytkownika jako klucza partycji i przedstawiono w poniższym przykładzie kodu:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink,
                        new FeedOptions
                        {
                          MaxItemCount = -1,
                          PartitionKey = new PartitionKey(UserId)
                        })
          .Where(item => !item.Id.Contains("permission"))
          .AsDocumentQuery();
while (query.HasMoreResults)
{
  Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
}
```

Zapytanie asynchronicznie pobiera wszystkie dokumenty należące do uwierzytelnionego użytkownika z określonej kolekcji i umieszcza je w `List<TodoItem>` kolekcji do wyświetlenia.

`CreateDocumentQuery<T>` Określa metodę `Uri` argument, który reprezentuje kolekcję, do której należy wysłać zapytanie dla dokumentów, a `FeedOptions` obiektu. `FeedOptions` Obiektu określa, że nieograniczoną liczbę elementów może być zwracany przez zapytanie i identyfikatorem użytkownika jako klucza partycji. Dzięki temu, że wyłącznie do dokumentów w kolekcji partycjonowanych użytkownika są zwracane w wynikach.

> [!NOTE]
> Należy pamiętać, że dokumenty uprawnienia, które są tworzone przez brokera token zasobu, są przechowywane w tej samej kolekcji dokumentu jako dokumenty utworzone przez aplikację platformy Xamarin.Forms. W związku z tym zapytanie dokumentu zawiera `Where` klauzuli, którego dotyczy kwerenda dotycząca kolekcji dokumentów predykat filtrowania. Klauzulę gwarantuje, że dokumenty uprawnienia nie są zwracane z kolekcji dokumentów.

Aby uzyskać więcej informacji na temat pobierania dokumentów z kolekcji dokumentów, zobacz [pobierania dokumentu kolekcji dokumentów](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#document_query).

## <a name="inserting-documents"></a>Wstawianie dokumentów

Przed wstawieniem dokumentu do kolekcji dokumentów, `TodoItem.UserId` właściwości powinien być zaktualizowany o wartości on używany jako klucza partycji, jak pokazano w poniższym przykładzie:

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

Daje to pewność, że dokument zostanie wstawiony do kolekcji partycjonowanych użytkownika.

Aby uzyskać więcej informacji na temat Wstawianie dokumentu do kolekcji dokumentów, zobacz [Wstawianie dokumentu do kolekcji dokumentów](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#inserting_document).

## <a name="deleting-documents"></a>Usuwanie dokumentów

Musi być określona wartość klucza partycji, gdy usunięcie dokumentu z kolekcji podzielone na partycje, jak przedstawiono w poniższym przykładzie kodu:

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

Dzięki temu DB rozwiązania Cosmos wie, które podzielona na partycje kolekcji można usunąć z dokumentu.

Aby uzyskać więcej informacji na temat usuwania dokumentu z kolekcji dokumentów, zobacz [usunięcie dokumentu z kolekcji dokumentów](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#deleting_document).

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono sposób łączenia kontroli dostępu z kolekcji podzielone na partycje, dzięki czemu użytkownik może uzyskiwać dostęp tylko do swoich własnych dokumentów bazy danych dokumentów w aplikacji platformy Xamarin.Forms. Określanie tożsamości użytkownika jako klucza partycji gwarantuje, że kolekcję partycjonowaną można przechowywać tylko dokumenty dla tego użytkownika.


## <a name="related-links"></a>Linki pokrewne

- [TodoDocumentDBAuth (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDBAuth/)
- [Korzystanie z bazy danych dokumentów usługi Azure Cosmos DB](~/xamarin-forms/data-cloud/cosmosdb/consuming.md)
- [Zabezpieczanie dostępu do danych bazy danych Azure rozwiązania Cosmos](/azure/cosmos-db/secure-access-to-data/)
- [Kontrola dostępu w usługach interfejsu API usługi DocumentDB](/rest/api/documentdb/access-control-on-documentdb-resources/).
- [Jak partycji i skali w usłudze Azure DB rozwiązania Cosmos](/azure/cosmos-db/partition-data/)
- [Biblioteka klienta DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Interfejs API Azure Cosmos DB](https://msdn.microsoft.com/library/azure/dn948556.aspx)

