---
title: "Sprawdź atrybutów"
ms.topic: article
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 10fb2e2824a05954e19f9b483884061b217be683
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="verify-attributes"></a>Sprawdź atrybutów


Dostępne są często, że powiązania utworzonego przez Sharpie cel adnotacją w `[Verify]` atrybutu. Te atrybuty wskazują, że należy _Sprawdź_ czy Sharpie cel została poprawny element porównując powiązania z oryginalnej deklaracji Objective-C-C (która będzie dostępna w komentarz powyżej deklaracji powiązanej).

Weryfikacja jest zalecane w przypadku _wszystkie_ powiązany deklaracje, ale najprawdopodobniej _wymagane_ dla deklaracji opatrzoną `[Verify]` atrybutu. Jest tak, ponieważ w wielu sytuacjach, nie ma wystarczającej ilości metadanych w oryginalny kod źródłowy natywnej do wywnioskowania jak najlepiej utworzyć wiązanie. Konieczne może odwoływać się do dokumentacji lub komentarze w kodzie wewnątrz pliki nagłówkowe najlepszych decyzji powiązania.

Po upewnieniu się, że powiązanie jest naprawić lub został rozwiązany, aby było poprawne, _Usuń_ `[Verify]` atrybut z powiązania.

> [!IMPORTANT]
> `[Verify]` atrybuty celowo powodować błędy kompilacji C#, dzięki czemu są wymuszone, aby sprawdzić powiązania. Należy usunąć `[Verify]` atrybutu po przejrzeniu (i prawdopodobnie poprawiona) kodu.

## <a name="verify-hints-reference"></a>Weryfikacja odwołania do wskazówki

Podany atrybut argument wskazówki można krzyżowego odwołuje się do dokumentacji poniżej. Dokumentacja dla każdego utworzonego `[Verify]` atrybuty będą znajdować się na konsoli, a także po zakończeniu wiązania.

|Sprawdź wskazówki|Opis|
|---|---|
|InferredFromPreceedingTypedef|Nazwa tej deklaracji została wykryta przez Konwencję wspólnego z bezpośrednio poprzedzający `typedef` w oryginalny kod źródłowy macierzystego. Sprawdź poprawność tę Konwencję jest niejednoznaczna nazwa wykrywany.|
|ConstantsInterfaceAssociation|Nie można określić, które interfejs Objective-C deklaracji zmiennych extern może być skojarzony idiotkę dowód jest. Wystąpienia te są powiązane jako `[Field]` właściwości częściowe interfejsu w pobliżu według konkretnego interfejsem do tworzenia bardziej intuicyjnego interfejsu API, prawdopodobnie wyeliminowanie "Stałe" całkowicie interfejsu.|
|MethodToProperty|Metody języka Objective-C została powiązana jako właściwość C# z powodu Konwencji, takie jak pobieranie żadnych parametrów i zwracanie wartości (z systemem innym niż void zwrot). Często metody, takie jak te należy powiązać jako właściwości powierzchni wrażeń interfejsu API, ale może wystąpić alarmów false i powiązania faktycznie powinna być metody.|
|StronglyTypedNSArray|Natywny `NSArray*` został powiązany jako `NSObject[]`. Może być możliwe do większego typu tablicy w powiązaniu oparte na oczekiwań ustawiana za pośrednictwem dokumentacji interfejsu API (np. komentarze w pliku nagłówka) lub przez badanie zawartości tablicy testy. Na przykład NSArray * zawierający tylko NSNumber * instancescan zostać powiązany jako `NSNumber[]` zamiast `NSObject[]`.|

Możesz szybko otrzymywać dokumentacji wskazówka przy użyciu `sharpie verify-docs` narzędzia, na przykład:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

