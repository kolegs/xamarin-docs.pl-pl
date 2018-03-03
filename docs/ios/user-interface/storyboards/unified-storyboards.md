---
title: Ujednolicone Scenorys
description: "Ujednolicone scenorys Zezwalaj iOS deweloperom tworzenie interfejsu użytkownika z jednego scenorysu, zamiast wielu scenorys, aby pokrywał większą liczbę rozmiaru ekranu urządzenia. W tym artykule jest przeznaczona do zapewniają bardziej omówienie działania ujednoliconego scenorysu w ramach platformy Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 077be02aacb9d4200db2d2eadf6f7388842b8e29
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="unified-storyboards"></a>Ujednolicone Scenorys

_Ujednolicone scenorys Zezwalaj iOS deweloperom tworzenie interfejsu użytkownika z jednego scenorysu, zamiast wielu scenorys, aby pokrywał większą liczbę rozmiaru ekranu urządzenia. W tym artykule jest przeznaczona do zapewniają bardziej omówienie działania ujednoliconego scenorysu w ramach platformy Xamarin.iOS._

System iOS 8 zawiera nowe, łatwiejsze w obsłudze mechanizm do tworzenia interfejsu użytkownika — ujednoliconą scenorysu. Pojedynczy scenorysu, obejmujące wszystkie sprzętu różnych rozmiarów ekranu, można tworzyć widoki szybkie i elastyczny w "projektu — jeden raz, użyj wielu" stylu.

Jak już deweloper musi utworzyć oddzielne i określonego scenorysu dla urządzenia iPhone i iPad, mają możliwość projektowania aplikacji za pomocą wspólny interfejs, a następnie dostosować ten interfejs dla klasy inny rozmiar. W ten sposób aplikacji mogą być dostosowywane do sile każdej obudowie i każdego interfejsu użytkownika można przedstawić zapewnia najlepsze środowisko.

<a name="size-classes" />

## <a name="size-classes"></a>Rozmiar klasy

Przed iOS 8, używane dewelopera `UIInterfaceOrientation` i `UIInterfaceIdiom` rozróżnianie między trybami pionowej i poziomej i między urządzenia iPhone i iPad. W iOS8, orientację i urządzenia jest określana za pomocą *klasy wielkości*.

Urządzenia są definiowane przez klasy wielkości w osi poziomej i pionowego, i istnieją dwa typy klas rozmiar w systemie iOS 8:

