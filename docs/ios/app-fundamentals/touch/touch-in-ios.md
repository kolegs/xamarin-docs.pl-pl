---
title: Touch w systemie iOS
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 78783089303eba09b0ee36534b0078b82674a1c6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="touch-in-ios"></a>Touch w systemie iOS

Ważne jest zrozumienie zdarzenia touch i touch interfejsów API w aplikacji systemu iOS, ponieważ są one centralnej na wszystkich fizycznych interakcji z urządzeniem. Wszystkie interakcje dotykowej dotyczą `UITouch` obiektu. W tym artykule firma Microsoft będzie Dowiedz się, jak używać `UITouch` klasy i jej interfejsów API do obsługi touch. Później firma Microsoft będzie rozszerzać na naszych wiedzy, aby dowiedzieć się, jak obsługiwać gestów.

## <a name="enabling-touch"></a>Włączanie Touch

Formanty w `UIKit` — te będące podklasami z kontrolne — są tak uzależnione interakcji z użytkownikiem, które mają gestów wbudowanej w UIKit i w związku z tym nie jest konieczne do włączenia Touch. Jest już włączony.

Jednak wiele widoków w `UIKit` nie mają touch domyślnie włączone. Istnieją dwa sposoby Włącz obsługę dotykową w formancie. Pierwszym sposobem jest wyboru włączenie interakcji użytkownika, w konsoli właściwości systemu IOS projektanta, jak pokazano na poniższym zrzucie ekranu:

 [![](touch-in-ios-images/image1.png "Zaznacz pole wyboru włączenia przez użytkownika interakcji w konsoli właściwości projektanta systemu IOS")](touch-in-ios-images/image1.png#lightbox)

Możemy również użyć kontrolera, aby ustawić `UserInteractionEnabled` właściwości na wartość true na `UIView` klasy. Jest to wymagane, jeśli interfejs użytkownika jest tworzona w kodzie.

Przykładem jest następujący wiersz kodu:

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>Touch zdarzenia

Istnieją trzy etapy touch występujących podczas użytkownik krawędzi ekranu, przenosi ich palca lub usuwa ich palca. Te metody są zdefiniowane w `UIResponder`, która jest klasą bazową dla UIView. iOS spowoduje zastąpienie metody skojarzone na `UIView` i `UIViewController` do obsługi dotykowej:

-  `TouchesBegan` — Jest to przy pierwszym jest dotknięciu ekranu.
-  `TouchesMoved` — To jest wywoływane, gdy lokalizacja zmiany touch jako użytkownik jest przedłużanie swoich palców po ekranie.
-  `TouchesEnded` lub `TouchesCancelled` — `TouchesEnded` jest wywoływane, gdy palców użytkownika są podnoszone poza ekranu.  `TouchesCancelled` pobiera wywoływana, gdy iOS anuluje touch — na przykład, jeśli użytkownik slajdów własnego palca poza przycisk, aby anulować, naciśnij przycisk.


Touch rekursywnie podróży zdarzenia w dół przez stos UIViews, aby sprawdzić, jeśli zdarzenie touch znajduje się w granicach obiektu widoku. Jest to często nazywane _testowania trafień_. Najpierw zostanie wywołana na najwyższym poziomie `UIView` lub `UIViewController` , a następnie zostanie wywołana na `UIView` i `UIViewControllers` znajdującą się pod nimi w hierarchii widoku.

A `UITouch` zostanie utworzony obiekt każdym razem, użytkownik dotyka ekranu. `UITouch` Obiekt zawiera dane dotyczące touch, takich jak touch wystąpienia, gdzie wystąpił, jeśli jednym naciśnięciem Przejdź itp. Zdarzenia touch zostały przekazane właściwości poprawek — `NSSet` zawierających jeden lub więcej poprawek. Firma Microsoft można używać tej właściwości można uzyskać odwołania do touch i określić odpowiedzi aplikacji.

Klasy przesłaniające zdarzeń touch należy najpierw wywoływać implementację podstawową, a następnie zachęcić `UITouch` obiekt powiązany ze zdarzeniem. Aby uzyskać odwołanie do pierwszego touch, należy wywołać `AnyObject` właściwości i rzutować go jako `UITouch` Pokaż tak jak w poniższym przykładzie:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        //code here to handle touch
    }
}
```

iOS automatycznie rozpoznaje kolejne szybkie dotyka na ekranie i zbierze wszystkie jako jedno naciśnięcie w jednej `UITouch` obiektu. Sprawia to, że sprawdzanie naciśnij dwa razy, wystarczy sprawdzanie `TapCount` właściwości, jak pokazano w poniższym kodzie:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        if (touch.TapCount == 2)
        {
            // do something with the double touch.
        }
    }
}
```

## <a name="multi-touch"></a>Multi-Touch

