---
title: Witaj, systemu tvOS Przewodnik Szybki Start
description: Ten przewodnik przeprowadzi Cię przez proces tworzenia pierwszej aplikacji Xamarin.tvOS i jego rozwoju łańcuch narzędzi. Podaj Xamarin Designer, który udostępnia kontrolek interfejsu użytkownika do kodu i ilustruje sposób tworzenia, uruchamianie i testowanie aplikacji Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/02/2018
ms.openlocfilehash: 0adf6e326dd29db15b6bd90626f424b803dc0bc9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="hello-tvos-quick-start-guide"></a>Witaj, systemu tvOS Przewodnik Szybki Start

_Ten przewodnik przeprowadzi Cię przez proces tworzenia pierwszej aplikacji Xamarin.tvOS i jego rozwoju łańcuch narzędzi. Podaj Xamarin Designer, który udostępnia kontrolek interfejsu użytkownika do kodu i ilustruje sposób tworzenia, uruchamianie i testowanie aplikacji Xamarin.tvOS._

Apple wydała 5. Generowanie Apple TV, Apple TV 4K, w którym jest uruchomiona systemu tvOS 11.

Platforma Apple TV jest otwarty dla deweloperów, dzięki czemu mogą tworzyć aplikacje rozbudowane, bez ramek i zwolnij za pośrednictwem sklepu z aplikacjami wbudowanych telewizora firmy Apple.

Jeśli znasz rozwoju platformy Xamarin.iOS powinien znajdować się proces przechodzenia do systemu tvOS dość proste. Większość funkcji i interfejsy API są takie same, jednak wiele wspólnych interfejsów API są niedostępne (na przykład WebKit). Ponadto Praca z za pomocą zdalnego Siri stanowi niektóre wyzwania projektu, które nie są obecne w ekran dotykowy na podstawie urządzeń z systemem iOS.

