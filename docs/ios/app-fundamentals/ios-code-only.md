---
title: "Tworzenie interfejsów użytkownika systemu iOS w kodzie"
description: "Xamarin.iOS udostępnia dwie metody tworzenia interfejsu użytkownika dla aplikacji — przy użyciu projektanta Xamarin dla systemu iOS lub w kodzie. W tym artykule sprawdza, czy tworzenie interfejsów użytkownika dla systemu iOS całkowicie w kodzie. Widoczny jest sposób Rozpocznij od szablonu projektu do tworzenia przez utworzenie hierarchii widoków z UIKit ekran aplikacji w kontrolerze. Następnie zawarto informacje, jak tworzyć widoki niestandardowe, które mogą być ładowane w kontrolerze."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 24fc64d1bd04cb1ebefb9bf9a359efb395b45074
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="creating-ios-user-interfaces-in-code"></a>Tworzenie interfejsów użytkownika systemu iOS w kodzie

_Xamarin.iOS udostępnia dwie metody tworzenia interfejsu użytkownika dla aplikacji — przy użyciu projektanta Xamarin dla systemu iOS lub w kodzie. W tym artykule sprawdza, czy tworzenie interfejsów użytkownika dla systemu iOS całkowicie w kodzie. Widoczny jest sposób Rozpocznij od szablonu projektu do tworzenia przez utworzenie hierarchii widoków z UIKit ekran aplikacji w kontrolerze. Następnie zawarto informacje, jak tworzyć widoki niestandardowe, które mogą być ładowane w kontrolerze._

Interfejs użytkownika aplikacji systemu iOS przypomina sklepu — aplikacji zwykle pobiera jedno okno, ale jego może zapełnić okna z musi wiele obiektów jego, a obiekty i ustalenia można zmieniać w zależności od tego, co aplikacja potrzebuje do wyświetlenia. W tym scenariuszu - rzeczy, które użytkownik widzi — obiekty są nazywane widoków. Tworzenie jednego ekranu w aplikacji, widoki stos na siebie w hierarchii widok zawartości i hierarchii jest zarządzany przez pojedynczy kontroler widoku. Aplikacje z ekranami wielu mają wiele zawartości widoku hierarchii, każde z nich własny kontroler widoku i aplikacji umieszcza widoków w oknie można utworzyć innej hierarchii widok zawartości oparte na ekranie, której należy użytkownik.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Na poniższym diagramie przedstawiono relacje między okna, widoków, widoków podrzędnych i kontrolera widoku, które Przełącz interfejs użytkownika ekranu urządzenia: 

