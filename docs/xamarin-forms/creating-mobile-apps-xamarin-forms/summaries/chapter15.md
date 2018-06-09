---
title: Podsumowanie działu 15. Interaktywnego interfejsu
description: 'Tworzenie aplikacji mobilnych za pomocą platformy Xamarin.Forms: Podsumowanie działu 15. Interaktywnego interfejsu'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: aac49c9e74dd22642396ea8daf5ee3abd85de7bf
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241900"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Podsumowanie działu 15. Interaktywnego interfejsu

W tym rozdziale Eksploruje osiem `View` pochodne, które zezwalają na interakcję z użytkownikiem.

## <a name="view-overview"></a>Widok — omówienie

Platformy Xamarin.Forms zawiera 20 tworzone jako wystąpienia klasy, które pochodzą z `View` , ale nie `Layout`. Sześć one zostać omówione w poprzednich rozdziałach:

- `Label`: [ **Rozdział 2. Struktura aplikacji**](chapter02.md)
- `BoxView`: [ **Rozdział 3. Przewijanie stosu**](chapter03.md)
- `Button`: [ **Rozdział 6. Kliknięcie przycisku**](chapter06.md)
- `Image`: [ **Rozdziale 13. Mapy bitowe**](chapter13.md)
- `ActivityIndicator`: [ **Rozdziale 13. Mapy bitowe**](chapter13.md)
- `ProgressBar`: [ **Rozdział 14. AbsoluteLayout**](chapter14.md)

Osiem widoków, w tym rozdziale skutecznie Zezwalaj użytkownikom na interakcję z podstawowych typów danych .NET:

|Typ danych|Widoki|
|--- |--- |
|`Double`|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/), [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|
|`Boolean`|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|
|`String`|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/), [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|
|`DateTime`|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/), [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|

Widoki te można traktować jako podstawowe typy danych visual interaktywnej prezentacji. To pojęcie jest bardziej przedstawione w rozdziale dalej [ **rozdziale 16. Powiązanie danych**](chapter16.md).

W poniższych rozdziałach opisano pozostałych widoków sześć:

- `WebView`: [ **Rozdziale 16. Powiązanie danych**](chapter16.md)
- `Picker`: [ **Rozdziale 19. Kolekcja widoków**](chapter19.md)
- `ListView`: [ **Rozdziale 19. Kolekcja widoków**](chapter19.md)
- `TableView`: [ **Rozdziale 19. Kolekcja widoków**](chapter19.md)
- `Map`: [ **Działu 28. Lokalizacja i map**](chapter28.md)
- `OpenGLView`: Nie omówione w tym książki (i nie obsługuje platformy systemu Windows)

## <a name="slider-and-stepper"></a>Suwak i stepper

Zarówno [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) i [ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) Zezwalaj użytkownikowi na wybranie wartość liczbową z zakresu. `Slider` Jest ciągły zakres podczas `Stepper` obejmuje wartości dyskretnych.

### <a name="slider-basics"></a>Podstawy suwaka

[ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) Jest poziomym paskiem reprezentujący zakres wartości co najmniej po lewej stronie na maksymalnie po prawej stronie. Definiuje właściwości publiczne trzy:

- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) typu `double`, domyślna wartość 0
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) typu `double`, domyślna wartość 0
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) typu `double`, domyślna wartość 1

Właściwości, które wykonują kopie tych właściwości upewnij się, że są one zgodne:

- Wszystkie trzy właściwości [ `coerceValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) określona dla właściwości możliwej do wiązania upewnia się, że metoda `Value` między `Minimum` i `Maximum`.
- [ `validateValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+ValidateValueDelegate/) Metoda `MinimumProperty` zwraca `false` Jeśli `Minimum` jest ustawiany na wartość, większa lub równa `Maximum`i podobne `MaximumProperty`. Zwracanie `false` z `validateValue` metody powoduje, że `ArgumentException` do wywołania.

`Slider` uruchamiany [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) zdarzenie z [ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) argumentu po `Value` zmiany właściwości programowo lub gdy użytkownik modyfikuje `Slider`.

[ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) przykładzie przedstawiono użycie prostego `Slider`.

### <a name="common-pitfalls"></a>Typowych problemów

Zarówno w kodzie, jak i w języku XAML `Minimum` i `Maximum` właściwości są ustawione w kolejności, który określisz. Należy zainicjować te właściwości, aby `Maximum` zawsze jest większa niż `Minimum`. W przeciwnym razie zostanie wygenerowany wyjątek.

Inicjowanie `Slider` właściwości może spowodować `Value` właściwości do zmiany i `ValueChanged` zdarzeń do uruchomienia. Należy upewnić się, że `Slider` program obsługi zdarzeń nie dostępu do widoków, które nie zostały jeszcze utworzone podczas inicjowania strony.

`ValueChanged` Zdarzeń nie wyzwalać podczas `Slider` inicjowania chyba że `Value` zmiany właściwości. Możesz wywołać `ValueChanged` obsługi bezpośrednio z kodu.

### <a name="slider-color-selection"></a>Wybór koloru suwaka

[ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) program zawiera trzy `Slider` elementów, które pozwala interaktywnie wybierać kolor, określając jego wartości RGB:

[![Potrójna zrzut ekranu przedstawiający suwaki R G B](images/ch15fg03-small.png "suwaki RGB")](images/ch15fg03-large.png#lightbox "RGB suwaki")

[ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) w przykładzie użyto dwóch `Slider` elementy, aby przenieść dwie `Label` elementów `AbsoluteLayout` i zanikania jednego do drugiego.

### <a name="the-stepper-difference"></a>Różnica Stepper

[ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) Definiuje te same właściwości i zdarzenia jako `Slider` , ale `Maximum` właściwość jest inicjowana do 100 i `Stepper` definiuje właściwość czwarty:

- [`Increment`](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) typu `double`, zainicjowane 1

Efekty wizualne `Stepper` składa się z dwóch przycisków z etykietą **&ndash;** i **+**. Naciśnięcie przycisku **&ndash;** zmniejsza `Value` przez `Increment` co najmniej `Minimum`. Naciśnięcie przycisku **+** zwiększa `Value` przez `Increment` maksymalnie `Maximum`.

Wskazuje na [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) próbki.

## <a name="switch-and-checkbox"></a>Przełącznik i pól wyboru

[ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) Pozwala użytkownikowi określić wartość logiczną.

### <a name="switch-basics"></a>Podstawowe informacje o przełączniku

Efekty wizualne `Switch` składa się z przełącznikiem, który może być włączony i Włącz. Klasa definiuje jedną właściwość:

- [`IsToggled`](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) typu `bool`

`Switch` Definiuje jedno zdarzenie:

- [`Toggled`](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) wraz z [ `ToggledEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToggledEventArgs/) obiektu, gdy uruchamiane `IsToggled` zmiany właściwości.

[ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) program ilustruje `Switch`.

### <a name="a-traditional-checkbox"></a>Tradycyjny wyboru

Niektórzy deweloperzy łączyli się bardziej tradycyjnej `CheckBox` do `Switch`. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka zawiera `CheckBox` klasą pochodzącą z `ContentView`. `CheckBox` jest implementowany przez [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) i [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) plików. `CheckBox` Definiuje właściwości trzy (`Text`, `FontSize`, i `IsChecked`) i `CheckedChanged` zdarzeń.

[ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) przykładzie pokazano to `CheckBox`.

## <a name="typing-text"></a>Wpisywanie tekstu

Platformy Xamarin.Forms definiuje trzy widoki, które umożliwiają użytkownikowi wprowadzanie i edytowanie tekstu:

- [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) dla jednego wiersza tekstu
- [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) wiele wierszy tekstu
- [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) dla jednego wiersza tekstu na potrzeby wyszukiwania.

`Entry` i `Editor` pochodzi od [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/), która jest pochodną `View`. `SearchBar` pochodzi bezpośrednio z `View`.

### <a name="keyboard-and-focus"></a>Klawiatura i fokus

Z telefonów i tabletów bez fizycznej klawiatury `Entry`, `Editor`, i `SearchBar` wszystkie elementy spowodować klawiatury wirtualnej przeskoczyć do góry. Obecność ten skróty na ekranie jest związana z fokus wprowadzania. W widoku musi być zarówno jego [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) i [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) właściwości `true` uzyskać fokus wprowadzania.

Dwie metody, jedną właściwość tylko do odczytu i dwa zdarzenia są związane z fokus wprowadzania. Są one wszystkie zdefiniowane przez `VisualElement`:

- [ `Focus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Focus()/) Metoda próbuje ustawić fokus wprowadzania do elementu i zwraca `true` w razie powodzenia
- [ `Unfocus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Unfocus()/) Metoda usuwa element fokus wprowadzania
- [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) Wskazuje właściwość tylko do odczytu, jeśli element ma wejściowych fokus
- [ `Focused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Focused/) Zdarzenie wskazuje, kiedy element pobiera fokus wprowadzania
- [ `Unfocused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Unfocused/) Zdarzenie wskazuje, kiedy element utraci fokus wprowadzania

### <a name="choosing-the-keyboard"></a>Wybieranie klawiatury

[ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) Klasy, z którego `Entry` i `Editor` pochodzi definiuje tylko jedną właściwość:

- [`Keyboard`](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) typu [`Keyboard`](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/)

To ustawienie określa typ klawiatury, która jest wyświetlana. Niektóre klawiatury są zoptymalizowane pod kątem identyfikatory URI lub cyfry.

`Keyboard` Klasa umożliwia definiowanie klawiatury ze statycznego [ `Keyboard.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Keyboard.Create/p/Xamarin.Forms.KeyboardFlags/) metody z argumentem typu [ `KeyboardFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardFlags/), wyliczenie z następujących flag bitowych:

- `None` wartość 0
- [`CapitalizeSentence`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.CapitalizeSentence/) Ustaw wartość 1
- [`Spellcheck`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Spellcheck/) wartość 2
- [`Suggestions`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Suggestions/) wartość 4
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.All/) Ustaw \xFFFFFFFF

Używając multiline [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) gdy akapitu lub więcej tekstu, wywoływania `Keyboard.Create` jest dobre rozwiązanie do wybierania klawiatury. Na jeden wiersz [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), następujące statycznej właściwości tylko do odczytu z `Keyboard` są przydatne:

- [`Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Default/)
- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Text/)
- [`Chat`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Chat/)
- [`Url`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Url/)
- [`Email`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Email/)
- [`Telephone`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Telephone/)
- [`Numeric`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Numeric/) dla liczb dodatnich z lub bez separatorem dziesiętnym.

