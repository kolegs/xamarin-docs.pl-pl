---
title: EventKit
description: "Ten przewodnik zawiera omówienie dotyczące dostępu i współpracować z kalendarzy, CalendarEvents i przypomnienia danych przechowywanych w bazie danych kalendarza udostępniane za pośrednictwem funkcji EventKit. Obejmuje ona główne kategorie i ich role w EventKit programowania, a także szereg typowych zadań związanych z EventKit framework."
ms.topic: article
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: db3662db50d8f3538f16f2af1f9e7880957dc25c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="eventkit"></a>EventKit

_Ten przewodnik zawiera omówienie dotyczące dostępu i współpracować z kalendarzy, CalendarEvents i przypomnienia danych przechowywanych w bazie danych kalendarza udostępniane za pośrednictwem funkcji EventKit. Obejmuje ona główne kategorie i ich role w EventKit programowania, a także szereg typowych zadań związanych z EventKit framework._

iOS ma dwie aplikacje związanych z kalendarzem wbudowanych: kalendarza aplikacji i przypomnienia aplikacji. Jest prosta zrozumieć, jak aplikacja kalendarza zarządza danych kalendarza, ale aplikacja przypomnienia jest mniej oczywista. Przypomnienia faktycznie może mieć dat skojarzonych z nimi w postaci liczby, gdy są one powodu, gdy jest ukończona, itp. W związku z systemem iOS przechowuje wszystkie dane kalendarza, czy jest ona zdarzenia kalendarza lub przypomnienia w jednej lokalizacji o nazwie *bazę danych kalendarza*.

EventKit framework zapewnia dostęp do *kalendarzy*, *zdarzenia kalendarza*, i *przypomnienia* danych przechowywanych w bazie danych kalendarza. Dostęp do kalendarzy i kalendarza zdarzeń została dostępne począwszy od zestawu iOS 4, ale jest nowy w systemie iOS 6 do przypomnienia.

W tym przewodniku zamierzamy obejmują:

-   **Podstawy EventKit** — to wprowadzi podstawowe elementy EventKit za pośrednictwem główne kategorie i udostępnia zrozumienie ich użycia. Ta sekcja jest wymagana, odczytywanie przed czoła następnej części dokumentu. 
-   **Typowe zadania** — sekcji typowych zadań ma być podręczny wykaz, jak to zrobić wspólne elementy, takie jak; wyliczanie kalendarzy, tworzenia, zapisywania i pobierania kalendarza zdarzeń i przypomnienia, jak również za pomocą wbudowanych kontrolerów dla Tworzenie i modyfikowanie zdarzenia kalendarza. W tej sekcji należy nie można odczytać przodu do tyłu, ponieważ spowodował on odwołania dla poszczególnych zadań. 


Wszystkie zadania w tym przewodniku są dostępne w pomocnika przykładowej aplikacji:

 [ ![](eventkit-images/01.png "Ekrany pomocnika przykładowej aplikacji")](eventkit-images/01.png)

## <a name="requirements"></a>Wymagania

EventKit została wprowadzona w systemie iOS 4.0, ale dostęp do danych przypomnienia została wprowadzona w systemie iOS 6.0. Tak, należy w celu ogólnego rozwoju EventKit należy docelowy co najmniej w wersji 4.0 i przypomnienia w wersji 6.0.

Ponadto aplikacja przypomnienia nie jest dostępna w symulatorze, co oznacza, że przypomnienia danych także nie będą dostępne, chyba że je najpierw dodać. Ponadto żądań dostępu są wyświetlane tylko dla użytkownika na rzeczywistego urządzenia. Tak EventKit programowanie najlepiej jest testowane na urządzeniu.

## <a name="event-kit-basics"></a>Podstawowe informacje dotyczące zdarzeń Kit

Podczas pracy z EventKit, należy mieć ujmij klasy wspólnych oraz ich użycia. Wszystkie te klasy można znaleźć w `EventKit` i `EventKitUI` (dla `EKEventEditController`).

