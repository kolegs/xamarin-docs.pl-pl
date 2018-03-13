---
title: TableLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 31713937f9423bacdee83620a7e852ea6f141ee6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="tablelayout"></a>TableLayout

[`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) jest [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) wyświetlający podrzędnych [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) elementów wierszy i kolumn.

Utwórz nowy projekt o nazwie **HelloTableLayout**.

Otwórz **Resources/Layout/Main.axml** pliku i Wstaw następujący:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:stretchColumns="1">

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Open..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-O"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save As..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-Shift-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Import..."
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Export..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-E"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Quit"
            android:padding="3dip"/>
    </TableRow>
</TableLayout>
```

Zwróć uwagę, jak to przypomina strukturę tabeli HTML. [ `TableLayout` ](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) Element przypomina HTML `<table>` element; [ `TableRow` ](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) przypomina `<tr>` element; ale w komórkach, można użyć dowolnego rodzaju [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) elementu. W tym przykładzie [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) jest używany dla każdej komórki. Between niektóre wiersze, jest również podstawowego [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/), który jest używany do rysowania linii poziomej.

Upewnij się, że Twoje **HelloTableLayout** działania ładuje ten układ w [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metody:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) Metody ładuje plik układu dla [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), określony przez identyfikator zasobu &mdash; `Resource.Layout.Main` odwołuje się do **zasobów/układ / Main.axml** pliku układu.

Uruchom aplikację. Powinny być widoczne następujące czynności:

[![Zrzut ekranu aplikacji TableLayout wyświetlanie wielu wierszy tabeli](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)



## <a name="references"></a>Odwołania

-   [`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 

-   [`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) 

-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Części tej stronie są zmiany na podstawie pracy utworzone i udostępnione przez Android Otwórz projekt źródłowy i używane zgodnie z warunki opisane w*
[*Creative Commons 2.5 autorstwa licencji* ](http://creativecommons.org/licenses/by/2.5/).
