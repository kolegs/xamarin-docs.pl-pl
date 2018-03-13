---
title: "Wdrażanie z fragmenty"
description: "Android 3.0 wprowadzono fragmenty. Fragmenty są samodzielnymi, modułowymi składnikami ułatwiającymi radzenie sobie ze złożonością pisania aplikacji, które mogą być uruchamiane na ekranach o różnych rozmiarach. W tym artykule przedstawiono sposób użycia fragmenty do tworzenia aplikacji platformy Xamarin.Android i jak obsługiwać fragmenty na urządzeniach wstępnie Android 3.0."
ms.topic: article
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2ed67eac51f6edcfda16caf73e4667c49124082c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="implementing-with-fragments"></a>Wdrażanie z fragmenty

_Android 3.0 wprowadzono fragmenty. Fragmenty są samodzielnymi, modułowymi składnikami ułatwiającymi radzenie sobie ze złożonością pisania aplikacji, które mogą być uruchamiane na ekranach o różnych rozmiarach. W tym artykule przedstawiono sposób użycia fragmenty do tworzenia aplikacji platformy Xamarin.Android i jak obsługiwać fragmenty na urządzeniach wstępnie Android 3.0._


## <a name="overview"></a>Omówienie

W tej sekcji zostanie omówiony sposób tworzenia aplikacji, która wyświetli listę Szekspir w pełni i oferty z każdego wybranego play. Aplikacji będzie korzystał fragmenty, dzięki czemu możemy zdefiniować naszych składniki interfejsu użytkownika w jednym miejscu, ale następnie używać ich w różnych rozmiarach. Na przykład poniższe zrzuty ekranu przedstawiają aplikacja była uruchomiona na komputerze typu tablet 10", a także na telefonie:

[![Zrzuty ekranu aplikacji przykład systemem tabletu lub telefonu](images/intro-screenshot-sml.png)](images/intro-screenshot.png#lightbox)

W tej sekcji omówiono następujące tematy:

- **Tworzenie fragmenty** &ndash; przedstawiono sposób tworzenia fragmentu, aby wyświetlić listę Szekspir w pełni i inny fragment do wyświetlenia oferty z każdym play.

- **Obsługa różnych rozmiarów ekranu** &ndash; przedstawia sposób układ aplikacji, aby móc korzystać z większych rozmiarów ekranu.

- **Przy użyciu pakietu obsługi systemu Android** &ndash; implementuje pakietu obsługi systemu Android, a następnie sprawia, że niektóre drobne zmiany z działaniami w aplikacji, co pozwala na uruchamianie jej w starszych wersjach systemu android.


## <a name="requirements"></a>Wymagania

W tym przewodniku wymaga platformy Xamarin.Android 4.0 lub nowszy. Będzie też niezbędne do zainstalowania pakietu obsługi systemu Android, jak opisano w dokumentacji fragmenty.


## <a name="introduction"></a>Wprowadzenie

W przykładzie oprzemy będzie w tej sekcji działania nie zawierają logikę ładowania listy, odpowiada na wybór użytkownika lub wyświetlanie oferty dla wybranego play. Istnieje tę logikę w poszczególnych fragmentów.
Umieszczając tę logikę w się fragmentów, możemy podzielić przepływu pracy aplikacji do obsługi dużych ekranów z jednego działania lub małe ekrany z wielu działań bez konieczności pisania logiki różne dla każdego działania. Na komputerze typu tablet obu fragmentach będzie mieć jedno działanie. Na telefonie fragmentów będą obsługiwane w różnych działań.

Ta aplikacja zawiera następujące elementy:

 **MainActivity** — przedstawia jedną lub obie z fragmentów, w zależności od wielkości ekranu. Jest to uruchomienia działania.

 **TitlesFragment** — zostanie wyświetlona lista odtwarzania Szekspir firmy, z których użytkownik może wybrać.

 **DetailsFragment** — Wyświetla oferty z wybranych play.

 **DetailsActivity** — obsługuje i wyświetla DetailsFragment.
To działanie jest używane przez urządzenia z ekranami małych, takich jak telefony.



## <a name="related-links"></a>Linki pokrewne

- [FragmentsWalkthrough (przykład)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Omówienie projektanta](~/android/user-interface/android-designer/index.md)
- [Przykłady Xamarin.Android: pustakowym galerii](https://developer.xamarin.com/samples/HoneycombGallery/)
- [Implementowanie fragmentów](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Pakiet pomocy technicznej](http://developer.android.com/sdk/compatibility-library.html)
