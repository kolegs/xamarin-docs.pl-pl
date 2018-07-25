---
title: podstawy narzędzia iOS Designer
description: Ten przewodnik stanowi wprowadzenie Projektant platformy Xamarin dla systemu iOS. Pokazuje sposób użycia narzędzia iOS Designer do wizualnego tworzenia układu kontrolek, jak uzyskać dostęp do tych kontrolek w kodzie i sposób edycji właściwości.
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/31/2018
ms.openlocfilehash: 6905eddbc4488b08f9c9e896efe5f980e0e03345
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242371"
---
# <a name="ios-designer-basics"></a>podstawy narzędzia iOS Designer

_Ten przewodnik stanowi wprowadzenie Projektant platformy Xamarin dla systemu iOS. Pokazuje sposób użycia narzędzia iOS Designer do wizualnego tworzenia układu kontrolek, jak uzyskać dostęp do tych kontrolek w kodzie i sposób edycji właściwości._

Projektant platformy Xamarin dla systemu iOS jest Projektant interfejs graficzny, podobnie jak program Xcode Interface Builder i narzędzia Android Designer. Niektóre z wielu funkcji to bezproblemową integrację z usługą Visual Studio dla komputerów Mac i Visual Studio 2015 i 2017, przeciągnij i upuść do edycji, interfejs do konfigurowania programów obsługi zdarzeń i wprowadzenie możliwości renderowania kontrolek niestandardowych.

## <a name="requirements"></a>Wymagania

IOS Designer jest dostępne w programie Visual Studio dla komputerów Mac i Visual Studio 2015 i 2017 na Windows. W programie Visual Studio 2015 lub 2017 narzędzia iOS Designer wymaga połączenia z hostem kompilacji Mac. prawidłowo skonfigurowane, chociaż Xcode nie muszą działać.

W tym przewodniku założono znajomość zawartość objęte [wprowadzenie prowadzi](~/ios/get-started/index.md).

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>Jak działa narzędzia iOS Designer

W tej sekcji opisano, jak narzędzia iOS Designer ułatwia tworzenie interfejsu użytkownika i połączenie jej z kodu.

Narzędzia iOS Designer umożliwia deweloperom wizualnie projektować interfejs użytkownika aplikacji. Jak wskazano w [wprowadzenie do scenorysów](~/ios/user-interface/storyboards/index.md) przewodniku scenorysu Opisuje ekrany (kontrolerów widoku), które tworzą aplikację, elementów interfejsu (widoki), umieszczenie tych kontrolerów widoku, a także ogólny przepływ nawigacji w aplikacji . 

Kontroler widoku ma dwie części: wizualne reprezentacje w narzędziu iOS Designer i skojarzonych klas języka C#:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Kontroler widoku w narzędziu iOS Designer](introduction-images/1-storyboardwithviewcontroller-vsmac.png "kontrolera widoku w narzędziu iOS Designer")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![Kod dla kontrolera widoku](introduction-images/2-viewcontrollercode-vsmac.png "kod dla kontrolera widoku")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kontroler widoku w narzędziu iOS Designer](introduction-images/1-storyboardwithviewcontroller-vs.png "kontrolera widoku w narzędziu iOS Designer")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![Kod dla kontrolera widoku](introduction-images/2-viewcontrollercode-vs.png "kod dla kontrolera widoku")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

W stanie domyślnym kontrolera widoku nie zawiera żadnych funkcji; należy określić ustawienia za pomocą kontrolek. Te kontrolki są umieszczane w widoku kontrolera widoku prostokątny obszar, który zawiera całą zawartość ekranu. Większość kontrolerów widoku zawiera wspólnych formantów, takich jak przyciski, etykiety i pola tekstowe, jak pokazano na poniższym zrzucie ekranu, pokazujące kontrolera widoku zawierający przycisk: 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Kontroler widoku zawierający przycisk](introduction-images/3-viewcontrollerwithbutton-vsmac.png "kontrolera widoku zawierającego przycisku")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kontroler widoku zawierający przycisk](introduction-images/3-viewcontrollerwithbutton-vs.png "kontrolera widoku zawierającego przycisku")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

