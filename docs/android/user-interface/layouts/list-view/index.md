---
title: ListView
description: Element ListView jest ważny składnik interfejsu użytkownika aplikacji systemu Android; Służy wszędzie z krótkim list opcji menu długie listy kontaktów lub internet Ulubione. Zapewnia prosty sposób do przedstawienia przewijanej listy wierszy, które mogą być sformatowany przy użyciu wbudowanych stylu lub dostosować często.
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2018
ms.openlocfilehash: 8499b9f186c12df22518893b6677cab22f0a3568
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="listview"></a>ListView

_Element ListView jest ważny składnik interfejsu użytkownika aplikacji systemu Android; Służy wszędzie z krótkim list opcji menu długie listy kontaktów lub internet Ulubione. Zapewnia prosty sposób do przedstawienia przewijanej listy wierszy, które mogą być sformatowany przy użyciu wbudowanych stylu lub dostosować często._


## <a name="overview"></a>Omówienie

Widoki list i karty znajdują się w najbardziej podstawowymi blokami konstrukcyjnymi aplikacji systemu Android. `ListView` Klasa umożliwia elastyczne obecnych danych krótkie menu lub przewijanego listy. Zapewnia funkcje użyteczność, takie jak szybkiego przewijania, indeksy i wybór jednego lub wielu ułatwiające tworzenie interfejsów użytkownika przyjazna dla aplikacji. A `ListView` wymaga wystąpienia *karty* ze źródłem go z danymi zawartymi w widokach wiersza.

Ten przewodnik opisuje sposób nadawania `ListView` i różnych `Adapter` klasy platformie Xamarin.Android. On również pokazano, jak dostosować wygląd `ListView`, i omówiono jej znaczenie wiersza ponownie użyj zmniejszyć zużycie pamięci. Dostępne są także niektóre omówienie wpływ cyklu życia działania `ListView` i `Adapter` użycia. Jeśli pracujesz nad wieloplatformowych aplikacji za pomocą platformy Xamarin.iOS, `ListView` kontroli przypomina strukturę iOS `UITableView` (i Android `Adapter` jest podobny do `UITableViewSource`).

Po pierwsze, krótki samouczek przedstawia `ListView` z przykładowego kodu podstawowe. Następnie ułatwiają znajdują się łącza do bardziej zaawansowanych tematów dotyczących `ListView` w rzeczywistych aplikacjach.


> [!NOTE]
> `RecyclerView` Widget jest bardziej zaawansowane i elastyczne wersji `ListView`. Ponieważ `RecyclerView` została zaprojektowana jako zastępuje `ListView` (i `GridView`), zaleca się, że używasz `RecyclerView` zamiast `ListView` do tworzenia nowej aplikacji. Aby uzyskać więcej informacji, zobacz [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).



## <a name="listview-tutorial"></a>ListView — samouczek

[`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) jest [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) który tworzy listę elementów przewijanego. Elementy listy są automatycznie dodaje do listy przy użyciu [ `IListAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.IListAdapter/).

W tym samouczku utworzysz listę o nazwy kraju, które są odczytywane z tablicy ciągów. Po wybraniu pozycji listy wyskakujące zostanie wyświetlony komunikat pozycji elementu na liście.


Utwórz nowy projekt o nazwie **HelloListView**.

