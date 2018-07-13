---
title: Wzorzec Model-View-ViewModel
description: W tym rozdziale wyjaśniono, jak korzysta z aplikacji mobilnej w ramach aplikacji eShopOnContainers wzorca MVVM na prawidłowo oddzielić logiki biznesowej i prezentacji aplikacji z jego interfejs użytkownika.
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: c947ec0c2fffbd9038ee58211c77bd947c445b6e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998445"
---
# <a name="the-model-view-viewmodel-pattern"></a>Wzorzec Model-View-ViewModel

Środowisko deweloperskie platformy Xamarin.Forms zwykle obejmuje tworzenie interfejsu użytkownika w XAML, a następnie dodając kodem, który działa w interfejsie użytkownika. Aplikacje są modyfikowane i zwiększanie się rozmiaru i zakresu, mogą wystąpić obsługi złożonych problemów. Te problemy obejmują ścisłego sprzężenia między kontrolkami interfejsu użytkownika i logikę biznesową, co zwiększa koszt wprowadzania modyfikacji interfejsu użytkownika oraz trudności testy jednostkowe takiego kodu.

Wzorzec Model-View-ViewModel (MVVM) pomaga oddzielić nie pozostawia żadnych śladów logiki biznesowej i prezentacji aplikacji za pomocą jego interfejsu użytkownika (UI). Utrzymywanie czystą separacji między logiki aplikacji i interfejsu użytkownika pozwala rozwiązać wiele problemów rozwoju i może ułatwić aplikacji do testowania, obsługa i rozwijać. Może również znacznie zwiększyć możliwości ponownego użycia kodu i umożliwia deweloperom i projektanci interfejsu użytkownika, aby łatwo współpracować podczas tworzenia ich odpowiednich części aplikacji.

## <a name="the-mvvm-pattern"></a>Wzorzec MVVM

Istnieją trzy podstawowe składniki w wzorca MVVM: model, widok i model widoku. Każdy pełni różne funkcje. Rysunek 2-1 przedstawiono relacje między trzy składniki.

![](mvvm-images/mvvm.png "Wzorzec MVVM")

**Rysunek 2-1**: wzorca MVVM

Oprócz zapoznania się z obowiązki poszczególnych składników, jest również ważne, aby zrozumieć, jak współdziałają ze sobą. Na wysokim poziomie widok "obsługującemu" model widoku modelu widoku "obsługującemu" model, ale modelu nie rozpoznaje modelu widoku i modelu widoku nie rozpoznaje tego widoku. W związku z tym model widoku widoku z modelu i pozwala modelu, który ma się rozwijać, niezależnie od tego widoku.

Korzyści z używania wzorca MVVM są następujące:

-   W przypadku istniejącej implementacji modelu, który hermetyzuje istniejącej logiki biznesowej, może być trudne lub ryzykowne, aby ją zmienić. W tym scenariuszu modelu widoku działa jako adapter dla klasy modeli i pozwala uniknąć wprowadzania żadnych większych zmian w kodzie modelu.
-   Deweloperzy mogą tworzyć testy jednostkowe dla modelu widoku i modelu, bez korzystania z widoku. Testy jednostkowe dla modelu widoku szybko sprawdzić taką samą funkcjonalność, którą podano w widoku.
-   Interfejsie użytkownika aplikacji mogą przeprojektowane bez zmiany kodu, pod warunkiem, że widok jest zaimplementowana w całości XAML. W związku z tym nowa wersja widoku powinny działać z istniejącego modelu widoku.
-   Projektanci i deweloperzy mogą pracować niezależnie i równolegle na ich składników podczas procesu projektowania. Projektanci mogą skupić się na widok, deweloperzy mogą pracować w modelu widoku i składników modelu.

Kluczem do skutecznego korzystania z modelem MVVM znajdujące się w zrozumieć, jak wziąć pod uwagę kodu aplikacji na poprawne klasy oraz opis sposobu interakcji klasy. W poniższych sekcjach omówiono obowiązki poszczególnych klas w wzorca MVVM.

### <a name="view"></a>Widok

Widok jest odpowiedzialny za zdefiniowanie struktury, układ i wygląd użytkownik zobaczy na ekranie. W idealnym przypadku każdy widok jest zdefiniowany w XAML, z ograniczoną związanym z kodem, który nie zawiera logiki biznesowej. Jednak w niektórych przypadkach związanym z kodem może zawierać logika interfejsu użytkownika, który implementuje zachowanie visual, który jest trudny do wyrażenia w XAML, takich jak animacji.

