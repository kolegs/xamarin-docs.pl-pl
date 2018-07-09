---
title: Uwierzytelnianie użytkowników za pomocą dostawcy tożsamości
description: W tym artykule wyjaśniono, jak używać Xamarin.Auth do zarządzania procesem uwierzytelniania w aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: 504b2789ef61b0339d1c32e92c852a779a193b52
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854769"
---
# <a name="authenticating-users-with-an-identity-provider"></a>Uwierzytelnianie użytkowników za pomocą dostawcy tożsamości

_Xamarin.Auth jest zestaw SDK dla wielu platform, uwierzytelniania użytkowników i przechowywanie ich kont. Obejmuje ona wystawcy uwierzytelniania OAuth, które zapewniają obsługę, co umożliwia korzystanie z dostawców tożsamości, takich jak Google, Microsoft, Facebook i Twitter. W tym artykule wyjaśniono, jak używać Xamarin.Auth do zarządzania procesem uwierzytelniania w aplikacji platformy Xamarin.Forms._

OAuth jest otwarty standard do uwierzytelniania i umożliwia właściciela zasobu powiadomić dostawcy zasobów, czy należy udzielić zezwolenia stronom trzecim na dostęp do swoich informacji bez udostępniania tożsamości właścicieli zasobu. Na przykład spowoduje włączenie użytkownika, aby powiadomić dostawcy tożsamości (np. Google, Microsoft, Facebook i Twitter), czy należy udzielić zezwolenia aplikacji dostęp do swoich danych, bez udostępniania tożsamość użytkownika. Najczęściej jest używana jako podejście dla użytkowników w zalogować się do witryny sieci Web i aplikacji przy użyciu dostawcy tożsamości, ale bez ujawniania hasła do aplikacji lub witryny sieci Web.

Ogólne omówienie przepływu uwierzytelniania podczas używania dostawcy tożsamości OAuth jest następująca:

1. Aplikacja przechodzi przeglądarki, aby adres URL dostawcy tożsamości.
1. Dostawca tożsamości obsługuje uwierzytelnianie użytkowników i zwraca kod autoryzacji do aplikacji.
1. Aplikacja wymienia kodu autoryzacji dla tokenu dostępu od dostawcy tożsamości.
1. Aplikacja używa tokenu dostępu, dostęp do interfejsów API dla dostawcy tożsamości, takich jak interfejs API dla żądania danych użytkowników w warstwie podstawowa.