-  **Regularne** — jest to o rozmiarze większym ekranem (takich jak iPad) lub gadżet, który daje wyobrażenie o dużych rozmiarach (takich jak `UIScrollView`
-  **Compact** — to jest przeznaczona dla mniejszych urządzeń (na przykład telefon iPhone). Ten rozmiar uwzględnia orientacji urządzenia.


Jeśli używane są ze sobą dwa pojęcia, wynikiem jest siatki 2 x 2, który definiuje różne rozmiary możliwości, które mogą być używane w obu kierunkach różne, jak pokazano na poniższym diagramie:

 [ ![](unified-storyboards-images/sizeclassgrid.png "Siatkę 2 x 2, która definiuje różne rozmiary możliwości, które mogą być używane w zwykłych oraz Compact orientacji")](unified-storyboards-images/sizeclassgrid.png)

Deweloper może utworzyć kontroler widoku korzystający z czterech możliwości, które umożliwiałyby układów (jak w powyższym grafiki).

### <a name="ipad-size-classes"></a>iPad klasy wielkości

Ma iPad, ze względu na rozmiar **regularne** klasy rozmiar dla obu orientacji.

 [ ![](unified-storyboards-images/image1.png "iPad klasy wielkości")](unified-storyboards-images/image1.png)


### <a name="iphone-size-classes"></a>iPhone klasy wielkości

IPhone ma inny rozmiar klasy oparte na orientacji urządzenia:

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone klasy wielkości")](unified-storyboards-images/iphonesizeclasses.png)

-  Gdy urządzenie jest w trybie portret, ekran ma **compact** poziomie klasy i **regularne** pionowo
-  Gdy urządzenie jest w trybie krajobraz, klasy ekranu są wycofywane w trybie portret.

### <a name="iphone-6-plus-size-classes"></a>iPhone 6 Plus rozmiar klasy

Rozmiary są takie same jak wcześniej iPhone w orientacji pionowej, ale o różnych w orientacji poziomej:

[![](unified-storyboards-images/iphone6sizeclasses.png "iPhone 6 Plus rozmiar klasy")](unified-storyboards-images/iphone6sizeclasses.png)

Ponieważ telefonów iPhone 6 Plus ma ekran wystarczająco duży, może mieć zwykłej klasy szerokości rozmiaru w trybie krajobraz.

### <a name="support-for-a-new-screen-scale"></a>Obsługa nowych skalowanie ekranu

IPhone 6 Plus używa nowego ekranu HD siatkówki współczynnik skali ekranu 3.0 (trzy razy oryginalnego iPhone rozdzielczość ekranu). Aby zapewnić najlepsze możliwe środowisko na tych urządzeniach, obejmują Nowa kompozycja przeznaczony dla tej skali ekranu. W środowisku Xcode 6 lub nowszym zasobów katalogi dołączać obrazy 1 x, 2 x i 3 x rozmiary; po prostu Dodaj nowe zasoby obrazów i iOS wybierze poprawne zasobów podczas uruchamiania na telefonie iPhone 6 Plus.

Rozpoznaje obrazu zachowania w systemie iOS ładowania również `@3x` sufiks na pliki obrazów. Na przykład, jeśli dewelopera zawiera zasób obrazu (na inną rozdzielczość) w pakiecie aplikacji o następujących nazwach: `MonkeyIcon.png`, `MonkeyIcon@2x.png`, i `MonkeyIcon@3x.png`. Na telefonie iPhone 6 Plus `MonkeyIcon@3x.png` obrazu użyte automatycznie dewelopera ładuje obraz, używając następującego kodu:

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
Lub jeśli obraz są przypisane do elementu interfejsu użytkownika za pomocą systemu iOS projektanta jako `MonkeyIcon.png`, `MonkeyIcon@3x.png` będzie używany, ponownie automatycznie, na telefonach iPhone 6 Plus.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>Ekrany dynamicznego uruchamiania

Uruchom plik ekranu zostanie wyświetlony jako ekran powitalny aplikacji systemu iOS jest uruchamiany w celu otrzymania opinii dla użytkownika, która aplikacja jest rzeczywiście uruchamianie w górę. Przed iOS 8, deweloper musi zawierać wiele `Default.png` obrazu zasobów dla każdego urządzenia typu i orientacji ekranu rozdzielczość aplikacja będzie działać na.

Nowe w systemach iOS 8, deweloper można utworzyć jeden atomic `.xib` pliku w programie Xcode, korzystającej z klasy rozmiaru i układu automatycznego tworzenia *dynamiczne ekranu uruchamiania* który będzie działać dla każdego urządzenia, rozdzielczość i orientacji. To nie tylko zmniejsza ilość pracy wymaganej od dewelopera można tworzyć i obsługiwać wszystkie zasoby wymagane obrazu, ale zmniejsza rozmiar zainstalowanego pakietu aplikacji.

## <a name="traits"></a>Cechy

Cechy są właściwości, które mogą służyć do określenia, jak zmienia układ jako jego zmiany środowiska. Składają się z zestawem właściwości ( `HorizontalSizeClass` i `VerticalSizeClass` na podstawie `UIUserInterfaceSizeClass`), a także idiom interfejsu ( `UIUserInterfaceIdiom`) i skalowanie ekranu.

Wszystkie powyższe Państwa są opakowywane w w kontenerze, który Apple odwołuje się do jako kolekcja cechy ( `UITraitCollection`), która zawiera nie tylko właściwości, ale również ich wartości.

## <a name="trait-environment"></a>Cechy środowiska

Środowisk cechy są nowy interfejs w systemie iOS 8 i mogą zwracać kolekcji cechy dla następujących obiektów:

-  Ekrany ( `UIScreens` ).
-  Windows ( `UIWindows` ).
-  Wyświetl kontrolerów ( `UIViewController` ).
-  Widoki ( `UIView` ).
-  Kontroler prezentacji ( `UIPresentationController` ).


Deweloper używa kolekcji cechy zwrócony przez środowisko cechy w celu określenia, interfejs użytkownika powinna być układ.

Wprowadź wszystkich środowisk cechy hierarchii, jak pokazano na poniższym diagramie:

 [ ![](unified-storyboards-images/viewhierarchy.png "Diagram hierarchii środowisk cechy")](unified-storyboards-images/viewhierarchy.png)

Kolekcja cechy każdego z powyższych środowisk cechy ma będą przepływać z obiektu nadrzędnego środowiska podrzędnego domyślnie.

Oprócz pobierania bieżącej kolekcji cechy, ma środowisko cechy `TraitCollectionDidChange` metodę, która może zostać zastąpiona w podklasach widoku lub widok kontroler. Deweloper można użyć tej metody, aby zmodyfikować dowolne elementy interfejsu użytkownika, które są zależne od cech, gdy tych cech zostały zmienione.

## <a name="typical-trait-collections"></a>Typowe cechy kolekcje

W tej sekcji opisano typy typowe cechy kolekcje, które użytkownik może wystąpić podczas pracy z systemem iOS 8.

Poniżej przedstawiono typowe cechy kolekcji deweloper może być wyświetlana na telefonie iPhone:

<table width="100%" border="1px">
<thead>
<tr>
    <td>Właściwość</td>
    <td>Wartość</td>
</tr>
</thead>
<tbody>
<tr>
    <td><code>HorizontalSizeClass</code></td>
    <td>CD</td>
</tr>
<tr>
    <td><code>VerticalSizeClass</code></td>
    <td>Regularne</td>
</tr>
<tr>
    <td><code>UserInterfaceIdom</code></td>
    <td>Telefon</td>
</tr>
<tr>
    <td><code>DisplayScale</code></td>
    <td>2.0</td>
</tr>
</tbody>
</table>

Powyżej zestawu czy stanowiące pełni kwalifikowaną cechy kolekcję, ponieważ nie ma ona wartości dla wszystkich właściwości cechy.

Istnieje również możliwość ma kolekcji cechy, w którym brakuje niektórych wartości jego (czyli Apple jako *nieokreślony*):

<table width="100%" border="1px">
<thead>
<tr>
    <td>Właściwość</td>
    <td>Wartość</td>
</tr>
</thead>
<tbody>
<tr>
    <td><code>HorizontalSizeClass</code></td>
    <td>CD</td>
</tr>
<tr>
    <td><code>VerticalSizeClass</code></td>
    <td>{nieokreślonych}</td>
</tr>
<tr>
    <td><code>UserInterfaceIdom</code></td>
    <td>{nieokreślonych}</td>
</tr>
<tr>
    <td><code>DisplayScale</code></td>
    <td>{nieokreślonych}</td>
</tr>
</tbody>
</table>

Ogólnie rzecz biorąc jednak podczas dewelopera wprowadza się jego kolekcja cechy środowiska cechy, zwróci pełną kolekcję jak w powyższym przykładzie.

Jeśli środowisku cechy (na przykład wyświetlanie lub View Controller) nie jest wewnątrz bieżącej hierarchii widoku, deweloper może odzyskać nieokreślone wartości co najmniej jednej właściwości cechy.

Deweloper również uzyskać kolekcję cechy częściowo kwalifikowanej, jeśli korzystają z jednej z metod tworzenia dostarczonymi przez firmę Apple, takich jak `UITraitCollection.FromHorizontalSizeClass`, aby utworzyć nową kolekcję.

Jednej operacji, które mogą być wykonywane na wielu kolekcji cechy porównuje je ze sobą, które obejmuje jedną kolekcję cechy pytaniem, czy zawiera on inną. Co oznacza *zawierania* jest, że dla dowolnego cechy określone w druga kolekcja, wartość musi być zgodna z wartości z pierwszej kolekcji.

Aby przetestować użyć dwóch cech `Contains` metody `UITraitCollection` przekazując wartość cechy do sprawdzenia.

Deweloper można ręcznie wykonać porównanie kod w celu określenia sposobu układu widoki lub widok kontrolerów. Jednak `UIKit` wewnętrznie używa tej metody, aby zapewnić niektórych jej funkcji, jak serwer Proxy wygląd, na przykład.

## <a name="appearance-proxy"></a>Wygląd serwera Proxy

Proxy wygląd została wprowadzona w starszych wersjach systemu IOS umożliwiają deweloperom dostosowywanie właściwości ich widoków. Została rozszerzona w systemie iOS 8 do obsługi kolekcji cechy.

Wygląd proxy zawierają teraz nową metodę `AppearanceForTraitCollection`, która zwraca nowe Proxy wygląd dla danej kolekcji cechy, który został przekazany w. Wszystkie dostosowania, które wykonuje dewelopera, na które Proxy wygląd tylko zacznie obowiązywać w widoków, które są zgodne w określonej kolekcji cechy.

Ogólnie rzecz biorąc, dewelopera przechodzą w częściowo określonej kolekcji cechy do `AppearanceForTraitCollection` metody, takiej jak określone właśnie poziomy rozmiar klasy z Compact, tak aby ich można dostosować dowolnym widoku w aplikacji, która jest compact w poziomie.

## <a name="uiimage"></a>UIImage

Innej klasy, która Apple został dodany cechy kolekcji jest `UIImage`. W przeszłości dewelopera musiał określić @1X i @2x wersji mapy bitowej zasobów graficznych, który miał zostać uwzględnione w aplikacji (takich jak ikony).

Aby umożliwić deweloperowi zawiera wiele wersji obrazu w katalogu obrazu oparte na kolekcji cechy została rozszerzona iOS 8. Na przykład deweloper może zawierać mniejszego obrazu do pracy z klasą Compact cechy i pełnego obrazu o rozmiarze do innej kolekcji.

Jeśli jeden z obrazów jest używany wewnątrz `UIImageView` klasy, widok obrazu zostanie automatycznie wyświetlona poprawną wersję obrazu dla jego kolekcja cechy. W przypadku zmiany w środowisku cechy (na przykład użytkownik przełączenie urządzenia z pionowej na poziomą), widok obrazu automatycznie wybiera nowy rozmiar obrazu umożliwia dopasowanie Nowa kolekcja cechy i zmianę rozmiaru odpowiadać bieżąca wersja jest obrazu wyświetlane.

## <a name="uiimageasset"></a>UIImageAsset

Apple zostało dodane nowe klasy w systemach iOS 8 o nazwie `UIImageAsset` umożliwiają dewelopera większą kontrolę nad wybór obrazu.

Zasób obrazu opakowuje zapasowej wszystkich innych wersji obrazu i umożliwia deweloperowi poproś dla określonego obrazu, który pasuje do kolekcji cechy został przekazany w. Obrazy mogą zostać dodane lub usunięte z zasób obrazu na bieżąco.

Aby uzyskać więcej informacji na zasoby obrazów, zobacz firmy Apple [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) dokumentacji.

## <a name="combining-trait-collections"></a>Łączenie kolekcji cechy

Innej funkcji, która deweloper może przeprowadzać w kolekcjach cechy jest dodanie dwóch razem ten będzie skutkować połączonych kolekcji, której nieokreślone wartości z jednej kolekcji są zastępowane przez wartości w polach drugie. Jest to realizowane przy użyciu `FromTraitsFromCollections` metody `UITraitCollection` klasy.

Jak już wspomniano, jeśli wszystkie cechy jest nieokreślona w jednej z kolekcji cechy i jest określona w innym wartość zostanie ustawiona na określonej wersji. Jednak w przypadku wielu wersji określoną wartość danej wartości w ciągu ostatnich cechy kolekcji będą wartość, która jest używana.


## <a name="adaptive-view-controllers"></a>Kontrolery adaptacyjną widoku

W tej sekcji opisano szczegóły jak iOS widoku oraz widoku kontrolerów wdrożyły cech i klasy rozmiar automatycznie za więcej adaptacyjną w aplikacjach dewelopera.

### <a name="split-view-controller"></a>Kontroler widoku podziału

Jednym z klasy kontrolera widoku, które zmieniło się najbardziej w iOS 8 jest `UISplitViewController` klasy. W przeszłości dewelopera często użyje kontrolera widoku podziału na wersję iPad aplikacji, a następnie zostałyby udostępnia dwóch zupełnie różnych wersji ich hierarchii widoku dla wersji iPhone aplikacji.

W systemie iOS 8 `UISplitViewController` klasa znajduje się na obu platform (iPhone i iPad), który umożliwia deweloperom tworzenie jednej hierarchii kontrolera widoku, który będzie działać dla urządzenia iPhone i iPad.

Po iPhone w orientacji poziomej kontroler widoku podziału przedstawi jego widoków side-by-side, tak samo, jak to by się działo podczas będzie wyświetlany na urządzeniu iPad.

### <a name="overriding-traits"></a>Zastępowanie cech

Cechy środowisk Kaskadowo z kontenera nadrzędnego do kontenerów podrzędnych, tak jak poniższy rysunek, przedstawiający kontrolera widoku podziału na urządzeniu iPad w orientacji poziomej:

 [ ![](unified-storyboards-images/cascadingclasses01.png "Kontrolera widoku podziału na urządzeniu iPad w orientacji poziomej")](unified-storyboards-images/cascadingclasses01.png)

Ponieważ tabletów iPad ma zwykłej klasy rozmiaru w poziomie i w pionie orientacji, Podziel zostaną wyświetlone widoki głównego i szczegółów.

Na telefonie iPhone, gdy klasa rozmiar jest compact w obu kierunkach, Podziel kontroler widoku wyświetlane są tylko widoku szczegółów, jak pokazano poniżej:

 [ ![](unified-storyboards-images/cascadingclasses02.png "Podziel kontroler widoku wyświetlane są tylko widok szczegółowy")](unified-storyboards-images/cascadingclasses02.png)

W aplikacji, gdzie deweloper chce wyświetlania widoku głównego i szczegółów na telefonie iPhone w orientacji poziomej Deweloper należy wstawić kontenera nadrzędnego dla kontrolera widoku podziału i zastąpić kolekcji cechy. Jak pokazano na rysunku poniżej:

 [ ![](unified-storyboards-images/cascadingclasses03.png "Deweloper musi Wstaw kontenera nadrzędnego dla kontrolera widoku podziału i zastąpić kolekcji cechy")](unified-storyboards-images/cascadingclasses03.png)

A `UIView` jest ustawiony jako element nadrzędny kontroler widoku podziału i `SetOverrideTraitCollection` metoda jest wywoływana w widoku, przekazując nową kolekcję cechy i przeznaczonych dla kontrolera widoku podziału. Nowa kolekcja cechy zastępuje `HorizontalSizeClass`, ustawieniem dla niego `Regular`, dzięki czemu podział kontroler widoku spowoduje wyświetlenie głównego i szczegółowe widoki na telefonie iPhone w orientacji poziomej.

Należy pamiętać, że `VerticalSizeClass` ustawiono `unspecified`, który umożliwia Nowa kolekcja cechy ma zostać dodany do kolekcji cechy nadrzędnego, co `Compact VerticalSizeClass` podrzędnych kontrolera widoku podziału.

### <a name="trait-changes"></a>Cechy zmiany

W tej sekcji zostanie Spójrz szczegółowo w sposób kolekcje cechy przejście po zmianie środowiska cechy. Na przykład, gdy urządzenie jest obracana z pionowej na poziomą.

 [ ![](unified-storyboards-images/traittransitions01.png "Pionowej na poziomą zmiany cechy — omówienie")](unified-storyboards-images/traittransitions01.png)

Najpierw iOS 8 powoduje przywrócenie konfiguracji w celu przygotowania do przejścia. Następnie system animuje przejście stanu. Na koniec iOS 8 czyści up wszystkie stany tymczasowego wymagane podczas przejścia.

System iOS 8 zawiera kilka wywołania zwrotne, które deweloper może użyć do udziału w przypadku zmiany cechy, jak pokazano w poniższej tabeli:

<table width="100%" border="1px">
<thead>
<tr>
    <td>Etap</td>
    <td>Wywołanie zwrotne</td>
    <td>Opis</td>
</tr>
</thead>
<tbody>
<tr>
    <td>Konfiguracja</td>
    <td>
        <ul>
        <li><code>WillTransitionToTraitCollection</code></li>
        <li><code>TraitCollectionDidChange</code></li>
        </ul>
    </td>
    <td>
        <ul>
        <li>Ta metoda jest wywoływana na początku zmiany cechy przed kolekcji cechy jest ustawiony na wartość nowego.</li>
        <li>Metoda jest wywoływana, gdy zmieniono wartość kolekcji cechy, ale przed dokonaniem żadnych animacji.</li>
        </ul>
    </td>
</tr>
<tr>
    <td>Animacja</td>
    <td>
        <ul>
        <li><code>WillTransitionToTraitCollection</code></li>
        </ul>
    </td>
    <td>
        <ul>
        <li>Koordynator przejścia, który zostanie przekazany do tej metody ma <code>AnimateAlongside</code> właściwość, która umożliwia deweloperowi Dodawanie animacji, które zostaną wykonane wraz z animacji domyślne.</li>
        </ul>
    </td>
</tr>
<tr>
    <td>Wyczyść</td>
    <td>
        <ul>
        <li><code>WillTransitionToTraitCollection</code></li>
        </ul>
    </td>
    <td>
        <ul>
        <li>Udostępnia metodę deweloperom zawierają własne oczyszczanie kodu po wystąpieniu przejścia.</li>
        </ul>
    </td>
</tr>
</tbody>
</table>

`WillTransitionToTraitCollection` Metody jest doskonałym rozwiązaniem dla animacji kontrolerów widoku oraz zmiany kolekcji cechy. `WillTransitionToTraitCollection` Metoda jest dostępna tylko na kontrolerach widoku ( `UIViewController`), a nie w innych środowiskach cechy, takie jak `UIViews`.

`TraitCollectionDidChange` Doskonale nadaje się do pracy z `UIView` klasy, gdzie deweloper chce uaktualnić interfejsu użytkownika, jak cechy są zmieniane.

### <a name="collapsing-the-split-view-controllers"></a>Zwijanie kontrolerów widoku podziału

Teraz załóżmy podjęcia bliższe spojrzenie na to, co się stanie w przypadku kontrolera widoku podziału zwija z dwie kolumny do widoku jedną kolumnę. W ramach tej zmiany istnieją dwa procesy, które muszą zostać wykonane:

-  Domyślnie kontroler widoku podziału użyje kontrolera widoku głównego jako widok, po wystąpieniu Zwiń. Deweloper może zmienić to zachowanie przez zastąpienie `GetPrimaryViewControllerForCollapsingSplitViewController` metody `UISplitViewControllerDelegate` i dowolnego kontrolera widoku, które mają być wyświetlane w stanie zwinięte.
-  Pomocniczy kontroler widoku ma scalony z podstawowym kontrolerem widoku. Ogólnie rzecz biorąc projektanta nie trzeba wykonywać żadnych czynności w tym kroku; Kontroler widoku podziału obejmuje automatyczną obsługę w tej fazie oparte na urządzeniu sprzętowym. Jednak może być czasami specjalne miejscu dewelopera na interakcję z tą zmianą. Wywoływanie `CollapseSecondViewController` metody `UISplitViewControllerDelegate` umożliwiający kontrolerowi widoku głównego, który będzie wyświetlany po wystąpieniu Zwiń, zamiast widoku szczegółów.


### <a name="expanding-the-split-view-controller"></a>Rozszerzanie kontroler widoku podziału

Teraz załóżmy podjęcia bliższe spojrzenie na to, co się stanie w przypadku kontrolera widoku podziału jest rozwinięta ze stanu zwinięte. Ponownie istnieją dwa etapy, które muszą zostać wykonane:

-  Najpierw należy zdefiniować nowego podstawowego kontrolera widoku. Domyślnie kontroler widoku podziału będą automatycznie używać podstawowego kontrolera widoku w widoku zwiniętym. Ponownie, deweloper może zastąpić, przy użyciu tego zachowania `GetPrimaryViewControllerForExpandingSplitViewController` metody `UISplitViewControllerDelegate` .
-  Gdy została wybrana podstawowego kontrolera widoku pomocniczy kontroler widoku muszą zostać ponownie utworzone. Ponownie kontroler widoku podziału obejmuje automatyczną obsługę w tej fazie oparte na urządzeniu sprzętowym. Deweloper może zmienić to zachowanie, wywołując `SeparateSecondaryViewController` metody `UISplitViewControllerDelegate` .


W kontrolerze widoku podziału podstawowego kontrolera widoku odgrywa rolę w rozwijanie i zwijanie widoki zaimplementowanie `CollapseSecondViewController` i `SeparateSecondaryViewController` metody `UISplitViewControllerDelegate`. `UINavigationController` implementuje te metody automatycznie wypychania i pop kontroler widoku dodatkowej.

### <a name="showing-view-controllers"></a>Wyświetlanie widoku kontrolerów

Jest inny zmiany dokonane Apple iOS 8 w taki sposób, że deweloper przedstawia widok kontrolerów. W przeszłości, jeśli aplikacja miała kontrolera widoku typu liść (na przykład tabeli View Controller), a deweloper wykazało inną (na przykład w odpowiedzi na naciśnięcie użytkownika w komórce), docierają aplikacji za pośrednictwem hierarchii kontrolera do Kontroler widoku nawigacji i wywołanie `PushViewController` metody, aby wyświetlić nowy widok.

To przedstawione bardzo ścisłej sprzężenia między kontrolerem nawigacji i został uruchomiony w środowisku. W systemie iOS 8 Apple jest całkowicie niezależna to zapewniając dwóch nowych metod:

-  `ShowViewController` — Dostosowuje się do wyświetlenia na nowy kontroler widoku na podstawie ich środowiska. Na przykład w `UINavigationController` po prostu wypchnięcia nowy widok na stosie. W kontrolerze widoku podziału na nowy kontroler widoku zostanie wyświetlone po lewej stronie jako nowego podstawowego kontrolera widoku. Jeśli kontroler widoku kontenera jest obecny, nowy widok będzie wyświetlany jako modalne kontrolera widoku.
-  `ShowDetailViewController` — Działa w sposób podobny do `ShowViewController`, ale jest zaimplementowana na kontrolerze widoku podziału zastąpić nowego kontrolera widoku przekazywany w widoku szczegółów. Jeśli kontroler widoku podziału jest zwinięty (jak może być widoczny w telefonie iPhone aplikacji), wywołanie zostanie przekierowany do `ShowViewController` — metoda i nowy widok będą wyświetlane jako podstawowy kontroler widoku. Ponownie Jeśli kontroler widoku kontenera jest obecny, nowy widok będzie wyświetlany jako modalne kontrolera widoku.


Te metody pracy przez uruchomienie na kontroler widoku typu liść i podejść wyświetlanie hierarchii, dopóki nie znajdą kontroler widoku kontenera prawo do obsługi wyświetlanie nowy widok.

Deweloperzy mogą implementować `ShowViewController` i `ShowDetailViewController` w własnych niestandardowych kontrolerów widok, aby uzyskać takie same automatycznego funkcji który `UINavigationController` i `UISplitViewController` udostępnia.

### <a name="how-it-works"></a>Jak to działa

W tej sekcji możemy będzie Przyjrzyjmy się faktycznie implementowania tych metod w systemie iOS 8. Pierwszy Przyjrzyjmy się nowe `GetTargetForAction` metody:

 [ ![](unified-storyboards-images/gettargetforaction.png "Nowa metoda GetTargetForAction")](unified-storyboards-images/gettargetforaction.png)

Ta metoda przeprowadzi łańcuch hierarchii, aż do znalezienia kontroler widoku poprawne kontenera. Na przykład:

1.  Jeśli `ShowViewController` metoda jest wywoływana, pierwszego kontrolera widoku w łańcuchu, który implementuje ta metoda jest kontrolerem nawigacji, dlatego jest używana jako element nadrzędny nowy widok.
1.  Jeśli `ShowDetailViewController` zamiast tego wywołano metodę, kontroler widoku podziału jest pierwszy kontroler widoku, który implementuje, więc jest używany jako element nadrzędny.


`GetTargetForAction` Metoda polega na znajdowania kontrolera widoku, który implementuje danego działania, a następnie pytaniem tego kontrolera widoku, jeśli chce otrzymywać tej akcji. Ponieważ ta metoda jest publiczny, deweloperzy mogą tworzyć własne niestandardowe metody, które działają podobnie jak wbudowanych w `ShowViewController` i `ShowDetailViewController` metody.

## <a name="adaptive-presentation"></a>Adaptacyjną prezentacji

W systemie iOS 8, Apple wprowadził prezentacji Popover ( `UIPopoverPresentationController`) adaptacyjną również. Dlatego kontrolera widoku prezentacji Popover automatycznie wyświetli normalnego widoku Popover w klasie rozmiar regularnych, ale zostanie wyświetlona pełnego ekranu w poziomie compact klasy wielkości (takich jak na telefonie iPhone).

Aby uwzględnić zmiany w ramach systemu ujednoliconego scenorysu, został utworzony nowy obiekt kontrolera zarządzania przedstawioną kontrolerów widoku — `UIPresentationController`. Ten kontroler jest tworzona na podstawie czasu, który kontroler widoku są prezentowane, dopóki nie zostanie on odrzucony. Jak zarządzanie klasy, jego jest uznawana za superklasie przez kontroler widoku jako odpowiedzi na zmiany urządzenia, które mają wpływ na kontroler widoku (na przykład orientacja), które są następnie posłużą wstecz do kontrolera widoku kontrolera prezentacji kontrolki.

Gdy dewelopera przedstawia kontrolera widoku przy użyciu `PresentViewController` metody zarządzania procesu prezentacji jest przekazywany do `UIKit`. UIKit obsługuje (między innymi) poprawne kontrolera stylu tworzona, z wyjątkiem tylko, gdy trwa kontrolera widoku ma ustawioną styl `UIModalPresentationCustom`. W tym miejscu, aplikacja może zapewniać jest własnych PresentationController zamiast `UIKit` kontrolera.

### <a name="custom-presentation-styles"></a>Style niestandardowe prezentacji

W przypadku styl niestandardowy prezentacji deweloperzy mają możliwość użycia niestandardowego kontrolera prezentacji. Ten kontroler niestandardowy może służyć do modyfikowania wygląd i zachowanie jest ona pokrewnych do widoku.

<a name="size-classes">

## <a name="working-with-size-classes"></a>Praca z klasami rozmiaru

Adaptacyjną projektu Xamarin zdjęć, która jest zawarta w tym artykule zapewnia pracy z przykładem użycia klasy wielkości i adaptacyjną kontrolerów widoku w aplikacji interfejsu Unified iOS 8.

Podczas gdy aplikacja tworzy jego interfejsie użytkownika całkowicie z kodu, a nie przy użyciu narzędzia Projektant z systemem IOS i zastosować te same techniki tworzenia Unified scenorysu. W dalszej części tego artykułu poniżej opisano sposób użycia klasy wielkości scenorysu Unified i projektanta w aplikacji platformy Xamarin dla systemu iOS.

Teraz Przyjrzyjmy bliższe spojrzenie na jak projektu adaptacyjną zdjęć implementuje niektóre funkcje klasy rozmiar w systemie iOS 8, aby utworzyć adaptacyjną aplikacji.

### <a name="adapting-to-trait-environment-changes"></a>Adaptacja do zmian środowiska cechy

Podczas uruchamiania aplikacji adaptacyjną zdjęć na telefonie iPhone, podczas obracania urządzenia z pionowej na poziomą, kontroler widoku podziału spowoduje wyświetlenie widoku głównego i szczegóły:

 [ ![](unified-storyboards-images/rotation.png "Kontroler widoku podziału zostaną wyświetlone zarówno głównym i wyświetlić szczegóły, jak wspomniano w tym miejscu")](unified-storyboards-images/rotation.png)

Jest to osiągane przez zastępowanie `UpdateConstraintsForTraitCollection` metody kontrolera widoku i dostosowywania ograniczenia na podstawie wartości z `VerticalSizeClass`. Na przykład:

```csharp
public void UpdateConstraintsForTraitCollection (UITraitCollection collection)
{
    var views = NSDictionary.FromObjectsAndKeys (
        new object[] { TopLayoutGuide, ImageView, NameLabel, ConversationsLabel, PhotosLabel },
        new object[] { "topLayoutGuide", "imageView", "nameLabel", "conversationsLabel", "photosLabel" }
    );

    var newConstraints = new List<NSLayoutConstraint> ();
    if (collection.VerticalSizeClass == UIUserInterfaceSizeClass.Compact) {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide][imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.Add (NSLayoutConstraint.Create (ImageView, NSLayoutAttribute.Width, NSLayoutRelation.Equal,
            View, NSLayoutAttribute.Width, 0.5f, 0.0f));
    } else {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]-20-[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));
    }

    if (constraints != null)
        View.RemoveConstraints (constraints.ToArray ());

    constraints = newConstraints;
    View.AddConstraints (constraints.ToArray ());
}
```

### <a name="adding-transition-animations"></a>Dodawanie animacji przejścia

Podczas podziału kontroler widoku w adaptacyjną zdjęć przejdzie do aplikacji z zwinięte do rozwinięte, animacje są dodawane do animacji domyślne przez zastąpienie `WillTransitionToTraitCollection` kontrolera widoku. Na przykład:

```csharp
public override void WillTransitionToTraitCollection (UITraitCollection traitCollection, IUIViewControllerTransitionCoordinator coordinator)
{
    base.WillTransitionToTraitCollection (traitCollection, coordinator);
    coordinator.AnimateAlongsideTransition ((UIViewControllerTransitionCoordinatorContext) => {
        UpdateConstraintsForTraitCollection (traitCollection);
        View.SetNeedsLayout ();
    }, (UIViewControllerTransitionCoordinatorContext) => {
    });
}
```

### <a name="overriding-the-trait-environment"></a>Zastępowanie środowiska cechy

Co zostało przedstawione powyżej, aplikacji adaptacyjną zdjęć wymusza podziału kontroler widoku do wyświetlenia zarówno szczegóły i główny widoków, w przypadku urządzeń iPhone w widoku orientacji poziomej.

To osiągnąć, używając następującego kodu w kontrolerze widoku:

```csharp
private UITraitCollection forcedTraitCollection = new UITraitCollection ();
...

public UITraitCollection ForcedTraitCollection {
    get {
        return forcedTraitCollection;
    }

    set {
        if (value != forcedTraitCollection) {
            forcedTraitCollection = value;
            UpdateForcedTraitCollection ();
        }
    }
}
...

public override void ViewWillTransitionToSize (SizeF toSize, IUIViewControllerTransitionCoordinator coordinator)
{
    ForcedTraitCollection = toSize.Width > 320.0f ?
         UITraitCollection.FromHorizontalSizeClass (UIUserInterfaceSizeClass.Regular) :
         new UITraitCollection ();

    base.ViewWillTransitionToSize (toSize, coordinator);
}

public void UpdateForcedTraitCollection ()
{
    SetOverrideTraitCollection (forcedTraitCollection, viewController);
}
```

### <a name="expanding-and-collapsing-the-split-view-controller"></a>Rozwijanie i zwijanie kontroler widoku podziału

Następny Przeanalizujmy implementowania zachowania powiększane i zwijanie podziału kontrolera widoku w programie Xamarin. W `AppDelegate`, podczas tworzenia kontrolera widoku podziału swojego delegata jest przypisany do obsługi tych zmian:

```csharp
public class SplitViewControllerDelegate : UISplitViewControllerDelegate
{
    public override bool CollapseSecondViewController (UISplitViewController splitViewController,
        UIViewController secondaryViewController, UIViewController primaryViewController)
    {
        AAPLPhoto photo = ((CustomViewController)secondaryViewController).Aapl_containedPhoto (null);
        if (photo == null) {
            return true;
        }

        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            var viewControllers = new List<UIViewController> ();
            foreach (var controller in ((UINavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containsPhoto");

                if ((bool)method.Invoke (controller, new object[] { null })) {
                    viewControllers.Add (controller);
                }
            }

            ((UINavigationController)primaryViewController).ViewControllers = viewControllers.ToArray<UIViewController> ();
        }

        return false;
    }

    public override UIViewController SeparateSecondaryViewController (UISplitViewController splitViewController,
        UIViewController primaryViewController)
    {
        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            foreach (var controller in ((CustomNavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containedPhoto");

                if (method.Invoke (controller, new object[] { null }) != null) {
                    return null;
                }
            }
        }

        return new AAPLEmptyViewController ();
    }
}
```

`SeparateSecondaryViewController` Testy metody czy zdjęcie jest wyświetlany i podejmuje działania na podstawie tego stanu. Jeśli nie zdjęcie jest pokazywane go zwija pomocniczy kontroler widoku pojawia się kontroler widoku głównego.

`CollapseSecondViewController` Metoda jest używana podczas rozszerzania kontroler widoku podziału można znaleźć żadnych zdjęć znajdować się na stosie, więc zwijany do tego widoku.

### <a name="moving-between-view-controllers"></a>Przenoszenie między kontrolerami widoku

Następnie Spójrzmy na jak aplikacji adaptacyjną zdjęć są przenoszone między kontrolerami widoku. W `AAPLConversationViewController` klasy, gdy użytkownik wybierze komórkę z tabeli, `ShowDetailViewController` metoda jest wywoływana, aby wyświetlić szczegóły:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    var photo = PhotoForIndexPath (indexPath);
    var controller = new AAPLPhotoViewController ();
    controller.Photo = photo;

    int photoNumber = indexPath.Row + 1;
    int photoCount = (int)Conversation.Photos.Count;
    controller.Title = string.Format ("{0} of {1}", photoNumber, photoCount);
    ShowDetailViewController (controller, this);
}
```

### <a name="displaying-disclosure-indicators"></a>Wyświetlanie ujawnianie wskaźników

W aplikacji adaptacyjną fotografii istnieją kilku miejscach, w którym ujawnianie wskaźników są ukryte lub pokazane na podstawie na zmiany w środowisku cechy. Jest to obsługiwane z następującym kodem:

```csharp
public bool Aapl_willShowingViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}

