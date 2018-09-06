---
title: kontrolek interfejsu użytkownika systemu macOS platformie Xamarin.Mac
description: Ten zawiera dokument łącza do przewodników, które opisują różne kontrolek interfejsu użytkownika dostępnych dla deweloperów platformy Xamarin.Mac. Połączona zawartość zajmuje się z systemu windows, okna dialogowe, alerty, menu, paski narzędzi, widoki tabel, widoki konspektu i więcej.
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: b2392f05a03015f903918f15013919be14b99292
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780593"
---
# <a name="macos-user-interface-controls-in-xamarinmac"></a>kontrolek interfejsu użytkownika systemu macOS platformie Xamarin.Mac

_Ten artykuł zawiera łącza do przewodników, które opisują różne formanty interfejsu użytkownika systemu macOS._

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tego samego użytkownika interfejsu formantów, które Deweloper pracujący w *języka Objective-C* i *Xcode* jest. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, można użyć program Xcode _programu Interface Builder_ tworzenie i obsługa interfejsów użytkownika (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Wymienione poniżej przewodników: szczegółowe informacje na temat pracy z systemem macOS elementów interfejsu użytkownika w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w każdym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak wyjaśniono `Register` i `Export` atrybuty, które umożliwiają o komunikacji sieciowej w górę klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

Ten artykuł dotyczy pracy z systemem windows i paneli w aplikacji platformy Xamarin.Mac. Obejmuje on tworzenia i obsługi systemu windows, panele, Xcode i programu Interface Builder ładowanie systemu windows i panele z .storyboard lub .pliki plików, przy użyciu systemu windows i odpowiada do systemu windows w kodzie języka C#.

## <a name="dialogsmacuser-interfacedialogmd"></a>[Okna dialogowe](~/mac/user-interface/dialog.md)

Ten artykuł dotyczy pracy z okien dialogowych i okna modalne w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie modalnego okna w Xcode i programu Interface Builder, pracę w standardowych oknach dialogowych, wyświetlanie i odpowiada do systemu windows w kodzie języka C#.

## <a name="alertsmacuser-interfacealertmd"></a>[Alerty](~/mac/user-interface/alert.md)

W tym artykule opisano Praca z alertami w aplikacji platformy Xamarin.Mac. Obejmuje ona tworzenie i wyświetlanie alertów z kodu C# oraz reagowanie na alerty.

## <a name="menusmacuser-interfacemenumd"></a>[Menu](~/mac/user-interface/menu.md)

Menu są używane w różnych częściach interfejsu użytkownika aplikacji dla komputerów Mac; w menu głównym aplikacji w górnej części ekranu, aby wyskakujących menu i menu kontekstowe, które mogą występować w dowolnym miejscu w oknie. Menu są integralną częścią środowiska użytkownika aplikacji dla komputerów Mac. W tym artykule opisano Praca z menu Cocoa w aplikacji platformy Xamarin.Mac.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Kontrolki standardowe](~/mac/user-interface/standard-controls.md)

Praca z standardowych kontrolek AppKit, takie jak przyciski, etykiety, pola tekstowe, pola wyboru i kontrolki segmentowane w aplikacji platformy Xamarin.Mac. Ten przewodnik obejmuje dodanie ich do projektu interfejsu użytkownika, w program Xcode Interface Builder Uwidacznianie ich dla kodu za pośrednictwem gniazd i akcje i Praca z kontrolek AppKit w kodzie języka C#.

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Paski narzędzi](~/mac/user-interface/toolbar.md)

Ten artykuł dotyczy pracy z pasków narzędzi w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie paski narzędzi w programie Xcode i programu Interface Builder jak udostępniać elementy paska narzędzi do kodu przy użyciu gniazd i akcje, włączanie i wyłączanie elementów paska narzędzi i na koniec reagowanie na elementy paska narzędzi w kodzie języka C#.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Widoki tabel](~/mac/user-interface/table-view.md)

Ten artykuł dotyczy pracy z widoki tabel w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie widoki tabel w programie Xcode i programu Interface Builder sposób, aby udostępnić widok tabeli elementów do kodu przy użyciu gniazd i akcje, wypełnianie tabel, widoków i reagowanie na tabeli wyświetlania elementów w kodzie języka C#.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Widoki konspektu](~/mac/user-interface/outline-view.md)

Ten artykuł dotyczy pracy z widoki konspektu w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie widoki konspektu w środowisku Xcode i programu Interface Builder sposób, aby udostępnić widok konspektu elementów kodu przy użyciu gniazd i akcje, wypełnianie widoki konspektu i reagowanie na konturu wyświetlania elementów w kodzie języka C#.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Listy źródeł](~/mac/user-interface/source-list.md)

Ten artykuł dotyczy pracy z listy źródeł w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie listy źródeł w środowisku Xcode i programu Interface Builder jak udostępniać elementy listy źródeł do kodu przy użyciu gniazd i akcje, podczas wypełniania listy źródeł i reagowanie na elementy listy źródeł, w kodzie języka C#.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Widoki kolekcji](~/mac/user-interface/collection-view.md)

Ten artykuł dotyczy pracy za pomocą widoków kolekcji w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie widoki kolekcji w programie Xcode i programu Interface Builder sposób, aby udostępnić widok kolekcji elementów kodu przy użyciu gniazd i akcje, wypełnianie widoków kolekcji i reagowanie na widoki kolekcji w kodzie języka C#.

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[Tworzenie niestandardowych formantów](~/mac/user-interface/custom-controls.md)

W tym artykule opisano tworzenie formantów interfejsu użytkownika niestandardowego (przez dziedziczenie z `NSControl`), rysowanie niestandardowego interfejsu dla kontrolki i tworzenie niestandardowe akcje, które mogą być używane z program Xcode Interface Builder.

## <a name="mac-samples-gallery"></a>Galeria przykładów Mac

Również sugerować przeglądając [galerii przykładów Mac](https://developer.xamarin.com/samples/mac/all/). Zawiera wiele gotowych do użycia kodu, które mogą ułatwić szybkie rozpoczęcie projektów platformy Xamarin.Mac na wyższy.

## <a name="related-links"></a>Linki pokrewne

- [System macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
