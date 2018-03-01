---
title: LinearLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 3cc5db39280c72f0de9dbdae07a49b56416c90a5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="linearlayout"></a>LinearLayout

[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) jest [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) wyświetlający podrzędnych [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) elementów w kierunku liniowej pionie lub poziomie.

Należy pamiętać o używaniu nadmiernie [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).
Jeśli rozpoczniesz zagnieżdżenia wielu [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s, warto rozważyć użycie [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) zamiast tego.

Utwórz nowy projekt o nazwie **HelloLinearLayout**.

Otwórz **Resources/Layout/Main.axml** i Wstaw następujący:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "fill_parent"
      android:layout_height=    "fill_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

Dokładnie sprawdź plik XML. Jest katalogiem głównym [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) definiuje orientacji być pionowy &ndash; wszystkich podrzędnych [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)s (które it ma dwa) będzie skumulowany pionowo. Pierwszy element podrzędny jest inny [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) używającą orientację poziomą i drugiego elementu podrzędnego jest [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) używającą orientacji pionowej. Każdy z tych zagnieżdżony [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s zawiera kilka [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) elementów, które są ukierunkowane ze sobą w ten sposób zdefiniowany przez ich nadrzędnego [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).

Teraz otworzyć **HelloLinearLayout.cs** i ładuje **Resources/Layout/Main.axml** układu w [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metody:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) Metody ładuje plik układu dla [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), określony przez identyfikator zasobu &ndash; `Resources.Layout.Main` odwołuje się do **zasobów/układ / Main.axml** pliku układu.

Uruchom aplikację. Powinny być widoczne następujące czynności:

[ ![Zrzut ekranu aplikacji pierwszy LinearLayout ułożone poziomo, drugie w pionie](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png)

Zwróć uwagę, jak atrybutów XML określają zachowanie każdego widoku. Spróbuj eksperymentowanie z różnych wartości `android:layout_weight` aby zobaczyć rozkład nieruchomości ekranu na podstawie wagi każdego elementu. Zobacz [wspólnych obiektów układu](http://developer.android.com/guide/topics/ui/declaring-layout.html) dokumentu, aby uzyskać więcej informacji na temat [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) dojść `android:layout_weight` atrybutu.

<a name="References" />

## <a name="references"></a>Odwołania

-   [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Części tej stronie są zmiany na podstawie pracy utworzone i udostępnione przez Android Otwórz projekt źródłowy i używane zgodnie z warunki opisane w*
[*Creative Commons 2.5 autorstwa licencji* ](http://creativecommons.org/licenses/by/2.5/).

