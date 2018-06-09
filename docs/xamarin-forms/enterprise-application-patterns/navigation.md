---
title: Nawigacji aplikacji przedsiębiorstwa
description: W tym rozdziale opisano sposób wykonywania aplikacji mobilnej eShopOnContainers nawigacji pierwszego modelu widoku z widoku modeli.
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9ac9f3200440001752c07ad45fdaaf2b1d9ba6a5
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243684"
---
# <a name="enterprise-app-navigation"></a>Nawigacji aplikacji przedsiębiorstwa

Platformy Xamarin.Forms obsługuje nawigacji strony, co powoduje zwykle z interakcji użytkownika przy użyciu interfejsu użytkownika lub aplikacji w wyniku zmian stanu wewnętrznego logiki driven. Jednak nawigacji może być skomplikowane do wdrażania aplikacji korzystających ze wzorca Model-View-ViewModel (MVVM) w muszą być spełnione następujące problemy:

-   Jak zidentyfikować widok, aby można go znaleźć, za pomocą metody, która nie zapewnia ścisłej sprzężenia i zależności między widokami.
-   Jak do koordynowania procesu za pomocą którego widok, aby nastąpi przejście, wystąpienie zostało utworzone i zainicjowane. Korzystając z modelem MVVM, widoku oraz widoku modelu muszą być tworzone i skojarzony ze sobą za pośrednictwem widoku kontekstu powiązania. Jeśli aplikacja korzysta z kontenera iniekcji zależności, podczas tworzenia wystąpienia widoków i wyświetlanie modeli może wymagać mechanizm określonych konstrukcji.
-   Określa, czy do wykonywania nawigacji pierwszy widok lub wyświetlić nawigacji pierwszego modelu. Z nawigacji widok pierwszej strony, aby przejść do odwołuje się do nazwy typu widoku. Podczas nawigacji określony widok zostanie uruchomiony, wraz z jego odpowiedni model widoku i innych usług zależnych. Informacje o innym podejściu ma użyć nawigacji pierwszego modelu widoku, gdzie można przejść do strony odnosi się do nazwy typu modelu widoku.
-   Jak się bezpośrednio oddzielne nawigacyjne zachowanie aplikacji dla widoków i wyświetlanie modeli. Wzorzec MVVM rozdzielenie między Interfejsie użytkownika aplikacji i jej prezentacji i logiki biznesowej. Jednak nawigacji zachowanie aplikacji będzie często span interfejsu użytkownika i prezentacje części aplikacji. Użytkownik zainicjuje często nawigacji w widoku oraz widoku zostaną zastąpione w wyniku nawigacji. Jednak nawigacji może często również należy zainicjować lub koordynowane z wewnątrz model widoku.
-   Jak przekazywać parametrów podczas nawigacji na potrzeby inicjowania. Na przykład jeśli użytkownik przejdzie do widoku, aby zaktualizować szczegółów zamówienia, kolejność danych będzie musiał przekazywane do widoku, dzięki czemu można wyświetlić prawidłowe dane.
-   Jak koordynuje nawigacji, aby upewnić się, że niektóre reguły biznesowe są przestrzegane. Na przykład użytkownicy mogą monit przed wyjściem widok, aby poprawić wszelkie nieprawidłowe dane lub monit przesyłania lub Odrzuć wszystkie zmiany danych, które zostały wprowadzone w widoku.

W tym rozdziale uwzględniają te problemy, z uwzględnieniem `NavigationService` klasy, która jest używana do wykonywania widok modelu na pierwszej stronie nawigacji.

> [!NOTE]
> `NavigationService` Używany przez aplikację jest przeznaczona tylko w celu przeprowadzania hierarchicznej nawigacji między wystąpieniami wartość ContentPage. Korzystanie z usługi do przechodzenia między innymi typami strony może spowodować nieoczekiwane zachowanie.

## <a name="navigating-between-pages"></a>Przechodzenie między stronami

Logika nawigacji można znajdują się w widoku kodem lub w danych powiązany model widoku. Podczas wprowadzania logiki nawigacji w widoku może być najprostsza metoda, nie jest łatwością testować przy użyciu testów jednostkowych. Wprowadzenie do logiki nawigacji w widoku klasy modelu oznacza, że logika może być wykonywane za pomocą testów jednostkowych. Ponadto model widoku następnie można wdrożyć logikę do sterowania nawigacji, aby upewnić się, że niektóre reguły biznesowe są wymuszane. Na przykład aplikacja może nie zezwolić użytkownikowi na opuścić stronę bez uprzedniego zapewnia, że wprowadzonych danych jest nieprawidłowa.

