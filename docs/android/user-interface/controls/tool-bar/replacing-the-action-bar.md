---
title: "Zastępowanie na pasku akcji"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: e71c6ea816b8b732d21148db32fd9395732dd4c0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="replacing-the-action-bar"></a>Zastępowanie na pasku akcji


## <a name="overview"></a>Omówienie

Jedną z najbardziej typowych używa `Toolbar` ma zastąpić na pasku akcji domyślnej niestandardowej `Toolbar` (gdy tworzony jest nowy projekt dla systemu Android, używa na pasku akcji domyślnej). Ponieważ `Toolbar` oferuje możliwość dodawania marką logo, tytułów elementów menu, przycisków nawigacji i nawet niestandardowych widoków do części paska aplikacji działania interfejsu użytkownika, oferuje znaczne uaktualniania na pasku akcji domyślnej.

Aby zastąpić pasku akcji domyślnej aplikacji z `Toolbar`: 

1.  Utwórz motyw niestandardowy i modyfikowanie właściwości aplikacji, tak aby były używane tę nową kompozycję. 

2.  Wyłącz `windowActionBar` atrybutu w niestandardowej kompozycji i Włącz `windowNoTitle` atrybutu.

3.  Zdefiniuj układ `Toolbar`.

4.  Obejmują `Toolbar` układu w działaniu **Main.axml** pliku układu. 

5.  Dodaj kod do działania `OnCreate` metodą lokalizowania `Toolbar` i Wywołaj `SetActionBar` do zainstalowania `ToolBar` jako pasek akcji.

W poniższych sekcjach opisano ten proces. Prosta aplikacja jest tworzona i zastępuje jego pasku akcji z dostosowany `Toolbar`. 



## <a name="start-an-app-project"></a>Projekt aplikacji

Utwórz nowy projekt dla systemu Android o nazwie **ToolbarFun** (zobacz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) uzyskać więcej informacji dotyczących tworzenia nowego projektu systemu Android). Po utworzeniu tego projektu, Ustaw poziom interfejsu API systemu Android docelowy i co najmniej **Android 5.0 (21 poziom interfejsu API — typu lizak)**. Aby uzyskać więcej informacji na temat ustawienie wersji Android poziomów zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md). Wbudowane i uruchamianie aplikacji wyświetla na pasku akcji domyślnej w tym zrzut ekranu: 

