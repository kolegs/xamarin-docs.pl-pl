---
title: Powiązanie z zestawu narzędzi Xamarin.Forms ControlTemplate
description: Powiązania szablonów Zezwalaj na kontrolki w szablonie kontrolki z danymi należy powiązać właściwości publiczne Włączanie wartości właściwości kontrolek w szablonie formantu, aby można łatwo zmienić. W tym artykule przedstawiono korzystanie z powiązania szablonów w celu wykonania powiązanie danych z szablonu kontrolki.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 86de2ad6009365b3debbe1a2310651002b023219
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995570"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Powiązanie z zestawu narzędzi Xamarin.Forms ControlTemplate

_Powiązania szablonów Zezwalaj na kontrolki w szablonie kontrolki z danymi należy powiązać właściwości publiczne Włączanie wartości właściwości kontrolek w szablonie formantu, aby można łatwo zmienić. W tym artykule przedstawiono korzystanie z powiązania szablonów w celu wykonania powiązanie danych z szablonu kontrolki._

A [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) jest używana właściwość formantu w szablonie formantu można powiązać właściwości możliwej do wiązania na element nadrzędny *docelowej* widoku, który jest właścicielem szablonu kontrolki. Na przykład, zamiast definiować po tekstu wyświetlanego przez [ `Label` ](xref:Xamarin.Forms.Label) wystąpień wewnątrz [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate), można użyć powiązanie szablonu można powiązać [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) właściwości możliwej do wiązania właściwości, które definiują tekst, który ma być wyświetlany.

A [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) jest podobny do istniejącego [ `Binding` ](xref:Xamarin.Forms.Binding), chyba że *źródła* z `TemplateBinding` zawsze automatycznie zostaje ustawiony poziom elementu nadrzędnego *docelowej* widoku, który jest właścicielem szablonu kontrolki. Należy jednak zauważyć, że używanie `TemplateBinding` poza [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) nie jest obsługiwane.

## <a name="creating-a-templatebinding-in-xaml"></a>Tworzenie elementu TemplateBinding w XAML

W XAML [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) jest tworzony przy użyciu [ `TemplateBinding` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) — rozszerzenie znaczników, jak pokazano w poniższym przykładzie kodu:

```xaml
<ControlTemplate x:Key="TealTemplate">
  <Grid>
    ...
    <Label Text="{TemplateBinding Parent.HeaderText}" ... />
    ...
    <Label Text="{TemplateBinding Parent.FooterText}" ... />
  </Grid>
</ControlTemplate>
```

Zamiast [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) właściwości na tekst statyczny, właściwości można użyć powiązania szablonów można powiązać właściwości elementu nadrzędnego powiązać *docelowej* widoku, który jest właścicielem [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). Należy jednak zauważyć, że powiązania szablonów powiązać `Parent.HeaderText` i `Parent.FooterText`, zamiast `HeaderText` i `FooterText`. To dlatego, w tym przykładzie, które można powiązać właściwości wobec nadrzędnych z *docelowej* wyświetlić zamiast elementu nadrzędnego, jak pokazano w poniższym przykładzie kodu:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

