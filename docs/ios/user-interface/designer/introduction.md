---
title: podstawowe informacje o projektancie systemu iOS
description: "W tym przewodniku przedstawiono projektanta Xamarin dla systemu iOS. Go przedstawiono sposób użycia iOS projektanta wizualnego układ formantów, jak dostęp do tych kontrolek w kodzie oraz edytować właściwości."
ms.topic: article
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/31/2018
ms.openlocfilehash: 3046d779239076098a8b2fb74fc87e2f211074e9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="ios-designer-basics"></a>podstawowe informacje o projektancie systemu iOS

_W tym przewodniku przedstawiono projektanta Xamarin dla systemu iOS. Go przedstawiono sposób użycia iOS projektanta wizualnego układ formantów, jak dostęp do tych kontrolek w kodzie oraz edytować właściwości._

Projektant Xamarin dla systemu iOS jest podobne do konstruktora interfejsu w środowisku Xcode Projektant wizualny interfejs i Projektant systemu Android. Wiele funkcji między innymi bezproblemową integrację z programu Visual Studio for Mac i Visual Studio 2015 i 2017, przeciągnij i upuść interfejsu do konfigurowania programów obsługi zdarzeń i możliwość renderowania kontrolek niestandardowych.

## <a name="requirements"></a>Wymagania

IOS projektanta jest dostępne w programie Visual Studio dla komputerów Mac i Visual Studio 2015 i 2017 w systemie Windows. W programie Visual Studio 2015 lub 2017 iOS Projektant wymaga połączenia z odpowiednio skonfigurowanego hosta kompilacji Mac, chociaż Xcode nie muszą być uruchomiona.

W tym przewodniku założono znajomość zawartość objęte [wprowadzenie prowadzi](~/ios/get-started/index.md).

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>Jak działa system iOS projektanta

W tej sekcji opisano, jak projektant z systemem iOS ułatwia tworzenie interfejsu użytkownika i połączenia jej kod.

Projektant z systemem iOS umożliwia deweloperom wizualne projektowanie interfejsu użytkownika aplikacji. Zgodnie z opisem w [wprowadzenie do scenorysu](~/ios/user-interface/storyboards/index.md) przewodniku scenorysu Opisuje ekrany (Widok kontrolery) wchodzące w skład aplikacji elementów interfejsu (widoki) dotyczącymi te kontrolery widoku i ogólny przepływ nawigacji aplikacji . 

Widok Kontroler ma dwie części: wizualną reprezentację w systemie iOS projektanta i skojarzonych klas C#:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Kontrolera widoku w systemie iOS projektanta](introduction-images/1-storyboardwithviewcontroller-vsmac.png "kontrolera widoku w Projektancie systemu iOS")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png)

[![Kod dla kontrolera widoku](introduction-images/2-viewcontrollercode-vsmac.png "kod kontrolera widoku")](introduction-images/2-viewcontrollercode-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kontrolera widoku w systemie iOS projektanta](introduction-images/1-storyboardwithviewcontroller-vs.png "kontrolera widoku w Projektancie systemu iOS")](introduction-images/1-storyboardwithviewcontroller-vs-large.png)

[![Kod dla kontrolera widoku](introduction-images/2-viewcontrollercode-vs.png "kod kontrolera widoku")](introduction-images/2-viewcontrollercode-vs-large.png)

-----

W stanie domyślnym kontrolera widoku nie udostępnia żadnych funkcji; powinno zostać zapełnione z formantami. Formanty są umieszczane w widoku kontrolera widoku prostokątny obszar, który zawiera wszystkie zawartość ekranu. Większość kontrolerów widok zawiera formanty standardowe, takie jak przyciski, etykiety i pola tekstowego, jak pokazano na poniższym zrzucie ekranu, przedstawiający widok kontroler zawierające przycisk: 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Widok kontroler zawierające przycisk](introduction-images/3-viewcontrollerwithbutton-vsmac.png "kontrolera widoku zawierające przycisk")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Widok kontroler zawierające przycisk](introduction-images/3-viewcontrollerwithbutton-vs.png "kontrolera widoku zawierające przycisk")](introduction-images/3-viewcontrollerwithbutton-vs-large.png)

