---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF vs. Cykl życia aplikacji platformy Xamarin.Forms
description: Opis procesu uruchamiania aplikacji i zajmujących się stanów tła
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b4f9aebbbcab48290d37c5732c69267897238272
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF vs. Cykl życia aplikacji platformy Xamarin.Forms

Platformy Xamarin.Forms zajmuje dużo wskazówki dotyczące projektowania z platform opartych na języku XAML, które obowiązywały przed jej szczególnie WPF. Jednak w inny sposób odbiega znacznie co może być trwałe punktu dla osób próby migrację. Ten dokument próbuje zidentyfikować niektóre z tych problemów oraz wskazówki w miarę możliwości Mostek wiedzy WPF do platformy Xamarin.Forms.

## <a name="app-lifecycle"></a>Cykl życia aplikacji

Przypomina cyklem życia aplikacji między WPF i platformy Xamarin.Forms. Zarówno uruchomić kod zewnętrzny (platform) i uruchom interfejs użytkownika za pośrednictwem wywołania metody. Różnica polega na tym, że platformy Xamarin.Forms zawsze zaczyna się w zestawie specyficzne dla platformy, który następnie inicjuje i tworzy interfejsu użytkownika dla aplikacji.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> `Main` Metoda jest domyślnie automatycznie wygenerowany i nie są widoczne w kodzie.

**Xamarin.Forms**

- **dla systemu iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>Klasa aplikacji

Zarówno WPF i platformy Xamarin.Forms `Application` klasy, który jest tworzony jako pojedynczą. W większości przypadków aplikacje będą pochodzić od tej klasy w celu zapewnienia aplikacji niestandardowych, chociaż nie jest to bezwzględnie wymagane na platformie WPF. Zarówno ujawnia `Application.Current` właściwości, aby zlokalizować utworzone pojedyncze wystąpienie.

### <a name="global-properties--persistence"></a>Właściwości globalne + trwałości

Zarówno WPF i platformy Xamarin.Forms `Application.Properties` słownika dostępne przechowywania obiektów globalnych poziomie aplikacji, które są dostępne w dowolnym miejscu w aplikacji. Najważniejsza różnica polega na tym będzie platformy Xamarin.Forms _utrwalić_ wszystkie typy pierwotne, przechowywane w kolekcji, gdy aplikacja jest wstrzymana, a następnie załadować je ponownie, gdy jest on dalszymi. WPF nie obsługuje automatycznie tego zachowania — zamiast tego większość deweloperów na izolowanych magazynów lub użyte wbudowane `Settings` obsługuje.

## <a name="defining-pages-and-the-visual-tree"></a>Definiowanie stron i drzewa wizualnego

Używa WPF `Window` jako element główny najwyższego poziomu elementów wizualnych. Definiuje HWND na świecie systemu Windows, aby wyświetlić informacje. Można tworzyć i wyświetlić tyle okien jednocześnie, jak na platformie WPF.

W przypadku platformy Xamarin.Forms, najwyższego poziomu visual zawsze jest definiowany przez platformę — na przykład w systemie iOS, jest `UIWindow`. Renderuje platformy Xamarin.Forms jest zawartości do tych reprezentacje natywnego platformy przy użyciu `Page` klasy. Każdy `Page` w platformy Xamarin.Forms reprezentuje unikatowy "page" w aplikacji, gdzie jest tylko jeden widoczne w czasie.

Zarówno WPFs `Window` i platformy Xamarin.Forms `Page` obejmują `Title` właściwość wpływ tytuł wyświetlanych, a jednocześnie ma `Icon` właściwości do wyświetlenia określonej ikony strony (**Uwaga** który Ikona i tytuł nie zawsze są widoczne w platformy Xamarin.Forms). Ponadto można zmienić wspólne właściwości visual zarówno takie jak kolor tła lub image.

Jest technicznie możliwe do renderowania dla dwóch osobnych platformy widoków (np. Zdefiniuj dwa `UIWindow` obiekty i mieć drugi jeden renderowania wyświetlanych lub funkcji AirPlay), wymaga kodu specyficzne dla platformy, aby to zrobić i nie jest bezpośrednio obsługiwana funkcja Platformy Xamarin.Forms samej siebie.

### <a name="views"></a>Widoki

Przypomina visual hierarchia elementów dla obu struktur. WPF jest nieco bardziej z powodu obsługi WYSIWYG dokumentów.

**WPF**

```
DependencyObject - base class for all bindable things
   Visual - rendering mechanics
      UIElement - common events + interactions
         FrameworkElement - adds layout
            Shape - 2D graphics
            Control - interactive controls
```

**Xamarin.Forms**

```
BindableObject - base class for all bindable things
   Element - basic parent/child support + resources + effects
      VisualElement - adds visual rendering properties (color, fonts, transforms, etc.)
         View - layout + gesture support
```

