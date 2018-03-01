---
title: "Przejścia kontrolera widoku"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: f85867f08b21a525937e1c938c7c8fd224554abc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="view-controller-transitions"></a>Przejścia kontrolera widoku

UIKit dodaje obsługę Dostosowywanie animowany przejścia, gdy przedstawienie widoku kontrolerów. Ta obsługa jest dołączana do wbudowanych kontrolery, a także wszystkich kontrolerach niestandardowych, które dziedziczą bezpośrednio z `UIViewController`. Ponadto `UICollectionViewController` wykorzystuje dostosowania przejścia kontrolera wykorzystać animowane przejścia w kolekcji układów widoku.

## <a name="custom-transitions"></a>Niestandardowe przejścia

Animowany przejścia między kontrolerami widoku w systemie iOS 7 jest w pełni dostosowywalny. `UIViewController` teraz obejmuje `TransitioningDelegate` właściwość, która udostępnia klasę zresztą niestandardowych do systemu, jeśli występuje przejście.

Aby użyć niestandardowej przejścia z `PresentViewController`:

1.  Ustaw `ModalPresentationStyle` do `UIModalPresentationStyle.Custom` na kontrolerze będą widoczne.
2.  Implementowanie `UIViewControllerTransitioningDelegate` można utworzyć klasy zresztą, który jest wystąpieniem `UIViewControllerAnimatedTransitioning` .
3.  Ustaw `TransitioningDelegate` dla właściwości wystąpienia `UIViewControllerTransitioningDelegate` , również na kontrolerze będą widoczne.
4.  Przedstawia widok kontroler.


Na przykład następujący kod stanowi kontrolera widoku typu `ControllerTwo` — `UIViewController` podklasy:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

Aplikację i naciskając przycisk powoduje, że animacja domyślna drugiego kontrolera widoku można animować w od dołu, jak pokazano poniżej:

 ![](transitions-images/no-custom-transition.png "Aplikację i naciskając przycisk powoduje, że animacja domyślna drugiego widoku kontrolerów, aby animować w od dołu")

Jednak ustawienie `ModalPresentationStyle` i `TransitioningDelegate` powoduje animacji niestandardowej przejścia:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom;
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

`TransitioningDelegate` Odpowiedzialną za tworzenie wystąpienia `UIViewControllerAnimatedTransitioning` podklasy - o nazwie `CustomAnimator` w poniższym przykładzie:

```csharp
public class TransitioningDelegate : UIViewControllerTransitioningDelegate
{
    CustomTransitionAnimator animator;

    public override IUIViewControllerAnimatedTransitioning PresentingController (UIViewController presented, UIViewController presenting, UIViewController source)
    {
        animator = new CustomTransitionAnimator ();
        return animator;
    }
}
```

Gdy odbywa się przejścia, system tworzy wystąpienie `IUIViewControllerContextTransitioning`, który go przekazywany do metody zresztą. `IUIViewControllerContextTransitioning` zawiera `ContainerView` położenie animacji, a także kontroler widoku inicjowanie przejścia i kontroler widoku trwa są przenoszone do.

`UIViewControllerAnimatedTransitioning` Klasa obsługuje rzeczywiste animacji. Musi być implementowana dwóch metod:

1.  `TransitionDuration` — Zwraca czasem trwania animacji w sekundach.
1.  `AnimateTransition` — przeprowadza rzeczywiste animacji.


Na przykład następujące klasy implementuje `UIViewControllerAnimatedTransitioning` do animowania ramki kontrolera widoku:

```csharp
public class CustomTransitionAnimator : UIViewControllerAnimatedTransitioning
{
    public CustomTransitionAnimator ()
    {
    }

    public override double TransitionDuration (IUIViewControllerContextTransitioning transitionContext)
    {
        return 1.0;
    }

    public override void AnimateTransition (IUIViewControllerContextTransitioning transitionContext)
        {
            var inView = transitionContext.ContainerView;
            var toVC = transitionContext.GetViewControllerForKey (UITransitionContext.ToViewControllerKey);
            var toView = toVC.View;

            inView.AddSubview (toView);

            var frame = toView.Frame;
            toView.Frame = CGRect.Empty;

            UIView.Animate (TransitionDuration (transitionContext), () => {
                toView.Frame = new CGRect (20, 20, frame.Width - 40, frame.Height - 40);
            }, () => {
                transitionContext.CompleteTransition (true);
            });
        }
}
```

Teraz, wybrany przycisk animacji są implementowane w `UIViewControllerAnimatedTransitioning` klasy jest używany:

 ![](transitions-images/custom-transition.png "Przykład powiększenia w celu uruchamiania")

## <a name="collection-view-transitions"></a>Przejścia widoków kolekcji

Kolekcja widoków ma wbudowaną obsługę tworzenia animowane przejścia:

-  **Kontrolery nawigacji** — animowane przejścia między dwiema `UICollectionViewController` wystąpień mogą opcjonalnie zostać obsłużone automatycznie po `UINavigationController` nimi zarządza.
-  **Przejście układu** — nowy `UICollectionViewTransitionLayout` klasa umożliwia interakcyjne przechodzenie między układów.


### <a name="navigation-controller-transitions"></a>Przejścia kontrolera nawigacji

Gdy jest używany w kontrolerze nawigacji `UICollectionViewController` obsługuje animowane przejścia między kontrolerami. Ta obsługa jest wbudowana i wymaga tylko kilku prostych czynności w celu wdrożenia:

1.  Ustaw `UseLayoutToLayoutNavigationTransitions` do `false` na `UICollectionViewController` .
1.  Dodaje wystąpienie `UICollectionViewController` do głównego kontrolera nawigacji stosu.
1.  Tworzenie drugiej `UICollectionViewController` i ustawić jej `UseLayoutToLayoutNavigtionTransitions` właściwości `true` .
1.  Wypychanie drugi `UICollectionViewController` na stosie kontrolera nawigacji.


Poniższy kod dodaje `UICollectionViewController` podklasy o nazwie `ImagesCollectionViewController` do katalogu głównego stosu kontrolera nawigacji z `UseLayoutToLayoutNavigationTransitions` ustawioną właściwość `false`:

```csharp
UIWindow window;
ImagesCollectionViewController viewController;
UICollectionViewFlowLayout layout;
UINavigationController navController;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    window = new UIWindow (UIScreen.MainScreen.Bounds);

    // create and initialize a UICollectionViewFlowLayout
    layout = new UICollectionViewFlowLayout (){
        SectionInset = new UIEdgeInsets (10,5,10,5),
        MinimumInteritemSpacing = 5,
        MinimumLineSpacing = 5,
        ItemSize = new CGSize (100, 100)
    };

    viewController = new ImagesCollectionViewController (layout) {
            UseLayoutToLayoutNavigationTransitions = false;
        }

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();
    
    return true;
}
```

Gdy element jest zaznaczony, drugie wystąpienie parametru `ImagesController` jest tworzony tylko przy użyciu klasy inny układ. Dla tego kontrolera `UseLayoutToLayoutNavigtionTransitions` ma ustawioną wartość `true`, jak pokazano poniżej:

```csharp
CircleLayout circleLayout;
ImagesCollectionViewController controller2;

...

public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // UseLayoutToLayoutNavigationTransitions when item is selected
        circleLayout = new CircleLayout (Monkeys.Instance.Count){
                ItemSize = new CGSize (100, 100)
            };
            
    controller2 = new ImagesCollectionViewController (circleLayout) {
        UseLayoutToLayoutNavigationTransitions = true;
        }

    NavigationController.PushViewController (controller2, true);
}
```

`UseLayoutToLayoutNavigationTransitions` Należy ustawić właściwość przed dodaniem kontrolera stos nawigacji. Z tego zestawu właściwości normalne poziomy przejścia przesuwanego zostaje zastąpiona opcją animowany przejścia między układów dwa kontrolery, jak przedstawiono poniżej:

