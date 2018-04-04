---
title: Strona Karuzela
description: CarouselPage platformy Xamarin.Forms jest strony, który użytkownicy mogą szybko przesuń strony do przechodzenia przez strony zawartości, takich jak galerii. W tym artykule przedstawiono sposób użycia CarouselPage poruszać się po jest zestawem stron.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: d55d8c8d98828097c842cc383037db88097b963d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="carousel-page"></a>Strona Karuzela

_CarouselPage platformy Xamarin.Forms jest strony, który użytkownicy mogą szybko przesuń strony do przechodzenia przez strony zawartości, takich jak galerii. W tym artykule przedstawiono sposób użycia CarouselPage poruszać się po jest zestawem stron._

## <a name="overview"></a>Omówienie

Pokaż poniższe zrzuty ekranu [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) na każdej platformie:

![](carousel-page-images/thirdpage.png "Element Thid CarouselPage")

Układ [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) jest taka sama na każdej z platform. Strony może zostać przesłane za pośrednictwem przesuwając od prawej do lewej można przejść do przodu za pomocą kolekcji i przesuwając od lewej do prawej strony, aby przejść wstecz za pomocą kolekcji. Poniższe zrzuty ekranu Pokaż na pierwszej stronie [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) wystąpienie:

![](carousel-page-images/firstpage.png "Pierwszy CarouselPage elementu")

Szybko przesuwając od prawej do lewej przenosi do drugiej strony, jak pokazano na poniższych zrzutach ekranu:

![](carousel-page-images/secondpage.png "Element CarouselPage sekundę")

Szybko przesuwając od prawej do lewej ponownie przemieszczany na stronie trzeciej, szybko przesuwając od lewej do prawej zwraca do poprzedniej strony.

<!--
> [!NOTE]
> The [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Tworzenie CarouselPage

Dwa podejścia może służyć do tworzenia [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/):

- [Wypełnij](#Populating_a_CarouselPage_with_a_Page_Collection) `CarouselPage` z kolekcją podrzędnego [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpień.
- [Przypisz](#Populating_a_CarouselPage_with_a_Template) kolekcji [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) właściwości i przypisać [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) do [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) właściwości do zwrócenia [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpień obiektów w kolekcji.

Z obu podejść `CarouselPage` zostanie następnie wyświetlania każdej strony z kolei z interakcji Przejdź, przeniesienie do następnej strony do wyświetlenia. To środowisko nawigacji zostanie uznać fizycznych i znany użytkownikom Windows Phone.

> [!NOTE]
> A [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) tylko można wypełniać za pomocą [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpień, lub `ContentPage` pochodnych.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>Wypełnianie CarouselPage z kolekcją strony

Przedstawia poniższy przykładowy kod XAML [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) wyświetlający trzy [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpień:

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

Każdy [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) po prostu wyświetla [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) dla określonego koloru i [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) tego koloru.

> [!NOTE]
> [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Nie obsługuje interfejsu użytkownika wirtualizacji. W związku z tym może mieć wpływ na wydajność `CarouselPage` zawiera zbyt wiele elementów podrzędnych.

Jeśli [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) jest osadzony w [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) strony [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) Właściwość powinna być ustawiona na `false` aby zapobiec konfliktom gestu między `CarouselPage` i `MasterDetailPage`.

Aby uzyskać więcej informacji na temat [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), zobacz [25 rozdział](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Petzold Charlesa książki platformy Xamarin.Forms.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Wypełnianie CarouselPage przy użyciu szablonu

Przedstawia poniższy przykładowy kod XAML [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) wykonane przez przypisanie [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) do [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) właściwości, aby powrócić do strony obiekty w kolekcji:

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

[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Jest wypełniane przy użyciu danych przez ustawienie [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) właściwości w Konstruktorze pliku CodeBehind:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

Poniższy przykład kodu pokazuje odpowiednik [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) utworzony w języku C#:

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

Każdy [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) po prostu wyświetla [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) dla określonego koloru i [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) tego koloru.

> [!NOTE]
> [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Nie obsługuje interfejsu użytkownika wirtualizacji. W związku z tym może mieć wpływ na wydajność `CarouselPage` zawiera zbyt wiele elementów podrzędnych.

Jeśli [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) jest osadzony w [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) strony [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) Właściwość powinna być ustawiona na `false` aby zapobiec konfliktom gestu między `CarouselPage` i `MasterDetailPage`.

Aby uzyskać więcej informacji na temat [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), zobacz [25 rozdział](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Petzold Charlesa książki platformy Xamarin.Forms.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) poruszać się po jest zestawem stron. `CarouselPage` Stronę, które użytkownicy mogą szybko przesuń strony do przechodzenia przez strony zawartości, takich jak galerii i zapewnia obsługę nawigacji, który tak jak fizyczną i znany użytkownikom Windows Phone.


## <a name="related-links"></a>Linki pokrewne

- [Odmian strony](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)
