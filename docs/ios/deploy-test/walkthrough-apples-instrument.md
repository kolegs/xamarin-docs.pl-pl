---
title: Przewodnik — używanie narzędzia Instruments firmy Apple
description: W tym artykule opisano sposób użycia narzędzia Instruments firmy Apple do diagnozowania problemów z pamięcią w aplikacji systemu iOS utworzone za pomocą platformy Xamarin. Pokazuje jak Uruchom instrumenty, wykonaj migawki sterty, analizowanie wzrostu użycia pamięci i więcej.
ms.prod: xamarin
ms.assetid: 8f21db1d-7107-4158-8058-d47e417689a0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a751488b4063a594904393faa605d36c3414d2ec
ms.sourcegitcommit: 021027b78cb2f8061b03a7c6ae59367ded32d587
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39182185"
---
# <a name="walkthrough---using-apples-instruments-tool"></a>Przewodnik — używanie narzędzia Instruments firmy Apple

_W tym artykule przedstawiono sposób użycia narzędzia Instruments firmy Apple do diagnozowania problemów z pamięcią w aplikacji systemu iOS utworzone za pomocą platformy Xamarin. Pokazuje sposób Uruchom instrumenty, wykonaj migawki sterty i analizowania wzrostu użycia pamięci. Pokazano również, jak używać instrumenty do wyświetlania i wskazanie dokładne wiersze kodu, które powodują problemu pamięci._

Ta strona pokazuje sposób użycia **narzędzia Instruments firmy Xcode** zdiagnozować problem pamięci w aplikacji systemu iOS.
Najpierw pobierz [przykładowe MemoryDemo](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) , a następnie otwórz **przed** rozwiązania w programie Visual Studio dla komputerów Mac.

## <a name="diagnosing-the-memory-issues"></a>Diagnozowanie problemów z pamięcią

1. W programie Visual Studio dla komputerów Mac, należy uruchomić **instrumentów** z **Narzędzia > Uruchom instrumenty** elementu menu.
2. Przekaż aplikację na urządzeniu, wybierając **Uruchom > Przekaż do urządzenia** elementu menu.
3. Wybierz **alokacje** szablonu (pomarańczowa ikona z polem biały)

    ![](walkthrough-apples-instrument-images/00-allocations-tempate.png "Wybierz szablon alokacji")

4. Wybierz **pokaz pamięci** w **wybierz szablon profilowania:** lista w górnej części okna. Kliknij przycisk na urządzeniu z systemem iOS, najpierw po to, aby rozwinąć menu, który pokazuje zainstalowanych aplikacji.

    ![](walkthrough-apples-instrument-images/01-mem-demo.png "Wybieranie aplikacji pokazowej pamięci")

5. Naciśnij klawisz **wybierz** przycisk (po prawej dolnej części okna), aby uruchomić **instrumentów**. Szablon ThiJs wyświetli dwa elementy w górnym okienku: alokacji i śledzenie maszyn wirtualnych.

6. Naciśnij klawisz **rekordu** przycisku (czerwone kółko w lewym górnym rogu) w dokumentach, które uruchomi aplikację.

7. Wybierz **maszyny Wirtualnej śledzenie** wiersza w górnym okienku (teraz, gdy aplikacja jest uruchomiona, będzie zawierać dwie sekcje: rozmiar zmieniony i rezydentnych widoczne). W **Inspektor** okienku wybierz **Pokaż ustawienia wyświetlania** opcji (ikonę koła zębatego), a następnie znaczników **automatycznego tworzenia migawek** pola wyboru, które są wyświetlane w prawej dolnej to Zrzut ekranu:

    ![](walkthrough-apples-instrument-images/02-auto-snapshot.png "Wybierz opcję Pokaż ustawienia wyświetlania ikonę koła zębatego, a następnie zaznacz pole wyboru automatycznego tworzenia migawek")

