---
title: ListView zestawu narzędzi Xamarin.Forms
description: Ten przewodnik stanowi wprowadzenie ListView Xamarin.Forms, która może służyć do prezentowania danych na listach pięknych, interaktywnych.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: dc039a7a984fae9bd856a9e7147ad899f86f0592
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "35245003"
---
# <a name="xamarinforms-listview"></a>ListView zestawu narzędzi Xamarin.Forms

ListView jest prezentowanie listy danych, szczególnie długie list, które wymagają przewijania widoku. W przewodniku opisano, jak używać ListView:

1. **[Źródła danych](data-and-databinding.md)**  &ndash; wypełnić ListView przy użyciu danych z lub bez powiązania danych.
2. **[Wygląd komórki](customizing-cell-appearance.md)**  &ndash; Dostosowywanie wyglądu komórek wbudowanych lub utworzyć własne niestandardowe komórki.
3. **[Wygląd listy](customizing-list-appearance.md)**  &ndash; dostosować wygląd ListView. Ustaw nagłówki i stopki, Włącz grupy i zmienić wysokość wierszy.
4. **[Interakcyjność](interactivity.md)**  &ndash; obsługi podsłuchu i opcji, wdrożenia ściągnięcia do odświeżania i dodać kontekstowego akcji.
5. **[Wydajność](performance.md)**  &ndash; uniknąć problemów z wydajnością.

## <a name="use-cases"></a>Przypadki użycia
Upewnij się, że ListView jest formantem odpowiednie do potrzeb. ListView może służyć w każdej sytuacji, w którym są wyświetlane przewijaną listę danych. ListViews obsługuje kontekstu akcji i powiązanie danych.

Nie należy mylić ListView przy użyciu [TableView](~/xamarin-forms/user-interface/tableview.md). Formant TableView jest lepszym rozwiązaniem, zawsze wtedy, gdy masz inne niż powiązane z listy opcji lub danych. Na przykład aplikacji ustawienia systemu iOS, która ma przede wszystkim wstępnie zdefiniowany zestaw opcji, lepiej jest nadaje się do korzystania z TableView niż ListView.

Również Zwróć uwagę, że ListView najlepiej nadaje się do homogenicznych danych &ndash; oznacza to, że wszystkie dane powinny być tego samego typu. Jest to spowodowane tylko jeden typ komórki może służyć do każdy wiersz na liście. TableViews może obsługiwać wiele typów komórki, dzięki czemu są one lepszym rozwiązaniem, jeśli musisz używać różnych miar widoków.


## <a name="components"></a>Składniki
ListView ma liczby składników dostępnych do wykonywania macierzystą funkcjonalność każdej z platform. Poniżej opisano każdy z tych składników:

- **[Nagłówki i stopki](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; tekstu lub widok, aby wyświetlić na początku i na końcu listy, należy oddzielić od danych listy. Nagłówki i stopki może być powiązana ze źródłem danych niezależnie od źródła danych ListView.
- **[Grupy](customizing-list-appearance.md#Grouping)**  &ndash; dane w ListView mogą być grupowane w celu zapewnienia łatwiejszej nawigacji. Grupy są zazwyczaj powiązane z danymi:

![](images/grouping-depth.png "ListView przy użyciu zgrupowanych danych")

- **[Komórki](customizing-cell-appearance.md)**  &ndash; dane w ListView są prezentowane w komórkach. Każda komórka odnosi się do wiersza danych. Istnieją wbudowane komórki do wyboru, lub można zdefiniować własne niestandardowe komórki. Niestandardowe i wbudowane komórek mogą być używane/zdefiniowany w XAML lub kodu.
  - **[Wbudowane](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; wbudowane komórek, szczególnie TextCell i ImageCell, może być doskonałą wydajność, ponieważ odnoszą się do natywnych kontrolek na każdej platformie.
       - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; wyświetla ciąg tekstowy, opcjonalnie z tekstem szczegółów. Szczegóły tekstu jest renderowane jako drugi wiersz w mniejszej czcionki przy użyciu koloru akcentu.
       - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; Wyświetla obraz z tekstem. Pojawia się jako TextCell za pomocą obrazu po lewej stronie.
  - **[Niestandardowe komórek](customizing-cell-appearance.md#customcells)**  &ndash; komórek niestandardowe sprawdzają się kiedy trzeba przedstawienie złożonych danych. Na przykład widok niestandardowy może służyć do przedstawienia listy utwory muzyczne, wykonawcy i albumów w tym:

![](images/image-cell-default.png "ListView przy użyciu ImageCells")

Aby dowiedzieć się więcej o dostosowywaniu komórek w ListView, zobacz [Dostosowywanie wygląd komórki ListView](customizing-cell-appearance.md).

## <a name="functionality"></a>Funkcja
ListView obsługuje szereg style interakcji, w tym:

- **[Ściągnięcia do odświeżania](interactivity.md#Pull_to_Refresh)**  &ndash; ListView obsługuje ściągania do odświeżania na każdej platformie.
- **[Kontekst akcji](interactivity.md#Context_Actions)**  &ndash; ListView obsługuje wykonywanie działań na poszczególne elementy na liście. Na przykład możesz można zaimplementować przesunięcia do działania w systemie iOS, lub naciśnij długo akcji w systemie Android.
- **[Wybór](interactivity.md#selectiontaps)**  &ndash; nasłuchiwanie wybrane opcje i usuwanie podjęcie działań jest wybrany wiersz.

![](images/context-default.png "ListView przy użyciu kontekstu akcji")

Aby dowiedzieć się więcej o funkcjach interakcyjność w ListView, zobacz [akcje i interakcję z ListView](interactivity.md).


## <a name="related-links"></a>Linki pokrewne

- [Praca z ListView (przykład)](https://developer.xamarin.com/samples/WorkingWithListview)
- [Dwa wiążących (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [Wbudowane w komórkach (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Niestandardowe komórek (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Grupowanie (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Widok niestandardowy moduł renderowania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [Interakcyjność ListView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
