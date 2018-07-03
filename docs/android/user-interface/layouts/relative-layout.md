---
title: Za pomocą RelativeLayout platformie Xamarin.Android
description: Jak używać RelativeLayout w aplikacji platformy Xamarin.Android
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/29/2018
ms.openlocfilehash: af8d37775a798fc6019106a66df75843a951c108
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403419"
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) jest [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) wyświetlającą podrzędnych [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) elementów w położenie względne. Pozycja [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) może być określony względem elementów równorzędnych, (na przykład po lewej stronie programu lub poniżej danego elementu) lub w pozycji względem [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) obszaru (takich jak wyrównane do dolnej, lewej strony Centrum).

A [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) jest narzędziem ogromne Projektowanie interfejsu użytkownika, ponieważ może ona wyeliminować zagnieżdżone [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s. Jeśli okaże się, że za pomocą kilku zagnieżdżone [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) grup, można je zastąpić za pomocą jednego [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/).

Utwórz nowy projekt o nazwie **HelloRelativeLayout**.

Otwórz **Resources/Layout/Main.axml** pliku i Wstaw następujący:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="match_parent"
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

Zwróć uwagę, każdy z `android:layout_*` atrybuty, takie jak `layout_below`, `layout_alignParentRight`, i `layout_toLeftOf`.
Korzystając z [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), można użyć tych atrybutów do opisywania sposobu każdej pozycji [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/). Każdej z nich te atrybuty zdefiniować inny rodzaj względne położenie. Niektóre atrybuty Użyj Identyfikatora zasobu element równorzędny [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) zdefiniować własne względne położenie. Na przykład ostatniego [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) zdefiniowano leży po lewej stronie programu i wyrównane z — top z [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) identyfikowane przez identyfikator `ok` (czyli poprzedniej [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

Wszystkie atrybuty dostępne układu są zdefiniowane w [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/).

Upewnij się, załadować ten układ w [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metody:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) Metoda ładuje plik układu dla [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), określony przez identyfikator zasobu &mdash; `Resource.Layout.Main` odwołuje się do **zasobów/układ / Main.axml** plik układu.

Uruchom aplikację. Powinny zostać wyświetlone następujące układu:

[![Zrzut ekranu przedstawiający względne układ z element TextView, typu części EditText oraz dwa przyciski](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>Resources

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*Części tej strony są modyfikacje oparte na pracy, utworzone i udostępnione przez Android Open Source Project i używane zgodnie z postanowieniami*
[*2.5 autorstwa licencjiCreativeCommons* ](http://creativecommons.org/licenses/by/2.5/).
