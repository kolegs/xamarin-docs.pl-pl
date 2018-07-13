---
title: Nawigacja aplikacji przedsiębiorstwa
description: W tym rozdziale wyjaśniono, jak aplikacja mobilna w ramach aplikacji eShopOnContainers wykonuje widoku pierwszy model nawigacyjnych z modeli widoków.
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d306b0c1c0d08129671e27b96911ec771acb658e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994773"
---
# <a name="enterprise-app-navigation"></a>Nawigacja aplikacji przedsiębiorstwa

Zestaw narzędzi Xamarin.Forms obsługuje nawigowania po stronach, co zwykle powoduje z interakcji użytkownika z interfejsu użytkownika lub aplikacji w wyniku zmiany stanu na podstawie logiki wewnętrznej. Jednak nawigacji może być złożone, aby zaimplementować w aplikacji, które używają wzorzec Model-View-ViewModel (MVVM), jako muszą być spełnione następujące problemy:

-   Jak zidentyfikować widok, aby można go znaleźć, przy użyciu metody, która nie wprowadza ścisłego sprzężenia i zależności między widokami.
-   Jak do koordynowania procesu za pomocą którego widoku na potrzeby nastąpi przejście, jest tworzone i inicjowane. Korzystając z modelem MVVM, widoku i modelu widoku muszą być tworzone i skojarzony ze sobą za pośrednictwem widoku kontekstu powiązania. Jeśli aplikacja korzysta z kontenera iniekcji zależności, wystąpienia widoki i modele widok może wymagać mechanizm określonych konstrukcji.
-   Określa, czy wykonać pierwszy widok nawigacji lub wyświetlić pierwszy model nawigacyjnych. Z nawigacją pierwszy widok strony aby przejść do odnosi się do nazwy typu widoku. Podczas nawigacji, a określony widok jest tworzone wraz z jego odpowiedniego modelu widoku i inne usługi zależne. Alternatywnym podejściem jest korzystanie z nawigacji modelu widoku, gdzie strony aby przejść do odnosi się do nazwy typu modelu widoku.
-   Jak: Aby prawidłowo zamykaj wystąpienia można oddzielić widoki i modele widoku nawigacyjne zachowanie aplikacji. Wzorzec MVVM rozdzielenie między Interfejsie użytkownika aplikacji i jej prezentacji i logiki biznesowej. Jednak zachowanie nawigacji w aplikacji często zostanie span części interfejsu użytkownika i prezentacji aplikacji. Użytkownik zainicjuje często nawigacji w widoku oraz widoku zostaną zastąpione w wyniku nawigacji. Jednak nawigacji często również konieczne może być zainicjowane lub koordynowany z w ramach modelu widoku.
-   Jak przekazać parametry podczas nawigowania do celów inicjalizacji. Na przykład jeśli użytkownik przejdzie do widoku, aby zaktualizować szczegóły zamówienia, dane kolejności będą mieć do przekazania do widoku, aby go wyświetlić prawidłowe dane.
-   Jak podjąć nawigacji, aby upewnić się, że niektóre reguły biznesowe są przestrzegane. Na przykład użytkownicy mogą monit przed przechodzenia poza widokiem, dzięki czemu można rozwiązać wszelkie nieprawidłowe dane, lub monit Prześlij lub Odrzuć wszystkie zmiany danych, które zostały wprowadzone w widoku.

W tym rozdziale uwzględniają te problemy, prezentując `NavigationService` klasę, która jest używana do wykonywania nawigowania po stronach pierwszy model widoku.

> [!NOTE]
> `NavigationService` Posługują się aplikacja jest przeznaczona tylko do wykonywania Nawigacja hierarchiczna między wystąpieniami ContentPage. Aby poruszać się między innymi typy stron przy użyciu usługi może spowodować nieoczekiwane zachowanie.

## <a name="navigating-between-pages"></a>Przechodzenie między stronami

Logika nawigacji można znajdują się w widoku związane z kodem lub w danych powiązane model widoku. Podczas wprowadzania do logiki nawigacji w widoku może być najprostsza metoda, nie jest łatwością testować przy użyciu testów jednostkowych. Wprowadzenie do logiki nawigacji w widoku klasy modelu oznacza, że logiki może być wykonywana za pomocą testów jednostkowych. Ponadto model widoku można zaimplementować logikę do kontroli nawigacji, aby upewnić się, że niektóre reguły biznesowe są wymuszane. Na przykład aplikacja może nie Zezwalaj użytkownikowi na opuścić stronę bez uprzedniego zapewnia, że wprowadzone dane jest nieprawidłowy.

