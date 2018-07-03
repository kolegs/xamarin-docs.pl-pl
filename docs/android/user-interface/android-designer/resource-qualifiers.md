---
title: Kwalifikatory zasobów i Opcje wizualizacji
description: W tym temacie wyjaśniono, jak zdefiniować zasoby, które będą używane tylko wtedy, gdy niektóre wartości kwalifikatora są dopasowywane. Prostym przykładem jest zasób w postaci ciągu kwalifikowana języka. Zasób w postaci ciągu można zdefiniować jako wartość domyślna z innymi zasobami alternatywnych definicja ma być używany dla dodatkowych języków. Wszystkie typy zasobów może być kwalifikowana, wraz z układem, sam.
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: 7bc8c1066e557085c1bf34f77765edbb2259ba7a
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403302"
---
# <a name="resource-qualifiers-and-visualization-options"></a>Kwalifikatory zasobów i Opcje wizualizacji

_W tym temacie wyjaśniono, jak zdefiniować zasoby, które będą używane tylko wtedy, gdy niektóre wartości kwalifikatora są dopasowywane. Prostym przykładem jest zasób w postaci ciągu kwalifikowana języka. Zasób w postaci ciągu można zdefiniować jako wartość domyślna z innymi zasobami alternatywnych definicja ma być używany dla dodatkowych języków. Wszystkie typy zasobów może być kwalifikowana, wraz z układem, sam._


## <a name="custom-device-configurations"></a>Konfiguracje niestandardowe

Android jest dostępny na mnóstwo urządzenia i rozdzielczości ekranu.
Aby ułatwić projektowanie interfejsów użytkownika, przeznaczonych dla wielu urządzeń, Projektant jest dostarczany z kilku różnych konfiguracjach urządzenia wbudowane. Obsługuje również Dodawanie konfiguracji dodatkowych urządzeń; Te konfiguracje są oparte na *kwalifikatory* umożliwia odróżnić jedną konfigurację urządzenia z innego. Istnieje wiele różnych typów kwalifikatorów. Aby uzyskać więcej informacji na temat tych typów zasobów, zobacz [zasoby systemu Android](~/android/app-fundamentals/resources-in-android/index.md).

