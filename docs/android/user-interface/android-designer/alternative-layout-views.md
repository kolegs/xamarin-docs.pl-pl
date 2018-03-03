---
title: "Widoki alternatywnych układu"
description: "W tym temacie wyjaśniono, jak układów można numerów wersji przy użyciu kwalifikatory zasobów. Na przykład może istnieć wersję układu, który jest używany tylko, gdy urządzenie jest w trybie krajobraz i tylko w trybie portret wersji układu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: f6884260315f8846720370c558f7435d2c5a9d91
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="alternative-layout-views"></a>Widoki alternatywnych układu

_W tym temacie wyjaśniono, jak układów można numerów wersji przy użyciu kwalifikatory zasobów. Na przykład może istnieć wersję układu, który jest używany tylko, gdy urządzenie jest w trybie krajobraz i tylko w trybie portret wersji układu._

<a name="creating_alternative_layouts" />

## <a name="creating-alternative-layouts"></a>Tworzenie układów alternatywnych

Po kliknięciu **alternatywnych widoku układu** ikona (z lewej strony **urządzenia**), Otwiera okienko podglądu listę układów alternatywne dostępne w projekcie. Jeśli nie ma żadnych alternatywnych układów **domyślne** przedstawiono widok: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Alternatywne układ widoku okienka](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "okienka widoku układu alternatywnej")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![W okienku widoku Układ alternatywnej](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png)

-----

Po kliknięciu zielony znak plus obok **nowej wersji**, **utworzyć zmiany układu** zostanie otwarte okno dialogowe, w którym można wybrać kwalifikatory zasobów dla tej zmiany układu: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tworzenie układu odmiany](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "tworzenie odmiany układu")](alternative-layout-views-images/vs/02-create-layout-variation.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Tworzenie układu odmiany](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png)

-----


W poniższym przykładzie kwalifikator zasobów dla **orientacji ekranu** ustawiono **pozioma**i **rozmiaru ekranu** jest zmieniana na **duży**. Spowoduje to utworzenie nowej wersji układu o nazwie **dużych ziemi**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Duży ziemi odmiany](alternative-layout-views-images/vs/03-large-land-sml.png "odmiany ziemi duży")](alternative-layout-views-images/vs/03-large-land.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Zmiana ziemi duży](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png)

-----


Należy pamiętać, że okienko podglądu po lewej stronie wyświetla skutków opcje kwalifikator zasobów. Kliknięcie przycisku **Dodaj** tworzy alternatywnych układ i zmienia do tego układu projektanta. **Widoku układu alternatywna** okienko podglądu wskazuje wyboru układu jest ładowany do projektanta za pośrednictwem małych wskaźnika prawo, jak wskazano na poniższym zrzucie ekranu: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wskaźnik załadować układu](alternative-layout-views-images/vs/04-new-layout-sml.png "wskaźnik załadować układu")](alternative-layout-views-images/vs/04-new-layout.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Wskaźnik załadować układu](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png)

-----


<a name="editing_alternative_layouts" />

## <a name="editing-alternative-layouts"></a>Edytowanie alternatywnych układów

Podczas tworzenia alternatywnych układów często jest pożądane, aby jednej zmiany, która ma zastosowanie do wszystkich wersji rozwidlonych układu. Na przykład można zmienić tekst przycisku żółty w wszystkich układów. Jeśli masz dużą liczbę układów konserwacji może szybko stać się skomplikowane błędów oraz jeśli potrzebujesz propagację pojedynczej zmiany do wszystkich wersji. 

Aby uprościć zarządzanie wieloma wersjami układu, Projektant udostępnia **edycji wielu** tryb, który rozprzestrzenia się zmiany w co najmniej jeden układ. Jeśli występuje więcej niż jeden układ **edycji wielu** jest wyświetlana ikona: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Ikona edytowania wielu](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "ikonę edycji wielu")](alternative-layout-views-images/vs/05-multi-layout-icon.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Ikona wielu edycji](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png)

-----


