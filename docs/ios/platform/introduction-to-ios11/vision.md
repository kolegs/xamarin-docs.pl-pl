---
title: Wizja Framework
ms.topic: article
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2016
ms.openlocfilehash: fb1b5b11ef0117a40267f805621797c3aee04810
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="vision-framework"></a>Wizja Framework

Framework wizji dodaje liczbę nowy obraz przetwarzania funkcji do systemu iOS 11, w tym:

- [Prostokąt wykrywania](#rectangles)
- [Wykrywanie twarzy na obrazie](#faces)
- Learning analizy obrazu komputera (omówiona w [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Wykrywanie kodu kreskowego
- Analiza wyrównanie obrazu
- Wykrywanie tekstu
- Wykrywanie typu horizon
- Wykrywanie obiektów & śledzenia

![Zdjęcie z trzech prostokąty wykryto](vision-images/found-rectangles-tiny.png) ![Zdjęcie z dwóch kroje wykryto](vision-images/xamarin-home-faces-tiny.png)

Prostokąt wykrywania i wykrywania twarzy na obrazie omówiono bardziej szczegółowo poniżej.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Prostokąt wykrywania

[Próbki VisionRects](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) przedstawia sposób przetwarzania obrazu i rysowanie prostokątów wykryte na nim.

### <a name="1-initialize-the-vision-request"></a>1. Inicjowanie żądania wizji

W `ViewDidLoad`, Utwórz `VNDetectRectanglesRequest` , która odwołuje się `HandleRectangles` metodę, która będzie wywoływana na koniec każdego żądania:

`MaximumObservations` Właściwość powinna być ustawiona, w przeciwnym razie domyślnie zostanie ustawiona na 1 i tylko jeden wynik zostanie zwrócony.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Rozpocznij przetwarzanie wizji

Poniższy kod rozpoczyna przetwarzanie żądania. W **VisionRects** przykładowa, ten kod jest uruchamiany po użytkownik wybrał obrazu:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Ten program obsługi przekazuje `ciImage` Framework wizji `VNDetectRectanglesRequest` utworzony w kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Obsługa wyników przetwarzania wizji

Po zakończeniu wykrywania prostokąt platformę wykonuje `HandleRectangles` metody, poniżej przedstawiono podsumowanie:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  bool atLeastOneValid = false;
  foreach (var o in observations){
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  if (!atLeastOneValid) return;
  // Show the pre-processed image
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Wyświetl wyniki

`OverlayRectangles` Metody w **VisionRectangles** próbki ma trzy funkcje:

- Renderowanie obrazu źródłowego
- Rysowanie prostokąta, aby wskazać, w których wykryto każdej z nich, i
- Dodawanie tekst etykiety dla każdego prostokąt przy użyciu CoreGraphics.

Widok [źródła w próbce](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) dla metody CoreGraphics.

![Zdjęcie z trzech prostokąty wykryto](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Dalsze przetwarzanie

Prostokąt wykrywania jest często tylko pierwszym krokiem w łańcuchu operacje, takie jak z [w tym przykładzie CoreMLVision](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), gdzie prostokątów są przekazywane do modelu CoreML przeanalizować odręcznie cyfr.


<a name="faces" />

## <a name="face-detection"></a>Wykrywanie twarzy na obrazie

[Próbki VisionFaces](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) działa w sposób podobny do **VisionRectangles** próbki, przy użyciu innej klasy żądania wizji.

### <a name="1-initialize-the-vision-request"></a>1. Inicjowanie żądania wizji

W `ViewDidLoad`, Utwórz `VNDetectFaceRectanglesRequest` , która odwołuje się `HandleRectangles` metodę, która będzie wywoływana na koniec każdego żądania.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Rozpocznij przetwarzanie wizji

Poniższy kod rozpoczyna przetwarzanie żądania. W **VisionFaces** przykładowy kod uruchamiany po użytkownik wybrał obrazu:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Ten program obsługi przekazuje `ciImage` Framework wizji `VNDetectFaceRectanglesRequest` utworzony w kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Obsługa wyników przetwarzania wizji

Po zakończeniu wykrywania twarzy na obrazie obsługi wykonuje `HandleRectangles` metodę, która wykonuje Obsługa błędów i wyświetla granice kroje wykryte i wywołania `OverlayRectangles` Rysowanie prostokątów ograniczający na oryginalnego obrazu:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNFaceObservation>();
  // ... omitted error handling...
  var summary = "";
  var imageSize = InputImage.Extent.Size;
  bool atLeastOneValid = false;
  Console.WriteLine("Faces:");
  summary += "Faces:";
  foreach (var o in observations) {
    // Verify detected rectangle is valid. omitted
    var boundingBox = o.BoundingBox.Scaled(imageSize);
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  // Show the pre-processed image (on UI thread)
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Wyświetl wyniki

`OverlayRectangles` Metody w **VisionFaces** próbki ma trzy funkcje:

- Renderowanie obrazu źródłowego
- Rysowanie prostokąt dla każdej powierzchni wykryte, i
- Dodawanie etykietę tekstową dla każdej powierzchni przy użyciu CoreGraphics.

Widok [źródła w próbce](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) dla metody CoreGraphics.

![Zdjęcie z dwóch kroje wykryto](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Dalsze przetwarzanie

Struktura wizji obejmuje dodatkowe możliwości wykrywania twarzy, takie jak oczu i ust. Użyj `VNDetectFaceLandmarksRequest` typu, który zwróci `VNFaceObservation` wyniki, co w kroku 3 powyżej, ale z dodatkowymi `VNFaceLandmark` danych.


## <a name="related-links"></a>Linki pokrewne

- [Wizja prostokąty (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [Wizja kroje (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Postępy w obrazie Core - filtry, systemu operacyjnego, wizja i bardziej (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/510/)