Niektóre formanty, takie jak etykiety, zawierających tekst statyczny, mogą być dodawane do kontrolera widoku lub left samodzielnie. Jednak bardziej często niż nie formantów muszą być dostosowane programowo. Na przykład dodanie powyżej przycisku powinien zrobić coś, co wybrany program obsługi zdarzeń musi zostać dodany w kodzie.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby uzyskać dostęp do manipulowania przycisku w kodzie, musi mieć unikatowy identyfikator. Podaj unikatowy identyfikator, wybierając przycisk, otwierając **konsoli właściwości**i ustawienie jej **nazwa** pole wartością, taką jak "SubmitButton":

[![Nazwa przycisku Ustawienia w okienku właściwości](introduction-images/4-settingbuttonname-vsmac.png "nazwa przycisku Ustawienia w okienku właściwości")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby uzyskać dostęp do manipulowania przycisku w kodzie, musi mieć unikatowy identyfikator. Podaj unikatowy identyfikator, wybierając przycisk, otwierając **okno właściwości**i ustawienie jej **nazwa** pole wartością, taką jak "SubmitButton":

[![Nazwa przycisku Ustawienia w oknie dialogowym właściwości](introduction-images/4-settingbuttonname-vs.png "nazwa przycisku Ustawienia w oknie dialogowym właściwości")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

Skoro już przycisk o nazwie, może zostać oceniony w kodzie. Ale jak to działa?

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W **konsoli rozwiązania**, po do **ViewController.cs** i klikając wskaźnik ujawnienie wykaże, że kontroler widoku `ViewController` zakresy definicji klasy, dwa pliki, z których każdy zawiera [klasy częściowej](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) definicji:

[![Dwa pliki wchodzących w skład klasa ViewController: ViewController.cs i ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "dwóch plików wchodzących w skład klasa ViewController: ViewController.cs i ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W **Eksploratora rozwiązań**, po do **ViewController.cs** i klikając wskaźnik ujawnienie wykaże, że kontroler widoku `ViewController` definicji klasy, które obejmuje dwa pliki, z których każdy z zawierającą [klasy częściowej](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) definicji:

[![Dwa pliki wchodzących w skład klasa ViewController: ViewController.cs i ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "dwóch plików wchodzących w skład klasa ViewController: ViewController.cs i ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs** powinny zostać wypełnione niestandardowego kodu powiązany z `ViewController` klasy. W tym pliku `ViewController` klasy może odpowiadać systemu IOS do różnych metod cyklu życia kontrolera widoku, dostosowywanie interfejsu użytkownika i reagować na dane wejściowe, takie jak naciśnięcia przycisku użytkownika.

- **ViewController.designer.cs** jest to wygenerowany plik utworzony przez narzędzia iOS Designer do mapowania interfejsu wizualnie tworzony kod. Ponieważ zmiany w tym pliku zostaną zastąpione, nie powinien być modyfikowany. Deklaracje właściwości w tym pliku umożliwiają kodu w `ViewController` klasy do dostępu przez **nazwa**, określa zestaw się w narzędziu iOS Designer. Otwieranie **ViewController.designer.cs** , co spowoduje wyświetlenie następującego kodu:

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

`SubmitButton` Deklaracja właściwości łączy całą `ViewController` klasy — nie tylko **ViewController.designer.cs** plik — przycisk zdefiniowane w scenorysu. Ponieważ **ViewController.cs** definiuje część `ViewController` klasa, ma dostęp do `SubmitButton`.

Poniższy zrzut ekranu przedstawia, że funkcja IntelliSense rozpoznaje teraz `SubmitButton` odwoływać w **ViewController.cs**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Rozpoznawanie odwołania SubmitButton IntelliSense](introduction-images/6-submitbuttonintellisense-vsmac.png "rozpoznawanie odwołań SubmitButton IntelliSense")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Rozpoznawanie odwołania SubmitButton IntelliSense](introduction-images/6-submitbuttonintellisense-vs.png "rozpoznawanie odwołań SubmitButton IntelliSense")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

W tej sekcji wykazała, jak utworzyć przycisk w narzędziu iOS Designer i dostęp do tego przycisku w kodzie.

W pozostałej części tego dokumentu zawiera dalsze Omówienie narzędzia iOS Designer.

## <a name="ios-designer-basics"></a>podstawy narzędzia iOS Designer

Ta sekcja wprowadza części narzędzia iOS Designer i zawiera przewodnik po jego funkcje.

### <a name="launching-the-ios-designer"></a>Uruchamianie narzędzia iOS Designer

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Projekty Xamarin.iOS utworzone za pomocą programu Visual Studio dla komputerów Mac obejmują scenorysu. Aby wyświetlić zawartość scenorysu, dwukrotnie kliknij plik .storyboard **konsoli rozwiązania**:

[![Otwórz scenorys, w narzędziu iOS Designer](introduction-images/7-storyboardopen-vsmac.png "scenorysu Otwórz w narzędziu iOS Designer")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Większość projektów platformy Xamarin.iOS utworzone za pomocą programu Visual Studio 2015 lub 2017 obejmują scenorysu. Aby wyświetlić zawartość scenorysu, dwukrotnie kliknij plik .storyboard **Eksploratora rozwiązań**:

[![Otwórz scenorys, w narzędziu iOS Designer](introduction-images/7-storyboardopen-vs.png "scenorysu Otwórz w narzędziu iOS Designer")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>Funkcje projektanta systemu iOS

Narzędzia iOS Designer zawiera sześć sekcji głównej:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Sekcje narzędzia iOS Designer](introduction-images/8-sixpartsofiosdesigner-vsmac.png "sekcji narzędzia iOS Designer")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **Powierzchni projektu** — narzędzia iOS Designer podstawowym obszarem roboczym. Wyświetlane w obszarze dokumentu, umożliwia ona wizualnego tworzenia interfejsu użytkownika.
2. **Pasek narzędzi ograniczeń** — pozwala na przełączanie między ramki edycji trybu ani ograniczenia edycji, dwa różne sposoby, aby umieść elementy w interfejsie użytkownika.
3. **Przybornik** — Wyświetla listę kontrolerów, obiekty, formanty, widoki danych, aparaty rozpoznawania gestów, windows i pasków, który może być przeciągnięte na powierzchnię projektową i dodawany do interfejsu użytkownika.
4. **Blok właściwości** — pokazuje właściwości wybranej kontrolki, w tym tożsamości, stylów wizualnych, ułatwień dostępu, układ i zachowanie.
5. **Konspekt dokumentu** — przedstawia drzewo elementów sterujących, które tworzą układ interfejsu edytowany. Kliknięcie elementu w drzewie zaznacza go w narzędziu iOS Designer i przedstawia jego właściwości w **konsoli właściwości**. Jest to przydatne do wybierania określonego formantu w interfejsie użytkownika głęboko zagnieżdżone.
6. **Dolny pasek narzędzi** — zawiera opcje umożliwiające zmianę sposobu wyświetlania pliku .storyboard lub .pliki, w tym urządzenia, orientacji i powiększania w narzędziu iOS Designer.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Sekcje narzędzia iOS Designer](introduction-images/8-sixpartsofiosdesigner-vs.png "sekcji narzędzia iOS Designer")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **Powierzchni projektu** — narzędzia iOS Designer podstawowym obszarem roboczym. Wyświetlane w obszarze dokumentu, umożliwia ona wizualnego tworzenia interfejsu użytkownika.
2. **Pasek narzędzi ograniczeń** — pozwala na przełączanie między ramki edycji trybu ani ograniczenia edycji, dwa różne sposoby, aby umieść elementy w interfejsie użytkownika.
3. **Przybornik** — Wyświetla listę kontrolerów, obiekty, formanty, widoki danych, aparaty rozpoznawania gestów, windows i pasków, który może być przeciągnięte na powierzchnię projektową i dodawany do interfejsu użytkownika.
4. **Okno właściwości** — pokazuje właściwości wybranej kontrolki, w tym tożsamości, stylów wizualnych, ułatwień dostępu, układ i zachowanie.
5. **Konspekt dokumentu** — przedstawia drzewo elementów sterujących, które tworzą układ interfejsu edytowany. Kliknięcie elementu w drzewie zaznacza go w narzędziu iOS Designer i przedstawia jego właściwości w **okno właściwości**. Jest to przydatne do wybierania określonego formantu w interfejsie użytkownika głęboko zagnieżdżone.
6. **Dolny pasek narzędzi** — zawiera opcje umożliwiające zmianę sposobu wyświetlania pliku .storyboard lub .pliki, w tym urządzenia, orientacji i powiększania w narzędziu iOS Designer.

-----

### <a name="design-workflow"></a>Projektowanie przepływów pracy

#### <a name="adding-a-control-to-the-interface"></a>Dodawanie kontrolki do interfejsu

Aby dodać formant do interfejsu, przeciągnij je z **przybornika** i upuść je na powierzchni projektowej. Podczas dodawania lub pozycjonowanie kontrolki, wytyczne poziome i pionowe Wyróżnij pozycji układu najczęściej używanych takich jak środka w pionie i w poziomie do środka, marginesy:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![Na powierzchni projektowej wytycznych zaznacz pozycje najczęściej używanych układ](introduction-images/9-layoutguides-vsmac.png "na powierzchni projektowej wytycznych zaznacz pozycje najczęściej używanych układu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Na powierzchni projektowej wytycznych zaznacz pozycje najczęściej używanych układ](introduction-images/9-layoutguides-vs.png "na powierzchni projektowej wytycznych zaznacz pozycje najczęściej używanych układu")

-----

Niebieska linia kropkowana w powyższym przykładzie przedstawiono wskazówki visual wyrównanie w poziomie do środka pomagające w położenie przycisku.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="context-menu-commands"></a>Polecenia menu kontekstowego

Menu kontekstowe jest dostępne zarówno na powierzchni projektowej i **konspekt dokumentu**. To menu zawiera polecenia dla wybranej kontrolki i jego obiektu nadrzędnego, co jest przydatne podczas pracy z widoków w zagnieżdżonej hierarchii:

[![Menu kontekstowe na powierzchni projektowej](introduction-images/10-contextmenudesignsurface-vsmac.png "menu kontekstowego na powierzchni projektowej")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

### <a name="constraints-toolbar"></a>Pasek narzędzi ograniczeń

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
[![Na pasku narzędzi ograniczenia](introduction-images/11-constraintstoolbar-vsmac.png "pasek narzędzi ograniczeń")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Na pasku narzędzi ograniczenia](introduction-images/11-constraintstoolbar-vs.png "pasek narzędzi ograniczeń")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

Pasek narzędzi ograniczeń została zaktualizowana i obecnie składa się z dwóch kontrolek: ramki, w trybie edycji / ograniczenie edytowanie Przełącz tryb i Aktualizuj ograniczenia / zaktualizować przycisk ramki.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Tryb edytowania ramki / ograniczenie edycji Przełącz tryb

W poprzednich wersjach narzędzia iOS Designer klikając pozycję Widok już wybrane na powierzchni projektowej przełączać między ramki edycji tryb i tryb edytowania ograniczenia. Teraz kontrolce przełącznika, na pasku narzędzi ograniczeń przełączy się między tymi tryby edycji.

- Tryb edytowania ramki:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Przycisk tryb edytowania ramki](introduction-images/12a-frameeditingmode-vsmac.png "przycisku tryb edytowania ramki")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Przycisk tryb edytowania ramki](introduction-images/12a-frameeditingmode-vs.png "przycisku tryb edytowania ramki")

-----

- Tryb edytowania ograniczenia:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Przycisk tryb edytowania ograniczenia](introduction-images/12b-constrainteditingmode-vsmac.png "edycji przycisku tryb ograniczenia")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Przycisk tryb edytowania ograniczenia](introduction-images/12b-constrainteditingmode-vs.png "edycji przycisku tryb ograniczenia")

-----

#### <a name="update-constraints--update-frames-button"></a>Aktualizuj ograniczenia / update przycisk ramki

Aktualizuj ograniczenia / update ramek znajdujący przycisku z prawej strony ramki, w trybie edycji / ograniczenie do Przełącz tryb edycji.

- W trybie edycji ramce kliknięcie tego przycisku dostosowuje ramek wszystkie wybrane elementy, aby dopasować swoje ograniczenia.
- W trybie edycji ograniczenia kliknięcie tego przycisku dostosowuje ograniczenia wszystkie wybrane elementy do dopasowania ich ramki.

### <a name="bottom-toolbar"></a>Dolny pasek narzędzi

Dolny pasek narzędzi zapewnia sposób Wybierz urządzenia, orientacji i powiększania, używany do wyświetlania plików storyboard lub .pliki w narzędziu iOS Designer:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dolny pasek narzędzi, używany do wybierania urządzeń i orientację na powierzchnię projektową](introduction-images/13-bottomtoolbar-vsmac.png "dolny pasek narzędzi, używany do wybierania urządzeń i orientację powierzchni projektowej")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dolny pasek narzędzi, używany do wybierania urządzeń i orientację na powierzchnię projektową](introduction-images/13-bottomtoolbar-vs.png "dolny pasek narzędzi, używany do wybierania urządzeń i orientację powierzchni projektowej")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>Urządzenia i orientacji

