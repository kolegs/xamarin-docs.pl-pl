---
title: Aplikacje ćwiczeń
description: W tym artykule omówiono ulepszenia Apple wprowadził do aplikacji ćwiczeń w watchOS 3 oraz sposób ich wdrażania w programie Xamarin.
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 96eb2eaca15ed0bccbb4c5cdb6a855fc7e0e3bb1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="workout-apps"></a>Aplikacje ćwiczeń

_W tym artykule omówiono ulepszenia Apple wprowadził do aplikacji ćwiczeń w watchOS 3 oraz sposób ich wdrażania w programie Xamarin._


Nowe watchOS 3 ćwiczeń związane z, aplikacje mają możliwość uruchomione w tle na Apple Watch i uzyskania dostępu do danych HealthKit. Aplikacja systemu iOS 10 na podstawie ich nadrzędny ma również możliwość uruchamiania aplikacji na podstawie watchOS 3 bez interwencji użytkownika.

Poniższe tematy zostanie omówiona szczegółowo:

## <a name="about-workout-apps"></a>Dotyczące ćwiczeń aplikacji

Użytkownicy przydatności i ćwiczeń aplikacji mogą być bardzo dedykowanych udoskonalenie kilka godzin dnia kierunku celów ich kondycję i przydatności. W związku z tym oczekiwane reakcji, łatwe w użyciu aplikacje, które dokładnie zbierać i wyświetlać dane i bezproblemowo zintegrować z kondycji firmy Apple.

Przemyślany przydatności lub ćwiczeń aplikacji ułatwia użytkownikom ich działania, aby osiągnąć cele ich przydatności wykresu. Za pomocą Apple Watch, przydatności i ćwiczeń aplikacje mają natychmiastowy dostęp do Puls, wykrywanie Kaloria nagrywania i działania.