Po kliknięciu **edycji wielu** ikona, są wyświetlane linie wskazujące, że układów połączono (jak pokazano poniżej); oznacza to, że po wprowadzeniu zmian w jeden układ taka zmiana jest propagowana do żadnych połączonego układów. Można odłączyć wszystkich układów, klikając ikonę kółku wskazanych na poniższym zrzucie ekranu: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Rozłącz wszystkich układów](alternative-layout-views-images/vs/06-multi-linked-sml.png "odłączyć wszystkich układów")](alternative-layout-views-images/vs/06-multi-linked.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Rozłącz wszystkich układów](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png)

-----


Jeśli masz więcej niż dwóch układów można selektywnie Przełącz przycisk Edytuj z lewej strony każdego Podgląd układu, aby określić, które układów są połączone ze sobą. Na przykład, jeśli chcesz utworzyć jednej zmiany, które propaguje pierwszy i ostatni trzy układy, czy najpierw odłączenie środkowej układu w sposób pokazany poniżej: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Rozłącz środkowej układu](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "układu środkowej Rozłącz")](alternative-layout-views-images/vs/07-unlink-middle-layout.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Rozłącz środkowej układu](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png)
 
-----
 

W tym przykładzie zmienione jako **domyślne** lub **długi** układu będzie propagowane do innych układu, ale nie do **dużych ziemi** układu. 


<a name="multi_edit_example" />

### <a name="multi-edit-example"></a>Przykład wielu edycji 

Ogólnie rzecz biorąc po wprowadzeniu zmian w jeden układ tego samego zmiana jest propagowana do wszystkich innych połączonych układów. Na przykład dodać nowy `TextView` elementu widget do **domyślne** układ i zmiana jego tekstu ciąg `Portrait` spowoduje tej samej zmiany do wszystkich połączonych układów. Oto, jak wygląda **domyślne** układu: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dodaj element TextView](alternative-layout-views-images/vs/08-add-textview-sml.png "Dodaj element TextView")](alternative-layout-views-images/vs/08-add-textview.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Dodaj element TextView](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png)
 
-----
 

`TextView` Jest także dodawane do **dużych ziemi** układ widoku, ponieważ jest on połączony **domyślne** układu: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Poziomą TextView](alternative-layout-views-images/vs/09-landscape-textview-sml.png "poziomą TextView")](alternative-layout-views-images/vs/09-landscape-textview.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Element TextView orientacji poziomej](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png)
 
-----
 

Ale co zrobić, jeśli chcesz wprowadzić zmianę, która jest tylko jeden układ lokalną (to znaczy, że nie chcesz zmiany są propagowane do żadnego z innych układów)? Aby to zrobić, musisz odłączyć układu, który chcesz zmienić przed przystąpieniem do modyfikacji, jak wyjaśniono dalej. 


<a name="making_local_changes" />

### <a name="making-local-changes"></a>Wprowadzanie zmian lokalnych 

Załóżmy, że chcemy mieć dodany obu układów `TextView`, ale chcemy też zmienić ciągu tekstowego w **dużych ziemi** układu `Landscape` zamiast `Portrait`. Jeśli firma Microsoft Ta zmiana **dużych ziemi** podczas obu układów są połączone, zmiany zostaną przeniesione do **domyślne** układu. W związku z tym firma Microsoft musi najpierw odłączyć dwóch układów przed zmiany. Gdy firma Microsoft zmodyfikuj tekst w **dużych ziemi** do `Landscape`, Projektant oznacza tej zmiany z czerwonym ramką, aby wskazać, że zmiana jest lokalny dla **dużych ziemi** układ i jest *nie* propagowane do **domyślne** układu: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zmiana lokalna](alternative-layout-views-images/vs/10-local-change-sml.png "lokalne zmiany")](alternative-layout-views-images/vs/10-local-change.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Zmiana lokalna](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png)
 
-----
 

Po kliknięciu **domyślne** układu, aby wyświetlić jego `TextView` ciąg tekstowy nadal jest ustawione `Portrait`. 


<a name="handling_conflicts" />

## <a name="handling-conflicts"></a>Konfliktów 

Jeśli zdecydujesz się zmienić kolor tekstu w **domyślne** układu zielony, pojawi się ikona ostrzeżenia są wyświetlane w układzie połączonej. Kliknięcie tego układu otwiera układu, aby wyświetlić konflikt. Elementu widget, które spowodowało konflikt jest wyróżniony czerwonym ramki i zostanie wyświetlony następujący komunikat: *najnowszych zmian spowodował konfliktów w tym układzie alternatywnych*. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zmiany powodujące konflikt](alternative-layout-views-images/vs/11-conflicting-change-sml.png "zmiany powodujące konflikt")](alternative-layout-views-images/vs/11-conflicting-change.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Powodujące konflikt zmiany](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png)
 
