---
title: Podsumowanie rozdziałów 23. Wyzwalacze i zachowania
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziałów 23. Wyzwalacze i zachowania'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b5257ca6df09c95b425b8a0929d25e3575bc9d8c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996145"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Podsumowanie rozdziałów 23. Wyzwalacze i zachowania

Wyzwalacze i zachowania są podobne, w tym zarówno mają one używane w plikach XAML, aby uprościć elementu poza użycie interakcji z klientami powiązań danych i rozszerzać funkcjonalność elementów XAML. Wyzwalacze i zachowania prawie zawsze są używane z obiektów wizualnych interfejsu użytkownika.

Aby obsługiwać wyzwalacze i zachowania, zarówno `VisualElement` i `Style` obsługuje dwie właściwości kolekcji:

- [`VisualElement.Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) i [ `Style.Triggers` ](xref:Xamarin.Forms.Style.Triggers) typu `IList<TriggerBase>`
- [`VisualElement.Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) i [ `Style.Behaviors` ](xref:Xamarin.Forms.Style.Behaviors) typu `IList<Behavior>`

## <a name="triggers"></a>Wyzwalacze

Wyzwalacz jest warunek (zmiana właściwości lub uruchomieniem zdarzenia), który skutkuje odpowiedzi (inny zmiana właściwości lub uruchamiania kodu). `Triggers` Właściwość `VisualElement` i `Style` typu `IList<TriggersBase>`. [`TriggerBase`](xref:Xamarin.Forms.TriggerBase) jest klasą abstrakcyjną, z której pochodzi cztery zapieczętowane klasy:

- [`Trigger`](xref:Xamarin.Forms.Trigger) Aby uzyskać odpowiedzi na podstawie zmian właściwości
- [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) dla odpowiedzi oparte na firings zdarzeń
- [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) Aby uzyskać odpowiedzi oparte na powiązania danych
- [`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) Aby uzyskać odpowiedzi oparte na wiele wyzwalaczy

Wyzwalacz jest zawsze ustawiony na element, którego właściwość został zmieniony przez wyzwalacz.

### <a name="the-simplest-trigger"></a>Najprostsza wyzwalacza

[ `Trigger` ](xref:Xamarin.Forms.Trigger) Klasa sprawdza, czy zmiana wartości właściwości i reaguje, ustawiając innej właściwości tego samego elementu.

`Trigger` definiuje trzy właściwości:

- [`Property`](xref:Xamarin.Forms.Trigger.Property) tego typu `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Trigger.Value) tego typu `Object`
- [`Setters`](xref:Xamarin.Forms.Trigger.Setters) typu `IList<SetterBase>`, właściwość zawartości `Trigger`

Ponadto `Trigger` wymaga, że następujące właściwości są dziedziczone z `TriggerBase` można ustawić:

- [`TargetType`](xref:Xamarin.Forms.TriggerBase.TargetType) Aby wskazać typ elementu, na którym `Trigger` jest dołączony

`Property` i `Value` obejmują warunku, a `Setters` kolekcja jest odpowiedź. Gdy wskazany `Property` ma wartość wskazywana przez `Value`, a następnie `Setter` obiekty w `Setters` kolekcji są stosowane. Gdy `Property` ma inną wartość, metody ustawiające zostały usunięte. `Setter` definiuje dwie właściwości, które są takie same jak właściwości dwóch pierwszych `Trigger`:

