---
title: Wywoływanie zdarzeń z efekty
description: Efekt można zdefiniować i wywołaj zdarzenie sygnalizowania zmiany w podstawowym widoku macierzystego. W tym artykule przedstawiono, jak zaimplementować niskiego poziomu palca wielodotyku śledzenia i sposób generowania zdarzeń, które sygnalizują touch działania.
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: e363cae4dd72a25e4768395410d4e56a8db30eba
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
ms.locfileid: "34050159"
---
# <a name="invoking-events-from-effects"></a>Wywoływanie zdarzeń z efekty

_Efekt można zdefiniować i wywołaj zdarzenie sygnalizowania zmiany w podstawowym widoku macierzystego. W tym artykule przedstawiono, jak zaimplementować niskiego poziomu palca wielodotyku śledzenia i sposób generowania zdarzeń, które sygnalizują touch działania._

Efekt opisane w tym artykule zapewnia dostęp do zdarzeń touch niskiego poziomu. Te zdarzenia niskiego poziomu nie są dostępne za pośrednictwem istniejące `GestureRecognizer` klasy, ale są istotne dla niektórych typów aplikacji. Na przykład finger-paint aplikacji należy śledzić poszczególne palców, jak przenieść ekranu. Klawiatura utworów muzycznych wymaga wykryć podsłuchu oraz w poszczególnych kluczy, a także palcem gliding z jednego klucza do innego w glissando udostępnia.

Efekt jest idealny dla palca wielodotyku śledzenia, ponieważ można dołączyć do dowolnego elementu platformy Xamarin.Forms.

## <a name="platform-touch-events"></a>Platformy Touch zdarzenia

IOS, Android i platformy uniwersalnej systemu Windows obejmują interfejs API niskiego poziomu, który umożliwia aplikacjom wykryć touch działania. Tych platform, które rozróżnienia wszystkie trzy podstawowe typy touch zdarzenia:

- *Naciśnięto*, gdy palcem dotyka ekranu
- *Przenieść*, gdy przesuwa palcem dotknięcie ekranu
- *Wydane*, po wydaniu palca z ekranu

W środowisku wielodotyku palcami wiele poprawić ekranu w tym samym czasie. Różne platformy to numer identyfikacyjny (ID), którego aplikacje mogą korzystać w celu rozróżnienia wielu palców.

W systemie iOS `UIView` klasa definiuje trzy metody możliwym do zastąpienia, `TouchesBegan`, `TouchesMoved`, i `TouchesEnded` odpowiadający te trzy podstawowe zdarzenia. Artykuł [śledzenia palca wielodotyku](~/ios/app-fundamentals/touch/touch-tracking.md) opisano sposób użycia tych metod. Jednak program iOS musi zastąpić klasą pochodzącą z `UIView` do użycia tych metod. IOS `UIGestureRecognizer` definiuje także te same trzy metody jak dołączyć wystąpienia klasy, która jest pochodną `UIGestureRecognizer` do dowolnego `UIView` obiektu.

W systemie Android `View` klasa definiuje metodę możliwym do zastąpienia o nazwie `OnTouchEvent` przetworzyć wszystkie działania touch. Typ działania touch jest określany przez elementy członkowskie wyliczenia `Down`, `PointerDown`, `Move`, `Up`, i `PointerUp` zgodnie z opisem w artykule [śledzenia palca wielodotyku](~/android/app-fundamentals/touch/touch-tracking.md). Android `View` również definiuje zdarzenia o nazwie `Touch` , która umożliwia program obsługi zdarzeń jest dołączony do żadnej `View` obiektu.

W systemie Windows platformy Uniwersalnej, `UIElement` klasa definiuje zdarzenia o nazwie `PointerPressed`, `PointerMoved`, i `PointerReleased`. Te ustawienia zostały opisane w artykule [obsługi danych wejściowych wskaźnika artykuł w witrynie MSDN](/windows/uwp/input-and-devices/handle-pointer-input/) oraz dokumentację interfejsu API dla [ `UIElement` ](/uwp/api/windows.ui.xaml.uielement/) klasy.

`Pointer` Interfejs API platformy uniwersalnej systemu Windows jest przeznaczony do ujednolicenie piórem myszą i dotykiem. Z tego powodu `PointerMoved` zdarzenie jest wywoływane, gdy wskaźnik myszy porusza się na element nawet wtedy, gdy przycisk myszy nie jest wciśnięty. `PointerRoutedEventArgs` Obiekt, który towarzyszy te zdarzenia ma właściwości o nazwie `Pointer` mający właściwość o nazwie `IsInContact` co oznacza, jeśli zostanie naciśnięty przycisk myszy lub palcem kontaktu z ekranu.

