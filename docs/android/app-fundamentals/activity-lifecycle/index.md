---
title: Cykl życia działania
description: Działania są podstawowych blok tworzenia aplikacji systemu Android i istnieją w wielu innych stanów. Cykl życia działania rozpoczyna się od wystąpienia kończy zniszczenie i zawiera wiele stanów między nimi. Po zmianie stanu działania wywoływana jest metoda zdarzenia cyklu życia odpowiednie, powiadamiania o zbliżającym się zmiana stanu działania, dzięki czemu jej do wykonania kodu w celu dostosowania do tej zmiany. W tym artykule sprawdza cyklu życia działań i wyjaśniono odpowiedzialność to działanie ma podczas każdego z tych zmian stanu jako część aplikacji dobrze behaved, niezawodne.
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: f35f3e59d8b669795ade3d370894e45866cea1ff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773556"
---
# <a name="activity-lifecycle"></a>Cykl życia działania

_Działania są podstawowych blok tworzenia aplikacji systemu Android i istnieją w wielu innych stanów. Cykl życia działania rozpoczyna się od wystąpienia kończy zniszczenie i zawiera wiele stanów między nimi. Po zmianie stanu działania wywoływana jest metoda zdarzenia cyklu życia odpowiednie, powiadamiania o zbliżającym się zmiana stanu działania, dzięki czemu jej do wykonania kodu w celu dostosowania do tej zmiany. W tym artykule sprawdza cyklu życia działań i wyjaśniono odpowiedzialność to działanie ma podczas każdego z tych zmian stanu jako część aplikacji dobrze behaved, niezawodne._

## <a name="activity-lifecycle-overview"></a>Przegląd cyklu życia działania

Działania są nietypowe koncepcji programowania specyficzne dla systemu Android. W aplikacjach tradycyjnych jest zwykle statycznej metody main, który jest wykonywany w celu uruchomienia aplikacji. Z systemami Android jednak czynności są różne; Aplikacje systemu android mogą być przeprowadzane za pośrednictwem żadnego zarejestrowanego działania aplikacji. W praktyce większość aplikacji będzie miał tylko określonego działania, które jest określone jako punkt wejścia aplikacji. Jednak jeśli wystąpiła awaria aplikacji, lub zostało zakończone przez system operacyjny, systemu operacyjnego można spróbować ponownie uruchomić aplikację w ostatniej aktywności open lub miejscach w stosie poprzedniego działania.
Ponadto system operacyjny może wstrzymać działania, gdy nie są one aktywne i odzyskiwanie ich, jeśli brakuje wolnej pamięci. Dokładne brany pod uwagę muszą być wprowadzane umożliwia aplikacji poprawne przywrócenie jego stanu w przypadku, gdy działanie zostanie ponownie uruchomiony, zwłaszcza, jeśli działanie zależne od danych z poprzednich działań.

Cykl życia działania jest zaimplementowany jako kolekcja wywołania metody systemu operacyjnego w całym cyklu życia działania. Te metody umożliwiają deweloperom implementuje funkcje, które są niezbędne do spełnienia wymagań dotyczących zarządzania stanu i zasobów aplikacji.

Jest bardzo ważne dla deweloperów aplikacji do analizowania wymagań poszczególnych działań w celu ustalenia, które metody ujawnione przez cyklu życia działania muszą zostać zaimplementowane. Błąd w tym celu może spowodować niestabilność aplikacji, awarii rozrostu zasobów i być może nawet podstawowej niestabilność systemu operacyjnego.

W tym rozdziale sprawdza cyklu życia działania szczegółowo, w tym:

-  Stan działania
-  Metody cykl życia
-  Zachowywanie stanu aplikacji


Ta część zawiera także [wskazówki](~/android/app-fundamentals/activity-lifecycle/saving-state.md) zawierających praktyczne przykłady dotyczące wydajnie Zapisz stan podczas cyklu działania. Do końca w tym rozdziale powinny mieć wiedzę o cyklu życia działania i sposobie jego obsługi w aplikacji systemu Android.