### <a name="view-lifcycle"></a>Widok Lifcycle

Platformy Xamarin.Forms przede wszystkim jest ukierunkowane wokół scenariuszach mobilnych. Jako takie aplikacje są _aktywowany_, _zawieszone_, i _ponownie uaktywnić_ jako użytkownik wchodzi w interakcję z nimi. Jest to podobne do kliknięcie od `Window` w aplikacji WPF i zestaw metod i pokrewnych zdarzeń można zastąpić lub utworzenie punktu zaczepienia w celu monitorowania tego zachowania.

| Cel | WPF — metoda | Metoda platformy Xamarin.Forms |
|--- |--- |--- |
|Początkowej aktywacji|ctor + Window.OnLoaded|ctor + Page.OnStart|
|Pokazano|Window.IsVisibleChanged|Page.Appearing|
|Hidden|Window.IsVisibleChanged|Page.Disapearing|
|Wstrzymaj utracone fokus|Window.OnDeactivated|Page.OnSleep|
|Aktywowany otrzymano fokus|Window.OnActivated|Page.OnResume|
|Zamknięte|Window.OnClosing + Window.OnClosed|n/d|


Zarówno Obsługa ukrywanie/pokazywanie formantów podrzędnych, a także na platformie WPF jest właściwością trzy stanowy `IsVisible` (widoczny, ukryty i zwinięte). W przypadku platformy Xamarin.Forms, jest tylko widoczne czy ukryte za pośrednictwem `IsVisible` właściwości.

### <a name="layout"></a>Układ

Układ strony odbywa się w tej samej 2-przebiegu (miar/Rozmieść) w WPF. Można przyłączanie się do strony układu przez zastąpienie poniższych metod w platformy Xamarin.Forms `Page` klasy:

| Metoda | Cel |
|--- |--- |
|OnChildMeasureInvalidated|Preferowany rozmiar elementu podrzędnego została zmieniona.|
|OnSizeAllocated|Strona zostanie przypisana szerokość/wysokość.|
|Zdarzenie LayoutChanged|Układ/rozmiar strony została zmieniona.|

Brak żadne zdarzenie globalne układu, nazywanego dzisiaj, ani nie istnieje globalnym `CompositionTarget.Rendering` zdarzenia, takiego jak znaleźć na platformie WPF.

#### <a name="common-layout-properties"></a>Wspólne właściwości układu

WPF i obsługuje zarówno platformy Xamarin.Forms `Margin` odstępy formantu wokół elementu i `Padding` odstępy formantu _wewnątrz_ elementu. Ponadto większość widoków układ platformy Xamarin.Forms ma właściwości, aby kontrolować odstępów (np. wiersz lub kolumnę).

Ponadto większość elementów ma właściwości, które mają wpływ na sposób znajdują się one w kontenerze nadrzędnym:

| WPF | Xamarin.Forms | Cel |
|--- |--- |--- |
|HorizontalAlignment|HorizontalOptions|Opcje lewej/Center/prawej/Stretch|
|HorizontalAlignment|VerticalOptions|Opcje TOP/Center/dół/Stretch|

> [!NOTE]
> Rzeczywiste interpretacji te właściwości zależy kontenera nadrzędnego.

#### <a name="layout-views"></a>Widoki układu

WPF i platformy Xamarin.Forms formanty układu położenie elementów podrzędnych. W większości przypadków są bardzo zbliżone pod względem funkcji.

| WPF | Xamarin.Forms | Styl układu |
|--- |--- |--- |
|StackPanel|StackLayout|Od lewej do prawej lub góry do dołu układania nieskończone|
|Siatka|Siatka|Tabelarycznej (wierszy i kolumn)|
|DockPanel|n/d|Dokowanie do krawędzi okna|
|Kanwa|AbsoluteLayout|Pozycjonowanie pikseli/współrzędnych|
|WrapPanel|n/d|Zawijanie stosu|
|n/d|RelativeLayout|Pozycjonowanie względne na podstawie reguł|

> [!NOTE]
> Nie obsługuje platformy Xamarin.Forms `GridSplitter`.

Użyj obu platform _dołączone właściwości_ do dostosowania elementy podrzędne.

### <a name="rendering"></a>Renderowanie

