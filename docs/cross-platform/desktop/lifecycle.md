---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF a. Cykl życia aplikacji platformy Xamarin.Forms
description: W tym dokumencie porównuje podobieństwa i różnice między cyklem życia aplikacji dla aplikacji platformy Xamarin.Forms i WPF. Ponadto monitoruje drzewa wizualnego, grafiki, zasobów i stylów.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: a59f257d1e6285fa2d899271a1aae9778b04d985
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780614"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF a. Cykl życia aplikacji platformy Xamarin.Forms

Zestaw narzędzi Xamarin.Forms zajmuje dużo wskazówki dotyczące projektowania z platform opartych na XAML, dołączone przed nią szczególnie WPF. Jednak w inny sposób odbiega znacznie który może być umocowany punktu dla osób próby migrację. W tym dokumencie próbuje zidentyfikować, niektóre z tych problemów i wytyczne możliwe do mostka wiedzy WPF do zestawu narzędzi Xamarin.Forms.

## <a name="app-lifecycle"></a>Cykl życia aplikacji

Cykl życia aplikacji między WPF a Xamarin.Forms jest podobny. Uruchom kod zewnętrzny (platformy) i uruchomić interfejs użytkownika za pomocą wywołania metody. Różnica polega na tym, że zestawu narzędzi Xamarin.Forms zawsze zaczyna się w zestawie specyficzne dla platformy, który następnie inicjuje i tworzy interfejsu użytkownika dla aplikacji.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> `Main` Metoda jest domyślnie automatycznie generowane i nie są widoczne w kodzie.

**Xamarin.Forms**

- **dla systemu iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **System android** &ndash; `MainActivity > App > ContentPage`
- **PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>Klasa aplikacji

WPF i zestawu narzędzi Xamarin.Forms mają `Application` klasy, która jest tworzona jako pojedynczą. W większości przypadków aplikacji będzie pochodzić od tej klasy, aby zapewnić aplikacji niestandardowych, mimo że to nie jest ściśle wymagany na platformie WPF. Zarówno ujawnić `Application.Current` właściwość, aby zlokalizować utworzone pojedyncze wystąpienie.

### <a name="global-properties--persistence"></a>Właściwości globalne + trwałości

WPF i zestawu narzędzi Xamarin.Forms mają `Application.Properties` słownika dostępne przechowywania obiektów globalnych poziomie aplikacji, które są dostępne w dowolnym miejscu w aplikacji. Główną różnicą jest tego zestawu narzędzi Xamarin.Forms _utrwalić_ żadnych typów pierwotnych, przechowywane w kolekcji, gdy aplikacja jest wstrzymana, a ich ładować ponownie, gdy jest on dalszymi. WPF nie obsługuje automatycznie tego zachowania — zamiast tego większość programistów opiera się na wydzielonej pamięci masowej lub wykorzystywane wbudowane `Settings` pomocy technicznej.

## <a name="defining-pages-and-the-visual-tree"></a>Definiowanie stron i drzewo wizualne

Używa WPF `Window` jako element główny dla dowolnego elementu wizualnego najwyższego poziomu. Definiuje właściwości HWND na świecie Windows, aby wyświetlić informacje. Można tworzyć i wyświetlić liczbę okna równocześnie, jak na platformie WPF.

W interfejsie Xamarin.Forms, najwyższego poziomu wizualnego, zawsze jest definiowany przez platformę — na przykład w systemie iOS, jest `UIWindow`. Renderuje zestaw narzędzi Xamarin.Forms jest zawartość w te reprezentacje platformy natywnej przy użyciu `Page` klasy. Każdy `Page` w interfejsie Xamarin.Forms reprezentuje unikatowy "page" w aplikacji, gdzie jest tylko jeden widoczne w danym momencie.

Zarówno WPFs `Window` i Xamarin.Forms `Page` obejmują `Title` właściwość wpływania na wyświetlonej tytuł i jednocześnie mieć `Icon` właściwość, aby wyświetlić określone ikony strony (**Uwaga** , Tytuł i ikona nie zawsze są widoczne w interfejsie Xamarin.Forms). Ponadto można zmienić wspólne właściwości visual zarówno takich jak kolor tła lub obrazu.

Jest technicznie możliwa do renderowania dla dwóch widoków oddzielne platformy (np. zdefiniować dwa `UIWindow` obiektów i mają jedną sekundę renderowania do ekranu zewnętrznego lub AirPlay), wymaga kodu specyficznego dla platformy, aby to zrobić i nie jest bezpośrednio obsługiwana funkcja Zestaw narzędzi Xamarin.Forms, sam.

### <a name="views"></a>Widoki

