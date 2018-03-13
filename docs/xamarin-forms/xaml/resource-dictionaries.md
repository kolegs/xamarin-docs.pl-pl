---
title: "Słowniki zasobów"
description: "Zasoby XAML są definicje obiektów, które mogą być używane w więcej niż raz. ResourceDictionary umożliwia zasoby zdefiniowane w jednej lokalizacji i ponownego użycia w aplikacji platformy Xamarin.Forms. W tym artykule opisano tworzenie i stosowanie ResourceDictionary oraz sposobu scalania słownikach zasobów."
ms.topic: article
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/17/2017
ms.openlocfilehash: 9602e4d99e8f5c004fe75ab724bb3746aca46003
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="resource-dictionaries"></a>Słowniki zasobów

_Zasoby XAML są definicje obiektów, które mogą być używane w więcej niż raz. ResourceDictionary umożliwia zasoby zdefiniowane w jednej lokalizacji i ponownego użycia w aplikacji platformy Xamarin.Forms. W tym artykule opisano tworzenie i stosowanie ResourceDictionary oraz sposobu scalania słownikach zasobów._

## <a name="overview"></a>Omówienie

A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) to repozytorium dla zasobów, które są używane przez aplikację platformy Xamarin.Forms. Typowe zasoby, które są przechowywane w `ResourceDictionary` obejmują [style](~/xamarin-forms/user-interface/styles/index.md), [kontrolować szablony](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [szablony danych](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), kolory i konwerterów.

W języku XAML, zasoby są zdefiniowane w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) pobrane i zastosowane do elementów przy użyciu `StaticResource` — rozszerzenie znaczników. W języku C#, zasoby są zdefiniowane w `ResourceDictionary` pobrane i zastosowane do elementów przy użyciu indeksatora oparte na ciągach. Istnieje jednak małego zaletą używania `ResourceDictionary` w języku C#, jako zasoby można łatwo bezpośrednio przypisać do właściwości elementów wizualnych, bez konieczności najpierw pobrać je z `ResourceDictionary`.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Tworzenie i korzystanie z ResourceDictionary

Zasoby można zdefiniować w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) dołączona do [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekcji strona lub kontrolka lub w celu [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) kolekcji aplikacji. Wybieranie miejsce definiowania [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) wpływu, których można użyć:

- Zasoby w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) zdefiniowane podczas kontroli poziomu mogą być stosowane tylko do formantu i jego elementów podrzędnych.
- Zasoby w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) zdefiniowane na stronie poziomu mogą być stosowane tylko do strony i jego elementów podrzędnych.
- Zasoby w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) zdefiniowanych w aplikacji poziomu mogą być stosowane w całej aplikacji.

