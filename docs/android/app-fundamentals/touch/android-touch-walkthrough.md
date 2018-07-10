---
title: 'Przewodnik: używanie dotyku w systemie Android'
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/09/2018
ms.openlocfilehash: d379630e3b7fa2b42bd9530e1dccd75e9634dd2f
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935530"
---
# <a name="walkthrough---using-touch-in-android"></a>Przewodnik: używanie dotyku w systemie Android

Daj nam dowiedzieć się, jak używać pojęcia z poprzedniej sekcji, w działającej aplikacji. Aplikacja zostanie utworzona przy użyciu czterech działań. Pierwsze działanie będzie menu lub przełączania, która zostanie otwarta innych działań, aby przedstawić różne interfejsy API. Poniższy zrzut ekranu przedstawia głównego działania:

[![Zrzut ekranu z Touch mnie przycisku](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

Pierwsze działanie Touch próbki, pokazują, jak na potrzeby ingerowania w widokach procedury obsługi zdarzeń. Działanie aparat rozpoznawania gestów pokażemy, jak podklasy `Android.View.Views` i obsługiwać zdarzenia także przedstawiają sposób obsługi gestów uszczypnięcia. Działanie trzeciej i ostatniej **gestów niestandardowych**, Pokaż jak użyje gestów niestandardowych. Aby ułatwić czynności należy wykonać i Obsługuj, firma Microsoft będzie Podziel tego przewodnika na sekcje, z każdą sekcję, skupiając się na działalność.

## <a name="touch-sample-activity"></a>Przykładowe działanie w ogóle

-   Otwórz projekt **TouchWalkthrough\_Start**. **MainActivity** to zestaw wszystkich przejść &ndash; zależy nam Implementowanie zachowania touch w działaniu. Jeśli możesz uruchomić aplikację i kliknąć pozycję **Touch przykładowe**, należy uruchomić następujące działania:

    [![Zrzut ekranu przedstawiający działanie z Touch rozpoczyna się wyświetlane](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   Teraz, gdy potwierdzamy, że działanie uruchamiania, otwórz plik **TouchActivity.cs** i Dodaj program obsługi `Touch` zdarzenia `ImageView`:

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

Należy zauważyć w powyższym kodzie, że traktujemy `Move` i `Down` akcja, taka sama. To dlatego, nawet jeśli użytkownik nie może przenoszenie ich finger `ImageView`, może poruszać lub nacisku przez użytkownika może ulec zmianie. Tego rodzaju zmiany spowoduje wygenerowanie `Move` akcji.

Każdym ma użytkownika `ImageView`, `Touch` zdarzenia zostaną podniesione i naszej obsługi wyświetli komunikat o **Touch rozpoczyna się** na ekranie, jak pokazano na poniższym zrzucie ekranu:

[![Zrzut ekranu przedstawiający działanie z Touch rozpoczyna się](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

Tak długo, jak użytkownik zachodzi `ImageView`, **Touch rozpoczyna się** będą wyświetlane w `TextView`. Gdy użytkownik jest już dotknięcie `ImageView`, komunikat **Touch kończy się** będą wyświetlane w `TextView`, jak pokazano na poniższym zrzucie ekranu:

[![Zrzut ekranu przedstawiający działanie z Touch kończy się](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>Działanie aparat rozpoznawania gestów

Teraz umożliwia Implementowanie działania aparat rozpoznawania gestów. To działanie pokażemy, jak przeciągać wyświetlanie na ekranie i pokazują jeden ze sposobów zaimplementowania powiększanie gestem uszczypnięcia.

-   Dodaj nowe działanie do aplikacji o nazwie `GestureRecognizer`.
    Edytowanie kodu dla tego działania, tak aby wyglądała jak poniższy kod:

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

-   Dodawanie nowych Android wyświetlić do projektu i nadaj mu nazwę `GestureRecognizerView`. Dodaj następujące zmienne do klasy:

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

-   Dodaj następującego konstruktora do `GestureRecognizerView`. Spowoduje to dodanie tego konstruktora `ImageView` nasze działania. W tym momencie kod nadal będzie niemożliwa &ndash; musimy utworzyć klasę `MyScaleListener` to pomoże ustalić o zmienionych rozmiarach `ImageView` po użytkownik pinches go:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   Rysowanie obrazu na nasze działania, należy zastąpić `OnDraw` metody klasy widoku, jak pokazano w poniższym fragmencie kodu. Ten kod zostanie przesunięty `ImageView` w położeniu wskazanym przez `_posX` i `_posY` również jako zmianę rozmiaru obrazu zgodnie z współczynnik skalowania:

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

-   Następnie należy zaktualizować zmienną instance `_scaleFactor` jako użytkownik pinches `ImageView`. Teraz dodamy klasę o nazwie `MyScaleListener`. Ta klasa będzie nasłuchiwać zdarzeń skalowania, które będą wywoływane przez system Android, gdy użytkownik pinches `ImageView`.
    Dodaj poniższą klasę wewnętrzny, aby `GestureRecognizerView`. Ta klasa jest `ScaleGesture.SimpleOnScaleGestureListener`. Ta klasa jest klasą wygody, czy odbiorniki jesteś zainteresowany podzbiór gestów, można podklasy:

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

            _view.Invalidate();
            return true;
        }
    }
    ```

-   Następna metoda, należy zastąpić w `GestureRecognizerView` jest `OnTouchEvent`. Poniższy kod zawiera pełną implementację tej metody. Istnieje duża ilość kodu, więc umożliwia chwilę potrwać, i sprawdź, co się dzieje w tym miejscu. Pierwszą rzeczą, ta metoda jest skalowanie ikony, jeśli to konieczne &ndash; odbywa się to przez wywołanie metody `_scaleDetector.OnTouchEvent`. Następnie podejmowane są próby zorientować się, jakie działania wywołuje tę metodę:

    - Jeśli użytkownik korzysta z ekranu, firma Microsoft Zarejestruj pozycji X i Y oraz identyfikator pierwszego wskaźnika, który korzysta z ekranu.

    - Jeśli użytkownik przeniesione ich touch na ekranie, następnie możemy Ustal, jak daleko użytkownik przesunął kursor.

    - Jeśli użytkownik został podniesiony jego finger mieściły się na ekranie, następnie przestaniemy śledzenia gestów.

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

-   Teraz uruchom aplikację i uruchomić działania aparat rozpoznawania gestów.
    Po uruchomieniu ekran powinien wyglądać podobnie jak na poniższym zrzucie ekranu:

    [![Aparat rozpoznawania gestów ekran startowy ikoną dla systemu Android](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   Teraz touch ikony i przeciągnij go na ekranie. Spróbuj gestu powiększanie gestem uszczypnięcia. W pewnym momencie ekranu może wyglądać jak na poniższym zrzucie ekranu:

    [![Ikona przenoszenia gestów na ekranie](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

W tym momencie należy nadać kontu samodzielnie osobisty token dostępu z tyłu: powiększanie gestem uszczypnięcia właśnie zostało zaimplementowane w aplikacji systemu Android! Zrobić sobie przerwę szybki i pozwala przejść do trzeciej i ostatniej aktywności w tym przewodniku &ndash; za pomocą gestów niestandardowych.

## <a name="custom-gesture-activity"></a>Działanie gestów niestandardowych

Ekran końcowy, w tym instruktażu będą używać gestów niestandardowych.

Na potrzeby tego przewodnika, biblioteka gestów został już utworzony za pomocą narzędzia gestu i dodane do projektu w pliku **zasobów/nieprzetworzone/gestów**. Z tego bitu gospodarstw domowych przeszkadza umożliwia pracujesz na ostatnie działanie w instruktażu.

-   Dodaj plik układu o nazwie **niestandardowe\_gestu\_layout.axml** do projektu z następującą zawartością. Projekt ma już wszystkie obrazy w **zasobów** folderu:

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

-   Następnie dodaj nowe działanie do projektu i nadaj mu nazwę `CustomGestureRecognizerActivity.cs`. Dodaj dwie zmienne wystąpienia klasy, jak pokazano w następujących dwóch wierszy kodu:

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   Edytuj `OnCreate` metody tego działania, tak że przypominają poniższy kod. Umożliwia potrwać chwilę, aby wyjaśnić, co się dzieje w tym kodzie. Pierwszą rzeczą, którą robimy to tworzenia wystąpienia `GestureOverlayView` i ustaw ją jako widoku głównego działania.
    Możemy również przypisać program obsługi zdarzeń do `GesturePerformed` zdarzenia `GestureOverlayView`. Następnie możemy rozszerzanie plik układu, który został utworzony wcześniej i dodać jako widok podrzędny `GestureOverlayView`. Ostatnim krokiem jest, aby zainicjować zmienną `_gestureLibrary` i załadować plik gestów z zasobów aplikacji. Jeśli z jakiegoś powodu nie można załadować pliku gestów, nie ma wiele, które można wykonać to działanie, dlatego zamknięcia:

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

-   Końcowe rzeczą, musimy zaimplementować metodę `GestureOverlayViewOnGesturePerformed` jak pokazano w poniższym fragmencie kodu. Gdy `GestureOverlayView` wykryciu gestu, ponownie wywołuje się do tej metody. Pierwszą rzeczą, którą spróbujemy uzyskać `IList<Prediction>` obiekty, które odpowiadają gestu przez wywołanie metody `_gestureLibrary.Recognize()`. Możemy użyć znacznej liczby LINQ, aby uzyskać `Prediction` ma najwyższy wynik dla gestu.

    Jeśli nie było żadnych pasujących gestu z wysokim wystarczająco dużo wynik, a następnie program obsługi zdarzeń kończy działanie bez żadnego działania. W przeciwnym razie firma Microsoft Sprawdź nazwę przewidywania i zmienić obraz jest wyświetlany w oparciu o nazwę gestu:

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

-   Uruchom aplikację i uruchomić działanie aparat rozpoznawania gestów niestandardowych. Powinien on wyglądać podobnie jak na poniższym zrzucie ekranu:

    [![Zrzut ekranu z Sprawdź mnie obrazu](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    Teraz rysować znacznik wyboru na ekranie i mapy bitowej, są wyświetlane powinien wyglądać jak pokazano w następnym zrzuty ekranu:

    [![Rozpoznano rysowane znacznik wyboru znacznik wyboru](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    Na koniec można rysować bazgrołów na ekranie. Pole wyboru, należy zmienić przywracając jego oryginalny obraz, jak pokazano w tych zrzuty ekranu:

    [![Bazgrołów na ekranie, oryginalny obraz jest wyświetlany.](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Masz teraz zrozumienia sposobu integrowania Dotyk i gesty w aplikacji systemu Android przy użyciu platformy Xamarin.Android.


## <a name="related-links"></a>Linki pokrewne

- [Dla systemu android w ogóle Start (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Dla systemu android w ogóle końcowe (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