-----

Niektóre formanty, takie jak etykiety, zawierających tekst statyczny, mogą być dodawane do kontrolera widoku lub pozostanie bez zmian. Jednak bardziej często niż nie formantów muszą być dostosowane programowo. Na przykład przycisk dodanych wcześniej należy zrobić coś wybrany program obsługi zdarzeń musi zostać dodany w kodzie.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby uzyskać dostęp i manipulowania przycisku w kodzie, musi mieć unikatowy identyfikator. Podaj unikatowy identyfikator, wybierając przycisk, otwierając **konsoli właściwości**i ustawienie jej **nazwa** pola do wartości, takich jak "SubmitButton":

[![Nazwa przycisku Ustawienia w konsoli właściwości](introduction-images/4-settingbuttonname-vsmac.png "nazwa przycisku Ustawienia w konsoli właściwości")](introduction-images/4-settingbuttonname-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby uzyskać dostęp i manipulowania przycisku w kodzie, musi mieć unikatowy identyfikator. Podaj unikatowy identyfikator, wybierając przycisk, otwierając **okna właściwości**i ustawienie jej **nazwa** pola do wartości, takich jak "SubmitButton":

[![Nazwa przycisku Ustawienia w oknie właściwości](introduction-images/4-settingbuttonname-vs.png "nazwa przycisku Ustawienia w oknie właściwości")](introduction-images/4-settingbuttonname-vs-large.png)

-----

Teraz, gdy przycisk ma nazwę, jest dostępny w kodzie. Ale jak to działa?

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W **konsoli rozwiązania**, nawigacyjnego do **ViewController.cs** i klikając wskaźnik ujawnienie wykaże, że kontroler widoku `ViewController` obejmuje definicję klasy, dwa pliki, z których każdy zawiera [częściowej klasy](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) definicji:

[![Dwa pliki wchodzących w skład klasy ViewController: ViewController.cs i ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "dwa pliki wchodzących w skład klasy ViewController: ViewController.cs i ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W **Eksploratora rozwiązań**, nawigacyjnego do **ViewController.cs** i klikając wskaźnik ujawnienie wykaże, że kontroler widoku `ViewController` definicji klasy obejmuje dwa pliki, każdy z zawierającą [częściowej klasy](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) definicji:

[![Dwa pliki wchodzących w skład klasy ViewController: ViewController.cs i ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "dwa pliki wchodzących w skład klasy ViewController: ViewController.cs i ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png)

-----

- **ViewController.cs** powinny zostać wypełnione niestandardowego kodu powiązany z `ViewController` klasy. W tym pliku `ViewController` klasy mogą odpowiedzieć iOS różnych metod cyklu życia kontrolera widoku, dostosowywanie interfejsu użytkownika i odpowiadanie na dane wejściowe, takie jak przycisk naciska użytkownika.

- **ViewController.designer.cs** plik został wygenerowany, utworzone przez projektanta do mapowania interfejsu wizualnie skonstruowany kodu dla systemu iOS. Ponieważ zmiany w tym pliku zostaną zastąpione, nie powinien być modyfikowany. Deklaracje właściwości w tym pliku umożliwiają kodu w `ViewController` klasy na dostęp, przez **nazwa**, określa zestaw zapasowych w systemie iOS projektanta. Otwieranie **ViewController.designer.cs** ujawnia następujący kod:

```csharp
namespace Designer
{
    [Register ("ViewController")]
    partial class ViewController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        UIKit.UIButton SubmitButton { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (SubmitButton != null) {
                SubmitButton.Dispose ();
                SubmitButton = null;
            }
        }
    }
}
```

`SubmitButton` Deklaracja właściwości łączy całą `ViewController` klasy — nie tylko **ViewController.designer.cs** pliku — na przycisk zdefiniowane w scenorysu. Ponieważ **ViewController.cs** definiuje część `ViewController` klasa, ma dostęp do `SubmitButton`.

Poniższy zrzut ekranu przedstawia IntelliSense teraz rozpoznaje `SubmitButton` odwołania w **ViewController.cs**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Rozpoznawanie odwołania SubmitButton IntelliSense](introduction-images/6-submitbuttonintellisense-vsmac.png "Rozpoznawanie odwołania SubmitButton IntelliSense")](introduction-images/6-submitbuttonintellisense-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Rozpoznawanie odwołania SubmitButton IntelliSense](introduction-images/6-submitbuttonintellisense-vs.png "Rozpoznawanie odwołania SubmitButton IntelliSense")](introduction-images/6-submitbuttonintellisense-vs-large.png)

-----

W tej sekcji wykazała, jak utworzyć przycisk w systemie iOS projektanta i dostęp do tego przycisku w kodzie.

W pozostałej części tego dokumentu zawiera przegląd dalsze iOS projektanta.

## <a name="ios-designer-basics"></a>podstawowe informacje o projektancie systemu iOS

W tej sekcji przedstawiono części iOS projektanta i zawiera przegląd funkcji.

### <a name="launching-the-ios-designer"></a>Uruchamianie projektanta dla systemu iOS

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Projekty platformy Xamarin.iOS utworzone w programie Visual Studio dla komputerów Mac obejmują scenorysu. Aby wyświetlić zawartość scenorysu, dwukrotnie kliknij plik .storyboard **konsoli rozwiązania**:

[![Otwórz scenorysu w systemie iOS projektanta](introduction-images/7-storyboardopen-vsmac.png "scenorysu Otwórz w Projektancie systemu iOS")](introduction-images/7-storyboardopen-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Większości projektów platformy Xamarin.iOS utworzone za pomocą programu Visual Studio 2015 lub 2017 obejmują scenorysu. Aby wyświetlić zawartość scenorysu, dwukrotnie kliknij plik .storyboard **Eksploratora rozwiązań**:

[![Otwórz scenorysu w systemie iOS projektanta](introduction-images/7-storyboardopen-vs.png "scenorysu Otwórz w Projektancie systemu iOS")](introduction-images/7-storyboardopen-vs-large.png)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>Funkcje projektanta systemu iOS

IOS projektanta ma sześć głównej sekcje:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Sekcje iOS projektanta](introduction-images/8-sixpartsofiosdesigner-vsmac.png "sekcje projektanta dla systemu iOS")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png)

1. **Projektowanie powierzchni** — Projektant iOS podstawowym obszarem roboczym. Wyświetlany w obszarze dokumentu, umożliwia wizualnego tworzenia interfejsów użytkownika.
2. **Ograniczenia narzędzi** — umożliwia przełączanie ramek edycji trybu i ograniczenia trybu edycji, dwa różne sposoby położenie elementów w interfejsie użytkownika.
3. **Przybornik** — zawiera listę kontrolerów, obiekty, kontrolek, widoki danych, aparaty rozpoznawania gestów, windows i pasków, który może być przeciągany na powierzchnię projektu i dodaje do interfejsu użytkownika.
4. **Konsola właściwości** — Wyświetla właściwości zaznaczonego formantu, takich jak tożsamość, style wizualne, dostępności, układu i zachowania.
5. **Konspekt dokumentu** — przedstawia drzewa formantów, które tworzą układ interfejsu edytowany. Kliknięcie elementu w drzewie zaznacza go w systemie iOS projektanta i pokazuje jego właściwości w **konsoli właściwości**. Jest to przydatne do wybierania określonego formantu interfejsu użytkownika głęboko zagnieżdżone.
6. **Dolna narzędzi** — zawiera opcje umożliwiające zmianę sposobu iOS projektanta wyświetlania pliku .storyboard lub .xib, w tym urządzeniu, orientację i powiększenia.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Sekcje iOS projektanta](introduction-images/8-sixpartsofiosdesigner-vs.png "sekcje projektanta dla systemu iOS")](introduction-images/8-sixpartsofiosdesigner-vs-large.png)

