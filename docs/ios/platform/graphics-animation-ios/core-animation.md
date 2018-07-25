---
title: Podstawowe funkcje animacji w rozszerzeniu Xamarin.iOS
description: W tym artykule sprawdza framework podstawowe funkcje animacji, pokazujący, jak umożliwia wydajne i płynne animacje w strukturze UIKit, oraz jak z niego korzystać bezpośrednio do formantu animacji niższego poziomu.
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3d26e58822385c20f3c08d0b75ba468467c2c9b1
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242134"
---
# <a name="core-animation-in-xamarinios"></a>Podstawowe funkcje animacji w rozszerzeniu Xamarin.iOS

_W tym artykule sprawdza framework podstawowe funkcje animacji, pokazujący, jak umożliwia wydajne i płynne animacje w strukturze UIKit, oraz jak z niego korzystać bezpośrednio do formantu animacji niższego poziomu._

dla systemu iOS zawiera [ *podstawowe funkcje animacji* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) do obsługi animacji w widokach w aplikacji.
Wszystkie niezwykle płynne animacje w systemie iOS, takie jak przewijanie tabel i szybko przesuwając między różnymi widokami wykonaj również robią, ponieważ są zależne od podstawowe funkcje animacji wewnętrznie.

Podstawowe funkcje animacji i podstawowe funkcje grafiki struktury mogą współpracować ze sobą do tworzenia atrakcyjnych, grafiki 2D animowany. W rzeczywistości podstawowe funkcje animacji można nawet przekształcać grafika 2D w przestrzeni 3D, tworzenie atrakcyjnych, filmowe środowiska. Jednak do tworzenia grafiki 3D wartość true, należy używać języków, takich jak OpenGL ES lub Włącz gry do interfejsu API, takich jak MonoGame, mimo że 3D wykracza poza zakres tego artykułu.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>Podstawowe funkcje animacji

dla systemu iOS używa framework podstawowe funkcje animacji do tworzenia efektów animacji, takie jak przechodzenie między widokami, przedłużanie menu i przewijanie efekty kilka. Istnieją dwa sposoby pracy z animacji:

- [Za pomocą UIKit](#Using_UIKit_Animation), w tym z wykorzystaniem animacji opartych na widoku, a także animowane przejścia między kontrolerami.
- [Za pomocą podstawowe funkcje animacji](#Using_Core_Animation), warstw, które bezpośrednio, co zapewnia bardziej szczegółowej kontroli.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>Przy użyciu UIKit animacji

UIKit udostępnia kilka funkcji, które ułatwiają dodawanie animacji do aplikacji. Mimo że podstawowe funkcje animacji jest używana wewnętrznie, jej wagę je więc pracować tylko z widoków i kontrolerów.

W tej sekcji omówiono funkcje animacji UIKit, w tym:

-  Przejścia między kontrolerami
-  Przejścia między widokami
-  Wyświetl właściwości animacji


### <a name="view-controller-transitions"></a>Przejścia kontrolera widoku

 `UIViewController` udostępnia wbudowaną obsługę przechodzenie między kontrolerami widoku za pośrednictwem `PresentViewController` metody. Korzystając z `PresentViewController`, przejście do drugiego kontrolera opcjonalnie mogą być animowane.

Rozważmy na przykład aplikacji za pomocą dwóch kontrolerów, w którym wywołuje dotknięcie przycisku w pierwszy kontroler `PresentViewController` do wyświetlenia drugiego kontrolera. Aby kontrolować, jakie animacji przejścia służy do pokazywania drugiego kontrolera, wystarczy ustawić dla jego [ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/) właściwości, jak pokazano poniżej:

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

W tym przypadku `PartialCurl` animacji jest używana, mimo że kilka innych są dostępne, w tym:

-  `CoverVertical` — Slajdy górę od dolnej części ekranu
-  `CrossDissolve` — Stary widok znika i stopniowo nowy widok
-  `FlipHorizontal` -Przerzuć poziomej od prawej do lewej. Na zwolnienia przejścia Przerzuca od lewej do prawej.


Aby animować przejścia, należy przekazać `true` jako drugi argument `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

Poniższy zrzut ekranu pokazuje, jak wygląda przejścia `PartialCurl` przypadków:

 ![](core-animation-images/06-view-transitions.png "Ten zrzut ekranu przedstawia przejście PartialCurl")

### <a name="view-transitions"></a>Wyświetl przejścia

Oprócz przejścia między kontrolerami UIKit obsługuje również animowanie przejścia między widokami w celu wymiany jeden widok dla innego.

Na przykład, załóżmy, że masz kontroler z `UIImageView`, gdzie naciskając obraz powinien być wyświetlany sekundy `UIImageView`. Aby animować obrazu superview widoku, do którego nastąpi przejście do drugiego widoku obrazu jest proste co wywołanie metody `UIView.Transition`, podając mu `toView` i `fromView` jak pokazano poniżej:

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` pobiera również `duration` parametr, który określa czas odliczania animacji, a także [ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/) określić elementy, takie jak animacji i funkcji sterowania tempem zmian. Ponadto można określić procedury obsługi zakończenia, który będzie wywoływany przy zakończeniu animacji.

Zrzut ekranu poniżej przedstawiono animowany przejścia między obrazu widoków, kiedy `TransitionFlipFromTop` jest używany:

 ![](core-animation-images/07-animated-transition.png "Ten zrzut ekranu przedstawia animowany przejścia między widokami obraz stosowania TransitionFlipFromTop")

### <a name="view-property-animations"></a>Animacje tej właściwości widoku

UIKit obsługuje animowanie różne właściwości na `UIView` za darmo klasy, w tym:

-  Klatka
-  Granice
-  Wyśrodkuj
-  alfa
-  Transformacja
-  Kolor


Te animacji niejawnie się zdarzyć, określając zmiany właściwości `NSAction` obiekt delegowany przekazany do statycznej `UIView.Animate` metody. Na przykład, poniższy kod animuje punktu centralnego `UIImageView`:

```csharp
pt = imgView.Center;

UIView.Animate (
    duration: 2, 
    delay: 0, 
    options: UIViewAnimationOptions.CurveEaseInOut | 
        UIViewAnimationOptions.Autoreverse,
    animation: () => {
        imgView.Center = new CGPoint (View.Bounds.GetMaxX () 
            - imgView.Frame.Width / 2, pt.Y);},
    completion: () => {
        imgView.Center = pt; }
);
```

Skutkuje to obraz animowanie i z powrotem w górnej części ekranu, jak pokazano poniżej:

 ![](core-animation-images/08-animate-center.png "Obraz animowanie i z powrotem w górnej części ekranu jako dane wyjściowe")

Podobnie jak w przypadku `Transition` metody `Animate` zezwala na czas trwania ustawić, wraz z funkcji sterowania tempem zmian. W tym przykładzie, używali także `UIViewAnimationOptions.Autoreverse` opcja, która powoduje, że animacja animacji od wartości, wróć do tego początkowego. Jednak kod ustawia również `Center` do swojej wartości początkowej programu obsługi zakończenia. Podczas animacji jest interpolacji wartości właściwości wraz z upływem czasu, faktycznej wartości modelu właściwości jest zawsze końcowa wartość, która została ustawiona. W tym przykładzie wartość jest punktem blisko prawej superview. Bez ustawienia `Center` do punktu początkowego, czyli, gdzie zakończeniu animacji ze względu na `Autoreverse` ustawiona, obraz, który będzie wracają do prawej strony po zakończeniu animacji, jak pokazano poniżej:

 ![](core-animation-images/09-animation-complete.png "Bez konieczności ustawiania Centrum do punktu początkowego, obraz, który będzie wracają do prawej po zakończeniu animacji")

## <a name="using-core-animation"></a>Za pomocą podstawowe funkcje animacji

 `UIView` animacje Zezwalaj na wiele możliwości i powinny być używane, jeśli jest to możliwe ze względu na łatwość implementacji. Jak wspomniano wcześniej, animacje UIView Użyj framework podstawowe funkcje animacji. Jednak niektóre elementy nie można wykonać za pomocą `UIView` animacji, takich jak animowanie dodatkowe właściwości, które nie mogą być animowane przy użyciu widoku lub interpolacji nieliniowych ścieżce. W takich przypadkach, gdzie potrzebujesz bardziej precyzyjną kontrolę podstawowe funkcje animacji można także bezpośrednio.

### <a name="layers"></a>Warstwy

Jeśli pracujesz w języku podstawowe funkcje animacji, animacji odbywa się za pośrednictwem *warstwy*, które są typu `CALayer`. Warstwy jest podobna do widoku, w tym ma hierarchię warstwy znacznie tak jak ma to hierarchia widoku. W rzeczywistości warstwy kopii widoków, widok dodanie obsługi interakcji z użytkownikiem. Możesz uzyskać dostęp warstwy dowolny widok za pośrednictwem widoku `Layer` właściwości. W rzeczywistości kontekst używany w `Draw` metody `UIView` jest faktycznie tworzona z warstwy. Wewnętrznie warstwy kopii `UIView` ma jego delegata ustawiony na wartość view, czyli o tym, co wywołuje `Draw`. Tak, gdy rysunek do `UIView`, faktycznie rysowania jej warstwy.

Animacje warstwa może być jawne lub niejawne. Niejawne animacji są deklaratywnego. Po prostu zadeklarować co należy zmieniać właściwości warstwy i animacji po prostu działa. Jawne animacji z drugiej strony są tworzone za pomocą klasy animacji, który jest dodawany do warstwy. Jawne animacje umożliwiają dodanie kontrolę sposobu tworzenia animacji. Poniższe sekcje delve do animacji jawne i niejawne pogłębione.

### <a name="implicit-animations"></a>Niejawne animacji

Jest jednym ze sposobów, aby animować właściwości warstwy za pomocą niejawnych animacji. `UIView` animacje tworzenie niejawne animacji. Jednak można utworzyć niejawnego animacji bezpośrednio w odniesieniu do warstwy, a także.

Na przykład, poniższy kod ustawia warstwy `Contents` z obrazu, Ustawia szerokość obramowania i kolor i dodaje warstwy jako podwarstwę warstwę widoku:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    layer = new CALayer ();
    layer.Bounds = new CGRect (0, 0, 50, 50);
    layer.Position = new CGPoint (50, 50);
    layer.Contents = UIImage.FromFile ("monkey2.png").CGImage;
    layer.ContentsGravity = CALayer.GravityResize;
    layer.BorderWidth = 1.5f;
    layer.BorderColor = UIColor.Green.CGColor;

    View.Layer.AddSublayer (layer);
}
```

Aby dodać animację niejawne dla warstwy, po prostu opakować zmiany właściwości `CATransaction`. Umożliwia to animowanie właściwości, które nie będą animatable z animacją widoku, takie jak `BorderWidth` i `BorderColor` jak pokazano poniżej:

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    CATransaction.Begin ();
    CATransaction.AnimationDuration = 10;
    layer.Position = new CGPoint (50, 400);
    layer.BorderWidth = 5.0f;
    layer.BorderColor = UIColor.Red.CGColor;
    CATransaction.Commit ();
}
```

Ten kod jest również animuje warstwy `Position`, która jest lokalizacją mierzony od lewego górnego rogu superlayer współrzędne punktu kontrolnego warstwy. Punkt kontrolny warstwy jest znormalizowana punktu, w tym układzie współrzędnych warstwy.

Poniższa ilustracja przedstawia położenie i zakotwiczenia punkt:

 ![](core-animation-images/10-postion-anchorpt.png "Poniższy rysunek pokazuje punktu pozycji i zakotwiczenia")

Po uruchomieniu przykładu `Position`, `BorderWidth` i `BorderColor` animować, jak pokazano na poniższych zrzutach ekranu:

 ![](core-animation-images/11-implicit-animation.png "Po uruchomieniu przykładu pozycja, BorderWidth i BorderColor animować pokazany")

### <a name="explicit-animations"></a>Jawne animacji

Oprócz niejawne animacji podstawowe funkcje animacji obejmuje szereg klas, które dziedziczą z `CAAnimation` umożliwiające hermetyzacji animacji, które następnie są jawnie dodawane do warstwy. Umożliwiają one bardziej szczegółowej kontroli nad animacji, takich jak modyfikowanie wartości początkowej animacji, grupowanie animacji i określając ramek kluczowych, aby zezwalać na inny niż liniowy ścieżki.

Poniższy kod przedstawia przykład jawne animację z użyciem `CAKeyframeAnimation` dla warstwy przedstawiony we wcześniejszej części (sekcja niejawne animacji):

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);
    
    // get the initial value to start the animation from
    CGPoint fromPt = layer.Position;
    
    /* set the position to coincide with the final animation value
    to prevent it from snapping back to the starting position
    after the animation completes*/
    layer.Position = new CGPoint (200, 300);
    
    // create a path for the animation to follow
    CGPath path = new CGPath ();
    path.AddLines (new CGPoint[] { fromPt, new CGPoint (50, 300), new CGPoint (200, 50), new CGPoint (200, 300) });
    
    // create a keyframe animation for the position using the path
    CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
    animPosition.Path = path;
    animPosition.Duration = 2;
    
    // add the animation to the layer.
    /* the "position" key is used to overwrite the implicit animation created
    when the layer positino is set above*/
    layer.AddAnimation (animPosition, "position");
}
```

Ten kod zmienia `Position` warstwy, tworząc ścieżkę, która jest następnie używany do definiowania odtwarzania klatki kluczowej. Należy zauważyć, że warstwy `Position` ustawiono końcowa wartość `Position` z animacji. Bez tego warstwy nagle zwróci się do jego `Position` przed animacji ponieważ animacji zmienia się tylko wtedy wartość prezentacji i nie faktycznej wartości modelu. Ustawiając wartość modelu końcowa wartość z animacji, warstwy utrzymane na końcu animacji.

Poniższych zrzutach ekranu przedstawiono warstwa zawierająca animowanie obrazu przy użyciu określonej ścieżki:

 ![](core-animation-images/12-explicit-animation.png "Ten zrzut ekranu przedstawia warstwa zawierająca animowanie obrazu przy użyciu określonej ścieżki")
 
## <a name="summary"></a>Podsumowanie

W tym artykule przyjrzeliśmy się możliwości animacje, udostępniane za pośrednictwem *podstawowe funkcje animacji* struktur. Zbadaliśmy podstawowe funkcje animacji, przedstawiający sposób obsługuje ona animacji w strukturze UIKit i jak może służyć bezpośrednio do formantu animacji niższego poziomu.

## <a name="related-links"></a>Linki pokrewne

- [Przykład animacji Core](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Podstawowe funkcje grafiki](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Grafika i animacje wskazówki](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Podstawowe funkcje animacji](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
