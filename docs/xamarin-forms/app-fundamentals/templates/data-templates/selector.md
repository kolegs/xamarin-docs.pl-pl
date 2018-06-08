---
title: Tworzenie DataTemplateSelector
description: DataTemplateSelector może służyć do wyboru DataTemplate w czasie wykonywania na podstawie wartości właściwości powiązanych z danymi. Dzięki temu wiele DataTemplates ma zostać zastosowany do tego samego typu obiektu, aby dostosować wygląd określonego obiektów. W tym artykule przedstawiono sposób tworzenia i zużywać DataTemplateSelector.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: ad8cce32cb9cfe2ea5e78f11b1250440371a9851
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847195"
---
# <a name="creating-a-datatemplateselector"></a>Tworzenie DataTemplateSelector

_DataTemplateSelector może służyć do wyboru DataTemplate w czasie wykonywania na podstawie wartości właściwości powiązanych z danymi. Dzięki temu wiele DataTemplates ma zostać zastosowany do tego samego typu obiektu, aby dostosować wygląd określonego obiektów. W tym artykule przedstawiono sposób tworzenia i zużywać DataTemplateSelector._

Wybór szablonu danych umożliwia realizację scenariuszy, takich jak [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) tworzenia powiązania z kolekcją obiektów, których wyglądu każdego obiektu w `ListView` przez selektor szablonu danych zwracanie można wybrać w czasie wykonywania określonego [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/).

## <a name="creating-a-datatemplateselector"></a>Tworzenie DataTemplateSelector

Wybór szablonu danych jest implementowana przez tworzenie klasy, która dziedziczy [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). `OnSelectTemplate` — Metoda jest następnie zastąpiony, aby powrócić do określonego [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), jak pokazano w poniższym przykładzie:

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

`OnSelectTemplate` Metoda zwraca odpowiedni szablon na podstawie wartości z `DateOfBirth` właściwości. Szablon do zwrócenia jest wartość `ValidTemplate` właściwości lub `InvalidTemplate` właściwości, które są ustawiane podczas używania `PersonDataTemplateSelector`.

Wystąpienie klasy selektor szablonu danych następnie można przypisać do właściwości formantu platformy Xamarin.Forms takich jak [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/). Aby uzyskać listę prawidłowych, zobacz [tworzenie DataTemplate](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Ograniczenia

[`DataTemplateSelector`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) wystąpienia mają następujące ograniczenia:

- `DataTemplateSelector` Podklasy zawsze musi zwracać tego samego szablonu dla tych samych danych, jeśli kwerenda wiele razy.
- `DataTemplateSelector` Podklasy nie może zwracać innego `DataTemplateSelector` podklasy.
- `DataTemplateSelector` Podklasy nie może zwracać nowych wystąpień `DataTemplate` przy każdym wywołaniu. Zamiast tego samego wystąpienia musi zostać zwrócona. Niepowodzenie w tym celu utworzy przeciek pamięci i spowoduje wyłączenie wirtualizacji.
- W systemie Android, nie mogą istnieć nie więcej niż 20 szablonów innych danych na `ListView`.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>Korzystanie z DataTemplateSelector w języku XAML

W języku XAML `PersonDataTemplateSelector` mogą zostać utworzone przez zadeklarowanie go jako zasób, jak pokazano w poniższym przykładzie:

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

Ten poziom strony [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definiuje dwie [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) wystąpień i `PersonDataTemplateSelector` wystąpienia. `PersonDataTemplateSelector` Wystąpienie zestawy jego `ValidTemplate` i `InvalidTemplate` właściwości do odpowiedniego `DataTemplate` wystąpienia przy użyciu `StaticResource` — rozszerzenie znaczników. Należy pamiętać, że podczas zasoby są zdefiniowane na stronie [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), może być również zdefiniowane na poziomie aplikacji lub poziom kontroli.

`PersonDataTemplateSelector` Wystąpienia jest używany przez przypisanie go do [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) właściwości, jak pokazano w poniższym przykładzie:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

W czasie wykonywania [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wywołania `PersonDataTemplateSelector.OnSelectTemplate` metody dla poszczególnych elementów w źródłowej kolekcji, z wywołaniem przekazanie obiektu danych jako `item` parametru. [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Zwracany przez metodę jest następnie stosowany do tego obiektu.

Poniższe zrzuty ekranu pokazać wynik [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) stosowania `PersonDataTemplateSelector` do każdego obiektu w kolekcji źródłowej:

![](selector-images/data-template-selector.png "Element ListView z selektorem danych szablonu")

Wszelkie `Person` obiektu, który ma `DateOfBirth` właściwości wartość większą lub równą 1980 r. są wyświetlane w zielony, pozostałe obiekty wyświetlane kolorem czerwonym.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>Korzystanie z DataTemplateSelector w języku C&num;

W języku C# `PersonDataTemplateSelector` można utworzyć wystąpienia i przypisane do [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) właściwości, jak pokazano w poniższym przykładzie:

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

`PersonDataTemplateSelector` Wystąpienie zestawy jego `ValidTemplate` i `InvalidTemplate` właściwości do odpowiedniego [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) wystąpienia utworzone przez `SetupDataTemplates` metody. W czasie wykonywania [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wywołania `PersonDataTemplateSelector.OnSelectTemplate` metody dla poszczególnych elementów w źródłowej kolekcji, z wywołaniem przekazanie obiektu danych jako `item` parametru. `DataTemplate` Zwracany przez metodę jest następnie stosowany do tego obiektu.

## <a name="summary"></a>Podsumowanie

W tym artykule wykazała sposobu tworzenia i zużywać [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). A `DataTemplateSelector` można wybrać [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) w czasie wykonywania na podstawie wartości właściwości powiązanych z danymi. Dzięki temu wiele `DataTemplate` wystąpień, które ma zostać zastosowany do tego samego typu obiektu, aby dostosować wygląd określonego obiektów.


## <a name="related-links"></a>Linki pokrewne

- [Wybór szablonu danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)
