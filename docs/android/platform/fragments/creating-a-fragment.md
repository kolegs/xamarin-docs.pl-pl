---
title: Tworzenie fragmentu
ms.topic: article
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: c1dd3495b0d7f76197126094cfd10e50d0ca760d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-fragment"></a>Tworzenie fragmentu

Aby utworzyć Fragment, musi dziedziczyć klasa `Android.App.Fragment` , a następnie zastąpić `OnCreateView` metody. `OnCreateView` zostanie wywołana przez działanie hostingu należy umieścić Fragment na ekranie, a zwróci `View`. Typowe `OnCreateView` utworzy `View` pompowania pliku układu i dołączenie go do kontenera nadrzędnego. Właściwości kontenera są ważne, jak Android zostanie parametrów układu elementu nadrzędnego dla interfejsu użytkownika fragmentu. Poniższy przykład przedstawia to:

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

Powyższy kod będzie zwiększyć widoku `Resource.Layout.Example_Fragment`i dodaj go jako widok podrzędny do `ViewGroup` kontenera.


> [!NOTE]
> **Uwaga:** podklas fragmentu musi mieć domyślnego publicznego konstruktora nie argumentów.

## <a name="adding-a-fragment-to-an-activity"></a>Dodanie fragmentu do działania

Istnieją dwa sposoby Fragment może być obsługiwany wewnątrz działania:

-   **Deklaratywnie** &ndash; fragmenty można deklaratywnie w `.axml` układu plików za pomocą `<Fragment>` tagu.

-   **Programowo** &ndash; fragmenty można również tworzyć dynamicznie za pomocą `FragmentManager` klasy interfejsu API.

Programowy użycia za pomocą `FragmentManager` klasy zostaną dokładniej omówione w dalszej części tego przewodnika.

### <a name="using-a-fragment-declaratively"></a>Za pomocą fragmentu deklaratywnie

Dodanie fragmentu w układzie wymaga użycia `<fragment>` tag, a następnie określenia Fragment, zapewniając albo `class` atrybutu lub `android:name` atrybutu. Poniższy fragment kodu przedstawia sposób użycia `class` atrybutu, aby zadeklarować `fragment`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Ta dalej Wstawka kodu pokazano, jak zadeklarować `fragment` przy użyciu `android:name` atrybut do identyfikowania klasy fragmentu:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Podczas tworzenia działania systemu Android zostanie wystąpienia każdego fragmentu określone w pliku układu i Wstaw widok, który został utworzony na podstawie `OnCreateView` zamiast `Fragment` elementu.
Fragmenty deklaratywnie dodawanych do działania są statyczne i pozostanie w działaniu, dopóki nie zostanie on zniszczony; nie jest możliwe dynamicznie Zamień lub usuń takie fragmentu przez cały okres istnienia działania, do której jest dołączona.

Każdy fragmentu musi być przypisany unikatowy identyfikator:

-  **android: identyfikator** &ndash; z innych elementów interfejsu użytkownika w pliku układu jest unikatowy identyfikator.

-  **android: tag** &ndash; ten atrybut jest unikatowym ciągiem.

Jeśli żaden z poprzednich dwóch metod jest używana, Fragment przyjmie identyfikator Widok kontenera. W poniższym przykładzie w przypadku gdy ani `android:id` ani `android:tag` została podana, Android przypisze identyfikator `fragment_container` do fragmentu:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:id="+@id/fragment_container"
                android:orientation="horizontal"
                android:layout_width="match_parent"
                android:layout_height="match_parent">

        <fragment class="com.example.android.apis.app.TitlesFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />
