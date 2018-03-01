---
title: "Wskazówki — za pomocą narzędzia dokumentów firmy Apple"
description: "W tym artykule przedstawiono sposób użycia narzędzia dokumentów firmy Apple do diagnozowania problemów pamięci w aplikacji systemu iOS utworzonej za pomocą platformy Xamarin. Go pokazano, jak uruchomić dokumentów, wykonać migawki sterty i analizować wzrostu pamięci. Widoczny jest również sposób używać dokumentów do wyświetlania i wskazanie dokładne wierszy kodu, które przyczyną problemu pamięci."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8f21db1d-7107-4158-8058-d47e417689a0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 949a2ea4d5838b664e19251e69a32efccc98d496
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-apples-instruments-tool"></a>Wskazówki — za pomocą narzędzia dokumentów firmy Apple

_W tym artykule przedstawiono sposób użycia narzędzia dokumentów firmy Apple do diagnozowania problemów pamięci w aplikacji systemu iOS utworzonej za pomocą platformy Xamarin. Go pokazano, jak uruchomić dokumentów, wykonać migawki sterty i analizować wzrostu pamięci. Widoczny jest również sposób używać dokumentów do wyświetlania i wskazanie dokładne wierszy kodu, które przyczyną problemu pamięci._

Ta strona przedstawiono sposób użycia **narzędzie dokumentów w środowisku Xcode** zdiagnozować problem pamięci w aplikacji systemu iOS.
Najpierw pobierz [próbki MemoryDemo](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) , a następnie otwórz **przed** rozwiązania w programie Visual Studio dla komputerów Mac.

## <a name="diagnosing-the-memory-issues"></a>Diagnozowanie problemów z pamięcią

1.  W programie Visual Studio dla komputerów Mac, należy uruchomić **instrumentów** z **Narzędzia > Uruchom instrumentów** elementu menu.
2.  Przekaż aplikację na urządzeniu, wybierając **Uruchom > Przekaż do urządzenia** elementu menu.
3.  Wybierz **alokacji** szablonu (pomarańczową ikoną z polem biały)

    ![](walkthrough-apples-instrument-images/00-allocations-tempate.png "Wybierz szablon alokacji")

4.  Wybierz **pokaz pamięci** aplikacji w **wybierz szablon profilowania:** listy w górnej części okna. Kliknij na urządzeniu z systemem iOS najpierw, aby rozwinąć menu, która zawiera zainstalowane aplikacje.

    ![](walkthrough-apples-instrument-images/01-mem-demo.png "Wybierz aplikację pokaz pamięci")

5.  Naciśnij klawisz **wybierz** przycisk (dolnym rogu okna), aby uruchomić **instrumentów**. Szablon ThiJs przedstawia dwa elementy w górnym okienku: alokacji i śledzenie maszyny Wirtualnej.

6.  Naciśnij klawisz **rekordu** przycisk (czerwone kółko w lewym górnym) w dokumentach, co spowoduje uruchomienie aplikacji.

7.  Wybierz **Tracker wirtualna** wiersza w górnym okienku (teraz, gdy aplikacja jest uruchomiona, będzie zawierać dwie sekcje: rozmiar zanieczyszczone i rezydentnych widoczne). W **inspektora** okienku wybierz **Pokaż ustawienia wyświetlania** opcji (koło zębate ikonę), a następnie znaczników **automatycznego tworzenia migawek** wyboru wyświetlany w prawym dolnym rogu tego Zrzut ekranu:

    ![](walkthrough-apples-instrument-images/02-auto-snapshot.png "Wybierz opcję Pokaż ustawienia wyświetlania koło zębate ikonę, a następnie zaznacz pole wyboru automatycznego tworzenia migawek")

8.  Wybierz **alokacji** wiersza w górnym okienku (teraz, gdy aplikacja jest uruchomiona, jest wyświetlany tekst *wszystkie sterty i anonimowe maszyny Wirtualnej*)
9.  W **inspektora** okienku wybierz **Pokaż ustawienia wyświetlania** opcji (koło zębate ikonę), a następnie kliknij następnie naciśnij klawisz **generowania znacznik** przycisk, aby ustalić linię bazową. Mała czerwona flaga będą wyświetlane na osi czasu, w górnej części okna
10.  Przewiń w aplikacji, a następnie wybierz **generowania znacznik** ponownie (Powtórz kilka razy)
11.  Kliknij przycisk **zatrzymać** przycisku.
12.  Rozwiń węzeł **generowania** węzła o największej **wzrostu** i sortowanie według **wzrostu** (malejąco).
13.  Zmień **inspektora** okienko, aby **Pokaż szczegóły rozszerzony** ("E"), który wskazuje **ślad stosu**.

14.  Powiadomienie **< niebędących obiektami >** węzeł Wyświetla wzrostu nadmiernego pamięci. Kliknij strzałkę obok ten węzeł, aby zobaczyć więcej szczegółów — kliknij prawym przyciskiem myszy w ślad stosu, aby dodać **lokalizacji źródłowej** do okienka:

    ![](walkthrough-apples-instrument-images/03-mem-growth.png "W okienku Dodaj lokalizacji źródłowej")

15.  Sortuj według **rozmiar** i wyświetlić **rozszerzony szczegółów** widoku:

    ![](walkthrough-apples-instrument-images/04-extended-detail.png "Sortuj według rozmiaru i Wyświetl widok rozszerzony szczegółów")

16.  Kliknij żądaną pozycję w stosie wywołań, aby zobaczyć powiązane kodu:

    ![](walkthrough-apples-instrument-images/05-related-code.png "Wyświetlanie kodu powiązanego")

W takim przypadku nowy obraz jest tworzone i przechowywane w kolekcji dla każdej komórki ani czy istniejący widok kolekcji komórek używane ponownie.

## <a name="resolving-the-memory-issues"></a>Rozwiązywanie problemów z pamięci

Istnieje możliwość rozwiązać te problemy i ponownie uruchom aplikację za pomocą dokumentów.

Deklarowanie pojedynczego wystąpienia na poziomie klasy, można ponownie użyć obrazu i obiektu komórki mogą być ponownie używane z istniejącej puli zamiast utworzone zawsze, jak pokazano poniżej:

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

Teraz, gdy aplikacja jest uruchamiana, użycie pamięci jest znacznie ograniczone — **wzrostu** między generacje jest teraz mierzony w Kib (w kilobajtach) zamiast MiB (MB) sprzed ustalania kodu:

![](walkthrough-apples-instrument-images/06-reduced-memory.png "Wyświetlanie użycia pamięci aplikacji")

Ulepszone kod jest dostępny w [próbki MemoryDemo](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) w **po** rozwiązania w programie Visual Studio dla komputerów Mac.

Ten blog społeczności o [Xamarin.iOS wyrzucanie elementów bezużytecznych](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/) jest przydatne odwołanie zajmujących się problemy z pamięcią z platformy Xamarin.iOS.


## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia instrumentów zdiagnozować problemy z pamięcią.
On opisany sposobu uruchamiania dokumentów z poziomu programu Visual Studio dla komputerów Mac, załadowania szablonu w alokacji pamięci i kroku migawki używany do punktu przyczepienia problemy z pamięcią.
Ponadto aplikacja została ponownie zbadane, aby sprawdzić, czy problem został rozwiązany.


## <a name="related-links"></a>Linki pokrewne

- [Przykładowe MemoryDemo](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)
- [Xamarin.iOS wyrzucanie elementów bezużytecznych](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)
