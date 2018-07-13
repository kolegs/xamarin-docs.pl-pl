---
title: Podsumowanie rozdziałów 17. Opanowanie siatki
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: podsumowanie z rozdziałem 17. Opanowanie siatki'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 5f5b934b5f828bf6f5e8d4a0f0738c7db633aefb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995843"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Podsumowanie rozdziałów 17. Opanowanie siatki

[ `Grid` ](xref:Xamarin.Forms.Grid) To mechanizm zaawansowane układ, który organizuje jego elementy podrzędne w wiersze i kolumny komórek. W przeciwieństwie do podobnych HTML `table` elementu `Grid` służy wyłącznie do celów układu, a nie w prezentacji.

## <a name="the-basic-grid"></a>Siatka podstawowa

`Grid` pochodzi od klasy [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1), która definiuje [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) właściwości, `Grid` dziedziczy. Należy wypełnić tej kolekcji w XAML lub kodu.

### <a name="the-grid-in-xaml"></a>Siatki w XAML

Definicja `Grid` w XAML zwykle zaczyna się od wypełnianie [ `RowDefinitions` ](xref:Xamarin.Forms.Grid.RowDefinitions) i [ `ColumnDefinitions` ](xref:Xamarin.Forms.Grid.ColumnDefinitions) kolekcji `Grid` z [ `RowDefinition` ](xref:Xamarin.Forms.RowDefinition) i [ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition) obiektów. Jest to, jak ustalenia liczby wierszy i kolumn `Grid`oraz ich właściwości.

`RowDefinition` ma [ `Height` ](xref:Xamarin.Forms.RowDefinition.Height) właściwości i `ColumnDefinition` ma [ `Width` ](xref:Xamarin.Forms.ColumnDefinition.Width) właściwość, oba typu [ `GridLength` ](xref:Xamarin.Forms.GridLength), strukturę.

W XAML [ `GridLengthTypeConverter` ](xref:Xamarin.Forms.GridLengthTypeConverter) konwertuje ciągi zwykłego tekstu do `GridLength` wartości. Za kulisami [ `GridLength` Konstruktor](xref:Xamarin.Forms.GridLength.%23ctor(System.Double,Xamarin.Forms.GridUnitType)) tworzy `GridLength` wartości na podstawie liczby i wartości typu [ `GridUnitType` ](xref:Xamarin.Forms.GridUnitType), wyliczenie z trzema elementami członkowskimi:

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; szerokość lub wysokość jest określona w jednostkach niezależnych od urządzenia (liczba w XAML)
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; szerokość lub wysokość jest autosized na podstawie zawartości komórki ("Auto" w XAML)
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; szerokość lub wysokość pozostawionych jest przydzielany proporcjonalnie (liczbę "\*", co jest nazywane *gwiazdki*, w XAML)

Każdy element podrzędny `Grid` muszą być przypisane wierszy i kolumn (jawnie lub niejawnie). Obejmuje wiersza i kolumny zakresy są opcjonalne. Te są wszystkie określone za pomocą dołączonych właściwości możliwej do wiązania &mdash; właściwości, które są definiowane przez `Grid` ale ustawiony na elementy podrzędne `Grid`. `Grid` definiuje cztery statyczne dołączone właściwości możliwej do wiązania:

- [`RowProperty`](xref:Xamarin.Forms.Grid.RowProperty) &mdash; wiersz zaczynający się od zera; domyślna to 0
- [`ColumnProperty`](xref:Xamarin.Forms.Grid.ColumnProperty) &mdash; liczony od zera kolumnach. domyślna to 0
- [`RowSpanProperty`](xref:Xamarin.Forms.Grid.RowSpanProperty) &mdash; Liczba wierszy obejmuje elementu podrzędnego; Wartość domyślna to 1
- [`ColumnSpanProperty`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) &mdash; Liczba kolumn obejmuje elementu podrzędnego; Wartość domyślna to 1

W kodzie programu umożliwia osiem metody statyczne ustawiają i pobierają te wartości:

- [`Grid.SetRow`](xref:Xamarin.Forms.Grid.SetRow(Xamarin.Forms.BindableObject,System.Int32)) i [`Grid.GetRow`](xref:Xamarin.Forms.Grid.GetRow(Xamarin.Forms.BindableObject))
- [`Grid.SetColumn`](xref:Xamarin.Forms.Grid.SetColumn(Xamarin.Forms.BindableObject,System.Int32)) i [`Grid.GetColumn`](xref:Xamarin.Forms.Grid.GetColumn(Xamarin.Forms.BindableObject))
- [`Grid.SetRowSpan`](xref:Xamarin.Forms.Grid.SetRowSpan(Xamarin.Forms.BindableObject,System.Int32)) i [`Grid.GetRowSpan`](xref:Xamarin.Forms.Grid.GetRowSpan(Xamarin.Forms.BindableObject))
- [`Grid.SetColumnSpan`](xref:Xamarin.Forms.Grid.SetColumnSpan(Xamarin.Forms.BindableObject,System.Int32)) i [`Grid.GetColumnSpan`](xref:Xamarin.Forms.Grid.GetColumnSpan(Xamarin.Forms.BindableObject))

W XAML można użyć następujących atrybutów do ustawiania tych wartości:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) w przykładzie pokazano tworzenie i Inicjowanie `Grid` w XAML.

`Grid` Dziedziczy [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) właściwość `Layout` i definiuje dwa dodatkowe właściwości, które zapewniają odstępy między wierszami i kolumnami:

- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) ma wartość domyślną 6
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) ma wartość domyślną 6

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
