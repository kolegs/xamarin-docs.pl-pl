---
title: Podsumowanie działu 23. Wyzwalacze i zachowania
description: 'Tworzenie aplikacji mobilnych za pomocą platformy Xamarin.Forms: Podsumowanie działu 23. Wyzwalacze i zachowania'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0e5609a3df94914594d6547d9a1887078d282127
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241286"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Podsumowanie działu 23. Wyzwalacze i zachowania

Wyzwalacze i zachowania są podobne, w tym są oba przeznaczone do użycia w plikach XAML uprościć interakcje elementu poza używanie powiązania danych i mogą rozszerzyć funkcjonalność elementów XAML. Zachowania elementów i wyzwalaczy prawie zawsze są używane z obiektami visual interfejsu użytkownika.

Obsługa wyzwalaczy i zachowania, zarówno `VisualElement` i `Style` obsługuje dwie właściwości kolekcji:

- [`VisualElement.Triggers`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) i [ `Style.Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Triggers/) typu `IList<TriggerBase>`
- [`VisualElement.Behaviors`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) i [ `Style.Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Behaviors/) typu `IList<Behavior>`

## <a name="triggers"></a>Wyzwalacze

Wyzwalacz jest warunek (zmiana właściwości lub uruchamiania zdarzenia), który powoduje odpowiedzi (inny zmiany właściwości lub uruchomienia niektórych kodu). `Triggers` Właściwość `VisualElement` i `Style` jest typu `IList<TriggersBase>`. [`TriggerBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerBase/) jest klasą abstrakcyjną, z którego pochodzi cztery zapieczętowane klasy:

- [`Trigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) Aby uzyskać odpowiedzi na podstawie zmian właściwości
- [`EventTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/) odpowiedzi oparte na firings zdarzeń
- [`DataTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) dla odpowiedzi na podstawie danych powiązania
- [`MultiTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) odpowiedzi na wiele wyzwalaczy podstawie

Wyzwalacz jest zawsze ustawiony na element, którego właściwość został zmieniony przez wyzwalacz.

### <a name="the-simplest-trigger"></a>Najprostsza wyzwalacza

[ `Trigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) Klasa sprawdza zmiany wartości właściwości i odpowiada ustawiając inną właściwość tego samego elementu.

`Trigger` definiuje trzy właściwości:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Property/) typu `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Value/) typu `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Setters/) typu `IList<SetterBase>`, właściwość zawartości `Trigger`

Ponadto `Trigger` wymaga się, że następujące właściwości odziedziczone `TriggerBase` można ustawić:

- [`TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.TargetType/) Aby wskazać typ elementu, na którym `Trigger` jest dołączony

`Property` i `Value` zawiera warunek i `Setters` kolekcja jest odpowiedź. Gdy wskazany `Property` ma wartość wskazywana przez `Value`, a następnie `Setter` obiekty w `Setters` kolekcji są stosowane. Gdy `Property` ma inną wartość, metody ustawiające zostaną usunięte. `Setter` definiuje dwie właściwości, które są takie same jak właściwości dwóch pierwszych `Trigger`:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) typu `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/) typu `Object`

[ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) w przykładzie pokazano, jak `Trigger` stosowane do `Entry` można zwiększyć rozmiar `Entry` za pośrednictwem `Scale` właściwości podczas `IsFocused`właściwość `Entry` jest `true`.

Chociaż nie jest to często, `Trigger` można ustawić w kodzie, jako [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) przykładzie pokazano.

[ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) w przykładzie pokazano, jak `Trigger` można ustawić w `Style` do zastosowania do wielu `Entry` elementów.

### <a name="trigger-actions-and-animations"></a>Akcje wyzwalacza i animacji

