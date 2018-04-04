---
title: Zgodność z paska narzędzi
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: c3418a007ded30175049e83d515f59d5134deee0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="toolbar-compatibility"></a>Zgodność z paska narzędzi


## <a name="overview"></a>Omówienie

W tej sekcji opisano sposób korzystania `Toolbar` na systemu android w wersjach wcześniejszych niż Android 5.0 interfejs typu lizak. Jeśli aplikacja nie obsługuje systemu android w wersjach wcześniejszych niż Android 5.0, możesz pominąć tę sekcję. 

Ponieważ `Toolbar` jest częścią biblioteki obsługi systemu Android w wersji 7, można użyć na urządzeniach z systemem Android 2.1 (poziom interfejsu API 7) lub nowszy. Jednak [biblioteki obsługi systemu Android w wersji 7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet musi być zainstalowana i zmodyfikować kod, tak aby były używane `Toolbar` implementacji podane w tej bibliotece. W tej sekcji opisano sposób instalowania NuGet to a modyfikować **ToolbarFun** aplikacji z [Dodawanie drugiego narzędzi](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) tak, aby była uruchamiana na systemu android w wersjach starszych niż 5.0 interfejs typu lizak.

Aby zmodyfikować aplikację na AppCompat wersja narzędzi: 

1.  Wartość minimalna i docelowy Android wersji aplikacji.

2.  Zainstaluj pakiet AppCompat NuGet.

3.  Użyj motywu AppCompat zamiast wbudowanego motywu systemu Android.

4.  Modyfikowanie `MainActivity` , aby go podklasy `AppCompatActivity` zamiast `Activity`. 

Każdy z tych kroków jest szczegółowo opisane w kolejnych sekcjach.



## <a name="set-the-minimum-and-target-android-version"></a>Wartość minimalna i docelowa wersja systemu Android

Platformę docelową aplikacji musi być ustawiony na 21 poziom interfejsu API lub większą lub aplikacja nie zostanie wdrożona prawidłowo. Jeśli wystąpił błąd, takich jak **identyfikatora zasobów organizacji nie znaleziono atrybutu "tileModeX" w pakiecie "android"** jest widoczna podczas wdrażania aplikacji, ponieważ platforma docelowa nie jest ustawiony na jest to **Android 5.0 (21 poziom interfejsu API — typu lizak)**  lub nowszej. 

Ustaw docelową platformę mniejsza od poziomu do 21 poziom interfejsu API i ustaw ustawienia poziomu projektu interfejsu API systemu Android minimalna wersja systemu Android aplikacji jest zapewnienie pomocy technicznej. Aby uzyskać więcej informacji na temat ustawiania poziomy interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md). W `ToolbarFun` przykład Minimalna wersja systemu Android ma ustawioną wartość KitKat (4.4 poziom interfejsu API). 


## <a name="install-the-appcompat-nuget-package"></a>Zainstaluj pakiet AppCompat NuGet

Następnie dodaj [biblioteki obsługi systemu Android w wersji 7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) pakietu do projektu. W programie Visual Studio, kliknij prawym przyciskiem myszy **odwołania** i wybierz **Zarządzaj pakietami NuGet...** . Kliknij przycisk **Przeglądaj** i wyszukaj **biblioteki obsługi systemu Android w wersji 7 AppCompat**. Wybierz **Xamarin.Android.Support.v7.AppCompat** i kliknij przycisk **zainstalować**: 

