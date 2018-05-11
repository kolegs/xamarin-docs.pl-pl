---
title: Tworzenie Cross Platform aplikacji
description: Tej sekcji omówiono w podsumowanie, a także sześć części, jak tworzyć aplikacje przy użyciu platformy programistycznej Xamarin — od zrozumienia, jak działa program Xamarin projektowanie aplikacji mobilnych, testowanie i wdrażanie różnych sklepów z aplikacjami.
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: fba13ab921949cd2361e78535d5ffc96952a1336
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="sharing-code-options"></a>Opcje udostępniania kodu

Dostępne są dwie opcje udostępnianie kodu aplikacji dla wielu platform urządzeń przenośnych: projekty zasobów udostępnionych i przenośnej biblioteki klas. Te opcje są [omówione w tym miejscu](~/cross-platform/app-fundamentals/code-sharing.md); więcej informacji na temat [przenośnej biblioteki klas](~/cross-platform/app-fundamentals/pcl.md) i [udostępnionych projektów](~/cross-platform/app-fundamentals/shared-projects.md) jest również dostępna.

<a name="Sections" />

## <a name="building-cross-platform-mobile-apps"></a>Tworzenie Cross Platform Mobile Apps

 [Omówienie](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [Część 1 — Opis platformy Xamarin Mobile](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [Część 2 — architektura](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [Część 3 — konfigurowanie rozwiązania platformy Xamarin między](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [Część 4 — zajmujących się wielu platform](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [Część 5 — praktyczne kodu strategii udostępniania](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [Część 6. Testowanie i zgody sklepu App Store](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />


## <a name="case-studies"></a>Analizy przypadków:

Zasady opisane w niniejszym dokumencie są umieszczane w praktyce w przykładowej aplikacji *Tasky*, jak również [wbudowana w aplikacjach](https://xamarin.com/prebuilt) jak [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm).

 <a name="Tasky" />


### <a name="tasky"></a>Tasky

Tasky jest aplikacją listy prostych zadań do wykonania dla systemu iOS, Android i Windows Phone.
On wprowadzającym zostały omówione podstawy tworzenie wieloplatformowych aplikacji za pomocą platformy Xamarin i używa lokalnej bazy danych SQLite.

 [![Lista tasky](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![tasky listy](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

Odczyt [Tasky analizę przypadku](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md).


## <a name="summary"></a>Podsumowanie

Ta sekcja zawiera wprowadzenie narzędzi do tworzenia aplikacji platformy Xamarin w oraz omówiono sposób tworzenia aplikacji przeznaczonych dla wielu platform urządzeń przenośnych.

Obejmuje warstwowa architektura struktury kodu do ponownego użycia na wielu platformach i opisuje wzorce innego oprogramowania, które mogą być używane w danej architekturze.

Przykłady znajdują się typowe funkcje aplikacji (np. plików i sieci operacji) i jak mogą być wbudowane w sposób między platformami.

Na koniec on krótko opisano testowanie i zawiera odwołania do analizę przypadku, która powoduje umieszczenie tych zasad w akcji.



## <a name="related-links"></a>Linki pokrewne

- [Opcje udostępniania kodu](~/cross-platform/app-fundamentals/code-sharing.md)
- [Analiza przypadku: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky przykładowej aplikacji (github)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Tworzenie aplikacji platformy Xamarin Mobile: Wieloplatformowych C# i podstawowe informacje na temat platformy Xamarin.Forms](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/))
- [Mobile Development w języku C# przez Gregowi kajdany (O'Reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [Professional i Platform Mobile Development w języku C# Scott Olson, Jan myśliwego, Ben Horgen, Goers Kenny (Wrox)](http://www.wiley.com/WileyCDA/WileyTitle/productCd-1118157702.html)
