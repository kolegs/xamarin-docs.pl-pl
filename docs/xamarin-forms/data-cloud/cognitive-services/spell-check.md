---
title: Sprawdzanie pisowni przy użyciu interfejsu API sprawdzania pisowni usługi Bing
description: Sprawdzania pisowni usługi Bing wykonuje kontekstowe pisowni sprawdzanie tekstu, zapewniając wbudowanego sugestie dotyczące pisowni. W tym artykule opisano sposób korzystania z usługi Bing pisowni Sprawdź interfejsu API REST do poprawy błędów pisowni w aplikacji platformy Xamarin.Forms.
ms.topic: article
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 420eea4622d9c90c3587899fb24e707524990b19
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/28/2018
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Sprawdzanie pisowni przy użyciu interfejsu API sprawdzania pisowni usługi Bing

_Sprawdzania pisowni usługi Bing wykonuje kontekstowe pisowni sprawdzanie tekstu, zapewniając wbudowanego sugestie dotyczące pisowni. W tym artykule opisano sposób korzystania z usługi Bing pisowni Sprawdź interfejsu API REST do poprawy błędów pisowni w aplikacji platformy Xamarin.Forms._

## <a name="overview"></a>Omówienie

Bing pisowni Sprawdź interfejsu API REST ma dwa tryby pracy i tryb należy określić podczas tworzenia żądania interfejsu API:

- `Spell` poprawia krótki tekst (słowa do 9) bez wprowadzania żadnych zmian wielkości liter.
- `Proof` poprawia długi tekst zawiera poprawki wielkość liter i znaków interpunkcyjnych podstawowe i pomija agresywne poprawki.

