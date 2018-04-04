---
title: RelativeLayout
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 51b58c02ca2768481ffd8bf0e508e94fd408f465
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) jest [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) wyświetlający podrzędnych [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) elementy w pozycji względem siebie. Pozycja [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) można określić jako względem elementów równorzędnych, (na przykład w lewo z lub poniżej danego elementu) lub w pozycji względem [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) obszaru (takich jak Wyrównanie w dół, lewo Centrum).

A [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) jest bardzo zaawansowane narzędzia do projektowania interfejsu użytkownika, ponieważ można go usunąć zagnieżdżone [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s. Jeśli okaże się, że przy użyciu kilku zagnieżdżone [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) grup, można je zastąpić za pomocą jednej [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/).

Utwórz nowy projekt o nazwie **HelloRelativeLayout**.

Otwórz **Resources/Layout/Main.axml** pliku i Wstaw następujący:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background"
        android:layout_below="@id/label"/>
    <Button
        android:id="@+id/ok"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/entry"
        android:layout_alignParentRight="true"
        android:layout_marginLeft="10dip"
        android:text="OK" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toLeftOf="@id/ok"
        android:layout_alignTop="@id/ok"
        android:text="Cancel" />
</RelativeLayout>
```

Zwróć uwagę, każdy z `android:layout_*` atrybutów, takich jak `layout_below`, `layout_alignParentRight`, i `layout_toLeftOf`.
Korzystając z [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), można użyć tych atrybutów opisujących sposób każdej pozycji [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/). Każdej z nich te atrybuty zdefiniuj inny rodzaj względne położenie. Niektóre atrybuty użyć Identyfikatora zasobu element równorzędny [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) do zdefiniowania własnych względne położenie. Na przykład w ciągu ostatnich [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) zdefiniowano wchodzić z lewej strony i wyrównane z — top z [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) identyfikowany przez identyfikator `ok` (czyli poprzedniej [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

Wszystkie atrybuty układu dostępne są zdefiniowane w [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/).

Upewnij się, załadować ten układ w [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metody:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) Metody ładuje plik układu dla [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), określony przez identyfikator zasobu &mdash; `Resource.Layout.Main` odwołuje się do **zasobów/układ / Main.axml** pliku układu.

Uruchom aplikację. Powinien zostać wyświetlony następujący układ:

[![Zrzut ekranu przedstawiający względną układu element TextView, typu części EditText i dwóch przycisków](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>Zasoby

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*Części tej stronie są zmiany na podstawie pracy utworzone i udostępnione przez Android Otwórz projekt źródłowy i używane zgodnie z warunki opisane w*
[*Creative Commons 2.5 autorstwa licencji* ](http://creativecommons.org/licenses/by/2.5/).
