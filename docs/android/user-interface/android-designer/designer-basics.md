---
title: Podstawy projektanta
description: "W tym temacie przedstawiono funkcje projektanta, wyjaśniono, jak uruchomić projektanta opisuje powierzchnię projektu i szczegółowe informacje dotyczące używania w okienku właściwości można edytować właściwości elementu widget."
ms.topic: article
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b2ed48ae9df7e950525fdc0cb97181ebe5a44dfb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="designer-basics"></a>Podstawy projektanta

_W tym temacie przedstawiono funkcje projektanta, wyjaśniono, jak uruchomić projektanta opisuje powierzchnię projektu i szczegółowe informacje dotyczące używania w okienku właściwości można edytować właściwości elementu widget._

<a name="Launching_the_Designer" />

## <a name="launching-the-designer"></a>Uruchamianie narzędzia Projektant

Projektant jest uruchamiana automatycznie, podczas tworzenia układu, lub można uruchomić przez dwukrotne kliknięcie istniejący plik .axml. Na przykład dwukrotnie **Main.axml** w **zasobów > układu** folderu załaduje projektancie, jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Projektant ekranu w programie Visual Studio](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Projektant ekranu w programie Visual Studio dla komputerów Mac](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Podobnie, można dodać nowy układ, klikając prawym przyciskiem myszy **układu** folderu w **Eksploratora rozwiązań** i wybierając **Dodaj > Nowy element... > Android układu**:

[![Dodaj nowy element okna dialogowego](designer-basics-images/vs/02-add-new-layout-sml.png)](designer-basics-images/vs/02-add-new-layout.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Podobnie, można dodać nowy układ, klikając prawym przyciskiem myszy **układu** folderu w **konsoli rozwiązania** i wybierając **Dodaj > Nowy plik > Android > układ**:

[![Dodaj nowy plik w oknie dialogowym](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png)

-----

Tworzy nowy plik .axml i załaduje go na powierzchnię projektu.


<a name="Designer_Features" />

## <a name="designer-features"></a>Funkcje projektanta

Projektant składa się z kilku sekcje, które obsługują jego różnych funkcji, jak pokazano na poniższym zrzucie ekranu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagram okienek projektanta](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Diagram okienek projektanta](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png)

-----

Podczas edytowania układu w Projektancie służy do tworzenia i kształtu projektu następujące funkcje:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **Projektowanie powierzchni** &ndash; ułatwia wizualnego tworzenia interfejsu użytkownika, zapewniając można edytować reprezentację wygląd układ na urządzeniu.

-   **Pasek narzędzi** &ndash; Wyświetla listę selektorów: **urządzenia**, **wersji**, **motywu**, konfiguracja układu i ustawienia paska akcji. Pasek narzędzi zawiera także ikony uruchamianie Edytor motywów oraz włączenie materiałów siatki projektu.

-   **Przybornik** &ndash; zawiera listę elementów widget i układy, które można przeciągnąć i upuścić na powierzchnię projektu.

-   **Okno właściwości** &ndash; Wyświetla właściwości zaznaczonego elementu widget do wyświetlania i edytowania.

-   **Konspekt dokumentu** &ndash; Wyświetla drzewo elementy widget, które tworzą układ. Kliknięcie elementu w drzewie, aby spowodować, że plik można wybrać w projektancie. Ponadto kliknięcie elementu w drzewie ładuje właściwości elementu w oknie właściwości.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   **Projektowanie powierzchni** &ndash; ułatwia wizualnego tworzenia interfejsu użytkownika, zapewniając można edytować reprezentację wygląd układ na urządzeniu.

-   **Pasek narzędzi** &ndash; Wyświetla listę selektorów: **urządzenia**, **wersji**, **motywu**, konfiguracja układu i ustawienia paska akcji. Pasek narzędzi zawiera także ikony uruchamianie Edytor motywów oraz włączenie materiałów siatki projektu.

-   **Przybornik** &ndash; zawiera listę elementów widget i układy, które można przeciągnąć i upuścić na powierzchnię projektu.

-   **Właściwość konsoli** &ndash; Wyświetla właściwości zaznaczonego elementu widget do wyświetlania i edytowania.

-   **Konspekt dokumentu** &ndash; Wyświetla drzewo elementy widget, które tworzą układ. Kliknięcie elementu w drzewie, aby spowodować, że plik można wybrać w projektancie. Ponadto kliknięcie elementu w drzewie ładuje właściwości elementu do właściwości konsoli.

-----


<a name="Toolbar" />

## <a name="toolbar"></a>Pasek narzędzi

Pasek narzędzi (znajduje się nad powierzchnię projektu) przedstawia selektorów konfiguracji i menu Narzędzia:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagram projektanta paska narzędzi](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Diagram projektanta paska narzędzi](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png)

-----


Pasek narzędzi zapewnia dostęp do następujących funkcji:

-   **Alternatywne selektora układu** &ndash; służy do wybierania z układu innej wersji.

-   **Selektor urządzenia** &ndash; definiuje zestaw kwalifikatory skojarzone z określonego urządzenia, takich jak rozmiar ekranu, rozdzielczość i dostępności klawiatury. Można również dodawać i usuwać nowych urządzeń.

-   **Selektor wersji android** &ndash; układ Android wersję. Projektant spowoduje, że układu w zależności od wybranej wersji dla systemu Android.

-   **Selektor motywu** &ndash; wybiera motywu interfejsu użytkownika dla układu.

-   **Selektor konfiguracji** &ndash; wybiera konfigurację urządzenia, takie jak *pionowa* lub *pozioma*.

-   **Opcje kwalifikatora zasobów** &ndash; otwiera okno dialogowe prezentuje menu rozwijane wyboru *języka*, *tryb interfejsu użytkownika*, *tryb nocy*, i *Round ekranu* opcje.

-   **Ustawienia paska akcji** &ndash; konfiguruje ustawienia paska akcji dla układu.

-   **Edytor motywów** &ndash; otwiera *Edytor motywów*, co umożliwia umożliwiające dostosowywanie elementów wybranego motywu.

-   **Materiał siatki projektu** &ndash; Włącza lub wyłącza *siatki projektu materiału*. Sąsiadujące siatki projektu materiał elementu menu rozwijanego otwiera okno dialogowe, które umożliwia dostosowanie siatki.

Każda z tych funkcji jest co omówiono bardziej szczegółowo w tych tematach:

[Kwalifikatory zasobów i Opcje wizualizacji](~/android/user-interface/android-designer/resource-qualifiers.md) zawiera szczegółowe informacje na temat **selektora urządzenia**, **Android selektora wersji**, **selektor motywu**, **Selektora konfiguracji**, **opcje kwalifikacji zasobów**, i **ustawienia paska akcji**.

[Alternatywne widoki układu](~/android/user-interface/android-designer/alternative-layout-views.md) wyjaśniono, jak używać **selektora układu zamiast**.

[Materiał projektowe](~/android/user-interface/android-designer/material-design-features.md) zawiera kompleksowe omówienie **Edytor motywów** i **siatki projektu materiału**.


<a name="Design_Surface" />

## <a name="design-surface"></a>Dzięki powierzchni projektowej

Projektant pozwala na przeciąganie i upuszczanie elementów widget z przybornika na powierzchnię projektu. Podczas interakcji z elementów widget w Projektancie (przez dodanie nowych elementów widget lub zmienianie położenia istniejących), aby oznaczyć punkty wstawienia dostępne są wyświetlane linie poziome i pionowe. W poniższym przykładzie nową `Button` przeciągania elementu widget na powierzchnię projektu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Przykładowe wiersze wstawiania na powierzchni projektowej](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Przykładowe wiersze wstawiania na powierzchni projektowej](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png)

