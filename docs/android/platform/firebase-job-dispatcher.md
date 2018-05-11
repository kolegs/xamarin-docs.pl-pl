---
title: Dyspozytor zadania firebase
description: W tym przewodniku omówiono sposób tworzenia harmonogramu praca w tle przy użyciu biblioteki Firebase zadania dyspozytora z Google.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 05/08/2018
ms.openlocfilehash: a714ac55c3a49b91cb21e3ba1793b9bccd7d1be2
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="firebase-job-dispatcher"></a>Dyspozytor zadania firebase

_W tym przewodniku omówiono sposób tworzenia harmonogramu praca w tle przy użyciu biblioteki Firebase zadania dyspozytora z Google._

![Dyspozytor zadania firebase w wersji zapoznawczej](~/media/shared/preview.png)

## <a name="overview"></a>Omówienie

Jednym z najlepszych sposobów zachowanie reakcji użytkownikowi aplikacji systemu Android jest upewnij się, że złożonych lub długotrwałych zadań są wykonywane w tle. Jednak jest ważne, aby praca w tle nie ma negatywnego wpływu przez użytkownika z urządzeniem. 

Na przykład zadanie w tle może sondować witryny sieci Web co trzy lub cztery minut do zapytania zmiany określonego zestawu danych. Prawdopodobnie niegroźne, jednak byłyby Fatalne wpływu na czas pracy baterii. Aplikacji będzie wielokrotnie wznawiania urządzenia, podnieść poziom Procesora do wyższych stan zasilania, włączenia komunikacji radiowej, żądania sieci, a następnie przetwarzania wyników. Pobiera on gorsza, ponieważ urządzenie nie jest od razu zasilanie i nie powrócić do stanu bezczynności niskiego poboru energii. Praca w tle nieprawidłowo zaplanowane przypadkowo mogą przechowywać urządzenie w stanie z zasilania niepotrzebnych i nadmierne wymagania. To działanie pozornie nieszkodliwie (sondowania witryny sieci Web) spowoduje, że urządzenie stanie się bezużyteczny w względnie krótkim czasie.

Android udostępnia następujące interfejsy API ułatwiające wykonywanie pracy w tle, ale samodzielnie nie są wystarczające do planowania zadań inteligentnego. 

