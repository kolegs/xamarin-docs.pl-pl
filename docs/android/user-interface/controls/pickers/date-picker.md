---
title: "Wybór daty"
description: "Wybranie daty kalendarza przy użyciu DatePickerDialog i DialogFragment"
ms.topic: article
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 3de935fd407524d7ba62a93205e333c7dd7adde0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="date-picker"></a>Wybór daty

## <a name="overview"></a>Omówienie

Brak sytuacji, gdy użytkownik musi wprowadzić dane do aplikacji systemu Android. Ułatwiających z tym platformy systemu Android udostępnia [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/) elementu widget i [ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/) . `DatePicker` Umożliwia użytkownikowi wybranie rok, miesiąc i dzień w spójny interfejs między urządzeniami i aplikacjami. `DatePickerDialog` Jest klasa pomocy, która hermetyzuje `DatePicker` w oknie dialogowym.

Powinien być wyświetlany w nowoczesnych aplikacji systemu Android `DatePickerDialog` w [ `DialogFragment` ](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). To umożliwi aplikacji do wyświetlenia jako podręcznego okna dialogowego selektora daty lub osadzone w działaniu. Ponadto `DialogFragment` będzie Zarządzanie cyklem życia i wyświetlanie okna dialogowego, zmniejsza ilość kodu, który musi zostać wdrożone.

W tym przewodniku przedstawiono sposób użycia `DatePickerDialog`, opakowaną w `DialogFragment`. Przykładowa aplikacja wyświetli `DatePickerDialog` jako modalnego okna dialogowego, gdy użytkownik kliknie przycisk w działaniu. Gdy daty jest ustawiony przez użytkownika, `TextView` zaktualizuje wszystkie informacje o dacie, które zostały wybrane.

