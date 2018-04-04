---
title: Widok stosu
description: W tym artykule omówiono zarządzanie za pomocą nowego formantu UIStackView w aplikacji platformy Xamarin.iOS zestaw widoków podrzędnych albo stosu ułożone poziomo czy pionowo.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: aec1c4ceb562f528d6bcef7e4de0f2708952d2a5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="stack-view"></a>Widok stosu

_W tym artykule omówiono zarządzanie za pomocą nowego formantu UIStackView w aplikacji platformy Xamarin.iOS zestaw widoków podrzędnych albo stosu ułożone poziomo czy pionowo._

> [!IMPORTANT]
> Należy pamiętać, że StackView jest obsługiwana w systemie iOS projektanta, mogą wystąpić usterki użyteczność po użyciu stabilna kanału. Przełączanie kanałów w wersji Beta lub alfa powinien rozwiązanie tego problemu. Postanowiliśmy istnieje w tym przewodniku za pomocą środowiska Xcode, dopóki poprawki wymagane są implementowane w kanale stabilna.

Formant widoku stosu (`UIStackView`) korzysta z możliwości automatycznego układu oraz klasy wielkości Zarządzanie stos widoków podrzędnych, poziomej lub pionowej, który dynamicznie odpowiada na ekran i orientację rozmiar urządzenia z systemem iOS.

Układ wszystkich widoków podrzędnych dołączone do widoku stosu są zarządzane przez go na podstawie właściwości developer zdefiniowane takich jak osi, dystrybucji i wyrównanie odstępy:

