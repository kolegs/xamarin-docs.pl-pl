---
title: Wypełnianie ListView z danymi
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: 83b398faf4fd634b7d492d372524b5fdd00163d0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="populating-a-listview-with-data"></a>Wypełnianie ListView z danymi


## <a name="overview"></a>Omówienie

Wiersze, aby dodać `ListView` należy dodać go do układu i wdrożenie `IListAdapter` za pomocą metod który `ListView` wywołania do wypełnienia samej siebie. Android obejmuje wbudowane `ListActivity` i `ArrayAdapter` klasy, które można użyć bez definiowania układu niestandardowego XML ani kodu innych. `ListActivity` Klasy automatycznie tworzy `ListView` i udostępnia `ListAdapter` właściwość, aby podać widoków wierszy do wyświetlenia za pośrednictwem karty.

Karty wbudowane przyjmować identyfikator zasobu widoku jako parametr, który jest używany dla każdego wiersza. Można użyć wbudowanych zasobów, takich jak zasoby w `Android.Resource.Layout` , nie trzeba napisać własne.


## <a name="using-listactivity-and-arrayadapterltstringgt"></a>Przy użyciu ListActivity i ArrayAdapter&lt;ciągu&gt;

Przykład **BasicTable/HomeScreen.cs** pokazano, jak używać tych klas do wyświetlania `ListView` tylko kilku wierszy kodu:

```csharp
[Activity(Label = "BasicTable", MainLauncher = true, Icon = "@drawable/icon")]
public class HomeScreen : ListActivity {
   string[] items;
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       items = new string[] { "Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers" };
       ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItem1, items);
   }
   protected override void OnListItemClick(ListView l, View v, int position, long id)
}
```


### <a name="handling-row-clicks"></a>Obsługa wiersza kliknie

Zazwyczaj `ListView` będzie również umożliwiać użytkownikowi touch wierszy do wykonania niektórych akcji (np. odtwarzanie utworu, wywoływania kontaktu lub przedstawiający inny ekran). Aby odpowiedzieć na poprawki użytkownika musi mieć więcej Metoda implementowana w `ListActivity` &ndash; `OnListItemClick` &ndash; podobnie do następującej:

