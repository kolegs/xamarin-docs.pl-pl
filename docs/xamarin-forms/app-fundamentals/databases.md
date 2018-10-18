---
title: Bazy danych lokalnego zestawu narzędzi Xamarin.Forms
description: Zestaw narzędzi Xamarin.Forms obsługuje opartego na bazie danych aplikacji przy użyciu aparatu bazy danych SQLite, dzięki czemu możliwe do ładowania i zapisywania obiektów w współużytkowanym kodem. W tym artykule opisano, jak aplikacje Xamarin.Forms może odczytywać i zapisywać dane w lokalnej bazie danych SQLite przy użyciu biblioteki SQLite.Net.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 05c77c4c47841a897d0d1de5c3ba004db524ea2d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "36310143"
---
# <a name="xamarinforms-local-databases"></a>Bazy danych lokalnego zestawu narzędzi Xamarin.Forms

_Zestaw narzędzi Xamarin.Forms obsługuje opartego na bazie danych aplikacji przy użyciu aparatu bazy danych SQLite, dzięki czemu możliwe do ładowania i zapisywania obiektów w współużytkowanym kodem. W tym artykule opisano, jak aplikacje Xamarin.Forms może odczytywać i zapisywać dane w lokalnej bazie danych SQLite przy użyciu biblioteki SQLite.Net._

## <a name="overview"></a>Omówienie

Można używać w aplikacjach Xamarin.Forms [NuGet PCL biblioteki SQLite.NET](https://www.nuget.org/packages/sqlite-net-pcl/) pakiet, aby zastosować operacji bazy danych do udostępnionego kodu, odwołując się do `SQLite` klas, które są dostarczane w pakietu NuGet. W projekcie biblioteki .NET Standard, rozwiązania Xamarin.Forms można zdefiniować operacje bazy danych.

Załączony [Przykładowa aplikacja](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) to prosta aplikacja listy zadań do wykonania. Poniższe zrzuty ekranu pokazują, jak próbki pojawia się na każdej z platform:

[![Zestaw narzędzi Xamarin.Forms bazy danych przykładowe zrzuty ekranu](databases-images/todo-list-sml.png "zrzuty ekranu strony pierwszego TodoList")](databases-images/todo-list.png#lightbox "zrzuty ekranu strony pierwszego TodoList") [ ![ Zestaw narzędzi Xamarin.Forms bazy danych przykładowe zrzuty ekranu](databases-images/todo-list-sml.png "zrzuty ekranu strony pierwszego TodoList")](databases-images/todo-list.png#lightbox "zrzuty ekranu strony pierwszego TodoList")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>Przy użyciu systemu SQLite

Aby dodać obsługę bazy danych SQLite do biblioteki zestawu narzędzi Xamarin.Forms .NET Standard, należy użyć funkcji wyszukiwania NuGet można znaleźć **sqlite-net-pcl** i zainstalowania najnowszego pakietu:

![Dodawanie pakietu aplikacji PCL NuGet biblioteki SQLite.NET](databases-images/vs2017-sqlite-pcl-nuget.png "Dodaj pakiet PCL NuGet biblioteki SQLite.NET")

Kilka pakietów NuGet o podobnych nazwach, poprawny pakiet ma następujące atrybuty:

- **Utworzone przez:** Frank A. Krueger
- **Identyfikator:** sqlite-net-pcl
- **NuGet link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Niezależnie od nazwy pakietu, należy użyć **sqlite-net-pcl** pakietu NuGet, nawet w projektach .NET Standard.

Po dodaniu odwołania dodać właściwość `App` klasy, która zwraca lokalną ścieżką pliku do przechowywania bazy danych:

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(
        Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "TodoSQLite.db3"));
    }
    return database;
  }
}
```

`TodoItemDatabase` Konstruktora, który przyjmuje ścieżkę do pliku bazy danych jako argument, znajdują się poniżej:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Zaletą ujawnienia bazy danych jako pojedynczego jest tworzone jest połączenie pojedynczej bazy danych, która pozostaje otwarty podczas aplikacja działa, w związku z tym uniknięcie wydatków otwierających i zamykających plik bazy danych za każdym razem operacja bazy danych jest wykonywana.

W pozostałej części `TodoItemDatabase` klasa zawiera zapytania bazy danych SQLite, uruchamiane dla wielu platform. Poniżej przedstawiono przykładowy kod zapytania (można znaleźć szczegółowe informacje na temat składni w [przy użyciu biblioteki SQLite.NET z rozszerzeniem Xamarin.iOS](~/ios/data-cloud/data/using-sqlite-orm.md).

```csharp
public Task<List<TodoItem>> GetItemsAsync()
{
  return database.Table<TodoItem>().ToListAsync();
}

public Task<List<TodoItem>> GetItemsNotDoneAsync()
{
  return database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
}

public Task<TodoItem> GetItemAsync(int id)
{
  return database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
}

public Task<int> SaveItemAsync(TodoItem item)
{
  if (item.ID != 0)
  {
    return database.UpdateAsync(item);
  }
  else {
    return database.InsertAsync(item);
  }
}

public Task<int> DeleteItemAsync(TodoItem item)
{
  return database.DeleteAsync(item);
}
```

> [!NOTE]
> Zaletą korzystania z asynchronicznego interfejsu API biblioteki SQLite.Net jest tej bazy danych, które operacje są przenoszone do wątków w tle. Ponadto nie ma potrzeby współbieżności dodatkowe obsługę kodu, ponieważ interfejs API zajmuje się go zapisać.

## <a name="summary"></a>Podsumowanie

Zestaw narzędzi Xamarin.Forms obsługuje opartego na bazie danych aplikacji przy użyciu aparatu bazy danych SQLite, dzięki czemu możliwe do ładowania i zapisywania obiektów w współużytkowanym kodem.

Ten artykuł koncentruje się na **uzyskiwania dostępu do** bazy danych SQLite przy użyciu zestawu narzędzi Xamarin.Forms. Aby uzyskać więcej informacji na temat pracy z biblioteki SQLite.Net sam dotyczą [biblioteki SQLite.NET w systemie Android](~/android/data-cloud/data-access/using-sqlite-orm.md) lub [biblioteki SQLite.NET w systemie iOS](~/ios/data-cloud/data/using-sqlite-orm.md) dokumentacji.

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe ToDo](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)

