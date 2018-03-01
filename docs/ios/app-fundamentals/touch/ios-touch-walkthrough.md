---
title: "Wskazówki — Touch korzystanie w systemie iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 848db0af436ad43e07e68de4d278f641ab83136d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough--using-touch-in-ios"></a>Wskazówki — Touch korzystanie w systemie iOS

W tym przewodniku pokazano, jak napisać kod, który odpowiada na różnych rodzajów zdarzeń touch. Każdy przykład jest zawarta w oddzielnych ekranu:

- [Przykłady Touch](#Touch_Samples) — odpowiadanie na zdarzenia touch.
- [Przykłady aparat rozpoznawania gestów](#Gesture_Recognizer_Samples) — jak używać aparatów rozpoznawania gestów wbudowanych.
- [Przykładowe aparat rozpoznawania gestów niestandardowych](#Custom_Gesture_Recognizer) — sposób tworzenia aparat rozpoznawania gestów niestandardowych.

Każda sekcja zawiera instrukcje dotyczące pisania kodu od początku.
[Uruchamianie przykładowy kod](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) zawiera już ekran ukończenia scenorysu i menu:

 [ ![](ios-touch-walkthrough-images/image3.png "Przykład obejmuje ekranu menu")](ios-touch-walkthrough-images/image3.png)

Postępuj zgodnie z instrukcjami poniżej, aby dodać kod do scenorysu, a informacje o różnych typach zdarzeń touch, które są dostępne w systemie iOS. Można również otworzyć [gotowej próbki](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final) Aby wyświetlić wszystkie elementy pracy.

## <a name="touch-samples"></a>Touch — przykłady

W tym przykładzie przedstawiono niektóre touch interfejsów API. Wykonaj następujące kroki, aby dodać kod wymagany do zaimplementowania touch zdarzenia:


1. Otwórz projekt **Touch_Start**. Najpierw uruchom projekt upewnij się, że wszystko jest poprawny i touch **przykłady Touch** przycisku. (Mimo że przycisków zostanie nie działają) powinien zostać wyświetlony ekran podobny do następującego:
    
    [![](ios-touch-walkthrough-images/image4.png "Przykładowa aplikacja Uruchom wolnego przycisków")](ios-touch-walkthrough-images/image4.png)


1. Przeprowadź edycję pliku **TouchViewController.cs** i dodaj następujące zmienne dwa wystąpienia klasy `TouchViewController`:

    ```csharp 
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```


1. Implementowanie `TouchesBegan` metody, jak pokazano w poniższym kodzie:

    ```csharp 
    public override void TouchesBegan(NSSet touches, UIEvent evt)
    {
        base.TouchesBegan(touches, evt);
    
        // If Multitouch is enabled, report the number of fingers down
        TouchStatus.Text = string.Format ("Number of fingers {0}", touches.Count);
    
        // Get the current touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            // Check to see if any of the images have been touched
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Fist image touched
                TouchImage.Image = UIImage.FromBundle("TouchMe_Touched.png");
                TouchStatus.Text = "Touches Began";
            } else if (touch.TapCount == 2 && DoubleTouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Second image double-tapped, toggle bitmap
                if (imageHighlighted)
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
                    TouchStatus.Text = "Double-Tapped Off";
                }
                else
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
                    TouchStatus.Text = "Double-Tapped On";
                }
                imageHighlighted = !imageHighlighted;
            } else if (DragImage.Frame.Contains(touch.LocationInView(View)))
            {
                // Third image touched, prepare to drag
                touchStartedInside = true;
            }
        }
    }
    ```
    Ta metoda działa przez wyszukiwanie `UITouch` obiektu, a jeśli istnieje wykonanie akcji oparte na którym wystąpił touch:

    * _Wewnątrz TouchImage_ — Wyświetl tekst `Touches Began` etykiety i zmiany obrazu.
    * _Wewnątrz DoubleTouchImage_ — zmienić obraz wyświetlany, jeśli gestu dwukrotnym naciśnięciu.
    * _Wewnątrz DragImage_ — Ustaw flagę wskazującą, czy touch została uruchomiona. Metoda `TouchesMoved` użyje do określenia, czy ta flaga `DragImage` powinna zostać przeniesiona po ekranie, lub nie, jak firma Microsoft przestrzega w następnym kroku.


    Powyższy kod tylko dotyczy poszczególnych poprawek, brak nadal zachowania w przypadku użytkownika jest przenoszenia ich palca na ekranie. Aby odpowiedzieć ruchu, należy zaimplementować `TouchesMoved` jak pokazano w poniższym kodzie:

    ```csharp 
    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchStatus.Text = "Touches Moved";
            }

            //==== IMAGE DRAG
            // check to see if the touch started in the drag me image
            if (touchStartedInside)
            {
                // move the shape
                float offsetX = touch.PreviousLocationInView(View).X - touch.LocationInView(View).X;
                float offsetY = touch.PreviousLocationInView(View).Y - touch.LocationInView(View).Y;
                DragImage.Frame = new RectangleF(new PointF(DragImage.Frame.X - offsetX, DragImage.Frame.Y - offsetY), DragImage.Frame.Size);
            }
        }
    }
    ```

    Ta metoda pobiera `UITouch` obiektu, a następnie sprawdza, gdzie wystąpił touch. Jeśli podczas touch `TouchImage`, następnie tekst przenieść poprawki jest wyświetlany na ekranie. 

    Jeśli `touchStartedInside` ma wartość true, a następnie wiemy, że użytkownik ma ich palca na `DragImage` i jest przenoszenia go. Kod `DragImage` gdy użytkownik przechodzi ich palcem po ekranie.

1. Musimy obsługi w przypadku, gdy użytkownik podnosi własnego palca odniosło lub iOS anuluje zdarzenie touch. W tym celu wprowadzimy `TouchesEnded` i `TouchesCancelled` w sposób przedstawiony poniżej:

    ```csharp
    public override void TouchesCancelled(NSSet touches, UIEvent evt)
    {
        base.TouchesCancelled(touches, evt);
    
        // reset our tracking flags
        touchStartedInside = false;
        TouchImage.Image = UIImage.FromBundle("TouchMe.png");
        TouchStatus.Text = "";
    }
    
    public override void TouchesEnded(NSSet touches, UIEvent evt)
    {
        base.TouchesEnded(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchImage.Image = UIImage.FromBundle("TouchMe.png");
                TouchStatus.Text = "Touches Ended";
            }
        }
        // reset our tracking flags
        touchStartedInside = false;
    }
    ```
    
    Obie te metody spowoduje zresetowanie `touchStartedInside` flagi na wartość false. `TouchesEnded` będzie również wyświetlana `TouchesEnded` na ekranie.

1. W tym momencie ekran dotykowy przykłady zostało zakończone. Zwróć uwagę, jak ekranu zmiany interakcję z każdym obrazów, jak pokazano na poniższym zrzucie ekranu:
        
    [![](ios-touch-walkthrough-images/image4.png "Początkowy ekranu aplikacji")](ios-touch-walkthrough-images/image4.png)
    
    [![](ios-touch-walkthrough-images/image5.png "Ekran po użytkownik przeciąga element button")](ios-touch-walkthrough-images/image5.png)
 

<a name="Gesture_Recognizer_Samples" />

##  <a name="gesture-recognizer-samples"></a>Aparat rozpoznawania gestów — przykłady

[Poprzedniej sekcji](#Touch_Samples) przedstawiono sposób przeciągania obiektu po ekranie przy użyciu zdarzenia touch.
W tej sekcji możemy będzie pozbyć się zdarzenia touch i pokazują, jak używać następujących aparaty rozpoznawania gestów:

-  `UIPanGestureRecognizer` Przeciągnąć obraz wokół ekranu.
-  `UITapGestureRecognizer` Odpowiedzieć na dwa razy podsłuchu na ekranie.

Po uruchomieniu [uruchamianie przykładowy kod](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) i wybierz polecenie **przykłady aparat rozpoznawania gestów** przycisku, powinien zostać wyświetlony następujący ekran:

 [ ![](ios-touch-walkthrough-images/image6.png "Kliknięcie przycisku przykłady aparat rozpoznawania gestów wyświetla ten ekran")](ios-touch-walkthrough-images/image6.png)

Wykonaj następujące kroki, aby zaimplementować aparaty rozpoznawania gestów:


1. Przeprowadź edycję pliku **GestureViewController.cs** i dodaj następującą zmienną wystąpienie:

    ```chsarp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    Potrzebujemy ta zmienna wystąpienia, aby śledzić poprzedniej lokalizacji obrazu.
Aparat rozpoznawania gestów przesuwanie użyje `originalImageFrame` wartość do obliczenia przesunięcia wymagane ponowne obrazu na ekranie.

1. Dodaj następującą metodę do kontrolera:

    ```chsarp
    private void WireUpDragGestureRecognizer()
    {
        // Create a new tap gesture
        UIPanGestureRecognizer gesture = new UIPanGestureRecognizer();
    
        // Wire up the event handler (have to use a selector)
        gesture.AddTarget(() => HandleDrag(gesture));  // to be defined
    
        // Add the gesture recognizer to the view
        DragImage.AddGestureRecognizer(gesture);
    }
    ```

    Ten kod tworzy `UIPanGestureRecognizer` wystąpienia i dodaje go do widoku.
Zwróć uwagę, możemy przypisać element docelowy do gestu w formularzu metodę `HandleDrag` — ta metoda jest dostępne w następnym kroku.

1. Aby zaimplementować HandleDrag, Dodaj następujący kod do kontrolera:

    ```chsarp
    private void HandleDrag(UIPanGestureRecognizer recognizer)
    {
        // If it's just began, cache the location of the image
        if (recognizer.State == UIGestureRecognizerState.Began)
        {
            originalImageFrame = DragImage.Frame;
        }
    
        // Move the image if the gesture is valid
        if (recognizer.State != (UIGestureRecognizerState.Cancelled | UIGestureRecognizerState.Failed
            | UIGestureRecognizerState.Possible))
        {
            // Move the image by adding the offset to the object's frame
            PointF offset = recognizer.TranslationInView(DragImage);
            RectangleF newFrame = originalImageFrame;
            newFrame.Offset(offset.X, offset.Y);
            DragImage.Frame = newFrame;
        }
    }
    ```

    Powyższy kod zostanie najpierw sprawdź stan aparat rozpoznawania gestów, a następnie przenieść obraz wokół ekranu. Ten kod w miejscu kontrolera można teraz obsługuje przeciąganie jeden obraz po ekranie.


1. Dodaj `UITapGestureRecognizer` zmieni to obraz jest wyświetlany w DoubleTouchImage. Dodaj następującą metodę do `GestureViewController` kontrolera:

    ```chsarp
    private void WireUpTapGestureRecognizer()
    {
        // Create a new tap gesture
        UITapGestureRecognizer tapGesture = null;
    
        // Report touch
        Action action = () => {
            TouchStatus.Text = string.Format("Image touched at: {0}",tapGesture.LocationOfTouch(0, DoubleTouchImage));
    
            // Toggle the image
            if (imageHighlighted)
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
            }
            else
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
            }
            imageHighlighted = !imageHighlighted;
        };
    
        tapGesture = new UITapGestureRecognizer(action);
    
        // Configure it
        tapGesture.NumberOfTapsRequired = 2;
    
        // Add the gesture recognizer to the view
        DoubleTouchImage.AddGestureRecognizer(tapGesture);
    }
    ```

    Ten kod jest bardzo podobny do kodu `UIPanGestureRecognizer` , ale zamiast delegat dla obiektu docelowego używamy `Action`. 

1. Element końcowego, konieczne jest zmodyfikowanie `ViewDidLoad` tak, aby metody właśnie dodaliśmy. Zmień ViewDidLoad, podobny do następującego kodu:

    ```chsarp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        Title = "Gesture Recognizers";
    
        // Save initial state
        originalImageFrame = DragImage.Frame;
    
        WireUpTapGestureRecognizer();
        WireUpDragGestureRecognizer();
    }
    ```

    Również zauważyć, że możemy zainicjować wartość `originalImageFrame`.


1. Uruchom aplikację i interakcję z dwóch obrazów.
Poniższy zrzut ekranu jest jednym z przykładów tych interakcji:
    
    [![](ios-touch-walkthrough-images/image7.png "Ten zrzut ekranu przedstawia interakcje przeciągania")](ios-touch-walkthrough-images/image7.png)



## <a name="custom-gesture-recognizer"></a>Aparat rozpoznawania gestów niestandardowych

W tej sekcji zostaną zastosowane pojęć związanych z poprzednich sekcjach, aby utworzyć aparat rozpoznawania gestów niestandardowych. Aparat rozpoznawania gestów niestandardowych będzie podklasy `UIGestureRecognizer`i będą rozpoznawać, gdy użytkownik wprowadzi "V" na ekranie, a następnie przełącz mapy bitowej. Poniższy zrzut ekranu znajduje się przykład tego ekranu:

 [ ![](ios-touch-walkthrough-images/image8.png "Aplikacja rozpozna, gdy użytkownik wprowadzi V na ekranie")](ios-touch-walkthrough-images/image8.png)

Wykonaj następujące kroki, aby utworzyć aparat rozpoznawania gestów niestandardowych:


1. Dodaj nową klasę do projektu o nazwie `CheckmarkGestureRecognizer`i przydzielić mu wyglądać podobnie do następującego kodu:

    ```chsarp
    using System;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace Touch
    {
        public class CheckmarkGestureRecognizer : UIGestureRecognizer
        {
            #region Private Variables
            private CGPoint midpoint = CGPoint.Empty;
            private bool strokeUp = false;
            #endregion
    
            #region Override Methods
            /// <summary>
            ///   Called when the touches end or the recognizer state fails
            /// </summary>
            public override void Reset()
            {
                base.Reset();
    
                strokeUp = false;
                midpoint = CGPoint.Empty;
            }
    
            /// <summary>
            ///   Is called when the fingers touch the screen.
            /// </summary>
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                // we want one and only one finger
                if (touches.Count != 1)
                {
                    base.State = UIGestureRecognizerState.Failed;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the touches are cancelled due to a phone call, etc.
            /// </summary>
            public override void TouchesCancelled(NSSet touches, UIEvent evt)
            {
                base.TouchesCancelled(touches, evt);
                // we fail the recognizer so that there isn't unexpected behavior
                // if the application comes back into view
                base.State = UIGestureRecognizerState.Failed;
            }
    
            /// <summary>
            ///   Called when the fingers lift off the screen
            /// </summary>
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                //
                if (base.State == UIGestureRecognizerState.Possible && strokeUp)
                {
                    base.State = UIGestureRecognizerState.Recognized;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the fingers move
            /// </summary>
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                // if we haven't already failed
                if (base.State != UIGestureRecognizerState.Failed)
                {
                    // get the current and previous touch point
                    CGPoint newPoint = (touches.AnyObject as UITouch).LocationInView(View);
                    CGPoint previousPoint = (touches.AnyObject as UITouch).PreviousLocationInView(View);
    
                    // if we're not already on the upstroke
                    if (!strokeUp)
                    {
                        // if we're moving down, just continue to set the midpoint at
                        // whatever point we're at. when we start to stroke up, it'll stick
                        // as the last point before we upticked
                        if (newPoint.X >= previousPoint.X && newPoint.Y >= previousPoint.Y)
                        {
                            midpoint = newPoint;
                        }
                        // if we're stroking up (moving right x and up y [y axis is flipped])
                        else if (newPoint.X >= previousPoint.X && newPoint.Y <= previousPoint.Y)
                        {
                            strokeUp = true;
                        }
                        // otherwise, we fail the recognizer
                        else
                        {
                            base.State = UIGestureRecognizerState.Failed;
                        }
                    }
                }
    
                Console.WriteLine(base.State.ToString());
            }
            #endregion
        }
    }
    ```

    Metoda Reset jest wywoływane, gdy `State` właściwości zmienia się na `Recognized` lub `Ended`. Jest to czas do resetowania w aparat rozpoznawania gestów niestandardowych stanów wewnętrznych.
Klasa można teraz rozpocząć od nowa pracę następnym razem, gdy użytkownik wchodzi w interakcję z aplikacją i będzie gotowa ponownie spróbować rozpoznawania gestów.



1. Teraz, gdy firma Microsoft zdefiniowany przez aparat rozpoznawania gestów niestandardowych (`CheckmarkGestureRecognizer`) Edytuj **CustomGestureViewController.cs** i dodaj następujące zmienne dwa wystąpienia:

    ```chsarp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. Aby utworzyć wystąpienia i skonfigurować naszych aparat rozpoznawania gestów, dodaj następującą metodę do kontrolera:

    ```chsarp
    private void WireUpCheckmarkGestureRecognizer()
    {
        // Create the recognizer
        checkmarkGesture = new CheckmarkGestureRecognizer();
    
        // Wire up the event handler
        checkmarkGesture.AddTarget(() => {
            if (checkmarkGesture.State == (UIGestureRecognizerState.Recognized | UIGestureRecognizerState.Ended))
            {
                if (isChecked)
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Unchecked.png");
                }
                else
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Checked.png");
                }
                isChecked = !isChecked;
            }
        });
    
        // Add the gesture recognizer to the view
        View.AddGestureRecognizer(checkmarkGesture);
    }
    ```

1. Edytuj `ViewDidLoad` tak, aby `WireUpCheckmarkGestureRecognizer`, jak pokazano w poniższy fragment kodu:

    ```chsarp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. Uruchom aplikację i spróbuj rysowania "V" na ekranie. Powinien pojawić się wyświetlanego obrazu zmiany, jak pokazano na poniższych zrzutach ekranu:
    
    [![](ios-touch-walkthrough-images/image9.png "Przycisk zaznaczone")](ios-touch-walkthrough-images/image9.png)
    
    [![](ios-touch-walkthrough-images/image10.png "Przycisk unchecked")](ios-touch-walkthrough-images/image10.png)



Powyższe trzy sekcje wykazały różne sposoby odpowiadanie na zdarzenia w systemie iOS touch: w przypadku używania zdarzeń touch, aparaty rozpoznawania gestów wbudowanych, lub za pomocą aparat rozpoznawania gestów niestandardowych.



## <a name="related-links"></a>Linki pokrewne

- [iOS Touch Start (przykład)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS Touch końcowego (na przykład)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
