---
title: Za pomocą ADO.NET z systemem Android
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 29e81afdf2c46cdefc68e2c2fae4e6e47999a346
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/20/2018
---
# <a name="using-adonet-with-android"></a>Za pomocą ADO.NET z systemem Android

Xamarin ma wbudowaną obsługę bazy danych SQLite jest dostępna w systemie Android, który można udostępnić przy użyciu znanych składni ADO.NET. Za pomocą tych interfejsów API wymaga tworzenia instrukcji SQL, które są przetwarzane przez SQLite, takich jak `CREATE TABLE`, `INSERT` i `SELECT` instrukcje.

## <a name="assembly-references"></a>Odwołania do zestawów

Umożliwia dostęp do bazy danych SQLite za pomocą ADO.NET, należy dodać `System.Data` i `Mono.Data.Sqlite` odwołuje się do projektu systemu Android, jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin) 

![Odwołania dla systemu android w programie Visual Studio](using-adonet-images/image7.png "odwołuje się do systemu Android w programie Visual Studio") 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac) 

![Odwołania dla systemu android w programie Visual Studio for Mac](using-adonet-images/image5.png "Android odwołuje się w programie Visual Studio dla komputerów Mac") 

-----


Kliknij prawym przyciskiem myszy **odwołania > Edytuj odwołania...**  , a następnie kliknij, aby wybrać wymaganych zestawów.

## <a name="about-monodatasqlite"></a>About Mono.Data.Sqlite

Używamy `Mono.Data.Sqlite.SqliteConnection` klasy w celu utworzenia pliku pustą bazę danych, a następnie można utworzyć wystąpienia `SqliteCommand` obiekty można używanym do wykonywania instrukcji SQL w bazie danych.

**Tworzenie pustej bazy danych** &ndash; wywołać `CreateFile` metody o prawidłowej (ie. zapisywalny) ścieżka pliku. Należy sprawdzić, czy plik już istnieje przed wywołaniem tej metody, w przeciwnym razie nową (pustą) bazę danych zostanie utworzone w górnej części starego i dane w starym pliku zostaną utracone.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` `dbPath` Zmiennej należy określić zgodnie z regułami opisem we wcześniejszej części tego dokumentu.

**Tworzenie połączenia z bazą danych** &ndash; po utworzeniu pliku bazy danych SQLite można utworzyć obiektu połączenia dostępu do danych. Połączenie jest tworzony przy użyciu parametrów połączenia, które mają postać `Data Source=file_path`, jak pokazano poniżej:

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Jak wspomniano wcześniej, połączenie powinien nigdy być ponownie używane w różnych wątkach. W razie wątpliwości, należy utworzyć połączenie zgodnie z wymaganiami i zamknij go, gdy wszystko będzie gotowe; ale mając na uwadze podczas tego więcej często niż jest to wymagane za.

**Tworzenie i wykonywanie polecenia bazy danych** &ndash; po mamy połączenia możemy wykonywać dowolne polecenia SQL na nim. Kod poniżej przedstawia `CREATE TABLE` instrukcja jest wykonywana.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

Podczas wykonywania bezpośrednio w bazie danych SQL należy wziąć zwykłe środki ostrożności nie dokonywać nieprawidłowych żądań, takich jak próby utworzenia tabeli, która już istnieje. Zachowaj informacje o strukturze bazy danych tak, aby nie spowodować `SqliteException` takich jak **istnieje już w tabeli błędów SQLite [elementy]**.

## <a name="basic-data-access"></a>Dostęp do podstawowych danych

*DataAccess_Basic* przykładowy kod dla tego dokumentu wygląda na to, gdy uruchomiony w systemie Android:

![Android próbki ADO.NET](using-adonet-images/image8.png "próbki ADO.NET dla systemu Android")

Poniższy kod ilustruje sposób wykonywać proste operacje SQLite i wyświetla wyniki w jako tekst w oknie głównym aplikacji.

Należy uwzględnić te przestrzeni nazw:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

Poniższy przykładowy kod przedstawia interakcji całej bazy danych:

1.  Tworzenie pliku bazy danych
2.  Wstawianie niektórych danych
3.  Wykonywanie zapytań dotyczących danych

Te operacje zwykle pojawią się w wielu miejscach w kodzie, na przykład można utworzyć pliku bazy danych i tabel po pierwszym uruchomieniu aplikacji i wykonywania danych odczyty i zapisy na poszczególnych ekranach w aplikacji. W poniższym przykładzie zostały zgrupowane jako pojedynczej metody w tym przykładzie:

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

## <a name="more-complex-queries"></a>Bardziej złożonych zapytań

Ponieważ SQLite pozwala dowolnego polecenia SQL do uruchomienia z danymi, można wykonywać niezależnie od `CREATE`, `INSERT`, `UPDATE`, `DELETE`, lub `SELECT` instrukcje chcesz. Możesz przeczytać o poleceniach SQL obsługiwanych przez SQLite w witrynie sieci Web Sqlite. Instrukcje SQL są uruchamiane przy użyciu jednej z trzech metod na `SqliteCommand` obiektu:

-   **ExecuteNonQuery** &ndash; zwykle używany w przypadku wstawiania do tworzenia lub danych tabeli. Liczba zmodyfikowanych wierszy jest wartości zwracanej przez niektóre operacje, w przeciwnym razie -1.

-   **ExecuteReader** &ndash; używany, gdy kolekcja wierszy ma zostać zwrócony jako `SqlDataReader`.

-   **ExecuteScalar** &ndash; pobiera pojedynczą wartość (na przykład agregacji).


### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT`, `UPDATE`, i `DELETE` instrukcje zwróci liczbę uwzględnionych wierszy. Wszystkie instrukcje SQL zostanie zwrócona wartość -1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Poniżej przedstawiono metody `WHERE` w klauzuli `SELECT` instrukcji.
Ponieważ kod jest obsługuje tworzenie pełną instrukcję SQL należy zwrócić uwagę, aby escape zarezerwowanych znaków, takich jak cudzysłowu (') ciągi.

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

`ExecuteReader` Metoda zwraca `SqliteDataReader` obiektu. Oprócz `Read` metod przedstawionych w tym przykładzie inne przydatne właściwości obejmują:

-   **RowsAffected** &ndash; liczby wierszy dotyczy zapytanie.

-   **HasRows** &ndash; Określa, czy wszystkie wiersze zostały zwrócone.


### <a name="executescalar"></a>EXECUTESCALAR

Użyj tej dla `SELECT` instrukcji, które zwraca pojedynczą wartość (na przykład agregacji).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Jest zwracany typ metody `object` &ndash; powinien Rzutuj wynik metody w zależności od kwerendy bazy danych. Wynik może być liczbą całkowitą od `COUNT` zapytania lub ciąg, z jedną kolumną `SELECT` zapytania. Należy pamiętać, że ta różni się od innych `Execute` metod, które zwracają obiekt czytnika lub liczbę liczbę uwzględnionych wierszy.



## <a name="related-links"></a>Linki pokrewne

- [Basic dostęp (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp Zaawansowane (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisami danych w systemie android](https://developer.xamarin.com/recipes/android/data/)
- [Dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
