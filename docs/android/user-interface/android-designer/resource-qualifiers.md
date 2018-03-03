---
title: "Kwalifikatory zasobów i Opcje wizualizacji"
description: "W tym temacie wyjaśniono, jak zdefiniować zasoby, które będą używane tylko wtedy, gdy są dopasowywane w niektórych wartości kwalifikatora. Prosty przykład jest zasób ciągu kwalifikowana języka. Zasób ciągu można zdefiniować jako domyślny, innymi zasobami alternatywne określone do użycia dla dodatkowych języków. Wszystkich typów zasobów mogą być kwalifikowane, w tym sam układ."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: 56fee71f2ed36b682d323bae1225430ff991f140
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="resource-qualifiers-and-visualization-options"></a>Kwalifikatory zasobów i Opcje wizualizacji

_W tym temacie wyjaśniono, jak zdefiniować zasoby, które będą używane tylko wtedy, gdy są dopasowywane w niektórych wartości kwalifikatora. Prosty przykład jest zasób ciągu kwalifikowana języka. Zasób ciągu można zdefiniować jako domyślny, innymi zasobami alternatywne określone do użycia dla dodatkowych języków. Wszystkich typów zasobów mogą być kwalifikowane, w tym sam układ._

<a name="Custom_Device_Configurations" />

## <a name="custom-device-configurations"></a>Konfiguracje niestandardowe urządzenie

Android jest dostępna na nadmiar urządzeń i rozdzielczości ekranu.
Aby ułatwić projektowanie interfejsów użytkownika, przeznaczonych dla wielu urządzeń, Projektant zawiera wiele wbudowanej w konfiguracji urządzenia. Obsługuje również Dodawanie konfiguracji dodatkowych urządzeń; Te konfiguracje są oparte na *kwalifikatory* opcji odróżnić jedną konfigurację urządzenia z innej. Istnieje wiele różnych typów kwalifikatorów. Aby uzyskać więcej informacji na temat tych typów zasobów, zobacz [Android zasobów](~/android/app-fundamentals/resources-in-android/index.md).

W dolnej części selektora urządzenia menu jest **Dostosuj** opcji, jak pokazano poniżej:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Menu selektora urządzenia](resource-qualifiers-images/vs/01-device-selector-sml.png)](resource-qualifiers-images/vs/01-device-selector.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Menu selektora urządzenia](resource-qualifiers-images/xs/01-device-selector-sml.png)](resource-qualifiers-images/xs/01-device-selector.png)

-----


Wybieranie **Dostosuj** Wyświetla okno dialogowe, czy można użyć do przeglądania konfiguracji dostępnego urządzenia. Po kliknięciu **definicje urządzenia** kartę, znajduje się lista wszystkich definicji znanych urządzeń:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Menedżer AVD](resource-qualifiers-images/vs/02-device-definitions-sml.png)](resource-qualifiers-images/vs/02-device-definitions.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Menedżer AVD](resource-qualifiers-images/xs/02-device-definitions-sml.png)](resource-qualifiers-images/xs/02-device-definitions.png)

-----


Nie można zmodyfikować urządzenia wstępnie skonfigurowane w projektancie. Jednak można kliknąć **tworzenia urządzenia...**  do definiowania definicji urządzeń niestandardowych. Alternatywnie można wybrać istniejącą definicję i kliknij przycisk **klonowania...**  można używać go jako punktu wyjścia nową definicję.
Na przykład wybranie **5 węzła** definicji i klikając **klonowania...**  następujące okno dialogowe:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Urządzenie w klonowania](resource-qualifiers-images/vs/03-clone-sml.png)](resource-qualifiers-images/vs/03-clone.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Urządzenie w klonowania](resource-qualifiers-images/xs/03-clone-sml.png)](resource-qualifiers-images/xs/03-clone.png)

-----