</LinearLayout>
```

### <a name="package-name-case"></a>Przypadek nazwa pakietu

Android nie zezwala na wielkie litery w nazwach pakietu; go spowoduje zgłoszenie wyjątku, próbując zwiększyć widoku, jeśli nazwa pakietu zawiera wielką literę. Jednak Xamarin.Android jest więcej forgiving i będą tolerować wielkich liter w przestrzeni nazw.

Na przykład oba poniższe fragmenty kodu będzie działać z platformy Xamarin.Android. Jednak spowoduje, że druga fragment `android.view.InflateException` zostanie wygenerowany przez pure aplikacji systemu Android opartych na języku Java.

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

LUB

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>Cykl życia fragmentu

Fragmenty mają własne cyklu życia, w pewnym stopniu niezależny od, ale nadal wykorzystywanych przez [cyklu życia hostingu działania](~/android/app-fundamentals/activity-lifecycle/index.md).
Na przykład gdy wstrzymuje działanie, wszystkie jego skojarzony fragmenty są wstrzymane. Poniższy diagram przedstawia cyklu życia fragmentu.

[![Diagram przepływu pokazujący fragmentu cykl życia](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png)


### <a name="fragment-creation-lifecycle-methods"></a>Metody cyklu życia tworzenia fragmentu

Poniższa lista zawiera przepływ różnych wywołań zwrotnych w cyklu życia fragmentu, jak zostało utworzone:

-   **`OnInflate()`** &ndash; Wywoływane, gdy Fragment jest tworzony jako część układ widoku. To może być wywoływana natychmiast po Fragment deklaratywnie z pliku XML układu. Fragment nie jest jeszcze skojarzona z jego działania, ale **działania**, **pakietu**, i **AttributeSet** z widoku hierarchii są przekazane jako parametry. Ta metoda najlepiej nadaje się do analizowania **AttributeSet** i do zapisywania atrybutów, które mogą używać później fragmentu.

-   **`OnAttach()`** &ndash; Metoda wywoływana po Fragment jest skojarzona z działaniem. Jest to pierwsza metoda ma być uruchamiany przy Fragment jest gotowa do użycia. Ogólnie rzecz biorąc fragmenty nie należy implementować konstruktora lub zastąpienie domyślnego konstruktora. Wszystkie składniki, które są wymagane dla fragmentu muszą zostać zainicjowane w tej metodzie.

-   **`OnCreate()`** &ndash; Metoda wywoływana przez działanie, aby utworzyć fragmentu. Gdy ta metoda jest wywoływana, Wyświetl hierarchię hostingu działania nie mogą być całkowicie dziedziczone, więc fragmentu nie należy polegać na żadnych części hierarchii widoku działania do momentu później w cyklu życia fragmentu. Na przykład nie należy używać tej metody do wykonania wszelkich ulepszeń lub dostosowania interfejsu użytkownika aplikacji. Jest to najwcześniejszą czasu, jaką Fragment może rozpocząć zbieranie danych, które wymagają. Fragment jest uruchomiona w wątku interfejsu użytkownika w tym momencie, tak uniknąć żadnych długich przetwarzania lub przetwarzania tego wątku w tle. Ta metoda mogą być pominięte, jeśli **SetRetainInstance(true)** jest wywoływana.
    To alternatywne zostaną opisane bardziej szczegółowo poniżej.

-   **`OnCreateView()`** &ndash; Tworzy widok dla fragmentu.
    Ta metoda jest wywoływana raz działania **OnCreate()** metody została ukończona. W tym momencie jest bezpieczne wchodzić w interakcje z hierarchii widoku działania. Ta metoda powinna zwrócić widok, który będzie używany przez Fragment.

-   **`OnActivityCreated()`** &ndash; Wywoływana po wykonaniu **metodę Activity.OnCreate** zakończył przez działanie hostingu.
    Końcowe ulepszeń interfejsu użytkownika należy wykonać w tej chwili.

-   **`OnStart()`** &ndash; Metoda wywoływana po zawierające działanie zostało wznowione. Dzięki temu Fragment widoczny dla użytkownika. W wielu przypadkach Fragment będzie zawierać kod, który byłby w **OnStart()** metody działania.

-   **`OnResume()`** &ndash; Jest to ostatnia metoda wywoływana przed użytkownik może interakcyjnie fragmentu. Przykład rodzaj kodu, która powinna być wykonywana w ramach tej metody spowoduje włączenie funkcji urządzenia, które użytkownik może wchodzić w interakcje, takie jak aparatu który usług lokalizacji. Usług, takich jak te mogą spowodować nadmierne zużycie baterii, jednak i ograniczać ich użycia, aby oszczędzać czas pracy baterii aplikacji.


### <a name="fragment-destruction-lifecycle-methods"></a>Metody cyklu usuwania fragmentu

Lista dalej wyjaśniono metody cyklu życia, które są nazywane jako Fragment jest niszczony:

-   **`OnPause()`** &ndash; Użytkownik nie jest już obsługiwać interakcję z fragmentu. Taka sytuacja nie istnieje, ponieważ inna operacja Fragment jest zmodyfikowanie ten Fragment lub hostingu działanie jest wstrzymane. Możliwe jest działanie hosting ten Fragment nadal mogą być widoczne, czyli działania w fokus jest częściowo przezroczyste lub nie zajmuje pełny ekran. Podczas aktywowania tej metody jest pierwsze wskazanie, że użytkownik opuszcza fragmentu. Fragment należy zapisać zmiany.

-   **`OnStop()`** &ndash; Fragment nie jest już widoczna. Host działania może być zatrzymana lub operacji Fragment jest zmodyfikowanie w działaniu. To wywołanie zwrotne pełni tę samą funkcję jak **Activity.OnStop**.

-   **`OnDestroyView()`** &ndash; Ta metoda jest wywoływana, aby wyczyścić zasoby skojarzone z tym widokiem. Jest to, gdy widok skojarzony z Fragment został zniszczony.

-   **`OnDestroy()`** &ndash; Ta metoda jest wywoływana, gdy fragmentu nie jest już używana. Jest nadal skojarzone z działaniem, ale fragmentu nie będzie już działać. Ta metoda powinna zwolnić wszystkie zasoby, które są używane przez Fragment, takich jak [ **SurfaceView** ](https://developer.xamarin.com/api/type/Android.Views.SurfaceView/) mogą być używane dla aparatu. Ta metoda mogą być pominięte, jeśli **SetRetainInstance(true)** jest wywoływana. To alternatywne zostaną opisane bardziej szczegółowo poniżej.

-   **`OnDetach()`** &ndash; Ta metoda jest wywoływana tuż przed fragmentu nie jest już skojarzona z działaniem. Wyświetl hierarchię fragmentu już nie istnieje, a wszystkie zasoby, które są używane przez Fragment powinny zostać zwolnione w tym momencie.


### <a name="using-setretaininstance"></a>Przy użyciu SetRetainInstance

Istnieje możliwość dla fragmentu określić, czy jego powinien nie można całkowicie niszczone, jeśli działanie ma zostać utworzony ponownie. `Fragment` Klasa dostarcza metody `SetRetainInstance` w tym celu. Jeśli `true` przekazany do tej metody, po ponownym uruchomieniu działania, to samo wystąpienie elementu Fragment zostanie użyta. Jeśli tak się stanie, a następnie wszystkie metody wywołania zwrotnego zostanie wywołany, z wyjątkiem `OnCreate` i `OnDestroy` wywołania zwrotne cyklu życia. Ten proces przedstawiono na diagramie cyklu życia podanego powyżej (przez zielone linie przerywana).


## <a name="fragment-state-management"></a>Zarządzanie stanem fragmentu

Fragmenty maja zapisywanie i przywracanie stanu podczas cyklu fragmentu przy użyciu wystąpienia `Bundle`. Pakiet umożliwia fragmentu zapisać dane jako pary klucz wartość i jest przydatne w przypadku prostego danych, która nie wymaga dużej ilości pamięci. Fragment można zapisać stanu z wywołaniem do `OnSaveInstanceState`:

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

Podczas tworzenia nowego wystąpienia Fragment, stan zapisane w `Bundle` staną się dostępne w nowym wystąpieniu za pośrednictwem `OnCreate`, `OnCreateView`, i `OnActivityCreated` metody nowego wystąpienia.
W poniższym przykładzie pokazano, jak pobrać wartość `current_choice` z `Bundle`:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    if (savedInstanceState != null)
    {
        _currentCheckPosition = savedInstanceState.GetInt("current_choice", 0);
    }
}
```

