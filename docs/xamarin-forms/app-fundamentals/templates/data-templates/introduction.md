---
title: Wprowadzenie do szablonów danych zestawu narzędzi Xamarin.Forms
description: Szablony danych zestawu narzędzi Xamarin.Forms zapewniają możliwość definiowania prezentacji danych na obsługiwanych formantów. Ten artykuł zawiera wprowadzenie do szablonów danych, sprawdzając, dlatego ich użycie jest konieczne.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 129ce7a04b93bfb3cb1b9a1639aee61cd56d09d5
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998923"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Wprowadzenie do szablonów danych zestawu narzędzi Xamarin.Forms

_Szablony danych zestawu narzędzi Xamarin.Forms zapewniają możliwość definiowania prezentacji danych na obsługiwanych formantów. Ten artykuł zawiera wprowadzenie do szablonów danych, sprawdzając, dlatego ich użycie jest konieczne._

Należy wziąć pod uwagę [ `ListView` ](xref:Xamarin.Forms.ListView) wyświetlającą zbiór `Person` obiektów. W poniższym przykładzie kodu pokazano definicję `Person` klasy:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person` Klasa definiuje `Name`, `Age`, i `Location` właściwości, które można ustawić podczas `Person` obiekt zostanie utworzony. [ `ListView` ](xref:Xamarin.Forms.ListView) Służy do wyświetlania kolekcję `Person` obiekty, jak pokazano w poniższym przykładzie kodu XAML:

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

Elementy są dodawane do [ `ListView` ](xref:Xamarin.Forms.ListView) w XAML, inicjując [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) właściwości przy użyciu tablicy `Person` wystąpień.

> [!NOTE]
> Należy pamiętać, że `x:Array` element wymaga `Type` atrybut wskazujący typ elementów w tablicy.

Pokazano w poniższym przykładzie kodu, który inicjuje równoważne stronę C# [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) właściwości `List` z `Person` wystąpień:

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

[ `ListView` ](xref:Xamarin.Forms.ListView) Wywołania `ToString` przy wyświetlaniu obiekty w kolekcji. Ponieważ nie istnieje żadne `Person.ToString` przesłonięcia `ToString` zwraca nazwę typu każdego obiektu, jak pokazano na poniższych zrzutach ekranu:

![](introduction-images/no-data-template.png "ListView bez szablonu danych")

`Person` Obiektu można zastąpić `ToString` metodę w celu wyświetlenia istotnych danych, jak pokazano w poniższym przykładzie kodu:

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

Skutkuje to [ `ListView` ](xref:Xamarin.Forms.ListView) wyświetlanie `Person.Name` wartości właściwości dla każdego obiektu w kolekcji, jak pokazano na poniższych zrzutach ekranu:

![](introduction-images/override-tostring.png "ListView przy użyciu szablonu danych")

`Person.ToString` Zastąpienie może zwrócić sformatowany ciąg składający się z `Name`, `Age`, i `Location` właściwości. Jednak to podejście zapewnia ograniczoną kontrolę wygląd każdego elementu danych. W celu zwiększenia elastyczności [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) mogą być tworzone która definiuje wygląd elementów danych.

## <a name="creating-a-datatemplate"></a>Tworzenie DataTemplate

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) służy do określania wyglądu danych i zazwyczaj używa powiązanie danych do wyświetlania danych. Jego typowy scenariusz użycia jest podczas wyświetlania danych z kolekcji obiektów w [ `ListView` ](xref:Xamarin.Forms.ListView). Na przykład, gdy `ListView` jest powiązana z kolekcją `Person` obiektów `ListView.ItemTemplate` będzie można ustawić właściwości `DataTemplate` definiuje wygląd każdego `Person` obiektu `ListView`. `DataTemplate` Będzie zawierać elementy, które należy powiązać wartości właściwości każdego `Person` obiektu. Aby uzyskać więcej informacji na temat tworzenia powiązań danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) może służyć jako wartości następujących właściwości:

- [`ListView.HeaderTemplate`](xref:Xamarin.Forms.ListView.HeaderTemplate)
- [`ListView.FooterTemplate`](xref:Xamarin.Forms.ListView.FooterTemplate)
- [`ListView.GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate)
- [`ItemsView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1), która jest dziedziczona przez [ `ListView` ](xref:Xamarin.Forms.ListView).
- [`MultiPage.ItemTemplate`](xref:Xamarin.Forms.MultiPage`1), która jest dziedziczona przez [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), i [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage).

> [!NOTE]
> Należy pamiętać, że chociaż [ `TableView` ](xref:Xamarin.Forms.TableView) sprawia, że przypadki użycia [ `Cell` ](xref:Xamarin.Forms.Cell) obiektów, używaj [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Jest to spowodowane powiązań danych są zawsze ustawiane bezpośrednio na `Cell` obiektów.

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) znajduje jako element podrzędny elementu wymienionych powyżej właściwości jest znany jako *wbudowany szablon*. Alternatywnie `DataTemplate` mogą być definiowane jako zasób poziom kontroli, na poziomie strony lub dodatku poziomu aplikacji. Wybieranie miejsce definiowania [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) na środowisko, w których można użyć:

- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) zdefiniowane podczas kontroli poziomu będzie stosowany tylko do kontrolki.
- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) zdefiniowane na stronie poziomu można zastosować do wielu prawidłowe formantów na stronie.
- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) zdefiniowane na poziomie aplikacji mogą być stosowane w przypadku prawidłowe kontrolek w całej aplikacji.

Szablony danych niżej w hierarchii widoku mają pierwszeństwo względem identyczne ze zdefiniowanymi wyższej się po udostępnieniu `x:Key` atrybutów. Na przykład szablonu usługi danych na poziomie aplikacji, zostanie ona zastąpiona szablonu danych na poziomie strony i szablon danych na poziomie strony zostaną zastąpione przez szablon poziom kontroli danych lub wbudowany szablon danych.


## <a name="related-links"></a>Linki pokrewne

- [Wygląd komórki](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Szablony danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