[![](workout-apps-images/workout01.png "Przykład aplikacji przydatności i ćwiczeń")](workout-apps-images/workout01.png#lightbox)

Jesteś nowym użytkownikiem watchOS 3, _uruchomione w tle_ aplikacje związane z ćwiczeń daje możliwość uruchomione w tle na Apple Watch i uzyskania dostępu do danych HealthKit.

Ten dokument zostanie wprowadzono funkcję uruchomione w tle, ćwiczeń cyklu życia aplikacji obejmują i pokazać, jak aplikacja ćwiczeń może przyczynić się do użytkownika _sygnałów działania_ na Apple Watch.

## <a name="about-workout-sessions"></a>Dotyczące ćwiczeń sesji

Każda aplikacja ćwiczeń to _sesji ćwiczeń_ (`HKWorkoutSession`), które użytkownik może uruchomić i zatrzymać. Interfejs API sesji ćwiczeń jest łatwa do wdrożenia i zapewnia kilka korzyści ćwiczeń aplikacji, takich jak:

- Ruchu i Kaloria nagrywanie wykrywanie na podstawie typu działania.
- Automatyczne udziału sygnałów działanie użytkownika.
- Znajduje się w sesji, aplikacja zostanie automatycznie wyświetlona zawsze, gdy użytkownik wznawia urządzenia (albo przez wywoływanie ich nadgarstka lub interakcji z Apple Watch).

## <a name="about-background-running"></a>O uruchomione w tle

Jak już wspomniano, z watchOS 3 aplikacji ćwiczeń można ustawić do uruchamiania w tle. Za pomocą tła uruchamiania aplikacji ćwiczeń może przetwarzać dane z czujników Apple Watch podczas działania w tle. Na przykład aplikację można nadal monitorować Puls użytkownika, nawet jeśli nie jest już wyświetlany na ekranie.

Uruchomione w tle umożliwia również użytkownik widzi opinii na żywo w dowolnym momencie podczas ćwiczeń sesji, takie jak wysyłanie dotykowych alert, aby poinformować użytkownika ich bieżący postęp.

Ponadto uruchomione w tle umożliwia szybkie zaktualizuj interfejs użytkownika, aby użytkownik ma najnowsze dane, gdy szybko przejrzeć, na ich Apple Watch aplikacji.

Aby zapewnić wysoką wydajność Apple Watch, aplikacji czujki, przy użyciu uruchomione w tle należy ograniczyć Praca w tle do oszczędzanie baterii. Jeśli aplikacja używa nadmiernego Procesora znajduje się w tle, można pobrać zawieszony przez watchOS.

### <a name="enabling-background-running"></a>Włączanie działa w tle

Aby włączyć uruchomione w tle, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie ikonę aplikacji rozszerzenia czujki pomocnika iPhone `Info.plist` plik, aby otworzyć do edycji.
2. Przełącz się do **źródła** widoku: 

    [![](workout-apps-images/plist01.png "Wyświetl źródło")](workout-apps-images/plist01.png#lightbox)
3. Dodaj nowy klucz o nazwie `WKBackgroundModes` i ustaw **typu** do `Array`: 

    [![](workout-apps-images/plist02.png "Dodaj nowy klucz o nazwie WKBackgroundModes")](workout-apps-images/plist02.png#lightbox)
4. Dodaj nowy element do tablicy z **typu** z `String` i wartości `workout-processing`: 

    [![](workout-apps-images/plist03.png "Dodaj nowy element do tablicy typu String i wartość ćwiczeń przetwarzania")](workout-apps-images/plist03.png#lightbox)
5. Zapisz zmiany w pliku.

## <a name="starting-a-workout-session"></a>Rozpoczynanie sesji ćwiczeń

Istnieją trzy główne kroki w celu rozpoczynania sesji ćwiczeń:

[![](workout-apps-images/workout02.png "Rozpoczynanie sesji ćwiczeń trzech głównych kroków")](workout-apps-images/workout02.png#lightbox)

1. Aplikacja musi żądać autoryzacji do dostępu do danych w HealthKit.
2. Utwórz obiekt ćwiczeń konfiguracji dla typu ćwiczeń uruchomienia.
3. Utwórz i Uruchom sesję ćwiczeń przy użyciu nowo utworzony konfiguracji ćwiczeń.

### <a name="requesting-authorization"></a>Żąda autoryzacji

Zanim aplikacja może uzyskiwać dostęp do danych HealthKit użytkownika, musi żądania i odbieranie autoryzację użytkownika. W zależności od charakteru aplikacji ćwiczeń może mieć następujące typy żądań:

- Autoryzacją do zapisu danych:
    - Ćwiczeń
- Zezwolenie na odczytywanie danych:
    - Kiedyś energii
    - odległość
    - Puls  

Przed aplikacji mogą żądać autoryzacji, musi być skonfigurowane do korzystania z HealthKit.

Wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Entitlements.plist` plik, aby otworzyć do edycji.
2. Przewiń w dół i sprawdź **włączyć HealthKit**: 

    [![](workout-apps-images/auth01.png "Sprawdź HealthKit Włącz")](workout-apps-images/auth01.png#lightbox)
3. Zapisz zmiany w pliku.
4. Postępuj zgodnie z instrukcjami [jawny identyfikator aplikacji i profilu inicjowania obsługi administracyjnej](~/ios/platform/healthkit.md) i [kojarzenia identyfikator aplikacji i Inicjowanie obsługi profilu z Twojej aplikacji platformy Xamarin.iOS](~/ios/platform/healthkit.md) sekcje [wprowadzenie do HealthKit](~/ios/platform/healthkit.md) artykułu poprawnie zainicjować obsługi aplikacji.
5. Na koniec, postępuj zgodnie z instrukcjami w [programowania zestawu kondycji](~/ios/platform/healthkit.md) i [żąda uprawnienia od użytkownika](~/ios/platform/healthkit.md) sekcje [wprowadzenie do HealthKit](~/ios/platform/healthkit.md) artykułu na żądanie zezwolenie na dostęp do magazynu danych HealthKit użytkownika.

### <a name="setting-the-workout-configuration"></a>Ustawienie konfiguracji ćwiczeń

Sesje ćwiczeń są tworzone za pomocą obiektu konfiguracji ćwiczeń (`HKWorkoutConfiguration`), który określa typ ćwiczeń (takich jak `HKWorkoutActivityType.Running`) i lokalizacja ćwiczeń (takich jak `HKWorkoutSessionLocationType.Outdoor`):

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>Tworzenie sesji ćwiczeń delegata 

Do obsługi zdarzeń, które mogą wystąpić podczas sesji ćwiczeń, należy utworzyć wystąpienie obiektu delegowanego sesji ćwiczeń aplikacji. Dodaj nową klasę w projekcie i utworzyć ją z `HKWorkoutSessionDelegate` klasy. Na przykład uruchom na zewnątrz może mieć następującą postać:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }
        #endregion
    }
}
```

Ta klasa tworzy kilka zdarzeń, które będą zgłaszane jako stanu zmiany sesji ćwiczeń (`DidChangeToState`) i w razie niepowodzenia sesji ćwiczeń (`DidFail`). 

### <a name="creating-a-workout-session"></a>Tworzenie sesji ćwiczeń

Za pomocą ćwiczeń konfiguracji i delegata sesji ćwiczeń utworzone powyżej do tworzenia nowej sesji ćwiczeń i należy ją uruchomić przed użytkownika domyślnego HealthKit magazynu:

```csharp
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...

private void StartOutdoorRun ()
{
    // Create a workout configuration
    var configuration = new HKWorkoutConfiguration () {
        ActivityType = HKWorkoutActivityType.Running,
        LocationType = HKWorkoutSessionLocationType.Outdoor
    };

    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (configuration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Jeśli użytkownik zmienia się ponownie do ich powierzchni czujki tej sesji ćwiczeń uruchomieniu aplikacji, niewielki rozmiar zielona ikona "man uruchomiona" zostanie wyświetlony powyżej kroju:

[![](workout-apps-images/workout03.png "Niewielki rozmiar zielona uruchomionych man ikona wyświetlana powyżej kroju")](workout-apps-images/workout03.png#lightbox)

Jeśli użytkownik naciska tę ikonę, ich nastąpi powrót do aplikacji.

## <a name="data-collection-and-control"></a>Zbieranie danych i kontrola

Po sesji ćwiczeń został skonfigurowany i uruchomiony, aplikacji konieczne będzie zbierać dane dotyczące sesji (na przykład Puls użytkownika) i kontroli stanu sesji:

[![](workout-apps-images/workout04.png "Zbieranie danych i Diagram sterowania")](workout-apps-images/workout04.png#lightbox)

1. **Przykłady obserwowania** -aplikacji będą musieli pobierać informacje z HealthKit, który będzie reagować i wyświetlane użytkownikowi.
2. **Obserwowania zdarzeń** — aplikacja będzie musiał odpowiadanie na zdarzenia generowane przez HealthKit oraz w Interfejsie użytkownika aplikacji (na przykład użytkownik wstrzymanie ćwiczeń).
3. **Wprowadź stan uruchomienia** -sesja została uruchomiona i jest obecnie uruchomiona.
4. **Stan wstrzymania** -użytkownika została wstrzymana w bieżącej sesji ćwiczeń i uruchomić go ponownie później. Użytkownik może przełączać się między Stanami działa i jest wstrzymana w jednej sesji ćwiczeń.
5. **Kończenie sesji ćwiczeń** — w dowolnym momencie użytkownik może zakończyć sesję ćwiczeń lub może wygaśnie i kończyć się na jego własnej, jeśli był naliczane ćwiczeń (na przykład uruchom dwóch mil).

Ostatnim krokiem jest być zapisane wyniki w sesji ćwiczeń użytkownika HealthKit z magazynem danych.

### <a name="observing-healthkit-samples"></a>Obserwowania HealthKit próbek

Aplikacja będzie trzeba otworzyć _zapytanie obiektu zakotwiczenia_ wszystkich danych HealthKit punktowi jest zainteresowana, takie jak kiedyś Puls lub aktywne energii. Dla każdego punktu danych obserwowana programu obsługi aktualizacji będzie muszą zostać utworzone do przechwytywania nowe dane, jak są wysyłane do aplikacji.

Z tych punktów danych aplikacji można accumulate sum (na przykład całkowita odległość wykonywania) i aktualizować jego interfejs użytkownika zgodnie z wymaganiami. Ponadto aplikacja może powiadomić użytkowników, po osiągnięciu określonego celu lub osiągnięcia, takich jak wykonanie następnego mil przebiegu.

Spójrz na następujący kod:

```csharp
private void ObserveHealthKitSamples ()
{
    // Get the starting date of the required samples
    var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

    // Get data from the local device
    var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
    var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

    // Assemble compound predicate
    var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

    // Get ActiveEnergyBurned
    var queryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
        // Valid?
        if (error == null) {
            // Yes, process all returned samples
            foreach (HKSample sample in addedObjects) {
                var quantitySample = sample as HKQuantitySample;
                ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);
            }
            
            // Update User Interface
            ...
        }
    });

    // Start Query
    HealthStore.ExecuteQuery (queryActiveEnergyBurned);
                                          
}
```

Tworzy predykat można ustawić daty początkowej, która chce uzyskać dane przy użyciu `GetPredicateForSamples` metody. Tworzy zbiór urządzeń do pobierania informacji o HealthKit z za pomocą `GetPredicateForObjectsFromDevices` metody, w tym przypadku tylko lokalne Apple Watch (`HKDevice.LocalDevice`). Predykaty dwóch są łączone w predykatu złożone (`NSCompoundPredicate`) przy użyciu `CreateAndPredicate` metody.

Nowy `HKAnchoredObjectQuery` jest tworzona dla żądanego punktu danych (w tym przypadku `HKQuantityTypeIdentifier.ActiveEnergyBurned` punktu aktywnego kiedyś energii), limit nie nakłada się na ilość danych zwróconych (`HKSampleQuery.NoLimit`) i programu obsługi aktualizacji jest zdefiniowany w celu obsługi danych jest zwracany do aplikacji z HealthKit. 

Procedura obsługi aktualizacji będzie wywoływana za każdym razem nowe dane są dostarczane do aplikacji dla danego punktu. Jeśli błąd jest zwracany, aplikację można bezpiecznie odczytywać dane, wprowadź wszelkie wymagane obliczenia i aktualizować jego interfejsie użytkownika zgodnie z wymaganiami.

Pętli wszystkich przykładów kodu (`HKSample`) zwracane w `addedObjects` tablicy i rzutuje je na próbkę ilość (`HKQuantitySample`). Następnie pobiera wartość elementu double próbki jako Joule — (`HKUnit.Joule`) i akumuluje ją do sumy active energii kiedyś dla ćwiczeń i aktualizacje interfejsu użytkownika.

### <a name="achieved-goal-notification"></a>Celem osiągnięte powiadomień

Jak wspomniano powyżej gdy użytkownik uzyskuje cel w aplikacji ćwiczeń (na przykład ukończenie pierwszego mil przebiegu) może wysłać opinię dotykowych do użytkownika za pomocą aparatu Taptic. Aplikacji należy również zaktualizować jej interfejsu użytkownika w tym momencie, ponieważ użytkownik zgłosi więcej niż prawdopodobnie ich nadgarstka, aby wyświetlić zdarzenia, które zostanie wyświetlony monit opinii.

Aby odtworzyć dotykowych opinii, należy użyć poniższego kodu:

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>Obserwowania zdarzeń

Zdarzenia są sygnatury czasowe, używaną przez aplikację do wyróżnienia niektórych punktów podczas ćwiczeń użytkownika. Niektóre zdarzenia zostanie utworzona bezpośrednio za pomocą aplikacji i zapisany w ćwiczeń i niektóre zdarzenia zostaną utworzone automatycznie HealthKit.

Aby przyjrzeć się zdarzenia, które zostały utworzone przy użyciu HealthKit, aplikacja zostanie zastąpione `DidGenerateEvent` metody `HKWorkoutSessionDelegate`:

```csharp
using System.Collections.Generic;
...

public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Save HealthKit generated event
    WorkoutEvents.Add (@event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Lap:
        break;
    case HKWorkoutEventType.Marker:
        break;
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

Apple dodał następujące nowe typy zdarzeń w watchOS 3:

- `HKWorkoutEventType.Lap` -Są zdarzenia, które Podziel ćwiczeń na fragmenty samej odległości. Na przykład dla oznaczenie jeden laptop wokół Śledź podczas pracy.
- `HKWorkoutEventType.Marker` -Należą do dowolnego punkty orientacyjne ćwiczeń. Na przykład osiągnie punkt określonych na trasie, uruchom na zewnątrz.

Te nowe typy mogą tworzonych przez aplikację i przechowywane w ćwiczeń do późniejszego użytku w tworzenie wykresów i statystyki.

Aby utworzyć zdarzenie znacznika, wykonaj następujące czynności:

```csharp
using System.Collections.Generic;
...

public float MilesRun { get; set; }
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public void ReachedNextMile ()
{
    // Create and save marker event
    var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
    WorkoutEvents.Add (markerEvent);

    // Notify user
    NotifyUserOfReachedMileGoal (++MilesRun);
}
```

Ten kod tworzy nowe wystąpienie klasy zdarzenia znacznika (`HKWorkoutEvent`) i zapisuje je w prywatnej kolekcji zdarzeń (które później zostaną zapisane w sesji ćwiczeń) i powiadamia użytkownika za pośrednictwem haptics zdarzenia.

### <a name="pausing-and-resuming-workouts"></a>Wstrzymywanie i wznawianie ćwiczeń

W dowolnym momencie w sesji ćwiczeń użytkownik może tymczasowo wstrzymać ćwiczeń i spowodują jego wznowienie w późniejszym czasie. Na przykład mogą one wstrzymać wewnętrznych Uruchom, aby pobrać wywołanie ważne i wznowienie uruchomienia po ukończeniu wywołania.

Interfejs użytkownika aplikacji powinien umożliwiają wstrzymywanie i wznawianie ćwiczeń (przez wywołanie HealthKit), dzięki czemu Apple Watch może zaoszczędzić miejsce zarówno zasilania, jak i dane, gdy użytkownik wstrzymał ich działania. Ponadto aplikacja ignorować wszystkie nowe punkty danych, które może być odebrane podczas sesji ćwiczeń jest w stanie wstrzymania.

HealthKit będzie odpowiadać na wstrzymywanie i wznawianie wywołania przez generowanie zdarzeń wstrzymania i wznowienia. Podczas sesji ćwiczeń jest wstrzymana, nie nowe zdarzenia lub dane zostaną wysłane do aplikacji przez HealthKit dopóki wznowić sesji.

Aby wstrzymać lub wznowić sesji ćwiczeń, należy użyć poniższego kodu:

```csharp
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public HKWorkoutSession WorkoutSession { get; set;}
...

public void PauseWorkout ()
{
    // Pause the current workout
    HealthStore.PauseWorkoutSession (WorkoutSession);
}

public void ResumeWorkout ()
{
    // Pause the current workout
    HealthStore.ResumeWorkoutSession (WorkoutSession);
}
```

Wstrzymywanie i wznawianie zdarzenia, które zostaną wygenerowane z HealthKit mogą być obsługiwane przez zastąpienie `DidGenerateEvent` metody `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);

    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

### <a name="motion-events"></a>Zdarzenia ruchu

Ponadto nowe watchOS 3 są wstrzymane ruchu (`HKWorkoutEventType.MotionPaused`) i wznawiać ruchu (`HKWorkoutEventType.MotionResumed`) zdarzenia. Te zdarzenia są generowane automatycznie przez HealthKit podczas ćwiczeń uruchomione gdy użytkownik uruchamia i zatrzymuje przenoszenia.

Gdy aplikacja odbiera zdarzenia wstrzymana ruchu, należy zatrzymać zbieranie danych, dopóki użytkownik wznawia ruchu i odebraniu zdarzenia wznawia ruchu. Aplikację aplikacji nie powinna wstrzymania sesji ćwiczeń w odpowiedzi na zdarzenie ruchu wstrzymana.

> [!IMPORTANT]
> Zdarzenia wstrzymana ruchu i Wznów ruchu są obsługiwane tylko dla działania typu RunningWorkout (`HKWorkoutActivityType.Running`).

Ponownie, zdarzenia te mogą być obsługiwane przez zastąpienie `DidGenerateEvent` metody `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    }
}

```

## <a name="ending-and-saving-the-workout-session"></a>Zakończenie i zapisywanie sesji ćwiczeń

Po ukończeniu ich ćwiczeń użytkownika aplikacji należy zakończyć bieżącej sesji ćwiczeń i zapisać go w bazie danych HealthKit. Zapisano HealthKit ćwiczeń automatycznie pojawi się na liście działanie ćwiczeń.

Nowość w systemach iOS 10, w tym na liście Lista działania ćwiczeń również iPhone użytkownika. Dlatego nawet jeśli dzielącą Apple Watch ćwiczeń zostanie wyświetlone na klawiaturze telefonu.

Ćwiczeń, które obejmują przykłady energii zaktualizuje pierścień przenieść użytkownika w aplikacji działań, 3 aplikacje firm teraz może przyczynić się do codziennego cele Przenieś użytkownika.

Poniższe kroki są wymagane do zakończenia i zapisać sesję ćwiczeń:

[![](workout-apps-images/workout05.png "Zakończenie i zapisać Diagram sesji ćwiczeń")](workout-apps-images/workout05.png#lightbox)

1. Po pierwsze aplikacji należy zakończyć sesję ćwiczeń.
2. Sesja ćwiczeń jest zapisywany HealthKit.
3. Dodaj wszystkie próbki (na przykład energii kiedyś lub odległość) do zapisanej sesji ćwiczeń.

### <a name="ending-the-session"></a>Kończenie sesji

Aby zakończyć sesję ćwiczeń, należy wywołać `EndWorkoutSession` metody `HKHealthStore` przekazując `HKWorkoutSession`:

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
...

public void EndOutdoorRun ()
{
    // End the current workout session
    HealthStore.EndWorkoutSession (WorkoutSession);
}
```

Spowoduje to zresetowanie czujników urządzeń do ich w trybie normalnym. Po zakończeniu HealthKit końcowy ćwiczeń otrzymają wywołania zwrotnego do `DidChangeToState` metody `HKWorkoutSessionDelegate`:

```csharp
public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
{
    // Take action based on the change in state
    switch (toState) {
    ...
    case HKWorkoutSessionState.Ended:
        StopObservingHealthKitSamples ();
        RaiseEnded ();
        break;
    }

}
```

### <a name="saving-the-session"></a>Zapisywanie sesji

Po jej zakończeniu sesji ćwiczeń, trzeba będzie utworzyć ćwiczeń (`HKWorkout`) i zapisz go (wraz z zdarzeń) w magazynie danych HealthKit (`HKHealthStore`):

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
public float MilesRun { get; set; }
public double ActiveEnergyBurned { get; set;}
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

private void SaveWorkoutSession ()
{
    // Build required workout quantities 
    var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
    var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

    // Create any required metadata
    var metadata = new NSMutableDictionary ();
    metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

    // Create workout
    var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                    WorkoutSession.StartDate, 
                                    NSDate.Now, 
                                    WorkoutEvents.ToArray (), 
                                    energyBurned, 
                                    distance, 
                                    metadata);

    // Save to HealthKit
    HealthStore.SaveObject (workout, (successful, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (successful) {

            }
        } else {
            // Report error
        }
    });

}
```

Ten kod tworzy wymagają łączną ilość energii kiedyś oraz odległość ćwiczeń jako `HKQuantity` obiektów. Słownik metadanych Definiowanie ćwiczeń jest tworzony i określono lokalizację ćwiczeń:

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

Nowy `HKWorkout` obiekt jest tworzony z takimi samymi `HKWorkoutActivityType` jako `HKWorkoutSession`, daty początkową i końcową, listę zdarzeń (jest sumą powyżej), energii kiedyś, łączna liczba odległości i słownika metadanych. Ten obiekt jest zapisany w magazynie kondycji i obsługi błędów.  

### <a name="adding-samples"></a>Dodawanie próbek

Gdy aplikacja zapisuje zestaw próbek do ćwiczeń, HealthKit generuje połączenie między przykłady i ćwiczeń, sam, dzięki czemu aplikacja może wysyłać zapytania HealthKit w późniejszym czasie dla wszystkich przykładów skojarzone z danym ćwiczeń. Korzystając z tych informacji, aplikacja może Generowanie wykresów na podstawie danych ćwiczeń i ich kreślenia na osi czasu ćwiczeń.

Aplikacja może przyczynić się do aplikacji działania Przenieś pierścień musi on zawierać przykłady energii z zapisanym ćwiczeń. Ponadto sumy dla odległości i energii musi być zgodna sumę wszystkie przykłady, które kojarzy aplikację z zapisanym ćwiczeń.

Aby dodać próbek do zapisanego ćwiczeń, wykonaj następujące czynności:

```csharp
using System.Collections.Generic;
using WatchKit;
using HealthKit;
...

