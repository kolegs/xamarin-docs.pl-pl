---
title: Wydajności platformy Xamarin.iOS
description: W tym dokumencie opisano techniki, które mogą służyć do poprawienia wydajności i użycia pamięci w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/29/2016
ms.openlocfilehash: afff9d3924c673edc363292efa1a9b7df43a9218
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinios-performance"></a>Wydajności platformy Xamarin.iOS

Niską wydajnością przedstawia na wiele sposobów. Go aplikacja prawdopodobnie nie odpowiada, może spowodować wolne przewijanie i może zmniejszyć czas pracy baterii. Jednak optymalizacji wydajności wymaga więcej niż tylko wdrażania wydajność kodu. Środowisko użytkownika wydajność aplikacji, należy również rozważyć. Na przykład zapewniając wykonanie operacji bez blokowania użytkownika wykonywanie innych działań może pomóc ulepszyć środowisko użytkownika. 

W tym dokumencie opisano techniki, które mogą służyć do poprawienia wydajności i użycia pamięci w aplikacji platformy Xamarin.iOS.

> [!NOTE]
> Przed przeczytaniem tego artykułu warto najpierw przeczytać artykuł [wydajności i Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md), omówiono w nim-platforma określonych technik w celu poprawy użycie pamięci i wydajność aplikacji utworzony za pomocą platformy Xamarin.

## <a name="avoid-strong-circular-references"></a>Unikaj silne odwołania cykliczne

