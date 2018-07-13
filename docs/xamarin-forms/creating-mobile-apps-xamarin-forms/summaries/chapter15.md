---
title: Podsumowanie rozdziałów 15. Interaktywny interfejs
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziałów 15. Interaktywny interfejs'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 6da3753d723ed44ca640d8c80ae07258a03cbbbc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998670"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Podsumowanie rozdziałów 15. Interaktywny interfejs

W tym rozdziale przedstawiono osiem `View` pochodnych, które zezwalają na interakcję z użytkownikiem.

## <a name="view-overview"></a>Wyświetl przegląd

Xamarin.Forms zawiera 20 tworzone jako wystąpienia klasy, które wynikają z `View` , ale nie `Layout`. Sześć one zostać omówione w poprzednich akapitach:

- `Label`: [ **Rozdział 2. Anatomia aplikacji**](chapter02.md)
- `BoxView`: [ **Rozdział 3. Przewijanie stosu**](chapter03.md)
- `Button`: [ **Rozdział 6. Kliknięcie przycisku**](chapter06.md)
- `Image`: [ **Rozdziale 13. Mapy bitowe**](chapter13.md)
- `ActivityIndicator`: [ **Rozdziale 13. Mapy bitowe**](chapter13.md)
- `ProgressBar`: [ **Rozdział 14. AbsoluteLayout**](chapter14.md)

Osiem widoków, w tym rozdziale skutecznie Zezwalaj użytkownikom na interakcję z podstawowych typów danych .NET:

|Typ danych|Widoki|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

Widoki te można traktować jako wizualnej reprezentacji interaktywne podstawowych typów danych. Takie podejście jest bardziej przedstawione w następnym rozdziale [ **rozdział 16. Powiązanie danych**](chapter16.md).

Pozostałe widoki sześć zostały omówione w następujących rozdziałach:

- `WebView`: [ **Rozdział 16. Powiązanie danych**](chapter16.md)
- `Picker`: [ **Rozdziale 19. Widoki kolekcji**](chapter19.md)
- `ListView`: [ **Rozdziale 19. Widoki kolekcji**](chapter19.md)
- `TableView`: [ **Rozdziale 19. Widoki kolekcji**](chapter19.md)
- `Map`: [ **Działu 28. Lokalizacja i mapy**](chapter28.md)
- `OpenGLView`: Nie omówione w tej książce (i nie obsługuje platformy Windows)

## <a name="slider-and-stepper"></a>Suwaka i stepper

Zarówno [ `Slider` ](xref:Xamarin.Forms.Slider) i [ `Stepper` ](xref:Xamarin.Forms.Stepper) Zezwalaj użytkownikowi na wybranie wartość liczbową z zakresu. `Slider` Jest ciągły zakres podczas `Stepper` obejmuje wartości dyskretnych.

### <a name="slider-basics"></a>Podstawy suwaka

[ `Slider` ](xref:Xamarin.Forms.Slider) Jest poziomym paskiem reprezentująca zakres wartości z co najmniej po lewej stronie do maksymalnie po prawej stronie. Definiuje trzy właściwości publiczne:

- [`Value`](xref:Xamarin.Forms.Slider.Value) typu `double`, domyślna wartość 0
- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) typu `double`, domyślna wartość 0
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) typu `double`, domyślna wartość 1

Właściwości możliwe do wiązania, które wykonują kopie tych właściwości upewnij się, że są spójne:

- W przypadku wszystkich trzech właściwości [ `coerceValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) określona dla właściwości możliwej do wiązania, masz pewność, że metoda `Value` między `Minimum` i `Maximum`.
- [ `validateValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+ValidateValueDelegate/) Metody `MinimumProperty` zwraca `false` Jeśli `Minimum` jest ustawiana na wartość, większa niż lub równa `Maximum`i podobnie dla `MaximumProperty`. Zwracanie `false` z `validateValue` powoduje, że metoda `ArgumentException` zgłoszenie.

`Slider` generowane [ `ValueChanged` ](xref:Xamarin.Forms.Slider.ValueChanged) zdarzenie z [ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs) argument po `Value` zmiany właściwości programowo lub gdy użytkownik modyfikuje `Slider`.

[ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) przykład demonstruje użycie prostego `Slider`.

### <a name="common-pitfalls"></a>Typowych pułapek

Zarówno w kodzie, jak i w XAML `Minimum` i `Maximum` właściwości są ustawione w określonej kolejności. Pamiętaj zainicjować te właściwości, aby `Maximum` jest zawsze większa niż `Minimum`. W przeciwnym razie zostanie wygenerowany wyjątek.

Inicjowanie `Slider` właściwości może spowodować, że `Value` właściwości do zmiany i `ValueChanged` zdarzeń do uruchomienia. Należy upewnić się, że `Slider` program obsługi zdarzeń nie dostępu do widoków, które nie zostały jeszcze utworzone podczas inicjowania strony.

`ValueChanged` Zdarzenie nie zostanie wyzwolony podczas `Slider` inicjowania chyba że `Value` zmiany właściwości. Możesz wywołać `ValueChanged` obsługi bezpośrednio z kodu.

### <a name="slider-color-selection"></a>Wybór koloru suwaka

[ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) program zawiera trzy `Slider` elementy, które umożliwiają interaktywnie wybierz kolor, określając jego wartości RGB:

[![Potrójna zrzut ekranu przedstawiający suwaki R G B](images/ch15fg03-small.png "suwaki RGB")](images/ch15fg03-large.png#lightbox "RGB suwaki")

[ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) przykład korzysta z dwóch `Slider` elementy można przenieść dwóch `Label` elementów między `AbsoluteLayout` i zanikanie jednego do drugiego.

### <a name="the-stepper-difference"></a>Różnica Stepper

[ `Stepper` ](xref:Xamarin.Forms.Stepper) Definiuje te same właściwości i zdarzenia jako `Slider` ale `Maximum` właściwość jest inicjowana do 100 i `Stepper` definiuje właściwość czwarty:

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) typu `double`, zainicjowana na 1

Wizualnie `Stepper` składa się z dwóch przycisków etykietą **&ndash;** i **+**. Naciśnięcie klawisza **&ndash;** zmniejsza `Value` przez `Increment` do minimalnej liczby `Minimum`. Naciśnięcie klawisza **+** zwiększa `Value` przez `Increment` maksymalnie `Maximum`.

Wskazuje na [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) próbki.

## <a name="switch-and-checkbox"></a>Przełącznik i pól wyboru

[ `Switch` ](xref:Xamarin.Forms.Switch) Zezwala użytkownikowi na określenie typu wartość logiczna.

### <a name="switch-basics"></a>Podstawowe informacje o przełącznikiem

Wizualnie `Switch` składa się z przełącznik, który można włączać, wyłączać i włączać. Klasa definiuje jedną właściwość:

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) tego typu `bool`

`Switch` Definiuje jedno zdarzenie:

- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) wraz z [ `ToggledEventArgs` ](xref:Xamarin.Forms.ToggledEventArgs) obiektu, gdy wywoływane `IsToggled` zmiany właściwości.

[ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) program pokazuje `Switch`.

### <a name="a-traditional-checkbox"></a>Tradycyjne pola wyboru

Niektórzy deweloperzy preferować bardziej tradycyjny `CheckBox` do `Switch`. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka zawiera `CheckBox` klasę pochodzącą od `ContentView`. `CheckBox` jest implementowana przez [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) i [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) plików. `CheckBox` definiuje trzy właściwości (`Text`, `FontSize`, i `IsChecked`) i `CheckedChanged` zdarzeń.

[ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) w przykładzie pokazano to `CheckBox`.

## <a name="typing-text"></a>Wpisywanie tekstu

Zestaw narzędzi Xamarin.Forms definiuje trzy widoki, które umożliwiają użytkownikowi wprowadzanie i edytowanie tekstu:

- [`Entry`](xref:Xamarin.Forms.Entry) Aby uzyskać pojedynczy wiersz tekstu
- [`Editor`](xref:Xamarin.Forms.Editor) wiele wierszy tekstu
- [`SearchBar`](xref:Xamarin.Forms.SearchBar) dla pojedynczego wiersza tekstu na potrzeby wyszukiwania.

