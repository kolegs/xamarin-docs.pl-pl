---
title: Touch w systemie Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1d9cf345aa971c40f4132cc7970ed1244640da14
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="touch-in-android"></a>Touch w systemie Android

Wiele takich jak systemy iOS, Android tworzy obiekt przechowujący dane dotyczące fizycznego interakcji użytkownika z ekranem &ndash; `Android.View.MotionEvent` obiektu. Ten obiekt zawiera dane, takie jak jaka akcja jest wykonywana, gdzie touch miało miejsce, nacisku została zastosowana, itp. A `MotionEvent` obiektu dzieli ruch na następujące wartości:

-  Kod akcji, który opisuje typ ruchu, takie jak początkowa touch touch przenoszenie między ekranu lub zakończenie touch.

-  Zbiór wartości na osi, które opisują położenie `MotionEvent` i inne właściwości przepływu, takich jak gdzie touch odbywa się po naciśnięciu miało miejsce i użyto nacisku.
   Wartości na osi mogą różnić się w zależności od urządzenia, więc poprzedniej liście nie opisano wszystkich wartości na osi.


`MotionEvent` Obiektu zostaną przekazane do odpowiedniej metody w aplikacji. Istnieją trzy sposoby dla aplikacji platformy Xamarin.Android odpowiadać na zdarzenie touch:

-  *Program obsługi zdarzeń, aby przypisać `View.Touch`*  - `Android.Views.View` klasa ma `EventHandler<View.TouchEventArgs>` aplikacji, które można przypisać do obsługi. Jest to typowe zachowanie platformy .NET.

-  *Implementowanie `View.IOnTouchListener`*  -wystąpień tego interfejsu może być przypisana do obiekt widoku przy użyciu widoku. `SetOnListener` Metoda. To jest funkcjonalnym odpowiednikiem przypisywanie obsługi zdarzeń do `View.Touch` zdarzeń. W przypadku niektórych typowych lub współużytkowanych logikę, która wiele różnych widoków może być konieczne, jeśli są one dotknąć będzie bardziej wydajne, Utwórz klasę i zaimplementuj tę metodę, niż można przypisać każdego widoku własny program obsługi zdarzeń.

-  *Zastąpienie `View.OnTouchEvent`*  — wszystkie widoki w Android podklasy `Android.Views.View`. Gdy widok jest dotknięciu, Android wywoła `OnTouchEvent` i przekaż go `MotionEvent` obiektu jako parametr.


> [!NOTE]
> Nie wszystkie urządzenia Android obsługują ekranów dotykowych. 

Dodawanie następujący tag do pliku manifestu powoduje Google Play do wyświetlenia tylko aplikację do tych urządzeń, które są z obsługą dotyku:

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>Gestów

Gest jest odręcznie kształt na ekran dotykowy. Gest może mieć co najmniej jeden pociągnięć, każdego pociągnięcia składające się z sekwencji punktów utworzonych przez inny punkt kontaktu z ekranem. Android może obsługiwać wiele różnych typów gesty, od prostego fling na ekranie do złożonych gesty, które obejmują wielodotyku.

Udostępnia android `Android.Gestures` przestrzeni nazw specjalnie z myślą o reagowanie na gestów i zarządzanie nimi. At wszystkie gesty to specjalne klasy o nazwie `Android.Gestures.GestureDetector`. Jak wskazuje nazwę, ta klasa będzie nasłuchiwać gestów i zdarzenia na podstawie `MotionEvents` dostarczane przez system operacyjny.

Aby zaimplementować detektora gestu, musi utworzyć wystąpienia działania `GestureDetector` klasy i zapewnić wystąpienie `IOnGestureListener`, jak pokazano poniższy fragment kodu:

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

Działanie musi także implementować OnTouchEvent i przekaż MotionEvent do detektora gestu. Poniższy fragment kodu przedstawia przykład:

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

Gdy wystąpienie klasy `GestureDetector` identyfikuje gestu zainteresowania, powiadomi działania lub aplikacji przez wywołanie zdarzenia lub za pomocą dostarczonych przez wywołanie zwrotne `GestureDetector.IOnGestureListener`.
Ten interfejs zapewnia metody sześciu różnych gestów:

-  *OnDown* -wywoływane, gdy występuje tap, ale nie została zwolniona.

-  *OnFling* -wywoływane, gdy występuje fling oraz są udostępniane dane touch rozpoczęcia i zakończenia, który wywołał zdarzenie.

-  *OnLongPress* -wywoływane, gdy występuje długa naciśnij.

-  *OnScroll* -wywoływana, gdy wystąpi zdarzenie przewijania.

-  *OnShowPress* — nazywany po OnDown i Przenieś lub zdarzenie zwolnienia nie została wykonana.

-  *OnSingleTapUp* -wywoływane, gdy występuje pojedynczym wybraniem.


W wielu przypadkach aplikacje mogą być zainteresowane tylko w podzbiorze gestów. W takim przypadku aplikacji powinien rozszerzenie klasy GestureDetector.SimpleOnGestureListener i zastąpić metody, które odpowiadają zdarzenia czy są zainteresowani.