Element `NavigationService` klasy zwykle jest wywoływana z modeli widoków, do wspierania testowania. Jednakże przechodząc do widoków z modeli widoków wymagałoby modeli widoków do widoków odwołania i szczególnie te widoki, które modelu widoku aktywnego nie jest skojarzona z, który nie jest zalecane. W związku z tym `NavigationService` przedstawione w tym miejscu określa typ modelu widoku jako element docelowy, aby przejść do.

Zastosowań aplikacji mobilnej w ramach aplikacji eShopOnContainers `NavigationService` klasy w celu umożliwienia nawigacji modelu widoku. Ta klasa implementuje `INavigationService` interfejsu, który jest pokazany w poniższym przykładzie kodu:

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

Ten interfejs określa, że klasa implementująca należy podać następujące metody:

|Metoda|Cel|
|--- |--- |
|`InitializeAsync`|Wykonuje nawigacji do jednej z dwóch stronach, gdy aplikacja jest uruchamiana.|
|`NavigateToAsync`|Wykonuje Nawigacja hierarchiczna do określonej strony.|
|`NavigateToAsync(parameter)`|Wykonuje Nawigacja hierarchiczna do określonej strony przekazanie parametru.|
|`RemoveLastFromBackStackAsync`|Usuwa poprzedniej strony z stos nawigacji.|
|`RemoveBackStackAsync`|Usuwa wszystkie poprzednie strony z stos nawigacji.|

Ponadto `INavigationService` interfejs określa, że klasa implementująca musi zapewnić `PreviousPageViewModel` właściwości. Ta właściwość zwraca typ modelu widoku, które są skojarzone z poprzedniej strony w stosie nawigacji.

> [!NOTE]
> `INavigationService` Interfejsu zwykle także określić `GoBackAsync` metody, która jest używana do programowego powrócić do poprzedniej strony w stosie nawigacji. Jednak ta metoda jest Brak z aplikacji mobilnej w ramach aplikacji eShopOnContainers, ponieważ nie jest to wymagane.

### <a name="creating-the-navigationservice-instance"></a>Tworzenie wystąpienia NavigationService

