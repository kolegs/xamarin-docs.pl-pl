---
title: Rozszerzanie przykład obiektu RecyclerView
description: Dodawanie obsługi zdarzeń kliknięcia elementu aplikacji przykład obiektu RecyclerView.
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/13/2018
ms.openlocfilehash: 73c14e76a4a65c73c5fe0cc3d43329a9f4965c74
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038524"
---
# <a name="extending-the-recyclerview-example"></a>Rozszerzanie przykład obiektu RecyclerView


Podstawowa aplikacja opisanego w [podstawowy przykład obiektu RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) faktycznie nie znacznie &ndash; po prostu przewija i wyświetla listę stałych elementów fotografii, aby ułatwić przeglądanie. W rzeczywistych aplikacjach użytkownicy chcą mieć możliwość interakcji z aplikacją, naciskając elementów na ekranie. Ponadto bazowego źródła danych można zmienić (lub można zmienić za pomocą aplikacji), a zawartość ekranu musi być zgodna z tych zmian. W poniższych sekcjach dowiesz się, jak obsługiwać zdarzenia kliknięcia elementu i zaktualizować `RecyclerView` po zmianie w źródle danych bazowych.


### <a name="handling-item-click-events"></a>Obsługa zdarzeń przedmiot, kliknij

Gdy użytkownik dotyka element `RecyclerView`, jest generowane zdarzenie kliknięcia elementu, by powiadomić aplikację, które zostało korzysta z elementu. To zdarzenie nie jest generowany przez `RecyclerView` &ndash; zamiast tego widoku elementu, (która jest opakowana w właściciela widoku) wykrywa poprawek i raporty z tych poprawek, ponieważ zdarzenia kliknięcia.

Aby zilustrować procedurę obsługi zdarzeń kliknięcia elementu, poniższe kroki wyjaśniają, jak podstawowa aplikacja wyświetlania zdjęć zostanie zmodyfikowany na potrzeby raportu fotografii, które miały korzystały użytkownika. Gdy wystąpi zdarzenie kliknięcia elementu w przykładowej aplikacji, następująca sekwencja zdarzeń:

1.  Zdjęcie `CardView` wykryje Zdarzenie kliknięcia elementu, a następnie powiadamia użytkownika karty.

2.  Karta przekazuje zdarzenia (z informacjami położenie elementu) do obsługi kliknięcia elementu tego działania.

3.  Procedury obsługi kliknięcia elementu działania reaguje na zdarzenie kliknięcia elementu.

Po pierwsze, o nazwie elementu członkowskiego program obsługi zdarzeń `ItemClick` jest dodawany do `PhotoAlbumAdapter` definicję klasy:

```csharp
public event EventHandler<int> ItemClick;
```

Następnie metoda obsługi zdarzeń kliknij element zostanie dodany do `MainActivity`.
Ten program obsługi krótko wyświetla wyskakujące, wskazujący, który element fotografii został dotknięciu:

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Następnie wiersz kodu jest wymagany do zarejestrowania `OnItemClick` procedura obsługi o `PhotoAlbumAdapter`. Dobrym miejscem, w tym celu jest natychmiast po `PhotoAlbumAdapter` zostanie utworzony: 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

W tym podstawowym przykładzie obsługi rejestracja odbywa się w głównym działaniu `OnCreate` metody, ale aplikacji produkcyjnej może zarejestrować programu obsługi w `OnResume` i wyrejestrować go w `OnPause` &ndash; zobacz [cykl życia aktywności ](~/android/app-fundamentals/activity-lifecycle/index.md) Aby uzyskać więcej informacji.

`PhotoAlbumAdapter` teraz będzie wywoływać `OnItemClick` po odebraniu zdarzenia kliknięcia elementu. Następnym krokiem jest utworzenie obsługi karty, która wywołuje to `ItemClick` zdarzeń. Następującą metodę `OnClick`, dodawany jest natychmiast po karty `ItemCount` metody:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

To `OnClick` metodą jest karty *odbiornika* zdarzeń kliknięcia elementu z elementu widoków. Zanim można zarejestrować tego odbiornika z widoku elementu (za pośrednictwem widoku elementu widoku symbol zastępczy), `PhotoViewHolder` Konstruktor muszą zostać zmodyfikowane, aby zaakceptować tę metodę jako dodatkowy argument, a następnie zarejestrować `OnClick` przy użyciu widoku elementu `Click` zdarzeń.
Oto zmodyfikowanego `PhotoViewHolder` Konstruktor:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

