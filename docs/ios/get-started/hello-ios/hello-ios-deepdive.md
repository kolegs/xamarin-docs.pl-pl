---
title: Witaj, iOS — szczegółowe informacje
description: W tym dokumencie przyjmuje się bardziej Hello, z systemem iOS przykładową aplikację, biorąc pod uwagę jego architekturę, interfejs użytkownika, hierarchia widok zawartości, testowania, wdrażania i inne.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 61ba3a7e-fe11-4439-8bc8-9809512b8eff
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 16920f27a1830dc6a3ab1a3cb0a267eb3b1d90ea
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203026"
---
# <a name="hello-ios--deep-dive"></a>Witaj, iOS — szczegółowe informacje

Przewodnik Szybki Start, wprowadzono kompilowanie i uruchamianie podstawowej aplikacji platformy Xamarin.iOS. Teraz nadszedł czas na programowanie lepiej zrozumieć, jak działają aplikacji systemu iOS, dzięki czemu można tworzyć bardziej zaawansowanych programów. Ten przewodnik sprawdza kroki w Witaj, iOS przewodnika w celu włączenia wiedzę na temat podstawowych pojęć dotyczących opracowywania aplikacji dla systemu iOS.

Ten przewodnik ułatwi Ci rozwijać umiejętności i wiedzy, które są wymagane do tworzenia aplikacji dla systemu iOS z jednym ekranem. Po zakończeniu pracy przez nią, należy zrozumieć różne części aplikacji platformy Xamarin.iOS i jak one współdziałają ze sobą.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Wprowadzenie do programu Visual Studio dla komputerów Mac

Program Visual Studio dla komputerów Mac jest bezpłatne, typu open-source IDE, który łączy funkcje programu Visual Studio i narzędziu XCode. Zawiera funkcje, w pełni zintegrowane wizualnego projektanta, edytora tekstów, pełną za pomocą narzędzi refaktoryzacji, przeglądarka zestawów i integracji kodu źródłowego. Ten przewodnik przedstawia niektóre podstawowe programu Visual Studio dla komputerów Mac funkcji, ale jeśli jesteś nowym użytkownikiem programu Visual Studio dla komputerów Mac, zapoznaj się z [programu Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/) dokumentacji.

Program Visual Studio for Mac następuje rozwiązanie programu Visual Studio kod do organizowania *rozwiązania* i *projektów*. To rozwiązanie jest kontenerem, który może zawierać jeden lub więcej projektów. Projekt może być aplikacji (np. iOS lub Android), biblioteka pomocnicze, aplikacja testowa i. W aplikacji Phoneword nowym telefonie iPhone projekt został dodany za pomocą **aplikacja pojedynczego widoku** szablonu. Początkowe rozwiązania zapoznaniu się następująco:

![](hello-ios-deepdive-images/image30.png "Zrzut ekranu przedstawiający początkowej rozwiązania")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Wprowadzenie do programu Visual Studio

Visual Studio to zaawansowane środowisko IDE firmy Microsoft. Zawiera funkcje, w pełni zintegrowane wizualnego projektanta, edytora tekstów, pełną za pomocą narzędzi refaktoryzacji, przeglądarka zestawów i integracji kodu źródłowego. Ten przewodnik wprowadzenie niektórych podstawowych funkcji programu Visual Studio za pomocą platformy Xamarin wtyczki.

Program Visual Studio umożliwia organizowanie kodu w _rozwiązania_ i *projektów*. To rozwiązanie jest kontenerem, który może zawierać jeden lub więcej projektów. Projekt może być aplikacji (np. iOS lub Android), biblioteka pomocnicze, aplikacja testowa i. W aplikacji Phoneword nowym telefonie iPhone projekt został dodany za pomocą **aplikacja pojedynczego widoku** szablonu. Początkowe rozwiązania zapoznaniu się następująco:

![](hello-ios-deepdive-images/vs-image30.png "Zrzut ekranu przedstawiający początkowej rozwiązania")

-----

## <a name="anatomy-of-a-xamarinios-application"></a>Anatomia aplikacji platformy Xamarin.iOS

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po lewej stronie jest *konsoli rozwiązania*, która zawiera strukturę katalogów i wszystkie pliki skojarzone z rozwiązania:

![](hello-ios-deepdive-images/image31.png "W konsoli rozwiązania, która zawiera strukturę katalogów oraz ich wszystkie pliki, które są skojarzone z rozwiązaniem")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po prawej stronie jest *okienko rozwiązania*, która zawiera strukturę katalogów i wszystkie pliki skojarzone z rozwiązania:

