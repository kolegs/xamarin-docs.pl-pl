---
title: Praca z kontrolkami segmentu
description: "Ten artykuł obejmuje projektowanie i Praca z kontrolkami Segmentowanych wewnątrz aplikacji Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: fd31413b777e1179e7f4faf6f91f91bc6c41e82b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-segmented-controls"></a>Praca z kontrolkami segmentu

_Ten artykuł obejmuje projektowanie i Praca z kontrolkami Segmentowanych wewnątrz aplikacji Xamarin.tvOS._


Segmentu kontrola zapewnia zbiór elementów liniowego, z których każdy może zawierać tekst lub ikonę i służy do zapewnienia zestawu opcji powiązanych z użytkownikiem.

[ ![](segmented-controls-images/segment01.png "Formanty segmentu próbki")](segmented-controls-images/segment01.png)

Apple ma poniższe sugestie dotyczące pracy z formantami Segmentowanych:

- **Zapewnić wystarczającą ilość miejsca** -szczególną uwagę należy podjęcia, aby zapewnić wystarczającą ilość miejsca między innymi [Focusable elementów](~/ios/tvos/app-fundamentals/navigation-focus.md) i Segmentowanych kontroli. Poszczególnych Segment staje się zaznaczony, gdy jest fokusu (nie po kliknięciu), a użytkownik może zmienić przypadkowo segmentów, chcąc faktycznie wybierz inny element Focusable w bieżącym segmencie.
- **Używanie widoków podziału filtrowania zawartości** -formanty segmentu nie podejmować decyzje dobrej zawartości filtrowania zgodnie z widokami złożonymi zostały zaprojektowane do nawigację między zawartości i filtry.
- **Limit do siedmiu Max segmentów** — należy starać się zachować maksymalną liczbę segmentów poniżej osiem (8) jako łatwiej to przeanalizować z między miejsca na kanapie i łatwiejsze w obsłudze, aby przejść.
- **Użyj spójne rozmiar zawartości segmentu** — wszystkie segmenty mają tej samej szerokości i, jeśli to możliwe, należy starać się zachować zawartość w każdym segmencie ten sam rozmiar. To nie tylko formanty segmentu staną się bardziej wizualnie czytelnych, ale ułatwia przykuć.
- **Unikaj mieszanie ikon i tekst** — każdy z segmentów poszczególnych może zawierać ikony lub tekstu, ale nie oba. Mimo że jest można mieszać zarówno ikony, jak i tekst w tej samej kontrolki segmentu, należy unikać to.

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>Temat segmentu ikon

Apple sugeruje, przy użyciu prostego, rozpoznawalnych obrazów dla ikon segmentu, takich jak Lupa wyszukiwania. Ikony zbyt skomplikowane są trudne do rozpoznania na ekranie TV w pomieszczeniu, więc warto ograniczyć ikony do reprezentacji proste.

Nie można mieszać tekst i ikony na danego segmentu i należy unikać mieszanie ikon i tekst w formancie jednego segmentu. Powinna ona wszystkie ikony lub całego tekstu.

<a name="Summary" />

## <a name="segment-text"></a>Segment tekstu

Poniższe sugestie dotyczące pracy z tekstem segmentu sprawia, że Apple:

- **Użyj krótki, łatwy do rozpoznania rzeczowniki** — tytuł segmentu powinny wyraźnie określają typ zawartości, którą użytkownik powinien oczekiwać po wybraniu danego segmentu. Na przykład: muzyka lub wideo.
- **Użyj wielkich liter w nazwach własnych** -każdego wyrazu tytuł segmentów powinien kapitalizacji z wyjątkiem artykuły, spójników i przyimki mniej niż cztery (4) znaki.
- **Użyj krótki, fokus tytułów** — Zachowaj tytuły krótko- i skupiają się na typ zawartości, można oczekiwać, gdy wybrano segmentu.

Ponownie nie można mieszać tekst i ikony na danego segmentu i należy unikać mieszanie ikon i tekst w formancie jednego segmentu.

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>Formanty segmentu i planów

Najprostszym sposobem korzystania z segmentu formantów w aplikacji Xamarin.tvOS jest dodanie ich do interfejsu użytkownika aplikacji przy użyciu projektanta dla systemu iOS.

[[ide name="xs"]]

1. W **konsoli rozwiązania**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **kontroli segmentu** z **przybornika** i upuść go w widoku: 

    [ ![](segmented-controls-images/segment02.png "Formant segmentu")](segmented-controls-images/segment02.png)
1. W **kartę Widget** z **konsoli właściwości**, można dostosować kilka właściwości formantu segmentu, takie jak jego **styl** i **stanu**: 

    [ ![](segmented-controls-images/segment03.png "Na karcie widżetu")](segmented-controls-images/segment03.png)
