---
title: Źródła danych elementu ListView
description: W tym artykule opisano sposób wypełniania elementu ListView platformy Xamarin.Forms z danymi i sposobu użycia powiązanie danych z elementu ListView.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: aa9c23266329c03b3b28c7795f67290bbc23c4bf
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245543"
---
# <a name="listview-data-sources"></a>Źródła danych elementu ListView

Element ListView jest używany do wyświetlania listy danych. Firma Microsoft będzie informacje o wprowadzaniu ListView przy użyciu danych i jak możemy można powiązać z wybranego elementu.

- **[Ustawienie ItemsSource](#ItemsSource)**  &ndash; korzysta z prostej listy lub tablicy.
- **[Powiązanie danych](#Data_Binding)**  &ndash; ustanawia relację między modelu i w elemencie ListView. Powiązanie jest idealny dla wzorca MVVM.

## <a name="itemssource"></a>Właściwość ItemsSource
Element ListView jest wypełniane przy użyciu danych przy użyciu `ItemsSource` właściwość, która może zaakceptować żadnych Implementowanie kolekcji `IEnumerable`. Najprostszym sposobem, aby wypełnić `ListView` polega na użyciu tablicy ciągów:

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};

//monochrome will not appear in the list because it was added
//after the list was populated.
listView.ItemsSource.Add("monochrome");
```

![](data-and-databinding-images/itemssource-simple.png "Wyświetlanie listy ListView ciągów")

Powyższe podejście spowoduje wypełnienie `ListView` z listą parametrów. Domyślnie `ListView` wywoła `ToString` i wyświetlić wyniki w `TextCell` dla każdego wiersza. Aby dostosować sposób wyświetlania danych, zobacz [wygląd komórek](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Ponieważ `ItemsSource` zostało wysłane do tablicy, zawartość nie będzie aktualizowana jako podstawowej zmiany listy lub tablicy. Jeśli chcesz, aby element ListView do automatycznego aktualizowania jako elementy są dodane, usunięte i zmienić na liście podstawowej, musisz użyć `ObservableCollection`. [`ObservableCollection`](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) jest zdefiniowany w `System.Collections.ObjectModel` i jest tak jak `List`, ale mogą wyświetlać powiadomienia `ListView` wszelkich zmian:

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>Powiązanie danych
Powiązanie danych jest "sklejki", który wiąże właściwości niektórych obiektu CLR, takich jak klasy w Twojej ViewModel właściwości obiektu interfejsu użytkownika. Powiązanie danych jest przydatne, ponieważ takie rozwiązanie upraszcza projektowanie interfejsów użytkownika przez zastąpienie dużo nudnych schematyczny kod.

Powiązanie danych polega na synchronizacja obiektów po zmianie wartości powiązanej. Nie trzeba zapisać obsługi zdarzeń za każdym razem, gdy zmienia się wartość formantu, ustanowić powiązanie i włączyć powiązania w Twojej ViewModel.

Aby uzyskać więcej informacji w powiązaniu danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) który stanowi część cztery [podstawy XAML platformy Xamarin.Forms artykuł z serii](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Powiązanie komórki
Właściwości komórki (i elementów podrzędnych komórek) może być powiązana z właściwości obiektów w `ItemsSource`. Na przykład ListView mogą służyć do prezentowania listy pracowników z obrazami.

Klasa pracowników:

```csharp
public class Employee{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` zostanie utworzona i ustawiona jako `ListView`w `ItemsSource`:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public EmployeeListPage()
{
  //defined in XAML to follow
  EmployeeView.ItemsSource = employees;
  ...
}
```

Listy są wypełniane przy użyciu danych:

```csharp
public EmployeeListPage()
{
  ...
  employees.Add(new Employee{ DisplayName="Rob Finnerty"});
  employees.Add(new Employee{ DisplayName="Bill Wrestler"});
  employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
  employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
  employees.Add(new Employee{ DisplayName="Sheri Spruce"});
  employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

Poniższy fragment kodu przedstawia `ListView` powiązany z listy pracowników:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
Title="Employee List">
  <ListView x:Name="EmployeeView">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Należy pamiętać, że powiązanie zostało skonfigurowane w kodzie dla uproszczenia, mimo że można został powiązany w języku XAML.

Określa poprzednie bit XAML `ContentPage` zawierający `ListView`. Źródło danych `ListView` ustawione za pośrednictwem `ItemsSource` atrybutu. Układ każdego wiersza w `ItemsSource`jest zdefiniowany w `ListView.ItemTemplate` elementu.

Jest to wynik:

![](data-and-databinding-images/bound-data.png "Element ListView przy użyciu powiązania danych")

### <a name="binding-selecteditem"></a>Powiązanie SelectedItem

Często należy powiązać z wybranego elementu `ListView`, zamiast niż używanie programu obsługi zdarzeń do reagowania na zmiany. Aby to zrobić w języku XAML, powiązać `SelectedItem` właściwości:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

Zakładając, że `listView`w `ItemsSource` znajduje się lista ciągów, `SomeLabel` będzie mieć swoją właściwość tekst powiązany z `SelectedItem`.



## <a name="related-links"></a>Linki pokrewne

- [Dwa wiążących (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [informacje o wersji 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [informacje o wersji 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
