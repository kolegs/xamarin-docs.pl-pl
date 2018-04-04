---
title: Uwierzytelniania użytkowników przy użyciu usługi Azure Mobile Apps
description: Aplikacje mobilne platformy Azure korzystają z różnych dostawców tożsamości zewnętrznych do obsługi uwierzytelniania i autoryzacji użytkowników aplikacji, w tym usługi Facebook, Google, Microsoft, Twitter i Azure Active Directory. Uprawnienia można ustawić w tabelach, aby ograniczyć dostęp tylko użytkownikom uwierzytelnionym. W tym artykule opisano sposób korzystania z usługi Azure Mobile Apps do zarządzania procesem uwierzytelniania w aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 5f5c69601c11a3c0d25bc804c60883841b0fb30d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>Uwierzytelniania użytkowników przy użyciu usługi Azure Mobile Apps

_Aplikacje mobilne platformy Azure korzystają z różnych dostawców tożsamości zewnętrznych do obsługi uwierzytelniania i autoryzacji użytkowników aplikacji, w tym usługi Facebook, Google, Microsoft, Twitter i Azure Active Directory. Uprawnienia można ustawić w tabelach, aby ograniczyć dostęp tylko użytkownikom uwierzytelnionym. W tym artykule opisano sposób korzystania z usługi Azure Mobile Apps do zarządzania procesem uwierzytelniania w aplikacji platformy Xamarin.Forms._

## <a name="overview"></a>Omówienie

Proces o usłudze Azure Mobile Apps Zarządzanie procesem uwierzytelniania w aplikacji platformy Xamarin.Forms wygląda następująco:

1. Rejestrowanie aplikacji mobilnej Azure w witrynie dostawcy tożsamości, a następnie ustaw generowanych przez dostawcę poświadczeń w zaplecza aplikacji mobilnej. Aby uzyskać więcej informacji, zobacz [zarejestrować aplikację do uwierzytelniania i konfigurowanie usług aplikacji](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services).
1. Zdefiniuj nowy schemat adresu URL dla aplikacji platformy Xamarin.Forms, co umożliwi systemowi uwierzytelniania przekierować z powrotem do aplikacji platformy Xamarin.Forms po zakończeniu procesu uwierzytelniania. Aby uzyskać więcej informacji, zobacz [Dodaj aplikację do dozwolone zewnętrznych adresów URL przekierowań](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl).
1. Ogranicz dostęp do zaplecza Azure Mobile Apps, aby tylko uwierzytelnione klientów. Aby uzyskać więcej informacji, zobacz [ograniczyć uprawnienia dla użytkowników uwierzytelnionych](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users).
1. Wywołanie uwierzytelniania z aplikacji platformy Xamarin.Forms. Aby uzyskać więcej informacji, zobacz [Dodawanie uwierzytelniania do biblioteki klas przenośnych](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library), [Dodawanie uwierzytelniania do aplikacji systemu iOS](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app), [Dodawanie uwierzytelniania do aplikacji systemu Android](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app)i [ Dodawanie uwierzytelniania do projektów aplikacji systemu Windows 10](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects).

