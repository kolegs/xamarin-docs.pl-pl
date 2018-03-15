---
title: "Podsumowanie rozdziale 19. Kolekcja widoków"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a085eb306ad81b3c9214df269f69558bc8fbfaa7
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-19-collection-views"></a>Podsumowanie rozdziale 19. Kolekcja widoków

Platformy Xamarin.Forms definiuje trzy widoki, które utrzymują kolekcje i wyświetlanie ich elementów:

- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) jest stosunkowo krótkich listy elementów ciąg, który umożliwia użytkownikowi wybranie jednego
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) jest często długą listę elementów, które zwykle tego samego typu i formatowania, zmieniona przez użytkownika, można wybrać jeden
- [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) jest to zbiór *komórek* (zazwyczaj z różnych typów i wyglądu) do wyświetlania danych lub zarządzać dane wejściowe użytkownika

Często MVVM do użycia przez aplikacje `ListView` do wyświetlenia wybieranych kolekcję obiektów.

## <a name="program-options-with-picker"></a>Opcje programu za pomocą selektora

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Jest dobrym rozwiązaniem, gdy należy zezwolić użytkownikowi na wybranie opcji spośród stosunkowo krótką listę `string` elementów.

### <a name="the-picker-and-event-handling"></a>Selektor i obsługa zdarzeń

[ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) przykładowych pokazano, jak użyć XAML, aby ustawić `Picker` [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) właściwości i dodać `string` elementów do [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekcji. Gdy użytkownik wybierze `Picker`, zawiera elementy `Items` kolekcji w sposób zależny od platformy.

[ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Zdarzeń wskazuje, kiedy użytkownik wybrał element. Liczony od zera [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) właściwość wskazuje następnie wybranego elementu. Jeśli nie wybrano elementów, `SelectedIndex` jest równe &ndash;1.

Można również użyć `SelectedIndex` zainicjować wybranego elementu, ale musi być ustawiona po `Items` kolekcji jest wypełnione. W języku XAML, oznacza to, że prawdopodobnie użyjesz elementu właściwości można ustawić `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Selektor powiązanie danych

`SelectedIndex` Właściwość nie jest obsługiwana przez właściwości możliwej do wiązania, ale `Items` nie jest, dlatego używanie powiązania danych z `Picker` jest trudne. Jeden rozwiązaniem jest użycie `Picker` w połączeniu z [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) znajdującego się w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki. [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) pokazano, jak to działa.

## <a name="rendering-data-with-listview"></a>Renderowanie danych z widoku listy.

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Jest to jedyna klasa, która jest pochodną [ `ItemsView<TVisual>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) z której ten dziedziczy [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) i [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) właściwości.

`ItemsSource` Typ jest `IEnumerable` , ale jest `null` domyślnie i musi być jawnie zainicjowany lub ustaw (częściej) do kolekcji za pośrednictwem powiązania danych. Elementy w tej kolekcji może być dowolnego typu.

`ListView` definiuje [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) właściwość, która jest albo ustaw na jeden z elementów w `ItemsSource` kolekcji lub `null` Jeśli nie wybrano elementu. `ListView` uruchamiany [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) zdarzenie, gdy nowy element jest zaznaczony.

### <a name="collections-and-selections"></a>Wybrane i kolekcji

[ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) przykładowe wypełnienia `ListView` z 17 `Color` wartości w `List<Color>` kolekcji. Elementy są możliwy, ale domyślnie są wyświetlane z ich nieatrakcyjnych `ToString` oświadczenia. Kilka przykładów w tym rozdziale pokazują, jak rozwiązać ten ekran i udostępnić go jako atrakcyjne zgodnie z potrzebami.

### <a name="the-row-separator"></a>Separator wierszy

W systemach iOS i Android Wyświetla alokowania wiersza oddziela wiersze. Można sterować tym [ `SeparatorVisibiliy` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorVisibility/) i [ `SeparatorColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorColor/) właściwości. `SeparatorVisibility` Właściwość jest typu [ `SeparatorVisbility` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SeparatorVisibility/), wyliczenie z dwóch elementów:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.Default/), ustawienie domyślne
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.None/)

### <a name="data-binding-the-selected-item"></a>Wybrany element powiązania danych

`SelectedItem` Właściwość nie jest obsługiwana przez właściwości możliwej do wiązania, więc może być serwerem źródłowym lub docelowym powiązania danych. Domyślnie `BindingMode` jest `OneWayToSource`, ale zazwyczaj jest to element docelowy powiązania danych dwukierunkowego, szczególnie w sytuacjach MVVM. [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) przykładzie pokazano tego typu powiązania.

### <a name="the-observablecollection-difference"></a>Różnica obiektu ObservableCollection

[ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) przykładowe zestawy `ItemsSource` właściwość `ListView` do `List<DateTime>` kolekcji, a następnie stopniowo dodaje nową `DateTime` obiektu do kolekcji co drugi za pomocą czasomierza.

Jednak `ListView` automatycznie nie automatycznie zaktualizowana, ponieważ `List<T>` kolekcji nie ma mechanizm powiadomień, aby wskazać, gdy elementy są dodane lub usunięte z kolekcji.

Jest znacznie lepszą klasę do użycia w takich scenariuszach [ `ObservableCollection<T>` ](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) zdefiniowane w `System.Collections.ObjectModel` przestrzeni nazw. Ta klasa implementuje [ `INotifyCollectionChanged` ](https://developer.xamarin.com/api/type/System.Collections.Specialized.INotifyCollectionChanged/) interfejsu i w związku z tym uruchamiany [ `CollectionChanged` ](https://developer.xamarin.com/api/event/System.Collections.ObjectModel.ObservableCollection%3CT%3E.CollectionChanged/) zdarzenie, gdy elementy są dodane lub usunięte z kolekcji lub, gdy są one zastąpione lub przenoszone w Kolekcja. Podczas `ListView` wewnętrznie wykrycia, że implementacja klasy `INotifyCollectionChanged` została ustawiona jako jego `ItemsSource` właściwości, dołącza program obsługi do `CollectionChanged` zdarzeń i aktualizuje jego wyświetlania po dokonaniu zmiany w kolekcji.

[ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) przykładzie przedstawiono użycie `ObservableCollection`.

### <a name="templates-and-cells"></a>Szablony i komórek

Domyślnie `ListView` elementy są wyświetlane w jego kolekcji przy użyciu każdego elementu `ToString` metody. Lepszym rozwiązaniem obejmuje definiowania szablonu, aby wyświetlić elementy.

Aby wypróbować tę funkcję, można użyć [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki. Ta klasa definiuje statycznego `All` właściwości typu `IList<NamedColor>` zawierający 141 `NamedColor` obiektów odpowiadającego pola publicznego `Color` struktury.

[ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) przykładowe zestawy `ItemsSource` z `ListView` tej `NamedColor.All` właściwości, ale tylko klasy w pełni kwalifikowanej nazwy `NamedColor` obiektów wyświetlane.

`ListView` wymaga szablonu, aby wyświetlić te elementy. W kodzie, można ustawić [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) właściwości zdefiniowane przez `ItemsView<TVisual>` do [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) przy użyciu [ `DataTemplate` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) który odwołuje się do pochodną [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) klasy. `Cell` zawiera pięć pochodne:

- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) &mdash; zawiera dwa `Label` widoków (koncepcyjnie mówiąc)
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) &mdash; dodaje `Image` widok do `TextCell`
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) &mdash; zawiera `Entry` widok z `Label`
- [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) &mdash; zawiera `Switch` z `Label`
- [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) &mdash; może to być dowolna `View` (prawdopodobnie z elementami podrzędnymi)

Następnie wywołaj [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) i [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) na `DataTemplate` powiązania wartości z obiektu `Cell` właściwości, lub w celu ustawienia wiązania danych na `Cell` właściwości odwołania do właściwości elementów w `ItemsSource` kolekcji. To jest przedstawiona w [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) próbki.

Ponieważ każdy element jest wyświetlany przez `ListView`małych drzewa wizualnego jest tworzony na podstawie szablonu i powiązania danych są połączenia między elementem i właściwości elementów w tym drzewie wizualnym. Informacje o tym, proces ten można uzyskać, instalując programy obsługi dla [ `ItemAppearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemAppearing/) i [ `ItemDisappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemDisappearing/) zdarzenia `ListView`, lub za pomocą zamiast [ `DataTemplate`Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Func%7BSystem.Object%7D/) używającą funkcji, która jest wywoływana za każdym razem, należy utworzyć element drzewa wizualnego.

[ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) zawiera program funkcjonalnie identyczne wyłącznie w języku XAML. A `DataTemplate` ma ustawioną wartość tagu `ItemTemplate` właściwość `ListView`, a następnie `TextCell` ma ustawioną wartość `DataTemplate`. Powiązania z właściwości elementów w kolekcji są ustawiane bezpośrednio na [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) i [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) właściwości `TextCell`.

### <a name="custom-cells"></a>Niestandardowe komórek

W języku XAML jest możliwość ustawienia [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) do `DataTemplate` , a następnie definiuje niestandardowe drzewo wizualne jako [ `View` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ViewCell.View/) właściwość `ViewCell`. (`View` jest właściwość content `ViewCell` więc `ViewCell.View` tagi nie są wymagane.) [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) przykładzie pokazano tej techniki:

[![Potrójna zrzut ekranu przedstawiający listę o nazwie kolorów niestandardowych](images/ch19fg11-small.png "niestandardowa o nazwie listy kolor")](images/ch19fg11-large.png#lightbox "listy o nazwie kolorów niestandardowych")

Może być trudnych pobieranie rozmiaru prawa dla wszystkich platform. [ `RowHeight` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RowHeight/) Właściwość jest przydatna, ale w niektórych przypadkach można odwołać się do [ `HasUnevenRows` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HasUnevenRows/) właściwość, która jest mniej wydajne, ale wymusza `ListView` rozmiar wierszy. Systemy iOS i Android muszą używać jednej z tych dwóch właściwości można pobrać rozmiar prawidłowego wiersza.

### <a name="grouping-the-listview-items"></a>Grupowanie elementów ListView

`ListView` obsługuje grupowania elementów i nawigacja pomiędzy tych grup. `ItemsSource` Musi być ustawiona właściwość kolekcji kolekcje: obiekt który `ItemsSource` ustawiono musi implementować `IEnumerable`, i każdego elementu w kolekcji musi implementować też `IEnumerable`. Każda grupa powinna zawierać dwie właściwości: opis grupy oraz skrót trzyliterowy.

[ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) Klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki tworzy siedem grup `NamedColor` obiektów. [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) przykładowych pokazano, jak używać tych grup z [ `IsGroupingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsGroupingEnabled/) właściwość `ListView` ustawioną `true`i [ `GroupDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) i [ `GroupShortNameBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) właściwości powiązany z właściwości w każdej grupie.

### <a name="custom-group-headers"></a>Nagłówki niestandardowe grupy

Istnieje możliwość tworzenia niestandardowych nagłówki `ListView` grup, zastępując `GroupDisplayBinding` właściwości o [ `GroupHeaderTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/) Definiowanie szablonu dla nagłówków.

### <a name="listview-and-interactivity"></a>ListView i interakcji

Zwykle aplikacja uzyskuje interakcji użytkowników z `ListView` przez dołączenie obsługi ma `ItemSelected` lub [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) zdarzeń, lub przez ustawienie wiązania z danymi na `SelectedItem` właściwości. Jednak niektóre typy komórki (`EntryCell` i `SwitchCell`) zezwalają na interakcję użytkownika, a także możliwe do tworzenia niestandardowych komórki tego interakcji z użytkownikiem. [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) tworzy wystąpienia 100 [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) i umożliwia użytkownikowi zmianę każdego kolorów przy użyciu Trójka `Slider` elementów. Program również sprawia, że użycie [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) w [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>Element ListView i MVVM

`ListView` pełni rolę big w scenariuszach MVVM. Zawsze, gdy `IEnumerable` w ViewModel istnieje w kolekcji, często jest powiązany z `ListView`. Ponadto wdrożenie elementów w kolekcji często `INotifyPropertyChanged` powiązania z właściwości w szablonie.

### <a name="a-collection-of-viewmodels"></a>Kolekcja ViewModels

Aby zapoznać się z tym [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) biblioteki tworzy kilka klas na podstawie [pliku danych XML](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) i obrazy fikcyjne studentów w tym szkole fikcyjne.

[ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) Pochodną klasy [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) Klasy jest kolekcją `Student` obiekty i również pochodzi z `ViewModelBase`. [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) Pobiera plik XML i składana wszystkich obiektów.

[ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) program używa `ImageCell` do wyświetlania studentów i obrazów w `ListView`:

[![Potrójna zrzut ekranu przedstawiający listę uczniów](images/ch19fg18-small.png "Lista studentów/uczniów")](images/ch19fg18-large.png#lightbox "Lista studentów/uczniów")

[ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) dodaje próbki [ `Header` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.Header/) właściwość, ale tylko zostaną wyświetlone w systemie Android.

### <a name="selection-and-the-binding-context"></a>Wybór i kontekstu powiązania

[ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) program wiązania `BindingContext` z `StackLayout` do `SelectedItem` właściwość `ListView`. Dzięki temu program wyświetlić szczegółowe informacje na temat wybranego uczniów.

### <a name="context-menus"></a>Menu kontekstowe

Komórki można zdefiniować menu kontekstowego, która jest zaimplementowana w sposób specyficzne dla platformy. Aby utworzyć tego menu, Dodaj [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) obiekty do [ `ContextActions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) właściwość `Cell`.

`MenuItem` Definiuje właściwości pięć:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) typu `string`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) typu `FileImageSource`
- [`IsDestructive`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.IsDestructive/) typu `bool`
- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Command/) typu `ICommand`
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.CommandParameter/) typu `object`

