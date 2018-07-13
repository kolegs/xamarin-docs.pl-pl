---
title: Podsumowanie rozdziałów 19. Widoki kolekcji
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziałów 19. Widoki kolekcji'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a04f72d0577c7b8bf467ee73df233e483f6aa401
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996428"
---
# <a name="summary-of-chapter-19-collection-views"></a>Podsumowanie rozdziałów 19. Widoki kolekcji

Zestaw narzędzi Xamarin.Forms definiuje trzy widoki, które utrzymują kolekcji i wyświetlać ich elementy:

- [`Picker`](xref:Xamarin.Forms.Picker) jest stosunkowo krótka listy elementów ciąg, który umożliwia użytkownikowi wybranie jednego
- [`ListView`](xref:Xamarin.Forms.ListView) często jest długą listę elementów, które zazwyczaj tego samego typu i formatowania, również pozwalającą użytkownikowi wybrać jeden
- [`TableView`](xref:Xamarin.Forms.TableView) jest to zbiór *komórek* (zwykle o różne typy i wystąpienia) do wyświetlania danych lub zarządzania danych wejściowych użytkownika

Jest wspólne MVVM do użycia przez aplikacje `ListView` do wyświetlenia można wybrać kolekcję obiektów.

## <a name="program-options-with-picker"></a>Opcje programu za pomocą selektora

[ `Picker` ](xref:Xamarin.Forms.Picker) To dobry wybór w przypadku, gdy należy zezwolić użytkownikowi na wybranie opcji spośród względnie krótkich listę `string` elementów.

### <a name="the-picker-and-event-handling"></a>Wybór i obsługa zdarzeń

[ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) przykład pokazuje, jak ustawić za pomocą XAML `Picker` [ `Title` ](xref:Xamarin.Forms.Picker.Title) właściwości i dodać `string` elementów do [ `Items` ](xref:Xamarin.Forms.Picker.Items) kolekcji. Gdy użytkownik wybierze `Picker`, wyświetla elementy w `Items` kolekcji w sposób zależny od platformy.

[ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Zdarzenie wskazuje, kiedy użytkownik wybrał element. Liczony od zera [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) właściwość wskazuje następnie wybranego elementu. Jeśli żaden element nie jest zaznaczone, `SelectedIndex` jest równa &ndash;1.

Można również użyć `SelectedIndex` zainicjować wybranego elementu, ale należy ustawić po `Items` kolekcji jest wypełnione. W XAML, oznacza to, że element właściwości prawdopodobnie będzie używana do ustawiania `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Selektor powiązania danych

`SelectedIndex` Właściwość jest wspierana przez właściwości możliwej do wiązania, ale `Items` nie jest dostępna, więc używanie powiązania danych z `Picker` jest trudne. Jednym rozwiązaniem jest użycie `Picker` w połączeniu z [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) znajdującego się w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki. [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) pokazuje, jak to działa.

## <a name="rendering-data-with-listview"></a>Renderowanie danych przy użyciu ListView

[ `ListView` ](xref:Xamarin.Forms.ListView) Jest to jedyna klasa, która pochodzi od klasy [ `ItemsView<TVisual>` ](xref:Xamarin.Forms.ItemsView`1) z której dziedziczy [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) i [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) właściwości.

`ItemsSource` Typ jest `IEnumerable` , ale jest `null` domyślnie i musi być jawnie zainicjowana lub ustaw (najczęściej) do kolekcji, za pomocą powiązania danych. Elementy w tej kolekcji może być dowolnego typu.

`ListView` definiuje [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) właściwość, która jest ustawiony na jedną z elementów w `ItemsSource` kolekcji lub `null` Jeśli nie wybrano elementu. `ListView` generowane [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) zdarzenie, gdy nowy element jest wybrany.

### <a name="collections-and-selections"></a>Kolekcje i opcje

[ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) przykładowy wypełnienia `ListView` z 17 `Color` wartości w `List<Color>` kolekcji. Elementy są można wybierać, ale domyślnie są wyświetlane z ich nieatrakcyjnych `ToString` reprezentacji. Kilka przykładów w tym rozdziale pokazano, jak naprawić, obok którego wyświetlona i jak atrakcyjny zgodnie z potrzebami.

### <a name="the-row-separator"></a>Separator wierszy

