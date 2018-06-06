---
title: Wprowadzenie do ARKit w Xamarin.iOS
description: W tym dokumencie opisano zwiększonej rzeczywistości w systemie iOS 11 z ARKit. Zawarto informacje, jak dodać 3D modelu dla aplikacji, skonfigurować widok zaimplementować delegata sesji, umieść modelu 3D na świecie i wstrzymać sesji zwiększonej rzeczywistości.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 55ef2004f66cb808f878b2215dfdd59a45015877
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787175"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Wprowadzenie do ARKit w Xamarin.iOS

_Zwiększonej rzeczywistości dla systemu iOS 11_

ARKit umożliwia szerokiej gamy zwiększonej stanu aplikacji i gier. W tej sekcji omówiono następujące tematy:

- [Wprowadzenie do korzystania z ARKit](#gettingstarted)
- [Przy użyciu ARKit z UrhoSharp](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>Wprowadzenie do korzystania z ARKit

Aby rozpocząć pracę z rzeczywistymi zwiększonej, poniższe instrukcje przeprowadzenie prostą aplikację: pozycjonowanie modelu 3D i pozwolić ARKit zachować modelu w miejscu z jego funkcji śledzenia.

![Model 3D Jet przestawne w camera obrazu](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. Dodaj 3D model

Zasoby powinny zostać dodane do projektu z **SceneKitAsset** akcji kompilacji.

![Zasoby SceneKit w projekcie](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. Konfigurowanie widoku

W widoku kontrolera `ViewDidLoad` metody, załadować trwałego sceny i ustaw `Scene` właściwości widoku:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. Opcjonalnie zaimplementować delegata sesji

Mimo że nie jest wymagane w przypadku prostego, implementacja delegata sesji może być przydatne podczas debugowania stanu sesji ARKit (i w rzeczywistych aplikacjach przekazywanie opinii dla użytkownika). Tworzenie prostego delegata za pomocą poniższy kod:

```csharp
public class SessionDelegate : ARSessionDelegate
{
  public SessionDelegate() {}
  public override void CameraDidChangeTrackingState(ARSession session, ARCamera camera)
  {
    Console.WriteLine("{0} {1}", camera.TrackingState, camera.TrackingStateReason);
  }
}
```

Przypisz delegata w `ViewDidLoad` metody:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. Umieść na świecie modelu 3D

W `ViewWillAppear`, poniższy kod ustanawia sesję ARKit i Ustawia położenie modelu 3D przestrzeni względem aparatu fotograficznego urządzenia:

```csharp
// Create a session configuration
var configuration = new ARWorldTrackingConfiguration {
  PlaneDetection = ARPlaneDetection.Horizontal,
  LightEstimationEnabled = true
};

// Run the view's session
SceneView.Session.Run(configuration, ARSessionRunOptions.ResetTracking);

// Find the ship and position it just in front of the camera
var ship = SceneView.Scene.RootNode.FindChildNode("ship", true);

ship.Position = new SCNVector3(2f, -2f, -9f);
```

Zawsze uruchomić lub wznowić, aplikacji modelu 3D zostanie umieszczony przed aparatu. Gdy model jest ustawiony, Przenieś aparatu i oglądać zgodnie z ARKit przechowuje modelu znajduje się.

### <a name="5-pause-the-augmented-reality-session"></a>5. Wstrzymaj zwiększonej stan sesji

Dobrym rozwiązaniem, aby wstrzymać sesji ARKit, gdy kontroler widoku nie jest widoczna jest (w `ViewWillDisappear` metody:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Podsumowanie

Powyższy kod powoduje prostą aplikację ARKit. Przykłady bardziej złożonych oczekiwać kontroler widoku hosting sesji zwiększonej rzeczywistości do zaimplementowania `IARSCNViewDelegate`, oraz dodatkowe metody.

ARKit zapewnia dużą bardziej zaawansowane funkcje, takie jak Śledzenie powierzchni i interakcji z użytkownikiem. Zobacz [pokaz UrhoSharp](urhosharp.md) na przykład łączenie ARKit śledzenia z UrhoSharp.


## <a name="related-links"></a>Linki pokrewne

- [Stan zwiększonej (Apple)](https://developer.apple.com/arkit/)
- [Przy użyciu ARKit z UrhoSharp](urhosharp.md)
- [Przykładowe ARKit proste (Jet)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [Obiekty umieszczenie ARKit (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [Wprowadzenie ARKit - zwiększonej rzeczywistości dla systemu iOS (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/602/)