public HKHealthStore HealthStore { get; private set; }
public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
...


private void SaveWorkoutSamples (HKWorkout workout)
{
    // Add samples to saved workout
    HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (success) {

            }
        } else {
            // Report error
        }
    });
}
```

Opcjonalnie można obliczyć aplikacji i utworzyć mniejszego podzestawu próbek lub jeden przykład mega (obejmujące cały zakres ćwiczeń), który następnie pobiera skojarzony z zapisanym ćwiczeń.

## <a name="workouts-and-ios-10"></a>Ćwiczeń i iOS 10

Każda aplikacja ćwiczeń watchOS 3 ma nadrzędny iOS 10 ćwiczeń oparte na aplikacji i nowych do 10, iOS tej aplikacji dla systemu iOS może służyć do uruchomienia ćwiczeń, umieść Apple Watch w trybie ćwiczeń (bez interwencji użytkownika) i uruchom aplikację watchOS w trybie uruchamiania tła (zobacz [informacje uruchomionych tła](#About-Background-Running) powyżej więcej szczegółów).

Po uruchomieniu aplikacji watchOS służy WatchConnectivity dla obsługi wiadomości i komunikacji z aplikacją dla systemu iOS nadrzędnej.

Spójrz na działanie tego procesu:

[![](workout-apps-images/workout06.png "urządzenia iPhone i Apple Watch komunikacji diagramu")](workout-apps-images/workout06.png#lightbox)

1. Tworzy aplikację telefonów iPhone `HKWorkoutConfiguration` obiektu i ustawia ćwiczeń typu i lokalizacji.
2. `HKWorkoutConfiguration` Wysłania obiektu wersji Apple Watch aplikacji i, jeśli nie jest już uruchomiona, jest uruchomiona przez system.
3. W konfiguracji ćwiczeń przy użyciu przekazanych, nowej sesji ćwiczeń uruchomieniu aplikacji watchOS 3 (`HKWorkoutSession`).

> [!IMPORTANT]
> Aby uruchomić ćwiczeń na Apple Watch aplikacji iPhone nadrzędnego aplikacja watchOS 3 musi mieć uruchomiony tła włączone. Zobacz [włączenie uruchamiania tła](#Enabling-Background-Running) powyżej więcej szczegółów.

Ten proces jest bardzo podobny do procesu uruchamiania sesji ćwiczeń bezpośrednio w aplikacji watchOS 3. Na telefonie iPhone należy użyć poniższego kodu:

```csharp
using System;
using HealthKit;
using WatchConnectivity;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
#endregion
...

