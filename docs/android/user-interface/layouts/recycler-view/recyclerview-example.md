---
title: "Podstawowy przykład RecyclerView"
ms.topic: article
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: a8de515563d9b9e38f049fd92c94b95e75239eb2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="a-basic-recyclerview-example"></a>Podstawowy przykład RecyclerView


Aby zrozumieć, jak `RecyclerView` działa w typowej aplikacji, w tym temacie opisuje [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) przykładowej aplikacji, prosty przykład kodu, który używa `RecyclerView` do wyświetlenia duży zbiór zdjęć: 

[ ![Dwa zrzuty ekranu aplikacji RecyclerView, która używa CardViews do wyświetlania zdjęć](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png)

**RecyclerViewer** używa [CardView](~/android/user-interface/controls/card-view.md) do zaimplementowania każdego elementu fotografii `RecyclerView` układu. Z powodu `RecyclerView`jego zalety wydajności, to przykładowa aplikacja jest w stanie szybko przewiń duży zbiór zdjęć sprawnie i bez zauważalnego opóźnienia.

<a name="datasource" />

### <a name="an-example-data-source"></a>Przykład źródła danych

W tej aplikacji przykład źródła danych "albumu" (reprezentowane przez `PhotoAlbum` klasy) dostarcza `RecyclerView` z zawartością elementu.
`PhotoAlbum` Kolekcja zdjęć z napisami; Możesz utworzyć, otrzymasz jest gotowe kolekcja zdjęć 32:

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Każde wystąpienie zdjęcie w `PhotoAlbum` udostępnia właściwości, które pozwalają na jej identyfikator zasobu obrazu do odczytu `PhotoID`, a jego ciąg podpisu `Caption`. Kolekcja zdjęć jest zorganizowana w taki sposób, że każdy fotografii można uzyskać, sprawdzając indeksatora. Na przykład następujące wiersze kodu dostępu, identyfikator zasobu obrazu i podpis dziesiątego zdjęcia w kolekcji:

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` dostępne są także `RandomSwap` metodę można wywołać w celu wymiany pierwszy zdjęcie w kolekcji z losowo wybranym zdjęcie w innym miejscu w kolekcji:

```csharp
mPhotoAlbum.RandomSwap ();
```

Ponieważ szczegóły implementacji `PhotoAlbum` nie są istotne dla zrozumienia `RecyclerView`, `PhotoAlbum` kodu źródłowego nie jest podana w tym miejscu. Aby kod źródłowy `PhotoAlbum` znajduje się w temacie [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) w [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) przykładowej aplikacji.

<a name="preliminaries" />

### <a name="layout-and-initialization"></a>Układ i inicjalizacji

Plik układu **Main.axml**, składa się pojedyncza `RecyclerView` w `LinearLayout`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Należy pamiętać, że muszą używać w pełni kwalifikowaną nazwę **android.support.v7.widget.RecyclerView** ponieważ `RecyclerView` jest spakowany w bibliotece pomocy technicznej. `OnCreate` Metody `MainActivity` inicjuje ten układ, karta tworzy i przygotowuje źródła danych:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Prepare the data source:
        mPhotoAlbum = new PhotoAlbum ();

        // Instantiate the adapter and pass in its data source:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);

        // Set our view from the "main" layout resource:
        SetContentView (Resource.Layout.Main);

        // Get our RecyclerView layout:
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug the adapter into the RecyclerView:
        mRecyclerView.SetAdapter (mAdapter);
```

Ten kod wykonuje następujące czynności:

1. Tworzy wystąpienie `PhotoAlbum` źródła danych.

2. Przekazuje do konstruktora karty, źródła danych albumy fotografii `PhotoAlbumAdapter` (która jest zdefiniowana w dalszej części tego przewodnika). 
   Należy pamiętać, że jest on uznawany za najlepiej przekazać źródła danych jako parametr do konstruktora karty. 

3. Pobiera `RecyclerView` z układu.

4. Podłączyć karty do `RecyclerView` wystąpienia przez wywołanie metody `RecyclerView` `SetAdapter` — metoda, jak pokazano powyżej.

### <a name="layout-manager"></a>Menedżer układu

Każdy element `RecyclerView` składa się z `CardView` zawiera obraz zdjęć i podpis fotografii (szczegółowe informacje znajdują się w [właściciela widoku](#view-holder) poniższej sekcji). Wstępnie zdefiniowane `LinearLayoutManager` służy do określania układu poszczególnych `CardView` w układzie przewijania pionowego:

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Ten kod znajduje się w głównym działaniu `OnCreate` metody. Konstruktor do menedżera układu wymaga *kontekstu*, więc `MainActivity` jest przekazywany za pomocą `this` jak pokazano powyżej.

Zamiast predefind `LinearLayoutManager`, możesz podłączyć w Menedżerze niestandardowego układu, w którym są wyświetlane dwa `CardView` side-by-side, implementacja efekt animacji page-turning przechodzenia za pomocą kolekcji zdjęć elementów. W dalszej części tego przewodnika zobaczysz przykładem zmodyfikować układ przez zamianę w Menedżerze inny układ.

<a name="view-holder" />

### <a name="view-holder"></a>Symbol zastępczy widoku

Klasa posiadacz widoku jest nazywana `PhotoViewHolder`. Każdy `PhotoViewHolder` wystąpienia zawiera odwołania do `ImageView` i `TextView` elementu wierszy, które jest określone w `CardView` jako diagramu tutaj:

[ ![Diagram CardView zawierające ImageView i TextView](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png)

`PhotoViewHolder` pochodną `RecyclerView.ViewHolder` i zawiera właściwości do przechowywania odwołań do `ImageView` i `TextView` widoczne w układzie powyżej.
`PhotoViewHolder` składa się z dwóch właściwości i jeden konstruktor:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```
W tym przykładzie kodu `PhotoViewHolder` Konstruktor jest przekazywany odwołanie do widoku elementu nadrzędnego ( `CardView`) który `PhotoViewHolder` zawijany. Należy pamiętać, że zawsze przesyłania dalej nadrzędnego elementu widoku do podstawowego konstruktora. `PhotoViewHolder` Wywołania konstruktora `FindViewById` w widoku elementu nadrzędnego wszystkich odwołań do widoku jego podrzędnych `ImageView` i `TextView`, zapisując wyniki w `Image` i `Caption` właściwości, odpowiednio. Karta później pobiera odwołuje się do widoku z tymi właściwościami, po to aktualizuje `CardView`przez widoki podrzędnych nowymi danymi.

Aby uzyskać więcej informacji na temat `RecyclerView.ViewHolder`, zobacz [odwołania do klasy RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="adapter"></a>Adapter

Karta ładuje każdego `RecyclerView` wiersza z danymi dla konkretnego zdjęcie. Dla danego fotografii na pozycji wiersza *P*, na przykład karta lokalizuje skojarzone dane na pozycji *P* w źródle danych i kopii tych danych do wiersza elementu na pozycji *P* w `RecyclerView` kolekcji. Karta korzysta posiadacz widoku do wyszukiwania odwołań dla `ImageView` i `TextView` na tej pozycji, więc nie wielokrotnie wywoływać `FindViewById` dla tych widoków jako użytkownik przewija kolekcji fotografii i widoki są ponownie używane.

W **RecyclerViewer**, karta klasa pochodzi od `RecyclerView.Adapter` utworzyć `PhotoAlbumAdapter`:

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;

    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }
    ...
}
```

`mPhotoAlbum` Elementu członkowskiego zawiera źródło danych (albumu), który jest przekazywany do konstruktora; konstruktora kopiuje albumu do tej zmiennej elementu członkowskiego. Następujące wymagane `RecyclerView.Adapter` metody są implementowane:

-   **`OnCreateViewHolder`** &ndash; Tworzy wystąpienie właściciela pliku i widoku układu elementu.

-   **`OnBindViewHolder`** &ndash; Ładuje dane w określonej pozycji do widoków, którego odwołania są przechowywane w posiadacz danego widoku.

-   **`ItemCount`** &ndash; Zwraca liczbę elementów w źródle danych.

Menedżer układu wywołuje tych metod, gdy jest pozycjonowanie elementów w obrębie `RecyclerView`. Wdrażanie tych metod jest analizowane w poniższych sekcjach.

<a name="oncreateviewholder" />

#### <a name="oncreateviewholder"></a>OnCreateViewHolder

Wywołuje menedżera układu `OnCreateViewHolder` podczas `RecyclerView` wymaga nowego właściciela widoku do reprezentowania elementu. `OnCreateViewHolder` nadyma widok elementu z pliku układu dla widoku i opakowuje widoku w nowym `PhotoViewHolder` wystąpienia. `PhotoViewHolder` Konstruktor lokalizuje i przechowuje odwołania do widoków podrzędne w układzie, jak opisano wcześniej w [posiadacz widoku](#view-holder).

Każdy element wiersza jest reprezentowana przez `CardView` zawierający `ImageView` (dla zdjęcie) i `TextView` (dla podpisu). Ten układ znajduje się w pliku **PhotoCardView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:card_view="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        card_view:cardElevation="4dp"
        card_view:cardUseCompatPadding="true"
        card_view:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Caption"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="4dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</FrameLayout>
```

Ten układ reprezentuje element pojedynczy wiersz w `RecyclerView`. `OnBindViewHolder` — Metoda (opisanych poniżej) kopiuje dane ze źródła danych do `ImageView` i `TextView` tego układu.
`OnCreateViewHolder` nadyma tego układu dla lokalizacji danego zdjęcie w `RecyclerView` i tworzy nowy `PhotoViewHolder` wystąpienia (który lokalizuje i buforuje odwołania do `ImageView` i `TextView` widoków podrzędnych w skojarzonym `CardView` układu):

```csharp
public override RecyclerView.ViewHolder
    OnCreateViewHolder (ViewGroup parent, int viewType)
{
    // Inflate the CardView for the photo:
    View itemView = LayoutInflater.From (parent.Context).
                Inflate (Resource.Layout.PhotoCardView, parent, false);

    // Create a ViewHolder to hold view references inside the CardView:
    PhotoViewHolder vh = new PhotoViewHolder (itemView);
    return vh;
}

```

Wynikowa wystąpienia właściciela widoku `vh`, jest zwracana do wywołującego (Menedżer układ).

<a name="onbindviewholder" />

#### <a name="onbindviewholder"></a>OnBindViewHolder

Gdy Menedżer układu jest gotowy do wyświetlania określonego widoku w `RecyclerView`przez obszar ekranu widoczne wywołuje karty `OnBindViewHolder` metody do wypełniania elementu na pozycji określony wiersz o zawartości ze źródła danych. `OnBindViewHolder` pobiera informacje o fotografii dla pozycji określony wiersz (zdjęcia zasobu obrazu i ciągu dla podpisu zdjęcia) i kopiuje dane z widokami skojarzone. Widoki znajdują się za pomocą odwołania przechowywane w obiekcie posiadacz widoku (który jest przekazywana w `holder` parametru):

```csharp
public override void
    OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
{
    PhotoViewHolder vh = holder as PhotoViewHolder;

    // Load the photo image resource from the photo album:
    vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);

    // Load the photo caption from the photo album:
    vh.Caption.Text = mPhotoAlbum[position].Caption;
}
```

Najpierw należy można rzutować obiektu posiadacz przekazany w widoku do typu posiadacz pochodnych widoku (w tym przypadku `PhotoViewHolder`) przed jego użyciem.
Karta ładuje zasób obrazu do widoku odwołuje się widok symbol zastępczy `Image` właściwości, a kopiuje tekst podpisu w widoku odwołuje się widok symbol zastępczy `Caption` właściwości. To *wiąże* ze swoimi danymi skojarzonego widoku.

Zwróć uwagę, że `OnBindViewHolder` jest kod, który bezpośrednio dotyczy struktury danych. W takim przypadku `OnBindViewHolder` zrozumienie sposobu mapowania `RecyclerView` elementu stanie element skojarzonych danych w źródle danych. Mapowanie jest prosta w takim przypadku ponieważ pozycja może być używany jako indeks tablicy do albumu; Jednakże bardziej złożone źródła danych mogą wymagać dodatkowego kodu do ustanawiania mapowanie programu.

<a name="itemcount" />

#### <a name="itemcount"></a>Wartość elementu ItemCount

`ItemCount` Metoda zwraca liczbę elementów w kolekcji danych. W przykładową aplikację Podgląd fotografii liczba elementów to liczba zdjęć w albumu:

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Aby uzyskać więcej informacji na temat `RecyclerView.Adapter`, zobacz [odwołania do klasy RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

<a name="together" />

### <a name="putting-it-all-together"></a>Zestawienie on wszystkie

Powstałe w ten sposób `RecyclerView` wdrożenia dla aplikacji zdjęcie przykład składa się z `MainActivity` kod, który tworzy źródła danych, Menedżera układu i karty. `MainActivity` Tworzy `mRecyclerView` wystąpienia, tworzy wystąpienie źródła danych i karty i mocowana w Menedżerze układ i karty:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        mPhotoAlbum = new PhotoAlbum();
        SetContentView (Resource.Layout.Main);
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug in the linear layout manager:
        mLayoutManager = new LinearLayoutManager (this);
        mRecyclerView.SetLayoutManager (mLayoutManager);

        // Plug in my adapter:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
        mRecyclerView.SetAdapter (mAdapter);
    }
}

```

`PhotoViewHolder` Lokalizuje i buforuje odwołuje się do widoku:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

`PhotoAlbumAdapter` implementuje trzy zastąpienia wymaganej metody:

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;
    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }

    public override RecyclerView.ViewHolder
        OnCreateViewHolder (ViewGroup parent, int viewType)
    {
        View itemView = LayoutInflater.From (parent.Context).
                    Inflate (Resource.Layout.PhotoCardView, parent, false);
        PhotoViewHolder vh = new PhotoViewHolder (itemView);
        return vh;
    }

    public override void
        OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
    {
        PhotoViewHolder vh = holder as PhotoViewHolder;
        vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);
        vh.Caption.Text = mPhotoAlbum[position].Caption;
    }

    public override int ItemCount
    {
        get { return mPhotoAlbum.NumPhotos; }
    }
}
```

Gdy ten kod jest skompilować i uruchomić, tworzy podstawowe fotografii, wyświetlanie aplikacji, jak pokazano na poniższych zrzutach ekranu:

[ ![Dwa zrzuty ekranu fotografii wyświetlanie aplikacji za pomocą obszaru poziomego przewijania kart zdjęcia](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png)

Podstawowa aplikacja obsługuje tylko przeglądanie albumu. Nie odpowie na element dotyku zdarzenia nie obsługuje zmian w danych źródłowych. Ta funkcja jest dodawany w [rozszerzanie przykład RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md).

<a name="layoutmanagerchange" />

### <a name="changing-the-layoutmanager"></a>Zmiana LayoutManager

Z powodu `RecyclerView`firmy elastyczności, jest łatwo zmodyfikować aplikację za pomocą Menedżera inny układ. W poniższym przykładzie jest modyfikowany do wyświetlenia albumu z układem siatki, które jest przewijane w poziomie, a nie z pionowego układu liniowej. W tym celu należy utworzyć wystąpienia menedżera układu jest modyfikacji w celu użycia `GridLayoutManager` w następujący sposób:

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Ta zmiana kodu zastępuje pionową `LinearLayoutManager` z `GridLayoutManager` przedstawiający Siatka składa się z dwóch wierszy, które przewijać w poziomie kierunku. Podczas kompilowania i ponownie uruchom aplikację, pojawi się, czy w siatce są wyświetlane fotografie i czy przewijanie jest poziomy, a nie pionowej:

[ ![Zrzut ekranu aplikacji z przewijania w poziomie zdjęć w siatce](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png)

Zmieniając tylko jeden wiersz kodu jest można modyfikować aplikacji wyświetlanie zdjęć do innego układu za pomocą inaczej.
Zwróć uwagę, że kod karty ani układu XML muszą zostać zmodyfikowane w celu Zmień styl układu. 

W następnym temacie [rozszerzanie przykład RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md), tej aplikacji przykładowej podstawowa jest rozszerzony do obsługi zdarzeń kliknięcia elementu i zaktualizuj `RecyclerView` po zmianie w źródle danych.



## <a name="related-links"></a>Linki pokrewne

- [RecyclerViewer (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Części RecyclerView i funkcji](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Rozszerzanie przykład RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
