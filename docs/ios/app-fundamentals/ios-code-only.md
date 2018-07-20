---
title: Tworzenie interfejsów użytkownika systemu iOS w kodzie w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano sposób tworzenia interfejsu użytkownika dla aplikacji platformy Xamarin.iOS przy użyciu kodu. Omówiono w nim kontrolerów widoku, tworzenie Wyświetl hierarchię obsługi rotację i nie tylko.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: 5e9bf9555d10c8b34ad9323529d4af5ea66110f8
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156785"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Tworzenie interfejsów użytkownika systemu iOS w kodzie w rozszerzeniu Xamarin.iOS

Interfejs użytkownika aplikacji systemu iOS przypomina storefront — aplikacja pobiera zazwyczaj jedno okno, ale go wypełnić okno jako wiele obiektów potrzebuje, a obiekty i ich rozmieszczenia można zmienić w zależności od tego, w jaki aplikacja chce, aby wyświetlić. Obiekty w tym scenariuszu - rzeczy, które widzi użytkownik -, są nazywane widoków. Do tworzenia na jednym ekranie, w aplikacji, widoki są ułożone jeden na drugim w zawartości Wyświetl hierarchię i hierarchii jest zarządzany przez pojedynczy kontroler widoku. Aplikacje z wieloma ekranami może mieć wielu zawartości widoku hierarchii, każdy z własną kontrolera widoku, a następnie aplikacja umieszcza widoków w oknie do tworzenia różnych hierarchii widok zawartości oparty na ekranie, której należy użytkownik.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Poniższy diagram ilustruje relacje między oknem, widoki, widoków podrzędnych i kontroler widoku, które Przesuń interfejs użytkownika na ekranie urządzenia: 