Utwórz plik XML o nazwie **list_item.xml** i zapisać go w programie **zasobów/układ/** folderu. Wstaw następujące czynności:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

Ten plik Określa układ dla każdego elementu, który zostanie umieszczony w [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).

Otwórz `MainActivity.cs` i modyfikować klasy do rozszerzania [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/) (zamiast [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)):

```csharp
public class MainActivity : ListActivity
{
```

Wstaw następujący kod do [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) metody:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);

    ListView.TextFilterEnabled = true;

    ListView.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args)
    {
        Toast.MakeText(Application, ((TextView)args.View).Text, ToastLength.Short).Show();
    };
}
```

Powiadomienie tego działania nie zostanie załadowany plik układu (co zrobić z [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32))).
Zamiast tego ustawienia [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/) automatycznie dodaje właściwość [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) do wypełnienia cały ekran [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/).
Ta metoda przyjmuje [ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/), która zarządza tablicę elementów listy, które zostaną umieszczone w [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).
[ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/) Konstruktor pobiera aplikacji [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/), opis układu dla każdego elementu listy (utworzonego w poprzednim kroku), a `T[]` lub [ `Java.Util.IList<T>` ](https://developer.xamarin.com/api/type/Java.Util.IList/) tablicę obiektów do wstawienia w [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) (zdefiniowany dalej).

[ `TextFilterEnabled` ](https://developer.xamarin.com/api/property/Android.Widget.AbsListView.TextFilterEnabled/) Właściwości powoduje włączenie filtrowania dla tekstu [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/), tak aby po rozpoczęciu wpisywania użytkownika będą filtrowane listy.

[ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) Zdarzeń może służyć do subskrybowania obsługi kliknięcia. Gdy element [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) jest kliknięciu wywołuje program obsługi i [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) zostanie wyświetlony komunikat przy użyciu tekstu z kliknięty element.

Można użyć listy projektów elementu dostarczone przez platformę, zamiast definiować własne pliku układu dla [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).
Na przykład, spróbuj użyć `Android.Resource.Layout.SimpleListItem1` zamiast `Resource.Layout.list_item`.

Dodaj następujące `using` instrukcji:

```csharp
using System;
```
Następnie dodaj poniższe tablicy ciągów jako element członkowski `MainActivity`:

```csharp
static readonly string[] countries = new String[] {
    "Afghanistan","Albania","Algeria","American Samoa","Andorra",
    "Angola","Anguilla","Antarctica","Antigua and Barbuda","Argentina",
    "Armenia","Aruba","Australia","Austria","Azerbaijan",
    "Bahrain","Bangladesh","Barbados","Belarus","Belgium",
    "Belize","Benin","Bermuda","Bhutan","Bolivia",
    "Bosnia and Herzegovina","Botswana","Bouvet Island","Brazil","British Indian Ocean Territory",
    "British Virgin Islands","Brunei","Bulgaria","Burkina Faso","Burundi",
    "Cote d'Ivoire","Cambodia","Cameroon","Canada","Cape Verde",
    "Cayman Islands","Central African Republic","Chad","Chile","China",
    "Christmas Island","Cocos (Keeling) Islands","Colombia","Comoros","Congo",
    "Cook Islands","Costa Rica","Croatia","Cuba","Cyprus","Czech Republic",
    "Democratic Republic of the Congo","Denmark","Djibouti","Dominica","Dominican Republic",
    "East Timor","Ecuador","Egypt","El Salvador","Equatorial Guinea","Eritrea",
    "Estonia","Ethiopia","Faeroe Islands","Falkland Islands","Fiji","Finland",
    "Former Yugoslav Republic of Macedonia","France","French Guiana","French Polynesia",
    "French Southern Territories","Gabon","Georgia","Germany","Ghana","Gibraltar",
    "Greece","Greenland","Grenada","Guadeloupe","Guam","Guatemala","Guinea","Guinea-Bissau",
    "Guyana","Haiti","Heard Island and McDonald Islands","Honduras","Hong Kong","Hungary",
    "Iceland","India","Indonesia","Iran","Iraq","Ireland","Israel","Italy","Jamaica",
    "Japan","Jordan","Kazakhstan","Kenya","Kiribati","Kuwait","Kyrgyzstan","Laos",
    "Latvia","Lebanon","Lesotho","Liberia","Libya","Liechtenstein","Lithuania","Luxembourg",
    "Macau","Madagascar","Malawi","Malaysia","Maldives","Mali","Malta","Marshall Islands",
    "Martinique","Mauritania","Mauritius","Mayotte","Mexico","Micronesia","Moldova",
    "Monaco","Mongolia","Montserrat","Morocco","Mozambique","Myanmar","Namibia",
    "Nauru","Nepal","Netherlands","Netherlands Antilles","New Caledonia","New Zealand",
    "Nicaragua","Niger","Nigeria","Niue","Norfolk Island","North Korea","Northern Marianas",
    "Norway","Oman","Pakistan","Palau","Panama","Papua New Guinea","Paraguay","Peru",
    "Philippines","Pitcairn Islands","Poland","Portugal","Puerto Rico","Qatar",
    "Reunion","Romania","Russia","Rwanda","Sqo Tome and Principe","Saint Helena",
    "Saint Kitts and Nevis","Saint Lucia","Saint Pierre and Miquelon",
    "Saint Vincent and the Grenadines","Samoa","San Marino","Saudi Arabia","Senegal",
    "Seychelles","Sierra Leone","Singapore","Slovakia","Slovenia","Solomon Islands",
    "Somalia","South Africa","South Georgia and the South Sandwich Islands","South Korea",
    "Spain","Sri Lanka","Sudan","Suriname","Svalbard and Jan Mayen","Swaziland","Sweden",
    "Switzerland","Syria","Taiwan","Tajikistan","Tanzania","Thailand","The Bahamas",
    "The Gambia","Togo","Tokelau","Tonga","Trinidad and Tobago","Tunisia","Turkey",
    "Turkmenistan","Turks and Caicos Islands","Tuvalu","Virgin Islands","Uganda",
    "Ukraine","United Arab Emirates","United Kingdom",
    "United States","United States Minor Outlying Islands","Uruguay","Uzbekistan",
    "Vanuatu","Vatican City","Venezuela","Vietnam","Wallis and Futuna","Western Sahara",
    "Yemen","Yugoslavia","Zambia","Zimbabwe"
  };
```

To jest tablica ciągów, które zostaną umieszczone w [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).

Uruchom aplikację. Możesz przewijać listę, lub wpisz, aby filtrować je, a następnie kliknij element, aby wyświetlić komunikat. Powinny pojawić się dane podobne do poniższych:

[![Zrzut ekranu elementu ListView z nazwami kraju](images/01-listview-example-sml.png)](images/01-listview-example.png#lightbox)

Należy pamiętać, że przy użyciu tablicy ciągów ustalony nie jest najlepszym rozwiązaniem projektu. Co najmniej jedna jest używana w tym samouczku dla uproszczenia, aby zademonstrować [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) elementu widget. Lepsze rozwiązaniem jest odwołanie tablicy ciągów, zdefiniowany przez zasób zewnętrzny, takich jak z `string-array` zasobów w projekcie **Resources/Values/Strings.xml** pliku. Na przykład:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloListView</string>
  <string-array name="countries_array">
    <item>Bahrain</item>
    <item>Bangladesh</item>
    <item>Barbados</item>
    <item>Belarus</item>
    <item>Belgium</item>
    <item>Belize</item>
    <item>Benin</item>
  </string-array>
</resources>
```

Aby użyć tych ciągów zasobów dla [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/), Zastąp oryginalny [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/) wiersz z następujących:

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```
Uruchom aplikację. Powinny pojawić się dane podobne do poniższych:

[![Przykład zrzut ekranu przedstawiający element ListView, z mniejszym listę nazw](images/02-smaller-example-sml.png)](images/02-smaller-example.png#lightbox)


## <a name="going-further-with-listview"></a>Kontynuacja z widoku listy.

Pozostałe tematy (połączony poniżej) Przyjrzyjmy się kompleksowe Praca z `ListView` klasy i różne typy karty typów można używać z nią. Struktura jest następująca:

-   **Wygląd** &ndash; części `ListView` kontroli i jak działają.

-   **Klasy** &ndash; Przegląd klas używany do wyświetlania `ListView`.

-   **Wyświetlanie danych w elemencie ListView** &ndash; sposób wyświetlania prosta lista danych; implementowania `ListView's` użyteczność funkcji; sposób użycia innego wiersza wbudowanych układów; i jak zapisać kart pamięci przez ponowne korzystanie z widoków wiersza.

-   **Niestandardowe wygląd** &ndash; Zmienianie stylu `ListView` układy niestandardowe, czcionki i kolory.

-   **Przy użyciu systemu SQLite** &ndash; sposób wyświetlania danych z bazy danych SQLite z `CursorAdapter`.

-   **Cykl życia działania** &ndash; zagadnienia dotyczące projektu podczas implementowania `ListView` działania, w tym, gdzie w cyklu życia należy wypełnić danych i kiedy należy zwolnić zasoby.

Omówienie (podzielone na sześć części) rozpoczyna się od omówienie `ListView` klasy się przed ich wprowadzeniem stopniowo bardziej złożonych przykłady sposobu jego używania.

-   [Elementy i funkcje obiektu ListView](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
-   [Wypełnianie ListView z danymi](~/android/user-interface/layouts/list-view/populating.md)
-   [Dostosowywanie wyglądu obiektu ListView](~/android/user-interface/layouts/list-view/customizing-appearance.md)
-   [Używanie obiektu CursorAdapters](~/android/user-interface/layouts/list-view/cursor-adapters.md)
-   [Używanie obiektu ContentProvider](~/android/user-interface/layouts/list-view/content-provider.md)
-   [Obiekt ListView i cyklem życia aktywności](~/android/user-interface/layouts/list-view/activity-lifecycle.md)


## <a name="summary"></a>Podsumowanie

Ten zestaw tematów wprowadzone `ListView` i przykłady dotyczące używania wbudowanych funkcji `ListActivity`. Jego opisem implementacji niestandardowych `ListView` dozwolone w przypadku układów kolorowe i korzystania z bazy danych SQLite i krótko dotknięciu na przydatność życia działania Twojej `ListView` implementacji.


## <a name="related-links"></a>Linki pokrewne

- [AccessoryViews (przykład)](https://developer.xamarin.com/samples/AccessoryViews/)
- [BasicTableAndroid (przykład)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (przykład)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [BuiltInViews (przykład)](https://developer.xamarin.com/samples/BuiltInViews/)
- [CustomRowView (przykład)](https://developer.xamarin.com/samples/CustomRowView/)
- [FastScroll (przykład)](https://developer.xamarin.com/samples/FastScroll/)
- [SectionIndex (przykład)](https://developer.xamarin.com/samples/SectionIndex/)
- [SimpleCursorTableAdapter (przykład)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (przykład)](https://developer.xamarin.com/samples/CursorTableAdapter/)
- [Samouczek cyklu życia działania](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Praca z tabelami i komórek (w Xamarin.iOS)](~/ios/user-interface/controls/tables/index.md)
- [Odwołania do klasy ListView](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [Odwołania do klasy ListActivity](https://developer.xamarin.com/api/type/Android.App.ListActivity/)
- [Odwołania do klasy BaseAdapter](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
- [Odwołania do klasy ArrayAdapter](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
- [Odwołania do klasy CursorAdapter](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