-----

Ponadto można kopiować elementy widget: można używać Kopiuj i Wklej, aby skopiować element widget lub możesz przeciągać i upuszczać istniejącego elementu widget podczas naciskając klawisz <kbd>Ctrl</kbd> klucza.

<a name="Context_Menu_Commands" />

### <a name="context-menu-commands"></a>Polecenia Menu kontekstowe

Menu kontekstowe jest dostępne zarówno w powierzchnię projektu i tworzenie konspektu dokumentu. Rozwijalna poleceń, które są dostępne dla wybranego elementu widget i jego kontenera, ułatwiając użytkownikom do wykonywania operacji na kontenery (które nie zawsze są łatwo wybrać na powierzchni projektu). Oto przykład menu kontekstowego:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Menu kontekstowe przykład po kliknięciu prawym przyciskiem myszy powierzchnię projektu](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png)

W tym przykładzie klikając prawym przyciskiem myszy `TextView` otwiera menu kontekstowego, który zapewnia kilka opcji:

-   **LinearLayout** &ndash; otwiera podmenu do edycji `LinearLayout` nadrzędny `TextView`.

-   **Usuń**, **kopiowania**, i **Wytnij** &ndash; operacje, które dotyczą klikniętego `TextView`.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Menu kontekstowe przykład po kliknięciu prawym przyciskiem myszy powierzchnię projektu](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png)

