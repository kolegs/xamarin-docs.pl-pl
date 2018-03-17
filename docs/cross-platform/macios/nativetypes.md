---
title: Typy natywne
ms.topic: article
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 09858bd7902b44bbedd96f1be9c9c827131ee16f
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="native-types"></a>Typy natywne

Fundament różnica zarówno Mac i interfejsów API systemu iOS Użyj typów danych architektury, które są zawsze 32-bitowych na platformach 32-bitowe i 64-bitowych na platformach 64 bitowych.

Na przykład mapuje Objective-C `NSInteger` typ danych `int32_t` w systemach 32-bitowe i do `int64_t` w systemach 64-bitowych.

Aby dopasować to zachowanie na naszych ujednolicony interfejs API, możemy zastępowanie wcześniejszych zastosowań `int` (który w .NET jest zdefiniowany jako zawsze `System.Int32`) na nowy typ danych: `System.nint`.  Można traktować "n" jako znaczenie "native", więc natywnych liczb całkowitych typu platformy.

Z tych nowych typów danych ten sam kod źródłowy jest kompilowany dla 32-bitowy, 32-bitowe i 64-bitowy lub 64-bitowy, w zależności od flagi kompilacji.

## <a name="new-data-types"></a>Nowe typy danych

W poniższej tabeli przedstawiono zmian w naszej typy danych do dopasowania tego nowego świata 32/x 64:

|Typ macierzysty|32-bitowe zapasowy typ|64-bitowych zapasowy typ|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

Wybraliśmy tych nazw, aby umożliwić kodu C# do wyglądać mniej więcej tak samo jak będzie wyglądać dzisiaj.

### <a name="implicit-and-explicit-conversions"></a>Konwersje jawne i niejawne

Nowe typy danych została zaprojektowana umożliwia jednego języka C# plik źródłowy naturalnie używać 32 lub 64 bitowej magazynu, w zależności od platformę hosta oraz ustawienia kompilacji.

Wymagane firmie Microsoft w celu projektowania zestaw Konwersje jawne i niejawne do i z typów specyficzne dla platformy danych do typów danych całkowitych i zmiennoprzecinkowych punkt .NET.

Niejawne konwersje operatorów są dostarczane, gdy nie było możliwości utraty danych (32-bitowe wartości są przechowywane w miejscu do 64-bitowe).

Jawne konwersje operatorów są dostarczane, gdy istnieje możliwość utraty danych (64-bitowa wartość jest przechowywane na lokalizację przechowywania 32 lub potencjalnie 32).

 `int`, `uint` i `float` są wszystkie umożliwiają niejawnej konwersji na `nint`, `nuint` i `nfloat` 32 lub 64 bitów zawsze pasujące 32-bitowy.

 `nint`, `nuint` i `nfloat` są wszystkie umożliwiają niejawnej konwersji na `long`, `ulong` i `double` jako wartości bitowe 32 lub 64 zawsze mieści się w magazynie 64-bitowym.

Należy użyć jawne konwersje z `nint`, `nuint` i `nfloat` do `int`, `uint` i `float` ponieważ natywnych typów może pomieścić 64-bitowy magazynu.

Należy użyć jawne konwersje z `long`, `ulong` i `double` do `nint`, `nuint` i `nfloat` ponieważ natywnych typów może mieć tylko do przechowywania 32-bitowy magazynu.

## <a name="coregraphics-types"></a>Typy CoreGraphics

Typy danych punktu, rozmiar i prostokąt, które są używane z CoreGraphics Użyj 32 lub 64 bitów w zależności od urządzenia, na którym jest uruchomiona na.  Gdy firma Microsoft pierwotnie powiązana z systemami iOS i Mac interfejsów API użyliśmy istniejącej struktury danych, które wystąpiły, aby dopasować rozmiary platformę hosta (typy danych w `System.Drawing`).

Podczas przenoszenia na **Unified**, należy zastąpić wystąpienia `System.Drawing` z ich `CoreGraphics` odpowiedników, jak pokazano w poniższej tabeli:

|Stary typ w System.Drawing|Nowe CoreGraphics typu danych|Opis|
|--- |--- |--- |
|`RectangleF`|`CGRect`|Liczby zmiennoprzecinkowe informacji dotyczących punktów prostokąta blokad.|
|`SizeF`|`CGSize`|Informacje o rozmiarze (szerokość, wysokość) punktu blokad liczby zmiennoprzecinkowe|
|`PointF`|`CGPoint`|Przechowuje zmiennoprzecinkowych, punktu informacje (X, Y)|

Stary elementów przestawnych typów używanych danych do przechowywania elementów struktury danych podczas nowej używa jednego `System.nfloat`.

## <a name="related-links"></a>Linki pokrewne

- [Praca z typami natywnymi w aplikacjach międzyplatformowych](~/cross-platform/macios/native-types-cross-platform.md)
- [Klasycznym vs różnice Unified API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