## <a name="activity-lifecycle"></a>Cykl życia działania

Cykl życia działanie systemu Android obejmuje zbiór metod w klasie działania, które dostarczają dewelopera platforma zarządzania zasobów. Ta struktura umożliwia deweloperom wymagań zarządzania unikatowy stan poszczególnych działań w aplikacji i poprawnie obsługiwać zarządzanie zasobami.

### <a name="activity-states"></a>Stan działania

System operacyjny Android rozstrzyga o kolejności przetwarzania działania na podstawie ich stanu. Dzięki temu można zidentyfikować działań, które nie są już w użyciu, dzięki czemu systemu operacyjnego w celu odzyskania pamięci i zasobów systemu Android. Na poniższym diagramie przedstawiono stany, które działania można przejść przez przez jego okres istnienia:

[![Diagram stanów działania](images/image1-sml.png)](images/image1.png#lightbox)

Te stany można podzielić na 4 głównej grupy w następujący sposób:

1.  *Aktywne lub nie działają* &ndash; działania są traktowane jako aktywny lub uruchomiony, jeśli na pierwszym planie, nazywany również ze szczytu stosu działania. To jest uznawany za działanie najwyższy priorytet w systemie Android i jako taki zostaną tylko skasowane przez system operacyjny, w ekstremalnych przypadkach, takie tak, jakby działania próbuje użyć więcej pamięci niż jest dostępne na urządzeniu, ponieważ może to spowodować interfejsu użytkownika przestać odpowiadać.

1.  *Wstrzymane* &ndash; przejdzie w stan uśpienia lub działanie jest nadal widoczny, ale częściowo ukryty przez działanie nowych, wielkości pełnego lub przezroczystego, jest uznawany za działanie wstrzymana. Wstrzymanie działania są wciąż jest aktywny, oznacza to, że ich obsługa wszystkich informacji o stanie i element członkowski i pozostają dołączony do Menedżera okien. To jest traktowany jako drugi najwyższy priorytet działania w systemie Android i działa w taki sposób, tylko zostaną skasowane przez system operacyjny, jeśli skasowanie tego działania będzie spełniać wymagania dotyczące zasobów potrzebnych do zachowania działania aktywny/uruchomiony stabilny i aktywny.

1.  *Zatrzymano Backgrounded* &ndash; zatrzymana lub w tle są traktowane jako działania, które są całkowicie zasłonięty przez innego działania.
    Zatrzymania działania nadal próbować zachować informacje o ich stan i element członkowski tak długo, jak to możliwe, ale zatrzymania działania są traktowane jako być najniższy priorytet trzy stany, i tak, system operacyjny zostanie zakończenia działania w tym stanie, aby najpierw spełniają zasobu wymagania dotyczące działań wyższy priorytet.

1.  *Ponowne uruchomienie* &ndash; jest możliwe działanie, które jest w dowolnym z Wstrzymano do zatrzymania w cyklu życia, aby były usuwane z pamięci przez system Android. Użytkownik nawiguje wstecz do działania, które go musi zostać ponownie uruchomione, przywrócony do stanu wcześniej zapisany, a następnie wyświetlane użytkownikowi.


### <a name="activity-re-creation-in-response-to-configuration-changes"></a>Ponowne tworzenie działania w odpowiedzi na zmiany w konfiguracji

Aby jeszcze bardziej skomplikowane, Android zgłasza jednego klucza więcej w mieszanym o nazwie zmian konfiguracji. Zmiany konfiguracji są szybkie działanie zniszczenie/ponownie-creation cykle występujących podczas działania zmiany w konfiguracji, np. gdy urządzenie jest [obrócony](~/android/app-fundamentals/handling-rotation.md) (i działania musi pobrać ponowne skompilowanie w orientacji poziomej lub pionowej tryb), gdy klawiatury jest wyświetlany (działania jest udostępniana opcja zmiany rozmiaru sam), lub jeśli urządzenie znajduje się w doku, między innymi.

Zmiany konfiguracji nadal powodują te same zmiany stanu działania, które wystąpiłyby podczas zatrzymanie i ponowne uruchomienie działania. Jednak aby mieć pewność, że aplikacja tak reakcji i wykonuje podczas zmiany konfiguracji, jest ważne, czy one zostać obsłużone tak szybko jak to możliwe. W związku z tym Android ma określonych interfejsu API, który może służyć do utrwalić stanu zmiany konfiguracji.
Omówione zostaną następujące czynności to później w [Zarządzanie stanu w całym cyklu](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle) sekcji.

### <a name="activity-lifecycle-methods"></a>Działanie metody cykl życia

Zestaw SDK systemu Android i przez rozszerzenie, Xamarin.Android framework dostarczają zaawansowanego modelu zarządzaniu stanem działań w ramach aplikacji. Podczas zmiany stanu działania działania jest powiadamiany przez system operacyjny, który wywołuje metody określonej dla danego działania. Na poniższym diagramie przedstawiono te metody w stosunku do cyklu życia działania:

[![Schemat blokowy cyklu życia działania](images/image2-sml.png)](images/image2.png#lightbox)

Deweloper może obsługiwać zmiany stanu przez zastąpienie tych metod w ramach działania. Należy jednak pamiętać, że wszystkie metody cyklu życia są wywoływane w wątku interfejsu użytkownika i blokuje systemu operacyjnego z wykonaniem następnego elementu pracy interfejsu użytkownika, takie jak ukrywanie bieżące działanie wyświetlanie nowego działania itd. Tak kod w tych metod powinien być zwięzły możliwe aplikacji możesz również wykonywania. Wszystkie długotrwałe zadania powinna zostać wykonana na wątku w tle.

Przeanalizujmy każdej z tych metod cykl życia i ich użycia:

#### <a name="oncreate"></a>OnCreate

[OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) jest pierwsza metoda wywoływana, gdy działanie jest tworzone.
`OnCreate` zawsze jest zastąpiona w celu wykonania dowolnego inicjowanie uruchamiania, które mogą być wymagane przez działanie takich jak:

-  Tworzenie widoków
-  Inicjowanie zmiennych
-  Powiązanie danych statycznych z listy


`OnCreate` przyjmuje [pakietu](https://developer.xamarin.com/api/type/Android.OS.Bundle/) parametr, który jest słownika do przechowywania i przekazanie informacji o stanie i obiektów między działaniami, jeśli pakiet nie jest równa null, oznacza to działanie jest uruchamiana ponownie i należy przywrócić jego stanu z poprzednie wystąpienie. Poniższy kod ilustruje sposób pobierania wartości z pakietu:

```csharp
protected override void OnCreate(Bundle bundle)
{
   base.OnCreate(bundle);

   string intentString;
   bool intentBool;

   if (bundle != null)
   {
      intentString = bundle.GetString("myString");
      intentBool = bundle.GetBoolean("myBool");
   }

   // Set our view from the "main" layout resource
   SetContentView(Resource.Layout.Main);
}
```

Raz `OnCreate` jest zakończone, wywoła Android `OnStart`.

#### <a name="onstart"></a>OnStart

[OnStart](https://developer.xamarin.com/api/member/Android.App.Activity.OnStart/) zawsze jest wywoływana przez system po `OnCreate` zostało zakończone. Działania mogą przesłaniać tę metodę, jeśli są niezbędne do wykonywania określonych zadań prawa przed działanie staje się widoczna, takich jak odświeżyć bieżące wartości widokach w ramach działania. Android wywoła `OnResume` natychmiast po tej metody.

#### <a name="onresume"></a>OnResume

Wywołania systemowe [OnResume](https://developer.xamarin.com/api/member/Android.App.Activity.OnResume/) podczas działania jest gotowy do uruchomienia, interakcji z użytkownikiem.
Działania powinny przesłaniać tę metodę, aby wykonywać zadania, takie jak:

-  Rozwijanie szybkość klatek (typowe zadania w budynku gier)
-  Uruchamianie animacji
-  Nasłuchiwanie GPS aktualizacji
-  Wyświetlanie alertów odpowiednich lub okien dialogowych
-  Połączenie się procedury obsługi zdarzeń zewnętrznych


Na przykład poniższy fragment kodu pokazano, jak zainicjować aparacie:

```csharp
public void OnResume()
{
    base.OnResume(); // Always call the superclass first.

    if (_camera==null)
    {
        // Do camera initializations here
    }
}
```

`OnResume` jest ważne, ponieważ każde działanie jest przeprowadzane w `OnPause` powinno być gotowe, które nie zostały w `OnResume`, ponieważ jest ona tylko metody cyklu życia, która może wykonać po `OnPause` podczas przełączania działanie do użytkowania.

#### <a name="onpause"></a>OnPause

[OnPause](https://developer.xamarin.com/api/member/Android.App.Activity.OnPause/) jest wywoływane, gdy system ma umieścić działania w tle lub działania staje się częściowo zasłonięty. Działania powinny przesłaniać tę metodę w razie potrzeby:

-   Zatwierdzenia niezapisane zmiany w trwałych danych

-   Zniszcz lub wyczyścić inne obiekty, które korzysta z zasobów

-   Zmniejszać szybkość klatek i wstrzymywanie animacji

-   Wyrejestruj procedury obsługi zdarzeń zewnętrznych lub obsługi powiadomień, (tj. te, które są powiązane z usługą). Należy to zrobić, aby zapobiec przecieki pamięci działania.

-   Podobnie, jeśli działanie ma wyświetlany żadnych okien dialogowych ani alerty one musi zostać wyczyszczone z `.Dismiss()` metody.

Na przykład poniższy fragment kodu opublikuje aparatu, ponieważ działanie nie może korzystać z niego podczas wstrzymania:

```csharp
public void OnPause()
{
    base.OnPause(); // Always call the superclass first

    // Release the camera as other activities might need it
    if (_camera != null)
    {
        _camera.Release();
        _camera = null;
    }
}
```

Istnieją dwie metody możliwe cyklu życia, które będą wywoływane po `OnPause`:

1.  `OnResume` zostanie wywołana, jeśli działanie jest zwracana do na pierwszym planie.
1.  `OnStop` będzie można wywołać, jeśli działanie jest umieszczany w tle.


#### <a name="onstop"></a>OnStop

[OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) jest wywoływane, gdy działanie nie jest już widoczny dla użytkownika. Dzieje się tak, gdy wystąpi jedno z następujących czynności:

-  Nowe działanie zostało uruchomione i pokrywa się to działanie.
-  Istniejące działanie jest przeznaczone do pierwszego planu.
-  Trwa niszczenie działania.


`OnStop` nie zawsze można wywołać w sytuacjach ilości pamięci, takie jak kiedy Android jest zagłodzone dla zasobów i nie można prawidłowo w tle działania. Z tego powodu najlepiej nie zależą od `OnStop` pobierania o nazwie podczas przygotowywania działania zniszczenia. Dalej metody cyklu życia, które można wywołać po zostaną tego `OnDestroy` jeśli mają działanie, lub `OnRestart` , gdy działanie pochodzi wstecz do interakcji z użytkownikiem.

#### <a name="ondestroy"></a>OnDestroy

[OnDestroy](https://developer.xamarin.com/api/member/Android.App.Activity.OnDestroy/) końcowego metoda, która jest wywoływana dla wystąpienia działania przed zniszczone i całkowicie usunięte z pamięci. W sytuacjach ekstremalnych Android mogą kasowanie procesu aplikacji, obsługujący działania, co spowoduje `OnDestroy` nie wywołana. Większość działań nie zaimplementować tę metodę, ponieważ większość wyczyścić i zamykania zostało wykonane `OnPause` i `OnStop` metody. `OnDestroy` Metoda jest zwykle zastąpiona. Aby wyczyścić long uruchomiono zasoby, które mogą wyciek zasobów. Na przykład może być wątki w tle, które zostały uruchomione w `OnCreate`.

Nie będzie żadnych metod cyklu życia wywołuje się po zniszczeniu działania.

#### <a name="onrestart"></a>OnRestart

[OnRestart](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestart/) jest wywoływana po Twoje działanie zostało zatrzymane, przed jego ponownego uruchomienia. Dobrym przykładem jest po naciśnięciu przycisku Strona główna znajduje się w działaniu w aplikacji. W takim przypadku `OnPause` , a następnie `OnStop` metody są wywoływane i działanie zostanie przeniesiony do tła, ale nie zostanie zniszczony. Jeśli użytkownik, a następnie przywrócić aplikacji za pomocą Menedżera zadań lub podobnej aplikacji, wywoła Android `OnRestart` metody działania.

Istnieją nie ogólne wytyczne, dla jakiego rodzaju logiki powinny być implementowane w `OnRestart`. Jest to spowodowane `OnStart` jest wywoływane zawsze, niezależnie od tego, czy działanie jest tworzony lub uruchamiany ponownie, więc wszystkie zasoby wymagane przez działanie powinny zostać zainicjowane w `OnStart`, a nie `OnRestart`.

Next — metoda cyklu życia wywoływana po wykonaniu `OnRestart` będzie `OnStart`.

### <a name="back-vs-home"></a>Utwórz kopię programu vs. Home

Wiele urządzeń z systemem Android ma dwie różne przyciski: przycisk "Wstecz", a przycisk "Home". Na przykład widać na poniższym zrzucie ekranu systemu Android to 4.0.3:

[![Strona główna przycisków Wstecz i](images/image4-sml.png)](images/image4.png#lightbox)

Istnieje niewielka różnica między przyciskami, mimo że pojawią się one mieć ten sam efekt umieszczania aplikacji w tle. Gdy użytkownik kliknie przycisk Wstecz, ich informuje Android czy są one wykonywane działania. Android zniszczy działania. Z kolei, gdy użytkownik kliknie przycisk Strona główna działania jest jedynie umieszczone w tle &ndash; Android nie zostanie zakończenia działania.

<a name="Managing_State_Throughout_the_Lifecycle" />

## <a name="managing-state-throughout-the-lifecycle"></a>Stan zarządzania przez cały cykl życia

Gdy działanie jest zatrzymana lub zniszczony system umożliwia zapisywanie stanu działania na nowsze Preparaty nawadniające.
To jest zapisany stan nazywa się stan wystąpienia. Android zawiera trzy opcje do przechowywania stanu wystąpienia podczas cyklu działania:

1. Przechowywanie wartości pierwotnych w `Dictionary` znany jako [pakietu](https://developer.xamarin.com/api/type/Android.OS.Bundle/) Android zostanie umożliwia zapisanie stanu.

1. Tworzenie niestandardowej klasy, które będą przechowywane wartości złożonych, takie jak map bitowych. Android będzie używać tej klasy niestandardowe można zapisać stanu.

1. Obejście cyklu życia zmiany konfiguracji przy założeniu, że ukończenie odpowiedzialność za utrzymanie stanu w działaniu.


W tym przewodniku dotyczą dwóch pierwszych opcji.



### <a name="bundle-state"></a>Stan pakietu

Podstawowy opcji zapisywania stan wystąpienia jest użycie obiekt słownika klucza i wartości, znany jako [pakietu](https://developer.xamarin.com/api/type/Android.OS.Bundle/).
Odwołania, które po utworzeniu działania, które `OnCreate` metody jest przekazywany pakietu jako parametru, tego pakietu może być używany w celu przywrócenia stanu wystąpienia. Nie zaleca się używania pakietu dla bardziej złożonych danych nie będzie szybko lub łatwo serializować klucz/wartość pary (na przykład map bitowych); zamiast tej opcji należy używać prostego wartości podobnie jak ciągów.

Działanie udostępnia metody do zapisywania i pobierania stanu wystąpienia w pakiecie:

-   [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) &ndash; to jest wywoływana przez system Android, gdy trwa niszczenie działania. Działania można zaimplementować tę metodę, jeśli muszą zachować wszystkie elementy stanu klucza i wartości.

-   [OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) &ndash; to jest wywoływana po wykonaniu `OnCreate` metoda została zakończona i zapewnia możliwość innego działania w celu przywrócenia jej stanu, po zakończeniu inicjowania.

Na poniższym diagramie przedstawiono, jak są używane następujące metody:

[![Schemat blokowy stanów pakietu](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>OnSaveInstanceState

[OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) będzie można wywołać, ponieważ działanie jest zatrzymywana. Odbieranie działania mogą przechowywać stanu w parametr pakietu. Podczas urządzenia napotyka zmianę konfiguracji, można użyć działania `Bundle` obiekt, który jest przekazywany w celu zachowania stanu działania przez zastąpienie `OnSaveInstanceState`. Rozważmy na przykład następujący kod:

```csharp
int c;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  this.SetContentView (Resource.Layout.SimpleStateView);

  var output = this.FindViewById<TextView> (Resource.Id.outputText);

  if (bundle != null) {
    c = bundle.GetInt ("counter", -1);
  } else {
    c = -1;
  }

  output.Text = c.ToString ();

  var incrementCounter = this.FindViewById<Button> (Resource.Id.incrementCounter);

  incrementCounter.Click += (s,e) => {
    output.Text = (++c).ToString();
  };
}
```

Powyższy kod zwiększa liczbą całkowitą o nazwie `c` podczas przycisk o nazwie `incrementCounter` kliknięciu wyświetlania wyniku w `TextView` o nazwie `output`. W przypadku zmiany konfiguracji — na przykład podczas obracania urządzenia — powyżej kodu spowoduje utratę wartość `c` ponieważ `bundle` będzie `null`, jak pokazano na poniższej ilustracji:

[![Ekranie nie są wyświetlane poprzedniej wartości](images/07-sml.png)](images/07.png#lightbox)

Aby zachować wartość `c` w tym przykładzie działanie można zastąpić `OnSaveInstanceState`, zapisywanie wartości w pakiecie, jak pokazano poniżej:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

Teraz podczas obracania urządzenia do nowego orientacji, liczba całkowita jest zapisywany w pakiecie i są pobierane z wierszem:

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> Jest ważne, aby zawsze wywołania Podstawowa implementacja `OnSaveInstanceState` , dzięki czemu można zapisać stanu hierarchii widoku.



##### <a name="view-state"></a>Stan widoku

Zastępowanie `OnSaveInstanceState` jest odpowiedni mechanizm zapisywania danych przejściowej w działaniu między zmiany orientacji, takie jak licznik w powyższym przykładzie. Jednak domyślna implementacja `OnSaveInstanceState` zajmie się zapisywania przejściowej danych w Interfejsie użytkownika dla każdego widoku tak długo, jak długo każdy widok ma identyfikator przypisany. Na przykład aplikacja ma `EditText` elementu zdefiniowanego w pliku XML w następujący sposób:

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

Ponieważ `EditText` formant ma `id` przypisany, gdy użytkownik wprowadza pewne dane i obraca się urządzenie, dane są nadal wyświetlane, jak pokazano poniżej:

[![Dane zostaną zachowane w trybie krajobraz](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>OnRestoreInstanceState

[OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) zostanie wywołana po `OnStart`. Udostępnia możliwość przywracania każdy stan, który został wcześniej zapisany do pakietu podczas poprzedniej działania `OnSaveInstanceState`. Jest to tego samego pakietu, który został dostarczony do `OnCreate`, jednak.

Poniższy kod przedstawia sposób można przywrócić stan w `OnRestoreInstanceState`:

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

Ta metoda istnieje udzielać elastycznością wokół stan ma zostać przywrócone. Czasami jest bardziej odpowiednia poczekać, aż wszystkie inicjalizacji są wykonywane przed przywróceniem stan wystąpienia. Ponadto podklasą klasy istniejącego działania może być tylko można przywrócić niektórych wartości ze stanu wystąpienia. W wielu przypadkach nie jest konieczne w celu zastąpienia `OnRestoreInstanceState`, ponieważ większość działań można przywrócić stan za pomocą pakiet dostarczony w celu `OnCreate`.

Przykład Zapisywanie stanu przy użyciu `Bundle`, zapoznaj się [wskazówki — stan zapisywania działania](saving-state.md).


#### <a name="bundle-limitations"></a>Ograniczenia pakietu

Mimo że `OnSaveInstanceState` umożliwia łatwe do zapisania danych przejściowa ma następujące ograniczenia:

-   Nie jest ona wywoływana we wszystkich przypadkach. Na przykład naciśnięcie przycisku **Home** lub **ponownie** aby zakończyć działanie nie spowoduje `OnSaveInstanceState` wywoływane.

-   Przekazany pakiet `OnSaveInstanceState` nie jest przeznaczony dla dużych obiektów, takich jak obrazy. W przypadku dużych obiektów zapisywania obiektu z [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) jest preferowana, zgodnie z opisem poniżej.

-   Dane zapisane za pomocą pakietu jest serializowana, a to może prowadzić do opóźnienia.

Stan pakietu jest przydatne w przypadku prostego danych, który nie używa dużej ilości pamięci, podczas gdy *dane wystąpienia-konfiguracji* jest przydatne w przypadku bardziej skomplikowanych danych lub danych, która jest kosztowna pobrać, na przykład wywołanie usługi sieci web lub zbyt skomplikowany, zapytanie bazy danych. Dane wystąpienia konfiguracji nie pobiera zapisane w obiekcie, zgodnie z potrzebami. Następna sekcja zawiera wprowadzenie `OnRetainNonConfigurationInstance` sposób zachowania bardziej złożone typy danych za pośrednictwem zmiany konfiguracji.


### <a name="persisting-complex-data"></a>Przechowywanie danych złożonych

Oprócz trwałych danych w pakiecie, Android obsługuje zapisywanie danych również przez zastąpienie [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) i zwracanie wystąpienia `Java.Lang.Object` zawierający dane do innej witryny. Istnieją dwie podstawowe zalety używania `OnRetainNonConfigurationInstance` można zapisać stanu:

-   Obiekt zwrócony z `OnRetainNonConfigurationInstance` wykonuje również przy użyciu typów większych i bardziej skomplikowanych danych, ponieważ ten obiekt zachowuje pamięć.

-   `OnRetainNonConfigurationInstance` Metoda jest wywoływane na żądanie i tylko wtedy, gdy potrzebne. Jest to bardziej ekonomiczne niż przy użyciu ręczne pamięci podręcznej.

Przy użyciu `OnRetainNonConfigurationInstance` jest odpowiednie w scenariuszach, w których jest kosztowne do pobierania danych wiele razy, takie jak wywołania usługi sieci web. Rozważmy na przykład następujący kod, który wyszukuje Twitter:

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);
    SearchTwitter ("xamarin");
  }

  public void SearchTwitter (string text)
  {
    string searchUrl = String.Format("http://search.twitter.com/search.json?" + "q={0}&rpp=10&include_entities=false&" + "result_type=mixed", text);

    var httpReq = (HttpWebRequest)HttpWebRequest.Create (new Uri (searchUrl));
    httpReq.BeginGetResponse (new AsyncCallback (ResponseCallback), httpReq);
  }

  void ResponseCallback (IAsyncResult ar)
  {
    var httpReq = (HttpWebRequest)ar.AsyncState;

    using (var httpRes = (HttpWebResponse)httpReq.EndGetResponse (ar)) {
      ParseResults (httpRes);
    }
  }

  void ParseResults (HttpWebResponse httpRes)
  {
    var s = httpRes.GetResponseStream ();
    var j = (JsonObject)JsonObject.Load (s);

    var results = (from result in (JsonArray)j ["results"] let jResult = result as JsonObject select jResult ["text"].ToString ()).ToArray ();

    RunOnUiThread (() => {
      PopulateTweetList (results);
    });
  }

  void PopulateTweetList (string[] results)
  {
    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
  }
}
```

Ten kod pobiera wyniki z sieci web w formacie JSON, analizuje je, a następnie wyników na liście, jak pokazano na poniższym zrzucie ekranu:

[![Wyniki wyświetlane na ekranie](images/06-sml.png)](images/06.png#lightbox)

Po zmianie konfiguracji — na przykład podczas obracania urządzenia - kod powtarza proces. Do ponownego wykorzystania pierwotnie pobrany wyników i nie powodują wywołania zbędne, nadmiarowe sieci, możemy użyć `OnRetainNonconfigurationInstance` zapisać wyniki, jak pokazano poniżej:

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  TweetListWrapper _savedInstance;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    var tweetsWrapper = LastNonConfigurationInstance as TweetListWrapper;

    if (tweetsWrapper != null) {
      PopulateTweetList (tweetsWrapper.Tweets);
    } else {
      SearchTwitter ("xamarin");
    }

    public override Java.Lang.Object OnRetainNonConfigurationInstance ()
    {
      base.OnRetainNonConfigurationInstance ();
      return _savedInstance;
    }

    ...

    void PopulateTweetList (string[] results)
    {
      ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
      _savedInstance = new TweetListWrapper{Tweets=results};
    }
}
```

Teraz podczas obracania urządzenia oryginalne wyniki są pobierane z `LastNonConfiguartionInstance` właściwości. W tym przykładzie wyników składają się z `string[]` zawierający tweetów. Ponieważ `OnRetainNonConfigurationInstance` wymaga, aby `Java.Lang.Object` zwrócone, `string[]` opakowane w klasie tej podklasy `Java.Lang.Object`, jak pokazano poniżej:

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

Na przykład podjęto próbę użycia `TextView` jako obiektu zwróconego z `OnRetainNonConfigurationInstance` będzie wyciek działania, jak pokazano w poniższym kodzie:

```csharp
TextView _textView;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  var tv = LastNonConfigurationInstance as TextViewWrapper;

  if(tv != null) {
    _textView = tv;
    var parent = _textView.Parent as FrameLayout;
    parent.RemoveView(_textView);
  } else {
    _textView = new TextView (this);
    _textView.Text = "This will leak.";
  }

  SetContentView (_textView);
}

public override Java.Lang.Object OnRetainNonConfigurationInstance ()
{
  base.OnRetainNonConfigurationInstance ();
  return _textView;
}
```

W tej sekcji opisano sposób przechowywania danych o stanie prostego z `Bundle`, i utrwalić bardziej złożone typy danych z `OnRetainNonConfigurationInstance`.

## <a name="summary"></a>Podsumowanie

Cykl życia działanie systemu Android zapewnia zaawansowane platformę dla zarządzania stanem działań w ramach aplikacji, ale może być trudnych do poznanie i wdrożenie. W tym rozdziale wprowadzono różne stany przejść przez działanie podczas jego okresu istnienia, a także metody cyklu życia, które są skojarzone z tych stanów. Następnie został wskazówki dotyczące rodzaj logiki powinny być wykonywane w każdej z tych metod.


## <a name="related-links"></a>Linki pokrewne

- [Obsługa obrotu](~/android/app-fundamentals/handling-rotation.md)
- [Działanie systemu android](https://developer.xamarin.com/api/type/Android.App.Activity/)
