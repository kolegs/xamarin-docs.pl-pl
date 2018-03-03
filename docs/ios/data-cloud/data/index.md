---
title: "iOS dostęp do danych"
description: "Większość aplikacji ma pewne wymagania w celu zapisywania danych na urządzeniu lokalnie. Jeśli nie jest trivially małe ilości danych, zwykle wymaga bazę danych i warstwy danych w aplikacji do zarządzania dostępem do bazy danych. iOS ma aparat bazy danych SQLite \"wbudowane\" i dostęp do przechowywania i pobierania danych zostało uproszczone dzięki platformie Xamarin w. Ten dokument przedstawia sposób uzyskać dostępu do bazy danych SQLite."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 9ca929f584ec2b0d98300e7645707131df89969f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="ios-data-access"></a>iOS dostęp do danych

_Większość aplikacji ma pewne wymagania w celu zapisywania danych na urządzeniu lokalnie. Jeśli nie jest trivially małe ilości danych, zwykle wymaga bazę danych i warstwy danych w aplikacji do zarządzania dostępem do bazy danych. iOS ma aparat bazy danych SQLite "wbudowane" i dostęp do przechowywania i pobierania danych zostało uproszczone dzięki platformie Xamarin w. Ten dokument przedstawia sposób uzyskać dostępu do bazy danych SQLite._

Xamarin.iOS obsługuje dostęp do bazy danych interfejsów API, takich jak:

-  ADO.NET framework.
-  SQLite NET 3rd strony biblioteki.

Ten przewodnik zawiera ogólne omówienie baz danych w zasadzie przed zawierająca opis sposobu konfigurowania ADO.NET i SQLite.NET dostępu do baz danych SQLite w aplikacji platformy Xamarin.iOS. 

Większość kodu w tym dokumencie to całkowicie platform i będzie można uruchamiać na systemu iOS lub Android bez modyfikacji. Istnieją dwie przykładowe aplikacje opisem:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) — proste danych operacji zapisuje wyniki do tekstu wyświetlanie formantu.
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) — integruje operacje na danych w małych działającą aplikację, która wyświetla i umożliwia edycję prosta struktura danych.

Oba rozwiązania próbki zawierają systemów iOS i Android przykładowych projektach aplikacji.

W przypadku aplikacji platformy Xamarin.Forms odczytu [pracy z bazami danych](~/xamarin-forms/app-fundamentals/databases.md) co wyjaśnia sposób pracy z bazy danych SQLite w bibliotece PCL z platformy Xamarin.Forms.

## <a name="sections"></a>Sekcje

-  [Wprowadzenie](introduction.md)
-  [Konfiguracja](configuration.md)
-  [Używanie mapowania obiektowo-relacyjnego biblioteki SQLite.NET](using-sqlite-orm.md)
-  [Używanie rozwiązania ADO.NET](using-adonet.md)
-  [Używanie danych w aplikacji](using-data-in-an-app.md)


## <a name="summary"></a>Podsumowanie

W tym rozdziale omówiono dostęp do danych w platformy Xamarin.iOS przy użyciu systemu SQLite jako aparat bazy danych. Bazy danych jest możliwy "bezpośrednio" przy użyciu składni ADO.NET lub można dołączyć SQLite.NET ORM i wykonywać operacje na danych w języku C#.

Firma Microsoft sprawdzone dwóch próbek: jeden zawierający kod dostępu bardzo proste danych, którego dane wyjściowe do pola tekstowego i prostą aplikację, która obejmuje tworzenia, odczytu, aktualizacji i usuwania funkcji. Omówiono także wątkowość oraz sposób inicjatora aplikacji przy użyciu wstępnie wypełnione bazy danych SQLite.

Dodatkowe przykłady dostęp do danych i platform można znaleźć naszych [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) analizę przypadku.

## <a name="related-links"></a>Linki pokrewne

- [Basic dostęp (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp Zaawansowane (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS przepisami danych](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
