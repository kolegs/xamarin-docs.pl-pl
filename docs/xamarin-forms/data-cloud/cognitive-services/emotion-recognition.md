---
title: Przy użyciu powierzchni interfejsu API rozpoznawania emocji
description: Interfejs API krój przyjmuje wyrażenia twarzy na obrazie jako dane wejściowe i zwraca danych, który zawiera poziomy zaufania zestawu emocji dla każdej powierzchni w obrazie. W tym artykule opisano sposób użycia interfejsu API krój do rozpoznawania emocji, aby ocenić aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 49e53425dbaf3aadd74d02ab030929e3311c7c8c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="emotion-recognition-using-the-face-api"></a>Przy użyciu powierzchni interfejsu API rozpoznawania emocji

_Interfejs API krój przyjmuje wyrażenia twarzy na obrazie jako dane wejściowe i zwraca danych, który zawiera poziomy zaufania zestawu emocji dla każdej powierzchni w obrazie. W tym artykule opisano sposób użycia interfejsu API krój do rozpoznawania emocji, aby ocenić aplikacji platformy Xamarin.Forms._

## <a name="overview"></a>Omówienie

Powierzchni interfejsu API można wykonać wykrywania emocji wykryć gniew, contempt, wstręt, obawy, szczęście neutralne, sadness i niespodziewanego, w wyrażeniu twarzy. Te emocji są uniwersalnie i cross-culturally przekazywane za pośrednictwem tego samego podstawowego twarzy. Oraz zwraca wynik emocji wyrażenia twarzy, powierzchni interfejsu API może również zwraca obwiedni dla wykrytych powierzchni. Należy pamiętać, że klucz interfejsu API musi uzyskana za pomocą interfejsu API twarzy na obrazie. To znajduje się w [spróbuj kognitywnych usług](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Rozpoznawania emocji można wykonać za pomocą biblioteki klienta i za pośrednictwem interfejsu API REST. Ten artykuł skupia się na wykonywanie rozpoznawania emocji za pośrednictwem [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/) biblioteki klienta, który można pobrać z pakietu NuGet.

Interfejs API krój mogą służyć do rozpoznawania twarzy osób wideo i może zwrócić podsumowanie ich emocji. Aby uzyskać więcej informacji, zobacz [sposobu analizowania wideo w czasie rzeczywistym](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Aby uzyskać więcej informacji na temat powierzchni interfejsu API, zobacz [powierzchni interfejsu API](/azure/cognitive-services/face/overview/).

## <a name="performing-emotion-recognition"></a>Wykonywanie rozpoznawania emocji

Rozpoznawania emocji uzyskuje się poprzez przekazanie strumień obrazu do powierzchni interfejsu API. Rozmiar pliku obrazu nie powinien być większy niż 4MB, a są obsługiwane formaty plików, JPEG, GIF, PNG i BMP.

Poniższy przykładowy kod przedstawia proces rozpoznawania emocji:

```csharp
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;

var faceServiceClient = new FaceServiceClient(Constants.FaceApiKey, Constants.FaceEndpoint);
// e.g. var faceServiceClient = new FaceServiceClient("a3dbe2ed6a5a9231bb66f9a964d64a12", "https://westus.api.cognitive.microsoft.com/face/v1.0/detect");

var faceAttributes = new FaceAttributeType[] { FaceAttributeType.Emotion };
using (var photoStream = photo.GetStream())
{
    Face[] faces = await faceServiceClient.DetectAsync(photoStream, true, false, faceAttributes);
    if (faces.Any())
    {
        // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
        emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
    }
    // Store emotion as app rating
    ...
}
```

`FaceServiceClient` Można utworzyć wystąpienia, aby wykonać rozpoznawania emocji, za pomocą klucza powierzchni interfejsu API i punktu końcowego są przekazywane jako argumenty do `FaceServiceClient` konstruktora.

> [!NOTE]
> W Twojej powierzchni interfejsu API podczas uzyskiwania kluczy subskrypcji, należy użyć tego samego regionu. Na przykład, jeśli użytkownik uzyskał klucze subskrypcji z `westus` regionu, punkt końcowy wykrywania twarzy na obrazie będzie `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

`DetectAsync` Metodę, która jest wywoływana `FaceServiceClient` wystąpienia, przesyła obraz do powierzchni interfejsu API, jako `Stream`. Klucz interfejsu API zostanie przesłany do interfejsu API krój po wywołaniu tej operacji. Brak przesłać prawidłowego klucza interfejsu API spowoduje `Microsoft.ProjectOxford.Face.FaceAPIException` został zgłoszony z wyjątek komunikat wskazujący, że przekazano nieprawidłowy klucz interfejsu API.

`DetectAsync` Metoda zwróci `Face` tablicy, pod warunkiem, że krój został rozpoznany. Każdy zwracany krój zawiera prostokąt wskazująca lokalizacji, w połączeniu z serii krój opcjonalne atrybuty, które są określone przez `faceAttributes` argument `DetectAsync` metody. W przypadku wykrycia nie krój pustą `Face` tablicy zostaną zwrócone.

Przy interpretowaniu wyników z powierzchni interfejsu API, wykryto emocji powinny być rozumiane jako emocji z najwyższym wynik, zgodnie z znormalizowanych są wyniki do zsumowania do jednego. W związku z tym przykładowej aplikacji wyświetla rozpoznanym emocji najwyższy wynik dla największy powierzchni wykrytych w obrazie, jak pokazano na poniższych zrzutach ekranu:

![](emotion-recognition-images/emotion-recognition.png "Rozpoznawania emocji")

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób użycia interfejsu API krój do rozpoznawania emocji, aby ocenić aplikacji platformy Xamarin.Forms. Interfejs API krój przyjmuje wyrażenia twarzy na obrazie jako dane wejściowe i zwraca danych, który zawiera zaufania zestawu emocji dla każdej powierzchni w obrazie.

## <a name="related-links"></a>Linki pokrewne

- [Dostęp do interfejsu API](/azure/cognitive-services/face/overview/).
- [Usługi kognitywnych ToDo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)
