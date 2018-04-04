---
title: Podsumowanie rozdział 22. Animacja
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d18b563c0fc47db8c6d7f8cebb3ec85989064d20
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-22-animation"></a>Podsumowanie rozdział 22. Animacja

W tym samouczku, możesz utworzyć własne animacje za pomocą czasomierza platformy Xamarin.Forms lub `Task.Delay`, ale łatwiej przy użyciu funkcji animacji udostępniane przez platformy Xamarin.Forms. Trzy klasy zaimplementować te animacji.

- [`ViewExtensions`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/), podejście wysokiego poziomu
- [`Animation`](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/), bardziej elastyczne, ale przeszkodę
- [`AnimationExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/), najbardziej podejście uniwersalny, najniższym poziomie

Ogólnie rzecz biorąc animacje docelowego właściwości, które bazują na właściwości. Nie jest wymagane, ale są tylko właściwości, które dynamicznie reagowania na zmiany.

Nie ma interfejsu XAML dla tych animacji, ale animacji można zintegrować XAML przy użyciu techniki opisane w [ **działu 23. Wyzwalacze i zachowania**](chapter23.md).

## <a name="exploring-basic-animations"></a>Eksploracja animacje podstawowe

Funkcje podstawowe animacji metodami rozszerzenia w [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) klasy. Te metody stosowane do każdego obiektu, która jest pochodną `VisualElement`. Najprostsza animacje docelowy przekształcenia właściwości omówione w [ `Chapter 21. Transforms` ](chapter21.md).

[ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) pokazano, jak `Clicked` programu obsługi zdarzeń dla `Button` można wywołać [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) — metoda rozszerzenia do obracania przycisk w okręgu.

`RotateTo` Zmiany metody `Rotation` właściwość `Button` z zakresu od 0 do 360 w ciągu sekundy co kwartał (domyślnie). Jeśli `Button` jest dotknięciu ponownie, jednak ją nie działają, ponieważ `Rotation` właściwość pojawiła się już 360 stopni.

### <a name="setting-the-animation-duration"></a>Ustawienie czasu trwania animacji

Drugi argument `RotateTo` jest czas w milisekundach. Jeśli ustawienie na wartość duże naciśnięcie `Button` podczas animacji rozpoczyna się nowy początek animacji pod kątem bieżącej.

### <a name="relative-animations"></a>Względne animacji

`RelRotateTo` Metoda przeprowadza obrotu względną, dodając określoną wartość do istniejącej wartości. Ta metoda umożliwia `Button` wybierać wiele razy, a każdy wzrost czasu `Rotation` właściwości przez 360 stopni.

### <a name="awaiting-animations"></a>Oczekiwanie na animacji

Wszystkie metody animacji w `ViewExtensions` zwracać `Task<bool>` obiektów. Oznacza to, że zdefiniować szereg sekwencję animacji przy użyciu `ContinueWith` lub `await`. `bool` Ukończenia zwracana wartość to `false` animacji zakończyli bez przeszkód lub `true` Jeśli zostało anulowane przez [ `CancelAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) metodę, która anuluje wszystkie animacje inicjowane przez inne metody w `ViewExtensions` ustawionych w tym samym elemencie.

### <a name="composite-animations"></a>Złożone animacji

