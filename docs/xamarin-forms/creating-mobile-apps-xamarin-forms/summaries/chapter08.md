---
title: "Podsumowanie rozdziału 8. Kod i języka XAML w zgodzie"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: e95d6a20e828c92deb0e03fe1bcbcf18aac9e508
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Podsumowanie rozdziału 8. Kod i języka XAML w zgodzie

W tym rozdziale Eksploruje głębsze XAML, a szczególnie kodu i języka XAML interakcje.

## <a name="passing-arguments"></a>Przekazywanie argumentów

W przypadku ogólnych klasy w pliku XAML musi mieć publicznego konstruktora bez parametrów; Wynikowy obiekt został zainicjowany za pomocą ustawień właściwości. Istnieją dwa inne sposoby, że obiekty można utworzyć wystąpienia, a zainicjowany.

Chociaż te techniki ogólnego przeznaczenia, przede wszystkim są one używane w połączeniu z modelem MVVM wyświetlanie modeli.

### <a name="constructors-with-arguments"></a>Konstruktorów z argumentami

[ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) przykładzie pokazano sposób użycia `x:Arguments` tag, aby określić argumentów konstruktora. Argumenty muszą być rozdzielane według znaczników elementu wskazujący typ argumentu. Podstawowe typy danych .NET dostępne są następujące znaczniki:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

### <a name="can-i-call-methods-from-xaml"></a>Czy można wywołać metody z XAML

[ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) przykładzie pokazano sposób użycia `x:FactoryMethod` element, aby określić metodę fabryki, które jest wywoływane, aby utworzyć obiekt. Metoda fabryki musi być publiczna i statyczna i należy utworzyć obiekt typu, w którym jest zdefiniowany. (Na przykład [ `Color.FromRgb` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/)) metoda kwalifikuje się, ponieważ jest publiczna i statyczna i zwraca wartość typu `Color`.) Argumenty metody fabryki są określone w `x:Arguments` tagów.

## <a name="the-xname-attribute"></a>Atrybut x: Name

`x:Name` Atrybutu umożliwia w języku XAML, należy podać nazwę obiektu. Zasady te nazwy są takie same jak nazwy zmiennych C#. Następujące powrotu `InitializeComponent` wywołać w konstruktorze, plik CodeBehind mogą odwoływać się do tych nazw do odpowiadającego mu elementu XAML. Nazwy są faktycznie konwertowane przez parser XAML do pól prywatnych w wygenerowanej częściowej klasy.

[ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) przykładzie przedstawiono użycie `x:Name` aby plik CodeBehind zachować dwa `Label` elementy zdefiniowane w języku XAML, zaktualizowano o aktualnej daty i godziny.

Nie można użyć tej samej nazwie dla wielu elementów na tej samej stronie. Jest to konkretnych problemów, jeśli używasz `OnPlatform` utworzyć równoległy o nazwie obiektów dla każdej platformy. [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) przykładzie pokazano lepszym sposobem coś tak samo, jak to zrobić.

## <a name="custom-xaml-based-views"></a>Niestandardowe widoki opartych na języku XAML

Istnieje kilka sposobów, aby uniknąć powtarzania znaczników w XAML. Co typowe techniki jest utworzenie nowej klasy opartych na języku XAML, która jest pochodną [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Ta technika jest przedstawiona w [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) próbki. `ColorView` Pochodną klasy `ContentView` do wyświetlania określonego koloru i jego nazwa, podczas gdy `ColorViewListPage` pochodną klasy `ContentPage` normalnie i jawnie tworzy wystąpienia 17 `ColorView`.

Uzyskiwanie dostępu do `ColorView` klasy w języku XAML wymaga innej deklaracji przestrzeni nazw XML, często o nazwie `local` klas w tym samym zestawie.

## <a name="events-and-handlers"></a>Zdarzenia i obsługi

Zdarzenia można przypisać do obsługi zdarzeń w języku XAML, ale sam program obsługi zdarzeń musi być implementowana w pliku CodeBehind. [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) pokazano, jak tworzyć interfejs użytkownika klawiatury w kodzie XAML i implementowania `Clicked` obsługi w pliku CodeBehind.

## <a name="tap-gestures"></a>Wybierz gestów

Wszelkie `View` uzyskać wprowadzania dotykowego i generowanie zdarzeń z te dane wejściowe obiektu. `View` Klasa definiuje [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) właściwości kolekcji, która może zawierać jeden lub więcej wystąpień klas pochodzących od [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/).

[ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Generuje [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) zdarzenia. [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) program pokazano, jak dołączyć `TapGestureRecognizer` obiektów cztery `BoxView` elementów do tworzenia gier imitacje:

[![Potrójna zrzut ekranu przedstawiający naciśnij małp](images/ch08fg07-small.png "gry imitacja")](images/ch08fg07-large.png "imitacja gry")

Ale **MonkeyTap** program wymaga naprawdę dźwięku. (Zobacz [następnego rozdziału](chapter09.md).)



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 8 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Przykłady rozdział 8](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Przykładowe działu 8 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
