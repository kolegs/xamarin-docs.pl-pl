---
title: Xamarin.Android Data Access
description: Większość aplikacji ma pewne wymagania w celu zapisywania danych na urządzeniu lokalnie. Jeśli nie jest trivially małe ilości danych, zwykle wymaga bazę danych i warstwy danych w aplikacji do zarządzania dostępem do bazy danych.  Android zawiera aparat bazy danych SQLite "wbudowane" i dostęp do przechowywania i pobierania danych zostało uproszczone dzięki platformie Xamarin w. Ten dokument przedstawia sposób dostępu do bazy danych SQLite w sposób między platformami.
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 508a8f54723bfdd30b1c8ea8d4b6c1d422ae24e9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763253"
---
# <a name="xamarinandroid-data-access"></a>Xamarin.Android Data Access

_Większość aplikacji ma pewne wymagania w celu zapisywania danych na urządzeniu lokalnie. Jeśli nie jest trivially małe ilości danych, zwykle wymaga bazę danych i warstwy danych w aplikacji do zarządzania dostępem do bazy danych.  Android zawiera aparat bazy danych SQLite "wbudowane" i dostęp do przechowywania i pobierania danych zostało uproszczone dzięki platformie Xamarin w. Ten dokument przedstawia sposób dostępu do bazy danych SQLite w sposób między platformami._

## <a name="data-access-overview"></a>Omówienie dostępu do danych

Większość aplikacji ma pewne wymagania w celu zapisywania danych na urządzeniu lokalnie. Jeśli nie jest trivially małe ilości danych, zwykle wymaga bazę danych i warstwy danych w aplikacji do zarządzania dostępem do bazy danych. Android zarówno zawiera aparat bazy danych Sqlite "wbudowane" i dostęp do danych zostało uproszczone dzięki platformy Xamarin w, który jest dostarczany z dostawcy danych SQLite.

Xamarin.Android obsługuje dostęp do bazy danych interfejsów API takich jak:

-  ADO.NET framework.
-  SQLite NET 3rd strony biblioteki.

Większość kodu w tej sekcji to całkowicie platform i będzie można uruchamiać na systemu iOS lub Android bez modyfikacji. Istnieją dwie przykładowe aplikacje opisem:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; danych proste operacje zapisuje wyniki do tekstu wyświetlanie formantu.

-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; integruje operacje na danych w małych działającą aplikację, która wyświetla i umożliwia edycję prosta struktura danych.

Oba rozwiązania próbki zawierają systemów iOS i Android przykładowych projektach aplikacji.

W przypadku aplikacji platformy Xamarin.Forms odczytu [pracy z bazami danych](~/xamarin-forms/app-fundamentals/databases.md) co wyjaśnia sposób pracy z bazy danych SQLite w bibliotece PCL z platformy Xamarin.Forms.

Tematy w tej sekcji omówiono w nim dostępu do danych na platformie Xamarin.Android przy użyciu systemu SQLite jako aparat bazy danych. "" Za pomocą składni ADO.NET można można uzyskać dostępu do bazy danych lub można dołączyć SQLite.NET ORM i wykonywać operacje na danych w języku C#.

Dwa przykłady zostaną zweryfikowane: jeden zawierający kod dostępu bardzo proste danych, którego dane wyjściowe do pola tekstowego i prostą aplikację, która obejmuje tworzenia, odczytu, aktualizacji i usuwania funkcji. Również omówiono wątki i jak inicjatora aplikacji przy użyciu wstępnie wypełnione bazy danych SQLite.

Dodatkowe przykłady dostęp do danych i platform można znaleźć naszych [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) analizę przypadku.


## <a name="related-links"></a>Linki pokrewne

- [Basic dostęp (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp Zaawansowane (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisami danych w systemie android](https://developer.xamarin.com/recipes/android/data/)
- [Dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
