---
title: Platformy Objective-C
ms.topic: article
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 17673c0d82f4e0a7c0b446bf51177441b1bdfbfa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="objective-c-platforms"></a>Platformy Objective-C


Osadzanie .NET można różnych platform docelowych podczas generowania kodu języka Objective-C:

* macOS
* iOS
* tvOS
* [nie zaimplementowano jeszcze] watchOS

Wybrano platformę przez przekazanie `--platform=<platform>` embeddinator argument wiersza polecenia.

Podczas kompilowania dla platform iOS i systemu tvOS watchOS, embeddinator zawsze utworzy platforma, która osadza Xamarin.iOS, ponieważ Xamarin.iOS zawiera wiele kod obsługi środowiska uruchomieniowego, co jest wymagane na tych platformach.

Jednak podczas kompilowania dla platformy macOS, jest możliwe wybranie, czy wygenerowane framework osadzić Xamarin.Mac lub nie. Istnieje możliwość osadzaj Xamarin.Mac Jeśli zestawu powiązania nie odwołuje się do Xamarin.Mac.dll (bezpośrednio lub pośrednio), a ta opcja jest zaznaczona przez przekazanie `--platform=macOS` do embeddinator.

Jeśli zestawu powiązania zawiera odwołanie do Xamarin.Mac.dll, konieczne jest osadzić Xamarin.Mac, a ponadto embeddinator musi wiedzieć, które platformy docelowej, aby użyć.

Istnieją trzy możliwe Xamarin.Mac docelowych platform: `modern` (wcześniej nazywane `mobile`), `full` i `system` (różnica między poszczególnymi jest opisana w jego Xamarin.Mac [platformy docelowej] [ 1] dokumentacji), i jest wybranych przez przekazanie `--platform=macOS-modern`, `--platform=macOS-full` lub `--platform=macOS-system` do embeddinator.

[1]: ~/mac/platform/target-framework.md
