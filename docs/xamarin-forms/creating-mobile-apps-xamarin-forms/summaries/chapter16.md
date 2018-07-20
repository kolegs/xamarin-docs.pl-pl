---
title: Podsumowanie rozdziałów 16. Powiązanie danych
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdział 16. Powiązanie danych'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: 083cb4ed57df989a55a26394cbf8440d53a9e769
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156668"
---
# <a name="summary-of-chapter-16-data-binding"></a>Podsumowanie rozdziałów 16. Powiązanie danych

> [!NOTE] 
> Uwagi na tej stronie wskazać obszary, w którym Xamarin.Forms podzielił z materiału znajdujące się w książce.

Programistów często stawia pisanie programów obsługi zdarzeń, które wykrywają, gdy zmieniono właściwość jednego obiektu i używać, aby zmienić wartość właściwości w innym obiekcie. Proces ten można zautomatyzować za pomocą techniki *powiązanie danych*. Powiązania danych zazwyczaj są zdefiniowane w XAML i stają się częścią definicji interfejsu użytkownika.

Bardzo często te powiązania danych obiektów interfejsu użytkownika połączyć się z danych źródłowych. Jest to technika, która jest bardziej przedstawione w [ **rozdział 18. MVVM**](chapter18.md). Jednak powiązania danych można również łączyć z co najmniej dwa elementy interfejsu użytkownika. Większość przykładów wczesnego wiązania danych w tym rozdziale zademonstrować tę technikę.

## <a name="binding-basics"></a>Podstawowe powiązanie

Kilka właściwości, metod i klas są zaangażowane w powiązaniu danych:

- [ `Binding` ](xref:Xamarin.Forms.Binding) Klasa pochodzi od [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) i hermetyzuje wiele właściwości powiązania danych
- [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Właściwość jest zdefiniowana przez [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) klasy
- [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) Metoda jest również definiowany przez [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) klasy
- [ `BindableObjectExtensions` ](xref:Xamarin.Forms.BindableObjectExtensions) Klasa definiuje trzy dodatkowe `SetBinding` metody

Następujące dwie klasy obsługę rozszerzeń struktury znaczników XAML powiązania:

- [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) obsługuje `Binding` — rozszerzenie znaczników
- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) obsługuje `x:Reference` — rozszerzenie znaczników

Dwa interfejsy są zaangażowane w powiązaniu danych:

- [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) w `System.ComponentModel` przestrzeń nazw jest implementowania powiadomienie, gdy zmiany właściwości
- [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) Służy do definiowania małych klas, które konwertują wartości z jednego typu na inny powiązania danych

Powiązanie danych łączy dwie właściwości tego samego obiektu lub (najczęściej) dwóch różnych obiektów. Te dwie właściwości są nazywane *źródła* i *docelowej*. Ogólnie rzecz biorąc zmiany we właściwości źródłowej powoduje, że zmiany w właściwość target, ale czasami kierunek została odwrócona. Niezależnie od tego:

- *docelowej* objęta właściwości [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)
- *źródła* właściwości zwykle jest elementem członkowskim klasy, która implementuje [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged)

Klasa, która implementuje `INotifyPropertyChanged` generowane [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged) zdarzenie, kiedy zmieni wartość. `BindableObject` implementuje `INotifyPropertyChanged` i automatycznie uruchamia `PropertyChanged` zdarzenie, gdy właściwość objęta `BindableProperty` zmiany wartości, ale można napisać własnych klas które implementują `INotifyPropertyChanged` bez pochodząca od `BindableObject`.

## <a name="code-and-xaml"></a>Kod i XAML

[ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) przykład pokazuje, jak ustawić powiązanie danych w kodzie:

- Obiekt źródłowy ma `Value` właściwość `Slider`
- Element docelowy jest `Opacity` właściwość `Label`

Dwa obiekty są połączone, ustawiając `BindingContext` z `Label` obiekt `Slider` obiektu. Dwie właściwości są połączone, wywołując [ `SetBinding` ](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) metody rozszerzenia `Label` odwołuje się do `OpacityProperty` właściwości możliwej do wiązania i `Value` właściwość `Slider` wyrażonej w postaci ciąg.

Manipulowanie `Slider` powoduje `Label` do zastosowania blaknięcia i widoku.

[ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) jest ten sam program przy użyciu powiązania danych w XAML. `BindingContext` z `Label` ustawiono `x:Reference` odwołanie do rozszerzenia znaczników `Slider`i `Opacity` właściwość `Label` jest ustawiona na `Binding` — rozszerzenie znaczników przy użyciu jego [ `Path` ](xref:Xamarin.Forms.Binding.Path) odwołuje się do właściwości `Value` właściwość `Slider`.

## <a name="source-and-bindingcontext"></a>Źródła i elementu BindingContext

[ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) przykład pokazuje alternatywne podejście w kodzie. A `Binding` obiekt jest tworzony przez ustawienie [ `Source` ](xref:Xamarin.Forms.Binding.Source) właściwości `Slider` obiektu i [ `Path` ](xref:Xamarin.Forms.Binding.Path) właściwość "Value". [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) Metody `BindableObject` jest następnie wywoływana na `Label` obiektu.

[ `Binding` Konstruktor](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) może być również używana do definiowania `Binding` obiektu.

[ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) pokazano techniki porównywalne w XAML. `Opacity` Właściwość `Label` ustawiono `Binding` — rozszerzenie znaczników za pomocą [ `Path` ](xref:Xamarin.Forms.Binding.Path) równa `Value` właściwości i [ `Source` ](xref:Xamarin.Forms.Binding.Source) równa osadzony `x:Reference` — rozszerzenie znaczników.

Podsumowując istnieją dwa sposoby, aby odwołać się do obiektu źródłowego powiązania:

- Za pomocą `BindingContext` właściwości elementu docelowego
- Za pomocą `Source` właściwość `Binding` samego obiektu

Jeśli są określone oba, drugi ma pierwszeństwo. Zaletą `BindingContext` jest, że jest propagowana do drzewa wizualnego. Jest to *bardzo* pomocne w przypadku wielu właściwości obiektu docelowego są powiązane z tym samym obiektem źródłowym.

[ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) program pokazano tej techniki, za pomocą [ `WebView` ](xref:Xamarin.Forms.WebView) elementu. Dwa `Button` elementy do nawigowania wstecz i będzie przekazywać dziedziczą `BindingContext` od jego obiektu nadrzędnego, który odwołuje się do `WebView`. `IsEnabled` Właściwości dwóch przycisków miał proste `Binding` rozszerzenia znaczników, których platformą docelową przycisku `IsEnabled` właściwości na podstawie ustawień z [ `CanGoBack` ](xref:Xamarin.Forms.WebView.CanGoBack) i [ `CanGoForward` ](xref:Xamarin.Forms.WebView.CanGoForward) właściwości tylko do odczytu `WebView`.

## <a name="the-binding-mode"></a>Tryb powiązania

Ustaw [ `Mode` ](xref:Xamarin.Forms.BindingBase.Mode) właściwość `Binding` do elementu członkowskiego [ `BindingMode` ](xref:Xamarin.Forms.BindingMode) wyliczenia:

- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) tak, aby zmiany właściwości source wpływają na obiekt docelowy
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) tak, aby zmiany właściwości docelowej wpływają na źródle
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) tak, aby zmiany w elemencie źródłowym i docelowym mają wpływ na siebie nawzajem
- [`Default`](xref:Xamarin.Forms.BindingMode.Default) Aby użyć [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) określony, gdy element docelowy `BindableProperty` został utworzony. Jeśli żaden nie został określony, wartością domyślną jest `OneWay` normalne możliwej do wiązania, właściwości i `OneWayToSource` dla właściwości może być powiązana tylko do odczytu.

> [!NOTE]
> `BindingMode` Wyliczenie teraz obejmuje również `OnTime` stosowania powiązanie, tylko wtedy, gdy zmienia kontekst powiązania, a nie w momencie zmiany właściwości source.

Właściwości, które mogą być celami powiązań danych w scenariuszach MVVM zazwyczaj mają `DefaultBindingMode` z `TwoWay`. Są to:

- `Value` Właściwość `Slider` i `Stepper`
- `IsToggled` Właściwość `Switch`
- `Text` Właściwość `Entry`, `Editor`, i `SearchBar`
- `Date` Właściwość `DatePicker`
- `Time` Właściwość `TimePicker`

[ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) w przykładzie pokazano tryby cztery powiązania przy użyciu powiązania danych, których obiektem docelowym są `FontSize` właściwość `Label` i źródłem jest `Value` Właściwość `Slider`. Dzięki temu każda `Slider` do kontrolowania rozmiaru czcionki odpowiadającego `Label`. Ale `Slider` elementy nie są inicjowane, ponieważ `DefaultBindingMode` z `FontSize` właściwość `OneWay`.

[ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) przykładowe powiązania ustawia `Value` właściwość `Slider` odwołuje się do `FontSize` właściwości każdego `Label`. Wydaje się być wcześniejszymi wersjami, ale działa lepiej initialzing `Slider` elementów ponieważ `Value` właściwość `Slider` ma `DefaultBindingMode` z `TwoWay`.

[![Potrójna zrzut ekranu przedstawiający odwrotnego powiązania](images/ch16fg06-small.png "odwrotnego powiązania")](images/ch16fg06-large.png#lightbox "odwrotnego powiązania")

To jest odpowiednikiem sposób powiązania są zdefiniowane w MVVM i użyjesz tego typu powiązania często.

## <a name="string-formatting"></a>Formatowanie ciągu

Gdy właściwość docelowa jest typu `string`, możesz użyć [ `StringFormat` ](xref:Xamarin.Forms.BindingBase.StringFormat) zdefiniowaną przez właściwość `BindingBase` do przekonwertowania na źródła `string`. Ustaw `StringFormat` właściwość .NET formatowanie ciągu, którego używasz za pomocą statycznych [ `String.Format` ](xref:System.String.Format(System.String,System.Object)) format wyświetlania obiektu. Korzystając z tego ciągu formatowania w ramach rozszerzenia znaczników, należy go ująć w znaki pojedynczego cudzysłowu, nawiasy klamrowe nie być mylone z rozszerzeniem znacznika osadzonego.

[ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) przykład pokazuje, jak używać `StringFormat` w XAML.

[ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) przykład demonstruje wyświetlanie rozmiar strony za pomocą powiązania z `Width` i `Height` właściwości `ContentPage`.

## <a name="why-is-it-called-path"></a>Dlaczego jest on nazywany "Path"

[ `Path` ](xref:Xamarin.Forms.Binding.Path) Właściwość `Binding` więc jest wywoływana, ponieważ może być szereg właściwości i indeksatorów, oddzielone kropkami. [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) przykład pokazuje kilka przykładów.

## <a name="binding-value-converters"></a>Konwertery wartości powiązania

Źródłowe i docelowe właściwości powiązania w przypadku różnych typów, można wykonywać konwersje między typów, przy użyciu konwerter powiązania. Jest to klasa, która implementuje [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) interfejs i zawiera dwie metody: [ `Convert` ](xref:Xamarin.Forms.IValueConverter.Convert(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) przekonwertować źródła do docelowego, a [ `ConvertBack` ](xref:Xamarin.Forms.IValueConverter.ConvertBack(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) Aby przekonwertować obiektu docelowego do źródła.

[ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki znajduje się przykład konwertowania `int` do `bool`. Jest dowodzą [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) próbki, która umożliwia jedynie `Button` Jeśli wpisywać co najmniej jeden znak `Entry`.

[ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) Klasy konwertuje `bool` do `string` i definiuje dwie właściwości, aby określić, jaki tekst powinien nastąpić powrót do `false` i `true` wartości.
[ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) Jest podobny. [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) przykładzie pokazano użycie tych dwóch konwerterów do wyświetlania tekstu w różnych kolorach, na podstawie `Switch` ustawienie.

Ogólny [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) można zastąpić `BoolToStringConverter` i `BoolToColorConverter` i służyć jako uogólniony `bool`-do-konwerter dowolnego typu.

## <a name="bindings-and-custom-views"></a>Powiązania i widoków niestandardowych

Można uprościć kontrolek niestandardowych przy użyciu powiązania danych. [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) Plik kodu definiuje `Text`, `TextColor`, `FontSize`, `FontAttributes`, i `IsChecked` właściwości, ale nie logikę na wszystkich wizualizacjach formantu.
Zamiast tego [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) plik zawiera wszystkie znaczniki dla formantu wizualizacji za pomocą powiązania danych na `Label` elementów na podstawie właściwości zdefiniowane w pliku związanym z kodem.

[ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) w przykładzie pokazano `NewCheckBox` kontrolki niestandardowej.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 16 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [Przykłady rozdział 16](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
- [Powiązanie danych](~/xamarin-forms/app-fundamentals/data-binding/index.md)
