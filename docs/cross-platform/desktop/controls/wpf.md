---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF a. Xamarin.Forms: Podobieństwa i różnice'
description: W tym dokumencie porównano i przeciwstawiono sobie WPF do zestawu narzędzi Xamarin.Forms. Omówiono w nim szablony kontrolek, XAML, infrastruktura powiązań i szablonów danych, a także elementu ItemsControl, UserControl, nawigacji i nawigację adresów URL.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 4d6585715b2fc118bb350c242abccbc68791ec0b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998521"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF a. Xamarin.Forms: Podobieństwa i różnice

## <a name="control-templates"></a>Szablony kontrolek

WPF obsługuje pojęcie *szablony kontrolek* które zawierają instrukcje wizualizacji dla formantu (`Button`, `ListBox`itp.). Jak wspomniano powyżej, Xamarin.Forms używa konkretny _renderowania_ klasy dla tego, które współdziałają z platformą natywne (iOS, Android itp.) do wizualizacji formantu.

Jednak zestaw narzędzi Xamarin.Forms _jest_ mają `ControlTemplate` typ — służy do tworzenia motywów `Page` obiektów. Zawiera definicję dla `Page` zapewnia spójne zawartość, ale pozwala na użytkownika strony, aby zmienić kolory, czcionki, itp., a nawet dodać elementy, aby była unikatowa w aplikacji.

