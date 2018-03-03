---
title: "Interfejs użytkownika"
description: "Ten artykuł zawiera łącza do prowadnic opisujących różne macOS kontrolki interfejsu użytkownika."
ms.topic: article
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: a8cb9488849dafc2cd720ecf59d654009a9ad781
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="user-interface"></a>Interfejs użytkownika

_Ten artykuł zawiera łącza do prowadnic opisujących różne macOS kontrolki interfejsu użytkownika._

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tego samego interfejsu użytkownika formantów, które osoba używająca *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć w środowisku Xcode _konstruktora interfejsu_ tworzenie i obsługa interfejsów użytkownika (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#). 

Przewodniki wymienionych poniżej zapewniają szczegółowe informacje na temat pracy z macOS elementy interfejsu użytkownika w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będziemy używać w każdym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` poleceń używany do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

W tym artykule omówiono pracy z systemem Windows i panele w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę systemu Windows i panele w Konstruktorze Xcode i interfejs podczas ładowania systemu Windows i panele z `.storyboard` lub `.xib` plików przy użyciu systemu Windows i odpowiada do systemu Windows w kodzie języka C#.

## <a name="dialogsmacuser-interfacedialogmd"></a>[Okna dialogowe](~/mac/user-interface/dialog.md)

W tym artykule omówiono pracy z okien dialogowych i okna modalne w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i obsługę okna modalne Xcode i interfejsu konstruktora, Praca z standardowych oknach dialogowych, wyświetlanie i odpowiada do systemu Windows w kodzie języka C#.

## <a name="alertsmacuser-interfacealertmd"></a>[Alerty](~/mac/user-interface/alert.md)

W tym artykule omówiono Praca z alertami w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i wyświetlanie alertów z kodu C# oraz reagowanie na alerty.

## <a name="menusmacuser-interfacemenumd"></a>[Menu](~/mac/user-interface/menu.md)

Menu są używane w różnych części interfejsu użytkownika aplikacji Mac; z menu głównego aplikacji w górnej części ekranu w menu podręcznego i kontekstowe, które mogą występować w dowolnym miejscu w oknie. Menu są integralną częścią aplikacji Mac środowisko użytkownika. W tym artykule omówiono Praca z menu Cocoa w aplikacji Xamarin.Mac.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Kontrolki standardowe](~/mac/user-interface/standard-controls.md)

Praca z standardowych formantów AppKit takie jak przyciski, etykiety, pola tekstowego, pola wyboru i Segmentowanych formantów w aplikacji Xamarin.Mac. Ten przewodnik obejmuje dodanie ich do projektu interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode, ujawnienia ich do kodu za pomocą akcji i gniazda i Praca z kontrolkami AppKit w kodzie języka C#.

 
## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Paski narzędzi](~/mac/user-interface/toolbar.md)

Ten artykuł dotyczy pracy z paski narzędzi w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę paski narzędzi w Konstruktorze Xcode i interfejsu, jak udostępnić elementów paska narzędzi do kodu za pomocą akcji i gniazda, włączanie i wyłączanie elementów paska narzędzi oraz ostatecznie odpowiada na żądania elementów paska narzędzi w kodzie języka C#.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Widoki tabel](~/mac/user-interface/table-view.md)

Ten artykuł dotyczy pracy z widoków tabel w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę widoków tabel w Konstruktorze Xcode i interfejsu, jak udostępnianie tabeli wyświetlanie elementów do kodu za pomocą akcji i gniazda, podczas wypełniania tabeli widoków i koniec odpowiada na tabeli Wyświetl elementy w kodzie języka C#.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Widoki konspektu](~/mac/user-interface/outline-view.md)

W tym artykule omówiono pracy z widokami konspektu w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę konspektu widoków w programie Xcode i interfejsu builder, jak udostępnianie wyświetlanie konspektu elementów do kodu za pomocą akcji i gniazda, podczas wypełniania elementy konspektu i finally odpowiada na żądania konspektu Wyświetl elementy w kodzie języka C#.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Listy źródeł](~/mac/user-interface/source-list.md)

W tym artykule omówiono pracy z listami źródła w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę wymieniono źródła w środowisku Xcode i interfejsu konstruktora, jak udostępnianie źródło zawiera listę elementów do kodu za pomocą akcji i gniazda, wypełnianie źródła elementów listy i na koniec reagowanie na elementy listy źródeł w kodzie języka C#.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Widoki kolekcji](~/mac/user-interface/collection-view.md)

Ten artykuł dotyczy pracy z widokiem kolekcji w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę widoki kolekcji w programie Xcode i interfejsu builder, jak udostępnianie elementów widok kolekcji do kodu przy użyciu akcji i gniazda, podczas wypełniania widoków kolekcji i na koniec reagowanie na widoki kolekcji w kodzie języka C#.

## <a name="creating-custom-user-controlsmacuser-interfacecustom-controlsmd"></a>[Tworzenie kontrolki użytkownika niestandardowego](~/mac/user-interface/custom-controls.md)

W tym artykule opisano tworzenie niestandardowych kontrolek interfejsu użytkownika (przez dziedziczenie z `NSControl`), rysowania niestandardowy interfejs dla formantu i tworzenie niestandardowe akcje, które mogą być używane z konstruktora interfejsu w środowisku Xcode.

## <a name="mac-samples-gallery"></a>Galeria przykładów Mac

Również sugerować biorąc przyjrzeć się [galerii przykładów Mac](http://developer.xamarin.com/samples/mac/all/), zawiera wiele gotowych do użycia kodu, które pomaga szybko uzyskać projektu Xamarin.Mac poza podstaw.

## <a name="related-links"></a>Linki pokrewne

- [System macOS Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
