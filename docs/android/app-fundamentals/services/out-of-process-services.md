---
title: Uruchamianie systemu Android usług w procesach zdalnego
description: Ogólnie rzecz biorąc wszystkie składniki aplikacji systemu Android zostanie uruchomiony w tym samym procesie. Usługi dla systemu android się zauważalne wyjątek to, że mogą być skonfigurowana do pracy w ich własnych procesów i udostępnionych przez inne aplikacje, w tym z innymi deweloperami systemu Android. W tym przewodniku będzie omawiać temat tworzenia i używania usługi zdalnego z systemem Android za pomocą platformy Xamarin.
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 57be8187509ad7607c38ea17233e9ab5d25b41f1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="running-android-services-in-remote-processes"></a>Uruchamianie systemu Android usług w procesach zdalnego

_Ogólnie rzecz biorąc wszystkie składniki aplikacji systemu Android zostanie uruchomiony w tym samym procesie. Usługi dla systemu android się zauważalne wyjątek to, że mogą być skonfigurowana do pracy w ich własnych procesów i udostępnionych przez inne aplikacje, w tym z innymi deweloperami systemu Android. W tym przewodniku będzie omawiać temat tworzenia i używania usługi zdalnego z systemem Android za pomocą platformy Xamarin._

## <a name="out-of-process-services-overview"></a>Poza procesu usługi — omówienie

Podczas uruchamiania aplikacji dla systemu Android tworzy proces do uruchamiania aplikacji. Zwykle wszystkie składniki uruchamiania aplikacji w jednym procesie. Usługi dla systemu android się zauważalne wyjątek to, że mogą być skonfigurowana do pracy w ich własnych procesów i udostępnionych przez inne aplikacje, w tym z innymi deweloperami systemu Android. Tego rodzaju usług są określane jako _usługi zdalnej_ lub _usługi poza procesem_. Kod dla tych usług będzie znajdować się w tej samej APK jako aplikacji głównej; Jednak po uruchomieniu usługi Android utworzy nowy proces tylko tej usługi. Z kolei usługi, która działa w ten sam proces jako pozostałej części aplikacji jest nazywane _Usługa lokalna_.

Ogólnie rzecz biorąc nie jest konieczne do wdrożenia usługi zdalnej aplikacji. Usługa lokalna jest wystarczające (i pożądane) wymagania aplikacji w wielu przypadkach. Poza procesem ma własne pamięci miejsca, które muszą być zarządzane przez system Android. Mimo że to powodować większe obciążenie do ogólnej aplikacji, istnieją sytuacje, w których może być korzystne uruchamianie usługi w swoim własnym procesie:

1. **Udostępnianie funkcji** &ndash; niektórzy deweloperzy aplikacji może mieć wiele aplikacji i funkcje, które są współużytkowane przez wszystkie aplikacje. Pakowanie te funkcje w usłudze systemu Android, w której działa we własnym procesie może uprościć konserwację aplikacji. Istnieje również możliwość pakietu usługi w własną APK autonomicznej i wdróż je oddzielnie od pozostałej części aplikacji.
2. **Ulepszanie środowiska użytkownika w zakresie** &ndash; istnieją dwa sposoby możliwe, że usługa poza procesem może poprawić środowisko użytkownika aplikacji. Pierwszy sposób podchodzi do zarządzania pamięcią. Gdy wyrzucania elementów bezużytecznych (GC) występuje cykl, Android wstrzyma wszystkie działania w procesie, aż do zakończenia GC. Użytkownik może postrzegać ta przerwa jako "przerywanego" lub "jank". Gdy usługa jest uruchomiona proces własny, jest proces usługi, który jest wstrzymana, nie w procesie aplikacji. Ta przerwa będzie znacznie mniej widoczne dla użytkownika, jak proces aplikacji (i w związku z tym interfejs użytkownika) nie została wstrzymana.

    Po drugie Jeśli wymagania dotyczące pamięci procesu stanie się zbyt duży, Android mogą zakończenia tego procesu, aby zwolnić zasoby urządzenia. Jeśli usługa ma wpływ dużej ilości pamięci i jest uruchomiona w tym samym procesie jako interfejsu użytkownika, następnie po Android wymuszanie zwraca te zasoby interfejsu użytkownika będzie można zamknąć, wymuszanie użytkownika, aby uruchomić aplikację. Jednak w przypadku usługi uruchomione w swoim własnym procesie zostanie zamknięta przez system Android, nie dotyczy pozostaje procesu interfejsu użytkownika. Interfejs użytkownika można powiązać (i uruchom ponownie) usługi niewidoczny dla użytkownika, a Wznowienie normalnego działania.

3. **Ulepszanie wydajności aplikacji** &ndash; procesu interfejsu użytkownika mogą być zakończone lub zamknięty niezależnie od procesu usługi. Przenosząc długich uruchamiania zadań do usługi poza procesem, jest możliwe, że czas uruchamiania interfejsu użytkownika nie może być poprawie (przy założeniu, że proces usługi jest utrzymywane w zakresie od godziny, które jest uruchamiane interfejsu użytkownika).

