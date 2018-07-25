---
title: Za pomocą ADO.NET z systemem Android
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 9e0c1be2e37355242db2fb70857d90127c3b5259
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242215"
---
# <a name="using-adonet-with-android"></a>Za pomocą ADO.NET z systemem Android

Xamarin ma wbudowaną obsługę bazy danych SQLite, która jest dostępna w systemie Android i udostępniane przy użyciu dobrze znanej składni notacji ADO.NET. Za pomocą tych interfejsów API do pisania instrukcji SQL, które są przetwarzane przez bazy danych SQLite, takich jak wymaga `CREATE TABLE`, `INSERT` i `SELECT` instrukcji.

## <a name="assembly-references"></a>Odwołania do zestawów

Aby korzystać z dostępu SQLite za pomocą ADO.NET, należy dodać `System.Data` i `Mono.Data.Sqlite` odwołuje się do projektu systemu Android, jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin) 

![Android — materiały dodatkowe w programie Visual Studio](using-adonet-images/image7.png "odwołuje się do systemu Android w programie Visual Studio") 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac) 

![Android — materiały dodatkowe w programie Visual Studio dla komputerów Mac](using-adonet-images/image5.png "Android odwołuje się w programie Visual Studio dla komputerów Mac") 

-----


Kliknij prawym przyciskiem myszy **odwołania > Edytuj odwołania...**  , a następnie kliknij, aby wybrać wymaganych zestawów.

## <a name="about-monodatasqlite"></a>About Mono.Data.Sqlite

Firma Microsoft użyje `Mono.Data.Sqlite.SqliteConnection` klasy w celu utworzenia pliku pustą bazę danych i następnie utworzyć `SqliteCommand` obiektów, że możemy użyć w celu wykonania instrukcji SQL w bazie danych.

**Tworzenie pustej bazy danych** &ndash; wywołania `CreateFile` metody przy użyciu prawidłowego (tj. zapisywalnego) ścieżki pliku. Należy sprawdzić, czy plik już istnieje przed wywołaniem tej metody, w przeciwnym razie nową (pustą) bazę danych zostanie utworzony na początku starą i dane w starym pliku zostaną utracone.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` `dbPath` Można określić zmienną, zgodnie z zasadami omówionych wcześniej w tym dokumencie.

**Tworzenie połączenia z bazą danych** &ndash; po utworzeniu pliku bazy danych SQLite, można utworzyć obiektu połączenia dostępu do danych. Połączenie jest konstruowany przy użyciu parametrów połączenia, które mają postać `Data Source=file_path`, jak pokazano poniżej:

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Jak wspomniano wcześniej, połączenie powinno nigdy nie być ponownie używane w różnych wątkach. W razie wątpliwości, tworzenia połączenia zgodnie z wymaganiami i zamknij go, gdy wszystko będzie gotowe; ale w trosce o wykonując bardziej często niż jest to wymagane za.

**Tworzenie i wykonywanie polecenia bazy danych** &ndash; po wygenerowaniu połączenia możemy wykonywać dowolne polecenia SQL przed nim. Poniższy kod przedstawia `CREATE TABLE` wykonywana instrukcja.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

Podczas wykonywania SQL bezpośrednio w odniesieniu do bazy danych, jakie należy podjąć w normalnym środki ostrożności nie dokonywać nieprawidłowych żądań, takich jak próby utworzenia tabeli, która już istnieje. Zachowaj informacje o struktury bazy danych, tak, aby nie spowodować `SqliteException` takich jak **tabeli błędów bazy danych SQLite [elementów] już istnieje**.

## <a name="basic-data-access"></a>Dostęp do danych podstawowych

*DataAccess_Basic* przykładowy kod dla tego dokumentu wyglądają następująco, podczas uruchamiania w systemie Android:

![Przykład systemu android ADO.NET](using-adonet-images/image8.png "przykładowe ADO.NET dla systemu Android")

Poniższy kod ilustruje sposób wykonywania prostych operacji bazy danych SQLite i przedstawiono wyniki w postaci tekstu w oknie głównym aplikacji.

Będą potrzebne uwzględnić te przestrzenie nazw:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

Poniższy przykład kodu pokazuje interakcji całą bazę danych:

1.  Tworzenie pliku bazy danych
2.  Wstawianie niektórych danych
3.  Wykonywanie zapytań dotyczących danych

Te operacje zazwyczaj pojawią się w wielu miejscach w kodzie, na przykład można utworzyć pliku bazy danych i tabel po pierwszym uruchomieniu aplikacji i wykonania danych operacji odczytu i zapisu na poszczególnych ekranach w swojej aplikacji. W poniższym przykładzie, zostały zgrupowane jako pojedynczej metody w tym przykładzie:

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}

```

## <a name="more-complex-queries"></a>Bardziej złożone zapytania

Ponieważ bazy danych SQLite pozwala dowolnego polecenia SQL, aby być uruchamiane względem danych, można wykonać dowolne `CREATE`, `INSERT`, `UPDATE`, `DELETE`, lub `SELECT` instrukcji, które chcesz. Możesz przeczytać o poleceń SQL obsługiwanych przez bazy danych SQLite w witrynie sieci Web bazy danych Sqlite. Instrukcje SQL są uruchamiane przy użyciu jednej z trzech metod w `SqliteCommand` obiektu:

-   **ExecuteNonQuery** &ndash; zwykle używany w przypadku wstawiania do tworzenia lub danych tabeli. Wartość zwracana w przypadku niektórych operacji jest liczbę uwzględnionych wierszy, w przeciwnym razie wartość -1.

-   **ExecuteReader** &ndash; używany, gdy kolekcja wierszy ma zostać zwrócony jako `SqlDataReader`.

-   **ExecuteScalar** &ndash; pobiera pojedynczą wartość (na przykład agregacją).


### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT`, `UPDATE`, i `DELETE` instrukcji zwróci liczbę uwzględnionych wierszy. Inne instrukcje SQL zostanie zwrócona wartość -1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Metoda pokazano `WHERE` w klauzuli `SELECT` instrukcji.
Ponieważ kod jest umożliwiają utworzenie dobrze dopasowanego pełną instrukcję SQL go należy uważać, aby wyprowadza zastrzeżonych znaków, takich jak cudzysłowu (') ciągi.

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

`ExecuteReader` Metoda zwraca `SqliteDataReader` obiektu. Oprócz `Read` metod przedstawionych w tym przykładzie innych użytecznych właściwości obejmują:

-   **RowsAffected** &ndash; liczbę wierszy dotyczy zapytanie.

-   **HasRows** &ndash; czy zwrócono żadnych wierszy.


### <a name="executescalar"></a>EXECUTESCALAR

Użyj tego zadania dla `SELECT` instrukcji, które zwraca pojedynczą wartość (takie jak zagregowanych).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Typ zwracany metody jest `object` &ndash; powinien Rzutuj wynik metody w zależności od zapytanie bazy danych. Wynik może być liczbą całkowitą od `COUNT` zapytania lub ciąg z jednej kolumny `SELECT` zapytania. Należy zauważyć, że to różni się od innych `Execute` metod, które zwracają obiekt czytnika lub liczbę uwzględnionych wierszy.



## <a name="related-links"></a>Linki pokrewne

- [Dostęp Basic (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp zaawansowany (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisy na danych w systemie android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Dostęp do danych z zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
