---
title: Podsumowanie rozdziałów 18. MVVM
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdział 18. MVVM'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b512b15ea40d963cd35379cf3162856d132e38dc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995265"
---
# <a name="summary-of-chapter-18-mvvm"></a>Podsumowanie rozdziałów 18. MVVM

Jest jednym z najlepszych sposobów architektury aplikacji, oddzielając interfejsu użytkownika z odpowiedniego kodu, który jest czasem nazywany *logikę biznesową*. Istnieje kilka technik, ale ten, który jest przeznaczony dla środowisk opartych na XAML jest znany jako Model-View-ViewModel lub MVVM.

## <a name="mvvm-interrelationships"></a>Wzajemne relacje MVVM

Aplikacja MVVM ma trzy warstwy:

- Model zapewnia danych bazowych, czasami za pomocą plików lub uzyskuje dostęp do sieci web
- Widok jest prezentacji lub interfejsu warstwę użytkownika, zazwyczaj implementowane w XAML
- ViewModel łączy Model i widoku

Model jest zakresu elementu ViewModel i zakresu widoku jest element ViewModel. Te trzy warstwy zazwyczaj łączyć się ze sobą przy użyciu następujących mechanizmów:

![View ViewModel i widoku](images/ch18fg03.png "MVVM")

W wielu mniejszych programów (a nawet większymi) często Model jest nieobecny lub jego działanie jest zintegrowana z ViewModel.

## <a name="viewmodels-and-data-binding"></a>Modele widoków i powiązania danych

Do powiązania danych, ViewModel musi umożliwiać powiadamiania widoku, gdy zmieniono właściwość ViewModel. Robi to poprzez implementację ViewModel [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) interfejsu w `System.ComponentModel` przestrzeni nazw. Jest to część platformy .NET, a nie zestawu narzędzi Xamarin.Forms. (Zazwyczaj modele widoków próby utrzymania niezależności platformy.)

`INotifyPropertyChanged` Interfejsu deklaruje pojedyncze zdarzenie o nazwie [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) oznacza to, właściwości, które uległy zmianie.

### <a name="a-viewmodel-clock"></a>Zegar ViewModel

[ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) Biblioteka definiuje właściwość typu `DateTime` , zmienia się na podstawie czasomierza. Klasa implementuje `INotifyPropertyChanged` i generowane `PropertyChanged` zdarzenie zawsze wtedy, gdy `DateTime` zmiany właściwości.

[ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) przykładowy tworzy wystąpienie tego ViewModel i wyświetla informacji zaktualizowane daty i godziny przy użyciu powiązania danych z ViewModel.

### <a name="interactive-properties-in-a-viewmodel"></a>Interaktywne właściwości ViewModel

Właściwości w ViewModel może być bardziej interaktywny, jak pokazano w [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) klasy, która jest częścią programu [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) próbki. Powiązania danych zapewniają którą mnożona jest mnożna i mnożnik wartości z dwóch `Slider` elementy i wyświetlić produktu, zapewniając `Label`. Jednak ułatwia rozległych zmian tego interfejsu użytkownika w XAML bez wynikającym zmian w pliku związanym z kodem lub ViewModel.

### <a name="a-color-viewmodel"></a>ViewModel kolorów

[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) biblioteki integruje modele kolorów RGB i HSL. Wspomniane [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) próbki:

[![Potrójna zrzut ekranu przedstawiający TK](images/ch18fg08-small.png "modelu kolorów HSL")](images/ch18fg08-large.png#lightbox "modelu kolorów HSL")

### <a name="streamlining-the-viewmodel"></a>Usprawnienie ViewModel

Kod w modele widoków, można uproszczenie, definiując `OnPropertyChanged` przy użyciu metody [ `CallerMemberName` ](xref:System.Runtime.CompilerServices.CallerMemberNameAttribute) atrybut, który automatycznie uzyskuje wywoływania nazwy właściwości. [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) biblioteki dzieje i udostępnia klasę bazową dla modele widoków.

## <a name="the-command-interface"></a>Interfejs polecenia

MVVM współpracuje z powiązań danych i powiązania danych Praca z właściwościami, więc MVVM wydaje się być niewystarczające, jeśli chodzi o obsługę `Clicked` zdarzenia `Button` lub `Tapped` zdarzenia `TapGestureRecognizer`. Aby umożliwić modele widoków do obsługi tych zdarzeń, obsługuje Xamarin.Forms *interfejsu polecenia*.

Interfejs polecenia w sytuacji, w `Button` za pomocą dwie właściwości publiczne:

- [`Command`](xref:Xamarin.Forms.Button.Command) typu [ `ICommand` ](xref:System.Windows.Input.ICommand) (zdefiniowane w `System.Windows.Input` przestrzeni nazw)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) tego typu `Object`