`Entry` i `Editor` dziedziczyć [ `InputView` ](xref:Xamarin.Forms.InputView), która jest pochodną `View`. `SearchBar` pochodzi bezpośrednio z `View`.

### <a name="keyboard-and-focus"></a>Klawiatura i koncentracji uwagi

Na telefonach i tabletach bez fizycznego klawiatury `Entry`, `Editor`, i `SearchBar` klawiatury wirtualnej przeskoczyć do góry spowodować, że wszystkie elementy. Obecność tego klawiatury na ekranie jest powiązana z fokus wejścia. W widoku musi być zarówno jego [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) i [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) właściwości ustawione na `true` można pobrać fokus wprowadzania.

Dwie metody, jedna właściwość tylko do odczytu i dwa zdarzenia biorących udział fokusa wejścia. Te są definiowane przez `VisualElement`:

- [ `Focus` ](xref:Xamarin.Forms.VisualElement.Focus) Metoda próbuje ustawić fokusu wprowadzania do elementu i zwraca `true` w razie powodzenia
- [ `Unfocus` ](xref:Xamarin.Forms.VisualElement.Unfocus) Metoda usuwa fokusu wprowadzania z elementu
- [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) Wskazuje właściwość tylko do odczytu, jeśli element ma fokus wejścia
- [ `Focused` ](xref:Xamarin.Forms.VisualElement.Focused) Zdarzenie wskazuje, kiedy element pobiera fokus wprowadzania
- [ `Unfocused` ](xref:Xamarin.Forms.VisualElement.Unfocused) Zdarzenie wskazuje, kiedy element utraci fokus wprowadzania

### <a name="choosing-the-keyboard"></a>Wybieranie klawiatury

[ `InputView` ](xref:Xamarin.Forms.InputView) Klasa, z której `Entry` i `Editor` pochodzić definiuje tylko jedną właściwość:

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) tego typu [`Keyboard`](xref:Xamarin.Forms.Keyboard)

To ustawienie określa typ klawiatury, który jest wyświetlany. Niektóre klawiatury są zoptymalizowane pod kątem identyfikatorów URI lub liczby.

`Keyboard` Klasa umożliwia definiowanie klawiatury przy użyciu statycznego [ `Keyboard.Create` ](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) metody za pomocą argumentu typu [ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags), wyliczenie z następujących flag bitowych:

- `None` ustawiona na 0
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) ustawiona na 1
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) wartość 2
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) równa 4
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) Ustaw \xFFFFFFFF

Korzystając z wielowierszowe [ `Editor` ](xref:Xamarin.Forms.Editor) chwilą oczekiwanego akapitu lub więcej tekstu podczas wywoływania `Keyboard.Create` to dobra metoda wybierania klawiatury. Dla pojedynczego wiersza [ `Entry` ](xref:Xamarin.Forms.Entry), następujące statycznych właściwości tylko do odczytu z `Keyboard` są przydatne:

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) dla liczb dodatnich z lub bez punktu dziesiętnego.

[ `KeyboardTypeConverter` ](xref:Xamarin.Forms.KeyboardTypeConverter) Umożliwia określenie tych właściwości w XAML, jak pokazano w [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) program.

### <a name="entry-properties-and-events"></a>Zapis właściwości i zdarzenia

Jeden wiersz [ `Entry` ](xref:Xamarin.Forms.Entry) definiuje następujące właściwości:

- [`Text`](xref:Xamarin.Forms.Entry.Text) typu `string`, tekst, który pojawia się w `Entry`
- [`TextColor`](xref:Xamarin.Forms.Entry.TextColor) tego typu `Color`
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily) tego typu `string`
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize) tego typu `double`
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes) tego typu `FontAttributes`
- [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) typu `bool`, co powoduje, że znaki, które mają być maskowane
- [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) typu `string`, dimly kolorowe tekstu, który pojawia się w `Entry` przed niczego kontrolą typów
- [`PlaceholderColor`](xref:Xamarin.Forms.Entry.PlaceholderColor) tego typu `Color`