- [`Property`](xref:Xamarin.Forms.Setter.Property) tego typu `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Setter.Value) tego typu `Object`

[ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) w przykładzie pokazano, jak `Trigger` dotyczą `Entry` można zwiększyć rozmiar `Entry` za pośrednictwem `Scale` właściwości podczas `IsFocused`właściwość `Entry` jest `true`.

Chociaż nie jest powszechne, `Trigger` można ustawić w kodzie jako [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) w przykładzie pokazano.

[ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) w przykładzie pokazano sposób, w jaki `Trigger` można ustawić w `Style` dotyczą wielu `Entry` elementów.

### <a name="trigger-actions-and-animations"></a>Wyzwalaj akcje i animacji

Istnieje również możliwość uruchomienia nieco kodu, w oparciu o wyzwalacz. Ten kod może być animacji, który jest przeznaczony dla właściwości. Jednym ze sposobów typowych jest użycie [ `EventTrigger` ](xref:Xamarin.Forms.EventTrigger), która definiuje dwie właściwości:

- [`Event`](xref:Xamarin.Forms.EventTrigger.Event) typu `string`, Nazwa zdarzenia
- [`Actions`](xref:Xamarin.Forms.EventTrigger.Actions) typu `IList<TriggerAction>`, lista akcje do wykonania w odpowiedzi.

Aby użyć tej opcji, należy napisać klasę, która pochodzi od klasy [ `TriggerAction<T>` ](xref:Xamarin.Forms.TriggerAction`1), zazwyczaj `TriggerAction<VisualElement>`. Można zdefiniować właściwości tej klasy. Są to zwykły CLR właściwości, a nie właściwości możliwej do wiązania ponieważ `TriggerAction` nie pochodzi od `BindableObject`. Konieczne jest przesłonięcie [ `Invoke` ](xref:Xamarin.Forms.TriggerAction`1.Invoke*) metodę, która jest wywoływana, gdy jest wywoływana Akcja. Argument jest elementem docelowym.

[ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka jest przykładem. Wywołuje `ScaleTo` właściwość animować `Scale` właściwości elementu. Ponieważ jedna z jego właściwości jest kolumną typu `Easing`, [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) klasy pozwala używać standardowych `Easing` pól statycznych w XAML.

[ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) przykład pokazuje, jak wywołać `ScaleAction` z `EventTrigger` obiektów monitorujących `Focused` i `Unfocused` zdarzenia.

[ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) przykład pokazuje jak zdefiniować funkcję sterowania tempem zmian niestandardową dla `ScaleAction` w pliku związanym z kodem.

Można także wywoływać akcje przy użyciu `Trigger` (distinguished od `EventTrigger`). Wymaga to, że masz świadomość, `TriggerBase` definiuje dwie kolekcje:

- [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) tego typu `IList<TriggerAction>`
- [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) tego typu `IList<TriggerAction>`

[ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) przykład pokazuje, jak używać tych kolekcji.

### <a name="more-event-triggers"></a>Więcej wyzwalaczy zdarzeń

[ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) wywołania biblioteki `ScaleTo` dwa razy, aby skalować w górę i w dół. [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) próbka używa tej wartości w ze stylem `EventTrigger` można przekazać wizualną opinię podczas `Button` naciśnięciu. Ta animacja double jest również możliwe, za pomocą dwie akcje w kolekcji typów [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

[ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) Klasy w **Xamarin.FormsBook.Toolkit** Biblioteka definiuje akcji shiver można dostosowywać. [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) w przykładzie pokazano go.

[ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) Klasy w **Xamarin.FormsBook.Toolkit** biblioteki jest ograniczony do `Entry` elementów i zestawów `TextColor` właściwości na czerwony, gdy `Text` właściwości nie jest `double`. [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) w przykładzie pokazano go.

### <a name="data-triggers"></a>Wyzwalacze danych

[ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) Przypomina `Trigger` z tą różnicą, że zamiast monitorowania zmian wartości właściwości, monitoruje ona powiązanie danych. Umożliwia to właściwość w jeden element wpływa na właściwości w innym elemencie.

`DataTrigger` definiuje trzy właściwości:

- [`Binding`](xref:Xamarin.Forms.DataTrigger.Binding) tego typu `BindingBase`
- [`Value`](xref:Xamarin.Forms.DataTrigger.Value) tego typu `Object`
- [`Setters`](xref:Xamarin.Forms.DataTrigger.Setters) tego typu `IList<SetterBase>`

[ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) przykładowy skrypt wymaga [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) biblioteki i zestawy kolory nazwiska uczniów na niebieski lub na podstawie różowy `Sex` właściwości:

[![Potrójna zrzut ekranu przedstawiający kolory płeć](images/ch23fg04-small.png "kolory płeć")](images/ch23fg04-large.png#lightbox "płeć kolorów")

[ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) przykładowe zestawy `IsEnabled` właściwość `Entry` do `False` Jeśli `Length` właściwość `Text` właściwość `Entry`jest równa 0. Należy zauważyć, że `Text` właściwość jest inicjowana na ciąg pusty; domyślnie jest `null`i `DataTrigger` nie działa prawidłowo.

### <a name="combining-conditions-in-the-multitrigger"></a>Łączenie warunków w SE

[ `MultiTrigger` ](xref:Xamarin.Forms.MultiTrigger) To zbiór warunków. Gdy są one wszystkie `true`, a następnie metody ustawiające są stosowane. Klasa definiuje dwie właściwości:

- [`Conditions`](xref:Xamarin.Forms.MultiTrigger.Conditions) tego typu `IList<Condition>`
- [`Setters`](xref:Xamarin.Forms.MultiTrigger.Setters) tego typu `IList<Setter>`

[`Condition`](xref:Xamarin.Forms.Condition) jest klasą abstrakcyjną i zawiera dwie klasy podrzędny:

- [`PropertyCondition`](xref:Xamarin.Forms.Condition), który ma [ `Property` ](xref:Xamarin.Forms.PropertyCondition.Property) i [ `Value` ](xref:Xamarin.Forms.PropertyCondition.Value) właściwości, takie jak `Trigger`
- [`BindingCondition`](xref:Xamarin.Forms.BindingCondition), który ma [ `Binding` ](xref:Xamarin.Forms.BindingCondition.Binding) i [ `Value` ](xref:Xamarin.Forms.BindingCondition.Value) właściwości, takie jak `DataTrigger`

W [ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) próbki, `BoxView` jest tylko kolorowe po czterech `Switch` elementy są włączone z.

[ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) przykład pokazuje, jak można `BoxView` kolor po *wszelkie* czterech `Switch` elementy są włączone. Wymaga to prawa De Morgan oraz cofania całą logikę aplikacji.

Łączenie i i logiki nie jest tak proste i zazwyczaj wymaga niewidoczne `Switch` elementy dla wyników pośrednich. [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) w przykładzie pokazano, jak `Button` można włączyć, jeśli jedna z dwóch `Entry` elementy mają jakiś tekst wpisany w, ale nie wtedy, gdy oba mają jakiś tekst wpisany w.

## <a name="behaviors"></a>Zachowania

Nic, możesz zrobić z wyzwalaczem, można również wykonać za pomocą zachowań, ale zachowania zawsze należy wymagać klasę, która pochodzi od klasy [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) i zastąpień w następujących dwóch metod:

- [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo*)
- [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom*)

Argument jest dołączona do zachowania elementu. Ogólnie rzecz biorąc `OnAttachedTo` metoda dołącza niektóre procedury obsługi zdarzeń i `OnDetachingFrom` odłącza je. Ponieważ takie klasy zwykle zapisuje pewnego stanu, jego ogólnie nie można udostępniać w `Style`.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) przykład jest podobny do **TriggerEntryValidation** z tą różnicą, że używa zachowania &mdash; [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki.

### <a name="behaviors-with-properties"></a>Zachowania za pomocą właściwości

`Behavior<T>` pochodzi od klasy `Behavior`, który pochodzi od klasy `BindableObject`, więc to zachowanie można zdefiniować właściwości możliwej do wiązania. Te właściwości mogą być aktywne w powiązań danych.

Jest to zaprezentowane w [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) program, który sprawia, że użycie [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) klasy w [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki. `ValidEmailBehavior` ma właściwość może być powiązana tylko do odczytu i służy jako źródło powiązania danych.

[ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) przykładowych użyto to takie samo zachowanie, aby wyświetlić inny typ wskaźnika do sygnalizowania, że adres e-mail jest nieprawidłowy.

[ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) przykład jest modyfikacją poprzedniego przykładu. Używa ButtonGlide `DataTrigger` w połączeniu z tego zachowania.

### <a name="toggles-and-check-boxes"></a>Włącza lub wyłącza i zaznacz pola wyboru

Istnieje możliwość hermetyzacji zachowanie przycisku przełącznika w klasie, takich jak [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki, a następnie zdefiniuj wszystkie elementy wizualne przełącznik w całości XAML.

[ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) przykładowy używa `ToggleBehavior` z `DataTrigger` używać `Label` z dwa ciągi tekstowe do przełącznika.

[ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) przykładowe rozszerza tę koncepcję przez przełączanie między dwoma `FormattedString` obiektów.

[ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) Klasy w **Xamarin.FormsBook.Toolkit** pochodzi od klasy biblioteki `ContentView`, definiuje `IsToggled` właściwości i dołącza `ToggleBehavior` dla przełącznika Logika. To sprawia, że łatwiej zdefiniować przycisk przełączania w XAML, jak pokazano w [ **TranditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) próbki.

[ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) obejmuje [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) klasę pochodzącą od `ToggleBase` i używa [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)klasy, aby utworzyć przycisk przełącznika, który przypomina Xamarin.Forms `Switch`.

A [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) w **Xamarin.FormsBook.Toolkit** zapewnia animacji umożliwia animowany suwaka w [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)próbki.

### <a name="responding-to-taps"></a>Odpowiadanie na podsłuchu

Wadą z `EventTrigger` jest, że nie można dołączyć ją do `TapGestureRecognizer` odpowiedzieć na podsłuchu. Poruszanie się po wystąpieniu jest celem [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) w [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

[ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) przykładowy używa `TapBehavior` do użycia wcześniej `ShiverAction` dla nacisnął `BoxView` elementów.

[ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) przykład pokazuje, jak w ograniczeniu znaczników poprzez hermetyzację `ShiverView` klasy.

### <a name="radio-buttons"></a>Przyciski radiowe

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka ma również [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) klasy składania przycisków radiowych, które są pogrupowane według `string` nazwę grupy.

[ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) program używa ciągów tekstowych dla jego przycisku radiowego. [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) przykładowy używa `Style` różnicy pomiędzy checked i unchecked przycisków. [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) próbki spakowany obrazy dla jego przycisków radiowych:

[![Potrójna zrzut ekranu przedstawiający obrazów Radio](images/ch23fg17-small.png "obrazy przycisków radiowych")](images/ch23fg17-large.png#lightbox "obrazy przycisków radiowych")

[ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) przykładowe rysuje tradycyjnych umieszczone przyciski radiowe z dot wewnątrz okręgu.

### <a name="fades-and-orientation"></a>Zanikanie i orientacji

Próbki końcowej [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) pozwala na przełączanie między trzy widoki wybór innego koloru za pomocą przycisków radiowych. Trzy widoki zanikanie wewnątrz i na zewnątrz przy użyciu [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki.

Również reaguje na zmiany w orientacji między pionowa i pozioma [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) w **Xamarin.FormsBook.Toolkit** biblioteki.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 23 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Przykłady działu 23](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Praca z wyzwalaczami](~/xamarin-forms/app-fundamentals/triggers.md)
- [Zachowania zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
