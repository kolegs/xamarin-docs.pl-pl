---
title: Wprowadzenie do scenorysu
description: "Ten artykuł zawiera wprowadzenie do pracy z Scenorys w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i obsługę przy użyciu scenorys i kompilatora interfejsu Xcode w Interfejsie użytkownika aplikacji."
ms.topic: article
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: ccee60b5d953987e858ef592d005cec9803b8b96
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-storyboards"></a>Wprowadzenie do scenorysu

_Ten artykuł zawiera wprowadzenie do pracy z Scenorys w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i obsługę przy użyciu scenorys i kompilatora interfejsu Xcode w Interfejsie użytkownika aplikacji._

Scenorys umożliwiają tworzenie interfejsu użytkownika dla aplikacji Xamarin.Mac obejmuje nie tylko definicji okna i kontrolek, ale również zawiera łącza między innego systemu windows (za pośrednictwem segues) i wyświetlanie stanów.

[![](images/intro01.png "Przykładowy interfejs użytkownika w środowisku Xcode")](images/intro01.png#lightbox)

W tym artykule zapewni wprowadzenie do używania Scenorys do definiowania aplikacji Xamarin.Mac użytkownika interfejsu.

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>Co to są Scenorys?

Za pomocą Scenorys, wszystkie Interfejsie użytkownika aplikacji Xamarin.Mac można zdefiniować w jednej lokalizacji, biorąc pod uwagę nawigacji między jego poszczególne elementy i interfejsów użytkownika. Scenorys dla Xamarin.Mac, działają w bardzo podobnie do Scenorys dla platformy Xamarin.iOS. Jednak zawierają one zestaw _Segue typy_ z powodu idioms innego interfejsu.

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>Praca z sceny

Jak już wspomniano, scenorysu definiuje wszystkie interfejsu użytkownika dla danej aplikacji, podzielić na funkcjonalności omówienie jej _kontrolerów widoku_. W Konstruktorze interfejsu w programie Xcode, każdy z tych kontrolerów przebywa w jego własnej _sceny_.

[![](images/intro02.png "Przykład kontrolera widoku")](images/intro02.png#lightbox)

Każdy sceny reprezentuje dany widok i para kontrolera widoku przy użyciu zestawu wierszy (nazywane Segues) łączących każdego sceny w interfejsie użytkownika, w związku z tym wyświetlanie ich relacji. Niektóre Segues zdefiniować sposób jeden kontroler widoku zawiera co najmniej jeden podrzędny widoki lub widok kontrolerów. Inne Segues zdefiniuj przejścia między kontrolerem widoku (na przykład wyświetlanie popover lub okno dialogowe). 

[![](images/intro03.png "Segue próbki")](images/intro03.png#lightbox)

Ważne jest, należy pamiętać, jest, że każdy Segue reprezentuje przepływ jakiegoś typu danych między danego elementu interfejsu użytkownika aplikacji.

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>Praca z kontrolerami widoku

Widok kontrolerów zdefiniowania relacji między danego widoku informacji w aplikacji Mac i model danych, który zawiera te informacje. Każdy sceny najwyższego poziomu w scenorysu reprezentuje jeden kontroler widoku w kodzie aplikacji Xamarin.Mac.

[![](images/intro04.png "Przykład dokumentów kontrolera widoku")](images/intro04.png#lightbox)

W ten sposób każdy kontroler widoku jest niezależna, wielokrotnego użytku parowanie informacji wizualnej reprezentacji (Widok), jak i logika stanowią i kontrolowania tych informacji.

W ramach danej sceny można wykonywać wszystkie czynności, które może być zwykle obsługiwane przez poszczególne `.xib` plików: 

 - Miejsce subviews i formanty (na przykład przycisków i pola tekstowe).
 - Zdefiniuj element pozycji i ograniczenia układu automatycznego.
 - Podczas transmisji akcje i gniazda do udostępnienia elementy interfejsu użytkownika do kodu.

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>Praca z Segues

Jak już wspomniano, Segues Podaj relacje między wszystkimi sceny, które definiują Interfejsie użytkownika aplikacji. Jeśli znasz pracy w Scenorys w systemie iOS, należy pamiętać, że Segues dla systemu iOS zwykle określają przejścia między widokami pełny ekran. To różni się od macOS, gdy Segues zazwyczaj Definiowanie "zawierania" (gdzie jedną scenę jest elementem podrzędnym elementu nadrzędnego sceny).

W macOS większość aplikacji często do grupowania ich widoków razem w tym samym oknie za pomocą elementów interfejsu użytkownika, takich jak widoki i karty. W przeciwieństwie do systemu iOS, których widoków musi zostać przeniesieni włączać i wyłączać ekranu, ze względu na ograniczone fizyczna wyświetlanie miejsca.

Biorąc pod uwagę tendencji macOS w kierunku zawierania, istnieją sytuacje, gdy _Segues prezentacji_ są używane, takie jak okna modalne, widoki arkusza i Popovers.

Podczas korzystania z Segues prezentacji, można zastąpić `PrepareForSegue` metoda kontrolera widoku nadrzędnego prezentacji zainicjować i zmienne i zapewniają kontroler widoku jest prezentowana żadnych danych.

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>Projektowanie i czasy uruchomienia

W czasie projektowania (gdy układu limit interfejsu użytkownika w programie Xcode interfejsu konstruktora), każdy element interfejsu użytkownika aplikacji jest podzielony na jego elementów składowych:

- **Sceny** — które składają się z:
    - **Wyświetlić kontroler** -definiującą relacje między widoki i dane, które je obsługują.
    - **Widoki i widoków podrzędnych** -rzeczywiste elementy wchodzące w skład interfejsu użytkownika.
    - **Zawieranie Segues** -definiującą relacji nadrzędny podrzędny między sceny.
- **Prezentacja Segues** -definiującą tryby poszczególnych prezentacji. 

Definiując każdego elementu w ten sposób, umożliwia opóźnionego ładowania każdego elementu tylko wtedy, gdy jest to potrzebne w czasie wykonywania. W macOS, cały proces została zaprojektowana w celu umożliwiają deweloperom tworzenie złożonych, elastyczne interfejsów użytkownika, która wymaga co najmniej systemu od zera kopii kod, aby ich działania, wszystkie przy jednoczesnym zachowaniu jako efektywne z zasobów systemowych, jak to możliwe.

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>Scenorysu Szybki Start

W [scenorysu Szybki Start](~/mac/platform/storyboards/quickstart.md) przewodnik, utworzymy prostej aplikacji Xamarin.Mac, który wprowadzono podstawowe pojęcia pracy z scenorys do tworzenia interfejsu użytkownika. Przykładowa aplikacja będzie składać się uwolnić widoku zawierającego _obszaru zawartości_ i _obszaru inspektora_ i przedstawi proste okno dialogowe Preferencje. Będziemy używać Segues powiązać razem, wszystkie elementy interfejsu użytkownika.

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>Praca z Scenorys

W tej sekcji podano szczegółowe szczegóły [Praca z Scenorys](~/mac/platform/storyboards/indepth.md) w aplikacji Xamarin.Mac. Traktujemy omówiono w tle i jak składają się z kontrolerów widoku oraz widoku. Następnie przeniesiemy przyjrzeć się jak powiązane są sceny wraz z Segues. Na koniec firma Microsoft będzie Przyjrzyjmy się pracy z niestandardowych typów Segue. 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Przykład scenorysu złożonych

Na przykład złożonego przykładu pracy z Scenorys w aplikacji Xamarin.Mac zobacz [SourceWriter Przykładowa aplikacja](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter jest proste źródła edytora kodu, który zapewnia obsługę uzupełniania kodu i wyróżnianie składni proste.

Pełni komentarzem kodu SourceWriter i, jeśli jest dostępna, zostały podane linki z technologii kluczy lub metod do odpowiednich informacji w dokumentacji przewodniki Xamarin.Mac.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało krótki przegląd pracy z Scenorys w aplikacji Xamarin.Mac. Widzieliśmy, jak utworzyć nową aplikację przy użyciu scenorys oraz sposób definiowania interfejsu użytkownika. Widzieliśmy jak przechodzić między innego systemu windows i stan widoku przy użyciu segues.


## <a name="related-links"></a>Linki pokrewne

- [Witaj, Mac (przykład)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Praca z systemu Windows](~/mac/user-interface/window.md)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do systemu Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