Na wiele sposobów, powiązanie z usługą uruchomiona w innym procesie jest taka sama jak [powiązanie z lokalną usługą](~/android/app-fundamentals/services/creating-a-service/bound-services.md). Klient wywoła `BindService` bind (i uruchomić, jeśli to konieczne) usługi. `Android.OS.IServiceConnection` Zostanie utworzony obiekt połączenia między klientem a usługą zarządzania. Jeśli klient pomyślnie wiąże się z usługą, a następnie Android, którą będzie zwracać obiekt za pośrednictwem `IServiceConnection` można wywołać metod w usłudze. Klient użyje usługi przy użyciu tego obiektu. Aby przejrzeć, poniżej przedstawiono kroki, aby powiązanie z usługą:

* **Utwórz celem** &ndash; jawne celem muszą być używane do powiązania z usługą.
* **Wdrożenie i wystąpień `IServiceConnection` obiektu** &ndash; `IServiceConnection` obiektu działa jako pośrednik między klientem a usługą.  Jest odpowiedzialna za monitorowanie połączenia między klientem i serwerem.
* **Wywołanie `BindService` metody** &ndash; wywoływania `BindService` wyśle zamiar i utworzone w poprzednich krokach w systemie Android, który zajmie się uruchomić tę usługę i nawiązywania połączenia z usługą między klientem a usługą.

Konieczność cross granice procesu wprowadzić dodatkowe złożoność: komunikacja jest jednokierunkowa (od klienta do serwera) i klienta nie można bezpośrednio wywołać metody w klasie usługi. Odwołaj się, że kiedy usługa jest uruchomiona te same czynności co klient, Android zapewnia `IBinder` obiektu, który może umożliwić dwukierunkowej komunikacji. To nie jest w przypadku uruchamiania w swoim własnym procesie usługi. Klient komunikuje się z usługą zdalnego z pomocy `Android.OS.Messenger` klasy.

Gdy klient żąda powiązania z usługi zdalnej, Android wywoła `Service.OnBind` cyklu życia metodę, która zwróci wewnętrznej `IBinder` obiekt, który jest hermetyzowany przez `Messenger`. `Messenger` Jest cienką otoką za pośrednictwem specjalnego `IBinder` implementacji, które są udostępniane przez zestaw SDK systemu Android. `Messenger` Dba o komunikacji między dwóch różnych procesów. Deweloper jest unconcerned szczegółowe informacje o serializacji komunikatu, kierowania wiadomości granicy procesu i deserializacji go na komputerze klienckim. Tej pracy jest obsługiwany przez `Messenger` obiektu. Ten diagram przedstawia składniki systemu Android po stronie klienta, które biorą udział, gdy klient jest inicjowany przez powiązanie z usługą poza procesem:

![Diagram przedstawiający kroki i składniki klienta powiązanie z usługą](out-of-process-services-images/ipc-01.png "diagramu, który przedstawia kroki i składniki klienta powiązanie z usługą.")

`Service` Klasy zdalnego procesu zostanie umieszczona przy użyciu tego samego cyklu życia wywołania zwrotne wykraczające powiązanej usługi w procesie lokalnej za pośrednictwem i wiele interfejsów API związanego są takie same. `Service.OnCreate` Służy do inicjowania `Handler` i który do wprowadzenia `Messenger` obiektu. Podobnie `OnBind` zastąpiona, ale zamiast zwracać `IBinder` obiektu, usługi zwróci `Messenger`.  Ten diagram przedstawia, co się stanie w usłudze, gdy klient jest powiązanie do niego:

![Diagram przedstawiający kroki i składniki usługi przechodzi przez gdy jest powiązana z przez klienta zdalnego](out-of-process-services-images/ipc-02.png "diagramu, który przedstawia kroki i składniki usługi przechodzi przez gdy jest powiązana z przez klienta zdalnego.")

Gdy `Message` otrzymania przez usługę, jednak jest przetwarzany przez w wystąpieniu `Android.OS.Handler`. Usługa będzie implementowany własną `Handler` podklasy, który należy zastąpić `HandleMessage` metody. Ta metoda jest wywoływana przez `Messenger` i odbiera `Message` jako parametr. `Handler` Będzie przeprowadzał inspekcję `Message` metadane i użyć tych informacji do wywoływania metod w usłudze.

Komunikacja jednokierunkowa występuje, gdy klient tworzy `Message` obiektów i wysyła go do usługi przy użyciu `Messenger.Send` metody. `Messenger.Send` będzie serializować `Message` i ręcznie, która serializacji danych wyłączone w systemie Android, którego będzie przekierowywać wiadomości granicy procesu i do usługi.  `Messenger` Hostowanej przez usługę utworzy `Message` obiektu z danych przychodzących. To `Message` znajduje się w kolejce, których wiadomości są przesłane pojedynczo do `Handler`. `Handler` Będzie przeprowadzał inspekcję meta dane zawarte w `Message` i wywołanie metody odpowiednie na `Service`. Na poniższym diagramie przedstawiono te pojęcia dotyczące różnych akcji:

![Diagram pokazujący sposób przekazywania wiadomości między procesami](out-of-process-services-images/ipc-03.png "Diagram pokazujący sposób przekazywania wiadomości między procesami.")

