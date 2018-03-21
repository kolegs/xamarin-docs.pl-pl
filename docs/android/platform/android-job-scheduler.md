---
title: "Harmonogram zadań dla systemu android"
description: "W tym przewodniku omówiono sposób tworzenia harmonogramu praca w tle przy użyciu interfejsu API systemu Android harmonogramu zadań."
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: dc72b7e4da330185b00541f923d9c4b64b91bc95
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2018
---
# <a name="android-job-scheduler"></a>Harmonogram zadań dla systemu android

_W tym przewodniku opisano, jak zaplanować Praca w tle przy użyciu zadania harmonogramu interfejsu API systemu Android, która jest dostępna na urządzeniach z systemem Android 5.0 (poziom interfejsu API 21) lub nowszy._


## <a name="overview"></a>Omówienie 

Jednym z najlepszych sposobów zachowanie reakcji użytkownikowi aplikacji systemu Android jest upewnij się, że złożonych lub długotrwałych zadań są wykonywane w tle. Jednak jest ważne, aby praca w tle nie ma negatywnego wpływu przez użytkownika z urządzeniem. 

Na przykład zadanie w tle może sondować witryny sieci Web co trzy lub cztery minut do zapytania zmiany określonego zestawu danych. Prawdopodobnie niegroźne, jednak byłyby Fatalne wpływu na czas pracy baterii. Aplikacji będzie wielokrotnie wznawiania urządzenia, podnieść poziom Procesora do wyższych stan zasilania, włączenia komunikacji radiowej, żądania sieci, a następnie przetwarzania wyników. Pobiera on gorsza, ponieważ urządzenie nie jest od razu zasilanie i nie powrócić do stanu bezczynności niskiego poboru energii. Praca w tle nieprawidłowo zaplanowane przypadkowo mogą przechowywać urządzenie w stanie z zasilania niepotrzebnych i nadmierne wymagania. To działanie pozornie nieszkodliwie (sondowania witryny sieci Web) spowoduje, że urządzenie stanie się bezużyteczny w względnie krótkim czasie.

Android udostępnia następujące interfejsy API ułatwiające wykonywanie pracy w tle, ale samodzielnie nie są wystarczające do planowania zadań inteligentnego. 