## <a name="custom-gestures"></a>Gestów niestandardowych

Gesty to doskonały sposób na użytkownikom na interakcję z aplikacją. Interfejsy API, którego dotychczasowy zaobserwowano czy wystarcza do prostych gestów, ale może okazać się nieco uciążliwe gestów bardziej skomplikowane. Aby pomóc w bardziej skomplikowane gesty, Android zawiera inny zestaw interfejsów API w przestrzeni nazw Android.Gestures, która ułatwi niektórych obciążeń skojarzone z gestów niestandardowych.

### <a name="creating-custom-gestures"></a>Tworzenie gestów niestandardowych

Od systemu Android w wersji 1.6 zestaw SDK systemu Android jest dostarczany z preinstalowanym emulatora wywołuje konstruktor gestów aplikacji. Ta aplikacja umożliwia deweloperom tworzenie wstępnie zdefiniowane gesty, które mogą być osadzone w aplikacji. Poniższy zrzut ekranu przedstawia przykład konstruktora gestów:

[![Zrzut ekranu gestów konstruktora za pomocą gestów przykład](touch-in-android-images/image11.png)](touch-in-android-images/image11.png#lightbox)

Ulepszone wersję tej aplikacji o nazwie gestu narzędzie można znaleźć Google Play. Narzędzie gestu jest bardzo podobne gesty, przy użyciu konstruktora z tą różnicą, że można testować gesty, po ich utworzeniu. Ten dalej zrzut ekranu przedstawia konstruktora gestów:

[![Zrzut ekranu narzędzia gestu za pomocą gestów przykład](touch-in-android-images/image12.png)](touch-in-android-images/image12.png#lightbox)

Narzędzie gestu jest nieco bardziej użyteczna w przypadku tworzenia gestów niestandardowych, który zezwala gestów do sprawdzenia, ponieważ są one tworzone i jest łatwo dostępna za pośrednictwem witryny Google Play.

Gest pozwala utworzyć gestu za pomocą rysowania na ekranie i przypisanie nazwy. Po utworzeniu gestów są zapisywane w pliku binarnego na karcie SD urządzenia. Ten plik należy pobrać z urządzenia, a następnie spakować z aplikacją w /Resources/raw folderu. Ten plik może zostać pobrany z emulatora przy użyciu mostka debugowania systemu Android. W poniższym przykładzie przedstawiono proces kopiowania pliku z węzła Galaxy do zasobu katalogu aplikacji:

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

Po pobraniu pliku musi być dołączane do aplikacji w katalogu /Resources/raw. Najprostszym sposobem, aby użyć tego pliku gestu jest załadowanie pliku do GestureLibrary, jak pokazano w poniższy fragment kodu:

```csharp
GestureLibary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>Za pomocą gestów niestandardowych

Do rozpoznawania gestów niestandardowych w działaniu, musi on mieć obiektu Android.Gesture.GestureOverlay dodane do jego układu. Poniższy fragment kodu przedstawia sposób programowego dodawania GestureOverlayView do działania:

```csharp
GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
gestureOverlayView.AddOnGesturePerformedListener(this);
SetContentView(gestureOverlayView);
```

Następujący fragment kodu XML przedstawiono sposób dodawania GestureOverlayView deklaratywnie:

```xml
<android.gesture.GestureOverlayView
    android:id="@+id/gestures"
    android:layout_width="match_parent "
    android:layout_height="match_parent" />
```

`GestureOverlayView` Ma kilka zdarzeń, które będą wywoływane podczas rysowania gestu. Jest najbardziej interesujących zdarzeń `GesturePeformed`. To zdarzenie jest wywoływane, gdy użytkownik zakończy rysowania ich gestu.

Gdy to zdarzenie jest zgłaszane, działanie zapyta, `GestureLibrary` i spróbuj odpowiada gestu użytkownika z jednego z gestów utworzony przez narzędzie gestu. `GestureLibrary` Zwraca listę obiektów prognozowania.

Każdy obiekt prognozowania zawiera wynik i nazwę jednego z gestów w `GestureLibrary`. Im większa jest ocena, tym bardziej prawdopodobne gestu o nazwie prognozowania odpowiada gestu rysowane przez użytkownika.
Ogólnie rzecz biorąc wyniki niższa niż 1.0 są traktowane jako niska dopasowań.

Poniższy kod przedstawia przykład dopasowywania gestu:

```csharp
private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
{
    // In this example _gestureLibrary was instantiated in OnCreate
    IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
    orderby p.Score descending
    where p.Score > 1.0
    select p;
    Prediction prediction = predictions.FirstOrDefault();

    if (prediction == null)
    {
        Log.Debug(GetType().FullName, "Nothing matched the user's gesture.");
        return;
    }

    Toast.MakeText(this, prediction.Name, ToastLength.Short).Show();
}
```

W to zrobić należy stosować zrozumienia sposobu używania touch i gestów w aplikacji platformy Xamarin.Android. Daj nam teraz przejść do wskazówki i wyświetlić wszystkie pojęcia związane z pracy przykładowej aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [Android Touch Start (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch końcowego (na przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
