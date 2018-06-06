---
title: System macOS interfejsów API dla deweloperów Xamarin.Mac
description: W tym dokumencie opisano sposób odczytać selektorów Objective-C i Znajdź w aplikacji Xamarin.Mac ich odpowiednich metod C#.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: cceaa2f6e89b712be5929f7e978663d8c47f18c5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791553"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>System macOS interfejsów API dla deweloperów Xamarin.Mac

## <a name="overview"></a>Omówienie

Dla wielu programowania z użyciem Xamarin.Mac czasu można wziąć pod uwagę, odczytać i zapisać w języku C# bez dużo problem z podstawowych interfejsów API języka Objective-C. Jednak czasami będzie można odczytać w dokumentacji interfejsu API firmy Apple, tłumaczenia odpowiedź od przepełnienie stosu do rozwiązania problemu, lub porównania istniejący przykład.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Za mało Objective-C, aby być niebezpieczne odczytu

Czasami konieczne będzie można odczytać definicji Objective-C lub metoda wywołania i który przełożyć na równoważne metody C#. Teraz Przyjrzyjmy się definicji funkcji języka Objective C i rozbić elementy. Ta metoda ( *selektora* w języku Objective-C) można znaleźć w `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Deklaracja mogą być odczytywane w lewej do prawej:

- `-` Prefiks oznacza jest metodą wystąpienia (niestatyczna). + oznacza, że jest to metoda klasy (statyczny)
- `(BOOL)` jest zwracany typ (bool w języku C#)
- `canDragRowsWithIndexes` to pierwsza część nazwy.
- `(NSIndexSet *)rowIndexes` jest pierwszy param i ma typ z nim. Pierwszym parametrem jest w formacie: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` jest drugi param i jej typie. Każdy parametr po pierwszym format to: `selectorPart:(Type) pararmName`
- Pełna nazwa selektor ten komunikat jest: `canDragRowsWithIndexes:atPoint:`. Uwaga `:` na końcu — jest ważna.
- Jest rzeczywistego powiązania Xamarin.Mac C#: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

To wywołanie selektora mogą być odczytywane w taki sam sposób:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- Wystąpienie `v` występują jego `canDragRowsWithIndexes:atPoint` selektora wywoływana z dwoma parametrami `set` i `point`, przekazana.
- W języku C# wywołanie metody wygląda następująco: `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>Wyszukiwanie elementu członkowskiego C# dla danego selektora

Teraz, gdy uda Ci się znaleźć selektor języka Objective-C, potrzebne do wywołania, następnym krokiem jest mapowanie na równoważny element członkowski C#. Istnieją cztery metody, można spróbować (kontynuowaniem `NSTableView CanDragRows` przykład):

1. Lista uzupełniania automatycznie umożliwia szybkie skanowanie w poszukiwaniu coś o takiej samej nazwie. Ponieważ było wiadomo, jest wystąpieniem `NSTableView` możesz wpisać:

    - `NSTableView x;`
    - `x.` [ctrl + miejsce, jeśli nie ma listy).
    - `CanDrag` [Wprowadź]
    - Kliknij prawym przyciskiem myszy metodę, przejdź do deklaracji, otwórz przeglądarkę zestawu, w którym można porównać `Export` w selektorze atrybutu

2. Wyszukaj powiązanie całej klasy. Ponieważ było wiadomo, jest wystąpieniem `NSTableView` możesz wpisać:

    - `NSTableView x;`
    - Kliknij prawym przyciskiem myszy `NSTableView`, przejdź do deklaracji przeglądarkę zestawu
    - Wyszukaj w selektorze

3. Można użyć [dokumentacji interfejsu API Xamarin.Mac](https://developer.xamarin.com/api/root/monomac-lib/) .

4. Miguel zawiera widok "Kamienie Rosetta" interfejsów API Xamarin.Mac [tutaj](http://tirania.org/tmp/rosetta.html) , które można przeszukiwać dla danego interfejsu API. Jeśli Twój interfejs API nie jest AppKit lub macOS określonych, może być brak.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
