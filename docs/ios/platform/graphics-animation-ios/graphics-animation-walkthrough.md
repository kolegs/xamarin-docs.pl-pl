---
title: Za pomocą podstawowe funkcje grafiki i podstawowe funkcje animacji w rozszerzeniu Xamarin.iOS
description: W tym artykule przedstawiono krok po kroku jak utworzyć aplikację, która używa podstawowe funkcje grafiki i podstawowe funkcje animacji. Przedstawia on sposób rysowania na ekranie w odpowiedzi na dotyk użytkownika, a także jak animować obraz podróży w ścieżce.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: cecfd7f3a9678f298af3ed547aa7b50a18238729
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242007"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>Za pomocą podstawowe funkcje grafiki i podstawowe funkcje animacji w rozszerzeniu Xamarin.iOS

W tym przewodniku użyjemy rysowanie ścieżki wprowadzania dotykowego, za pomocą podstawowe funkcje grafiki w odpowiedzi. Następnie dodamy `CALayer` zawierającego obraz, który firma Microsoft będzie animować wzdłuż ścieżki.

Poniższy zrzut ekranu przedstawia gotową aplikację:

![](graphics-animation-walkthrough-images/00-final-app.png "Ukończona aplikacja")

Przed rozpoczęciem pobierania *GraphicsDemo* przykładowe znajdująca się na tym przewodniku. Można go pobrać [tutaj](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) i znajduje się wewnątrz **GraphicsWalkthrough** katalogu uruchamiania projektu o nazwie **GraphicsDemo_starter** przez dwukrotne kliknięcie, i Otwórz `DemoView` klasy.

## <a name="drawing-a-path"></a>Rysowanie ścieżki


1. W `DemoView` Dodaj `CGPath` zmiennej do klasy i tworzenia jego instancji w konstruktorze. Również zadeklarować dwa `CGPoint` zmiennych, `initialPoint` i `latestPoint`, firma Microsoft użyje do przechwytywania punktu touch, z którego możemy utworzyć ścieżki:
    
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

2. Dodaj następujące dyrektywy using:

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. Następnie zastąp `TouchesBegan` i `TouchesMoved,` i dodaj następujące implementacje do przechwytywania punktu początkowego touch i każdy punkt kolejnych touch odpowiednio:

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

    `SetNeedsDisplay` będzie być wywoływana za każdym razem, ma przenieść aby `Draw` ma być wywołane na następny przebieg wykonywania pętli.

4. Dodamy wierszy w ścieżce w `Draw` metody i użyj czerwona linia przerywana do rysowania za pomocą. [Implementowanie `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) przy użyciu kodu pokazany poniżej:

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

Jeśli możemy uruchomić aplikację teraz, firma Microsoft dotyku Aby rysować na ekranie, jak pokazano na poniższym zrzucie ekranu:

![](graphics-animation-walkthrough-images/01-path.png "Rysowanie na ekranie")

## <a name="animating-along-a-path"></a>Animowanie w ścieżce

Teraz, gdy wdrożyliśmy kodu, aby umożliwić użytkownikom rysowanie ścieżki możemy dodać kod, aby animować warstwy rysowane ścieżce.

1. Najpierw dodaj [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md) zmiennej do klasy i utworzyć ją w Konstruktorze:

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

2. Następnie dodamy warstwy jako podwarstwę warstwę widoku po użytkownik wind się ich palców od ekranu. Następnie utworzymy odtwarzania klatki kluczowej przy użyciu ścieżki, animowanie warstwy `Position`.

    W tym należy zastąpić `TouchesEnded` i Dodaj następujący kod:

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

3. Teraz, jak i po nim rysowania, należy uruchomić aplikację, warstwy za pomocą obrazu zostanie dodany oraz przybliżone ilości tych danych na ścieżce rysowane:

![](graphics-animation-walkthrough-images/00-final-app.png "Warstwy za pomocą obrazu zostanie dodany oraz przybliżone ilości tych danych na ścieżce rysowane")

## <a name="summary"></a>Podsumowanie

W tym artykule będziemy schodkowego za pośrednictwem przykładowi, który ze sobą powiązane grafiki i animacje pojęcia. Po pierwsze, firma Microsoft pokazano, jak podstawowe funkcje grafiki umożliwia rysowanie ścieżki `UIView` w odpowiedzi na dotyk użytkownika. Firma Microsoft pokazano, jak utworzyć obraz podróży w tej ścieżce za pomocą podstawowe funkcje animacji.


## <a name="related-links"></a>Linki pokrewne

- [Podstawowe funkcje animacji](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Podstawowe funkcje grafiki](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Podstawowe przepisy animacji](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
