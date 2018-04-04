---
title: Wprowadzenie do systemu iOS 7
description: W tym artykule omówiono głównych wprowadzone w systemie iOS 7, takich jak przejścia kontrolera widoku, ulepszenia UIView animacji, UIKit Dynamics i tekst zestawu nowych interfejsów API. Obejmuje ona również niektórych zmian w interfejsie użytkownika i nowe funkcje wielozadaniowości enchanced.
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 9ae82eba78f099f675d21bf53a250923630a0ff6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-ios-7"></a>Wprowadzenie do systemu iOS 7

_W tym artykule omówiono głównych wprowadzone w systemie iOS 7, takich jak przejścia kontrolera widoku, ulepszenia UIView animacji, UIKit Dynamics i tekst zestawu nowych interfejsów API. Obejmuje ona również niektórych zmian w interfejsie użytkownika i nowe funkcje wielozadaniowości enchanced._

System iOS 7 jest dużej aktualizacji do systemu iOS. Podaj zupełnie nowego projektu interfejsu użytkownika, który umieszcza fokus na chrome zawartości, a nie aplikacji. Razem zmiany wizualne system iOS 7 dodaje nadmiar nowych interfejsów API do tworzenia bardziej zaawansowane funkcje interakcji i środowiska. Ten dokument ankiet nowych technologii wprowadzone w systemie iOS 7 i służy jako punkt początkowy dla dalszego eksploracji.

## <a name="uiview-animation-enhancements"></a>Ulepszenia animacji UIView

System iOS 7 wspomaga obsługę animacji w UIKit, dzięki czemu aplikacje wykonać czynności, które wcześniej wymagały usunięcie bezpośrednio w ramach Core animacji. Na przykład `UIView` można teraz wykonywać animacji wzrastania, a także klatek kluczowych animacji, które wcześniej `CAKeyframeAnimation` stosowane do `CALayer`.

### <a name="spring-animations"></a>Animacji wzrastania

 `UIView` obsługuje teraz animowanie zmian właściwości z efektem sprężyny. Aby dodać ten, wywołaj `AnimateNotify` lub `AnimateNotifyAsync` metody, przekazując wartości stosunek tłumienia spring i prędkość spring początkowej zgodnie z poniższym opisem:

-  `springWithDampingRatio` — Wartość z zakresu od 0 do 1, w którym drgań rośnie mniejszą wartość.
-  `initialSpringVelocity` — Spring początkowej prędkości w procentach odległość całkowita animacji na sekundę.


Poniższy kod tworzy efekt spring po zmianie środek widoku obrazu:

```csharp
void AnimateWithSpring ()
{ 
    float springDampingRatio = 0.25f;
    float initialSpringVelocity = 1.0f;
    
    UIView.AnimateNotify (3.0, 0.0, springDampingRatio, initialSpringVelocity, 0, () => {
    
        imageView.Center = new CGPoint (imageView.Center.X, 400);   
            
    }, null);
}
```

W tym celu spring powoduje, że widok obraz wydaje się Odbijanie po ukończeniu animacji do nowej lokalizacji Centrum, jak przedstawiono poniżej:

 ![](images/spring-animation.png "W tym celu spring powoduje, że widok obraz wydaje się Odbijanie po ukończeniu animacji do nowej lokalizacji center")

### <a name="keyframe-animations"></a>Klatek kluczowych animacji

`UIView` Zawiera teraz klasy `AnimateWithKeyframes` metodę tworzenia klatek kluczowych animacji na `UIView`. Ta metoda jest podobna do innych `UIView` z wyjątkiem metod animacji, które dodatkowo `NSAction` jest przekazywana jako parametr do uwzględnienia klatek kluczowych. W ramach `NSAction`, kluczowych są dodawane przez wywołanie metody `UIView.AddKeyframeWithRelativeStartTime`.

Na przykład poniższy fragment kodu tworzy odtwarzania klatki kluczowej do animowania widoku center oraz aby obrócić widok:

```csharp
void AnimateViewWithKeyframes ()
{
    var initialTransform = imageView.Transform;
    var initialCeneter = imageView.Center;

    // can now use keyframes directly on UIView without needing to drop directly into Core Animation

    UIView.AnimateKeyframes (2.0, 0, UIViewKeyframeAnimationOptions.Autoreverse, () => {
        UIView.AddKeyframeWithRelativeStartTime (0.0, 0.5, () => { 
            imageView.Center = new CGPoint (200, 200);
        });

        UIView.AddKeyframeWithRelativeStartTime (0.5, 0.5, () => { 
            imageView.Transform = CGAffineTransform.MakeRotation ((float)Math.PI / 2);
        });
    }, (finished) => {
        imageView.Center = initialCeneter;
        imageView.Transform = initialTransform;

        AnimateWithSpring ();
    });
}
```

