---
title: Praca z kontrolera kart
description: "Ten artykuł obejmuje projektowanie i Praca z kontrolera pasek kartę wewnątrz aplikacji Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7b3a7a2347ed93aff5cddc6f15e25028c61a53d8
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-tab-bar-controller"></a>Praca z kontrolera kart

_Ten artykuł obejmuje projektowanie i Praca z kontrolera pasek kartę wewnątrz aplikacji Xamarin.tvOS._

Dla wielu typów aplikacji systemu tvOS głównej nawigacji jest przedstawiany jako pasek karty uruchomionych w górnej części ekranu. Użytkownik swipes lewy i prawy przez listę możliwych kategorii i obszar zawartości poniżej zmiany, aby odzwierciedlić wybór użytkownika.

[![](tab-bars-images/tab01.png "Przykładowe kart")](tab-bars-images/tab01.png#lightbox)

Na pasku karty jest domyślnie przezroczyste i zawsze jest wyświetlany w górnej części ekranu. Aktywnego, pasek karty obejmie top 140 pikseli ekranu, ale gdy fokus jest przenoszony do obszaru zawartości, poniżej zostanie szybko Przesuń do optymalizacji.

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>Paski kartę w systemu tvOS

`UITabViewController` Działa w podobny sposób i pełni podobne funkcje na systemu tvOS, jak w systemie iOS z następującymi różnicami klucza:

- Inaczej niż w przypadku kart w systemie iOS, która pojawia się w dolnej części ekranu pasków kartę systemu tvOS zajmują top 140 pikseli ekranu i są domyślnie przezroczyste.
- Gdy utraci fokus na pasku karty poniżej obszaru zawartości, na pasku karty zostanie szybko przesuń poza górnej części ekranu i być ukryty. Użytkownik może raz naciśnij przycisk Menu lub szybko przesuń się na [zdalnego Siri](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) pokazanie na pasku karty.
- Szybko przesuwając w dół na komputerze zdalnym Siri będzie Przenieś fokus do obszaru zawartości poniżej paska kartę do pierwszej [Focusable elementu](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) w zawartości jest pokazywany. Ponownie to zostanie ukryć pasek karty po otrzymuje fokus.
- Kliknięcie przycisku Wybierz kategorię wyświetlana na pasku karty przechodzi do tej zawartości i skoncentrować się kategorii zostanie przełączone do pierwszego elementu Focusable w tym widoku.
- Należy ustalić liczbę kategorii wyświetlane na pasku karty i wszystkie kategorie powinna być dostępna przez cały czas, danej kategorii nigdy nie powinna być wyłączona.
- Karta paski nie obsługują dostosowywania na systemu tvOS. Ponadto nie są widoczne **więcej** kategorii (na przykład iOS) Jeśli istnieją kategorie więcej niż można zmieścić w pasku karty.

Apple ma poniższe sugestie dotyczące pracy z paskami karty:

- **Użyj karty paski logicznie organizowanie zawartości** -organizację aplikację systemu tvOS współpracuje z zawartości za pomocą paska kartę. Na przykład duży, wykresy Top, zakupione i wyszukiwania.
- **Dodaj identyfikatory do poinformować użytkowników o nową zawartość** — Opcjonalnie można wyświetlić wskaźnika (czerwony oval z liczbą białe lub wykrzyknik) informują użytkownika o nową zawartość w kategorii.
- **Identyfikatory efektów** — nie zajmowały kart z identyfikatory i wyświetlać tylko ich gdzie zapewniają kluczowych informacji do użytkownika.
- **Ogranicz liczbę kategorii** — Aby zmniejszyć złożoność i umożliwia aplikacji można zarządzać, nie przeciążać pasku karty z kategorii i upewnij się, że wszystkie kategorie są widoczne i nie wiele. Najlepiej proste, krótki tytułów.
- **Nie należy wyłączać kategorię** -wszystkich kartach (kategorie) powinny być widoczne i włączone przez cały czas. Jeśli w danej karcie nie ma zawartości, dlaczego dostarcza wyjaśnienie dla użytkownika. Na przykład karta zakupy jest pusta, jeśli użytkownik nie dokonał zakupów.

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>Elementy kart

Każda kategoria (Tab) na pasku karty jest reprezentowany przez element paska karty (`UITabBarItem`). Apple ma poniższe sugestie dotyczące pracy z elementami paska kartę:

- **Użyj karty na podstawie tekstu** — gdy element paska karty jest może być reprezentowany jako ikona, Apple sugeruje przy użyciu tekstu, tylko ponieważ zwięzły tytuł jest łatwiej interpretować niż ikona.
- **Użyj krótki, łatwy do rozpoznania rzeczowniki lub zleceń** — element paska kartę A wyraźnie należy przekazywać zawartość, która nie zawiera i działa najlepiej, gdy jest proste rzeczownik (na przykład fotografii, filmy lub utworów muzycznych) lub zlecenia (na przykład wyszukiwania lub Play).

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>Karta paski i planów

Najprostszym sposobem Praca z paskami kartę w aplikacji Xamarin.tvOS jest dodanie ich do interfejsu użytkownika aplikacji przy użyciu projektanta dla systemu iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. Uruchom nową aplikację Xamarin.tvOS i wybierz **systemu tvOS** > **aplikacji** > **aplikacji z kartami**: 

    [![](tab-bars-images/tab02.png "Wybierz aplikację z kartami")](tab-bars-images/tab02.png#lightbox)
1. Postępuj zgodnie z monitami, aby utworzyć nowe rozwiązanie Xamarin.tvOS wszystkie.
1. W **konsoli rozwiązania**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Aby zmienić **ikona** lub **tytuł** dla danej kategorii, wybierz **element paska kartę** dla **kontrolera widoku** w  **Konspekt dokumentu**:

    [![](tab-bars-images/tab03a.png "Na pasku karty elementu kontrolera widoku konspektu dokumentu")](tab-bars-images/tab03a.png#lightbox)
1. Następnie ustaw właściwości wymaganych **kartę Widget** z **Explorer właściwości**: 

    [![](tab-bars-images/tab03.png "Na karcie widżetu")](tab-bars-images/tab03.png#lightbox)
1. Aby dodać nową kategorię (Tab), należy porzucić **kontrolera widoku** na Twoje powierzchnię projektu: 

    [![](tab-bars-images/tab04.png "Kontroler widoku")](tab-bars-images/tab04.png#lightbox)
1. Formant kliknij i przeciągnij od **kontrolera widoku kartę** do nowego **kontrolera widoku**.
1. Wybierz z menu podręcznego **wyświetlić kontrolerów** Aby dodać nowy widok jako karty (kategoria): 

    [![](tab-bars-images/tab05.png "Wybierz kartę")](tab-bars-images/tab05.png#lightbox)
1. Zaprojektować układ interfejsu użytkownika dla każdego obszaru zawartości Caterogies normalnie, dodając elementy interfejsu użytkownika w systemie iOS projektanta.
1. Ujawnia żadnych zdarzeń wymagane, aby pracować z formantów interfejsu użytkownika w kodzie języka C#.
1. Nazwy formantów interfejsu użytkownika, które chcesz udostępnić w kodzie języka C#.
1. Zapisz zmiany.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Uruchom nową aplikację Xamarin.tvOS i wybierz **systemu tvOS** > **aplikacji** > **aplikacji z kartami**: 

    [![](tab-bars-images/tab02vs.png "Wybierz aplikację z kartami")](tab-bars-images/tab02vs.png#lightbox)
1. Postępuj zgodnie z monitami, aby utworzyć nowe rozwiązanie Xamarin.tvOS wszystkie.
1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Aby zmienić **ikona** lub **tytuł** dla danej kategorii, wybierz **element paska kartę** dla **kontrolera widoku** w  **Konspekt dokumentu**:

    [![](tab-bars-images/tab03avs.png "Kontroler widoku konspektu dokumentu")](tab-bars-images/tab03avs.png#lightbox)
1. Następnie ustaw właściwości wymaganych **kartę Widget** z **Explorer właściwości**: 

    [![](tab-bars-images/tab03vs.png "Na karcie widżetu")](tab-bars-images/tab03vs.png#lightbox)
1. Aby dodać nową kategorię (Tab), przeciągnij **kontrolera widoku** z **przybornika** i upuść ją z powierzchni projektu: 

    [![](tab-bars-images/tab04vs.png "Kontroler widoku")](tab-bars-images/tab04vs.png#lightbox)
1. Formant kliknij i przeciągnij od **kontrolera widoku kartę** do nowego **kontrolera widoku**.
1. Wybierz z menu podręcznego **wyświetlić kontrolerów** Aby dodać nowy widok jako karty (kategoria): 

    [![](tab-bars-images/tab05vs.png "Wybierz kartę")](tab-bars-images/tab05vs.png#lightbox)
1. Zaprojektować układ interfejsu użytkownika dla każdego obszaru zawartości Caterogies normalnie, dodając elementy interfejsu użytkownika w systemie iOS projektanta.
1. Ujawnia żadnych zdarzeń wymagane, aby pracować z formantów interfejsu użytkownika w kodzie języka C#.
1. Nazwy formantów interfejsu użytkownika, które chcesz udostępnić w kodzie języka C#.
1. Zapisz zmiany.
    
-----

> [!IMPORTANT]
> Gdy jest można przypisać zdarzenia, takie jak `TouchUpInside` do elementu interfejsu użytkownika (takich jak `UIButton`) w systemie iOS projektanta go zostanie nigdy nie można wywołać, ponieważ Apple TV, nie ma dotykowego ekranu lub obsługuje zdarzenia touch. Zawsze należy używać `Primary Action ` zdarzenie, gdy tworzenie obsługi zdarzeń dla systemu tvOS elementy interfejsu użytkownika.

Aby uzyskać więcej informacji na temat pracy z Scenorys, zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>Praca z paskami kartę

Użyj `Items` właściwość `UITabBar` można uzyskać dostępu do kolekcji z `UITabBarItems` zawiera jako zero (0) tablicy indeksowanej. `SelectedItem` Właściwości zwróci aktualnie wybrana karta (kategoria) jako `UITabBarItem`.


<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>Praca z elementami kart

Aby wyświetlić wskaźnika na danej karcie (czerwony oval tekstem biały), należy użyć poniższego kodu:

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

Które dałby w efekcie uruchamianych następujące wyniki:

[![](tab-bars-images/tab06.png "Element paska karty z wskaźnika")](tab-bars-images/tab06.png#lightbox)

Użyj `Title` właściwość `UITabBarItem` zmienić tytuł i `Image` właściwości, aby zmienić ikony.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z kontrolera pasek kartę wewnątrz aplikacji Xamarin.tvOS.




## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