Obsługa wielodotyku nie jest włączone domyślnie na formanty. Obsługa wielodotyku można włączyć w systemie iOS projektanta, jak pokazano na poniższym zrzucie ekranu:

 [![](touch-in-ios-images/image2.png "Obsługa wielodotyku włączona w systemie iOS projektanta")](touch-in-ios-images/image2.png#lightbox)

Istnieje również możliwość ustawienia wielodotyku programowo, ustawiając `MultipleTouchEnabled` właściwości, jak pokazano w następującym wierszu kodu:

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

Aby określić, ile palców dotknięciu ekranu, użyj `Count` właściwość `UITouch` właściwości:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>Określanie lokalizacji Touch

Metoda `UITouch.LocationInView` zwraca obiekt CGPoint, który zawiera współrzędne touch w danym widoku. Ponadto można sprawdzać, aby zobaczyć, jeśli jest to lokalizacja w formancie, wywołując metodę `Frame.Contains`. Poniższy fragment kodu przedstawia przykład:

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

Teraz, gdy mamy zrozumienia touch zdarzeń w systemie iOS poznamy aparaty rozpoznawania gestów.

## <a name="gesture-recognizers"></a>Aparaty rozpoznawania gestów

Aparaty rozpoznawania gestów znacznie można uprościć i zredukować programistycznej do obsługi dotykowej w aplikacji. aparaty rozpoznawania gestów iOS zagregować szereg touch zdarzenia do zdarzenia jednym naciśnięciem przycisku.

Klasa zawiera Xamarin.iOS `UIGestureRecognizer` jako klasę podstawową dla następujących aparatów rozpoznawania gestów wbudowany:

-  *UITapGesturesRecognizer* — jest to na co najmniej jeden podsłuchu.
-  *UIPinchGestureRecognizer* — Pinching i rozmieszczania palców od siebie.
-  *UIPanGestureRecognizer* — przesuwania lub przeciągania.
-  *UISwipeGestureRecognizer* — szybko przesuwając w dowolnym kierunku.
-  *UIRotationGestureRecognizer* — obracanie dwoma palcami w ruchu zegara lub przeciwnie do ruchu wskazówek zegara.
-  *UILongPressGestureRecognizer* — naciśnij i przytrzymaj klawisz, czasami określane jako long naciśnij lub kliknij long.


Podstawowy wzorzec przy użyciu aparat rozpoznawania gestów wygląda następująco:

1.  **Utwórz wystąpienie aparat rozpoznawania gestów** — najpierw utworzyć wystąpienia `UIGestureRecognizer` podklasy. Zostanie skojarzony obiekt, który zostanie uruchomiony przez widok i zostaną odzyskiwanie zebrane, gdy widok jest usunięte. Nie jest konieczne do utworzenia tego widoku jako zmienną poziomu klasy.
1.  **Skonfiguruj ustawienia gestu** — następnym krokiem jest skonfigurowanie aparat rozpoznawania gestów. Dokumentacja firmy Xamarin na `UIGestureRecognizer` oraz w jej podklasach listę właściwości, które można ustawić do sterowania zachowaniem `UIGestureRecognizer` wystąpienia.
1.  **Skonfiguruj cel** — ze względu na jego dziedzictwa Objective-C, Xamarin.iOS nie wywoływanie zdarzeń, gdy aparat rozpoznawania gestów zgodna gestu.  `UIGestureRecognizer` Metoda — `AddTarget` — która może zaakceptować anonimowe delegata lub selektor języka Objective-C kod do wykonania, gdy aparat rozpoznawania gestów umożliwia dopasowanie.
1.  **Włącz aparat rozpoznawania gestów** — podobnie jak ze zdarzeniami touch gestów tylko są rozpoznawane przy włączonych interakcje touch.
1.  **Dodaj aparat rozpoznawania gestów do widoku** — ostatnim krokiem jest dodanie gestu widoku przez wywołanie metody `View.AddGestureRecognizer` i przekazanie jej przez obiekt aparat rozpoznawania gestów.

Zapoznaj się [przykłady aparat rozpoznawania gestów](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) Aby uzyskać więcej informacji na temat ich implementacji w kodzie.

Wywołanego gestu docelowy zostanie przekazana odwołanie do gestu, który wystąpił. Dzięki temu docelowy gestu uzyskać informacje o gestu, który wystąpił. Zakres dostępnych informacji zależy od typu aparat rozpoznawania gestów, który został użyty. Zobacz dokumentację platformy Xamarin dla informacji o dostępnych danych dla każdego `UIGestureRecognizer` podklasy.

Należy pamiętać, że po dodaniu widoku aparat rozpoznawania gestów widoku (i wszystkie widoki poniżej) nie będą otrzymywać zdarzeń touch. Aby umożliwić zdarzenia touch jednocześnie za pomocą gestów, `CancelsTouchesInView` właściwość musi być ustawiona na wartość false, jak pokazano w następującym kodem:

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

Każdy `UIGestureRecognizer` ma właściwość stanu, która zawiera ważne informacje o stanie aparat rozpoznawania gestów. Za każdym razem, gdy zmieni się wartość tej właściwości, z systemem iOS będzie wywoływać metodę subskrypcji nadanie mu aktualizacji. Jeśli aparat rozpoznawania gestów niestandardowych nigdy nie aktualizuje właściwości stanu, subskrybenta nigdy nie nosi nazwę, renderowania aparat rozpoznawania gestów bezużyteczny.

Gestów można podsumować jako jeden z dwóch typów:

1.  *Odrębny* — czas fire pierwszy tylko te gestów są rozpoznawane.
1.  *Ciągłe* — te gestów w dalszym ciągu wyzwalać tak długo, jak są rozpoznawane.


Aparaty rozpoznawania gestów występuje w jednym z następujących stanów:

-  *Możliwe* — jest to początkowy stan wszystkich aparatów rozpoznawania gestów. Jest to wartość domyślna właściwości stanu.
-  *Began* — po rozpoznaniu gestu ciągłego stan ma ustawioną wartość Began. Dzięki temu subskrybuje rozróżniania podczas uruchamiania rozpoznawania gestów oraz gdy zostanie zmieniona.
-  *Zmieniono* — po ciągłego gestu zostało uruchomione, ale nie została zakończona, stan zostanie ustawiona do zmienione każdorazowo touch przenosi lub zmiany, tak długo, jak jest nadal w oczekiwanym parametry gestu.
-  *Anulowane* — ten stan zostanie ustawiona, jeśli aparat rozpoznawania próby z Began zmienione, a następnie poprawek nie jest już zmienić w taki sposób, aby dopasować wzorzec gestu.
-  *Rozpoznany* — stan zostanie ustawiona, gdy aparat rozpoznawania gestów dopasowuje zestaw poprawek i poinformuje subskrybenta zakończył gestu.
-  *Zakończono* — jest to alias dla stanu Recognized.
-  *Niepowodzenie* — gdy aparat rozpoznawania gestów nie może dopasować poprawki nasłuchuje na stan zostanie zmieniony na niepowodzenie.


Xamarin.iOS reprezentuje tych wartości w `UIGestureRecognizerState` wyliczenia.

## <a name="working-with-multiple-gestures"></a>Praca z kilku gestów

Domyślnie dla systemu iOS nie zezwala na domyślne gestów jednoczesną. Zamiast tego każdy aparat rozpoznawania gestów otrzyma touch zdarzenia w kolejności deterministyczna. Poniższy fragment kodu przedstawia sposób wprowadzania aparat rozpoznawania gestów działać jednocześnie:

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

Istnieje również możliwość wyłączania gestu w systemie iOS. Istnieją dwie właściwości delegata, które umożliwiają aparat rozpoznawania gestów sprawdzić stan aplikacji i aktualności touch decyzji o tym, jak i jeśli gestu powinien był zostać rozpoznany. Są dwa zdarzenia:

1.  *ShouldReceiveTouch* — ten delegat jest wywoływana tuż przed aparat rozpoznawania gestów jest przekazywany zdarzeń touch i zapewnia możliwość badania poprawek i zdecydować, które poprawki będzie obsługiwany przez aparat rozpoznawania gestów.
1.  *ShouldBegin* — jest to, gdy aparat rozpoznawania próbuje zmienić stan z możliwych na inny stan. Zwrócenie wartości false spowoduje wymuszenie stan zostanie zmieniony na niepowodzenie aparat rozpoznawania gestów.


Można zastąpić te metody z silnie typizowaną `UIGestureRecognizerDelegate`, delegat słabych lub powiązania za pośrednictwem Składnia programu obsługi zdarzeń, jak pokazano na poniższy fragment kodu:

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

Ponadto istnieje możliwość kolejki aparat rozpoznawania gestów, dzięki czemu będzie powiedzie się tylko, jeśli inny aparat rozpoznawania gestów nie powiedzie się. Na przykład aparat rozpoznawania gestów pojedynczego wybrania powinien pomyślnie tylko, gdy naciśnij dwa razy aparat rozpoznawania gestów nie powiedzie się. Poniższy fragment kodu zawiera na przykład:

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>Tworzenie niestandardowych gestu

Mimo że iOS udostępnia pewne domyślne aparaty rozpoznawania gestów, może być konieczne utworzenie aparaty rozpoznawania gestów niestandardowych w niektórych przypadkach. Tworzenie aparat rozpoznawania gestów niestandardowych obejmuje następujące kroki:

1.  Podklasy `UIGestureRecognizer` .
1.  Zastępowanie metody touch odpowiednie zdarzenie.
1.  Bąbelkowy stanie uznania za pomocą właściwości stanu klasy podstawowej.


Praktyczne przykładem zostaną objęte [przy użyciu funkcji Touch w systemie iOS](ios-touch-walkthrough.md) wskazówki.