Pierwsze dwa parametry `AddKeyframeWithRelativeStartTime` metody Określ czas rozpoczęcia i czas trwania klatki kluczowej, odpowiednio, jako procent całkowitej długości animacji. Przykład powyżej powoduje obrazu widoku animacji do jego nowego Centrum w pierwszym drugim następuje obracanie 90 stopni w następnej sekundy. Ponieważ określa animacji `UIViewKeyframeAnimationOptions.Autoreverse` opcjonalnie zarówno klatek kluczowych animacji odwrotnie, a także. Na koniec końcowego wartości są ustawiane początkowy stan programu obsługi zakończenia.

Poniższe zrzuty ekranu przedstawiono animacji połączonych za pośrednictwem klatek kluczowych:

 ![](images/keyframes.png "Ta zrzuty ekranu przedstawiono animacji połączonych za pośrednictwem klatek kluczowych")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics to nowy zestaw interfejsów API w UIKit, które umożliwiają aplikacjom utworzyć animowany interakcje oparte na fizycznych. UIKit Dynamics hermetyzuje aparat 2D fizycznych, aby było to możliwe.

Interfejs API jest deklaratywnego charakteru. Należy zadeklarować zachowanie interakcje fizycznych, tworząc obiektów - o nazwie *zachowania* — aby express fizycznych pojęcia, takie jak grawitacji, kolizji, sprężyny itp. Następnie dołącz behavior(s) do innego obiektu o nazwie *dynamiczne zresztą*, który hermetyzuje widoku. Dynamiczne zresztą przyjmuje dba stosowania zachowań fizycznych zadeklarowanych powoduje *dynamiczne elementy* — elementy, które implementują `IUIDynamicItem`, takich jak `UIView`.

Istnieje kilka różnych pierwotnych zachowań dostępny do uruchomienia złożonych interakcji, w tym:

-  `UIAttachmentBehavior` — Dołącza dwóch elementów dynamicznych tak, aby była przenoszona razem lub dołącza elementów dynamicznych do punktu załącznika.
-  `UICollisionBehavior` — Umożliwia dynamiczne elementy do udziału w kolizji.
-  `UIDynamicItemBehavior` — Określa ogólne zbiór właściwości do zastosowania do dynamicznych elementów, takich jak elastyczność, gęstości i tarcie.
-  `UIGravityBehavior` -Dotyczy grawitacji elementów dynamicznych, powodując elementów w celu przyspieszenia w kierunku grawitacyjne.
-  `UIPushBehavior` — Dotyczy życie elementów dynamicznych.
-  `UISnapBehavior` — Umożliwia dynamiczny elementu przyciąganie do pozycji z efektem sprężyny.


Choć wiele podstawowych, ogólny proces dodawania interakcji opartych na fizycznych do widoku, używając UIKit Dynamics jest spójna między zachowania:

1.  Utwórz zresztą dynamicznych.
1.  Utwórz behavior(s).
1.  Dodaj zachowania do dynamicznego zresztą.


### <a name="dynamics-example"></a>Przykład Dynamics

Oto przykład, w którym dodaje grawitacji i granicę kolizji `UIView`.

#### <a name="uigravitybehavior"></a>UIGravityBehavior

Dodawanie grawitacji widokowi obraz poniżej 3 czynności opisanych powyżej.

Firma Microsoft będzie działać w `ViewDidLoad` metody w tym przykładzie. Najpierw dodaj `UIImageView` wystąpienia w następujący sposób:

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

Spowoduje to utworzenie widoku obrazu wyśrodkowany do górnej krawędzi ekranu. Aby z grawitacji obrazu "spadek", Utwórz wystąpienie `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

`UIDynamicAnimator` Przyjmuje wystąpienia odwołanie `UIView` lub `UICollectionViewLayout`, który zawiera elementy, które będą animowane na behavior(s) dołączone.

Następnie należy utworzyć `UIGravityBehavior` wystąpienia. Można przekazać co najmniej jeden obiekt implementacja `IUIDynamicItem`, takiej jak `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

Zachowanie jest przekazywany tablicę `IUIDynamicItem`, który w tym przypadku zawiera pojedynczą `UIImageView` wystąpienia są możemy animacji.

Na koniec należy dodać zachowanie do dynamicznego zresztą:

```csharp
dynAnimator.AddBehavior (gravity);
```

Powoduje to obraz animacji w dół o grawitacji jako ilustrowane poniżej:

![](images/gravity2.png "Położenie początkowe obrazu") 
![](images/gravity3.png "lokalizacja końcowa obrazu")

Ponieważ nie ma niczego ograniczający granice ekranu, widok obrazu po prostu znajduje się u dołu. Aby ograniczyć widoku, aby obraz, który powoduje konflikt z krawędzi ekranu, można dodać `UICollisionBehavior`. Firma Microsoft będzie obejmować w następnej sekcji.

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

Rozpocznie się przez utworzenie `UICollisionBehavior` i dodanie go do dynamicznego zresztą, tak samo, jak robiliśmy `UIGravityBehavior`.

