---
title: Obsługa Touch w aplikacji platformy Xamarin.iOS
description: Ten dokument prowadzi do prowadnic, które opisują sposób pracy z touch, wielodotyku i gestów oraz technologii 3D Touch w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: eb8dce8b13345c13a6f95ae7784bd135e7d1f1f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784165"
---
# <a name="handling-touch-in-xamarinios-apps"></a>Obsługa Touch w aplikacji platformy Xamarin.iOS

Podobnie jak inne platformy mobilne dla systemu iOS ma kilka różnych sposobów do obsługi dotykowej. Obsługuje on wielodotyku — wiele punktów kontaktu na ekranie — i gestów złożonych. Ten przewodnik przedstawia niektóre z pojęć, a także szczegóły dotyczące implementowania touch i gestów w systemie iOS.

iOS hermetyzuje dane touch `UITouch` klasy, która ma zostać udostępnione aplikacji za pośrednictwem szereg `UIResponder` metody. Aplikacje można zastąpić tych metod w podklasach z `UIView` i `UIViewController`, które dziedziczą z `UIResponder`.

Oprócz przechwytywania danych touch z systemem iOS zapewnia oznacza interpretowanie wzorce poprawek do gestów. Z kolei można te aparaty rozpoznawania gestów interpretować polecenia specyficzne dla aplikacji, takie jak obrót obrazu lub Włącz strony. iOS udostępnia kolekcję sformatowanego klas do obsługi wspólnej gestów minimalna dodany kod.

Wybór między poprawek i aparatów rozpoznawania gestów mogą być mylące. W tym przewodniku zaleca się, że ogólnie rzecz biorąc, powinien preferowane aparaty rozpoznawania gestów. Aparaty rozpoznawania gestów są zaimplementowane jako odrębny klasy, które zapewniają większą rozdzielenie problemy i lepsze hermetyzacji. Dzięki temu proste udostępnianie logiki między różnymi widokami, minimalizując ilość kodu napisanego.

Jednak są potrzebne do przetwarzania touch niskiego poziomu, a nawet śledzić wiele palców, na przykład, aby utworzyć finger-paint program.

## <a name="sections"></a>Sekcje

-  [Dotyk w systemie iOS](touch-in-ios.md)
-  [Wskazówki: Używanie Touch w systemie iOS](ios-touch-walkthrough.md)
-  [Śledzenie wielodotyku](touch-tracking.md)

W tym przewodniku stanowi wprowadzenie do platformy Touch w systemie iOS. Aby uzyskać więcej informacji na temat używania 3D Touch i dotykowych opinie w systemie iOS, które zostały wprowadzone w systemie iOS 9 i 10 odpowiednio, zapoznaj się z przewodnikami określonych poniżej:

* [3D Touch](~/ios/platform/3d-touch.md)
* [Przekazywanie opinii dotykowych](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>Linki pokrewne

- [iOS Touch Start (przykład)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS Touch końcowego (na przykład)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [FingerPaint (przykład)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
