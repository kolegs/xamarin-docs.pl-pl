---
title: Xamarin dla deweloperów języka Objective C
description: Ten dokument zawiera opis Xamarin.iOS dla deweloperów języka Objective-C. Łączy z przewodników, które opisują jak do przejścia do języka C# z języka Objective-C, jak można powiązać biblioteka języka Objective-C do użycia w języku C# i sposobu tworzenia i platform aplikacji mobilnej.
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 027ef8cabc55ace41bcf201b8355aab8ca6ec625
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786054"
---
# <a name="xamarin-for-objective-c-developers"></a>Xamarin dla deweloperów języka Objective C

Oferty Xamarin ścieżki dla deweloperów korzystających z systemem iOS można przenieść interfejsu użytkownika innych niż kodu niezależny od platformy C#, dzięki czemu mogą być używane wszędzie C# jest dostępna, łącznie z systemem Android za pośrednictwem platformy Xamarin.Android i różnych odmian systemu Windows. Jednak po prostu, ponieważ używasz C# za pomocą platformy Xamarin nie oznaczają, że nie mogą wykorzystać posiadane umiejętności i kod języka Objective-C. W rzeczywistości znajomość języka Objective-C sprawia, że można lepiej deweloperów platformy Xamarin.iOS ponieważ Xamarin przedstawia wszystkich natywnych platform iOS i OS X interfejsów API należy znają i lubią, takich jak UIKit, Core animacji, Core Foundation i Core grafiki kilka. W tym samym czasie otrzymasz możliwości języka C#, włączając funkcje jak LINQ i typy ogólne, a także sformatowanego .NET podstawowej biblioteki klas do użycia w natywnej aplikacji.

Ponadto Xamarin umożliwia wykorzystanie istniejącej Objective-C zasobów za pomocą technologii wiedzieć, jak powiązania. Po prostu Utwórz bibliotekę statyczną w języku Objective C i ujawnienia go do języka C# za pomocą powiązania, jak pokazano na poniższym diagramie:

 [![](images/01-bindings.png "Biblioteka statyczna w języku Objective-C ujawniony dla C# za pomocą powiązania")](images/01-bindings.png#lightbox)

To nie musi być ograniczone do kodu bez interfejsu użytkownika. Powiązania mogą uwidaczniać opracowane w języku Objective C również kod interfejsu użytkownika.

## <a name="transitioning-from-objective-c"></a>Przejście z języka Objective C

Znajdziesz nadmiar informacji w naszej witrynie dokumentacji w celu ułatwienia tranistion do platformy Xamarin, pokazujący sposób integracji kod w języku C# z już informacji. Niektóre najważniejsze funkcje ułatwiające rozpoczęcie pracy obejmują:

-   [Elementarz C# dla programistów języka Objective-C](primer.md) — krótka Elementarz dla deweloperów języka Objective-C chce przenieść do platformy Xamarin i języka C#. 
-   [Wskazówki: Powiązywanie biblioteka języka Objective-C](~/ios/platform/binding-objective-c/walkthrough.md) — przewodnik krok po kroku dla ponowne wykorzystywanie istniejących kodu języka Objective-C w aplikacji platformy Xamarin.iOS. 


## <a name="binding-objective-c"></a>Powiązanie Objective-C

Po ujmij o jak C# porównuje Objective-C i działały za pomocą wskazówki powiązanie powyżej, będziesz w dobrej kształtu przejście do platformy Xamarin. Jako flaga monitująca bardziej szczegółowych informacji dotyczących technologii powiązania Xamarin.iOS, łącznie z odwołaniem kompleksowe powiązania jest dostępna w [powiązania Objective-C](~/ios/platform/binding-objective-c/index.md) sekcji.

## <a name="cross-platform-development"></a>Tworzenie aplikacji wieloplatformowych

Na koniec należy po przeniesieniu do Xamarin.iOS, zapoznaj się ze wskazówkami między platformami, który mamy, w tym wdrożeń aplikacji odwołanie mamy devleoped oraz najlepsze rozwiązania dotyczące tworzenia wielokrotnego użytku, międzyplatformowa kod zawarty w [ Tworzenie aplikacji dla wielu Platform sekcji](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