Można mieszać animacji oczekiwano i oczekiwane do tworzenia złożonych animacji. Są to animacji `ViewExtensions` kierowanych `TranslatonX`, `TranslationY`, i `Scale` przekształcenie właściwości:

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`ScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.ScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)

Zwróć uwagę, że `TranslateTo` może potencjalnie dotyczyć zarówno `TranslationX` i `TranslationY` właściwości.

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll i Task.WhenAny

Istnieje również możliwość zarządzania jednoczesnych animacji przy użyciu [ `Task.WhenAll` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAll%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), która sygnalizuje wraz z wielu zadań mają wszystkie, i [ `Task.WhenAny` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAny%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), która sygnalizuje, kiedy pierwszy kilku zawarła zadania.

### <a name="rotation-and-anchors"></a>Obracanie i punkty kontrolne

Podczas wywoływania metody `ScaleTo`, `RelScaleTo`, `RotateTo`, i `RelRotateTo` metod, można ustawić [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) i [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) właściwości w celu wskazania Centrum skalowanie i obrót.

[ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) pokazuje ta technika przez obracanie `Button` wokół środka strony.

### <a name="easing-functions"></a>Funkcji sterowania tempem zmian

Ogólnie rzecz biorąc animacje są liniowej z wartość początkową wartość end. Ułatwianie funkcji może spowodować animacji przyspieszenia lub zmniejszyć za pośrednictwem ich kursu. Ostatni argument opcjonalne metod animacji jest typu [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/), klasa, która definiuje 11 statycznego pola tylko do odczytu typu `Easing`:

- [`Linear`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/), wartość domyślna
- [`SinIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/), [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/), i [`SinInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/)
- [`CubicIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/), [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/), i [`CubicInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/)
- [`BounceIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) I [`BounceOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/)
- [`SpringIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) I [`SpringOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)

`In` Sufiks oznacza, że wpływ na początku animacji, `Out` oznacza, że przed upływem i `InOut` oznacza, że jest na początku i końca animacji.

[ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) przykładzie przedstawiono użycie łatwiejszym funkcji.

### <a name="your-own-easing-functions"></a>Funkcji sterowania tempem zmian

Można również zdefiniować możesz własnych funkcji sterowania tempem zmian przez przekazanie [ `Func<double, double>` ](https://developer.xamarin.com/api/type/System.Func%3CT,TResult%3E/) do [ `Easing` konstruktora](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Easing.Easing/p/System.Func%7BSystem.Double,System.Double%7D/). `Easing` definiuje również niejawna konwersja z `Func<double, double>` do `Easing`. Argument funkcji sterowania tempem zmian jest zawsze z zakresu od 0 do 1, jak animacji liniowo rozpoczynające się od początku do końca. Funkcja *zwykle* zwraca wartość z zakresu od 0 do 1, ale może być krótko ujemna ani większa niż 1 (jak w przypadku `SpringIn` i `SpringOut` funkcje) lub może spowodować przerwanie reguły, jeśli znasz wykonywanych czynności.

[ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) przykładzie pokazano niestandardowej funkcji sterowania tempem zmian i [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) pokazuje innego.

[ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) przykładzie pokazano także niestandardowej funkcji sterowania tempem zmian i techniki zmiany `AnchorX` i `AnchorY` właściwości w sekwencji animacji obrotu.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka ma [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) klasy, która używa niestandardowych łatwiejszym funkcja jiggle przycisku po kliknięciu. [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) przykładzie pokazano go.

### <a name="entrance-animations"></a>Wejście animacji

Jeden typ popularnych animacji występuje, gdy najpierw zostanie wyświetlona strona. Animacja takie mogą być uruchamiane w [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) zastąpienie strony. W przypadku tych animacji, zaleca się konfigurowanie XAML określający sposób stronie pojawią się *po* animacji, zainicjować i animować układu z kodu.

[ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) przykładowe używa [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) — metoda rozszerzenia, aby ukryć w treści strony.

[ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) przykładowe używa [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Przesuń w treści strony z boków metodę rozszerzenie.

[ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) przykładowe używa [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) — metoda rozszerzenia do animowania `RotationY` właściwości. A [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metody jest również dostępna.

### <a name="forever-animations"></a>Nieskończona animacji

Y. "nieskończoność" animacji uruchomiony do momentu program zostanie zakończony. Zazwyczaj są przeznaczone do celów demonstracyjnych.

[ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) przykładowe używa [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) animacji i wylogowywanie zanikania dwóch fragmentów tekstu.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) Wyświetla palindrome, a następnie sekwencyjnie obraca poszczególne koperty o 180 stopni, są one wszystkich dołu. Następnie cały ciąg jest odwrócony 180 stopni do odczytu w taki sam jak oryginalny ciąg.

[ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) próbki obraca prostą `BoxView` śmigłowca podczas obracanie go wokół środka ekranu.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) się funkcjonowanie `BoxView` szprychy wokół środka ekranu, a następnie obraca każdego gwiazdy, aby utworzyć interesujących wzorców:

[![Potrójna zrzut ekranu przedstawiający obracanie szprychy](images/ch22fg21-small.png "obracanie szprychy")](images/ch22fg21-large.png#lightbox "obracanie szprychy")

Jednak stopniowe zwiększanie `Rotation` właściwości elementu może nie działać w długim okresie, jako [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) przykładzie pokazano.

[ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) przykładowe używa [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), i [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) aby stał się wydawać się tak, jakby mapy bitowej jest obracanie przestrzeni 3D.

### <a name="animating-the-bounds-property"></a>Animowanie właściwości granic

Jedyną metodą rozszerzenia w `ViewExtensions` jeszcze nie zostało to pokazane jest [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), które skutecznie animuje tylko do odczytu [ `Bounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) właściwości przez wywołanie metody [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metody. Ta metoda jest zazwyczaj wywoływana przez `Layout` pochodne, jak wspomniano w temacie [ **rozdział 26. CustomLayouts**](chapter26.md).

`LayoutTo` Metody powinny być ograniczone do celów specjalnych. [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) program używa go do skompresowania i rozwiń `BoxView` zgodnie z jego odrzuceń poza strony, strony.

[ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) przykładowe używa `LayoutTo` można przenieść Kafelki w implementacji klasycznego układanki 15-16, zawierający zaszyfrowaną obrazu, a nie numerem Kafelki:

[![Potrójna zrzut ekranu przedstawiający Xamarin Xuzzle](images/ch22fg26-small.png "gry Układanka — Xuzzle")](images/ch22fg26-large.png#lightbox "Xuzzle Układanka — gry")

### <a name="your-own-awaitable-animations"></a>Oczekujący animacji

[ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) przykładowe tworzy oczekujący animacji. Kluczowe znaczenie klasy, która może zwracać `Task` obiekt z metody i sygnału po zakończeniu animacji jest [ `TaskCompletionSource` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskCompletionSource%3CTResult%3E/).

## <a name="deeper-into-animations"></a>Zapoznanie się z animacji

System animacji platformy Xamarin.Forms może być nieco mylące. Oprócz `Easing` klasa, system animacji obejmuje `ViewExtensions`, `Animation`, i `AnimationExtension` klasy.

### <a name="viewextensions-class"></a>Klasa ViewExtensions

Napotkano już [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/). Definiuje metody dziewięć, które zwracają `Task<bool>` i [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/). Przekształć siedem docelowy metody dziewięć właściwości. Są dwa inne [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), które elementy docelowe [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) właściwości, oraz [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), które wywołuje [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metody.

### <a name="animation-class"></a>Animacja — klasa

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) Klasa ma [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Animation.Animation/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Action/) z pięciu argumentów do definiowania wywołania zwrotnego i metod Zakończono oraz parametry animacji.

Animacje podrzędne można dodać z [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/), i i przeciążenia z [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Double/System.Double/).

Obiektu animacji jest następnie uruchomiona z wywołaniem do [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BSystem.Double,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) metody.

### <a name="animationextensions-class"></a>Klasa AnimationExtensions

[ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) Klasy zawiera głównie metody rozszerzenia. Dostępne są różne wersje programu `Animate` — metoda i ogólnych [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) metody jest na tyle uniwersalny, że to naprawdę funkcja tylko animacji należy.

## <a name="working-with-the-animation-class"></a>Praca z klasy animacji

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) przykładzie pokazano [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) klasy z kilku różnych animacji.

### <a name="child-animations"></a>Animacje podrzędne

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) przykładzie przedstawiono również podrzędny animacji, które powodują, że użycie (bardzo podobnie) [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) i [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/) metody.

