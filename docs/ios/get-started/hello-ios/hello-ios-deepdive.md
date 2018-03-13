---
title: "Witaj, iOS: nowości"
description: "W tym przewodniku dwuczęściową opisuje sposób tworzenia podstawowej aplikacji platformy Xamarin.iOS przy użyciu programu Visual Studio dla komputerów Mac lub Visual Studio i zrozumienia podstaw dotyczących tworzenia aplikacji systemu iOS za pomocą platformy Xamarin. Spowoduje to wprowadzenie narzędzi, pojęcia i kroki wymagane do tworzenia i wdrażania aplikacji platformy Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 61ba3a7e-fe11-4439-8bc8-9809512b8eff
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: d7a458a0a0c2da1dbb40ae7222fcd35cf7172953
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="hello-ios-deep-dive"></a>Witaj, nowości w systemie iOS

Przewodnik Szybki Start, wprowadzono tworzenia i uruchamiania podstawowej aplikacji platformy Xamarin.iOS. Teraz nadszedł czas na opracowanie lepiej zrozumieć, jak aplikacje systemu iOS działają, można je tworzyć bardziej złożone programy. W tym przewodniku przegląda kroki w Hello, iOS wskazówki umożliwiające opis podstawowych założeń projektowanie aplikacji systemu iOS.

Następujące tematy zostały opisane w tym artykule:


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- **Wprowadzenie do programu Visual Studio dla komputerów Mac** — wprowadzenie do programu Visual Studio for Mac i tworzenie nowej aplikacji.
- **Anatomia aplikacji platformy Xamarin.iOS** — samouczek podstawowych części aplikacji platformy Xamarin.iOS.
- **Architektura i podstawowe informacje na temat aplikacji** — przegląd części aplikacji systemu iOS i relacji między nimi.
- **Interfejs użytkownika (UI)** — Tworzenie interfejsów użytkownika z systemem iOS projektanta.
- **Wyświetlenie kontrolerów i cyklem życia widoku** — wprowadzenie cyklu życia widoku i zarządzanie nimi zawartości widoku hierarchii z kontrolera widoku.
- **Testowanie, wdrożenia i ostateczne poprawki** — zakończenie aplikacji przy użyciu wskazówki dotyczące testowania, wdrożenia generowania kompozycji i więcej.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- **Wprowadzenie do programu Visual Studio** — wprowadzenie do programu Visual Studio i tworzenie nowej aplikacji.
- **Anatomia aplikacji platformy Xamarin.iOS** — samouczek podstawowych części aplikacji platformy Xamarin.iOS.
- **Architektura i podstawowe informacje na temat aplikacji** — przegląd części aplikacji systemu iOS i relacji między nimi.
- **Interfejs użytkownika (UI)** — Tworzenie interfejsów użytkownika z systemem iOS projektanta.
- **Wyświetlenie kontrolerów i cyklem życia widoku** — wprowadzenie cyklu życia widoku i zarządzanie nimi zawartości widoku hierarchii z kontrolera widoku.
- **Testowanie, wdrożenia i ostateczne poprawki** — zakończenie aplikacji przy użyciu wskazówki dotyczące testowania, wdrożenia generowania kompozycji i więcej.

-----

Ten przewodnik ułatwia tworzenie umiejętności i wiedzy wymagane do tworzenia aplikacji iOS jednego ekranu. Po zakończeniu pracy przy jego użyciu, powinien mieć wiedzę o różnych części aplikacji platformy Xamarin.iOS i sposób ich dopasowania.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Wprowadzenie do programu Visual Studio dla komputerów Mac

Visual Studio for Mac jest bezpłatne, open source IDE, łączącą funkcje programu Visual Studio i XCode. Zawiera funkcje pełni zintegrowane wizualnego projektanta, Edytor tekstu, wraz z narzędzia do refaktoryzacji, przeglądarkę zestawu i integracji kodu źródłowego. W tym przewodniku przedstawiono niektóre podstawowe programu Visual Studio dla funkcji Mac, ale jeśli jesteś nowym użytkownikiem programu Visual Studio for Mac, zapoznaj się [programu Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/) dokumentacji.

Visual Studio for Mac następuje rozwiązanie Visual Studio kodu do organizowania *rozwiązań* i *projekty*. Rozwiązanie to kontener, który może zawierać jeden lub więcej projektów. Projekt może być aplikacji (na przykład iOS lub Android), biblioteki obsługi aplikacji testu i więcej. W aplikacji Phoneword iPhone nowy projekt został dodany przy użyciu **pojedynczą aplikacją widoku** szablonu. Początkowa rozwiązanie po zapoznaniu się następująco:

![](hello-ios-deepdive-images/image30.png "Zrzut ekranu przedstawiający początkowej rozwiązania")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Wprowadzenie do programu Visual Studio

