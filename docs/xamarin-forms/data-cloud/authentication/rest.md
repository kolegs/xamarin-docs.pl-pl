---
title: "Uwierzytelnianie usługi sieci RESTful Web"
description: "HTTP obsługuje kilka mechanizmów uwierzytelniania w celu kontroli dostępu do zasobów. Uwierzytelnianie podstawowe zapewnia dostęp do zasobów tylko do klientów mających prawidłowe poświadczenia. W tym artykule przedstawiono sposób użycia uwierzytelniania podstawowego do ochrony dostępu do zasobów usługi sieci web RESTful."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 7aea74f95e8738cc415eaac3a5ac4f86b069d0f7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="authenticating-a-restful-web-service"></a>Uwierzytelnianie usługi sieci RESTful Web

_HTTP obsługuje kilka mechanizmów uwierzytelniania w celu kontroli dostępu do zasobów. Uwierzytelnianie podstawowe zapewnia dostęp do zasobów tylko do klientów mających prawidłowe poświadczenia. W tym artykule przedstawiono sposób użycia uwierzytelniania podstawowego do ochrony dostępu do zasobów usługi sieci web RESTful._

Towarzyszący przykładowej aplikacji platformy Xamarin.Forms zużywa usługa hostowana Xamarin REST, która umożliwia dostęp tylko do odczytu do usługi sieci web. W związku z tym operacje, które tworzenie, aktualizowanie i usuwanie danych nie ma wpływu czy dane używane w aplikacji. Jednak jest dostępna w wersji pełnić rolę hosta usługi REST *TodoRESTService* można znaleźć folderu w przykładowej aplikacji i instrukcje dotyczące konfigurowania usługi. Ta wersja pełnić rolę hosta usługi REST zapewnia pełne tworzenia, aktualizacji, odczytu i usuwania dostępu do danych.