### <a name="beyond-the-high-level-animation-methods"></a>Poza metodami animacji wysokiego poziomu

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) próbki również pokazano, jak wykonać animacji, które wykraczają poza docelowych właściwości `ViewExtensions` metody. W jednym z przykładów ciągu okresów uzyskać dłużej; w kolejnym przykładzie `BackgroundColor` jest animowany właściwości.

### <a name="more-of-your-own-awaitable-methods"></a>Jeden oczekujący metody

[ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metody `ViewExtensions` nie działa z [ `Easing.SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) funkcji. Zatrzymuje go po pobiera wyjściowych dynamiki powyżej 1.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka zawiera [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) klasy z [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) i [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) metody rozszerzenia, które nie mają ten problem, a także [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) i [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) metod dla tych anulowania animacji.

[ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) pokazuje `TranslateXTo` metody.

`MoreExtensions` Klasa zawiera także [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) — metoda rozszerzenia łączącą tłumaczenia X i Y, a [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) metody.

### <a name="implementing-a-bezier-animation"></a>Implementowanie animacji Beziera

Istnieje również możliwość wprowadzenia animacji, który przenosi element krzywej Beziera krzywej składanej na ścieżce. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka zawiera [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) strukturę, która hermetyzuje Beziera krzywej składanej i [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) wyliczeniu, aby Orientacja formantu.

[ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) Klasa zawiera [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) — metoda rozszerzenia i [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) metody.

[ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) przykładzie pokazano element wzdłuż ścieżki Beizer animacji.

## <a name="working-with-animationextensions"></a>Praca z AnimationExtensions

Brakuje standardowej kolekcji animacji jest animacji kolorów. Problem jest, że nie istnieje sposób prawo do interpolacji między dwiema `Color` wartości. Istnieje możliwość interpolować poszczególne wartości RGB, ale tylko jako prawidłowy interpolacji wartości HSL.

Z tego powodu [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka zawiera dwa `Color` metody animacji: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)i [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Dostępne są również dwie metody anulowania: [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) i [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

Obie metody należy korzystać z [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), który wykonuje animacji przez wywołanie metody ogólnej szeroką gamę [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) metody w [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/).

[ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) przykładzie pokazano, przy użyciu tych dwóch typów animacji kolorów.

## <a name="structuring-your-animations"></a>Struktury animacji

Czasami jest przydatne do express animacji w języku XAML i ich używać w połączeniu z modelem MVVM. Ten temat znajdują się w rozdziale dalej [ **działu 23. Wyzwalacze i zachowania**](chapter23.md).



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst 22 rozdziałów (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Przykłady rozdział 22](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Animacja](~/xamarin-forms/user-interface/animation/index.md)
