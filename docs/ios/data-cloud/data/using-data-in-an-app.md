---
title: Przy użyciu danych w aplikacji systemu iOS
description: W tym dokumencie opisano DataAccess_Adv przykładzie pokazano, jak zbierać dane wejściowe użytkownika i wykonywać tworzenia, odczytu, aktualizacji i usuwania (CRUD) bazy danych operacji w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 5c9eab9316539ecf5988c8768bef9ef2cd61513e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784543"
---
# <a name="using-data-in-an-ios-app"></a>Przy użyciu danych w aplikacji systemu iOS

**DataAccess_Adv** pokazano działającą aplikację, która umożliwia dane wejściowe użytkownika i *CRUD* funkcji bazy danych (tworzenia, odczytu, aktualizacji i usuwania). Aplikacja składa się z dwóch ekrany: Lista i formularzem wprowadzania danych. Wszystkie dane kod dostępu jest wielokrotnego użytku w systemach iOS i Android bez żadnych modyfikacji.

Po dodaniu niektóre dane ekrany aplikacji następującą postać w systemie iOS:

 ![](using-data-in-an-app-images/image9.png "Lista przykładów dla systemu iOS")

 ![](using-data-in-an-app-images/image10.png "Szczegóły próbki z systemem iOS")

IOS projektu są wyświetlane poniżej — kodem przedstawionym w tej sekcji znajduje się w **Orm** katalogu:

 ![](using-data-in-an-app-images/image13.png "drzewa projektu systemu iOS")

Kodu natywnego interfejsu użytkownika dla ViewControllers w systemie iOS wykracza poza zakres tego dokumentu.
Zapoznaj się [iOS Praca z tabelami i komórkami](~/ios/user-interface/controls/tables/index.md) przewodnika, aby uzyskać więcej informacji na temat interfejsu użytkownika.

## <a name="read"></a>Odczyt

Istnieje kilka operacji odczytu w przykładzie:

-  Odczytywanie listy
-  Poszczególne rekordy odczytu


Te dwie metody w `StockDatabase` są klasy:

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

Aby uprościć kod aplikacji, pojedynczy Zapisz — metoda jest pod warunkiem, który wykonuje Insert lub Update w zależności od tego, czy ustawiono PrimaryKey. Ponieważ `Id` właściwość jest oznaczona `[PrimaryKey]` atrybutu nie należy ustawiać go w kodzie.
Ta metoda wykryje, czy wartość został poprzedniej zapisane (przez sprawdzenie właściwości klucza podstawowego) i wstawić lub zaktualizować obiekt, w związku z tym:

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



Rzeczywistych aplikacji zwykle wymaga niektórych weryfikacji (na przykład wymagane pola, minimalnej długości lub inne reguły biznesowe).
Dobrym aplikacji dla wielu platform implementuje tyle weryfikacji logicznego, jak to możliwe w kodzie udostępnionego, przekazywanie błędów sprawdzania poprawności, tworzenie kopii zapasowych interfejsu użytkownika do wyświetlenia zgodnie z możliwości platformy.

## <a name="delete"></a>Usuwanie

W odróżnieniu od `Insert` i `Update` metod, `Delete<T>` metody może akceptować tylko wartość klucza podstawowego, a nie pełnego `Stock` obiektu.
W tym przykładzie `Stock` obiekt jest przekazywany do metody, ale właściwość Id jest przekazywany do `Delete<T>` metody.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Za pomocą wstępnie wypełnionych pliku bazy danych SQLite

Niektóre aplikacje są dostarczane z bazy danych już wypełniony danych.
Można łatwo to zrobić w aplikacji mobilnej wysyłania istniejącego pliku bazy danych SQLite z aplikacją i skopiować go do katalogu zapisywalny przed uzyskaniem dostępu do jej. Ponieważ SQLite jest standardowym formatem, która jest używana na wielu platformach, istnieje szereg dostępne narzędzia do tworzenia pliku bazy danych SQLite:

-  **Rozszerzenia programu Firefox Menedżera SQLite** — działa na Mac i systemu Windows i tworzy pliki, które są zgodne z systemem iOS i Android.
-  **Command Line** – See  [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .


Podczas tworzenia pliku bazy danych dystrybucji przy użyciu aplikacji, należy zadbać o nazewnictwa tabel i kolumn do zapewnienia spełniają co oczekuje kodu, zwłaszcza, jeśli używasz SQLite.NET, który będzie oczekiwać nazwy do dopasowania właściwości i klas C# (lub skojarzonych z nimi atrybutów niestandardowych)

Dla systemu iOS, należy dołączyć plik bazy danych sqlite w aplikacji i upewnij się, jest on oznaczony **Akcja kompilacji: zawartości**. Umieść kod w `FinishedLaunching` można skopiować pliku do zapisu katalogu *przed* wywołania metody żadnych danych. Poniższy kod spowoduje skopiowanie istniejącej bazy danych o nazwie **data.sqlite**, tylko wtedy, gdy jeszcze nie istnieje.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

Kod dostępu do danych (czy ADO.NET lub przy użyciu SQLite.NET), który jest wykonywany po to ustawienie zostanie ukończone, mają dostęp do danych wstępnie wypełnione.


## <a name="related-links"></a>Linki pokrewne

- [Basic dostęp (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp Zaawansowane (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS przepisami danych](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