W aplikacji platformy Xamarin.Forms, widok jest zazwyczaj [ `Page` ](xref:Xamarin.Forms.Page)-pochodnych lub [ `ContentView` ](xref:Xamarin.Forms.ContentView)-klasy pochodnej. Jednak widoków może być reprezentowany przez szablon danych, który określa elementy interfejsu użytkownika, który ma być używany do reprezentowania wizualnie obiektu, gdy jest on wyświetlany. Szablon danych w widoku nie ma żadnych związanym z kodem i jest przeznaczona do powiązania w typie modelu określonego widoku.

> [!TIP]
> Unikaj włączania i wyłączania elementów interfejsu użytkownika w związanym z kodem. Upewnij się, że modeli widoków są odpowiedzialne za definiowanie zmiany stanu logicznego, które mają wpływ na pewne aspekty wyświetlania widoku, takie jak czy polecenie jest dostępne oraz wskazanie, że operacja oczekuje. W związku z tym Włącz i wyłączający elementy interfejsu użytkownika przez powiązanie, aby wyświetlić właściwości modelu, a nie Włączanie i wyłączanie je w związanym z kodem.

Istnieje kilka opcji umożliwiających wykonywanie kodu na podstawie modelu widoku w odpowiedzi na interakcje w widoku, takie jak kliknięcie przycisku lub zaznaczenie elementu. Jeśli formant obsługuje poleceń, formant firmy `Command` właściwość może być powiązany z danymi `ICommand` właściwości w modelu widoku. Po wywołaniu polecenia sterowania kod w modelu widoku zostaną wykonane. Oprócz poleceń zachowań może zostać dołączony do obiektu w widoku i może nasłuchiwać w oczekiwaniu polecenie do wywołania lub zdarzenia. W odpowiedzi, następnie wywołać działanie `ICommand` na model widoku lub metody w modelu widoku.

### <a name="viewmodel"></a>ViewModel

Model widoku implementuje właściwości i poleceń, do których widok może zostać powiązane dane, a następnie powiadamia widoku wszelkie zmiany stanu, za pomocą zdarzenia powiadomień zmiany. Właściwości i poleceń, które zapewnia model widoku zdefiniowane funkcje, które mają być oferowane przez interfejs użytkownika, ale widok Określa, jak te funkcje mają być wyświetlane.

> [!TIP]
> Zachowaj interfejsu użytkownika odpowiada za pomocą operacji asynchronicznych. Aplikacje mobilne, należy zachować wątku interfejsu użytkownika zostało odblokowane, aby zwiększyć jego punktu widzenia wydajności. W związku z tym w modelu widoku używać metod asynchronicznych operacji We/Wy i wywoływanie zdarzeń do asynchronicznego powiadomienia widoków zmiany właściwości.

Model widoku jest również odpowiedzialny za koordynowanie widoku interakcje z dowolnej klasy modeli, które są wymagane. Zazwyczaj jest relacji jeden do wielu między klasy modelu i model widoku. Model widoku może wybrać do udostępnienia klasy modelu bezpośrednio do widoku, tak aby formantów w widoku można utworzyć powiązania danych bezpośrednio do nich. W tym przypadku klasy modelu należy uwzględniać obsługuje powiązanie danych i zmieniać zdarzenia powiadomień.

Każdy model widoku zawiera dane z modelu w postaci, która widoku z łatwością mogą konsumować. W tym modelu widoku wykonuje czasami Konwersja danych. Umieszczenie tej konwersji danych w modelu widoku jest dobrym pomysłem, ponieważ udostępnia właściwości, które można powiązać z widoku. Model widoku może na przykład połączyć wartości dwie właściwości, aby ułatwić do wyświetlenia w widoku.

> [!TIP]
> Scentralizowanie konwersji danych w warstwie konwersji. Istnieje również możliwość użycia konwerterów jako osobne dane warstwy konwersji, która pośredniczy między modelu widoku i w widoku. Może to być konieczne, na przykład, gdy dane wymagają specjalne formatowanie, który nie zapewnia model widoku.

Aby model widoku do wzięcia udziału w powiązaniu danych dwukierunkowe z widokiem, trzeba zwiększyć jego właściwości `PropertyChanged` zdarzeń. Wyświetl modele spełnienia tego wymagania, implementując `INotifyPropertyChanged` interfejsu i wywoływanie `PropertyChanged` zdarzenie po zmianie właściwości.

Dla kolekcji, przyjazne widoku `ObservableCollection<T>` podano. Ta kolekcja implementuje powiadomienia kolekcji zmienione zwalniający dewelopera z konieczności implementowania `INotifyCollectionChanged` interfejsu w kolekcjach.

### <a name="model"></a>Model

Klasy modelu są klas innym niż wizualny, które zapewniają dane aplikacji. W związku z tym model można traktować jako modelu domeny aplikacji, który zwykle zawiera model danych oraz logiki biznesowej i sprawdzania poprawności. Przykładami obiekty modelu obiektów transferu danych (dto), zwykłych starych obiektów CLR (POCOs) i jednostki wygenerowany i obiekty serwerów proxy.

