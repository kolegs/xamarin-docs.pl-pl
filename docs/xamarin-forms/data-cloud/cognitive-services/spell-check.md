---
title: "Sprawdzanie pisowni przy użyciu interfejsu API sprawdzania pisowni usługi Bing"
description: "Sprawdzania pisowni usługi Bing wykonuje kontekstowe pisowni sprawdzanie tekstu, zapewniając wbudowanego sugestie dotyczące pisowni. W tym artykule opisano sposób korzystania z usługi Bing pisowni Sprawdź interfejsu API REST do poprawy błędów pisowni w aplikacji platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: ad2bdf27323fd7d7e108a25387cd6aea6d442098
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Sprawdzanie pisowni przy użyciu interfejsu API sprawdzania pisowni usługi Bing

_Sprawdzania pisowni usługi Bing wykonuje kontekstowe pisowni sprawdzanie tekstu, zapewniając wbudowanego sugestie dotyczące pisowni. W tym artykule opisano sposób korzystania z usługi Bing pisowni Sprawdź interfejsu API REST do poprawy błędów pisowni w aplikacji platformy Xamarin.Forms._

## <a name="overview"></a>Omówienie

Bing pisowni Sprawdź interfejsu API REST ma dwa tryby pracy i tryb należy określić podczas tworzenia żądania interfejsu API:

- `Spell` 
- `Proof` 

