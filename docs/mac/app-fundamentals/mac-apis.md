---
title: System macOS interfejsów API dla deweloperów platformy Xamarin.Mac
description: W tym dokumencie opisano, jak odczytać selektory języka Objective-C i sposobach znajdowania ich odpowiednich metod języka C# w aplikacji platformy Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: 6dfaa3c7bf988228bfbacefe7c8e7268edc8117a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994315"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>System macOS interfejsów API dla deweloperów platformy Xamarin.Mac

## <a name="overview"></a>Omówienie

Przez większość czasu opracowywanie zawartości przy użyciu platformy Xamarin.Mac można traktować, odczytu i zapisu w języku C# w zapewnieniu wiele z podstawowych interfejsów API języka Objective-C. Jednak czasami będziesz potrzebujesz w dokumentacji interfejsu API firmy Apple, tłumaczenia odpowiedź na podstawie Stack Overflow do rozwiązania problemu lub porównania istniejący przykład.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Za mało języka Objective-C niebezpieczne do czytania

Czasami konieczne będzie można odczytać definicji języka Objective-C lub metodę wywołania i Przetłumacz, na równoważne metodę języka C#. Teraz zapoznaj się z definicji funkcji w języku Objective-C demonstrować i analizować elementy. Ta metoda ( *selektor* w języku Objective-C) można znaleźć na `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Deklaracja może odczytywane od lewej do prawej:

- `-` Prefiks oznacza, że jest metodą wystąpienia (niestatycznych). + oznacza, że jest to metoda klasy (statyczny)
- `(BOOL)` jest zwracany typ (wartość logiczna, w języku C#)
- `canDragRowsWithIndexes` to pierwsza część nazwy.
- `(NSIndexSet *)rowIndexes` jest pierwszy parametr i typ ma z nią. Pierwszy parametr jest w formacie: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` jest drugi parametr i jego typu. Każdy parametr po pierwszym jest format: `selectorPart:(Type) pararmName`
- Pełna nazwa tego selektora komunikatu jest: `canDragRowsWithIndexes:atPoint:`. Uwaga `:` na końcu — jest ważne.
- To faktycznego wiązania Xamarin.Mac C#: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

To wywołanie selektor mogą być odczytywane w taki sam sposób:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- Wystąpienie `v` występują w jego `canDragRowsWithIndexes:atPoint` selektor wywoływana z dwoma parametrami `set` i `point`, przekazana.
- W języku C# wywołanie metody wygląda następująco: `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>Wyszukiwanie elementu członkowskiego języka C# dla danego selektora

Teraz, gdy znajdziesz selektor języka Objective-C, czego potrzebujesz do wywołania, następnym krokiem jest mapowanie równoważne członkowi języka C#. Istnieją cztery metody, można spróbować (kontynuowaniem `NSTableView CanDragRows` przykład):

1. Na liście uzupełniania automatycznie umożliwia szybkie skanowanie w poszukiwaniu coś o takiej samej nazwie. Ponieważ wiemy, jest wystąpieniem `NSTableView` możesz wpisać:

    - `NSTableView x;`
    - `x.` [ctrl + miejsce, jeśli nie ma listy).
    - `CanDrag` [Wprowadź]
    - Kliknij prawym przyciskiem myszy metodę, przejdź do deklaracji, otwórz przeglądarkę zestawu, w którym można porównać `Export` atrybutu do danego selektora

2. Wyszukaj powiązania całej klasy. Ponieważ wiemy, jest wystąpieniem `NSTableView` możesz wpisać:

    - `NSTableView x;`
    - Kliknij prawym przyciskiem myszy `NSTableView`, przejdź do deklaracji, przeglądarka zestawów
    - Wyszukaj w danym selektora

3. Możesz użyć [dokumentację interfejsu API platformy Xamarin.Mac w trybie online](https://docs.microsoft.com/dotnet/api/?view=xamarinmac-3.0) .

4. Miguel udostępnia widok "Rosetta kamień" interfejsy API platformy Xamarin.Mac [tutaj](http://tirania.org/tmp/rosetta.html) , można wyszukiwać za pośrednictwem danego interfejsu API. Jeśli Twój interfejs API nie jest AppKit lub macOS określonych, może go tam Znajdź.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
