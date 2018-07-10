---
title: Wydajności zestawu narzędzi Xamarin.Forms
description: Istnieje wiele technik na zwiększenie wydajności aplikacji platformy Xamarin.Forms. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywanej przez Procesora i ilość pamięci używanej przez aplikację. W tym artykule opisano i omówiono tych metod.
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: ae284cf90ccb2d2735b4fafa0c0e44f69533638f
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935163"
---
# <a name="xamarinforms-performance"></a>Wydajności zestawu narzędzi Xamarin.Forms

_Istnieje wiele technik na zwiększenie wydajności aplikacji platformy Xamarin.Forms. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywanej przez Procesora i ilość pamięci używanej przez aplikację. W tym artykule opisano i omówiono tych metod._

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**Rozwój 2016: Optymalizacja wydajności aplikacji za pomocą zestawu narzędzi Xamarin.Forms**

## <a name="overview"></a>Omówienie

Niską wydajność aplikacji przedstawia na wiele sposobów. Może sprawić, że aplikacja prawdopodobnie nie odpowiada, mogą powodować wolne przewijanie i może zmniejszyć czas pracy baterii. Jednak Optymalizacja wydajności polega na więcej niż tylko wdrażania skutecznego kodu. Należy również uwzględnić środowisko wydajność aplikacji. Na przykład zapewnienie, że operacje są wykonywane bez blokowania użytkownikowi wykonywanie innych działań może pomóc ulepszyć środowisko użytkownika.

Istnieje kilka technik, pozwalające zwiększyć wydajność i obserwowaną wydajność aplikacji platformy Xamarin.Forms. Obejmują one:

- [Włącz kompilator XAML](#xamlc)
- [Wybierz prawidłowe układu](#correctlayout)
- [Włącz kompresję układu](#layoutcompression)
- [Użyj szybkie programy renderujące](#fastrenderers)
- [Ograniczyć niepotrzebne powiązania](#databinding)
- [Zoptymalizuj układ](#optimizelayout)
- [Optymalizuj wydajność ListView](#optimizelistview)
- [Optymalizuj zasoby obrazów](#optimizeimages)
- [Zmniejsz rozmiar drzewa wizualnego](#visualtree)
- [Zmniejsz rozmiar słownika zasobów aplikacji](#resourcedictionary)
- [Użyj wzorca niestandardowego modułu renderowania](#rendererpattern)

> [!NOTE]
>  Przed przeczytaniem tego artykułu warto najpierw przeczytać artykuł [wydajności dla wielu Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md), który w tym artykule omówiono określonych technik-platform, aby poprawić wykorzystanie pamięci i wydajność aplikacji utworzonych za pomocą platformy Xamarin.

<a name="xamlc" />

## <a name="enable-the-xaml-compiler"></a>Włącz kompilator XAML

XAML może być opcjonalnie kompilowane bezpośrednio do języka pośredniego (IL) za pomocą kompilatora XAML (XAMLC). XAMLC oferuje wiele korzyści:

- Wykonuje sprawdzanie kompilacji XAML, powiadamiania użytkownika o błędach.
- Usuwa pewien czas ładowania i wystąpienia elementów XAML.
- Pomaga zmniejszyć rozmiar pliku ostateczny zestaw przy nie jest już w tym pliki .xaml.

XAMLC jest domyślnie wyłączona, aby zapewnić zgodność wstecz. Jednakże można włączyć na zestawie i na poziomie klasy. Aby uzyskać więcej informacji, zobacz [kompilacji XAML](~/xamarin-forms/xaml/xamlc.md).

<a name="correctlayout" />

## <a name="choose-the-correct-layout"></a>Wybierz prawidłowe układu

Układ jest umożliwiająca wyświetlanie wiele obiektów podrzędnych, ale ma tylko pojedynczy element podrzędny, jest niepotrzebne. Na przykład, poniższy kod przedstawia przykład [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) z pojedynczy element podrzędny:

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

Jest to marnotrawstwa i [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) element powinny zostać usunięte, jak pokazano w poniższym przykładzie kodu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <Image Source="waterfront.jpg" />
    </ContentPage.Content>
</ContentPage>
```

Ponadto nie podejmuj próby odtworzenia wygląd określonego układu przy użyciu kombinacji innych układów, co powoduje to niepotrzebne układ obliczenia wykonywane. Na przykład Nie podejmuj próby odtworzenia [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) układu przy użyciu kombinacji [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) wystąpień. Poniższy przykład kodu przedstawia przykład tego złym zwyczajem:

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

Może to być niepotrzebne, ponieważ układ niepotrzebne obliczenia są wykonywane. Zamiast tego żądany układ mogą być osiągnięte przy użyciu [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), jak pokazano w poniższym przykładzie kodu:

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

Kompresja układu usuwa określony układy z drzewa wizualnego w celu zwiększenia wydajności renderowania strony. Korzyści wydajności, które ten dostarcza różni się w zależności od złożoności strony, wersja używanego systemu operacyjnego i urządzenia, na którym działa aplikacja. Jednakże wydajność będzie widoczna w starszych urządzeń. Aby uzyskać więcej informacji, zobacz [kompresja układu](~/xamarin-forms/user-interface/layouts/layout-compression.md).

<a name="fastrenderers" />

## <a name="use-fast-renderers"></a>Użyj szybkie programy renderujące

Szybkie programy renderujące skrócić inflacji i koszty renderowanie kontrolek zestawu narzędzi Xamarin.Forms w systemie Android spłaszczając wynikowy hierarchii kontroli natywnych. To dodatkowo zwiększa wydajność, tworząc mniej obiektów, spowoduje to w wynikach w drzewie wizualnym mniej skomplikowany i mniej wykorzystania pamięci. Aby uzyskać więcej informacji, zobacz [szybkie programy renderujące](~/xamarin-forms/internals/fast-renderers.md).

<a name="databinding" />

## <a name="reduce-unnecessary-bindings"></a>Ograniczyć niepotrzebne powiązania

Nie używaj wiązania dla zawartości, która może być łatwo ustawiony statycznie. Nie ma żadnych dodatkowych zalet w powiązaniu danych, które nie muszą być powiązane, ponieważ powiązania nie są ponoszenia wysokich kosztów. Na przykład ustawienie `Button.Text = "Accept"` ma mniejsze obciążenie niż powiązanie [ `Button.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/) do ViewModel `string` właściwość z wartością "Akceptuj".

<a name="optimizelayout" />

## <a name="optimize-layout-performance"></a>Zoptymalizuj układ

Zestaw narzędzi Xamarin.Forms 2 wprowadzono aparat zoptymalizowany układ, który wpływa na aktualizacje układu. Aby uzyskać najlepszą wydajność układ możliwe, należy przestrzegać następujących wytycznych:

- Zmniejsz głębokość hierarchii układu, określając [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) wartości właściwości, zezwalanie na tworzenie układy za pomocą mniejszej liczby widoków zawijania. Aby uzyskać więcej informacji, zobacz [marginesy i dopełnienie](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).
- Korzystając z [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), spróbuj upewnić się, że jak najmniejszej liczby wierszy i kolumn, jak to możliwe, są ustawione na [ `Auto` ](https://developer.xamarin.com/api/property/Xamarin.Forms.GridLength.Auto/) rozmiar. Każdy rozmiar automatyczny wiersza lub kolumny spowoduje, że aparat układu do wykonywania obliczeń dodatkowe układu. Zamiast tego Użyj stałego rozmiaru wierszy i kolumn, jeśli jest to możliwe. Alternatywnie zestawu wierszy i kolumn zajmować proporcjonalną ilość miejsca na [ `GridUnitType.Star` ](xref:Xamarin.Forms.GridUnitType.Star) wartość wyliczenia, pod warunkiem że drzewa nadrzędnego następuje wskazówek układu.
- Nie należy ustawiać [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) i [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości układu o ile nie jest wymagane. Z wartościami domyślnymi [ `LayoutOptions.Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill) i [ `LayoutOptions.FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) umożliwiający najlepsze optymalizacji układu. Zmiana tych właściwości jest kosztowne i wykorzystuje pamięć, nawet wtedy, gdy ustawić dla nich wartości domyślne.
- Unikaj używania [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) zawsze, gdy jest to możliwe. Spowoduje on CPU o do wykonywania pracy znacznie więcej.
- Korzystając z [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), należy unikać [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) właściwość, jeśli to możliwe.
- Korzystając z [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), upewnij się, że tylko jeden element podrzędny jest ustawiona na [ `LayoutOptions.Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/). Tej właściwości gwarantuje, że zajmie określonego elementu podrzędnego największy miejsca `StackLayout` może być i jest marnotrawstwa do wykonywania tych obliczeń w więcej niż jeden raz.
- Nie mogą wywoływać żadnych metod [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) klasy, ponieważ powodują one układ kosztowne obliczenia wykonywane. Zamiast tego jest prawdopodobne, że zachowanie żądany układ można uzyskać przez ustawienie [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) i [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) właściwości. Alternatywnie podklasy [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) klasy, aby osiągnąć żądany układ zachowanie.
- Nie aktualizuj dowolne [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień częściej niż jest to wymagane, ponieważ zmiana rozmiaru etykieta może spowodować w układzie cały ekran, są ponownie obliczane.
- Nie należy ustawiać [ `Label.VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) właściwości wymagane.
- Ustaw [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) dowolnego [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień do [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode.NoWrap) zawsze, gdy jest to możliwe.

<a name="optimizelistview" />

## <a name="optimize-listview-performance"></a>Optymalizuj wydajność ListView

Korzystając z [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kontroli, istnieje kilka funkcji środowiska użytkownika, które powinno być zoptymalizowane pod:

- **Inicjowanie** — interwał czasu, rozpoczynając od formant zostanie utworzony, a kończy, gdy elementy są wyświetlane na ekranie.
- **Przewijanie** — możliwość przewiń listę i upewnij się, że interfejs użytkownika nie opóźnione w stosunku gesty dotykowe.
- **Interakcja** Dodawanie, usuwanie i Wybieranie elementów.

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Kontrolki wymaga od aplikacji do dostarczania danych i komórki szablonów. Jak odbywa się to będzie mieć duży wpływ na wydajność, które kontrolki. Aby uzyskać więcej informacji, zobacz [wydajności ListView](~/xamarin-forms/user-interface/listview/performance.md).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optymalizuj zasoby obrazów

Zasoby obrazów do wyświetlania może znacznie zwiększyć zużycie pamięci przez aplikację. W związku z tym ich powinien zostać utworzony tylko podczas wymagane i powinny zostać opublikowane, jak aplikacja nie wymaga już je. Na przykład jeśli aplikacja wyświetla obraz, czytając dane ze strumienia, upewnij się, że strumień jest tworzony tylko wtedy, gdy jest to wymagane i upewnij się, że strumień jest zwalniany, gdy nie jest już potrzebne. Można to osiągnąć, tworząc strumienia, gdy strona jest tworzona, lub gdy [ `Page.Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) generowane zdarzenie, a następnie usuwania strumienia podczas [ `Page.Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Disappearing/) generowane zdarzenie.

Podczas pobierania obrazu do wyświetlania z [ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) metody pobrany obraz poprzez zapewnienie, że pamięć podręczna [ `UriImageSource.CachingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) właściwość jest ustawiona na `true`. Aby uzyskać więcej informacji, zobacz [Praca z obrazami](~/xamarin-forms/user-interface/images.md).

Aby uzyskać więcej informacji, zobacz [zoptymalizować zasoby obrazów](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages).

<a name="visualtree" />

## <a name="reduce-the-visual-tree-size"></a>Zmniejsz rozmiar drzewa wizualnego

Zmniejszenie liczby elementów na stronie spowoduje, że strona szybciej renderować. Istnieją dwie główne techniki służące osiągnięciu tego celu. Pierwsza to ukrycie elementów, które nie są widoczne. [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) Właściwości każdego elementu Określa, czy element powinien być częścią drzewa wizualnego, czy nie. W związku z tym, jeśli element jest niewidoczny, ponieważ jest ukryta za innymi elementami, Usuń element lub ustaw jego `IsVisible` właściwość `false`.

Druga metoda jest Usuń zbędne elementy. Na przykład, poniższy kod przedstawia układu strony, która przedstawia szereg [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) elementy:

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

Ten sam układ można zarządzać z poziomu wraz z liczbą mniejsze elementu, jak pokazano w poniższym przykładzie kodu:

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

Wszystkie zasoby, które są używane w całej aplikacji powinny być przechowywane w słowniku zasobów aplikacji, aby uniknąć jego duplikowania. Dzięki temu można zmniejszyć ilość XAML, który ma zostać przeanalizowany w całej aplikacji. Poniższy kod przedstawia przykład `HeadingLabelStyle` zasób, który jest używana aplikacja szerokie, a więc jest zdefiniowany w słowniku zasobów aplikacji:

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

Jednak XAML, które są specyficzne dla strony nie powinny być uwzględniane w słowniku zasobów aplikacji, jak zasoby następnie zostanie przetworzona przy uruchamianiu aplikacji zamiast, gdy jest to wymagane przez stronę. Jeśli zasób jest używany przez strony, który nie jest stronę startową, powinny znaleźć się w słowniku zasobów dla tej strony, w związku z tym co pomogło w zmniejszeniu XAML, który jest analizowany podczas uruchamiania aplikacji. Poniższy kod przedstawia przykład `HeadingLabelStyle` zasób, który jest tylko na jednej stronie, a więc jest zdefiniowany w słowniku zasobów strony:

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

Moduł renderowania większości klas udostępniają `OnElementChanged` metody, która jest wywoływana, gdy tworzone jest formantu niestandardowego zestawu narzędzi Xamarin.Forms w celu renderowania odpowiedni formant natywnych. Klasy niestandardowego modułu renderowania, w każdej klasie renderowania specyficzne dla platformy następnie zastąpić tę metodę w celu utworzenia wystąpienia i dostosowywanie kontrolki natywne. `SetNativeControl` Metoda jest używana do uruchomienia natywnych formantu, a ta metoda również spowoduje przypisanie odwołania kontrolki do `Control` właściwości.

Jednak w niektórych sytuacjach `OnElementChanged` metoda może być wywoływana wiele razy. W związku z tym aby zapobiec przeciekom pamięci, które mogą mieć negatywny wpływ na wydajność, należy uważać podczas tworzenia wystąpienia nowego formantu natywnych. W poniższym przykładzie kodu pokazano podejście do użycia podczas tworzenia wystąpienia nowego formantu natywne w niestandardowego modułu renderowania:

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

Nowy formant natywnych były tworzone tylko raz, gdy `Control` właściwość `null`. Kontrolki powinny być konfigurowane tylko i subskrypcję procedury obsługi zdarzeń, gdy niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Podobnie programy obsługi zdarzeń, które zostały zasubskrybowane przez powinna składać się wyłącznie usuwania subskrypcji podczas renderowania element jest dołączony do zmiany. Przyjęcie tego podejścia pomoże utworzyć wydajnego wykonywania niestandardowego modułu renderowania, nie odczuwają przecieków pamięci.

Aby uzyskać więcej informacji na temat niestandardowe programy renderujące, zobacz [Dostosowywanie formantów na każdej platformie](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule opisano i omówiono techniki zwiększenie wydajności aplikacji platformy Xamarin.Forms. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywanej przez Procesora i ilość pamięci używanej przez aplikację.


## <a name="related-links"></a>Linki pokrewne

- [Wydajności dla wielu Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Wydajność ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [Szybkie programy renderujące](~/xamarin-forms/internals/fast-renderers.md)
- [Kompresja układu](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Przykładowe zmiany rozmiaru obrazu platformy Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/XamFormsImageResize/)
- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilation/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