`Command` i `CommandParameter` właściwości oznacza, że ViewModel dla każdego elementu zawiera metody do wykonywania żądanego polecenia. W scenariuszach z systemem innym niż MVVM `MenuItem` definiuje również [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) zdarzeń.

[ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) pokazuje tej metody. `Command` Właściwości każdego `MenuItem` jest powiązany z właściwością typu `ICommand` w `Student` klasy. Ustaw `IsDestructive` właściwości `true` dla `MenuItem` usuwa albo Usuwa wybrany obiekt.

### <a name="varying-the-visuals"></a>Zróżnicowanie wizualnych

Czasami trzeba będzie wahań niewielką wizualnych elementów w `ListView` na podstawie właściwości. Na przykład gdy średniej klasy punktu studenta spada poniżej 2.0, [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) próbki wyświetla nazwę tego uczniów kolorem czerwonym.
Odbywa się za pomocą konwertera wartości powiązania, [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki.

### <a name="refreshing-the-content"></a>Odświeżanie zawartości

`ListView` Obsługuje gestu rozwijanego odświeżanie danych. Należy ustawić program [ `IsPullToRefresh` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsPullToRefreshEnabled/) właściwości `true` aby je włączyć. `ListView` Odpowiada gestu rozwijanego przez ustawienie jej [ `IsRefreshing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsRefreshing/) właściwości `true`i podnosząc [ `Refreshing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.Refreshing/) zdarzenia i (w przypadku scenariuszy MVVM) wywoływania `Execute` metoda jego [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) właściwości.

Kod obsługi `Refresh` zdarzenia lub `RefreshCommand` prawdopodobnie aktualizuje dane wyświetlane przez `ListView` i ustawia `IsRefreshing` do `false`.

[ **RssFeed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) przykładzie pokazano, za pomocą [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) implementującej `RefreshCommand` i `IsRefreshing` właściwości dla powiązania danych.

## <a name="the-tableview-and-its-intents"></a>TableView i jego lokalizacji docelowych

Gdy `ListView` zwykle zawiera wiele wystąpień tego samego typu [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) zazwyczaj jest ukierunkowana na zapewnienie interfejsu użytkownika dla właściwości wielu różnych typów. Każdy element jest skojarzony z własną [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) pochodnych do wyświetlania właściwości lub udostępnia interfejs użytkownika do niego.

### <a name="properties-and-hierarchies"></a>Właściwości i hierarchii

`TableView` Definiuje właściwości tylko cztery:

- [`Intent`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Intent/) typu [ `TableIntent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableIntent/), — wyliczenie
- [`Root`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) typu [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/), właściwość zawartości `TableView`
- [`RowHeight`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.RowHeight/) typu `int`
- [`HasUnevenRows`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.HasUnevenRows/) typu `bool`

`TableIntent` Wyliczenie wskazuje, jak zamierzasz używać `TableView`:

- [`Data`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Data/)
- [`Form`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Form/)
- [`Settings`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Settings/)
- [`Menu`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Menu/)

Te elementy Członkowskie również sugerować niektóre zastosowania `TableView`.

Definiowanie tabeli obejmuje kilka innych klas:

- [`TableSectionBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase/) jest klasą abstrakcyjną, która jest pochodną `BindableObject` i definiuje [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableSectionBase.Title/) właściwości

- [`TableSectionBase<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase%3CT%3E/) jest klasą abstrakcyjną, która jest pochodną `TableSectionBase` i implementuje `IList<T>` i `INotifyCollectionChanged`

- [`TableSection`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) pochodną `TableSectionBase<Cell>`

- [`TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/) pochodną `TableSectionBase<TableSection>`

Krótko mówiąc `TableView` ma `Root` właściwości ustawionej dla `TableRoot` obiektu, który jest kolekcją z `TableSection` obiektów, z których każdy jest kolekcją `Cell` obiektów. Tabela ma wiele sekcji, a każda sekcja ma wiele komórek. Sama tabela może mieć tytuł i każdej sekcji można mieć tytuł. Mimo że `TableView` sprawia, że użycie `Cell` pochodne, nie powoduje ona użycie `DataTemplate`.

### <a name="a-prosaic-form"></a>Prosaic formularza

[ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) definiuje próbki [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) modelu widoku, wystąpienie staje się `BindingContext` z `TableView`. Każdy `Cell` pochodnych w jego `TableSection` następnie może mieć powiązania z właściwości `PersonalInformation` klasy.

### <a name="custom-cells"></a>Niestandardowe komórek

[ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) Przykładowe rozszerzenie **EntryForm**. [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Klasy zawiera właściwości typu Boolean określająca sposób stosowania dwóch dodatkowych właściwości. Dla tych dwóch dodatkowych właściwości program używa niestandardowego `PickerCell` na podstawie [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) i [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) w [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki.

Mimo że `IsEnabled` właściwości dwóch `PickerCell` elementy są powiązane z właściwości typu Boolean w `ProgrammerInformation`, ta metoda prawdopodobnie nie działa, które monituje Następna próbka.

### <a name="conditional-sections"></a>Sekcje warunkowe

[ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) próbki umieszcza dwa elementy, które są uzależnione od wyboru Boolean elementu w oddzielnej `TableSection`. W tej sekcji z usuwa plik CodeBehind `TableView` lub dodaje go ponownie na podstawie właściwości typu Boolean.

### <a name="a-tableview-menu"></a>TableView menu

Użycie innego `TableView` jest menu. [ **MenuCommands** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) przykładzie pokazano menu, która umożliwia przenoszenie nieco `BoxView` po ekranie.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdziale 19 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Przykłady rozdziale 19](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