### <a name="eventstore"></a>EventStore

*EventStore* klasy to klasa najważniejszych w EventKit, ponieważ są wymagane, aby wykonywać żadnych operacji w EventKit. Go można traktować jako magazynu trwałego lub aparatu bazy danych dla wszystkich danych EventKit. Z `EventStore` masz dostęp do kalendarzy i zdarzenia kalendarza w aplikacji kalendarza, a także przypomnienia w aplikacji przypomnienia.

Ponieważ `EventStore` jest jak aparatu bazy danych, powinny być długotrwałe, co oznacza, że powinny być tworzone i niszczone jak najmniejszy przez cały okres istnienia wystąpienia aplikacji. W rzeczywistości ma zalecane po utworzeniu jednego wystąpienia `EventStore` w aplikacji, zachowasz odwołującymi się wokół przez cały czas ich istnienia aplikacji, jeśli nie masz pewności, nie trzeba go ponownie. Ponadto wszystkie wywołania powinna przejdź do jednego `EventStore` wystąpienia. Do przechowywania pojedynczego wystąpienia dostępne z tego powodu zaleca się wzorzec Singleton.

#### <a name="creating-an-event-store"></a>Utworzenie magazynu zdarzeń

Poniższy kod ilustruje wydajny sposób, aby utworzyć jedno wystąpienie `EventStore` klasy i udostępni go statycznie z poziomu aplikacji:

```csharp
public class App
{
    public static App Current {
            get { return current; }
    }
    private static App current;

    public EKEventStore EventStore {
            get { return eventStore; }
    }
    protected EKEventStore eventStore;

    static App ()
    {
            current = new App();
    }
    protected App () 
    {
            eventStore = new EKEventStore ( );
    }
}
```

Powyższy kod korzysta ze wzorca Singleton można utworzyć wystąpienia elementu `EventStore` podczas ładowania aplikacji. `EventStore` Mają dostęp globalny z poziomu aplikacji w następujący sposób:

```csharp
App.Current.EventStore;
```

Należy pamiętać, że wszystkie przykłady w tym miejscu użyć tego wzorca tak odwołujących się do `EventStore` za pośrednictwem `App.Current.EventStore`.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>Żądanie dostępu do kalendarza i dane monitu

Przed uzyskaniem dostępu do żadnych danych za pośrednictwem EventStore aplikacji muszą najpierw zażądać dostępu do danych zdarzenia kalendarza lub przypomnienia dane, w zależności od tego, która z nich. Ułatwia to `EventStore` udostępnia metodę o nazwie `RequestAccess` który — wywołanego — wyświetli widok alertów dla użytkownika z informacją o tym, że aplikacja żąda dostępu do danych kalendarza, albo monitu danych, w zależności od tego, które `EKEntityType`jest przekazywany do niego. Ponieważ uruchamia widok alertów, wywołanie jest asynchroniczne, a wywoła obsługi uzupełniania przekazany jako `NSAction` (lub Lambda) do niego, które będzie odbierało dwóch parametrów; boolean elementu czy przyznano dostęp i `NSError`, która jeśli będzie not null zawiera informacje o błędzie w żądaniu. Na przykład następujące kodowanego zażąda dostępu do danych zdarzenia kalendarza i Pokaż wyświetlić alert, jeśli żądanie nie zostało zrealizowane.

```csharp
App.Current.EventStore.RequestAccess (EKEntityType.Event, 
    (bool granted, NSError e) => {
            if (granted)
                    //do something here
            else
                    new UIAlertView ( "Access Denied", 
"User Denied Access to Calendar Data", null,
"ok", null).Show ();
            } );
```

