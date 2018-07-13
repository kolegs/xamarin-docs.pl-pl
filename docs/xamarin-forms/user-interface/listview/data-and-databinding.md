---
title: Źródła danych ListView
description: W tym artykule opisano sposób wypełnienia ListView Xamarin.Forms przy użyciu danych oraz powiązanie danych za pomocą ListView.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 17c353844a7ddc808e5d9f0632434472913170a4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995209"
---
# <a name="listview-data-sources"></a>Źródła danych ListView

ListView służy do wyświetlania listy danych. Poznasz wypełnianie ListView przy użyciu danych i jak możemy powiązać wybranego elementu.

- **[Ustawienie ItemsSource](#ItemsSource)**  &ndash; używa prostej listy lub tablicy.
- **[Powiązanie danych](#Data_Binding)**  &ndash; ustanawia relację między modelem i ListView. Powiązanie jest idealny dla wzorca MVVM.

## <a name="itemssource"></a>ItemsSource
ListView jest wypełniana przy użyciu danych za pomocą `ItemsSource` właściwość, która może akceptować żadnych kolekcji Implementowanie `IEnumerable`. Najprostszym sposobem, aby wypełnić `ListView` polega na użyciu tablicy ciągów:

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

![](data-and-databinding-images/itemssource-simple.png "ListView wyświetlanie listy ciągów")

Powyższe podejście zostanie wypełniony `ListView` z listą ciągów. Domyślnie `ListView` wywoła `ToString` i wyświetlić wyniki w `TextCell` dla każdego wiersza. Aby dostosować sposób wyświetlania danych, zobacz [wygląd komórki](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Ponieważ `ItemsSource` została wysłana do tablicy, zawartość nie będzie aktualizowana jako podstawowej zmiany listy lub tablicy. Chcąc ListView można automatycznie zaktualizować, ponieważ elementy są dodane, usunięte i zmienić na liście podstawowej, musisz użyć `ObservableCollection`. [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) jest zdefiniowany w `System.Collections.ObjectModel` i podobnie jak `List`, z tą różnicą, że może ona generować powiadomienia `ListView` o wszelkich zmianach:

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>Powiązanie danych
Wiązanie danych jest "skleić", który wiąże właściwości obiektu interfejsu użytkownika do właściwości niektóre obiektu CLR, na przykład klasa w swojej ViewModel. Wiązanie danych jest przydatne, ponieważ upraszcza tworzenie interfejsów użytkownika, zastępując wiele zadań żmudnych standardowy kod.

Powiązanie danych działa synchronizacja obiektów po zmianie ich powiązanych wartości. Zamiast pisać programy obsługi zdarzeń dla za każdym razem, gdy zmieni się wartość kontrolki, można określić powiązanie i powiązanie w swojej ViewModel.

Aby uzyskać więcej informacji na temat wiązania danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) którego jest czwartą częścią [seria artykułów podstawy XAML zestawu narzędzi Xamarin.Forms](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Powiązanie komórki
Właściwości komórki (i ich elementów podrzędnych komórek) może być powiązana z właściwości obiektów w `ItemsSource`. Na przykład ListView może służyć do prezentowania listę pracowników przy użyciu obrazów.

Klasa pracowników:

```csharp
public class Employee{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` zostanie utworzony i Ustaw jako `ListView`firmy `ItemsSource`:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public EmployeeListPage()
{
  //defined in XAML to follow
  EmployeeView.ItemsSource = employees;
  ...
}
```

Lista jest wypełniana danymi:

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

Należy pamiętać, że powiązanie zostało skonfigurowane w kodzie dla uproszczenia, mimo że można mieć granicy w XAML.

Poprzednie bitu XAML definiuje `ContentPage` zawierający `ListView`. Źródło danych `ListView` jest ustawiony za pomocą `ItemsSource` atrybutu. Układ każdego wiersza w `ItemsSource`jest zdefiniowany w obrębie `ListView.ItemTemplate` elementu.

Jest to wynik:

![](data-and-databinding-images/bound-data.png "ListView przy użyciu powiązania danych")

### <a name="binding-selecteditem"></a>Powiązanie SelectedItem

Często należy powiązać z wybranego elementu `ListView`, a nie reagować na zmiany przy użyciu programu obsługi zdarzeń. Aby to zrobić w XAML, powiąż `SelectedItem` właściwości:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

Zakładając, że `listView`firmy `ItemsSource` znajduje się lista ciągów, `SomeLabel` będzie mieć właściwości text powiązany z `SelectedItem`.



## <a name="related-links"></a>Linki pokrewne

- [Dwa wiążących (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [informacje o wersji 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [informacje o wersji 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
