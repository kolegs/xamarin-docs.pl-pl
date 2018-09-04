---
title: Widoki stosu w rozszerzeniu Xamarin.iOS
description: W tym artykule opisano zarządzanie za pomocą nowego formantu UIStackView w aplikacji platformy Xamarin.iOS zestaw widoków podrzędnych albo poziomo lub pionowo ułożonymi stosu.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: a894efebe4089adefeb02007bd394c13fc77974c
ms.sourcegitcommit: 213b0315f1d6d0791e255794f87512fb253c492f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/04/2018
ms.locfileid: "34790103"
---
# <a name="stack-views-in-xamarinios"></a>Widoki stosu w rozszerzeniu Xamarin.iOS

_W tym artykule opisano zarządzanie za pomocą nowego formantu UIStackView w aplikacji platformy Xamarin.iOS zestaw widoków podrzędnych albo poziomo lub pionowo ułożonymi stosu._

> [!IMPORTANT]
> Należy pamiętać, że StackView jest obsługiwane w narzędziu iOS Designer, mogą wystąpić w użyteczność usterki przy użyciu kanału stabilne. Przełączanie kanały Beta lub Alpha powinno usunąć zakłócenia ten problem. Podjęliśmy decyzję o obecne w tym instruktażu, za pomocą środowiska Xcode, dopóki poprawki, wymagane są implementowane w kanale stabilnych.

Formant widoku stosu (`UIStackView`) wykorzystuje możliwości automatycznego układu oraz klasy rozmiaru do zarządzania stosem widoków podrzędnych, w poziomie lub pionie, który dynamicznie reaguje do orientacji i rozmiaru ekranu urządzenia z systemem iOS.

Układ wszystkich widoków podrzędnych, dołączone do widoku stosu są zarządzane przez nią na podstawie właściwości zdefiniowane dla deweloperów, takie jak osi, dystrybucja, wyrównanie i odstępy:

[![](uistackview-images/stacked01.png "Diagram układu widoku stosu")](uistackview-images/stacked01.png#lightbox)

Korzystając z `UIStackView` w aplikacji platformy Xamarin.iOS dla deweloperów można zdefiniować widoków podrzędnych albo wewnątrz scenorysu w narzędziu iOS Designer lub dodając i usuwając widoków podrzędnych w kodzie języka C#.

Ten dokument składa się z dwóch części: Szybki start ułatwiające Implementowanie wyświetlić pierwszy stosu, a następnie niektóre bardziej szczegółowe informacje techniczne dotyczące sposobu działania.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView, [platformy Xamarin University](https://university.xamarin.com/)**

## <a name="uistackview-quickstart"></a>Przewodnik Szybki Start UIStackView

Jako krótkie wprowadzenie do `UIStackView` kontroli zamierzamy utworzyć prosty interfejs, który umożliwia użytkownikowi wprowadzanie klasyfikację z zakresu od 1 do 5. Będziemy używać dwóch widoków stosu: jeden rozmieścić interfejs w pionie na ekranie i urządzenia do Rozmieść ikony ocenę od 1 do 5 w poziomie na ekranie.

### <a name="define-the-ui"></a>Definiowanie interfejsu użytkownika

Utwórz nowy projekt platformy Xamarin.iOS i edytować **Main.storyboard** pliku w program Xcode Interface Builder. Po pierwsze, przeciągnij jeden **widok pionowy stos** na **kontrolera widoku**:

[![](uistackview-images/quick01.png "Przeciągnij jeden widok pionowy stos na kontrolerze widoku")](uistackview-images/quick01.png#lightbox)

W **Inspektor atrybutu**, ustaw następujące opcje:

[![](uistackview-images/quick02.png "Ustawianie opcji widoku stosu")](uistackview-images/quick02.png#lightbox)

Gdzie:

- **Oś** — Określa, jeśli widok stosu rozmieszcza widoków podrzędnych albo **poziomo** lub **pionowo**.
- **Wyrównanie** — określa sposób wyrównania widoków podrzędnych w widoku stosu.
- **Dystrybucja** — Określa, jak o rozmiarze widoków podrzędnych w widoku stosu.
- **Odstępy** — Określa minimalny odstęp między każdym widok podrzędny w widoku stosu.
- **Względna linii bazowej** — Jeśli opcja jest zaznaczona, odstępy w pionie na każdy widok podrzędny będzie pochodną jego linii bazowej.
- **Układ marginesy względną** — umieszcza widoków podrzędnych względem marginesu standardowego układu.

Podczas pracy z widoku stosu, można traktować **wyrównanie** jako **X** i **Y** lokalizacji widok podrzędny i **dystrybucji** jako **Wysokość** i **szerokość**.

> [!IMPORTANT]
> `UIStackView` została zaprojektowana jako widok-rendering kontenera i jako takie nie jest rysowana na obszar roboczy, podobnie jak inne podklasy `UIView`. Dlatego takie jak ustawienie właściwości `BackgroundColor` lub zastępowanie `DrawRect` odniesie żadnego skutku visual.

W dalszym ciągu układ interfejsu aplikacji przez dodanie etykietę, ImageView, dwa przyciski i poziome wyświetlanie stosu, dzięki czemu jest podobny do następującego:

[![](uistackview-images/quick03.png "Układ interfejsu użytkownika widoku stosu")](uistackview-images/quick03.png#lightbox)

Widok poziomy stosu należy skonfigurować następujące opcje:

[![](uistackview-images/quick04.png "Konfigurowanie opcji widok poziomy stosu")](uistackview-images/quick04.png#lightbox)

Ponieważ nie chcemy, aby w klasyfikacji, aby być rozciągnięty ikonę, która reprezentuje każdego "point" gdy jest ona dodawana do widoku poziomym stosu ustawiliśmy **wyrównanie** do **Centrum** i  **Dystrybucja** do **wypełnienia równie**.

Na koniec Podłączanie następujące **gniazd** i **akcje**:

[![](uistackview-images/quick05.png "Gniazda widoku stosu i akcje")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>Wypełnij UIStackView z kodu

Wróć do programu Visual Studio dla komputerów Mac i Edytuj **ViewController.cs** pliku i Dodaj następujący kod:

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

Spójrzmy na kilka rodzajów ten kod szczegółowo. Po pierwsze, używamy `if` instrukcji, aby sprawdzić, czy nie ma więcej niż pięć "gwiazdy" lub mniejsza niż zero.

Aby dodać nowy "gwiazdy" możemy załadować obraz i ustaw jego **zawartości tryb** do **Dopasuj aspekt skalowania**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Dzięki temu ikony "gwiazdki" z jest zniekształcony, gdy jest ona dodawana do widoku stosu.

Następnie dodamy nową ikonę "gwiazdy" do kolekcji widoku stosu widoków podrzędnych:

```csharp
RatingView.AddArrangedSubview(icon);
```

Można zauważyć, że dodaliśmy `UIImageView` do `UIStackView`firmy `ArrangedSubviews` właściwości i nie `SubView`. Dowolny widok ma widok stosu, aby kontrolować jego układ muszą zostać dodane do `ArrangedSubviews` właściwości.

Aby usunąć widok podrzędny z widoku stosu, najpierw uzyskujemy widok podrzędny do usunięcia:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Następnie należy usunąć go z obu `ArrangedSubviews` kolekcji i w widoku Super:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Usuwanie widok podrzędny tylko `ArrangedSubviews` kolekcji przełączy go poza formant w widoku stosu, ale nie powoduje usunięcia z ekranu.

### <a name="testing-the-ui"></a>Testowanie interfejsu użytkownika

Za pomocą wszystkie wymagane elementy interfejsu użytkownika i kodu w miejscu można uruchomić i przetestować interfejs. Gdy interfejs użytkownika jest wyświetlana, wszystkie elementy w pionie widoku stosu będzie są równomiernie rozmieszczone od góry do dołu.

Kiedy użytkownik naciska **Zwiększ ocenę** przycisku "gwiazdy" zostanie dodany do ekranu (maksymalnie 5):

[![](uistackview-images/intro01.png "Uruchamianie przykładowej aplikacji")](uistackview-images/intro01.png#lightbox)

"Gwiazdy" zostanie automatycznie wyśrodkowane i równomiernie rozłożone w widoku poziomym stosu. Kiedy użytkownik naciska **Zmniejsz ocenę** przycisku "gwiazdy" zostanie usunięty (aż do Brak po lewej).

## <a name="stack-view-details"></a>Szczegóły widoku stosu

Teraz, gdy ogólne informacje o tym, co `UIStackView` formantu i jak działa, Przyjrzyjmy się bardziej niektóre jej funkcje i szczegóły.

### <a name="auto-layout-and-size-classes"></a>Automatycznego układu oraz klasy rozmiaru

Ponieważ był widoczny powyżej, gdy widok podrzędny jest dodawany do widoku stosu jego układ jest całkowicie kontrolowana przez tego widoku stosu przy użyciu automatycznego układu oraz klasy rozmiar można zmieniać położenie i rozmiar ułożonymi widoków.

Widok stosu będzie _numeru pin_ imię i nazwisko widok podrzędny w swojej kolekcji do **górnej** i **dolnej** krawędzie pionowy stos widoków lub **Left**i **po prawej stronie** krawędzie w poziome widoków stosu. Jeśli ustawisz `LayoutMarginsRelativeArrangement` właściwości `true`, a następnie widok Przypina widoków podrzędnych do odpowiednich marginesów zamiast edge.

Widok stosu używa widok podrzędny `IntrinsicContentSize` właściwości podczas obliczania rozmiaru widoków podrzędnych wzdłuż zdefiniowane `Axis` (z wyjątkiem `FillEqually Distribution`). `FillEqually Distribution` Zmienia rozmiar wszystkich widoków podrzędnych, aby były one taki sam rozmiar, dlatego wypełnianie widoku stosu wzdłuż `Axis`.

Z wyjątkiem produktów `Fill Alignment`, widok stosu używa widok podrzędny `IntrinsicContentSize` właściwości obliczania rozmiaru widoku prostopadłe do danego `Axis`. Aby uzyskać `Fill Alignment`, widoków wszystkich podrzędnych mają rozmiar, tak aby zajmowały widoku stosu prostopadłe do danego `Axis`.

### <a name="positioning-and-sizing-the-stack-view"></a>Zmienianie położenia i rozmiaru widoku stosu

Natomiast w widoku stosu całkowitej kontroli nad układ wszelkie widok podrzędny (na podstawie właściwości takich jak `Axis` i `Distribution`), nadal należy umieścić w widoku stosu (`UIStackView`) w widoku nadrzędnym za pomocą automatycznego układu oraz klas rozmiaru.

Ogólnie rzecz biorąc oznacza to, przypinanie co najmniej dwóch krawędzi widoku stosu, aby rozwinąć lub zwinąć w ten sposób definiowania położenia. Bez żadnych dodatkowych ograniczeń widoku stosu zostaną automatycznie dopasowane do wszystkich widoków jego podrzędnych w następujący sposób:

 - Rozmiar wzdłuż jej `Axis` będzie sumą wszystkich rozmiarów widok podrzędny oraz wszystkie miejsca, zdefiniowanego między każdym widok podrzędny.
 - Jeśli `LayoutMarginsRelativeArrangement` właściwość `true`, rozmiar stosu widoki będą również zawierać pokoju marginesy.
 - Rozmiar prostopadłe do `Axis` zostanie ustawiony na największą widok podrzędny w kolekcji.

Ponadto można określić ograniczenia dla widoku stosu **wysokość** i **szerokość**. W tym przypadku widoków podrzędnych układane będą (rozmiar) do wypełnienia miejsca na dysku określonego przez widok stosu, zgodnie z ustaleniami `Distribution` i `Alignment` właściwości.

Jeśli `BaselineRelativeArrangement` właściwość `true`, widoków podrzędnych układane będą oparte na pierwszy lub ostatni widok podrzędny w linii bazowej, zamiast **górnej**, **dolnej** lub **Centrum** -  **Y** pozycji. Te są obliczane na zawartość widoku stosu w następujący sposób:

 - Widok pionowy stos zwróci pierwszy widok podrzędny dla pierwszej linii bazowej, a ostatni przez ostatnie. Jeśli jedno z tych widoków podrzędnych siebie widoki stosu, ich pierwszego lub ostatniego punktu odniesienia będą używane.
 - Widok poziomy stosu użyje jej najwyższego widok podrzędny imię i nazwisko odniesienia. Jeśli najwyższego widoku jest również widokiem stosu, jego Widok najwyższego podrzędny zostanie użyty jako linii bazowej.

> [!IMPORTANT]
> Wyrównanie linii bazowej nie działa na temat rozmiarów rozciągnięty lub skompresowany widok podrzędny jako punktu odniesienia, które będą obliczane do niewłaściwej pozycji. Dla wyrównanie linii bazowej, upewnij się, że widok podrzędny **wysokość** odpowiada wewnętrzne widok zawartości **wysokość**.

### <a name="common-stack-view-uses"></a>Najczęstsze zastosowania widoku stosu

Istnieje kilka typów układu, które działają prawidłowo w widoku stosu kontrolki. Zgodnie z firmy Apple poniżej przedstawiono niektóre typowe zastosowania:

- **Zdefiniuj rozmiar wzdłuż osi** — przypinając obu krawędzi wzdłuż widoku stosu `Axis` i jednej z sąsiadujących krawędzi, aby ustawić położenie stosu widok będzie się zwiększać wzdłuż osi dopasowywane zdefiniowane przez jego widoków podrzędnych.
 -  **Zdefiniowanie widok podrzędny** — przypinając sąsiadujących krawędzi widoku stosu, aby go w widoku nadrzędnym, widok stosu będzie się zwiększać w obu wymiarach, aby dopasować jej widoków zawierających podrzędnych.
- **Zdefiniuj, rozmiar i położenie stosu** — przypinając wszystkich czterech krawędzi widoku stosu dla widoku nadrzędnego, widok stosu rozmieszcza widoków podrzędnych oparte na przestrzeni określonej w widoku stosu.
 -  **Zdefiniuj prostopadłym rozmiaru osi** — przypinając obie krawędzie prostopadle widoku stosu `Axis` i jednej krawędzi wzdłuż osi, aby ustawić położenie stosu widok będzie się zwiększać prostopadłej do osi dopasowywane zdefiniowane przez jego widoków podrzędnych.

### <a name="managing-the-appearance"></a>Zarządzanie wyglądu

`UIStackView` Została zaprojektowana jako widok-rendering kontenera i jako takie nie jest rysowana na obszar roboczy, podobnie jak inne podklasy `UIView`. Ustawianie właściwości, takie jak `BackgroundColor` lub zastępowanie `DrawRect` odniesie żadnego skutku visual.

Istnieje kilka właściwości, które kontrolują, jak widok stosu zorganizuje swoją kolekcję widoków podrzędnych:

- **Oś** — Określa, jeśli widok stosu rozmieszcza widoków podrzędnych albo **poziomo** lub **pionowo**.
- **Wyrównanie** — określa sposób wyrównania widoków podrzędnych w widoku stosu.
- **Dystrybucja** — Określa, jak o rozmiarze widoków podrzędnych w widoku stosu.
- **Odstępy** — Określa minimalny odstęp między każdym widok podrzędny w widoku stosu.
- **Względna linii bazowej** — Jeśli `true`, odstępy w pionie na każdy widok podrzędny będą pochodzić z jego linii bazowej.
- **Układ marginesy względną** — umieszcza widoków podrzędnych względem marginesu standardowego układu.

Zazwyczaj użyje widoku stosu rozmieścić niewielkiej liczby widoków podrzędnych. Bardziej złożone interfejsy użytkownika mogą być tworzone przez zagnieżdżanie jeden lub więcej widoków stosu wewnątrz siebie (jak robiliśmy [Szybki Start UIStackView](#UIStackView-Quickstart) powyżej).

Dodatkowo można dostosować wygląd interfejsów użytkownika, dodając dodatkowe ograniczenia do widoków podrzędnych (na przykład w celu sterowania szerokość lub wysokość). Jednak należy z rozwagą dołączanie powodujące konflikt ograniczeń służącym do wprowadzonych przez widok stosu, sam.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Obsługa ułożone widoków i spójności widoków Sub

Widok stosu gwarantuje, że jego `ArrangedSubviews` właściwości jest zawsze podzbiorem jego `Subviews` właściwości, używając następujących reguł:

 - Jeśli widok podrzędny jest dodawany do `ArrangedSubviews` kolekcji, jego zostanie automatycznie dodany do `Subviews` kolekcji (o ile nie jest już częścią tej kolekcji).
 - Jeśli widok podrzędny zostanie usunięty z `Subviews` kolekcji (usuwane z ekranu), jest również usuwana ze `ArrangedSubviews` kolekcji.
 - Usuwanie widok podrzędny z `ArrangedSubviews` kolekcji nie spowoduje usunięcia go z `Subviews` kolekcji. Dlatego go już nie będą rozmieszczony w widoku stosu, ale nadal będzie widoczny na ekranie.

`ArrangedSubviews` Kolekcji jest zawsze podzbiorem `Subview` kolekcji, jednak kolejność poszczególnych widoków podrzędnych w ramach każdej kolekcji jest oddzielne i kontrolowane przez następujące elementy:

 - Kolejność widoków podrzędnych w ramach `ArrangedSubviews` kolekcji określić ich kolejność wyświetlania w stosie.
 - Kolejność widoków podrzędnych w ramach `Subview` kolekcji określa ich porządku osi Z (lub warstwowe) w widoku do przodu.

### <a name="dynamically-changing-content"></a>Dynamicznie zmieniające się zawartości

Widok stosu spowoduje automatyczne dopasowanie układu widoków podrzędnych, zawsze wtedy, gdy widok podrzędny jest dodane, usunięte lub ukryte. Układ również zostaną dostosowane, jeśli jest korygowane w dowolnej właściwości widoku stosu (takie jak jego `Axis`).

Zmian układu mogą być animowane, umieszczając je w ramach bloku animacji, na przykład:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Wiele właściwości widoku stosu można określić korzystającego z klas rozmiaru w ramach scenorysu. Te właściwości zostaną automatycznie animowane jest odpowiedzi na zmiany rozmiaru i orientacji.

## <a name="summary"></a>Podsumowanie

W tym artykule został objęty nowy `UIStackView` sterowania (dla systemu iOS 9) do zarządzania zestawem widoków podrzędnych poziomo lub pionowo ułożonymi stosu w aplikacji platformy Xamarin.iOS.
Rozpoczął się prosty przykład przy użyciu widoków stosu, aby utworzyć interfejs użytkownika i zakończone z bardziej szczegółowy widok stosu, widoki i ich właściwości i funkcji.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [What's New in iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Odwołanie UIStackView](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Wprowadzenie do UIStackView (wideo)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
