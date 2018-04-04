---
title: Praca z tabelami i komórek
description: Wyświetlanie danych z platformy Xamarin.iOS przy użyciu UITableView
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: a1cda3632a75c7e462e763a34fdb5b586237b670
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-tables-and-cells"></a>Praca z tabelami i komórek


W tej sekcji przedstawiono klasy służące do tworzenia i wyświetlania w tabelach, a następnie przykłady sposobu ich używania w platformy Xamarin.iOS. Obejmuje on przy użyciu domyślnego wyglądu w przypadku tabel, dostosowywanie układu Implementowanie edycji i zaprojektować wizualnie tabeli za pomocą platformy Xamarin iOS projektanta. Czasami ekran jest oczywiście listę wierszy (na przykład aplikacja utworów muzycznych) i innych wystąpień jest trudne do rozpoznania formantu table (na przykład edytowanie w aplikacji kontaktów lub konwersacji w aplikacji Messages).

Dla osób pracuje wieloplatformowych aplikacji za pomocą platformy Xamarin.Android kontroli UITableView jest podobny do klasy ListView w systemie Android (i klasy UITableViewSource jest podobny do klasy karty w systemie Android).

Te artykuły będzie Przyjrzyjmy się kompleksowe Praca z tabelami, w tym:

-   **Tabela części** — Introducing i wyjaśniający elementy wizualne `UITableView` formantu. 
-   **Wyświetlanie danych w tabelach** — pokazująca, jak utworzyć i wypełnić tabeli, używaj różnych stylów tabeli i komórki, a problemy z pamięcią przez odtwarzanie obiektów komórki. 
-   **Zaawansowane użycia** — Tworzenie niestandardowych komórek i za pomocą funkcje edycji klasy UITableView. 
-   **Tworzenie tabeli wizualnie** — przy użyciu narzędzia Projektant Xamarin dla systemu iOS, aby utworzyć interfejs tabeli scenorysu. 


## <a name="contents"></a>Spis treści

 [Tabela części &amp; funkcji](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Wypełnianie tabeli za pomocą danych](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Dostosowywanie wyglądu tabeli](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Edytowanie](~/ios/user-interface/controls/tables/editing.md)
 
 [Akcje wiersza](~/ios/user-interface/controls/tables/row-action.md)

 [Trwa tworzenie tabel w scenorysu](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Automatyczne ustalanie rozmiaru wiersza](~/ios/user-interface/controls/tables/autosizing-row-height.md)


## <a name="related-links"></a>Linki pokrewne

- [WorkingWithTables (przykład)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [Tabele w Scenorys (przykład)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Wprowadzenie do scenorysów](~/ios/user-interface/storyboards/index.md)
- [Scenorysu przepisu TableView](https://developer.xamarin.com/recipes/ios/general/storyboard/storyboard_a_tableview)
- [Wprowadzenie do MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Przykładem TableEditing w witrynie Github](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Przykładem TableParts w witrynie Github](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Przykładem TableAndCellStyles w witrynie Github](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [Odwołania do klasy UITableView](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [Odwołania do klasy UITableViewCell](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
