---
title: Uwierzytelnianie i autoryzacja
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 9c6f3ae19b3e1b89220cbdf0985f4bdf789f2209
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="authentication-and-authorization"></a>Uwierzytelnianie i autoryzacja

Uwierzytelnianie to proces uzyskiwania od użytkownika poświadczeń tożsamości, takie jak nazwa i hasło, i sprawdzanie poprawności poświadczeń dla urzędu. Jeśli poświadczenia są prawidłowe, jednostki, który wysłał jest traktowany jako uwierzytelniony. Po uwierzytelnieniu tożsamości proces autoryzacji określa, czy jego tożsamość ma dostęp do zasobu.

Istnieje wiele sposobów integracji aplikacji platformy Xamarin.Forms, który komunikuje się z aplikacją sieci web platformy ASP.NET MVC przy użyciu dostawcy uwierzytelniania zewnętrznego, takich jak Microsoft, Google, ASP.NET Core Identity, w tym uwierzytelniania i autoryzacji Facebook, lub Twitter i uwierzytelniania oprogramowania pośredniczącego. Aplikacji mobilnej eShopOnContainers przeprowadza uwierzytelnianie i autoryzacja z mikrousługi konteneryzowanych tożsamości, która używa IdentityServer 4. Aplikacja mobilna żądanie tokeny zabezpieczające IdentityServer, do uwierzytelniania użytkownika lub do uzyskiwania dostępu do zasobu. Dla IdentityServer tokeny problem w imieniu użytkownika użytkownik musi Zaloguj się do IdentityServer. Jednak IdentityServer nie zapewnia interfejsu użytkownika lub bazy danych dla uwierzytelniania. W związku z tym w aplikacji odwołanie eShopOnContainers ASP.NET Core Identity jest używany w tym celu.

## <a name="authentication"></a>Uwierzytelnianie

Gdy aplikacja musi znać tożsamość bieżącego użytkownika jest wymagane uwierzytelnianie. Mechanizm podstawowej platformy ASP.NET Core do identyfikacji użytkowników jest systemu członkostwa ASP.NET Core Identity, która przechowuje informacje o użytkowniku w magazynie danych skonfigurowane przez dewelopera. Zazwyczaj ten magazyn danych zostanie zapisana EntityFramework, chociaż niestandardowe magazyny lub pakiety innych firm może służyć do przechowywania informacji o tożsamości usługi Azure storage, bazy danych Azure rozwiązania Cosmos lub innych lokalizacji.

Scenariusze uwierzytelniania, należy użyć magazynu danych użytkowników lokalnych i który utrwalenia informacji o tożsamości między żądaniami za pomocą plików cookie (podobnie jak w typowej w aplikacji sieci web platformy ASP.NET MVC), ASP.NET Core Identity to odpowiednie rozwiązanie. Jednak pliki cookie nie zawsze są fizyczną oznacza przechowywanie i przesyłania danych. Na przykład aplikacji sieci web platformy ASP.NET Core, który ujawnia RESTful punktów końcowych, które są dostępne z aplikacji mobilnej zwykle należy użyć tokenu uwierzytelniania elementu nośnego, ponieważ w tym scenariuszu nie można używać plików cookie. Jednak tokenów elementu nośnego można łatwo je pobrać i zawarte w nagłówku autoryzacji żądania sieci web z poziomu aplikacji mobilnej.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Wystawianie tokenów elementu nośnego przy użyciu IdentityServer 4

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) to struktura OpenID Connect i OAuth 2.0 typu open source dla platformy ASP.NET Core, którego można użyć w różnych scenariuszach uwierzytelniania i autoryzacji, łącznie z wystawiania tokenów zabezpieczających dla lokalnych użytkowników ASP.NET Core Identity.

> [!NOTE]
> OpenID Connect i OAuth 2.0 są bardzo podobne, a jednocześnie ma różne obowiązki.

OpenID Connect to warstwa uwierzytelniania na podstawie protokołu OAuth 2.0. OAuth 2 to protokół, który umożliwia aplikacjom do żądania tokenów dostępu z usługi tokenu zabezpieczeń i ich używać do komunikowania się z interfejsami API. To delegowanie zmniejsza złożoność API i aplikacje klienckie, ponieważ były scentralizowane uwierzytelnianie i autoryzację.

