---
title: Klasy fragmentu specjalne
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 7ddb4b7d4867813311448258bb4fb177ae4cd175
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="specialized-fragment-classes"></a>Klasy fragmentu specjalne

Interfejs API fragmenty zawiera inne podklas, które hermetyzują niektóre funkcje częściej można znaleźć w aplikacjach. Podklasy te są:

-   **ListFragment** &ndash; ten Fragment jest używana do wyświetlania listy elementów powiązanych ze źródłem danych, takich jak tablica lub kursora.

-   **DialogFragment** &ndash; fragmentu ten jest używany jako otokę okna dialogowego. Fragment wyświetli okno dialogowe na podstawie jego działania.

-   **PreferenceFragment** &ndash; ten Fragment służy do wyświetlenia preferencji obiekty w postaci listy.



## <a name="the-listfragment"></a>ListFragment

`ListFragment` Jest bardzo podobny do koncepcji i funkcji do `ListActivity`; jest otoki, który jest hostem `ListView` w fragmentu. Obraz poniżej przedstawia `ListFragment` uruchomiony na komputerze typu tablet i telefonu:

[![Zrzuty ekranu ListFragment na komputerze typu tablet i na telefonie](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)


### <a name="binding-data-with-the-listadapter"></a>Powiązanie danych z ListAdapter

`ListFragment` Klasa już udostępnia układ domyślny, więc nie jest konieczne w celu zastąpienia `OnCreateView` do wyświetlenia zawartości `ListFragment`. `ListView` Jest powiązany z danymi przy użyciu `ListAdapter` implementacji. W poniższym przykładzie pokazano, jak można to zrobić za pomocą prostego tablicy ciągów:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    string[] values = new[] { "Android", "iPhone", "WindowsMobile",
                "Blackberry", "WebOS", "Ubuntu", "Windows7", "Max OS X",
                "Linux", "OS/2" };
    this.ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleExpandableListItem1, values);
}
```

Podczas ustawiania `ListAdapter`, ważne jest, aby użyć `ListFragment.ListAdapter` właściwości, a nie `ListView.ListAdapter` właściwości. Przy użyciu `ListView.ListAdapter` spowoduje, że kod inicjujący ważne był pomijany.



### <a name="responding-to-user-selection"></a>Odpowiada na wybór użytkownika

Aby odpowiedzieć wybory użytkownika, aplikacja musi zastąpić `OnListItemClick` metody. Poniższy przykład przedstawia jedną z możliwości:

```csharp
public override void OnListItemClick(ListView l, View v, int index, long id)
{
    // We can display everything in place with fragments.
    // Have the list highlight this item and show the data.
    ListView.SetItemChecked(index, true);

    // Check what fragment is shown, replace if needed.
    var details = FragmentManager.FindFragmentById<DetailsFragment>(Resource.Id.details);
    if (details == null || details.ShownIndex != index)
    {
        // Make new fragment to show this selection.
        details = DetailsFragment.NewInstance(index);

        // Execute a transaction, replacing any existing
        // fragment with this one inside the frame.
        var ft = FragmentManager.BeginTransaction();
        ft.Replace(Resource.Id.details, details);
        ft.SetTransition(FragmentTransit.FragmentFade);
        ft.Commit();
    }
}
```

W kodzie powyżej, gdy użytkownik wybierze element `ListFragment`, nowy Fragment jest wyświetlany w działaniu hostingu, przedstawiający więcej szczegółów na temat elementu, który został wybrany.



## <a name="dialogfragment"></a>DialogFragment

*DialogFragment* to Fragment, który służy do wyświetlania obiektu okna dialogowego wewnątrz Fragment, który będzie przestawiany u góry okna działania. Temat ten może zastąpić okna dialogowego zarządzanych interfejsów API (począwszy od systemu Android 3.0). Poniższy zrzut ekranu przedstawia przykład `DialogFragment`:

[![Zrzut ekranu DialogFragment wyświetlanie Dodaj nowe pole Vehicle edycji](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

A `DialogFragment` gwarantuje, że stanu między fragmentu i okno dialogowe zachować spójność. Wszystkie interakcje i kontrolę nad obiektu okna dialogowego powinna wystąpić z powodu `DialogFragment` interfejsu API, a nie było możliwe z bezpośrednim odwołaniem do obiektu okna dialogowego. `DialogFragment` Interfejs API zawiera każde wystąpienie z `Show()` metodę, która jest używana do wyświetlania fragmentu. Istnieją dwa sposoby pozbyć się Fragment:

-  Wywołanie `DialogFragment.Dismiss()` na `DialogFragment` wystąpienia. 

-  Wyświetlanie innego `DialogFragment`.

Aby utworzyć `DialogFragment`, dziedziczy po klasie `Android.App.DialogFragment,` , a następnie zastępuje jedną z następujących dwóch metod:

- **OnCreateView** &ndash; to tworzy i zwraca widok.

- **OnCreateDialog** &ndash; spowoduje to utworzenie niestandardowego okna dialogowego. Zwykle jest używana do wyświetlenia *AlertDialog*. W przypadku przesłaniania tej metody, nie jest konieczne zastąpić `OnCreateView` .



### <a name="a-simple-dialogfragment"></a>Proste DialogFragment

Poniższy zrzut ekranu przedstawia prostą `DialogFragment` mający `TextView` i dwa `Button`s:

[![Przykład DialogFragment element TextView i dwóch przycisków](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

`TextView` Wyświetli ile razy użytkownik kliknął przycisk jeden `DialogFragment`, a kliknięcie przycisku innych zamknie Fragment. Kod `DialogFragment` jest:

```csharp
public class MyDialogFragment : DialogFragment
{
    private int _clickCount;
    public override void OnCreate(Bundle savedInstanceState)
    {
        _clickCount = 0;
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState)
        
