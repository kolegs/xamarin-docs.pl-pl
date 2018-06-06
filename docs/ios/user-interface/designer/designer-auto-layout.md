---
title: Automatycznie Rozmieść przy użyciu projektanta Xamarin dla systemu iOS
description: Ten przewodnik zawiera wprowadzenie iOS automatycznego układu oraz opisano, jak tworzyć i edytować układów Używanie ograniczeń za pomocą projektanta Xamarin dla systemu iOS. Zawiera omówienie również Modyfikowanie ograniczenia w kodzie animowanie zmian ograniczenia i inne.
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 876bf3de19d2bcce7d951facc92d5b05a928cd38
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790204"
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>Automatycznie Rozmieść przy użyciu projektanta Xamarin dla systemu iOS

Automatycznie Rozmieść (zwane również "adaptacyjną układu") jest to podejście elastyczny projekt. W przeciwieństwie do systemu układu przejściowe, gdzie każdy element lokalizacji jest ustalony na punkcie ekranu, jest automatycznie układu o *relacje* -położenia elementów do innych elementów na powierzchni projektu. Istotą układu automatycznego jest ograniczenia lub reguły określające położenie element lub zbiór elementów w kontekście innych elementów na ekranie. Ponieważ elementy nie są związane z określonej pozycji na ekranie, warunki ograniczające zmniejszają tworzenie adaptacyjnych układu, który wygląda dobrze na różnych rozmiarów ekranu i orientacji urządzenia.

W tym przewodniku wprowadzeniu ograniczeń i sposobu pracy z nimi w Xamarin iOS projektanta. Ten przewodnik nie obejmuje programowo Praca z ograniczeniami. Informacji na temat używania układu automatycznego programowo, można znaleźć w [dokumentacji firmy Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html).

## <a name="requirements"></a>Wymagania

Projektant Xamarin dla systemu iOS jest dostępna w programie Visual Studio dla komputerów Mac w programie Visual Studio 2015 i 2017 w systemie Windows.

W tym przewodniku założono znajomość projektanta składników z [wprowadzenie do projektanta dla systemu iOS](~/ios/user-interface/designer/introduction.md) przewodnik.

## <a name="introduction-to-constraints"></a>Wprowadzenie do ograniczenia

Ograniczenie jest matematyczną reprezentację relacji między dwoma elementami na ekranie. Położenie elementu jako relacja matematyczne reprezentujący interfejsu użytkownika rozwiązuje kilka problemów związanych z kodować lokalizacji elementu interfejsu użytkownika. Na przykład jeśli możemy umieścić 20px przycisk w dolnej części ekranu w trybie portret pozycji przycisku będzie wyłączone ekranu w trybie krajobraz. Aby tego uniknąć, firma Microsoft może ustawić ograniczenia, które umieszcza dolnej krawędzi 20px przycisk w dolnej części widoku. Następnie oblicza pozycji Edge przycisk *button.bottom = view.bottom - 20px*, który będzie umieścić 20px przycisk w dolnej części widoku w trybie zarówno pionowej i poziomej. Możliwość obliczyć umieszczania, na podstawie relacji matematyczne jest, co sprawia, że ograniczenia bardzo przydatne w projekcie interfejsu użytkownika.

Ustawianą ograniczenie, utworzymy `NSLayoutConstraint` obiektu, który przyjmuje jako argumenty, można ograniczyć obiekty i właściwości, lub *atrybuty*, które będą działać ograniczenia. W Projektancie iOS atrybuty obejmują krawędzi takie jak *po lewej stronie*, *prawo*, *górnej*, i *dolnej* elementu. Obejmuje to też rozmiar atrybuty takie jak *wysokość* i *szerokość*, a Centrum wskaż lokalizację, *centerX* i *centerY*. Na przykład dodając ograniczenie dotyczące pozycji od lewej granicy dwóch przycisków, Projektant generuje następujący kod w obszarze obejmuje:

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

