---
title: Dokumentacja pokrewna
description: "Linki do dodatkowej dokumentacji dla deweloperów macOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0a282c58-1c37-4f73-8440-85de2daf454a
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: 000061d8ed884f1eb248517c6131ec49134cb8a1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="related-documentation"></a>Dokumentacja pokrewna

Oprócz sekcji Mac [developer.xamarin.com](~/mac/get-started/index.md) istnieją trzy doskonałe źródła dokumentacji, więc można też pomocy z Xamarin.Mac pytania:

- [**Dokumentacja platformy Xamarin.iOS** ](~/ios/get-started/index.md) — w przypadku wielu interfejsów API (przede wszystkim poza AppKit/UIKit) są tylko niewielkie różnice między wersjami systemu iOS i macOS. W niektórych przypadkach, gdy dany interfejs API systemu iOS ma nazwę `UIFoo`, interfejs API podobne o nazwie `NSFoo` można znaleźć w macOS. Poniższe przykłady będzie zazwyczaj w języku C# już.

- **Firmy Apple [Centrum deweloperów Mac](https://developer.apple.com/devcenter/mac/)**  — wiele razy przykład jakie interfejsów API do wywoływania w języku Objective C można przekonwertować na C# w sposób proste. Zobacz [interfejsy API interpretacji Mac](~/mac/app-fundamentals/mac-apis.md) szczegółowe informacje na temat sposobu wykonania tego zadania.

- [**Przepełnienie stosu** ](http://stackoverflow.com/) -doskonałą pomocą podczas istnieje tylko jedna poza pytania, takie jak ["jak automatycznie Rozwiń wszystkie węzły w NSOutlineView"](http://stackoverflow.com/questions/519751/nsoutlineview-auto-expand-all-nodes). Często będzie te przykłady w języku Objective C i trzeba być konwertowane na język C#, ale jest podzbiorem odpowiedzi w języku C#.

## <a name="user-interface"></a>Interfejs użytkownika

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac dewelopera ma dostęp do tego samego interfejsu użytkownika formantów, które osoba używająca *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, deweloper może użyć w środowisku Xcode _konstruktora interfejsu_ tworzenie i obsługa interfejsu użytkownika aplikacji (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Przewodniki wymienionych poniżej zapewniają szczegółowe informacje na temat pracy z elementami macOS w aplikacji Xamarin.Mac:

- [Windows](~/mac/user-interface/window.md)
- [Okna dialogowe](~/mac/user-interface/dialog.md)
- [Alerty](~/mac/user-interface/alert.md)
- [Menu](~/mac/user-interface/menu.md)
- [Paski narzędzi](~/mac/user-interface/toolbar.md)
- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Widoki konspektu](~/mac/user-interface/outline-view.md)
- [Źródło listy](~/mac/user-interface/source-list.md)
