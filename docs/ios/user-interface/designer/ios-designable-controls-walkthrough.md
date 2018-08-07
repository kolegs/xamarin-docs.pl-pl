---
title: Używanie kontrolek niestandardowych w narzędziu iOS Designer
description: W tym dokumencie opisano, jak utworzyć formant niestandardowy i używać go za pomocą projektanta platformy Xamarin dla systemu iOS. Widoczny jest sposób udostępnić kontrolki przybornika projektanta systemu iOS, wdrożenia kontroli, tak aby poprawnie renderowana i zaprojektować czasu i nie tylko.
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 0097cdf006944a51d938ea91d3ea0b0c2aee08cf
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573584"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>Używanie kontrolek niestandardowych w narzędziu iOS Designer

## <a name="requirements"></a>Wymagania

Projektant platformy Xamarin dla systemu iOS dostępnej w programie Visual Studio dla komputerów Mac i Visual Studio 2015 i 2017 na Windows.

W tym przewodniku założono znajomość zawartość objęte [wprowadzenie prowadzi](~/ios/get-started/index.md).

## <a name="walkthrough"></a>Przewodnik

> [!IMPORTANT]
> Począwszy od wersji 5.5 Xamarin.Studio sposób, w którym tworzone są niestandardowe formanty różni się nieco do wcześniejszych wersji. Aby utworzyć formant niestandardowy, albo `IComponent` interfejsu jest wymagana (przy użyciu metody wdrożenia skojarzone) lub klasa może być być oznaczona przy użyciu `[DesignTimeVisible(true)]`. Druga metoda jest używana w poniższym przykładzie wskazówki.


1. Utwórz nowe rozwiązanie z **dla systemu iOS > aplikacji > Aplikacja pojedynczego widoku > C#** szablonu, nadaj jej nazwę `ScratchTicket`i kontynuować pracę w Kreatorze nowego projektu:

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


1. Dodaj `FillTexture.png`, `FillTexture2.png` i `Monkey.png` plików (dostępne [z serwisu GitHub](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)) do **zasobów** folderu.
    
1. Kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć go w Projektancie:

    [![](ios-designable-controls-walkthrough-images/03new.png "Narzędzia iOS Designer")](ios-designable-controls-walkthrough-images/03new.png#lightbox)


1. Przeciągania i upuszczania **widoku obrazu** z **przybornika** na widok w scenorysu.

    [![](ios-designable-controls-walkthrough-images/04new.png "Wyświetl obraz dodawane do układu")](ios-designable-controls-walkthrough-images/04new.png#lightbox)


1. Wybierz **widoku obrazu** i zmień jego **obraz** właściwość `Monkey.png`.

    [![](ios-designable-controls-walkthrough-images/05new.png "Ustawienie właściwości obrazu Wyświetl obraz na Monkey.png")](ios-designable-controls-walkthrough-images/05new.png#lightbox)

    
1. Jak firma Microsoft korzystającego z klas rozmiaru musimy ograniczyć ten widok obrazu. Kliknij obraz, dwa razy, aby przełączyć w tryb ograniczenia. Umożliwia ograniczenie do Centrum, klikając uchwyt przypinanie Centrum i wyrównanie go w pionie i w poziomie:

    [![](ios-designable-controls-walkthrough-images/06new.png "Centrowanie obrazu")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Aby ograniczyć szerokość i wysokość, kliknij obsługuje przypinanie rozmiaru (uchwytów "kości" w kształcie) i wybierz pozycję szerokości i wysokości odpowiednio:

    [![](ios-designable-controls-walkthrough-images/07new.png "Dodawanie ograniczenia")](ios-designable-controls-walkthrough-images/07new.png#lightbox)


1. Zaktualizuj ramki na podstawie ograniczeń, klikając przycisk Aktualizuj, na pasku narzędzi:

    [![](ios-designable-controls-walkthrough-images/08new.png "Pasek narzędzi ograniczeń")](ios-designable-controls-walkthrough-images/08new.png#lightbox)


1. Następnie, skompiluj projekt, aby **Scratch widoku biletu** pojawi się w obszarze **składników niestandardowych** w przyborniku:

    [![](ios-designable-controls-walkthrough-images/09new.png "Przybornik składników niestandardowych")](ios-designable-controls-walkthrough-images/09new.png#lightbox)


1. Przeciąganie i upuszczanie **Scratch widoku biletu** tak, aby wyświetlał się nad małp obrazu. Dopasuj przeciągnij uchwyty, więc Wyświetl pliki tymczasowe biletu obejmuje małp całkowicie, jak pokazano poniżej:

    [![](ios-designable-controls-walkthrough-images/10new.png "Widok pliki tymczasowe bilet za pośrednictwem widoku obrazu")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Ograniczenie widoku biletu pliki tymczasowe na wyświetlanie obrazów za pomocą rysowania prostokąt otaczający, aby wybrać obu widokach. Wybierz opcje, aby ograniczyć do ramek szerokość, wysokość, Centrum i Środkowej i aktualizacji, na podstawie ograniczeń, jak pokazano poniżej:

    [![](ios-designable-controls-walkthrough-images/11new.png "Wyśrodkowanie i dodawanie ograniczeń")](ios-designable-controls-walkthrough-images/11new.png#lightbox)


1. Uruchom aplikację, a następnie "scratch off" obraz, aby wyświetlić małp.

    [![](ios-designable-controls-walkthrough-images/10-app.png "Uruchom przykładową aplikację")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Dodawanie właściwości czasu projektowania

Projektant zawiera również w przypadku kontrolek niestandardowych właściwości typu liczbowego, wyliczenia, string, bool, CGSize, UIColor i UIImage obsługi w czasie projektowania. Aby zademonstrować, możemy dodać właściwość, która ma `ScratchTicketView` można ustawić obrazu, który jest "wewnętrzna wyłączone."

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

Może również chcemy dodać sprawdzanie wartości null do `Draw` metody, w następujący sposób:

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

W tym `ExportAttribute` i `BrowsableAttribute` z wartością argumentu `true` skutkuje właściwości są wyświetlane w oknie Projektant **właściwość** panelu. Zmiana wartości właściwości do innego obrazu, dołączone do projektu, taki jak `FillTexture2.png`, powoduje aktualizowanie formantu w czasie projektowania, jak pokazano poniżej:

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "Edytowanie właściwości czasu projektowania")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Podsumowanie

W tym artykule będziemy tamtych jak utworzyć formant niestandardowy, a także pobrać go w aplikacji systemu iOS przy użyciu narzędzia iOS designer. Firma Microsoft pokazaliśmy, jak utworzyć i skompilować formant aby był dostępny do aplikacji za pomocą projektanta **przybornika**. Ponadto zobaczyliśmy, jak zaimplementować formantu w taki sposób, że poprawnie renderowania w czasie projektowania i w czasie wykonywania, a także jak udostępniać właściwości kontrolki niestandardowej w projektancie.



## <a name="related-links"></a>Linki pokrewne

- [ScratchTicket (przykład)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [wymagane obrazy (przykład)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