Istnieje również możliwość uruchomienia trochę kodu opartego na wyzwalacza. Ten kod może być przeznaczonego dla właściwości animacji. Typowy sposób jest użycie [ `EventTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/), który definiuje dwie właściwości:

- [`Event`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Event/) typu `string`, nazwę zdarzenia
- [`Actions`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Actions/) typu `IList<TriggerAction>`, listy Akcje do wykonania w odpowiedzi.

Aby użyć tej funkcji, należy napisać klasą pochodzącą z [ `TriggerAction<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/), zazwyczaj `TriggerAction<VisualElement>`. Można określić właściwości tej klasy. Są to zwykły CLR właściwości, a nie można powiązać właściwości, ponieważ `TriggerAction` nie pochodzi od `BindableObject`. Konieczne jest przesłonięcie [ `Invoke` ](https://developer.xamarin.com/api/member/Xamarin.Forms.TriggerAction%3CT%3E.Invoke/p/T/) metodę, która jest wywoływana po wywołaniu akcji. Argument jest elementem docelowym.

[ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka jest przykładem. Wywołuje `ScaleTo` właściwości animacji `Scale` właściwości elementu. Ponieważ typ właściwości jest `Easing`, [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) klasa pozwala na korzystanie ze standardu `Easing` pól statycznych w języku XAML.

[ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) przykładowych pokazano, jak wywołać `ScaleAction` z `EventTrigger` obiektów monitorujących `Focused` i `Unfocused` zdarzenia.

[ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) pokazano sposób definiowania niestandardowych funkcji sterowania tempem zmian dla `ScaleAction` w pliku CodeBehind.

Można także wywoływać akcji przy użyciu `Trigger` (distinguished od `EventTrigger`). Wymaga to, że masz świadomość który `TriggerBase` definiuje dwie kolekcje:

- [`EnterActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.EnterActions/) typu `IList<TriggerAction>`
- [`ExitActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.ExitActions/) typu `IList<TriggerAction>`

[ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) przykładowych pokazano, jak używać tych kolekcji.

### <a name="more-event-triggers"></a>Wyzwalacze zdarzeń więcej

[ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) wywołania biblioteki `ScaleTo` dwa razy, aby skalować w górę i w dół. [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) w przykładzie użyto to w zastosowaniu stylu `EventTrigger` do wizualne podczas `Button` zostanie naciśnięty. To podwójne animacji jest również możliwe przy użyciu dwie akcje w kolekcji typu [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

[ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) Klasy w **Xamarin.FormsBook.Toolkit** Biblioteka definiuje akcji shiver można dostosowywać. [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) przykładzie pokazano go.

[ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) Klasy w **Xamarin.FormsBook.Toolkit** biblioteki jest ograniczony do `Entry` elementy i zestawy `TextColor` właściwości na czerwony, gdy `Text` właściwości nie jest `double`. [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) przykładzie pokazano go.

### <a name="data-triggers"></a>Wyzwalacze danych

[ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) Jest podobny do `Trigger` z tą różnicą, że zamiast monitorowania zmian wartości właściwości, monitoruje powiązanie danych. Dzięki temu właściwość w jeden element wpływać na właściwość innego elementu.

`DataTrigger` definiuje trzy właściwości:

- [`Binding`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Binding/) typu `BindingBase`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Value/) typu `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Setters/) typu `IList<SetterBase>`

[ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) przykład wymaga [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) biblioteki i ustawia kolory nazwy studentów na niebieski lub na podstawie różowy `Sex` właściwości:

[![Potrójna zrzut ekranu przedstawiający kolory płci](images/ch23fg04-small.png "kolory płci")](images/ch23fg04-large.png#lightbox "płci kolorów")

[ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) przykładowe zestawy `IsEnabled` właściwość `Entry` do `False` Jeśli `Length` właściwość `Text` właściwości `Entry`jest równe 0. Zwróć uwagę, że `Text` właściwość jest inicjowana na ciąg pusty; domyślnie jest `null`i `DataTrigger` nie działać poprawnie.

### <a name="combining-conditions-in-the-multitrigger"></a>Łączenie warunków w MultiTrigger

[ `MultiTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) To zbiór warunków. Gdy są one wszystkich `true`, a następnie ustawiające są stosowane. Klasa definiuje dwie właściwości:

- [`Conditions`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Conditions/) typu `IList<Condition>`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Setters/) typu `IList<Setter>`

[`Condition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/) jest klasą abstrakcyjną i zawiera dwa pochodną klasy:

- [`PropertyCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/), która zawiera [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Property/) i [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Value/) właściwości, takie jak `Trigger`
- [`BindingCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingCondition/), która zawiera [ `Binding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Binding/) i [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Value/) właściwości, takie jak `DataTrigger`

W [ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) próbki, `BoxView` jest tylko pokolorowane przy czterech `Switch` elementy są włączone.

[ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) przykładzie pokazano, jak można wprowadzać `BoxView` kolor po *żadnych* czterech `Switch` elementy są włączone. Wymaga to De Morgan prawa i cofania całą logikę aplikacji.

Łączenie i i logiki nie jest tak proste i zwykle wymaga niewidoczne `Switch` elementów pośrednich wyników. [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) w przykładzie pokazano, jak `Button` można włączyć, jeśli dwa `Entry` elementy niektórych tekstu wpisane, ale nie, jeśli oba mają wpisane tekst.

## <a name="behaviors"></a>Zachowania