[![](ios-code-only-images/image9.png "Ten diagram przedstawia relacje między okna, widoki, widoków podrzędnych i kontroler widoku")](ios-code-only-images/image9.png#lightbox)

Te hierarchie z widoku można skonstruować przy użyciu [Projektant platformy Xamarin dla systemu iOS](~/ios/user-interface/designer/index.md) w programie Visual Studio, jednak warto mieć powinieneś rozumieć podstawy pracy całkowicie w kodzie. W tym artykule przedstawiono niektóre podstawowe punkty do uruchomienia i uruchomiona za pomocą interfejsu użytkownika tylko do kodu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Poniższy diagram ilustruje relacje między oknem, widoki, widoków podrzędnych i kontroler widoku, które Przesuń interfejs użytkownika na ekranie urządzenia: 

[![](ios-code-only-images/image9.png "Ten diagram przedstawia relacje między okna, widoki, widoków podrzędnych i kontroler widoku")](ios-code-only-images/image9.png#lightbox)

Te hierarchie z widoku można skonstruować przy użyciu [Projektant platformy Xamarin dla systemu iOS](~/ios/user-interface/designer/index.md) w programie Visual Studio dla komputerów Mac, jednak warto mieć powinieneś rozumieć podstawy pracy całkowicie w kodzie. W tym artykule przedstawiono niektóre podstawowe punkty do uruchomienia i uruchomiona za pomocą interfejsu użytkownika tylko do kodu.

-----

## <a name="creating-a-code-only-project"></a>Tworzenie tylko kod projektu

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>Pusty szablon projektu systemu iOS

Najpierw utwórz projekt dla systemu iOS w programie Visual Studio przy użyciu **Plik > Nowy Projekt > Visual C# > iPhone & iPad > aplikacji (Xamarin) dla systemu iOS** projektu, pokazano poniżej:

[![Okno dialogowe nowego projektu](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

Następnie wybierz pozycję **pusta aplikacja** szablonu projektu:

[![Wybierz okno dialogowe z szablonu](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

Pusty szablon projektu służący do projektu o rozmiarze 4 plików:

[![Pliki projektu](ios-code-only-images/empty-project.w157-sml.png "pliki projektu")](ios-code-only-images/empty-project.w157.png#lightbox)


1. **AppDelegate.cs** — zawiera `UIApplicationDelegate` podklasy, `AppDelegate` , który jest używany do obsługi zdarzeń aplikacji z systemem iOS. W oknie aplikacji jest tworzony w `AppDelegate`firmy `FinishedLaunching` metody.
1. **Main.cs** — zawiera punkt wejścia dla aplikacji, która określa klasę dla `AppDelegate` .
1. **Plik info.plist** -pliku listy właściwości, który zawiera informacje o konfiguracji aplikacji.
1. **Plik Entitlements.plist** — pliku listy właściwości, który zawiera informacje o możliwościach i uprawnienia aplikacji.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="ios-templates"></a>Szablony systemu iOS


Visual Studio dla komputerów Mac nie oferuje pustego szablonu. Wszystkie szablony są dostarczane z obsługą scenorysu, którym Apple zalecane jako podstawowy sposób tworzenia interfejsu użytkownika. Jednak jest możliwe utworzenie interfejs użytkownika całkowicie w kodzie. 

Poniższe kroki prowadzące przez usuwanie scenorysu z aplikacji: 


1. Szablon aplikacja pojedynczego widoku służy do tworzenia nowego projektu systemu iOS:
    
    [![](ios-code-only-images/single-view-app.png "Szablon aplikacja pojedynczego widoku")](ios-code-only-images/single-view-app.png#lightbox)

1. Usuń `Main.Storyboard` i `ViewController.cs` plików. Czy **nie** Usuń `LaunchScreen.Storyboard`. Kontroler widoku należy usunąć, ponieważ CodeBehind dla kontrolera widoku, który jest tworzony w scenorysu:
1. Upewnij się, że wybrano **Usuń** w podręcznym oknie dialogowym:
    
    [![](ios-code-only-images/delete.png "Przycisk Usuń w wyskakującym oknie dialogowym")](ios-code-only-images/delete.png#lightbox)

1. W pliku Info.plist, należy usunąć informacji o wewnątrz **informacje o wdrożeniu > Main interfejsu** opcji:
    
    [![](ios-code-only-images/main-interface.png "Usuń informacje wewnątrz opcja interfejsu Main")](ios-code-only-images/main-interface.png#lightbox)

1. Na koniec Dodaj następujący kod, aby Twoje `FinishedLaunching` metody w klasie w elemencie AppDelegate:
        
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            // create a new window instance based on the screen size
            window = new UIWindow(UIScreen.MainScreen.Bounds);

            // make the window visible
            window.MakeKeyAndVisible();

            return true;
        }

Kod, który został dodany do `FinishedLaunching` metoda w kroku 5 powyżej, jest minimalna ilość kodu wymaganą można utworzyć okna dla aplikacji systemu iOS.


-----



aplikacje dla systemu iOS są tworzone przy użyciu [wzorzec MVC](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller). Pierwszy ekran, który wyświetla aplikacji jest tworzony z kontrolera widoku głównego okna. Zobacz [Witaj, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) przewodnik dla samego wzorca, szczegółowe informacje na temat platformy MVC.

Wykonania na `AppDelegate` dodane przez ten szablon tworzy okno aplikacji z istnieje tylko jeden dla każdej aplikacji dla systemu iOS, która sprawia, że widoczne z następującym kodem:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    public override UIWindow Window
            {
                get;
                set;
            }

    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        window.MakeKeyAndVisible();

        return true;
    }
}
```

W przypadku uruchomienia tej aplikacji teraz prawdopodobnie otrzymamy wyjątek zgłoszony z informacją, że `Application windows are expected to have a root view controller at the end of application launch`. Teraz Dodaj kontroler, co kontroler widoku głównego aplikacji.

## <a name="adding-a-controller"></a>Dodawanie kontrolera

Aplikacja może zawierać wiele kontrolerów widoku, ale musi ona mieć jeden kontroler widoku głównego do kontrolowania wszystkich kontrolerów widoku.  Dodawanie kontrolera do okna, tworząc `UIViewController` wystąpienia oraz ustawienie `window.RootViewController` właściwości:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;

        Window.RootViewController = controller;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Każdy kontroler ma skojarzony widok, który jest dostępny z `View` właściwości. Powyższy kod zmienia widok `BackgroundColor` właściwość `UIColor.LightGray` tak, że będzie ona widoczna, jak pokazano poniżej:

 [![](ios-code-only-images/image1.png "Tło widoku jest widoczny szary światła")](ios-code-only-images/image1.png#lightbox)

Firma Microsoft można ustawić dowolny `UIViewController` podklasy jako `RootViewController` w ten sposób, łącznie z kontrolerów UIKit, a także tych, napiszemy, aby określić główną przyczynę. Na przykład, poniższy kod dodaje `UINavigationController` jako `RootViewController`:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;
        controller.Title = "My Controller";

        var navController = new UINavigationController(controller);

        Window.RootViewController = navController;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

To daje kontrolera zagnieżdżone w obrębie kontrolera nawigacji, jak pokazano poniżej:

 [![](ios-code-only-images/image2.png "Kontroler zagnieżdżone w obrębie kontrolera nawigacji")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Tworzenia kontrolera widoku

Teraz, gdy zobaczyliśmy, jak dodać kontrolera jako `RootViewController` okna, zobaczmy, jak utworzyć kontroler widoku niestandardowego w kodzie.

Dodaj nową klasę o nazwie `CustomViewController` jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.w157-sml.png "Dodaj nową klasę o nazwie CustomViewController")](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](ios-code-only-images/new-file.png "Dodaj nową klasę o nazwie CustomViewController")](ios-code-only-images/new-file.png#lightbox)

-----

Klasa powinien dziedziczy `UIViewController`, która znajduje się w `UIKit` przestrzeni nazw, jak pokazano:

```csharp
using System;
using UIKit;

namespace CodeOnlyDemo
{
    class CustomViewController : UIViewController
    {
    }
}
```

<a name="Initializing_the_View"/>

## <a name="initializing-the-view"></a>Inicjowanie widoku

`UIViewController` zawiera metodę o nazwie `ViewDidLoad` który jest wywoływany, gdy kontroler widoku najpierw jest ładowany do pamięci. Jest to odpowiednie miejsce, w celu inicjowania widoku, takie jak ustawienie jego właściwości.

Na przykład poniższy kod dodaje przycisk i program obsługi zdarzeń, aby wypchnąć nowy kontroler widoku na stosie nawigacji po naciśnięciu przycisku:

```csharp
using System;
using CoreGraphics;
using UIKit;

namespace CodyOnlyDemo
{
    public class CustomViewController : UIViewController
    {
        public CustomViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            View.BackgroundColor = UIColor.White;
            Title = "My Custom View Controller";

            var btn = UIButton.FromType (UIButtonType.System);
            btn.Frame = new CGRect (20, 200, 280, 44);
            btn.SetTitle ("Click Me", UIControlState.Normal);

            var user = new UIViewController ();
            user.View.BackgroundColor = UIColor.Magenta;

            btn.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (user, true);
            };

            View.AddSubview (btn);

        }
    }
}
```

Aby obciążenia tego kontrolera w aplikacji i zaprezentować proste nawigacji, Utwórz nowe wystąpienie klasy `CustomViewController`. Tworzenie nowego kontrolera nawigacji, w ramach wystąpienia kontrolera widoku i ustaw nowego kontrolera nawigacji w oknie `RootViewController` w `AppDelegate` tak jak poprzednio:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Teraz podczas ładowania aplikacji `CustomViewController` jest ładowany wewnątrz kontrolera nawigacji:

 [![](ios-code-only-images/customvc.png "CustomViewController jest ładowany wewnątrz kontrolera nawigacji")](ios-code-only-images/customvc.png#lightbox)
 
Kliknięcie przycisku spowoduje _wypychania_ nowy kontroler widoku na stosie nawigacji:

[![](ios-code-only-images/customvca.png "Nowy kontroler widoku wypychane na stosie nawigacji")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Tworzenie hierarchii widoku

W powyższym przykładzie Rozpoczęliśmy tworzymy interfejs użytkownika w kodzie, przez dodanie przycisku do kontrolera widoku.

interfejsy użytkownika dla systemu iOS składają się z widoku hierarchii. Dodatkowe widoki, takie jak etykiety, przyciski, suwaki, itp., są dodawane jako widoków podrzędnych niektóre widoku nadrzędnego.

Na przykład możemy edytować `CustomViewController` Aby utworzyć ekran logowania, w którym użytkownik może wprowadzić nazwę użytkownika i hasło. Ekran będzie składać się z dwóch pól tekstowych i przycisku.

### <a name="adding-the-text-fields"></a>Dodawanie pól tekstowych

Najpierw należy usunąć program obsługi przycisk i zdarzeń, który został dodany w [inicjowania widoku](#Initializing_the_View) sekcji. 

Dodaj formant nazwy użytkownika przez tworzenie i Inicjowanie `UITextField` i dodanie go do widoku hierarchii, jak pokazano poniżej:

```csharp
class CustomViewController : UIViewController
{
    UITextField usernameField;

    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        View.BackgroundColor = UIColor.Gray;

        nfloat h = 31.0f;
        nfloat w = View.Bounds.Width;

        usernameField = new UITextField
        {
            Placeholder = "Enter your username",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 82, w - 20, h)
        };

        View.AddSubview(usernameField);
    }
}
```

Kiedy tworzymy `UITextField`, ustawiliśmy `Frame` właściwości, aby zdefiniować jej lokalizacja i rozmiar. W systemie iOS 0,0 Współrzędna jest w lewym górnym rogu znakiem + x z prawej strony i + y w dół. Po ustawieniu `Frame` oraz kilka innych właściwości nazywamy `View.AddSubview` dodać `UITextField` na wyświetlanie hierarchii. To sprawia, że `usernameField` widok podrzędny z `UIView` wystąpienie, które `View` odwołania do właściwości. Widok podrzędny zostanie dodany z porządku, który jest większy niż jego widoku nadrzędnego, więc pojawia się ono przed widoku nadrzędnego na ekranie.

Aplikacja o `UITextField` uwzględnione znajdują się poniżej:

 [![](ios-code-only-images/image4.png "Aplikacja o UITextField uwzględnione")](ios-code-only-images/image4.png#lightbox)

Możemy dodać `UITextField` hasło w podobny sposób, tylko tym razem ustawimy `SecureTextEntry` właściwości na wartość true, jak pokazano poniżej:

```csharp
public class CustomViewController : UIViewController
{
    UITextField usernameField, passwordField;
    public override void ViewDidLoad()
    {
       // keep the code the username UITextField
        passwordField = new UITextField
        {
            Placeholder = "Enter your password",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 114, w - 20, h),
            SecureTextEntry = true
        };

      View.AddSubview(usernameField); 
      View.AddSubview(passwordField);
   }
}

```

Ustawienie `SecureTextEntry = true` ukrywa tekstem wprowadzonym w `UITextField` przez użytkownika, jak pokazano poniżej:

 [![](ios-code-only-images/image4a.png "Ustawienie SecureTextEntry wartość true powoduje ukrycie tekstem wprowadzonym przez użytkownika")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Dodawanie przycisku

Następnie dodamy przycisku, dzięki czemu użytkownik może przesłać nazwę użytkownika i hasło. Ten przycisk jest dodawany do wyświetlanie hierarchii jak inne kontrolki, przez przekazanie jej jako argument dla widoku nadrzędnego `AddSubview` ponownie metodą.

Poniższy kod dodaje przycisk i rejestruje zdarzenia obsługi dla `TouchUpInside` zdarzeń:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

Dzięki temu w miejscu ekranu logowania wygląda jak poniżej:

 [![](ios-code-only-images/image5.png "Ekran logowania")](ios-code-only-images/image5.png#lightbox)

W przeciwieństwie do poprzednich wersji systemu IOS, domyślne tło przycisku jest niewidoczny. Zmienianie przycisku `BackgroundColor` właściwość ulegnie zmianie, to:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

To spowoduje kwadratowy przycisk, a nie typowej zaokrąglone krawędziowa przycisku. Aby uzyskać jego zaokrąglenia, użyj następującego fragmentu kodu:

```csharp
submitButton.Layer.CornerRadius = 5f;
```

Te zmiany widoku będzie wyglądać następująco:

[![](ios-code-only-images/image6.png "Uruchom przykład widoku")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Dodawanie wielu widoków do Wyświetl hierarchię

systemu iOS zapewnia możliwość Dodawanie wielu widoków do wyświetlanie hierarchii za pomocą `AddSubviews`.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>Dodawanie funkcji przycisku

Po kliknięciu przycisku, użytkownicy będą oczekiwać, że coś, co ma być wykonywana. Na przykład jest wyświetlany alert lub nawigacji odbywa się na innym ekranie. 

Dodajmy trochę kodu do przekazania drugiego kontrolera widoku stos nawigacji.

Najpierw utwórz drugi kontroler widoku:

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

Następnie należy dodać funkcje do `TouchUpInside` zdarzeń:

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

Poniżej przedstawiono nawigacji:

[![](ios-code-only-images/navigation.png "Ten wykres przedstawia nawigacji")](ios-code-only-images/navigation.png#lightbox)

Należy zauważyć, że domyślnie, korzystając z kontrolera nawigacji dla systemu iOS umożliwia aplikacji pasku nawigacyjnym, a przycisk Wstecz, aby możliwe było przenieść z powrotem przez stos.

## <a name="iterating-through-the-view-hierarchy"></a>Iteracja Wyświetl hierarchię

Istnieje możliwość do iterowania po hierarchii widok podrzędny i wybrania dowolnego określonego widoku. Na przykład, aby znaleźć każdą `UIButton` i nadać inny tego przycisku `BackgroundColor`, służy poniższy fragment kodu

```csharp
foreach(var subview in View.Subviews)
{
    if (subview is UIButton)
    {
         var btn = subview as UIButton;
         btn.BackgroundColor = UIColor.Green;
    }
}
```

To, jednak nie będzie działać, jeśli widok jest postanowiliśmy dla `UIView` jako we wszystkich widokach przechodzi jako `UIView` jako obiekty dodawane do widoku nadrzędnym siebie dziedziczą `UIView`.

## <a name="handling-rotation"></a>Obsługa obrotu

Jeśli obrocie urządzenia na poziomą formanty nie zmienić rozmiar, tak jak pokazano na poniższym zrzucie ekranu:

 [![](ios-code-only-images/image7.png "Jeśli obrocie urządzenia na poziomą formanty zmienia swojej wielkości odpowiednio")](ios-code-only-images/image7.png#lightbox)

Jednym ze sposobów, aby rozwiązać ten problem jest ustawienie `AutoresizingMask` właściwości każdego widoku. W tym przypadku chcemy, aby formanty do usługi stretch w poziomie, dlatego możemy ustawić każdy `AutoresizingMask`. Poniższy przykład dotyczy programu `usernameField`, ale takie same należałoby mają być stosowane do każdego gadżetu w hierarchii widoku.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Teraz gdy mamy obrócić urządzenie lub symulator, wszystko, co zostanie rozciągnięty w celu wypełnienia dodatkowe miejsce, jak pokazano poniżej:

 [![](ios-code-only-images/image8.png "Wszystkie formanty rozciągnąć na dodatkowy obszar")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Tworzenie niestandardowych widoków

Oprócz używania kontrolek, które są częścią UIKit, widoki niestandardowe można również. Można utworzyć widok niestandardowy dziedziczący po `UIView` i zastępowanie `Draw`. Możemy utworzyć widok niestandardowy i dodaj go do hierarchii widok, aby zademonstrować.

### <a name="inheriting-from-uiview"></a>Dziedziczenie z UIView

Pierwszą rzeczą, jaką musimy jest, Utwórz klasę dla widoku niestandardowego. Możemy to zrobić za pomocą **klasy** szablonu w programie Visual Studio, aby dodać pustą klasę o nazwie `CircleView`. Klasa podstawowa powinna być równa `UIView`, która możemy Odwołaj znajduje się w `UIKit` przestrzeni nazw. Będziemy również potrzebować `System.Drawing` także przestrzeni nazw. Inne różnych `System.*` przestrzeni nazw nie będzie używany w tym przykładzie, więc możesz je usunąć.

Klasa powinien wyglądać następująco:

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>Rysowanie w UIView

Każdy `UIView` ma `Draw` metodę, która jest wywoływana przez system, gdy musi zostać narysowany. `Draw` nigdy nie powinna być wywoływana bezpośrednio. Jest ona wywoływana przez system podczas przetwarzania wykonywania pętli. Po raz pierwszy przy użyciu wykonywania pętli po dodaniu do hierarchii widoków, widok jego `Draw` metoda jest wywoływana. Kolejne wywołania `Draw` występują, gdy widok jest oznaczony jako wymagające do rysowania, wywołując jedną `SetNeedsDisplay` lub `SetNeedsDisplayInRect` w widoku.

Możemy dodać kod rysowania naszych widoku przez dodanie takiego kodu wewnątrz zastąpione `Draw` metody, jak pokazano poniżej:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    //get graphics context
    using (var g = UIGraphics.GetCurrentContext())
    {
        // set up drawing attributes
        g.SetLineWidth(10.0f);
        UIColor.Green.SetFill();
        UIColor.Blue.SetStroke();

        // create geometry
        var path = new CGPath();
        path.AddArc(Bounds.GetMidX(), Bounds.GetMidY(), 50f, 0, 2.0f * (float)Math.PI, true);

        // add geometry to graphics context and draw
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);
    }
}
```

Ponieważ `CircleView` jest `UIView`, firma Microsoft można również ustawić `UIView` również właściwości. Na przykład możemy ustawić `BackgroundColor` w Konstruktorze:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Aby użyć `CircleView` właśnie utworzyliśmy, firma Microsoft może albo dodaj go jako widok podrzędny do Wyświetl hierarchię w istniejącego kontrolera, ile My mieliśmy z `UILabels` i `UIButton` wcześniej, albo załadować go jako widok nowy kontroler. Wykonamy teraz zadania z drugim.

### <a name="loading-a-view"></a>Trwa ładowanie widoku

 `UIViewController` zawiera metodę o nazwie `LoadView` który jest wywoływany przez kontroler utworzyć jej widok. Jest to odpowiednie miejsce, aby utworzyć widok i przypisać ją do kontrolera `View` właściwości.

Po pierwsze potrzebujemy kontrolera, dzięki czemu można tworzyć nowe pustą klasę o nazwie `CircleController`.

W `CircleController` Dodaj następujący kod, aby ustawić `View` do `CircleView` (nie należy wywołać `base` implementacji przesłonięcia):

```csharp
using UIKit;

namespace CodeOnlyDemo
{
    class CircleController : UIViewController
    {
        CircleView view;

        public override void LoadView()
        {
            view = new CircleView();
            View = view;
        }
    }
}
```

Na koniec należy przedstawić kontrolera w czasie wykonywania. Możemy to zrobić, dodając procedurę obsługi zdarzeń na przycisk przesyłania, który dodano wcześniej, wykonując następujące czynności:

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Teraz gdy firma Microsoft może uruchomić aplikację, a następnie naciśnij przycisk Prześlij, jest wyświetlany nowy widok jest okrąg:

 [![](ios-code-only-images/circles.png "Zostanie wyświetlony nowy widok jest okrąg")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Tworzenie ekranu startowego

A [ekran startowy](~/ios/app-fundamentals/images-icons/launch-screens.md) jest wyświetlany podczas uruchamiania aplikacji jako sposób mają być wyświetlane użytkownikom, że jest elastyczny. Ponieważ ekran startowy jest wyświetlany podczas ładowania aplikacji, nie można utworzyć w kodzie, ponieważ aplikacja jest nadal ładowany do pamięci. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po usługi tworzenia projektu w programie Visual Studio, na ekranie uruchamiania, jest dostarczany w formie pliku .pliki, który znajduje się w systemie iOS **zasobów** folder wewnątrz projektu. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po usługi tworzenia projektu systemu iOS w programie Visual Studio dla komputerów Mac, na ekranie uruchamiania, jest dostarczany w formie pliku scenorysu. 

-----

Mogą to być edytowane przez podwójne kliknięcie jej i otworzyć go w narzędziu iOS Designer.

Firma Apple zaleca czy .pliki lub plikiem scenorysu jest używana dla aplikacji przeznaczonych dla systemu iOS 8 lub później, po uruchomieniu dowolnego pliku w narzędziu iOS Designer, korzystając z klas rozmiaru oraz układu automatycznego dostosowania układu, tak aby wygląda dobrze, a wyświetlany poprawnie, dla wszystkich urządzeń rozmiary. Obraz statyczny uruchamiania może służyć oprócz .pliki lub Storyboard, aby umożliwić obsługę aplikacji przeznaczonych dla wcześniejszych wersji.

Więcej informacji na temat tworzenia ekranu uruchamiania można znaleźć w poniższych dokumentach:

- [Tworzenie ekranu uruchamiania przy użyciu .pliki](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [Zarządzanie ekrany uruchamiania, za pomocą scenorysów](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> Począwszy od systemu iOS 9 firmy Apple, zaleca się, że scenorysów powinien być używany jako podstawowej metody tworzenia ekranu uruchamiania.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Tworzenie obrazów uruchamiania dla wstępnego dla systemu iOS 8 aplikacji

Obraz statyczny może służyć także .pliki lub ekranu startowego scenorysu, jeśli aplikacja jest przeznaczony dla wersji wcześniejszych niż system iOS 8. 

Ten obraz statyczny można ustawić w pliku Info.plist lub jako katalog zasobów (dla systemu iOS 7) w aplikacji. Należy podać oddzielne obrazy dla każdego rozmiaru urządzenia (320 x 480, 640 x 960 640 x 1136), które aplikacja może być uruchamiana. Aby uzyskać więcej informacji na temat rozmiarów ekranu uruchamiania, należy wyświetlić [obrazy ekranu uruchamiania](~/ios/app-fundamentals/images-icons/launch-screens.md) przewodnik.

> [!IMPORTANT]
> Jeśli aplikacja ma ekranu nie uruchamianie, można zauważyć, że nie pełni dopasowania do ekranu. Jeśli jest to możliwe, należy pamiętać uwzględnić co najmniej obraz 640 x 1136, o nazwie `Default-568@2x.png` do pliku Info.plist. 



## <a name="summary"></a>Podsumowanie

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W tym artykule omówiono sposób tworzenia aplikacji systemu iOS w programie Visual Studio. Zobaczyliśmy, jak utworzyć projekt z szablonu pusty projekt Omawiając sposób utworzyć i dodać kontroler widoku głównego okna. Firma Microsoft następnie pokazano, jak używać kontrolki z UIKit do tworzenia Wyświetl hierarchię w ramach kontrolera do tworzenia ekranu aplikacji. Następnie zbadaliśmy, jak się widoki układ odpowiednio w różnych orientacji i widzieliśmy, jak utworzyć widok niestandardowy przez podklasy `UIView`, a także można załadować widoku w kontrolerze. Na koniec rozważyliśmy jak dodać ekran startowy aplikacji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W tym artykule omówiono sposób tworzenia aplikacji systemu iOS w programie Visual Studio dla komputerów Mac. Przyjrzeliśmy się sposób budowania projektu z szablonem pojedynczy widok, omawiając sposób utworzyć i dodać kontroler widoku głównego okna. Firma Microsoft następnie pokazano, jak używać kontrolki z UIKit do tworzenia Wyświetl hierarchię w ramach kontrolera do tworzenia ekranu aplikacji. Następnie zbadaliśmy, jak się widoki układ odpowiednio w różnych orientacji i widzieliśmy, jak utworzyć widok niestandardowy przez podklasy `UIView`, a także można załadować widoku w kontrolerze. Na koniec rozważyliśmy jak dodać ekran startowy aplikacji.

-----




## <a name="related-links"></a>Linki pokrewne

- [SimpleLogin (przykład)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
