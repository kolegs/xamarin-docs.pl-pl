---
title: Fragmenty
description: Android 3.0 wprowadzono fragmenty przedstawiająca sposób obsługi bardziej elastyczne projektów dla wielu różnych rozmiarów ekranu na telefony i tablety. W tym artykule opisano, jak używać do tworzenia aplikacji platformy Xamarin.Android fragmenty, a także obsługuje fragmenty na urządzeniach wstępnie Android 3.0 (11 poziom interfejsu API).
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 08c2edb3acc15518c7d5a69f227fb9ef819887be
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767017"
---
# <a name="fragments"></a>Fragmenty

_Android 3.0 wprowadzono fragmenty przedstawiająca sposób obsługi bardziej elastyczne projektów dla wielu różnych rozmiarów ekranu na telefony i tablety. W tym artykule opisano, jak używać do tworzenia aplikacji platformy Xamarin.Android fragmenty, a także obsługuje fragmenty na urządzeniach wstępnie Android 3.0 (11 poziom interfejsu API)._

## <a name="fragments-overview"></a>Fragmenty — omówienie

Większe rozmiary ekranu na większości tabletów dodać dodatkową złożoność do rozwoju systemu Android — układ przeznaczony dla małego ekranu nie zawsze działa także dla większych ekranów i na odwrót. Aby zmniejszyć liczbę komplikacji, które to wprowadzone, Android 3.0 spowodował dodanie dwóch nowych funkcji *fragmenty* i *obsługi pakietów*.

Fragmenty można traktować jako moduły interfejsu użytkownika. Umożliwiają one developer dzielenie na części izolowanym, wielokrotnego użytku, które mogą być uruchamiane w oddzielnych działania interfejsu użytkownika. W czasie wykonywania działania, same podejmie decyzję, które fragmenty do użycia.

Obsługa pakietów pierwotnie były nazywane *biblioteki zgodności* i dozwolone fragmenty do użycia na urządzeniach, na których działają wersje systemu android przed 3.0 dla systemu Android (11 poziom interfejsu API).

Na przykład na ilustracji poniżej pokazano, jak pojedynczą aplikacją używa fragmentów w różnych formach urządzenia.

[![Diagram przedstawiający sposób fragmenty są używane w tabletów i aparaty telefoniczne](images/00.png)](images/00.png#lightbox)

*Fragment A* zawiera listę, podczas gdy *B fragmentu* zawiera szczegóły dotyczące elementu zaznaczonego na tej liście. Gdy aplikacja jest uruchamiana na komputerze typu tablet, będzie możliwe wyświetlenie obu fragmentach na tego samego działania. Gdy ta sama aplikacja jest uruchamiana na słuchawki (z jego mniejszego rozmiaru ekranu), fragmentów znajdują się w dwóch oddzielnych działalności. Fragment A i B fragmentu są takie same, w obu formach, ale różnią się działań, które je obsługują.

Aby pomóc działanie koordynuje i zarządzanie nimi te fragmenty, Android wprowadziła nową klasę o nazwie *FragmentManager*. Każde działanie ma własne wystąpienie `FragmentManager` dodawania, usuwania i znajdowania hostowanej fragmenty. Na poniższym diagramie przedstawiono związek między fragmentów i działań:

[![Diagram pokazujący relacje między działania Menedżera fragmentu i fragmenty](images/01.png)](images/01.png#lightbox)

Niektóre względem fragmenty można traktować jako formanty złożone lub mini działań. One zawierać elementy interfejsu użytkownika w modułach wielokrotnego użytku, które mogą być następnie używane niezależnie przez deweloperów w działaniach. Fragment ma hierarchię widoku — podobnie jak działanie — jednak w przeciwieństwie do działania może być współużytkowana przez ekranów. Widoki różnią się z fragmentów, że fragmenty ma swoje własne cyklu życia; nie widoków.

W trakcie działania hosta do co najmniej jednego fragmentu nie jest bezpośrednio pamiętać się fragmentów. Podobnie fragmenty nie potrafią bezpośrednio z pozostałych fragmentów w działaniu hostingu. Jednak fragmentów i działania potrafią `FragmentManager` w ich działania. Za pomocą `FragmentManager`, istnieje możliwość działanie lub fragmentu otrzymać odwołanie do określonego wystąpienia fragmentu, a następnie wywołać metody w tym wystąpieniu. W ten sposób działania lub fragmenty mogą komunikować się i interakcję z pozostałych fragmentów.

Ten przewodnik zawiera kompleksowym o sposobie używania fragmentów, w tym:

-   **Tworzenie fragmenty** — Tworzenie podstawowych Fragment i klucza metod, które muszą zostać zaimplementowane.
-   **Fragmentu zarządzania i transakcji** — sposób operowania fragmentów w czasie wykonywania.
-   **Android pakietu obsługi** — jak przy użyciu bibliotek, które umożliwiają fragmenty ma być używany dla starszych wersji systemu android.


## <a name="requirements"></a>Wymagania

Fragmenty są dostępne w zestawie SDK systemu Android, począwszy od poziom interfejsu API 11 (3.0 dla systemu Android), jak pokazano na poniższym zrzucie ekranu:

[![Wybieranie poziom interfejsu API w Menedżerze zestawu SDK systemu Android](images/02.png)](images/02.png#lightbox)

Fragmenty są dostępne w Xamarin.Android 4.0 lub nowszy. Aplikacji platformy Xamarin.Android musi wskazywać na co najmniej poziom interfejsu API 11 (Android 3.0) lub nowszej, aby można było używać fragmenty. Platforma docelowa może być ustawiona w projekcie właściwości, jak pokazano poniżej:

[![Ustawianie docelowy Framework poziom interfejsu API w opcje projektu](images/03-sml.png)](images/03.png#lightbox)

Istnieje możliwość użycia fragmentów w starszych wersjach systemu Android przy użyciu pakietu obsługi systemu Android i platformy Xamarin.Android 4.2 lub nowszej. Jak to zrobić zostało opisane bardziej szczegółowo w dokumentach w tej sekcji.


## <a name="related-links"></a>Linki pokrewne

- [Galeria pustakowym (przykład)](https://developer.xamarin.com/samples/monodroid/HoneycombGallery)
- [Fragmenty](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Pakiet pomocy technicznej](http://developer.android.com/sdk/compatibility-library.html)
- [MOTODEV Webinar: Fragmenty wprowadzenie](http://motodev.adobeconnect.com/p9h1aqk3ttn/)
