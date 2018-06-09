---
title: Iniekcji zależności
description: W tym rozdziale opisano, jak korzysta z aplikacji mobilnej eShopOnContainers iniekcji zależności rozdzielenie typów konkretnych z kodu, która jest zależna od tych typów.
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fb225349b9ffb1c950486a817897b3c26c6ffbe4
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242573"
---
# <a name="dependency-injection"></a>Iniekcji zależności

Zazwyczaj Konstruktor klasy jest wywoływana podczas tworzenia wystąpienia obiektu, a wszystkie wartości, które wymaga obiektu są przekazywane jako argumenty konstruktora. To jest przykładem iniekcji zależności, a w szczególności nosi nazwę *iniekcji konstruktora*. Zależności, których potrzebuje obiektu są wstrzykiwane do konstruktora.

Określając zależności jako typów interfejsów, iniekcji zależności umożliwia rozdzielenie typów konkretnych z kodu, która jest zależna od tych typów. Zazwyczaj używa kontener, który zawiera listę rejestracji i mapowania między interfejsami i typy abstrakcyjne i konkretne typy, które implementuje lub rozszerzyć tych typów.

Istnieją także inne rodzaje iniekcji zależności, takie jak *iniekcji metody ustawiającej właściwość*, i *iniekcji wywołanie metody*, ale rzadko są one widoczne. W związku z tym w tym rozdziale dotyczą wyłącznie wykonywania iniekcji konstruktora z kontenera iniekcji zależności.

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>Wprowadzenie do iniekcji zależności

Iniekcji zależności to specjalna wersja wzorca Inwersja kontroli (IoC), której dotyczą jest odwrócony jest proces uzyskiwania wymaganej zależności. Z iniekcji zależności innej klasy jest odpowiedzialny za wstrzykiwania zależności do obiektu w czasie wykonywania. Poniższy kod przedstawia przykład sposobu `ProfileViewModel` klasy mają strukturę przy użyciu iniekcji zależności:

```csharp
public class ProfileViewModel : ViewModelBase  
{  
    private IOrderService _orderService;  

    public ProfileViewModel(IOrderService orderService)  
    {  
        _orderService = orderService;  
    }  
    ...  
}
```

`ProfileViewModel` Odbiera konstruktora `IOrderService` wystąpienie jako argument, dodane przez inną klasę. Tylko zależności w `ProfileViewModel` klasa znajduje się na typ interfejsu. W związku z tym `ProfileViewModel` klasa nie ma żadnych wiedzy klasy, która odpowiada za tworzenie wystąpień `IOrderService` obiektu. Klasa, która odpowiada za tworzenie wystąpień `IOrderService` obiektu i wstawiany `ProfileViewModel` klasy, nosi nazwę *kontenera iniekcji zależności*.

Kontenery iniekcji zależności zmniejszyć sprzężenia między obiektami, zapewniając możliwość wystąpienia wystąpień klas i zarządzanie nimi życia na podstawie konfiguracji kontenera. Podczas tworzenia obiektów kontenera injects wszelkie zależności, które wymaga obiektu do niego. Jeśli te zależności nie zostały jeszcze utworzone, tworzy i jest rozpoznawany jako ich zależności kontenera.

> [!NOTE]
> Ponadto można zaimplementować iniekcji zależności ręcznie za pomocą fabryk. Jednak przy użyciu kontenera zapewnia dodatkowe funkcje, takie jak zarządzanie okresem istnienia i rejestracja za pomocą zestawu skanowania.

Istnieje kilka zalety korzystania z kontenera iniekcji zależności:

-   Kontener eliminuje to potrzebę klasę, aby zlokalizować jego zależności i zarządzanie nimi ich życia.
-   Kontener umożliwia mapowanie zaimplementowanym zależności bez wpływu na klasie.
-   Kontener umożliwia zmianę zezwalając zależności, które można mocked.
-   Kontener zwiększa łatwość utrzymania, zezwalając nowe klasy w prosty sposób można dodać do aplikacji.

W kontekście aplikacji platformy Xamarin.Forms, który używa MVVM kontener iniekcji zależności zwykle posłuży rejestrowania i rozpoznawania wyświetlanie modeli i rejestrowania usług i ich wstrzyknięcie do widoku modeli.

