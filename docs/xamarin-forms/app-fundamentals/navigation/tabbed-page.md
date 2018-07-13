---
title: Strona z kartami zestawu narzędzi Xamarin.Forms
description: Xamarin.Forms TabbedPage składa się z listy kart i większy obszar szczegółów każdej karcie ładowanie zawartości w obszarze szczegółów. W tym artykule przedstawiono sposób użycia TabbedPage przechodzenie przez zbiór stron.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 3eb978780222da2050fc91dfa41c68ef4bd3b6f4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996298"
---
# <a name="xamarinforms-tabbed-page"></a>Strona z kartami zestawu narzędzi Xamarin.Forms

_Xamarin.Forms TabbedPage składa się z listy kart i większy obszar szczegółów każdej karcie ładowanie zawartości w obszarze szczegółów. W tym artykule przedstawiono sposób użycia TabbedPage przechodzenie przez zbiór stron._

## <a name="overview"></a>Omówienie

Poniższe zrzuty ekranu Pokaż [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) na każdej z platform:

![](tabbed-page-images/tab1.png "Przykład TabbedPage")

Poniższe zrzuty ekranu skupić się na format kartę na każdej z platform:

![](tabbed-page-images/tabbedpage-components.png "Karta TabbedPage składników")

Układ [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), jego karty i jest zależny od platformy:

- W systemach iOS zostanie wyświetlona lista kart w dolnej części ekranu, a powyżej znajduje się w obszarze szczegółów. Każda karta zawiera również obraz ikony, która powinna być 30 x 30 PNG z przezroczystością dla zwykłej rozdzielczości, 60 x 60 dla o wysokiej rozdzielczości i 90 x 90 dla telefonu iPhone 6 Plus rozdzielczości. Jeśli istnieje więcej niż pięć kart *więcej* pojawi się karta, która może służyć do dostępu dodatkowe karty. Aby uzyskać więcej informacji na temat ładowania obrazów w aplikacji platformy Xamarin.Forms, zobacz [Praca z obrazami](~/xamarin-forms/user-interface/images.md). Aby uzyskać więcej informacji na temat wymagań dotyczących ikonę zobacz [tworzenie aplikacji z kartami](~/ios/user-interface/controls/creating-tabbed-applications.md).

    > [!NOTE]
  > Należy pamiętać, że `TabbedRenderer` dla systemu iOS jest możliwym do zastąpienia `GetIcon` metodę, która może służyć do załadowania ikon kart z określonego źródła. To zastąpienie sprawia, że można używać obrazów SVG jako ikony na `TabbedPage`. Ponadto można podać zaznaczone i niezaznaczone wersji ikony.

- W systemie Android listę kart w górnej części ekranu pojawi się domyślnie, a w obszarze szczegółów znajduje się poniżej. Jednak lista kart, mogą być przenoszone do dolnej części ekranu przy użyciu określonych platform. Aby uzyskać więcej informacji, zobacz [ustawienie TabbedPage narzędzi położenie i kolor](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar).

    > [!NOTE]
  > Należy pamiętać, że używając AppCompat w systemie Android, każda karta będą również wyświetlane ikony. Ponadto `TabbedPageRenderer` dla systemu Android AppCompat ma możliwym do zastąpienia `SetTabIcon` metodę, która może służyć do załadowania ikon kart z niestandardowego `Drawable`. To zastąpienie sprawia, że można używać obrazów SVG jako ikony na `TabbedPage`.

- Na tablecie Windows — standardy, karty nie zawsze są widoczne, a użytkownicy nie muszą Przesuń w dół (lub kliknij prawym przyciskiem myszy, jeśli mają one dołączone myszy) na kartach w widoku `TabbedPage` (jak pokazano poniżej).

![](tabbed-page-images/windows-tabs.png "TabbedPage kartach Windows")

## <a name="creating-a-tabbedpage"></a>Tworzenie TabbedPage