Poniższy przykładowy kod XAML zawiera zasoby zdefiniowane w poziomie aplikacji [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Application ...>
    <Application.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Yellow</Color>
            <Color x:Key="HeadingTextColor">Black</Color>
            <Color x:Key="NormalTextColor">Blue</Color>
            <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
                <Setter Property="FontAttributes" Value="Bold" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

To [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definiuje trzy [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) zasobów i [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) zasobów. Aby uzyskać więcej informacji o tworzeniu XAML `App` , zobacz [klasy aplikacji](~/xamarin-forms/app-fundamentals/application-class.md).

Wszystkie zasoby, ma klucz, który jest określany przy użyciu `x:Key` atrybut, który zapewnia jej opisową klucza w `ResourceDictionary`. Klucz służy do pobierania zasobu z [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) przez `StaticResource` — rozszerzenie znaczników, jak pokazano w poniższym przykładzie kodu XAML zawiera dodatkowe zasoby zdefiniowane w formancie poziomu `ResourceDictionary`:

```xaml
<StackLayout Margin="0,20,0,0">
  <StackLayout.Resources>
    <ResourceDictionary>
      <Style x:Key="LabelNormalStyle" TargetType="Label">
        <Setter Property="TextColor" Value="{StaticResource NormalTextColor}" />
      </Style>
      <Style x:Key="MediumBoldText" TargetType="Button">
        <Setter Property="FontSize" Value="Medium" />
        <Setter Property="FontAttributes" Value="Bold" />
      </Style>
    </ResourceDictionary>
  </StackLayout.Resources>
  <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
    <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
           Margin="10,20,10,0"
           Style="{StaticResource LabelNormalStyle}" />
    <Button Text="Navigate"
            Clicked="OnNavigateButtonClicked"
            TextColor="{StaticResource NormalTextColor}"
            Margin="0,20,0,0"
            HorizontalOptions="Center"
            Style="{StaticResource MediumBoldText}" />
</StackLayout>
```

Pierwszy [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpienia pobiera i wykorzystuje `LabelPageHeadingStyle` zasobów zdefiniowane na poziomie aplikacji [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), drugi `Label` wystąpienia Pobieranie i przetwarzanie `LabelNormalStyle` zasobów zdefiniowane na poziomie formantu `ResourceDictionary`. Podobnie [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wystąpienia pobiera i wykorzystuje `NormalTextColor` zasobów zdefiniowane na poziomie aplikacji `ResourceDictionary`i `MediumBoldText` zasobów zdefiniowane na poziomie formantu `ResourceDictionary`. Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

[![](resource-dictionaries-images/screenshots-sml.png "Korzystanie z zasobów ResourceDictionary")](resource-dictionaries-images/screenshots.png#lightbox "korzysta z zasobów ResourceDictionary")

> [!NOTE]
> Zasoby, które są specyficzne dla pojedynczej strony nie powinny znajdować się w aplikacji zasobów na poziomie słownika, jako takie zasoby następnie będzie analizowany przy uruchamianiu aplikacji zamiast, gdy jest to wymagane przez stronę. Aby uzyskać więcej informacji, zobacz [zmniejszyć rozmiar słownika zasobów aplikacji](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Zastępowanie zasobów

Gdy [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) udostępnianie zasobów `x:Key` wartości atrybutów zasobów niższych zdefiniowany w hierarchii widoku mają wyższy priorytet niż określone wyższy się. Na przykład ustawienie `PageBackgroundColor` zasobów do `Blue` w aplikacji poziom zostanie ona zastąpiona na poziomie strony `PageBackgroundColor` ustawioną zasobów `Yellow`. Podobnie, poziomu strony `PageBackgroundColor` zasobu zostanie przesłonięta przez poziom kontroli `PageBackgroundColor` zasobów. Pierwszeństwo tego przedstawiono na poniższym przykładzie kodu XAML:

```xaml
<ContentPage ... BackgroundColor="{StaticResource PageBackgroundColor}">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Blue</Color>
            <Color x:Key="NormalTextColor">Yellow</Color>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="0,20,0,0">
        ...
        <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
               Margin="10,20,10,0"
               Style="{StaticResource LabelNormalStyle}" />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked"
                TextColor="{StaticResource NormalTextColor}"
                Margin="0,20,0,0"
                HorizontalOptions="Center"
                Style="{StaticResource MediumBoldText}" />
    </StackLayout>
</ContentPage>
```

Oryginalna `PageBackgroundColor` i `NormalTextColor` wystąpień zdefiniowane na poziomie aplikacji zostały zastąpione przez `PageBackgroundColor` i `NormalTextColor` wystąpień zdefiniowane na poziomie strony. W związku z tym kolor tła strony staje się niebieska i tekstu na stronie stanie się żółty, jak pokazano na poniższych zrzutach ekranu:

[![](resource-dictionaries-images/overridding-screenshots-sml.png "Zastępowanie zasobów ResourceDictionary")](resource-dictionaries-images/overridding-screenshots.png#lightbox "zastępowanie ResourceDictionary zasobów")

Jednak należy pamiętać, że na pasku tła [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) jest nadal żółty, ponieważ [ `BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/) właściwości ustawiono wartość `PageBackgroundColor` zasobów zdefiniowanych w aplikacji poziom [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).

## <a name="merged-resource-dictionaries"></a>Połączone słowniki zasobów

Łączenie scalonych słownikach zasobów co najmniej jeden [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) wystąpień do innego. Jest to osiągane przez ustawienie `ResourceDictionary.MergedDictionaries` właściwości słowniki zasobów, które zostaną scalone w aplikacji, strony lub poziom kontroli `ResourceDictionary`.

> [!IMPORTANT]
> [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Ma także typ [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) właściwość, która może służyć do scalenia pojedynczy `ResourceDictionary` do innego, z `ResourceDictionary` określony jako wartość `MergedWith`właściwość scalane w bieżącej `ResourceDictionary` wystąpienia. Podczas scalania za pośrednictwem `MergedWith` właściwości, wszystkie zasoby w bieżącym `ResourceDictionary` tego udziału `x:Key` wartości z zasobami w atrybutów `ResourceDictionary` do scalenia, zostaną zastąpione. Jednak `MergedWith` właściwości zostaną wycofane w przyszłej wersji platformy Xamarin.Forms. Dlatego zaleca się użyć `MergedDictionaries` właściwości do scalenia `ResourceDictionary` wystąpień.

Przedstawia poniższy przykładowy kod XAML [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) o nazwie `MyResourceDictionary` zawierający pojedynczego zasobu:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ResourceDictionaryDemo.MyResourceDictionary">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}" TextColor="{StaticResource NormalTextColor}" FontAttributes="Bold" />
                <Label Grid.Column="1" Text="{Binding Age}" TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2" Text="{Binding Location}" TextColor="{StaticResource NormalTextColor}" HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

`MyResourceDictionary` można scalić w dowolnej aplikacji, strony lub poziom kontroli `ResourceDictionary`. Poniższy przykładowy kod XAML zawiera on scalane w poziomie strony `ResourceDictionary` przy użyciu `MergedDictionaries` właściwości:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

W przypadku scalania [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) zasoby współużytkować identyczne `x:Key` wartości atrybutów, platformy Xamarin.Forms używa w następującej kolejności zasobów:

1. Zasoby lokalne do słownika zasobów.
1. Zasoby zawartych w słowniku zasobów, który został scalony za pośrednictwem [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) właściwości.
1. Zasoby zawarte w słownikach zasobów, które zostały scalone za pośrednictwem `MergedDictionaries` kolekcji w kolejności, w jakiej występują w `MergedDictionaries` właściwości.

> [!NOTE]
> Wyszukiwanie słowniki zasobów może być praktyce znacznym zadania, jeśli aplikacja zawiera wiele słowniki dużych zasobów. W związku z tym upewnić się, że każdej strony w aplikacji tylko używa słowniki zasobów, które są odpowiednie do strony, aby uniknąć niepotrzebnych wyszukiwania.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak utworzyć i korzystać z [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)oraz sposób scalania słownikach zasobów. A `ResourceDictionary` umożliwia zasoby zdefiniowane w jednej lokalizacji i ponownego użycia w aplikacji platformy Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Słowniki zasobów (na przykład)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Style](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
