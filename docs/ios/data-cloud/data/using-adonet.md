---
title: Za pomocą ADO.NET
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 4bf908c51deefea4e8a7e76fbf18b1aea5edee03
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="using-adonet"></a>Za pomocą ADO.NET

Xamarin ma wbudowaną obsługę bazy danych SQLite jest dostępna w systemach iOS, które są udostępniane za pomocą znanych składni ADO.NET. Za pomocą tych interfejsów API wymaga tworzenia instrukcji SQL, które są przetwarzane przez SQLite, takich jak `CREATE TABLE`, `INSERT` i `SELECT` instrukcje.

## <a name="assembly-references"></a>Odwołania do zestawów

Umożliwia dostęp do bazy danych SQLite za pomocą ADO.NET, należy dodać `System.Data` i `Mono.Data.Sqlite` odwołuje się do projektu systemu iOS, jak pokazano poniżej (dla programu Visual Studio for Mac i Visual Studio — przykłady):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](using-adonet-images/image4.png "Odwołania do zestawów w programie Visual Studio dla komputerów Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Odwołania do zestawów w programie Visual Studio")

-----

Kliknij prawym przyciskiem myszy **odwołania > Edytuj odwołania...**  , a następnie kliknij, aby wybrać wymaganych zestawów.

## <a name="about-monodatasqlite"></a>About Mono.Data.Sqlite

Używamy `Mono.Data.Sqlite.SqliteConnection` klasy w celu utworzenia pliku pustą bazę danych, a następnie można utworzyć wystąpienia `SqliteCommand` obiekty można używanym do wykonywania instrukcji SQL w bazie danych.


1. **Tworzenie pustej bazy danych** -Wywołaj `CreateFile` metody o prawidłowej (ie. zapisywalny) ścieżka pliku. Należy sprawdzić, czy plik już istnieje przed wywołaniem tej metody, w przeciwnym razie nową (pustą) bazę danych zostanie utworzone w górnej części starego i dane w starym pliku zostaną utracone:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > `dbPath` Zmiennej należy określić zgodnie z regułami opisem we wcześniejszej części tego dokumentu.

2. **Tworzenie połączenia z bazą danych** — po utworzeniu pliku bazy danych SQLite można utworzyć obiektu połączenia dostępu do danych. Połączenie jest tworzony przy użyciu parametrów połączenia, które mają postać `Data Source=file_path`, jak pokazano poniżej:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Jak wspomniano wcześniej, połączenie powinien nigdy być ponownie używane w różnych wątkach. W razie wątpliwości, należy utworzyć połączenie zgodnie z wymaganiami i zamknij go, gdy wszystko będzie gotowe; ale mając na uwadze podczas tego więcej często niż jest to wymagane za.
    
3. **Tworzenie i wykonywanie polecenia bazy danych** — gdy mamy możemy wykonywać dowolne polecenia SQL na nim połączenia. Poniższy kod wykonywany instrukcji CREATE TABLE.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

Podczas wykonywania bezpośrednio w bazie danych SQL należy wziąć zwykłe środki ostrożności nie dokonywać nieprawidłowych żądań, takich jak próby utworzenia tabeli, która już istnieje. Zachowaj informacje o strukturze bazy danych tak, aby nie spowodować SqliteException, takie jak "istnieje już tabela błąd SQLite [elementy]".

## <a name="basic-data-access"></a>Dostęp do podstawowych danych

*DataAccess_Basic* przykładowy kod dla tego dokumentu wygląda na to, gdy uruchomiony w systemie iOS:

 ![](using-adonet-images/image9.png "Przykładowe ADO.NET dla systemu iOS")

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

Ponieważ SQLite pozwala dowolnego polecenia SQL do uruchomienia z danymi, można wykonywać niezależnie od Utwórz, wstawianie, AKTUALIZOWANIE, usunąć lub wybierz instrukcje, które chcesz. Możesz przeczytać o poleceniach SQL obsługiwanych przez SQLite w witrynie sieci Web Sqlite. Instrukcje SQL są uruchamiane przy użyciu jednej z trzech metod w obiekcie SqliteCommand:

-  **ExecuteNonQuery** — zwykle używane do wstawienia do tworzenia lub danych tabeli. Liczba zmodyfikowanych wierszy jest wartości zwracanej przez niektóre operacje, w przeciwnym razie -1.
-  **ExecuteReader** — używany, gdy kolekcja wierszy ma zostać zwrócony jako `SqlDataReader` .
-  **ExecuteScalar** — pobiera pojedynczą wartość (na przykład agregacji).


### <a name="executenonquery"></a>EXECUTENONQUERY

Instrukcje INSERT, UPDATE i DELETE zwróci liczbę uwzględnionych wierszy. Wszystkie instrukcje SQL zostanie zwrócona wartość -1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Następująca metoda zawiera klauzulę WHERE w instrukcji SELECT. Ponieważ kod jest obsługuje tworzenie pełną instrukcję SQL należy zwrócić uwagę, aby escape zarezerwowanych znaków, takich jak cudzysłowu (') ciągi.

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

Metoda ExecuteReader zwraca obiekt SqliteDataReader. Oprócz metody Read pokazano w przykładzie inne przydatne właściwości obejmują:

-  **RowsAffected** — liczba wierszy dotyczy zapytanie.
-  **HasRows** — czy wszystkie wiersze zostały zwrócone.


### <a name="executescalar"></a>EXECUTESCALAR

Użyj tego dla instrukcji SELECT, które zwraca pojedynczą wartość (na przykład agregacji).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Jest zwracany typ metody `object` — powinien Rzutuj wynik metody w zależności od kwerendy bazy danych. Wynik może być liczbą całkowitą z zapytania COUNT lub ciąg z jednej kolumny zapytania SELECT. Należy pamiętać, że jest to różni się od innych metod Execute, które zwracają obiekt czytnika lub liczbę liczbę uwzględnionych wierszy.


## <a name="related-links"></a>Linki pokrewne

- [Basic dostęp (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp Zaawansowane (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS przepisami danych](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