U dołu selektora urządzenie znajduje się menu **Dostosuj** opcji, jak pokazano poniżej:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Menu selektora urządzenia](resource-qualifiers-images/vs/01-device-selector-sml.png)](resource-qualifiers-images/vs/01-device-selector.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Menu selektora urządzenia](resource-qualifiers-images/xs/01-device-selector-sml.png)](resource-qualifiers-images/xs/01-device-selector.png#lightbox)

-----


Wybieranie **Dostosuj** Wyświetla okno dialogowe, która umożliwia przeglądanie za pomocą konfiguracji dostępnych urządzeń. Po kliknięciu **definicji urządzenia** kartę, zobaczy listy definicji wszystkich znanych urządzeń:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Menedżera AVD](resource-qualifiers-images/vs/02-device-definitions-sml.png)](resource-qualifiers-images/vs/02-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Menedżera AVD](resource-qualifiers-images/xs/02-device-definitions-sml.png)](resource-qualifiers-images/xs/02-device-definitions.png#lightbox)

-----


Nie można zmodyfikować urządzeń we wstępnie skonfigurowanym w projektancie. Jednakże, możesz kliknąć pozycję **tworzenie urządzenia...**  do definiowania definicji urządzeń niestandardowych. Alternatywnie można wybrać istniejącą definicję i kliknąć **klonowania...**  będzie używany jako punkt początkowy dla nowych definicji.
Na przykład wybranie **Nexus 5** definicji i klikając **klonowania...**  wyświetla następujące okno dialogowe:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Klonowanie urządzenia](resource-qualifiers-images/vs/03-clone-sml.png)](resource-qualifiers-images/vs/03-clone.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Klonowanie urządzenia](resource-qualifiers-images/xs/03-clone-sml.png)](resource-qualifiers-images/xs/03-clone.png#lightbox)

-----


W następnym zrzucie ekranu Nazwa jest zmieniana na **Nexus 5 niestandardowe** i parametrów urządzenia, które są modyfikowane w celu utwórz nową definicję urządzeń niestandardowych. W tym przykładzie **pionowa** jest wyłączony, tak aby definicję urządzenia jest tylko do orientacji:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Niestandardowe](resource-qualifiers-images/vs/04-custom-sml.png)](resource-qualifiers-images/vs/04-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Niestandardowe](resource-qualifiers-images/xs/04-custom-sml.png)](resource-qualifiers-images/xs/04-custom.png#lightbox)

-----


Klikając **klonowania urządzenie** tworzy nową definicję urządzenia pojawi się w **definicji urządzenia** listy:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Urządzenie zaktualizowane definicje](resource-qualifiers-images/vs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/vs/05-updated-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Urządzenie zaktualizowane definicje](resource-qualifiers-images/xs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/xs/05-updated-device-definitions.png#lightbox)

-----


Należy pamiętać o tym, czy każda definicja urządzeń utworzonych przez użytkownika są wyświetlane z zieloną ikonę, jak pokazano powyżej. Po powrocie do **urządzenia** menu selektora nową definicję urządzenia niestandardowe są prezentowane w sekcji najwyższego poziomu listy (Jeśli nie widzisz niestandardowej konfiguracji urządzenia sieci w tej liście, spróbuj ponownie uruchomić środowisko IDE):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Niestandardowe urządzenie znajduje się na liście urządzeń](resource-qualifiers-images/vs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/vs/06-nexus-5-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Niestandardowe urządzenie znajduje się na liście urządzeń](resource-qualifiers-images/xs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/xs/06-nexus-5-custom.png#lightbox)

-----


Wybranie tej konfiguracji urządzenia modyfikuje układu do dostosowania utworzone wcześniej (w tym trybie przypadków należy korzystać tylko z pozioma):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Urządzenie niestandardowe w użyciu](resource-qualifiers-images/vs/07-custom-in-use-sml.png)](resource-qualifiers-images/vs/07-custom-in-use.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Urządzenie niestandardowe w użyciu](resource-qualifiers-images/xs/07-custom-in-use-sml.png)](resource-qualifiers-images/xs/07-custom-in-use.png#lightbox)

-----



## <a name="resource-qualifier-options"></a>Opcje kwalifikatora zasobu

**Opcje kwalifikatora zasobu** jest możliwy, klikając trzy kropki znajdujące się po prawej stronie **konfiguracji urządzenia** opcje:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Opcje kwalifikatora zasobu](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Opcje kwalifikatora zasobu](resource-qualifiers-images/xs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt.png#lightbox)

-----


To okno dialogowe wyświetla menu rozwijane dla następujących kwalifikatory zasobów:

-   **Język** &ndash; Wyświetla zasoby dostępny język i oferuje możliwość dodawania nowych zasobów języka i regionu.

-   **Tryb interfejsu użytkownika** &ndash; tryby wyświetlania list (takich jak **stacja dokująca w samochodzie** i **stacja dokująca na biurku**) oraz wskazówki układu.

Każda z tych menu rozwijanego powoduje otwarcie nowego okna dialogowe, której można wybrać i skonfigurować kwalifikatory zasobów (jak wyjaśniono dalej).



### <a name="language"></a>Język

**Języka** menu rozwijanego wyświetla tylko te języki, które mają zasoby zdefiniowane (lub **wszystkie języki**, co jest ustawieniem domyślnym). Dostępna jest również **Dodaj język/region...**  opcja, która pozwala dodać nowy język do listy:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dodaj język/region](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dodaj język/region](resource-qualifiers-images/xs/09-add-language-region-sml.png)](resource-qualifiers-images/xs/09-add-language-region.png#lightbox)

-----


Po kliknięciu **Dodaj język/region...** , **wybierz język** zostanie otwarte okno dialogowe, aby wyświetlić listy rozwijane w językach i regionach:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Lista języków](resource-qualifiers-images/vs/10-languages.png "listy języków")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Lista języków](resource-qualifiers-images/xs/10-languages-sml.png)](resource-qualifiers-images/xs/10-languages.png#lightbox)

-----


W tym przykładzie Wybraliśmy **fr (francuski)** dla języka i **BE** (Belgia) dla regionalnych dialekt języka francuskiego. Należy pamiętać, że **Region** pole jest opcjonalne, ponieważ wiele języków, które można określić bez względu na określonych regionów. Gdy **języka** ponownym otwarciu menu rozwijanego, wyświetla się zasobów nowo dodane język/region:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Wybrany Region i język](resource-qualifiers-images/vs/11-language-region-added.png "wybranego języka i regionu")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Język i Region wybrany](resource-qualifiers-images/xs/11-language-region-added-sml.png)](resource-qualifiers-images/xs/11-language-region-added.png#lightbox)

-----


Należy pamiętać, że jeśli dodasz nowy język, ale nie tworzyć nowe zasoby, aby wykazać, że dodano języka nie będą już przy następnym otwarciu projektu.



### <a name="ui-mode"></a>Tryb interfejsu użytkownika

Po kliknięciu **tryb interfejsu użytkownika** menu rozwijanego, Lista trybów jest wyświetlana, takich jak **normalny**, **stacja dokująca w samochodzie**, **stacja dokująca na biurku**, **Telewizyjnych**, **urządzenia**, i **Obejrzyj**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Menu Tryb interfejsu użytkownika](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

Poniżej tej listy są tryby nocy **nie noc** i **nocy**, a następnie kierunkach układ **od lewej do prawej** i **od prawej do lewej** (dla informacje o **od lewej do prawej** i **od prawej do lewej** opcji, zobacz [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).
Ostatnie elementy w **Opcje kwalifikatora zasobu** to okno dialogowe **zaokrąglanie ekrany** (do użytku z systemem Android Wear) lub **zaokrąglać ekrany** (uzyskać informacji na temat działania i bez działanie ekrany, zobacz [układy](https://developer.android.com/training/wearables/ui/layouts.html)).
Aby uzyskać więcej informacji na temat trybów interfejsu użytkownika dla systemu Android, zobacz [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Menu Tryb interfejsu użytkownika](resource-qualifiers-images/xs/12-ui-mode-sml.png)](resource-qualifiers-images/xs/12-ui-mode.png#lightbox)

Poniżej tej listy są tryby nocy **nie noc** i **nocy**, a następnie kierunkach układ **od lewej do prawej** i **od prawej do lewej**. Aby uzyskać więcej informacji na temat trybów interfejsu użytkownika dla systemu Android, zobacz [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
Aby uzyskać informacje o **od lewej do prawej** i **od prawej do lewej** opcji, zobacz [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).

### <a name="round-screen"></a>Okrągły ekran

Ostatnim elementem w **Opcje kwalifikatora zasobu** okno dialogowe jest **Round ekranu** menu. To menu umożliwia wybranie jednej **zaokrąglanie ekrany** (do użytku z systemem Android Wear) lub **prostokątnymi ekranami**:

[![Okrągły ekran menu](resource-qualifiers-images/xs/13-round-screen-sml.png)](resource-qualifiers-images/xs/13-round-screen.png#lightbox)

-----



## <a name="action-bar-settings"></a>Ustawienia paska akcji

**Ustawienia paska akcji** ikona jest dostępna po lewej stronie ikonę pędzla (Edytor motywów):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ustawienia paska akcji](resource-qualifiers-images/vs/14-action-bar.png "ustawienia paska akcji")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Ustawienia paska akcji](resource-qualifiers-images/xs/13b-action-bar-sml.png)](resource-qualifiers-images/xs/13b-action-bar.png#lightbox)

-----


Ta ikona otwiera popover okna dialogowego, która zapewnia sposób wybierz jedną z trzech trybów pasek akcji:

-   **Standardowa** &ndash; składa się z obu logo lub ikonę i tytuł tekstu za pomocą opcjonalnych podtytuł.

-   **Lista** &ndash; tryb nawigacji listy. Zamiast tekstu statycznego tytuł w tym trybie wyświetli menu listy do nawigacji w ramach działania (czyli przedstawienia użytkownikowi jako listy rozwijanej).

-   **Karty** &ndash; tryb nawigacji kartę. Zamiast tekstu statycznego tytuł w tym trybie przedstawia szereg kart nawigacji w ramach działania.



## <a name="themes"></a>Motywy

**Motyw** menu rozwijane wyświetli wszystkie motywy zdefiniowane w projekcie. Wybieranie **więcej motywów** otwiera okno dialogowe z listą dostępnych z zainstalowanego zestawu SDK systemu Android, wszystkie motywy, jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Więcej motywów listy](resource-qualifiers-images/vs/15-theme-menu-sml.png "listy więcej motywów")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Więcej motywów listy](resource-qualifiers-images/xs/14-theme-menu-sml.png)](resource-qualifiers-images/xs/14-theme-menu.png#lightbox)

-----


Po wybraniu motyw na powierzchnię projektową jest aktualizowana w celu wyświetlenia efekt nowy motyw. Należy pamiętać, że ta zmiana jest trwałe, tylko wtedy, gdy **OK** przycisku w **motyw** okna dialogowego. Po wybraniu motyw zostaną uwzględnione w **motyw** menu rozwijanego jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Motyw jasny jest teraz dostępna](resource-qualifiers-images/vs/16-light-theme.png "motyw jasny jest teraz dostępna")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Motyw jasny jest teraz dostępna](resource-qualifiers-images/xs/15-light-theme-sml.png)](resource-qualifiers-images/xs/15-light-theme.png#lightbox)

-----



## <a name="android-version"></a>Wersja systemu android

Android **wersji** selektor ustawia wersja systemu Android, który jest używany do renderowania układów w projektancie. Selektor Wyświetla wszystkie wersje, które są zgodne z wersji struktury docelowej projektu:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Lista wszystkich wersji systemu Android](resource-qualifiers-images/vs/17-android-version.png "listy z systemem Android w wersjach")

