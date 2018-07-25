---
title: Wybór daty
description: Wybieranie przy użyciu DatePickerDialog i DialogFragment dat w kalendarzu
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 8f4e6318d904efb2f77c36732fc6519699f72ac9
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241139"
---
# <a name="date-picker"></a>Wybór daty

## <a name="overview"></a>Omówienie

Istnieją sytuacje, gdy użytkownik musi dane wejściowe do aplikacji systemu Android. Aby ułatwić to, zapewnia platformy systemu Android [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/) elementu widget oraz [ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/) . `DatePicker` Umożliwia użytkownikom wybranie rok, miesiąc i dzień w spójny interfejs między urządzeniami i aplikacjami. `DatePickerDialog` Jest pomocnikiem klasy, która hermetyzuje `DatePicker` w oknie dialogowym.

Nowoczesne aplikacje dla systemu Android powinien być wyświetlany `DatePickerDialog` w [ `DialogFragment` ](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). To umożliwi aplikacji do wyświetlenia DatePicker jako menu podręczne okno dialogowe lub osadzona w działaniu. Ponadto `DialogFragment` będą zarządzać cyklem życia i wyświetlanie okna dialogowego, co zmniejsza ilość kodu, które należy zaimplementować.

W przewodniku pokazano, jak używać `DatePickerDialog`otoczone `DialogFragment`. Przykładowa aplikacja zostanie wyświetlona `DatePickerDialog` jako modalne okno dialogowe, gdy użytkownik kliknie przycisk w ramach działania. Gdy data jest ustawiona przez użytkownika, `TextView` zostaną zaktualizowane o daty, który został wybrany.

[![Zrzut ekranu wyboru daty przycisk, a następnie okna dialogowego selektora daty](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>Wymagania

Przykładowa aplikacja dla tego przewodnika jest przeznaczony dla systemu Android 4.1 (poziom interfejsu API
16) lub nowszej, ale ma zastosowanie do wersji 3.0 dla systemu Android (poziom interfejsu API 11 lub nowszy). Istnieje możliwość do obsługi starszych wersji systemu android dodając v4 biblioteki obsługi systemu Android do projektu i niektórych zmian w kodzie.

## <a name="using-the-datepicker"></a>Za pomocą selektora daty

W tym przykładzie zostaną rozszerzone `DialogFragment`. Podklasy będzie hostować i wyświetlić `DatePickerDialog`:

![Okno dialogowe zbliżenie selektora daty](date-picker-images/image-02.png)

Gdy użytkownik wybiera wartość typu date i kliknie **OK** przycisku `DatePickerDialog` wywoła metodę [ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/).
Ten interfejs jest implementowany przez hostingu `DialogFragment`. Jeśli użytkownik kliknie **anulować** przycisk, a następnie fragmentu i okno dialogowe będzie odrzucać samodzielnie.

Istnieje kilka sposobów `DialogFragment` może zwrócić wybranej daty do hostingu działania:

1. **Wywoływanie metody lub ustawić właściwość** &ndash; działanie może zapewnić właściwość lub metodę specjalnie ustawienie tej wartości.

2. **Wywołać zdarzenie** &ndash; `DialogFragment` można zdefiniować zdarzenie, które będą podniesione, gdy `OnDateSet` jest wywoływana.

3. **Użyj `Action`**  &ndash; `DialogFragment` może wywołać `Action<DateTime>` Aby wyświetlić datę w działaniu. Działanie zapewni `Action<DateTime` podczas tworzenia wystąpienia `DialogFragment`. W tym przykładzie trzeci techniki używać i wymagają, że działanie podać `Action<DateTime>` do `DialogFragment`.



### <a name="extending-dialogfragment"></a>Rozszerzanie DialogFragment

Pierwszym krokiem podczas wyświetlania `DatePickerDialog` jest podklasą `DialogFragment` potem z łatwością wdrożyć `IOnDateSetListener` interfejsu:

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

`NewInstance` Metoda jest wywoływana w celu utworzenia wystąpienia nowego `DatePickerFragment`. Ta metoda przyjmuje `Action<DateTime>` , zostanie wywołany, gdy użytkownik kliknie **OK** znajdujący się w `DatePickerDialog`.

Gdy fragment ma być wyświetlana, systemu Android będzie wywoływać metodę `OnCreateDialog`. Ta metoda zostanie utworzony nowy `DatePickerDialog` obiektu i zainicjować go z bieżącą datą i obiektu wywołania zwrotnego (czyli bieżące wystąpienie `DatePickerFragment`).


> [!NOTE]
> Należy pamiętać, że wartość miesiąca po `IOnDateSetListener.OnDateSet` jest wywoływany jest z zakresu od 0 do 11, a nie od 1 do 12. Dzień miesiąca, będą w zakresie od 1 do 31 (w zależności od tego, który został wybrany miesiąc).



### <a name="showing-the-datepickerfragment"></a>Wyświetlanie DatePickerFragment

Teraz, gdy `DialogFragment` został zaimplementowany, w tej sekcji omówiono sposób użycia tego fragmentu w działaniu. Przykładowa aplikacja znajdująca się na tym przewodniku zostanie wystąpienia działania `DialogFragment` przy użyciu `NewInstance` metoda fabryki, a następnie wyświetl ją wywołać `DialogFragment.Show`. W ramach tworzenia wystąpienia `DialogFragment`, przekazuje działania `Action<DateTime>`, co spowoduje wyświetlenie daty w `TextView` hostowaną przez działanie:

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


## <a name="summary"></a>Podsumowanie

W tym przykładzie omówiono sposób wyświetlania `DatePicker` widżet jako menu podręczne modalne okno dialogowe jako część działanie systemu Android. Podany przykład implementacji DialogFragment oraz omówionych `IOnDateSetListener` interfejsu. Również w tym przykładzie pokazano, jak DialogFragment mogą wchodzić w interakcje z hostem, aby wyświetlić wybraną datą.


## <a name="related-links"></a>Linki pokrewne

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [Wybierz datę](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
