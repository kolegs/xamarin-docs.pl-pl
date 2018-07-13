---
title: Strony modalne zestawu narzędzi Xamarin.Forms
description: Zestaw narzędzi Xamarin.Forms zapewnia obsługę strony modalne. Strony modalne zaleca użytkownikom ukończenie niezależna zadanie, które nie mogą być opuszczeniu aż zadanie jest ukończone lub anulowane. W tym artykule przedstawiono sposób przejścia do strony modalne.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 44aee8500c7de2ae56b59049368d6025ec49cc5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994831"
---
# <a name="xamarinforms-modal-pages"></a>Strony modalne zestawu narzędzi Xamarin.Forms

_Zestaw narzędzi Xamarin.Forms zapewnia obsługę strony modalne. Strony modalne zaleca użytkownikom ukończenie niezależna zadanie, które nie mogą być opuszczeniu aż zadanie jest ukończone lub anulowane. W tym artykule przedstawiono sposób przejścia do strony modalne._

W tym artykule omówiono następujące tematy:

- [Wykonywanie nawigacji](#Performing_Navigation) — wypychanie strony modalne stosu zdejmowanie strony ze stosu modalne wyłączenie przycisku Wstecz i animowanie przejścia stron.
- [Przekazywanie danych podczas nawigowania](#Passing_Data_when_Navigating) — przekazywanie danych za pośrednictwem konstruktora strony, a także za pośrednictwem `BindingContext`.

## <a name="overview"></a>Omówienie

Strony modalne może być dowolny z [strony](~/xamarin-forms/user-interface/controls/pages.md) typy obsługiwane przez zestaw narzędzi Xamarin.Forms. Można wyświetlić strony modalne aplikacji będzie umożliwiać wypychanie powiadomień go na stosie modalne, gdy stanie się stroną aktywną, jak pokazano na poniższym diagramie:

![](modal-images/pushing.png "Wypychanie strony modalne stosu")

Aby powrócić do poprzedniej strony aplikacji zostanie wyświetlona bieżącej strony z modalne stosu i Nowa strona najwyższego poziomu staje się stroną aktywną jak pokazano na poniższym diagramie:

![](modal-images/popping.png "Usuwanie strony ze stosu modalne")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Wykonywanie nawigacji

Modalne nawigacji metody są udostępniane przez [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) właściwości na dowolnym [ `Page` ](xref:Xamarin.Forms.Page) typów pochodnych. Metody te zapewniają możliwość [wypychania strony modalne](#Pushing_Pages_to_the_Modal_Stack) na stosie modalne i [pop strony modalne](#Popping_Pages_from_the_Modal_Stack) z modalne stosu.

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Udostępnia również właściwość [ `ModalStack` ](xref:Xamarin.Forms.INavigation.ModalStack) właściwości, z którego można uzyskać strony modalne modalne stosu. Jednakże nie obowiązuje koncepcja wykonywania stosem modalne lub usuwanie do strony głównej w modalnym nawigacji. Jest to spowodowane te operacje nie są powszechnie obsługiwane na platformach bazowego.

> [!NOTE]
> A [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) wystąpienia nie jest wymagane do wykonywania Nawigacja strony modalne.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>Wypychanie strony modalne stosu

Aby przejść do `ModalPage` należy wywołać [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) metody [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) właściwości bieżącej strony, jakie wykazano w poniższym przykładzie kodu:

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

Powoduje to, że `ModalPage` wystąpienia, które ma zostać wypchnięty na stos modalne, gdzie staje się stroną aktywną, pod warunkiem wybranego elementu w [ `ListView` ](xref:Xamarin.Forms.ListView) na `MainPage` wystąpienia. `ModalPage` Na poniższych zrzutach ekranu przedstawiono wystąpienie:

![](modal-images/modalpage.png "Przykładowa strona modalne")

Gdy [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) zostanie wywołana, zachodzą następujące zdarzenia:

- Wywoływanie strony `PushModalAsync` ma jego [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) zastąpienie wywołana, pod warunkiem, że podstawowej platformy nie jest systemu Android.
- Na stronie do jego [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) zastąpienie wywołana.
- `PushAsync` Zadanie zostanie ukończone.

Jednak dokładne kolejność te zdarzenia występują to platforma zależnych. Aby uzyskać więcej informacji, zobacz [rozdziału 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms książki.

> [!NOTE]
> Wywołania [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) i [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) przesłonięcia nie może być traktowany jako gwarantowane oznaczenia nawigacji strony. Na przykład w systemie iOS `OnDisappearing` zastąpienie jest wywoływana na aktywnej stronie, gdy kończy działanie aplikacji.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>Zdejmowanie strony ze stosu modalne

Aktywnej strony mogą zostać zdjęte ze stosu z modalne stosu, naciskając klawisz *ponownie* znajdujący się na urządzeniu, bez względu na to czy jest to przycisku fizycznego na urządzeniu lub przycisk na ekranie.

Aby programowo powrócić do oryginalnej strony `ModalPage` wystąpienie musi zostać wywołany [ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync) metody, jak pokazano w poniższym przykładzie kodu:

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

Powoduje to, że `ModalPage` wystąpienia do usunięcia ze stosu modalne przy użyciu nowej strony najwyższego poziomu, staje się stroną aktywną. Gdy [ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync) zostanie wywołana, zachodzą następujące zdarzenia:

- Wywoływanie strony `PopModalAsync` ma jego [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) zastąpienie wywołana.
- Na stronie są zwracane do jego [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) zastąpienie wywołana, pod warunkiem, że podstawowej platformy nie jest systemu Android.
- `PopModalAsync` Zadanie zwraca.

Jednak dokładne kolejność te zdarzenia występują to platforma zależnych. Aby uzyskać więcej informacji, zobacz [rozdziału 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms książki.

### <a name="disabling-the-back-button"></a>Wyłączenie przycisku Wstecz

W systemie Android użytkownik zawsze możesz wrócić do poprzedniej strony, naciskając klawisz standard *ponownie* przycisk na urządzeniu. Jeśli strony modalne wymaga od użytkownika do ukończenia zadania niezależna przed opuszczeniem strony, aplikacji, musisz wyłączyć *ponownie* przycisku. Można to osiągnąć przez zastąpienie [ `Page.OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed) metody na stronie modalnych. Aby uzyskać więcej informacji, zobacz [rozdziału 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms książki.

### <a name="animating-page-transitions"></a>Animowanie przejścia stron

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Właściwości każdej strony udostępnia również zastąpiona wypychania i pop metod, które obejmują `boolean` parametr, który kontroluje, czy ma być wyświetlana Animacja strony, podczas nawigacji, jak pokazano w poniższym kodzie przykład:

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

Ustawienie `boolean` parametr `false` wyłącza animacji przejście do strony, podczas ustawienie dla parametru `true` umożliwia przejście do strony animację, pod warunkiem, że nie jest obsługiwany przez możliwości platformy. Jednak metody wypychania i pop, które nie mają tego parametru włączyć animacji domyślnie.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Przekazywanie danych podczas nawigowania

Czasami jest konieczne dla strony przekazać dane do innej strony podczas nawigowania. Dwie techniki realizacji tego zadania są przez przekazywanie danych za pośrednictwem konstruktora strony i ustawiając nową stronę [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) do danych. Każda będzie teraz omawiane kolejno.

### <a name="passing-data-through-a-page-constructor"></a>Przekazywanie danych za pośrednictwem konstruktora strony

Najprostsza metoda przekazywania danych do innej strony podczas nawigacji jest za pośrednictwem strony parametr konstruktora, który jest pokazany w poniższym przykładzie kodu:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

Ten kod tworzy `MainPage` wystąpienia, przekazując bieżącą datę i godzinę w formacie ISO8601.

`MainPage` Wystąpienia odbiera dane za pomocą parametru konstruktora, jak pokazano w poniższym przykładzie kodu:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Dane są następnie wyświetlane na stronie przez ustawienie [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) właściwości.

### <a name="passing-data-through-a-bindingcontext"></a>Przekazywanie danych za pośrednictwem elementu BindingContext

Inną metodą przekazywania danych do innej strony podczas nawigacji jest ustawiając nową stronę [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) danych, jak pokazano w poniższym przykładzie kodu:

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

Ten kod ustawia [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) z `DetailPage` wystąpienia do `Contact` wystąpienia, a następnie przechodzi do `DetailPage`.

`DetailPage` Użyto powiązania danych, aby wyświetlić `Contact` wystąpienie danych, jak pokazano w poniższym przykładzie kodu XAML:

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

Poniższy przykład kodu pokazuje, jak można osiągnąć powiązanie danych w języku C#:

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

Dane są następnie wyświetlane na stronie przez szereg [ `Label` ](xref:Xamarin.Forms.Label) kontrolki.

Aby uzyskać więcej informacji na temat tworzenia powiązań danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób przejścia do strony modalne. Strony modalne zaleca użytkownikom ukończenie niezależna zadanie, które nie mogą być opuszczeniu aż zadanie jest ukończone lub anulowane.


## <a name="related-links"></a>Linki pokrewne

- [Nawigacja strony](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Modalne (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
