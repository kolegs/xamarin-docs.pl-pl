---
title: Odzyskiwanie pamięci
ms.topic: article
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: e27e9577957229f347b217a8920eac239799da15
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/28/2018
---
# <a name="garbage-collection"></a>Odzyskiwanie pamięci

Xamarin.Android używa w Mono [proste pokoleniowej modułu zbierającego elementy bezużyteczne](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/). To jest moduł zbierający elementy bezużyteczne znaku i odchylenia z dwie generacje i *miejsca dużego obiektu*, z dwa rodzaje kolekcji: 

-   Kolekcje pomocnicze (zbiera Gen0 sterty) 
-   Kolekcje głównych (stosów miejsce zbiera Gen1 i dużego obiektu). 

> [!NOTE]
> W przypadku braku jawne kolekcji za pomocą [GC. Collect()](xref:System.GC.Collect) kolekcje są *na żądanie*, oparte na alokacji sterty. *To nie jest zliczanie systemu*; obiekty *nie zostaną zebrane, jak są żadnych oczekujących odwołań*, lub gdy zakres został zakończony. Wykaz Globalny zostanie uruchomiony, gdy pomocnicza sterty zabrakło pamięci dla nowej alokacji. Jeśli nie ma żadnych alokacji, nie będzie działał.


Drobne kolekcje są tanie i częste i służą do łączenia obiektów ostatnio przydzielone i martwy. Drobne kolekcje są wykonywane po co kilka MB przydzielonych obiektów. Kolekcje pomocnicza może być wykonana ręcznie przez wywołanie metody [GC. Zbieraj (0)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

Kolekcje głównych jest kosztowne i mniej częste i służą do odzyskania wszystkie obiekty martwy. Główne kolekcje są wykonywane po wyczerpaniu pamięci dla bieżącego rozmiaru sterty (przed zmianą rozmiaru sterty). Główne kolekcje może być wykonana ręcznie przez wywołanie metody [GC. Zbieraj ()](xref:System.GC.Collect) , przez wywołanie [GC. Zbieraj (int)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) z argumentem [GC. MaxGeneration](xref:System.GC.MaxGeneration). 



## <a name="cross-vm-object-collections"></a>Maszyna wirtualna między kolekcje obiektów

Istnieją trzy kategorie typów obiektów.

-   **Zarządzane obiekty**: typy, które są *nie* dziedziczyć [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) , np. [System.String](xref:System.String). 
    Są one zebrane zwykle przez wykaz Globalny. 

-   **Obiekty Java**: typy Java, które znajdują się w obrębie maszyny Wirtualnej w czasie wykonywania dla systemu Android, ale nie są widoczne dla maszyny Wirtualnej Mono. Są to nudnych które nie omówiono dalej. Są one zbierane zwykle przez środowisko wykonawcze systemu Android maszyny Wirtualnej. 

-   **Obiekty równorzędne**: typów implementujących [IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) , np. wszystkie [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) i [Java.Lang.Throwable](https://developer.xamarin.com/api/type/Java.Lang.Throwable/) podklasy. Wystąpień tych typów ma dwie "halfs" *zarządzanego elementu równorzędnego* i *natywnego równorzędnej*. Zarządzanego elementu równorzędnego jest wystąpieniem klasy C#. Natywny elementu równorzędnego jest wystąpienie klasy Java w systemie Android runtime maszyny Wirtualnej i C# [IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) właściwość zawiera globalny JNI odwołanie do natywnego elementu równorzędnego. 


Istnieją dwa typy elementów równorzędnych native:

-   **Elementy równorzędne Framework** : typy Java "Normal", które znać nic Xamarin.Android, np. [android.content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/).

-   **Elementy równorzędne użytkownika** : [Android wywoływane otoki](~/android/platform/java-integration/working-with-jni.md) które są generowane w czasie kompilacji dla każdej podklasy Java.Lang.Object obecny w aplikacji.


Istnieją dwie maszyny wirtualne w ramach procesu Xamarin.Android, dostępne są dwa typy wyrzucania:

-   Kolekcje środowiska uruchomieniowego systemu android 
-   Kolekcjach mono 

Kolekcje android środowiska uruchomieniowego działa normalnie, ale z Ostrzeżenie: odwołania do globalnej JNI jest traktowany jako katalog główny GC. W związku z tym, jeśli jest globalna JNI odwołania, zawierający na Android środowiska uruchomieniowego wirtualna obiektu, obiekt *nie* gromadzone, nawet jeśli w przeciwnym razie nie kwalifikuje się do kolekcji.

Mono kolekcje są, gdzie fun sytuacji. Zbierane są zwykle zarządzanych obiektów. Obiekty równorzędne są zbierane przez wykonanie następującego procesu:

1.  Wszystkie obiekty równorzędne kwalifikuje się do kolekcji Mono mieć ich JNI odwołania globalne zastąpione JNI słabe odwołanie globalnego. 

2.  Android środowiska uruchomieniowego GC maszyny Wirtualnej jest wywoływany. Wystąpienie równorzędnego natywnego mogą być zbierane. 

3.  Sprawdza, czy JNI słabe globalne odwołań do utworzonych w (1). Jeśli zostały zebrane słabe odwołanie, zbierane są obiektu równorzędnego. Jeśli ma słabe odwołanie *nie* zostały zebrane, następnie słabe odwołanie jest zastępowany JNI odwołania globalnej i obiektu równorzędnego nie są zbierane. Uwaga: na 14+ interfejsu API, oznacza to, że wartość zwracana z `IJavaObject.Handle` mogą ulec zmianie po wykazem Globalnym. 

W rezultacie zaletą jest to, że wystąpienie obiektu równorzędnego będzie funkcjonować tak długo, jak odwołuje się do niego albo kodu zarządzanego (np przechowywane w `static` zmiennej) lub odwołuje się kod języka Java. Ponadto zostanie rozszerzony okres istnienia natywnego elementów równorzędnych poza co jak inaczej na żywo, zgodnie z macierzystego elementu równorzędnego będzie kolekcjonowanych do momentu zarówno natywnego elementów równorzędnych i równorzędnej zarządzane kolekcjonowanych.


## <a name="object-cycles"></a>Cykle obiektu

Obiekty równorzędne są logicznie obecne w ramach środowiska uruchomieniowego systemu Android i Mono maszyny Wirtualnej. Na przykład [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) wystąpienia zarządzanego elementu równorzędnego będzie mieć odpowiednią [android.app.Activity](http://developer.android.com/reference/android/app/Activity.html) framework równorzędnej Java wystąpienia. Wszystkie obiekty, które dziedziczą z [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) można spodziewać się ma reprezentacji w obu maszynach wirtualnych. 

Wszystkie obiekty, które ma reprezentacji w obie maszyny wirtualne mają okresy istnienia, które zostały rozszerzone w porównaniu do obiektów, które znajdują się tylko w obrębie jednej maszyny Wirtualnej (takie jak [ `System.Collections.Generic.List<int>` ](xref:System.Collections.Generic.List%601)). Wywoływanie [GC. Zbieraj](xref:System.GC.Collect) musi będzie zbierać tych obiektów upewnić się, że obiekt nie odwołuje się albo maszyny Wirtualnej przed zbierania potrzeb GC platformy Xamarin.Android. 

Aby skrócić czas życia obiektów, [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) powinna być wywoływana. To spowoduje ręcznie "Server" połączenia dla obiektu między dwóch maszyn wirtualnych przy zwalnianiu odwołanie do globalnych, dzięki czemu obiekty, które mają być zbierane szybciej. 


## <a name="automatic-collections"></a>Automatyczne kolekcje

Począwszy od [wersji 4.1.0](https://developer.xamarin.com/releases/android/mono_for_android_4/mono_for_android_4.1.0), po przekroczeniu progu gref Xamarin.Android automatycznie wykonuje pełną operacją GC. Ten próg wynosi 90% znane grefs maksymalną platformy: grefs 1800 emulatora (2000 max) i 46800 grefs na sprzęcie (maksymalna 52000). *Uwaga:* Xamarin.Android zlicza tylko grefs utworzone przez [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)i nie będzie wiedzieć o innych grefs utworzone w procesie. Jest to heurystyki *tylko*. 

Po wykonaniu automatyczne zbieranie dzienniku debugowania będą wypisywane komunikat podobny do następującego:

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

To wystąpienie jest deterministyczna i może się tak zdarzyć w nieodpowiednim czasie (np. w trakcie renderowania grafiki). Jeśli ten komunikat zostanie wyświetlony, może być konieczne wykonanie w innym miejscu jawne kolekcji lub chcesz spróbować [Zmniejsz okres istnienia obiektów równorzędnych](#Helping_the_GC). 

## <a name="gc-bridge-options"></a>Opcje Mostek GC

Xamarin.Android oferuje Zarządzanie przezroczysty pamięci za pomocą systemu Android i środowiska uruchomieniowego systemu Android. Są one zaimplementowane jako rozszerzenie Mono modułu zbierającego elementy bezużyteczne o nazwie *Mostek GC*. 

Mostek GC działa podczas Mono wyrzucania elementów bezużytecznych i rysunki limit równorzędnej, które obiekty wymaga ich "liveness" zweryfikować stos obsługi systemu Android. Mostek GC sprawdza to, wykonując następujące czynności (w kolejności):

1.  Wywołać wykres mono odwołania obiektów równorzędnych nieosiągalny na obiekty Java, które reprezentują. 

2.  Wykonaj Java GC.

3.  Sprawdź, które obiekty są naprawdę martwy. 

Ten proces skomplikowane jest, co umożliwia podklasy `Java.Lang.Object` za darmo odwołaniem żadne obiekty; spowoduje usunięcie wszelkich ograniczeń, na które Java można powiązać obiekty C#. Ze względu na to złożoności procesu mostek może być bardzo kosztowna i może spowodować zauważalnych przerw w aplikacji. Aplikacja występują znaczne pauzy, warto badanie jedną z następujących trzech implementacje Mostek GC: 

-   **Tarjan** — na podstawie całkowicie nowych projektu mostka GC [algorytm Robert Tarjan i odwołuje się wstecz propagacji](http://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm).
    Posiada najlepszej wydajności w naszym symulowane obciążeń, ale ma także większy udział eksperymentalne kodu. 

-   **Nowe** -głównych naprawie oryginalnego kodu dwa wystąpienia kwadratową zachowanie ustalania, ale zachowuje algorytm core (na podstawie [Kosaraju przez algorytm](http://en.wikipedia.org/wiki/Kosaraju's_algorithm) dla silnie wyszukiwanie połączonych składników). 

-   **Stary** -oryginalnego wdrożenia (traktowane jako najbardziej stabilny trzech). Jest to mostek, która aplikacja powinna być używana, jeśli `GC_BRIDGE` pauzy są dozwolone. 


Jedynym sposobem, aby dowiedzieć się, które działania mostka GC, najlepiej jest eksperymentowanie w aplikacji i analizowanie danych wyjściowych. Istnieją dwa sposoby zbierania danych na potrzeby przeprowadzenia testów porównawczych: 

-   **Włącz rejestrowanie** — Włączanie rejestrowania (jak opisano w [konfiguracji](~/android/internals/garbage-collection.md) sekcji) dla każdej opcji Mostek GC, następnie przechwycić i porównywanie danych wyjściowych dziennika z każdego ustawienia. Sprawdź `GC` wiadomości dla każdej opcji; w szczególności `GC_BRIDGE` wiadomości. Wstrzymuje do 150ms dla nieinterakcyjnych aplikacji jest dopuszczalna, ale problem są wstrzymywane powyżej 60ms dla bardzo aplikacji (na przykład gry). 

-   **Włączyć mostek** -ewidencjonowania aktywności Mostek wyświetli średni koszt obiektów wskazywanej przez każdy obiekt związane z procesem mostek. Sortowanie według rozmiaru te informacje zawierają wskazówki dotyczące co wstrzymał największe dodatkowe obiekty. 


Aby określić, które `GC_BRIDGE` opcję aplikacja powinna nam należy przekazać `bridge-implementation=old`, `bridge-implementation=new` lub `bridge-implementation=tarjan` do `MONO_GC_PARAMS` zmiennej środowiska, na przykład: 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

Ustawieniem domyślnym jest **Tarjan**. Jeśli okaże się regresji, może być konieczne ustawić tę opcję, **starego**. Ponadto można użyć więcej stabilny **starego** Jeśli **Tarjan** nie tworzy poprawę wydajności. 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>Pomoc w wykazie Globalnym

Istnieje wiele sposobów GC, aby zmniejszyć czasy zbieranie i wykorzystywanie pamięci.



### <a name="disposing-of-peer-instances"></a>Usuwanie wystąpienia elementu równorzędnego

Wykaz Globalny jest niekompletne widok procesu i może nie być możliwe po jest za mało pamięci, ponieważ wykaz Globalny nie może ustalić, że pamięć jest niski. 

Na przykład wystąpienie [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) lub typu pochodnego jest co najmniej 20 bajtów (mogą ulec zmianie bez powiadomienia itp., itp.). 
[Wywoływane otoki zarządzane](~/android/internals/architecture.md) nie należy dodawać elementów członkowskich dodatkowe wystąpienia, więc jeśli masz [Android.Graphics.Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) wystąpienia, która odwołuje się do obiektu blob 10MB pamięci, GC dla platformy Xamarin.Android będą wiedzieć, który &ndash; GC zostanie wyświetlony obiektu 20-bajtową i nie będzie można określić, że jest połączony do systemu Android przydzielone środowiska uruchomieniowego obiektów, które może być utrzymywanie 10MB pamięci aktywności. 

Często należy pomocy GC. Niestety *GC. AddMemoryPressure()* i *GC. RemoveMemoryPressure()* nie są obsługiwane, więc jeśli możesz *wiedzieć* właśnie zwolniła wykres dużego obiektu przydzielone Java może być konieczne ręczne wywołać [GC. Collect()](xref:System.GC.Collect) monit GC, aby zwolnić Java serwerowe pamięci, lub można jawnie usuwa *Java.Lang.Object* podklas uszkodzić mapowania między zarządzanych wywoływana otoka i wystąpienia Java. Na przykład, zobacz [1084 usterki](http://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6). 


> [!NOTE]
> Musi być *bardzo* zachować ostrożność podczas usuwania `Java.Lang.Object` wystąpień podklasy.

Aby zminimalizować możliwości uszkodzenie pamięci, należy uwzględnić następujące wskazówki podczas wywoływania metody `Dispose()`.


#### <a name="sharing-between-multiple-threads"></a>Udostępnianie między wiele wątków

Jeśli *Java lub zarządzanych* wystąpienia może być współużytkowane przez wiele wątków *nie powinny być `Dispose()`d*, **kiedykolwiek**. Na przykład [ `Typeface.Create()` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/(System.String%2cAndroid.Graphics.TypefaceStyle)) może zwrócić *buforowane wystąpienie*. Jeśli wiele wątków zapewnia te same argumenty, będzie pobierał *tego samego* wystąpienia. W rezultacie `Dispose()`używać z `Typeface` wystąpienia z jednego wątku może unieważnić innych wątków, które mogą skutkować `ArgumentException`s z `JNIEnv.CallVoidMethod()` (między innymi), ponieważ wystąpienie zostało usunięte z innego wątku. 


#### <a name="disposing-bound-java-types"></a>Usuwanie Java powiązane typy

Jeśli wystąpienie jest typu powiązanej Java, można usunąć wystąpienia z *tak długo, jak* z kodu zarządzanego nie można ponownie użyć wystąpienia *i* wystąpienia Java nie może być udostępniana między wątków (zobacz poprzednie `Typeface.Create()` dyskusji). (Co to może być trudne.) Przy następnym wejścia wystąpienia Java kod zarządzany, *nowe* otoki zostaną utworzone dla niego. 

Jest to często przydatne, jeśli chodzi o Drawables i innych zasobów silnie wystąpień:

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

Powyższe jest bezpieczne ponieważ element równorzędny który [Drawable.CreateFromPath()](https://developer.xamarin.com/api/member/Android.Graphics.Drawables.Drawable.CreateFromPath/) zwraca odnoszą się do elementu równorzędnego Framework *nie* elementu równorzędnego użytkownika. `Dispose()` Wywołać na końcu `using` bloku spowoduje zerwanie relacji między zarządzanej [Drawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.Drawable/) i framework [Drawable](http://developer.android.com/reference/android/graphics/drawable/Drawable.html) wystąpień, dzięki czemu Java wystąpienie było zbierane jak musi Android środowiska wykonawczego. Spowoduje to *nie* być bezpieczne w przypadku wystąpienia elementów równorzędnych określonego elementu równorzędnego użytkownika; w tym miejscu używamy "external" informacje *wiedzieć* który `Drawable` nie może odwoływać się do elementu równorzędnego użytkownika i w związku z tym `Dispose()` wywołania jest bezpieczne. 


#### <a name="disposing-other-types"></a>Usuwanie innych typów 

Jeśli wystąpienie odwołuje się do typu, który nie ma powiązania typu Java (takie jak niestandardowego `Activity`), **nie** wywołania `Dispose()` chyba że możesz *wiedzieć* żadnego kodu języka Java będzie wywoływać przesłoniętych metod na tej wystąpienie. W wyniku błędu w tym celu [ `NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls). 

Na przykład jeśli masz niestandardowego kliknij odbiornika:

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

Możesz *nie powinien* usunięcia tego wystąpienia, ponieważ Java podejmie próbę wywołania metody na niej w przyszłości:

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```


#### <a name="using-explicit-checks-to-avoid-exceptions"></a>Aby uniknąć wyjątków przy użyciu kontroli jawne

Jeśli zostały zaimplementowane [Java.Lang.Object.Dispose](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose(System.Boolean)/) przeciążyć metodę, należy unikać modyfikowania obiektów, które obejmują JNI. Może to spowodować *dispose o podwójnej precyzji* sytuacji, która umożliwia swój kod (śmiertelnie) próbuje uzyskać dostęp podstawowego obiektu Java, który został już zebrane pamięci. Dzięki temu tworzy wyjątek podobny do następującego: 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

Taka sytuacja często występuje podczas pierwszego usunięcia obiektu powoduje, że elementem członkowskim jako wartość null, a następnie próba dostęp do tego elementu członkowskiego w wartości null powoduje zgłoszenie wyjątku zostanie wygenerowany. W szczególności obiektu `Handle` (która łączy zarządzanego wystąpienia jego podstawowym wystąpieniem Java) zostało unieważnione na pierwszym metodę dispose, ale kodu zarządzanego nadal próbuje uzyskać dostęp tego wystąpienia źródłowy Java, nawet jeśli nie jest już dostępna (zobacz [ Zarządzane wywoływane otoki](~/android/internals/architecture.md#Managed_Callable_Wrappers) więcej informacji o mapowanie między wystąpieniami Java i zarządzanych wystąpień). 

Dobrym sposobem uniknięcia tego wyjątku jest jawnie sprawdzenie w Twojej `Dispose` metodę, która jest nadal ważny; to mapowanie między zarządzanego wystąpienia i podstawowej wystąpienia Java, sprawdź, czy obiekt `Handle` ma wartość null (`IntPtr.Zero`) przed uzyskaniem dostępu do jego elementów członkowskich. Na przykład następująca `Dispose` uzyskuje dostęp do metody `childViews` obiektu: 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);
        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```

Początkowa dispose przekazania przyczyny `childViews` ma nieprawidłową `Handle`, `for` zgłosi dostępu do pętli `ArgumentException`. Przez dodanie jawnego `Handle` null wyboru przed pierwszym `childViews` dostęp do następujących `Dispose` metody uniemożliwia wyjątku z występujących: 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);

        // Check for a null handle:
        if (this.childViews.Handle == IntPtr.Zero)
            return;

        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```


### <a name="reduce-referenced-instances"></a>Zmniejsz przywoływanego wystąpień

Zawsze, gdy wystąpienie `Java.Lang.Object` typu lub podklasa klasy jest zeskanowanych podczas GC, całą *wykres obiektu* czy wystąpienie odwołuje się do również muszą być skanowane. Wykres obiektu to zbiór obiektów, które dotyczą "wystąpienia głównego" *plus* wszystko odwołuje się wystąpienia głównego odwołuje się do lokalizacji. 

Należy wziąć pod uwagę następujące klasy:

```csharp
class BadActivity : Activity {

    private List<string> strings;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```

Gdy `BadActivity` jest utworzone, wykres obiektu będzie zawierać 10004 wystąpień (1 x `BadActivity`, 1 x `strings`, 1 x `string[]` posiadanych przez `strings`, 10000 x wystąpień ciągu), *wszystkie* z którego będą musiały mieć po każdej zmianie skanowane `BadActivity` wystąpienia zostaje przeprowadzone skanowanie. 

Może to mieć niekorzystny wpływ na Twoje czasy kolekcji, co powoduje zwiększoną razy Wstrzymaj GC. 

Możesz pomóc GC przez *zmniejszenie* rozmiar wykresów obiektów, które są umieszczone przez użytkownika wystąpienia elementu równorzędnego. W powyższym przykładzie, można to zrobić za pomocą przenoszenia `BadActivity.strings` do osobnej klasy, który nie dziedziczy z Java.Lang.Object: 

```csharp
class HiddenReference<T> {

    static Dictionary<int, T> table = new Dictionary<int, T> ();
    static int idgen = 0;

    int id;

    public HiddenReference ()
    {
        lock (table) {
            id = idgen ++;
        }
    }

    ~HiddenReference ()
    {
        lock (table) {
            table.Remove (id);
        }
    }

    public T Value {
        get { lock (table) { return table [id]; } }
        set { lock (table) { table [id] = value; } }
    }
}

class BetterActivity : Activity {

    HiddenReference<List<string>> strings = new HiddenReference<List<string>>();

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```


## <a name="minor-collections"></a>Drobne kolekcje

Kolekcje pomocnicza może być wykonana ręcznie przez wywołanie metody [GC. Collect(0)](xref:System.GC.Collect). Drobne kolekcje są tanie (w porównaniu do kolekcji głównej), ale mają znaczny koszt, stały, więc nie ma uruchamiać je zbyt często i powinien mieć czasu wstrzymania, kilka milisekund. 

Jeśli aplikacja ma "cykl pracy" w którym to samo odbywa się samodzielnego, może być wskazane ręcznie wykonać pomocnicza kolekcji po zakończeniu cykl pracy. Przykład należności cykle obejmują: 

-  Cykl renderowania jedną ramkę gier.
-  Całe interakcji z danej aplikacji okna dialogowego (Otwieranie, wypełnianie zamknięcie) 
-  Grupa żądań sieciowych do odświeżania/synchronizacji danych aplikacji.



## <a name="major-collections"></a>Główne kolekcje

Główne kolekcje może być wykonana ręcznie przez wywołanie metody [GC. Collect()](xref:System.GC.Collect) lub `GC.Collect(GC.MaxGeneration)`. 

Powinny być rzadko wykonywane i może mieć czasu wstrzymania, sekundy na urządzeniu z systemem Android stylu, zbierając sterty 512MB. 

Główne kolekcje można wywołać tylko wtedy ręcznie, jeśli kiedykolwiek: 

-   Na końcu długich należności cykle, jak i kiedy wstrzymać długi nie będą wyświetlane problem dla użytkownika. 

-   W ramach przesłoniętych [Android.App.Activity.OnLowMemory()](https://developer.xamarin.com/api/member/Android.App.Activity.OnLowMemory/) metody. 



## <a name="diagnostics"></a>Diagnostyka

Aby sprawdzić, kiedy globalne odwołania są tworzone i niszczone, można ustawić [debug.mono.log](~/android/troubleshooting/index.md) właściwości systemu, aby zawierała [ *gref* ](~/android/troubleshooting/index.md) i/lub [ *gc*](~/android/troubleshooting/index.md). 



## <a name="configuration"></a>Konfiguracja

Moduł zbierający elementy bezużyteczne Xamarin.Android można skonfigurować przez ustawienie `MONO_GC_PARAMS` zmiennej środowiskowej. Można ustawić zmienne środowiskowe z akcją kompilacji [AndroidEnvironment](~/android/deploy-test/environment.md).

`MONO_GC_PARAMS` Zmienną środowiskową jest rozdzielana przecinkami lista następujące parametry: 

-   `nursery-size` = *rozmiar* : umożliwia ustawienie rozmiaru szkółki. Rozmiar jest określony w bajtach i musi być potęgą liczby dwa. Sufiksy `k` , `m` i `g` może służyć do określania kilo-, mega - i gigabajtach, odpowiednio. Szkółki jest pierwsza generacja (dwa). Większe szkółki zwykle przyspieszy program, ale oczywiście będzie używać większej ilości pamięci. Szkółki domyślny rozmiar 512 kb. 

-   `soft-heap-limit` = *rozmiar* : maksymalna docelowych zarządzanych zużycie pamięci dla aplikacji. Gdy wykorzystanie pamięci jest poniżej określonej wartości, wykaz Globalny jest zoptymalizowana pod kątem czasu wykonywania (mniej kolekcji). 
    Przekracza ten limit wykaz Globalny jest zoptymalizowana pod kątem użycia pamięci (więcej kolekcji). 

-   `evacuation-threshold` = *Próg* : ustawia próg ewakuacji w procentach. Wartość musi być liczbą całkowitą z zakresu od 0 do 100. Wartość domyślna to 66. Faza odchylenia kolekcji stwierdzi, że miejsce zajęte przez typ bloku sterty określonych poniżej tej wartości procentowej, wykona kopiowanie kolekcji dla tego typu bloku w następnym kolekcji głównych, a tym samym Przywracanie zajęte do prawie 100 procent. Wartość 0 wyłącza ewakuacji. 

-   `bridge-implementation` = *Mostek implementacji* : spowoduje to ustawienie opcji Mostek GC ułatwiające problemy z wydajnością adres GC. Istnieją trzy możliwe wartości: *starego* , *nowe* , *tarjan*.

-   `bridge-require-precise-merge`Tarjan Mostek zawiera optymalizacji, które mogą w rzadkich przypadkach spowodować, że obiekt, aby można zbierane GC jeden po staje się najpierw pamięci. Włącznie tej opcji powoduje wyłączenie tej optymalizacji, co bardziej przewidywalna, ale wolniej potencjalnie wykazów globalnych.

Na przykład aby skonfigurować GC mają limit rozmiaru stosu 128 MB, Dodaj nowy plik do projektu z **Akcja kompilacji** z `AndroidEnvironment` z zawartością: 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
