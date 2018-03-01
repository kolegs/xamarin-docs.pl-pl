---
title: "Części RecyclerView i funkcji"
ms.topic: article
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b1ddcca25fd83a806e8383a5717462b518b46d0b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="recyclerview-parts-and-functionality"></a>Części RecyclerView i funkcji


`RecyclerView` Niektóre zadania wewnętrznie (takie jak przewijanie i odtwarzania widoków), może dojść jest zasadniczo Menedżera koordynująca klasy pomocy, aby wyświetlić kolekcji. `RecyclerView` Obiekty delegowane na następujących klas pomocniczych zadań:

-   **`Adapter`** &ndash; Układy elementu nadyma (tworzy zawartość pliku układu) i powiązanie danych z widoków, które są wyświetlane w `RecyclerView`. Karta również raporty zdarzeń kliknięcia elementu.

-   **`LayoutManager`** &ndash; Mierzy i umieszcza elementu widokach w ramach `RecyclerView` i zarządza zasadami recyklingu widoku.

-   **`ViewHolder`** &ndash; Wyszukuje i przechowuje odwołuje się do widoku. Symbol zastępczy widok pomaga również z wykrywania kliknięć widok elementu.

-   **`ItemDecoration`** &ndash; Umożliwia aplikacji dodać specjalne przesunięcia związane z rysowaniem i układu do konkretnych widoków rysowania separatorów między elementami, najważniejsze funkcje i granice grupowania visual.

-   **`ItemAnimator`** &ndash; Definiuje animacji, które mają miejsce w akcji elementu lub jako zmiany zostały wprowadzone do karty sieciowej.

Relacja między `RecyclerView`, `LayoutManager`, i `Adapter` klasy został przedstawiony na poniższym diagramie:

![Diagram RecyclerView zawierający LayoutManager, otwieranie zestawu danych za pomocą karty](parts-and-functionality-images/01-recyclerview-diagram.png)

Jak pokazano na poniższym rysunku, `LayoutManager` można traktować jako pośrednik między `Adapter` i `RecyclerView`. `LayoutManager` Wykonywania wywołań do `Adapter` metod w imieniu `RecyclerView`. Na przykład `LayoutManager` wywołania `Adapter` metody, gdy nadejdzie czas, aby utworzyć nowy widok dla danego elementu pozycji w `RecyclerView`. `Adapter` Nadyma układu dla tego elementu i tworzy `ViewHolder` wystąpienia (tego nie pokazano) do pamięci podręcznej odwołań do widoków na tej pozycji. Gdy `LayoutManager` wywołania `Adapter` powiązać danego elementu zestawu danych `Adapter` lokalizuje dane dla tego elementu, pobiera go z zestawu danych i kopiuje go do widoku skojarzonego elementu.

Korzystając z `RecyclerView` w aplikacji, wymagane jest tworzenie typów pochodnych następujących klas:

-   **`RecyclerView.Adapter`** &ndash; Udostępnia powiązania z zestawu danych aplikacji (która jest specyficzna dla aplikacji) do elementu widoków, które są wyświetlane w `RecyclerView`. Karta potrafi skojarzyć każdej pozycji widok elementu w `RecyclerView` do określonej lokalizacji w źródle danych. Ponadto adapter obsługuje układ zawartości w poszczególnych widokach pojedynczy element i tworzy posiadacz widoku dla każdego widoku. Karta również raporty zdarzeń kliknięcia elementu, które są wykrywane przez widok elementu.

-   **`RecyclerView.ViewHolder`** &ndash; Buforuje odwołania do widoków w pliku układu elementu, tak aby przypadków przeszukiwania zasobów nie są powtarzane niepotrzebnie. Symbol zastępczy widoku również rozmieszcza zdarzeń kliknięcia elementu do przekazania do adaptera po naciśnięciu widok skojarzony element zastępczy widoku.

-   **`RecyclerView.LayoutManager`** &ndash; Określa położenie elementów w obrębie `RecyclerView`. Można użyć jednej z kilku menedżerów układu wstępnie zdefiniowanych lub można zaimplementować Menedżera niestandardowego układu.
    `RecyclerView` Obiekty delegowane zasad układu z menedżerem układu, więc możesz podłączyć w Menedżerze inny układ bez wprowadzania istotne zmiany w aplikacji.

Ponadto można opcjonalnie rozszerzono następujących klas, aby zmienić wygląd i działanie `RecyclerView` w aplikacji:

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

