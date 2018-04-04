---
title: Praca z widokiem skumulowany
description: Ten artykuł obejmuje projektowanie i Praca z widokiem skumulowany wewnątrz aplikacji Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: a6300e4da47022199c0503e6be63b0c90f15654d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-stacked-view"></a>Praca z widokiem skumulowany

_Ten artykuł obejmuje projektowanie i Praca z widokiem skumulowany wewnątrz aplikacji Xamarin.tvOS._


Formant widoku stosu (`UIStackView`) wykorzystuje zasilania układu automatycznego i rozmiar klas, aby zarządzać stos widoków podrzędnych, poziomej lub pionowej, która odpowiada dynamicznie do zmiany zawartości i rozmiaru ekranu urządzenia Apple TV.

Układ wszystkich widoków podrzędnych dołączone do widoku stosu są zarządzane przez go na podstawie właściwości developer zdefiniowane takich jak osi, dystrybucji i wyrównanie odstępy:

[![](stacked-views-images/stacked01.png "Diagram układu widok podrzędny")](stacked-views-images/stacked01.png#lightbox)

Korzystając z `UIStackView` w aplikacji Xamarin.tvOS dewelopera można zdefiniować widoków podrzędnych albo wewnątrz scenorysu w systemie iOS projektanta lub przez dodawanie i usuwanie widoków podrzędnych w kodzie języka C#.

## <a name="about-stacked-view-controls"></a>Informacje o formantach skumulowany widoku 

`UIStackView` Zaprojektowano jako widok-rendering kontenera i jako taki nie jest rysowana na obszar roboczy, podobnie jak inne podklasy `UIView`. Ustawianie właściwości, takie jak `BackgroundColor` lub przesłonięcie `DrawRect` nie odniesie żadnego skutku visual.

Istnieje kilka właściwości, które kontrolują sposób widoku stosu zorganizuje jego Kolekcja widoków podrzędnych:

- **Oś** — Określa, czy widok stosu rozmieszcza widoków podrzędnych albo **poziomie** lub **pionowo**.
- **Wyrównanie** — określa sposób wyrównania widoków podrzędnych w widoku stosu.
- **Dystrybucji** — określa sposób o rozmiarze widoków podrzędnych w widoku stosu.
- **Odstępy** — Określa minimalny odstęp między każdym widok podrzędny w widoku stosu.
- **Względem linii bazowej** — Jeśli `true`, odstępy w pionie na każdy widok podrzędny będzie pochodzić z jego linii bazowej.
- **Układ marginesy względną** — umieszcza widoków podrzędnych względem marginesy standardowego układu.

Zwykle użyjesz widoku stosu ułożyć niewielkiej liczby widoków podrzędnych. Bardziej złożone interfejsy użytkownika mogą być tworzone przez co najmniej jeden widok stosu wewnątrz siebie zagnieżdżania.

Dodatkowo można dostosować wygląd UI, dodając dodatkowe ograniczenia do widoków podrzędnych (na przykład w celu sterowania wysokość lub szerokość). Jednak z rozwagą nie, aby uwzględnić powodujące konflikt ograniczenia do wprowadzonych przez widok stosu, samej siebie.

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>Automatycznie Rozmieść i rozmiar klasy

Gdy widok podrzędny zostanie dodany do widoku stosu jego układ całkowicie zależy od tego widoku stosu przy użyciu automatycznego układu i rozmiar klasy do położenia i rozmiaru uszeregowanych widoków.

Widok stosu będzie _numeru pin_ pierwszy i ostatni widok podrzędny w swojej kolekcji, aby **górnej** i **dolnej** krawędzi dla pionowego widoków stosu lub **pozostałych**i **prawej** krawędzi poziomych widoków stosu. Jeśli ustawisz `LayoutMarginsRelativeArrangement` właściwości `true`, a następnie widok PIN widoków podrzędnych do odpowiednich marginesów zamiast krawędzi.

Widok podrzędny używa widoku stosu `IntrinsicContentSize` właściwości podczas obliczania rozmiaru widoków podrzędnych wzdłuż zdefiniowanych `Axis` (z wyjątkiem `FillEqually Distribution`). `FillEqually Distribution` Zmienia rozmiar wszystkich widoków podrzędnych, dzięki czemu są one ten sam rozmiar, w związku z tym wypełnianie widoku stosu wzdłuż `Axis`.

Z wyjątkiem produktów `Fill Alignment`, widok podrzędny używa widoku stosu `IntrinsicContentSize` właściwości obliczania rozmiaru widoku prostopadłe do danego `Axis`. Aby uzyskać `Fill Alignment`, widoków wszystkich podrzędnych mają rozmiar, tak aby zajmowały widoku stosu prostopadłe do danego `Axis`.

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>Pozycjonowanie i rozmiary widoku stosu

Gdy widok stosu ma pełną kontrolę nad układem żadnych widok podrzędny (na podstawie właściwości takich jak `Axis` i `Distribution`), nadal konieczne pozycji widok stosu (`UIStackView`) w widoku nadrzędnym za pomocą klasy rozmiaru i układu automatycznego.

Ogólnie rzecz biorąc oznacza to, co najmniej dwóch krawędzi widoku stosu, aby rozwinąć lub zwinąć, w związku z tym Definiowanie położenia przypinanie. Bez dowolne dodatkowe ograniczenia widoku stosu zostaną automatycznie dopasowane do wszystkich widoków jej podrzędnych w następujący sposób:

* Rozmiar wraz z jego `Axis` będzie suma wszystkich rozmiarów widok podrzędny i dowolnego miejsca, zdefiniowanego między każdym widok podrzędny.
* Jeśli `LayoutMarginsRelativeArrangement` właściwość jest `true`, rozmiar stosu widoków będą także obejmować miejsca marginesy.
* Rozmiar prostopadłe do `Axis` zostanie ustawiona do największej widok podrzędny w kolekcji.

Ponadto można określić ograniczenia widoku stosu **wysokość** i **szerokość**. W takim przypadku widoków podrzędnych układane będą (o rozmiarze) do wypełnienia obszaru określonego przez widok stosu, zgodnie z ustaleniami `Distribution` i `Alignment` właściwości.

Jeśli `BaselineRelativeArrangement` właściwość jest `true`, widoków podrzędnych układane będą oparte na pierwszej lub ostatniej widok podrzędny w linii bazowej, zamiast **górnej**, **dolnej** lub **Center* -  **Y** pozycji. Te są obliczane na zawartość widoku stosu w następujący sposób:

* Zwraca pionowy widoku stosu pierwszy widok podrzędny dla pierwszego planu bazowego i ostatnio przez ostatnie. Jeśli jeden z tych widoków podrzędnych są widoki stosu, będzie można użyć ich pierwszej lub ostatniej linii bazowej.
* Poziomy widoku stosu użyje jego najwyższego widok podrzędny dla podstawy imię i nazwisko. Jeśli najwyższego widok również jest widokiem stosu, jego Widok najwyższego podrzędny zostanie użyty jako linii bazowej.

> [!IMPORTANT]
> Wyrównanie linii bazowej nie działa na rozciągnięty lub skompresowanych widok podrzędny rozmiarów linii bazowej zostanie obliczony na niewłaściwym miejscu. Dla wyrównanie linii bazowej, upewnij się, że widok podrzędny **wysokość** odpowiada wewnętrzne widok zawartości **wysokość**.




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>Najczęstsze zastosowania widoku stosu

Istnieją różne typy układu, które dobrze współdziałają z kontrolki widoku stosu. Zgodnie z firmy Apple poniżej przedstawiono kilka z najczęściej zastosowań:

- **Zdefiniuj rozmiar wzdłuż osi** — przez przypinanie obu krawędzi wzdłuż widoku stosu `Axis` i jeden z sąsiadujących ze sobą krawędzi, aby umieścić stosu wzrośnie widoku osi do miejsca wynika z jego widoków podrzędnych.
*  **Zdefiniuj pozycji widok podrzędny** — przez przypinanie sąsiadujących krawędziami widok stosu w celu jego widoku nadrzędnego, widok stosu wzrośnie w obu tych wymiarów, aby dopasować jego widoków zawierających podrzędnych.
- **Zdefiniuj rozmiar i położenie stosu** — przez przypinanie wszystkich czterech krawędzi widoku stosu dla widoku nadrzędnego, widok stosu rozmieszcza widoków podrzędnych, oparte na przestrzeni określonej w widoku stosu.
*  **Zdefiniuj prostopadłym rozmiaru osi** — przez przypinanie obu krawędzi prostopadle do widoku stosu `Axis` i jeden krawędzi osi można ustawić pozycji stosu widok będzie rosnąć prostopadłe do osi do miejsca zdefiniowane przez jego widoków podrzędnych.

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>Widoki stosu i planów

Najprostszym sposobem Praca z widokami stosu w aplikacji Xamarin.tvOS jest dodanie ich do interfejsu użytkownika aplikacji przy użyciu projektanta dla systemu iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. W **konsoli rozwiązania**, klikając dwukrotnie pozycję `Main.storyboard` pliku i otwórz go do edycji.
1. Projektowanie układu poszczególnych elementów, które chcesz dodać do widoku stosu: 

    [![](stacked-views-images/layout01.png "Przykład układu elementu")](stacked-views-images/layout01.png#lightbox)
1. Dodaj wszystkie wymagane ograniczenia do elementy, aby upewnić się, że są one poprawnie skalowania. Ten krok jest ważne, gdy element zostanie dodany do widoku stosu.
1. Aby wymagana liczba kopii (cztery w tym przypadku): 

    [![](stacked-views-images/layout02.png "Wymagana liczba kopii")](stacked-views-images/layout02.png#lightbox)
1. Przeciągnij **widoku stosu** z **przybornika** i upuść go w widoku: 

    [![](stacked-views-images/layout03.png "Widok stosu")](stacked-views-images/layout03.png#lightbox)
1. Wybierz widok stosu w **kartę Widget** z **konsoli właściwości** wybierz **wypełnienia** dla **wyrównanie**, **wypełnienia Równie** dla **dystrybucji** , a następnie wprowadź `25` dla **odstępy**: 

    [![](stacked-views-images/layout04.png "Na karcie widżetu")](stacked-views-images/layout04.png#lightbox)
1. Umieść widoku stosu na ekranie, gdzie będzie i dodać ograniczeń do przechowuj go w lokalizacji wymagane.
1. Wybierz poszczególne elementy i przeciągnij je do widoku stosu: 

    [![](stacked-views-images/layout05.png "Poszczególne elementy w widoku stosu")](stacked-views-images/layout05.png#lightbox)
1. Układ zostaną dostosowane i rozmieszczenia elementów w widoku stosu na podstawie atrybutów ustawione powyżej.
1. Przypisz **nazwy** w **kartę Widget** z **Explorer właściwości** do pracy z formantów interfejsu użytkownika w kodzie języka C#.
1. Zapisz zmiany.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. W **Eksploratora rozwiązań**, klikając dwukrotnie pozycję `Main.storyboard` pliku i otwórz go do edycji.
1. Projektowanie układu poszczególnych elementów, które chcesz dodać do widoku stosu: 

    [![](stacked-views-images/layout01.png "Przykład elementu układu")](stacked-views-images/layout01.png#lightbox)
1. Dodaj wszystkie wymagane ograniczenia do elementy, aby upewnić się, że są one poprawnie skalowania. Ten krok jest ważne, gdy element zostanie dodany do widoku stosu.
1. Aby wymagana liczba kopii (cztery w tym przypadku): 

    [![](stacked-views-images/layout02.png "Wymagana liczba kopii")](stacked-views-images/layout02.png#lightbox)
1. Przeciągnij **widoku stosu** z **przybornika** i upuść go w widoku: 

    [![](stacked-views-images/layout03-vs.png "Widok stosu")](stacked-views-images/layout03-vs.png#lightbox)
1. Wybierz widok stosu w **kartę Widget** z **Explorer właściwości** wybierz **wypełnienia** dla **wyrównanie**, **wypełnienia Równie** dla **dystrybucji** , a następnie wprowadź `25` dla **odstępy**: 

    [![](stacked-views-images/layout04-vs.png "Na karcie widżetu")](stacked-views-images/layout04-vs.png#lightbox)
1. Umieść widoku stosu na ekranie, gdzie będzie i dodać ograniczeń do przechowuj go w lokalizacji wymagane.
1. Wybierz poszczególne elementy i przeciągnij je do widoku stosu: 

    [![](stacked-views-images/layout05-vs.png "Poszczególne elementy w widoku stosu")](stacked-views-images/layout05-vs.png#lightbox)
1. Układ zostaną dostosowane i rozmieszczenia elementów w widoku stosu na podstawie atrybutów ustawione powyżej.
1. Przypisz **nazwy** w **kartę Widget** z **Explorer właściwości** do pracy z formantów interfejsu użytkownika w kodzie języka C#.
1. Zapisz zmiany.

-----

> [!IMPORTANT]
> Mimo że jest można przypisać akcje, takie jak `TouchUpInside` do elementu interfejsu użytkownika (takich jak `UIButton`) w systemie iOS projektanta podczas tworzenia program obsługi zdarzeń go zostanie nigdy nie można wywołać, ponieważ Apple TV, nie ma dotykowego ekranu lub obsługuje zdarzenia touch. Należy zawsze używać domyślnej `Action Type` podczas tworzenia akcji dla systemu tvOS elementy interfejsu użytkownika.

Aby uzyskać więcej informacji na temat pracy z Scenorys, zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md).

W przypadku naszym przykładzie firma Microsoft już widoczne gniazda i akcji dla formantu segmentu i gniazda dla każdej "karty player". W kodzie możemy Ukryj i Pokaż player w oparciu o bieżący segment. Na przykład:

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take Action based on the segment
    switch(PlayerCount.SelectedSegment) {
    case 0:
        Player1.Hidden = false;
        Player2.Hidden = true;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 1:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 2:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = true;
        break;
    case 3:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = false;
        break;
    }
}
```

Gdy aplikacja jest uruchamiana, cztery elementy będą dystrybuowane jednakowo naszym zdaniem stosu:

[![](stacked-views-images/layout06.png "Gdy aplikacja jest uruchamiana, cztery elementy będą równomiernie naszym zdaniem stosu")](stacked-views-images/layout06.png#lightbox)

Jeśli zmniejsza się liczba uczestników, nieużywane widoki są ukryte i Wyświetl stosu dopasowanie układu, aby dopasować:

[![](stacked-views-images/layout07.png "Jeśli zmniejsza się liczba uczestników, nieużywane widoki są ukryte i Wyświetl stosu dopasowanie układu, aby dopasować")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>Wypełnij widoku stosu z kodu

Oprócz całkowicie definiowanie zawartości i układu widoku stosu w systemie iOS projektanta, można tworzyć i usunąć go z dynamicznie z kodu C#.

Wykonaj poniższy przykład, który używa widoku stosu do obsługi "gwiazdek" w przeglądzie (od 1 do 5):

```csharp
public int Rating { get; set;} = 0;
...

partial void IncreaseRating (Foundation.NSObject sender) {

    // Maximum of 5 "stars"
    if (++Rating > 5 ) {
        // Abort
        Rating = 5;
        return;
    }

    // Create new rating icon and add it to stack
    var icon = new UIImageView (new UIImage("icon.png"));
    icon.ContentMode = UIViewContentMode.ScaleAspectFit;
    RatingView.AddArrangedSubview(icon);

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });

}

partial void DecreaseRating (Foundation.NSObject sender) {

    // Minimum of zero "stars"
    if (--Rating < 0) {
        // Abort
        Rating =0;
        return;
    }

    // Get the last subview added
    var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];

    // Remove from stack and screen
    RatingView.RemoveArrangedSubview(icon);
    icon.RemoveFromSuperview();

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });
}
```

Spójrzmy na kilka fragmentów kodu szczegółowo. Po pierwsze, używamy `if` instrukcje, aby sprawdzić, czy nie ma więcej niż pięciu "gwiazdek" lub mniejsza od zera.

Aby dodać nowy "gwiazdy" możemy załadować jego obrazu i ustaw jej **zawartości tryb** do **dopasowania aspekt skali**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Dzięki temu ikonę "gwiazdki" z jest zniekształcony, gdy jest ona dodawana do widoku stosu.

Następnie dodamy nową ikonę "gwiazdy" do kolekcji widoku stosu widoków podrzędnych:

```csharp
RatingView.AddArrangedSubview(icon);
```

Można zauważyć, że dodano `UIImageView` do `UIStackView`w `ArrangedSubviews` właściwości i nie `SubView`. Dowolnym widoku, które mają widoku stos, aby sterować jego układ musi zostać dodany do `ArrangedSubviews` właściwości.

Aby usunąć widok podrzędny z widoku stosu, najpierw uzyskujemy widok podrzędny do usunięcia:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Następnie należy usunąć go z obu `ArrangedSubviews` kolekcji i widoku Super:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Usuwanie widok podrzędny z właśnie `ArrangedSubviews` kolekcji przełączy go poza formantu widoku stosu, ale nie powoduje usunięcia z ekranu.

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>Dynamicznie zmieniające się zawartości

Widok stosu spowoduje automatyczne dopasowanie układu widoków podrzędnych zawsze, gdy widok podrzędny jest dodany, usunięty lub ukryte. Układ również zostaną dostosowane, jeśli żadnej właściwości widoku stosu jest korygowana (takie jak jego `Axis`).

Zmian układu można animować przez umieszczenie w bloku animacji, na przykład:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Wiele właściwości widoku stosu można określić przy użyciu rozmiaru klas w ramach scenorysu. Te właściwości zostaną automatycznie animowany jest odpowiedzi na zmiany rozmiaru i orientacji.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z widokiem skumulowany wewnątrz aplikacji Xamarin.tvOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
