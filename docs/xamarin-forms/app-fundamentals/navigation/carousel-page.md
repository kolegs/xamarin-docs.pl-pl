---
title: Strona karuzeli zestawu narzędzi Xamarin.Forms
description: CarouselPage zestawu narzędzi Xamarin.Forms to strona, które użytkownicy mogą szybko przesuń bok przechodzenie przez strony zawartości, takich jak Galeria. W tym artykule przedstawiono sposób użycia CarouselPage przechodzenie przez zbiór stron.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: bce3a60f3647a537906cfa11fc1dcfcc6f5cf365
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998614"
---
# <a name="xamarinforms-carousel-page"></a>Strona karuzeli zestawu narzędzi Xamarin.Forms

_CarouselPage zestawu narzędzi Xamarin.Forms to strona, które użytkownicy mogą szybko przesuń bok przechodzenie przez strony zawartości, takich jak Galeria. W tym artykule przedstawiono sposób użycia CarouselPage przechodzenie przez zbiór stron._

## <a name="overview"></a>Omówienie

Poniższe zrzuty ekranu Pokaż [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) na każdej z platform:

![](carousel-page-images/thirdpage.png "Element Thid CarouselPage")

Układ [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) jest taka sama na każdej platformie. Strony można nawigować przy użyciu, szybko przesuwając palcem od prawej do lewej Przejdź przekazuje kolekcji i szybko przesuwając palcem od lewej do prawej przechodzenie wstecz przez kolekcję. Poniższych zrzutach ekranu przedstawiono na pierwszej stronie [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) wystąpienie:

![](carousel-page-images/firstpage.png "CarouselPage pierwszy element")

Szybko przesuwając od prawej do lewej przenosi do drugiej strony, jak pokazano na poniższych zrzutach ekranu:

![](carousel-page-images/secondpage.png "Element CarouselPage sekundy")

Szybko przesuwając od prawej do lewej, ponownie przemieszczany do strony trzeciej, szybko przesuwając od lewej do prawej powraca do poprzedniej strony.

<!--
> [!NOTE]
> The [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](xref:Xamarin.Forms.CarouselView) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Tworzenie CarouselPage

Dwie metody mogą służyć do tworzenia [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage):

- [Wypełnij](#Populating_a_CarouselPage_with_a_Page_Collection) `CarouselPage` z kolekcją podrzędnych [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpień.
- [Przypisz](#Populating_a_CarouselPage_with_a_Template) kolekcję, aby [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) właściwości i przypisz [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) do [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) właściwości do zwrócenia [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpień dla obiektów w kolekcji.

Za pomocą obu metod `CarouselPage` zostanie następnie wyświetlana każda strona z kolei z interakcji przesunięcia, przejście do następnej strony do wyświetlenia.

> [!NOTE]
> A [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) tylko można wypełnić przy użyciu [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpień lub `ContentPage` pochodnych.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>Wypełnianie CarouselPage z kolekcją strony

Ilustruje poniższy przykład kodu XAML [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) wyświetlającą trzy [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpień:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <ContentPage>
        <ContentPage.Padding>
          <OnPlatform x:TypeArguments="Thickness">
              <On Platform="iOS, Android" Value="0,40,0,0" />
          </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
            <Label Text="Red" FontSize="Medium" HorizontalOptions="Center" />
            <BoxView Color="Red" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
</CarouselPage>
```

Poniższy przykład kodu pokazuje równoważne interfejsu użytkownika w języku C#:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        var redContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                Children = {
                    new Label {
                        Text = "Red",
                        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                        HorizontalOptions = LayoutOptions.Center
                    },
                    new BoxView {
                        Color = Color.Red,
                        WidthRequest = 200,
                        HeightRequest = 200,
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.CenterAndExpand
                    }
                }
            }
        };
        var greenContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };
        var blueContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };

        Children.Add (redContentPage);
        Children.Add (greenContentPage);
        Children.Add (blueContentPage);
    }
}
```

Każdy [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) po prostu wyświetla [ `Label` ](xref:Xamarin.Forms.Label) dla określonego koloru i [ `BoxView` ](xref:Xamarin.Forms.BoxView) tego koloru.

> [!NOTE]
> [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) Nie obsługuje wirtualizacja interfejsu użytkownika. W związku z tym, może mieć wpływ na wydajność `CarouselPage` zawiera zbyt wiele elementów podrzędnych.

Jeśli [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) osadzane w [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) strony [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) Właściwość powinna być ustawiona na `false` aby zapobiec konfliktom gestu między `CarouselPage` i `MasterDetailPage`.

Aby uzyskać więcej informacji na temat [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), zobacz [25 rozdział](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold Xamarin.Forms książki.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Wypełnianie CarouselPage przy użyciu szablonu

Ilustruje poniższy przykład kodu XAML [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) skonstruowany, przypisując [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) do [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) właściwości, aby powrócić do strony obiekty w kolekcji:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <CarouselPage.ItemTemplate>
        <DataTemplate>
            <ContentPage>
                <ContentPage.Padding>
                  <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS, Android" Value="0,40,0,0" />
                  </OnPlatform>
                </ContentPage.Padding>
                <StackLayout>
                    <Label Text="{Binding Name}" FontSize="Medium" HorizontalOptions="Center" />
                    <BoxView Color="{Binding Color}" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
                </StackLayout>
            </ContentPage>
        </DataTemplate>
    </CarouselPage.ItemTemplate>
</CarouselPage>
```

[ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) Wypełniony danymi, ustawiając [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) właściwość w Konstruktorze dla pliku związanym z kodem:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

Poniższy przykład kodu pokazuje odpowiednik [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) utworzone w języku C#:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        ItemTemplate = new DataTemplate (() => {
            var nameLabel = new Label {
                FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                HorizontalOptions = LayoutOptions.Center
            };
            nameLabel.SetBinding (Label.TextProperty, "Name");

            var colorBoxView = new BoxView {
                WidthRequest = 200,
                HeightRequest = 200,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.CenterAndExpand
            };
            colorBoxView.SetBinding (BoxView.ColorProperty, "Color");

            return new ContentPage {
                Padding = padding,
                Content = new StackLayout {
                    Children = {
                        nameLabel,
                        colorBoxView
                    }
                }
            };
        });

        ItemsSource = ColorsDataModel.All;
    }
}
```

Każdy [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) po prostu wyświetla [ `Label` ](xref:Xamarin.Forms.Label) dla określonego koloru i [ `BoxView` ](xref:Xamarin.Forms.BoxView) tego koloru.

> [!NOTE]
> [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) Nie obsługuje wirtualizacja interfejsu użytkownika. W związku z tym, może mieć wpływ na wydajność `CarouselPage` zawiera zbyt wiele elementów podrzędnych.

Jeśli [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) osadzane w [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) strony [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) Właściwość powinna być ustawiona na `false` aby zapobiec konfliktom gestu między `CarouselPage` i `MasterDetailPage`.

Aby uzyskać więcej informacji na temat [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), zobacz [25 rozdział](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold Xamarin.Forms książki.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) przechodzenie przez zbiór stron. `CarouselPage` To strona, które użytkownicy mogą szybko przesuń bok przechodzenie przez strony zawartości, podobnie jak w galerii.


## <a name="related-links"></a>Linki pokrewne

- [Różne typy stron](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](xref:Xamarin.Forms.CarouselPage)