Typowego użycia tego są rzeczy, takich jak okna dialogowe uwierzytelniania, monity i w celu zapewnienia strony standardowych, ale themable wyglądu i działania, który można dostosować w aplikacji. W ramach tej obsługi używane są wiele kontrolek znanych o nazwie WPF:

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Jednak ważne jest, aby dowiedzieć się, że są one _nie_ spełniające te same zadania w interfejsie Xamarin.Forms. Aby uzyskać więcej informacji na temat tej funkcji, zapoznaj się z [stronę z dokumentacją dotyczącą](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML jest deklaratywnym językiem znaczników stosowane do WPF i zestawu narzędzi Xamarin.Forms. W większości przypadków składnia jest taka sama — podstawowa różnica polega na obiekty, które są definiowane/tworzone przez wykresy XAML.

- Obsługuje platformy Xamarin.Forms [specyfikacji XAML 2009](/dotnet/framework/xaml-services/xaml-2009-language-features/); to sprawia, że łatwiej zdefiniować dane, takie jak `string`s, `int`s, itp. oraz jak Definiowanie typów ogólnych i przekazywanie argumentów do konstruktorów.

- Obecnie nie ma możliwości dynamicznie załadować XAML, takich jak WPF, jak za pomocą `XamlReader`. Możesz uzyskać te same funkcje podstawowe z [pakietu NuGet](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) chociaż.

### <a name="markup-extensions"></a>Rozszerzenia znaczników

Zestaw narzędzi Xamarin.Forms obsługuje rozszerzanie XAML za pośrednictwem rozszerzenia adiustacji, podobnie jak w programie WPF. Natychmiast po ma tych samych podstawowych bloków konstrukcyjnych:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

Ponadto zawiera `{x:Reference}` ze specyfikacji XAML 2009 oraz `{TemplateBinding}` rozszerzenie znaczników, który służy do wersji specjalistycznej metody `ControlTemplate` obsługiwane przez zestaw narzędzi Xamarin.Forms.

> [!WARNING]
> `ControlTemplate` Pomocy technicznej nie jest taka sama — nawet jeśli ma taką samą nazwę.

Xamarin.Forms obsługuje również — rozszerzenia znaczników niestandardowe, ale implementacja jest nieco inne. W środowisku WPF muszą pochodzić od `MarkupExtension` -abstrakcyjną klasę bazową. W interfejsie Xamarin.Forms, który jest zastępowany interfejs `IMarkupExtension` lub `IMarkupExtension<T>` co jest bardziej elastyczna.

Podobnie jak WPF, jest jednym wymaganej metody `ProvideValue` metody w celu zwrócenia wartości z poziomu rozszerzenia znaczników.

## <a name="binding-infrastructure"></a>Infrastruktura powiązań

Jednym z podstawowych pojęć przenoszone jest infrastruktura powiązań danych, nawiązać właściwości wizualnego właściwości danych .NET. Dzięki temu wzorców architektonicznych, takich jak MVVM. Podstawowy projekt jest taka sama — ma klasy podstawowej, które można powiązać [BindableObject](xref:Xamarin.Forms.BindableObject), na platformie WPF to [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) klasy. Ta klasa bazowa jest używany jako element nadrzędny głównego dla wszystkich obiektów, które będą uczestniczyć jako elementy docelowe w powiązaniu danych. Klasy pochodne następnie udostępnić [BindableProperty](xref:Xamarin.Forms.BindableProperty) obiekty, które działają jako magazyn zapasowy dla wartości właściwości (są one zdefiniowane jako [DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) obiektów na platformie WPF).

### <a name="defining-bindable-properties"></a>Definiowanie właściwości możliwej do wiązania

Definicja właściwości możliwej do wiązania w interfejsie Xamarin.Forms jest taka sama jak WPF:
1. Obiekt musi pochodzić od klasy `BindableObject`.
2. Musi to być publicznym statycznym polem typu `BindableProperty` zadeklarowana, aby zdefiniować klucz magazynu zapasowego dla właściwości.
3. Powinna istnieć otoki właściwości publiczne wystąpienia, która używa `GetValue` i `SetValue` do pobrania, a następnie zmień wartość właściwości.

Aby uzyskać kompletny przykład, zobacz [właściwości możliwe do wiązania w interfejsie Xamarin.Forms](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Dołączone właściwości

Dołączone właściwości stanowią podzbiór właściwości możliwej do wiązania i działają w taki sam sposób jak w programie WPF. Główną różnicą jest, że otoki właściwości w tym przypadku jest ommitted i zastąpione przez zestaw metod get/set statyczne klasy będącej właścicielem. Zobacz [dołączonych właściwości w interfejsie Xamarin.Forms](~/xamarin-forms/xaml/attached-properties.md) Aby uzyskać więcej informacji.

### <a name="using-the-binding-engine"></a>Przy użyciu aparatu powiązania

Proces przy użyciu aparatu powiązania jest taka sama, jak w programie WPF. Może być wykorzystywany w związanym z kodem, tworząc `Binding` obiektu powiązane z obiektem źródłowym (dowolny typ .NET) i wartość właściwości opcjonalnych (jeśli ommitted, traktuje obiekt źródłowy jako właściwość, sama — podobnie jak WPF). Następnie można użyć `SetBinding` na dowolnym `BindableObject` skojarzyć wiązania `BindableProperty`.

Alternatywnie można zdefiniować relacji powiązania w XAML przy użyciu `BindingExtension`. Ma te same podstawowe wartości rozszerzenia w WPF.

Obsługa powiązania i aparat są bardziej podobne do implementację Silverlight niż WPF. Istnieje kilka brakujących funkcji, które nie zostały zaimplementowane w interfejsie Xamarin.Forms:

- Nie jest obsługiwane w powiązania następujące funkcje:
    - BindingGroupName
    - BindsDirectlyToSource
    - IsAsync
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - Obiektu UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - ValidatesOnDataErrors
    - ValidatesOnExceptions
    - Kolekcja ValidationRules
    - XPath
    - XmlNamespaceManager
- `Binding.Mode` nie obsługuje `OneTime`, zamiast tego po prostu użyć `OneWay`.

#### <a name="relativesource"></a>RelativeSource

Nie jest obsługiwane dla `RelativeSource` powiązania. W środowisku WPF te umożliwiają powiązać inne elementy wizualne, które zostały zdefiniowane w XAML. W interfejsie Xamarin.Forms, to te same funkcje można osiągnąć za pomocą `{x:Reference}` — rozszerzenie znaczników. Na przykład przy założeniu, że mamy formantu za pomocą nazwy "otherControl" mającego właściwość Text, firma Microsoft może powiązać go następująco:

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

Ta sama funkcja może służyć do `{RelativeSource Self}` funkcji. Jednak nie jest obsługiwane do lokalizowania obiektów nadrzędnych według typu (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>Kontekst powiązania

W środowisku WPF można zdefiniować `DataContext` wartość która reprents domyślne powiązanie źródła. Jeśli nie zdefiniowano źródło powiązania, wartość tej właściwości jest używany. Wartość jest dziedziczona dół drzewa wizualnego, dzięki któremu można zdefiniować na wyższym poziomie, a następnie używane przez elementy podrzędne.

W interfejsie Xamarin.Forms, ta sama funkcja jest dostępnych, ale nazwa właściwości jest `BindingContext`.

#### <a name="value-converters"></a>Konwertery wartości

Konwertery wartości są w pełni obsługiwane w interfejsie Xamarin.Forms — podobnie jak WPF. Jest używany ten sam kształt interfejsu, ale Xamarin.Forms jest interfejsem zdefiniowanym w `Xamarin.Forms` przestrzeni nazw.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

MVVM jest w pełni obsługiwane przez WPF i zestawu narzędzi Xamarin.Forms.

WPF zawiera wbudowane w `RoutedCommand` jest czasami używane; Zestaw narzędzi Xamarin.Forms nie ma wbudowanej sterująca obsługi poza `ICommand` definicji interfejsu. Może zawierać szereg struktur MVVM, aby dodać niezbędne klas bazowych do zaimplementowania MVVM.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged i interfejsu INotifyCollectionChanged

Oba interfejsy są w pełni obsługiwane w powiązaniach zestawu narzędzi Xamarin.Forms. W przeciwieństwie do wielu platform opartych na XAML powiadomienia o zmianie właściwości można podnieść na wątkach w tle w interfejsie Xamarin.Forms (podobnie jak WPF), a aparat powiązania prawidłowo przechodzi w wątku interfejsu użytkownika.

Ponadto obsługa obu środowiskach `SynchronziationContext` i `async` / `await` celu kierowania wątku. Obejmuje WPF `Dispatcher` klasy na wszystkie elementy wizualne, Xamarin.Forms ma metodę statyczną `Device.BeginInvokeOnMainThread` którego można również (mimo że `SynchronizationContext` była preferowana dla programowania dla wielu platform).

- Obejmuje zestaw narzędzi Xamarin.Forms `ObservableCollection<T>` obsługującą powiadomienia o zmianie kolekcji.
- Możesz użyć `BindingBase.EnableCollectionSynchronization` włączającą aktualizacje międzywątkowe dla kolekcji. Interfejs API jest nieco inne niż zmiana WPF [Sprawdź dokumentację, aby uzyskać szczegóły użycia](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*).

## <a name="data-templates"></a>Szablony danych

Szablony danych są obsługiwane w interfejsie Xamarin.Forms, aby dostosować renderowania `ListView` wiersza (komórka). W przeciwieństwie do WPF, która może korzystać `DataTemplate`pod kątem dowolnej zorientowane na zawartość kontrolki zestawu narzędzi Xamarin.Forms aktualnie używa tylko ich `ListView`. Definicja szablonu może być zdefiniowano w tekście (przypisana do `ItemTemplate` właściwości), lub jako zasób w `ResourceDictionary`.

Ponadto nie są one jeszcze tak elastyczne, jak ich odpowiedników WPF.

1. Element główny `DataTemplate` musi _zawsze_ można `ViewCell` obiektu.
2. Wyzwalacze dane są w pełni obsługiwane w szablonie danych, ale mogą zawierać `DataType` właściwość wskazujący typ właściwości, wyzwalacz jest skojarzony.
3. `DataTemplateSelector` jest również obsługiwany, ale jest pochodną `DataTemplate` i w związku z tym po prostu przypisano bezpośrednio do `ItemTemplate` właściwości (a `ItemTemplateSelector` na platformie WPF).

## <a name="itemscontrol"></a>Elementu ItemsControl

Nie ma żadnych wbudowanych equivelent do `ItemsControl` w interfejsie Xamarin.Forms; ale [Klasyfikacja niestandardowa dla platformy Xamarin.Forms dostępne tutaj](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Formanty użytkownika

W środowisku WPF `UserControl`s służą do zapewniania części interfejsu użytkownika, który jest skojarzony z zachowaniem. W interfejsie Xamarin.Forms, używamy `ContentView` w tym samym celu. Obsługuje zarówno powiązanie oraz włączenia w XAML.

## <a name="navigation"></a>Nawigacja

WPF zawiera rzadko używane `NavigationService` który może służyć do zapewnienia funkcji "Przeglądarka" nawigacji. Większość aplikacji nie zostały odblokowane z tym jednak i zamiast tego używać różnych `Window` elementów lub poszczególne sekcje okno, aby wyświetlić dane.

Na urządzeniach phone różnych _ekrany_ są często rozwiązania i dlatego Xamarin.Forms obsługuje kilka postaci nawigacji:

| Styl nawigacji | Typ strony |
|--- |--- |
|Oparty na stosie (push/POP, Point)|NavigationPage|
|Wzorzec/szczegół|MasterDetailPage|
|Karty|TabbedPage|
|Po lewej stronie Przesuń palcem po prawej stronie|CarouselView|

`NavigationPage` Jest najbardziej typowym podejściem i każdej strony ma `Navigation` właściwość, która może służyć do wypychania lub pop stron włączać i wyłączać stos nawigacji. Jest to najbliższy equivelent do `NavigationService` znalezione na platformie WPF.

### <a name="url-navigation"></a>Adres URL nawigacji

WPF jest technologią zorientowane na pulpicie i może akceptować parametry wiersza polecenia, aby nakazać zachowanie podczas uruchamiania. Można użyć zestawu narzędzi Xamarin.Forms [tworzenie linku do adresu URL](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) przejść na stronę przy uruchamianiu.
