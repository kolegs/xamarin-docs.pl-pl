---
title: Xamarin.Forms Performance
description: "Istnieje wiele technik zwiększania wydajności aplikacji platformy Xamarin.Forms. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację. W tym artykule opisano i omówiono te techniki."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: b7b5093f9a564c0711ddc8a711f9b609d44e7dad
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinforms-performance"></a>Xamarin.Forms Performance

_Istnieje wiele technik zwiększania wydajności aplikacji platformy Xamarin.Forms. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację. W tym artykule opisano i omówiono te techniki._

[ ![](performance-images/evolve-jason-perf-sml.png "Optymalizacja wydajności aplikacji za pomocą platformy Xamarin.Forms")](https://evolve.xamarin.com/session/56e205b0bad314273ca4d817)

[Rozwija 2016: Optymalizacja wydajności aplikacji za pomocą platformy Xamarin.Forms](https://evolve.xamarin.com/session/56e205b0bad314273ca4d817)

## <a name="overview"></a>Omówienie

Niską wydajnością przedstawia na wiele sposobów. Go aplikacja prawdopodobnie nie odpowiada, może spowodować wolne przewijanie i może zmniejszyć czas pracy baterii. Jednak optymalizacji wydajności wymaga więcej niż tylko wdrażania wydajność kodu. Środowisko użytkownika wydajność aplikacji, należy również rozważyć. Na przykład zapewniając wykonanie operacji bez blokowania użytkownika wykonywanie innych działań może pomóc ulepszyć środowisko użytkownika.

Istnieje szereg technik zwiększenie wydajności i obserwowaną wydajność aplikacji platformy Xamarin.Forms. Obejmują one:

- [Włącz kompilatora XAML](#xamlc)
- [Wybierz prawidłowe układu](#correctlayout)
- [Włącz kompresję układu](#layoutcompression)
- [Użyj szybkiego renderowania](#fastrenderers)
- [Zmniejsz niepotrzebnych powiązania](#databinding)
- [Optymalizacja wydajności układu](#optimizelayout)
- [Optymalizowanie ListView](#optimizelistview)
- [Optymalizacja zasoby obrazów](#optimizeimages)
- [Zmniejsz rozmiar drzewa wizualnego](#visualtree)
- [Zmniejsz rozmiar słownika zasobów aplikacji](#resourcedictionary)
- [Użyj wzorca niestandardowego modułu renderowania](#rendererpattern)

> [!NOTE]
>  Przed przeczytaniem tego artykułu warto najpierw przeczytać artykuł [wydajności i Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md), omówiono w nim-platforma określonych technik w celu poprawy użycie pamięci i wydajność aplikacji utworzony za pomocą platformy Xamarin.

<a name="xamlc" />

## <a name="enable-the-xaml-compiler"></a>Włącz kompilatora XAML

XAML mogą być opcjonalnie kompilowane bezpośrednio na język pośredni (IL) z kompilatora XAML (XAMLC). XAMLC oferuje wiele korzyści:

- Wykonuje sprawdzanie kompilacji XAML powiadomienia użytkownika o błędach.
- Usuwa niektóre godziny obciążenia i konkretyzacji elementów XAML.
- Pomaga zmniejszyć rozmiar pliku w zestawie końcowym poprzez nie jest już w tym pliki .xaml.

XAMLC jest domyślnie wyłączona, aby zapewnić zgodność wstecz. Jednak można ją włączyć w zestawie i poziomie klasy. Aby uzyskać więcej informacji, zobacz [kompilacji XAML](~/xamarin-forms/xaml/xamlc.md).

<a name="correctlayout" />

## <a name="choose-the-correct-layout"></a>Wybierz prawidłowe układu

Układ, który jest umożliwiająca wyświetlanie wiele obiektów podrzędnych, ale ma tylko pojedynczy element potomny, jest niepotrzebne. Na przykład poniższy kod przedstawia przykład [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) z pojedynczy element potomny:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <StackLayout>
            <Image Source="waterfront.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

To jest niepotrzebne i [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) element powinna zostać usunięta, jak pokazano w poniższym przykładzie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <Image Source="waterfront.jpg" />
    </ContentPage.Content>
</ContentPage>
```

Ponadto nie próba odtworzenia wygląd określonego układu przy użyciu kombinacji innych układów, powoduje to niepotrzebnych układu obliczenia wykonywane. Na przykład nie próbować odtworzyć [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) układu przy użyciu kombinacji [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) wystąpień. Poniższy przykładowy kod przedstawia przykład to złe rozwiązanie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" />
                <Entry Placeholder="Enter your name" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Age:" />
                <Entry Placeholder="Enter your age" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Occupation:" />
                <Entry Placeholder="Enter your occupation" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Address:" />
                <Entry Placeholder="Enter your address" />
            </StackLayout>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Może to być niepotrzebne, ponieważ układ niepotrzebnych obliczenia są wykonywane. Zamiast tego żądany układ mogą być osiągnięte przy użyciu [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), jak pokazano w poniższym przykładzie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="100" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
            </Grid.RowDefinitions>
            <Label Text="Name:" />
            <Entry Grid.Column="1" Placeholder="Enter your name" />
            <Label Grid.Row="1" Text="Age:" />
            <Entry Grid.Row="1" Grid.Column="1" Placeholder="Enter your age" />
            <Label Grid.Row="2" Text="Occupation:" />
            <Entry Grid.Row="2" Grid.Column="1" Placeholder="Enter your occupation" />
            <Label Grid.Row="3" Text="Address:" />
            <Entry Grid.Row="3" Grid.Column="1" Placeholder="Enter your address" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

<a name="layoutcompression" />

## <a name="enable-layout-compression"></a>Włącz kompresję układu

Kompresja układu usuwa określony układów z drzewa wizualnego, w celu zwiększenia wydajności renderowania strony. Korzyści wydajności, który zapewnia to różni się w zależności od złożoności strony, wersja używanego systemu operacyjnego i urządzenia, na którym jest uruchomiona aplikacja. Jednakże wydajność będzie widoczny na starszych urządzeń. Aby uzyskać więcej informacji, zobacz [kompresji układu](~/xamarin-forms/user-interface/layouts/layout-compression.md).

<a name="fastrenderers" />

## <a name="use-fast-renderers"></a>Użyj szybkiego renderowania

Szybkie renderowania zmniejszyć inflacji i kosztów renderowanie kontrolek platformy Xamarin.Forms w systemie Android przez spłaszczanie wynikowy hierarchii macierzystego formantu. Dalsze to zwiększa wydajność tworzenia mniejszą liczbę obiektów, który włącza wyniki w drzewie wizualnym mniej złożona i mniej wykorzystania pamięci. Aby uzyskać więcej informacji, zobacz [Fast renderowania](~/xamarin-forms/internals/fast-renderers.md).

<a name="databinding" />

## <a name="reduce-unnecessary-bindings"></a>Zmniejsz niepotrzebnych powiązania

Nie używaj wiązania dla zawartości, którą można łatwo skonfigurować statycznie. Nie ma żadnych dodatkowych zalet w powiązania danych, które nie muszą być powiązane, ponieważ powiązania nie są wydajne kosztów. Na przykład ustawienie `Button.Text = "Accept"` mają mniejszy narzut niż powiązanie [ `Button.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/) do ViewModel `string` właściwość z wartością "Akceptuj".

<a name="optimizelayout" />

## <a name="optimize-layout-performance"></a>Optymalizacja wydajności układu

Platformy Xamarin.Forms 2 wprowadzono aparatu układu zoptymalizowane, który ma wpływ na aktualizacje układu. Aby uzyskać najlepszą wydajność układ, postępuj zgodnie z tymi wytycznymi:

- Zmniejsz głębokość hierarchii układu, określając [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) wartości właściwości, umożliwiające tworzenie układów z widokami zawijania mniej. Aby uzyskać więcej informacji, zobacz [marginesy i dopełnienia](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).
- Korzystając z [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), należy starać się ustawioną jako kilka wierszy i kolumn, jak to możliwe [ `Auto` ](https://developer.xamarin.com/api/property/Xamarin.Forms.GridLength.Auto/) rozmiar. Każdego wiersza o rozmiarze auto lub kolumny spowoduje, że aparat układu do obliczeń dodatkowe układu. Zamiast tego Użyj stałego rozmiaru wierszy i kolumn, jeśli to możliwe. Możesz również ustawić wierszy i kolumn zajmować proporcjonalna ilość miejsca na [ `GridUnitType.Star` ](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) wartość wyliczenia, pod warunkiem że drzewa nadrzędnego postępuje wytyczne układu.
- Nie należy ustawiać [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) i [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości układu o ile nie jest wymagane. Wartości domyślne [ `LayoutOptions.Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) i [ `LayoutOptions.FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/) Zezwalaj najlepsze optymalizacji układu. Zmiana tych właściwości koszt i zajmuje pamięć, nawet wtedy, gdy ich ustawienie na wartości domyślne.
- Unikaj używania [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) zawsze, gdy jest to możliwe. Spowoduje Procesora o przeprowadzenie znacznie więcej pracy.
- Korzystając z [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), należy unikać [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) właściwości, jeśli to możliwe.
- Korzystając z [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), upewnij się, że tylko jeden element podrzędny ma ustawioną wartość [ `LayoutOptions.Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/). Ta właściwość zapewnia zajmie określonego elementu podrzędnego największej miejsce `StackLayout` można przekazać go i jest niepotrzebne przeprowadzenie tych obliczeń więcej niż raz.
- Nie wywołuj dowolnej z metod [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) klasy, zgodnie z ich wynikiem obliczenia kosztowne układu wykonywana. Zamiast tego jest prawdopodobne, że zachowanie żądany układ można uzyskać przez ustawienie [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) i [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) właściwości. Alternatywnie podklasy [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) klasa umożliwia zachowanie żądany układ.
- Nie aktualizuj żadnego [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpienia częściej niż to konieczne, ponieważ zmiana rozmiaru etykiety może spowodować w układzie cały ekran jest ponownie obliczane.
- Nie należy ustawiać [ `Label.VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) właściwości o ile nie jest wymagane.
- Ustaw [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) dowolnego [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień do [ `NoWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.NoWrap/) zawsze, gdy jest to możliwe.

<a name="optimizelistview" />

## <a name="optimize-listview-performance"></a>Optymalizowanie ListView

Korzystając z [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kontroli istnieje szereg funkcji środowiska użytkownika, które powinno być zoptymalizowane:

- **Inicjowanie** — przedział czasu, uruchomienie, gdy formant nie zostanie utworzony, a podczas elementy są wyświetlane na ekranie.
- **Przewijanie** — możliwość przewiń listę i upewnij się, że interfejs użytkownika nie opóźniona touch gestów.
- **Interakcja** Dodawanie, usuwanie i Zaznaczanie elementów.

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Formant wymaga aplikacji do dostarczania danych i komórki szablonów. Jak jest to osiągane mają duży wpływ na wydajność formantu. Aby uzyskać więcej informacji, zobacz [wydajności ListView](~/xamarin-forms/user-interface/listview/performance.md).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optymalizacja zasoby obrazów

Wyświetlanie zasobów obrazu może znacznie zwiększyć zużycie pamięci w aplikacji. W związku z tym ich powinien zostać utworzony tylko gdy jest wymagana, a powinny zostać zwolnione, jak aplikacja nie wymaga już je. Na przykład jeśli aplikacja wyświetla obraz przez odczytanie danych ze strumienia, upewnij się, że strumieniu jest tworzony tylko wtedy, gdy jest to wymagane i upewnij się, zwolnienie strumienia, gdy nie są już wymagane. Można to osiągnąć, tworząc strumień po utworzeniu strony lub gdy [ `Page.Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) generowane zdarzenie, a następnie usuwania strumienia po [ `Page.Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Disappearing/) generowane zdarzenie.

Podczas pobierania obraz do wyświetlenia z [ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) metodę, pamięć podręczna pobrany obraz za zapewnienie, że [ `UriImageSource.CachingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) właściwość jest ustawiona na `true`. Aby uzyskać więcej informacji, zobacz [Praca z obrazami](~/xamarin-forms/user-interface/images.md).

Aby uzyskać więcej informacji, zobacz [optymalizacji zasobów obrazu](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages).

<a name="visualtree" />

## <a name="reduce-the-visual-tree-size"></a>Zmniejsz rozmiar drzewa wizualnego

Zmniejszenie liczby elementów na stronie spowoduje szybsze renderowania strony. Istnieją dwie metody głównej na osiągnięcie tego celu. Pierwsza to Ukryj elementy, które nie są widoczne. [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) Właściwości każdy element określa, czy element powinien być częścią drzewa wizualnego, czy nie. W związku z tym, jeśli element jest niewidoczny, ponieważ jest ukryta za innymi elementami, Usuń element lub ustaw jego `IsVisible` właściwości `false`.

Drugi technika jest usunięcie niepotrzebnych elementów. Na przykład następujący przykładowy kod przedstawia układ strony, który prezentuje serię z [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) elementy:

```xaml
<ContentPage.Content>
    <StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Hello" />
        </StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Welcome to the App!" />
        </StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Downloading Data..." />
        </StackLayout>
    </StackLayout>
</ContentPage.Content>
```

Ten sam układ strony mogą być obsługiwane z liczbą zmniejszenie element, jak pokazano w poniższym przykładzie kodu:

```xaml
<ContentPage.Content>
  <StackLayout Padding="20,20,0,0" Spacing="25">
    <Label Text="Hello" />
    <Label Text="Welcome to the App!" />
    <Label Text="Downloading Data..." />
  </StackLayout>
</ContentPage.Content>
```

<a name="resourcedictionary" />

## <a name="reduce-the-application-resource-dictionary-size"></a>Zmniejsz rozmiar słownika zasobów aplikacji

Wszystkie zasoby, które są używane w całej aplikacji powinny być przechowywane w słowniku zasobów aplikacji, aby uniknąć jego duplikowania. Dzięki temu można zmniejszyć liczbę XAML, który ma zostać przeanalizowany w całej aplikacji. Poniższy kod przedstawia przykład `HeadingLabelStyle` zasób, który jest używana aplikacja szerokości i dlatego jest zdefiniowany w słowniku zasobów aplikacji:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Resources.App">
     <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
     </Application.Resources>
</Application>
```

Jednak XAML, specyficzne dla strony nie powinny być uwzględniane słownik zasobów aplikacji, jak zasoby następnie będzie analizowany przy uruchamianiu aplikacji zamiast, gdy jest to wymagane przez stronę. Jeśli zasób jest używany przez strony, która nie jest strony początkowej, powinna zostać umieszczona w słowniku zasobów dla tej strony, dlatego obniżając XAML, który jest analizowana podczas uruchamiania aplikacji. Poniższy kod przedstawia przykład `HeadingLabelStyle` zasób, który jest tylko na jednej stronie, a więc jest zdefiniowany w słowniku zasobów strony:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Test.HomePage"
             Padding="0,20,0,0">
     <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
     </ContentPage.Resources>
    <ContentPage.Content>
      ...
    </ContentPage.Content>
</ContentPage>

```

Aby uzyskać więcej informacji na temat zasobów aplikacji, zobacz [ `Working with Styles` ](~/xamarin-forms/user-interface/styles/index.md).

<a name="rendererpattern" />

## <a name="use-the-custom-renderer-pattern"></a>Użyj wzorca niestandardowego modułu renderowania

Większość renderowania klasy Uwidacznianie `OnElementChanged` metodę, która jest wywoływana po utworzeniu platformy Xamarin.Forms formantu niestandardowego do renderowania kontrolki na odpowiednich macierzystego. Klasy niestandardowego modułu renderowania, w każdej klasie renderowania specyficzne dla platformy, następnie zastąpić tę metodę w celu utworzenia wystąpienia i dostosowanie macierzystego formantu. `SetNativeControl` Metoda jest używana do macierzystego formantu, a ta metoda będzie również przypisać odwołania formantu do `Control` właściwości.

Jednak w pewnych okolicznościach `OnElementChanged` metoda może być wywołana wiele razy. W związku z tym aby zapobiec przecieki pamięci, które mogą mieć negatywny wpływ na wydajność, należy uważać podczas tworzenia wystąpienia nowego macierzystego formantu. Podejście do użycia podczas tworzenia wystąpienia nowego macierzystego formantu w niestandardowego modułu renderowania pokazano w poniższym przykładzie kodu:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Nowe macierzystego formantu były tworzone tylko raz, jeśli `Control` jest właściwość `null`. Kontrolki tylko należy skonfigurować i subskrypcję procedury obsługi zdarzeń, gdy niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Podobnie programy obsługi zdarzeń, które zostały subskrybuje tylko należy anulować w, gdy element renderującego jest dołączony do zmiany. Przyjmowanie takie podejście pomoże utworzyć wydajnie wykonywania niestandardowego modułu renderowania, które nie występują z przecieki pamięci.

Aby uzyskać więcej informacji na temat niestandardowe moduły renderowania, zobacz [Dostosowywanie formantów na każdej platformie](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule opisano i opisem techniki zwiększania wydajności aplikacji platformy Xamarin.Forms. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację.


## <a name="related-links"></a>Linki pokrewne

- [Wydajność i Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Wydajność ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [Szybkie programy renderujące](~/xamarin-forms/internals/fast-renderers.md)
- [Kompresja układu](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Przykładowe zmiany rozmiaru obrazu platformy Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/XamFormsImageResize/)
- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilation/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
