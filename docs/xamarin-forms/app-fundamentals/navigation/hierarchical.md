---
title: Nawigacja hierarchiczna
description: W tym artykule przedstawiono sposób użycia klasy NavigationPage przeprowadzić nawigacji w stosie ostatni na wejściu, first-out (LIFO) stron.
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: f8f8f9b4e5755e8b1707178fef633321b64e4e94
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994679"
---
# <a name="hierarchical-navigation"></a>Nawigacja hierarchiczna

_Klasa NavigationPage udostępnia środowisko Nawigacja hierarchiczna, gdzie użytkownik jest powinni poruszać się po stronach przodu i do tyłu, zgodnie z potrzebami. Klasa implementuje nawigacji jako ostatni na wejściu, first-out (LIFO) stosu obiektów strony. W tym artykule przedstawiono sposób użycia klasy NavigationPage przeprowadzić nawigacji w stosie stron._

W tym artykule omówiono następujące tematy:

- [Wykonywanie nawigacji](#Performing_Navigation) — Tworzenie strony głównej, wypychanie stron na stos nawigacji, usuwanie stron z stos nawigacji i animowanie przejścia stron.
- [Przekazywanie danych podczas nawigowania](#Passing_Data_when_Navigating) — przekazywanie danych za pośrednictwem konstruktora strony, a także za pośrednictwem `BindingContext`.
- [Manipulowanie stos nawigacji](#Manipulating_the_Navigation_Stack) — manipulowanie stosu, wstawiając lub usuwając strony.

## <a name="overview"></a>Omówienie

Aby przenieść się z jednej strony, aplikacja będzie umożliwiać wypychanie powiadomień nową stronę na stosie nawigacji, gdy stanie się stroną aktywną, jak pokazano na poniższym diagramie:

![](hierarchical-images/pushing.png "Wypychanie strony stosu nawigacji")

Aby wrócić do poprzedniej strony, aplikacja wyświetli bieżącą stronę ze stosu nawigacji, a nowa strona najwyższego poziomu staje się stroną aktywną, jak pokazano na poniższym diagramie:

![](hierarchical-images/popping.png "Usuwanie strony ze stosu nawigacji")

Metody nawigacji są udostępniane przez [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) właściwości na dowolnym [ `Page` ](xref:Xamarin.Forms.Page) typów pochodnych. Te metody umożliwiają wypychania stron na stosie nawigacji do stron zdejmowania ze stosu nawigacji, a także wykonywać operacje na stosie.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Wykonywanie nawigacji

W obszarze nawigacji hierarchiczne [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) klasa jest używana do nawigowania w stos [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) obiektów. Poniższych zrzutach ekranu przedstawiono główne składniki `NavigationPage` na każdej z platform:

![](hierarchical-images/navigationpage-components.png "Składniki NavigationPage")

Układ [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) jest zależny od platformy:

- W systemie iOS znajduje się w górnej części strony, która wyświetla tytuł i ma pasku nawigacyjnym *ponownie* przycisku, który zwraca do poprzedniej strony.
- W systemie Android pasek nawigacyjny znajduje się w górnej części strony, która wyświetla tytuł, ikona i *ponownie* przycisku, który zwraca do poprzedniej strony. Ikona jest zdefiniowany w `[Activity]` atrybut, który rozszerza `MainActivity` klasy w projekcie dla konkretnej platformy systemu Android.
- Na platformie Universal Windows paska nawigacji jest obecny w górnej części strony, która wyświetla tytuł.

Na wszystkich platformach, wartość [ `Page.Title` ](xref:Xamarin.Forms.Page.Title) właściwości, które będą wyświetlane jako tytuł strony.

> [!NOTE]
> Zalecamy, aby `NavigationPage` powinny zostać wypełnione `ContentPage` tylko wystąpień.

### <a name="creating-the-root-page"></a>Tworzenie strony głównej

Pierwsza strona dodane do stos nawigacji jest określany jako *głównego* strony aplikacji i poniższy przykład kodu pokazuje, jak to zrobić:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

Powoduje to, że `Page1Xaml` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpienia, które ma zostać wypchnięty na stos nawigacji, gdzie staje się stroną aktywną i strony głównej aplikacji. Na poniższych zrzutach ekranu przedstawiono to:

![](hierarchical-images/mainpage.png "Główny strony stosu nawigacji")

> [!NOTE]
> [ `RootPage` ](xref:Xamarin.Forms.NavigationPage.RootPage) Właściwość [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) wystąpienia zapewnia dostęp do pierwszej strony w stosie nawigacji.

### <a name="pushing-pages-to-the-navigation-stack"></a>Wypychanie stron na stosie nawigacji

Aby przejść do `Page2Xaml`, należy wywołać [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) metody [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) właściwości bieżącej strony, jakie wykazano w poniższym przykładzie kodu:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

Powoduje to, że `Page2Xaml` wystąpienia, które ma zostać wypchnięty na stos nawigacji, gdzie staje się stroną aktywną. Na poniższych zrzutach ekranu przedstawiono to:

![](hierarchical-images/secondpage.png "Strony są wypychane na stosie nawigacji")

Gdy [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) metoda jest wywoływana, zachodzą następujące zdarzenia:

- Wywoływanie strony `PushAsync` ma jego [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) zastąpienie wywołana.
- Na stronie do jego [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) zastąpienie wywołana.
- `PushAsync` Zadanie zostanie ukończone.

Jednak dokładne kolejność, w którym te zdarzenia występują to platforma zależnych. Aby uzyskać więcej informacji, zobacz [rozdziału 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms książki.

> [!NOTE]
> Wywołania [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) i [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) przesłonięcia nie może być traktowany jako gwarantowane oznaczenia nawigacji strony. Na przykład w systemie iOS `OnDisappearing` zastąpienie jest wywoływana na aktywnej stronie, gdy kończy działanie aplikacji.

### <a name="popping-pages-from-the-navigation-stack"></a>Zdejmowanie stron w stosie nawigacji

Aktywnej strony mogą zostać zdjęte ze stosu w stosie nawigacji, naciskając klawisz *ponownie* znajdujący się na urządzeniu, bez względu na to czy jest to przycisku fizycznego na urządzeniu lub przycisk na ekranie.

Aby programowo powrócić do oryginalnej strony `Page2Xaml` wystąpienie musi zostać wywołany [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) metody, jak pokazano w poniższym przykładzie kodu:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

Powoduje to, że `Page2Xaml` wystąpienia, aby były usuwane z stos nawigacji przy użyciu nowej strony najwyższego poziomu, staje się stroną aktywną. Gdy [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) metoda jest wywoływana, zachodzą następujące zdarzenia:

- Wywoływanie strony `PopAsync` ma jego [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) zastąpienie wywołana.
- Na stronie są zwracane do jego [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) zastąpienie wywołana.
- `PopAsync` Zadanie zwraca.

Jednak dokładne kolejność, w którym te zdarzenia występują to platforma zależnych. Aby uzyskać więcej informacji, zobacz [rozdziału 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms książki.

Także [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) i [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) metod [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) właściwości każdej strony są także [ `PopToRootAsync` ](xref:Xamarin.Forms.NavigationPage.PopToRootAsync) metody, która jest wyświetlana w poniższym przykładzie kodu:

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

Metoda ta pojawia się wszystkie z wyjątkiem głównego [ `Page` ](xref:Xamarin.Forms.Page) stosu nawigacji, co na stronie głównej aplikacji się stroną aktywną.

### <a name="animating-page-transitions"></a>Animowanie przejścia stron

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Właściwości każdej strony udostępnia również zastąpiona wypychania i pop metod, które obejmują `boolean` parametr, który kontroluje, czy ma być wyświetlana Animacja strony, podczas nawigacji, jak pokazano w poniższym kodzie przykład:

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

Ustawienie `boolean` parametr `false` wyłącza animacji przejście do strony, podczas ustawienie dla parametru `true` umożliwia przejście do strony animację, pod warunkiem, że nie jest obsługiwany przez możliwości platformy. Jednak metody wypychania i pop, które nie mają tego parametru włączyć animacji domyślnie.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Przekazywanie danych podczas nawigowania

Czasami jest konieczne dla strony przekazać dane do innej strony podczas nawigowania. Dwie techniki realizacji tego zadania przekazywania danych za pośrednictwem konstruktora strony i ustawiając nową stronę [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) do danych. Każda będzie teraz omawiane kolejno.

### <a name="passing-data-through-a-page-constructor"></a>Przekazywanie danych za pośrednictwem konstruktora strony

Najprostsza metoda przekazywania danych do innej strony podczas nawigacji jest za pośrednictwem strony parametr konstruktora, który jest pokazany w poniższym przykładzie kodu:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

Ten kod tworzy `MainPage` wystąpienia, przekazując bieżącą datę i godzinę w formacie ISO8601, który jest ujęte w [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) wystąpienia.

`MainPage` Wystąpienia odbiera dane za pomocą parametru konstruktora, jak pokazano w poniższym przykładzie kodu:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Dane są następnie wyświetlane na stronie przez ustawienie [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) właściwości, jak pokazano na poniższych zrzutach ekranu:

![](hierarchical-images/passing-data-constructor.png "Dane przekazane za pośrednictwem konstruktora strony")

### <a name="passing-data-through-a-bindingcontext"></a>Przekazywanie danych za pośrednictwem elementu BindingContext

Inną metodą przekazywania danych do innej strony podczas nawigacji jest ustawiając nową stronę [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) danych, jak pokazano w poniższym przykładzie kodu:

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

Ten kod ustawia [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) z `SecondPage` wystąpienia do `Contact` wystąpienia, a następnie przechodzi do `SecondPage`.

`SecondPage` Użyto powiązania danych, aby wyświetlić `Contact` wystąpienie danych, jak pokazano w poniższym przykładzie kodu XAML:

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

Poniższy przykład kodu pokazuje, jak można osiągnąć powiązanie danych w języku C#:

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

Dane są następnie wyświetlane na stronie przez szereg [ `Label` ](xref:Xamarin.Forms.Label) kontroluje, jak pokazano na poniższych zrzutach ekranu:

![](hierarchical-images/passing-data-bindingcontext.png "Dane przekazane za pośrednictwem elementu BindingContext")

Aby uzyskać więcej informacji na temat tworzenia powiązań danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>Manipulowanie stos nawigacji

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Ujawnia właściwość [ `NavigationStack` ](xref:Xamarin.Forms.INavigation.NavigationStack) właściwości, z którego można uzyskać stron w stosie nawigacji. Gdy Xamarin.Forms obsługuje dostęp do stos nawigacji `Navigation` dostarcza [ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*) i [ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*) metody do manipulowania stosu, wstawiając strony lub usuwania ich.

[ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*) Metoda wstawia określonej strony w stosie nawigacji przed istniejącej strony określonego, jak pokazano na poniższym diagramie:

![](hierarchical-images/insert-page-before.png "Wstawianie strony w stosie nawigacji")

[ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*) Metoda usuwa określonej strony ze stosu nawigacji, jak pokazano na poniższym diagramie:

![](hierarchical-images/remove-page.png "Usuwanie strony z stos nawigacji")

Te metody umożliwiają środowisko nawigacji niestandardowe, takie jak zastąpienie strony logowania przy użyciu nowej strony, po pomyślnym zalogowaniu. Poniższy przykład kodu pokazuje, w tym scenariuszu:

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

Pod warunkiem, że poświadczenia użytkownika są poprawne, `MainPage` wystąpienie znajduje się w stosie nawigacji przed bieżącą stronę. [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) Metoda następnie usuwa bieżącą stronę ze stosu nawigacji za pomocą `MainPage` wystąpienia staje się stroną aktywną.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) klasy w celu wykonania nawigacji w stosie stron. Ta klasa udostępnia środowisko Nawigacja hierarchiczna, gdzie użytkownik jest powinni poruszać się po stronach przodu i do tyłu, zgodnie z potrzebami. Klasa implementuje nawigacji jako ostatni na wejściu, first-out (LIFO) stos [ `Page` ](xref:Xamarin.Forms.Page) obiektów.


## <a name="related-links"></a>Linki pokrewne

- [Nawigacja strony](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Hierarchiczne (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [Jak utworzyć znak w przepływie ekranu w przykładzie Xamarin.Forms (Xamarin University wideo)](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Jak utworzyć znak w przepływie ekranu w interfejsie Xamarin.Forms (Xamarin University wideo)](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](xref:Xamarin.Forms.NavigationPage)