![](hello-ios-deepdive-images/vs-image31.png "W okienku rozwiązania, która zawiera strukturę katalogów oraz ich wszystkie pliki, które są skojarzone z rozwiązaniem")

-----

W [Witaj, iOS](~/ios/get-started/hello-ios/hello-ios-quickstart.md) przewodnika, możesz tworzyć rozwiązania o nazwie **Phoneword** i jest umieszczany systemu iOS projekt — **Phoneword_iOS** — wewnątrz niego. Elementy w projekcie obejmują:

-  **Odwołania** — zawiera zestawy wymagany do kompilowania i uruchamiania aplikacji. Rozwiń katalog, aby zobaczyć odwołania do zestawów .NET, takich jak [systemu](http://msdn.microsoft.com/library/system%28v=vs.110%29.aspx) , System.Core, i [System.Xml](http://msdn.microsoft.com/library/system.xml%28v=vs.110%29.aspx) , a także odwołania do zestawu platformy Xamarin.iOS platformy Xamarin.
-  **Pakiety** — katalog pakietów zawiera gotowe pakiety NuGet.
-  **Zasoby** -folder zasobów przechowuje innych nośników.
-  **Main.cs** — zawiera główny punkt wejścia aplikacji. Aby uruchomić aplikację, nazwa klasy głównej aplikacji `AppDelegate`, jest przekazywany w.
-  **AppDelegate.cs** — ten plik zawiera klasę głównego aplikacji i jest odpowiedzialny za utworzenie okna, tworzenia interfejsu użytkownika i nasłuchuje zdarzeń z systemu operacyjnego.
-  **Main.Storyboard** -scenorysu zawiera projektowania wizualnego interfejsu użytkownika aplikacji. Scenorysu pliki otwarte w edytorze graficznym o nazwie narzędzia iOS Designer.
-  **ViewController.cs** — widok kontroler obsługuje ekranu (Widok), który użytkownik będzie widział i dotyka. Kontroler widoku jest odpowiedzialny za obsługę interakcji między użytkownikiem i widoku.
-  **ViewController.designer.cs** — `designer.cs` jest wygenerowany automatycznie plik służący jako pośredniczącego między kontrolkami w widoku i ich reprezentacje kod na kontrolerze widoku. Jest to plik nadmiar wewnętrznego, IDE spowoduje zastąpienie wszelkie ręczne zmiany i w większości przypadków, który można zignorować ten plik. Aby uzyskać więcej informacji na temat relacji między Projektant wizualny oraz kod zapasowy, zobacz [wprowadzenie do narzędzia iOS Designer](~/ios/user-interface/designer/introduction.md) przewodnik.
-  **Plik info.plist** — `Info.plist` jest, gdzie są ustawione właściwości aplikacji, takie jak nazwa aplikacji, ikony, obrazy uruchamiania i. Jest to plik zaawansowane i dokładne wprowadzenie do niego jest dostępny w [Praca z listy właściwości](~/ios/app-fundamentals/property-lists.md) przewodnik.
-  **Plik Entitlements.plist** — lista właściwości uprawnień pozwala nam określić aplikację *możliwości* (nazywane również App Store technologii) takich jak usługi iCloud, PassKit i inne. Więcej informacji na temat `Entitlements.plist` znajdują się w [Praca z listy właściwości](~/ios/app-fundamentals/property-lists.md) przewodnik. Aby uzyskać ogólne wprowadzenie do uprawnień, zobacz [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) przewodnik.

## <a name="architecture-and-app-fundamentals"></a>Podstawy architektury i aplikacji

Zanim aplikacji systemu iOS można załadować interfejsu użytkownika, dwie rzeczy muszą znajdować się w miejscu. Po pierwsze, aplikacja musi zdefiniować *punktu wejścia* — pierwszy kod, który jest uruchamiany, gdy proces aplikacji jest ładowany do pamięci. Po drugie musi definiować klasy do obsługi zdarzeń w całej aplikacji i wchodzić w interakcje z systemem operacyjnym.

W tej sekcji badań na relacje zilustrowane na poniższym diagramie:

[![](hello-ios-deepdive-images/image32.png "Relacje architektury i podstawowe informacje dotyczące aplikacji zostały zilustrowane na poniższym diagramie")](hello-ios-deepdive-images/image32.png#lightbox)

Przejdźmy rozpoczynają się od początku i Dowiedz się, co dzieje się po uruchomieniu aplikacji.

### <a name="main"></a>Main

Główny punkt wejścia aplikacji dla systemu iOS jest `Main.cs` pliku. `Main.cs` zawiera statycznej metody Main, które tworzy nowe wystąpienie aplikacji platformy Xamarin.iOS i przekazuje nazwę *delegata aplikacji* klasy, która będzie obsługiwać zdarzenia systemu operacyjnego. Kod szablonu dla statycznych `Main` metoda pojawia się poniżej:

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

W systemie iOS *delegata aplikacji* klasa obsługuje zdarzenia systemowe; ta klasa znajduje się wewnątrz `AppDelegate.cs`. `AppDelegate` Klasy zarządza stosowaniem *okna*. Okno jest pojedyncze wystąpienie `UIWindow` klasę, która służy jako kontener dla interfejsu użytkownika. Domyślnie aplikacja uzyskuje tylko jedno okno, do którego można załadować jej zawartość, a okno jest dołączone do *ekranu* (pojedynczy `UIScreen` wystąpienia) zapewniającej prostokąt otaczający dopasowania wymiary fizyczne ekran urządzenia.

*Elemencie AppDelegate* jest również odpowiedzialny za subskrypcję do aktualizacji systemu o zdarzeniach ważnych aplikacji, np. po jej zakończeniu, uruchamiania lub gdy jest za mało pamięci.

Poniżej przedstawiono kod szablonu w elemencie AppDelegate:

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

Po zdefiniowaniu jego okna aplikacji można rozpocząć, ładowanie interfejsu użytkownika. Następnej sekcji przedstawiono tworzenie interfejsu użytkownika.

## <a name="user-interface"></a>Interfejs użytkownika

Interfejs użytkownika aplikacji systemu iOS przypomina storefront — aplikacja pobiera zazwyczaj jedno okno, ale go wypełnić okno wiele obiektów, co wymaga, a obiekty i ich rozmieszczenia można zmienić w zależności od tego, co aplikacja chce, aby wyświetlić. Obiekty w tym scenariuszu - rzeczy, które widzi użytkownik -, są nazywane widoków. Aby skompilować na jednym ekranie, w aplikacji, widoki są ułożone jeden na drugim w *zawartości Wyświetl hierarchię*, i hierarchii jest zarządzany przez pojedynczy kontroler widoku. Aplikacje z wieloma ekranami może mieć wielu zawartości widoku hierarchii, każdy z własną kontrolera widoku, a następnie aplikacja umieszcza widoków w oknie do tworzenia różnych hierarchii widok zawartości oparty na ekranie, której należy użytkownik.

W tej sekcji zagłębił się w interfejsie użytkownika poprzez opisanie widoków, zawartości hierarchie z widoku lub narzędzia iOS Designer.

### <a name="ios-designer-and-storyboards"></a>System iOS Designer i scenorysów

IOS Designer jest wizualne narzędzia do tworzenia interfejsów użytkownika w środowisku Xamarin. Projektant można uruchamiać przez dwukrotne kliknięcie każdego pliku scenorysu (.storyboard), co spowoduje otwarcie widoku, który przypomina poniższy zrzut ekranu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image33.png "Projektant interfejsu systemu iOS")

A *scenorysu* jest plik, który zawiera projekty visual ekranów do naszej aplikacji, a także przejścia i relacje między ekranami. Reprezentacja ekranu aplikacji w Scenorys jest nazywany _sceny_. Każdy sceny reprezentuje kontroler widoku i stos widoków zarządzania (hierarchia widok zawartości). Gdy nowy **aplikacja pojedynczego widoku** projekt jest tworzony na podstawie szablonu, Visual Studio dla komputerów Mac automatycznie generuje plik scenorysu o nazwie `Main.storyboard` i wypełnia ją za pomocą pojedynczego sceny, jak pokazano na zrzucie ekranu poniżej:

![](hello-ios-deepdive-images/image34.png "Visual Studio dla komputerów Mac generuje plik scenorysu o nazwie Main.storyboard i automatycznie wypełnia pojedynczego sceny")

Czarne pasku w dolnej części ekranu scenorysu można wybrać, aby wybrać kontroler widoku dla sceny. Kontroler widoku jest wystąpieniem `UIViewController` klasę, która zawiera kod zapasowego dla hierarchii widok zawartości. Właściwości na tym kontrolerze widoku można wyświetlać i ustawić wewnątrz **konsoli właściwości**, jak pokazano na poniższym zrzucie ekranu:

![](hello-ios-deepdive-images/image35.png "Zawartość okienka właściwości")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image33.png "Projektant interfejsu systemu iOS")