Przypomina visual hierarchia elementów dla obu platform. WPF jest nieco bardziej ze względu na obsługę dokumentów WYSIWYG.

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

### <a name="view-lifecycle"></a>Cykl życia widoku

Xamarin.Forms to przede wszystkim zorientowanej na wokół mobilnych. Jako takie aplikacje są _aktywowane_, _zawieszone_, i _ponownie uaktywnione_ jako użytkownik wchodzi w interakcję z nimi. Jest to podobne do klikając opuszczenie `Window` w aplikacji WPF wiąże się z zestawu metod i pokrewnych zdarzeń można zastąpić lub dołączyć do monitorowania tego zachowania.

| Cel | Metoda WPF | Metoda zestawu narzędzi Xamarin.Forms |
|--- |--- |--- |
|Początkowej aktywacji|ctor + Window.OnLoaded|ctor + Page.OnStart|
|Wyświetlane|Window.IsVisibleChanged|Page.Appearing|
|Hidden|Window.IsVisibleChanged|Page.Disapearing|
|Wstrzymywanie/utracone fokus|Window.OnDeactivated|Page.OnSleep|
|Aktywowany otrzymano fokus|Window.OnActivated|Page.OnResume|
|Zamknięte|Window.OnClosing + Window.OnClosed|n/d|


Zarówno Obsługa ukrywanie/pokazywanie także formantów podrzędnych w WPF jest właściwością-stanowy `IsVisible` (widoczne, ukryte i zwinięty). W interfejsie Xamarin.Forms, jest po prostu widoczne czy ukryte za pośrednictwem `IsVisible` właściwości.

### <a name="layout"></a>Układ

Układ strony występuje w tym samym 2-przebiegu (miara/Rozmieść) w WPF. Można dołączyć do układu strony poprzez zastąpienie następujące metody w interfejsie Xamarin.Forms `Page` klasy:

| Metoda | Cel |
|--- |--- |
|OnChildMeasureInvalidated|Preferowany rozmiar element podrzędny został zmieniony.|
|OnSizeAllocated|Strona przypisano szerokość/wysokość.|
|Zdarzenie LayoutChanged|Układ/rozmiar strony został zmieniony.|

Istnieje żadne zdarzenie globalnego układ, który nazywa się już dziś, podobnie jak ma globalną `CompositionTarget.Rendering` zdarzenie, takie jak znaleźć się na platformie WPF.

#### <a name="common-layout-properties"></a>Wspólne właściwości układu

WPF a Xamarin.Forms obsługuje zarówno `Margin` do sterowania odstęp wokół elementu i `Padding` odstępy kontroli _wewnątrz_ elementu. Ponadto większość widoków układów Xamarin.Forms mają właściwości, aby kontrolować odstępy (np. wiersz lub kolumnę).

Ponadto większość elementów ma właściwości, aby mieć wpływ na sposób znajdują się one w kontenerze nadrzędnym:

| WPF | Xamarin.Forms | Cel |
|--- |--- |--- |
|HorizontalAlignment|HorizontalOptions|Opcje po lewej stronie/Center/po prawej stronie/Stretch|
|VerticalAlignment|VerticalOptions|Opcji TOP/Center/dół/Stretch|

> [!NOTE]
> Rzeczywiste interpretacji tych właściwości jest zależna od nadrzędnego kontenera.

#### <a name="layout-views"></a>Widoki układów

WPF a Xamarin.Forms umożliwia układu kontrolek pozycjonować elementy podrzędne. W większości przypadków są bardzo blisko siebie pod względem funkcji.

| WPF | Xamarin.Forms | Styl układu |
|--- |--- |--- |
|StackPanel|StackLayout|Od lewej do prawej lub od góry do dołu układania nieskończona|
|Siatka|Siatka|Format tabeli (wierszy i kolumn)|
|DockPanel|n/d|Dokuj do krawędzi okna|
|Kanwa|AbsoluteLayout|Piksel/współrzędne pozycjonowania|
|WrapPanel|n/d|Zawijanie stosu|
|n/d|RelativeLayout|Pozycjonowanie oparty na regułach względne|

> [!NOTE]
> Nie obsługuje zestawu narzędzi Xamarin.Forms `GridSplitter`.

Użyj obu platform _dołączonych właściwości_ Aby precyzyjnie dostosować elementy podrzędne.

### <a name="rendering"></a>Renderowanie

