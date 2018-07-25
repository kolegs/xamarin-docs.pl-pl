---
title: Obraz podstawowe w rozszerzeniu Xamarin.iOS
description: Obraz Core jest nowej struktury, wprowadzona w systemie iOS 5, aby zapewnić przetwarzanie obrazu i na żywo wideo ulepszenie funkcji. W tym artykule przedstawiono te funkcje przy użyciu przykłady rozszerzenia Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 7af57856079813e8cb1831a7f22a0a098a6be771
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242169"
---
# <a name="core-image-in-xamarinios"></a>Obraz podstawowe w rozszerzeniu Xamarin.iOS

_Obraz Core jest nowej struktury, wprowadzona w systemie iOS 5, aby zapewnić przetwarzanie obrazu i na żywo wideo ulepszenie funkcji. W tym artykule przedstawiono te funkcje przy użyciu przykłady rozszerzenia Xamarin.iOS._

Obraz Core jest nowej struktury wprowadzony w systemie iOS 5, która udostępnia szereg wbudowanych filtrów i efekty do zastosowania do obrazów i klipów wideo, w tym wykrywanie twarzy.

Ten dokument zawiera proste przykłady:

-  Wykrywanie twarzy.
-  Stosowanie filtrów do obrazu
-  Listę dostępnych filtrów.


Przykłady te powinny pomóc w ułatwiające rozpoczęcie pracy, opakowując obraz podstawowe funkcje aplikacji platformy Xamarin.iOS.

## <a name="requirements"></a>Wymagania

Należy użyć najnowszej wersji środowiska Xcode.

## <a name="face-detection"></a>Wykrywanie twarzy

Funkcja wykrywania twarzy obrazu Core jest po prostu widnieje — próbuje identyfikowanie twarzy na fotografii i zwraca współrzędne wszelkie twarzy, które rozpoznaje. Informacja ta może służyć do liczby osób w obrazie, narysuj wskaźników w obrazie (np.) "tagowania" osób na zdjęciu), aby uzyskać lub innych elementów można traktować.

Ten kod z CoreImage\SampleCode.cs przedstawia sposób tworzenia i używania wykrywanie twarzy na obrazie embedded:

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Zostanie wypełniony tablicę funkcji `CIFaceFeature` obiektów (jeśli zostały wykryte wszystkie twarzy). Brak `CIFaceFeature` każdej twarzy. `CIFaceFeature` ma następujące właściwości:

-  HasMouthPosition — czy usta zostało wykryte w tym twarzy.
-  HasLeftEyePosition — czy lewego zostało wykryte w tym twarzy.
-  HasRightEyePosition — czy oka prawego zostało wykryte w tym twarzy. 
-  MouthPosition — współrzędne ujścia to twarzy.
-  LeftEyePosition — współrzędne oka po lewej stronie, to twarzy.
-  RightEyePosition — współrzędne oka prawego to twarzy.


Współrzędne te właściwości mają ich pochodzenia w dolnym lewym rogu — w przeciwieństwie do UIKit, który używa lewego górnego jako punkt początkowy. W przypadku korzystania z współrzędne na `CIFaceFeature` koniecznie "przerzucić". Ten widok wykraczającego poza podstawowe obrazu niestandardowego w CoreImage\CoreImageViewController.cs pokazuje, jak narysować trójkąty "wskaźnik twarzy" w obrazie (Uwaga `FlipForBottomOrigin` metoda):

```csharp
public class FaceDetectImageView : UIView
{
    public Xamarin.iOS.CoreImage.CIFeature[] Features;
    public UIImage Image;
    public FaceDetectImageView (RectangleF rect) : base(rect) {}
    CGPath path;
    public override void Draw (RectangleF rect) {
        base.Draw (rect);
        if (Image != null)
            Image.Draw(rect);

        using (var context = UIGraphics.GetCurrentContext()) {
            context.SetLineWidth(4);
            UIColor.Red.SetStroke ();
            UIColor.Clear.SetFill ();
            if (Features != null) {
                foreach (var feature in Features) { // for each face
                    var facefeature = (CIFaceFeature)feature;
                    path = new CGPath ();
                    path.AddLines(new PointF[]{ // assumes all 3 features found
                        FlipForBottomOrigin(facefeature.LeftEyePosition, 200),
                        FlipForBottomOrigin(facefeature.RightEyePosition, 200),
                        FlipForBottomOrigin(facefeature.MouthPosition, 200)
                    });
                    path.CloseSubpath();
                    context.AddPath(path);
                    context.DrawPath(CGPathDrawingMode.FillStroke);
                }
            }
        }
    }
    /// <summary>
    /// Face recognition coordinates have their origin in the bottom-left
    /// but we are drawing with the origin in the top-left, so "flip" the point
    /// </summary>
    PointF FlipForBottomOrigin (PointF point, int height)
    {
        return new PointF(point.X, height - point.Y);
    }
}
```

Następnie w pliku SampleCode.cs obrazu i funkcje są przypisane przed odświeżeniu obrazu:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

