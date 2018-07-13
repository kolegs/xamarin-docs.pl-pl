---
title: Strona Xamarin.Forms wzorzec / szczegół
description: Xamarin.Forms MasterDetailPage jest strona, która zarządza dwie strony powiązane, informacji — strony wzorcowej, która przedstawia elementy i stronę szczegółów, która przedstawia szczegółowe informacje na temat elementów na stronie wzorcowej. W tym artykule wyjaśniono, jak używać MasterDetailPage i przechodzenia między stronami, jego informacji.
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: a3d0edbd933339ee8b8a0a277a4f2493cc8dc70e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997468"
---
# <a name="xamarinforms-master-detail-page"></a>Strona Xamarin.Forms wzorzec / szczegół

_Xamarin.Forms MasterDetailPage jest strona, która zarządza dwie strony powiązane, informacji — strony wzorcowej, która przedstawia elementy i stronę szczegółów, która przedstawia szczegółowe informacje na temat elementów na stronie wzorcowej. W tym artykule wyjaśniono, jak używać MasterDetailPage i przechodzenia między stronami, jego informacji._

## <a name="overview"></a>Omówienie

Strona wzorcowa zwykle wyświetla listę elementów, jak pokazano na poniższych zrzutach ekranu:

[![](master-detail-page-images/masterpage-components.png "Składniki strony główne")](master-detail-page-images/masterpage-components-large.png#lightbox "składniki strony główne")

Lokalizacja listy elementów jest taka sama na każdej platformie i wybierając jedną z pozycji spowoduje przejście do odpowiedniej strony szczegółów. Ponadto strona wzorcowa obejmuje także pasek nawigacyjny, który zawiera przycisk, który może służyć do przejdź do strony szczegółów active:

- W systemach iOS na pasku nawigacyjnym znajduje się w górnej części strony i ma przycisk, który prowadzi do strony szczegółów. Ponadto strona szczegółów active można nastąpi przejście, szybko przesuwając palcem w stronę wzorcową po lewej stronie.
- W systemie Android na pasku nawigacyjnym znajduje się w górnej części strony i Wyświetla tytuł, ikona i przycisk, który prowadzi do strony szczegółów. Ikona jest zdefiniowany w `[Activity]` atrybut, który rozszerza `MainActivity` klasy w projekcie dla konkretnej platformy systemu Android. Ponadto strona szczegółów active można nastąpi przejście, szybko przesuwając palcem w stronę wzorcową po lewej stronie, wybierając stronę szczegółów po prawej stronie ekranu i naciskając *ponownie* znajdujący się u dołu ekranu.
- Na Universal Windows Platform (platformy UWP), na pasku nawigacyjnym znajduje się w górnej części strony, a przycisk, który prowadzi do strony szczegółów.

Szczegóły strony wyświetla dane, które odpowiada elementowi zaznaczone na wzorcu strony, a w poniższych zrzutach ekranu przedstawiono główne składniki strony szczegółów:

![](master-detail-page-images/detailpage-components.png "Składniki strony szczegółów")

Strona szczegółów zawiera paska nawigacyjnego, w których zawartość jest zależny od platformy:

- W systemach iOS, na pasku nawigacyjnym znajduje się w górnej części strony Wyświetla tytuł i ma przycisk, który zwraca strony wzorcowej, pod warunkiem, że wystąpienie strony szczegółów jest opakowana w [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) wystąpienia. Ponadto strona wzorcowa mogą być zwracane do, szybko przesuwając palcem strony szczegółów po prawej stronie.
- W systemie Android pasek nawigacyjny znajduje się w górnej części strony i Wyświetla tytuł, ikona i przycisk, który zwraca strony wzorcowej. Ikona jest zdefiniowany w `[Activity]` atrybut, który rozszerza `MainActivity` klasy w projekcie dla konkretnej platformy systemu Android.
- Na platformie UWP na pasku nawigacyjnym znajduje się w górnej części strony Wyświetla tytuł i znajduje się przycisk, która zwraca strony wzorcowej.

### <a name="navigation-behavior"></a>Zachowania nawigacji

Zachowanie środowisko nawigacji między strony wzorcowe i szczegółowe to platforma zależne:

- W systemach iOS, strona szczegółów *slajdy* z prawej strony jako stronę wzorcową slajdy, z lewej strony i lewej części szczegółów strony jest nadal wyświetlany.
- W systemie Android strony szczegółów i wzorzec są *nakładany* na siebie nawzajem.
- Na platformie UWP, strony szczegółów i wzorzec są *zamienione*.

Podobne zachowanie będzie można zauważyć w orientacji poziomej, z tą różnicą, że strony wzorcowej w systemach iOS i Android ma podobne szerokość jako strony wzorcowej w orientacji pionowej, dlatego więcej strony szczegółów będzie widoczna.

Aby dowiedzieć się, jak kontrolowanie zachowania nawigacji, zobacz [kontrolowanie zachowania wyświetlania strony szczegółów](#Controlling_the_Detail_Page_Display_Behavior).

## <a name="creating-a-masterdetailpage"></a>Tworzenie MasterDetailPage

A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) zawiera [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) i [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) właściwości, które są zarówno typ [ `Page` ](xref:Xamarin.Forms.Page), które są używane do pobierania i ustawiania odpowiednio stron wzorcowych i szczegółów.

> [!IMPORTANT]
> A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) została zaprojektowana jako strony głównej i używając go, ponieważ strona podrzędna, w innych typach strony może spowodować nieoczekiwane i niespójne zachowanie. Ponadto zalecamy, aby strona wzorcowa [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) powinna zawsze być [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpienie i że strona szczegółów tylko będzie zapełniony stanem [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), i `ContentPage` wystąpień. Pomoże to zapewnić spójne środowisko użytkownika na wszystkich platformach.

Ilustruje poniższy przykład kodu XAML [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) określająca [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) i [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) właściwości:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  xmlns:local="clr-namespace:MasterDetailPageNavigation;assembly=MasterDetailPageNavigation"
                  x:Class="MasterDetailPageNavigation.MainPage">
    <MasterDetailPage.Master>
        <local:MasterPage x:Name="masterPage" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage>
            <x:Arguments>
                <local:ContactsPage />
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Poniższy przykład kodu pokazuje odpowiednik [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) utworzone w języku C#:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        masterPage = new MasterPageCS ();
        Master = masterPage;
        Detail = new NavigationPage (new ContactsPageCS ());
        ...
    }
    ...
}
```

[ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) Właściwość jest ustawiona na [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpienia. [ `MasterDetailPage.Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) Właściwość jest ustawiona na [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) zawierający `ContentPage` wystąpienia.

