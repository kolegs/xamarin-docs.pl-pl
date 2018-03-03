---
title: "Przy użyciu emocji interfejsu API rozpoznawania emocji"
description: "Interfejs API rozpoznawania emocji — warstwa przyjmuje wyrażenia twarzy na obrazie jako dane wejściowe i zwraca poziomy zaufania zestawu emocji dla każdej powierzchni w obrazie. W tym artykule opisano sposób korzystania z interfejsu API rozpoznawania emocji — warstwa do rozpoznawania emocji, aby ocenić aplikacji platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 159bd1b23eb7505c5d5629570a34d54e0525567e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="emotion-recognition-using-the-emotion-api"></a>Przy użyciu emocji interfejsu API rozpoznawania emocji

_Interfejs API rozpoznawania emocji — warstwa przyjmuje wyrażenia twarzy na obrazie jako dane wejściowe i zwraca poziomy zaufania zestawu emocji dla każdej powierzchni w obrazie. W tym artykule opisano sposób korzystania z interfejsu API rozpoznawania emocji — warstwa do rozpoznawania emocji, aby ocenić aplikacji platformy Xamarin.Forms._

![](~/media/shared/preview.png "Ten interfejs API jest obecnie wersji wstępnej")

> [!NOTE]
> Interfejs API rozpoznawania emocji — warstwa jest wciąż w wersji zapoznawczej. Może być istotne zmiany przed ostateczną wersją interfejsu API.

## <a name="overview"></a>Omówienie

API rozpoznawania emocji — warstwa może wykryć gniew, contempt wstręt, obawy, szczęście, neutral, sadness i niespodziewanego, w wyrażeniu twarzy. Te emocji są uniwersalnie i cross-culturally przekazywane za pośrednictwem tego samego podstawowego twarzy. Oraz zwraca wynik emocji wyrażenia twarzy, interfejsu API rozpoznawania emocji — warstwa zwraca obwiedni kroje wykryte za pomocą interfejsu API twarzy na obrazie. Jeśli użytkownik została już wywołana powierzchni interfejsu API, prostokąt krój może ją następnie przesłać jako opcjonalny danych wejściowych. Należy pamiętać, że używanie interfejsu API rozpoznawania emocji — warstwa należy uzyskać klucz interfejsu API. To znajduje się w [wprowadzenie bezpłatnie](https://www.microsoft.com/cognitive-services/sign-up) w witrynie microsoft.com.

Rozpoznawania emocji można wykonać za pomocą biblioteki klienta i za pośrednictwem interfejsu API REST. Ten artykuł skupia się na wykonywanie rozpoznawania emocji za pośrednictwem [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) biblioteki klienta, który można pobrać z pakietu NuGet.

Interfejs API rozpoznawania emocji — warstwa może służyć do rozpoznawania twarzy osób wideo i zwraca podsumowanie ich emocji. Aby uzyskać więcej informacji, zobacz [emocji wideo](https://www.microsoft.com/cognitive-services/emotion-api/documentation#emotion-in-video) w witrynie microsoft.com.

Aby uzyskać więcej informacji na temat interfejsu API rozpoznawania emocji — warstwa zobacz [dokumentacji interfejsu API rozpoznawania emocji — warstwa](https://www.microsoft.com/cognitive-services/emotion-api/documentation) w witrynie microsoft.com.

## <a name="performing-emotion-recognition"></a>Wykonywanie rozpoznawania emocji

Rozpoznawania emocji uzyskuje się poprzez przekazanie strumień obrazu do interfejsu API rozpoznawania emocji — warstwa. Rozmiar pliku obrazu nie powinien być większy niż 4MB, a są obsługiwane formaty plików, JPEG, GIF, PNG i BMP. W obrazie zakres rozmiaru wykrywalny krój to 36 x 36 do 4096 x 4096 pikseli. Nie można wykryć żadnych kroje poza tym zakresem.

Poniższy przykładowy kod przedstawia proces rozpoznawania emocji:

```csharp
using Microsoft.ProjectOxford.Emotion;
using Microsoft.ProjectOxford.Emotion.Contract;

var emotionClient = new EmotionServiceClient(Constants.EmotionApiKey);

using (var photoStream = photo.GetStream())
{
  Emotion[] emotionResult = await emotionClient.RecognizeAsync(photoStream);
  if (emotionResult.Any())
  {
    // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
    emotionResultLabel.Text = emotionResult.FirstOrDefault().Scores.ToRankedList().FirstOrDefault().Key;
  }
  // Store emotion as app rating
  ...
}
```

`EmotionServiceClient` Można utworzyć wystąpienia, przeprowadzać rozpoznawania emocji przy użyciu klucza interfejsu API rozpoznawania emocji — warstwa przekazywany jako argument `EmotionServiceClient` konstruktora.

`RecognizeAsync` Metodę, która jest wywoływana `EmotionServiceClient` wystąpienia, przesyła obraz do interfejsu API rozpoznawania emocji — warstwa jako `Stream`. Klucz interfejsu API zostanie przesłany do interfejsu API rozpoznawania emocji — warstwa po wywołaniu tej operacji. Brak przesłać prawidłowego klucza interfejsu API spowoduje `Microsoft.ProjectOxford.Common.ClientException` został zgłoszony z wyjątek komunikat wskazujący, że przekazano nieprawidłowy klucz interfejsu API.

`RecognizeAsync` Metoda zwróci `Emotion` tablicy, pod warunkiem, że krój został rozpoznany. Dla każdego obrazu maksymalna liczba kroje, które mogą być wykrywane to 64, a powierzchni są uporządkowane według krój rozmiar prostokąta w kolejności malejącej. W przypadku wykrycia nie krój pustą `Emotion` tablicy zostaną zwrócone.

Przy interpretowaniu wyników z interfejsu API rozpoznawania emocji — warstwa, wykryto emocji powinny być rozumiane jako emocji z najwyższym wynik, zgodnie z znormalizowanych są wyniki do zsumowania do jednego. W związku z tym przykładowej aplikacji wyświetla rozpoznanym emocji najwyższy wynik dla największy powierzchni wykrytych w obrazie, jak pokazano na poniższych zrzutach ekranu:

![](emotion-recognition-images/emotion-recognition.png "Rozpoznawania emocji")

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono sposób użycia interfejsu API rozpoznawania emocji — warstwa do rozpoznawania emocji, aby ocenić aplikacji platformy Xamarin.Forms. Interfejs API rozpoznawania emocji — warstwa ma wyrażenia twarzy na obrazie jako dane wejściowe i zwraca zaufania zestawu emocji dla każdej powierzchni w obrazie.


## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja interfejsu API emocji](https://www.microsoft.com/cognitive-services/emotion-api/documentation)
- [Usługi kognitywnych ToDo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/)
- [Emocji interfejsu API](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)