W systemach iOS i Android wyświetla linię alokowania elastycznego oddziela wierszy. Można kontrolować za pomocą [ `SeparatorVisibiliy` ](xref:Xamarin.Forms.ListView.SeparatorVisibility) i [ `SeparatorColor` ](xref:Xamarin.Forms.ListView.SeparatorColor) właściwości. `SeparatorVisibility` Właściwość jest typu [ `SeparatorVisbility` ](xref:Xamarin.Forms.SeparatorVisibility), wyliczenie z dwóch elementów:

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default), domyślne ustawienie
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>Wybrany element powiązania danych

`SelectedItem` Właściwość jest objęta właściwości możliwej do wiązania, aby mogło być ono źródłowym lub docelowym powiązanie danych. Domyślną `BindingMode` jest `OneWayToSource`, ale zazwyczaj jest celem powiązanie dwukierunkowe danych, szczególnie w scenariuszach MVVM. [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) przykład demonstruje ten typ powiązania.

### <a name="the-observablecollection-difference"></a>Różnica observablecollection —

[ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) przykładowe zestawy `ItemsSource` właściwość `ListView` do `List<DateTime>` kolekcji, a następnie stopniowo dodaje nową `DateTime` do kolekcji co sekundę przy użyciu czasomierz.

Jednak `ListView` automatycznie nie automatycznie zaktualizowana, ponieważ `List<T>` kolekcji nie ma mechanizmu powiadomienie, aby wskazać, kiedy elementy są dodawane do lub usuwane z kolekcji.

