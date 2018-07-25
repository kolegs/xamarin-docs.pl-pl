---
title: Wprowadzenie do magazynu danych w systemie Android
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/28/2017
ms.openlocfilehash: 26576fe31919822237022572a4e490cf6fc19d65
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241447"
---
# <a name="introduction"></a>Wprowadzenie

## <a name="when-to-use-a-database"></a>Kiedy należy używać bazy danych

Gdy coraz więcej możliwości przechowywania i przetwarzania urządzeń przenośnych, telefony komórkowe i tablety nadal opóźnione w stosunku do ich odpowiedników stacjonarnych i przenośnych. Z tego powodu, dla których warto powoli do zaplanowania Architektura magazynu danych dla aplikacji, a nie tylko przy założeniu, że baza danych jest prawidłowej odpowiedzi przez cały czas. Istnieje szereg różnych opcji, które odpowiadają różne wymagania, takie jak:

-  **Preferencje** — system Android oferuje wbudowany mechanizm do przechowywania proste pary klucz wartość danych. Jeśli przechowujesz prostych ustawień ani małych fragmentów danych (na przykład informacje o personalizacji), użyj natywne funkcje platformy do przechowywania informacje tego typu.
-  **Pliki tekstowe** — dane wejściowe użytkownika lub pamięci podręcznych obiektu pobierania zawartości (np.) HTML) mogą być przechowywane bezpośrednio w systemie plików. Użyj odpowiednich konwencji nazewnictwa plików, aby ułatwić organizowanie plików i wyszukiwania danych.
-  **Pliki danych serializacji** — obiekty mogą zostać utrwalone w formacie XML lub JSON w systemie plików. .NET framework zawiera biblioteki, które serializacji i deserializacji obiektów łatwo. Użyj nazw odpowiednie do organizowania plików danych.
-  **Baza danych** — aparat bazy danych SQLite jest dostępna na platformie systemu Android, i jest przydatne do przechowywania strukturalnych danych potrzebnych do zapytania, sortować lub inny sposób modyfikować. Magazyn bazy danych jest dostosowane do listy danych z wieloma właściwościami.
-  **Pliki obrazów** — mimo że jest możliwość przechowywania danych binarnych w bazie danych na urządzeniu przenośnym, zaleca się przechowywać je bezpośrednio w systemie plików. W razie potrzeby nazwy plików można przechowywać w bazie danych, aby skojarzyć go z innymi danymi. Podczas pracy z dużych obrazów lub wiele obrazów, jest dobrym rozwiązaniem Planowanie strategii buforowania, który służy do usuwania plików, które nie są już potrzebne, aby zapobiec użyciu miejsca do magazynowania wszystkich użytkowników.

Jeśli baza danych jest mechanizm magazynu odpowiednie dla twojej aplikacji, pozostałej części tego dokumentu w tym artykule omówiono sposób korzystania z bazy danych SQLite na platformie Xamarin.

## <a name="advantages-of-using-a-database"></a>Zalety korzystania z bazy danych

Istnieje kilka zalet korzystania z bazy danych SQL w aplikacji mobilnej:

-  Bazy danych SQL umożliwia efektywne przechowywanie danych strukturalnych.
-  Można wyodrębnić szczegółowe dane przy użyciu złożonych zapytań.
-  Można sortować wyniki zapytania.
-  Wyniki zapytania można agregować.
-  Deweloperzy dzięki posiadanym umiejętnościom bazy danych mogą wykorzystywać swojej wiedzy na temat projektowania kodu dostępu do bazy danych i danych.
-  Modelu danych, z poziomu składnika serwera, połączonych aplikacji mogą być ponownie używane (w całości lub części) w aplikacji mobilnej.


## <a name="sqlite-database-engine"></a>Aparat bazy danych SQLite

Bazy danych SQLite jest aparat bazy danych typu open source, która została przyjęta przez firmę Google dla ich platformy urządzeń mobilnych. Aparat bazy danych SQLite jest wbudowana w obu systemach operacyjnych, więc nie ma żadnych dodatkową pracą dla deweloperów z niej korzystać. Bazy danych SQLite jest dobrze nadaje się do opracowywania aplikacji mobilnych dla wielu platform, ponieważ:

-  Aparat bazy danych jest małe, szybko i łatwo przenośny.
-  Bazy danych są przechowywane w pojedynczym pliku, który jest łatwa w zarządzaniu na urządzeniach przenośnych.
-  Format pliku jest łatwa w użyciu na wielu platformach: czy 32 - lub 64-bitowy i systemów big - lub -endian.
-  Implementuje większość standardu SQL92 standardowych.


Ponieważ bazy danych SQLite jest bardzo mały i szybkie, istnieją niektóre zastrzeżenia dotyczące jej użycie:

-  Niektóre przypadki składni sprzężenia zewnętrzne nie jest obsługiwana.
-  Tylko tabeli zmiany nazwy i ADDCOLUMN są obsługiwane. Nie można wykonać innych modyfikacji schematu.
-  Widoki są tylko do odczytu.


Znajdziesz więcej informacji na temat oprogramowania SQLite w witrynie sieci Web - [SQLite.org](http://SQLite.org) — jednak wszystkie informacje, należy użyć bazy danych SQLite za pomocą platformy Xamarin jest zawarty w tym dokumencie, a skojarzone przykładów. Aparat bazy danych SQLite jest obsługiwane począwszy od 2 dla systemu Android w systemie Android.
Mimo że nieuwzględnione w tym rozdziale, bazy danych SQLite jest również dostępny do użycia w aplikacji Windows i Windows Phone.

## <a name="windows-and-windows-phone"></a>Windows i Windows Phone

Bazy danych SQLite można także na platformach Windows, mimo że w tym dokumencie nie omówiono tych platform.
Dowiedz się więcej w [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) i [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) zamierzone, Zapisz badania, a także Przejrzyj [blog Timem Heuerem](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).


## <a name="related-links"></a>Linki pokrewne

- [Dostęp Basic (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp zaawansowany (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisy na danych w systemie android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Dostęp do danych z zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