Klasy modelu są zwykle używane w połączeniu z usługami lub repozytoria, które hermetyzują dostęp do danych i buforowania.

## <a name="connecting-view-models-to-views"></a>Łączenie z modeli widoków do widoków

Wyświetl modele mogą połączone z widokami przy użyciu funkcji wiązania danych z zestawu narzędzi Xamarin.Forms. Istnieje wiele metod, które mogą służyć do tworzenia widoków i wyświetl modele i skojarzyć je w czasie wykonywania. Tych metod można podzielić na dwie kategorie, znany jako widok pierwszej kompozycji i kompozycji pierwszy model widoku. Wybieranie między kompozycji pierwszy widok i Wyświetl skład pierwszy model jest problem preferencji i złożoności. Jednak wszystkie podejścia współużytkować ten sam cel, czyli widok modelu widoku, przypisane do jego właściwość elementu BindingContext.

Skład pierwszej aplikacji przy użyciu widoku koncepcyjnie składa się z widoków, które łączyć się z modelami widok, do których one zależą. Podstawową zaletą tego podejścia jest fakt, że ułatwiają do konstruowania luźno powiązanych aplikacji sprawdzalnego działa zgodnie z jednostki ponieważ modele widoku mają ma zależności od siebie widoki. Jest również łatwa do zrozumienia struktury aplikacji po jej struktury efektów wizualnych, zamiast konieczności śledzić wykonywanie kodu, aby zrozumieć, jak utworzono i skojarzono klasy. Ponadto konstrukcja pierwszy widok wyrównuje przy użyciu zestawu narzędzi Xamarin.Forms nawigacji system, który jest odpowiedzialny za tworzenia stron podczas nawigacji, co sprawia, że skład pierwszy model widoku złożonego i niewyrównane z platformą.

Przy użyciu widoku modelu skład pierwszej aplikacji pod względem koncepcyjnym składa się z modeli widoków przy użyciu usługi są odpowiedzialne za lokalizowanie widok, w którym model widoku. Kompozycja pierwszy model widoku okazać bardziej naturalne, dla niektórych programistów, ponieważ tworzenie widoku może być usunięte, pozwalając firmie skoncentrować się na strukturze logicznej bez interfejsu użytkownika aplikacji. Ponadto umożliwia wyświetlanie modeli tworzoną przez innych modeli widoków. Jednak ta metoda jest często złożone i mogą stać się trudne do zrozumienia, jak utworzono i skojarzono różne części aplikacji.

> [!TIP]
> Zachowaj niezależnie od modeli widoków i widoków. Powiązanie widoków z właściwością w źródle danych powinien być widok głównej zależność od jego odpowiedni model widoku. W szczególności nie odwołują się do typach widoków, takich jak [ `Button` ](xref:Xamarin.Forms.Button) i [ `ListView` ](xref:Xamarin.Forms.ListView), z modeli widoków. Postępując zgodnie z zasadami przedstawionymi w tym temacie, można przetestować modeli widoków w izolacji, w związku z tym zmniejszenie prawdopodobieństwa usterek oprogramowania, ograniczając zakres.

W poniższych sekcjach omówiono główne sposoby łączenia modeli widoków do widoków.

### <a name="creating-a-view-model-declaratively"></a>Deklaratywne Tworzenie modelu widoku

Najprostszą metodą jest widoku deklaratywne utworzyć jego odpowiedni model widoku w XAML. Gdy widok jest konstruowany, również można konstruować odpowiedni obiekt modelu widoku. To podejście jest przedstawiona w poniższym przykładzie kodu:

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

Gdy [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) jest tworzone wystąpienie `LoginViewModel` jest automatycznie tworzony i Ustaw jako widok [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext).

Ta konstrukcja deklaratywne i przypisania model widoku w widoku ma tę zaletę, że jest proste, ale ma wadą wymaga domyślnego (bezparametrowego) konstruktora w modelu widoku.

### <a name="creating-a-view-model-programmatically"></a>Programowe tworzenie modelu widoku

Widok może mieć kod w pliku związanym z kodem, który skutkuje model widoku przypisywanych do jego [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) właściwości. Często jest to zrobić w Konstruktorze tego widoku, jak pokazano w poniższym przykładzie kodu:

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

Konstrukcja programowy i przypisania model widoku w ramach tego widoku związane z kodem ma tę zaletę, że jest proste. Główną wadą tego podejścia jest jednak, że widok musi zapewnić model widoku wszystkie wymagane zależności. Za pomocą kontenera iniekcji zależności może ułatwić utrzymanie luźne, sprzężenia między widokiem a model widoku. Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