Kombinacja protokołu OpenID Connect i OAuth 2.0 połączyć dwóch podstawowych bezpieczeństwem uwierzytelniania i dostępu do interfejsu API, a IdentityServer 4 jest implementacją tych protokołów.

W aplikacjach używających bezpośrednia komunikacja klient mikrousługi, takie jak aplikacja referencyjna eShopOnContainers mikrousługi dedykowanych uwierzytelniania, działając jako zabezpieczeń tokenu usługi (STS) może służyć do uwierzytelniania użytkowników, jak pokazano na rysunku 9-1. Aby uzyskać więcej informacji na temat bezpośrednia komunikacja klient mikrousługi, zobacz [komunikacji między klientem i Mikrousług](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Uwierzytelnianie przez mikrousługi dedykowanych uwierzytelniania")

**Rysunek 9-1.** uwierzytelnienia za pomocą mikrousługi dedykowanych uwierzytelniania

Aplikacja mobilna eShopOnContainers komunikuje się z mikrousługi tożsamości, który używa IdentityServer 4 do uwierzytelniania i kontroli dostępu do interfejsów API. W związku z tym aplikacji mobilnej żądań tokenów z IdentityServer, do uwierzytelniania użytkownika lub do uzyskiwania dostępu do zasobu:

-   Uwierzytelnianie użytkowników z IdentityServer odbywa się przez zażądanie aplikacji mobilnej *tożsamości* tokenem, który reprezentuje wynik procesu uwierzytelniania. Co najmniej systemu od zera zawiera identyfikator użytkownika i dowiedzieć się, jak i kiedy użytkownik uwierzytelniony. Może ona także zawierać dane dodatkowe tożsamości.
-   Przez zażądanie aplikacji mobilnej uzyskuje się dostęp do zasobu z IdentityServer *dostępu* tokenem, który zezwala na dostęp do zasobu usługi interfejsu API. Klienci żądań tokenów dostępu i przekazują je do interfejsu API. Tokeny dostępu zawierają informacje o kliencie, a użytkownik (jeśli istnieje). Interfejsy API następnie użyć tych informacji do autoryzowania dostępu do swoich danych.

> [!NOTE]
> Klient musi być zarejestrowany z IdentityServer przed on żądania tokenów.

### <a name="adding-identityserver-to-a-web-application"></a>Dodawanie IdentityServer do aplikacji sieci Web

Aby aplikacja sieci web platformy ASP.NET Core może używać IdentityServer 4 musi ona dodana do aplikacji sieci web rozwiązania Visual Studio. Aby uzyskać więcej informacji, zobacz [instalacji i przegląd](https://identityserver4.readthedocs.io/en/release/quickstarts/0_overview.html) w dokumentacji IdentityServer.

Po IdentityServer jest uwzględniony w rozwiązaniu Visual Studio aplikacji sieci web, musi być dodana do aplikacji sieci web żądania HTTP przetwarzania potoku, tak, aby mógł obsługiwać żądań do punktów końcowych protokołu OpenID Connect i OAuth 2.0. Jest to osiągane w `Configure` metody w aplikacji sieci web `Startup` klasy, jak pokazano w poniższym przykładzie:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Kolejność ma znaczenie w aplikacji sieci web żądania HTTP przetwarzania potoku. W związku z tym IdentityServer należy dodać do potoku przed framework interfejsu użytkownika, który implementuje ekran logowania.

### <a name="configuring-identityserver"></a>Konfigurowanie IdentityServer

IdentityServer powinna być skonfigurowana w `ConfigureServices` metody w aplikacji sieci web `Startup` klasy przez wywołanie metody `services.AddIdentityServer` metody, jak pokazano w poniższym przykładzie kodu z aplikacji odwołanie eShopOnContainers:

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

Po wywołaniu `services.AddIdentityServer` metody dodatkowe interfejsy API fluent są nazywane skonfiguruj następujące opcje:

-   Poświadczenia używane do podpisywania.
-   Zasoby interfejsu API i tożsamości, które użytkownicy mogą żądać dostępu do.
-   Klienci, którzy będą łączyć się z żądania tokenów.
-   ASP.NET Core Identity.

>💡 **Porada**: dynamicznie załadować konfiguracji IdentityServer 4. IdentityServer 4 interfejsów API umożliwiają konfigurowanie IdentityServer z listy obiektów konfiguracji w pamięci. W aplikacji odwołanie eShopOnContainers kolekcjach w pamięci są zakodowane na stałe do aplikacji. Jednak w środowisku produkcyjnym scenariuszach mogą być załadować dynamicznie pliku konfiguracji lub z bazy danych.

Informacje o konfigurowaniu IdentityServer do korzystania z platformy ASP.NET Core tożsamości, zobacz [za pomocą ASP.NET Identity Core](https://identityserver4.readthedocs.io/en/release/quickstarts/6_aspnet_identity.html) w dokumentacji IdentityServer.

#### <a name="configuring-api-resources"></a>Konfigurowanie zasobów interfejsu API

Podczas konfigurowania zasobów interfejsu API `AddInMemoryApiResources` metoda oczekuje `IEnumerable<ApiResource>` kolekcji. Poniższy kod przedstawia przykład `GetApis` metodę, która zawiera tę kolekcję na eShopOnContainers zawierają odwołania do aplikacji:

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

Ta metoda określa, że IdentityServer powinna zapewniać ochronę zleceń i koszyka APIs. W związku z tym IdentityServer zarządzany dostęp tokenów będzie wymagane podczas wykonywania wywołań do tych interfejsów API. Aby uzyskać więcej informacji na temat `ApiResource` typu, zobacz [zasobu interfejsu API](https://identityserver4.readthedocs.io/en/release/reference/api_resource.html#refapiresource) w dokumentacji IdentityServer 4.

#### <a name="configuring-identity-resources"></a>Konfigurowanie tożsamości zasobów

Podczas konfigurowania zasobów tożsamości `AddInMemoryIdentityResources` metoda oczekuje `IEnumerable<IdentityResource>` kolekcji. Zasoby tożsamości są dane, takie jak nazwa użytkownika, nazwa lub adres e-mail. Każdy zasób tożsamości ma unikatową nazwę i dowolnego oświadczenia można przypisać do niego, które zostaną następnie uwzględnione w tokenie tożsamości użytkownika. Poniższy kod przedstawia przykład `GetResources` metodę, która zawiera tę kolekcję na eShopOnContainers zawierają odwołania do aplikacji:

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

Specyfikacja protokołu OpenID Connect określa niektóre [tożsamości standardowe zasoby](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). Minimalna wymagana wartość to jest obsługiwane dla emitowanie Unikatowy identyfikator dla użytkowników. Jest to osiągane przez udostępnianie `IdentityResources.OpenId` tożsamości zasobu.

> [!NOTE]
> `IdentityResources` Klasa obsługuje wszystkie zakresy specyfikacją OpenID Connect (openid, poczty e-mail, profil, telefonu i adresu).

IdentityServer obsługuje również definiowanie zasobów tożsamości niestandardowej. Aby uzyskać więcej informacji, zobacz [Definiowanie zasobów tożsamość niestandardowa](https://identityserver4.readthedocs.io/en/release/topics/resources.html#defining-custom-identity-resources) w dokumentacji IdentityServer. Aby uzyskać więcej informacji na temat `IdentityResource` typu, zobacz [zasobu tożsamości](https://identityserver4.readthedocs.io/en/release/reference/identity_resource.html) w dokumentacji IdentityServer 4.

#### <a name="configuring-clients"></a>Konfigurowanie klientów

Klienci znajdują się aplikacje, które mogą żądać tokenów z IdentityServer. Zazwyczaj poniższe ustawienia muszą zostać zdefiniowane dla każdego klienta, co najmniej:

-   Identyfikator unikatowy klienta.
-   Dozwolone interakcji z usługą tokenu (znany jako typu przydziału).
-   Lokalizacji, w którym tożsamościami i dostępem tokeny są wysyłane do (znany jako identyfikator URI przekierowania).
-   Lista zasobów, które klient ma dostęp do (nazywanych zakresami).

Podczas konfigurowania klientów, `AddInMemoryClients` metoda oczekuje `IEnumerable<Client>` kolekcji. Poniższy przykład kodu pokazuje konfiguracji aplikacji mobilnej eShopOnContainers w `GetClients` metodę, która zawiera tę kolekcję na eShopOnContainers zawierają odwołania do aplikacji:

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
-   `ClientName`: Klient nazwę wyświetlaną, który służy do rejestrowania i ekran zgody.
-   `AllowedGrantTypes`: Określa, jak klient wykonuje działanie obejmujące IdentityServer. Aby uzyskać więcej informacji, zobacz [Konfigurowanie przepływ uwierzytelniania](#configuring_the_authentication_flow).
-   `ClientSecrets`: Określa tajnego poświadczeń klienta, które są używane podczas żądania tokenów z punktu końcowego tokena.
-   `RedirectUris`: Określa dozwolone identyfikatory URI, do którego ma zostać zwrócona tokeny lub kodach autoryzacji.
-   `RequireConsent`: Określa, czy ekran zgody jest wymagana.
-   `RequirePkce`: Określa, czy klientów przy użyciu kodu autoryzacji musi wysłać klucza potwierdzającego.
-   `PostLogoutRedirectUris`: Określa dozwolone identyfikatory URI przekierowania po wylogowania.
-   `AllowedCorsOrigins`: Określa punkt początkowy klienta, dzięki czemu można zezwolić na IdentityServer wywołań między źródłami ze źródła.
-   `AllowedScopes`: Określa, który klient ma dostęp do zasobów. Domyślnie klient nie ma dostępu do żadnych zasobów.
-   `AllowOfflineAccess`: Określa, czy klient może żądać tokenów odświeżania.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Konfigurowanie przepływu uwierzytelniania

Przepływ uwierzytelniania między klientem a IdentityServer można skonfigurować przez określenie typów przydziałów w `Client.AllowedGrantTypes` właściwości. Specyfikacje OpenID Connect i OAuth 2.0 zdefiniować wiele przepływów uwierzytelniania, w tym:

-   Niejawne. Ten przepływ jest zoptymalizowana pod kątem aplikacji opartych na przeglądarce i powinna być używana dla użytkownika tylko do uwierzytelniania lub żądania uwierzytelniania i dostęp do tokenu. Wszystkie tokeny są przesyłane za pośrednictwem przeglądarki, a w związku z tym zaawansowane funkcje, takie jak tokenów odświeżania są niedozwolone.
-   Kod autoryzacji. Ten przepływ umożliwia pobranie tokenów na kanału zwrotnego, w przeciwieństwie do kanału front przeglądarki, również obsługuje uwierzytelnianie klienta.
-   Hybrydowe. Ten przebieg jest kombinacją niejawne i typy przydziałów kod autoryzacji. Token tożsamości są przesyłane za pośrednictwem kanału przeglądarki i zawiera odpowiedzi protokołu podpisem wraz z pozostałych artefaktów, takich jak kod autoryzacji. Po sprawdzeniu poprawności odpowiedzi kanału zwrotnego należy pobrać dostępu i token odświeżania.

> [!TIP]
> Użyj przepływ uwierzytelniania hybrydowa. Przepływ uwierzytelniania hybrydowa zmniejsza liczbę ataków, które są stosowane do kanału przeglądarki i jest zalecany przepływ dla natywnych aplikacji, które mają być pobranie tokenów dostępu (i prawdopodobnie tokenów odświeżania).

Aby uzyskać więcej informacji na temat przepływów uwierzytelniania, zobacz [typów przydziałów](https://identityserver4.readthedocs.io/en/release/topics/grant_types.html) w dokumentacji IdentityServer 4.

### <a name="performing-authentication"></a>Uwierzytelniania

Dla IdentityServer tokeny problem w imieniu użytkownika użytkownik musi Zaloguj się do IdentityServer. Jednak IdentityServer nie zapewnia interfejsu użytkownika lub bazy danych dla uwierzytelniania. W związku z tym w aplikacji odwołanie eShopOnContainers ASP.NET Core Identity jest używany w tym celu.

Aplikacja mobilna eShopOnContainers jest uwierzytelniany w usłudze IdentityServer z przepływ uwierzytelniania hybrydowa, który przedstawiono na rysunku 9-2.

![](authentication-and-authorization-images/sign-in.png "Omówienie procesu logowania")

**Rysunek 9-2:** ogólne omówienie procesu logowania

Zaloguj się w żądaniu skierowanym do `<base endpoint>:5105/connect/authorize`. Po pomyślnym uwierzytelnieniu IdentityServer zwraca odpowiedź uwierzytelnienia zawierający kod autoryzacji i tokena tożsamości. Kod autoryzacji są następnie wysyłane do `<base endpoint>:5105/connect/token`, która odpowiada dostępu, tożsamości i tokenów odświeżania.

EShopOnContainers aplikacji mobilnej znaki poza IdentityServer, wysyłając żądanie `<base endpoint>:5105/connect/endsession`, z dodatkowymi parametrami. Po wystąpieniu wylogowania IdentityServer reaguje wysłaniem post identyfikator URI przekierowania wylogowania z powrotem do aplikacji mobilnej. Rysunek 9-3 przedstawiono ten proces.

![](authentication-and-authorization-images/sign-out.png "Omówienie procesu wylogowania")

**Rysunek 9 — 3:** ogólne omówienie proces logowania

W aplikacji mobilnej eShopOnContainers komunikację z IdentityServer jest wykonywane przez `IdentityService` klasy, która implementuje `IIdentityService` interfejsu. Ten interfejs określa, że klasy implementującej musi zapewniać `CreateAuthorizationRequest`, `CreateLogoutRequest`, i `GetTokenAsync` metody.

#### <a name="signing-in"></a>Signing-in

Po naciśnięciu **logowania** znajdującego się na `LoginView`, `SignInCommand` w `LoginViewModel` klasy jest wykonywane, który z kolei wykonuje `SignInAsync` metody. Poniższy przykład kodu pokazuje tej metody:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Ta metoda wywołuje `CreateAuthorizationRequest` metoda `IdentityService` klasy, co przedstawiono w poniższym przykładzie:

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

Ta metoda tworzy identyfikator URI dla tego IdentityServer [punktu końcowego autoryzacji](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), z wymaganymi parametrami. Punkt końcowy autoryzacji jest na `/connect/authorize` na porcie 5105 podstawowy punkt końcowy udostępniony jako ustawienia użytkownika. Aby uzyskać więcej informacji na temat ustawień użytkownika, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Obszar ataków, aplikacji mobilnej eShopOnContainers zostanie zmniejszona zaimplementowanie klucza dowód dla kodu programu Exchange (PKCE) rozszerzenia OAuth. PKCE uniemożliwia używany, jeśli zostanie przechwycona przez kod autoryzacji. Jest to osiągane przez klienta generowania tajny weryfikatora żądania autoryzacji jest przekazywana przez skrótu, i która jest prezentowana bez haszowania podczas realizowanie kod autoryzacji. Aby uzyskać więcej informacji o PKCE, zobacz [dowód klucza wymiany kodu przez klientów publicznego OAuth](https://tools.ietf.org/html/rfc7636) w witrynie sieci web Internet Engineering Task Force.

Zwracane identyfikatora URI jest przechowywany w `LoginUrl` właściwość `LoginViewModel` klasy. Gdy `IsLogin` staje się właściwość `true`, [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) w `LoginView` staje się widoczna. `WebView` Powiązania danych przy użyciu jego [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) właściwości `LoginUrl` właściwość `LoginViewModel` klasy, a więc zgłasza żądanie logowania IdentityServer podczas `LoginUrl` ma ustawioną wartość właściwości IdentityServer dla punktu końcowego autoryzacji. Gdy IdentityServer odbiera żądanie, a użytkownik nie jest uwierzytelniony, `WebView` nastąpi przekierowanie do strony logowania skonfigurowany, który jest wyświetlany w rysunek 9-4.

![](authentication-and-authorization-images/login.png "Strona logowania wyświetlane w widoku sieci Web")

**Rysunek 9 — 4:** strony logowania wyświetlane w widoku sieci Web

Po zakończeniu logowania [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) nastąpi przekierowanie do zwracane identyfikatora URI. To `WebView` nawigacji spowoduje, że `NavigateAsync` metoda `LoginViewModel` klasy do wykonania, które przedstawiono w poniższym przykładzie:

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

Ta metoda analizuje odpowiedzi uwierzytelniania, która znajduje się w zwracanych identyfikatora URI i pod warunkiem, że występuje kod autoryzacji prawidłowy zgłasza żądanie do jego IdentityServer [punktu końcowego tokena](https://identityserver4.readthedocs.io/en/release/endpoints/token.html), przekazywanie kod autoryzacji Tajny weryfikatora PKCE i inne wymagane parametry. Token punktu końcowego wynosi `/connect/token` na porcie 5105 podstawowy punkt końcowy udostępniony jako ustawienia użytkownika. Aby uzyskać więcej informacji na temat ustawień użytkownika, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

>💡 **Porada**: Sprawdzanie poprawności zwracać identyfikatorów URI. Mimo że eShopOnContainers aplikacji mobilnej nie zweryfikować zwracane identyfikatora URI, najlepszym rozwiązaniem jest zweryfikować, że zwracane identyfikatora URI odwołuje się do znanej lokalizacji, aby zapobiec atakom przekierowania Otwórz.

Punktu końcowego tokena odbiera kod prawidłowego autoryzacji i weryfikatora tajny PKCE, należy przeprowadzić z tokenu dostępu, token tożsamości i token odświeżania. Token dostępu, (który umożliwia dostęp do zasobów interfejsu API) i token tożsamości są następnie przechowywane jako ustawienia aplikacji i odbywa się Nawigacja strony. W związku z tym jest to ogólny efekt w aplikacji mobilnej eShopOnContainers: pod warunkiem, że użytkownicy będą mogli pomyślnie uwierzytelnić z IdentityServer, są one przejście `MainView` strony, która jest [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) która wyświetla `CatalogView` jako jego wybranej karty.

Informacje o nawigacji strony, zobacz [nawigacji](~/xamarin-forms/enterprise-application-patterns/navigation.md). Aby uzyskać informacje na temat [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) nawigacji powoduje, że metoda modelu widoku, można wykonać, zobacz [wywoływania nawigacji za pomocą zachowań](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Aby uzyskać informacje o ustawieniach aplikacji, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> EShopOnContainers umożliwia również logowanie zasymulować gdy aplikacja jest skonfigurowana do używania usług zasymulować w `SettingsView`. W tym trybie aplikacja nie komunikują się z IdentityServer, zamiast tego umożliwiający użytkownikowi logowanie przy użyciu dowolnego poświadczeń.

#### <a name="signing-out"></a>Signing-out

Po naciśnięciu **WYLOGUJ** przycisk `ProfileView`, `LogoutCommand` w `ProfileViewModel` klasy jest wykonywane, który z kolei wykonuje `LogoutAsync` metody. Ta metoda wykonuje nawigacji strony, aby `LoginView` strony przekazywanie `LogoutParameter` ustawioną wystąpienia `true` jako parametr. Aby uzyskać więcej informacji na temat przekazywanie parametrów podczas nawigacji strony, zobacz [przekazywanie parametrów podczas nawigacji](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Podczas tworzenia i przejście do widoku `InitializeAsync` metody model skojarzony widok widoku jest wykonywane, która następnie wykonuje `Logout` metody `LoginViewModel` klasy, co przedstawiono w poniższym przykładzie:

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

Ta metoda wywołuje `CreateLogoutRequest` metoda `IdentityService` klasy, przekazując token tożsamości pobierane z ustawień aplikacji jako parametr. Aby uzyskać więcej informacji na temat ustawień aplikacji, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Poniższy kod przedstawia przykład `CreateLogoutRequest` metody:

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

Ta metoda tworzy identyfikator URI do jego IdentityServer [zakończenia sesji punktu końcowego](https://identityserver4.readthedocs.io/en/release/endpoints/endsession.html#refendsession), z wymaganymi parametrami. Punkt końcowy sesji zakończenia jest w `/connect/endsession` na porcie 5105 podstawowy punkt końcowy udostępniony jako ustawienia użytkownika. Aby uzyskać więcej informacji na temat ustawień użytkownika, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Zwracane identyfikatora URI jest przechowywany w `LoginUrl` właściwość `LoginViewModel` klasy. Gdy `IsLogin` właściwość jest `true`, [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) w `LoginView` jest widoczny. `WebView` Powiązania danych przy użyciu jego [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) właściwości `LoginUrl` właściwość `LoginViewModel` klasy, a więc zgłasza żądanie wylogowania IdentityServer podczas `LoginUrl` ma ustawioną wartość właściwości IdentityServer na końcu sesji punkt końcowy. Gdy IdentityServer odbiera to żądanie, pod warunkiem, że użytkownik jest zalogowany, są wylogowania występuje. Uwierzytelnianie jest śledzony z pliku cookie zarządzane przez oprogramowanie pośredniczące uwierzytelniania plików cookie z platformy ASP.NET Core. W związku z tym podpisywania poza IdentityServer usuwa plik cookie uwierzytelniania i wysyła przekierowania wylogowania post identyfikator URI z powrotem do klienta.

W aplikacji mobilnej [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) nastąpi przekierowanie do identyfikatora URI przekierowania wylogowania post. To `WebView` nawigacji spowoduje, że `NavigateAsync` metoda `LoginViewModel` klasy do wykonania, które przedstawiono w poniższym przykładzie:

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

Ta metoda usuwa token tożsamości jak token dostępu z ustawień aplikacji i ustawia `IsLogin` właściwości `false`, co powoduje, że [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) na `LoginView` stronę, aby stają się niewidoczne . Na koniec `LoginUrl` właściwość jest ustawiona na identyfikator URI IdentityServer [punktu końcowego autoryzacji](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), z wymaganymi parametrami w ramach przygotowania do następnego użytkownik inicjuje logowania.

Informacje o nawigacji strony, zobacz [nawigacji](~/xamarin-forms/enterprise-application-patterns/navigation.md). Aby uzyskać informacje na temat [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) nawigacji powoduje, że metoda modelu widoku, można wykonać, zobacz [wywoływania nawigacji za pomocą zachowań](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Aby uzyskać informacje o ustawieniach aplikacji, zobacz [zarządzania konfiguracją](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> EShopOnContainers umożliwia także makiety wylogowania, gdy aplikacja jest skonfigurowana do używania usług zasymulować w SettingsView. W tym trybie aplikacja nie komunikują się z IdentityServer i zamiast tego czyści przechowywanych tokenów z ustawień aplikacji.

<a name="authorization" />

## <a name="authorization"></a>Autoryzacja

Po uwierzytelnieniu sieci web platformy ASP.NET Core API często muszą do autoryzowania dostępu, które umożliwia usłudze upewnij interfejsów API niektórych uwierzytelnionych użytkowników, ale nie do wszystkich.

Ograniczanie dostępu do platformy ASP.NET Core MVC trasy uzyskuje się poprzez zastosowanie atrybutu autoryzacji do kontrolera lub akcji, który ogranicza dostęp do kontrolera lub akcji do uwierzytelnionych użytkowników, jak pokazano w poniższym przykładzie:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Jeśli nieautoryzowany użytkownik próbuje uzyskać dostęp do kontrolera lub akcji, który jest oznaczony atrybutem `Authorize` atrybutu, struktura MVC zwróci kod stanu 401 (nieautoryzowanych) HTTP.

> [!NOTE]
> Parametry można określić na `Authorize` atrybutu, aby ograniczyć interfejs API do określonych użytkowników. Aby uzyskać więcej informacji, zobacz [autoryzacji](/aspnet/core/security/authorization/introduction/).

IdentityServer można zintegrować przepływu pracy autoryzacji, aby tokeny dostępu umożliwia Sterowanie autoryzacją. Takie podejście jest wyświetlany w rysunek 9 – 5.

![](authentication-and-authorization-images/authorization.png "Autoryzacji przez token dostępu")

**Rysunek 9 — 5:** autoryzacji przez token dostępu

Aplikacja mobilna eShopOnContainers komunikuje się z mikrousługi tożsamości i żądania tokenu dostępu w ramach procesu uwierzytelniania. Token dostępu jest następnie przekazywane do interfejsach API udostępnianych przez mikrousług porządkowania i koszyka jako część żądania dostępu. Tokeny dostępu zawierają informacje o kliencie, a użytkownik. Interfejsy API następnie użyć tych informacji do autoryzowania dostępu do swoich danych. Aby uzyskać informacje o sposobie konfigurowania IdentityServer do chronienia interfejsów API, zobacz [Konfigurowanie zasobów interfejsu API](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Konfigurowanie IdentityServer przeprowadzić autoryzacji

Aby przeprowadzić autoryzacji z IdentityServer, jego oprogramowanie pośredniczące autoryzacji musi można dodać do Potok żądań HTTP w aplikacji sieci web. Oprogramowanie pośredniczące został dodany w `ConfigureAuth` metody w aplikacji sieci web `Startup` klasy, który jest wywoływany z `Configure` metody i przedstawiono w poniższym przykładzie kodu z aplikacji odwołanie eShopOnContainers:

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

Ta metoda gwarantuje, że interfejsu API można uzyskać tylko z tokenem dostępu prawidłowe. Oprogramowanie pośredniczące weryfikuje przychodzącym tokenie, aby upewnić się, że jest wysyłany z zaufanym wystawcą i weryfikuje, czy token jest ważny do użycia przy użyciu interfejsu API, który odbiera on. W związku z tym przeglądanie do kontrolera porządkowanie lub koszyka zwróci (nieautoryzowanych) kod stanu HTTP 401, wskazujący, że token dostępu jest wymagana.

> [!NOTE]
> Oprogramowanie pośredniczące autoryzacji na IdentityServer musi zostać dodany do potoku żądania HTTP dla aplikacji sieci web przed dodaniem MVC z `app.UseMvc()` lub `app.UseMvcWithDefaultRoute()`.

### <a name="making-access-requests-to-apis"></a>Tworzenie żądania dostępu do interfejsów API

W przypadku wysyłania żądań do mikrousług porządkowania i koszyka, dostęp tokenu, uzyskane z IdentityServer podczas procesu uwierzytelniania muszą być zawarte w żądaniu, jak pokazano w poniższym przykładzie:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Token dostępu jest przechowywana jako ustawienie aplikacji, pobrać z magazynu specyficzne dla platformy i które są zawarte w wywołaniu `GetOrderAsync` metoda `OrderService` klasy.

Podobnie token dostępu muszą być uwzględnione podczas wysyłania danych do IdentityServer chronione interfejsu API, jak pokazano w poniższym przykładzie:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Token dostępu jest pobierana z magazynu specyficzne dla platformy i zawarte w wywołaniu `UpdateBasketAsync` metoda `BasketService` klasy.

`RequestProvider` Klasy, w aplikacji mobilnej eShopOnContainers używa `HttpClient` klasy na wysyłanie żądań do interfejsów API RESTful udostępniany przez aplikację odwołanie eShopOnContainers. Podczas wprowadzania żądania do porządkowania i koszyka interfejsów API, które wymagają autoryzacji, tokenu dostępu prawidłowy muszą być zawarte w żądaniu. Jest to osiągane przez dodanie token dostępu do nagłówki `HttpClient` wystąpienie, jak pokazano w poniższym przykładzie:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders` Właściwość `HttpClient` klasa przedstawia nagłówki, które są wysyłane z każdym żądaniem, a token dostępu jest dodawany do `Authorization` nagłówka poprzedzona ciągiem `Bearer`. Gdy żądanie zostanie wysłane do interfejsu API RESTful, wartość `Authorization` nagłówka jest wyodrębniany i zatwierdzone do zapewnienia, że został wysłany z zaufanym wystawcą i używany w celu określenia, czy użytkownik ma uprawnienia do wywoływania interfejsu API odbierająca go.

Aby uzyskać więcej informacji dotyczących sposobu aplikacji mobilnej eShopOnContainers zgłasza żądania sieci web, zobacz [podczas uzyskiwania dostępu do danych zdalnych](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Podsumowanie

Istnieje wiele sposobów integracji aplikacji platformy Xamarin.Forms, który komunikuje się z aplikacji sieci web platformy ASP.NET MVC uwierzytelniania i autoryzacji. Aplikacji mobilnej eShopOnContainers przeprowadza uwierzytelnianie i autoryzacja z mikrousługi konteneryzowanych tożsamości, która używa IdentityServer 4. IdentityServer to struktura OpenID Connect i OAuth 2.0 typu open source dla platformy ASP.NET Core, która integruje się z ASP.NET Identity Core do uwierzytelniania tokenu elementu nośnego.

Aplikacja mobilna żądanie tokeny zabezpieczające IdentityServer, do uwierzytelniania użytkownika lub do uzyskiwania dostępu do zasobu. Podczas uzyskiwania dostępu do zasobu, token dostępu musi być uwzględniona w żądaniu do interfejsów API, która wymaga autoryzacji. Oprogramowanie pośredniczące w IdentityServer weryfikuje przychodzących tokenów dostępu, aby upewnić się, te są wysyłane z zaufanym wystawcą i są prawidłowe do użycia przy użyciu interfejsu API, który odbiera je.


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
