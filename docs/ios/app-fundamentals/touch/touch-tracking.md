---
title: Obsługa wielodotyku palca śledzenia w Xamarin.iOS
description: Ten dokument zawiera opis sposobu śledzić poszczególne palców w wielodotyku gestów w aplikacji platformy Xamarin.iOS. Koncentruje się wokół finger-painting przykład aplikacji.
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 85dbd3c158408026f4ecef5fb2b01c265747140e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784406"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>Obsługa wielodotyku palca śledzenia w Xamarin.iOS

_W tym dokumencie przedstawiono sposób śledzenia zdarzeń touch z wielu palcami_

Brak razy, gdy aplikacja wielodotyku musi śledzić poszczególne palców, jak przenieść jednocześnie na ekranie. Jeden Typowa aplikacja jest programem finger-paint. Chcesz, aby umożliwić użytkownikom rysowanie za pomocą jednej linii papilarnych, ale także Rysowanie za pomocą wielu palców jednocześnie. Jak program przetwarza wiele zdarzeń touch, musi rozróżnienia tych palców.

W przypadku palcem najpierw krawędzi ekranu, tworzy iOS [ `UITouch` ](https://developer.xamarin.com/api/type/UIKit.UITouch/) obiektu dla tej linii papilarnych. Ten obiekt jest taka sama, jak palca przenosi na ekranie, a następnie podnosi na ekranie, w którym obiekt jest usunięty. Aby śledzić palców, program należy unikać przechowywania to `UITouch` obiekt bezpośrednio. Zamiast tego można użyć [ `Handle` ](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) właściwości typu `IntPtr` do unikatowego identyfikowania te `UITouch` obiektów.

Program, który śledzi palców poszczególnych obsługuje prawie zawsze słownik na potrzeby śledzenia touch. Dla programu iOS jest klucz słownika `Handle` wartość, która identyfikuje określonej linii papilarnych. Wartość słownika zależy od aplikacji. W [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) program, każde pociągnięcie palca (od touch zwolnienia) jest skojarzony z obiekt, który zawiera wszystkie informacje niezbędne do renderowania wiersza z tym palca. Program definiuje małą `FingerPaintPolyline` klasy w tym celu:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new CGPath();
    }

    public CGColor Color { set; get; }

    public float StrokeWidth { set; get; }

    public CGPath Path { private set; get; }
}
```

Każdej linii łamanej ma kolor, szerokość pociągnięć i grafiki iOS [ `CGPath` ](https://developer.xamarin.com/api/type/CoreGraphics.CGPath/) obiektu gromadzenia i renderowania wiele punktów wiersza, ponieważ jest rysowana.


Wszystkie pozostałe kodu pokazano poniżej znajduje się w `UIView` zależnych o nazwie `FingerPaintCanvasView`. Czy klasy utrzymują słownik obiektów typu `FingerPaintPolyline` w czasie są one aktywnie rysowane co najmniej jeden palców:

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

Ten słownik umożliwia widok, aby szybko uzyskać `FingerPaintPolyline` na podstawie informacji skojarzonych z każdym palca `Handle` właściwość `UITouch` obiektu.

`FingerPaintCanvasView` Zachowuje również klasy `List` obiektu linię, które zostały ukończone:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Obiekty w tym `List` znajdują się w tej samej kolejności, że zostały one wstawiany.

`FingerPaintCanvasView` Zastępuje pięć metody zdefiniowane przez `View`:

- [`TouchesBegan`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesBegan/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesMoved`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesMoved/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesEnded`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesEnded/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesCancelled`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesCancelled/p/Foundation.NSSet/UIKit.UIEvent/)
- [`Draw`](https://developer.xamarin.com/api/member/UIKit.UIView.Draw/p/CoreGraphics.CGRect/)

Różnych `Touches` punktów, które tworzą linię kumulują się zastąpienia.

[`Draw`] Zastąpienie rysuje linię ukończone, a następnie linię w toku:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (CGContext context = UIGraphics.GetCurrentContext())
    {
        // Stroke settings
        context.SetLineCap(CGLineCap.Round);
        context.SetLineJoin(CGLineJoin.Round);

        // Draw the completed polylines
        foreach (FingerPaintPolyline polyline in completedPolylines)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }

        // Draw the in-progress polylines
        foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Każdy z `Touches` zastąpienia potencjalnie raporty akcje wielu palców, wskazane przez jedną lub więcej `UITouch` obiekty przechowywane w `touches` argument do metody. `TouchesBegan` Zastąpienia pętlę z powodu tych obiektów. Dla każdego `UITouch` obiektów, metoda tworzy i inicjuje nowy `FingerPaintPolyline` obiektu, w tym przechowywania początkową lokalizację palca uzyskane z `LocationInView` metody. To `FingerPaintPolyline` obiekt jest dodawany do `InProgressPolylines` za pomocą słownika `Handle` właściwość `UITouch` obiektu jako klucza słownika:

```csharp
public override void TouchesBegan(NSSet touches, UIEvent evt)
{
    base.TouchesBegan(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Create a FingerPaintPolyline, set the initial point, and store it
        FingerPaintPolyline polyline = new FingerPaintPolyline
        {
            Color = StrokeColor,
            StrokeWidth = StrokeWidth,
        };

        polyline.Path.MoveToPoint(touch.LocationInView(this));
        inProgressPolylines.Add(touch.Handle, polyline);
    }
    SetNeedsDisplay();
}
```

Stwierdza, wywołując metodę `SetNeedsDisplay` do generowania wywołanie `Draw` zastąpienia i zaktualizować ekranu.

Gdy palca lub palców są przenoszone na ekranie `View` pobiera wielu wywołań do jego `TouchesMoved` zastąpienia. To zastąpienie podobnie w pętli `UITouch` obiekty przechowywane w `touches` argumentu i dodaje do ścieżki grafiki bieżącą lokalizację palca:

```csharp
public override void TouchesMoved(NSSet touches, UIEvent evt)
{
    base.TouchesMoved(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Add point to path
        inProgressPolylines[touch.Handle].Path.AddLineToPoint(touch.LocationInView(this));
    }
    SetNeedsDisplay();
}
```

`touches` Kolekcja zawiera tylko te `UITouch` obiekty palców, które zostały przeniesione od czasu ostatniego wywołania `TouchesBegan` lub `TouchesMoved`. Jeśli kiedykolwiek zajdzie `UITouch` obiekty odpowiadające *wszystkie* palcami obecnie kontaktu z ekranu, informacje te są dostępne za pośrednictwem `AllTouches` właściwość `UIEvent` argument do metody.

`TouchesEnded` Zastąpienie ma dwa zadania. Niezbędny ostatniego punktu ścieżki grafiki i transfer `FingerPaintPolyline` obiekt z `inProgressPolylines` słownik, który `completedPolylines` listy:

```csharp
public override void TouchesEnded(NSSet touches, UIEvent evt)
{
    base.TouchesEnded(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Get polyline from dictionary and remove it from dictionary
        FingerPaintPolyline polyline = inProgressPolylines[touch.Handle];
        inProgressPolylines.Remove(touch.Handle);

        // Add final point to path and save with completed polylines
        polyline.Path.AddLineToPoint(touch.LocationInView(this));
        completedPolylines.Add(polyline);
    }
    SetNeedsDisplay();
}
```

`TouchesCancelled` Zastąpienia jest obsługiwany przez po prostu porzucanie `FingerPaintPolyline` obiektu w słowniku:

```csharp
public override void TouchesCancelled(NSSet touches, UIEvent evt)
{
    base.TouchesCancelled(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        inProgressPolylines.Remove(touch.Handle);
    }
    SetNeedsDisplay();
}
```

Cała tego przetwarzania umożliwia [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) program, aby śledzić poszczególne palców i rysowanie wyników na ekranie:

[![](touch-tracking-images/image01.png "Śledzenie poszczególnych palców i rysowanie wyników na ekranie")](touch-tracking-images/image01.png#lightbox)

Teraz przedstawiono sposób śledzenia poszczególnych palców na ekranie i odróżnić je.



## <a name="related-links"></a>Linki pokrewne

- [Odpowiednik przewodnik dla systemu Xamarin Android](~/android/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (przykład)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
