---
title: Praca z tabelami i komórek w rozszerzeniu Xamarin.iOS
description: Ten dokument prowadzi do różnych przewodników, które opisują sposób wyświetlania danych za pomocą kontrolki UITableView w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: ea5b6ba532d577bd503529065eef803acf3a7aa9
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241701"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Praca z tabelami i komórek w rozszerzeniu Xamarin.iOS

W tej sekcji przedstawiono klasy służące do tworzenia i wyświetlania w tabelach, a następnie zawiera przykłady dotyczące używania ich w rozszerzeniu Xamarin.iOS. Pokrycie dla tabel, dostosowywanie układu Implementowanie edycji i za pomocą platformy Xamarin iOS Designer wizualnie projektować tabelę przy użyciu domyślny wygląd. Czasami wyświetlanie oczywiście znajduje się lista wierszy (na przykład aplikacja Muzyka) oraz innych czas, jaki jest trudne, rozpoznawał kontrolki tabeli (na przykład edytowanie w aplikacji kontakty lub rozmowy w aplikacji Messages).

Praca na aplikacje dla wielu platform przy użyciu platformy Xamarin.Android, kontrolka UITableView jest podobna do klasy ListView w systemie Android (i klasa UITableViewSource jest podobna do klasy adapterów systemu Android).

Te artykuły będzie wykonywał kompleksowych informacji o pracy z tabelami, w tym:

-   **Tabela części** — Introducing i wyjaśnia elementów wizualnych `UITableView` kontroli. 
-   **Wyświetlanie danych w tabelach** — pokazująca, jak utworzyć i wypełnić tabelę, użyj różne style tabeli i komórki i uniknąć problemów z pamięcią, odtwarzanie obiektów komórki. 
-   **Zaawansowane zastosowania** — Tworzenie niestandardowych komórek i za pomocą funkcji edycji klasy UITableView. 
-   **Wizualne Tworzenie tabeli** — za pomocą projektanta platformy Xamarin dla systemu iOS do utworzenia interfejsu opartego na tabeli za pomocą scenorysu. 

## <a name="contents"></a>Spis treści

 [Tabela części &amp; funkcji](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Wypełnianie tabeli za pomocą danych](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Dostosowywanie wyglądu tabeli](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Edytowanie](~/ios/user-interface/controls/tables/editing.md)
 
 [Akcje wiersza](~/ios/user-interface/controls/tables/row-action.md)

 [Tworzenie tabel w scenorysu](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Automatyczne ustalanie rozmiaru wiersza](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>Linki pokrewne

- [WorkingWithTables (przykład)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [Tabele w scenorysów (przykład)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Wprowadzenie do scenorysów](~/ios/user-interface/storyboards/index.md)
- [Tworzenie scenorysów przepisu TableView](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [Wprowadzenie do elementu MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Przykład TableEditing w witrynie Github](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Przykład TableParts w witrynie Github](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Przykład TableAndCellStyles w witrynie Github](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [Informacje o klasach UITableView](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [Informacje o klasach UITableViewCell](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