W tym przewodniku zapewni wprowadzenie do pracy z systemu tvOS w aplikacji platformy Xamarin. Aby uzyskać więcej informacji dotyczących systemu tvOS, zobacz firmy Apple [przygotowania dla Apple TV 4K](https://developer.apple.com/tvos/) dokumentacji.

## <a name="overview"></a>Omówienie

Xamarin.tvOS umożliwia tworzenie aplikacji całkowicie natywnych Apple TV w języku C# i platformy .NET przy użyciu tej samej biblioteki OS X i formantów interfejsu, które są używane podczas tworzenia w *Swift* (lub *Objective-C*) i *Xcode*.

Ponadto ponieważ Xamarin.tvOS aplikacje są napisane w języku C# i .NET, typowe, kod zaplecza można udostępniać aplikacji platformy Xamarin.iOS, Xamarin.Android i Xamarin.Mac; wszystkie dostarczając natywnym środowiskiem na każdej z platform.

W tym artykule przedstawiono podstawowe pojęcia, które są potrzebne do utworzenia aplikacji TV firmy Apple, używając Instruktaż proces tworzenia podstawowego Xamarin.tvOS i Visual Studio **Hello, systemu tvOS** aplikacji, które zlicza liczbę razy przycisk ma kliknięta:

[![](hello-tvos-images/run05.png "Przykładową aplikację, uruchom")](hello-tvos-images/run05.png#lightbox)

Następujące kwestie omówione zostaną następujące czynności:

-  **Visual Studio for Mac** — wprowadzenie do programu Visual Studio for Mac oraz sposobu tworzenia aplikacji Xamarin.tvOS z nim.
-  **Anatomia aplikacji Xamarin.tvOS** — Xamarin.tvOS co aplikacja składa się z.
-  **Tworzenie interfejsu użytkownika** — jak używać do projektanta Xamarin dla systemu iOS do tworzenia interfejsu użytkownika.
-  **Wdrażanie i testowanie** — jak uruchamianie i testowanie aplikacji w symulatorze systemu tvOS i na sprzęcie systemu tvOS prawdziwe.

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Uruchamianie nowej aplikacji Xamarin.tvOS w programie Visual Studio dla komputerów Mac

Jak już wspomniano, utworzymy aplikację Apple TV, nazywany `Hello-tvOS` dodaje pojedynczy przycisków i etykiet do ekranu głównego. Po kliknięciu przycisku etykiety wyświetli liczbę razy, który zostanie kliknięta.

Aby rozpocząć, teraz wykonać następujące czynności:

1. Uruchom program Visual Studio dla komputerów Mac:

    [![](hello-tvos-images/setup01.png "Visual Studio for Mac")](hello-tvos-images/setup01.png#lightbox)
2. Polecenie **nowe rozwiązanie...**  łącze w lewy górny róg ekranu, aby otworzyć **nowy projekt** okno dialogowe.
3. Wybierz **systemu tvOS** > **aplikacji** > **jednej aplikacji widoku** i kliknij przycisk **dalej** przycisk:

    [![](hello-tvos-images/setup02.png "Wybierz jednego widoku aplikacji")](hello-tvos-images/setup02.png#lightbox)
4. Wprowadź `Hello, tvOS` dla **Nazwa aplikacji**, wprowadź użytkownika **identyfikator organizacji** i kliknij przycisk **dalej** przycisk:

    [![](hello-tvos-images/setup04.png "Enter Hello, tvOS")](hello-tvos-images/setup04.png#lightbox)
5. Wprowadź `Hello_tvOS` dla **Nazwa projektu** i kliknij przycisk **Utwórz** przycisk:

    [![](hello-tvos-images/setup03.png "Enter HellotvOS")](hello-tvos-images/setup03.png#lightbox)

Visual Studio for Mac będzie Utwórz nową aplikację Xamarin.tvOS i wyświetlić domyślne pliki, które zostaną dodane do rozwiązania aplikacji:

 [![](hello-tvos-images/project01.png "Domyślny widok plików")](hello-tvos-images/project01.png#lightbox)

Programu Visual Studio for Mac używa **rozwiązań** i **projektów**, w dokładnie tak samo, jak program Visual Studio nie. Rozwiązanie to kontener, który może zawierać jeden lub więcej projektów; projekty mogą zawierać aplikacje obsługujące bibliotek, testowanie aplikacji itp. W takim przypadku programu Visual Studio for Mac, został utworzony zarówno rozwiązania, jak i projekt aplikacji dla Ciebie.

Jeśli chcesz, można utworzyć co najmniej jeden kod biblioteki projektów, które zawierają kod wspólnego, udostępnionych. Te projekty biblioteki mogą być używane przez projekt aplikacji lub udostępniane innych projektów aplikacji Xamarin.tvOS (lub Xamarin.iOS, Xamarin.Android i Xamarin.Mac na podstawie typu kodu), podobnie jak w przypadku, jeśli zostały tworzenia standardowej aplikacji .NET.

## <a name="anatomy-of-a-xamarintvos-app"></a>Anatomia aplikacji Xamarin.tvOS

Jeśli znasz iOS programowania, można zauważyć wiele podobieństw tutaj. W rzeczywistości systemu tvOS 9 jest podzbiór systemu iOS 9, więc partii koncepcji będzie skrzyżowany tutaj.

Spójrzmy na pliki w projekcie:

-   `Main.cs` — Zawiera główny punkt wejścia aplikacji. Gdy aplikacja jest uruchamiana, zawiera bardzo pierwszej klasy i metody, która jest uruchamiana.
-   `AppDelegate.cs` — Ten plik zawiera klasy głównym aplikacji, która jest odpowiedzialny za nasłuchuje zdarzeń z systemu operacyjnego.
-   `Info.plist` — Ten plik zawiera właściwości aplikacji, takie jak nazwa aplikacji, ikony itp.
-   `ViewController.cs` — To jest klasa, która reprezentuje okna głównego i kontroluje cykl życia go.
-   `ViewController.designer.cs` — Ten plik zawiera kod żmudne procesy, które pomaga można zintegrować z interfejsem użytkownika ekranu głównego.
-  `Main.storyboard` — Interfejs użytkownika dla głównego okna. Ten plik można tworzyć i aktualizować przez projektanta Xamarin dla systemu iOS.

W poniższych sekcjach przeniesiemy szybko wyświetlić niektóre z tych plików. Firma Microsoft będzie eksplorować je bardziej szczegółowo później, ale jest dobrym pomysłem jest teraz zrozumienie ich podstawy.

### <a name="maincs"></a>Main.CS

`Main.cs` Plik zawiera statycznego `Main` metodę, która tworzy nowe wystąpienie aplikacji Xamarin.tvOS i przekazuje nazwę klasy, która będzie obsługiwać zdarzeń systemu operacyjnego, który w tym przypadku jest `AppDelegate` klasy:

```csharp
using UIKit;

namespace Hello_tvOS
{
    public class Application
    {
        // This is the main entry point of the application.
        static void Main (string[] args)
        {
            // if you want to use a different Application Delegate class from "AppDelegate"
            // you can specify it here.
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs` Plik zawiera naszych `AppDelegate` klasy, która odpowiada za tworzenie naszych okna i nasłuchuje zdarzeń systemu operacyjnego:

```csharp
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    // The UIApplicationDelegate for the application. This class is responsible for launching the
    // User Interface of the application, as well as listening (and optionally responding) to application events from iOS.
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        // class-level declarations

        public override UIWindow Window {
            get;
            set;
        }

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Override point for customization after application launch.
            // If not required for your application you can safely delete this method

            return true;
        }

        public override void OnResignActivation (UIApplication application)
        {
            // Invoked when the application is about to move from active to inactive state.
            // This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message)
            // or when the user quits the application and it begins the transition to the background state.
            // Games should use this method to pause the game.
        }

        public override void DidEnterBackground (UIApplication application)
        {
            // Use this method to release shared resources, save user data, invalidate timers and store the application state.
            // If your application supports background execution this method is called instead of WillTerminate when the user quits.
        }

        public override void WillEnterForeground (UIApplication application)
        {
            // Called as part of the transition from background to active state.
            // Here you can undo many of the changes made on entering the background.
        }

        public override void OnActivated (UIApplication application)
        {
            // Restart any tasks that were paused (or not yet started) while the application was inactive.
            // If the application was previously in the background, optionally refresh the user interface.
        }

        public override void WillTerminate (UIApplication application)
        {
            // Called when the application is about to terminate. Save data, if needed. See also DidEnterBackground.
        }
    }
}
```

Ten kod jest prawdopodobnie nieznane, chyba że został utworzony przed aplikacji systemu iOS, ale jest bardzo prosta. Przeanalizujmy ważne wierszy.

Po pierwsze Spójrzmy w deklaracji zmiennej poziomie klasy:

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

`Window` Właściwości zapewnia dostęp do głównego okna. używa systemu tvOS, co jest nazywane *Model View Controller* wzorzec (MVC). Ogólnie rzecz biorąc dla każdego okna tworzonych (i wiele innych zastosowań w systemie windows) znajduje się kontroler, który jest odpowiedzialny za cykl życiowy okna, takie jak wyświetlanie, dodawanie nowych widoków (formanty) do jego itp.

Następnie mamy `FinishedLaunching` metody. Ta metoda jest uruchamiana po utworzeniu wystąpienia aplikacji i jest odpowiedzialny za faktycznie tworzenia okna aplikacji i rozpoczyna proces wyświetlania widoku w nim. Aplikacji używa scenorysu w celu zdefiniowania jego interfejsie użytkownika, żaden dodatkowy kod nie jest wymagany w tym miejscu.

Istnieje wiele metod, które zostały opublikowane w szablonie, takich jak `DidEnterBackground` i `WillEnterForeground`. Te można bezpiecznie usunąć Jeśli zdarzenia aplikacji nie są używane w aplikacji.

### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController` Klasa jest nasze główne okno kontrolera. Oznacza to, że jest odpowiedzialny za cyklu życia głównego okna. Zamierzamy należy zbadać to szczegółowo później, obecnie tylko Spójrzmy szybkie go:

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
    }
}
```

#### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Teraz pliku projektanta dla klasy okna głównego jest pusta, ale jej zostaną wypełnione automatycznie przez program Visual Studio dla komputerów Mac jako utworzymy naszych interfejsu użytkownika z projektanta dla systemu iOS:

```csharp
using Foundation;

namespace HellotvOS
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

Firma Microsoft nie są zwykle związanych z plików projektanta, ponieważ są one tylko automatycznie zarządzane przez program Visual Studio dla komputerów Mac i wystarczy podać kod wymagania żmudne procesy, który umożliwia dostęp do formantów, możemy dodać do dowolnego okna lub wyświetlić w naszej aplikacji.

Utworzyliśmy aplikacji Xamarin.tvOS i będziemy mieć podstawową wiedzę na temat składników, Przyjrzyjmy się tworzenie interfejsu użytkownika.

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>Tworzenie interfejsu użytkownika

Nie trzeba utworzyć interfejsu użytkownika dla aplikacji Xamarin.tvOS za pomocą projektanta Xamarin dla systemu iOS, interfejsu użytkownika można tworzyć bezpośrednio w kodzie języka C#, ale która wykracza poza zakres tego artykułu. Dla uproszczenia będziemy używać iOS Designer do tworzenia naszego interfejsu użytkownika w dalszej części tego samouczka.

Aby rozpocząć tworzenie interfejsu użytkownika, możemy kliknij dwukrotnie `Main.storyboard` w pliku **Eksploratora rozwiązań** go otworzyć do edycji w Projektancie z systemem iOS:

[![](hello-tvos-images/designer01.png "Plik Main.storyboard w Eksploratorze rozwiązań")](hello-tvos-images/designer01.png#lightbox)

To należy uruchomić projektanta i wyglądać następująco:

[![](hello-tvos-images/designer02.png "Projektant")](hello-tvos-images/designer02.png#lightbox)

Więcej informacji na temat iOS projektanta i jak działa, można znaleźć w temacie [wprowadzenie do projektanta Xamarin dla systemu iOS](~/ios/user-interface/designer/introduction.md) przewodnik.

Możemy teraz rozpocząć dodawanie formantów do naszej aplikacji Xamarin.tvOS powierzchni projektu.

Wykonaj następujące czynności:

1. Zlokalizuj **przybornika**, powinny być po prawej stronie powierzchni projektu:

    [![](hello-tvos-images/designer03.png "Przybornika")](hello-tvos-images/designer03.png#lightbox)

    Jeśli nie możesz znaleźć go w tym miejscu, przejdź do **Widok > konsole > przybornika** aby go wyświetlić.
2. Przeciągnij **etykiety** z **przybornika** na powierzchnię projektu:

    [![](hello-tvos-images/designer04.png "Przeciągnij z przybornika etykiety")](hello-tvos-images/designer04.png#lightbox)
3. Polecenie **tytuł** właściwości w **konsoli właściwości** i Zmień tytuł przycisku do `Hello, tvOS` i ustaw **rozmiar czcionki** 128:

    [![](hello-tvos-images/designer05.png "Ustaw tytuł na powitania, systemu tvOS i ustaw rozmiar czcionki do 128")](hello-tvos-images/designer05.png#lightbox)
4. Zmienić rozmiar etykiety, dzięki czemu wszystkie wyrazy są widoczne i umieść go wyśrodkowany w górnej części okna:

    [![](hello-tvos-images/designer06.png "Zmienianie rozmiaru i Centrum etykiety")](hello-tvos-images/designer06.png#lightbox)
5. Etykieta teraz należy można ograniczyć do jego pozycji, aby był on wyświetlany zgodnie z oczekiwaniami. niezależnie od rozmiaru ekranu. Aby to zrobić, kliknij etykietę do *w kształcie T dojście* pojawi się:

    [![](hello-tvos-images/designer07.png "Dojście w kształcie T")](hello-tvos-images/designer07.png#lightbox)
6. Aby ograniczyć etykiety w poziomie, zaznacz pole Centrum i przeciągnij go do linia przerywana w pionie:

    [![](hello-tvos-images/designer08.png "Wybierz kwadrat center")](hello-tvos-images/designer08zoom.png#lightbox)

     Etykieta należy włączyć pomarańczowy.
7. Wybierz dojście T w górnej części etykiety, a następnie przeciągnij go do górnej krawędzi okna:

    [![](hello-tvos-images/designer09.png "Przeciągnij uchwyt do górnej krawędzi okna")](hello-tvos-images/designer09.png#lightbox)
8. Następnie kliknij przycisk wysokość i szerokość następnie *dojście kości* jak przedstawiono poniżej:

    [![](hello-tvos-images/designer10.png "Szerokość i wysokość kości dojść")](hello-tvos-images/designer10.png#lightbox)

     Podczas każdego *kości dojście* jest kliknięty, wybierz zarówno szerokości i wysokości odpowiednio stałym wymiary.
9. Po zakończeniu Twojej ograniczenia powinien wyglądać podobnie do tych na karcie Układ konsoli do właściwości:

    [![](hello-tvos-images/designer11.png "Przykład ograniczenia")](hello-tvos-images/designer11.png#lightbox)
8. Przeciągnij **przycisk** z **przybornika** i umieść go pod etykietą.
9. Polecenie **tytuł** właściwości w **konsoli właściwości** i Zmień tytuł przycisku do `Click Me`:

    [![](hello-tvos-images/designer12.png "Zmień tytuł przycisków użytkownikowi. Kliknij przycisk")](hello-tvos-images/designer12.png#lightbox)
10. Powtórz kroki 5 – 8 powyżej, aby ograniczyć przycisk w oknie systemu tvOS. Jednak przeciąganie T-dojścia do góry okna (jak #7 kroku), przeciągnij ją do dołu etykiety:

    [![](hello-tvos-images/designer14.png "Ogranicz przycisku")](hello-tvos-images/designer14.png#lightbox)
11. Przeciągnij inną etykietę przycisku, rozmiar, aby mieć szerokość jako pierwsza etykieta i ustaw jej **wyrównanie** do **Center**:

    [![](hello-tvos-images/designer15.png "Przeciągnij inną etykietę po kliknięciu przycisku, rozmiar, aby mieć szerokość jako pierwsza etykieta a jego położenie do Centrum")](hello-tvos-images/designer15.png#lightbox)
12. Podobnie jak pierwszy etykiety i przycisk Ustaw tę etykietę do wyśrodkowywania i przypiąć go do lokalizacji i rozmiaru:

    [![](hello-tvos-images/designer16.png "Etykieta do lokalizacji i rozmiaru kodu PIN")](hello-tvos-images/designer16.png#lightbox)
13. Zapisz zmiany w interfejsie użytkownika.

Jak zostały Zmienianie rozmiaru i przenoszenia kontrolek wokół, powinien zauważenia czy projektant zapewnia wskazówki przydatne przystawki, oparte na [Apple TV Human Interface Guidelines](https://developer.apple.com/tvos/human-interface-guidelines/). Niniejsze wytyczne pomoże Ci tworzenie wysokiej jakości aplikacji, które będą miały znanych wyglądu i działania dla Apple TV użytkowników.

W **konspekt dokumentu** sekcji, zwróć uwagę, jak pokazano układ i hierarchię elementów wchodzące w skład naszych interfejsu użytkownika:

[![](hello-tvos-images/designer17.png "W sekcji konspektu dokumentu")](hello-tvos-images/designer17.png#lightbox)

W tym miejscu można wybrać elementy do edycji lub przeciągnij, aby zmienić kolejność elementów interfejsu użytkownika, jeśli to konieczne. Na przykład jeśli element interfejsu użytkownika są objęte przez inny element, można go przeciągnij w dół na liście, aby stał się najwyżej elementu w oknie.

Teraz, gdy mamy naszych interfejsu użytkownika utworzone musimy ujawnia elementów interfejsu użytkownika, dzięki czemu Xamarin.tvOS mogą uzyskać dostęp i interakcji z użytkownikiem w kodzie języka C#.

### <a name="accessing-the-controls-in-the-code-behind"></a>Uzyskiwanie dostępu do formantów w kodzie

Istnieją dwa sposoby dostęp do formantów, które zostały dodane w Projektancie systemu iOS z kodu:

* Tworzenie obsługi zdarzeń w formancie.
* Nadanie formantu nazwy, tak, aby firma Microsoft może później odwoływać.

Gdy każda z tych zostaną dodane, częściowej klasy w `ViewController.designer.cs` zostanie zaktualizowany w celu odzwierciedlenia zmian. Umożliwi to dostęp do formantów w kontroler widoku.

### <a name="creating-an-event-handler"></a>Tworzenie obsługi zdarzeń

W tej przykładowej aplikacji, po kliknięciu przycisku chcemy _coś_ na to, aby program obsługi zdarzeń musi zostać dodany do określonego zdarzenia na przycisku. Aby to skonfigurować, wykonaj następujące czynności:

1. W Xamarin iOS projektanta wybierz przycisk na kontroler widoku.
2. W konsoli do właściwości, wybierz **zdarzenia** karty:

    [![](hello-tvos-images/event1.png "Na karcie zdarzenia")](hello-tvos-images/event1.png#lightbox)
3. Znajdź zdarzenie TouchUpInside i nadaj mu program obsługi zdarzeń o nazwie `Clicked`:

    [![](hello-tvos-images/event2.png "Zdarzenie TouchUpInside")](hello-tvos-images/event2.png#lightbox)
4. Po naciśnięciu **Enter**, **ViewController**zostanie otwarty plik CS sugerowanie lokalizacje obsługi zdarzenia w kodzie. Użyj klawiszy strzałek na klawiaturze, aby ustawić lokalizacji:

    [![](hello-tvos-images/event3.png "Ustawianie lokalizacji")](hello-tvos-images/event3.png#lightbox)
5. Spowoduje to utworzenie metody częściowej, jak pokazano poniżej:

    [![](hello-tvos-images/event4.png "Metody częściowe")](hello-tvos-images/event4.png#lightbox)

Firma Microsoft są teraz rozpocząć dodawanie niektórych kodu, aby umożliwić przycisk, aby funkcja.

### <a name="naming-a-control"></a>Nazewnictwo formantu

Po kliknięciu przycisku, należy zaktualizować etykiety na podstawie liczby kliknięć. Aby to zrobić, potrzebujemy dostępu etykiety w kodzie. Jest to realizowane przez nadanie mu nazwy. Wykonaj następujące czynności:

1. Otwórz scenorysu, a następnie wybierz etykietę w dolnej części kontroler widoku.
2. W konsoli do właściwości, wybierz **elementu Widget** karty:

    [![](hello-tvos-images/name1.png "Wybierz kartę widżetu")](hello-tvos-images/name1.png#lightbox)
3. W obszarze **tożsamości > nazwa**, Dodaj `ClickedLabel`:

    [![](hello-tvos-images/name2.png "Ustaw ClickedLabel")](hello-tvos-images/name2.png#lightbox)

Firma Microsoft są teraz gotowe do rozpoczęcia aktualizowania etykiety!

### <a name="how-controls-are-accessed"></a>Jak są dostępne formantów

W przypadku wybrania `ViewController.designer.cs` w **Eksploratora rozwiązań** będzie mógł wyświetlić sposób `ClickedLabel` etykiety i `Clicked` obsługi zdarzeń zostały zamapowane do **gniazda** i  **Akcja** w języku C#:

[![](hello-tvos-images/accesscontrol.png "Gniazda i akcji")](hello-tvos-images/accesscontrol.png#lightbox)

Należy również zauważyć, że `ViewController.designer.cs` jest częściowej klasy, dzięki czemu nie ma programu Visual Studio for Mac, można zmodyfikować `ViewController.cs` który zastąpiłaby wszelkie zmiany, które zostały wprowadzone do klasy.

Udostępnianie elementów interfejsu użytkownika w ten sposób pozwala uzyskiwać do nich dostęp kontroler widoku.

Zwykle nie należy otworzyć `ViewController.designer.cs` samodzielnie, jego został przedstawiony tutaj wyłącznie w celach edukacyjnych.

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>Pisanie kodu

Z naszych interfejsu użytkownika utworzone i jego elementów interfejsu użytkownika do kodu za pomocą **gniazda** i **akcje**, możemy finally gotowy do pisania kodu, aby zapewnić funkcji programu.

W naszej aplikacji za każdym razem, gdy po kliknięciu przycisku pierwszej zamierzamy zaktualizować naszych etykiety w celu przedstawienia ile razy przycisk został kliknięty. W tym celu należy otworzyć `ViewController.cs` plik do edycji przez dwukrotne kliknięcie w **konsoli rozwiązania**:

[![](hello-tvos-images/code01.png "Konsola rozwiązania")](hello-tvos-images/code01.png#lightbox)

Najpierw należy utworzyć zmienną poziomie klasy w naszym `ViewController` klasę, aby śledzić liczbę kliknięć, które miały miejsce. Edytowanie definicji klasy i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Dalej w tej samej klasie (`ViewController`), należy zastąpić **ViewDidLoad** — metoda i dodać niektóre kod, aby ustawić pierwszy komunikat dla naszych etykiety:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

Należy użyć `ViewDidLoad `, zamiast innej metody, takie jak `Initialize`, ponieważ `ViewDidLoad ` jest nazywany *po* system operacyjny został załadowany i utworzyć wystąpienia interfejsu użytkownika z `.storyboard` pliku. Jeśli podjęto próbę dostępu formantu etykiety przed `.storyboard` pliku została całkowicie załadowany i wystąpienia, może pobrać `NullReferenceException` błąd ponieważ formantu etykiety nie zostałyby jeszcze utworzone.

Następnie należy dodać kod, aby odpowiadać na użytkownika, klikając przycisk. Dodaj następujący kod do częściowego klasy się, że utworzyliśmy:

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

Ten kod zostanie wywołana w dowolnym momencie użytkownik kliknie przycisk naszych.

Z wszystko w miejscu możemy teraz przystąpić do tworzenia i testowania aplikacji Xamarin.tvOS.

## <a name="testing-the-application"></a>Testowanie aplikacji

Nadszedł czas, aby skompilować i uruchomić aplikację do upewnij się, że działa zgodnie z oczekiwaniami. Firma Microsoft może skompilować i uruchomić w jednym kroku lub budujemy go bez konieczności uruchamiania ich.

Zawsze, gdy mamy utworzyć aplikację, wybieramy opcję chcemy jaki rodzaj kompilacji:

-   **Debugowanie** — Kompilacja debugowania ma zostać skompilowany w '' plik (aplikacja) o dodatkowe metadane, który umożliwia firmie Microsoft w celu debugowania, co dzieje, gdy aplikacja jest uruchomiona.
-   **Zwolnij** — tworzy także kompilacji wydania '' plik, ale nie zawiera informacji o debugowaniu, więc jest mniejsze i wykonuje szybciej.  

Można wybrać typ kompilacji z **selektora konfiguracji** w górnym lewym dolnym rogu programu Visual Studio for Mac ekranu:

[![](hello-tvos-images/run01.png "Wybierz typ kompilacji")](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>Kompilowanie aplikacji

W tym przypadku firma Microsoft po prostu chcesz kompilację debugowania, więc warto upewnij się, że **debugowania** jest zaznaczone. Utworzymy naszej aplikacji najpierw przez albo naciskając klawisz **⌘B**, lub z **kompilacji** menu, wybierz **kompilacji wszystkich**.

Jeśli nie odnaleziono żadnych błędów, zostanie wyświetlone **kompilacji zakończyło się pomyślnie** komunikat w programie Visual Studio dla komputerów Mac w pasku stanu. Jeśli wystąpiły błędy, przejrzyj projektu i upewnij się, że kroki zostały wykonane prawidłowo. Rozpocznij od potwierdzenie, że kod (zarówno w środowisku Xcode, jak i w programie Visual Studio dla komputerów Mac) zgodny z kodem w samouczku.

### <a name="running-the-application"></a>Uruchamianie aplikacji

Aby uruchomić aplikację, będziemy mieć trzy opcje:

-  Naciśnij klawisz **⌘ + Enter**.
-  Z **Uruchom** menu, wybierz **debugowania**.
-  Kliknij przycisk **odtwarzanie** przycisk w Visual Studio for Mac paska narzędzi (tylko powyżej **Eksploratora rozwiązań**).

Aplikacja będzie kompilowana (Jeśli nie został skompilowany już), w trybie debugowania, systemu tvOS symulatora będzie start i aplikacja będzie Uruchom i wyświetl jego okno główne interfejsu:

[![Przykład ekranu głównego aplikacji](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

Z **sprzętu** menu wybierz opcję **Pokaż zdalnego Apple TV** , można kontrolować symulatora.

[![](hello-tvos-images/run04.png "Wybierz Pokaż Apple TV zdalnego")](hello-tvos-images/run04.png#lightbox)

Za pomocą symulatora zdalnej, po kliknięciu przycisku kilka razy etykiety powinien być zaktualizowany o licznika:

[![](hello-tvos-images/run05.png "Etykieta o liczba zaktualizowanych")](hello-tvos-images/run05.png#lightbox)

Gratulacje! Firma Microsoft obejmuje wiele podstaw w tym miejscu, ale po wykonaniu tego samouczka od początku do końca po wykonaniu pełny opis składników aplikacji Xamarin.tvOS, a także narzędzia służące do ich tworzenia.

## <a name="where-to-next"></a>Gdzie dalej?

Tworzenie przedstawia aplikacje Apple TV kilka będzie wymagał z powodu zakończenia połączenia między użytkownikiem i interfejsu (jest w pomieszczeniu nie znajduje się w ręcznie przez użytkownika) i systemu tvOS ograniczenia umieszcza na rozmiar aplikacji i magazynu.

W związku z tym zdecydowanie zaleca się czy Twoje odczytu następujące dokumenty przed przeskakiwanie do projektowania aplikacji Xamarin.tvOS:

- [Wprowadzenie do systemu tvOS 9](~/ios/tvos/platform/tvos9.md) — w tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w systemu tvOS 9 dla deweloperów Xamarin.tvOS.
- [Praca z nawigacji i skoncentrować się](~/ios/tvos/app-fundamentals/navigation-focus.md) — użytkowników aplikacji Xamarin.tvOS nie będzie interakcja jego interfejsu bezpośrednio z systemem iOS których one wybierz obrazy na ekranie urządzenia, ale pośrednio ze wszystkich miejsca przy użyciu zdalnego Siri. W tym artykule omówiono pojęcia fokus i jak jest używany do obsługi nawigacji w interfejsie użytkownika aplikacji Xamarin.tvOS.
- [Siri Remote i kontrolerów Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) — główny sposób użytkownicy będą interakcji z Apple TV, a aplikacja Xamarin.tvOS, przy użyciu zdalnego Siri dołączone. Jeśli aplikacja jest gier, można opcjonalnie utworzyć wsparcie dla strony 3, wprowadzone dla kontrolery gier dla systemu iOS (MIF) Bluetooth w aplikacji oraz. W tym artykule omówiono pomocniczych w aplikacjach Xamarin.tvOS nowe kontrolery gier Siri Remote i Bluetooth.
- [Magazyn danych i zasoby](~/ios/tvos/app-fundamentals/resources-data-storage.md) — w odróżnieniu od urządzeń z systemem iOS, nowe Apple TV nie ma stałego i lokalny magazyn dla aplikacji systemu tvOS. W związku z tym jeśli Xamarin.tvOS aplikacji musi być zachowany informacje (takie jak preferencje użytkownika) musi przechowywanie i pobieranie danych z usługi iCloud. W tym artykule omówiono pracy z zasobami i magazynu danych w aplikacji Xamarin.tvOS.
- [Praca z obrazów i ikon](~/ios/tvos/app-fundamentals/icons-images.md) — tworzenie urzekające ikon i obrazów jest krytyczną częścią zdobywania doświadczenia użytkownika bez ramek do pracy z aplikacjami firmy Apple TV. W tym przewodniku opisano kroki wymagane do tworzenia i obejmują niezbędnych zasobów graficznych dla aplikacji Xamarin.tvOS.
- [Interfejs użytkownika](~/ios/tvos/user-interface/index.md) — pokrycia ogólne środowisko użytkownika (UX), w tym formantów interfejsu użytkownika (UI), użyj Konstruktora interfejsu i zasady projektowania środowiska użytkownika w środowisku Xcode podczas pracy z Xamarin.tvOS.
- [Wdrażanie i testowanie](~/ios/tvos/deploy-test/index.md) — w tej sekcji omówiono tematy wykorzystywane do testowania aplikacji oraz do rozpowszechniania. Tematy w tym miejscu zawierają elementów, takich jak narzędzia używane do debugowania, wdrażania, testerów i sposób publikowania aplikacji do sklepu z aplikacjami firmy Apple TV.

W razie problemów Praca z Xamarin.tvOS, zobacz nasze [Rozwiązywanie problemów](~/ios/tvos/troubleshooting.md) w dokumentacji listę na wiedzieć, problemy i rozwiązania.

## <a name="summary"></a>Podsumowanie

W tym artykule podać szybki start do tworzenie aplikacji dla systemu tvOS z programem Visual Studio dla komputerów Mac, tworząc prostego powitania, systemu tvOS aplikacji. Go omówione podstawy urządzenia systemu tvOS inicjowania obsługi tworzenia interfejsu, kodowania systemu tvOS i testowania w symulatorze systemu tvOS.


## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Tworzenie aplikacji dla systemu tvOS za pomocą platformy Xamarin (klip wideo)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
