---
title: Xamarin dla deweloperów języka Objective-C
description: Ten dokument zawiera opis platformy Xamarin.iOS dla deweloperów języka Objective-C. Łączy się on do przewodników, które opisują sposób przejścia do języka C# w języku Objective-C, jak powiązać biblioteki języka Objective-C do użycia w języku C# oraz tworzenie aplikacji mobilnych dla wielu platform.
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 574bd4c0b6bed639230dbfb6209eb78216e8e47b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351011"
---
# <a name="xamarin-for-objective-c-developers"></a>Xamarin dla deweloperów języka Objective-C

Oferty platformy Xamarin, ścieżka dla deweloperów korzystających z systemem iOS można przenieść interfejsu użytkownika inne niż kod niezależny od platformy C#, dzięki czemu mogą być używane wszędzie C# jest niedostępnych, w tym systemów Android, za pośrednictwem platformy Xamarin.Android i różnych odmian systemu Windows. Jednak po prostu, ponieważ używasz języka C# za pomocą platformy Xamarin nie oznacza, że nie mogą wykorzystać posiadane umiejętności i kod języka Objective-C. W rzeczywistości języka Objective-C, wiedząc, sprawia, że możesz lepiej dewelopera platformy Xamarin.iOS ponieważ Xamarin ujawnia natywnych dla systemów iOS i OS X, platforma interfejsów API, które znasz i lubisz, takich jak UIKit podstawowe funkcje animacji, Core Foundation i podstawowe funkcje grafiki kilka. W tym samym czasie możesz korzystać z języka C#, w tym funkcje takie jak LINQ i typy ogólne, a także zaawansowane .NET podstawowej biblioteki klas do użycia w aplikacji natywnej.

Ponadto Xamarin umożliwiają wykorzystanie istniejących języka Objective-C zasobów za pomocą technologii nazywana powiązania. Po prostu utworzyć bibliotekę statycznych w języku Objective-C i udostępnić ją C# za pomocą powiązania, jak pokazano na poniższym diagramie:

 [![](images/01-bindings.png "Biblioteka statyczna w języku Objective-C, uwidaczniany w języku C# za pomocą powiązania")](images/01-bindings.png#lightbox)

To nie musi być ograniczone do kodu bez interfejsu użytkownika. Powiązania może narazić opracowany w języku Objective-C również kod interfejsu użytkownika.

## <a name="transitioning-from-objective-c"></a>Przechodzenie ze języka Objective-C

Znajdziesz mnóstwo informacji w naszej witrynie dokumentacji, aby ułatwić tranistion środowiska xamarin, przedstawiający sposób integracji kodu C# za pomocą tego, co już znasz. Niektóre najważniejsze funkcje ułatwiające rozpoczęcie pracy obejmują:

-   [Elementarz języka C# dla deweloperów języka Objective-C](primer.md) — Krótki podręcznik dla deweloperów języka Objective-C, które chcesz przenieść do platformy Xamarin i języka C#. 
-   [Wskazówki: Powiązywanie biblioteki języka Objective-C](~/ios/platform/binding-objective-c/walkthrough.md) — przewodnik krok po kroku dla ponowne używanie istniejącego kodu języka Objective-C w aplikacji platformy Xamarin.iOS. 


## <a name="binding-objective-c"></a>Powiązania Objective-C

Gdy już opanujesz jak C# porównuje języka Objective-C i działały przez powyższe wskazówki powiązania, będzie odpowiednio przygotowane do przejścia do platformy Xamarin. Udzieli, bardziej szczegółowe informacje o technologiach powiązanie interfejsu Xamarin.iOS, łącznie z odwołaniem kompleksowe powiązania jest dostępna w [powiązania Objective-C](~/ios/platform/binding-objective-c/index.md) sekcji.

## <a name="cross-platform-development"></a>Tworzenie aplikacji wieloplatformowych

Na koniec należy po przejściu do rozwiązania Xamarin.iOS, zapoznaj się ze wskazówkami dla wielu platform, które mamy, w tym przypadków aplikacji odwołanie mamy devleoped oraz najlepsze rozwiązania dotyczące tworzenia wielokrotnego użytku dla wielu platform kod zawarty w [ Budowanie Cross Platform aplikacji](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
