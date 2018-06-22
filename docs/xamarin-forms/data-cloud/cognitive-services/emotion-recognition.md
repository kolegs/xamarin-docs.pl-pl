---
title: Przy użyciu powierzchni interfejsu API rozpoznawania emocji
description: Interfejs API krój przyjmuje wyrażenia twarzy na obrazie jako dane wejściowe i zwraca danych, który zawiera poziomy zaufania zestawu emocji dla każdej powierzchni w obrazie. W tym artykule opisano sposób użycia interfejsu API krój do rozpoznawania emocji, aby ocenić aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 4dc04cb077b894b255eb496b2cb2983626573897
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
ms.locfileid: "34049769"
---
# <a name="emotion-recognition-using-the-face-api"></a>Przy użyciu powierzchni interfejsu API rozpoznawania emocji

_Interfejs API krój przyjmuje wyrażenia twarzy na obrazie jako dane wejściowe i zwraca danych, który zawiera poziomy zaufania zestawu emocji dla każdej powierzchni w obrazie. W tym artykule opisano sposób użycia interfejsu API krój do rozpoznawania emocji, aby ocenić aplikacji platformy Xamarin.Forms._

## <a name="overview"></a>Omówienie

Powierzchni interfejsu API można wykonać wykrywania emocji wykryć gniew, contempt, wstręt, obawy, szczęście neutralne, sadness i niespodziewanego, w wyrażeniu twarzy. Te emocji są uniwersalnie i cross-culturally przekazywane za pośrednictwem tego samego podstawowego twarzy. Oraz zwraca wynik emocji wyrażenia twarzy, powierzchni interfejsu API może również zwraca obwiedni dla wykrytych powierzchni. Należy pamiętać, że klucz interfejsu API musi uzyskana za pomocą interfejsu API twarzy na obrazie. To znajduje się w [spróbuj kognitywnych usług](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Rozpoznawania emocji można wykonać za pomocą biblioteki klienta i za pośrednictwem interfejsu API REST. Ten artykuł skupia się na wykonywanie rozpoznawania emocji za pośrednictwem interfejsu API REST. Aby uzyskać więcej informacji na temat interfejsu API REST, zobacz [powierzchni interfejsu API REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Interfejs API krój mogą służyć do rozpoznawania twarzy osób wideo i może zwrócić podsumowanie ich emocji. Aby uzyskać więcej informacji, zobacz [sposobu analizowania wideo w czasie rzeczywistym](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Aby uzyskać więcej informacji na temat powierzchni interfejsu API, zobacz [powierzchni interfejsu API](/azure/cognitive-services/face/overview/).

## <a name="authentication"></a>Uwierzytelnianie

Wszystkie żądania skierowane do powierzchni interfejsu API wymaga klucz interfejsu API, które powinny być określone jako wartość `Ocp-Apim-Subscription-Key` nagłówka. W poniższym przykładzie przedstawiono sposób dodawania klucz interfejsu API `Ocp-Apim-Subscription-Key` nagłówka żądania:

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Nie można przekazać prawidłowy klucz interfejsu API do powierzchni interfejsu API spowoduje błąd odpowiedzi 401.

## <a name="performing-emotion-recognition"></a>Wykonywanie rozpoznawania emocji

Rozpoznawania emocji jest wykonywane czyniąc żądanie POST, zawierający obraz `detect` interfejsu API w `https://[location].api.cognitive.microsoft.com/face/v1.0`, gdzie `[location]]` to region używany do uzyskania klucza interfejsu API. Dostępne są następujące parametry opcjonalne żądania:

- `returnFaceId` — czy mają być zwracane faceIds wykryto powierzchni. Wartość domyślna to `true`.
- `returnFaceLandmarks` — czy mają być zwracane punkty orientacyjne krojów z wykrytym powierzchni. Wartość domyślna to `false`.
- `returnFaceAttributes` — czy do analizowania i zwraca co najmniej jeden określony stają przed atrybutów. Atrybuty obsługiwanych powierzchni obejmują `age`, `gender`, `headPose`, `smile`, `facialHair`, `glasses`, `emotion`, `hair`, `makeup`, `occlusion`, `accessories`, `blur`, `exposure`, i `noise`. Pamiętaj, że analizy atrybutu krój ma dodatkowych kosztów obliczeniową i czasu.

Zawartości obrazu muszą znajdować się w treści żądania POST jako adres URL lub dane binarne.

> [!NOTE]
> Obsługiwane formaty plików są JPEG, GIF, PNG i BMP, a dozwolony rozmiar pliku to od 1KB do 4MB.

W przykładowej aplikacji, proces rozpoznawania emocji jest wywoływany przez wywołanie metody `DetectAsync` metody:

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

Wywołanie tej metody określa zawierające dane obrazu, który ma zostać zwrócony faceIds, punkty orientacyjne krój nie powinny zostać zwrócone i emocji obrazu należy przeprowadzić analizę strumienia. Określa on również, że wyniki zostaną zwrócone jako tablica `Face` obiektów. Z kolei `DetectAsync` wywołuje metodę `detect` interfejsu API REST, który wykonuje rozpoznawania emocji:

```csharp
public async Task<Face[]> DetectAsync(Stream imageStream, bool returnFaceId, bool returnFaceLandmarks, IEnumerable<FaceAttributeType> returnFaceAttributes)
{
  var requestUrl =
    $"{Constants.FaceEndpoint}/detect?returnFaceId={returnFaceId}" +
    "&returnFaceLandmarks={returnFaceLandmarks}" +
    "&returnFaceAttributes={GetAttributeString(returnFaceAttributes)}";
  return await SendRequestAsync<Stream, Face[]>(HttpMethod.Post, requestUrl, imageStream);
}
```

Ta metoda generuje identyfikator URI żądania, a następnie wysyła żądanie do `detect` interfejsu API za pomocą `SendRequestAsync` metody.

> [!NOTE]
> W Twojej powierzchni interfejsu API podczas uzyskiwania kluczy subskrypcji, należy użyć tego samego regionu. Na przykład, jeśli użytkownik uzyskał klucze subskrypcji z `westus` regionu, punkt końcowy wykrywania twarzy na obrazie będzie `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

### <a name="sending-the-request"></a>Wysyłanie żądania

`SendRequestAsync` Metoda wysyła żądanie POST do powierzchni interfejsu API i zwraca wynik w postaci `Face` tablicy:

```csharp
async Task<TResponse> SendRequestAsync<TRequest, TResponse>(HttpMethod httpMethod, string requestUrl, TRequest requestBody)
{
  var request = new HttpRequestMessage(httpMethod, Constants.FaceEndpoint);
  request.RequestUri = new Uri(requestUrl);
  if (requestBody != null)
  {
    if (requestBody is Stream)
    {
      request.Content = new StreamContent(requestBody as Stream);
      request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
    }
    else
    {
      // If the image is supplied via a URL
      request.Content = new StringContent(JsonConvert.SerializeObject(requestBody, s_settings), Encoding.UTF8, "application/json");
    }
  }

  HttpResponseMessage responseMessage = await _client.SendAsync(request);
  if (responseMessage.IsSuccessStatusCode)
  {
    string responseContent = null;
    if (responseMessage.Content != null)
    {
      responseContent = await responseMessage.Content.ReadAsStringAsync();
    }
    if (!string.IsNullOrWhiteSpace(responseContent))
    {
      return JsonConvert.DeserializeObject<TResponse>(responseContent, s_settings);
    }
    return default(TResponse);
  }
  else
  {
    ...
  }
  return default(TResponse);
}
```

Jeśli obraz jest dostarczany za pomocą strumienia, metoda tworzy żądanie POST zawijania strumień obrazu w `StreamContent` wystąpienia, która udostępnia zawartość HTTP w oparciu o strumień. Alternatywnie, jeśli obraz jest dostarczany za pomocą adresu URL, metoda kompilacje żądania POST zawijania adres URL w `StringContent` wystąpienia, która udostępnia zawartość HTTP w oparciu o ciąg.

Następnie wysłaniu żądania POST do `detect` interfejsu API. Odpowiedź jest do odczytu, deserializacji i powrót do wywoływania metody.

`detect` Interfejsu API będzie wysyłać kod stanu HTTP 200 (OK) w odpowiedzi, pod warunkiem, że żądanie jest prawidłowe, co oznacza, że żądanie zakończyło się pomyślnie i że żądane informacje są w odpowiedzi. Lista może zawierać błąd odpowiedzi, zobacz [powierzchni interfejsu API REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

### <a name="processing-the-response"></a>Przetwarzanie odpowiedzi

Interfejs API odpowiedzi jest zwracany w formacie JSON. Następujące dane JSON pokazuje komunikat typowe odpowiedź oznaczająca Powodzenie dostarczająca dane żądane przez przykładową aplikację:

```json
[  
   {  
      "faceId":"8a1a80fe-1027-48cf-a7f0-e61c0f005051",
      "faceRectangle":{  
         "top":192,
         "left":164,
         "width":339,
         "height":339
      },
      "faceAttributes":{  
         "emotion":{  
            "anger":0.0,
            "contempt":0.0,
            "disgust":0.0,
            "fear":0.0,
            "happiness":1.0,
            "neutral":0.0,
            "sadness":0.0,
            "surprise":0.0
         }
      }
   }
]
```

Komunikat odpowiedzi pomyślne składa się z tablicę wpisów krój uszeregowane według rozmiaru prostokąt krój w kolejności malejącej, gdy pustą odpowiedź wskazuje, nie kroje wykryte. Każdy rozpoznany krój zawiera szereg krój opcjonalne atrybuty, które są określone przez `returnFaceAttributes` argument `DetectAsync` metody.

W przykładowej aplikacji odpowiedź w formacie JSON jest rozszeregować na tablicę `Face` obiektów. Przy interpretowaniu wyników z powierzchni interfejsu API, wykryto emocji powinny być rozumiane jako emocji z najwyższym wynik, zgodnie z znormalizowanych są wyniki do zsumowania do jednego. W związku z tym aplikacji przykładowej zostaną wyświetlone rozpoznanym emocji najwyższy wynik dla największy powierzchni wykrytych w obrazie. Jest to osiągane następującym kodem:

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

Poniższy zrzut ekranu przedstawia wynik procesu rozpoznawania emocji w przykładowej aplikacji:

![](emotion-recognition-images/emotion-recognition.png "Rozpoznawania emocji")

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób użycia interfejsu API krój do rozpoznawania emocji, aby ocenić aplikacji platformy Xamarin.Forms. Interfejs API krój przyjmuje wyrażenia twarzy na obrazie jako dane wejściowe i zwraca danych, który zawiera zaufania zestawu emocji dla każdej powierzchni w obrazie.

## <a name="related-links"></a>Linki pokrewne

- [Dostęp do interfejsu API](/azure/cognitive-services/face/overview/).
- [Usługi kognitywnych ToDo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Powierzchni interfejsu API REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