### <a name="creating-the-master-page"></a>Tworzenie strony wzorcowej

Poniższy przykład kodu XAML przedstawiono deklarację `MasterPage` obiektu, który jest przywoływane za pośrednictwem [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) właściwości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView" x:FieldModifier="public">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:MasterPageItem}">
                    <local:MasterPageItem Title="Contacts" IconSource="contacts.png" TargetType="{x:Type local:ContactsPage}" />
                    <local:MasterPageItem Title="TodoList" IconSource="todo.png" TargetType="{x:Type local:TodoListPage}" />
                    <local:MasterPageItem Title="Reminders" IconSource="reminders.png" TargetType="{x:Type local:ReminderPage}" />
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid Padding="5,10">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="30"/>
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding IconSource}" />
                            <Label Grid.Column="1" Text="{Binding Title}" />
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Strona składa się z [ `ListView` ](xref:Xamarin.Forms.ListView) , jest wypełniana danymi w XAML, ustawiając jego [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) właściwości tablicy `MasterPageItem` wystąpień. Każdy `MasterPageItem` definiuje `Title`, `IconSource`, i `TargetType` właściwości.

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) jest przypisany do [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) właściwość do wyświetlenia każdego `MasterPageItem`. `DataTemplate` Zawiera [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) składający się z [ `Image` ](xref:Xamarin.Forms.Image) i [ `Label` ](xref:Xamarin.Forms.Label). [ `Image` ](xref:Xamarin.Forms.Image) Wyświetla `IconSource` wartości właściwości i [ `Label` ](xref:Xamarin.Forms.Label) Wyświetla `Title` wartości właściwości dla każdego `MasterPageItem`.

Na stronie znajduje się jego [ `Title` ](xref:Xamarin.Forms.Page.Title) i [ `Icon` ](xref:Xamarin.Forms.Page.Icon) zestawu właściwości. Ikona pojawi się na stronie szczegółów, pod warunkiem, że strona szczegółów paska tytułu. To musi być włączona w systemie iOS opakowując wystąpienie strony szczegółów w [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) wystąpienia.

> [!NOTE]
> [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) Strony musi mieć jej [ `Title` ](xref:Xamarin.Forms.Page.Title) właściwością lub wystąpi wyjątek.

Poniższy przykład kodu pokazuje odpowiednich stron, które są tworzone w języku C#:

```csharp
public class MasterPageCS : ContentPage
{
  public ListView ListView { get { return listView; } }

  ListView listView;

  public MasterPageCS ()
  {
    var masterPageItems = new List<MasterPageItem> ();
    masterPageItems.Add (new MasterPageItem {
      Title = "Contacts",
      IconSource = "contacts.png",
      TargetType = typeof(ContactsPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "TodoList",
      IconSource = "todo.png",
      TargetType = typeof(TodoListPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "Reminders",
      IconSource = "reminders.png",
      TargetType = typeof(ReminderPageCS)
    });

    listView = new ListView {
      ItemsSource = masterPageItems,
      ItemTemplate = new DataTemplate (() => {
        var grid = new Grid { Padding = new Thickness(5, 10) };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(30) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });

        var image = new Image();
        image.SetBinding(Image.SourceProperty, "IconSource");
        var label = new Label { VerticalOptions = LayoutOptions.FillAndExpand };
        label.SetBinding(Label.TextProperty, "Title");

        grid.Children.Add(image);
        grid.Children.Add(label, 1, 0);

        return new ViewCell { View = grid };
      }),
      SeparatorVisibility = SeparatorVisibility.None
    };

    Icon = "hamburger.png";
    Title = "Personal Organiser";
    Content = new StackLayout
    {
      Children = { listView }
    };
  }
}
```

Poniższych zrzutach ekranu przedstawiono strony wzorcowej na każdej z platform:

![](master-detail-page-images/masterpage.png "Przykładowa strona wzorca")

### <a name="creating-and-displaying-the-detail-page"></a>Tworzenie i wyświetlanie strony szczegółów

`MasterPage` Wystąpienie zawiera `ListView` właściwość, która uwidacznia jego [ `ListView` ](xref:Xamarin.Forms.ListView) wystąpienia, aby `MainPage` [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) zarejestrować wystąpienie Program obsługi zdarzeń do obsługi [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) zdarzeń. Dzięki temu `MainPage` wystąpienia, aby ustawić [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) właściwości do strony, która reprezentuje wybrane `ListView` elementu. Poniższy przykład kodu pokazuje programu obsługi zdarzeń:

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.listView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.listView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

`OnItemSelected` Metoda wykonuje następujące czynności:

- Pobiera [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) z [ `ListView` ](xref:Xamarin.Forms.ListView) metodą wystąpienia oraz pod warunkiem, że nie jest `null`, ustawia stronę szczegółów do nowego wystąpienia typu strony, przechowywana w `TargetType`właściwość `MasterPageItem`. Typ strony jest opakowana w [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) wystąpienia, aby upewnić się, że ikona przywoływane za pośrednictwem [ `Icon` ](xref:Xamarin.Forms.Page.Icon) właściwość `MasterPage` jest wyświetlany na stronie szczegółów w systemie iOS.
- Wybrany element w [ `ListView` ](xref:Xamarin.Forms.ListView) jest ustawiona na `null` do upewnij się, że żaden z `ListView` elementy zostaną wybrane następnym razem `MasterPage` zostanie wyświetlony.
- Strona szczegółów jest wyświetlone użytkownikowi, ustawiając [ `MasterDetailPage.IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) właściwość `false`. Ta właściwość określa, czy strona wzorca lub szczegółów zostanie wyświetlony. Powinien być ustawiony na `true` do wyświetlenia strony wzorcowej i `false` można wyświetlić strony szczegółów.

Poniższe zrzuty ekranu Pokaż `ContactPage` strony szczegółów, który jest wyświetlany po został wybrany na stronie głównej:

![](master-detail-page-images/detailpage.png "Przykładowa strona szczegółów")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>Kontrolowanie zachowania wyświetlania strony szczegółów

Sposób, w jaki [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) zarządza strony wzorcowe i szczegółowe zależy od tego, czy aplikacja jest uruchomiona na telefonie lub tablecie orientacji urządzenia, a wartość [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) Właściwość. Ta właściwość określa, jak zostanie wyświetlona strona szczegółów. Jego możliwe wartości to:

- **Domyślne** — strony są wyświetlane przy użyciu domyślnego platformy.
- **Popover** — strona szczegółów obejmuje lub częściowo obejmuje strony wzorcowej.
- **Podziel** — strona główna jest wyświetlany po lewej stronie i strony szczegółów po prawej.
- **SplitOnLandscape** — na ekranie podzielonym jest używany, gdy urządzenie jest w orientacji poziomej.
- **SplitOnPortrait** — na ekranie podzielonym jest używany, gdy urządzenie jest w orientacji pionowej.

Poniższy przykład kodu XAML pokazuje, jak ustawić [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) właściwość [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

Poniższy przykład kodu pokazuje odpowiednik [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) utworzone w języku C#:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        MasterBehavior = MasterBehavior.Popover;
        ...
    }
}
```

Jednak wartość [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) właściwość ma wpływ tylko na aplikacje na tablety lub pulpitu. Aplikacje działające na telefonach zawsze mieć *Popover* zachowanie.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) oraz przechodzenia między stronami, jego informacji. Xamarin.Forms `MasterDetailPage` to strona, która zarządza dwie strony z powiązanymi informacjami — strony wzorcowej, która przedstawia elementy i stronę szczegółów, która przedstawia szczegółowe informacje na temat elementów na stronie wzorcowej.


## <a name="related-links"></a>Linki pokrewne

- [Różne typy stron](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](xref:Xamarin.Forms.MasterDetailPage)