Jest znacznie lepiej klasy do użycia w takich scenariuszach [ `ObservableCollection<T>` ](xref:System.Collections.ObjectModel.ObservableCollection`1) zdefiniowane w `System.Collections.ObjectModel` przestrzeni nazw. Ta klasa implementuje [ `INotifyCollectionChanged` ](xref:System.Collections.Specialized.INotifyCollectionChanged) interfejsu i w związku z tym generowane [ `CollectionChanged` ](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged) zdarzenie, gdy elementy są dodawane do lub usuwane z kolekcji lub, gdy są one zastąpione lub przeniesione wewnątrz Kolekcja. Podczas `ListView` wewnętrznie wykrycia, że klasy wykonania `INotifyCollectionChanged` został ustawiony na jego `ItemsSource` właściwości dołącza obsługi ma `CollectionChanged` zdarzeń i aktualizuje swoją wyświetlaną po zmianie kolekcji.

[ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) przykład demonstruje użycie `ObservableCollection`.

### <a name="templates-and-cells"></a>Szablony i komórki

Domyślnie `ListView` Wyświetla elementy w kolekcji, jego przy użyciu każdego elementu `ToString` metody. Lepszym rozwiązaniem obejmuje Definiowanie szablonu, aby wyświetlić elementy.

Aby wypróbować tę funkcję, możesz użyć [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki. Ta klasa definiuje statycznego `All` właściwości typu `IList<NamedColor>` zawierający 141 `NamedColor` obiekty odpowiadające pola publiczne `Color` struktury.

[ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) przykładowe zestawy `ItemsSource` z `ListView` tej `NamedColor.All` właściwości, ale tylko klasy w pełni kwalifikowanej nazwy `NamedColor` obiektów wyświetlane.

`ListView` musi mieć szablon, aby wyświetlić te elementy. W kodzie, można ustawić [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) zdefiniowaną przez właściwość `ItemsView<TVisual>` do [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) przy użyciu [ `DataTemplate` Konstruktor](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) , odwołuje się do procesu programu [ `Cell` ](xref:Xamarin.Forms.Cell) klasy. `Cell` zawiera pięć pochodnych:

- [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash; zawiera dwa `Label` widoki (koncepcyjnie wypowiedzi)
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash; dodaje `Image` Wyświetl do `TextCell`
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash; zawiera `Entry` widoku z `Label`
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash; zawiera `Switch` z `Label`
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash; może to być dowolna `View` (prawdopodobnie z elementami podrzędnymi)

Następnie wywołaj [ `SetValue` ](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object)) i [ `SetBinding` ](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) na `DataTemplate` powiązania wartości z obiektu `Cell` właściwości, lub ustawić powiązania danych na `Cell` właściwości odwołujące się do właściwości elementów w `ItemsSource` kolekcji. Jest to zaprezentowane w [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) próbki.

Ponieważ każdy element jest wyświetlany przez `ListView`małych drzewo wizualne jest tworzona z szablonu i powiązania danych są ustanawiane między elementu i właściwości elementów w tym drzewo wizualne. Pomysł ten proces można uzyskać, instalując programy obsługi dla [ `ItemAppearing` ](xref:Xamarin.Forms.ListView.ItemAppearing) i [ `ItemDisappearing` ](xref:Xamarin.Forms.ListView.ItemDisappearing) zdarzenia `ListView`, lub za pomocą alternatywę [ `DataTemplate`Konstruktor](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object})) używającą funkcji, która jest wywoływana za każdym razem, musi zostać utworzony element drzewo wizualne.

[ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) zawiera funkcjonalnie identyczny program całkowicie w XAML. A `DataTemplate` ustawiony jest tag `ItemTemplate` właściwość `ListView`, a następnie `TextCell` ustawiono `DataTemplate`. Powiązania właściwości elementów w kolekcji są ustawiane bezpośrednio na [ `Text` ](xref:Xamarin.Forms.TextCell.Text) i [ `Detail` ](xref:Xamarin.Forms.TextCell.Detail) właściwości `TextCell`.

### <a name="custom-cells"></a>Niestandardowe komórek

W XAML jest możliwe ustawienie [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) do `DataTemplate` , a następnie zdefiniować niestandardowe drzewo wizualne jako [ `View` ](xref:Xamarin.Forms.ViewCell.View) właściwość `ViewCell`. (`View` jest właściwość content `ViewCell` więc `ViewCell.View` tagi nie są wymagane.) [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) w przykładzie pokazano tej techniki:

[![Potrójna zrzut ekranu przedstawiający listę kolorów o nazwie niestandardowy](images/ch19fg11-small.png "Lista kolorów o nazwie niestandardowy")](images/ch19fg11-large.png#lightbox "Lista kolorów o nazwie niestandardowy")

Pobieranie rozmiaru odpowiednią dla wszystkich platform może być trudne. [ `RowHeight` ](xref:Xamarin.Forms.ListView.RowHeight) Właściwość jest przydatna, ale w niektórych przypadkach można odwołać się do [ `HasUnevenRows` ](xref:Xamarin.Forms.ListView.HasUnevenRows) właściwość, która jest mniej wydajne rozwiązanie, ale wymusza `ListView` rozmiar wierszy. Dla systemów iOS i Android musi być jedną z tych dwóch właściwości Pobierz wiersz odpowiedniego rozmiaru.

### <a name="grouping-the-listview-items"></a>Grupowanie elementów ListView

`ListView` obsługuje grupowanie elementów i nawigacja pomiędzy tych grup. `ItemsSource` Do kolekcji z kolekcji, można ustawić właściwości: obiekt, `ItemsSource` ustawiono musi implementować `IEnumerable`, i każdy element w kolekcji musi implementować też `IEnumerable`. Każda grupa powinna zawierać dwie właściwości: opis grupy oraz — trzyliterowy skrót.

[ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki tworzy grupy siedem `NamedColor` obiektów. [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) przykład pokazuje, jak korzystać z tych grup z [ `IsGroupingEnabled` ](xref:Xamarin.Forms.ListView.IsGroupingEnabled) właściwość `ListView` równa `true`i [ `GroupDisplayBinding` ](xref:Xamarin.Forms.ListView.GroupDisplayBinding) i [ `GroupShortNameBinding` ](xref:Xamarin.Forms.ListView.GroupShortNameBinding) właściwości powiązane z właściwości w każdej grupie.

### <a name="custom-group-headers"></a>Nagłówki niestandardowe grupy

Istnieje możliwość utworzyć niestandardowe nagłówki dla `ListView` grup, zastępując `GroupDisplayBinding` właściwość o [ `GroupHeaderTemplate` ](xref:Xamarin.Forms.ListView.GroupHeaderTemplate) Definiowanie szablonu dla nagłówków.

### <a name="listview-and-interactivity"></a>ListView i interakcyjność

Zazwyczaj aplikacja uzyskuje interakcji użytkownika z `ListView` przez dołączenie obsługi ma `ItemSelected` lub [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) zdarzenia lub poprzez skonfigurowanie powiązanie danych w `SelectedItem` właściwości. Jednak niektóre typy komórki (`EntryCell` i `SwitchCell`) Zezwalaj na interakcję użytkownika, a także możliwych do utworzenia niestandardowych komórki tej interakcji z użytkownikiem. [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) tworzy wystąpienia 100 [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) i umożliwia użytkownikowi zmianę każdy kolor przy użyciu Trójka `Slider` elementów. Program również sprawia, że użycie [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) w [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView i MVVM

`ListView` odgrywa ważną rolę w scenariuszach MVVM. Zawsze, gdy `IEnumerable` w ViewModel istnieje w kolekcji, często jest powiązany z `ListView`. Ponadto zaimplementować elementów w kolekcji często `INotifyPropertyChanged` do powiązania z właściwościami w szablonie.

### <a name="a-collection-of-viewmodels"></a>Kolekcja modele widoków

Aby zapoznać się z tym [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) biblioteki tworzy kilka klas na podstawie [pliku danych XML](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) i obrazy fikcyjne studentów tej szkoły fikcyjne.

[ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) Klasa pochodzi od [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) Klasy jest kolekcją `Student` obiektów, a także pochodzi od klasy `ViewModelBase`. [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) Pobiera plik XML i składana wszystkich obiektów.

[ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) program używa `ImageCell` do wyświetlania uczniów i ich obrazów w `ListView`:

[![Potrójna zrzut ekranu przedstawiający listę uczniów](images/ch19fg18-small.png "listy uczniów")](images/ch19fg18-large.png#lightbox "listy studentów")

[ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) Przykładowa aplikacja dodaje [ `Header` ](xref:Xamarin.Forms.ListView.Header) właściwość, ale wyświetlane tylko w systemie Android.

### <a name="selection-and-the-binding-context"></a>Wybór i kontekstu powiązania

[ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) program powiązań `BindingContext` z `StackLayout` do `SelectedItem` właściwość `ListView`. Dzięki temu program wyświetlić szczegółowe informacje na temat wybranego dla uczniów.

### <a name="context-menus"></a>Menu kontekstowe

Komórka można zdefiniować menu kontekstowego, który jest implementowany w sposób specyficzny dla platformy. Aby utworzyć to menu, Dodaj [ `MenuItem` ](xref:Xamarin.Forms.MenuItem) obiekty do [ `ContextActions` ](xref:Xamarin.Forms.Cell.ContextActions) właściwość `Cell`.

`MenuItem` Definiuje właściwości pięć:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) tego typu `string`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) tego typu `FileImageSource`
- [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive) tego typu `bool`
- [`Command`](xref:Xamarin.Forms.MenuItem.Command) tego typu `ICommand`
- [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) tego typu `object`

`Command` i `CommandParameter` właściwości oznacza, że ViewModel dla każdego elementu zawiera metody służące do przeprowadzania żądanego polecenia. W scenariuszach bez MVVM `MenuItem` definiuje również [ `Clicked` ](xref:Xamarin.Forms.MenuItem.Clicked) zdarzeń.

[ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) pokazano tej techniki. `Command` Właściwości każdego `MenuItem` jest powiązana z właściwością typu `ICommand` w `Student` klasy. Ustaw `IsDestructive` właściwości `true` dla `MenuItem` , usuwa lub usuwa wybranego obiektu.

### <a name="varying-the-visuals"></a>Różnicowanie wizualizacji

Czasami można niewielkie różnice w wizualizacji elementów w `ListView` na podstawie właściwości. Na przykład, gdy średnią ocen studenta w punkcie klasy korporacyjnej spada poniżej w wersji 2.0, [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) przykład wyświetla nazwę tego uczniów na czerwono.
Jest to realizowane przy użyciu konwertera wartości wiązania [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki.

### <a name="refreshing-the-content"></a>Odświeżanie zawartości

`ListView` Obsługę gestu rozwijanego odświeżanie danych. Należy ustawić program [ `IsPullToRefresh` ](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) właściwość `true` Aby włączyć tę opcję. `ListView` Odpowiada gestu rozwijanego, ustawiając jego [ `IsRefreshing` ](xref:Xamarin.Forms.ListView.IsRefreshing) właściwości `true`i podnosząc [ `Refreshing` ](xref:Xamarin.Forms.ListView.Refreshing) zdarzenia i (MVVM) podczas wywoływania `Execute` metody jej [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand) właściwości.

Kod obsługi `Refresh` zdarzeń lub `RefreshCommand` prawdopodobnie aktualizuje dane wyświetlane przez `ListView` i ustawia `IsRefreshing` do `false`.

[ **RssFeed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) przykładzie pokazano użycie [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) implementującej `RefreshCommand` i `IsRefreshing` właściwości do wiązania danych.

## <a name="the-tableview-and-its-intents"></a>TableView i jego intencji

Gdy `ListView` zwykle wyświetla wiele wystąpień tego samego typu [ `TableView` ](xref:Xamarin.Forms.TableView) zwykle skupia się na dostarczanie interfejsu użytkownika dla właściwości wielu różnych typów. Każdy element jest skojarzony z własną [ `Cell` ](xref:Xamarin.Forms.Cell) pochodnych do wyświetlania właściwości lub dostarczanie interfejsu użytkownika do niego.

### <a name="properties-and-hierarchies"></a>Właściwości i hierarchie

`TableView` Definiuje tylko cztery właściwości:

- [`Intent`](xref:Xamarin.Forms.TableView.Intent) typu [ `TableIntent` ](xref:Xamarin.Forms.TableIntent), wyliczenie
- [`Root`](xref:Xamarin.Forms.TableView.Root) typu [ `TableRoot` ](xref:Xamarin.Forms.TableRoot), właściwość zawartości `TableView`
- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) tego typu `int`
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) tego typu `bool`

`TableIntent` Wyliczenia wskazuje, jak zamierzasz używać `TableView`:

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

Te elementy Członkowskie również sugerować do niektórych zastosowań `TableView`.

Definiowanie tabeli obejmuje kilka innych klas:

- [`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase) jest klasą abstrakcyjną, która pochodzi od klasy `BindableObject` i definiuje [ `Title` ](xref:Xamarin.Forms.TableSectionBase.Title) właściwości

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1) jest klasą abstrakcyjną, która pochodzi od klasy `TableSectionBase` i implementuje `IList<T>` i `INotifyCollectionChanged`