Program Visual Studio jest zaawansowanym IDE firmy Microsoft. Zawiera funkcje pełni zintegrowane wizualnego projektanta, Edytor tekstu, wraz z narzędzia do refaktoryzacji, przeglądarkę zestawu i integracji kodu źródłowego. W tym przewodniku przedstawiono niektóre podstawowe funkcje programu Visual Studio z platformą Xamarin wtyczki.

Program Visual Studio umożliwia organizowanie kodu w _rozwiązań_ i *projekty*. Rozwiązanie to kontener, który może zawierać jeden lub więcej projektów. Projekt może być aplikacji (na przykład iOS lub Android), biblioteki obsługi aplikacji testu i więcej. W aplikacji Phoneword iPhone nowy projekt został dodany przy użyciu **pojedynczą aplikacją widoku** szablonu. Początkowa rozwiązanie po zapoznaniu się następująco:

![](hello-ios-deepdive-images/vs-image30.png "Zrzut ekranu przedstawiający początkowej rozwiązania")

-----

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinios-application"></a>Anatomia aplikacji platformy Xamarin.iOS

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po lewej stronie jest *konsoli rozwiązania*, który zawiera strukturę katalogów i wszystkie pliki skojarzone z rozwiązania:

![](hello-ios-deepdive-images/image31.png "Konsoli rozwiązanie zawiera strukturę katalogów oraz ich wszystkie pliki skojarzone z rozwiązania")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po prawej stronie jest *okienko rozwiązania*, który zawiera strukturę katalogów i wszystkie pliki skojarzone z rozwiązania:

![](hello-ios-deepdive-images/vs-image31.png "Okienko rozwiązania, które zawiera strukturę katalogów oraz ich wszystkie pliki skojarzone z rozwiązania")

-----

W [Hello, iOS](~/ios/get-started/hello-ios/hello-ios-quickstart.md) przewodnika tworzenia rozwiązania o nazwie **Phoneword** i umieścić iOS projektu - **Phoneword_iOS** — wewnątrz niej. Elementy wewnątrz projektu obejmują:

