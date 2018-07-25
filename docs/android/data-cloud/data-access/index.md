---
title: Xamarin.Android Data Access
description: Większość aplikacji ma pewne wymagania w celu zapisywania danych na urządzeniu lokalnie. Chyba że przypadku małych ilości danych zwykle wymaga to bazę danych i warstwy danych w aplikacji, aby zarządzać dostępem do bazy danych.  Systemu android zawiera aparat bazy danych SQLite "wbudowane", a dostęp do przechowywania i pobierania danych jest uproszczone przez platformy Xamarin. W tym dokumencie pokazano, jak korzystać z bazy danych SQLite w sposób dla wielu platform.
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 0ee89728395134f26972ebefa28ca96925b98c84
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241097"
---
# <a name="xamarinandroid-data-access"></a>Xamarin.Android Data Access

_Większość aplikacji ma pewne wymagania w celu zapisywania danych na urządzeniu lokalnie. Chyba że przypadku małych ilości danych zwykle wymaga to bazę danych i warstwy danych w aplikacji, aby zarządzać dostępem do bazy danych.  Systemu android zawiera aparat bazy danych SQLite "wbudowane", a dostęp do przechowywania i pobierania danych jest uproszczone przez platformy Xamarin. W tym dokumencie pokazano, jak korzystać z bazy danych SQLite w sposób dla wielu platform._

## <a name="data-access-overview"></a>Omówienie dostępu do danych

Większość aplikacji ma pewne wymagania w celu zapisywania danych na urządzeniu lokalnie. Chyba że przypadku małych ilości danych zwykle wymaga to bazę danych i warstwy danych w aplikacji, aby zarządzać dostępem do bazy danych. Android zarówno zawiera aparat bazy danych Sqlite "wbudowane" i dostęp do danych jest uproszczone przez platformę Xamarin, która jest dostarczana razem z dostawcą danych SQLite.

Platforma Xamarin.Android obsługuje dostęp do bazy danych interfejsów API takich jak:

-  Platforma ADO.NET.
-  3 bazy danych SQLite NET biblioteki innej firmy.

Większość kodu w tej sekcji jest całkowicie dla wielu platform i zostanie uruchomiony w systemie iOS lub Android bez żadnych modyfikacji. Istnieją dwie przykładowe aplikacje omówiono:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; danych proste operacje zapisuje wyniki do tekstu są wyświetlane formantu.

-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; integruje operacje na danych w małych działającą aplikację, która zawiera listę i edytuje prostą strukturą.

Oba rozwiązania przykładowe zawierają systemów iOS i Android przykładowe projekty aplikacji.

W przypadku aplikacji platformy Xamarin.Forms, przeczytaj [Praca z bazami danych](~/xamarin-forms/app-fundamentals/databases.md) co wyjaśnia sposób pracy z bazy danych SQLite w bibliotece PCL z zestawem narzędzi Xamarin.Forms.

W tematach w tej sekcji omówiono dostęp do danych na platformie Xamarin.Android przy użyciu systemu SQLite jako aparat bazy danych. Baza danych jest możliwy "bezpośrednio" przy użyciu składni ADO.NET, lub możesz uwzględnić Relacyjnego biblioteki SQLite.NET i wykonywania operacji na danych w języku C#.

Dwa przykłady są przeglądane: taki, który zawiera kod dostępu do danych bardzo prosty, że dane wyjściowe do pola tekstowego i prostą aplikację, która obejmuje tworzenia, odczytu, aktualizowanie i usuwanie funkcji. Wątki i jak do generowania aplikacji za pomocą wstępnie wypełnionych bazy danych SQLite jest również omówiona.

Dodatkowe przykłady dostęp do danych dla wielu platform, zobacz nasze [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) analizą przypadku firmy.


## <a name="related-links"></a>Linki pokrewne

- [Dostęp Basic (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp zaawansowany (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisy na danych w systemie android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Dostęp do danych z zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