### <a name="creating-a-view-defined-as-a-data-template"></a>Tworzenie widoku definiowane jako szablonu danych

Widok można zdefiniowana jako szablon danych i skojarzony typ modelu widoku. Szablony danych mogą być definiowane jako zasoby, lub można je zdefiniowano w tekście w kontrolce, będzie wyświetlana modelu widoku. Zawartość kontrolki jest wystąpienie modelu widoku, a szablon danych służy do wizualnie przedstawić ją. Ta technika jest przykładem sytuacji, w którym model widoku konkretyzacji najpierw następuje utworzenie widoku.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>Automatyczne tworzenie modelu widoku z lokalizatorem modelu widoku

Lokalizator modelu widoku jest klasą niestandardowej, która zarządza wystąpienia modeli widoków i ich powiązania z widokami. W ramach aplikacji eShopOnContainers aplikacji mobilnej `ViewModelLocator` klasa ma dołączoną właściwość `AutoWireViewModel`, które jest używane do skojarzenia z widoków modeli w widoku. W widoku XAML to dołączonej właściwości jest równa true, aby wskazać, model widoku powinny być automatycznie połączone do widoku, jak pokazano w poniższym przykładzie kodu:

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

`AutoWireViewModel` Właściwość jest właściwością może być powiązana, który jest inicjowany na wartość false, a kiedy zmienia się jej wartość `OnAutoWireViewModelChanged` program obsługi zdarzeń jest wywoływany. Ta metoda jest rozpoznawany jako model widoku dla widoku. Poniższy przykład kodu pokazuje, jak odbywa się to:

```csharp
private static void OnAutoWireViewModelChanged(BindableObject bindable, object oldValue, object newValue)  
{  
    var view = bindable as Element;  
    if (view == null)  
    {  
        return;  
    }  

    var viewType = view.GetType();  
    var viewName = viewType.FullName.Replace(".Views.", ".ViewModels.");  
    var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;  
    var viewModelName = string.Format(  
        CultureInfo.InvariantCulture, "{0}Model, {1}", viewName, viewAssemblyName);  

    var viewModelType = Type.GetType(viewModelName);  
    if (viewModelType == null)  
    {  
        return;  
    }  
    var viewModel = _container.Resolve(viewModelType);  
    view.BindingContext = viewModel;  
}
```

`OnAutoWireViewModelChanged` Metoda próbuje rozpoznać modelu widoku przy użyciu podejścia oparty na Konwencji. Ta konwencja założono, że:

-   Wyświetl modele są tego samego zestawu jako typy widoków.
-   Widoki są w. Podrzędna przestrzeń nazw widoków.
-   Wyświetl modele znajdują się w. Podrzędna przestrzeń nazw modele widoków.
-   Wyświetlanie modelu nazwy zgodne z nazwami widoków i kończyć się znakiem "ViewModel".

Na koniec `OnAutoWireViewModelChanged` metody ustawia [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) typu widoku widok rozwiązania typu modelu. Aby uzyskać więcej informacji o rozwiązywaniu problemów z typu modelu widoku, zobacz [rozpoznawania](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution).

Takie podejście ma tę zaletę, że aplikacja ma jedną klasę, który jest odpowiedzialny za wystąpienia modeli widoków i ich połączenie z widokami.

> [!TIP]
> Użyj Lokalizator modelu widoku w celu ułatwienia podstawienia. Lokalizator modelu widoku można również jako punkt podstawienia alternatywnych implementacji zależności, takie jak dla jednostki danych czasowych testowania lub projektu.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>Aktualizowanie widoków w odpowiedzi na zmiany w źródłowym wyświetlanie modelu lub modelu

Wszystkie modelu widoku i klasy modeli, które są dostępne do widoku powinny implementować `INotifyPropertyChanged` interfejsu. Implementowanie interfejsu w modelu widoku lub klasy modelu umożliwia, aby klasa zapewniała powiadomienia o zmianach na żadną kontrolkę powiązanych z danymi w widoku, po zmianie wartości właściwości podstawowej.

Aplikacje powinny być zaprojektowana z myślą o poprawnego użycia powiadomienie o zmianie właściwości, spełniając następujące wymagania:

