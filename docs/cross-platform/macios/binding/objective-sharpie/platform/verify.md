---
title: Narzędzie Objective Sharpie weryfikowanie atrybutów
description: W tym dokumencie opisano atrybut [Sprawdź] generowane przez Objective Sharpie. Atrybut [Sprawdź] prezentuje deweloperom, gdzie one należy ręcznie zweryfikować Objective Sharpie danych wyjściowych.
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4bca896afb4dfc96fd6c1d7cdf489feb6a879e31
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855032"
---
# <a name="objective-sharpie-verify-attributes"></a>Narzędzie Objective Sharpie weryfikowanie atrybutów

Często znajdziesz, że powiązania produkowane przez Objective Sharpie zostanie oznaczony za pomocą `[Verify]` atrybutu. Te atrybuty wskazują, że należy _Sprawdź_ czy Objective Sharpie czy poprawny element porównując powiązania z oryginalnej deklaracji Objective-C-C (który będzie świadczona w komentarzu powyżej deklaracji powiązanej).

Weryfikacja jest zalecane w przypadku _wszystkich_ powiązany deklaracji, ale najprawdopodobniej _wymagane_ dla deklaracji oznaczony za pomocą `[Verify]` atrybutu. Jest to spowodowane w wielu sytuacjach, w nie ma wystarczającej ilości metadanych oryginalnego kodu źródłowego natywnej w celu jak najlepiej utworzyć wiązanie. Konieczne może odwoływać się do dokumentacji lub komentarzy do kodu wewnątrz pliki nagłówkowe najlepszych decyzji powiązania.

Po upewnieniu się, że powiązanie jest Popraw lub naprawiona, aby było poprawne, _Usuń_ `[Verify]` atrybut z powiązania.

> [!IMPORTANT]
> `[Verify]` atrybuty celowo spowodować błędy kompilacji C#, dzięki czemu jest wymuszone, aby sprawdzić powiązania. Należy usunąć `[Verify]` atrybutu po przejrzeniu (i prawdopodobnie poprawiona) kod.

## <a name="verify-hints-reference"></a>Sprawdź wskazówki odwołania

Podany atrybut argument wskazówki może być między, do którego odwołuje się poniższą dokumentacją. Dokumentację dotyczącą dowolnego produkowane `[Verify]` atrybuty będzie świadczona w konsoli, a także po zakończeniu wiązania.

|`[Verify]` Wskazówka|Opis|
|---|---|
|InferredFromPreceedingTypedef|Nazwa tej deklaracji została wykryta przez Konwencję wspólne, z bezpośrednio poprzedzający `typedef` oryginalnego kodu źródłowego natywnych. Sprawdź, czy wykrywany nazwa jest poprawna, jak ta Konwencja jest niejednoznaczny.|
|ConstantsInterfaceAssociation|Nie ma idiotkę dowód możliwości można określić za pomocą interfejsu języka Objective-C, które mogą być skojarzone deklaracji zmiennej extern. Te wystąpienia są powiązane jako `[Field]` właściwości w interfejs częściowy w pobliżu przez konkretne interfejs do tworzenia bardziej intuicyjny interfejs API, prawdopodobnie wyeliminowanie "Stałe" interfejs całkowicie.|
|MethodToProperty|Metoda języka Objective-C był powiązany jako właściwość języka C# ze względu na Konwencję, takich jak wykonanie żadnych parametrów i zwraca wartość (inny niż void zwrot). Często metod, takich jak te powinna być powiązana jako właściwości powierzchni wrażeń interfejsu API, ale czasami może wystąpić fałszywych alarmów i powiązanie powinna być faktycznie metody.|
|StronglyTypedNSArray|Natywny `NSArray*` był powiązany jako `NSObject[]`. Może być możliwe do mocno typu tablicy w powiązaniu oparte na oczekiwania ustawiana za pośrednictwem dokumentacji interfejsu API (np. komentarze w pliku nagłówkowym) lub, sprawdzając zawartość tablicy za pomocą testowania. Na przykład NSArray * zawierający tylko NSNumber * instancescan zostać powiązany jako `NSNumber[]` zamiast `NSObject[]`.|

Możesz również szybko otrzymywać dokumentacji wskazówka przy użyciu `sharpie verify-docs` narzędzia, na przykład:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

## <a name="related-links"></a>Linki pokrewne

- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
