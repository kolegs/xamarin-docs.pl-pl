---
title: Konfigurowanie bazy danych SQLite w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano sposób określania lokalizacji pliku bazy danych SQLite w aplikacji platformy Xamarin.iOS. Te pojęcia mają zastosowanie bez względu na to mechanizm dostępu do wybranych danych.
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: f170aa753ff490b75ab66ac858051516935876fe
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241269"
---
# <a name="configuring-sqlite-in-xamarinios"></a>Konfigurowanie bazy danych SQLite w rozszerzeniu Xamarin.iOS

Aby użyć bazy danych SQLite w aplikacji platformy Xamarin.iOS należy ustalić prawidłowej lokalizacji pliku dla pliku bazy danych.

## <a name="database-file-path"></a>Ścieżka pliku bazy danych

Przed dane mogą być przechowywane za pomocą SQLite, niezależnie od tego, w których możesz użyć metoda dostępu do danych, należy utworzyć plik bazy danych. W zależności od platformy docelowej lokalizacji pliku może się różnić. Dla systemu iOS można użyć klasy środowiska do konstruowania prawidłową ścieżkę, jak pokazano w poniższym fragmencie kodu:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Istnieją inne czynności, aby wziąć pod uwagę podczas podejmowania decyzji o miejsce przechowywania plików bazy danych. W systemie iOS, możesz chcieć bazę danych do kopii zapasowej automatycznie (lub nie).

Jeśli chcesz użyć innej lokalizacji na każdej z platform aplikacji dla wielu platform umożliwia dyrektywy kompilatora pokazany Generowanie inną ścieżkę dla każdej platformy:

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Zapoznaj się [Praca w systemie plików](~/ios/app-fundamentals/file-system.md) artykuł, aby uzyskać więcej informacji na temat ich lokalizacje plików do użycia w systemie iOS. Zobacz [tworzenie Cross Platform aplikacji](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokumentu, aby uzyskać więcej informacji na temat używania dyrektywy kompilatora pisanie kodu określonych dla każdej platformy.

## <a name="threading"></a>Wątkowość

Nie należy używać tego samego połączenia bazy danych SQLite w wielu wątkach. Uważaj otworzyć, użycia, a następnie zamknij wszystkie połączenia, które utworzono na tym samym wątku.

Aby upewnić się, że kod nie jest próba uzyskania dostępu bazy danych SQLite z wielu wątków, w tym samym czasie, ręcznie zastosować blokadę, zawsze wtedy, gdy ma dostęp do bazy danych, takich jak to:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Wszystkie dostępu do bazy danych (odczyty, zapisy, aktualizacje itp.) powinna być otoczona przy użyciu tego samego blokady. Należy uważać, aby uniknąć sytuacji zakleszczenia przez zagwarantowanie, że praca wewnątrz klauzuli blokady jest uproszczone i nie wymaga do innych metod, które również mogą zastosować blokadę!


## <a name="related-links"></a>Linki pokrewne

- [Dostęp Basic (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp zaawansowany (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisy na danych w systemie iOS](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Dostęp do danych z zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
