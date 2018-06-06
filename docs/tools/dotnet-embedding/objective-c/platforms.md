---
title: Platformy Objective-C
description: W tym dokumencie opisano różne platformy, które osadzanie .NET można kierować podczas pracy z kodu języka Objective-C. Zawarto informacje macOS, iOS, systemu tvOS i watchOS.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 6eeb776959d1a2a37d67bfae6603971d0e5a22b7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793894"
---
# <a name="objective-c-platforms"></a>Platformy Objective-C

Osadzanie .NET można różnych platform docelowych podczas generowania kodu języka Objective-C:

* macOS
* iOS
* tvOS
* [nie zaimplementowano jeszcze] watchOS

Wybrano platformę przez przekazanie `--platform=<platform>` argumentu wiersza polecenia do osadzania .NET.

Podczas kompilowania dla systemu iOS, systemu tvOS i watchOS platformy .NET osadzanie zawsze utworzy platforma, która osadza Xamarin.iOS, ponieważ Xamarin.iOS zawiera wiele kod obsługi środowiska uruchomieniowego, co jest wymagane na tych platformach.

Jednak podczas kompilowania dla platformy macOS, jest możliwe wybranie, czy wygenerowane framework osadzić Xamarin.Mac lub nie. Istnieje możliwość osadzaj Xamarin.Mac Jeśli zestawu powiązania nie odwołuje się do Xamarin.Mac.dll (bezpośrednio lub pośrednio), a ta opcja jest zaznaczona przez przekazanie `--platform=macOS` do narzędzia do osadzania .NET.

Jeśli zestawu powiązania zawiera odwołanie do Xamarin.Mac.dll, konieczne jest osadzić Xamarin.Mac, a ponadto embeddinator musi wiedzieć, które platformy docelowej, aby użyć.

Istnieją trzy możliwe Xamarin.Mac docelowych platform: `modern` (wcześniej nazywane `mobile`), `full` i `system` (różnica między poszczególnymi jest opisana w jego Xamarin.Mac [platformy docelowej] [ 1] dokumentacji), i jest wybranych przez przekazanie `--platform=macOS-modern`, `--platform=macOS-full` lub `--platform=macOS-system` do narzędzia do osadzania .NET.

[1]: ~/mac/platform/target-framework.md
