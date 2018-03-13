---
title: SpriteKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: c533e38fcd8775fd6b9a49c2e14de76673641553
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="spritekit"></a>SpriteKit

Zestaw Sprite, gier 2W platformę od firmy Apple, posiada niektóre ciekawe nowe funkcje w systemie iOS 8 i OS X Yosemite. Obejmuje to integrację z zestawu sceny, obsługuje programów do cieniowania oświetlenia, cieni, ograniczenia, generowania mapy normalnej i ulepszenia fizycznych. W szczególności nowe funkcje fizycznych ułatwiają bardzo realistyczne efektów gier.

## <a name="physics-bodies"></a>Jednostek fizycznych

Zestaw Sprite zawiera 2W fizycznych sztywne treści interfejsu API. Każdy sprite ma treść skojarzone fizycznych (`SKPhysicsBody`) Definiuje właściwości fizycznych, takie jak masowej i tarcia, a także geometrii treści na świecie fizycznych.

## <a name="creating-a-physics-body-from-a-texture"></a>Tworzenie treści fizycznych z tekstury
Zestaw Sprite obsługuje teraz pochodny treści fizycznych sprite jego tekstury. Ułatwia to zaimplementować konflikty, które wyglądają bardziej naturalny.

Przykładem w następujących kolizji jak bananów i małp dojść do kolizji prawie na powierzchni każdego obrazu:
 
![](spritekit-images/image13.png "Bananów i małp dojść do kolizji prawie na powierzchni każdego obrazu")

Zestaw Sprite umożliwia tworzenie treści fizycznych z jednego wiersza kodu. Po prostu Wywołaj `SKPhysicsBody.Create` tekstury i rozmiar: sprite. PhysicsBody = SKPhysicsBody.Create (sprite. Tekstury, sprite. Rozmiar);

## <a name="alpha-threshold"></a>Próg alfa

Oprócz wystarczy wybrać ustawienie `PhysicsBody` można ustawić właściwości bezpośrednio do geometrii pochodną tekstury, aplikacji i próg alfa, aby kontrolować sposób pochodzi geometrii. 

Próg alfa określa minimalną wartość alfa piksel muszą mieć do uwzględnienia w wynikowej treści fizycznych. Na przykład następujący kod wyniki w treści fizycznych nieco inne:

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

Efekt Dostosowywanie próg alfa takie fine-tunes poprzedniego kolizji tak, aby małp znajduje się przypadku kolizji z bananów:

![](spritekit-images/image14.png "Małp znajduje się przypadku kolizji z bananów")
 
## <a name="physics-fields"></a>Pola fizycznych

Inny oprócz dużą Sprite zestawu jest obsługuje nowe pole fizycznych. Można dodawać elementy, np. pola vortex grawitacji promieniowych i spring polach nazwę kilku.

Pola fizycznych są tworzone za pomocą klasy SKFieldNode, która jest dodawana do sceny podobnie jak każdy inny `SKNode`. Istnieje wiele metod fabryki na `SKFieldNode` można utworzyć pola w różnych fizycznych. Pole źródła można utworzyć przez wywołanie metody `SKFieldNode.CreateSpringField()`, pola grawitacji promieniowego, wywołując `SKFieldNode.CreateRadialGravityField()`i tak dalej.

`SKFieldNode` zawiera także właściwości, aby kontrolować pola atrybutów, takich jak siły pola, pole region i tłumienie wymusza pola.

## <a name="spring-field"></a>Pole źródła

Na przykład poniższy kod tworzy pole spring i dodaje go do sceny:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

Następnie można dodać ikony i ustawić ich `PhysicsBody` właściwości, aby pole fizycznych będzie miało wpływ na ikony, jak następujący kod, gdy użytkownik dotyka ekranu:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    var touch = touches.AnyObject as UITouch;
    var pt = touch.LocationInNode (this);
    var node = SKSpriteNode.FromImageNamed ("TinyBanana");
    node.PhysicsBody = SKPhysicsBody.Create (node.Texture, node.Size);
    node.PhysicsBody.AffectedByGravity = false;
    node.PhysicsBody.AllowsRotation = true;
    node.PhysicsBody.Mass = 0.03f;
    node.Position = pt;
    AddChild (node);
}
```

Powoduje to bananów do oscillate jak spring wokół węzeł pola:

![](spritekit-images/image15.png "Banany oscillate jak spring wokół węzeł pola")
 
## <a name="radial-gravity-field"></a>Pole grawitacji promieniowego

Dodawanie innego pola jest podobne. Na przykład poniższy kod tworzy pole grawitacji promieniowego.

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

Powoduje to innego pola force, gdzie banany są pobierane poprzecznie o polu:

![](spritekit-images/image16.png "Banany są pobierane poprzecznie wokół pola")