---
title: "Podsumowanie rozdziale 16. Powiązanie danych"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 954d5d9e270db156f5ef2577706c667e05ab544c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-16-data-binding"></a>Podsumowanie rozdziale 16. Powiązanie danych

Programiści często znajdują się pisanie programów obsługi zdarzeń, które Wykryj, kiedy zmieniono właściwość jednego obiektu i używać, aby zmienić wartość właściwości w innym obiekcie. Ten proces można zautomatyzować z techniki *wiązania z danymi*. Powiązania danych są zazwyczaj definiowane w języku XAML i stać się częścią definicji interfejsu użytkownika.

Bardzo często tych powiązań danych obiektów interfejsu użytkownika połączyć się z danych. Jest to metoda, która jest bardziej przedstawione w [ **rozdział 18. MVVM**](chapter18.md). Jednak powiązania danych można też podłączyć co najmniej dwa elementy interfejsu użytkownika. Większość przykładów wczesne powiązania danych w tym rozdziale pokazują tej metody.

## <a name="binding-basics"></a>Podstawowe powiązanie

Powiązanie danych obejmuje kilka właściwości, metody i klasy:

- [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) Pochodną klasy [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) i hermetyzuje wiele właściwości powiązania danych
- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Właściwość jest zdefiniowana przez [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) — klasa
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Metody jest też definiowany przez [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) — klasa
- [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) Klasa definiuje trzy dodatkowe `SetBinding` metody

Następujące dwie klasy obsługę rozszerzeń znaczników XAML powiązania:

- [`BindingExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) obsługuje `Binding` — rozszerzenie znaczników
- [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) obsługuje `x:Reference` — rozszerzenie znaczników

Powiązanie danych obejmuje dwa interfejsy:

- [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) w `System.ComponentModel` przestrzeń nazw jest wykonywania powiadomienia, gdy zostanie zmieniona właściwość
- [`IValueConverter`](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) Służy do definiowania małych klasy, które konwertują wartości z jednego typu na inny powiązania danych

Powiązanie danych łączy dwie właściwości tego samego obiektu lub (częściej) dwóch różnych obiektów. Te dwie właściwości są określane jako *źródła* i *docelowej*. Ogólnie rzecz biorąc zmiana właściwości source powoduje zmianę w właściwość target, ale czasami kierunek jest odwrócony. Niezależnie od:

- *docelowej* właściwości musi być obsługiwana przez [`BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- *źródła* właściwość zazwyczaj jest elementem członkowskim klasy, która implementuje [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)

Klasa, która implementuje `INotifyPropertyChanged` uruchamiany [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/) zdarzenie, gdy zmienia się wartość właściwości. `BindableObject` implementuje `INotifyPropertyChanged` i jest automatycznie uruchamiany `PropertyChanged` zdarzenie, gdy właściwość jest obsługiwana przez `BindableProperty` zmiany wartości, ale można pisać własne klasy które implementują `INotifyPropertyChanged` bez pochodny `BindableObject`.

## <a name="code-and-xaml"></a>Kod i języka XAML

[ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) przykładowych pokazano, jak ustawić powiązanie danych w kodzie:

- Źródło jest `Value` właściwości `Slider`
- Element docelowy jest `Opacity` właściwości `Label`

Dwa obiekty są połączone przez ustawienie `BindingContext` z `Label` do obiektu `Slider` obiektu. Dwie właściwości są połączone przez wywołanie metody [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) — metoda rozszerzenia na `Label` odwołuje się do `OpacityProperty` właściwości możliwej do wiązania i `Value` właściwość `Slider` wyrażonej w postaci ciąg.

Manipulowanie `Slider` następnie powoduje `Label` do zanikania i widoku.

[ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) jest ten sam program dla powiązania danych w języku XAML. `BindingContext` z `Label` ustawiono `x:Reference` odwołania do rozszerzenia znacznika `Slider`i `Opacity` właściwość `Label` ustawiono `Binding` — rozszerzenie znaczników z jego [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) odwołuje się do właściwości `Value` właściwość `Slider`.

## <a name="source-and-bindingcontext"></a>Źródło i BindingContext

[ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) pokazano o innym podejściu w kodzie. A `Binding` obiekt jest tworzony przez ustawienie [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) właściwości `Slider` obiektu i [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) dla właściwości "Value". [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Metody `BindableObject` następnie wywołano `Label` obiektu.

[ `Binding` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) może być również używane do definiowania `Binding` obiektu.

[ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) pokazano porównywalne techniki w języku XAML. `Opacity` Właściwość `Label` ustawiono `Binding` — rozszerzenie znaczników z [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) ustawioną `Value` właściwości i [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) ustawioną osadzony `x:Reference` — rozszerzenie znaczników.

Podsumowując istnieją dwa sposoby odwołuje się do obiektu źródłowego powiązania:

- Za pomocą `BindingContext` właściwości elementu docelowego
- Za pomocą `Source` właściwość `Binding` samego obiektu

Jeśli określono oba, drugi ma pierwszeństwo. Zaletą `BindingContext` jest, że jest propagowana do drzewa wizualnego. Jest to *bardzo* przydatne, jeśli wiele właściwości obiektu docelowego jest powiązana z tego samego obiektu źródłowego.

[ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) program ilustruje ta technika z [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) elementu. Dwa `Button` elementy do nawigowania wstecz i do przodu dziedziczą `BindingContext` z ich nadrzędnego, który odwołuje się do `WebView`. `IsEnabled` Właściwości dwóch przycisków zmuszony proste `Binding` rozszerzenia znaczników, które odnoszą się do przycisku `IsEnabled` właściwości na podstawie ustawień z [ `CanGoBack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoBack/) i [ `CanGoForward` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoForward/) właściwości tylko do odczytu `WebView`.

## <a name="the-binding-mode"></a>Tryb wiązania

Ustaw [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) właściwość `Binding` do elementu członkowskiego [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) wyliczenie:

- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) tak, aby zmiany właściwości source wpływać na element docelowy
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) tak, aby zmiany właściwości target wpływać na źródło
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) tak, aby zmiany w źródłowych i docelowych wpływają na siebie
- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) Aby użyć [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) określony podczas docelowy `BindableProperty` został utworzony. Jeśli żaden nie został określony, wartością domyślną jest `OneWay` normalne można powiązać właściwości, oraz `OneWayToSource` dla właściwości tylko do odczytu.

Właściwości, które mogą być elementami docelowymi powiązania danych w scenariuszach MVVM zazwyczaj mają `DefaultBindingMode` z `TwoWay`. Są to:

- `Value` Właściwość `Slider` i `Stepper`
- `IsToggled` Właściwość `Switch`
- `Text` Właściwość `Entry`, `Editor`, i `SearchBar`
- `Date` Właściwość `DatePicker`
- `Time` Właściwość `TimePicker`

[ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) przykładzie pokazano tryby cztery powiązania z powiązaniem danych, gdy obiektem docelowym jest `FontSize` właściwość `Label` i źródło jest `Value` Właściwość `Slider`. Dzięki temu każda `Slider` kontrolować rozmiar czcionki odpowiadającego `Label`. Ale `Slider` elementy nie są inicjowane, ponieważ `DefaultBindingMode` z `FontSize` jest właściwość `OneWay`.

[ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) przykładowe ustawia powiązania `Value` właściwość `Slider` odwołuje się do `FontSize` właściwości każdego `Label`. Wydaje się być Wstecz, ale działa lepiej initialzing `Slider` elementów ponieważ `Value` właściwość `Slider` ma `DefaultBindingMode` z `TwoWay`.

[![Potrójna zrzut ekranu przedstawiający wstecznego powiązania](images/ch16fg06-small.png "wstecznego powiązania")](images/ch16fg06-large.png "wstecznego powiązania")

To jest odpowiednikiem jak powiązania są definiowane w MVVM i użyjesz tego typu powiązania często.

## <a name="string-formatting"></a>Ciąg formatowania

Gdy właściwość target jest typu `string`, można użyć [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) właściwości zdefiniowane przez `BindingBase` do przekonwertowania na źródła `string`. Ustaw `StringFormat` właściwości .NET ciąg, który ma zostać użyty z statycznych formatowania [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) format wyświetlania obiektu. Korzystając z tego ciągu formatowania w rozszerzeniu znaczników, należy ująć ją w pojedynczy cudzysłów, nawiasy klamrowe nie pomylone dla rozszerzenia znacznika osadzonego.

[ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) przykładzie pokazano sposób użycia `StringFormat` w języku XAML.

[ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) przykładzie pokazano wyświetlanie rozmiar strony z powiązaniami do `Width` i `Height` właściwości `ContentPage`.

## <a name="why-is-it-called-path"></a>Dlaczego jest on nazywany "Path"?

[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) Właściwość `Binding` jest nazywany tak, ponieważ może być szereg właściwości i indeksatorów, oddzielone kropkami. [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) pokazano kilka przykładów.

## <a name="binding-value-converters"></a>Konwertery wartości powiązania

Gdy powiązania właściwości źródłowa i docelowa są różne typy, można konwertować między typami za pomocą konwertera powiązania. Jest to klasa implementująca [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) interfejsu i zawiera dwie metody: [ `Convert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.Convert/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) przekonwertować źródła do obiektu docelowego i [ `ConvertBack` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.ConvertBack/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) Aby przekonwertować obiektu docelowego w źródle.

[ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka jest przykład konwertowanie `int` do `bool`. Jest dowodzą [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) próbki, dzięki czemu tylko `Button` Jeśli co najmniej jeden znak została wpisana do `Entry`.

[ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) Klasy konwertuje `bool` do `string` i definiuje dwie właściwości, aby określić, jaki tekst ma zostać zwrócony dla `false` i `true` wartości.
[ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) Przypomina. [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) przykładzie pokazano, przy użyciu tych dwóch konwerterów do wyświetlania tekstu w różnych kolorach, na podstawie `Switch` ustawienie.

Ogólnego [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) można zastąpić `BoolToStringConverter` i `BoolToColorConverter` i służyć jako uogólniony `bool`-do-obiektu konwerterem dowolnego typu.

## <a name="bindings-and-custom-views"></a>Powiązania i widoków niestandardowych

Można uprościć niestandardowych formantów przy użyciu powiązań danych. [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) Pliku kodu definiuje `Text`, `TextColor`, `FontSize`, `FontAttributes`, i `IsChecked` właściwości, ale ma nie logikę na wszystkich elementów wizualnych formantu.
Zamiast tego [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) plik zawiera kod znaczników dla elementów wizualnych formantu za pośrednictwem powiązania danych na `Label` elementy na podstawie właściwości zdefiniowane w pliku CodeBehind.

[ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) przykładzie pokazano `NewCheckBox` kontrolki niestandardowej.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 16 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [Przykłady rozdział 16](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
