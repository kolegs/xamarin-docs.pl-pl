---
title: Powiązanie z ControlTemplate platformy Xamarin.Forms
description: Powiązań szablonów Zezwalaj formantów w szablonie formantu danych powiązania właściwości publiczne, włączanie wartości właściwości formantów w kontroli szablonu do można łatwo zmienić. W tym artykule przedstawiono przy użyciu szablonu powiązań do przeprowadzenia powiązania danych z szablonu formantu.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 99d798ce2c74da0cf7fa0d497128db628a12ead5
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241584"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Powiązanie z ControlTemplate platformy Xamarin.Forms

_Powiązań szablonów Zezwalaj formantów w szablonie formantu danych powiązania właściwości publiczne, włączanie wartości właściwości formantów w kontroli szablonu do można łatwo zmienić. W tym artykule przedstawiono przy użyciu szablonu powiązań do przeprowadzenia powiązania danych z szablonu formantu._

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) jest używana do powiązania właściwości formantu w szablonie formantu do powiązania właściwości w nadrzędnym *docelowej* widoku, który jest właścicielem szablonu formantu. Na przykład zamiast tekstu wyświetlanego przez definiowanie [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpienia wewnątrz [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/), można użyć powiązanie szablonu powiązać [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) właściwości do powiązania właściwości, które definiują tekst, który ma być wyświetlany.

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) jest podobny do istniejącego [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/), ale *źródła* z `TemplateBinding` jest zawsze automatycznie ustawioną jako element nadrzędny *docelowej* widoku, który jest właścicielem szablonu formantu. Jednak należy pamiętać, że przy użyciu `TemplateBinding` poza [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) nie jest obsługiwane.

## <a name="creating-a-templatebinding-in-xaml"></a>Tworzenie na TemplateBinding w języku XAML

W języku XAML [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) jest tworzony przy użyciu [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) — rozszerzenie znaczników, jak pokazano w poniższym przykładzie:

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

Zamiast [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) właściwości na tekst statyczny, właściwości można użyć szablonu powiązania powiązać można powiązać właściwości elementu nadrzędnego *docelowej* widoku, który jest właścicielem [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). Pamiętaj jednak, że powiązań szablonów należy powiązać `Parent.HeaderText` i `Parent.FooterText`, a nie `HeaderText` i `FooterText`. Wynika to z faktu w tym przykładzie można powiązać właściwości są zdefiniowane w ponadnadrzędny z *docelowej* wyświetlić, a nie nadrzędnego, jak pokazano w poniższym przykładzie:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

*Źródła* szablonu wiązanie jest zawsze automatycznie ustawiana nadrzędny *docelowej* widoku, który jest właścicielem szablonu formantu, który jest tutaj [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) wystąpienie. Szablon wiązanie [ `Parent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Parent/) zwraca element nadrzędny dla właściwości `ContentView` wystąpienia, która jest [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpienia. W związku z tym przy użyciu [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) w [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) powiązać `Parent.HeaderText` i `Parent.FooterText` lokalizuje właściwości, które są zdefiniowane na `ContentPage`, jako pokazano w poniższym przykładzie:

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

Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

![](template-binding-images/teal-theme.png "Szablon formantu jest przy użyciu szablonu powiązań")

## <a name="creating-a-templatebinding-in-c35"></a>Tworzenie na TemplateBinding w języku C&#35;

W języku C# [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) jest tworzona przy użyciu `TemplateBinding` konstruktora, jak pokazano w poniższym przykładzie:

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

Zamiast [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) właściwości na tekst statyczny, właściwości można użyć szablonu powiązania powiązać można powiązać właściwości elementu nadrzędnego *docelowej* widoku, który jest właścicielem [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). Powiązanie szablonu jest tworzona przy użyciu [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) metody, określając [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) wystąpienia jako drugiego parametru. Należy pamiętać, że powiązać powiązań szablonów `Parent.HeaderText` i `Parent.FooterText`, ponieważ można powiązać właściwości są definiowane na ponadnadrzędny z *docelowej* wyświetlić, a nie nadrzędnego, jak pokazano w poniższym przykładzie:

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

Można powiązać właściwości są definiowane na `ContentPage`, jak przedstawiono wcześniej.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>Powiązanie BindableProperty z właściwością ViewModel

Jak wcześniej wspomniano, [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) wiąże właściwości formantu w szablonie formant powiązania właściwości w nadrzędnym *docelowej* widoku, który jest właścicielem szablonu formantu. Z kolei te właściwości może być powiązana z właściwości w ViewModels.

Poniższy przykład kodu definiuje dwie właściwości na ViewModel:

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

`HeaderText` i `FooterText` ViewModel właściwości może być powiązana do, jak pokazano w poniższym przykładzie kodu XAML:

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

`HeaderText` i `FooterText` właściwości są powiązane z `HomePageViewModel.HeaderText` i `HomePageViewModel.FooterText` właściwości z powodu ustawienia [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) na wystąpienie `HomePageViewModel` klasy. Ogólne, powoduje to właściwości formantu w [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) związany [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpień [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), który z kolei powiązania Właściwości ViewModel.

W poniższym przykładzie kodu pokazano równoważne kodu C#:

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

Aby uzyskać więcej informacji na temat ViewModels powiązania danych, zobacz [z powiązania danych z modelem MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono przy użyciu szablonu powiązań do przeprowadzenia powiązania danych z szablonu kontroli. Powiązań szablonów Zezwalaj formantów w szablonie formantu danych powiązania właściwości publiczne, włączanie wartości właściwości formantów w kontroli szablonu do można łatwo zmienić.



## <a name="related-links"></a>Linki pokrewne

- [Podstawowe informacje dotyczące powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Z danych powiązania z modelem MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Proste cechą powiązanie szablonu (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [Motyw proste powiązanie szablonu i ViewModel (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
