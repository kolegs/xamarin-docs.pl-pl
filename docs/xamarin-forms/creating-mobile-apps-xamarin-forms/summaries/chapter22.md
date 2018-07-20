---
title: Podsumowanie rozdziałów 22. Animacja
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziałów 22. Animacja'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c4d92784654db8e566b41c8270dbe2095bd28b94
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156603"
---
# <a name="summary-of-chapter-22-animation"></a>Podsumowanie rozdziałów 22. Animacja

W tym samouczku, możesz utworzyć własne animacji przy użyciu zestawu narzędzi Xamarin.Forms czasomierz lub `Task.Delay`, ale dużo łatwiej za pomocą funkcji animacji, dostarczone przez zestaw narzędzi Xamarin.Forms. Trzy klasy zaimplementować te animacji.

- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions), ogólne podejście
- [`Animation`](xref:Xamarin.Forms.Animation), bardziej wszechstronna, ale trudniejsze
- [`AnimationExtension`](xref:Xamarin.Forms.AnimationExtensions), najbardziej wszechstronny najniższy poziom podejścia

Ogólnie rzecz biorąc animacje docelowe właściwości, które są wspierane przez właściwości możliwej do wiązania. Nie jest to wymagane, ale są to jedyne właściwości, które dynamicznie reagować na zmiany.

Brak interfejsu XAML dla tych animacji, ale możesz zintegrować animacji przy użyciu techniki opisane w XAML [ **działu 23. Wyzwalacze i zachowania**](chapter23.md).

## <a name="exploring-basic-animations"></a>Eksplorowanie podstawowe animacji

Funkcje podstawowe animacji są metody rozszerzające w [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) klasy. Te metody stosowane do każdego obiektu, która pochodzi od klasy `VisualElement`. Najprostsza animacji docelowych przekształcenia właściwości omówione w [ `Chapter 21. Transforms` ](chapter21.md).

[ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) pokazuje, jak `Clicked` program obsługi zdarzeń dla `Button` może wywołać [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodę rozszerzenia, aby pokrętła przycisk koła.

`RotateTo` Zmiany metody `Rotation` właściwość `Button` z zakresu od 0 do 360 w ciągu sekundy jednej czwartej (domyślnie). Jeśli `Button` dotknięcie ponownie, ale nic nie robi ponieważ `Rotation` właściwość jest już 360 stopni.

### <a name="setting-the-animation-duration"></a>Ustawianie czasu trwania animacji

Drugi argument `RotateTo` jest czas w milisekundach. Jeśli ustawiona na wartość duże, naciskając `Button` podczas animacji, rozpoczyna się nowej animacji rozpoczynający się od bieżącego kąta.

### <a name="relative-animations"></a>Względna animacji

`RelRotateTo` Metoda wykonuje rotację względną, dodając określoną wartość do istniejącej wartości. Ta metoda umożliwia `Button` wybierać wiele razy, a każdy czasu zwiększa `Rotation` właściwość, według 360 stopni.

### <a name="awaiting-animations"></a>Oczekiwanie na animacji

Wszystkie metody animacji w `ViewExtensions` zwracają `Task<bool>` obiektów. Oznacza to, że zdefiniować szereg sekwencyjne animacji przy użyciu `ContinueWith` lub `await`. `bool` Uzupełniania zwracają wartość `false` Jeśli animacji z produktu bez przeszkód lub `true` Jeśli zostało anulowane przez [ `CancelAnimation` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) metody, co z kolei anuluje wszystkie animacje zainicjowane przez inne metody w `ViewExtensions` ustawione w tym samym elemencie.

### <a name="composite-animations"></a>Złożone animacji

Możesz mieszać oczekiwanych i najczęściej inne niż oczekiwane animacji do tworzenia złożonych animacji. Są to animacje w `ViewExtensions` przeznaczonych `TranslatonX`, `TranslationY`, i `Scale` Przekształcanie właściwości:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))

Należy zauważyć, że `TranslateTo` może mieć wpływ na obie `TranslationX` i `TranslationY` właściwości.

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll i Task.WhenAny

Istnieje również możliwość zarządzania jednoczesnych animacji przy użyciu [ `Task.WhenAll` ](xref:System.Threading.Tasks.Task.WhenAll*), co sygnalizuje, gdy wiele zadań wszystkie zawrzeć, i [ `Task.WhenAny` ](xref:System.Threading.Tasks.Task.WhenAny*), co sygnalizuje, kiedy to pierwszy kilku zadania został zakończony.

### <a name="rotation-and-anchors"></a>Obrót i zakotwiczenia

Podczas wywoływania `ScaleTo`, `RelScaleTo`, `RotateTo`, i `RelRotateTo` metody, można ustawić [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) i [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) właściwości w celu wskazania środek skalowanie i obrót.

[ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) pokazano tej techniki przez obracanie `Button` wokół środka strony.

### <a name="easing-functions"></a>Funkcje easingu