Po udzieleniu żądanie zostanie zapamiętany tak długo, jak aplikacja jest zainstalowana na urządzeniu i nie będzie wyskakujące alert dla użytkownika.
Jednak tylko uzyskują dostęp do typu zasobu, zdarzenia kalendarza lub przypomnienia przyznane. Jeśli aplikacja potrzebuje dostępu do obu, go zażądać obu.

Ponieważ uprawnienia są zapamiętywane, jest stosunkowo tanie do wysłania żądania każdorazowo, dzięki czemu dobrze jest zawsze żądania dostępu przed wykonaniem operacji.

Ponadto, ponieważ program obsługi uzupełniania jest wywołany w wątku, oddzielne (bez interfejsu użytkownika), wszelkie aktualizacje interfejsu użytkownika programu obsługi zakończenia powinna być wywoływana za pośrednictwem `InvokeOnMainThread`, w przeciwnym razie zostanie wygenerowany wyjątek, a jeśli nie przechwycono, aplikacja ulegnie awarii.

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` to wyliczenie opisujące typ `EventKit` element lub danych. Składa się z dwóch wartości: `Event` i przypomnienia. Jest on używany na kilka różnych metod, w tym `EventStore.RequestAccess` mówić `EventKit` rodzaj danych, aby uzyskać dostęp do lub pobrać.

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar* reprezentuje kalendarza, który zawiera grupę zdarzenia kalendarza. Kalendarze mogą być przechowywane w wielu różnych miejscach, takich jak lokalnie, w *iCloud*w 3rd strony lokalizacji dostawcy, takich jak *programu Exchange Server* lub *Google*itp. Wiele razy `EKCalendar` informuje `EventKit` gdzie szukać zdarzenia lub miejsca je zapisać.

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController* można znaleźć w `EventKitUI` przestrzeni nazw i jest wbudowany kontroler, który służy do edytowania lub tworzenia zdarzenia kalendarza. Podobnie jak wbudowanych w kontrolerów aparatu `EKEventEditController` czy lifting ciężki w wyświetlanie interfejsu użytkownika i obsługa zapisywania.

### <a name="ekevent"></a>EKEvent

 *EKEvent* reprezentuje zdarzenia kalendarza. Zarówno `EKEvent` i `EKReminder` dziedziczyć `EKCalendarItem` i mieć pól, takich jak `Title`, `Notes`i tak dalej.

### <a name="ekreminder"></a>EKReminder

 *EKReminder* reprezentuje element monitu.

### <a name="ekspan"></a>EKSpan

*EKSpan* jest wyliczenie opisujące zakres zdarzeń podczas modyfikowania zdarzeń, które można powtarzać i ma dwie wartości: *ThisEvent* i *FutureEvents*. `ThisEvent` oznacza, że wszelkie zmiany będzie miało miejsce tylko do określonego zdarzenia w serii, która odwołuje się do, podczas gdy `FutureEvents` będzie miało wpływ na zdarzenie, a wszystkie przyszłe powtórzenia.

## <a name="tasks"></a>Zadania

Łatwość użycia, aby uzyskać EventKit użycia ma została podzielona na typowe zadania opisane w poniższych sekcjach.

### <a name="enumerate-calendars"></a>Wyliczanie kalendarzy

Aby wyliczyć kalendarzy skonfigurowane na urządzeniu, należy wywołać `GetCalendars` na `EventStore` i przekazać typ kalendarzy (przypomnienia lub zdarzeń), które mają być wysyłane:

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>Dodaje lub modyfikuje zdarzenia za pomocą wbudowanego kontrolera

*EKEventEditViewController* jest dużo lifting ciężki dla Ciebie, jeśli chcesz utworzyć lub edytować zdarzenia przy użyciu tego samego interfejsu użytkownika podczas korzystania z aplikacji kalendarza zostanie wyświetlony dla użytkownika:

 [ ![](eventkit-images/02.png "Interfejs użytkownika, który jest proponowane użytkownikowi w przypadku korzystania z aplikacji kalendarza")](eventkit-images/02.png)

Aby go użyć, należy zadeklarować jako zmienną poziomie klasy, tak, aby go nie uzyskiwać zbierane pamięci, jeśli jest zadeklarowana w metodzie:

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

Następnie można go uruchomić: utworzyć, nadaj odwołanie do `EventStore`, połączenie się *EKEventEditViewDelegate* przekazać go, a następnie wyświetlać je za pomocą `PresentViewController`:

```csharp
EventKitUI.EKEventEditViewController eventController = 
        new EventKitUI.EKEventEditViewController ();