Aby zapewnić obsługę interfejsu polecenia, ViewModel należy zdefiniować właściwości typu `ICommand` oznacza to następnie dane powiązane z `Command` właściwość `Button`. `ICommand` Interfejsu deklaruje dwie metody i jedno zdarzenie:

- [ `Execute` ](xref:System.Windows.Input.ICommand.Execute(System.Object)) Metody za pomocą argumentu typu `object`
- A [ `CanExecute` ](xref:System.Windows.Input.ICommand.CanExecute(System.Object)) metody za pomocą argumentu typu `object` zwracającego `bool`
- A [ `CanExecuteChanged` ](xref:System.Windows.Input.ICommand.CanExecuteChanged) zdarzeń

Wewnętrznie ViewModel ustawia właściwości każdego typu `ICommand` do wystąpienia klasy, która implementuje `ICommand` interfejsu. Za pomocą powiązania danych `Button` początkowo wywołuje `CanExecute` metody i wyłączy, jeśli metoda zwraca `false`. Ustawia również funkcję obsługi `CanExecuteChanged` zdarzenia i wywołania `CanExecute` zawsze, gdy zdarzenie jest wyzwalane. Jeśli `Button` jest włączona, wywoływanych przez nią `Execute` metody przy każdym `Button` kliknięciu.

Może mieć niektóre modele widoków, które powstały wcześniej niż zestawu narzędzi Xamarin.Forms, a te mogą już obsługiwać interfejs polecenia. Dla nowe modele widoków, które ma być używana tylko z zestawu narzędzi Xamarin.Forms, dostarcza Xamarin.Forms [ `Command` ](xref:Xamarin.Forms.Command) klasy i [ `Command<T>` ](xref:Xamarin.Forms.Command`1) klas które implementują `ICommand` interfejsu. Typ ogólny jest typem argumentu `Execute` i `CanExecute` metody.

### <a name="simple-method-executions"></a>Prosty sposób wykonania

[ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) przykład pokazuje, jak używać interfejsu polecenia w ViewModel. [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) Klasa definiuje dwie właściwości typu `ICommand` i definiuje również dwie właściwości prywatne, które przekazuje do najprostszą [ `Command` Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action)). Program zawiera powiązania danych z tym ViewModel do `Command` właściwości dwóch `Button` elementów.

`Button` Elementy, można łatwo zastąpić `TapGestureRecognizer` obiektów w XAML nie wprowadzania zmian w kodzie.

### <a name="a-calculator-almost"></a>Kalkulator, prawie

[ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) przykładowy sprawia, że wykorzystanie zarówno `Execute` i `CanExecute` metody `ICommand`. Używa ona [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) biblioteki. ViewModel zawiera sześć właściwości typu `ICommand`. Są one inicjowane z [ `Command` Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action)) i [ `Command` Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) z `Command` i [ `Command<T>` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/) z `Command<T>`. Liczbowe kluczy maszynę dodającą są powiązane z właściwość, która jest inicjowany za pomocą `Command<T>`, a `string` argument `Execute` i `CanExecute` identyfikuje określonego klucza.

## <a name="viewmodels-and-the-application-lifecycle"></a>Modele widoków i cyklem życia aplikacji

`AdderViewModel` Używane w **AddingMachine** Przykładowa aplikacja definiuje również dwie metody o nazwie `SaveState` i `RestoreState`. Te metody są wywoływane z aplikacji, gdy przejdzie w tryb uśpienia i podczas jego uruchamiania ponownie.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 18 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Przykłady rozdział 18](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