-   Zawsze wywoływanie `PropertyChanged` zdarzeń w przypadku zmiany wartości właściwości publicznej. Nie należy zakładać, że wywoływanie `PropertyChanged` zdarzenia można zignorować, ze względu na wiedzy na temat sposobu powiązania w XAML występuje.
-   Zawsze wywoływanie `PropertyChanged` zdarzeń dla każdego obliczane właściwości, których wartości są używane przez inne właściwości w widoku modelu lub modelu.
-   Zawsze wywoływanie `PropertyChanged` zdarzeń na końcu metody sprawia to, że właściwość, zmienić lub gdy obiekt jest znany będzie w stanie awaryjnym. Podnoszonego zdarzenia przerwania operacji za pomocą wywołania procedur obsługi zdarzeń synchronicznie. Jeśli występuje on w trakcie wykonywania operacji, może narazić obiektu do funkcji wywołania zwrotnego, gdy jest w stanie niebezpieczne, częściowo zaktualizowany. Ponadto, istnieje możliwość kaskadowych zmiany przez `PropertyChanged` zdarzenia. Kaskadowe zmiany zwykle wymagają aktualizacji do być ukończone, zanim zmiany kaskadowych jest bezpieczne do wykonywania.
-   Nigdy nie wywoływanie `PropertyChanged` zdarzeń, jeśli właściwość nie zmienia się. Oznacza to, że można porównać stare i nowe wartości przed zgłoszeniem `PropertyChanged` zdarzeń.
-   Nigdy nie wywoływanie `PropertyChanged` zdarzeń w modelu widoku konstruktora, jeśli są inicjowanie właściwości. Formanty powiązane z danymi w widoku nie będzie subskrybowany otrzymywać powiadomienia o zmianach na tym etapie.
-   Nigdy nie więcej niż jedną wywoływanie `PropertyChanged` zdarzenie z tej samej argument nazwy właściwości w ramach pojedynczej grupy wywołanie synchronicznej metody publicznej z klasy. Na przykład, biorąc pod uwagę `NumberOfItems` właściwość jest którego zapasowy magazyn `_numberOfItems` pola, jeśli metody zwiększa `_numberOfItems` pięćdziesiąt razy w czasie wykonywania pętli go powinna tylko zgłosić powiadomienie o zmianie właściwości na `NumberOfItems` raz, właściwość Po zakończeniu całą pracę. W przypadku metod asynchronicznych podnieść `PropertyChanged` zdarzeń dla nazwy danej właściwości w każdym segmencie synchroniczne łańcucha kontynuację asynchroniczną.

Zastosowań aplikacji mobilnej w ramach aplikacji eShopOnContainers `ExtendedBindableObject` Aby klasa zapewniała powiadomienia o zmianie, która została przedstawiona w poniższym przykładzie kodu:

```csharp
public abstract class ExtendedBindableObject : BindableObject  
{  
    public void RaisePropertyChanged<T>(Expression<Func<T>> property)  
    {  
        var name = GetMemberInfo(property).Name;  
        OnPropertyChanged(name);  
    }  

    private MemberInfo GetMemberInfo(Expression expression)  
    {  
        ...  
    }  
}
```

Firmy Xamarin.Form [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) klasy implementuje `INotifyPropertyChanged` interfejsu, a także [ `OnPropertyChanged` ](xref:Xamarin.Forms.BindableObject.OnPropertyChanged(System.String)) metody. `ExtendedBindableObject` Klasa udostępnia `RaisePropertyChanged` metody do wywołania właściwości powiadomienie o zmianie i w ten sposób wykorzystuje funkcje udostępniane przez `BindableObject` klasy.

Każda klasa modelu widoku w aplikacji mobilnej w ramach aplikacji eShopOnContainers pochodzi od klasy `ViewModelBase` klasy, która z kolei pochodzi od klasy `ExtendedBindableObject` klasy. W związku z tym, korzysta z każdej klasy modelu widoku `RaisePropertyChanged` method in Class metoda `ExtendedBindableObject` Aby klasa zapewniała powiadomienie o zmianie właściwości. Poniższy przykład kodu pokazuje, jak aplikacja mobilna w ramach aplikacji eShopOnContainers wywołuje powiadomienie o zmianie właściwości przy użyciu wyrażenia lambda:

```csharp
public bool IsLogin  
{  
    get  
    {  
        return _isLogin;  
    }  
    set  
    {  
        _isLogin = value;  
        RaisePropertyChanged(() => IsLogin);  
    }  
}
```

Należy pamiętać, że użycie wyrażenia lambda w ten sposób polega na małych negatywnie na wydajność, ponieważ wyrażenie lambda ma zostać obliczone dla każdego wywołania. Mimo że spadek wydajności jest mały i nie będzie zwykle wpływu na aplikację, koszty może zostać naliczona przypadku czy wiele powiadomień o zmianie. Jednak zaletą tego podejścia jest zapewnia bezpieczeństwo typów w czasie kompilacji i refaktoryzacji pomocy technicznej podczas zmieniania nazw właściwości.

## <a name="ui-interaction-using-commands-and-behaviors"></a>Interakcja interfejsu użytkownika przy użyciu polecenia i zachowania