W następnym zrzut ekranu, nazwa została zmieniona na **węzła 5 niestandardowych** i parametrów urządzenia, które są modyfikowane w celu utwórz nową definicję urządzeń niestandardowych. W tym przykładzie **pionowa** jest wyłączone, tak aby definicji urządzeń jest tylko do orientacji poziomej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Urządzeń niestandardowych](resource-qualifiers-images/vs/04-custom-sml.png)](resource-qualifiers-images/vs/04-custom.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Urządzeń niestandardowych](resource-qualifiers-images/xs/04-custom-sml.png)](resource-qualifiers-images/xs/04-custom.png)

-----


Kliknięcie przycisku **urządzenia w klonowania** tworzy nową definicję urządzenie jest teraz wyświetlany w **definicje urządzenia** listy:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Definicje zaktualizowanych urządzeniach](resource-qualifiers-images/vs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/vs/05-updated-device-definitions.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Definicje zaktualizowanych urządzeniach](resource-qualifiers-images/xs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/xs/05-updated-device-definitions.png)

-----


Należy pamiętać, że każda definicja utworzonych przez użytkownika urządzenia zostanie wyświetlony z zieloną ikonę zgodnie z powyższym. Po powrocie do **urządzenia** menu selektora nową definicję urządzeń niestandardowych są prezentowane w sekcji znajdujących się na górze listy (Jeśli nie ma konfiguracji niestandardowej urządzenia w tej listy, spróbuj uruchomić ponownie IDE):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Niestandardowe urządzenie pojawi się lista urządzeń](resource-qualifiers-images/vs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/vs/06-nexus-5-custom.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Niestandardowe urządzenie pojawi się lista urządzeń](resource-qualifiers-images/xs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/xs/06-nexus-5-custom.png)

-----


Wybranie tej konfiguracji urządzenia modyfikuje układ odpowiada dostosowania utworzone wcześniej (w tym trybie case, tylko pozioma):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Niestandardowe urządzenie w użyciu](resource-qualifiers-images/vs/07-custom-in-use-sml.png)](resource-qualifiers-images/vs/07-custom-in-use.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Niestandardowe urządzenie w użyciu](resource-qualifiers-images/xs/07-custom-in-use-sml.png)](resource-qualifiers-images/xs/07-custom-in-use.png)

-----


<a name="resource_qualifier_options" />

## <a name="resource-qualifier-options"></a>Opcje kwalifikatora zasobów

**Opcje kwalifikatora zasobów** jest możliwy, klikając ikonę trójkąt dół na prawo od **konfiguracji urządzenia** opcje:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Opcje kwalifikatora zasobów](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Opcje kwalifikatora zasobów](resource-qualifiers-images/xs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt.png)

-----


To okno dialogowe wyświetla menu rozwijanego dla następujących kwalifikatory zasobów:

-   **Język** &ndash; Wyświetla zasoby dostępne języka i oferuje możliwość dodawania nowych zasobów języka i regionu.

-   **Tryb interfejsu użytkownika** &ndash; tryby wyświetlania list (takich jak **dokowania samochodu** i **dokowania technicznej**) oraz wskazówki układu.

Każdy z tych menu rozwijanego otwiera nowy okien dialogowych, gdzie można wybrać i skonfigurować kwalifikatory zasobów (jak wyjaśniono dalej).


<a name="Language_and_Region" />

### <a name="language"></a>Język

**Języka** menu rozwijanego wyświetla tylko te języki, które mają zasoby zdefiniowane (lub **wszystkie języki**, co jest ustawieniem domyślnym). Istnieje jednak również **dodać języka i regionu...**  opcja, która pozwala dodać nowy język do listy:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Dodawanie języka i regionu](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Dodawanie języka i regionu](resource-qualifiers-images/xs/09-add-language-region-sml.png)](resource-qualifiers-images/xs/09-add-language-region.png)

-----


Po kliknięciu **dodać języka i regionu...** , **wybierz język** zostanie otwarte okno dialogowe, aby wyświetlić listy rozwijanej dostępnych języków i regionów:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Lista języków](resource-qualifiers-images/vs/10-languages.png "listę języków")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Lista języków](resource-qualifiers-images/xs/10-languages-sml.png)](resource-qualifiers-images/xs/10-languages.png)

