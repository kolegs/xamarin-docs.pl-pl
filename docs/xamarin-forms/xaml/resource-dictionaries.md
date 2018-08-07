---
title: Słowniki zasobów
description: Zasoby XAML są obiekty, które mogą być udostępniane i ponownie używane w całej aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: d305de651f6e770d02ca35f5f8f8ffcf08424e28
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573649"
---
# <a name="resource-dictionaries"></a>Słowniki zasobów

_Zasoby XAML są definicje obiektów, które mogą być udostępniane i ponownie używane w całej aplikacji platformy Xamarin.Forms._

Te obiekty zasobów są przechowywane w słowniku zasobów. W tym artykule opisano sposób tworzenia i wykorzystywania słownika zasobów, a także sposób scalania słowniki zasobów.

## <a name="overview"></a>Omówienie

A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) to repozytorium dla zasobów, które są używane przez aplikację platformy Xamarin.Forms. Typowe zasoby, które są przechowywane w `ResourceDictionary` obejmują [style](~/xamarin-forms/user-interface/styles/index.md), [kontrolować szablony](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [szablony danych](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), kolory i konwertery.

W XAML, zasoby, które są przechowywane w `ResourceDictionary` następnie można je pobrać i stosowana do elementów przy użyciu `StaticResource` — rozszerzenie znaczników. W języku C#, zasoby mogą być także definiowane w `ResourceDictionary` pobrane i zastosowane do elementów za pomocą indeksatora usługi opartej na ciągach. Istnieje jednak niewielkie zaletą używania `ResourceDictionary` w języku C# jako obiekty udostępnione można po prostu być przechowywane w postaci pól lub właściwości i dostępne bezpośrednio bez konieczności uprzedniego pobierać je ze słownika.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Tworzenie i korzystanie z ResourceDictionary

Zasoby są zdefiniowane w [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) oznacza to następnie ustawiony na jedną z następujących `Resources` właściwości:

- [ `Resources` ](xref:Xamarin.Forms.Application.Resources) Właściwości każdej klasy, która pochodzi od klasy [`Application`](xref:Xamarin.Forms.Application)
- [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Właściwości każdej klasy, która pochodzi od klasy [`VisualElement`](xref:Xamarin.Forms.Application)

Xamarin.Forms program zawiera tylko jedną klasę, która pochodzi od klasy `Application` , ale często korzysta z wielu klas, które wynikają z `VisualElement`, w tym stron, układy i kontrolek. Dowolnego z tych obiektów może mieć jej `Resources` właściwością `ResourceDictionary`. Wybieranie miejsce umieszczenia określonego `ResourceDictionary` na środowisko, w którym zasoby mogą być używane:

- Zasoby w `ResourceDictionary` dołączona do widoku, takie jak `Button` lub `Label` można stosować tylko do danego obiektu, dzięki czemu nie jest to bardzo przydatne.
- Zasoby w `ResourceDictionary` dołączone do układu, takich jak `StackLayout` lub `Grid` mogą być stosowane do układu i wszystkie elementy podrzędne tego układu.
- Zasoby w `ResourceDictionary` zdefiniowane na stronie poziomu może odnosić się do strony i jego elementów podrzędnych.
- Zasoby w `ResourceDictionary` zdefiniowane na poziomie aplikacji, poziom mogą być stosowane w całej aplikacji.

Następujące XAML zawiera zasoby zdefiniowane na poziomie aplikacji `ResourceDictionary` w **App.xaml** pliku utworzonego w ramach standardowego programu Xamarin.Forms:

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

To `ResourceDictionary` definiuje trzy [ `Color` ](xref:Xamarin.Forms.Color) zasobów i [ `Style` ](xref:Xamarin.Forms.Style) zasobów. Aby uzyskać więcej informacji na temat `App` klasy, zobacz [klasy aplikacji](~/xamarin-forms/app-fundamentals/application-class.md).

Począwszy od zestawu narzędzi Xamarin.Forms 3.0, jawnie `ResourceDictionary` tagi nie są wymagane. `ResourceDictionary` Obiekt jest tworzony automatycznie i zasobów można wstawić bezpośrednio między `Resources` tagi element właściwości:

```xaml
<Application ...>
    <Application.Resources>
        <Color x:Key="PageBackgroundColor">Yellow</Color>
        <Color x:Key="HeadingTextColor">Black</Color>
        <Color x:Key="NormalTextColor">Blue</Color>
        <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
            <Setter Property="FontAttributes" Value="Bold" />
            <Setter Property="HorizontalOptions" Value="Center" />
            <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
        </Style>
    </Application.Resources>
</Application>
```

Każdy zasób zawiera klucz, który jest określony przy użyciu `x:Key` atrybut, który staje się on w klucz ze słownika `ResourceDictionary`. Ten klucz służy do pobierania zasobu z `ResourceDictionary` przez [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) — rozszerzenie znaczników, jak pokazano w poniższym przykładzie kodu XAML zawiera dodatkowe zasoby zdefiniowane w ramach `StackLayout`:

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

Pierwszy [ `Label` ](xref:Xamarin.Forms.Label) wystąpienia pobiera i wykorzystuje `LabelPageHeadingStyle` zasobu zdefiniowanego na poziomie aplikacji `ResourceDictionary`, drugi `Label` wystąpienia pobierania i wykorzystywania `LabelNormalStyle`zasobu zdefiniowanego w poziom kontroli `ResourceDictionary`. Podobnie [ `Button` ](xref:Xamarin.Forms.Button) wystąpienia pobiera i wykorzystuje `NormalTextColor` zasobu zdefiniowanego na poziomie aplikacji `ResourceDictionary`i `MediumBoldText` zasobu zdefiniowanego w poziom kontroli `ResourceDictionary`. Skutkuje to wygląd pokazano na poniższych zrzutach ekranu:

[![](resource-dictionaries-images/screenshots-sml.png "Korzystanie z zasobów ResourceDictionary")](resource-dictionaries-images/screenshots.png#lightbox "zużywają zasoby ResourceDictionary")

> [!NOTE]
> Zasoby, które są specyficzne dla pojedynczej strony nie powinny być uwzględniane w aplikacji poziomu słownika zasobów, w związku zasobów następnie zostanie przetworzona przy uruchamianiu aplikacji zamiast, gdy jest to wymagane przez stronę. Aby uzyskać więcej informacji, zobacz [zmniejszyć rozmiar słownika zasobów aplikacji](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Zasoby przesłaniające

Gdy `ResourceDictionary` udostępnianie zasobów `x:Key` wartości atrybutów zasoby zdefiniowane w niższych w hierarchii widoku mają wyższy priorytet niż identyczne ze zdefiniowanymi wyższej w górę. Na przykład ustawienie `PageBackgroundColor` zasób `Blue` na poziomie aplikacji, poziom zostanie ona zastąpiona poziomu strony `PageBackgroundColor` zasobów równa `Yellow`. Podobnie, poziomu strony `PageBackgroundColor` zasobu zostanie przesłonięta przez poziom kontroli `PageBackgroundColor` zasobów. Pierwszeństwo tego dowodzą poniższy kod XAML:

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

Oryginalny `PageBackgroundColor` i `NormalTextColor` wystąpień, zdefiniowanych na poziomie aplikacji są zastępowane przez `PageBackgroundColor` i `NormalTextColor` wystąpień zdefiniowane na poziomie strony. W związku z tym kolor tła strony staje się niebieska i tekstu na stronie staje się żółty, jak pokazano w poniższych zrzutach ekranu:

[![](resource-dictionaries-images/overridding-screenshots-sml.png "Zastępowanie zasobów ResourceDictionary")](resource-dictionaries-images/overridding-screenshots.png#lightbox "ResourceDictionary zasoby przesłaniające")

Jednak należy pamiętać, że pasek w tle [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) jest nadal żółty, ponieważ [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) właściwość jest ustawiona na wartość `PageBackgroundColor` zasobu zdefiniowanego w aplikacji poziom `ResourceDictionary`.

Zastanów się, w inny sposób `ResourceDictionary` pierwszeństwo: gdy XAML analizator składni napotka `StaticResource`dopasowany klucz wyszukiwania przez pobaw drzewa wizualnego, za pomocą pierwsze dopasowanie znajdzie. Jeśli to wyszukiwanie kończy się na stronie, a klucz nadal nie został znaleziony, analizator składni XAML wyszukuje `ResourceDictionary` dołączone do `App` obiektu. Jeśli nadal nie znaleziono klucza, zgłaszany jest wyjątek.

## <a name="stand-alone-resource-dictionaries"></a>Słowniki zasobów autonomiczny

Klasa pochodząca z `ResourceDictionary` może również być w oddzielnym pliku autonomicznym. (Bardziej precyzyjne klasy pochodzącej od `ResourceDictionary` zwykle wymaga _pary_ plików, ponieważ zasoby są zdefiniowane w pliku XAML, ale plik CodeBehind z `InitializeComponent` wywołanie jest również.) Plik wynikowy następnie mogą być współużytkowane przez aplikacje.

Aby utworzyć taki plik, Dodaj nowy **widok zawartości** lub **strony zawartości** do projektu (, ale nie **widok zawartości** lub **strony zawartości** z tylko plik języka C#). W pliku XAML i C# pliku, należy zmienić nazwę klasy bazowej z `ContentView` lub `ContentPage` do `ResourceDictionary`. W pliku XAML Nazwa klasy bazowej jest element najwyższego poziomu.

W poniższym przykładzie przedstawiono XAML [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) o nazwie `MyResourceDictionary`:

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

To `ResourceDictionary` zawiera pojedynczy zasób, który jest obiektem typu `DataTemplate`.

Można utworzyć wystąpienie `MyResourceDictionary` , umieszczając go między dwoma `Resources` element Właściwości tagów, na przykład w `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

Wystąpienie `MyResourceDictionary` ustawiono `Resources` właściwość `ContentPage` obiektu.

Jednak takie podejście ma pewne ograniczenia: `Resources` właściwość `ContentPage` odwołuje się tylko ten jeden `ResourceDictionary`. W większości przypadków, chcesz, aby dołączyć inne `ResourceDictionary` wystąpień i może być inne zasoby także.

To zadanie wymaga połączone słowniki zasobów.

## <a name="merged-resource-dictionaries"></a>Połączone słowniki zasobów

Połączone słowniki zasobów połączyć jeden lub więcej `ResourceDictionary` wystąpień do innego `ResourceDictionary`. Możesz to zrobić w pliku XAML, ustawiając [ `MergedDictionaries` ](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) właściwość słowniki zasobów, które zostaną scalone w aplikacji, stron lub poziom kontroli `ResourceDictionary`.

> [!IMPORTANT]
> `ResourceDictionary` definiuje również [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) właściwości. Nie należy używać tej właściwości; jest uznany za przestarzały począwszy od zestawu narzędzi Xamarin.Forms 3.0.

Wystąpienie `MyResourceDictionary` można scalić w dowolnej aplikacji, strony lub poziom kontroli `ResourceDictionary`. Poniższy kod XAML zawiera ona scalana na poziomie strony `ResourceDictionary` przy użyciu `MergedDictionaries` właściwości:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Ten kod przedstawia tylko wystąpienie `MyResourceDictionary` dodawane do `ResourceDictionary` , ale możesz też przywołać inne `ResourceDictionary` wystąpień `MergedDictionary` element Właściwości tagów i innych zasobów spoza tych znaczników:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary.MergedDictionaries>

                <!-- Add more resource dictionaries here -->

                <local:MyResourceDictionary />

                <!-- Add more resource dictionaries here -->

            </ResourceDictionary.MergedDictionaries>

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Może istnieć tylko jeden `MergedDictionaries` sekcji `ResourceDictionary`, ale można umieścić dowolną liczbę `ResourceDictionary` wystąpienia w miejscu, jak chcesz.

W przypadku scalania [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) zasoby udostępnionej identyczne `x:Key` wartości atrybutów i zestawu narzędzi Xamarin.Forms używa w następującej kolejności zasobów:

1. Zasobów lokalnych do słownika zasobów.
1. Do zasobów znajdujących się w słowniku zasobów, która została scalona za pomocą przestarzałego [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) właściwości.
1. Zasoby zawarte w słownikach zasobów, które zostały scalone za pośrednictwem `MergedDictionaries` kolekcji w odwrotnej kolejności, w jakiej występują w `MergedDictionaries` właściwości.

> [!NOTE]
> Wyszukiwanie słowniki zasobów może być zadaniem wymaga dużej mocy obliczeniowej, jeśli aplikacja zawiera wiele słowników dużej ilości zasobów. W związku z tym aby uniknąć niepotrzebnych wyszukiwanie, należy sprawdzić, czy każda strona w aplikacji używa tylko słowniki zasobów, które są odpowiednie dla strony.

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Scalanie słowników w interfejsie Xamarin.Forms 3.0

Począwszy od zestawu narzędzi Xamarin.Forms 3.0, proces scalania [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) wystąpień stało się nieco prostsze i bardziej elastycznym. `MergedDictionaries` Element właściwości tagi nie są już potrzebne. Zamiast tego możesz dodać do słownika zasobów innego `ResourceDictionary` tagu przy użyciu nowego [ `Source` ](xref:Xamarin.Forms.ResourceDictionary.Source) właściwość jest ustawiona na nazwę pliku XAML z zasobami:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary Source="MyResourceDictionary.xaml" />

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Ponieważ automatycznie tworzy zestaw narzędzi Xamarin.Forms 3.0 `ResourceDictionary`, te dwa zewnętrzne `ResourceDictionary` tagi nie są już wymagane:

```xaml
<ContentPage ...>
    <ContentPage.Resources>

        <!-- Add more resources here -->

        <ResourceDictionary Source="MyResourceDictionary.xaml" />

        <!-- Add more resources here -->

    </ContentPage.Resources>
    ...
</ContentPage>
```

Jest to nowa składnia _nie_ wystąpienia `MyResourceDictionary` klasy. Zamiast tego odwołuje się plik XAML. Tego powodu pliku związanego z kodem (**MyResourceDictionary.xaml.cs**) nie jest już wymagane. Można również usunąć `x:Class` atrybut w tagu głównego **MyResourceDictionary.xaml** pliku.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono sposób tworzenia i wykorzystywania [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)oraz sposób scalania słowniki zasobów. A `ResourceDictionary` umożliwia zasoby zdefiniowane w obrębie jednej lokalizacji, i ponownie używane w całej aplikacji platformy Xamarin.Forms.

## <a name="related-links"></a>Linki pokrewne

- [Słowniki zasobów (przykład)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Style](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
