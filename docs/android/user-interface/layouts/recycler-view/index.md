---
title: RecyclerView
description: RecyclerView jest grupą widoku do wyświetlania kolekcji; zaprojektowano go jako bardziej elastyczne zastępuje starsze wyświetlanie grup, takie jak ListView i GridView.  W tym przewodniku opisano sposób używania i dostosowywania RecyclerView w aplikacji platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/03/2018
ms.openlocfilehash: 187339244d53c154cc22672a3d2ceba7e0a75bcf
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView jest grupą widoku do wyświetlania kolekcji; zaprojektowano go jako bardziej elastyczne zastępuje starsze wyświetlanie grup, takie jak ListView i GridView.  W tym przewodniku opisano sposób używania i dostosowywania RecyclerView w aplikacji platformy Xamarin.Android._

## <a name="recyclerview"></a>RecyclerView

Wiele aplikacji konieczne jest wyświetlenie kolekcji tego samego typu (np. wiadomości, kontakty, obrazów lub utworów muzycznych); często tej kolekcji jest zbyt duży do ekranu, więc kolekcji są prezentowane w małe okno, które można sprawnie przewijać wszystkich elementów w kolekcji.
`RecyclerView` jest Android elementu widget, wyświetlająca kolekcję elementów listy lub siatkę, włączenie użytkownika przewijać kolekcji. Poniżej przedstawiono zrzut ekranu przedstawiający przykładową aplikację, która używa `RecyclerView` Aby wyświetlić zawartość skrzynki odbiorczej poczty e-mail w lista przewijana pionowo:

[![Przykładową aplikację za pomocą RecyclerView lista skrzynki odbiorczej wiadomości](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` oferuje dwie funkcje atrakcyjnych:

-  Ma ona elastyczną architekturę, która umożliwia modyfikowanie zachowania przez podłączenie składniki preferowanych sieci.

-  Jest efektywne z dużych kolekcjach, ponieważ ponownie używa widoków elementu i wymaga użycia *wyświetlić posiadaczy* do pamięci podręcznej odwołań widoku.

W tym przewodniku opisano sposób korzystania `RecyclerView` w aplikacji platformy Xamarin.Android; wyjaśniono, jak dodać `RecyclerView` pakietu do projektu platformy Xamarin.Android i opisano sposób `RecyclerView` funkcje w typowej aplikacji. Przedstawione przykłady rzeczywisty kod pokazano, jak zintegrować `RecyclerView` do aplikacji, kliknij widok elementu implementowania i jak odświeżyć `RecyclerView` podczas zmiany jej odpowiednie dane. W tym przewodniku założono, że czytelnik zna rozwoju platformy Xamarin.Android.


### <a name="requirements"></a>Wymagania

Mimo że `RecyclerView` jest często skojarzony z systemem Android 5.0 interfejs typu lizak, więc jest oferowany jako Biblioteka obsługi &ndash; `RecyclerView` współpracuje z aplikacjami tego poziomu docelowego interfejsu API (Android 2.1) 7 lub nowszy. Użycie wymaga następujących `RecyclerView` w aplikacjach opartych na platformie Xamarin:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 lub nowszej, musi być zainstalowana i skonfigurowana przy użyciu programu Visual Studio lub Visual Studio dla komputerów Mac.

-  Projekt aplikacji musi zawierać **Xamarin.Android.Support.v7.RecyclerView** pakietu. Aby uzyskać więcej informacji na temat instalowania pakietów NuGet, zobacz [wskazówki: w tym NuGet w projekcie](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


### <a name="overview"></a>Omówienie

`RecyclerView` można traktować jako zamiennik `ListView` i `GridView` elementy widget w systemie Android. Podobnie jak poprzednie wersje `RecyclerView` zaprojektowano w celu wyświetlenia dużych zestawów danych w oknie mały, ale `RecyclerView` oferuje dodatkowe opcje układu i jest zoptymalizowana do wyświetlania w dużych kolekcjach. Jeśli znasz `ListView`, istnieje kilka istotnych różnic między `ListView` i `RecyclerView`:

-   `RecyclerView` jest nieco bardziej skomplikowane do użycia: trzeba napisać kod więcej `RecyclerView` w porównaniu do `ListView`.

-   `RecyclerView` nie ma wstępnie zdefiniowane karty; musisz zaimplementować kod karty, który uzyskuje dostęp do źródła danych. Jednak Android zawiera kilka wstępnie zdefiniowanych kart, które działają z `ListView` i `GridView`.

-   `RecyclerView` nie oferuje Zdarzenie kliknięcia elementu, gdy użytkownik naciska elementu; Zamiast tego zdarzenia kliknięcia elementu są obsługiwane przez klasy pomocy. Z kolei `ListView` oferuje Zdarzenie kliknięcia elementu.

-   `RecyclerView` zwiększa wydajność przez odtwarzanie widoków i wymuszania wzorcu symbol zastępczy widoku eliminuje przypadków przeszukiwania zasobów niepotrzebnych układu. Użyj wzorca właściciela widoku jest opcjonalne w `ListView`.

-   `RecyclerView` jest oparta na modularny projekt, który ułatwia dostosować. Na przykład można dodać zasadę inny układ bez istotne zmiany kodu do aplikacji.
    Z kolei `ListView` jest stosunkowo wbudowanymi w strukturze.

-   `RecyclerView` zawiera wbudowane animacje dla elementu dodawania i usuwania. `ListView` animacje wymagają niektórych dodatkowego nakładu pracy ze strony Deweloper aplikacji.


### <a name="sections"></a>Sekcje

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[Części RecyclerView i funkcji](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

W tym temacie opisano sposób `Adapter`, `LayoutManager`, i `ViewHolder` współpracują jako klasy pomocy do obsługi `RecyclerView`.
Zawiera ogólne omówienie każdego z tych klas pomocnika i wyjaśniono, jak używać w aplikacji.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[Podstawowy przykład RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

W tym temacie opiera się na informacji dostępnych w [RecyclerView części i funkcji](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) zapewniając rzeczywisty kod przykłady różnych `RecyclerView` elementy zostaną zaimplementowane tworzenie aplikacji przeglądanie fotografii rzeczywistych.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[Rozszerzanie przykład RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

W tym temacie dodaje dodatkowy kod w aplikacji przykład przedstawionych w [A podstawowy przykład RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) celu zademonstrowania sposobu obsługi zdarzeń kliknięcia elementu i zaktualizuj `RecyclerView` po zmianie w źródle danych.


### <a name="summary"></a>Podsumowanie

Android wprowadzone w tym przewodniku `RecyclerView` elementu widget; go wyjaśniono, jak dodać `RecyclerView` biblioteki do projektów platformy Xamarin.Android obsługi jak `RecyclerView` odtwarzana widoków, jak wymusza wzorzec posiadacz widoku w celu zwiększenia wydajności i jak różnych klasy pomocy, które tworzą `RecyclerView` współpracę do wyświetlenia w kolekcji. Zapewniała przykładowy kod, aby zademonstrować sposób `RecyclerView` jest zintegrowany do aplikacji, jego wyjaśniono sposób dostosować `RecyclerView`jego układ zasad przez podłączenie menedżerów inny układ, a opisano sposób obsługi elementu zdarzenia kliknięcia i powiadom `RecyclerView`zmian źródła danych.

Aby uzyskać więcej informacji na temat `RecyclerView`, zobacz [odwołania do klasy RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html).


## <a name="related-links"></a>Linki pokrewne

- [RecyclerViewer (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Wprowadzenie do interfejs typu lizak](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
