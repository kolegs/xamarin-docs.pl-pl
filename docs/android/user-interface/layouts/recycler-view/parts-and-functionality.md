---
title: Obiektu RecyclerView elementy i funkcje
description: Omówienie Menedżera układu obiektu RecyclerView, karta i właściciela widoku.
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/13/2018
ms.openlocfilehash: 4d55124e9a02489d1f55e900c537939ff3450509
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038511"
---
# <a name="recyclerview-parts-and-functionality"></a>Obiektu RecyclerView elementy i funkcje


`RecyclerView` Niektóre zadania wewnętrznie (takie jak przewijanie i odtwarzanie widoków), ale jej uchwyty to zasadniczo Menedżer koordynującego klas pomocniczych na potrzeby wyświetlania kolekcji. `RecyclerView` Deleguje zadania do następujących klas pomocniczych:

-   **`Adapter`** &ndash; Zwiększa układów z elementu (tworzy zawartość pliku układu) i tworzy powiązanie danych z widoków, które są wyświetlane w ramach `RecyclerView`. Karta raporty również zdarzeń kliknięcia elementu.

-   **`LayoutManager`** &ndash; Mierzy i określa położenie elementu widokach w ramach `RecyclerView` i zarządza zasadami recyklingu widoku.

-   **`ViewHolder`** &ndash; Wyszukuje i przechowuje odwołania do widoku. Posiadacz widok pomaga również w wykrywaniu kliknięć widoku elementu.

-   **`ItemDecoration`** &ndash; Umożliwia aplikacji można dodać specjalnego przesunięcia związane z rysowaniem i układu do określonych widoków rysowania separator między elementami, najważniejsze funkcje i granice wizualne grupowanie.

-   **`ItemAnimator`** &ndash; Definiuje animacji, które mają miejsce w akcji elementu lub jako zmiany zostały wprowadzone do karty sieciowej.

Relacja między `RecyclerView`, `LayoutManager`, i `Adapter` klasy jest przedstawiony na poniższym diagramie:

![Diagram przedstawiający obiektu RecyclerView zawierający LayoutManager, otwieranie zestawu danych za pomocą karty](parts-and-functionality-images/01-recyclerview-diagram.png)

Jak poniższy rysunek przedstawia `LayoutManager` można traktować jako pośrednik między `Adapter` i `RecyclerView`. `LayoutManager` Sprawia, że wywołania do `Adapter` metody w imieniu osób `RecyclerView`. Na przykład `LayoutManager` wywołania `Adapter` metody, gdy nadejdzie czas, aby utworzyć nowy widok na potrzeby w miejsce elementu `RecyclerView`. `Adapter` Zwiększa układu dla tego elementu, a następnie tworzy `ViewHolder` wystąpienia (niewyświetlany) do pamięci podręcznej odwołań do widoków w tej pozycji. Gdy `LayoutManager` wywołania `Adapter` powiązać określonego elementu z zestawu danych `Adapter` lokalizuje dane dla tego elementu, pobiera je z zestawu danych i kopiuje go do widoku skojarzonego elementu.

Korzystając z `RecyclerView` w swojej aplikacji, wymagana jest tworzenia typów pochodnych następujące klasy:

-   **`RecyclerView.Adapter`** &ndash; Zawiera powiązanie z zestawu danych aplikacji (czyli specyficzne dla aplikacji) do widoków elementów, które są wyświetlane w ramach `RecyclerView`. Karta wie, jak skojarzyć każdej pozycji elementu widoku w `RecyclerView` do określonej lokalizacji w źródle danych. Ponadto karta obsługuje układ zawartości w ramach każdego pojedynczego elementu widoku i tworzy właściciela widoku dla każdego widoku. Karta raporty również zdarzenia kliknięcia elementu, które są wykrywane przez widok elementu.

-   **`RecyclerView.ViewHolder`** &ndash; Buforuje odwołania do widoków w pliku układu elementu, tak aby przypadków przeszukiwania zasobów nie powtarzają się niepotrzebnie. Symbol zastępczy widok również rozmieszcza zdarzeń przedmiot, kliknij były przekazywane do adaptera, kiedy użytkownik naciska właściciela widoku skojarzonego elementu widoku.

-   **`RecyclerView.LayoutManager`** &ndash; Określa położenie elementów w obrębie `RecyclerView`. Można użyć jednej z kilku menedżerów wstępnie zdefiniowany układ, lub możesz zaimplementować menedżera układu niestandardowego.
    `RecyclerView` delegaty zasad układu do menedżera układu, dzięki czemu możesz podłączyć w Menedżerze inny układ bez konieczności wprowadzania istotne zmian do aplikacji.

Ponadto opcjonalnie rozszerzono następujące klasy, aby zmienić wygląd i działanie `RecyclerView` w swojej aplikacji:

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