`Entry` Definiuje również dwa zdarzenia:

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) za pomocą [ `TextChangedEventArgs` ](xref:Xamarin.Forms.TextChangedEventArgs) obiektu, uruchamiane zawsze, gdy `Text` zmiany właściwości
- [`Completed`](xref:Xamarin.Forms.Entry.Completed), uruchamiane, gdy zostanie zakończone i klawiatury jest odrzucane. Użytkownik wskazuje ukończenie w sposób specyficzny dla platformy

[ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) w przykładzie pokazano te dwa zdarzenia.

### <a name="the-editor-difference"></a>Różnica edytora

Wielowierszowy [ `Editor` ](xref:Xamarin.Forms.Editor) definiuje takie same `Text` i `Font` właściwości `Entry` , ale nie inne właściwości. `Editor` definiuje również te same właściwości dwóch jako `Entry`.

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) to program pobieranie informacje o dowolnych, która zapisuje i przywraca zawartość `Editor`.

### <a name="the-searchbar"></a>SearchBar

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar) Nie pochodzi od `InputView`, więc nie ma `Keyboard` właściwości. Ale niesie ze sobą wszystkich `Text`, `Font`, i `Placeholder` właściwości, `Entry` definiuje. Ponadto `SearchBar` definiuje trzy dodatkowe właściwości:

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) tego typu `Color`
- [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) typu [ `ICommand` ](xref:System.Windows.Input.ICommand) do użytku z powiązań danych i MVVM
- [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) typu `Object`, do użytku z programem `SearchCommand`

Specyficzne dla platformy Anuluj wymazywanie przycisku tekstu. `SearchBar` Również znajduje się przycisk wyszukiwania specyficznego dla platformy. Naciskając jeden z tych przycisków podnosi jednego z dwóch zdarzeń, `SearchBar` definiuje:

- [`TextChanged`](xref:Xamarin.Forms.SearchBar.TextChanged) wraz z [ `TextChangedEventArgs` ](xref:Xamarin.Forms.TextChangedEventArgs) obiektu
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

[ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) w przykładzie pokazano `SearchBar`.

## <a name="date-and-time-selection"></a>Wybór daty i godziny

[ `DatePicker` ](xref:Xamarin.Forms.DatePicker) i [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) widoków zaimplementować kontrolę specyficzne dla platformy, które umożliwiają użytkownikowi na określenie daty lub godziny.

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](xref:Xamarin.Forms.DatePicker) definiuje cztery właściwości:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) typu `DateTime`, zainicjowana na 1 stycznia 1900 roku.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) typu `DateTime`, zainicjowana na 31 grudnia 2100
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) typu `DateTime`, zainicjowana na `DateTime.Today`
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) typu `string`, .NET, formatowanie ciągu, inicjowany do "d", wzorzec daty krótkiej skutkuje wyświetlania daty, takich jak "7/20/1969" w Stanach Zjednoczonych.

Możesz ustawić `DateTime` właściwości w XAML, przedstawiając właściwości jako elementy właściwości i używając daty krótkiej niezmiennej kultury formatu ("7/20/1969").   

[ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) przykładowe oblicza liczbę dni między dwiema datami wybrane przez użytkownika.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (lub jest on TimeSpanPicker?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) definiuje dwie właściwości i zdarzeń:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) Typ jest `TimeSpan` zamiast `DateTime`, wskazujący czas, jaki upłynął od północy
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) typu `string`, .NET, formatowanie ciągu, inicjowany do "t", wzorzec godziny krótkiej, co spowoduje wyświetlanie godziny, takich jak "1:45 PM" w Stanach Zjednoczonych.

[ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) program pokazuje sposób użycia `TimePicker` Aby określić godzinę dla czasomierza. Program tylko wtedy, gdy zachowanie na pierwszym planie.

**SetTimer** także pokazano sposób użycia [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) metody `Page` Aby wyświetlić okno dialogowe alertu.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 15 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Przykłady działu 15](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Wpis](~/xamarin-forms/user-interface/text/entry.md)
- [Edytor](~/xamarin-forms/user-interface/text/editor.md)