W aplikacjach mobilnych akcje są zwykle wywoływani w odpowiedzi na akcję użytkownika, takie jak kliknięcie przycisku, który może być implementowany przez tworzenie obsługi zdarzeń w pliku związanym z kodem. Jednak w wzorca MVVM odpowiedzialność za wykonanie akcji znajduje się za pomocą modelu widoku i należy unikać wprowadzania do kodu w związanym z kodem.

Polecenia zapewniają wygodny sposób do reprezentowania akcje, które może być powiązana z kontrolkami w interfejsie użytkownika. Hermetyzuj kod, który implementuje akcji i pomóc zachować odłączone od jego wizualnej reprezentacji w widoku. Xamarin.Forms zawiera formanty, które mogą być w sposób deklaratywny połączone z poleceniem, a te kontrolki wywoła polecenie, gdy użytkownik wchodzi w interakcję z kontrolką.

Zachowania również umożliwiać kontrolek w sposób deklaratywny podłączenie do polecenia. Jednak zachowania może służyć do wywołania akcji, która jest skojarzona z szeroką gamę zdarzeń wywołanych przez kontrolkę. W związku z tym zachowania adresów wiele z tych samych scenariuszy co polecenie włączone formantów, przy jednoczesnym zapewnieniu większego stopnia elastyczności i kontroli. Ponadto zachowania można również skojarzyć polecenia obiektów lub metod z kontrolkami, które nie zostały specjalnie zaprojektowane do interakcji z poleceniami.

### <a name="implementing-commands"></a>Implementacja poleceń

Wyświetl zazwyczaj modelach polecenie Właściwości, do powiązania w widoku wystąpienia obiektów, które implementują `ICommand` interfejsu. Podaj liczbę kontrolek zestawu narzędzi Xamarin.Forms `Command` właściwości, które mogą zawierać dane powiązane z `ICommand` obiekt udostępniany przez model widoku. `ICommand` Interfejs definiuje `Execute` metody, która hermetyzuje samą operacją `CanExecute` metody, która wskazuje, czy polecenie może być wywołana i `CanExecuteChanged` zdarzenia, które występuje, gdy tego, czy występują zmiany wpływają na polecenie powinno być wykonane. [ `Command` ](xref:Xamarin.Forms.Command) i [ `Command<T>` ](xref:Xamarin.Forms.Command) Implementowanie klas, dostarczonych przez zestawu narzędzi Xamarin.Forms `ICommand` interfejsu, gdzie `T` jest typem argumenty `Execute`i `CanExecute`.

W ramach modelu widoku powinny być obiektu typu [ `Command` ](xref:Xamarin.Forms.Command) lub [ `Command<T>` ](xref:Xamarin.Forms.Command) dla każdej publicznej właściwości w modelu widoku typu `ICommand`. `Command` Lub `Command<T>` Konstruktor wymaga `Action` obiektu wywołania zwrotnego, która jest wywoływana, gdy `ICommand.Execute` metoda jest wywoływana. `CanExecute` Metoda to parametr opcjonalny konstruktora i jest `Func` zwracającego `bool`.

Poniższy kod przedstawia sposób [ `Command` ](xref:Xamarin.Forms.Command) wystąpienia, co reprezentuje polecenie, zarejestruj się, jest tworzony przez określenie delegat `Register` przeglądać model metoda:

```csharp
public ICommand RegisterCommand => new Command(Register);
```

Polecenie jest uwidaczniany w widoku za pomocą właściwości, która zwraca odwołanie do `ICommand`. Gdy `Execute` wywoływana jest metoda [ `Command` ](xref:Xamarin.Forms.Command) obiektu, po prostu przekazuje wywołanie do metody w modelu widoku za pośrednictwem delegata, która została określona w `Command` konstruktora.

Metoda asynchroniczna może być wywoływany za pomocą polecenia przy użyciu `async` i `await` słów kluczowych podczas określania polecenia `Execute` delegować. Oznacza to, że wywołanie zwrotne jest `Task` i powinny być oczekiwana. Na przykład, poniższy kod przedstawia sposób [ `Command` ](xref:Xamarin.Forms.Command) wystąpienia, co reprezentuje polecenie logowania, jest tworzony przez określenie delegat `SignInAsync` przeglądać model metoda:

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

