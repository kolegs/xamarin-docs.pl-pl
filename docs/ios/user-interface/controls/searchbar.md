---
title: Paski wyszukiwania w Xamarin.iOS
description: Ten dokument zawiera opis sposobu użycia paski wyszukiwania w Xamarin.iOS. Zawarto informacje, jak utworzyć paski wyszukiwania programowo i w scenorysu.
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: cd78c58ecb119c437296a0befe1d319d8837edae
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789928"
---
# <a name="search-bars-in-xamarinios"></a>Paski wyszukiwania w Xamarin.iOS

UISearchBar służy do wyszukiwania za pomocą listy wartości. 

Ten przewodnik zawiera trzy główne składniki: 

- Pole używane do wprowadzania tekstu. Użytkownicy mogą korzystać z tutaj, aby wprowadzić swoje terminu wyszukiwania.
- Przycisk Wyczyść, aby usunąć dowolny tekst w polu wyszukiwania.
- Przycisk Anuluj, aby zakończyć działanie funkcji wyszukiwania.

![Pasek wyszukiwania](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>Implementowanie pasek wyszukiwania

Aby zaimplementować start pasek wyszukiwania przy uruchamianiu nową:

```csharp
searchBar = new UISearchBar();
```

A następnie umieść go. W poniższym przykładzie pokazano, jak można umieścić go w pasku nawigacji lub HeaderView tabeli:

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

![Właściwości pasek wyszukiwania](searchbar-images/image6.png)

Zgłoś `SearchButtonClicked` zdarzenie, gdy zostanie naciśnięty przycisk wyszukiwania. To spowoduje wywołanie logika wyszukiwania:

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

Informacje dotyczące zarządzania prezentacji pasek wyszukiwania i wyniki wyszukiwania, można znaleźć w [kontrolera wyszukiwania ](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/) przepisu.

## <a name="using-the-search-bar-in-the-designer"></a>Przy użyciu pasek wyszukiwania w Projektancie

Projektant oferuje dwie opcje realizacji pasek wyszukiwania w Projektancie

- Pasek wyszukiwania
- Pasek wyszukiwania z kontrolerem wyświetlania wyszukiwania (przestarzałe)

![Formanty paska wyszukiwania w Projektancie](searchbar-images/image2.png)

Użyj panelu Właściwości można ustawić właściwości na pasku wyszukiwania

![Projektant właściwości pasek wyszukiwania](searchbar-images/image3.png)

Poniżej opisano następujące właściwości:

- **Symbolu zastępczego, tekst monitu** — te właściwości są używane do sugerują oraz poinstruuj, jak użytkownicy powinni używać na pasku wyszukiwania. Na przykład jeśli aplikacja wyświetlane listy sklepów umożliwiają właściwość prompt poinformować, że użytkownicy mogą "Wprowadź miejscowość, Nazwa wątku lub kod pocztowy"
- **Wyszukaj styl** — można ustawić na pasku wyszukiwania albo być **Prominent** lub **minimalnego**. Przy użyciu widocznych będzie odcień wszystkich pozostałych na ekranie, z wyjątkiem wyszukiwania paska powoduje fokus jest narysowanie na pasku wyszukiwania. Styl minimalnego pasek wyszukiwania będzie blend się przy użyciu jego otoczenia.
- **Możliwości** — Włączanie tych właściwości są wyświetlane tylko element interfejsu użytkownika. Funkcje musi zostać wdrożone dla tych elementów przy wywołaniem zdarzenia poprawny zgodnie z opisem w [dokumentacja interfejsu API pasek wyszukiwania](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - Wyświetla wyniki wyszukiwania / przycisk zakładki — pokazuje wyniki wyszukiwania lub zakładki ikonę na pasku wyszukiwania
    - Przedstawia przycisk Anuluj — umożliwia użytkownikom powodować wyjście z funkcji wyszukiwania. Zaleca się, że ta opcja jest zaznaczona.
    - Pokazuje pasek zakres — dzięki temu użytkownicy mogą ograniczyć zakres wyszukiwania. Na przykład podczas wyszukiwania w aplikacji utworów muzycznych użytkownik może wybrać, czy chcą, aby wyszukać Apple muzyki lub biblioteki danego utworu lub wykonawcy. Aby wyświetlić różne opcje, Dodaj tablicę tytułów **ScopeBarTitles** właściwości.
    ![Tytuły zakresu pasek wyszukiwania](searchbar-images/image4.png)

- **Zachowanie tekstu** — opcje te są wykorzystywana do adresowania, sposób formatowania danych wejściowych użytkownika podczas wpisywania. Wielkość liter zostanie ustawiony na początku każdego słowo lub zdanie, lub każdy znak jako wielkimi literami. Korekcja i sprawdzanie pisowni z monitowanie użytkownika o sugerowane pisowni słów, ich pisania.
- **Klawiatura** — formanty styl klawiatury wyświetlana dla danych wejściowych, i w związku z tym jakie klucze są dostępne na klawiaturze. Dotyczy to również na klawiaturze numerycznej, dopełniania telefonicznej, wiadomości E-mail, adres URL wraz z innymi opcjami.
- **Wygląd** — Określa styl wyglądu klawiatury i będzie albo ciemny lub jasny motywów.
- **Zwraca klucz** — zmienić etykiety na klawisz Return w celu lepszego odzwierciedlenia, jakie działania zostaną podjęte. Obsługiwane wartości to Przejdź, sprzężenia, dalej, trasy, gotowe i wyszukiwania.
- **Zabezpieczanie** — wskazuje, czy dane wejściowe są wyświetlane jako znaki (takie jak w przypadku wprowadzania hasła).

## <a name="related-links"></a>Linki pokrewne

- [Kontroler wyszukiwania](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
