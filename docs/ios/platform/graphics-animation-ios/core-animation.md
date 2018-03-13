---
title: Animacja Core
description: "W tym artykule sprawdza framework Core animacji, pokazujący, jak wysoka wydajność, płynne animacje w UIKit, oraz umożliwia użycie bezpośrednio do formantu animacji niższego poziomu."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f0cb4e00abffead854c2590bde6df45c200ff0bb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="core-animation"></a>Animacja Core

_W tym artykule sprawdza framework Core animacji, pokazujący, jak wysoka wydajność, płynne animacje w UIKit, oraz umożliwia użycie bezpośrednio do formantu animacji niższego poziomu._

obejmuje iOS [ *animacji Core* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) aby zapewnić obsługę animacji dla widoków w aplikacji.
Wszystkie niezwykle smooth animacji w systemie iOS, takie jak przewijanie tabel i szybko przesuwając między różnymi widokami wykonaj również robią, ponieważ opierają się na podstawowych animacji wewnętrznie.

Struktury animacji Core i Core grafiki mogą współdziałać, aby tworzenie doskonałych, animowany 2D grafiki. W rzeczywistości Core animacji można nawet przekształcać grafiki 2D w przestrzeni 3D, tworzenie niesamowitych, kinowe środowiska. Jednak aby utworzyć true grafiki 3D, konieczne będzie używać języków, takich jak OpenGL ES lub Włącz gry do interfejsu API, takich jak MonoGame, mimo że 3D wykracza poza zakres tego artykułu.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>Animacja Core

iOS używa framework Core animacji w celu utworzenia efekty animacji, takich jak przechodzenie między widokami, przedłużanie menu i przewijanie efekty kilka. Istnieją dwa sposoby pracy z animacji:

