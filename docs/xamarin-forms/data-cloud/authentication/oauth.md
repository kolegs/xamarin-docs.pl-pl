---
title: "Uwierzytelnianie użytkowników przy użyciu dostawcy tożsamości"
description: "Xamarin.Auth jest SDK i platform do uwierzytelniania użytkowników i przechowywania ich kont. Obejmuje on wystawców uwierzytelnienia OAuth, które zapewniają obsługę służący do konsumowania dostawców tożsamości, takich jak Google, Microsoft, Facebook i Twitter. W tym artykule opisano sposób użycia Xamarin.Auth do zarządzania procesem uwierzytelniania w aplikacji platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: ff0403fedf75ab22986f5fc83d16db3dbf8d92b6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="authenticating-users-with-an-identity-provider"></a>Uwierzytelnianie użytkowników przy użyciu dostawcy tożsamości

_Xamarin.Auth jest SDK i platform do uwierzytelniania użytkowników i przechowywania ich kont. Obejmuje on wystawców uwierzytelnienia OAuth, które zapewniają obsługę służący do konsumowania dostawców tożsamości, takich jak Google, Microsoft, Facebook i Twitter. W tym artykule opisano sposób użycia Xamarin.Auth do zarządzania procesem uwierzytelniania w aplikacji platformy Xamarin.Forms._

OAuth jest standardem otwartym do uwierzytelniania i umożliwia właścicielowi zasobów powiadomienia dostawcy zasobów, czy należy udzielić zezwolenia osobom trzecim na dostęp do informacji o ich bez udostępniania tożsamości właścicieli zasobu. Na przykład spowoduje włączenie użytkownika, aby powiadomić dostawcy tożsamości (na przykład Google, Microsoft, Facebook i Twitter), czy można udzielić uprawnienia do aplikacji dostępu do danych, bez udostępniania tożsamość użytkownika. Jest często używany jako rozwiązaniem dla użytkowników do logowania do witryn internetowych i aplikacji przy użyciu dostawcy tożsamości, ale bez narażania swoje hasło do witryny sieci Web lub aplikacji.

Ogólne omówienie przebieg uwierzytelniania w przypadku uzyskiwania dostępu do dostawcy tożsamości OAuth jest następujący:

1. Aplikacja przechodzi w przeglądarce adres URL dostawcy tożsamości.
1. Dostawca tożsamości obsługuje uwierzytelnianie użytkowników i zwraca kod autoryzacji do aplikacji.
1. Aplikacja wymienia kodu autoryzacji do tokena dostępu przez dostawcę tożsamości.
1. Aplikacja używa tokenu dostępu dostępu do interfejsów API dla dostawcy tożsamości, takie jak interfejsu API dla żądania danych użytkownika podstawowego.