Jeśli nie zostanie rozszerzony `ItemDecoration` i `ItemAnimator`, `RecyclerView` używa domyślnej implementacji. Ten przewodnik nie wyjaśniono, jak utworzyć niestandardowe `ItemDecoration` i `ItemAnimator` klasy; Aby uzyskać więcej informacji na temat tych klas, zobacz [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) i [RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>Jak wyświetlić odtwarzanie działa

`RecyclerView` nie przydziela widoku elementu dla każdego elementu w źródle danych. Zamiast tego należy przydziela tylko liczbę widoków elementów, które mieszczą się na ekranie i układy tych elementów, gdy użytkownik przewija ponownie używane. Gdy widok najpierw Przewija psuje, przechodzi on przez proces odtwarzania zilustrowane na poniższym rysunku:

[![Diagram pokazujący sześć kroków recyklingu widoku](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1.  Widok Przewija psuje i nie będzie już wyświetlany, staje się *złom widoku*.

2.  Wyświetl braków znajduje się w puli i staje się *odtwarzanie widoku*.
    Ta pula jest pamięć podręczna widoków, które wyświetlają ten sam typ danych.

3.  Gdy nowy element ma być wyświetlana, widok jest pobierana z puli odtwarzanie do ponownego wykorzystania. Ponieważ ten widok musi być ponownie powiązana przez kartę przed wyświetleniem, jest nazywany *zakłóconych widoku*.

4.  Widok zanieczyszczone zostanie odtworzona: karta lokalizuje dane dla następnego elementu mają być wyświetlane i kopiuje dane z widokami dla tego elementu. Odwołania do tych widoków są pobierane z właściciela widoku skojarzonego z widokiem odtworzona.

5.  Widok odtwarzania zostanie dodany do listy elementów w `RecyclerView` który zbliża się przejść na ekranie.

6.  Widok odtwarzania przechodzi na ekranie jako użytkownik przewija `RecyclerView` do następnego elementu na liście. W międzyczasie innego widoku Przewija psuje i odtwarzania zgodnie z powyższych kroków.

Oprócz ponownemu widoku elementu `RecyclerView` używa także innego Optymalizacja wydajności: Wyświetl właścicieli. A *właściciela widoku* jest klasą prostą, pamięci podręcznych wyświetlenia odwołań. Za każdym razem, karta zwiększa plik układu elementu, a także tworzy odpowiedniego właściciela widoku. Symbol zastępczy widoku używa `FindViewById` można pobrać odwołań do widoków wewnątrz pliku nadmuchany układu elementu. Te odwołania są używane do ładowania danych nowych widoków za każdym razem, gdy układ jest odtwarzania, aby wyświetlić nowe dane.
 


### <a name="the-layout-manager"></a>Menedżer układu

Menedżer układ jest odpowiedzialny za pozycjonowanie elementów w `RecyclerView` wyświetlić; Określa typ prezentacji (lista lub Siatka), orientacja (czy elementy są wyświetlane w pionie lub w poziomie) i elementy kierunku, które powinny być wyświetlane (w normalnej kolejności lub w odwrotnej kolejności). Menedżer układ jest również odpowiedzialny za obliczenia rozmiaru i położenia każdego elementu w **RecycleView** wyświetlania.

Menedżer układ ma dodatkowe cel: Określa zasady dotyczące Odtwórz widoki elementów, które nie są już widoczne dla użytkownika.
Ponieważ Menedżer układ jest korzystają z widoków, które są widoczne (i nie są), to najlepsze możliwości do określania, kiedy widok może zostać odtworzona. Odtwarzanie widoku, Menedżera układu zwykle wykonywania wywołań do karty do Zastąp zawartość widoku odtwarzania z różnymi danymi, jak opisano wcześniej w [sposób działania odtwarzanie widoku](#recycling).

Możesz rozszerzyć `RecyclerView.LayoutManager` tworzenie własnych układów menedżera lub użyć wstępnie zdefiniowany układ menedżera. `RecyclerView` udostępnia następujące menedżerów wstępnie zdefiniowanego układu:

-   **`LinearLayoutManager`** &ndash; Rozmieszcza elementy w kolumnie, która może być przewijane w pionie lub w wierszu, który może być przewijane w poziomie.

-   **`GridLayoutManager`** &ndash; Wyświetla elementy w siatce.

-   **`StaggeredGridLayoutManager`** &ndash; Wyświetla elementy w siatce samodzielnym, w której niektóre elementy mają różnej wysokości i szerokości.

Aby określić menedżera układu, Utwórz wystąpienie Twojego Menedżera wybrany układ i przekazać ją do `SetLayoutManager` metody. Należy pamiętać, *musi* określić układ Menedżera &ndash; `RecyclerView` nie wybierze Menedżera wstępnie zdefiniowany układ domyślny.

Aby uzyskać więcej informacji na temat Menedżera układu, zobacz [odwołań do klas RecyclerView.LayoutManager](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html).


### <a name="the-view-holder"></a>Symbol zastępczy widoku

Symbol zastępczy widok jest klasę, która jest zdefiniowana dla pamięci podręcznej odwołań do widoku. Karta używa tych odwołań widoku można powiązać każdy widok do jego zawartości. Każdy element `RecyclerView` ma skojarzony widok wystąpienia posiadacza buforującego odwołania widok dla tego elementu. Aby utworzyć właściciela widoku, umożliwia definiowanie klasy, aby pomieścić dokładny zestaw widoków na element następujące czynności:

1.  Podklasy `RecyclerView.ViewHolder`.
2.  Należy zaimplementować konstruktora, który wyszukuje i przechowuje odwołania do widoku.
3.  Implementuje właściwości, które karty umożliwiają dostęp do tych odwołań.

Szczegółowy przykład `ViewHolder` wdrożenia są prezentowane w [podstawowy przykład obiektu RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md).
Aby uzyskać więcej informacji na temat `RecyclerView.ViewHolder`, zobacz [odwołań do klas RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="the-adapter"></a>Karta

Większość "ciężkich podnoszenia" `RecyclerView` integracji kodu odbywa się na karcie. `RecyclerView` wymaga to dostarczenia adaptera pochodną `RecyclerView.Adapter` dostępu do źródła danych i wypełniania każdego elementu z zawartością ze źródła danych.
Ponieważ źródło danych jest specyficzny dla aplikacji, należy zaimplementować funkcje karty, która rozumie sposób uzyskiwać dostęp do danych. Karta wyodrębnia informacje ze źródła danych i ładuje je do każdego elementu w `RecyclerView` kolekcji.

Na poniższym rysunku przedstawiono sposób adapter mapowania zawartości w źródle danych za pośrednictwem widoku posiadaczy poszczególnych widokach w ramach każdego elementu wiersza `RecyclerView`:

[![Diagram pokazujący karty nawiązywania połączenia ViewHolders źródła danych](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

Karta ładuje każdego `RecyclerView` wiersz z danych dla elementu określonego wiersza. Dla pozycji wiersza *P*, na przykład karta lokalizuje powiązane dane w położeniu *P* w źródle danych i kopii tych danych do wiersza elementów w położeniu *P* w `RecyclerView` kolekcji.
Na powyższym rysunku, na przykład karta używa posiadacza widok do wyszukiwania odwołań dla `ImageView` i `TextView` na tej pozycji, dzięki czemu nie trzeba wywoływać wielokrotnie `FindViewById` tych widoków, jak użytkownik przewija kolekcji i ponownie używa widoków.

Podczas implementowania adapter, konieczne jest przesłonięcie następujących `RecyclerView.Adapter` metody:

-   **`OnCreateViewHolder`** &ndash; Tworzy wystąpienie właściciela pliku i widoku układu elementu.

-   **`OnBindViewHolder`** &ndash; Ładuje dane w określonej pozycji do widoków, w których odwołania są przechowywane w właściciela danego widoku.

-   **`ItemCount`** &ndash; Zwraca liczbę elementów w źródle danych.

Menedżer układ wywołuje te metody, gdy jest on pozycjonowanie elementów w obrębie `RecyclerView`. 



### <a name="notifying-recyclerview-of-data-changes"></a>Powiadamianie recyclerview wprowadzonych zmian danych

`RecyclerView` nie jest aktualizowane automatycznie wyświetlana jego po zawartości jego danych źródłowych zmiany. Karta musi powiadomić `RecyclerView` po zmiany w zestawie danych. Zestaw danych można zmienić na wiele sposobów; na przykład można zmienić zawartość elementu lub ogólną strukturę danych mogą ulec zmianie.
`RecyclerView.Adapter` udostępnia wiele metod, które można wywołać, aby `RecyclerView` reaguje na zmiany danych w sposób najbardziej efektywny sposób:

-  **`NotifyItemChanged`** &ndash; Sygnalizuje, że element w określonej pozycji został zmieniony.

-  **`NotifyItemRangeChanged`** &ndash; Sygnały, które uległy zmianie elementów w określonym zakresie, stanowiska.

-  **`NotifyItemInserted`** &ndash; Sygnały nowo wstawione elementu w określonej pozycji.

-  **`NotifyItemRangeInserted`** &ndash; Sygnały, że elementów w określonym zakresie, stanowiska nowo wstawiona.

-  **`NotifyItemRemoved`** &ndash; Sygnały usunięcie elementu w określonej pozycji.

-  **`NotifyItemRangeRemoved`** &ndash; Sygnały usunięcie elementów w określonym zakresie, stanowiska.

-  **`NotifyDataSetChanged`** &ndash; Sygnalizuje, że zestaw danych został zmieniony (wymusza pełnej aktualizacji).

Jeśli znasz się dokładnie, jak zestaw danych został zmieniony, możesz wywołać właściwe metody powyżej, aby odświeżyć `RecyclerView` w najbardziej efektywny sposób. Jeśli nie wiadomo dokładnie, jak zestaw danych został zmieniony, możesz wywołać `NotifyDataSetChanged`, co jest znacznie mniej wydajne ponieważ `RecyclerView` należy odświeżyć wszystkie widoki, które są widoczne dla użytkownika. Aby uzyskać więcej informacji o tych metodach, zobacz [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

W następnym temacie [podstawowy przykład obiektu RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), przykładowej aplikacji jest implementowane w celu wykazania, przykłady kodu rzeczywistych części i funkcji opisanych powyżej.


## <a name="related-links"></a>Linki pokrewne

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Podstawowy przykład obiektu RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [Rozszerzanie przykład obiektu RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
