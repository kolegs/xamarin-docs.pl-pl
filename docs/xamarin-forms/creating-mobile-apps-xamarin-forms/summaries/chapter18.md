---
title: "Podsumowanie rozdział 18. MVVM"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: cadab2432d0b6c29ead9cde1f4220bb64e1e1886
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-18-mvvm"></a>Podsumowanie rozdział 18. MVVM

Jednym z najlepszych sposobów projektowania aplikacji jest oddzielając interfejsu użytkownika z odpowiedniego kodu, jest czasem nazywany *logiki biznesowej*. Istnieje kilka technik, ale ten, który został zoptymalizowany pod kątem dla środowisk opartych na języku XAML jest określane jako Model-View-ViewModel lub MVVM.

## <a name="mvvm-interrelationships"></a>Wzajemne relacje MVVM

Aplikacja MVVM ma trzy warstwy:

- Model zawiera danych, czasami za pomocą plików lub uzyskuje dostęp do sieci web
- Widok jest użytkownika interfejsu lub prezentacji warstwie, zwykle implementowany w języku XAML
- ViewModel łączy modelu i widoku

Model jest ignorujących z ViewModel i ViewModel jest ignorujących widoku. Te trzy warstwy zazwyczaj połączyć ze sobą przy użyciu następujących mechanizmów:

![Widok, ViewModel oraz widok](images/ch18fg03.png "MVVM")

Dużo mniejsze programy (i jeszcze większym z nich), często modelu nie istnieje lub jego działanie jest zintegrowany z ViewModel.

## <a name="viewmodels-and-data-binding"></a>ViewModels i powiązanie danych

Nawiązanie powiązania danych, ViewModel musi umożliwiać powiadamiania widoku, gdy zmieniono właściwość ViewModel. ViewModel robi to zaimplementowanie [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) interfejsu w `System.ComponentModel` przestrzeni nazw. Jest to część .NET zamiast platformy Xamarin.Forms. (Zazwyczaj ViewModels próbować zachować niezależność od platformy.)