* **[Usługi w celu](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; celem usługi są doskonałe do wykonywania pracy, jednak zapewniają sposób harmonogramu pracy.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; te interfejsy API umożliwiają tylko pracy można zaplanować, ale nie umożliwiają faktycznie wykonać operację. Ponadto AlarmManager umożliwia tylko ograniczenia na podstawie czasu, co oznacza, że podnieść alarmu w określonym czasie lub po upływie pewnego czasu. 
* **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)**  &ndash; harmonogram zadań to doskonały interfejs API, który działa w systemie operacyjnym do planowania zadań. Jednak jest tylko dostępne dla tych aplikacji systemu Android, które odnoszą się do poziom interfejsu API 21 lub nowszej. 
* **[Emisji odbiorcy](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; aplikacji systemu Android można skonfigurować emisji odbiorcy do wykonywania pracy w odpowiedzi na zdarzenia systemowe lub lokalizacji docelowych. Jednak emisji odbiorcy nie udostępniają kontrolę uruchamiania zadania. Również ograniczy zmian w systemie Android podczas emisji odbiorcy będą działać lub rodzaje pracy, którą może odpowiedzieć. 

Istnieją dwa kluczowe funkcje do wykonywania wydajnie Praca w tle (czasami zwanej _zadanie w tle_ lub _zadania_):

1. **Inteligentnie Planowanie pracy** &ndash; należy pamiętać, że gdy aplikacja jest operacją pracy w tle robi to jako dobra obywateli. Najlepiej, jeśli w aplikacji uruchomić zadania nie powinna wymagać. Zamiast tego aplikacja powinna określają warunki, które muszą być spełnione, gdy zadanie można uruchomić, a następnie Zaplanuj pracy do uruchomienia po spełnieniu warunków. Dzięki temu systemu Android do inteligentnie wykonywania pracy. Na przykład żądania sieci mogą można umieścić w partii do uruchomienia wszystkich w tym samym czasie, aby maksymalne użycie koszty związane z obsługą sieci.
2. **Zawieranie pracy** &ndash; kod, aby wykonać Praca w tle należy hermetyzowany w odrębny składnik, który może działać niezależnie od interfejsu użytkownika i będzie stosunkowo łatwa do ponownie zaplanować, jeśli praca nie powiedzie się niektóre przyczyny.

Dyspozytor zadania Firebase to biblioteka z Google, która zapewnia interfejs API fluent uprościć planowania Praca w tle. Ma ona być zastąpienia dla Menedżera chmury Google. Dyspozytor zadania Firebase składa się z następujących interfejsów API:

* A `Firebase.JobDispatcher.JobService` jest klasą abstrakcyjną, która musi zostać rozszerzony z logiką, który zostanie wykonany w tle zadanie.
* A `Firebase.JobDispatcher.JobTrigger` deklaruje się rozpoczęcie zadania. To jest zwykle wyrażony jako okno czasu, na przykład, poczekaj co najmniej 30 sekund przed wykonaniem zadania, ale Uruchom zadanie w ciągu 5 minut.
* A `Firebase.JobDispatcher.RetryStrategy` zawiera informacje o co należy zrobić, gdy zadanie wykonanie nie powiedzie się poprawnie. Strategia ponownych prób określa, jak długo czekać przed podjęciem próby ponownego uruchomienia zadania. 
* A `Firebase.JobDispatcher.Constraint` to wartość opcjonalna, który opisuje warunek, który muszą zostać spełnione przed uruchomienia zadania, takie jak urządzenie znajduje się w sieci unmetered lub ładowania.
* `Firebase.JobDispatcher.Job` To interfejs API, który łączy poprzedniej interfejsy API w celu jednostkę pracy, które mogą być planowane przez `JobDispatcher`. `Job.Builder` Klasa jest używana do utworzenia wystąpienia `Job`.
* A `Firebasee.JobDispatcher.JobDispatcher` używa trzech poprzednich interfejsy API do zaplanowania pracy w systemie operacyjnym i umożliwiają anulować zadania, jeśli to konieczne.

Aby zaplanować pracę z dyspozytora zadania Firebase, aplikacji platformy Xamarin.Android musi hermetyzować kodu w typie, rozszerzający `JobService` klasy. `JobService` ma trzy metody cyklu życia który, które może być wywołane przez cały okres istnienia zadania:

* **`bool OnStartJob(IJobParameters parameters)`** &ndash; Ta metoda jest, gdzie pracy nastąpi i zawsze powinny zostać wdrożone. Jest ono wykonywane w głównym wątku. Ta metoda zwróci `true` w przypadku pracy pozostało, lub `false` Jeśli praca jest wykonywana. 
* **`bool OnStopJob(IJobParameters parameters)`** &ndash; Jest to, gdy zadanie zostało zatrzymane dla jakiegoś powodu. Powinien on zwrócić `true` Jeśli zadanie powinno przełożone na później.
* **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; Ta metoda jest wywoływana, gdy `JobService` zakończył pracę asynchronicznego. 

Zaplanowane zadanie, aplikacja będzie wystąpienia `JobDispatcher` obiektu. A następnie `Job.Builder` służy do tworzenia `Job` obiektu, który został dostarczony do `JobDispatcher` co będzie spróbuj i zaplanować zadania do uruchomienia.

W tym przewodniku będzie omawiać temat Dodawanie Firebase dyspozytora zadań do aplikacji platformy Xamarin.Android i użyć go do zaplanowania Praca w tle.

## <a name="requirements"></a>Wymagania

Dyspozytor zadania Firebase wymaga poziom interfejsu API systemu Android 9 lub nowszą. Biblioteka dyspozytora zadania Firebase opiera się na niektórych składników podał usług Google Play; urządzenie musi mieć usług Google Play zainstalowane.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Za pomocą biblioteki Firebase zadania dyspozytora platformie Xamarin.Android

Aby rozpocząć pracę z Firebase dyspozytora zadań, należy najpierw dodać [pakietu Xamarin.Firebase.JobDispatcher NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) w projekcie platformy Xamarin.Android. Wyszukaj Menedżera pakietów NuGet dla **Xamarin.Firebase.JobDispatcher** pakietu (czyli w wersji wstępnej).

Po dodaniu biblioteki Firebase dyspozytora zadań, należy utworzyć `JobService` klasy, a następnie zaplanować uruchomienie przy użyciu wystąpienia `FirebaseJobDispatcher`.

> [!NOTE]
> Bieżącego powiązania dla dyspozytora zadania Firebase jest przeznaczony dla starszej wersji biblioteki. Jest [znaną usterką [(https://bugzilla.xamarin.com/show_bug.cgi?id=59046)] uniemożliwiający powiązania aktualizacji nowsza wersja docelowa platformy Firebase dyspozytora zadania.


### <a name="creating-a-jobservice"></a>Tworzenie JobService

Wszystkie pracy wykonanej przez bibliotekę Firebase zadania dyspozytora musi odbywać się w typie, rozszerzający `Firebase.JobDispatcher.JobService` klasy abstrakcyjnej. Tworzenie `JobService` jest bardzo podobny do tworzenia `Service` z platformy systemu Android: 

1. Rozszerzanie `JobService` — klasa
2. Dekoracji podklasy z `ServiceAttribute`. Mimo że nie są ściśle wymagane, zalecane jest jawnie ustawiona `Name` parametr, aby pomóc w debugowaniu `JobService`. 
3. Dodaj `IntentFilter` Aby zadeklarować `JobService` w **AndroidManifest.xml**. Pomoże to również biblioteki dyspozytora zadania Firebase zlokalizuj i wywołania `JobService`.

Następujący kod jest przykładem najprostszą `JobService` dla aplikacji, za pomocą TPL asynchronicznie wykonania dodatkowych czynności:

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Task.Run(() =>
        {
            // Work is happening asynchronously (code omitted)
                       
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### <a name="creating-a-firebasejobdispatcher"></a>Tworzenie FirebaseJobDispatcher

Przed można zaplanować pracę, należy utworzyć `Firebase.JobDispatcher.FirebaseJobDispatcher` obiektu. `FirebaseJobDispatcher` Jest odpowiedzialny za planowanie `JobService`. Poniższy fragment kodu jest jednym ze sposobów tworzenia wystąpienia `FirebaseJobDispatcher`: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

W poprzednim fragmencie kodu `GooglePlayDriver` jest klasa, która ułatwia `FirebaseJobDispatcher` interakcji z niektórych planowania interfejsów API w usług Google Play na urządzeniu. Parametr `context` jest żadnych Android `Context`, takie jak działania. Obecnie `GooglePlayDriver` jest jedyną `IDriver` implementacji w bibliotece Firebase zadania dyspozytora. 

Powiązanie Xamarin.Android dyspozytora zadania Firebase udostępnia metody rozszerzenia, aby utworzyć `FirebaseJobDispatcher` z `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Raz `FirebaseJobDispatcher` został uruchomiony, jest możliwość utworzenia `Job` i uruchamianie kodu w `JobService` klasy. `Job` Jest tworzony przez `Job.Builder` obiektu i zostanie omówiona w następnej sekcji.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Tworzenie Firebase.JobDispatcher.Job z Job.Builder

`Firebase.JobDispatcher.Job` Klasy jest odpowiedzialny za, hermetyzując meta dane niezbędne do uruchomienia `JobService`. A`Job` zawiera informacje, takie jak wszystkie ograniczenia, które muszą zostać spełnione przed uruchomienia zadania, jeśli `Job` cykliczne, lub żadne wyzwalacze, które spowoduje, że aby uruchomić zadanie.  Co najmniej systemu od zera `Job` musi mieć _tag_ (unikatowy ciąg identyfikujący zadanie `FirebaseJobDispatcher`) i typ `JobService` powinny być uruchamiane. Dyspozytor zadania Firebase zostanie utworzenie wystąpienia `JobService` po nadszedł czas, aby uruchomić to zadanie.  A `Job` jest tworzona przy użyciu wystąpienia `Firebase.JobDispatcher.Job.JobBuilder` klasy. 

Poniższy fragment kodu jest najprostsza przykład sposobu tworzenia `Job` za pomocą tego powiązania Xamarin.Android:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` Będzie wykonywać pewne podstawowe sprawdzanie poprawności kontrole wartości wejściowe dla zadania. Wyjątek będzie generowany, jeśli go nie jest możliwe do `Job.Builder` do utworzenia `Job`.  `Job.Builder` Utworzy `Job` przy użyciu następujących ustawień domyślnych:

* A `Job`w _okres istnienia_ (jak długo go zostanie zaplanowane do uruchomienia) jest tylko do momentu ponownego uruchamiania urządzenia &ndash; po ponownym uruchomieniu urządzenia `Job` zostaną utracone.
* A `Job` nie jest cykliczny &ndash; zostanie uruchomiony tylko raz.
* A `Job` zostanie zaplanowane do uruchomienia tak szybko, jak to możliwe.
* Strategia ponawiania domyślne `Job` jest użycie _wykładniczego wycofywania_ (w bardziej szczegółowo poniżej w sekcji [ustawienie RetryStrategy](#Setting_a_RetryStrategy))

### <a name="scheduling-a-job"></a>Planowanie zadania

Po utworzeniu `Job`, musi on zostać zaplanowane z `FirebaseJobDispatcher` przed uruchomieniem. Istnieją dwie metody planowania `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Wartość zwrócona przez `FirebaseJobDispatcher.Schedule` będzie jedną z następujących wartości całkowitych:

* `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; `Job` Została zaplanowana pomyślnie.
* `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; Wystąpił nieznany problem, który uniemożliwił `Job` z zaplanowane.
* `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; Nieprawidłowy `IDriver` użyto lub `IDriver` jakiś sposób był niedostępny. 
* `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger` Nie jest obsługiwana.
* `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; Usługa nie została poprawnie skonfigurowana lub jest niedostępny.
 
### <a name="configuring-a-job"></a>Konfigurowanie zadania

Istnieje możliwość dostosowania zadania. Jak można dostosować zadania przykłady następujących czynności:

* [Przekazywanie parametrów z zadaniem](#Passing_Parameters_to_a_Job) &ndash; A `Job` może wymagać dodatkowych wartości do wykonywania pracy, na przykład pobierania pliku.
* [Ustaw ograniczenia](#Setting_Constraints) &ndash; może być konieczne uruchomienie zadania tylko, gdy zostaną spełnione określone warunki. Na przykład uruchomić tylko `Job` gdy urządzenie jest ładowana. 
* [Określ, kiedy `Job` powinno być ono uruchomione](#Setting_Job_Triggers) &ndash; dyspozytora zadania Firebase umożliwia aplikacjom określić czas uruchomienia zadania.  
* [Deklarowanie strategię ponów próbę wykonania zadania zakończone niepowodzeniem](#Setting_a_RetryStrategy) &ndash; A _ponów strategii_ zawiera wskazówki dotyczące `FirebaseJobDispatcher` co zrobić z `Jobs` który nie powiedzie się. 

Każdego z tych tematów zostanie omówiona bardziej w poniższych sekcjach.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-jarameters-to-a-job"></a>Przekazywanie jarameters do zadania

Parametry są przekazywane do zadania, tworząc `Bundle` jest ono przekazywane wraz z programem `Job.Builder.SetExtras` metody:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

`Bundle` Jest dostępny z `IJobParameters.Extras` właściwość `OnStartJob` metody:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>Ustawienie ograniczenia

Ograniczenia może ułatwić zmniejszenie kosztów lub baterii opróżniania na urządzeniu. `Firebase.JobDispatcher.Constraint` Klasa definiuje tych warunków ograniczających jako wartości całkowite:

* `Constraint.OnUnmeteredNetwork` &ndash; Uruchom zadanie tylko, gdy urządzenie jest połączone z siecią unmetered. Dzięki takiemu grupowaniu można uniemożliwić użytkownikowi naliczania opłat danych.
* `Constraint.OnAnyNetwork` &ndash; Uruchom zadanie w sieci, niezależnie od urządzenie jest połączone. Jeśli określona wraz z programem `Constraint.OnUnmeteredNetwork`, wyższy priorytet będzie miało tę wartość.
* `Constraint.DeviceCharging` &ndash; Tylko wtedy, gdy urządzenie jest ładowana, należy uruchomić zadanie.

Ograniczenia są konfigurowane przy użyciu `Job.Builder.SetConstraint` metody: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger` Zawiera wskazówki dotyczące systemu operacyjnego dotyczące rozpoczęcia zadania. A `JobTrigger` ma _wykonywania okna_ definiuje zaplanowanym czasie program `Job` powinno być ono uruchomione. Okna wykonania ma _uruchomić okno_ wartość i _zakończenia okna_ wartość. Okno rozpoczęcia jest liczbę sekund, które urządzenia ma odczekać przed uruchomieniem zadania i wartość końcowa okna jest maksymalna liczba sekund oczekiwania przed uruchomieniem `Job`. 

A `JobTrigger` mogą być tworzone za pomocą `Firebase.Jobdispatcher.Trigger.ExecutionWindow` metody.  Na przykład `Trigger.ExecutionWindow(15,60)` oznacza, że zadanie powinno być ono uruchomione od 15 do 60 sekund z zaplanowanym. `Job.Builder.SetTrigger` Jest używane do — metoda 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

Wartość domyślna `JobTrigger` dla zadania jest reprezentowany przez wartość `Trigger.Now`, który określa, że zadanie można uruchomić jak najszybciej po planowany..

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Ustawienie RetryStrategy

`Firebase.JobDispatcher.RetryStrategy` Służy do określania, jaka część opóźnienie urządzenia należy używać przed podjęciem próby Aby ponownie uruchomić zadanie zakończone niepowodzeniem. A `RetryStrategy` ma _zasad_, który definiuje, jakie algorytm podstawowy czas ma zostać użyty, aby ponownie zaplanować zadanie zakończone niepowodzeniem oraz okna wykonania, który określa okno, w którym powinny zostać zaplanowane zadanie. To _planowaniem mogą okna_ jest definiowana za pomocą dwóch wartości. Pierwsza wartość to liczba sekund oczekiwania przed ponowne zaplanowanie zadania ( _początkowej wycofywania_ wartość), i druga liczba jest maksymalna liczba sekund przed należy uruchomić zadanie ( _maksymalną wycofywania_wartości). 
 
Dwa typy zasad ponawiania są identyfikowane przez te wartości int:

* `RetryStrategy.RetryPolicyExponential` &ndash; _Wykładniczego wycofywania_ zasad spowoduje zwiększenie wartości początkowej wycofywania wykładniczo po każdym błędzie. Podczas pierwszego zadania nie powiedzie się, biblioteka będzie czekać interwał _initial określono przed ponowne zaplanowanie zadania &ndash; przykład 30 sekund. Zadanie nie powiedzie się, po raz drugi biblioteki będzie oczekiwał co najmniej 60 sekund przed ponowną próbą uruchomienia zadania. Po trzeci próba nie powiodła się, biblioteka 120 sekund oczekiwania i tak dalej. Wartość domyślna `RetryStrategy` dla dyspozytora zadania Firebase biblioteki jest reprezentowana przez `RetryStrategy.DefaultExponential` obiektu. Ma początkowej wycofywania 30 sekund i maksymalna wycofywania wynoszącej 3600 sekund.
* `RetryStrategy.RetryPolicyLinear` &ndash; Ta strategia jest _liniowej wycofywania_ czy zadanie powinno można przeprowadzić na ustawienie interwałów (dopóki nie zakończy się pomyślnie). Liniowy wycofywania najlepiej nadaje się do pracy, które należy wykonać jak najszybciej lub problemów, które szybko rozwiązać samodzielnie. Biblioteka dyspozytora zadania Firebase definiuje `RetryStrategy.DefaultLinear` mającego planowaniem mogą okna co najmniej 30 sekund i do 3600 sekund.

Umożliwia zdefiniowanie niestandardowego `RetryStrategy` z `FirebaseJobDispatcher.NewRetryStrategy` metody. Trwa trzy parametry:

1. `int policy` &ndash; _Zasad_ jest jednym z poprzedniej `RetryStrategy` wartości, `RetryStrategy.RetryPolicyLinear`, lub `RetryStrategy.RetryPolicyExponential`.
2. `int intialBackoffSeconds` &ndash; _Początkowej wycofywania_ z opóźnieniem, w sekundach, która jest wymagana przed podjęciem próby ponownego uruchomienia zadania. Wartość domyślna to to 30 sekund. 
3. `int maximumBackoffSeconds` &ndash; _Maksymalna wycofywania_ wartość deklaruje maksymalną liczbę sekund opóźnienia przed podjęciem próby ponownego uruchomienia zadania. Wartość domyślna to 3600 sekund. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Anulowanie zadania

Jest to możliwe anulować wszystkie zadania, które zostały zaplanowane lub po prostu jednego zadania za pomocą polecenia `FirebaseJobDispatcher.CancelAll()` — metoda lub `FirebaseJobDispatcher.Cancel(string)` metody:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Każda metoda zwróci wartość całkowitą:

* `FirebaseJobDispatcher.CancelResultSuccess` &ndash; Zadanie zostało pomyślnie anulowane.
* `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; Błąd uniemożliwił Trwa anulowanie zadania.
* `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; `FirebaseJobDispatcher` Nie może anulować zadania, ponieważ nie ma nie prawidłowy `IDriver` dostępne.

## <a name="summary"></a>Podsumowanie

W tym przewodniku omówiono sposób użycia dyspozytora zadania Firebase może inteligentnie wykonywać zadania w tle. Go omówiony sposób Hermetyzowanie pracy do wykonania jako `JobService` i sposobu użycia `FirebaseJobDispatcher` do zaplanowania pracy, określając kryteria `JobTrigger` i sposób obsługi błędów z `RetryStrategy`.


## <a name="related-links"></a>Linki pokrewne

- [Generator powiązania kończy się niepowodzeniem z nieobsługiwany WYJĄTEK krytyczny błąd: System.ArgumentNullException: wartość nie może mieć wartości null.](https://bugzilla.xamarin.com/show_bug.cgi?id=59046)
- [Xamarin.Firebase.JobDispatcher na NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [firebase zadania dyspozytora w witrynie GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Powiązanie Xamarin.Firebase.JobDispatcher](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Inteligentnego Planowanie zadań](https://developer.android.com/topic/performance/scheduling.html)
- [Android baterii i optymalizacji pamięci - Google we/wy 2016 (klip wideo)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