Następna sekcja dotyczy pracy z ograniczeniami przy użyciu projektanta, w tym Włączanie i wyłączanie automatycznego układu przy użyciu narzędzi ograniczenia dla systemu iOS.

## <a name="enable-auto-layout"></a>Włączanie automatycznego układu

Domyślna konfiguracja projektanta iOS ma włączony tryb ograniczenia. Należy jednak włączyć lub wyłączyć go ręcznie, możesz to zrobić w dwóch krokach:

1.  Kliknij puste miejsce na powierzchnię projektu. To powoduje usunięcie zaznaczenia elementów i wyświetlenie właściwości dokumentu scenorysu.
1.  Zaznaczenie lub usunięcie zaznaczenia **Autolayout użyj** wyboru w panelu Właściwość:

    ![](designer-auto-layout-images/image01.png "Pole wyboru Użyj Autolayout w panelu Właściwość")


Domyślnie nie ograniczenia są utworzone lub widoczne na powierzchni. Zamiast tego one są automatycznie wywnioskować na podstawie informacji ramki w czasie kompilacji. Aby dodać ograniczenia, musimy wybierz element na powierzchni projektu i dodać ograniczeń do niego. Możemy tego za pomocą **narzędzi ograniczenia**.

## <a name="constraints-toolbar"></a>Ograniczenia paska narzędzi

 [![](designer-auto-layout-images/toolbarnew.png "Polecenia Menu kontekstowe")](designer-auto-layout-images/toolbarnew.png#lightbox)

Narzędzi ograniczenia została zaktualizowana i obecnie obejmuje dwie główne części:

- **Przełącz przycisk Tryb ograniczenia**: wcześniej, wprowadzono tryb ograniczenia, klikając ponownie wybrany widok na powierzchni projektu. Teraz należy używać tego przycisku przełącznika na pasku ograniczenia:

  ![przełączanie trybów ograniczenia](designer-auto-layout-images/constraints.png)

- **Przycisk "Aktualizacja ograniczenia":** jest należy pamiętać, że zmiany w zależności od tego, jeśli jesteś w trybie edycji ograniczenia.
  - W trybie edycji przez ograniczenie ten przycisk dopasowuje ograniczenia do dopasowania elementu ramki.
  - W trybie edycji ramce ten przycisk dopasowuje ramki elementu do dopasowania pozycji, które są definiowane ograniczenia.


## <a name="surface-based-constraint-editing"></a>Edytowanie ograniczenia na podstawie powierzchni

W poprzedniej sekcji dowiedzieliśmy się dodać domyślnego ograniczenia i usunąć ograniczenia przy użyciu narzędzi ograniczenia. Więcej edycji dostosować ograniczenia, firma Microsoft może współdziałać z ograniczeń bezpośrednio na powierzchni projektu. W tej sekcji przedstawiono podstawowe informacje dotyczące edytowania oparte na powierzchni ograniczenie, w tym formantów odstępy numeru pin, obszary upuszczania i pracy z różnymi typami warunków ograniczających.

### <a name="creating-constraints"></a>Tworzenie ograniczenia

Narzędzia projektanta iOS oferują dwa typy formanty do manipulowania elementy na powierzchnię projektu. *Przeciąganie formanty* i *formanty numeru pin odstępy*, jak pokazano na poniższej ilustracji:

![Kontrolki widoku](designer-auto-layout-images/controls.png)

Te są przełączane wybierając przycisk Tryb ograniczenia na pasku ograniczenia.

Zdefiniuj 4 dojść w kształcie T na każdej stronie elementu *górnej*, *prawo*, *dolnej*, i *po lewej stronie* krawędzi elementu dla ograniczenia. Zdefiniuj dwóch I kształcie dojść w prawo i w dolnej części elementu *wysokość* i *szerokość* ograniczenia odpowiednio. Środkowy kwadrat obsługuje zarówno *centerX* i *centerY* ograniczenia.

Aby utworzyć ograniczenia, wybierają uchwyt i przeciągnij je gdzieś na powierzchnię projektu. Po uruchomieniu przeciągania szereg zielone wierszy/pola będą wyświetlane na powierzchni informujący, co może ograniczyć. Na przykład na poniższym zrzucie ekranu, możemy są ograniczający górnej środkowego przycisku:

 [![](designer-auto-layout-images/image07.png "Ograniczający górnej środkowego przycisku")](designer-auto-layout-images/image07.png#lightbox)

Należy zauważyć trzy wiersze zielony przerywane w dwóch przycisków. Zielony linie wskazują *obszarów upuszczania*, lub innych elementów, których firma Microsoft może ograniczyć atrybuty. Na zrzucie ekranu powyżej, dwóch przycisków oferują 3 obszary upuszczania pionowy ( *dolnej*, *centerY*, *górnej*) aby ograniczyć naszych przycisku. Zielony linia przerywana w górnej części widoku oznacza kontroler widoku umożliwia ograniczenie u góry widoku i wypełniony prostokąt zielony oznacza, że kontroler widoku oferuje ograniczenia poniżej przewodnik układu top.

> [!IMPORTANT]
> Prowadnice to specjalne typy celów ograniczenia, które umożliwiają tworzenie górnej i dolnej ograniczenia, które wziąć pod uwagę obecności paski systemu, takie jak paski stanu lub paski narzędzi. Jednym z głównych zastosowań ma mieć aplikacji, które są zgodne z systemem iOS 6 lub iOS 7, ponieważ najnowsza wersja ma widok kontenera rozszerzanie poniżej paska stanu. Aby uzyskać więcej informacji na przewodnik układu top, zapoznaj się [dokumentacji firmy Apple](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2).



Trzech kolejnych sekcjach przedstawiono pracy z różnymi typami warunków ograniczających.

### <a name="size-constraints"></a>Ograniczenia rozmiaru

Z ograniczeniami rozmiar - *wysokość* i *szerokość* — dostępne są dwie opcje. Pierwsza opcja jest przeciągnij uchwyt, aby ograniczyć do rozmiaru elementu sąsiedniego, jak pokazano w przykładzie powyżej. Inną możliwością jest kliknij dwukrotnie uchwyt, aby utworzyć ograniczenia samoobsługowego. Pozwala to określić wartość stały rozmiar, jak pokazano na poniższym zrzucie ekranu:

 [![](designer-auto-layout-images/sizec.png "Przeciągnij uchwyt, aby ograniczyć do rozmiaru elementu sąsiedniego, jak pokazano w tym miejscu")](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>Ograniczenia Center

Utworzy kwadratowy uchwyt *centerX* lub *centerY* ograniczenie, w zależności od kontekstu. Przeciąganie kwadratowy uchwyt uaktywni się inne elementy do zaoferowania obydwu tych obszarach poziome i pionowe upuszczania, jak pokazano na poniższym zrzucie ekranu:

 [![](designer-auto-layout-images/centerc.png "Ograniczenia Center")](designer-auto-layout-images/centerc.png#lightbox)

Jeśli wybierzesz obszar upuszczania pionowy *centerY* można utworzyć ograniczenia. Jeśli wybierzesz obszar upuszczania poziome, na podstawie ograniczenie *centerX*.

### <a name="combinational-constraints"></a>Ograniczenia combinational

Do utworzenia zarówno wyrównanie i ograniczenia rozmiaru równości między dwoma elementami, możesz wybrać elementy z górnym pasku narzędzi, aby określić — w kolejności - wyrównanie poziome wyrównanie w pionie i equalities rozmiar, jak pokazano na poniższym zrzucie ekranu:

 [![](designer-auto-layout-images/image06.png "Ograniczenia combinational")](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>Wizualizacja i edytowania ograniczenia

Po dodaniu ograniczenie będzie wyświetlana na powierzchni projektowej jako linię niebieski po wybraniu elementu:

 [![](designer-auto-layout-images/image09.png "Wizualizacja ograniczenia")](designer-auto-layout-images/image09.png#lightbox)

Ograniczenie można wybrać, klikając niebieski wiersza i edytowania wartości ograniczenia bezpośrednio w panelu właściwość. Alternatywnie klikając niebieski wiersza zostanie wyświetlone okno popover, który umożliwia edytowanie wartości bezpośrednio na powierzchni projektu:

 [![](designer-auto-layout-images/image08.png "Edytowanie ograniczenia")](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>Ograniczenie problemów

Różne problemy mogą wystąpić podczas korzystania z ograniczeniami:

-  **Powodujące konflikt ograniczenia** — dzieje, gdy wiele ograniczeń wymusić mają sprzeczne wartości atrybutu elementu i nie może uzgodnić je aparat ograniczenia.
-  **Underconstrained elementy** — właściwości elementu (lokalizacja i rozmiar) muszą być całkowicie objęte jego zestaw ograniczeń i rozmiary wewnętrzne dla ograniczenia jest nieprawidłowy. Te wartości są niejednoznaczne, jest nazywany można underconstrained elementu.
-  **Ramka przemieszczenie** — dzieje się tak, gdy ramka elementu i jego zestaw ograniczeń zdefiniować dwa różne prostokąty wynikowy.


W tej sekcji rosnącego trzy powyższe problemy i zawiera szczegółowe informacje na temat sposobu ich obsługę.

### <a name="conflicting-constraints"></a>Powodujące konflikt ograniczenia

Powodujące konflikt ograniczenia są oznaczone na czerwono i symboli ostrzeżenie. Wyświetlenie kursora myszy nad symbole ostrzeżenie popover informacje o konflikt:

 [![](designer-auto-layout-images/image11.png "Powodujące konflikt ograniczenia ostrzeżenie")](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>Elementy underconstrained

Underconstrained elementy są wyświetlane w kolorze pomarańczowym i wyzwolić wyglądu znacznika pomarańczowy ikony na pasku obiektu kontrolera widoku:

 [![](designer-auto-layout-images/image02.png "Underconstrained elementy są wyświetlane w kolorze pomarańczowym")](designer-auto-layout-images/image02.png#lightbox)

Po kliknięciu tej ikony znacznika można uzyskać informacji o underconstrained elementów w scenie i rozwiązać problemy przez albo pełni ograniczający je lub usuwając ich ograniczenia, jak pokazano na poniższym zrzucie ekranu:

 [![](designer-auto-layout-images/image10.png "Ustalania Underconstrained elementów")](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>Przemieszczenie ramki

Przemieszczenie ramki używa tego samego kodu kolor jako underconstrained elementów. Element będzie zawsze renderowany na powierzchni przy użyciu jego natywną, ale w przypadku przemieszczenie ramki czerwonym prostokątem oznaczy, gdy element zakończą się po uruchomieniu aplikacji, jak pokazano na poniższym zrzucie ekranu:

 [![](designer-auto-layout-images/image05.png "Przykładowy widok przemieszczenie ramki")](designer-auto-layout-images/image05.png#lightbox)

Aby usunąć błędy przemieszczenie ramki, wybierz **aktualizacji ramek na podstawie ograniczeń** przycisku paska narzędzi ograniczenia (przycisk prawej):

 [![](designer-auto-layout-images/image03.png "Aktualizowanie oparte na przycisku paska narzędzi ograniczenia ramki")](designer-auto-layout-images/image03.png#lightbox)

Automatycznie dopasuje ramki elementu do dopasowania pozycji zdefiniowane przez formanty.

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>Modyfikowanie ograniczenia w kodzie

Na podstawie wymagań aplikacji, może być potrzebne do modyfikowania ograniczenia w kodzie. Na przykład aby Zmienianie rozmiaru lub położenia widoku ograniczenie jest podłączony do, aby zmienić priorytet ograniczenia lub całkowicie dezaktywować ograniczenie.

Aby uzyskać dostęp do ograniczenia w kodzie, najpierw trzeba je ujawnić w systemie iOS projektanta, wykonując następujące czynności:

1. Ograniczenia Normal (przy użyciu dowolnej z metod wymienionych powyżej).
2. W **Explorer konspektu dokumentu**, Znajdź żądane ograniczenie i wybierz ją:

    [![](designer-auto-layout-images/modify01.png "W Eksploratorze konspektu dokumentu")](designer-auto-layout-images/modify01.png#lightbox)
3. Następnie przypisać **nazwa** ograniczenia w **elementu Widget** karcie **Explorer właściwości**:

    [![](designer-auto-layout-images/modify02.png "Na karcie widżetu")](designer-auto-layout-images/modify02.png#lightbox)
4. Zapisz zmiany.

Powyższe zmiany w miejscu można dostępu ograniczenia w kodzie i zmodyfikować jego właściwości. Na przykład aby ustawić wysokości widoku podłączonego do zera może Użyj następujące:

```csharp
ViewInfoHeight.Constant = 0;
```

Podane następujące ustawienie ograniczenia w Projektancie z systemem iOS:

[![](designer-auto-layout-images/modify03.png "Edytowanie ograniczenia w Eksploratorze właściwości")](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>Odroczone przebiegu układu

Zamiast natychmiast, trwa aktualizowanie widoku dołączonych w odpowiedzi na zmiany ograniczenia planuje aparatu układu automatycznego _przebiegu układu opóźnieniem_ na najbliższą przyszłość. W trakcie tego przebiegu odroczonego nie tylko jest aktualizowany, ograniczenie danego widoku ograniczenia dla każdego widoku w hierarchii jest obliczany ponownie i zaktualizowane, aby dopasować nowy układ.

W dowolnym momencie można zaplanować przebieg układu własne opóźnieniem przez wywołanie metody `SetNeedsLayout` lub `SetNeedsUpdateConstraints` metody widoku nadrzędnego. 

Przebieg układu opóźnieniem składa się z dwóch unikatowy przechodzi przez wyświetlanie hierarchii:

- **Przebieg aktualizacji** — w tym przebiegu aparatu układu automatycznego przechodzi przez wyświetlanie hierarchii i wywołuje `UpdateViewConstraints` metody na wszystkich kontrolerach widoku i `UpdateConstraints` metody dla wszystkich widoków.
- **Przebieg układu** — ponownie, aparatu układu automatycznego przechodzi przez wyświetlanie hierarchii, ale tym razem wywołuje `ViewWillLayoutSubviews` metody na wszystkich kontrolerach widoku i `LayoutSubviews` metody dla wszystkich widoków. `LayoutSubviews` Metody aktualizacji `Frame` właściwości każdego widok podrzędny prostokąt obliczana przez aparat układu automatycznego.

### <a name="animating-constraint-changes"></a>Animowanie zmian ograniczenia

Oprócz modyfikowanie właściwości ograniczenia, umożliwia animacji Core animowanie zmian do ograniczenia widoku. Na przykład:

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

Wywołuje tutaj klucz `LayoutIfNeeded` metody widoku nadrzędnego wewnątrz bloku animacji. Ta wartość informuje widok w celu rysowania każdego "frame" animowany lokalizacji lub zmiany rozmiaru. Bez tego wiersza widok może po prostu przyciąganie do wersji ostatecznej bez animacji.

## <a name="summary"></a>Podsumowanie

W tym przewodniku wprowadzone iOS Auto (lub "adaptacyjną") układ i pojęcia ograniczenia jako matematyczne reprezentacje relacji między elementami na powierzchni projektu. On opisany sposób włączania automatycznie Rozmieść w Projektancie systemu iOS, Praca z **narzędzi ograniczenia**i edytowania ograniczenia osobno na powierzchni projektu. Następnie szczegółowo opisane jak rozwiązywać problemy z trzech typowych problemów z ograniczeniami. Na koniec go pokazano, jak modyfikować ograniczenia w kodzie.

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do scenorysów](~/ios/user-interface/storyboards/index.md)
- [iOS Designable wskazówki formantów](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Omówienie projektanta dla systemu android](~/android/user-interface/android-designer/index.md)
- [Ograniczenia programowe](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple — przewodnik układu automatycznego](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