-----


W tym przykładzie Wybraliśmy **fr (francuski)** dla języka i **BE** (Belgia) dla regionalnych dialekt francuski. Należy pamiętać, że **Region** pole jest opcjonalne, ponieważ wiele języków można określić bez względu na określonych regionach. Gdy **języka** menu rozwijanego zostanie otwarta ponownie, wyświetlane są zasobów nowo dodanych języka i regionu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Wybrany Region i język](resource-qualifiers-images/vs/11-language-region-added.png "wybrany Region i język")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Język i wybrać Region](resource-qualifiers-images/xs/11-language-region-added-sml.png)](resource-qualifiers-images/xs/11-language-region-added.png)

-----


Należy pamiętać, że jeśli dodać nowy język, ale nie należy tworzyć nowe zasoby, aby wykazać, że dodano języka nie będzie przy następnym otwarciu projektu.


<a name="ui_mode" />

### <a name="ui-mode"></a>Tryb interfejsu użytkownika

Po kliknięciu **tryb interfejsu użytkownika** menu rozwijanego, Lista trybów jest wyświetlana, takich jak **normalny**, **dokowania samochodu**, **dokowania technicznej**, **Telewizji**, **urządzenia**, i **czujki**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Menu Tryb interfejsu użytkownika](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png)

Poniżej tej listy są tryby nocy **nie nocy** i **nocy**, a następnie z instrukcjami układu **od lewej do prawej** i **od prawej do lewej** (dla informacje o **od lewej do prawej** i **od prawej do lewej** opcji, zobacz [funkcji LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).
Ostatnich elementów w **zasobów kwalifikatora** okna dialogowego są **zaokrąglona ekrany** (do użytku z systemem Android nosić) lub **zaokrągla ekrany** (informacji o rotacji i ekrany z systemem innym niż round, zobacz [układów](https://developer.android.com/training/wearables/ui/layouts.html)).
Aby uzyskać więcej informacji na temat trybów interfejsu użytkownika dla systemu Android, zobacz [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Menu Tryb interfejsu użytkownika](resource-qualifiers-images/xs/12-ui-mode-sml.png)](resource-qualifiers-images/xs/12-ui-mode.png)

Poniżej tej listy są tryby nocy **nie nocy** i **nocy**, a następnie z instrukcjami układu **od lewej do prawej** i **od prawej do lewej**. Aby uzyskać więcej informacji na temat trybów interfejsu użytkownika dla systemu Android, zobacz [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
Aby uzyskać informacje o **od lewej do prawej** i **od prawej do lewej** opcji, zobacz [funkcji LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).

### <a name="round-screen"></a>ROUND ekranu

Ostatni element **zasobów kwalifikatora** okno dialogowe jest **Round ekranu** menu. To menu służy do wybierania albo **zaokrąglona ekrany** (do użytku z systemem Android nosić) lub **prostokątne ekrany**:

[ ![ROUND menu ekranu](resource-qualifiers-images/xs/13-round-screen-sml.png)](resource-qualifiers-images/xs/13-round-screen.png)

-----


<a name="Action_Bar" />

## <a name="action-bar-settings"></a>Ustawienia paska akcji

**Ustawienia paska akcji** ikona jest dostępna w lewo ikonę pędzla (Edytor motywów):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ustawienia paska akcji](resource-qualifiers-images/vs/14-action-bar.png "ustawienia paska akcji")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Ustawienia paska akcji](resource-qualifiers-images/xs/13b-action-bar-sml.png)](resource-qualifiers-images/xs/13b-action-bar.png)

-----


Ta ikona zostanie otwarty popover okno dialogowe, które pozwala wybrać jeden z trzech trybów pasku akcji:

-   **Standardowe** &ndash; składa się z logo lub ikony, jak i tytuł text z pomocniczą opcjonalne.

-   **Lista** &ndash; tryb nawigacji listy. Zamiast tekstu tytułu statycznych, w tym trybie przedstawia listę menu nawigacji w ramach działania (to znaczy przedstawienia użytkownikowi jako listy rozwijanej).

