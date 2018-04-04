---
title: Tłumaczenie tekstu przy użyciu translatora interfejsu API
description: Interfejs API Translator Microsoft może służyć do tłumaczenia mowy i tekstowym za pośrednictwem interfejsu API REST. W tym artykule opisano sposób użycia interfejsu API usługi Microsoft Translator tekstu umożliwia tłumaczenie tekstu z jednego języka do innego w aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 5c1001335fb030f9a91ec72456042316864ccf5c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="text-translation-using-the-translator-api"></a>Tłumaczenie tekstu przy użyciu translatora interfejsu API

_Interfejs API Translator Microsoft może służyć do tłumaczenia mowy i tekstowym za pośrednictwem interfejsu API REST. W tym artykule opisano sposób użycia interfejsu API usługi Microsoft Translator tekstu umożliwia tłumaczenie tekstu z jednego języka do innego w aplikacji platformy Xamarin.Forms._

## <a name="overview"></a>Omówienie

Translator interfejsu API zawiera dwa składniki:

- Tłumaczenie tekstu interfejsu API REST umożliwia tłumaczenie tekstu z jednego języka na tekst innego języka. Interfejs API automatycznie wykrywa język tekstu, który został wysłany przed jego tłumaczenia.
- Tłumaczenie mowy interfejsu API REST wykonać transkrypcji mowy z jednego języka na tekst innego języka. Interfejs API integruje się również mowę porozmawiać tłumaczenia ponownie.

Ten artykuł skupia się na tłumaczenie tekstu z jednego języka do innego przy użyciu interfejsu API tekst translatora.

Należy uzyskać klucz interfejsu API za pomocą interfejsu API Translator tekstu. To znajduje się w [jak zalogowania się do interfejsu API usługi Microsoft Translator tekstu](/azure/cognitive-services/translator/translator-text-how-to-signup/).

Aby uzyskać więcej informacji na temat interfejsu API Microsoft Translator tekstu, zobacz [dokumentacji interfejsu API tekst Translator](/azure/cognitive-services/translator/).

## <a name="authentication"></a>Uwierzytelnianie

Wszystkie żądania skierowane do interfejsu API tekst Translator wymaga tokenu dostępu tokenu Web JSON (JWT), który można uzyskać z usługi tokenu usługi kognitywnych w `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Tokenu można uzyskać poprzez wysłanie żądania POST do usługi tokenu, określając `Ocp-Apim-Subscription-Key` nagłówek, który zawiera klucz interfejsu API, jako jego wartość.

Poniższy przykładowy kod przedstawia sposób żądania dostępu do tokenu z usługi tokenu:

```csharp
public AuthenticationService(string apiKey)
{
    subscriptionKey = apiKey;
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
}
...
async Task<string> FetchTokenAsync(string fetchUri)
{
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";
    var result = await httpClient.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
}
```

Token dostępu zwrócone, czyli tekstu Base64, ma czasu wygaśnięcia 10 minut. W związku z tym przykładowej aplikacji odnawia tokenu dostępu, co 9 minut.

Token dostępu musi być określony w każdej API tekst Translator wywołać jako `Authorization` nagłówka poprzedzona ciągiem `Bearer`, jak pokazano w poniższym przykładzie:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Aby uzyskać więcej informacji na temat kognitywnych tokenu usługi, zobacz [interfejs API tokenu uwierzytelniania](http://docs.microsofttranslator.com/oauth-token.html).

## <a name="performing-text-translation"></a>Wykonuje translację tekstu

Tłumaczenie tekstu uzyskuje się poprzez żądanie GET `translate` interfejsu API w `https://api.microsofttranslator.com/v2/http.svc/translate`. W przykładowej aplikacji `TranslateTextAsync` metoda wywołuje proces translacji tekst:

```csharp
public async Task<string> TranslateTextAsync(string text)
{
  ...
  string requestUri = GenerateRequestUri(Constants.TextTranslatorEndpoint, text, "en", "de");
  string accessToken = authenticationService.GetAccessToken();
  var response = await SendRequestAsync(requestUri, accessToken);
  var xml = XDocument.Parse(response);
  return xml.Root.Value;
}
```

`TranslateTextAsync` Metoda generuje identyfikator URI żądania i pobiera token dostępu z usługi tokenu. Żądanie tłumaczenie tekstu jest następnie wysyłane do `translate` interfejsu API, który zwraca odpowiedź XML zawierający wynik. Analizowania odpowiedzi XML, a wynik tłumaczenia zostanie zwrócony do wywoływania metody do wyświetlenia.

Aby uzyskać więcej informacji na temat interfejsów API REST tłumaczenie tekstu, zobacz [interfejsu API usługi Microsoft Translator tekstu](http://docs.microsofttranslator.com/text-translate.html).

### <a name="configuring-text-translation"></a>Konfigurowanie tłumaczenie tekstu

Proces translacji tekst można skonfigurować, określając parametry zapytania HTTP:

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

Ta metoda ustawia tekst, który ma zostać poddany translacji i języka umożliwia tłumaczenie tekstu do. Aby uzyskać listę języków obsługiwanych przez Microsoft Translator, zobacz [obsługiwanych języków w interfejsie API Microsoft Translator tekstu](/azure/cognitive-services/translator/languages/).

> [!NOTE]
> Jeśli aplikacja musi wiedzieć, język tekstu jest w `Detect` można wywołać interfejsu API w celu wykrywania języka ciąg tekstowy.

### <a name="sending-the-request"></a>Wysyłanie żądania

`SendRequestAsync` Metoda zgłasza żądanie GET interfejsu API REST tłumaczenie tekstu i zwraca odpowiedź:

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Ta metoda tworzy żądanie GET, dodając tokenu dostępu do `Authorization` nagłówka poprzedzona ciągiem `Bearer`. Żądania GET są następnie wysyłane do `translate` o adresie URL żądania Określanie tekstu do tłumaczenia i języka umożliwia tłumaczenie tekstu do interfejsu API. Odpowiedź jest następnie odczytu i powrót do wywoływania metody.

`translate` Interfejsu API będzie wysyłać kod stanu HTTP 200 (OK) w odpowiedzi, pod warunkiem, że żądanie jest prawidłowe, co oznacza, że żądanie zakończyło się pomyślnie i że żądane informacje są w odpowiedzi. Lista może zawierać błąd odpowiedzi, zobacz wiadomości odpowiedzi na [UZYSKAĆ tłumaczenie](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate).

### <a name="processing-the-response"></a>Przetwarzanie odpowiedzi

Interfejs API odpowiedzi jest zwracany w formacie XML. Następujące dane XML zawiera komunikat typowe pomyślnej odpowiedzi:

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

W przykładowej aplikacji odpowiedzi XML jest analizowana w `XDocument` wystąpienia o wartości głównego XML zwracanych do wywoływania metody do wyświetlania, jak pokazano na poniższych zrzutach ekranu:

![](text-translation-images/text-translation.png "Tłumaczenie tekstu na język niemiecki")

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób użycia interfejsu API usługi Microsoft Translator tekstu umożliwia tłumaczenie tekstu z jednego języka na tekst innego języka w aplikacji platformy Xamarin.Forms. Oprócz tłumaczenie tekstu, interfejsu API usługi Microsoft Translator można również wykonać transkrypcji mowy z jednego języka, na tekst innego języka.

## <a name="related-links"></a>Linki pokrewne

- [Translator dokumentacji interfejsu API tekstu](/azure/cognitive-services/translator/).
- [Korzystanie z usługi sieci RESTful Web](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Usługi kognitywnych ToDo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Tekst Microsoft Translator interfejsu API](http://docs.microsofttranslator.com/text-translate.html).