// set the controller's event store - it needs to know where/how to save the event
eventController.EventStore = App.Current.EventStore;

// wire up a delegate to handle events from the controller
eventControllerDelegate = new CreateEventEditViewDelegate ( eventController );
eventController.EditViewDelegate = eventControllerDelegate;

// show the event controller
PresentViewController ( eventController, true, null );
```

Opcjonalnie Jeśli chcesz wstępnie wypełnić zdarzenia, możesz utworzyć nowego zdarzenia (jak pokazano poniżej) lub można pobrać zapisanych zdarzeń:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and exercise!";
newEvent.Notes = "This is your reminder to go and exercise for 30 minutes.”;
```

Jeśli chcesz wstępnie wypełnić interfejsu użytkownika, należy ustawić właściwość zdarzenia na kontrolerze:

```csharp
eventController.Event = newEvent;
```

Aby użyć istniejącego zdarzenia, zobacz *pobrać zdarzenia według Identyfikatora* sekcji później.

Delegat powinny zastępować `Completed` metodę, która jest wywoływana przez kontroler, gdy zostanie zakończone z okna dialogowego:

```csharp
protected class CreateEventEditViewDelegate : EventKitUI.EKEventEditViewDelegate
{
        // we need to keep a reference to the controller so we can dismiss it
        protected EventKitUI.EKEventEditViewController eventController;

        public CreateEventEditViewDelegate (EventKitUI.EKEventEditViewController eventController)
        {
                // save our controller reference
                this.eventController = eventController;
        }

        // completed is called when a user eith
        public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
        {
                eventController.DismissViewController (true, null);
                }
        }
}
```

Opcjonalnie w elemencie delegowanym, można sprawdzić *akcji* w `Completed` metodę, aby zmodyfikować zdarzeń i ponownie zapisać lub wykonywać inne czynności, jeśli zostanie ona anulowana, etcetera:

```csharp
public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
{
        eventController.DismissViewController (true, null);

        switch ( action ) {

        case EKEventEditViewAction.Canceled:
                break;
        case EKEventEditViewAction.Deleted:
                break;
        case EKEventEditViewAction.Saved:
                // if you wanted to modify the event you could do so here,
// and then save:
                //App.Current.EventStore.SaveEvent ( controller.Event, )
                break;
        }
}
```

### <a name="creating-an-event-programmatically"></a>Programowe tworzenie zdarzenia

Aby utworzyć zdarzenie w kodzie, użyj *FromStore* metoda fabryki na `EKEvent` klasy, a następnie ustawić na nim żadnych danych:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and do some exercise!";
newEvent.Notes = "This is your motivational event to go and do 30 minutes of exercise. Super important. Do this.";
```

Należy ustawić interesujące zdarzenia zapisywane w kalendarzu, ale jeśli brak preferencji, użytkownik korzysta z domyślnych:

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

Aby zapisać zdarzenia, należy wywołać *SaveEvent* metoda `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

Po zapisaniu, *EventIdentifier* zostanie zaktualizowana z unikatowym identyfikatorem później służący do pobierania zdarzeń:

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` to ciąg sformatowany identyfikator GUID.

### <a name="create-a-reminder-programmatically"></a>Programowe tworzenie przypomnienia

Tworzenie przypomnienia w kodzie jest znacznie takie same, jak podczas tworzenia zdarzenia kalendarza:

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

Aby zapisać, należy wywołać *SaveReminder* metoda `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>Pobieranie zdarzenia według Identyfikatora