public bool Aapl_willShowingDetailViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingDetailViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}
```

Te są implementowane za pomocą `GetTargetViewControllerForAction` szczegółowo opisana w powyższej metody.

Gdy kontroler widoku tabeli są wyświetlane dane, używa metody zaimplementowana powyżej, aby sprawdzić, czy wypychanej ma mieć miejsce i czy należy wyświetlić lub ukryć wskaźnik ujawnienie odpowiednio:

```csharp
public override void WillDisplay (UITableView tableView, UITableViewCell cell, NSIndexPath indexPath)
{
    bool pushes = ShouldShowConversationViewForIndexPath (indexPath) ?
         Aapl_willShowingViewControllerPushWithSender () :
         Aapl_willShowingDetailViewControllerPushWithSender ();

    cell.Accessory = pushes ? UITableViewCellAccessory.DisclosureIndicator : UITableViewCellAccessory.None;
    var conversation = ConversationForIndexPath (indexPath);
    cell.TextLabel.Text = conversation.Name;
}
```

### <a name="new-showdetailtargetdidchangenotification-type"></a>Nowe `ShowDetailTargetDidChangeNotification` typu

Apple został dodany nowy typ powiadomienia do pracy z klasy wielkości i środowisk cechy w ramach kontrolera widoku podziału `ShowDetailTargetDidChangeNotification`. Powiadomienia są wysyłane przy każdej zmianie docelowych: widok szczegółów dla kontrolera widoku podziału, na przykład w przypadku kontrolera rozwija lub zwija.

Aplikacja adaptacyjną zdjęć używa tego powiadomienia zaktualizowany stan wskaźnika ujawnienie po zmianie kontroler widoku szczegółów:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), AAPLListTableViewControllerCellIdentifier);
    NSNotificationCenter.DefaultCenter.AddObserver (this, new Selector ("showDetailTargetDidChange:"),
        UIViewController.ShowDetailTargetDidChangeNotification, null);
    ClearsSelectionOnViewWillAppear = false;
}
```

