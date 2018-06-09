---
title: Element ListView platformy Xamarin.Forms
description: W tym przewodniku przedstawiono ListView platformy Xamarin.Forms, która może służyć do prezentowania danych na listach doskonałych, interakcyjne.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: f73e7b70749a7a6913856d8c753db63a6d0a2bbe
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245003"
---
# <a name="xamarinforms-listview"></a>Element ListView platformy Xamarin.Forms

Element ListView jest w widoku listy danych, szczególnie długie list, które wymagają przewijanie prezentacji. W tym przewodniku opisano, jak używać ListView:

1. **[Źródła danych](data-and-databinding.md)**  &ndash; wypełnienia ListView z danymi, z lub bez powiązania danych.
2. **[Wygląd komórek](customizing-cell-appearance.md)**  &ndash; dostosować wygląd komórek wbudowanych lub utworzyć własne niestandardowe komórki.
3. **[Lista wygląd](customizing-list-appearance.md)**  &ndash; Dostosowywanie wyglądu elementu ListView. Ustaw nagłówki i stopki, Włącz grup i zmienić wysokość wierszy.
4. **[Interakcyjności](interactivity.md)**  &ndash; podsłuchu i opcji, wdrożenia pociągnij, aby odświeżyć i Dodaj akcje kontekstowe.
5. **[Wydajność](performance.md)**  &ndash; uniknąć problemów z wydajnością.

## <a name="use-cases"></a>Przypadki użycia
Upewnij się, że element ListView jest urządzenie odpowiednie do potrzeb. Element ListView można w każdej sytuacji, w którym są wyświetlane przewijanej listy danych. Widokach listy obsługuje kontekstu akcji i powiązania danych.

ListView — nie należy mylić z [TableView](~/xamarin-forms/user-interface/tableview.md). Formant TableView jest lepszym rozwiązaniem, zawsze, gdy masz listę z systemem innym niż granica opcje lub danych. Na przykład ustawienia aplikacji dla systemu iOS, mającą przeważnie wstępnie zdefiniowane opcje lepiej jest odpowiednie do użycia TableView niż ListView.

Również Pamiętaj, że element ListView jest najlepiej nadaje się do danych jednorodnych &ndash; oznacza to, że wszystkie dane powinny być tego samego typu. Jest to spowodowane użyciem tylko jeden typ komórki dla każdego wiersza w liście. TableViews może obsługiwać wiele typów komórki, dzięki czemu są one lepszym rozwiązaniem, gdy trzeba mieszać widoków.


## <a name="components"></a>Składniki
Element ListView ma liczbę składników dostępnych do wykonywania funkcji macierzystego dla każdej z platform. Poniżej opisano każdy z tych składników:

- **[Nagłówki i stopki](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; tekstu lub widok, aby wyświetlić na początku i na końcu listy oddzielić od danych listy. Nagłówki i stopki może być powiązana ze źródłem danych niezależnie od źródła danych elementu ListView.
- **[Grupy](customizing-list-appearance.md#Grouping)**  &ndash; ułatwiające nawigację można grupować dane w elemencie ListView. Grupy są zwykle powiązane z danymi:

![](images/grouping-depth.png "Element ListView z zgrupowanych danych")

- **[Komórki](customizing-cell-appearance.md)**  &ndash; zobaczy w komórkach danych w elemencie ListView. Każda komórka odpowiada wiersz danych. Dostępne są wbudowane komórek do wyboru, lub można definiować własne niestandardowe komórki. Komórek zarówno wbudowanych i niestandardowych można używać/zdefiniowany w języku XAML lub kodu.
  - **[Wbudowane](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; wbudowane w komórkach, szczególnie TextCell i ImageCell, może być doskonałą wydajność, ponieważ odpowiada kontrolki natywne na każdej z platform.
       - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; wyświetla ciąg tekstu, opcjonalnie z tekstem szczegółów. Tekst szczegółów jest renderowane jako drugi wiersz w mniejszej czcionki z kolor akcentu.
       - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; Wyświetla obraz z tekstem. Pojawia się jako TextCell z obrazem po lewej stronie.
  - **[Niestandardowe komórek](customizing-cell-appearance.md#customcells)**  &ndash; niestandardowe komórki są doskonałe, gdy użytkownik musi przedstawiać dane złożone. Na przykład widok niestandardowy może posłużyć do przedstawienia listy utworów muzycznych, w tym wykonawcy i albumu:

![](images/image-cell-default.png "Element ListView z ImageCells")

Aby dowiedzieć się więcej na temat dostosowywania komórek w elemencie ListView, zobacz [Dostosowywanie wyglądu komórek ListView](customizing-cell-appearance.md).

## <a name="functionality"></a>Funkcja
Element ListView jest obsługiwanych kilka stylów interakcji, w tym:

- **[Pociągnij, aby odświeżyć](interactivity.md#Pull_to_Refresh)**  &ndash; ListView obsługuje pociągnij, aby odświeżyć na każdej z platform.
- **[Kontekst akcji](interactivity.md#Context_Actions)**  &ndash; ListView obsługuje podjęcie działań na poszczególne elementy na liście. Na przykład możesz można zaimplementować przejdź do działania w systemie iOS, lub wybierz long akcji w systemie Android.
- **[Wybór](interactivity.md#selectiontaps)**  &ndash; nasłuchiwania wybrane opcje i usuwanie podjęcie działań jest wybrany wiersz.

![](images/context-default.png "Element ListView z kontekstu akcji")

Aby dowiedzieć się więcej o funkcjach interakcyjności ListView, zobacz [akcje & interakcję z ListView](interactivity.md).


## <a name="related-links"></a>Linki pokrewne

- [Praca z ListView (przykład)](https://developer.xamarin.com/samples/WorkingWithListview)
- [Dwa wiążących (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [Wbudowane w komórkach (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Niestandardowe komórek (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Grupowanie (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Widok niestandardowy moduł renderowania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [Element ListView interakcyjności (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [iOS skoroszytu](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-ios.workbook)
- [Android skoroszytu](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-android.workbook)