> [!NOTE]
> W systemie iOS 9 i większości aplikacji zabezpieczeń transportu (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych. Ponieważ ATS jest domyślnie włączone w aplikacje dla systemu iOS 9, wszystkie połączenia będą podlegać ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.
> ATS można można korzystać z, jeśli nie jest możliwe użycie `HTTPS` protokołu i zabezpieczania komunikacji dla zasobów w Internecie. Można to osiągnąć przez zaktualizowanie aplikacji **Info.plist** pliku. Aby uzyskać więcej informacji, zobacz [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md).

W przeszłości aplikacji dla urządzeń przenośnych zostały użyte widoki osadzonych sieci web do uwierzytelniania z dostawcy tożsamości. Nie jest to zalecane w następujących sytuacjach:

- Aplikacji, która obsługuje widoku sieci web można uzyskać dostępu do poświadczeń pełnego uwierzytelniania użytkownika, nie tylko udzielania autoryzacji jest przeznaczona dla aplikacji. Narusza zasadę najmniejszych uprawnień, jako aplikacja ma dostęp do bardziej zaawansowanych poświadczenia niż go wymaga potencjalnie zwiększa podatność na aplikację.
- Aplikacja hosta można przechwycić nazwy użytkowników i hasła, automatycznie przesyłania formularzy i obejścia zgodę użytkownika i skopiuj pliki cookie z sesji i ich używać do wykonywania akcji uwierzytelniony jako użytkownik.
- Widoki sieci web osadzone nie udostępniaj stanu uwierzytelniania z innych aplikacji lub przeglądarki sieci web urządzenia, gdyż użytkownik musi zalogować przy każdym żądaniu autoryzacji, która jest uznawana za środowisko użytkownika mniejsza niż.
- Niektóre punkty końcowe autoryzacji wykonać czynności, aby wykrywać i blokować żądania autoryzacji, które pochodzą od widoki sieci web.

Alternatywą jest korzystanie z przeglądarki sieci web urządzenia do uwierzytelniania, który jest podejściom najnowszą wersję [zestawu SDK klienta usługi Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/). Żądania uwierzytelniania za pomocą przeglądarki urządzenia poprawia działanie aplikacji, ponieważ użytkownicy potrzebują tylko do logowania do dostawcy tożsamości raz na urządzeniu, poprawy szybkości konwersji przepływów logowania i autoryzacji w aplikacji. W przeglądarce urządzenia również zapewnia wyższy poziom zabezpieczeń, jak aplikacje są w stanie sprawdzić i modyfikować zawartość w widoku sieci web, ale nie zawartość wyświetlaną w przeglądarce.

## <a name="using-an-azure-mobile-apps-instance"></a>Za pomocą wystąpienia usługi Azure Mobile Apps

[Zestawu SDK klienta usługi Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) zapewnia `MobileServiceClient` klasy, która jest używana przez aplikację platformy Xamarin.Forms uzyskać dostępu do wystąpienia usługi Azure Mobile Apps.

Przykładowa aplikacja korzysta z usługi Google jako dostawca tożsamości, dzięki czemu użytkownicy z kontami Google, aby zalogować się do aplikacji platformy Xamarin.Forms. Gdy Google jest używany jako dostawca tożsamości, w tym artykule, podejście jednakowo stosuje się do innych dostawców tożsamości.

<a name="logging-in" />

### <a name="logging-in-users"></a>Logowanie użytkowników

Ekran logowania w przykładowej aplikacji jest wyświetlany na poniższych zrzutach ekranu:

![](azure-images/login.png "Strony logowania")

Gdy Google jest używany jako dostawca tożsamości, różnych innych dostawców tożsamości można, łącznie z usługi Facebook, Microsoft, Twitter i Azure Active Directory.

Poniższy przykład kodu pokazuje sposób wywoływania procesu logowania:

```csharp
async void OnLoginButtonClicked(object sender, EventArgs e)
{
  ...
  if (App.Authenticator != null)
  {
    authenticated = await App.Authenticator.AuthenticateAsync();
  }
  ...
}
```

`App.Authenticator` Jest właściwość `IAuthenticate` wystąpienie, które jest ustawiana przez każdy projekt specyficzne dla platformy. `IAuthenticate` Określa interfejs `AuthenticateAsync` operacja, która musi być dostarczona przez każdy projekt platformy. W związku z tym wywoływania `App.Authenticator.AuthenticateAsync` wykonuje metodę `IAuthenticate.AuthenticateAsync` metody w projekcie platformy.

Wszystkie platformy `IAuthenticate.AuthenticateAsync` wywołania metody `MobileServiceClient.LoginAsync` metodę w celu wyświetlenia interfejsu i pamięci podręcznej danych logowania. Poniższy kod przedstawia przykład `LoginAsync` metody dla platformy iOS:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    UIApplication.SharedApplication.KeyWindow.RootViewController,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Poniższy kod przedstawia przykład `LoginAsync` metodę dla platformy systemu Android:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    this,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Poniższy kod przedstawia przykład `LoginAsync` metodę dla platformy uniwersalnej systemu Windows:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Na wszystkich platformach `MobileServiceAuthenticationProvider` wyliczenia służy do określania dostawcy tożsamości, który będzie używany w procesie uwierzytelniania. Gdy `MobileServiceClient.LoginAsync` wywołania metody, Azure Mobile Apps inicjuje przebieg uwierzytelniania, wyświetlając stronę logowania wybranego dostawcy, a także generowania tokenu uwierzytelniania po pomyślnego logowania przy użyciu dostawcy tożsamości. `MobileServiceClient.LoginAsync` Metoda zwraca `MobileServiceUser` wystąpienie, które będą przechowywane w `MobileServiceClient.CurrentUser` właściwości. Ta właściwość zapewnia `UserId` i `MobileServiceAuthenticationToken` właściwości. Reprezentuje on uwierzytelnionego użytkownika oraz tokenu uwierzytelniania dla użytkownika. Token uwierzytelniania zostaną uwzględnione w wszystkie żądania kierowane do wystąpienia usługi Azure Mobile Apps, umożliwiając aplikacji platformy Xamarin.Forms do wykonania akcji w wystąpieniu aplikacji mobilnej Azure, które wymagają uprawnień uwierzytelnionego użytkownika.

<a name="logging-out" />

### <a name="logging-out-users"></a>Wylogowywanie użytkowników

Poniższy przykład kodu pokazuje sposób wywoływania procesu wylogowania:

```csharp
async void OnLogoutButtonClicked(object sender, EventArgs e)
{
  bool loggedOut = false;

  if (App.Authenticator != null)
  {
    loggedOut = await App.Authenticator.LogoutAsync ();
  }
  ...
}
```

`App.Authenticator` Jest właściwość `IAuthenticate` wystąpienie, które jest ustawiana przez każdy platformproject. `IAuthenticate` Określa interfejs `LogoutAsync` operacja, która musi być dostarczona przez każdy projekt platformy. W związku z tym wywoływania `App.Authenticator.LogoutAsync` wykonuje metodę `IAuthenticate.LogoutAsync` metody w projekcie platformy.

Wszystkie platformy `IAuthenticate.LogoutAsync` wywołania metody `MobileServiceClient.LogoutAsync` metodę, aby wyłączyć uwierzytelnianie użytkowników zalogowali się przy użyciu dostawcy tożsamości. Poniższy kod przedstawia przykład `LogoutAsync` metody dla platformy iOS:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  foreach (var cookie in NSHttpCookieStorage.SharedStorage.Cookies)
  {
    NSHttpCookieStorage.SharedStorage.DeleteCookie (cookie);
  }
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Poniższy kod przedstawia przykład `LogoutAsync` metodę dla platformy systemu Android:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  CookieManager.Instance.RemoveAllCookie();
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Poniższy kod przedstawia przykład `LogoutAsync` metodę dla platformy uniwersalnej systemu Windows:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Gdy `IAuthenticate.LogoutAsync` wywołania metody, wszystkie pliki cookie ustawione przez dostawcę tożsamości zostaną wyczyszczone, przed `MobileServiceClient.LogoutAsync` metoda jest wywoływana, aby wyłączyć uwierzytelnianie użytkowników zalogowali się przy użyciu dostawcy tożsamości.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak używać usługi Azure Mobile Apps do zarządzania procesem uwierzytelniania w aplikacji platformy Xamarin.Forms. Aplikacje mobilne platformy Azure korzystają z różnych dostawców tożsamości zewnętrznych do obsługi uwierzytelniania i autoryzacji użytkowników aplikacji, w tym usługi Facebook, Google, Microsoft, Twitter i Azure Active Directory. `MobileServiceClient` Klasa jest używana przez aplikację platformy Xamarin.Forms do kontrolowania dostępu do wystąpienia usługi Azure Mobile Apps.


## <a name="related-links"></a>Linki pokrewne

- [TodoAzureAuth (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Korzystanie z aplikacji mobilnej Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Dodawanie uwierzytelniania do aplikacji platformy Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