Przyjrzyjmy się bliżej aplikacji adaptacyjną zdjęć aby zobaczyć wszystkie metody tej klasy wielkości, kolekcje cechy i adaptacyjną kontrolerów widok może być używany łatwe tworzenie aplikacji programu Unified w platformy Xamarin.iOS.

## <a name="unified-storyboards"></a>Ujednolicone Scenorys

Nowość w systemach iOS 8, Unified Scenorys umożliwiają deweloperom tworzenie, unified plik scenorysu, która może być wyświetlana na urządzeniach zarówno iPhone i iPad, wybierając wiele klas rozmiar. Przy użyciu ujednoliconego Scenorys, deweloper zapisuje mniejsza ilość kodu określonego interfejsu użytkownika i zawiera tylko jeden interfejs projekt do tworzenia i obsługi.

Najważniejsze zalety Unified Scenorys są:

-  Użyj tego samego pliku scenorysu iPhone i iPad.
-  Wdróż wstecz iOS 6 lub iOS 7.
-  Wyświetl podgląd układu dla różnych urządzeń, orientacji i wersji systemu operacyjnego wszystko w ramach systemu Xamarin iOS projektanta.

Ta funkcja jest w pełni obsługiwany w programie Visual Studio dla komputerów Mac

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>Włączenie klas wielkości