[![Zrzut ekranu przedstawiający pasek Akcja domyślna](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)



## <a name="create-a-custom-theme"></a>Utwórz kompozycję niestandardową

Otwórz **zasobów/wartości** katalogu i Utwórz nowy plik o nazwie **styles.xml**. Zastąp jego zawartość XML następujące: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="@android:style/Theme.Material.Light.DarkActionBar">
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowActionBar">false</item>
    <item name="android:colorPrimary">#5A8622</item>
  </style>
</resources>
```

Plik XML definiuje nowy motyw niestandardowy o nazwie **MyTheme** opartego na **Theme.Material.Light.DarkActionBar** motyw w interfejs typu lizak. `windowNoTitle` Atrybut ma ustawioną `true` Aby ukryć pasek tytułu: 

```xml
<item name="android:windowNoTitle">true</item>
```

Aby wyświetlić narzędzi niestandardowych, domyślnie `ActionBar` musi zostać wyłączone: 

```xml
<item name="android:windowActionBar">false</item>
```

Oliwkowozielony `colorPrimary` ustawienie jest stosowane do kolor tła paska narzędzi: 
 
```xml
<item name="android:colorPrimary">#5A8622</item>
```

Edytuj **Properties/AndroidManifest.xml** i dodaj następującą `android:theme` atrybutu `<application>` element, aby aplikacja korzysta z `MyTheme` motyw niestandardowy: 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

Aby uzyskać więcej informacji dotyczących stosowania niestandardowy motyw do aplikacji, zobacz [przy użyciu niestandardowych kompozycji](~/android/user-interface/material-theme.md#customtheme). 



## <a name="define-a-toolbar-layout"></a>Zdefiniuj Układ paska narzędzi

W **zasobów/układ** katalogu, Utwórz nowy plik o nazwie **toolbar.xml**. Zastąp jego zawartość XML następujące: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?android:attr/actionBarSize"
    android:background="?android:attr/colorPrimary"
    android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"/>
```

Plik XML definiuje niestandardowe `Toolbar` zastępuje domyślny pasek akcji. Minimalna wysokość `Toolbar` ma ustawioną wartość rozmiaru na pasku akcji, która zastępuje ona: 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

Kolor tła `Toolbar` ustawiono Oliwkowozielony koloru zdefiniowanego wcześniej w **styles.xml**:

```csharp
android:background="?android:attr/colorPrimary"
```

Począwszy od interfejs typu lizak, `android:theme` atrybut może służyć do określania stylu poszczególnych widoku. `ThemeOverlay.Material` Motywów wprowadzone w interfejs typu lizak umożliwiają nakładki domyślnie `Theme.Material` kompozycje, zastępując odpowiednie atrybuty, aby je jasny lub ciemny. W tym przykładzie `Toolbar` używa ciemnego motywu jasny kolor jest jego zawartość: 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

To ustawienie jest używane, tak aby elementy menu, natomiast ciemniejszego kolor tła.



## <a name="include-the-toolbar-layout"></a>Obejmują Układ paska narzędzi

Przeprowadź edycję pliku układu **Resources/layout/Main.axml** i zastąp jego zawartość XML następujące:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
    <Button
        android:id="@+id/MyButton"
        android:layout_below="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Hello World, Click Me!" />
</RelativeLayout>
```

Ten układ zawiera `Toolbar` zdefiniowane w **toolbar.xml** i używa `RelativeLayout` Aby określić, że `Toolbar` ma być umieszczone na górze interfejsu użytkownika (p. wyżej przycisku). 



## <a name="find-and-activate-the-toolbar"></a>Znajdź i uaktywnić pasek narzędzi

Edytuj **MainActivity.cs** i dodaj następującą instrukcję using:

```csharp
using Android.Views;
```

Ponadto Dodaj następujące wiersze kodu na końcu `OnCreate` metody:

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

Ten kod znajduje `Toolbar` i wywołania `SetActionBar` , aby `Toolbar` spowoduje przejście na domyślne działania Pasek właściwości. Tytuł paska narzędzi jest zmieniana na **Moje narzędzi**. W tym przykładzie kodu `ToolBar` można odwoływać się bezpośrednio jako pasek akcji. Kompilowanie i uruchamianie tej aplikacji &ndash; dostosowywane `Toolbar` jest wyświetlany zamiast na pasku akcji domyślne: 

[![Zrzut ekranu przedstawiający niestandardowych narzędzi ze schematem kolor zielony](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

Zwróć uwagę, że `Toolbar` wyglądzie niezależnie od `Theme.Material.Light.DarkActionBar` motywu, która jest stosowana do pozostałej części aplikacji. 


 
## <a name="add-menu-items"></a>Dodawanie elementów Menu 

W tej sekcji menu są dodawane do `Toolbar`. Górny obszar prawym `ToolBar` jest zarezerwowana dla elementów menu &ndash; każdego elementu menu (nazywane również *elementu akcji*) można wykonać akcji w obrębie bieżącego działania lub może wykonywać akcję w imieniu całej aplikacji. 

Aby dodać menu `Toolbar`: 

1.  Dodawanie ikon menu (jeśli jest to wymagane) do `mipmap-` folderów projektu aplikacji. Google zawiera zestaw wolnego menu ikony na [materiału ikony](https://design.google.com/icons/) strony. 

2.  Zdefiniuj zawartość elementów menu przez dodanie nowego pliku zasobu menu w obszarze **zasobów/menu**. 

3.  Implementowanie `OnCreateOptionsMenu` metody działania &ndash; tej metody nadyma elementów menu. 

4.  Implementowanie `OnOptionsItemSelected` metody działania &ndash; ta metoda wykonuje akcję jest wybrany element menu. 

Poniższe sekcje pokazują ten proces przez dodanie **Edytuj** i **zapisać** elementy menu do dostosowywane `Toolbar`. 



### <a name="install-menu-icons"></a>Zainstaluj ikony Menu

Kontynuowanie `ToolbarFun` przykładową aplikację, Dodaj menu ikony do projektu aplikacji. Pobierz [icons.zip narzędzi](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons.zip?raw=true) i Rozpakuj go. Skopiuj zawartość wyodrębnionego *mipmap -* foldery do projektu *mipmap -* foldery znajdujące się w **ToolbarFun/zasoby** i dołączyć każdy plik dodany ikony w projekcie.


### <a name="define-a-menu-resource"></a>Zdefiniuj zasób Menu

Utwórz nową **menu** podkatalogu w obszarze **zasobów**. W **menu** podkatalogu, Utwórz nowy plik zasobu menu o nazwie **top_menus.xml** i zastąp jego zawartość XML następujące: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       android:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       android:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       android:showAsAction="never"
       android:title="Preferences" />
</menu>
```

Plik XML tworzy trzy elementy menu:

-   **Edytuj** element menu, która używa `ic_action_content_create.png` ikona (ołówka). 

-   A **zapisać** element menu, która używa `ic_action_content_save.png` ikona (dyskietki). 

-   A **preferencje** element menu, który nie ma ikony.

`showAsAction` Atrybuty **Edytuj** i **zapisać** elementy menu są ustawione na `ifRoom` &ndash; to ustawienie powoduje, że te elementy menu w wynikach `Toolbar` Jeśli istnieje wystarczające miejsce na ich do wyświetlenia. **Preferencje** ustawia element menu `showAsAction` do `never` &ndash; powoduje **preferencje** menu pojawią się w *przepełnienie* menu (trzy punktów w pionie). 


### <a name="implement-oncreateoptionsmenu"></a>Implementowanie OnCreateOptionsMenu

Dodaj następującą metodę do **MainActivity.cs**:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android wywołania `OnCreateOptionsMenu` metody, dzięki czemu aplikacja może określić zasobów menu dla działania. W przypadku tej metody **top_menus.xml** zasobów jest zwiększony do przekazanych `menu`. Ten kod powoduje, że nowe **Edytuj**, **zapisać**, i **preferencje** elementów menu w wynikach `Toolbar`. 



### <a name="implement-onoptionsitemselected"></a>Implementowanie OnOptionsItemSelected

Dodaj następującą metodę do **MainActivity.cs**:

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

Po naciśnięciu pozycji menu Android wywołuje `OnOptionsItemSelected` — metoda i przebiegów w elemencie menu, które zostały wybrane. W tym przykładzie wdrożenia wyświetli alert, aby wskazać, który element menu został dotknięciu. 

Tworzenie i uruchamianie `ToolbarFun` nowych elementów menu na pasku narzędzi. `Toolbar` Zostaną wyświetlone trzy ikony menu, w tym zrzut ekranu: 

[![Diagram ilustrujący lokalizacje edycji, zapisywania i przepełnienie elementów menu](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

Gdy podsłuchu użytkownika **Edytuj** element menu, wyskakujące jest wyświetlany w celu wskazania, że `OnOptionsItemSelected` wywołano metodę: 

[![Zrzut ekranu wyskakujące wyświetlany jest wybrany element edycji](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

Gdy użytkownik naciska menu przeciążenia **preferencje** element menu jest wyświetlany. Zazwyczaj mniej typowe akcje powinny być umieszczone w taki sposób, w menu przeciążenia &ndash; w tym przykładzie użyto menu przeciążenia dla **preferencje** ponieważ tyle razy, nie jest używany jako **Edytuj** i  **Zapisz**: 

[![Zrzut ekranu preferencje element menu, który jest wyświetlany w menu przeciążenia](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Aby uzyskać więcej informacji na temat menu systemu Android, zobacz Android Developer [menu](https://developer.android.com/guide/topics/ui/menus.html) tematu. 
 



## <a name="related-links"></a>Linki pokrewne

- [Interfejs typu lizak paska narzędzi (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat paska narzędzi (przykład)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
