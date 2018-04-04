---
title: Konfiguracja
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: f4a1bc65fbba68b59196702978633eaf46bc44c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="configuration"></a>Konfiguracja

Do korzystania z bazy danych SQLite w aplikacji platformy Xamarin.iOS należy ustalić prawidłowej lokalizacji pliku dla pliku bazy danych.

## <a name="database-file-path"></a>Ścieżki pliku bazy danych

Niezależnie od tego, które używanej metody dostępu do danych należy utworzyć plik bazy danych, przed mogą być przechowywane dane z bazy danych SQLite. W zależności od tego, jakie platformy docelowej lokalizacji pliku będą inne. Dla systemu iOS umożliwia klasy środowiska utworzyć prawidłową ścieżkę, jak pokazano w poniższy fragment kodu:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Istnieją inne czynności, aby wziąć pod uwagę podczas podejmowania decyzji o miejsce przechowywania plików bazy danych. W systemie iOS możesz bazę danych do kopii zapasowej automatycznie (lub nie).

Jeśli chcesz użyć w innej lokalizacji na każdej z platform w aplikacji międzyplatformowego umożliwia dyrektywy kompilatora jak wygenerować różne ścieżki dla każdej platformy:

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

Zapoznaj się [Praca w systemie plików](~/ios/app-fundamentals/file-system.md) artykułu, aby uzyskać więcej informacji na ich lokalizacje plików do użycia w systemie iOS. Zobacz [tworzenie aplikacji dla wielu Platform](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokumentu, aby uzyskać więcej informacji na temat używania dyrektywy kompilatora do pisania kodu, które są określone dla każdej platformy.

## <a name="threading"></a>Wątkowość

Nie należy używać tego samego połączenia bazy danych SQLite przez wiele wątków. Należy zachować ostrożność otworzyć, użyj i zamknij wszystkie połączenia utworzone na tym samym wątku.

Aby upewnić się, że kod nie próbuje dostęp do bazy danych SQLite wiele wątków jednocześnie, ręcznie wykonać blokady zawsze, gdy ma dostęp do bazy danych, takich jak to:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Z tego samego blokady musi być ujęte wszystkie dostęp do bazy danych (operacji odczytu, zapisu, aktualizacji itp.). Należy zadbać, aby uniknąć sytuacji zakleszczenie za zapewnienie, że pracy wewnątrz klauzuli blokady jest uproszczone i nie wywołuje do innych metod, które może również podjąć blokady!


## <a name="related-links"></a>Linki pokrewne

- [Basic dostęp (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp Zaawansowane (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS przepisami danych](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