Ponadto platformy uniwersalnej systemu Windows definiuje dwie więcej zdarzeń o nazwie `PointerEntered` i `PointerExited`. Oznaczają one, gdy myszy lub palca przenoszone z jednego elementu na inny. Rozważmy na przykład dwóch sąsiadujących ze sobą elementów o nazwie A i B. Oba elementy zainstalowane programy obsługi zdarzeń wskaźnika. Naciśnięcie palcem na A, `PointerPressed` zdarzenie jest wywoływane. Podczas przenoszenia palca, A wywołuje `PointerMoved` zdarzenia. Jeśli palca zostanie przeniesiony z zakresu od A do B, A wywołuje `PointerExited` wywołuje zdarzenie i B `PointerEntered` zdarzeń. Jeśli następnie wydaniu palca B wywołuje `PointerReleased` zdarzeń.

IOS i Android platform różnią się od platformy uniwersalnej systemu Windows: widok, który najpierw pobiera wywołanie `TouchesBegan` lub `OnTouchEvent` podczas dotyka palcem widoku w dalszym ciągu otrzymywać wszystkie touch działania nawet wtedy, gdy w przypadku palca przenosi do innych widoków. Platformy uniwersalnej systemu Windows można zachowują się podobnie, jeśli aplikacja przechwytuje wskaźnika: W `PointerEntered` obsługi zdarzeń, wywołania elementu `CapturePointer` , a następnie pobiera wszystkie działania touch z tym palca.

Podejście platformy uniwersalnej systemu Windows okaże się, że bardzo przydatne w przypadku niektórych typów aplikacji, na przykład klawiatura utworów muzycznych. Każdy klucz można obsługiwać zdarzenia touch dla tego klucza i Wykryj, kiedy palcem ma przesuwać z jednego klucza przy użyciu `PointerEntered` i `PointerExited` zdarzenia.

Z tego powodu efekt śledzenia touch opisane w tym artykule implementuje metody platformy uniwersalnej systemu Windows.

## <a name="the-touch-tracking-effect-api"></a>Interfejs API efekt Touch śledzenia

[ **Touch śledzenia pokazy efekt** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) próbka zawiera klasy (i wyliczenie), które implementują niskiego poziomu śledzenia touch. Tego typu należy do przestrzeni nazw `TouchTracking` i rozpoczynać się od słowa `Touch`. **TouchTrackingEffectDemos** obejmuje .NET Standard projektu biblioteki `TouchActionType` wyliczenie dla typu touch zdarzeń:

```csharp
public enum TouchActionType
{
    Entered,
    Pressed,
    Moved,
    Released,
    Exited,
    Cancelled
}
```

Wszystkie platformy także zdarzenia wskazującego zdarzeń touch została anulowana.

`TouchEffect` Pochodną klasy w bibliotece programu .NET Standard `RoutingEffect` i definiuje zdarzenia o nazwie `TouchAction` i metodę o nazwie `OnTouchAction` który wywołuje `TouchAction` zdarzeń:

```csharp
public class TouchEffect : RoutingEffect
{
    public event TouchActionEventHandler TouchAction;

    public TouchEffect() : base("XamarinDocs.TouchEffect")
    {
    }

    public bool Capture { set; get; }

    public void OnTouchAction(Element element, TouchActionEventArgs args)
    {
        TouchAction?.Invoke(element, args);
    }
}
```

Ponadto `Capture` właściwości. Do przechwytywania zdarzeń touch, aplikacja musi ustawić tę właściwość na `true` przed `Pressed` zdarzeń. W przeciwnym razie zdarzenia touch zachowują się podobnie jak w przypadku platformy uniwersalnej systemu Windows.

`TouchActionEventArgs` Klasy w bibliotece programu .NET Standard zawiera wszystkie informacje, który towarzyszy każdego zdarzenia:

```csharp
public class TouchActionEventArgs : EventArgs
{
    public TouchActionEventArgs(long id, TouchActionType type, Point location, bool isInContact)
    {
        Id = id;
        Type = type;
        Location = location;
        IsInContact = isInContact;
    }

    public long Id { private set; get; }

    public TouchActionType Type { private set; get; }

    public Point Location { private set; get; }

    public bool IsInContact { private set; get; }
}
```

Aplikacja może użyć `Id` właściwości śledzenia poszczególnych palców. Powiadomienie `IsInContact` właściwości. Ta właściwość jest zawsze `true` dla `Pressed` zdarzeń i `false` dla `Released` zdarzenia. Warto również `true` dla `Moved` zdarzeń w systemach iOS i Android. `IsInContact` Właściwość może być `false` dla `Moved` zdarzeń na uniwersalnych platformy systemu Windows, gdy program jest uruchomiony na pulpicie, a wskaźnik myszy jest przesuwany bez przycisk naciśnięty.

Można użyć `TouchEffect` klasy w aplikacjach przez dołączenie pliku do projektu biblioteki .NET Standard rozwiązania i przez dodanie wystąpienia `Effects` kolekcji dowolny element platformy Xamarin.Forms. Dołącz do obsługi `TouchAction` zdarzenia w celu uzyskania zdarzeń touch.

