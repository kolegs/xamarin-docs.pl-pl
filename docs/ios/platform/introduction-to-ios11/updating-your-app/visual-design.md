---
title: Projekt Visual aktualizacji
description: Poznawanie nowych funkcji systemu IOS 11
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 50858592ec6246fe825e80a3039f3640ebf708a9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="visual-design-updates"></a>Projekt Visual aktualizacji

_Poznawanie nowych funkcji systemu IOS 11_

W systemie iOS 11 Firma Apple wprowadziła nowe zmiany wizualne w tym aktualizacje na pasku nawigacyjnym, pasek wyszukiwania i widoków tabel. Dodatkowo udoskonalono aby umożliwić większą elastyczność za pośrednictwem marginesy i pełny ekran zawartości. Te zmiany zostały uwzględnione w tym przewodniku.

## <a name="uikit"></a>UIKit

Paski UIKit zostały przystosowane w systemie iOS 11, aby były bardziej dostępny dla użytkowników końcowych.

Taka zmiana jest nowy wyświetlaną HUD, który jest wyświetlany, gdy użytkownik long naciśnięcie na pasku elementu. Aby włączyć tę opcję, należy ustawić `largeContentSizeImage` właściwość `UIBarItem` i Dodaj większy obraz za pomocą [katalogu zasobów](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>Pasek nawigacyjny
iOS 11 wprowadzono nową funkcję, aby ułatwić tytuły paska nawigacji. Aplikacje można wyświetlić ten tytuł większy, przypisując `PrefersLargeTitles` właściwości na wartość true:

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

Umożliwia ustawienie większych tytuły w aplikacji _wszystkie_ tytułów pasków nawigacji w aplikacji wyświetlane, jak pokazano na poniższym zrzucie ekranu:

![Tytuł dużych nawigacji](visual-design-images/image7.png)

Aby kontrolować, gdy duże tytułu jest wyświetlany na pasku nawigacyjnym, ustaw `LargeTitleDisplayMode` element nawigacji, aby `Always`, `Never`, lub `Automatic`.

### <a name="search-controller"></a>Kontroler wyszukiwania

iOS 11 ma się łatwiejsze dodać kontroler wyszukiwania bezpośrednio na pasku nawigacyjnym. Po utworzeniu kontrolera wyszukiwania, dodaj go do paska nawigacyjnego z `SearchController` właściwości:

```csharp
NavigationItem.SearchController = searchController;
```

[![Tytuł dużych nawigacji o pasek wyszukiwania](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

W zależności od funkcji aplikacji może lub nie może być na pasku wyszukiwania ukrywanych, gdy użytkownik przewija listy. Można ją dostosować przy użyciu `HidesSearchBarWhenScrolling` właściwości.

## <a name="margins"></a>Marginesy

Apple utworzył nową właściwość — `directionalLayoutMargins` — można ustawić odstęp między widoki i widoków podrzędnych. Użyj `directionalLayoutMargins` z `leading` lub `trailing` marginesy. Niezależnie od tego, czy system języka od lewej do prawej czy od prawej do lewej odstępy w Twojej aplikacji jest odpowiednio ustawiony przez system iOS.

W systemie iOS 10 i przed wszystkie widoki mają rozmiar minimalny margines, do którego zostaną wyrównane. iOS 11 wprowadza opcję użycia zastąpienie tego za pomocą `ViewRespectsSystemMinimumLayoutMargins`. Na przykład ustawienie wartości false dla tej właściwości pozwala na dostosowanie Twojej marginesy krawędzi od zera:

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![Obraz przedstawiający marginesu z zero krawędzi ios 11](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>Zawartość pełny ekran

System iOS 7 [wprowadzone](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` i `bottomLayoutGuide` sposób ograniczający widoków, dzięki czemu nie są ukryte przez UIKit paski i są widoczne obszarze ekranu. Te są przestarzałe w systemie iOS 11 uzyskać _bezpieczny obszar_.

Bezpieczne obszar jest nowy sposób planowania widoczne miejsca w aplikacji, oraz sposób ograniczenia są dodawane między widoku oraz widoku nadrzędnego. Rozważmy na przykład na poniższej ilustracji:

[![Vs bezpieczny obszar górnej i dolnej układu przewodnik](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

Wcześniej, jeśli został dodany widoku i chce się widoczne w obszarze zielony powyżej, w przypadku czy ograniczenia go do _dolnej_ z `TopLayoutGuide` i _górnej_ z `BottomLayoutGuide`. W systemie iOS 11, w przypadku czy zamiast tego ograniczenia go do _górnej_ i _dolnej_ bezpieczne obszaru. Następujący przykład:

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>Widok tabeli

UITableView miał liczbę małych, ale istotne, zmiany w systemie iOS 11.

Domyślnie nagłówków, stopek i komórki są teraz automatycznie ustalany na podstawie ich zawartości. Aby zrezygnować z tego zestawu zachowanie ustalania rozmiaru automatycznie `EstimatedRowHeight`, `EstimatedSectionHeaderHeight`, lub `EstimatedSectionFooterHeight` od zera.

Jednak w niektórych sytuacjach (np. gdy dodawanie UITableViewController w systemie iOS projektanta lub korzystając z istniejących Storboards konstruktora interfejsu) może być konieczne ręczne włączenie dopasowują komórek. Aby to zrobić, upewnij się, że ustawiono następujących właściwości w widoku tabeli dla komórki, nagłówki i stopki, odpowiednio:

```csharp
// Cells
TableView.RowHeight = UITableView.AutomaticDimension;
TableView.EstimatedRowHeight = UITableView.AutomaticDimension;

// Header
TableView.SectionHeaderHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionHeaderHeight = 40f;

//Footer
TableView.SectionFooterHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionFooterHeight = 40f;

```

iOS 11 rozszerzono funkcjonalność akcje wiersza. `UISwipeActionsConfiguration` wprowadzono w celu zdefiniowania zestawu działań, które powinny mieć miejsce, gdy użytkownik swipes w żadnym kierunku wiersza w widoku tabeli. To zachowanie jest podobne do natywnej Mail.app. Aby uzyskać więcej informacji, zobacz [akcje wiersza](~/ios/user-interface/controls/tables/row-action.md) przewodnik.

Widoki tabel jest obsługiwane w systemie iOS 11 przeciągania i upuszczania. Aby uzyskać więcej informacji, zobacz [przeciągnij i upuść](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) przewodnik.


## <a name="related-links"></a>Linki pokrewne

- [Nowości w systemie iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Strona produktu zaktualizowane sklepu App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualizowanie aplikacji dla systemu iOS 11 (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/204/)
