---
title: Witaj, Mac
description: Ten przewodnik przeprowadzi Cię przez kroki tworzenia pierwszej aplikacji Xamarin.Mac, a w procesie wprowadza łańcuch narzędzi rozwoju, w tym programu Visual Studio for Mac, Xcode i konstruktora interfejsu. Również wprowadza gniazda i akcji, które ujawnia kontrolek interfejsu użytkownika do kodu, a na koniec go ilustruje sposób tworzenia, uruchamianie i testowanie aplikacji Xamarin.Mac.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 37D0E9E6-979B-7069-B3BE-C5F0AF99BA72
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: e5d87d42765480c97da392cf07b6599108895321
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="hello-mac"></a>Witaj, Mac

Xamarin.Mac pozwala na projektowanie aplikacji całkowicie natywnych Mac w języku C# i platformy .NET przy użyciu tej samej biblioteki OS X i formantów interfejsu, które są używane podczas tworzenia w *Objective-C* i *Xcode*. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, deweloper może użyć w środowisku Xcode _konstruktora interfejsu_ utworzenie aplikacji interfejsy użytkownika (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Ponadto ponieważ Xamarin.Mac aplikacje są napisane w języku C# i .NET, typowy kod zaplecza można udostępniać aplikacje mobilne platformy Xamarin.iOS i Xamarin.Android; wszystkie dostarczając natywnym środowiskiem na każdej z platform.

W tym artykule przedstawiono podstawowe pojęcia, które są potrzebne do utworzenia przy użyciu Xamarin.Mac, programu Visual Studio for Mac i w środowisku Xcode konstruktora interfejsu przez krótki proces tworzenia prostej aplikacji Mac **Hello, Mac** aplikacji, które zlicza liczbę razy przycisk zostanie kliknięta:

