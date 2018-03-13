---
title: "Podsumowanie rozdział 7. XAML i kodem"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 1104f7576cabfed9988154f3b6a8beb429136fb3
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Podsumowanie rozdział 7. XAML i kodem

Platformy Xamarin.Forms obsługuje języku Extensible Application Markup Language znaczników opartych na języku XML i XAML (Wymowa "zammel"). XAML stanowi alternatywę dla C# podczas definiowania układu interfejsu użytkownika aplikacji platformy Xamarin.Forms i w celu zdefiniowania powiązania między elementy interfejsu użytkownika i podstawowego danych.

## <a name="properties-and-attributes"></a>Właściwości i atrybutów

Platformy Xamarin.Forms klas i struktur stają się elementów XML w języku XAML, a właściwości tych klas i struktur stają się atrybutów XML. Z myślą o uruchamianiu w języku XAML, klasy zwykle musi mieć publicznego konstruktora bez parametrów. Właściwości w języku XAML musi mieć publiczny `set` metody dostępu.

Właściwości danych podstawowych (`string`, `double`, `bool`itd), analizator XAML korzysta ze standardu `TryParse` metod do konwertowania ustawień atrybutów do tych typów. Analizator XAML może obsługiwać także łatwo Typy wyliczeniowe i można połączyć elementy członkowskie wyliczenia, jeśli typ wyliczeniowy jest oznaczone stanem `Flags` atrybutu.

Ułatwiających analizator XAML może zawierać bardziej złożone typy (lub właściwości tych typów) [ `TypeConverterAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/) identyfikującym rozszerzenie klasy, która jest pochodną [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) który obsługuje konwersję z wartości ciągu do tych typów. Na przykład [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) Konwertuje kolor nazwy i ciągów, takich jak "#rrggbb", do `Color` wartości.

## <a name="property-element-syntax"></a>Składnia elementu właściwości

W języku XAML klasy i obiekty utworzone na podstawie ich są wyrażane jako elementy XML. Są one nazywane *obiekt elementy*. Większość właściwości te obiekty są wyrażane jako atrybuty XML. Są one nazywane *atrybuty właściwości*.

Czasami właściwość musi mieć ustawioną obiektu, który nie może być wyrażona jako prosty ciąg znaków. W takim przypadku XAML obsługuje tag o nazwie *elementu właściwości* składający się z nazwy klasy i nazwę właściwości oddzielone kropką. Element obiektu następnie może występować w pary znaczników elementu właściwości.

## <a name="adding-a-xaml-page-to-your-project"></a>Dodawanie strony XAML do projektu

Biblioteka klas przenośnych platformy Xamarin.Forms może zawierać strony XAML po pierwszym utworzeniu lub strony XAML można dodać do istniejącego projektu. W oknie dialogowym, aby dodać nowy element, wybierz element, który odwołuje się do strony XAML, lub `ContentPage` i języka XAML. (Nie `ContentView`.)

Zostaną utworzone dwa pliki: plik XAML z .xaml rozszerzenia nazwy pliku, a plik C# z rozszerzeniem. xaml.cs. Plik języka C# jest często określany jako *CodeBehind* pliku XAML. Plik CodeBehind jest definicji częściowej klasy, która jest pochodną `ContentPage`. W czasie kompilacji jest analizowana XAML i innej definicji częściowej klasy jest generowany dla tej samej klasy. Ta klasa wygenerowanego zawiera metodę o nazwie `InitializeComponent` która jest wywoływana z konstruktora pliku CodeBehind.

W czasie wykonywania, po zakończeniu `InitializeComponent` wywołać, wszystkie elementy pliku XAML zostały utworzone i zainicjowane, tak jakby były one utworzone w kodzie języka C#.

Element główny w pliku XAML jest `ContentPage`. Tag główny zawiera co najmniej dwa deklaracje przestrzeni nazw XML, jeden dla elementów platformy Xamarin.Forms i innych definiowanie `x` prefiks dla elementów i atrybutów wewnętrzne wszystkich implementacji XAML. Zawiera także tag główny `x:Class` atrybut, który wskazuje przestrzeń nazw i nazwę klasy, która jest pochodną `ContentPage`. To jest zgodna z nazwą przestrzeni nazw i klasy w pliku CodeBehind.

Dowodzą kombinację XAML i kodem [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) próbki.

## <a name="the-xaml-compiler"></a>Kompilator języka XAML

Platformy Xamarin.Forms ma kompilatora XAML, ale jej użycie jest opcjonalne, oparte na korzystanie z [ `XamlCompilationAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/). Jeśli XAML nie jest skompilowany, XAML jest analizowana w czasie kompilacji, a plik XAML jest osadzony w PCL, w którym również jest analizowana w czasie wykonywania. Jeśli ma być kompilowana XAML, proces kompilacji konwertuje XAML w formie binarnej i przetwarzania środowiska uruchomieniowego jest bardziej wydajny.