A *scenorysu* jest plik, który zawiera projekty visual ekranów do naszej aplikacji, a także przejścia i relacje między ekranami. Reprezentacja ekranu aplikacji w Scenorys jest nazywany _sceny_. Każdy sceny reprezentuje kontroler widoku i stos widoków zarządzania (hierarchia widok zawartości). Gdy nowy **aplikacja pojedynczego widoku** projekt jest tworzony na podstawie szablonu, program Visual Studio automatycznie generuje plik scenorysu o nazwie `Main.storyboard` i wypełnia ją za pomocą pojedynczego sceny, jak pokazano na poniższym zrzucie ekranu:

![](hello-ios-deepdive-images/vs-image34.png "Program Visual Studio automatycznie generuje plik scenorysu o nazwie Main.storyboard i wypełnia ją za pomocą pojedynczego sceny")

Na pasku w dolnej części ekranu scenorysu można wybrać, aby wybrać kontroler widoku dla sceny. Kontroler widoku jest wystąpieniem `UIViewController` klasę, która zawiera kod zapasowego dla hierarchii widok zawartości. Właściwości na tym kontrolerze widoku można wyświetlać i ustawić wewnątrz **w okienku właściwości**, jak pokazano na poniższym zrzucie ekranu:

![](hello-ios-deepdive-images/vs-image35.png "Zawartość okienka właściwości")