-  **Odwołania** — zawiera zestawy wymagane, aby skompilować i uruchomić aplikację. Rozwiń węzeł katalogu, aby zobaczyć odwołania do zestawów platformy .NET, takie jak [systemu](http://msdn.microsoft.com/en-us/library/system%28v=vs.110%29.aspx) , System.Core, i [System.Xml](http://msdn.microsoft.com/en-us/library/system.xml%28v=vs.110%29.aspx) , oraz odwołanie do zestawu Xamarin.iOS na platformie Xamarin.
-  **Pakiety** -katalog pakietów zawiera gotowe pakietów NuGet.
-  **Zasoby** — folder zasobów przechowuje innego nośnika.
-  **Main.cs** — zawiera główny punkt wejścia aplikacji. Aby uruchomić aplikację, nazwa klasy głównym aplikacji `AppDelegate`, jest przekazany.
-  **AppDelegate.cs** — ten plik zawiera klasy głównym aplikacji i jest odpowiedzialny za tworzenie okna, tworzeniu interfejsu użytkownika i nasłuchuje zdarzeń z systemu operacyjnego.
-  **Main.Storyboard** -scenorysu zawiera projekt visual interfejsu użytkownika aplikacji. Pliki scenorysu Otwórz za pomocą edytora graficznego o nazwie iOS projektanta.
-  **ViewController.cs** — ekranu (Widok), który użytkownik będzie widział i dotyka uprawnień kontrolera widoku. Kontroler widoku jest odpowiedzialny za obsługę interakcje między użytkownikiem i widoku.
-  **ViewController.designer.cs** — `designer.cs` jest plikiem automatycznie generowanej służy jako sklejki między formantami w widoku i ich oświadczenia kodu w kontrolerze widoku. Jest to plik wewnętrzny żmudne procesy, IDE spowoduje zastąpienie wszystkich zmian ręcznych i w większości przypadków, które można zignorować ten plik. Aby uzyskać więcej informacji na relacji między Projektant wizualny oraz kod zapasowy odwoływać się do [wprowadzenie do projektanta dla systemu iOS](~/ios/user-interface/designer/introduction.md) przewodnik.
-  **Info.plist** — `Info.plist` jest, gdzie są ustawione właściwości aplikacji, takie jak nazwa aplikacji, ikony, uruchom obrazów i. Jest to plik wydajne i dokładne wprowadzenie do niego jest dostępny w [Praca z listy właściwości](~/ios/app-fundamentals/property-lists.md) przewodnik.
-  **Entitlements.plist** — listę właściwości uprawnień pozwoli określić aplikację *możliwości* (nazywanych również technologie magazynu aplikacji), takich jak iCloud, PassKit i inne. Więcej informacji na temat `Entitlements.plist` znajdują się w [Praca z listy właściwości](~/ios/app-fundamentals/property-lists.md) przewodnik. Ogólne wprowadzenie do uprawnień, można znaleźć w temacie [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) przewodnik.

## <a name="architecture-and-app-fundamentals"></a>Architektura i podstawowe informacje dotyczące aplikacji

Przed aplikacji systemu iOS można załadować interfejsu użytkownika, dwie czynności konieczne w miejscu. Po pierwsze, aplikacja musi definiować *punktu wejścia* — pierwszy kod, który jest uruchamiany, gdy proces aplikacji jest ładowany do pamięci. Po drugie należy zdefiniować klasę do obsługi zdarzeń dla całej aplikacji i interakcji z systemem operacyjnym.

W tej sekcji badań relacje przedstawiony na poniższym diagramie:

[![](hello-ios-deepdive-images/image32.png "Przedstawiono relacje architektury i podstawowe informacje na temat aplikacji na tym diagramie")](hello-ios-deepdive-images/image32.png#lightbox)

Teraz rozpocząć od początku i Dowiedz się, co się dzieje podczas uruchamiania aplikacji.

### <a name="main"></a>Main

Główny punkt wejścia aplikacji systemu iOS jest `Main.cs` pliku. `Main.cs` zawiera statyczną metodę Main, która tworzy nowe wystąpienie aplikacji platformy Xamarin.iOS i przekazuje nazwę *delegata aplikacji* klasy, która będzie obsługiwać zdarzenia systemu operacyjnego. Kod szablonu dla statycznych `Main` metody pojawia się poniżej:

```csharp
using System;
using UIKit;

namespace Phoneword_iOS
{
    public class Application
    {
        static void Main (string[] args)
        {
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="application-delegate"></a>Delegat aplikacji

W systemie iOS *delegata aplikacji* klasa obsługuje zdarzenia systemowe; ta klasa znajduje się wewnątrz `AppDelegate.cs`. `AppDelegate` Klasy zarządza stosowaniem *okna*. Okno jest jedno wystąpienie `UIWindow` klasy, która służy jako kontener dla interfejsu użytkownika. Domyślnie aplikacja uzyskuje tylko jedno okno, na który ma zostać załadowana zawartość i okna jest dołączony do *ekranu* (pojedynczy `UIScreen` wystąpienia) zapewnia prostokątem dopasowania wymiary fizyczny ekranu urządzenia.

*AppDelegate* również jest odpowiedzialny za subskrybowanie aktualizacji systemu o zdarzeniach ważne aplikacji, np. po zakończeniu uruchamiania aplikacji lub gdy jest za mało pamięci.

Kod szablonu AppDelegate znajduje się poniżej:

```csharp
using System;
using Foundation;
using UIKit;

namespace Phoneword_iOS
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        public override UIWindow Window {
            get;
            set;
        }

        ...
    }
}
```

Gdy aplikacja ma zdefiniowane okna, można rozpocząć ładowania interfejsu użytkownika. Następna sekcja opisuje tworzenie interfejsu użytkownika.

## <a name="user-interface"></a>Interfejs użytkownika

Interfejs użytkownika aplikacji systemu iOS przypomina sklepu — aplikacji zwykle pobiera jedno okno, ale jego może zapełnić okna z musi wiele obiektów jego, a obiekty i ustalenia można zmieniać w zależności od tego, co aplikacja potrzebuje do wyświetlenia. W tym scenariuszu - rzeczy, które użytkownik widzi — obiekty są nazywane widoków. Aby utworzyć jednym ekranie w aplikacji, widoki stos na siebie w *hierarchii widok zawartości*, i hierarchii jest zarządzany przez pojedynczy kontroler widoku. Aplikacje z ekranami wielu mają wiele zawartości widoku hierarchii, każde z nich własny kontroler widoku i aplikacji umieszcza widoków w oknie można utworzyć innej hierarchii widok zawartości oparte na ekranie, której należy użytkownik.

W tej sekcji dives w interfejsie użytkownika przez opisujące widoków, zawartości hierarchii widok i Projektant z systemem iOS.

### <a name="ios-designer-and-storyboards"></a>iOS projektanta i planów

IOS Projektant jest visual narzędzia do tworzenia interfejsów użytkownika w programie Xamarin. Projektant można uruchomić przez dwukrotne kliknięcie w dowolnym pliku scenorysu (.storyboard), który zostanie otwarty widok przypominający poniższy zrzut ekranu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image33.png "iOS Projektant — interfejs")

A *scenorysu* to plik zawiera projekty wizualne ekrany naszej aplikacji oraz przejścia i relacje między ekranów. Reprezentacja ekran aplikacji w scenorysu jest nazywany _sceny_. Każdy sceny reprezentuje kontroler widoku i stosu widoków zarządza (hierarchii widok zawartości). Podczas tworzenia nowego **pojedynczą aplikacją widoku** projektu jest tworzona na podstawie szablonu, Visual Studio for Mac automatycznie generuje plik scenorysu o nazwie `Main.storyboard` i wypełnia je pojedynczego sceny, jak pokazano na zrzucie ekranu poniżej:

![](hello-ios-deepdive-images/image34.png "Visual Studio for Mac automatycznie generuje plik scenorysu o nazwie Main.storyboard i wypełnia pojedynczego sceny")

Wybieranie kontrolera widoku w sceny można wybrać czarny pasku w dolnej części ekranu scenorysu. Kontroler widoku jest wystąpieniem `UIViewController` klasy, która zawiera kod zapasowy hierarchii widok zawartości. Właściwości na tym kontrolerze widoku można wyświetlać i ustawić wewnątrz **konsoli właściwości**, jak pokazano na poniższym zrzucie ekranu:

![](hello-ios-deepdive-images/image35.png "W okienku właściwości")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image33.png "iOS Projektant — interfejs")

A *scenorysu* to plik zawiera projekty wizualne ekrany naszej aplikacji oraz przejścia i relacje między ekranów. Reprezentacja ekran aplikacji w scenorysu jest nazywany _sceny_. Każdy sceny reprezentuje kontroler widoku i stosu widoków zarządza (hierarchii widok zawartości). Podczas tworzenia nowego **pojedynczą aplikacją widoku** projektu jest tworzona na podstawie szablonu, Visual Studio automatycznie generuje plik scenorysu o nazwie `Main.storyboard` i wypełnia je pojedynczego sceny, jak pokazano na poniższym zrzucie ekranu:

![](hello-ios-deepdive-images/vs-image34.png "Program Visual Studio automatycznie generuje plik scenorysu o nazwie Main.storyboard i wypełnia pojedynczego sceny")

Na pasku w dolnej części ekranu scenorysu, można wybrać do Wybierz kontroler widoku dla sceny. Kontroler widoku jest wystąpieniem `UIViewController` klasy, która zawiera kod zapasowy hierarchii widok zawartości. Właściwości na tym kontrolerze widoku można wyświetlać i ustawić wewnątrz **w okienku właściwości**, jak pokazano na poniższym zrzucie ekranu:

![](hello-ios-deepdive-images/vs-image35.png "W okienku właściwości")

-----

_Widoku_ można wybrać, klikając wewnątrz biała część sceny. Widok jest wystąpieniem `UIView` klasa, która definiuje obszar ekranu i udostępnia interfejsy do pracy z zawartości w tym obszarze. Domyślny widok jest jeden *widoku głównego* który wypełnia ekranu całych urządzenia.

Z lewej strony sceny jest Szara strzałka z ikoną flagi, jak pokazano na poniższym zrzucie ekranu:

 [![](hello-ios-deepdive-images/image37.png "Szara strzałka z ikoną flagi")](hello-ios-deepdive-images/image37.png#lightbox)

Szara strzałka reprezentuje przejście scenorysu o nazwie *Segue* (Wymowa "seg sposób"). Ponieważ ta Segue nie ma żadnych źródła, jest nazywany *Sourceless Segue*. Sourceless Segue wskazuje pierwszy sceny, którego widoki pobiera załadowane do naszej aplikacji okna przy uruchamianiu aplikacji. Sceny i widoki w nim będą najpierw, użytkownik zobaczy załadowanie aplikacji.

Podczas konstruowania interfejsu użytkownika, dodatkowe widoki mogą być przeciągnięte z **przybornika** na widok główny na powierzchni projektu, jak pokazano na poniższym zrzucie ekranu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image38.png "Dodatkowe widoki mogą być przeciągnięte z przybornika do widoku głównego na powierzchni projektu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image38.png "Dodatkowe widoki mogą być przeciągnięte z przybornika do widoku głównego na powierzchni projektu")

-----

Widoki te dodatkowe są nazywane *widoków podrzędnych*. Razem widoku głównego i widoków podrzędnych są częścią *hierarchii widok zawartości* zarządzanym przez `ViewController`. Konspekt wszystkie elementy w sceny można wyświetlić, sprawdzając w **konspekt dokumentu** konsoli:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image39.png "Konsola konspekt dokumentu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image39.png "Konsola konspekt dokumentu")

-----

Widoków podrzędnych są wyróżnione na poniższym diagramie:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image40.png "Widoków podrzędnych są wyróżnione na diagramie")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image40.png "Widoków podrzędnych są wyróżnione na diagramie")

-----

Następna sekcja zawiera podział według hierarchii widok zawartości reprezentowany przez ten sceny.

## <a name="content-view-hierarchy"></a>Wyświetlanie zawartości hierarchii

A _hierarchii widok zawartości_ jest stos widoki i widoków podrzędnych zarządzany przez pojedynczy kontroler widoku, jak pokazano na poniższym diagramie:

 [![](hello-ios-deepdive-images/image41.png "Wyświetlanie zawartości hierarchii")](hello-ios-deepdive-images/image41.png#lightbox)

Firma Microsoft może wprowadzać hierarchii widok zawartości naszych `ViewController` widoczność tymczasowo zmiana koloru tła widoku głównego żółty w sekcji widoku z **konsoli właściwości**, jak pokazano na poniższym zrzucie ekranu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image42.png "Zmiana koloru tła widoku głównego żółty w sekcji widoku konsoli do właściwości")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image42.png "Zmiana koloru tła widoku głównego żółty w sekcji widoku konsoli do właściwości")

-----

Na poniższym diagramie przedstawiono relacje między okna, widoków, widoków podrzędnych i kontrolera widoku, które Przełącz interfejs użytkownika ekranu urządzenia:

 [![](hello-ios-deepdive-images/image43.png "Relacje między okna, widoków, widoków podrzędnych i kontrolera widoku")](hello-ios-deepdive-images/image43.png#lightbox)

W następnej sekcji opisano sposób pracy z widoków w kodzie i Naucz się programować za pomocą kontrolerów widoku i cyklem życia widok interakcji z użytkownikiem.

## <a name="view-controllers-and-the-view-lifecycle"></a>Widok kontrolerów i cyklem życia widoku

Każdej hierarchii widok zawartości ma odpowiedniego kontrolera widoku w celu interakcji z użytkownikiem zasilania. Roli kontrolera widoku jest zarządzanie widokami w hierarchii widok zawartości. Kontroler widoku nie jest częścią hierarchii widok zawartości, a nie jest elementem interfejsu. Zamiast zawiera kod, który obsługuje interakcji użytkownika z obiektami na ekranie.

### <a name="view-controllers-and-storyboards"></a>Widok kontrolerów i planów

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kontroler widoku jest reprezentowana w scenorysu jako pasek u dołu sceny. Wybiera kontroler widoku powoduje wyświetlenie jej właściwości w **konsoli właściwości**:

![](hello-ios-deepdive-images/image44.png "Wybiera kontroler widoku powoduje wyświetlenie jej właściwości w okienku właściwości")

Niestandardowej klasy kontrolera widoku zawartości hierarchii widoku reprezentowany przez ten sceny można ustawić, edytując **klasy** właściwości w **tożsamości** sekcji **konsoli właściwości**. Na przykład naszych **Phoneword** zestawów aplikacji `ViewController` jako kontroler widoku dla naszych pierwszy ekran, jak pokazano na poniższym zrzucie ekranu:

![](hello-ios-deepdive-images/image45new.png "Aplikacja Phoneword ustawia ViewController jako kontroler widoku")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kontroler widoku jest reprezentowana w scenorysu jako pasek u dołu sceny. Wybiera kontroler widoku powoduje wyświetlenie jej właściwości w **w okienku właściwości**:

![](hello-ios-deepdive-images/vs-image44.png "Wybiera kontroler widoku powoduje wyświetlenie jej właściwości w okienku właściwości")

Niestandardowej klasy kontrolera widoku zawartości hierarchii widoku reprezentowany przez ten sceny można ustawić, edytując **klasy** właściwości w **tożsamości** sekcji **wokienkuwłaściwości**. Na przykład naszych **Phoneword** zestawów aplikacji `ViewController` jako kontroler widoku dla naszych pierwszy ekran, jak pokazano na poniższym zrzucie ekranu:

![](hello-ios-deepdive-images/vs-image45.png "Aplikacja Phoneword ustawia ViewController jako kontroler widoku")

-----

To łączy scenorysu reprezentację kontroler widoku do `ViewController` klasy C#. Otwórz `ViewController.cs` plików i zwróć uwagę, kontroler widoku jest *podklasy* z `UIViewController`, jak pokazano w poniższym kodzie:

```csharp
public partial class ViewController : UIViewController
{
    public ViewController (IntPtr handle) : base (handle)
    {

    }
}
```

`ViewController` Teraz dyski interakcji hierarchii widok zawartości skojarzone z tym kontrolerem widoku w scenorysu. Następnie dowiesz się rolę kontrolera widoku w zarządzaniu widoki dzięki zastosowaniu w procesie nazywanym cyklu życia widoku.

> [!NOTE]
> **Uwaga:** dla tylko visual ekrany, które nie wymagają interakcji użytkownika, **klasy** właściwość może być pusty w **konsoli właściwości**. To ustawienie jako domyślna Implementacja klasy zapasowy kontroler widoku `UIViewController`, które jest odpowiednie, jeśli nie jest planowane dodanie niestandardowego kodu.

### <a name="view-lifecycle"></a>Cykl życia widoku

Kontroler widoku jest odpowiedzialny za ładowanie i zwalnianie zawartości hierarchie widok z okna. Znaczenie najważniejsza do widoku w hierarchii widok zawartości, system operacyjny powiadamia kontroler widoku za pomocą zdarzeń w cyklu życia widoku. Przez zastąpienie metody w cyklu życia widoku, może współpracować z obiektów na ekranie i Tworzenie interfejsu użytkownika dynamicznych, dynamiczne.

Są to metody podstawowy cykl życia i ich funkcji:

-  **ViewDidLoad** — wywołane *po* po raz pierwszy kontroler widoku ładuje hierarchii widok zawartości do pamięci. To jest dobrym miejscem do początkowej konfiguracji, ponieważ jest on widoków podrzędnych najpierw stają się dostępne w kodzie.
-  **ViewWillAppear** -wywoływana za każdym razem, gdy widok kontrolera widoku zostanie dodany do zawartości wyświetlanie hierarchii i są wyświetlane na ekranie.
-  **ViewWillDisappear** -wywoływana za każdym razem, gdy widok kontrolera widoku zostanie usunięta z hierarchii widok zawartości i są usuwane z ekranu. To zdarzenie cyklu życia służy do oczyszczania i Zapisywanie stanu.
-  **ViewDidAppear** i **ViewDidDisappear** -wywoływane, gdy widok pobiera dodane lub usunięte z hierarchią widok zawartości odpowiednio.


Po dodaniu niestandardowych kodów na dowolnym etapie cyklu życia, metoda cyklu życia w *Podstawowa implementacja* musi być *zastąpiona*. Jest to osiągane przez naciśnięcie przycisku do istniejącą metodę cyklu życia jest już dołączony do niej kodu, i rozszerzanie jej dodatkowy kod. Podstawowa implementacja jest wywoływana z wewnątrz metody, aby upewnić się, że oryginalny kod jest uruchamiany przed nowy kod. Na przykład jest przedstawiona w następnej sekcji.

Aby uzyskać więcej informacji na temat pracy z widoku kontrolerów odwoływać się do firmy Apple [widoku kontrolera Programming Guide dla systemu iOS](https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/ViewLoadingandUnloading/ViewLoadingandUnloading.html) i [odwołania UIViewController](https://developer.apple.com/library/ios/documentation/uikit/reference/UIViewController_Class/Reference/Reference.html).

### <a name="responding-to-user-interaction"></a>Odpowiada na żądania interakcji z użytkownikiem

Najważniejsze roli kontrolera widoku odpowiada na żądania interakcji użytkownika, takich jak naciśnięcie przycisków nawigacji i inne. Najprostszym sposobem obsługi interakcji z użytkownikiem jest okablować Góra formantu słuchać użytkownika argument wejściowy i dołączyć program obsługi zdarzeń, aby odpowiedzieć na dane wejściowe. Na przykład przycisk można można przewodowej się odpowiedzieć na zdarzenie touch, jak pokazano w aplikacji Phoneword.

Teraz, aby mieć lepiej zrozumieć widokach i kontrolerach widoku, Przyjrzyjmy się, jak to działa.
W `Phoneword_iOS` projektu, przycisk dodano o nazwie `TranslateButton` do hierarchii widok zawartości:

 [![](hello-ios-deepdive-images/image1.png "Przycisk dodano wywołane TranslateButton hierarchii widoku zawartości")](hello-ios-deepdive-images/image1.png#lightbox)

Gdy **nazwa** jest przypisany do **przycisk** kontroli w **właściwości konsoli**, projektanta iOS automatycznie mapowane go do kontroli w  **ViewController.designer.cs**, wprowadzania `TranslateButton` dostępne wewnątrz `ViewController` klasy. Formanty najpierw staną się dostępne w `ViewDidLoad` etapie cyklu życia widoku, więc ta metoda cyklu życia służy odpowiedzieć touch użytkownika:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // wire up TranslateButton here
}
```