> [!NOTE]
> W systemie iOS 9 i większości aplikacji zabezpieczeń transportu (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych. Ponieważ ATS jest domyślnie włączone w aplikacje dla systemu iOS 9, wszystkie połączenia będą podlegać ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.
> ATS można można korzystać z, jeśli nie jest możliwe użycie `HTTPS` protokołu i zabezpieczania komunikacji dla zasobów w Internecie. Można to osiągnąć przez zaktualizowanie aplikacji **Info.plist** pliku. Aby uzyskać więcej informacji, zobacz [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md).

## <a name="authenticating-users-over-http"></a>Uwierzytelnianie użytkowników za pośrednictwem protokołu HTTP

Uwierzytelnianie podstawowe jest najprostsza mechanizmu uwierzytelniania obsługiwanych przez protokół HTTP i obejmuje klient wysyła nazwę użytkownika i hasło jako tekst niezaszyfrowane kodowany w standardzie base64. Działa on w następujący sposób:

- Jeśli usługi sieci web odebrał żądanie zasobu chronionego, odrzuca żądania z kodem stanu HTTP 401 (odmowa dostępu) i ustawia nagłówka WWW-Authenticate odpowiedzi, jak pokazano na poniższym diagramie:

![](rest-images/basic-authentication-fail.png "Niepowodzenie uwierzytelniania podstawowego")

- Jeśli usługa sieci web otrzymuje żądanie zasobu chronionego z `Authorization` nagłówka poprawnie ustawiona, wyświetlania odpowiada z usługi sieci web z kodem stanu HTTP 200, co oznacza, że żądanie zakończyło się pomyślnie i że żądane informacje są w odpowiedzi. Taki scenariusz przedstawiono na poniższym diagramie:

![](rest-images/basic-authentication-success.png "Pomyślne wykonanie uwierzytelniania podstawowego")

> [!NOTE]
> Uwierzytelnianie podstawowe należy używać tylko za pośrednictwem połączenia HTTPS. Gdy jest używany przez połączenie HTTP <code>Authorization</code> nagłówka łatwo może zostać odczytany, jeśli ruch HTTP są przechwytywane przez osobę atakującą.

## <a name="specifying-basic-authentication-in-a-web-request"></a>Określanie uwierzytelnianie podstawowe w żądania sieci Web

Korzystanie z uwierzytelniania podstawowego jest określony w następujący sposób:

1. Ciąg "Basic" zostanie dodany do `Authorization` nagłówka żądania.
1. Nazwa użytkownika i hasło są łączone w ciąg w formacie "USERNAME", który jest następnie kodowany i dodane do w standardzie base64 `Authorization` nagłówka żądania.

W związku z tym nazwę użytkownika "XamarinUser" i hasło "XamarinPassword" Nagłówek staje się:

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

`HttpClient` Klasa może ustawić `Authorization` wartość nagłówka w `HttpClient.DefaultRequestHeaders.Authorization` właściwości. Ponieważ `HttpClient` wystąpienie istnieje wiele żądań `Authorization` nagłówka tylko musi być ustawiona, zamiast podczas wprowadzania każde żądanie, jak pokazano w poniższym przykładzie:

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    client = new HttpClient ();
    ...
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

Następnie po wysłaniu żądania operacji usługi sieci web żądania jest podpisany za pomocą `Authorization` nagłówka, wskazującą, czy użytkownik ma uprawnienia do wywoływania operacji.

> [!NOTE]
> Gdy usługa REST próbki przechowuje poświadczenia jako stałe, nie powinny być przechowywane w formacie niezabezpieczonych w opublikowanej aplikacji. [Xamarith.Auth](https://www.nuget.org/packages/Xamarin.Auth/) NuGet udostępnia funkcję dla bezpieczne przechowywanie poświadczeń. Aby uzyskać więcej informacji, zobacz [przechowywanie i pobieranie informacji o koncie na urządzeniach](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="processing-the-authorization-header-server-side"></a>Przetwarzania po stronie serwera nagłówek autoryzacji

Towarzyszące usługi REST próbki decorates każdej akcji z `[BasicAuthentication]` atrybutu. Ten atrybut jest implementowany przez `BasicAuthenticationAttribute` klasy w rozwiązaniu i jest używany do analizowania `Authorization` nagłówka i ustalenia, czy kodowanie base64 poświadczenia są prawidłowe względem wartości przechowywane w *Web.config*. Gdy ta metoda jest odpowiedni dla przykładowej usługi, wymaga rozszerzenia usługi sieci web publicznych.

W module uwierzytelnianie podstawowe, używany przez usługi IIS użytkownicy są uwierzytelniani poświadczeń systemu Windows. W związku z tym użytkownicy muszą mieć konta w domenie serwera. Jednak model podstawowego uwierzytelniania można skonfigurować do Zezwalaj na niestandardowe uwierzytelnianie, gdy konta użytkowników są uwierzytelniani źródła zewnętrznego, na przykład w bazie danych. Aby uzyskać więcej informacji, zobacz [uwierzytelnianie podstawowe w interfejsie API sieci Web ASP.NET](http://www.asp.net/web-api/overview/security/basic-authentication) w witrynie sieci Web programu ASP.NET.

> [!NOTE]
> Uwierzytelnianie podstawowe nie jest przeznaczone do zarządzania wylogowaniu. W związku z tym wylogowaniu metoda standardowe uwierzytelnianie podstawowe jest aby zakończyć sesję.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób dodawania uwierzytelnianie podstawowe do żądań sieci web za pomocą aplikacji platformy Xamarin.Forms `HttpClient` klasy. Uwierzytelnianie podstawowe zapewnia dostęp do zasobów tylko do klientów mających prawidłowe poświadczenia. Aby uzyskać informacje o sposobie używania [Xamarin.Auth](https://www.nuget.org/packages/Xamarin.Auth/) Aby zarządzać procesu uwierzytelniania w aplikacji platformy Xamarin.Forms, dzięki czemu użytkownicy mogą udostępniać wewnętrznej bazie danych podczas tylko mieli dostęp do swoich danych, zobacz [uwierzytelniania użytkowników przy użyciu dostawcy tożsamości](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="related-links"></a>Linki pokrewne

- [TodoREST (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [Korzystanie z usługi sieci RESTful web](~/xamarin-forms/data-cloud/consuming/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