`NavigationService` Klasy, która implementuje `INavigationService` interfejsu, jest zarejestrowany jako pojedyncze z kontenera iniekcji zależności Autofac, jak pokazano w poniższym przykładzie kodu:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService` Interfejs został rozwiązany w `ViewModelBase` konstruktora klasy, jak pokazano w poniższym przykładzie kodu:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

To zwraca odwołanie do `NavigationService` obiekt, który jest przechowywany w Autofac kontenera iniekcji zależności, która została utworzona przy `InitNavigation` method in Class metoda `App` klasy. Aby uzyskać więcej informacji, zobacz [nawigowanie po uruchomieniu aplikacji](#navigating_when_the_app_is_launched).

`ViewModelBase` Klasa przechowuje `NavigationService` wystąpienia w `NavigationService` właściwość typu `INavigationService`. W związku z tym, wyświetlić wszystkie klasy modeli, które wynikają z `ViewModelBase` klasy, można użyć `NavigationService` dostęp do metod, określony przez właściwość `INavigationService` interfejsu. Pozwala to uniknąć obciążenie wprowadza `NavigationService` obiektu z kontenera iniekcji zależności Autofac do każdej klasy modelu widoku.

### <a name="handling-navigation-requests"></a>Obsługa żądań nawigacji

Udostępnia zestaw narzędzi Xamarin.Forms [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) klasy, która implementuje środowisko Nawigacja hierarchiczna, w którym użytkownik jest powinni poruszać się po stronach przodu i do tyłu, zgodnie z potrzebami. Aby uzyskać więcej informacji na temat Nawigacja hierarchiczna zobacz [Nawigacja hierarchiczna](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

Zamiast używać [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) bezpośrednio, klasa zawija aplikacji w ramach aplikacji eShopOnContainers `NavigationPage` klasy w `CustomNavigationView` klasy, jak pokazano w poniższym przykładzie kodu:

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

Celem tego zawijania jest w celu ułatwienia stylów [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) wystąpienia wewnątrz pliku XAML dla klasy.

Nawigacja jest przeprowadzane w ramach klasy modelu widoku, wywołując jedną z `NavigateToAsync` metod, określając typ modelu widoku dla strony jest przejście do, jak pokazano w poniższym przykładzie kodu:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

Poniższy kod przedstawia przykład `NavigateToAsync` metod dostarczonych przez `NavigationService` klasy:

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

Każda metoda umożliwia dowolnej klasy modelu widoku, która pochodzi od klasy `ViewModelBase` klasy w celu wykonania Nawigacja hierarchiczna, wywołując `InternalNavigateToAsync` metody. Ponadto drugi `NavigateToAsync` metoda umożliwia nawigacji danych można określić jako argument, który jest przekazywany do modelu widoku trwa przejście, zazwyczaj służy do wykonywania inicjowania. Aby uzyskać więcej informacji, zobacz [przekazywanie parametrów podczas nawigacji](#passing_parameters_during_navigation).

`InternalNavigateToAsync` Metoda wykonuje żądanie nawigacji i przedstawiono w poniższym przykładzie kodu:

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

`InternalNavigateToAsync` Metoda wykonuje nawigację do modelu widoku za pośrednictwem pierwszego wywołania `CreatePage` metody. Ta metoda lokalizuje widok, który odnosi się do widoku określonego typu modelu i tworzy i zwraca wystąpienie tego typu widoku. Lokalizowanie widok, który odnosi się do typu modelu widoku używa oparty na Konwencji podejście, zakłada się, że:

-   Widoki są tego samego zestawu jako typy modelu widoku.
-   Widoki są w. Podrzędna przestrzeń nazw widoków.
-   Wyświetl modele znajdują się w. Podrzędna przestrzeń nazw modele widoków.
-   Wyświetlanie nazw odnoszą się do wyświetlania nazwy modelu z "Model" usunięte.

Podczas tworzenia wystąpienia widoku, jest ona skojarzona z jego odpowiedni model widoku. Aby uzyskać więcej informacji na temat sposobu dzieje się tak, zobacz [automatyczne tworzenie modelu widoku z lokalizatorem modelu widoku](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Jeśli widok, tworzona jest `LoginView`, jest opakowywany w nowe wystąpienie klasy `CustomNavigationView` klasy i przypisane do [ `Application.Current.MainPage` ](xref:Xamarin.Forms.Application.MainPage) właściwości. W przeciwnym razie `CustomNavigationView` wystąpienie jest pobierana i pod warunkiem, że nie ma wartość null, [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage) metoda jest wywoływana wypychania widoku, tworzona na stosie nawigacji. Jednak jeśli pobrany `CustomNavigationView` wystąpienie jest `null`, widoku, tworzona jest opakowywany w nowe wystąpienie klasy `CustomNavigationView` klasy i przypisane do `Application.Current.MainPage` właściwości. Ten mechanizm zapewnia, że podczas nawigacji strony są poprawnie dodana do stos nawigacji zarówno podczas jest pusta i zawiera dane.

> [!TIP]
> Należy wziąć pod uwagę buforowanie stron. Strony pamięci podręcznej powoduje zużycie pamięci dla widoków, które nie są obecnie wyświetlane. Bez buforowania strony jej oznacza to jednak, że analiza kodu XAML i konstruowania strony i jego model widoku nastąpi za każdym razem, gdy nowa strona jest przejście, który może mieć wpływ na wydajność ze stroną, złożone. Dobrze zaprojektowana strony, który nie używa nadmiernej liczby kontrolek działania powinny być wystarczające. Jednak buforowania strony może pomóc, jeśli zostaną napotkane czasy ładowania stron powolne.

Po widok jest tworzony i przejście, `InitializeAsync` metoda model skojarzony widok tego widoku jest wykonywana. Aby uzyskać więcej informacji, zobacz [przekazywanie parametrów podczas nawigacji](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Nawigowanie po aplikacji jest uruchamiany.

Gdy aplikacja jest uruchamiana, `InitNavigation` method in Class metoda `App` klasy jest wywoływany. Poniższy przykład kodu pokazuje tę metodę:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Metoda tworzy nowy `NavigationService` obiekt kontenera iniekcji zależności Autofac i zwraca odwołanie do niego przed wywołaniem jego `InitializeAsync` metody.

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

Aby uzyskać więcej informacji na temat kontenera iniekcji zależności Autofac zobacz [wprowadzenie do wstrzykiwania zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Przekazywanie parametrów podczas nawigacji

Jedną z `NavigateToAsync` metod, określony przez `INavigationService` interfejs umożliwia nawigacji danych można określić jako argument, który jest przekazywany do modelu widoku trwa przejście, zazwyczaj służy do wykonywania inicjowania.

Na przykład `ProfileViewModel` klasa zawiera `OrderDetailCommand` , jest wykonywany, gdy użytkownik wybierze zamówienie na `ProfileView` strony. Z kolei wykonuje `OrderDetailAsync` metody, która jest wyświetlana w poniższym przykładzie kodu:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Ta metoda wywołuje nawigacji `OrderDetailViewModel`, przekazując `Order` wystąpienia, która reprezentuje porządek, który użytkownik zaznaczył na `ProfileView` strony. Gdy `NavigationService` tworzy klasę `OrderDetailView`, `OrderDetailViewModel` klasy jest tworzone i przypisane do tego widoku [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Po przejściu do `OrderDetailView`, `InternalNavigateToAsync` metoda jest wykonywana `InitializeAsync` metoda widoku skojarzonej z modelu widoku.

`InitializeAsync` Metoda jest zdefiniowana w `ViewModelBase` klasy jako metodę, która może zostać zastąpiona. Ta metoda Określa `object` argument, który reprezentuje dane, które mają być przekazane do modelu widoku podczas operacji nawigacji. W związku z tym, widok modelu klas, które chcesz odbierać dane z operacją nawigacji zapewniają zapewniali własną implementację `InitializeAsync` metodę w celu inicjowania wymagane. Poniższy kod przedstawia przykład `InitializeAsync` metody z `OrderDetailViewModel` klasy:

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

Ta metoda umożliwia pobranie `Order` szczegółowe informacje o wystąpienia, która została przekazana do modelu widoku podczas operacji nawigacji i używa go w celu pobrania pełnego zamówień `OrderService` wystąpienia.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Wywoływanie nawigacji za pomocą zachowań

Nawigacja jest zwykle wyzwalanych z widoku interakcji z użytkownikiem. Na przykład `LoginView` wykonuje nawigacji po pomyślnym uwierzytelnieniu. Poniższy przykład kodu pokazuje, jak nawigacja jest wywoływany przez zachowanie:

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

W czasie wykonywania `EventToCommandBehavior` odpowie na interakcję z [ `WebView` ](xref:Xamarin.Forms.WebView). Gdy `WebView` powoduje przejście do strony sieci web [ `Navigating` ](xref:Xamarin.Forms.WebView.Navigating) nastąpi zdarzenie, które będą wykonywane `NavigateCommand` w `LoginViewModel`. Domyślnie argumenty zdarzeń dla zdarzenia są przekazywane do polecenia. Tych danych jest konwertowany, ponieważ jest przekazywana przez konwerter określony w elemencie źródłowym i docelowym `EventArgsConverter` właściwość, która zwraca [ `Url` ](xref:Xamarin.Forms.WebNavigationEventArgs.Url) z [ `WebNavigatingEventArgs` ](xref:Xamarin.Forms.WebNavigatingEventArgs). W związku z tym, kiedy `NavigationCommand` jest wykonywane, adres Url strony sieci web jest przekazywany jako parametr do zarejestrowaną `Action`.

Z kolei `NavigationCommand` wykonuje `NavigateAsync` metody, która jest wyświetlana w poniższym przykładzie kodu:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Ta metoda wywołuje nawigacji `MainViewModel`, i po nawigacji, usuwa `LoginView` strony stos nawigacji.

### <a name="confirming-or-cancelling-navigation"></a>Trwa Potwierdzanie lub anulowanie nawigacji

Aplikacja może być konieczne do interakcji z użytkownikiem podczas operacji nawigacji, aby użytkownik może zaakceptować lub odrzucić nawigacji. Może to być konieczne, na przykład, gdy użytkownik próbuje Przejdź przed upływem pełni strony wprowadzania danych. W takiej sytuacji aplikacja powinna dostarczyć powiadomień, który umożliwia użytkownikowi opuścić stronę lub anulować operację nawigacji, zanim nastąpi. Można to osiągnąć w widoku klasy modelu przy użyciu odpowiedzi z powiadomienie do kontrolowania, czy jest wywoływany nawigacji.

## <a name="summary"></a>Podsumowanie

Zestaw narzędzi Xamarin.Forms obsługuje nawigowania po stronach, co zwykle powoduje z interakcji użytkownika z interfejsu użytkownika lub aplikacji, w wyniku zmiany stanu na podstawie logiki wewnętrznej. Jednak nawigacji może być skomplikowane, aby zaimplementować w aplikacji, które używają wzorca MVVM.

W tym rozdziale prezentowane `NavigationService` klasy, która jest używana do wykonywania widoku pierwszy model nawigacyjnych z modeli widoków. Umieszczenie logiki nawigacji w widoku klas modelu oznacza, że logika może być wykonywana za pomocą zautomatyzowanych testów. Ponadto model widoku można zaimplementować logikę do kontroli nawigacji, aby upewnić się, że niektóre reguły biznesowe są wymuszane.


## <a name="related-links"></a>Linki pokrewne

- [Pobierz książkę elektroniczną (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [ramach aplikacji eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