Aplikacja Phoneword używa zdarzeń touch, nazywany `TouchUpInside` słuchać touch użytkownika. `TouchUpInside` wykrywa touch się zdarzenia (palca podnoszenia odniosło) znajdujący się w granicach formantu touch w dół (palca dotknięcie ekranu). Przeciwieństwem `TouchUpInside` jest `TouchDown` zdarzenie, które są generowane, gdy użytkownik naciśnie w dół w formancie. `TouchDown` Zdarzeń przechwytuje dużo hałasu i zapewnia możliwość anulowania touch przez przesuwanie ich palca poza formantu. `TouchUpInside` jest najczęściej odpowiedzieć na **przycisk** touch i tworzy środowisko użytkownika oczekuje po naciśnięcie przycisku. Więcej informacji na ten jest dostępny w firmy Apple [iOS Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html).

Aplikacja obsługi `TouchUpInside` zdarzenie z wyrażenia lambda, ale delegata lub obsługi zdarzenia o nazwie można również zostały użyte. Końcowe kod przycisku przypominających następujące czynności:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
      translatedNumber = Core.PhonewordTranslator.ToNumber(PhoneNumberText.Text);
      PhoneNumberText.ResignFirstResponder ();

      if (translatedNumber == "") {
        CallButton.SetTitle ("Call", UIControlState.Normal);
        CallButton.Enabled = false;
      } else {
        CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
        CallButton.Enabled = true;
      }
  };
}
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Dodatkowe założenia Phoneword

