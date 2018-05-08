---
title: Słowniki zasobów
description: Zasoby XAML są obiekty, które mogą być udostępniane i ponownego użycia w aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/02/2018
ms.openlocfilehash: ee3e4c984072fc019fe3719aab650a44d3899911
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="resource-dictionaries"></a>Słowniki zasobów

_Zasoby XAML są definicje obiektów, które mogą być udostępniane i ponownego użycia w aplikacji platformy Xamarin.Forms._

Te obiekty zasobów są przechowywane w słowniku zasobów. W tym artykule opisano sposób tworzenia i zużywać słownik zasobów i sposób scalania słownikach zasobów.

## <a name="overview"></a>Omówienie

A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) to repozytorium dla zasobów, które są używane przez aplikację platformy Xamarin.Forms. Typowe zasoby, które są przechowywane w `ResourceDictionary` obejmują [style](~/xamarin-forms/user-interface/styles/index.md), [kontrolować szablony](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [szablony danych](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), kolory i konwerterów.

W języku XAML, zasobów, które są przechowywane w `ResourceDictionary` następnie można je pobrać i stosowane do elementów za pomocą `StaticResource` — rozszerzenie znaczników. W języku C#, zasoby mogą być zdefiniowana w `ResourceDictionary` pobrane i zastosowane do elementów przy użyciu indeksatora oparte na ciągach. Istnieje jednak małego zaletą używania `ResourceDictionary` w języku C# jako obiekty udostępnione można po prostu być przechowywane jako pola lub właściwości i dostępne bezpośrednio bez konieczności najpierw pobrać je ze słownika.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Tworzenie i korzystanie z ResourceDictionary

Zasoby są zdefiniowane w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) który jest następnie ustawioną na jedną z następujących `Resources` właściwości:

- [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) Właściwości każdej klasy, która jest pochodną [`Application`](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)
- [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Właściwości każdej klasy, która jest pochodną ["VisualElement"](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)

Program platformy Xamarin.Forms zawiera tylko jedną klasę, która pochodzi z `Application` , ale często korzysta z wielu klas, które pochodzą z `VisualElement`, w tym stron, układów i kontrolek. Dowolnego z tych obiektów może mieć jego `Resources` ustawioną właściwość `ResourceDictionary`. Wybieranie gdzie umieścić określonego `ResourceDictionary` wpływa na której można użyć zasobów:

- Zasoby w `ResourceDictionary` takich jak, która jest dołączona do widoku `Button` lub `Label` można stosować na dany obiekt, więc nie jest to bardzo przydatne.
- Zasoby w `ResourceDictionary` dołączony układ, takich jak `StackLayout` lub `Grid` można zastosować układu i wszystkie elementy podrzędne tego układu. 
- Zasoby w `ResourceDictionary` zdefiniowane na stronie poziomu może odnosić się do strony i wszystkich jego elementów podrzędnych.
- Zasoby w `ResourceDictionary` zdefiniowanych w aplikacji poziomu mogą być stosowane w całej aplikacji.

Następujący kod XAML zawiera zasoby zdefiniowane w poziomie aplikacji `ResourceDictionary` w **App.xaml** pliku utworzonego w ramach standardowego programu platformy Xamarin.Forms:

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

To `ResourceDictionary` definiuje trzy [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) zasobów i [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) zasobów. Aby uzyskać więcej informacji na temat `App` , zobacz [klasy aplikacji](~/xamarin-forms/app-fundamentals/application-class.md).

Począwszy od platformy Xamarin.Forms 3.0 jawnych `ResourceDictionary` tagi nie są wymagane. `ResourceDictionary` Obiekt jest tworzony automatycznie i zasobów można wstawić bezpośrednio między `Resources` znaczniki elementu właściwości:

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

Wszystkie zasoby, ma klucz, który jest określany przy użyciu `x:Key` atrybut, który staje się on klucz słownika w `ResourceDictionary`. Klucz służy do pobierania zasobu z `ResourceDictionary` przez [ `StaticResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/) — rozszerzenie znaczników, jak pokazano w poniższym przykładzie kodu XAML zawiera dodatkowe zasoby zdefiniowane w ramach `StackLayout`:

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

Pierwszy [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpienia pobiera i wykorzystuje `LabelPageHeadingStyle` zasobów zdefiniowane na poziomie aplikacji `ResourceDictionary`, drugi `Label` wystąpienie pobieranie i przetwarzanie `LabelNormalStyle`zasobów zdefiniowane na poziomie formantu `ResourceDictionary`. Podobnie [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wystąpienia pobiera i wykorzystuje `NormalTextColor` zasobów zdefiniowane na poziomie aplikacji `ResourceDictionary`i `MediumBoldText` zasobów zdefiniowane na poziomie formantu `ResourceDictionary`. Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

[![](resource-dictionaries-images/screenshots-sml.png "Korzystanie z zasobów ResourceDictionary")](resource-dictionaries-images/screenshots.png#lightbox "korzysta z zasobów ResourceDictionary")

> [!NOTE]
> Zasoby, które są specyficzne dla pojedynczej strony nie powinny znajdować się w aplikacji zasobów na poziomie słownika, jako takie zasoby następnie będzie analizowany przy uruchamianiu aplikacji zamiast, gdy jest to wymagane przez stronę. Aby uzyskać więcej informacji, zobacz [zmniejszyć rozmiar słownika zasobów aplikacji](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Zastępowanie zasobów

Gdy `ResourceDictionary` udostępnianie zasobów `x:Key` wartości atrybutów zasobów niższych zdefiniowany w hierarchii widoku mają wyższy priorytet niż określone wyższy się. Na przykład ustawienie `PageBackgroundColor` zasobów do `Blue` w aplikacji poziom zostanie ona zastąpiona na poziomie strony `PageBackgroundColor` ustawioną zasobów `Yellow`. Podobnie, poziomu strony `PageBackgroundColor` zasobu zostanie przesłonięta przez poziom kontroli `PageBackgroundColor` zasobów. Pierwszeństwo tego przedstawiono na poniższym przykładzie kodu XAML:

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

Jednak należy pamiętać, że na pasku tła [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) jest nadal żółty, ponieważ [ `BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/) właściwości ustawiono wartość `PageBackgroundColor` zasobów zdefiniowanych w aplikacji poziom `ResourceDictionary`.

W inny sposób myśleć o `ResourceDictionary` pierwszeństwo: analizatora składni po XAML napotka `StaticResource`dopasowany klucz wyszukiwania przez określanego górę drzewa wizualnego, za pomocą pierwszego dopasowania znajdzie. Jeżeli to wyszukiwanie kończy się na stronie, a klucz nadal nie został znaleziony, analizator XAML wyszukuje `ResourceDictionary` dołączony do `App` obiektu. Jeśli nadal nie znaleziono klucza, wyjątek.

## <a name="stand-alone-resource-dictionaries"></a>Słowniki zasobów autonomicznej

Klasa pochodna od `ResourceDictionary` można też w osobnym pliku autonomicznym. (Dokładnie klasę pochodzącą od `ResourceDictionary` zwykle wymaga _pary_ plików, ponieważ zasoby są zdefiniowane w pliku XAML, ale plik CodeBehind o `InitializeComponent` wywołanie jest także niezbędne.) Wynikowy plik może być następnie udostępniane między aplikacjami.

Aby utworzyć taki plik, Dodaj nową **widok zawartości** lub **strony zawartości** elementu do projektu (, ale nie **widok zawartości** lub **strony zawartości** z tylko C# plik). W pliku XAML i C# pliku, należy zmienić nazwę klasy podstawowej z `ContentView` lub `ContentPage` do `ResourceDictionary`. W pliku XAML nazwa klasą podstawową jest element najwyższego poziomu.

W poniższym przykładzie przedstawiono XAML [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) o nazwie `MyResourceDictionary`:

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

To `ResourceDictionary` zawiera pojedynczego zasobu, który jest obiektem typu `DataTemplate`.

Można utworzyć wystąpienia `MyResourceDictionary` umieszczając między dwoma `Resources` elementu właściwości tagów, na przykład w `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>  
```

Wystąpienie `MyResourceDictionary` ustawiono `Resources` właściwość `ContentPage` obiektu.

Jednak takie podejście ma następujące ograniczenia: `Resources` właściwość `ContentPage` odwołuje się tylko ten jeden `ResourceDictionary`. W większości przypadków, ma możliwość włączenia innych `ResourceDictionary` wystąpień i prawdopodobnie innych zasobów oraz.

To zadanie wymaga scalonych słownikach zasobów.

## <a name="merged-resource-dictionaries"></a>Połączone słowniki zasobów

Łączenie scalonych słownikach zasobów co najmniej jeden `ResourceDictionary` wystąpień do innego `ResourceDictionary`. Można to zrobić w pliku XAML, ustawiając [ `MergedDictionaries` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedDictionaries/) właściwości słowniki zasobów, które zostaną scalone w aplikacji, strony lub poziom kontroli `ResourceDictionary`.

> [!IMPORTANT]
> `ResourceDictionary` definiuje również [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) właściwości. Nie należy używać tej właściwości; jest przestarzały począwszy od platformy Xamarin.Forms 3.0.

Obiekt `MyResourceDictionary` można scalić w dowolnej aplikacji, strony lub poziom kontroli `ResourceDictionary`. Poniższy przykładowy kod XAML zawiera on scalane w poziomie strony `ResourceDictionary` przy użyciu `MergedDictionaries` właściwości:

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

Ten kod przedstawia tylko wystąpienia `MyResourceDictionary` dodawany do `ResourceDictionary` , ale można także odwoływać się do innych `ResourceDictionary` wystąpień w ramach `MergedDictionary` znaczniki elementu właściwości i innych zasobów poza tych tagów:

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

Może istnieć tylko jedna `MergedDictionaries` sekcji `ResourceDictionary`, ale możesz też zaznaczyć tyle `ResourceDictionary` wystąpień w nim można dowolnie.

W przypadku scalania [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) zasoby współużytkować identyczne `x:Key` wartości atrybutów, platformy Xamarin.Forms używa w następującej kolejności zasobów:

1. Zasoby lokalne do słownika zasobów.
1. Zasoby zawartych w słowniku zasobów, który został scalony za pośrednictwem przestarzałe [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) właściwości.
1. Zasoby zawarte w słownikach zasobów, które zostały scalone za pośrednictwem `MergedDictionaries` kolekcji w kolejności, w jakiej występują w `MergedDictionaries` właściwości.

> [!NOTE]
> Wyszukiwanie słowniki zasobów może być praktyce znacznym zadania, jeśli aplikacja zawiera wiele słowniki dużych zasobów. W związku z tym aby uniknąć niepotrzebnych wyszukiwanie, należy upewnić się czy każdej strony w aplikacji używa tylko słowniki zasobów, które są odpowiednie do strony.

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Scalanie słowniki w platformy Xamarin.Forms 3.0

Począwszy od platformy Xamarin.Forms 3.0, proces scalania `ResourceDictionaries` stał się nieco łatwiejsze i bardziej elastyczne. `MergedDictionaries` Znaczniki elementu właściwości nie są już wymagane. Zamiast tego możesz dodać do słownika zasobów innego `ResourceDictionary` tagu z nowym [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.Source/) właściwość nazwy pliku XAML z zasobami:

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

Ponieważ 3.0 platformy Xamarin.Forms automatycznie tworzy `ResourceDictionary`, te dwie zewnętrzne `ResourceDictionary` tagi nie są już wymagane:

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

Jest to nowej składni _nie_ wystąpienia `MyResourceDictionary` klasy. Zamiast tego odwołuje się plik XAML. Który przyczyny pliku CodeBehind (**MyResourceDictionary.xaml.cs**) nie jest już wymagane. Można również usunąć `x:Class` atrybut z tagu głównego **MyResourceDictionary.xaml** pliku. 

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak utworzyć i korzystać z [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)oraz sposób scalania słownikach zasobów. A `ResourceDictionary` umożliwia zasoby zdefiniowane w jednej lokalizacji i ponownego użycia w aplikacji platformy Xamarin.Forms.

## <a name="related-links"></a>Linki pokrewne

- [Słowniki zasobów (na przykład)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Style](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