1. **Projektowanie powierzchni** — Projektant iOS podstawowym obszarem roboczym. Wyświetlany w obszarze dokumentu, umożliwia wizualnego tworzenia interfejsów użytkownika.
2. **Ograniczenia narzędzi** — umożliwia przełączanie ramek edycji trybu i ograniczenia trybu edycji, dwa różne sposoby położenie elementów w interfejsie użytkownika.
3. **Przybornik** — zawiera listę kontrolerów, obiekty, kontrolek, widoki danych, aparaty rozpoznawania gestów, windows i pasków, który może być przeciągany na powierzchnię projektu i dodaje do interfejsu użytkownika.
4. **Okno właściwości** — Wyświetla właściwości zaznaczonego formantu, takich jak tożsamość, style wizualne, dostępności, układu i zachowania.
5. **Konspekt dokumentu** — przedstawia drzewa formantów, które tworzą układ interfejsu edytowany. Kliknięcie elementu w drzewie zaznacza go w systemie iOS projektanta i pokazuje jego właściwości w **okna właściwości**. Jest to przydatne do wybierania określonego formantu interfejsu użytkownika głęboko zagnieżdżone.
6. **Dolna narzędzi** — zawiera opcje umożliwiające zmianę sposobu iOS projektanta wyświetlania pliku .storyboard lub .xib, w tym urządzeniu, orientację i powiększenia.