-----
 

A *okno konfliktów* jest wyświetlana po prawej stronie widżetu wyjaśnić konfliktu: 

[ ![Ostrzeżenie konflikt](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png)

Pole konflikt zawiera listę właściwości, które zostały zmienione, a także ich wartości. Kliknięcie przycisku **Ignoruj konflikt** stosuje zmiany właściwości tylko do tego elementu widget. Kliknięcie przycisku **Zastosuj** stosuje zmiany właściwości do tego elementu widget oraz aby widżetów duplikat w połączonych **domyślne** układu. Jeśli wszystkie zmiany właściwości są stosowane, konflikt jest automatycznie odrzucane. 

<a name="view_group_conflicts" />

### <a name="view-group-conflicts"></a>Konflikty widoku grupy 

Zmiany właściwości nie są jedyne źródło konfliktów. Konflikty mogą być wykrywane podczas wstawiania lub usuwania elementów widget. Na przykład, gdy **dużych ziemi** układ jest rozłączony z **domyślne** układ i `TextView` w **dużych ziemi** układ jest przeciągnięto i Upuszczono powyżej `Button`, Projektant oznacza przeniesionego widget wskazująca konflikt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wyświetl konflikt grupy](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "wyświetlić konflikt grupy")](alternative-layout-views-images/vs/12-view-group-conflict.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Widok grupy konflikt](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png)
 
-----
 

Jednak nie ma żadnych znaczników na `Button`. Chociaż położenie `Button` uległ zmianie, `Button` pokazuje ma zastosowane zmian, które są specyficzne dla **dużych ziemi** konfiguracji. 

Jeśli `CheckBox` jest dodawany do **domyślne** układu, jest generowany innego konflikt i jest wyświetlana ikona ostrzeżenia za pośrednictwem **dużych ziemi** układu: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Konflikt wyboru](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "konflikt wyboru")](alternative-layout-views-images/vs/13-checkbox-conflict.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Konflikt wyboru](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png)
 
-----
 

Kliknięcie przycisku **dużych ziemi** układu ujawnia konflikt. Zostanie wyświetlony następujący komunikat: *najnowszych zmian spowodował konfliktów w tym układzie alternatywnych*. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Konflikt ALT układu](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "konflikt układu Alt")](alternative-layout-views-images/vs/14-alt-layout-conflict.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Konflikt układu ALT](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png)
 
-----
 

Ponadto w polu konflikt zostanie wyświetlony następujący komunikat:

[ ![Konflikt wiadomości](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png)

Dodawanie `CheckBox` powoduje konflikt, ponieważ **dużych ziemi** układ ma zmiany `LinearLayout` go zawiera. Jednak w takim przypadku pole konflikt zawiera element widget, która właśnie została umieszczona w **domyślne** układu ( `CheckBox`).

Jeśli klikniesz przycisk **Ignoruj konflikt**, Projektant rozwiązuje konflikt, umożliwiając widget wyświetlana w polu konflikt przeciągnąć i upuścić na układ, w której brakuje elementu widget (w tym przypadku **dużych ziemi**  układu): 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Rozwiązano konflikt grupy](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "rozwiązano konflikt grupy")](alternative-layout-views-images/vs/15-resolved-group-conflict.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Rozwiązano konflikt grupy](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png)
 
-----
 

Jak pokazano w poprzednim przykładzie z `Button`, `CheckBox` nie ma znacznik red zmian, ponieważ tylko `LinearLayout` zawiera zmiany, które zostały zastosowane w **dużych ziemi** układu.


<a name="Conflict_Persistence" />

### <a name="conflict-persistence"></a>Konflikt trwałości

Konflikty są zachowywane w pliku układu jako komentarze XML, jak pokazano poniżej:

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

W związku z tym, gdy projekt jest zamknięte i otwarte ponownie, wszystkie konflikty, będzie nadal będzie &ndash; nawet te, które zostały zignorowane.

