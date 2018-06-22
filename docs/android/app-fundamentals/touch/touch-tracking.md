---
title: Śledzenie na platformie Xamarin.Android palca wielodotyku
description: W tym temacie przedstawiono sposób śledzenia zdarzeń touch z wielu palcami
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 97e848e74a4513c55c0bf50258621c010b347fcd
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436609"
---
# <a name="multi-touch-finger-tracking"></a>Obsługa wielodotyku palca śledzenia

_W tym temacie przedstawiono sposób śledzenia zdarzeń touch z wielu palcami_

Brak razy, gdy aplikacja wielodotyku musi śledzić poszczególne palców, jak przenieść jednocześnie na ekranie. Jeden Typowa aplikacja jest programem finger-paint. Chcesz, aby umożliwić użytkownikom rysowanie za pomocą jednej linii papilarnych, ale także Rysowanie za pomocą wielu palców jednocześnie. Jak program przetwarza wiele zdarzeń touch, należy go rozróżnić zdarzenia, które odpowiadają każdej linii papilarnych. Android dostarcza kod Identyfikatora w tym celu, ale mogą być nieco trudnych uzyskiwania i obsługa tego kodu.

Dla wszystkich zdarzeń skojarzonych z określonym palca kod Identyfikatora jest taka sama. Kod ID jest przypisywany podczas palcem najpierw krawędzi ekranu, a staje się nieprawidłowy, po palca wind na ekranie.
Te kody identyfikatorów są zazwyczaj bardzo małej liczby całkowite i Android wykorzystuje je ponownie nowszych zdarzeń touch.

Program, który śledzi palców poszczególnych obsługuje prawie zawsze słownik na potrzeby śledzenia touch. Klucz słownika jest kod identyfikator, który identyfikuje określonej linii papilarnych. Wartość słownika zależy od aplikacji. W [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) program, każde pociągnięcie palca (od touch zwolnienia) jest skojarzony z obiekt, który zawiera wszystkie informacje niezbędne do renderowania wiersza z tym palca. Program definiuje małą `FingerPaintPolyline` klasy w tym celu:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new Path();
    }

    public Color Color { set; get; }

    public float StrokeWidth { set; get; }

    public Path Path { private set; get; }
}
```

Każdej linii łamanej ma kolor, szerokość pociągnięć i Android grafiki [ `Path` ](https://developer.xamarin.com/api/type/Android.Graphics.Path/) obiektu gromadzenia i renderowania wiele punktów wiersza, ponieważ jest rysowana.

W pozostałej części kodu pokazano poniżej znajduje się w `View` zależnych o nazwie `FingerPaintCanvasView`. Czy klasy utrzymują słownik obiektów typu `FingerPaintPolyline` w czasie są one aktywnie rysowane co najmniej jeden palców:

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

Ten słownik umożliwia widok, aby szybko uzyskać `FingerPaintPolyline` informacje skojarzone z określonym palca.

`FingerPaintCanvasView` Zachowuje również klasy `List` obiektu linię, które zostały ukończone:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Obiekty w tym `List` znajdują się w tej samej kolejności, że zostały one wstawiany.

`FingerPaintCanvasView` zastępuje dwie metody zdefiniowane przez `View`: [ `OnDraw` ](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/p/Android.Graphics.Canvas/) i [ `OnTouchEvent` ](https://developer.xamarin.com/api/member/Android.Views.View.OnTouchEvent/p/Android.Views.MotionEvent/).
W jego `OnDraw` override, widok rysuje linię ukończone, a następnie rysuje linię w toku.

Zastąpienie z `OnTouchEvent` metoda rozpoczyna, uzyskując `pointerIndex` wartość z `ActionIndex` właściwości. To `ActionIndex` wartość rozróżnia między wieloma palców, ale nie jest zgodne przez wiele zdarzeń. Z tego powodu, użyj `pointerIndex` uzyskać wskaźnik `id` wartość z `GetPointerId` metody. Ten identyfikator *jest* spójność w ramach wielu zdarzeń:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        // ...
    }

    // Invalidate to update the view
    Invalidate();

    // Request continued touch input
    return true;
}
```

Należy zauważyć, że używa zastąpienie `ActionMasked` właściwości w `switch` instrukcji zamiast `Action` właściwości. Poniżej przedstawiono przyczyny:

