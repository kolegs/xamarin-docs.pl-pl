---
title: Dane systemowe w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano sposób używania dane systemowe i Mono.Data.Sqlite.dll dostępu do danych SQLite w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 183079c150ad4df05424d4dbf2980a307a889352
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997201"
---
# <a name="systemdata-in-xamarinios"></a>Dane systemowe w rozszerzeniu Xamarin.iOS

Xamarin.iOS 8.10 dodaje obsługę [System.Data](xref:System.Data), w tym `Mono.Data.Sqlite.dll` dostawcy ADO.NET. Obsługa obejmuje dodanie następujących [zestawy](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`

<a name="Example" />

## <a name="example"></a>Przykład

Następujący program tworzy bazę danych w `Documents/mydb.db3`, a jeśli baza danych wcześniej nie istnieje, jest wypełniona przykładowymi danymi. Baza danych jest następnie wysyłane zapytanie, za pomocą danych wyjściowych zapisywanych `stderr`.

### <a name="add-references"></a>Dodaj odwołania

Po pierwsze, kliknij prawym przyciskiem myszy **odwołania** węzeł i wybierz polecenie **Edytuj odwołania...**  polecenie `System.Data` i `Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "Dodawanie nowych odwołań")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>Przykładowy kod

Poniższy kod pokazuje prosty przykład tworzenia tabel i wstawianie wierszy przy użyciu osadzonych polecenia SQL:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;

class Demo {
    static void Main (string [] args)
    {
        var connection = GetConnection ();
        using (var cmd = connection.CreateCommand ()) {
            connection.Open ();
            cmd.CommandText = "SELECT * FROM People";
            using (var reader = cmd.ExecuteReader ()) {
                while (reader.Read ()) {
                    Console.Error.Write ("(Row ");
                    Write (reader, 0);
                    for (nint i = 1; i < reader.FieldCount; ++i) {
                        Console.Error.Write(" ");
                        Write (reader, i);
                    }
                    Console.Error.WriteLine(")");
                }
            }
            connection.Close ();
        }
    }

    static SqliteConnection GetConnection()
    {
        var documents = Environment.GetFolderPath (
                Environment.SpecialFolder.Personal);
        string db = Path.Combine (documents, "mydb.db3");
        bool exists = File.Exists (db);
        if (!exists)
            SqliteConnection.CreateFile (db);
        var conn = new SqliteConnection("Data Source=" + db);
        if (!exists) {
            var commands = new[] {
            "CREATE TABLE People (PersonID INTEGER NOT NULL, FirstName ntext, LastName ntext)",
            // WARNING: never insert user-entered data with embedded parameter values
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (1, 'First', 'Last')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (2, 'Dewey', 'Cheatem')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (3, 'And', 'How')",
            };
            conn.Open ();
            foreach (var cmd in commands) {
                using (var c = conn.CreateCommand()) {
                    c.CommandText = cmd;
                    c.CommandType = CommandType.Text;
                    c.ExecuteNonQuery ();
                }
            }
            conn.Close ();
        }
        return conn;
    }

    static void Write(SqliteDataReader reader, int index)
    {
        Console.Error.Write("({0} '{1}')",
                reader.GetName(index),
                reader [index]);
    }
}
```

> [!IMPORTANT]
> Jak wspomniano w powyższym przykładowym kodzie, jest złym zwyczajem osadzanie ciągów w poleceń SQL, ponieważ sprawia, że Twój kod narażony na ataki [wstrzyknięcie kodu SQL](http://en.wikipedia.org/wiki/SQL_injection).


### <a name="using-command-parameters"></a>Przy użyciu parametrów polecenia

Poniższy kod pokazuje jak wstawianie tekstu wprowadzanego przez użytkownika bezpiecznie bazy danych (nawet jeśli tekst zawiera znaki specjalne SQL, takich jak apostrof pojedynczego) za pomocą parametrów polecenia:

```csharp
// user input from Textbox control
var fname = fnameTextbox.Text;
var lname = lnameTextbox.Text;
// use command parameters to safely insert into database
using (var addCmd = conn.CreateCommand ()) {
    addCmd.CommandText = "INSERT INTO [People] (PersonID, FirstName, LastName) VALUES (@COL1, @COL2, @COL3)";
    addCmd.CommandType = System.Data.CommandType.Text;
    addCmd.AddParameterWithValue ("@COL1", 1);
    addCmd.AddParameterWithValue ("@COL2", fname);
    addCmd.AddParameterWithValue ("@COL3", lname);
    addCmd.ExecuteNonQuery ();
}
```

<a name="Missing_Functionality" />

## <a name="missing-functionality"></a>Brakujące funkcje

Zarówno **System.Data** i **Mono.Data.Sqlite** brakuje niektórych funkcji.

<a name="System.Data" />

### <a name="systemdata"></a>System.Data

Brak funkcji **System.Data.dll** obejmuje:

-  Cokolwiek wymagające [System.CodeDom](xref:System.CodeDom) (np.  [System.Data.TypedDataSetGenerator](xref:System.Data.TypedDataSetGenerator) )
-  (Np. plik konfiguracji XML pomocy technicznej  [System.Data.Common.DbProviderConfigurationHandler](xref:System.Data.Common.DbProviderConfigurationHandler) )
-   [System.Data.Common.DbProviderFactories](xref:System.Data.Common.DbProviderFactories) (w zależności od Obsługa plików konfiguracji XML)
-   [System.Data.OleDb](xref:System.Data.OleDb)
-   [System.Data.Odbc](xref:System.Data.Odbc)
-  `System.EnterpriseServices.dll` Został zależności *usunięte* z `System.Data.dll` , dając usunięcie [SqlConnection.EnlistDistributedTransaction(ITransaction)](xref:System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction*) metody.


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

W tym samym czasie **Mono.Data.Sqlite.dll** jego nie zmiany kodu źródłowego, ale zamiast tego może być hosta do liczby *środowiska uruchomieniowego* problemów od `Mono.Data.Sqlite.dll` wiąże 3.5 bazy danych SQLite. System iOS 8, w tym samym czasie jest dostarczany z bazy danych SQLite 3.8.5. Suffice nim, że pewne rzeczy uległy zmianie między dwoma wersjami.

Starsza wersja systemu IOS są dostarczane z następujących wersji oprogramowania SQLite:

- **System iOS 7** — wersja 3.7.13.
- **iOS 6** — wersja 3.7.13.
- **System iOS 5** — wersja 3.7.7.
- **iOS 4** — wersja 3.6.22.

Najbardziej typowe problemy z pojawiają się powiązania do zapytania schematu bazy danych, np. określanie w czasie wykonywania, który istnieje dla danej tabeli, takich jak kolumny `Mono.Data.Sqlite.SqliteConnection.GetSchema` (zastępowanie [DbConnection.GetSchema](xref:System.Data.Common.DbConnection.GetSchema) i `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` (zastępowanie [DbDataReader.GetSchemaTable](xref:System.Data.Common.DbDataReader.GetSchemaTable). Krótko mówiąc, wydaje się to wszystko za pomocą [DataTable](xref:System.Data.DataTable) prawdopodobnie nie będzie działać.

<a name="Data_Binding" />

## <a name="data-binding"></a>Powiązanie danych

Powiązanie danych nie jest obsługiwana w tej chwili.