Wszystko można zrobić za pomocą wyzwalacza, możesz również wykonać za pomocą zachowań, ale zachowania zawsze wymagają klasą pochodzącą z [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) i zastępuje następujących dwóch metod:

- [`OnAttachedTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/T/)
- [`OnDetachingFrom`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/T/)

Argument jest dołączona do zachowania elementu. Ogólnie rzecz biorąc `OnAttachedTo` metody dołącza niektóre programy obsługi zdarzeń i `OnDetachingFrom` odłącza je. Ponieważ takie klasy zwykle zapisuje niektórych stanu, jego zazwyczaj nie może być współużytkowana w `Style`.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) próbki jest podobny do **TriggerEntryValidation** z tą różnicą, że używa zachowanie &mdash; [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki.

### <a name="behaviors-with-properties"></a>Zachowania z właściwościami

`Behavior<T>` pochodzi z `Behavior`, która jest pochodną `BindableObject`, więc można powiązać właściwości można zdefiniować na zachowanie. Te właściwości mogą być aktywne w powiązania danych.

To jest przedstawiona w [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) program, który sprawia, że użycie [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) klasy w [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki. `ValidEmailBehavior` ma można powiązać właściwości tylko do odczytu i służy jako źródło powiązania danych.

[ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) próbki używa tego samego zachowania, aby wyświetlić innego typu wskaźnika, która sygnalizuje, że adres e-mail jest nieprawidłowy.

[ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) próbki jest odmianą na powyższego przykładu. Używa ButtonGlide `DataTrigger` w połączeniu z tego zachowania.

### <a name="toggles-and-check-boxes"></a>Włącza lub wyłącza i pól wyboru

Istnieje możliwość Hermetyzuj zachowanie przycisk przełącznika w klasie, takich jak [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki, a następnie zdefiniuj wszystkie obraz podczas przełączania w całości w języku XAML.

[ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) przykładowe używa `ToggleBehavior` z `DataTrigger` do używania `Label` z dwa ciągi tekstowe dla przełącznik.

[ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) próbki rozszerza tę koncepcję przez przełączanie między dwiema `FormattedString` obiektów.

[ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) Klasy w **Xamarin.FormsBook.Toolkit** biblioteki pochodną `ContentView`, definiuje `IsToggled` właściwości i dołącza `ToggleBehavior` dla przełącznika Logika. Ułatwia określenie przycisk przełącznika w języku XAML, jak pokazano w [ **TranditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) próbki.

[ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) obejmuje [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) klasą pochodzącą z `ToggleBase` i używa [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)klasy do skonstruowania przycisk przełącznika podobny platformy Xamarin.Forms `Switch`.

A [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) w **Xamarin.FormsBook.Toolkit** zawiera animacji umożliwia pokazanie animowany poziom w [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)próbki.

### <a name="responding-to-taps"></a>Odpowiada na żądania podsłuchu

Wadą z `EventTrigger` jest, że nie można dołączyć do `TapGestureRecognizer` odpowiedzieć podsłuchu. Pobieranie rozwiązać ten problem, jest celem [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) w [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

[ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) przykładowe używa `TapBehavior` Aby użyć wcześniej `ShiverAction` dla dotknięciu `BoxView` elementów.

[ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) przykładowych pokazano, jak wycinanie znaczników hermetyzując `ShiverView` klasy.

### <a name="radio-buttons"></a>Przyciski opcji

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ma również Biblioteka [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) klasy dokonywania przycisków radiowych, które są pogrupowane według `string` nazwę grupy.

[ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) program używa ciągów tekstowych dla jego przycisku radiowego. [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) przykładowe używa `Style` różnicy pomiędzy zaznaczony i niezaznaczony przycisków. [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) w przykładzie użyto opakowanego obrazy jego przycisków radiowych:

[![Potrójna zrzut ekranu opcji obrazów](images/ch23fg17-small.png "obrazy dla przycisków radiowych")](images/ch23fg17-large.png#lightbox "obrazy dla przycisków radiowych")

[ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) próbki rysuje tradycyjnych umieszczone przyciski radiowe z kropką okręgu.

### <a name="fades-and-orientation"></a>Zanikanie i orientacji

Próbka końcowa [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) można przełączać się między trzy widoki różnych wybór kolorów za pomocą przycisków radiowych. Trzy widoki zanikania i zmniejszanie przy użyciu [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki.

Program również odpowiada na zmiany w orientacji pomiędzy pionowy i pozioma przy użyciu [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) w **Xamarin.FormsBook.Toolkit** biblioteki.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 23 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Przykłady działu 23](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Praca z wyzwalaczy](~/xamarin-forms/app-fundamentals/triggers.md)
- [Zachowania zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