`itemView` Parametr zawiera odwołanie do `CardView` , został korzystał z usług przez użytkownika. Należy pamiętać, że klasę bazową widoku posiadacza zna pozycji układu elementu (`CardView`), czyli przedstawia liczbę (za pośrednictwem `LayoutPosition` właściwości), a ta pozycja jest przekazywana do karty `OnClick` metody po wystąpieniu zdarzenia kliknięcia elementu. Karty `OnCreateViewHolder` metoda zostanie zmodyfikowany na potrzeby przekazywania karty `OnClick` metody do konstruktora właściciela widoku:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Teraz gdy kompilujesz i uruchamianie przykładowej aplikacji wyświetlanie zdjęć, naciskając zdjęć na ekranie spowoduje, że wyskakujące do pojawiają się raporty fotografii, który został dotknięciu:

[![Dotknięcie przykład wyskakujące, wyświetlanego, gdy karta zdjęcie](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

W tym przykładzie przedstawiono tylko jedno z podejść do implementowania procedury obsługi zdarzeń za pomocą `RecyclerView`. Innym rozwiązaniem, która mogłaby być używana w tym miejscu jest umieszczenie zdarzenia właścicielowi widoku i mieć kartę subskrybowania tych zdarzeń. Jeśli przykładowa aplikacja zdjęcia możliwości do edycji zdjęć, odrębne zdarzenia jest wymagana dla `ImageView` i `TextView` w ramach każdej `CardView`: styka się `TextView` spowoduje uruchomienie `EditView` okno dialogowe, które umożliwia użytkownikowi edytowanie Podpis i ma na `ImageView` może uruchomić narzędzie Korygowanie zdjęć, które umożliwia użytkownikowi obrócić zdjęcie lub Przytnij. W zależności od potrzeb aplikacji należy zaprojektować najlepszym rozwiązaniem w zakresie obsługi i reagowanie na zdarzenia dotykowe.

Aby zademonstrować sposób `RecyclerView` mogą być aktualizowane, gdy zmiany zestawu danych, wyświetlanie zdjęć przykładowej aplikacji można modyfikować, aby losowo wybrać zdjęcie w źródle danych i jego zamianę z pierwszego zdjęcia. Po pierwsze, **wybierz losowe** zostanie dodany do Przykładowa aplikacja zdjęcia **Main.axml** układu:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/randPickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:text="Random Pick" />
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Następnie kod zostanie dodany na końcu głównego działania `OnCreate` metodą lokalizowania `Random Pick` znajdujący się w układzie i do niej dołączyć program obsługi:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        // Randomly swap a photo with the first photo:
        int idx = mPhotoAlbum.RandomSwap();
    }
};

```

Ten program obsługi wywołania albumów zdjęć `RandomSwap` metody podczas **wybierz losowe** naciśnięcia przycisku. `RandomSwap` Metoda losowo zamienia zdjęć z pierwszego zdjęcia w źródle danych, a następnie zwraca indeks zdjęcie losowo zamienione. Gdy kompilujesz i uruchamianie przykładowej aplikacji przy użyciu tego kodu, naciskając **wybierz losowe** przycisku nie powoduje zmiany wyświetlania, ponieważ `RecyclerView` nie ma informacji o zmiany w źródle danych.

Zapewnienie `RecyclerView` aktualizowane po zmianie, w źródle danych **wybierz losowe** kliknij program obsługi, należy zmodyfikować w celu wywołania karty `NotifyItemChanged` metodę dla każdego elementu w kolekcji, które uległy zmianie (w tym przypadku dwa elementy mają zmienione: pierwszy zdjęć i zdjęcie wymienione). Powoduje to, że `RecyclerView` można zaktualizować jego wyświetlania, aby była spójna ze stanem z nowego źródła danych:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        int idx = mPhotoAlbum.RandomSwap();

        // First photo has changed:
        mAdapter.NotifyItemChanged(0);

        // Swapped photo has changed:
        mAdapter.NotifyItemChanged(idx);
    }
};

```

Teraz, gdy **wybierz losowe** naciśnięcia przycisku `RecyclerView` aktualizuje widok, aby pokazać, że zdjęcie dalsze w dół w kolekcji została zapisana z pierwszego zdjęcia w kolekcji:

[![Pierwszy zrzut ekranu przed wymiany, drugi zrzut ekranu po wymiany](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Oczywiście `NotifyDataSetChanged` mógł zostać wywołany zamiast dwóch wywołań do `NotifyItemChanged`, ale wykonanie tej tak może wymusić `RecyclerView` do odświeżenia całej kolekcji mimo że tylko dwa elementy w kolekcji zostały zmienione. Wywoływanie `NotifyItemChanged` jest znacznie bardziej efektywne niż wywoływania `NotifyDataSetChanged`.


## <a name="related-links"></a>Linki pokrewne

- [RecyclerViewer (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Obiektu RecyclerView elementy i funkcje](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Podstawowy przykład obiektu RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
