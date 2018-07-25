---
title: Selektor godziny
description: Wybieranie godziny za pomocą TimePickerDialog i DialogFragment
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 85d887cba7a596226d44bc13ca7155bc4a0d03ee
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242306"
---
# <a name="time-picker"></a>Selektor godziny

Aby umożliwić użytkownikowi na wybranie czasu, można użyć [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/). Aplikacje dla systemu android zazwyczaj używa się `TimePicker` z [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/) służąca do wybierania wartości godziny &ndash; pomaga to zapewnić spójny interfejs między urządzeniami i aplikacjami. `TimePicker` Umożliwia użytkownikom wybranie porze dnia w trybie 24-godzinny lub 12-godzinny AM/PM.
`TimePickerDialog` jest pomocnikiem klasy, która hermetyzuje `TimePicker` w oknie dialogowym.

[![Zrzut ekranu przedstawiający okno dialogowe selektora czasu w działaniu](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>Omówienie

Wyświetlanie nowoczesnych aplikacji dla systemu Android `TimePickerDialog` w [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Dzięki temu możliwe dla aplikacji, aby wyświetlić `TimePicker` jako menu podręczne okno dialogowe lub osadzenie go w działaniu. Ponadto `DialogFragment` zarządza cyklem życia i wyświetlanie okna dialogowego, co zmniejsza ilość kodu, które należy zaimplementować.

Ten przewodnik pokazuje sposób użycia `TimePickerDialog`otoczone `DialogFragment`. Wyświetla aplikacji przykładowej `TimePickerDialog` jako modalne okno dialogowe, gdy użytkownik kliknie przycisk w ramach działania. Jeśli czas jest ustawiony przez użytkownika, zamyka okno dialogowe i program obsługi aktualizacji `TextView` na ekranie działania o czasie, który został wybrany.

## <a name="requirements"></a>Wymagania

Przykładowa aplikacja dla tego przewodnika jest przeznaczony dla systemu Android 4.1 (poziom interfejsu API
16) lub nowszej, ale mogą być używane z 3.0 dla systemu Android (poziom interfejsu API 11 lub nowszy). Istnieje możliwość do obsługi starszych wersji systemu android dodając v4 biblioteki obsługi systemu Android do projektu i niektórych zmian w kodzie.

## <a name="using-the-timepicker"></a>Za pomocą TimePicker

W tym przykładzie rozszerza `DialogFragment`; wykonania podklasy `DialogFragment` (o nazwie `TimePickerFragment` poniżej) obsługuje i wyświetla `TimePickerDialog`. Po pierwszym uruchomieniu Przykładowa aplikacja wyświetla **czas pobrania** powyższego przycisku `TextView` który będzie używany do wyświetlania wybrana wartość czasu:

[![Początkowe próbki ekranu aplikacji](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Po kliknięciu **czas pobrania** przycisk zostanie uruchomiona aplikacja przykład `TimePickerDialog` jak widać na tym zrzucie ekranu:

[![Zrzut ekranu okna dialogowego selektora czasu domyślne wyświetlany przez aplikację](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

W `TimePickerDialog`, wybierając czas i klikając **OK** przycisku powoduje, że `TimePickerDialog` było wywołanie metody [IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/).
Ten interfejs jest implementowany przez hostingu `DialogFragment` (`TimePickerFragment`, które zostały opisane poniżej). Klikając **anulować** przycisku powoduje, że fragment i okno dialogowe, aby być ukryty.

`DialogFragment` Zwraca zaznaczony czas do hostingu Actvity w jeden z trzech sposobów:

1. **Wywoływanie metody lub ustawienie właściwości** &ndash; działanie może zapewnić właściwość lub metodę specjalnie ustawienie tej wartości.

2. **Podnoszenie zdarzenia** &ndash; `DialogFragment` można zdefiniować zdarzenie, które będą podniesione, gdy `OnTimeSet` jest wywoływana.

3. **Za pomocą `Action`**  &ndash; `DialogFragment` może wywołać `Action<DateTime>` do wyświetlania czasu w działaniu. Działanie zapewni `Action<DateTime` podczas tworzenia wystąpienia `DialogFragment`. 

W tym przykładzie użyje trzeci techniką, która wymaga, aby dostaw działania `Action<DateTime>` program obsługi `DialogFragment`.



## <a name="start-an-app-project"></a>Rozpocznij projekt aplikacji

Utwórz nowy projekt dla systemu Android o nazwie **TimePickerDemo** (Jeśli użytkownik nie jest zaznajomiony z tworzeniem projekty platformy Xamarin.Android, zobacz [Witaj, Android](~/android/get-started/hello-android/hello-android-quickstart.md) dowiesz się, jak utworzyć nowy projekt).

Edytuj **Resources/layout/Main.axml** i zastąp jego zawartość następujący kod XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_gravity="center_horizontal"
    android:padding="16dp">
    <Button
        android:id="@+id/select_button"
        android:paddingLeft="24dp"
        android:paddingRight="24dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="PICK TIME"
        android:textSize="20dp" />
    <TextView
        android:id="@+id/time_display"
        android:layout_height="wrap_content"
        android:layout_width="match_parent"
        android:paddingTop="22dp"
        android:text="Picked time will be displayed here"
        android:textSize="24dp" />
</LinearLayout>
```

Jest to podstawowy [LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) z [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/) wyświetlającą czas i [przycisk](https://developer.xamarin.com/api/type/Android.Widget.Button/) które otwiera `TimePickerDialog`. Należy pamiętać, że ten układ używa zakodowane sprzętowo ciągi i wymiarów, aby zapewnić aplikacji, prostsze i łatwiejsze do zrozumienia &ndash; aplikacji produkcyjnej zwykle używa zasobów dla tych wartości (jak widać w [DatePicker](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) przykład kodu).

Edytuj **MainActivity.cs** i zastąp jego zawartość następującym kodem:

```csharp
using Android.App;
using Android.Widget;
using Android.OS;
using System;
using Android.Util;
using Android.Text.Format;

namespace TimePickerDemo
{
    [Activity(Label = "TimePickerDemo", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        TextView timeDisplay;
        Button timeSelectButton;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            SetContentView(Resource.Layout.Main);
            timeDisplay = FindViewById<TextView>(Resource.Id.time_display);
            timeSelectButton = FindViewById<Button>(Resource.Id.select_button);
        }
    }
}
```

Gdy kompilujesz i przeprowadzania tego przykładu, powinien zostać wyświetlony ekran początkowy, podobnie jak na poniższym zrzucie ekranu:

[![Ekran początkowy aplikacji](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Klikając **czas pobrania** przycisk nic nie robi ponieważ `DialogFragment` nie została jeszcze zaimplementowana do wyświetlenia `TimePicker`.
Następnym krokiem jest utworzenie to `DialogFragment`.



## <a name="extending-dialogfragment"></a>Rozszerzanie DialogFragment

Aby rozszerzyć `DialogFragment` do użytku z programem `TimePicker`, należy utworzyć podklasę, która jest pochodną `DialogFragment` i implementuje `TimePickerDialog.IOnTimeSetListener`. Dodaj poniższą klasę do **MainActivity.cs**:

```csharp
public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
{
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };

    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }

    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }

    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
        Log.Debug(TAG, selectedTime.ToLongTimeString());
        timeSelectedHandler (selectedTime);
    }
}
```

To `TimePickerFragment` klasa jest podzielony na mniejsze kawałki i wyjaśniono w następnej sekcji.


### <a name="dialogfragment-implementation"></a>Implementacja DialogFragment

`TimePickerFragment` implementuje kilka metod: metoda fabryki, okno dialogowe metodę wystąpienia, a `OnTimeSet` metody obsługi wymaganych przez `TimePickerDialog.IOnTimeSetListener`.

-   `TimePickerFragment` jest podklasą `DialogFragment`. Implementuje także `TimePickerDialog.IOnTimeSetListener` interfejsu (czyli dostarcza wymagane `OnTimeSet` metoda):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` zainicjowano dla celów rejestrowania (*MyTimePickerFragment* można zmienić do dowolnego ciągu chcesz użyć). `timeSelectedHandler` Akcji jest inicjowany do pustego delegata, aby zapobiec wyjątki odwołanie o wartości null:

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   `NewInstance` Fabryki metoda jest wywoływana w celu tworzenia wystąpienia nowego `TimePickerFragment`. Ta metoda przyjmuje `Action<DateTime>` program obsługi, które jest wywoływane, gdy użytkownik kliknie **OK** znajdujący się w `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   Gdy fragment ma być wyświetlana, wywołuje Android `DialogFragment` metoda [OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/). 
    Ta metoda tworzy nowy `TimePickerDialog` obiektu i inicjuje ją za pomocą działania obiektu wywołania zwrotnego (czyli bieżące wystąpienie `TimePickerFragment`) i bieżący czas:

    ```csharp
    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }
    ```

-   Gdy użytkownik zmieni ustawienie czasu `TimePicker` okno dialogowe, `OnTimeSet` metoda jest wywoływana. `OnTimeSet` Tworzy `DateTime` przy użyciu z bieżącą datą i scalenia w czasie (godzinowe i minutowe), które zostały wybrane przez użytkownika:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   To `DateTime` obiekt jest przekazywany do `timeSelectedHandler` zarejestrowaniu w usłudze `TimePickerFragment` obiektu w czasie jego tworzenia. `OnTimeSet` wywołuje ten program obsługi aktualizacji wyświetlanie godziny działania do wybranego czasu (ten program obsługi jest zaimplementowana w następnej sekcji):

    ```csharp
    timeSelectedHandler (selectedTime);
    ```



## <a name="displaying-the-timepickerfragment"></a>Wyświetlanie TimePickerFragment

Teraz, gdy `DialogFragment` został zaimplementowany, jest moment, aby utworzyć wystąpienie `DialogFragment` przy użyciu `NewInstance` metoda fabryki i wyświetl ją za pomocą wywołania [DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

Dodaj następującą metodę do `MainActivity`:

```csharp
void TimeSelectOnClick (object sender, EventArgs eventArgs)
{
    TimePickerFragment frag = TimePickerFragment.NewInstance (
        delegate (DateTime time)
        {
            timeDisplay.Text = time.ToShortTimeString();
        });

    frag.Show(FragmentManager, TimePickerFragment.TAG);
}
```

Po `TimeSelectOnClick` tworzy `TimePickerFragment`, tworzy i przekazuje delegata dla metody anonimowej, która aktualizuje działania, wyświetlanie godziny za pomocą wartości przekazywane w czasie. Na koniec uruchamia `TimePicker` fragmentu okna dialogowego (za pośrednictwem `DialogFragment.Show`) do wyświetlenia `TimePicker` dla użytkownika.

Na koniec `OnCreate` metody, Dodaj następujący wiersz, aby dołączyć program obsługi zdarzeń do **czas pobrania** przycisk, który uruchamia okno dialogowe:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

Gdy **czas pobrania** kliknięto przycisk `TimeSelectOnClick` zostanie wywołany, aby wyświetlić `TimePicker` fragmentu okna dialogowego, aby użytkownik.



## <a name="try-it"></a>Wypróbuj!

Skompiluj i uruchom aplikację. Po kliknięciu **czas pobrania** przycisku `TimePickerDialog` jest wyświetlana w domyślnym formacie czas działania (w tym przypadku, 12-godzinny AM/PM trybie):

[![Zostanie wyświetlone okno dialogowe czas, w trybie AM/PM](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
Po kliknięciu **OK** w `TimePicker` okno dialogowe, program obsługi aktualizacji działania `TextView` przy wybranym czasie, a następnie zamyka:

[![Czas A/M jest wyświetlany w TextView działania](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

Następnie dodaj następujący wiersz kodu w celu `OnCreateDialog` natychmiast po `is24HourFormat` są deklarowane i inicjowane:

```csharp
is24HourFormat = true;
```

Ta zmiana wymusza flagi przekazywane do `TimePickerDialog` Konstruktor jako `true` więc ten tryb 24-godzinny jest używana zamiast format czasu działania hostingu. Gdy kompilujesz i ponownie uruchom aplikację, kliknij przycisk **czas pobrania** przycisku `TimePicker` w formacie 24-godzinnym teraz zostanie wyświetlone okno dialogowe:

[![Okno dialogowe TimePicker, w formacie 24-godzinnym](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

Ponieważ program obsługi wyjątku wywołuje [DateTime.ToShortTimeString](https://msdn.microsoft.com/en-us/library/system.datetime.toshortdatestring%28v=vs.110%29.aspx) do drukowania czas działania `TextView`, czas nadal jest drukowany w domyślnym formacie AM/PM 12-godzinnym.



## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób wyświetlania `TimePicker` widżet jako menu podręczne modalne okno dialogowe z działanie systemu Android. Ona udostępniana próbkę `DialogFragment` implementacji oraz omówionych `IOnTimeSetListener` interfejsu. W tym przykładzie również pokazano sposób, w jaki `DialogFragment` mogą wchodzić w interakcje z hostem, aby wyświetlić wybrana wartość czasu.


## <a name="related-links"></a>Linki pokrewne

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (przykład)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