Domyślnie wszystkie nowego projektu platformy Xamarin.iOS będzie nam klasy wielkości. Używać klas rozmiar i adaptacyjną Segues wewnątrz scenorysu z projektu starsze, go muszą najpierw zostać przekonwertowane na format Xcode 6 Unified scenorysu z wewnątrz iOS projektanta.

Aby zrobić to otwarcie scenorysu, który ma zostać przekonwertowany w systemie iOS projektanta i wyboru **Użyj klasy wielkości** pola wyboru:

 [ ![](unified-storyboards-images/sizeclass01.png "Pole wyboru Użyj rozmiar klasy")](unified-storyboards-images/sizeclass01.png)

IOS projektanta wyświetli monit o potwierdzenie Deweloper chce przekonwertować format scenorysu używać klas rozmiar:

 [ ![](unified-storyboards-images/sizeclass02.png "Użyj klasy rozmiar alertu")](unified-storyboards-images/sizeclass02.png)

> [!IMPORTANT]
> **Uwaga**: automatycznie Rozmieść również muszą zostać sprawdzone za klasy rozmiar, aby działać poprawnie.

### <a name="generic-device-types"></a>Typy ogólne urządzeń

Po scenorysu został przekonwertowany na używać klas rozmiar, jego będzie można wyświetlane ponownie w powierzchnię projektu i **widoku jako** urządzenie będzie ogólne:

 [ ![](unified-storyboards-images/sizeclass03.png "Wyświetl jako typ urządzenia")](unified-storyboards-images/sizeclass03.png)