Przykładowa aplikacja demonstruje sposób skorzystania Xamarin.Auth do zaimplementowania przepływu uwierzytelniania macierzystego względem Google. Google jest używane jako dostawcy tożsamości, w tym temacie, to podejście jest równie mające zastosowanie do innych dostawców tożsamości. Aby uzyskać więcej informacji na temat uwierzytelniania przy użyciu punktu końcowego protokołu OAuth 2.0 firmy Google, zobacz [OAuth2.0 przy użyciu interfejsów API Google dostępu](https://developers.google.com/identity/protocols/OAuth2) w witrynie internetowej firmy Google.

> [!NOTE]
> W systemie iOS 9 lub nowszym App Transport Security (ATS) wymusza nawiązywać bezpieczne połączenia między zasobami Internetu (na przykład serwer zaplecza aplikacji) i aplikacji, zapobiegając w ten sposób przypadkowego ujawnienia poufnych informacji. Ponieważ ATS jest włączone domyślnie aplikacje są kompilowane dla systemu iOS 9, wszystkie połączenia będą podlegać wymagań dotyczących zabezpieczeń ATS. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.
> Jeśli nie jest możliwe użycie ATS można wyłączony z `HTTPS` protokołu i bezpiecznej komunikacji dla zasobów w Internecie. Można to osiągnąć przez zaktualizowanie aplikacji **Info.plist** pliku. Aby uzyskać więcej informacji, zobacz [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="using-xamarinauth-to-authenticate-users"></a>Uwierzytelnianie użytkowników przy użyciu Xamarin.Auth

Xamarin.Auth obsługuje dwa podejścia do aplikacjom komunikowanie się z punktu końcowego autoryzacji dostawcy tożsamości:

1. Przy użyciu widoku sieci web osadzonych. Gdy jest to powszechną praktyką, nie jest zalecane w następujących sytuacjach:

    - Aplikacji, który jest hostem widoku sieci web można uzyskać dostęp do poświadczeń pełnego uwierzytelniania użytkownika, nie tylko udzielania autoryzacji OAuth, która jest przeznaczona dla aplikacji. Narusza zasadę najmniejszych uprawnień, ponieważ aplikacja ma dostęp do poświadczeń bardziej wydajne niż go wymaga potencjalnie zwiększenia powierzchni ataku w aplikacji.
    - Aplikacji hosta można przechwycić nazwy użytkowników i hasła, automatycznie przesyłania formularzy obejścia zgody użytkownika i kopiowanie plików cookie sesji i ich używać do wykonywania akcji uwierzytelniony jako użytkownik.
    - Widoki sieci web Embedded nie udostępniaj stan uwierzytelniania z innych aplikacji lub przeglądarkę internetową na urządzeniu, użytkownik musi zalogować przy każdym żądaniu autoryzacji, co jest uznawane za środowisko użytkownika mniejszą.
    - Niektóre punkty końcowe autoryzacji, wykonaj kroki, aby wykrywać i blokować żądania autoryzacji, które pochodzą z widoki sieci web.

1. Za pomocą przeglądarki sieci web urządzenia, która przedstawia zalecane podejście. Przy użyciu przeglądarki urządzenia dla żądania OAuth zwiększa użyteczność aplikacji, ponieważ użytkownicy potrzebują tylko zalogować się do dostawcy tożsamości raz na urządzeniu, zwiększanie liczby pozyskanych przepływów logowania i autoryzacji w aplikacji. W przeglądarce na urządzeniu również zapewnia wyższy poziom zabezpieczeń, ponieważ aplikacje są w stanie sprawdzanie i modyfikowanie zawartości w widoku sieci web, ale nie zawartości wyświetlane w przeglądarce. To podejście w tej artykuł i przykładowej aplikacji.

Ogólne omówienie, jak przykładowa aplikacja używa Xamarin.Auth do uwierzytelniania użytkowników oraz pobierania ich podstawowych danych przedstawiono na poniższym diagramie:

![](oauth-images/google-auth.png "Za pomocą Xamarin.Auth, aby uwierzytelniać za pomocą usługi Google")

Aplikacja sprawia, że żądanie uwierzytelnienia za pomocą Google `OAuth2Authenticator` klasy. Odpowiedź uwierzytelniania jest zwracany po użytkownik został pomyślnie uwierzytelniony za pomocą usługi Google za pośrednictwem ich strony logowania, który zawiera token dostępu. Następnie aplikacja wysyła żądanie do usługi Google do danych użytkowników w warstwie podstawowa, przy użyciu `OAuth2Request` klasy przy użyciu tokenu dostępu, są uwzględnione w żądaniu.

### <a name="setup"></a>Konfiguracja

Projekt konsoli interfejsu API Google musi zostać utworzona zintegrować Google Zaloguj się przy użyciu aplikacji platformy Xamarin.Forms. Można to zrobić w następujący sposób:

1. Przejdź do [Konsola interfejsu API Google](http://console.developers.google.com) witryny sieci Web i zaloguj się przy użyciu poświadczeń konta Google.
1. Z projektu listę rozwijaną wybierz istniejący projekt lub Utwórz nową.
1. Na pasku bocznym, w obszarze "Menedżer interfejsu API", wybierz **poświadczenia**, a następnie wybierz **kartę ekran zgody OAuth**. Wybierz **adres E-mail**, określ **nazwa produktu pokazywana użytkownikom**i naciśnij klawisz **Zapisz**.
1. W **poświadczenia** zaznacz **Utwórz poświadczenia** listy rozwijanej, a wybierz **identyfikator klienta OAuth**.
1. W obszarze **typ aplikacji**, wybierz platformę, na której zostanie uruchomiony w aplikacji mobilnej (**dla systemu iOS** lub **Android**).
1. Wypełnij wymagane szczegóły, a następnie wybierz **Utwórz** przycisku.

> [!NOTE]
> Identyfikator klienta umożliwia aplikacji dostęp do włączone interfejsy API Google, a dla aplikacji mobilnych jest unikatowy dla pojedynczej platformy. W związku z tym **identyfikator klienta OAuth** powinny być tworzone dla każdej platformy, który będzie używany logowania Google.

Po wykonaniu tych kroków, Xamarin.Auth może służyć do inicjowania przepływ uwierzytelniania OAuth2 za pomocą usługi Google.

### <a name="creating-and-configuring-an-authenticator"></a>Tworzenie i konfigurowanie uwierzytelniania

Firmy Xamarin.Auth `OAuth2Authenticator` klasa jest odpowiedzialny za obsługę przepływu uwierzytelniania OAuth. Poniższy przykład kodu pokazuje procesu tworzenia wystąpienia `OAuth2Authenticator` klasy podczas przeprowadzania uwierzytelniania za pomocą przeglądarki sieci web urządzenia:

```csharp
var authenticator = new OAuth2Authenticator(
    clientId,
    null,
    Constants.Scope,
    new Uri(Constants.AuthorizeUrl),
    new Uri(redirectUri),
    new Uri(Constants.AccessTokenUrl),
    null,
    true);
```

`OAuth2Authenticator` Klasy wymaga kilku parametrów, które są następujące:

- **Identyfikator klienta** — identyfikuje klienta, który wysłał żądanie i mogą być pobierane z projektu w [Konsola interfejsu API Google](http://console.developers.google.com).
- **Klucz tajny klienta** — powinna to być `null` lub `string.Empty`.
- **Zakres** — identyfikuje dostęp do interfejsu API, które są wymagane przez aplikację, a wartość informuje ekranie wyrażania zgody, która jest wyświetlana użytkownikowi. Aby uzyskać więcej informacji na temat zakresów, zobacz [żądania autoryzacji interfejsu API](https://developers.google.com/+/web/api/rest/oauth) w witrynie internetowej firmy Google.
- **Autoryzacja adresów URL** — identyfikuje adresu URL, gdzie otrzymany kod autoryzacji.
- **Adres URL przekierowania** — identyfikuje adres URL, do których będą wysyłane odpowiedzi. Wartość tego parametru musi odpowiadać jednej z wartości, które pojawia się w **poświadczenia** kartę dla projektu w [konsoli deweloperów Google](https://console.developers.google.com/).
- **Adres AccessToken Url** — identyfikuje adres URL używany do żądania tokenów dostępu po uzyskaniu kodu autoryzacji.
- **GetUserNameAsync Func** — opcjonalnie `Func` posłuży asynchronicznie pobrać nazwę użytkownika konta, po to został pomyślnie uwierzytelniony.
- **Za pomocą natywnego interfejsu użytkownika** — `boolean` wartość wskazującą, czy używać przeglądarki sieci web urządzenia do wykonywania żądania uwierzytelnienia.

### <a name="setup-authentication-event-handlers"></a>Konfigurowanie programów obsługi zdarzeń uwierzytelniania

Przed rozpoczęciem prezentacji interfejsu użytkownika programu obsługi zdarzeń dla `OAuth2Authenticator.Completed` zdarzenia muszą być zarejestrowane, jak pokazano w poniższym przykładzie kodu:

```csharp
authenticator.Completed += OnAuthCompleted;
```

To zdarzenie zostanie uruchomiony, gdy użytkownik pomyślnie uwierzytelnia lub anuluje logowania.

Opcjonalnie, program obsługi zdarzeń dla `OAuth2Authenticator.Error` również mogą być rejestrowane zdarzenia.

### <a name="presenting-the-sign-in-user-interface"></a>Umożliwienie korzystania z interfejsu użytkownika logowania

Interfejs użytkownika logowania użytkownika można wyświetlić za pomocą Xamarin.Auth prezentera logowania, które muszą być zainicjowane w każdym projekcie platformy. Poniższy przykład kodu pokazuje, jak zainicjować Prelegenci logowania w `AppDelegate` klasy w projekcie dla systemu iOS:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

Poniższy przykład kodu pokazuje, jak zainicjować Prelegenci logowania w `MainActivity` klasy w projekcie dla systemu Android:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

Projekt biblioteki .NET Standard można następnie wywołać prezentera logowania w następujący sposób:

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

Należy pamiętać, że argument `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` metodą jest `OAuth2Authenticator` wystąpienia. Gdy `Login` metoda jest wywoływana, interfejs użytkownika logowania są prezentowane użytkownikowi na karcie w przeglądarce internetowej urządzenia, który jest pokazany na poniższych zrzutach ekranu:

![](oauth-images/login.png "Google Sign In")

### <a name="processing-the-redirect-url"></a>Przetwarzania adresu URL przekierowania

Po ukończeniu procesu uwierzytelniania użytkownika kontroli powróci do aplikacji z kartę przeglądarki sieci web. Jest to osiągane przez zarejestrowanie niestandardowy schemat adresu URL dla adresu URL przekierowania, który jest zwracany z procesu uwierzytelniania, a następnie wykrywania i obsługi niestandardowego adresu URL po wysłaniu.

Podczas wybierania niestandardowy schemat adresu URL do skojarzenia z aplikacją, aplikacje muszą używać schematu, na podstawie nazwy pod jego kontrolą. Można to osiągnąć za pomocą nazwy identyfikatora pakietu w systemach iOS i nazwy pakietu w systemie Android, a następnie cofania umożliwia podejmowanie schemat adresu URL. Jednak niektórzy dostawcy tożsamości, takich jak Google, przypisać identyfikatory klienta na podstawie nazw domen, które są następnie wycofać i używane jako schemat adresu URL. Na przykład, jeśli Google tworzy identyfikator klienta `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`, schemat adresu URL będzie `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`. Należy pamiętać, że tylko jeden `/` może następować po elemencie składnik schematu. Dlatego jest kompletny przykładowy adres URL przekierowania wykorzystujących niestandardowy schemat adresu URL `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`.

Gdy przeglądarka sieci web odbiera odpowiedź od dostawcy tożsamości, który zawiera niestandardowy schemat adresu URL, próbuje załadować adresu URL, który zakończy się niepowodzeniem. Zamiast tego niestandardowego schemat adresu URL jest zgłaszany do systemu operacyjnego przez podnoszenie zdarzenia. System operacyjny wyszukuje następnie schematów zarejestrowanych, a jeśli został znaleziony, system operacyjny spowoduje uruchomienie aplikacji, który zarejestrował to schemat i wysyłać je adres URL przekierowania.

Rejestrowanie niestandardowe schemat adresu URL przy użyciu systemu operacyjnego i obsługi systemu przy użyciu mechanizmu jest specyficzne dla poszczególnych platform.

#### <a name="ios"></a>iOS

W systemach iOS, niestandardowy schemat adresu URL jest zarejestrowany w **Info.plist**, jak pokazano na poniższym zrzucie ekranu:

![](oauth-images/info-plist.png "Rejestracja schemat adresu URL")

**Identyfikator** wartość może być dowolna i **roli** musi być równa wartości **podglądu**. **Schematy adresów Url** wartość, która rozpoczyna się od `com.googleusercontent.apps`, można uzyskać identyfikator klienta dla systemu iOS dla projektu na [Konsola interfejsu API Google](http://console.developers.google.com).

Po ukończeniu żądania autoryzacji dostawcy tożsamości, zostanie przekierowany do adresu URL przekierowania aplikacji. Ponieważ adres URL używa schematem niestandardowym powoduje uruchomienie aplikacji dla systemu iOS, przekazując w adresie URL jako parametru uruchamiania, gdy jest przetwarzany przez `OpenUrl` zastąpienia aplikacji `AppDelegate` klasy, który jest pokazany w poniższym przykładzie kodu:

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    // Convert NSUrl to Uri
    var uri = new Uri(url.AbsoluteString);

    // Load redirectUrl page
    AuthenticationState.Authenticator.OnPageLoading(uri);

    return true;
}
```

`OpenUrl` Metoda konwertuje odebranych adres URL z `NSUrl` do .NET `Uri`, przed rozpoczęciem przetwarzania adresu URL przekierowania z `OnPageLoading` metodę publiczną `OAuth2Authenticator` obiektu. Powoduje to Xamarin.Auth, aby zamknąć kartę przeglądarki sieci web i przeanalizować odebranego danych OAuth.

#### <a name="android"></a>Android

W systemie Android niestandardowy schemat adresu URL jest zarejestrowany, określając [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) atrybutu na `Activity` która będzie obsługiwać schemat. Po ukończeniu żądania autoryzacji dostawcy tożsamości, zostanie przekierowany do adresu URL przekierowania aplikacji. Ponieważ adres URL używa schematem niestandardowym powoduje uruchamianie aplikacji systemu Android, przekazując w adresie URL jako parametru uruchamiania, gdy jest przetwarzany przez `OnCreate` metody `Activity` zarejestrowana w celu obsługi niestandardowych schemat adresu URL. Poniższy przykład kodu pokazuje klasy z przykładowej aplikacji, który obsługuje niestandardowe schemat adresu URL:

```csharp
[Activity(Label = "CustomUrlSchemeInterceptorActivity", NoHistory = true, LaunchMode = LaunchMode.SingleTop )]
[IntentFilter(
    new[] { Intent.ActionView },
    Categories = new [] { Intent.CategoryDefault, Intent.CategoryBrowsable },
    DataSchemes = new [] { "<insert custom URL here>" },
    DataPath = "/oauth2redirect")]
public class CustomUrlSchemeInterceptorActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        // Convert Android.Net.Url to Uri
        var uri = new Uri(Intent.Data.ToString());

        // Load redirectUrl page
        AuthenticationState.Authenticator.OnPageLoading(uri);

        Finish();
    }
}
```

`DataSchemes` Właściwość [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) musi być ustawione na identyfikator klienta odwróconej, które są uzyskiwane z identyfikator klienta dla systemu Android w projekcie na [Konsola interfejsu API Google](http://console.developers.google.com).

`OnCreate` Metoda konwertuje odebranych adres URL z `Android.Net.Url` do .NET `Uri`, przed rozpoczęciem przetwarzania adresu URL przekierowania z `OnPageLoading` metodę publiczną `OAuth2Authenticator` obiektu. Powoduje to Xamarin.Auth zamknąć kartę przeglądarki sieci web i przeanalizować odebrane dane OAuth.

> [!IMPORTANT]
> W systemie Android używa Xamarin.Auth `CustomTabs` interfejsu API w celu komunikowania się z przeglądarki sieci web i systemu operacyjnego. Jednak go ma nie masz gwarancję, że `CustomTabs` zgodnej przeglądarki zostanie zainstalowana na urządzeniu użytkownika.

### <a name="examining-the-oauth-response"></a>Badanie w odpowiedzi OAuth

Po analizie odebranych danych uwierzytelniania OAuth, zgłosi Xamarin.Auth `OAuth2Authenticator.Completed` zdarzeń. W procedurze obsługi zdarzeń dla tego zdarzenia `AuthenticatorCompletedEventArgs.IsAuthenticated` właściwość może służyć ustaleniu, czy uwierzytelnianie zakończyło się pomyślnie, jak pokazano w poniższym przykładzie kodu:

```csharp
async void OnAuthCompleted(object sender, AuthenticatorCompletedEventArgs e)
{
  ...
  if (e.IsAuthenticated)
  {
    ...
  }
}
```

Dane zbierane z pomyślnym uwierzytelnieniu są dostępne w `AuthenticatorCompletedEventArgs.Account` właściwości. W tym token dostępu, który może być używany do podpisywania żądań dotyczących danych do interfejsu API udostępnionych przez dostawcę tożsamości.

### <a name="making-requests-for-data"></a>Tworzenie żądań dotyczących danych

Po aplikacja uzyskuje token dostępu, jest używany, aby zgłosić wniosek o `https://www.googleapis.com/oauth2/v2/userinfo` interfejsu API, aby żądania użytkowników typu basic danych od dostawcy tożsamości. Żądania z firmy Xamarin.Auth `OAuth2Request` klasy, która reprezentuje żądania, który jest uwierzytelniany przy użyciu konta, pobierane `OAuth2Authenticator` wystąpienia, jak pokazano w poniższym przykładzie kodu:

```csharp
// UserInfoUrl = https://www.googleapis.com/oauth2/v2/userinfo
var request = new OAuth2Request ("GET", new Uri (Constants.UserInfoUrl), null, e.Account);
var response = await request.GetResponseAsync ();
if (response != null)
{
  string userJson = response.GetResponseText ();
  var user = JsonConvert.DeserializeObject<User> (userJson);
}
```

A także metody HTTP i adres URL interfejsu API `OAuth2Request` określa również wystąpienia `Account` wystąpienia, które zawiera token dostępu, który podpisuje żądanie do adresu URL określonego przez `Constants.UserInfoUrl` właściwości. Dostawca tożsamości zwraca dane podstawowe użytkownika w postaci odpowiedź JSON, w tym nazwę użytkownika i adres e-mail, pod warunkiem, że rozpoznaje token dostępu jako nieprawidłowy. Odpowiedź w formacie JSON jest odczytywana i przeprowadzona deserializacja `user` zmiennej.

Aby uzyskać więcej informacji, zobacz [wywoływanie interfejsu API Google](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) w portalu deweloperów Google.

### <a name="storing-and-retrieving-account-information-on-devices"></a>Przechowywanie i pobieranie informacji o koncie na urządzeniach

Xamarin.Auth x bezpiecznie przechowa Twoje `Account` przechowywanie obiektów w ramach konta, tak, aby aplikacje nie zawsze być konieczność ponownego uwierzytelnienia użytkowników. `AccountStore` Klasy jest odpowiedzialny za przechowywanie informacji o koncie i jest uzupełniana przez usługi łańcucha kluczy w systemie iOS, a `KeyStore` klasy w systemie Android.

Poniższy kod przedstawia przykład sposobu `Account` bezpiecznie zapisaniu obiektu:

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

Zapisane kont odbywa się niepowtarzalna identyfikacja przy użyciu klucza składa się z konta `Username` właściwość i identyfikator usługi, który jest ciągiem, który jest używany podczas pobierania kont z konta magazynu. Jeśli `Account` został wcześniej zapisany, wywołanie `Save` metoda ponownie spowoduje zastąpienie.

`Account` obiekty dla usługi może być pobierany przez wywołanie `FindAccountsForService` metodzie, jak pokazano w poniższym przykładzie kodu:

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

`FindAccountsForService` Metoda zwraca `IEnumerable` zbiór `Account` obiekty z pierwszego elementu w kolekcji zostanie ustawiony jako konto dopasowany.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak używać Xamarin.Auth do zarządzania procesem uwierzytelniania w aplikacji platformy Xamarin.Forms. Udostępnia Xamarin.Auth `OAuth2Authenticator` i `OAuth2Request` klas, które są używane przez aplikacje Xamarin.Forms korzystanie z dostawców tożsamości, takich jak Google, Microsoft, Facebook i Twitter.


## <a name="related-links"></a>Linki pokrewne

- [OAuthNativeFlow (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/OAuthNativeFlow/)
- [W przypadku aplikacji natywnych, OAuth 2.0](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Za pomocą OAuth 2.0 dostęp do interfejsów API firmy Google](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
