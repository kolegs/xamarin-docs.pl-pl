---
title: Automatyczne uzupełnianie
ms.prod: xamarin
ms.assetid: D4C8CA49-8369-35B7-798D-B147FDC24185
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 333264e65e814ad73eabeb7be55abb320400a59a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764453"
---
# <a name="auto-complete"></a>Automatyczne uzupełnianie


## <a name="overview"></a>Omówienie

Aby utworzyć widżet wpis tekst, który zawiera sugestie funkcja automatycznego uzupełniania, użyj [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) elementu widget. Sugestie są odbierane z kolekcji ciągów skojarzony z elementem widget za pośrednictwem [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/).

W tym samouczku utworzysz [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) elementu widget, który zawiera sugestie dotyczące nazwy kraju.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

[ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) Jest etykietę, którą wprowadzono [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) elementu widget.


## <a name="tutorial"></a>Samouczek

Utwórz nowy projekt o nazwie *HelloAutoComplete*.

Utwórz plik XML o nazwie `list_item.xml` i zapisać go w programie **zasobów/układ** folderu. Ustawić akcji kompilacji tego pliku do `AndroidResource`. Edytuj plik, który będzie wyglądać następująco:

```xml
<?xml version="1.0" encoding="utf-8"?>

<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp"
    android:textColor="#000">
</TextView>
```

Ten plik definiuje prosty [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) które będą używane dla każdego elementu, który pojawi się na liście sugestie.

Otwórz **Resources/Layout/Main.axml** i Wstaw następujący:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