[ `KeyboardTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardTypeConverter/) Umożliwiają określenie tych właściwości w języku XAML, jak pokazano w [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) programu.

### <a name="entry-properties-and-events"></a>Właściwości wpisu i zdarzenia

Jeden wiersz [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) definiuje następujące właściwości:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) typu `string`, tekst wyświetlany w `Entry`
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) typu `Color`
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontFamily/) typu `string`
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontSize/) typu `double`
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontAttributes/) typu `FontAttributes`
- [`IsPassword`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.IsPassword/) typu `bool`, co powoduje, że znaków do zamaskowania
- [`Placeholder`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) typu `string`, dimly kolorowe tekstu, która jest wyświetlana w `Entry` przed niczego wpisano
- [`PlaceholderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.PlaceholderColor/) typu `Color`

`Entry` Definiuje również dwa zdarzenia:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) z [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) obiektu wywoływane zawsze, gdy `Text` zmiany właściwości
- [`Completed`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/), po zakończeniu i klawiatury jest odrzucane. Użytkownik wskazuje uzupełniania w sposób specyficzne dla platformy

[ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) przykładzie pokazano te dwa zdarzenia.

### <a name="the-editor-difference"></a>Różnica edytora

Multiline [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) definiuje takie same `Text` i `Font` właściwości jako `Entry` , ale nie inne właściwości. `Editor` definiuje również dwa takie same właściwości jako `Entry`.

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) to program sporządzania notatek dowolnych, który zapisuje i przywraca zawartość `Editor`.

### <a name="the-searchbar"></a>SearchBar

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Nie pochodzi od `InputView`, więc nie ma `Keyboard` właściwości. Ale ma wszystkie `Text`, `Font`, i `Placeholder` właściwości który `Entry` definiuje. Ponadto `SearchBar` definiuje trzy dodatkowe właściwości:

- [`CancelButtonColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.CancelButtonColor/) typu `Color`
- [`SearchCommand`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) typu [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) do użytku z powiązania danych i MVVM
- [`SearchCommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) typu `Object`, do użycia z programem `SearchCommand`

Specyficzne dla platformy anulować wymazywanie przycisk tekst. `SearchBar` Również znajduje się przycisk wyszukiwania specyficznego dla platformy. Jeden z tych przycisków naciśnięcie zgłasza jednego z dwóch zdarzeń który `SearchBar` definiuje:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.TextChanged/) wraz z [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) obiektu
- [`SearchButtonPressed`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/)

[ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) przykładzie pokazano `SearchBar`.

## <a name="date-and-time-selection"></a>Wybór daty i godziny

[ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) i [ `TimePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) widoków wdrożenie środków specyficzne dla platformy, które umożliwiają użytkownikowi określenie daty lub godziny.

### <a name="the-datepicker"></a>Selektora daty

[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) Definiuje właściwości cztery:

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) typu `DateTime`, zainicjowane do 1 stycznia 1900 roku.
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) typu `DateTime`, zainicjowane do 31 grudnia 2100
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) typu `DateTime`, zainicjowane do `DateTime.Today`
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) typu `string`, .NET został zainicjowany w "d", wzorzec krótkiej daty ciąg formatowania, co powoduje wyświetlanie dat, takich jak "7/20/1969" w Stanach Zjednoczonych.

Można ustawić `DateTime` właściwości w języku XAML, przedstawiając właściwości jako elementy właściwości i używając daty krótkiej niezmiennej kultury formatu ("7/20/1969").   

[ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) próbki oblicza liczbę dni między datami wybrane przez użytkownika.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (lub jest on TimeSpanPicker?)

[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) definiuje dwie właściwości i zdarzeń:

- [`Time`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) Typ jest `TimeSpan` zamiast `DateTime`, wskazująca czas, jaki upłynął od północy
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Format/) typu `string`, .NET formatowanie ciąg został zainicjowany w "t" wzorzec krótki czas, co powoduje wyświetlanie godziny, takich jak "1:45 PM" w Stanach Zjednoczonych.

[ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) program ilustruje sposób używania `TimePicker` Aby określić godzinę dla czasomierza. Program działa tylko, jeśli zachowasz na pierwszym planie.

**SetTimer** ilustruje też przy użyciu [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) metody `Page` Aby wyświetlić okno dialogowe alertu.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 15 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Przykłady działu 15](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Wpis](~/xamarin-forms/user-interface/text/entry.md)
- [Edytor](~/xamarin-forms/user-interface/text/editor.md)
