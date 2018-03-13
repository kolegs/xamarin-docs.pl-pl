---
title: "Materiał projektowe"
description: "W tym temacie opisano funkcje projektanta, które ułatwiają deweloperom tworzenie układów materiału projektowania CLS. Ta sekcja zawiera wprowadzenie oraz wyjaśniono, jak użyć siatki materiałów, paletę kolorów materiały związane z typografią skali i Edytor motywów."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 9c1797398fba580ab7f34526b10e1da455eb2dc5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="material-design-features"></a>Materiał projektowe

_W tym temacie opisano funkcje projektanta, które ułatwiają deweloperom tworzenie układów materiału projektowania CLS. Ta sekcja zawiera wprowadzenie oraz wyjaśniono, jak użyć siatki materiałów, paletę kolorów materiały związane z typografią skali i Edytor motywów._


> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**Rozwijać 2016: Wszyscy tworzenie doskonałych aplikacji z materiału projektu**

## <a name="overview"></a>Omówienie

Projektant Xamarin.Android zawiera funkcje, które ułatwiają tworzenie układów materiałów projektu CLS. Jeśli nie masz doświadczenia z projektu materiałów, zobacz [wprowadzenie projektowania materiałów](https://www.google.com/design/spec/material-design/introduction.html).

W tym przewodniku będziesz mamy przyjrzeć się następujące funkcje projektanta:

-   *Siatka materiału* &ndash; nakładki na powierzchni projektu, pokazujący siatki, odstępy i styku ułatwiające umieść elementy widget układu zgodnie z wytycznymi projektowania materiału.

-   *Palety kolorów materiału* &ndash; okno dialogowe właściwości Konsola ułatwia wybór koloru z oficjalnego palety materiałów projektu.

-   *Związane z typografią skali* &ndash; Konsola właściwości okna dialogowego, który umożliwia wybór materiału projektowania CLS ustawienia dla `textAppearance` właściwości pola tekstowego.

-   *Edytor motywów* &ndash; Edytor zasobów kolorów, które umożliwia ustawianie informacji o kolorze do podzbioru motywu. Na przykład można wyświetlić podgląd i modyfikowanie materiału kolorów, takich jak `colorPrimary`, `colorPrimaryDark`, i `colorAccent`.

Firma Microsoft będzie wygląd w każdej z tych funkcji i zawierają przykłady sposobu ich używania.



## <a name="material-design-grid"></a>Siatki materiału projektu

Materiał siatki projektu jest dostępne na pasku narzędzi u góry projektanta:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Materiał siatki projektu](material-design-features-images/vs/01-material-design-grid-sml.png)](material-design-features-images/vs/01-material-design-grid.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Materiał siatki projektu](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

-----

Po kliknięciu ikony siatki projektu materiałów, Projektant wyświetla nakładki na powierzchni projektu, który obejmuje następujące elementy:

-   Styku (linie pomarańczowy)

-   Odstęp (obszary zielony)

-   Siatki (linie niebieski)

Te elementy są widoczne w followng zrzut ekranu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wyróżnienie, odstępy i siatki](material-design-features-images/vs/02-grid-and-keylines-sml.png)](material-design-features-images/vs/02-grid-and-keylines.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Wyróżnienie, odstępy i siatki](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Konfiguruje się każdy z tych elementów nakładki. Po kliknięciu przycisku wielokropka obok menu siatki projektu materiałów popover okna dialogowego otwiera umożliwiający Włącz/Wyłącz siatki, skonfigurowania położenie styku i długości. Należy pamiętać, że wszystkie wartości są wyrażane w `dp` (w pikselach niezależnych od gęstość):

![Siatki, wyróżnienie i odstępy konfiguracji](material-design-features-images/vs/03-grid-configuration.png)

Aby dodać nowy wyróżnienie, wprowadź nową wartość przesunięcia w **przesunięcie** wybierz lokalizację (**po lewej stronie**, **górnej**, **prawo**, lub  **dolny**) i kliknij pozycję + ikonę, aby dodać nowy wyróżnienie.

Podobnie, aby dodać nowy odstępów, wprowadź rozmiar i przesunięcie (w punkcie dystrybucji) do **rozmiar** i **przesunięcie** pola odpowiednio. Wybierz lokalizację (**po lewej stronie**, **górnej**, **prawo**, lub **dolnej**) i kliknij pozycję + ikonę, aby dodać nowy odstępy.

Zmiana tych wartości konfiguracji są zapisywane w pliku XML układu i ponownie używana po ponownym otwarciu układu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Konfiguruje się każdy z tych elementów nakładki. Po kliknięciu trójkąt dół obok menu siatki projektu materiałów popover okna dialogowego otwiera umożliwiający Włącz/Wyłącz siatki, skonfigurowania położenie styku i długości. Należy pamiętać, że wszystkie wartości są wyrażane w `dp` (w pikselach niezależnych od gęstość):

[![Siatki, wyróżnienie i odstępy konfiguracji](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

Aby dodać nowy wyróżnienie, wprowadź nową wartość przesunięcia w **przesunięcie** wybierz lokalizację (**po lewej stronie**, **górnej**, **prawo**, lub  **dolny**) i kliknij pozycję + ikonę, aby dodać nowy wyróżnienie.

Podobnie, aby dodać nowy odstępów, wprowadź rozmiar i przesunięcie (w punkcie dystrybucji) do **rozmiar** i **przesunięcie** pola odpowiednio. Wybierz lokalizację (**po lewej stronie**, **górnej**, **prawo**, lub **dolnej**) i kliknij pozycję + ikonę, aby dodać nowy odstępy.

Zmiana tych wartości konfiguracji są zapisywane w pliku XML układu i ponownie używana po ponownym otwarciu układu.


## <a name="material-design-color-palette"></a>Palety kolorów materiału projektu

Każdy element panelu Właściwości, który akceptuje teraz kolor ma dodatkowe ikonę, która umożliwia otworzyć paletę kolorów projektowania materiałów, jak pokazano w tym zrzucie ekranu pokazano:

[![Kolor ikony](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

Po kliknięciu tej ikony spowoduje otwarcie popover okno dialogowe umożliwia konfigurowanie kolor tej właściwości z palety kolorów materiałów projektu:

[![Materiał paletę kolorów projektu](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

Górnej części palety kolorów przedstawia podstawowy kolory materiałów projektu podczas dolnej części palety wyświetla zakres barw dla wybranych kolorów podstawowych. Na przykład po wybraniu **indygo**, Kolekcja **indygo** barwy są wyświetlane u dołu okna dialogowego.
Po wybraniu hue kolor właściwości jest zmieniany na wybranych hue. W poniższym przykładzie `Background Tint` przycisku jest zmieniana na *indygo 500*:

[![Wybierz indygo 500](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint` ustawiono kod kolor *indygo 500* (`#ff3f51b5`), i projektanta aktualizuje kolor tła przycisku w celu odzwierciedlenia tej zmiany:

[![Zmiany odcień tła](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

Aby uzyskać więcej informacji dotyczących projektowania materiałów paletę kolorów, zobacz projekt materiałów [kolorów palety przewodnik](http://www.google.com/design/spec/style/color.html#color-color-palette).

## <a name="typographic-scale"></a>Skala związane z typografią

**Wygląd tekstu** sekcji **właściwości** konsoli **styl** karta zawiera ikonę, która umożliwia select z `TextAppearance` styl, który odpowiada wymaganiom projektu materiałów Specyfikacja:

[![Karta stylów](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

Po kliknięciu tej ikony spowoduje to otwarcie **związane z typografią skali** popover okna dialogowego, które wyświetla listę style tekstu wstępnie skonfigurowane, które są dostępne:

[![Selektor stylu tekstu](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

W poniższym przykładzie, klikając pozycję **ekran 1** zmienia tekst przycisku do większych czcionki **ekran 1**:

[![Styl wyświetlania 1](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

Styl tekstu w **związane z typografią skali** następuje okna dialogowego **motyw** ustawienie. Na przykład jeśli **światła** motywu jest wybierany w projektancie, lista wstecznych style tekstu dostępne **światła** motywu:

[![Motywu jasny](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

-----



## <a name="theme-editor"></a>Edytor motywów

**Edytor motywów** umożliwia dostosowanie informacji o kolorze do podzbioru atrybutów motywu. Aby otworzyć **Edytor motywów**, kliknij ikonę pędzla na pasku narzędzi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ikona Edytor motywów](material-design-features-images/vs/04-theme-editor-icon.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Ikona Edytor motywów](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

-----

Mimo że **Edytor motywów** dotyczy dostępny na pasku narzędzi target wszystkie wersje systemu Android i poziomy interfejsu API tylko podzestaw możliwości opisane poniżej są dostępne, jeśli poziom docelowego interfejsu API jest starsza niż 21 interfejsu API (Android 5.0 Interfejs typu lizak).

Na lewym panelu **Edytor motywów** Wyświetla listę kolorów, które tworzą aktualnie wybranego motywu (w tym przykładzie używamy `Default Theme`):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Edytor motywów](material-design-features-images/vs/05-theme-editor-sml.png)](material-design-features-images/vs/05-theme-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Edytor motywów](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

-----

Po wybraniu kolor po lewej stronie po prawej stronie zawiera następujących kart, aby ułatwić edytowanie koloru:

-   **Dziedzicz** &ndash; przedstawia diagram dziedziczenia stylu dla wybranego koloru i listy kolor rozwiązany i kod koloru przypisane do tego kolor motywu.

-   **Kolor próbnika** &ndash; pozwala zmienić wybrany kolor na dowolną wartość.

-   **Palety materiału** &ndash; umożliwia zmianie wybranego na kolor ma wartość, która odpowiada wymaganiom materiałów projektu.

-   **Zasoby** &ndash; pozwala zmienić wybrane do kolorów do jednego z innych istniejących kolor zasobów w motywie.

Przyjrzyjmy się każdej z nich z tych kart szczegółowo.



### <a name="inherit-tab"></a>Dziedzicz kartę

Jak pokazano w poniższym przykładzie **Dziedzicz** karta zawiera listę dziedziczenie styl **tła** kolor **domyślny motyw**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dziedzicz kartę](material-design-features-images/vs/06-inherit-tab-sml.png)](material-design-features-images/vs/06-inherit-tab.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dziedzicz kartę](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

-----

W tym przykładzie **domyślny motyw** dziedziczy styl, który używa `@color/background_material_dark` , ale zastępuje go tekstem `color/material_grey_850`, który ma wartość kodu kolor `#ff303030`.
Aby uzyskać więcej informacji o stylu dziedziczenia, zobacz [stylów i motywów](http://developer.android.com/guide/topics/ui/themes.html#Inheritance).



### <a name="color-picker"></a>Selektor kolorów

Poniższy zrzut ekranu przedstawia **próbnika kolorów**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Selektor kolorów](material-design-features-images/vs/07-color-picker-sml.png)](material-design-features-images/vs/07-color-picker.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Selektor kolorów](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)

-----

W tym przykładzie **tła** kolor można zmienić wartości za pośrednictwem różnych środków:

-   Klikając przycisk koloru bezpośrednio.
-   Wprowadzanie wartości odcień, nasycenie i jasność.
-   Wprowadź wartości RGB (czerwony, zielony, niebieski) po przecinku.
-   Ustawienie alfa (nieprzezroczystość) dla wybranego koloru.
-   Wprowadzenie kodu szesnastkowe kolor bezpośrednio.

Kolor wybrany w próbnika kolorów jest *nie* ograniczone do zaleceń dotyczących projektowania materiałów lub zestaw zasobów dostępny kolor.


### <a name="resources"></a>Resources

**Zasobów** kartę udostępnia listę zasobów kolorów, które już znajdują się w motywu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zasoby](material-design-features-images/vs/08-resources-sml.png)](material-design-features-images/vs/08-resources.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Zasoby](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

-----

Przy użyciu **zasobów** kartę ogranicza wybrane opcje do tej listy kolorów. Należy pamiętać, że jeśli wybierzesz zasób koloru, który jest już przypisana do innej części motywu, dwóch sąsiadujących ze sobą elementów interfejsu użytkownika może "razem" (ponieważ mają one ten sam kolor) i stać się trudne do odróżnienia przez użytkownika.



### <a name="material-palette"></a>Materiał palety

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Palety materiałów** karcie otwiera **paletę kolorów projektowania materiałów**. Wybranie wartości koloru z tej palety, ogranicza wybrany kolor, aby były one zgodne z wytycznymi projektowania materiału.

[![Materiał palety](material-design-features-images/vs/09-material-palette-sml.png)](material-design-features-images/vs/09-material-palette.png#lightbox)

Górnej części palety kolorów przedstawia podstawowy kolory materiałów projektu podczas dolnej części palety wyświetla zakres barw dla wybranych kolorów podstawowych. Na przykład po wybraniu **indygo**, Kolekcja **indygo** barwy są wyświetlane u dołu okna dialogowego.
Po wybraniu hue kolor właściwości jest zmieniany na wybranych hue. W poniższym przykładzie `Background Tint` przycisku jest zmieniana na *indygo 500*:

![Wybierz indygo 500](material-design-features-images/vs/10-indigo.png)

`Background Tint` ustawiono kod kolor *indygo 500* (`#ff3f51b5`), i projektanta aktualizuje kolor tła w celu odzwierciedlenia tej zmiany:

[![Zmienić odcień tła](material-design-features-images/vs/11-background-tint-sml.png)](material-design-features-images/vs/11-background-tint.png#lightbox)

Aby uzyskać więcej informacji dotyczących projektowania materiałów paletę kolorów, zobacz projekt materiałów [kolorów palety przewodnik](http://www.google.com/design/spec/style/color.html#color-color-palette).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**Palety materiałów** karcie otwiera **paletę kolorów projektowania materiałów** opisane [wcześniejszych](#material_palette). Wybranie wartości koloru z tej palety, ogranicza wybrany kolor, aby były one zgodne z wytycznymi projektowania materiału.

[![Materiał palety](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

-----



### <a name="creating-a-new-theme"></a>Tworzenie nowego motywu

W poniższym przykładzie użyjemy palety materiałów, aby utworzyć nowy motyw niestandardowy. Po pierwsze, zmienimy **tła** kolor *Blue 900*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Zmień tło 900 niebieski](material-design-features-images/vs/12-change-background-to-blue.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Zmień tło 900 niebieski](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

-----


Po zmianie zasobu kolor komunikat pojawia się komunikat o *bieżącego motywu istnieją niezapisane zmiany*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Niezapisane zmiany ostrzeżenie](material-design-features-images/vs/13-unsaved-changes-sml.png)](material-design-features-images/vs/13-unsaved-changes.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Niezapisane zmiany ostrzeżenie](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

-----


**Tła** kolorów w Projektancie zostały zmienione na nowe zaznaczenie kolorów, ale ta zmiana nie został jeszcze zapisany. Dwa wybory są oferowane sposobu obsługi zmian:

-   **Odrzuć zmiany** &ndash; odrzuca nowe kolory do wyboru (lub opcji) i powraca do stanu pierwotnego motywu.

-   **Tworzenie nowego motywu** &ndash; tworzy nowy motyw, jest określana na podstawie motywu aktualnie zaznaczone i używa zastąpienia kolorów w **Edytor motywów**.

Po kliknięciu **Utwórz nowy motyw**, jeden z następujących nastąpi, w zależności od wybranego motywu:

-   Jeśli aktualnie wybranego motywu w projektancie nie jest motyw projektu, jest wyświetlana opcja tworzenia nowego pliku zasobu do niestandardowych kompozycji (przy użyciu wybranego motywu jako element nadrzędny dostosowane motywu). Projektant zmienia się na motywu dostosowane po jego utworzeniu.

-   Jeśli aktualnie wybranego motywu motyw projektu jest zdefiniowany w jednej lokalizacji, jest wyświetlana **motyw aktualizacji** opcję; po kliknięciu tej opcji aktualizuje aktualnie wybranego motywu zamiast tworzenia nowej.

-   Jeśli aktualnie wybranego motywu jest motyw projektu, który jest zdefiniowany w wielu lokalizacjach (na przykład w różny sposób sufiks **wartości** folderów, takie jak **v21 wartości**), są wyświetlone okno dialogowe z pytaniem, dla nowej lokalizacji zapisywania dostosowane motywu.

W ramach poprzedniego przykładu, klikając **Utwórz nowy motyw** o nazwie powoduje utworzenie nowego motywu **niestandardowy**:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Motyw niestandardowy dodany](material-design-features-images/vs/14-custom-theme-sml.png)](material-design-features-images/vs/14-custom-theme.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Motyw niestandardowy dodany](material-design-features-images/xs/19-custom-theme-sml.png)](material-design-features-images/xs/19-custom-theme.png#lightbox)

-----


Aktualnie wybrany motyw nie jest motyw projektu, nie istnieje żadne okna dialogowego, aby zaktualizować wybranego motywu lub określ nową lokalizację.


## <a name="summary"></a>Podsumowanie

W tym temacie opisano projekt materiałów funkcje dostępne w Projektancie platformy Xamarin.Android. Go wyjaśniono, jak włączyć i skonfigurować siatki projektu materiałów, jak umożliwia edytowanie właściwości kolorów palety kolorów projektowania materiałów oraz sposobu selektor związane z typografią skali umożliwia konfigurowanie właściwości tekstu. On również wykazać, jak utworzyć nowe niestandardowe kompozycje, zgodnych ze standardami zaleceń dotyczących projektowania materiałów za pomocą edytora motywu. Aby uzyskać więcej informacji na temat platformy Xamarin.Android obsługę projektowania materiałów, zobacz [motyw materiału](~/android/user-interface/material-theme.md).



## <a name="related-links"></a>Linki pokrewne

- [Motyw materiału](~/android/user-interface/material-theme.md)
- [Wprowadzenie materiału projektu](https://www.google.com/design/spec/material-design/introduction.html)