* **[Usługi w celu](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; celem usługi są doskonałe do wykonywania pracy, jednak zapewniają sposób harmonogramu pracy.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; te interfejsy API umożliwiają tylko pracy można zaplanować, ale nie umożliwiają faktycznie wykonać operację. Ponadto AlarmManager umożliwia tylko ograniczenia na podstawie czasu, co oznacza, że podnieść alarmu w określonym czasie lub po upływie pewnego czasu. 
* **[Emisji odbiorcy](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; aplikacji systemu Android można skonfigurować emisji odbiorcy do wykonywania pracy w odpowiedzi na zdarzenia systemowe lub lokalizacji docelowych. Jednak emisji odbiorcy nie udostępniają kontrolę uruchamiania zadania. Również ograniczy zmian w systemie Android podczas emisji odbiorcy będą działać lub rodzaje pracy, którą może odpowiedzieć. 

Istnieją dwa kluczowe funkcje do wykonywania wydajnie Praca w tle (czasami zwanej _zadanie w tle_ lub _zadania_):

1. **Inteligentnie Planowanie pracy** &ndash; należy pamiętać, że gdy aplikacja jest operacją pracy w tle robi to jako dobra obywateli. Najlepiej, jeśli w aplikacji uruchomić zadania nie powinna wymagać. Zamiast tego aplikacja powinna określają warunki, które muszą być spełnione, gdy zadanie można uruchomić, a następnie zaplanować zadania w systemie operacyjnym, który będzie wykonywał pracę, gdy są spełnione warunki. Dzięki temu systemu Android do wykonywania zadania w celu zapewnienia maksymalnej wydajności na urządzeniu. Na przykład żądania sieci mogą można umieścić w partii do uruchomienia wszystkich w tym samym czasie, aby maksymalne użycie koszty związane z obsługą sieci.
2. **Zawieranie pracy** &ndash; kod, aby wykonać Praca w tle należy hermetyzowany w odrębny składnik, który może działać niezależnie od interfejsu użytkownika i będzie stosunkowo łatwa do ponownie zaplanować, jeśli praca nie powiedzie się niektóre przyczyny.

Harmonogram zadań dla systemu Android to platforma wbudowanej w system operacyjny Android udostępnia interfejs API fluent uprościć planowania Praca w tle.  Harmonogram zadań dla systemu Android składa się z następujących typów:

* `Android.App.Job.JobScheduler` Jest usługa systemowa, która służy do planowania, wykonywania i w razie potrzeby anulować, zadań w imieniu aplikacji systemu Android.
* `Android.App.Job.JobService` Jest klasą abstrakcyjną, która musi zostać rozszerzony z logiką, które zostanie uruchomione zadanie na wątku głównego aplikacji. Oznacza to, że `JobService` jest odpowiedzialny za sposób pracy ma być wykonywane asynchronicznie.
* `Android.App.Job.JobInfo` Obiekt zawiera kryteria prowadzące systemu Android, gdy zadanie powinno zostać uruchomione.

Aby zaplanować pracy z systemem Android harmonogramu zadań, do aplikacji platformy Xamarin.Android musi hermetyzować kod w klasie, rozszerzający `JobService` klasy. `JobService` ma trzy metody cyklu życia który, które może być wywołane przez cały okres istnienia zadania:

* **wartość logiczna (parametry JobParameters) OnStartJob** &ndash; ta metoda jest wywoływana przez `JobScheduler` do wykonywania pracy i działa na wątku głównego aplikacji. Jest on odpowiedzialny za `JobService` asynchronicznie wykonywanie pracy i `true` w przypadku pracy pozostało, lub `false` Jeśli praca jest wykonywana.
    
    Gdy `JobScheduler` wywołuje tę metodę, zostanie żądania i zachować wakelock z systemem Android na czas trwania zadania. Po zakończeniu zadania jest odpowiedzialny za `JobService` mówić `JobScheduler` o tym fakcie przez wywołanie `JobFinished` — metoda (zostało to opisane poniżej).

* **JobFinished (parametry JobParameters, bool needsReschedule)** &ndash; ta metoda musi zostać wywołana `JobService` mówić `JobScheduler` który praca jest wykonywana. Jeśli `JobFinished` nie jest wywoływany `JobScheduler` nie spowoduje usunięcia wakelock, powoduje zużycie baterii niepotrzebne. 

* **wartość logiczna (parametry JobParameters) OnStopJob** &ndash; jest to, gdy zadanie zostanie przedwcześnie zatrzymana przez system Android. Powinien on zwrócić `true` Jeśli zadanie powinno można przeprowadzić na podstawie kryteriów ponownych prób (bardziej szczegółowo opisanych poniżej).

Istnieje możliwość określenia _ograniczenia_ lub _wyzwalaczy_ nastąpi po zadania można lub powinno być ono uruchomione. Na przykład jest możliwe ograniczyć zadania, dzięki czemu będzie je uruchomić tylko, gdy urządzenie jest ładowana lub się uruchomienie zadania, gdy obraz jest zajęta.

W tym przewodniku będzie omawiać szczegółowo implementowania `JobService` klasy i zaplanowania jej z `JobScheduler`.

## <a name="requirements"></a>Wymagania

Harmonogram zadań dla systemu Android wymaga poziom interfejsu API systemu Android (Android 5.0) 21 lub nowszej. 

## <a name="using-the-android-job-scheduler"></a>Za pomocą harmonogramu zadań systemu Android

Istnieją trzy kroki dotyczące korzystania z interfejsu API systemu Android JobScheduler:

1. Implementuje typu JobService w celu hermetyzacji pracy.
2. Użyj `JobInfo.Builder` obiekt, aby utworzyć `JobInfo` obiektu, w którym będą przechowywane kryteria `JobScheduler` uruchomić zadanie. 
3. Zaplanuj zadania za pomocą polecenia `JobScheduler.Schedule`.

### <a name="implement-a-jobservice"></a>Implementowanie JobService

Wszystkie pracy wykonanej w bibliotece harmonogramu zadań systemu Android musi odbywać się w typie, rozszerzający `Android.App.Job.JobService` klasy abstrakcyjnej. Tworzenie `JobService` jest bardzo podobny do tworzenia `Service` z platformy systemu Android: 

1. Rozszerzanie `JobService` klasy.
2. Dekoracji podklasy z `ServiceAttribute` i ustaw `Name` parametru na ciąg, który składa się z nazwy pakietu i nazwę klasy (zobacz poniższy przykład).
3. Ustaw `Permission` właściwość `ServiceAttribute` ciąg `android.permission.BIND_JOB_SERVICE`.
4. Zastąpienie `OnStartJob` metody Dodawanie kodu do wykonywania pracy. Android wywoła tę metodę w wątku głównego aplikacji, aby uruchomić to zadanie. Praca będzie trwać dłużej, że kilka milisekund powinna być wykonana na wątku w celu unikania blokowania aplikacji.
5. Po zakończeniu pracy `JobService` należy wywołać `JobFinished` metody. Ta metoda jest sposób `JobService` informuje `JobScheduler` czy praca jest wykonywana. Nie można wywołać `JobFinished` spowoduje `JobService` umieszczanie niepotrzebnych zapotrzebowanie na urządzeniu, skrócić czas pracy baterii. 
6. Jest dobrym pomysłem jest również zastąpić `OnStopJob` metody. Ta metoda jest wywoływana przez system Android, gdy zadanie jest zamykany, zanim została zakończona która zapewnia `JobService` możliwość prawidłowo Zlikwiduj wszelkie zasoby. Ta metoda powinna zwrócić `true` w razie potrzeby zmienić harmonogram zadania, lub `false` Jeśli nie jest desireable, aby ponownie uruchomić zadanie.
   

Następujący kod jest przykładem najprostszą `JobService` dla aplikacji, za pomocą TPL asynchronicznie wykonania dodatkowych czynności:

```csharp
[Service(Name = "com.xamarin.samples.downloadscheduler.DownloadJob", 
         Permission = "android.permission.BIND_JOB_SERVICE")]
public class DownloadJob : JobService
{
    public override bool OnStartJob(JobParameters jobParams)
    {            
        Task.Run(() =>
        {
            // Work is happening asynchronously
                      
            // Have to tell the JobScheduler the work is done. 
            JobFinished(jobParams, false);
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(JobParameters jobParams)
    {
        // we don't want to reschedule the job if it is stopped or cancelled.
        return false; 
    }
}
```

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>Tworzenie informacji o zadaniu tworzenia harmonogramu zadań

Tworzy wystąpienia aplikacji platformy Xamarin.Android `JobService` bezpośrednio, zamiast tego przekaże `JobInfo` do obiektu `JobScheduler`. `JobScheduler` Będzie utworzyć wystąpienia żądanego `JobService` obiektu, planowania i uruchamiania `JobService` zgodnie z metadanych w `JobInfo`. A `JobInfo` obiektu musi zawierać następujące informacje:

* **Identyfikator zadania** &ndash; jest `int` wartość, która służy do identyfikowania zadania `JobScheduler`. Ponowne użycie tej wartości spowoduje zaktualizowanie istniejących zadania. Wartość musi być unikatowa dla aplikacji. 
* **JobService** &ndash; ten parametr jest `ComponentName` które jednoznacznie identyfikuje typ który `JobScheduler` powinna być używana do uruchomienia zadania. 
  

Ta metoda rozszerzenia przedstawia sposób tworzenia `JobInfo.Builder` z Android `Context`, takie jak działania:

```csharp
public static class JobSchedulerHelpers
{
    public static JobInfo.Builder CreateJobBuilderUsingJobId<T>(this Context context, int jobId) where T:JobService
    {
        var javaClass = Java.Lang.Class.FromType(typeof(T));
        var componentName = new ComponentName(context, javaClass);
        return new JobInfo.Builder(jobId, componentName);
    }
}

// Sample usage - creates a JobBuilder for a DownloadJob andsets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creats a JobInfo object.
```

Rozbudowanych funkcji systemu Android harmonogramu zadań jest możliwość kontrolowania po uruchomieniu zadania lub może działać, w jakich warunki zadania. W poniższej tabeli opisano niektóre metody na `JobInfo.Builder` umożliwiające aplikacja może spowodować uruchomienie zadania:  


|  Metoda | Opis   |
|---|---|
| `SetMinimumLatency`  | Określa, że opóźnienie (w milisekundach), który powinien być uwzględniony przed zadanie jest uruchomione. |
| `SetOverridingDeadline`  | Deklaruje, że zadanie należy uruchomić przed upływem tego czasu (w milisekundach). |
| `SetRequiredNetworkType`  | Określa wymagania sieciowe dotyczące zadania. |
| `SetRequiresBatteryNotLow` | Zadania mogą uruchamiać tylko w sytuacji, gdy urządzenia nie są wyświetlane ostrzeżenia "niskim poziomie naładowania baterii" dla użytkownika. |
| `SetRequiresCharging` | Zadanie może uruchomiony tylko wtedy, gdy jest ładowana baterii. |
| `SetDeviceIdle` | Zadanie będzie uruchamiane, gdy urządzenie jest zajęty. |
| `SetPeriodic` | Określa, w którym zadanie powinno być regularnie uruchamiane. |
| `SetPersisted` | Zadanie powinno perisist między ponownymi uruchomieniami komputera urządzenia. | 


`SetBackoffCriteria` Zawiera pewne wskazówki na temat długo `JobScheduler` powinien zaczekać na próby Uruchom zadanie ponownie. Kryteria wycofywania, składa się z dwóch części: opóźnienie w milisekundach (domyślnie 30 sekund) i typ wycofywania, które mają być używane (czasami zwanej _wycofywania zasad_ lub _zasady ponawiania_) . Dwie zasady są hermetyzowane w `Android.App.Job.BackoffPolicy` wyliczenia:

* `BackoffPolicy.Exponential` &ndash; Zasady wykładniczego wycofywania spowoduje zwiększenie wartości początkowej wycofywania wykładniczo po każdym błędzie. Przy pierwszym uruchomieniu zadanie nie powiedzie się, biblioteka będzie czekać początkowej interwał, w którym określono przed ponowne zaplanowanie zadania — przykład 30 sekund. Zadanie nie powiedzie się, po raz drugi biblioteki będzie oczekiwał co najmniej 60 sekund przed ponowną próbą uruchomienia zadania. Po trzeci próba nie powiodła się, biblioteka 120 sekund oczekiwania i tak dalej. Jest to wartość domyślna.
* `BackoffPolicy.Linear` &ndash; Ta strategia jest wycofywania liniowej, w którym zadanie powinno zmienił do uruchamiania w ustalonych odstępach czasu (dopóki nie zakończy się pomyślnie). Liniowy wycofywania najlepiej nadaje się do pracy, które należy wykonać jak najszybciej lub problemów, które szybko rozwiązać samodzielnie. 

Więcej informacji na temat Utwórz `JobInfo` obiektu, przeczytaj [dokumentacji firmy Google `JobInfo.Builder` klasy](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html).

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>Przekazywanie parametrów zadania przy użyciu informacji o zadaniu

Parametry są przekazywane do zadania, tworząc `PersistableBundle` jest ono przekazywane wraz z programem `Job.Builder.SetExtras` metody:

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

`PersistableBundle` Jest dostępny z `Android.App.Job.JobParameters.Extras` właściwości w `OnStartJob` metody `JobService`:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>Planowanie zadania

Aby zaplanować zadanie, aplikacji platformy Xamarin.Android pobierze odwołanie do `JobScheduler` usługa systemowa i wywołanie `JobScheduler.Schedule` metody z `JobInfo` obiektu, który został utworzony w poprzednim kroku. `JobScheduler.Schedule` natychmiast zwraca jeden z dwóch wartości całkowite:

* **JobScheduler.ResultSuccess** &ndash; zadanie została zaplanowana pomyślnie. 
* **JobScheduler.ResultFailure** &ndash; nie można zaplanować zadanie. Jest to zazwyczaj spowodowane powodujące konflikt `JobInfo` parametrów.

Ten kod jest przykładem planowania zadania i powiadamianie użytkownika o wynikiem próby planowania:

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
}
else
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
}
```
 
### <a name="cancelling-a-job"></a>Anulowanie zadania

Jest to możliwe anulować wszystkie zadania, które zostały zaplanowane lub po prostu jednego zadania za pomocą polecenia `JobsScheduler.CancelAll()` — metoda lub `JobScheduler.Cancel(jobId)` metody:

```csharp
// Cancel all jobs
jobSchduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano inteligentnie wykonywania pracy w tle przy użyciu harmonogramu zadań systemu Android. Go omówiony sposób Hermetyzowanie pracy do wykonania jako `JobService` i sposobu użycia `JobScheduler` do zaplanowania pracy, określając kryteria `JobTrigger` i sposób obsługi błędów z `RetryStrategy`.

## <a name="related-links"></a>Linki pokrewne

- [Inteligentnego Planowanie zadań](https://developer.android.com/topic/performance/scheduling.html)
- [Dokumentacja interfejsu API JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [Planowanie zadań, takich jak pro z JobScheduler](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android baterii i optymalizacji pamięci - Google we/wy 2016 (klip wideo)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [University Xamarin android JobScheduler - René Ruppert-](https://www.youtube.com/watch?v=aSjBBPYjelE)