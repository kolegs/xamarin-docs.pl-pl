---
title: Używanie formantów niestandardowych z systemem iOS projektanta
description: Ten dokument zawiera opis sposobu utworzyć niestandardowego formantu i korzystać z niego przy użyciu projektanta Xamarin dla systemu iOS. Widoczny jest sposób udostępnić formantu w przyborniku projektanta dla systemu iOS, zaimplementować formantu, aby poprawnie renderowania i projektowania czasu i inne.
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: dae675d65cb2be93ac828a1aebe560354630ab54
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790168"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>Używanie formantów niestandardowych z systemem iOS projektanta

## <a name="requirements"></a>Wymagania

Projektant Xamarin dla systemu iOS jest dostępna w programie Visual Studio for Mac i Visual Studio 2015 i 2017 w systemie Windows.

W tym przewodniku założono znajomość zawartość objęte [wprowadzenie prowadzi](~/ios/get-started/index.md).

## <a name="walkthrough"></a>Wskazówki

> [!IMPORTANT]
> Począwszy od wersji 5.5 Xamarin.Studio sposób, w którym są tworzone Kontrolki niestandardowe różni się nieznacznie we wcześniejszych wersjach. Można utworzyć niestandardowego formantu, albo `IComponent` interfejsu jest wymagana (za pomocą metody skojarzone implementacji) lub klasa może być oznaczony za pomocą `[DesignTimeVisible(true)]`. Druga metoda jest używany w poniższym przykładzie wskazówki.


