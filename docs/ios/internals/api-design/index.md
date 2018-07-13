---
title: Projekt interfejsu API Xamarin.iOS
description: W tym dokumencie opisano niektóre wytyczne, które używane do projektowania interfejsów API platformy Xamarin.iOS i jak odnoszą się do Objective-C.
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 275db96435639a60be89e0e3ddb7fa120a30de1c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996415"
---
# <a name="xamarinios-api-design"></a>Projekt interfejsu API Xamarin.iOS

Oprócz podstawowych podstawowej biblioteki klas, które są częścią platformy Mono [Xamarin.iOS](http://www.xamarin.com/iOS) jest dostarczany z powiązania dla różnych interfejsów API, aby umożliwić deweloperom tworzenie aplikacji natywnych dla systemów iOS przy użyciu platformy Mono dla systemu iOS.

W samym sercu Xamarin.iOS jest aparat międzyoperacyjnego, który wypełnia world języka C# za pomocą Światowej języka Objective-C, a także powiązań dla systemu iOS na podstawie C interfejsów API, takich jak CoreGraphics i [OpenGL ES](#OpenGLES).

Środowisko uruchomieniowe niskiego poziomu do komunikowania się z kodem języka Objective-C jest [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime). Na podstawie tego powiązania [Foundation](#MonoTouch.Foundation), CoreFoundation, i [UIKit](#MonoTouch.UIKit) są dostarczane.

## <a name="design-principles"></a>Zasady projektowania

Oto niektóre z naszych zasad projektowania dla powiązań platformy Xamarin.iOS (także odnoszą się do platformy Xamarin.Mac, Mono powiązań języka Objective-C w systemie macOS):

- Postępuj zgodnie z [Framework — zalecenia dotyczące projektowania](https://docs.microsoft.com/dotnet/standard/design-guidelines)
- Umożliwia deweloperom podklasy języka Objective-C klasy:

  - Pochodzi z istniejącej klasy
  - Wywołanie konstruktora podstawowego łańcucha
  - Zastępowanie metody powinno się odbywać za pomocą języka C# w zastąpienie systemu
  - Podklasy powinny współpracować z C# konstrukcje

- Nie ujawniaj deweloperom selektory języka Objective-C
- Mechanizm do wywołania dowolnego bibliotek języka Objective-C
- Wykonywanie typowych zadań języka Objective-C proste, a stwarza trudności możliwości zadania języka Objective-C
- Udostępnianie właściwości języka Objective-C jako właściwości języka C#
- Udostępnić silnie typizowanym interfejsem API:

  - Zwiększ bezpieczeństwo typów
  - Zminimalizować błędy w czasie wykonywania
  - Uzyskaj IDE IntelliSense na typy zwracane
  - Umożliwia dokumentacji okno podręczne IDE

- Zachęć eksploracji w IDE, interfejsów API:

  - Na przykład zamiast uwidaczniania tablicą ze słabą kontrolą typów, takich jak to:
    
    ```objc
    NSArray *getViews
    ```
    Udostępnianie silnego typu, następująco:
    
    ```csharp
    NSView [] Views { get; set; }
    ```
    
    To umożliwia programu Visual Studio dla komputerów Mac w celu automatycznego uzupełniania podczas przeglądania interfejsu API, że wszystkie `System.Array` operacje, które są dostępne na wartości zwracane i zezwala na zwracaną wartość, aby wziąć udział w programie LINQ.

- Natywne typy C#:

  - [`NSString` staje się `string`](~/ios/internals/api-design/nsstring.md)
  - Włącz `int` i `uint` parametry, które powinny zostać wyliczeń w języku C#, wyliczeniach i wyliczenia języka C# za pomocą `[Flags]` atrybutów
  - Zamiast niezależny od typu `NSArray` obiektów, udostępnianie tablic jako silnie typizowane tablic.
  - Dla zdarzeń i powiadomień użytkownikom wybór między:

    - Silnie typizowane wersji domyślnie
    - W przypadku zaawansowanych wersji ze słabą kontrolą typów

- Obsługa języka Objective-C wzorzec delegata:

    - System zdarzeń w języku C#
    - Udostępnianie delegatów w języku C# (wyrażenia lambda, metody anonimowe i `System.Delegate`) do interfejsów API języka Objective-C, jako bloki

### <a name="assemblies"></a>Zestawy

Xamarin.iOS obejmuje pewną liczbę zestawów, które stanowią *profilu Xamarin.iOS*. [Zestawy](~/cross-platform/internals/available-assemblies.md) strona zawiera więcej informacji.

### <a name="major-namespaces"></a>Przestrzenie nazw głównych 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) przestrzeń nazw umożliwia deweloperom światy między C# i Objective-C.
Jest to nowe powiązanie, zaprojektowany specjalnie dla systemów iOS, w oparciu o doświadczenie z Cocoa # i biblioteki Gtk #.

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

[Foundation](https://developer.xamarin.com/api/namespace/Foundation/) przestrzeń nazw zawiera typy danych podstawowych zaprojektowany pod kątem współdziałania z struktury Foundation języka Objective-C, która jest częścią systemu iOS i jest podstawą zorientowana obiektowo, Programowanie w Objective-C.

Xamarin.iOS odzwierciedla w języku C# hierarchii klas z Objective-C. Na przykład klasa bazowa języka Objective-C [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) można używać z kodu C# za pomocą [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).

Chociaż ta przestrzeń nazw zapewnia powiązania dla typów podstawowych Foundation języka Objective-C, w niektórych przypadkach firma Microsoft obecnie typów podstawowych typów .NET. Na przykład:

- Zamiast zajmowanie się [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) i [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), środowisko uruchomieniowe udostępnia je jako języka C# [ciąg](xref:System.String)s i silnie typizowaną [tablicy](xref:System.Array)s w całym Interfejs API.

- Pomocnik różne interfejsy API są widoczne w tym miejscu umożliwiają deweloperom do powiązania innej interfejsów API języka Objective-C, iOS, innych interfejsów API ani nowych interfejsów API, które obecnie nie są powiązane przez rozszerzenia Xamarin.iOS.

Aby uzyskać szczegółowe informacje na temat wiązania interfejsów API, zobacz [Generator powiązanie interfejsu Xamarin.iOS](~/cross-platform/macios/binding/binding-types-reference.md) sekcji.


##### <a name="nsobject"></a>NSObject

[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) typu jest podstawą dla wszystkich powiązań języka Objective-C. Typy Xamarin.iOS duplikatów dwóch klas typów z CocoaTouch interfejsów API dla systemu iOS: typy języka C (zazwyczaj określona jako typy CoreFoundation) i typy języka Objective-C, (te wszystkie pochodzi od klasy NSObject).

Dla każdego typu, która odzwierciedla niezarządzany typ, istnieje możliwość uzyskania natywnego obiektu za pomocą [obsługi](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) właściwości.

Gdy narzędzie Mono zapewni wyrzucania elementów bezużytecznych dla wszystkich obiektów, `Foundation.NSObject` implementuje [System.IDisposable](xref:System.IDisposable) interfejsu. Oznacza to, można jawnie zwalniać zasoby wszelkie NSObject danego bez konieczności oczekiwania na moduł Garbage Collector moment w. Jest to ważne w przypadku korzystania z dużymi NSObjects, na przykład UIImages, który może pomieścić wskaźników do dużych bloków danych.

Jeśli danego typu musi wykonać finalizacja deterministyczna, Zastąp [metoda NSObject.Dispose(bool)](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose) parametru do metody Dispose to "bool disposing", i jeśli ustawiona wartość true, jego oznacza to, ponieważ jest wywoływana metoda Dispose użytkownika jawnie wywoływane metody Dispose () w obiekcie. Jeśli wartość wynosi false, oznacza to, że jest wywoływana metoda Dispose (bool disposing) z finalizatora w wątku finalizatora. []()


##### <a name="categories"></a>Kategorie

Uruchamianie przy użyciu rozszerzenia Xamarin.iOS 8.10 możliwe jest utworzenie kategorii języka Objective-C za pomocą języka C#.

Odbywa się przy użyciu `Category` atrybut określający typ rozszerzenie jako argument do atrybutu. Poniższy przykład rozszerzy NSString.

    [Category (typeof (NSString))]

Każda metoda kategorii jest przy użyciu normalnych mechanizmu eksportowania metod języka Objective-C za pomocą `Export` atrybutu:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Wszystkie metody rozszerzenia zarządzane musi być statyczna, ale istnieje możliwość tworzenia metod wystąpienia języka Objective-C przy użyciu standardowej składni metody rozszerzenia w języku C#:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

pierwszy argument dla metody rozszerzenia. zostanie ona wystąpienie, na którym została wywołana metoda.

Pełny przykład:

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
}
```

W tym przykładzie doda metodę wystąpienia toUpper natywnej do klasy NSString, która może być wywoływany z Objective-C.

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAudoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

Jeden scenariusz, gdzie jest to przydatne, jest dodanie metody do całego zestawu klas w bazie kodu, na przykład dzięki temu upewnisz się wszystkie `UIViewController` wystąpień raportu, czy też obrócić:

```csharp
[Category (typeof (UINavigationController))]
class Rotation_IOS6 {
      [Export ("shouldAutorotate:")]
      static bool ShouldAutoRotate (this UINavigationController self)
      {
          return true;
      }
}
```


##### <a name="preserveattribute"></a>PreserveAttribute

PreserveAttribute jest niestandardowy atrybut, który informuje mtouch — narzędzia wdrażania platformy Xamarin.iOS — Aby zachować typu lub składowej typu podczas fazy, gdy aplikacja jest przetwarzany zmniejszyć jego rozmiar.

Każdy element członkowski, który nie jest połączona statycznie przez aplikację jest warunkiem usunięcia. W związku z tym ten atrybut służy do oznaczania elementów członkowskich, które nie są statyczne odwołania, ale nadal wymagane przez aplikację.

Na przykład tworzenia wystąpienia typu dynamicznego, można zachować domyślny konstruktor obiektu typów. Jeśli używasz serializacji XML, można zachować właściwości typów.

Ten atrybut można zastosować, każdy członek typu lub samego typu. Jeśli chcesz zachować całą typu, możesz użyć składni [zachować (AllMembers = true)] typu.

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>UIKit

[UIKit](https://developer.xamarin.com/api/namespace/UIKit/) przestrzeń nazw zawiera mapowanie jeden do jednego dla wszystkich składników interfejsu użytkownika, które tworzą CocoaTouch w postaci klas języka C#. Interfejs API została zmodyfikowana, aby można było zgodne z konwencjami w używany w języku C#.

C# delegatów są dostarczane dla typowych operacji. Zobacz [delegatów](#Delegates) sekcji, aby uzyskać więcej informacji.

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

W przypadku OpenGLES, firma Microsoft rozpowszechniać [zmodyfikowana wersja](https://developer.xamarin.com/api/namespace/OpenTK/) z [OpenTK](http://www.opentk.com/) interfejsu API, zorientowany obiektowo powiązanie ze specyfikacji OpenGL, która została zmodyfikowana, aby użyć CoreGraphics typy i struktury danych, a także Uwidacznianie tylko Funkcje, które są dostępne w systemie iOS.

Funkcja OpenGLES 1.1 jest dostępna za pośrednictwem typu ES11.GL, udokumentowane [tutaj](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) typu.

Funkcja OpenGLES w wersji 2.0 jest dostępna za pośrednictwem typu ES20.GL, udokumentowane [tutaj](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) typu.

Funkcja OpenGLES 3.0 jest dostępna za pośrednictwem typu ES30.GL, udokumentowane [tutaj](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) typu.


### <a name="binding-design"></a>Projekt powiązania

Rozszerzenie Xamarin.iOS nie jest jedynie powiązania dla podstawowej platformy języka Objective-C. Rozszerza system typów .NET i wysyłania systemu, aby lepiej blend C# i Objective-C.

Tak, jak P/Invoke jest użytecznym narzędziem do wywołania bibliotek natywnych w systemach Windows i Linux lub jako IJW pomocy technicznej może służyć do współdziałania z modelem COM na Windows, platformy Xamarin.iOS rozszerza środowisko uruchomieniowe do obsługi obiekty języka C# powiązania do obiektów języka Objective-C.

Dyskusja w ciągu następnych kilka sekcji nie jest konieczne dla użytkowników, którzy tworzenia aplikacji platformy Xamarin.iOS, ale pomaga deweloperom Dowiedz się, jak czynności są wykonywane i pomoże im podczas tworzenia bardziej skomplikowanych aplikacji.



#### <a name="types"></a>Types

Gdzie go miało sens, typów języka C# są udostępniane zamiast niskiego poziomu typów Foundation, aby środowisko C#.  Oznacza to, że [interfejs API korzysta z typu "string" C# zamiast NSString](~/ios/internals/api-design/nsstring.md) i używa zamiast uwidaczniania NSArray silnie typizowaną tablice języka C#.

Ogólnie rzecz biorąc, w przypadku projektu Xamarin.iOS i Xamarin.Mac znajdujące się poniżej `NSArray` obiektu nie jest uwidaczniana. Zamiast tego, środowisko wykonawcze automatycznie konwertuje `NSArray`s na silnie typizowaną tablice niektórych `NSObject` klasy. Tak Xamarin.iOS uwidacznia metodę ze słabą kontrolą typów, takich jak GetViews do zwrócenia NSArray:

```csharp
NSArray GetViews ();
```

Zamiast tego powiązania udostępnia silnie typizowaną wartość zwracana następująco:

```csharp
UIView [] GetViews ();
```

Kilka metod udostępnianych w `NSArray`, dla przypadki brzegowe, gdzie możesz chcieć użyć `NSArray` bezpośrednio, ale ich użycie nie jest zalecane w powiązaniu interfejsu API.

Ponadto **klasycznego interfejsu API** zamiast uwidaczniania `CGRect`, `CGPoint` i `CGSize` z interfejsu API CoreGraphics, obejmujących zastąpiliśmy `System.Drawing` implementacje `RectangleF`, `PointF`i `SizeF` zgodnie z nich może pomóc deweloperom zachować istniejący kod ze specyfikacji OpenGL, który korzysta z biblioteki OpenTK. Korzystając z nowego 64-bitowych **ujednoliconego interfejsu API**, interfejs API CoreGraphics powinny być używane.

<a name="Inheritance" />

#### <a name="inheritance"></a>Dziedziczenie

Projekt interfejsu API Xamarin.iOS umożliwia deweloperom rozszerzenie natywnych typów języka Objective-C w taki sam sposób, że może rozszerzyć się typ C#, za pomocą słowa kluczowego "override" w klasie pochodnej, a także tworzenie łańcuchów do podstawowego wdrożenia przy użyciu "podstawowe" słowo kluczowe języka C#.

Ten projekt umożliwia deweloperom uniknąć radzenia sobie z selektory języka Objective-C, jako część procesu projektowania, ponieważ cały system języka Objective-C jest opakowana wewnątrz biblioteki rozszerzenia Xamarin.iOS.


#### <a name="types-and-interface-builder"></a>Typy i programu Interface Builder

Podczas tworzenia klasy .NET, które są wystąpieniami typów utworzonych przez programu Interface Builder, należy podać konstruktora, który przyjmuje pojedynczy `IntPtr` parametru.
Jest to wymagane do powiązania wystąpienia obiektu zarządzanego za pomocą niezarządzanych obiektów.
Kod składa się z jednym wierszu, w następujący sposób:

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

<a name="Delegates" />


#### <a name="delegates"></a>Delegaty

Objective-C i C# mają różne znaczenie dla delegata słowo w każdym języku.

W świecie języka Objective-C, a w dokumentacji, który można znaleźć online o CocoaTouch obiekt delegowany jest zazwyczaj wystąpienia klasy, które będą odpowiadać na zestaw metod. Może to być bardzo podobne do języka C# interfejsu, z tą różnicą, możliwe, że metody są zawsze obowiązkowe.

Ci delegaci odgrywa ważną rolę w UIKit i innych interfejsów API CocoaTouch. Są one używane do wykonywania różnych zadań:

-  Do udostępniania powiadomień o kodzie (podobny do dostarczania zdarzeń w języku C# lub biblioteki Gtk +).
-  Aby zaimplementować modele formanty wizualizacji danych.
-  Dysk zachowanie kontrolki.


Wzorzec programowania zaprojektowano tak, aby zminimalizować tworzenia klasy pochodne, aby zmienić zachowanie kontrolki. To rozwiązanie jest podobny do innych zestawów narzędzi z graficznym interfejsem użytkownika wykonano w ciągu lat duchu: firmy Gtk sygnały, Qt miejsc, zdarzenia Winforms, WPF i Silverlight zdarzenia i tak dalej. Aby uniknąć konieczności setki interfejsów (po jednym dla każdej akcji) bądź wymagają takiej deweloperom wdrażanie ramek zbyt wiele metod, których nie ma potrzeby, Objective-C obsługuje definicje opcjonalnych metody. To różni się od C# interfejsy, które wymagają wszystkie metody do zaimplementowania.

W klasach języka Objective-C, zobaczysz, że klasy korzystające z tego wzorca programowania Ujawnij właściwość, zwykle nazywane `delegate`, co jest wymagane do zaimplementowania obowiązkowe części interfejsu i zero lub więcej, z opcjonalnych części.

W rozszerzeniu Xamarin.iOS trzy mechanizmy wzajemnie się wykluczają, aby powiązać te obiekty delegowane są oferowane:

1.  [Za pośrednictwem zdarzeń](#Via_Events).
2.  [Silnie typizowane za pośrednictwem `Delegate` właściwości](#StrongDelegate)
3.  [Typowaniem za pośrednictwem `WeakDelegate` właściwości](#WeakDelegate)

Na przykład, rozważmy [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) klasy. Wywołuje się to do [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) wystąpienia, która jest przypisana do [delegować](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) właściwości.

<a name="Via_Events" />

##### <a name="via-events"></a>Za pomocą zdarzeń

Dla wielu typów Xamarin.iOS automatycznie utworzy odpowiednie delegata, która będzie przekazywać `UIWebViewDelegate` wywołań na zdarzenia języka C#. Aby uzyskać `UIWebView`:

-  [WebViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) metody jest mapowany na [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/) zdarzeń.
-  [WebViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) metody jest mapowany na [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/) zdarzeń.
-  [WebView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) metody jest mapowany na [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/) zdarzeń.

Na przykład to prosty program rejestruje czas rozpoczęcia i zakończenia, gdy ładowanie sieci web wyświetlają:

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>Za pomocą właściwości

Zdarzenia są przydatne, gdy może istnieć więcej niż jeden subskrybent zdarzenia. Ponadto zdarzenia są ograniczone do przypadków, gdy zwraca żadnej wartości, które są dostępne z kodu.

W przypadkach, w którym kod powinien zostać zwrócona wartość postanowiliśmy zamiast właściwości. Oznacza to, że tylko jedna metoda można ustawić w danym momencie w obiekcie.

Na przykład, można użyć tego mechanizmu można odrzucić klawiatury na ekranie na obsługę programu `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

`UITextField`Firmy `ShouldReturn` właściwości w tym przypadku przyjmuje jako argument zwraca wartość typu wartość logiczna, która określa, czy TextField zrobić coś o naciśnięcie przycisku zwracany delegata. W naszym metoda zostanie zwrócona *true* do obiektu wywołującego, ale możemy też usunąć klawiatury na ekranie (tak się stanie, gdy wywołuje element textfield `ResignFirstResponder`).

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>Silnie Typizowane za pośrednictwem właściwości delegata

Jeśli wolisz nie należy używać zdarzeń, możesz podać własne [UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/) podklasy i przypisz ją do [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/) właściwości. Po przypisaniu UIWebView.Delegate mechanizm wysyłania zdarzeń UIWebView przestaną działać, a metody UIWebViewDelegate zostanie wywołany, gdy występują odpowiednie zdarzenia.

Na przykład tego typu prostego rejestruje czas potrzebny do załadowania widoku sieci web:

```csharp
class Notifier : UIWebViewDelegate  {
    DateTime startTime, endTime;

    public override LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    public override LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}
```

Powyżej jest używany w kodzie w następujący sposób:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

Powyższe utworzy UIWebViewer i zleca go, aby wysyłać komunikaty do wystąpienia zgłaszającego, klasy, które utworzyliśmy, aby odpowiadać na komunikaty.

Ten wzorzec jest również używane do kontrolowania zachowania w przypadku pewnych formantów, na przykład w przypadku UIWebView [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/) właściwość umożliwia `UIWebView` wystąpienie kontrolki czy `UIWebView` załaduje Strona, czy nie.

Wzorzec jest również używana do dostarczania danych na żądanie dla kilku formantów. Na przykład [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/) formant jest formantem zaawansowane renderowanie tabeli — i zarówno wyglądu, jak i zawartości są regulowane przez wystąpienie [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>Typowaniem za pomocą właściwości WeakDelegate

Oprócz silnie typizowane właściwości jest również słabe delegat wpisane, który umożliwia deweloperom można powiązać rzeczy inaczej, jeśli to konieczne.
Wszędzie silnie typizowaną `Delegate` właściwość jest uwidaczniana w powiązaniu firmy Xamarin.iOS odpowiednią `WeakDelegate` właściwość również jest uwidaczniana.

Korzystając z `WeakDelegate`, ponosisz odpowiedzialność za prawidłowe urządzanie przy użyciu klasy [wyeksportować](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) atrybutu, aby określić selektora. Na przykład:

```csharp
class Notifier : NSObject  {
    DateTime startTime, endTime;

    [Export ("webViewDidStartLoad:")]
    public void LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    [Export ("webViewDidFinishLoad:")]
    public void LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}

[...]

var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.WeakDelegate = new Notifier ();
```

Należy pamiętać, że raz `WeakDelegate` przypisany właściwość `Delegate` nie zostanie użyta właściwość. Ponadto w przypadku zastosowania metody w dziedziczone klasy bazowej, którą chcesz [eksportu], należy go publiczną metodę.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Mapowanie wzorzec delegata języka Objective-C-C&#35;

Po wyświetleniu przykłady języka Objective-C, które wyglądają następująco:

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

To powoduje, że język do tworzenia i utworzyć wystąpienie klasy "SomethingDelegate" i przypisania wartości do właściwości delegata na zmiennej foo. Ten mechanizm jest obsługiwana przez platformy Xamarin.iOS i C# składnia to:

```csharp
foo.Delegate = new SomethingDelegate ();
```

W rozszerzeniu Xamarin.iOS zostały zamieszczone silnie typizowanych klas, które mapują do języka Objective-C delegować klasy. Aby korzystać z nich, będziesz Tworzenie podklasy i przesłanianie metody zdefiniowane przez implementację interfejsu Xamarin.iOS firmy. Aby uzyskać więcej informacji na temat sposobu działania zobacz sekcję "modele" poniżej.


##### <a name="mapping-delegates-to-c35"></a>Mapowanie delegatów do języka C&#35;

UIKit ogólnie rzecz biorąc używa języka Objective-C delegatów w dwóch formach.

Pierwszy formularz udostępnia interfejs dla składników modelu. Na przykład jako mechanizm do dostarczania danych na żądanie dla widoku, takie jak funkcji magazynu danych dla widoku listy.  W takich przypadkach należy zawsze utworzyć wystąpienie klasy właściwe i przypisać zmiennej.

W poniższym przykładzie firma Microsoft zapewnia `UIPickerView` z implementacją dla modelu, który używa ciągów:

```csharp
public class SampleTitleModel : UIPickerViewTitleModel {

    public override string TitleForRow (UIPickerView picker, nint row, nint component)
    {
        return String.Format ("At {0} {1}", row, component);
    }
}

[...]

pickerView.Model = new MyPickerModel ();
```

Druga forma jest zapewnienie powiadomienia dla zdarzenia. W przypadkach mimo że nadal uwidaczniamy interfejsu API w postaci opisanych powyżej, oferujemy również zdarzenia języka C#, które powinny być łatwiejszy w obsłudze dla szybkich operacji i zintegrowana z anonimowych delegaci i wyrażenia lambda w języku C#.

Na przykład, możesz zasubskrybować `UIAccelerometer` zdarzenia:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

Dostępne są dwie opcje, z których one mieć sens, ale programistą należy wybrać jednego lub drugiego. Jeśli utworzysz wystąpienie silnie typizowany obiekt odpowiadający w trybie/delegata i przypisać ją, zdarzenia języka C# nie będzie działać. Użycie zdarzeń języka C#, nigdy nie można wywołać metody w klasie obiektu odpowiadającego w trybie/delegata.

Poprzedni przykład, jak używać `UIWebView` mogą być napisane przy użyciu języka C# 3.0 wyrażeń lambda w następujący sposób:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>Odpowiadanie na zdarzenia

W kodzie języka Objective-C czasami procedury obsługi zdarzeń dla wielu formantów i dostawców informacji w przypadku wielu kontrolek będą obsługiwane w tej samej klasy. Jest to możliwe, ponieważ klasy odpowiadanie na wiadomości, a tak długo, jak klasy, odpowiadanie na wiadomości, można połączyć ze sobą obiektów.

Zgodnie z wcześniej opisem Xamarin.iOS obsługuje zarówno C# oparte na zdarzeniach model programowania, a wzorzec delegata języka Objective-C, umożliwiającego utworzenie nowej klasy, implementuje delegata i zastępuje żądanej metody.

Użytkownik może również na potrzeby obsługi wzorca Objective-C gdzie obiektów odpowiadających w wielu różnych operacji znajdują się w tym samym wystąpieniu klasy. W tym celu należy jednak trzeba będzie korzystać z funkcji niskiego poziomu powiązanie interfejsu Xamarin.iOS.

Na przykład, jeżeli chcesz swojej klasy, aby odpowiedzieć na obu `UITextFieldDelegate.textFieldShouldClear`: komunikat i `UIWebViewDelegate.webViewDidStartLoad`: w tym samym wystąpieniu klasy, trzeba użyć deklaracji atrybutu [eksportu]:

```csharp
public class MyCallbacks : NSObject {
    [Export ("textFieldShouldClear:"]
    public bool should_we_clear (UITextField tf)
    {
        return true;
    }

    [Export ("webViewDidStartLoad:")]
    public void OnWebViewStart (UIWebView view)
    {
        Console.WriteLine ("Loading started");
    }
}
```

Nazwy języka C# dla metod nie są ważne; wszystkie, które ma znaczenie są ciągi przekazywane z atrybutem [eksportu].

Korzystając z tego stylu programowania, upewnij się, że parametry języka C# zgodne rzeczywiste typy, które przekazują aparatu wykonawczego.

<a name="Models" />

#### <a name="models"></a>Modele

W obiektach magazynu UIKit lub obiektów, które są implementowane przy użyciu klas pomocy te są zwykle określane w kodzie języka Objective-C pełnomocnikiem, i są implementowane jako protokołów.

Języka Objective-C protokoły są podobne do interfejsów, ale obsługują metody opcjonalne — oznacza to, że nie wszystkie metody muszą zostać zaimplementowane dla protokołu do pracy.

Istnieją dwa sposoby wdrażania modelu. Można ją wdrożyć ręcznie lub użyć istniejących definicji silnie typizowanych.


Niezbędne jest ręczne mechanizm, przy próbie implementuje klasę, która nie została powiązana przez rozszerzenia Xamarin.iOS. Jest bardzo proste:

-  Flaga klasy do rejestracji przy użyciu środowiska uruchomieniowego
-  Zastosuj atrybut [eksportu] o nazwie rzeczywiste selektor na poszczególnych metod, które chcesz zastąpić
-  Utwórz wystąpienie klasy i przekazać go.

Na przykład następujące zaimplementować tylko jednej z metod opcjonalny w definicji protokołu UIApplicationDelegate:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Nazwa selektora języka Objective-C ("applicationDidFinishLaunching:") jest zadeklarowana za pomocą atrybutu eksportu i klasy jest zarejestrowane w usłudze `[Register]` atrybutu.

Xamarin.iOS udostępnia silnie typizowaną deklaracji, gotowych do użycia, które nie wymagają ręcznego powiązania. Aby obsługiwać ten model programowania, w czasie wykonywania rozszerzenia Xamarin.iOS obsługuje atrybutu [Model] w deklaracji klasy. Informuje, że środowisko uruchomieniowe, które go powinna nie Podłączanie wszystkie metody w klasie, chyba że metody są jawnie jest zaimplementowana.

Oznacza to, że w strukturze UIKit, klas, które reprezentują protokołu za pomocą metod opcjonalne są zapisywane następująco:

```csharp
[Model]
public class SomeViewModel : NSObject {
    [Export ("someMethod:")]
    public virtual int SomeMethod (TheView view) {
       throw new ModelNotImplementedException ();
    }
    ...
}
```

Można wdrożyć modelu, który tylko niektóre metody implementuje wszystko, co należy zrobić to Przesłaniaj metody interesuje i Ignoruj pozostałych metod. Środowisko uruchomieniowe będzie tylko podpiąć zastąpionych metod, nie oryginalnej metody służące do świata języka Objective-C.

Jest równoważny poprzedniemu ręczne:

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Korzyści to brak konieczności zagłębiania się w pliki nagłówkowe języka Objective-C, aby znaleźć selektor, typy argumentów lub mapowanie do języka C# i uzyskasz intellisense w programie Visual Studio dla komputerów Mac, wraz z Silne typy


#### <a name="xib-outlets-and-c35"></a>Gniazda XIB i języka C&#35;

> [!IMPORTANT]
> W tej sekcji opisano Integracja środowiska IDE z gniazda w przypadku korzystania z plików XIB. Korzystając z projektanta platformy Xamarin dla systemu iOS, to wszystkie zastępuje, wprowadzając nazwę w polu **tożsamości > nazwa** w sekcji właściwości w IDE, jak pokazano poniżej:
>
> [![](images/designeroutlet.png "Wprowadź nazwę elementu w narzędziu iOS Designer")](images/designeroutlet.png#lightbox)
>
>Aby uzyskać więcej informacji na temat narzędzia iOS Designer, przejrzyj [wprowadzenie do narzędzia iOS Designer](~/ios/user-interface/designer/introduction.md#how-it-works) dokumentu.

Niskiego poziomu opisano sposób integrowania gniazd w języku C# i podano dla zaawansowanych użytkowników platformy Xamarin.iOS. Gdy za pomocą programu Visual Studio dla komputerów Mac, mapowanie odbywa się automatycznie w tle za pomocą wygenerowany kod podczas lotu.

Podczas projektowania interfejsu użytkownika za pomocą programu Interface Builder tylko projektowanie wyglądu aplikacji i umożliwi nawiązanie połączenia niektóre domyślne. Jeśli chcesz programowo pobrać informacje, zmiany zachowania formantu w czasie wykonywania lub zmodyfikować formantu w czasie wykonywania jest konieczne niektóre formanty można powiązać z kodu zarządzanego.

Można to zrobić w kilku krokach:

1.  Dodaj **deklaracji ujścia** do Twojej **właściciel pliku**.
1.  Połącz Twoją kontrolą w celu **właściciel pliku**.
1.  Store interfejsu użytkownika, a także połączenia w pliku XIB/NIB.
1.  Załaduj plik NIB w czasie wykonywania.
1.  Dostęp do gniazda zmiennej.


Kroki (1) za pośrednictwem (3) znajdują się w dokumentacji firmy Apple do tworzenia interfejsów za pomocą programu Interface Builder.

Korzystając z platformy Xamarin.iOS, aplikację należy utworzyć klasę, która pochodzi z klasą UIViewController. Wdrażana jest ona następująco:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Następnie można załadować usługi ViewController z pliku NIB, możesz to zrobić:

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Spowoduje to załadowanie interfejsu użytkownika z NIB. Teraz Aby uzyskać dostęp do gniazda, należy poinformować środowiska uruchomieniowego, które chcemy uzyskać do nich dostęp. Aby to zrobić, `UIViewController` podklasy musi zadeklarować właściwości i dodawać do nich adnotacje z atrybutem [Connect]. Jak to:

```csharp
[Connect]
UITextField UserName {
    get {
        return (UITextField) GetNativeField ("UserName");
    }
    set {
        SetNativeField ("UserName", value);
    }
}
```

Implementacja właściwości jest ten, który faktycznie pobiera i przechowuje wartość dla rzeczywistego typu natywnego.

Nie trzeba już martwić się o to przy użyciu programu Visual Studio dla komputerów Mac i InterfaceBuilder. Visual Studio dla komputerów Mac automatycznie odzwierciedla wszystkich gniazd zadeklarowane z kodem w częściowej klasie, która jest kompilowana jako część Twojego projektu.

#### <a name="selectors"></a>Selektory

Podstawowa koncepcja programowania w języku Objective C jest selektorów. Będzie można często jest opracowywane przez interfejsy API, które wymagają przekazania selektora lub oczekuje, że kod odpowiedzi z selektorem.

Tworzenie nowego selektory w języku C# jest bardzo proste — wystarczy utworzyć nowe wystąpienie klasy `ObjCRuntime.Selector` klasy i użyj wyniku w dowolnym miejscu w interfejsie API, który go wymaga. Na przykład:

```csharp
var selector_add = new Selector ("add:plus:");
```

Dla języka C# metoda odpowiedź na wywołania selektor, musi on dziedziczyć z `NSObject` typu i metodę języka C# musi posiadać przy użyciu nazwy selektora `[Export]` atrybutu. Na przykład:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Należy pamiętać, że selektor nazwy **musi** dokładnie odpowiadać, łącznie z wszystkich pośrednich i końcowe średnikami (":"), jeśli jest obecny.

#### <a name="nsobject-constructors"></a>Konstruktory NSObject

Większość klas w rozszerzeniu Xamarin.iOS, która pochodzi od `NSObject` udostępni konstruktory specyficzne funkcje obiektu, ale także udostępni różne konstruktory, które nie są natychmiast oczywisty.

Konstruktory są używane w następujący sposób:

```csharp
public Foo (IntPtr handle)
```

Ten konstruktor jest używany do utworzenia wystąpienia klasy, gdy środowisko uruchomieniowe potrzebuje do mapowania klasy do klasy niezarządzane. Dzieje się tak, podczas ładowania pliku XIB/NIB.  W tym momencie środowiska uruchomieniowego języka Objective-C zostanie utworzony obiekt w świecie niezarządzane, a ten konstruktor zostanie wywołana w celu zainicjowania zarządzanych po stronie.

Zazwyczaj jest wszystko, co należy zrobić, wywołać konstruktora bazowego z parametrem uchwyt, a w treści, wykonaj wszelkie inicjowania, które są niezbędne.

```csharp
public Foo ()
```

Jest to domyślny konstruktor dla klasy, a w rozszerzeniu Xamarin.iOS podane klasy, to między inicjuje klasy Foundation.NSObject i wszystkie klasy i na końcu, tworzy powiązanie to języka Objective-C `init` metody w klasie.

```csharp
public Foo (NSObjectFlag x)
```

Ten konstruktor jest używany do inicjowania wystąpienia, ale uniemożliwić kodu wywołującego metodę "init" języka Objective-C na końcu. Jest zazwyczaj używana podczas rejestrowania już inicjowania (Jeśli używasz `[Export]` na konstruktora) lub kiedy jeszcze inicjalizacji za pośrednictwem innego średniej.

```csharp
public Foo (NSCoder coder)
```

Ten konstruktor znajduje się w sytuacjach, gdy obiekt jest inicjowany z wystąpienia NSCoding. Aby uzyskać więcej informacji, zobacz firmy Apple [archiwa i przewodnik programowania w serializacji.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>Wyjątki

Projekt interfejsu API Xamarin.iOS nie powoduje wyjątki języka Objective-C jako wyjątki w języku C#. Projekt wymusza, aby nie wyrzucania elementów można w pierwszej kolejności wysyłane do środowiska języka Objective-C i że ewentualne wyjątki, które muszą być tworzone są produkowane przez powiązanie, sam, przed nieprawidłowe dane nigdy nie są przekazywane do świata języka Objective-C.

#### <a name="notifications"></a>Powiadomienia

W systemie iOS i OS X deweloperzy mogą subskrybować powiadomienia, które są emitowane z odpowiedniej platformy. Jest to wykonywane przy użyciu `NSNotificationCenter.DefaultCenter.AddObserver` metody. `AddObserver` Metoda przyjmuje dwa parametry jednym jest powiadomienie, które chcesz subskrybować; drugi to metoda do wywołania, gdy zostanie wywołane przez powiadomienie.

W Xamarin.iOS i Xamarin.Mac kluczy dla różnych powiadomień znajdują się w klasie, która wyzwala powiadomienia. Na przykład powiadomienia zgłoszone przez `UIMenuController` znajdują się jako `static NSString` właściwości `UIMenuController` klas, które kończą się o nazwie "Powiadomienie".

### <a name="memory-management"></a>Zarządzanie pamięcią

Xamarin.iOS ma moduł odśmiecania pamięci, który zajmie się przy zwalnianiu zasobów dla Ciebie, gdy są one już używane. Oprócz modułu odśmiecania pamięci, wszystkie obiekty, dziedziczyć `NSObject` zaimplementować `System.IDisposable` interfejsu.

#### <a name="nsobject-and-idisposable"></a>NSObject i interfejs IDisposable

Udostępnianie `IDisposable` interfejs jest wygodny sposób pomoc deweloperom w przy zwalnianiu obiektów hermetyzujących może dużych bloków pamięci (na przykład `UIImage` wygląda po prostu nieszkodliwie wskaźnika, ale może wskazywać obraz 2 MB ) i inne ważne i ograniczone zasoby (na przykład wideo buforu dekodowania).

NSObject implementuje interfejs IDisposable, a także [wzorzec .NET Dispose](http://msdn.microsoft.com/library/fs2xkftw.aspx). Umożliwia to deweloperom tej podklasy NSObject, aby zastąpić zachowanie Dispose i wersji swoje zasoby na żądanie. Na przykład rozważmy ten kontroler widoku, który utrzymuje wokół wiele obrazów:

```csharp
class MenuViewController : UIViewController {
    UIImage breakfast, lunch, dinner;
    [...]
    public override void Dispose (bool disposing)
    {
        if (disposing){
             if (breakfast != null) breakfast.Dispose (); breakfast = null;
             if (lunch != null) lunch.Dispose (); lunch = null;
             if (dinner != null) dinner.Dispose (); dinner = null;
        }
        base.Dispose (disposing)
    }
}
```

Po usunięciu obiektu zarządzanego nie jest już przydatna. Nadal masz odwołania do obiektów, ale obiekt na wszystkich intents i purposes jest nieprawidłowy w tym momencie. Niektóre interfejsy API platformy .NET to upewnij się, zwracając objectdisposedexception — Jeśli zostanie podjęta próba dostępu do żadnych metod dla zlikwidowanego obiektu, na przykład:

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

Nawet wtedy, gdy można nadal uzyskać dostęp do zmiennej "image", że to naprawdę nieprawidłowe odwołanie, a nie wskazuje na obiekt języka Objective-C, który jest przechowywany obraz.

Ale usuwania obiektu w języku C# nie oznacza to, czy obiekt musi zostać zniszczone. To wszystko, co możesz zrobić, zwolnij odwołanie, w której C# do obiektu. Jest to możliwe, może być trzymane odwołań wokół na własny użytek cocoa dla środowiska. Na przykład, jeśli Ustawianie właściwości Image UIImageView do obrazu, a następnie usunąć obrazu, podstawowej UIImageView miały własne odwołanie i zapewnią odwołanie do tego obiektu, dopóki zostanie zakończone jej używać.

#### <a name="when-to-call-dispose"></a>Kiedy wywołać metodę Dispose

Powinny wywoływać metodę Dispose, gdy będą potrzebne w obiekcie możliwość pozbycia platformy Mono. Przypadek użycia możliwe jest, gdy narzędzie Mono nie zna swoje NSObject faktycznie zawiera odwołanie do ważnych zasobów, takich jak pamięci lub pulę informacji. W takich przypadkach należy wywołać metodę Dispose, aby od razu Zwolnij odwołanie do pamięci, zamiast czekać, aż platformy Mono wykonać cykl zbierania wyrzucania elementów.

Wewnętrznie, gdy narzędzie Mono tworzy [NSString odwołuje się z ciągów języka C#](~/ios/internals/api-design/nsstring.md), zlikwiduje je od razu, aby zmniejszyć ilość pracy moduł zbierający elementy bezużyteczne musi wykonać. Im mniej obiektów około do czynienia, tym szybciej GC zostaną uruchomione.

#### <a name="when-to-keep-references-to-objects"></a>Kiedy należy zachować odwołania do obiektów

Jeden efekt uboczny zawierającej automatyczne zarządzanie pamięcią to, że GC będzie usuwanie nieużywanych obiektów tak długo, jak istnieją żadnych odwołań do nich. Czasami może to mieć Zaskakujące efekty uboczne, na przykład, jeśli tworzysz zmienną lokalną, aby pomieścić kontroler widoku najwyższego poziomu lub z oknem najwyższego poziomu, a następnie konieczność tych znikają w tle kopii.

Jeśli użytkownik nie przechowuje odwołania w Twojej statyczny lub zmienne wystąpienia obiektów, narzędzie Mono trafem korzysta będzie wywoływać metodę Dispose() na nich, a następnie zwalnia odwołanie do obiektu. Ponieważ może to być tylko oczekujących odwołania, środowisko uruchomieniowe języka Objective-C zniszcz obiekt.

## <a name="related-links"></a>Linki pokrewne

- [Powiązywanie pól](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
