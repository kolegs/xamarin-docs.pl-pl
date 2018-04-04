---
title: Bazy danych
description: W tym artykule omówiono przy użyciu kluczy i wartości kodowania i klucz wartość obserwowania, aby umożliwić wiązanie danych między bazami danych SQLite i elementy interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode. Obejmuje ona również przy użyciu SQLite.NET ORM w celu zapewnienia dostępu do danych SQLite.
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 33c1ab7092669bb1dbd4e7bfae628b58a0bf3726
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="databases"></a>Bazy danych

_W tym artykule omówiono przy użyciu kluczy i wartości kodowania i klucz wartość obserwowania, aby umożliwić wiązanie danych między bazami danych SQLite i elementy interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode. Obejmuje ona również przy użyciu SQLite.NET ORM w celu zapewnienia dostępu do danych SQLite._

## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tej samej bazy danych SQLite, które mogą uzyskiwać dostęp do aplikacji platformy Xamarin.iOS lub platformy Xamarin.Android.

W tym artykule firma Microsoft będzie obejmował dostęp do danych SQLite na dwa sposoby:

1. **Bezpośredni dostęp** — uzyskując bezpośrednio dostęp do bazy danych SQLite, możemy użyć danych z bazy danych dla kodowania klucz wartość, a powiązanie danych z elementów interfejsu użytkownika utworzone w konstruktora interfejsu w środowisku Xcode. Przy użyciu kodowania i powiązanie technik w aplikacji Xamarin.Mac danych klucz wartość, można znacznie zmniejszyć ilość kodu, które trzeba zapisać i zachować do wypełnienia i pracować z nimi elementy interfejsu użytkownika. Masz również zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) od z przodu kończyć interfejsu użytkownika (_Model-View-Controller_), prowadzących do łatwiejsze w obsłudze, bardziej elastyczne aplikacji Projekt.
2. **SQLite.NET ORM** — przy użyciu typu open source [SQLite.NET](http://www.sqlite.org) relacji Menedżera ORM (Object) firma Microsoft może znacznie zmniejszyć ilość kod wymagany do odczytywania i zapisywania danych z bazy danych SQLite. Aby wypełnić element interfejsu użytkownika, takie jak widok tabeli można następnie te dane.

[![Przykład uruchomionej aplikacji](databases-images/intro01.png "przykładem uruchomionej aplikacji")](databases-images/intro01-large.png#lightbox)

W tym artykule omówione zostaną następujące czynności podstawowe informacje dotyczące pracy z kluczy i wartości kodowania i powiązanie danych z bazy danych SQLite w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Ponieważ użyjemy kluczy i wartości kodowania i powiązania danych, należy skorzystać z [powiązania danych i kluczy i wartości kodowania](~/mac/app-fundamentals/databinding.md) najpierw jako podstawowe techniki i koncepcje zostały omówione który będzie używany w tej dokumentacji i przykładowego jego aplikacja.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` atrybutów umożliwiają połączenie klas C# do obiektów języka Objective C i interfejsu użytkownika elementy.

## <a name="direct-sqlite-access"></a>Bezpośredni dostęp do bazy danych SQLite

Dla danych SQLite, który ma być powiązana do elementów interfejsu użytkownika w programie Xcode interfejsu konstruktora zdecydowanie zaleca się uzyskać dostępu do bazy danych SQLite bezpośrednio (a nie przy użyciu technik, takich jak ORM), ponieważ mają pełną kontrolę nad sposobem dane są zapisywane i do odczytu  z bazy danych.

Jak mamy już wspomniano w [powiązania danych i kluczy i wartości kodowania](~/mac/app-fundamentals/databinding.md) dokumentacji, przy użyciu kodowania i powiązanie technik w aplikacji Xamarin.Mac danych klucz wartość można znacznie zmniejszyć ilość kodu, który trzeba napisać i Obsługa, aby wypełnić i pracować z elementów interfejsu użytkownika. W połączeniu z bezpośredni dostęp do bazy danych SQLite, znacznie zmniejsza ilość kodu wymagane do odczytywania i zapisywania danych z tej bazy danych.

W tym artykule firma Microsoft będzie można modyfikowanie przykładową aplikację z powiązania danych i kluczy i wartości kodowania dokumentu do użycia bazy danych SQLite jako źródło zapasowego dla wiązania.

### <a name="including-sqlite-database-support"></a>W tym obsługi bazy danych SQLite

Zanim będzie można kontynuować, należy dodać obsługi bazy danych SQLite do naszej aplikacji przy tym odniesienia do kilku. Pliki biblioteki dll.

Wykonaj następujące czynności:

1. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **odwołania** i wybierz polecenie **edytować odwołania**.
2. Wybierz oba **Mono.Data.Sqlite** i **system.dane** zestawów: 

    [![Dodawanie odwołania wymagane](databases-images/reference01.png "Dodawanie odwołania wymagane")](databases-images/reference01-large.png#lightbox)
3. Kliknij przycisk **OK** przycisk, aby zapisać zmiany i dodać odwołań.

### <a name="modifying-the-data-model"></a>Modyfikowanie modelu danych

Teraz, dodano obsługę bezpośredni dostęp do bazy danych SQLite do naszej aplikacji, należy zmodyfikować naszych obiekt modelu danych do odczytu i zapisu danych z bazy danych (a także podać klucz wartość kodowania i powiązania danych). W przypadku naszego przykładowej aplikacji, firma Microsoft będzie edytować **PersonModel.cs** klasy i zapewnić ich wyglądać następująco:

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

Po pierwsze, dodano kilka przy użyciu instrukcji, które są wymagane do używania bazy danych SQLite i dodaliśmy zmiennej, aby zapisać naszych ostatniego połączenia z bazą danych SQLite:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

Użyjemy tego połączenia zapisane automatycznie zapisać jakiekolwiek zmiany z rekordem do bazy danych, gdy użytkownik modyfikuje zawartość w Interfejsie użytkownika za pośrednictwem powiązania danych:

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

Wszelkie zmiany wprowadzone do **nazwa**, **zawód** lub **isManager** właściwości zostaną wysłane do bazy danych, jeśli istnieje zapisać dane przed (np. Jeśli `_conn` Zmienna nie jest `null`). Następnie Przyjrzyjmy się metody, które dodano do **Utwórz**, **aktualizacji**, **obciążenia** i **usunąć** osób z bazy danych.

#### <a name="create-a-new-record"></a>Tworzenie nowego rekordu

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

Używamy `SQLiteCommand` Aby utworzyć nowy rekord w bazie danych. Uzyskujemy nowe polecenie z `SQLiteConnection` (conn) czy możemy przekazany do metody przez wywołanie metody `CreateCommand`. Następnie ustawiliśmy na instrukcję SQL faktycznie zapisać nowy rekord podając parametrami rzeczywistymi wartościami:

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

Później możemy ustawić wartości dla parametrów przy użyciu `Parameters.AddWithValue` metoda `SQLiteCommand`. Za pomocą parametrów Upewniamy się, że wartości (takich jak pojedynczy cudzysłów) Pobierz poprawnie zakodowany przed wysłaniem do bazy danych SQLite. Przykład:

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

Ponadto ponieważ osoby mogą być Menedżer i zawiera zbiór pracowników w ramach ich, możemy rekursywnie wywoływania `Create` metody dla tych osób, aby zapisać je w bazie danych oraz:

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

#### <a name="updating-a-record"></a>Aktualizacja rekordu

Następujący kod został dodany do zaktualizowania istniejącego rekordu bazy danych SQLite:

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

Podobnie jak **Utwórz** powyżej, uzyskujemy `SQLiteCommand` z przekazanego w `SQLiteConnection`i ustaw naszych SQL do zaktualizowania naszych rekordu (podając parametrami):

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

Możemy podać wartości parametrów (przykład: `command.Parameters.AddWithValue ("@COL1", ID);`) i ponownie rekursywnie wywoływania funkcji update na wszystkich podrzędnych rekordów:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Update sub record
    Person.Update (conn);
}
```
#### <a name="loading-a-record"></a>Ładowanie rekordu

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

Ponieważ procedura może być wywołana z obiektu nadrzędnego (na przykład obiekt menedżera ładowania ich obiektu pracowników) rekursywnie, specjalne kod został dodany do obsługi otwieranie i zamykanie połączenia z bazą danych:

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

Jak zawsze ustawiliśmy na naszym SQL do pobierania rekordu i użycia parametrów:

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

Na koniec używamy czytnika danych w celu wykonania zapytania i zwróć pola rekordu (który skopiowane do wystąpienia `PersonModel` klasy):

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

Jeśli ta osoba jest Menedżer, należy także załadować wszystkich pracowników (ponownie, przez wywołanie rekursywnie ich `Load` metody):

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

#### <a name="deleting-a-record"></a>Usunięcie rekordu

Następujący kod został dodany do usuwanie istniejącego rekordu z bazy danych SQLite:

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

W tym miejscu udostępniamy SQL, aby usunąć rekord menedżerów razem rekordy żadnych pracowników w ramach tego menedżera (przy użyciu parametrów):

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

Usunięcie rekordu wyczyszczenie możemy bieżące wystąpienie klasy `PersonModel` klasy:

```csharp
// Empty class
ID = "";
ManagerID = "";
Name = "";
Occupation = "";
isManager = false;
_people = new NSMutableArray();
```

### <a name="initializing-the-database"></a>Podczas inicjowania bazy danych

Wprowadzanie zmian do naszego modelu danych w miejscu obsługują odczytywanie i zapisywanie do bazy danych należy otworzyć połączenia z bazą danych i zainicjować go przy pierwszym uruchomieniu. Teraz Dodaj następujący kod do naszej **MainWindow.cs** pliku:

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

Spójrzmy bliższe spojrzenie na powyższym kodzie. Najpierw wybierz możemy lokalizację dla nowej bazy danych (w tym przykładzie pulpitu użytkownika), sprawdź, czy baza danych istnieje, a w przeciwnym razie utwórz go:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

Następnie nawiązanie połączenia z bazą danych przy użyciu ścieżki utworzone powyżej:

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

Następnie utworzymy tabel SQL w bazie danych, których wymagamy:

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

Na koniec używamy nasz Model danych (`PersonModel`) do utworzenia domyślny zestaw rekordów dla czasu bazy danych w pierwszym uruchomieniu aplikacji lub jeśli brak bazy danych:

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

Gdy aplikacja rozpoczyna się i zostanie otwarte okno główne, wykonujemy połączenia z bazą danych przy użyciu kodu dodane powyżej:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>Trwa ładowanie powiązanych danych

Wszystkie składniki uzyskać bezpośredni dostęp do danych związane z bazy danych SQLite w miejscu możemy załadować danych w różne widoki, które zapewnia naszej aplikacji i automatycznie będzie on wyświetlany w naszym interfejsie użytkownika.

#### <a name="loading-a-single-record"></a>Ładowanie z pojedynczym rekordem

Załadować pojedynczy rekord, w których identyfikator jest znana, możemy użyć poniższego kodu:

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>Ładowania wszystkich rekordów

Aby załadować wszystkich użytkowników, niezależnie od tego, jeśli są one menedżera lub nie, należy użyć poniższego kodu:

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

W tym miejscu używamy przeciążenia konstruktora dla `PersonModel` klasę, aby załadować osoby do pamięci:

```csharp
var person = new PersonModel (_conn, childID);
```

Również wywołania powiązany z danymi klasa do dodania osoby do naszej kolekcji osób `AddPerson (person)`, rozpoznaje zmiany naszego interfejsu użytkownika i wyświetla go:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>Ładowanie tylko rekordów najwyższego poziomu

Aby załadować tylko menedżerów (na przykład, aby wyświetlić dane w widoku konspektu), możemy użyć poniższego kodu:

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

Tylko rzeczywistych różnica w instrukcji SQL (który ładuje tylko menedżerowie `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`), ale działa tak samo jak w powyższej sekcji, w przeciwnym razie wartość.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>Bazy danych i polach kombi

Formanty Menu dostępne dla macOS (np. pola kombi) można ustawić do wypełnienia listy rozwijanej, albo z listy wewnętrznych (która może być wstępnie zdefiniowanych w Konstruktorze interfejsu lub wypełniane przy użyciu kodu) lub podając źródła danych niestandardowych, zewnętrznych. Zobacz [dostarczanie danych formant Menu](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) więcej szczegółów.

Na przykład edytować proste powiązania powyższego przykładu w Konstruktorze interfejsu dodać pole kombi i pozostawić ją przy użyciu gniazda o nazwie `EmployeeSelector`:

[![Udostępnianie gniazda pole kombi](databases-images/combo01.png "udostępnianie gniazda pola kombi")](databases-images/combo01-large.png#lightbox)

W **inspektora atrybuty**, sprawdź **Autocompletes** i **źródło danych używa** właściwości:

![Konfigurowanie atrybutów pola kombi](databases-images/combo02.png "Konfigurowanie atrybutów pola kombi")

Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji.

#### <a name="providing-combobox-data"></a>Dostarcza dane combobox

Następnie dodaj nową klasę do projektu o nazwie `ComboBoxDataSource` i zapewnić ich wyglądać następująco:

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

W tym przykładzie tworzymy nową `NSComboBoxDataSource` można obecne elementy pola kombi z dowolnego źródła danych SQLite. Najpierw należy zdefiniować następujące właściwości:

- `Conn` -Pobiera lub ustawia połączenie z bazą danych SQLite.
- `TableName` -Pobiera lub ustawia nazwę tabeli.
- `IDField` -Pobiera lub ustawia pole zawiera unikatowy identyfikator dla danej tabeli. Wartość domyślna to `ID`.
- `DisplayField` -Pobiera lub ustawia pole, które jest wyświetlane na liście rozwijanej.
- `RecordCount` -Pobiera liczby rekordów w danej tabeli.

Tworząc nowe wystąpienie obiektu, jest przekazywana w połączenia, nazwa tabeli, opcjonalnie w polu Identyfikatora i wyświetlania pola:

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

Jest to dowolnej chwili `TableName`, `IDField` lub `DisplayField` wartość właściwości zostanie zmieniona.

`IDForIndex` Metoda zwraca Unikatowy identyfikator (`IDField`) dla rekordu w indeksie elementu listy rozwijanej danego: 

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

`ValueForIndex` Metoda zwraca wartość (`DisplayField`) dla elementu o indeksie listy rozwijanej danego:

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

`ItemCount` Zwraca wstępnie obliczonych liczbę elementów na liście, gdy obliczona `TableName`, `IDField` lub `DisplayField` właściwości zostały zmienione:

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

Należy zauważyć, że użyto `LIMIT` i `OFFSET` instrukcje w naszym polecenia SQLite ogranicza jeden rekord, możemy potrzebne.

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

`CompletedString` Metoda zwraca pierwszy pasującej wartości (`DisplayField`) dla częściowo typu wpisu:

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

Ze sobą wszystkie elementy, Edytuj `SubviewSimpleBindingController` i zapewnić ich wyglądać następująco:

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

`DataSource` Właściwość zapewnia skrót do `ComboBoxDataSource` (utworzone powyżej) dołączona do pola kombi.

`LoadSelectedPerson` Metody ładuje osoby z bazy danych dla podanego Identyfikatora unikatowy:

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

W `AwakeFromNib` zastąpienie metody najpierw dołączyć możemy wystąpienia źródła danych naszych niestandardowe pole kombi:

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

Następnie odpowiemy użytkownikowi edytowanie wartości tekstowej pola kombi przez wyszukiwanie skojarzone Unikatowy identyfikator (`IDField`) danych znaleziono prezentacji i ładowanie danej osoby, jeśli:

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

Możemy również załadować nowej osoby, jeśli użytkownik wybierze nowy element z listy rozwijanej:

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

Ponadto firma Microsoft automatyczne wypełnianie pole kombi i wyświetlane osoba mająca pierwszy element na liście:

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>SQLite.NET ORM

Jak wspomniano powyżej, przy użyciu typu open source [SQLite.NET](http://www.sqlite.org) relacji Menedżera ORM (Object) firma Microsoft może znacznie zmniejszyć ilość kod wymagany do odczytywania i zapisywania danych z bazy danych SQLite. Nie można najlepszą trasę w sytuacji, gdy wiązanie danych z powodu niektóre wymagania, które kluczy i wartości kodowania i wiązania z danymi należy umieścić na obiekt.

Zgodnie z witryny sieci Web SQLite.Net _"SQLite jest biblioteki oprogramowania, która implementuje niezależne, niekorzystającą, niewymagającą konfiguracji, transakcyjne aparatu bazy danych SQL. SQLite jest aparat najczęściej wdrożonej bazy danych na całym świecie. Kod źródłowy SQLite jest w domenie publicznej."_

W poniższych sekcjach pokażemy sposób użycia SQLite.Net dostarczający dane dla widoku tabeli.

### <a name="including-the-sqlitenet-nuget"></a>W tym polu SQLite.net

SQLite.NET jest przedstawiany jako pakietu NuGet, który można umieścić w aplikacji. Zanim dodamy obsługi bazy danych przy użyciu SQLite.NET, musimy dołączyć tego pakietu.

Wykonaj następujące polecenie, aby dodać pakiet:

1. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **pakiety** i wybierz polecenie **Dodawanie pakietów...**
2. Wprowadź `SQLite.net` w **pole wyszukiwania** i wybierz **sqlite net** wpis:

    [![Dodawanie pakietu SQLite NuGet](databases-images/nuget01.png "dodanie pakietu SQLite NuGet")](databases-images/nuget01-large.png#lightbox)
3. Kliknij przycisk **Dodaj pakiet** przycisk, aby zakończyć.

### <a name="creating-the-data-model"></a>Tworzenie modelu danych

Umożliwia dodawanie nowych klas do projektu i wywołania w `OccupationModel`. Następnie umożliwia edytowanie **OccupationModel.cs** pliku i zapewnić ich wyglądać następująco:

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

Po pierwsze, przeprowadzamy SQLite.NET (`using Sqlite`), następnie uwidaczniamy kilka właściwości, z których każdy zostaną zapisane w bazie danych po zapisaniu tego rekordu. Pierwszą właściwością, firma Microsoft jako klucz podstawowy i ustaw ją na automatyczne zwiększanie w następujący sposób:

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```
### <a name="initializing-the-database"></a>Podczas inicjowania bazy danych

Wprowadzanie zmian do naszego modelu danych w miejscu obsługują odczytywanie i zapisywanie do bazy danych należy otworzyć połączenia z bazą danych i zainicjować go przy pierwszym uruchomieniu. Teraz Dodaj następujący kod:

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

Najpierw możemy pobrać ścieżki do bazy danych (w tym przypadku pulpitu użytkownika) i zobacz, czy baza danych już istnieje:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

Następnie nawiązanie połączenia z bazą danych w ścieżce utworzone powyżej:

```csharp
var conn = new SQLiteConnection (db);
```

Na koniec możemy utworzyć tabeli i dodać niektóre domyślne rekordy:

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

Jako przykład użycia dodamy widoku tabeli do naszego interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode. Będzie uwidaczniamy ten widok tabeli za pomocą gniazda (`OccupationTable`), firma Microsoft może do niego dostęp przez kod w języku C#:

[![Udostępnianie gniazda widok tabeli](databases-images/table01.png "udostępnianie gniazda widoku tabeli")](databases-images/table01-large.png#lightbox)

Następnie dodamy klas niestandardowych, aby wypełnić tę tabelę z danymi z bazy danych SQLite.NET.

### <a name="creating-the-table-data-source"></a>Tworzenie tabeli źródła danych

Teraz Utwórz niestandardowe źródło danych dostarczający dane dla gości. Najpierw dodaj nową klasę o nazwie `TableORMDatasource` i zapewnić ich wyglądać następująco:

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

Tworząc wystąpienia tej klasy później, będzie przekazywana w naszym Otwórz połączenie z bazą danych SQLite.NET. `LoadOccupations` Metoda wysyła zapytanie do bazy danych i kopiuje znalezionych rekordów do pamięci (za pomocą naszych `OccupationModel` modelu danych).

### <a name="creating-the-table-delegate"></a>Tworzenie tabeli delegata

Klasy końcowe, potrzebne jest niestandardowe pełnomocnika tabeli, aby wyświetlić informacje, które mają został załadowany z bazy danych SQLite.NET. Możemy dodać nową `TableORMDelegate` do naszej projektu i przydzielić mu wyglądać następująco:

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

Tutaj używamy źródła danych `Occupations` kolekcji (do którego został załadowany z bazy danych SQLite.NET) do wypełnienia kolumny naszych tabeli za pomocą `GetViewForItem` zastąpienie metody.

### <a name="populating-the-table"></a>Wypełnianie tabeli

Wszystkie elementy w miejscu umożliwia wypełnianie naszych tabeli, gdy jest on zwiększony z pliku .xib przez zastąpienie `AwakeFromNib` — metoda i udostępnianie ich wyglądać podobnie do następującego:

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

Po pierwsze możemy uzyskać dostęp do bazy jest kompletny SQLite.NET, tworzenie i wypełnianie jej, jeśli jeszcze nie istnieje. Następnie utworzymy nowe wystąpienie z naszych niestandardowe źródło danych tabeli, przekaż połączenie z bazą danych i firma Microsoft dołączenie go do tabeli. Ponadto firma Microsoft utworzenia nowego wystąpienia naszych niestandardowych delegata tabeli, należy przekazać w naszym źródła danych i dołączenie go do tabeli.

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się praca z powiązania danych i kluczy i wartości kodowania z bazami danych SQLite w aplikacji Xamarin.Mac. Najpierw należy go przeglądał udostępnianie klasy C# do języka Objective-C za pomocą kluczy i wartości kodowania (KVC) i klucz wartość obserwowania (KVO). Następnie go pokazano, jak użyć klasy zgodne KVO i danych powiązać go z elementów interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode. Artykuł objętych usługą, Praca z danymi SQLite za pośrednictwem SQLite.NET ORM i wyświetlanie danych w widoku tabeli.



## <a name="related-links"></a>Linki pokrewne

- [MacDatabase (przykład)](https://developer.xamarin.com/samples/mac/MacDatabase/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Powiązanie danych i kluczy i wartości kodowania](~/mac/app-fundamentals/databinding.md)
- [Formanty standardowe](~/mac/user-interface/standard-controls.md)
- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Widoki konspektu](~/mac/user-interface/outline-view.md)
- [Kolekcja widoków](~/mac/user-interface/collection-view.md)
- [Kodowanie przewodnik programowania w języku klucz wartość](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Wprowadzenie do programowania tematy powiązania Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Wprowadzenie do Cocoa powiązania odwołania](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [System macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
