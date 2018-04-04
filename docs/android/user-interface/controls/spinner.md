---
title: pokrętło
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 039c3f3a177d62a43679facba821675c6d716a91
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="spinner"></a>pokrętło

[`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) jest element widget prezentuje listy rozwijanej wyboru elementów. W tym przewodniku objaśniono sposób tworzenia prostej aplikacji powoduje wyświetlenie listy dostępnych opcji w pokrętła, następuje modyfikacje innych wartości skojarzonych z wybrane do wyświetlenia.

## <a name="basic-spinner"></a>Podstawowe pokrętła.

W pierwszej części tego samouczka utworzysz widżet proste pokrętła, który wyświetla listę planety. Po wybraniu planety wyskakujące zostanie wyświetlony komunikat z wybranego elementu:

[![Przykład zrzuty ekranu aplikacji HelloSpinner](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

Utwórz nowy projekt o nazwie **HelloSpinner**.

Otwórz **Resources/Layout/Main.axml** i Wstaw następujący kod XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:padding="10dip"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dip"
        android:text="@string/planet_prompt"
    />
    <Spinner
        android:id="@+id/spinner"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:prompt="@string/planet_prompt"
    />
</LinearLayout>
```

Zwróć uwagę, że [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/)w `android:text` atrybutu i [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)w `android:prompt` atrybutu zarówno odwoływać się do tego samego zasobu ciągu. Ten tekst zachowuje się jak tytuł elementu widget. Gdy jest stosowany do [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/), tekst tytułu będą wyświetlane w oknie dialogowym wyboru, która pojawia się po wybraniu elementu widget.

Edytuj **Resources/Values/Strings.xml** i zmodyfikuj plik, który będzie wyglądać następująco:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloSpinner</string>
  <string name="planet_prompt">Choose a planet</string>
  <string-array name="planets_array">
    <item>Mercury</item>
    <item>Venus</item>
    <item>Earth</item>
    <item>Mars</item>
    <item>Jupiter</item>
    <item>Saturn</item>
    <item>Uranus</item>
    <item>Neptune</item>
  </string-array>
</resources>
```

Drugi `<string>` element definiuje parametry tytuł odwołuje się [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) i [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) w układzie powyżej.
`<string-array>` Element definiuje listę ciągów, które będą wyświetlane jako lista w [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) elementu widget.

Teraz otworzyć **MainActivity.cs** i dodaj następującą `using` instrukcji:

```csharp
using System;
```

Następnie wstaw następujący kod do [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) metody:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    Spinner spinner = FindViewById<Spinner> (Resource.Id.spinner);

    spinner.ItemSelected += new EventHandler<AdapterView.ItemSelectedEventArgs> (spinner_ItemSelected);
    var adapter = ArrayAdapter.CreateFromResource (
            this, Resource.Array.planets_array, Android.Resource.Layout.SimpleSpinnerItem);

    adapter.SetDropDownViewResource (Android.Resource.Layout.SimpleSpinnerDropDownItem);
    spinner.Adapter = adapter;
}
```

Po `Main.axml` układ jest ustawiony jako widok zawartości [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) elementu widget są przechwytywane z Układem o [ `FindViewById<>(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
[ `CreateFromResource()` ](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.CreateFromResource/p/Android.Content.Context/System.Int32/System.Int32/) Następnie metoda tworzy nowy [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/), która wiąże każdego elementu w tablicy ciągów początkowej wygląd [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) (co ma wygląd każdego elementu w pokrętła zaznaczenie). `Resource.Array.planets_array` Odwołania do identyfikatorów `string-array` zdefiniowanych powyżej i `Android.Resource.Layout.SimpleSpinnerItem` identyfikator odwołuje się do układu wyglądu standardowe pokrętła, zdefiniowane przez platformę.
[`SetDropDownViewResource`](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.SetDropDownViewResource/p/System.Int32/) jest wywołana w celu określenia jej wyglądu dla każdego elementu, po otwarciu elementu widget. Na koniec [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) ustawiono skojarzyć wszystkie jego elementy z [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) przez ustawienie [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter) właściwości.

Teraz podaj metodę wywołania zwrotnego, która notifys aplikacji, gdy został wybrany element z [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/). Oto, co ta metoda powinien wyglądać następująco:

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

Po wybraniu elementu nadawcy jest rzutowane na [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) tak, aby elementy są dostępne. Przy użyciu `Position` właściwość `ItemEventArgs`, można sprawdzić tekstu zaznaczonego obiektu i umożliwia wyświetlanie [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/).

Uruchamianie aplikacji; go powinien wyglądać następująco:

[![Zrzut ekranu przykład pokrętło z Mars wybrany jako świecie](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>Pokrętła przy użyciu pary klucz wartość

Często jest konieczne użycie `Spinner` do wyświetlania wartości kluczy, które są skojarzone z określonego rodzaju dane używane przez aplikację. Ponieważ `Spinner` nie działa bezpośrednio z pary klucz wartość, należy przechowywać oddzielnie parę klucza i wartości, wypełnić `Spinner` wartości klucza, następnie za pomocą pozycji wybranego klucza w pokrętła wartość skojarzonych danych. 

W poniższych krokach **HelloSpinner** aplikacji są modyfikowane w celu wyświetlenia średnia temperatura wybranego świecie:

Dodaj następujące `using` instrukcji **MainActivity.cs**:

```csharp
using System.Collections.Generic;
```

Dodaj następujące zmienna wystąpienia na `MainActivity` klasy.
Ta lista będzie przechowywać pary klucz wartość dla planety i ich średnia temperatura:

```csharp
private List<KeyValuePair<string, string>> planets;
```

W `OnCreate` metody, Dodaj następujący kod przed `adapter` jest zadeklarowany jako:

```csharp
planets = new List<KeyValuePair<string, string>>
{
    new KeyValuePair<string, string>("Mercury", "167 degrees C"),
    new KeyValuePair<string, string>("Venus", "464 degrees C"),
    new KeyValuePair<string, string>("Earth", "15 degrees C"),
    new KeyValuePair<string, string>("Mars", "-65 degrees C"),
    new KeyValuePair<string, string>("Jupiter" , "-110 degrees C"),
    new KeyValuePair<string, string>("Saturn", "-140 degrees C"),
    new KeyValuePair<string, string>("Uranus", "-195 degrees C"),
    new KeyValuePair<string, string>("Neptune", "-200 degrees C")
};
```

Ten kod tworzy prosty magazyn planety i ich skojarzonych temperatury średniej. (W aplikacji rzeczywistych, bazy danych jest zwykle używany do przechowywania kluczy i ich skojarzonych danych.)

Bezpośrednio po kodzie powyżej Dodaj następujące wiersze do wyodrębniania kluczy i umieść je na listę (w kolejności):

```csharp
List<string> planetNames = new List<string>();
foreach (var item in planets)
    planetNames.Add (item.Key);
```

Tej listy, aby przekazać `ArrayAdapter` — Konstruktor (zamiast `planets_array` zasobów):

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

Modyfikowanie `spinner_ItemSelected` tak, aby wybrane pozycja jest używana do odszukania wartości (temperatury) skojarzonej z wybranych świecie:

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

Uruchamianie aplikacji; wyskakujące powinien wyglądać następująco:

[![Przykład zaznaczenia planety wyświetlanie temperatury](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)
   
  

## <a name="resources"></a>Zasoby

-   [`Resource.Layout`](https://developer.xamarin.com/api/type/Android.Resource+Layout/) 
-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) 
-   [`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) 

*Części tej stronie są zmiany na podstawie pracy utworzone i udostępnione przez Android Otwórz projekt źródłowy i używane zgodnie z warunki opisane w*
[*Creative Commons 2.5 autorstwa licencji* ](http://creativecommons.org/licenses/by/2.5/).