[![](uistackview-images/stacked01.png "Diagram układu widoku stosu")](uistackview-images/stacked01.png#lightbox)

Korzystając z `UIStackView` w aplikacji platformy Xamarin.iOS deweloper może zdefiniować widoków podrzędnych albo wewnątrz scenorysu w systemie iOS projektanta lub przez dodawanie i usuwanie widoków podrzędnych w kodzie języka C#.

Ten dokument składa się z dwóch części: Szybki start, aby wyświetlić pierwszy stosu wdrożenie, a następnie niektórych bardziej szczegółowe informacje techniczne dotyczące sposobu działania.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView, przez [Xamarin University](https://university.xamarin.com/)**

## <a name="uistackview-quickstart"></a>UIStackView Szybki Start

Jako szybkie wprowadzenie do `UIStackView` kontroli, zamierzamy utworzyć prosty interfejs, który umożliwia użytkownikowi wprowadź klasyfikację od 1 do 5. Będziemy używać dwóch widoków stosu: jeden ułożyć interfejsu w pionie na ekranie urządzenia i je do Rozmieść ikony klasyfikacji 1 do 5 w poziomie na ekranie.

### <a name="define-the-ui"></a>Definiowanie interfejsu użytkownika

Utwórz nowy projekt platformy Xamarin.iOS i edytować **Main.storyboard** pliku w Konstruktorze interfejsu w środowisku Xcode. Najpierw przeciągnij jeden **pionowy widoku stosu** na **kontrolera widoku**:

[![](uistackview-images/quick01.png "Przeciągnij pojedynczego widoku stosu pionowego na kontroler widoku")](uistackview-images/quick01.png#lightbox)

W **inspektora atrybutu**, ustaw następujące opcje:

[![](uistackview-images/quick02.png "Ustawianie opcji widoku stosu")](uistackview-images/quick02.png#lightbox)

Gdzie:

- **Oś** — Określa, czy widok stosu rozmieszcza widoków podrzędnych albo **poziomie** lub **pionowo**.
- **Wyrównanie** — określa sposób wyrównania widoków podrzędnych w widoku stosu.
- **Dystrybucji** — określa sposób o rozmiarze widoków podrzędnych w widoku stosu.
- **Odstępy** — Określa minimalny odstęp między każdym widok podrzędny w widoku stosu.
- **Względem linii bazowej** — Jeśli zaznaczone, odstępy w pionie na każdy widok podrzędny będzie pochodzić z jego linii bazowej.
- **Układ marginesy względną** — umieszcza widoków podrzędnych względem marginesy standardowego układu.

Podczas pracy z widokiem stosu, możesz traktować **wyrównanie** jako **X** i **Y** lokalizacji widok podrzędny i **dystrybucji** jako **Wysokość** i **szerokość**.

> [!IMPORTANT]
> `UIStackView` zaprojektowano jako widok-rendering kontenera i jako taki nie jest rysowana na obszar roboczy, podobnie jak inne podklasy `UIView`. Dlatego takie jak ustawienie właściwości `BackgroundColor` lub przesłonięcie `DrawRect` nie odniesie żadnego skutku visual.

Nadal układ interfejsu aplikacji przez dodanie etykietę, ImageView, przycisków i poziomy widoku stosu, dzięki czemu jest podobny do następującego:

[![](uistackview-images/quick03.png "Układ interfejsu użytkownika widoku stosu")](uistackview-images/quick03.png#lightbox)

Konfigurowanie widoku poziomym stosu z następujących opcji:

[![](uistackview-images/quick04.png "Skonfiguruj opcje widoku poziomym stosu")](uistackview-images/quick04.png#lightbox)

Ponieważ nie chcemy, aby w klasyfikacji, aby zostać rozciągnięty ikonę, która reprezentuje każdego "punkt" po zostanie dodany do widoku poziome stosu został skonfigurowany **wyrównanie** do **Center** i  **Dystrybucji** do **wypełnienia jednakowo**.

Na koniec okablować zapasowe następujących **gniazda** i **akcje**:

[![](uistackview-images/quick05.png "Gniazda widoku stosu i akcji")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>Wypełnij UIStackView z kodu

Wróć do programu Visual Studio for Mac i edycja **ViewController.cs** pliku i Dodaj następujący kod:

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

### <a name="testing-the-ui"></a>Testowanie interfejsu użytkownika

Wszystkie wymagane elementy interfejsu użytkownika i kodu w miejscu można uruchomić i przetestować interfejs. Podczas wyświetlania interfejsu użytkownika, wszystkie elementy w pionie widoku stos będzie można równomiernie rozłożone od góry do dołu.

Po naciśnięciu **Zwiększ ocenę** przycisku "gwiazdy" zostanie dodany do ekranu (maksymalnie 5):

[![](uistackview-images/intro01.png "Przykładowa aplikacja Uruchom")](uistackview-images/intro01.png#lightbox)

"Gwiazdek" zostanie automatycznie wyśrodkowywany i równomiernie w widoku poziomym stosu. Po naciśnięciu **Zmniejsz ocenę** przycisku "gwiazdy" jest wycofany (Brak po lewej).

## <a name="stack-view-details"></a>Szczegóły widoku stosu

Teraz, gdy mamy ogólne informacje o tym, co `UIStackView` formant jest i jak działa on Spójrzmy głębiej w niektóre z jego funkcji i uzyskać szczegółowe informacje.

### <a name="auto-layout-and-size-classes"></a>Automatycznie Rozmieść i rozmiar klasy

Widzieliśmy powyżej, gdy widok podrzędny są dodawane do widoku stosu jego układ całkowicie jest kontrolowany przez tego widoku stosu przy użyciu automatycznego układu i rozmiar klasy do położenia i rozmiaru uszeregowanych widoków.

Widok stosu będzie _numeru pin_ pierwszy i ostatni widok podrzędny w swojej kolekcji, aby **górnej** i **dolnej** krawędzi dla pionowego widoków stosu lub **pozostałych**i **prawej** krawędzi poziomych widoków stosu. Jeśli ustawisz `LayoutMarginsRelativeArrangement` właściwości `true`, a następnie widok PIN widoków podrzędnych do odpowiednich marginesów zamiast krawędzi.

Widok podrzędny używa widoku stosu `IntrinsicContentSize` właściwości podczas obliczania rozmiaru widoków podrzędnych wzdłuż zdefiniowanych `Axis` (z wyjątkiem `FillEqually Distribution`). `FillEqually Distribution` Zmienia rozmiar wszystkich widoków podrzędnych, dzięki czemu są one ten sam rozmiar, w związku z tym wypełnianie widoku stosu wzdłuż `Axis`.

Z wyjątkiem produktów `Fill Alignment`, widok podrzędny używa widoku stosu `IntrinsicContentSize` właściwości obliczania rozmiaru widoku prostopadłe do danego `Axis`. Aby uzyskać `Fill Alignment`, widoków wszystkich podrzędnych mają rozmiar, tak aby zajmowały widoku stosu prostopadłe do danego `Axis`.

### <a name="positioning-and-sizing-the-stack-view"></a>Pozycjonowanie i rozmiary widoku stosu

Gdy widok stosu ma pełną kontrolę nad układem żadnych widok podrzędny (na podstawie właściwości takich jak `Axis` i `Distribution`), nadal konieczne pozycji widok stosu (`UIStackView`) w widoku nadrzędnym za pomocą klasy rozmiaru i układu automatycznego.

Ogólnie rzecz biorąc oznacza to, co najmniej dwóch krawędzi widoku stosu, aby rozwinąć lub zwinąć, w związku z tym Definiowanie położenia przypinanie. Bez dowolne dodatkowe ograniczenia widoku stosu zostaną automatycznie dopasowane do wszystkich widoków jej podrzędnych w następujący sposób:

 - Rozmiar wraz z jego `Axis` będzie suma wszystkich rozmiarów widok podrzędny i dowolnego miejsca, zdefiniowanego między każdym widok podrzędny.
 - Jeśli `LayoutMarginsRelativeArrangement` właściwość jest `true`, rozmiar stosu widoków będą także obejmować miejsca marginesy.
 - Rozmiar prostopadłe do `Axis` zostanie ustawiona do największej widok podrzędny w kolekcji.

Ponadto można określić ograniczenia widoku stosu **wysokość** i **szerokość**. W takim przypadku widoków podrzędnych układane będą (o rozmiarze) do wypełnienia obszaru określonego przez widok stosu, zgodnie z ustaleniami `Distribution` i `Alignment` właściwości.

Jeśli `BaselineRelativeArrangement` właściwość jest `true`, widoków podrzędnych układane będą oparte na pierwszej lub ostatniej widok podrzędny w linii bazowej, zamiast **górnej**, **dolnej** lub **Center* -  **Y** pozycji. Te są obliczane na zawartość widoku stosu w następujący sposób:

 - Zwraca pionowy widoku stosu pierwszy widok podrzędny dla pierwszego planu bazowego i ostatnio przez ostatnie. Jeśli jeden z tych widoków podrzędnych są widoki stosu, będzie można użyć ich pierwszej lub ostatniej linii bazowej.
 - Poziomy widoku stosu użyje jego najwyższego widok podrzędny dla podstawy imię i nazwisko. Jeśli najwyższego widok również jest widokiem stosu, jego Widok najwyższego podrzędny zostanie użyty jako linii bazowej.

> [!IMPORTANT]
> Wyrównanie linii bazowej nie działa na rozciągnięty lub skompresowanych widok podrzędny rozmiarów linii bazowej zostanie obliczony na niewłaściwym miejscu. Dla wyrównanie linii bazowej, upewnij się, że widok podrzędny **wysokość** odpowiada wewnętrzne widok zawartości **wysokość**.

### <a name="common-stack-view-uses"></a>Najczęstsze zastosowania widoku stosu

Istnieją różne typy układu, które dobrze współdziałają z kontrolki widoku stosu. Zgodnie z firmy Apple poniżej przedstawiono kilka z najczęściej zastosowań:

- **Zdefiniuj rozmiar wzdłuż osi** — przez przypinanie obu krawędzi wzdłuż widoku stosu `Axis` i jeden z sąsiadujących ze sobą krawędzi, aby umieścić stosu wzrośnie widoku osi do miejsca wynika z jego widoków podrzędnych.
 -  **Zdefiniuj pozycji widok podrzędny** — przez przypinanie sąsiadujących krawędziami widok stosu w celu jego widoku nadrzędnego, widok stosu wzrośnie w obu tych wymiarów, aby dopasować jego widoków zawierających podrzędnych.
- **Zdefiniuj rozmiar i położenie stosu** — przez przypinanie wszystkich czterech krawędzi widoku stosu dla widoku nadrzędnego, widok stosu rozmieszcza widoków podrzędnych, oparte na przestrzeni określonej w widoku stosu.
 -  **Zdefiniuj prostopadłym rozmiaru osi** — przez przypinanie obu krawędzi prostopadle do widoku stosu `Axis` i jeden krawędzi osi można ustawić pozycji stosu widok będzie rosnąć prostopadłe do osi do miejsca zdefiniowane przez jego widoków podrzędnych.

### <a name="managing-the-appearance"></a>Zarządzanie wyglądu

`UIStackView` Zaprojektowano jako widok-rendering kontenera i jako taki nie jest rysowana na obszar roboczy, podobnie jak inne podklasy `UIView`. Ustawianie właściwości, takie jak `BackgroundColor` lub przesłonięcie `DrawRect` nie odniesie żadnego skutku visual.

Istnieje kilka właściwości, które kontrolują sposób widoku stosu zorganizuje jego Kolekcja widoków podrzędnych:

- **Oś** — Określa, czy widok stosu rozmieszcza widoków podrzędnych albo **poziomie** lub **pionowo**.
- **Wyrównanie** — określa sposób wyrównania widoków podrzędnych w widoku stosu.
- **Dystrybucji** — określa sposób o rozmiarze widoków podrzędnych w widoku stosu.
- **Odstępy** — Określa minimalny odstęp między każdym widok podrzędny w widoku stosu.
- **Względem linii bazowej** — Jeśli `true`, odstępy w pionie na każdy widok podrzędny będzie pochodzić z jego linii bazowej.
- **Układ marginesy względną** — umieszcza widoków podrzędnych względem marginesy standardowego układu.

Zwykle użyjesz widoku stosu ułożyć niewielkiej liczby widoków podrzędnych. Bardziej złożone interfejsy użytkownika mogą być tworzone przez co najmniej jeden widok stosu wewnątrz siebie zagnieżdżania (jak robiliśmy [szybkiego startu UIStackView](#UIStackView-Quickstart) powyżej).

Dodatkowo można dostosować wygląd UI, dodając dodatkowe ograniczenia do widoków podrzędnych (na przykład w celu sterowania wysokość lub szerokość). Jednak z rozwagą nie, aby uwzględnić powodujące konflikt ograniczenia do wprowadzonych przez widok stosu, samej siebie.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Obsługa ułożone widoki i spójności widoków Sub

Widok stosu zagwarantuje, że jego `ArrangedSubviews` właściwości jest zawsze podzbiór jego `Subviews` właściwości za pomocą następujących reguł:

 - Jeśli widok podrzędny jest dodawany do `ArrangedSubviews` kolekcji, zostanie ona automatycznie dodana do `Subviews` kolekcji (o ile nie jest już częścią kolekcji).
 - Jeśli widok podrzędny zostanie usunięty z `Subviews` kolekcji (usuwane z ekranu), również zostanie usunięty z `ArrangedSubviews` kolekcji.
 - Usuwanie widok podrzędny z `ArrangedSubviews` kolekcji nie powoduje usunięcia go z `Subviews` kolekcji. Dlatego ten nie będzie poukładany przez widok stosu, ale nadal będą widoczne na ekranie.

`ArrangedSubviews` Kolekcji jest zawsze podzbiór `Subview` kolekcji, jednak kolejność poszczególnych widoków podrzędnych w ramach każdej kolekcji jest oddzielne i kontrolowane przez następujące czynności:

 - Kolejność widoków podrzędnych w `ArrangedSubviews` kolekcji określić kolejność ich wyświetlania w stosie.
 - Kolejność widoków podrzędnych w `Subview` kolekcji określa ich kolejności (lub tworzenie warstw) w widoku do przodu.

### <a name="dynamically-changing-content"></a>Dynamicznie zmieniające się zawartości

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

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego nowe `UIStackView` kontroli (dla systemu iOS 9), aby zarządzać zestawem widoków podrzędnych ułożone poziomo czy pionowo stosu w aplikacji platformy Xamarin.iOS.
Rozpoczął się prosty przykład Tworzenie interfejsu użytkownika przy użyciu widoków stosu i zakończeniu bardziej szczegółowy widok widoki stosu oraz ich właściwości i funkcji.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [Nowości w systemie iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Odwołanie UIStackView](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Wprowadzenie UIStackView (klip wideo)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
