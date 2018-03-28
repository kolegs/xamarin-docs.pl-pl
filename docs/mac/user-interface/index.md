---
title: System macOS interfejsu użytkownika
description: Ten artykuł zawiera łącza do prowadnic opisujących różne macOS kontrolki interfejsu użytkownika.
ms.topic: article
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: 2ad3d37e1a191423e0a9d744be2329870ba732d3
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/28/2018
---
# <a name="macos-user-interface"></a>System macOS interfejsu użytkownika

_Ten artykuł zawiera łącza do prowadnic opisujących różne macOS kontrolki interfejsu użytkownika._

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tego samego użytkownika interfejsu formantów, które osoba używająca *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć w środowisku Xcode _konstruktora interfejsu_ tworzenie i obsługa interfejsów użytkownika (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Przewodniki wymienionych poniżej zapewniają szczegółowe informacje na temat pracy z macOS elementy interfejsu użytkownika w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będziemy używać w każdym artykule.

Można przyjrzeć [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również, jak wyjaśniono `Register` i `Export` atrybuty używane do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

W tym artykule omówiono pracy z systemem windows i panele w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i utrzymywanie i panele z .storyboard lub .xib plików systemu windows oraz paneli w środowisku Xcode i konstruktora interfejsu, podczas ładowania systemu windows, przy użyciu systemu windows i odpowiada do systemu windows w kodzie języka C#.

## <a name="dialogsmacuser-interfacedialogmd"></a>[Okna dialogowe](~/mac/user-interface/dialog.md)

Ten artykuł dotyczy pracy z okien dialogowych i modalnych okien aplikacji Xamarin.Mac. Obejmuje ona tworzenie i obsługę modalne systemu windows w środowisku Xcode i interfejs konstruktora, Praca z standardowych oknach dialogowych oraz wyświetlanie i odpowiada do systemu windows w kodzie języka C#.

## <a name="alertsmacuser-interfacealertmd"></a>[Alerty](~/mac/user-interface/alert.md)

W tym artykule omówiono Praca z alertami w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i wyświetlanie alertów z kodu C# oraz reagowanie na alerty.

## <a name="menusmacuser-interfacemenumd"></a>[Menu](~/mac/user-interface/menu.md)

Menu są używane w różnych części interfejsu użytkownika aplikacji Mac; z menu głównego aplikacji w górnej części ekranu w menu podręcznego i menu kontekstowe, które mogą występować w dowolnym miejscu w oknie. Menu są integralną częścią aplikacji Mac środowisko użytkownika. W tym artykule omówiono Praca z menu Cocoa w aplikacji Xamarin.Mac.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Formanty standardowe](~/mac/user-interface/standard-controls.md)

Praca z standardowych formantów AppKit, takie jak przyciski, etykiet pól tekstowych, pola wyboru i segmentowanych formantów w aplikacji Xamarin.Mac. Ten przewodnik obejmuje dodanie ich do projektu interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode, ujawnienia ich do kodu za pomocą akcji i gniazda i Praca z kontrolkami AppKit w kodzie języka C#.

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Paski narzędzi](~/mac/user-interface/toolbar.md)

Ten artykuł dotyczy pracy z paski narzędzi w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę paski narzędzi w środowisku Xcode i konstruktora interfejsu, jak udostępnianie elementów paska narzędzi do kodu przy użyciu akcji i gniazda, włączanie i wyłączanie elementów paska narzędzi i finally reagowanie na elementów paska narzędzi w kodzie języka C#.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Widoki tabel](~/mac/user-interface/table-view.md)

Ten artykuł dotyczy pracy z widoków tabel w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę widoków tabel w środowisku Xcode i konstruktora interfejsu, jak do udostępnienia widoku tabeli elementy kodu za pomocą akcji i gniazda, podczas wypełniania tabeli widoków i reagowanie na tabeli Wyświetl elementy w kodzie języka C#.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Widoki konspektu](~/mac/user-interface/outline-view.md)

W tym artykule omówiono pracy z widokami konspektu w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę konspektu widoków w programie Xcode i kompilatora interfejsu jak do udostępnienia widoku konspektu elementy kodu za pomocą akcji i gniazda, podczas wypełniania widoków konspektu i reagowanie na konspektu Wyświetl elementy w kodzie języka C#.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Źródło listy](~/mac/user-interface/source-list.md)

Ten artykuł dotyczy pracy z listy źródła w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę list źródła w środowisku Xcode i konstruktora interfejsu jak udostępnianie elementów listy źródła do kodu za pomocą akcji i gniazda, podczas wypełniania listy źródłowej i reagowanie na elementy listy źródła w kodzie języka C#.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Kolekcja widoków](~/mac/user-interface/collection-view.md)

Ten artykuł dotyczy pracy z widokiem kolekcji w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługa kolekcji widoków w programie Xcode i kompilatora interfejsu jak do udostępnienia widok kolekcji elementy kodu za pomocą akcji i gniazda, podczas wypełniania widoków kolekcji i reagowanie na widoki kolekcji w kodzie języka C#.

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[Tworzenie niestandardowych formantów](~/mac/user-interface/custom-controls.md)

W tym artykule opisano tworzenie formantów interfejsu użytkownika niestandardowego (przez dziedziczenie z `NSControl`), rysowania niestandardowy interfejs dla formantu i tworzenie niestandardowe akcje, które mogą być używane z konstruktora interfejsu w środowisku Xcode.

## <a name="mac-samples-gallery"></a>Galeria przykładów Mac

Również sugerować biorąc przyjrzeć się [galerii przykładów Mac](https://developer.xamarin.com/samples/mac/all/). Zawiera wiele gotowych do użycia kodu, które pomaga szybko uzyskać projektu Xamarin.Mac poza podstaw.

## <a name="related-links"></a>Linki pokrewne

- [System macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