Dwie metody mogą służyć do tworzenia [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

- [Wypełnij](#Populating_a_TabbedPage_with_a_Page_Collection) [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) z kolekcją podrzędnych [ `Page` ](xref:Xamarin.Forms.Page) obiektów, takich jak kolekcja [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpień.
- [Przypisz](#Populating_a_TabbedPage_with_a_Template) kolekcję, aby [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) właściwości i przypisz [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) do [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) właściwości, aby powrócić do strony obiekty w kolekcji.

Za pomocą obu metod [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) wyświetlić każdą stronę, gdy użytkownik wybierze każdej karty.

> [!NOTE]
> Zalecamy, aby [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) powinny zostać wypełnione [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) i [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)tylko wystąpień. Pomoże to zapewnić spójne środowisko użytkownika na wszystkich platformach.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>Wypełnianie TabbedPage z kolekcją strony

Ilustruje poniższy przykład kodu XAML [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) tworzony przez zapełnianie kolekcji podrzędnej [ `Page` ](xref:Xamarin.Forms.Page) obiektów:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" Icon="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

Poniższy przykład kodu pokazuje odpowiednik [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) utworzone w języku C#:

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    var navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.Icon = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Jest wypełniana przy użyciu dwóch podrzędnych [ `Page` ](xref:Xamarin.Forms.Page) obiektów. Jest pierwszym elementem podrzędnym [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpienia, a druga karta jest [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) zawierający `ContentPage` wystąpienia.

> [!NOTE]
> [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Nie obsługuje wirtualizacja interfejsu użytkownika. W związku z tym, może mieć wpływ na wydajność `TabbedPage` zawiera zbyt wiele elementów podrzędnych.

Poniższe zrzuty ekranu Pokaż `TodayPage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpienia, które są wyświetlane na *już dziś* karty:

![](tabbed-page-images/today-page.png "ContentPage w TabbedPage")

Wybieranie *harmonogram* karcie Wyświetla `SchedulePage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpienia, co jest opakowana w [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) metodą wystąpienia oraz jest wyświetlany w Poniższy zrzut ekranu:

![](tabbed-page-images/schedule-page.png "NavigationPage w TabbedPage")

Aby uzyskać informacje dotyczące układu [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), zobacz [wykonywania nawigacji](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> Mimo że jest dopuszczalne można umieścić [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) do [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), nie zaleca się umieszczenie `TabbedPage` do `NavigationPage`. Jest to, ponieważ w systemie iOS `UITabBarController` zawsze działa jako otokę dla `UINavigationController`. Aby uzyskać więcej informacji, zobacz [połączone interfejsów kontrolera widoku](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) w bibliotece deweloperów systemu iOS.

#### <a name="navigation-inside-a-tab"></a>Nawigacja wewnątrz karty

Nawigacji może być wykonywana w drugiej karcie wywołując [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) metody [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) właściwość [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpienia jak pokazano w poniższym przykładzie kodu:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

Powoduje to, że `UpcomingAppointmentsPage` wystąpienia, które ma zostać wypchnięty na stos nawigacji, gdzie staje się stroną aktywną. Na poniższych zrzutach ekranu przedstawiono to:

![](tabbed-page-images/navigationpage.png "Nawigacja wewnątrz karty")

Aby uzyskać więcej informacji na temat wykonywania za pomocą nawigacji [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) klasy, zobacz [Nawigacja hierarchiczna](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Wypełnianie TabbedPage przy użyciu szablonu

Ilustruje poniższy przykład kodu XAML [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) skonstruowany, przypisując [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) do [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) właściwości, aby powrócić do strony obiekty w kolekcji:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage">
  <TabbedPage.Resources>
    <ResourceDictionary>
      <local:NonNullToBooleanConverter x:Key="booleanConverter" />
    </ResourceDictionary>
  </TabbedPage.Resources>
  <TabbedPage.ItemTemplate>
    <DataTemplate>
      <ContentPage Title="{Binding Name}" Icon="monkeyicon.png">
        <StackLayout Padding="5, 25">
          <Label Text="{Binding Name}" Font="Bold,Large" HorizontalOptions="Center" />
          <Image Source="{Binding PhotoUrl}" WidthRequest="200" HeightRequest="200" />
          <StackLayout Padding="50, 10">
            <StackLayout Orientation="Horizontal">
              <Label Text="Family:" HorizontalOptions="FillAndExpand" />
              <Label Text="{Binding Family}" Font="Bold,Medium" />
            </StackLayout>
            ...
          </StackLayout>
        </StackLayout>
      </ContentPage>
    </DataTemplate>
  </TabbedPage.ItemTemplate>
</TabbedPage>
```

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Wypełniony danymi, ustawiając [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) właściwość w Konstruktorze dla pliku związanym z kodem:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

Poniższy przykład kodu pokazuje odpowiednik [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) utworzone w języku C#:

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() => {
      var nameLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage {
        Icon = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children = {
            nameLabel,
            image,
            new StackLayout {
              Padding = new Thickness (50, 10),
              Children = {
                new StackLayout {
                  Orientation = StackOrientation.Horizontal,
                  Children = {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                ...
              }
            }
          }
        }
      };
      contentPage.SetBinding (TitleProperty, "Name");
      return contentPage;
    });
    ItemsSource = MonkeyDataModel.All;
  }
}
```

Każda karta zawiera [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) serii, który używa [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) i [ `Label` ](xref:Xamarin.Forms.Label) wystąpienia, aby wyświetlić dane dla karty. Poniższych zrzutach ekranu przedstawiono zawartość *Tamarin* karty:

![](tabbed-page-images/tab3.png "Wypełnianie TabbedPage przy użyciu szablonu")

Wybraniu innej karty, a następnie wyświetla zawartość dla tej karty.

> [!NOTE]
> [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Nie obsługuje wirtualizacja interfejsu użytkownika. W związku z tym, może mieć wpływ na wydajność `TabbedPage` zawiera zbyt wiele elementów podrzędnych.

Aby uzyskać więcej informacji na temat [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), zobacz [25 rozdział](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold Xamarin.Forms książki.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia TabbedPage przechodzenie przez zbiór stron. Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) składa się z listy kart i większy obszar szczegółów, o każdej karcie ładowanie zawartości w obszarze szczegółów.


## <a name="related-links"></a>Linki pokrewne

- [Różne typy stron](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](xref:Xamarin.Forms.TabbedPage)
