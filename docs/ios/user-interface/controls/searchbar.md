---
title: Paski wyszukiwania w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano sposób użycia paski wyszukiwania w rozszerzeniu Xamarin.iOS. Omówiono w nim sposób tworzenia paski wyszukiwania, programowe i scenorysu.
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: fdd9fe647f1a2f63b2a86a64ad92d1e71d6fcd2e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242082"
---
# <a name="search-bars-in-xamarinios"></a>Paski wyszukiwania w rozszerzeniu Xamarin.iOS

UISearchBar umożliwia przeszukiwanie listy wartości. 

Zawiera trzy główne składniki: 

- Pole używane do wprowadzania tekstu. Użytkownicy mogą wykorzystywać ten element, aby wprowadzić ich termin wyszukiwania.
- Przycisk Wyczyść, aby usunąć dowolny tekst z pola wyszukiwania.
- Przycisk Anuluj, aby zakończyć działanie funkcji wyszukiwania.

![Pasek wyszukiwania](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>Implementowanie pasek wyszukiwania

Aby zaimplementować start pasek wyszukiwania przez utworzenie wystąpienia nowy:

```csharp
searchBar = new UISearchBar();
```

A następnie umieść go. W poniższym przykładzie pokazano, jak umieścić go na pasku nawigacyjnym lub w HeaderView tabeli:

```csharp
NavigationItem.TitleView = searchBar;

\\or

TableView.TableHeaderView = searchBar;
```

Ustawianie właściwości na pasku wyszukiwania:

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![Właściwości paska wyszukiwania](searchbar-images/image6.png)

Wywoływanie `SearchButtonClicked` zdarzeń po naciśnięciu przycisku wyszukiwania. Wywoła to logika wyszukiwania:

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

Aby uzyskać informacji o zarządzaniu prezentacji pasek wyszukiwania i wyniki wyszukiwania, zobacz [kontrolera wyszukiwania ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller) przepisu.

## <a name="using-the-search-bar-in-the-designer"></a>Korzystając z paska wyszukiwania w Projektancie

Projektant oferuje dwie opcje dotyczące implementowania pasek wyszukiwania w Projektancie

- Pasek wyszukiwania
- Pasek wyszukiwania z kontrolerem ekran wyszukiwania (przestarzałe)

![Formanty paska wyszukiwania w Projektancie](searchbar-images/image2.png)

Ustaw właściwości na pasku wyszukiwania za pomocą panelu Właściwości

![Projektant właściwości pasek wyszukiwania](searchbar-images/image3.png)

Te właściwości są wyjaśnione poniżej:

- **Tekst symbolu zastępczego wiersza** — te właściwości są używane do zasugerować i wydać, jak użytkownicy powinni używać na pasku wyszukiwania. Na przykład jeśli aplikacji wyświetlona lista magazynów można użyć właściwości monitu informujące, że użytkownicy mogą "wpisać miasta, Nazwa wątku lub kod pocztowy"
- **Wyszukaj styl** — można ustawić na pasku wyszukiwania, aby być **Prominent** lub **minimalny**. Przy użyciu widocznych będzie odcień wszystkich elementów na ekranie, z wyjątkiem wyszukiwania paska, powodując fokus do rysowania na pasku wyszukiwania. Na pasku wyszukiwania minimalny styl będzie mieszają się z jego otoczenia.
- **Możliwości** — Włączanie tych właściwości są wyświetlane tylko element interfejsu użytkownika. Funkcje muszą być zaimplementowane dla tych, tworząc poprawnego zdarzenia zgodnie z opisem w [dokumentacji interfejsu API pasek wyszukiwania](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - Wyświetla wyniki wyszukiwania / przycisk zakładek — ikona wyniki wyszukiwania lub zakładkach jest widoczny na pasku wyszukiwania
    - Przedstawia przycisk Anuluj — umożliwia użytkownikom wyjście z funkcji wyszukiwania. Zaleca się, że ta opcja jest zaznaczona.
    - Przedstawia pasek zakresu — dzięki temu użytkownicy mogą ograniczyć zakres wyszukiwania. Na przykład podczas wyszukiwania w aplikacji music użytkownik może wybrać, czy mają być wyszukiwanie Apple Music lub ich biblioteki dla danego utworu lub wykonawcy. Aby wyświetlić różne opcje, Dodaj tablicę tytułów do **ScopeBarTitles** właściwości.
    ![Tytuły zakresu pasek wyszukiwania](searchbar-images/image4.png)

- **Zachowanie tekstu** — opcje te są wykorzystywana do adresowania, sposób formatowania danych wejściowych użytkownika podczas wpisywania. Wielkość liter zostanie ustawiony na początku każdego słowo lub zdanie, lub każdy znak jako wielkimi literami. Poprawianie i sprawdzania pisowni z monitować użytkownika o pisowni sugerowanych wyrazów, podczas ich wpisywania.
- **Klawiatura** — kontrolki stylu klawiatury wyświetlana dla danych wejściowych i w związku z tym jakie klucze są dostępne na klawiaturze. W tym na klawiaturze numerycznej, konsola telefonicznej, wiadomości E-mail, adres URL wraz z innymi opcjami.
- **Wygląd** — Określa styl wyglądu klawiatury i będzie albo ciemny lub jasny motywów.
- **Klawisz Return** — Zmiana etykiety na klawisz ENTER w celu lepszego odzwierciedlenia, jakie działania zostaną podjęte. Obsługiwane wartości to z rzeczywistym użyciem, sprzężenia, dalej, trasy, wykonywane i wyszukiwania.
- **Zabezpieczanie** — wskazuje, czy dane wejściowe są wyświetlane jako znaki (np. wprowadzenie hasła).

## <a name="related-links"></a>Linki pokrewne

- [Kontroler wyszukiwania](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
