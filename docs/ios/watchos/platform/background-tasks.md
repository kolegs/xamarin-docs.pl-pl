---
title: watchOS zadania w tle w programie Xamarin
description: Ten dokument zawiera opis sposobu zadania w tle za pomocą watchOS w Xamarin, biorąc przyjrzeć się typów zadań tła, korzystając z zasobów, wykonania zadania w tle, planowania, najlepsze rozwiązania i inne.
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/13/2017
ms.openlocfilehash: 5ab53d4aea32cf41c492e286c18cbe85a619889a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792050"
---
# <a name="watchos-background-tasks-in-xamarin"></a>watchOS zadania w tle w programie Xamarin

WatchOS 3 istnieją trzy sposoby czy aplikacji czujki można zachować informacje aktualne: 

- Przy użyciu jednej z kilku nowych zadań w tle. 
- O jeden z jego komplikacji na powierzchni czujki (nadanie bardzo czasu aktualizacji). 
- Konieczności pin użytkownika do aplikacji nowe doku (gdzie jego przechowywany w pamięci i zaktualizować często). 

## <a name="keeping-an-app-up-to-date"></a>Aktualizowanie aplikacji

Przed omówieniem wszystkich sposobów deweloper może przechowywać dane i interfejs użytkownika aplikacji watchOS bieżące i zaktualizowane, w tej sekcji zostanie zapoznaj się z typowym zestawem wzorce użycia i jak użytkownik może przechodzić między ich iPhone i ich Apple Watch w ciągu dnia na podstawie  Godzina i działania aktualnie robią (na przykład zwiększają).

Wykonaj poniższy przykład:

[![](background-tasks-images/update00.png "Jak użytkownik może przechodzić między ich iPhone i ich Apple Watch w ciągu dnia")](background-tasks-images/update00.png#lightbox)

1. W nocy, podczas oczekiwania w kolejce kawy użytkownik będzie przeglądać bieżącej wiadomości w ich iPhone kilka minut.
2. Przed opuszczeniem kawiarni, szybko sprawdzić pogodzie Complication na ich powierzchni czujki.
3. Przed obiad używają aplikacji mapy na telefonie iPhone można znaleźć pobliskich restauracjach i książki zastrzeżenie ma spełnia klienta.
4. Podczas podróży do restauracji, otrzymują powiadomienie na ich Apple Watch lub szybkiego dostępu, które znają ich terminu obiad działa późne.
5. Wieczorem, używają aplikacji mapy na telefonie iPhone sprawdza ruch przed zwiększają home.
6. W ten sposób głównej, otrzymają powiadomienie iMessage na ich Apple Watch, prosząc ich do pobrania niektórych mleka i używają funkcji szybkie odpowiadanie na wysłanie odpowiedzi "OK".

Z powodu "szybkiego dostępu" (mniej niż trzy sekundy) rodzaj jak użytkownik jest chcą skorzystać aplikację Apple Watch zwykle nie jest wystarczająco dużo czasu na potrzeby aplikacji, aby pobrać odpowiednie informacje i zaktualizuj jego interfejsie użytkownika przed wyświetleniem go użytkownikowi.

Przy użyciu nowych interfejsów API firmy Apple była dostępna w watchOS 3 aplikacji można zaplanować na _odświeżania w tle_ i mieć odpowiednie informacje gotowe, aby na żądanie użytkownika. Wykonaj przykład Complication pogody opisanych wyżej:

[![](background-tasks-images/update01.png "Przykład Complication pogody")](background-tasks-images/update01.png#lightbox)

1. Harmonogramy aplikacji do wznawiane przez system w określonym czasie. 
2. Aplikacja pobiera informacje, że trzeba będzie generować aktualizacji.
3. Aplikacja generuje ponownie interfejs użytkownika w celu uwzględnienia nowych danych.
4. Gdy użytkownik glances na Complication aplikacji, ma aktualne informacje bez potrzeby czekania na aktualizację użytkownika.

Jak pokazano powyżej, watchOS system działania aplikacji przy użyciu jednego lub więcej zadań, które ma bardzo ograniczony puli dostępne:

[![](background-tasks-images/update02.png "WatchOS system działania aplikacji przy użyciu jednego lub więcej zadań")](background-tasks-images/update02.png#lightbox)

Apple sugeruje efektywne korzystanie z tego zadania (ponieważ jest to ograniczona zasobów do aplikacji), przytrzymując na niego, do momentu zakończenia procesu aktualizacji samej aplikacji.

System oferuje te zadania przez wywołanie metody nowy `HandleBackgroundTasks` metody `WKExtensionDelegate` delegowanie. Na przykład:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Constructors
        public ExtensionDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request here
            ...
        }
        #endregion
    }
}
```

Gdy aplikacja zakończy działanie danego zadania, zwraca go do systemu przez oznaczenie go wykonać:

[![](background-tasks-images/update03.png "Zadanie zwraca systemu przez oznaczenie jej ukończone")](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>Nowego zadania w tle

watchOS 3 wprowadzono kilka zadań w tle, które aplikacji można użyć do zaktualizowania jej informacje o sprawdzeniu, czy ma ona zawartość użytkownik powinien przed otwarciem aplikacji, takich jak:

- **Aplikacja odświeżania w tle** - [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) zadanie pozwala aplikacji na aktualizowanie stanu w tle. Zwykle będzie to obejmowało innego zadania, takie jak pobieranie nowej zawartości z Internetu za pomocą [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Migawki odświeżania w tle** - [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) zadanie pozwala aplikacji na aktualizowanie jej zawartości i interfejsu użytkownika, zanim system tworzy migawkę, który będzie używany do wypełniania doku.
- **Obejrzyj łączność w tle** - [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) zadanie zostało uruchomione dla aplikacji, gdy odbiera dane w tle z sparowanego iPhone.
- **Adres URL sesja w tle** - [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) zadanie zostało uruchomione dla aplikacji podczas transferu w tle wymaga autoryzacji lub zakończeniu (pomyślnie lub błędów).

Te zadania zostanie omówiona szczegółowo w poniższych sekcjach.

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

`WKApplicationRefreshBackgroundTask` To ogólny zadanie, które mogą być planowane aplikacjami wybudzane w przyszłości:

[![](background-tasks-images/update04.png "WKApplicationRefreshBackgroundTask wybudzane w przyszłości")](background-tasks-images/update04.png#lightbox)

W czasie wykonywania zadania aplikacji można wykonać dowolny rodzaj przetwarzania lokalnego, takich jak aktualizacja osi czasu Complication lub pobrać niektórych wymaganych danych z `NSUrlSession`.


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

System będzie wysyłać `WKURLSessionRefreshBackgroundTask` kiedy dane Zakończono pobieranie i gotowe do przetworzenia przez aplikację:

[![](background-tasks-images/update05.png "WKURLSessionRefreshBackgroundTask, gdy dane zakończył pobieranie")](background-tasks-images/update05.png#lightbox)

Aplikacja nie pozostaje uruchomiona podczas pobierania danych w tle. Zamiast tego aplikacja planuje żądanie dotyczące danych, a następnie jest wstrzymana i system obsługuje pobieranie danych, tylko reawakening aplikacji po ukończeniu pobierania.

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

W watchOS 3 Apple został dodany dokowania, w którym użytkownicy mogą przypiąć ich ulubionych aplikacji i szybko uzyskiwać do nich dostęp. Gdy użytkownik naciśnie przycisk po stronie na Apple Watch, pojawi się galerii migawek przypiętych aplikacji. Użytkownik może szybko przesuń od lewej lub prawej strony, aby znaleźć odpowiednią aplikację, a następnie wybierz aplikację, aby uruchomić go zamianę migawki interfejsu uruchomionej aplikacji.

[![](background-tasks-images/update06.png "Zastępowanie migawki interfejsie uruchomionej aplikacji")](background-tasks-images/update06.png#lightbox)

System ma okresowo migawki w Interfejsie użytkownika aplikacji (wysyłając `WKSnapshotRefreshBackgroundTask`) i użyje tych migawek, aby wypełnić doku. watchOS daje aplikacji możliwość jego zawartość i interfejsu użytkownika aktualizacji przed podjęciem tej migawki.

Migawki są bardzo ważne w watchOS 3, ponieważ działają jako obrazy zarówno w wersji zapoznawczej i uruchamiania aplikacji. Jeśli użytkownik reguluje aplikacji w doku, utworzy Rozwiń do pełnego ekranu, wprowadź pierwszego planu i uruchomione, więc jest konieczne, że migawki być aktualne:

[![](background-tasks-images/update07.png "Jeśli użytkownik reguluje aplikacji w doku, rozwinie do pełnego ekranu")](background-tasks-images/update07.png#lightbox)

Ponownie, system będzie wystawiać `WKSnapshotRefreshBackgroundTask` tak, aby przygotować aplikację (aktualizując dane i interfejsu użytkownika) przed migawki:

[![](background-tasks-images/update08.png "Aplikację można przygotować przez zaktualizowanie danych i Interfejsie użytkownika przed migawki")](background-tasks-images/update08.png#lightbox)

Gdy aplikacja oznacza `WKSnapshotRefreshBackgroundTask` ukończone, system automatycznie podejmie migawkę Interfejsie użytkownika aplikacji.

> [!IMPORTANT]
> Ważne jest, aby zawsze zaplanować ` WKSnapshotRefreshBackgroundTask` po otrzymał nowe dane i zaktualizować interfejs użytkownika aplikacji lub modyfikacji informacje nie będą wyświetlane przez użytkownika.




Ponadto gdy użytkownik otrzyma powiadomienie z aplikacji i naciska go do przełączenia aplikacji na pierwszym planie, migawki musi być aktualne, ponieważ działa jako ekran startowy również:

[![](background-tasks-images/update09.png "Użytkownik otrzyma powiadomienie z aplikacji i naciska go do przełączenia aplikacji na pierwszym planie.")](background-tasks-images/update09.png#lightbox)

Ponieważ użytkownik ma interakcji z aplikacją watchOS została już więcej niż jedną godzinę, będzie można przywrócić do stanu domyślnego. Domyślny stan może mieć różne znaczenie do różnych aplikacji i oparte na projekt aplikacji, może nie mieć domyślny stan w ogóle.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

W watchOS 3 Apple ma zintegrowany czujki łączności z tła Odśwież API za pomocą nowej `WKWatchConnectivityRefreshBackgroundTask`. Przy użyciu tej nowej funkcji, aplikację telefonów iPhone można dostarczać nowe dane z jego odpowiednikiem aplikacji czujki uruchomionej aplikacji watchOS w tle:

[![](background-tasks-images/update10.png "Aplikację telefonów iPhone mogą dostarczać nowe dane do partnera czujki aplikacji uruchomionej aplikacji watchOS w tle")](background-tasks-images/update10.png#lightbox)

Inicjowanie Complication Push, kontekst aplikacji, wysłanie pliku lub aktualizowanie informacji o użytkowniku z aplikacji iPhone spowoduje wznowienie pracy aplikacji Apple Watch w tle.

Gdy aplikacja czujki jest wybudzany za pośrednictwem `WKWatchConnectivityRefreshBackgroundTask` trzeba będzie użyć standardowych metod interfejsu API do odbierania danych z aplikacji iPhone.

[![](background-tasks-images/update11.png "Przepływ danych WKWatchConnectivityRefreshBackgroundTask")](background-tasks-images/update11.png#lightbox)

1. Upewnij się, że sesja została uaktywniona.
2. Monitorowanie nowej `HasContentPending` tak długo, jak jest wartość właściwości `true`, aplikacja nadal zawiera dane do przetwarzania. Jak przedtem aplikacji powinno zawierać na zadania przed zakończeniem przetwarzania wszystkich danych.
3. Jeśli nie ma żadnych więcej danych na przetworzenie (`HasContentPending = false`), Oznacz zadania, aby przywrócić go do systemu. Można to zrobić wyczerpie spowodować, że raport o awarii wyznaczonym tło aplikacji w czasie wykonywania.

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>Cykl życia interfejsu API tła

Umieszczenie ze sobą wszystkie elementy nowy interfejs API zadania tła, typowy zestaw interakcje będzie wyglądać następujące czynności:

[![](background-tasks-images/update12.png "Cykl życia interfejsu API tła")](background-tasks-images/update12.png#lightbox)

1. Najpierw aplikacji watchOS planuje tle zadanie do uaktywnienie jako pewnym momencie w przyszłości.
2. Aplikacja jest wybudzany przez system i wysłane zadanie.
3. Aplikacja przetwarza zadanie, aby zakończyć pracę była wymagana.
4. W związku z tym przetwarzania zadania, aplikacja może być konieczne zaplanowanie więcej tła na ukończenie pracy w przyszłości, takie jak pobieranie więcej zawartości przy użyciu zadań `NSUrlSession`.
5. Aplikacja oznacza zadanie zostało zakończone i zwraca go do systemu.

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>Odpowiedzialne za pomocą zasobów

Bardzo ważne, czy aplikacja watchOS zachowuje się odpowiedzialne w ekosystemie to poprzez ograniczenie jego zużycie zasobów udostępnionych systemu jest.

Spójrz na następujący scenariusz:

[![](background-tasks-images/update13.png "Aplikacja watchOS ogranicza jego zużycie zasobów udostępnionych systemu")](background-tasks-images/update13.png#lightbox)

1. Użytkownik uruchamia aplikację watchOS na 1:00 PM.
2. Aplikacja planuje zadania wznawiania i Pobierz nową zawartość w ciągu godziny przy 2:00 PM.
3. Godzinie 1:50 użytkownik otwiera ponownie aplikacji, dzięki czemu w celu zaktualizowania interfejsu użytkownika i danych w tej chwili.
4. Zamiast czekać na wznawiania zadań aplikację ponownie za 10 minut, aplikacji należy zaplanować zadanie do uruchomienia na godzinę później godzinie 2:50.

Gdy każda aplikacja jest inny, Apple sugeruje wyszukuj wzorce użycia, takich jak te wyświetlane powyżej, aby oszczędzać zasoby systemowe.

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>Implementowanie zadań w tle

Ze względu na przykład tego dokumentu użyje fałszywych aplikacji MonkeySoccer sportowych raporty nożną wyniki do użytkownika. 

Spójrz na poniższy scenariusz, w typowy sposób użycia:

[![](background-tasks-images/update14.png "Scenariusz typowy sposób")](background-tasks-images/update14.png#lightbox)

Użytkownika nożną ulubionych zespołu odtwarzania big dopasowania z 7:00 PM do 9:00, aplikacja oczekiwać użytkownikowi należy regularnie sprawdzanie wynik i zdecyduje się na 30 minut aktualizacji.

1. Użytkownik otwiera aplikację i harmonogramy zadań tła aktualizacji 30 minutach. Interfejs API tła umożliwia tylko jeden typ tła zadań działa w danym momencie.
2. Aplikacja odbiera zadania i aktualizuje jego danych i interfejsu użytkownika, a następnie harmonogramy dla innej w tle zadanie 30 minutach. Należy pamiętać, że deweloper pamięta można zaplanować tła innego zadania lub aplikacji nigdy nie będzie można ponownie obudzony można pobrać dodatkowe aktualizacje.
3. Ponownie aplikacja odbiera zadania i aktualizuje jego dane, jego interfejsie użytkownika aktualizacji i planuje tła innego zadania 30 minutach.
4. Ten sam proces jest powtarzany ponownie.
5. Tło ostatniego odebrania zadań i aplikację ponownie aktualizacje interfejsu użytkownika i danych. Ponieważ jest to końcowy wynik nie zaplanować dla nowego odświeżania w tle. 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>Planowanie aktualizacji w tle

Podana w powyższym scenariuszu, MonkeySoccer aplikacji można użyć poniższego kodu do zaplanowania aktualizacji tła:

```csharp
private void ScheduleNextBackgroundUpdate ()
{
    // Create a fire date 30 minutes into the future
    var fireDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

    // Create 
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("LastActiveDate"), NSDate.FromTimeIntervalSinceNow(0));
    userInfo.Add (new NSString ("Reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleBackgroundRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Tworzy nowy `NSDate` 30 minut w przyszłości gdy chce się obudzony aplikacji i tworzy `NSMutableDictionary` do przechowywania danych żądanego zadania. `ScheduleBackgroundRefresh` Metoda `SharedExtension` służy do żądania można zaplanować zadanie.

System zwróci `NSError` Jeśli nie można było zaplanować żądanego zadania.

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>Przetwarzanie aktualizacji

Następnie należy przełączyć bliższe spojrzenie na okna 5 minut, przedstawiający na krokach wymaganych do aktualizacji wynik:

[![](background-tasks-images/update15.png "Okno 5 minut, przedstawiający na krokach wymaganych do aktualizacji wynik")](background-tasks-images/update15.png#lightbox)

1. W 19:30:02: 00 aplikacji jest wznowione przez system i danych zadania w tle aktualizacji. Pobierz najnowsze wyniki z serwera jest jej priorytet. Zobacz [planowania NSUrlSession](#Scheduling-a-NSUrlSession) poniżej.
2. W 7:30:05 aplikacji zakończeniu zadania oryginalnego, system umieszcza aplikacji w tryb uśpienia i kontynuuje można pobrać żądanych danych w tle.
3. Po ukończeniu pobierania system tworzy nowe zadanie do wznawiania pracy aplikacji, aby przetworzyć pobrane informacje. Zobacz [obsługi zadania w tle](#Handling-Background-Tasks) i [obsługi kończenie pobierania](#Handling-the-Download-Completing) poniżej. 
4. Aplikacja zapisuje zaktualizowane informacje i oznacza zadanie zostało zakończone. Deweloper może być wydawać się zaktualizuj interfejs użytkownika aplikacji w tej chwili, jednak Apple sugeruje planowania zadania migawki do obsługi tego procesu. Zobacz [planowania aktualizacji migawki](#Scheduling-a-Snapshot-Update) poniżej.
5. Aplikacja odbiera zadania migawki, aktualizuje interfejs użytkownika i oznacza zadanie zostało zakończone. Zobacz [obsługi aktualizacja migawki](#Handling-a-Snapshot-Update) poniżej.

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>Planowanie NSUrlSession

Poniższy kod może służyć do planowania pobieranie najnowszych wyników:

```csharp
private void ScheduleURLUpdateSession ()
{
    // Create new configuration
    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.example.urlsession");

    // Create new session
    var backgroundSession = NSUrlSession.FromConfiguration (configuration);

    // Create and start download task
    var downloadTask = backgroundSession.CreateDownloadTask (new NSUrl ("https://example.com/gamexxx/currentScores.json"));
    downloadTask.Resume ();
}
```

Konfiguruje i tworzy nowy `NSUrlSession`, następnie używa tej sesji do utworzenia nowego pobierania zadań przy użyciu `CreateDownloadTask` metody. Wywołuje `Resume` metody pobierania zadania, aby rozpocząć sesję.

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>Obsługa zadania w tle

Przez zastąpienie `HandleBackgroundTasks` metody `WKExtensionDelegate`, aplikacja może obsłużyć przychodzące zadania w tle:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
        #endregion

        ...
        
        #region Public Methods
        public void CompleteTask (WKRefreshBackgroundTask task)
        {
            // Mark the task completed and remove from the collection
            task.SetTaskCompleted ();
            PendingTasks.Remove (task);
        }
        #endregion 

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request
            foreach (WKRefreshBackgroundTask task in backgroundTasks) {
                // Is this a background session task?
                var urlTask = task as WKUrlSessionRefreshBackgroundTask;
                if (urlTask != null) {
                    // Create new configuration
                    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

                    // Create new session
                    var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

                    // Keep track of all pending tasks
                    PendingTasks.Add (task);
                } else {
                    // Ensure that all tasks are completed
                    task.SetTaskCompleted ();
                }
            }
        }
        #endregion
        
        ...
    }
}
```

`HandleBackgroundTasks` Metody przełączanie po kolei wszystkich zadań, że system wysłał aplikacji (w `backgroundTasks`) wyszukiwanie `WKUrlSessionRefreshBackgroundTask`. Jeśli został znaleziony, dołącza sesji i dołącza `NSUrlSessionDownloadDelegate` do obsługi kończenie pobierania (zobacz [obsługi Pobierz Kończenie](#Handling-the-Download-Completing) poniżej):

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

Uniemożliwia to dojście w zadaniu dopóki zostanie ukończona, dodając ją do kolekcji:

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

Wszystkie zadania wysyłane do aplikacji muszą zostać wykonane w przypadku zadań nie jest obecnie obsługiwane, oznacz go pełną:

```csharp
if (urlTask != null) {
    ...
} else {
    // Ensure that all tasks are completed
    task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>Obsługa ukończeniu pobierania

Aplikacja MonkeySoccer korzysta z następujących `NSUrlSessionDownloadDelegate` pełnomocnika, aby obsłużyć kończenie pobierania i przetworzyć żądanych danych:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class BackgroundSessionDelegate : NSUrlSessionDownloadDelegate
    {
        #region Computed Properties
        public ExtensionDelegate WatchExtensionDelegate { get; set; }

        public WKRefreshBackgroundTask Task { get; set; }
        #endregion

        #region Constructors
        public BackgroundSessionDelegate (ExtensionDelegate extensionDelegate, WKRefreshBackgroundTask task)
        {
            // Initialize
            this.WatchExtensionDelegate = extensionDelegate;
            this.Task = task;
        }
        #endregion

        #region Override Methods
        public override void DidFinishDownloading (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, NSUrl location)
        {
            // Handle the downloaded data
            ...

            // Mark the task completed
            WatchExtensionDelegate.CompleteTask (Task);

        }
        #endregion
    }
}
```

Po zainicjowaniu, przechowuje dojście do obu `ExtensionDelegate` i `WKRefreshBackgroundTask` który zduplikowany go. Zastępuje on `DidFinishDownloading` można obsłużyć ukończeniu pobierania. Następnie używa `CompleteTask` metody `ExtensionDelegate` informują o tym zadania jego zostało ukończone i usuń go z kolekcji zadań w toku. Zobacz [obsługi zadania w tle](#Handling-Background-Tasks) powyżej.

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>Planowanie aktualizacji migawki

Poniższy kod może służyć do zaplanować zadanie migawki aktualizowania interfejsu użytkownika przy użyciu najnowszych wyników:

```csharp
private void ScheduleSnapshotUpdate ()
{
    // Create a fire date of now
    var fireDate = NSDate.FromTimeIntervalSinceNow (0);

    // Create user info dictionary
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
    userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleSnapshotRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Podobnie jak `ScheduleURLUpdateSession` metod powyżej, tworzy nową `NSDate` dla gdy aplikacja chce się obudzony i tworzy `NSMutableDictionary` do przechowywania danych żądanego zadania. `ScheduleSnapshotRefresh` Metoda `SharedExtension` służy do żądania można zaplanować zadanie.

System zwróci `NSError` Jeśli nie można było zaplanować żądanego zadania.

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>Obsługa aktualizacji migawki

Aby obsłużyć zadania migawki `HandleBackgroundTasks` — metoda (zobacz [obsługi zadania w tle](#Handling-Background-Tasks) powyżej) są modyfikowane w celu wyglądać następująco:

```csharp
public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
{
    // Handle background request
    foreach (WKRefreshBackgroundTask task in backgroundTasks) {
        // Take action based on task type
        if (task is WKUrlSessionRefreshBackgroundTask) {
            var urlTask = task as WKUrlSessionRefreshBackgroundTask;

            // Create new configuration
            var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

            // Create new session
            var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

            // Keep track of all pending tasks
            PendingTasks.Add (task);
        } else if (task is WKSnapshotRefreshBackgroundTask) {
            var snapshotTask = task as WKSnapshotRefreshBackgroundTask;

            // Update UI
            ...

            // Create a expiration date 30 minutes into the future
            var expirationDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

            // Create user info dictionary
            var userInfo = new NSMutableDictionary ();
            userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
            userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

            // Mark task complete
            snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
        } else {
            // Ensure that all tasks are completed
            task.SetTaskCompleted ();
        }
    }
}
```

Metoda testów dla typu zadania przetwarzania. Jeśli jest `WKSnapshotRefreshBackgroundTask` uzyskuje on dostęp do zadania:

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

Metoda aktualizacji interfejsu użytkownika, a następnie tworzy `NSDate` mówić systemu, gdy migawka jest przestarzała. Tworzy `NSMutableDictionary` z użyciem informacji użytkownika do opisania nową migawkę i znaczniki zadania migawki z tymi informacjami:

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

Ponadto również informuje o tym zadania migawki nie powraca do stanu domyślnego (w pierwszym parametrze) aplikacji. Aplikacje, które nie znają pojęcia domyślny stan zawsze należy ustawić tę właściwość na `true`.

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>Wydajności pracy

Jak pokazano w powyższym przykładzie okna pięciu minut, w którym aplikacja MonkeySoccer trwało zaktualizować jego wyniki wydajności pracy, a następnie użyć nowego watchOS 3 zadania w tle, aplikacja była tylko aktywne dla wszystkich 15 sekund: 

[![](background-tasks-images/update16.png "Aplikacja była tylko aktywne dla wszystkich 15 sekund")](background-tasks-images/update16.png#lightbox)

Zmniejsza to wpływ, jaki aplikacja będzie mieć na dostępne zasoby Apple Watch i czas pracy baterii, a także umożliwia aplikacji lepiej współpracuje z innych aplikacji działających na czujki.

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>Jak działa planowania

Gdy aplikacja watchOS 3 działa na pierwszym planie, zawsze zostało zaplanowane do uruchomienia i można wykonać dowolnego typu przetworzenia, takich jak dane aktualizacji lub odświeżyć jego interfejsie użytkownika. Gdy aplikacja zostanie przeniesiony w tle, zazwyczaj jest zawieszony przez system i wszystkie operacje obsługi są zatrzymane. 

Gdy aplikacja działa w tle, może być obiektem przez system, aby szybko uruchomić określonego zadania. Dlatego w watchOS 2 systemu może tymczasowo wznawiania aplikacji tła, aby wykonywać następujące czynności obsługuje powiadomienie o długie wygląd lub zaktualizować Complication aplikacji. WatchOS 3 istnieje kilka nowych metod, które można uruchomić aplikację w tle.

Gdy aplikacja jest w tle, system nakłada kilka ograniczeniem:

- Go otrzymuje tylko kilka sekund do ukończenia każdego zadania. System bierze pod uwagę nie tylko czas przekazany, ale również potrzebną moc procesora CPU aplikacja zużywa pochodzić ten limit.
- Dowolną aplikację, która przekracza limit zostaną skasowane następujące kody błędów:
    - **Procesor CPU** -0xc51bad01
    - **Czas** -0xc51bad02
- System będzie nakładają różne ograniczenia na podstawie typu został poproszony aplikacji do wykonania zadania w tle. Na przykład `WKApplicationRefreshBackgroundTask` i `WKURLSessionRefreshBackgroundTask` zadania podane są nieco dłużej środowisk uruchomieniowych za pośrednictwem innych typów zadań w tle.

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>Komplikacji i aktualizacje aplikacji

Oprócz nowych zadań tła Apple został dodany do watchOS 3 to aplikacja watchOS komplikacji może mieć wpływ na sposób i czas aplikacja odbiera aktualizacje w tle.

Komplikacji są małe elementy wizualne, dostarczające przydatnych informacji na pierwszy rzut oka. W zależności od kroju czujki zaznaczone użytkownik ma możliwość dostosowania krój czujki z co najmniej jeden Complication, które mogą być dostarczane przez aplikację czujki w watchOS 3.

Jeśli użytkownik zawiera jeden z komplikacji aplikacji na ich powierzchni czujki, daje aplikacji zaktualizowano następujące korzyści:

- Ta powoduje, że system przechowywania aplikacji w gotowych do uruchamiania stanu, w którym próbuje uruchomić aplikację w tle, utrzymuje je w pamięci i zapewnia bardzo czasu aktualizacji.
- Komplikacji dotrą co najmniej 50 wypychaną aktualizacji na dzień.

Deweloper zawsze należy dążyć do tworzenia atrakcyjnych komplikacji dla swoje aplikacje do nakłonić użytkownika do dodawania ich do ich powierzchni czujki z powodów wymienionych powyżej.

W watchOS 2 komplikacji były podstawową metodą otrzymanie aplikacji środowiska wykonawczego znajduje się w tle. W watchOS 3 aplikacji Complication nadal będą dostępne do odbierania wielu aktualizacji na godzinę, jednak można użyć `WKExtensions` więcej środowiska uruchomieniowego do zaktualizowania jej komplikacji żądania.

Spójrz na następujący kod, używane do aktualizowania Complication z aplikacji połączonych iPhone:

```csharp
using System;
using WatchConnectivity;
using UIKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
...

private void UpdateComplication ()
{

    // Get session and the number of remaining transfers
    var session = WCSession.DefaultSession;
    var transfers = session.RemainingComplicationUserInfoTransfers;

    // Create user info dictionary
    var iconattrs = new Dictionary<NSString, NSObject>
        {
            {new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0)},
            {new NSString ("reason"), new NSString ("UpdateScore")}
        };

    var userInfo = NSDictionary<NSString, NSObject>.FromObjectsAndKeys (iconattrs.Values.ToArray (), iconattrs.Keys.ToArray ());

    // Take action based on the number of transfers left
    if (transfers < 1) {
        // No transfers left, either attempt to send or inform
        // user of situation.
        ...
    } else if (transfers < 11) {
        // Running low on transfers, only send on important updates
        // else conserve for a significant change.
        ...
    } else {
        // Send data
        session.TransferCurrentComplicationUserInfo (userInfo);
    }
}
```

Używa `RemainingComplicationUserInfoTransfers` właściwość `WCSession` aby zobaczyć, ile o wysokim priorytecie przesyła aplikacji opuścił dzień, a następnie pobiera akcji na podstawie tego numeru. Jeśli aplikacja zacznie brakować transferów, go blokadę przy wysyłaniu niewielkie aktualizacje i wysłać tylko informacje po znaczące zmiany.

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>Planowanie i doku

W watchOS 3 Apple został dodany dokowania, w którym użytkownicy mogą przypiąć ich ulubionych aplikacji i szybko uzyskiwać do nich dostęp. Gdy użytkownik naciśnie przycisk po stronie na Apple Watch, pojawi się galerii migawek przypiętych aplikacji. Użytkownik może szybko przesuń od lewej lub prawej strony, aby znaleźć odpowiednią aplikację, a następnie wybierz aplikację, aby uruchomić go zamianę migawki interfejsu uruchomionej aplikacji.

[![](background-tasks-images/dock01.png "Doku")](background-tasks-images/dock01.png#lightbox)

System okresowo przyjmuje migawki w Interfejsie użytkownika aplikacji i użyje tych migawek, aby wypełnić z dokumentami. watchOS daje aplikacji możliwość jego zawartość i interfejsu użytkownika aktualizacji przed podjęciem tej migawki.

Aplikacje, które została przypięta do stacji dokującej można spodziewać się poniżej:

- Otrzymają co najmniej jednej aktualizacji na godzinę. Obejmuje to zarówno zadanie Odśwież aplikację i zadania migawki.
- Budżet aktualizacji jest dystrybuowane między wszystkie aplikacje w doku. Dlatego mniej aplikacje, które użytkownik ma przypięty, więcej potencjalnych aktualizacje, które każda aplikacja będzie otrzymywać.
- Aplikacja będzie utrzymywana w pamięci, aplikacja zostanie wznowiona szybko w przypadku wybrania z doku.

Ostatni aplikacji użytkownik uruchomi będą uznawane za _ostatnio używanych_ aplikacji i zajmują ostatniego miejsca w doku. Z tego miejsca można przypiąć go trwale do stacji dokującej. Ostatnio używanych będą traktowane jak wszystkie inne ulubionych aplikacji użytkownik ma już przypięty do doku.

> [!IMPORTANT]
> Aplikacje, które zostały dodane tylko do ekranu Home nie będzie mógł skorzystać z regularnych planowania. Odbierać Planowanie regularnych i tła aktualizacji aplikacji _musi_ można dodać do stacji dokującej.

Jak wspomniano wcześniej w tym dokumencie, migawki są bardzo ważne w watchOS 3 od momentu ich działanie jako obrazy zarówno w wersji zapoznawczej i uruchamiania aplikacji. Jeśli użytkownik reguluje aplikacji w doku, utworzy Rozwiń do pełnego ekranu, wprowadź pierwszego planu i uruchomione, więc jest konieczne, że migawki są nieaktualne.

Może to być razy podczas systemu decyduje o wymaganych nową migawkę Interfejsie użytkownika aplikacji. W tej sytuacji żądania migawki nie będą uwzględniane budżetu środowiska uruchomieniowego aplikacji. Żądanie migawki system wyzwoli:

- Complication osi czasu aktualizacji.
- Posługiwanie się powiadomienie z aplikacji.
- Przełączanie z pierwszego planu do stanu tła.
- Po upływie godziny są w stanie tła więc aplikacja może zwrócić do stanu domyślnego.
- Po pierwszym uruchomieniu watchOS.

<a name="Best-Practices" />

## <a name="best-practices"></a>Najlepsze praktyki 

Podczas pracy z zadaniami w tle, Apple sugeruje następujące najlepsze rozwiązania:

- Harmonogram tyle razy, ile aplikacji musi zostać zaktualizowany. Za każdym razem, gdy aplikacja jest uruchamiana powinna ponownie ocenić jego przyszłych potrzeb i dostosować ten harmonogram, zgodnie z wymaganiami.
- Jeśli aplikacja nie wymaga aktualizacji systemu wysyła zadanie odświeżania w tle, należy odłożyć pracy do momentu rzeczywistości wymagana jest aktualizacja.
- Należy wziąć pod uwagę wszystkie możliwości środowiska uruchomieniowego dostępne dla aplikacji:
    - Aktywacja Dock i pierwszego planu.
    - Powiadomienia.
    - Complication aktualizacji.
    - Operacji odświeżania w tle.
- Użyj `ScheduleBackgroundRefresh` Runtime tła ogólnego przeznaczenia, takich jak:
    - Sondowanie systemu pod kątem informacji.
    - Zaplanuj przyszłe `NSURLSessions` na dane żądanie w tle. 
    - Znane czas przejścia.
    - Wyzwolenie Complication aktualizacji.

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>Najlepsze rozwiązania w zakresie migawki

Podczas pracy z aktualizacji migawek, Apple sprawia, że poniższe sugestie:

- Unieważnienie migawki tylko wtedy, gdy jest to wymagane, na przykład gdy jest istotna zmiana zawartości.
- Unikaj unieważniania migawki o dużej częstotliwości. Na przykład aplikacji czasomierza nie należy zaktualizować migawki co sekundę, należy je wykonywać tylko, gdy czasomierz zakończył.

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>Przepływ danych aplikacji

Apple Sugeruj następujące do pracy z przepływem danych:

[![](background-tasks-images/update17.png "Diagram przepływu danych aplikacji")](background-tasks-images/update17.png#lightbox)

Zdarzenie zewnętrzne (na przykład łączności czujki) wznawia działanie aplikacji. Dzięki temu aplikacji na aktualizowanie modelu danych, jej (reprezentujący bieżący stan aplikacji). W wyniku zmiany modelu danych aplikacji należy zaktualizować jej komplikacji żądania nową migawkę, prawdopodobnie uruchomienie tło `NSURLSession` można ściągnąć większej ilości danych i zaplanować dodatkowe tła odświeża.

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>Cykl życia aplikacji

Z powodu doku oraz możliwość przypiąć ulubionych aplikacji do niego Apple sądzi, które użytkownicy będą przenoszenia między znacznie więcej aplikacji, do tej pory częściej, następnie jak z watchOS 2. W związku z tym aplikacja powinna być już obsłużyć tej zmiany i szybkie przenoszenie między Stanami pierwszego planu i tła.

Apple ma poniższe sugestie:

- Upewnij się, że aplikacja kończy wszystkie zadania w tle tak szybko, jak to możliwe, po wprowadzeniu pierwszego planu aktywacji.
- Upewnij się, na zakończenie pracy pierwszego planu przed wejściem w tle przez wywołanie metody `NSProcessInfo.PerformExpiringActivity`.
- Podczas testowania aplikacji w symulatorze watchOS, zostaną Brak budżetów zadań wymuszone, aplikację można odświeżyć jak potrzebne do testowania poprawnie funkcji.
- Należy zawsze przetestować Apple Watch sprzęcie prawdziwe, aby upewnić się, czy aplikacja nie jest uruchomiony poza jego budżetów przed opublikowaniem do iTunes Connect.
- Apple sugeruje utrzymywanie Apple Watch na ładowarki podczas testowania i debugowania.
- Upewnij się, łatwe przetestowane zarówno zimnych uruchamianie i wznawianie aplikacji.
- Sprawdź ukończenie wszystkich zadań aplikacji.
- Różnicowanie liczby aplikacji, które są przypięte w doku do testowania najlepiej i najgorszy przypadek scenariuszy.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego ulepszenia wprowadzone watchOS i jak one używane w celu zapewnienia aktualności aplikacji czujki firmy Apple. Po pierwsze, objęte wszystkie nowe Apple zadania w tle została dodana w watchOS 3. Następnie objęte cykl życia interfejsu API tła i sposobu wykonania zadania w tle w aplikacji watchOS Xamarin. Na koniec objętych działa jak planowania i nadać najlepsze rozwiązania.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
