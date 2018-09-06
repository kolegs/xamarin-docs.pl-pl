---
title: Witaj, Mac — wskazówki
description: Ten dokument przedstawia sposób tworzenia aplikacji rozszerzenia Xamarin.Mac i wprowadza programu Visual Studio dla komputerów Mac, Xcode i programu Interface Builder. Omówiono w nim uwidaczniającą kontrolki interfejsu użytkownika do kodu za pośrednictwem gniazd i akcji, a następnie pokazano, jak kompilowanie, uruchamianie i testowanie aplikacji platformy Xamarin.Mac.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 37D0E9E6-979B-7069-B3BE-C5F0AF99BA72
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: 81a15f85c3b3b10525e2eb4966900edc95224fe0
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780659"
---
# <a name="hello-mac--walkthrough"></a>Witaj, Mac — wskazówki

Platforma Xamarin.Mac umożliwia tworzenie w pełni natywnych aplikacji dla komputerów Mac w językach C# i platformy .NET za pomocą tej samej biblioteki OS X i formantów interfejsu, które są używane podczas tworzenia w *języka Objective-C* i *Xcode*. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, deweloper może skorzystać program Xcode _programu Interface Builder_ do tworzenia interfejsów użytkownika aplikacji (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Ponadto ponieważ aplikacji rozszerzenia Xamarin.Mac jest napisana w języku C# i .NET, wspólny kod zaplecza mogą być udostępniane Xamarin.iOS i Xamarin.Android aplikacje mobilne Korzystając z możliwości dostarczania natywne środowisko na każdej platformie.

W tym artykule przedstawiono kluczowe założenia niezbędne do utworzenia aplikacja dla komputerów Mac przy użyciu platformy Xamarin.Mac, program Visual Studio dla komputerów Mac i program Xcode Interface Builder przez Instruktaż proces tworzenia prostego **Witaj, Mac** aplikacji, który zlicza liczbę razy przycisk kliknięto:

