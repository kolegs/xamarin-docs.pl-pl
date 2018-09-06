---
title: Bazy danych na platformie Xamarin.Mac
description: W tym artykule opisano, przy użyciu kodowania i obserwowania umożliwiający powiązanie danych między bazami danych SQLite i elementy interfejsu użytkownika w program Xcode Interface Builder pary klucz wartość pary klucz wartość. Obejmuje ona również przy użyciu Relacyjnego biblioteki SQLite.NET w celu zapewnienia dostępu do danych SQLite.
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: db418df0869d73e9f04982fb508fd261304240c0
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780579"
---
# <a name="databases-in-xamarinmac"></a>Bazy danych na platformie Xamarin.Mac

_W tym artykule opisano, przy użyciu kodowania i obserwowania umożliwiający powiązanie danych między bazami danych SQLite i elementy interfejsu użytkownika w program Xcode Interface Builder pary klucz wartość pary klucz wartość. Obejmuje ona również przy użyciu Relacyjnego biblioteki SQLite.NET w celu zapewnienia dostępu do danych SQLite._

## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tej samej bazy danych SQLite, które mogą uzyskiwać dostęp do aplikacji platformy Xamarin.iOS lub Xamarin.Android.

W tym artykule firma Microsoft będzie obejmował dwa sposoby uzyskania dostępu do danych SQLite:

1. **Bezpośredni dostęp do** —, uzyskując bezpośrednio dostęp do bazy danych SQLite, zostaną użyte dane z bazy danych dla pary klucz wartość kodowania i powiązanie danych za pomocą elementów interfejsu użytkownika utworzony w program Xcode Interface Builder. Przy użyciu kodowania i powiązania danych technik w aplikacji platformy Xamarin.Mac pary klucz wartość, może znacznie skrócić ilość kodu, który trzeba napisać i obsługiwać do wypełniania i pracować z elementami interfejsu użytkownika. Masz także zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) z usługi frontonu zakończenia interfejsu użytkownika (_Model-View-Controller_), prowadzącego do łatwiejsza Obsługa bardziej elastycznych aplikacji Projekt.
2. **Relacyjnego biblioteki SQLite.NET** — za pomocą "open source" [biblioteki SQLite.NET](http://www.sqlite.org) obiektu relacji Menedżera (ORM), firma Microsoft może znacznie zmniejszyć ilość kodu wymaganą do odczytywania i zapisywania danych z bazy danych SQLite. Następnie można tych danych do wypełnienia elementu interfejsu użytkownika, takie jak widok tabeli.

[![Przykładem uruchomionej aplikacji](databases-images/intro01.png "przykładem uruchomionej aplikacji")](databases-images/intro01-large.png#lightbox)

W tym artykule omówiono podstawowe informacje dotyczące pracy z kodowanie klucz wartość i powiązanie danych z bazy danych SQLite w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Ponieważ firma Microsoft użyje kodowanie klucz wartość i powiązania danych, skontaktuj się za pośrednictwem [powiązanie danych i kodowanie klucz wartość](~/mac/app-fundamentals/databinding.md) najpierw jako podstawowe techniki i pojęcia zostały omówione który będzie używany w tej dokumentacji i jego próbki aplikacja.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` atrybutów Umożliwia podłączanie klas języka C# do języka Objective-C obiektów i interfejsu użytkownika elementów.

## <a name="direct-sqlite-access"></a>Bezpośredni dostęp do bazy danych SQLite

Danych SQLite, który ma być powiązane elementy interfejsu użytkownika w program Xcode Interface Builder zdecydowanie zalecane jest dostępu bazy danych SQLite bezpośrednio (zamiast przy użyciu technik, takich jak ORM), ponieważ mają całkowitą kontrolę nad sposobem dane są zapisywane i odczytu  z bazy danych.

Jak zauważono w [powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md) dokumentacji, przy użyciu kodowania pary klucz wartość i powiązania danych technik w aplikacji platformy Xamarin.Mac, użytkownik może znacznie skrócić ilość kodu, który trzeba napisać i Obsługa do wypełniania i pracować z elementami interfejsu użytkownika. W połączeniu z bezpośredniego dostępu do bazy danych SQLite, znacznie zmniejsza ilość kodu wymaganą do odczytu i zapisu danych do tej bazy danych.

W tym artykule firma Microsoft będzie modyfikować przykładową aplikację z powiązanie danych oraz kodowania dokumentu pary klucz wartość, do użytku jako źródło zapasowy powiązania bazy danych SQLite.

### <a name="including-sqlite-database-support"></a>W tym obsługi bazy danych SQLite

Zanim będzie można kontynuować, musimy dodać obsługi bazy danych SQLite do naszej aplikacji, umieszczając odwołania do kilku. Pliki biblioteki dll.

Wykonaj następujące czynności:

1. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **odwołania** i wybierz polecenie **Edytuj odwołania**.
2. Wybierz obie **Mono.Data.Sqlite** i **System.Data** zestawów: 

    [![Dodawanie odwołań wymagane](databases-images/reference01.png "Dodawanie odwołań wymagane")](databases-images/reference01-large.png#lightbox)
3. Kliknij przycisk **OK** przycisk, aby zapisać zmiany i dodaj odwołania.

### <a name="modifying-the-data-model"></a>Modyfikowanie modelu danych

Teraz, Dodaliśmy obsługę bezpośredni dostęp do bazy danych SQLite do naszej aplikacji, należy zmodyfikować naszych obiekt modelu danych, aby odczytywać i zapisywać dane z bazy danych (a także zapewniają kodowanie klucz wartość i powiązania danych). W przypadku naszej przykładowej aplikacji będzie będziemy edytować **PersonModel.cs** klasy i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _ID = "";
        private string _managerID = "";
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        private SqliteConnection _conn = null;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        [Export("ID")]
        public string ID {
            get { return _ID; }
            set {
                WillChangeValue ("ID");
                _ID = value;
                DidChangeValue ("ID");
            }
        }

        [Export("ManagerID")]
        public string ManagerID {
            get { return _managerID; }
            set {
                WillChangeValue ("ManagerID");
                _managerID = value;
                DidChangeValue ("ManagerID");
            }
        }

        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }

        public PersonModel (string id, string name, string occupation)
        {
            // Initialize
            this.ID = id;
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (SqliteConnection conn, string id)
        {
            // Load from database
            Load (conn, id);
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion

        #region SQLite Routines
        public void Create(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Create new record ID?
            if (ID == "") {
                ID = Guid.NewGuid ().ToString();
            }

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Save manager ID and create the sub record
                Person.ManagerID = ID;
                Person.Create (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Update(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Update sub record
                Person.Update (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Load(SqliteConnection conn, string id) {
            bool shouldClose = false;

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Is the database already open?
            if (conn.State != ConnectionState.Open) {
                shouldClose = true;
                conn.Open ();
            }

            // Execute query
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", id);

                using (var reader = command.ExecuteReader ()) {
                    while (reader.Read ()) {
                        // Pull values back into class
                        ID = (string)reader [0];
                        Name = (string)reader [1];
                        Occupation = (string)reader [2];
                        isManager = (bool)reader [3];
                        ManagerID = (string)reader [4];
                    }
                }
            }

            // Is this a manager?
            if (isManager) {
                // Yes, load children
                using (var command = conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

                    // Populate with data from the record
                    command.Parameters.AddWithValue ("@COL1", id);

                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Load child and add to collection
                            var childID = (string)reader [0];
                            var person = new PersonModel (conn, childID);
                            _people.Add (person);
                        }
                    }
                }
            }

            // Should we close the connection to the database
            if (shouldClose) {
                conn.Close ();
            }

            // Save last connection
            _conn = conn;
        }

        public void Delete(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Empty class
            ID = "";
            ManagerID = "";
            Name = "";
            Occupation = "";
            isManager = false;
            _people = new NSMutableArray();

            // Save last connection
            _conn = conn;
        }
        #endregion
    }
}
```

Spójrzmy na modyfikacje szczegółowo poniżej.

Po pierwsze, dodaliśmy kilka przy użyciu instrukcji, które są wymagane do używania bazy danych SQLite i dodaliśmy zmiennej, aby zapisać naszych ostatniego połączenia do bazy danych SQLite:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

Użyjemy tego połączenia, aby automatycznie zapisywać zmiany rekordów do bazy danych, gdy użytkownik modyfikuje zawartość w Interfejsie użytkownika za pomocą powiązania danych:

```csharp
[Export("Name")]
public string Name {
    get { return _name; }
    set {
        WillChangeValue ("Name");
        _name = value;
        DidChangeValue ("Name");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("Occupation")]
public string Occupation {
    get { return _occupation; }
    set {
        WillChangeValue ("Occupation");
        _occupation = value;
        DidChangeValue ("Occupation");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}
```

Wszelkie zmiany wprowadzone do **nazwa**, **Dziadka** lub **isManager** właściwości będą wysyłane do bazy danych, jeśli istnieją zapisane dane przed (np. Jeśli `_conn` Zmienna nie jest `null`). Następnie Przyjrzyjmy się metody, które dodaliśmy **Utwórz**, **aktualizacji**, **obciążenia** i **Usuń** osób z bazy danych.

#### <a name="create-a-new-record"></a>Utwórz nowy rekord

Następujący kod został dodany, aby utworzyć nowy rekord w bazie danych SQLite:

```csharp
public void Create(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Create new record ID?
    if (ID == "") {
        ID = Guid.NewGuid ().ToString();
    }

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Save manager ID and create the sub record
        Person.ManagerID = ID;
        Person.Create (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Używamy `SQLiteCommand` Aby utworzyć nowy rekord w bazie danych. Otrzymujemy nowe polecenie z `SQLiteConnection` (conn), firma Microsoft przekazana do metody, wywołując `CreateCommand`. Następnie ustawiamy instrukcji SQL, które faktycznie zapisać nowy rekord podanie parametrów na rzeczywiste wartości:

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

Później możemy ustawić wartości dla parametrów przy użyciu `Parameters.AddWithValue` metody `SQLiteCommand`. Przy użyciu parametrów, Upewniamy się, że wartości (takich jak pojedynczy cudzysłów) Pobierz poprawnie zaszyfrowana przed wysłaniem do bazy danych SQLite. Przykład:

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

Na koniec, ponieważ osoby mogą być menedżera i kolekcji pracowników w ramach nich, jesteśmy rekursywnie wywoływania `Create` metody w tym osobom, aby zapisać je w bazie danych oraz:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Save manager ID and create the sub record
    Person.ManagerID = ID;
    Person.Create (conn);
}
```

#### <a name="updating-a-record"></a>Trwa aktualizowanie rekordu

Następujący kod został dodany do aktualizacji istniejący rekord w bazie danych SQLite:

```csharp
public void Update(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Update sub record
        Person.Update (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Podobnie jak **Utwórz** nowszym, uzyskujemy `SQLiteCommand` z przekazanych w `SQLiteConnection`i ustaw SQL aktualizację naszej rekordu (podanie parametrów):

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

Firma Microsoft Podaj wartości parametrów (przykład: `command.Parameters.AddWithValue ("@COL1", ID);`) i ponownie rekursywnie wywoływania funkcji update na wszystkie podrzędne rekordy:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Update sub record
    Person.Update (conn);
}
```
#### <a name="loading-a-record"></a>Trwa ładowanie rekordu

Następujący kod został dodany do załadowania istniejącego rekordu z bazy danych SQLite:

```csharp
public void Load(SqliteConnection conn, string id) {
    bool shouldClose = false;

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Is the database already open?
    if (conn.State != ConnectionState.Open) {
        shouldClose = true;
        conn.Open ();
    }

    // Execute query
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Pull values back into class
                ID = (string)reader [0];
                Name = (string)reader [1];
                Occupation = (string)reader [2];
                isManager = (bool)reader [3];
                ManagerID = (string)reader [4];
            }
        }
    }

    // Is this a manager?
    if (isManager) {
        // Yes, load children
        using (var command = conn.CreateCommand ()) {
            // Create new command
            command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

            // Populate with data from the record
            command.Parameters.AddWithValue ("@COL1", id);

            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Load child and add to collection
                    var childID = (string)reader [0];
                    var person = new PersonModel (conn, childID);
                    _people.Add (person);
                }
            }
        }
    }

    // Should we close the connection to the database
    if (shouldClose) {
        conn.Close ();
    }

    // Save last connection
    _conn = conn;
}
```

Ponieważ procedura może być wywoływana z obiektu nadrzędnego (na przykład obiekt menedżera ładowania obiektu swoich pracowników) cyklicznie, specjalnego kodu została dodana do obsługi otwierające i zamykające połączenia z bazą danych:

```csharp
bool shouldClose = false;
...

// Is the database already open?
if (conn.State != ConnectionState.Open) {
    shouldClose = true;
    conn.Open ();
}
...

// Should we close the connection to the database
if (shouldClose) {
    conn.Close ();
}

```

Jak zawsze możemy ustawić SQL do pobrania rekordu i korzystanie z parametrów:

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

Na koniec używamy czytnik danych do wykonywania zapytania i zwrócić pól rekordu (które firma Microsoft skopiować do wystąpienia programu `PersonModel` klasy):

```csharp
using (var reader = command.ExecuteReader ()) {
    while (reader.Read ()) {
        // Pull values back into class
        ID = (string)reader [0];
        Name = (string)reader [1];
        Occupation = (string)reader [2];
        isManager = (bool)reader [3];
        ManagerID = (string)reader [4];
    }
}
```

Jeśli ta osoba menedżera, musimy również załadować wszystkich pracowników (ponownie, za pośrednictwem rekursywnie wywołania ich `Load` metoda):

```csharp
// Is this a manager?
if (isManager) {
    // Yes, load children
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Load child and add to collection
                var childID = (string)reader [0];
                var person = new PersonModel (conn, childID);
                _people.Add (person);
            }
        }
    }
}
```

#### <a name="deleting-a-record"></a>Usuwanie rekordu

Następujący kod został dodany do usunięcia istniejącego rekordu z bazy danych SQLite:

```csharp
public void Delete(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Empty class
    ID = "";
    ManagerID = "";
    Name = "";
    Occupation = "";
    isManager = false;
    _people = new NSMutableArray();

    // Save last connection
    _conn = conn;
}
```

W tym miejscu udostępniamy SQL można usunąć rekordu menedżerów i rekordy wszelkie pracowników w ramach tego menedżera (przy użyciu parametrów):

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

Po usunięciu rekordu usuwamy zaznaczenie się bieżące wystąpienie `PersonModel` klasy:

```csharp
// Empty class
ID = "";
ManagerID = "";
Name = "";
Occupation = "";
isManager = false;
_people = new NSMutableArray();
```

### <a name="initializing-the-database"></a>Inicjowanie bazy danych

Zmian w naszym modelu danych w miejscu do obsługi odczytu i zapisu do bazy danych należy otworzyć połączenia z bazą danych i zainicjuj ją przy pierwszym uruchomieniu. Możemy dodać następujący kod do naszych **MainWindow.cs** pliku:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection DatabaseConnection = null;
...

private SqliteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "People.db3");

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);
    if (!exists)
        SqliteConnection.CreateFile (db);

    // Create connection to the database
    var conn = new SqliteConnection("Data Source=" + db);

    // Set the structure of the database
    if (!exists) {
        var commands = new[] {
            "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
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

        // Build list of employees
        var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
        Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
        Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
        Craig.Create (conn);

        var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
        Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
        Larry.Create (conn);
    }

    // Return new connection
    return conn;
}
```

Przyjrzyjmy się bliżej w powyższym kodzie. Najpierw wybierz mamy lokalizację dla nowej bazy danych (w tym przykładzie pulpit użytkownika), sprawdzić, czy baza danych istnieje, a w przeciwnym razie utwórz go:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

Następnie możemy nawiązać połączenia z bazą danych przy użyciu ścieżki, którą utworzyliśmy powyżej:

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

Następnie utworzymy tabel SQL w bazie danych, w których firma Microsoft wymaga:

```csharp
var commands = new[] {
    "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
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
```

Na koniec używamy nasz Model danych (`PersonModel`) do utworzenia domyślnego zestawu rekordów po raz pierwszy bazy danych aplikacja jest uruchomiona lub jeśli brakuje bazy danych:

```csharp
// Build list of employees
var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
Craig.Create (conn);

var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
Larry.Create (conn);
``` 

Po uruchomieniu aplikacji zostanie otwarte okno główne, firma Microsoft nawiązania połączenia z bazą danych przy użyciu kodu, które dodaliśmy powyżej:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>Trwa ładowanie powiązanych danych

Wszystkie komponenty uzyskać bezpośredni dostęp do powiązanych danych z bazy danych SQLite w miejscu możemy załadować dane w różnych widoków, które zawiera nasza aplikacja i jego automatycznie pojawi się w naszym interfejsie użytkownika.

#### <a name="loading-a-single-record"></a>Ładowanie z pojedynczym rekordem

Aby załadować pojedynczy rekord, w którym identyfikator jest znana, możemy użyć następującego kodu:

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>Trwa ładowanie wszystkich rekordów

Aby załadować wszystkie osoby, niezależnie od tego, jeśli są one menedżera, czy nie, użyj poniższego kodu:

```csharp
// Load all employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People]";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();

```

W tym miejscu użyjemy przeciążenia konstruktora dla `PersonModel` klasy, aby załadować osoby do pamięci:

```csharp
var person = new PersonModel (_conn, childID);
```

Również dzwonimy klasy powiązania danych, aby dodać osoby do naszych zbiór osób `AddPerson (person)`, gwarantuje to, że naszego interfejsu użytkownika rozpoznaje zmiany i wyświetla go:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>Trwa ładowanie tylko rekordy najwyższego poziomu

Aby załadować tylko menedżerowie (na przykład, aby wyświetlić dane w widoku konspektu), firma Microsoft Użyj poniższego kodu:

```csharp
// Load only managers employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();
```

Tylko rzeczywistego różnica w instrukcji SQL (które ładuje tylko menedżerowie `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`), ale działa tak samo jak w powyższej sekcji, w przeciwnym razie.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>Bazy danych i polach kombi

Formanty Menu dostępne dla systemu macOS (np. pola kombi) można ustawić do wypełnienia listy rozwijanej opcję z listy wewnętrznych, (które mogą być wstępnie zdefiniowane w programu Interface Builder lub wypełnione przy użyciu kodu) lub podając źródła danych niestandardowych, zewnętrznych. Zobacz [dostarczanie danych formantu Menu](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) Aby uzyskać więcej informacji.

Na przykład edytować proste powiązanie powyższego przykładu w programu Interface Builder dodać pole kombi i udostępnić ją przy użyciu źródła o nazwie `EmployeeSelector`:

[![Udostępnianie ujścia pole kombi](databases-images/combo01.png "udostępnianie ujścia pola kombi")](databases-images/combo01-large.png#lightbox)

W **Inspektor atrybuty**, sprawdź **uzupełnić** i **źródło danych używa** właściwości:

![Konfigurowanie atrybutów pola kombi](databases-images/combo02.png "Konfigurowanie atrybutów pola kombi")

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji.

#### <a name="providing-combobox-data"></a>Dostarcza dane combobox

Następnie dodaj nową klasę do projektu o nazwie `ComboBoxDataSource` i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public class ComboBoxDataSource : NSComboBoxDataSource
    {
        #region Private Variables
        private SqliteConnection _conn = null;
        private string _tableName = "";
        private string _IDField = "ID";
        private string _displayField = "";
        private nint _recordCount = 0;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        public string TableName {
            get { return _tableName; }
            set { 
                _tableName = value;
                _recordCount = GetRecordCount ();
            }
        }

        public string IDField {
            get { return _IDField; }
            set {
                _IDField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public string DisplayField {
            get { return _displayField; }
            set { 
                _displayField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public nint RecordCount {
            get { return _recordCount; }
        }
        #endregion

        #region Constructors
        public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.DisplayField = displayField;
        }

        public ComboBoxDataSource (SqliteConnection conn, string tableName, string idField, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.IDField = idField;
            this.DisplayField = displayField;
        }
        #endregion

        #region Private Methods
        private nint GetRecordCount ()
        {
            bool shouldClose = false;
            nint count = 0;

            // Has a Table, ID and display field been specified?
            if (TableName !="" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read count from query
                            var result = (long)reader [0];
                            count = (nint)result;
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return the number of records
            return count;
        }
        #endregion

        #region Public Methods
        public string IDForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string ValueForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string IDForValue (string value)
        {
            NSString result = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", value);

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            result = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return result;
        }
        #endregion 

        #region Override Methods
        public override nint ItemCount (NSComboBox comboBox)
        {
            return RecordCount;
        }

        public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public override nint IndexOfItem (NSComboBox comboBox, string value)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";
            nint index = NSRange.NotFound;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read () && !found) {
                            // Read the display field from the query
                            field = (string)reader [0];
                            ++index;

                            // Is this the value we are searching for?
                            if (value == field) {
                                // Yes, exit loop
                                found = true;
                            }
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return index;
        }

        public override string CompletedString (NSComboBox comboBox, string uncompletedString)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Escape search string
                uncompletedString = uncompletedString.Replace ("'", "");

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            field = (string)reader [0];
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return field;
        }
        #endregion
    }
}
```

W tym przykładzie tworzymy nową `NSComboBoxDataSource` przekazujących elementy pola kombi z dowolnego źródła danych bazy danych SQLite. Najpierw należy zdefiniować następujące właściwości:

- `Conn` -Pobiera lub ustawia połączenie z bazą danych SQLite.
- `TableName` -Pobiera lub ustawia nazwę tabeli.
- `IDField` -Pobiera lub ustawia pole, które zawiera unikatowy identyfikator dla danej tabeli. Wartość domyślna to `ID`.
- `DisplayField` -Pobiera lub ustawia pole, które jest wyświetlane na liście rozwijanej.
- `RecordCount` -Pobiera liczbę rekordów w danej tabeli.

Podczas tworzenia nowego wystąpienia obiektu, przekazanie połączenia, nazwę tabeli, opcjonalnie pole Identyfikatora i wyświetlanego pola:

```csharp
public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
{
    // Initialize
    this.Conn = conn;
    this.TableName = tableName;
    this.DisplayField = displayField;
}
```

`GetRecordCount` Metoda zwraca liczbę rekordów w danej tabeli:

```csharp
private nint GetRecordCount ()
{
    bool shouldClose = false;
    nint count = 0;

    // Has a Table, ID and display field been specified?
    if (TableName !="" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read count from query
                    var result = (long)reader [0];
                    count = (nint)result;
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return the number of records
    return count;
}
```

Jest to ilekroć `TableName`, `IDField` lub `DisplayField` wartość właściwości została zmieniona.

`IDForIndex` Metoda zwraca Unikatowy identyfikator (`IDField`) dla rekordu w indeksie elementu listy danej listy rozwijanej: 

```csharp
public string IDForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

`ValueForIndex` Metoda zwraca wartość (`DisplayField`) dla elementu o indeksie listy danej listy rozwijanej:

```csharp
public string ValueForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

`IDForValue` Metoda zwraca Unikatowy identyfikator (`IDField`) dla danej wartości (`DisplayField`):

```csharp
public string IDForValue (string value)
{
    NSString result = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", value);

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    result = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return result;
}
```

`ItemCount` Zwraca wstępnie obliczonych liczbę elementów na liście, gdy obliczona `TableName`, `IDField` lub `DisplayField` zostaną zmienione właściwości:

```csharp
public override nint ItemCount (NSComboBox comboBox)
{
    return RecordCount;
}
```

`ObjectValueForItem` Metody zawiera wartość (`DisplayField`) dla danej listy rozwijanej indeks elementu listy:

```csharp
public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

Należy zauważyć, że użyto `LIMIT` i `OFFSET` instrukcji w naszych polecenia bazy danych SQLite, ograniczenie do jednego rekordu, firma Microsoft potrzebne.

`IndexOfItem` Metoda zwraca indeks elementu listy rozwijanej wartości (`DisplayField`) danego:

```csharp
public override nint IndexOfItem (NSComboBox comboBox, string value)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";
    nint index = NSRange.NotFound;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read () && !found) {
                    // Read the display field from the query
                    field = (string)reader [0];
                    ++index;

                    // Is this the value we are searching for?
                    if (value == field) {
                        // Yes, exit loop
                        found = true;
                    }
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return index;
}
```

Jeśli wartość nie zostanie znaleziony, `NSRange.NotFound` wartość jest zwracany, a wszystkie elementy są nie na liście rozwijanej.

`CompletedString` Metoda zwraca pierwszą wartość dopasowania (`DisplayField`) dla częściowo wpisane wpisu:

```csharp
public override string CompletedString (NSComboBox comboBox, string uncompletedString)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Escape search string
        uncompletedString = uncompletedString.Replace ("'", "");

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    field = (string)reader [0];
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return field;
}
```

#### <a name="displaying-data-and-responding-to-events"></a>Wyświetlanie danych i reagowanie na zdarzenia

Aby wyświetlić wszystkie elementy są ze sobą, Edytuj `SubviewSimpleBindingController` i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public partial class SubviewSimpleBindingController : AppKit.NSViewController
    {
        #region Private Variables
        private PersonModel _person = new PersonModel();
        private SqliteConnection Conn;
        #endregion

        #region Computed Properties
        //strongly typed view accessor
        public new SubviewSimpleBinding View {
            get {
                return (SubviewSimpleBinding)base.View;
            }
        }

        [Export("Person")]
        public PersonModel Person {
            get {return _person; }
            set {
                WillChangeValue ("Person");
                _person = value;
                DidChangeValue ("Person");
            }
        }

        public ComboBoxDataSource DataSource {
            get { return EmployeeSelector.DataSource as ComboBoxDataSource; }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public SubviewSimpleBindingController (IntPtr handle) : base (handle)
        {
            Initialize ();
        }

        // Called when created directly from a XIB file
        [Export ("initWithCoder:")]
        public SubviewSimpleBindingController (NSCoder coder) : base (coder)
        {
            Initialize ();
        }

        // Call to load from the XIB/NIB file
        public SubviewSimpleBindingController (SqliteConnection conn) : base ("SubviewSimpleBinding", NSBundle.MainBundle)
        {
            // Initialize
            this.Conn = conn;
            Initialize ();
        }

        // Shared initialization code
        void Initialize ()
        {
        }
        #endregion

        #region Private Methods
        private void LoadSelectedPerson (string id)
        {

            // Found?
            if (id != "") {
                // Yes, load requested record
                Person = new PersonModel (Conn, id);
            }
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Configure Employee selector dropdown
            EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");

            // Wireup events
            EmployeeSelector.Changed += (sender, e) => {
                // Get ID
                var id = DataSource.IDForValue (EmployeeSelector.StringValue);
                LoadSelectedPerson (id);
            };

            EmployeeSelector.SelectionChanged += (sender, e) => {
                // Get ID
                var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
                LoadSelectedPerson (id);
            };

            // Auto select the first person
            EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
            Person = new PersonModel (Conn, DataSource.IDForIndex(0));
    
        }
        #endregion
    }
}
```

