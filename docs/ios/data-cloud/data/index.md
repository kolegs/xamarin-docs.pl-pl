---
title: Dostęp do danych platformy Xamarin.iOS
description: Ten dokument prowadzi do przewodników, które opisują sposób pracy z lokalnymi bazami danych w aplikacji platformy Xamarin.iOS. Połączonej zawartości w tym artykule omówiono biblioteki SQLite.NET, ADO.NET i nie tylko.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 017118a74528718ea99fe218f443b8df83d7c52e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241282"
---
# <a name="xamarinios-data-access"></a>Dostęp do danych platformy Xamarin.iOS

Xamarin.iOS obsługuje dostęp do bazy danych interfejsów API, takie jak:

-  Platforma ADO.NET.
-  3 bazy danych SQLite NET biblioteki innej firmy.

Ten przewodnik zawiera ogólne omówienie baz danych, ogólnie rzecz biorąc, przed opisujące sposób konfigurowania programu ADO.NET i biblioteki SQLite.NET dostępu do bazy danych SQLite baz danych w aplikacji platformy Xamarin.iOS. 

Większość kodu w tym dokumencie jest całkowicie dla wielu platform i zostanie uruchomiony w systemie iOS lub Android bez żadnych modyfikacji. Istnieją dwie przykładowe aplikacje omówiono:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) — proste operacjami zapisuje wyniki do tekstu są wyświetlane formantu.
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) — operacje na danych integruje się z małych działającą aplikację, która zawiera listę i edytuje prostą strukturą.

Oba rozwiązania przykładowe zawierają systemów iOS i Android przykładowe projekty aplikacji.

W przypadku aplikacji platformy Xamarin.Forms, przeczytaj [Praca z bazami danych](~/xamarin-forms/app-fundamentals/databases.md) co wyjaśnia sposób pracy z bazy danych SQLite w bibliotece PCL z zestawem narzędzi Xamarin.Forms.

## <a name="sections"></a>Sekcje

-  [Wprowadzenie](introduction.md)
-  [Konfiguracja](configuration.md)
-  [Używanie mapowania obiektowo-relacyjnego biblioteki SQLite.NET](using-sqlite-orm.md)
-  [Używanie rozwiązania ADO.NET](using-adonet.md)
-  [Używanie danych w aplikacji](using-data-in-an-app.md)

## <a name="summary"></a>Podsumowanie

W tym rozdziale omówiono dostęp do danych w rozszerzeniu Xamarin.iOS przy użyciu systemu SQLite jako aparat bazy danych. Baza danych jest możliwy "bezpośrednio" przy użyciu składni ADO.NET, lub możesz uwzględnić Relacyjnego biblioteki SQLite.NET i wykonywania operacji na danych w języku C#.

Zapoznaliśmy się z dwóch próbek: taki, który zawiera kod dostępu do danych bardzo prosty, że dane wyjściowe do pola tekstowego i prostą aplikację, która obejmuje tworzenia, odczytu, aktualizowanie i usuwanie funkcji. Omówiono również wielowątkowości i jak do generowania aplikacji za pomocą wstępnie wypełnionych bazy danych SQLite.

Dodatkowe przykłady dostęp do danych dla wielu platform, zobacz nasze [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) analizą przypadku firmy.

## <a name="related-links"></a>Linki pokrewne

- [Dostęp Basic (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp zaawansowany (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisy na danych w systemie iOS](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Dostęp do danych z zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
