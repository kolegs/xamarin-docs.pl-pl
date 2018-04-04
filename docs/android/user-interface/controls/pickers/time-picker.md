---
title: Selektor czasu
description: Wybieranie za TimePickerDialog i DialogFragment
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: c4261e3dccaccc4c88afe9c1033fb16b730fea6e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="time-picker"></a>Selektor czasu

Aby umożliwić użytkownikowi wybranie pojedynczego, można użyć [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/). Aplikacje dla systemu android jest zwykle używany `TimePicker` z [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/) wybierania wartość czasu &ndash; pomaga to zapewnić spójny interfejs między urządzeniami i aplikacjami. `TimePicker` Umożliwia użytkownikowi wybranie godzinę w 24-godzinnym lub 12-godzinnym trybie AM/PM.
`TimePickerDialog` Klasa pomocy, która hermetyzuje jest `TimePicker` w oknie dialogowym.

[![Zrzut ekranu okna dialogowego selektora czas działania](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>Omówienie

Wyświetl nowoczesnych aplikacji systemu Android `TimePickerDialog` w [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Dzięki temu aplikacja wyświetlić `TimePicker` jako okno podręczne lub ją osadzić w działaniu. Ponadto `DialogFragment` zarządzania cyklem życia i wyświetlanie okna dialogowego, zmniejsza ilość kodu, który musi zostać wdrożone.

W tym przewodniku przedstawiono sposób użycia `TimePickerDialog`, opakowaną w `DialogFragment`. Wyświetla aplikacji przykładowej `TimePickerDialog` jako modalnego okna dialogowego, gdy użytkownik kliknie przycisk w działaniu. Gdy czas jest ustawiony przez użytkownika, zamyka okno dialogowe i program obsługi aktualizacji `TextView` na ekranie działania o czasie, który został wybrany.

## <a name="requirements"></a>Wymagania

Przykładowa aplikacja dla tego przewodnika jest przeznaczony dla systemu Android 4.1 (poziom interfejsu API
16) lub nowszej, ale może być używany z systemem Android 3.0 (poziom interfejsu API 11 lub nowszy). Istnieje możliwość obsługi starszych wersji systemu android, z uwzględnieniem wersji 4 obsługuje biblioteki systemu Android do projektu i niektóre zmiany kodu.

## <a name="using-the-timepicker"></a>Przy użyciu TimePicker

W tym przykładzie rozszerza `DialogFragment`; wykonania podklasy `DialogFragment` (nazywane `TimePickerFragment` poniżej) obsługuje i wyświetla `TimePickerDialog`. Przykładowa aplikacja jest uruchamiana pierwszy raz, wyświetla **czas pobrania** powyżej przycisku `TextView` który będzie używany do wyświetlania zaznaczony czas:

[![Ekran aplikacji przykładowej początkowej](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Po kliknięciu **czas pobrania** przycisku spowoduje uruchomienie aplikacji przykład `TimePickerDialog` w tym zrzut ekranu:

[![Zrzut ekranu okna dialogowego selektora czas domyślny wyświetlany przez aplikację](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

W `TimePickerDialog`, wybierając czas i klikając **OK** przycisku powoduje, że `TimePickerDialog` można wywołać metody [IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/).
Ten interfejs jest implementowany przez hostingu `DialogFragment` (`TimePickerFragment`, które zostały opisane poniżej). Kliknięcie przycisku **anulować** kliknięcie przycisku powoduje fragment i okna dialogowego, aby być ukryty.

`DialogFragment` Zwraca zaznaczony czas do hostingu Actvity w jednym z trzech sposobów:

1. **Wywoływanie metody lub ustawienia właściwości** &ndash; działania zapewniają właściwości lub metody specjalnie z myślą o ustawienie tej wartości.

2. **Wywoływanie zdarzeń** &ndash; `DialogFragment` można zdefiniować zdarzenie, które będą wywoływane, gdy `OnTimeSet` jest wywoływana.

3. **Przy użyciu `Action`**  &ndash; `DialogFragment` może wywołać `Action<DateTime>` do wyświetlania czasu w działaniu. Działanie będzie udostępniać `Action<DateTime` podczas tworzenia wystąpienia `DialogFragment`. 

W tym przykładzie użyje trzecia technika, wymaga, aby dostaw działania `Action<DateTime>` program obsługi `DialogFragment`.



## <a name="start-an-app-project"></a>Projekt aplikacji

Utwórz nowy projekt dla systemu Android o nazwie **TimePickerDemo** (Jeśli nie masz doświadczenia w tworzeniu projektów platformy Xamarin.Android, zobacz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) informacje na temat tworzenia nowego projektu).

Edytuj **Resources/layout/Main.axml** i zastąp jego zawartość XML następujące:

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

Jest to podstawowy [LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) z [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/) która wyświetla czas i [przycisk](https://developer.xamarin.com/api/type/Android.Widget.Button/) który otwiera `TimePickerDialog`. Zwróć uwagę, czy ten układ używa ustalony ciągów i wymiarów, aby zapewnić aplikacji prostsze i łatwiejsze do zrozumienia &ndash; aplikacji produkcyjnej zwykle używa zasobów dla tych wartości (jak widać w [selektora daty](https://github.com/xamarin/recipes/blob/master/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) przykład kodu).

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

Podczas tworzenia i uruchomienia tego przykładu, powinien zostać wyświetlony ekran początkowy, podobnie jak na poniższym zrzucie ekranu:

[![Ekran początkowy aplikacji](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Kliknięcie przycisku **czas pobrania** przycisku nie robi nic, ponieważ `DialogFragment` nie została jeszcze zaimplementowana do wyświetlenia `TimePicker`.
Następnym krokiem jest utworzenie to `DialogFragment`.



## <a name="extending-dialogfragment"></a>Rozszerzanie DialogFragment

Aby rozszerzyć `DialogFragment` do użytku z `TimePicker`, należy utworzyć podklasy, która jest pochodną `DialogFragment` i implementuje `TimePickerDialog.IOnTimeSetListener`. Dodaj następujące klasy **MainActivity.cs**:

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

To `TimePickerFragment` klasy jest podzielony na mniejsze części i szczegółowo w następnej sekcji.


### <a name="dialogfragment-implementation"></a>Implementacja DialogFragment

`TimePickerFragment` implementuje metody kilka: metoda fabryki, metodę wystąpienia okna dialogowego i `OnTimeSet` wymagane przez metodę obsługi `TimePickerDialog.IOnTimeSetListener`.

-   `TimePickerFragment` jest podklasą `DialogFragment`. Implementuje również `TimePickerDialog.IOnTimeSetListener` interfejsu (to znaczy dostarcza wymagane `OnTimeSet` metody):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` zainicjowano do celów rejestracji (*MyTimePickerFragment* można zmienić do dowolnego ciągu ma być używany). `timeSelectedHandler` Akcja jest zainicjowana pusty delegatem, aby zapobiec wyjątki odwołanie o wartości null:

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   `NewInstance` Wywoływana jest metoda fabryki wystąpienia nowy `TimePickerFragment`. Ta metoda przyjmuje `Action<DateTime>` programu obsługi, które jest wywoływane, gdy użytkownik kliknie **OK** przycisk `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   Gdy fragment ma być wyświetlany, Android wywoła `DialogFragment` metody [OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/). 
    Ta metoda tworzy nowy `TimePickerDialog` obiektu i inicjuje go z działaniem obiektu wywołania zwrotnego (czyli bieżące wystąpienie klasy `TimePickerFragment`), a bieżącą godziną:

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

-   Gdy użytkownik zmieni ustawienie czasu `TimePicker` okna dialogowego, `OnTimeSet` wywołania metody. `OnTimeSet` Tworzy `DateTime` przy użyciu z bieżącą datą i scalenia w czasie (godzinowe i minutowe) wybrane przez użytkownika:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   To `DateTime` obiekt jest przekazywany do `timeSelectedHandler` zarejestrowaniu w usłudze `TimePickerFragment` obiektu w czasie tworzenia. `OnTimeSet` wywołuje ten program obsługi, aby zaktualizować wyświetlanie godziny działania zaznaczony czas (ten program obsługi jest zaimplementowana w następnej sekcji):

    ```csharp
    timeSelectedHandler (selectedTime);
    ```



## <a name="displaying-the-timepickerfragment"></a>Wyświetlanie TimePickerFragment

Teraz, gdy `DialogFragment` została zaimplementowana, to czas utworzenia wystąpienia `DialogFragment` przy użyciu `NewInstance` metoda fabryki i wyświetl ją za pomocą [DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

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

Po `TimeSelectOnClick` tworzy `TimePickerFragment`, tworzy i przekazuje delegat dla metody anonimowej, która aktualizuje działania czas wyświetlania o wartości przekazywane w czasie. Na koniec uruchamia `TimePicker` fragmentu okna dialogowego (za pośrednictwem `DialogFragment.Show`) do wyświetlenia `TimePicker` dla użytkownika.

Na koniec `OnCreate` metody, Dodaj następujący wiersz do dołączenia do programu obsługi zdarzeń **czas pobrania** przycisku uruchamiającego okna dialogowego:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

Gdy **czas pobrania** kliknięciu przycisku `TimeSelectOnClick` zostanie wywołany, aby wyświetlić `TimePicker` fragmentu okna dialogowego do użytkownika.



## <a name="try-it"></a>Wypróbuj!

Skompiluj i uruchom aplikację. Po kliknięciu **czas pobrania** przycisku `TimePickerDialog` jest wyświetlany w domyślnym formacie czas działania (w tym przypadku, 12-godzinnym AM/PM trybie):

[![Zostanie wyświetlone okno dialogowe czasu, w trybie AM/PM](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
Po kliknięciu **OK** w `TimePicker` okna dialogowego, program obsługi aktualizacji działania `TextView` z wybranym czasie, a następnie zamyka:

[![Czas A/M jest wyświetlany w TextView działania](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

Następnie dodaj następujący wiersz kodu w celu `OnCreateDialog` natychmiast po `is24HourFormat` został zadeklarowany i zainicjowana:

```csharp
is24HourFormat = true;
```

Ta zmiana wymusza flagi przekazywane do `TimePickerDialog` konstruktora, aby być `true` tak trybie 24-godzinnym jest używany zamiast format czasu działania hostingu. Podczas kompilacji, a następnie ponownie uruchom aplikację, kliknij przycisk **czas pobrania** przycisku `TimePicker` w formacie 24-godzinnym teraz zostanie wyświetlone okno dialogowe:

[![Okno dialogowe TimePicker w formacie 24-godzinnym](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

Ponieważ wywołuje program obsługi [DateTime.ToShortTimeString](https://msdn.microsoft.com/en-us/library/system.datetime.toshortdatestring%28v=vs.110%29.aspx) do drukowania czas działania `TextView`, czas nadal jest drukowany w domyślnym 12-godzinnym formacie AM/PM.



## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono sposób wyświetlania `TimePicker` elementu widget jako menu podręczne modalnego okna dialogowego z działanie systemu Android. Zapewniała próbkę `DialogFragment` implementacji i opisem `IOnTimeSetListener` interfejsu. W tym przykładzie również wykazać, jak `DialogFragment` mogą współdziałać z hostem, aby wyświetlić zaznaczony czas.


## <a name="related-links"></a>Linki pokrewne

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (przykład)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