-   **Karty** &ndash; trybie nawigacji kart. Zamiast tekstu tytułu statycznych w tym trybie przedstawia szereg kart nawigacji w ramach działania.


<a name="Themes" />

## <a name="themes"></a>Motywy

**Motyw** menu rozwijanego Wyświetla wszystkie motywów zdefiniowanych w projekcie. Wybieranie **więcej motywów** otwiera okno dialogowe z listą wszystkich motywów dostępnej w sklepie zainstalowany zestaw SDK systemu Android, jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Więcej listy motywów](resource-qualifiers-images/vs/15-theme-menu-sml.png "listy więcej motywów")](resource-qualifiers-images/vs/15-theme-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Więcej listy motywów](resource-qualifiers-images/xs/14-theme-menu-sml.png)](resource-qualifiers-images/xs/14-theme-menu.png)

-----


Po wybraniu motywu powierzchnię projektu jest aktualizowana w celu wyświetlenia efekt nowego motywu. Należy pamiętać, że ta zmiana jest trwałe tylko wtedy, gdy **OK** przycisku w **motyw** okna dialogowego. Po wybraniu motywu mają być uwzględnieni w **motyw** menu rozwijanego jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Motywu jasny jest teraz dostępna](resource-qualifiers-images/vs/16-light-theme.png "motywu jasny jest teraz dostępna")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Motywu jasny jest teraz dostępna](resource-qualifiers-images/xs/15-light-theme-sml.png)](resource-qualifiers-images/xs/15-light-theme.png)

-----


<a name="Android_Version" />

## <a name="android-version"></a>Wersja systemu android

Android **wersji** selektora ustawia wersji dla systemu Android, która jest używana do renderowania układu w projektancie. Selektor Wyświetla wszystkie wersje, które są zgodne z docelową wersję platformy projektu:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Lista wszystkich wersji systemu Android](resource-qualifiers-images/vs/17-android-version.png "wersji listy Android")

Wersja docelowego frameworka można ustawić w ustawieniach projektu w obszarze **właściwości > aplikacji > skompilować przy użyciu wersji Android**. Aby uzyskać więcej informacji na temat wersja docelowego frameworka, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).

Zbiór elementów widget dostępnych w przyborniku jest określana przez wersję platformy docelowej projektu. Dotyczy to również dostępne w właściwości **okna właściwości**. Listę dostępnych elementów widget jest *nie* określana przez wartość wybrana w **wersji** selektora paska narzędzi. Na przykład, jeśli wybrana wersja docelowa projektu systemu Android 4.4, nadal można wybrać system Android 6.0 w selektorze wersji narzędzi, aby zobaczyć, jak wygląda projekt Android 6.0, ale nie będzie można dodać elementy widget, które są specyficzne dla systemu Android 6.0 &ndash;  nadal będzie ograniczona do elementy widget, które są dostępne w systemie Android 4.4.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Lista wszystkich wersji systemu Android](resource-qualifiers-images/xs/16-android-version-sml.png)](resource-qualifiers-images/xs/16-android-version.png)

Wersja docelowego frameworka można ustawić w ustawieniach projektu w obszarze **opcje projektu > kompilacji > Ogólne** sekcji. Aby uzyskać więcej informacji na temat wersja docelowego frameworka, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).

Zbiór elementów widget dostępnych w przyborniku jest określana przez wersję platformy docelowej projektu. Dotyczy to również dostępne w właściwości **konsoli właściwości**. Listę dostępnych elementów widget jest *nie* określana przez wartość wybrana w **wersji** selektora paska narzędzi. Na przykład, jeśli wybrana wersja docelowa projektu systemu Android 4.4, nadal można wybrać system Android 6.0 w selektorze wersji narzędzi, aby zobaczyć, jak wygląda projekt Android 6.0, ale nie będzie można dodać elementy widget, które są specyficzne dla systemu Android 6.0 &ndash;  nadal będzie ograniczona do elementy widget, które są dostępne w systemie Android 4.4.

-----
