---
title: Karta paski i kontrolerów kart w Xamarin.iOS
description: W tym dokumencie opisano kontrolerów pasek kartę iOS i z nich korzystać z platformy Xamarin.iOS. Go pokazano, jak skonfigurować UITabBarController, Praca z obrazami, ustaw wartości wskaźnika, Praca ze zdarzeniami i inne.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: d8b096774e60ec0e0b69e109fa5da53c25e66d25
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789761"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Karta paski i kontrolerów kart w Xamarin.iOS

Aplikacje z kartami są używane w systemie iOS do obsługi interfejsu użytkownika, której można uzyskać dostęp do wielu ekranów w określonej kolejności. Za pomocą `UITabBarController` klasy, aplikacje można łatwo obejmują obsługę takich scenariuszy z wieloma ekranu. `UITabBarController` zapewnia obsługę zarządzania wieloma ekranu, dzięki czemu Deweloper aplikacji skoncentrować się na szczegóły każdego ekranu.

Zwykle z kartami aplikacje są tworzone za `UITabBarController` trwa `RootViewController` głównego okna. Jednak z bitowego kodu dodatkowe, aplikacje z kartami można również kolejno niektóre inne początkowej ekranu, takich jak scenariusz, w którym aplikacja najpierw przedstawia informacje na ekranie logowania, a następnie z kartami interfejsu.

Zajmiemy się, używając karty w tym miejscu pracy za pomocą wskazówki prostą aplikację. Następnie wyjaśniono, jak pracować z karty w nienależących `RootViewController` scenariusza.

## <a name="introducing-uitabbarcontroller"></a>Wprowadzenie do UITabBarController

`UITabBarController` Obsługuje tworzenie aplikacji na kartach przez następujące:

-  Zezwalanie na wielu kontrolerach do dodania do niego.
-  Udostępnia interfejs użytkownika z kartami za pośrednictwem `UITabBar` klasy, aby zezwolić użytkownikowi na przełączanie między ich widoków i kontrolerów. 


Kontrolery są dodawane do `UITabBarController` za pośrednictwem jego `ViewControllers` właściwość, która jest `UIViewController` tablicy. `UITabBarController` Sama obsługuje ładowanie prawidłowego kontrolera i umożliwienie korzystania z jego widoku ustalane na podstawie wybranej karty.

Karty są wystąpieniami klasy `UITabBarItem` klasy, które są zawarte w `UITabBar` wystąpienia. Każdy `UITabBar` wystąpienie jest dostępny za pośrednictwem `TabBarItem` właściwości kontrolera na każdej karcie.

Aby uzyskać opis sposobu pracy z `UITabBarController`, Przejdźmy tworzenia prostej aplikacji, która korzysta z jednego.

## <a name="tabbed-application-walkthrough"></a>Przewodnik po aplikacji z kartami

W ramach tego przewodnika zamierzamy utworzyć następujących aplikacji:

[![](creating-tabbed-applications-images/00-app.png "Przykładowa aplikacja z kartami")](creating-tabbed-applications-images/00-app.png#lightbox)

Ma już szablon z kartami aplikacji dostępnych w programie Visual Studio dla komputerów Mac, w tym przykładzie, ale chcemy się pracy z pustego projektu, aby lepiej zrozumieć, w jaki sposób jest tworzony aplikacji.

 <a name="Creating_the_Application" />


### <a name="creating-the-application"></a>Tworzenie aplikacji

Zacznijmy od utworzenia nowej aplikacji.

Wybierz **Plik > Nowy > rozwiązania** w programie Visual Studio for Mac i wybierz z menu **systemu iOS > aplikacji > pusty projekt** szablonu, nazwy projektu `TabbedApplication`, jak pokazano poniżej:

[![](creating-tabbed-applications-images/newsolution1.png "Wybierz szablon pustego projektu")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Nazwa projektu TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)



### <a name="adding-the-uitabbarcontroller"></a>Dodawanie UITabBarController

Następnie dodaj pustą klasę, wybierając **Plik > Nowy plik** i wybierając polecenie **ogólne: pustą klasę** szablonu. Nadaj nazwę plikowi `TabController` w sposób przedstawiony poniżej:

[![](creating-tabbed-applications-images/02-newclass.png "Dodaj klasę TabController")](creating-tabbed-applications-images/02-newclass.png#lightbox)

`TabController` Klasy będzie zawierać implementację `UITabBarController` która będzie zarządzać tablicę `UIViewControllers`. Gdy użytkownik wybierze kartę `UITabBarController` zajmie się przedstawienie widoku dla kontrolera widoku.

Aby zaimplementować `UITabBarController` należy wykonać następujące czynności:

1.  Ustaw klasy podstawowej z `TabController` do `UITabBarController` . 
1.  Utwórz `UIViewController` wystąpień do dodania do `TabController` . 
1.  Dodaj `UIViewController` wystąpień do tablicy przypisane do `ViewControllers` właściwość `TabController` . 


Dodaj następujący kod do `TabController` klasę, aby osiągnąć następujące kroki:

```csharp
using System;
using UIKit;

namespace TabbedApplication {
        public class TabController : UITabBarController {

                UIViewController tab1, tab2, tab3;

                public TabController ()
                {
                        tab1 = new UIViewController();
                        tab1.Title = "Green";
                        tab1.View.BackgroundColor = UIColor.Green;

                        tab2 = new UIViewController();
                        tab2.Title = "Orange";
                        tab2.View.BackgroundColor = UIColor.Orange;

                        tab3 = new UIViewController();
                        tab3.Title = "Red";
                        tab3.View.BackgroundColor = UIColor.Red;

                        var tabs = new UIViewController[] {
                                tab1, tab2, tab3
                        };

                        ViewControllers = tabs;
                }
        }
}
```

Zwróć uwagę, że dla każdego `UIViewController` wystąpienia, możemy ustawić `Title` właściwość `UIViewController`. Gdy kontrolery są dodawane do `UITabBarController`, `UITabBarController` odczyta `Title` dla każdego kontrolera i wyświetlić je na karcie skojarzone etykiety, jak pokazano poniżej:

[![](creating-tabbed-applications-images/00-app.png "Przykładowa aplikacja Uruchom")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Ustawienie TabController jako RootViewController

Kolejność, że kontrolery są umieszczane w kartach odpowiada kolejności ich dodawania do `ViewControllers` tablicy.

Aby uzyskać `UITabController` załadować jako pierwszy ekran, musimy stał się okna `RootViewController`, jak pokazano w poniższym kodzie dla `AppDelegate`:

```csharp
[Register ("AppDelegate")]
        public partial class AppDelegate : UIApplicationDelegate
        {
                UIWindow window;
                TabController tabController;

                public override bool FinishedLaunching (UIApplication app, NSDictionary options)
                {
                        window = new UIWindow (UIScreen.MainScreen.Bounds);

                        var tabController = new TabController ();
                        window.RootViewController = tabController;

                        window.MakeKeyAndVisible ();
            
                        return true;
                }
        }
```

Czy możemy uruchomić aplikację teraz `UITabBarController` zostanie załadowany z pierwszej karcie wybrane domyślnie. Wybranie dowolnego inne karty wyników w skojarzonego kontrolera wyświetlić jest przedstawiony przez `UITabBarController,` w sposób przedstawiony poniżej, gdy użytkownik końcowy został wybrany drugiej karty:

[![](creating-tabbed-applications-images/03-secondtab.png "Drugiej karcie wyświetlane")](creating-tabbed-applications-images/03-secondtab.png#lightbox)

 <a name="Modifying_TabBarItems" />


### <a name="modifying-tabbaritems"></a>Modyfikowanie TabBarItems

Teraz, gdy mamy uruchomionej aplikacji pozycję zmodyfikujmy `TabBarItem` zmienić obraz i tekst, który jest wyświetlany, a także dodać wskaźnika do jednej z kart.

 <a name="Setting_a_System_Item" />


#### <a name="setting-a-system-item"></a>Ustawienie elementu systemu

Po pierwsze umożliwia Ustaw pierwszą kartę, aby użyć elementu systemu. W Konstruktorze `TabController`, Usuń wiersz, który ustawia kontrolera `Title` dla `tab1` wystąpienia i Zastąp następujący kod, aby ustawić kontroler `TabBarItem` właściwości:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

Podczas tworzenia `UITabBarItem` przy użyciu `UITabBarSystemItem`, tytuł i obrazu są dostarczane automatycznie przez systemy iOS, jak pokazano na zrzucie ekranu poniżej pokazujący **ulubione** ikony, jak i tytuł na pierwszej karcie:

 ![](creating-tabbed-applications-images/04a-tabimage.png "Pierwszej karcie ikoną gwiazdki")

 <a name="Setting_the_Title_and_Image" />


#### <a name="setting-the-title-and-image"></a>Ustawianie tytuł i obrazu

Oprócz przy użyciu elementu systemu, tytuł i obraz `UITabBarItem` można ustawić wartości niestandardowych. Na przykład zmień kod, który ustawia `TabBarItem` właściwość o nazwie kontrolera `tab2` w następujący sposób:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

Powyższy kod zakłada obrazu o nazwie `second.png` zostały dodane do katalogu głównego projektu programu Visual Studio dla komputerów Mac. Dodaliśmy faktycznie trzy obrazy do naszej projektu, aby pokrywał rozwiązania wszystkie urządzenia, jak pokazano poniżej:

 [![](creating-tabbed-applications-images/tabbedimages7new.png "Obrazy dodane do projektu")](creating-tabbed-applications-images/tabbedimages7new.png#lightbox)

Karta obraz powinien być 30 x 30 png o przejrzystości normalne rozpoznawanie, 60 x 60 dla wysokiej rozdzielczości i 90 x 90 dla telefonu iPhone 6 Plus rozpoznawania. W naszym kodzie tylko należy załadować plik o nazwie `second.png` i z systemem iOS zostanie automatycznie załadowany wysokiej rozdzielczości, jeden na urządzeniach za pomocą wyświetlania siatkówki. Możesz przeczytać więcej na temat [Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md) przewodników. Domyślnie elementy paska kartę są szary, z niebieskim odcień w przypadku wybrania.

**Uwaga**

Obrazy powyżej można również dodać w **zasobów** katalogu, który jest katalogiem specjalne, którego zawartość zostaną automatycznie skopiowane do katalogu głównego pakietu aplikacji:

[![](creating-tabbed-applications-images/tabbedapplication8.png "Obrazy jako zasoby")](creating-tabbed-applications-images/tabbedapplication8.png#lightbox)

Ponadto, kiedy możemy ustawić `Title` właściwości bezpośrednio na `TabBarItem`, spowoduje zastąpienie wartości ustawione dla `Title` na kontrolerze samej siebie.

Uruchomienie aplikacji teraz, druga karta przedstawia naszych niestandardowy tytuł i obrazu w sposób przedstawiony poniżej:

[![](creating-tabbed-applications-images/05-customtab.png "Druga karta z wyświetlana kwadratowa ikona")](creating-tabbed-applications-images/05-customtab.png#lightbox)

 <a name="Setting_the_Badge_Value" />


#### <a name="setting-the-badge-value"></a>Ustawienie wartości wskaźnika

Karta można również wyświetlić wskaźnika. Na przykład dodaj następujący wiersz kodu można ustawić wskaźnika na karcie trzeci:

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

Uruchomiona wyniki red etykiety z ciągiem "Hi" w lewym górnym rogu kartę, jak pokazano poniżej:

[![](creating-tabbed-applications-images/06-badge.png "Druga karta z Hi wskaźnika")](creating-tabbed-applications-images/06-badge.png#lightbox)

Wskaźnika jest często używana do wyświetlania oznaczenie numer nieprzeczytana, nowych elementów. Aby usunąć wskaźnika, ustaw `BadgeValue` równej null, jak pokazano poniżej:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />


## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Karty w scenariuszach z systemem innym niż RootViewController

W powyższym przykładzie mamy pokazano, jak pracować z `UITabBarController` gdy jest `RootViewController` okna. W tym przykładzie zostaną omówione, jak używać `UITabBarController` gdy nie jest `RootViewController` i Pokaż to tworzenia Użyj Scenorys.

 <a name="Initial_Screen_Example" />


### <a name="initial-screen-example"></a>Przykład ekranu początkowego

W tym scenariuszu, ekran początkowy ładuje z kontrolera, który nie jest `UITabBarController`. Gdy użytkownik użyje ekran, naciskając przycisk, tego samego kontrolera widoku zostaną załadowane na `UITabBarController`, który zostanie przedstawiony użytkownikowi. Poniższy zrzut ekranu przedstawia przepływ aplikacji:

[![](creating-tabbed-applications-images/inital-screen-application.png "Ten zrzut ekranu przedstawia przepływ aplikacji")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

Zacznijmy nowej aplikacji, w tym przykładzie. Ponownie, użyjemy **iPhone > aplikacji > pusty projekt (C#)** szablonu, tym razem nazewnictwa projektu `InitialScreenDemo`.


W tym przykładzie potrzebujemy scenorysu do przechowywania naszych kontrolerów widoku. Aby dodać scenorysu:

- Kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Dodaj > Nowy plik**.

- Po wyświetleniu okna dialogowego Nowy plik, przejdź do **systemu iOS > pusty iPhone scenorysu**.

Umożliwia wywołanie tego nowego scenorysu **MainStoryboard** , jak pokazano poniżej: 

[![](creating-tabbed-applications-images/new-file-dialog.png "Dodaj plik MainStoryboard do projektu")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

Istnieje kilka ważnych czynności, należy zwrócić uwagę podczas dodawania scenorysu do pliku uprzednio z systemem innym niż scenorysu, które są objęte [wprowadzenie do scenorysu](~/ios/user-interface/storyboards/index.md) przewodnik. Są to:

 
1. Dodaj nazwę scenorysu do **interfejsu Main** sekcji `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "Ustaw główny interfejsu MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. W Twojej `App Delegate`, Zastąp metodę okno z następującym kodem:

    ```csharp
    public override UIWindow Window {
      get;
      set;
    }
    ```

Zamierzamy potrzebne w tym przykładzie trzy kontrolery widoku. Jedną o nazwie `ViewController1`, będzie używany jako początkowa kontrolera widoku oraz w pierwszej karcie. Pozostałe dwa o nazwie `ViewController2` i `ViewController3`, który będzie używany w kartach drugiego i trzeciego odpowiednio.

Otwórz projektanta przez dwukrotne kliknięcie pliku MainStoryboard.storyboard i przeciągnij trzy kontrolery widok na powierzchnię projektu. Chcemy każdego z tych kontrolerów widoku do własnych klasa odpowiadającej nazwie powyżej, tak, w obszarze **tożsamości > klasy**, wpisz jego nazwę, jak pokazano na poniższym zrzucie ekranu:

[![](creating-tabbed-applications-images/class-name.png "Ustaw klasy ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

Visual Studio for Mac automatycznie wygeneruje klasy i projektanta pliki potrzebne, można to zaobserwować w konsoli do rozwiązania, jak przedstawiono poniżej:

[![](creating-tabbed-applications-images/solution-pad2.png "Wygenerowany automatycznie pliki w projekcie")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

 <a name="Creating_the_UI" />


#### <a name="creating-the-ui"></a>Tworzenie interfejsu użytkownika

Następnie utworzymy prosty interfejs użytkownika dla każdej z widoków ViewController za pomocą platformy Xamarin iOS projektanta.

Chcemy przeciągnąć `Label` i `Button` na ViewController1 z **przybornika** po prawej stronie. Następnie użyjemy konsoli do właściwości można edytować nazwę i tekst kontrolki do następującego:

-  **Etykieta** : `Text`  =  **jeden**
-  **Przycisk** : `Title`  =  **użytkownik wykona akcję początkowej**


Firma Microsoft będzie kontrolowanie widoczność naszych przycisku w `TouchUpInside` zdarzeń i firma Microsoft muszą odwoływać się do niego w kodzie. Umożliwia identyfikację z **nazwa** `aButton` w konsoli do właściwości, jak pokazano na poniższym zrzucie ekranu:

[![](creating-tabbed-applications-images/abutton-properties.png "Ustaw nazwę aButton w konsoli właściwości")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

Obszar projektu powinna wyglądać podobnie jak na poniższym zrzucie ekranu:

[![](creating-tabbed-applications-images/design-surface1.png "Obszar projektu powinna wyglądać podobnie do tego zrzutu ekranu")](creating-tabbed-applications-images/design-surface1.png#lightbox)

Dodajmy trochę więcej szczegółów `ViewController2` i `ViewController3`, dodając etykietę do każdej i zmiana tekstu odpowiednio na "2" i "3". Ten rezultat podkreśla użytkownikowi czekamy na karcie/widoku.

#### <a name="wiring-up-the-button"></a>Podłączanie przycisku

Chcemy się załadować `ViewController1` po pierwszym uruchomieniu aplikacji. Po naciśnięciu przycisku, firma Microsoft będzie ukrywanie przycisku i załadować `UITabBarController` z `ViewController1` wystąpienie w pierwszej karcie.

Gdy użytkownik zwolni `aButton`, chcemy zdarzeń TouchUpInside, które będą wyzwalane. Teraz kliknij przycisk i **kartę zdarzenia** konsoli właściwości zadeklarować programu obsługi zdarzeń — `InitialActionCompleted` — tak go mogą być przywoływane w kodzie. Jest to zilustrowane na poniższym zrzucie ekranu:

[![](creating-tabbed-applications-images/event-handler.png "Gdy użytkownik zwalnia aButton, wyzwolić zdarzenie TouchUpInside")](creating-tabbed-applications-images/event-handler.png#lightbox)

Teraz musimy sprawdzić kontroler widoku, aby ukryć przycisku, gdy zdarzenie jest generowane `InitialActionCompleted`. W `ViewController1`, dodaj następującą metodę częściowe:

```csharp
partial void InitialActionCompleted (UIButton sender)
    {
      aButton.Hidden = true;  
    }
```

Zapisz plik i uruchomić aplikację. Firma Microsoft powinna zostać wyświetlona jeden pojawia się ekran i przycisk znikają na Touch w górę.

#### <a name="adding-the-tab-bar-controller"></a>Dodawanie kart kontrolera

Mamy teraz naszych początkowy widok działa zgodnie z oczekiwaniami. Następnie chcemy, aby dodać go do `UITabBarController`, wraz z widoków 2 i 3. Teraz Otwórz scenorysu w projektancie.

W **przybornika**, wyszukaj **kontrolera pasek kartę** w kontrolerach & obiekty i przeciągnij na powierzchnię projektu. Jak widać na poniższym zrzucie ekranu, gdy kontroler paska kartę jest bez interfejsu użytkownika i dlatego oferuje dwa kontrolery widoku z nim domyślnie:

[![](creating-tabbed-applications-images/tabbarcontroller.png "Dodawanie kontrolera pasek kartę do układu")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

Usunięcie tych nowych kontrolerów widoku wybierając czarny pasek u dołu i naciskając klawisz delete.

W naszym scenorysu możemy użyć Segues do obsługi przejścia między TabBarController i naszych kontrolerów widoku. Po interakcji z widok początkowy, chcemy załadować do TabBarController widoczne dla użytkownika. Skonfiguruj to w projektancie.

**Klawisz CTRL** i **przeciągania** przycisku do TabBarController. Na myszy w górę będą wyświetlane menu kontekstowe. Chcemy używać modalne segue. 
 
Do konfigurowania wszystkich naszych karty **klawisz Ctrl** z TabBarController do poszczególnych kontrolerów naszych widoku w kolejności od jednej do trzech, a następnie wybierz relacji **kartę** z menu kontekstowego, jak przedstawiono poniżej:

[![](creating-tabbed-applications-images/context-menu.png "Wybierz kartę relacji")](creating-tabbed-applications-images/context-menu.png#lightbox)

Twoje scenorysu powinien wyglądać na poniższym zrzucie ekranu:

[![](creating-tabbed-applications-images/segue-layout.png "Scenorysu powinien przypominać ten zrzut ekranu")](creating-tabbed-applications-images/segue-layout.png#lightbox)

Jeśli firma Microsoft kliknij jeden z elementów paska kartę i Eksploruj panelu Właściwości widać szereg różnych opcji, jak przedstawiono poniżej:

[![](creating-tabbed-applications-images/properties-panel.png "Ustawianie opcji kartę w Eksploratorze właściwości")](creating-tabbed-applications-images/properties-panel.png#lightbox)

Firma Microsoft to służy do edytowania niektórych atrybutów, takich jak wskaźnika, tytuł i iOS [identyfikator](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html), między innymi

Czy możemy zapisać i uruchomić aplikację teraz możemy znaleźć, gdy wystąpienie ViewController1 jest ładowany do TabBarController pojawi się przycisk. Ta funkcja pozwala rozwiązać ten problem przez sprawdzenie, czy bieżący widok ma element nadrzędny kontrolera widoku. Jeśli tak, wiemy, możemy wewnątrz TabBarController, i dlatego powinien być ukryty przycisku. Teraz Dodaj poniższy kod do klasy ViewController1:

```csharp
public override void ViewDidLoad ()
{
     if (ParentViewController != null){
       aButton.Hidden = true;
     }

}
```

Po uruchomieniu aplikacji, a użytkownik naciska przycisk na pierwszym ekranie UITabBarController jest ładowany, mając na pierwszym ekranie umieszczane w pierwszej karcie, jak pokazano poniżej:

[![](creating-tabbed-applications-images/first-view.png "Przykładowe dane wyjściowe aplikacji")](creating-tabbed-applications-images/first-view.png#lightbox)

<!--Save the files and run the application:

[![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)-->

## <a name="summary"></a>Podsumowanie

Jak używać omówione w tym artykule `UITabBarController` w aplikacji. Rezygnacja za pośrednictwem jak załadować kontrolerów do każdej z kart, a także sposób ustawiania właściwości na kartach taki tytuł, obrazu i wskaźnika. Firma Microsoft badane, przy użyciu scenorys, jak załadować `UITabBarController` w czasie wykonywania, gdy nie jest ona `RootViewController` okna.


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie aplikacji na kartach (na przykład)](https://developer.xamarin.com/samples/monotouch/CreatingTabbedApplications/)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [Odwołania do klasy UITabBarController](http://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
