---
title: Touch
description: Ekranów dotykowych na wielu urządzeniach współczesnych Zezwalaj użytkownikom na interakcję z urządzeniami, szybkie i skuteczne jak fizyczną i intuicyjne. Ta interakcja nie jest ograniczona tylko w celu wykrywania touch proste — można również gesty. Na przykład gestu powiększanie gestem uszczypnięcia jest bardzo typowym przykładem tego — punkty zaciskające części ekranu z dwoma palcami, które użytkownik może powiększanie lub pomniejszanie. W tym przewodniku sprawdza touch i gestów w systemie iOS.
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: f34b502e3c0d67f33d41bc489f7ec1d93356af99
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="touch"></a>Touch

_Ekranów dotykowych na wielu urządzeniach współczesnych Zezwalaj użytkownikom na interakcję z urządzeniami, szybkie i skuteczne jak fizyczną i intuicyjne. Ta interakcja nie jest ograniczona tylko w celu wykrywania touch proste — można również gesty. Na przykład gestu powiększanie gestem uszczypnięcia jest bardzo typowym przykładem tego — punkty zaciskające części ekranu z dwoma palcami, które użytkownik może powiększanie lub pomniejszanie. W tym przewodniku sprawdza touch i gestów w systemie iOS._


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
