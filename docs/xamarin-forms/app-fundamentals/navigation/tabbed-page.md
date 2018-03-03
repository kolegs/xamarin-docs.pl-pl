---
title: Strona z kartami
description: "TabbedPage platformy Xamarin.Forms składa się z listy kart i większy obszar szczegółów, każdej karcie ładowania zawartości w obszarze szczegółów. W tym artykule przedstawiono sposób użycia TabbedPage poruszać się po jest zestawem stron."
ms.topic: article
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 97ca114f1160168c7fd9439e31dc475bc37b0467
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="tabbed-page"></a>Strona z kartami

_TabbedPage platformy Xamarin.Forms składa się z listy kart i większy obszar szczegółów, każdej karcie ładowania zawartości w obszarze szczegółów. W tym artykule przedstawiono sposób użycia TabbedPage poruszać się po jest zestawem stron._

## <a name="overview"></a>Omówienie

Pokaż poniższe zrzuty ekranu [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) na każdej platformie:

![](tabbed-page-images/tab1.png "Przykład TabbedPage")

Poniższe zrzuty ekranu skupić się na format kartę na każdej platformie:

![](tabbed-page-images/tabbedpage-components.png "Karta TabbedPage składników")

Układ [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)i jego karty jest zależny od platformy:

- W systemach iOS w dolnej części ekranu zostanie wyświetlona lista kart, a obszar szczegółów jest powyżej. Każda karta ma również obrazu ikony, która powinna być 30 x 30 PNG o przezroczystość zwykłej rozdzielczości, 60 x 60 dla wysokiej rozdzielczości i 90 x 90 dla telefonu iPhone 6 Plus rozpoznawania. Jeśli istnieje więcej niż pięć kart *więcej* karta pojawi się, które mogą służyć do dodatkowe karty. Aby uzyskać więcej informacji na temat ładowania obrazów w aplikacji platformy Xamarin.Forms, zobacz [Praca z obrazami](~/xamarin-forms/user-interface/images.md). Aby uzyskać więcej informacji o wymaganiach dotyczących ikona, zobacz [tworzenie aplikacji na kartach](~/ios/user-interface/controls/creating-tabbed-applications.md).

    > [!NOTE]
  > Należy pamiętać, że `TabbedRenderer` dla systemu iOS ma możliwym do zastąpienia `GetIcon` metodę, która może służyć do obciążenia karty ikony pochodzących z określonego źródła. To zastąpienie pozwala używać obrazów SVG jako ikony na `TabbedPage`. Ponadto można podać zaznaczone i niezaznaczone wersji ikony.

- W systemie Android w górnej części ekranu zostanie wyświetlona lista kart, a w obszarze szczegółów znajduje się poniżej. Automatycznie kapitalizacji nazwy kart, a użytkownik można przewijać kolekcję kart, jeśli istnieje zbyt wiele do jednego ekranu.

    > [!NOTE]
  > Należy pamiętać, że przy użyciu AppCompat w systemie Android, każdej karcie będą również wyświetlane ikony. Ponadto `TabbedPageRenderer` dla systemu Android AppCompat ma możliwym do zastąpienia `SetTabIcon` metodę, która może służyć do załadować ikony kartę z niestandardowego `Drawable`. To zastąpienie pozwala używać obrazów SVG jako ikony na `TabbedPage`.

- Na Windows Phone w górnej części ekranu zostanie wyświetlona lista kart i obszaru szczegółów znajduje się poniżej. Jeśli zbyt wiele do jednego ekranu karty, których nazwy są konwertowane na małe litery, a użytkownik mogą być przewijane kolekcję kart.
- Na typu tablet z systemem Windows-rozmiarach, kartach nie zawsze są widoczne, a użytkownicy muszą Przejdź w dół (lub kliknij prawym przyciskiem myszy, jeśli mają myszy dołączony) do wyświetlania na kartach w `TabbedPage` (jak pokazano poniżej).

![](tabbed-page-images/windows-tabs.png "TabbedPage karty w systemie Windows")

## <a name="creating-a-tabbedpage"></a>Tworzenie TabbedPage

Dwa podejścia może służyć do tworzenia [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/):