Aby użyć `TouchEffect` w swojej aplikacji, musisz również implementacji platformy objęte **TouchTrackingEffectDemos** rozwiązania.

## <a name="the-touch-tracking-effect-implementations"></a>Implementacje efekt Touch śledzenia

IOS, Android i platformy uniwersalnej systemu Windows implementacje `TouchEffect` są opisane poniżej począwszy od implementacji najprostszym (UWP) i kończąc implementacji systemu iOS, ponieważ jest strukturalnie bardziej złożone od innych.

### <a name="the-uwp-implementation"></a>Implementacja platformy uniwersalnej systemu Windows

Implementacja platformy uniwersalnej systemu Windows `TouchEffect` jest najprostsza. W zwykły sposób, klasę pochodną `PlatformEffect` i zawiera dwa atrybuty zestawu:

```csharp
[assembly: ResolutionGroupName("XamarinDocs")]
[assembly: ExportEffect(typeof(TouchTracking.UWP.TouchEffect), "TouchEffect")]

namespace TouchTracking.UWP
{
    public class TouchEffect : PlatformEffect
    {
        ...
    }
}
```

`OnAttached` Zastąpienie zapisuje informacje jako programy obsługi pól i dołącza do wszystkich zdarzeń wskaźnika:

```csharp
public class TouchEffect : PlatformEffect
{
    FrameworkElement frameworkElement;
    TouchTracking.TouchEffect effect;
    Action<Element, TouchActionEventArgs> onTouchAction;

    protected override void OnAttached()
    {
        // Get the Windows FrameworkElement corresponding to the Element that the effect is attached to
        frameworkElement = Control == null ? Container : Control;

        // Get access to the TouchEffect class in the .NET Standard library
        effect = (TouchTracking.TouchEffect)Element.Effects.
                    FirstOrDefault(e => e is TouchTracking.TouchEffect);

        if (effect != null && frameworkElement != null)
        {
            // Save the method to call on touch events
            onTouchAction = effect.OnTouchAction;

            // Set event handlers on FrameworkElement
            frameworkElement.PointerEntered += OnPointerEntered;
            frameworkElement.PointerPressed += OnPointerPressed;
            frameworkElement.PointerMoved += OnPointerMoved;
            frameworkElement.PointerReleased += OnPointerReleased;
            frameworkElement.PointerExited += OnPointerExited;
            frameworkElement.PointerCanceled += OnPointerCancelled;
        }
    }
    ...
}    
```

`OnPointerPressed` Obsługi wywołuje zdarzenie efekt, wywołując `onTouchAction` w `CommonHandler` metody:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerPressed(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Pressed, args);

        // Check setting of Capture property
        if (effect.Capture)
        {
            (sender as FrameworkElement).CapturePointer(args.Pointer);
        }
    }
    ...
    void CommonHandler(object sender, TouchActionType touchActionType, PointerRoutedEventArgs args)
    {
        PointerPoint pointerPoint = args.GetCurrentPoint(sender as UIElement);
        Windows.Foundation.Point windowsPoint = pointerPoint.Position;  

        onTouchAction(Element, new TouchActionEventArgs(args.Pointer.PointerId,
                                                        touchActionType,
                                                        new Point(windowsPoint.X, windowsPoint.Y),
                                                        args.Pointer.IsInContact));
    }
}
```

`OnPointerPressed` sprawdza również wartość `Capture` właściwości klasy obowiązywać w bibliotece .NET Standard i wywołania `CapturePointer` przypadku `true`.

 Innych programów obsługi zdarzeń platformy uniwersalnej systemu Windows są nawet prostsze:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerEntered(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Entered, args);
    }
    ...
}
```

### <a name="the-android-implementation"></a>Implementacja systemu Android

Implementacje Android i iOS są zawsze bardziej złożonych, ponieważ ich musi implementować `Exited` i `Entered` zdarzenia, gdy palcem są przenoszone z jednego elementu na inny. Zarówno implementacje mają strukturę podobną.

Android `TouchEffect` klasy instaluje program obsługi `Touch` zdarzeń:

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

Klasa określa również dwa statycznego słowniki:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    static Dictionary<Android.Views.View, TouchEffect> viewDictionary =
        new Dictionary<Android.Views.View, TouchEffect>();

    static Dictionary<int, TouchEffect> idToEffectDictionary =
        new Dictionary<int, TouchEffect>();
    ...