W tym przewodniku będzie omawiać szczegóły dotyczące implementowania usługi poza procesem. Przedstawimy sposobu implementacji usługi, który jest przeznaczony do uruchamiania w swoim własnym procesie i jak klient może komunikować się z tej usługi przy użyciu `Messenger` framework. Również krótkie przedstawimy komunikacja dwukierunkowa: klienta, wysyłanie komunikatu do usługi i wysyła wiadomość do klienta. Ponieważ usługi mogą być współużytkowane między różnymi aplikacjami, w tym przewodniku również przedstawimy co metoda ograniczania dostępu klienta do usługi za pomocą uprawnień systemu Android.

> [!IMPORTANT]
> [Nie można poprawnie rozpoznać przeciążenia Bugzilla 51940 - usług o procesach izolowanych i niestandardowej klasy aplikacji](https://bugzilla.xamarin.com/show_bug.cgi?id=51940) raportów, które usługa platformy Xamarin.Android nie zostanie uruchomiony prawidłowo po `IsolatedProcess` ma ustawioną wartość `true`. Ten przewodnik dotyczy odwołanie. Aplikacji platformy Xamarin.Android nadal będą już mogli nawiązać połączenia z usługą poza procesem, która jest napisany w języku Java.

## <a name="requirements"></a>Wymagania

W tym przewodniku założono znajomość tworzenia usług.

Mimo że można używać niejawnej konwersji kolorów w aplikacjach przeznaczonych starsze interfejsów API systemu Android, w tym przewodniku dotyczą wyłącznie na korzystanie z profilów jawnej konwersji. Aplikacje na platformy Android 5.0 (poziom interfejsu API 21) lub wyższej muszą mieć jawną celem powiązania z usługą; jest to metoda, która zostanie uruchomiona w tym przewodniku.

## <a name="create-a-service-that-runs-in-a-separate-process"></a>Utworzyć usługę, która działa w ramach osobnej operacji

Zgodnie z powyższym opisem fakt, że usługa jest uruchomiona w swoim własnym procesie oznacza, że niektóre różnych interfejsów API są zaangażowane. Jak to szybki przegląd poniżej przedstawiono kroki, aby powiązać i korzystanie z usługi zdalnej:  

* **Utwórz `Service` podklasy** &ndash; podklasy `Service` wpisz i implementować metody cyklu życia powiązanej usługi. Należy również ustawić metadane, który będzie informować systemu Android, które usługa jest uruchomienie we własnym procesie.
* **Implementowanie `Handler`**  &ndash; `Handler` jest odpowiedzialny za analizowanie żądań klientów, wyodrębnianie żadnych parametrów, które zostały przekazane z klienta i wywoływanie odpowiednich metod w usłudze.
* **Utwórz wystąpienie `Messenger`**  &ndash; opisane powyżej, `Service` muszą zachować wystąpienia `Messenger` klasy, która będzie przekierowywać żądania klientów do `Handler` utworzony w poprzednim kroku.

Usługa, która jest przeznaczony do uruchamiania w swoim własnym procesie jest, znacznie, nadal powiązanej usługi. Klasa usługi rozszerzy podstawowym `Service` klasy i ma `ServiceAttribute` zawierający dane meta wymagające pakietu w manifestu systemu Android dla systemu Android. Aby rozpocząć z następujących właściwości `ServiceAttribute` które są ważne z usługą out-of-process:

1. `Exported` &ndash; Ta właściwość musi mieć ustawioną `true` umożliwia innym aplikacjom interakcję z usługą. Wartość domyślna tej właściwości to `false`.
2. `Process` &ndash; Można ustawić tej właściwości. Służy do określenia nazwy procesu, który usługa zostanie uruchomiona.
3. `IsolatedProcess` &ndash; Ta właściwość umożliwi dodatkowe zabezpieczenia, informuje system Android, aby uruchomić usługę w izolowanej piaskownicy z minimalnym uprawnieniem do iteract z resztą systemu. Zobacz [Bugzilla 51940 - usługi w procesach izolowanych i niestandardowej klasy aplikacji nie można poprawnie rozpoznać przeciążenia](https://bugzilla.xamarin.com/show_bug.cgi?id=51940).
4. `Permission` &ndash; Istnieje możliwość kontroli dostępu klienta do usługi, określając uprawnienia, które klientów należy zażądać (i otrzymać).

Aby uruchomić usługę własnym procesie `Process` właściwość `ServiceAttribute` musi mieć ustawioną nazwę usługi. Do interakcji z aplikacjami poza `Exported` powinien mieć ustawioną właściwość `true`. Jeśli `Exported` jest `false`, a następnie tylko klientów w tej samej APK (tj. tej samej aplikacji) i działa w tym samym procesie będą mogli interakcji z usługą.

Jakiego rodzaju procesu, usługa zostanie uruchomiona w zależy od wartości `Process` właściwości. Android identyfikuje trzy różne typy procesów:

-   **Proces prywatnego** &ndash; prywatnych procesu to taki, który jest dostępny tylko dla aplikacji, który je zainicjował. Aby zidentyfikować proces jako prywatny, jego nazwa musi rozpoczynać się od **:** (średnikami). Usługa opisany w poprzednim fragment kodu i tworzenia zrzutów ekranu jest procesem prywatnych. Poniższy fragment kodu przedstawiono przykładowy `ServiceAttribute`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

-   **Globalny** &ndash; to usługa, która jest uruchamiane w procesie globalnego jest dostępny dla wszystkich aplikacji uruchomionej na urządzeniu. Globalne proces musi być nazwy FQDN klasy, która musi się zaczynać się małą literą.
    (Chyba że kroków mających na celu zabezpieczenia usługi, inne aplikacje mogą powiązać i korzystać z niego. Zabezpieczanie usługi przed nieautoryzowanym użyciem zostanie dokładnie omówione w dalszej części tego podręcznika.)

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

-   **Izolowane procesu** &ndash; procesie izolowanym jest proces, który jest uruchamiany w jego własnej izolowanym, odizolowane od pozostałych systemu i żadne specjalne uprawnienia własnych. Aby uruchomić usługę w procesie izolowanym, `IsolatedProcess` właściwość `ServiceAttribute` ustawiono `true` jak pokazano w poniższym przykładzie:
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> Zobacz [Bugzilla 51940 - usługi w procesach izolowanych i niestandardowej klasy aplikacji nie można poprawnie rozpoznać przeciążenia](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)

Izolowane usługi to prosty sposób do zabezpieczania aplikacji i urządzeń przed kodzie niezaufanym. Na przykład aplikacja może pobrać i uruchomienia skryptu z witryny sieci Web. W takim przypadku to wykonywanie w procesie izolowanym zapewnia dodatkową warstwę zabezpieczeń przed kodzie niezaufanym naruszenie urządzeniu z systemem Android.

> [!IMPORTANT]
> Po wyeksportowaniu usługi nie należy zmieniać nazwę usługi. Zmiana nazwy usługi mogą być dzielone inne aplikacje, które korzystają z usługi.

Aby zobaczyć efekt który `Process` właściwość, poniższy zrzut ekranu przedstawia w usłudze działającej w swoim własnym procesie prywatnych:

![Zrzut ekranu pokazujący usługi uruchomionej w prywatnej procesu](out-of-process-services-images/ipc-04.png "zrzut ekranu pokazujący usługi uruchomionej w prywatnej procesu.")

Ten dalej zrzut ekranu przedstawia `Process="com.xamarin.xample.messengerservice.timestampservice_process"` i usługi uruchomione w procesie globalne:

![Zrzut ekranu przedstawiający usługi uruchomione w procesie globalne](out-of-process-services-images/ipc-05.png "zrzut ekranu przedstawiający usługi uruchomione w procesie globalnego.")

Raz `ServiceAttribute` została ustawiona, potrzebom usługi implementacji `Handler`.

### <a name="implementing-a-handler"></a>Implementacja programu obsługi

Do przetwarzania żądań klientów, usługa musi implementować `Handler` i zastąpić `HandleMessage` methodThis jest metoda przyjmuje `Message` wystąpienie, które która hermetyzuje wywołanie metody z klienta i tłumaczy tego wywołania do określonej akcji lub zadanie, które wykona usługi. `Message` Obiekt ujawnia właściwość o nazwie `What` czyli wartość całkowitą, który znaczenia jest udostępniana między klientem a usługą i odnosi się do niektóre zadania usługi do wykonania dla klienta.

Poniższy fragment kodu z przykładowej aplikacji przedstawiono przykład `HandleMessage`. W tym przykładzie istnieją dwie akcje, które może żądać klient usługi:

* Pierwszą akcją jest _Hello, World_ wiadomości, klient wysłał prostego komunikatu do usługi.
* Drugiej akcji zostanie wywołana metoda usługi i pobrać ciąg, w tym przypadku jest komunikat, który zwraca czas, jaki usługa uruchomiony i jak długo ma on uruchomiony w ciągu:

```csharp
public class TimestampRequestHandler : Android.OS.Handler
{
    // other code omitted for clarity

    public override void HandleMessage(Message msg)
    {
        int messageType = msg.What;
        Log.Debug(TAG, $"Message type: {messageType}.");

        switch (messageType)
        {
            case Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE:
                // The client as sent a simple Hello, say in the Android Log.
                break;

            case Constants.GET_UTC_TIMESTAMP:
                // Call methods on the service to retrive a timestamp message.
                break;
            default:
                Log.Warn(TAG, $"Unknown messageType, ignoring the value {messageType}.");
                base.HandleMessage(msg);
                break;
        }
    }
}
```

Istnieje również możliwość do pakietu parametrów dla usługi w `Message`. To zostanie dokładnie omówione w dalszej części tego przewodnika. Tworzy następnego tematu wziąć pod uwagę `Messenger` obiektu do przetwarzania przychodzących `Message`s.

### <a name="instantiating-the-messenger"></a>Tworzenie wystąpień Messenger

Jak wcześniej wspomniano, deserializacji `Message` obiektów i wywoływania `Handler.HandleMessage` jest kompetencyjnego z `Messenger` obiektu. `Messenger` Klasa udostępnia także `IBinder` obiektów, że klient będzie używać do wysyłania komunikatów do usługi.  

Po uruchomieniu usługi będzie wystąpienia `Messenger` i wstrzyknąć `Handler`. Dobrym miejscem do wykonania tego inicjowania znajduje się na `OnCreate` metody usługi. Następujący fragment kodu jest przykładem usługi, która inicjuje własną `Handler` i `Messenger`:

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

W tym momencie ostatnim krokiem jest dla `Service` do przesłonięcia `OnBind`.

### <a name="implementing-serviceonbind"></a>Implementowanie Service.OnBind

Wszystkie usługi powiązane, czy działają we własnym procesie lub nie, musi implementować `OnBind` metody. Zwracana wartość ta metoda jest niektórych obiekt, który klient może używać do interakcji z usługą. Dokładnie co ten obiekt jest zależy od tego, czy usługa jest usługi lokalnej lub zdalnej usługi. Gdy Usługa lokalna zwróci niestandardowego `IBinder` implementacji usługi zdalnej zwróci `IBinder` który jest hermetyzowany ale `Messenger` utworzony w poprzedniej sekcji:

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

Gdy te trzy kroki są wykonywane, zdalna usługa jest uznawana za pełną.

## <a name="consuming-the-service"></a>Korzystanie z usługi

Wszyscy klienci muszą implementować niektórych kod, aby można było powiązać i korzystanie z usługi zdalnej. Koncepcyjnie z punktu widzenia klienta, istnieje bardzo niewielkie różnice między powiązania usługi lokalnej lub zdalnej usługi. Klient wywołuje `BindService` jest metoda jawne zamiar do identyfikowania usługi i `IServiceConnection` czy pomaga zarządzać połączenia między klientem a usługą.

Następujący fragment kodu przedstawiono przykładowy sposób tworzenia **jawne zamiar** dla powiązania usługi zdalnej. Celem musi identyfikować pakiet, który zawiera usługi i nazwę usługi. Jednym ze sposobów ustawić te informacje jest użycie `Android.Content.ComponentName` obiektu i zapewnienie, że aby celem. Następujący fragment kodu jest przykładem:  

```csharp
// This is the package name of the APK, set in the Android manifest
const string REMOTE_SERVICE_COMPONENT_NAME = "com.xamarin.TimestampService";
// This is the name of the service, according the value of ServiceAttribute.Name
const string REMOTE_SERVICE_PACKAGE_NAME   = "com.xamarin.xample.messengerservice";

// Provide the package name and the name of the service with a ComponentName object.
ComponentName cn = new ComponentName(REMOTE_SERVICE_PACKAGE_NAME, REMOTE_SERVICE_COMPONENT_NAME);
Intent serviceToStart = new Intent();
serviceToStart.SetComponent(cn);
```

Gdy usługa jest powiązana, `IServiceConnection.OnServiceConnected` metoda jest wywoływana i udostępnia `IBinder` do klienta. Jednak klient nie będzie bezpośrednio używać `IBinder`. Zamiast tego zostanie utworzenie wystąpienia `Messenger` obiektu, z którego `IBinder`. Jest to `Messenger` używanego przez klienta do interakcji z usługi zdalnej.

Poniżej przedstawiono przykład bardzo podstawowe `IServiceConnection` wdrożenia, który demonstruje sposób klient będzie obsługi połączenie i odłączanie od usługi. Zwróć uwagę, że `OnServiceConnected` odbiera — metoda i `IBinder`, a klient tworzy `Messenger` niż `IBinder`:

```csharp
public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection
{
    static readonly string TAG = typeof(TimestampServiceConnection).FullName;

    MainActivity mainActivity;
    Messenger messenger;

    public TimestampServiceConnection(MainActivity activity)
    {
        IsConnected = false;
        mainActivity = activity;
    }

    public bool IsConnected { get; private set; }
    public Messenger Messenger { get; private set; }

    public void OnServiceConnected(ComponentName name, IBinder service)
    {
        Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

        IsConnected = service != null
        Messenger = new Messenger(service);

        if (IsConnected)
        {
            // things to do when the connection is successful. perhaps notify the client? enable UI features?
        }
        else
        {
            // things to do when the connection isn't successful.
        }
    }

    public void OnServiceDisconnected(ComponentName name)
    {
        Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
        IsConnected = false;
        Messenger = null;

        // Things to do when the service disconnects. perhaps notify the client? disable UI features?

    }
}
```

Po utworzeniu połączenia z usługą i celem jest możliwe w dla klienta do wywołania `BindService` i zainicjuj proces wiązania:

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

Po klient został pomyślnie powiązany z usługami i `Messenger` jest dostępna, możliwe jest to klientowi wysłanie `Messages` do usługi.

## <a name="sending-messages-to-the-service"></a>Wysyłanie komunikatów do usługi

Kiedy klient jest połączony i ma `Messenger` obiektu, istnieje możliwość komunikacji z usługą, poprzez wywoływanie `Message` obiektów za pomocą `Messenger`. Ta komunikacja jest jednokierunkowa, klient wysyła komunikat, ale nie ma zwracanego z usługi do klienta. W tym zakresie `Message` mechanizm fire i zapomnij.

Preferowany sposób tworzenia `Message` obiektu jest użycie [ `Message.Obtain` ](https://developer.xamarin.com/api/type/Android.OS.Message/#Public%20Methods) metoda fabryki. Ta metoda będzie pobierać `Message` obiektu z globalnej puli, która jest obsługiwana przez system Android. `Message.Obtain` również ma niektóre przeciążone metody, które umożliwiają `Message` obiekt do zainicjowania z wartości i parametrów wymaganych przez usługę.  Raz `Message` jest uruchomiony, jego wysyłane do usługi przez wywołanie metody `Messenger.Send`. Ta Wstawka kodu jest przykładem tworzenia i wysyłania `Message` do procesu usługi:

```csharp
Message msg = Message.Obtain(null, Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE);
try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a error trying to send the message.");
}
```

Istnieje kilka różnych metod `Message.Obtain` metody. W poprzednim przykładzie użyto [ `Message.Obtain(Handler h, Int32 what)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/). Ponieważ jest to żądanie asynchroniczne usługą poza procesem; nie będzie brak odpowiedzi z usługi, więc `Handler` ma ustawioną wartość `null`. Drugi parametr `Int32 what`, będą przechowywane w `.What` właściwość `Message` obiektu. `.What` Właściwość jest używana przez kod w procesie usługi do wywoływania metod w usłudze.

`Message` Klasy udostępnia również dwie dodatkowe właściwości, które mogą być przydatne do recipent: `Arg1` i `Arg2`. Te dwie właściwości są całkowitych wartości, które mogą mieć niektóre specjalne uzgodnione wartości, które mają znaczenie między klientem a usługą. Na przykład `Arg1` może utrzymywać identyfikator klienta i `Arg2` może utrzymywać numer zamówienia zakupu dla tego klienta. [ `Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/System.Int32/System.Int32/) Może być używany do ustawiania właściwości dwóch podczas `Message` jest tworzony. Inny sposób, aby wypełnić te dwie wartości jest skonfigurowanie `.Arg` i `.Arg2` właściwości bezpośrednio na `Message` obiektu po jego utworzeniu.

### <a name="passing-additional-values-to-the-service"></a>Przekazywanie dodatkowe wartości z usługą

Istnieje możliwość przekazywania danych bardziej złożonych z usługą za pomocą `Bundle`. W takim przypadku dodatkowe wartości można umieścić w `Bundle` i wysłane razem z `Message` przez ustawienie [ `.Data` właściwości](https://developer.xamarin.com/api/property/Android.OS.Message.Data/) właściwości przed wysłaniem.

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> Ogólnie rzecz biorąc `Message` nie powinny mieć większe niż 1 MB ładunku. Limit rozmiaru może się różnić zależnie systemu android i na zmiany własnościowych dostawcy może zostały wprowadzone do ich wdrożenia z systemem Android Otwórz źródła projektu (AOSP), który jest powiązany z urządzenia.

## <a name="returning-values-from-the-service"></a>Zwracanie wartości z usługi

Architektura obsługi komunikatów, który został omówiony w tym punkcie jest jednokierunkowa, klient wysyła komunikat do usługi. Jeśli niezbędne usługi zwrócić wartość do klienta jest następnie wszystko, który został omówiony w tym punkcie została odwrócona. Należy utworzyć usługę `Message`, pakowanie wszelkie zwracanych wartości i wysyłania `Message` za pośrednictwem `Messenger` do klienta. Jednak usługa nie tworzy własne `Messenger`; zamiast niego, zależy od klienta, tworzenie wystąpień i pakiet `Messenger` jako część żądania początkowego. Usługa będzie `Send` wiadomości za pomocą tego dostarczonych przez klienta `Messenger`.  

Kolejność zdarzeń dwukierunkowe komunikacji jest to:

1. Klient wiąże się z usługą. Łącząc usługę i klienta, `IServiceConnection` obsługiwany przez klienta będzie zawierać odwołanie do `Messenger` obiekt, który jest używany do przesyłania `Message`s do usługi. Aby uniknąć pomyłek, to będzie ona nazywana jako _usługa Posłaniec_.
2. Tworzy wystąpienie klienta `Handler` (nazywane _obsługi klienta_) i użyty do zainicjowania własną `Messenger` ( _Messenger klienta_). Należy pamiętać, że usługa Posłaniec i Messenger klienta są dwa różne obiekty, które obsługi ruchu w dwóch różnych kierunkach. Usługa Posłaniec obsługuje komunikaty z klienta do usługi podczas Messenger klienta obsługi wiadomości z usługi do klienta.
3. Klient tworzy `Message` obiektu i zestawy `ReplyTo` właściwości programu Messenger klienta. Wiadomości są następnie wysyłane do usługi przy użyciu usługi Messenger.
4. Usługa odbiera komunikat z klienta i wykonuje żądanej pracy.
5. Gdy nadejdzie czas na wysłać odpowiedź do klienta usługi, zostanie użyty `Message.Obtain` do tworzenia nowego `Message` obiektu.
6. Aby wysłać tę wiadomość do klienta, usługa wyodrębni Messenger klienta z `.ReplyTo` właściwości klienta wiadomości i użyj jej do `.Send` `Message` do klienta.
7. Po odebraniu odpowiedzi przez klienta ma własne `Handler` który przetworzy `Message` sprawdzając `.What` właściwości (i w razie potrzeby wyodrębniania żadnych parametrów zawarty w `Message`).

Ten przykładowy kod przedstawia sposób tworzenia wystąpienia klienta `Message` i pakietu `Messenger` która powinna być używana przez usługę odpowiedzi:

```csharp
Handler clientHandler = new ActivityHandler();
Messenger clientMessenger = new Messenger(activityHandler);

Message msg = Message.Obtain(null, Constants.GET_UTC_TIMESTAMP);
msg.ReplyTo = clientMessenger;

try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a problem sending the message.");
}
```

Usługa musi wprowadzić kilka zmian do jego własnej `Handler` wyodrębnić `Messenger` i używać go do wysyłania odpowiedzi do klienta. Następujący fragment kodu jest przykładem usługi `Handler` spowoduje utworzenie `Message` i wysyłania go do klienta:  

```csharp
// This is the message that the service will send to the client.
Message responseMessage = Message.Obtain(null, Constants.RESPONSE_TO_SERVICE);
Bundle dataToReturn = new Bundle();
dataToReturn.PutString(Constants.RESPONSE_MESSAGE_KEY, "This is the result from the service.");
responseMessage.Data = dataToReturn;

// The msg object here is the message that was received by the service. The service will not instantiate a client,
// It will use the client that is encapsulated by the message from the client.
Messenger clientMessenger = msg.ReplyTo;
if (clientMessenger!= null)
{
    try
    {
        clientMessenger.Send(responseMessage);
    }
    catch (Exception ex)
    {
        Log.Error(TAG, ex, "There was a problem sending the message.");
    }
}
```

Należy pamiętać, że w powyższym, przykłady kodu `Messenger` jest tworzony przez klienta *nie* tego samego obiektu, który jest odbierane przez usługę. Są to dwie różne `Messenger` obiektów uruchomionych w dwa osobne procesy, które reprezentują kanał komunikacyjny.

## <a name="securing-the-service-with-android-permissions"></a>Zabezpieczanie usługi z uprawnieniami systemu Android

Usługa, która działa w procesie globalnego jest dostępny dla wszystkich aplikacji uruchomionych na urządzenia z systemem Android. W pewnych sytuacjach ten przejrzystości i dostępności jest niepożądanych i konieczne jest bezpieczny usługi przed dostępem z nieautoryzowanych klientów. Jednym ze sposobów ograniczenia dostępu do zdalnej usługi jest korzystanie z uprawnień systemu Android.

Uprawnienia mogą zostać zidentyfikowane przez `Permission` właściwość `ServiceAttribute` który decorates `Service` klasy. Nadaj nazwę uprawnienia, które klient musi otrzymać podczas wiązania z usługą. Jeśli klient nie ma odpowiednich uprawnień, a następnie zgłosi Android `Java.Lang.SecurityException` kiedy klient podejmie próbę utworzenia powiązania z usługą.

Istnieją cztery różne poziomy uprawnień dostępnych w systemie Android:

* **normalne** &ndash; jest to domyślny poziom uprawnień. Służy do identyfikowania uprawnienia niskiego ryzyka, które mogą być automatycznie przyznane przez system Android żądających klientów. Użytkownik nie ma jawnie udzielić tych uprawnień, ale uprawnienia można wyświetlić w ustawieniach aplikacji.
* **podpis** &ndash; jest kategorią specjalne uprawnienia przyznane automatycznie przez system Android dla aplikacji, które są wszystkie podpisane za pomocą tego samego certyfikatu. To uprawnienie jest umożliwia łatwe dla deweloperów aplikacji udostępnić składniki lub danych między swoje aplikacje bez bothering użytkownika o stałej zatwierdzenia.
* **signatureOrSystem** &ndash; jest bardzo podobny do **podpisu** uprawnienia opisane powyżej. Oprócz jest automatycznie przyznawane do aplikacji, które są podpisane przez ten sam certyfikat, to zostanie również można udzielić uprawnienia aplikacji, które są podpisane ten sam certyfikat, który został użyty do podpisania aplikacji zainstalowane z obrazu systemu Android. To uprawnienie jest zwykle tylko umożliwia przez deweloperów systemu Android ROM swoich aplikacji do pracy z aplikacjami innych firm. Często nie jest używany przez aplikacje, które są przeznaczone do dystrybucji ogólnej dla ogólnej dostępności.
* **niebezpieczne** &ndash; niebezpieczne uprawnienia są tymi, które mogą powodować problemy dla użytkownika. Z tego powodu **niebezpiecznych** uprawnienia muszą być jawnie zatwierdzone przez użytkownika.

Ponieważ `signature` i `normal` automatycznie przyznano uprawnienia w czasie zainstalowanych przez system Android, ważne jest, zainstalowania APK obsługującego usługę **przed** APK zawierający klienta. Jeśli klient jest zainstalowany jako pierwszy, Android nie przyznają uprawnienia. W takim przypadku będzie konieczne odinstalowanie klienta APK, zainstaluj usługę APK i ponownie zainstalować klienta APK.

Istnieją dwa podstawowe sposoby Zabezpieczanie usługi za pomocą uprawnień systemu Android:

1.  **Implementowanie zabezpieczeń na poziomie podpisu** &ndash; zabezpieczeń na poziomie podpisu oznacza, że uprawnienie jest automatycznie przyznanych dla tych aplikacji, które są podpisane za pomocą tego samego klucza, który został użyty do podpisania APK zawierający usługi. Jest to prosty sposób deweloperom ich usługi bezpiecznego jeszcze były dostępne z własnych aplikacji. Uprawnienia na poziomie podpisu są zadeklarowane przez ustawienie `Permission` właściwość `ServiceAttribute` do `signature`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2.  **Utwórz niestandardowe uprawnienie** &ndash; jest możliwe w dla deweloperów usługi można utworzyć uprawnienia niestandardowe dla usługi. Jest to najlepsze w przypadku gdy deweloper chce udostępnić swoje usługi z aplikacjami z innymi deweloperami. Niestandardowe uprawnienie wymaga nieco więcej wysiłku do zaimplementowania zostaną opisane poniżej.

Uproszczony przykład tworzenia niestandardowego `normal` uprawnienia będzie opisana w następnej sekcji. Aby uzyskać więcej informacji o uprawnieniach systemu Android, zapoznaj się dokumentacją firmy Google dla [najlepszych rozwiązań i zabezpieczenia](https://developer.android.com/training/articles/security-tips.html). Aby uzyskać więcej informacji o uprawnieniach systemu Android, zobacz [sekcji uprawnień](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) Android dokumentacji dla manifest aplikacji, aby uzyskać więcej informacji o uprawnieniach systemu Android.

> [!NOTE]
> Ogólnie rzecz biorąc [Google odradza się stosowanie uprawnień niestandardowych](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions) jako może być mylące dla użytkowników.

### <a name="creating-a-custom-permission"></a>Tworzenie uprawnień niestandardowych

Aby korzystać z uprawnień niestandardowych, jest zadeklarowany jako przez usługę podczas, gdy wyraźnie zażąda tego uprawnienia.

Można utworzyć uprawnienia w usłudze APK, `permission` element zostanie dodany do `manifest` element **AndroidManifest.xml**. To uprawnienie musi mieć `name`, `protectionLevel`, i `label` zestawu atrybutów. `name` Atrybut musi mieć ustawioną ciąg, który unikatowo identyfikuje uprawnienia. Nazwa będzie wyświetlana w **informacje o aplikacji** widoku **ustawień systemu Android** (jak pokazano w następnej sekcji).

`protectionLevel` Atrybut musi być ustawiony na jedną z wartości ciągu czterech, które zostały opisane powyżej.  `label` i `description` musi odwoływać się do zasobów ciągu i służą do zapewniania przyjazną dla użytkownika nazwę i opis dla użytkownika.

Ta Wstawka kodu jest przykładem deklarowanie niestandardowego `permission` atrybutu w **AndroidManifest.xml** z APK, który zawiera usługę:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerservice">

    <uses-sdk android:minSdkVersion="21" />

    <permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP"
                android:protectionLevel="signature"
                android:label="@string/permission_label"
                android:description="@string/permission_description"
                />

    <application android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">

    </application>
</manifest>
```

Następnie **AndroidManifest.xml** klienta APK jawnie należy zażądać tego nowe uprawnienia. Jest to realizowane przez dodanie `users-permission` atrybutu **AndroidManifest.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerclient">

    <uses-sdk android:minSdkVersion="21" />

    <uses-permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP" />

    <application
            android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">
    </application>
    </manifest>
```

### <a name="view-the-permissions-granted-to-an-app"></a>Wyświetlanie uprawnień aplikacji

Aby wyświetlić uprawnienia, które zostały przyznane aplikacji, Otwórz aplikację systemu Android ustawienia, a następnie wybierz **aplikacji**. Znajdź i wybierz z listy aplikacji. Z **informacje o aplikacji** ekranu, naciśnij przycisk **uprawnienia** którego zostanie wyświetlone okno widoku, który zawiera wszystkie uprawnienia przyznane aplikacji:

[![Zrzuty ekranu z urządzenia z systemem Android przedstawiający sposób wyszukiwania uprawnienia do aplikacji](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>Podsumowanie

Ten przewodnik został zaawansowane dyskusji dotyczących uruchamiania usługi dla systemu Android w proces zdalny. Wyjaśniono różnice między lokalnej i zdalnej usługi, wraz z określonych przyczyn, dlaczego zdalnej usługi mogą być przydatne do stabilności i wydajności aplikacji systemu Android. Po wyjaśniający implementowania usługi zdalnej i jak klient może komunikować się z usługą, przewodnik poszło jeden sposób, aby ograniczyć dostęp do usługi z tylko autoryzowani klienci.


## <a name="related-links"></a>Linki pokrewne

- [Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Komunikat](https://developer.xamarin.com/api/type/Android.OS.Message/)
- [Messenger](https://developer.xamarin.com/api/type/Android.OS.Messenger/)
- [ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)
- [Atrybut eksportować](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [Nie można poprawnie rozpoznać przeciążenia usług o procesach izolowanych i niestandardowej klasy aplikacji](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [Procesów i wątków](https://developer.android.com/guide/components/processes-and-threads.html)
- [Manifestu systemu android - uprawnień](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [Porady dotyczące zabezpieczeń](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/MessengerServiceDemo/)