-----

### <a name="design-workflow"></a>Projekt przepływu pracy

#### <a name="adding-a-control-to-the-interface"></a>Dodawanie formantu do interfejsu

Aby dodać kontrolkę do interfejsu, przeciągnij je z **przybornika** i upuść ją na powierzchni projektu. Podczas dodawania lub pozycjonowanie formantu, wytycznych i pionowe Wyróżnij pozycji najczęściej używanych układ pionowy Centrum, wyśrodkowanego i marginesów:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![Na powierzchni projektowej wytyczne zaznacz pozycje najczęściej używanych układu](introduction-images/9-layoutguides-vsmac.png "na powierzchni projektowej wytyczne zaznacz pozycje najczęściej używanych układu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Na powierzchni projektowej wytyczne zaznacz pozycje najczęściej używanych układu](introduction-images/9-layoutguides-vs.png "na powierzchni projektowej wytyczne zaznacz pozycje najczęściej używanych układu")

-----

Niebieska linia przerywana w powyższym przykładzie przedstawiono wskazówki visual wyrównanie poziome Centrum ułatwiające położenie przycisku.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="context-menu-commands"></a>Polecenia menu kontekstowe

Menu kontekstowe jest dostępne zarówno na powierzchni projektu i **konspekt dokumentu**. To menu zawiera polecenia do zaznaczonego formantu i jego element nadrzędny, co jest przydatne podczas pracy z widoków w zagnieżdżonych hierarchii:

[![Menu kontekstowe na powierzchni projektowej](introduction-images/10-contextmenudesignsurface-vsmac.png "menu kontekstowego na powierzchni projektu")](introduction-images/10-contextmenudesignsurface-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

### <a name="constraints-toolbar"></a>Ograniczenia paska narzędzi

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
[![Na pasku narzędzi ograniczenia](introduction-images/11-constraintstoolbar-vsmac.png "narzędzi ograniczenia")](introduction-images/11-constraintstoolbar-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Na pasku narzędzi ograniczenia](introduction-images/11-constraintstoolbar-vs.png "narzędzi ograniczenia")](introduction-images/11-constraintstoolbar-vs-large.png)

-----

Na pasku narzędzi ograniczenia została zaktualizowana i obecnie składa się z dwóch formantów: ramki, w trybie edycji / ograniczenia edycji Przełącz tryb i ograniczenia aktualizacji / aktualizacji przycisk ramki.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Tryb edycji ramki / Przełącz tryb edycji ograniczenia

W poprzednich wersjach systemu IOS projektanta klikając już wybrany widok na powierzchni projektowej przełączane między ramki edycji trybu i ograniczenia trybu edycji. Teraz formantu Przełącz na pasku narzędzi ograniczenia przełącza między tymi tryby edycji.

- Tryb edycji ramki:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Przycisk trybu edycji ramki](introduction-images/12a-frameeditingmode-vsmac.png "ramki przycisk Tryb edycji")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Przycisk trybu edycji ramki](introduction-images/12a-frameeditingmode-vs.png "ramki przycisk Tryb edycji")