Po rozwinięciu dolny pasek narzędzi Wyświetla wszystkie urządzenia, orientacje i/lub adaptacje mające zastosowanie do bieżącego dokumentu. Klikając je zmienia widok wyświetlany na powierzchni projektowej. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dolny pasek narzędzi, rozwinąć, aby wyświetlić urządzenia i orientacje](introduction-images/14-bottomtoolbarexpanded-vsmac.png "dolny pasek narzędzi, rozwinąć, aby wyświetlić urządzenia i orientacji")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dolny pasek narzędzi, rozwinąć, aby wyświetlić urządzenia i orientacje](introduction-images/14-bottomtoolbarexpanded-vs.png "dolny pasek narzędzi, rozwinąć, aby wyświetlić urządzenia i orientacji")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

Należy pamiętać o tym, wybierając urządzenie i orientację zmienia tylko, jak narzędzia iOS Designer podglądy projektu. Niezależnie od bieżącego zaznaczenia, nowo dodane ograniczenia są stosowane na wszystkich urządzeniach oraz orientacje, chyba że **Edytuj cechy** przycisk został użyty do określono inaczej.

Gdy [klasy wielkości](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) są [włączone](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes), **Edytuj cechy** przycisk pojawi się na rozwinięte dolnego paska narzędzi.  Klikając **Edytuj cechy** przycisk powoduje wyświetlenie opcji umożliwiających tworzenie odmiany interfejsu, na podstawie klasy rozmiaru, reprezentowane przez wybrane urządzenie i orientacji. Należy wziąć pod uwagę następujące przykłady:

- Jeśli **iPhone SE** / **pionowa**, jest zaznaczone, popover zapewnia funkcje umożliwiające tworzenie odmiany interfejsu compact szerokość, wysokość regularne klasy rozmiaru. 
- Jeśli **iPad Pro 9,7"** / **pozioma** / **pełny ekran** jest zaznaczone, popover zapewnia funkcje umożliwiające tworzenie odmiany interfejsu dla regularne szerokość, wysokość regularne klasy rozmiaru.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dolny pasek narzędzi, używane przez klasy rozmiaru będzie się różnić w interfejsie](introduction-images/15-edittraitsbutton-vsmac.png "dolny pasek narzędzi jest służące do różnicowania interfejs od klasy rozmiaru")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dolny pasek narzędzi, używane przez klasy rozmiaru będzie się różnić w interfejsie](introduction-images/15-edittraitsbutton-vs.png "dolny pasek narzędzi jest służące do różnicowania interfejs od klasy rozmiaru")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>Kontrolki powiększania

Powierzchni projektowej obsługuje powiększanie za pośrednictwem kilku formantów:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![Kontrolki powiększania na pasku dolnym](introduction-images/16-zoomcontrols-vsmac.png "kontrolki powiększania w dolny pasek narzędzi")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Kontrolki powiększania na pasku dolnym](introduction-images/16-zoomcontrols-vs.png "kontrolki powiększania w dolny pasek narzędzi")