Ogólnie rzecz biorąc animacji są liniowej z wartość początkową na wartość końcową. Funkcje easingu może spowodować animacji przyspieszyć lub zwolnić za pośrednictwem ich kursu. Ostatni argument opcjonalne dla metod animacji jest typu [ `Easing` ](xref:Xamarin.Forms.Easing), klasa, która definiuje 11 statycznego pola tylko do odczytu typu `Easing`:

- [`Linear`](xref:Xamarin.Forms.Easing.Linear), wartość domyślna
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn), [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut), i [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn), [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut), i [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)
- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) i [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) i [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)

`In` Sufiks wskazuje, że wpływ na początku animacji, `Out` oznacza, że na końcu, a `InOut` oznacza, że jest na początku i końca animacji.

[ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) przykład demonstruje użycie funkcje easingu.

### <a name="your-own-easing-functions"></a>Funkcje easingu

Można również definiować możesz własnych funkcji sterowania tempem zmian przez przekazanie [ `Func<double, double>` ](xref:System.Func`2) do [ `Easing` Konstruktor](xref:Xamarin.Forms.Easing.%23ctor(System.Func{System.Double,System.Double})). `Easing` definiuje również niejawna konwersja z `Func<double, double>` do `Easing`. Argument funkcji sterowania tempem zmian jest zawsze w zakresie od 0 do 1, ponieważ animacji liniowo rozpoczynające się od początku do końca. Funkcja *zwykle* zwraca wartość z zakresu od 0 do 1, ale może być krótko ujemna ani większa niż 1 (jak w przypadku `SpringIn` i `SpringOut` funkcji) lub może spowodować przerwanie reguły, jeśli wiesz, co robisz.

[ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) w przykładzie pokazano niestandardową funkcję sterowania tempem zmian i [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) pokazuje innego.

[ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) w przykładzie pokazano również niestandardowych funkcji sterowania tempem zmian, a także techniki zmiany `AnchorX` i `AnchorY` właściwości w ramach sekwencji animacji obrotu.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka ma [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) klasę, która używa niestandardowych ułatwianie funkcja jiggle przycisku, po kliknięciu. [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) w przykładzie pokazano go.

### <a name="entrance-animations"></a>Wejście animacji

Jeden typ popularnych animacji występuje, gdy najpierw zostanie wyświetlona strona. Takie animacji, może zostać uruchomiony w [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) zastąpienie części strony. W przypadku tych animacji, zaleca się konfigurowanie XAML dla jak strona ma się pojawić *po* animacji, zainicjować i animować układ z kodu.

[ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) przykładowy używa [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metody rozszerzenia do zastosowania blaknięcia w zawartości strony.

[ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) przykładowy używa [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodę rozszerzenia, aby slajdów w zawartości strony, od ścian.

[ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) przykładowy używa [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodę rozszerzenia, aby animować `RotationY` właściwości. A [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metoda jest również dostępna.

### <a name="forever-animations"></a>Nieskończona animacji

Y. "nieskończoność" animacji działać do czasu zakończenia programu. Zazwyczaj są one przeznaczone do celów demonstracyjnych.

[ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) przykładowy używa [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) animacji zanikania dwóch rodzajów tekst wewnątrz i na zewnątrz.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) Wyświetla palindromem, a następnie sekwencyjnie obraca się poszczególne listy o 180 stopni, dzięki czemu są one wszystkie nogami. Następnie cały ciąg jest odwrócony 180 stopni do odczytu w taki sam jak oryginalny ciąg.

[ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) przykładowe obraca się prostej `BoxView` śmigłowca podczas obrotowymi środku ekranu.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) opiera się funkcjonowanie `BoxView` szprychy wokół środka ekranu, a następnie obraca się w każdej szprysze, aby utworzyć interesujących wzorców:

[![Potrójna zrzut ekranu przedstawiający obracanie szprychy](images/ch22fg21-small.png "obracanie szprychy")](images/ch22fg21-large.png#lightbox "obracanie szprychy")

Stopniowe zwiększanie jednak `Rotation` właściwość elementu może nie działać w długim okresie, jako [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) w przykładzie pokazano.

[ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) przykładowy używa [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), i [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) się to wydawać się tak, jakby mapy bitowej obraca się w przestrzeni 3D.

### <a name="animating-the-bounds-property"></a>Animowanie właściwości granice

Jedyną metodą rozszerzenia w `ViewExtensions` nie została jeszcze przedstawiona jest [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), które skutecznie animuje tylko do odczytu [ `Bounds` ](xref:Xamarin.Forms.VisualElement.Bounds) właściwości przez wywołanie metody [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metody. Ta metoda jest zazwyczaj wywoływana `Layout` pochodne, jak wspomniano w temacie [ **rozdział 26. CustomLayouts**](chapter26.md).

`LayoutTo` Metody powinny być ograniczone do specjalnych celów. [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) program używa jej do kompresowania i rozwiń `BoxView` zgodnie z jego przerzucona boki strony.

[ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) przykładowy używa `LayoutTo` przenieść Kafelki w implementacji klasycznego układanki 15-16, wyświetlającego zaszyfrowaną obrazu, a nie numerowane Kafelki:

[![Potrójna zrzut ekranu przedstawiający Xamarin Xuzzle](images/ch22fg26-small.png "Xuzzle Układanka — gra")](images/ch22fg26-large.png#lightbox "Xuzzle Układanka — gra")

### <a name="your-own-awaitable-animations"></a>Oczekujący animacji

[ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) przykładowy skrypt umożliwia utworzenie oczekujący animacji. Kluczowym klasę, która może zwracać `Task` obiekt z metody i sygnału po zakończeniu animacji jest [ `TaskCompletionSource` ](xref:System.Threading.Tasks.TaskCompletionSource`1).