1. Utworzenie nowego rozwiązania z **systemu iOS > aplikacji > Aplikacja pojedynczego widoku > C#** szablonu, nadaj jej nazwę `ScratchTicket`i kontynuuj pracę Kreatora nowego projektu:

    [![](ios-designable-controls-walkthrough-images/01new.png "Utwórz nowe rozwiązanie")](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. Utwórz nowy plik pustą klasę o nazwie `ScratchTicketView`:

    [![](ios-designable-controls-walkthrough-images/02new.png "Utwórz nową klasę ScratchTicketView")](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. Dodaj następujący kod do `ScratchTicketView` klasy:

    ```csharp
    using System;
    using System.ComponentModel;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace ScratchTicket
    {
        [Register("ScratchTicketView"), DesignTimeVisible(true)]
        public class ScratchTicketView : UIView
        {
            CGPath path;
            CGPoint initialPoint;
            CGPoint latestPoint;
            bool startNewPath = false;
            UIImage image;
    
            [Export("Image"), Browsable(true)]
            public UIImage Image
            {
                get { return image; }
                set
                {
                    image = value;
                    SetNeedsDisplay();
                }
            }
    
            public ScratchTicketView(IntPtr p)
                : base(p)
            {
                Initialize();
            }
    
            public ScratchTicketView()
            {
                Initialize();
            }
    
            void Initialize()
            {
                initialPoint = CGPoint.Empty;
                latestPoint = CGPoint.Empty;
                BackgroundColor = UIColor.Clear;
                Opaque = false;
                path = new CGPath();
                SetNeedsDisplay();
            }
    
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    initialPoint = touch.LocationInView(this);
                }
            }
    
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    latestPoint = touch.LocationInView(this);
                    SetNeedsDisplay();
                }
            }
    
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                startNewPath = true;
            }
    
            public override void Draw(CGRect rect)
            {
                base.Draw(rect);
    
                using (var g = UIGraphics.GetCurrentContext())
                {
                    if (image != null)
                        g.SetFillColor((UIColor.FromPatternImage(image).CGColor));
                    else
                        g.SetFillColor(UIColor.LightGray.CGColor);
                    g.FillRect(rect);
    
                    if (!initialPoint.IsEmpty)
                    {
                        g.SetLineWidth(20);
                        g.SetBlendMode(CGBlendMode.Clear);
                        UIColor.Clear.SetColor();
    
                        if (path.IsEmpty || startNewPath)
                        {
                            path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                            startNewPath = false;
                        }
                        else
                        {
                            path.AddLineToPoint(latestPoint);
                        }
    
                        g.SetLineCap(CGLineCap.Round);
                        g.AddPath(path);        
                        g.DrawPath(CGPathDrawingMode.Stroke);
                    }
                }
            }
        }
    }
    ```


1. Dodaj `FillTexture.png`, `FillTexture2.png` i `Monkey.png` plików (dostępne [z usługi GitHub](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)) do **zasobów** folderu.
    
1. Kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć go w Projektancie:

    [![](ios-designable-controls-walkthrough-images/03new.png "Projektant dla systemu iOS")](ios-designable-controls-walkthrough-images/03new.png#lightbox)


1. Przeciągnij i upuść **widoku obrazu** z **przybornika** na widok scenorysu.

    [![](ios-designable-controls-walkthrough-images/04new.png "Wyświetl obraz dodany do układu")](ios-designable-controls-walkthrough-images/04new.png#lightbox)


1. Wybierz **widoku obrazu** i zmień jego **obrazu** właściwości `Monkey.png`.

    [! [] (z systemem ios-designable — formanty — wskazówki — obrazy/05new.png "obraz widoku obrazu ustawienie właściwości Monkey.png)](ios-designable-controls-walkthrough-images/05new.png#lightbox)

    
1. Ponieważ używamy klasy wielkości musimy ograniczyć ten widok obrazu. Kliknij go dwukrotnie, aby przełączyć w tryb ograniczenia. Umożliwia ograniczenie go do Centrum, klikając uchwyt przypinanie Centrum i jego wyrównanie w pionie i w poziomie:

    [![](ios-designable-controls-walkthrough-images/06new.png "Centrowanie obrazu")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Aby ograniczyć szerokość i wysokość, kliknij uchwytów przypinanie rozmiaru (dojść w kształcie "kości") i wybierz polecenie szerokości i wysokości odpowiednio:

    [![](ios-designable-controls-walkthrough-images/07new.png "Dodanie ograniczeń")](ios-designable-controls-walkthrough-images/07new.png#lightbox)


1. Aktualizacja ramki na podstawie ograniczeń przez kliknięcie przycisku Aktualizuj na pasku narzędzi:

    [![](ios-designable-controls-walkthrough-images/08new.png "Na pasku narzędzi z ograniczeniami")](ios-designable-controls-walkthrough-images/08new.png#lightbox)


1. Następnie Skompiluj projekt, aby **pliki tymczasowe widoku biletu** będą wyświetlane w obszarze **niestandardowe składniki** w przyborniku:

    [![](ios-designable-controls-walkthrough-images/09new.png "Niestandardowe składniki przybornika")](ios-designable-controls-walkthrough-images/09new.png#lightbox)


1. Przeciągnij i upuść **pliki tymczasowe widoku biletu** , aby był wyświetlany nad małp obrazu. Dopasuj uchwyty przeciągania, więc widoku biletu pliki tymczasowe obejmuje małp całkowicie, jak pokazano poniżej:

    [![](ios-designable-controls-walkthrough-images/10new.png "Pliki tymczasowe biletu widoku za pośrednictwem widoku obrazu")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Ogranicz widoku biletu przechowywania plików tymczasowych do widoku obrazu za pomocą rysowania prostokąt ograniczający, aby wybrać obu widokach. Wybierz opcje, aby ograniczyć do ramki szerokość, wysokość, wyśrodkowanie i bliski i aktualizacji, na podstawie ograniczeń, jak pokazano poniżej:

    [![](ios-designable-controls-walkthrough-images/11new.png "Wyśrodkowanie i dodanie ograniczeń")](ios-designable-controls-walkthrough-images/11new.png#lightbox)


1. Uruchom aplikację i "pliki tymczasowe wyłączanie" obraz, aby ujawnić małp.

    [![](ios-designable-controls-walkthrough-images/10-app.png "Uruchom przykładową aplikację")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Dodawanie właściwości czasu projektowania

Projektant obejmuje również obsługę czasu projektowania w przypadku kontrolek niestandardowych właściwości typu liczbowego, wyliczenie, string, bool, CGSize, UIColor i UIImage. Aby zademonstrować, możemy dodać właściwości do `ScratchTicketView` można ustawić obrazu, który jest "wewnętrzna poza."

Dodaj następujący kod do `ScratchTicketView` klasy dla właściwości:

```csharp
[Export("Image"), Browsable(true)]
public UIImage Image 
{
    get { return image; }
    set { 
            image = value;
            SetNeedsDisplay ();
        }
}
```

Firma Microsoft może być również dodać sprawdzania wartości null do `Draw` metody, w następujący sposób:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (var g = UIGraphics.GetCurrentContext())
    {
        if (image != null)
            g.SetFillColor ((UIColor.FromPatternImage (image).CGColor));
        else
            g.SetFillColor (UIColor.LightGray.CGColor);
            
        g.FillRect(rect);

        if (!initialPoint.IsEmpty)
        {
             g.SetLineWidth(20);
             g.SetBlendMode(CGBlendMode.Clear);
             UIColor.Clear.SetColor();

             if (path.IsEmpty || startNewPath)
             {
                 path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                 startNewPath = false;
             }
             else
             {
                 path.AddLineToPoint(latestPoint);
             }

             g.SetLineCap(CGLineCap.Round);
             g.AddPath(path);       
             g.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

W tym `ExportAttribute` i `BrowsableAttribute` z wartością argumentu `true` powoduje właściwości wyświetlane w Projektancie **właściwości** panelu. Zmiana wartości właściwości do innego obrazu dostępnych w projekcie, takich jak `FillTexture2.png`, powoduje aktualizowanie formantu w czasie projektowania, jak pokazano poniżej:

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "Edytowanie właściwości czasu projektowania")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Podsumowanie

W tym artykule Rezygnacja za pośrednictwem jak utworzyć niestandardowego formantu, jak również pobrać go w aplikacji systemu iOS przy użyciu narzędzia Projektant z systemem iOS. Widzieliśmy sposobu tworzenia i tworzenie formantu, aby udostępnić ją do aplikacji w Projektancie **przybornika**. Ponadto analizujemy implementowania formantu w taki sposób, że poprawnie renderuje się zarówno w czasie projektowania, jak i środowiska uruchomieniowego, a także sposobu udostępniają właściwości kontrolki niestandardowej w projektancie.



## <a name="related-links"></a>Linki pokrewne

- [ScratchTicket (przykład)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [wymagane obrazy (przykład)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