```

`viewDictionary` Pobiera nowy wpis zawsze `OnAttached` nazywa się zastąpienia:

```csharp
viewDictionary.Add(view, this);
```

Wpis zostanie usunięty ze słownika w `OnDetached`. Każde wystąpienie `TouchEffect` skojarzonego z konkretnym widoku, który efekt jest dołączony do. W słowniku statycznym umożliwia żadnego `TouchEffect` wystąpienia wyliczyć za pośrednictwem innych widoków i odpowiednie `TouchEffect` wystąpień. Jest to konieczne umożliwić przesyłania zdarzeń z jednego widoku.

Android przypisuje kod identyfikator dostępu do zdarzeń, który umożliwia aplikacjom śledzić poszczególne palców. `idToEffectDictionary` Kojarzy ten kod identyfikator z `TouchEffect` wystąpienia. Element zostanie dodany do tego słownika podczas `Touch` obsługi jest wywoływana dla naciśnij palca:

```csharp
void OnTouch(object sender, Android.Views.View.TouchEventArgs args)
{
    ...
    switch (args.Event.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:
            FireEvent(this, id, TouchActionType.Pressed, screenPointerCoords, true);

            idToEffectDictionary.Add(id, this);

            capture = libTouchEffect.Capture;
            break;

```

Element zostanie usunięty z `idToEffectDictionary` po wydaniu palca z ekranu. `FireEvent` Metoda po prostu gromadzi wszystkie informacje niezbędne do wywołania `OnTouchAction` metody:

```csharp
void FireEvent(TouchEffect touchEffect, int id, TouchActionType actionType, Point pointerLocation, bool isInContact)
{
    // Get the method to call for firing events
    Action<Element, TouchActionEventArgs> onTouchAction = touchEffect.libTouchEffect.OnTouchAction;

    // Get the location of the pointer within the view
    touchEffect.view.GetLocationOnScreen(twoIntArray);
    double x = pointerLocation.X - twoIntArray[0];
    double y = pointerLocation.Y - twoIntArray[1];
    Point point = new Point(fromPixels(x), fromPixels(y));

    // Call the method
    onTouchAction(touchEffect.formsElement,
        new TouchActionEventArgs(id, actionType, point, isInContact));
}
```

Wszystkie inne typy touch są przetwarzane na dwa sposoby: Jeśli `Capture` właściwość jest `true`, zdarzenie touch jest dość proste tłumaczenia na język `TouchEffect` informacji. Pobiera bardziej skomplikowane kiedy `Capture` jest `false` ponieważ touch zdarzeń może być konieczne można przenosić z jednego widoku do innego. To jest odpowiedzialny za `CheckForBoundaryHop` metodę, która jest wywoływana podczas przenoszenia zdarzeń. Ta metoda korzysta z obu statycznych słowników. Wylicza go `viewDictionary` ustalenie widok, który obecnie zachodzi palca i używa `idToEffectDictionary` do przechowywania bieżącego `TouchEffect` wystąpienia (i w związku z tym bieżący widok) skojarzonych z określonym Identyfikatorem:

```csharp
void CheckForBoundaryHop(int id, Point pointerLocation)
{
    TouchEffect touchEffectHit = null;

    foreach (Android.Views.View view in viewDictionary.Keys)
    {
        // Get the view rectangle
        try
        {
            view.GetLocationOnScreen(twoIntArray);
        }
        catch // System.ObjectDisposedException: Cannot access a disposed object.
        {
            continue;
        }
        Rectangle viewRect = new Rectangle(twoIntArray[0], twoIntArray[1], view.Width, view.Height);

        if (viewRect.Contains(pointerLocation))
        {
            touchEffectHit = viewDictionary[view];
        }
    }

    if (touchEffectHit != idToEffectDictionary[id])
    {
        if (idToEffectDictionary[id] != null)
        {
            FireEvent(idToEffectDictionary[id], id, TouchActionType.Exited, pointerLocation, true);
        }
        if (touchEffectHit != null)
        {
            FireEvent(touchEffectHit, id, TouchActionType.Entered, pointerLocation, true);
        }
        idToEffectDictionary[id] = touchEffectHit;
    }
}
```

Jeśli wprowadzono zmiany w `idToEffectionDictionary`, potencjalnie wywołuje metodę `FireEvent` dla `Exited` i `Entered` przekazaniem z jednego widoku. Jednak palca mógł zostać przeniesiony do widoku bez dołączonego obszar `TouchEffect`, lub w obszarze widoku z mocą dołączony.

Powiadomienie `try` i `catch` zablokować podczas uzyskiwania dostępu do widoku. Na stronie, która jest przejście do tej, a następnie wraca do strony głównej `OnDetached` nie wywołano metody i elementy pozostają w `viewDictionary` , ale Android uzna je usunąć.

### <a name="the-ios-implementation"></a>Implementacja systemu iOS

Implementacja systemu iOS jest podobny do implementacji systemu Android, z wyjątkiem systemu iOS `TouchEffect` klasy musi utworzyć wystąpienia pochodną elementu `UIGestureRecognizer`. To jest klasa w projekcie systemu iOS o nazwie `TouchRecognizer`. Ta klasa obsługuje dwa statycznego słowników, przechowujących `TouchRecognizer` wystąpień:

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

Wiele struktury to `TouchRecognizer` klasy jest podobna do Android `TouchEffect` klasy.

## <a name="putting-the-touch-effect-to-work"></a>Wprowadzenie do pracy efekt Touch

[ **TouchTrackingEffectDemos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) program zawiera pięć stron, które testowanie skutku touch śledzenia dla typowych zadań.

**Przeciąganie BoxView** strony umożliwia dodanie `BoxView` elementy `AbsoluteLayout` i przeciągnij je po ekranie. [Pliku XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml) tworzy dwa `Button` widoków dodawania `BoxView` elementy `AbsoluteLayout` i wyczyszczenie `AbsoluteLayout`.

Metoda [pliku CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml.cs) dodaje nowy `BoxView` do `AbsoluteLayout` dodaje również `TouchEffect` do obiektu `BoxView` i dołącza program obsługi zdarzeń w celu:

```csharp
void AddBoxViewToLayout()
{
    BoxView boxView = new BoxView
    {
        WidthRequest = 100,
        HeightRequest = 100,
        Color = new Color(random.NextDouble(),
                          random.NextDouble(),
                          random.NextDouble())
    };

    TouchEffect touchEffect = new TouchEffect();
    touchEffect.TouchAction += OnTouchEffectAction;
    boxView.Effects.Add(touchEffect);
    absoluteLayout.Children.Add(boxView);
}
```

`TouchAction` Obsługi zdarzenia przetwarza wszystkie zdarzenia touch dla wszystkich `BoxView` elementów, ale trzeba niektórych należy zachować ostrożność: dwoma palcami nie zezwala na jednym `BoxView` ponieważ program tylko implementuje przeciąganie i czy dwoma palcami koliduje ze sobą. Z tego powodu stronie definiuje osadzonych klasy dla każdej linii papilarnych aktualnie śledzone:

```csharp
class DragInfo
{
    public DragInfo(long id, Point pressPoint)
    {
        Id = id;
        PressPoint = pressPoint;
    }

    public long Id { private set; get; }

    public Point PressPoint { private set; get; }
}

Dictionary<BoxView, DragInfo> dragDictionary = new Dictionary<BoxView, DragInfo>();
```

`dragDictionary` Zawiera wpis dla każdego `BoxView` aktualnie przeciągany.

`Pressed` Akcji touch dodaje element do tego słownika i `Released` akcji spowoduje usunięcie jej. `Pressed` Logiki musi sprawdzić, czy istnieje już element w słowniku, w tym `BoxView`. Jeśli tak, `BoxView` jest już przeciągnąć i nowe zdarzenie jest drugi palcem na tym sam `BoxView`. Dla `Moved` i `Released` akcje, obsługi zdarzeń musi Sprawdź, czy słownik zawiera wpis dla tej `BoxView` oraz że touch `Id` właściwości w tym przeciągnąć `BoxView` jest zgodna ze strukturą we wpisie słownika:

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    BoxView boxView = sender as BoxView;

    switch (args.Type)
    {
        case TouchActionType.Pressed:
            // Don't allow a second touch on an already touched BoxView
            if (!dragDictionary.ContainsKey(boxView))
            {
                dragDictionary.Add(boxView, new DragInfo(args.Id, args.Location));

                // Set Capture property to true
                TouchEffect touchEffect = (TouchEffect)boxView.Effects.FirstOrDefault(e => e is TouchEffect);
                touchEffect.Capture = true;
            }
            break;

        case TouchActionType.Moved:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                Rectangle rect = AbsoluteLayout.GetLayoutBounds(boxView);
                Point initialLocation = dragDictionary[boxView].PressPoint;
                rect.X += args.Location.X - initialLocation.X;
                rect.Y += args.Location.Y - initialLocation.Y;
                AbsoluteLayout.SetLayoutBounds(boxView, rect);
            }
            break;

        case TouchActionType.Released:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                dragDictionary.Remove(boxView);
            }
            break;
    }
}
```

`Pressed` Ustawia logikę `Capture` właściwość `TouchEffect` do obiektu `true`. To ustawienie działa dostarczania wszystkich kolejnych zdarzeń dla tej linii papilarnych do tej procedury obsługi zdarzeń.

`Moved` Przenosi logiki `BoxView` , zmieniając `LayoutBounds` dołączona właściwość. `Location` Właściwości argumentów zdarzenia jest zawsze względem `BoxView` przeciąganie i w razie `BoxView` przeciągania przy stałym, `Location` właściwości kolejnych zdarzeń zostaną około takie same. Na przykład, jeśli naciśnie palcem `BoxView` w środku, `Pressed` magazynów akcji `PressPoint` właściwości (50, 50), która jest taka sama dla kolejnych zdarzeń. Jeśli `BoxView` zostanie przeciągnięty przekątnej przy stałym, kolejne `Location` właściwości podczas `Moved` akcji mogą być wartościami (55, 55), w którym to przypadku `Moved` logiki dodaje 5 do poziome i pionowe położenie `BoxView`. Spowoduje to przeniesienie `BoxView` tak, aby jego Centrum jest ponownie bezpośrednio pod palca.

Można przenieść wielu `BoxView` elementów jednocześnie za pomocą różnych palców.

[![](touch-tracking-images/boxviewdragging-small.png "Potrójna zrzut ekranu przedstawiający stronę przeciąganie BoxView")](touch-tracking-images/boxviewdragging-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę BoxView przeciągania")

### <a name="subclassing-the-view"></a>Tworzenie podklas widoku

Często jest łatwiejsze dla elementu platformy Xamarin.Forms do obsługi własnych zdarzeń touch. **Możliwością przeciągania przeciąganie BoxView** strony działa tak samo, jak **przeciąganie BoxView** strony, ale elementy, które drags użytkownika są wystąpieniami [ `DraggableBoxView` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/DraggableBoxView.cs) Klasa, która jest pochodną `BoxView`:

```csharp
class DraggableBoxView : BoxView
{
    bool isBeingDragged;
    long touchId;
    Point pressPoint;

    public DraggableBoxView()
    {
        TouchEffect touchEffect = new TouchEffect
        {
            Capture = true
        };
        touchEffect.TouchAction += OnTouchEffectAction;
        Effects.Add(touchEffect);
    }

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!isBeingDragged)
                {
                    isBeingDragged = true;
                    touchId = args.Id;
                    pressPoint = args.Location;
                }
                break;

            case TouchActionType.Moved:
                if (isBeingDragged && touchId == args.Id)
                {
                    TranslationX += args.Location.X - pressPoint.X;
                    TranslationY += args.Location.Y - pressPoint.Y;
                }
                break;

            case TouchActionType.Released:
                if (isBeingDragged && touchId == args.Id)
                {
                    isBeingDragged = false;
                }
                break;
        }
    }
}
```

Konstruktor tworzy i dołącza `TouchEffect`i ustawia `Capture` właściwości, gdy jest pierwsze wystąpienie tego obiektu. Słownik nie jest wymagana, ponieważ ta sama klasa przechowuje `isBeingDragged`, `pressPoint`, i `touchId` wartości skojarzone z każdym palca. `Moved` Zmienia Obsługa `TranslationX` i `TranslationY` właściwości, więc logiki będzie działać nawet wtedy, gdy element nadrzędny `DraggableBoxView` nie jest `AbsoluteLayout`.

### <a name="integrating-with-skiasharp"></a>Integrowanie z SkiaSharp

Następne dwa pokazów wymagają grafiki i używają SkiaSharp w tym celu. Warto poznać [przy użyciu SkiaSharp w platformy Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) przed badanie tych przykładów. Pierwsze dwa artykuły ("SkiaSharp rysunku podstawy" i "SkiaSharp wierszy i ścieżki") obejmuje wszystko, co należy tutaj.

**Rysowania elipsy** strony można narysować elipsę przesuwając palca na ekranie. W zależności od tego, jak przenieść palca, można narysowana Elipsa z lewego górnego do prawej dolnej lub innych narożnik przeciwną rogu. Elipsy jest rysowany z losowe koloru i przezroczystość.

[![](touch-tracking-images/ellipsedrawing-small.png "Potrójna zrzut ekranu przedstawiający stronę rysowania elipsy")](touch-tracking-images/ellipsedrawing-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę rysowania elipsy")

Jeśli następnie touch jedną wielokropek, przeciągnij go do innej lokalizacji. Wymaga to technika, nazywany "trafień testów," który obejmuje wyszukiwania dla obiektu graficznego w określonym punkcie. Wielokropek SkiaSharp nie są elementami platformy Xamarin.Forms, nie mogą wykonywać własne `TouchEffect` przetwarzania. `TouchEffect` Muszą być stosowane do całej `SKCanvasView` obiektu.

[EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml) tworzy plik `SKCanvasView` w komórce jednym `Grid`. `TouchEffect` Obiekt jest dołączony do tego `Grid`:

```xaml
<Grid x:Name="canvasViewGrid"
        Grid.Row="1"
        BackgroundColor="White">

    <skia:SKCanvasView x:Name="canvasView"
                        PaintSurface="OnCanvasViewPaintSurface" />
    <Grid.Effects>
        <tt:TouchEffect Capture="True"
                        TouchAction="OnTouchEffectAction" />
    </Grid.Effects>
</Grid>
```

Android i platformy uniwersalnej systemu Windows `TouchEffect` może zostać dołączony bezpośrednio do `SKCanvasView`, ale w systemie iOS, która nie działa. Zwróć uwagę, że `Capture` właściwość jest ustawiona na `true`.

Każdy elipsy, który renderuje SkiaSharp jest reprezentowana przez obiekt typu `EllipseDrawingFigure`:

```csharp
class EllipseDrawingFigure
{
    SKPoint pt1, pt2;

    public EllipseDrawingFigure()
    {
    }

    public SKColor Color { set; get; }

    public SKPoint StartPoint
    {
        set
        {
            pt1 = value;
            MakeRectangle();
        }
    }

    public SKPoint EndPoint
    {
        set
        {
            pt2 = value;
            MakeRectangle();
        }
    }

    void MakeRectangle()
    {
        Rectangle = new SKRect(pt1.X, pt1.Y, pt2.X, pt2.Y).Standardized;
    }

    public SKRect Rectangle { set; get; }

    // For dragging operations
    public Point LastFingerLocation { set; get; }

    // For the dragging hit-test
    public bool IsInEllipse(SKPoint pt)
    {
        SKRect rect = Rectangle;

        return (Math.Pow(pt.X - rect.MidX, 2) / Math.Pow(rect.Width / 2, 2) +
                Math.Pow(pt.Y - rect.MidY, 2) / Math.Pow(rect.Height / 2, 2)) < 1;
    }
}
```

`StartPoint` i `EndPoint` właściwości są używane, gdy program przetwarza dotykowym; `Rectangle` właściwość jest używana do rysowania elipsy. `LastFingerLocation` Właściwości wejścia play podczas przeciągania elipsy i `IsInEllipse` metody pomocy do testowania trafień. Metoda zwraca `true` czy punkt znajduje się wewnątrz elipsy.

[Pliku CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml.cs) przechowuje trzy kolekcje:

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

`draggingFigure` Słownik zawiera podzbiór `completedFigures` kolekcji. SkiaSharp `PaintSurface` obsługi zdarzeń po prostu renderuje obiektów w tych `completedFigures` i `inProgressFigures` kolekcje:

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Fill
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (EllipseDrawingFigure figure in completedFigures)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
    foreach (EllipseDrawingFigure figure in inProgressFigures.Values)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
}
```

Trickiest część przetwarzania touch jest `Pressed` obsługi. Jest to, gdy testowanie trafień jest wykonywane, ale jeśli kod wykryje elipsy w obszarze palca użytkownika, że elipsy można tylko przeciąganych Jeśli nie jest obecnie przeciąganie jej innym palca. Jeśli nie ma żadnych elipsy w obszarze palca użytkownika, kod rozpoczyna proces rysowania elipsy nowe:

```csharp
case TouchActionType.Pressed:
    bool isDragOperation = false;

    // Loop through the completed figures
    foreach (EllipseDrawingFigure fig in completedFigures.Reverse<EllipseDrawingFigure>())
    {
        // Check if the finger is touching one of the ellipses
        if (fig.IsInEllipse(ConvertToPixel(args.Location)))
        {
            // Tentatively assume this is a dragging operation
            isDragOperation = true;

            // Loop through all the figures currently being dragged
            foreach (EllipseDrawingFigure draggedFigure in draggingFigures.Values)
            {
                // If there's a match, we'll need to dig deeper
                if (fig == draggedFigure)
                {
                    isDragOperation = false;
                    break;
                }
            }

            if (isDragOperation)
            {
                fig.LastFingerLocation = args.Location;
                draggingFigures.Add(args.Id, fig);
                break;
            }
        }
    }

    if (isDragOperation)
    {
        // Move the dragged ellipse to the end of completedFigures so it's drawn on top
        EllipseDrawingFigure fig = draggingFigures[args.Id];
        completedFigures.Remove(fig);
        completedFigures.Add(fig);
    }
    else // start making a new ellipse
    {
        // Random bytes for random color
        byte[] buffer = new byte[4];
        random.NextBytes(buffer);

        EllipseDrawingFigure figure = new EllipseDrawingFigure
        {
            Color = new SKColor(buffer[0], buffer[1], buffer[2], buffer[3]),
            StartPoint = ConvertToPixel(args.Location),
            EndPoint = ConvertToPixel(args.Location)
        };
        inProgressFigures.Add(args.Id, figure);
    }
    canvasView.InvalidateSurface();
    break;
```

W przykładzie SkiaSharp **Paint palca** strony. Można wybrać kolor obrysu i jego szerokość z dwóch `Picker` widoków, a następnie narysuj z co najmniej jeden palcami:

[![](touch-tracking-images/fingerpaint-small.png "Potrójna zrzut ekranu przedstawiający stronę Paint palca")](touch-tracking-images/fingerpaint-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Paint palca")

W tym przykładzie wymaga również osobnej klasy do reprezentowania każdego wiersza rysowane na ekranie:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new SKPath();
    }

    public SKPath Path { set; get; }

    public Color StrokeColor { set; get; }

    public float StrokeWidth { set; get; }
}
```

`SKPath` Obiekt jest używany do renderowania każdego wiersza. [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/FingerPaintPage.xaml.cs) pliku przechowuje dwie kolekcje tych obiektów, dla tych linię obecnie rysowane i drugi dla ukończonych linię:

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

`Pressed` Przetwarzania tworzy nowy `FingerPaintPolyline`, wywołania `MoveTo` na obiekt ścieżki do przechowywania punktu początkowego i dodaje je do obiektu `inProgressPolylines` słownika. `Moved` Przetwarzania wywołania `LineTo` dla obiektu ścieżka o nowe położenie linii papilarnych i `Released` przetwarzania przesyła linię łamaną ukończone z `inProgressPolylines` do `completedPolylines`. Jeszcze raz SkiaSharp rzeczywisty kod rysowania jest stosunkowo prosta:

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    StrokeCap = SKStrokeCap.Round,
    StrokeJoin = SKStrokeJoin.Round
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (FingerPaintPolyline polyline in completedPolylines)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }

    foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }
}
```

### <a name="tracking-view-to-view-touch"></a>Śledzenie Touch widoku do widoku

Ustawiono poprzednich przykładach `Capture` właściwość `TouchEffect` do `true`, albo gdy `TouchEffect` został utworzony lub gdy `Pressed` wystąpiło zdarzenie. Dzięki temu, że tego samego elementu odbiera wszystkie zdarzenia, które są skojarzone z palca, które naciskanie widoku. Próbki jest *nie* ustawić `Capture` do `true`. Powoduje to inaczej, gdy palca kontaktu z ekranu są przenoszone z jednego elementu na inny. Element, który przenosi palca z odbiera zdarzenia z `Type` ustawioną właściwość `TouchActionType.Exited` i drugi element odbiera zdarzenia z `Type` ustawienie `TouchActionType.Entered`.

Ten typ przetwarzania touch jest bardzo przydatna do klawiatury utworów muzycznych. Klucz powinno być możliwe do wykrycia, gdy naciśnięcia, ale także w przypadku palcem slajdów z jednego klucza do innego.

**Dyskretnej klawiatury** strony małe definiuje [ `WhiteKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/WhiteKey.cs) i [ `BlackKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BlackKey.cs) klasy, które pochodzą z [ `Key` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/Key.cs), która jest pochodną `BoxView`.

`Key` Klasa jest gotowa do użycia w programie rzeczywiste utworów muzycznych. Definiuje właściwości publicznej o nazwie `IsPressed` i `KeyNumber`, których ma mieć ustawioną na kod klucza ustala MIDI standard. `Key` Klasa również definiuje zdarzenia o nazwie `StatusChanged`, które jest wywoływane, gdy `IsPressed` zmiany właściwości.

Wiele palców są dozwolone dla każdego klucza. Z tego powodu `Key` obsługuje klasy `List` numerów Identyfikacyjnych touch wszystkich palców obecnie dotknięcie tego klucza:

```csharp
List<long> ids = new List<long>();
```

`TouchAction` Obsługi zdarzeń dodaje identyfikator do `ids` listy zarówno `Pressed` typ zdarzenia i `Entered` typu, ale tylko wtedy, gdy `IsInContact` właściwość jest `true` dla `Entered` zdarzeń. Identyfikator jest usuwany z `List` dla `Released` lub `Exited` zdarzeń:

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    switch (args.Type)
    {
      case TouchActionType.Pressed:
          AddToList(args.Id);
          break;

        case TouchActionType.Entered:
            if (args.IsInContact)
            {
                AddToList(args.Id);
            }
            break;

        case TouchActionType.Moved:
            break;

        case TouchActionType.Released:
        case TouchActionType.Exited:
            RemoveFromList(args.Id);
            break;
    }
}

```

`AddToList` i `RemoveFromList` metody Sprawdź, czy `List` została zmieniona między pusta i nie jest pusty, a jeśli tak jest, wywołuje `StatusChanged` zdarzeń.

Różnych `WhiteKey` i `BlackKey` ułożone elementy na stronie [pliku XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/SilentKeyboardPage.xaml), który wygląda najlepiej, gdy telefon jest przechowywany w trybie krajobraz:

[![](touch-tracking-images/silentkeyboard-small.png "Potrójna zrzut ekranu przedstawiający stronę dyskretnej klawiatury")](touch-tracking-images/silentkeyboard-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę dyskretnej klawiatury")

Jeśli między kluczy jest odchylenia palca, zostanie wyświetlony niewielkie zmiany w kolorze czy zdarzenia touch są przenoszone z jednego klucza do innego.

## <a name="summary"></a>Podsumowanie

W tym artykule wykazała wywołać zdarzenia w efekt oraz sposobu zapisu i efektu implementuje przetwarzania wielodotyku niskiego poziomu.


## <a name="related-links"></a>Linki pokrewne

- [Obsługa wielodotyku palca śledzenia w systemie iOS](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Obsługa wielodotyku palca śledzenia w systemie Android](~/android/app-fundamentals/touch/touch-tracking.md)
- [Efekt śledzenia Touch (przykład)](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)
