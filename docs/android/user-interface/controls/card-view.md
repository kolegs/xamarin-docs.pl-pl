---
title: CardView
description: -Cardview to składnik interfejsu użytkownika, który przedstawia zawartość tekstowych i obrazów w widokach, które przypominają kart. W tym przewodniku opisano sposób używania i dostosowywania CardView w aplikacji platformy Xamarin.Android, zachowując zgodność z wcześniejszymi wersjami systemu android.
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 21e2a2e8ef04936664344cb4fb758bc2af3b4d05
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="cardview"></a>CardView

_-Cardview to składnik interfejsu użytkownika, który przedstawia zawartość tekstowych i obrazów w widokach, które przypominają kart. W tym przewodniku opisano sposób używania i dostosowywania CardView w aplikacji platformy Xamarin.Android, zachowując zgodność z wcześniejszymi wersjami systemu android._


## <a name="overview"></a>Omówienie

`Cardview` Widget, wprowadzona w systemie Android 5.0 (interfejs typu lizak), jest składnik interfejsu użytkownika, który przedstawia zawartość tekstowych i obrazów w widokach, które przypominają kart. `CardView` jest zaimplementowany jako `FrameLayout` elementu widget zaokrąglone narożniki i cienia. Zazwyczaj `CardView` służy do prezentowania elementu pojedynczy wiersz w `ListView` lub `GridView` grupy widoku. Na przykład poniższy zrzut ekranu jest przykładem aplikacji rezerwacji podróży, który implementuje `CardView`— na podstawie kart docelowego podróży w przewijanego `ListView`:

![Przykładową aplikację przy użyciu CardView dla każdej lokalizacji docelowej w podróży](card-view-images/01-cardview-example.png)

W tym przewodniku wyjaśniono sposób dodawania `CardView` pakietu do Twojej platformy Xamarin.Android projektu, jak dodać `CardView` układ i jak dostosować wygląd `CardView` w aplikacji. Ponadto ten przewodnik zawiera szczegółową listę `CardView` atrybuty, które można zmienić, łącznie z atrybutami ułatwiają `CardView` na systemu android w wersjach wcześniejszych niż Android 5.0 interfejs typu lizak.

<a name="requirements" />

## <a name="requirements"></a>Wymagania

Musisz wybrać nowe Android 5.0 i nowszych funkcji następujące (w tym `CardView`) w aplikacji opartych na platformie Xamarin:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 lub nowszej, musi być zainstalowana i skonfigurowana przy użyciu programu Visual Studio lub Visual Studio dla komputerów Mac.

-  **Zestaw SDK systemu android** &ndash; Android 5.0 (interfejs API 21) lub nowszym należy zainstalować za pomocą Menedżera zestawu SDK systemu Android.

