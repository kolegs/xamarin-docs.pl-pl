---
title: Projekt interfejsu API
description: Perspektywy na projekt interfejsu API platformy Xamarin.iOS
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: b7604633a5dfad6134d7b549299194ab6707a865
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="api-design"></a>Projekt interfejsu API

Oprócz podstawowych podstawowej biblioteki klas, które są częścią Mono [Xamarin.iOS](http://www.xamarin.com/iOS) jest dostarczany z powiązań dla różnych interfejsów API umożliwiają deweloperom tworzenie aplikacji natywnej systemu iOS z Mono z systemem iOS.

Fundament Xamarin.iOS jest aparat międzyoperacyjnego, który stanowi world C# z world Objective-C, a także powiązań dla systemu iOS na podstawie C interfejsów API, takich jak CoreGraphics i [OpenGL ES](#OpenGLES).

Środowisko uruchomieniowe niskiego poziomu do komunikowania się z kodu języka Objective C jest [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime). U góry tego powiązania [Foundation](#MonoTouch.Foundation), CoreFoundation, i [UIKit](#MonoTouch.UIKit) są udostępniane.

## <a name="design-principles"></a>Zasady projektowania

Oto nasze zasady projektowania dla powiązania Xamarin.iOS (również dotyczą one Xamarin.Mac, Mono powiązań dla języka Objective-C w macOS):

- Wykonaj [zaleceń dotyczących projektowania Framework](https://docs.microsoft.com/dotnet/standard/design-guidelines)
- Umożliwiają deweloperom klasy podklasy Objective-C:

  - Pochodzi z istniejącej klasy
  - Wywołanie konstruktora podstawowego łańcucha
  - Zastępowanie metody ma się odbywać z C# przez zastąpienie systemu
  - Podklasy powinny współpracować z C# konstrukcje

- Nie ujawniaj deweloperom selektorów Objective-C
- Mechanizm do wywołania dowolnego bibliotek języka Objective C
- Należy typowych zadań Objective-C łatwe i twardych możliwe zadania Objective-C
- Udostępnianie właściwości Objective-C jako właściwości języka C#
- Uwidacznia interfejs API jednoznacznie:

  - Zwiększyć bezpieczeństwo typów
  - Minimalizowanie błędy środowiska wykonawczego
  - Pobierz IDE IntelliSense na typy zwracane
  - Umożliwia dokumentacji podręcznego IDE

- Zachęca eksploracji w IDE interfejsów API:

  - Na przykład zamiast udostępnianie tablicą słabą kontrolą następująco:
    
    ```objc
    NSArray *getViews
    ```
    Pokazuje silnego typu, jak to:
    
    ```csharp
    NSView [] Views { get; set; }
    ```
    
    To umożliwia Visual Studio dla komputerów Mac do automatycznego uzupełniania podczas przeglądania interfejsu API, że wszystkie `System.Array` operacje dostępne dla zwracanej wartości i umożliwia wartości zwracanej brać udziału w składniku LINQ.

- C# typach natywnych:

  - [`NSString` staje się `string`](~/ios/internals/api-design/nsstring.md)
  - Włącz `int` i `uint` parametrów, które powinny zostać wyliczenia do wyliczenia C# i C# wyliczeń o `[Flags]` atrybutów
  - Zamiast niezależny od typu `NSArray` obiektów, ujawnia tablic jako jednoznacznie tablic.
  - Dla zdarzeń i powiadomień zapewniają użytkownikom wybór między:

    - Domyślnie wersję silnie typizowane
    - Słabą kontrolą wersji dla przypadków użycia zaawansowane

- Wzorzec delegata obsługi języka Objective-C:

    - System zdarzeń w języku C#
    - Uwidacznia obiekty delegowane C# (wyrażeń lambda, metody anonimowe i `System.Delegate`) do interfejsów API języka Objective-C jako bloków

### <a name="assemblies"></a>Zestawy

Xamarin.iOS obejmuje kilka zestawów, które stanowią *profilu Xamarin.iOS*. [Zestawy](~/cross-platform/internals/available-assemblies.md) strona zawiera więcej informacji.

### <a name="major-namespaces"></a>Przestrzenie nazw głównych 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) przestrzeni nazw umożliwia deweloperom zestawiania względem między C# i Objective-C.
Jest to nowe powiązanie, przeznaczonego dla systemu iOS, na podstawie doświadczeń z Cocoa # i Gtk #.

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

[Foundation](https://developer.xamarin.com/api/namespace/Foundation/) przestrzeń nazw zawiera typy podstawowe dane zaprojektowany na potrzeby współdziałania z struktury Foundation Objective-C, który jest częścią systemu iOS i jest podstawą obiektowej programowania w języku Objective-c.

Xamarin.iOS odzwierciedla w języku C# hierarchia klas z Objective-C. Na przykład klasa podstawowa Objective-C [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) można używać w języku C# za pomocą [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).

Ta przestrzeń nazw zapewnia powiązania dla typów podstawowych Foundation Objective-C, jednak w niektórych przypadkach firma Microsoft zamapowaniu typy bazowe do typów .NET. Na przykład:

- Zamiast zajmowanie [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) i [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), środowisko uruchomieniowe udostępnia je jako C# [ciąg](https://developer.xamarin.com/api/type/System.String/)s i silnie typizowaną [tablicy](https://developer.xamarin.com/api/type/System.Array/)s w całym Interfejs API.

- Różne pomocnika interfejsów API są widoczne w tym miejscu umożliwiają deweloperom do powiązania innej interfejsów API języka Objective-C, iOS, innych interfejsów API i interfejsów API, które aktualnie nie są powiązane przez platformy Xamarin.iOS.

Więcej szczegółów na powiązanie interfejsów API, zobacz [Xamarin.iOS powiązania Generator](~/cross-platform/macios/binding/binding-types-reference.md) sekcji.


##### <a name="nsobject"></a>NSObject

[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) typu stanowi podstawę dla wszystkich powiązań Objective-C. Typy Xamarin.iOS duplikatów dwóch klas typów z interfejsów API CocoaTouch z systemem iOS: typy C (zwykle określonych jako typy CoreFoundation) i typy Objective-C (te wszystkie pochodzi od klasy NSObject).

Dla każdego typu, która odzwierciedla typem niezarządzanym jest możliwość uzyskania obiekt natywny za pomocą [obsługi](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) właściwości.

Gdy Mono zapewni wyrzucanie elementów bezużytecznych dla wszystkich obiektów, `Foundation.NSObject` implementuje [System.IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/) interfejsu. Oznacza to, można jawnego zwolnienia zasobów żadnych danego NSObject bez potrzeby czekania na moduł zbierający elementy bezużyteczne w spotkaniu. Jest to ważne w przypadku, gdy używana jest mocno NSObjects, na przykład UIImages, które może przechowywać wskaźników do dużych bloków danych.

Jeśli z danym typem musi wykonać finalizacja deterministyczna, Zastąp [NSObject.Dispose(bool) — Metoda](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose) parametru do usunięcia jest "bool disposing", i jeśli ustawić go TRUE oznacza, że metodę Dispose jest wywoływana, ponieważ użytkownik jawnie wywoływane metody Dispose () w obiekcie. Jeśli wartość to false, oznacza to, że metodę Dispose (bool disposing) jest wywoływana z finalizator w wątku finalizatora. []()


##### <a name="categories"></a>Kategorie

Uruchamianie z Xamarin.iOS 8.10 możliwe jest utworzenie kategorii Objective-C w języku C#.

Jest to realizowane przy użyciu `Category` atrybut określający typ do rozszerzenia jako argument atrybutu. Poniższy przykład będzie na przykład rozszerzyć NSString.

    [Category (typeof (NSString))]

Każda metoda kategorii jest przy użyciu mechanizmu normalne eksportowania metody przy użyciu języka Objective-C `Export` atrybutu:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Wszystkie metody zarządzanych rozszerzeń musi być statyczna, ale można utworzyć metody wystąpienia Objective-C przy użyciu standardowej składni metody rozszerzenia w języku C#:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

i pierwszy argument dla metody rozszerzenia będą wystąpienia, na którym została wywołana metoda.

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

W tym przykładzie spowoduje dodanie metody wystąpienia toUpper natywnej do klasy NSString, która może być wywoływany z Objective-C.

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

Jeden scenariusz, gdzie jest to przydatne jest dodanie metody do całego zestawu klas w baza kodu, na przykład może to być wszystkie `UIViewController` wystąpień zgłosić, że można obracać:

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

PreserveAttribute jest niestandardowy atrybut, który informuje mtouch — Narzędzie wdrażania platformy Xamarin.iOS — w celu zachowania typu lub elementu członkowskiego typu, w fazie przetworzenia aplikacji zmniejszyć jego rozmiar.

Każdy członek, który nie jest połączony statycznie przez aplikację jest warunkiem usunięte. W związku z tym ten atrybut służy do oznaczania elementów członkowskich, które nie są wywoływane statycznie, ale nadal wymagane przez aplikację.

Na przykład można utworzyć wystąpienia dynamicznie typy, można zachować domyślnego konstruktora dla typów. Jeśli używasz serializacji XML, można zachować właściwości typów.

Ten atrybut można stosować każdy element członkowski typu lub samego typu. Jeśli chcesz zachować całą typu za pomocą składni [zachować (AllMembers = true)] typu.

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>UIKit

[UIKit](https://developer.xamarin.com/api/namespace/UIKit/) przestrzeń nazw zawiera mapowanie jeden do jednego do wszystkich składników interfejsu użytkownika, które tworzą CocoaTouch w postaci klas C#. Interfejs API został zmodyfikowany w celu postępuj zgodnie z Konwencją w języku C#.

C# delegatów są dostępne dla typowych operacji. Zobacz [delegatów](#Delegates) sekcji, aby uzyskać więcej informacji.

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

Dla OpenGLES, możemy dystrybucji [zmodyfikowana wersja](https://developer.xamarin.com/api/namespace/OpenTK/) z [OpenTK](http://www.opentk.com/) interfejsu API, zorientowany obiektowo powiązania OpenGL, który został zmodyfikowany w celu użycia CoreGraphics typy i struktury danych, a także tylko udostępnianie Funkcje dostępne w systemie iOS.

Funkcje OpenGLES 1.1 jest dostępna za pośrednictwem typu ES11.GL udokumentowane [tutaj](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) typu.

Funkcja OpenGLES 2.0 jest dostępna za pośrednictwem typu ES20.GL, udokumentowane [tutaj](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) typu.

Funkcja OpenGLES 3.0 jest dostępna za pośrednictwem typu ES30.GL, udokumentowane [tutaj](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) typu.


### <a name="binding-design"></a>Powiązania projektu

Xamarin.iOS nie jest jedynie powiązanie podstawowej platformy Objective-C. Rozszerza system typów .NET i wysyłania system, aby lepiej blend C# i Objective-C.

Tak samo, jak P/Invoke jest przydatne narzędzie do wywołania natywnych bibliotek w systemach Windows i Linux, lub jako IJW pomocy technicznej może służyć do międzyoperacyjności z modelem COM w systemie Windows, Xamarin.iOS rozszerza środowiska uruchomieniowego do obsługi powiązanie C# obiekty do obiektów języka Objective-C.

Omówienie w następnych sekcjach kilka nie jest niezbędna dla użytkowników, którzy są tworzenie aplikacji platformy Xamarin.iOS, ale pomaga deweloperom Dowiedz się, jak czynności są wykonywane i pomoże im podczas tworzenia bardziej złożonych aplikacji.



#### <a name="types"></a>Types

Którym dokonane znaczeniu, C# typy są widoczne zamiast niskiego poziomu typów Foundation, do universe C#.  Oznacza to, że [interfejs API korzysta z typu "string" C# zamiast NSString](~/ios/internals/api-design/nsstring.md) i używa zamiast udostępnianie NSArray jednoznacznie tablice C#.

Ogólnie rzecz biorąc, w projekcie platformy Xamarin.iOS i Xamarin.Mac, podstawowa `NSArray` obiektu nie jest widoczne. Zamiast tego środowiska uruchomieniowego powoduje automatyczną konwersję `NSArray`s, aby jednoznacznie tablice niektórych `NSObject` klasy. Tak Xamarin.iOS nie ujawnia metody słabą kontrolą jak GetViews do zwrócenia NSArray:

```csharp
NSArray GetViews ();
```

Zamiast tego powiązania udostępnia silnie typizowaną wartość zwracaną, jak to:

```csharp
UIView [] GetViews ();
```

Istnieje kilka metod w `NSArray`, dla sytuacjach wyjątkowych, których można użyć `NSArray` bezpośrednio, ale ich użycie nie jest zalecane w powiązaniu interfejsu API.

Ponadto w **klasycznego interfejsu API** zamiast udostępnianie `CGRect`, `CGPoint` i `CGSize` z interfejsu API CoreGraphics, możemy zastąpić z `System.Drawing` implementacje `RectangleF`, `PointF`i `SizeF` zgodnie z ich może pomóc deweloperom zachować istniejący kod OpenGL, która używa OpenTK. Korzystając z nowego 64-bitowych **Unified API**, należy użyć interfejsu API CoreGraphics.

<a name="Inheritance" />

#### <a name="inheritance"></a>Dziedziczenie

Projekt interfejsu API platformy Xamarin.iOS umożliwia deweloperom rozszerzyć natywnych typów języka Objective-C w taki sam sposób, że może rozszerzyć się C# typu przy użyciu słowa kluczowego "override" w klasie pochodnej, a także tworzenie łańcuchów do podstawowej implementacji przy użyciu "base" C# — słowo kluczowe.

Ten projekt umożliwia deweloperom uniknięcie korzystania z języka Objective-C selektorów jako część procesu tworzenia, ponieważ całego systemu w języku Objective C jest już opakowana wewnątrz bibliotek platformy Xamarin.iOS.


#### <a name="types-and-interface-builder"></a>Typy i konstruktora interfejsu

Podczas tworzenia klasy .NET, które są wystąpienia utworzone przez interfejs konstruktora typów, musisz podać konstruktora, który przyjmuje jeden `IntPtr` parametru.
Jest to wymagane do powiązania wystąpienia obiektu zarządzanego z niezarządzanego obiektu.
Kod składa się z jednym wierszu, następująco:

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

<a name="Delegates" />


#### <a name="delegates"></a>Delegaty

Objective-C i C# ma inną funkcję do delegata słowo w każdym z tych języków.

W środowisku Objective-C, a w dokumentacji, który można znaleźć online o CocoaTouch delegat jest zwykle wystąpienia klasy, który będzie odpowiadał na zestaw metod. Jest to bardzo podobnie do języka C# interfejs, z tą różnicą, że nie metody zawsze są obowiązkowe.

Te obiekty delegowane odgrywa ważną rolę w UIKit i innych interfejsów API CocoaTouch. Są one używane do wykonywania różnych zadań:

-  Aby zapewnić powiadomienia w kodzie (podobny do dostarczania zdarzeń w języku C# lub Gtk +).
-  Aby zaimplementować modeli formanty wizualizacji danych.
-  Dysk zachowanie formantu.


Programowania wzorca zaprojektowano tak, aby zminimalizować Tworzenie klasy pochodne, aby zmienić zachowanie dla formantu. To rozwiązanie jest podobny do innych narzędzi z graficznym interfejsem użytkownika wykonano całościowo duchu: Gtk na sygnały, Qt miejsc, Winforms zdarzenia, zdarzenia WPF/Silverlight i tak dalej. Aby uniknąć konieczności setki interfejsów (po jednym dla każdej akcji) lub wymagających deweloperom wdrażanie zbyt wiele metod, które nie ma potrzeby, Objective-C obsługuje definicjami metod opcjonalne. To jest inny niż C# interfejsów, które wymagają wszystkie metody implementowane.

W klasach Objective-C, zobaczysz, że klasy, które używają tego wzorca programowania Ujawnij właściwość, zwykle nazywane `delegate`, który jest wymagany do zaimplementowania obowiązkowe części interfejsu i części zero lub więcej z opcjonalnym.

W Xamarin.iOS trzy mechanizmy wykluczają się wzajemnie, aby powiązać te obiekty delegowane są oferowane:

1.  [Za pomocą zdarzeń](#Via_Events).
2.  [Jednoznacznie za pośrednictwem `Delegate` właściwości](#StrongDelegate)
3.  [Typowaniem za pośrednictwem `WeakDelegate` właściwości](#WeakDelegate)

Rozważmy na przykład [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) klasy. Wywołuje się to do [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) wystąpienia, która jest przypisana do [delegować](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) właściwości.

<a name="Via_Events" />

##### <a name="via-events"></a>Za pomocą zdarzeń

Dla wielu typów Xamarin.iOS automatycznie utworzy odpowiedni delegata, który przekaże `UIWebViewDelegate` wywołania na zdarzenia C#. Dla `UIWebView`:

-  [WebViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) metody jest mapowany na [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/) zdarzeń.
-  [WebViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) metody jest mapowany na [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/) zdarzeń.
-  [WebView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) metody jest mapowany na [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/) zdarzeń.

Na przykład prosty program rejestruje czas rozpoczęcia i zakończenia, gdy wyświetlić ładowania sieci web:

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>Za pomocą właściwości

Zdarzenia są przydatne może być więcej niż jeden subskrybent zdarzenia. Ponadto zdarzenia są ograniczone do przypadków, gdy nie ma żadnej wartości zwracanych z kodu.

W przypadkach, gdy kod powinien zwrócić wartość postanowiliśmy zamiast tego właściwości. Oznacza to, że w danym momencie w obiekcie można ustawić tylko jedną metodę.

Na przykład mechanizm ten umożliwia odrzucić klawiatury na ekranie na obsługę programu `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

`UITextField`w `ShouldReturn` właściwości w tym przypadku przyjmuje jako argumentu delegata, który zwraca wartości logicznej i określa, czy TextField zrobić coś o naciśnięcie przycisku powrotu. W naszym metoda zostanie zwrócona *true* do obiektu wywołującego, ale możemy również usunąć klawiatury z ekranu (dzieje się tak podczas wywołania elementu textfield `ResignFirstResponder`).

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>Silnie Typizowane za pomocą właściwości delegata

Jeśli użytkownik nie chce używać zdarzeń, możesz podać własne [UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/) podklasy i przypisz go do [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/) właściwości. Po przypisaniu UIWebView.Delegate, mechanizmu wysyłania zdarzeń UIWebView przestaną działać, a metody UIWebViewDelegate zostanie wywołany, gdy występują odpowiednie zdarzenia.

Na przykład tego typu prostego rejestruje czas potrzebny na załadowanie widoku sieci web:

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

Powyższe jest używana w kodzie w następujący sposób:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

Powyższe utworzy UIWebViewer i zleca go do wysyłania komunikatów do wystąpienia powiadomienie, klasie utworzyliśmy odpowiada na komunikaty.

Ten wzorzec jest również służące do sterowania zachowaniem dla niektórych formantów, na przykład w przypadku UIWebView [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/) właściwość umożliwia `UIWebView` wystąpienie do sterowania czy `UIWebView` załaduje Strona, czy nie.

Wzorzec służy także do dostarczania danych na żądanie dla kilku formantów. Na przykład [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/) formant jest formantem zaawansowanych renderowanie tabeli — i zarówno wyglądu, jak i zawartość są regulowane przez wystąpienie [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>Typowaniem za pomocą właściwości WeakDelegate

Oprócz właściwość jednoznacznie jest także słabe typu delegata, który umożliwia deweloperowi inaczej powiązać rzeczy, w razie potrzeby.
Wszędzie silnie typizowaną `Delegate` właściwości jest widoczna w Xamarin.iOS przez powiązanie, odpowiadający jej `WeakDelegate` właściwość również jest widoczne.

Korzystając z `WeakDelegate`, jest odpowiedzialny za prawidłowo dekoracji przy użyciu klasy [wyeksportować](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) atrybutu, aby określić selektor. Na przykład:

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

Należy pamiętać, że raz `WeakDelegate` przypisany właściwości `Delegate` właściwość nie będzie używana. Ponadto w przypadku zastosowania metody w dziedziczonej klasie podstawowej, którego ma zostać [eksportu], użytkownik musi być publiczną metodę.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Mapowanie wzorca delegata Objective-C-C&#35;

Po wyświetleniu przykłady Objective-C, które wyglądają następująco:

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

Powoduje to, że języka Aby utworzyć i utworzenia wystąpienia klasy "SomethingDelegate" i przypisać wartość właściwości delegata w zmiennej foo. Ten mechanizm jest obsługiwana przez platformy Xamarin.iOS i C# składnia to:

```csharp
foo.Delegate = new SomethingDelegate ();
```

W Xamarin.iOS zostały podane jednoznacznie klas, które mapują Objective-C delegować klasy. Z nich korzystać, użytkownik zostanie tworzenie podklas i zastępowanie metody zdefiniowane przez implementację dla platformy Xamarin.iOS. Aby uzyskać więcej informacji dotyczących sposobu ich działania zobacz sekcję "modele" poniżej.


##### <a name="mapping-delegates-to-c35"></a>Mapowanie delegatów C&#35;

UIKit ogólnie używa języka Objective-C delegatów w dwóch formach.

Pierwszy formularz udostępnia interfejs do składnika modelu. Na przykład jako mechanizm dostarczania danych na żądanie dla widoku, takie jak funkcji magazynu danych w widoku listy.  W takich przypadkach należy zawsze utworzyć wystąpienia klasy prawidłowego i przypisać zmiennej.

W poniższym przykładzie, firma Microsoft udostępnia `UIPickerView` z implementacją dla modelu, który używa ciągów:

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

Drugi formularz ma na celu dostarczenie powiadomienia o zdarzeniach. W takich przypadkach, mimo że nadal uwidaczniamy interfejsu API w formularzu opisanych powyżej, firma Microsoft udostępnia również C# zdarzenia, które powinny być łatwiejsze do użycia dla operacji szybki i zintegrowane z anonimowego delegatów i wyrażeń lambda w języku C#.

Na przykład można subskrybować `UIAccelerometer` zdarzenia:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

Dostępne są dwie opcje, których one sensu, ale dla programisty należy wybrać jednego lub drugiego. Jeśli musisz utworzyć własne wystąpienie silnie typizowanego obiektu odpowiadającego w trybie lub obiekt delegowany i przypisz je zdarzenia C# nie będzie działać. Użycie zdarzenia C#, nigdy nie można wywołać metody w klasie obiektu odpowiadającego w trybie lub obiekt delegowany.

Poprzedni przykład, że używane `UIWebView` można pisać przy użyciu wyrażeń lambda C# 3.0 następująco:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>Odpowiadanie na zdarzenia

W kodzie języka Objective-C czasami obsługi zdarzeń dla wielu formantów i dostawców informacji wielu formantów będzie obsługiwana w tej samej klasy. Jest to możliwe, ponieważ klasy odpowiada na komunikaty i jak długo klasy odpowiada na komunikaty, można połączyć ze sobą obiektów.

Zgodnie z wcześniej Xamarin.iOS obsługuje zarówno C# oparty na zdarzeniach modelu programowania i wzorzec delegata Objective-C, umożliwiającego utworzenie nowej klasy który implementuje delegata i zastępuje żądanej metody.

Istnieje również możliwość obsługi wzorzec Objective-C gdzie obiektów odpowiadających w wielu różnych operacjach znajdują się w tym samym wystąpieniu klasy. W tym celu należy jednak konieczne będzie korzystanie z funkcji powiązania Xamarin.iOS niskiego poziomu.

Na przykład, jeśli potrzebujesz klasy odpowiedzieć na obu `UITextFieldDelegate.textFieldShouldClear`: komunikat i `UIWebViewDelegate.webViewDidStartLoad`: w tym samym wystąpieniu klasy, należy użyć deklaracji atrybutu [eksportu]:

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

Nazwy C# dla metod nie są ważne; wszystkie, które ma znaczenie są ciągi przekazywane z atrybutem [eksportu].

Korzystając z tym stylem programowania, upewnij się, że parametry C# zgodne rzeczywiste typy, które przechodzą przez aparat środowiska wykonawczego.

<a name="Models" />

#### <a name="models"></a>Modele

W urządzenia magazynu UIKit lub obiektów, które są implementowane za pomocą klasy pomocy te zwykle są określane w kodzie języka Objective-C pełnomocnikiem i są implementowane jako protokołów.

Objective-C protokoły są podobne do interfejsów, ale obsługują opcjonalny metody — oznacza to, że nie wszystkie metody muszą zostać zaimplementowane dla protokołu do pracy.

Istnieją dwa sposoby wdrażania modelu. Można ręcznie zaimplementować lub użyj istniejącej definicji jednoznacznie.


Podczas próby zaimplementować klasę, która nie została powiązana przez Xamarin.iOS niezbędne jest mechanizm ręcznego. Jest bardzo łatwo zrobić:

-  Flaga klasy dla rejestracji w czasie wykonywania
-  Zastosuj atrybut [eksportu] o nazwie rzeczywiste selektora dla każdej metody, który chcesz zastąpić
-  Utworzenie wystąpienia klasy i przekaż go.

Na przykład następujące wdrożenie tylko jednej z metod opcjonalne w definicji protokołu UIApplicationDelegate:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Nazwa selektora języka Objective-C ("applicationDidFinishLaunching:") jest zadeklarowana z atrybutem eksportu, a klasa jest zarejestrowana na `[Register]` atrybutu.

Xamarin.iOS udostępnia silnie typizowaną deklaracje, gotowe do użycia, które nie wymagają ręcznego powiązania. Aby zapewnić obsługę tego modelu programowania, środowisko uruchomieniowe platformy Xamarin.iOS obsługuje atrybutu [Model] w deklaracji klasy. Informuje, że jest jawnie implementowana w czasie wykonywania, który go powinien nie okablować wszystkich metod w klasie, chyba że są metody.

Oznacza to, że w UIKit, klasy, które reprezentują protokołu za pomocą metod opcjonalne są zapisany jako:

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

Jeśli chcesz wdrożyć modelu, który tylko implementuje niektórych metod, wszystko, co należy zrobić jest Przesłaniaj metody interesuje i Ignoruj innych metod. Środowisko uruchomieniowe będzie tylko Podłączanie metod zastąpiony, nie oryginalnej metody World Objective-C.

Jest to równoważne z powyższego przykładu ręczne:

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Zalety są czy nie istnieje potrzeba dalszego w plikach nagłówka Objective-C, aby znaleźć selektor, typy argumentów lub mapowania na język C# i uzyskanie intellisense w programie Visual Studio dla komputerów Mac, wraz z silnymi typami


#### <a name="xib-outlets-and-c35"></a>Gniazda XIB i C&#35;

> [!IMPORTANT]
> W tej sekcji opisano integracji IDE z gniazda podczas korzystania z plików XIB. Jeśli przy użyciu narzędzia Projektant Xamarin dla systemu iOS, to wszystkie zastępuje, wprowadzając nazwę w polu **tożsamości > nazwa** w sekcji właściwości IDE, jak pokazano poniżej:
>
> [![](images/designeroutlet.png "Wprowadzanie nazwy elementu w Projektancie systemu iOS")](images/designeroutlet.png#lightbox)
>
>Aby uzyskać więcej informacji w systemie iOS projektanta, zapoznaj się z tematem [wprowadzenie do projektanta dla systemu iOS](~/ios/user-interface/designer/introduction.md#how-it-works) dokumentu.

To niskiego poziomu opisano sposób integrowania gniazda w języku C# i jest dostępne dla użytkowników zaawansowanych platformy Xamarin.iOS. Jeśli przy użyciu programu Visual Studio for Mac mapowanie odbywają się automatycznie w tle przy użyciu wygenerowana kodu w locie.

Podczas projektowania interfejsu użytkownika z interfejsu konstruktora tylko projektowania wyglądu aplikacji i będzie nawiązywać połączenia niektóre domyślne. Jeśli chcesz programowo pobieranie informacji, zmienić zachowanie formantu w czasie wykonywania lub zmienić formantu w czasie wykonywania, należy powiązać niektóre formanty z kodu zarządzanego.

Można to zrobić w kilku krokach:

1.  Dodaj **deklaracji gniazda** do Twojej **właścicielem pliku**.
1.  Połącz formantu do **właścicielem pliku**.
1.  Przechowywanie interfejsu użytkownika oraz połączeń do pliku XIB/NIB.
1.  Załaduj plik NIB w czasie wykonywania.
1.  Dostęp do zmiennej gniazda.


Kroki (1) do (3) znajdują się w dokumentacji firmy Apple do tworzenia interfejsów z konstruktora interfejsu.

Podczas korzystania z platformy Xamarin.iOS, aplikacji należy utworzyć klasę pochodną UIViewController. Wdrażana jest ona następująco:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Następnie można załadować ViewController użytkownika z pliku NIB, możesz to zrobić:

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Spowoduje to załadowanie interfejsu użytkownika z NIB. Teraz Aby uzyskać dostęp do gniazda, należy poinformować środowiska uruchomieniowego, która ma dostęp do nich. Aby to zrobić, `UIViewController` podklasy musi zadeklarować właściwości i dodawania do nich adnotacji z atrybutem [Connect]. Jak to:

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

Implementacja właściwości jest ten, który faktycznie pobiera i przechowuje wartość rzeczywista typu macierzystego.

Nie trzeba martwić się o to przy użyciu programu Visual Studio for Mac i InterfaceBuilder. Visual Studio for Mac automatycznie odzwierciedla wszystkie zadeklarowane gniazda kodu z częściowa klasy, która ma być kompilowana w ramach projektu.

#### <a name="selectors"></a>Selektorów.

Kluczową ideą programowania w języku Objective C jest selektorów. Będzie często napotkać interfejsów API, które trzeba przekazać selektora lub oczekuje swój kod, aby odpowiedzieć na selektora.

Tworzenie nowego selektorów w języku C# jest bardzo proste — wystarczy utworzyć nowe wystąpienie klasy `ObjCRuntime.Selector` klasy i użyj wyniku w dowolnym miejscu w interfejsie API, która go wymaga. Na przykład:

```csharp
var selector_add = new Selector ("add:plus:");
```

Dla języka C# — metoda odpowiedź na wywołania selektora, musi on dziedziczyć z `NSObject` metody C# i typu musi być dekorowane za przy użyciu nazwy selektora `[Export]` atrybutu. Na przykład:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Należy pamiętać, że selektora nazwy **musi** są takie same, w tym wszystkie pośrednie i końcowe dwukropki (":"), jeśli jest obecny.

#### <a name="nsobject-constructors"></a>Konstruktory NSObject

Większość klas w Xamarin.iOS, który pochodzi z `NSObject` uwidoczni konstruktorów określone funkcje obiektu, ale udostępniają również różne konstruktory, które nie są od razu widoczne.

Konstruktory są używane w następujący sposób:

```csharp
public Foo (IntPtr handle)
```

Ten konstruktor jest używany do utworzenia wystąpienia klasy, gdy wymaga środowiska uruchomieniowego do mapowania klasy klasa niezarządzana. Dzieje się tak podczas ładowania pliku XIB/NIB.  W tym momencie środowiska uruchomieniowego języka Objective-C zostanie utworzony obiekt w świecie niezarządzane, a ten konstruktor zostanie wywołana w celu zainicjowania zarządzanych po stronie.

Zazwyczaj wszystko, co należy zrobić to wywołać konstruktora podstawowego z parametrem dojścia i w treści, czy inicjowanie niezbędne.

```csharp
public Foo ()
```

Jest to domyślny konstruktor dla klasy i w Xamarin.iOS podana klasy, to między inicjuje klasy Foundation.NSObject i wszystkie klasy, a na końcu, powiązany to Objective-C `init` metody w klasie.

```csharp
public Foo (NSObjectFlag x)
```

Ten konstruktor jest używany do inicjowania wystąpienia, ale uniemożliw kod wywołanie metody "init" Objective-C na końcu. Jest zazwyczaj używana, gdy zostało już zarejestrowane dla inicjowania (Jeśli używasz `[Export]` w Konstruktorze sieci) lub gdy jeszcze Twojej inicjowania za pośrednictwem innego średniej.

```csharp
public Foo (NSCoder coder)
```

Ten konstruktor jest podana w przypadkach, gdy obiekt jest inicjowany z wystąpienia NSCoding. Aby uzyskać więcej informacji, zobacz firmy Apple [archiwa i przewodnik programowania w języku serializacji.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>Wyjątki

Projekt interfejsu API platformy Xamarin.iOS nie zgłaszaj wyjątków języka Objective-C jako wyjątki C#. Projekt wymusza czy pamięci nie można wysłać do world Objective-C w pierwszej kolejności i że wszelkie wyjątki, które muszą być tworzone są produkowane przez powiązanie się przed kiedykolwiek nieprawidłowe dane przekazane do world Objective-C.

#### <a name="notifications"></a>Powiadomienia

W systemie iOS i OS X deweloperzy mogą subskrybować powiadomień, które są emitowane przez podstawowej platformy. Jest to zrobić za pomocą `NSNotificationCenter.DefaultCenter.AddObserver` metody. `AddObserver` Metoda przyjmuje dwa parametry jedna jest powiadomienie, które chcesz subskrybować; druga to metoda do wywołania, gdy zostanie zgłoszony powiadomienia.

Xamarin.iOS i Xamarin.Mac kluczy dla różnych powiadomień znajdują się w klasie, która wyzwala powiadomień. Na przykład powiadomienia zgłoszone przez `UIMenuController` są obsługiwane jako `static NSString` właściwości `UIMenuController` klasy, które kończą się o nazwie "Powiadomienia".

### <a name="memory-management"></a>Zarządzanie pamięcią

Xamarin.iOS ma moduł zbierający elementy bezużyteczne, który zajmie się zwolnienie zasobów dla Ciebie, gdy są one już używane. Oprócz modułu zbierającego elementy bezużyteczne, wszystkie obiekty który pochodzi od `NSObject` zaimplementować `System.IDisposable` interfejsu.

#### <a name="nsobject-and-idisposable"></a>NSObject i IDisposable

Udostępnianie `IDisposable` interfejsu to wygodny sposób udzielania pomocy deweloperów podczas zwalniania obiektów, które mogą Hermetyzowanie dużych bloków pamięci (na przykład `UIImage` może wyglądać podobnie nieszkodliwie wskaźnika, ale może wskazywać na obraz 2 megabajtów ) i inne ważne i ograniczone zasoby (na przykład wideo buforu dekodowania).

NSObject implementuje interfejs IDisposable, a także [wzorzec .NET Dispose](http://msdn.microsoft.com/en-us/library/fs2xkftw.aspx). Dzięki temu deweloperzy tego podklasy NSObject zastąpienie zachowania Dispose i wersji własnych zasobów na żądanie. Rozważmy na przykład tego kontrolera widoku, który utrzymuje wokół grupy obrazów:

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

Po usunięciu obiektu zarządzanego nie jest już przydatne. Nadal masz odwołania do obiektów, ale obiekt na wszystkich intents i purposes jest nieprawidłowy w tym momencie. Niektóre interfejsów API architektury .NET zapewnia to przez zgłaszanie objectdisposedexception — Jeśli spróbujesz uzyskać dostępu do żadnych metod na zlikwidowanym obiekcie, na przykład:

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

Nawet jeśli nadal mają dostęp do zmiennej "obrazu", że to naprawdę nieprawidłowe odwołanie i nie wskazuje na obiekt Objective-C, który jest przechowywany obraz.

Ale usuwania obiektu w języku C# nie oznacza to, że obiekt musi zostać zniszczone. Wszystko, co możesz zrobić to zwolnienia odwołania mająca C# do obiektu. Istnieje możliwość, może być trzymane odwołanie wokół do własnego użytku w środowisku Cocoa. Na przykład, jeśli właściwość UIImageView obrazu do obrazu, a następnie usunąć obrazu, UIImageView podstawowej miały własne odwołanie i zachowa odwołania do tego obiektu, dopóki zostanie zakończone przy jej użyciu.

#### <a name="when-to-call-dispose"></a>Kiedy wywołać metodę Dispose

Powinny wywoływać metodę Dispose, gdy będziesz potrzebować Mono w usuwania obiektu. Przypadek użycia możliwe jest, gdy Mono nie zna NSObject Twojego faktycznie posiada odwołanie do ważnych zasobów pamięci lub puli informacji. W takich przypadkach należy wywołać metodę Dispose, aby natychmiast zwolnienia odwołania do pamięci, zamiast czekać na Mono przeprowadzić cykl zbierania pamięci.

Wewnętrznie, gdy Mono tworzy [NSString odwołuje się z ciągów C#](~/ios/internals/api-design/nsstring.md), zlikwiduje je od razu, aby zmniejszyć ilość pracy związana moduł garbage collector. Mniej obiektów około na wypadek, tym szybciej GC zostanie uruchomiony.

#### <a name="when-to-keep-references-to-objects"></a>Kiedy należy zachować odwołania do obiektów

Jeden efekt uboczny mającej automatyczne zarządzanie pamięcią jest czy GC będzie usuwanie nieużywanych obiektów tak długo, jak są żadnych odwołań do nich. Czasami może to mieć zaskakująco efekty uboczne, na przykład, jeśli Utwórz zmienną lokalną, aby pomieścić kontroler widoku najwyższego poziomu lub okna najwyższego poziomu, a następnie konieczność tych znikają za kopii.

Jeśli użytkownik nie przechowuje odwołania w Twojej static lub zmienne wystąpienia obiektów, Mono happily będzie wywoływać metodę Dispose() na nich, a zostaną one Zwolnij odwołania do obiektu. Ponieważ może to być tylko oczekujących odwołania, środowiska uruchomieniowego języka Objective-C zniszczy obiektu.

## <a name="related-links"></a>Linki pokrewne

- [Powiązywanie pól](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
