---
title: Kalendarz
ms.topic: article
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 9b744f4c8582aa9295645b2bdc22e6fddf2bedc3
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="calendar"></a>Kalendarz


## <a name="calendar-api"></a>Kalendarza interfejsu API

Nowy zestaw kalendarza interfejsów API wprowadzona w systemie Android 4 obsługuje aplikacje, które są przeznaczone do odczytu lub zapisu danych do dostawcy kalendarza. Te interfejsy API obsługuje wiele opcji interakcji z danymi kalendarza, łącznie z możliwością odczytu i zapisu zdarzenia, uczestników i przypomnienia. Przy użyciu dostawcy kalendarza w aplikacji, dane, które można dodać za pomocą interfejsu API będą wyświetlane w aplikacji wbudowanych kalendarza dołączoną do systemu Android 4.


## <a name="adding-permissions"></a>Dodawanie uprawnień

Podczas pracy z kalendarzem nowych interfejsów API w aplikacji, w pierwszej kolejności należy wykonać jest dodanie odpowiednich uprawnień do manifestu systemu Android. Należy dodać uprawnienia są `android.permisson.READ_CALENDAR` i `android.permission.WRITE_CALENDAR`, w zależności od tego, czy odczytywania lub zapisywania danych kalendarza.


## <a name="using-the-calendar-contract"></a>Za pomocą kontraktu kalendarza

Po ustawieniu uprawnień, możesz użyć danych kalendarza przy użyciu `CalendarContract` klasy. Ta klasa udostępnia model danych, które aplikacje mogą używać, gdy współdziałają z dostawcą kalendarza. `CalendarContract` Umożliwia aplikacjom rozpoznać identyfikatory URI jednostki kalendarza, na przykład kalendarzy i wydarzeń. Umożliwia także sposób interakcji z różnymi polami w każdej jednostki, takie jak nazwa i identyfikator, lub początek zdarzenia i Data zakończenia w kalendarzu.

Oto przykład, wykorzystujący interfejs API kalendarza. W tym przykładzie zajmiemy się, jak wyliczyć kalendarzy i wydarzeń ich, jak również sposób dodawania nowych zdarzeń do kalendarza.


## <a name="listing-calendars"></a>Listę kalendarzy

Po pierwsze Przeanalizujmy jak wyliczyć kalendarzy, które zostały zarejestrowane w aplikacji kalendarza. Aby to zrobić, możemy utworzyć wystąpienia `CursorLoader`. Wprowadzone w systemie Android 3.0 (11 interfejsu API), `CursorLoader` jest preferowany sposób korzystać z `ContentProvider`. Co najmniej musisz określić identyfikator Uri zawartości kalendarzy i kolumny, które chcemy, aby zwrócić; określenie tej wartości kolumny jest nazywany _projekcji_.

Wywoływanie `CursorLoader.LoadInBackground` metoda pozwala zapytanie do dostawcy zawartości dla danych, takiego jak dostawca kalendarza.
`LoadInBackground` wykonuje operację rzeczywiste obciążenie i zwraca `Cursor` z wynikami zapytania.

`CalendarContract` Pomaga nam w określenie zarówno zawartość `Uri` i projekcji. Można pobrać zawartości `Uri` na potrzeby zapytań kalendarzy, firma Microsoft może po prostu użyj `CalendarContract.Calendars.ContentUri` właściwości w następujący sposób:

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

Przy użyciu `CalendarContract` Aby określić, który kalendarz kolumn chcemy jest równie proste. Firma Microsoft po prostu Dodaj pola w `CalendarContract.Calendars.InterfaceConsts` klasy do tablicy. Na przykład poniższy kod zawiera identyfikator kalendarza, nazwę wyświetlaną i nazwa konta:

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

`Id` Ważne jest, aby uwzględnić, jeśli używasz `SimpleCursorAdapter` powiązać dane do interfejsu użytkownika, jak firma Microsoft będzie wkrótce. Zawartość identyfikatora Uri i projekcji w miejscu, możemy utworzyć wystąpienia `CursorLoader` i Wywołaj `CursorLoader.LoadInBackground` metodę, aby zwrócić kursora z danymi kalendarza, jak pokazano poniżej:

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

Zawiera interfejsu użytkownika w tym przykładzie `ListView`, z każdym elementem na liście reprezentujący pojedynczego kalendarza. Następujący kod XML zawiera kod znaczników, który zawiera `ListView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
  <ListView
    android:id="@android:id/android:list"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```

Ponadto należy określić interfejsu użytkownika dla każdego elementu listy, możemy umieścić w osobnym pliku XML w następujący sposób:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="wrap_content">
  <TextView android:id="@+id/calDisplayName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="16dip" />
  <TextView android:id="@+id/calAccountName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="12dip" />