-----

_Widoku_ można wybrać, klikając pozycję wewnątrz biała część sceny. Widok jest wystąpieniem `UIView` klasy, która definiuje obszar ekranu i zapewnia interfejsy do pracy z zawartością, w tym obszarze. Domyślny widok jest pojedynczym *widokiem głównym* , wypełnia ekran całym urządzeniem.

Po lewej stronie sceny jest Szara strzałka z ikoną flagi, jak pokazano na poniższym zrzucie ekranu:

 [![](hello-ios-deepdive-images/image37.png "Szara strzałka z ikoną flagi")](hello-ios-deepdive-images/image37.png#lightbox)

Szara strzałka reprezentuje przejście scenorysu o nazwie *Segue* (wymawiane "seg — sposób"). Ponieważ ta Segue nie źródła, jest nazywany *Sourceless Segue*. Sourceless Segue wskazuje na pierwszy sceny, w których widoki pobiera załadowane do naszej aplikacji okna przy uruchamianiu aplikacji. Sceny i widoki wewnątrz niej będą najpierw, użytkownik zobaczy podczas ładowania aplikacji.

Podczas tworzenia interfejsu użytkownika, dodatkowe widoki mogą być przeciągnięte z **przybornika** na widok główny na powierzchni projektowej, jak pokazano na poniższym zrzucie ekranu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image38.png "Dodatkowe widoki mogą być przeciągnięte z przybornika na widok główny na powierzchni projektowej")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image38.png "Dodatkowe widoki mogą być przeciągnięte z przybornika na widok główny na powierzchni projektowej")

-----

Te dodatkowe widoki są nazywane *widoków podrzędnych*. Razem widoku głównego i widoków podrzędnych są częścią *zawartości Wyświetl hierarchię* zarządzanym przez `ViewController`. Konspekt wszystkie elementy w scenie, mogą być wyświetlane, badając ją w **konspekt dokumentu** konsoli:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image39.png "Konsola konspekt dokumentu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image39.png "Konsola konspekt dokumentu")

-----

Widoków podrzędnych, zostały wyróżnione na poniższym diagramie:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image40.png "Widoków podrzędnych są wyróżnione na diagramie")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image40.png "Widoków podrzędnych są wyróżnione na diagramie")

-----

Następna sekcja dzieli zawartość Wyświetl hierarchię, reprezentowane przez ten sceny.

## <a name="content-view-hierarchy"></a>Hierarchia widoku zawartości

A _zawartości Wyświetl hierarchię_ to stos widoków i zarządzane przez kontrolera pojedynczego widoku widoków podrzędnych, jak pokazano na poniższym diagramie:

 [![](hello-ios-deepdive-images/image41.png "Hierarchia widoku zawartości")](hello-ios-deepdive-images/image41.png#lightbox)

Ułatwiamy zawartości widoku hierarchii naszych `ViewController` łatwiej zobaczyć tymczasowo zmieniając kolor tła głównego widok na żółty w sekcji widoku z **konsoli właściwości**, jak pokazano na poniższym zrzucie ekranu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image42.png "Zmiana koloru tła głównego widok na żółty w sekcji widoku konsoli do właściwości")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image42.png "Zmiana koloru tła głównego widok na żółty w sekcji widoku konsoli do właściwości")

