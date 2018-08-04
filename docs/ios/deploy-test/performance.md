---
title: Wydajność platformy Xamarin.iOS
description: W tym dokumencie opisano techniki, które mogą służyć do zwiększenia wydajności i użycia pamięci w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/29/2016
ms.openlocfilehash: 40a2acf28819279b2a0d5c1d50c651a79b455465
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514466"
---
# <a name="xamarinios-performance"></a>Wydajność platformy Xamarin.iOS

Niską wydajność aplikacji przedstawia na wiele sposobów. Może sprawić, że aplikacja prawdopodobnie nie odpowiada, mogą powodować wolne przewijanie i może zmniejszyć czas pracy baterii. Jednak Optymalizacja wydajności polega na więcej niż tylko wdrażania skutecznego kodu. Należy również uwzględnić środowisko wydajność aplikacji. Na przykład zapewnienie, że operacje są wykonywane bez blokowania użytkownikowi wykonywanie innych działań może pomóc ulepszyć środowisko użytkownika. 

W tym dokumencie opisano techniki, które mogą służyć do zwiększenia wydajności i użycia pamięci w aplikacji platformy Xamarin.iOS.

> [!NOTE]
> Przed przeczytaniem tego artykułu warto najpierw przeczytać artykuł [wydajności dla wielu Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md), który w tym artykule omówiono określonych technik-platform, aby poprawić wykorzystanie pamięci i wydajność aplikacji utworzonych za pomocą platformy Xamarin.

## <a name="avoid-strong-circular-references"></a>Należy unikać silne. odwołania cykliczne

