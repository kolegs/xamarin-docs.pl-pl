---
title: Podstawowy przykład obiektu RecyclerView
description: Przykładowa aplikacja, która pokazuje, jak używać obiektu RecyclerView.
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/13/2018
ms.openlocfilehash: abc21c3830126346ffb877639657c973da474812
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038394"
---
# <a name="a-basic-recyclerview-example"></a>Podstawowy przykład obiektu RecyclerView

Aby zrozumieć, jak `RecyclerView` działa w typowej aplikacji, w tym temacie przedstawiono [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) przykładowej aplikacji, na przykład prostego kodu, który używa `RecyclerView` do wyświetlenia duża kolekcja zdjęć: 

[![Dwa zrzuty ekranu aplikacji recyclerview wprowadzonych, która używa CardViews do wyświetlania zdjęcia](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer** używa [CardView](~/android/user-interface/controls/card-view.md) zaimplementować każdy element fotografii `RecyclerView` układu. Z powodu `RecyclerView`firmy korzyści związanych z wydajnością, ta Przykładowa aplikacja jest w stanie szybko przewiń duży zbiór zdjęcia, sprawnie i bez zauważalnego opóźnienia.


### <a name="an-example-data-source"></a>Przykładowe źródła danych

W tej przykładowej aplikacji, źródła danych "albumu" (reprezentowane przez `PhotoAlbum` klasy) dostarcza `RecyclerView` z zawartością elementu.
`PhotoAlbum` jest to zbiór zdjęcia z podpisami; Podczas tworzenia jego instancji, otrzymasz gotowej do użycia kolekcji 32 zdjęć:

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Każde wystąpienie zdjęcie w `PhotoAlbum` udostępnia właściwości umożliwiające odczyt jego identyfikator zasobu obrazu `PhotoID`i jego ciąg podpisu `Caption`. Kolekcja zdjęć jest zorganizowana w taki sposób, że każdego zdjęcia może zostać oceniony przez indeksator. Na przykład następujące wiersze kodu dostępu, identyfikator zasobu obrazu i transkrypcji oraz zdjęcia w kolekcji:

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` udostępnia również `RandomSwap` metody, które można wywołać w celu wymiany pierwszego zdjęcia w kolekcji ze zdjęciem losowo wybranych w innym miejscu w kolekcji:

```csharp
mPhotoAlbum.RandomSwap ();
```

Ponieważ szczegóły implementacji `PhotoAlbum` nie są istotne dla zrozumienia `RecyclerView`, `PhotoAlbum` kod źródłowy nie jest podana w tym miejscu. Kod źródłowy w celu `PhotoAlbum` znajduje się w temacie [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) w [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) przykładową aplikację.


### <a name="layout-and-initialization"></a>Układ i inicjalizacji

Plik układu **Main.axml**, składa się z pojedynczego `RecyclerView` w ramach `LinearLayout`:

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

Należy pamiętać, że należy użyć w pełni kwalifikowaną nazwę **android.support.v7.widget.RecyclerView** ponieważ `RecyclerView` jest spakowany w bibliotekę techniczną. `OnCreate` Metody `MainActivity` inicjuje ten układ, tworzy kartę i przygotowuje bazowego źródła danych:

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

2. Przekazuje źródła danych albumów zdjęć do konstruktora karty, `PhotoAlbumAdapter` (który jest zdefiniowany w dalszej części tego przewodnika). 
   Należy pamiętać, że jest uważana za najlepszym rozwiązaniem, aby przekazać źródła danych jako parametr do konstruktora karty. 

3. Pobiera `RecyclerView` z układu.

4. Należy podłączyć karty do `RecyclerView` wystąpienia, wywołując `RecyclerView` `SetAdapter` metody, jak pokazano powyżej.

### <a name="layout-manager"></a>Menedżer układu

Każdy element w `RecyclerView` składa się z `CardView` zawierający obraz zdjęcia i podpis fotografii (szczegółowe informacje znajdują się w [właściciela widoku](#view-holder) poniższej sekcji). Wstępnie zdefiniowane `LinearLayoutManager` służy do określania układu każdej `CardView` w układzie pionowe przewijanie:

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Ten kod, który znajduje się w głównym działaniu `OnCreate` metody. Konstruktor do Menedżera układ wymaga *kontekstu*, więc `MainActivity` jest przekazywany przy użyciu `this` jak pokazano powyżej.

Zamiast używania predefind `LinearLayoutManager`, możesz podłączyć w Menedżerze układu niestandardowego, który wyświetla dwa `CardView` elementy side-by-side, implementowanie page-turning efekt animacji, można przejść wzdłuż kolekcja zdjęć. W dalszej części tego przewodnika zobaczysz przykładowy sposób modyfikowania układu przez zamianę w Menedżerze innego układu.

<a name="view-holder" />

### <a name="view-holder"></a>Symbol zastępczy widoku

Widok klasy symbol zastępczy jest wywoływana `PhotoViewHolder`. Każdy `PhotoViewHolder` wystąpienie zawiera odwołania do `ImageView` i `TextView` elementu skojarzonego wiersza, który został rozmieszczony w `CardView` zgodnie z diagramu tutaj:

[![Diagram przedstawiający CardView zawierające ImageView i TextView](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` pochodzi od klasy `RecyclerView.ViewHolder` i zawiera właściwości do przechowywania odwołań do `ImageView` i `TextView` pokazane w układzie powyżej.
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
W tym przykładzie kodu `PhotoViewHolder` Konstruktor jest przekazywany odwołanie do widoku elementu nadrzędnego ( `CardView`) który `PhotoViewHolder` zawijany. Należy pamiętać, że w przypadku zawsze przekazywania nadrzędnego widoku elementu do konstruktora podstawowego. `PhotoViewHolder` Konstruktor wywołuje `FindViewById` w widoku elementu nadrzędnego wszystkich odwołań do widoku jego podrzędnych `ImageView` i `TextView`, zapisując wyniki w `Image` i `Caption` właściwości, odpowiednio. Karta później pobiera widoku odwołania z tych właściwości, po aktualizuje to `CardView`w widokach podrzędnych za pomocą nowych danych.

Aby uzyskać więcej informacji na temat `RecyclerView.ViewHolder`, zobacz [odwołań do klas RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="adapter"></a>Adapter

Karta ładuje każdego `RecyclerView` wierszy danych dotyczących określonego fotografii. Dla danego fotografii w położeniu wiersz *P*, na przykład karta lokalizuje powiązane dane w położeniu *P* w źródle danych i kopii tych danych do wiersza elementów w położeniu *P* w `RecyclerView` kolekcji. Karta używa posiadacza widok do wyszukiwania odwołań dla `ImageView` i `TextView` na tej pozycji, dzięki czemu nie trzeba wywoływać wielokrotnie `FindViewById` dla tych widoków, kiedy użytkownik przewija kolekcji zdjęcie i ponownie używa widoków.

W **RecyclerViewer**, jest pochodną klasę karty `RecyclerView.Adapter` utworzyć `PhotoAlbumAdapter`:

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

`mPhotoAlbum` Elementu członkowskiego zawiera źródła danych (albumu), który jest przekazywany do konstruktora; Konstruktor kopiuje albumu do tej zmiennej elementu członkowskiego. Następujące wymagane `RecyclerView.Adapter` metody są implementowane:

-   **`OnCreateViewHolder`** &ndash; Tworzy wystąpienie właściciela pliku i widoku układu elementu.

-   **`OnBindViewHolder`** &ndash; Ładuje dane w określonej pozycji do widoków, w których odwołania są przechowywane w właściciela danego widoku.

-   **`ItemCount`** &ndash; Zwraca liczbę elementów w źródle danych.

Menedżer układ wywołuje te metody, gdy jest on pozycjonowanie elementów w obrębie `RecyclerView`. Wykonanie tych metod jest badany w poniższych sekcjach.


#### <a name="oncreateviewholder"></a>OnCreateViewHolder

Wywołuje Menedżera układ `OnCreateViewHolder` podczas `RecyclerView` wymaga nowego właściciela widoku do reprezentowania elementu. `OnCreateViewHolder` zwiększa widoku elementu z pliku układu dla widoku i otacza widoku w nowym `PhotoViewHolder` wystąpienia. `PhotoViewHolder` Konstruktor lokalizuje i przechowuje odwołania do widoków podrzędne w układzie, jak opisano wcześniej w [właściciela widoku](#view-holder).

Każdy element wiersza jest reprezentowany przez `CardView` zawierający `ImageView` (w przypadku zdjęcie) i `TextView` (dla podpisu). Ten układ, który znajduje się w pliku **PhotoCardView.axml**:

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

Ten układ reprezentuje element pojedynczego wiersza w `RecyclerView`. `OnBindViewHolder` — Metoda (opisanych poniżej) kopiuje dane ze źródła danych do `ImageView` i `TextView` tego układu.
`OnCreateViewHolder` zwiększa ten układ dla lokalizacji zdjęć danym w `RecyclerView` i tworzy nową `PhotoViewHolder` wystąpienia (który lokalizuje i zapisuje w pamięci podręcznej odwołań do `ImageView` i `TextView` widoków podrzędnych w skojarzonej `CardView` układ):

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

Wynikowy wystąpienia właściciela widoku `vh`, jest zwracany do obiektu wywołującego (Menedżer układ).


#### <a name="onbindviewholder"></a>OnBindViewHolder

Gdy Menedżer układ jest gotowy do wyświetlania określonego widoku w `RecyclerView`firmy obszar ekranu widoczny wywoływanych przez nią karty `OnBindViewHolder` metody do wypełniania elementów w położeniu określony wiersz z zawartością ze źródła danych. `OnBindViewHolder` pobiera informacje o zdjęcie pozycji określony wiersz (zasób obrazu zdjęcia i ciągu dla podpisu zdjęcia), a następnie kopiuje te dane do widoków skojarzonych. Widoki znajdują się za pomocą odwołania przechowywanych w obiekcie właściciela widoku (która jest przekazywana w `holder` parametr):

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

Obiekt zastępczy przekazany w widoku najpierw musi być rzutowany na typ właściciela widoku pochodnych (w tym przypadku `PhotoViewHolder`) przed jego użyciem.
Karta ładuje zasób obrazu do widoku odwołuje się widok symbol zastępczy `Image` właściwości i kopiuje tekst podpisu do widoku odwołuje się widok symbol zastępczy `Caption` właściwości. To *wiąże* widok skojarzony ze swoimi danymi.

Należy zauważyć, że `OnBindViewHolder` jest kod, który bezpośrednio dotyczy strukturę danych. W tym przypadku `OnBindViewHolder` rozumie sposób mapowania `RecyclerView` elementu pozycji, aby element powiązane dane w źródle danych. Mapowanie jest prosta, w tym przypadku ponieważ pozycja może służyć jako indeks tablicy do albumów zdjęć; jednak bardziej złożone źródła danych może wymagać dodatkowego kodu, aby ustanowić takie mapowanie.


#### <a name="itemcount"></a>: ItemCount

`ItemCount` Metoda zwraca liczbę elementów w kolekcji danych. W aplikacji Podgląd przykład zdjęcia liczba elementów jest liczba zdjęcia w albumu:

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Aby uzyskać więcej informacji na temat `RecyclerView.Adapter`, zobacz [odwołań do klas RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).


### <a name="putting-it-all-together"></a>Zestawiania je wszystkie

Wartość wynikowa `RecyclerView` implementację dla aplikacji photo przykład składa się z `MainActivity` kod, który tworzy źródła danych, Menedżera układu i karty. `MainActivity` Tworzy `mRecyclerView` wystąpienie, tworzy wystąpienie źródła danych i karty i mocowana w Menedżerze układ i karty:

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

`PhotoViewHolder` Lokalizuje i zapisuje w pamięci podręcznej odwołań do widoku:

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

`PhotoAlbumAdapter` implementuje trzy zastąpienia wymagane metody:

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

W przypadku ten kod jest skompilowany i uruchomienia, tworzy podstawowe zdjęcia wyświetlania aplikacji, jak pokazano na poniższych zrzutach ekranu:

[![Dwa zrzuty ekranu, wyświetlania aplikacji za pomocą obszaru poziomego przewijania kart zdjęcie zdjęcia](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

To podstawowa aplikacja obsługuje tylko przeglądania albumu. Nie będzie reagować na zdarzenia elementu dotyku, a nie obsługuje zmiany w danych bazowych. Ta funkcja zostanie dodany do [rozszerzanie przykład obiektu RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md).


### <a name="changing-the-layoutmanager"></a>Zmiana LayoutManager

Z powodu `RecyclerView`firmy elastyczności jest łatwo modyfikować aplikację do używania Menedżera inny układ. W poniższym przykładzie zostanie zmodyfikowany do wyświetlania albumu przy użyciu układu siatki, który przewija w poziomie, a nie z pionowego układu liniowego. Aby to zrobić, modyfikacji wystąpienia menedżera układu w celu użycia `GridLayoutManager` w następujący sposób:

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Ta zmiana kodu zastępuje pionowe `LinearLayoutManager` z `GridLayoutManager` przedstawiające siatki składają się z dwóch wierszach, które można przewijać w poziomie kierunku. Gdy kompilujesz i ponownie uruchom aplikację zobaczysz, że fotografie są wyświetlane w siatce, i czy przewijanie jest poziomy, a nie pionowy:

[![Zrzut ekranu aplikacji za pomocą przewijanie w poziomie zdjęcia w siatce](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

Zmieniając tylko jeden wiersz kodu, to istnieje możliwość modyfikowania aplikacji wyświetlania zdjęć na inny układ za pomocą inne zachowanie.
Należy zauważyć, że kod karty ani układu XML nie miał modyfikacji w celu Zmień styl układu. 

W następnym temacie [rozszerzanie przykład obiektu RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md), ta aplikacja podstawowy przykład jest rozszerzony do obsługi zdarzeń kliknięcia elementu i zaktualizuj `RecyclerView` po zmianie w źródle danych bazowych.



## <a name="related-links"></a>Linki pokrewne

- [RecyclerViewer (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Obiektu RecyclerView elementy i funkcje](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Rozszerzanie przykład obiektu RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