        var view = inflater.Inflate(Resource.Layout.dialog_fragment_layout, container, false);
        var textView = view.FindViewById<TextView>(Resource.Id.dialog_text_view);
            
        view.FindViewById<Button>(Resource.Id.dialog_button).Click += delegate
            {
                textView.Text = "You clicked the button " + _clickCount++ + " times.";
            };

        // Set up a handler to dismiss this DialogFragment when this button is clicked.
        view.FindViewById<Button>(Resource.Id.dismiss_dialog_button).Click += (sender, args) => Dismiss();
        return view;
        }
    }
}
```


### <a name="displaying-a-fragment"></a>Wyświetlanie fragmentu

Wszystkie fragmenty, takich jak `DialogFragment` jest wyświetlany w kontekście `FragmentTransaction`.

`Show()` Metoda `DialogFragment` przyjmuje `FragmentTransaction` i `string` jako danych wejściowych. Okno dialogowe zostanie dodany do działania, a `FragmentTransaction` zatwierdzone.

Poniższy kod przedstawia jedną z możliwych metod działania mogą używać `Show()` metodę, aby wyświetlić `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```


### <a name="dismissing-a-fragment"></a>Odrzucanie fragmentu

Wywoływanie `Dismiss()` na wystąpienie `DialogFragment` powoduje fragmentu były usuwane z działania i zatwierdza tej transakcji.
Standardowe metody cyklu życia fragmentu związane z zniszczenie Fragment zostanie wywołana.


### <a name="alert-dialog"></a>Okna dialogowego alertu

Zamiast przesłaniać metodę `OnCreateView`, `DialogFragment` zamiast tego mogą zastępować `OnCreateDialog`. Umożliwia to aplikacji do tworzenia [AlertDialog](https://developer.xamarin.com/api/type/Android.App.AlertDialog/) zarządzanym przez fragmentu. Następujący kod jest przykładem, który używa `AlertDialog.Builder` utworzyć `Dialog`:

```csharp
public class AlertDialogFragment : DialogFragment
{
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        EventHandler<DialogClickEventArgs> okhandler;
        var builder = new AlertDialog.Builder(Activity)
            .SetMessage("This is my dialog.")
            .SetPositiveButton("Ok", (sender, args) =>
                                         {
                                             // Do something when this button is clicked.
                                         })
            .SetTitle("Custom Dialog");
        return builder.Create();
    }
}
```



## <a name="preferencefragment"></a>PreferenceFragment

Aby ułatwić zarządzanie preferencje, zapewnia API fragmenty `PreferenceFragment` podklasy. `PreferenceFragment` Jest podobny do [PreferenceActivity](https://developer.xamarin.com/api/type/Android.Preferences.PreferenceActivity/) &ndash; hierarchii preferencji użytkownika będzie widoczny na fragmentu. Jako użytkownik wchodzi w interakcję z preferencji, zostaną one automatycznie zapisane do [SharedPreferences](http://developer.android.com/reference/android/content/SharedPreferences.html).
W Android 3.0 lub nowszej aplikacji, użyj `PreferenceFragment` radzenia sobie z Preferencje w aplikacjach. Na poniższej ilustracji przedstawiono przykład `PreferenceFragment`:

[![Przykład PreferencesFragment z wbudowanego, okno dialogowe i preferencje uruchamiania](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)


### <a name="create-a-preference-fragment-from-a-resource"></a>Utwórz Fragment preferencji z zasobu

Preferencje Fragment może zwiększony z pliku XML zasobów za pomocą [PreferenceFragment.AddPreferencesFromResource](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromResource/p/System.Int32/) metody. Jest logiczną miejsca, aby wywołać tę metodę w cyklu życia fragmentu w `OnCreate` metody.

`PreferenceFragment` Pokazano powyżej został utworzony przez ładowanie zasobu z pliku XML. Plik zasobu jest:

```xml
<?xml version="1.0" encoding="utf-8"?>

