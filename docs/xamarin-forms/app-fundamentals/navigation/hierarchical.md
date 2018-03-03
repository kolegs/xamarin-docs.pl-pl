---
title: Hierarchiczna nawigacji
description: "Klasa NavigationPage udostępnia środowisko hierarchiczna nawigacji, gdy użytkownik jest powinni poruszać się po stronach przodu i do tyłu, zgodnie z potrzebami. Klasa implementuje nawigacji jako ostatni na, wytworzenia stosu obiektów strony. W tym artykule przedstawiono sposób użycia klasy NavigationPage w celu wykonania nawigacji w stosie stron."
ms.topic: article
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 95fc958beb71cba8f4d575eaa96d0612aa458966
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="hierarchical-navigation"></a>Hierarchiczna nawigacji

_Klasa NavigationPage udostępnia środowisko hierarchiczna nawigacji, gdy użytkownik jest powinni poruszać się po stronach przodu i do tyłu, zgodnie z potrzebami. Klasa implementuje nawigacji jako ostatni na, wytworzenia stosu obiektów strony. W tym artykule przedstawiono sposób użycia klasy NavigationPage w celu wykonania nawigacji w stosie stron._

W tym artykule omówiono następujące zagadnienia:

- [Wykonywanie nawigacji](#Performing_Navigation) — Tworzenie strony głównej, wypychania stron w stosie nawigacji wyświetlanie stron z stos nawigacji i animacji przejścia stron.
- [Przekazywanie danych podczas nawigowania](#Passing_Data_when_Navigating) — przekazywanie danych za pośrednictwem konstruktora strony oraz `BindingContext`.
- [Manipulowanie stos nawigacji](#Manipulating_the_Navigation_Stack) — manipulowanie stosu oddzielonych, wstawiając lub usuwając je.

## <a name="overview"></a>Omówienie

Aby przenieść się z jednej strony, aplikacji przeprowadzi wypychanie nową stronę na stosie nawigacji, gdy stanie się aktywnej strony, jak pokazano na poniższym diagramie:

![](hierarchical-images/pushing.png "Wypychanie stronę stos nawigacji")

Aby wrócić do poprzedniej strony, aplikacja będzie pop bieżącej strony z stos nawigacji, a nowa strona najwyższego poziomu staje się aktywnej strony, jak pokazano na poniższym diagramie:

![](hierarchical-images/popping.png "Wyświetlanie strony z stos nawigacji")

Metody nawigacji są udostępniane przez [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) właściwości na dowolnym [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) typów pochodnych. Te metody umożliwiają do dystrybuowania stron na stosie nawigacji do pop stron z stos nawigacji i wykonywania stosem.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Wykonywanie nawigacji

W hierarchicznej nawigacji [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) klasa jest używana do nawigowania w stosie [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) obiektów. Poniższe zrzuty ekranu Pokaż głównymi składnikami `NavigationPage` na każdej platformie:

![](hierarchical-images/navigationpage-components.png "Składniki NavigationPage")

Układ [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) jest zależny od platformy:

- W systemach iOS, pasek nawigacyjny jest obecny w górnej części strony, która wyświetla tytuł i ma *ponownie* przycisku, który zwraca do poprzedniej strony.
- W systemie Android, jest obecny w górnej części strony, która wyświetla tytuł, ikona na pasku nawigacji i *ponownie* przycisku, który zwraca do poprzedniej strony. Ikona jest zdefiniowany w `[Activity]` atrybut, który decorates `MainActivity` klasy w projekcie specyficzne dla platformy systemu Android.
- Na Windows Phone pasek nawigacyjny jest obecny w górnej części strony, która wyświetla tytuł. Windows Phone nie ma *ponownie* znajdującego się na pasku nawigacyjnym, ponieważ na ekranie *ponownie* przycisk znajduje się w dolnej części ekranu.

Na platformach, wartość [ `Page.Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) właściwości będzie wyświetlany jako tytuł strony.

> [!NOTE]
> Zalecane jest `NavigationPage` powinny zostać wypełnione `ContentPage` tylko wystąpienia.

### <a name="creating-the-root-page"></a>Tworzenie strony głównej

Pierwsza strona dodane stos nawigacji jest określany jako *głównego* strony aplikacji i poniższy przykład kodu pokazuje, jak to zrobić:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

Powoduje to `Page1Xaml` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpienia wypychana na stosie nawigacji, gdzie staje się stroną aktywną i na stronie katalogu głównego aplikacji. Przedstawiono to na poniższych zrzutach ekranu:

![](hierarchical-images/mainpage.png "Strona główny stos nawigacji")

> [!NOTE]
> [ `RootPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.RootPage/) Właściwość [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wystąpienia zapewnia dostęp do pierwszej strony w stosie nawigacji.

### <a name="pushing-pages-to-the-navigation-stack"></a>Wypychanie stron na stos nawigacji

Aby przejść do `Page2Xaml`, należy wywołać [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) metoda [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) właściwości bieżącej strony jako wykazały w poniższym przykładzie kodu:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

Powoduje to `Page2Xaml` wystąpienia wypychana na stosie nawigacji, gdzie staje się stroną aktywną. Przedstawiono to na poniższych zrzutach ekranu:

![](hierarchical-images/secondpage.png "Strona wypychana na stosie nawigacji")

Gdy [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) wywoływana jest metoda, występują następujące zdarzenia:

- Wywoływanie strony `PushAsync` ma jego [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) zastąpienie wywołany.
- Na stronie do jego [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) zastąpienie wywołany.
- `PushAsync` Zakończeniu zadania.

Dokładne kolejność, w którym te zdarzenia występują jest jednak platformy zależnej. Aby uzyskać więcej informacji, zobacz [rozdziału 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Petzold Charlesa książki platformy Xamarin.Forms.

> [!NOTE]
> Wywołuje się [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) i [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) zastąpienia nie może być traktowany jako gwarantowane oznaczenia nawigacji strony. Na przykład w systemach iOS `OnDisappearing` zastąpienia jest wywoływana na stronie aktywne po zakończeniu działania aplikacji.

### <a name="popping-pages-from-the-navigation-stack"></a>Wyświetlanie stron z stos nawigacji

Aktywna strona może zostać zdjęte ze stosu ze stosu nawigacji, naciskając klawisz *ponownie* znajdującego się na urządzeniu, niezależnie od tego czy to fizyczne przycisk na urządzeniu lub na ekranie przycisku.

Aby programowo powrócić do oryginalnej strony `Page2Xaml` wystąpienia należy wywołać [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) metody, jak pokazano w poniższym przykładzie:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

Powoduje to `Page2Xaml` wystąpienia, aby były usuwane z stos nawigacji z nowej strony najwyższego poziomu staje się stroną aktywną. Gdy [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) wywoływana jest metoda, występują następujące zdarzenia:

- Wywoływanie strony `PopAsync` ma jego [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) zastąpienie wywołany.
- Na stronie są zwracane do jego [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) zastąpienie wywołany.
- `PopAsync` Zwraca zadanie.

Dokładne kolejność, w którym te zdarzenia występują jest jednak platformy zależnej. Aby uzyskać więcej informacji, zobacz [rozdziału 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Petzold Charlesa książki platformy Xamarin.Forms.

A także [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) i [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) metod, [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) właściwości każdej strony są także [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopToRootAsync()/) metodę, która jest wyświetlana w poniższym przykładzie:

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

Ta metoda powoduje jednak głównego [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) stosu nawigacji co strony głównej aplikacji aktywnej strony.

### <a name="animating-page-transitions"></a>Animowanie przejścia stron

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Właściwości każdej strony są także przesłoniętych wypychania i pop metod, które obejmują `boolean` parametru, która kontroluje, czy ma być wyświetlana Animacja strony, podczas nawigacji, jak pokazano w poniższym kodzie przykład:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushAsync (new Page2Xaml (), false);
}

async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopAsync (false);
}

async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopToRootAsync (false);
}
```

Ustawienie `boolean` parametr `false` wyłącza animacji przejście do strony, podczas ustawienie dla parametru `true` umożliwia przejście do strony animacji, pod warunkiem, że jest to obsługiwane przez podstawowej platformy. Jednak metod wypychania i pop, których brakuje tego parametru animacji domyślnie włączone.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Przekazywanie danych podczas nawigowania

Czasami jest niezbędne dla strony do przekazania danych do innej strony podczas nawigacji. Dwie metody realizacji tego zadania są przekazywania danych za pośrednictwem Konstruktor strony i ustawiając Nowa strona [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) do danych. Każda będzie teraz omówione z kolei.

### <a name="passing-data-through-a-page-constructor"></a>Przekazywanie danych za pośrednictwem konstruktora strony

Najprostsza metoda przekazywania danych do innej strony podczas nawigacji odbywa się za pośrednictwem parametru konstruktora strony, które przedstawiono w poniższym przykładzie:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

Ten kod tworzy `MainPage` wystąpienia, przekazując bieżącą datę i godzinę w formacie ISO8601, który jest ujęte w [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wystąpienia.

`MainPage` Wystąpienia odbiera dane za pomocą parametru konstruktora, jak pokazano w poniższym przykładzie:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Dane są następnie wyświetlane na stronie przez ustawienie [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) właściwości, jak pokazano na poniższych zrzutach ekranu:

![](hierarchical-images/passing-data-constructor.png "Dane przekazywane Konstruktor strony")

### <a name="passing-data-through-a-bindingcontext"></a>Przekazywanie danych za pośrednictwem BindingContext

Informacje o innym podejściu do przekazywania danych do innej strony podczas nawigacji jest ustawiając nowej strony [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) do danych, jak pokazano w poniższym przykładzie:

```csharp
async void OnNavigateButtonClicked (object sender, EventArgs e)
{
  var contact = new Contact {
    Name = "Jane Doe",
    Age = 30,
    Occupation = "Developer",
    Country = "USA"
  };

  var secondPage = new SecondPage ();
  secondPage.BindingContext = contact;
  await Navigation.PushAsync (secondPage);
}
```

Ten kod ustawia [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) z `SecondPage` wystąpienie do `Contact` wystąpienia, a następnie przechodzi do `SecondPage`.

`SecondPage` Następnie używa wiązanie danych do wyświetlenia `Contact` wystąpienie danych, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="PassingData.SecondPage"
             Title="Second Page">
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
            ...
            <Button x:Name="navigateButton" Text="Previous Page" Clicked="OnNavigateButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Poniższy przykład kodu pokazuje, jak można wykonać wiązania danych w języku C#:

```csharp
public class SecondPageCS : ContentPage
{
  public SecondPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var navigateButton = new Button { Text = "Previous Page" };
    navigateButton.Clicked += OnNavigateButtonClicked;

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
        navigateButton
      }
    };
  }

  async void OnNavigateButtonClicked (object sender, EventArgs e)
  {
    await Navigation.PopAsync ();
  }
}
```

Dane są następnie wyświetlane na stronie przez szereg [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Określa, jak pokazano na poniższych zrzutach ekranu:

![](hierarchical-images/passing-data-bindingcontext.png "Dane przekazywane BindingContext")

Aby uzyskać więcej informacji na temat wiązania danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>Manipulowanie stos nawigacji

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Ujawnia właściwości [ `NavigationStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) właściwości, z którego można uzyskać stron w stosie nawigacji. Gdy platformy Xamarin.Forms obsługuje dostęp do stos nawigacji `Navigation` zawiera właściwość [ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) i [ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) metody stosu wstawiając strony lub usuwania ich.

[ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) Metody wstawia określonej strony w stosie nawigacji przed istniejącej określonej strony, jak pokazano na poniższym diagramie:

![](hierarchical-images/insert-page-before.png "Wstawianie strony w stosie nawigacji")

[ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) Metoda usuwa określonej strony stos nawigacji, jak pokazano na poniższym diagramie:

![](hierarchical-images/remove-page.png "Usuwanie strony z stos nawigacji")

Te metody włączyć środowisko do nawigacji niestandardowe, takie jak zastąpienie nowej strony, po pomyślnym zalogowaniu strony logowania. W poniższym przykładzie kodu pokazano tego scenariusza:

```csharp
async void OnLoginButtonClicked (object sender, EventArgs e)
{
  ...
  var isValid = AreCredentialsCorrect (user);
  if (isValid) {
    App.IsUserLoggedIn = true;
    Navigation.InsertPageBefore (new MainPage (), this);
    await Navigation.PopAsync ();
  } else {
    // Login failed
  }
}

```

Pod warunkiem, że poświadczenia użytkownika są prawidłowe, `MainPage` wystąpienia są wstawiane do stos nawigacji przed bieżącej strony. [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) Metoda następnie usuwa bieżącej strony z stos nawigacji, z `MainPage` wystąpienia staje się stroną aktywną.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) klasy w celu wykonania nawigacji w stosie stron. Ta klasa udostępnia środowisko hierarchiczna nawigacji, gdy użytkownik jest powinni poruszać się po stronach przodu i do tyłu, zgodnie z potrzebami. Klasa implementuje nawigacji jako ostatni na, wytworzenia stos [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) obiektów.


## <a name="related-links"></a>Linki pokrewne

- [Nawigacja strony](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Hierarchiczne (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [Jak utworzyć znak w przepływie ekranu w przykładowym platformy Xamarin.Forms (Xamarin University wideo)](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Jak utworzyć znak w przepływie ekranu w platformy Xamarin.Forms (Xamarin University wideo)](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)
