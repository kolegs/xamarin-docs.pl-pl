---
title: MVVM
ms.topic: article
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: ac8cf026fba8cac565ad622dcba24834a2ea8098
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="mvvm"></a>MVVM

Środowisko dewelopera platformy Xamarin.Forms zazwyczaj polega to na tworzenie interfejsu użytkownika w języku XAML, a następnie dodanie kodem, który działa w interfejsie użytkownika. Aplikacje zostaną zmodyfikowane i zwiększa się rozmiar i zakres, mogą wystąpić problemy obsługi złożonych. Te problemy obejmują ścisłej sprzężenia między kontrolek interfejsu użytkownika i logiki biznesowej, co zwiększa koszt dokonania zmiany interfejsu użytkownika oraz trudności taki kod do testowania jednostek.

Wzorca Model-View-ViewModel (MVVM) ułatwia prawidłowo oddzielnych działalności biznesowej i prezentacji logiki aplikacji za pomocą jego interfejsu użytkownika (UI). Obsługa czyste rozdzielenie aplikacji logiki i interfejsu użytkownika pomaga rozwiązywać problemy programowanie wiele i może ułatwić testowanie aplikacji, obsługa i rozwijać. Może znacznie zwiększyć możliwości ponownego użycia kodu i umożliwia deweloperom i projektantom interfejsu użytkownika więcej łatwiej współpracować przy opracowywaniu ich odpowiednich części aplikacji.

## <a name="the-mvvm-pattern"></a>Wzorzec MVVM

Istnieją trzy podstawowe składniki we wzorcu MVVM: model, widok i model widoku. Każdy pełni różne funkcje. Rysunek 2-1 przedstawiono relacje między trzech składników.

![](mvvm-images/mvvm.png "")

**

Oprócz ustalenia obowiązki poszczególnych składników, jest również wziąć pod uwagę, jak współdziałają ze sobą. Na wysokim poziomie widok "zna" model widoku modelu widoku "zna" model, ale modelu nie rozpoznaje modelu widoku i modelu widoku nie zna widoku. W związku z tym model widoku widoku z modelu i pozwala modelu podlegać ewolucji niezależnie od tego widoku.

Korzyści wynikające ze stosowania wzorca MVVM są następujące:

-   W przypadku z istniejącej implementacji modelu, który hermetyzuje istniejących logiki biznesowej, może być trudne lub ryzykowne je zmienić. W tym scenariuszu model widoku działa jako adaptera dla klasy modeli i pozwala uniknąć wprowadzania żadnych większych zmian w kodzie modelu.
-   Deweloperzy mogą tworzyć testów jednostkowych dla widoku i modelu, bez korzystania z widoku. Testy jednostkowe dla modelu widoku może wykorzystać te same funkcje, które jest używane przez widok.
-   Aplikacji interfejsu użytkownika mogą być przeprojektowany bez modyfikowania kodu, pod warunkiem, że widok jest zaimplementowana w całości w języku XAML. W związku z tym nowej wersji widoku powinien współpracować z istniejącego modelu widoku.
-   Projektanci i deweloperzy mogą pracować niezależnie i równolegle na ich składników podczas procesu projektowania. Projektanci mogą skupić się na widoku, gdy deweloperzy mogą pracować na model widoku i składniki modelu.

Klawisz, aby efektywnie przy użyciu MVVM znajduje się w zrozumienie sposobu współczynnika kodu aplikacji do poprawne klas i zrozumieć sposób interakcji klasy. W poniższych sekcjach omówiono obowiązki poszczególnych klas we wzorcu MVVM.

### <a name="view"></a>Widok

Widok jest odpowiedzialny za definiowanie strukturę, układu i wyglądu użytkownik widzi na ekranie. W idealnym przypadku każdego widoku jest zdefiniowany w języku XAML, z ograniczoną CodeBehind niezawierającą logiki biznesowej. Jednak w niektórych przypadkach CodeBehind może zawierać logikę interfejsu użytkownika, która implementuje visual zachowania, które jest trudne do wyrażenia w języku XAML, takich jak animacji.