W tym przykładzie klikając prawym przyciskiem myszy `TextView` otwiera menu kontekstowego, który zapewnia kilka opcji:

-   **RelativeLayout** &ndash; otwiera podmenu do edycji `RelativeLayout` nadrzędny `TextView`.

-   **LinearLayout** &ndash; otwiera podmenu do edycji `LinearLayout` nadrzędny `TextView`.

-   **Usuń**, **kopiowania**, i **Wytnij** &ndash; operacje, które dotyczą klikniętego `TextView`.

-----

W tym przykładzie klikając prawym przyciskiem myszy `TextView` otwiera menu kontekstowego, który zapewnia kilka opcji:

-   **LinearLayout** &ndash; otwiera podmenu do edycji `LinearLayout` nadrzędny `TextView`.

-   **Usuń**, **kopiowania**, i **Wytnij** &ndash; operacje, które dotyczą klikniętego `TextView`.


<a name="Zoom_Controls" />

### <a name="zoom-controls"></a>Kontrolki powiększania

Powierzchni projektowej obsługuje powiększanie za pośrednictwem kilku formantów, jak pokazano:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagram kontrolki powiększania powierzchni projektowej](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Diagram kontrolki powiększania powierzchni projektowej](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png)

-----

Te kontrolki ułatwiają Zobacz niektórych obszarach interfejsu użytkownika w Projektancie:

-   **Wyróżnij kontenery** &ndash; wyróżnia kontenerów na powierzchni projektu, dzięki czemu są one łatwiej zlokalizować podczas powiększanie i zmniejszanie.

-   **Rozmiar normalny** &ndash; renderuje układu pikseli do piksela, tak aby były widoczne wygląd układ z rozdzielczością wybranego urządzenia.

-   **Dopasuj do okna** &ndash; ustawia poziom powiększenia, dzięki czemu cały układ jest widoczny na powierzchni projektu.

-   **Powiększ** &ndash; powiększa przyrostowo kliknięcie powiększanie układu.

-   **Pomniejsz** &ndash; stopniowo zmniejsza się kliknięcie tworzenie układu są mniejsze na powierzchni projektu.

Należy pamiętać, że wybrana powiększenie ustawienia nie wpływa na interfejs użytkownika aplikacji w czasie wykonywania.

<a name="property_pad" />

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="property-pad"></a>Właściwości konsoli

Projektant obsługuje edycji właściwości elementu widget za pośrednictwem **konsoli właściwości**. Właściwości wyświetlane w przypadku zmiany właściwości konsoli, w zależności od tego, który wybrano elementu widget na powierzchni projektanta. Gdy `Button` w poprzednim przykładzie jest zaznaczone, właściwości, dla którego `Button` widget są wyświetlane:

[![Zrzut ekranu konsoli właściwości](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png)

-----

<a name="Property_Pad_Sections" />

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="properties-window"></a>Okno Właściwości

Projektant obsługuje edycji właściwości elementu widget za pośrednictwem **okna właściwości**. Właściwości wyświetlane w przypadku zmiany okna właściwości, w zależności od tego, który wybrano elementu widget na powierzchni projektu.
Gdy `Button` w poprzednim przykładzie jest zaznaczone, właściwości, dla którego `Button` widget są wyświetlane:

![Zrzut ekranu przedstawiający okno właściwości](designer-basics-images/vs/08-property-pad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="property-pad-sections"></a>Sekcji właściwości konsoli

Konsoli do właściwości jest podzielony na wiele sekcji, które grupują podobne właściwości &ndash; ułatwia można zlokalizować właściwości odsetek:

-   **Widżet** &ndash; właściwości głównego elementu widget, takich jak `id`, `visibility`, `text`itp. Właściwości do zarządzania zawartością w elemencie widget zwykle są umieszczane w tym miejscu.

-   **Styl** &ndash; właściwości, które zmienić wygląd elementu widget, takich jak `font`, `text color`, `background`itp.

-   **Układ** &ndash; właściwości, które ustawić lokalizacja i rozmiar elementu widget.

-   **Przewijania** &ndash; przewijanie właściwości.

-   **Zachowanie** &ndash; flagi, które ustawione zachowania elementu widget.

-----


<a name="Default_Values" />

### <a name="default-values"></a>Wartości domyślne

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Właściwości większości widżetów jest puste w **właściwości** okna ponieważ dziedziczy wartości z wybranego motywu systemu Android.
**Właściwości** oknie będą wyświetlane tylko wartości, które są jawnie ustawione dla wybranego elementu widget; wartości, które są dziedziczone z motywu nie będą widoczne.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Właściwości większości widżetów jest puste w **konsoli właściwości** ponieważ dziedziczy wartości z wybranego motywu systemu Android. **Konsoli właściwości** zostaną wyświetlone tylko wartości, które są jawnie ustawione dla wybranego elementu widget; wartości, które są dziedziczone z motywu nie będą widoczne.

-----

<a name="Referencing_resources" />

### <a name="referencing-resources"></a>Odwołania do zasobów

Niektóre właściwości mogą odwoływać się zasoby, które są zdefiniowane w plikach inne niż układu **.axml** pliku. Typowe przypadki tego typu są `string` i `drawable` zasobów. Jednak odwołania można również dla innych zasobów, takich jak `Boolean` wartości i wymiary.
Gdy właściwość obsługuje odwołania do zasobu, ikona Przeglądaj (moduł wielokropka, &hellip;) jest wyświetlany obok tekstu wpis dla właściwości.
Ten przycisk Otwiera selektor zasobów, po kliknięciu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Na przykład poniższy zrzut ekranu przedstawia zasoby dostępne po kliknięciu przycisku wielokropka z prawej strony pola tekstowego do `Button` elementu widget w **właściwości** okno:

[![Zrzut ekranu zasobów z zasobami dwa wymienione](designer-basics-images/vs/09-resources-sml.png)](designer-basics-images/vs/09-resources.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Na przykład poniższy zrzut ekranu przedstawia zasoby dostępne po kliknięciu przycisku wielokropka z prawej strony pola tekstowego do `Button` elementu widget w **konsoli właściwości**:

[![Zrzut ekranu zasobów z zasobami dwa wymienione](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png)

-----

Kolejnym przykładzie pokazano selektor zasobów `Src` właściwości `ImageView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wyświetlanie listy zasobu ikony dla ImageView selektor zasobów](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Wyświetlanie listy zasobu ikony dla ImageView selektor zasobów](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png)

-----


<a name="Boolean_Property_References" />

### <a name="boolean-property-references"></a>Odwołania do właściwości typu Boolean

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

*Wartość logiczna* właściwości są zwykle wybrany z listy rozwijanej w oknie właściwości. Możesz wybrać `true` lub `false` lub odwołania do właściwości za pomocą można wybrać **wybierz zasobów...** . Można również bezpośrednio wprowadzisz wartość, takich jak `true` lub `false`.

![Przykładowa konfiguracja operatory logiczne](designer-basics-images/vs/11-boolean.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

*Wartość logiczna* właściwości są zwykle wyświetlane pole wyboru w konsoli właściwości. Gdy `Boolean` właściwość obsługuje odwołania do zasobu, małe pole wyboru obok właściwości. Oznacza, że jest zaznaczone pole wyboru `true` i puste pole oznacza `false`. Można również bezpośrednio wprowadzisz wartość, takich jak `true` lub `false`. Ustawiając kursor myszy nad danych wejściowych wywołuje ikona pola małego tekstu. Możesz kliknąć na nim Jeśli chcesz ręcznie wprowadź wartość.

[![Przykładowa konfiguracja operatory logiczne](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png)

<a name="Grouped_Properties" />

## <a name="grouped-properties"></a>Właściwości grupowanych

Niektóre elementy widget ma wiele wartości właściwości, które są zgrupowane (takich jak `Padding`, na przykład). Wartości tych właściwości są wymienione w **konsoli właściwości** w wierszu jednej, można rozwijać. Niektóre z tych właściwości można edytować bezpośrednio w wierszu grupowanych, takich jak `Padding` właściwości pokazano poniżej:

[![Przykład ustawienia właściwości dopełnienia](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png)

-----

<a name="Editing_Properties_Inline" />

## <a name="editing-properties-inline"></a>Edytowanie właściwości wbudowany

Projektant Android obsługuje bezpośredniej edycji niektórych właściwości na powierzchni projektu (dzięki czemu nie trzeba wyszukiwać tych właściwości na liście właściwości). Właściwości, które można bezpośrednio edytować obejmują tekstu marginesu i rozmiar.

<a name="Text" />

### <a name="text"></a>Tekst

Właściwości tekst niektóre elementy widget (takich jak `Button` i `TextView`), można edytować bezpośrednio na powierzchni projektu. Dwukrotne kliknięcie elementu widget umieści je w trybie edycji, jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Tekst zasobu ciągu hello](designer-basics-images/vs/12-text-resource.png "zasobów tekstu")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Tekst zasobu ciągu hello](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png)

-----

Możesz wprowadzić nowe wartości tekstowej lub wprowadzić nowy ciąg zasobu. W poniższym przykładzie `@string/hello` zasobów jest zastępowany tekstem, `CLICK THIS BUTTON`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Shift + Enter, aby automatycznie połączyć tekst na nowy zasób](designer-basics-images/vs/13-shift-enter-resource.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Shift + Enter, aby automatycznie połączyć tekst na nowy zasób](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png)

-----

Ta zmiana jest przechowywana w elemencie widget `text` właściwości; nie modyfikuje wartość przypisana do `@string/hello` zasobów.
Gdy wprowadzenie nowego ciągu tekstowego, możesz nacisnąć przycisk <kbd>Shift</kbd> +
<kbd>Enter</kbd> połączyć automatycznie wprowadzony tekst na nowy zasób.

<a name="Margin" />

### <a name="margin"></a>Margines

Po wybraniu elementu widget projektanta Wyświetla dojść, które umożliwiają zmianę rozmiaru lub margines widżetu interaktywnie. Kliknięcie elementu widget, gdy jest wybrana przełącza między trybem edycji margines a trybem edycji rozmiar.

Po kliknięciu elementu widget po raz pierwszy margines uchwyty są wyświetlane. Po przeniesieniu przycisku myszy do jednego z uchwytów projektanta Wyświetla właściwości, która zmieni dojście (jak pokazano poniżej, aby uzyskać `layout_marginLeft` właściwość):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Zrzut ekranu przedstawiający margines dojść w Projektancie](designer-basics-images/vs/15-margin-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Zrzut ekranu przedstawiający margines dojść w Projektancie](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png)

-----

Czy margines została już ustawiona, są wyświetlane linie przerywana, wskazujące miejsca, która zajmuje margines:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Przykładowe wiersze przerywana oznaczenie przycisk ilości wolnego miejsca](designer-basics-images/vs/16-margins-set.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Przykładowe wiersze przerywana oznaczenie przycisk ilości wolnego miejsca](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png)

-----


<a name="Size" />

### <a name="size"></a>Rozmiar

Jak wspomniano wcześniej, można przełączyć do trybu edycji rozmiar, klikając element widget, gdy jest ono już zaznaczone. Kliknij trójkątny uchwyt do ustawiania rozmiaru dla wskazanego wymiaru do `wrap_content`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Uchwyty zawijania zawartości i zmiana rozmiaru](designer-basics-images/vs/17-wrap-content.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Uchwyty zawijania zawartości i zmiana rozmiaru](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png)

-----

Kliknięcie przycisku **zawijać zawartości** dojście zmniejsza widget w tym wymiarze, wtedy nie większą niż jest to konieczne do zakodowania zawartości zamkniętych. W tym przykładzie tekst przycisku zmniejsza poziomie, jak pokazano w następnym zrzut ekranu.

Jeśli ustawiono wartość rozmiaru **zawijać zawartości**, Projektant wyświetla uchwyt trójkątny wskazanie w przeciwnym kierunku do zmiany rozmiaru na `match_parent`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dopasowanie nadrzędnego dojścia](designer-basics-images/vs/18-match-parent.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dopasowanie nadrzędnego dojścia](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png)

-----

Kliknięcie przycisku **nadrzędnego dopasowania** dojście Przywraca rozmiar w tym wymiarze, aby była taka sama jak nadrzędnego elementu widget.

Ponadto można przeciągnąć uchwyt zmiany rozmiaru cykliczne (jak pokazano na powyższym zrzutach ekranu) do zmiany rozmiaru elementu widget do dowolnego `dp` wartość. Po wykonaniu, zarówno **zawijać zawartości** i **nadrzędnego dopasowania** uchwyty są wyświetlone dla tego wymiaru:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Uchwyty cykliczne zmiany rozmiaru](designer-basics-images/vs/19-resize-dp.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Uchwyty cykliczne zmiany rozmiaru](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png)

-----

Nie wszystkie kontenery Zezwól na edytowanie `Size` widżetu. Na przykład, zwróć uwagę, że na zrzucie ekranu poniżej z `LinearLayout` zaznaczone, uchwyty skalowania nie są wyświetlane:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Nie dojścia do zmiany rozmiaru](designer-basics-images/vs/20-no-resize-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nie dojścia do zmiany rozmiaru](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png)

-----


<a name="Outline_View" />

## <a name="document-outline"></a>Konspekt dokumentu

**Konspekt dokumentu** Wyświetla hierarchię widget układu.
W poniższym przykładzie, zawierający `LinearLayout` wybrano element widget:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Konspekt dokumentu](designer-basics-images/vs/21-document-outline.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Konspekt dokumentu](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png)

-----

Konspekt wybranego elementu widget (w tym przypadku `LinearLayout`) wyróżnione na powierzchni projektu. Wybrany element widget konspektu dokumentu pozostaje zsynchronizowane z jego odpowiednikiem na powierzchnię projektu. Jest to przydatne w przypadku wybraniu widoku grupy, które nie są zawsze łatwo wybrać na powierzchni projektu.

Konspekt dokumentu obsługuje kopiowania i wklejania lub można użyć przeciągania i upuszczania. Przeciąganie i upuszczanie jest obsługiwana z konspektu dokumentu na powierzchnię projektu, a także z powierzchni projektu na tworzenie konspektu dokumentu. Ponadto prawym przyciskiem myszy element konspekt dokumentu Wyświetla menu kontekstowego dla tego elementu (tym samym kontekście menu wyświetlanym po kliknięciu prawym przyciskiem myszy tego samego elementu widget na powierzchni projektu).