<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android">

  <PreferenceCategory android:title="Inline Preferences">
    <CheckBoxPreference android:key="checkbox_preference"
                        android:title="Checkbox Preference Title"
                        android:summary="Checkbox Preference Summary" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Dialog Based Preferences">

    <EditTextPreference android:key="edittext_preference"
                        android:title="EditText Preference Title"
                        android:summary="EditText Preference Summary"
                        android:dialogTitle="Edit Text Preferrence Dialog Title" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Launch Preferences">

    <!-- This PreferenceScreen tag serves as a screen break (similar to page break
             in word processing). Like for other preference types, we assign a key
             here so it is able to save and restore its instance state. -->
    <PreferenceScreen android:key="screen_preference"
                      android:title="Title Screen Preferences"
                      android:summary="Summary Screen Preferences">

      <!-- You can place more preferences here that will be shown on the next screen. -->

      <CheckBoxPreference android:key="next_screen_checkbox_preference"
                          android:title="Next Screen Toggle Preference Title"
                          android:summary="Next Screen Toggle Preference Summary" />

    </PreferenceScreen>

    <PreferenceScreen android:title="Intent Preference Title"
                      android:summary="Intent Preference Summary">

      <intent android:action="android.intent.action.VIEW"
              android:data="http://www.android.com" />

    </PreferenceScreen>

  </PreferenceCategory>

</PreferenceScreen>
```

Kod preferencji Fragment jest następujący:

```csharp
public class PrefFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        AddPreferencesFromResource(Resource.Xml.preferences);
    }
}
```



### <a name="querying-activities-to-create-a-preference-fragment"></a>Wykonywanie zapytania działania w celu utworzenia fragmentu preferencji

Innej techniki tworzenia `PreferenceFragment` obejmuje badania działań. Każde działanie można użyć [METADANYCH\_klucza\_preferencji](https://developer.xamarin.com/api/field/Android.Preferences.PreferenceManager.MetadataKeyPreferences/) atrybut, który będzie wskazywać plik zasobu XML. Platformie Xamarin.Android, w tym celu należy adorning działania `MetaDataAttribute`, a następnie określając plik zasobu, który będzie używany. `PreferenceFragment` Klasa dostarcza metody [AddPreferenceFromIntent](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromIntent/p/Android.Content.Intent/)) który może służyć do badania działanie, aby znaleźć ten zasób XML i zwiększyć dla niego hierarchii preferencji.

Przykładem tego procesu jest udostępniany w poniższy fragment kodu, który używa `AddPreferencesFromIntent` utworzyć `PreferenceFragment`:

```csharp
public class MyPreferenceFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        var intent = new Intent(this.Activity, typeof (MyActivityWithPreferences));
        AddPreferencesFromIntent(intent);
    }
}
```

System android będzie wyglądać w klasie `MyActivityWithPreference`. Klasa musi być adorned z `MetaDataAttribute,` pokazane na poniższy fragment kodu:

```csharp
[Activity(Label = "My Activity with Preferences")]
[MetaData(PreferenceManager.MetadataKeyPreferences, Resource = "@xml/preference_from_intent")]
public class MyActivityWithPreferences : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        // This is deliberately blank
    }
}
```

`MetaDataAttribute` Deklaruje pliku zasobu XML, który `PreferenceFragment` umożliwia zwiększyć hierarchii preferencji. Jeśli `MetatDataAttribute` nie zostanie podany, a następnie zostanie wygenerowany wyjątek w czasie wykonywania. Po uruchomieniu tego kodu `PreferenceFragment` zostanie wyświetlone w poniższy zrzut ekranu:

[![Zrzut ekranu przedstawiający przykładową aplikację z PreferenceFragment wyświetlany](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