## <a name="platform-specificity-in-the-xaml-file"></a>Szczegółowością platformy w pliku XAML

W języku XAML [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) klasa może być używana do wybrania znaczników zależny od platformy. To jest klasą szablonową i musi być utworzone z `x:TypeArguments` atrybut, który jest zgodny z typem docelowym. [ `OnIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnIdiom%3CT%3E/) Klasy jest znacznie mniej często podobne, lecz używane.

Korzystanie z `OnPlatform` została zmieniona od czasu opublikowania książce. Pierwotnie została użyta w połączeniu z właściwości o nazwie `iOS`, `Android`, i `WinPhone`. Obecnie jest używany z podrzędnych [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) obiektów. Ustaw [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) właściwości na ciąg zgodny z publicznego `const` pola [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) klasy. Ustaw [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Value/) właściwości na wartość, które są zgodne z `x:TypeArguments` atrybutu `OnPlatform` znacznika.

`OnPlatform` przedstawiono w [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) próbki, więc wywołuje się, ponieważ zawiera bloki niemal identyczne XAML. Istnienie tego znacznika występują powtarzające sugeruje, że techniki powinny być dostępne dla jej zmniejszenia.

## <a name="the-content-property-attributes"></a>Atrybuty właściwości zawartości

Niektóre elementy właściwości występować bardzo często, takich jak `<ContentPage.Content>` tag główny element `ContentPage`, lub `<StackLayout.Children>` tag, który umieszcza elementów podrzędnych `StackLayout`.

Każda klasa może zidentyfikować jedną właściwość z [ `ContentPropertyAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPropertyAttribute/) w klasie. Dla tej właściwości znaczniki elementu właściwości nie są wymagane. `ContentPage` Definiuje właściwości zawartości jako `Content`, i `Layout<T>` (klasy, z którego `StackLayout` pochodzi) Definiuje właściwości zawartości jako `Children`. Znaczniki elementu właściwości nie są wymagane.

Właściwość elementu `Label` jest `Text`.

## <a name="formatted-text"></a>Tekst sformatowany

[ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) próbka zawiera kilka przykładów ustawienie `Text` i `FormattedText` właściwości `Label`. W języku XAML `Span` obiekty są wyświetlane jako elementy podrzędne `FormattedString` obiektu.

 Gdy ma wartość Wielowierszowy ciąg `Text` właściwości, znaki końca wiersza są konwertowane na znaki spacji, ale znaki końca wiersza są zachowywane podczas wielowierszowy ciąg, który jest wyświetlany jako zawartość `Label` lub `Label.Text` tagów:

 [![Potrójna zrzut ekranu przedstawiający zmiany tekstu udostępnianie](images/ch07fg03-small.png "odmiany tekst sformatowany")](images/ch07fg03-large.png#lightbox "odmiany tekst sformatowany")



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 7 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Przykłady rozdział 7](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Przykładowe działu 7 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
