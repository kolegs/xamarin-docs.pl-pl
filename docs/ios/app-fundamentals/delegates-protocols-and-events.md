---
title: "Zdarzenia, protokołów i Delegaty"
description: "Ten artykuł przedstawia informacje o technologii klucza iOS używany do odbierania wywołania zwrotne i wypełnianie formantów interfejsu użytkownika z danymi. Technologie te są zdarzenia, protokołów i delegatów. W tym artykule opisano, co każdy z nich jest i jak są używane w języku C#. Pokazuje, jak Xamarin.iOS używa kontrolki z systemem iOS do udostępnienia znanych zdarzenia platformy .NET, jak również, jak Xamarin.iOS zapewnia obsługę pojęcia języka Objective-C, takie jak protokoły i delegatów (Objective-C delegatów nie należy mylić z delegatów C#). Ten artykuł zawiera również przykłady, których pokazano, jak używane protokoły — zarówno jako podstawę dla języka Objective-C delegatów i w scenariuszach niebędącego delegatem."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 69296992c503d536a4160f172022c7ce5578812f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="events-protocols-and-delegates"></a>Zdarzenia, protokołów i Delegaty

_Ten artykuł przedstawia informacje o technologii klucza iOS używany do odbierania wywołania zwrotne i wypełnianie formantów interfejsu użytkownika z danymi. Technologie te są zdarzenia, protokołów i delegatów. W tym artykule opisano, co każdy z nich jest i jak są używane w języku C#. Pokazuje, jak Xamarin.iOS używa kontrolki z systemem iOS do udostępnienia znanych zdarzenia platformy .NET, jak również, jak Xamarin.iOS zapewnia obsługę pojęcia języka Objective-C, takie jak protokoły i delegatów (Objective-C delegatów nie należy mylić z delegatów C#). Ten artykuł zawiera również przykłady, których pokazano, jak używane protokoły — zarówno jako podstawę dla języka Objective-C delegatów i w scenariuszach niebędącego delegatem._

Xamarin.iOS używa kontrolki do udostępnienia zdarzeń dla większości interakcji użytkownika.
Aplikacji platformy Xamarin.iOS pobierać te zdarzenia w podobny sposób jak tradycyjne aplikacje .NET. Na przykład klasa Xamarin.iOS UIButton ma zdarzenia o nazwie TouchUpInside i wykorzystuje to zdarzenie, tak jak gdyby tej klasy i zdarzeń w aplikacji .NET.

Oprócz tego podejścia .NET Xamarin.iOS przedstawia inny model, który może służyć do interakcji z bardziej złożone i powiązanie danych. Tej metody używa co Apple wywołuje delegatów i protokołów. Obiekty delegowane są podobne do delegatów w języku C#, ale zamiast definiowanie i wywołanie metody pojedynczego, delegata w języku Objective C jest całej klasy, który jest zgodny z protokołem. Protokół jest podobny do interfejsu w języku C#, z tą różnicą, że jego metody mogą być opcjonalne. Tak na przykład aby wypełnić UITableView z danymi, należy utworzyć klasa delegata, która implementuje metody zdefiniowane w protokole UITableViewDataSource UITableView za wywoływanie do wypełnienia sam.

W tym artykule dowiesz się o tych tematów, umożliwiając solidną podstawę dla obsługi scenariuszy wywołania zwrotnego w Xamarin.iOS, w tym:

-  **Zdarzenia** — przy użyciu zdarzenia platformy .NET z formantami UIKit.
-  **Protokoły** — co uczenia protokoły są i sposobu ich używania i tworzenia przykład, który dostarcza dane dla adnotacji mapy.
-  **Obiekty delegowane** — uczenie się o delegatów Objective-C rozszerzanie przykład mapy do obsługi interakcji z użytkownikiem, który zawiera adnotację, a następnie uczenia różnica między delegatów silna i słaba i kiedy należy używać każdego z tych.


