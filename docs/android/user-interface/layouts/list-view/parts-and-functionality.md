---
title: "Części ListView i funkcji"
ms.topic: article
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: 4a7947c40d80c0ff8cb35dab54a11907280335d9
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="listview-parts-and-functionality"></a>Części ListView i funkcji


## <a name="overview"></a>Omówienie

A `ListView` składa się z następujących elementów:

- **Wiersze** &ndash; widoczne reprezentację danych na liście.

- **Karta** &ndash; Niewizualne klasę, która jest powiązana ze źródłem danych widoku listy.

- **Szybkie przewijanie** &ndash; uchwyt umożliwia użytkownikowi przewiń długość listy.

- **Sekcja indeksu** &ndash; elementu interfejsu użytkownika, który jest wyświetlany nad przewijania wiersze, aby wskazać, którym na liście Bieżące wiersze znajdują się.

Te zrzuty ekranu Użyj podstawowego `ListView` kontroli pokazanie sposobu renderowania szybkie przewijanie i Indeks sekcji:

[![Zrzuty ekranu aplikacji przy użyciu zwykłego starego wierszy, szybkie przewijanie i Indeks sekcji](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

Elementy wchodzące w skład `ListView` są opisane bardziej szczegółowo poniżej:


## <a name="rows"></a>Wiersze

Każdy wiersz ma własną `View`. Widok może być jedną z wbudowanych widoki zdefiniowane w `Android.Resources`, lub widok niestandardowy. Każdy wiersz może używać ten sam układ widoku lub wszystkie były różne. Istnieją przykłady w tym dokumencie korzystania z wbudowanych układów i innymi informacjami o tym, jak zdefiniować układy niestandardowe.


## <a name="adapter"></a>Adapter

`ListView` Formant wymaga `Adapter` do dostarczania sformatowany `View` dla każdego wiersza. Android ma wbudowanych kart i widoki, które mogą być używane, lub można tworzyć niestandardowe klasy.


## <a name="fast-scrolling"></a>Szybkie przewijanie

Gdy `ListView` zawiera wiele wierszy danych przewijanie fast można włączyć pomóc użytkownikowi, przejdź do dowolnej części listy. Fast przewijania "scroll bar" może być opcjonalnie włączone (i dostosowane na poziomie interfejsu API 11 lub nowszej).


## <a name="section-index"></a>Indeks sekcji

Podczas przewijania długich list, Indeks sekcji opcjonalnej udostępnia użytkownikowi opinii na jakie części listy są obecnie wyświetlane. Należy tylko na listach długie, zwykle w połączeniu z szybkie przewijanie.


## <a name="classes-overview"></a>Przegląd klas

Klasy podstawowej, używany do wyświetlania `ListViews` są wyświetlane tutaj:

[![Diagram UML pokazujący relacje między ListView i skojarzonych klas](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

Poniżej opisano przeznaczenie każdej klasy:

- **Element ListView** &ndash; elementu interfejsu użytkownika, wyświetlająca kolekcję przewijanego wierszy. Na telefonach on zazwyczaj używa się cały ekran (w takim przypadku `ListActivity` klasa może być używana) lub może być większy układu w telefonach lub urządzeń typu tablet.

- **Widok** &ndash; widoku w systemie Android, może być dowolny element interfejsu użytkownika, ale w kontekście `ListView` wymaga `View` mają być dostarczone dla każdego wiersza.

- **BaseAdapter** &ndash; klasę podstawową dla implementacji karty powiązać `ListView` ze źródłem danych.

- **ArrayAdapter** &ndash; karty wbudowane klasy, która wiąże tablicy ciągów `ListView` do wyświetlenia. Ogólnego `ArrayAdapter<T>` takie same jak w przypadku innych typów.

- **CursorAdapter** &ndash; użyj `CursorAdapter` lub `SimpleCursorAdapter` do wyświetlania danych na podstawie zapytania bazy danych SQLite.

Ten dokument zawiera proste przykłady, które używają `ArrayAdapter` oraz przykłady bardziej złożonych, które wymagają implementacji niestandardowych `BaseAdapter` lub `CursorAdapter`.

