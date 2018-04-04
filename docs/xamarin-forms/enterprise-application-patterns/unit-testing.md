---
title: Testowanie jednostkowe
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 57201a32f5ffc0ae962f6db851a25a737e1cb17d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="unit-testing"></a>Testowanie jednostkowe

Aplikacje mobilne ma unikatowy problemów, które pulpitów i aplikacji sieci web, nie trzeba martwić się. Użytkownicy mobilni zostanie różnią się od urządzenia, które używają, poprzez łączność z siecią, dostępność usług i wielu innych czynników. W związku z tym aplikacje mobilne powinny być testowane, jak będą one używane w świecie rzeczywistym zwiększające ich jakości, niezawodności i wydajności. Istnieje wiele typów testów, które powinny zostać wykonane w aplikacji, w tym testy jednostkowe, testowanie integracji i interfejs użytkownika testowanie za pomocą testowania jest najczęściej używana forma testowania jednostek.

Testu jednostkowego przyjmuje małą jednostką aplikacji, zazwyczaj metodę izoluje go od pozostałej części kodu i sprawdza się, że działa zgodnie z oczekiwaniami. Jego celem jest sprawdzenie, czy każdej jednostki funkcja zadziałała zgodnie z oczekiwaniami, tak, aby błędy nie propagację w całej aplikacji. Wykrywanie błędów, gdzie występuje jest bardziej wydajny obserwowania skutków błędu pośrednio pod adresem dodatkowej punktu awarii.

Testy jednostkowe ma największy wpływ na jakości kodu, gdy jest integralną częścią przepływu pracy rozwoju oprogramowania. Jak najszybciej po zapisaniu metody testów jednostkowych powinna być zapisana który Sprawdź zachowanie w odpowiedzi na standardowy, granic lub niepoprawne przypadków w danych wejściowych metody i sprawdzanie żadnym założeniu jawnych ani niejawnych wprowadzone przez kod. Alternatywnie z testem driven development, testy jednostkowe są zapisywane przed kodem. W tym scenariuszu testy jednostkowe działa jako dokumentacja i specyfikacje funkcjonalne.

> [!NOTE]
> Testy jednostkowe są bardzo skuteczne przed regresji — to znaczy, funkcje, które używanej do pracy, ale został zakłóceń przez aktualizację uszkodzony.

Testy jednostkowe zwykle Użyj wzorca assert act Rozmieść:

-   *Rozmieść* sekcji metody testowej jednostki inicjuje obiekty i ustawia wartość danych, który jest przekazywany do metody w ramach testu.
-   *Działania* sekcji wywołuje metodę w ramach testu z liczbą wymaganych argumentów.
-   *Assert* sekcji sprawdza, czy akcja metody w ramach testu działa zgodnie z oczekiwaniami.

Po tym wzorcu zapewnia, że testy jednostkowe są do odczytu i spójny.

## <a name="dependency-injection-and-unit-testing"></a>Iniekcji zależności i testy jednostkowe

Jednym z motywacji przyjmowania to architektura luźno powiązane jest ułatwia ona testowania jednostek. Jeden z typów zarejestrowany Autofac jest `OrderService` klasy. Poniższy przykład kodu pokazuje konspekt tej klasy:

```csharp
public class OrderDetailViewModel : ViewModelBase  
{  
    private IOrderService _ordersService;  

    public OrderDetailViewModel(IOrderService ordersService)  
    {  
        _ordersService = ordersService;  
    }  
    ...  
}
```

`OrderDetailViewModel` Klasa ma zależność `IOrderService` typu, który jest rozpoznawany jako kontenera, gdy tworzy `OrderDetailViewModel` obiektu. Jednak zamiast tworzyć `OrderService` obiekt do testu jednostkowego `OrderDetailViewModel` klasy, należy zastąpić `OrderService` obiektu z makiety na potrzeby testów. Rysunek 10-1 przedstawiono tę relację.