Należy uzyskać klucz interfejsu API do używania API sprawdzania pisowni usługi Bing. To znajduje się w [spróbuj kognitywnych usług](https://azure.microsoft.com/try/cognitive-services/)

Aby uzyskać listę języków obsługiwanych przez API sprawdzania pisowni usługi Bing, zobacz [obsługiwanych języków](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/). Aby uzyskać więcej informacji o interfejsie API sprawdzania pisowni usługi Bing, zobacz [dokumentacji sprawdzania pisowni usługi Bing](/azure/cognitive-services/bing-spell-check/).

## <a name="authentication"></a>Uwierzytelnianie

Wszystkie żądania skierowane do interfejsu API sprawdzania pisowni usługi Bing wymaga klucz interfejsu API, które powinny być określone jako wartość `Ocp-Apim-Subscription-Key` nagłówka. W poniższym przykładzie przedstawiono sposób dodawania klucz interfejsu API `Ocp-Apim-Subscription-Key` nagłówka żądania:

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Nie można przekazać prawidłowy klucz interfejsu API do interfejsu API sprawdzania pisowni usługi Bing spowoduje błąd odpowiedzi 401.

## <a name="performing-spell-checking"></a>Wykonywanie sprawdzania pisowni

Sprawdzanie pisowni, uzyskuje się poprzez żądanie GET lub POST `SpellCheck` interfejsu API w `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck`. Podczas tworzenia żądania GET, tekst, który ma być sprawdzenie pisowni jest wysyłany jako parametr zapytania. Podczas tworzenia żądania POST, tekst, który ma być sprawdzenie pisowni są wysyłane w treści żądania. Żądania GET to maksymalnie 1500 znaków z powodu ograniczenia długości ciągu parametru zapytania do sprawdzania pisowni. W związku z tym żądań POST zwykle należy chyba że krótkich ciągów są sprawdzenie pisowni.

W przykładowej aplikacji `SpellCheckTextAsync` metoda wywołuje proces sprawdzania pisowni:

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

`SpellCheckTextAsync` Metoda generuje identyfikator URI żądania, a następnie wysyła żądanie `SpellCheck` interfejsu API, który zwraca odpowiedź w formacie JSON zawierający wynik. Odpowiedź JSON jest rozszeregować z wynikiem zwracanych do wywoływania metody do wyświetlenia.

### <a name="configuring-spell-checking"></a>Konfigurowanie sprawdzanie pisowni

Proces sprawdzania pisowni można skonfigurować, określając parametry zapytania HTTP:

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

Aby uzyskać więcej informacji na temat Bing pisowni Sprawdź interfejsu API REST, zobacz [pisowni Sprawdź dokumentacja interfejsu API w wersji 7](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/).

### <a name="sending-the-request"></a>Wysyłanie żądania

`SendRequestAsync` Metoda zgłasza żądanie GET z usługi Bing pisowni Sprawdź interfejsu API REST i zwraca odpowiedź:

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Ta metoda tworzy żądanie GET, dodając klucz interfejsu API jako wartość `Ocp-Apim-Subscription-Key` nagłówka. Żądania GET są następnie wysyłane do `SpellCheck` interfejsu API, za pomocą adresu URL żądania Określanie tekstu do tłumaczenia i tryb sprawdzania pisowni. Odpowiedź jest następnie odczytu i powrót do wywoływania metody.

`SpellCheck` Interfejsu API będzie wysyłać kod stanu HTTP 200 (OK) w odpowiedzi, pod warunkiem, że żądanie jest prawidłowe, co oznacza, że żądanie zakończyło się pomyślnie i że żądane informacje są w odpowiedzi. Aby uzyskać listę obiektów odpowiedzi, zobacz [obiektów odpowiedzi](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects).

### <a name="processing-the-response"></a>Przetwarzanie odpowiedzi

Interfejs API odpowiedzi jest zwracany w formacie JSON. Następujące dane JSON zawiera komunikat odpowiedzi dla tekstu błędnie `Go shappin tommorow`:

```json
{  
   "_type":"SpellCheck",
   "flaggedTokens":[  
      {  
         "offset":3,
         "token":"shappin",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"shopping",
               "score":1
            }
         ]
      },
      {  
         "offset":11,
         "token":"tommorow",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"tomorrow",
               "score":1
            }
         ]
      }
   ],
   "correctionType":"High"
}
```

`flaggedTokens` Tablica zawiera tablicę wyrazy, które zostały oznaczone jako nie jest poprawna lub są niepoprawne gramatycznie w tekście. Tablica jest pusta, jeśli zostaną znalezione nie pisowni lub błędy gramatyczne. Tagi w tablicy są:

- `offset` — liczony od zera przesunięcie od początku ciągu tekstowego do programu word, który został oznaczony.
- `token` — słowa w ciągu tekstowym, która nie została wpisana poprawnie lub jest niepoprawne gramatycznie.
- `type` — Typ błędu, który spowodował oflagowane słowo. Istnieją dwie możliwe wartości — `RepeatedToken` i `UnknownToken`.
- `suggestions` — tablicę słowa, które będą Popraw błąd pisowni i gramatyki. Tablica składa się z `suggestion` i `score`, która wskazuje poziom ufności czy sugerowanej poprawki jest poprawna.

W przykładowej aplikacji odpowiedź w formacie JSON jest przeprowadzona deserializacja `SpellCheckResult` wystąpienia, w wyniku zwracanych do wywoływania metody do wyświetlenia. Poniższy kod przedstawia przykład sposobu `SpellCheckResult` wystąpienia są przetwarzane dla wyświetlania:

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

Ten kod iteruje `FlaggedTokens` kolekcji i zastępuje wszelkie błędne lub niepoprawne gramatycznie wyrazów w tekst źródłowy z pierwszego uwag. Poniższe zrzuty ekranu pokazują przed i po sprawdzanie pisowni:

![](spell-check-images/before-spell-check.png "Przed sprawdzanie pisowni")

![](spell-check-images/after-spell-check.png "Po sprawdzanie pisowni")

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak używać usługi Bing pisowni Sprawdź interfejsu API REST do Popraw pisownię w aplikacji platformy Xamarin.Forms. Sprawdzania pisowni usługi Bing wykonuje kontekstowe pisowni sprawdzanie tekstu, zapewniając wbudowanego sugestie dotyczące pisowni.

## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja sprawdzania pisowni usługi Bing](/azure/cognitive-services/bing-spell-check/)
- [Korzystanie z usługi sieci RESTful Web](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Usługi kognitywnych ToDo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Odwołanie do API sprawdzania pisowni usługi Bing w wersji 7](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
