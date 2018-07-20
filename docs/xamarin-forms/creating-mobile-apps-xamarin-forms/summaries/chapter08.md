---
title: Podsumowanie rozdziałów 8. Kod i XAML w zgodzie
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziału 8. Kod i XAML w zgodzie'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 1aa5226e1e6867f77eea4d7679650e8d62f5b981
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156993"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Podsumowanie rozdziałów 8. Kod i XAML w zgodzie

W tym rozdziale przedstawiono XAML głębiej, a zwłaszcza kodem i XAML interakcje.

## <a name="passing-arguments"></a>Przekazywanie argumentów

W przypadku ogólnych klasa w XAML musi mieć publicznego konstruktora bez parametrów; Wynikowy obiekt jest inicjowany za pomocą ustawień właściwości. Istnieją jednak dwa inne sposoby, że obiekty można utworzyć wystąpienia, a zainicjowany.

Chociaż te techniki ogólnego przeznaczenia, przede wszystkim są one używane w połączeniu z modelem MVVM modeli widoków.

### <a name="constructors-with-arguments"></a>Konstruktory z argumentami

[ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) przykład pokazuje, jak używać `x:Arguments` tag, aby określić argumentów konstruktora. Te argumenty muszą być rozdzielone tagi elementów, wskazujący typ argumentu. Podstawowe typy danych .NET dostępne są następujące tagi:

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

### <a name="can-i-call-methods-from-xaml"></a>Z XAML może wywoływać metody?

[ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) przykład pokazuje, jak używać `x:FactoryMethod` elementu, aby określić metodę fabryka, która jest wywoływana w celu utworzenia obiektu. Metoda fabryki muszą być publiczne ale statyczne i należy go utworzyć obiektu typu, w którym jest zdefiniowany. (Na przykład [ `Color.FromRgb` ](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) metoda kwalifikuje się, ponieważ są publiczne ale statyczne i zwraca wartość typu `Color`.) Argumenty do metody fabryki są określone w `x:Arguments` tagów.

## <a name="the-xname-attribute"></a>X: Name — atrybut

`x:Name` Atrybut umożliwia obiekt, który został utworzony w XAML, aby nadać nazwę. Zasady te nazwy są takie same jak nazwy zmiennych języka C#. Następujące powrotu `InitializeComponent` wywołanie w konstruktorze, plik związany z kodem może odwoływać się do tych nazw, które umożliwiają dostęp do odpowiedniego elementu XAML. Nazwy są faktycznie konwertowane przez parser XAML do pól prywatnych w generowanej klasie częściowej.

[ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) przykład demonstruje użycie `x:Name` umożliwia pliku związanego z kodem zachować dwa `Label` elementy zdefiniowane w XAML aktualizowane przy użyciu bieżącej daty i godziny.

Nie można użyć tej samej nazwy dla wielu elementów na tej samej stronie. Jest to konkretnych problemów, jeśli używasz `OnPlatform` tworzenia równoległych o nazwie obiektów dla każdej platformy. [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) w przykładzie pokazano lepszy sposób, jak coś zrobić.

## <a name="custom-xaml-based-views"></a>Niestandardowe widoki oparte na XAML

Istnieje kilka sposobów, aby uniknąć powtarzania znaczników w XAML. Jedną typową techniką jest utworzenie nowej klasy oparte na XAML, która pochodzi od klasy [ `ContentView` ](xref:Xamarin.Forms.ContentView). Ta technika jest przedstawiona w [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) próbki. `ColorView` Klasa pochodzi od `ContentView` do wyświetlania określonego koloru i jego nazwy, podczas gdy `ColorViewListPage` klasa pochodzi od `ContentPage` w zwykły sposób i jawnie tworzy 17 wystąpień `ColorView`.

Uzyskiwanie dostępu do `ColorView` class w XAML wymaga innej deklaracji przestrzeni nazw XML, powszechnie o nazwie `local` klas w tym samym zestawie.

## <a name="events-and-handlers"></a>Zdarzenia i procedury obsługi

Zdarzenia mogą być przypisane do obsługi zdarzeń w XAML, ale sam program obsługi zdarzeń musi zostać wdrożone w pliku związanym z kodem. [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) pokazuje, jak utworzyć interfejs użytkownika klawiatury numerycznej w XAML i jak implementować `Clicked` obsługi w pliku związanym z kodem.

## <a name="tap-gestures"></a>Naciśnij pozycję gesty

Wszelkie `View` uzyskać wprowadzanie dotykowe i generowanie zdarzeń na podstawie te dane wejściowe obiektu. `View` Klasa definiuje [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) właściwość kolekcji, który może zawierać jedno lub więcej wystąpień klas, które wynikają z [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer).

[ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Generuje [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) zdarzenia. [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) program pokazuje, jak dołączyć `TapGestureRecognizer` obiekty do czterech `BoxView` elementy, aby utworzyć grę sztucznej:

[![Potrójna zrzut ekranu przedstawiający naciśnij małp](images/ch08fg07-small.png "gry imitacja")](images/ch08fg07-large.png#lightbox "imitacja gry")

Ale **MonkeyTap** program naprawdę potrzebuje dźwięku. (Zobacz [następny rozdział](chapter09.md).)

## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 8 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Przykłady rozdział 8](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Przykładowe rozdział 8 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
- [Przekazywanie argumentów w XAML](~/xamarin-forms/xaml/passing-arguments.md)