Otwórz **MainActivity.cs** i Wstaw następujący kod do [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) metody:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    AutoCompleteTextView textView = FindViewById<AutoCompleteTextView> (Resource.Id.autocomplete_country);
    var adapter = new ArrayAdapter<String> (this, Resource.Layout.list_item, COUNTRIES);

    textView.Adapter = adapter;
}
```

Po ustawieniu widok zawartości `main.xml` układ [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) elementu widget są przechwytywane z Układem o [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/). Nowy [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) zainicjowano następnie powiązać `list_item.xml` układu do każdego elementu listy `COUNTRIES` tablicy ciągów (zdefiniowany w następnym kroku). Na koniec `SetAdapter()` jest wywoływana w celu skojarzenia [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) z [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) elementu widget, dzięki czemu są powielane listy sugestii w tablicy ciągów.

Wewnątrz `MainActivity` klasy, Dodaj tablicy ciągów:

```csharp
static string[] COUNTRIES = new string[] {
  "Afghanistan", "Albania", "Algeria", "American Samoa", "Andorra",
  "Angola", "Anguilla", "Antarctica", "Antigua and Barbuda", "Argentina",
  "Armenia", "Aruba", "Australia", "Austria", "Azerbaijan",
  "Bahrain", "Bangladesh", "Barbados", "Belarus", "Belgium",
  "Belize", "Benin", "Bermuda", "Bhutan", "Bolivia",
  "Bosnia and Herzegovina", "Botswana", "Bouvet Island", "Brazil", "British Indian Ocean Territory",
  "British Virgin Islands", "Brunei", "Bulgaria", "Burkina Faso", "Burundi",
  "Cote d'Ivoire", "Cambodia", "Cameroon", "Canada", "Cape Verde",
  "Cayman Islands", "Central African Republic", "Chad", "Chile", "China",
  "Christmas Island", "Cocos (Keeling) Islands", "Colombia", "Comoros", "Congo",
  "Cook Islands", "Costa Rica", "Croatia", "Cuba", "Cyprus", "Czech Republic",
  "Democratic Republic of the Congo", "Denmark", "Djibouti", "Dominica", "Dominican Republic",
  "East Timor", "Ecuador", "Egypt", "El Salvador", "Equatorial Guinea", "Eritrea",
  "Estonia", "Ethiopia", "Faeroe Islands", "Falkland Islands", "Fiji", "Finland",
  "Former Yugoslav Republic of Macedonia", "France", "French Guiana", "French Polynesia",
  "French Southern Territories", "Gabon", "Georgia", "Germany", "Ghana", "Gibraltar",
  "Greece", "Greenland", "Grenada", "Guadeloupe", "Guam", "Guatemala", "Guinea", "Guinea-Bissau",
  "Guyana", "Haiti", "Heard Island and McDonald Islands", "Honduras", "Hong Kong", "Hungary",
  "Iceland", "India", "Indonesia", "Iran", "Iraq", "Ireland", "Israel", "Italy", "Jamaica",
  "Japan", "Jordan", "Kazakhstan", "Kenya", "Kiribati", "Kuwait", "Kyrgyzstan", "Laos",
  "Latvia", "Lebanon", "Lesotho", "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg",
  "Macau", "Madagascar", "Malawi", "Malaysia", "Maldives", "Mali", "Malta", "Marshall Islands",
  "Martinique", "Mauritania", "Mauritius", "Mayotte", "Mexico", "Micronesia", "Moldova",
  "Monaco", "Mongolia", "Montserrat", "Morocco", "Mozambique", "Myanmar", "Namibia",
  "Nauru", "Nepal", "Netherlands", "Netherlands Antilles", "New Caledonia", "New Zealand",
  "Nicaragua", "Niger", "Nigeria", "Niue", "Norfolk Island", "North Korea", "Northern Marianas",
  "Norway", "Oman", "Pakistan", "Palau", "Panama", "Papua New Guinea", "Paraguay", "Peru",
  "Philippines", "Pitcairn Islands", "Poland", "Portugal", "Puerto Rico", "Qatar",
  "Reunion", "Romania", "Russia", "Rwanda", "Sqo Tome and Principe", "Saint Helena",
  "Saint Kitts and Nevis", "Saint Lucia", "Saint Pierre and Miquelon",
  "Saint Vincent and the Grenadines", "Samoa", "San Marino", "Saudi Arabia", "Senegal",
  "Seychelles", "Sierra Leone", "Singapore", "Slovakia", "Slovenia", "Solomon Islands",
  "Somalia", "South Africa", "South Georgia and the South Sandwich Islands", "South Korea",
  "Spain", "Sri Lanka", "Sudan", "Suriname", "Svalbard and Jan Mayen", "Swaziland", "Sweden",
  "Switzerland", "Syria", "Taiwan", "Tajikistan", "Tanzania", "Thailand", "The Bahamas",
  "The Gambia", "Togo", "Tokelau", "Tonga", "Trinidad and Tobago", "Tunisia", "Turkey",
  "Turkmenistan", "Turks and Caicos Islands", "Tuvalu", "Virgin Islands", "Uganda",
  "Ukraine", "United Arab Emirates", "United Kingdom",
  "United States", "United States Minor Outlying Islands", "Uruguay", "Uzbekistan",
  "Vanuatu", "Vatican City", "Venezuela", "Vietnam", "Wallis and Futuna", "Western Sahara",
  "Yemen", "Yugoslavia", "Zambia", "Zimbabwe"
};
```

Jest to lista sugestii, które zostaną dodane do listy rozwijanej, gdy użytkownik wpisze w [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) elementu widget.

Uruchom aplikację. Podczas wpisywania powinny pojawić się dane podobne do poniższych:

[![Funkcja automatycznego uzupełniania Zrzut ekranu listę nazw, które zawierają "ca"](auto-complete-images/helloautocomplete.png)](auto-complete-images/helloautocomplete.png#lightbox)



## <a name="more-information"></a>Więcej informacji

Należy pamiętać, że przy użyciu tablicy ciągów ustalony nie jest rozwiązaniem zalecany, ponieważ kod aplikacji należy skoncentrować się na zachowanie nie zawartości. Zawartość aplikacji, na przykład ciągi powinny externalized z kodu, aby ułatwić modyfikacje zawartości i ułatwienia lokalizacji zawartości. Ciągi ustalony są używane w tym samouczku tylko do był prosty i skoncentrować się na [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) elementu widget. Zamiast tego aplikacja powinna deklarować takie tablice ciągu w pliku XML. Można to zrobić z `<string-array>` zasobów w projekcie `res/values/strings.xml` pliku. Na przykład:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
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

Aby użyć tych ciągów zasobów dla [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/), Zastąp oryginalny [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) wiersza konstruktora z następującymi:

```csharp
string[] countries = Resources.GetStringArray (Resource.array.countries_array);
var adapter = new ArrayAdapter<String> (this, Resource.layout.list_item, countries);
```


### <a name="references"></a>Odwołania

-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
-   [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)

*Części tej stronie są zmiany na podstawie pracy utworzone i udostępnione przez Android Otwórz projekt źródłowy i używane zgodnie z warunki opisane w* 
 [ *Creative Commons 2.5 autorstwa licencji* ](http://creativecommons.org/licenses/by/2.5/) *. W tym samouczku jest oparta na* 
 [ *Android Autouzupełnianie samouczek*](http://developer.android.com/resources/tutorials/views/hello-autocomplete.html)
 *.*
