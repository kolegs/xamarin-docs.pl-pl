---
title: Struktury Vision w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano, jak używać systemu iOS 11 struktury Vision w rozszerzeniu Xamarin.iOS. W szczególności omówiono w nim wykrywania prostokąta oraz wykrywanie twarzy.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2017
ms.openlocfilehash: 4746de2f351e866fd72946b204f97e997c3e88c4
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350667"
---
# <a name="vision-framework-in-xamarinios"></a>Struktury Vision w rozszerzeniu Xamarin.iOS

Struktury Vision dodaje nowy obraz przetwarzania funkcji do systemu iOS 11, w tym:

- [Wykrywanie prostokąt](#rectangles)
- [Wykrywanie twarzy](#faces)
- Usługi Machine Learning, analizy obrazów (omówionych w [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Wykrywanie kodu kreskowego
- Analiza wyrównanie obrazu
- Wykrywanie tekstu
- Wykrywanie typu horizon
- Wykrywanie obiektów ś & ledzenie

![Zdjęcie z trzy prostokąty wykryto](vision-images/found-rectangles-tiny.png) ![Zdjęcie za pomocą dwóch wykryte twarze:](vision-images/xamarin-home-faces-tiny.png)

Wykrywanie prostokąta oraz wykrywanie twarzy zostały omówione bardziej szczegółowo poniżej.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Wykrywanie prostokąt

[Przykładowe VisionRects](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) pokazuje, jak przetwarzanie obrazu i rysowanie prostokątów wykryte na nim.

### <a name="1-initialize-the-vision-request"></a>1. Inicjowanie żądania przetwarzania

W `ViewDidLoad`, Utwórz `VNDetectRectanglesRequest` odwołujący się `HandleRectangles` metody, która będzie wywoływana na końcu każdego żądania:

`MaximumObservations` Właściwość powinna być ustawiona, w przeciwnym razie będą domyślnie do 1 i tylko jeden wynik zostanie zwrócony.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Rozpocznij przetwarzanie Vision

Poniższy kod uruchamia przetwarzanie żądania. W **VisionRects** próbki, ten kod jest uruchamiany po użytkownik wybrał obrazu:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Ten program obsługi wyjątków przekazuje `ciImage` do struktury Vision `VNDetectRectanglesRequest` utworzony w kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Obsługa wyników przetwarzania Vision

Po zakończeniu wykrywania prostokąt wykonuje szablon `HandleRectangles` metody, poniżej przedstawiono podsumowanie:

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

`OverlayRectangles` Method in Class metoda **VisionRectangles** przykład ma trzy funkcje:

- Renderowanie obrazu źródłowego
- Rysowanie prostokąta, aby wskazać, gdzie każdy z nich zostało wykryte, i
- Dodawanie etykietę tekstową dla każdego prostokąta przy użyciu CoreGraphics.

Widok [w przykładowym źródle](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) dokładnie metody CoreGraphics.

![Zdjęcie z trzy prostokąty wykryto](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Dalsze przetwarzanie

Wykrywanie prostokąta jest często tylko pierwszy krok w łańcuchu operacje, takie jak za pomocą [w tym przykładzie CoreMLVision](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), gdzie prostokątów są przekazywane do modelu CoreML, można przeanalizować pisma odręcznego cyfr.


<a name="faces" />

## <a name="face-detection"></a>Wykrywanie twarzy

[Przykładowe VisionFaces](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) działa w sposób podobny do **VisionRectangles** przykładowy, przy użyciu innej klasy żądania przetwarzania.

### <a name="1-initialize-the-vision-request"></a>1. Inicjowanie żądania przetwarzania

W `ViewDidLoad`, Utwórz `VNDetectFaceRectanglesRequest` odwołujący się `HandleRectangles` metody, która będzie wywoływana na końcu każdego żądania.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Rozpocznij przetwarzanie Vision

Poniższy kod uruchamia przetwarzanie żądania. W **VisionFaces** przykładowy kod jest uruchamiany po użytkownik wybrał obrazu:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Ten program obsługi wyjątków przekazuje `ciImage` do struktury Vision `VNDetectFaceRectanglesRequest` utworzony w kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Obsługa wyników przetwarzania Vision

Po zakończeniu wykrywania twarzy, program obsługi wykonuje `HandleRectangles` metodę, która obsługuje błędy i wyświetla granice wykryte twarze i wywołania `OverlayRectangles` Rysowanie prostokąty na oryginalnego obrazu:

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

`OverlayRectangles` Method in Class metoda **VisionFaces** przykład ma trzy funkcje:

- Renderowanie obrazu źródłowego
- Rysowanie prostokąta każdej twarzy wykryć, a
- Dodawanie etykietę tekstową dla każdej twarzy przy użyciu CoreGraphics.

Widok [w przykładowym źródle](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) dokładnie metody CoreGraphics.

![Zdjęcie za pomocą dwóch wykryte twarze:](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Dalsze przetwarzanie

Struktury Vision obejmuje dodatkowe możliwości wykrywania twarzy, takich jak Ty masz dostęp i ujścia. Użyj `VNDetectFaceLandmarksRequest` typ, który zwróci `VNFaceObservation` wyniki, co w kroku 3 powyżej, ale z dodatkowymi `VNFaceLandmark` danych.


## <a name="related-links"></a>Linki pokrewne

- [Wizja prostokąty (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [Wizja twarzy (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Postęp w obrazu Core - filtry, systemu operacyjnego, przetwarzania i więcej (WWDC) (wideo)](https://developer.apple.com/videos/play/wwdc2017/510/)
