---
title: Rozpoznawanie mowy przy użyciu interfejsu API mowy firmy Microsoft
description: Interfejs API mowy firmy Microsoft jest oparte na chmurze interfejs API, który zawiera algorytmy przetwarzania używany. W tym artykule opisano sposób użycia interfejsu API REST rozpoznawania mowy firmy Microsoft można przekonwertować na tekst w aplikacji platformy Xamarin.Forms audio.
ms.topic: article
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 2230b11f9553fb779a86d7504ed5507d2e7cbaa7
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/28/2018
---
# <a name="speech-recognition-using-the-microsoft-speech-api"></a>Rozpoznawanie mowy przy użyciu interfejsu API mowy firmy Microsoft

_Interfejs API mowy firmy Microsoft jest oparte na chmurze interfejs API, który zawiera algorytmy przetwarzania używany. W tym artykule opisano sposób użycia interfejsu API REST rozpoznawania mowy firmy Microsoft można przekonwertować na tekst w aplikacji platformy Xamarin.Forms audio._

## <a name="overview"></a>Omówienie

Microsoft Speech API zawiera dwa składniki:

- Rozpoznawanie mowy interfejsu API do konwertowania wypowiadane słowa na tekst. Rozpoznawanie mowy można wykonać za pomocą interfejsu API REST, klient biblioteki lub biblioteki usługi.
- Tekst na mowę interfejsu API konwersji tekstu na rozmowy słowa. Tekst na mowę konwersji odbywa się za pośrednictwem interfejsu API REST.

Ten artykuł skupia się na wykonywanie rozpoznawania mowy za pomocą interfejsu API REST. Gdy biblioteki klienta i usługi obsługują zwracania wyników częściowych, interfejsu API REST może zwracać tylko w wyniku pojedynczy rozpoznawania bez żadnych wyników częściowych.

Należy uzyskać klucz interfejsu API za pomocą interfejsu API mowy firmy Microsoft. To znajduje się w [spróbuj kognitywnych usług](https://azure.microsoft.com/try/cognitive-services/).

Aby uzyskać więcej informacji o interfejsie API mowy firmy Microsoft, zobacz [dokumentacji interfejsu API mowy Microsoft](/azure/cognitive-services/speech/home/).

## <a name="authentication"></a>Uwierzytelnianie

Wszystkie żądania skierowane do interfejsu API REST mowy firmy Microsoft wymaga tokenu dostępu tokenu Web JSON (JWT), który można uzyskać z usługi tokenu usługi kognitywnych w `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Tokenu można uzyskać poprzez wysłanie żądania POST do usługi tokenu, określając `Ocp-Apim-Subscription-Key` nagłówek, który zawiera klucz interfejsu API, jako jego wartość.

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

Token dostępu musi być określony w każdym interfejsu API REST mowy Microsoft wywołać jako `Authorization` nagłówka poprzedzona ciągiem `Bearer`, jak pokazano w poniższym przykładzie:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Nie można przekazać tokenu prawomocny dostęp do interfejsu API REST mowy Microsoft spowoduje błąd 403 odpowiedzi.

## <a name="performing-speech-recognition"></a>Wykonywanie rozpoznawania mowy

Rozpoznawanie mowy uzyskuje się poprzez wysłanie żądania POST na `recognition` interfejsu API w `https://speech.platform.bing.com/speech/recognition/`. Pojedyncze żądanie nie może zawierać więcej niż 10 sekund dźwięku, a żądanie całkowity czas trwania nie może przekraczać 14 sekund.

Zawartość audio muszą znajdować się w treści żądania w formacie wav POST.

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
    var speechResult = JsonConvert.DeserializeObject<SpeechResult>(response);

    fileStream.Dispose();
    return speechResult;
}
```

Dźwięk jest rejestrowany w każdym projekcie specyficzne dla platformy jako dane wav PCM i `RecognizeSpeechAsync` używa metody `PCLStorage` pakietu NuGet, aby otworzyć plik dźwiękowy jako strumień. Żądanie rozpoznawania mowy wygenerowania identyfikatora URI i token dostępu są pobierane z usługi tokenu. Żądanie rozpoznawania mowy publikowanego `recognition` interfejsu API, który zwraca odpowiedź w formacie JSON zawierający wynik. Odpowiedź JSON jest rozszeregować z wynikiem zwracanych do wywoływania metody do wyświetlenia.

### <a name="configuring-speech-recognition"></a>Konfigurowanie rozpoznawania mowy

Proces rozpoznawania mowy można skonfigurować, określając parametry zapytania HTTP:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    // To build a request URL, you should follow:
    // https://docs.microsoft.com/en-us/azure/cognitive-services/speech/getstarted/getstartedrest
    string requestUri = speechEndpoint;
    requestUri += @"dictation/cognitiveservices/v1?";
    requestUri += @"language=en-us";
    requestUri += @"&format=simple";
    System.Diagnostics.Debug.WriteLine(requestUri.ToString());
    return requestUri;
}
```

