---
title: "Części tabeli i funkcji"
ms.topic: article
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: fece27db2945ee7142296ec0872f395c127e318e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="table-parts-and-functionality"></a>Części tabeli i funkcji

UITableView może mieć stylu "grupowanych" lub "zwykły" i składa się z następujących elementów:

-  [Nagłówek sekcji](#Section_Header)
-  [Komórki](#Cells) (lub wierszy, jeśli wolisz)
-  [Sekcja stopki](#Section_Footer)
-  [Index](#Index)
-  [Tryb edycji](#Edit_Features) (obejmuje "szybko Przesuń do usunięcia" i przeciągnij uchwyt, aby zmienić kolejność wierszy) 

Te zrzuty ekranu przedstawiają sposób wyświetlania wierszy sekcji, nagłówków, stopek, formantów edycji i indeksu.

 [![](table-parts-and-functionality-images/image1a.png "Te zrzuty ekranu Pokaż sposób wyświetlania wierszy sekcji, nagłówków, stopek, formantów edycji i indeks")](table-parts-and-functionality-images/image1a.png#lightbox)

Części te są opisane bardziej szczegółowo poniżej:

<a name="Section_Header" />

## <a name="section-header"></a>Nagłówek sekcji

Komórki można opcjonalnie można podzielone na sekcje, etykietą niestandardowy nagłówek i/lub etykietą stopki. Nagłówek może być ustawiona z wartością ciągu lub widok niestandardowy może dostarczyć umożliwiające stylu lub inny układ.

<a name="Cells" />

## <a name="cells"></a>Komórki

Komórki są elementu interfejsu użytkownika głównego dla tabeli. Po zaimplementowaniu poprawnie, komórki są ponownie używane w celu zwiększenia wydajności pamięci. Istnieją cztery wbudowanych stylów komórki, a przy użyciu Scenorys można utworzyć własne niestandardowe komórek — w kodzie, lub w projektancie.

<a name="Section_Footer"/>

## <a name="section-footer"></a>Sekcja stopki

Stopki sekcji opcjonalnej można skonfigurować z wartością ciągu lub widok niestandardowy może dostarczyć umożliwiające stylu lub inny układ. Sekcja nagłówków i stopek można skonfigurować niezależnie.

<a name="Index" />

## <a name="index"></a>Indeks

Indeks jest wyświetlana jako pasek znaków w dół do prawej krawędzi tabeli.
Dotknięcie lub przeciąganie w indeksie przyspiesza przewijanie do odpowiedniej części tabeli. Indeks jest opcjonalne, ale zaleca się, aby ułatwić przechodzenie długich list. Indeks nie jest zwykle używany przy użyciu stylu grupowanego.

<a name="Edit_Features" />

## <a name="editing-mode"></a>Tryb edycji

Dostępne są dwa różne funkcje edycji:

- Przejdź do usunięcia pojedynczych komórek.
- Aby wyświetlić usuń przyciski w każdym wierszu trybu edycji 
- Przechodzenie do trybu edycji można ujawnić uchwytów ponownie porządkowania. 
- Wstawianie nowych komórek (z animacji).

W pozostałej części tego dokumentu pokazano, jak wdrożyć te funkcje UITableView z platformy Xamarin.iOS.


## <a name="classes-overview"></a>Przegląd klas

Klasy podstawowej, używany do wyświetlania widoku tabeli przedstawiono poniżej:

[![](table-parts-and-functionality-images/classdiagram.png "Klasy podstawowej, używany do wyświetlania widoków tabel są wyświetlane tutaj")](table-parts-and-functionality-images/classdiagram.png#lightbox)

Poniżej opisano przeznaczenie każdej klasy:

- **UITableView** — widok zawierający Kolekcja komórek wewnątrz kontenera przewijania. Widok tabeli zwykle wykorzystuje cały ekran aplikację telefonów iPhone, ale może istnieć jako część większego widoku tabletów iPad (lub pojawia się popover). 
- **UITableViewCell** — widoku, który reprezentuje komórkę pojedynczego (lub wiersza) w widoku tabeli. Istnieją cztery typy wbudowanego i umożliwia tworzenie niestandardowych komórek, zarówno w języku C# lub z systemem iOS projektanta. 
- **UITableViewSource** — Xamarin.iOS na wyłączność abstrakcyjna klasa, która zawiera wszystkie metody, które są wymagane, aby wyświetlić tabelę, w tym liczba wierszy, zwracanie widoku komórki dla każdego wiersza, obsługa zaznaczenie wiersza i wiele innych funkcji opcjonalnych. Możesz *musi* podklasy tutaj, aby rozpocząć pracę UITableView. 
- **NSIndexPath** — sekcji i wiersz zawiera właściwości, które identyfikują położenia komórki w tabeli. 
- **UITableViewController** — UIViewController gotowe do użycia, który ma ustalony UITableView jako jego widoku i jest dostępny za pośrednictwem właściwości TableView. 
- **UIViewController** — Jeśli tabela nie zajmują cały ekran, możesz dodać odpowiednio ustawione UITableView do UIViewController dowolnego z jego ramki. 

UITableViewSource zastępuje następujące dwie klasy, które są nadal dostępne w Xamarin.iOS, ale nie są zwykle wymagane:

- **UITableViewDataSource** — protokół Objective-C, który jest uformowana w Xamarin.iOS jako klasy abstrakcyjnej. Musi być podklasą klasy zapewnienie tabeli z widoku dla każdej komórki, a także informacje dotyczące nagłówków, stopek i liczby wierszy i sekcje w tabeli. 
- **UITableViewDelegate** — protokół Objective-C, który jest uformowana w Xamarin.iOS jako klasa. Obsługuje wybieranie, edytowanie i inne funkcje opcjonalne tabeli. 

W tym dokumencie przykłady używają UITableViewSource i ignorować te dwie klasy. Są wymienione w tym miejscu ponieważ przykładami Objective-C znaleźć w dokumentacji firmy Apple będzie odwoływać się do ich, więc warto poznać co zrobić (i czy można używać w UITableViewSource Xamarin.iOS w zamian).

## <a name="related-links"></a>Linki pokrewne

- [WorkingWithTables (przykład)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
