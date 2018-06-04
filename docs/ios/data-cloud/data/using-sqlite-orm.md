---
title: Przy użyciu SQLite.NET z systemem iOS
description: Biblioteka SQLite.NET PCL NuGet udostępnia proste danych mechanizm dostępu do aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 79813B09-42D7-47DD-AE71-A605E6B9EF24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/18/2018
ms.openlocfilehash: 861b024a7ff5dd07752662cc45306b6533cd38bd
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34689504"
---
# <a name="using-sqlitenet-with-ios"></a>Przy użyciu SQLite.NET z systemem iOS

Biblioteka SQLite.NET zalecający Xamarin jest podstawowe ORM, która umożliwia przechowywanie i pobieranie obiekty w lokalnej bazie danych SQLite na urządzeniu z systemem iOS.
ORM oznacza relacyjne mapowania obiektu — interfejs API, który umożliwia zapisywanie i pobrać "obiekty" z bazy danych bez pisania instrukcji SQL.

<a name="Usage"/>

## <a name="usage"></a>Użycie

Aby uwzględnić biblioteki SQLite.NET w aplikacji platformy Xamarin, Dodaj następujący pakiet NuGet do projektu:

- **Nazwa pakietu:** sqlite-net-pcl
- **Autor:** Paweł Krueger A.
- **Identyfikator:** sqlite-net-pcl
- **Adres URL:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet package](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet package")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> Istnieje kilka różnych pakietów SQLite — należy wybrać właściwy (może być najwyższym wynik wyszukiwania).

Po utworzeniu biblioteki SQLite.NET dostępna, wykonaj następujące trzy kroki, aby umożliwia dostęp do bazy danych:

1. **Dodaj using instrukcji** — Dodaj następującą instrukcję do plików języka C# którym wymagany jest dostęp do danych:

    ```csharp
    using SQLite;
    ```

1. **Utwórz pustą bazę danych** — odwołanie do bazy danych można utworzyć konstruktora klasy SQLiteConnection przekazując ścieżkę pliku. Nie trzeba sprawdzić, czy plik już istnieje — zostanie automatycznie utworzone Jeśli wymagane, w przeciwnym razie istniejącego pliku bazy danych zostanie otwarty.

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

    Zmienna dbPath należy określić zgodnie z regułami opisem we wcześniejszej części tego dokumentu.

1. **Zapisz dane** — po utworzeniu obiektu SQLiteConnection bazy danych polecenia są wykonywane przez wywołanie jego metody, takie jak CreateTable i Wstaw następująco:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

1. **Pobieranie danych** — można pobrać obiektu (lub listę obiektów), użyj następującej składni:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Podstawowe dane przykładowe dostępu

*DataAccess_Basic* przykładowy kod dla tego dokumentu wygląda na to, gdy uruchomiony w systemie iOS. Kod przedstawia sposób wykonywać proste operacje SQLite.NET i wyświetla wyniki w jako tekst w oknie głównym aplikacji.

**iOS**

 [![Przykładowe SQLite.NET systemu iOS](using-sqlite-orm-images/image2-sml.png)](using-sqlite-orm-images/image2-sml.png#lightbox)

Poniższy przykład kodu pokazuje interakcji całej bazy danych przy użyciu biblioteki SQLite.NET w celu hermetyzacji podstawowego dostępu do bazy danych. Zawiera:

1.  Tworzenie pliku bazy danych
1.  Wstawianie niektórych danych, tworząc obiektów i ich zapisywania
1.  Wykonywanie zapytań dotyczących danych

Należy uwzględnić te przestrzeni nazw:

```csharp
using SQLite; // from the github SQLite.cs class
```

Wymaga to, że dodano SQLite do projektu, jak wyróżniono [tutaj](#Usage). Należy pamiętać, że w tabeli bazy danych SQLite jest zdefiniowany przez dodanie atrybutów do klasy ( `Stock` klasy) zamiast polecenia CREATE TABLE.

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

-  **[PrimaryKey]**  — Ten atrybut można zastosować do właściwość typu integer, aby wymusić jako klucz podstawowy tabeli podstawowej. Złożonych kluczy podstawowych nie są obsługiwane.
-  **[AutoIncrement]**  — Tego atrybutu spowoduje, że wartość właściwością liczby całkowitej na automatycznego przyrostu dla każdego nowego obiektu wstawione do bazy danych
-  **[Column(name)]**  — Dostarczanie opcjonalny `name` parametru zastąpi wartość domyślną nazwę kolumny bazy danych źródłowego, (która jest taka sama jak właściwość).
-  **[Table(name)]**  — Oznacza klasy jest w stanie mają być przechowywane w tabeli podstawowej bazy danych SQLite. Określenie parametru opcjonalna nazwa zastąpi wartość domyślną nazwę bazy danych dla tabeli podstawowej, (która jest taka sama jak nazwa klasy).
-  **[MaxLength(value)]**  — Ogranicz długość właściwości tekstu, próba wstawienia bazy danych. Korzystanie z kodu należy to sprawdzić przed wstawieniem obiektu, ponieważ ten atrybut jest tylko "sprawdzana" podczas wstawiania bazy danych lub nastąpiła operacji aktualizacji.
-  **[Ignoruj]**  — SQLite.NET powoduje ignorowanie tej właściwości. Jest to szczególnie przydatne dla właściwości, które mają typ, który nie mogą być przechowywane w bazie danych lub właściwości modelu kolekcje, których nie można rozpoznać automatycznie konieczności SQLite.
-  **[Unikatowe]**  — Zapewnia, że wartości w kolumnie podstawowej bazy danych są unikatowe.


Większość tych atrybutów są opcjonalne, SQLite użyje wartości domyślnych dla nazwy tabel i kolumn. Należy zawsze podać klucz podstawowy całkowitą tak, aby zapytania wyboru i usuwanie można wykonać wydajnie na danych.

## <a name="more-complex-queries"></a>Bardziej złożonych zapytań

Następujące metody na `SQLiteConnection` może służyć do wykonywania innych operacji danych:

-  **Wstaw** — dodaje nowy obiekt do bazy danych.
-  **Pobierz<T>**  — podejmie próbę pobrania obiektu przy użyciu klucza podstawowego.
-  **Tabela<T>**  — zwraca wszystkie obiekty w tabeli.
-  **Usuń** — usuwa obiektu przy użyciu klucza podstawowego.
-  **Zapytanie<T>**  -wykonania kwerendy SQL, która zwraca liczbę wierszy (jako obiekty).
-  **Wykonanie** — Użyj tej metody (i nie `Query` ) podczas nieoczekiwanie wierszy z SQL (np. instrukcje INSERT, UPDATE i DELETE).


### <a name="getting-an-object-by-the-primary-key"></a>Pobieranie obiektu przy użyciu klucza podstawowego

SQLite.Net udostępnia metodę Get można pobrać pojedynczy obiekt na podstawie jego klucza podstawowego.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Wybieranie obiektu za pomocą Linq

Metody, które zwracają kolekcje obsługuje interfejsu IEnumerable<T> dzięki Linq można użyć do zapytań, lub sortowanie zawartości tabeli. Poniższy kod przedstawia przykład za pomocą Linq filtrowanie wszystkie pozycje, które zaczynają się od litery "A":

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

> [!IMPORTANT]
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

## <a name="related-links"></a>Linki pokrewne

- [Basic dostęp (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp Zaawansowane (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
