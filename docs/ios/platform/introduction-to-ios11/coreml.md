---
title: Wprowadzenie do CoreML
description: Machine learning dla aplikacji mobilnych w systemie iOS 11
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 412a534829349dbbc3f3b76b166882fa6e0e1cd1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-coreml"></a>Wprowadzenie do CoreML

_Machine learning dla aplikacji mobilnych w systemie iOS 11_

CoreML przełącza uczenia maszynowego w systemach iOS — aplikacje można wykorzystać modele uczenia przeszkolone maszyny do szerokiej gamy zadania, od rozwiązywania problemów do rozpoznawania obrazu.

To wprowadzenie obejmuje następujące czynności:

- [Wprowadzenie do korzystania z CoreML](#coreml)
- [Przy użyciu CoreML Framework wizji](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>Wprowadzenie do korzystania z CoreML

Te kroki opisano, jak dodać CoreML do projektu systemu iOS. Zapoznaj się [próbki Mars siedliska Pricer](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) na przykład praktyczne.

![Zrzut ekranu przykładu predykcyjne cen siedliska MARS](coreml-images/marspricer-heading.png)

### <a name="1-add-the-model-to-the-project"></a>1. Dodaj model do projektu

Dodaj model skompilowanych (katalog o **.modelc** rozszerzenia) do **zasobów** katalogu projektu. Zawartość katalogu powinny mieć akcją kompilacji **BundleResource**:

![Folder zasobów powinien zawierać skompilowanego modelu](coreml-images/resources-modelc.png)

[Przykłady](https://developer.xamarin.com/samples/monotouch/ios11/) korzystania z modeli utworzonych w programie Xcode 9 lub ręcznie przy użyciu następującego polecenia terminala:

```csharp
xcrun coremlcompiler compile {model.mlmodel} {outputFolder}
```

> [!NOTE]
> **.model —** pliki _musi_ można skompilować do **.modelc** przed udostępnieniem przez CoreML

### <a name="2-load-the-model"></a>2. Załadować modelu

Przed rozpoczęciem korzystania z modelu, załadować go za pomocą `MLModel.FromUrl` metody statycznej:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.FromUrl(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Ustaw parametry

Parametry modelu są przekazywane i zmniejszanie przy użyciu klasy kontenera, który implementuje `IMLFeatureProvider`.

Klasy dostawców funkcji przypominają słownik ciągu i `MLFeatureValue`s, gdzie każda wartość funkcja może być prosty ciąg lub liczba, tablicy lub danych lub bufor pikseli, zawierający obraz.

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

Przy użyciu klas takich jak ta, parametry wejściowe można podać w taki sposób, który jest rozpoznawany przez CoreML. Nazwy funkcji (takich jak `myParam` w przykładzie kodu) muszą być zgodne, oczekuje modelu.

### <a name="4-run-the-model"></a>4. Uruchom modelu

Przy użyciu modelu wymaga, aby dostawca funkcji z myślą o uruchamianiu i parametrów, następnie który `GetPrediction` można wywołać metody:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Wyodrębnij wyników

Wynik prognozowania `outFeatures` jest także wystąpienie `IMLFeatureProvider`; dane wyjściowe wartości można uzyskać dostęp za pomocą `GetFeatureValue` o nazwie każdego parametru wyjściowego (takie jak `theResult`), ponieważ w tym przykładzie:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>Przy użyciu CoreML Framework wizji

CoreML można również używane w połączeniu z framework wizji do wykonywania operacji na obrazie, takie jak rozpoznawanie kształtu, identyfikator obiektu i innych zadań.

Poniżej opisano sposób CoreML i wizji są używane razem w [próbki CoreMLVision](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/). Łączy próbki [rozpoznawania prostokąty](~/ios/platform/introduction-to-ios11/vision.md#rectangles) w ramach wizji _MNINSTClassifier_ CoreML modelu w celu identyfikacji cyfrą odręcznie fotografii.

![Obraz rozpoznawania liczby 3](coreml-images/vision3.png) ![Obraz rozpoznawania liczby 5](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Tworzenie modelu CoreML wizji

CoreML model _MNISTClassifier_ jest załadowane, a następnie otoczona `VNCoreMLModel` który udostępnia model dla zadań wizji. Ten kod tworzy również dwa żądania wizji: najpierw w celu odnalezienia prostokąty obrazu, a następnie do przetwarzania prostokąt z modelem CoreML:

```csharp
// Load the ML model
var assetPath = NSBundle.MainBundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
var mlModel = MLModel.FromUrl(assetPath, out NSError mlErr);
var vModel = VNCoreMLModel.FromMLModel(mlModel, out NSError vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(vModel, HandleClassification);
```

Klasa nadal należy zaimplementować `HandleRectangles` i `HandleClassification` metod żądań wizji, przedstawiono w kroku 3 i 4 poniżej.

### <a name="2-start-the-vision-processing"></a>2. Rozpocznij przetwarzanie wizji

Poniższy kod rozpoczyna przetwarzanie żądania. W **CoreMLVision** przykładowa, ten kod jest uruchamiany po użytkownik wybrał obrazu:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Ten program obsługi przekazuje `ciImage` Framework wizji `VNDetectRectanglesRequest` utworzony w kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Obsługa wyników przetwarzania wizji

Po zakończeniu wykrywania prostokąt wykonuje `HandleRectangles` metodę, która Przycina obraz, aby wyodrębnić pierwszy prostokąta, konwertuje obraz prostokąt szarości i przekazuje je do modelu CoreML klasyfikacji.

`request` Parametr przekazany do tej metody zawiera szczegóły żądania wizja i przy użyciu `GetResults<VNRectangleObservation>()` metoda zwraca listę prostokąty znaleziono w obrazie. Pierwszy prostokąt `observations[0]` jest wyodrębniany i przekazane do modelu CoreML:

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] { ClassificationRequest }, out NSError err);
  });
}
```

`ClassificationRequest` Został zainicjowany w kroku 1, aby użyć `HandleClassification` metody zdefiniowanej w następnym kroku.

### <a name="4-handle-the-coreml"></a>4. Dojście CoreML

`request` Parametr przekazany do tej metody zawiera szczegóły żądania CoreML i przy użyciu `GetResults<VNClassificationObservation>()` metoda zwraca listę możliwych wyniki uporządkowane według zaufania (najwyższy zaufania pierwszy):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```



## <a name="samples"></a>Przykłady

Istnieją trzy próbki CoreML próby:

* [Próbki Mars siedliska cen predykcyjne](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) ma prosty liczbowych wejściami i wyjściami.

* [Przykład Vision & CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) akceptuje parametr obrazu i używa framework wizji do identyfikowania kwadratowy regionów na obrazie, które są przekazywane do modelu CoreML, który rozpoznaje pojedynczego cyfr.

* Na koniec [próbki rozpoznawania obrazu CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) używa CoreML do identyfikowania funkcje zdjęcie. Domyślnie używa mniejszej **SqueezeNet** modelu (5MB), ale jest zostały zapisane, dzięki czemu można pobrać i dołączyć do nich większy **VGG16** modelu (553 MB). Aby uzyskać więcej informacji, zobacz [readme w próbce](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md).


## <a name="related-links"></a>Linki pokrewne

- [Uczenie maszynowe (Apple)](https://developer.apple.com/machine-learning/)
- [Przykład CoreML (Mars siedliska) (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML i widzenia (rozpoznawanie numer) (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [Rozpoznawanie obrazu CoreML (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [CoreML z wizji niestandardowe Azure (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [Wprowadzenie do CoreML (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/703/)