Parametry mogą być przekazywane do `Execute` i `CanExecute` akcji za pomocą [ `Command<T>` ](xref:Xamarin.Forms.Command) klasy w celu uruchomienia polecenia. Na przykład, poniższy kod przedstawia sposób `Command<T>` wystąpienie jest używane w celu wskazania, że `NavigateAsync` wymaga argumentu typu metody `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

W obu [ `Command` ](xref:Xamarin.Forms.Command) i [ `Command<T>` ](xref:Xamarin.Forms.Command) klasy delegata do `CanExecute` w każdym Konstruktor jest opcjonalne. Jeśli delegat nie jest określona, `Command` zwróci `true` dla `CanExecute`. Jednak model widoku można wskazać zmianę w poleceniu `CanExecute` stanu przez wywołanie metody `ChangeCanExecute` metody `Command` obiektu. Powoduje to, że `CanExecuteChanged` zdarzenia. Wszystkie formanty w interfejsie użytkownika, które są powiązane z poleceniem następnie zaktualizuje stan włączony na dostępności polecenia powiązanych z danymi.

#### <a name="invoking-commands-from-a-view"></a>Wywoływania poleceń w widoku

Poniższy kod przedstawia przykład sposobu [ `Grid` ](xref:Xamarin.Forms.Grid) w `LoginView` wiąże `RegisterCommand` w `LoginViewModel` przy użyciu [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) wystąpienie:

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

Parametr polecenia można również opcjonalnie określić przy użyciu [ `CommandParameter` ](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) właściwości. Typ oczekiwanego argumentu jest określona w `Execute` i `CanExecute` metody docelowej. [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Automatycznie wywoła polecenie docelowe, gdy użytkownik wchodzi w interakcję z kontrolką dołączone. Parametr polecenia, jeśli podany, zostanie przekazany jako argument do polecenia `Execute` delegować.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>Implementowanie zachowań

Zachowania Zezwalaj na funkcje, które mają zostać dodane do kontrolki interfejsu użytkownika bez konieczności podklasy je. Zamiast tego funkcje jest zaimplementowana w klasie zachowanie i dołączone do formantu, tak, jakby były one częścią sama kontrolka. Zachowania umożliwiają wdrożenie kodu, który będzie zazwyczaj trzeba napisać jako związany z kodem, ponieważ współpracuje bezpośrednio z interfejsem API formantu w taki sposób, że można go zwięźle dołączonej do formantu i w pakiecie do ponownego wykorzystania w więcej niż jednego widoku lub aplikacji. W kontekście MVVM zachowania są przydatne do łączenia z formantów na polecenia.

To zachowanie, który jest dołączony do formantu za pomocą właściwości dołączone jest znany jako *dołączone zachowania*. To zachowanie można następnie użyć narażonych interfejsu API elementu, do której jest dołączony, aby dodać funkcje do tej kontrolki lub inne kontrolki, w drzewie wizualnym widoku. Aplikacja mobilna w ramach aplikacji eShopOnContainers zawiera `LineColorBehavior` klasy, która jest dołączone zachowania. Aby uzyskać więcej informacji na temat tego zachowania, zobacz [wyświetlanie błędów walidacji](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors).

Zachowania zestawu narzędzi Xamarin.Forms to klasa, która jest pochodną [ `Behavior` ](xref:Xamarin.Forms.Behavior) lub [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) klasy, gdzie `T `jest typu formantu, którego powinien dotyczyć zachowanie. Te klasy oferują `OnAttachedTo` i `OnDetachingFrom` metody, które powinny zostać zastąpiona w celu zapewnić logikę, która zostanie wykonana, gdy zachowanie jest dołączony do odłączona od kontrolek.

W ramach aplikacji eShopOnContainers aplikacji mobilnej `BindableBehavior<T>` klasa pochodzi od [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) klasy. Celem `BindableBehavior<T>` klasa ma na celu dostarczenie klasę bazową dla zachowania zestawu narzędzi Xamarin.Forms, które wymagają [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) zachowania, należy ustawić dołączonych kontroli.

`BindableBehavior<T>` Klasa udostępnia możliwym do zastąpienia `OnAttachedTo` metodę, która ustawia [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) zachowanie i którą można przesłonić `OnDetachingFrom` metodę, która czyści `BindingContext`. Ponadto klasa przechowuje odwołanie do dołączonych formantu w `AssociatedObject` właściwości.

Obejmuje aplikacji mobilnej w ramach aplikacji eShopOnContainers `EventToCommandBehavior` klasy, która wykonuje polecenie w odpowiedzi na zdarzenia występujące. Ta klasa jest pochodną `BindableBehavior<T>` klasy tak, aby zachowanie może powiązać i wykonywać `ICommand` określony przez `Command` właściwości, gdy jest używane działanie. Poniższy kod przedstawia przykład `EventToCommandBehavior` klasy:

```csharp
public class EventToCommandBehavior : BindableBehavior<View>  
{  
    ...  
    protected override void OnAttachedTo(View visualElement)  
    {  
        base.OnAttachedTo(visualElement);  

        var events = AssociatedObject.GetType().GetRuntimeEvents().ToArray();  
        if (events.Any())  
        {  
            _eventInfo = events.FirstOrDefault(e => e.Name == EventName);  
            if (_eventInfo == null)  
                throw new ArgumentException(string.Format(  
                        "EventToCommand: Can't find any event named '{0}' on attached type",   
                        EventName));  

            AddEventHandler(_eventInfo, AssociatedObject, OnFired);  
        }  
    }  