</LinearLayout>
```

Od tej chwili jest tylko normalne kodu dla systemu Android, aby powiązać dane z kursora do interfejsu użytkownika. Użyjemy `SimpleCursorAdapter` w następujący sposób:

```csharp
string[] sourceColumns = {
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName };

int[] targetResources = {
    Resource.Id.calDisplayName, Resource.Id.calAccountName };      

SimpleCursorAdapter adapter = new SimpleCursorAdapter (this,
    Resource.Layout.CalListItem, cursor, sourceColumns, targetResources);

ListAdapter = adapter;
```

W powyższym kodzie karta przyjmuje kolumn określonych w `sourceColumns` tablicy i zapisuje je w elementy interfejsu użytkownika w `targetResources` tablicy dla każdego wpisu kalendarza w kursorze. Działanie używane w tym miejscu jest podklasą `ListActivity`; zawiera `ListAdapter` właściwości, do którego możemy Ustaw kartę.

Poniżej przedstawiono zrzut ekranu przedstawiający wynik końcowy z użyciem informacji kalendarza, wyświetlane w `ListView`:

[![CalendarDemo uruchomione w emulatorze, wyświetlanie dwóch wpisów kalendarza](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>Lista zdarzenia kalendarza

Następny Zobaczmy, jak wyliczyć zdarzenia dla danego kalendarza.
Kompilowanie na powyższym przykładzie, firma Microsoft będzie ona listę zdarzeń, gdy użytkownik wybierze jeden z kalendarzy. W związku z tym trzeba będzie obsługiwać Zaznaczanie elementów w poprzednim kodzie:

```csharp
ListView.ItemClick += (sender, e) => {
    int i = (e as ItemEventArgs).Position;

    cursor.MoveToPosition(i);
    int calId =
        cursor.GetInt (cursor.GetColumnIndex (calendarsProjection [0]));

    var showEvents = new Intent(this, typeof(EventListActivity));
    showEvents.PutExtra("calId", calId);
    StartActivity(showEvents);
};
```

W tym kodzie tworzymy zamiarem otwieranie działania typu `EventListActivity`, przekazując identyfikator kalendarza celem. Potrzebujemy identyfikator wiedzieć, który kalendarz zapytania do zdarzenia. W `EventListActivity`w `OnCreate` metody, można pobrać identyfikator z `Intent` w sposób przedstawiony poniżej:

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

Teraz załóżmy zdarzeń zapytań dla tego kalendarza identyfikatora. Proces zapytania do zdarzenia jest podobnie jak możemy zbadać listę kalendarzy wcześniej, tylko ten czas, firma Microsoft będzie współpracować `CalendarContract.Events` klasy. Poniższy kod tworzy kwerendę, aby pobrać zdarzenia:

```csharp
var eventsUri = CalendarContract.Events.ContentUri;

string[] eventsProjection = {
    CalendarContract.Events.InterfaceConsts.Id,
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart
};

var loader = new CursorLoader(this, eventsUri, eventsProjection,
                   String.Format ("calendar_id={0}", _calId), null, "dtstart ASC");
var cursor = (ICursor)loader.LoadInBackground();
```

W tym kodzie możemy najpierw Pobierz zawartość `Uri` zdarzeń z `CalendarContract.Events.ContentUri` właściwości. Następnie określ kolumny zdarzeń, które chcesz pobrać w tablicy eventsProjection.
Na koniec, możemy utworzyć wystąpienia `CursorLoader` te informacje i wywołania modułu ładującego `LoadInBackground` metodę, aby zwrócić `Cursor` z danych zdarzenia.

Aby wyświetlić dane zdarzenie w interfejsie użytkownika, możemy użyć znaczników i kodu tak samo, jak robiliśmy przed Aby wyświetlić listę kalendarzy. Ponownie używamy `SimpleCursorAdapter` wiązanie danych do `ListView` zgodnie z poniższym kodem:

```csharp
string[] sourceColumns = {
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart };

int[] targetResources = {
    Resource.Id.eventTitle,
    Resource.Id.eventStartDate };

var adapter = new SimpleCursorAdapter (this, Resource.Layout.EventListItem,
    cursor, sourceColumns, targetResources);