- [Za pomocą UIKit](#Using_UIKit_Animation), która obejmuje animacji na podstawie widoku, a także animowane przejścia między kontrolerami.
- [Za pomocą animacji Core](#Using_Core_Animation), które warstwy bezpośrednio, co zapewnia bardziej precyzyjną system kontroli.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>Przy użyciu UIKit animacji

UIKit zawiera kilka funkcji, które ułatwiają dodawanie animacji do aplikacji. Mimo że Core animacji jest używana wewnętrznie, jego abstracts go optymalizacji Aby pracować tylko z widoków i kontrolerów.

W tej sekcji omówiono UIKit animacji funkcje, takie jak:

-  Przejścia między kontrolerami
-  Przejścia między widokami
-  Wyświetl właściwości animacji


### <a name="view-controller-transitions"></a>Przejścia kontrolera widoku

 `UIViewController` udostępnia wbudowaną obsługę przechodzenie między kontrolerami widoku za pośrednictwem `PresentViewController` metody. Korzystając z `PresentViewController`, opcjonalnie można animować przejścia do drugiego kontrolera.

Rozważmy na przykład aplikacji przy użyciu dwóch kontrolerów, gdzie dotknięcie przycisk w pierwszy kontroler wywołuje `PresentViewController` do wyświetlenia drugiego kontrolera. Aby kontrolować, jakie animacji przejścia służy do wyświetlenia drugiego kontrolera, ustaw jej [ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/) właściwości, jak pokazano poniżej:

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

W takim przypadku `PartialCurl` jest używana animacja, mimo że kilka innych są dostępne, w tym:

-  `CoverVertical` — Slajdów się w dolnej części ekranu
-  `CrossDissolve` — Znika widok stary i nowy widok stopniowo
-  `FlipHorizontal` -Przerzuć poziomym od prawej do lewej. Na zwolnienia przejścia Przerzuca od lewej do prawej.


Aby animować przejścia, należy przekazać `true` jako drugi argument `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

Poniższy zrzut ekranu pokazuje, jak wygląda przejścia `PartialCurl` przypadku:

 ![](core-animation-images/06-view-transitions.png "Ten zrzut ekranu przedstawia przejścia PartialCurl")

### <a name="view-transitions"></a>Widok przejścia

Oprócz przejścia między kontrolerami UIKit obsługuje również animacji przejścia między widokami można zamienić jeden widok dla innej.

Na przykład, że ma kontroler z `UIImageView`, gdzie naciśnięcie obraz powinien być wyświetlany drugi `UIImageView`. Aby animować obrazu superview widoku do przejścia do drugiego widoku obrazu jest równie proste co wywołanie `UIView.Transition`, przekazanie jej `toView` i `fromView` w sposób przedstawiony poniżej:

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` ma również `duration` parametr, który określa, jak długo działa animacji, a także [ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/) do określenia czynności, takich jak animacji i funkcji sterowania tempem zmian. Ponadto można określić obsługi zakończenia, która będzie wywoływana po zakończeniu animacji.

Zrzut ekranu poniżej Pokaż Wyświetla animowany przejścia między obrazu podczas `TransitionFlipFromTop` jest używany:

 ![](core-animation-images/07-animated-transition.png "Ten zrzut ekranu przedstawia animowany przejścia między widokami obrazu stosowania TransitionFlipFromTop")

### <a name="view-property-animations"></a>Wyświetl właściwości animacji

Obsługuje UIKit animacji na różne właściwości `UIView` klasy bezpłatnie, w tym:

-  Klatka
-  Granice
-  Wyśrodkuj
-  Alpha
-  Transformacja
-  Kolor


Te animacji niejawnie się zdarzyć, określając właściwość zmiany w `NSAction` delegata przekazany do statycznego `UIView.Animate` metody. Na przykład następujący kod animuje punktu centralnego `UIImageView`:

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

Powoduje to obraz animacji i z powrotem w górnej części ekranu, jak pokazano poniżej:

 ![](core-animation-images/08-animate-center.png "Obraz animacji i z powrotem w górnej części ekranu jako dane wyjściowe")

Jak `Transition` metody `Animate` umożliwia czas trwania, aby ustawić, wraz z funkcji sterowania tempem zmian. W tym przykładzie również `UIViewAnimationOptions.Autoreverse` opcja, która powoduje animacji animować od wartości do jednego początkowej. Jednak kod także ustawia `Center` do swojej wartości początkowej programu obsługi zakończenia. Gdy animacji jest interpolacji wartości właściwości wraz z upływem czasu, faktycznej wartości modelu właściwości jest zawsze końcowa wartość, która została ustawiona. W tym przykładzie wartość jest punkt superview blisko prawej strony. Bez ustawienia `Center` do punktu początkowego, czyli gdzie zakończeniu animacji ze względu na `Autoreverse` ustawiany, obrazu będzie wracają do prawej strony po zakończeniu animacji, jak pokazano poniżej:

 ![](core-animation-images/09-animation-complete.png "Bez ustawień Centrum punkt początkowy, obrazu będzie wracają do prawej strony po zakończeniu animacji")

## <a name="using-core-animation"></a>Przy użyciu animacji Core

 `UIView` animacje Zezwalaj na wiele możliwości i powinna być używana, jeśli to możliwe ze względu na łatwość wdrażania. Jak wspomniano wcześniej, animacje UIView Użyj framework Core animacji. Jednak niektóre elementy nie można zrobić za pomocą `UIView` animacji, takich jak animacji dodatkowe właściwości, których nie można animować z widoku lub interpolacji ścieżce liniowej. W takich przypadkach, w którym ma być bardziej precyzyjną kontrolę Core animacji może służyć także bezpośrednio.

### <a name="layers"></a>Warstwy

Podczas pracy z animacji Core, animacja wykonuje się za pośrednictwem *warstwy*, które są typu `CALayer`. Warstwa jest podobny do widoku, w tym jest hierarchii warstw, podobnie jak wyświetlanie hierarchii. W rzeczywistości warstwy kopii widoków z widokiem dodanie obsługi interakcji z użytkownikiem. Dostęp można uzyskać warstwy każdego widoku przy użyciu widoku `Layer` właściwości. W rzeczywistości kontekst jest używany w `Draw` metody `UIView` faktycznie jest tworzona na podstawie warstwy. Wewnętrznie warstwie kopii `UIView` ma swojego delegata ustawiony na widok, który jest, co wymaga `Draw`. Tak, gdy rysunek `UIView`, faktycznie rysowania jego warstwy.

Warstwa animacji można niejawnym lub jawnym argumentem. Niejawne animacje są deklaratywne. Po prostu deklarować co należy zmienić właściwości warstwy i po prostu działa animacji. Jawne animacje z drugiej strony są tworzone za pomocą klasy animacji, który jest dodawany do warstwy. Jawne animacje umożliwiają dodanie kontroli nad procesem tworzenia animacji. Poniższe sekcje delve do animacji jawne i niejawne szczegółowo.

### <a name="implicit-animations"></a>Niejawne animacji

Jednym ze sposobów animowania właściwości warstwy odbywa się za pośrednictwem niejawne animacji. `UIView` animacje utworzyć niejawnego animacji. Jednak można utworzyć niejawnego animacje bezpośrednio przed również warstwy.

Na przykład następujący kod ustawia warstwy `Contents` z obrazu, Ustawia szerokość obramowania i koloru i dodaje warstwy jako podwarstwa warstwy widoku:

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

Aby dodać niejawne animacji dla warstwy, po prostu zawijać zmiany właściwości w `CATransaction`. Umożliwia to animowanie właściwości, które nie będzie można animować z animacją widoku, takie jak `BorderWidth` i `BorderColor` w sposób przedstawiony poniżej:

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

Ten kod również animuje warstwy `Position`, czyli lokalizacji zmierzony od lewego górnego rogu superlayer współrzędne punktu zakotwiczenia warstwy. Punkt kontrolny warstwy jest punktem znormalizowane w układzie współrzędnych warstwy.

Na poniższej ilustracji przedstawiono punktu pozycji i zakotwiczenia:

 ![](core-animation-images/10-postion-anchorpt.png "Na poniższym rysunku pokazano punktu pozycji i zakotwiczenia")

Po uruchomieniu przykładzie `Position`, `BorderWidth` i `BorderColor` animować, jak pokazano na poniższych zrzutach ekranu:

 ![](core-animation-images/11-implicit-animation.png "Po uruchomieniu przykładzie, pozycja, BorderWidth i BorderColor animować pokazany")

### <a name="explicit-animations"></a>Jawne animacji

Oprócz niejawne animacje animacji Core zawiera szereg klas, które dziedziczą z `CAAnimation` umożliwiające Hermetyzowanie animacji, które następnie są w sposób jawny dodane do warstwy. Umożliwiają one dopasowanymi bardziej precyzyjną kontrolę nad animacji, takie jak wartości rozpoczęcia animacji, grupowanie animacji i określenie kluczowych, aby zezwalać na ścieżki liniowej.

Poniższy kod przedstawia przykład zastosowania animacji jawne `CAKeyframeAnimation` warstwy przedstawionej (w sekcji niejawne animacji):

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

Zmiany tego kodu `Position` warstwy tworząc ścieżki, która jest następnie używany do definiowania animacji klatki kluczowej. Zwróć uwagę, że warstwy `Position` ma ustawioną wartość końcowa wartość `Position` z animacji. W przeciwnym razie warstwy nagle przywrócenie do jego `Position` przed animacji ponieważ animacji tylko zmienia wartość prezentacji i nie faktycznej wartości modelu. Za pomocą ustawienia wartości modelu do końcowej z animacji, warstwy pozostanie w miejscu po zakończeniu animacji.

Poniższe zrzuty ekranu pokazują warstwa zawierająca animacji obrazu przy użyciu określonej ścieżki:

 ![](core-animation-images/12-explicit-animation.png "Ten zrzut ekranu przedstawia warstwa zawierająca animacji obrazu przy użyciu określonej ścieżki")
 
## <a name="summary"></a>Podsumowanie

W tym artykule analizujemy dostępnych za pośrednictwem możliwości animacji *animacji Core* struktury. Firma Microsoft zbadać animacji Core, zawierający jak jego uprawnień animacji UIKit, i jak można bezpośrednio dla formantu animacji niższego poziomu.

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe animacji Core](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Podstawowe funkcje grafiki](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Grafika i wskazówki animacji](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Podstawowe funkcje animacji](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