-  **Java JDK 1.8** &ndash; JDK 1.7 mogą być używane, gdy są w szczególności interfejsu API wskazuje poziom 23 i wcześniejszych. JDK 1.8 jest dostępna z [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Aplikacja musi także obejmować `Xamarin.Android.Support.v7.CardView` pakietu. Aby dodać `Xamarin.Android.Support.v7.CardView` pakietu w programie Visual Studio dla komputerów Mac:

1. Otwórz projekt, kliknij prawym przyciskiem myszy **pakiety** i wybierz **Dodawanie pakietów**.

2. W **Dodawanie pakietów** okno dialogowe, wyszukaj **CardView**.

3. Wybierz **Biblioteka obsługi platformy Xamarin w wersji 7 CardView**, następnie kliknij przycisk **Dodaj pakiet**.

Aby dodać `Xamarin.Android.Support.v7.CardView` pakietu w programie Visual Studio:

1. Otwórz projekt, kliknij prawym przyciskiem myszy **odwołania** węzeł (w **Eksploratora rozwiązań** okienko) i wybierz **Zarządzaj pakietami NuGet...** .

2. Gdy **Zarządzaj pakietami NuGet** zostanie wyświetlone okno dialogowe, wprowadź **CardView** w polu wyszukiwania.

3. Gdy **Biblioteka obsługi platformy Xamarin w wersji 7 CardView** pojawia się, kliknij przycisk **zainstalować**.

Aby dowiedzieć się, jak skonfigurować projekt aplikacji systemu Android 5.0, zobacz [ustawienie się projekt systemu Android 5.0](~/android/platform/lollipop.md).
Aby uzyskać więcej informacji na temat instalowania pakietów NuGet, zobacz [wskazówki: w tym NuGet w projekcie](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


## <a name="introducing-cardview"></a>Wprowadzenie do CardView

Wartość domyślna `CardView` podobny białe karty z co najmniej zaokrąglone narożniki i nieznaczne cienia. Poniższy przykład **Main.axml** układ wyświetla pojedynczy `CardView` elementu widget, który zawiera `TextView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal">
        <TextView
            android:text="Basic CardView"
            android:layout_marginTop="0dp"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center"
            android:layout_centerVertical="true"
            android:layout_alignParentRight="true"
            android:layout_alignParentEnd="true" />
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Jeśli używasz tego kodu XML, aby zastąpić istniejącą zawartość elementu **Main.axml**, pamiętaj przekształcić w komentarz kod z dowolnego **MainActivity.cs** odwołujący się do zasobów w poprzednim XML.

W tym przykładzie układu tworzy domyślne `CardView` z jednego wiersza tekstu, jak pokazano na poniższym zrzucie ekranu:

[![Zrzut ekranu CardView białe tło i wiersz tekstu](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

W tym przykładzie styl aplikacji ustawiono jasny motyw materiału (`Theme.Material.Light`), aby `CardView` cieni i krawędzi są lepiej widoczne. Aby uzyskać więcej informacji o aplikacjach motywów Android 5.0, zobacz [motyw materiału](~/android/user-interface/material-theme.md). W następnej sekcji możemy dowiedzieć się, jak dostosować `CardView` dla aplikacji.


## <a name="customizing-cardview"></a>Dostosowywanie CardView

Można zmodyfikować podstawowego `CardView` atrybutów, aby dostosować wygląd `CardView` w aplikacji. Na przykład podniesienie `CardView` można zwiększyć można rzutować większych cienia (dzięki czemu karty prawdopodobnie float wyższej powyżej tła). Ponadto można zwiększyć promień narożnika dokonanie narożników więcej kart zaokrąglona.

W następnym przykładzie układu, dostosowany `CardView` służy do tworzenia symulację fotografii wydruku ("migawkę"). `ImageView` Jest dodawany do `CardView` do wyświetlania obrazu, a `TextView` znajduje się poniżej `ImageView` do wyświetlania tytułu obrazu.
W tym układzie przykład `CardView` ma następujące modyfikacje:

-  `cardElevation` Zostaje zwiększona do 4dp można rzutować większych cienia.
-  `cardCornerRadius` Zostaje zwiększona do 5dp dokonanie pojawiają się bardziej zaokrąglone narożniki.

Ponieważ `CardView` jest udostępniane przez biblioteki obsługi systemu Android w wersji 7, jego atrybuty nie są dostępne w `android:` przestrzeni nazw. W związku z tym musi definiować własne przestrzeni nazw XML i użyć tej przestrzeni nazw jako `CardView` prefiks atrybutu. W poniższym przykładzie układ użyjemy tego wiersza, aby zdefiniować przestrzeni nazw o nazwie `cardview`:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

Można wywołać tej przestrzeni nazw `card_view` lub nawet `myapp` wybranie opcji (nie jest dostępny tylko w zakresie tego pliku). Niezależnie od wybierz do wywołania tej przestrzeni nazw, należy użyć go jako prefiks `CardView` atrybut, który chcesz zmodyfikować. W tym przykładzie układu `cardview` przestrzeń nazw jest prefiks `cardElevation` i `cardCornerRadius`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal"
        cardview:cardElevation="4dp"
        cardview:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="fill_parent"
            android:layout_height="240dp"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="fill_parent"
                android:layout_height="190dp"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="fill_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Photo Title"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="5dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Jeśli w tym przykładzie układ jest używany do wyświetlania obrazu w aplikacji wyświetlanie zdjęć, `CardView` ma wygląd migawkę fotografii, jak pokazano na poniższym zrzucie ekranu:

[![CardView z obrazu i opisach obrazu](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

Tego zrzutu ekranu jest pobierana z [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer) przykładowej aplikacji, który używa `RecyclerView` elementu widget do prezentowania przewijanej listy `CardView` obrazów do wyświetlania zdjęcia. Aby uzyskać więcej informacji na temat `RecyclerView`, zobacz [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) przewodnik.

Zwróć uwagę, że `CardView` można wyświetlić więcej niż jeden widok podrzędny w obszarze jego zawartości. Na przykład, na przykład aplikacji wyświetlanie zdjęciu powyżej, obszar zawartości składa się z `ListView` zawierający `ImageView` i `TextView`. Mimo że `CardView` wystąpień często są ułożone pionowo, można także porządkować je w poziomie (zobacz [tworzenie niestandardowy widok styl](~/android/user-interface/material-theme.md#customview) na zrzucie ekranu).


### <a name="cardview-layout-options"></a>Opcje układu CardView

`CardView` układy może zostać dostosowane przez ustawienie jeden lub więcej atrybutów, które mają wpływ na dopełnienie, podniesienia uprawnień, promień narożnika i kolor tła:

[![Diagram CardView atrybutów](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

Każdy atrybut można zmienić dynamicznie przez wywołanie metody odpowiednik `CardView` — metoda (Aby uzyskać więcej informacji na temat `CardView` metod, zobacz [odwołania do klasy CardView](https://developer.android.com/reference/android/support/v7/widget/CardView.html)).
Należy pamiętać, że te atrybuty (z wyjątkiem kolor tła) zaakceptuj wartości wymiaru, która jest liczbą dziesiętną, a po niej jednostkę. Na przykład `11.5dp` określa 11,5 pikselach niezależnych od gęstości.


#### <a name="padding"></a>Dopełnienie
`
CardView` oferuje pięciu atrybutów dopełnienie położenie zawartości w ramach karty. Można je ustawić w układzie XML lub analogicznych metody można wywołać w kodzie:

[![Diagram CardView dopełnienie atrybutów](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

Atrybuty dopełnienie opisano szczegółowo w następujący sposób:

-  `contentPadding` &ndash; Dopełnienie między widokami podrzędnych wewnętrzny `CardView` i wszystkich krawędzi karty.

-  `contentPaddingBottom` &ndash; Dopełnienie między widokami podrzędnych wewnętrzny `CardView` i dolną krawędzią karty.

-  `contentPaddingLeft` &ndash; Dopełnienie między widokami podrzędnych wewnętrzny `CardView` a lewą krawędzią karty.

-  `contentPaddingRight` &ndash; Dopełnienie między widokami podrzędnych wewnętrzny `CardView` i prawej krawędzi karty.

-  `contentPaddingTop` &ndash; Dopełnienie między widokami podrzędnych wewnętrzny `CardView` a górną krawędzią karty.

Atrybuty uzupełnienie zawartości są względem granic obszaru zawartości, a nie na dowolnym danego elementu widget znajduje się wewnątrz obszaru zawartości.
Na przykład jeśli `contentPadding` wystarczająco wzroście w aplikacji wyświetlanie zdjęć `CardView` czy przyciąć tekst wyświetlany na karcie i obrazu.



#### <a name="elevation"></a>Podniesienie uprawnień

`CardView` oferuje dwa atrybuty podniesienia uprawnień do sterowania jego podniesienia uprawnień, co w efekcie, rozmiar jego cień:

[![Diagram CardView atrybutów podniesienia uprawnień](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

Atrybuty podniesienia uprawnień opisano szczegółowo w następujący sposób:

-  `cardElevation` &ndash; Podwyższenie poziomu `CardView` (reprezentuje osi Z).

-  `cardMaxElevation` &ndash; Maksymalna wartość `CardView`dla podniesienia uprawnień.

Większe wartości `cardElevation` Zwiększ rozmiar cienia, aby `CardView` prawdopodobnie float wyższej powyżej tła. `cardElevation` Atrybut określa również kolejność rysowania nakładających się widoków; `CardView` będzie rysowany innego widoku nakładające się przy użyciu nowszej ustawienia podniesienia uprawnień i powyżej wszelkich nakładających się widoków z na niższy podniesienia uprawnień.
`cardMaxElevation` Ustawienie jest przydatne w przypadku aplikacji zmiany podniesienia uprawnień dynamicznie &ndash; uniemożliwia cień rozszerzanie minął limit definiujący tego ustawienia.


#### <a name="corner-radius-and-background-color"></a>Promień narożnika i kolor tła

`CardView` udostępnia Atrybuty, których można kontrolować jego promień narożnika i jego kolor tła. Te dwie właściwości umożliwia zmianę ogólnej stylu `CardView`:

[![Diagram rogu CardView radious i atrybutów koloru tła](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

Te atrybuty zostały wyjaśnione w następujący sposób:

-  `cardCornerRadius` &ndash; Promień narożnika wszystkie narożniki `CardView`.

-  `cardBackgroundColor` &ndash; Kolor tła `CardView`.

Na tym diagramie `cardCornerRadius` ustawiono więcej zaokrąglony 10dp i `cardBackgroundColor` ma ustawioną wartość `"#FFFFCC"` (żółty światła).


## <a name="compatibility"></a>Zgodność

Można użyć `CardView` na systemu android w wersjach wcześniejszych niż Android 5.0 interfejs typu lizak. Ponieważ `CardView` jest częścią biblioteki obsługi systemu Android w wersji 7, można użyć `CardView` z 2.1 systemu Android (interfejs API na poziomie 7) lub nowszym.
Jednak należy zainstalować `Xamarin.Android.Support.v7.CardView` pakietu zgodnie z opisem w [wymagania](#requirements)powyżej.

`CardView` wykazuje nieco inaczej na urządzeniach przed interfejs typu lizak (poziom interfejsu API 21):

-  `CardView` używa implementacji programowe tle, który dodaje dodatkowe dopełnienia.

-  `CardView` nie obcina widoki podrzędne, które intersect z `CardView`obiektu zaokrąglone narożniki.

Aby pomóc w zarządzaniu tych różnic zgodności `CardView` udostępnia kilka dodatkowych atrybutów, które można skonfigurować w układzie:

-   `cardPreventCornerOverlap` &ndash; Ten atrybut na `true` można dodać dopełnienie, gdy aplikacja działa w starszych wersjach systemu Android (poziom interfejsu API 20 i starszych). To ustawienie uniemożliwia `CardView` zawartości z przecinające się z `CardView`obiektu zaokrąglone narożniki.

-   `cardUseCompatPadding` &ndash; Ten atrybut na `true` można dodać dopełnienie, gdy aplikacja działa w wersji dla systemu Android w lub większa niż poziom interfejsu API 21. Jeśli chcesz użyć `CardView` na urządzeniach wstępne interfejs typu lizak i wyglądu interfejs typu lizak (lub nowsza), ustaw ten atrybut `true`. Po włączeniu tego atrybutu `CardView` dodaje dodatkowe uzupełnienia do rysowania cieni, gdy jest uruchamiany na urządzeniach wstępne interfejs typu lizak. Pozwala to naprawiać ewentualne różnice w dopełnienie, które zostaną wprowadzone po implementacji tle programowe wstępne interfejs typu lizak.

Aby uzyskać więcej informacji dotyczących zachowania zgodności z wcześniejszymi wersjami systemu android, zobacz [utrzymania zgodności](https://developer.android.com/training/material/compatibility.html).


## <a name="summary"></a>Podsumowanie

W tym przewodniku wprowadzono nowe `CardView` widget zawarte w systemie Android 5.0 (interfejs typu lizak). Wykazać, domyślnie `CardView` wygląd i wyjaśniono sposób dostosowywania `CardView` zmieniając jej podniesienia uprawnień, zaokrąglenie narożników zawartości uzupełnienie i kolor tła. Pojawią się `CardView` atrybuty układu (przy użyciu diagramów odwołania) i wyjaśniono sposób użycia `CardView` urządzeń z systemem Android wcześniejszych niż Android 5.0 interfejs typu lizak. Aby uzyskać więcej informacji na temat `CardView`, zobacz [odwołania do klasy CardView](https://developer.android.com/reference/android/support/v7/widget/CardView.html).


## <a name="related-links"></a>Linki pokrewne

- [RecyclerView (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Wprowadzenie do interfejs typu lizak](~/android/platform/lollipop.md)
- [Odwołania do klasy CardView](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