Aby zilustrować protokołów i delegatów, firma Microsoft będzie kompilacji aplikacji proste mapy, umożliwiający dodawanie adnotacji do mapy, jak pokazano poniżej:

 [![](delegates-protocols-and-events-images/01-map.png "Przykładem aplikacji proste mapy, umożliwiający dodawanie adnotacji do mapy") ](delegates-protocols-and-events-images/01-map.png#lightbox) [ ![ ] (delegates-protocols-and-events-images/04-annotation-with-callout.png "adnotacji przykład dodany do mapy")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Przed czoła tej aplikacji, Rozpocznijmy analizując zdarzenia platformy .NET w obszarze UIKit.

 <a name=".NET_Events_with_UIKit" />


## <a name="net-events-with-uikit"></a>Zdarzenia platformy .NET z UIKit

Xamarin.iOS przedstawia zdarzenia platformy .NET w formantach UIKit. Na przykład UIButton ma zdarzenia TouchUpInside obsługi normalnie .NET, jak pokazano w poniższym kodzie, który korzysta z wyrażenia lambda C#:

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

Można też wdrożyć to za pomocą języka C# 2.0 stylu anonimowe metody podobne do następującego:

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

Poprzedni kod przewodowej się w metodzie ViewDidLoad UIViewContoller. Zmienna aButton odwołuje się do przycisku, który można dodać w systemie iOS Designer lub z kodem. Na poniższej ilustracji przedstawiono ten przycisk, który jest dodawany w systemie iOS projektanta, pobiera z próbki, dołączony w tym artykule:

 [![](delegates-protocols-and-events-images/02-interface-builder-outlet.png "Przycisk dodane w systemie iOS projektanta")](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

Xamarin.iOS obsługuje również łączenie kodu z interakcją występuje z formantem styl akcję docelową. Aby utworzyć akcję docelową dla przycisku Hello, kliknij dwukrotnie tę pozycję w systemie iOS projektanta. Plik CodeBehind UIViewController zostanie wyświetlony i dewelopera poprosimy Cię o wybierz lokalizację, aby wstawić łączącego — metoda:

 [![](delegates-protocols-and-events-images/03-interface-builder-action.png "Plik CodeBehind UIViewControllers")](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

Po wybraniu lokalizacji nowej metody jest utworzony i przewodowej w górę do formantu. W poniższym przykładzie komunikat zostanie wyświetlony w konsoli, po kliknięciu przycisku:

 [![](delegates-protocols-and-events-images/05-interface-builder-action.png "Komunikat zostanie wyświetlony w konsoli, po kliknięciu przycisku")](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

Aby uzyskać więcej informacji o wzorcu akcję docelową dla systemu iOS, zobacz sekcję akcję docelową " [podstawowe umiejętności aplikacji dla systemu iOS](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)" w bibliotece deweloperów systemu iOS firmy Apple.

Aby uzyskać więcej informacji o korzystaniu z platformy Xamarin.iOS iOS projektanta, zobacz " [iOS Omówienie projektanta](~/ios/user-interface/designer/index.md)" dokumentacji.

 <a name="Events" />


## <a name="events"></a>Zdarzenia

Jeśli chcesz przechwytywać zdarzenia z kontrolne, ma zakres opcje: z korzystanie z funkcji delegowanego przy użyciu niskiego poziomu interfejsów API języka Objective C i C# wyrażeń lambda.

W tej części opisano, jak będzie przechwytywania zdarzeń TouchDown na przycisku, w zależności od tego formantu, ile potrzebujesz.

 <a name="C#_Style" />


## <a name="c-style"></a>Styl C#

Przy użyciu składni delegata:

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {    
    Console.WriteLine ("Touched");
};
```

Jeśli chcesz zamiast tego wyrażenia lambda:

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

Jeśli chcesz mieć wielu przycisków umożliwiają udostępnianie tego samego kodu tej procedury obsługi:

```csharp
void handler (object sender, EventArgs args)
{
   if (sender == button1)
      Console.WriteLine ("button1");
   else
      Console.WriteLine ("some other button");
}

button1.TouchDown += handler;
button2.TouchDown += handler;
```

<a name="Monitoring_more_than_one_kind_of_Event" />


## <a name="monitoring-more-than-one-kind-of-event"></a>Monitorowanie więcej niż jednego rodzaju zdarzenia

Zdarzenia C# dla flag UIControlEvent mają mapowanie jeden do jednego do poszczególnych flag. Jeśli chcesz, aby sam fragment kodu obsługi dwóch lub więcej zdarzeń, użyj `UIControl.AddTarget` metody:

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Przy użyciu składni lambda:

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Jeśli musisz używać niskiego poziomu funkcji Objective-C, takich jak Podłączanie do wystąpienia określonego obiektu i wywoływanie określonego selektora:

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

Zwróć uwagę, zaimplementuj metodę wystąpienia w dziedziczonej klasie podstawowej, musi mieć publiczną metodę.

 <a name="Protocols" />


## <a name="protocols"></a>protokoły

Protokół jest to funkcja języka Objective-C, która zawiera listę deklaracje metody. Służy ona cel podobny do interfejsu w języku C#, główną różnicą, że protokół może mieć metody opcjonalne. Opcjonalne metod nie są nazywane, jeśli klasa, która przyjmuje protokół nie implementuje je. Ponadto jednej klasy w języku Objective C można zaimplementować wiele protokołów, tak samo, jak klasa C# można zaimplementować wiele interfejsów.

Przy użyciu protokołów w całym iOS firmy Apple Definiowanie kontraktów dla klas do przyjęcia, podczas optymalizacji dostępnego dla klasy implementującej wywołujący, w związku z tym działających podobnie jak interfejs języka C#. Protokoły są używane zarówno w scenariuszach Niedelegowany (takich jak z `MKAnnotation` następnym przykładzie) oraz z delegatów (przedstawionych w dalszej części tego dokumentu, w sekcji delegatów).

 <a name="Protocols_with_Monotouch" />


### <a name="protocols-with-xamarinios"></a>Protokoły z platformy Xamarin.ios

Spójrzmy na przykład przy użyciu protokołu Objective-C z platformy Xamarin.iOS. W tym przykładzie użyjemy `MKAnnotation` protokołu, który jest częścią programu `MapKit` framework. `MKAnnotation` jest to protokół, który umożliwia dowolny obiekt, który przyjmuje je do udostępniania informacji na temat adnotacji do dodania do mapy. Na przykład obiekt implementacja `MKAnnotation` miejsce adnotacja i tytuł skojarzonych z nim.

W ten sposób `MKAnnotation` protokół służy do zapewnienia potrzebnych danych dołączona adnotacja. Rzeczywista widok dla adnotacji sam składa się z danych obiektu, który przyjmuje `MKAnnotation` protokołu. Na przykład tekst objaśnienia, który jest wyświetlany, gdy użytkownik naciska adnotację (jak pokazano na poniższym zrzucie ekranu) pochodzą z `Title` właściwości klasy, który implementuje ten protokół:

 [![](delegates-protocols-and-events-images/04-annotation-with-callout.png "Przykładowy tekst objaśnienia po naciśnięciu na adnotacji")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Zgodnie z opisem w następnej sekcji nowości protokołów Xamarin.iOS wiąże protokoły klas abstrakcyjnych. Dla `MKAnnotation` protokołu, powiązanej klasy C# o nazwie `MKAnnotation` aby naśladował nazwę protokołu który jest podklasą `NSObject`, klasa podstawowa dla CocoaTouch. Protokół wymaga określonej metody pobierającej i ustawiającej do zaimplementowania współrzędnej; Jednakże tytuł i podtytuł są opcjonalne. W związku z tym w `MKAnnotation` klasy `Coordinate` właściwość jest *abstrakcyjny*, wymaganiem go do zaimplementowania i `Title` i `Subtitle` właściwości są oznaczone *wirtualnego* , dzięki czemu opcjonalne, jak pokazano poniżej:

```csharp
[Register ("MKAnnotation"), Model ]
public abstract class MKAnnotation : NSObject
{
    public abstract CLLocationCoordinate2D Coordinate
    {
        [Export ("coordinate")]
        get;
        [Export ("setCoordinate:")]
        set;
    }

    public virtual string Title
    {
        [Export ("title")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }

    public virtual string Subtitle
    {
        [Export ("subtitle")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }
...
}
```

Każda klasa może zapewnić adnotacji danych po prostu pochodny `MKAnnotation`, tak długo, jak co najmniej `Coordinate` właściwości jest zaimplementowana. Na przykład poniżej przedstawiono klasy próbki, która przyjmuje Współrzędna w Konstruktorze i zwraca ciąg tytułu:

```csharp
/// <summary>
/// Annotation class that subclasses MKAnnotation abstract class
/// MKAnnotation is bound by Xamarin.iOS to the MKAnnotation protocol
/// </summary>
public class SampleMapAnnotation : MKAnnotation
{
    string _title;

    public SampleMapAnnotation (CLLocationCoordinate2D coordinate)
    {
        Coordinate = coordinate;
        _title = "Sample";
    }

    public override CLLocationCoordinate2D Coordinate { get; set; }

    public override string Title {
        get {
            return _title;
        }
    }
}
```

Za pomocą protokołu jest powiązany z dowolnej klasy tego podklasy `MKAnnotation` może zapewnić odpowiednie dane, które będą używane przez mapy, podczas tworzenia widoku adnotacji. Dodawanie adnotacji do mapy, po prostu Wywołaj `AddAnnotation` metody `MKMapView` wystąpienie, jak pokazano w poniższym kodzie:

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
new CLLocationCoordinate2D (42.3467512, -71.0969456);

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

Zmienna mapy w tym miejscu jest wystąpieniem `MKMapView`, która jest klasa, która reprezentuje mapy. `MKMapView` Użyje `Coordinate` dane pochodzące z `SampleMapAnnotation` wystąpienie do pozycji widok adnotacji na mapie.

`MKAnnotation` Protokół zapewnia znanego zestawu funkcji przez wszystkie obiekty, które implementuje ona bez konsumenta (mapy w tym przypadku) konieczności wiedzieć o szczegóły implementacji. Usprawnia to dodawanie różne możliwe adnotacje do mapy.

 <a name="Protocols_Deep_Dive" />


### <a name="protocols-deep-dive"></a>Protokoły nowości

Ponieważ interfejsy języka C# nie obsługuje metody opcjonalne, Xamarin.iOS mapuje protokołów do klasy abstrakcyjnej. W związku z tym przyjmowanie protokołu w języku Objective C odbywa się w platformy Xamarin.iOS przy tworzenia klasy pochodnej z klasy abstrakcyjnej, który jest powiązany z protokołem i wdrażanie wymaganych metod. Te metody mają być widoczne jako metody abstrakcyjne klasy. Opcjonalny metody protokołu, który zostanie powiązany wirtualnej metody klasy C#.

Na przykład poniżej przedstawiono fragment `UITableViewDataSource` protokołu jako powiązane w Xamarin.iOS:

```csharp
public abstract class UITableViewDataSource : NSObject
{
    [Export ("tableView:cellForRowAtIndexPath:")]
    public abstract UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath);
    [Export ("numberOfSectionsInTableView:")]
    public virtual int NumberOfSections (UITableView tableView){...}
...
}
```

Należy pamiętać, że klasa jest abstrakcyjna. Xamarin.iOS sprawia, że klasa jest abstrakcyjny, który obsługuje opcjonalne/wymagane metod w protokołach.
Jednak w przeciwieństwie do języka Objective-C protokołów (lub interfejsy języka C#), klas C# nie obsługuje dziedziczenie wielokrotne. Ma to wpływ na projekt kodu C#, która korzysta z protokołów i zwykle prowadzi do klasy zagnieżdżone. Więcej o tym problemie jest uwzględnione w dalszej części tego dokumentu, w sekcji delegatów.

 `GetCell(…)` Metoda abstrakcyjna, powiązany z języka Objective C jest *selektora*, `tableView:cellForRowAtIndexPath:`, która jest wymagana metoda `UITableViewDataSource` protokołu. Selektor to termin języka Objective C dla nazwy metody. Aby wymusić metodę zgodnie z wymaganiami, Xamarin.iOS deklaruje go jako abstrakcyjny. Inne metody `NumberOfSections(…)`, jest powiązany z `numberOfSectionsInTableview:`. Ta metoda jest opcjonalne w protokole, więc Xamarin.iOS deklaruje, go jako wirtualnego, dzięki czemu można opcjonalnie do zastąpienia w języku C#.

Xamarin.iOS odpowiada on za wszystkie powiązania z systemem iOS dla Ciebie. Jednak jeśli trzeba będzie ręcznie powiązania protokołu z języka Objective-C, możesz to zrobić przez dekoracji klasy z `ExportAttribute`. Jest to tej samej metody używane przez Xamarin.iOS samej siebie.

Aby uzyskać więcej informacji na temat powiązać typów języka Objective-C w Xamarin.iOS, zobacz artykuł [powiązanie typów języka Objective-C](~/ios/platform/binding-objective-c/index.md).

Nie jesteśmy za pośrednictwem z protokołami jeszcze, mimo że. One są również używane w systemie iOS jako podstawy do delegatów Objective-C, co stanowi temat następnej sekcji.

 <a name="Delegates" />


## <a name="delegates"></a>Delegaty

iOS używa delegatów Objective-C do implementacji wzorca delegowania, w której jeden obiekt przekazuje pracy do innego. Obiekt wykonywania pracy jest delegat pierwszy obiekt. Obiekt informuje swojego delegata do pracy przez wysłanie wiadomości od wprowadzenia pewne zagadnienia. Wysyła komunikat typu w języku Objective C jest funkcjonalnym odpowiednikiem wywołania metody w języku C#. Delegat implementuje metody w odpowiedzi na te wywołania, a więc zapewnia funkcje do aplikacji.

Obiekty delegowane umożliwiają rozszerzanie zachowanie klas bez konieczności tworzenia podklasy. Aplikacje w systemie iOS często używają delegatów, gdy jedna klasa wywołuje do innego po wystąpieniu ważne akcji. Na przykład `MKMapView` klasy swojego delegata wywołania po naciśnięciu adnotacji na mapie, podając autora klasa obiektu delegowanego możliwość udzielenia odpowiedzi aplikacji. Za pomocą z przykładem użycia delegata w dalszej części tego artykułu, na przykład przy użyciu tego typu można pracować delegata z platformy Xamarin.iOS.

W tym momencie możesz się zastanawiać, jak klasa określa, jakie metody do wywołania w jego delegata. Jest to inne miejsce, w których są używane protokoły. Zazwyczaj dostępne metody delegata pochodzą z protokołów, które przyjmują.

 <a name="How_Protocols_are_used_with_Delegates" />


### <a name="how-protocols-are-used-with-delegates"></a>Jak protokoły są używane z delegatów

Widzieliśmy wcześniej jak protokoły są używane do obsługi dodawania adnotacji do mapy.
Protokoły są również używane do zapewnienia znanego zestawu metod klasy do wywołania po określone zdarzenia wystąpić, takie jak po podsłuchu użytkownika adnotacji na mapie lub zaznacza komórki w tabeli. Klasy, które implementują te metody są nazywane delegatów klasy, które połączeń telefonicznych z nimi.

Klasy, które obsługują delegowania to zrobić przez udostępnienie właściwości delegata, do której jest przypisany do klasy implementującej delegata. Metody, które implementuje dla obiekt delegowany zależy od protokołu, który przyjmuje określonych pełnomocników. Dla `UITableView` implementacji metody, `UITableViewDelegate` protokół, dla `UIAccelerometer` metoda może implementować `UIAccelerometerDelegate`i tak dalej do innych klas w ciągu z systemem iOS, dla którego należy udostępnić delegata.

`MKMapView` Klasy widzieliśmy w naszym przykładzie wcześniejszych ma również właściwość o nazwie delegata, która zostanie wywołany po różnych zdarzeń. Delegat dla `MKMapView` jest typu `MKMapViewDelegate`.
Użyjesz to wkrótce w przykład odpowiedzieć adnotacja po zaznaczeniu, ale najpierw teraz omówimy różnica między silne i słabe delegatów.

 <a name="Strong_Delegates_vs._Weak_Delegates" />


### <a name="strong-delegates-vs-weak-delegates"></a>Obiekty delegowane silne vs. Słabe delegatów

Delegatów, które zostały analizujemy wykonanej do tej pory są silne delegatów, co oznacza, że są one silnie typizowane. Powiązania Xamarin.iOS składnikiem silnie typizowanej klasy dla każdego protokołu delegata w systemie iOS. IOS ma również pojęcie słabe delegata. Tworzenie podklas klasy powiązane z protokołu Objective-C w przypadku określonych pełnomocników, a nie iOS umożliwia również powiązania samodzielnie metod protokołu w dowolnej klasy Ci się podoba, która pochodzi z NSObject dekoracji metody z ExportAttribute, a następnie dostarczanie odpowiednich selektorów.
W przypadku zastosowania tego podejścia wystąpienia klasy są przypisywane do właściwości WeakDelegate zamiast właściwości obiektu delegowanego. Słabe delegata zapewnia elastyczność podjęcie klasa delegata w dół hierarchii dziedziczenia różnych. Oto przykład Xamarin.iOS, który używa zarówno silne i słabe delegatów.

 <a name="Example_using_a_Delegate_with_Xamarin.iOS" />


### <a name="example-using-a-delegate-with-xamarinios"></a>Przykład użycia delegata z platformy Xamarin.iOS

Wykonanie kodu w odpowiedzi na użytkownika, wybierając polecenie adnotacji w naszym przykładzie, możemy podklasy `MKMapViewDelegate` i przypisz wystąpienie do `MKMapView`w `Delegate` właściwości. `MKMapViewDelegate` Protokół zawiera tylko opcjonalny metody.
W związku z tym wirtualnych są wszystkie metody powiązanych protokołu w Xamarin.iOS `MKMapViewDelegate` klasy. Gdy użytkownik wybierze adnotacji `MKMapView` wyśle wystąpienia `mapView:didSelectAnnotationView:` komunikat do swojego delegata. Aby obsłużyć to w Xamarin.iOS, należy zastąpić `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` metoda podklasy MKMapViewDelegate następująco:

```csharp
public class SampleMapDelegate : MKMapViewDelegate
{
    public override void DidSelectAnnotationView (
        MKMapView mapView, MKAnnotationView annotationView)
    {
        var sampleAnnotation =
            annotationView.Annotation as SampleMapAnnotation;

        if (sampleAnnotation != null) {

            //demo accessing the coordinate of the selected annotation to
            //zoom in on it
            mapView.Region = MKCoordinateRegion.FromDistance(
                sampleAnnotation.Coordinate, 500, 500);

            //demo accessing the title of the selected annotation
            Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
        }
    }
}
```

Klasa SampleMapDelegate pokazanym powyżej jest zaimplementowany jako klasa zagnieżdżona w kontrolerze, który zawiera `MKMapView` wystąpienia. W języku Objective-C zobaczysz często przyjmuje wiele protokołów bezpośrednio w ramach klasy kontrolera. Jednak ponieważ protokoły są powiązane z klas w Xamarin.iOS, klasy, które implementują jednoznacznie delegatów są zwykle dołączone jako zagnieżdżonych klas.

Z implementacją klasy delegata w miejscu, wystarczy tylko utworzyć wystąpienia obiektu delegowanego w kontrolerze i przypisz go do `MKMapView`w `Delegate` właściwości, jak pokazano poniżej:

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    SampleMapDelegate _mapDelegate;
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        //set the map's delegate
        _mapDelegate = new SampleMapDelegate ();
        map.Delegate = _mapDelegate;
        ...
    }
    class SampleMapDelegate : MKMapViewDelegate
    {
        ...
    }
}
```

Użycia słabe delegata samo, należy powiązać metodę samodzielnie w dowolnej klasy, która jest pochodną `NSObject` i przypisz go do `WeakDelegate` właściwość `MKMapView`. Ponieważ `UIViewController` ostatecznie pochodną klasy `NSObject` (np. Każda Objective-C klasa w CocoaTouch), firma Microsoft może po prostu implementować metodę powiązany z `mapView:didSelectAnnotationView:` bezpośrednio w kontrolerze i przypisz kontrolera w celu `MKMapView`w `WeakDelegate`, co pozwala uniknąć konieczności bardzo zagnieżdżona klasa. Poniższy kod ilustruje tego podejścia:

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        //assign the controller directly to the weak delegate
        map.WeakDelegate = this;
    }
    //bind to the Objective-C selector mapView:didSelectAnnotationView:
    [Export("mapView:didSelectAnnotationView:")]
    public void DidSelectAnnotationView (MKMapView mapView,
        MKAnnotationView annotationView)
    {
        ...
    }
}
```

Podczas uruchamiania tego kodu, aplikacja działa dokładnie tak jak uruchomionej wersji jednoznacznie delegata. Korzyści z tego kodu jest, że słabe delegata nie wymaga utworzenia dodatkowe klasy, który został utworzony podczas użyliśmy jednoznacznie delegata. Jednak jeśli źródłem jest kosztem zabezpieczeń. Jeśli masz zamiar popełnienia błędu w selektorze, który został przekazany do `ExportAttribute`, nie możesz znaleźć do środowiska wykonawczego.

 <a name="Events_and_Delegates" />


### <a name="events-and-delegates"></a>Zdarzenia i Delegaty

Obiekty delegowane są używane do wywołania zwrotne w systemie iOS, podobnie jak w sposób .NET używa zdarzenia. Aby iOS interfejsów API i sposób ich używać delegatów Objective-C wydawać się bardziej .NET, Xamarin.iOS przedstawia zdarzenia platformy .NET w wielu miejscach, w którym obiekty delegowane są używane w systemie iOS.

Na przykład starszej implementacji gdzie `MKMapViewDelegate` odpowiedzi wybrana adnotacja można również implementowana w platformy Xamarin.iOS przy użyciu zdarzenia platformy .NET. W takim przypadku zdarzenie może być zdefiniowana w `MKMapView` i wywołać `DidSelectAnnotationView`. Może mieć `EventArgs` podklasą typu `MKMapViewAnnotationEventsArgs`. `View` Właściwość `MKMapViewAnnotationEventsArgs` pozwoli uzyskać odwołanie do widoku adnotacji, z którego można kontynuować tej samej implementacji konieczne było wcześniej jako ilustrowane tutaj:

```csharp
map.DidSelectAnnotationView += (s,e) => {
    var sampleAnnotation = e.View.Annotation as SampleMapAnnotation;
    if (sampleAnnotation != null) {
        //demo accessing the coordinate of the selected annotation to
        //zoom in on it
        mapView.Region = MKCoordinateRegion.FromDistance (
            sampleAnnotation.Coordinate, 500, 500);

        //demo accessing the title of the selected annotation
        Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
    }
};
```

 <a name="Summary" />


## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób użycia zdarzeń, protokołów i delegatów w platformy Xamarin.iOS. Widzieliśmy jak Xamarin.iOS opisuje normalne zdarzenia styl .NET dla formantów.
Następnie dowiedzieliśmy się o protokołów Objective-C, w tym, jak są one różne od interfejsów języka C# i używania platformy Xamarin.iOS je. Ponadto firma Microsoft zbadać delegatów Objective-C z punktu widzenia platformy Xamarin.iOS. Widzieliśmy jak Xamarin.iOS obsługuje zarówno silnie i słabo typizowanych obiektów delegowanych oraz sposób powiązania zdarzenia platformy .NET, aby delegować metody.


## <a name="related-links"></a>Linki pokrewne

- [Protokoły, delegaci i zdarzenia (przykład)](https://developer.xamarin.com/samples/Protocols_Delegates_Events/)
- [Witaj, iOS](~/ios/get-started/hello-ios/index.md)
- [Typ powiązania Objective-C](~/ios/platform/binding-objective-c/index.md)
- [Język programowania w języku Objective-C](http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Projektowanie interfejsów użytkownika w środowisku Xcode 4](http://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [Podstawowe umiejętności aplikacji dla systemu iOS](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