- [`TableSection`](xref:Xamarin.Forms.TableSection) pochodzi od klasy `TableSectionBase<Cell>`

- [`TableRoot`](xref:Xamarin.Forms.TableRoot) pochodzi od klasy `TableSectionBase<TableSection>`

Krótko mówiąc `TableView` ma `Root` właściwość, która jest ustawiona na `TableRoot` obiektu, który jest kolekcją z `TableSection` obiektów, z których każdy jest kolekcją `Cell` obiektów. Tabela zawiera wiele sekcji, a każda z nich zawiera wiele komórek. Sama tabela może mieć tytuł, a każda sekcja może mieć tytuł. Mimo że `TableView` sprawia, że użycie `Cell` pochodne, nie sprawia, że użycie `DataTemplate`.

### <a name="a-prosaic-form"></a>Prosaic formularza

[ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) Przykładowa aplikacja definiuje [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) model widoku, wystąpienie staje się `BindingContext` z `TableView`. Każdy `Cell` pochodnych w jego `TableSection` następnie może mieć powiązania z właściwości `PersonalInformation` klasy.

### <a name="custom-cells"></a>Niestandardowe komórek

[ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) rozszerza przykładowe **EntryForm**. [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Klasa zawiera właściwość typu Boolean, które regulują możliwości zastosowania dwa dodatkowe właściwości. W przypadku tych dwóch dodatkowych właściwości program używa niestandardowego `PickerCell` na podstawie [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) i [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) w [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki.

Mimo że `IsEnabled` właściwości dwóch `PickerCell` elementy są powiązane z właściwość logiczna w `ProgrammerInformation`, ta technika nie wydaje się działać, powoduje wyświetlenie monitu o Następna próbka.

### <a name="conditional-sections"></a>Sekcje warunkowe

[ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) przykładowe umieszcza dwa elementy, które są uzależnione od wyboru elementu Boolean w osobnym `TableSection`. Plik związany z kodem spowoduje usunięcie w tej sekcji z `TableView` lub dodaje go ponownie na podstawie właściwości typu Boolean.

### <a name="a-tableview-menu"></a>TableView menu

Używanie innego `TableView` jest menu. [ **MenuCommands** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) przykład pokazuje, menu, który pozwala na przechodzenie nieco `BoxView` na ekranie.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdziale 19 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Przykłady rozdziale 19](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
