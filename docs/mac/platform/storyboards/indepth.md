---
title: Praca z Scenorys w Xamarin.Mac
description: Ten dokument w tym artykule opisano sposób pracy z scenorys w Xamarin.Mac badanie jak załadować je z kodu, cyklu życia kontrolera widoku, łańcuch obiektu odpowiadającego w trybie, segues okna kontrolerów, aparaty rozpoznawania gestów i inne.
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 72986ed4247c3b6f66f6f1813d74bf0a95d0de53
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792843"
---
# <a name="working-with-storyboards-in-xamarinmac"></a>Praca z Scenorys w Xamarin.Mac

Scenorysu definiuje wszystkie interfejsu użytkownika dla danej aplikacji, podzielić na funkcjonalności omówienie jego kontrolerów widoku. W Konstruktorze interfejsu w programie Xcode każdy z tych kontrolerów przebywa w jego własnej sceny.

[![](indepth-images/intro01.png "Scenorysu w Konstruktorze interfejsu w środowisku Xcode")](indepth-images/intro01.png#lightbox)

Plik zasobów jest scenorysu (z rozszerzeniem `.storyboard`) która pobiera zawartych w pakiecie aplikacji Xamarin.Mac podczas kompilacji i wysłane. Aby zdefiniować początkowy scenorysu dla aplikacji, edytować go w `Info.plist` plik i wybierz **interfejsu Main** w polu listy rozwijanej: 

[![](indepth-images/sb01.png "Edytor Info.plist")](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>Podczas ładowania z kodu

Może to być potrzebne do ładowania określonej scenorysu z kodu i ręcznie utworzyć kontroler widoku. Poniższy kod służy do wykonania tej akcji:

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

`FromName` Ładuje plik scenorysu o podanej nazwie, która została uwzględniona w pakiecie aplikacji. `InstantiateControllerWithIdentifier` Tworzy wystąpienie kontrolera widoku o podanej tożsamości. Ustawisz tożsamość konstruktora interfejsu w środowisku Xcode podczas projektowania interfejsu użytkownika:

[![](indepth-images/sb02.png "Ustawienie Identyfikatora scenorysu")](indepth-images/sb02.png#lightbox)

Opcjonalnie można użyć `InstantiateInitialController` metodę, aby załadować przypisany początkowej kontrolera interfejsu konstruktora kontroler widoku:

[![](indepth-images/sb03.png "Ustawianie początkowego kontrolera")](indepth-images/sb03.png#lightbox)

Zostało oznaczone przez **punktu wejścia scenorysu** i powyżej Strzałka Otwórz została zakończona.

<a name="View-Controllers" />

## <a name="view-controllers"></a>Kontrolery widoku

Widok kontrolerów zdefiniowania relacji między danego widoku informacji w aplikacji Mac i model danych, który zawiera te informacje. Każdy sceny najwyższego poziomu w scenorysu reprezentuje jeden kontroler widoku w kodzie aplikacji Xamarin.Mac.

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>Cykl życia kontrolera widoku

Dodano kilka nowych metod `NSViewController` klasy do obsługi Scenorys w macOS. To przede wszystkim metody wykonaj używać odpowiedzieć na cyklu życia widoku kontrolowane przez dany kontroler widoku:

- `ViewDidLoad` — Ta metoda jest wywoływana po załadowaniu widoku z pliku scenorysu.
- `ViewWillAppear` — Ta metoda jest wywoływana tuż przed widok jest wyświetlany na ekranie.
- `ViewDidAppear` — Ta metoda jest wywoływana bezpośrednio po widoku został wyświetlony na ekranie.
- `ViewWillDisappear` — Ta metoda jest wywoływana tuż przed widoku jest usuwany z ekranu.
- `ViewDidDisappear` — Ta metoda jest wywoływana bezpośrednio po widoku został usunięty z ekranu.
- `UpdateViewConstraints` — Ta metoda jest wywoływana, gdy ograniczenia, które określają widok automatycznie układu położenie i rozmiar muszą zostać zaktualizowane.
- `ViewWillLayout` — Ta metoda jest wywoływana tuż przed widoków podrzędnych tego widoku zostały przedstawione na ekranie.
- `ViewDidLayout` — Ta metoda jest wywoływana bezpośrednio po widoków podrzędnych widoku widoku zostały przedstawione na ekranie.

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>Łańcuch obiektu odpowiadającego

Ponadto `NSViewControllers` są teraz częścią okna _obiektu odpowiadającego w łańcuchu_:

[![](indepth-images/vc01.png "Łańcuch obiektu odpowiadającego")](indepth-images/vc01.png#lightbox)

I jako taki są przewodowej w górę do pobierania i reagowania na zdarzenia, takie jak wycinania, kopiowania i wklejania zaznaczenia elementu menu. To automatyczne kontrolera widoku danych przesyłanych w sieci w górę występuje tylko w aplikacji działających na macOS Sierra (10.12) lub nowszej.

<a name="Containment" />

### <a name="containment"></a>Zawierania

W Scenorys, kontrolerów widoku (na przykład kontroler widoku podziału i kartę View Controller) można teraz wdrożyć _zawierania_, że "zawierają" innych sub kontrolerów widoku:

[![](indepth-images/vc02.png "Przykład zawierania kontrolera widoku")](indepth-images/vc02.png#lightbox)

Kontrolery widok podrzędny zawiera metody i właściwości, aby powiązać je wykonać ich kopię do ich nadrzędnej widoku kontrolera i pracy z wyświetlanie i usuwanie widoków z ekranu.

Wszystkie kontrolery widoku kontenera wbudowane w system macOS mają określony układ, które sugeruje firmy Apple należy wykonać w przypadku tworzenia własnego niestandardowego kontrolerów widoku kontenera:

[![](indepth-images/vc03.png "Układ kontrolera widoku")](indepth-images/vc03.png#lightbox)

Kontroler widoku kolekcji zawiera tablicę elementów widok kolekcji, z których każdy zawiera co najmniej jeden kontroler widoku, które zawierają własne widoki.

<a name="Segues" />

## <a name="segues"></a>Segues

Segues Podaj relacje między wszystkimi sceny, które definiują Interfejsie użytkownika aplikacji. Jeśli znasz pracy w Scenorys w systemie iOS, należy pamiętać, że Segues dla systemu iOS zwykle określają przejścia między widokami pełny ekran. Ta różni się od macOS, gdy Segues zazwyczaj Definiowanie "[zawierania](#Containment)", gdzie jedną scenę jest elementem podrzędnym elementu nadrzędnego sceny.

W macOS większość aplikacji często do grupowania ich widoków razem w tym samym oknie za pomocą elementów interfejsu użytkownika, takich jak widoki i karty. W przeciwieństwie do systemu iOS, których widoków musi zostać przeniesieni włączać i wyłączać ekranu, ze względu na ograniczone fizyczna wyświetlanie miejsca.

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>Segues prezentacji

Biorąc pod uwagę tendencji macOS w kierunku zawierania, istnieją sytuacje, gdy _Segues prezentacji_ są używane, takie jak okna modalne, widoki arkusza i Popovers. System macOS zapewnia segue następujące wbudowane typy:

- **Pokaż** — element docelowy Segue będzie wyświetlany jako okno niemodalne. Na przykład można użyć tego typu Segue do prezentowania inne wystąpienie okno dokumentu w aplikacji.
- **Modalne** — przedstawia element docelowy Segue jako modalne okno. Na przykład można użyć tego typu Segue do prezentowania okna Preferencje dla aplikacji.
- **Arkusz** — przedstawia element docelowy Segue jako arkusz dołączony do okna nadrzędnego. Na przykład, użyj tego typu segue do prezentowania Znajdź i Zamień arkusza.
- **Popover** — przedstawia element docelowy Segue jak popover okna. Na przykład można użyć tego typu Segue do prezentowania opcji, po kliknięciu przez użytkownika elementu interfejsu użytkownika.
- **Niestandardowe** — przedstawia element docelowy Segue przy użyciu niestandardowego typu Segue zdefiniowane przez dewelopera. Zobacz [Tworzenie niestandardowych Segues](#Creating-Custom-Segues) sekcji poniżej, aby uzyskać więcej informacji.

Podczas korzystania z Segues prezentacji, można zastąpić `PrepareForSegue` metoda kontrolera widoku nadrzędnego prezentacji zainicjować i zmienne i zapewniają kontroler widoku jest prezentowana żadnych danych.

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>Wyzwalane Segues

Wyzwalane Segues umożliwiają określenie nazwane Segues (za pośrednictwem ich **identyfikator** właściwości konstruktora interfejsu) i je wyzwalane przez zdarzenia, takie jak kliknięcie przycisku użytkownika lub przez wywołanie metody `PerformSegue` w kodzie:

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

Identyfikator Segue zdefiniowano wewnątrz konstruktora interfejsu w środowisku Xcode podczas układania Interfejsie użytkownika aplikacji:

[![](indepth-images/sg02.png "Wprowadzanie nazwy Segue")](indepth-images/sg02.png#lightbox)

W kontroler widoku, który działa jako źródło Segue, należy zastąpić `PrepareForSegue` — metoda i czy inicjowanie wymagane przed wykonaniem Segue i określony kontroler widoku są wyświetlane:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

Opcjonalnie można zastąpić `ShouldPerfromSegue` metody i kontroli czy Segue faktycznie jest wykonywane przy użyciu kodu C#. Ręcznie przedstawioną kontrolerów widoku, wywołania ich `DismissController` metodę, aby usunąć je z ekranu, gdy nie są już potrzebne.

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>Tworzenie niestandardowych Segues

Może to być razy, gdy aplikacja wymaga typu Segue nie został dostarczony przez Segues kompilacji w zdefiniowanych w macOS. Jeśli jest to możliwe, można utworzyć Segue niestandardowe, które można przypisać w Konstruktorze interfejsu w środowisku Xcode podczas rozmieszczania w Interfejsie użytkownika aplikacji.

Na przykład aby utworzyć nowy typ Segue, który zastępuje bieżący kontroler widoku w oknie (zamiast otwierania docelowy sceny w nowym oknie), możemy użyć poniższego kodu:

```csharp
using System;
using AppKit;
using Foundation;

namespace OnCardMac
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Swap the controllers
            source.View.Window.ContentViewController = destination;

            // Release memory
            source.RemoveFromParentViewController ();
        }
        #endregion

    }
        
}
```

Kilka rzeczy do uwzględnienia w tym miejscu:

- Używamy `Register` atrybut do udostępnienia tej klasy Objective-C/macOS.
- Firma Microsoft są zastępowanie `Perform` metoda faktycznie wykonania akcji naszych Segue niestandardowych.
- Możemy zastąpić okna `ContentViewController` kontroler z jednym określonym przez element docelowy (docelowy) Segue.
- Zostaną usunięte oryginalnego kontrolera widoku w celu zwolnienia pamięci przy użyciu `RemoveFromParentViewController` metody.

Aby użyć tego nowego typu Segue w Konstruktorze interfejsu w programie Xcode, należy najpierw skompilować aplikację, a następnie przełącz xcode i dodać nowe Segue między dwoma sceny. Ustaw **styl** do **niestandardowych** i **Segue klasy** do `ReplaceViewSegue` (Nazwa klasy Segue nasze niestandardowe):

[![](indepth-images/sg01.png "Klasa Segue ustawień")](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>Kontrolery okna

Okno kontrolery zawierają i kontrolowania różnych typów okna, utworzonych aplikacji macOS. Scenorys mają następujące funkcje:

1. Użytkownik musi podać zawartości kontrolera widoku. Są to tej samej zawartości kontroler widoku, który ma podrzędne okna.
2. `Storyboard` Właściwości będzie zawierać scenorysu, który kontroler okna został załadowany z, w przeciwnym razie `null` Jeśli nie został załadowany z scenorysu.
3. Możesz wywołać `DismissController` metodę, aby zamknąć okno danego i usunąć go z widoku.

Jak widok kontrolery, zaimplementować okno kontrolerów `PerformSegue`, `PrepareForSegue` i `ShouldPerfromSegue` metody i mogą być używane jako źródło operacji Segue.

Okno kontrolera są odpowiedzialne za poniższe funkcje aplikacji macOS:

- Zarządzają wdowa określone.
- Zarządzają pasek tytułu okna i paska narzędzi (jeśli jest dostępna).
- Zarządzają zawartości kontroler widoku do wyświetlenia zawartości okna.

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>Aparaty rozpoznawania gestów

Aparaty rozpoznawania gestów dla macOS są prawie takie same jak ich odpowiedniki w systemie iOS i umożliwiają deweloperom łatwe dodawanie gestów (np. Kliknięcie przycisku myszy) do elementów w Interfejsie użytkownika aplikacji.

Jednak gdy gestów w systemie iOS są określane przez projekt aplikacji (takich jak naciśnięcie ekranu z dwoma palcami), najbardziej gestów macOS są określane przez sprzęt.

Za pomocą aparatów rozpoznawania gestów, można znacznie zmniejszyć ilość kod wymagany do dodawania niestandardowych interakcji dla elementu w interfejsie użytkownika. Jak mógł automatycznie określić między dwa razy i jednego kliknięcia, kliknij i przeciągnij zdarzeń itd.

Zamiast przesłaniać metodę `MouseDown` zdarzenia w kontrolerze widoku, należy używać aparat rozpoznawania gestów obsługi zdarzenia wejściowe użytkownika podczas pracy z Scenorys.

Następujące aparaty rozpoznawania gestów są dostępne w macOS:

- `NSClickGestureRecognizer` — Rejestracja myszy w dół i zdarzeń.
- `NSPanGestureRecognizer` -Rejestrów myszy przycisk w dół, przeciągnij i zwolnij zdarzenia.
- `NSPressGestureRecognizer` -Rejestrów przytrzymując przycisk myszy na określoną ilość czasu zdarzeń.
- `NSMagnificationGestureRecognizer` -Spowoduje zarejestrowanie zdarzenia powiększenia trackpad sprzętu.
- `NSRotationGestureRecognizer` -Spowoduje zarejestrowanie zdarzenia obrotu trackpad sprzętu.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Za pomocą odwołań do scenorysu

Odwołanie do scenorysu pozwala pobrać projekt scenorysu dużych, złożonych i podziel go na mniejsze Scenorys, których uzyskać odwołania z oryginalnego, w związku z tym usuwanie złożoność oraz wprowadzania poszczególnych powstałe w ten sposób usuwania Scenorys łatwiejsze do projektowania i Obsługa.

Ponadto można podać odwołanie scenorysu _zakotwiczenia_ do innego sceny w ramach tej samej Storyboard lub określonych sceny na inny.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Odwołanie do zewnętrznej scenorysu

Aby dodać odwołanie do zewnętrznej scenorysu, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Dodaj** > **nowego pliku...**   >  **Mac** > **scenorysu**. Wprowadź **nazwa** nowego scenorysu i kliknij **nowy** przycisk: 

    [![](indepth-images/ref01.png "Dodawanie nowego scenorysu")](indepth-images/ref01.png#lightbox)
2. W **Eksploratora rozwiązań**, kliknij dwukrotnie nową nazwę scenorysu, aby go otworzyć do edycji w Konstruktorze interfejsu w środowisku Xcode.
2. Projektowanie układu sceny nowego scenorysu zazwyczaj będzie i Zapisz zmiany: 

    [![](indepth-images/ref02.png "Projektowanie interfejsu")](indepth-images/ref02.png#lightbox)
3. Przełącz się do scenorysu, który ma być Dodawanie odwołania do konstruktora interfejsu.
4. Przeciągnij **scenorysu odwołanie** z **obiekt bibliotece** na powierzchnię projektu: 

    [![](indepth-images/ref03.png "Wybieranie odwołanie scenorysu w bibliotece")](indepth-images/ref03.png#lightbox)
5. W **inspektora atrybutu**, wybierz nazwę **scenorysu** utworzoną wcześniej: 

    [![](indepth-images/ref04.png "Konfigurowanie odwołania")](indepth-images/ref04.png#lightbox)
6. Formantu, kliknij pozycję element Widget interfejsu użytkownika (na przykład przycisk) na istniejących sceny i Utwórz nowe Segue do **odwołania scenorysu** nowo utworzony.  Wybierz z menu podręcznego **Pokaż** przeprowadzenie Segue: 

    [![](indepth-images/ref06.png "Ustawienie typu Segue")](indepth-images/ref06.png#lightbox) 
8. Zapisz zmiany do scenorysu.
9. Wróć do programu Visual Studio for Mac zsynchronizować zmiany.

Gdy aplikacja jest uruchamiana, a użytkownik kliknie elementu interfejsu użytkownika utworzone Segue z początkowej kontrolera okno z zewnętrznego scenorysu, określonych w odwołaniu scenorysu będą wyświetlane.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Odwołanie do określonego sceny w zewnętrznych scenorysu

Można dodać odwołania do sceny określonego scenorysu zewnętrzne (i nie początkowej kontroler okna), wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie zewnętrznych scenorysu, aby go otworzyć do edycji w Konstruktorze interfejsu w środowisku Xcode.
2. Dodaj nowe sceny i projektowanie układu w zwykły sposób: 

    [![](indepth-images/ref07.png "Projektowanie układu w środowisku Xcode")](indepth-images/ref07.png#lightbox)
3. W **inspektora tożsamości**, wprowadź **identyfikator scenorysu** sceny nowe okno kontrolera: 

    [![](indepth-images/ref08.png "Ustawienie Identyfikatora scenorysu")](indepth-images/ref08.png#lightbox)
3. Otwórz scenorysu, który ma być Dodawanie odwołania do konstruktora interfejsu.
4. Przeciągnij **scenorysu odwołanie** z **obiekt bibliotece** na powierzchnię projektu: 

    [![](indepth-images/ref03.png "Wybieranie odwołanie scenorysu z biblioteki")](indepth-images/ref03.png#lightbox)
5. W **inspektora tożsamości**, wybierz nazwę **scenorysu** i **Identyfikatora odwołania** (identyfikator scenorysu) sceny utworzoną wcześniej: 

    [![](indepth-images/ref09.png "Ustawienie Identyfikatora odwołania")](indepth-images/ref09.png#lightbox)
6. Formantu, kliknij pozycję element Widget interfejsu użytkownika (na przykład przycisk) na istniejących sceny i Utwórz nowe Segue do **odwołania scenorysu** nowo utworzony. Wybierz z menu podręcznego **Pokaż** przeprowadzenie Segue: 

    [![](indepth-images/ref06.png "Ustawienie typu Segue")](indepth-images/ref06.png#lightbox) 
8. Zapisz zmiany do scenorysu.
9. Wróć do programu Visual Studio for Mac zsynchronizować zmiany.

Gdy aplikacja jest uruchamiania i użytkownik kliknie elementu interfejsu użytkownika, utworzony Segue przy użyciu sceny z danego **identyfikator scenorysu** z zewnętrznego scenorysu, określonych w odwołaniu scenorysu będzie wyświetlany.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Odwołanie do sceny określonych w tej samej scenorysu

Aby dodać odwołanie do określonego sceny tego samego scenorysu, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie scenorysu, aby go otworzyć do edycji.
2. Dodaj nowe sceny i projektowanie układu w zwykły sposób: 

    [![](indepth-images/ref11.png "Edytowanie scenorysu w środowisku Xcode")](indepth-images/ref11.png#lightbox)
3. W **inspektora tożsamości**, wprowadź **identyfikator scenorysu** sceny nowe okno kontrolera: 

    [![](indepth-images/ref12.png "Ustawienie Identyfikatora scenorysu")](indepth-images/ref12.png#lightbox)
3. Przeciągnij **scenorysu odwołanie** z **przybornika** na powierzchnię projektu: 

    [![](indepth-images/ref03.png "Wybieranie odwołanie scenorysu z biblioteki")](indepth-images/ref03.png#lightbox)
5. W **inspektora atrybutu**, wybierz pozycję **Identyfikatora odwołania** (identyfikator scenorysu) sceny utworzoną wcześniej: 

    [![](indepth-images/ref13.png "Ustawienie Identyfikatora odwołania")](indepth-images/ref13.png#lightbox)
6. Formantu, kliknij pozycję element Widget interfejsu użytkownika (na przykład przycisk) na istniejących sceny i Utwórz nowe Segue do **odwołania scenorysu** nowo utworzony. Wybierz z menu podręcznego **Pokaż** przeprowadzenie Segue: 

    [![](indepth-images/ref06.png "Wybieranie typu Segue")](indepth-images/ref06.png#lightbox) 
8. Zapisz zmiany do scenorysu.
9. Wróć do programu Visual Studio for Mac zsynchronizować zmiany.

Gdy aplikacja jest uruchamiania i użytkownik kliknie elementu interfejsu użytkownika, utworzony Segue przy użyciu sceny z danym **identyfikator scenorysu** w tej samej scenorysu, określonych w odwołaniu scenorysu będzie wyświetlany.

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Przykład scenorysu złożonych

Złożonego przykładu pracy z Scenorys w aplikacji Xamarin.Mac, zobacz [SourceWriter Przykładowa aplikacja](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter jest proste źródła edytora kodu, który zapewnia obsługę uzupełniania kodu i wyróżnianie składni proste.

Pełni komentarzem kodu SourceWriter i, jeśli jest dostępna, zostały podane linki z technologii kluczy lub metod do odpowiednich informacji w dokumentacji przewodniki Xamarin.Mac.

## <a name="related-links"></a>Linki pokrewne

- [MacStoryboard (przykład)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Praca z systemu Windows](~/mac/user-interface/window.md)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do systemu Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