A `NavigationService` klasy zwykle jest wywoływany z modeli widoku, aby podwyższyć poziom kontroli. Jednak przechodząc do widoków z widoku modeli wymagają wyświetlanie modeli widoków odwołania, a szczególnie widoki, które modelu widoku aktywnego nie jest skojarzona z, która nie jest zalecane. W związku z tym `NavigationService` przedstawione tutaj Określa typ modelu widoku jako element docelowy, aby przejść do.

Używa aplikacji mobilnej eShopOnContainers `NavigationService` klasy nawigację pierwszy model widoku. Ta klasa implementuje `INavigationService` interfejsu, co przedstawiono w poniższym przykładzie:

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

Ten interfejs określa, że klasa implementująca musi zapewniać następujących metod:

|Metoda|Cel|
|--- |--- |
|`InitializeAsync`|Wykonuje nawigacji do jednej z dwóch stron, gdy aplikacja jest uruchamiana.|
|`NavigateToAsync`|Wykonuje hierarchiczna nawigacji do określonej strony.|
|`NavigateToAsync(parameter)`|Wykonuje hierarchiczna nawigacji do określonej strony, przekazanie parametru.|
|`RemoveLastFromBackStackAsync`|Usuwa poprzedniej strony z stos nawigacji.|
|`RemoveBackStackAsync`|Usuwa wszystkich poprzednich stron z stos nawigacji.|

Ponadto `INavigationService` interfejsu określa, że klasa implementująca musi zapewniać `PreviousPageViewModel` właściwości. Ta właściwość zwraca typ modelu widoku skojarzone z poprzedniej strony w stosie nawigacji.

> [!NOTE]
> `INavigationService` Interfejsu zazwyczaj będzie również określić `GoBackAsync` metodę, która służy do programowego zwracania do poprzedniej strony w stosie nawigacji. Jednak ta metoda jest Brak z aplikacji mobilnej eShopOnContainers, ponieważ nie jest to wymagane.

### <a name="creating-the-navigationservice-instance"></a>Tworzenie wystąpienia NavigationService

`NavigationService` Klasy, która implementuje `INavigationService` interfejsu, jest zarejestrowany jako pojedyncze z kontenera iniekcji zależności Autofac, jak pokazano w poniższym przykładzie:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService` Interfejs zostanie rozwiązany w `ViewModelBase` konstruktora klasy, jak pokazano w poniższym przykładzie:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

To zwraca odwołanie do `NavigationService` obiekt, który jest przechowywany w Autofac kontenera iniekcji zależności, który jest tworzony przez `InitNavigation` metoda `App` klasy. Aby uzyskać więcej informacji, zobacz [nawigowanie po uruchomieniu aplikacji](#navigating_when_the_app_is_launched).

`ViewModelBase` Klasy magazynów `NavigationService` wystąpienia w `NavigationService` właściwość typu `INavigationService`. W związku z tym wyświetlić wszystkie klasy modeli, które wynikają z `ViewModelBase` klasy, można użyć `NavigationService` metody określonej przez dostępu dla właściwości `INavigationService` interfejsu. Dzięki temu można uniknąć ponoszenia dodatkowych nakładów na iniekcję `NavigationService` obiektów z kontenera iniekcji zależności Autofac do każdej klasy modelu widoku.

### <a name="handling-navigation-requests"></a>Obsługa żądania nawigacji

Udostępnia platformy Xamarin.Forms [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) klasy, która implementuje obsługi hierarchicznych nawigacji, w którym użytkownik jest powinni poruszać się po stronach przodu i do tyłu, zgodnie z potrzebami. Aby uzyskać więcej informacji na temat hierarchiczna nawigacji, zobacz [hierarchiczna nawigacji](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

Zamiast używać [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) bezpośrednio, klasa zawija aplikacji eShopOnContainers `NavigationPage` klasy w `CustomNavigationView` klasy, jak pokazano w poniższym przykładzie:

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

Celem tego zawijania jest w celu ułatwienia stylów [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wystąpienie w pliku XAML klasy.

Nawigacji jest wykonywane wewnątrz klasy modelu widoku, wywołując jedną z `NavigateToAsync` metody określenie typu model widoku dla strony jest przejście do, jak pokazano w poniższym przykładzie:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

Poniższy kod przedstawia przykład `NavigateToAsync` metody udostępniane przez `NavigationService` klasy:

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

Każda metoda umożliwia dowolnej klasy modelu widoku, która pochodzi z `ViewModelBase` klasy do wykonania hierarchiczna nawigacji, wywołując `InternalNavigateToAsync` metody. Ponadto drugi `NavigateToAsync` metody umożliwia nawigacji danych można określić jako argument, który jest przekazywany do modelu widoku użytkownik opuszcza, gdzie zwykle jest używana do wykonania inicjowania. Aby uzyskać więcej informacji, zobacz [przekazywanie parametrów podczas nawigacji](#passing_parameters_during_navigation).

`InternalNavigateToAsync` Metoda wykonuje żądania nawigacji i przedstawiono w poniższym przykładzie:

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

`InternalNavigateToAsync` Metoda przeprowadza nawigacji na model widoku przy pierwszym wywołaniu `CreatePage` metody. Ta metoda lokalizuje widok, który odpowiada typowi modelu określonego widoku i tworzy i zwraca wystąpienie klasy tego typu widoku. Lokalizowanie widok, który odpowiada typowi modelu widoku używa opartych na konwencjach podejście, przy założeniu, że:

-   Widoki są w tym samym zestawie co typów modelu widoku.
-   Widoków. Widoki podrzędna przestrzeń nazw.
-   Wyświetl modele są w. ViewModels podrzędna przestrzeń nazw.
-   Wyświetlanie nazw odpowiadają wyświetlić nazwy modelu z "Modelu" i usunięty.

Gdy widok jest uruchomiony, jest ona skojarzona z jego odpowiedni model widoku. Aby uzyskać więcej informacji o sposobie dzieje się tak, zobacz [automatyczne tworzenie modelu widoku z lokalizatorem modelu widoku](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Jeśli widok tworzona jest `LoginView`, jest on zawijany wewnątrz nowe wystąpienie klasy `CustomNavigationView` klasy i przypisane do [ `Application.Current.MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) właściwości. W przeciwnym razie `CustomNavigationView` wystąpienia jest pobierany i pod warunkiem, że nie ma wartość null, [ `PushAsync` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) metoda wywoływana w celu wypychania widoku tworzone na stosie nawigacji. Jednak jeśli pobranej `CustomNavigationView` wystąpienie jest `null`, widok tworzona jest zawijany wewnątrz nowe wystąpienie klasy `CustomNavigationView` klasy i przypisane do `Application.Current.MainPage` właściwości. Ten mechanizm zapewnia, że podczas nawigacji strony są poprawnie dodana stos nawigacji zarówno podczas jest pusty i po nim danych.