-----

- Tryb edycji ograniczenia:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Przycisk trybu edycji ograniczenia](introduction-images/12b-constrainteditingmode-vsmac.png "przycisk trybu edycji ograniczenia")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Przycisk trybu edycji ograniczenia](introduction-images/12b-constrainteditingmode-vs.png "przycisk trybu edycji ograniczenia")

-----

#### <a name="update-constraints--update-frames-button"></a>Zaktualizuj ograniczenia / zaktualizować przycisk ramki

Ograniczenia aktualizacji / aktualizacji ramki znajduje się przycisk z prawej strony ramki, w trybie edycji / Przełącz tryb edycji ograniczenia.

- W trybie edycji ramce kliknięcie tego przycisku dopasowuje ramek wszystkie wybrane elementy odpowiadające ich ograniczenia.
- W trybie edycji przez ograniczenie kliknięcie tego przycisku dopasowuje ograniczenia wszystkie wybrane elementy odpowiadające ich ramki.

### <a name="bottom-toolbar"></a>Dolny pasek narzędzi

Dolnym pasku narzędzi umożliwia wybierz urządzenie, orientację i powiększenia używany do wyświetlania plików storyboard lub .xib w Projektancie systemu iOS:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dolnym pasku narzędzi, używany do wybierania urządzenia i orientację powierzchni projektowej](introduction-images/13-bottomtoolbar-vsmac.png "dolnym pasku narzędzi, używany do wybierania urządzenia i orientację powierzchnię projektu")](introduction-images/13-bottomtoolbar-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dolnym pasku narzędzi, używany do wybierania urządzenia i orientację powierzchni projektowej](introduction-images/13-bottomtoolbar-vs.png "dolnym pasku narzędzi, używany do wybierania urządzenia i orientację powierzchnię projektu")](introduction-images/13-bottomtoolbar-vs-large.png)

-----

#### <a name="device-and-orientation"></a>Urządzenia i orientacji

Po rozwinięciu dolnym pasku narzędzi Wyświetla wszystkie urządzenia, orientacji i/lub dostosowania mające zastosowanie do bieżącego dokumentu. Kliknięcie zmienia widok wyświetlany na powierzchni projektu. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dolnym pasku narzędzi, rozwinięty, ukazując urządzenia i orientacji](introduction-images/14-bottomtoolbarexpanded-vsmac.png "dolnym pasku narzędzi, rozwinięty, ukazując urządzenia i orientacji")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dolnym pasku narzędzi, rozwinięty, ukazując urządzenia i orientacji](introduction-images/14-bottomtoolbarexpanded-vs.png "dolnym pasku narzędzi, rozwinięty, ukazując urządzenia i orientacji")](introduction-images/14-bottomtoolbarexpanded-vs-large.png)

-----

Należy zwrócić uwagę na to, wybierając urządzenie i orientację zmienia tylko sposób iOS projektanta Wyświetla podgląd projektu. Niezależnie od bieżącego zaznaczenia, nowo dodanych ograniczenia są stosowane we wszystkich urządzeń i orientacji, chyba że **Edytuj cech** przycisk został użyty do określenia, w przeciwnym razie wartość.

Gdy [klasy wielkości](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) są [włączone](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes), **Edytuj cech** przycisk będą wyświetlane w rozszerzonej dolnym pasku narzędzi.  Kliknięcie przycisku **Edytuj cech** przycisk Wyświetla opcje tworzenia odmiany interfejs oparty na klasie rozmiar reprezentowany przez wybrane urządzenie i orientacji. Rozważ następujące przykłady:

- Jeśli **iPhone SE** / **pionowa**, jest zaznaczone, popover zapewnia opcje, aby utworzyć odmiany interfejsu compact szerokości, wysokości regularne rozmiar klasy. 
- Jeśli **iPad Pro 9.7"** / **pozioma** / **pełnoekranowy** jest zaznaczone, popover zapewnia opcje, aby utworzyć odmiana interfejsu regularne szerokość, wysokość zwykły rozmiar klasy.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Trwa służące do różnicowania interfejs przez klasę rozmiar dolnym pasku narzędzi](introduction-images/15-edittraitsbutton-vsmac.png "dolnym pasku narzędzi są służące do różnicowania interfejs według klasy wielkości")](introduction-images/15-edittraitsbutton-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Trwa służące do różnicowania interfejs przez klasę rozmiar dolnym pasku narzędzi](introduction-images/15-edittraitsbutton-vs.png "dolnym pasku narzędzi są służące do różnicowania interfejs według klasy wielkości")](introduction-images/15-edittraitsbutton-vs-large.png)

-----

#### <a name="zoom-controls"></a>Kontrolki powiększania

Powierzchni projektowej obsługuje powiększanie za pośrednictwem kilku formantów:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![Kontrolki powiększania na dolnym pasku narzędzi](introduction-images/16-zoomcontrols-vsmac.png "kontrolki powiększania na dolnym pasku narzędzi")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Kontrolki powiększania na dolnym pasku narzędzi](introduction-images/16-zoomcontrols-vs.png "kontrolki powiększania na dolnym pasku narzędzi")

-----

Formanty są następujące:

1. Dopasuj do
2. Pomniejszanie
3. Powiększanie
4. Rzeczywisty rozmiar (pikseli 1:1)

Formanty Dostosuj powiększenia na powierzchni projektu. Nie ma wpływu na interfejsie użytkownika aplikacji w czasie wykonywania.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="properties-pad"></a>Właściwości konsoli

Użyj **konsoli właściwości** do edycji tożsamości, style wizualne, dostępność i zachowanie formantu. Poniższy zrzut ekranu przedstawia **konsoli właściwości** opcje dla przycisku:

[![Konsola właściwości dla przycisku](introduction-images/17-buttonpropertiespad-vsmac.png "konsoli właściwości dla przycisku")](introduction-images/17-buttonpropertiespad-vsmac-large.png)
#### <a name="properties-pad-sections"></a>Właściwości sekcji konsoli

**Konsoli właściwości** zawiera trzy sekcje:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="properties-window"></a>Okno Właściwości

Użyj **okna właściwości** do edycji tożsamości, style wizualne, dostępność i zachowanie formantu. Poniższy zrzut ekranu przedstawia **okna właściwości** opcje dla przycisku:

[![W oknie właściwości przycisku](introduction-images/17-buttonpropertieswindow-vs.png "okno właściwości dla przycisku")](introduction-images/17-buttonpropertieswindow-vs-large.png)

#### <a name="properties-window-sections"></a>Sekcje okno właściwości

**Okna właściwości** zawiera trzy sekcje:

-----

1.  **Widżet** — głównego właściwości formantu, takie jak nazwa, klasę, właściwości stylu itp. Właściwości do zarządzania zawartością formantu zwykle są umieszczane w tym miejscu.
2.  **Układ** — właściwości, które zachować informacje o położenie i rozmiar formantu, ograniczenia i ramki, w tym są wyświetlane tutaj.
3.  **Zdarzenia** — zdarzenia i procedury obsługi zdarzeń są określone w tym miejscu. Przydatne w przypadku obsługi zdarzeń, takie jak touch, naciśnij pozycję, przeciągnij itp. Zdarzenia można również obsługiwać bezpośrednio w kodzie.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="editing-properties-in-the-properties-pad"></a>Edytowanie właściwości w konsoli właściwości

Oprócz visual edycji na powierzchni projektu iOS projektanta obsługuje edycję właściwości w **konsoli właściwości**. Zmiana właściwości dostępne oparte na wybraną kontrolkę, jak pokazano na zrzutach ekranu poniżej:

[![Przycisk Właściwości](introduction-images/18a-buttonpropertiespad-vsmac.png "przycisk Właściwości")](introduction-images/18a-buttonpropertiespad-vsmac-large.png)

