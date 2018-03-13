---
title: Formant tabeli
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 0b8d8d08db15959a47093f255a891605a089ea00
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="table-control"></a>Formant tabeli

WatchOS `WKInterfaceTable` kontroli jest znacznie prostsze niż jego odpowiednik z systemem iOS, ale wykonuje podobną rolę. Tworzy przewijanej listy wierszy, które mają układy niestandardowe i które odpowiadanie na zdarzenia touch.

![](table-images/table-list-sml.png "Lista tabel czujki") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>Dodawanie tabeli

Przeciągnij **tabeli** kontroli do sceny. Domyślnie będzie wyglądać (przedstawiający układu pojedynczy wiersz nieokreślony):

[![](table-images/add-table-sml.png "Dodawanie tabeli")](table-images/add-table.png#lightbox)

Nadaj nazwę tabeli **właściwości** pad **nazwa** polu, tak, aby go mogą być przywoływane w kodzie.

## <a name="add-a-row-controller"></a>Dodawanie kontrolera wiersza

Automatycznie uwzględniono w jednym wierszu, reprezentowany przez kontroler wiersza, który zawiera **grupy** kontroli domyślnie.

Aby ustawić **klasy** kontroler wiersza, zaznacz wiersz w **konspekt dokumentu** i wpisz nazwę klasy w **właściwości** konsoli:

[![](table-images/add-row-controller-sml.png "Wprowadzanie nazwy klasy w konsoli do właściwości")](table-images/add-row-controller.png#lightbox)

Po ustawieniu klasy dla wiersza kontrolera IDE spowoduje utworzenie odpowiedniego pliku języka C# w projekcie. Przeciągnij formanty (na przykład etykiety) na wiersz i nadaj im nazwy, więc ich mogą być przywoływane w kodzie.




## <a name="create-and-populate-rows"></a>Utworzyć i wypełnić wierszy

`SetNumberOfRows` Tworzy klasy kontrolera wiersz dla każdego wiersza, przy użyciu `Identifier` wybrać właściwy. Jeśli należy nadać wiersza kontroler niestandardowy `Identifier`, zmień **domyślne** we fragmencie kodu poniżej identyfikator używany. `RowController` *Dla każdego wiersza* jest tworzone, gdy `SetNumberOfRows` nosi nazwę i tabeli wyświetlane.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
        // loads row controller by identifier
```

> [!IMPORTANT]
> **Uwaga**: wiersze tabeli nie są zwirtualizowane, jak są one w systemie iOS. Spróbuj ograniczyć liczbę wierszy (Apple zaleca mniej niż 20).
Po utworzeniu wiersze, musisz wypełnić każdej komórki (takie jak `GetCell` sposób jak w systemie iOS). Następujący fragment kodu z [WatchTables przykład](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/) aktualizuje etykiety w każdym wierszu

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> **Uwaga:** Using `SetNumberOfRows` , a następnie w pętli przy użyciu `GetRowController` powoduje, że do wysłania do czujki całą tabelę. Na kolejnych widoków tabeli, jeśli chcesz dodać lub usunąć określonych wierszy użyj `InsertRowsAt` i `RemoveRowsAt` w celu poprawy wydajności.


## <a name="respond-to-taps"></a>Odpowiadanie na podsłuchu

Zaznaczenie wiersza może odpowiedzieć na dwa sposoby:

- Implementowanie `DidSelectRow` metody na kontrolerze interfejsu lub
- Utwórz segue dla scenorysu i wdrożenie `GetContextForSegue` Jeśli zaznaczenie wiersza, aby otworzyć inny sceny.

### <a name="didselectrow"></a>DidSelectRow

Aby programowo obsłużyć zaznaczenie wiersza, zaimplementuj `DidSelectRow` metody. Aby otworzyć nowe sceny, użyj `PushController` i podaj identyfikator sceny i kontekst danych do użycia:

```csharp
public override void DidSelectRow (WKInterfaceTable table, nint rowIndex)
{
    var rowData = rows [(int)rowIndex];
    Console.WriteLine ("Row selected:" + rowData);
    // if selection should open a new scene
    PushController ("secondInterface", rows[(int)rowIndex]);
}
```

### <a name="getcontextforsegue"></a>GetContextForSegue

Przeciągnij segue dla scenorysu z wiersza z tabeli do innego sceny (przytrzymaj **kontroli** klucza podczas przeciągania).
Pamiętaj wybrać segue i nadaj mu identyfikator **właściwości** konsoli (takich jak `secondLevel` w poniższym przykładzie).

W kontrolerze interfejsu zaimplementować `GetContextForSegue` — metoda i przywracać kontekstu danych, który należy do sceny prezentowanym przez segue.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

Te dane są przekazywane do sceny docelowego scenorysu w jego `Awake` metody.

## <a name="multiple-row-types"></a>Wiele typów wiersza

Domyślnie formantu table ma typ pojedynczy wiersz, który można zaprojektować. Aby dodać więcej wierszy "Szablony" Użyj **wierszy** polu **właściwości** konsoli, aby utworzyć więcej kontrolerów wiersza:

![](table-images/prototype-rows1.png "Ustawienie liczby wierszy prototypu")

Ustawienie **wierszy** właściwości **3** spowoduje utworzenie dodatkowych wierszy symbole zastępcze można przeciągnięcie kontrolek do. Dla każdego wiersza, ustaw **klasy** w nazwie **właściwości** upewnij się, klasa kontrolera wiersza jest tworzona w konsoli.

![](table-images/prototype-rows2.png "Wiersze prototypu w Projektancie")

Aby wypełnić tabelę przy użyciu typów innego wiersza `SetRowTypes` metodę, aby określić typ kontrolera wiersza, który ma być używany dla każdego wiersza w tabeli. Identyfikatory wiersza umożliwia określenie, który kontroler wierszy powinna być używana do każdego wiersza.

Liczba elementów w tej macierzy, powinien być zgodny liczbę wierszy, która może znajdować się w tabeli:

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

Podczas wypełnienia tabeli z wielu wierszy kontrolerów, należy do śledzenia typu spodziewasz się jak wypełnić interfejsu użytkownika:

```csharp
for (var i = 0; i < rows.Count; i++) {
    if (i == 0) {
        var elementRow = (Type1RowController)myTable.GetRowController (i);
        // populate UI controls
    } else if (i == 3) {
        var elementRow = (Type2RowController)myTable.GetRowController (i);
        // populate UI controls
    } else {
        var elementRow = (DefaultRowController)myTable.GetRowController (i);
        // populate UI controls
    }
}
```


## <a name="vertical-detail-paging"></a>Szczegóły pionowy stronicowania

watchOS 3 wprowadzono nową funkcję dla tabel: możliwość przewijać strony szczegółów związanych z każdego wiersza, bez konieczności wrócić do tabeli i wybrać nowy wiersz. Ekrany szczegółów może być przewijane, szybko przesuwając w górę i w dół lub użyj Wierzchołek cyfrowego.

![](table-images/table-scroll-sml.png "Przykład szczegółów stronicowania pionowe") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> **Ostrzeżenie:** ta funkcja jest obecnie dostępna tylko, edytując scenorysu w Konstruktorze interfejsu Xcode.

Aby włączyć tę funkcję, wybierz `WKInterfaceTable` na powierzchni projektowej oraz znaczników **pionowy stronicowania szczegółów** opcji:

![](table-images/vertical-detail-paging-sml.png "Wybranie opcji stronicowania pionowy szczegółów")

Jako [wyjaśniono przez firmę Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) nawigacji tabeli muszą używać segues do poprawnego funkcji stronicowania. Napisz ponownie istniejący kod, który używa `PushController` do użycia zamiast niej segues.

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>Dodatku: Przykład kodu kontrolera wiersza

IDE automatycznie utworzy dwa pliki kodu podczas tworzenia kontrolera wiersza w projektancie. Poniżej przedstawiono w tych plikach wygenerowanego kodu dla odwołania.

Pierwszy będzie miała nazwę klasy, na przykład **RowController.cs**, podobnie do następującej:

```csharp
using System;
using Foundation;

namespace WatchTablesExtension
{
    public partial class RowController : NSObject
    {
        public RowController ()
        {
        }
    }
}
```

Druga **. Designer.cs narzędzie** plik jest definicji częściowej klasy, która zawiera punkty i akcji, które są tworzone na powierzchni projektowej, np. w tym przykładzie z jednym `WKInterfaceLabel` sterowania:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using UIKit;

namespace WatchTables.OnWatchExtension
{
    [Register ("RowController")]
    partial class RowController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        public WatchKit.WKInterfaceLabel MyLabel { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (MyLabel != null) {
                MyLabel.Dispose ();
                MyLabel = null;
            }
        }
    }
}
```

Gniazda i akcje zadeklarowany w tym miejscu można odwoływać w kodzie — ale **. Designer.cs narzędzie** pliku nie można edytować bezpośrednio.



## <a name="related-links"></a>Linki pokrewne

- [WatchTables (przykład)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
- [WatchKitCatalog (przykład)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Doc tabeli firmy Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable)