![](unit-testing-images/unittesting.png "Klasy, które implementują interfejs IOrderService")

**Rysunek 10-1.** klas implementujących interfejs IOrderService

Takie podejście umożliwia `OrderService` obiektu do przekazania do `OrderDetailViewModel` klasy w czasie wykonywania, a w celu testowania, umożliwia `OrderMockService` klasy do przekazania do `OrderDetailViewModel` klasy w czasie testu. Główną zaletą tej metody jest możliwość testów jednostkowych do wykonania bez konieczności niewygodna zasoby, takie jak usługi sieci web i baz danych.

## <a name="testing-mvvm-applications"></a>Testowanie aplikacji z modelem MVVM

Badania modeli i wyświetlanie modeli z modelem MVVM aplikacji są identyczne z testowaniem innych klas, a tym samym narzędzi i technik —, takich jak jednostki testowania i mocking, mogą być używane. Istnieją pewne wzorce, które są typowe dla modelu i klasy modelu widoku, które mogą korzystać z techniki testowania określonej jednostki.

> [!TIP]
> Przetestuj rzecz z każdego z testów jednostkowych. Nie należy tego robić, aby jednostka test wykonywania więcej niż jednym aspekcie zachowanie jednostki. Dzięki temu prowadzi do testów, które są trudne do odczytywania i aktualizowania. On również może prowadzić do pomyłek przy interpretowaniu awarii.

