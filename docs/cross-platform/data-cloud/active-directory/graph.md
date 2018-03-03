---
title: "Uzyskiwanie dostępu do interfejsu API Graph"
description: "Przy użyciu usługi Active Directory do zapytania interfejsu API programu Graph, za pomocą platformy Xamarin"
ms.topic: article
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 2b0e4a9466ab59ec27b957bf284a7d3895f6a4fc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="accessing-the-graph-api"></a>Uzyskiwanie dostępu do interfejsu API Graph

_Przy użyciu usługi Active Directory do zapytania interfejsu API programu Graph, za pomocą platformy Xamarin_

Wykonaj następujące kroki, aby użyć interfejsu API programu Graph z poziomu aplikacji platformy Xamarin:

1. [Rejestrowanie w usłudze Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) na *windowsazure.com* portalu, a następnie
2. [Konfigurowanie usług](~/cross-platform/data-cloud/active-directory/get-started/configure.md).

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>Krok 3. Dodawanie uwierzytelniania usługi Active Directory do aplikacji

W aplikacji, Dodaj odwołanie do **Azure Active Directory Authentication Library (biblioteki Azure ADAL)** za pomocą Menedżera pakietów NuGet w programie Visual Studio lub Visual Studio dla komputerów Mac.
Upewnij się, że wybrano **Pokaż pakiety wersji wstępnej** uwzględnienie tego pakietu, ponieważ jest on dostępny w wersji zapoznawczej.

> [!IMPORTANT]
> Uwaga: Azure ADAL 3.0 obecnie Podgląd i może być istotne zmiany przed wydaniem ostatecznej wersji. 


![](graph-images/06.-adal-nuget-package.jpg "Dodaj odwołanie do usługi Azure Active Directory Authentication Library (biblioteki Azure ADAL)")

W aplikacji należy teraz dodać następujące zmienne poziomu klasy, które są wymagane dla przepływ uwierzytelniania.

```csharp
//Client ID
public static string clientId = "25927d3c-.....-63f2304b90de";
public static string commonAuthority = "https://login.windows.net/common"
//Redirect URI
public static Uri returnUri = new Uri("http://xam-demo-redirect");
//Graph URI if you've given permission to Azure Active Directory
const string graphResourceUri = "https://graph.windows.net";
public static string graphApiVersion = "2013-11-08";
//AuthenticationResult will hold the result after authentication completes
AuthenticationResult authResult = null;
```

Jedyną operacją, należy pamiętać, w tym miejscu `commonAuthority`. Gdy punktu końcowego uwierzytelniania jest `common`, aplikacja stanie się **wielodostępnej**, co oznacza każdy użytkownik, można użyć nazwy logowania przy użyciu poświadczeń usługi Active Directory. Po uwierzytelnieniu użytkownik będzie działać w kontekście własne usługi Active Directory — czyli zobaczą szczegółowe informacje związane z jego usługi Active Directory.

### <a name="write-method-to-acquire-access-token"></a>Write — Metoda uzyskać Token dostępu

Poniższy kod (dla systemu Android) będą rozpoczęcia uwierzytelniania i po zakończeniu Przypisz wynik w `authResult`. IOS i Windows Phone implementacje różnić się nieznacznie: drugi parametr (`Activity`) i różni się w systemie iOS znajduje się na Windows Phone.

```csharp
public static async Task<AuthenticationResult> GetAccessToken
            (string serviceResourceId, Activity activity)
{
    authContext = new AuthenticationContext(Authority);
    if (authContext.TokenCache.ReadItems().Count() > 0)
        authContext = new AuthenticationContext(authContext.TokenCache.ReadItems().First().Authority);
    var authResult = await authContext.AcquireTokenAsync(serviceResourceId, clientId, returnUri, new AuthorizationParameters(activity));
    return authResult;
}  
```

W powyższym kodzie `AuthenticationContext` jest odpowiedzialny za uwierzytelniania z commonAuthority. Ma ona `AcquireTokenAsync` metodę, która przyjmować parametrów jako zasób, który należy udostępnić w takim przypadku `graphResourceUri`, `clientId`, i `returnUri`. Powrót do aplikacji `returnUri` po zakończeniu uwierzytelniania. Ten kod jest taka sama dla wszystkich platform, jednak ostatni parametr `AuthorizationParameters`, będzie różna na różnych platformach i jest odpowiedzialny za regulujące przepływ uwierzytelniania.

W przypadku systemu Android lub iOS jest przekazywana `this` parametr `AuthorizationParameters(this)` udostępnianie kontekstu, podczas gdy w systemie Windows jest przekazywany bez żadnych parametrów nowe `AuthorizationParameters()`.

### <a name="handle-continuation-for-android"></a>Dojście do kontynuacji dla systemu Android

Po zakończeniu uwierzytelniania przepływ powinien zwrócić do aplikacji. W przypadku systemu Android jest obsługiwany przez następujący kod, który powinien zostać dodany do **MainActivity.cs**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Dojście do kontynuacji dla Windows Phone

Windows Phone można zmodyfikować `OnActivated` metody w **App.xaml.cs** pliku z poniższego kodu:

```csharp
protected override void OnActivated(IActivatedEventArgs args)
{
#if WINDOWS_PHONE_APP
  if (args is IWebAuthenticationBrokerContinuationEventArgs)
  {
     WebAuthenticationBrokerContinuationHelper.SetWebAuthenticationBrokerContinuationEventArgs(args as IWebAuthenticationBrokerContinuationEventArgs);
  }
#endif
  base.OnActivated(args);
}
```

Teraz po uruchomieniu aplikacji powinna zostać wyświetlona dialog uwierzytelniania.
Po pomyślnym uwierzytelnieniu poprosi uprawnień do uzyskania dostępu do zasobów (w tym przypadku interfejsu API programu Graph):

![](graph-images/08.-authentication-flow.jpg "Po pomyślnym uwierzytelnieniu poprosi uprawnień do uzyskania dostępu do zasobów w tym przypadku interfejsu API programu Graph")

Jeśli uwierzytelnianie zakończy się pomyślnie i został autoryzowany aplikację, aby uzyskać dostęp do zasobów, należy pobrać `AccessToken` i `RefreshToken` kombi w `authResult`. Tokeny te są wymagane dla dalszego wywołań interfejsu API oraz autoryzacji w usłudze Azure Active Directory w tle.

![](graph-images/07.-access-token-for-authentication.jpg "Tokeny te są wymagane dla dalszego wywołań interfejsu API oraz autoryzacji w usłudze Azure Active Directory w tle")

Na przykład poniższy kod umożliwia pobranie listy użytkowników z usługi Active Directory. Adres URL interfejsu API sieci Web można zastąpić Twój interfejs API sieci Web, która jest chroniona przez usługę Azure AD.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