![](transitions-images/nav2.png "Animowany przejścia między układów dwóch kontrolerów")

### <a name="transition-layout"></a>Układ przejścia

Oprócz obsługi przejścia układu w nawigacji kontrolerów, nowy układ o nazwie `UICollectionViewTransitionLayout` jest teraz dostępna. Ta klasa układu umożliwia sterowania interaktywnego podczas procesu przechodzenia układu, zezwalając `TransitionProgress` ustawiono z kodu. `UICollectionViewTransitionLayout` różni się od - i nie może zastąpić - `SetCollectionViewLayout` metody z iOS 6, który spowodował przejście animowany układu występuje. Tej metody nie dostarczył wbudowana obsługa kontrolowania postęp animowany przejścia.

 `UICollectionViewTransitionLayout` Umożliwia na przykład skonfigurować w celu kontroli przechodzenia między układów w odpowiedzi do interakcji z użytkownikiem, zarządzając oryginalny układ, a także układ przeznaczone do przejścia do aparat rozpoznawania gestów.

Kroki w celu wykonania interakcyjne przejścia w aparat rozpoznawania gestów przy użyciu `UICollectionViewTransitionLayout` są następujące:

1.  Utwórz aparat rozpoznawania gestów.
1.  Wywołanie `StartInteractiveTransition` metody `UICollectionView` , przekazanie jej układu celu i obsługi uzupełniania.
1.  Ustaw `TransitionProgress` właściwość `UICollectionViewTransitionLayout` wystąpienia zwrócony z `StartInteractiveTransition` metody.
1.  Unieważnienie układu.
1.  Wywołanie `FinishInteractiveTransition` metody `UICollectionView` aby zakończyć proces przechodzenia lub `CancelInteractiveTransition` metody, aby je anulować.  `FinishInteractiveTransition` powoduje animacji ukończyć jego przejścia do układu docelowej, natomiast `CancelInteractiveTransition` powoduje animacji powrotu do oryginalnego układu.
1.  Obsługa uzupełniania przejścia w procedurze obsługi zakończenia `StartInteractiveTransition` metody.
1.  Aparat rozpoznawania gestów dodania do widoku kolekcji.


Poniższy kod implementuje przejście interakcyjne układu w aparat rozpoznawania gestów uszczypnięcia:

```csharp
imagesController = new ImagesCollectionViewController (flowLayout);

nfloat sf = 0.4f;
UICollectionViewTransitionLayout trLayout = null;
UICollectionViewLayout nextLayout;

pinch = new UIPinchGestureRecognizer (g => {

    var progress = Math.Abs(1.0f -  g.Scale)/sf;

    if(trLayout == null){
        if(imagesController.CollectionView.CollectionViewLayout is CircleLayout)
            nextLayout = flowLayout;
        else
            nextLayout = circleLayout;

        trLayout = imagesController.CollectionView.StartInteractiveTransition (nextLayout, (completed, finished) => {   
            Console.WriteLine ("transition completed");
            trLayout = null;
        });
    }

    trLayout.TransitionProgress = (nfloat)progress;

    imagesController.CollectionView.CollectionViewLayout.InvalidateLayout ();

    if(g.State == UIGestureRecognizerState.Ended){
        if (trLayout.TransitionProgress > 0.5f)
            imagesController.CollectionView.FinishInteractiveTransition ();
        else
            imagesController.CollectionView.CancelInteractiveTransition ();
    }

});

imagesController.CollectionView.AddGestureRecognizer (pinch);

```

Jako użytkownik pinches widok kolekcji `TransitionProgress` ustawiono względem skali uszczypnięcia. W tej implementacji, gdy użytkownik kończy się uszczypnięcia przed przejście to 50% zakończona, anulowana przejścia. W przeciwnym wypadku przejścia zostało zakończone.




## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do systemu iOS 7 (przykład)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [System iOS 7 omówienie interfejsu użytkownika](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
