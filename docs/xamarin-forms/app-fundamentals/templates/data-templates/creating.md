---
title: Tworzenie DataTemplate zestawu narzędzi Xamarin.Forms
description: Szablony danych można tworzyć w tekście, ResourceDictionary, lub z typu niestandardowego lub odpowiedni typ komórki zestawu narzędzi Xamarin.Forms. Ten artykuł opisuje każdą z technik.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 63f9bf82bc8e637aced1afa5d5699ac1e8dc3f8c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994618"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Tworzenie DataTemplate zestawu narzędzi Xamarin.Forms

_Szablony danych można tworzyć w tekście, ResourceDictionary, lub z typu niestandardowego lub odpowiedni typ komórki zestawu narzędzi Xamarin.Forms. Ten artykuł opisuje każdą z technik._

Typowy scenariusz użycia dla [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) jest wyświetlanie danych za pomocą kolekcji obiektów w [ `ListView` ](xref:Xamarin.Forms.ListView). Wygląd danych dla każdej komórki w [ `ListView` ](xref:Xamarin.Forms.ListView) mogą być zarządzane przez ustawienie [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) właściwości [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Istnieje kilka metod, których można użyć w tym celu:

- [Tworzenie DataTemplate wbudowane](#inline).
- [Tworzenie DataTemplate z typem](#type).
- [Tworzenie DataTemplate jako zasób](#resource).

Bez względu na to technika używana, wynik jest to, że wygląd każdej komórki w [ `ListView` ](xref:Xamarin.Forms.ListView) jest definiowany przez [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), jak pokazano na poniższych zrzutach ekranu:

![](creating-images/data-template-appearance.png "ListView przy użyciu DataTemplate")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Tworzenie DataTemplate wbudowane

[ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) Właściwość można ustawić wbudował [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Szablon wbudowane, który jest umieszczony jako element podrzędny właściwości właściwej opcji kontroli, należy używać, jeśli nie ma potrzeby to ponowne użycie szablonu danych w innym miejscu. Elementy określone `DataTemplate` zdefiniować wygląd każdej komórki, jak pokazano w poniższym przykładzie kodu XAML:

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

Element podrzędny wbudowany [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) musi być typu, lub pochodzić od, wpisz [ `ViewCell` ](xref:Xamarin.Forms.ViewCell). Układ wewnątrz `ViewCell` zarządza tutaj [ `Grid` ](xref:Xamarin.Forms.Grid). `Grid` Zawiera trzy [ `Label` ](xref:Xamarin.Forms.Label) wystąpień tego powiązania ich [ `Text` ](xref:Xamarin.Forms.Label.Text) właściwości do odpowiedniej właściwości `Person` obiektu w kolekcji.

Równoważny kod C# pokazano w poniższym przykładzie kodu:

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

W języku C#, wbudowanej [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) jest tworzony za pomocą przeciążenia konstruktora, który określa `Func` argumentu.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Tworzenie DataTemplate z typem

[ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) Właściwość można ustawić [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) utworzonego z typu komórki. Zaletą tego podejścia jest to, że wygląd określona przez typ komórki mogą być ponownie używane przez wiele szablonów danych w całej aplikacji. Poniższy kod XAML przedstawiono przykład tej metody:

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

W tym miejscu [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) właściwość jest ustawiona na [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) utworzonego z typu niestandardowego, która definiuje wygląd komórki. Niestandardowy typ musi pochodzić od typu [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), jak pokazano w poniższym przykładzie kodu:

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

W ramach [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), układ zarządza tutaj [ `Grid` ](xref:Xamarin.Forms.Grid). `Grid` Zawiera trzy [ `Label` ](xref:Xamarin.Forms.Label) wystąpień tego powiązania ich [ `Text` ](xref:Xamarin.Forms.Label.Text) właściwości do odpowiedniej właściwości `Person` obiektu w kolekcji.

Równoważny kod C# pokazano w poniższym przykładzie:

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

W języku C# [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) jest tworzony za pomocą przeciążenia konstruktora, który określa typ komórki jako argument. Typ komórki musi pochodzić od typu [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), jak pokazano w poniższym przykładzie kodu:

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
> Należy pamiętać, że Xamarin.Forms obejmuje również typy komórki, które może służyć do wyświetlania danych proste w [ `ListView` ](xref:Xamarin.Forms.ListView) komórek. Aby uzyskać więcej informacji, zobacz [wygląd komórki](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Tworzenie DataTemplate jako zasób

Można również tworzyć szablony danych jako obiekty wielokrotnego użytku w [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). To osiągnąć, podając unikatową każdego zgłoszenia `x:Key` atrybut, który dostarcza mu kluczem opisowe w `ResourceDictionary`, jak pokazano w poniższym przykładzie kodu XAML:

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

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Jest przypisany do [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) właściwości przy użyciu `StaticResource` — rozszerzenie znaczników. Należy pamiętać, że podczas `DataTemplate` jest zdefiniowana na stronie [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), również może być zdefiniowana na poziomie aplikacji lub poziom kontroli.

Poniższy przykład kodu pokazuje odpowiednich stron w języku C#:

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

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Jest dodawany do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) przy użyciu [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) metody, która określa `Key` ciąg, który jest używany do Odwołanie `DataTemplate` podczas pobierania go.

## <a name="summary"></a>Podsumowanie

Tym artykule wyjaśniono, jak utworzyć szablony danych, w tekście, z typu niestandardowego lub w [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Wbudowany szablon należy używać, jeśli nie ma potrzeby to ponowne użycie szablonu danych w innym miejscu. Alternatywnie szablonu danych może nastąpić, definiując je jako typ niestandardowy, lub poziom kontroli zasobu strony lub na poziomie aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [Wygląd komórki](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Szablony danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
