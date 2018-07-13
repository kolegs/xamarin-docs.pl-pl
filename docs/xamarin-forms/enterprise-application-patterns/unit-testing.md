---
title: Aplikacje firmowe testowanie jednostkowe
description: W tym rozdziale wyjaśniono, jak testy jednostkowe odbywa się w ramach aplikacji eShopOnContainers aplikacji mobilnej.
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 02aeedd5498c47950e2fbc0d218de05bc0bb3204
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998686"
---
# <a name="unit-testing-enterprise-apps"></a>Aplikacje firmowe testowanie jednostkowe

Aplikacje mobilne mają unikatowe problemy, które pulpitu i aplikacje sieci web, nie musisz martwić się o. Użytkownicy urządzeń przenośnych różnią się przez urządzenia, które używają, przez połączenie sieciowe, według dostępności usług i wielu innych czynników. W związku z tym powinien zostać przetestowany aplikacje mobilne, jak będą używane w świecie rzeczywistym, aby zwiększyć ich jakość, niezawodność i wydajność. Istnieje wiele typów testów, które powinny być wykonywane w aplikacji, w tym testy jednostkowe, testowanie integracji i testowania za pomocą testowania jest najczęściej używany typ testowania jednostkowego interfejsu użytkownika.

Test jednostkowy przyjmuje małą jednostką aplikacji, zazwyczaj metodę, jednocześnie zostanie odizolowana od pozostałej części kodu i sprawdza się, że działa zgodnie z oczekiwaniami. Jej celem jest sprawdzanie, czy każda jednostka wersji funkcji działa zgodnie z oczekiwaniami, tak aby błędy nie propagowane w całej aplikacji. Wykrywanie błędów, w którym występuje jest bardziej wydajne obserwowania wpływu błędów pośrednio w dodatkowej punkt awarii.

Testy jednostkowe ma największy wpływ na jakość kodu, gdy jest integralną częścią przepływu pracy tworzenia oprogramowania. Zaraz po zapisaniu metody testów jednostkowych zapisywane, zweryfikować zachowanie metody w odpowiedzi na standardowy, granic lub niepoprawny przypadki danych wejściowych i sprawdzanie jawnego lub niejawnego założenia przez kod. Alternatywnie za pomocą testów opartych na rozwój, testy jednostkowe są zapisywane przed kodem. W tym scenariuszu testy jednostkowe działają jako specyfikacji funkcjonalnych i dokumentacji projektu.

> [!NOTE]
> Testy jednostkowe są bardzo efektywne przeciwko regresji — oznacza to, że te funkcje, które używanej do pracy, ale został zakłócony wadliwe aktualizacji.

Testy jednostkowe zwykle używać assert act Rozmieść wzorca:

-   *Rozmieść* sekcji metodę testu jednostkowego inicjuje obiektów i ustawia wartość danych, który jest przekazywany do metody w ramach testu.
-   *Działają* sekcji wywołuje metodę w ramach testu z wymaganymi argumentami.
-   *Asercja* sekcji sprawdza, czy akcja testowaną metodę zachowuje się zgodnie z oczekiwaniami.

Następujące ten wzorzec zapewnia się, że testy jednostkowe są czytelne i spójne.

## <a name="dependency-injection-and-unit-testing"></a>Wstrzykiwanie zależności i testy jednostkowe

Jedną z zresztą przyjmowania luźno powiązane architektury jest, że ułatwia tworzenie testów jednostkowych. Jeden z typów zarejestrowane w usłudze Autofac jest `OrderService` klasy. Poniższy przykładowy kod przedstawia zarys tej klasy:

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

`OrderDetailViewModel` Klasy zależny od `IOrderService` typ, który jest rozpoznawany jako kontener, gdy tworzy `OrderDetailViewModel` obiektu. Jednak zamiast tworzyć `OrderService` obiekt do testów jednostkowych `OrderDetailViewModel` klasy, należy zastąpić `OrderService` obiektu z pozorny na potrzeby testów. Rysunek 10-1 ilustruje tę relację.

![](unit-testing-images/unittesting.png "Klasy, które implementują interfejs IOrderService")

**Rysunek 10-1:** klasy, które implementują interfejs IOrderService

Takie podejście umożliwia `OrderService` obiektu do przekazania do `OrderDetailViewModel` klasy w czasie wykonywania oraz w celu testowania, umożliwia ona `OrderMockService` klasy do przekazania do `OrderDetailViewModel` klasy w czasie testu. Główną zaletą tego podejścia jest umożliwienie testów jednostkowych, aby być wykonywane bez konieczności one nieporęczne za zasoby, takie jak usługi sieci web i baz danych.