[![Zrzut ekranu przedstawiający SimpleListItem](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

Teraz użytkownik może touch wiersza i a `Toast` pojawi się alert:

[![Zrzut ekranu z powiadomienia wyskakującego o jest wyświetlany, gdy wiersz jest dotknięciu](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)


## <a name="implementing-a-listadapter"></a>Implementowanie ListAdapter

`ArrayAdapter<string>` jest bardzo ze względu na jego prostotę, ale bardzo ograniczone. Jednakże często istnieje kolekcja jednostki biznesowe, a nie tylko ciągi, które chcesz powiązać.
Na przykład jeśli dane zawierają kolekcja klas pracownika, następnie można listy, aby wyświetlić tylko nazwy każdego pracownika. Aby dostosować zachowanie `ListView` kontrolować, jakie dane są wyświetlane musi implementować podklasą `BaseAdapter` zastępowanie cztery następujące elementy:

-   **Liczba** &ndash; formantu stwierdzić, ile wierszy znajdują się w danych.

-   **GetView** &ndash; wypełnione do zwrócenia widoku dla każdego wiersza danych.
    Ta metoda ma parametr `ListView` do przekazania w wierszu istniejących, nieużywane do ponownego użycia.

-   **GetItemId** &ndash; zwraca identyfikator wiersza (zazwyczaj wiersz numer, chociaż może być wartość długa, który chcesz).

-   **Ta [int]** indeksatora &ndash; aby zwrócić dane skojarzone z liczbą określonego wiersza.

Przykładowy kod w **BasicTableAdapter/HomeScreenAdapter.cs** pokazano, jak podklasy `BaseAdapter`:

```csharp
public class HomeScreenAdapter : BaseAdapter<string> {
   string[] items;
   Activity context;
   public HomeScreenAdapter(Activity context, string[] items) : base() {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
  {
       return position;
   }
   public override string this[int position] {  
       get { return items[position]; }
   }
   public override int Count {
       get { return items.Length; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       View view = convertView; // re-use an existing view, if one is available
      if (view == null) // otherwise create a new one
           view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
       view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
       return view;
   }
}
```


### <a name="using-a-custom-adapter"></a>Za pomocą karty niestandardowe

Za pomocą karty niestandardowe jest podobny do wbudowanej `ArrayAdapter`, przekazując `context` i `string[]` wartości do wyświetlenia:

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

Ponieważ w tym przykładzie użyto ten sam układ wiersza (`SimpleListItem1`) aplikacji wynikowy będzie wyglądać takie same jak w poprzednim przykładzie.


### <a name="row-view-re-use"></a>Wiersz widoku ponownego użycia.

W tym przykładzie są tylko sześć elementów. Ponieważ ekranu można zmieścić osiem, nie wiersza ponownego użycia wymagane. Wyświetlając setkami lub tysiącami wierszy, jednak byłoby odpady pamięci do utworzenia setek lub tysięcy `View` obiektów, kiedy tylko osiem na ekranie mieści się w czasie. Aby uniknąć tej sytuacji, gdy wiersz zniknie z ekranu, jego widoku jest umieszczony w kolejce do ponownego użycia. Jako użytkownik przewija widok, `ListView` wywołania `GetView` żądania nowych widoków do wyświetlenia &ndash; Jeśli dostępne przekazaniem nieużywane widoku w `convertView` parametru. Jeśli ta wartość jest równa null, kod należy utworzyć nowe wystąpienie widoku, w przeciwnym razie możesz ponownie określić właściwości tego obiektu i użyć go ponownie.

`GetView` Metody należy wykonać ten wzorzec do ponownego użycia widoków wiersza:

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

Adapter niestandardowy implementacje powinny *zawsze* ponownego użycia `convertView` obiektu przed utworzeniem nowych widoków, aby upewnić się, nie można uruchamiać za mało pamięci podczas wyświetlania długich list.

Niektóre implementacje karty (takich jak `CursorAdapter`) nie ma `GetView` metody, a nie wymagają one dwie różne metody `NewView` i `BindView` którego wymuszanie ponownego użycia wiersza oddzielając obowiązków `GetView` na dwie metody. Brak `CursorAdapter` przykładzie w dalszej części dokumentu.


## <a name="enabling-fast-scrolling"></a>Włączanie szybkie przewijanie

Szybkie przewijanie pomaga użytkownika, aby przewijać długich list, zapewniając dodatkowe 'dojścia"działającego jako pasek przewijania bezpośredni dostęp do części listy. Ten zrzut ekranu przedstawia uchwytu szybkiego przewijania:

[![Zrzut ekranu przedstawiający szybkie przewijanie z dojściem przewijania](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

Powoduje szybkiego przewijania dojście pojawią się jest tak proste, jak ustawienie `FastScrollEnabled` właściwości `true`:

```csharp
ListView.FastScrollEnabled = true;
```


### <a name="adding-a-section-index"></a>Dodawanie indeksu sekcji

Indeks sekcji zapewnia dodatkowe opinii użytkowników, gdy są one przewijanie długą listę fast &ndash; zawiera które "sekcji" były one przewijane w. Powoduje indeks sekcji się podklasą karty musi implementować `ISectionIndexer` interfejs do dostarczania tekst indeksu w zależności od jest wyświetlanych wierszy:

[![Zrzut ekranu H znajdującego się powyżej sekcji, która rozpoczyna się od H](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

Aby zaimplementować `ISectionIndexer` należy dodać do karty trzech metod:

-   **GetSections** &ndash; zawiera pełną listę sekcji indeks tytułów, które mogą być wyświetlane. Ta metoda wymaga tablicę obiektów języka Java, dlatego ten kod musi utworzyć `Java.Lang.Object[]` z kolekcji .NET. W naszym przykładzie zwraca listę początkowego znaków na liście jako `Java.Lang.String` .

-   **GetPositionForSection** &ndash; zwraca pierwszą pozycję wiersza dla sekcji danego indeksu.

-   **GetSectionForPosition** &ndash; zwraca indeks sekcji, który będzie wyświetlany dla danego wiersza.


Przykład `SectionIndex/HomeScreenAdapter.cs` pliku implementuje jednej z tych metod i dodatkowy kod w konstruktorze. Konstruktor tworzy indeks sekcji każdego wiersza w pętli i wyodrębnianie pierwszego znaku tytułu (elementy już muszą być posortowane w tym celu pracy).

```csharp
alphaIndex = new Dictionary<string, int>();
for (int i = 0; i < items.Length; i++) { // loop through items
   var key = items[i][0].ToString();
   if (!alphaIndex.ContainsKey(key))
       alphaIndex.Add(key, i); // add each 'new' letter to the index
}
sections = new string[alphaIndex.Keys.Count];
alphaIndex.Keys.CopyTo(sections, 0); // convert letters list to string[]

// Interface requires a Java.Lang.Object[], so we create one here
sectionsObjects = new Java.Lang.Object[sections.Length];
for (int i = 0; i < sections.Length; i++) {
   sectionsObjects[i] = new Java.Lang.String(sections[i]);
}
```

Z struktur danych utworzone `ISectionIndexer` metody są bardzo proste:

```csharp
public Java.Lang.Object[] GetSections()
{
   return sectionsObjects;
}
public int GetPositionForSection(int section)
{
   return alphaIndexer[sections[section]];
}
public int GetSectionForPosition(int position)
{   // this method isn't called in this example, but code is provided for completeness
    int prevSection = 0;
    for (int i = 0; i < sections.Length; i++)
    {
        if (GetPositionForSection(i) > position)
        {
            break;
        }
        prevSection = i;
    }
    return prevSection;
}
```

Twoje tytuły indeks sekcji nie ma potrzeby mapowanie 1:1, do rzeczywistych sekcje. Jest to dlaczego `GetPositionForSection` istnieje metoda.
`GetPositionForSection` zapewnia możliwość mapy, niezależnie od indeksy są na liście indeksu do dowolnego sekcjach znajdują się w widoku listy. Na przykład "z" może mieć w Twoim indeksie, ale nie może mieć sekcji tabela dla każdej litery, dlatego zamiast mapowanie "z" 26, są mapowane do 25 lub 24 lub niezależnie od indeks sekcji "z" powinny być mapowane na.



## <a name="related-links"></a>Linki pokrewne

- [BasicTableAndroid (przykład)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (przykład)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [FastScroll (przykład)](https://developer.xamarin.com/samples/FastScroll/)