Zastępowanie `OnSaveInstanceState` jest odpowiedni mechanizm zapisu danych przejściowy na Fragment zachowuje zmiany orientacji, takich jak `current_choice` wartość w powyższym przykładzie. Jednak domyślna implementacja `OnSaveInstanceState` zajmuje się zapisywania przejściowej danych w Interfejsie użytkownika dla każdego widoku, który ma identyfikator przypisany. Na przykład przyjrzeć się aplikacja, która ma `EditText` elementu zdefiniowanego w pliku XML w następujący sposób:

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

Ponieważ `EditText` formant ma `id` przypisany, Fragment automatycznie zapisuje dane w elemencie widget podczas `OnSaveInstanceState` jest wywoływana.


### <a name="bundle-limitations"></a>Ograniczenia pakietu

Mimo że używanie `OnSaveInstanceState` sprawia, że ułatwia Zapisz przejściowej danych, użyj tej metody ma następujące ograniczenia:

-  Jeśli Fragment nie została dodana do stosie przechodzenia wstecz, a następnie jego stan nie zostanie przywrócony, gdy użytkownik naciśnie **ponownie** przycisku.

-  W przypadku pakietu w celu zapisywania danych tych danych jest serializowany. Może to prowadzić do przetwarzania opóźnienia.


## <a name="contributing-to-the-menu"></a>Współtworzenie Menu

Fragmenty może przyczynić się elementy do menu ich działanie hostingu.
Działanie obsługuje najpierw elementów menu. Jeśli działanie nie ma obsługi, następnie zdarzenia zostaną przekazane do fragmentu, który następnie będzie obsługiwać.

Aby dodać elementy do menu działania, Fragment, należy wykonać dwie czynności.
Po pierwsze, Fragment musi implementować metodę `OnCreateOptionsMenu` i umieszczenie jej elementów w menu, jak pokazano w poniższym kodzie:

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

Menu w poprzednim fragment kodu jest zwiększony z następującego pliku XML, znajdujących się w pliku `menu_fragment_vehicle_list.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

Następnie należy wywołać Fragment `SetHasOptionsMenu(true)`. Wywołanie tej metody ogłasza systemu Android, że Fragment ma elementy menu przyczynić się do menu opcji. Jeśli wywołanie tej metody elementy menu dla fragmentu nie zostanie dodany do menu opcji działania. Jest to zazwyczaj wykonywane w metodzie cyklu życia `OnCreate()`, jak pokazano w następnym fragment kodu:

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

Następujący ekran pokazuje, jak będzie wyglądać menu:

[![Zrzut ekranu aplikacji My rund wyświetlanie elementów menu](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png)