Używa aplikacji mobilnej eShopOnContainers [xUnit](https://xunit.github.io/) przeprowadzenie testowania, jednostek, który obsługuje dwa rodzaje testów jednostkowych:

-   Faktów są testy, które są zawsze ma wartość true, który niezmiennej warunkach testowych.
-   Teorii są testy, które są tylko wartość true dla określonego zestawu danych.

Testy jednostkowe dołączony do aplikacji mobilnych eShopOnContainers są testy fakt, a więc ozdobione każdej metody testowej jednostki `[Fact]` atrybutu.

> [!NOTE]
> xUnit testy są wykonywane przez moduł uruchamiający. Aby wykonać uruchamiający, uruchom projekt eShopOnContainers.TestRunner wymagane platformy.

### <a name="testing-asynchronous-functionality"></a>Testowanie funkcji asynchronicznych

Podczas implementowania wzorzec MVVM, wyświetlanie modeli zwykle wywoływać operacje w usługach, często asynchronicznie. Testy kod, który wywołuje te operacje zwykle używane mocks jako elementy zastępcze rzeczywiste usługami. Poniższy przykład kodu pokazuje, testowanie funkcji asynchronicznych przez przekazanie zasymulować usługi do modelu widoku:

```csharp
[Fact]  
public async Task OrderPropertyIsNotNullAfterViewModelInitializationTest()  
{  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.NotNull(orderViewModel.Order);  
}
```

Ten test jednostkowy sprawdza, czy `Order` właściwość `OrderDetailViewModel` wystąpienie będzie miał wartość po `InitializeAsync` została wywołana metoda. `InitializeAsync` Metoda jest wywoływana, gdy jest przejście odpowiedni widok modelu widoku. Aby uzyskać więcej informacji na temat nawigacji, zobacz [nawigacji](~/xamarin-forms/enterprise-application-patterns/navigation.md).

Gdy `OrderDetailViewModel` jest tworzone wystąpienie, oczekuje `OrderService` wystąpienia, należy określić jako argument. Jednak `OrderService` pobiera dane z usługi sieci web. W związku z tym `OrderMockService` wystąpienia, która jest wersję z `OrderService` klasa, jest określony jako argument `OrderDetailViewModel` konstruktora. Następnie, jeśli model widoku `InitializeAsync` wywoływana jest metoda, która wywołuje `IOrderService` operacji danych testowych jest pobrane zamiast komunikacji z usługą sieci web.

### <a name="testing-inotifypropertychanged-implementations"></a>Testowanie implementacji interfejsu INotifyPropertyChanged

Implementowanie `INotifyPropertyChanged` interfejs umożliwiający widoków reagowanie na zmiany, które pochodzą z widoku modeli i modeli. Te zmiany nie są ograniczone do danych wyświetlanych w formantach — są również używane do kontrolowania widoku, takie jak wyświetlanie stanów modelu, powodujących animacji można uruchomić lub formantów, które mają zostać wyłączone.

Właściwości, które może być aktualizowana bezpośrednio przez testu jednostkowego można przetestować przez dołączenie obsługi zdarzeń do `PropertyChanged` zdarzeń i sprawdzanie, czy zdarzenie jest wywoływane po wykonaniu ustawieniem nowej wartości właściwości. Poniższy przykład kodu pokazuje tych testów:

```csharp
[Fact]  
public async Task SettingOrderPropertyShouldRaisePropertyChanged()  
{  
    bool invoked = false;  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    orderViewModel.PropertyChanged += (sender, e) =>  
    {  
        if (e.PropertyName.Equals("Order"))  
            invoked = true;  
    };  
    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.True(invoked);  
}
```

Ten test jednostkowy wywołuje `InitializeAsync` metody `OrderViewModel` klasy, co powoduje, że jego `Order` właściwości do zaktualizowania. Przekazuje testu jednostkowego, pod warunkiem, że `PropertyChanged` zdarzenie jest wywoływane dla `Order` właściwości.

### <a name="testing-message-based-communication"></a>Komunikacja oparta na wiadomości testowych

Wyświetl modele używające [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) klasy do komunikacji między klasami luźno połączonych można jednostki przetestować Subskrybuj komunikat jest wysyłany przez kod w ramach testu, jak pokazano w poniższym przykładzie:

```csharp
[Fact]  
public void AddCatalogItemCommandSendsAddProductMessageTest()  
{  
    bool messageReceived = false;  
    var catalogService = new CatalogMockService();  
    var catalogViewModel = new CatalogViewModel(catalogService);  

    Xamarin.Forms.MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
        this, MessageKeys.AddProduct, (sender, arg) =>  
    {  
        messageReceived = true;  
    });  
    catalogViewModel.AddCatalogItemCommand.Execute(null);  

    Assert.True(messageReceived);  
}
```

Ten test jednostkowy sprawdza, czy `CatalogViewModel` publikuje `AddProduct` komunikat w odpowiedzi na jego `AddCatalogItemCommand` wykonywana. Ponieważ [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) klasa obsługuje komunikatu multiemisji subskrypcje, mogą subskrybować testu jednostkowego `AddProduct` wiadomości i wykonać delegata wywołania zwrotnego w odpowiedzi na jego otrzymania. Ten delegat wywołania zwrotnego, określone jako wyrażenia lambda, ustawia `boolean` pola używanego przez `Assert` instrukcji, aby sprawdzić działanie testu.

### <a name="testing-exception-handling"></a>Testowanie obsługi wyjątków

Testy jednostkowe można również zapisać tego Sprawdź, czy określone wyjątki zostaną zgłoszone dla nieprawidłowych akcji lub danych wejściowych, jak pokazano w poniższym przykładzie:

```csharp
[Fact]  
public void InvalidEventNameShouldThrowArgumentExceptionText()  
{  
    var behavior = new MockEventToCommandBehavior  
    {  
        EventName = "OnItemTapped"  
    };  
    var listView = new ListView();  

    Assert.Throws<ArgumentException>(() => listView.Behaviors.Add(behavior));  
}
```

Ten test jednostkowy spowoduje zgłoszenie wyjątku, ponieważ [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) formant nie ma zdarzenia o nazwie `OnItemTapped`. `Assert.Throws<T>` Metoda to metoda rodzajowa gdzie `T` jest typ oczekiwany wyjątek. Argument przekazany do `Assert.Throws<T>` metoda jest wyrażenie lambda, które spowoduje zgłoszenie wyjątku. W związku z tym testu jednostkowego zostanie przekazany, pod warunkiem, że zgłasza wyjątek, wyrażenia lambda `ArgumentException`.

>💡 **Porada**: unikać pisania testów jednostkowych, które zbadać ciągi komunikat wyjątku. Ciągi komunikat wyjątku może ulec zmianie, a więc testy jednostek, które opierają się na ich obecność są traktowane jako niestabilnego.

### <a name="testing-validation"></a>Testowanie poprawności

Istnieją dwa aspekty do testowania implementacji sprawdzania poprawności: testowania poprawnie wykonanie reguł sprawdzania poprawności i testowania, który `ValidatableObject<T>` klasa działa zgodnie z oczekiwaniami.

Logikę weryfikacji jest zwykle proste do testowania, ponieważ zwykle jest procesem niezależne, których dane wyjściowe zależy od danych wejściowych. Powinna być testy z wyników wywoływania `Validate` metody dla każdej właściwości, która ma co najmniej jedną regułę walidacji skojarzony, jak pokazano w poniższym przykładzie kodu:

```csharp
[Fact]  
public void CheckValidationPassesWhenBothPropertiesHaveDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  
    mockViewModel.Surname.Value = "Smith";  

    bool isValid = mockViewModel.Validate();  

    Assert.True(isValid);  
}
```

Ten test jednostkowy sprawdza, czy weryfikacja zakończy się powodzeniem po dwa `ValidatableObject<T>` właściwości w `MockViewModel` wystąpienia obu znajdują się dane.

A także sprawdzania, czy weryfikacja zakończy się powodzeniem, testy jednostkowe sprawdzania poprawności należy także sprawdzić wartości `Value`, `IsValid`, i `Errors` właściwości każdego `ValidatableObject<T>` wystąpienia, aby sprawdzić, czy klasa działa zgodnie z oczekiwaniami. Poniższy przykład kodu pokazuje testu jednostkowego, w tym:

```csharp
[Fact]  
public void CheckValidationFailsWhenOnlyForenameHasDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  

    bool isValid = mockViewModel.Validate();  

    Assert.False(isValid);  
    Assert.NotNull(mockViewModel.Forename.Value);  
    Assert.Null(mockViewModel.Surname.Value);  
    Assert.True(mockViewModel.Forename.IsValid);  
    Assert.False(mockViewModel.Surname.IsValid);  
    Assert.Empty(mockViewModel.Forename.Errors);  
    Assert.NotEmpty(mockViewModel.Surname.Errors);  
}
```

Ten test jednostkowy sprawdza, czy niepowodzenia weryfikacji, kiedy `Surname` właściwość `MockViewModel` nie ma żadnych danych i `Value`, `IsValid`, i `Errors` właściwości każdego `ValidatableObject<T>` wystąpienia są poprawnie ustawione.

## <a name="summary"></a>Podsumowanie

Testu jednostkowego przyjmuje małą jednostką aplikacji, zazwyczaj metodę izoluje go od pozostałej części kodu i sprawdza się, że działa zgodnie z oczekiwaniami. Jego celem jest sprawdzenie, czy każdej jednostki funkcja zadziałała zgodnie z oczekiwaniami, tak, aby błędy nie propagację w całej aplikacji.

Zachowanie obiektu w ramach testu można samodzielnie przez zamianę obiekty zależne zasymulować obiektów, które symulować obiekty zależne. Dzięki temu testów jednostkowych do wykonania bez konieczności niewygodna zasoby, takie jak usługi sieci web i baz danych.

Testowanie modeli i wyświetlanie modeli z modelem MVVM aplikacji są identyczne z testowaniem innych klas, a następnie można użyć tego samego narzędzi i technik.


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
