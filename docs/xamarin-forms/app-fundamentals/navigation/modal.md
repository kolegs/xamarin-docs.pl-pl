---
title: Modalne stron
description: Platformy Xamarin.Forms zapewnia obsługę modalne stron. Modalne strony zachęca użytkowników do ukończenia zadania niezależne, który nie może być opuszczeniu do czasu ukończenia zadania lub anulowane. W tym artykule pokazano, jak można przejść do strony modalne.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 909a04398043a3c2f0c30e4da82d174a6bfaf148
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="modal-pages"></a>Modalne stron

_Platformy Xamarin.Forms zapewnia obsługę modalne stron. Modalne strony zachęca użytkowników do ukończenia zadania niezależne, który nie może być opuszczeniu do czasu ukończenia zadania lub anulowane. W tym artykule pokazano, jak można przejść do strony modalne._

W tym artykule omówiono następujące zagadnienia:

- [Wykonywanie nawigacji](#Performing_Navigation) — wypychania stron w stosie modalne wyświetlanie stron ze stosu modalne wyłączenie przycisku Wstecz i animacji przejścia stron.
- [Przekazywanie danych podczas nawigowania](#Passing_Data_when_Navigating) — przekazywanie danych za pośrednictwem konstruktora strony oraz `BindingContext`.

## <a name="overview"></a>Omówienie

Modalne strony może być dowolny z [strony](~/xamarin-forms/user-interface/controls/pages.md) typy obsługiwanych przez platformy Xamarin.Forms. Należy wyświetlić stronę modalne aplikacji będzie wypchnąć go na stosie modalne, gdy stanie się aktywnej strony, jak pokazano na poniższym diagramie:

![](modal-images/pushing.png "Wypychanie modalne stos strony")

Aby powrócić do poprzedniej strony aplikacji będzie pop bieżącej strony z modalne stosu, a nowa strona najwyższego poziomu staje się aktywnej strony, jak pokazano na poniższym diagramie:

![](modal-images/popping.png "Wyświetlanie strony z modalne stosu")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Wykonywanie nawigacji

Modalne nawigacji metody są udostępniane przez [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) właściwości na dowolnym [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) typów pochodnych. Te metody umożliwiają [push modalne stron](#Pushing_Pages_to_the_Modal_Stack) na stosie modalne i [pop modalne stron](#Popping_Pages_from_the_Modal_Stack) ze stosu modalne.

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Udostępnia również właściwość [ `ModalStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) właściwości, z którego można uzyskać modalne stron modalne stosu. Jednak nie pojęcie wykonywania modalne stosem lub wyświetlanie do strony głównej modalne nawigacji. To dlatego te operacje nie są powszechnie obsługiwane na platformach podstawowej.

> [!NOTE]
> A [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wystąpienie nie jest wymagane do wykonywania Nawigacja strony modalne.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>Wypychanie stron modalne stosu

Aby przejść do `ModalPage` należy wywołać [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) metoda [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) właściwości bieżącej strony jako wykazały w poniższym przykładzie kodu:

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    ...
    await Navigation.PushModalAsync (detailPage);
  }
}
```

Powoduje to `ModalPage` wystąpienia wypychana na stosie modalne, gdzie staje się stroną aktywną, pod warunkiem wybranego elementu w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) na `MainPage` wystąpienia. `ModalPage` Wystąpienia jest wyświetlany na poniższych zrzutach ekranu:

![](modal-images/modalpage.png "Modalne przykład strony")

Gdy [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) zostanie wywołany, występują następujące zdarzenia:

- Wywoływanie strony `PushModalAsync` ma jego [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) zastąpienie wywołany, pod warunkiem, że platforma podstawowa nie jest Android.
- Na stronie do jego [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) zastąpienie wywołany.
- `PushAsync` Zakończeniu zadania.

Dokładne kolejność zdarzenia te występują jest jednak platformy zależnej. Aby uzyskać więcej informacji, zobacz [rozdziału 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Petzold Charlesa książki platformy Xamarin.Forms.

> [!NOTE]
> Wywołuje się [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) i [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) zastąpienia nie może być traktowany jako gwarantowane oznaczenia nawigacji strony. Na przykład w systemach iOS `OnDisappearing` zastąpienia jest wywoływana na stronie aktywne po zakończeniu działania aplikacji.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>Wyświetlanie stron z modalne stosu

Aktywna strona może zostać zdjęte ze stosu ze stosu modalne naciskając *ponownie* znajdującego się na urządzeniu, niezależnie od tego czy to fizyczne przycisk na urządzeniu lub na ekranie przycisku.

Aby programowo powrócić do oryginalnej strony `ModalPage` wystąpienia należy wywołać [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) metody, jak pokazano w poniższym przykładzie:

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

Powoduje to `ModalPage` wystąpienie ma zostać usunięty ze stosu modalne z nowej strony najwyższego poziomu staje się stroną aktywną. Gdy [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) zostanie wywołany, występują następujące zdarzenia:

- Wywoływanie strony `PopModalAsync` ma jego [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) zastąpienie wywołany.
- Na stronie są zwracane do jego [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) zastąpienie wywołany, pod warunkiem, że platforma podstawowa nie jest Android.
- `PopModalAsync` Zwraca zadanie.

Dokładne kolejność zdarzenia te występują jest jednak platformy zależnej. Aby uzyskać więcej informacji, zobacz [rozdziału 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Petzold Charlesa książki platformy Xamarin.Forms.

### <a name="disabling-the-back-button"></a>Wyłączenie przycisku Wstecz

W systemach Android i Windows Phone, użytkownik zawsze wrócić do poprzedniej strony, naciskając klawisz standardowego *ponownie* przycisk na urządzeniu. Jeśli strona modalne wymaga od użytkownika do ukończenia zadania niezależne przed opuszczeniem strony, należy wyłączyć aplikacji *ponownie* przycisku. Można to osiągnąć przez zastąpienie [ `Page.OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed/) metody na stronie modalne. Aby uzyskać więcej informacji, zobacz [rozdziału 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Petzold Charlesa książki platformy Xamarin.Forms.

### <a name="animating-page-transitions"></a>Animowanie przejścia stron

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Właściwości każdej strony są także przesłoniętych wypychania i pop metod, które obejmują `boolean` parametru, która kontroluje, czy ma być wyświetlana Animacja strony, podczas nawigacji, jak pokazano w poniższym kodzie przykład:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushModalAsync (new DetailPage (), false);
}

async void OnDismissButtonClicked (object sender, EventArgs args)
{
  // Page appearance not animated
  await Navigation.PopModalAsync (false);
}
```

Ustawienie `boolean` parametr `false` wyłącza animacji przejście do strony, podczas ustawienie dla parametru `true` umożliwia przejście do strony animacji, pod warunkiem, że jest to obsługiwane przez podstawowej platformy. Jednak metod wypychania i pop, których brakuje tego parametru animacji domyślnie włączone.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Przekazywanie danych podczas nawigowania

Czasami jest niezbędne dla strony do przekazania danych do innej strony podczas nawigacji. Są dwie metody realizacji tego zadania na przekazywanie danych za pośrednictwem Konstruktor strony i ustawiając Nowa strona [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) do danych. Każda będzie teraz omówione z kolei.

### <a name="passing-data-through-a-page-constructor"></a>Przekazywanie danych za pośrednictwem konstruktora strony

Najprostsza metoda przekazywania danych do innej strony podczas nawigacji odbywa się za pośrednictwem parametru konstruktora strony, które przedstawiono w poniższym przykładzie:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

Ten kod tworzy `MainPage` wystąpienia, przekazując bieżącą datę i godzinę w formacie ISO8601.

`MainPage` Wystąpienia odbiera dane za pomocą parametru konstruktora, jak pokazano w poniższym przykładzie:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Dane są następnie wyświetlane na stronie przez ustawienie [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) właściwości.

### <a name="passing-data-through-a-bindingcontext"></a>Przekazywanie danych za pośrednictwem BindingContext

Informacje o innym podejściu do przekazywania danych do innej strony podczas nawigacji jest ustawiając nowej strony [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) do danych, jak pokazano w poniższym przykładzie:

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    detailPage.BindingContext = e.SelectedItem as Contact;
    listView.SelectedItem = null;
    await Navigation.PushModalAsync (detailPage);
  }
}
```

Ten kod ustawia [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) z `DetailPage` wystąpienie do `Contact` wystąpienia, a następnie przechodzi do `DetailPage`.

`DetailPage` Następnie używa wiązanie danych do wyświetlenia `Contact` wystąpienie danych, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ModalNavigation.DetailPage">
    <ContentPage.Padding>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,40,0,0" />
      </OnPlatform>
    </ContentPage.Padding>
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" FontSize="Medium" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
              ...
            <Button x:Name="dismissButton" Text="Dismiss" Clicked="OnDismissButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Poniższy przykład kodu pokazuje, jak można wykonać wiązania danych w języku C#:

```csharp
public class DetailPageCS : ContentPage
{
  public DetailPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var dismissButton = new Button { Text = "Dismiss" };
    dismissButton.Clicked += OnDismissButtonClicked;

    Thickness padding;
    switch (Device.RuntimePlatform)
    {
        case Device.iOS:
            padding = new Thickness(0, 40, 0, 0);
            break;
        default:
            padding = new Thickness();
            break;
    }

    Padding = padding;
    Content = new StackLayout {
      HorizontalOptions = LayoutOptions.Center,
      VerticalOptions = LayoutOptions.Center,
      Children = {
        new StackLayout {
          Orientation = StackOrientation.Horizontal,
          Children = {
            new Label{ Text = "Name:", FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)), HorizontalOptions = LayoutOptions.FillAndExpand },
            nameLabel
          }
        },
        ...
        dismissButton
      }
    };
  }

  async void OnDismissButtonClicked (object sender, EventArgs args)
  {
    await Navigation.PopModalAsync ();
  }
}
```

Dane są następnie wyświetlane na stronie przez szereg [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontrolki.

Aby uzyskać więcej informacji na temat wiązania danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób przejścia do strony modalne. Modalne strony zachęca użytkowników do ukończenia zadania niezależne, który nie może być opuszczeniu do czasu ukończenia zadania lub anulowane.


## <a name="related-links"></a>Linki pokrewne

- [Nawigacja strony](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Modalne (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
