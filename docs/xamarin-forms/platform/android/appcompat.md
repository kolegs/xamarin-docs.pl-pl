---
title: "Dodawanie AppCompat i materiały projektu"
description: "Wykonaj te kroki, aby przekonwertować istniejących aplikacji platformy Xamarin.Forms Android użycie AppCompat i materiały projektu"
ms.topic: article
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: 3014db91ff87f0e73291595a17dd780b5e3cd3c2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="adding-appcompat-and-material-design"></a>Dodawanie AppCompat i materiały projektu

_Wykonaj te kroki, aby przekonwertować istniejących aplikacji platformy Xamarin.Forms Android użycie AppCompat i materiały projektu_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>Omówienie

Te instrukcje opisano sposób aktualizowania istniejących aplikacji platformy Xamarin.Forms Android biblioteki AppCompat oraz włączyć materiałów projektowania w systemie Android w wersji aplikacji platformy Xamarin.Forms.

### <a name="1-update-xamarinforms"></a>1. Update Xamarin.Forms

Upewnij się, że dane rozwiązanie korzysta z platformy Xamarin.Forms 2.0 lub nowszej. Zaktualizuj pakiet Nuget platformy Xamarin.Forms 2.0, jeśli jest to wymagane.

### <a name="2-check-android-version"></a>2. Sprawdź wersję systemu Android

Upewnij się, że platforma docelowa projektu dla systemu Android jest system Android 6.0 (Marshmallow). Sprawdź **projekt systemu Android > Opcje > kompilacji > Ogólne** wybrano ustawienia, aby zapewnić corrent framework:

 ![](appcompat-images/target-android-6-sml.png "Konfiguracja kompilacji dla systemu android ogólne")

### <a name="3-add-new-themes-to-support-material-design"></a>3. Dodaj nowe kompozycje do obsługi materiałów projektu

Utwórz następujące trzy pliki w projekcie systemu Android i Wklej zawartość poniżej. Udostępnia Google [Przewodnik po stylu](http://www.google.com/design/spec/style/color.html#color-color-palette) i [kolorów palety generator](http://www.materialpalette.com/) pomaga wybrać schemat kolorów alternatywnych, z określoną.

**Resources/values/colors.xml**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**Resources/values/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
  </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primaryDark</item>
    <item name="colorAccent">@color/accent</item>
    <item name="android:windowBackground">@color/window_background</item>
    <item name="windowActionModeOverlay">true</item>
  </style>
</resources>
```

Styl dodatkowe muszą być zawarte w **v21 wartości** folderu do zastosowania określonych właściwości podczas uruchamiania na Android interfejs typu lizak i nowszych.

**Resources/values-v21/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. Update AndroidManifest.xml

Zapewnienie tę nową kompozycję informacje są używane, ustawić motyw w **AndroidManifest** przez dodanie `android:theme="@style/MyTheme"` (pozostaw reszty XML była).

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. Podaj układy narzędzi i kartę

Utwórz **Tabbar.axml** i **Toolbar.axml** plików **zasobów/układ** katalogu i Wklej w ich zawartość z poniżej:

**Resources/layout/Tabbar.axml**

```xml
<android.support.design.widget.TabLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/sliding_tabs"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:tabIndicatorColor="@android:color/white"
    app:tabGravity="fill"
    app:tabMode="fixed" />
```

Kilka właściwości kart zostały ustawione w tym grawitacji kartę do `fill` i tryb w celu `fixed`.
Jeśli masz wiele kart może chcesz przełączyć, to aby przewijanego — zapoznaj się z artykułem Android [dokumentacji TabLayout](http://developer.android.com/reference/android/support/design/widget/TabLayout.html) Aby dowiedzieć się więcej.

**Resources/layout/Toolbar.axml**

```xml
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
    app:layout_scrollFlags="scroll|enterAlways" />
```

W tych plikach tworzymy motyw określonych dla narzędzi, które mogą się różnić dla aplikacji.
Zapoznaj się [Hello narzędzi](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/) blogu, aby dowiedzieć się więcej.


### <a name="6-update-the-mainactivity"></a>6. Aktualizacja `MainActivity`

W istniejących aplikacji platformy Xamarin.Forms **MainActivity.cs** klasa będzie dziedziczyć `FormsApplicationActivity`. To muszą zostać zastąpione `FormsAppCompatActivity` Aby włączyć nowe funkcje.

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

Na koniec "okablować się" nowe układów z kroku 5 w `OnCreate` metody, jak pokazano poniżej:

```csharp
protected override void OnCreate(Bundle bundle)
{
  // set the layout resources first
  FormsAppCompatActivity.ToolbarResource = Resource.Layout.Toolbar;
  FormsAppCompatActivity.TabLayoutResource = Resource.Layout.Tabbar;

  // then call base.OnCreate and the Xamarin.Forms methods
  base.OnCreate(bundle);
  Forms.Init(this, bundle);
  LoadApplication(new App());
}
```
