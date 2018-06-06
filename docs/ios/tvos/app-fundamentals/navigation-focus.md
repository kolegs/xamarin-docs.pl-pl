---
title: Praca z systemu tvOS nawigacji i skoncentrować się w programie Xamarin
description: W tym artykule omówiono pojęcia fokus i jak jest używany do przedstawienia i obsługi nawigacji wewnątrz aplikacji Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 2af3306b605ee802b72b159d5f2759d71d292a64
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788735"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>Praca z systemu tvOS nawigacji i skoncentrować się w programie Xamarin

_W tym artykule omówiono pojęcia fokus i jak jest używany do przedstawienia i obsługi nawigacji wewnątrz aplikacji Xamarin.tvOS._


W tym artykule omówiono pojęcia [fokus](#Focus-and-Selection) i sposobie ich użycia do obsługi [nawigacji](#Navigation) w interfejsie użytkownika aplikacji Xamarin.tvOS. Zajmiemy się, jak wbudowanych systemu tvOS kontrolki używać fokusu, podświetlanie i wybór nawigację interfejsu użytkownika aplikacji Xamarin.tvOS.

[![](navigation-focus-images/intro01.png "aplikacje systemu tvOS nawigacji interfejsu użytkownika")](navigation-focus-images/intro01.png#lightbox)

Następnie przeniesiemy przyjrzeć się jak zespół może być używany z [paralaksy](#Focus-and-Parallax) i *warstwie obrazów* zapewnienie wskazówek wizualnego bieżący stan nawigacji dla użytkownika końcowego.

Na koniec przyjrzymy Praca z [fokus](#Working-with-Focus), [aktualizacje fokus](#Working-with-Focus-Updates), [przewodniki fokus](#Working-with-Focus-Guides), [fokusu w kolekcjach](#Working-with-Focus-in-Collections) i [ Włączanie paralaksy](#Enabling-Parallax) w widokach obrazu w aplikacjach Xamarin.tvOS.

<a name="Navigation" />

## <a name="navigation"></a>Nawigacji

Użytkownicy aplikacji Xamarin.tvOS nie będzie interakcja jego interfejsu bezpośrednio z systemem iOS gdzie one wybierz obrazów na ekranie urządzenia, ale pośrednio z między przy użyciu miejsca [zdalnego Siri](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote). Należy pamiętać, to podczas projektowania interfejsu użytkownika aplikacji, dzięki czemu przepływa naturalnie umożliwia zachowanie użytkownika w środowisku Apple TV, ale.

Aplikacja pomyślnie systemu tvOS implementuje nawigacji w sposób zapewniający sprawnie obsługuje celem aplikacji i struktury danych, który stanowi bez wywoływania uwagi do nawigacji, sam. Projektowanie nawigacyjny tak, aby uważa fizycznych i znanych bez zajmujących większość interfejsu użytkownika lub rysunku fokus zawartości i środowisko użytkownika aplikacji.

[![](navigation-focus-images/nav01.png "Ustawienia systemu tvOS aplikacji")](navigation-focus-images/nav01.png#lightbox)

Gdy zwykle przy użyciu firmy Apple TV, użytkownik nawiguje skumulowany zestawu ekranów, każdy przedstawiający danego zestawu zawartości. Z kolei każdy nowy ekran może prowadzić do co najmniej jednego podrzędnego ekrany zawartości przy użyciu standardowych formantów interfejsu użytkownika, takie jak [przyciski](~/ios/tvos/user-interface/buttons.md), [paski kartę](~/ios/tvos/user-interface/tab-bars.md), tabel, [widoki kolekcji](~/ios/tvos/user-interface/collection-views.md) lub [ Dzielenie widoków](~/ios/tvos/user-interface/split-views.md).

Z każdym nowym ekranie danych użytkownik przechodzi głębiej do tego stosu ekranów. Za pomocą **Menu** przycisk na komputerze zdalnym Siri można nawigowania wstecz stos, aby powrócić do poprzedniego ekranu lub Menu głównego.

Apple sugeruje, pamiętając o następujących podczas projektowania nawigacji dla aplikacji systemu tvOS:

- **Układ nawigacyjny upewnij znajdowanie Fast zawartości i łatwy** -użytkownikom dostęp do zawartości w Twojej aplikacji w najmniejszej liczby podsłuchu, klika i swipes, jak to możliwe. Uproszczenie sieci nawigacji i organizowanie zawartości do minimalnej liczby ekrany.
- **Utwórz płynu interfejsu za pomocą funkcji Touch** — upewnij się, że użytkownik może przechodzić między _Focusable elementów_ z minimalnym tarcia za pomocą najmniejszej liczby możliwych gestów.
- **Projekt z fokusem pamiętać** — od użytkownika prowadzi interakcję z zawartością w pomieszczeniu, należy przed interakcję z nią za pomocą zdalnego Siri Przenieś fokus do elementów interfejsu użytkownika. Użytkownicy będą uzyskiwać sfrustrowani z aplikacją, jeśli wymaga ona za dużo gestów dla nich ich celach.
- **Podaj wstecz nawigacji za pomocą przycisku Menu** — Aby utworzyć środowisko znane i umożliwiają użytkownikom przechodzenie wstecz przy użyciu zdalnego Siri **Menu** przycisku. Naciśnięcie przycisku **Menu** przycisk powinien zawsze powrócić do poprzedniego ekranu lub wrócić do Menu głównego aplikacji. W aplikacji najwyższego poziomu, naciskając klawisz **Menu** przycisk powinien zwrócić do ekranu Apple TV Home.
- **Zazwyczaj należy unikać wyświetlania przycisku Wstecz** — ponieważ naciśnięcie **Menu** przycisk na komputerze zdalnym Siri nawiguje wstecz na stosie ekranu, zapobiec wyświetlaniu dodatkowe formant, który stanowi duplikat tego zachowania. Wyjątek od tej reguły jest do zakupu ekrany lub ekrany z akcjami destrukcyjnego (np. usunięcie zawartości) gdzie **anulować** również powinien być wyświetlany przycisk.
- **Pokaż dużych kolekcjach na jednym ekranie, zamiast wielu** -zdalnego Siri została zaprojektowana w celu przechodzenia przez duży zbiór zawartości szybkie i łatwe za pomocą gestów. Jeśli aplikacja działa z dużą kolekcję elementów Focusable, pomyśl o pozostawieniu ich na jednym ekranie zamiast dzielenie ich na wielu ekrany, które wymagają więcej nawigacji ze strony użytkownika.
- **Używanie standardowych kontrolek nawigacji** -ponownie celu znane i użytkownika, gdy jest to możliwe, należy użyć wbudowanych `UIKit` formanty, takie jak formantów strony, paski kartę, formanty segmentu, widoków tabel, widoków kolekcji i podziału Widoki nawigacji aplikacji. Ponieważ użytkownik zna już tych elementów, intuicyjnie będą oni mogli można przejść do aplikacji.
- **Preferuj poziome zawartości nawigacji** — z powodu charakter Apple TV, szybko przesuwając od lewej do prawej na komputerze zdalnym Siri jest bardziej naturalne niż w górę i w dół. Rozważ użycie tej opcji podczas projektowania układy zawartości aplikacji.

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>Wybieranie i fokus

Na Apple TV, obraz, przycisk lub innego elementu interfejsu użytkownika jest uznawane za _aktywnego_ gdy jest to element docelowy bieżącego nawigacji.

[![](navigation-focus-images/focus01.png "Przykład fokus i zaznaczenia")](navigation-focus-images/focus01.png#lightbox)

W odróżnieniu od urządzeń z systemem iOS, gdy użytkownik bezpośrednio prowadzi interakcję z elementami na ekran dotykowy na urządzeniu użytkownicy korzystają z systemu tvOS elementy z między miejsca przy użyciu zdalnego Siri. Aby przedstawić i obsługują interakcję z użytkownikiem, używa Apple TV _fokus_ na podstawie modelu.

Za pomocą gestów i przycisk naciśnie w [zdalnego Siri](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), użytkownik przenosi fokus z jednego elementu interfejsu użytkownika do drugiego. Gdy fokus spowoduje przesunięcie do żądanego elementu, użytkownik klika polecenie Wybierz zawartość lub aktywować działania reprezentowany przez ten element.

Jako zmiany fokusu niewielkie animacje i efekty (na przykład efekt paralaksy na obrazy) są używane w celu jednoznacznego zidentyfikowania elementu interfejsu użytkownika, który aktualnie ma fokus.

Apple ma poniższe sugestie dotyczące pracy z fokusem i wybór:

- **Użyj wbudowanego kontrolek interfejsu użytkownika dla ruchu efekty** — za pomocą `UIKit` i interfejsu API fokusu w interfejsie użytkownika, Model fokus zostanie automatycznie zastosowana domyślna ruchu i efekty wizualne, do elementów interfejsu użytkownika. To sprawia, że uznać aplikacji natywnych i znany użytkownikom platformy Apple TV i umożliwia płynne i intuicyjne przepływ między elementami Focusable.
- **Przenieś fokus w kierunkach Oczekiwano** — na Apple TV niemal każdego elementu używa manipulowania pośrednich. Na przykład użytkownik używa zdalnego Siri przenosi zaznaczenie, a system automatycznie przewija interfejsu, który ma być widoczne mającym aktualnie fokus elementu. Jeśli aplikacji implementuje ten typ interakcji, upewnij się, że fokusu w kierunku gestu użytkownika. Jeśli użytkownik szybko przesuń w prawo na, fokus Siri zdalnego należy przenieść do prawej (która może spowodować ekranu do przewijania w lewo). Jedynym wyjątkiem od tej reguły jest pełny ekran używające bezpośredniej (gdzie szybko przesuwając w górę zostanie przesunięty elementu).
- **Upewnij się, że element fokus jest Obvious** -użytkowników korzysta z od afar elementów interfejsu użytkownika, dlatego jest krytyczne, aby oznaczyć element mającym aktualnie fokus. Zwykle to będzie obsługiwany automatycznie przez wbudowane `UIKit` elementów. W przypadku kontrolek niestandardowych umożliwia wyświetlanie fokus funkcji, takich jak rozmiar elementu lub w tle.
- **Użyj paralaksy, aby wprowadzić fokus elementy reakcji** — jest to mała, cykliczne gestów na komputerze zdalnym Siri spowodować łagodne, w czasie rzeczywistym przepływu ukierunkowanych elementu. To [efekt paralaksy](#Focus-and-Parallax) jest wbudowana w `UIKit` _warstwie obrazów_ umożliwiają użytkownika w pewnym sensie połączenia z elementem mającym fokus.
- **Tworzenie elementów Focusable odpowiedni rozmiar** — duże elementy z wystarczającą odstępy są łatwiejsze do wybierz i przejdź niż mniejsze elementów.
- **Projektowanie elementów interfejsu użytkownika, aby znaleźć dobrej albo fokus lub Unfocused** — zwykle Apple TV reprezentuje element fokus przez zwiększenie jego rozmiaru. Sprawdź, czy elementy interfejsu użytkownika z aplikacji wygląda świetnie w dowolnej wielkości prezentacji i, jeśli to konieczne, podaj zasoby większy rozmiar elementów.
- **Reprezentują korzystanie z zmiany fokusu** — umożliwia sprawne zanikania między elementami animacji **Focused** i **Unfocused** stanu, tak aby zachować jarring przejść z trwa.
- **Nie wyświetlaj kursora** -Użytkownicy oczekują przejść interfejsu użytkownika aplikacji przy użyciu fokus, a nie przez przeniesienie kursora po ekranie. Interfejs użytkownika zawsze należy używać modelu fokus do prezentowania spójną obsługę użytkowników.

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>Praca z fokusem

Może to być razy, które chcesz utworzyć kontrolki niestandardowej, która może stać się elementem Focusable. Jeśli więc zastąpić `CanBecomeFocused` właściwości i przywracać `true`, przeciwnym razie zwracany `false`. Na przykład:

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

W dowolnym momencie można użyć `Focused` właściwość `UIKit` formantu, aby sprawdzić, czy jest bieżącego elementu. Jeśli `true` elementu interfejsu użytkownika aktualnie ma fokus, w przeciwnym razie nie. Na przykład:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

Gdy nie można bezpośrednio Przenieś fokus do innego elementu interfejsu użytkownika za pomocą kodu, można określić elementu interfejsu użytkownika, który najpierw uzyska fokus, gdy ekran jest ładowany przez ustawienie jej `PreferredFocusedView` właściwości `true`. Na przykład:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>Praca z aktualizacjami fokus 

Gdy użytkownik powoduje fokus, które mają zostać przesunięte z jednego elementu interfejsu użytkownika do innego (na przykład w odpowiedzi na gestów na komputerze zdalnym Siri) _zdarzeń aktualizacji fokus_ zostaną wysłane do zarówno element zyska fokus, jak i element utraci fokus.

Niestandardowych elementów, które dziedziczą z `UIView` lub `UIViewController`, można zastąpić pracować ze zdarzeniem aktualizacji fokus na kilka sposobów:

- **DidUpdateFocus** — ta metoda zostanie wywołana dowolnym momencie widoku uzyska lub utraci fokus.
- **ShouldUpdateFocus** — ta metoda służy do definiowania, gdy fokus jest dozwolone przenoszenia.

Żądanie, że aparat fokus przenosi fokus z powrotem do `PreferredFocusedView` elementu interfejsu użytkownika, wywołania `SetNeedsUpdateFocus` kontrolera widoku.

> [!IMPORTANT]
> Wywoływanie `SetNeedsUpdateFocus` ma znaczenie tylko wtedy, gdy kontroler widoku jest wywoływana przed zawiera widok, który aktualnie ma fokus.




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>Praca z przewodników fokus

Aparat fokus wbudowane systemu tvOS jest doskonały na Obsługa przeniesień między elementami, które dzielą się na poziomie i w pionie siatki. Zazwyczaj należy nic nie rób ponad dodać elementy interfejsu użytkownika do projektu interfejsu i aparat fokus automatycznie będzie obsługiwać ruch między tymi elementami bez interwencji developer.

Jednak może być razy, z powodu artykuły pierwszej potrzeby projektu interfejsu użytkownika, gdy elementy interfejsu użytkownika nie można podzielić na siatce poziome i pionowe i mogą być niedostępne, ponieważ są one ukośnych ze sobą. Dzieje się tak, ponieważ aparat fokus został zaprojektowany do obsługi prostego górę, dół, lewo i w prawo ruchu między tylko elementy interfejsu użytkownika.

Wykonaj następujący układ interfejsu użytkownika, na przykład:

 [![](navigation-focus-images/guide01.png "Praca z przykład przewodniki fokus")](navigation-focus-images/guide01.png#lightbox)
 
Ponieważ **więcej informacji o** przycisku nie znajduje się na poziomie i w pionie siatkę z **kupić** przycisk jest niedostępny dla użytkownika. Jednak ten problem można łatwo rozwiązać przy użyciu _przewodnik fokus_ zapewnienie wskazówek przepływu w aparacie fokusu. 

Przewodnik fokus (`UIFocusGuide`) udostępnia niewidoczne obszaru widoku jako Focusable aparatowi fokus, dzięki czemu fokus zostać przekierowane do innego widoku.

Tak, rozwiązywać przykład powyższych następujący kod została dodana do kontrolera widoku, aby utworzyć linię fokus między **więcej informacji o** i **kupić** przyciski:

```csharp
public UIFocusGuide FocusGuide = new UIFocusGuide ();
...

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Add Focus Guide to layout
    View.AddLayoutGuide (FocusGuide);

    // Define Focus Guide that will allow the user to move
    // between the More Info and Buy buttons.
    FocusGuide.LeftAnchor.ConstraintEqualTo (BuyButton.LeftAnchor).Active = true;
    FocusGuide.TopAnchor.ConstraintEqualTo (MoreInfoButton.TopAnchor).Active = true;
    FocusGuide.WidthAnchor.ConstraintEqualTo (BuyButton.WidthAnchor).Active = true;
    FocusGuide.HeightAnchor.ConstraintEqualTo (MoreInfoButton.HeightAnchor).Active = true;
}
```

Po pierwsze, nowy `UIFocusGuide` jest tworzony i dodawany do widoku prowadnicy układu kolekcja używa `AddLayoutGuide` metody.

Następnie dostosować przewodnik fokus Top, po lewej, szerokość i wysokość kotwice względem **więcej informacji o** i **kupić** , aby umieścić go między nimi. Zobacz:

[![](navigation-focus-images/guide02.png "Przykład fokus przewodnik")](navigation-focus-images/guide02.png#lightbox)

Jest również należy pamiętać, że nowe ograniczenia są uaktywnione tworzonych przez ustawienie ich `Active` właściwości `true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

Nowe prowadnicy fokus połączenia i dodać do widoku, kontroler widoku `DidUpdateFocus` metodę można przesłonić i dodać następujący kod do przechodzenia między **więcej informacji o** i **kupić** przyciski:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    base.DidUpdateFocus (context, coordinator);

    // Get next focusable item from context
    var nextFocusableItem = context.NextFocusedView;

    // Anything to process?
    if (nextFocusableItem == null) return;

    // Decide the next focusable item based on the current
    // item with focus
    if (nextFocusableItem == MoreInfoButton) {
        // Move from the More Info to Buy button
        FocusGuide.PreferredFocusedView = BuyButton;
    } else if (nextFocusableItem == BuyButton) {
        // Move from the Buy to the More Info button
        FocusGuide.PreferredFocusedView = MoreInfoButton;
    } else {
        // No valid move
        FocusGuide.PreferredFocusedView = null;
    }
}
```

Po pierwsze, get ten kod `NextFocusedView` z `UIFocusUpdateContext` który został przekazany w (`context`). Jeśli jest to widok `null`, niezbędne jest brak przetwarzania i wyjście z metody.

Następnie `nextFocusableItem` oceny. Jeśli jest on zgodny **więcej informacji o** lub **kupić** przycisków, fokus są wysyłane do przycisku przeciwną przy użyciu przewodnika fokus `PreferredFocusedView` właściwości. Na przykład:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

W przypadku, gdy przycisk nie jest źródłem zmiany fokusu `PreferredFocusedView` wyczyścić właściwości:

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>Praca z fokusem w kolekcjach

Przy podejmowaniu decyzji, czy pojedynczy element może być focusable w `UICollectionView` lub `UITableView`, będzie można zastąpić metody `UICollectionViewDelegate` lub `UITableViewDelegate` odpowiednio. Na przykład:

```csharp
public class CardHandDelegate : UICollectionViewDelegateFlowLayout
{
    ...
    public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
    {
        if (indexPath == null) {
            return false;
        } else {
            var controller = collectionView as CardHandViewController;
            return !controller.Hand [indexPath.Row].IsFaceDown;
        }
    }
}
```

`CanFocusItem` Metoda zwraca `true` Jeśli bieżący element może być aktywny, przeciwnym razie zwraca `false`.

Jeśli chcesz `UICollectionView` lub `UITableView` do zapamiętania i przywrócić fokus do ostatniego elementu utraci i odzyskaniu przez zespół, należy ustawić `RemembersLastFocusedIndexPath` właściwości `true`.

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>Fokus i paralaksy

Jak już wspomniano, element interfejsu użytkownika jest traktowany jako _aktywnego_ gdy jest bieżący element podczas zdarzenia nawigacji. Zwykle elementu wejścia fokus zwiększają się nieznacznie jego rozmiar i jego wizualnie uprawnień przy użyciu cienia. 

Jeśli użytkownik wysyła gestu wolny, cykliczne na komputerze zdalnym Siri, element fokus zostanie omija w czasie rzeczywistym w odpowiedzi na ten ruch. Po wystąpieniu kołysanie poprzeczne oświetlonej odcieniu jest zastosowany do obrazu, jego wprowadzania powierzchni wydają się widoczne. Po upływie podanego czasu nieaktywności wygasza żadnej zawartości poza fokusu i elementu Focused wzrośnie jeszcze większym. 

Tych skutków współdziałają ze sobą zapewnienie psychicznego połączenie między zawartości ekranu TV i interakcji z Apple TV z kanapie użytkownika.

Na Apple TV efektu paralaksy jest używana w całym systemie do przedstawienia w pewnym sensie głębi 3W i dynamics elementów skoncentrować się. systemu tvOS korzysta z szeregu przezroczyste, [warstwie obrazów](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) przenosi i skaluje dynamicznie, aby utworzyć ten efekt.

Obrazy z warstwami są wymagane dla ikony aplikacji systemu tvOS i jest obsługiwana dla zawartości dynamicznej górnej półki. Podczas gdy nie jest to wymagane, Apple zdecydowanie nie zaleca się implementowania warstwie obrazów w innych elementach Focusable w Interfejsie użytkownika aplikacji.

### <a name="enabling-parallax"></a>Włączanie paralaksy

`UIImageView` Formantu (lub żadnego formantu, który dziedziczy `UIImageView`) automatycznie obsługuje efekt paralaksy. Domyślnie ta obsługa jest wyłączona, aby ją włączyć, użyj następującego kodu:

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

Ta właściwość `true`, widok obrazu automatycznie pobierze efekt paralaksy po wybraniu przez użytkownika i fokus.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego pojęcie fokus i jak jest używany do obsługi nawigacji w interfejsie użytkownika aplikacji Xamarin.tvOS. Sprawdź go wbudowanych systemu tvOS kontrolki wykorzystania fokus, podświetlanie i wybór nawigację. Następnie należy go przeglądał fokus możliwości korzystania z paralaksy i obrazy z warstwami zapewnienie wskazówek wizualnego bieżący stan nawigacji dla użytkownika końcowego. Na koniec ją zbadać pracy z fokusem, aktualizacje fokus fokusu w kolekcji i włączenie paralaksy.




## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