- [Wypełnij](#Populating_a_TabbedPage_with_a_Page_Collection) [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) z kolekcją podrzędnego [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) obiektów, takich jak kolekcja [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpień.
- [Przypisz](#Populating_a_TabbedPage_with_a_Template) kolekcji [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) właściwości i przypisać [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) do [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) właściwości, aby powrócić do strony obiekty w kolekcji.

Z obu podejść [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) wyświetli każdej stronie, gdy użytkownik wybiera każdej karcie.

> [!NOTE]
> Zalecane jest [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) powinny zostać wypełnione [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) i [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)tylko wystąpienia. Pomoże to w celu zapewnienia spójną obsługę użytkowników na wszystkich platformach.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>Wypełnianie TabbedPage z kolekcją strony

Przedstawia poniższy przykładowy kod XAML [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) tworzony przez zapełnianie kolekcji elementu podrzędnego [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) obiektów:

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

Poniższy przykład kodu pokazuje odpowiednik [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) utworzony w języku C#:

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

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Jest wypełniane przy użyciu dwóch podrzędnych [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) obiektów. Pierwszy element podrzędny jest [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpienia, a druga karta jest [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) zawierający `ContentPage` wystąpienia.

> [!NOTE]
> **Uwaga**: [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) nie obsługuje interfejsu użytkownika wirtualizacji. W związku z tym może mieć wpływ na wydajność `TabbedPage` zawiera zbyt wiele elementów podrzędnych.

Pokaż poniższe zrzuty ekranu `TodayPage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpienia, która jest wyświetlana na *dzisiaj* karty:

![](tabbed-page-images/today-page.png "Wartość ContentPage w TabbedPage")

Wybieranie *harmonogram* karcie Wyświetla `SchedulePage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpienia, która jest ujęte w [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wystąpienia i jest wyświetlany w Poniższy zrzut ekranu:

![](tabbed-page-images/schedule-page.png "NavigationPage w TabbedPage")

Aby uzyskać informacje dotyczące układu [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), zobacz [wykonaniem nawigacji](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> Podczas gdy można umieścić [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) do [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), nie zaleca się umieszczenie `TabbedPage` do `NavigationPage`. Wynika to z faktu, w systemach iOS, `UITabBarController` zawsze działa jako otoki dla `UINavigationController`. Aby uzyskać więcej informacji, zobacz [łączyć interfejsy kontrolera widoku](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) w bibliotece deweloperów systemu iOS.

#### <a name="navigation-inside-a-tab"></a>Nawigacji wewnątrz karty

Nawigacji może być wykonywana w drugiej karcie wywołując [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) metoda [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) właściwość [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpienia, jak pokazano w poniższym przykładzie:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

Powoduje to `UpcomingAppointmentsPage` wystąpienia wypychana na stosie nawigacji, gdzie staje się stroną aktywną. Przedstawiono to na poniższych zrzutach ekranu:

![](tabbed-page-images/navigationpage.png "Nawigacji wewnątrz karty")

Aby uzyskać więcej informacji o wykonywaniu nawigacji za pomocą [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) , zobacz [hierarchiczna nawigacji](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Wypełnianie TabbedPage przy użyciu szablonu

Przedstawia poniższy przykładowy kod XAML [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) wykonane przez przypisanie [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) do [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) właściwości, aby powrócić do strony obiekty w kolekcji:

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

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Jest wypełniane przy użyciu danych przez ustawienie [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) właściwości w Konstruktorze pliku CodeBehind:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

Poniższy przykład kodu pokazuje odpowiednik [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) utworzony w języku C#:

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

Każdej karcie są wyświetlane [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) używającą szereg [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) i [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpienia, aby wyświetlić dane dla karty. Poniższe zrzuty ekranu Pokaż zawartość *Tamarin* karty:

![](tabbed-page-images/tab3.png "Wypełnianie TabbedPage przy użyciu szablonu")

Następnie należy wybrać inną kartę Wyświetla zawartość dla danej karty.

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Nie obsługuje interfejsu użytkownika wirtualizacji. W związku z tym może mieć wpływ na wydajność `TabbedPage` zawiera zbyt wiele elementów podrzędnych.

Aby uzyskać więcej informacji na temat [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), zobacz [25 rozdział](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Petzold Charlesa książki platformy Xamarin.Forms.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia TabbedPage poruszać się po jest zestawem stron. Platformy Xamarin.Forms [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) składa się z listy kart i większy obszar szczegółów, z każdej karcie ładowania zawartości w obszarze szczegółów.


## <a name="related-links"></a>Linki pokrewne

- [Odmian strony](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)