> [!TIP]
> Należy wziąć pod uwagę buforowanie stron. Strona buforowanie powoduje zmniejszenie zużycia pamięci dla widoków, które nie są obecnie wyświetlane. Jednak bez buforowania strony to oznaczać, że analizowanie zawartości XAML i konstrukcji strony i jego model widoku nastąpi każdorazowego Nowa strona jest przejście, co może mieć negatywny wpływ na wydajność złożonych strony. Dla dobrze zaprojektowanego strony, która nie używa nadmiernej liczby formantów wydajność powinna być wystarczająca. Jednak buforowanie strony mogą ułatwić Jeśli wystąpi czas ładowania strony powolne.

Po utworzeniu i przejście do widoku `InitializeAsync` metody modelu widoku skojarzonego widoku. Aby uzyskać więcej informacji, zobacz [przekazywanie parametrów podczas nawigacji](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Nawigowanie po aplikacja jest uruchamiana.

Gdy aplikacja jest uruchamiana, `InitNavigation` metoda `App` wywołaniu klasy. Poniższy przykład kodu pokazuje tej metody:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Metoda tworzy nowy `NavigationService` obiektów w kontenerze iniekcji zależności Autofac i zwraca odwołanie do niej, zanim wywoła jego `InitializeAsync` metody.

> [!NOTE]
> Gdy `INavigationService` interfejs został rozwiązany przez `ViewModelBase` klasy kontenera zwraca odwołanie do `NavigationService` obiekt, który został utworzony, gdy wywoływana jest metoda InitNavigation.

Poniższy kod przedstawia przykład `NavigationService` `InitializeAsync` metody:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

`MainView` Jest przejście, jeśli aplikacja ma token dostępu pamięci podręcznej, który jest używany do uwierzytelniania. W przeciwnym razie `LoginView` jest przejście.

Aby uzyskać więcej informacji na temat kontenera iniekcji zależności Autofac, zobacz [wprowadzenie do iniekcji zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Przekazywanie parametrów podczas nawigacji

Jeden z `NavigateToAsync` metody określonej przez `INavigationService` interfejs umożliwia nawigacji danych można określić jako argument, który jest przekazywany do modelu widoku użytkownik opuszcza, gdzie zwykle jest używana do wykonania inicjowania.

Na przykład `ProfileViewModel` klasa zawiera `OrderDetailCommand` który jest wykonywany, gdy użytkownik wybierze zamówienia na `ProfileView` strony. Z kolei wykonuje `OrderDetailAsync` metodę, która jest wyświetlana w poniższym przykładzie:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Ta metoda wywołuje nawigacji do `OrderDetailViewModel`, przechodzącą `Order` wystąpienia, która reprezentuje porządek, którą użytkownik wybrał na `ProfileView` strony. Gdy `NavigationService` tworzy klasy `OrderDetailView`, `OrderDetailViewModel` wystąpienia klasy i ma przypisaną do widoku [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Po nawigowania do `OrderDetailView`, `InternalNavigateToAsync` wykonuje metodę `InitializeAsync` metody widoku powiązanych model widoku.

`InitializeAsync` Metoda jest zdefiniowana w `ViewModelBase` klasy jako metodę, która może zostać zastąpiona. Ta metoda Określa `object` argumentu, który reprezentuje dane do przekazania do modelu widoku podczas operacji nawigacji. W związku z tym klasy modelu widoku, które chcesz odbierać dane z operacji nawigacji Podaj realizacji `InitializeAsync` metodę w celu inicjowania wymagane. Poniższy kod przedstawia przykład `InitializeAsync` metody `OrderDetailViewModel` klasy:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

Ta metoda pobiera `Order` szczegółowe informacje o wystąpienia został przekazany do modelu widoku podczas operacji nawigacji, która korzysta z niego pobrać pełną kolejności `OrderService` wystąpienia.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Wywoływanie nawigacji za pomocą zachowań

Nawigacja jest zwykle wyzwalanych z widoku interakcji z użytkownikiem. Na przykład `LoginView` wykonuje nawigacji po pomyślnym uwierzytelnieniu. Poniższy przykład kodu pokazuje sposób wywoływania nawigacji zachowanie:

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

W czasie wykonywania `EventToCommandBehavior` odpowie na interakcję z [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/). Gdy `WebView` przechodzi do strony sieci web, [ `Navigating` ](https://developer.xamarin.com/api/event/Xamarin.Forms.WebView.Navigating/) uruchomią zdarzeń, które będą wykonywane `NavigateCommand` w `LoginViewModel`. Domyślnie argumenty dla zdarzenia są przekazywane do polecenia. Te dane jest konwertowana zgodnie z przekazaniem między źródłem a celem przez konwerter określone w `EventArgsConverter` właściwość, która zwraca [ `Url` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebNavigationEventArgs.Url/) z [ `WebNavigatingEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebNavigatingEventArgs/). W związku z tym, kiedy `NavigationCommand` jest wykonywane, adres Url strony sieci web jest przekazywany jako parametr do zarejestrowaną `Action`.

Z kolei `NavigationCommand` wykonuje `NavigateAsync` metodę, która jest wyświetlana w poniższym przykładzie:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Ta metoda wywołuje nawigacji do `MainViewModel`, i po nawigacji, usuwa `LoginView` strony z stos nawigacji.

### <a name="confirming-or-cancelling-navigation"></a>Potwierdzenia lub anulowania nawigacji

Aplikacja może być konieczne do interakcji z użytkownikiem podczas operacji nawigacji, aby użytkownik może zaakceptować lub odrzucić nawigacji. Może to być konieczne, na przykład gdy użytkownik próbuje Przejdź przed upływem pełni strony wprowadzania danych. W takim przypadku aplikacji powinien podać powiadomienie, które umożliwia użytkownikowi opuścić strony lub anulować operację nawigacji, zanim nastąpi. Można to osiągnąć w klasie widoku modelu za pomocą odpowiedzi z powiadomienie do kontrolowania, czy jest wywoływany nawigacji.

## <a name="summary"></a>Podsumowanie

Platformy Xamarin.Forms obsługuje nawigacji strony, co powoduje zwykle z interakcji użytkownika przy użyciu interfejsu użytkownika lub aplikacji, w wyniku zmian stanu wewnętrznego logiki driven. Jednak nawigacji może być skomplikowane do zaimplementowania w aplikacji korzystających ze wzorca MVVM.

W tym rozdziale przedstawione `NavigationService` klasy, która jest używana do wykonywania nawigacji pierwszego modelu widoku z widoku modeli. Wprowadzenie do logiki nawigacji w widoku klasy modelu oznacza, że logika może być wykonywane za pomocą testów automatycznych. Ponadto model widoku następnie można wdrożyć logikę do sterowania nawigacji, aby upewnić się, że niektóre reguły biznesowe są wymuszane.


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