## <a name="deeper-into-animations"></a>Większe zagłębienie w animacji

System animacji Xamarin.Forms może być nieco mylące. Oprócz `Easing` klasa, obejmuje system animacji `ViewExtensions`, `Animation`, i `AnimationExtension` klasy.

### <a name="viewextensions-class"></a>Klasa ViewExtensions

Napotkano już [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions). Definiuje dziewięć metody, które zwracają `Task<bool>` i [ `CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)). Przekształć siedem docelowej dziewięć metody właściwości. Istnieją dwie [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), które elementy docelowe [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) właściwości i [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), która wywołuje metodę [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metody.

### <a name="animation-class"></a>Klasa animacji

[ `Animation` ](xref:Xamarin.Forms.AnimationExtensions) Klasa ma [Konstruktor](xref:Xamarin.Forms.Animation.%23ctor(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Action)) z pięciu argumentów, aby zdefiniować wywołanie zwrotne i zakończono metod oraz parametry animacji.

Animacje podrzędne można dodać z [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)), [ `Insert` ](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)), [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)), i i przeciążenia [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Double,System.Double)).

Obiekt animacji jest następnie uruchamiany przy użyciu wywołania do [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) metody.

### <a name="animationextensions-class"></a>Klasa AnimationExtensions

[ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions) Klasy zawiera głównie metody rozszerzenia. Istnieją różne wersje programu `Animate` metody i ogólnego [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) metody jest na tyle uniwersalny, że to naprawdę funkcja animacji tylko potrzebne.

## <a name="working-with-the-animation-class"></a>Praca z klasy animacji

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) w przykładzie pokazano [ `Animation` ](xref:Xamarin.Forms.Animation) klasy przy użyciu kilku różnych animacji.

### <a name="child-animations"></a>Animacje podrzędne

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) w przykładzie pokazano również podrzędne animacji, które powodują, że użycie (bardzo podobne) [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) i [ `Insert` ](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)) metody.

### <a name="beyond-the-high-level-animation-methods"></a>Poza metody wysokiego poziomu animacji

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) przykładowy pokazuje również, jak wykonać animacji, które wykraczają poza docelowe właściwości przez `ViewExtensions` metody. W jednym z przykładów Pobierz dłużej; ciągu okresów w kolejnym przykładzie `BackgroundColor` właściwość jest animowany.

### <a name="more-of-your-own-awaitable-methods"></a>Jedną z metod oczekujący

[ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metody `ViewExtensions` nie działa w przypadku [ `Easing.SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) funkcji. Zatrzymuje ją, gdy sterowania tempem zmian danych wyjściowych pobiera powyżej 1.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka zawiera [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) klasy [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) i [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) metody rozszerzenia, które nie mają tego problemu, a także [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) i [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) metody anulowania tych animacje.

[ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) pokazuje `TranslateXTo` metody.

`MoreExtensions` Klasa zawiera także [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) metodę rozszerzenia, która łączy translacji X i Y, a [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) metody.

### <a name="implementing-a-bezier-animation"></a>Implementowanie animacji Beziera

Istnieje również możliwość tworzenia animacji, który przenosi element wzdłuż ścieżki Krzywa Beziera. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka zawiera [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) strukturę, która hermetyzuje Krzywa Beziera i [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) wyliczenia do sterowania orientacji.

[ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) Klasa zawiera [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) — metoda rozszerzenia i [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) metody.

[ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) przykład pokazuje, Animowanie elementu w ścieżce Beizer.

## <a name="working-with-animationextensions"></a>Praca z AnimationExtensions

Brakuje standardowa kolekcja animacji jest animacji kolorów. Problem jest, że nie istnieje sposób prawo do interpolacji między dwoma `Color` wartości. Istnieje możliwość interpolacji poszczególne wartości RGB, ale tylko jako prawidłowy interpolacji wartości HSL.

Z tego powodu [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka zawiera dwa `Color` metody animacji: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)i [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Dostępne są również dwie metody anulowania: [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) i [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

Obie metody użytkowania [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), który wykonuje animacji, wywołując obszerne ogólne [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) method in Class metoda [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions).

[ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) przykładzie pokazano użycie tych dwóch typów animacji kolorów.

## <a name="structuring-your-animations"></a>Tworzenie struktury animacji

Czasami warto express animacje w XAML i używaj ich w połączeniu z modelem MVVM. To zagadnienie opisano w następnym rozdziale [ **działu 23. Wyzwalacze i zachowania**](chapter23.md).



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 22 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Przykłady rozdział 22](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Animacja](~/xamarin-forms/user-interface/animation/index.md)