W aplikacji platformy Xamarin.Forms widok jest zwykle [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-pochodnych lub [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)-klasy. Jednak widoków może być reprezentowany przez szablon danych, który określa elementy interfejsu użytkownika, który ma być używany do wizualnego reprezentowania obiektu pojawi się. Szablon danych jako widok nie ma żadnych kodem i jest przeznaczony do powiązania widoku określonego typu modelu.

> [!TIP]
> Unikaj włączania i wyłączania elementy interfejsu użytkownika w CodeBehind. Upewnij się, że modele widoku są odpowiedzialne za definiowanie zmiany stanu logicznego, które mają wpływ na pewne aspekty wyświetlania widoku, takie jak czy polecenie ma być możliwe lub wskazanie, że operacja oczekuje. W związku z tym Włączanie i wyłączanie elementów interfejsu użytkownika przez powiązanie, aby wyświetlić właściwości modelu, a nie Włączanie i wyłączanie je w związane z kodem.

Istnieje kilka sposobów wykonywania kodu na model widoku w odpowiedzi na interakcje w widoku, takie jak kliknij przycisk lub wybrany element. Jeśli formant obsługuje poleceń, formantu w `Command` właściwość może być powiązany z danymi `ICommand` właściwości na model widoku. Po wywołaniu polecenia formantu kod w modelu widoku zostaną wykonane. Oprócz poleceń zachowania może zostać dołączony do obiektu w widoku i wykrywać polecenie do wywołania lub zdarzenia do wywołania. W odpowiedzi, zachowanie może następnie wywołać `ICommand` na model widoku lub metody na model widoku.

### <a name="viewmodel"></a>ViewModel

Model widoku implementuje właściwości i poleceń, których widok może tworzenia powiązań danych i powiadamia widoku wszystkie zmiany stanu za pośrednictwem zdarzenia zmiany powiadomienia. Właściwości i poleceń, które udostępnia model widoku zdefiniować funkcje, które są oferowane przez interfejs użytkownika, ale widok Określa, jak funkcja ma być wyświetlany.

> [!TIP]
> Zachowaj reakcji z operacji asynchronicznych interfejsu użytkownika. Aplikacje mobilne Zachowaj odblokowany w celu poprawy wydajności wrażenie użytkownika wątku interfejsu użytkownika. W związku z tym w modelu widoku, użyj metod asynchronicznych dla operacji We/Wy i wywoływanie zdarzeń asynchronicznie powiadomiono widoków zmiany właściwości.

Model widoku jest również odpowiedzialne za koordynację widoku interakcje z dowolnej klasy modeli, które są wymagane. Zazwyczaj jest relacji jeden do wielu między modelu widoku i klasy modelu. Model widoku może wybrać do udostępnienia klasy modelu bezpośrednio do widoku, dzięki czemu formantów w widoku można utworzyć powiązania danych bezpośrednio dla nich. W takim przypadku należy klasy modelu należy planować obsługuje powiązanie danych i zmieniać zdarzenia powiadomień.

Każdy model widoku zawiera dane z modelu w postaci można łatwo korzystać z widoku. W tym modelu widoku czasami przeprowadza konwersję danych. Wprowadzenie do konwersji danych w modelu widoku jest dobrym rozwiązaniem, ponieważ zapewnia właściwości, które można powiązać z widoku. Model widoku może na przykład połączyć wartości dwie właściwości w celu ułatwienia do wyświetlania przez widok.

> [!TIP]
> Scentralizowanie konwersji danych w warstwie konwersji. Istnieje również możliwość użycia konwerterów jako odrębne warstwa konwersji, która znajduje się między model widoku oraz widoku. Może to być konieczne, na przykład gdy danych wymaga specjalnego formatowania, który nie zapewnia model widoku.

Aby model widoku o uczestnictwie w powiązanie dwukierunkowe danych z widoku, trzeba zwiększyć jego właściwości `PropertyChanged` zdarzeń. Wyświetl modele spełnienia tego wymagania, implementując `INotifyPropertyChanged` interfejsu i wywoływanie `PropertyChanged` zdarzenie, gdy właściwość zostanie zmieniona.

Dla kolekcji, widok przyjaznego `ObservableCollection<T>` jest dostępne. Ta kolekcja implementuje powiadomień Kolekcja została zmieniona, co uwalnia dewelopera z konieczności wdrożenia `INotifyCollectionChanged` interfejsu w kolekcji.

### <a name="model"></a>Model

Klasy modeli są niewidoczne klas, które zapewniają danych aplikacji. W związku z tym modelu można traktować jako reprezentujący modelu domeny aplikacji, które zwykle zawiera model danych oraz logiki biznesowej i sprawdzania poprawności. Przykładami obiekty modelu obiektów transfer danych (DTOs), zwykły stare obiekty CLR (POCOs) i wygenerowanego jednostek i obiektów pośredniczących.

Klasy modeli są zazwyczaj używane w połączeniu z usługami lub repozytoriów, które zapewniają dostęp do danych i buforowania.

## <a name="connecting-view-models-to-views"></a>Wyświetl modele nawiązywania połączenia z widoków

Wyświetlanie modeli można podłączyć do widoków przy użyciu możliwości platformy Xamarin.Forms powiązania danych. Istnieje wiele metod, które mogą służyć do tworzenia widoków i wyświetl modele i kojarzyć je w czasie wykonywania. Tych metod można podzielić na dwie kategorie, znany jako pierwszy kompozycji widoku i kompozycji pierwszy widok modelu. Wybór między kompozycji pierwszy widok oraz przeglądanie kompozycji pierwszy model jest problem preferencji i złożoności. Jednak wszystkie podejścia współużytkować ten sam cel do widoku do modelu widoku, przypisane do właściwości BindingContext.

Skład pierwszej aplikacji z widokiem koncepcyjnie składa się z widoków, które nawiązać modele widoku, które są one zależne od. Główną zaletą tej metody jest fakt, że go łatwo utworzyć luźno powiązanych jednostki testować aplikacje ponieważ modele widoku ma zależności w widokach samodzielnie. Jest również zrozumiałe struktury aplikacji po jej visual struktury, zamiast do śledzenia realizacji kodu zrozumienie sposobu tworzenia i skojarzonych klas. Ponadto konstrukcji pierwszy widok wyrównana z platformy Xamarin.Forms nawigacji system, który jest odpowiedzialny za tworzenia stron podczas nawigacji, co sprawia, że skład pierwszej modelu widoku złożone i niewyrównanych z platformą.

Z widoku modelu skład pierwszej aplikacji koncepcyjnie składa się z modeli widoku z usługą jest odpowiedzialny za lokalizowania widoku do modelu widoku. Kompozycja pierwszy widok modelu okazać bardziej naturalne niektórych deweloperów, od momentu utworzenia widoku może być usunięte, dzięki czemu można skupić się na strukturze logicznej bez interfejsu użytkownika aplikacji. Ponadto umożliwia wyświetlanie modeli ma zostać utworzony przez inne modele widoku. Jednak ta metoda jest często złożonych i może być trudne do zrozumienia sposobu tworzenia i skojarzone różne części aplikacji.

> [!TIP]
> Zachowaj niezależne wyświetlanie modeli i widoków. Powiązanie widoków do właściwości źródła danych powinny być widok główny zależność od jego odpowiedni model widoku. W szczególności nie widoku typy referencyjne, takie jak [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) i [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), z widoku modeli. Wykonując przedstawione tu zasady, wyświetlanie modeli można przetestować osobno, w związku z tym zmniejszenie prawdopodobieństwa wady oprogramowania poprzez ograniczenie zakresu.

W poniższych sekcjach omówiono głównego sposobów nawiązywania wyświetlanie modeli widoków.

### <a name="creating-a-view-model-declaratively"></a>Tworzenie modelu widoku deklaratywnie

Najprostsza metoda jest widoku deklaratywnie tworzenia wystąpienia jego odpowiedni model widoku w języku XAML. Widok jest tworzony, odpowiedni obiekt modelu widoku również być skonstruowany. Takie podejście jest przedstawiona w poniższym przykładzie kodu:

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

Gdy [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) jest tworzone wystąpienie `LoginViewModel` jest automatycznie tworzony i ustawić jako widok [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/).

Tej konstrukcji deklaratywne i przypisanie model widoku przez widok ma tę zaletę, że jest proste, ale ma wadą wymaga konstruktora domyślnego (bezparametrowego) w modelu widoku.

### <a name="creating-a-view-model-programmatically"></a>Programowe tworzenie modelu widoku

Widok może mieć kod w pliku związanym z kodem, których wynikiem jest przypisywany do modelu widoku jego [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) właściwości. Często jest to zrobić w Konstruktorze widoku, jak pokazano w poniższym przykładzie:

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

Konstrukcji programowych i przypisania model widoku w widoku kodem ma zaletą jest proste. Główną wadą tego podejścia jest jednak, czy widok powinien udostępnić model widoku wszystkie wymagane zależności. Za pomocą kontenera iniekcji zależności może pomóc Obsługa utracić sprzężenia między widoku oraz widoku modelu. Aby uzyskać więcej informacji, zobacz [iniekcji zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

### <a name="creating-a-view-defined-as-a-data-template"></a>Tworzenie widoku zdefiniowany jako szablon danych

Widok mogą być definiowane jako szablon danych i skojarzone z typem modelu widoku. Szablony danych można zdefiniować jako zasoby lub można je zdefiniowano w tekście w formancie wyświetlające model widoku. Zawartość formantu jest wystąpienie modelu widoku, a szablon danych służy do wizualnego reprezentowania go. Ta technika jest przykład sytuacji, w którym model widoku zostanie uruchomiony najpierw następuje utworzenie widoku.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>Automatyczne tworzenie modelu widoku z lokalizatorem modelu widoku

Lokalizator modelu widoku jest niestandardowej klasy, która zarządza wystąpienia widoku modeli i ich powiązania z widokami. W aplikacji mobilnej eShopOnContainers `ViewModelLocator` klasa ma dołączona właściwość `AutoWireViewModel`, który służy do skojarzenia z widoków modeli widoku. W widoku XAML to dołączona właściwość ma wartość true, aby wskazać model widoku powinny być automatycznie podłączone do widoku, jak pokazano w poniższym przykładzie kodu:

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

`AutoWireViewModel` Właściwość jest właściwością powiązania, który jest ustawiana na wartość false, a gdy zmienia się jego wartość `OnAutoWireViewModelChanged` jest wywoływana procedura obsługi zdarzeń. Ta metoda usuwa model widoku dla widoku. Poniższy przykład kodu pokazuje, jak to osiągnąć:

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

`OnAutoWireViewModelChanged` Metoda próbuje rozpoznać przy użyciu podejścia opartych na konwencjach model widoku. Tę Konwencję zakłada się, że:

-   Wyświetl modele są w tym samym zestawie co typów widoku.
-   Widoków. Widoki podrzędna przestrzeń nazw.
-   Wyświetl modele są w. ViewModels podrzędna przestrzeń nazw.
-   Wyświetlanie nazw modelu odpowiadają nazwy widoku i kończyć "ViewModel".

Na koniec `OnAutoWireViewModelChanged` Ustawia metodę [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) typu widoku do widoku rozpoznać typu modelu. Aby uzyskać więcej informacji na temat rozpoznawania typu modelu widoku, zobacz [rozpoznawania](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution).

Takie podejście ma tę zaletę, że aplikacja ma jedną klasę odpowiedzialną za wystąpienia widoku modeli i ich połączenie z widokami.

> [!TIP]
> Użyj Lokalizator modelu widoku w celu ułatwienia podstawienia. Lokalizator modelu widoku mogą służyć jako punkt podstawienia alternatywnych implementacji zależności, takich jak jednostki danych czas testowania lub projekt.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>Trwa aktualizowanie widoków w odpowiedzi na zmiany w podstawowych wyświetlić modelu lub modelu

Wszystkie modelu widoku i modelu klasy, które są dostępne w widoku powinny implementować `INotifyPropertyChanged` interfejsu. Implementacja interfejsu w modelu widoku lub klasy modelu pozwala klasę, aby dostarczyć powiadomień o zmianach na żaden formant powiązany z danymi w widoku po zmianie wartości właściwości podstawowej.

Aplikacji powinny być zaprojektowane pod kątem prawidłowe użycie powiadomienia o zmianie właściwości, spełniając następujące wymagania:

-   Wywoływanie zawsze `PropertyChanged` zdarzeń w przypadku zmiany wartości właściwości publicznej. Zakłada się, że wywoływanie `PropertyChanged` zdarzeń może być ignorowane z powodu wiedzę na temat sposobu występuje powiązanie XAML.
-   Wywoływanie zawsze `PropertyChanged` zdarzeń dla każdego obliczone właściwości, których wartości są używane przez inne właściwości w widoku modelu lub modelu.
-   Wywoływanie zawsze `PropertyChanged` zdarzeń na końcu metody, która powoduje, że właściwość zmienić lub gdy obiekt jest znana znajdować się w stanie awaryjnym. Wywoływanie zdarzenia przerwania operacji przez synchronicznego wywoływania obsługi zdarzeń. Dzieje się w trakcie wykonywania operacji, może narazić obiekt do funkcji wywołania zwrotnego, gdy znajduje się w stanie niebezpieczne, częściowo zaktualizowany. Ponadto istnieje możliwość zmiany kaskadowych przez `PropertyChanged` zdarzenia. Zmiany w kaskadowego wymagają aktualizacji są kompletne, zanim będzie można bezpiecznie wykonać kaskadowych zmiany.
-   Nigdy nie wywoływanie `PropertyChanged` zdarzeń, jeśli właściwość nie ulega zmianie. Oznacza to, że możesz porównać starej i nowej wartości przed zgłoszeniem `PropertyChanged` zdarzeń.
-   Nigdy nie wywoływanie `PropertyChanged` zdarzeń podczas model widoku o konstruktora, jeśli są inicjowania właściwości. Formanty powiązane z danymi w widoku nie będzie masz subskrypcję do otrzymywania powiadomień o zmianach w tym momencie.
-   Nigdy nie więcej niż jeden wywoływanie `PropertyChanged` zdarzenie z tej samej argument nazwy właściwości w ramach pojedynczego wywołania synchronicznego publicznej metody klasy. Na przykład `NumberOfItems` właściwości, których magazynu zapasowego jest `_numberOfItems` pola, jeśli zwiększa — metoda `_numberOfItems` 50 razy podczas wykonywania pętli go powinna tylko podnieść powiadomienia o zmianie właściwości na `NumberOfItems` raz, właściwości Po zakończeniu wszystkich prac. Dla metod asynchronicznych podnieść `PropertyChanged` zdarzenia dla nazwy danej właściwości w każdym segmencie synchroniczne łańcucha kontynuację asynchroniczną.

Używa aplikacji mobilnej eShopOnContainers `ExtendedBindableObject` klasę, aby dostarczyć powiadomień o zmianie, które przedstawiono w poniższym przykładzie kodu:

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

Xamarin.Form w [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) klasa implementuje `INotifyPropertyChanged` interfejs, a także [ `OnPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.OnPropertyChanged/p/System.String/) metody. `ExtendedBindableObject` Klasa udostępnia `RaisePropertyChanged` metody do wywołania właściwości powiadomienie o zmianie i w ten sposób wykorzystuje funkcje udostępniane przez `BindableObject` klasy.

Pochodną klasy modelu każdego widoku w aplikacji mobilnej eShopOnContainers `ViewModelBase` klasy, która z kolei jest pochodną `ExtendedBindableObject` klasy. W związku z tym każda klasa modelu widoku używa `RaisePropertyChanged` metoda `ExtendedBindableObject` klasy w celu zapewnienia powiadomienia o zmianie właściwości. Poniższy przykład kodu pokazuje, jak eShopOnContainers aplikacja mobilna wywołuje powiadomienia o zmianie właściwości przy użyciu wyrażenia lambda:

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

Należy pamiętać, że za pomocą wyrażenia lambda w ten sposób polega na małych wydajność, ponieważ wyrażenie lambda ma zostać obliczone dla każdego wywołania. Koszt wydajności jest mała i nie będzie zwykle wpływu aplikacji, kosztów może zostać naliczona, gdy istnieje wiele powiadomień o zmianie. Zaletą tej metody jest jednak zapewnia bezpieczeństwo typów w czasie kompilacji i refaktoryzacji pomocy technicznej, gdy zmieniana jest nazwa właściwości.

## <a name="ui-interaction-using-commands-and-behaviors"></a>Korzystanie z poleceń i zachowania interakcja interfejsu użytkownika

W aplikacjach mobilnych akcje są zwykle wywoływane w odpowiedzi na akcję użytkownika, takich jak kliknięcia przycisku, który może być zaimplementowany przez tworzenie obsługi zdarzeń w pliku CodeBehind. Jednak we wzorcu MVVM odpowiedzialność wykonywania akcji spoczywa modelu widoku i należy unikać wprowadzania do kodu w CodeBehind.

Polecenia zapewniają wygodną metodą reprezentują akcje, które mogą zostać powiązane do formantów w interfejsie użytkownika. Hermetyzuj kod, który implementuje akcji i pomoc, aby zachować całkowicie niezależna od jego wizualną reprezentację w widoku. Platformy Xamarin.Forms zawiera formanty, które można deklaratywnie podłączyć do polecenia, a tych kontrolek wywoła polecenie, gdy użytkownik wchodzi w interakcję z formantem.

Zachowania również umożliwić formanty deklaratywnie będą podłączone do polecenia. Jednak zachowania może służyć do wywoływania akcji skojarzoną z zakresem zdarzeń zgłaszanych przez formant. W związku z tym zachowania adresów wiele z tych samych operacji jako polecenie włączone formantów, zapewniając wysoką elastyczność i kontrolę. Ponadto zachowania można również skojarzyć polecenie obiektów lub metod z formantami, które nie są specjalnie zaprojektowane do interakcji z poleceń.

### <a name="implementing-commands"></a>Implementacja poleceń

Wyświetl modele zwykle ujawnić właściwości polecenia, dla powiązania z widoku, które są wystąpienia obiektów, które implementują `ICommand` interfejsu. Podaj liczbę formantów platformy Xamarin.Forms `Command` właściwość, która może być dane powiązane z `ICommand` obiekt udostępniany przez model widoku. `ICommand` Interfejs definiuje `Execute` metodę, która hermetyzuje samą operacją `CanExecute` metodę, która wskazuje, czy polecenie może być wywoływany i `CanExecuteChanged` zdarzenie, które występuje, gdy zmian wpływających na czy polecenie powinno być wykonane. [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) i [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) implementuje klasy udostępniane przez platformy Xamarin.Forms, `ICommand` interfejsu, gdzie `T` jest typem argumenty `Execute`i `CanExecute`.

W ramach modelu widoku, powinien być typu obiektu [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) lub [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) dla każdej publicznej właściwości w modelu widoku typu `ICommand`. `Command` Lub `Command<T>` Konstruktor wymaga `Action` obiektu wywołania zwrotnego, które jest wywoływane, gdy `ICommand.Execute` wywołania metody. `CanExecute` Metoda jest parametr opcjonalny konstruktora i jest `Func` zwracającą `bool`.

Poniższy kod przedstawia sposób [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) wystąpienia, która reprezentuje polecenie rejestrowania, jest tworzony za pośrednictwem pełnomocnika, aby `Register` wyświetlić metody modelu:

```csharp
public ICommand RegisterCommand => new Command(Register);
```

Polecenie jest uwidaczniany w widoku za pomocą właściwości, która zwraca odwołanie do `ICommand`. Gdy `Execute` wywoływana jest metoda [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) obiektu, po prostu przekazuje wywołanie do metody w modelu widoku za pośrednictwem delegata, który został określony w `Command` konstruktora.

Za pomocą polecenia można wywołać metody asynchronicznej przy użyciu `async` i `await` słów kluczowych podczas określania polecenia `Execute` delegowanie. Oznacza to, że wywołanie zwrotne jest `Task` i powinny być oczekiwane. Na przykład poniższy kod przedstawia sposób [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) wystąpienia, która reprezentuje polecenie logowania, jest tworzony za pośrednictwem pełnomocnika, aby `SignInAsync` wyświetlić metody modelu:

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

Parametry mogą być przekazywane do `Execute` i `CanExecute` akcji za pomocą [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) klasy do uruchamiania polecenia. Na przykład poniższy kod przedstawia sposób `Command<T>` wystąpienia służy do wskazania, że `NavigateAsync` metoda wymaga argumentu typu `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

W obu [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) i [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) klas, delegat do `CanExecute` w każdym Konstruktor jest opcjonalne. Jeśli delegat nie jest określony, `Command` zwróci `true` dla `CanExecute`. Jednak model widoku może wskazywać na zmiany w tym poleceniu `CanExecute` stanu przez wywołanie metody `ChangeCanExecute` metoda `Command` obiektu. Powoduje to `CanExecuteChanged` się zdarzenia. Formanty w interfejsie użytkownika, które są powiązane z polecenia następnie zaktualizuje stan włączony na dostępności polecenia powiązane z danymi.

#### <a name="invoking-commands-from-a-view"></a>Wywoływanie polecenia z widoku

Poniższy kod przedstawia przykład sposobu [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) w `LoginView` wiąże `RegisterCommand` w `LoginViewModel` przy użyciu [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) wystąpienie:

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

Parametr polecenia można opcjonalnie zdefiniować za pomocą [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.CommandParameter/) właściwości. Typu oczekiwanego argumentu jest określona w `Execute` i `CanExecute` docelowy metody. [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Automatycznie wywoła polecenie docelowe, gdy użytkownik użyje dołączone formantu. Parametr polecenia, jeśli zostanie podana, zostanie przekazany jako argument do polecenia `Execute` delegowanie.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>Implementowanie zachowania

Zachowania Zezwalaj na funkcje, które mają zostać dodane do kontrolek interfejsu użytkownika bez konieczności podklasy je. Zamiast tego funkcji jest zaimplementowana w klasie zachowania i dołączonej do formantu, jakby było częścią samego formantu. Zachowania służą do implementowania kodu, który zwykle należy zapisać jako kodem, ponieważ bezpośrednio prowadzi interakcję z interfejsem API formantu w taki sposób, że można go zwięzłym dołączonej do formantu i pakiecie w celu ponownego wykorzystania przez więcej niż jeden widok lub aplikacji. W kontekście MVVM zachowania są przydatne podejście do połączenia formanty poleceń.

Zachowanie, które jest dołączony do formantu za pośrednictwem dołączone właściwości nosi nazwę *dołączony zachowanie*. Zachowanie można użyć interfejsu API dostępnego elementu, do której jest dołączona do dodawania funkcji do tego formantu lub inne formanty, w widoku drzewa wizualnego. Zawiera aplikacji mobilnej eShopOnContainers `LineColorBehavior` klasy, która jest dołączona zachowanie. Aby uzyskać więcej informacji dotyczących tego zachowania, zobacz [wyświetlanie błędy sprawdzania poprawności](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors).

Zachowanie platformy Xamarin.Forms jest klasą pochodzącą z [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) lub [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) klasy, gdzie `T `jest typ kontroli, do którego należy zastosować zachowanie. Podaj te klasy `OnAttachedTo` i `OnDetachingFrom` metody, które powinna zostać zastąpiona w celu zapewnienia logikę, która zostanie wykonana, jeśli zachowanie jest dołączony do i odłączone od kontroli.

W aplikacji mobilnej eShopOnContainers `BindableBehavior<T>` pochodną klasy [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) klasy. Celem `BindableBehavior<T>` klasy jest zapewnienie klasę podstawową dla platformy Xamarin.Forms zachowania, które wymagają [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) zachowania, należy ustawić dołączonych kontroli.

`BindableBehavior<T>` Klasa udostępnia możliwym do zastąpienia `OnAttachedTo` metodę, która ustawia [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) zachowania i którą można przesłonić `OnDetachingFrom` metodę, która czyści `BindingContext`. Ponadto klasa zawiera odwołanie do formantu dołączonych w `AssociatedObject` właściwości.

Obejmuje aplikacji mobilnej eShopOnContainers `EventToCommandBehavior` klasy, która wykonuje polecenie w odpowiedzi na zdarzenia występujące. Ta klasa pochodzi od `BindableBehavior<T>` klasy, tak aby zachowanie może powiązać i wykonywać `ICommand` określonego przez `Command` właściwości, gdy jest używane przez zachowanie. Poniższy kod przedstawia przykład `EventToCommandBehavior` klasy:

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

`OnAttachedTo` i `OnDetachingFrom` metody są używane do rejestrowania i wyrejestrowania program obsługi zdarzeń dla zdarzenia, zdefiniowany w `EventName` właściwości. Następnie, gdy zdarzenie jest generowane, `OnFired` wywoływana jest metoda, która wykonuje polecenie.

Zaletą korzystania z `EventToCommandBehavior` do wykonania polecenia, gdy zdarzenie jest generowane, jest, że polecenia może być skojarzony z formantami, które nie zostały zaprojektowane do interakcji z poleceń. Ponadto zostanie przeniesiony kod obsługi zdarzeń do widoku modeli, gdzie może być testowane jednostki.

#### <a name="invoking-behaviors-from-a-view"></a>Wywoływanie zachowania z widoku

`EventToCommandBehavior` Jest szczególnie przydatne podczas dołączania do formantu, który nie obsługuje polecenia polecenie. Na przykład `ProfileView` używa `EventToCommandBehavior` do wykonania `OrderDetailCommand` podczas [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) zdarzenia są generowane na [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) zamówień użytkownika, który wyświetla pokazany w poniższym kodzie:

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

W czasie wykonywania `EventToCommandBehavior` odpowie na interakcję z [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Po wybraniu elementu w `ListView`, [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) uruchomią zdarzeń, które będą wykonywane `OrderDetailCommand` w `ProfileViewModel`. Domyślnie argumenty dla zdarzenia są przekazywane do polecenia. Te dane jest konwertowana zgodnie z przekazaniem między źródłem a celem przez konwerter określone w `EventArgsConverter` właściwość, która zwraca [ `Item` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemTappedEventArgs.Item/) z `ListView` z [ `ItemTappedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemTappedEventArgs/). W związku z tym, kiedy `OrderDetailCommand` jest wykonywane, wybranego `Order` jest przekazywana jako parametr do zarejestrowanych akcji.

Aby uzyskać więcej informacji dotyczących zachowania, zobacz [zachowania](~/xamarin-forms/app-fundamentals/behaviors/index.md).

## <a name="summary"></a>Podsumowanie

Wzorca Model-View-ViewModel (MVVM) ułatwia prawidłowo oddzielnych działalności biznesowej i prezentacji logiki aplikacji za pomocą jego interfejsu użytkownika (UI). Obsługa czyste rozdzielenie aplikacji logiki i interfejsu użytkownika pomaga rozwiązywać problemy programowanie wiele i może ułatwić testowanie aplikacji, obsługa i rozwijać. Może znacznie zwiększyć możliwości ponownego użycia kodu i umożliwia deweloperom i projektantom interfejsu użytkownika więcej łatwiej współpracować przy opracowywaniu ich odpowiednich części aplikacji.

Przy użyciu MVVM wzorca interfejsu użytkownika aplikacji i podstawowej prezentacji i logiki biznesowej jest podzielone na trzy osobne klasy: widoku, który hermetyzuje interfejsu użytkownika i interfejsu użytkownika logiki; model widoku, który hermetyzuje logikę prezentacji i stan; i model, który hermetyzuje logikę biznesową i danych aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
