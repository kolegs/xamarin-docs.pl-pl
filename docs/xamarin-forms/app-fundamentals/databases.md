---
title: Platformy Xamarin.Forms lokalnych baz danych
description: Platformy Xamarin.Forms obsługuje opartej na bazie danych aplikacji przy użyciu aparatu bazy danych SQLite, co umożliwia przy ładowaniu i zapisywaniu obiektów w kodzie udostępnionego. W tym artykule opisano sposób aplikacji platformy Xamarin.Forms można odczytywania i zapisywania danych przy użyciu SQLite.Net z lokalną bazą danych SQLite.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: feec4993a0719a083d713e084552b18aead8ee42
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310143"
---
# <a name="xamarinforms-local-databases"></a>Platformy Xamarin.Forms lokalnych baz danych

_Platformy Xamarin.Forms obsługuje opartej na bazie danych aplikacji przy użyciu aparatu bazy danych SQLite, co umożliwia przy ładowaniu i zapisywaniu obiektów w kodzie udostępnionego. W tym artykule opisano sposób aplikacji platformy Xamarin.Forms można odczytywania i zapisywania danych przy użyciu SQLite.Net z lokalną bazą danych SQLite._

## <a name="overview"></a>Omówienie

Można użyć aplikacji platformy Xamarin.Forms [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) pakietu, aby operacje bazy danych do udostępnionych kodu za pomocą odwołań do `SQLite` klasy, które są dostarczane w polu. Operacje bazy danych można zdefiniować w projekcie biblioteki .NET Standard rozwiązania platformy Xamarin.Forms.

Towarzyszącego [Przykładowa aplikacja](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) jest prostą aplikację lista czynności do wykonania. Poniższe zrzuty ekranu pokazują, jak próbki pojawia się na każdej platformie:

[![Zrzuty ekranu przykładowe bazy danych platformy Xamarin.Forms](databases-images/todo-list-sml.png "zrzuty ekranu TodoList pierwszej strony")](databases-images/todo-list.png#lightbox "zrzuty ekranu TodoList pierwszej strony") [ ![ Zrzuty ekranu przykładowe bazy danych platformy Xamarin.Forms](databases-images/todo-list-sml.png "zrzuty ekranu TodoList pierwszej strony")](databases-images/todo-list.png#lightbox "zrzuty ekranu TodoList pierwszej strony")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>Przy użyciu systemu SQLite

Aby dodać obsługę SQLite do biblioteki platformy Xamarin.Forms .NET Standard, użyj funkcji wyszukiwania w narzędziu NuGet można znaleźć **sqlite-net-pcl** i zainstaluj najnowszy pakiet:

![Dodaj pakiet PCL NuGet SQLite.NET](databases-images/vs2017-sqlite-pcl-nuget.png "Dodaj pakiet PCL NuGet SQLite.NET")

Istnieje wiele pakietów NuGet o podobnej nazwie, poprawny pakiet ma następujące atrybuty:

- **Utworzone przez:** Paweł Krueger A.
- **Identyfikator:** sqlite-net-pcl
- **NuGet link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Pomimo nazwę pakietu, należy użyć **sqlite-net-pcl** pakietu NuGet, nawet w projektach platformy .NET Standard.

Po dodaniu odwołania Dodawanie właściwości do `App` klasy, która zwraca ścieżkę do pliku lokalnego do przechowywania bazy danych:

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

`TodoItemDatabase` Konstruktora, który przyjmuje ścieżkę do pliku bazy danych jako argument, przedstawiono poniżej:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Zaletą udostępnianie bazy danych jako pojedynczego jest utworzone połączenie pojedynczej bazy danych są stale otwarte podczas aplikacja działa, w związku z tym uniknąć kosztów otwieranie i zamykanie plików bazy danych zawsze operacji bazy danych jest wykonywana.

W pozostałej części `TodoItemDatabase` klasa zawiera kwerendy SQLite działać i platform. Poniżej przedstawiono przykładowy kod zapytania (więcej informacji o składni można znaleźć w [przy użyciu SQLite.NET](~/cross-platform/app-fundamentals/index.md) artykułu):

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
> Zaletą używania asynchronicznego interfejsu API SQLite.Net jest tej bazy danych, które operacje są przenoszone do wątki w tle. Ponadto jest niepotrzebna dodatkowe współbieżności kod obsługi, ponieważ interfejs API zajmuje się go zapisać.

## <a name="summary"></a>Podsumowanie

Platformy Xamarin.Forms obsługuje opartej na bazie danych aplikacji przy użyciu aparatu bazy danych SQLite, co umożliwia przy ładowaniu i zapisywaniu obiektów w kodzie udostępnionego.

Ten artykuł skupia się na **podczas uzyskiwania dostępu do** bazy danych SQLite za pomocą platformy Xamarin.Forms. Aby uzyskać więcej informacji na temat pracy z SQLite.Net się odwoływać się do [SQLite.NET w systemie Android](~/android/data-cloud/data-access/using-sqlite-orm.md) lub [SQLite.NET w systemie iOS](~/ios/data-cloud/data/using-sqlite-orm.md) dokumentacji.

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe ToDo](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Baza danych skoroszytu](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