[![Zrzut ekranu w wersji 7 Appcompat pakietu wybranego w pakiety zarządzania pakietami NuGet](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

Podczas instalowania NuGet to kilka pakietów NuGet są również instalowane Jeśli nie są jeszcze zainstalowane (takich jak **Xamarin.Android.Support.Animated.Vector.Drawable**, **Xamarin.Android.Support.v4**, i **Xamarin.Android.Support.Vector.Drawable**). Aby uzyskać więcej informacji na temat instalowania pakietów NuGet, zobacz [wskazówki: w tym NuGet w projekcie](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 


## <a name="use-an-appcompat-theme-and-toolbar"></a>Użyj AppCompat kompozycji i narzędzi

Biblioteka AppCompat jest dostarczany z kilku `Theme.AppCompat` kompozycje, których można użyć w dowolnej wersji systemu Android obsługiwany przez bibliotekę AppCompat. `ToolbarFun` Przykład motywu aplikacji jest pochodną `Theme.Material.Light.DarkActionBar`, która nie jest dostępne na systemu Android w wersji wcześniejszej niż interfejs typu lizak. W związku z tym `ToolbarFun` muszą być dostosowane do użycia dla tego motywu odpowiednikiem AppCompat `Theme.AppCompat.Light.DarkActionBar`. Ponadto ponieważ `Toolbar` jest niedostępne w wersjach systemu android starszych niż interfejs typu lizak, musi używamy wersja AppCompat `Toolbar`. W związku z tym należy użyć układów `android.support.v7.widget.Toolbar` zamiast `Toolbar`. 


### <a name="update-layouts"></a>Układy aktualizacji

Edytuj **Resources/layout/Main.axml** i Zastąp `Toolbar` — element XML następujące: 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Edytuj **Resources/layout/toolbar.xml** i zastąp jego zawartość XML następujące: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>
```

Należy pamiętać, że `?attr` wartości nie są poprzedzane prefiksem `android:` (przypominają, iż `?` notacji odwołuje się do zasobu w bieżącego motywu). Jeśli `?android:attr` nadal były używane w tym miejscu Android będzie odwoływać wartość atrybutu z aktualnie uruchomioną platformy, a nie z biblioteki AppCompat. Ponieważ w tym przykładzie użyto `actionBarSize` zdefiniowany przez bibliotekę AppCompat `android:` prefiks zostało porzucone. Podobnie `@android:style` jest zmieniana na `@style` , aby `android:theme` atrybut ma ustawioną w bibliotece AppCompat motyw &ndash; `ThemeOverlay.AppCompat.Dark.ActionBar` motywu jest tu używany zamiast `ThemeOverlay.Material.Dark.ActionBar`. 


### <a name="update-the-style"></a>Aktualizuj styl

Edytuj **Resources/values/styles.xml** i zastąp jego zawartość XML następujące: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="MyTheme.Base"> </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="windowNoTitle">true</item>
    <item name="windowActionBar">false</item>
    <item name="colorPrimary">#5A8622</item>
    <item name="colorAccent">#A88F2D</item>
  </style>
</resources>
```

Nazwy elementów i motyw nadrzędnego, w tym przykładzie nie są poprzedzane prefiksem `android:` ponieważ używamy AppCompat biblioteki. Ponadto motywu nadrzędnego jest zmieniana na wersję AppCompat `Light.DarkActionBar`. 



### <a name="update-menus"></a>Aktualizowanie menu

Aby obsługiwać starszych wersjach systemu android, biblioteka AppCompat używa atrybutów niestandardowych, które Duplikuj atrybuty `android:` przestrzeni nazw. Jednak niektóre atrybuty (takich jak `showAsAction` atrybutu używanego w `<menu>` tagu) nie istnieje w ramach systemu Android na starszych urządzeń &ndash; `showAsAction` została wprowadzona w systemie Android 11 interfejsu API, ale nie jest dostępna w systemie Android 7 interfejsu API. Z tego powodu niestandardowej przestrzeni nazw może służyć jako prefiks wszystkie atrybuty zdefiniowane przez Biblioteka obsługi. W menu Pliki zasobów, nazwę przestrzeni nazw `local` jest zdefiniowany dla prefiksu `showAsAction` atrybutu. 

Edytuj **Resources/menu/top_menus.xml** i zastąp jego zawartość XML następujące:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       local:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       local:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       local:showAsAction="never"
       android:title="Preferences" />
</menu>
```

`local` Przestrzeni nazw jest dodawany w tym:

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

`showAsAction` Atrybutu jest poprzedzone znakiem to `local:` przestrzeni nazw zamiast `android:` 

```csharp
local:showAsAction="ifRoom"
```

Podobnie, Edytuj **Resources/menu/edit_menus.xml** i zastąp jego zawartość XML następujące:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

W jaki sposób ten przełącznik przestrzeń nazw zapewnia obsługę `showAsAction` atrybutu Android wersje starsze niż 11 poziom interfejsu API? Atrybut niestandardowy `showAsAction` i wszystkie jego możliwych wartości zostaną uwzględnione w aplikacji, podczas instalowania AppCompat NuGet. 


## <a name="subclass-appcompatactivity"></a>Podklasy AppCompatActivity

Ostatnim krokiem w konwersji jest zmodyfikowanie `MainActivity` tak, aby podklasy `AppCompactActivity`. Edytuj **MainActivity.cs** i dodaj następującą `using` instrukcji: 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

To deklaruje `Toolbar` jako wersja AppCompat `Toolbar`. Następnie należy zmienić definicję klasy `MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

Można ustawić na pasku akcji do wersji AppCompat `Toolbar`, Zastąp wywołanie `SetActionBar` z `SetSupportActionBar`. W tym przykładzie tytuł również zostanie zmieniony z informacją, że wersja AppCompat `Toolbar` jest używany:

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

Na koniec zmień Minimum Android poziom na wartość wstępne interfejs typu lizak, która ma być obsługiwany (np. interfejsu API 19). 

Tworzenie aplikacji i uruchom go na wstępne interfejs typu lizak urządzenia lub emulatora systemu Android. Poniższy zrzut ekranu przedstawia wersję AppCompat **ToolbarFun** na 4 węzła uruchomionego KitKat (interfejsu API 19): 

[![Zrzut pełnego ekranu aplikacji uruchomionej na urządzeniu KitKat przedstawiono oba paski narzędzi](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

W przypadku biblioteki AppCompat kompozycji nie trzeba można przełączyć w wersji dla systemu Android &ndash; biblioteki AppCompat pozwala zapewnić spójne środowisko we wszystkich obsługiwanych wersjach systemu Android. 




## <a name="related-links"></a>Linki pokrewne

- [Interfejs typu lizak paska narzędzi (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat paska narzędzi (przykład)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
