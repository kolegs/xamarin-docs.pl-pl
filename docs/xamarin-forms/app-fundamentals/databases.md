---
title: Lokalnych baz danych
description: "Platformy Xamarin.Forms obsługuje opartej na bazie danych aplikacji przy użyciu aparatu bazy danych SQLite, co umożliwia przy ładowaniu i zapisywaniu obiektów w kodzie udostępnionego. W tym artykule opisano sposób aplikacji platformy Xamarin.Forms można odczytywania i zapisywania danych przy użyciu SQLite.Net z lokalną bazą danych SQLite."
ms.topic: article
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2017
ms.openlocfilehash: afbd2d8f25cdf51c7c7c33f72f10e3b5ce8762ef
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="local-databases"></a>Lokalnych baz danych

_Platformy Xamarin.Forms obsługuje opartej na bazie danych aplikacji przy użyciu aparatu bazy danych SQLite, co umożliwia przy ładowaniu i zapisywaniu obiektów w kodzie udostępnionego. W tym artykule opisano sposób aplikacji platformy Xamarin.Forms można odczytywania i zapisywania danych przy użyciu SQLite.Net z lokalną bazą danych SQLite._

## <a name="overview"></a>Omówienie

Można użyć aplikacji platformy Xamarin.Forms [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) pakietu, aby operacje bazy danych do udostępnionych kodu za pomocą odwołań do `SQLite` klasy, które są dostarczane w polu. Operacje bazy danych można zdefiniować w projekcie przenośnej biblioteki klasy (PCL) rozwiązania platformy Xamarin.Forms z projektami specyficzne dla platformy, zwracany jest ścieżką do przechowywania bazy danych.

Towarzyszącego [Przykładowa aplikacja](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) jest prostą aplikację lista czynności do wykonania. Poniższe zrzuty ekranu pokazują, jak próbki pojawia się na każdej platformie:

[ ![Zrzuty ekranu przykładowe bazy danych platformy Xamarin.Forms](databases-images/todo-list-sml.png "zrzuty ekranu TodoList pierwszej strony")](databases-images/todo-list.png "zrzuty ekranu TodoList pierwszej strony") [ ![ Zrzuty ekranu przykładowe bazy danych platformy Xamarin.Forms](databases-images/todo-detail-sml.png "zrzuty ekranu TodoList drugiej strony")](databases-images/todo-detail.png "zrzuty ekranu TodoList drugiej strony")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>Przy użyciu systemu SQLite

W tej sekcji pokazano, jak dodać SQLite.Net dzaj pakietami nuget rozwiązania platformy Xamarin.Forms, zapis metody służące do wykonywania operacji bazy danych, a następnie użyj [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) Aby określić lokalizację do przechowywania bazy danych na każdej z platform.

<a name="XamarinForms_PCL_Project" />

### <a name="xamarinsforms-pcl-project"></a>Xamarins.Forms PCL Project

Aby dodać obsługę SQLite do projektu platformy Xamarin.Forms PCL, użyj funkcji wyszukiwania w narzędziu NuGet można znaleźć **sqlite-net-pcl** i zainstalować pakiet:

![](databases-images/vs2017-sqlite-pcl-nuget.png "Dodaj pakiet PCL NuGet SQLite.NET")

Istnieje wiele pakietów NuGet o podobnej nazwie, poprawny pakiet ma następujące atrybuty:

- **Utworzone przez:** Paweł Krueger A.
- **Identyfikator:** sqlite-net-pcl
- **NuGet link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

Po dodaniu odwołania zapisu interfejs abstrakcyjnej możliwości specyficzne dla platformy, które można określić lokalizacji plików bazy danych. Interfejs używany w próbce definiuje jedną metodę:

```csharp
public interface IFileHelper
{
  string GetLocalFilePath(string filename);
}
```

