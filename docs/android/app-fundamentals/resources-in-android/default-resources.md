---
title: Domyślne zasoby
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 119aa8b967ace858ee56f521624f6356e08a8b80
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="default-resources"></a>Domyślne zasoby

Zasoby domyślne są elementy, które nie są specyficzne dla każdego określonego urządzenia lub obudowie i dlatego są domyślnie wybierana przez system operacyjny Android Jeśli żadne dokładniejsze znajdują się zasoby. Tak są one najczęściej spotykanym typem zasobu do utworzenia. Są one podzielone na podkatalogów z **zasobów** zgodnie z ich typ zasobu katalogu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Domyślne pliki zasobów](default-resources-images/01-resource-files-vs.png)

Na ilustracji powyżej projektu zawiera wartości domyślne dla obiektów drawable zasobów, układów i wartości (pliki XML, które zawierają wartości prosty).

Pełną listę typów zasobów znajduje się poniżej:

-  **Zresztą** &ndash; plików XML, które opisują właściwości animacji.
   Animacji właściwość wprowadzono w programie poziom interfejsu API 11 (3.0 dla systemu Android) i udostępnia dla animacji właściwości obiektu. Właściwości animacji są bardziej elastycznym i wydajnym sposobem opisują animacji na obiekty dowolnego typu.

-  **anim** &ndash; plików XML, które opisują *animacji* animacji. Animacji są szereg instrukcji animacji w celu wykonania przekształcenia na zawartość widoku obrót obiektu lub przykład obrazu lub rośnie rozmiar tekstu. Animacji są ograniczone do wyświetlania tylko obiektów.

-  **kolor** &ndash; pliki XML, opisujące stan listę kolorów. Aby zrozumieć Wyświetla stan kolorów, warto rozważyć element widget interfejsu użytkownika, takie jak przycisk.
   Może mieć różne stany, takich jak naciśnięcie lub wyłączona, a przycisk może zmienić kolor przy każdej zmianie stanu. Listy jest wyrażona w postaci listy stanu.

-  **obiektów drawable** &ndash; obiektów Drawable zasoby są ogólne koncepcji grafiki, która może być skompilowany w aplikacji, a następnie dostęp do interfejsu API lub odwołuje się inne zasoby XML.
   Przykłady drawables są pliki map bitowych (PNG, GIF, jpg), specjalne map bitowych o zmiennych rozmiarach, znany jako [dziewięć poprawki](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), stan listy ogólnego kształtów zdefiniowane w XML itp.
 
-  **Układ** &ndash; plików XML, które opisują układ interfejsu użytkownika, takie jak działania lub wiersz w postaci listy.

-  **menu** &ndash; plików XML, które opisują menu aplikacji, takich jak *opcje menu*, *menu kontekstowe*, i *podmenu*. Na przykład menu zobacz [pokaz Menu podręcznego](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) lub [standardowych formantów](https://developer.xamarin.com/samples/mobile/StandardControls/) próbki.

-  **nieprzetworzona** &ndash; dowolne pliki, które są zapisywane w postaci raw, binary. Te pliki są kompilowane do aplikacji systemu Android w formacie binarnym.

-  **wartości** &ndash; plików XML, które zawierają wartości prostego. Plik XML w katalogu wartości nie definiuje pojedynczego zasobu, ale zamiast tego można zdefiniować wiele zasobów. Na przykład jeden plik XML może utrzymywać listę wartości ciągu, podczas gdy inny plik XML może utrzymywać listę wartości kolorów.

-  **XML** &ndash; plików XML, które przypominają w funkcji pliki konfiguracji platformy .NET. Są to dowolne XML, który może zostać odczytany przez aplikację w czasie wykonywania


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Domyślne pliki zasobów](default-resources-images/01-resource-files-xs.png)

Na ilustracji powyżej projektu zawiera wartości domyślne dla obiektów drawable zasobów, układów i wartości (pliki XML, które zawierają wartości prosty).

Pełną listę typów zasobów znajduje się poniżej:

-  **Zresztą** &ndash; plików XML, które opisują właściwości animacji.
   Animacji właściwość wprowadzono w programie poziom interfejsu API 11 (3.0 dla systemu Android) i udostępnia dla animacji właściwości obiektu. Właściwości animacji są bardziej elastycznym i wydajnym sposobem opisują animacji na obiekty dowolnego typu.

-  **anim** &ndash; plików XML, które opisują *animacji* animacji. Animacji są szereg instrukcji animacji w celu wykonania przekształcenia na zawartość widoku obrót obiektu lub przykład obrazu lub rośnie rozmiar tekstu. Animacji są ograniczone do wyświetlania tylko obiektów.

-  **kolor** &ndash; pliki XML, opisujące stan listę kolorów. Aby zrozumieć Wyświetla stan kolorów, warto rozważyć element widget interfejsu użytkownika, takie jak przycisk.
   Rozsądne może mieć różne stany, takich jak naciśnięcie lub wyłączona, a przycisk może zmienić kolor przy każdej zmianie stanu. Listy jest wyrażona w postaci listy stanu.

-  **czcionki** &ndash; począwszy poziom interfejsu API 26, istnieje możliwość osadzania czcionek jako zasób w aplikacji systemu Android. Obsługa 26 biblioteki będzie czcionki Poprawka usterki systemu, które mają poziom interfejsu API 14. Osadzanie czcionek umożliwia aplikacjom

-  **mipmap** &ndash; obiektów Drawable zasoby są ogólne koncepcji grafiki, która może być skompilowany w aplikacji, a następnie dostęp do interfejsu API lub odwołuje się inne zasoby XML.
   Przykłady drawables są pliki map bitowych (PNG, GIF, jpg), specjalne map bitowych o zmiennych rozmiarach, znany jako [dziewięć poprawki](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), stan listy ogólnego kształtów zdefiniowane w XML itp.

-  **Układ** &ndash; plików XML, które opisują układ interfejsu użytkownika, takie jak działania lub wiersz w postaci listy.

-  **menu** &ndash; plików XML, które opisują menu aplikacji, takich jak *opcje menu*, *menu kontekstowe*, i *podmenu*. Na przykład menu zobacz [pokaz Menu podręcznego](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) lub [standardowych formantów](https://developer.xamarin.com/samples/mobile/StandardControls/) próbki.

-  **nieprzetworzona** &ndash; dowolne pliki, które są zapisywane w postaci raw, binary. Te pliki są kompilowane do aplikacji systemu Android w formacie binarnym.

-  **wartości** &ndash; plików XML, które zawierają wartości prostego. Plik XML w katalogu wartości nie definiuje pojedynczego zasobu, ale zamiast tego można zdefiniować wiele zasobów. Na przykład jeden plik XML może utrzymywać listę wartości ciągu, podczas gdy inny plik XML może utrzymywać listę wartości kolorów.

-  **XML** &ndash; plików XML, które przypominają w funkcji pliki konfiguracji platformy .NET. Są to dowolne XML, który może zostać odczytany przez aplikację w czasie wykonywania

-----