`INotifyPropertyChanged` Interfejsu deklaruje pojedynczego zdarzenia o nazwie [ `PropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) który wskazuje właściwość, która została zmieniona.

### <a name="a-viewmodel-clock"></a>Zegar ViewModel

[ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) Biblioteka definiuje właściwość typu `DateTime` czy zmiany na podstawie przez czasomierz. Implementuje klasy `INotifyPropertyChanged` i jest uruchamiany `PropertyChanged` zdarzenia przy każdym `DateTime` zmiany właściwości.

[ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) przykładowe tworzy wystąpienie tego ViewModel i używa powiązania danych do ViewModel do wyświetlania informacji zaktualizowane daty i godziny.

### <a name="interactive-properties-in-a-viewmodel"></a>Interaktywne właściwości w ViewModel

Właściwości w ViewModel może być większej liczby interaktywnych, jak pokazano w [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) klasy, która jest częścią programu [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) próbki. Powiązania danych Podaj multiplicand i mnożnik wartości z dwóch `Slider` elementów i wyświetlić produktu z `Label`. Jednak umożliwia rozległych zmian do tego interfejsu użytkownika w języku XAML bez zmian wynikające z niego ViewModel lub pliku CodeBehind.

### <a name="a-color-viewmodel"></a>ViewModel kolorów

[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) modele kolorów RGB i HSL integruje się biblioteki. Zostało to przedstawione w [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) próbki:

[![Potrójna zrzut ekranu przedstawiający TK](images/ch18fg08-small.png "HSL modelu")](images/ch18fg08-large.png#lightbox "modelu HSL")

### <a name="streamlining-the-viewmodel"></a>Usprawnienie ViewModel

Kod w ViewModels może usprawnić, definiując `OnPropertyChanged` przy użyciu metody [ `CallerMemberName` ](https://developer.xamarin.com/api/type/System.Runtime.CompilerServices.CallerMemberNameAttribute/) atrybut, który automatycznie pobiera wywoływania nazwa właściwości. [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) biblioteki działanie i udostępnia klasę podstawową dla ViewModels.

## <a name="the-command-interface"></a>Interfejs polecenia

MVVM współpracuje z powiązania danych i powiązania danych Praca z właściwościami, więc MVVM wydaje się być niewystarczające, jeśli chodzi o Obsługa `Clicked` zdarzenie `Button` lub `Tapped` zdarzenie `TapGestureRecognizer`. Aby umożliwić ViewModels do obsługi tych zdarzeń, obsługuje platformy Xamarin.Forms *interfejsu polecenia*.

Interfejs polecenia w sytuacji, w `Button` z dwóch właściwości publiczne:

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) typu [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) (zdefiniowany w `System.Windows.Input` przestrzeni nazw)
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) typu `Object`

Aby obsługiwać interfejs polecenia, ViewModel musi definiować właściwość typu `ICommand` który jest następnie danymi powiązanymi `Command` właściwość `Button`. `ICommand` Interfejsu deklaruje dwie metody i jedno zdarzenie:

- [ `Execute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.Execute/p/System.Object/) Metody z argumentem typu `object`
- A [ `CanExecute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.CanExecute/p/System.Object/) metody z argumentem typu `object` zwracającą `bool`
- A [ `CanExecuteChanged` ](https://developer.xamarin.com/api/event/System.Windows.Input.ICommand.CanExecuteChanged/) zdarzeń

Wewnętrznie ViewModel ustawia właściwości każdego typu `ICommand` do wystąpienia klasy, która implementuje `ICommand` interfejsu. Za pomocą wiązania danych `Button` początkowo wywołuje `CanExecute` metody i wyłącza się, jeśli metoda zwraca `false`. Ustawia również funkcję obsługi `CanExecuteChanged` zdarzeń i wywołania `CanExecute` zawsze, gdy zdarzenie jest wywoływane. Jeśli `Button` jest włączona, wywołuje `Execute` metody zawsze, gdy `Button` zostanie kliknięty.

Mogą być niektórych ViewModels, który przed powstaniem platformy Xamarin.Forms, a te mogą już obsługiwać interfejs polecenia. Dla nowego ViewModels, które ma być używany tylko z platformy Xamarin.Forms, dostarcza platformy Xamarin.Forms [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) klasy i [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) klasy, które implementują `ICommand` interfejsu. Typ ogólny jest typem argumentu `Execute` i `CanExecute` metody.

### <a name="simple-method-executions"></a>Prosta metoda wykonania

[ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) przykładowych pokazano, jak używać polecenia interfejsu w ViewModel. [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) Klasa definiuje dwie właściwości typu `ICommand` , a także definiuje dwie właściwości prywatne, które przechodzą do najprostszą [ `Command` konstruktora](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/). Program zawiera powiązań danych z tej ViewModel do `Command` właściwości dwóch `Button` elementów.

`Button` Elementy można łatwo zastąpić `TapGestureRecognizer` obiektów w języku XAML bez zmian kodu.

### <a name="a-calculator-almost"></a>Kalkulator, prawie

[ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) przykładowe sprawia, że użycie zarówno `Execute` i `CanExecute` metody `ICommand`. Używa [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) biblioteki. ViewModel zawiera sześć właściwości typu `ICommand`. Są one inicjowane z [ `Command` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/) i [ `Command` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) z `Command` i [ `Command<T>` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/) z `Command<T>`. Klawiszy numerycznych Dodawanie maszyny jest powiązana z właściwością jest inicjowany z `Command<T>`, a `string` argument `Execute` i `CanExecute` identyfikuje określonego klucza.

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModels i cyklem życia aplikacji

`AdderViewModel` Używane w **AddingMachine** próbki definiuje również dwie metody o nazwie `SaveState` i `RestoreState`. Te metody są wywoływane z aplikacji podczas jej przechodzi w stan uśpienia i ponownie uruchamiania.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 18 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Przykłady rozdział 18](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
