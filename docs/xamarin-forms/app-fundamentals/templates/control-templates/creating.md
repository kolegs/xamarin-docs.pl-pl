---
title: Tworzenie ControlTemplate
description: "Szablony formantu można zdefiniować na poziomie aplikacji lub strony. W tym artykule przedstawiono sposób tworzenia i zużywać szablony formantu."
ms.topic: article
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 53309be2712f14c79b84c2eabb519b86dd73a404
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="creating-a-controltemplate"></a>Tworzenie ControlTemplate

_Szablony formantu można zdefiniować na poziomie aplikacji lub strony. W tym artykule przedstawiono sposób tworzenia i zużywać szablony formantu._

## <a name="creating-a-controltemplate-in-xaml"></a>Tworzenie ControlTemplate w języku XAML

Aby zdefiniować [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) na poziomie aplikacji [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) musi zostać dodany do `App` klasy. Domyślnie wszystkie aplikacje platformy Xamarin.Forms utworzonych na podstawie szablonu używają **aplikacji** klasy do zaimplementowania [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) podklasy. Aby zadeklarować `ControlTemplate` na poziomie aplikacji w aplikacji `ResourceDictionary` przy użyciu kodu XAML, domyślnie **aplikacji** klasy muszą zostać zastąpione XAML **aplikacji** klasy i skojarzone kodem, jako przedstawiono w poniższym przykładzie:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.App">
    <Application.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                <Grid>
                    ...
                    <BoxView ... />
                    <Label Text="Control Template Demo App"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                    <ContentPresenter ... />
                    <BoxView Color="Teal" ... />
                    <Label Text="(c) Xamarin 2016"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                </Grid>
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Każdy [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) jest tworzone wystąpienie jako obiekt wielokrotnego użytku w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).  Jest to osiągane przez nadanie każdej deklaracji unikatowego `x:Key` atrybut, który zapewnia opisową klucza w `ResourceDictionary`.

Poniższy przykład kodu pokazuje skojarzony `App` związane z kodem:

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent ();
    MainPage = new HomePage ();
  }
}
```

Oraz ustawienie [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) właściwość CodeBehind musi także wywołać metodę `InitializeComponent` metodę, aby załadować i przeanalizować skojarzone XAML.

Poniższy kod przedstawia przykład [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) stosowania `TealTemplate` do [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0"
                 ControlTemplate="{StaticResource TealTemplate}">
        <StackLayout VerticalOptions="CenterAndExpand">
            <Label Text="Welcome to the app!" HorizontalOptions="Center" />
            <Button Text="Change Theme" Clicked="OnButtonClicked" />
        </StackLayout>
    </ContentView>
</ContentPage>
```

`TealTemplate` Jest przypisany do [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) właściwości przy użyciu `StaticResource` — rozszerzenie znaczników. [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Właściwość jest ustawiona na [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) definiuje zawartości, który będzie wyświetlany na [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Ta zawartość będzie wyświetlany przez [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) zawartych w `TealTemplate`. Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

![](creating-images/teal-theme.png "Szablon jest formantu")

### <a name="re-theming-an-application-at-runtime"></a>Ponowna tworzenia motywów aplikacji w czasie wykonywania

Kliknięcie przycisku **zmiany motywu** wykonuje przycisk `OnButtonClicked` metodę, która jest wyświetlana w poniższym przykładzie:

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Ta metoda zastępuje aktywne [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) wystąpienia o alternatywnej `ControlTemplate` wystąpienia, co powoduje Poniższy zrzut ekranu:

![](creating-images/aqua-theme.png "Szablon Akwamaryna formantu")

> [!NOTE]
> Na `ContentPage`, `Content` można przypisać właściwości i `ControlTemplate` można również ustawić właściwość. Jeśli ten problem wystąpi, jeśli `ControlTemplate` zawiera `ContentPresenter` wystąpienia, zawartość przypisana do `Content` właściwość zostanie przedstawiony przez `ContentPresenter` w `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>Ustawienie ControlTemplate przy użyciu stylu

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) również będą stosowane za pośrednictwem [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) dalszy rozwój możliwości motywu. Można to osiągnąć, tworząc *niejawne* lub *jawne* styl widok elementu docelowego w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)i ustawienie `ControlTemplate` właściwości elementu docelowego Wyświetl w [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienia. Poniższy kod przedstawia przykład *niejawne* styl dodawanej do poziomu aplikacji [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Ponieważ [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wystąpienie jest *niejawne*, zostaną zastosowane do wszystkich `ContentView` wystąpień w aplikacji. W związku z tym nie jest już konieczne ustawić [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) właściwości, jak pokazano w poniższym przykładzie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Aby uzyskać więcej informacji na temat stylów, zobacz [style](~/xamarin-forms/user-interface/styles/index.md).

### <a name="creating-a-controltemplate-at-page-level"></a>Tworzenie ControlTemplate na poziomie strony

Oprócz tworzenia [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) wystąpienia na poziomie aplikacji, ich można także utworzyć na poziomie strony, jak pokazano w poniższym przykładzie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                ...
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
        ...
    </ContentView>
</ContentPage>
```

Podczas dodawania [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) na poziomie strony [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) jest dodawany do [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), a następnie `ControlTemplate` dostępnych wystąpień w `ResourceDictionary`.

## <a name="creating-a-controltemplate-in-c35"></a>Tworzenie ControlTemplate w języku C&#35;

Aby zdefiniować [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) na poziomie aplikacji `class` musi zostać utworzony reprezentująca `ControlTemplate`. Klasa powinien pochodzić od [układu](~/xamarin-forms/user-interface/layouts/index.md) używany dla szablonu, jak pokazano w poniższym przykładzie:

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var contentPresenter = new ContentPresenter ();
    Children.Add (contentPresenter, 0, 1);
    Grid.SetColumnSpan (contentPresenter, 2);
    ...
  }
}

class AquaTemplate : Grid
{
  ...
}
```

`AquaTemplate` Jest taka sama jak klasa `TealTemplate` klasy, z wyjątkiem tego, że różne kolory dla [ `BoxView.Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) i [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) właściwości.

Poniższy kod przedstawia przykład [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) stosowania `TealTemplate` do [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
public class HomePageCS : ContentPage
{
  ...
  ControlTemplate tealTemplate = new ControlTemplate (typeof(TealTemplate));
  ControlTemplate aquaTemplate = new ControlTemplate (typeof(AquaTemplate));

  public HomePageCS ()
  {
    var button = new Button { Text = "Change Theme" };
    var contentView = new ContentView {
      Padding = new Thickness (0, 20, 0, 0),
      Content = new StackLayout {
        VerticalOptions = LayoutOptions.CenterAndExpand,
        Children = {
          new Label { Text = "Welcome to the app!", HorizontalOptions = LayoutOptions.Center },
          button
        }
      },
      ControlTemplate = tealTemplate
    };
    ...
    Content = contentView;
  }
}
```

[ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) Wystąpienia są tworzone przez określenie typu klasy, które definiują szablony formantu w `ControlTemplate` konstruktora.

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Właściwość jest ustawiona na [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) definiuje zawartości, który będzie wyświetlany na [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Ta zawartość będzie wyświetlany przez [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) zawartych w `TealTemplate`. Ten sam mechanizm opisane wcześniej umożliwia Zmień motyw w czasie wykonania `AquaTheme`.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób tworzenia i korzystać z szablonów kontrolki. Szablony formantu można zdefiniować na poziomie aplikacji lub strony.


## <a name="related-links"></a>Linki pokrewne

- [Style](~/xamarin-forms/user-interface/styles/index.md)
- [Motyw proste (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
