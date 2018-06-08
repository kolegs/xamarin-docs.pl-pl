---
title: Wprowadzenie
description: Szablony danych platformy Xamarin.Forms zapewniają możliwość definiowania prezentację danych na obsługiwanych formantów. Ten artykuł zawiera wprowadzenie do szablonów danych, sprawdzając, dlatego ich użycie jest konieczne.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: beb1633919a1d4d5bc2f5a8b0452b535a5cc8f3f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847120"
---
# <a name="introduction"></a>Wprowadzenie

_Szablony danych platformy Xamarin.Forms zapewniają możliwość definiowania prezentację danych na obsługiwanych formantów. Ten artykuł zawiera wprowadzenie do szablonów danych, sprawdzając, dlatego ich użycie jest konieczne._

Należy wziąć pod uwagę [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) który wyświetla kolekcję `Person` obiektów. Poniższy przykładowy kod przedstawia definicję `Person` klasy:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person` Klasa definiuje `Name`, `Age`, i `Location` właściwości, które można ustawić podczas obliczania `Person` tworzony jest obiekt. [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Służy do wyświetlania Kolekcja `Person` obiekty, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataTemplates"
             ...>
    <StackLayout Margin="20">
        ...
        <ListView Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    <local:Person Name="John" Age="37" Location="USA" />
                    <local:Person Name="Tom" Age="42" Location="UK" />
                    <local:Person Name="Lucas" Age="29" Location="Germany" />
                    <local:Person Name="Tariq" Age="39" Location="UK" />
                    <local:Person Name="Jane" Age="30" Location="USA" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

Elementy są dodawane do [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) w języku XAML, inicjując [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) właściwości z tablicy `Person` wystąpień.

> [!NOTE]
> Należy pamiętać, że `x:Array` wymaga elementu `Type` atrybut wskazujący typ elementów w tablicy.

Odpowiedniej strony C# przedstawiono w poniższym przykładzie kodu, który inicjuje [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) właściwości `List` z `Person` wystąpień:

```csharp
public WithoutDataTemplatePageCS()
{
    ...
    var people = new List<Person>
    {
        new Person { Name = "Steve", Age = 21, Location = "USA" },
        new Person { Name = "John", Age = 37, Location = "USA" },
        new Person { Name = "Tom", Age = 42, Location = "UK" },
        new Person { Name = "Lucas", Age = 29, Location = "Germany" },
        new Person { Name = "Tariq", Age = 39, Location = "UK" },
        new Person { Name = "Jane", Age = 30, Location = "USA" }
    };

    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children = {
            ...
            new ListView { ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
        }
    };
}
```

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Wywołania `ToString` podczas wyświetlania obiektów w kolekcji. Ponieważ nie istnieje żadne `Person.ToString` zastąpić, `ToString` zwraca nazwę typu każdego obiektu, jak pokazano na poniższych zrzutach ekranu:

![](introduction-images/no-data-template.png "Element ListView bez szablonu danych")

`Person` Obiektu można zastąpić `ToString` metodę w celu wyświetlenia istotnych danych, jak pokazano w poniższym przykładzie:

```csharp
public class Person
{
  ...
  public override string ToString ()
  {
    return Name;
  }
}
```

Powoduje to [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wyświetlanie `Person.Name` wartości właściwości dla każdego obiektu w kolekcji, jak pokazano na poniższych zrzutach ekranu:

![](introduction-images/override-tostring.png "Element ListView przy użyciu szablonu danych")

`Person.ToString` Zastąpienie może zwrócić sformatowany ciąg składający się z `Name`, `Age`, i `Location` właściwości. Jednak takie podejście ma tylko ograniczoną kontrolę nad wyglądem każdego elementu danych. Aby uzyskać większą elastyczność [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) można tworzyć definiuje wygląd danych.

## <a name="creating-a-datatemplate"></a>Tworzenie DataTemplate

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) służy do określania wyglądu danych i zwykle wykorzystuje wiązania danych do wyświetlania danych. Jest jego typowy scenariusz użycia przy wyświetlaniu danych z kolekcji obiektów w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Na przykład, jeśli `ListView` jest powiązana z kolekcją `Person` obiektów, `ListView.ItemTemplate` właściwość zostanie ustawiona do `DataTemplate` która definiuje wygląd każdego `Person` obiektu w `ListView`. `DataTemplate` Będzie zawierać elementy, które wiążą się wartości właściwości każdego `Person` obiektu. Aby uzyskać więcej informacji na temat wiązania danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) można użyć jako wartości następujących właściwości:

- [`ListView.HeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HeaderTemplate/)
- [`ListView.FooterTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.FooterTemplate/)
- [`ListView.GroupHeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/)
- [`ItemsView.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/), która jest dziedziczona przez [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/).
- [`MultiPage.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), która jest dziedziczona przez [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), i [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/).

> [!NOTE]
> Należy pamiętać, że chociaż [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) używa [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) obiektów, nie używa [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Wynika to z faktu powiązania danych jest zawsze ustawiona bezpośrednio na `Cell` obiektów.

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) umieszczono jako element podrzędny elementu wymienionych powyżej właściwości nosi nazwę *wbudowanej szablonu*. Alternatywnie `DataTemplate` można zdefiniować jako zasób poziom kontroli, poziomu strony lub na poziomie aplikacji. Wybieranie miejsce definiowania [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) wpływu, których można użyć:

- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) zdefiniowane w formancie poziom można stosować do formantu.
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) zdefiniowane na stronie poziomu można zastosować do wielu prawidłowy formantów na stronie.
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) zdefiniowane na poziomie aplikacji może być zastosowane do prawidłowy formantów w całej aplikacji.

Szablony danych niżej w hierarchii widoku pierwszeństwo wyższy określone się po udostępnieniu `x:Key` atrybutów. Na przykład szablon danych na poziomie aplikacji zostaną zastąpione przez szablon danych na poziomie strony, a szablon danych na poziomie strony zostaną zastąpione przez szablon poziom kontroli danych lub szablon danych wbudowanego.


## <a name="related-links"></a>Linki pokrewne

- [Wygląd komórki](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Szablony danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