W przypadku zajmujące wielodotyku, `Action` właściwość ma wartość `MotionEventsAction.Down` dla palca pierwszego dostępu do ekranu, a następnie wartości `Pointer2Down` i `Pointer3Down` jako drugi i trzeci palcami również ekran dotykowy. Ponieważ palców czwartym i piątym kontaktu, `Action` właściwość ma wartości liczbowe, które jeszcze nie odnoszą się do elementów członkowskich `MotionEventsAction` wyliczenie! Należy sprawdzić wartości flagi bitów dla wartości do interpretowania ich znaczenie.

Podobnie jak palcami pozostaw ekranu, skontaktuj się z `Action` właściwość ma wartości `Pointer2Up` i `Pointer3Up` dla drugiego i trzeciego palcami i `Up` dla pierwszej palca.

`ActionMasked` Właściwość przyjmuje mniejszą liczbę wartości, ponieważ jest on przeznaczony do użycia w połączeniu z `ActionIndex` właściwości w celu rozróżnienia wielu palców. Gdy ekran dotykowy palców, właściwość może być tylko równa `MotionEventActions.Down` dla palca pierwszy i `PointerDown` dla kolejnych palców. Jak palcami zamknięciu ekranu, `ActionMasked` ma wartości `Pointer1Up` dla kolejnych palców i `Up` dla pierwszej palca.

Korzystając z `ActionMasked`, `ActionIndex` odróżnia kolejnych palcami touch i pozostaw ekranu, ale zwykle nie trzeba używać tej wartości, z wyjątkiem jako argument do innych metod w `MotionEvent` obiektu. Dla wielodotyku, jednym z najważniejszych z tych metod jest `GetPointerId` o nazwie w powyższym kodzie. Czy metoda zwraca wartość służy do klucza słownika do skojarzenia określonego zdarzenia palców.

`OnTouchEvent` Zastąpienia w [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) procesów programów `MotionEventActions.Down` i `PointerDown` zdarzenia identycznie tworząc nową `FingerPaintPolyline` obiektu i dodanie go do słownika:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:

            // Create a Polyline, set the initial point, and store it
            FingerPaintPolyline polyline = new FingerPaintPolyline
            {
                Color = StrokeColor,
                StrokeWidth = StrokeWidth
            };

            polyline.Path.MoveTo(args.GetX(pointerIndex),
                                 args.GetY(pointerIndex));

            inProgressPolylines.Add(id, polyline);
            break;
        // ...
    }
    // ...        
}
```

Zwróć uwagę, że `pointerIndex` służy również do uzyskania pozycja palca w widoku. Wszystkie informacje touch jest skojarzony z `pointerIndex` wartości. `id` Unikatowo identyfikuje palców między wiele komunikatów, dzięki czemu jego użyty do utworzenia wpisu słownika.

Podobnie `OnTouchEvent` również zastąpić dojść `MotionEventActions.Up` i `Pointer1Up` tak samo, przekazując ukończone linię łamaną na `completedPolylines` kolekcji, mogą być wystawiane podczas `OnDraw` zastąpienia. Kod spowoduje również usunięcie `id` wpis ze słownika:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Up:
        case MotionEventActions.Pointer1Up:

            inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                args.GetY(pointerIndex));

            // Transfer the in-progress polyline to a completed polyline
            completedPolylines.Add(inProgressPolylines[id]);
            inProgressPolylines.Remove(id);
            break;

        case MotionEventActions.Cancel:
            inProgressPolylines.Remove(id);
            break;
    }
    // ...        
}
```

Teraz dla trudnych części.

Między dół i zdarzeń, zazwyczaj istnieje wiele `MotionEventActions.Move` zdarzenia. Są one połączone w jednym wywołaniu `OnTouchEvent`, i musi się różni od `Down` i `Up` zdarzenia. `pointerIndex` Wartość uzyskanymi wcześniej z `ActionIndex` właściwości muszą być ignorowane. Zamiast tego metodę musi uzyskać wielu `pointerIndex` wartości pętli między 0 i `PointerCount` właściwości, a następnie uzyskać `id` dla każdego z tych `pointerIndex` wartości:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Move:

            // Multiple Move events are bundled, so handle them differently
            for (pointerIndex = 0; pointerIndex < args.PointerCount; pointerIndex++)
            {
                id = args.GetPointerId(pointerIndex);

                inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                    args.GetY(pointerIndex));
            }
            break;
        // ...
    }
    // ...        
}
```

Ten typ przetwarzania umożliwia [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) program, aby śledzić poszczególne palców i rysowanie wyników na ekranie:

[![Zrzut ekranu z przykładu FingerPaint](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

Teraz przedstawiono sposób śledzenia poszczególnych palców na ekranie i odróżnić je.


## <a name="related-links"></a>Linki pokrewne

- [Przewodnik dotyczący równoważne Xamarin iOS](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
