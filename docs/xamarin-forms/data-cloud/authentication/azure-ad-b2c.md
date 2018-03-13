---
title: "Uwierzytelnianie użytkowników w usłudze Azure Active Directory B2C"
description: "Usługa Azure Active Directory B2C jest chmury rozwiązania do zarządzania tożsamościami internetowych i aplikacji dla urządzeń przenośnych. W tym artykule pokazano, jak zintegrować Zarządzanie tożsamościami konsumentów aplikacjami mobilnymi za pomocą biblioteki uwierzytelniania firmy Microsoft i Azure Active Directory B2C."
ms.topic: article
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 5d64c7c1dbc502acd3876c2442f9bae1c46eeb74
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Uwierzytelnianie użytkowników w usłudze Azure Active Directory B2C

_Usługa Azure Active Directory B2C jest chmury rozwiązania do zarządzania tożsamościami internetowych i aplikacji dla urządzeń przenośnych. W tym artykule pokazano, jak zintegrować Zarządzanie tożsamościami konsumentów aplikacjami mobilnymi za pomocą biblioteki uwierzytelniania firmy Microsoft i Azure Active Directory B2C._

![](~/media/shared/preview.png "Ten interfejs API jest obecnie wersji wstępnej")

> [!NOTE]
> [Biblioteki uwierzytelniania](https://www.nuget.org/packages/Microsoft.Identity.Client) jest wciąż w wersji zapoznawczej, ale są odpowiednie do użycia w środowisku produkcyjnym. Jednak mogą być istotne zmiany interfejsu API, format wewnętrznej pamięci podręcznej i innych mechanizmów biblioteki, która może wpływać na działanie aplikacji.

## <a name="overview"></a>Omówienie

Usługa Azure Active Directory B2C jest usługa identity management w przypadku aplikacji dla użytkownika umożliwiający konsumentów do logowania do aplikacji przez:

- Przy użyciu istniejących kont społecznościowych (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Tworzenie nowego poświadczenia (adres e-mail i hasło lub nazwa użytkownika i hasło). Te poświadczenia są określane jako *lokalnego* kont.

Proces zintegrować usługę Azure Active Directory B2C tożsamość zarządzania aplikacjami mobilnymi jest następujący:

1. Tworzenie dzierżawy usługi Azure Active Directory B2C. Aby uzyskać więcej informacji, zobacz [tworzenie dzierżawy usługi Azure Active Directory B2C w portalu Azure](/azure/active-directory-b2c/active-directory-b2c-get-started/).
1. Zarejestrować aplikację mobilną z dzierżawą usługi Azure Active Directory B2C. Proces rejestracji przypisuje **identyfikator aplikacji** aplikacji, który unikatowo identyfikuje i **adresem URL przekierowania** który może służyć do kierowania odpowiedzi z powrotem do aplikacji. Aby uzyskać więcej informacji, zobacz [usługi Azure Active Directory B2C: Rejestrowanie aplikacji](/azure/active-directory-b2c/active-directory-b2c-app-registration/).
1. Tworzenie zasad rejestracji i logowania. Te zasady zostaną zdefiniowane procesy, które konsumentów będzie przejście przez podczas rejestracji i logowania oraz określa zawartość tokenów aplikacji zostanie wyświetlony na pomyślne rejestracji lub logowania. Aby uzyskać więcej informacji, zobacz [usługi Azure Active Directory B2C: wbudowane zasady](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).
1. Użyj [biblioteki uwierzytelniania](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) w Twojej aplikacji mobilnej, aby zainicjować przepływ uwierzytelniania z dzierżawą usługi Azure Active Directory B2C.

> [!NOTE]
> A także włączenie zarządzania tożsamościami w usłudze Azure Active Directory B2C do aplikacji dla urządzeń przenośnych, MSAL można również zintegrować Zarządzanie tożsamościami w usłudze Azure Active Directory aplikacji dla urządzeń przenośnych. Można to zrobić przez zarejestrowanie aplikacjami mobilnymi w usłudze Azure Active Directory w [portalu rejestracji aplikacji](https://apps.dev.microsoft.com/). Proces rejestracji przypisuje **identyfikator aplikacji** który unikatowo identyfikuje aplikacji, które powinny być określone przy użyciu MSAL. Aby uzyskać więcej informacji, zobacz [jak zarejestrować aplikację z punktem końcowym v2.0](/azure/active-directory/develop/active-directory-v2-app-registration/), i [uwierzytelniania Your Mobile Apps przy użyciu uwierzytelniania biblioteki Microsoft](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) na blogu Xamarin.

MSAL używa przeglądarki sieci web na urządzeniu w celu przeprowadzenia uwierzytelniania. Poprawia to użyteczność aplikacji, ponieważ użytkownicy potrzebują tylko do logowania po na urządzenie, przepływy poprawy logowania i autoryzacji w aplikacji. Przeglądarki urządzenia także udostępnia lepsze zabezpieczenia. Po zakończeniu procesu uwierzytelniania użytkownika formant powróci do aplikacji na karcie przeglądarki sieci web. Jest to osiągane przez zarejestrowanie schemat niestandardowy adres URL jest zwracana z procesu uwierzytelniania, a następnie wykrywania i obsługi niestandardowy adres URL po przesłaniu jej adresu URL przekierowania. Aby uzyskać więcej informacji o wybieraniu schemat niestandardowy adres URL, zobacz [Wybieranie na identyfikator URI przekierowania aplikacji natywnej](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/).

> [!NOTE]
> Mechanizm rejestrowania niestandardowego schemat adresu URL za pomocą systemu operacyjnego i obsługi systemu jest specyficzne dla poszczególnych platform.

Określa każdego żądania, które są wysyłane do dzierżawy usługi Azure Active Directory B2C *zasad*. Zasady opisano funkcje tożsamości konsumentów przykład rejestracji lub logowania. Na przykład zasad rejestracji umożliwia zachowanie dzierżawy usługi Azure Active Directory B2C można skonfigurować za pomocą następujących ustawień:

- Typy kont używanych przez konsumentów do logowania do aplikacji.
- Dane mają zostać pobrane od konsumenta podczas tworzenia konta.
- Uwierzytelnianie wieloskładnikowe.
- Zawartość strony tworzenia konta.
- Token oświadczeń odbieranych w aplikacji mobilnej do zasad zostało wykonane.

Dzierżawy usługi Azure Active Directory może zawierać wiele zasad o różnych typach, które mogą być następnie używane w aplikacji zgodnie z potrzebami. Ponadto zasady mogą być ponownie używane w aplikacjach, co umożliwia definiowanie i modyfikowanie środowiska tożsamości użytkownika bez modyfikowania kodu. Aby uzyskać więcej informacji na temat zasad, zobacz [usługi Azure Active Directory B2C: wbudowane zasady](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

## <a name="setup"></a>Konfiguracja

Biblioteka NuGet biblioteki uwierzytelniania firmy Microsoft (MSAL) musi zostać dodany do projektu przenośnej biblioteki klasy (PCL) i platforma projektów w rozwiązaniu platformy Xamarin.Forms. Poniższe sekcje zawierają dodatkowe instrukcje dotyczące konfiguracji związane z używaniem MSAL do komunikowania się z dzierżawy usługi Azure Active Directory B2C z aplikacji mobilnej.

### <a name="portable-class-library"></a>Biblioteka klas przenośnych

MSAL nie obsługuje systemu Windows Phone 8.1, a więc PCLs używające MSAL będzie konieczne usunięcie tego obiektu docelowego. Można to zrobić przez PCLs do użycia Profile7 przekierowania. Aby uzyskać więcej informacji o PCLs, zobacz [wprowadzenie do bibliotek klas przenośnych](~/cross-platform/app-fundamentals/pcl.md).

### <a name="ios"></a>iOS

W systemach iOS, niestandardowe schemat adresu URL, który został zarejestrowany z usługi Azure Active Directory B2C musi być zarejestrowana w **Info.plist**, jak pokazano na poniższym zrzucie ekranu:

![](azure-ad-b2c-images/customurl-ios.png "Rejestrowanie niestandardowe schemat adresów URL w systemie iOS")

Po zakończeniu żądania autoryzacji usługi Azure Active Directory B2C przekierowania do adresu URL przekierowania w zarejestrowany. Ponieważ adres URL używa schematu niestandardowego powoduje uruchamianie aplikacji mobilnej systemu iOS, przekazywanie w adresie URL jako parametru uruchamiania, gdzie jest przetwarzany przez `OpenUrl` zastępowania aplikacji `AppDelegate` klasy, która jest wyświetlana w poniższym kodzie przykład:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        ...
        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
            return true;
        }
    }
}
```

Kod w `OpenURL` metoda gwarantuje, że formant zwraca MSAL po zakończeniu interaktywnego część przepływu pracy uwierzytelniania.

### <a name="android"></a>Android

W systemie Android, niestandardowe schemat adresu URL, który został zarejestrowany z usługi Azure Active Directory B2C musi być zarejestrowana w **AndroidManifest.xml**, dodając `<activity>` element wewnątrz istniejącego `<application>` elementu. `<activity>` Określa element `IntentFilter` na `Activity` obsługuje schemat i przedstawiono w poniższym przykładzie:

```xml
<application ...>
  <activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
      <action android:name="android.intent.action.VIEW" />
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
      <data android:scheme="INSERT_URL_SCHEME_HERE" android:host="auth" />
    </intent-filter>
  </activity>
</application>
```

Po zakończeniu żądania autoryzacji usługi Azure Active Directory B2C przekierowania do adresu URL przekierowania w zarejestrowany. Ponieważ adres URL używa schematu niestandardowego powoduje uruchamianie aplikacji mobilnej systemu Android, przekazywanie w adresie URL jako parametru uruchamiania, gdzie jest przetwarzany przez `microsoft.identity.client.BrowserTabActivity`. Należy pamiętać, że `data android:scheme` musi być ustawiona właściwość niestandardowych schemat adresu URL, która jest zarejestrowana w usłudze aplikacji Azure Active Directory B2C.

Ponadto `MainActivity` muszą zostać zmodyfikowane klasy, jak pokazano w poniższym przykładzie:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            global::Xamarin.Forms.Forms.Init(this, bundle);
            Microsoft.WindowsAzure.MobileServices.CurrentPlatform.Init();
            LoadApplication(new App());
            App.UiParent = new UIParent(this);
        }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
        }
    }
}

```

`OnCreate` Metody jest modyfikowany przez przypisanie `UIParent` wystąpienie do `App.UiParent` właściwości. Dzięki temu, że przepływ uwierzytelniania występuje w kontekście bieżącego działania.

Kod w `OnActivityResult` metoda gwarantuje, że formant zwraca MSAL po zakończeniu interaktywnego część przepływu pracy uwierzytelniania.

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Na platformie Windows Universal nie dodatkowe ustawienia musisz wybrać MSAL.

## <a name="initialization"></a>Inicjalizacja

Biblioteka uwierzytelniania firmy Microsoft używa członkami `PublicClientApplication` klasy do inicjowania przepływu pracy uwierzytelniania. Przykładowa aplikacja deklaruje i inicjuje `public` właściwości tego typu, o nazwie `ADB2CClient`w `AuthenticationProvider` klasy. Poniższy przykład kodu pokazuje, jak ta właściwość jest inicjowana:

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

W przypadku aplikacji mobilnej został zarejestrowany z dzierżawą usługi Azure Active Directory B2C, proces rejestracji przypisane **identyfikator aplikacji**. Ten identyfikator musi być określony w `PublicClientApplication` konstruktora, wraz z `Authority` stała, która składa się z podstawowego adresu URL i zasady usługi Azure Active Directory B2C do wykonania.

## <a name="signing-in"></a>Logowanie

Ekran logowania w przykładowej aplikacji jest wyświetlany na poniższych zrzutach ekranu:

![](azure-ad-b2c-images/login.png "Strony logowania")

Logowania z dostawców tożsamości społecznościowych lub przy użyciu konta lokalnego, są dozwolone. Microsoft, Google i Facebook, jak pokazano powyżej, są używane jako dostawcy tożsamości społecznościowych innych dostawców tożsamości można także.

Poniższy przykład kodu pokazuje sposób wywoływania procesu logowania:

```csharp
using Microsoft.Identity.Client;

public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);
    ...
}

```

`AcquireTokenAsync` Metoda spowoduje uruchomienie przeglądarki sieci web na urządzeniu i wyświetla opcje uwierzytelniania zdefiniowane w zasadach usługi Azure Active Directory B2C, określonej przez zasady, do których odwołuje się za pośrednictwem `Constants.Authority` stałej. Ta zasada definiuje procesy, które konsumentów będzie przejście przez podczas rejestracji i logowania i oświadczenia się, że aplikacja otrzyma pomyślnie rejestracji lub logowania.

Wynik `AcquireTokenAsync` jest wywołanie metody `AuthenticationResult` wystąpienia. Jeśli uwierzytelnianie się powiedzie, `AuthenticationResult` wystąpienie będzie zawierać token tożsamości, które będą buforowane lokalnie. Jeśli uwierzytelnianie zakończy się niepowodzeniem, `AuthenticationResult` wystąpienie będzie zawierać dane, które wskazuje na to, dlaczego uwierzytelnianie nie powiodło się.

W przykładowej aplikacji, jeśli uwierzytelnianie się powiedzie `TodoList` jest przejście strony.

## <a name="silent-re-authentication"></a>Ponowne uwierzytelnianie w trybie dyskretnym

Gdy `LoginPage` w próbce. aplikacja zostanie wyświetlona, w próby pobrania tokenu użytkownika bez wyświetlania interfejsu użytkownika uwierzytelniania. Jest to osiągane z `AcquireTokenSilentAsync` metody, jak pokazano w poniższym przykładzie:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult;

    if (useSilent)
    {
        authenticationResult = await ADB2CClient.AcquireTokenSilentAsync(
            Constants.Scopes,
            GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
            Constants.Authority,
            false);
    }
    ...
}
```

`AcquireTokenSilentAsync` Metoda próbuje pobrać tokenu użytkownika z pamięci podręcznej, bez konieczności użytkownika do logowania. Obsługuje scenariusz, gdzie token odpowiedniego już może znajdować się w pamięci podręcznej z poprzedniej sesji. Jeśli próba uzyskania tokenu zakończy się pomyślnie, `TodoList` jest przejście strony. Jeśli próba uzyskania tokenu zakończy się niepowodzeniem, nic się nie dzieje, a użytkownik będzie miał możliwość inicjowania nowego przepływu pracy uwierzytelniania.

## <a name="signing-out"></a>Wylogowywanie

Poniższy przykład kodu pokazuje sposób wywoływania proces logowania:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

Czyści wszystkie tokeny uwierzytelniania z lokalnej pamięci podręcznej.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób zintegrować Zarządzanie tożsamościami konsumentów aplikacjami mobilnymi za pomocą biblioteki uwierzytelniania firmy Microsoft (MSAL) i Azure Active Directory B2C. Usługa Azure Active Directory B2C jest chmury rozwiązania do zarządzania tożsamościami internetowych i aplikacji dla urządzeń przenośnych.


## <a name="related-links"></a>Linki pokrewne

- [AzureADB2CAuth (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Biblioteka uwierzytelniania firmy Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client)
