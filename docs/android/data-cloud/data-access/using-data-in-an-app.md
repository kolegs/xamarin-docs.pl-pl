---
title: "Przy użyciu danych w aplikacji"
ms.topic: article
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 5350317c82d1073d7679843272e3215634f91217
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="using-data-in-an-app"></a>Przy użyciu danych w aplikacji

**DataAccess_Adv** pokazano działającą aplikację, która pozwala na dane wejściowe użytkownika i funkcje bazy danych CRUD (tworzenia, odczytu, aktualizacji i usuwania). Aplikacja składa się z dwóch ekrany: Lista i formularzem wprowadzania danych. Wszystkie dane kod dostępu jest wielokrotnego użytku w systemach iOS i Android bez żadnych modyfikacji.

Po dodaniu niektóre dane ekrany aplikacji następującą postać w systemie Android:

![Lista przykładów android](using-data-in-an-app-images/image11.png "lista przykładów dla systemu Android")

![Szczegóły próbki android](using-data-in-an-app-images/image12.png "szczegółów próbki systemu Android")

Poniżej przedstawiono projekt systemu Android &ndash; kodem przedstawionym w tej sekcji znajduje się w **Orm** katalogu:

![Projekt systemu android drzewa](using-data-in-an-app-images/image14.png "drzewa projektu dla systemu Android")

Kodu natywnego interfejsu użytkownika dla działań w systemie Android wykracza poza zakres tego dokumentu. Zapoznaj się [Android widokach listy i karty](~/android/user-interface/layouts/list-view/index.md) przewodnika, aby uzyskać więcej informacji na temat interfejsu użytkownika.

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

Android renderuje dane jako `ListView`.

## <a name="create-and-update"></a>Tworzenie i aktualizowanie

Aby uprościć kod aplikacji, pojedynczy Zapisz — metoda jest pod warunkiem, który wykonuje Insert lub Update w zależności od tego, czy ustawiono PrimaryKey. Ponieważ `Id` właściwość jest oznaczona `[PrimaryKey]` atrybutu nie należy ustawiać go w kodzie. Ta metoda wykryje, czy wartość został poprzedniej zapisane (przez sprawdzenie właściwości klucza podstawowego) i wstawić lub zaktualizować obiekt, w związku z tym:

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

Rzeczywistych aplikacji zwykle wymaga niektórych weryfikacji (na przykład wymagane pola, minimalnej długości lub inne reguły biznesowe). Dobrym aplikacji dla wielu platform implementuje tyle weryfikacji logicznego, jak to możliwe w kodzie udostępnionego, przekazywanie błędów sprawdzania poprawności, tworzenie kopii zapasowych interfejsu użytkownika do wyświetlenia zgodnie z możliwości platformy.

## <a name="delete"></a>Usuwanie

W odróżnieniu od `Insert` i `Update` metod, `Delete<T>` metody może akceptować tylko wartość klucza podstawowego, a nie pełnego `Stock` obiektu. W tym przykładzie `Stock` obiekt jest przekazywany do metody, ale właściwość Id jest przekazywany do `Delete<T>` metody.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Za pomocą wstępnie wypełnionych pliku bazy danych SQLite

Niektóre aplikacje są dostarczane z bazy danych już wypełniony danych. Można łatwo to zrobić w aplikacji mobilnej wysyłania istniejącego pliku bazy danych SQLite z aplikacją i skopiować go do katalogu zapisywalny przed uzyskaniem dostępu do jej. Ponieważ SQLite jest standardowym formatem, która jest używana na wielu platformach, istnieje szereg dostępne narzędzia do tworzenia pliku bazy danych SQLite:

-   **Rozszerzenia programu Firefox Menedżera SQLite** &ndash; działa na Mac i systemu Windows i tworzy pliki, które są zgodne z systemem iOS i Android.

-   **Command Line** &ndash; See [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .

Podczas tworzenia pliku bazy danych dystrybucji przy użyciu aplikacji, należy zadbać o nazewnictwa tabel i kolumn do zapewnienia spełniają co oczekuje kodu, zwłaszcza, jeśli używasz SQLite.NET, który będzie oczekiwać nazwy do dopasowania właściwości i klas C# (lub skojarzonych z nimi atrybutów niestandardowych)

Aby upewnić się, że kod jest uruchamiany przed niczego więcej w aplikacji systemu Android, należy je umieścić w pierwsze działanie, aby załadować lub można utworzyć `Application` podklasy, który jest ładowany przed żadnych działań. Kod poniżej przedstawia `Application` podklasy, który kopiuje istniejącego pliku bazy danych **data.sqlite** z **/Resources/Raw/** katalogu.

```csharp
[Application]
public class YourAndroidApp : Application {
    public override void OnCreate ()
    {
        base.OnCreate ();
        var docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
        Console.WriteLine ("Data path:" + Database.DatabaseFilePath);
        var dbFile = Path.Combine(docFolder, "data.sqlite"); // FILE NAME TO USE WHEN COPIED
        if (!System.IO.File.Exists(dbFile)) {
            var s = Resources.OpenRawResource(Resource.Raw.data);  // DATA FILE RESOURCE ID
            FileStream writeStream = new FileStream(dbFile, FileMode.OpenOrCreate, FileAccess.Write);
            ReadWriteStream(s, writeStream);
        }
    }
    // readStream is the stream you need to read
    // writeStream is the stream you want to write to
    private void ReadWriteStream(Stream readStream, Stream writeStream)
    {
        int Length = 256;
        Byte[] buffer = new Byte[Length];
        int bytesRead = readStream.Read(buffer, 0, Length);
        // write the required bytes
        while (bytesRead > 0)
        {
            writeStream.Write(buffer, 0, bytesRead);
            bytesRead = readStream.Read(buffer, 0, Length);
        }
        readStream.Close();
        writeStream.Close();
    }
}
```


## <a name="related-links"></a>Linki pokrewne

- [Basic dostęp (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp Zaawansowane (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisami danych w systemie android](https://developer.xamarin.com/recipes/android/data/)
- [Dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