[![](hello-mac-images/run02.png "Przykład Witaj, uruchomieniu aplikacji Mac")](hello-mac-images/run02.png#lightbox)

Zostały omówione następujące kwestie:

-  **Visual Studio for Mac** — wprowadzenie do programu Visual Studio for Mac oraz sposobu tworzenia aplikacji Xamarin.Mac z nim.
-  **Anatomia aplikacji Xamarin.Mac** — Xamarin.Mac co aplikacja składa się z.
-  **W środowisku Xcode interfejsu konstruktora** — sposób używania Xcode przez konstruktora interfejsu do definiowania interfejsu użytkownika aplikacji.
-  **Gniazda i akcje** — jak używać gniazda oraz akcji do okablować się formantów interfejsu użytkownika.
-  **Wdrożenie/testowanie** — jak uruchamianie i testowanie aplikacji Xamarin.Mac.


<a name="Requirements" />

## <a name="requirements"></a>Wymagania

Poniżej jest wymagany do opracowywania aplikacji macOS z Xamarin.Mac:

- Komputer Mac z systemem macOS Yosemite(10.10) lub nowszej.
- Xcode 7 i nowsze (jest to zalecane, aby zainstalować najnowsza stabilna wersja z [sklepu z aplikacjami](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).)
- Najnowszą wersję Xamarin.Mac i Visual Studio for Mac.

Uruchamianie aplikacji Mac utworzone za pomocą Xamarin.Mac mają następujące wymagania systemowe:

- Komputer Mac z systemem systemu Mac OS X 10,7 lub większą.

<a name="Starting_a_new_Xamarin.Mac_App_in_Xamarin_Studio" />

## <a name="starting-a-new-xamarinmac-app-in-visual-studio-for-mac"></a>Uruchamianie nowej aplikacji Xamarin.Mac w programie Visual Studio dla komputerów Mac

Jak już wspomniano, ten przewodnik przeprowadzi kroki umożliwiające utworzenie aplikacji Mac o nazwie `Hello_Mac` dodaje pojedynczy przycisków i etykiet do głównego okna. Po kliknięciu przycisku etykiety wyświetli liczbę razy, który zostanie kliknięta.

Aby rozpocząć pracę, wykonaj następujące czynności:

1. Uruchom program Visual Studio dla komputerów Mac:

    [![](hello-mac-images/setup01.png "Głównym programu Visual Studio for Mac interfejsu")](hello-mac-images/setup01.png#lightbox)

2. Polecenie **nowe rozwiązanie...**  łącze w lewy górny róg ekranu, aby otworzyć **nowy projekt** okno dialogowe:

    [![](hello-mac-images/setup03.png "Tworzenie nowego rozwiązania programu Visual Studio dla komputerów Mac")](hello-mac-images/setup02.png#lightbox)

3. Wybierz **Mac** > **aplikacji** > **aplikacji Cocoa** i kliknij przycisk **dalej** przycisk:

    [![](hello-mac-images/setup03.png "Wybieranie aplikacji Cocoa")](hello-mac-images/setup03.png#lightbox)

4. Wprowadź `Hello_Mac` dla **Nazwa aplikacji**i zachować wszystkie inne jako domyślny. Kliknij przycisk **dalej**:

    [![](hello-mac-images/setup05.png "Ustawienie nazwy aplikacji")](hello-mac-images/setup05.png#lightbox)

4. Podczas tworzenia rozwiązania, który będzie zawierać kilka różnych projektów, deweloper może być ustawienie innej **Nazwa rozwiązania** w tym miejscu, ale dla tego przykładu pozostaw ustawioną wartość domyślną jest taka sama jak  **Nazwa projektu**:

    [![](hello-mac-images/setup04.png "Weryfikowanie nowego szczegóły rozwiązania")](hello-mac-images/setup04.png#lightbox)

5. Kliknij przycisk **Utwórz** przycisku.

Visual Studio for Mac będzie Utwórz nową aplikację Xamarin.Mac i wyświetlić domyślne pliki, które zostaną dodane do rozwiązania aplikacji:

 [![](hello-mac-images/project01.png "Nowy widok domyślny rozwiązania")](hello-mac-images/project01.png#lightbox)

Programu Visual Studio for Mac używa **rozwiązań** i **projektów**, dokładnie tak samo, jak program Visual Studio nie. Rozwiązanie to kontener, który może zawierać jeden lub więcej projektów; projekty mogą zawierać aplikacje obsługujące bibliotek, testowanie aplikacji itp. W takim przypadku programu Visual Studio for Mac utworzył zarówno projekt aplikacji, jak i rozwiązanie automatycznie.

W razie potrzeby dewelopera można utworzyć co najmniej jeden kod biblioteki projektów, które zawierają kod wspólnego, udostępnionych. Te projekty biblioteki mogą być używane przez projekt aplikacji lub udostępniane innych projektów aplikacji Xamarin.Mac lub Xamarin.iOS i Xamarin.Android na podstawie typu kodu, w taki sam sposób jak w przypadku standardowej aplikacji .NET.

<a name="The_Project" />

## <a name="anatomy-of-a-xamarinmac-application"></a>Anatomia aplikacji Xamarin.Mac

Jeśli znasz iOS programowania, istnieje wiele podobieństw. W rzeczywistości iOS używa struktury CocoaTouch, która jest wersja slimmed rozwijanej Cocoa, używany przez Mac, więc będzie skrzyżowany partii koncepcji.

Spójrz na pliki w projekcie:

-   `Main.cs` — Zawiera główny punkt wejścia aplikacji. Gdy aplikacja jest uruchamiana, zawiera bardzo pierwszej klasy i metody, która jest uruchamiana.
-   `AppDelegate.cs` — Ten plik zawiera klasy głównym aplikacji, która jest odpowiedzialny za nasłuchuje zdarzeń z systemu operacyjnego.
-   `Info.plist` — Ten plik zawiera właściwości aplikacji, takie jak nazwa aplikacji, ikony itp.
-   `Entitlements.plist` — Pliki zawiera uprawnień dla aplikacji i umożliwia dostęp do elementów, takich jak obsługa Sandboxing i usługi iCloud.
-  `Main.storyboard` — Definiuje interfejs użytkownika (Windows i menu) dla aplikacji i układa połączenie między systemem Windows za pośrednictwem Segues. Scenorys to pliki XML, które zawierają definicji widoków (elementy interfejsu użytkownika). Ten plik można tworzyć i obsługiwanego przez konstruktora interfejsu wewnątrz Xcode.
-   `ViewController.cs` — Jest to kontroler dla głównego okna. Kontrolery zostanie omówiona szczegółowo w innym artykule, ale obecnie można traktować kontrolera aparatu głównym każdego określonego widoku.
-   `ViewController.designer.cs` — Ten plik zawiera kod żmudne procesy, które ułatwia integrowanie z interfejsem użytkownika ekranu głównego.

Poniższe sekcje potrwa szybko wyświetlić niektóre z tych plików. Później będzie można przedstawione szczegółowo, ale jest dobrym pomysłem jest teraz zrozumienie ich podstawy.

<a name="Main_cs" />

### <a name="maincs"></a>Main.CS

**Main.cs** plik jest bardzo proste. Zawiera on statycznego `Main` metodę, która tworzy nowe wystąpienie aplikacji Xamarin.Mac i przekazuje nazwę klasy, która będzie obsługiwać zdarzeń systemu operacyjnego, czyli w tym przypadku `AppDelegate` klasy:

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

<a name="AppDelegate_cs" />

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs` Plik zawiera `AppDelegate` klasy, która odpowiada za tworzenie okien i nasłuchuje zdarzeń systemu operacyjnego:

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

Ten kod jest prawdopodobnie nieznane, chyba że deweloper opracowała aplikację systemu iOS przed, ale jest dość proste.

`DidFinishLaunching` Metoda jest uruchamiana po utworzeniu wystąpienia aplikacji i jest odpowiedzialny za faktycznie tworzenia okna aplikacji i rozpoczyna proces wyświetlania widoku w nim.

`WillTerminate` Metoda zostanie wywołana po użytkownik lub system ma wystąpienia zamknięcia aplikacji. Dewelopera należy używać tej metody, aby zakończyć aplikację przed jej kończy działanie (na przykład zapisywania preferencji użytkownika lub rozmiaru okna i lokalizacji).

<a name="ViewController_cs" />

### <a name="viewcontrollercs"></a>ViewController.cs

Cocoa (i przez pochodnym, CocoaTouch) używa, co jest nazywane *Model View Controller* wzorzec (MVC). `ViewController` Deklaracji reprezentuje obiekt, który kontroluje okna rzeczywistej aplikacji. Ogólnie rzecz biorąc dla każdego okna utworzonego (i wiele innych zastosowań w systemie windows) znajduje się kontroler, który jest odpowiedzialny za cały cykl życia okna, takie jak wyświetlanie, dodawanie nowych widoków (formanty) do jego itp.

`ViewController` Klasa jest głównym oknie kontrolera. Oznacza to, że jest odpowiedzialny za cykl życiowy okna głównego. To spowoduje badane szczegółowo później, na wykonaj teraz krótki przegląd go:

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

<a name="ViewController_Designer_cs" />

### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Teraz pliku projektanta dla klasy okna głównego jest pusty, ale jego zostaną wypełnione automatycznie przez program Visual Studio dla komputerów Mac przy tworzeniu interfejsu użytkownika z interfejsu konstruktora wewnątrz Xcode:

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

Projektanta nie jest zazwyczaj zainteresowani plików projektanta, automatycznie są zarządzane przez program Visual Studio dla komputerów Mac i podaj kod wymagania żmudne procesy, który umożliwia dostęp do formantów, które zostały dodane do dowolnego okna lub widoku w aplikacji.

Z utworzonego projektu aplikacji Xamarin.Mac i podstawową wiedzę na temat składników Przełącz się do Xcode, można utworzyć interfejsu użytkownika przy użyciu narzędzia Konstruktor interfejsu.

<a name="Info_plist" />

### <a name="infoplist"></a>Info.plist

`Info.plist` Plik zawiera informacje o aplikacji Xamarin.Mac jego **nazwa** i **identyfikator pakietu**:

[![](hello-mac-images/infoplist01.png "Visual Studio for Mac plist edytora")](hello-mac-images/infoplist01.png#lightbox)

Definiuje również _scenorysu_ który będzie używany do wyświetlania interfejsu użytkownika dla aplikacji Xamarin.Mac w obszarze **interfejsu Main** listy rozwijanej. W przypadku powyższym przykładzie `Main` na liście rozwijanej odnosi się do `Main.storyboard` w drzewie źródła projektu w **Eksploratora rozwiązań**. Definiuje również ikon aplikacji, określając *katalogu zasobów* zawierający je (AppIcons w tym przypadku).

### <a name="entitlementsplist"></a>Entitlements.plist

Aplikacji `Entitlements.plist` plik steruje uprawnień, które aplikacja Xamarin.Mac ma takie jak **Sandboxing** i **iCloud**:

[![](hello-mac-images/entitlements01.png "Visual Studio for Mac uprawnień do edytora")](hello-mac-images/entitlements01.png#lightbox)

Na przykład Hello World uprawnień nie będzie wymagane. Następnej sekcji pokazano, jak edytować za pomocą konstruktora interfejsu w środowisku Xcode `Main.storyboard` plików i zdefiniuj aplikacji Xamarin.Mac interfejsu użytkownika.

<a name="Introduction_to_Xcode_and_Interface_Builder" />

## <a name="introduction-to-xcode-and-interface-builder"></a>Wprowadzenie do programów Xcode i konstruktora interfejsu

W ramach Xcode Apple utworzył narzędzie kompilatora interfejsu, który umożliwia deweloperom tworzenie interfejsu użytkownika wizualnie w projektancie. Xamarin.Mac fluently integruje się z konstruktora interfejsu, dzięki czemu interfejsu użytkownika są tworzone z tych samych narzędzi jako użytkowników języka Objective-C.

Aby rozpocząć, kliknij dwukrotnie `Main.storyboard` w pliku **Eksploratora rozwiązań** go otworzyć do edycji w programie Xcode i kompilatora interfejsu:

[![](hello-mac-images/xcode01.png "Plik Main.storyboard w Eksploratorze rozwiązań")](hello-mac-images/xcode01.png#lightbox)

To należy uruchomić Xcode i wyglądać jak poniżej:

[![](hello-mac-images/xcode02.png "Widok domyślny konstruktor interfejsu Xcode")](hello-mac-images/xcode02.png#lightbox)

Przed rozpoczęciem projektowania interfejsu należy podjąć szybki przegląd Xcode, aby z najważniejszych funkcji, które będą używane.

> [!NOTE]
> Deweloper nie ma używać Xcode i interfejs Konstruktor do tworzenia interfejsu użytkownika dla aplikacji Xamarin.Mac, interfejsu użytkownika można tworzyć bezpośrednio w kodzie języka C#, ale która wykracza poza zakres tego artykułu. Dla uproszczenia, należy go będzie używać konstruktora interfejs do tworzenia interfejsu użytkownika w dalszej części tego samouczka.


<a name="Components_of_Xcode" />

### <a name="components-of-xcode"></a>Składniki Xcode

Podczas otwierania `.storyboard` plików w środowisku Xcode z programu Visual Studio dla komputerów Mac, zostanie ona otwarta z **Nawigatora projektu** po lewej stronie, **hierarchii interfejsów** i **Edytor interfejsu**w środku, a **właściwości i narzędzia** sekcji po prawej stronie:

[![](hello-mac-images/xcode03.png "Różnych sekcji kompilatora interfejsu w środowisku Xcode")](hello-mac-images/xcode03.png#lightbox)

Poniższe sekcje Przyjrzyjmy się funkcji każdego z tych Xcode funkcji i sposobie ich używać do tworzenia interfejsu aplikacji Xamarin.Mac.

<a name="Project_Navigation" />

### <a name="project-navigation"></a>Projekt nawigacji

Podczas otwierania `.storyboard` pliku do edycji w programie Xcode, Visual Studio dla komputerów Mac tworzy *pliku projektu Xcode* w tle informacji o zmianach między sobą i Xcode. Później po dewelopera zmienia do programu Visual Studio dla komputerów Mac w programie Xcode, zmiany wprowadzone w tym projekcie są synchronizowane z projektu Xamarin.Mac przez program Visual Studio dla komputerów Mac.

**Nawigacji projektu** sekcja umożliwia deweloperowi przechodzić między wszystkie pliki wchodzące w skład to _podkładki_ projektu Xcode. Zwykle, będą oni tylko zainteresowana `.storyboard` , takich jak pliki na tej liście `Main.storyboard`.

<a name="Interface_Hierarchy" />

### <a name="interface-hierarchy"></a>Interfejs hierarchii

**Hierarchii interfejsów** sekcja umożliwia deweloperowi łatwo uzyskiwać dostęp do wielu właściwości klucza interfejsu użytkownika takich jak jej **symbole zastępcze** i głównym **okna**. W tej sekcji służy do uzyskania dostępu do poszczególnych elementów (widoki), wchodzące w skład interfejsu użytkownika i dostosowanie sposobu są zagnieżdżone, przeciągając je w hierarchii.

<a name="Interface_Editor" />

### <a name="interface-editor"></a>Interfejs edytora

**Edytor interfejsu** sekcja zawiera obszar, w którym interfejs użytkownika jest graficznie rozmieszczona. Przeciągnij elementy z **biblioteki** sekcji **właściwości i narzędzia** sekcji, aby utworzyć projekt. Gdy elementy interfejsu użytkownika (widoki) są dodawane do powierzchni projektowej, będzie można dodać do **hierarchii interfejsów** sekcji w kolejności, w jakiej występują w **Edytor interfejsu**.

<a name="Properties_Utilities" />

### <a name="properties--utilities"></a>Właściwości i narzędzia

**Właściwości i narzędzia** sekcja jest podzielona na dwie sekcje główne, **właściwości** (nazywanych również inspektorzy) i **biblioteki**:

[![](hello-mac-images/xcode04.png "Inspektora właściwości")](hello-mac-images/xcode04.png#lightbox)

Początkowo w tej sekcji jest prawie pusta, ale jeśli Deweloper wybiera element **Edytor interfejsu** lub **hierarchii interfejsów**, **właściwości** sekcja będzie wypełnione informacjami o danego elementu i właściwości, które można zmienić.

W ramach **właściwości** sekcji, istnieją ośmiu różnych *karty inspektora*, jak pokazano na poniższej ilustracji:

[![](hello-mac-images/xcode05.png "Przegląd wszystkich inspektorzy")](hello-mac-images/xcode05.png#lightbox)

<a name="Properties_Utility_Types" />

### <a name="properties--utility-types"></a>Narzędzie typy & właściwości

Od lewej do prawej są następujące karty:

-   **Plik inspektora** — Inspector plików zawiera informacje o pliku, takie jak nazwa pliku i lokalizację pliku Xib, który jest edytowany.
-   **Szybką pomoc** — karta szybki pomocy zawiera pomocy kontekstowej oparte na wybranej w środowisku Xcode.
-   **Tożsamość inspektora** — inspektora tożsamości zawiera informacje o wybrany formant/widok.
-   **Atrybuty inspektora** — atrybuty inspektora umożliwia deweloperowi dostosować różne atrybuty wybrany formant/widok.
-   **Rozmiar inspektora** — rozmiar inspektora umożliwia deweloperowi kontrolować rozmiar i zmienianie rozmiaru zachowanie kontroli/widok.
-   **Połączenia inspektora** — przedstawia inspektora połączeń **gniazda** i **akcji** połączeń formanty. Gniazda i akcji zostanie dokładnie omówione szczegółowo poniżej.
-   **Powiązania inspektora** — inspektora powiązania umożliwia deweloperowi umożliwiają konfigurowanie kontrolek, tak aby ich wartości są automatycznie powiązane z modelami danych.
-   **Wyświetl inspektora efekty** — inspektora efekty widoku umożliwia deweloperowi określić efekty w formantach, takich jak animacji.

Użyj **biblioteki** sekcji, aby znaleźć kontrolek i obiekty, które można umieścić w projektancie do tworzenia graficznego interfejsu użytkownika:

[![](hello-mac-images/xcode06.png "Inspektor biblioteki Xcode")](hello-mac-images/xcode06.png#lightbox)

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Tworzenie interfejsu

Podstawy Xcode IDE i objęte konstruktora interfejsu dewelopera umożliwia tworzenie interfejsu użytkownika dla widoku głównego.

Wykonaj następujące czynności:

1. W programie Xcode, przeciągnij **przycisk** z **biblioteki**:

    [![](hello-mac-images/xcode07.png "Wybieranie NSButton inspektora biblioteki")](hello-mac-images/xcode07.png#lightbox)

2. Upuść ten przycisk na **widoku** (w obszarze **kontrolera okna**) w **Edytor interfejsu**:

    [![](hello-mac-images/xcode08.png "Dodawanie przycisku do projektu interfejsu")](hello-mac-images/xcode08.png#lightbox)

3. Polecenie **tytuł** właściwości w **inspektora atrybutu** i Zmień tytuł przycisku do `Click Me`:

    [![](hello-mac-images/xcode09.png "Ustawianie właściwości przycisku")](hello-mac-images/xcode09.png#lightbox)

4. Przeciągnij **etykiety** z **biblioteki**:

    [![](hello-mac-images/xcode10.png "Wybranie etykiety z Inspektora biblioteki")](hello-mac-images/xcode10.png#lightbox)

5. Upuść etykiety na **okna** obok przycisku w **Edytor interfejsu**:

    [![](hello-mac-images/xcode11.png "Dodawanie etykiet do projektu interfejsu")](hello-mac-images/xcode11.png#lightbox)

6. Wystarczy pobrać uchwytu na etykiecie po prawej stronie, a następnie przeciągnij go do czasu jego pobliżu krawędzi okna:

    [![](hello-mac-images/xcode12.png "Zmiana rozmiaru etykiety")](hello-mac-images/xcode12.png#lightbox)

7. Wybierz przycisk dodane w **Edytor interfejsu**i kliknij przycisk **Edytor ograniczenia** ikonę i u dołu okna:

    [![](hello-mac-images/xcode13.png "Dodawanie ograniczeń do przycisku")](hello-mac-images/xcode13.png#lightbox)

8. W górnej części edytora, kliknij przycisk **czerwony I świateł** w górnej i lewej strony. Ponieważ zmiany rozmiaru okna, pozwoli to zachować przycisku w tej samej lokalizacji, w lewym górnym rogu ekranu.

9. Następnie sprawdź **wysokość** i **szerokość** pola i użyj rozmiarów domyślne. Dzięki temu przycisku w taki sam rozmiar, gdy zmienia rozmiar okna.

10. Kliknij przycisk **dodać ograniczenia 4** przycisk, aby dodać ograniczenia i zamknij Edytor.

11. Wybierz etykietę, a następnie kliknij przycisk **Edytor ograniczenia** ikonę ponownie:

    [![](hello-mac-images/xcode14.png "Dodawanie ograniczeń do etykiety")](hello-mac-images/xcode14.png#lightbox)

12. Klikając **czerwony I świateł** u góry, prawej i lewej z **Edytor ograniczenia**, określa, że etykieta zostać zablokowana do danego X i Y lokalizacji, a także zwiększyć lub zmniejszyć, ponieważ zmieniono rozmiar okna w działaniu aplikacja.

13. Ponownie sprawdź **wysokość** polu i użyj domyślny rozmiar, a następnie kliknij przycisk **dodać ograniczenia 4** przycisk, aby dodać ograniczenia i zamknij Edytor.

14. Zapisać zmiany w interfejsie użytkownika.

Podczas zmiany rozmiaru i przenoszenia kontrolek wokół, zwróć uwagę, że interfejs konstruktora zapewnia wskazówki przydatne przystawki, oparte na [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Te wskazówki pomogą deweloperów do tworzenia wysokiej jakości aplikacji, które będą miały znanych wyglądu i działania dla użytkowników komputerów Mac.

Szukaj w **hierarchii interfejsów** sekcji jak układ i hierarchię elementów, które składają się na interfejsie użytkownika są wyświetlane:

[![](hello-mac-images/xcode15.png "Zaznaczenie elementu w hierarchii interfejsów")](hello-mac-images/xcode15.png#lightbox)

W tym miejscu deweloper może wybierz elementy do edycji lub przeciągnij, aby zmienić kolejność elementów interfejsu użytkownika, jeśli to konieczne. Na przykład jeśli element interfejsu użytkownika są objęte przez inny element, można go przeciągania w dół na liście, aby stał się najwyżej elementu w oknie.

Przy użyciu interfejsu użytkownika utworzone deweloper musi ujawniać elementów interfejsu użytkownika, dzięki czemu Xamarin.Mac mogą uzyskać dostęp i interakcji z użytkownikiem w kodzie języka C#. Następna sekcja **gniazda i akcje**, pokazuje, jak to zrobić.

<a name="Outlets_and_Actions" />

### <a name="outlets-and-actions"></a>Gniazda i akcji

Co to są **gniazda** i **akcje**? W tradycyjnych programowania interfejsu użytkownika platformy .NET, formantu w interfejsie użytkownika jest automatycznie widoczne jako właściwość, gdy jest ona dodawana. Elementy działają inaczej w Mac, po prostu Dodawanie formantu do widoku nie ona łatwiej dostępna dla kodu. Deweloper musi ujawniać jawnie elementu interfejsu użytkownika do kodu. W kolejności to zrobić, Apple są dostępne dwie opcje:

-   **Gniazda** — gniazda są podobne do właściwości. Jeśli dewelopera tworzącej formantu do gniazda, jest widoczne w kodzie za pomocą właściwości, więc mogą je rzeczy, takich jak dołączanie procedury obsługi zdarzeń, wywoływać metod w jego itp.
-   **Akcje** — akcje są analogiczne do polecenia wzorzec na platformie WPF. Na przykład jeśli akcja jest wykonywana na formancie, powiedz kliknij przycisk, formantu zostanie automatycznie wywołania metody w kodzie. Akcje są wydajne i wygodne, ponieważ deweloper może okablować się wiele formantów do tego samego działania.

W środowisku Xcode **gniazda** i **akcje** zostaną dodane bezpośrednio w kodzie za pomocą *przeciąganie kontroli*. Oznacza to, że można utworzyć bardziej precyzyjnego **gniazda** lub **akcji**, deweloper wybierze element sterowania, aby dodać **gniazda** lub **akcji** do, naciśnij i przytrzymaj **kontroli** klucza na klawiaturze, a następnie przeciągnij formant bezpośrednio w kodzie.

Dla deweloperów Xamarin.Mac, oznacza to, że deweloper będzie przeciągnij do pliki szczątkowe Objective-C, które odpowiadają w pliku C# których chce utworzyć **gniazda** lub **akcji**. Visual Studio for Mac utworzony plik o nazwie `ViewController.h` jako część podkładki projektu Xcode wygenerowane za pomocą konstruktora interfejsu:

[![](hello-mac-images/xcode16.png "Wyświetlanie źródła w środowisku Xcode")](hello-mac-images/xcode16.png#lightbox)

To skrótowa `.h` pliku wstecznych `ViewController.designer.cs` jest automatycznie dodawany do projektu Xamarin.Mac podczas tworzenia nowego `NSWindow` jest tworzony. Ten plik będzie używane do synchronizowania zmiany wprowadzone przez konstruktora interfejsu i jest where **gniazda** i **akcje** są tworzone, tak aby elementy interfejsu użytkownika są widoczne dla kodu C#.

<a name="Adding_an_Outlet" />

#### <a name="adding-an-outlet"></a>Dodawanie gniazda

Z podstawową wiedzę na temat tego, jaki **gniazda** i **akcje** Utwórz **gniazda** do udostępnienia Etykieta utworzona do naszego kodu C#.

Wykonaj następujące czynności:

1. W środowisku Xcode w prawej top górny róg ekranu, kliknij przycisk **dwukrotnie okrąg** przycisk, aby otworzyć **Edytor Asystenta**:

    [![](hello-mac-images/outlet01.png "Wyświetlanie Edytor Asystenta")](hello-mac-images/outlet01.png#lightbox)

2. Xcode nastąpi przełączenie do trybu widok podzielony z **Edytor interfejsu** po jednej stronie i **edytora kodu** z drugiej strony.

3. Należy zauważyć, że środowisko Xcode jest pobierany automatycznie **ViewController.m** w pliku **edytora kodu**, która jest nieprawidłowa. Z dyskusji co **gniazda** i **akcje** są powyżej, deweloper musi mieć **ViewController.h** wybrane.

4. W górnej części **edytora kodu** kliknij **łącze automatyczne** i wybierz `ViewController.h` pliku:

    [![](hello-mac-images/outlet02.png "Wybranie poprawnego pliku")](hello-mac-images/outlet02.png#lightbox)

5. Xcode teraz powinny mieć poprawny plik wybrane:

    [![](hello-mac-images/outlet03.png "Wyświetlanie pliku ViewController.h")](hello-mac-images/outlet03.png#lightbox)

6. **Ostatnim krokiem było bardzo ważne!** Jeśli projektanta nie ma poprawnego pliku wybrane, nie będzie mógł tworzyć **gniazda** i **akcje** lub mają być widoczne do niewłaściwej klasy w języku C#!

7. W **Edytor interfejsu**, naciśnij i przytrzymaj **kontroli** klucza na klawiaturze i kliknij i przeciągnij etykiety utworzone powyżej do edytora kodu właśnie poniżej `@interface ViewController : NSViewController {}` kodu:

    [![](hello-mac-images/outlet04.png "Przeciąganie w celu utworzenia gniazda")](hello-mac-images/outlet04.png#lightbox)

8. Zostanie wyświetlone okno dialogowe. Pozostaw **połączenia** ustawioną **gniazda** , a następnie wprowadź `ClickedLabel` dla **nazwa**:

    [![](hello-mac-images/outlet05.png "Definiowanie gniazda")](hello-mac-images/outlet05.png#lightbox)

9. Kliknij przycisk **Connect** przycisk, aby utworzyć **gniazda**:

    [![](hello-mac-images/outlet06.png "Wyświetlanie końcowy gniazda")](hello-mac-images/outlet06.png#lightbox)

10. Zapisz zmiany w pliku.

<a name="Adding_an_Action" />

#### <a name="adding-an-action"></a>Dodawanie operacji

Następnie ujawnić przycisk, aby kod w języku C#. Podobnie jak powyżej etykiety deweloper może okablować przycisku do **gniazda**. Ponieważ chcemy odpowiedzieć na przycisk zostanie kliknięty, użyj **akcji** zamiast tego.

Wykonaj następujące czynności:

1. Upewnij się, że środowisko Xcode jest nadal w **Edytor Asystenta** i **ViewController.h** plik jest widoczny w **edytora kodu**.
2. W **Edytor interfejsu**, naciśnij i przytrzymaj **kontroli** klucza na klawiaturze i kliknij i przeciągnij przycisk utworzone powyżej do edytora kodu właśnie poniżej `@property (assign) IBOutlet NSTextField *ClickedLabel;` kodu:

    [![](hello-mac-images/action01.png "Przeciąganie w celu utworzenia akcji")](hello-mac-images/action01.png#lightbox)

3. Zmień **połączenia** typ **akcji**:

    [![](hello-mac-images/action02.png "Definiowanie akcji")](hello-mac-images/action02.png#lightbox)

4. Wprowadź `ClickedButton` jako **nazwa**:

    [![](hello-mac-images/action03.png "Nowa akcja nazewnictwa")](hello-mac-images/action03.png#lightbox)

5. Kliknij przycisk **Connect** przycisk, aby utworzyć **akcji**:

    [![](hello-mac-images/action04.png "Wyświetlanie ostateczne działania")](hello-mac-images/action04.png#lightbox)

6. Zapisz zmiany w pliku.

Przy użyciu interfejsu użytkownika przewodowej w pionie i ujawniony dla kodu C# przełączyć się do programu Visual Studio dla komputerów Mac i pozwól mu zsynchronizować zmiany wprowadzone w programie Xcode i kompilatora interfejsu.

> [!NOTE]
> Prawdopodobnie zajęło dużo czasu, można utworzyć interfejsu użytkownika i **gniazda** i **akcje** to pierwszy aplikacji która może się wydawać dużo pracy, ale wprowadzono wiele nowych pojęć i mnóstwo czasu był poświęcony na obejmujące nowe podstaw. Po ćwiczenia przez pewien czas i pracy z konstruktora interfejsu, tego interfejsu i wszystkie jego **gniazda** i **akcje** można tworzyć w tylko minutę lub dwie.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Synchronizowanie zmian z Xcode

Kiedy dewelopera przełącza do programu Visual Studio dla komputerów Mac w programie Xcode, wszystkie zmiany wprowadzone w programie Xcode automatycznie zostaną zsynchronizowane z projektu Xamarin.Mac.

Wybierz **ViewController.designer.cs** w **Eksploratora rozwiązań** aby zobaczyć, jak **gniazda** i **akcji** zostały przewodowej się w języku C# Kod:

[![](hello-mac-images/sync01.png "Synchronizowanie zmian z Xcode")](hello-mac-images/sync01.png#lightbox)

Powiadomienie jak dwie definicje w **ViewController.designer.cs** pliku:

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

Visual Studio for Mac wykrywa zmiany **.h** pliku, a następnie automatycznie synchronizuje te zmiany w odpowiednich **. Designer.cs narzędzie** pliku do udostępnienia ich do aplikacji. Zwróć uwagę, że **ViewController.designer.cs** jest częściowej klasy, dzięki czemu nie ma programu Visual Studio for Mac, można zmodyfikować **ViewController.cs** który zastąpiłaby wszelkie zmiany wprowadzone do projektanta Klasa.

Zwykle deweloper nigdy nie będzie można otworzyć **ViewController.designer.cs**, jego został przedstawiony tutaj wyłącznie w celach edukacyjnych.

> [!NOTE]
> W większości przypadków programu Visual Studio for Mac automatycznie Zobacz wszystkie zmiany wprowadzone w programie Xcode i zsynchronizować je do projektu Xamarin.Mac. W wystąpieniu wyłączenia synchronizacji nie jest realizowane automatycznie wrócić do Xcode, a następnie z powrotem do programu Visual Studio dla komputerów Mac. Zwykle będzie to rozpocząć poza cyklu synchronizacji.

<a name="Writing_the_Code" />

## <a name="writing-the-code"></a>Pisanie kodu

Interfejs użytkownika utworzone i jego elementów interfejsu użytkownika do kodu za pomocą **gniazda** i **akcje**, możemy finally już przystąpić do pisania kodu można wyświetlić program do życia.

Dla tej aplikacji przykładowej za każdym razem, gdy po kliknięciu przycisku pierwszej etykiety zostaną zaktualizowane do wyświetlenia, ile razy przycisk został kliknięty. W tym celu otwórz `ViewController.cs` plik do edycji przez dwukrotne kliknięcie w **Eksploratora rozwiązań**:

[![](hello-mac-images/code01.png "Wyświetlanie pliku ViewController.cs w programie Visual Studio dla komputerów Mac")](hello-mac-images/code01.png#lightbox)

Najpierw Utwórz zmienną poziomie klasy w `ViewController` klasę, aby śledzić liczbę kliknięć, które miały miejsce. Edytowanie definicji klasy i zapewnić ich wyglądać następująco:

```csharp
namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Dalej w tej samej klasie (`ViewController`), Zastąp `ViewDidLoad` — metoda i dodać niektóre kod, aby ustawić pierwszy komunikat dla etykiety:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Użyj `ViewDidLoad`, zamiast innej metody, takie jak `Initialize`, ponieważ `ViewDidLoad` jest nazywany *po* system operacyjny został załadowany i utworzyć wystąpienia interfejsu użytkownika z **.storyboard** pliku. Jeśli dewelopera próbował uzyskać dostęp formantu etykiety przed **.storyboard** pliku została całkowicie załadowany i wystąpienia, zostałyby `NullReferenceException` błąd ponieważ formantu etykiety nie będą jeszcze istnieje.

Następnie dodaj kod, który odpowiada na użytkownika, klikając przycisk. Dodaj następujące metody częściowej do `ViewController` klasy:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Ten kod dołącza do **akcji** utworzone w programie Xcode i kompilatora interfejsu i zostanie wywołana w dowolnym momencie użytkownik kliknie przycisk.

<a name="Testing_the_Application" />

## <a name="testing-the-application"></a>Testowanie aplikacji

Nadszedł czas, aby skompilować i uruchomić aplikację, aby upewnić się, że działa zgodnie z oczekiwaniami. Deweloper może skompilować i uruchomić w jednym kroku, lub można go tworzyć bez konieczności uruchamiania ich.

Zawsze, gdy aplikacja jest wbudowana, deweloper można wybrać, jaki rodzaj kompilacji mają:

-   **Debugowanie** — Kompilacja debugowania ma zostać skompilowany w **.app** pliku (aplikacja) z licznych dodatkowe metadane, który umożliwia deweloperom debugowania, co dzieje, gdy aplikacja jest uruchomiona.
-   **Zwolnij** — tworzy także kompilacji wydania **.app** pliku, ale nie zawiera informacji o debugowaniu, więc jest mniejsze i wykonuje szybciej.

Deweloper można wybrać typ kompilacji z **selektora konfiguracji** w górnym lewym dolnym rogu programu Visual Studio for Mac ekranu:

[![](hello-mac-images/run01.png "Zaznaczanie kompilacji debugowania")](hello-mac-images/run01.png#lightbox)

<a name="Building_the_Application" />

## <a name="building-the-application"></a>Kompilowanie aplikacji

W tym przykładzie mamy po prostu chcesz kompilację debugowania, dlatego upewnij się, że **debugowania** jest zaznaczone. Tworzenie pierwszej kompilacji aplikacji przez albo naciskając klawisz **⌘B**, lub z **kompilacji** menu, wybierz **kompilacji wszystkich**.

Jeśli nie odnaleziono żadnych błędów, **kompilacji zakończyło się pomyślnie** zostanie wyświetlony komunikat w programie Visual Studio dla komputerów Mac w pasku stanu. Jeśli wystąpiły błędy, przejrzyj projekt i upewnij się, że powyższe kroki zostały prawidłowo wykonane. Rozpocznij od potwierdzenie, że kod, (zarówno w środowisku Xcode, jak i w programie Visual Studio dla komputerów Mac) zgodny z kodem w samouczku.

<a name="Running_the_Application" />

## <a name="running-the-application"></a>Uruchamianie aplikacji

Istnieją trzy sposoby, aby uruchomić aplikację:

-  Naciśnij klawisz **⌘ + Enter**.
-  Z **Uruchom** menu, wybierz **debugowania**.
-  Kliknij przycisk **odtwarzanie** przycisk w Visual Studio for Mac paska narzędzi (tylko powyżej **Eksploratora rozwiązań**).

Aplikacja kompilacji (Jeśli nie został skompilowany już), uruchomi w trybie debugowania i wyświetl jego okno główne interfejsu:

[![](hello-mac-images/run02.png "Uruchamianie aplikacji")](hello-mac-images/run02.png#lightbox)

Jeśli przycisk zostanie kliknięty kilka razy, etykiety powinny zostać zaktualizowane licznika:

[![](hello-mac-images/run03.png "Wyświetlanie wyników kliknięcie przycisku")](hello-mac-images/run03.png#lightbox)

<a name="Where_to_Next" />

## <a name="where-to-next"></a>Gdzie można dalej

Podstawy pracy z aplikacją Xamarin.Mac dół Spójrz na poniższe dokumenty, aby uzyskać więcej informacji:

- [Wprowadzenie do scenorysu](~/mac/platform/storyboards/index.md) — ten artykuł zawiera wprowadzenie do pracy z Scenorys w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i obsługę przy użyciu scenorys i kompilatora interfejsu Xcode w Interfejsie użytkownika aplikacji.
- [Windows](~/mac/user-interface/window.md) — w tym artykule omówiono pracy z systemem Windows i panele w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i obsługę systemu Windows i panele w programie Xcode i interfejsu builder ładowania systemu Windows i panele z plików .xib przy użyciu systemu Windows i odpowiada do systemu Windows w kodzie języka C#.
- [Okna dialogowe](~/mac/user-interface/dialog.md) — w tym artykule omówiono pracy z okien dialogowych i okna modalne w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i obsługę okna modalne Xcode i interfejsu konstruktora, Praca z standardowych oknach dialogowych, wyświetlanie i odpowiada do systemu Windows w kodzie języka C#.
- [Alerty](~/mac/user-interface/alert.md) — w tym artykule omówiono Praca z alertami w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i wyświetlanie alertów z kodu C# oraz reagowanie na alerty.
- [Menu](~/mac/user-interface/menu.md) -menu są używane w różnych części w interfejsie użytkownika aplikacji Mac; z menu głównego aplikacji, w górnej części ekranu w menu podręcznego i kontekstowe, które mogą występować w dowolnym miejscu w oknie. Menu są integralną częścią aplikacji Mac środowisko użytkownika. W tym artykule omówiono Praca z menu Cocoa w aplikacji Xamarin.Mac.
- [Paski narzędzi](~/mac/user-interface/toolbar.md) — w tym artykule omówiono pracy z paski narzędzi w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę paski narzędzi w Konstruktorze Xcode i interfejsu, jak udostępnić elementów paska narzędzi do kodu za pomocą akcji i gniazda, włączanie i wyłączanie elementów paska narzędzi oraz ostatecznie odpowiada na żądania elementów paska narzędzi w kodzie języka C#.
- [Tabela widoków](~/mac/user-interface/table-view.md) — w tym artykule omówiono pracy z widoków tabel w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę widoków tabel w Konstruktorze Xcode i interfejsu, jak udostępnianie tabeli wyświetlanie elementów do kodu za pomocą akcji i gniazda, podczas wypełniania tabeli elementów i ostatecznie odpowiada na tabeli Wyświetl elementy w kodzie języka C#.
- [Konspekt widoków](~/mac/user-interface/outline-view.md) — w tym artykule omówiono pracy z widokami konspektu w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę konspektu widoków w programie Xcode i interfejsu builder, jak udostępnianie wyświetlanie konspektu elementów do kodu za pomocą akcji i gniazda, podczas wypełniania elementy konspektu i finally odpowiada na żądania konspektu Wyświetl elementy w kodzie języka C#.
- [Źródło listy](~/mac/user-interface/source-list.md) — w tym artykule omówiono pracy z listami źródła w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę wymieniono źródła w środowisku Xcode i interfejsu konstruktora, jak udostępnianie źródło zawiera listę elementów do kodu za pomocą akcji i gniazda, wypełnianie źródła elementów listy i na koniec reagowanie na elementy listy źródeł w kodzie języka C#.
- [Kolekcja widoków](~/mac/user-interface/collection-view.md) — w tym artykule omówiono pracy z widokiem kolekcji w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługę widoki kolekcji w programie Xcode i interfejsu builder, jak udostępnianie elementów widok kolekcji do kodu przy użyciu akcji i gniazda, podczas wypełniania widoków kolekcji i na koniec reagowanie na widoki kolekcji w kodzie języka C#.
- [Praca z obrazami](~/mac/app-fundamentals/image.md) — w tym artykule omówiono pracy z obrazów i ikon w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługa obrazów potrzebne do utworzenia aplikacji ikony, jak i używanie obrazów w kodzie C# i kompilatora interfejsu w środowisku Xcode.

Również sugerować biorąc przyjrzeć się [galerii przykładów Mac](https://developer.xamarin.com/samples/mac/all/), zawiera wiele gotowych do użycia kodu, który może pomóc developer szybko uzyskać projektu Xamarin.Mac poza podstaw.

Na przykład Pełna aplikacja Xamarin.Mac, który zawiera wiele funkcji oczekiwać użytkownika można znaleźć w typowej aplikacji dla komputerów Mac, zobacz [SourceWriter Przykładowa aplikacja](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter jest proste źródła edytora kodu, który zapewnia obsługę uzupełniania kodu i wyróżnianie składni proste.

Pełni komentarzem kodu SourceWriter i, jeśli jest dostępna, zostały podane linki z technologii kluczy lub metod do odpowiednich informacji w dokumentacji przewodniki Xamarin.Mac.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano podstawy standardowych aplikacji Xamarin.Mac. Go pasuje do tworzenia nowej aplikacji w programie Visual Studio dla komputerów Mac, projektowanie interfejsu użytkownika w środowisku Xcode i kompilatora interfejsu, udostępnianie elementy interfejsu użytkownika do języka C# kodu za pomocą **gniazda** i **akcje**, dodawanie kodu do pracy z Elementy interfejsu użytkownika, a na koniec, tworzenia i testowania aplikacji Xamarin.Mac.

## <a name="related-links"></a>Linki pokrewne

- [Witaj, Mac (przykład)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