Są dostępne w aplikacji mobilnej eShopOnContainers Zarządzanie wystąpienia modelu widoku i usługi klas w aplikacji przy użyciu Autofac wiele kontenerów iniekcji zależności. Autofac umożliwia tworzenie luźno powiązanych aplikacji i oferuje wszystkie funkcje w kontenerach iniekcji zależności metody do rejestrowania mapowania typów i wystąpienia obiektów, w tym rozwiązania obiektów, zarządzanie okresy istnienia obiektu i wstrzyknąć obiekty zależne do konstruktorów obiektów, które rozpoznaje. Aby uzyskać więcej informacji o Autofac, zobacz [Autofac](http://autofac.readthedocs.io/en/latest/index.html) na readthedocs.io.

W Autofac `IContainer` interfejs zapewnia kontener iniekcji zależności. Rysunek 3-1 zawiera zależności, korzystając z tego kontenera, który tworzy `IOrderService` obiektu i injects go do `ProfileViewModel` klasy.

![](dependency-injection-images/dependencyinjection.png "Przykład zależności przy użyciu iniekcji zależności")

**Rysunek 3-1.** zależności przy użyciu iniekcji zależności

W czasie wykonywania, kontener musi znać życie `IOrderService` go należy utworzyć wystąpienia, zanim można utworzyć wystąpienia interfejsu `ProfileViewModel` obiektu. Obejmuje to:

-   Kontener podejmowania decyzji o sposobie tworzenia wystąpienia obiektu implementującego `IOrderService` interfejsu. Jest to nazywane *rejestracji*.
-   Kontener tworzenia wystąpienia obiektu, który implementuje `IOrderService` interfejsu i `ProfileViewModel` obiektu. Jest to nazywane *rozpoznawania*.

Po pewnym czasie aplikacja zakończy się przy użyciu `ProfileViewModel` obiekt, a staną się dostępne dla wyrzucanie elementów bezużytecznych. W tym momencie moduł zbierający elementy bezużyteczne powinny dysponować `IOrderService` wystąpienia, jeśli inne klasy nie mają tego samego wystąpienia.

> [!TIP]
> Pisanie kodu pochodzącego od dowolnego kontenera. Zawsze próbować napisać kod niezależny od kontenera oddziel aplikacji z kontenera określonej zależności używany.

## <a name="registration"></a>Rejestracja

Przed zależności mogą zostać dodane do obiektu, typy zależności musi najpierw zostać zarejestrowana z kontenerem. Rejestrowanie typu zazwyczaj polega na przekazanie kontenera interfejs i typu konkretnego, który implementuje interfejs.

Istnieją dwa sposoby rejestrowania typów i obiektów w kontenerze, za pośrednictwem kodu:

-   Zarejestruj typu lub mapowania z kontenerem. Gdy jest to wymagane, kontener utworzy wystąpienia określonego typu.
-   Zarejestruj istniejący obiekt w kontenerze jako pojedynczą. Gdy jest to wymagane, kontener zwraca odwołanie do istniejącego obiektu.

> [!TIP]
> Kontenery iniekcji zależności nie zawsze są odpowiednie. Iniekcji zależności wprowadza dodatkową złożoność i wymagania, które mogą nie być odpowiednie lub przydatne do małych aplikacji. Klasa nie ma żadnych zależności lub nie jest zależności dla innych typów, może nie być uzasadnione go umieścić w kontenerze. Ponadto jeśli klasa ma jeden zestaw zależności, które są integralną częścią typu i nigdy nie spowoduje zmiany, nie może być uzasadnione go umieścić w kontenerze.

Rejestracja typy, które wymagają iniekcji zależności powinny być wykonywane w jednej metody w aplikacji, a ta metoda powinna być wywoływana na wczesnym etapie cyklu życia aplikacji w celu upewnij się, że dana aplikacja jest sprawdzić zależności między jej klas. W aplikacji mobilnej eShopOnContainers jest to wykonywane przez `ViewModelLocator` klasy, które kompilacje `IContainer` obiektu i jest to jedyna klasa w aplikacji, które zawiera odwołanie do tego obiektu. Poniższy przykład kodu pokazuje, jak deklaruje aplikacji mobilnej eShopOnContainers `IContainer` obiektu w `ViewModelLocator` klasy:

```csharp
private static IContainer _container;
```

Typy i wystąpienia są zarejestrowane w `RegisterDependencies` metoda `ViewModelLocator` klasy. Jest to osiągane przez utworzenie `ContainerBuilder` wystąpienia, które przedstawiono w poniższym przykładzie:

```csharp
var builder = new ContainerBuilder();
```

Typy i wystąpienia są następnie zarejestrowane w usłudze `ContainerBuilder` obiekt i poniższy przykład kodu pokazuje najczęściej rejestracji typu:

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

`RegisterType` Metod przedstawionych w tym miejscu mapy typu interfejsu do konkretnego typu. Informuje o tym kontener można utworzyć wystąpienia `RequestProvider` obiektu podczas tworzenia wystąpień obiektu, który wymaga iniekcję `IRequestProvider` za pośrednictwem konstruktora.

Konkretne typy również mogą być rejestrowane bezpośrednio, bez mapowanie z typu interfejsu, jak pokazano w poniższym przykładzie:

```csharp
builder.RegisterType<ProfileViewModel>();
```

Gdy `ProfileViewModel` typu został rozwiązany, kontener będzie wstrzyknąć jego wymaganych zależności.

Autofac umożliwia także rejestracja wystąpienia, gdzie jest odpowiedzialny za konserwację odwołanie do pojedyncze wystąpienie typu kontenera. Na przykład w poniższym przykładzie kodu pokazano, jak aplikacji mobilnej eShopOnContainers rejestruje konkretnego typu do użycia podczas `ProfileViewModel` wymaga wystąpienia `IOrderService` wystąpienie:

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

`RegisterType` Metod przedstawionych w tym miejscu mapy typu interfejsu do konkretnego typu. `SingleInstance` Metody konfiguruje rejestracji tak, aby każdy obiekt zależny odbiera tego samego udostępnionego wystąpienia. W związku z tym tylko jeden `OrderService` wystąpienie będzie istnieć w kontenerze, który jest współużytkowany przez obiekty, które wymagają iniekcję `IOrderService` za pośrednictwem konstruktora.

Rejestracja wystąpienia można również wykonać z `RegisterInstance` metody, które przedstawiono w poniższym przykładzie:

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

`RegisterInstance` Pokazane metoda tworzy nowy `OrderMockService` wystąpienia i rejestruje go z kontenerem. W związku z tym tylko jeden `OrderMockService` wystąpienie istnieje w kontenerze, który jest współużytkowany przez obiekty, które wymagają iniekcję `IOrderService` za pośrednictwem konstruktora.

Po zarejestrowaniu typu i wystąpienia, `IContainer` obiektu muszą zostać skompilowane, które przedstawiono w poniższym przykładzie kodu:

```csharp
_container = builder.Build();
```

Wywoływanie `Build` metoda `ContainerBuilder` wystąpienia tworzy nowy kontener iniekcji zależności, zawierający rejestracji, które zostały wprowadzone.

>💡 **Porada**: należy wziąć pod uwagę `IContainer` jako niezmienialny. Gdy zapewnia Autofac `Update` w miarę możliwości należy unikać metodę aktualizowania rejestracji z istniejącego kontenera, wywołanie tej metody. Istnieje ryzyko do modyfikowania kontener po jest został utworzony, zwłaszcza w przypadku, gdy kontener został już użyty. Aby uzyskać więcej informacji, zobacz [należy wziąć pod uwagę kontener jako Immutable](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable) na readthedocs.io.

<a name="resolution" />

## <a name="resolution"></a>Rozwiązanie

Po zarejestrowaniu typu można rozwiązać lub dodane jako zależność. Gdy typem jest rozwiązywany i kontener potrzebuje do utworzenia nowego wystąpienia, injects wszelkie zależności do wystąpienia.

Ogólnie rzecz biorąc po usunięciu typu jedno z trzech zdarzeń konsekwencje:

1.  Jeśli typ nie został zarejestrowany, kontener zgłasza wyjątek.
1.  Jeśli ten typ został zarejestrowany jako pojedynczą, kontener zwraca pojedyncze wystąpienie. Jest to typ jest wywoływana dla po raz pierwszy, kontenera ją w razie potrzeby tworzy i przechowuje odwołanie do niej.
1.  Jeśli typ nie został zarejestrowany jako pojedynczą, kontener zwraca nowe wystąpienie i nie przechowuje odwołanie do niej.

Poniższy kod przedstawia przykład sposobu `RequestProvider` typu, który został wcześniej zarejestrowany Autofac mogą zostać rozwiązane:

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

W tym przykładzie Autofac jest proszony o rozwiązanie konkretnego typu dla `IRequestProvider` typu oraz wszelkie zależności. Zazwyczaj `Resolve` metoda jest wywoływana, gdy wymagane jest wystąpienie określonego typu. Informacje dotyczące sterowania okres istnienia obiektów rozwiązane, zobacz [Zarządzanie okresem istnienia rozpoznać obiektów](#managing_the_lifetime_of_resolved_objects).

Poniższy przykład kodu pokazuje, jak aplikacji mobilnej eShopOnContainers tworzy typy modelu widoku oraz ich zależności:

```csharp
var viewModel = _container.Resolve(viewModelType);
```

W tym przykładzie Autofac jest proszony o rozwiązanie typu widoku modelu dla modelu, żądany widok, a kontener może również rozpoznać zależności. Podczas rozpoznawania `ProfileViewModel` jest zależności można rozpoznać typu `IOrderService` obiektu. W związku z tym Autofac najpierw tworzy `OrderService` obiekt, a następnie przekazuje do konstruktora obiektu `ProfileViewModel` klasy. Aby uzyskać więcej informacji na temat sposobu aplikacji mobilnej eShopOnContainers tworzy widok modeli i skojarzyć je z widokami, zobacz [automatyczne tworzenie modelu widoku z lokalizatorem modelu widoku](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

> [!NOTE]
> Rejestrowanie i rozpoznawania typów, z kontenerem ma wydajności koszt z powodu użycia kontenera odbicia do tworzenia poszczególnych typów, szczególnie w przypadku zależności są trwa odtworzyć dla każdego Nawigacja strony w aplikacji. Jeśli istnieje wiele lub zależności bezpośrednich koszt tworzenia znacznie wzrasta.

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>Zarządzanie okres istnienia obiektów rozwiązany

Po zarejestrowaniu typu domyślne zachowanie dla Autofac jest do utworzenia nowego wystąpienia zarejestrowanego typu zawsze typ został rozwiązany, lub gdy mechanizm zależności injects wystąpień do innych klas. W tym scenariuszu kontenera nie przechowuje odwołania do obiektu rozwiązane. Jednak podczas rejestrowania wystąpienia, domyślne zachowanie dla Autofac ma zarządzać okres istnienia obiektu jako pojedynczą. Dlatego wystąpienie pozostaje w zakresie podczas kontenera znajduje się w zakresie, a następnie zostanie usunięty, kontener wykracza poza zakres i są zbierane pamięci lub kod usuwa jawnie kontenera.

Zakres wystąpień Autofac może służyć do określania zachowania singleton dla obiekt, który tworzy Autofac z zarejestrowanych typów. Zakresy wystąpienia Autofac Zarządzanie okresy istnienia obiektu utworzone przez kontener. Domyślne wystąpienie zakres `RegisterType` jest metoda `InstancePerDependency` zakresu. Jednak `SingleInstance` zakresu, może być używany z `RegisterType` metody, tak aby kontenera tworzy i zwraca pojedyncze wystąpienie typu podczas wywoływania metody `Resolve` — metoda. Poniższy przykład kodu pokazuje, jak Autofac instrukcje tworzenia pojedyncze wystąpienie `NavigationService` klasy:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

Przy pierwszej `INavigationService` interfejsu nie zostanie rozwiązany, tworzy nowy kontener `NavigationService` obiektu i przechowuje odwołanie do niej. Na wszystkie kolejne rozdzielczości `INavigationService` interfejsu, kontener zwraca odwołanie do `NavigationService` wcześniej utworzony obiekt.

> [!NOTE]
> Zakres SingleInstance usuwa obiekty utworzone po usunięciu kontenera.

Autofac zawiera zakresy dodatkowego wystąpienia. Aby uzyskać więcej informacji, zobacz [zakresu wystąpienia](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html) na readthedocs.io.

## <a name="summary"></a>Podsumowanie

Iniekcji zależności umożliwia rozdzielenie typów konkretnych z kodu, która jest zależna od tych typów. Zwykle wykorzystuje kontener, który zawiera listę rejestracji i mapowania między interfejsami i typy abstrakcyjne i konkretne typy, które implementuje lub rozszerzyć tych typów.

Autofac umożliwia tworzenie luźno powiązanych aplikacji i oferuje wszystkie funkcje w kontenerach iniekcji zależności metody do rejestrowania mapowania typów i wystąpienia obiektów, w tym rozwiązania obiektów, zarządzanie okresy istnienia obiektu i wstrzyknąć obiekty zależne do konstruktorów obiektów, które spowodowało to rozwiązanie.


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
