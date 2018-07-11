---
title: Uwierzytelnianie użytkowników za pomocą usługi Azure Active Directory B2C
description: Usługa Azure Active Directory B2C to rozwiązanie zarządzania tożsamością w chmurze dla aplikacji przeznaczonych dla klientów internetowych i mobilnych. W tym artykule przedstawiono sposób używania Biblioteka Microsoft Authentication Library i Azure Active Directory B2C do integracji do zarządzania tożsamościami klientów w aplikacji mobilnej.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: b6989782c438ec41911cc9317d9f911d6518132d
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38872706"
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Uwierzytelnianie użytkowników za pomocą usługi Azure Active Directory B2C

_Usługa Azure Active Directory B2C to rozwiązanie zarządzania tożsamością w chmurze dla aplikacji przeznaczonych dla klientów internetowych i mobilnych. W tym artykule przedstawiono sposób używania Biblioteka Microsoft Authentication Library i Azure Active Directory B2C do integracji do zarządzania tożsamościami klientów w aplikacji mobilnej._

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji wstępnej")

> [!NOTE]
> [Biblioteka Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) jest nadal w wersji zapoznawczej, ale jest odpowiedni do użytku w środowisku produkcyjnym. Jednak mogą być istotne zmiany w poleceniach interfejsu API, wewnętrznej pamięci podręcznej format i innych mechanizmów biblioteki, która może mieć wpływ na aplikację.

## <a name="overview"></a>Omówienie

Usługa Azure Active Directory B2C to usługa zarządzania tożsamościami dla aplikacji przeznaczonych dla konsumentów, umożliwia w konsumentach napisanych logowania do aplikacji przez:

- Przy użyciu istniejących kont sieci społecznościowych (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Tworzenie nowego poświadczenia (adres e-mail i hasło lub nazwa użytkownika i hasło). Te poświadczenia są określane jako *lokalnego* kont.

Proces integracji z usługą zarządzania tożsamościami usługi Azure Active Directory B2C w aplikacji mobilnej jest w następujący sposób:

1. Tworzenie dzierżawy usługi Azure Active Directory B2C. Aby uzyskać więcej informacji, zobacz [tworzenie dzierżawy usługi Azure Active Directory B2C w witrynie Azure portal](/azure/active-directory-b2c/active-directory-b2c-get-started/).
1. Zarejestruj swoją aplikację mobilną z dzierżawą usługi Azure Active Directory B2C. Proces rejestracji przypisuje **identyfikator aplikacji** , który jednoznacznie identyfikuje aplikację, a **adresu URL przekierowania** który może służyć do kierowania odpowiedzi z powrotem do aplikacji. Aby uzyskać więcej informacji, zobacz [usługi Azure Active Directory B2C: Rejestrowanie aplikacji](/azure/active-directory-b2c/active-directory-b2c-app-registration/).
1. Utwórz zasady rejestracji i logowania. Te zasady zostaną zdefiniowane procesy, przez które przejdą podczas rejestracji i logowania oraz określa również zawartość tokenów, które aplikacja otrzyma pomyślnie rejestracji lub logowania. Aby uzyskać więcej informacji, zobacz [usługi Azure Active Directory B2C: wbudowane zasady](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).
1. Użyj [Biblioteka Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) w Twojej aplikacji mobilnej, aby zainicjować przepływ pracy uwierzytelniania w dzierżawie usługi Azure Active Directory B2C.

> [!NOTE]
> Oraz integrowania zarządzania tożsamością usługi Azure Active Directory B2C do aplikacji dla urządzeń przenośnych, biblioteki MSAL można również zintegrować Zarządzanie tożsamościami usługi Azure Active Directory do aplikacji dla urządzeń przenośnych. Można to zrobić, rejestrując aplikację mobilną z usługą Azure Active Directory w [portalu rejestracji aplikacji](https://apps.dev.microsoft.com/). Proces rejestracji przypisuje **identyfikator aplikacji** , który jednoznacznie identyfikuje aplikację, należy określić w przypadku korzystania z biblioteki MSAL. Aby uzyskać więcej informacji, zobacz [jak zarejestrować aplikację za pośrednictwem punktu końcowego v2.0](/azure/active-directory/develop/active-directory-v2-app-registration/), i [uwierzytelniania usługi Mobile Apps za pomocą Biblioteka Microsoft Authentication Library](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) na blogu platformy Xamarin.

Biblioteka MSAL używa przeglądarki sieci web urządzenia do wykonywania uwierzytelniania. Zwiększa użyteczność aplikacji, ponieważ użytkownicy potrzebują tylko zalogować się po na każdym urządzeniu, przepływy poprawy logowania i autoryzacji w aplikacji. W przeglądarce na urządzeniu również zapewnia wyższy poziom zabezpieczeń. Po ukończeniu procesu uwierzytelniania użytkownika kontroli powróci do aplikacji z kartę przeglądarki sieci web. Jest to osiągane przez zarejestrowanie niestandardowy schemat adresu URL dla adresu URL przekierowania, który jest zwracany z procesu uwierzytelniania, a następnie wykrywania i obsługi niestandardowego adresu URL po wysłaniu. Aby uzyskać więcej informacji o wybieraniu niestandardowy schemat adresu URL, zobacz [Wybieranie identyfikatora URI przekierowania aplikacji natywnej](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/).

> [!NOTE]
> Rejestrowanie niestandardowe schemat adresu URL przy użyciu systemu operacyjnego i obsługi systemu przy użyciu mechanizmu jest specyficzne dla poszczególnych platform.

Każde żądanie, które są wysyłane do dzierżawy usługi Azure Active Directory B2C Określa *zasad*. Zasady opisują konsumenta środowiska tożsamości, takich jak rejestracji lub logowania. Na przykład zasady rejestracji umożliwia zachowanie dzierżawy usługi Azure Active Directory B2C można skonfigurować za pomocą następujących ustawień:

- Typy kont użytkowników, za pomocą których logowania do aplikacji.
- Dane mają być zbierane od użytkownika podczas rejestracji.
- Uwierzytelnianie wieloskładnikowe.
- Zawartość strony rejestracji.
- Token oświadczeń, odbierane w aplikacji mobilnej do zasad zostało wykonane.

Dzierżawy usługi Azure Active Directory może zawierać wiele zasad różnych typów, które następnie mogą być używane w aplikacji zgodnie z potrzebami. Ponadto zasad może zostać ponownie użyte w aplikacjach, co umożliwia definiowanie i modyfikowanie środowiska tożsamości klientów bez konieczności zmieniania kodu. Aby uzyskać więcej informacji na temat zasad, zobacz [usługi Azure Active Directory B2C: wbudowane zasady](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

## <a name="setup"></a>Konfiguracja

Biblioteka Microsoft Authentication Library (MSAL) NuGet, należy dodać do projektu biblioteki klas przenośnych (PCL) i platform projektów w rozwiązaniu Xamarin.Forms. Poniższe sekcje zawierają dodatkowe instrukcje dotyczące konfiguracji dla zastosowaniem biblioteki MSAL do komunikowania się z dzierżawą usługi Azure Active Directory B2C z aplikacji mobilnej.

### <a name="portable-class-library"></a>Biblioteka klas przenośnych

PCLs, których wartość użycia biblioteki MSAL należy przekierować go do użycia Profile7. Aby uzyskać więcej informacji na temat PCLs zobacz [wprowadzenie do biblioteki klas przenośnych](~/cross-platform/app-fundamentals/pcl.md).

### <a name="ios"></a>iOS

W systemach iOS, niestandardowy schemat adresu URL, który został zarejestrowany za pomocą usługi Azure Active Directory B2C musi być zarejestrowana w **Info.plist**, jak pokazano na poniższym zrzucie ekranu:

![](azure-ad-b2c-images/customurl-ios.png "Rejestrowanie niestandardowy schemat adresu URL, w systemie iOS")

Po ukończeniu żądanie autoryzacji usługi Azure Active Directory B2C przekierowuje adres URL przekierowania zarejestrowany. Ponieważ adres URL używa schematem niestandardowym powoduje uruchomienie aplikacji mobilnej systemu iOS, przekazując w adresie URL jako parametru uruchamiania, gdy jest przetwarzany przez `OpenUrl` zastąpienia aplikacji `AppDelegate` klasy, który jest pokazany w poniższym kodzie przykład:

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

Kod w `OpenURL` metoda gwarantuje, że formant powraca do biblioteki MSAL po zakończeniu interaktywne części przepływu pracy uwierzytelniania.

### <a name="android"></a>Android

W systemie Android niestandardowy schemat adresu URL, który został zarejestrowany za pomocą usługi Azure Active Directory B2C musi być zarejestrowana w **AndroidManifest.xml**, dodając `<activity>` element wewnątrz istniejącego `<application>` elementu. `<activity>` Element Określa `IntentFilter` na `Activity` obsługuje schemat i przedstawiono w poniższym przykładzie:

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

Po ukończeniu żądanie autoryzacji usługi Azure Active Directory B2C przekierowuje adres URL przekierowania zarejestrowany. Ponieważ adres URL używa schematem niestandardowym powoduje uruchomienie aplikacji mobilnej systemu Android, przekazując w adresie URL jako parametru uruchamiania, gdy jest przetwarzany przez `microsoft.identity.client.BrowserTabActivity`. Należy pamiętać, że `data android:scheme` właściwość musi być ustawiona na niestandardowy schemat adresu URL, która jest zarejestrowana przy użyciu aplikacji Azure Active Directory B2C.

Ponadto `MainActivity` muszą zostać zmodyfikowane klasy, jak pokazano w poniższym przykładzie kodu:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : FormsAppCompatActivity
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

`OnCreate` Metody jest modyfikowany przez przypisywanie `UIParent` wystąpienia do `App.UiParent` właściwości. Gwarantuje to, że przepływ uwierzytelniania odbywa się w kontekście bieżącego działania.

Kod w `OnActivityResult` metoda gwarantuje, że formant powraca do biblioteki MSAL po zakończeniu interaktywne części przepływu pracy uwierzytelniania.

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Na platformie Universal Windows żadna dodatkowa konfiguracja jest wymagana do użycia biblioteki MSAL.

## <a name="initialization"></a>Inicjalizacja

Biblioteka Microsoft Authentication Library używa elementów członkowskich `PublicClientApplication` klasy, aby zainicjować przepływ pracy uwierzytelniania. Przykładowa aplikacja deklaruje i inicjuje `public` właściwości tego typu, o nazwie `ADB2CClient`w `AuthenticationProvider` klasy. Poniższy przykład kodu pokazuje, jak ta właściwość jest inicjowana:

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

W przypadku aplikacji mobilnej został zarejestrowany z dzierżawą usługi Azure Active Directory B2C, proces rejestracji przypisanie **identyfikator aplikacji**. Ten identyfikator musi być określona w `PublicClientApplication` konstruktora, wraz z `Authority` stałą, która składa się z podstawowego adresu URL i zasad usługi Azure Active Directory B2C do wykonania.

## <a name="signing-in"></a>Logowanie

Na poniższych zrzutach ekranu przedstawiono ekran logowania w przykładowej aplikacji:

![](azure-ad-b2c-images/login.png "Strona logowania")

Zaloguj się za pomocą dostawców tożsamości społecznościowych lub za pomocą konta lokalnego, są dozwolone. Microsoft, Google i Facebook, jak pokazano powyżej, są używane jako dostawcy tożsamości społecznościowych, innych dostawców tożsamości można również.

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

`AcquireTokenAsync` Metoda uruchamia przeglądarkę internetową na urządzeniu i wyświetla opcje uwierzytelniania zdefiniowane w zasadach usługi Azure Active Directory B2C, który jest określony przez zasady, są przywoływane za pośrednictwem `Constants.Authority` stałej. Zasady te definiują procesy, przez które przejdą podczas rejestracji i logowania i oświadczenia, że aplikacja otrzyma o pomyślnej rejestracji lub logowania.

Wynik `AcquireTokenAsync` jest wywołanie metody `AuthenticationResult` wystąpienia. Jeśli uwierzytelnianie się powiedzie, `AuthenticationResult` wystąpienie będzie zawierać token tożsamości, które będą buforowane lokalnie. Jeśli uwierzytelnianie zakończy się niepowodzeniem, `AuthenticationResult` wystąpienie będzie zawierać dane, która wskazuje, dlaczego uwierzytelnianie nie powiodło się.

W przykładowej aplikacji, jeśli uwierzytelnianie się powiedzie `TodoList` strony jest przejście.

## <a name="silent-re-authentication"></a>Dyskretnej ponowne uwierzytelnianie

Gdy `LoginPage` w przykładzie pojawi się aplikacja, jest podejmowana próba pobrania tokenu użytkownika bez wyświetlania interfejsu użytkownika uwierzytelniania. Jest to osiągane przy użyciu `AcquireTokenSilentAsync` metody, jak pokazano w poniższym przykładzie kodu:

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

`AcquireTokenSilentAsync` Metoda próbuje pobrać token użytkownika z pamięci podręcznej, bez konieczności użytkownika do logowania. Obsługuje scenariusz, gdzie odpowiedni token może już istnieć w pamięci podręcznej z poprzedniej sesji. Jeśli próba uzyskania tokenu zakończy się powodzeniem, `TodoList` strony jest przejście. Jeśli nie powiedzie się próba uzyskania tokenu, nic się nie dzieje, a użytkownik będzie miał możliwość inicjowania nowego przepływu pracy uwierzytelniania.

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

W tym artykule pokazano, jak zintegrować Zarządzanie tożsamościami konsumentów w aplikacji mobilnej za pomocą Microsoft Authentication Library (MSAL) i Azure Active Directory B2C. Usługa Azure Active Directory B2C to rozwiązanie zarządzania tożsamością w chmurze dla aplikacji przeznaczonych dla klientów internetowych i mobilnych.


## <a name="related-links"></a>Linki pokrewne

- [AzureADB2CAuth (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Biblioteka Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client)