Po wybraniu typu urządzenia, wszystkich kontrolerów widoku będzie zmieniany kwadrat 600 x 600. Ta kwadratowe reprezentuje rozmiary szerokości i wysokości wszystkie. IOS Projektant jest w tym trybie, wszelkie zmiany będą stosowane do wszystkich klas wielkości.

Deweloper ma także możliwość wyświetlania powierzchnię projektu jako iPhone:

 [ ![](unified-storyboards-images/sizeclass04.png "Wyświetlanie powierzchnię projektu jako iPhone")](unified-storyboards-images/sizeclass04.png)

Lub wyświetlanie go jako iPad:

 [ ![](unified-storyboards-images/sizeclass05.png "Wyświetlanie powierzchnię projektu jako iPad")](unified-storyboards-images/sizeclass05.png)

### <a name="select-a-size-class"></a>Wybierz klasę, rozmiar

Przycisk selektora klasy rozmiar jest lewy górny róg powierzchni projektowej (obok wyświetlanie jako menu rozwijanego). Umożliwia deweloperowi wybierz rozmiar klas, które są aktualnie edytowany:

 [ ![](unified-storyboards-images/sizeclass06.png "Wybierz klasę, rozmiar")](unified-storyboards-images/sizeclass06.png)

Selektor przedstawia wyboru klasy wielkości jako siatka 3 x 3. Każdy kwadratów w siatce reprezentuje kombinację klasy szerokość i wysokość klasę. Środkowy kwadrat wybiera klasę dowolny rozmiar wysokość Width/Any (który jest domyślnym widokiem dla scenorysu Unified). Gdy ta jest zaznaczona, deweloper edytuje układ domyślny, który jest dziedziczona przez wszystkie inne konfiguracje.

Pole w lewym górnym rogu siatki reprezentuje klasę rozmiar Wysokość szerokość/CD Compact:

 [ ![](unified-storyboards-images/sizeclass07.png "Klasa rozmiar wysokość Compact szerokość/CD")](unified-storyboards-images/sizeclass07.png)

W tym trybie odpowiada iPhone w orientacji poziomej. Kwadrat w prawym dolnym rogu siatki reprezentuje regularne szerokość/Regular wysokości rozmiaru klasy, która reprezentuje iPad:

 [ ![](unified-storyboards-images/sizeclass08.png "Klasa rozmiar wysokość regularne szerokość/regularne")](unified-storyboards-images/sizeclass08.png)

Aby edytować układu dla telefonu iPhone w orientacji pionowej, zaznacz pole w dolnym rogu po lewej stronie. To reprezentuje klasę rozmiar wysokość Compact szerokość/regularne:

 [ ![](unified-storyboards-images/sizeclass09.png "Klasa rozmiar wysokość Compact szerokość/regularne")](unified-storyboards-images/sizeclass09.png)

