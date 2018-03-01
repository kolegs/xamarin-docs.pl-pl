---
title: "Rozszerzanie przykład RecyclerView"
ms.topic: article
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: de4683ca660224aa3cf17398ac649086b7e4ad88
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="extending-the-recyclerview-example"></a>Rozszerzanie przykład RecyclerView


Podstawowa aplikacja opisanego w [A podstawowy przykład RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) faktycznie nie znacznie &ndash; po prostu przewija i wyświetla listę stałych elementów fotografii ułatwia przeglądanie. W rzeczywistych aplikacjach użytkownicy będą mieć możliwość interakcji z aplikacją, naciskając elementów na ekranie. Ponadto źródła danych można zmienić (lub można zmienić za pomocą aplikacji), a zawartość ekranu musi być zgodna z tych zmian. W poniższych sekcjach dowiesz się, jak obsługi zdarzeń kliknięcia elementu i aktualizować `RecyclerView` po zmianie w źródle danych.

<a name="itemclick" />

### <a name="handling-item-click-events"></a>Obsługa zdarzeń kliknięcia elementu

Gdy użytkownik dotyka element `RecyclerView`, by powiadomić aplikację, które zostało dotknięciu elementu jest generowane zdarzenie kliknięcia elementu. To zdarzenie nie jest generowany przez `RecyclerView` &ndash; zamiast tego widoku elementu (która jest ujęte w widoku posiadacz) wykrywa poprawek i raporty te poprawki, jak zdarzenia kliknięcia.

Aby zilustrować sposobu obsługi zdarzenia kliknięcia elementu, w poniższych krokach opisano, jak zmienić Podstawowa aplikacja wyświetlanie zdjęć raportu fotografii, który miał korzystały użytkownika. Gdy wystąpi zdarzenie kliknięcia elementu w przykładowej aplikacji, następująca sekwencja zdarzeń:

1.  Zdjęcie `CardView` wykryje Zdarzenie kliknięcia elementu i powiadamia karty.

2.  Karta przekazuje zdarzenia (z elementu informacji o położeniu) do obsługi kliknięcia elementu działania.

3.  Działania obsługi kliknięcia elementu reaguje na zdarzenia kliknięcia elementu.

Najpierw należy wywołać elementu członkowskiego program obsługi zdarzeń `ItemClick` jest dodawany do `PhotoAlbumAdapter` definicji klasy:

```csharp
public event EventHandler<int> ItemClick;
```

Następnie metoda obsługi zdarzeń kliknięcia elementu zostanie dodany do `MainActivity`.
Ten program obsługi wyświetla krótki wyskakujące, wskazujący, który element fotografii została dotknięciu:

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Następnie wiersz kodu jest wymagany do zarejestrowania `OnItemClick` obsługi z `PhotoAlbumAdapter`. Dobrym miejscem, w tym celu jest natychmiast po `PhotoAlbumAdapter` utworzeniu (w działaniu głównym `OnCreate` metody):

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

`PhotoAlbumAdapter` teraz wywoła `OnItemClick` po odebraniu zdarzenia kliknięcia elementu. Następnym krokiem jest utworzenie obsługi karty, który wywołuje to `ItemClick` zdarzeń. Następujące metody `OnClick`, natychmiast po karty `ItemCount` metody:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

To `OnClick` metoda jest karty *odbiornika* zdarzeń kliknięcia elementu z widoków elementu. Przed tym odbiorniku mogą być rejestrowane w widoku elementu (za pośrednictwem widoku elementu posiadacz widok), `PhotoViewHolder` Konstruktor należy zmodyfikować, aby zaakceptować tej metody jako dodatkowy argument, a następnie zarejestrować `OnClick` z widokiem elementu `Click` zdarzeń.
Oto zmodyfikowanych `PhotoViewHolder` konstruktora:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

