---
title: "Podsumowanie rozdział 17. Opanowanie siatki"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 09f63dd418ea1fb523c028edb02c28c22bfdccd1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Podsumowanie rozdział 17. Opanowanie siatki

[ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) Jest mechanizmem zaawansowanych układu Rozmieszcza elementy podrzędne w wiersze i kolumny komórek. W przeciwieństwie do podobnych HTML `table` elementu `Grid` jest wyłącznie do celów układ, a nie na prezentacji.

## <a name="the-basic-grid"></a>Podstawowe siatki

`Grid` pochodną [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), który definiuje [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) właściwości który `Grid` dziedziczy. Możesz wpisać tę kolekcję na języku XAML lub kodu.

### <a name="the-grid-in-xaml"></a>Siatki w języku XAML

Definicja `Grid` w języku XAML zazwyczaj rozpoczyna się od wypełnianie [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) i [ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) kolekcji `Grid` z [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) i [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) obiektów. Jest to, jak ustalenia liczby wierszy i kolumn `Grid`oraz ich właściwości.

`RowDefinition` ma [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) właściwości i `ColumnDefinition` ma [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) właściwości, zarówno typu [ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/), strukturę.

W języku XAML [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) konwertuje ciągów prosty tekst do `GridLength` wartości. W tle [ `GridLength` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) tworzy `GridLength` wartości na podstawie numeru i wartości typu [ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), wyliczenie z trzech elementów członkowskich:

- [`Absolute`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Absolute/) & #x 2014; szerokość lub wysokość jest określony w jednostkach niezależnych od urządzenia (liczba w języku XAML)
- [`Auto`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Auto/) & #x 2014; wysokość lub szerokość jest autosized na podstawie zawartości komórki ("Auto" w języku XAML)
- [`Star`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) & #x 2014; pozostałość wysokość lub szerokość jest przydzielona proporcjonalnie (liczbę "\*" o nazwie *gwiazdy*, w języku XAML)

Poszczególne elementy podrzędne elementu `Grid` muszą być przypisane wierszy i kolumn (jawnie lub niejawnie). Obejmuje wiersza i kolumny zakresy są opcjonalne. Te są wszystkie określone za pomocą dołączonych właściwości & #x 2014; właściwości zdefiniowane przez `Grid` ale ustawione na elementy podrzędne `Grid`. `Grid` definiuje cztery statyczne dołączonych właściwości:

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) & #x 2014; liczony od zera wiersza; domyślna to 0
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) & #x 2014; liczony od zera kolumnach. domyślna to 0
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) & #x 2014; Liczba wierszy podrzędnych obejmuje; Wartość domyślna to 1
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) & #x 2014; Liczba kolumn podrzędnych obejmuje; Wartość domyślna to 1

W kodzie program umożliwia osiem metod statycznych Ustawianie i pobieranie te wartości:

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) I [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) I [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) I [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) I [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

W języku XAML do tych wartości użyć następujących atrybutów:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) przykładzie pokazano tworzenie i Inicjowanie `Grid` w języku XAML.

`Grid` Dziedziczy [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) właściwość z `Layout` i definiuje dwie dodatkowe właściwości, które dostarczają odstępy między wierszy i kolumn:

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) ma wartość domyślną 6
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) ma wartość domyślną 6

`RowDefinitions` i `ColumnDefinitions` kolekcje nie są ściśle wymagane. Jeśli go nie ma, `Grid` tworzy wierszy i kolumn `Grid` dzieci oraz udostępnia je wszystkie domyślne `GridLength` z "\*" (gwiazdkę).

### <a name="the-grid-in-code"></a>Siatki w kodzie

[ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) przykładowych pokazano, jak utworzyć i wypełnić `Grid` w kodzie. Dołączone właściwości można ustawić dla każdego elementu podrzędnego bezpośrednio lub pośrednio przez wywołanie metody dodatkowe `Add` metod, takich jak [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) zdefiniowane przez [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) interfejs.

### <a name="the-grid-bar-chart"></a>Wykres słupkowy siatki

[ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) przykładowych pokazano, jak dodawać wiele `BoxView` elementy `Grid` przy użyciu zbiorczego [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) metody. Domyślnie te `BoxView` elementy mają takie same szerokości. Wysokość każdego `BoxView` można sterować tak, aby przypominały wykresu słupkowego.

`Grid` w **GridBarChart** przykładowe udziałów `AbsoluteLayout` nadrzędny początkowo niewidoczne `Frame`. Ustawia również program `TapGestureRecognizer` na każdym `BoxView` do używania `Frame` do wyświetlania informacji o tapped paska.

### <a name="alignment-in-the-grid"></a>Wyrównanie w siatce

[ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) przykładzie pokazano sposób użycia `VerticalOptions` i `HorizontalOptions` właściwości, aby były wyrównane dzieci w `Grid` komórki.

[ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) przykładowe jednakowo spacje `Button` elementy wyśrodkowany w `Grid` komórek.

### <a name="cell-dividers-and-borders"></a>Komórki separatorów i obramowania

`Grid` Nie obejmuje funkcję rysuje separatorów komórki lub obramowań. Jednak należy utworzyć własny.

[ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) pokazano, jak zdefiniować dodatkowe wiersze i kolumny specjalnie z myślą o cienki `BoxView` elementy, które imitują linii podziału.

[ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) program nie tworzy dodatkowe komórki, ale zamiast tego wyrównuje `BoxView` elementów w każdej komórki w naśladować krawędzi komórki.

## <a name="almost-real-life-grid-examples"></a>Przykłady siatki prawie rzeczywistych

[ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) przykładowe używa `Grid` do wyświetlenia klawiatury:

[![Potrójna zrzut ekranu przedstawiający siatki klawiatury numerycznej](images/ch17fg12-small.png "siatki klawiatury numerycznej")](images/ch17fg12-large.png#lightbox "siatki klawiatury numerycznej")

### <a name="responding-to-orientation-changes"></a>Odpowiada na żądania zmiany orientacji

`Grid` Może pomóc struktura programu w celu reagowania na zmiany orientacji. [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) przykładzie pokazano technika, który przenosi element między drugiego wiersza w orientacji pionowej telefonu i drugiej kolumny telefonu orientacji poziomej.

Inicjuje program `Slider` elementy do zakresu od 0 do 255 i używa wiązania danych, aby wyświetlić wartość suwaki w formacie szesnastkowym. Ponieważ `Slider` wartości są zmiennoprzecinkową punktu i .NET formatowanie ciąg szesnastkowy działa tylko z liczb całkowitych, [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki pomaga.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 17 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Przykłady rozdział 17](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Siatka](~/xamarin-forms/user-interface/layouts/grid.md)
