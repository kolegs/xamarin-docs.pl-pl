---
title: TextKit
description: "Interfejsu API zestawu tekstu oferuje zaawansowane tekst układ i renderowania funkcje w Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 7ae41e99d20f0e8f3cad6b933e415002903a3294
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="text-kit"></a>Zestaw tekstu

Tekst Kit to nowy interfejs API, który oferuje tekst zaawansowane funkcje układ i renderowania. Jest oparty na niski poziom framework Core tekstu, ale jest znacznie prostsze niż tekst Core korzystanie z.

Aby udostępnić funkcje tekstu zestaw standardowych kontrolek, kilka formantów tekstu iOS zostały ponownie wdrożony, aby użyć zestawu tekstu, w tym:

-  UITextView
-  UITextField
-  UILabel


## <a name="architecture"></a>Architektura

Zestaw tekst zawiera warstwowa architektura, która oddziela przechowywanie tekstu z układu i wyświetlania, w tym następujące klasy:

-  `NSTextContainer` — Zapewnia współrzędnych i geometry, który jest używany do układu tekstu.
-  `NSLayoutManager` — Wychodzi poza tekstu przez włączenie tekstu do symboli. 
-  `NSTextStorage` — Przechowuje dane tekstowe, a także obsługuje aktualizacji właściwości tekst partii. Wszelkie aktualizacje wsadowe są przekazywane do menedżera układu rzeczywiste przetwarzanie zmian, takie jak ponowne obliczanie układ i ponownego narysowania tekst.


Te trzy klasy są stosowane do widoku, który renderuje tekstu. Tekst wbudowany obsługi widoków, takich jak `UITextView`, `UITextField`, i `UILabel` już je ustawić, ale można utworzyć i zastosować je do dowolnego `UIView` również wystąpienia.

Na poniższym rysunku przedstawiono tej architektury:

 ![](textkit-images/textkitarch.png "Poniższy rysunek przedstawia architektura zestawu tekstu")

## <a name="text-storage-and-attributes"></a>Przechowywanie tekstu i atrybuty

`NSTextStorage` Klasa przechowuje tekst, który jest wyświetlany w widoku. Komunikuje się także wszelkie zmiany tekstu — na przykład zmiany znaków lub ich atrybutów — Menedżer układu do wyświetlenia. `NSTextStorage` dziedziczy `MSMutableAttributed` ciąg, dzięki czemu zmiany atrybutów tekstu, należy określić w partiach między `BeginEditing` i `EndEditing` wywołania.

Na przykład poniższy fragment kodu określa zmianę pierwszego planu i tła kolory i jest przeznaczony dla konkretnego zakresów:

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

Po `EndEditing` jest wywoływana, zmiany są wysyłane do menedżera układu, który z kolei wykonuje wszelkie niezbędne układ i obliczeń renderowanie tekstu, który będzie wyświetlany w widoku.

## <a name="layout-with-exclusion-path"></a>Układ z ścieżka wykluczenia

Zestaw tekstu również obsługuje układ i umożliwia złożonych scenariuszy, takich jak wielokolumnowego tekstu i przechodzenia tekstem określonych ścieżek o nazwie *ścieżki wykluczeń*. Ścieżki wykluczeń są stosowane do kontenera tekst, który modyfikuje geometrii układu tekstu, powodując przebieg wokół określonych ścieżek tekstu.

Dodawanie ścieżka wykluczenia wymaga ustawienia `ExclusionPaths` właściwości Menedżera układu. Ustawienie tej właściwości powoduje, że Menedżer układu unieważnienie układu tekstu i przepływu tekstu wokół ścieżka wykluczenia.

### <a name="exclusion-based-on-a-cgpath"></a>Oparte na CGPath wykluczeń

Należy rozważyć `UITextView` podklasy implementacji:

```csharp
public class ExclusionPathView : UITextView
{
    CGPath exclusionPath;
    CGPoint initialPoint;
    CGPoint latestPoint;
    UIBezierPath bezierPath;

    public ExclusionPathView (string text)
    {
        Text = text;
        ContentInset = new UIEdgeInsets (20, 0, 0, 0);
        BackgroundColor = UIColor.White;
        exclusionPath = new CGPath ();
        bezierPath = UIBezierPath.Create ();

        LayoutManager.AllowsNonContiguousLayout = false;
    }

    public override void TouchesBegan (NSSet touches, UIEvent evt)
    {
        base.TouchesBegan (touches, evt);

        var touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt)
    {
        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }

    public override void TouchesEnded (NSSet touches, UIEvent evt)
    {
        base.TouchesEnded (touches, evt);

        bezierPath.CGPath = exclusionPath;
        TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
    }

    public override void Draw (CGRect rect)
    {
        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            using (var g = UIGraphics.GetCurrentContext ()) {

                g.SetLineWidth (4);
                UIColor.Blue.SetStroke ();

                if (exclusionPath.IsEmpty) {
                    exclusionPath.AddLines (new CGPoint[] { initialPoint, latestPoint });
                } else {
                    exclusionPath.AddLineToPoint (latestPoint);
                }

                g.AddPath (exclusionPath);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
}
```

Ten kod dodaje obsługę Rysowanie w widoku tekstu przy użyciu grafiki Core. Ponieważ `UITextView` klasy jest teraz wbudowany do renderowania tekstu i układu za pomocą zestawu tekstu, można użyć wszystkich funkcji zestawu tekstu, takie jak ustawianie ścieżki wykluczeń.

> [!IMPORTANT]
>   Uwaga: Ten przykład podklasy `UITextView` można dodać touch rysowania pomocy technicznej. Tworzenie podklas `UITextView` nie jest niezbędne, aby uzyskać funkcji zestawu tekstu.



Po użytkownik rysuje w widoku tekstu, narysowanego `CGPath` jest stosowany do `UIBezierPath` wystąpienia przez ustawienie `UIBezierPath.CGPath` właściwości:

```csharp
bezierPath.CGPath = exclusionPath;
```

Aktualizowanie następujący wiersz kodu powoduje układu tekstu zaktualizować ścieżkę:

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

Poniższy zrzut ekranu przedstawia sposób zmiany układu tekstu przepływ narysowanego ścieżkę:

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")--> 
![](textkit-images/exclusionpath2.png "Ten zrzut ekranu pokazano, jak układu tekstu zmienia przepływ narysowanego ścieżkę")

Zwróć uwagę, że Menedżer układu `AllowsNonContiguousLayout` właściwość jest ustawiona na wartość false w takim przypadku. Powoduje to układu ponownego obliczenia we wszystkich przypadkach, gdy tekst zostanie zmieniony. To ustawienie na wartość true, mogą korzystać wydajności dzięki unikaniu odświeżenia układu pełnej, szczególnie w przypadku dużych dokumentów. Jednak ustawienie `AllowsNonContiguousLayout` na wartość true uniemożliwiłyby ścieżka wykluczenia aktualizowanie układu w niektórych przypadkach — na przykład, jeśli wprowadzony tekst w czasie wykonywania bez powrotu karetki końcowe przed ścieżki ustawiany.


## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do systemu iOS 7 (przykład)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Omówienie interfejsu użytkownika systemu iOS 7](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Uruchamianie procesów w tle](~/ios/app-fundamentals/backgrounding/index.md)
