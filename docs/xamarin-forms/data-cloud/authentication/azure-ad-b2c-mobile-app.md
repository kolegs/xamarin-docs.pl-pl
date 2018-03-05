---
title: "Integrowanie usługi Azure Active Directory B2C z usługi Azure Mobile Apps"
description: "Usługa Azure Active Directory B2C jest chmury rozwiązania do zarządzania tożsamościami internetowych i aplikacji dla urządzeń przenośnych. W tym artykule przedstawiono sposób użycia usługi Azure Active Directory B2C, aby zapewnić uwierzytelnianie i autoryzację do wystąpienia usługi Azure Mobile Apps platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 3a7d89d9b0f383d365b18364e5d902ee0642f395
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Integrowanie usługi Azure Active Directory B2C z usługi Azure Mobile Apps

_Usługa Azure Active Directory B2C jest chmury rozwiązania do zarządzania tożsamościami internetowych i aplikacji dla urządzeń przenośnych. W tym artykule przedstawiono sposób użycia usługi Azure Active Directory B2C, aby zapewnić uwierzytelnianie i autoryzację do wystąpienia usługi Azure Mobile Apps platformy Xamarin.Forms._

![](~/media/shared/preview.png "Ten interfejs API jest obecnie wersji wstępnej")

> [!NOTE]
> **Uwaga**: [biblioteki uwierzytelniania](https://www.nuget.org/packages/Microsoft.Identity.Client) jest wciąż w wersji zapoznawczej, ale są odpowiednie do użycia w środowisku produkcyjnym. Jednak mogą być istotne zmiany interfejsu API, format wewnętrznej pamięci podręcznej i innych mechanizmów biblioteki, która może wpływać na działanie aplikacji.

## <a name="overview"></a>Omówienie

Aplikacje mobilne platformy Azure umożliwiają tworzenie aplikacji z zapleczy skalowalne hostowanych w usłudze Azure App Service z obsługą przenośnych uwierzytelniania, synchronizacji w trybie offline i powiadomień wypychanych. Aby uzyskać więcej informacji na temat usługi Azure Mobile Apps, zobacz [korzystanie z aplikacji mobilnej Azure](~/xamarin-forms/data-cloud/consuming/azure.md), i [uwierzytelniania użytkowników z usługą Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

Usługa Azure Active Directory B2C jest usługa identity management w przypadku aplikacji dla użytkownika umożliwiający konsumentów do logowania do aplikacji przez:

- Przy użyciu istniejących kont społecznościowych (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Tworzenie nowego poświadczenia (adres e-mail i hasło lub nazwa użytkownika i hasło). Te poświadczenia są określane jako *lokalnego* kont.

Aby uzyskać więcej informacji na temat usługi Azure Active Directory B2C, zobacz [uwierzytelniania użytkowników z usługi Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Usługa Azure Active Directory B2C może służyć do zarządzania procesem uwierzytelniania dla aplikacji mobilnej Azure. Z tej metody możliwości zarządzania tożsamościami jest całkowicie zdefiniowane w chmurze i może być modyfikowany bez zmieniania kodu aplikacji mobilnej.

Istnieją dwa przepływów pracy uwierzytelniania, które może przyjąć podczas integracji z wystąpieniem usługi Azure Mobile Apps dzierżawy usługi Azure Active Directory B2C:

- [Zarządzane przez klienta](#client_managed) — w tym podejścia inicjuje aplikacji mobilnej platformy Xamarin.Forms proces uwierzytelniania z dzierżawy usługi Azure Active Directory B2C i przekazuje token uwierzytelniania odebranych z wystąpieniem usługi Azure Mobile Apps.
- [Serwer zarządzany](#server_managed) — w takie podejście Azure Mobile Apps wystąpienia korzysta dzierżawy usługi Azure Active Directory B2C, aby zainicjować proces uwierzytelniania za pomocą przepływu pracy opartych na sieci web.

W obu przypadkach środowisko uwierzytelniania jest udostępniany przez dzierżawy usługi Azure Active Directory B2C. W przykładowej aplikacji w efekcie ekranu logowania pokazano na poniższych zrzutach ekranu:

![](azure-ad-b2c-mobile-app-images/screenshots.png "Strony logowania")

Logowania z dostawców tożsamości społecznościowych lub przy użyciu konta lokalnego, są dozwolone. Microsoft, Google i Facebook są używane jako dostawcy tożsamości społecznościowych w tym przykładzie, można również używane innych dostawców tożsamości.

## <a name="setup"></a>Konfiguracja

Niezależnie od tego przepływu pracy uwierzytelniania używane początkowego procesu do integracji z wystąpieniem usługi Azure Mobile Apps dzierżawy usługi Azure Active Directory B2C jest następujący:

1. Utwórz wystąpienie usługi Azure Mobile Apps. Aby uzyskać więcej informacji, zobacz [korzystanie z aplikacji mobilnej Azure](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Włączanie uwierzytelniania w wystąpieniu usługi Azure Mobile Apps i aplikacji platformy Xamarin.Forms. Aby uzyskać więcej informacji, zobacz [uwierzytelniania użytkowników z usługą Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).
1. Tworzenie dzierżawy usługi Azure Active Directory B2C. Aby uzyskać więcej informacji, zobacz [uwierzytelniania użytkowników z usługi Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Należy pamiętać, że biblioteki uwierzytelniania firmy Microsoft (MSAL) jest wymagana podczas używania przepływu pracy uwierzytelniania zarządzanych przez klienta. MSAL używa przeglądarki sieci web na urządzeniu w celu przeprowadzenia uwierzytelniania. Poprawia to użyteczność aplikacji, ponieważ użytkownicy potrzebują tylko do logowania po na urządzenie, przepływy poprawy logowania i autoryzacji w aplikacji. Przeglądarki urządzenia także udostępnia lepsze zabezpieczenia. Po zakończeniu procesu uwierzytelniania użytkownika formant powróci do aplikacji na karcie przeglądarki sieci web. Jest to osiągane przez zarejestrowanie schemat niestandardowy adres URL jest zwracana z procesu uwierzytelniania, a następnie wykrywania i obsługi niestandardowy adres URL po przesłaniu jej adresu URL przekierowania. Aby uzyskać więcej informacji o używaniu MSAL do komunikowania się z dzierżawy usługi Azure Active Directory B2C, zobacz [uwierzytelniania użytkowników z usługi Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

<a name="client_managed" />

## <a name="client-managed-authentication"></a>Zarządzane przez klienta do uwierzytelniania

W przypadku zarządzanych przez klienta uwierzytelniania aplikacji mobilnej platformy Xamarin.Forms kontaktuje się z dzierżawy usługi Azure Active Directory B2C, aby zainicjować przepływ uwierzytelniania. Po pomyślnym logowania jednokrotnego usługi Azure Active Directory B2C dzierżawy zwraca token tożsamości, który jest następnie dostarczany podczas logowania się z wystąpieniem usługi Azure Mobile Apps. Umożliwia to aplikacji platformy Xamarin.Forms do wykonania akcji w wystąpieniu usługi Azure Mobile Apps, które wymaga uprawnień uwierzytelnionego użytkownika.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Skonfigurowania dzierżawy usługi Azure Active Directory B2C

Zarządzane przez klienta uwierzytelniania przepływu pracy dzierżawy usługi Azure Active Directory B2C należy skonfigurować w następujący sposób:

- Obejmują klientami.
- Ustaw identyfikator URI przekierowania niestandardowy schemat adresu URL, który unikatowo identyfikuje aplikacji mobilnej, a następnie `://auth/`. Aby uzyskać więcej informacji o wybieraniu schemat niestandardowy adres URL, zobacz [Wybieranie na identyfikator URI przekierowania aplikacji natywnej](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri).

Poniższy zrzut ekranu przedstawia tej konfiguracji:

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Usługa Azure Active Directory B2C konfiguracji")](azure-ad-b2c-mobile-app-images/client-flow-config.png "usługi Azure Active Directory B2C konfiguracji")

Zasady usługi Azure Active Directory B2C dzierżawy powinien również być skonfigurowany tak, aby adres URL odpowiedzi jest ustawiona na tej samej niestandardowych schemat adresu URL, a następnie `://auth/`. Poniższy zrzut ekranu przedstawia tej konfiguracji:

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Usługa Azure Active Directory B2C zasad")

### <a name="azure-mobile-app-configuration"></a>Konfiguracja aplikacji mobilnej Azure

Zarządzane przez klienta uwierzytelniania przepływu pracy wystąpienie usługi Azure Mobile Apps należy skonfigurować w następujący sposób:

- Uwierzytelnianie usługi aplikacji powinna być włączona.
- Działanie podejmowane w przypadku nieuwierzytelnionego żądania powinien mieć ustawioną **Zaloguj się za pomocą usługi Azure Active Directory**.

Poniższy zrzut ekranu przedstawia tej konfiguracji:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Konfiguracja usługi Azure Mobile Apps")

Wystąpienie usługi Azure Mobile Apps powinien również być skonfigurowany do komunikowania się z dzierżawy usługi Azure Active Directory B2C. Można to zrobić przez włączenie **zaawansowane** tryb dla dostawcy uwierzytelniania usługi Azure Active Directory, z **identyfikator klienta** trwa **identyfikator aplikacji** Azure Dzierżawy usługi Active Directory B2C i **adres Url wystawcy** jest punktem końcowym metadanych dla zasad usługi Azure Active Directory B2C. Poniższy zrzut ekranu przedstawia tej konfiguracji:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Azure Mobile Apps Konfiguracja zaawansowana")

### <a name="signing-in"></a>Logowanie

Poniższy przykładowy kod przedstawia sposób inicjowania przepływu pracy uwierzytelniania zarządzanych przez klienta:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);

    ...
    var payload = new JObject();
    payload["access_token"] = authenticationResult.IdToken;

    User = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        payload);
    ...
}
```

Biblioteka uwierzytelniania firmy Microsoft (MSAL) jest używany do inicjowania przepływu pracy uwierzytelniania z dzierżawą usługi Azure Active Directory B2C. `AcquireTokenAsync` Metoda spowoduje uruchomienie przeglądarki sieci web na urządzeniu i wyświetla opcje uwierzytelniania zdefiniowane w zasadach usługi Azure Active Directory B2C, określonej przez zasady, do których odwołuje się za pośrednictwem `Constants.Authority` stałej. Ta zasada definiuje procesy, które konsumentów będzie przejście przez podczas rejestracji i logowania i oświadczenia się, że aplikacja otrzyma pomyślnie rejestracji lub logowania.

Wynik `AcquireTokenAsync` jest wywołanie metody `AuthenticationResult` wystąpienia. Jeśli uwierzytelnianie się powiedzie, `AuthenticationResult` wystąpienie będzie zawierać token tożsamości, które będą buforowane lokalnie. Jeśli uwierzytelnianie zakończy się niepowodzeniem, `AuthenticationResult` wystąpienie będzie zawierać dane, które wskazuje na to, dlaczego uwierzytelnianie nie powiodło się. Aby uzyskać informacje dotyczące sposobu używania MSAL do komunikowania się z dzierżawy usługi Azure Active Directory B2C, zobacz [uwierzytelniania użytkowników z usługi Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Gdy `MobileServiceClient.LoginAsync` wywołania metody, wystąpienie usługi Azure Mobile Apps otrzymuje token tożsamości w `JObject`. Obecność prawidłowy token oznacza, że wystąpienie usługi Azure Mobile Apps nie musi inicjować własną przepływ uwierzytelniania OAuth 2.0. Zamiast tego `MobileServiceClient.LoginAsync` metoda zwraca `MobileServiceUser` wystąpienie, które będą przechowywane w `MobileServiceClient.CurrentUser` właściwości. Ta właściwość zapewnia `UserId` i `MobileServiceAuthenticationToken` właściwości. Reprezentuje on uwierzytelnionego użytkownika oraz tokenu uwierzytelniania użytkownika, która może służyć do momentu wygaśnięcia. Token uwierzytelniania zostaną uwzględnione w wszystkie żądania kierowane do wystąpienia usługi Azure Mobile Apps, umożliwiając aplikacji platformy Xamarin.Forms do wykonania akcji w wystąpieniu usługi Azure Mobile Apps, które wymagają uprawnień uwierzytelnionego użytkownika.

### <a name="signing-out"></a>Wylogowywanie

Poniższy przykład kodu pokazuje sposób wywoływania proces wylogowywania zarządzanych przez klienta:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();

    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

`MobileServiceClient.LogoutAsync` Metoda zwalnia uwierzytelnia użytkownika z wystąpieniem usługi Azure Mobile Apps, a następnie wszystkie tokeny uwierzytelniania są usuwane z lokalnej pamięci podręcznej utworzone przez MSAL.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>Serwer zarządzany uwierzytelniania

W przypadku uwierzytelniania serwer zarządzany aplikacji platformy Xamarin.Forms kontaktuje się wystąpienia usługi Azure Mobile Apps, która używa dzierżawy usługi Azure Active Directory B2C do zarządzania przepływ uwierzytelniania OAuth 2.0, zgodnie z definicją w zasadach B2C, wyświetlając stronę logowania. Po pomyślnym logowania jednokrotnego wystąpienie usługi Azure Mobile Apps zwraca token, który umożliwia aplikacji platformy Xamarin.Forms do wykonania akcji w wystąpieniu usługi Azure Mobile Apps, które wymagają uprawnień uwierzytelnionego użytkownika.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Skonfigurowania dzierżawy usługi Azure Active Directory B2C

Serwer zarządzany uwierzytelniania przepływu pracy dzierżawy usługi Azure Active Directory B2C należy skonfigurować w następujący sposób:

- Obejmują sieci web aplikacji/składnika web API i umożliwić niejawnego przepływu.
- Ustaw adres URL odpowiedzi na adres aplikacji mobilnej Azure, a następnie `/.auth/login/aad/callback`.

Poniższy zrzut ekranu przedstawia tej konfiguracji:

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Usługa Azure Active Directory B2C konfiguracji")](azure-ad-b2c-mobile-app-images/server-flow-config.png "usługi Azure Active Directory B2C konfiguracji")

Zasady usługi Azure Active Directory B2C dzierżawy powinien również być skonfigurowany tak, aby adres URL odpowiedzi jest ustawiona jako adres aplikacji mobilnej Azure, a następnie `/.auth/login/aad/callback`. Poniższy zrzut ekranu przedstawia tej konfiguracji:

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Usługa Azure Active Directory B2C zasad")

### <a name="azure-mobile-apps-instance-configuration"></a>Konfiguracja wystąpienia usługi Azure Mobile Apps

Serwer zarządzany uwierzytelniania przepływu pracy wystąpienie usługi Azure Mobile Apps należy skonfigurować w następujący sposób:

- Uwierzytelnianie usługi aplikacji powinna być włączona.
- Działanie podejmowane w przypadku nieuwierzytelnionego żądania powinien mieć ustawioną **Zaloguj się za pomocą usługi Azure Active Directory**.

Poniższy zrzut ekranu przedstawia tej konfiguracji:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Konfiguracja usługi Azure Mobile Apps")

Wystąpienie usługi Azure Mobile Apps powinien również być skonfigurowany do komunikowania się z dzierżawy usługi Azure Active Directory B2C. Można to zrobić przez włączenie **zaawansowane** tryb dla dostawcy uwierzytelniania usługi Azure Active Directory, z **identyfikator klienta** trwa **identyfikator aplikacji** Azure Dzierżawy usługi Active Directory B2C i **adres Url wystawcy** jest punktem końcowym metadanych dla zasad usługi Azure Active Directory B2C. Poniższy zrzut ekranu przedstawia tej konfiguracji:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Azure Mobile Apps Konfiguracja zaawansowana")

### <a name="signing-in"></a>Logowanie

W poniższym przykładzie pokazano, jak zainicjować uwierzytelniania serwera zarządzany przepływu pracy:

```csharp
public async Task<bool> AuthenticateAsync()
{
    ...
    MobileServiceUser user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        UIApplication.SharedApplication.KeyWindow.RootViewController,
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        Constants.URLScheme);
    ...
}
```

Gdy `MobileServiceClient.LoginAsync` wywołania metody, wystąpienia usługi Azure Mobile Apps wykonuje połączone zasady usługi Azure Active Directory B2C, który inicjuje przebieg uwierzytelniania OAuth 2.0. Należy pamiętać, że każdy `AuthenticateAsync` metoda jest specyficzne dla platformy. Jednak każdy `AuthenticateAsync` używa metody `MobileServiceClient.LoginAsync` — metoda i określa, że dzierżawy usługi Azure Active Directory będzie używany w procesie uwierzytelniania. Aby uzyskać więcej informacji, zobacz [logowanie użytkowników](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

`MobileServiceClient.LoginAsync` Metoda zwraca `MobileServiceUser` wystąpienie, które będą przechowywane w `MobileServiceClient.CurrentUser` właściwości. Ta właściwość zapewnia `UserId` i `MobileServiceAuthenticationToken` właściwości. Reprezentuje on uwierzytelnionego użytkownika oraz tokenu uwierzytelniania użytkownika, która może służyć do momentu wygaśnięcia. Token uwierzytelniania zostaną uwzględnione w wszystkie żądania kierowane do wystąpienia usługi Azure Mobile Apps, umożliwiając aplikacji platformy Xamarin.Forms do wykonania akcji w wystąpieniu usługi Azure Mobile Apps, które wymagają uprawnień uwierzytelnionego użytkownika.

### <a name="signing-out"></a>Wylogowywanie

Poniższy przykład kodu pokazuje sposób wywoływania serwer zarządzany proces logowania:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

`MobileServiceClient.LogoutAsync` Metoda zwalnia uwierzytelnia użytkownika z wystąpieniem usługi Azure Mobile Apps. Aby uzyskać więcej informacji, zobacz [rejestrowanie użytkowników](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia usługi Azure Active Directory B2C, aby zapewnić uwierzytelnianie i autoryzację do wystąpienia usługi Azure Mobile Apps platformy Xamarin.Forms. Usługa Azure Active Directory B2C jest chmury rozwiązania do zarządzania tożsamościami internetowych i aplikacji dla urządzeń przenośnych.


## <a name="related-links"></a>Linki pokrewne

- [TodoAzureAuth ServerFlow (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [](~/xamarin-forms/data-cloud/consuming/azure.md)
- [](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Uwierzytelnianie użytkowników w usłudze Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Biblioteka uwierzytelniania firmy Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client)
