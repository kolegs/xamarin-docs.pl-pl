---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF vs. Platformy Xamarin.Forms: Podobieństwa & różnice'
ms.technology: xamarin-crossplatform
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: e72297963b6be50572e4bb539263065592737c42
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF vs. Platformy Xamarin.Forms: Podobieństwa & różnice

## <a name="control-templates"></a>Szablony formantu

WPF obsługuje pojęcie *szablonów kontrolki* zapewniające instrukcje wizualizacji kontrolki (`Button`, `ListBox`itp.). Jak wspomniano powyżej, platformy Xamarin.Forms używa konkretnych _renderowania_ dla tej klasy, które współdziałają z platformą natywnego (z systemem iOS, Android, itp.) w celu wizualizacji formantu.

Jednak platformy Xamarin.Forms _jest_ ma `ControlTemplate` typ — służy do tworzenia motywów `Page` obiektów. Zawiera definicję dla `Page` zapewnia spójne zawartości, ale umożliwia użytkownikowi strony zmienić kolory, czcionki, itp., a nawet dodać elementy do zapewnienia unikatowości do aplikacji.

Typowe zastosowania to są elementy, takie jak uwierzytelnianie okien dialogowych, monity i zapewnienie strony standardowych, ale themable wyglądu i działania, który można dostosować w aplikacji. W ramach tej obsługi używane są wielu znanych formantów WPF o nazwie:

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Jednak ważne jest, aby dowiedzieć się, że są one _nie_ pełnią taką samą funkcję platformy Xamarin.Forms. Aby uzyskać więcej informacji na temat tej funkcji, zapoznaj się z [stronę dokumentacji](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML jest używany jako język znaczników deklaratywne WPF i platformy Xamarin.Forms. W większości przypadków składnia jest identyczne — podstawowa różnica polega na obiekty, które są zdefiniowane/utworzyć na wykresach XAML.

- Obsługuje platformy Xamarin.Forms [specyfikacji języka XAML 2009](/dotnet/framework/xaml-services/xaml-2009-language-features/); ułatwia określenie danych, takich jak `string`s, `int`s, itp. oraz jak Definiowanie typów ogólnych i przekazywanie argumentów konstruktorów.

- Nie istnieje obecnie sposób załadować dyanmically XAML jak WPF z `XamlReader`. Możesz uzyskać te same funkcje podstawowe z [pakietu NuGet](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) chociaż.

### <a name="markup-extensions"></a>Rozszerzenia znaczników

Platformy Xamarin.Forms obsługuje rozszerzanie za pośrednictwem rozszerzenia znaczników, podobnie jak WPF XAML. Fabrycznej składa się z tym samym podstawowe bloki konstrukcyjne:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

Ponadto zawiera `{x:Reference}` ze specyfikacji języka XAML 2009 i `{TemplateBinding}` rozszerzenie znaczników, który służy do specjalna wersja `ControlTemplate` obsługiwana przez platformy Xamarin.Forms.

> [!WARNING]
> `ControlTemplate` Obsługi nie jest tym samym — nawet jeśli ma taką samą nazwę.

Platformy Xamarin.Forms obsługuje również — rozszerzenia znaczników niestandardowe, ale implementacja jest nieco inne. Na platformie WPF, musi pochodzić od `MarkupExtension` -abstrakcyjną klasę podstawową. W platformy Xamarin.Forms, który zostanie zastąpiony interfejs `IMarkupExtension` lub `IMarkupExtension<T>` który jest bardziej elastyczne.

Podobnie jak WPF, jest jednym wymaganej metody `ProvideValue` metoda zwraca wartość z rozszerzeniem znacznika.

## <a name="binding-infrastructure"></a>Infrastruktura powiązań

Jeden z podstawowych pojęciach przenoszone jest infrastruktury powiązania danych nawiązać właściwości wizualnego właściwości danych .NET. Dzięki temu architektury wzorców, takich jak MVVM. Podstawowy projekt jest identyczne — ma klasę podstawową można powiązać [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), na platformie WPF jest [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) klasy. Ta klasa podstawowa jest używany jako element nadrzędny głównego dla wszystkich obiektów, które będą uczestniczyć jako miejsca docelowe w powiązaniu danych. Klasy pochodne następnie uwidaczniaj [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) obiektów, które działają jako magazynu zapasowego dla wartości właściwości (są one zdefiniowane jako [DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) obiektów na platformie WPF).

### <a name="defining-bindable-properties"></a>Definiowanie właściwości

Definicja właściwości możliwej do wiązania w platformy Xamarin.Forms jest taka sama jak WPF:
1. Obiekt musi pochodzić od `BindableObject`.
2. Musi być publicznym statycznym polem typu `BindableProperty` zadeklarowana, aby zdefiniować klucz magazynu zapasowego dla właściwości.
3. Powinien być otoki właściwości wystąpienia publicznego, który używa `GetValue` i `SetValue` pobrać i zmień wartość właściwości.

Pełny przykład, zobacz [można powiązać właściwości platformy Xamarin.Forms](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Dołączone właściwości

Dołączone właściwości są podzbiorem właściwości możliwej do wiązania i działają w taki sam sposób jak w WPF. Główną różnicą jest otoki właściwości jest w tym przypadku jego pominięcia oraz zastąpiony zestaw metod statycznych pobierania/ustawiania klasa będąca właścicielem. Zobacz [dołączonych właściwości platformy Xamarin.Forms](~/xamarin-forms/xaml/attached-properties.md) Aby uzyskać więcej informacji.

### <a name="using-the-binding-engine"></a>Przy użyciu aparatu powiązania

Proces użycia aparat wiązania jest taka sama, jak w WPF. Mogą zostać użyte w CodeBehind przez tworzenie `Binding` obiektu powiązana z obiektem źródłowym (dowolny typ .NET) i opcjonalna wartość właściwości (jeśli jego pominięcia traktuje obiektu źródłowego jako właściwość — podobnie jak WPF). Następnie można użyć `SetBinding` na dowolnym `BindableObject` do skojarzenia powiązania `BindableProperty`.

Alternatywnie można zdefiniować relacji powiązania za pomocą XAML `BindingExtension`. Ma takie same wartości podstawowych jako rozszerzenie na platformie WPF.

Obsługa powiązania i aparatu bardziej przypominają implementacji Silverlight niż WPF. Istnieje kilka Brak funkcji, które nie zostały wdrożone w platformy Xamarin.Forms:

- Nie jest obsługiwane w powiązaniach następujące funkcje:
    - BindingGroupName
    - BindsDirectlyToSource
    - IsAsync
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - ValidatesOnDataErrors
    - ValidatesOnExceptions
    - Kolekcja ValidationRules
    - XPath
    - XmlNamespaceManager
- `Binding.Mode` nie obsługuje `OneTime`, zamiast tego użyć `OneWay`.

#### <a name="relativesource"></a>RelativeSource

Nie jest obsługiwane dla `RelativeSource` powiązania. Na platformie WPF te umożliwiają powiązać inne elementy wizualne zdefiniowany w języku XAML. W przypadku platformy Xamarin.Forms, ta sama funkcja można uzyskać za pomocą `{x:Reference}` — rozszerzenie znaczników. Na przykład przy założeniu, że mamy formantu o nazwę "otherControl" mającej właściwość Text, firma Microsoft można powiązać go następująco:

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

Na platformie WPF, można zdefiniować `DataContext` wartość właściwości które reprents domyślne powiązania źródła. Jeśli nie zdefiniowano dla powiązania źródła, jest używana wartość tej właściwości. Wartość jest dziedziczona w dół drzewa wizualnego, dzięki któremu można zdefiniować na wyższym poziomie, a następnie używane przez elementy podrzędne.

W przypadku platformy Xamarin.Forms, ta sama funkcja jest avaialable, ale nazwa właściwości jest `BindingContext`.

#### <a name="value-converters"></a>Konwertery wartości

Konwertery wartości są w pełni obsługiwane w platformy Xamarin.Forms — podobnie jak WPF. Jest używany ten sam interfejs kształt, ale platformy Xamarin.Forms ma zdefiniowany w interfejsie `Xamarin.Forms` przestrzeni nazw.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

MVVM jest całkowicie obsługiwana zarówno przez WPF i platformy Xamarin.Forms.

WPF zawiera wbudowane w `RoutedCommand` jest czasami używana; Platformy Xamarin.Forms ma bez wbudowanej obsługi sterująca poza `ICommand` definicji interfejsu. Mogą zawierać różne RAM MVVM, aby dodać niezbędne klasy podstawowej implementacji MVVM.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged i interfejsu INotifyCollectionChanged

Oba interfejsy są w pełni obsługiwane w powiązaniach platformy Xamarin.Forms. W przeciwieństwie do wielu platform opartych na języku XAML powiadomienia o zmianie właściwości można wywoływane na wątki w tle w platformy Xamarin.Forms (podobnie jak WPF) i aparat wiązania prawidłowo spowoduje przejście do wątku interfejsu użytkownika.

Ponadto program obsługuje obu środowiskach `SynchronziationContext` i `async` / `await` celu kierowania prawidłowego wątku. Obejmuje WPF `Dispatcher` klasy na wszystkie elementy wizualne, platformy Xamarin.Forms ma statycznej metody `Device.BeginInvokeOnMainThread` również umożliwia (chociaż `SynchronizationContext` jest preferowana kodowania i platform).

- Obejmuje platformy Xamarin.Forms `ObservableCollection<T>` który obsługuje powiadomienia o zmianie kolekcji.
- Można użyć `BindingBase.EnableCollectionSynchronization` zezwalając na aktualizowanie innych wątków dla kolekcji. Interfejs API jest nieco inne niż odmiany WPF [Sprawdź docs szczegóły użycia](https://developer.xamarin.com/api/member/Xamarin.Forms.BindingBase.EnableCollectionSynchronization/).

## <a name="data-templates"></a>Szablony danych

Szablony danych są obsługiwane w platformy Xamarin.Forms, aby dostosować renderowanie `ListView` wiersza (komórki). W przeciwieństwie do WPF, które mogą wykorzystywać `DataTemplate`je na używa tylko s dla dowolnego zorientowane na zawartość formantu, w obecnie platformy Xamarin.Forms `ListView`. Definicja szablonu może być zdefiniowano w tekście (przypisana do `ItemTemplate` właściwości), lub jako zasób w `ResourceDictionary`.

Ponadto nie są jeszcze tak elastyczne, jak ich odpowiednika WPF.

1. Element główny `DataTemplate` musi _zawsze_ można `ViewCell` obiektu.
2. Wyzwalacze danych są w pełni obsługiwane w szablonie danych, ale musi zawierać `DataType` Właściwość wskazująca typ właściwości skojarzonego z wyzwalacza.
3. `DataTemplateSelector` jest również obsługiwany, ale jest pochodną `DataTemplate` , dlatego właśnie przypisano bezpośrednio do `ItemTemplate` właściwości (a `ItemTemplateSelector` na platformie WPF).

## <a name="itemscontrol"></a>ItemsControl

Nie istnieje żadne wbudowane equivelent do `ItemsControl` w platformy Xamarin.Forms; ale [jedno niestandardowe dla platformy Xamarin.Forms dostępne tutaj](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Formanty użytkownika

Na platformie WPF `UserControl`s służą do zapewniania część interfejsu użytkownika, w którym są skojarzone zachowanie. W przypadku platformy Xamarin.Forms, używamy `ContentView` do takiej obsługi. Obsługuje zarówno powiązania i dołączenia w języku XAML.

## <a name="navigation"></a>Nawigacji

WPF zawiera rzadko używane `NavigationService` który może służyć do zapewnienia funkcji "przeglądarki" nawigacji. Większość aplikacji nie zostało odblokowane z tym jednak i zamiast tego użyć różnych `Window` elementów lub różne sekcje okna, aby wyświetlić dane.

Na urządzeniach phone różnych _ekrany_ są często rozwiązania i dlatego platformy Xamarin.Forms obsługuje kilka metod nawigacji:

| Styl nawigacji | Typ strony |
|--- |--- |
|Na podstawie stosu (push/pop)|NavigationPage|
|Wzorzec/szczegół|MasterDetailPage|
|Karty|TabbedPage|
|Przejdź prawej/lewej strony|CarouselView|

`NavigationPage` Jest najczęściej i każdej strony ma `Navigation` właściwość, która może służyć do wypychania lub Powiększ stron włączać i wyłączać stos nawigacji. Jest to najbliższy equivelent do `NavigationService` znaleziono na platformie WPF.

### <a name="url-navigation"></a>Adres URL nawigacji

WPF to zorientowane na pulpicie technologia która może zaakceptować parametry wiersza polecenia, aby kierować zachowanie podczas uruchamiania. Można używać platformy Xamarin.Forms [łączenie adres URL głębokiego](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) aby przejść na stronę przy uruchamianiu.