W niektórych sytuacjach jest możliwe utworzenie cykle silne odwołanie, które może uniemożliwić ich pamięci odzyskana przez moduł odśmiecania pamięci posiadające obiektów. Na przykład, rozważmy przypadek gdzie [ `NSObject` ](https://developer.xamarin.com/api/type/Foundation.NSObject/)-pochodnych podklasy, taki jak klasa, która dziedziczy z [ `UIView` ](https://developer.xamarin.com/api/type/UIKit.UIView/), zostanie dodany do `NSObject`-pochodne kontenera i jest zdecydowanie odwołanie z języka Objective-C, jak pokazano w poniższym przykładzie kodu:

```csharp
class Container : UIView
{
    public void Poke ()
    {
    // Call this method to poke this object
    }
}

class MyView : UIView
{
    Container parent;
    public MyView (Container parent)
    {
        this.parent = parent;
    }

    void PokeParent ()
    {
        parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Kiedy ten kod tworzy `Container` wystąpienie klasy C# obiekt będzie miał silne odwołanie do obiektu języka Objective-C. Podobnie `MyView` wystąpienia także będą miały silne odwołanie do obiektu języka Objective-C.

Ponadto wywołanie `container.AddSubview` spowoduje zwiększenie licznika odwołań w niezarządzanej `MyView` wystąpienia. W takim wypadku środowisko uruchomieniowe platformy Xamarin.iOS tworzy `GCHandle` wystąpienia, aby zachować `MyView` obiektu w zarządzanym kodzie podtrzymywania połączenia, ponieważ nie ma żadnej gwarancji, że dowolne zarządzane obiekty zapewnią odwołanie do niej. Z punktu widzenia kodu zarządzanego `MyView` obiektu będzie można odzyskać po [ `AddSubview` ](https://developer.xamarin.com/api/member/UIKit.UIView.AddSubview/p/UIKit.UIView/) wywołania zostały go nie `GCHandle`.

Niezarządzaną `MyView` obiekt będzie miał `GCHandle` wskazuje obiektu zarządzanego, znane jako *mocne powiązanie*. Obiektu zarządzanego będzie zawierać odwołanie do `Container` wystąpienia. Z kolei `Container` wystąpienie będzie mieć zarządzanych odwołania do `MyView` obiektu.

W sytuacjach, w którym przechowywany obiekt przechowuje link do jego kontenera istnieje kilka opcji poradzić sobie z odwołanie cykliczne:

-  Ręcznie Wejdź cyklu, ustawiając łącze do kontenera do `null`.
-  Ręcznie usuń zawartego w nim obiektu z kontenera.
-  Wywołaj `Dispose` na obiektach.
-  Należy unikać odwołanie cykliczne, utrzymywanie słabe odwołanie do kontenera. Aby uzyskać więcej informacji na temat słabe odwołania.

### <a name="using-weakreferences"></a>Za pomocą WeakReferences

Jednym ze sposobów, aby zapobiec cykl jest do użycia z elementem podrzędnym słabe odwołanie do elementu nadrzędnego. Na przykład powyższy kod może być zapisany jako:

```csharp
class Container : UIView
{
    public void Poke ()
    {
        // Call this method to poke this object
    }
}

class MyView : UIView
{
    WeakReference<Container> weakParent;
    public MyView (Container parent)
    {
        this.weakParent = new WeakReference<Container> (parent);
    }

    void PokeParent ()
    {
        if (weakParent.TryGetTarget (out var parent))
            parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

W tym miejscu zawartego w nim obiektu nie zachowa nadrzędnego podtrzymywania połączenia. Jednak element nadrzędny utrzymuje podrzędne aktywność za pośrednictwem wywołania wykonywane w celu `container.AddSubView`.

Również dzieje się tak w systemie iOS interfejsów API, które Użyj wzorca źródła delegata lub danych, w których klasa elementu równorzędnego zawiera implementację; na przykład podczas ustawiania [ `Delegate` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.Delegate/) właściwości lub [ `DataSource` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.DataSource/) w [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) klasy.

W przypadku klas, które są tworzone wyłącznie w celu implementacji protokołu, na przykład [ `IUITableViewDataSource` ](https://developer.xamarin.com/api/type/MonoTouch.UIKit.IUITableViewDataSource/), co możesz zrobić to zamiast tworzenia podklasę, można po prostu implementować interfejs w klasie i zastąpienia Metoda i przypisz `DataSource` właściwość `this`.

#### <a name="weak-attribute"></a>Słabe atrybutu

[Xamarin.iOS 11.10](https://developer.xamarin.com/releases/ios/xamarin.ios_11/xamarin.ios_11.10/#WeakAttribute) wprowadzone `[Weak]` atrybutu. Podobnie jak `WeakReference <T>`, `[Weak]` może służyć do przerwania [silne. odwołania cykliczne](https://docs.microsoft.com/en-us/xamarin/ios/deploy-test/performance#avoid-strong-circular-references), ale nawet mniejszej ilości kodu.

Należy wziąć pod uwagę następujący kod, który używa `WeakReference <T>`:

```csharp
public class MyFooDelegate : FooDelegate {
    WeakReference<MyViewController> controller;
    public MyFooDelegate (MyViewController ctrl) => controller = new WeakReference<MyViewController> (ctrl);
    public void CallDoSomething ()
    {
        MyViewController ctrl;
        if (controller.TryGetTarget (out ctrl)) {
            ctrl.DoSomething ();
        }
    }
}
```

Równoważny kod z `[Weak]` jest znacznie bardziej zwięzły:

```csharp
public class MyFooDelegate : FooDelegate {
    [Weak] MyViewController controller;
    public MyFooDelegate (MyViewController ctrl) => controller = ctrl;
    public void CallDoSomething () => controller.DoSomething ();
}
```

Oto inny przykład użycia `[Weak]` w kontekście [delegowania](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html) wzorca:

```csharp
public class MyViewController : UIViewController 
{
    WKWebView webView;

    protected MyViewController (IntPtr handle) : base (handle) { }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        webView = new WKWebView (View.Bounds, new WKWebViewConfiguration ());
        webView.UIDelegate = new UIDelegate (this);
        View.AddSubview (webView);
    }
}

public class UIDelegate : WKUIDelegate 
{
    [Weak] MyViewController controller;

    public UIDelegate (MyViewController ctrl) => controller = ctrl;

    public override void RunJavaScriptAlertPanel (WKWebView webView, string message, WKFrameInfo frame, Action completionHandler)
    {
        var msg = $"Hello from: {controller.Title}";
        var alertController = UIAlertController.Create (null, msg, UIAlertControllerStyle.Alert);
        alertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
        controller.PresentViewController (alertController, true, null);
        completionHandler ();
    }
}
```

### <a name="disposing-of-objects-with-strong-references"></a>Usuwanie obiektów z odwołaniami silne

Silne odwołanie istnieje i jest trudny do usunięcia zależności, aby `Dispose` metoda wyczyść wskaźnika nadrzędnej.

Kontenery, można zastąpić `Dispose` metody Usuń zawarte obiekty, jak pokazano w poniższym przykładzie kodu:

```csharp
class MyContainer : UIView
{
    public override void Dispose ()
    {
        // Brute force, remove everything
        foreach (var view in Subviews)
        {
              view.RemoveFromSuperview ();
        }
        base.Dispose ();
    }
}
```

Dla obiektu podrzędnego, który zapewnia silne odwołanie do elementu nadrzędnego, należy wyczyścić odwołanie do elementu nadrzędnego w `Dispose` implementacji:

```csharp
class MyChild : UIView 
{
    MyContainer container;
    public MyChild (MyContainer container)
    {
        this.container = container;
    }
    public override void Dispose ()
    {
        container = null;
    }
}
```

Aby uzyskać więcej informacji na temat zwalniania silne odwołania, zobacz [zwolnienia zasobów interfejsu IDisposable](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).
Dostępna jest również dobrym dyskusji w tym wpisie w blogu: [Xamarin.iOS, moduł odśmiecania pamięci i me](http://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/).

### <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji, zobacz [reguły w celu uniknięcia cyklów zachować](http://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html) na Cocoa z Love [to usterkę w MonoTouch GC](http://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc) na stronie StackOverflow, i [Dlaczego nie MonoTouch GC kill zarządzane obiekty z refcount > 1? ](http://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1) na stronie StackOverflow.

## <a name="optimize-table-views"></a>Optymalizowanie widoki tabel

Użytkownicy oczekują płynne przewijanie i szybkie ładować [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) wystąpień. Jednak przewijanie wydajności mogą występować podczas komórka zawiera widok głęboko zagnieżdżone hierarchie lub komórki zawierają złożonych układów. Istnieją jednak technik, które mogą być używane, aby uniknąć niska `UITableView` wydajności:

- Ponowne używanie komórek. Aby uzyskać więcej informacji, zobacz [ponowne używanie komórek](#reusecells).
- Zmniejsz liczbę widoków podrzędnych.
- Zawartość komórki pamięci podręcznej, które są pobierane z usługi sieci web.
- Jeśli nie są identyczne w pamięci podręcznej wysokości wszystkich wierszy.
- Należy nieprzezroczyste komórce i innych widokach.
- Należy unikać skalowanie obrazów i gradientów.

Zbiorczo te techniki może pomóc zachować [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) wystąpień płynne przewijanie.

### <a name="reuse-cells"></a>Ponowne używanie komórek

Podczas wyświetlania setki wierszy w [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/), byłoby strat pamięci do tworzenia setek [ `UITableViewCell` ](https://developer.xamarin.com/api/type/UIKit.UITableViewCell/) obiekty, gdy tylko niewielka liczba ich są wyświetlane na ekranie jednocześnie. Zamiast tego, tylko komórki, które są widoczne na ekranie mogą zostać załadowane do pamięci, za pomocą **zawartości** do tych ładowany ponownie komórek. Zapobiega to wystąpienia setki dodatkowe obiekty, oszczędzając czas i pamięci.

W związku z tym gdy komórka zniknie z ekranu jej widok można umieścić w kolejce do ponownego wykorzystania, jak pokazano w poniższym przykładzie kodu:

```csharp
class MyTableSource : UITableViewSource
{
    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        // iOS will create a cell automatically if one isn't available in the reuse pool
        var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

        // Perform required cell actions
        return cell;
    }
}
```

Jak użytkownik przewija widok, [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) wywołania `GetCell` należy przesłonić, aby zażądać nowych widoków do wyświetlenia. To zastąpienie następnie wywołuje [ `DequeueReusableCell` ](https://developer.xamarin.com/api/member/UIKit.UITableView.DequeueReusableCell/p/Foundation.NSString/) metody i jeśli komórka jest dostępny do ponownego wykorzystania, zostaną zwrócone.

Aby uzyskać więcej informacji, zobacz [ponownie użyć komórki](~/ios/user-interface/controls/tables/populating-a-table-with-data.md) w [Wypełnianie tabel danymi](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

## <a name="use-opaque-views"></a>Za pomocą nieprzezroczystych widoków

Upewnij się, że wszystkie widoki, które mają bez przezroczystych zdefiniowane ich [ `Opaque` ](https://developer.xamarin.com/api/property/UIKit.UIView.Opaque/) zestawu właściwości. Pozwoli to zagwarantować optymalnie renderowania widoków przez system rysowania. Jest to szczególnie ważne, jeśli widok jest osadzony w [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/), lub jest częścią złożonej animacji. W przeciwnym razie rysowania system będzie złożone widoków z innej zawartości, który znacznie może mieć wpływ na wydajność.

## <a name="avoid-fat-xibs"></a>Należy unikać fat XIb

Mimo że XIb stopniu zostały zastąpione przez scenorysów, istnieją pewne okoliczności gdzie XIb może być stosowany. Gdy XIB jest ładowany do pamięci, całą jego zawartość są ładowane do pamięci, w tym żadnych obrazów. Jeśli XIB zawiera widok, który nie jest od razu jest używana, zostanie on zmarnowane pamięci. W związku z tym korzystając z XIb upewnij się, że istnieje tylko jeden XIB każdy kontroler widoku, a jeśli to możliwe, dzielenie kontrolera widoku Wyświetl hierarchię XIb oddzielne.

## <a name="optimize-image-resources"></a>Optymalizuj zasoby obrazów

Obrazy są najbardziej kosztowne zasoby, których aplikacje, i często są przechwytywane w wysokiej rozdzielczości. W związku z tym podczas wyświetlania obrazu z pakietu aplikacji w [ `UIImageView` ](https://developer.xamarin.com/api/type/UIKit.UIImageView/), upewnij się, że obraz i `UIImageView` są takie same rozmiary. Skalowanie obrazów w czasie wykonywania może być kosztowna operacja, szczególnie jeśli `UIImageView` jest osadzony w [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/).

Aby uzyskać więcej informacji, zobacz [zoptymalizować zasoby obrazów](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) w [wydajności dla wielu Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md) przewodnik.

## <a name="test-on-devices"></a>Testuj na urządzeniach

Rozpocznij wdrażanie i testowanie aplikacji na urządzeniu fizycznym możliwie jak najszybciej. Symulatorów doskonale niezgodne zachowań i ograniczenia dotyczące urządzeń, a więc jest ważne, aby test w scenariuszu rzeczywistych urządzeń tak szybko, jak to możliwe.

W szczególności symulatora w żaden sposób nie symulować pamięci lub Procesora ograniczenia urządzenia fizycznego.

## <a name="synchronize-animations-with-the-display-refresh"></a>Synchronizuj animacji z odświeżaniem wyświetlania

Gry się zwykle ścisłej pętli, które uruchamiają logikę gier i zaktualizować ekran. Typowej ramki kursy z zakresu od trzydzieści do 60 klatek na sekundę. Niektórzy deweloperzy czuć się należy zaktualizować ekranu w formie tyle razy, ile to możliwe, na sekundę, łącząc ich symulacji gier za pomocą aktualizacji do ekranu i może być kuszące czegoś więcej niż 60 klatek na sekundę.

Jednak serwer wyświetlania wykonuje aktualizacje ekranu, górny limit sześćdziesiąt razy na sekundę. W związku z tym podjęcie próby zaktualizowania ekranu szybciej niż to ograniczenie może prowadzić do ekranu przerwanie i przestojów micro. Najlepiej jest struktury kodu, aby ekran aktualizacje są synchronizowane z aktualizacją update wyświetlania. Można to osiągnąć przy użyciu [ `CoreAnimation.CADisplayLink` ](https://developer.xamarin.com/api/type/CoreAnimation.CADisplayLink/) klasy, która jest odpowiednia dla wizualizacji Czasomierz i gry, które jest uruchamiane w sixty klatek na sekundę.

## <a name="avoid-core-animation-transparency"></a>Należy unikać przezroczystości podstawowe funkcje animacji

Unikanie przezroczystość animacji core poprawić wydajność składania mapy bitowej. Ogólnie rzecz biorąc Unikaj przezroczyste warstwy i rozmyty obramowania, jeśli jest to możliwe.

## <a name="avoid-code-generation"></a>Należy unikać generowania kodu

Generowanie kodu dynamicznie przy użyciu `System.Reflection.Emit` lub *środowiska Dynamic Language Runtime* należy unikać ponieważ jądra dla systemu iOS uniemożliwia wykonanie kodu dynamicznych.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano i omówiono techniki pozwalające zwiększyć wydajność aplikacji utworzonych za pomocą platformy Xamarin.iOS. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywanej przez Procesora i ilość pamięci używanej przez aplikację.

## <a name="related-links"></a>Linki pokrewne

- [Wydajności dla wielu Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md)
