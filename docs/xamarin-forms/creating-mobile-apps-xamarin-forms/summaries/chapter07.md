---
title: Podsumowanie rozdział 7. XAML a kod
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdział 7. XAML a kod'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: d04012d5d2ea6a7617d5c7559aa3e1532dad15d1
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156915"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Podsumowanie rozdział 7. XAML a kod

> [!NOTE] 
> Uwagi na tej stronie wskazać obszary, w którym Xamarin.Forms podzielił z materiału znajdujące się w książce.

Zestaw narzędzi Xamarin.Forms obsługuje język znaczników oparty na formacie XML o nazwie Extensible Application Markup Language lub XAML (wymawiane "zammel"). XAML stanowi alternatywę dla języka C# określając układ interfejsu użytkownika aplikacji platformy Xamarin.Forms i w celu zdefiniowania powiązań między elementami interfejsu użytkownika i bazowych danych.

## <a name="properties-and-attributes"></a>Właściwości i atrybutów

Zestaw narzędzi Xamarin.Forms klas i struktur stają się elementy XML w XAML i właściwości tych klas i struktur stają się atrybutów XML. Z myślą o uruchamianiu w XAML, klasa ogólnie musi mieć publicznego konstruktora bez parametrów. Właściwości w XAML musi mieć publiczny `set` metod dostępu.

Właściwości typów podstawowych danych (`string`, `double`, `bool`, i tak dalej), analizatora XAML używa standardu `TryParse` metody konwersji ustawień atrybutów do tych typów. Analizator XAML może również łatwo obsługiwać typy wyliczeniowe i można połączyć elementów członkowskich wyliczenia, jeśli typ wyliczeniowy jest oznaczony za pomocą `Flags` atrybutu.

Aby ułatwić analizatora XAML, bardziej złożonych typów (lub właściwości tych typów) może zawierać [ `TypeConverterAttribute` ](xref:Xamarin.Forms.TypeConverterAttribute) klasę, która pochodzi od klasy, które identyfikują [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) obsługującą konwersja ciąg wartości dla tych typów. Na przykład [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter) Konwertuje kolor nazwy i parametry, takie jak "#rrggbb" do `Color` wartości.

## <a name="property-element-syntax"></a>Składnia elementu właściwości

W XAML klasy i obiekty utworzone na podstawie ich są wyrażane jako elementy XML. Są to znane jako *obiektu elementy*. Większość właściwości te obiekty są wyrażane jako atrybuty XML. Są to tak zwane *atrybuty właściwości*.

Czasami właściwość musi być równa obiekt, który nie może być wyrażona jako prosty ciąg znaków. W takim przypadku XAML obsługuje tag o nazwie *element właściwości* składający się z nazwy klasy i nazwy właściwości oddzielony kropką. Element obiektu następnie może znajdować się w parę znaczników elementu właściwości.

## <a name="adding-a-xaml-page-to-your-project"></a>Dodawanie strony XAML do projektu

Zestaw narzędzi Xamarin.Forms Portable Class Library mogą zawierać strony XAML podczas jej pierwszego tworzenia lub strony XAML można dodać do istniejącego projektu. W oknie dialogowym, aby dodać nowy element, wybierz element który odwołuje się do strony XAML lub `ContentPage` i XAML. (Nie `ContentView`.)

> [!NOTE] 
> Opcje programu Visual Studio zostały zmienione od czasu, w tym rozdziale został zapisany.

Zostaną utworzone dwa pliki: plik XAML z .xaml rozszerzenie nazwy pliku, a plik języka C# z rozszerzeniem. xaml.cs. Plik języka C# jest często nazywany *związanym z kodem* pliku XAML. Plik związany z kodem jest definicji klasy częściowej, która pochodzi od klasy `ContentPage`. W czasie kompilacji XAML jest analizowany i innej definicji częściowej klasy jest generowany dla tej samej klasy. Ta klasa wygenerowanego zawiera metodę o nazwie `InitializeComponent` , jest wywoływana z konstruktora pliku CodeBehind.

Podczas wykonywania po zakończeniu `InitializeComponent` wywołać, wszystkie elementy pliku XAML zostały utworzone i zainicjowane tak, jakby były one tworzone w kodzie języka C#.

