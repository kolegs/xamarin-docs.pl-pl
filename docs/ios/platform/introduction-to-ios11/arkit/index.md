---
title: Wprowadzenie do ARKit w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano rzeczywistość rozszerzona w systemie iOS 11 z ARKit. Omówiono w nim sposób dodawania modelu 3D do aplikacji, skonfigurować widok, zaimplementować delegata sesji, umieść modelu 3D na całym świecie i wstrzymania sesji rzeczywistości rozszerzonej.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: 14bbb35477c098738e9cd7e2cb92154422d394ee
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350618"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Wprowadzenie do ARKit w rozszerzeniu Xamarin.iOS

_Rzeczywistość rozszerzona dla systemu iOS 11_

ARKit umożliwia szerokiej gamy rzeczywistość rozszerzona aplikacji i gier. W tej sekcji omówiono następujące tematy:

- [Wprowadzenie do ARKit](#gettingstarted)
- [ARKit przy użyciu platformy UrhoSharp](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>Wprowadzenie do ARKit

Wprowadzenie do rzeczywistości rozszerzonej, zgodnie z poniższymi instrukcjami przeprowadzenie prostą aplikację: pozycjonowanie modelu 3D i umożliwienie ARKit zachować modelu w miejscu z jej funkcjami śledzenia.

![Model 3D Jet liczb zmiennoprzecinkowych w camera obrazu](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. Dodawanie modelu 3D

Zasoby powinny zostać dodane do projektu z **SceneKitAsset** Akcja kompilacji.

![Zasoby struktury SceneKit w projekcie](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. Konfigurowanie widoku

W kontrolerze widoku `ViewDidLoad` metody ładowania zasobów sceny i ustaw `Scene` właściwości w widoku:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. Opcjonalnie zaimplementować delegata sesji

Chociaż nie jest to wymagane w przypadku prostych, implementowanie delegata sesji mogą być przydatne podczas debugowania stanu sesji ARKit (i w rzeczywistych aplikacjach opinii do użytkownika). Utwórz proste delegata za pomocą poniższego kodu:

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

### <a name="4-position-the-3d-model-in-the-world"></a>4. Umieść modelu 3D na całym świecie

W `ViewWillAppear`, poniższy kod ustanawia sesję ARKit i Ustawia położenie modelu 3D w przestrzeni względem aparatu urządzenia:

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

Każdorazowo, uruchamiania lub wznowione, aplikacji modelu 3D zostanie umieszczony przed aparatu. Gdy model jest umieszczony, przesunięcie kamery i obserwuj pojawianie się ARKit przechowuje model umieszczony.

### <a name="5-pause-the-augmented-reality-session"></a>5. Wstrzymaj sesję rzeczywistość rozszerzona

Jest dobrą praktyką, aby wstrzymać sesji ARKit, gdy kontroler widoku nie jest widoczna (w `ViewWillDisappear` metody:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Podsumowanie

Powyższy kod powoduje prostą aplikację ARKit. Przykłady bardziej złożonych oczekiwać kontrolera widoku hostingu sesji rzeczywistość rozszerzona, aby zaimplementować `IARSCNViewDelegate`, i dodatkowe metody można zaimplementować.

ARKit oferuje wiele bardziej zaawansowane funkcje, takie jak Śledzenie powierzchni i interakcji z użytkownikiem. Zobacz [pokaz platformy UrhoSharp](urhosharp.md) na przykład łączenie ARKit śledzenia przy użyciu platformy UrhoSharp.


## <a name="related-links"></a>Linki pokrewne

- [Rzeczywistość rozszerzona (Apple)](https://developer.apple.com/arkit/)
- [ARKit przy użyciu platformy UrhoSharp](urhosharp.md)
- [Przykładowe ARKit proste (Jet)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [Obiekty umieszczenie ARKit (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [Wprowadzenie do ARKit — w rzeczywistości rozszerzonej dla systemu iOS (WWDC) (wideo)](https://developer.apple.com/videos/play/wwdc2017/602/)