*Źródła* szablonu wiązanie jest zawsze automatycznie ustawiany na element nadrzędny *docelowej* widoku, który jest właścicielem szablonu kontrolki, która jest tutaj [ `ContentView` ](xref:Xamarin.Forms.ContentView) wystąpienie. Szablon powiązaniu jest używany [ `Parent` ](xref:Xamarin.Forms.Element.Parent) właściwości, aby zwrócić element nadrzędny `ContentView` wystąpienia, co jest [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpienia. W związku z tym, za pomocą [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) w [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) można powiązać `Parent.HeaderText` i `Parent.FooterText` lokalizuje właściwości możliwej do wiązania, które są zdefiniowane na `ContentPage`, jako pokazano w poniższym przykładzie kodu:

```csharp
public static readonly BindableProperty HeaderTextProperty =
  BindableProperty.Create ("HeaderText", typeof(string), typeof(HomePage), "Control Template Demo App");
public static readonly BindableProperty FooterTextProperty =
  BindableProperty.Create ("FooterText", typeof(string), typeof(HomePage), "(c) Xamarin 2016");

public string HeaderText {
  get { return (string)GetValue (HeaderTextProperty); }
}

public string FooterText {
  get { return (string)GetValue (FooterTextProperty); }
}
```

Skutkuje to wygląd pokazano na poniższych zrzutach ekranu:

![](template-binding-images/teal-theme.png "Szablon kontrolki zielonomodrym przy użyciu powiązania szablonów")

## <a name="creating-a-templatebinding-in-c35"></a>Tworzenie elementu TemplateBinding w języku C&#35;

W języku C# [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) jest tworzona przy użyciu `TemplateBinding` konstruktora, jak pokazano w poniższym przykładzie kodu:

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var topLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    topLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.HeaderText"));
    ...
    var bottomLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    bottomLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.FooterText"));
    ...
  }
}
```

Zamiast [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) właściwości na tekst statyczny, właściwości można użyć powiązania szablonów można powiązać właściwości elementu nadrzędnego powiązać *docelowej* widoku, który jest właścicielem [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). Powiązanie szablonu jest tworzona przy użyciu [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) metody, określając [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) wystąpienia jako drugi parametr. Należy zauważyć, że powiązania szablonów powiązać `Parent.HeaderText` i `Parent.FooterText`, ponieważ nie zdefiniowano właściwości, które można powiązać na nadrzędnych z *docelowej* wyświetlić zamiast elementu nadrzędnego, jak pokazano w poniższym przykładzie kodu:

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    Content = new ContentView {
      ControlTemplate = tealTemplate,
      Content = new StackLayout {
        ...
      },
      ...
    };
    ...
  }
}
```

Właściwości możliwe do wiązania są zdefiniowane na `ContentPage`, co zostało opisane wcześniej.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>Powiązanie BindableProperty z właściwością ViewModel

Jak wcześniej wspomniano, [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) wiąże właściwości formantu w szablonie formantu właściwości możliwej do wiązania na element nadrzędny *docelowej* widoku, który jest właścicielem szablonu kontrolki. Z kolei te, które można powiązać właściwości może być powiązana z właściwościami w modele widoków.

Poniższy przykład kodu definiuje dwie właściwości na ViewModel:

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

`HeaderText` i `FooterText` ViewModel właściwości, które mogą być powiązane pozycji, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage xmlns:local="clr-namespace:SimpleTheme;assembly=SimpleTheme"
             HeaderText="{Binding HeaderText}" FooterText="{Binding FooterText}" ...>
    <ContentPage.BindingContext>
        <local:HomePageViewModel />
    </ContentPage.BindingContext>
    <ContentView ControlTemplate="{StaticResource TealTemplate}" ...>
    ...
    </ContentView>
</ContentPage>
```

`HeaderText` i `FooterText` właściwości może być powiązana, są powiązane z `HomePageViewModel.HeaderText` i `HomePageViewModel.FooterText` właściwości ze względu na ustawienie [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) do wystąpienia `HomePageViewModel` klasy. Ogólne, powoduje to właściwości formantu w [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) związania z [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wystąpień [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), który z kolei powiązać Właściwości ViewModel.

Równoważny kod C# pokazano w poniższym przykładzie kodu:

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    BindingContext = new HomePageViewModel ();
    this.SetBinding (HeaderTextProperty, "HeaderText");
    this.SetBinding (FooterTextProperty, "FooterText");
    ...
  }
}
```

Aby uzyskać więcej informacji na temat powiązania danych modele widoków zobacz [z powiązań danych do wzorca MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono korzystanie z powiązania szablonów w celu wykonania powiązanie danych z szablonu kontrolki. Powiązania szablonów Zezwalaj na kontrolki w szablonie kontrolki z danymi należy powiązać właściwości publiczne Włączanie wartości właściwości kontrolek w szablonie formantu, aby można łatwo zmienić.



## <a name="related-links"></a>Linki pokrewne

- [Podstawowe powiązanie danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Od powiązań danych do wzorca MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Proste cechą powiązanie szablonów (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [Motyw proste powiązanie szablonów i ViewModel (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](xref:Xamarin.Forms.TemplateBinding)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentView](xref:Xamarin.Forms.ContentView)
