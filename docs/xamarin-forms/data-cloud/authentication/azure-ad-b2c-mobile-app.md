---
title: Integrowanie usługi Azure Active Directory B2C przy użyciu usługi Azure Mobile Apps
description: Usługa Azure Active Directory B2C to rozwiązanie zarządzania tożsamością w chmurze dla aplikacji przeznaczonych dla klientów internetowych i mobilnych. W tym artykule przedstawiono sposób użycia usługi Azure Active Directory B2C do uwierzytelniania i autoryzacji do wystąpienia usługi Azure Mobile Apps za pomocą zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: cafc1e78779dc393fa0409daa08b3daa8948a1ee
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815680"
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Integrowanie usługi Azure Active Directory B2C przy użyciu usługi Azure Mobile Apps

_Usługa Azure Active Directory B2C to rozwiązanie zarządzania tożsamością w chmurze dla aplikacji przeznaczonych dla klientów internetowych i mobilnych. W tym artykule przedstawiono sposób użycia usługi Azure Active Directory B2C do uwierzytelniania i autoryzacji do wystąpienia usługi Azure Mobile Apps za pomocą zestawu narzędzi Xamarin.Forms._

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji wstępnej")

> [!NOTE]
> [Biblioteka Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) jest nadal w wersji zapoznawczej, ale jest odpowiedni do użytku w środowisku produkcyjnym. Jednak mogą być istotne zmiany w poleceniach interfejsu API, wewnętrznej pamięci podręcznej format i innych mechanizmów biblioteki, która może mieć wpływ na aplikację.

## <a name="overview"></a>Omówienie