[![](ios-code-only-images/image9.png "Ten diagram przedstawia relacje między okna, widoków, widoków podrzędnych i kontrolera widoku")](ios-code-only-images/image9.png#lightbox)

Te hierarchie widoku można skonstruować przy użyciu [projektanta Xamarin dla systemu iOS](~/ios/user-interface/designer/index.md) w programie Visual Studio, jednak warto podstawową wiedzę pracy całkowicie w kodzie. W tym artykule przedstawiono niektóre podstawowe punkty do uruchomienia i działa z programowanie interfejsu użytkownika tylko do kodu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Na poniższym diagramie przedstawiono relacje między okna, widoków, widoków podrzędnych i kontrolera widoku, które Przełącz interfejs użytkownika ekranu urządzenia: 

[![](ios-code-only-images/image9.png "Ten diagram przedstawia relacje między okna, widoków, widoków podrzędnych i kontrolera widoku")](ios-code-only-images/image9.png#lightbox)


Te hierarchie widoku można skonstruować przy użyciu [projektanta Xamarin dla systemu iOS](~/ios/user-interface/designer/index.md) w programie Visual Studio dla komputerów Mac, jednak warto podstawową wiedzę pracy całkowicie w kodzie. W tym artykule przedstawiono niektóre podstawowe punkty do uruchomienia i działa z programowanie interfejsu użytkownika tylko do kodu.


-----

## <a name="creating-a-code-only-project"></a>Tworzenie projektu tylko do kodu

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>Pusty szablon projektu systemu iOS

Najpierw utwórz projekt dla systemu iOS w programie Visual Studio przy użyciu telefonów iPhone **pusty projekt** szablonu, pokazano poniżej, które będziemy rozszerzać można dodać widoków i kontrolerów.


[![](ios-code-only-images/blankapp-vs.png "Okno dialogowe nowego projektu")](ios-code-only-images/blankapp-vs.png#lightbox)


Pusty szablon projektu dodaje 4 plików do projektu:


[![](ios-code-only-images/empty-project.png "Pliki projektu")](ios-code-only-images/empty-project.png#lightbox)


1. **AppDelegate.cs** — zawiera `UIApplicationDelegate` podklasy, `AppDelegate` , które jest używane do obsługi zdarzeń aplikacji z systemem iOS. W oknie aplikacji jest tworzony w `AppDelegate`w `FinishedLaunching` metody.
1. **Main.cs** — zawiera punkt wejścia dla aplikacji, która określa klasę dla `AppDelegate` .
1. **Info.plist** -pliku listy właściwości, który zawiera informacje o konfiguracji aplikacji.
1. **Entitlements.plist** — pliku listy właściwości, który zawiera informacje o możliwości i uprawnienia aplikacji.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="ios-templates"></a>iOS szablonów


Visual Studio for Mac nie oferuje pustego szablonu. Wszystkie szablony pochodzą z obsługą scenorysu, którym Apple zalecane jako podstawowy sposób tworzenia interfejsu użytkownika. Istnieje możliwość utworzenia Interfejsie całkowicie w kodzie. 

Poniższe kroki prowadzące przez usunięcie scenorysu z aplikacji: 


1. Szablon pojedynczego widoku aplikacji do tworzenia nowego projektu systemu iOS:
    
    [![](ios-code-only-images/single-view-app.png "Użyj szablonu pojedynczego widoku aplikacji")](ios-code-only-images/single-view-app.png#lightbox)

1. Usuń `Main.Storyboard` i `ViewController.cs` plików. Czy **nie** usunąć `LaunchScreen.Storyboard`. Kontroler widoku powinny być usunąć, ponieważ jest on CodeBehind dla kontrolera widoku, która jest tworzona w scenorysu:
1. Upewnij się wybrać **usunąć** w wyskakującym oknie dialogowym:
    
    [![](ios-code-only-images/delete.png "Przycisk Usuń w wyskakującym oknie dialogowym")](ios-code-only-images/delete.png#lightbox)

1. W pliku Info.plist, należy usunąć informacje dotyczące wewnątrz **informacji o wdrożeniu > Main interfejsu** opcji:
    
    [![](ios-code-only-images/main-interface.png "Usuń informacje wewnątrz opcja interfejsu Main")](ios-code-only-images/main-interface.png#lightbox)

1. Na koniec należy dodać następujący kod, aby Twoje `FinishedLaunching` metody w klasie AppDelegate:
        
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            // create a new window instance based on the screen size
            window = new UIWindow(UIScreen.MainScreen.Bounds);

            // make the window visible
            window.MakeKeyAndVisible();

            return true;
        }


-----

## <a name="creating-a-window"></a>Tworzenie okna

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kod, który został dodany do `FinishedLaunching` metoda w kroku 3 powyżej, jest minimalna ilość kod wymagany można utworzyć okna dla aplikacji systemu iOS.  

-----

aplikacje systemu iOS są tworzone przy użyciu [wzorzec MVC](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller). Pierwszy ekran, który wyświetla aplikacji jest tworzona na podstawie kontrolera widoku głównego okna. Zobacz [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) przewodnik dla więcej informacji na temat platformy MVC wzorca samej siebie.

Implementację `AppDelegate` dodane przez szablon tworzy okna aplikacji, z którego jest tylko jeden dla każdej aplikacji systemu iOS i ułatwia widoczne z następującym kodem:

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

Jeśli do uruchomienia tej aplikacji teraz, prawdopodobnie jak wyjątek, informujący `Application windows are expected to have a root view controller at the end of application launch`. Umożliwia dodawanie kontrolera i stał się Appcontroller widoku głównego.

## <a name="adding-a-controller"></a>Dodawanie kontrolera

Aplikacja może zawierać wiele kontrolerów widoku, ale musi mieć jeden kontroler widoku głównego do sterowania wszystkie kontrolery widoku.  Dodaj kontroler do okna, tworząc `UIViewController` wystąpienia i ustawieniem dla niego `window.RootViewController` właściwości:

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

Każdy kontroler ma skojarzone widoku, który jest dostępny z `View` właściwości. Powyższy kod zmiany widoku `BackgroundColor` właściwości `UIColor.LightGray` , dzięki czemu będzie ona widoczna, jak pokazano poniżej:

 [![](ios-code-only-images/image1.png "Tło widoku jest widoczny szary lekkich")](ios-code-only-images/image1.png#lightbox)

Firma Microsoft może ustawić dowolną `UIViewController` podklasy jako `RootViewController` w ten sposób również tym kontrolerów z UIKit, a także tych, możemy nad zapisu. Na przykład poniższy kod dodaje `UINavigationController` jako `RootViewController`:

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

Daje to kontrolera zagnieżdżone w obrębie kontrolera nawigacji, jak pokazano poniżej:

 [![](ios-code-only-images/image2.png "Kontroler zagnieżdżone w obrębie kontrolera nawigacji")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Tworzenie kontrolera widoku

Teraz, gdy firma Microsoft przedstawiono sposób dodawania kontrolera jako `RootViewController` okna Zobaczmy, jak utworzyć kontroler niestandardowy widok w kodzie.

Dodaj nową klasę o nazwie `CustomViewController` w sposób przedstawiony poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.png "Dodaj nową klasę o nazwie CustomViewController")](ios-code-only-images/customviewcontroller.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](ios-code-only-images/new-file.png "Dodaj nową klasę o nazwie CustomViewController")](ios-code-only-images/new-file.png#lightbox)

-----

Klasa powinna dziedziczy `UIViewController`, która znajduje się w `UIKit` przestrzeni nazw, jak pokazano:

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

## <a name="initializing-the-view"></a>Podczas inicjowania widoku

`UIViewController` zawiera metodę o nazwie `ViewDidLoad` który wywoływaną, gdy kontroler widoku najpierw jest ładowany do pamięci. Jest to odpowiednie miejsce do inicjowania widoku, takie jak ustawienia jego właściwości.

Na przykład następujący kod dodaje przycisk i program obsługi zdarzeń, aby wypychać nowego kontrolera widoku na stosie nawigacji po naciśnięciu przycisku:

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

Aby załadować tego kontrolera w aplikacji i Wykaż, prosty nawigacji, Utwórz nowe wystąpienie klasy `CustomViewController`. Tworzenie nowego kontrolera nawigacji, przekaż wystąpienie kontrolera widoku i ustaw nowy kontroler nawigacji w oknie `RootViewController` w `AppDelegate` jak wcześniej:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Teraz podczas ładowania aplikacji `CustomViewController` jest ładowany w kontrolerze nawigacji:

 [![](ios-code-only-images/customvc.png "Załadowano CustomViewController wewnątrz kontrolera nawigacji")](ios-code-only-images/customvc.png#lightbox)
 
Kliknięcie przycisku, będzie _wypychania_ nowego kontrolera widoku na stosie nawigacji:

[![](ios-code-only-images/customvca.png "Nowy kontroler widok wypchnięta na stosie nawigacji")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Tworzenie widoku hierarchii

W powyższym przykładzie mamy rozpoczął tworzenie interfejsu użytkownika w kodzie przez dodawanie przycisku do kontrolera widoku.

interfejsy użytkownika iOS składają się z hierarchii widoku. Dodatkowe widoki, takich jak etykiety, przyciski, suwaki, itp., są dodawane jako widoków podrzędnych niektórych widoku nadrzędnego.

Na przykład, załóżmy edytować `CustomViewController` utworzyć ekran logowania, w którym użytkownik może wprowadzić nazwę użytkownika i hasło. Ekranu będzie zawierać dwa pola tekstowe i przycisk.

### <a name="adding-the-text-fields"></a>Dodawanie pól tekstowych

Najpierw należy usunąć program obsługi przycisku i zdarzenia, który został dodany w [inicjowania widoku](#Initializing_the_View) sekcji. 

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

Gdy utworzymy `UITextField`, możemy ustawić `Frame` właściwości, aby zdefiniować jej lokalizacja i rozmiar. W systemie iOS 0,0 współrzędnych znajduje się w lewym górnym rogu z + x z prawej strony i + y w dół. Po ustawieniu `Frame` oraz kilka innych właściwości nazywamy `View.AddSubview` można dodać `UITextField` do hierarchii widoku. Dzięki temu `usernameField` widok podrzędny z `UIView` wystąpienie `View` odwołań do właściwości. Widok podrzędny zostanie dodany z porządek jest większa niż jego widoku nadrzędnego, aby był przed widoku nadrzędnego na ekranie.

Aplikację z `UITextField` uwzględnione są wyświetlane poniżej:

 [![](ios-code-only-images/image4.png "Aplikację z UITextField włączone")](ios-code-only-images/image4.png#lightbox)

Można dodać `UITextField` hasła w podobny sposób, tylko w tej chwili ustawiliśmy `SecureTextEntry` właściwości na wartość true, jak pokazano poniżej:

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

Ustawienie `SecureTextEntry = true` ukrywa tekst wprowadzony w `UITextField` przez użytkownika, jak pokazano poniżej:

 [![](ios-code-only-images/image4a.png "Ustawienie SecureTextEntry wartość true powoduje ukrycie tekst wprowadzony przez użytkownika")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Dodawanie przycisku

Następnie dodamy przycisk Tak, użytkownik może przesyłania nazwy użytkownika i hasła. Zostanie dodany do hierarchii widoków innych formantu, przekazując go jako argument dla widoku nadrzędnego `AddSubview` metody ponownie.

Poniższy kod dodaje przycisk i rejestruje program obsługi zdarzeń dla `TouchUpInside` zdarzeń:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

Dzięki temu w miejscu ekran logowania wygląda jak poniżej:

 [![](ios-code-only-images/image5.png "Ekran logowania")](ios-code-only-images/image5.png#lightbox)

W przeciwieństwie do poprzednich wersji systemu IOS, domyślne tło przycisku jest niewidoczny. Zmienianie przycisku `BackgroundColor` zmiany właściwości to:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

To spowoduje kwadratowym przycisku zamiast typowe zaokrąglona krawędziowa przycisku. Aby uzyskać zaokrąglone krawędzi, użyj następującego fragmentu kodu:

```csharp
submitButton.Layer.CornerRadius = 5f;
```

Wprowadzone zmiany widok będzie wyglądać następująco:

[![](ios-code-only-images/image6.png "Uruchom przykład widoku")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Dodawanie wielu widoków do widoku hierarchii

iOS oferuje możliwość dodawania wielu widoków do hierarchii widoku przy użyciu `AddSubviews`.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>Dodawanie przycisku funkcji

Po kliknięciu przycisku użytkownicy będą oczekiwać, że coś nastąpić. Na przykład wyświetlany jest alert lub nawigacji odbywa się na inny ekran. 

Dodajmy trochę kodu do dystrybuowania drugiego kontrolera widoku na stosie nawigacji.

Najpierw utwórz drugi kontroler widoku:

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

Następnie należy dodać funkcję w `TouchUpInside` zdarzeń:

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

Poniżej przedstawiono nawigacji:

[![](ios-code-only-images/navigation.png "Na tym wykresie przedstawiono nawigacji")](ios-code-only-images/navigation.png#lightbox)

Należy zauważyć, że domyślnie, korzystając z kontrolera nawigacji iOS daje aplikacji paska nawigacyjnego i przycisk Wstecz, aby umożliwić powrót przez stos.

## <a name="iterating-through-the-view-hierarchy"></a>Iteracja przez wyświetlanie hierarchii

Istnieje możliwość iteracji hierarchii widok podrzędny i wybrania żadnych konkretnym widoku. Na przykład, aby znaleźć każdego `UIButton` i nadaj tym przycisku innej `BackgroundColor`, może służyć następujący fragment kodu

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

Jednak nie będzie działać w przypadku widoku jest iterowane dla `UIView` jako we wszystkich widokach będą Wróć za `UIView` jako obiekty dodane do widoku nadrzędnym siebie dziedziczyć `UIView`.

## <a name="handling-rotation"></a>Obsługa obrotu

Jeśli obracania urządzenia na poziomą formantów rozmiary nie są zmieniane, jak pokazano w poniższym zrzucie ekranu:

 [![](ios-code-only-images/image7.png "Jeśli użytkownik obraca urządzenia na poziomą, formantów nie zmieniać rozmiar odpowiednio")](ios-code-only-images/image7.png#lightbox)

Jest jednym ze sposobów to naprawić przez ustawienie `AutoresizingMask` właściwości w każdym widoku. W takim przypadku chcemy formanty do rozciągania w poziomie, dlatego ustawimy usługę· każdego `AutoresizingMask`. Poniższy przykład dotyczy programu `usernameField`, ale takie same musi odnosić się do każdego gadżet w hierarchii widoku.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Teraz podczas obracania możemy urządzenie lub symulator, wszystko, co zostanie rozciągnięty w celu wypełnienia dodatkowe miejsce w sposób przedstawiony poniżej:

 [![](ios-code-only-images/image8.png "Wszystkie kontrolki rozciągają się, aby wypełnić dodatkowa miejsce na dysku")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Tworzenie niestandardowych widoków

Oprócz za pomocą formantów, które są częścią UIKit, może także służyć widoków niestandardowych. Widok niestandardowy mogą być tworzone przez dziedziczenie z `UIView` i zastępowanie `Draw`. Teraz Utwórz widok niestandardowy i dodaj go do hierarchii widoku, aby zaprezentować.

### <a name="inheriting-from-uiview"></a>Dziedziczenie z UIView

W pierwszej kolejności konieczne jest, Utwórz klasę widoku niestandardowego. Firma Microsoft będzie to zrobić przy użyciu **klasy** szablonu w programie Visual Studio, aby dodać pustą klasę o nazwie `CircleView`. Klasa podstawowa powinna być równa `UIView`, który mamy odwołania jest w `UIKit` przestrzeni nazw. Poprosimy Cię o również `System.Drawing` również przestrzeni nazw. Inne różnych `System.*` przestrzeni nazw nie będzie używana w tym przykładzie, więc możesz je usunąć.

Klasa powinna wyglądać następująco:

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

Każdy `UIView` ma `Draw` metodę, która jest wywoływana przez system, kiedy zachodzi potrzeba narysowania. `Draw` nigdy nie powinna być wywoływana bezpośrednio. Jest ona wywoływana przez system podczas przetwarzania wykonywania pętli. Po raz pierwszy przez wykonywania pętli po dodaniu widoku do hierarchii widoku jego `Draw` metoda jest wywoływana. Kolejne wywołania `Draw` wystąpić, gdy widok jest oznaczony jako wymagające narysowania przez wywołanie albo `SetNeedsDisplay` lub `SetNeedsDisplayInRect` w widoku.

Można dodać kod rysowania do naszej widoku przez dodanie takich kodu wewnątrz przesłoniętej `Draw` metody, jak pokazano poniżej:

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

Ponieważ `CircleView` jest `UIView`, firma Microsoft może także ustawić `UIView` również właściwości. Na przykład możemy ustawić `BackgroundColor` w Konstruktorze:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Aby użyć `CircleView` właśnie utworzony, możemy dodać go jako widok podrzędny hierarchii widoku w istniejącego kontrolera, jak robiliśmy z `UILabels` i `UIButton` wcześniej, lub firma Microsoft może ładować jako widok nowego kontrolera. Poznajmy drugie.

### <a name="loading-a-view"></a>Trwa ładowanie widoku

 `UIViewController` zawiera metodę o nazwie `LoadView` który jest wywoływany przez kontrolera w celu utworzenia jego widoku. Jest to odpowiednie miejsce do utworzenia widoku i przypisz go do kontrolera `View` właściwości.

Najpierw należy kontrolera, dlatego Utwórz pusty nową klasę o nazwie `CircleController`.

W `CircleController` Dodaj następujący kod, aby ustawić `View` do `CircleView` (nie należy wywołać `base` implementacja zastąpienia):

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

Na koniec należy przedstawić kontrolera w czasie wykonywania. Umożliwia to zrobić przez dodawanie obsługi zdarzeń na przycisk przesyłania dodaliśmy wcześniej w następujący sposób:

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Teraz gdy firma Microsoft może uruchomić aplikację, a następnie naciśnij przycisk Prześlij, zostanie wyświetlony nowy widok z kółkiem:

 [![](ios-code-only-images/circles.png "Zostanie wyświetlony nowy widok z okręgu")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Tworzenie ekranu uruchamiania

A [ekran startowy](~/ios/app-fundamentals/images-icons/launch-screens.md) jest wyświetlane, gdy aplikacja jest uruchamiany jako sposób wyświetlenia użytkownikom jest elastyczny. Ponieważ ekran startowy jest wyświetlany podczas ładowania aplikacji, nie można utworzyć w kodzie jako aplikacja jest nadal ładowany do pamięci. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Gdy Twoje Tworzenie projektu programu Visual Studio, uruchomić ekranu jest dostarczany w formie pliku .xib, który znajduje się w systemie iOS **zasobów** folder wewnątrz projektu. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Jeśli Twoje tworzenia projektu systemu iOS w programie Visual Studio dla komputerów Mac, uruchom ekranu jest dostarczany w formie pliku scenorysu. 

-----

To można edytować podwójne kliknięcie jej i otwierając go w systemie iOS projektanta.

Firma Apple zaleca czy .xib lub scenorysu, plik jest używany dla aplikacji przeznaczonych dla systemu iOS 8 lub później, podczas uruchamiania albo plikiem w systemie iOS projektanta użyjesz klasy wielkości i układu automatycznego dostosowania układu tak, aby wygląda dobrze i wyświetla prawidłowo, wszystkie urządzenia rozmiary. Obraz statyczny uruchamiania może służyć oprócz .xib lub Storyboard, umożliwia obsługę aplikacji przeznaczonych dla wcześniejszych wersji.

Aby uzyskać więcej informacji na temat tworzenia ekranu uruchamiania można skorzystać z dokumentów poniżej:

- [Tworzenie ekranu uruchamiania przy użyciu .xib](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [Zarządzanie uruchamiania ekrany z Scenorys](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> Począwszy od systemu iOS 9 Apple, zaleca się, że Scenorys powinna być używana jako podstawowej metody tworzenia ekranu uruchamiania.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Tworzenie obrazu uruchamiania dla wstępnego systemu iOS 8 aplikacji

Obraz statyczny umożliwia oprócz .xib lub ekran startowy scenorysu aplikacji elementów docelowych w wersjach starszych niż system iOS 8. 

Ten statyczny obraz można ustawić w pliku Info.plist lub jako katalog zasobów (dla systemu iOS 7) w aplikacji. Należy podać oddzielne obrazy dla każdego rozmiaru urządzenia (320 x 480, 640 x 960 640 x 1136), który aplikacja może być uruchamiana. Aby uzyskać więcej informacji na temat rozmiarów ekranu uruchamiania wyświetlić [uruchamianie obrazy ekranu](~/ios/app-fundamentals/images-icons/launch-screens.md) przewodnik.

> [!IMPORTANT]
> Jeśli aplikacja nie ma żadnych ekranu uruchamiania, można zauważyć, że nie pełni dopasowania do ekranu. Jeśli jest to możliwe, należy upewnić się, że zawierają co najmniej obraz 640 x 1136 o nazwie `Default-568@2x.png` do Twojego pliku Info.plist. 



## <a name="summary"></a>Podsumowanie

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W tym artykule opisano sposób tworzenia aplikacji systemu iOS w programie Visual Studio. Analizujemy jak zbudować projekt z pusty szablon projektu, dyskutować sposobu tworzenia i dodać do okna kontroler widoku głównego. Firma Microsoft następnie pokazano, jak używać formantów z UIKit do tworzenia hierarchii widoku w kontrolerze umożliwiające tworzenie ekranu aplikacji. Obok możemy zbadać jak dokonanie widoki układ odpowiednio w różnych położeniach i widzieliśmy jak utworzyć widok niestandardowy przez podklasy `UIView`, a także załadować widoku w kontrolerze. Na koniec mamy przedstawione sposób dodawania ekran startowy do aplikacji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W tym artykule opisano sposób tworzenia aplikacji systemu iOS w programie Visual Studio dla komputerów Mac. Analizujemy jak zbudować projekt z szablonu pojedynczego widoku dyskutować sposobu tworzenia i dodać do okna kontroler widoku głównego. Firma Microsoft następnie pokazano, jak używać formantów z UIKit do tworzenia hierarchii widoku w kontrolerze umożliwiające tworzenie ekranu aplikacji. Obok możemy zbadać jak dokonanie widoki układ odpowiednio w różnych położeniach i widzieliśmy jak utworzyć widok niestandardowy przez podklasy `UIView`, a także załadować widoku w kontrolerze. Na koniec mamy przedstawione sposób dodawania ekran startowy do aplikacji.

-----




## <a name="related-links"></a>Linki pokrewne

- [SimpleLogin (przykład)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
