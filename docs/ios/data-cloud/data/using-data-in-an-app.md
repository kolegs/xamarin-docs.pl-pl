---
title: Korzystanie z danych w aplikacji systemu iOS
description: W tym dokumencie opisano DataAccess_Adv Utwórz przykładowy, który pokazuje, jak zbierać dane wejściowe użytkownika i wykonywać, odczytu, aktualizacji i usuwania (CRUD) bazy danych operacji w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 35caae657700e321a7560d1e95c8551b7b10a5ca
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242108"
---
# <a name="using-data-in-an-ios-app"></a>Korzystanie z danych w aplikacji systemu iOS

**DataAccess_Adv** przykład pokazuje działającą aplikację, która zezwala na dane wejściowe użytkownika i *CRUD* funkcje bazy danych (tworzenia, odczytu, aktualizowania lub usuwania). Aplikacja składa się z dwóch ekranach: listę i formularz wprowadzania danych. Cały kod dostępu do danych jest ponownie wykorzystać w systemach iOS i Android bez żadnych modyfikacji.

Po dodaniu pewne dane na ekranach aplikację wyglądać następująco w systemie iOS:

 ![](using-data-in-an-app-images/image9.png "Lista przykładów dla systemu iOS")

 ![](using-data-in-an-app-images/image10.png "Szczegóły próbki z systemem iOS")

Projekt systemu iOS jest wyświetlany poniżej — kod przedstawiony w tej sekcji znajduje się w obrębie **Orm** katalogu:

 ![](using-data-in-an-app-images/image13.png "drzewa projektu dla systemu iOS")

Natywny kod interfejsu użytkownika dla ViewControllers w systemie iOS jest poza zakresem dla tego dokumentu.
Zapoznaj się [iOS Praca z tabelami i komórek](~/ios/user-interface/controls/tables/index.md) przewodnika, aby uzyskać więcej informacji na temat kontrolek interfejsu użytkownika.

## <a name="read"></a>Odczyt

Istnieje kilka operacji odczytu w przykładzie:

-  Odczytywanie listy
-  Odczytywanie poszczególnych rekordów


Te dwie metody w `StockDatabase` klasy są:

```csharp
public IEnumerable<Stock> GetStocks ()
{
    lock (locker) {
        return (from i in Table<Stock> () select i).ToList ();
    }
}
public Stock GetStock (int id)
{
    lock (locker) {
        return Table<Stock>().FirstOrDefault(x => x.Id == id);
    }
}
```

iOS renderuje dane inaczej jako `UITableView`.

## <a name="create-and-update"></a>Tworzenie i aktualizowanie

Aby uprościć kod aplikacji, pojedynczy Zapisz metody jest pod warunkiem, który wykonuje wstawiania lub aktualizacji, w zależności od tego, czy ustawiono PrimaryKey. Ponieważ `Id` właściwość jest oznaczona za pomocą `[PrimaryKey]` atrybutu, nie należy ustawiać go w kodzie.
Ta metoda będzie wykrywał, czy wartość została poprzedniego zapisany (na przykład, sprawdzając właściwość klucza podstawowego) i wstawić lub zaktualizować obiekt, w związku z tym:

```csharp
public int SaveStock (Stock item)
{
    lock (locker) {
        if (item.Id != 0) {
            Update (item);
            return item.Id;
    } else {
            return Insert (item);
        }
    }
}
```



Rzeczywistych aplikacji zwykle wymaga niektórych sprawdzania poprawności (na przykład wymagane pola, minimalnej długości lub innych reguł biznesowych).
Implementowanie dobre aplikacje dla wielu platform, jaka weryfikacji logicznego możliwie udostępnionego kodu i przekazywanie błędów sprawdzania poprawności wykonywać kopie zapasowe w Interfejsie użytkownika do wyświetlania, zgodnie z możliwości platformy.

## <a name="delete"></a>Usuwanie

W odróżnieniu od `Insert` i `Update` metod `Delete<T>` metoda może akceptować tylko wartość klucza podstawowego, a nie pełne `Stock` obiektu.
W tym przykładzie `Stock` obiekt jest przekazywany do metody, ale właściwość Id jest przekazywany do `Delete<T>` metody.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Za pomocą wstępnie wypełnionych plik bazy danych SQLite

Niektóre aplikacje są dostarczane z bazy danych już wypełniony danymi.
Można to łatwo zrobić w aplikacji mobilnej, wysyłki istniejącego pliku bazy danych SQLite ze swoją aplikacją i skopiować go do katalogów zapisu przed uzyskaniem dostępu do jego. Ponieważ bazy danych SQLite jest standardowym formatem, która jest używana na wielu platformach, istnieje kilka narzędzi dostępnych w celu utworzenia pliku bazy danych SQLite:

-  **Rozszerzenie przeglądarki Firefox Menedżera bazy danych SQLite** — działa dla systemów Mac i Windows i tworzy pliki, które są zgodne z systemem iOS i Android.
-  **Command Line** – See  [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .


Podczas tworzenia pliku bazy danych dystrybucji razem z aplikacją należy zadbać o nazewnictwa tabele i kolumny, aby upewnić się, są zgodne kodu oczekiwaniom, zwłaszcza, jeśli używasz biblioteki SQLite.NET, który będzie oczekiwać nazwy do dopasowania właściwości i klas języka C# (lub skojarzone niestandardowych atrybutów).

Dla systemów iOS, należy dołączyć plik bazy danych sqlite w aplikacji i upewnij się, jest oznaczona za pomocą **Build Action: zawartości**. Umieść kod w `FinishedLaunching` można skopiować pliku do zapisu katalogu *przed* wywołanie dowolnej metody dotyczące danych. Poniższy kod kopiuje istniejącą bazę danych o nazwie **data.sqlite**, tylko wtedy, gdy jeszcze nie istnieje.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

Kod dostępu do żadnych danych (czy ADO.NET lub przy użyciu biblioteki SQLite.NET), który jest wykonywany po to ukończone będą mieć dostęp do danych wstępnie wypełnione.


## <a name="related-links"></a>Linki pokrewne

- [Dostęp Basic (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp zaawansowany (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisy na danych w systemie iOS](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Dostęp do danych z zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
