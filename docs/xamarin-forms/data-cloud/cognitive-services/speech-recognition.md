---
title: "Rozpoznawanie mowy przy użyciu interfejsu API mowy usługi Bing"
description: "Interfejs API mowy usługi Bing jest oparta na chmurze interfejsu API udostępniający algorytmy przetwarzania używany. W tym artykule opisano sposób użycia interfejsu API REST rozpoznawania mowy usługi Bing można przekonwertować na tekst w aplikacji platformy Xamarin.Forms audio."
ms.topic: article
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 186ea6277ec7bd4ceb52855186e6fd88344b1b86
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="speech-recognition-using-the-bing-speech-api"></a>Rozpoznawanie mowy przy użyciu interfejsu API mowy usługi Bing

_Interfejs API mowy usługi Bing jest oparta na chmurze interfejsu API udostępniający algorytmy przetwarzania używany. W tym artykule opisano sposób użycia interfejsu API REST rozpoznawania mowy usługi Bing można przekonwertować na tekst w aplikacji platformy Xamarin.Forms audio._

![](~/media/shared/preview.png "Ten interfejs API jest obecnie wersji wstępnej")

> [!NOTE]
> Interfejs API mowy usługi Bing jest wciąż w wersji zapoznawczej. Może być istotne zmiany przed ostateczną wersją interfejsu API.

## <a name="overview"></a>Omówienie

Interfejs API mowy usługi Bing ma dwa składniki:

- Rozpoznawanie mowy interfejsu API do konwertowania wypowiadane słowa na tekst. Rozpoznawanie mowy można wykonać za pomocą interfejsu API REST, klient biblioteki lub biblioteki usługi.
- Tekst na mowę interfejsu API konwersji tekstu na rozmowy słowa. Tekst na mowę konwersji odbywa się za pośrednictwem interfejsu API REST.

Ten artykuł skupia się na wykonywanie rozpoznawania mowy za pomocą interfejsu API REST. Gdy biblioteki klienta i usługi obsługują zwracania wyników częściowych, interfejsu API REST może zwracać tylko w wyniku pojedynczy rozpoznawania bez żadnych wyników częściowych.

Należy uzyskać klucz interfejsu API za pomocą interfejsu API rozpoznawania mowy usługi Bing. To znajduje się w [wprowadzenie bezpłatnie](https://www.microsoft.com/cognitive-services/sign-up?ReturnUrl=/cognitive-services/subscriptions?productId=%2fproducts%2fBing.Speech.Preview) w witrynie microsoft.com.

Aby uzyskać więcej informacji na temat interfejsu API mowy usługi Bing, zobacz [dokumentacji mowy usługi Bing](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview) w witrynie microsoft.com.

## <a name="authentication"></a>Uwierzytelnianie

Wszystkie żądania skierowane do interfejsu API REST rozpoznawania mowy usługi Bing wymaga tokenu dostępu tokenu Web JSON (JWT), który można uzyskać z usługi tokenu usługi kognitywnych w `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Tokenu można uzyskać poprzez wysłanie żądania POST do usługi tokenu, określając `Ocp-Apim-Subscription-Key` nagłówek, który zawiera klucz interfejsu API, jako jego wartość.

Poniższy przykładowy kod przedstawia sposób żądania dostępu do tokenu z usługi tokenu:

```csharp
async Task<string> FetchTokenAsync(string fetchUri, string apiKey)
{
  using (var client = new HttpClient())
  {
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";

    var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
  }
}
```

Token dostępu zwrócone, czyli tekstu Base64, ma czasu wygaśnięcia 10 minut. W związku z tym przykładowej aplikacji odnawia tokenu dostępu, co 9 minut.

Należy określić token dostępu w każdym API REST rozpoznawania mowy usługi Bing wywołać jako `Authorization` nagłówka poprzedzona ciągiem `Bearer`, jak pokazano w poniższym przykładzie:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Nie można przekazać tokenu prawomocny dostęp do interfejsu API REST rozpoznawania mowy usługi Bing spowoduje błąd 403 odpowiedzi.

## <a name="performing-speech-recognition"></a>Wykonywanie rozpoznawania mowy

Rozpoznawanie mowy uzyskuje się poprzez wysłanie żądania POST na `recognize` interfejsu API w `https://speech.platform.bing.com/recognize`. Pojedyncze żądanie nie może zawierać więcej niż 10 sekund dźwięku, a żądanie całkowity czas trwania nie może przekraczać 14 sekund.

Zawartość audio muszą znajdować się w treści żądania w formacie wav POST. Aby uzyskać informacje o obsługiwanych koderów-dekoderów, zobacz [kodery-dekodery obsługiwane](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-codecs) w witrynie microsoft.com.

W przykładowej aplikacji `RecognizeSpeechAsync` procesu rozpoznawania mowy wywołuje metodę:

```csharp
public async Task<SpeechResult> RecognizeSpeechAsync(string filename)
{
    ...

    // Read audio file to a stream
    var file = await PCLStorage.FileSystem.Current.LocalStorage.GetFileAsync(filename);
    var fileStream = await file.OpenAsync(PCLStorage.FileAccess.Read);

    // Send audio stream to Bing and deserialize the response
    string requestUri = GenerateRequestUri(Constants.SpeechRecognitionEndpoint);
    string accessToken = authenticationService.GetAccessToken();
    var response = await SendRequestAsync(fileStream, requestUri, accessToken, Constants.AudioContentType);
    var speechResults = JsonConvert.DeserializeObject<SpeechResults>(response);

    fileStream.Dispose();
    return speechResults.results.FirstOrDefault();
}
```

Dźwięk jest rejestrowany w każdym projekcie specyficzne dla platformy jako dane wav PCM i `RecognizeSpeechAsync` używa metody `PCLStorage` pakietu NuGet, aby otworzyć plik dźwiękowy jako strumień. Żądanie rozpoznawania mowy wygenerowania identyfikatora URI i token dostępu są pobierane z usługi tokenu. Żądanie rozpoznawania mowy publikowanego `recognize` interfejsu API, który zwraca odpowiedź w formacie JSON zawierający wynik. Odpowiedź JSON jest rozszeregować z wynikiem zwracanych do wywoływania metody do wyświetlenia.

### <a name="configuring-speech-recognition"></a>Konfigurowanie rozpoznawania mowy

Proces rozpoznawania mowy można skonfigurować, określając parametry zapytania HTTP. Dostępne są obowiązkowe i opcjonalne parametry, przy użyciu następującej metody przedstawiający obowiązkowe parametry, które musi być ustawiona:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    string requestUri = speechEndpoint;
    requestUri += @"?scenarios=ulm";                                    // websearch is the other option
    requestUri += @"&appid=D4D52672-91D7-4C74-8AD8-42B1D98141A5";       // You must use this ID
    requestUri += @"&locale=en-US";                                     // Other languages supported
    requestUri += string.Format("&device.os={0}", operatingSystem);     // Open field
    requestUri += @"&version=3.0";                                      // Required value
    requestUri += @"&format=json";                                      // Required value
    requestUri += @"&instanceid=fe34a4de-7927-4e24-be60-f0629ce1d808";  // GUID for device making the request
    requestUri += @"&requestid=" + Guid.NewGuid().ToString();           // GUID for the request
    return requestUri;
}
```

Konfiguracja głównego wykonywane przez `GenerateRequestUri` metodą jest skonfigurowanie ustawień regionalnych zawartości audio. Aby uzyskać listę obsługiwanych ustawień regionalnych, zobacz [obsługiwanych ustawień regionalnych ](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-locales) w witrynie microsoft.com.

Aby uzyskać więcej informacji na temat możliwych wartości parametrów obowiązkowych, zobacz [wymagane parametry](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#required-parameters) w witrynie microsoft.com. Aby uzyskać informacje o następujące parametry opcjonalne, zobacz [następujące parametry opcjonalne](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition) w witrynie microsoft.com.

### <a name="sending-the-request"></a>Wysyłanie żądania

`SendRequestAsync` Metoda wysyła żądanie POST do interfejsu API REST rozpoznawania mowy usługi Bing i zwraca odpowiedź:

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);

    using (var httpClient = new HttpClient())
    {
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
        var response = await httpClient.PostAsync(url, content);

        return await response.Content.ReadAsStringAsync();
    }
}
```

