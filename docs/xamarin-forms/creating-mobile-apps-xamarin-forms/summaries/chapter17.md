---
title: Podsumowanie rozdziałów 17. Opanowanie siatki
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: podsumowanie z rozdziałem 17. Opanowanie siatki'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b71859d0848d7bf790b3cc4beddc67a5ea86d340
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935481"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Podsumowanie rozdziałów 17. Opanowanie siatki

[ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) To mechanizm zaawansowane układ, który organizuje jego elementy podrzędne w wiersze i kolumny komórek. W przeciwieństwie do podobnych HTML `table` elementu `Grid` służy wyłącznie do celów układu, a nie w prezentacji.

## <a name="the-basic-grid"></a>Siatka podstawowa

`Grid` pochodzi od klasy [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), która definiuje [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) właściwości, `Grid` dziedziczy. Należy wypełnić tej kolekcji w XAML lub kodu.

### <a name="the-grid-in-xaml"></a>Siatki w XAML

Definicja `Grid` w XAML zwykle zaczyna się od wypełnianie [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) i [ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) kolekcji `Grid` z [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) i [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) obiektów. Jest to, jak ustalenia liczby wierszy i kolumn `Grid`oraz ich właściwości.

`RowDefinition` ma [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) właściwości i `ColumnDefinition` ma [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) właściwość, oba typu [ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/), strukturę.

W XAML [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) konwertuje ciągi zwykłego tekstu do `GridLength` wartości. Za kulisami [ `GridLength` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) tworzy `GridLength` wartości na podstawie liczby i wartości typu [ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), wyliczenie z trzema elementami członkowskimi:

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; szerokość lub wysokość jest określona w jednostkach niezależnych od urządzenia (liczba w XAML)
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; szerokość lub wysokość jest autosized na podstawie zawartości komórki ("Auto" w XAML)
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; szerokość lub wysokość pozostawionych jest przydzielany proporcjonalnie (liczbę "\*", co jest nazywane *gwiazdki*, w XAML)

Każdy element podrzędny `Grid` muszą być przypisane wierszy i kolumn (jawnie lub niejawnie). Obejmuje wiersza i kolumny zakresy są opcjonalne. Te są wszystkie określone za pomocą dołączonych właściwości możliwej do wiązania &mdash; właściwości, które są definiowane przez `Grid` ale ustawiony na elementy podrzędne `Grid`. `Grid` definiuje cztery statyczne dołączone właściwości możliwej do wiązania:

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) &mdash; wiersz zaczynający się od zera; domyślna to 0
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) &mdash; liczony od zera kolumnach. domyślna to 0
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) &mdash; Liczba wierszy obejmuje elementu podrzędnego; Wartość domyślna to 1
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) &mdash; Liczba kolumn obejmuje elementu podrzędnego; Wartość domyślna to 1

W kodzie programu umożliwia osiem metody statyczne ustawiają i pobierają te wartości:

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) i [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) i [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) i [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) i [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

W XAML można użyć następujących atrybutów do ustawiania tych wartości:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) w przykładzie pokazano tworzenie i Inicjowanie `Grid` w XAML.

`Grid` Dziedziczy [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) właściwość `Layout` i definiuje dwa dodatkowe właściwości, które zapewniają odstępy między wierszami i kolumnami:

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) ma wartość domyślną 6
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) ma wartość domyślną 6

`RowDefinitions` i `ColumnDefinitions` kolekcje nie są ściśle wymagane. Jeśli go nie ma, `Grid` tworzy wierszy i kolumn `Grid` elementów podrzędnych i udostępnia je wszystkie domyślne `GridLength` z "\*" (gwiazdka).

### <a name="the-grid-in-code"></a>Siatki w kodzie

[ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) przykładowych pokazano, jak utworzyć i wypełnić `Grid` w kodzie. Dołączone właściwości można ustawić dla każdego elementu podrzędnego bezpośrednio lub pośrednio przez wywołanie metody dodatkowe `Add` metody takie jak [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) definicją [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) interfejs.

### <a name="the-grid-bar-chart"></a>Wykres słupkowy siatki

[ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) przykładowych pokazano, jak dodać wiele `BoxView` elementów `Grid` za pomocą zbiorczego [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) metody. Domyślnie te `BoxView` elementy mają równa szerokość. Wysokość każdego `BoxView` można sterować tak, aby przypominały wykres słupkowy.

`Grid` w **GridBarChart** przykładowy udziałów `AbsoluteLayout` elementu nadrzędnego z początkowo niewidoczne `Frame`. Ustawia również program `TapGestureRecognizer` na każdym `BoxView` używać `Frame` do wyświetlania informacji na temat naciśnięty paska.

### <a name="alignment-in-the-grid"></a>Wyrównanie w siatce

[ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) przykład pokazuje, jak używać `VerticalOptions` i `HorizontalOptions` właściwości do wyrównania elementów podrzędnych w `Grid` komórki.

[ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) przykładowy równie miejsca do magazynowania `Button` elementy wyśrodkowany w `Grid` komórek.

### <a name="cell-dividers-and-borders"></a>Separator komórki i krawędzie

`Grid` Nie ma funkcji, który Rysuje linie oddzielające komórki lub obramowań. Jednakże możesz utworzyć swój własny.

[ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) pokazuje, jak zdefiniować dodatkowe wiersze i kolumny specjalnie pod kątem wiele do zrobienia `BoxView` elementy do naśladowania linii podziału.

[ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) program nie tworzy dodatkowe komórki, ale zamiast tego wyrównuje `BoxView` elementów w każdej komórce, aby mógł naśladować krawędzi komórki.

## <a name="almost-real-life-grid-examples"></a>Prawie życia siatki przykłady

[ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) przykładowy używa `Grid` wyświetlać klawiaturę:

[![Potrójna zrzut ekranu przedstawiający siatki klawiatury numerycznej](images/ch17fg12-small.png "siatki klawiatury numerycznej")](images/ch17fg12-large.png#lightbox "siatki klawiatury numerycznej")

### <a name="responding-to-orientation-changes"></a>Reagowanie na zmiany orientacji

`Grid` Może pomóc struktury programu służącego do reagowania na zmiany orientacji. [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) w przykładzie pokazano technika, która przenosi element drugiego wiersza w orientacji pionowej telefonu i druga kolumna w orientacji poziomej telefonu.

Inicjuje program `Slider` elementy do zakresu od 0 do 255 i wiązania danych używa, aby wyświetlić wartość suwaki w formacie szesnastkowym. Ponieważ `Slider` wartości są liczb zmiennoprzecinkowych punkt i .NET, formatowanie ciągu szesnastkowego działa tylko z liczb całkowitych, [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki pomaga w.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst 17 rozdziałów (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Przykłady 17 rozdziałów](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Siatka](~/xamarin-forms/user-interface/layouts/grid.md)