Należy uzyskać klucz interfejsu API do używania API sprawdzania pisowni usługi Bing. To znajduje się w [wprowadzenie bezpłatnie](https://www.microsoft.com/cognitive-services/sign-up?ReturnUrl=/cognitive-services/subscriptions?productId=%2fproducts%2fBing.Speech.Preview) w witrynie microsoft.com.

Aby uzyskać listę języków obsługiwanych przez API sprawdzania pisowni usługi Bing, zobacz [obsługę języka](https://www.microsoft.com/cognitive-services/Bing-Spell-check-API/documentation#language-support) w witrynie microsoft.com. Aby uzyskać więcej informacji o interfejsie API sprawdzania pisowni usługi Bing, zobacz [API sprawdzania pisowni usługi Bing](https://www.microsoft.com/cognitive-services/bing-spell-check-api/documentation) w witrynie microsoft.com.

## <a name="authentication"></a>Uwierzytelnianie

Wszystkie żądania skierowane do interfejsu API sprawdzania pisowni usługi Bing wymaga klucz interfejsu API, które powinny być określone jako wartość `Ocp-Apim-Subscription-Key` nagłówka. W poniższym przykładzie przedstawiono sposób dodawania klucz interfejsu API `Ocp-Apim-Subscription-Key` nagłówka żądania:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
  ...
}
```

Nie można przekazać prawidłowy klucz interfejsu API do interfejsu API sprawdzania pisowni usługi Bing spowoduje błąd odpowiedzi 401.

## <a name="performing-spell-checking"></a>Wykonywanie sprawdzania pisowni

Sprawdzanie pisowni, uzyskuje się poprzez żądanie GET lub POST `SpellCheck` interfejsu API w `https://api.cognitive.microsoft.com/bing/v5.0/SpellCheck`. Podczas tworzenia żądania GET, tekst, który ma być sprawdzenie pisowni jest wysyłany jako parametr zapytania. Podczas tworzenia żądania POST, tekst, który ma być sprawdzenie pisowni są wysyłane w treści żądania. Żądania GET to maksymalnie 1500 znaków z powodu ograniczenia długości ciągu parametru zapytania do sprawdzania pisowni. W związku z tym żądań POST zwykle zostaną wprowadzone, chyba że krótkich ciągów są sprawdzenie pisowni.

W przykładowej aplikacji `SpellCheckTextAsync` metoda wywołuje proces sprawdzania pisowni:

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
  string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
  var response = await SendRequestAsync(requestUri, Constants.BingSpellCheckApiKey);
  var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
  return spellCheckResults;
}
```

`SpellCheckTextAsync` Metoda generuje identyfikator URI żądania, a następnie wysyła żądanie `SpellCheck` interfejsu API, który zwraca odpowiedź w formacie JSON zawierający wynik. Odpowiedź JSON jest rozszeregować z wynikiem zwracanych do wywoływania metody do wyświetlenia.

Aby uzyskać więcej informacji na temat Bing pisowni Sprawdź interfejsu API REST, zobacz [API sprawdzania pisowni](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358) w witrynie microsoft.com.

### <a name="configuring-spell-checking"></a>Konfigurowanie sprawdzanie pisowni

Proces sprawdzania pisowni można skonfigurować, określając parametry zapytania HTTP. Parametry obowiązkowe i opcjonalne, przy użyciu następującej metody przedstawiający obowiązkowe parametry, które musi być ustawiona dla żądania GET są:

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

Ta metoda ustawia tekst, który ma być sprawdzenie pisowni i tryb sprawdzania pisowni.

Aby uzyskać więcej informacji o parametrach obowiązkowe i opcjonalne, zobacz [API sprawdzania pisowni](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358) w witrynie microsoft.com.

### <a name="sending-the-request"></a>Wysyłanie żądania

`SendRequestAsync` Metoda zgłasza żądanie GET z usługi Bing pisowni Sprawdź interfejsu API REST i zwraca odpowiedź:

```csharp
async Task<string> SendRequestAsync(string url, string apiKey)
{
  using (var httpClient = new HttpClient())
  {
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
  }
}
```

Ta metoda tworzy żądanie GET, dodając klucz interfejsu API jako wartość `Ocp-Apim-Subscription-Key` nagłówka. Żądania GET są następnie wysyłane do `SpellCheck` interfejsu API, za pomocą adresu URL żądania Określanie tekstu do tłumaczenia i tryb sprawdzania pisowni. Odpowiedź jest następnie odczytu i powrót do wywoływania metody.

`SpellCheck` Interfejsu API będzie wysyłać kod stanu HTTP 200 (OK) w odpowiedzi, pod warunkiem, że żądanie jest prawidłowe, co oznacza, że żądanie zakończyło się pomyślnie i że żądane informacje są w odpowiedzi. Lista może zawierać błąd odpowiedzi, zobacz odpowiedzi na [API sprawdzania pisowni](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358) w witrynie microsoft.com.

### <a name="processing-the-response"></a>Przetwarzanie odpowiedzi

Interfejs API odpowiedzi jest zwracany w formacie JSON. Następujące dane JSON zawiera komunikat odpowiedzi dla tekstu błędnie `Go shappin tommorow`:

```csharp
{
  "_type": "SpellCheck",
  "flaggedTokens": [
    {
      "offset": 3,
      "token": "shappin",
      "type": "UnknownToken",
      "suggestions": [
        {
          "suggestion": "shopping",
          "score": 1
        }
      ]
    },
    {
      "offset": 11,
      "token": "tommorow",
      "type": "UnknownToken",
      "suggestions": [
        {
          "suggestion": "tomorrow",
          "score": 1
        }
      ]
    }
  ]
}
```

`flaggedTokens` Tablica zawiera tablicę wyrazy, które zostały oznaczone jako nie jest poprawna lub są niepoprawne gramatycznie w tekście. Tablica jest pusta, jeśli zostaną znalezione nie pisowni lub błędy gramatyczne. Tagi w tablicy są:

- `offset` 
- `token` 
- `type`  Istnieją dwie możliwe wartości — `RepeatedToken` i `UnknownToken`.
- `suggestions`  Tablica składa się z `suggestion` i `score`, która wskazuje poziom ufności czy sugerowanej poprawki jest poprawna.

W przykładowej aplikacji odpowiedź w formacie JSON jest przeprowadzona deserializacja `SpellCheckResult` wystąpienia, w wyniku zwracanych do wywoływania metody do wyświetlenia. Poniższy kod przedstawia przykład sposobu `SpellCheckResult` wystąpienia są przetwarzane dla wyświetlania:

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

Ten kod iteruje `FlaggedTokens` kolekcji i zastępuje wszelkie błędne lub niepoprawne gramatycznie wyrazów w tekst źródłowy z pierwszego uwag. Poniższe zrzuty ekranu pokazują przed i po sprawdzanie pisowni:

![](spell-check-images/before-spell-check.png "")

![](spell-check-images/after-spell-check.png "")

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak używać usługi Bing pisowni Sprawdź interfejsu API REST do Popraw pisownię w aplikacji platformy Xamarin.Forms. Sprawdzania pisowni usługi Bing wykonuje kontekstowe pisowni sprawdzanie tekstu, zapewniając wbudowanego sugestie dotyczące pisowni.



## <a name="related-links"></a>Linki pokrewne

- [](https://www.microsoft.com/cognitive-services/bing-spell-check-api/documentation)
- [Korzystanie z usługi sieci RESTful Web](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Usługi kognitywnych ToDo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358)
