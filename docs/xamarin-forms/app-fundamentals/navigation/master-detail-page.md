---
title: Strona platformy Xamarin.Forms główny szczegółowy
description: MasterDetailPage platformy Xamarin.Forms jest strona, która zarządza dwie strony powiązane, informacji — prezentuje elementy strony głównej i strony szczegółów, który przedstawia szczegółowe informacje dotyczące elementów na stronie głównej. W tym artykule opisano sposób używania MasterDetailPage i przechodzenia między stronami jego informacji.
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 80d86e1aa6a00d4a55c0fdba1b858bfef7bcbc84
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241347"
---
# <a name="xamarinforms-master-detail-page"></a>Strona platformy Xamarin.Forms główny szczegółowy

_MasterDetailPage platformy Xamarin.Forms jest strona, która zarządza dwie strony powiązane, informacji — prezentuje elementy strony głównej i strony szczegółów, który przedstawia szczegółowe informacje dotyczące elementów na stronie głównej. W tym artykule opisano sposób używania MasterDetailPage i przechodzenia między stronami jego informacji._

## <a name="overview"></a>Omówienie

Strona wzorcowa zwykle wyświetla listę elementów, jak pokazano na poniższych zrzutach ekranu:

[![](master-detail-page-images/masterpage-components.png "Składniki strony wzorca")](master-detail-page-images/masterpage-components-large.png#lightbox "składniki strony wzorca")

Lokalizacja listy elementów jest identyczne w każdej z platform i wybranie jednego z elementów spowoduje przejście do odpowiedniej strony szczegółów. Ponadto strony wzorcowej również funkcje paska nawigacyjnego, który zawiera przycisk, który może służyć do przejdź do strony szczegółów active:

- W systemach iOS, na pasku nawigacyjnym znajdują się w górnej części strony oraz przycisku, który przechodzi do strony szczegółów. Ponadto strona szczegółów active może zostać przesłane do przesuwając strony głównej po lewej stronie.
- W systemie Android na pasku nawigacyjnym znajduje się u góry strony i Wyświetla tytuł, ikonę i przycisku, który przechodzi do strony szczegółów. Ikona jest zdefiniowany w `[Activity]` atrybut, który decorates `MainActivity` klasy w projekcie specyficzne dla platformy systemu Android. Ponadto strona szczegółów active może zostać przesłane do przesuwając strony wzorcowej do lewej strony, wybierając stronę szczegółów prawej strony ekranu i naciskając *ponownie* u dołu ekranu.
- W systemie Windows platformy Uniwersalnej, na pasku nawigacyjnym znajduje się w górnej części strony i ma przycisku, który przechodzi do strony szczegółów.

Szczegółowe dane Wyświetla strony odpowiada elementowi wybrane na głównym strony, a w poniższe zrzuty ekranu przedstawiono główne składniki strony szczegółów:

![](master-detail-page-images/detailpage-components.png "Składniki strony szczegółów")

Strona szczegółów zawiera paska nawigacyjnego, których zawartość jest zależny od platformy:

- W systemach iOS, na pasku nawigacyjnym znajdują się w górnej części strony i Wyświetla tytuł oraz przycisku, który zwraca strony wzorcowej, pod warunkiem, że wystąpienie strony szczegółów jest ujęte w [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wystąpienia. Ponadto strony wzorcowej może być zwracany do przesuwając strony szczegółów z prawej strony.
- W systemie Android pasek nawigacyjny znajduje się u góry strony i Wyświetla tytuł, ikonę i przycisku, który zwraca strony wzorcowej. Ikona jest zdefiniowany w `[Activity]` atrybut, który decorates `MainActivity` klasy w projekcie specyficzne dla platformy systemu Android.
- Na platformy uniwersalnej systemu Windows na pasku nawigacyjnym znajduje się u góry strony i Wyświetla tytuł i ma przycisku, który zwraca strony wzorcowej.

### <a name="navigation-behavior"></a>Zachowanie nawigacji

Zachowanie środowiska nawigacji między stron wzorcowych i szczegółowych to platforma zależne:

- W systemach iOS, strona szczegółów *slajdów* z prawej strony jako stronę wzorcową slajdów z lewej strony i lewej części szczegółów strona jest nadal widoczna.
- W systemie Android, strony główne i szczegóły są *nakładany* na siebie.
- Na platformy uniwersalnej systemu Windows, szczegóły i wzorzec strony są *miejscami*.

Podobnie, będzie można zauważyć w trybie krajobraz z tym, że strony wzorcowej w systemach iOS i Android ma podobne szerokość jako strony wzorcowej w trybie portret, więc więcej strony szczegółów będzie widoczna.

Informacje dotyczące sterowania zachowaniem nawigacji, zobacz [kontrolowanie zachowania wyświetlania strony szczegółów](#Controlling_the_Detail_Page_Display_Behavior).

## <a name="creating-a-masterdetailpage"></a>Tworzenie MasterDetailPage

A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) zawiera [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) i [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) właściwości, które są typu [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), które służą do pobierania i ustawiania odpowiednio stron wzorcowych i szczegółów.

> [!IMPORTANT]
> A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) została zaprojektowana jako strony głównej i jako strony podrzędnej w innych typach strony może spowodować nieoczekiwane i niespójne zachowanie. Ponadto ma zalecane strona wzorcowa [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) powinna zawsze być [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpienie, a strona szczegółów tylko powinny zostać wypełnione z [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), i `ContentPage` wystąpień. Pomoże to w celu zapewnienia spójną obsługę użytkowników na wszystkich platformach.

Przedstawia poniższy przykładowy kod XAML [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) stanowiąca [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) i [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) właściwości:

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

Poniższy przykład kodu pokazuje odpowiednik [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) utworzony w języku C#:

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

[ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) Właściwość jest ustawiona na [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpienia. [ `MasterDetailPage.Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) Właściwość jest ustawiona na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) zawierający `ContentPage` wystąpienia.

### <a name="creating-the-master-page"></a>Tworzenie strony wzorcowej

Poniższy przykładowy kod XAML zawiera deklarację `MasterPage` obiektu, który odwołuje się do [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) właściwości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView">
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

Strona składa się z [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) który wypełnione z danymi w języku XAML, ustawiając jego [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) właściwości do tablicy `MasterPageItem` wystąpień. Każdy `MasterPageItem` definiuje `Title`, `IconSource`, i `TargetType` właściwości.

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) jest przypisany do [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) właściwości do wyświetlenia każdego `MasterPageItem`. `DataTemplate` Zawiera [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) składający się z [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) i [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/). [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Wyświetla `IconSource` wartość właściwości oraz [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Wyświetla `Title` wartości właściwości dla każdego `MasterPageItem`.

Na stronie jej [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) i [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) zestawu właściwości. Ikona pojawi się na stronie szczegółów, pod warunkiem, że strona szczegółów paska tytułu. To musi być włączona w systemie iOS zawijania wystąpienie strony szczegółów w [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wystąpienia.

> [!NOTE]
> [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) Strona musi mieć jego [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) ustawić właściwości lub wystąpi wyjątek.

Poniższy przykład kodu pokazuje odpowiedniej strony utworzone w języku C#:

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

Poniższe zrzuty ekranu Pokaż strony wzorcowej na każdej platformie:

![](master-detail-page-images/masterpage.png "Przykład strony wzorca")

### <a name="creating-and-displaying-the-detail-page"></a>Tworzenie i wyświetlanie strony szczegółów

`MasterPage` Wystąpienie zawiera `ListView` właściwość, która przedstawia jego [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wystąpienia, aby `MainPage` [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) można zarejestrować wystąpienia Program obsługi zdarzeń do obsługi [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) zdarzeń. Dzięki temu `MainPage` wystąpienia, aby ustawić [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) właściwości do strony, która reprezentuje wybranego `ListView` elementu. Poniższy przykład kodu pokazuje programu obsługi zdarzeń:

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.ListView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.ListView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

`OnItemSelected` Metoda wykonuje następujące czynności:

- Pobiera on [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) z [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wystąpienia oraz pod warunkiem, że nie jest on `null`, ustawia strony szczegółów do nowego wystąpienia typu strony przechowywane w `TargetType`właściwość `MasterPageItem`. Typ strony jest ujęte w [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wystąpienia, aby upewnić się, że ikona odwołania za pośrednictwem [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) właściwość `MasterPage` jest wyświetlany na stronie szczegółów w systemie iOS.
- Wybrany element w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ma ustawioną wartość `null` do upewnij się, że żaden z `ListView` elementy zostaną wybrane następnym `MasterPage` jest przedstawiony.
- Strona szczegółów jest wyświetlana przez ustawienie [ `MasterDetailPage.IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) właściwości `false`. Ta właściwość określa, czy strony głównego lub szczegółów jest przedstawiony. Należy wybrać opcję `true` do wyświetlenia strony wzorcowej i `false` do wyświetlenia strony szczegółów.

Pokaż poniższe zrzuty ekranu `ContactPage` strony szczegółów, który jest wyświetlany po został wybrany na stronie głównej:

![](master-detail-page-images/detailpage.png "Przykład strony szczegółów")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>Kontrolowanie zachowania wyświetlana strona szczegółów

Jak [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) zarządza stron wzorcowych i szczegółowych zależy od tego, czy aplikacja działa na telefon lub tablet, orientacji urządzenia, a wartość [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) Właściwość. Ta właściwość określa sposób wyświetlania strony szczegółów. Jego możliwe wartości to:

- **Domyślne** — strony są wyświetlane przy użyciu domyślnego platformy.
- **Popover** — strona szczegółów obejmuje lub częściowo obejmuje strony wzorcowej.
- **Podziel** — strony wzorcowej jest wyświetlana po lewej stronie i strony szczegółów jest po prawej stronie.
- **SplitOnLandscape** — ekranu podziału jest używany, gdy urządzenie jest w orientacji poziomej.
- **SplitOnPortrait** — ekranu podziału jest używany, gdy urządzenie jest w orientacji pionowej.

Poniższy przykład kodu XAML pokazano, jak ustawić [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) właściwość [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

Poniższy przykład kodu pokazuje odpowiednik [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) utworzony w języku C#:

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

Jednak wartość [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) właściwość dotyczy tylko aplikacji uruchomionych na tabletów lub pulpitu. Aplikacje uruchomione na telefonach zawsze mieć *Popover* zachowanie.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) i przechodzenia między stronami jego informacji. Platformy Xamarin.Forms `MasterDetailPage` jest strona, która zarządza dwie strony powiązane informacje — prezentuje elementy strony wzorcowej oraz strony szczegółów, który przedstawia szczegółowe informacje dotyczące elementów na stronie głównej.


## <a name="related-links"></a>Linki pokrewne

- [Odmian strony](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)
