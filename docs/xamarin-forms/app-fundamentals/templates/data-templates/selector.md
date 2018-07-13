---
title: Tworzenie DataTemplateSelector zestawu narzędzi Xamarin.Forms
description: W tym artykule przedstawiono sposób tworzenia i wykorzystywania DataTemplateSelector, która może służyć do wyboru DataTemplate w czasie wykonywania na podstawie wartości właściwości powiązanych z danymi.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: a72777c7e51e96a8e123ecd85ad0aa24fc60fc6c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994520"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>Tworzenie DataTemplateSelector zestawu narzędzi Xamarin.Forms

_DataTemplateSelector może służyć do wybierz DataTemplate w czasie wykonywania na podstawie wartości właściwości powiązanych z danymi. Dzięki temu wiele DataTemplates mają być stosowane do tego samego typu obiektu, aby dostosować wygląd obiektów określonego. W tym artykule przedstawiono sposób tworzenia i wykorzystywania DataTemplateSelector._

Wybór szablonu danych umożliwia realizację scenariuszy, takich jak [ `ListView` ](xref:Xamarin.Forms.ListView) tworzenia powiązania z kolekcją obiektów, których wygląd każdego obiektu w `ListView` można wybrać w czasie wykonywania przez wybór szablonu danych przekazujących określonego [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="creating-a-datatemplateselector"></a>Tworzenie DataTemplateSelector

Wybór szablonu danych jest implementowany przez utworzenie klasy, która dziedziczy [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector). `OnSelectTemplate` Metoda następnie zostanie przesłonięta do zwrócenia określonego [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), jak pokazano w poniższym przykładzie kodu:

```csharp
public class PersonDataTemplateSelector : DataTemplateSelector
{
  public DataTemplate ValidTemplate { get; set; }
  public DataTemplate InvalidTemplate { get; set; }

  protected override DataTemplate OnSelectTemplate (object item, BindableObject container)
  {
    return ((Person)item).DateOfBirth.Year >= 1980 ? ValidTemplate : InvalidTemplate;
  }
}
```

`OnSelectTemplate` Metoda zwraca odpowiedni szablon na podstawie wartości z `DateOfBirth` właściwości. Szablon do zwrócenia jest wartością `ValidTemplate` właściwości lub `InvalidTemplate` właściwości są ustawiane podczas używania `PersonDataTemplateSelector`.

Wystąpienie klasy selektor szablonu danych można przypisać do właściwości kontrolki zestawu narzędzi Xamarin.Forms takich jak [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1). Aby uzyskać listę prawidłowe właściwości, zobacz [tworzenia DataTemplate](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Ograniczenia

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) wystąpienia mają następujące ograniczenia:

- `DataTemplateSelector` Podklasy muszą zawsze zwracać tego samego szablonu dla tych samych danych, jeśli kwerenda wiele razy.
- `DataTemplateSelector` Podklasy nie może zwracać innego `DataTemplateSelector` podklasę.
- `DataTemplateSelector` Podklasy nie może zwracać nowych wystąpień `DataTemplate` przy każdym wywołaniu. Zamiast tego należy ponownie wprowadzić to samo wystąpienie. Niewykonanie tej czynności spowoduje utworzenie przeciek pamięci, a także spowoduje wyłączenie wirtualizacji.
- W systemie Android, może istnieć nie więcej niż 20 szablonów różnych danych dla jednego `ListView`.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>Korzystanie z DataTemplateSelector w XAML

W XAML `PersonDataTemplateSelector` można tworzyć wystąpienia deklarując ją jako zasób, jak pokazano w poniższym przykładzie kodu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Selector;assembly=Selector" x:Class="Selector.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="validPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <DataTemplate x:Key="invalidPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <local:PersonDataTemplateSelector x:Key="personDataTemplateSelector"
                ValidTemplate="{StaticResource validPersonTemplate}"
                InvalidTemplate="{StaticResource invalidPersonTemplate}" />
        </ResourceDictionary>
    </ContentPage.Resources>
  ...
</ContentPage>
```

Ten poziom strony [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) definiuje dwa [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) wystąpień i `PersonDataTemplateSelector` wystąpienia. `PersonDataTemplateSelector` Wystąpienia ustawia jego `ValidTemplate` i `InvalidTemplate` właściwości do odpowiednich `DataTemplate` wystąpień przy użyciu `StaticResource` — rozszerzenie znaczników. Należy zauważyć, że podczas zasoby są zdefiniowane na stronie [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), również można zdefiniować na poziomie aplikacji lub poziom kontroli.

`PersonDataTemplateSelector` Wystąpienie jest używane, przypisując go do [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) właściwości, jak pokazano w poniższym przykładzie kodu:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

W czasie wykonywania [ `ListView` ](xref:Xamarin.Forms.ListView) wywołania `PersonDataTemplateSelector.OnSelectTemplate` metodę dla każdego z elementów w kolekcji źródłowej, za pomocą wywołania przekazanie obiektu danych jako `item` parametru. [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Zwracanym przez metodę jest następnie stosowane do tego obiektu.

Poniższych zrzutach ekranu przedstawiono wynik [ `ListView` ](xref:Xamarin.Forms.ListView) stosowanie `PersonDataTemplateSelector` do każdego obiektu w kolekcji źródłowej:

![](selector-images/data-template-selector.png "ListView przy użyciu wybór szablonu danych")

Wszelkie `Person` obiekt, który ma `DateOfBirth` wartość właściwości większa lub równa 1980 jest wyświetlany w kolorze zielonym, za pomocą pozostałe obiekty, które są wyświetlane w kolorze czerwonym.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>Korzystanie z DataTemplateSelector w języku C&num;

W języku C# `PersonDataTemplateSelector` utworzone i przypisane do [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) właściwości, jak pokazano w poniższym przykładzie kodu:

```csharp
public class HomePageCS : ContentPage
{
  DataTemplate validTemplate;
  DataTemplate invalidTemplate;

  public HomePageCS ()
  {
    ...
    SetupDataTemplates ();
    var listView = new ListView {
      ItemsSource = people,
      ItemTemplate = new PersonDataTemplateSelector {
        ValidTemplate = validTemplate,
        InvalidTemplate = invalidTemplate }
    };

    Content = new StackLayout {
      Margin = new Thickness (20),
      Children = {
        ...
        listView
      }
    };
  }
  ...  
}
```

`PersonDataTemplateSelector` Wystąpienia ustawia jego `ValidTemplate` i `InvalidTemplate` właściwości do odpowiednich [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) wystąpienia utworzone przez `SetupDataTemplates` metody. W czasie wykonywania [ `ListView` ](xref:Xamarin.Forms.ListView) wywołania `PersonDataTemplateSelector.OnSelectTemplate` metodę dla każdego z elementów w kolekcji źródłowej, za pomocą wywołania przekazanie obiektu danych jako `item` parametru. `DataTemplate` Zwracanym przez metodę jest następnie stosowane do tego obiektu.

## <a name="summary"></a>Podsumowanie

Ten artykuł ma pokazano sposób tworzenia i wykorzystywania [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector). A `DataTemplateSelector` może służyć do wybierz [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) w czasie wykonywania na podstawie wartości właściwości powiązanych z danymi. Dzięki temu wiele `DataTemplate` wystąpień, które mają być stosowane do tego samego typu obiektu, aby dostosować wygląd obiektów określonego.


## <a name="related-links"></a>Linki pokrewne

- [Wybór szablonu danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