Kliknij pole, aby go zaznaczyć i powierzchni projektowej zmieni rozmiar kontrolerów widoku odpowiadające wyboru nowego:

 [ ![](unified-storyboards-images/sizeclass10.png "Powierzchni projektowej zmieni rozmiar zgodnie z wyborem nowe, jak pokazano na kontrolerach widoku")](unified-storyboards-images/sizeclass10.png)

Zobacz sekcję klasy rozmiar tego artykułu, aby uzyskać więcej informacji na rozmiar klasy i ich wpływ na układ dla urządzenia iPhone i Ipad.

### <a name="adaptive-segue-types"></a>Adaptacyjną Segue typów

Jeśli projektanta został użyty scenorys przed, a następnie będzie można zapoznać się z istniejących typów segue **Push**, **modalne** i **Popover**. Po włączeniu klas rozmiaru na plik scenorysu Unified adaptacyjną Segue następujące (odpowiadające nowy widok kontrolera interfejsu API opisanych wyżej) stają się dostępne: **Pokaż** i **Pokaż szczegóły** .

> [!IMPORTANT]
> **Uwaga**: gdy klasy rozmiar są włączone, wszelkie istniejące segues zostanie przekonwertowane na nowe typy.

Zająć przykład iOS 8 aplikacji, która używa scenorysu Unified dla kontrolera widoku podziału, który zawiera menu proste gier nawigacji w widoku głównego. Gdy użytkownik kliknie przycisk menu, wybranego elementu View Controller powinny być wyświetlane w sekcji szczegółów kontrolera widoku podziału uruchomionej na urządzeniu iPad. Na telefonie iPhone elementu View Controller powinna zostać przesunięta na stosie nawigacji.

Aby osiągnąć, w systemie iOS projektanta kontroli — kliknij przycisk i przeciągnij linię, aby kontroler widoku, który będzie wyświetlany. Po zwolnieniu przycisku myszy, wybierz `Show Detail` z menu podręcznego Segue typu:

 [ ![](unified-storyboards-images/segue01.png "Pokaż szczegóły, wybierz z menu podręcznego Segue typu")](unified-storyboards-images/segue01.png)

Nowe segue zostanie utworzona między przycisku i kontroler widoku. Teraz uruchom aplikację w symulatorze telefonów iPhone i zostanie wyświetlone Menu główne:

 [ ![](unified-storyboards-images/segue02.png "Menu główne")](unified-storyboards-images/segue02.png)

Polecenie **wybierz gry** przycisk i elementu View Controller wypychanie zostanie wykonane na stosie nawigacji:

 [ ![](unified-storyboards-images/segue03.png "Elementy kontrolera widoku wypychanie zostanie wykonane na stosie nawigacji wyświetlane")](unified-storyboards-images/segue03.png)

Zatrzymaj iPhone symulator i uruchomić aplikację w symulatorze tabletów iPad. Przełącz się do orientacji poziomej i głównym ponownie zostanie wyświetlone menu:

 [ ![](unified-storyboards-images/segue04.png "Wyświetlić menu głównego")](unified-storyboards-images/segue04.png)

Ponownie, kliknij **wybierz gry** przycisk i elementu w widoku kontrolera są widoczne w sekcji szczegółów podziału kontroler widoku:

 [ ![](unified-storyboards-images/segue05.png "Elementy wyświetlane w sekcji szczegółów kontrolera widoku podziału kontrolera widoku")](unified-storyboards-images/segue05.png)

### <a name="excluding-an-element-from-a-size-class"></a>Wykluczanie Element klasy wielkości

Brak godziny, kiedy dany element (na przykład widoku sterowania lub ograniczenie) nie jest wymagana wewnątrz określonej klasy rozmiar. Aby wykluczyć elementu z klasy rozmiar, wybierz odpowiedni element do wykluczenia w **powierzchni projektowej**. Przewiń do dołu **Explorer właściwości** i kliknij przycisk **koło zębate** menu rozwijanym. Wybierz kombinację **szerokość** i **wysokość** do wykluczenia z elementu:

[ ![](unified-storyboards-images/exclude-a.png "Wybierz kombinację szerokości i wysokości.")](unified-storyboards-images/exclude-a.png)

Nowy *przypadku wykluczenia* zostanie dodany do elementu w dolnej części **Explorer właściwości**. Następnie usuń zaznaczenie pola wyboru **zainstalowana** wyboru dla danej klasy rozmiar:

[ ![](unified-storyboards-images/exclude-b.png "Usuń zaznaczenie pola wyboru zainstalowana")](unified-storyboards-images/exclude-b.png)

Przełącz powierzchnię projektu na szerokość i wysokość elementu została wykluczona z, został on usunięty z danej klasy rozmiar, ale nie cały projekt interfejsu użytkownika:

 [ ![](unified-storyboards-images/exclude02.png "Przełącz powierzchnię projektu na szerokość i wysokość elementu została wykluczona z")](unified-storyboards-images/exclude02.png)

Przełączanie z powrotem do dowolnego wysokość Width/Any klasy wielkości i element została zachowana:

 [ ![](unified-storyboards-images/exclude03.png "Przełączanie z powrotem do dowolnego wysokość Width/Any klasy wielkości")](unified-storyboards-images/exclude03.png)

Po uruchomieniu aplikacji w symulatorze tabletów iPad, zostanie wyświetlony element:

 [ ![](unified-storyboards-images/exclude04.png "Element jak uruchomionej aplikacji w symulatorze tabletów iPad")](unified-storyboards-images/exclude04.png)

A po uruchomieniu aplikacji na telefonie iPhone symulatora brakuje elementu:

 [ ![](unified-storyboards-images/exclude05.png "Brak elementu podczas uruchomionej aplikacji w symulatorze telefonów iPhone")](unified-storyboards-images/exclude05.png)

Aby usunąć element przypadku wykluczenia, po prostu wybierz element w **powierzchni projektowej**, przewiń do dołu **Explorer właściwości** i kliknij przycisk  **-** przycisk obok wielkości liter, aby usunąć.

Implementacja interfejsu Unified Scenorys obejrzeć w `UnifiedStoryboard` Przykładowa aplikacja dla systemu iOS 8 Xamarin dołączony do tego dokumentu.

## <a name="dynamic-launch-screens"></a>Ekrany dynamicznego uruchamiania

Uruchom plik ekranu zostanie wyświetlony jako ekran powitalny aplikacji systemu iOS jest uruchamiany w celu otrzymania opinii dla użytkownika, która aplikacja jest rzeczywiście uruchamianie w górę. Przed iOS 8, deweloper musi zawierać wiele `Default.png` obrazu zasobów dla każdego urządzenia typu i orientacji ekranu rozdzielczość aplikacja będzie działać na. Na przykład `Default@2x.png`, `Default-Landscape@2x~ipad.png`, `Default-Portrait@2x~ipad.png`itp.

Factoring w nowych iPhone 6 i iPhone 6 Plus urządzenia (i nadchodzących Apple Watch) ze wszystkimi istniejących iPhone i urządzenia iPad, ta pozycja reprezentuje dużą tablicę różnych rozmiarach, orientacji i rozdzielczości `Default.png` zasobów obrazu ekranu uruchamiania, które muszą można tworzyć i obsługiwać. Ponadto tych plików może być dość duży i będzie "wybrzuszanie" pakiet aplikacji dostarczanych, zwiększenie ilości czasu wymaganego do pobrania aplikacji w sklepie iTunes App Store (prawdopodobnie wówczas zachowanie uniemożliwia ma być dostarczona przez sieć komórkową) i zwiększenie ilości miejsca wymaganego na urządzeniu użytkownika końcowego.