-----

Formanty są następujące:

1. Dopasuj widok do rozmiaru
2. Pomniejszanie
3. Powiększanie
4. Rzeczywisty rozmiar (rozmiar w pikselach 1:1)

Te kontrolki Dopasuj powiększenie na powierzchni projektowej. Nie wpływają na interfejsie użytkownika aplikacji w czasie wykonywania.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="properties-pad"></a>Właściwości konsoli

Użyj **konsoli właściwości** do edycji tożsamości, stylów wizualnych, dostępność i zachowanie kontrolki. Poniższy zrzut ekranu przedstawia **konsoli właściwości** opcje dla przycisku:

[![Blok właściwości dla przycisku](introduction-images/17-buttonpropertiespad-vsmac.png "blok właściwości dla przycisku")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>Właściwości sekcje konsoli

**Konsoli właściwości** zawiera trzy sekcje:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="properties-window"></a>Okno Właściwości

Użyj **okno właściwości** do edycji tożsamości, stylów wizualnych, dostępność i zachowanie kontrolki. Poniższy zrzut ekranu przedstawia **okno właściwości** opcje dla przycisku:

[![Okno właściwości dla przycisku](introduction-images/17-buttonpropertieswindow-vs.png "okno właściwości dla przycisku")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>Sekcje okna właściwości

**Okno właściwości** zawiera trzy sekcje:

-----

1.  **Element widget** — najważniejszych właściwości kontrolki, takie jak nazwa klasy, ponownego obliczenia właściwości stylu itp. Właściwości dla zarządzania zawartością formantu są zwyczajowo umieszczane w tym miejscu.
2.  **Układ** — właściwości, które śledzą informacje o położenie i rozmiar kontrolki, w tym ograniczenia i ramki, są wymienione w tym miejscu.
3.  **Zdarzenia** — zdarzenia i procedury obsługi zdarzeń są określone w tym miejscu. Przydatne do obsługi zdarzeń, takich jak dotyk, naciśnij pozycję, przeciągnij itp. Zdarzenia mogą być również obsługiwane bezpośrednio w kodzie.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="editing-properties-in-the-properties-pad"></a>Edytowanie właściwości w okienku właściwości

Oprócz edycja wizualna na powierzchni projektowej, obsługuje edycję właściwości w narzędziu iOS Designer **konsoli właściwości**. Dostępne właściwości zmianom w zależności od wybranej kontrolki, jak pokazano na zrzutach ekranu poniżej:

[![Przycisk Właściwości](introduction-images/18a-buttonpropertiespad-vsmac.png "przycisk Właściwości")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![Wyświetlanie właściwości kontrolera](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "wyświetlić właściwości kontrolera")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="editing-properties-in-the-properties-window"></a>Edytowanie właściwości w oknie dialogowym właściwości

Oprócz edycja wizualna na powierzchni projektowej, obsługuje edycję właściwości w narzędziu iOS Designer **okno właściwości**. Dostępne właściwości zmianom w zależności od wybranej kontrolki, jak pokazano na zrzutach ekranu poniżej:

[![Przycisk Właściwości](introduction-images/18a-buttonpropertieswindow-vs.png "przycisk Właściwości")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![Wyświetlanie właściwości kontrolera](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "wyświetlić właściwości kontrolera")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> Tożsamość części pokazuje teraz Konsola właściwości **modułu** pola. Należy wypełnić w tej sekcji tylko wtedy, gdy współdziałanie z klasami Swift. Dzięki niemu wprowadź nazwę modułu Swift klasy, które są namespaced.

#### <a name="default-values"></a>Wartości domyślne

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Wiele właściwości w **konsoli właściwości** Pokaż żadna wartość lub wartość domyślną. Jednak kod aplikacji nadal mogą modyfikować te wartości. **Konsoli właściwości** nie są wyświetlane wartości ustawione w kodzie.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wiele właściwości w **okno właściwości** Pokaż żadna wartość lub wartość domyślną. Jednak kod aplikacji nadal mogą modyfikować te wartości. **Okno właściwości** nie są wyświetlane wartości ustawione w kodzie.

-----

#### <a name="event-handlers"></a>Programy obsługi zdarzeń

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby określić obsługę zdarzeń niestandardowych dla różnych zdarzeń, użyj **zdarzenia** karcie **konsoli właściwości**. Na przykład w poniższym zrzucie ekranu `HandleClick` obsługiwała przycisku **Touch się wewnątrz** zdarzeń:

[![Blok właściwości, za pomocą programu obsługi zdarzeń dla przycisku](introduction-images/19-buttonpropertiespadevents-vsmac.png "konsoli do właściwości, za pomocą programu obsługi zdarzeń dla przycisku")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby określić obsługę zdarzeń niestandardowych dla różnych zdarzeń, użyj **zdarzenia** karcie **okno właściwości**. Na przykład w poniższym zrzucie ekranu `HandleClick` obsługiwała przycisku **Touch się wewnątrz** zdarzeń:

[![Okno właściwości, za pomocą programu obsługi zdarzeń dla przycisku](introduction-images/19-buttonpropertieswindowevents-vs.png "okno właściwości, za pomocą programu obsługi zdarzeń dla przycisku")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

Po program obsługi zdarzeń została określona, metody o tej samej nazwie należy dodać do odpowiedniej klasy kontrolera widoku. W przeciwnym razie `unrecognized selector` wyjątek ma miejsce, gdy naciśnięcia przycisku:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Wyjątek nierozpoznany selektor](introduction-images/20-unrecognizedselector-vsmac.png "wyjątek nierozpoznany selektora")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

Należy zauważyć, że po program obsługi zdarzeń została określona w **konsoli właściwości**, z systemem iOS Designer Otwórz odpowiadającym mu plikiem kodu natychmiast i oferty wstawić deklaracji metody. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wyjątek nierozpoznany selektor](introduction-images/20-unrecognizedselector-vs.png "wyjątek nierozpoznany selektora")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

Na przykład, który używa procedur obsługi zdarzeń niestandardowych, zobacz [Witaj, iOS przewodnik wprowadzenie](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>Wyświetlanie konspektu

Narzędzia iOS Designer można również wyświetlić hierarchię interfejsu kontrolek jako kontur. Konspekt jest dostępna, wybierając **konspekt dokumentu** karty, jak pokazano poniżej:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Konspekt dokumentu](introduction-images/21-buttonoutlineview-vsmac.png "konspekt dokumentu")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Konspekt dokumentu](introduction-images/21-buttonoutlineview-vs.png "konspekt dokumentu")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

Zaznaczony formant w widoku konspektu pozostają zsynchronizowane z wybranej kontrolki na powierzchni projektowej.  Ta funkcja jest przydatne w przypadku wybraniu elementu w hierarchii interfejsu głęboko zagnieżdżonych.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="revert-to-xcode"></a>Przywróć Xcode

Istnieje możliwość zamiennie używać środowiska Xcode Interface Builder i Projektant ustawień systemu iOS. Aby otworzyć scenorysu lub plik .pliki w Xcode Interface Builder, kliknij prawym przyciskiem myszy plik i wybierz pozycję **Otwórz za pomocą > Xcode Interface Builder**, jak pokazano na poniższym zrzucie ekranu:

[![Otwieranie scenorysu w środowisku Xcode Interface Builder](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "otwierania scenorysu w środowisku Xcode Interface Builder")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Po wprowadzeniu zmian w środowisku Xcode Interface Builder, Zapisz plik, a następnie wróć do programu Visual Studio dla komputerów Mac. Zmiany będą synchronizowane z projektu platformy Xamarin.iOS.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

## <a name="xib-support"></a>.pliki pomocy technicznej

Narzędzia iOS Designer obsługuje tworzenia, edytowania i zarządzania nimi .pliki XIb. Te są plikami XML tego respresent pojedynczego, niestandardowe widoki, które mogą być dodawane do hierarchii widoku aplikacji. Plik .pliki zazwyczaj reprezentuje interfejs dla jednego widoku lub ekranu w aplikacji scenorysu stanowi wiele ekranów i przejść między nimi.

Istnieje wiele opinii, o których rozwiązanie — .pliki XIb, scenorysów lub kodu — działa najlepiej za utworzenie i utrzymywanie interfejsu użytkownika. W rzeczywistości istnieje doskonałe rozwiązanie, a jest zawsze warto biorąc pod uwagę najlepszym narzędziem dla zadania pod ręką. Inaczej mówiąc, .pliki XIb są zazwyczaj najbardziej użyteczne, gdy jest używana do reprezentowania widok niestandardowy potrzebne w wielu miejscach w aplikacji, takich jak komórki widoku tabeli niestandardowej. 

Więcej dokumentacji na temat używania .pliki XIb można znaleźć w następujących przepisy:

- [Przy użyciu szablonu .pliki widoku](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/using_the_ios_view_xib_template)
- [Tworzenie TableViewCell niestandardowe, za pomocą .pliki](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/tables/custom-tableviewcell)
- [Tworzenie ekranu uruchamiania przy użyciu .pliki](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)

Aby uzyskać więcej informacji na temat użycia scenorysów dotyczą [wprowadzenie do scenorysów](~/ios/user-interface/storyboards/index.md).

To i inne przewodniki dotyczące projektanta iOS odwołują się do użycia scenorysów jako standardowego podejścia do tworzenia interfejsów użytkownika, ponieważ większość Xamarin.iOS nowe szablony projektów domyślnie udostępnia scenorysu.

## <a name="summary"></a>Podsumowanie

Ten przewodnik podana wprowadzenie do narzędzia iOS Designer, opisujące jego funkcji i tworzenie konspektu narzędzi, które oferuje projektowania projektowania atrakcyjnych interfejsów użytkownika.

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do scenorysów](~/ios/user-interface/storyboards/index.md)
- [iOS Designable wskazówki formantów](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Witaj, iOS](~/ios/get-started/hello-ios/index.md)
- [Witaj, iOS Multiscreen (wiele ekranów)](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Omówienie projektanta systemu android](~/android/user-interface/android-designer/index.md)
- [Klasy częściowe i metody](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Zagłębieniem się w Projektancie platformy Xamarin dla systemu iOS — rozwój 2014 (wideo)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [Za pomocą narzędzia iOS Designer, aby utworzyć ekran uruchamiania (wideo)](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
