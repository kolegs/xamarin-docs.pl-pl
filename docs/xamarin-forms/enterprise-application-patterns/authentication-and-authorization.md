---
title: Uwierzytelnianie i autoryzacja
description: W tym rozdziale wyjaśniono, jak aplikacja mobilna w ramach aplikacji eShopOnContainers wykonuje uwierzytelnianie i autoryzacja przed konteneryzowane mikrousługi.
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: beb9e8f351a1cecc6017a08345f7cfc5e207ba35
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996220"
---
# <a name="authentication-and-authorization"></a>Uwierzytelnianie i autoryzacja

Uwierzytelnianie jest proces uzyskiwania poświadczeń tożsamości, takie jak nazwa i hasło z danych użytkownika i sprawdzanie poprawności tych poświadczeń względem urzędu. Jeśli poświadczenia są prawidłowe, jednostka, która przesyłane poświadczenia jest uważany za uwierzytelnionej tożsamości. Po uwierzytelnieniu tożsamość procesu autoryzacji określa, czy tej tożsamości ma dostęp do danego zasobu.

Dostępnych jest wiele metod do integracji aplikacji platformy Xamarin.Forms, który komunikuje się z aplikacją sieci web platformy ASP.NET MVC za pomocą tożsamości platformy ASP.NET Core, dostawców uwierzytelniania zewnętrznych, takich jak Microsoft, Google, w tym uwierzytelnianie i autoryzacja Facebook, lub Twitter, a także uwierzytelniania oprogramowania pośredniczącego. Aplikacja mobilna w ramach aplikacji eShopOnContainers przeprowadza uwierzytelnianie i autoryzacja przy użyciu mikrousług konteneryzowanych tożsamości, który używa IdentityServer 4. Aplikacja mobilna żąda tokeny zabezpieczające od IdentityServer, do uwierzytelniania użytkownika lub do uzyskiwania dostępu do zasobu. Dla IdentityServer tokenami problemu w imieniu użytkownika użytkownik musi Zaloguj się do IdentityServer. Jednak IdentityServer nie zapewnia interfejsu użytkownika lub bazy danych dla uwierzytelniania. W związku z tym w ramach aplikacji eShopOnContainers aplikacji odwołanie do tożsamości platformy ASP.NET Core jest używany w tym celu.

## <a name="authentication"></a>Uwierzytelnianie

Uwierzytelnianie jest wymagane, gdy aplikacja musi znać tożsamość bieżącego użytkownika. ASP.NET Core podstawowy mechanizm do identyfikowania użytkowników jest system członkostwa tożsamości platformy ASP.NET Core, która przechowuje informacje o użytkowniku w magazynie danych skonfigurowane przez dewelopera. Zazwyczaj ten magazyn danych będzie z magazynu platformy EntityFramework, chociaż pakietów innych firm lub niestandardowych magazynów można użyć do przechowywania informacji o tożsamości w usłudze Azure storage, Azure Cosmos DB lub w innych lokalizacjach.

Scenariusze uwierzytelniania, które za pomocą użytkownika lokalnego magazynu danych, a które utrwalanie informacji o tożsamości między żądaniami, za pomocą plików cookie (co jest typowe w aplikacjach sieci web platformy ASP.NET MVC) tożsamości platformy ASP.NET Core jest odpowiednie rozwiązanie. Pliki cookie nie są jednak zawsze naturalnych środków, przechowywanie i przesyłanie danych. Na przykład aplikację sieci web platformy ASP.NET Core, która udostępnia punktów końcowych RESTful, które są dostępne z aplikacji mobilnej zazwyczaj musisz użyć uwierzytelniania tokenu elementu nośnego, ponieważ pliki cookie nie mogą być używane w tym scenariuszu. Jednak tokenów elementu nośnego można łatwo je pobrać i zawarte w nagłówku autoryzacji żądania sieci web z aplikacji mobilnej.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Wystawianie tokenów elementu nośnego przy użyciu IdentityServer 4

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) to architektura typu open source OpenID Connect i OAuth 2.0 dla platformy ASP.NET Core, który może służyć do wielu scenariuszy uwierzytelniania i autoryzacji, łącznie z wystawianie tokenów zabezpieczających dla użytkowników lokalnych tożsamości platformy ASP.NET Core.

> [!NOTE]
> OpenID Connect i OAuth 2.0 są bardzo podobne, a jednocześnie ma różne obowiązki.