Nowe w systemach iOS 8, deweloper można utworzyć jeden atomic `.xib` pliku w programie Xcode, korzystającej z klasy rozmiaru i układu automatycznego tworzenia *dynamiczne ekranu uruchamiania* który będzie działać dla każdego urządzenia, rozdzielczość i orientacji. To nie tylko zmniejsza ilość pracy wymaganej od dewelopera można tworzyć i obsługiwać wszystkie zasoby wymagane obrazu, ale znacznie zmniejsza rozmiar zainstalowanego pakietu aplikacji.


Dynamiczne ekrany uruchamianie mają następujące ograniczenia i zagadnienia:

 - Użyj tylko `UIKit` klasy.
 - Użyj widoku z jednym elementem głównym, który jest `UIView` lub `UIViewController` obiektu.
 - Nie tworzy żadnych połączeń do kodu aplikacji (nie dodawaj **akcje** lub **gniazda**).
 - Nie dodawaj `UIWebView` obiektów.
 - Nie używaj żadnych klas niestandardowych.
 - Nie używaj atrybutów środowiska wykonawczego.

Z wytycznymi powyższych pamiętać Przyjrzyjmy się dodanie do istniejącego projektu Xamarin iOS 8 dynamiczne ekranu uruchamiania.

Wykonaj następujące czynności:

1. Otwórz **programu Visual Studio for Mac** i załadować **rozwiązania** można dodać dynamiczne ekranu uruchamiania.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy `MainStoryboard.storyboard` plik i wybierz **Otwórz za pomocą** > **Xcode interfejsu konstruktora**:

    [![](unified-storyboards-images/dls01.png "Otwórz za pomocą konstruktora Xcode — interfejs")](unified-storyboards-images/dls01.png)
3. W programie Xcode, wybierz **pliku** > **nowy** > **pliku...** :

    [![](unified-storyboards-images/dls02.png "Wybierz plik / nowy")](unified-storyboards-images/dls02.png)
4. Wybierz **iOS** > **interfejsu użytkownika** > **uruchomić ekranu** i kliknij przycisk **dalej** przycisk:

    [![](unified-storyboards-images/dls03.png "Wybierz dla systemu iOS / interfejsu użytkownika / uruchomić ekranu")](unified-storyboards-images/dls03.png)
5. Nadaj nazwę plikowi `LaunchScreen.xib` i kliknij przycisk **Utwórz** przycisk:

    [![](unified-storyboards-images/dls04.png "Nazwa pliku LaunchScreen.xib")](unified-storyboards-images/dls04.png)
6. Edytuj projekt ekranu startowego przez dodanie elementów graficznych i umieść je dla danego urządzenia, orientacji i rozmiaru ekranu przy użyciu układu ograniczenia:

    [![](unified-storyboards-images/dls05.png "Edytowanie projekt ekranu startowego")](unified-storyboards-images/dls05.png)
7. Zapisać zmiany w `LaunchScreen.xib`.
8. Wybierz **aplikacji docelowej** i **ogólne** karty:

    [![](unified-storyboards-images/dls06.png "Wybierz element docelowy aplikacji i karta Ogólne")](unified-storyboards-images/dls06.png)
9. Kliknij przycisk **wybierz Info.plist** przycisku Wybierz `Info.plist` dla aplikacji platformy Xamarin i kliknij przycisk **wybierz** przycisk:

    [![](unified-storyboards-images/dls07.png "Wybierz Info.plist dla aplikacji platformy Xamarin")](unified-storyboards-images/dls07.png)
10. W **uruchomić obrazów i ikon aplikacji** sekcji Otwórz **Uruchom plik ekranu** listy rozwijanej i wybierz polecenie `LaunchScreen.xib` utworzone powyżej:

    [![](unified-storyboards-images/dls08.png "Wybierz LaunchScreen.xib")](unified-storyboards-images/dls08.png)
11. Zapisz zmiany w pliku i powrócić do programu Visual Studio dla komputerów Mac.
12. Poczekaj, aż programu Visual Studio for Mac zakończyć synchronizację zmian z Xcode.
13. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **zasobów** i wybierz polecenie **Dodaj** > **Dodawanie plików...** :

    [![](unified-storyboards-images/dls09.png "Wybierz opcję Dodaj / dodawania plików...")](unified-storyboards-images/dls09.png)
14. Wybierz `LaunchScreen.xib` utworzone powyżej i kliknij przycisk **Otwórz** przycisk:

    [![](unified-storyboards-images/dls10.png "Wybierz plik LaunchScreen.xib")](unified-storyboards-images/dls10.png)
15. Tworzenie aplikacji.

### <a name="testing-the-dynamic-launch-screen"></a>Testowanie ekran startowy dynamiczne

W programie Visual Studio dla komputerów Mac wybierz symulatora telefonu iPhone 4 siatkówki i uruchomić aplikację. Dynamiczne uruchomić ekranu zostanie wyświetlony w poprawnym formacie i orientacji:

[![](unified-storyboards-images/dls11.png "Dynamiczne ekranu uruchamiania wyświetlany w orientacji pionowej")](unified-storyboards-images/dls11.png)

Zatrzymaj aplikację w programie Visual Studio dla komputerów Mac, a następnie wybierz urządzenie z systemem iOS 8 iPad. Uruchom aplikację i ekran startowy zostanie poprawnie sformatowany dla tego urządzenia i orientację:

[![](unified-storyboards-images/dls12.png "Dynamiczne ekranu uruchamiania wyświetlany w orientacji poziomej")](unified-storyboards-images/dls12.png)

Wróć do programu Visual Studio dla komputerów Mac i zatrzymać działanie aplikacji.

### <a name="working-with-ios-7"></a>Praca z systemem iOS 7

Aby zachować zgodność z poprzednimi wersjami z systemem iOS 7, po prostu dołączyć zwykle `Default.png` obrazu zasobów, jak zwykle w aplikacji systemu iOS 8. iOS powrócić do poprzedniej zachowanie, a następnie wykorzystać te pliki jako ekran startowy podczas uruchamiania na urządzeniu z systemem iOS 7.

Implementacja dynamiczne ekranu uruchamiania w Xamarin obejrzeć w [dynamiczne ekrany uruchamianie](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/) Przykładowa aplikacja dla systemu iOS 8 dołączony do tego dokumentu.

## <a name="summary"></a>Podsumowanie

W tym artykule miał krótki przegląd klasy wielkości i ich wpływ na układ na urządzeniach iPhone i iPad. Omówione go, jak działają cech, cechy środowiska i cechy kolekcje z klasami rozmiar do utworzenia Unified interfejsów. Zajęło krótki opis adaptacyjną kontrolerów widoku oraz ich współdziałaniu z klasami rozmiar wewnątrz Unified interfejsów. Go przeglądał Implementowanie rozmiar klasy i interfejsy Unified całkowicie z kodu C# w aplikacji systemu iOS 8 Xamarin.

Na koniec w tym artykule omówione podstawy tworzenia Unified Scenorys z Xamarin iOS projektanta, który będzie działać na urządzeniach z systemem iOS i tworzenia pojedynczej dynamiczne ekranu uruchamiania, który będzie wyświetlany jako ekranu startowego na każdym urządzeniu z systemem iOS 8.

## <a name="related-links"></a>Linki pokrewne

- [Adaptacyjną zdjęć (przykład)](https://developer.xamarin.com/samples/monotouch/ios8/AdaptivePhotos/)
- [Przykładowe StoryboardIntro](https://developer.xamarin.com/samples/monotouch/StoryboardIntro/)
- [Ekrany dynamicznego uruchamiania (przykład)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Wprowadzenie do systemu iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Dynamiczne układów w iOS8 - rozwijać 2014 r. (klip wideo)](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