Na zrzucie ekranu przedstawiono przykładowe dane wyjściowe: lokalizacje wykryte twarzy są wyświetlane w UITextView i rysowane na przy użyciu CoreGraphics obrazu źródłowego.

Ze względu na sposób rozpoznawania twarzy działa ona od czasu do czasu wykryje elementów innych niż twarze (np. te małpy zabawki!).

## <a name="filters"></a>Filtry

Istnieje ponad 50 różnymi filtrami wbudowane, a struktura jest rozszerzalny, dzięki czemu można zaimplementować nowych filtrów.

## <a name="using-filters"></a>Przy użyciu filtrów

Zastosowanie filtru do obrazu składa się z czterech różnych kroków: podczas ładowania obrazu, tworzenia filtru, zastosowanie filtru i zapisywanie (lub wyświetlanie) wynik.

Najpierw Załaduj obraz w `CIImage` obiektu.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

Po drugie Utwórz klasę filtru i ustaw jego właściwości.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

Po trzecie, dostęp do `OutputImage` właściwości i wywołania `CreateCGImage` metody do renderowania na wynik końcowy.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Na koniec należy przypisać obrazu do widoku, aby wyświetlić wynik. W rzeczywistych aplikacjach obraz wynikowy może być zapisany do systemu plików, albumu Tweetu lub wiadomości e-mail.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

Te zrzuty ekranu pokazują wynik `CISepia` i `CIHueAdjust` filtry, które zostały przedstawione w CoreImage.zip przykładowego kodu.

Zobacz [dostosować kontraktu i jasności przepisu obraz](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) przykład `CIColorControls` filtru.

```csharp
var uiimage = UIImage.FromFile("photo.JPG");
var ciimage = new CIImage(uiimage);
var hueAdjust = new CIHueAdjust();   // first filter
hueAdjust.Image = ciimage;
hueAdjust.Angle = 2.094f;
var sepia = new CISepiaTone();       // second filter
sepia.Image = hueAdjust.OutputImage; // output from last filter, input to this one
sepia.Intensity = 0.3f;
CIFilter color = new CIColorControls() { // third filter
    Saturation = 2,
    Brightness = 1,
    Contrast = 3,
    Image = sepia.OutputImage    // output from last filter, input to this one
};
var output = color.OutputImage;
var context = CIContext.FromOptions(null);
// ONLY when CreateCGImage is called do all the effects get rendered
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

```csharp
var context = CIContext.FromOptions (null);
```

```csharp
var context = CIContext.FromOptions(new CIContextOptions() {
    UseSoftwareRenderer = true  // CPU
});
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

### <a name="listing-filters-and-their-properties"></a>Lista filtrów i ich właściwości

Ten kod z CoreImage\SampleCode.cs generuje pełną listę wbudowanych filtrów i ich parametrów.

```csharp
var filters = CIFilter.FilterNamesInCategories(new string[0]);
foreach (var filter in filters){
   display.Text += filter +"\n";
   var f = CIFilter.FromName (filter);
   foreach (var key in f.InputKeys){
     var attributes = (NSDictionary)f.Attributes[new NSString(key)];
     var attributeClass = attributes[new NSString("CIAttributeClass")];
     display.Text += "   " + key;
     display.Text += "   " + attributeClass + "\n";
   }
}
```

[Odwołań do klas CIFilter](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) opisuje 50 wbudowanych filtrów i ich właściwości. Przy użyciu kodu powyżej można tworzyć zapytania filtru klas, w tym wartości domyślne parametrów i wartości dozwoloną maksymalną i minimalną, (które może służyć do sprawdzania poprawności danych wejściowych przed zastosowaniem filtru).

Kategorie List dane wyjściowe wyglądają następująco w symulatorze — możesz przewijać listę, aby zobaczyć wszystkie filtry i ich parametrów.

 [![](introduction-to-coreimage-images/coreimage05.png "Kategorie List dane wyjściowe wyglądają następująco w symulatorze")](introduction-to-coreimage-images/coreimage05.png#lightbox)

Każdy filtr wymienione została udostępniona jako klasę w rozszerzeniu Xamarin.iOS, dzięki czemu możesz też zapoznać się z interfejsem API Xamarin.iOS.CoreImage w przeglądarce zestawu lub za pomocą automatycznego uzupełniania w Visual Studio for Mac lub Visual Studio. 

## <a name="summary"></a>Podsumowanie

Ten artykuł pokazuje, jak korzystać z niektórych nowych funkcji framework obrazu podstawowego z systemem iOS 5 takich jak wykrywanie twarzy oraz zastosowanie filtrów do obrazu. Istnieją dziesiątek, jak inny obraz filtrów dostępnych w ramach do użycia.

## <a name="related-links"></a>Linki pokrewne

- [Obraz Core (przykład)](https://developer.xamarin.com/samples/CoreImage/)
- [Dostosuj kontraktu i jasności przepisu obrazu](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [Przy użyciu filtrów obrazów podstawowych](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [Informacje o klasach CIFilter](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
