---
title: GridLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: a8a9735845139da700959caf3639defa6594f307
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="gridlayout"></a>GridLayout

`GridLayout` To nowa `ViewGroup` podklasy obsługującego układania widoków w siatce 2D podobne do tabeli HTML, jak pokazano poniżej:

 [![Przycięty GridLayout wyświetlanie czterech komórek](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` współpracuje z hierarchii płaski widok, w których podrzędny widoków wartość ich lokalizacji w siatce, określając wiersze i kolumny, które powinny być w. W ten sposób *GridLayout* jest umieszczany widoków w siatce, bez konieczności takie, jak pokazano w wierszach tabeli używana w TableLayout że wszystkie widoki pośredniego zapewniają struktury tabeli. Dzięki utrzymywaniu hierarchii płaskiej *GridLayout* jest w stanie więcej szybko układu widokach podrzędnych. Spójrzmy na przykład w celu zilustrowania koncepcji faktycznie znaczenia w kodzie.


## <a name="creating-a-grid-layout"></a>Tworzenie układu tabelarycznego

Następujący kod XML dodaje kilka `TextView` formanty *GridLayout*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip" />
</GridLayout>
```

Układ dopasuje rozmiary wierszy i kolumn tak, aby komórki zmieści się ich zawartość, jak pokazano na poniższym diagramie:

 [![Diagram przedstawiający dwa komórek po lewej stronie mniejszy niż po prawej stronie układu](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

Powoduje to w interfejsie użytkownika następujące przy uruchamianiu aplikacji:

 [![Zrzut ekranu GridLayoutDemo aplikacji wyświetlanie czterech komórek](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)



## <a name="specifying-orientation"></a>Określanie orientacji

Zwróć uwagę, w formacie XML powyżej, każdy `TextView` nie określa wiersz lub kolumnę. Jeśli nie są one określone, `GridLayout` przypisuje każdy widok podrzędny w kolejności, bazując na orientacji. Na przykład Zmieńmy orientacji GridLayout z domyślnej, poziomy, do pionowego, jak to:

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

Teraz `GridLayout` będzie umieść komórek od góry do dołu w każdej kolumnie, zamiast od lewej do prawej, jak pokazano poniżej:

 [![Diagram pokazujący położenie komórek w orientacji pionowej](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

Powoduje to interfejsu użytkownika w czasie wykonywania:

 [![Zrzut ekranu GridLayoutDemo z komórek znajduje się w orientacji pionowej](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)



### <a name="specifying-explicit-position"></a>Określanie jawnie określonych pozycji

Jeśli chcemy, aby jawnie kontrolować pozycji widoków podrzędnych `GridLayout`, firma Microsoft może ustawić ich `layout_row` i `layout_column` atrybutów. Na przykład następujący kod XML spowoduje układ wskazany w pierwszym zrzut ekranu (pokazano powyżej), niezależnie od orientacji.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="1" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="1"  />
</GridLayout>
```



### <a name="specifying-spacing"></a>Określanie odstępów

Mamy kilka opcji, które zapewni odstępy między elementem podrzędnym widoki `GridLayout`. Możemy użyć `layout_margin` atrybutu, aby ustawić margines na każdy widok podrzędny bezpośrednio, jak pokazano poniżej

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

Ponadto w systemie Android 4 nowy widok ogólnego przeznaczenia odstępy o nazwie `Space` jest teraz dostępna. Aby go użyć, po prostu dodaj go jako widok podrzędny.
Na przykład poniżej XML dodaje dodatkowy wiersz do `GridLayout` przez ustawienie jej `rowcount` 3 i dodaje `Space` widoku, który zapewnia odstępy między `TextViews`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="3"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"         
            android:layout_height="50dp" />    
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="1" />
</GridLayout>
```

Plik XML tworzy odstęp w `GridLayout` w sposób przedstawiony poniżej:

 [![Zrzut ekranu pokazujący większe komórki z odstępami GridLayoutDemo](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

Zaletą używania nowego `Space` widok jest umożliwia odstępy i nie wymaga nam można ustawić atrybutów na każdy widok podrzędny.



### <a name="spanning-columns-and-rows"></a>Łączenie węzłów kolumn i wierszy

`GridLayout` Obsługuje również komórki obejmujące wiele kolumn i wierszy. Na przykład możemy dodać inny wiersz zawiera przycisk, aby `GridLayout` w sposób przedstawiony poniżej:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="4"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"        
            android:layout_height="50dp" />   
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"        
            android:layout_row="2"        
            android:layout_column="1" />
     <Button
            android:id="@+id/myButton"
            android:text="@string/hello"        
            android:layout_row="3"
            android:layout_column="0" />
</GridLayout>
```

Spowoduje to w pierwszej kolumnie `GridLayout` jest rozciągany tak, aby zmieścił się przycisku, jak przedstawiono poniżej:

[![Zrzut ekranu GridLayoutDemo z przyciskiem spanning tylko pierwsza kolumna](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

Aby w pierwszej kolumnie rozciąganie, możemy skonfigurować przycisk zakresu dwie kolumny przez ustawienie jej columnspan w następujący sposób:

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

W ten sposób powstaje w układzie dla `TextViews` przypomina do układu było wcześniej, przy użyciu przycisku dodane do dołu `GridLayout` w sposób przedstawiony poniżej:

 [![Zrzut ekranu GridLayoutDemo z przyciskiem obejmujących zarówno kolumny](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)


## <a name="related-links"></a>Linki pokrewne

- [GridLayoutDemo (przykład)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [Wprowadzenie Sandwich lodów](http://www.android.com/about/ice-cream-sandwich/)
- [Platformy systemu android 4.0](http://developer.android.com/sdk/android-4.0.html)