Konfiguracja głównego wykonywane przez `GenerateRequestUri` metodą jest skonfigurowanie ustawień regionalnych zawartości audio. Aby uzyskać listę obsługiwanych ustawień regionalnych, zobacz [obsługiwanych języków ](/azure/cognitive-services/speech/api-reference-rest/supportedlanguages/).

### <a name="sending-the-request"></a>Wysyłanie żądania

`SendRequestAsync` Metoda wysyła żądanie POST do interfejsu API REST mowy firmy Microsoft i zwraca odpowiedź:

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);
    var response = await httpClient.PostAsync(url, content);
    return await response.Content.ReadAsStringAsync();
}
```

Ta metoda tworzy żądanie POST przez:

- Zawijanie strumieniem audio w `StreamContent` wystąpienia, która udostępnia zawartość HTTP w oparciu o strumień.
- Ustawienie `Content-Type` nagłówka żądania `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Dodawanie tokenu dostępu do `Authorization` nagłówka poprzedzona ciągiem `Bearer`.

Następnie wysłaniu żądania POST do `recognition` interfejsu API. Odpowiedź jest następnie odczytu i powrót do wywoływania metody.

`recognition` Interfejsu API będzie wysyłać kod stanu HTTP 200 (OK) w odpowiedzi, pod warunkiem, że żądanie jest prawidłowe, co oznacza, że żądanie zakończyło się pomyślnie i że żądane informacje są w odpowiedzi. Lista może zawierać błąd odpowiedzi, zobacz [Rozwiązywanie problemów](/azure/cognitive-services/speech/troubleshooting).

### <a name="processing-the-response"></a>Przetwarzanie odpowiedzi

Interfejs API zwracania odpowiedzi w formacie JSON z rozpoznany tekst jest zawarty w `name` tagu. Następujące dane JSON pokazuje komunikat typowe pomyślnej odpowiedzi:

```json
{  
   "RecognitionStatus":"Success",
   "DisplayText":"Go shopping tomorrow.",
   "Offset":16000000,
   "Duration":17100000
}
```

W przykładowej aplikacji odpowiedź w formacie JSON jest przeprowadzona deserializacja `SpeechResult` wystąpienia, w wyniku zwracanych do wywoływania metody do wyświetlania, jak pokazano na poniższych zrzutach ekranu:

![](speech-recognition-images/speech-recognition.png "Rozpoznawanie mowy")

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób użycia interfejsu API REST mowy Microsoft można przekonwertować na tekst w aplikacji platformy Xamarin.Forms audio. Oprócz przeprowadzania rozpoznawania mowy, Microsoft Speech API można przekonwertować tekst na rozmowy słowa.

## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja firmy Microsoft mowy interfejsu API](/azure/cognitive-services/speech/home/).
- [Korzystanie z usługi sieci RESTful Web](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Usługi kognitywnych ToDo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