Jeśli nie zostanie rozszerzony `ItemDecoration` i `ItemAnimator`, `RecyclerView` używa domyślnej implementacji. W tym przewodniku wyjaśniono, jak utworzyć niestandardowe `ItemDecoration` i `ItemAnimator` klas, aby uzyskać więcej informacji na temat tych klas, zobacz [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) i [RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>Jak wyświetlić odtwarzania działania

`RecyclerView` Widok elementu nie przydzielić dla każdego elementu w źródle danych. Zamiast tego przydzielania tylko liczbę elementu widoków, które mieszczą się na ekranie i układy tych elementów, gdy użytkownik przewija ponownie używane. Gdy widok najpierw przewinie niewidocznym, przechodzi ona przez proces odtwarzania pokazano na poniższej ilustracji:

[ ![Diagram pokazujący sześć kroków recyklingu widoku](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png)

1.  Widok Przewija niewidocznym i nie będzie już wyświetlany, staje się *złom widoku*.

2.  Widok braków znajduje się w puli i staje się *Odtwórz widoku*.
    Ta pula jest to pamięć podręczna widoków, które zawierają ten sam typ danych.

3.  Gdy nowy element ma być wyświetlany, widok jest pobierana z puli odtworzenia do ponownego użycia. Ponieważ ten widok musi zostać ponownie powiązany przez kartę przed wyświetleniem, jest nazywany *dirty widoku*.

4.  Widok zakłócone zostanie odtworzony: karta lokalizuje danych dla następnego elementu, który będzie wyświetlany i kopiuje dane z widokami dla tego elementu. Odwołania do tych widoków są pobierane z posiadacz widoku skojarzonego z widokiem odtwarzania.

5.  Widok odtwarzania zostanie dodany do listy elementów w `RecyclerView` będących przeprowadzona na ekranie.

6.  Widok odtwarzania przechodzi na ekranie gdy użytkownik przewija `RecyclerView` do następnego elementu na liście. W tym samym czasie innego widoku Przewija niewidocznym i odtwarzania zgodnie z powyższych kroków.

Oprócz ponownemu widok elementu `RecyclerView` również używa innego optymalizacji wydajności: Wyświetl posiadaczy. A *posiadacz widoku* jest klasą prostą, że pamięci podręcznych wyświetlić odwołania. Zawsze karta nadyma pliku układu elementu tworzy również odpowiedniego właściciela widoku. Symbol zastępczy widoku używa `FindViewById` można pobrać odwołań do widoków wewnątrz pliku nadmuchany układu elementu. Te odwołania są używane do ładowanie nowych danych do widoków każdorazowego układ jest przetworzony ponownie, aby wyświetlić nowe dane.
 

<a name="layoutmanager" />

### <a name="the-layout-manager"></a>Menedżer układu

Menedżer układ jest odpowiedzialny za pozycjonowanie elementów w `RecyclerView` wyświetlić; Określa typ prezentacji (listy lub Siatka), orientację (określa, czy elementy są wyświetlane w pionie lub poziomie) i kierunek elementy, które powinny być wyświetlane w normalnej kolejności lub w odwrotnej kolejności. Menedżer układu również jest odpowiedzialny za obliczenia rozmiaru i pozycji każdego elementu w **RecycleView** wyświetlania.

Menedżer układu ma dodatkowe cel: Określa zasady program do odtworzenia elementu widoków, które nie są już widoczne dla użytkowników.
Ponieważ Menedżer układu korzystają z widoków, które są widoczne (i które nie są), jest w pozycji najlepszych decyzji o tym, kiedy widok może zostać odtworzona. Do odtworzenia widoku, Menedżer układu zwykle wywołań do karty zastąpić zawartość widoku odtwarzania z wykorzystaniem różnych danych, jak opisano wcześniej w [sposób działania odtwarzania widoku](#recycling).

Można rozszerzyć `RecyclerView.LayoutManager` do utworzenia własnego układu menedżera, lub można użyć Menedżera wstępnie zdefiniowany układ. `RecyclerView` udostępnia następujące menedżerów wstępnie zdefiniowany układ:

-   **`LinearLayoutManager`** &ndash; Rozmieszcza elementy w kolumnie, która może być przewijana pionowo lub w wierszu, które mogą być przewijane w poziomie.

-   **`GridLayoutManager`** &ndash; Wyświetla elementy w siatce.

-   **`StaggeredGridLayoutManager`** &ndash; Wyświetla elementy w siatce okres, w której niektóre elementy mają różne wysokości i szerokości.

Aby określić menedżera układu, Utwórz wystąpienie Menedżera wybranego układu i przekaż go do `SetLayoutManager` metody. Uwaga które *musi* Określ menedżera układu &ndash; `RecyclerView` nie wybierz Menedżera wstępnie zdefiniowany układ domyślny.

Aby uzyskać więcej informacji na temat Menedżera układu, zobacz [odwołania do klasy RecyclerView.LayoutManager](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html).

<a name="viewholder" />

### <a name="the-view-holder"></a>Symbol zastępczy widoku

Symbol zastępczy widoku jest klasą dla buforowania odwołuje się do widoku. Karta korzysta te odwołania widoku można powiązać każdy widok do jego zawartości. Każdy element w `RecyclerView` ma skojarzony widok wystąpienia posiadacz buforującego odwołania widok dla tego elementu. Aby utworzyć właściciela widoku, umożliwiają definiowanie klasy, aby pomieścić dokładnie zestaw widoków dla każdego elementu następujące czynności:

1.  Podklasy `RecyclerView.ViewHolder`.
2.  Implementuje konstruktora, który wyszukuje i przechowuje odwołuje się do widoku.
3.  Implementowanie właściwości, które adapter umożliwia dostęp do tych odwołań.

Szczegółowy przykład `ViewHolder` wdrażania jest przedstawiona w [A podstawowy przykład RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md).
Aby uzyskać więcej informacji na temat `RecyclerView.ViewHolder`, zobacz [odwołania do klasy RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).

<a name="adapter" />

### <a name="the-adapter"></a>Karta

Większość "ciężki podnoszenia" `RecyclerView` kodu integracji odbywa się na karcie. `RecyclerView` wymaga podania adapter pochodną `RecyclerView.Adapter` dostępu do źródła danych i wypełnić każdego elementu zawartości ze źródła danych.
Ponieważ źródło danych jest specyficzny dla aplikacji, musisz zaimplementować funkcji karty rozumie, jak uzyskać dostęp do danych. Karta wyodrębnia dane ze źródła danych i ładuje go do każdego elementu w `RecyclerView` kolekcji.

Na poniższym rysunku przedstawiono sposób adapter mapowania zawartości ze źródła danych za pośrednictwem widoku posiadaczy poszczególnych widokach w ramach każdego wiersza elementu `RecyclerView`:

[ ![Diagram pokazujący karty nawiązywania połączenia ViewHolders źródła danych](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png)

Karta ładuje każdego `RecyclerView` wiersza z danymi dla elementu określonego wiersza. Dla pozycji wiersza *P*, na przykład karta lokalizuje skojarzone dane na pozycji *P* w źródle danych i kopii tych danych do wiersza elementu na pozycji *P* w `RecyclerView` kolekcji.
Na rysunku powyżej, na przykład karta używa posiadacz widoku do wyszukiwania odwołań dla `ImageView` i `TextView` na tej pozycji, więc nie wielokrotnie wywoływać `FindViewById` dla tych widoków jako użytkownik przewija kolekcji i Widoki są ponownie używane.

Podczas implementowania karty, konieczne jest przesłonięcie następujących `RecyclerView.Adapter` metod:

-   **`OnCreateViewHolder`** &ndash; Tworzy wystąpienie właściciela pliku i widoku układu elementu.

-   **`OnBindViewHolder`** &ndash; Ładuje dane w określonej pozycji do widoków, którego odwołania są przechowywane w posiadacz danego widoku.

-   **`ItemCount`** &ndash; Zwraca liczbę elementów w źródle danych.

Menedżer układu wywołuje tych metod, gdy jest pozycjonowanie elementów w obrębie `RecyclerView`. 


<a name="datachanges" />

### <a name="notifying-recyclerview-of-data-changes"></a>Powiadamianie RecyclerView zmian danych

`RecyclerView` nie jest aktualizowana automatycznie ich wyświetlania po zawartości danych jego źródle zmiany. Karta musi powiadomić `RecyclerView` podczas zmiany w zestawie danych. Zestaw danych można zmienić na wiele sposobów; na przykład można zmienić zawartość elementu lub ogólną strukturę danych mogą ulec zmianie.
`RecyclerView.Adapter` udostępnia kilka metod, które można wywołać, aby `RecyclerView` odpowiada zmiany danych w najbardziej efektywny sposób:

-  **`NotifyItemChanged`** &ndash; Sygnalizuje, że element w określonej pozycji został zmieniony.

-  **`NotifyItemRangeChanged`** &ndash; Sygnalizuje, że elementy w określonym zakresie pozycji zostały zmienione.

-  **`NotifyItemInserted`** &ndash; Sygnały nowo wstawić elementu w określonej pozycji.

-  **`NotifyItemRangeInserted`** &ndash; Sygnalizuje, że elementy w określonym zakresie pozycji nowo wstawiono.

-  **`NotifyItemRemoved`** &ndash; Sygnały usunięcie elementu w określonej pozycji.

-  **`NotifyItemRangeRemoved`** &ndash; Sygnały usunięcie elementów w określonym zakresie pozycji.

-  **`NotifyDataSetChanged`** &ndash; Sygnalizuje, że zestaw danych został zmieniony (wymusza pełnej aktualizacji).

Jeśli znasz dokładnie, jak zestaw danych został zmieniony, należy wywołać odpowiednich metod powyżej, aby odświeżyć `RecyclerView` w najbardziej efektywny sposób. Jeśli nie znasz dokładnie, jak zestaw danych został zmieniony, należy wywołać `NotifyDataSetChanged`, czyli znacznie mniej wydajna ponieważ `RecyclerView` należy odświeżyć wszystkie widoki, które są widoczne dla użytkownika. Aby uzyskać więcej informacji na temat tych metod, zobacz [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

W następnym temacie [A podstawowy przykład RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), zaimplementowano przykładową aplikację do zaprezentowania przykłady kodu rzeczywistych części i funkcji opisanych powyżej.


## <a name="related-links"></a>Linki pokrewne

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Podstawowy przykład RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [Rozszerzanie przykład RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