Ta metoda tworzy żądanie POST przez:

- Zawijanie strumieniem audio w `StreamContent` wystąpienia, która udostępnia zawartość HTTP w oparciu o strumień.
- Ustawienie `Content-Type` nagłówka żądania `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Dodawanie tokenu dostępu do `Authorization` nagłówka poprzedzona ciągiem `Bearer`.

Następnie wysłaniu żądania POST do `recognize` interfejsu API. Odpowiedź jest następnie odczytu i powrót do wywoływania metody.

`recognize` Interfejsu API będzie wysyłać kod stanu HTTP 200 (OK) w odpowiedzi, pod warunkiem, że żądanie jest prawidłowe, co oznacza, że żądanie zakończyło się pomyślnie i że żądane informacje są w odpowiedzi. Lista może zawierać błąd odpowiedzi, zobacz [odpowiedzi na błędy](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#error-responses) w witrynie microsoft.com.

### <a name="processing-the-response"></a>Przetwarzanie odpowiedzi

Interfejs API zwracania odpowiedzi w formacie JSON z rozpoznany tekst jest zawarty w `name` tagu. Następujące dane JSON pokazuje komunikat typowe pomyślnej odpowiedzi:

```csharp
{
  "version": "3.0",
  "header": {
    "status": "success",
    "scenario": "ulm",
    "name": "go shopping tomorrow",
    "lexical": "go shopping tomorrow",
    "properties": {
      "requestid": "e06c059d-fa37-4bb1-843f-4914350279a8",
      "HIGHCONF": "1"
    }
  },
  "results": [
    {
      "scenario": "ulm",
      "name": "go shopping tomorrow",
      "lexical": "go shopping tomorrow",
      "confidence": "0.9493451",
      "properties": {
        "HIGHCONF": "1"
      }
    }
  ]
}
```

W przykładowej aplikacji odpowiedź w formacie JSON jest przeprowadzona deserializacja `SpeechResult` wystąpienia, w wyniku zwracanych do wywoływania metody do wyświetlania, jak pokazano na poniższych zrzutach ekranu:

![](speech-recognition-images/speech-recognition.png "Rozpoznawanie mowy")

Aby uzyskać informacje o wartości tagami JSON, zobacz [odpowiedzi rozpoznawania mowy](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#speech-recognition-responses) w witrynie microsoft.com.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób użycia interfejsu API REST rozpoznawania mowy usługi Bing można przekonwertować na tekst w aplikacji platformy Xamarin.Forms audio. Oprócz przeprowadzania rozpoznawania mowy, interfejsu API mowy usługi Bing można przekonwertować tekst na rozmowy słowa.



## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja mowy usługi Bing](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview)
- [Korzystanie z usługi sieci RESTful Web](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Usługi kognitywnych ToDo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Rozpoznawanie mowy usługi Bing interfejsu API REST](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition)
