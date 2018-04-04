---
title: Wysokość wiersza Auto-Sizing
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 73e16c3381b639645463e3e8aaeed35224b67861
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="auto-sizing-row-height"></a>Wysokość wiersza Auto-Sizing

Począwszy od systemu iOS 8, Apple dodano możliwość utworzenia widoku tabeli (`UITableView`) który automatycznie zwiększyć lub zmniejszyć wysokość danego wiersza, zależnie od rozmiaru jego zawartości przy użyciu automatycznego układu, rozmiar klasy i ograniczenia.

iOS 11 został dodany przez wiersze rozwinąć automatycznie. Komórek nagłówków, stopek i można teraz ustalany automatycznie na podstawie ich zawartości. Jednak jeśli tabela jest tworzony w systemie iOS projektanta, konstruktora interfejsu lub ma ustaloną wysokość wierszy należy ręcznie włączyć self zmiany rozmiaru komórek, zgodnie z opisem w tym przewodniku.

## <a name="cell-layout-in-the-ios-designer"></a>Układ komórki w Projektancie systemu iOS

Otwórz scenorysu widoku tabeli mają w celu zapewnienia wiersza automatycznej zmiany rozmiaru dla systemu iOS Designer, zaznacz komórki *prototypu* i zaprojektować układ komórki. Na przykład:

[![](autosizing-row-height-images/table01.png "Projekt prototypu komórki")](autosizing-row-height-images/table01.png#lightbox)

Dla każdego elementu w prototyp dodać ograniczeń do zachowania elementów w poprawnej pozycji jako rozmiaru widoku tabeli obrotu lub różnych rozmiarów ekranu urządzenia z systemem iOS. Na przykład przypinanie `Title` u góry po lewej i prawej komórki *widok zawartości*:

[![](autosizing-row-height-images/table02.png "Przypinanie tytuł do góry, lewy i prawy widoku zawartości komórek")](autosizing-row-height-images/table02.png#lightbox)

W przypadku naszego Przykładowa tabela małą `Label` (w obszarze `Title`) to pole, można zmniejszyć rozmiar i powiększać, aby zwiększyć lub zmniejszyć wysokość wiersza. Aby uzyskać ten efekt, Dodaj następujące ograniczenia, aby przypiąć lewo, prawo, górny i dolny etykiety:

[![](autosizing-row-height-images/table03.png "Te ograniczenia, aby przypiąć lewo, prawo, górny i dolny etykiety")](autosizing-row-height-images/table03.png#lightbox)

Teraz, gdy będziemy mieć pełni ograniczonego elementów w komórce, musimy wyjaśnienia, który element powinien zostać rozciągnięty. Aby to zrobić, ustaw **zawartości priorytet — potem** i **zawartości priorytet odporności kompresji** w miarę potrzeb **układu** sekcji konsoli do właściwości:

[![](autosizing-row-height-images/table03a.png "Układ części konsoli do właściwości")](autosizing-row-height-images/table03a.png#lightbox)

Ustaw element, który ma być rozszerzony, aby **niższe** wartość priorytetu — potem, a **niższe** wartość priorytetu odporności kompresji.

Następnie należy zaznaczyć komórki prototypu i nadaj mu unikatowy **identyfikator**:

[![](autosizing-row-height-images/table04.png "Nadanie prototypu komórki Unikatowy identyfikator")](autosizing-row-height-images/table04.png#lightbox)

W przypadku naszym przykładzie `GrowCell`. Użyjemy tej wartości później podczas wypełnimy tabeli.

> [!IMPORTANT]
> Jeśli tabela zawiera więcej niż jeden typ komórki (**prototypu**), należy się upewnić, każdy typ ma własny unikatowy `Identifier` automatycznej pracy zmiany rozmiaru wiersza.

Dla każdego elementu naszych prototypu komórki przypisać **nazwa** je ujawnić do kodu C#. Na przykład:

[![](autosizing-row-height-images/table05.png "Przypisz nazwę do udostępnienia jej do kodu C#")](autosizing-row-height-images/table05.png#lightbox)

Następnie Dodaj klasę niestandardowych dla `UITableViewController`, `UITableView` i `UITableCell` (prototypu). Na przykład: 

[![](autosizing-row-height-images/table06.png "Dodawanie niestandardowej klasy UITableViewController, UITableView i UITableCell")](autosizing-row-height-images/table06.png#lightbox)

Na koniec, aby wszystkie oczekiwane zawartość jest wyświetlana w naszym etykiety, ustaw **wierszy** właściwości `0`:

[![](autosizing-row-height-images/table06.png "Właściwość linii ustawiony na 0")](autosizing-row-height-images/table06a.png#lightbox)

Przy użyciu interfejsu użytkownika zdefiniowane możemy dodać kod, aby umożliwić zmianę rozmiaru wysokość wiersza automatycznie.

## <a name="enabling-auto-resizing-height"></a>Włączanie automatycznej zmiany rozmiaru wysokości

W jednym widoku naszych tabeli źródła danych (`UITableViewDatasource`) lub źródło (`UITableViewSource`), gdy firma Microsoft usuwania z kolejki w komórce, należy użyć `Identifier` zdefiniowanego w projektancie. Na przykład:

```csharp
public string CellID {
    get { return "GrowCell"; }
}
...

public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (CellID, indexPath) as GrowRowTableCell;
    var item = Items [indexPath.Row];

    // Setup
    cell.Image = UIImage.FromFile(item.ImageName);
    cell.Title = item.Title;
    cell.Description = item.Description;

    return cell;
}
```

Domyślnie zostanie ustawiona widok tabeli dla automatycznej zmiany rozmiaru wysokości wiersza. Aby to zapewnić, `RowHeight` powinien mieć ustawioną właściwość `UITableView.AutomaticDimension`. Możemy również konieczne skonfigurowanie `EstimatedRowHeight` właściwości w naszym `UITableViewController`. Na przykład:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Initialize table
    TableView.DataSource = new GrowRowTableDataSource(this);
    TableView.Delegate = new GrowRowTableDelegate (this);
    TableView.RowHeight = UITableView.AutomaticDimension;
    TableView.EstimatedRowHeight = 40f;
    TableView.ReloadData ();
}
```

Ta Szacowana nie musi być dokładnie, wstępnie Oszacowana średnia wysokość każdego wiersza w widoku tabeli.

Ten kod w miejscu gdy aplikacja jest uruchamiana, każdego wiersza zostanie zmniejszyć i powiększania na podstawie wysokości ostatnia etykieta w prototypu komórki. Na przykład:

[![](autosizing-row-height-images/table07.png "Uruchom przykładową tabelę")](autosizing-row-height-images/table07.png#lightbox)


## <a name="related-links"></a>Linki pokrewne

- [GrowRowTable (przykład)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