Aplikacja Phoneword wprowadzono kilka koncepcji, które nie są uwzględnione w tym przewodniku. Pojęcia te obejmują:

- **Zmień tekst przycisku** — aplikacji Phoneword wykazać, jak zmienić tekst **przycisk** przez wywołanie metody `SetTitle` na **przycisk** i przekazując nowego tekstu i  **Przycisk**w _stan kontrolki_. Na przykład poniższy kod zmienia tekst CallButton "Wywołania":

    ```csharp
    CallButton.SetTitle ("Call", UIControlState.Normal);
    ```
- **Włączanie i wyłączanie przycisków** — **przyciski** może znajdować się w `Enabled` lub `Disabled` stanu. A wyłączone **przycisk** nie będzie uwzględniał dane wejściowe użytkownika. Na przykład poniższy kod wyłącza `CallButton`: CallButton.Enabled = false; Aby uzyskać więcej informacji na przyciskach dotyczą [przyciski](~/ios/user-interface/controls/buttons.md) przewodnik.
- **Odrzuć klawiatury** — po podsłuchu użytkownika pola tekstowego, system iOS Wyświetla klawiatury, aby umożliwić użytkownikowi wprowadzenie danych wejściowych. Niestety nie ma żadnych wbudowanych funkcji, aby odrzucić klawiatury. Następujący kod został dodany do `TranslateButton` aby odrzucić klawiatury, gdy użytkownik naciśnie `TranslateButton`: PhoneNumberText.ResignFirstResponder (); Innym przykładem odrzuceniu klawiatury, można znaleźć w temacie [odrzucić klawiatury](https://developer.xamarin.com/recipes/ios/input/keyboards/dismiss_the_keyboard) przepisu.
- **Miejsce połączenie telefoniczne z adresem URL** — w aplikacji Phoneword schemat adresu URL Apple jest używane do uruchomienia aplikacji phone systemu. Schemat niestandardowy adres URL składa się z "tel:" prefiks przetłumaczonego numerze telefonu i, jak pokazano w poniższym kodzie:

    ```csharp
    var url = new NSUrl ("tel:" + translatedNumber);
    if (!UIApplication.SharedApplication.OpenUrl (url))
    {
        // show alert Controller
    }
    ```
- **Pokaż Alert** — gdy użytkownik próbuje umieścić połączeń telefonicznych na urządzeniu, która nie obsługuje wywołań — na przykład symulator lub iPod Touch — okna dialogowego alertu jest wyświetlany użytkownikowi należy znać rozmowy telefonicznej nie może zostać umieszczona. Poniższy kod tworzy i wypełnia kontrolera alertu:

    ```csharp
    if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
    ```

Aby uzyskać więcej informacji na widoki alertów dla systemu iOS, zapoznaj się [przepisu kontrolera Alert](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/).

## <a name="testing-deployment-and-finishing-touches"></a>Testowanie wdrażania i ostateczne poprawki

Zarówno program Visual Studio dla komputerów Mac, jak i programu Visual Studio oferują wiele opcji testowania i wdrażania aplikacji. W tej sekcji opisano opcje debugowania, przedstawiono testowanie aplikacji na urządzeniu i wprowadza narzędzia do tworzenia, uruchamiania obrazów i ikon niestandardowych aplikacji.

### <a name="debugging-tools"></a>Narzędzia debugowania

Czasami problemów w kodzie aplikacji są trudne do diagnozowania. Aby ułatwić diagnozowanie problemów złożonego kodu, można [Ustaw punkt przerwania](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [kod za pomocą kroku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/), lub [dane wyjściowe do okna dziennika](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).

### <a name="deploy-to-a-device"></a>Wdrażanie do urządzenia

Symulatora systemu iOS jest szybkim sposobem testowania aplikacji. Symulator ma liczbę optymalizacje przydatne do testowania, zasymulować lokalizacji, w tym [symulowanie przepływu](https://developer.xamarin.com/recipes/ios/multitasking/test_location_changes_in_simulator/)itd. Jednak użytkownicy nie będą korzystać z ostatecznej aplikacji w symulatorze. Wszystkie aplikacje powinny być testowane na urządzeniach prawdziwe, wczesne i często.

Urządzenie czas do udostępniania i wymaga konta dewelopera firmy Apple. [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) przewodniku znajdują się szczegółowe instrukcje na przygotowanie urządzenie do programowania.

> [!NOTE]
> **Uwaga:** w chwili obecnej, ze względu na wymagania firmy Apple, należy mieć certyfikatu deweloperskiego lub _tożsamości podpisywania_ do kompilacji kodu dla urządzenie lub symulator. Postępuj zgodnie z instrukcjami [Inicjowanie obsługi administracyjnej urządzeń przewodnik](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) tej konfiguracji.

Po zainicjowaniu obsługi urządzenia, można wdrożyć do niej przez podłączenie w zmiana obiekt docelowy w pasku narzędzi kompilacji na urządzeniu z systemem iOS, a następnie naciskając klawisz **Start** ( **odtwarzanie**) jak pokazano na poniższym zrzucie ekranu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image46new.png "Naciskając klawisz Start/Play")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image46.png "Naciskając klawisz Start/Play")

-----

Aplikacja zostanie wdrożona na urządzeniu z systemem iOS:

[![](hello-ios-deepdive-images/image1.png "Wdroży na urządzeniu z systemem iOS i uruchamianie aplikacji")](hello-ios-deepdive-images/image1.png#lightbox)

### <a name="generate-custom-icons-and-launch-images"></a>Generowanie ikon niestandardowych i uruchamianie obrazów

Nie zawsze ma dostępne do tworzenia ikon niestandardowych i uruchamianie obrazów, które aplikacja powinna wyróżniające projektanta. Poniżej przedstawiono kilka innych sposobów generowania aplikacji niestandardowej kompozycji:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- [**Rysunek** ](https://www.sketchapp.com") — schemat jest to aplikacja Mac za projektowanie interfejsów użytkownika, ikony i inne. To jest aplikacja, który został użyty do zaprojektowania zestaw ikony aplikacji platformy Xamarin i uruchamianie obrazów. Rysunek 3 jest dostępna w sklepie z aplikacjami. Można wypróbować wolnych [narzędzie szkicu](http://bohemiancoding.com/sketch/tool/) również.
- [**Pixelmator** ](http://www.pixelmator.com/) — uniwersalne obrazu, edytowanie aplikacji dla komputerów Mac, który koszty około 30 $.
- [**Glyphish** ](http://www.glyphish.com/) — ikona wbudowane wysokiej jakości bezpłatnie ustawia pobierania i zakupu.
- [**Fiverr** ](http://www.fiverr.com/) — możliwość wyboru z szerokiej gamy Designer, aby utworzyć ikonę ustawiane, zaczynając od wynosi 5.  Może być hit lub miss, ale dobrym zasobu, jeśli potrzebujesz ikony przeznaczony na bieżąco

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

* Visual Studio — można to utworzyć prosty ikonę dla aplikacji bezpośrednio w środowisku IDE.
- [**Glyphish** ](http://www.glyphish.com/) — ikona wbudowane wysokiej jakości bezpłatnie ustawia pobierania i zakupu.
- [**Fiverr** ](http://www.fiverr.com/) — możliwość wyboru z szerokiej gamy Designer, aby utworzyć ikonę ustawiane, zaczynając od wynosi 5.  Może być hit lub miss, ale dobrym zasobu, jeśli potrzebujesz ikony przeznaczony na bieżąco

-----

Aby uzyskać więcej informacji na temat ikonę, a następnie uruchom rozmiary obrazów i wymagania dotyczą [Praca z obrazami przewodnik](~/ios/app-fundamentals/images-icons/index.md).

## <a name="summary"></a>Podsumowanie

Gratulacje! Obecnie masz pełny opis składników aplikacji platformy Xamarin.iOS, a także narzędzia służące do ich tworzenia.
W [następny samouczek z serii wprowadzenie](~/ios/get-started/hello-ios-multiscreen/index.md), można rozszerzać naszą aplikację do obsługi wielu ekranów. Wzdłuż sposób będzie wdrożenia kontrolera nawigacji, Dowiedz się więcej o Segues scenorysu i powodować modelu widoku, kontroler (MVC) wzorca poszerzają naszą aplikację do obsługi wielu ekranów.


## <a name="related-links"></a>Linki pokrewne

- [Witaj, iOS (przykład)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [System iOS w portalu inicjowania obsługi](https://developer.apple.com/ios/manage/overview/index.action)
