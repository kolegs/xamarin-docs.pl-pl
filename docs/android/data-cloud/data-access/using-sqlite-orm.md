---
title: Przy użyciu SQLite.NET z systemem Android
description: Biblioteka SQLite.NET PCL NuGet zapewnia mechanizm dostępu proste danych dotyczących aplikacji platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/18/2018
ms.openlocfilehash: e8e6e98cb6ada8d8da494e408e8db66ad5038799
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/20/2018
---
# <a name="using-sqlitenet-with-android"></a>Przy użyciu SQLite.NET z systemem Android

Biblioteka SQLite.NET Xamarin zalecane jest bardzo proste ORM, która pozwala w prosty sposób przechowywania i pobierania obiekty w lokalnej bazie danych SQLite na urządzeniu z systemem Android. ORM oznacza obiektów relacyjnych mapowania &ndash; interfejs API, który umożliwia zapisywanie i pobrać "obiekty" z bazy danych bez pisania instrukcji SQL.

Aby uwzględnić biblioteki SQLite.NET w aplikacji platformy Xamarin, Dodaj następujący pakiet NuGet do projektu:

- **Nazwa pakietu:** sqlite-net-pcl
- **Autor:** Paweł Krueger A.
- **Identyfikator:** sqlite-net-pcl
- **Adres URL:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet package](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet package")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> Istnieje kilka różnych pakietów SQLite — należy wybrać właściwy (może być najwyższym wynik wyszukiwania).

Po utworzeniu biblioteki SQLite.NET dostępna, wykonaj następujące trzy kroki, aby umożliwia dostęp do bazy danych:

1.  **Dodaj using instrukcji** &ndash; Dodaj następującą instrukcję do plików języka C# którym wymagany jest dostęp do danych:

    ```csharp
    using SQLite;
    ```

2.  **Utwórz pustą bazę danych** &ndash; odwołanie do bazy danych można utworzyć konstruktora klasy SQLiteConnection przekazując ścieżkę pliku. Nie trzeba sprawdzić, czy plik już istnieje &ndash; zostanie on utworzony automatycznie, jeśli jest to wymagane, w przeciwnym razie zostanie otwarty istniejącego pliku bazy danych. `dbPath` Zmiennej należy określić zgodnie z regułami omówionych wcześniej w tym dokumencie:

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3.  **Zapisz dane** &ndash; po utworzeniu obiektu SQLiteConnection polecenia bazy danych są wykonywane przez wywołanie jego metody, takie jak CreateTable i Wstaw następująco:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4.  **Pobieranie danych** &ndash; można pobrać obiektu (lub listę obiektów) należy użyć następującej składni:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Podstawowe dane przykładowe dostępu

*DataAccess_Basic* przykładowy kod dla tego dokumentu wygląda na to, gdy uruchomiony w systemie Android. Kod przedstawia sposób wykonywać proste operacje SQLite.NET i wyświetla wyniki w jako tekst w oknie głównym aplikacji.


**Android**

![Android próbki SQLite.NET](using-sqlite-orm-images/image3.png "próbki SQLite.NET systemu Android")

Poniższy przykład kodu pokazuje interakcji całej bazy danych przy użyciu biblioteki SQLite.NET w celu hermetyzacji podstawowego dostępu do bazy danych.
Zawiera:

1.  Tworzenie pliku bazy danych

2.  Wstawianie niektórych danych, tworząc obiektów i ich zapisywania

3.  Wykonywanie zapytań dotyczących danych

Należy uwzględnić te przestrzeni nazw:

```csharp
using SQLite; // from the github SQLite.cs class
```

Ostatnią wymaga, aby SQLite zostały dodane do projektu. Należy pamiętać, że w tabeli bazy danych SQLite jest zdefiniowany przez dodanie atrybutów do klasy ( `Stock` klasy) zamiast polecenia CREATE TABLE.

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

Przy użyciu `[Table]` atrybutu bez określenia parametru Nazwa tabeli spowoduje, że tabela bazy danych mają taką samą nazwę jak klasa (w tym przypadku "Stock"). Nazwa tabeli rzeczywistego jest ważne, jeśli zapisu kwerendy SQL bezpośrednio do bazy danych zamiast używać metod dostępu do danych ORM. Podobnie `[Column("_id")]` atrybutu jest opcjonalny, a jeśli go nie ma kolumny zostaną dodane do tabeli o takiej samej nazwie jak właściwość klasy.

## <a name="sqlite-attributes"></a>Atrybuty bazy danych SQLite

Atrybuty wspólne, które można zastosować do klas do kontrolowania, jak są przechowywane w bazie danych obejmują:

-   **[PrimaryKey]**  &ndash; Ten atrybut można zastosować do właściwość typu integer, aby wymusić jako klucz podstawowy tabeli podstawowej. Złożonych kluczy podstawowych nie są obsługiwane.

-   **[AutoIncrement]**  &ndash; Tego atrybutu spowoduje, że wartość właściwością liczby całkowitej na automatycznego przyrostu dla każdego nowego obiektu wstawione do bazy danych

-   **[Column(name)]**  &ndash; Dostarczanie opcjonalny `name` parametru zastąpi wartość domyślną nazwę kolumny bazy danych źródłowego, (która jest taka sama jak właściwość).

-   **[Table(name)]**  &ndash; Oznacza klasy jest w stanie mają być przechowywane w tabeli podstawowej bazy danych SQLite. Określenie parametru opcjonalna nazwa zastąpi wartość domyślną nazwę bazy danych dla tabeli podstawowej, (która jest taka sama jak nazwa klasy).

-   **[MaxLength(value)]**  &ndash; Ograniczyć długość właściwości tekstu, gdy podejmowana jest próba wstawienia bazy danych. Korzystanie z kodu należy to sprawdzić przed wstawieniem obiektu, ponieważ ten atrybut jest tylko "sprawdzana" podczas wstawiania bazy danych lub nastąpiła operacji aktualizacji.

-   **[Ignoruj]**  &ndash; SQLite.NET powoduje ignorowanie tej właściwości.
    Jest to szczególnie przydatne dla właściwości, które mają typ, który nie mogą być przechowywane w bazie danych lub właściwości modelu kolekcje, których nie można rozpoznać automatycznie konieczności SQLite.

-   **[Unikatowe]**  &ndash; Gwarantuje, że wartości w kolumnie podstawowej bazy danych są unikatowe.


Większość tych atrybutów są opcjonalne, SQLite użyje wartości domyślnych dla nazwy tabel i kolumn. Należy zawsze podać klucz podstawowy całkowitą tak, aby zapytania wyboru i usuwanie można wykonać wydajnie na danych.

## <a name="more-complex-queries"></a>Bardziej złożonych zapytań

Następujące metody na `SQLiteConnection` może służyć do wykonywania innych operacji danych:

-   **Wstaw** &ndash; dodaje nowy obiekt do bazy danych.

-   **Pobierz&lt;T&gt;**  &ndash; podejmie próbę pobrania obiektu przy użyciu klucza podstawowego.

-   **Tabela&lt;T&gt;**  &ndash; zwraca wszystkie obiekty w tabeli.

-   **Usuń** &ndash; usuwa obiektu przy użyciu klucza podstawowego.

-   **Zapytanie&lt;T&gt;**  &ndash; wykonania kwerendy SQL, która zwraca liczbę wierszy (jako obiekty).

-   **Wykonanie** &ndash; używania tej metody (i nie `Query`) podczas nieoczekiwanie wierszy z SQL (np. instrukcje INSERT, UPDATE i DELETE).


### <a name="getting-an-object-by-the-primary-key"></a>Pobieranie obiektu przy użyciu klucza podstawowego

SQLite.Net udostępnia metodę Get można pobrać pojedynczy obiekt na podstawie jego klucza podstawowego.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Wybieranie obiektu za pomocą Linq

Metody, które zwracają kolekcje obsługują `IEnumerable<T>` dzięki Linq można użyć do zapytań, lub sortowanie zawartości tabeli. Poniższy kod przedstawia przykład za pomocą Linq filtrowanie wszystkie pozycje, które zaczynają się od litery "A":

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>Wybieranie obiektu przy użyciu programu SQL

Mimo że SQLite.Net zapewniają dostęp do danych za pośrednictwem obiektu, czasami użytkownik może być konieczne wykonanie bardziej złożonego zapytania, niż pozwala Linq (lub może być konieczne szybsza). Program poleceń SQL przy użyciu metody zapytania, jak pokazano poniżej:

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!NOTE]
> Podczas pisania instrukcji SQL bezpośrednio tworzona zależności na nazwy tabel i kolumn w bazie danych, które zostały wygenerowane z klas i ich atrybutów. W przypadku zmiany nazwy w kodzie należy pamiętać, aby zaktualizować wszystkie ręcznie napisane instrukcji SQL.