Można pobrać zdarzenia przez jego identyfikator, użyj *EventFromIdentifier* metoda `EventStore` i przekaż go `EventIdentifier` ściągnięcia ze zdarzenia:

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

Zdarzenia, brak są dwa inne właściwości identyfikatora, ale `EventIdentifier` jest tylko jeden, która działa w tym.

### <a name="retrieving-a-reminder-by-id"></a>Trwa pobieranie monitu według Identyfikatora

Aby uzyskać monit, użyj *GetCalendarItem* metoda `EventStore` i przekaż go *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

Ponieważ `GetCalendarItem` zwraca `EKCalendarItem`, musi być rzutowane na `EKReminder` Jeśli musisz uzyskać dostęp do danych monitu lub użyć wystąpienia jako `EKReminder` później.

Nie używaj `GetCalendarItem` zdarzeń kalendarza, ponieważ w czasie zapisu, nie działa.

### <a name="deleting-an-event"></a>Usuwanie zdarzeń

Aby usunąć zdarzenia kalendarza, wywołaj *RemoveEvent* na Twojej `EventStore` i przekaż odwołanie do zdarzenia, a odpowiedni `EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

Uwaga Po usunięciu zdarzenia odwołanie do zdarzenia będą jednak `null`.

### <a name="deleting-a-reminder"></a>Usuwanie przypomnienia

Aby usunąć przypomnienie, wywołaj *RemoveReminder* na `EventStore` i przekaż odwołanie do monitu:

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

Należy pamiętać, że w powyższym kodzie Rzutowanie na `EKReminder`, ponieważ `GetCalendarItem` został użyty do pobrania go

### <a name="searching-for-events"></a>Wyszukiwanie zdarzeń

Aby wyszukać zdarzenia kalendarza, należy utworzyć *NSPredicate* obiektu za pomocą *PredicateForEvents* metoda `EventStore`. `NSPredicate` Kwerendy jest obiekt danych tego systemu iOS używa do lokalizowania dopasowań:

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

Po utworzeniu `NSPredicate`, użyj *EventsMatching* metoda `EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

Należy pamiętać, że zapytania są synchroniczne (blokowanie) i może zająć dużo czasu, w zależności od tego zapytania, może zajść potrzeba aż nowego wątku lub zadań, aby wykonać to zadanie.

### <a name="searching-for-reminders"></a>Wyszukiwanie przypomnienia

Wyszukiwanie przypomnienia jest podobny do zdarzeń; wymaga to predykatu, ale wywołanie już jest asynchroniczne, więc nie musi martwić blokuje wątku:

```csharp
// create our NSPredicate which we'll use for the query
NSPredicate query = App.Current.EventStore.PredicateForReminders ( null );

// execute the query
App.Current.EventStore.FetchReminders (
        query, ( EKReminder[] items ) => {
                // do someting with the items
        } );
```

## <a name="summary"></a>Podsumowanie

Ten dokument udzielił omówienie zarówno ważne elementy EventKit framework i kilka najbardziej typowych zadań. Jednak EventKit framework jest bardzo duży i wydajne i zawiera funkcje, które nie zostały wprowadzone w tym miejscu, takich jak: partii aktualizacje, konfigurowanie alarmy Konfigurowanie cyklu na zdarzenia, rejestrowanie i nasłuchiwanie zmian w bazie danych kalendarza Ustawienie wirtualne ogrodzenia i inne.  Aby uzyskać więcej informacji, zobacz firmy Apple [kalendarza i przypomnienia przewodnik programowania w języku](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html).


## <a name="related-links"></a>Linki pokrewne

- [Kalendarze (przykład)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [Wprowadzenie do systemu iOS 6](~/ios/platform/introduction-to-ios6/index.md)
- [Wprowadzenie do kalendarzy i przypomnienia](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
