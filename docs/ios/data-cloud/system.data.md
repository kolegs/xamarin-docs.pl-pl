---
title: System.Data
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b141dfac49e2cfa2dc80b7c0e4ca3a93968590a6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="systemdata"></a>System.Data

Xamarin.iOS 8.10 dodaje obsługę [system.dane](https://developer.xamarin.com/api/namespace/System.Data/), takie jak `Mono.Data.Sqlite.dll` dostawcy ADO.NET. Obsługa obejmuje dodanie następujących [zestawy](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`


<a name="Example" />

## <a name="example"></a>Przykład

Następujący program tworzy bazę danych w `Documents/mydb.db3`, i jeśli bazy danych wcześniej nie istnieje on został wypełniony przykładowymi danymi. Następnie dotyczy kwerenda bazy danych, z danych wyjściowych zapisane `stderr`.

### <a name="add-references"></a>Dodawanie odwołań

Najpierw, kliknij prawym przyciskiem myszy **odwołania** węzeł i wybierz polecenie **odwołuje się do edycji...**  następnie wybierz `System.Data` i `Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "Dodawanie nowych odwołań")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>Przykładowy kod

Poniższy kod przedstawia prosty przykład tworzenia tabel i wstawianie wierszy osadzonych poleceń SQL:

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
> Jak wspomniano w powyższym przykładowym kodzie, osadzanie ciągów w poleceń SQL, ponieważ sprawia, że kod narażony na ataki zły rozwiązaniem jest [iniekcja kodu SQL](http://en.wikipedia.org/wiki/SQL_injection).


### <a name="using-command-parameters"></a>Przy użyciu parametrów polecenia

Poniższy kod przedstawia sposób użycia parametry polecenia, aby wstawić tekst użytkownik wprowadził bezpiecznie do bazy danych (nawet jeśli tekst zawiera znaki specjalne SQL, takich jak apostrof jednym):

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

## <a name="missing-functionality"></a>Brak funkcji

Zarówno **system.dane** i **Mono.Data.Sqlite** brakuje niektórych funkcji.

<a name="System.Data" />

### <a name="systemdata"></a>System.Data

Funkcje brakuje **System.Data.dll** obejmuje:

-  Cokolwiek wymagające [System.CodeDom](https://developer.xamarin.com/api/namespace/System.CodeDom/) (np.  [System.Data.TypedDataSetGenerator](https://developer.xamarin.com/api/type/System.Data.TypedDataSetGenerator/) )
-  Plik pomocy technicznej konfiguracji XML (np.  [System.Data.Common.DbProviderConfigurationHandler](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderConfigurationHandler/) )
-   [System.Data.Common.DbProviderFactories](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderFactories/) (zależy od Obsługa pliku konfiguracji XML)
-   [System.Data.OleDb](https://developer.xamarin.com/api/namespace/System.Data.OleDb/)
-   [System.Data.Odbc](https://developer.xamarin.com/api/namespace/System.Data.Odbc/)
-  `System.EnterpriseServices.dll` Zależnością było *usunięte* z `System.Data.dll` , co usunięcie [SqlConnection.EnlistDistributedTransaction(ITransaction)](https://developer.xamarin.com/api/member/System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction/(System.EnterpriseServices.ITransaction)) metody.


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

W tym samym czasie **Mono.Data.Sqlite.dll** odniesionej nie zmiany kodu źródłowego, ale może być zamiast tego hosta do liczby *środowiska uruchomieniowego* wystawia od `Mono.Data.Sqlite.dll` wiąże SQLite 3.5. System iOS 8, w tym samym czasie dostarczany z SQLite 3.8.5. Suffice nim, że niektóre elementy zostały zmienione między dwoma wersjami.

Starsza wersja systemu IOS są dostarczane z następujących wersji systemu SQLite:

- **System iOS 7** — wersja 3.7.13.
- **iOS 6** — wersja 3.7.13.
- **System iOS 5** — wersja 3.7.7.
- **iOS 4** — wersja 3.6.22.

Najbardziej typowe problemy ma dotyczyć zapytanie schematu bazy danych, np. określanie w czasie wykonywania, który istnieje w danej tabeli, takich jak kolumny `Mono.Data.Sqlite.SqliteConnection.GetSchema` (zastępowanie [DbConnection.GetSchema](https://developer.xamarin.com/api/member/System.Data.Common.DbConnection.GetSchema/)) i `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` () Zastępowanie [DbDataReader.GetSchemaTable](https://developer.xamarin.com/api/member/System.Data.Common.DbDataReader.GetSchemaTable/)). Krótko mówiąc, wydaje się, że cokolwiek przy użyciu [DataTable](https://developer.xamarin.com/api/type/System.Data.DataTable/) prawdopodobnie nie będzie działać.

<a name="Data_Binding" />

## <a name="data-binding"></a>Powiązanie danych

Powiązanie danych nie jest obsługiwana w tej chwili.