Mechanika renderowania dla platformy WPF i zestawu narzędzi Xamarin.Forms różnią się znacznie. W środowisku WPF formanty, które bezpośrednio tworzyć renderowania zawartości w pikselach na ekranie. WPF obsługuje obiektu dwa wykresy (_drzew_) do reprezentowania to - _drzewo logiczne_ reprezentuje kontrolki, zgodnie z definicją w kodzie lub XAML oraz _drzewa wizualnego_ reprezentuje rzeczywiste renderowania, występujący na ekranie, który jest wykonywane bezpośrednio przez elementu wizualnego (za pośrednictwem metody wirtualnej draw), lub za pośrednictwem zdefiniowane XAML `ControlTemplate` które zastąpione lub dostosować. Zazwyczaj drzewo wizualne jest bardziej złożone, ponieważ ma ona to przykład obramowania kontrolki, etykiety niejawne zawartości itp. WPF zawiera zestaw interfejsów API (`LogicalTreeHelper` i `VisualTreeHelper`) do zbadania tych dwóch obiektów wykresów.

W interfejsie Xamarin.Forms, formanty zdefiniowane w `Page` są tylko proste dane obiektów. Oni są podobne do reprezentacji drzewo logiczne, ale nigdy nie renderowania zawartości własnych. Zamiast tego są one _modelu danych_ co wpływa renderowanie elementów. Rzeczywiste renderowania odbywa się [oddzielić zbiorem _visual programy renderujące_ są mapowane każdego typu formantu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Te programy renderujące są rejestrowane we wszystkich projektach specyficzne dla platformy dzięki specyficzne dla platformy Xamarin.Forms zestawów. Wyświetlana jest lista [tutaj](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md). Oprócz zastępowanie i rozszerzanie modułu renderowania, Xamarin.Forms zapewnia również obsługę [efekty](~/xamarin-forms/app-fundamentals/effects/index.md) który może służyć do wywierania wpływu na natywny renderowania na podstawie poszczególnych z platformą.

#### <a name="the-logicalvisual-tree"></a>Drzewo logiczne/Visual

Nie ma żadnych narażonych interfejsów API, aby zapoznać się z drzewa logicznego w interfejsie Xamarin.Forms — ale odbicia można użyć, aby uzyskać te same informacje. Na przykład [poniżej przedstawiono metody, która może wyliczyć podrzędnych logicznego](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) za pomocą odbicia.

## <a name="graphics"></a>Grafika

Zestaw narzędzi Xamarin.Forms nie obejmuje system graficzny dla elementów podstawowych poza prosty prostokąt (`BoxView`). Może zawierać 3 bibliotek innych firm, takich jak [SkiaSharp](~/graphics-games/skiasharp/index.md) można pobrać rysowania 2D dla wielu platform, lub [UrhoSharp](~/graphics-games/urhosharp/index.md) 3D.

## <a name="resources"></a>Resources

WPF a Xamarin.Forms mają koncepcji zasobów i słowniki zasobów. Możesz umieścić dowolny typ obiektu do `ResourceDictionary` za pomocą klucza i wyszukaj ją za pomocą `{StaticResource}` w przypadku elementów, które nie ulegną zmianie, lub `{DynamicResource}` w przypadku elementów, które można zmienić w słowniku, w czasie wykonywania. Użycie i mechanikę są takie same, z jedną różnicą: Xamarin.Forms wymaga zdefiniowania `ResourceDictionary` można przypisać do `Resources` właściwość WPF wstępnie tworzony jest jeden, a następnie przypisuje go dla Ciebie.

Na przykład zobacz definicję poniżej:

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

Jeżeli nie zdefiniujesz `ResourceDictionary`, zostanie wygenerowany błąd środowiska uruchomieniowego.

## <a name="styles"></a>Style

Style są również w pełni obsługiwane w interfejsie Xamarin.Forms i mogą być używane do motywu elementy zestawu narzędzi Xamarin.Forms, które tworzą interfejs użytkownika. Obsługują one wyzwalacze właściwości, zdarzeń i danych, dziedziczenie za pośrednictwem `BasedOn`i wyszukiwania zasobów dla wartości. Style są stosowane do elementów albo jawnie za pomocą `Style` właściwości lub implicitely przez nie podano klucza zasobu — podobnie jak WPF.

### <a name="device-styles"></a>Style urządzenia

WPF zawiera zestaw wstępnie zdefiniowanych właściwości (przechowywane jako wartości statyczne na zestaw klas statycznych, takich jak `SystemColors`) który dyktowanie hostującego te role kolory, czcionki i metryki w formie wartości i kluczy zasobu. Zestaw narzędzi Xamarin.Forms jest podobny, ale definiuje zestaw [style urządzenia](~/xamarin-forms/user-interface/styles/device.md) do reprezentowania tych samych czynności. Te style są dostarczane przez frameowrk i ustaw wartości w oparciu o środowisko uruchomieniowe (np. dostępność).

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