[![](hello-mac-images/run02.png "Przykład Witaj, Mac aplikacji")](hello-mac-images/run02.png#lightbox)

Następujące pojęcia będą objęte pomocą techniczną:

-  **Program Visual Studio for Mac** — wprowadzenie do programu Visual Studio dla komputerów Mac oraz sposób tworzenia aplikacji platformy Xamarin.Mac z nim.
-  **Anatomia aplikacji rozszerzenia Xamarin.Mac** — Xamarin.Mac co aplikacja składa się z.
-  **Program Xcode Interface Builder** — w jaki sposób do korzystania ze środowiska Xcode użytkownika programu Interface Builder, aby zdefiniować interfejs użytkownika aplikacji.
-  **Gniazda i akcje** — jak Podłączanie formantów interfejsu użytkownika za pomocą gniazd i akcji.
-  **Wdrożenie/testowanie** — jak uruchamianie i testowanie aplikacji platformy Xamarin.Mac.

## <a name="requirements"></a>Wymagania

Aby opracować aplikacje platformy Xamarin.Mac przy użyciu najnowszych interfejsów API dla systemu macOS, należy:

- Mac komputera uruchomionego system macOS High Sierra (10.13) lub nowszej.
- [Środowisko Xcode 9 lub nowszą](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).
- Najnowszą wersję [Xamarin.Mac i Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/installation).

Do uruchamiania aplikacji utworzonych za pomocą platformy Xamarin.Mac, potrzebne będą:

- Komputer Mac, systemem Mac OS X 10,7 lub nowszej.

> [!WARNING]
> Nadchodząca wersja platformy Xamarin.Mac 4.8 będzie obsługiwała tylko system macOS w wersji 10.9 lub wyższej.
> Poprzednie wersje platformy Xamarin.Mac obsługiwały system macOS w wersji 10.7 lub wyższej, ale starsze wersje systemu macOS nie posiadają odpowiedniej infrastruktury TLS do obsługi protokołu TLS 1.2. W przypadku systemu macOS 10.7 lub macOS 10.8 należy użyć platformy Xamarin.Mac w wersji 4.6 lub wcześniejszej.

## <a name="starting-a-new-xamarinmac-app-in-visual-studio-for-mac"></a>Uruchamianie nowej aplikacji platformy Xamarin.Mac w programie Visual Studio dla komputerów Mac

Jak wspomniano powyżej, ten przewodnik przeprowadzi użytkownika przez kroki umożliwiające utworzenie aplikacji Mac o nazwie `Hello_Mac` dodająca jeden przycisk i etykietę do głównego okna. Po kliknięciu przycisku etykiety wyświetli liczba przypadków, gdy zostanie kliknięta.

Aby rozpocząć pracę, wykonaj następujące czynności:

1. Uruchom program Visual Studio dla komputerów Mac:

    [![](hello-mac-images/setup01.png "Główny program Visual Studio for Mac interfejsu")](hello-mac-images/setup01.png#lightbox)

2. Kliknij pozycję **nowe rozwiązanie...**  w lewy górny róg ekranu, aby otworzyć łącze **nowy projekt** okno dialogowe:

    [![](hello-mac-images/setup03.png "Tworzenie nowego rozwiązania w programie Visual Studio dla komputerów Mac")](hello-mac-images/setup02.png#lightbox)

3. Wybierz **Mac** > **aplikacji** > **aplikacja cocoa dla** i kliknij przycisk **dalej** przycisku:

    [![](hello-mac-images/setup03.png "Wybieranie aplikacja cocoa dla")](hello-mac-images/setup03.png#lightbox)

4. Wprowadź `Hello_Mac` dla **nazwy aplikacji**i zachować wszystkie inne jako domyślny. Kliknij przycisk **dalej**:

    [![](hello-mac-images/setup05.png "Ustawianie nazwy aplikacji")](hello-mac-images/setup05.png#lightbox)

4. Podczas tworzenia rozwiązania, który będzie przechowywania kilku różnych projektów, deweloper może być ustawienie innego **Nazwa rozwiązania** w tym miejscu, ale dla celów tego przykładu, pozostaw ona ustawiona domyślnie jest taka sama jak  **Nazwa projektu**:

    [![](hello-mac-images/setup04.png "Weryfikowanie nowe szczegóły rozwiązania")](hello-mac-images/setup04.png#lightbox)

5. Kliknij przycisk **Utwórz** przycisku.

Visual Studio dla komputerów Mac Tworzenie nowej aplikacji platformy Xamarin.Mac i wyświetlić domyślne pliki, które poproś o dodanie Cię do rozwiązania aplikacji:

 [![](hello-mac-images/project01.png "Nowy widok domyślny rozwiązania")](hello-mac-images/project01.png#lightbox)

Program Visual Studio for Mac używa **rozwiązania** i **projektów**, dokładnie tak samo, jak program Visual Studio. To rozwiązanie jest kontenerem, który może zawierać jeden lub więcej projektów; projekty mogą obejmować aplikacje, obsługa bibliotek, testowanie aplikacji itp. W takich przypadkach program Visual Studio for Mac został utworzony rozwiązania i projektu aplikacji automatycznie.

Jeśli to konieczne, deweloper może utworzyć co najmniej jeden kod projekty bibliotek, które zawierają kod wspólnej, udostępnionej. Te projekty biblioteki mogą być używane przez projekt aplikacji lub współużytkowane z innymi projektami aplikacji rozszerzenia Xamarin.Mac (lub Xamarin.iOS i Xamarin.Android na podstawie typu kodu) w taki sam sposób jak standardowej aplikacji .NET.

## <a name="anatomy-of-a-xamarinmac-application"></a>Anatomia aplikacji rozszerzenia Xamarin.Mac

Jeśli znasz programowania dla systemu iOS jest dostępnych wiele podobieństw. W rzeczywistości systemu iOS używa struktury CocoaTouch, która jest wersją slimmed w dół Cocoa, używany przez Mac, więc będzie skrzyżowany wiele pojęć związanych z.

Przyjrzyj się pliki w projekcie:

-   `Main.cs` — Zawiera główny punkt wejścia aplikacji. Po uruchomieniu aplikacji zawiera bardzo najwyższej klasy i metody, która jest uruchamiana.
-   `AppDelegate.cs` — Ten plik zawiera klasy głównej aplikacji, która jest odpowiedzialna za nasłuchuje zdarzeń z systemu operacyjnego.
-   `Info.plist` — Ten plik zawiera właściwości aplikacji, takie jak nazwa aplikacji, ikony itp.
-   `Entitlements.plist` — W tym pliki zawiera uprawnień dla aplikacji i zezwala na dostęp do elementów, takich jak obsługa piaskownicy i usługi iCloud.
-  `Main.storyboard` — Definiuje interfejs użytkownika (Windows i menu) dla aplikacji i układa połączenie między Windows za pomocą przejścia. Scenorysy są plikami XML, które zawierają definicje widoków (elementy interfejsu użytkownika). Ten plik może być tworzone i konserwowane przez programu Interface Builder wewnątrz środowiska Xcode.
-   `ViewController.cs` — Jest to kontroler dla okna głównego. Kontrolery zostaną objęte szczegółowo w innym artykule, ale na razie można traktować kontrolera głównego silnika dowolnego określonego widoku.
-   `ViewController.designer.cs` — Ten plik zawiera kod nadmiar, który ułatwia integrowanie z ekranu głównego interfejsu użytkownika.

Poniższe sekcje potrwa rzut oka za pośrednictwem niektóre z tych plików. Później będzie można eksplorować bardziej szczegółowo, ale to dobry pomysł, aby zrozumieć ich podstawy teraz.

### <a name="maincs"></a>Main.CS

**Main.cs** plik jest bardzo proste. Zawiera ona statyczna `Main` metodę, która tworzy nowe wystąpienie aplikacji rozszerzenia Xamarin.Mac i przekazuje nazwę klasy, która będzie obsługiwać zdarzenia systemu operacyjnego, czyli w tym przypadku `AppDelegate` klasy:

```csharp
using System;
using System.Drawing;
using Foundation;
using AppKit;
using ObjCRuntime;

namespace Hello_Mac
{
        class MainClass
        {
                static void Main (string[] args)
                {
                        NSApplication.Init ();
                        NSApplication.Main (args);
                }
        }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs` Plik zawiera `AppDelegate` klasy, która jest odpowiedzialna za tworzenie okien i nasłuchuje zdarzeń systemu operacyjnego:

```csharp
using AppKit;
using Foundation;

namespace Hello_Mac
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

Ten kod jest prawdopodobnie nieznane, chyba że deweloperów opracowała aplikację dla systemu iOS przed, ale jest to dość proste.

`DidFinishLaunching` Metoda uruchamia się po utworzeniu wystąpienia aplikacji i jest odpowiedzialna za faktycznie tworzenia okna aplikacji i rozpoczyna proces wyświetlania widoku w nim.

`WillTerminate` Metoda zostanie wywołana po użytkownik lub system ma wystąpienia zamknięcie aplikacji. Deweloper należy używać tej metody do zakończenia aplikacji przed jej kończy działanie (na przykład zapisywania preferencji użytkownika lub rozmiaru okna i lokalizacji).

### <a name="viewcontrollercs"></a>ViewController.cs

Cocoa (oraz poprzez tworzenie wartości pochodnych, CocoaTouch) używa, co jest nazywane *Model View Controller* wzorzec (MVC). `ViewController` Deklaracji reprezentuje obiekt, który kontroluje okna rzeczywistych aplikacji. Ogólnie rzecz biorąc dla każdego okna tworzone (i wiele zastosowań w systemie windows) ma kontroler, który jest odpowiedzialny za cyklu życia okna, takie jak wyświetlanie, dodawanie nowych widoków (formanty) do jego itp.

`ViewController` Klasa jest kontrolera głównego okna. Oznacza to, że jest on odpowiedzialny cykl życiowy okna głównego. To będą badane szczegółowo później na take teraz rzut oka na jego:

```csharp
using System;

using AppKit;
using Foundation;

namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }
    }
}
```

### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Pliku projektanta dla klasy okno główne jest puste w tej chwili, ale jej zostaną wypełnione automatycznie przez program Visual Studio dla komputerów Mac podczas tworzenia interfejsu użytkownika za pomocą programu Interface Builder wewnątrz środowiska Xcode:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac to store outlets and
// actions made in the UI designer. If it is removed, they will be lost.
// Manual changes to this file may not be handled correctly.
//
using Foundation;

namespace Hello_Mac
{
    [Register ("ViewController")]
    partial class ViewController
    {
        void ReleaseDesignerOutlets ()
        {
        }
    }
}
```

Deweloper nie jest zazwyczaj zainteresowani pliki projektanta, ponieważ są automatycznie zarządzane przez program Visual Studio dla komputerów Mac, a kod wymaganego nadmiar, który zezwala na dostęp do formantów, które zostały dodane do dowolnego okna lub widoku w aplikacji.

Projekt aplikacji platformy Xamarin.Mac, które są tworzone i podstawową wiedzę na temat jego składników Przełącz się do środowiska Xcode, aby utworzyć interfejs użytkownika przy użyciu programu Interface Builder.

### <a name="infoplist"></a>Info.plist

`Info.plist` Plik zawiera informacje na temat aplikacji platformy Xamarin.Mac jego **nazwa** i **identyfikatora pakietu**:

[![](hello-mac-images/infoplist01.png "Visual Studio dla komputerów Mac Edytor plist")](hello-mac-images/infoplist01.png#lightbox)

Umożliwia on również definiowanie _scenorysu_ który będzie używany do wyświetlania interfejsu użytkownika dla aplikacji platformy Xamarin.Mac w obszarze **interfejsu Main** listy rozwijanej. W przypadku w powyższym przykładzie `Main` na liście rozwijanej odnosi się do `Main.storyboard` w drzewie źródła projektu **Eksploratora rozwiązań**. Definiuje również wady aplikacji poprzez określenie *wykaz zasobów* zawierającą je (**AppIcon** w tym przypadku).

### <a name="entitlementsplist"></a>Entitlements.plist

Aplikacja `Entitlements.plist` plik steruje uprawnień, które zawiera aplikacji rozszerzenia Xamarin.Mac, takie jak **Sandboxing** i **iCloud**:

[![](hello-mac-images/entitlements01.png "Visual Studio dla komputerów Mac Edytor uprawnień")](hello-mac-images/entitlements01.png#lightbox)

Na przykład Witaj, świecie wymagane będzie nie uprawnień. Następna sekcja przedstawia sposób użycia program Xcode Interface Builder, aby edytować `Main.storyboard` plików i definiowania interfejsu użytkownika aplikacji rozszerzenia Xamarin.Mac.

## <a name="introduction-to-xcode-and-interface-builder"></a>Wprowadzenie do programu Xcode i programu Interface Builder

W ramach programu Xcode Apple został utworzony z narzędzia o nazwie programu Interface Builder, która umożliwia deweloperom wizualne Tworzenie interfejsu użytkownika w projektancie. Platforma Xamarin.Mac bezproblemowe integruje się z programu Interface Builder, dzięki czemu można utworzyć przy użyciu tych samych narzędzi, co użytkownicy języka Objective-C w interfejsie użytkownika.

Aby rozpocząć pracę, kliknij dwukrotnie `Main.storyboard` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji w programie Xcode i programu Interface Builder:

[![](hello-mac-images/xcode01.png "Plik Main.storyboard w Eksploratorze rozwiązań")](hello-mac-images/xcode01.png#lightbox)

To powinno Uruchom Xcode i wyglądać mniej więcej następująco:

[![](hello-mac-images/xcode02.png "Domyślny widok Xcode Interface Builder")](hello-mac-images/xcode02.png#lightbox)

Przed rozpoczęciem projektowania interfejsu, skorzystaj z krótkim omówieniem poznaniu z najważniejszych funkcji, które będą używane w środowisku Xcode.

> [!NOTE]
> Deweloper nie ma używać środowiska Xcode i programu Interface Builder do tworzenia interfejsu użytkownika dla aplikacji platformy Xamarin.Mac, interfejs użytkownika można tworzyć bezpośrednio z kodu C#, ale która wykracza poza zakres tego artykułu. Dla uproszczenia, należy go będzie używać programu Interface Builder do tworzenia interfejsu użytkownika w pozostałej części tego samouczka.


### <a name="components-of-xcode"></a>Składniki programu Xcode

Podczas otwierania `.storyboard` pliku w programie Xcode w programie Visual Studio dla komputerów Mac, zostanie ona otwarta z **Nawigatora projektu** po lewej stronie, **hierarchii interfejsów** i **Edytor interfejsu**w środku i **energetyczne i użyteczności publicznej właściwości** sekcji po prawej stronie:

[![](hello-mac-images/xcode03.png "Różne części programu Interface Builder w środowisku Xcode")](hello-mac-images/xcode03.png#lightbox)

Poniższe sekcje Przyjrzyj się każdej z tych czy funkcje środowiska Xcode i jak ich używać do tworzenia interfejsu dla aplikacji platformy Xamarin.Mac.

### <a name="project-navigation"></a>Nawigacja po projekcie

Podczas otwierania `.storyboard` plik do edycji w środowisku Xcode program Visual Studio dla komputerów Mac tworzy *plik projektu Xcode* w tle informacji o zmianach między sobą i Xcode. Później gdy deweloper przełącza powrót do programu Visual Studio dla komputerów Mac z narzędzia Xcode, wszelkie zmiany wprowadzone do tego projektu są synchronizowane z projektem platformy Xamarin.Mac przez program Visual Studio dla komputerów Mac.

**Nawigacja po projekcie** sekcja umożliwia dla deweloperów poruszać się między wszystkie pliki, które tworzą to _podkładki_ projektu Xcode. Zwykle, tylko będą zainteresowani `.storyboard` , takich jak pliki na tej liście `Main.storyboard`.

### <a name="interface-hierarchy"></a>Interfejs hierarchii

**Hierarchii interfejsów** sekcja umożliwia deweloperom łatwy dostęp kilka właściwości klucza obiektu interfejsu użytkownika, takie jak jego **symbole zastępcze** i głównych **okna**. W tej sekcji można uzyskać dostęp do poszczególnych elementów (widoki), które tworzą interfejs użytkownika oraz dostosować sposób, w których są zagnieżdżone, przeciągając je w hierarchii.

### <a name="interface-editor"></a>Edytor interfejsu

**Edytor interfejsu** sekcja zawiera powierzchni, w którym interfejs użytkownika jest graficznie rozmieszczony. Przeciągnij elementy z **biblioteki** części **energetyczne i użyteczności publicznej właściwości** sekcję, aby utworzyć projekt. Gdy elementy interfejsu użytkownika (widoki) są dodawane do powierzchni projektu, zostaną one dodane do **hierarchii interfejsów** w kolejności, w jakiej pojawiają się w temacie **Edytor interfejsu**.

### <a name="properties--utilities"></a>Właściwości & Narzędzia

**Energetyczne i użyteczności publicznej właściwości** sekcja jest podzielona na dwie główne sekcje **właściwości** (nazywane również inspektorzy) i **biblioteki**:

[![](hello-mac-images/xcode04.png "Inspektor właściwości")](hello-mac-images/xcode04.png#lightbox)

Początkowo w tej sekcji jest prawie pusta, jednak jeśli Deweloper wybiera element w **Edytor interfejsu** lub **hierarchii interfejsów**, **właściwości** sekcja będzie wypełniana przy użyciu informacji na temat danego elementu i właściwości, które można zmienić.

W ramach **właściwości** sekcji, istnieją ośmiu różnych *karty Inspektor*, jak pokazano na poniższej ilustracji:

[![](hello-mac-images/xcode05.png "Przegląd wszystkich inspektorzy")](hello-mac-images/xcode05.png#lightbox)

### <a name="properties--utility-types"></a>Właściwości & Narzędzia typów

Od lewej do prawej są następujące karty:

-   **Plik Inspektor** — Inspektor plik zawiera informacje o plikach, takie jak nazwa pliku i lokalizację pliku Xib, która jest edytowany.
-   **Szybka pomoc** — karta szybki pomocy zawiera pomocy kontekstowej, w oparciu o co jest wybrane w środowisku Xcode.
-   **Inspektor tożsamości** — Inspektor tożsamości zawiera informacje dotyczące wybranego formantu/widoku.
-   **Atrybuty Inspektor** — Inspektor atrybuty umożliwia deweloperowi dostosować różne atrybuty formantu/widoku.
-   **Rozmiar Inspektor** — rozmiar Inspector umożliwia deweloperowi kontrolowanie rozmiaru i zmienianie rozmiaru zachowanie kontroli/widoku.
-   **Inspektor połączeń** — pokazuje Inspektor połączeń **ujścia** i **akcji** połączeń z wybranych kontrolek. Gniazda i akcje zostaną dokładniej omówione poniżej.
-   **Inspektor powiązania** — Inspektor powiązania umożliwia deweloperom konfigurowanie kontrolek, tak aby ich wartości są automatycznie powiązany z modelami danych.
-   **Wyświetlić efekty inspektora** — widok efekty Inspektor umożliwia deweloperowi do określenia wpływu na kontrolki, takie jak animacji.

Użyj **biblioteki** sekcję, aby znaleźć formanty i obiekty, aby umieścić designer do tworzenia graficznego interfejsu użytkownika:

[![](hello-mac-images/xcode06.png "Inspektor biblioteki środowiska Xcode")](hello-mac-images/xcode06.png#lightbox)

## <a name="creating-the-interface"></a>Tworzenie interfejsu

Podstawowe informacje dotyczące środowiska Xcode IDE i programu Interface Builder objętych usługą Deweloper umożliwia tworzenie interfejsu użytkownika dla widoku głównego.

Wykonaj następujące czynności:

1. W programie Xcode, przeciągnij **przycisk** z **biblioteki**:

    [![](hello-mac-images/xcode07.png "Wybieranie NSButton inspektora biblioteki")](hello-mac-images/xcode07.png#lightbox)

2. Upuść przycisk na **widoku** (w obszarze **kontroler okna**) w **Edytor interfejsu**:

    [![](hello-mac-images/xcode08.png "Dodawanie przycisku do projektu interfejsu")](hello-mac-images/xcode08.png#lightbox)

3. Kliknij pozycję **tytuł** właściwość **Inspektor atrybut** i Zmień tytuł na przycisku, aby `Click Me`:

    [![](hello-mac-images/xcode09.png "Ustawianie właściwości przycisku.")](hello-mac-images/xcode09.png#lightbox)

4. Przeciągnij **etykiety** z **biblioteki**:

    [![](hello-mac-images/xcode10.png "Wybieranie etykiety z Inspektora biblioteki")](hello-mac-images/xcode10.png#lightbox)

5. Upuść etykiety na **okna** obok przycisku w **Edytor interfejsu**:

    [![](hello-mac-images/xcode11.png "Dodawanie etykiety do projektu interfejsu")](hello-mac-images/xcode11.png#lightbox)

6. Chwyć prawy uchwyt na etykiecie i przeciągnij go do czasu jego krawędzi okna:

    [![](hello-mac-images/xcode12.png "Zmiana rozmiaru etykiety")](hello-mac-images/xcode12.png#lightbox)

7. Wybierz przycisk, który właśnie został dodany w **Edytor interfejsu**i kliknij przycisk **Edytor ograniczenia** ikonę i u dołu okna:

    [![](hello-mac-images/xcode13.png "Dodawanie ograniczenia do przycisku")](hello-mac-images/xcode13.png#lightbox)

8. W górnej części edytora kliknij **czerwoną I świateł** u góry i po lewej stronie. Ponieważ zmiany rozmiaru okna, pozwoli to zachować przycisku w tej samej lokalizacji, w lewym górnym rogu ekranu.

9. Następnie sprawdź **wysokość** i **szerokość** pola i użyj rozmiarów domyślne. Dzięki temu przycisk w taki sam rozmiar, gdy okno zmienia rozmiar.

10. Kliknij przycisk **Dodaj ograniczenia 4** przycisk, aby dodać ograniczenia i zamknij Edytor.

11. Wybierz etykietę, a następnie kliknij przycisk **Edytor ograniczenia** ponownie ikonę:

    [![](hello-mac-images/xcode14.png "Dodawanie ograniczenia do etykiety")](hello-mac-images/xcode14.png#lightbox)

12. Klikając **czerwoną I świateł** u góry, prawej i lewej z **Edytor ograniczenia**, informuje etykiety reaguje na danym X i Y lokalizacji i powiększanie i zmniejszanie jako uruchomiona zmiany rozmiaru okna aplikacja.

13. Ponownie sprawdź **wysokość** polu i użyj domyślnego rozmiaru, a następnie kliknij przycisk **Dodaj ograniczenia 4** przycisk, aby dodać ograniczenia i zamknij Edytor.

14. Zapisz zmiany w interfejsie użytkownika.

Podczas zmiany rozmiaru i przenoszenie kontrolę nad, zwróć uwagę, że programu Interface Builder zapewnia wskazówki przydatne przystawki, oparte na [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Te wskazówki pomogą deweloperom tworzenie wysokiej jakości aplikacji, których będą używać dobrze znanych wygląd i działanie dla użytkowników komputerów Mac.

Szukaj w **hierarchii interfejsów** sekcji, aby zobaczyć, jak pokazano układ i hierarchię elementów, które tworzą interfejs użytkownika:

[![](hello-mac-images/xcode15.png "Zaznaczenie elementu w hierarchii interfejsów")](hello-mac-images/xcode15.png#lightbox)

W tym miejscu deweloper może wybierz elementy do edycji lub przeciągnij, aby zmienić kolejność elementów interfejsu użytkownika, jeśli to konieczne. Na przykład jeśli elementu interfejsu użytkownika są objęte przez inny element, można go przeciągania w dół na liście, aby stał się element umieszczony najwyżej w oknie.

Przy użyciu interfejsu użytkownika tworzone deweloper musi udostępnić elementów interfejsu użytkownika, dzięki czemu platformy Xamarin.Mac mogą uzyskać dostęp i wchodzić z nimi w interakcje w kodzie języka C#. Następnej sekcji **gniazd i akcje**, pokazuje, jak to zrobić.

### <a name="outlets-and-actions"></a>Gniazda i akcje

Dlatego co to są **gniazd** i **akcje**? W tradycyjnego programowania interfejsu użytkownika platformy .NET, kontrolki interfejsu użytkownika jest automatycznie udostępniane jako właściwość, gdy jest ona dodawana. Rzeczy działają inaczej w Mac, po prostu dodawania formantów do widoku nie stał się dostępny do kodu. Deweloper musi udostępnić jawnie elementu interfejsu użytkownika do kodu. W kolejności to zrobić, Apple oferuje dwie opcje:

-   **Gniazda** — gniazda są analogiczne do właściwości. Jeśli Deweloper tworzącej kontrolki do źródła, jest uwidaczniany w kodzie za pomocą właściwości, tak robią czynności, jak dołączyć procedury obsługi zdarzeń, wywoływanie metod na jego itp.
-   **Akcje** — akcje są analogiczne do wzorca polecenia na platformie WPF. Na przykład gdy akcja jest wykonywana na kontrolki, załóżmy, że kliknięcie przycisku, formant będzie automatycznie wywoływać metodę w kodzie. Akcje są wydajne i wygodne, ponieważ Deweloper można powiązać wiele formantów do tego samego działania.

W środowisku Xcode **gniazd** i **akcje** są dodawane bezpośrednio w kodzie za pomocą *przeciągnięcie formantu*. Oznacza to, że do utworzenia w szczególności **ujścia** lub **akcji**, deweloper wybierze element control, aby dodać **ujścia** lub **akcji** do, naciśnij i przytrzymaj **kontroli** klawisz na klawiaturze, a następnie przeciągnij tę kontrolkę bezpośrednio w kodzie.

Dla deweloperów platformy Xamarin.Mac, oznacza to, że deweloper będzie przeciągnij do pliki szczątkowe języka Objective-C, odpowiadające w pliku C# gdzie chcesz utworzyć **ujścia** lub **akcji**. Program Visual Studio for Mac utworzony plik o nazwie `ViewController.h` jako część podkładki projekt Xcode wygenerowane za pomocą konstruktora interfejsu:

[![](hello-mac-images/xcode16.png "Wyświetlanie źródła w środowisku Xcode")](hello-mac-images/xcode16.png#lightbox)

Tego wycinka `.h` pliku wstecznych `ViewController.designer.cs` , jest automatycznie dodawany do projektu rozszerzenia Xamarin.Mac, gdy nowy `NSWindow` zostanie utworzony. Ten plik będzie używany do synchronizowania zmian wprowadzonych przez programu Interface Builder i on **gniazd** i **akcje** są tworzone tak, aby elementy interfejsu użytkownika są widoczne dla kodu C#.

#### <a name="adding-an-outlet"></a>Dodawanie źródła

Z podstawową wiedzę na temat tego, jaki **gniazd** i **akcje** Utwórz **ujścia** do udostępnienia etykiety utworzone w celu kodu języka C#.

Wykonaj następujące czynności:

1. W środowisku Xcode w najdalej z prawej strony dłoni górnym rogu ekranu, kliknij przycisk **okrąg Double** przycisk, aby otworzyć **edytora Asystenta**:

    [![](hello-mac-images/outlet01.png "Wyświetlania edytora Asystenta ustawień")](hello-mac-images/outlet01.png#lightbox)

2. Xcode zostanie przełączona do trybu widoku złożonego za pomocą **Edytor interfejsu** po jednej stronie i **Edytor kodu** z drugiej strony.

3. Należy zauważyć, że środowisko Xcode jest pobierany automatycznie **ViewController.m** w pliku **Edytor kodu**, który jest nieprawidłowy. Dyskusji nad czym **gniazd** i **akcje** znajdują się powyżej, deweloper będzie musiała mieć **ViewController.h** wybrane.

4. W górnej części **Edytor kodu** kliknij **łącze automatyczne** i wybierz `ViewController.h` pliku:

    [![](hello-mac-images/outlet02.png "Wybieranie odpowiedniego pliku")](hello-mac-images/outlet02.png#lightbox)

5. Xcode teraz powinny mieć poprawny plik wybrane:

    [![](hello-mac-images/outlet03.png "Wyświetlanie plik ViewController.h")](hello-mac-images/outlet03.png#lightbox)

6. **Ostatnim krokiem było bardzo ważne!** Jeśli deweloper nie ma odpowiedniego pliku wybrane, nie będzie mógł utworzyć **gniazd** i **akcje** lub będzie narażona do niewłaściwej klasy w języku C#!

7. W **Edytor interfejsu**, naciśnij i przytrzymaj **kontroli** klawisz na klawiaturze i kliknij i przeciągnij etykietę utworzone powyżej do edytora kodu tuż poniżej `@interface ViewController : NSViewController {}` kodu:

    [![](hello-mac-images/outlet04.png "Przeciąganie w celu utworzenia źródła")](hello-mac-images/outlet04.png#lightbox)

8. Pojawi się okno dialogowe. Pozostaw **połączenia** równa **ujścia** i wprowadź `ClickedLabel` dla **nazwa**:

    [![](hello-mac-images/outlet05.png "Definiowanie ujścia")](hello-mac-images/outlet05.png#lightbox)

9. Kliknij przycisk **Connect** przycisk, aby utworzyć **ujścia**:

    [![](hello-mac-images/outlet06.png "Wyświetlanie końcowy gniazda")](hello-mac-images/outlet06.png#lightbox)

10. Zapisz zmiany w pliku.

#### <a name="adding-an-action"></a>Dodawanie operacji

Następnie udostępnić ten przycisk, aby kod C#. Podobnie jak etykiety powyżej, deweloper może powiązać przycisku do **ujścia**. Ponieważ chcemy odpowiedzieć na przycisku kliknięto, użyj **akcji** zamiast tego.

Wykonaj następujące czynności:

1. Upewnij się, że środowisko Xcode jest nadal w **edytora Asystenta** i **ViewController.h** plik jest widoczny w **Edytor kodu**.
2. W **Edytor interfejsu**, naciśnij i przytrzymaj **kontroli** klawisz na klawiaturze i kliknij i przeciągnij przycisk utworzone powyżej do edytora kodu tuż poniżej `@property (assign) IBOutlet NSTextField *ClickedLabel;` kodu:

    [![](hello-mac-images/action01.png "Przeciąganie w celu utworzenia akcji")](hello-mac-images/action01.png#lightbox)

3. Zmiana **połączenia** typ **akcji**:

    [![](hello-mac-images/action02.png "Definiowanie akcji")](hello-mac-images/action02.png#lightbox)

4. Wprowadź `ClickedButton` jako **nazwa**:

    [![](hello-mac-images/action03.png "Nowa akcja nazewnictwa")](hello-mac-images/action03.png#lightbox)

5. Kliknij przycisk **Connect** przycisk, aby utworzyć **akcji**:

    [![](hello-mac-images/action04.png "Wyświetlanie ostateczna Akcja")](hello-mac-images/action04.png#lightbox)

6. Zapisz zmiany w pliku.

Przy użyciu interfejsu użytkownika przewodowej w górę i udostępniana dla kodu C# przejdź z powrotem do programu Visual Studio dla komputerów Mac, a następnie przejdź do niego synchronizować zmiany wprowadzone w programie Xcode i programu Interface Builder.

> [!NOTE]
> Prawdopodobnie zajęło dużo czasu, aby utworzyć interfejs użytkownika i **gniazd** i **akcje** tego pierwszej aplikacji i może się wydawać dużo pracy, ale wprowadzono wiele nowych pojęć i zajmuje dużo czasu obejmujących nowych podstaw. Po ćwiczenia, od pewnego czasu i pracy z programu Interface Builder, ten interfejs i wszystkie jego **gniazd** i **akcje** mogą być tworzone w tylko minutę lub dwie.

### <a name="synchronizing-changes-with-xcode"></a>Synchronizowanie zmian z narzędziem Xcode

Gdy deweloper przełącza powrót do programu Visual Studio dla komputerów Mac z narzędzia Xcode, wszelkie zmiany wprowadzone w programie Xcode zostaną automatycznie zsynchronizowane z projektem platformy Xamarin.Mac.

Wybierz **ViewController.designer.cs** w **Eksploratora rozwiązań** aby zobaczyć, jak **ujścia** i **akcji** gotowe zostały w języku C# Kod:

[![](hello-mac-images/sync01.png "Synchronizowanie zmian z narzędziem Xcode")](hello-mac-images/sync01.png#lightbox)

Zwróć uwagę jak dwie definicje w **ViewController.designer.cs** pliku:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Wiersz w górę z definicjami w `ViewController.h` pliku w programie Xcode:

```csharp
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Visual Studio dla komputerów Mac wykrywa zmiany **.h** pliku i automatycznie synchronizuje zmiany we właściwej **. Designer.cs narzędzie** plik, aby udostępnić je do aplikacji. Należy zauważyć, że **ViewController.designer.cs** jest klasę częściową, tak aby program Visual Studio for Mac nie trzeba modyfikować **ViewController.cs** który spowodowałoby zastąpienie wszelkie zmiany wprowadzone do dewelopera Klasa.

Normalnie, deweloper nie będzie trzeba otworzyć **ViewController.designer.cs**, jego został przedstawiony tutaj wyłącznie w celach edukacyjnych.

> [!NOTE]
> W większości sytuacji programu Visual Studio dla komputerów Mac zostanie automatycznie zobaczyć wszelkie zmiany wprowadzone w środowisku Xcode i synchronizować je do projektu platformy Xamarin.Mac. W wystąpieniu wyłączone synchronizacji nie jest realizowane automatycznie przejdź z powrotem do środowiska Xcode, a następnie z powrotem do programu Visual Studio dla komputerów Mac. Zwykle spowoduje to rozpoczęcie cyklu synchronizacji.

## <a name="writing-the-code"></a>Pisanie kodu

Za pomocą interfejsu użytkownika tworzone i jego elementy interfejsu użytkownika udostępniana dla kodu za pomocą **gniazd** i **akcje**, możemy przystąpić na koniec napisać kod Ożyw program.

Dla tej aplikacji przykładowej za każdym razem, gdy kliknięto przycisk pierwszej etykiecie zostaną zaktualizowane do pokazania, ile razy przycisk został kliknięty. Aby to zrobić, otwórz `ViewController.cs` plik do edycji, klikając go dwukrotnie **Eksploratora rozwiązań**:

[![](hello-mac-images/code01.png "Wyświetlanie pliku ViewController.cs w programie Visual Studio dla komputerów Mac")](hello-mac-images/code01.png#lightbox)

Najpierw Utwórz zmienną poziomie klasy w `ViewController` klasy do śledzenia liczby kliknięć, które miały miejsce. Edytuj definicję klasy i przypisz ją wyglądać następująco:

```csharp
namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Dalej w tej samej klasy (`ViewController`), Zastąp `ViewDidLoad` metody i dodawanie fragmentów kodu, aby ustawić pierwszy komunikat dla etykiety:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Użyj `ViewDidLoad`, zamiast innej metody, takie jak `Initialize`, ponieważ `ViewDidLoad` nosi nazwę *po* system operacyjny został załadowany i tworzone w interfejsie użytkownika z **.storyboard** pliku. Jeśli Deweloper próbowano uzyskać dostęp formant etykiety przed **.storyboard** plik został w pełni załadowany i wystąpienia, jaką uzyskują `NullReferenceException` błąd, ponieważ formant etykiety nie będą jeszcze istnieje.

Następnie dodaj kod, aby odpowiedzieć na kliknięcie przycisku przez użytkownika. Dodaj następującą metodę częściowe do `ViewController` klasy:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Ten kod dołącza do **akcji** utworzone w środowisku Xcode i programu Interface Builder i zostanie wywołana w dowolnym momencie użytkownik kliknie przycisk.

## <a name="testing-the-application"></a>Testowanie aplikacji

Nadszedł czas, aby skompilować i uruchomić aplikację, aby upewnić się, że działa on zgodnie z oczekiwaniami. Deweloper może skompilować i uruchomić wszystkie w jednym kroku, lub można je tworzyć bez konieczności uruchamiania ich.

Zawsze, gdy aplikacja jest wbudowana, deweloper można wybrać, jakiego rodzaju kompilacji mają:

-   **Debugowanie** — Kompilacja debugowania jest skompilowany w **.app** pliku (aplikacje) przy użyciu licznych dodatkowe metadane, który umożliwia deweloperom debugowania, co się dzieje, gdy aplikacja jest uruchomiona.
-   **Zwolnij** — kompilację wydania wzrasta, powstaje **.app** plik, ale nie zawiera informacje o debugowaniu, dzięki czemu jest mniejszy i jest wykonywany szybciej.

Deweloper można wybrać typ kompilacji z **selektor konfiguracji** lewy górny róg programu Visual Studio dla komputerów Mac ekranu:

[![](hello-mac-images/run01.png "Wybieranie kompilacji debugowania")](hello-mac-images/run01.png#lightbox)

## <a name="building-the-application"></a>Kompilowanie aplikacji

W tym przykładzie mamy po prostu chcesz kompilacji debugowania, więc upewnij się, że **debugowania** jest zaznaczone. Najpierw utwórz aplikację, albo naciskając **⌘B**, lub z **kompilacji** menu, wybierz **kompilacji wszystkich**.

Jeśli nie odnaleziono żadnych błędów, **kompilacji zakończyło się pomyślnie** komunikat zostanie wyświetlony w programie Visual Studio dla komputerów Mac, pasek stanu. Jeśli wystąpiły błędy, przejrzyj projekt i upewnij się, że powyższe kroki zostały prawidłowo wykonane. Rozpocznij, sprawdzając, czy kod (zarówno w środowisku Xcode i w programie Visual Studio dla komputerów Mac) zgodny z kodem w tym samouczku.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Istnieją trzy sposoby, aby uruchomić aplikację:

-  Naciśnij klawisz **⌘ + Enter**.
-  Z **Uruchom** menu, wybierz **debugowania**.
-  Kliknij przycisk **Odtwórz** przycisku w programie Visual Studio dla komputerów Mac paska narzędzi (tuż powyżej **Eksploratora rozwiązań**).

Aplikacja utworzy (Jeśli nie zostały skompilowane już), uruchomić w trybie debugowania oraz Wyświetl jego okno główny interfejs:

[![](hello-mac-images/run02.png "Uruchamianie aplikacji")](hello-mac-images/run02.png#lightbox)

Jeśli tego przycisku kilka razy, etykieta powinien zostać zaktualizowany z liczbą:

[![](hello-mac-images/run03.png "Wyświetlanie wyników klikanie przycisku")](hello-mac-images/run03.png#lightbox)

## <a name="where-to-next"></a>Gdzie można dalej

Za pomocą podstawy pracy z aplikacją platformy Xamarin.Mac w dół Spójrz na następujące dokumenty, aby lepiej zrozumieć zasady:

- [Wprowadzenie do scenorysów](~/mac/platform/storyboards/index.md) — ten artykuł zawiera wprowadzenie do pracy ze Scenorysami w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie Interfejsie użytkownika aplikacji za pomocą scenorysów i plików program Xcode Interface Builder.
- [Windows](~/mac/user-interface/window.md) — w tym artykule opisano pracy z Windows i paneli w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie Windows i paneli w programie Xcode i interfejs builder ładowania Windows i panele z .pliki XIb przy użyciu Windows i reagowanie na Windows w kodzie języka C#.
- [Okna dialogowe](~/mac/user-interface/dialog.md) — w tym artykule opisano pracę z oknami dialogowymi i modalne Windows w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie modalne Windows w programie Xcode i interfejs builder pracę w standardowych oknach dialogowych, wyświetlanie i reagowanie na Windows w kodzie języka C#.
- [Alerty](~/mac/user-interface/alert.md) — w tym artykule opisano Praca z alertami w aplikacji platformy Xamarin.Mac. Obejmuje ona tworzenie i wyświetlanie alertów z kodu C# oraz reagowanie na alerty.
- [Menu](~/mac/user-interface/menu.md) — menu są używane w różnych części interfejsu użytkownika aplikacji dla komputerów Mac; z głównego menu aplikacji w górnej części ekranu, aby menu podręczne i kontekstowych, które mogą występować w dowolnym miejscu w oknie. Menu są integralną częścią środowiska użytkownika aplikacji dla komputerów Mac. W tym artykule opisano Praca z menu Cocoa w aplikacji platformy Xamarin.Mac.
- [Paski narzędzi](~/mac/user-interface/toolbar.md) — w tym artykule opisano pracę z pasków narzędzi w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie pasków narzędzi Xcode i interfejs konstruktora, jak udostępnić elementów paska narzędzi do kodu przy użyciu gniazd i akcje, włączanie i wyłączanie elementów paska narzędzi i na koniec odpowiadanie na elementy paska narzędzi w kodzie języka C#.
- [Widoki tabel](~/mac/user-interface/table-view.md) — w tym artykule opisano pracę z widoki tabel w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie widoki tabel w programie Xcode i interfejs builder, jak udostępniać elementy widoku tabeli do kodu przy użyciu gniazd i akcje, wypełnianie elementy tabeli i na koniec odpowiadanie na tabeli wyświetlania elementów w kodzie języka C#.
- [Widoki konspektu](~/mac/user-interface/outline-view.md) — w tym artykule opisano pracę z widoki konspektu w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie widoki konspektu w Xcode i interfejs konstruktora, jak udostępnić konspektu wyświetlanie elementów kodu przy użyciu gniazd i akcje, wypełnianie elementy konspektu i na koniec odpowiadanie na konspekt wyświetlania elementów w kodzie języka C#.
- [Źródło listy](~/mac/user-interface/source-list.md) — w tym artykule opisano pracę z listami źródła w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie źródła wyświetla w Xcode i interfejs konstruktora, jak udostępnić źródła Wyświetla listę elementów do kodu przy użyciu gniazd i akcje, wypełnianie źródła elementów listy i na koniec odpowiada dla źródłowej listy elementów w kodzie języka C#.
- [Widoki kolekcji](~/mac/user-interface/collection-view.md) — w tym artykule opisano pracy za pomocą widoków kolekcji w aplikacji platformy Xamarin.Mac. Obejmuje on utworzenie i utrzymywanie widoki kolekcji w programie Xcode i interfejs builder, jak udostępnić widok kolekcji elementów kodu przy użyciu gniazd i akcje, wypełnianie widoków kolekcji i na koniec odpowiadanie na widoki kolekcji w kodzie języka C#.
- [Praca z obrazami](~/mac/app-fundamentals/image.md) — w tym artykule opisano pracę z obrazów i ikon w aplikacji platformy Xamarin.Mac. Obejmuje on tworzenia i utrzymywania obrazów potrzebne do utworzenia ikony aplikacji i korzystania z obrazów w kodzie C# i program Xcode Interface Builder.

Również sugerować przeglądając [galerii przykładów Mac](https://developer.xamarin.com/samples/mac/all/), zawiera wiele gotowych do użycia kodu, ułatwiający deweloperów szybkie rozpoczęcie projektów platformy Xamarin.Mac na wyższy.

Na przykład Pełna aplikacja platformy Xamarin.Mac, który zawiera wiele funkcji, będą zgodne z oczekiwaniami użytkownika można znaleźć w typowej aplikacji dla komputerów Mac zobacz [SourceWriter przykładową aplikację](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter jest proste źródła edytora kodu, który zapewnia obsługę uzupełniania kodu i wyróżniania składni proste.

Kod SourceWriter została ujęta w pełni i ile to możliwe, linki zostały podane z technologii kluczy lub metod do odpowiednich informacji w dokumentacji przewodniki platformy Xamarin.Mac.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano podstawowe informacje o standardowych aplikacji rozszerzenia Xamarin.Mac. Jego objętych usługą, utworzenie nowej aplikacji w programie Visual Studio dla komputerów Mac, projektowanie interfejsu użytkownika w środowisku Xcode i programu Interface Builder, udostępnianie elementów interfejsu użytkownika w języku C# kodu za pomocą **gniazd** i **akcje**, dodając kod do pracy z Elementy interfejsu użytkownika i na koniec, tworzenie i testowanie aplikacji platformy Xamarin.Mac.

## <a name="related-links"></a>Linki pokrewne

- [Witaj, Mac (przykład)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
