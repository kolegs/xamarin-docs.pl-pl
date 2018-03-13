---
title: "Dostosowywanie wyglądu elementu ListView"
ms.topic: article
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 1bf481e4999365f4afc52cb9dda83c6e627950e1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="customizing-a-listviews-appearance"></a>Dostosowywanie wyglądu elementu ListView


## <a name="overview"></a>Omówienie

Układ jest wyświetlanych wierszy jest zależna wyglądu w elemencie ListView. Aby zmienić wygląd `ListView`, użyj układu innego wiersza.


## <a name="built-in-row-views"></a>Widoki wbudowanych wiersza

Brak dwanaście wbudowana widoki, które można odwoływać się przy użyciu **Android.Resource.Layout**:

- **TestListItem** &ndash; pojedynczy wiersz tekstu z minimalnym formatowaniem.

- **SimpleListItem1** &ndash; pojedynczy wiersz tekstu.

- **SimpleListItem2** &ndash; dwóch wierszy tekstu.

- **SimpleSelectableListItem** &ndash; pojedynczy wiersz tekstu, który obsługuje wybór jednego lub wielu elementów (dodany w poziom interfejsu API 11).

- **SimpleListItemActivated1** &ndash; podobny do SimpleListItem1, ale kolor tła wskazuje, kiedy zostanie wybrany wiersz (dodany w poziom interfejsu API 11).

- **SimpleListItemActivated2** &ndash; podobny do SimpleListItem2, ale kolor tła wskazuje, kiedy zostanie wybrany wiersz (dodany w poziom interfejsu API 11).

- **SimpleListItemChecked** &ndash; Wyświetla znaczniki wyboru, aby wskazać zaznaczenia.

- **SimpleListItemMultipleChoice** &ndash; wyświetlane pola wyboru, aby wskazać wybór wielokrotny.

- **SimpleListItemSingleChoice** &ndash; Wyświetla radiowe przycisków, aby wskazać wzajemnie wykluczającymi się zaznaczenie.

- **TwoLineListItem** &ndash; dwóch wierszy tekstu.

- **ActivityListItem** &ndash; pojedynczy wiersz tekstu z obrazem.

- **SimpleExpandableListItem** &ndash; wierszy grupy kategorii, a każda grupa może rozwinięte lub zwinięte.

Każdy widok wbudowany wiersz ma wbudowany styl skojarzony z nim. Te zrzuty ekranu przedstawiają sposób wyświetlania każdego widoku:

[![Zrzuty ekranu TestListItem, SimpleSelectableListItem SimpleListitem1 i SimpleListItem2](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![Zrzuty ekranu SimpleListItemActivated1, SimpleListItemActivated2 SimpleListItemChecked i SimpleListItemMultipleChecked](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![Zrzuty ekranu SimpleListItemSingleChoice, TwoLineListItem ActivityListItem i SimpleExpandableListItem](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

**BuiltInViews/HomeScreenAdapter.cs** przykładowego pliku (w **BuiltInViews** rozwiązania) zawiera kod, aby utworzyć ekrany element listy nie można rozwijać. Widok jest ustawiany w `GetView` metody następująco:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

Wyświetl właściwości można następnie ustawić za pomocą odwołań do identyfikatorów formantu standardowego `Text1`, `Text2` i `Icon` w obszarze `Android.Resource.Id` (nie ustawiono właściwości, które widok nie zawiera lub będzie zgłaszany wyjątek):

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

**BuiltInExpandableViews/ExpandableScreenAdapter.cs** przykładowego pliku (w **BuiltInViews** rozwiązania) zawiera kod, aby utworzyć SimpleExpandableListItem ekranu. Widok grupy znajduje się w `GetGroupView` metody następująco:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

Widok podrzędny jest ustawiany w `GetChildView` metody następująco:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

Następnie można ustawić właściwości widoku grupy i widok podrzędny za pomocą odwołań do standardowego `Text1` i `Text2` kontrolować identyfikatory, jak pokazano powyżej. Zrzut ekranu SimpleExpandableListItem (należy pokazanym powyżej) zawiera przykładowy widok grupy jednego wiersza (SimpleExpandableListItem1) oraz widok podrzędny dwóch wierszy (SimpleExpandableListItem2). Alternatywnie widok grupy można skonfigurować na dwa wiersze (SimpleExpandableListItem2) i widok podrzędny można skonfigurować dla jednego wiersza (SimpleExpandableListItem1), lub obie grupy widoku i widok podrzędny może mieć taką samą liczbę wierszy. 



## <a name="accessories"></a>Akcesoria

Wiersze mogą być dodawane po prawej stronie widoku w celu wskazania stanu zaznaczenia akcesoria:

- **SimpleListItemChecked** &ndash; tworzy listę pojedynczego wyboru ze sprawdzaniem jako wskaźnik.

- **SimpleListItemSingleChoice** &ndash; tworzy typ radia — przycisk list, jeśli to możliwe tylko jedną opcję.

- **SimpleListItemMultipleChoice** &ndash; tworzy Jeżeli wiele opcji do wyboru są możliwe do listy wyboru typu.

Na następnych ekranach, ich kolejność odpowiednich przedstawiono Akcesoria wyżej:

[![Zrzuty ekranu SimpleListItemChecked, SimpleListItemSingleChoice i SimpleListItemMultipleChoice z Akcesoria](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

Aby wyświetlić jeden z tych przebiegu Akcesoria układu wymagany identyfikator zasobu do karty następnie ręcznie ustawić stan zaznaczenia wierszy wymagane. Ten wiersz kodu pokazano, jak utworzyć i przypisać `Adapter` przy użyciu jednej z tych układów:

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

`ListView` Sama obsługuje tryby wyboru różnych, niezależnie od akcesoriów będzie wyświetlany. Aby uniknąć pomyłek, należy użyć `Single` tryb zaznaczania z `Checked` i `SingleChoice` Akcesoria i `Multiple` trybu `MultipleChoice` stylu. Tryb zaznaczania jest kontrolowany przez `ChoiceMode` właściwość `ListView`.


### <a name="handling-api-level"></a>Obsługa poziom interfejsu API

Wcześniejszych wersji platformy Xamarin.Android zaimplementowane wyliczenia jako liczba całkowita właściwości. Najnowszą wersję wprowadziła odpowiednie typy wyliczeniowe .NET, co czyni go łatwiej odnajdywać potencjalnych opcji.

W zależności od poziomu interfejsu API, który ma być przeznaczona dla, `ChoiceMode` jest liczbą całkowitą lub wyliczenia. Przykładowy plik **AccessoryViews/HomeScreen.cs** ma blok oznaczone jako komentarz, jeśli ma zostać celem pierniki interfejsu API:

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // Single
//lv.ChoiceMode = 0; // none
//lv.ChoiceMode = 2; // Multiple
//lv.ChoiceMode = 3; // MultipleModal
*/
```


### <a name="selecting-items-programmatically"></a>Programowy wybór elementów

Ręczne ustawienie elementy, które są "wybrane" wykonuje się za pomocą `SetItemChecked` — metoda (może ona zostać wywołana wiele razy dla zaznaczenia wielu):

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

Kod wymaga także wykryć jednej opcji inaczej niż wielokrotny. Aby określić wiersz, który wybrano w `Single` Użyj trybu `CheckedItemPosition` właściwość typu integer:

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

Aby określić wiersze, które zostały wybrane w `Multiple` tryb należy pętli `CheckedItemPositions` `SparseBooleanArray`. Rozrzedzony tablicy przypomina słownik, który zawiera tylko wpisy gdzie wartość została zmieniona, więc musi przejść przez cały tablicy wyszukiwanie `true` wartości, aby dowiedzieć się, co zostało wybrane na liście zgodnie z opisami w poniższy fragment kodu:

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```


## <a name="creating-custom-row-layouts"></a>Tworzenie układów wiersza niestandardowe

Cztery widoki wbudowanych wiersza są bardzo proste. Aby wyświetlić bardziej złożonych układów (na przykład lista wiadomości e-mail, lub tweetów i informacje kontaktowe) widok niestandardowy jest wymagana. Niestandardowe widoki zazwyczaj są deklarowane jako pliki AXML w **zasobów/układ** katalogu, a następnie załadować, przy użyciu ich identyfikator przez kartę niestandardowego zasobu. Widok może zawierać dowolną liczbę klas wyświetlanych (na przykład TextViews ImageViews i innych kontrolek) z niestandardowych kolory, czcionki i układu.

W tym przykładzie różni się w poprzednich przykładach na kilka sposobów:

-  Dziedziczy `Activity` , a nie `ListActivity` . Wierszy można dostosować do dowolnego `ListView` , ale można również uwzględnić inne formanty w `Activity` układu (takie jak nagłówek, przyciski lub inne elementy interfejsu użytkownika). W tym przykładzie dodaje nagłówek powyżej `ListView` ilustrujący.

-  Wymaga pliku układu AXML dla ekranu; w poprzednich przykładach `ListActivity` nie wymaga pliku układu. Zawiera ten AXML `ListView` kontrolować deklaracji.

-  Wymaga pliku układu AXML do renderowania każdego wiersza. Ten plik AXML zawiera tekstowych i obrazów formantów niestandardowych czcionek i kolorów.

-  Używa pliku XML opcjonalne selektora niestandardowych można ustawić wygląd wiersza po jego wybraniu.

-  `Adapter` Implementacja zwraca niestandardowego układu z `GetView` zastąpienia.

-  `ItemClick` musi być zadeklarowany jako inaczej (procedura obsługi zdarzeń jest dołączony do `ListView.ItemClick` zamiast zastępowanie `OnListItemClick` w `ListActivity`).


Te zmiany są szczegółowo opisane poniżej, począwszy od tworzenia działania widoku oraz widoku niestandardowego wiersza i następnie obejmujące modyfikacje karty i aktywności do renderowania je.


### <a name="adding-a-listview-to-an-activity-layout"></a>Dodawanie elementu ListView z układem działania

Ponieważ `HomeScreen` już dziedziczy `ListActivity` nie ma widok domyślny, dlatego należy utworzyć plik AXML układu dla widoku HomeScreen. W tym przykładzie widok ma nagłówek (przy użyciu `TextView`) i `ListView` do wyświetlania danych. Układ jest zdefiniowany w **Resources/Layout/HomeScreen.axml** pliku, który jest następujący:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent">
    <TextView android:id="@+id/Heading"
        android:text="Vegetable Groups"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="#00000000"
        android:textSize="30dp"
        android:textColor="#FF267F00"
        android:textStyle="bold"
        android:padding="5dp"
    />
    <ListView android:id="@+id/List"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:cacheColorHint="#FFDAFF7F"
    />
</LinearLayout>
```

Zaletą używania `Activity` z niestandardowego układu (zamiast `ListActivity`) zaletą jest możliwość dodawania dodatkowych funkcji kontroli na ekranie, takie jak nagłówek `TextView` w tym przykładzie.


### <a name="creating-a-custom-row-layout"></a>Tworzenie układu niestandardowego wiersza

Inny plik układu AXML jest wymagany do zawierają niestandardowego układu dla każdego wiersza, który będzie wyświetlany w widoku listy. W tym przykładzie wiersz ma tła zielony, brązowy tekstowych i obrazów wyrównany do prawej. Znaczników XML dla systemu Android, aby zadeklarować ten układ jest opisany w **Resources/Layout/CustomView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout  xmlns:android="http://schemas.android.com/apk/res/android"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:background="#FFDAFF7F"
   android:padding="8dp">
    <LinearLayout android:id="@+id/Text"
       android:orientation="vertical"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:paddingLeft="10dip">
        <TextView
         android:id="@+id/Text1"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textColor="#FF7F3300"
         android:textSize="20dip"
         android:textStyle="italic"
         />
        <TextView
         android:id="@+id/Text2"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textSize="14dip"
         android:textColor="#FF267F00"
         android:paddingLeft="100dip"
         />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout >
```

Układ wiersza niestandardowej może zawierać wiele inne formanty, przewijanie wydajności mogą mieć wpływ złożonych wzorów i przy użyciu obrazów (zwłaszcza, jeśli mają być ładowane za pośrednictwem sieci). Zobacz artykuł firmy Google, aby uzyskać więcej informacji w rozwiązaniu problemów z wydajnością przewijania.


### <a name="referencing-a-custom-row-view"></a>Odwołuje się do widoku wiersza niestandardowe

Implementacja przykład adapter niestandardowy znajduje się w `HomeScreenAdapter.cs`. Metoda klucza jest `GetView` gdzie ładuje AXML niestandardowych, za pomocą Identyfikatora zasobu `Resource.Layout.CustomView`, a następnie ustawia na każdym z formantów w widoku przed zwróceniem jej właściwości. Przedstawiono to klasa pełną karty:

```csharp
public class HomeScreenAdapter : BaseAdapter<TableItem> {
   List<TableItem> items;
   Activity context;
   public HomeScreenAdapter(Activity context, List<TableItem> items)
       : base()
   {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
   {
       return position;
   }
   public override TableItem this[int position]
   {
       get { return items[position]; }
   }
   public override int Count
   {
       get { return items.Count; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       var item = items[position];
       View view = convertView;
       if (view == null) // no view to re-use, create new
           view = context.LayoutInflater.Inflate(Resource.Layout.CustomView, null);
       view.FindViewById<TextView>(Resource.Id.Text1).Text = item.Heading;
       view.FindViewById<TextView>(Resource.Id.Text2).Text = item.SubHeading;
       view.FindViewById<ImageView>(Resource.Id.Image).SetImageResource(item.ImageResourceId);
       return view;
   }
}
```


### <a name="referencing-the-custom-listview-in-the-activity"></a>Odwołuje się do niestandardowego elementu ListView w działaniu

Ponieważ `HomeScreen` teraz dziedziczy klasa `Activity`, `ListView` pole jest zadeklarowana w klasę, aby pomieścić odwołanie do formantu zadeklarowany w AXML:

```csharp
ListView listView;
```

Klasa następnie załaduj działania niestandardowego układu AXML przy użyciu `SetContentView` metody. Następnie można znaleźć `ListView` kontroli w układzie następnie tworzy i przypisuje karty i przypisuje obsługi kliknięcia. Kod metody OnCreate jest następujący:

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

Na koniec `ItemClick` obsługi musi być zdefiniowana; w takim przypadku wyświetli `Toast` komunikat:

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

Wynikowa ekranu wygląda następująco:

[![Zrzut ekranu przedstawiający wynikowy CustomRowView](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)



### <a name="customizing-the-row-selector-color"></a>Dostosowywanie kolorów selektora wiersza

Wiersz jest dotknięciu powinien być zaznaczony na opinie użytkowników. Gdy widok niestandardowy określa jako kolor tła jako **CustomView.axml** zastępuje ona również wyróżnienie zaznaczenia. Ten wiersz kodu w **CustomView.axml** zestawy tła jasnozielony, ale również oznacza nie nie wizualnej przy dotknięciu jest wiersz:

```xml
android:background="#FFDAFF7F"
```

Aby ponownie włączyć zachowanie wyróżnienia, a także aby dostosować kolor, który jest używany ustaw dla atrybutu tła z selektorem niestandardowych. Selektor będzie zadeklarować zarówno domyślny kolor tła, jak i kolor wyróżnienia. Plik **Resources/Drawable/CustomSelector.xml** zawiera następujące oświadczenie:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
<item android:state_pressed="false"
  android:state_selected="false"
  android:drawable="@color/cellback" />
<item android:state_pressed="true" >
  <shape>
     <gradient
      android:startColor="#E77A26"
        android:endColor="#E77A26"
        android:angle="270" />
  </shape>
</item>
<item android:state_selected="true"
  android:state_pressed="false"
  android:drawable="@color/cellback" />
</selector>
```

Aby odwołać selektor niestandardowych, zmienić atrybut tła w **CustomView.axml** do:

```xml
android:background="@drawable/CustomSelector"
```

Wybranego wiersza i odpowiadający mu `Toast` komunikatu teraz wygląda następująco:

[![Wybranego wiersza w kolorze pomarańczowym z komunikatem powiadomienia wyskakującego wyświetlanie nazwy wybranego wiersza](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)



### <a name="preventing-flickering-on-custom-layouts"></a>Zapobieganie migotanie na układy niestandardowe

Android próbuje poprawić wydajność `ListView` przewijanie przez buforowanie informacji o układzie. Jeśli masz długo przewijanie listy danych należy również ustawić `android:cacheColorHint` właściwość `ListView` deklaracji w definicji AXML działania (na tę samą wartość koloru jako tło układu niestandardowego wiersza). Nie można dołączyć tę wskazówkę może spowodować "migotanie" gdy użytkownik przewija za pośrednictwem listy z kolorów tła wiersza niestandardowych.



## <a name="related-links"></a>Linki pokrewne

- [BuiltInViews (przykład)](https://developer.xamarin.com/samples/BuiltInViews/)
- [AccessoryViews (przykład)](https://developer.xamarin.com/samples/AccessoryViews/)
- [CustomRowView (przykład)](https://developer.xamarin.com/samples/CustomRowView/)