-----

Poniższy diagram ilustruje relacje między oknem, widoki, widoków podrzędnych i kontroler widoku, które Przesuń interfejs użytkownika na ekranie urządzenia:

 [![](hello-ios-deepdive-images/image43.png "Relacje między okna, widoki, widoków podrzędnych i kontroler widoku")](hello-ios-deepdive-images/image43.png#lightbox)

W następnej sekcji omówiono sposób pracy z widoków w kodzie i Naucz się programować przy użyciu kontrolerów widoku i cyklem życia widok interakcji z użytkownikiem.

## <a name="view-controllers-and-the-view-lifecycle"></a>Cykl życia widoku i kontrolerów widoku

Każdy zawartości wyświetlanie hierarchii ma odpowiedni kontroler widoku na interakcję z użytkownikiem usługi power. Rola kontrolera widoku jest zarządzanie widokami w hierarchii widok zawartości. Kontroler widoku nie jest częścią hierarchii widok zawartości, a nie jest elementem w interfejsie. Przeciwnie zawiera kod, który obsługuje interakcji użytkownika z obiektów na ekranie.

### <a name="view-controllers-and-storyboards"></a>Scenorysy i kontrolerów widoku

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kontroler widoku jest reprezentowany w scenorysu pasek u dołu sceny. Wybiera kontroler widoku wyświetlenie jego właściwości w **konsoli właściwości**:

![](hello-ios-deepdive-images/image44.png "Wybiera kontroler widoku wyświetlenie jej właściwości na panelu Właściwości")

Można ustawić niestandardowe klasę kontroler widoku dla hierarchii widok zawartości, reprezentowane przez ten sceny, edytując **klasy** właściwość **tożsamości** części **konsoli właściwości**. Na przykład naszym **Phoneword** aplikacja ustawi `ViewController` jako kontroler widoku dla naszych pierwszego ekranu, jak pokazano na poniższym zrzucie ekranu:

![](hello-ios-deepdive-images/image45new.png "Aplikacja Phoneword ustawia ViewController jako kontroler widoku")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kontroler widoku jest reprezentowany w scenorysu pasek u dołu sceny. Wybiera kontroler widoku wyświetlenie jego właściwości w **w okienku właściwości**:

![](hello-ios-deepdive-images/vs-image44.png "Wybiera kontroler widoku wyświetlenie jej właściwości na panelu Właściwości")

Można ustawić niestandardowe klasę kontroler widoku dla hierarchii widok zawartości, reprezentowane przez ten sceny, edytując **klasy** właściwość **tożsamości** części **wokienkuwłaściwości**. Na przykład naszym **Phoneword** aplikacja ustawi `ViewController` jako kontroler widoku dla naszych pierwszego ekranu, jak pokazano na poniższym zrzucie ekranu:

![](hello-ios-deepdive-images/vs-image45.png "Aplikacja Phoneword ustawia ViewController jako kontroler widoku")

-----

W ten sposób scenorysu reprezentacja kontroler widoku do `ViewController` klasy C#. Otwórz `ViewController.cs` pliku i zwróć uwagę, jest kontroler widoku *podklasy* z `UIViewController`, co ilustruje poniższy kod:

```csharp
public partial class ViewController : UIViewController
{
    public ViewController (IntPtr handle) : base (handle)
    {

    }
}
```

`ViewController` Teraz dyski interakcje hierarchii widok zawartości skojarzone z tego kontrolera widoku w scenorysu. Następnie dowiesz się o roli kontrolera widoku w zarządzaniu widoków, wprowadzając w procesie zwanym cyklu życia widoku.

> [!NOTE]
> Dla tylko do wizualizacji ekranów, które nie wymagają interakcji z użytkownikiem **klasy** właściwość może być puste w **konsoli właściwości**. To ustawienie jako domyślna Implementacja klasy zapasowy kontroler widoku `UIViewController`, która jest odpowiednia, jeśli użytkownik nie jest planowana na dodawanie kodu niestandardowego.

### <a name="view-lifecycle"></a>Cykl życia widoku

Kontroler widoku jest odpowiedzialne za ładowanie i zwalnianie zawartości hierarchie widok z okna. Znaczenie najważniejsza do widoku w hierarchii widok zawartości, system operacyjny powiadamia kontrolera widoku za pomocą zdarzeń w cyklu życia widoku. Poprzez zastąpienie metody w cyklu życia widok, można korzystać z obiektów na ekranie i tworzymy interfejs użytkownika dynamicznych, interaktywnych.

Poniżej przedstawiono metody podstawowy cykl życia i ich funkcji:

-  **ViewDidLoad** — wywoływany *po* po raz pierwszy kontroler widoku ładuje hierarchii widok zawartości do pamięci. Jest to dobre miejsce do początkowej instalacji, ponieważ jest on widoków podrzędnych najpierw stają się dostępne w kodzie.
-  **ViewWillAppear** -wywoływana za każdym razem, gdy widok kontrolera widoku zostanie dodany do zawartości Wyświetl hierarchię i są wyświetlane na ekranie.
-  **ViewWillDisappear** -wywoływana za każdym razem, gdy widok kontrolera widoku zostanie usunięta z hierarchii widok zawartości i są usuwane z ekranu. To zdarzenie cyklu życia służy do oczyszczania i Zapisywanie stanu.
-  **ViewDidAppear** i **ViewDidDisappear** -wywoływana, gdy widok pobiera dodane lub usunięte z hierarchii widok zawartości, odpowiednio.


Po dodaniu niestandardowego kodu na każdym etapie cyklu życia tej metody cyklu życia firmy *implementację podstawową* musi być *przesłonięta*. Jest to osiągane przez sięgnięcie do istniejącej metody cyklu życia, która ma jakiś kod już dołączone do niego, i rozszerzania jej przy użyciu dodatkowego kodu. Podstawowa implementacja jest wywoływana z wewnątrz metody, aby upewnić się, że oryginalny kod jest uruchamiany zanim nowy kod. Na przykład jest przedstawiona w następnej sekcji.

Aby uzyskać więcej informacji na temat pracy z kontrolerów widoku odnoszą się do firmy Apple [przewodnik programowania w widoku kontrolera dla systemu iOS](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1) i [odwołania UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller?language=objc).

### <a name="responding-to-user-interaction"></a>Reagowanie na interakcję z użytkownikiem

Najważniejsze roli kontrolera widoku odpowiada na żądania interakcji użytkownika, takie jak naciśnięcie przycisku, nawigacji i nie tylko. Najprostszym sposobem obsługi interakcji użytkownika jest połączenie w górę kontrolki do nasłuchiwania użytkownika danych wejściowych i dołączyć program obsługi zdarzeń, aby odpowiedzieć na dane wejściowe. Na przykład przycisk może być gotowe i reagowanie na zdarzenia dotykowe, jak pokazano w aplikacji Phoneword.

Skoro już istnieje, ma lepiej zrozumieć, widoków i kontrolerów widoku, Przyjrzyjmy się, jak to działa.
W `Phoneword_iOS` projektu, przycisk został dodany o nazwie `TranslateButton` do hierarchii widok zawartości:

 [![](hello-ios-deepdive-images/image1.png "Przycisk został dodany o nazwie TranslateButton do hierarchii widok zawartości")](hello-ios-deepdive-images/image1.png#lightbox)

Gdy **nazwa** jest przypisany do **przycisk** w kontrolce **konsoli właściwości**, narzędzia iOS designer automatycznie mapowane go do formantu w  **ViewController.designer.cs**, a `TranslateButton` dostępna wewnątrz `ViewController` klasy. Formanty najpierw stają się dostępne w `ViewDidLoad` etap cyklu życia widoku, więc ta metoda cyklu życia jest używana na dotyk przez użytkownika:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // wire up TranslateButton here
}
```

Aplikacja Phoneword używa zdarzenia dotykowe, o nazwie `TouchUpInside` do nasłuchiwania dotykowego użytkownika. `TouchUpInside` nasłuchuje touch się zdarzenie (finger podnoszenia mieściły się na ekranie), znajdujący się w granicach formantu touch w dół (finger dotykanie ekranu). Przeciwieństwo `TouchUpInside` jest `TouchDown` zdarzenie, które są generowane, gdy użytkownik naciśnie klawisz na kontrolce. `TouchDown` Zdarzeń przechwytuje wiele szumu i daje możliwość anulowania na dotyk przez przesuwanie ich finger poza formant użytkownika. `TouchUpInside` to najbardziej popularny sposób, aby odpowiedzieć na **przycisk** touch i tworzy środowisko użytkownika oczekuje, że po naciśnięciu klawisza przycisk. Więcej informacji na ten jest dostępny w firmy Apple [iOS Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html).

Aplikacja obsługiwana `TouchUpInside` zdarzeń za pomocą wyrażenia lambda, ale delegata lub program obsługi zdarzeń o nazwie można również zostały użyte. Końcowe kod przycisku przypominających następujące czynności:

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

Aplikacja Phoneword wprowadzono kilka koncepcji, które nie są uwzględnione w tym przewodniku. Te pojęcia obejmują:

- **Zmień tekst przycisku** — aplikacji Phoneword pokazano, jak zmienić tekst **przycisk** przez wywołanie metody `SetTitle` na **przycisk** i przekazując nowy tekst i  **Przycisk**firmy _stan formantu_. Na przykład poniższy kod zmienia CallButton tekst "Wywołania":

    ```csharp
    CallButton.SetTitle ("Call", UIControlState.Normal);
    ```
- **Włączanie i wyłączanie przycisków** — **przyciski** mogą znajdować się w `Enabled` lub `Disabled` stanu. Jest wyłączona **przycisk** nie będzie odpowiadać na dane wejściowe użytkownika. Na przykład, poniższy kod wyłącza `CallButton`: CallButton.Enabled = false;. Aby uzyskać więcej informacji na temat przycisków, zobacz [przyciski](~/ios/user-interface/controls/buttons.md) przewodnik.
- **Odrzuć klawiatury** — po użytkownik podsłuchu w polu tekstowym dla systemu iOS Wyświetla klawiatury, aby zezwolić użytkownikom na wprowadzanie danych wejściowych. Niestety nie ma żadnych funkcji wbudowanych, aby odrzucić klawiatury. Poniższy kod jest dodawany do `TranslateButton` odrzucać klawiatury, gdy użytkownik naciśnie `TranslateButton`: PhoneNumberText.ResignFirstResponder (); Inny przykład odrzucanie klawiatury, można znaleźć [odrzucić klawiatury](https://github.com/xamarin/recipes/tree/master/Recipes/ios/input/keyboards/dismiss_the_keyboard) przepisu.
- **Połączenie telefoniczne miejscu za pomocą adresu URL** — w aplikacji Phoneword schemat adresu URL Apple służy do uruchamiania aplikacji phone systemu. Niestandardowy schemat adresu URL składa się z "tel:" prefiks przetłumaczone numerze telefonu i, co ilustruje poniższy kod:

    ```csharp
    var url = new NSUrl ("tel:" + translatedNumber);
    if (!UIApplication.SharedApplication.OpenUrl (url))
    {
        // show alert Controller
    }
    ```
- **Pokaż Alert** — po użytkownik próbuje umieścić połączenie telefoniczne na urządzeniu, które nie obsługują wywołania — na przykład symulatorze lub urządzeniu iPod Touch — okna dialogowego alertu jest wyświetlany, użytkownik nie może zostać umieszczona połączeń telefonicznych. Poniższy kod tworzy i wypełnia kontrolera alertu:

    ```csharp
    if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
    ```

Aby uzyskać więcej informacji na temat widoki alertów dla systemu iOS, zobacz [przepisu kontrolera Alert](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller).

## <a name="testing-deployment-and-finishing-touches"></a>Testowanie, wdrażanie i poprawek

Visual Studio dla komputerów Mac i Visual Studio oferują wiele opcji testowania i wdrażania aplikacji. W tej sekcji omówiono opcje debugowania, pokazuje testowania aplikacji na urządzeniu i wprowadza narzędzia służące do tworzenia, uruchamiania obrazów i ikon aplikacji niestandardowej.

### <a name="debugging-tools"></a>Narzędzia do debugowania

Czasami problemów w kodzie aplikacji są trudne do zdiagnozowania. Aby ułatwić diagnozowanie problemów z kodem złożonych, można wykonać następujące akcje [Ustaw punkt przerwania](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [kod za pomocą kroku](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code), lub [informacji wyjściowych w oknie dziennika](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

### <a name="deploy-to-a-device"></a>Wdrażanie na urządzeniu

W narzędziu iOS Simulator jest możliwość szybkiego testowania aplikacji. Symulator ma kilka optymalizacji przydatne w przypadku testowania makiety lokalizacji, w tym [symulowania przenoszenia](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/test_location_changes_in_simulator)i nie tylko. Jednak użytkownicy nie będą wymagały ostatecznej aplikacji w symulatorze. Powinien zostać przetestowany wszystkie aplikacje na prawdziwych urządzeniach, wcześnie i często.

Urządzenie zajmuje trochę czasu, aby aprowizować i konieczne jest posiadanie konta dewelopera firmy Apple. [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) przewodnik zawiera szczegółowe instrukcje na temat pobierania urządzenia gotowe do tworzenia aplikacji.

> [!NOTE]
> W chwili obecnej, ze względu na wymagania firmy Apple, należy mieć certyfikatu deweloperskiego lub _tożsamość do podpisywania_ do tworzenia kodu dla urządzenia lub symulatora. Postępuj zgodnie z instrukcjami w [Device Provisioning guide](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) o skonfigurowanie tego numeru.

Po aprowizacji urządzenia można wdrożyć do niego przez podłączenie go w zmiana docelowej, na pasku narzędzi kompilacji na urządzenia z systemem iOS, a następnie naciskając klawisz **Start** ( **Odtwórz**) jak pokazano na poniższym zrzucie ekranu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image46new.png "Naciśnięcie Start/odtwarzania")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image46.png "Naciśnięcie Start/odtwarzania")

-----

Aplikacja zostanie wdrożona na urządzeniu z systemem iOS:

[![](hello-ios-deepdive-images/image1.png "Aplikacja będzie wdrożyć na urządzeniu z systemem iOS i uruchomienie")](hello-ios-deepdive-images/image1.png#lightbox)

### <a name="generate-custom-icons-and-launch-images"></a>Generowanie niestandardowych ikon i obrazy uruchamiania

Nie wszyscy ma dostępne do tworzenia niestandardowych ikon i uruchamianie obrazów, czego potrzebuje aplikacji, aby wyróżnić projektanta. Oto kilka innych sposobów generowania kompozycji aplikacji niestandardowej:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- [**Szkic** ](https://www.sketchapp.com") — schemat jest aplikacja dla komputerów Mac dotyczące projektowania interfejsów użytkownika, ikon i nie tylko. To jest aplikacja, który został użyty do projektowania zestawu ikon aplikacji platformy Xamarin i uruchamianie obrazów. Rysunek 3 jest dostępna w Store aplikacji. Możesz wypróbować bezpłatną [narzędzie szkic](http://bohemiancoding.com/sketch/tool/) także.
- [**Pixelmator** ](http://www.pixelmator.com/) — wszechstronny obraz, edytowanie aplikacji dla komputerów Mac, który koszt wynosi około 30 USD.
- [**Glyphish** ](http://www.glyphish.com/) — ikona wbudowanych wysokiej jakości za darmo ustawia pobierania i możliwości zakupu.
- [**Fiverr** ](http://www.fiverr.com/) — możliwość wyboru z różnych projektantów do tworzenia ikony ustawiane, zaczynając od 5 USD.  Może być hit lub miss, ale odpowiedni zasób, jeśli potrzebujesz ikony przeznaczony na bieżąco

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

* Visual Studio — umożliwia to twórz proste ikony dla aplikacji bezpośrednio w środowisku IDE.
- [**Glyphish** ](http://www.glyphish.com/) — ikona wbudowanych wysokiej jakości za darmo ustawia pobierania i możliwości zakupu.
- [**Fiverr** ](http://www.fiverr.com/) — możliwość wyboru z różnych projektantów do tworzenia ikony ustawiane, zaczynając od 5 USD.  Może być hit lub miss, ale odpowiedni zasób, jeśli potrzebujesz ikony przeznaczony na bieżąco

-----

Aby uzyskać więcej informacji na temat ikonę, a następnie uruchom rozmiary obrazów i wymagań dotyczą [Praca z obrazami przewodnik](~/ios/app-fundamentals/images-icons/index.md).

## <a name="summary"></a>Podsumowanie

Gratulacje! Masz teraz pełny opis składniki aplikacji platformy Xamarin.iOS, a także narzędzia służące do ich tworzenia.
W [następny samouczek z serii wprowadzenie](~/ios/get-started/hello-ios-multiscreen/index.md), możesz rozszerzać naszej aplikacji do obsługi wielu ekranów. Po drodze będzie implementować kontrolera nawigacji, Dowiedz się więcej o serii ujęć przejść i wprowadzenie, widoku kontrolera MVC (Model) wzorca rozszerzaniu naszej aplikacji do obsługi wielu ekranów.


## <a name="related-links"></a>Linki pokrewne

- [Witaj, iOS (przykład)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/overview/themes/)
- [Portal Aprowizowania dla systemu iOS](http://developer.apple.com/account/#/overview)
