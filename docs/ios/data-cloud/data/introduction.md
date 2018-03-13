---
title: Wprowadzenie
ms.topic: article
ms.prod: xamarin
ms.assetid: B1994468-FD06-4FD9-96B3-FCEBB13A972A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: bd3f02ab4fd5b6e96db8bd986e5a698ce30a6b74
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="introduction"></a>Wprowadzenie

## <a name="when-to-use-a-database"></a>Kiedy należy używać bazy danych

Gdy coraz więcej możliwości przechowywania i przetwarzania urządzeń przenośnych, telefony i tablety nadal opóźniona pulpitu &amp; odpowiednikami komputera przenośnego. Z tego powodu warto biorąc trochę czasu na planowanie Architektura magazynu danych dla aplikacji, a nie tylko przy założeniu, że baza danych jest prawidłowa odpowiedź cały czas. Istnieje szereg różne opcje, które wskazują inne wymagania, takich jak:

-  **Preferencje** — systemu iOS zapewnia wbudowany mechanizm przechowywania proste pary klucz wartość danych. Jeśli przechowujesz ustawienia użytkownika prostego lub małych fragmentów danych (na przykład informacje o personalizacji), użyj funkcji natywnego platformy do przechowywania tego typu danych. Dla systemu iOS mogą również czerpać korzyści iCloud synchronizacji dla tych danych, zarówno dla kopii zapasowej i synchronizacji dla użytkowników z wielu urządzeń.
-  **Pliki tekstowe** — dane wejściowe użytkownika lub pamięci podręcznych obiektu pobranej zawartości (np.) HTML) mogą być przechowywane bezpośrednio w systemie plików. Użyj odpowiednie konwencji nazewnictwa plików, aby ułatwić organizowanie używanych plików i wyszukiwanie danych.
-  **Pliki danych serializacji** — obiekty mógł być trwały jako XML lub JSON w systemie plików. .NET framework obejmuje biblioteki, w których serializacji i do serializacji obiektów łatwe. Użyj odpowiedniej nazwy do organizowania danych plików.
-  **Baza danych** — aparat bazy danych SQLite jest dostępne dla systemu iOS i przydaje się do przechowywania danych strukturalnych potrzebne do zapytań, sortowania lub w przeciwnym razie manipulowania. Magazyn baz danych jest dostosowane do listy danych z wielu właściwości.
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

SQLite jest aparat bazy danych open source, która została przyjęta przez firmę Apple do ich platform przenośnych. Aparat bazy danych SQLite są wbudowane w systemach iOS, więc nie ma żadnych dodatkowych działań dla deweloperów móc korzystać z niego. SQLite jest dobrze nadają się do aplikacji mobilnych dla wielu platform, ponieważ:

-  Aparat bazy danych jest mały, szybkie i łatwe przenośnej.
-  Baza danych jest przechowywana w jednym pliku, który ułatwia zarządzanie nimi na urządzeniach przenośnych.
-  Format pliku jest łatwy w użyciu na platformach: czy 32 - lub 64-bitowych i systemy big - lub little-endian.
-  Implementuje większość standardowych SQL92.


Ponieważ SQLite jest bardzo mała i szybkie, istnieją pewne ostrzeżenia przy jego użyciu:

-  Niektóre składni sprzężenia zewnętrzne nie jest obsługiwane.
-  Tylko tabela zmiany nazwy i ADDCOLUMN są obsługiwane. Nie można wykonać innych modyfikacji schematu.
-  Widoki są tylko do odczytu.


Użytkownik może dowiedzieć się więcej o SQLite w witrynie sieci Web - [SQLite.org](http://SQLite.org) — jednak wszystkie informacje, należy użyć SQLite za pomocą platformy Xamarin znajduje się w tym dokumencie, a skojarzone próbek. Aparat bazy danych SQLite jest wbudowana wszystkie wersje systemu IOS.
Mimo że nie omówione w tym rozdziale, SQLite jest również dostępna do użycia w Windows Phone i aplikacji systemu Windows.

## <a name="windows-and-windows-phone"></a>System Windows i Windows Phone

SQLite można także na platformach systemu Windows, mimo że w tym dokumencie nie są objęte tymi platformami.
Dowiedz się więcej w [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) i [Tasky Pro](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/building_cross_platform_applications/case_study%3A_tasky) przypadku badania i przejrzyj [blogu Timowi Heuer](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).



## <a name="related-links"></a>Linki pokrewne

- [Basic dostęp (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp Zaawansowane (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS przepisami danych](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