Po zdefiniowaniu interfejsu, użyj [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) Aby uzyskać implementację i ścieżkę do pliku lokalnego (Zauważ, że ten interfejs nie została zaimplementowana jeszcze). Poniższy kod pobiera implementację w `App.Database` właściwości:

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(DependencyService.Get<IFileHelper>().GetLocalFilePath("TodoSQLite.db3"));
    }
    return database;
  }
}
```

`TodoItemDatabase` Konstruktor jest pokazany poniżej:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Ta metoda tworzy połączenie pojedynczej bazy danych, które pozostaje otwarty podczas uruchamiania aplikacji, w związku z tym uniknąć kosztów otwierające i zamykające plik bazy danych za każdym razem, gdy wykonywana jest operacja bazy danych.

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

Wszystkie dane kod dostępu jest zapisywany w projekcie PCL do udostępnienia na wszystkich platformach. Tylko pierwsze ścieżkę do pliku lokalnego dla bazy danych wymaga kod specyficzne dla platformy, zgodnie z opisem w poniższych sekcjach.

<a name="PCL_iOS" />

### <a name="ios-project"></a>iOS projektu

Aby skonfigurować aplikację systemu iOS, należy dodać ten sam pakiet NuGet do projektu systemu iOS przy użyciu *NuGet* okno:

![](databases-images/vsmac-sqlite-nuget.png "Dodaj pakiet PCL NuGet SQLite.NET")

Jest wymagany kod tylko `IFileHelper` implementację, która określa ścieżkę pliku danych. Poniższy kod umieszcza plik bazy danych SQLite w **bazy danych i bibliotek** folder w aplikacji piaskownicy. Zobacz [iOS Praca w systemie plików](~/ios/app-fundamentals/file-system.md) dokumentację, aby uzyskać więcej informacji na temat różnych katalogach, które są dostępne dla magazynu.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.iOS
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            string docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
            string libFolder = Path.Combine(docFolder, "..", "Library", "Databases");

            if (!Directory.Exists(libFolder))
            {
                Directory.CreateDirectory(libFolder);
            }

            return Path.Combine(libFolder, filename);
        }
    }
}
```

Należy pamiętać, że kod zawiera `assembly:Dependency` atrybutu, dzięki czemu ta implementacja będzie wykrywalny przez `DependencyService`.

<a name="PCL_Android" />

### <a name="android-project"></a>Projekt systemu android

Aby skonfigurować aplikację systemu Android, Dodaj ten sam pakiet NuGet do projektu systemu Android przy użyciu *NuGet* okno:

![](databases-images/vsmac-sqlite-nuget.png "Dodaj pakiet PCL NuGet SQLite.NET")

Po dodaniu tego odwołania, jest tylko kod wymagany `IFileHelper` implementację, która określa ścieżkę pliku danych.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.Droid
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            string path = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
            return Path.Combine(path, filename);
        }
    }
}
```

<a name="PCL_UWP" />

### <a name="windows-10-universal-windows-platform-uwp"></a>Platforma uniwersalna systemu Windows 10 systemu Windows (UWP)

Do konfigurowania aplikacji platformy uniwersalnej systemu Windows, należy dodać ten sam pakiet NuGet do projektu platformy uniwersalnej systemu Windows przy użyciu *NuGet* okno:

![](databases-images/vs2017-sqlite-uwp-nuget.png "Dodaj pakiet PCL NuGet SQLite.NET")

Po dodaniu odwołania zaimplementować `IFileHelper` interfejsu przy użyciu określonych platform `Windows.Storage` interfejsu API, aby określić ścieżkę pliku danych.

```csharp
using Windows.Storage;
...

[assembly: Dependency(typeof(FileHelper))]
namespace Todo.UWP
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            return Path.Combine(ApplicationData.Current.LocalFolder.Path, filename);
        }
    }
}

```

## <a name="summary"></a>Podsumowanie

Platformy Xamarin.Forms obsługuje opartej na bazie danych aplikacji przy użyciu aparatu bazy danych SQLite, co umożliwia przy ładowaniu i zapisywaniu obiektów w kodzie udostępnionego.

Ten artykuł skupia się na **podczas uzyskiwania dostępu do** bazy danych SQLite za pomocą platformy Xamarin.Forms. Aby uzyskać więcej informacji na temat pracy z SQLite.Net się odwoływać się do [dostępu do danych: za pomocą SQLite.NET](~/cross-platform/app-fundamentals/index.md) dokumentacji. Większość kodu SQLite.Net jest zabezpieczać na wszystkich platformach; Konfigurowanie tylko lokalizacja pliku bazy danych SQLite wymaga funkcji specyficzne dla platformy.


## <a name="related-links"></a>Linki pokrewne

- [Przykładowe ToDo](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Baza danych skoroszytu](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