Znacząco różnią się mechanika renderowania WPF i platformy Xamarin.Forms. Na platformie WPF formanty, które można tworzyć bezpośrednio renderowania zawartości do pikseli na ekranie. WPF obsługuje dwa obiekt wykresy (_drzew_) do reprezentowania to - _drzewa logicznego_ reprezentuje kontrolki, zgodnie z definicją w kodzie lub XAML i _drzewa wizualnego_ reprezentuje rzeczywiste renderowania, która występuje na ekranie, który jest wykonywane bezpośrednio przez element wizualny (za pośrednictwem metody rysowania wirtualnego), lub za pośrednictwem zdefiniowane XAML `ControlTemplate` który można zastąpić lub dostosować. Zazwyczaj drzewa wizualnego jest bardziej złożonych obejmuje to takie jak obramowania wokół formantów, etykiety niejawne zawartości itp. WPF zawiera zestaw interfejsów API (`LogicalTreeHelper` i `VisualTreeHelper`) do sprawdzenia tych dwóch obiektów wykresy.

W przypadku platformy Xamarin.Forms, formanty można zdefiniować w `Page` są naprawdę zwykłych danych obiektów. One są podobne do reprezentacji drzewa logicznego, ale nigdy nie renderowania zawartości na ich własnych. Zamiast tego są one _modelu danych_ co wpływa renderowanie elementów. Rzeczywiste renderowania odbywa się [oddzielnych zbiór _wizualnego renderowania_ które są zamapowane na każdy typ formantu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Te moduły renderowania są rejestrowane w poszczególnych projektach specyficzne dla platformy przez zestawów platformy Xamarin.Forms specyficzne dla platformy. Zostanie wyświetlona lista [tutaj](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md). Oprócz zastępowania lub rozszerzanie renderującego, platformy Xamarin.Forms ma również obsługę [efekty](~/xamarin-forms/app-fundamentals/effects/index.md) umożliwia wpływ natywnego renderowania na podstawie ciągu z platformą.

#### <a name="the-logicalvisual-tree"></a>Drzewo logiczne/Visual

Nie ma żadnego dostępnego interfejsu API do przeszukania drzewa logicznego w platformy Xamarin.Forms -, ale można uzyskać informacji o tym samym odbicia. Na przykład [w tym miejscu jest metoda, która może wyliczyć logiczne elementy podrzędne](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) za pomocą odbicia.

## <a name="graphics"></a>Grafika

Platformy Xamarin.Forms nie zawiera systemu grafiki dla elementów podstawowych poza prosty prostokąt (`BoxView`). 3 bibliotek firm mogą obejmować takie jak [SkiaSharp](~/graphics-games/skiasharp/index.md) uzyskać 2D rysowania i platform, lub [UrhoSharp](~/graphics-games/urhosharp/index.md) 3D.

## <a name="resources"></a>Zasoby

WPF i platformy Xamarin.Forms mają koncepcji zasobów i słownikach zasobów. Możesz umieścić typ obiektu do `ResourceDictionary` za pomocą klucza, a następnie wyszukaj ją z `{StaticResource}` dla elementów, które nie spowoduje zmiany lub `{DynamicResource}` dla elementów, które można zmienić w słowniku, w czasie wykonywania. Użycie i mechanika są takie same, z jedną różnicą: platformy Xamarin.Forms wymaga zdefiniowania `ResourceDictionary` do przypisania do `Resources` właściwości WPF wstępnie tworzy i przypisuje go dla Ciebie.

Na przykład patrz definicja poniżej:

**WPF**

```xaml
<Window.Resources>
   <Color x:Key="redColor">#ff0000</Color>
   ...
</Window.Resources>
```

**Xamarin.Forms**

```xaml
<ContentPage.Resources>
   <ResourceDictionary>
      <Color x:Key="redColor">#ff0000</Color>
      ...
   </ResourceDictionary>
</ContentPage.Resources>
```

Jeśli nie zostanie zdefiniowana `ResourceDictionary`, generowany jest błąd czasu wykonywania.

## <a name="styles"></a>Style

Style są także w pełni obsługiwane platformy Xamarin.Forms i może być używany do motywu elementy platformy Xamarin.Forms wchodzące w skład interfejsu użytkownika. Obsługuje dziedziczenia właściwości, zdarzeń i danych, za pomocą wyzwalaczy `BasedOn`i przypadków przeszukiwania zasobów dla wartości. Style są stosowane do elementów albo jawnie za pośrednictwem `Style` właściwości lub implicitely nie dostarczając klucza zasobu — podobnie jak WPF.

### <a name="device-styles"></a>Style urządzenia

WPF ma zestaw wstępnie zdefiniowanych właściwości (takich jak przechowywane jako wartości statyczne na zestaw klas statycznych `SystemColors`) który dyktowania systemu kolory, czcionki i metryki w formie wartości i kluczy zasobów. Platformy Xamarin.Forms jest podobny, ale definiuje zestaw [style urządzenia](~/xamarin-forms/user-interface/styles/device.md) do reprezentowania te same elementy. Te style są dostarczane przez frameowrk i wartości oparte na środowisko uruchomieniowe (np. dostępność).

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