W niektórych sytuacjach jest można tworzyć cykle silne odwołanie, które może uniemożliwić obiekty o ich pamięci odzyskana przez moduł garbage collector. Na przykład rozważmy przypadek gdzie [ `NSObject` ](https://developer.xamarin.com/api/type/Foundation.NSObject/)-pochodnych podklasy, takich jak klasy, która dziedziczy po [ `UIView` ](https://developer.xamarin.com/api/type/UIKit.UIView/), jest dodawany do `NSObject`-pochodnych kontenera i jest silnie Odwołanie do Objective-c, jak pokazano w poniższym przykładzie:

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

Gdy ten kod tworzy `Container` wystąpienia, C# obiekt będzie miał silne odwołanie do obiektu języka Objective-C. Podobnie `MyView` wystąpienia będą także mieć silne odwołanie do obiektu języka Objective-C.

Ponadto wywołanie `container.AddSubview` spowoduje zwiększenie liczby odwołania na niezarządzanych `MyView` wystąpienia. W takim przypadku tworzy środowiska wykonawczego platformy Xamarin.iOS `GCHandle` wystąpienia, aby zachować `MyView` obiektu w kodzie zarządzanym aktywności, ponieważ nie ma żadnej gwarancji, że wszelkie zarządzane obiekty zachowa odwołanie do niej. Z punktu widzenia kodu zarządzanego `MyView` obiektu będzie można odzyskać po [ `AddSubview` ](https://developer.xamarin.com/api/member/UIKit.UIView.AddSubview/p/UIKit.UIView/) wywołania zostały go nie `GCHandle`.

Niezarządzane `MyView` obiekt będzie miał `GCHandle` wskazuje obiekt zarządzany, znany jako *silne łącze*. Zarządzanego obiektu będzie zawierać odwołanie do `Container` wystąpienia. Z kolei `Container` wystąpienie będzie mieć zarządzane odniesienia do `MyView` obiektu.

W sytuacjach, w którym przechowywany obiekt przechowuje link do jego kontenera jest dostępny na wypadek odwołanie cykliczne kilka opcji:

-  Ręcznie przerwać cykl przez ustawienie łącze do kontenera do `null`.
-  Ręcznie usuń przechowywany obiekt z kontenera.
-  Wywołanie `Dispose` obiektów.
-  Unikaj odwołanie cykliczne utrzymywanie słabe odwołanie do kontenera. Aby uzyskać więcej informacji na temat słabe odwołania.

### <a name="using-weakreferences"></a>Przy użyciu WeakReferences

Jest jednym ze sposobów zapobiec cykl do używania z elementem podrzędnym słabe odwołanie do elementu nadrzędnego. Na przykład powyższy kod może być zapisany jako:

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

W tym miejscu zawartego w nim obiektu nie zachowa nadrzędnego aktywności. Jednak nadrzędnego śledzi podrzędne aktywności za pomocą wywołania gotowe do `container.AddSubView`.

Również dzieje się to w systemie iOS interfejsów API, które Użyj wzorca źródła delegata lub danych, w których klasa elementu równorzędnego zawiera implementację; na przykład podczas ustawiania [ `Delegate` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.Delegate/) właściwości lub [ `DataSource` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.DataSource/) w [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) klasy.

W przypadku klasy, które są tworzone wyłącznie w celu wykonania Protokołu, na przykład [ `IUITableViewDataSource` ](https://developer.xamarin.com/api/type/MonoTouch.UIKit.IUITableViewDataSource/), co możesz zrobić to zamiast tworzenia podklasy, może po prostu implementować interfejs klasy i zastąpienia Metoda i przypisz `DataSource` właściwości `this`.

#### <a name="weak-attribute"></a>Słabe atrybutu

[Xamarin.iOS 11.10](https://developer.xamarin.com/releases/ios/xamarin.ios_11/xamarin.ios_11.10/#WeakAttribute) wprowadzone `[Weak]` atrybutu. Podobnie jak `WeakReference <T>`, `[Weak]` może służyć do dzielenia [silne odwołania cykliczne](https://docs.microsoft.com/en-us/xamarin/ios/deploy-test/performance#avoid-strong-circular-references), ale bez jeszcze mniej kodem.

Należy rozważyć następujący kod, który używa `WeakReference <T>`:

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

Odpowiednik kodu za pomocą `[Weak]` jest znacznie bardziej zwięzły:

```csharp
public class MyFooDelegate : FooDelegate {
    [Weak] MyViewController controller;
    public MyFooDelegate (MyViewController ctrl) => controller = ctrl;
    public void CallDoSomething () => controller.DoSomething ();
}
```

Inny przykład użycia `[Weak]` w kontekście [delegowania](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html) wzorca:

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

Silne odwołanie istnieje i jest trudne do usunięcia zależności, aby `Dispose` metody clear wskaźnika nadrzędnej.

Dla kontenerów, Zastąp `Dispose` metodę, aby usunąć zawartych w niej obiekty, jak pokazano w poniższym przykładzie kodu:

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

Dla obiekt podrzędny, który rozdziela silne odwołanie do elementu nadrzędnego, Usuń odwołanie do elementu nadrzędnego w `Dispose` implementacji:

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

Aby uzyskać więcej informacji na temat zwalniania silne odwołań, zobacz [wersji interfejsu IDisposable zasobów](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).
Jest również dobrym omówione w tym wpisie w blogu: [Xamarin.iOS, moduł zbierający elementy bezużyteczne i me](http://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/).

### <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji, zobacz [reguły w celu uniknięcia cykle zachować](http://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html) na Cocoa z Love [jest to błąd w MonoTouch GC](http://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc) w witrynie StackOverflow, i [Dlaczego nie MonoTouch GC kill zarządzane obiekty z licznikiem > 1? ](http://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1) w witrynie StackOverflow.

## <a name="optimize-table-views"></a>Optymalizacja widoków tabel

Użytkownicy oczekują przewijanie płynne i szybkie ładować [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) wystąpień. Jednak przewijanie wydajności mogą występować, gdy komórka zawiera widok głęboko zagnieżdżone hierarchie lub gdy komórka zawiera złożone układów. Istnieją jednak techniki, których można użyć w celu uniknięcia słaby `UITableView` wydajności:

- Ponowne użycie komórek. Aby uzyskać więcej informacji, zobacz [ponowne użycie komórek](#reusecells).
- Zmniejsz liczbę widoków podrzędnych.
- Zawartość komórki pamięci podręcznej, które są pobierane z usługi sieci web.
- Jeśli nie są identyczne w pamięci podręcznej wysokość jakiekolwiek wiersze.
- Należy nieprzezroczyste komórki, a innymi widokami.
- Unikaj skalowanie obrazu i gradienty.

Zbiorczo te techniki może pomóc zapewnić [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) wystąpień sprawnie przewijania.

### <a name="reuse-cells"></a>Ponowne użycie komórek

Podczas wyświetlania setki wierszy w [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/), byłoby odpady pamięci do utworzenia setki [ `UITableViewCell` ](https://developer.xamarin.com/api/type/UIKit.UITableViewCell/) obiektów, gdy tylko niewielką liczbę ich są wyświetlane na ekranie naraz. Zamiast tego, tylko komórki widoczne na ekranie mogą zostać załadowane do pamięci z **zawartości** ładowany w tych ponownie komórek. Zapobiega to tworzeniu wystąpienia elementu setki dodatkowe obiekty, oszczędzając czas i pamięci.

W związku z tym gdy komórki zniknie z ekranu jego widoku można umieścić w kolejce do ponownego użycia, jak pokazano w poniższym przykładzie:

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

Jako użytkownik przewija widok, [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) wywołania `GetCell` należy przesłonić, aby zażądać nowych widoków do wyświetlenia. To zastąpienie następnie wywołania [ `DequeueReusableCell` ](https://developer.xamarin.com/api/member/UIKit.UITableView.DequeueReusableCell/p/Foundation.NSString/) — metoda i jeśli komórka jest dostępny do ponownego użycia zostaną zwrócone.

Aby uzyskać więcej informacji, zobacz [komórki ponowne użycie](~/ios/user-interface/controls/tables/populating-a-table-with-data.md) w [wypełnianie tabeli z danymi](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

## <a name="use-opaque-views"></a>Używanie widoków nieprzezroczyste

Upewnij się, że wszystkie widoki, które mają bez przezroczystości zdefiniowane ich [ `Opaque` ](https://developer.xamarin.com/api/property/UIKit.UIView.Opaque/) zestawu właściwości. Zapewni to optymalnie renderowania widoków przez system rysowania. Jest to szczególnie ważne, gdy widok jest osadzony w [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/), lub wchodzi w skład złożonych animacji. W przeciwnym razie rysowania system będzie złożone widoków z innej zawartości, która może znacznie wpłynąć na wydajność.

## <a name="avoid-fat-xibs"></a>Unikaj fat XIBs

Mimo że XIBs przede wszystkim zostały zastąpione przez scenorys, istnieją pewne okoliczności gdzie XIBs może być stosowany. Gdy XIB jest ładowany do pamięci, całą jego zawartość są ładowane do pamięci, w tym wszystkie obrazy. Jeśli XIB zawiera widok, który nie jest od razu jest używana, jest on niewykorzystana pamięci. W związku z tym korzystając z XIBs upewnij się, że istnieje tylko jeden XIB każdy kontroler widoku, a jeśli to możliwe, podzielić kontroler widoku Wyświetl hierarchię na oddzielnych XIBs.

## <a name="optimize-image-resources"></a>Optymalizacja zasoby obrazów

Obrazy są niektóre z najdroższych zasobów korzystających z aplikacji, a często są przechwytywane w wysokiej rozdzielczości. W związku z tym podczas wyświetlania obrazu z pakietu aplikacji w [ `UIImageView` ](https://developer.xamarin.com/api/type/UIKit.UIImageView/), upewnij się, że obraz i `UIImageView` są identyczne rozmiary. Skalowanie obrazów w czasie wykonywania mogą być kosztowna operacja, zwłaszcza w wypadku `UIImageView` jest osadzony w [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/).

Aby uzyskać więcej informacji, zobacz [optymalizacji zasobów obrazu](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) w [wydajności i Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md) przewodnik.

## <a name="test-on-devices"></a>Test na urządzeniach

Rozpocznij wdrażanie i testowanie aplikacji na urządzeniu fizycznym możliwie jak najszybciej. Symulatorów nie są całkowicie zgodne zachowania i ograniczenia dotyczące urządzeń, i dlatego ważne jest, aby przetestować w scenariuszu urządzenie rzeczywistych tak szybko jak to możliwe.

W szczególności symulatora w żaden sposób nie symulować pamięci lub Procesora ograniczenia urządzenia fizycznego.

## <a name="synchronize-animations-with-the-display-refresh"></a>Synchronizowanie animacji z odświeżania wyświetlania

Gry mają ścisłej pętli do uruchomienia gry logiki i aktualizacje ekranu. Typowy ramki szybkości w zakresie od trzydzieści do 60 klatek na sekundę. Niektórzy deweloperzy uznać, należy zaktualizować ekranu w formie tyle razy, ile to możliwe, na sekundę, łączenie ich gier symulacji z aktualizacjami ekranu i może się wydawać się wykracza poza 60 klatek na sekundę.

Jednak serwer wyświetlania wykonuje aktualizacje ekranu, górny limit sześćdziesiąt razy na sekundę. W związku z tym podjęto próbę zaktualizowania ekranu szybciej niż to ograniczenie może prowadzić do ekranu przerwanie i micro przestojów. Najlepiej kodu struktury tak, aby aktualizacje ekranu są synchronizowane z aktualizacją wyświetlania. Można to osiągnąć przy użyciu [ `CoreAnimation.CADisplayLink` ](https://developer.xamarin.com/api/type/CoreAnimation.CADisplayLink/) klasy, która jest odpowiedni dla wizualizacji Czasomierz i gier, które jest uruchamiane w sixty ramek na sekundę.

## <a name="avoid-core-animation-transparency"></a>Unikaj przezroczystość animacji Core

Unikanie przezroczystość animacji core poprawić wydajność składania mapy bitowej. Ogólnie rzecz biorąc należy unikać przezroczyste warstwy i rozmyty obramowania, jeśli to możliwe.

## <a name="avoid-code-generation"></a>Uniknąć generowania kodu

Generowanie kodu dynamicznie z `System.Reflection.Emit` lub *środowiska Dynamic Language Runtime* należy unikać ponieważ jądra dla systemu iOS uniemożliwia wykonanie kodu dynamicznych.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano i opisem techniki zwiększania wydajności aplikacji skompilowanej za pomocą platformy Xamarin.iOS. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację.

## <a name="related-links"></a>Linki pokrewne

- [Wydajność i Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md)