[![Zrzut ekranu wybierz datę przycisk następuje okna dialogowego wyboru daty](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png)

## <a name="requirements"></a>Wymagania

Przykładowa aplikacja dla tego przewodnika jest przeznaczony dla systemu Android 4.1 (poziom interfejsu API
16) lub nowszej, ale ma zastosowanie do 3.0 dla systemu Android (poziom interfejsu API 11 lub nowszy). Istnieje możliwość obsługi starszych wersji systemu android, z uwzględnieniem wersji 4 obsługuje biblioteki systemu Android do projektu i niektóre zmiany kodu.

## <a name="using-the-datepicker"></a>Za pomocą selektora daty

W tym przykładzie rozszerzy `DialogFragment`. Podklasy hostującego i wyświetlić `DatePickerDialog`:

![Okno dialogowe wyboru Closeup daty](date-picker-images/image-02.png)

Gdy użytkownik zaznaczy datę i klika pozycję **OK** przycisku `DatePickerDialog` wywoła metodę [ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/).
Ten interfejs jest implementowany przez hostingu `DialogFragment`. Gdy użytkownik kliknie **anulować** przycisk Zignoruj fragment i okno dialogowe będzie się.

Istnieje kilka sposobów `DialogFragment` można wrócić wybranej daty do hostingu działania:

1. **Wywoływanie metody lub ustaw właściwość** &ndash; działania zapewniają właściwości lub metody specjalnie z myślą o ustawienie tej wartości.

2. **Wywołaj zdarzenie** &ndash; `DialogFragment` można zdefiniować zdarzenie, które będą wywoływane, gdy `OnDateSet` jest wywoływana.

3. **Użyj `Action`**  &ndash; `DialogFragment` może wywołać `Action<DateTime>` Aby wyświetlić datę w działaniu. Działanie będzie udostępniać `Action<DateTime` podczas tworzenia wystąpienia `DialogFragment`. W tym przykładzie będzie używane techniki trzeci i wymagają, aby podać działania `Action<DateTime>` do `DialogFragment`.


<a name="extending_dialogfragment" />

### <a name="extending-dialogfragment"></a>Rozszerzanie DialogFragment

Pierwszym krokiem przy wyświetlaniu `DatePickerDialog` jest podklasą `DialogFragment` i jego wdrożenie `IOnDateSetListener` interfejsu:

```csharp
public class DatePickerFragment : DialogFragment, 
                                  DatePickerDialog.IOnDateSetListener
{
    // TAG can be any string of your choice.
    public static readonly string TAG = "X:" + typeof (DatePickerFragment).Name.ToUpper();
    
    // Initialize this value to prevent NullReferenceExceptions.
    Action<DateTime> _dateSelectedHandler = delegate { };
    
    public static DatePickerFragment NewInstance(Action<DateTime> onDateSelected)
    {
        DatePickerFragment frag = new DatePickerFragment();
        frag._dateSelectedHandler = onDateSelected;
        return frag;
    }
    
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        DateTime currently = DateTime.Now;
        DatePickerDialog dialog = new DatePickerDialog(Activity, 
                                                       this, 
                                                       currently.Year, 
                                                       currently.Month - 1,
                                                       currently.Day);
        return dialog;
    }
    
    public void OnDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth)
    {
        // Note: monthOfYear is a value between 0 and 11, not 1 and 12!
        DateTime selectedDate = new DateTime(year, monthOfYear + 1, dayOfMonth);
        Log.Debug(TAG, selectedDate.ToLongDateString());
        _dateSelectedHandler(selectedDate);
    }
}
```

`NewInstance` — Metoda wywoływana w celu utworzenia wystąpienia nowy `DatePickerFragment`. Ta metoda przyjmuje `Action<DateTime>` który zostanie wywołany, gdy użytkownik kliknie **OK** przycisk `DatePickerDialog`.

Gdy fragment ma być wyświetlany, Android wywoła metodę `OnCreateDialog`. Ta metoda tworzy nowy `DatePickerDialog` i zainicjować go z bieżącą datą i obiektu wywołania zwrotnego (czyli bieżące wystąpienie klasy `DatePickerFragment`).


> [!NOTE]
> **Uwaga:** należy pamiętać, że wartość miesiąca po `IOnDateSetListener.OnDateSet` jest wywoływany jest z zakresu od 0 do 11, a nie od 1 do 12. Dzień miesiąca będzie w zakresie od 1 do 31 (w zależności od tego, który został wybrany miesiąc).


<a name="date_picker_fragment" />

### <a name="showing-the-datepickerfragment"></a>Wyświetlanie DatePickerFragment

Teraz, gdy `DialogFragment` został zaimplementowany, w tej sekcji zbada sposób użycia fragment w działaniu. W przykładowej aplikacji, który towarzyszy w tym przewodniku, działanie zostanie utworzenie wystąpienia `DialogFragment` przy użyciu `NewInstance` metoda fabryki i następnie wyświetlić ją wywołać `DialogFragment.Show`. W ramach tworzenia wystąpienia `DialogFragment`, przekazuje działania `Action<DateTime>`, która będzie wyświetlana data w `TextView` jest hostowana przez działanie:

```csharp
[Activity(Label = "@string/app_name", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
    TextView _dateDisplay;
    Button _dateSelectButton;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);

        _dateDisplay = FindViewById<TextView>(Resource.Id.date_display);
        _dateSelectButton = FindViewById<Button>(Resource.Id.date_select_button);
        _dateSelectButton.Click += DateSelect_OnClick;
    }

    void DateSelect_OnClick(object sender, EventArgs eventArgs)
    {
        DatePickerFragment frag = DatePickerFragment.NewInstance(delegate(DateTime time)
                                                                 {
                                                                     _dateDisplay.Text = time.ToLongDateString();
                                                                 });
        frag.Show(FragmentManager, DatePickerFragment.TAG);
    }
}
```

<a name="summary" />

## <a name="summary"></a>Podsumowanie

W tym przykładzie omówiono sposób wyświetlania `DatePicker` elementu widget jako menu podręczne modalnego okna dialogowego jako część działanie systemu Android. Podano przykładowe zastosowanie DialogFragment i opisano `IOnDateSetListener` interfejsu. W tym przykładzie przedstawiono również DialogFragment może interakcji z hostem, aby wyświetlić wybraną datą.


## <a name="related-links"></a>Linki pokrewne

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [Wybierz datę](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/android/controls/datepicker/select_a_date)