[![Wyświetl właściwości kontrolera](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "wyświetlić właściwości kontrolera")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="editing-properties-in-the-properties-window"></a>Edytowanie właściwości w oknie właściwości

Oprócz visual edycji na powierzchni projektu iOS projektanta obsługuje edycję właściwości w **okna właściwości**. Zmiana właściwości dostępne oparte na wybraną kontrolkę, jak pokazano na zrzutach ekranu poniżej:

[![Przycisk Właściwości](introduction-images/18a-buttonpropertieswindow-vs.png "przycisk Właściwości")](introduction-images/18a-buttonpropertieswindow-vs-large.png)

[![Wyświetl właściwości kontrolera](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "wyświetlić właściwości kontrolera")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png)

-----

> [!IMPORTANT]
> Tożsamość części przedstawiono teraz konsoli właściwości **modułu** pola. Należy wypełnić w tej sekcji, tylko w przypadku współpracy z klasy Swift. Należy wprowadzić nazwę modułu dla klasy Swift, które są należących do przestrzeni nazw.

#### <a name="default-values"></a>Wartości domyślne

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Wiele właściwości w **konsoli właściwości** Pokaż żadna wartość lub wartość domyślną. Jednak kod aplikacji, nadal mogą zmodyfikować te wartości. **Konsoli właściwości** nie są wyświetlane wartości ustawione w kodzie.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wiele właściwości w **okna właściwości** Pokaż żadna wartość lub wartość domyślną. Jednak kod aplikacji, nadal mogą zmodyfikować te wartości. **Okna właściwości** nie są wyświetlane wartości ustawione w kodzie.

-----

#### <a name="event-handlers"></a>Uchwyty zdarzeń

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby określić obsługi zdarzeń niestandardowych dla różnych zdarzeń, użyj **zdarzenia** karcie **konsoli właściwości**. Na przykład na zrzucie ekranu poniżej `HandleClick` metoda obsługuje przycisku **Touch się wewnątrz** zdarzeń:

[![W konsoli właściwości z obsługi zdarzeń dla przycisku](introduction-images/19-buttonpropertiespadevents-vsmac.png "konsoli właściwości z obsługi zdarzeń dla przycisku")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby określić obsługi zdarzeń niestandardowych dla różnych zdarzeń, użyj **zdarzenia** karcie **okna właściwości**. Na przykład na zrzucie ekranu poniżej `HandleClick` metoda obsługuje przycisku **Touch się wewnątrz** zdarzeń:

[![Okno właściwości z obsługi zdarzeń dla przycisku](introduction-images/19-buttonpropertieswindowevents-vs.png "okna właściwości, z obsługi zdarzeń dla przycisku")](introduction-images/19-buttonpropertieswindowevents-vs-large.png)

-----

Gdy program obsługi zdarzeń został określony, metody o tej samej nazwie należy dodać do odpowiedniej klasy kontrolera widoku. W przeciwnym razie `unrecognized selector` wyjątek ma miejsce, gdy przycisk jest dotknięciu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Wystąpił wyjątek nierozpoznany selektora](introduction-images/20-unrecognizedselector-vsmac.png "wyjątek nierozpoznany selektora")](introduction-images/20-unrecognizedselector-vsmac-large.png)

Należy pamiętać, że po program obsługi zdarzeń został określony w **konsoli właściwości**, iOS projektanta zostanie natychmiast otworzyć odpowiedni plik kodu i oferują do wstawienia deklaracji metody. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wystąpił wyjątek nierozpoznany selektora](introduction-images/20-unrecognizedselector-vs.png "wyjątek nierozpoznany selektora")](introduction-images/20-unrecognizedselector-vs-large.png)

-----

Przykład używającej obsługi zdarzeń niestandardowych, można znaleźć w temacie [Hello, iOS przewodnik wprowadzenie](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>Wyświetlanie konspektu

Projektant z systemem iOS można również wyświetlić hierarchii interfejsów formantów jako konspekt. Konspekt jest dostępna po wybraniu **konspekt dokumentu** karcie, jak pokazano poniżej:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Konspekt dokumentu](introduction-images/21-buttonoutlineview-vsmac.png "konspektu dokumentu")](introduction-images/21-buttonoutlineview-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Konspekt dokumentu](introduction-images/21-buttonoutlineview-vs.png "konspektu dokumentu")](introduction-images/21-buttonoutlineview-vs-large.png)

