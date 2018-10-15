---
title: Dodawanie AppCompat i materiały projektu
description: W tym artykule opisano sposób konwertowania istniejących aplikacji systemu Android na platformie Xamarin.Forms w celu umożliwienia korzystania z AppCompat i Material Design.
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: c2eed44a7c684b91ceed4493a83ff3b4e1578b5f
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242924"
---
# <a name="adding-appcompat-and-material-design"></a>Dodawanie AppCompat i materiały projektu

_Wykonaj te kroki, aby przygotować istniejące aplikacje platformy Xamarin.Forms Android do użycia AppCompat i Material Design_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>Omówienie

Te instrukcje wyjaśniają, w jaki sposób przygotować istniejące aplikacje platformy Xamarin.Forms Android do użycia biblioteki AppCompat oraz jak włączyć Material Design w Twojej aplikacji Xamarin.Forms w wersji na Androida.

### <a name="1-update-xamarinforms"></a>1. Aktualizacja platformy Xamarin.Forms

Upewnij się, że dane rozwiązanie korzysta z platformy Xamarin.Forms 2.0 lub nowszej. Zaktualizuj pakiet Nuget platformy Xamarin.Forms do wersji 2.0, jeśli jest to konieczne.

### <a name="2-check-android-version"></a>2. Sprawdź wersję systemu Android

Upewnij się, że platforma docelowa projektu dla systemu Android to Android 6.0 (Marshmallow). Sprawdź, czy w obszarze **Projekt systemu Android > Opcje > Kompilacja > Ogólne** wybrano prawidłowy framework:

 ![](appcompat-images/target-android-6-sml.png "Konfiguracja kompilacji dla systemu Android")

### <a name="3-add-new-themes-to-support-material-design"></a>3. Dodaj nowe kompozycje do obsługi Material Design

Utwórz następujące trzy pliki w projekcie systemu Android i wklej zawartość podaną poniżej. Google udostępnia [Przewodnik po stylu](http://www.google.com/design/spec/style/color.html#color-color-palette) i [generator palet kolorów](http://www.materialpalette.com/), które pomagają wybrać schemat kolorów alternatywny do tego określonego poniżej.

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

Styl dodatkowy musi być zawarty w folderze **values-v21**, by można było zastosować określone właściwości na Androidzie w wersji Lollipop i nowszych.

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

### <a name="4-update-androidmanifestxml"></a>4. Zaktualizuj plik AndroidManifest.xml

By mieć pewność, że nowy motyw będzie używany, należy ustawić motyw w pliku **AndroidManifest** poprzez dodanie `android:theme="@style/MyTheme"` (bez zmieniania reszty pliku XML).

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. Podaj układy paska narzędzi i kart

Utwórz pliki **Tabbar.axml** i **Toolbar.axml** w folderze **Resources/layout** i wklej do nich zawartość podaną poniżej:

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

Ustawiono tutaj niektóre właściwości kart, w tym grawitację jako `fill` i tryb jako `fixed`.
Jeśli masz wiele kart, możesz chcieć ustawić tryb przewijany — zapoznaj się z [dokumentacją TabLayout](http://developer.android.com/reference/android/support/design/widget/TabLayout.html) dla systemu Android, aby dowiedzieć się więcej.

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

W tych plikach określamy motyw paska narzędzi, który może się różnić w Twojej aplikacji.
Zapoznaj się z wpisem [Witaj pasku narzędzi](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/) na blogu, aby dowiedzieć się więcej.


### <a name="6-update-the-mainactivity"></a>6. Zaktualizuj `MainActivity`

W istniejących aplikacjach platformy Xamarin.Forms klasa **MainActivity.cs** będzie dziedziczyć po `FormsApplicationActivity`. Ta klasa powinna zostać zastąpiona przez `FormsAppCompatActivity`, aby włączyć nowe funkcje.

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity // was FormsApplicationActivity
```

Na koniec należy ustawić nowe układy z kroku 5 w metodzie `OnCreate`, jak poniżej:

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