adapter.ViewBinder = new ViewBinder ();       
ListAdapter = adapter;
```

Podstawowa różnica między ten kod i kodu, które zostały wcześniej użyte do wyświetlenia na liście kalendarza jest użycie `ViewBinder`, która jest ustawiona w wierszu:

```csharp
adapter.ViewBinder = new ViewBinder ();
```

`ViewBinder` Klasa umożliwia firmie Microsoft w celu ściślejszego sterowania, jak firma Microsoft powiązać wartości widoków. W takim przypadku stosujemy go godziny rozpoczęcia zdarzenia w milisekundach jest skonwertowana do ciągu daty, jak pokazano w poniższych implementacji:

```csharp
class ViewBinder : Java.Lang.Object, SimpleCursorAdapter.IViewBinder
{    
    public bool SetViewValue (View view, Android.Database.ICursor cursor,
        int columnIndex)
    {
        if (columnIndex == 2) {
            long ms = cursor.GetLong (columnIndex);

            DateTime date = new DateTime (1970, 1, 1, 0, 0, 0,
                DateTimeKind.Utc).AddMilliseconds (ms).ToLocalTime ();

            TextView textView = (TextView)view;
            textView.Text = date.ToLongDateString ();

            return true;
        }
        return false;
    }    
}
```

Wyświetla listę zdarzeń, jak pokazano poniżej:

[![Zrzut ekranu przedstawiający przykładową aplikację wyświetlanie trzy zdarzenia kalendarza](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>Dodawanie zdarzenia kalendarza

Firma Microsoft przedstawiono sposób odczytywania danych kalendarza. Teraz zobaczmy, jak dodać zdarzeń do kalendarza. Aby to zrobić, należy uwzględnić `android.permission.WRITE_CALENDAR` uprawnienia wspomniano wcześniej. Aby dodać zdarzeń do kalendarza, zostaną wykonane następujące czynności:

1.  Utwórz `ContentValues` wystąpienia.
1.  Użyj klawiszy z `CalendarContract.Events.InterfaceConsts` klasę, aby wypełnić `ContentValues` wystąpienia.
1.  Ustaw stref czasowych rozpoczęcia zdarzenia i zakończenia godzin.
1.  Użyj `ContentResolver` do wstawiania danych zdarzeń do kalendarza.


Poniższy kod ilustruje następujące kroki:

```csharp
ContentValues eventValues = new ContentValues ();

eventValues.Put (CalendarContract.Events.InterfaceConsts.CalendarId,
    _calId);
eventValues.Put (CalendarContract.Events.InterfaceConsts.Title,
    "Test Event from M4A");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Description,
    "This is an event created from Xamarin.Android");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtstart,
    GetDateTimeMS (2011, 12, 15, 10, 0));
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtend,
    GetDateTimeMS (2011, 12, 15, 11, 0));

eventValues.Put(CalendarContract.Events.InterfaceConsts.EventTimezone,
    "UTC");
eventValues.Put(CalendarContract.Events.InterfaceConsts.EventEndTimezone,
    "UTC");

var uri = ContentResolver.Insert (CalendarContract.Events.ContentUri,
    eventValues);
```

Należy pamiętać, że jeśli firma Microsoft nie Ustaw strefę czasową, wyjątek typu `Java.Lang.IllegalArgumentException` zostanie wygenerowany. Ponieważ wartości czasu zdarzeń muszą być wyrażone w milisekundach od epoki, utworzymy `GetDateTimeMS` — metoda (w `EventListActivity`) można przekonwertować naszych specyfikacje Data w formacie milisekund:

```csharp
long GetDateTimeMS (int yr, int month, int day, int hr, int min)
{
    Calendar c = Calendar.GetInstance (Java.Util.TimeZone.Default);

    c.Set (Java.Util.CalendarField.DayOfMonth, 15);
    c.Set (Java.Util.CalendarField.HourOfDay, hr);
    c.Set (Java.Util.CalendarField.Minute, min);
    c.Set (Java.Util.CalendarField.Month, Calendar.December);
    c.Set (Java.Util.CalendarField.Year, 2011);

    return c.TimeInMillis;
}
```

Jeśli firma Microsoft Dodawanie przycisku do listy zdarzeń interfejsu użytkownika i uruchamiania powyższej kodu na przycisku kliknij program obsługi zdarzeń, zdarzenie zostanie dodany do kalendarza i aktualizowane w naszej listy, jak pokazano poniżej:

[![Zrzut ekranu przedstawiający przykładową aplikację z zdarzenia kalendarza, a następnie przycisk Dodaj zdarzenie próbkowania](calendar-images/13.png)](calendar-images/13.png#lightbox)

Jeśli firma Microsoft Otwórz aplikację kalendarza, następnie zostanie wyświetlone że zdarzenie jest zapisywane istnieje również:

[![Zrzut ekranu przedstawiający kalendarza aplikacji wyświetlanie zdarzenia kalendarza](calendar-images/14.png)](calendar-images/14.png#lightbox)

Jak widać, Android umożliwia wydajne i łatwy dostęp do pobrania i utrwalenia danych kalendarza, umożliwiając aplikacjom bezproblemowe integrowanie możliwości kalendarza.


## <a name="related-links"></a>Linki pokrewne

- [Wersja demonstracyjna kalendarza (przykład)](https://developer.xamarin.com/samples/CalendarDemo/)
- [Wprowadzenie Sandwich lodów](http://www.android.com/about/ice-cream-sandwich/)
- [Platformy systemu android 4.0](http://developer.android.com/sdk/android-4.0.html)