1. Użyj **segmentów** pole, aby kontrolować liczbę segmentów w kontrolerze.
1. Wybierz danego segmentu z **listy rozwijanej segmentu** takich jak modyfikować jego właściwości poszczególnych **tytuł** lub **obrazu** i kontroli w przypadku danego segmentu  **Włączone** lub **wybrane** gdy formant jest wyświetlany.
1. Na koniec przypisać **nazwy** do formantów, dzięki czemu można odpowiedzieć je w kodzie języka C#. Na przykład: 

    [ ![](segmented-controls-images/segment04.png "Przypisz nazwę")](segmented-controls-images/segment04.png)
1. Zapisz zmiany.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **kontroli segmentu** z **przybornika** i upuść go w widoku: 

    [ ![](segmented-controls-images/segment02-vs.png "Formant segmentu")](segmented-controls-images/segment02-vs.png)
1. W **kartę Widget** z **Explorer właściwości**, można dostosować kilka właściwości formantu segmentu, takie jak jego **styl** i **stanu**: 

    [ ![](segmented-controls-images/segment03-vs.png "Na karcie widżetu")](segmented-controls-images/segment03-vs.png)
1. Użyj **segmentów** pole, aby kontrolować liczbę segmentów w kontrolerze.
1. Wybierz danego segmentu z **listy rozwijanej segmentu** takich jak modyfikować jego właściwości poszczególnych **tytuł** lub **obrazu** i kontroli w przypadku danego segmentu  **Włączone** lub **wybrane** gdy formant jest wyświetlany.
1. Na koniec przypisać **nazwy** do formantów, dzięki czemu można odpowiedzieć je w kodzie języka C#. Na przykład: 

    [ ![](segmented-controls-images/segment04-vs.png "Przypisz nazwę")](segmented-controls-images/segment04-vs.png)
1. Zapisz zmiany.
    
-----

Aby uzyskać więcej informacji na temat pracy z Scenorys, zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>Praca z kontrolkami segmentu

Jak już wspomniano, s Segmentowanych kontroli oferuje zestaw elementów liniowego, z których każdy może zawierać tekst lub ikonę, a służy do zapewnienia zestawu opcji powiązanych z użytkownikiem.

Istnieje kilka różnych sposobów, które możesz pracować z segmentu formantów w aplikacji Xamarin.tvOS.

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>Udostępniony jako nazwy i zdarzenia

Jeśli utworzono formantu segmentu w Projektancie interfejsu i udostępniony go jako formant o nazwie i zdarzenia umożliwia następujący kod reagować na zmieniające segmentu:

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take action based on the number of players selected
    switch(PlayerCount.SelectedSegment) {
    case 0:
        // Do something if the segment is selected
        ...
        break;
    case 1:
        // Do something if the segment is selected
        ...
        break;
    case 2:
        // Do something if the segment is selected
        ...
        break;
    }
}
```

W przypadku powyższym przykładzie kontroli segmentu został udostępniony jako `PlayerCount` nazwy i `PlayerCountChanged` akcji zdarzeń. Aby uzyskać więcej informacji na temat pracy z akcjami i gniazda, zobacz [pisanie kodu z gniazda i działaniami](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) sekcji naszych [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md).

`SelectedSegment` Właściwość pobiera lub ustawia Indeks aktualnie zaznaczony segment zero (0) na podstawie. Dlatego jeśli masz pięć (5) segmentów pierwszy Segment będą indeksu (0) oraz za ostatni indeks cztery (4).

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>Modyfikowanie segmentów

W dowolnym momencie można modyfikować zawartość formantów segmentu i numeru. Aby wstawić nową ikonę segmentu, należy użyć poniższego kodu:

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

Drugi parametr określa, gdzie będzie segmentu za pomocą zero (0) oparte na indeks. Jeśli jest ostatnim parametrem `true` będzie można animować insert.

Aby usunąć danego segmentu, użyj następującego polecenia:

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

Lub następujących czynności, aby usunąć wszystkie segmenty:

```csharp
SegmentedControl.RemoveAllSegments();
```

Ponownie Jeśli ostatni parametr jest `true`, usunięcie spowoduje animowany. Użyj `NumberOfSegments` właściwości do zwrócenia bieżącą liczbę segmentów.

Aby uzyskać **tytuł** lub **ikona** danego segmentu, należy użyć następującej składni:

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

I zmienić **tytuł** lub **ikona**, należy użyć następującego:

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

Aby sprawdzić, czy danego segmentu jest **włączone**, należy użyć następującego:

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

I **Włączanie/wyłączanie** danego segmentu, należy użyć następującego:

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>Modyfikowanie wygląd formantu segmentu

Poniższy kod służy do zmiany tła danego segmentu do obrazu:

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

Gdzie `UIControlState` Określa stan formantu, który konfigurujesz obrazu jako:

- Normalny
- Wyróżnione
- Wyłączone
- Wybrane
- Fokus

I `UIBarMetrics` określa metryk, które ma być używana jako:

- Domyślny
- CD
- DefaultPrompt
- CompactPrompt

Ponadto można ustawić linię podziału między segmentami przy użyciu:

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

Gdy pierwszy `UIControlState` Określa stan segmentu z lewej strony linię podziału, a drugi `UIControlState` Określa stan segmentu z prawej strony.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z formantem Segmentowanych wewnątrz aplikacji Xamarin.tvOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
