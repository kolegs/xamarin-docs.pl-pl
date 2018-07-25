---
title: Korzystanie z danych w aplikacji systemu Android
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 563c04ef1c8eec00108844894c5f9bdc0e9950e3
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241893"
---
# <a name="using-data-in-an-app"></a>Korzystanie z danych w aplikacji

**DataAccess_Adv** przykład pokazuje działającą aplikację, która pozwala na dane wejściowe użytkownika i funkcje bazy danych operacji CRUD (tworzenia, odczytu, aktualizowania lub usuwania). Aplikacja składa się z dwóch ekranach: listę i formularz wprowadzania danych. Cały kod dostępu do danych jest ponownie wykorzystać w systemach iOS i Android bez żadnych modyfikacji.

Po dodaniu pewne dane na ekranach aplikację wyglądać następująco w systemie Android:

![Lista przykładów dla systemu android](using-data-in-an-app-images/image11.png "lista przykładów dla systemu Android")

![Szczegółowy przykład systemu android](using-data-in-an-app-images/image12.png "szczegółowy przykład systemu Android")

Poniżej przedstawiono projekt systemu Android &ndash; kod przedstawiony w tej sekcji znajduje się w obrębie **Orm** katalogu:

![Projekt dla systemu android drzewa](using-data-in-an-app-images/image14.png "drzewa projektu dla systemu Android")

Kod macierzysty interfejsu użytkownika dla działań w systemie Android jest poza zakresem dla tego dokumentu. Zapoznaj się [ListViews dla systemu Android i karty](~/android/user-interface/layouts/list-view/index.md) przewodnika, aby uzyskać więcej informacji na temat kontrolek interfejsu użytkownika.

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

Android renderuje je jako `ListView`.

## <a name="create-and-update"></a>Tworzenie i aktualizowanie

Aby uprościć kod aplikacji, pojedynczy Zapisz metody jest pod warunkiem, który wykonuje wstawiania lub aktualizacji, w zależności od tego, czy ustawiono PrimaryKey. Ponieważ `Id` właściwość jest oznaczona za pomocą `[PrimaryKey]` atrybutu, nie należy ustawiać go w kodzie. Ta metoda będzie wykrywał, czy wartość została poprzedniego zapisany (na przykład, sprawdzając właściwość klucza podstawowego) i wstawić lub zaktualizować obiekt, w związku z tym:

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

Rzeczywistych aplikacji zwykle wymaga niektórych sprawdzania poprawności (na przykład wymagane pola, minimalnej długości lub innych reguł biznesowych). Implementowanie dobre aplikacje dla wielu platform, jaka weryfikacji logicznego możliwie udostępnionego kodu i przekazywanie błędów sprawdzania poprawności wykonywać kopie zapasowe w Interfejsie użytkownika do wyświetlania, zgodnie z możliwości platformy.

## <a name="delete"></a>Usuwanie

W odróżnieniu od `Insert` i `Update` metod `Delete<T>` metoda może akceptować tylko wartość klucza podstawowego, a nie pełne `Stock` obiektu. W tym przykładzie `Stock` obiekt jest przekazywany do metody, ale właściwość Id jest przekazywany do `Delete<T>` metody.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Za pomocą wstępnie wypełnionych plik bazy danych SQLite

Niektóre aplikacje są dostarczane z bazy danych już wypełniony danymi. Można to łatwo zrobić w aplikacji mobilnej, wysyłki istniejącego pliku bazy danych SQLite ze swoją aplikacją i skopiować go do katalogów zapisu przed uzyskaniem dostępu do jego. Ponieważ bazy danych SQLite jest standardowym formatem, która jest używana na wielu platformach, istnieje kilka narzędzi dostępnych w celu utworzenia pliku bazy danych SQLite:

-   **Rozszerzenie przeglądarki Firefox Menedżera bazy danych SQLite** &ndash; działa dla systemów Mac i Windows i tworzy pliki, które są zgodne z systemem iOS i Android.

-   **Command Line** &ndash; See [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .

Podczas tworzenia pliku bazy danych dystrybucji razem z aplikacją należy zadbać o nazewnictwa tabele i kolumny, aby upewnić się, są zgodne kodu oczekiwaniom, zwłaszcza, jeśli używasz biblioteki SQLite.NET, który będzie oczekiwać nazwy do dopasowania właściwości i klas języka C# (lub skojarzone niestandardowych atrybutów).

Aby upewnić się, że jakiś kod jest uruchamiany przed niczego więcej w aplikacji systemu Android, można go umieścić na pierwsze działanie, aby załadować lub utworzyć `Application` podklasy, który jest ładowany przed żadnych działań. Poniższy kod przedstawia `Application` podklasy, który kopiuje istniejącego pliku bazy danych **data.sqlite** poza **/Resources/Raw/** katalogu.

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

- [Dostęp Basic (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Dostęp zaawansowany (przykład)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Przepisy na danych w systemie android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Dostęp do danych z zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