-----

Wybrany formant w widoku konspektu pozostaje zsynchronizowany z wybraną kontrolkę na powierzchni projektu.  Ta funkcja jest przydatna do wybierania elementu z interfejsu głęboko zagnieżdżone hierarchii.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="revert-to-xcode"></a>Przywróć Xcode

Istnieje możliwość Użyj zamiennie iOS projektanta i kompilatora interfejsu Xcode. Aby otworzyć scenorysu lub plik .xib konstruktora interfejsu Xcode, kliknij prawym przyciskiem myszy plik i wybierz **Otwórz za pomocą > konstruktora interfejsu Xcode**, jak pokazano na poniższym zrzucie ekranu:

[![Otwieranie scenorysu w Konstruktorze interfejsu Xcode](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "otwierania scenorysu w Konstruktorze interfejsu Xcode")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png)

Po wprowadzeniu zmian w środowisku Xcode interfejsu konstruktora, Zapisz plik i wróć do programu Visual Studio dla komputerów Mac. Zmiany zsynchronizuje się do projektu platformy Xamarin.iOS.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

## <a name="xib-support"></a>Obsługa .xib

IOS projektanta obsługuje tworzeniem, edytowaniem i zarządzanie plikami .xib. Te są plikami XML tego element reprezentować pojedynczą, niestandardowe widoki, które można dodać do hierarchii widoku aplikacji. Plik .xib zazwyczaj reprezentuje interfejs dla jednego widoku lub ekranu w aplikacji, scenorysu stanowi wiele ekranów i przejścia między nimi.

Istnieje wiele opinie, o których rozwiązania — pliki .xib, scenorys lub kodu — działa najlepiej do tworzenia i obsługi interfejsu użytkownika. W rzeczywistości istnieje doskonałe rozwiązanie, a warto zawsze uwzględnieniu najlepsze narzędzie dla wykonywanego zadania. Inaczej mówiąc, .xib plików są zwykle najbardziej zaawansowanych, gdy jest używany do reprezentowania potrzebne w wielu miejscach w aplikacji, takie jak komórce widoku tabeli niestandardowy widok niestandardowy. 

Więcej dokumentacji dotyczące korzystania z plików .xib można znaleźć w następujących przepisami:

- [Przy użyciu szablonu .xib widoku](https://developer.xamarin.com/recipes/ios/general/templates/using_the_ios_view_xib_template/)
- [Tworzenie TableViewCell niestandardowe, przy użyciu .xib](https://developer.xamarin.com/recipes/ios/content_controls/tables/custom-tableviewcell/)
- [Tworzenie ekranu uruchamiania przy użyciu .xib](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)

Aby uzyskać więcej informacji dotyczących użycia scenorys, zapoznaj się [wprowadzenie do Scenorys](~/ios/user-interface/storyboards/index.md).

To i inne iOS przewodników związanych z projektanta odnoszą się do stosowania scenorys jako standardowa podejście do tworzenia interfejsów użytkownika, ponieważ większość Xamarin.iOS nowych szablonów projektu udostępniają scenorysu domyślnie.

## <a name="summary"></a>Podsumowanie

W tym przewodniku podać wprowadzenie do projektanta, opisujący jej funkcje oraz przedstawiające te narzędzia, które oferuje za projektowanie interfejsów użytkownika doskonałych z systemem iOS.

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do scenorysów](~/ios/user-interface/storyboards/index.md)
- [iOS Designable wskazówki formantów](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Witaj, iOS](~/ios/get-started/hello-ios/index.md)
- [Witaj, iOS Multiscreen (wiele ekranów)](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Omówienie projektanta dla systemu android](~/android/user-interface/android-designer/index.md)
- [Klasy częściowe i metody](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Rozpoczęciem pracy w Projektancie Xamarin dla systemu iOS - rozwijać 2014 r. (klip wideo)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [Tworzenie ekranu uruchamiania (klip wideo) przy użyciu projektanta dla systemu iOS](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