## <a name="testing-mvvm-applications"></a>Testowanie aplikacji z modelem MVVM

Testowanie modeli i modeli widoków z modelem MVVM aplikacji jest identyczne z testowaniem innych klas, a te same narzędzia i techniki — takich jak jednostki, testowania i pozorowanie, mogą być używane. Jednak istnieją pewne wzorców, które są typowe dla modelu i widoku klasy modeli, które mogą skorzystać z technik testowania określonej jednostki.

> [!TIP]
> Przetestuj jedno z każdy test jednostkowy. Nie należy tego robić, zapewnienie jednostki testowania więcej niż jeden z aspektów zachowania jednostki wykonywania. Prowadzi to do testów, które są trudne do odczytywania i aktualizowania. Może również powodować w błąd przy interpretowaniu awarii.

Zastosowań aplikacji mobilnej w ramach aplikacji eShopOnContainers [xUnit](https://xunit.github.io/) przeprowadzić testy jednostkowe, która obsługuje dwa rodzaje testów jednostkowych:

-   Fakty są testy, które są zawsze ma wartość true, które testują niezmiennych warunków.
-   Teorii są testy, które są tylko wartość true dla określonego zestawu danych.

Fakt testy są dołączone do aplikacji mobilnej w ramach aplikacji eShopOnContainers testy jednostkowe, a więc każdej metody testowej jednostki zostanie nadany `[Fact]` atrybutu.

> [!NOTE]
> testów jednostkowych xUnit są wykonywane przez narzędzie test runner. Aby wykonać narzędzia test runner, uruchom projekt eShopOnContainers.TestRunner wymagane platformy.

### <a name="testing-asynchronous-functionality"></a>Testowanie funkcji asynchronicznych

Podczas implementowania wzorca MVVM, modeli widoków zwykle wywołuje operacje na usługi, często asynchronicznie. Testów dla kodu, który wywołuje te operacje, zwykle na użytek mocks jako części zamienne rzeczywiste usługi. Poniższy przykład kodu pokazuje, testowanie funkcji asynchronicznej, przekazując makiety usługi do modelu widoku:

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

Ten test jednostkowy sprawdza, czy `Order` właściwość `OrderDetailViewModel` wystąpienie będzie mieć wartość po `InitializeAsync` wywołaniu metody. `InitializeAsync` Metoda jest wywoływana, gdy jest przejście odpowiedni widok modelu widoku. Aby uzyskać więcej informacji o nawigacji, zobacz [nawigacji](~/xamarin-forms/enterprise-application-patterns/navigation.md).

Gdy `OrderDetailViewModel` tworzone jest wystąpienie, oczekuje `OrderService` wystąpienia, należy określić jako argument. Jednak `OrderService` pobiera dane z usługi sieci web. W związku z tym `OrderMockService` wystąpienia, co jest binders wersję z `OrderService` klasy, jest określony jako argument `OrderDetailViewModel` konstruktora. Następnie, gdy model widoku `InitializeAsync` wywoływana jest metoda, która wywołuje `IOrderService` operacji, danych testowych jest pobrane zamiast komunikować się z usługą sieci web.

### <a name="testing-inotifypropertychanged-implementations"></a>Testowanie implementacje INotifyPropertyChanged

Implementowanie `INotifyPropertyChanged` interfejs umożliwia widoków, aby reagować na zmiany, które pochodzą z widoku modeli i modeli. Te zmiany nie są ograniczone do danych wyświetlanych w kontrolkach — są one również używane do kontrolowania widoku, takie jak stanów modelu widoku, które powodują animacji do uruchomienia lub kontrolki, które mają zostać wyłączone.

Właściwości, które mogą być aktualizowane bezpośrednio przez test jednostkowy mogą być testowane przez dołączenie program obsługi zdarzeń do `PropertyChanged` zdarzeń i sprawdzanie, czy zdarzenie jest wywoływane po ustawieniu nową wartość dla właściwości. Poniższy przykład kodu pokazuje tych testów:

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

Ten test jednostkowy wywołuje `InitializeAsync` metody `OrderViewModel` klasy, co powoduje, że jego `Order` właściwości do zaktualizowania. Przejdzie test jednostkowy, pod warunkiem, że `PropertyChanged` zdarzenie jest wywoływane dla `Order` właściwości.

### <a name="testing-message-based-communication"></a>Testowanie komunikacji oparta na komunikatach

Widok modeluje używające [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) klasy do komunikowania się między luźno powiązanych klas mogą być jednostki testowane przez subskrybowanie wiadomości wysyłanych przez kod testu, jak pokazano w poniższym przykładzie kodu:

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

Ten test jednostkowy sprawdza, czy `CatalogViewModel` publikuje `AddProduct` komunikat w odpowiedzi na jego `AddCatalogItemCommand` wykonywana. Ponieważ [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) klasy obsługuje subskrypcje komunikatu multiemisji, może być subskrybowana przez test jednostkowy `AddProduct` komunikatu i wykonywanie delegata wywołania zwrotnego w odpowiedzi na jego otrzymania. Ustawia ten delegat wywołania zwrotnego, określony jako wyrażenie lambda `boolean` pola, które jest używane przez `Assert` instrukcję, aby sprawdzić zachowanie testu.

### <a name="testing-exception-handling"></a>Testowanie obsługi wyjątków

Testy jednostkowe można również zapisać tego upewnij się, że określone wyjątki są zgłaszane dla nieprawidłowych akcji lub danych wejściowych, jak pokazano w poniższym przykładzie kodu:

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

Ten test jednostkowy spowoduje zgłoszenie wyjątku, ponieważ [ `ListView` ](xref:Xamarin.Forms.ListView) formant nie ma zdarzenia o nazwie `OnItemTapped`. `Assert.Throws<T>` Metoda jest metodą ogólnego gdzie `T` jest typ oczekiwanego wyjątku. Argumentu przekazanego do `Assert.Throws<T>` metoda to wyrażenie lambda, które spowoduje zgłoszenie wyjątku. W związku z tym, test jednostkowy zostanie przekazany, pod warunkiem, że wyrażenie lambda zgłasza `ArgumentException`.

>💡 **Porada**: uniknięcia, pisanie testów jednostkowych, które zbadać ciągi komunikatów wyjątku. Ciągi komunikatów wyjątek mogą ulec zmianie wraz z upływem czasu, a więc testów jednostkowych, które zależą od ich obecność są traktowane jako elastycznego.

### <a name="testing-validation"></a>Testowanie poprawności

Istnieją dwa aspekty do testowania implementacji sprawdzania poprawności: testowanie, że reguł sprawdzania poprawności są wykonywane prawidłowo, a testy, które `ValidatableObject<T>` klasy działa zgodnie z oczekiwaniami.

Logikę weryfikacji jest zwykle prosty przetestować, ponieważ zwykle jest niezależna proces, w której dane wyjściowe jest zależny od danych wejściowych. Powinna istnieć testy z wyników wywołania `Validate` metody dla każdej właściwości, która ma co najmniej jedną regułę sprawdzania poprawności skojarzone, jak pokazano w poniższym przykładzie kodu:

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

Ten test jednostkowy sprawdza, czy weryfikacja zakończy się powodzeniem po dwóch `ValidatableObject<T>` właściwości `MockViewModel` wystąpienia zarówno znajdują się dane.

A także sprawdzania, czy weryfikacja zakończy się powodzeniem, testy jednostkowe weryfikacji należy także sprawdzić wartości `Value`, `IsValid`, i `Errors` właściwości każdego `ValidatableObject<T>` wystąpienia, aby sprawdzić, czy klasa działa zgodnie z oczekiwaniami. Poniższy przykład kodu demonstruje test jednostkowy, który wykonuje to:

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

Ten test jednostkowy sprawdza, czy sprawdzanie poprawności zakończy się niepowodzeniem kiedy `Surname` właściwość `MockViewModel` nie zawiera żadnych danych i `Value`, `IsValid`, i `Errors` właściwości każdego `ValidatableObject<T>` wystąpienia są poprawnie ustawione.

## <a name="summary"></a>Podsumowanie

Test jednostkowy przyjmuje małą jednostką aplikacji, zazwyczaj metodę, jednocześnie zostanie odizolowana od pozostałej części kodu i sprawdza się, że działa zgodnie z oczekiwaniami. Jej celem jest sprawdzanie, czy każda jednostka wersji funkcji działa zgodnie z oczekiwaniami, tak aby błędy nie propagowane w całej aplikacji.

Zachowanie obiektu w trakcie testu można samodzielnie, zastępując obiekty zależne makiety obiektów, które symulować obiekty zależne. Dzięki temu testów jednostkowych, aby być wykonywane bez konieczności one nieporęczne za zasoby, takie jak usługi sieci web i baz danych.

Testowanie modeli i modeli widoków z modelem MVVM aplikacji jest identyczne z testowaniem innych klas, a te same narzędzia i techniki mogą być używane.


## <a name="related-links"></a>Linki pokrewne

- [Pobierz książkę elektroniczną (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [ramach aplikacji eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
