---
title: Wprowadzenie do CoreML w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano CoreML, co umożliwia usługi machine learning w systemie iOS. W tym dokumencie omówiono, jak rozpocząć pracę z CoreML i jak z niej korzystać ze struktury Vision.
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: 13178d4530e3214c6cf31c1018b21815ccd2227f
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350694"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Wprowadzenie do CoreML w rozszerzeniu Xamarin.iOS

CoreML udostępnia usługi machine learning do systemu iOS — aplikacje mogą korzystać wytrenowane modele uczenia maszynowego wykonywać różnorodne zadania z rozwiązywania problemów do rozpoznawania obrazów.

To wprowadzenie obejmuje następujące czynności:

- [Wprowadzenie do CoreML](#coreml)
- [Przy użyciu struktury Vision CoreML](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>Wprowadzenie do CoreML

Poniższe kroki zawierają opis sposobu dodawania CoreML do projektu systemu iOS. Zapoznaj się [przykładowe Mars Habitat Pricer](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) praktyczny przykład.

![Zrzut ekranu przykładu MARS Habitat cena predykcyjne](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1. Dodaj modeli CoreML do projektu

Dodawanie modeli CoreML (plik o **.mlmodel** rozszerzenia) do **zasobów** katalogu projektu. 

W oknie dialogowym właściwości pliku modelu jego **Akcja kompilacji** ustawiono **CoreMLModel**. Oznacza to, że zostanie skompilowany w **.mlmodelc** plików podczas kompilowania aplikacji.

### <a name="2-load-the-model"></a>2. Model obciążenia

Ładowanie modelu przy użyciu `MLModel.Create` statycznej metody:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Ustaw parametry

Model parametry są przekazywane wewnątrz i na zewnątrz przy użyciu klasy kontenera, który implementuje `IMLFeatureProvider`.

Klasy dostawców funkcji zachowują się jak słownik ciągu i `MLFeatureValue`s, gdzie każda wartość funkcji może być prosty ciąg lub liczba, tablica lub danych albo bufor pikseli zawierający obraz.

Kod dla dostawcy funkcji jednowartościowych jest pokazany poniżej:

```csharp
public class MyInput : NSObject, IMLFeatureProvider
{
  public double MyParam { get; set; }
  public NSSet<NSString> FeatureNames => new NSSet<NSString>(new NSString("myParam"));
  public MLFeatureValue GetFeatureValue(string featureName)
  {
    if (featureName == "myParam")
      return MLFeatureValue.FromDouble(MyParam);
    return MLFeatureValue.FromDouble(0); // default value
  }
```

Za pomocą klasy tak, parametry wejściowe można podać w taki sposób, że zrozumiałe CoreML. Nazwy funkcji (takich jak `myParam` w przykładzie kodu) musi odpowiadać oczekiwaniom modelu.

### <a name="4-run-the-model"></a>4. Uruchom modelu

Przy użyciu modelu wymaga, aby dostawca funkcji z myślą o uruchamianiu i parametrów, następnie który `GetPrediction` można wywołać metody:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Wyodrębnione wyniki

Wyniki prognozowania `outFeatures` jest także wystąpienie `IMLFeatureProvider`; dane wyjściowe wartości można uzyskać dostęp za pomocą `GetFeatureValue` o nazwie każdego parametru wyjściowego (takie jak `theResult`), jak w tym przykładzie:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>Przy użyciu struktury Vision CoreML

CoreML mogą być również używane w połączeniu z struktury Vision do wykonywania operacji na obrazie, takich jak rozpoznawanie kształtów, identyfikator obiektu i innych zadań.

Poniższe kroki opisują, jak CoreML i obrazów są używane razem w [przykładowe CoreMLVision](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/). Przykład łączy [rozpoznawania prostokąty](~/ios/platform/introduction-to-ios11/vision.md#rectangles) ze struktury Vision z _MNINSTClassifier_ modeli CoreML do identyfikowania cyfrą pisma odręcznego na zdjęciu.

![Rozpoznawanie obrazów liczby 3](coreml-images/vision3.png) ![Rozpoznawanie obrazów liczby 5](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Tworzenie modeli Vision CoreML

Modeli CoreML _MNISTClassifier_ jest ładowany i następnie otoczona `VNCoreMLModel` co sprawia, że model dostępny dla zadań przetwarzania. Ten kod tworzy również dwa żądania przetwarzania: najpierw służące do znajdowania prostokątów w obrazie, a następnie przetwarzania prostokąt przy użyciu modeli CoreML:

```csharp
// Load the ML model
var bundle = NSBundle.MainBundle;
var assetPath = bundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
NSError mlErr, vnErr;
var mlModel = MLModel.Create(assetPath, out mlErr);
var model = VNCoreMLModel.FromMLModel(mlModel, out vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(model, HandleClassification);
```

Klasa nadal należy zaimplementować `HandleRectangles` i `HandleClassification` metody dla żądania przetwarzania, pokazane w kroku 3 i 4 opisane poniżej.

### <a name="2-start-the-vision-processing"></a>2. Rozpocznij przetwarzanie Vision

Poniższy kod uruchamia przetwarzanie żądania. W **CoreMLVision** próbki, ten kod jest uruchamiany po użytkownik wybrał obrazu:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Ten program obsługi wyjątków przekazuje `ciImage` do struktury Vision `VNDetectRectanglesRequest` utworzony w kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Obsługa wyników przetwarzania Vision

Po zakończeniu wykrywania prostokąt wykonuje `HandleRectangles` metody, która Przycina obraz, aby wyodrębnić pierwszy prostokąt, konwertuje obraz prostokąt na odcienie szarości i przekazuje je do modeli CoreML klasyfikacji.

`request` Parametr przekazany do tej metody zawiera szczegóły żądania przetwarzania i przy użyciu `GetResults<VNRectangleObservation>()` metody zwraca listę prostokątów w obrazie. Pierwszy prostokąt `observations[0]` jest wyodrębniany i przekazywane do modeli CoreML:

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] {ClassificationRequest}, out NSError err);
  });
}
```

`ClassificationRequest` Został zainicjowany w kroku 1, aby użyć `HandleClassification` metody zdefiniowanej w następnym kroku.

### <a name="4-handle-the-coreml"></a>4. Obsługa CoreML

`request` Parametr przekazany do tej metody zawiera szczegółowe informacje o żądaniu CoreML i przy użyciu `GetResults<VNClassificationObservation>()` metody zwraca listę możliwe wyniki uporządkowane według zaufania (najwyższy ufności pierwszego):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  // ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```

## <a name="samples"></a>Przykłady

Istnieją trzy próbki CoreML do wypróbowania:

* [Przykładowe Mars Habitat cena predykcyjne](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) ma proste liczbowe dane wejściowe i wyjściowe.

* [Przykład Vision i CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) akceptuje parametr obrazu i używa struktury Vision do identyfikowania kwadratowy regionów na obrazie, które są przekazywane do modelu CoreML, który rozpoznaje jednej cyfry.

* Na koniec [przykład rozpoznawanie obrazów CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) używa CoreML do identyfikowania funkcji zdjęcie. Domyślnie używa mniejszego **SqueezeNet** modelu (5MB), ale zapisane tak, aby pobrać i zastosować, równa **VGG16** modelu (553 MB). Aby uzyskać więcej informacji, zobacz [readme w próbce](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md).

## <a name="related-links"></a>Linki pokrewne

- [W usłudze Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [Przykład CoreML (Mars Habitat) (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML i obrazów (rozpoznawanie numer) (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [Rozpoznawanie obrazów CoreML (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [CoreML za pomocą usługi Azure Custom Vision (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [Wprowadzenie do CoreML (WWDC) (wideo)](https://developer.apple.com/videos/play/wwdc2017/703/)
