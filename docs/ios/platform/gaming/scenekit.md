---
title: SceneKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 785faab84c4f3c5176750f4d86eeaeae3bb9332d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="scenekit"></a>SceneKit

Zestaw sceny jest wykres scenę 3D interfejsu API, które upraszcza pracy z grafiki 3D. Została wprowadzona w systemie OS X 10.8 i teraz nadszedł iOS 8. Z zestawem sceny tworzenie wizualizacji 3W bez ramek i zwykłych grach 3W nie wymaga doświadczenia w OpenGL. Opierając się na wspólne pojęcia wykres sceny, zestaw sceny abstracts optymalizacji złożoności OpenGL i interfejsy OpenGL ES, dzięki czemu bardzo łatwo dodać 3D zawartości do aplikacji. Jednak w przypadku eksperta OpenGL sceny zestawu ma doskonałą pomoc techniczną dla poleceń bezpośrednio z również OpenGL. Ponadto zawiera wiele funkcji, które stanowią uzupełnienie grafiki 3D, takie jak fizycznych, a bardzo dobrze integruje się z kilku innych platform firmy Apple, takich jak zestaw Sprite, obrazu Core i Core animacji.

Zestaw sceny jest bardzo proste w użyciu. Jest deklaracyjnego interfejsu API, który zapewnia obsługę renderowania. Wystarczy skonfigurować sceny, Dodaj właściwości, a także uchwytów do sceny zestawu renderowania sceny.

Aby pracować z zestawu sceny tworzenia wykresu sceny przy użyciu `SCNScene` klasy. Sceny zawiera hierarchię węzłów reprezentowany przez wystąpienie `SCNNode`, definiowanie lokalizacji w przestrzeni 3D. Każdy węzeł ma właściwości, takie jak geometria, oświetlenia i materiały, które mają wpływ na jego wyglądu, jak pokazano na poniższej ilustracji:

![](scenekit-images/image7.png "Hierarchia SceneKit") 

## <a name="create-a-scene"></a>Utwórz sceny

Sceny są wyświetlane na ekranie, aby dodać go do `SCNView` przez przypisanie go do właściwości scenę w widoku. Ponadto, jeśli zostaną wprowadzone zmiany do sceny, `SCNView` jest aktualizowane automatycznie, aby wyświetlić zmiany.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

Sceny można wypełniać z pliki wyeksportowane za pomocą narzędzia modelowania 3d lub programowo geometrycznych elementów podstawowych. Na przykład jest tworzenie kuli i dodaj go do sceny:

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>Dodawanie jasny

W tym momencie kuli nie będzie zawierał żadnych informacji ponieważ nie istnieje żadne światło sceny. Dołączanie `SCNLight` wystąpień do węzłów tworzy świateł w zestawie sceny. Istnieje kilka typów świateł od różne rodzaje oświetlenia kierunkową oświetlenia otoczenia. Na przykład poniższy kod tworzy wielokierunkowego jasny boku zakresie:

```csharp
// omnidirectional light
var light = SCNLight.Create ();
var lightNode = SCNNode.Create ();
light.LightType = SCNLightType.Omni;
light.Color = UIColor.Blue;
lightNode.Light = light;
lightNode.Position = new SCNVector3 (-40, 40, 60);
scene.RootNode.AddChildNode (lightNode);
```

Oświetlenie wielokierunkowego tworzy rozpraszania odbicia, co powoduje nawet oświetlenia sortowania jak oświetlenia z latarki. Tworzenie światła otoczenia przypomina, chociaż nie bez kierunku zgodnie z jego możliwości równomiernie we wszystkich kierunkach. Go traktować jak stylu oświetlenia :)

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

Z kontrolki w miejscu kuli jest teraz widoczne na scenie.

![](scenekit-images/image8.png "Kuli jest widoczny na scenie, gdy włączone")
 
## <a name="adding-a-camera"></a>Dodawanie aparatu

Dodawanie aparatu (SCNCamera) do sceny zmienia widok. Przypomina wzorzec, aby dodać aparatu. Utwórz aparatu, dołącz je do węzła i dodać węzeł do sceny.

```csharp
// camera
camera = new SCNCamera {
    XFov = 80,
    YFov = 80
};
cameraNode = new SCNNode {
    Camera = camera,
    Position = new SCNVector3 (0, 0, 40)
};
scene.RootNode.AddChildNode (cameraNode);
```

Jak widać z powyższego kodu obiekty mogą być tworzone za pomocą konstruktorów zestawu sceny lub metody fabryki Create. Pierwsza umożliwia przy użyciu składni inicjatora C#, ale które polega przede wszystkim na preferencji.

Aparatem w miejscu całego kuli jest widoczny dla użytkownika:

![](scenekit-images/image9.png "Cały kuli jest widoczny dla użytkownika")
 
Dodatkowe świateł można dodać do sceny również. Oto wygląda następująco z kilku więcej świateł wielokierunkowego:

![](scenekit-images/image10.png "Kuli z kilku świateł wielokierunkowego więcej")
 
Ponadto, ustawiając `sceneView.AllowsCameraControl = true`, użytkownik może zmienić widok z gestów dotykowych.

### <a name="materials"></a>Materiały

Materiały są tworzone przy użyciu klasy SCNMaterial. Na przykład dodać obraz na powierzchni sfery Ustawianie obrazu do materiałów *rozproszone* zawartość.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

Warstwy obrazu na węźle, jak pokazano poniżej:

![](scenekit-images/image11.png "Układanie warstwowo obrazu na zakresie")
 
Można ustawić materiału odpowiedzieć na inne typy zbyt oświetlenia. Na przykład obiekt może być utworzona obiektowi błyszczący i ustawiono zawartością odblasków do wyświetlenia odblasków odbicia, co powoduje jasny miejscu na powierzchni, jak pokazano poniżej:

![](scenekit-images/image12.png "Wprowadzone obiektowi błyszczący za pomocą odbicia odblasków, co w jasny miejscu na powierzchni obiektu")
 
Materiały są bardzo elastyczne, umożliwiając osiągnięcia partii z kodem bardzo mało. Na przykład zamiast ustawień obrazu rozpraszania zawartość, ustaw jej odbicia zawartości zamiast tego.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

Małp pojawi się wizualnie znajdują się w zakresie, niezależnie od punktu widzenia.

### <a name="animation"></a>Animacja

Zestaw sceny jest przeznaczona do pracy z animacji. Można utworzyć jawnych ani niejawnych animacji, a nawet można renderować sceny z drzewa warstwy animacji Core. Podczas tworzenia niejawne animacji, zestaw sceny zapewnia własnej klasy przejścia `SCNTransaction`.

Oto przykład, w którym obraca się w zakresie:

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

Można animować znacznie więcej niż obrotu jednak. Wiele właściwości zestawu sceny są można animować. Na przykład następujący kod animuje materiałów `Shininess` zwiększające odblasków odbicia.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

Zestaw sceny jest bardzo prosta do użycia. Oferuje wiele dodatkowych funkcji, takich jak ograniczenia, fizycznych, deklaratywne akcje, Tekst 3W, głębokość pola pomocy technicznej, integration Sprite Kit i integracji obrazu Core nazw kilku.