Zmodyfikuj kod do obejmują `UICollisionBehavior`:

```csharp
using (image = UIImage.FromFile ("monkeys.jpg")) {

    imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
        Image =  image
    };

    View.AddSubview (imageView);

    // 1. create the dynamic animator
    dynAnimator = new UIDynamicAnimator (this.View);

    // 2. create behavior(s)
    var gravity = new UIGravityBehavior (imageView);
    var collision = new UICollisionBehavior (imageView) {
        TranslatesReferenceBoundsIntoBoundary = true
    };

    // 3. add behaviors(s) to the dynamic animator
    dynAnimator.AddBehaviors (gravity, collision);
}
```

`UICollisionBehavior` Ma właściwość o nazwie `TranslatesReferenceBoundsIntoBoundry`. To ustawienie `true` powoduje, że odwołanie granice widoku do użycia jako granic kolizji.

Teraz gdy obraz dół animuje z grawitacji, jego odrzuceń nieco u dołu ekranu przed rozpoczęciem do pozostałej istnieje.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

Firma Microsoft dodatkowo można kontrolować zachowanie objęte widoku obrazu za pomocą zachowań dodatkowe. Na przykład możemy dodać `UIDynamicItemBehavior` zwiększenie elastyczności, powodując widoku obrazu na więcej odbijanie, jeśli powoduje ona konflikt z dołu ekranu.

Dodawanie `UIDynamicItemBehavior` opisano te same kroki, podobnie jak w przypadku innych zachowań. Najpierw utwórz zachowanie:

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

Następnie należy dodać zachowanie do dynamicznego zresztą:

 `dynAnimator.AddBehavior (dynBehavior);`

Z tego zachowania w miejscu widoku obrazu odrzuceń więcej jeśli powoduje ona konflikt z granicą.

## <a name="general-user-interface-changes"></a>Interfejs użytkownika ogólne zmiany

Oprócz nowych interfejsów API UIKit UIKit Dynamics, przejścia kontrolera i rozszerzone animacje UIView opisane powyżej system iOS 7 wprowadzono szereg zmiany wizualne do interfejsu użytkownika i powiązanych zmian interfejsu API dla różnych widoków i kontrolek. Aby uzyskać więcej informacji, zobacz [systemu iOS 7 omówienie interfejsu użytkownika](~/ios/platform/introduction-to-ios7/ios7-ui.md).

## <a name="text-kit"></a>Zestaw tekstu

Tekst Kit to nowy interfejs API, który oferuje tekst zaawansowane funkcje układ i renderowania. Jest oparty na niski poziom framework Core tekstu, ale jest znacznie prostsze niż tekst Core korzystanie z.

Aby uzyskać więcej informacji, zobacz nasze [TextKit](~/ios/platform/textkit.md)

## <a name="multitasking"></a>Wielozadaniowości

System iOS 7 zmiany, kiedy i jak odbywa się praca w tle. Zadanie zostało zakończone w systemie iOS 7 nie zapewnia aplikacji wznowione, gdy zadania są uruchomione w tle i aplikacje są wybudzane dla przetwarzania w sposób nieciągłych w tle. System iOS 7 dodaje również trzech nowych interfejsów API aktualizowania aplikacji z nowej zawartości w tle:

-  Pobieranie w tle — umożliwia aplikacji, aby zaktualizować zawartość w tle w regularnych odstępach czasu.
-  Powiadomienia zdalnego - umożliwia aplikacjom zaktualizować zawartość podczas odbierania powiadomień wypychanych. Powiadomienia mogą być dyskretnej albo można wyświetlić banera na ekranie blokady.
-  Usługa transferu w tle — umożliwia wysyłanie i pobieranie danych, takich jak duże pliki, bez określonego czasu.


Aby uzyskać więcej informacji o nowych funkcjach wielozadaniowości, zobacz sekcje iOS Xamarin [przewodnik Backgrounding](~/ios/app-fundamentals/backgrounding/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono kilka najważniejszych nowych ma zostać dodany do systemu iOS. Najpierw go przedstawiono sposób dodawania niestandardowych przejścia do widoku kontrolerów. Następnie należy go przedstawia sposób użycia przejścia w widokach kolekcji, zarówno z wewnątrz kontrolera nawigacji, a także interaktywnie między widokiem kolekcji. Następnie podaj kilka ulepszeń do animacji UIView, pokazujący, jak aplikacje używają UIKit rzeczy, które wcześniej wymagały programowanie bezpośrednio przed Core animacji. Na koniec wprowadzono nowe UIKit Dynamics interfejsu API, który oferuje aparat fizyczny UIKit, obok obsługi tekstu sformatowanego teraz dostępne w ramach zestawu tekstu.

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do systemu iOS 7 (przykład)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Omówienie interfejsu użytkownika systemu iOS 7](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Uruchamianie procesów w tle](~/ios/app-fundamentals/backgrounding/index.md)