### <a name="deleting-an-object"></a>Usunięcie obiektu

Klucz podstawowy służy do usuwania wierszy, jak pokazano poniżej:

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

Możesz sprawdzić `rowcount` do potwierdzenia, ile wierszy zostały zainfekowane (w tym przypadku usunięte).

## <a name="using-sqlitenet-with-multiple-threads"></a>Przy użyciu SQLite.NET z wielu wątków

SQLite obsługuje trzy różne tryby wątków: *jednym wątku*, *wielu wątków*, i *serializowanych*. Jeśli chcesz uzyskać dostępu do bazy danych z wielu wątków bez ograniczeń, można skonfigurować SQLite do użycia **serializowanych** wątków trybu. Ważne jest, aby ustawić ten tryb na początku aplikacji (na przykład na początku `OnCreate` metody).

Aby zmienić tryb wątków, należy wywołać `SqliteConnection.SetConfig`. Na przykład ten wiersz kodu konfiguruje SQLite dla **serializowanych** tryb: 

```csharp
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

Wersja bazy danych SQLite dla systemu Android ma ograniczenie, które wymaga podjęcia kilku czynności więcej. Jeśli wywołanie `SqliteConnection.SetConfig` tworzy wyjątek SQLite, takich jak `library used incorrectly`, należy użyć następującego rozwiązania:

1.  Łącze do natywnego **libsqlite.so** biblioteki, aby `sqlite3_shutdown` i `sqlite3_initialize` interfejsy API są udostępniane dla aplikacji:

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```


2.  Na początku bardzo `OnCreate` metoda, Dodaj ten kod do zamknięcia systemu SQLite, konfigurowania jej dla **serializowanych** tryb i ponownie zainicjować SQLite:

    ```csharp
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

To rozwiązanie działa także dla `Mono.Data.Sqlite` biblioteki. Aby uzyskać więcej informacji na temat SQLite i wielowątkowości, zobacz [SQLite i wiele wątków](https://www.sqlite.org/threadsafe.html). 

## <a name="related-links"></a>Linki pokrewne

- [Basic dostęp (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp Zaawansowane (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisami danych w systemie android](https://developer.xamarin.com/recipes/android/data/)
- [Dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