8. Wybierz **alokacje** wiersza w górnym okienku (teraz, gdy aplikacja jest uruchomiona, będzie wyświetlany tekst *anonimowe maszyn wirtualnych i wszystkich sterty*)
9. W **Inspektor** okienku wybierz **Pokaż ustawienia wyświetlania** opcji (ikonę koła zębatego), a następnie kliknij naciśnięciem **generowania znacznik** przycisk, aby ustalić punkt odniesienia. Małe czerwona flaga będą wyświetlane na osi czasu w górnej części okna
10. Przewiń w aplikacji, a następnie wybierz **generowania znacznik** ponownie (powtarzać kilka razy)
11. Kliknij przycisk **zatrzymać** przycisku.
12. Rozwiń **generowania** węzła o największej **wzrostu** i Sortuj według **wzrostu** (malejąco).
13. Zmiana **Inspektor** okienko, aby **Pokaż szczegóły rozszerzone** ("E"), który wskazuje **ślad stosu**.

14. Zwróć uwagę  **&lt;-object >** węzeł zawiera wzrostu zbyt dużej ilości pamięci. Kliknij strzałkę obok ten węzeł, aby zobaczyć więcej szczegółów — kliknij prawym przyciskiem myszy w ślad stosu, aby dodać **lokalizacja źródłowa** do okienka:

    ![](walkthrough-apples-instrument-images/03-mem-growth.png "Dodaj lokalizację źródłową do okienka")

15. Sortuj według **rozmiar** i wyświetlić **rozszerzone szczegóły** widoku:

    ![](walkthrough-apples-instrument-images/04-extended-detail.png "Sortuj według rozmiaru i wyświetlania rozszerzonych szczegółowy widok")

16. Kliknij żądnego wpisu w stosie wywołań, aby sprawdzić kod pokrewny:

    ![](walkthrough-apples-instrument-images/05-related-code.png "Wyświetlanie kodu powiązanego")

W takim przypadku nowy obraz jest tworzone i przechowywane w kolekcji dla każdej komórki ani czy istniejący widok kolekcji komórek używane ponownie.

## <a name="resolving-the-memory-issues"></a>Rozwiązywanie problemów z pamięcią

Istnieje możliwość rozwiązać te problemy i ponownie uruchom aplikację za pomocą dokumentów.

DEKLARUJĄC pojedynczego wystąpienia na poziomie klasy, obraz, który mogą być używane ponownie i obiekt komórki mogą być ponownie używane z istniejącej puli zamiast po utworzeniu, jak pokazano poniżej:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Dequeue a cell from the reuse pool
    var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);

    // Reuse the image declared at the class level
    imageCell.ImageView.Image = image;

    return imageCell;
}
```

Teraz, gdy aplikacja jest uruchomiona, jest znacznie mniejsze zużycie pamięci — **wzrostu** między generacji jest teraz mierzony w Kib (w kilobajtach) zamiast MiB (w megabajtach) sprzed naprawia kod:

![](walkthrough-apples-instrument-images/06-reduced-memory.png "Wyświetlanie użycia pamięci aplikacji")

Ulepszone kod jest dostępny w [przykładowe MemoryDemo](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) w **po** rozwiązania w programie Visual Studio dla komputerów Mac.

Ten blog społeczności o [Xamarin.iOS wyrzucania elementów bezużytecznych](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/) jest odwołaniem przydatne radzenia sobie z problemami pamięci z rozszerzeniem Xamarin.iOS.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia instrumenty do diagnozowania problemów z pamięcią.
On opisany sposób uruchamiania związanej z poziomu programu Visual Studio dla komputerów Mac, załadować szablon alokacji pamięci i krok migawki użycia w celu określenia problemów z pamięcią.
Na koniec aplikacja została ponownie zbadane, aby sprawdzić, czy problem został rozwiązany.

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe MemoryDemo](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)
- [Xamarin.iOS wyrzucania elementów bezużytecznych (wpis na blogu)](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/)
