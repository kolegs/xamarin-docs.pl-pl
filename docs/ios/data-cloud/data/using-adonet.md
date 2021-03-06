---
title: Za pomocą ADO.NET z rozszerzeniem Xamarin.iOS
description: W tym dokumencie opisano, jak na potrzeby ADO.NET jako metodę dostępu do bazy danych SQLite w aplikacji platformy Xamarin.iOS. Omówiono w nim odwołania do zestawów, Mono.Data.Sqlite i przykładowe BasicDataAccess.
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 83f6059c405b2156270f4359cbba33177861af02
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241241"
---
# <a name="using-adonet-with-xamarinios"></a>Za pomocą ADO.NET z rozszerzeniem Xamarin.iOS

Xamarin ma wbudowaną obsługę bazy danych SQLite, która jest dostępna w systemach iOS, które są uwidaczniane za pomocą dobrze znanej składni notacji ADO.NET. Za pomocą tych interfejsów API do pisania instrukcji SQL, które są przetwarzane przez bazy danych SQLite, takich jak wymaga `CREATE TABLE`, `INSERT` i `SELECT` instrukcji.

## <a name="assembly-references"></a>Odwołania do zestawów

Aby korzystać z dostępu SQLite za pomocą ADO.NET, należy dodać `System.Data` i `Mono.Data.Sqlite` odwołuje się do projektu systemu iOS, jak pokazano w tym miejscu (Aby uzyskać przykłady w programie Visual Studio dla komputerów Mac i Visual Studio):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](using-adonet-images/image4.png "Odwołania do zestawów w programie Visual Studio dla komputerów Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Odwołania do zestawów w programie Visual Studio")

-----

Kliknij prawym przyciskiem myszy **odwołania > Edytuj odwołania...**  , a następnie kliknij, aby wybrać wymaganych zestawów.

## <a name="about-monodatasqlite"></a>About Mono.Data.Sqlite

Firma Microsoft użyje `Mono.Data.Sqlite.SqliteConnection` klasy w celu utworzenia pliku pustą bazę danych i następnie utworzyć `SqliteCommand` obiektów, że możemy użyć w celu wykonania instrukcji SQL w bazie danych.


1. **Tworzenie pustej bazy danych** -Wywołaj `CreateFile` metody przy użyciu prawidłowego (ie. zapisywalny) ścieżki pliku. Należy sprawdzić, czy plik już istnieje przed wywołaniem tej metody, w przeciwnym razie nową (pustą) bazę danych zostanie utworzony na początku starą i dane w starym pliku zostaną utracone:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > `dbPath` Można określić zmienną, zgodnie z zasadami omówionych wcześniej w tym dokumencie.

2. **Tworzenie połączenia z bazą danych** — po utworzeniu pliku bazy danych SQLite można utworzyć obiektu połączenia dostępu do danych. Połączenie jest konstruowany przy użyciu parametrów połączenia, które mają postać `Data Source=file_path`, jak pokazano poniżej:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Jak wspomniano wcześniej, połączenie powinno nigdy nie być ponownie używane w różnych wątkach. W razie wątpliwości, tworzenia połączenia zgodnie z wymaganiami i zamknij go, gdy wszystko będzie gotowe; ale w trosce o wykonując bardziej często niż jest to wymagane za.
    
3. **Tworzenie i wykonywanie polecenia bazy danych** — gdy będziemy już mieć połączenie możemy wykonywać dowolne polecenia SQL przed nim. Poniższy kod pokazuje wykonywana instrukcja CREATE TABLE.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

Podczas wykonywania SQL bezpośrednio w odniesieniu do bazy danych, jakie należy podjąć w normalnym środki ostrożności nie dokonywać nieprawidłowych żądań, takich jak próby utworzenia tabeli, która już istnieje. Zachowaj informacje o strukturze bazy danych, tak, aby nie spowodować SqliteException, takie jak "istnieje już tabela błędu oprogramowania SQLite [elementów]".

## <a name="basic-data-access"></a>Dostęp do danych podstawowych

*DataAccess_Basic* przykładowy kod dla tego dokumentu wyglądają następująco, podczas uruchamiania w systemie iOS:

 ![](using-adonet-images/image9.png "przykład ADO.NET dla systemu iOS")

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

Ponieważ bazy danych SQLite pozwala dowolnego polecenia SQL, aby być uruchamiane względem danych, można wykonać niezależnie od utworzenia, WSTAWIANIA, aktualizacji, usuń lub wybierz instrukcji, które chcesz. Możesz przeczytać o poleceń SQL obsługiwanych przez bazy danych SQLite w witrynie sieci Web bazy danych Sqlite. Instrukcje SQL są uruchamiane przy użyciu jednej z trzech metod na obiekcie SqliteCommand:

-  **ExecuteNonQuery** — zwykle używane do wstawienia do tworzenia lub danych tabeli. Wartość zwracana w przypadku niektórych operacji jest liczbę uwzględnionych wierszy, w przeciwnym razie wartość -1.
-  **ExecuteReader** — używany, gdy kolekcja wierszy ma zostać zwrócony jako `SqlDataReader` .
-  **ExecuteScalar** — pobiera pojedynczą wartość (na przykład agregacją).


### <a name="executenonquery"></a>EXECUTENONQUERY

Instrukcje INSERT, UPDATE i DELETE zwróci liczbę uwzględnionych wierszy. Inne instrukcje SQL zostanie zwrócona wartość -1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Następujące metody zawiera klauzulę WHERE w instrukcji SELECT. Ponieważ kod jest umożliwiają utworzenie dobrze dopasowanego pełną instrukcję SQL go należy uważać, aby wyprowadza zastrzeżonych znaków, takich jak cudzysłowu (') ciągi.

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

Metoda ExecuteReader zwraca obiekt SqliteDataReader. Oprócz metody odczytu, jak w przykładzie innych użytecznych właściwości obejmują:

-  **RowsAffected** — liczba wierszy dotyczy zapytanie.
-  **HasRows** — Określa, czy wszystkie wiersze zostały zwrócone.


### <a name="executescalar"></a>EXECUTESCALAR

Używane dla instrukcji SELECT, które zwraca pojedynczą wartość (takie jak zagregowanych).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Typ zwracany metody jest `object` — powinien Rzutuj wynik metody w zależności od zapytanie bazy danych. Wynik może być liczbą całkowitą z kwerendy liczba lub ciąg, w wyniku zapytania wybierz jedną kolumnę. Należy zauważyć, że to różni się od innych metod wykonania, które zwracają obiekt czytnika lub liczbę uwzględnionych wierszy.


## <a name="related-links"></a>Linki pokrewne

- [Dostęp Basic (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp zaawansowany (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisy na danych w systemie iOS](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Dostęp do danych z zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