Można ustawić wersji platformy docelowej projektu w ustawieniach w obszarze **właściwości > aplikacji > Kompilowanie przy użyciu systemu Android w wersji**. Aby uzyskać więcej informacji o wersji platformy docelowej, zobacz [poziomy interfejsu API systemu Android w interpretacji](~/android/app-fundamentals/android-api-levels.md).

Zestaw elementów widget, dostępne w przyborniku, zależy od wersji struktury docelowej projektu. Dotyczy to również właściwości, które są dostępne w **okno właściwości**. Jest dostępna lista elementów widget *nie* ustalany na podstawie wartości wybranej **wersji** selektor paska narzędzi. Na przykład, jeśli wersją docelową projektu jest ustawiona na Android 4.4, nadal możesz wybrać system Android 6.0 w selektorze wersja narzędzi, aby zobaczyć, jak wygląda projektu dla systemu Android 6.0, ale nie będzie można dodać elementy widget, które są specyficzne dla systemu Android 6.0 &ndash;  nadal będzie ograniczony do elementów widget, które są dostępne w systemie Android 4.4.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Lista wszystkich wersji systemu Android](resource-qualifiers-images/xs/16-android-version-sml.png)](resource-qualifiers-images/xs/16-android-version.png#lightbox)

Można ustawić wersji platformy docelowej projektu w ustawieniach w obszarze **opcje projektu > kompilacji > Ogólne** sekcji. Aby uzyskać więcej informacji o wersji platformy docelowej, zobacz [poziomy interfejsu API systemu Android w interpretacji](~/android/app-fundamentals/android-api-levels.md).

Zestaw elementów widget, dostępne w przyborniku, zależy od wersji struktury docelowej projektu. Dotyczy to również właściwości, które są dostępne w **Konsola właściwość**. Jest dostępna lista elementów widget *nie* ustalany na podstawie wartości wybranej **wersji** selektor paska narzędzi. Na przykład, jeśli wersją docelową projektu jest ustawiona na Android 4.4, nadal możesz wybrać system Android 6.0 w selektorze wersja narzędzi, aby zobaczyć, jak wygląda projektu dla systemu Android 6.0, ale nie będzie można dodać elementy widget, które są specyficzne dla systemu Android 6.0 &ndash;  nadal będzie ograniczony do elementów widget, które są dostępne w systemie Android 4.4.

-----
