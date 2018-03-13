---
title: CoreImage
description: "CoreImage jest nowa struktura wprowadzone w systemie iOS 5, aby zapewnić przetwarzania obrazów i na żywo wideo zwiększające funkcjonalność. W tym artykule przedstawiono te funkcje próbki platformy Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: da9b9230a466c70cd584a00af848ffe87dacbc5b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="coreimage"></a>CoreImage

_CoreImage jest nowa struktura wprowadzone w systemie iOS 5, aby zapewnić przetwarzania obrazów i na żywo wideo zwiększające funkcjonalność. W tym artykule przedstawiono te funkcje próbki platformy Xamarin.iOS._

CoreImage jest nowa struktura wprowadzone w systemie iOS 5, która udostępnia szereg wbudowanych filtrów i efekty do zastosowania do obrazów i plików wideo, w tym wykrywania twarzy na obrazie.

Ten dokument zawiera proste przykłady:

-  Wykrywanie twarzy na obrazie.
-  Stosowanie filtrów do obrazu
-  Lista dostępnych filtrów.


Te przykłady powinna ułatwić rozpoczęcie włączenia funkcji CoreImage do aplikacji platformy Xamarin.iOS.

## <a name="requirements"></a>Wymagania

Należy używać najnowszej wersji narzędzia xcode.

## <a name="face-detection"></a>Wykrywanie twarzy na obrazie

Funkcja wykrywania twarzy na obrazie CoreImage jest po prostu widnieje — próbuje określić powierzchni fotografii i zwraca współrzędne żadnych kroje, które rozpoznaje. Te informacje może służyć do liczbę osób w obrazie, rysowania wskaźników do obrazu (np.) dla "znakowanie" osób w fotografii), lub innych elementów można traktować.

Ten kod z CoreImage\SampleCode.cs ilustruje sposób tworzenia i używania wykrywania twarzy na obrazie osadzonego obrazu:

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Tablica funkcji zostanie wypełniona `CIFaceFeature` obiektów (jeśli zostały wykryte wszystkie kroje). Brak `CIFaceFeature` dla każdej powierzchni. `CIFaceFeature` ma następujące właściwości:

-  HasMouthPosition — czy usta wykryto tej powierzchni.
-  HasLeftEyePosition — czy lewego wykryto tej powierzchni.
-  HasRightEyePosition — czy oka prawego wykryto tej powierzchni. 
-  MouthPosition — współrzędne ujścia dla tej powierzchni.
-  LeftEyePosition — współrzędne po lewej stronie oka dla tej powierzchni.
-  RightEyePosition — współrzędne oka prawego dla tej powierzchni.


Współrzędne tych właściwości rozpoczynające się w lewym dolnym rogu — w odróżnieniu od UIKit używający górnego lewego jako punkt początkowy. Za pomocą współrzędnych `CIFaceFeature` należy koniecznie "przerzucić". Ten widok bardzo podstawowe obraz niestandardowy w CoreImage\CoreImageViewController.cs pokazano, jak Rysuj trójkąty "wskaźnik krój" w obrazie (Uwaga `FlipForBottomOrigin` metody):

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

Następnie w pliku SampleCode.cs obrazu i funkcje są przypisane przed obrazu zostanie narysowany ponownie:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

Zrzut ekranu pokazuje przykładowe dane wyjściowe: lokalizacje wykryto twarzy są wyświetlane w UITextView i rysowane na przy użyciu CoreGraphics obrazu źródłowego.

Ze względu na sposób rozpoznawania twarzy, działa on czasami wykryje elementów innych niż kroje człowieka (takich jak te małpy zabawka!).

## <a name="filters"></a>Filtry

Istnieje ponad 50 różnych filtrów wbudowanych i ramach jest rozszerzalny, dzięki czemu można stosować nowe filtry.

## <a name="using-filters"></a>Za pomocą filtrów

Stosowanie filtru do obrazu obejmuje cztery kroki distinct: podczas ładowania obrazu, tworzenia filtru, filtra i zapisywanie (lub wyświetlanie) wynik.

Najpierw załadować obrazu na `CIImage` obiektu.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

Po drugie Utwórz klasę filtru i ustawienia swoich właściwości.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

Trzecie, dostęp do `OutputImage` właściwości i wywołanie `CreateCGImage` metody do renderowania końcowego wyniku.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Na koniec należy przypisać obrazu do widoku, aby zobaczyć wynik. W rzeczywistych aplikacjach obraz wynikowy mogły zostać zapisane na system plików, albumy fotografii, Tweeta lub wiadomości e-mail.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

Te zrzuty ekranu pokazać wynik `CISepia` i `CIHueAdjust` filtry, które przedstawiono w części CoreImage.zip przykładowy kod.

Zobacz [dostosować kontraktu i jasności przepisu obrazu](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) przykład `CIColorControls` filtru.

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

Ten kod z CoreImage\SampleCode.cs generuje pełną listę wbudowanych filtrów i ich parametry.

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

[Odwołania do klasy CIFilter](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) opisuje 50 wbudowanych filtrów i ich właściwości. Przy użyciu kodu powyżej, które mogą wysyłać zapytania klas filtrów, w tym wartości domyślne dla parametrów i maksymalne i minimalne dozwolone wartości (które może służyć do sprawdzania poprawności danych wejściowych przed zastosowaniem filtru).

Dane wyjściowe listy kategorii wygląda następująco w symulatorze — możesz przewijać listy, aby zobaczyć wszystkie filtry i ich parametry.

 [![](introduction-to-coreimage-images/coreimage05.png "Dane wyjściowe listy kategorii wygląda następująco w symulatorze")](introduction-to-coreimage-images/coreimage05.png#lightbox)

Każdego wymienionego filtru narażony był jako klasa w Xamarin.iOS, więc można również zapoznać się z interfejsu API Xamarin.iOS.CoreImage w przeglądarce zestawu lub przy użyciu funkcja automatycznego uzupełniania w Visual Studio for Mac lub Visual Studio. 

## <a name="summary"></a>Podsumowanie

W tym artykule pokazuje, jak korzystać z niektórych nowych funkcji framework CoreImage iOS 5, takich jak wykrywania twarzy na obrazie i stosowania filtrów do obrazu. Są dostępne w ramach można użyć wielu filtrów inny obraz.

## <a name="related-links"></a>Linki pokrewne

- [Obraz Core (przykład)](https://developer.xamarin.com/samples/CoreImage/)
- [Dostosuj kontraktu i jasności przepisu obrazu](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [Za pomocą filtrów CoreImage](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [Odwołania do klasy CIFilter](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
