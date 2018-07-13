---
title: Tworzenie ControlTemplate
description: Szablony kontrolek można definiować na poziomie aplikacji lub strony. W tym artykule przedstawiono sposób tworzenia i wykorzystywania szablonów kontrolek.
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: b83668f6836b1d5d98f67592bf3e2b01e7319edc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998193"
---
# <a name="creating-a-controltemplate"></a>Tworzenie ControlTemplate

_Szablony kontrolek można definiować na poziomie aplikacji lub strony. W tym artykule przedstawiono sposób tworzenia i wykorzystywania szablonów kontrolek._

## <a name="creating-a-controltemplate-in-xaml"></a>Tworzenie ControlTemplate w XAML

Aby zdefiniować [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) na poziomie aplikacji [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) muszą zostać dodane do `App` klasy. Domyślnie wszystkie aplikacje Xamarin.Forms tworzone na podstawie szablonu używają **aplikacji** klasy do zaimplementowania [ `Application` ](xref:Xamarin.Forms.Application) podklasę. Aby zadeklarować `ControlTemplate` na poziomie aplikacji w aplikacji w witrynie `ResourceDictionary` przy użyciu XAML, wartość domyślna **aplikacji** klasy muszą zostać zastąpione XAML **aplikacji** klasy i skojarzone związanym z kodem, jako pokazano w poniższym przykładzie kodu:

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

Każdy [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) tworzone jest wystąpienie jako obiekt wielokrotnego użytku w [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary).  To osiągnąć, podając unikatową każdego zgłoszenia `x:Key` atrybut, który dostarcza mu kluczem opisowe w `ResourceDictionary`.

Poniższy przykład kodu pokazuje skojarzonego `App` związanym z kodem:

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

A także ustawienie [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) właściwości związane z kodem również wywołać `InitializeComponent` metodę, aby załadować i przeanalizować skojarzone XAML.

Poniższy kod przedstawia przykład [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) stosowanie `TealTemplate` do [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

`TealTemplate` Jest przypisany do [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) właściwości przy użyciu `StaticResource` — rozszerzenie znaczników. [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) Właściwość jest ustawiona na [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) definiujący zawartości, który będzie wyświetlany w [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Ta zawartość zostanie wyświetlony przez [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) zawarte w `TealTemplate`. Skutkuje to wygląd pokazano na poniższych zrzutach ekranu:

![](creating-images/teal-theme.png "Szablon kontrolki turkusowy")

### <a name="re-theming-an-application-at-runtime"></a>Składanie motywów aplikacji w czasie wykonywania

Klikając **Zmień motyw** wykonuje przycisk `OnButtonClicked` metody, która jest wyświetlana w poniższym przykładzie kodu:

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Ta metoda zastępuje aktywny [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) wystąpienie z alternatywnym `ControlTemplate` wystąpienia, co na następującym zrzucie ekranu:

![](creating-images/aqua-theme.png "Szablon kontrolki Akwamaryna")

> [!NOTE]
> Na `ContentPage`, `Content` można przypisać właściwości i `ControlTemplate` można również ustawić właściwości. Jeśli ten problem wystąpi, jeśli `ControlTemplate` zawiera `ContentPresenter` wystąpienia, zawartość przypisana do `Content` właściwości będą przedstawiane przez `ContentPresenter` w ramach `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>Ustawienie ControlTemplate przy użyciu stylu

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) mogą być stosowane również za pośrednictwem [ `Style` ](xref:Xamarin.Forms.Style) można rozwinąć możliwości motywu. Można to osiągnąć, tworząc *niejawne* lub *jawne* styl widok elementu docelowego w [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)i ustawienie `ControlTemplate` właściwości elementu docelowego Wyświetl w [ `Style` ](xref:Xamarin.Forms.Style) wystąpienia. Poniższy kod przedstawia przykład *niejawne* styl, który jest dodano na poziomie aplikacji [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Ponieważ [ `Style` ](xref:Xamarin.Forms.Style) wystąpienie jest *niejawne*, zostaną zastosowane do wszystkich `ContentView` wystąpień w aplikacji. W związku z tym, nie jest już konieczne ustawić [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) właściwości, jak pokazano w poniższym przykładzie kodu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Aby uzyskać więcej informacji na temat style zobacz [style](~/xamarin-forms/user-interface/styles/index.md).

### <a name="creating-a-controltemplate-at-page-level"></a>Tworzenie ControlTemplate na poziomie strony

Oprócz tworzenia [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) wystąpień na poziomie aplikacji, ich można również utworzyć na poziomie strony, jak pokazano w poniższym przykładzie kodu:

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

Podczas dodawania [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) na poziomie strony [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) jest dodawany do [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), a następnie `ControlTemplate` wystąpienia są uwzględnione w `ResourceDictionary`.

## <a name="creating-a-controltemplate-in-c35"></a>Tworzenie ControlTemplate w języku C&#35;

Aby zdefiniować [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) na poziomie aplikacji `class` musi zostać utworzona, który reprezentuje `ControlTemplate`. Klasa powinien pochodzić od [układ](~/xamarin-forms/user-interface/layouts/index.md) używane dla szablonu, jak pokazano w poniższym przykładzie kodu:

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

`AquaTemplate` Klasy jest taka sama jak `TealTemplate` klasy, z tą różnicą, że są używane różne kolory dla [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) i [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) właściwości.

Poniższy kod przedstawia przykład [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) stosowanie `TealTemplate` do [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

[ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) Wystąpienia są tworzone przez określenie typu klasy, które definiują szablonów kontrolek, w `ControlTemplate` konstruktora.

[ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) Właściwość jest ustawiona na [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) definiujący zawartości, który będzie wyświetlany w [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Ta zawartość zostanie wyświetlony przez [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) zawarte w `TealTemplate`. Ten sam mechanizm opisane wcześniej umożliwia zmienianie motywu w czasie wykonywania, aby `AquaTheme`.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób tworzenia i wykorzystywania szablonów kontrolek. Szablony kontrolek można definiować na poziomie aplikacji lub strony.


## <a name="related-links"></a>Linki pokrewne

- [Style](~/xamarin-forms/user-interface/styles/index.md)
- [Motyw proste (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
- [ContentView](xref:Xamarin.Forms.ContentView)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
