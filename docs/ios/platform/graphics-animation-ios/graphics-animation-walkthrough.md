---
title: Wskazówki — przy użyciu CoreGraphics i CoreAnimation
description: W tym artykule krok po kroku przedstawiono sposób tworzenia aplikacji, która używa grafiki Core i Core animacji. Pokazuje sposób rysowania na ekranie w odpowiedzi na touch użytkownika, a także sposobu animowany obraz przesyłanie wzdłuż ścieżki.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f857accfcdec4cb60e781936d1d0836dbf8d6ffb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="drawing-and-animating-along-a-path"></a>Rysowanie i animowanie wzdłuż ścieżki

W ramach tego przewodnika zamierzamy narysować ścieżkę przy użyciu grafiki podstawowej w odpowiedzi do wprowadzania dotykowego. Następnie dodamy `CALayer` zawierającego obraz, który firma Microsoft będzie animować wzdłuż ścieżki.

Poniższy zrzut ekranu przedstawia gotową aplikację:

![](graphics-animation-walkthrough-images/00-final-app.png "Ukończona aplikacja")

Zanim zaczniemy pobierania *GraphicsDemo* próbki, dołączony w tym przewodniku. Można go pobrać [tutaj](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) i znajduje się wewnątrz **GraphicsWalkthrough** katalog uruchomienia projektu o nazwie **GraphicsDemo_starter** przez dwukrotne kliknięcie, i Otwórz `DemoView` klasy.

## <a name="drawing-a-path"></a>Rysowanie ścieżki


1. W `DemoView` dodać `CGPath` zmiennej do klasy i wystąpienia go w konstruktorze. Zadeklarować dwa `CGPoint` zmiennych, `initialPoint` i `latestPoint`, użyjemy do przechwytywania punktu touch, z którego możemy utworzyć ścieżki:
    
    ```csharp
    public class DemoView : UIView
    {
        CGPath path;
        CGPoint initialPoint;
        CGPoint latestPoint;
    
        public DemoView ()
        {
            BackgroundColor = UIColor.White;
    
            path = new CGPath ();
        }
    }
    ```

2. Dodaj następujące dyrektyw using:

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. Następnie zastąp `TouchesBegan` i `TouchesMoved,` i dodaj następujące implementacje przechwytywania punktu początkowego touch oraz w każdym punkcie kolejnych touch odpowiednio:

    ```csharp
    public override void TouchesBegan (NSSet touches, UIEvent evt){
    
        base.TouchesBegan (touches, evt);
    
        UITouch touch = touches.AnyObject as UITouch;
        
        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }
    
    public override void TouchesMoved (NSSet touches, UIEvent evt){
    
        base.TouchesMoved (touches, evt);
    
        UITouch touch = touches.AnyObject as UITouch;
        
        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }
    ```

    `SetNeedsDisplay` zostanie wywołana zawsze poprawki Przenieś aby `Draw` ma być wywołane na następny przebieg wykonywania pętli.

4. Dodamy wierszy do ścieżki w `Draw` — metoda i użyj czerwona linia przerywana do rysowania z. [Implementowanie `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) kodem przedstawionym poniżej:

    ```csharp
    public override void Draw (CGRect rect){
    
        base.Draw (rect);
    
        if (!initialPoint.IsEmpty) {
    
            //get graphics context
            using(CGContext g = UIGraphics.GetCurrentContext ()){
                    
                //set up drawing attributes
                g.SetLineWidth (2);
                UIColor.Red.SetStroke ();
    
                //add lines to the touch points
                if (path.IsEmpty) {
                    path.AddLines (new CGPoint[]{initialPoint, latestPoint});
                } else {
                    path.AddLineToPoint (latestPoint);
                }
            
                //use a dashed line
                g.SetLineDash (0, new nfloat[] { 5, 2 * (nfloat)Math.PI });
                                
                //add geometry to graphics context and draw it
                g.AddPath (path);       
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
    ```

Czy możemy uruchomić aplikację teraz, możemy touch do rysowania na ekranie, jak pokazano na poniższym zrzucie ekranu:

![](graphics-animation-walkthrough-images/01-path.png "Na ekranie")

## <a name="animating-along-a-path"></a>Animowanie wzdłuż ścieżki

Teraz, wdrożonych kod, aby umożliwić użytkownikom narysuj ścieżkę, możemy dodać kod, aby animować warstwy ścieżce narysowanego.

1. Najpierw dodaj [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md) zmiennej do klasy i utwórz go w Konstruktorze:

    ```csharp
    public class DemoView : UIView
        {
            …
    
            CALayer layer;
    
            public DemoView (){
                …
    
                //create layer
                layer = new CALayer ();
                layer.Bounds = new CGRect (0, 0, 50, 50);
                layer.Position = new CGPoint (50, 50);
                layer.Contents = UIImage.FromFile ("monkey.png").CGImage;
                layer.ContentsGravity = CALayer.GravityResizeAspect;
                layer.BorderWidth = 1.5f;
                layer.CornerRadius = 5;
                layer.BorderColor = UIColor.Blue.CGColor;
                layer.BackgroundColor = UIColor.Purple.CGColor;
            }
    ```

2. Następnie dodamy warstwy jako podwarstwa warstwy widoku, gdy użytkownik podnosi się ich palca z ekranu. Następnie zostanie utworzony przy użyciu ścieżki, odtwarzania klatki kluczowej animacji warstwy `Position`.

    Należy zastąpić w tym celu `TouchesEnded` i Dodaj następujący kod:

    ```csharp
    public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            base.TouchesEnded (touches, evt);

            //add layer with image and animate along path

            if (layer.SuperLayer == null)
                Layer.AddSublayer (layer);

            // create a keyframe animation for the position using the path
            layer.Position = latestPoint;
            CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
            animPosition.Path = path;
            animPosition.Duration = 3;
            layer.AddAnimation (animPosition, "position");
        }
    ```

3. Uruchom aplikację teraz i po rysunku, warstwy z obrazem zostanie dodany i podróżuje rysowanej ścieżce:

![](graphics-animation-walkthrough-images/00-final-app.png "Warstwa obrazu zostanie dodany i porusza się na ścieżce narysowanego")

## <a name="summary"></a>Podsumowanie

W tym artykule firma Microsoft przeprowadził przez przykładowy ze sobą powiązane grafiki i animacji pojęcia. Po pierwsze, możemy pokazano, jak na potrzeby grafiki Core rysowanie ścieżki `UIView` w odpowiedzi na touch użytkownika. Firma Microsoft pokazano, jak pomocą animacji Core obraz przesyłane w tej ścieżce.


## <a name="related-links"></a>Linki pokrewne

- [Podstawowe funkcje animacji](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Podstawowe funkcje grafiki](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Podstawowe przepisami animacji](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
