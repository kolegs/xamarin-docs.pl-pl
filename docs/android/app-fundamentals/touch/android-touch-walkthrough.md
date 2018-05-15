---
title: Wskazówki — przy użyciu Touch w systemie Android
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/09/2018
ms.openlocfilehash: 625ba800ce498f80c0344c67e26bd79360de4002
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="walkthrough---using-touch-in-android"></a>Wskazówki — przy użyciu Touch w systemie Android

Daj nam poznać sposób użyć koncepcji z poprzedniej sekcji, w działającą aplikację. Aplikacja zostanie utworzona z czterech działań. Wykonywanie pierwszego działania będzie menu lub przełączania, która zostanie otwarta innych działań, aby zademonstrować różnych interfejsach API. Poniższy zrzut ekranu przedstawia działanie główne:

[![Zrzut ekranu z Touch mnie przycisku](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

Pierwsze działanie Touch próbki wyświetli sposób użycia procedury obsługi zdarzeń dla dotknięcie widoków. Aparat rozpoznawania gestów działania zostaną przedstawione jak podklasy `Android.View.Views` i obsługi zdarzeń także przedstawiają sposób obsługi gestów uszczypnięcia. Działanie trzeci i końcowych **gestów niestandardowych**, Pokaż jak użyje gestów niestandardowych. Aby ułatwić czynności do wykonania i przyjęcia, firma Microsoft będzie podzielić tego przewodnika sekcje z każdej sekcji koncentrujących się na jednym z działania.

## <a name="touch-sample-activity"></a>Przykładowe działanie Touch

-   Otwórz projekt **TouchWalkthrough\_Start**. **MainActivity** jest ustawione Przejdź &ndash; do nas do zaimplementowania zachowania touch w działaniu. Jeżeli możesz uruchomić aplikację i kliknąć pozycję **Touch próbki**, należy uruchomić następujące działania:

    [![Zrzut ekranu przedstawiający działania Touch rozpoczyna wyświetlane](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   Teraz, możemy potwierdzić, że działania uruchamiania, otwórz plik **TouchActivity.cs** i Dodaj program obsługi `Touch` zdarzenie `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   Następnie dodaj następującą metodę do **TouchActivity.cs**:

    ```csharp
    private void TouchMeImageViewOnTouch(object sender, View.TouchEventArgs touchEventArgs)
    {
        string message;
        switch (touchEventArgs.Event.Action & MotionEventActions.Mask)
        {
            case MotionEventActions.Down:
            case MotionEventActions.Move:
            message = "Touch Begins";
            break;

            case MotionEventActions.Up:
            message = "Touch Ends";
            break;

            default:
            message = string.Empty;
            break;
        }

        _touchInfoTextView.Text = message;
    }
    ```

Zwróć uwagę w powyższym kodzie, że traktujemy `Move` i `Down` akcji jako takie same. Wynika to z faktu, nawet jeśli użytkownik nie może podnieść ich palca `ImageView`, może przenosić lub zmiany nacisku przez użytkownika. Tego rodzaju zmiany spowoduje wygenerowanie `Move` akcji.

Po każdej aktualizacji poprawki użytkownika `ImageView`, `Touch` zostanie wygenerowany, zdarzeń i naszych obsługi wyświetli komunikat **Touch rozpoczyna się** na ekranie, jak pokazano na poniższym zrzucie ekranu:

[![Zrzut ekranu przedstawiający działania Touch rozpoczyna się](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

Jak długo użytkownik zachodzi `ImageView`, **Touch rozpoczyna się** będą wyświetlane w `TextView`. Gdy użytkownik jest już dotknięcie `ImageView`, wiadomość **Touch kończy się** będą wyświetlane w `TextView`, jak pokazano na poniższym zrzucie ekranu:

[![Zrzut ekranu przedstawiający działania Touch kończy się](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>Działanie aparat rozpoznawania gestów

Umożliwia teraz implementacji działania aparat rozpoznawania gestów. To działanie pokazują, jak przeciągnij widoku po ekranie i zilustrowania jeden sposób implementowania powiększanie gestem uszczypnięcia.

-   Dodaj nowe działanie do aplikacji o nazwie `GestureRecognizer`.
    Edytuj kod dla tego działania, aby podobny do następującego kodu:

    ```csharp
    public class GestureRecognizerActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            View v = new GestureRecognizerView(this);
            SetContentView(v);
        }
    }
    ```

-   Dodaj nowy Android wyświetlić do projektu i nadaj mu nazwę `GestureRecognizerView`. Do tej klasy, Dodaj następujące zmienne:

    ```csharp
    private static readonly int InvalidPointerId = -1;

    private readonly Drawable _icon;
    private readonly ScaleGestureDetector _scaleDetector;

    private int _activePointerId = InvalidPointerId;
    private float _lastTouchX;
    private float _lastTouchY;
    private float _posX;
    private float _posY;
    private float _scaleFactor = 1.0f;
    ```

-   Dodaj następujący Konstruktor do `GestureRecognizerView`. Ten konstruktor doda `ImageView` nasze działania. W tym momencie nadal nie kompilacji kodu &ndash; należy utworzyć klasę `MyScaleListener` ułatwiające z zmiana rozmiaru `ImageView` gdy użytkownik pinches go:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   Rysowanie obrazu na naszych działań, należy zastąpić `OnDraw` metody klasy widoku, jak pokazano w poniższy fragment kodu. Ten kod zostanie przesunięty `ImageView` określonej przez pozycji `_posX` i `_posY` również jako Zmień rozmiar obrazu zgodnie z czynnik skalowania:

    ```csharp
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        canvas.Save();
        canvas.Translate(_posX, _posY);
        canvas.Scale(_scaleFactor, _scaleFactor);
        _icon.Draw(canvas);
        canvas.Restore();
    }
    ```

-   Następnie należy zaktualizować zmienna wystąpienia `_scaleFactor` jako użytkownik pinches `ImageView`. Teraz dodamy klasy o nazwie `MyScaleListener`. Ta klasa będzie nasłuchiwać zdarzeń skalowania, które będą wywoływane przez system Android, gdy użytkownik pinches `ImageView`.
    Dodaj następujące klasy wewnętrznej do `GestureRecognizerView`. Ta klasa jest `ScaleGesture.SimpleOnScaleGestureListener`. Ta klasa jest klasą wygody czy odbiorniki myślisz podzbiór gesty, można podklasy:

    ```csharp
    private class MyScaleListener : ScaleGestureDetector.SimpleOnScaleGestureListener
    {
        private readonly GestureRecognizerView _view;

        public MyScaleListener(GestureRecognizerView view)
        {
            _view = view;
        }

        public override bool OnScale(ScaleGestureDetector detector)
        {
            _view._scaleFactor *= detector.ScaleFactor;

            // put a limit on how small or big the image can get.
            if (_view._scaleFactor > 5.0f)
            {
                _view._scaleFactor = 5.0f;
            }
            if (_view._scaleFactor < 0.1f)
            {
                _view._scaleFactor = 0.1f;
            }

            _iconview.Invalidate();
            return true;
        }
    }
    ```

-   Next — metoda, należy zastąpić w `GestureRecognizerView` jest `OnTouchEvent`. Poniższy kod wyświetla pełne implementacja tej metody. Istnieje wiele kodu, więc umożliwia zabrać kilka minut i sprawdź, co się dzieje w tym miejscu. Najpierw jest ta metoda jest skalowanie ikony, w razie potrzeby &ndash; jest to obsługiwane przez wywołanie metody `_scaleDetector.OnTouchEvent`. Następnie spróbujemy dowiedzieć się, jakie działania wywołać tej metody:

    - Jeśli użytkownik dotknięciu ekranu, firma Microsoft rejestrowania pozycji X i Y i identyfikator pierwszego wskaźnik dotknięciu ekranu.

    - Jeśli użytkownik przenieść ich touch na ekranie, następnie możemy dowiedzieć się, jak daleko użytkownik przesunął kursor.

    - Jeśli użytkownik ma unosiło jego palca odniosło, następnie firma Microsoft będzie Zatrzymaj śledzenie gestów.

    ```csharp
    public override bool OnTouchEvent(MotionEvent ev)
    {
        _scaleDetector.OnTouchEvent(ev);

        MotionEventActions action = ev.Action & MotionEventActions.Mask;
        int pointerIndex;

        switch (action)
        {
            case MotionEventActions.Down:
            _lastTouchX = ev.GetX();
            _lastTouchY = ev.GetY();
            _activePointerId = ev.GetPointerId(0);
            break;

            case MotionEventActions.Move:
            pointerIndex = ev.FindPointerIndex(_activePointerId);
            float x = ev.GetX(pointerIndex);
            float y = ev.GetY(pointerIndex);
            if (!_scaleDetector.IsInProgress)
            {
                // Only move the ScaleGestureDetector isn't already processing a gesture.
                float deltaX = x - _lastTouchX;
                float deltaY = y - _lastTouchY;
                _posX += deltaX;
                _posY += deltaY;
                Invalidate();
            }

            _lastTouchX = x;
            _lastTouchY = y;
            break;

            case MotionEventActions.Up:
            case MotionEventActions.Cancel:
            // We no longer need to keep track of the active pointer.
            _activePointerId = InvalidPointerId;
            break;

            case MotionEventActions.PointerUp:
            // check to make sure that the pointer that went up is for the gesture we're tracking.
            pointerIndex = (int) (ev.Action & MotionEventActions.PointerIndexMask) >> (int) MotionEventActions.PointerIndexShift;
            int pointerId = ev.GetPointerId(pointerIndex);
            if (pointerId == _activePointerId)
            {
                // This was our active pointer going up. Choose a new
                // action pointer and adjust accordingly
                int newPointerIndex = pointerIndex == 0 ? 1 : 0;
                _lastTouchX = ev.GetX(newPointerIndex);
                _lastTouchY = ev.GetY(newPointerIndex);
                _activePointerId = ev.GetPointerId(newPointerIndex);
            }
            break;

        }
        return true;
    }
    ```

-   Teraz uruchom aplikację i uruchomienia działania aparat rozpoznawania gestów.
    Podczas uruchamiania ekranu powinien wyglądać jak na poniższym zrzucie ekranu:

    [![Aparat rozpoznawania gestów ekranie startowym ikoną systemu Android](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   Teraz touch ikonę, a następnie przeciągnij ją po ekranie. Spróbuj gestu powiększanie gestem uszczypnięcia. W pewnym momencie ekranu może wyglądać jak na poniższym zrzucie ekranu:

    [![Ikona przenoszenia gestów wokół ekranu](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

Na tym etapie należy nadać samodzielnie jest element pat na tylnej: powiększanie gestem uszczypnięcia właśnie zostało zaimplementowane w aplikacji systemu Android! Pobrać podział szybki i pozwala przejść do działania trzeci i końcowe w ramach tego przewodnika &ndash; za pomocą gestów niestandardowych.

## <a name="custom-gesture-activity"></a>Działanie gestów niestandardowych

W tym przewodniku na ekranie końcowym będzie używać gestów niestandardowych.

Na potrzeby tego przewodnika biblioteki gestów został już utworzony za pomocą gestu narzędzia i dodane do projektu w pliku **zasobów/raw/gestów**. Z tego bit housekeeping przeszkadza pozwala uzyskać na z ostatnie działanie w tym przewodnikiem.

-   Dodaj plik układu o nazwie **niestandardowych\_gestu\_layout.axml** do projektu z następującą zawartość. Projekt posiada już wszystkie obrazy **zasobów** folderu:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
        <ImageView
            android:src="@drawable/check_me"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="3"
            android:id="@+id/imageView1"
            android:layout_gravity="center_vertical" />
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
    </LinearLayout>
    ```

-   Następnie dodaj nowe działanie do projektu i nadaj mu nazwę `CustomGestureRecognizerActivity.cs`. Dodaj dwie zmienne wystąpienia klasy, jako przedstawiający w następujących dwóch wierszy kodu:

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   Edytuj `OnCreate` metoda tego działania, tak że podobny do następującego kodu. Umożliwia zająć kilka minut, aby wyjaśnić, co dzieje się w tym kodzie. Najpierw czynności jest utworzenie wystąpienia `GestureOverlayView` i ustawić go jako widok główny działania.
    Możemy również przypisać do obsługi zdarzeń `GesturePerformed` zdarzenie `GestureOverlayView`. Następnie możemy zwiększyć pliku układu, który został utworzony wcześniej i dodać go jako widok podrzędny `GestureOverlayView`. Ostatnim krokiem jest można zainicjować zmiennej `_gestureLibrary` i załadować plik gestów z zasobów aplikacji. Jeśli z jakiegoś powodu nie można załadować pliku gesty, nie ma znacznie, które można wykonać tego działania, tak aby zawierała zamknięcia:

    ```csharp
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
        SetContentView(gestureOverlayView);
        gestureOverlayView.GesturePerformed += GestureOverlayViewOnGesturePerformed;

        View view = LayoutInflater.Inflate(Resource.Layout.custom_gesture_layout, null);
        _imageView = view.FindViewById<ImageView>(Resource.Id.imageView1);
        gestureOverlayView.AddView(view);

        _gestureLibrary = GestureLibraries.FromRawResource(this, Resource.Raw.gestures);
        if (!_gestureLibrary.Load())
        {
            Log.Wtf(GetType().FullName, "There was a problem loading the gesture library.");
            Finish();
        }
    }
    ```

-   Końcowy element, należy zaimplementować metodę `GestureOverlayViewOnGesturePerformed` pokazane na poniższy fragment kodu. Gdy `GestureOverlayView` wykryciu gestu, wywołuje zwrotnie do tej metody. Pierwszą rzeczą, którą próbujemy pobrać `IList<Prediction>` obiekty, które odpowiadają gestu przez wywołanie metody `_gestureLibrary.Recognize()`. Korzystamy z bitowego LINQ można pobrać `Prediction` ma najwyższy wynik dla gestu.

    Nie ma pasującego gestu z wysokim za mało wynik, a następnie program obsługi zdarzeń kończy działanie bez żadnego działania. W przeciwnym razie możemy Sprawdź nazwę Prognozowanie i zmienić obraz jest wyświetlany na podstawie nazwy gestu:

    ```csharp
    private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
    {
        IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
        orderby p.Score descending
        where p.Score > 1.0
        select p;
        Prediction prediction = predictions.FirstOrDefault();

        if (prediction == null)
        {
            Log.Debug(GetType().FullName, "Nothing seemed to match the user's gesture, so don't do anything.");
            return;
        }

        Log.Debug(GetType().FullName, "Using the prediction named {0} with a score of {1}.", prediction.Name, prediction.Score);

        if (prediction.Name.StartsWith("checkmark"))
        {
            _imageView.SetImageResource(Resource.Drawable.checked_me);
        }
        else if (prediction.Name.StartsWith("erase", StringComparison.OrdinalIgnoreCase))
        {
            // Match one of our "erase" gestures
            _imageView.SetImageResource(Resource.Drawable.check_me);
        }
    }
    ```

-   Uruchom aplikację i uruchomienia działania aparat rozpoznawania gestów niestandardowych. Powinien on wyglądać podobnie jak poniższy zrzut ekranu:

    [![Zrzut ekranu z Sprawdź mnie obrazu](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    Teraz narysuj wyboru na ekranie i mapy bitowej będzie wyświetlany powinien wyglądać jak wyświetlanego w następnym zrzuty ekranu:

    [![Rozpoznano narysowanego znacznik wyboru, znacznik wyboru](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    Na koniec Rysuj bazgrołów na ekranie. Pole wyboru należy zmieniać wstecz do oryginalnego obrazu, jak pokazano w tych zrzuty ekranu:

    [![Zostanie wyświetlony bazgrołów na ekranie oryginalnego obrazu](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Masz teraz zrozumienia sposobu integracji touch i gestów w aplikacji systemu Android przy użyciu platformy Xamarin.Android.


## <a name="related-links"></a>Linki pokrewne

- [Android Touch Start (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch końcowego (na przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