Usługa Azure Mobile Apps umożliwiają tworzenie aplikacji za pomocą skalowalnej zaplecza aplikacji hostowanej w usłudze Azure App Service z obsługą mobilnej uwierzytelniania, synchronizacji w trybie offline i powiadomień wypychanych. Aby uzyskać więcej informacji na temat usługi Azure Mobile Apps, zobacz [korzystanie z usługi Azure Mobile App](~/xamarin-forms/data-cloud/consuming/azure.md), i [uwierzytelniania użytkowników za pomocą usługi Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

Usługa Azure Active Directory B2C to usługa zarządzania tożsamościami dla aplikacji przeznaczonych dla konsumentów, umożliwia w konsumentach napisanych logowania do aplikacji przez:

- Przy użyciu istniejących kont sieci społecznościowych (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Tworzenie nowego poświadczenia (adres e-mail i hasło lub nazwa użytkownika i hasło). Te poświadczenia są określane jako *lokalnego* kont.

Aby uzyskać więcej informacji na temat usługi Azure Active Directory B2C, zobacz [uwierzytelniania użytkowników za pomocą usługi Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Usługa Azure Active Directory B2C może służyć do zarządzania procesem uwierzytelniania dla aplikacji mobilnej platformy Azure. W przypadku tej metody możliwości zarządzania tożsamościami jest w pełni zdefiniowana w chmurze i może być modyfikowana bez zmieniania kodu aplikacji mobilnej.

Istnieją dwa przepływów pracy uwierzytelniania, które może przyjąć podczas integrowania dzierżawy usługi Azure Active Directory B2C z wystąpieniem usługi Azure Mobile Apps:

- [Klient zarządzany](#client_managed) — w tym podejście inicjuje aplikacji mobilnej platformy Xamarin.Forms procesu uwierzytelniania za pomocą dzierżawy usługi Azure Active Directory B2C i przekazanie tokenu uwierzytelniania odebrane do wystąpienia usługi Azure Mobile Apps.
- [Serwer zarządzany](#server_managed) — w tym podejściu Azure Mobile Apps wystąpienie używa dzierżawy usługi Azure Active Directory B2C, aby zainicjować proces uwierzytelniania za pomocą przepływu pracy opartego na sieci web.

W obu przypadkach uwierzytelnianie jest dostarczane przez dzierżawcę usługi Azure Active Directory B2C. W przykładowej aplikacji powoduje to ekran logowania pokazano na poniższych zrzutach ekranu:

![](azure-ad-b2c-mobile-app-images/screenshots.png "Strona logowania")

Zaloguj się za pomocą dostawców tożsamości społecznościowych lub za pomocą konta lokalnego, są dozwolone. Microsoft, Google i Facebook są używane jako dostawcy tożsamości społecznościowych, w tym przykładzie, innych dostawców tożsamości można również.

## <a name="setup"></a>Konfiguracja

Niezależnie od tego przepływu pracy uwierzytelniania używane początkowego procesu integracji dzierżawy usługi Azure Active Directory B2C z wystąpieniem usługi Azure Mobile Apps jest następująca:

1. Utwórz wystąpienie usługi Azure Mobile Apps. Aby uzyskać więcej informacji, zobacz [korzystanie z usługi Azure Mobile App](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Włączanie uwierzytelniania w wystąpieniu usługi Azure Mobile Apps i aplikacji platformy Xamarin.Forms. Aby uzyskać więcej informacji, zobacz [uwierzytelniania użytkowników za pomocą usługi Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).
1. Tworzenie dzierżawy usługi Azure Active Directory B2C. Aby uzyskać więcej informacji, zobacz [uwierzytelniania użytkowników za pomocą usługi Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Należy pamiętać, że biblioteka Microsoft Authentication Library (MSAL) wymaganego podczas korzystania z przepływu pracy uwierzytelniania zarządzanych przez klienta. Biblioteka MSAL używa przeglądarki sieci web urządzenia do wykonywania uwierzytelniania. Zwiększa użyteczność aplikacji, ponieważ użytkownicy potrzebują tylko zalogować się po na każdym urządzeniu, przepływy poprawy logowania i autoryzacji w aplikacji. W przeglądarce na urządzeniu również zapewnia wyższy poziom zabezpieczeń. Po ukończeniu procesu uwierzytelniania użytkownika kontroli powróci do aplikacji z kartę przeglądarki sieci web. Jest to osiągane przez zarejestrowanie niestandardowy schemat adresu URL dla adresu URL przekierowania, który jest zwracany z procesu uwierzytelniania, a następnie wykrywania i obsługi niestandardowego adresu URL po wysłaniu. Aby uzyskać więcej informacji na temat używania biblioteki MSAL do komunikowania się z dzierżawą usługi Azure Active Directory B2C, zobacz [uwierzytelniania użytkowników za pomocą usługi Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

<a name="client_managed" />

## <a name="client-managed-authentication"></a>Klient zarządzany uwierzytelniania

W przypadku zarządzanych przez klienta uwierzytelniania aplikacji mobilnej platformy Xamarin.Forms kontaktuje się z dzierżawy usługi Azure Active Directory B2C, aby zainicjować przepływ uwierzytelniania. Po pomyślnym zalogowaniu Azure Active Directory B2C dzierżawy zwraca token tożsamości, który następnie zostanie podany podczas logowania do wystąpienia usługi Azure Mobile Apps. Umożliwia to aplikacji platformy Xamarin.Forms do wykonywania akcji względem wystąpienia usługi Azure Mobile Apps, które wymaga uprawnień uwierzytelnionego użytkownika.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Konfiguracja dzierżawy B2C usługi Azure Active Directory

Dla przepływu uwierzytelniania zarządzanych przez klienta dzierżawy usługi Azure Active Directory B2C należy skonfigurować w następujący sposób:

- Dołącz klienta natywnego.
- Ustaw identyfikator URI przekierowania niestandardowego na schemat adresu URL, który unikatowo identyfikuje w aplikacji mobilnej, a następnie `://auth/`. Aby uzyskać więcej informacji o wybieraniu niestandardowy schemat adresu URL, zobacz [Wybieranie identyfikatora URI przekierowania aplikacji natywnej](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri).

Poniższy zrzut ekranu przedstawia tę konfigurację:

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Usługa Azure Active Directory B2C konfiguracji")](azure-ad-b2c-mobile-app-images/client-flow-config.png#lightbox "usługi Azure Active Directory B2C konfiguracji")

Zasady usługi Azure Active Directory B2C dzierżawy powinny być również skonfigurowane tak, aby adres URL odpowiedzi do tego samego niestandardowy schemat adresu URL, a następnie `://auth/`. Poniższy zrzut ekranu przedstawia tę konfigurację:

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Usługa Azure Active Directory B2C zasad")

### <a name="azure-mobile-app-configuration"></a>Konfiguracja aplikacji mobilnej platformy Azure

Dla przepływu pracy uwierzytelniania zarządzanych przez klienta wystąpienie usługi Azure Mobile Apps należy skonfigurować w następujący sposób:

- Uwierzytelnianie usługi App Service powinna być włączona.
- Akcja do wykonania w przypadku nieuwierzytelnionego żądania powinna być równa **Zaloguj się przy użyciu usługi Azure Active Directory**.

Poniższy zrzut ekranu przedstawia tę konfigurację:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Konfiguracja usługi Azure Mobile Apps")

Wystąpienie usługi Azure Mobile Apps należy również skonfigurować do komunikowania się z dzierżawy usługi Azure Active Directory B2C. Można to zrobić przez włączenie **zaawansowane** tryb dla dostawcy uwierzytelniania usługi Azure Active Directory, za pomocą **identyfikator klienta** trwa **identyfikator aplikacji** platformy Azure Dzierżawy usługi Active Directory B2C i **adres Url wystawcy** jest punkt końcowy metadanych dla zasad usługi Azure Active Directory B2C. Poniższy zrzut ekranu przedstawia tę konfigurację:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Usługi Azure Mobile Apps, Zaawansowana konfiguracja")

### <a name="signing-in"></a>Logowanie

Poniższy przykład kodu pokazuje, jak inicjować przepływ pracy uwierzytelniania zarządzanych przez klienta:

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

Biblioteka Microsoft Authentication Library (MSAL) jest używany do inicjowania przepływu pracy uwierzytelniania za pomocą dzierżawy usługi Azure Active Directory B2C. `AcquireTokenAsync` Metoda uruchamia przeglądarkę internetową na urządzeniu i wyświetla opcje uwierzytelniania zdefiniowane w zasadach usługi Azure Active Directory B2C, który jest określony przez zasady, są przywoływane za pośrednictwem `Constants.Authority` stałej. Zasady te definiują procesy, przez które przejdą podczas rejestracji i logowania i oświadczenia, że aplikacja otrzyma o pomyślnej rejestracji lub logowania.

Wynik `AcquireTokenAsync` jest wywołanie metody `AuthenticationResult` wystąpienia. Jeśli uwierzytelnianie się powiedzie, `AuthenticationResult` wystąpienie będzie zawierać token tożsamości, które będą buforowane lokalnie. Jeśli uwierzytelnianie zakończy się niepowodzeniem, `AuthenticationResult` wystąpienie będzie zawierać dane, która wskazuje, dlaczego uwierzytelnianie nie powiodło się. Aby uzyskać informacje na temat sposobu użycia biblioteki MSAL do komunikowania się z dzierżawą usługi Azure Active Directory B2C, zobacz [uwierzytelniania użytkowników za pomocą usługi Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Gdy `MobileServiceClient.LoginAsync` metoda jest wywoływana, wystąpienie usługi Azure Mobile Apps odbiera token tożsamości w `JObject`. Obecność prawidłowy token oznacza, że wystąpienia usługi Azure Mobile Apps nie musi inicjować przepływ OAuth 2.0 uwierzytelniania. Zamiast tego `MobileServiceClient.LoginAsync` metoda zwraca `MobileServiceUser` wystąpienie, które będą przechowywane w `MobileServiceClient.CurrentUser` właściwości. Ta właściwość zapewnia `UserId` i `MobileServiceAuthenticationToken` właściwości. Te reprezentują uwierzytelnionego użytkownika i token uwierzytelniania użytkownika, który może być używany, dopóki nie wygaśnie. Token uwierzytelniania zostaną uwzględnione w wszystkie żądania przekazywane do wystąpienia usługi Azure Mobile Apps, umożliwiając aplikacji platformy Xamarin.Forms do wykonywania akcji względem wystąpienia usługi Azure Mobile Apps, które wymagają uprawnień uwierzytelnionego użytkownika.

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

`MobileServiceClient.LogoutAsync` Metoda cofnąć uwierzytelnia użytkownika przy użyciu wystąpienia usługi Azure Mobile Apps, a następnie wszystkie tokeny uwierzytelniania są wyczyszczone z lokalnej pamięci podręcznej, utworzone przez biblioteki MSAL.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>Serwer zarządzany uwierzytelniania

W przypadku uwierzytelniania serwer zarządzany aplikacji platformy Xamarin.Forms kontaktuje się z wystąpienia usługi Azure Mobile Apps używa dzierżawy usługi Azure Active Directory B2C w celu zarządzania przepływem uwierzytelniania OAuth 2.0, wyświetlając stronę logowania, zgodnie z definicją w zasadach usługi B2C. Po pomyślnym zalogowaniu wystąpienie usługi Azure Mobile Apps zwraca token, który umożliwia aplikacji platformy Xamarin.Forms do wykonywania akcji względem wystąpienia usługi Azure Mobile Apps, które wymagają uprawnień uwierzytelnionego użytkownika.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Konfiguracja dzierżawy B2C usługi Azure Active Directory

Serwer zarządzany uwierzytelniania przepływu pracy dzierżawy usługi Azure Active Directory B2C należy skonfigurować w następujący sposób:

- Obejmują aplikacji sieci web/internetowego interfejsu API i Zezwalaj na niejawny przepływ.
- Ustaw adres URL odpowiedzi na adres aplikacji mobilnej platformy Azure, a następnie `/.auth/login/aad/callback`.

Poniższy zrzut ekranu przedstawia tę konfigurację:

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Usługa Azure Active Directory B2C konfiguracji")](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "usługi Azure Active Directory B2C konfiguracji")

Zasady stosowane w usłudze Azure Active Directory B2C dzierżawy powinny być również skonfigurowane tak, aby adres URL odpowiedzi na adres aplikacji mobilnej platformy Azure, a następnie `/.auth/login/aad/callback`. Poniższy zrzut ekranu przedstawia tę konfigurację:

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Usługa Azure Active Directory B2C zasad")

### <a name="azure-mobile-apps-instance-configuration"></a>Konfiguracja wystąpienia usługi Azure Mobile Apps

Serwer zarządzany uwierzytelniania przepływu pracy wystąpienie usługi Azure Mobile Apps należy skonfigurować w następujący sposób:

- Uwierzytelnianie usługi App Service powinna być włączona.
- Akcja do wykonania w przypadku nieuwierzytelnionego żądania powinna być równa **Zaloguj się przy użyciu usługi Azure Active Directory**.

Poniższy zrzut ekranu przedstawia tę konfigurację:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Konfiguracja usługi Azure Mobile Apps")

Wystąpienie usługi Azure Mobile Apps należy również skonfigurować do komunikowania się z dzierżawy usługi Azure Active Directory B2C. Można to zrobić przez włączenie **zaawansowane** tryb dla dostawcy uwierzytelniania usługi Azure Active Directory, za pomocą **identyfikator klienta** trwa **identyfikator aplikacji** platformy Azure Dzierżawy usługi Active Directory B2C i **adres Url wystawcy** jest punkt końcowy metadanych dla zasad usługi Azure Active Directory B2C. Poniższy zrzut ekranu przedstawia tę konfigurację:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Usługi Azure Mobile Apps, Zaawansowana konfiguracja")

### <a name="signing-in"></a>Logowanie

Poniższy przykład kodu pokazuje sposób inicjowania przepływu pracy zarządzanego serwera uwierzytelniania:

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

Gdy `MobileServiceClient.LoginAsync` metoda jest wywoływana, wystąpienie usługi Azure Mobile Apps wykonuje połączonego zasad usługi Azure Active Directory B2C, który inicjuje przepływ uwierzytelniania OAuth 2.0. Należy pamiętać, że każdy `AuthenticateAsync` metodą jest specyficzne dla platformy. Jednak każdy `AuthenticateAsync` metoda używa `MobileServiceClient.LoginAsync` metody i określa, że dzierżawa usługi Azure Active Directory będzie używane w procesie uwierzytelniania. Aby uzyskać więcej informacji, zobacz [logowania użytkowników](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

`MobileServiceClient.LoginAsync` Metoda zwraca `MobileServiceUser` wystąpienie, które będą przechowywane w `MobileServiceClient.CurrentUser` właściwości. Ta właściwość zapewnia `UserId` i `MobileServiceAuthenticationToken` właściwości. Te reprezentują uwierzytelnionego użytkownika i token uwierzytelniania użytkownika, który może być używany, dopóki nie wygaśnie. Token uwierzytelniania zostaną uwzględnione w wszystkie żądania przekazywane do wystąpienia usługi Azure Mobile Apps, umożliwiając aplikacji platformy Xamarin.Forms do wykonywania akcji względem wystąpienia usługi Azure Mobile Apps, które wymagają uprawnień uwierzytelnionego użytkownika.

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

`MobileServiceClient.LogoutAsync` Metoda cofnąć uwierzytelnia użytkownika przy użyciu wystąpienia usługi Azure Mobile Apps. Aby uzyskać więcej informacji, zobacz [rejestrowanie użytkowników](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out).

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak używać usługi Azure Active Directory B2C do uwierzytelniania i autoryzacji do wystąpienia usługi Azure Mobile Apps za pomocą zestawu narzędzi Xamarin.Forms. Usługa Azure Active Directory B2C to rozwiązanie zarządzania tożsamością w chmurze dla aplikacji przeznaczonych dla klientów internetowych i mobilnych.


## <a name="related-links"></a>Linki pokrewne

- [TodoAzureAuth ServerFlow (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Korzystanie z aplikacji mobilnej platformy Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Uwierzytelnianie użytkowników za pomocą usługi Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Uwierzytelnianie użytkowników za pomocą usługi Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Biblioteka Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client)