Element główny w pliku XAML jest `ContentPage`. Tag główny zawiera co najmniej dwie deklaracje przestrzeni nazw XML, jeden dla elementów zestawu narzędzi Xamarin.Forms i innych definiowanie `x` prefiks dla elementów i atrybutów, które są nierozerwalnie związane wszystkich implementacjach XAML. Zawiera także tag główny `x:Class` atrybutu, która wskazuje obszar nazw i nazwę klasy, która pochodzi od klasy `ContentPage`. Odpowiada to nazwa przestrzeni nazw i klasy w pliku związanym z kodem.

Obrazuje kombinacji XAML i kodu [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) próbki.

## <a name="the-xaml-compiler"></a>Kompilator XAML

Xamarin.Forms ma kompilatora XAML, ale jej użycie jest opcjonalna, odpowiednio do stosowania [ `XamlCompilationAttribute` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute). Jeśli XAML nie jest kompilowana, XAML jest analizowany w czasie kompilacji, a plik XAML jest osadzony w aplikacji PCL, gdzie również jest analizowany w czasie wykonywania. Jeśli XAML jest kompilowany, proces kompilacji konwertuje XAML w formie binarnej, a przetwarzanie środowiska uruchomieniowego jest bardziej wydajne.

## <a name="platform-specificity-in-the-xaml-file"></a>Platforma szczegółowością w pliku XAML

W XAML [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) klasa może być używana do wybrania znaczników zależny od platformy. To jest klasa generyczna i musi być utworzone przy użyciu `x:TypeArguments` atrybut, który jest zgodny z typem docelowym. [ `OnIdiom` ](xref:Xamarin.Forms.OnIdiom`1) Klasy odbywa się podobnie, ale używanych znacznie mniej.

Korzystanie z `OnPlatform` została zmieniona od czasu opublikowania książki. Pierwotnie został użyty w połączeniu z właściwościami o nazwie `iOS`, `Android`, i `WinPhone`. Obecnie jest używany z podrzędnych [ `On` ](xref:Xamarin.Forms.On) obiektów. Ustaw [ `Platform` ](xref:Xamarin.Forms.On.Platform) właściwości na ciąg, które są zgodne z publicznie `const` pola [ `Device` ](xref:Xamarin.Forms.Device) klasy. Ustaw [ `Value` ](xref:Xamarin.Forms.On.Value) właściwość z wartością, które są zgodne z `x:TypeArguments` atrybutu `OnPlatform` tagu.

`OnPlatform` zostało to przedstawione w [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) próbki, więc wywołuje się, ponieważ zawiera bloki konstrukcyjne XAML niemal identyczne. Istnienie tego monotonnych znaczników sugeruje, że techniki powinny być dostępne dla ograniczenia go.

## <a name="the-content-property-attributes"></a>Atrybuty właściwości zawartości

Niektóre elementy właściwości występują bardzo często, takich jak `<ContentPage.Content>` tagiem na element główny `ContentPage`, lub `<StackLayout.Children>` tag, który zawiera elementy podrzędne `StackLayout`.

Każda klasa będzie mógł zidentyfikować jedną właściwość z [ `ContentPropertyAttribute` ](xref:Xamarin.Forms.ContentPropertyAttribute) w klasie. Dla tej właściwości tagi element właściwości nie są wymagane. `ContentPage` Definiuje jej właściwości zawartości jako `Content`, i `Layout<T>` (klasa, z której `StackLayout` pochodzi) definiuje jej właściwości zawartości jako `Children`. Te znaczniki element właściwości nie są wymagane.

Właściwość elementu `Label` jest `Text`.

## <a name="formatted-text"></a>Tekst sformatowany

[ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) sample zawiera kilka przykładów ustawienie `Text` i `FormattedText` właściwości `Label`. W XAML `Span` obiekty są wyświetlane jako elementy podrzędne `FormattedString` obiektu.

 Gdy wielowierszowy ciąg ma wartość `Text` właściwości znaki końca wiersza są konwertowane na znaki spacji, ale znaki końca wiersza są zachowywane podczas wielowierszowy ciąg, który jest wyświetlany jako zawartość `Label` lub `Label.Text` tagi:

 [![Potrójna zrzut ekranu przedstawiający odmiany tekstu udostępnianie](images/ch07fg03-small.png "sformatowany tekst odmiany")](images/ch07fg03-large.png#lightbox "odmiany tekst sformatowany")

## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 7 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Przykłady rozdział 7](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Przykładowe rozdział 7 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
- [XAML — podstawy](~/xamarin-forms/xaml/xaml-basics/index.md)