OpenID Connect jest warstwy uwierzytelniania, na podstawie protokołu OAuth 2.0. Protokołu OAuth 2 to protokół, który umożliwia aplikacjom żądania tokenów dostępu z usługi tokenu zabezpieczającego używać ich do komunikowania się z interfejsami API. To delegowanie zmniejsza złożoność interfejsów API i aplikacje klienckie, ponieważ były scentralizowane uwierzytelnianie i autoryzację.

Kombinacja protokołu OpenID Connect i OAuth 2.0 połączyć dwa problemy dotyczące zabezpieczeń podstawowe, uwierzytelnianie i dostęp do interfejsu API, a IdentityServer 4 jest implementacją tych protokołów.

W aplikacji, które używają bezpośrednia komunikacja klienta z mikrousługą, takich jak aplikacji odwołanie w ramach aplikacji eShopOnContainers mikrousługi dedykowanych uwierzytelniania, działając jako usługa tokenu zabezpieczającego (STS) może służyć do uwierzytelniania użytkowników, jak pokazano na rysunku 9-1. Aby uzyskać więcej informacji na temat bezpośrednia komunikacja klienta z mikrousługą zobacz [komunikacji między klientem i Mikrousług](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Uwierzytelnianie w mikrousługach dedykowanych uwierzytelniania")

**Rysunek 9-1:** uwierzytelnienia za pomocą mikrousług dedykowanych uwierzytelniania

Aplikacja mobilna w ramach aplikacji eShopOnContainers komunikuje się z mikrousług tożsamości, który używa IdentityServer 4, aby przeprowadzać uwierzytelnianie i kontrola dostępu do interfejsów API. W związku z tym aplikacji mobilnej żądań tokenów z IdentityServer, do uwierzytelniania użytkownika lub do uzyskiwania dostępu do zasobu:

-   Uwierzytelnianie użytkowników za pomocą IdentityServer odbywa się przez żądanie aplikacji mobilnej *tożsamości* token, który reprezentuje wynik procesu uwierzytelniania. W ramach absolutnego minimum zawiera identyfikator użytkownika i dowiedzieć się, jak i kiedy użytkownik jest uwierzytelniony. Może również zawierać dane dodatkowe tożsamości.
-   Uzyskiwanie dostępu do zasobów przy użyciu IdentityServer odbywa się przez żądanie aplikacji mobilnej *dostępu* token, który zezwala na dostęp do zasobu interfejsu API. Klienci żądać tokenów dostępu i przekazują je do interfejsu API. Tokeny dostępu zawierają informacje o kliencie, a użytkownik (jeśli istnieje). Interfejsy API następnie użyć tych informacji do autoryzowania dostępu do swoich danych.

> [!NOTE]
> Klient musi być zarejestrowane przy użyciu IdentityServer on żądania tokenów.

### <a name="adding-identityserver-to-a-web-application"></a>Dodawanie IdentityServer do aplikacji sieci Web

Aby dla aplikacji sieci web platformy ASP.NET Core na używanie IdentityServer 4 należy dodać do rozwiązania Visual Studio w aplikacji sieci web. Aby uzyskać więcej informacji, zobacz [instalacji i przegląd](https://identityserver4.readthedocs.io/en/release/quickstarts/0_overview.html) w dokumentacji IdentityServer.

Po IdentityServer znajduje się w rozwiązaniu Visual Studio w aplikacji sieci web, należy można dodać do potoku przetwarzania żądania HTTP aplikacji sieci web tak, aby mógł obsługiwać żądania do punktów końcowych protokołu OpenID Connect i OAuth 2.0. Jest to osiągane w `Configure` metody w aplikacji sieci web `Startup` klasy, jak pokazano w poniższym przykładzie kodu:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Kolejność ma znaczenie w aplikacji sieci web żądania HTTP przetwarzania potoku. W związku z tym IdentityServer należy dodać do potoku przed struktury interfejsu użytkownika, który implementuje ekran logowania.

### <a name="configuring-identityserver"></a>Konfigurowanie IdentityServer

IdentityServer powinna być skonfigurowana w `ConfigureServices` metody w aplikacji sieci web `Startup` klasy przez wywołanie metody `services.AddIdentityServer` metody, jak pokazano w poniższym przykładzie kodu z poziomu aplikacji odwołania w ramach aplikacji eShopOnContainers:

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

Po wywołaniu `services.AddIdentityServer` metody dodatkowe interfejsy API fluent są wywoływane w celu skonfiguruj następujące opcje:

-   Poświadczenia używane do podpisywania.
-   Zasoby interfejsu API i tożsamości, które użytkownicy mogą żądać dostępu do danych.
-   Klienci, którzy będą łączyć do żądania tokenów.
-   ASP.NET Core Identity.

>💡 **Porada**: dynamicznie załadować konfiguracji IdentityServer 4. IdentityServer 4 interfejsy API umożliwiają konfigurowanie IdentityServer z listy w pamięci obiektów konfiguracji. W ramach aplikacji eShopOnContainers aplikacji odwołanie do tych kolekcji w pamięci są zakodowane na aplikację. Jednak w scenariuszach produkcyjnych można załadować dynamicznie pliku konfiguracji lub z bazą danych.

Aby uzyskać informacje o konfigurowaniu IdentityServer używania tożsamości platformy ASP.NET Core, zobacz [za pomocą tożsamości platformy ASP.NET Core](https://identityserver4.readthedocs.io/en/release/quickstarts/6_aspnet_identity.html) w dokumentacji IdentityServer.

#### <a name="configuring-api-resources"></a>Konfigurowanie zasobów interfejsu API

Podczas konfigurowania zasobów interfejsu API `AddInMemoryApiResources` oczekuje, że metoda `IEnumerable<ApiResource>` kolekcji. Poniższy kod przedstawia przykład `GetApis` metodę, która zapewnia tej kolekcji, w ramach aplikacji eShopOnContainers odwoływać się do aplikacji:

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

Ta metoda określa, że IdentityServer powinna zapewniać ochronę zamówień i koszyka interfejsów API. W związku z tym, IdentityServer zarządzany dostęp tokenów będzie wymagane podczas wykonywania wywołań do tych interfejsów API. Aby uzyskać więcej informacji na temat `ApiResource` typu, zobacz [zasobu interfejsu API](https://identityserver4.readthedocs.io/en/release/reference/api_resource.html#refapiresource) w dokumentacji IdentityServer 4.

#### <a name="configuring-identity-resources"></a>Konfigurowanie tożsamości zasobów

Podczas konfigurowania zasobów tożsamości `AddInMemoryIdentityResources` oczekuje, że metoda `IEnumerable<IdentityResource>` kolekcji. Zasoby tożsamości są dane, takie jak identyfikator użytkownika, nazwa lub adres e-mail. Każdy zasób tożsamość ma unikatową nazwę, a typy oświadczeń dowolnego można przypisać do niego, które następnie zostanie uwzględnione w tokenie tożsamości użytkownika. Poniższy kod przedstawia przykład `GetResources` metodę, która zapewnia tej kolekcji, w ramach aplikacji eShopOnContainers odwoływać się do aplikacji:

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

Specyfikacja protokołu OpenID Connect określa niektóre [tożsamości standardowe zasoby](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). Minimalnym wymaganiem jest, że pomoc techniczna jest dostępna do emitowania Unikatowy identyfikator dla użytkowników. Jest to osiągane przez udostępnianie `IdentityResources.OpenId` tożsamości zasobu.

> [!NOTE]
> `IdentityResources` Klasy obsługuje wszystkie zakresy zdefiniowane w specyfikacji protokołu OpenID Connect (openid, poczty e-mail, profil, telefonu i adres).

IdentityServer obsługuje również definiowanie zasobów tożsamości niestandardowej. Aby uzyskać więcej informacji, zobacz [Definiowanie zasobów tożsamość niestandardowa](https://identityserver4.readthedocs.io/en/release/topics/resources.html#defining-custom-identity-resources) w dokumentacji IdentityServer. Aby uzyskać więcej informacji na temat `IdentityResource` typu, zobacz [tożsamości zasobu](https://identityserver4.readthedocs.io/en/release/reference/identity_resource.html) w dokumentacji IdentityServer 4.

#### <a name="configuring-clients"></a>Konfigurowanie klientów

Klienci znajdują się aplikacje, które mogą żądać tokenów z IdentityServer. Zazwyczaj poniższe ustawienia muszą zostać zdefiniowane dla każdego klienta, co najmniej:

-   Identyfikator unikatowy klienta.
-   Dozwolone interakcje usługi tokenu (znanych jako typu przydziału).
-   Lokalizacja, w których wysyłane są tokeny tożsamości i dostępu do (tzn. identyfikator URI przekierowania).
-   Listę zasobów, które klient ma mieć dostęp do (tzn. zakresy).

Podczas konfigurowania klientów, `AddInMemoryClients` oczekuje, że metoda `IEnumerable<Client>` kolekcji. W poniższym przykładzie kodu pokazano konfigurację w ramach aplikacji eShopOnContainers aplikacji mobilnej w `GetClients` metodę, która zapewnia tej kolekcji, w ramach aplikacji eShopOnContainers odwoływać się do aplikacji:

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

Ta konfiguracja Określa dane do następujących właściwości:

-   `ClientId`: Unikatowy identyfikator klienta.
-   `ClientName`: Klient nazwę wyświetlaną, która jest używana do rejestrowania i ekranu wyrażania zgody.
-   `AllowedGrantTypes`: Określa, jak klient chce korzystać z IdentityServer. Aby uzyskać więcej informacji, zobacz [Konfigurowanie przepływu uwierzytelniania](#configuring_the_authentication_flow).
-   `ClientSecrets`: Określa poświadczenia wpisu tajnego klienta, które są używane podczas żądania tokenów z punktu końcowego tokenu.
-   `RedirectUris`: Określa dozwolone identyfikatory URI, do której ma zostać zwrócone kody autoryzacji lub tokenów.
-   `RequireConsent`: Określa, czy ekranie wyrażania zgody jest wymagana.
-   `RequirePkce`: Określa, czy klienci korzystający z kodu autoryzacji muszą wysyłać dowód klucza.
-   `PostLogoutRedirectUris`: Określa dozwolone identyfikatory URI przekierowywania do po wylogowania.
-   `AllowedCorsOrigins`: Określa pochodzenia klienta tak, aby IdentityServer umożliwia wywołań między źródłami ze źródła.
-   `AllowedScopes`: Określa, który klient ma dostęp do zasobów. Domyślnie klient nie ma dostępu do żadnych zasobów.
-   `AllowOfflineAccess`: Określa, czy klient może żądać tokenów odświeżania.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Konfigurowanie przepływu uwierzytelniania

Przepływ uwierzytelniania między klientem a IdentityServer mogą być konfigurowane przez określenie typów przydziałów w `Client.AllowedGrantTypes` właściwości. OpenID Connect i OAuth 2.0 specyfikacji zdefiniować liczbę przepływów uwierzytelniania, w tym:

-   Niejawne. Ten przepływ jest zoptymalizowany pod kątem aplikacji opartych na przeglądarce, a powinien być używany dla użytkownika tylko do uwierzytelniania lub uwierzytelnianie i dostęp do żądania tokenu. Wszystkie tokeny są przesyłane za pośrednictwem przeglądarki i w związku z tym zaawansowane funkcje, takie jak tokeny odświeżania nie są dozwolone.
-   Kod autoryzacji. Ten przepływ umożliwia pobieranie tokenów w kanale Wstecz, w przeciwieństwie do kanału frontonu przeglądarki, jednocześnie obsługując uwierzytelniania klienta.
-   Hybrydowe. Ten przepływ jest kombinacją niejawny i typy przydziałów kod autoryzacji. Token tożsamości są przesyłane za pośrednictwem kanału przeglądarki i zawiera odpowiedź protokołu podpisem wraz z innych artefaktów, takich jak kod autoryzacji. Po pomyślnej weryfikacji odpowiedzi kanału zwrotnego powinny służyć do pobierania dostępu i token odświeżania.

> [!TIP]
> Korzystać z tego przepływu uwierzytelniania hybrydowych. Przepływ uwierzytelniania hybrydowe zmniejsza liczbę ataków, które mają zastosowanie do kanału przeglądarki i jest zalecana przepływ dla natywnych aplikacji, które chcesz pobrać tokenów dostępu (i ewentualnie tokenów odświeżania).

Aby uzyskać więcej informacji na temat przepływów uwierzytelniania, zobacz [typów przydziałów](https://identityserver4.readthedocs.io/en/release/topics/grant_types.html) w dokumentacji IdentityServer 4.

### <a name="performing-authentication"></a>Uwierzytelniania

Dla IdentityServer tokenami problemu w imieniu użytkownika użytkownik musi Zaloguj się do IdentityServer. Jednak IdentityServer nie zapewnia interfejsu użytkownika lub bazy danych dla uwierzytelniania. W związku z tym w ramach aplikacji eShopOnContainers aplikacji odwołanie do tożsamości platformy ASP.NET Core jest używany w tym celu.

Aplikacja mobilna w ramach aplikacji eShopOnContainers uwierzytelnia się za pomocą IdentityServer z przepływem uwierzytelniania hybrydowe, przedstawionym w rysunek 9-2.

![](authentication-and-authorization-images/sign-in.png "Ogólne omówienie procesu logowania")

**Rysunek 9-2:** ogólny przegląd procesu logowania

Zaloguj się żądanie `<base endpoint>:5105/connect/authorize`. Po pomyślnym uwierzytelnieniu IdentityServer zwraca odpowiedź uwierzytelniania zawierający kod autoryzacji i token tożsamości. Kod autoryzacji są następnie wysyłane do `<base endpoint>:5105/connect/token`, która odpowiada za pomocą programu access, tożsamości i tokenów odświeżania.

Ramach aplikacji eShopOnContainers aplikacji mobilnej objawy — poza IdentityServer, wysyłając żądanie do `<base endpoint>:5105/connect/endsession`, za pomocą dodatkowych parametrów. Po wystąpieniu wylogowania IdentityServer odpowiada, wysyłając identyfikator URI przekierowania wylogowania post do aplikacji mobilnej. Rysunek 9-3 ilustruje ten proces.

![](authentication-and-authorization-images/sign-out.png "Ogólne omówienie proces logowania")

**Rysunek 9-3:** ogólny przegląd procesu wylogowania

W ramach aplikacji eShopOnContainers aplikacji mobilnej, komunikację z IdentityServer odbywa się przez `IdentityService` klasy, która implementuje `IIdentityService` interfejsu. Ten interfejs określa, czy należy podać klasy implementującej `CreateAuthorizationRequest`, `CreateLogoutRequest`, i `GetTokenAsync` metody.

#### <a name="signing-in"></a>Logowanie się

Kiedy użytkownik naciska **logowania** znajdujący się na `LoginView`, `SignInCommand` w `LoginViewModel` wykonywane klasy, które z kolei wykonuje `SignInAsync` metody. Poniższy przykład kodu pokazuje tę metodę:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Ta metoda wywołuje `CreateAuthorizationRequest` method in Class metoda `IdentityService` klasy, który jest pokazany w poniższym przykładzie kodu:

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

Ta metoda tworzy identyfikator URI dla firmy IdentityServer [punkt końcowy autoryzacji](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), z wymaganymi parametrami. Punkt końcowy autoryzacji wynosi `/connect/authorize` na porcie 5105 podstawowego punktu końcowego, widoczne jako ustawienia użytkownika. Aby uzyskać więcej informacji na temat ustawień użytkownika, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Podatność aplikacji mobilnej w ramach aplikacji eShopOnContainers została zmniejszona o wdrażanie klucza dowodu dla rozszerzenia kodu programu Exchange (PKCE) do uwierzytelniania OAuth. PKCE ochronę kod autoryzacji używany, jeśli zostanie przechwycona. Jest to osiągane przez klienta, generowanie klucza tajnego weryfikatora skrót jest przekazywany w żądaniu autoryzacji, i która jest prezentowana bez haszowania podczas realizacji przydziału kodu autoryzacji. Aby uzyskać więcej informacji na temat PKCE zobacz [dowód klucz wymiany kodu przez publiczny klientów uwierzytelniania OAuth](https://tools.ietf.org/html/rfc7636) w witrynie sieci web Internet Engineering Task Force.

Zwracane identyfikatora URI jest przechowywana w `LoginUrl` właściwość `LoginViewModel` klasy. Gdy `IsLogin` staje się właściwość `true`, [ `WebView` ](xref:Xamarin.Forms.WebView) w `LoginView` staje się widoczny. `WebView` Powiązań danych jego [ `Source` ](xref:Xamarin.Forms.WebView.Source) właściwości `LoginUrl` właściwość `LoginViewModel` klasy, a więc zgłasza żądanie logowania IdentityServer podczas `LoginUrl` właściwość jest ustawiona na Punkt końcowy autoryzacji IdentityServer firmy. Gdy IdentityServer odbiera żądanie i użytkownik nie jest uwierzytelniony, `WebView` nastąpi przekierowanie do strony logowania skonfigurowany, który jest pokazany na rysunku 9-4.

![](authentication-and-authorization-images/login.png "Strona logowania wyświetlany przez element WebView")

**Rysunek 9-4:** strony logowania wyświetlany przez element WebView

Po ukończeniu logowania [ `WebView` ](xref:Xamarin.Forms.WebView) nastąpi przekierowanie do zwrotu identyfikatora URI. To `WebView` nawigacji spowoduje, że `NavigateAsync` method in Class metoda `LoginViewModel` klasy do wykonania, który jest pokazany w poniższym przykładzie kodu:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

Metoda ta analizuje odpowiedź uwierzytelniania, która znajduje się w zwracane identyfikatora URI i pod warunkiem, że występuje kod autoryzacji prawidłowe kieruje żądanie do firmy IdentityServer [punktu końcowego tokenu](https://identityserver4.readthedocs.io/en/release/endpoints/token.html), przekazując kod autoryzacji PKCE weryfikatora wpisu tajnego i inne wymagane parametry. Punkt końcowy tokenu jest w `/connect/token` na porcie 5105 podstawowego punktu końcowego, widoczne jako ustawienia użytkownika. Aby uzyskać więcej informacji na temat ustawień użytkownika, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

>💡 **Porada**: Sprawdzanie poprawności zwracają identyfikatorów URI. Mimo że w ramach aplikacji eShopOnContainers aplikacji mobilnej nie weryfikuje zwracane identyfikatora URI, najlepszym rozwiązaniem jest zweryfikować, że zwracane identyfikatora URI odwołuje się do znanej lokalizacji, aby zapobiec atakom na otwarte przekierowywanie.

Jeśli punkt końcowy tokenu otrzyma kod autoryzacji prawidłowe i wpisu tajnego weryfikatora PKCE, odpowiada za pomocą tokenu dostępu, tożsamość token i token odświeżania. Token dostępu, (które zezwala na dostęp do zasobów interfejsu API) i tokenu tożsamości są następnie zapisywane jako ustawienia aplikacji, a Nawigacja strony jest wykonywane. W związku z tym, czy to ogólny efekt w aplikacji mobilnej w ramach aplikacji eShopOnContainers: pod warunkiem, że użytkownicy będą mogli pomyślnie uwierzytelnić za pomocą IdentityServer, są one przejście `MainView` strony, która jest [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) wyświetlającą `CatalogView` jako jego wybranej karty.

Aby uzyskać informacji o nawigacji na stronie, zobacz [nawigacji](~/xamarin-forms/enterprise-application-patterns/navigation.md). Aby uzyskać informacje dotyczące [ `WebView` ](xref:Xamarin.Forms.WebView) nawigacji sprawia, że metoda modelu widoku można wykonać, zobacz [wywoływania nawigacji za pomocą zachowań](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Aby uzyskać informacje o ustawieniach aplikacji, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Ramach aplikacji eShopOnContainers umożliwia także makiety logowania, gdy aplikacja jest skonfigurowana do korzystania z usług makiety w `SettingsView`. W tym trybie aplikacja nie komunikują się z IdentityServer, zamiast tego umożliwienie użytkownikowi logowanie się za pomocą żadnych poświadczeń.

#### <a name="signing-out"></a>Signing-out

Kiedy użytkownik naciska **WYLOGUJ** znajdujący się w `ProfileView`, `LogoutCommand` w `ProfileViewModel` wykonywane klasy, które z kolei wykonuje `LogoutAsync` metody. Ta metoda przeprowadza do nawigowania po stronach `LoginView` strony, przekazując `LogoutParameter` wystąpienia jest równa `true` jako parametr. Aby uzyskać więcej informacji na temat przekazywanie parametrów podczas nawigowania po stronach, zobacz [przekazywanie parametrów podczas nawigacji](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Jeśli widok jest tworzony i przejście, `InitializeAsync` jest wykonywana metoda model skojarzony widok widoku, która następnie wykonuje `Logout` metody `LoginViewModel` klasy, który jest pokazany w poniższym przykładzie kodu:

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

Ta metoda wywołuje `CreateLogoutRequest` method in Class metoda `IdentityService` klasy, przekazując token tożsamości pobierana z ustawienia aplikacji jako parametr. Aby uzyskać więcej informacji na temat ustawień aplikacji, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Poniższy kod przedstawia przykład `CreateLogoutRequest` metody:

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

Ta metoda tworzy identyfikator URI do firmy IdentityServer [zakończenia sesji punktu końcowego](https://identityserver4.readthedocs.io/en/release/endpoints/endsession.html#refendsession), z wymaganymi parametrami. Punkt końcowy zakończenia sesji wynosi `/connect/endsession` na porcie 5105 podstawowego punktu końcowego, widoczne jako ustawienia użytkownika. Aby uzyskać więcej informacji na temat ustawień użytkownika, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Zwracane identyfikatora URI jest przechowywana w `LoginUrl` właściwość `LoginViewModel` klasy. Gdy `IsLogin` właściwość `true`, [ `WebView` ](xref:Xamarin.Forms.WebView) w `LoginView` jest widoczna. `WebView` Powiązań danych jego [ `Source` ](xref:Xamarin.Forms.WebView.Source) właściwości `LoginUrl` właściwość `LoginViewModel` klasy, a więc zgłasza żądanie wylogowania IdentityServer podczas `LoginUrl` właściwość jest ustawiona na Firmy IdentityServer zakończenia sesji punktu końcowego. Po IdentityServer odbiera tym żądaniem, pod warunkiem, że użytkownik jest zalogowany, wylogowania odbywa się. Uwierzytelnianie jest śledzone za pomocą plików cookie zarządzanych przez oprogramowanie pośredniczące uwierzytelniania plików cookie z platformą ASP.NET Core. Dlatego wylogowanie się IdentityServer usuwa plik cookie uwierzytelniania i wysyła przekierowania po wylogowaniu identyfikatora URI z powrotem do klienta.

W aplikacji mobilnej [ `WebView` ](xref:Xamarin.Forms.WebView) nastąpi przekierowanie do przekierowania po wylogowaniu identyfikatora URI. To `WebView` nawigacji spowoduje, że `NavigateAsync` method in Class metoda `LoginViewModel` klasy do wykonania, który jest pokazany w poniższym przykładzie kodu:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

Ta metoda usuwa token tożsamości i token dostępu z ustawień aplikacji i ustawia `IsLogin` właściwości `false`, co powoduje, że [ `WebView` ](xref:Xamarin.Forms.WebView) na `LoginView` strony, aby stają się niewidoczne . Na koniec `LoginUrl` właściwość jest ustawiona na identyfikator URI IdentityServer [punkt końcowy autoryzacji](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), z wymaganymi parametrami w ramach przygotowania do następnym razem użytkownik zainicjuje logowania.

Aby uzyskać informacji o nawigacji na stronie, zobacz [nawigacji](~/xamarin-forms/enterprise-application-patterns/navigation.md). Aby uzyskać informacje dotyczące [ `WebView` ](xref:Xamarin.Forms.WebView) nawigacji sprawia, że metoda modelu widoku można wykonać, zobacz [wywoływania nawigacji za pomocą zachowań](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Aby uzyskać informacje o ustawieniach aplikacji, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Ramach aplikacji eShopOnContainers umożliwia także pozorny wylogowania, gdy aplikacja jest skonfigurowana do korzystania z usług makiety w SettingsView. W tym trybie aplikacja nie komunikują się za pomocą IdentityServer i zamiast tego usuwa wszystkie tokeny przechowywanych w ustawieniach aplikacji.

<a name="authorization" />

## <a name="authorization"></a>Autoryzacja

Po uwierzytelnieniu w sieci web platformy ASP.NET Core API często zachodzi potrzeba autoryzowania dostępu, co umożliwia zapewnienie interfejsów API usługi niektóre uwierzytelnionych użytkowników, ale nie dla wszystkich.

Ograniczanie dostępu do trasy ASP.NET Core MVC, można osiągnąć, stosując atrybut autoryzacji do kontrolera lub akcji, która ogranicza dostęp do kontrolera lub akcji do uwierzytelnionych użytkowników, jak pokazano w poniższym przykładzie kodu:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Jeśli nieautoryzowany użytkownik próbuje uzyskać dostęp do kontrolera lub akcji, która jest oznaczona za pomocą `Authorize` atrybutu, struktura MVC zwróci (nieautoryzowanych) kod stanu HTTP 401.

> [!NOTE]
> Parametry można określić na `Authorize` atrybutu, aby ograniczyć interfejsu API do określonych użytkowników. Aby uzyskać więcej informacji, zobacz [autoryzacji](/aspnet/core/security/authorization/introduction/).

IdentityServer mogą być zintegrowane przepływu pracy autoryzacji, tak aby tokeny dostępu zawiera kontrolki autoryzacji. To podejście jest wyświetlany w rysunek 9 – 5.

![](authentication-and-authorization-images/authorization.png "Autoryzacja za tokenu dostępu")

**Rysunek 9-5:** autoryzacji przez token dostępu

Aplikacji mobilnej w ramach aplikacji eShopOnContainers komunikuje się z mikrousług tożsamości i żąda tokenu dostępu w ramach procesu uwierzytelniania. Token dostępu jest następnie przekazywany do interfejsów API ujawnianych przez porządkowanie i koszyka mikrousług jako część żądania dostępu. Tokeny dostępu zawierają informacje o kliencie i użytkownika. Interfejsy API następnie użyć tych informacji do autoryzowania dostępu do swoich danych. Aby uzyskać informacje o sposobie konfigurowania IdentityServer ochrona interfejsów API, zobacz [Konfigurowanie zasobów interfejsu API](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Konfigurowanie IdentityServer przeprowadzić autoryzację

Aby przeprowadzić autoryzację przy użyciu IdentityServer, jego oprogramowanie pośredniczące autoryzacji należy dodać do Potok żądań HTTP w aplikacji sieci web. Oprogramowanie pośredniczące zostanie dodany do `ConfigureAuth` metody w aplikacji sieci web `Startup` klasy, która jest wywoływana z `Configure` metody i została przedstawiona w poniższym przykładzie kodu z poziomu aplikacji odwołania w ramach aplikacji eShopOnContainers:

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

Ta metoda zapewnia, że interfejs API może zostać oceniony jedynie z prawidłowym tokenem dostępu. Oprogramowanie pośredniczące sprawdza poprawność przychodzącego tokenu, aby upewnić się, że jest wysyłany z zaufanych wystawców i weryfikuje, czy token jest ważny do użycia z interfejsem API, który odbiera on. W związku z tym przechodząc do kontrolera zamawiania lub koszyka zwróci (nieautoryzowanych) kod stanu HTTP 401, wskazujący, że token dostępu jest wymagany.

> [!NOTE]
> Oprogramowanie pośredniczące autoryzacji IdentityServer firmy muszą zostać dodane do Potok żądań HTTP w aplikacji sieci web przed dodaniem MVC za pomocą `app.UseMvc()` lub `app.UseMvcWithDefaultRoute()`.

### <a name="making-access-requests-to-apis"></a>Tworzenie żądania dostępu do interfejsów API

W przypadku wysyłania żądań do mikrousług porządkowanie i koszyka, dostęp tokenu, uzyskany z IdentityServer podczas procesu uwierzytelniania muszą być zawarte w żądaniu, jak pokazano w poniższym przykładzie kodu:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Token dostępu jest przechowywany jako ustawienie aplikacji, jest pobierane z magazynu specyficzne dla platformy i uwzględnione w wywołaniu `GetOrderAsync` method in Class metoda `OrderService` klasy.

Podobnie token dostępu muszą zostać uwzględnione podczas wysyłania danych do IdentityServer chronionego interfejsu API, jak pokazano w poniższym przykładzie kodu:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Token dostępu jest pobierane z magazynu specyficzne dla platformy i zawarty w wywołaniu `UpdateBasketAsync` method in Class metoda `BasketService` klasy.

`RequestProvider` Klasy, w ramach aplikacji eShopOnContainers aplikacji mobilnej, używa `HttpClient` klasy, aby wysyłać żądania do interfejsów API RESTful udostępnianych przez aplikację odwołania w ramach aplikacji eShopOnContainers. Podczas wprowadzania żądań do ustalania kolejności i koszyka interfejsów API, które wymagają autoryzacji, prawidłowym tokenem dostępu musi być dołączony do żądania. Jest to osiągane przez dodanie token dostępu do nagłówków `HttpClient` wystąpienia, jak pokazano w poniższym przykładzie kodu:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders` Właściwość `HttpClient` klasa udostępnia nagłówki, które są wysyłane z każdym żądaniem, a token dostępu zostanie dodany do `Authorization` nagłówków z prefiksem ciągu `Bearer`. Jeśli żądanie jest wysyłane do interfejsu API RESTful, wartość `Authorization` nagłówek jest wyodrębniany i weryfikowane tak, aby zagwarantować, że zostało wysłane z zaufanych wystawców i używany do określenia, czy użytkownik ma uprawnienia do wywoływania interfejsu API, która otrzymuje go.

Aby uzyskać więcej informacji na temat sposobu aplikacji mobilnej w ramach aplikacji eShopOnContainers sprawia, że żądania sieci web, zobacz [uzyskiwania dostępu do danych zdalnych](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Podsumowanie

Dostępnych jest wiele metod do integracji uwierzytelniania i autoryzacji w aplikacji platformy Xamarin.Forms, który komunikuje się z aplikacji sieci web ASP.NET MVC. Aplikacja mobilna w ramach aplikacji eShopOnContainers przeprowadza uwierzytelnianie i autoryzacja przy użyciu mikrousług konteneryzowanych tożsamości, który używa IdentityServer 4. IdentityServer to architektura typu open source OpenID Connect i OAuth 2.0 dla platformy ASP.NET Core, która integruje się z tożsamości platformy ASP.NET Core w celu przeprowadzenia uwierzytelniania tokenu elementu nośnego.

Aplikacja mobilna żąda tokeny zabezpieczające od IdentityServer, do uwierzytelniania użytkownika lub do uzyskiwania dostępu do zasobu. Podczas uzyskiwania dostępu do zasobu, token dostępu musi zawierać żądań do interfejsów API, które wymagają autoryzacji. Oprogramowanie pośredniczące firmy IdentityServer weryfikuje przychodzące tokeny dostępu w celu upewnij się, że są wysyłane z zaufanych wystawców i czy są one prawidłowe ma być używany z interfejsu API, który otrzymuje je.


## <a name="related-links"></a>Linki pokrewne

- [Pobierz książkę elektroniczną (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [ramach aplikacji eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