private void StartOutdoorRun ()
{
    // Can the app communicate with the watchOS version of the app?
    if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
        // Create a workout configuration
        var configuration = new HKWorkoutConfiguration () {
            ActivityType = HKWorkoutActivityType.Running,
            LocationType = HKWorkoutSessionLocationType.Outdoor
        };

        // Start watch app
        HealthStore.StartWatchApp (configuration, (success, error) => {
            // Handle any errors
            if (error == null) {
                // Was the save successful
                if (success) {
                    ...
                }
            } else {
                // Report error
                ...
            }
        });
    }
}
```

Ten kod gwarantuje, że jest zainstalowana wersja watchOS aplikacji i wersji iPhone nawiązać z nim najpierw:

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

A następnie tworzy `HKWorkoutConfiguration` normalnie i używa `StartWatchApp` metody `HKHealthStore` przesyłają Apple Watch i uruchom aplikację i sesji ćwiczeń.

I w aplikacji czujki systemu operacyjnego, należy użyć poniższego kodu w `WKExtensionDelegate`:

```csharp
using WatchKit;
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...


public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
{
    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Trwa `HKWorkoutConfiguration` i tworzy nowy `HKWorkoutSession` i dołącza wystąpienia niestandardowego `HKWorkoutSessionDelegate`. Sesja ćwiczeń jest uruchomiona z magazynu kondycji HealthKit użytkownika.

## <a name="bringing-all-the-pieces-together"></a>Łącząc wszystkie elementy

Biorąc, wszystkie informacje przedstawione w tym dokumencie, aplikacji na podstawie ćwiczeń watchOS 3 i jego nadrzędnego iOS 10 ćwiczeń oparte na aplikacji może obejmować następujące elementy:

1. **iOS 10 `ViewController.cs`**  -obsługuje uruchamianie sesji czujki łączności i ćwiczeń na Apple Watch.
2. **watchOS 3 `ExtensionDelegate.cs`**  -obsługuje wersję watchOS 3 aplikacji ćwiczeń.
3. **watchOS 3 `OutdoorRunDelegate.cs`**  -niestandardowego `HKWorkoutSessionDelegate` do obsługi zdarzeń dla ćwiczeń.

> [!IMPORTANT]
> Kodu pokazano w poniższych sekcjach zawiera tylko elementy wymagane do wdrożenia nowego, ulepszone funkcje oferowane ćwiczeń w aplikacjach w watchOS 3. Cały kod obsługi i kod, aby przedstawić i zaktualizuj interfejs użytkownika nie jest dołączana, ale można łatwo tworzyć wykonując naszej dokumentacji watchOS.<p/>



### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController.cs` Plik w wersji dla systemu iOS 10 nadrzędnej aplikacji ćwiczeń będzie zawierać następujący kod:

```csharp
using System;
using HealthKit;
using UIKit;
using WatchConnectivity;

namespace MonkeyWorkout
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
        public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Private Methods
        private void InitializeWatchConnectivity ()
        {
            // Is Watch Connectivity supported?
            if (!WCSession.IsSupported) {
                // No, abort
                return;
            }

            // Is the session already active?
            if (ConnectivitySession.ActivationState != WCSessionActivationState.Activated) {
                // No, start session
                ConnectivitySession.ActivateSession ();
            }
        }

        private void StartOutdoorRun ()
        {
            // Can the app communicate with the watchOS version of the app?
            if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
                // Create a workout configuration
                var configuration = new HKWorkoutConfiguration () {
                    ActivityType = HKWorkoutActivityType.Running,
                    LocationType = HKWorkoutSessionLocationType.Outdoor
                };

                // Start watch app
                HealthStore.StartWatchApp (configuration, (success, error) => {
                    // Handle any errors
                    if (error == null) {
                        // Was the save successful
                        if (success) {
                            ...
                        }
                    } else {
                        // Report error
                        ...
                    }
                });
            }
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Start Watch Connectivity
            InitializeWatchConnectivity ();
        }
        #endregion
    }
}
```

### <a name="extensiondelegatecs"></a>ExtensionDelegate.cs

`ExtensionDelegate.cs` Plik w wersji watchOS 3 aplikacji ćwiczeń będzie zawierać następujący kod:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
        public OutdoorRunDelegate RunDelegate { get; set; }
        #endregion

        #region Constructors
        public ExtensionDelegate ()
        {
            
        }
        #endregion

        #region Private Methods
        private void StartWorkoutSession (HKWorkoutConfiguration workoutConfiguration)
        {
            // Create workout session
            // Start workout session
            NSError error = null;
            var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

            // Successful?
            if (error != null) {
                // Report error to user and return
                return;
            }

            // Create workout session delegate and wire-up events
            RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

            RunDelegate.Failed += () => {
                // Handle the session failing
                ...
            };

            RunDelegate.Paused += () => {
                // Handle the session being paused
                ...
            };

            RunDelegate.Running += () => {
                // Handle the session running
                ...
            };

            RunDelegate.Ended += () => {
                // Handle the session ending
                ...
            };
            
            RunDelegate.ReachedMileGoal += (miles) => {
                // Handle the reaching a session goal
                ...
            };

            RunDelegate.HealthKitSamplesUpdated += () => {
                // Update UI as required
                ...
            };

            // Start session
            HealthStore.StartWorkoutSession (workoutSession);
        }

        private void StartOutdoorRun ()
        {
            // Create a workout configuration
            var workoutConfiguration = new HKWorkoutConfiguration () {
                ActivityType = HKWorkoutActivityType.Running,
                LocationType = HKWorkoutSessionLocationType.Outdoor
            };

            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion

        #region Override Methods
        public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
        {
            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion
    }
}
```

### <a name="outdoorrundelegatecs"></a>OutdoorRunDelegate.cs

`OutdoorRunDelegate.cs` Plik w wersji watchOS 3 aplikacji ćwiczeń będzie zawierać następujący kod:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Private Variables
        private HKAnchoredObjectQuery QueryActiveEnergyBurned;
        #endregion

        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        public float MilesRun { get; set; }
        public double ActiveEnergyBurned { get; set;}
        public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
        public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;

        }
        #endregion

        #region Private Methods
        private void ObserveHealthKitSamples ()
        {
            // Get the starting date of the required samples
            var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

            // Get data from the local device
            var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
            var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

            // Assemble compound predicate
            var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

            // Get ActiveEnergyBurned
            QueryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
                // Valid?
                if (error == null) {
                    // Yes, process all returned samples
                    foreach (HKSample sample in addedObjects) {
                        // Accumulate totals
                        var quantitySample = sample as HKQuantitySample;
                        ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);

                        // Save samples
                        WorkoutSamples.Add (sample);
                    }

                    // Inform caller
                    RaiseHealthKitSamplesUpdated ();
                }
            });

            // Start Query
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
                                                  
        }

        private void StopObservingHealthKitSamples ()
        {
            // Stop query
            HealthStore.StopQuery (QueryActiveEnergyBurned);
        }

        private void ResumeObservingHealthkitSamples ()
        {
            // Resume current queries 
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
        }

        private void NotifyUserOfReachedMileGoal (float miles)
        {
            // Play haptic feedback
            WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);

            // Raise event
            RaiseReachedMileGoal (miles);
        }

        private void SaveWorkoutSession ()
        {
            // Build required workout quantities
            var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
            var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

            // Create any required metadata
            var metadata = new NSMutableDictionary ();
            metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

            // Create workout
            var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                            WorkoutSession.StartDate, 
                                            NSDate.Now, 
                                            WorkoutEvents.ToArray (), 
                                            energyBurned, 
                                            distance, 
                                            metadata);

            // Save to HealthKit
            HealthStore.SaveObject (workout, (successful, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (successful) {
                        // Add samples to workout
                        SaveWorkoutSamples (workout);
                    }
                } else {
                    // Report error
                    ...
                }
            });

        }

        private void SaveWorkoutSamples (HKWorkout workout)
        {
            // Add samples to saved workout
            HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (success) {
                        ...
                    }
                } else {
                    // Report error
                    ...
                }
            });
        }
        #endregion

        #region Public Methods
        public void PauseWorkout ()
        {
            // Pause the current workout
            HealthStore.PauseWorkoutSession (WorkoutSession);
        }

        public void ResumeWorkout ()
        {
            // Pause the current workout
            HealthStore.ResumeWorkoutSession (WorkoutSession);
        }

        public void ReachedNextMile ()
        {
            // Create and save marker event
            var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
            WorkoutEvents.Add (markerEvent);

            // Notify user
            NotifyUserOfReachedMileGoal (++MilesRun);
        }

        public void EndOutdoorRun ()
        {
            // End the current workout session
            HealthStore.EndWorkoutSession (WorkoutSession);
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                StopObservingHealthKitSamples ();
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                if (fromState == HKWorkoutSessionState.Paused) {
                    ResumeObservingHealthkitSamples ();
                } else {
                    ObserveHealthKitSamples ();
                }
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                StopObservingHealthKitSamples ();
                SaveWorkoutSession ();
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);

            // Save HealthKit generated event
            WorkoutEvents.Add (@event);

            // Take action based on the type of event
            switch (@event.Type) {
            case HKWorkoutEventType.Lap:
                ...
                break;
            case HKWorkoutEventType.Marker:
                ...
                break;
            case HKWorkoutEventType.MotionPaused:
                ...
                break;
            case HKWorkoutEventType.MotionResumed:
                ...
                break;
            case HKWorkoutEventType.Pause:
                ...
                break;
            case HKWorkoutEventType.Resume:
                ...
                break;
            }
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();
        public delegate void OutdoorRunMileGoalDelegate (float miles);

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }

        public event OutdoorRunMileGoalDelegate ReachedMileGoal;
        internal void RaiseReachedMileGoal (float miles)
        {
            if (this.ReachedMileGoal != null) this.ReachedMileGoal (miles);
        }

        public event OutdoorRunEventDelegate HealthKitSamplesUpdated;
        internal void RaiseHealthKitSamplesUpdated ()
        {
            if (this.HealthKitSamplesUpdated != null) this.HealthKitSamplesUpdated ();
        }
        #endregion
    }
}
```

## <a name="best-practices"></a>Najlepsze praktyki

Apple sugeruje przy użyciu następujących najlepszych rozwiązań podczas projektowania i wdrażania aplikacji ćwiczeń w watchOS 3 i iOS 10:

- Upewnij się, że aplikacja ćwiczeń watchOS 3 nadal działa nawet wtedy, gdy nie można nawiązać połączenia z telefonów iPhone i wersji systemu iOS 10 aplikacji.
- Za pomocą odległość HealthKit GPS jest niedostępna, ponieważ jest w stanie wygenerować przykłady odległość bez GPS.
- Zezwalaj użytkownikowi na uruchamianie ćwiczeń z Apple Watch lub telefonów iPhone.
- Zezwalaj aplikacji na wyświetlanie ćwiczeń z innych źródeł (np. aplikacje firm innych 3) w widokach danych historycznych.
- Upewnij się, że aplikacja nie nie usunął wyświetlania ćwiczeń w danych historycznych.

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego ulepszenia Apple wprowadził do aplikacji ćwiczeń w watchOS 3 oraz sposób ich wdrażania w programie Xamarin.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Wprowadzenie do HealthKit](~/ios/platform/healthkit.md)