`itemView` Parametr zawiera odwołanie do `CardView` który został dotknięciu przez użytkownika. Należy pamiętać, że klasa podstawowa posiadacz widoku zna pozycji układu elementu (`CardView`), który reprezentuje (za pośrednictwem `LayoutPosition` właściwości), a tej pozycji jest przekazywana do karty `OnClick` metody po wystąpieniu zdarzenia kliknięcia elementu. Karta `OnCreateViewHolder` metody są modyfikowane w celu przekazania karty `OnClick` metody do konstruktora posiadacz widoku:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Teraz podczas kompilacji i uruchamianie przykładowej aplikacji wyświetlanie zdjęć, naciskając zdjęcie wyświetlania spowoduje, że wyskakujące się pojawiać się, że aby raporty, które fotografii została dotknięciu:

[ ![Obszar jest wyskakujące przykładzie, który jest wyświetlany, gdy zdjęcie karty](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png)

W tym przykładzie pokazano tylko jeden ze sposobów wykonywania programów obsługi zdarzeń z `RecyclerView`. Innym rozwiązaniem, które mogą zostać użyte w tym miejscu jest umieszczenie zdarzenia właścicielowi widoku i karta subskrybowanie tych zdarzeń. W przypadku przykładowej aplikacji zdjęcie zdjęcie funkcje edycji odrębne zdarzenia jest wymagana dla `ImageView` i `TextView` w ramach każdej `CardView`: dotyczące `TextView` czy uruchomić `EditView` okna dialogowego, w którym użytkownik może edytować Podpis i poprawki na `ImageView` może uruchomić narzędzie Korygowanie fotografii, którym użytkownik może obracać zdjęcie lub Przytnij. W zależności od potrzeb aplikacji należy zaprojektować najlepszym podejściem w celu obsługi i reagowanie na zdarzenia touch.

Aby zademonstrować sposób `RecyclerView` mogą być aktualizowane podczas zmiany zestawu danych, przykładowej aplikacji wyświetlanie zdjęć można zmodyfikować, aby losowo Pobierz zdjęcie w źródle danych i Zamień pierwszego zdjęcia. Najpierw **pobranie losowej** zostanie dodany do przykładową aplikację fotografii **Main.axml** układu:

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

Następnie kod zostanie dodany na końcu działania głównego `OnCreate` metodą lokalizowania `Random Pick` znajdujący się w układzie i dołączenia do programu obsługi:

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

Ten program obsługi wymaga albumy fotografii `RandomSwap` metody podczas **losowe wybieranie** dotknięciu jest przycisk. `RandomSwap` — Metoda losowo zamienia zdjęć z pierwszego zdjęcia w źródle danych, a następnie zwraca indeks fotografii losowo zamienione. Gdy kompilowanie i uruchamianie przykładowej aplikacji o tym kodzie, naciskając **pobranie losowej** przycisku nie powoduje zmiany wyświetlania, ponieważ `RecyclerView` nie został powiadomiony o zmianie w źródle danych.

Aby zachować `RecyclerView` zaktualizowane po źródła danych zmian, **losowe wybieranie** kliknij przycisk obsługi należy zmodyfikować, aby wywołać karty `NotifyItemChanged` metody dla każdego elementu w kolekcji, które uległy zmianie (w tym przypadku dwóch elementów istnieją zmienione: pierwszy zdjęć i zamieniono fotografii). Powoduje to `RecyclerView` zaktualizować wyświetlanie, aby była spójna ze stanem z nowego źródła danych:

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

Teraz, gdy **pobranie losowej** wybrany przycisk `RecyclerView` aktualizuje widok, aby pokazać, że dalsze zdjęcie w dół w kolekcji została zapisana przy pierwszym zdjęcie w kolekcji:

[ ![Pierwszy zrzut ekranu przed wymiany, drugi zrzut ekranu po wymiany](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png)

Oczywiście `NotifyDataSetChanged` mógł zostać wywołany zamiast dwóch wywołań do `NotifyItemChanged`, ale spowoduje to spowodowałby `RecyclerView` odświeżyć całej kolekcji mimo że tylko dwa elementy w kolekcji zostały zmienione. Wywoływanie `NotifyItemChanged` jest znacznie bardziej efektywne niż wywołania `NotifyDataSetChanged`.


## <a name="related-links"></a>Linki pokrewne

- [RecyclerViewer (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Części RecyclerView i funkcji](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Podstawowy przykład RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