    protected override void OnDetachingFrom(View view)  
    {  
        if (_handler != null)  
            _eventInfo.RemoveEventHandler(AssociatedObject, _handler);  

        base.OnDetachingFrom(view);  
    }  

    private void AddEventHandler(  
            EventInfo eventInfo, object item, Action<object, EventArgs> action)  
    {  
        ...  
    }  

    private void OnFired(object sender, EventArgs eventArgs)  
    {  
        ...  
    }  
}
```

`OnAttachedTo` i `OnDetachingFrom` metody są używane do rejestrowania i wyrejestrowania program obsługi zdarzeń dla zdarzenia, zdefiniowany w `EventName` właściwości. Następnie, gdy zdarzenie zostanie wyzwolony, `OnFired` wywoływana jest metoda, która wykonuje polecenie.

Zaletą korzystania z `EventToCommandBehavior` do wykonania polecenia, gdy zdarzenie zostanie wyzwolony, to, że polecenia mogą być skojarzone z kontrolkami, które nie zostały przeznaczone do interakcji z poleceniami. Ponadto to przenosi kod obsługi zdarzeń do modeli widoków, gdzie mogą być testowane jednostki.

#### <a name="invoking-behaviors-from-a-view"></a>Wywoływanie zachowań z widoku

`EventToCommandBehavior` Jest szczególnie przydatne w przypadku dołączania polecenie do formantu, który nie obsługuje polecenia. Na przykład `ProfileView` używa `EventToCommandBehavior` do wykonania `OrderDetailCommand` podczas [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) zdarzeń jest uruchamiana na [ `ListView` ](xref:Xamarin.Forms.ListView) zamówienia przez użytkownika, który wyświetla pokazany w poniższym kodzie:

```xaml
<ListView>  
    <ListView.Behaviors>  
        <behaviors:EventToCommandBehavior             
            EventName="ItemTapped"  
            Command="{Binding OrderDetailCommand}"  
            EventArgsConverter="{StaticResource ItemTappedEventArgsConverter}" />  
    </ListView.Behaviors>  
    ...  
</ListView>
```

W czasie wykonywania `EventToCommandBehavior` odpowie na interakcję z [ `ListView` ](xref:Xamarin.Forms.ListView). Po wybraniu elementu w `ListView`, [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) nastąpi zdarzenie, które będą wykonywane `OrderDetailCommand` w `ProfileViewModel`. Domyślnie argumenty zdarzeń dla zdarzenia są przekazywane do polecenia. Tych danych jest konwertowany, ponieważ jest przekazywana przez konwerter określony w elemencie źródłowym i docelowym `EventArgsConverter` właściwość, która zwraca [ `Item` ](xref:Xamarin.Forms.ItemTappedEventArgs.Item) z `ListView` z [ `ItemTappedEventArgs` ](xref:Xamarin.Forms.ItemTappedEventArgs). W związku z tym, kiedy `OrderDetailCommand` jest wykonywane, wybranych `Order` jest przekazywany jako parametr do zarejestrowanych akcji.

Aby uzyskać więcej informacji na temat zachowań, zobacz [zachowania](~/xamarin-forms/app-fundamentals/behaviors/index.md).

## <a name="summary"></a>Podsumowanie

Wzorzec Model-View-ViewModel (MVVM) pomaga oddzielić nie pozostawia żadnych śladów logiki biznesowej i prezentacji aplikacji za pomocą jego interfejsu użytkownika (UI). Utrzymywanie czystą separacji między logiki aplikacji i interfejsu użytkownika pozwala rozwiązać wiele problemów rozwoju i może ułatwić aplikacji do testowania, obsługa i rozwijać. Może również znacznie zwiększyć możliwości ponownego użycia kodu i umożliwia deweloperom i projektanci interfejsu użytkownika, aby łatwo współpracować podczas tworzenia ich odpowiednich części aplikacji.

Przy użyciu MVVM wzorca interfejsu użytkownika aplikacji i podstawowej logiki prezentacji i business jest dzielony na trzech osobnych klas: widoku, który hermetyzuje interfejs użytkownika i interfejsu użytkownika logiki... model widoku, który hermetyzuje logikę prezentacji i stan; i model, który hermetyzuje logikę biznesową i danych aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [Pobierz książkę elektroniczną (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [ramach aplikacji eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
