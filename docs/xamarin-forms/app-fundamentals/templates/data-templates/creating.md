---
title: Tworzenie DataTemplate
description: Szablony danych można tworzyć w tekście, ResourceDictionary, lub z typu niestandardowego lub odpowiedniego typu komórki platformy Xamarin.Forms. Ten artykuł opisuje każdego technik.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: cd4266fb8e7d68a9f93bd169ca70c6a0f516a357
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-datatemplate"></a>Tworzenie DataTemplate

_Szablony danych można tworzyć w tekście, ResourceDictionary, lub z typu niestandardowego lub odpowiedniego typu komórki platformy Xamarin.Forms. Ten artykuł opisuje każdego technik._

Typowy scenariusz użycia dla [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) są wyświetlane dane z kolekcji obiektów w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Wygląd danych dla każdej komórki w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) mogą być zarządzane przez ustawienie [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) właściwości [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Istnieje wiele metod, które mogą być używane w tym celu:

- [Tworzenie DataTemplate wbudowanego](#inline).
- [Tworzenie typu obiekt DataTemplate](#type).
- [Tworzenie DataTemplate jako zasób](#resource).

Niezależnie od tego, używane techniki, wynikiem jest to, że wygląd każdej komórki w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) jest definiowana za pomocą [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), jak pokazano na poniższych zrzutach ekranu:

![](creating-images/data-template-appearance.png "Element ListView o szablonie danych")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Tworzenie DataTemplate wbudowany

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Właściwości można ustawić wbudowanego [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Szablon wbudowany, który jest taki, który znajduje się jako element podrzędny właściwości właściwej opcji kontroli, należy używać, gdy nie istnieje potrzeba ponowne użycie szablonu danych w innym miejscu. Elementy określone `DataTemplate` określenia jej wyglądu każdej komórki, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
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
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Label Text="{Binding Name}" FontAttributes="Bold" />
                    <Label Grid.Column="1" Text="{Binding Age}" />
                    <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Podrzędne wbudowanego [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) musi być typu, lub pochodzić od, wpisz [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/). Układ wewnątrz `ViewCell` zarządza tutaj [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). `Grid` Zawiera trzy [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień tego powiązania ich [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) właściwości do odpowiedniej właściwości `Person` obiektu w kolekcji.

W poniższym przykładzie kodu pokazano równoważne kodu C#:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
    public WithDataTemplatePageCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        var personDataTemplate = new DataTemplate(() =>
        {
            var grid = new Grid();
            ...
            var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
            var ageLabel = new Label();
            var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

            nameLabel.SetBinding(Label.TextProperty, "Name");
            ageLabel.SetBinding(Label.TextProperty, "Age");
            locationLabel.SetBinding(Label.TextProperty, "Location");

            grid.Children.Add(nameLabel);
            grid.Children.Add(ageLabel, 1, 0);
            grid.Children.Add(locationLabel, 2, 0);

            return new ViewCell { View = grid };
        });

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemsSource = people, ItemTemplate = personDataTemplate, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

W języku C#, wbudowanej [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) jest tworzony przy użyciu przeładowania konstruktora, który określa `Func` argumentu.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Tworzenie DataTemplate z typem

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Można również ustawić właściwość [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) utworzonego z typem komórki. Zaletą tej metody jest, że wygląd zdefiniowane przez typ komórki mogą być ponownie używane przez wiele szablonów danych w całej aplikacji. Poniższy kod XAML zawiera przykład tej metody:

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
                    ...
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <local:PersonCell />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

W tym miejscu [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) właściwość jest ustawiona na [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) utworzonego z niestandardowego typu, która definiuje wygląd komórek. Niestandardowy typ musi pochodzić od typu [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), jak pokazano w poniższym przykładzie:

```xaml
<ViewCell xmlns="http://xamarin.com/schemas/2014/forms"
          xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
          x:Class="DataTemplates.PersonCell">
     <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.2*" />
            <ColumnDefinition Width="0.3*" />
        </Grid.ColumnDefinitions>
        <Label Text="{Binding Name}" FontAttributes="Bold" />
        <Label Grid.Column="1" Text="{Binding Age}" />
        <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
    </Grid>
</ViewCell>
```

W ramach [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), układ zarządza tutaj [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). `Grid` Zawiera trzy [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień tego powiązania ich [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) właściwości do odpowiedniej właściwości `Person` obiektu w kolekcji.

W poniższym przykładzie pokazano równoważne kodu C#:

```csharp
public class WithDataTemplatePageFromTypeCS : ContentPage
{
    public WithDataTemplatePageFromTypeCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemTemplate = new DataTemplate(typeof(PersonCellCS)), ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

W języku C# [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) jest tworzony przy użyciu przeładowania konstruktora, który określa typ komórki jako argument. Komórka typ musi pochodzić od typu [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), jak pokazano w poniższym przykładzie:

```csharp
public class PersonCellCS : ViewCell
{
    public PersonCellCS()
    {
        var grid = new Grid();
        ...
        var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        var ageLabel = new Label();
        var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

        nameLabel.SetBinding(Label.TextProperty, "Name");
        ageLabel.SetBinding(Label.TextProperty, "Age");
        locationLabel.SetBinding(Label.TextProperty, "Location");

        grid.Children.Add(nameLabel);
        grid.Children.Add(ageLabel, 1, 0);
        grid.Children.Add(locationLabel, 2, 0);

        View = grid;
    }
}
```

> [!NOTE]
> Należy pamiętać, że platformy Xamarin.Forms zawiera także typy komórki, których można użyć do wyświetlenia proste danych w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) komórki. Aby uzyskać więcej informacji, zobacz [wygląd komórek](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Tworzenie DataTemplate jako zasób

Można również tworzyć szablony danych jako obiekty do ponownego użycia w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Jest to osiągane przez nadanie każdej deklaracji unikatowego `x:Key` atrybut, który zapewnia opisową klucza w `ResourceDictionary`, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="personTemplate">
                 <ViewCell>
                    <Grid>
                        ...
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        ...
        <ListView ItemTemplate="{StaticResource personTemplate}" Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Jest przypisany do [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) właściwości przy użyciu `StaticResource` — rozszerzenie znaczników. Należy pamiętać, że podczas `DataTemplate` jest zdefiniowana na stronie [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), może także być zdefiniowany na poziomie aplikacji lub poziom kontroli.

Poniższy przykład kodu pokazuje odpowiedniej strony w języku C#:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
  public WithDataTemplatePageCS ()
  {
    ...
    var personDataTemplate = new DataTemplate (() => {
      var grid = new Grid ();
      ...
      return new ViewCell { View = grid };
    });

    Resources = new ResourceDictionary ();
    Resources.Add ("personTemplate", personDataTemplate);

    Content = new StackLayout {
      Margin = new Thickness(20),
      Children = {
        ...
        new ListView { ItemTemplate = (DataTemplate)Resources ["personTemplate"], ItemsSource = people };
      }
    };
  }
}
```

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Jest dodawany do [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) przy użyciu [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) metodę, która określa `Key` ciąg, który służy do Odwołanie `DataTemplate` podczas pobierania go.

## <a name="summary"></a>Podsumowanie

Ten artykuł ma wyjaśniono, jak utworzyć szablony danych, wbudowany, typu niestandardowego lub w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Szablon wbudowanego należy używać, gdy nie istnieje potrzeba ponowne użycie szablonu danych w innym miejscu. Alternatywnie szablon danych mogą być ponownie używane, definiując go jako niestandardowy typ, lub poziom kontroli zasobów strony, lub na poziomie aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [Wygląd komórki](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Szablony danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