Przykładowa aplikacja przedstawiono sposób użycia Xamarin.Auth do zaimplementowania przepływ uwierzytelniania macierzystego przed Google. Gdy Google jest używany jako dostawca tożsamości, w tym temacie, podejście jednakowo stosuje się do innych dostawców tożsamości. Aby uzyskać więcej informacji na temat uwierzytelniania przy użyciu punkt końcowy protokołu OAuth 2.0 firmy Google, zobacz [przy użyciu OAuth2.0 do interfejsów API firmy Google dostępu](https://developers.google.com/identity/protocols/OAuth2) w witrynie sieci Web firmy Google.

> [!NOTE]
> W systemie iOS 9 i większości aplikacji zabezpieczeń transportu (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych. Ponieważ ATS jest domyślnie włączone w aplikacje dla systemu iOS 9, wszystkie połączenia będą podlegać ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.
> ATS można można korzystać z, jeśli nie jest możliwe użycie `HTTPS` protokołu i zabezpieczania komunikacji dla zasobów w Internecie. Można to osiągnąć przez zaktualizowanie aplikacji **Info.plist** pliku. Aby uzyskać więcej informacji, zobacz [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md).

## <a name="using-xamarinauth-to-authenticate-users"></a>Uwierzytelnianie użytkowników przy użyciu Xamarin.Auth

Xamarin.Auth obsługuje dwa podejścia do aplikacjom komunikowanie się z dostawcą tożsamości punktu końcowego autoryzacji:

1. Używanie widoku sieci web osadzonych. Podczas było to popularną praktyką, nie zaleca się z następujących powodów:

    - Aplikacji, która obsługuje widoku sieci web można uzyskać dostępu do poświadczeń pełnego uwierzytelniania użytkownika, nie tylko udzielania autoryzacji OAuth zamierzony dla aplikacji. Narusza zasadę najmniejszych uprawnień, jako aplikacja ma dostęp do bardziej zaawansowanych poświadczenia niż go wymaga potencjalnie zwiększa podatność na aplikację.
    - Aplikacja hosta można przechwycić nazwy użytkowników i hasła, automatycznie przesyłania formularzy i obejścia zgodę użytkownika i skopiuj pliki cookie z sesji i ich używać do wykonywania akcji uwierzytelniony jako użytkownik.
    - Widoki sieci web osadzone nie udostępniaj stanu uwierzytelniania z innych aplikacji lub przeglądarki sieci web urządzenia, gdyż użytkownik musi zalogować przy każdym żądaniu autoryzacji, która jest uznawana za środowisko użytkownika mniejsza niż.
    - Niektóre punkty końcowe autoryzacji wykonać czynności, aby wykrywać i blokować żądania autoryzacji, które pochodzą od widoki sieci web.

1. Za pomocą przeglądarki sieci web na urządzeniu, czyli Zalecanym podejściem. Za pomocą przeglądarki urządzenia dla żądania OAuth poprawia działanie aplikacji, ponieważ użytkownicy potrzebują tylko do logowania do dostawcy tożsamości raz na urządzeniu, poprawy szybkości konwersji przepływów logowania i autoryzacji w aplikacji. W przeglądarce urządzenia również zapewnia wyższy poziom zabezpieczeń, jak aplikacje są w stanie sprawdzić i modyfikować zawartość w widoku sieci web, ale nie zawartość wyświetlaną w przeglądarce. To podejście przyjęte w tej artykułu i przykładowej aplikacji.

Ogólne omówienie sposobu Przykładowa aplikacja korzysta z Xamarin.Auth do uwierzytelniania użytkowników i pobierania ich podstawowe dane przedstawiono na poniższym diagramie:

![](oauth-images/google-auth.png "Przy użyciu Xamarin.Auth do uwierzytelniania w usłudze Google")

Aplikacja zgłasza żądanie uwierzytelniania przy użyciu usługi Google `OAuth2Authenticator` klasy. Odpowiedź uwierzytelnienia jest zwracany, gdy użytkownik pomyślnie uwierzytelnił się z serwisem Google za pośrednictwem ich strony logowania, który zawiera token dostępu. Następnie aplikacja wysyła żądanie do Google dla danych podstawowych użytkownika, przy użyciu `OAuth2Request` klasy przy użyciu tokenu dostępu, które są zawarte w żądaniu.

### <a name="setup"></a>Konfiguracja

Integracja Google, logowanie przy użyciu aplikacji platformy Xamarin.Forms musi zostać utworzony projekt konsoli interfejsu API firmy Google. Można to zrobić w następujący sposób:

1. Przejdź do [konsoli interfejsu API firmy Google](http://console.developers.google.com) witryny sieci Web i zaloguj się przy użyciu poświadczeń konta Google.
1. Z projektu listy rozwijanej wybierz istniejący projekt lub Utwórz nową.
1. Na pasku bocznym w obszarze "Menedżer interfejsu API" Wybierz **poświadczenia**, a następnie wybierz pozycję **kartę ekranu zgoda OAuth**. Wybierz **adres E-mail**, określ **nazwa produktu pokazywana użytkownikom**i naciśnij klawisz **zapisać**.
1. W **poświadczenia** wybierz opcję **Utwórz poświadczenia** listy rozwijanej liście, a następnie wybierz pozycję **identyfikator klienta OAuth**.
1. W obszarze **typu aplikacji**, wybierz platformę aplikacji mobilnej, które będą działały na (**iOS** lub **Android**).
1. Wypełnij wymagane szczegóły i wybierz **Utwórz** przycisku.

> [!NOTE]
> Identyfikator klienta umożliwia aplikacji dostęp do włączone interfejsy API Google, a dla aplikacji dla urządzeń przenośnych jest unikatowy dla pojedynczą platformę. W związku z tym **identyfikator klienta OAuth** powinien zostać utworzony dla każdej platformy, która będzie używana logowania w witrynie Google.

Po wykonaniu tych kroków, Xamarin.Auth może służyć do zainicjowania przepływ uwierzytelniania OAuth2 z serwisem Google.

### <a name="creating-and-configuring-an-authenticator"></a>Tworzenie i konfigurowanie uwierzytelniania

W Xamarin.Auth `OAuth2Authenticator` klasy jest odpowiedzialny za obsługę przepływ uwierzytelniania OAuth. Poniższy przykładowy kod przedstawia tworzenie wystąpienia elementu `OAuth2Authenticator` klasy podczas przeprowadzania uwierzytelniania za pomocą przeglądarki sieci web na urządzeniu:

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

- **Identyfikator klienta** — identyfikuje klienta, który wysłał żądanie i mogą być pobierane z projektu w [konsoli interfejsu API firmy Google](http://console.developers.google.com).
- **Klucz tajny klienta** — powinna to być `null` lub `string.Empty`.
- **Zakres** — identyfikuje dostępu do interfejsu API, które są wymagane przez aplikację, a wartość informuje ekranu zgody, który jest pokazywana użytkownikowi. Aby uzyskać więcej informacji na temat zakresów, zobacz [żądania autoryzacji interfejsu API](https://developers.google.com/+/web/api/rest/oauth) w witrynie sieci Web firmy Google.
- **Adres URL autoryzacji** — identyfikuje adres URL, gdzie otrzymany kod autoryzacji.
- **Adres URL przekierowania** — identyfikuje adres URL, do którego zostanie wysłana odpowiedź. Wartość tego parametru musi być jedną z wartości, w których zgodna **poświadczenia** kartę dla projektu w [konsoli deweloperów Google](https://console.developers.google.com/).
- **Adres AccessToken Url** — identyfikuje adres URL używany do żądania tokenów dostępu po uzyskaniu kod autoryzacji.
- **GetUserNameAsync Func** — jako opcjonalny `Func` które będą używane do asynchronicznego pobieranie nazwy użytkownika konta po został pomyślnie uwierzytelniony.
- **Użyj natywnego interfejsu użytkownika** — `boolean` wartość wskazującą, czy ma być używany do wykonania żądania uwierzytelniania przeglądarki sieci web na urządzeniu.

### <a name="setup-authentication-event-handlers"></a>Konfiguracja obsługi zdarzeń uwierzytelniania

Przed rozpoczęciem prezentacji interfejsu użytkownika programu obsługi zdarzeń dla `OAuth2Authenticator.Completed` zdarzeń muszą być zarejestrowane, jak pokazano w poniższym przykładzie:

```csharp
authenticator.Completed += OnAuthCompleted;
```

To zdarzenie będą uruchamiane, gdy użytkownik pomyślnie uwierzytelnia lub anuluje przy logowaniu.

Opcjonalnie, program obsługi zdarzeń dla `OAuth2Authenticator.Error` również mogą być rejestrowane zdarzenia.

### <a name="presenting-the-sign-in-user-interface"></a>Umożliwienie korzystania z interfejsu użytkownika logowania

Użytkownik może przedstawiane interfejsu użytkownika logowania za pomocą Xamarin.Auth prezenterze logowania, które muszą zostać zainicjowane w każdym projekcie platformy. Poniższy przykład kodu pokazuje, jak zainicjować prezenterze logowania, w `AppDelegate` klasy w projekcie systemu iOS:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

Poniższy przykład kodu pokazuje, jak zainicjować prezenterze logowania, w `MainActivity` klasy w projekcie systemu Android:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

Projekt przenośnej biblioteki klasy (PCL) może następnie wywołać prezenterze logowania w następujący sposób:

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

Należy pamiętać, że argument `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` jest metoda `OAuth2Authenticator` wystąpienia. Gdy `Login` wywołania metody, interfejs użytkownika logowania jest wyświetlana na karcie z przeglądarki sieci web urządzenia, które przedstawiono w następujących zrzuty ekranu:

![](oauth-images/login.png "Google Sign-In")

### <a name="processing-the-redirect-url"></a>Adres URL przekierowania przetwarzania

Po zakończeniu procesu uwierzytelniania użytkownika formant powróci do aplikacji na karcie przeglądarki sieci web. Jest to osiągane przez zarejestrowanie schemat niestandardowy adres URL jest zwracana z procesu uwierzytelniania, a następnie wykrywania i obsługi niestandardowy adres URL po przesłaniu jej adresu URL przekierowania.

W przypadku wybrania schemat niestandardowy adres URL do skojarzenia z aplikacją, aplikacje muszą używać systemu na podstawie nazwy pod jego kontrolą. Można to osiągnąć przy użyciu nazwy identyfikatora pakietu w systemach iOS i nazwy pakietu w systemie Android, a następnie cofania umożliwia podejmowanie schemat adresu URL. Jednak niektóre dostawców tożsamości, takich jak Google, przypisz identyfikatorach klientów opartych na nazwach domen, które są następnie wycofane i używane jako schemat adresu URL. Na przykład, jeśli identyfikator klienta tworzy Google `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`, będzie schemat adresu URL `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`. Należy pamiętać, że tylko jeden `/` może występować po składniku schematu. W związku z tym jest pełny przykład adres URL przekierowania przy użyciu niestandardowych schemat adresu URL `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`.

Gdy przeglądarka sieci web otrzymuje odpowiedzi od dostawcy tożsamości, która zawiera schemat niestandardowy adres URL, próbuje załadować adresu URL, który zakończy się niepowodzeniem. Zamiast tego schematu niestandardowego adresu URL został zgłoszony do systemu operacyjnego przez wywołanie zdarzenia. System operacyjny następnie wyszukuje zarejestrowane schematy, a jeśli został znaleziony, system operacyjny będzie uruchomienia aplikacji, który zarejestrował schemat i wysłać adres URL przekierowania.

Mechanizm rejestrowania niestandardowego schemat adresu URL za pomocą systemu operacyjnego i obsługi systemu jest specyficzne dla poszczególnych platform.

#### <a name="ios"></a>iOS

W systemach iOS, schemat niestandardowy adres URL jest zarejestrowany w **Info.plist**, jak pokazano na poniższym zrzucie ekranu:

![](oauth-images/info-plist.png "Rejestracja schemat adresu URL")

**Identyfikator** wartość może być dowolna i **roli** musi mieć ustawioną wartość **podglądu**. **Schematy adresów Url** wartość, która rozpoczyna się od `com.googleusercontent.apps`, można uzyskać od identyfikatora klienta dla systemu iOS dla projektu na [konsoli interfejsu API firmy Google](http://console.developers.google.com).

Po ukończeniu dostawca tożsamości żądania autoryzacji przekierowania do adresu URL przekierowania aplikacji. Ponieważ adres URL używa schematu niestandardowego powoduje uruchomienie aplikacji systemu iOS, przekazywanie w adresie URL jako parametru uruchamiania, gdzie jest przetwarzany przez `OpenUrl` zastępowania aplikacji `AppDelegate` klasy, co przedstawiono w poniższym przykładzie:

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

`OpenUrl` Metoda konwertuje adres URL odebranych z `NSUrl` do .NET `Uri`, przed rozpoczęciem przetwarzania adres URL przekierowania z `OnPageLoading` metodę publiczną `OAuth2Authenticator` obiektu. Powoduje to Xamarin.Auth, aby zamknąć kartę przeglądarki sieci web i przeanalizować odebranych danych OAuth.

#### <a name="android"></a>Android

W systemie Android, schemat niestandardowy adres URL jest zarejestrowany, określając [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) atrybutu `Activity` która będzie obsługiwać schemat. Po ukończeniu dostawca tożsamości żądania autoryzacji przekierowania do adresu URL przekierowania aplikacji. Jako adres URL używa schematu niestandardowego powoduje uruchomienie aplikacji systemu Android, przekazywanie w adresie URL jako parametru uruchamiania, gdzie jest przetwarzany przez `OnCreate` metody `Activity` zarejestrowana do obsługi niestandardowych schemat adresu URL. Poniższy przykład kodu pokazuje klasy z przykładowej aplikacji, która obsługuje niestandardowe schemat adresu URL:

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

`DataSchemes` Właściwość [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) musi być ustawiona na identyfikator klient wycofane uzyskany z identyfikatorem klienta dla systemu Android dla projektu na [konsoli interfejsu API firmy Google](http://console.developers.google.com).

`OnCreate` Metoda konwertuje adres URL odebranych z `Android.Net.Url` do .NET `Uri`, przed rozpoczęciem przetwarzania adres URL przekierowania z `OnPageLoading` metodę publiczną `OAuth2Authenticator` obiektu. Powoduje to Xamarin.Auth zamknąć kartę przeglądarki sieci web i przeanalizować odebranych danych OAuth.

> [!IMPORTANT]
> W systemie Android, używa Xamarin.Auth `CustomTabs` interfejsu API do komunikowania się z systemów operacyjnych i przeglądarek sieci web. Jednak go ma nie gwarancję, że `CustomTabs` zgodnej przeglądarki zostanie zainstalowana na urządzeniu użytkownika.

### <a name="examining-the-oauth-response"></a>Badanie odpowiedzi OAuth

Po analizie odebranych danych OAuth, zostanie podniesiony Xamarin.Auth `OAuth2Authenticator.Completed` zdarzeń. W obsłudze zdarzeń dla tego zdarzenia `AuthenticatorCompletedEventArgs.IsAuthenticated` właściwości można ustalić, czy uwierzytelnianie zakończyło się pomyślnie, jak pokazano w poniższym przykładzie:

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

Dane zbierane z pomyślnym uwierzytelnieniu są dostępne w `AuthenticatorCompletedEventArgs.Account` właściwości. W tym token dostępu, który może być używany do podpisywania żądań dotyczących danych do interfejsu API dostarczonych przez dostawcę tożsamości.

### <a name="making-requests-for-data"></a>Wstrzymywanie żądań dotyczących danych

Po aplikacja uzyskuje token dostępu, służy do przesyłania żądania do `https://www.googleapis.com/oauth2/v2/userinfo` interfejsu API do danych użytkownika podstawowego żądania przez dostawcę tożsamości. Żądań z jego Xamarin.Auth `OAuth2Request` klasy, która reprezentuje żądanie jest uwierzytelniany przy użyciu konta, pobierane z `OAuth2Authenticator` wystąpienie, jak pokazano w poniższym przykładzie:

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

A także metody HTTP i adres URL interfejsu API `OAuth2Request` określa również wystąpienia `Account` wystąpienia, które zawiera token dostępu, który podpisuje żądanie na adres URL określony przez `Constants.UserInfoUrl` właściwości. Dostawca tożsamości następnie zwraca dane użytkownika podstawowego w odpowiedzi JSON, w tym nazwę użytkownika i adres e-mail, pod warunkiem, że rozpoznawanych jako ważny token dostępu. Odpowiedź JSON jest do odczytu i przeprowadzona deserializacja `user` zmiennej.

Aby uzyskać więcej informacji, zobacz [wywoływania funkcji Google API](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) portalu deweloperów Google.

### <a name="storing-and-retrieving-account-information-on-devices"></a>Przechowywanie i pobieranie informacji o koncie na urządzeniach

Xamarin.Auth są bezpiecznie przechowywane `Account` obiektów na koncie magazynu, dzięki czemu aplikacje nie zawsze jest konieczne ponowne uwierzytelnianie użytkowników. `AccountStore` Klasy są zobowiązani do przechowywania informacji o koncie, a nie jest obsługiwana przez usługi łańcucha kluczy w systemie iOS i `KeyStore` klasy w systemie Android.

Poniższy kod przedstawia przykład sposobu `Account` bezpiecznie zapisaniu obiektu:

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

Zapisane konta są identyfikowane za pomocą klucza składa się z konta `Username` właściwości i Identyfikatora usługi, który jest ciągiem, który jest używany podczas pobierania kont z konta magazynu. Jeśli `Account` został wcześniej zapisany, wywoływania `Save` metody ponownie spowoduje zastąpienie go.

`Account` obiekty dla usługi można pobranej poprzez wywołanie `FindAccountsForService` metody, jak pokazano w poniższym przykładzie:

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

`FindAccountsForService` Metoda zwraca `IEnumerable` Kolekcja `Account` obiekty z pierwszego elementu w kolekcji jest ustawiony jako dopasowane konta.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak używać Xamarin.Auth do zarządzania procesem uwierzytelniania w aplikacji platformy Xamarin.Forms. Udostępnia Xamarin.Auth `OAuth2Authenticator` i `OAuth2Request` klasy, które są używane przez aplikacje platformy Xamarin.Forms użycie dostawców tożsamości, takich jak Google, Microsoft, Facebook i Twitter.


## <a name="related-links"></a>Linki pokrewne

- [OAuthNativeFlow (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/OAuthNativeFlow/)
- [OAuth 2.0 dla natywnych aplikacji](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Przy użyciu OAuth2.0 dostępu do interfejsów API firmy Google](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
