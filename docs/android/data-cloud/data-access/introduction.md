---
title: Wprowadzenie
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/28/2017
ms.openlocfilehash: a2d92b6f7bfa1e2a40e30d0b832dd8a8f737cee2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction"></a>Wprowadzenie

## <a name="when-to-use-a-database"></a>Kiedy należy używać bazy danych

Gdy coraz więcej możliwości przechowywania i przetwarzania urządzeń przenośnych, telefony i tablety nadal opóźnione w stosunku do ich odpowiedniki stacjonarnych i przenośnych. Z tego powodu warto biorąc trochę czasu na planowanie Architektura magazynu danych dla aplikacji, a nie tylko przy założeniu, że baza danych jest prawidłowa odpowiedź cały czas. Istnieje szereg różne opcje, które wskazują inne wymagania, takich jak:

-  **Preferencje** — Android zapewnia wbudowany mechanizm przechowywania proste pary klucz wartość danych. Jeśli przechowujesz ustawienia użytkownika prostego lub małych fragmentów danych (na przykład informacje o personalizacji), użyj funkcji natywnego platformy do przechowywania tego typu danych.
-  **Pliki tekstowe** — dane wejściowe użytkownika lub pamięci podręcznych obiektu pobranej zawartości (np.) HTML) mogą być przechowywane bezpośrednio w systemie plików. Użyj odpowiednie konwencji nazewnictwa plików, aby ułatwić organizowanie używanych plików i wyszukiwanie danych.
-  **Pliki danych serializacji** — obiekty mógł być trwały jako XML lub JSON w systemie plików. .NET framework obejmuje biblioteki, w których serializacji i do serializacji obiektów łatwe. Użyj odpowiedniej nazwy do organizowania danych plików.
-  **Baza danych** — aparat bazy danych SQLite jest dostępna na platformie Android i jest przydatne do przechowywania strukturalnych danych potrzebne do zapytań, sortowania lub w przeciwnym razie manipulowania. Magazyn baz danych jest dostosowane do listy danych z wielu właściwości.
-  **Pliki obrazów** — mimo że jest możliwe do przechowywania danych binarnych w bazie danych na urządzeniu przenośnym, zalecane jest zapisanie ich bezpośrednio w systemie plików. W razie potrzeby można przechowywać nazwy plików w bazie danych do skojarzenia z innymi danymi obrazu. Podczas pracy nad duże obrazy lub wiele obrazów, jest dobrym rozwiązaniem Planowanie strategii buforowania, która usuwa pliki nie są już potrzebne w celu uniknięcia zajmując miejsce przechowywania wszystkich użytkowników.

Jeśli baza danych jest mechanizm przechowywania dla aplikacji, w pozostałej części tego dokumentu omówiono sposób korzystania z bazy danych SQLite na platformie Xamarin.

## <a name="advantages-of-using-a-database"></a>Zalety korzystania z bazy danych

Istnieje wiele zalet korzystania z bazy danych SQL w aplikacji mobilnej:

-  Bazy danych SQL umożliwia efektywne przechowywanie danych strukturalnych.
-  Określone dane można wyodrębnić z złożonych zapytań.
-  Można sortować wyniki zapytania.
-  Może być agregowany wyników zapytania.
-  Deweloperom posiadane umiejętności bazy danych mogą wykorzystywać wiedzę do projektowania kod dostępu i bazy danych.
-  Model danych ze składnika serwera aplikacji połączonych ponownie można (w całości lub częściowo) w aplikacji mobilnej.


## <a name="sqlite-database-engine"></a>Aparat bazy danych SQLite

SQLite jest aparat bazy danych open source, która została przyjęta przez firmę Google dla ich platform przenośnych. Aparat bazy danych SQLite jest wbudowana w obu systemów operacyjnych, więc nie ma żadnych dodatkowych działań dla deweloperów móc korzystać z niego. SQLite jest dobrze nadają się do aplikacji mobilnych dla wielu platform, ponieważ:

-  Aparat bazy danych jest mały, szybkie i łatwe przenośnej.
-  Baza danych jest przechowywana w jednym pliku, który ułatwia zarządzanie nimi na urządzeniach przenośnych.
-  Format pliku jest łatwy w użyciu na platformach: czy 32 - lub 64-bitowych i systemy big - lub little-endian.
-  Implementuje większość standardowych SQL92.


Ponieważ SQLite jest bardzo mała i szybkie, istnieją pewne ostrzeżenia przy jego użyciu:

-  Niektóre składni sprzężenia zewnętrzne nie jest obsługiwane.
-  Tylko tabela zmiany nazwy i ADDCOLUMN są obsługiwane. Nie można wykonać innych modyfikacji schematu.
-  Widoki są tylko do odczytu.


Użytkownik może dowiedzieć się więcej o SQLite w witrynie sieci Web - [SQLite.org](http://SQLite.org) — jednak wszystkie informacje, należy użyć SQLite za pomocą platformy Xamarin znajduje się w tym dokumencie, a skojarzone próbek. Aparat bazy danych SQLite obsługiwanego w systemie Android od 2 dla systemu Android.
Mimo że nie omówione w tym rozdziale, SQLite jest również dostępna do użycia w Windows Phone i aplikacji systemu Windows.

## <a name="windows-and-windows-phone"></a>System Windows i Windows Phone

SQLite można także na platformach systemu Windows, mimo że w tym dokumencie nie są objęte tymi platformami.
Dowiedz się więcej w [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) i [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) przypadku badania i przejrzyj [blogu Timowi Heuer](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).


## <a name="related-links"></a>Linki pokrewne

- [Basic dostęp (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp Zaawansowane (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisami danych w systemie android](https://developer.xamarin.com/recipes/android/data/)
- [Dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
