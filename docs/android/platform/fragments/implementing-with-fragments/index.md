---
title: Implementowanie fragmenty — wskazówki
description: W tym artykule przedstawiono sposób użycia fragmenty do tworzenia aplikacji platformy Xamarin.Android.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 92c68298d7abd2570efd89e12d7cfb6364e90972
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798359"
---
# <a name="implementing-fragments---walkthrough"></a>Implementowanie fragmenty — wskazówki

_Fragmenty są niezależne, modularna składników, które mogą pomóc w pokonywaniu złożoność aplikacji systemu Android, które odnoszą się do urządzenia z różnych rozmiarów ekranu. W tym artykule przedstawiono sposób tworzenia i używania fragmenty, podczas tworzenia aplikacji platformy Xamarin.Android._

## <a name="overview"></a>Omówienie

W tej sekcji będzie przeprowadzenie sposobu tworzenia i używania fragmentów w aplikacji platformy Xamarin.Android. Ta aplikacja wyświetli tytuły kilka pełni przez Szekspir łączy na liście. Po naciśnięciu tytuł odtwarzania, w aplikacji zostaną wyświetlone oferty z tym play w oddzielnych działania:

[![Aplikacji uruchomionej na telefonie z systemem Android w trybie portret](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Gdy telefon jest obracana na poziomą tryb, zmieni się wygląd aplikacji: listy odtwarzania i ofert pojawi się w tym samym działaniu. Po wybraniu Odtwórz oferty będą wyświetlane w tej samej działania:

[![Aplikacji uruchomionej na telefonie z systemem Android w trybie krajobraz](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

Na koniec Jeśli aplikacja jest uruchomiona na komputerze typu tablet:

[![Aplikacji uruchomionej na tablet z systemem Android](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

Ta przykładowa aplikacja mogą łatwo dostosować do różnych rozmiarach i orientacji przy minimalnych zmianach w kodzie za pomocą fragmentów i [alternatywne układy](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources).

Dane aplikacji będą istnieć w dwóch tablic ciągów, które są zapisane na stałe w aplikacji jako ciąg tablice C#. Każdy tablic będzie służyć jako źródło danych dla jednego fragmentu.  Jedna tablica będą przechowywane nazwy niektórych pełni przez Szekspir i innych tablicy będą przechowywane w sklepie play tej oferty. Podczas uruchamiania aplikacji, będą wyświetlane nazwy play `ListFragment`. Gdy użytkownik kliknie Odtwarzaj na `ListFragment`, aplikacja będzie uruchamiać innego działania, która będzie wyświetlana oferty.

Interfejs użytkownika dla aplikacji będzie składają się z dwóch układów, jedno dla orientacji pionowej i w trybie krajobraz. W czasie wykonywania Android będzie ustalić, jakie układu, aby załadować oparta na orientacji urządzenia i zapewnia taki układ do działania do renderowania. Wszystkie logikę odpowiada na żądania użytkownik klika przycisk i wyświetlanie danych będzie znajdować się w fragmenty. Działania w aplikacji istnieje tylko jako kontenerów, które będą obsługiwać fragmentów.

W tym przewodniku będzie można podzielić na dwa przewodniki. [Najpierw część](./walkthrough.md) koncentruje się na podstawowych części aplikacji. Pojedynczy zestaw układów (zoptymalizowane pod kątem trybie portret) zostanie utworzona oraz dwa fragmenty i dwa działania:

1. `MainActivity` &nbsp; Jest to uruchomienia działania aplikacji.
1. `TitlesFragment` &nbsp; Ten fragment wyświetli listę tytułów odtwarzania, które zostały napisane przez Szekspir łączy. Będzie obsługiwana przez `MainActivity`.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` rozpocznie się `PlayQuoteActivity` w odpowiedzi na użytkownika, wybierając Odtwarzaj na `TitlesFragment`.
1. `PlayQuoteFragment` &nbsp; Ten fragment wyświetli ofertę z Odtwórz przez Szekspir łączy. Będzie obsługiwana przez `PlayQuoteActivity`.

[Drugiej części tego przewodnika](./walkthrough-landscape.md) przedstawimy Dodawanie alternatywnego układu (zoptymalizowane pod kątem tryb poziomo) wyświetlanych obu fragmentach na ekranie. Ponadto niektóre zmiany kodu drobne będą kod tak, aby aplikacja będzie dostosować zachowanie, aby liczba fragmentów, które jednocześnie są wyświetlane na ekranie.

## <a name="related-links"></a>Linki pokrewne

- [FragmentsWalkthrough (przykład)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Omówienie projektanta](~/android/user-interface/android-designer/index.md)
- [Implementowanie fragmentów](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Pakiet pomocy technicznej](http://developer.android.com/sdk/compatibility-library.html)
