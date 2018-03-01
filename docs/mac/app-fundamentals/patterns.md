---
title: "Wspólne wzorce i idioms"
description: "Ten dokument zawiera opis wzorca model-view-controller, wzorce źródła a obiektem delegowanym danych i protokoły."
ms.topic: article
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/17/2016
ms.openlocfilehash: 4926670a0cd0960c9a390bd388d3530929634153
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="common-patterns-and-idioms"></a>Wspólne wzorce i idioms

W całej firmy Apple interfejsach API udostępnianych za pośrednictwem C# niektórych idioms i wzorce znaleziona wielokrotnie. Jeśli masz doświadczenie z programowania za pomocą platformy Xamarin.iOS te mogą wyglądać znajomo. Dokumentacja będzie często odnosić się do tych wzorców i idioms wielokrotnie, więc o stałych zrozumienie ich pomoże Ci sensu dokumentację można znaleźć.

## <a name="mvc---model-view-controller"></a>MVC — Model View Controller

Model View Controller lub MVC skrócie, to bardzo typowy wzorzec znaleziono w całym Cocoa. Szczegółowe omówienie wykracza poza zakres tego dokumentu, ale w skrócie jest sposób struktury aplikacji na składniki:

- **Model** reprezentować danych podstawowych jest wyświetlanie i manipulowanie (na przykład adresy w książce adresowej)
- **Widok** obiektów obsługiwać rysunku danego obiektu na ekranie i obsługi interakcji z użytkownikiem (wyświetlane na ekranie adresu pola tekstowego)
- **Kontroler** interakcji między modelu obsługi obiektów. One Wypchnij zmiany modelu "nawet" Aby zaktualizować widoku i wypchnąć "" zmiany z widoku użytkowników wprowadzenia zmian w interfejsie użytkownika.

Jeśli znasz z modelem MVVM (Model View ViewModel) z innych bibliotek, takich jak WPF, działa podobnie do ViewModel kontrolera, ale często jest bardziej ściśle powiązana z określonych elementów interfejsu użytkownika.

Więcej informacji można znaleźć tutaj:

- [Learning MVC w witrynie sieci Web firmy Apple](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Model View Controller w języku Objective C](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Praca z systemu Windows](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>Źródło danych / delegować / tworzenie podklas

Inny bardzo typowy wzorzec Cocoa podchodzi do dostarczania danych do elementów interfejsu użytkownika i reagowanie na interakcji użytkownika. Przy użyciu `NSTableView` na przykład konieczne jest zapewnienie jakiś sposób dane dla każdego wiersza, ustaw niektórych elementów interfejsu użytkownika reprezentujące wiersz niektóre zestaw zachowań, aby zareagować na interakcje użytkownika i prawdopodobnie ilość niektóre dostosowania. Wzorce źródła a obiektem delegowanym danych umożliwiają obsługę większości przypadków bez potrzeby odwołać się do podklasy `NSTableView` samodzielnie.

- `DataSource` Właściwości przypisano wystąpienia niestandardowego podklasą `NSTableViewDataSource` która jest wywoływana w celu wypełnienia tabeli danych (za pośrednictwem `GetRowCount` i `GetObjectValue`).

- `Delegate` Właściwości przypisano wystąpienia niestandardowego podklasą klasy `NSTableViewDelegate` zapewniające widok dla danego modelu obiektu (za pośrednictwem `GetViewForItem`) i obsługuje interakcje interfejsu użytkownika (za pośrednictwem `DidClickTableColumn`, `MouseDownInHeaderOfTableColumn`itp).

W niektórych przypadkach należy dostosować formantu w sposób poza punkty zaczepienia, podany w źródle danych lub delegata i bezpośrednio można podklasy widoku. Należy jednak zachować ostrożność, w wielu przypadkach Zastępowanie domyślnego zachowania następnie konieczne będzie obsługiwać wszystkie tego zachowania samodzielnie (Dostosowywanie zachowania wyboru może wymagać wykonania wszystkie zachowania wyboru użytkownika).

W Xamarin.iOS, niektóre funkcje interfejsu API, takich jak `UITableView` zostały rozszerzone o właściwości, która implementuje zarówno delegata, jak i źródła danych (`UITableViewSource`). Aby obejść ograniczenie dotyczące typowych, że pojedyncza Klasa C# może mieć tylko jedną klasę podstawową i udostępniając naszych protokołów odbywa się za pośrednictwem klas podstawowych.

Aby uzyskać więcej informacji na temat pracy z widoków tabel w aplikacji Xamarin.Mac, zobacz nasze [widoku tabeli](~/mac/user-interface/table-view.md) dokumentacji.

## <a name="protocols"></a>protokoły

Protokoły w języku Objective C można porównać do interfejsów w języku C#, a w wielu przypadkach są używane w takich sytuacjach. Na przykład `NSTableView` powyższym przykładzie zarówno delegata, jak i źródła danych są faktycznie protokołów. Xamarin.Mac udostępnia je jako klasy podstawowe metody wirtualne, które można zastąpić. Główną różnicą między interfejsów języka C# i protokoły języka Objective C jest, czy niektóre metody w protokole może być opcjonalny do zaimplementowania. Należy szukać w dokumentacji i/lub definicji interfejsu API, aby określić, co to jest opcjonalne.

Więcej informacji można znaleźć pod adresem naszych [delegatów, protokołów i zdarzenia](~/ios/app-fundamentals/delegates-protocols-and-events.md) dokumentacji.



## <a name="related-links"></a>Linki pokrewne

- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Praca z systemu windows](~/mac/user-interface/window.md)
- [Obiekty delegowane, protokołów i zdarzenia](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