`DataSource` Właściwość zapewnia skrót do `ComboBoxDataSource` (utworzonego powyżej) dołączone do pola kombi.

`LoadSelectedPerson` Metoda ładuje osoby z bazy danych dla danego Unikatowy identyfikator:

```csharp
private void LoadSelectedPerson (string id)
{

    // Found?
    if (id != "") {
        // Yes, load requested record
        Person = new PersonModel (Conn, id);
    }
}
```

W `AwakeFromNib` zastąpienie metody najpierw dołączyć wystąpienia źródła danych niestandardowe pole kombi:

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

Następnie pod uwagę brane są użytkownikowi edytowanie wartości tekstowej pola kombi, wyszukując skojarzony Unikatowy identyfikator (`IDField`) danych prezentowania i ładowanie danej osoby, jeśli znaleziono:

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

Możemy również obciążenia nową osobę, jeśli użytkownik wybierze nowy element z listy rozwijanej:

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

Ponadto firma Microsoft automatycznie wypełniać pola kombi i wyświetlane osoba mająca pierwszy element na liście:

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>SQLite.NET ORM

Jak wspomniano powyżej, za pomocą "open source" [biblioteki SQLite.NET](http://www.sqlite.org) obiektu relacji Menedżera (ORM), firma Microsoft może znacznie zmniejszyć ilość kodu wymaganą do odczytywania i zapisywania danych z bazy danych SQLite. To może nie być najlepszą trasę do wykonania podczas wiązania danych ze względu na kilka wymagań które kodowanie klucz wartość i powiązania danych należy umieścić na obiekcie.

Zgodnie z witryny sieci Web biblioteki SQLite.Net _"SQLite jest biblioteka oprogramowania, który implementuje niezależna, bez użycia serwera, niewymagającą konfiguracji, transakcyjnych aparatu bazy danych SQL. Bazy danych SQLite jest aparat najczęściej wdrożonej bazy danych na całym świecie. Kod źródłowy dla bazy danych SQLite jest własnością publiczną."_

W poniższych sekcjach pokażemy sposób użycia biblioteki SQLite.Net dostarczający dane dla widoku tabeli.

### <a name="including-the-sqlitenet-nuget"></a>W tym NuGet biblioteki SQLite.net

Biblioteki SQLite.NET jest przedstawiany jako pakiet NuGet, który można umieścić w aplikacji. Zanim dodamy obsługi bazy danych przy użyciu biblioteki SQLite.NET, musimy uwzględnić tego pakietu.

Wykonaj następujące polecenie, aby dodać pakiet:

1. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **pakietów** i wybierz polecenie **Dodawanie pakietów...**
2. Wprowadź `SQLite.net` w **pole wyszukiwania** i wybierz **sqlite net** wpis:

    [![Dodanie pakietu SQLite NuGet](databases-images/nuget01.png "dodanie pakietu SQLite NuGet")](databases-images/nuget01-large.png#lightbox)
3. Kliknij przycisk **Dodaj pakiet** przycisk, aby zakończyć.

### <a name="creating-the-data-model"></a>Tworzenie modelu danych

Dodajmy nową klasę do projektu i wywołania `OccupationModel`. Następnie możemy edytować **OccupationModel.cs** plików i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using SQLite;

namespace MacDatabase
{
    public class OccupationModel
    {
        #region Computed Properties
        [PrimaryKey, AutoIncrement]
        public int ID { get; set; }

        public string Name { get; set;}
        public string Description { get; set;}
        #endregion

        #region Constructors
        public OccupationModel ()
        {
        }

        public OccupationModel (string name, string description)
        {

            // Initialize
            this.Name = name;
            this.Description = description;

        }
        #endregion
    }
}
```

Po pierwsze, dołączamy biblioteki SQLite.NET (`using Sqlite`), następnie uwidaczniamy kilka właściwości, które będą zapisywane w bazie danych po zapisaniu tego rekordu. Pierwszą właściwością możemy wprowadzić jako klucz podstawowy i wybierz opcję automatycznego przyrostu w następujący sposób:

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```
### <a name="initializing-the-database"></a>Inicjowanie bazy danych

Zmian w naszym modelu danych w miejscu do obsługi odczytu i zapisu do bazy danych należy otworzyć połączenia z bazą danych i zainicjuj ją przy pierwszym uruchomieniu. Teraz Dodaj następujący kod:

```csharp
using SQLite;
...

public SQLiteConnection Conn { get; set; }
...

private SQLiteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "Occupation.db3");
    OccupationModel Occupation;

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);

    // Create connection to database
    var conn = new SQLiteConnection (db);

    // Initially populate table?
    if (!exists) {
        // Yes, build table
        conn.CreateTable<OccupationModel> ();

        // Add occupations
        Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
        conn.Insert (Occupation);
    }

    return conn;
}
```

Najpierw możemy pobrać ścieżki do bazy danych (pulpitu użytkownika, w tym przypadku) i sprawdzić, czy baza danych już istnieje:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

Następnie możemy nawiązać połączenia z bazą danych w ścieżce utworzonego powyżej:

```csharp
var conn = new SQLiteConnection (db);
```

Na koniec utworzymy tabelę i dodać niektóre domyślne rekordy:

```csharp
// Yes, build table
conn.CreateTable<OccupationModel> ();

// Add occupations
Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
conn.Insert (Occupation);
```

### <a name="adding-a-table-view"></a>Dodawanie widoku tabeli

Jako przykład użycia widoku tabeli zostanie dodany do naszego interfejsu użytkownika w program Xcode Interface builder. Uwidaczniamy będzie ten widok tabeli za pośrednictwem gniazda (`OccupationTable`), dzięki czemu możemy do niego dostęp przy użyciu kodu C#:

[![Udostępnianie ujścia widok tabeli](databases-images/table01.png "udostępnianie ujścia widok tabeli")](databases-images/table01-large.png#lightbox)

Następnie dodamy niestandardowych klas, aby wypełnić tę tabelę z danymi z bazy danych biblioteki SQLite.NET.

### <a name="creating-the-table-data-source"></a>Tworzenie tabeli źródła danych

Utwórz niestandardowe źródło danych do dostarczania danych do tabeli. Najpierw dodaj nową klasę o nazwie `TableORMDatasource` i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDatasource : NSTableViewDataSource
    {
        #region Computed Properties
        public List<OccupationModel> Occupations { get; set;} = new List<OccupationModel>();
        public SQLiteConnection Conn { get; set; }
        #endregion

        #region Constructors
        public TableORMDatasource (SQLiteConnection conn)
        {
            // Initialize
            this.Conn = conn;
            LoadOccupations ();
        }
        #endregion

        #region Public Methods
        public void LoadOccupations() {

            // Get occupations from database
            var query = Conn.Table<OccupationModel> ();

            // Copy into table collection
            Occupations.Clear ();
            foreach (OccupationModel occupation in query) {
                Occupations.Add (occupation);
            }

        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Occupations.Count;
        }
        #endregion
    }
}
```

Podczas tworzenia wystąpienia tej klasy później, firma Microsoft będzie przekazywane w naszych otwartego połączenia bazy danych biblioteki SQLite.NET. `LoadOccupations` Metody zapytania bazy danych i znaleziono rekordy są kopiowane do pamięci (przy użyciu naszego `OccupationModel` modelu danych).

### <a name="creating-the-table-delegate"></a>Utworzenie obiektu delegowanego tabeli

Ostatnią klasę, potrzebne jest niestandardowego delegata tabeli, aby wyświetlić informacje, które możemy zostały załadowane z bazy danych biblioteki SQLite.NET. Dodajmy nową `TableORMDelegate` do naszego projektu i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDelegate : NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "OccCell";
        #endregion

        #region Private Variables
        private TableORMDatasource DataSource;
        #endregion

        #region Constructors
        public TableORMDelegate (TableORMDatasource dataSource)
        {
            // Initialize
            this.DataSource = dataSource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Occupation":
                view.StringValue = DataSource.Occupations [(int)row].Name;
                break;
            case "Description":
                view.StringValue = DataSource.Occupations [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

W tym miejscu użyjemy źródła danych `Occupations` kolekcji (która wczytany z bazy danych biblioteki SQLite.NET) do wypełnienia w kolumnach tabeli za pomocą `GetViewForItem` zastąpienie metody.

### <a name="populating-the-table"></a>Wypełnianie tabeli

W przypadku wszystkich elementów w miejscu, Przyjrzyjmy wypełniania tabeli po jej zwiększony z pliku .pliki, poprzez zastąpienie `AwakeFromNib` metody i udostępnianie ich wyglądać podobnie do następującego:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get database connection
    Conn = GetDatabaseConnection ();

    // Create the Occupation Table Data Source and populate it
    var DataSource = new TableORMDatasource (Conn);

    // Populate the Product Table
    OccupationTable.DataSource = DataSource;
    OccupationTable.Delegate = new TableORMDelegate (DataSource);
}
```

Po pierwsze możemy uzyskać dostęp do naszej biblioteki SQLite.NET bazie danych, tworzenie i wypełnianie jej, jeśli jeszcze nie istnieje. Następnie utworzymy nowe wystąpienie z naszych niestandardowe źródło danych tabeli, połączenie z bazą danych i w dołączamy do tabeli. Na koniec możemy utworzenia nowego wystąpienia naszej niestandardową klasę delegatów tabeli, przekazać źródła danych i dołączyć go do tabeli.

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z powiązania danych i kodowanie klucz wartość z bazy danych SQLite, w aplikacji platformy Xamarin.Mac. Po pierwsze znaleziono w znajdowaniu klasy C# do języka Objective-C przy użyciu kodowanie klucz wartość (KVC) i klucz wartość obserwowania (KVO). Następnie pokazano, jak używać klasy zgodne KVO i powiązanie danych go do elementów interfejsu użytkownika w program Xcode Interface Builder. Artykuł objętych usługą, Praca z SQLite dane za pośrednictwem Relacyjnego biblioteki SQLite.NET i wyświetlanie danych w widoku tabeli.



## <a name="related-links"></a>Linki pokrewne

- [MacDatabase (przykład)](https://developer.xamarin.com/samples/mac/MacDatabase/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Powiązanie danych i kodowanie klucz wartość](~/mac/app-fundamentals/databinding.md)
- [Kontrolki standardowe](~/mac/user-interface/standard-controls.md)
- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Widoki konspektu](~/mac/user-interface/outline-view.md)
- [Widoki kolekcji](~/mac/user-interface/collection-view.md)
- [Pary klucz wartość, kodowania, przewodnik programowania](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Wprowadzenie do powiązania cocoa dla tematów związanych z programowaniem](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Wprowadzenie do odwołania powiązania Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [System macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
