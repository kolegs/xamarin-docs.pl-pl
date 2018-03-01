---
title: "Dodawanie drugiego paska narzędzi"
ms.topic: article
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: b471742ae9fb365d75e8dd3ca0f93f5e55208f19
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-second-toolbar"></a>Dodawanie drugiego paska narzędzi

<a name="overview" />

## <a name="overview"></a>Omówienie 

`Toolbar` Zrobić więcej niż Zastąp na pasku akcji &ndash; go może być używany wielokrotnie w ramach działania, można być dostosowane do umieszczenia w dowolnym miejscu na ekranie i może być skonfigurowana do span z częściowa szerokości ekranu. Poniższe przykłady przedstawiają sposób utworzyć drugi `Toolbar` i umieść go w dolnej części ekranu. To `Toolbar` implementuje **kopiowania**, **Wytnij**, i **Wklej** elementów menu. 

<a name="define_second" />

## <a name="define-the-second-toolbar"></a>Zdefiniuj drugi paska narzędzi 

Przeprowadź edycję pliku układu **Main.axml** i zastąp jego zawartość z następującego kodu XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:id="@+id/main_content"
        android:layout_below="@id/toolbar">
      <ImageView
          android:layout_width="fill_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
      <Toolbar
          android:id="@+id/edit_toolbar"
          android:minHeight="?android:attr/actionBarSize"
          android:background="?android:attr/colorAccent"
          android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
          android:layout_width="match_parent"
          android:layout_height="wrap_content" />
    </LinearLayout>
</RelativeLayout>
```

Plik XML dodaje drugi `Toolbar` do dołu ekranu z pustą `ImageView` wypełnianie środka ekranu. Wysokość tego `Toolbar` ustawiono wysokość paska akcji: 

```xml
android:minHeight="?android:attr/actionBarSize"
```

Kolor tła `Toolbar` ma ustawioną wartość kolor akcentu, który zostanie zdefiniowany obok:

```xml
android:background="?android:attr/colorAccent
```

Powiadomienie tego `Toolbar` opiera się na innej kompozycji (**ThemeOverlay.Material.Dark.ActionBar**) niż używany przez `Toolbar` utworzone w [zastępowanie na pasku akcji](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash;nie jest ona powiązana decor okna działania lub motywu używanego w pierwszym `Toolbar`.

Edytuj **Resources/values/styles.xml** i dodaj następujące kolor akcentu do definicji stylu: 

```xml
<item name="android:colorAccent">#C7A935</item>
```

Dzięki temu dolnym pasku narzędzi żółte ciemnym. Tworzenie i uruchamianie aplikacji wyświetla pusty drugi paska narzędzi w dolnej części ekranu: 

[![Zrzut ekranu przedstawiający aplikację z żółtym drugi paska narzędzi w dolnej części ekranu](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png)


<a name="second_menus" />
 
## <a name="add-edit-menu-items"></a>Dodawanie elementów Menu Edycja 

W tej sekcji wyjaśniono, jak dodać elementy menu Edycja w dół `Toolbar`. 

Aby dodać elementy menu do dodatkowej `Toolbar`: 

1.  Dodawanie ikon menu `mipmap-` folderów projektu aplikacji (jeśli jest to wymagane).

2.  Zdefiniuj zawartość elementów menu przez dodanie dodatkowych menu Plik zasobów do **zasobów/menu**. 

3.  W działaniu `OnCreate` metody, Znajdź `Toolbar` (wywołując `FindViewById`) i zwiększyć `Toolbar`w menu.

4.  Implementowanie obsługi kliknięcia w `OnCreate` dla nowych elementów menu. 

Poniższe sekcje pokazują, ten proces szczegółowo: **Wytnij**, **kopiowania**, i **Wklej** elementy menu są dodawane do dołu `Toolbar`. 


<a name="second_resource" />

### <a name="define-the-edit-menu-resource"></a>Zdefiniuj zasób Menu Edycja

W **zasobów/menu** podkatalogu, Utwórz nowy plik XML o nazwie **edit_menus.xml** i Zastąp zawartość XML następujące:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Tworzy plik XML **Wytnij**, **kopiowania**, i **Wklej** elementów menu (przy użyciu ikony, które zostały dodane do `mipmap-` folderów w [zastępowanie na pasku akcji ](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).


<a name="inflate_menus" />

### <a name="inflate-the-menus"></a>Podniesienie menu

Na koniec `OnCreate` metody w **MainActivity.cs**, Dodaj następujące wiersze kodu: 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

Lokalizuje ten kod `edit_toolbar` zdefiniowany w widoku **Main.axml**, ustawia jego tytuł **edycji**i nadyma elementy jego menu (zdefiniowany w **edit_menus.xml**). Definiuje menu kliknij program obsługi, który wyświetla wyskakujące, aby wskazać, jaką ikonę edycji został dotknięciu. 

Skompiluj i uruchom aplikację. Po uruchomieniu aplikacji, tekst i ikony dodanych wcześniej będą wyświetlane w sposób pokazany poniżej: 

[![Diagram dolnym pasku narzędzi z ikonami wycinania, kopiowania i wklejania](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png)

Naciskając **Wytnij** ikonę menu powoduje, że następujące wyskakujące do wyświetlenia: 

[![Zrzut ekranu wyskakujące wskazujący, że dotknięciu ikonę menu wycinania](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png)

Naciskając elementów menu na obu narzędzi wyświetla wynikowy wyskakujące powiadomienia: 

[![Zrzuty ekranu wyskakujące powiadomienia dla zapisać kopię i wkleić elementy menu są dotknięciu](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png)


<a name="up_button" />

## <a name="the-up-button"></a>Przycisku do góry 

Większość aplikacji systemu Android polegać na **ponownie** przycisk nawigacji aplikacji; naciśnięcie **ponownie** przycisk umożliwia przejście do poprzedniego ekranu użytkownika.
Jednak możesz zapewnienie **się** przycisku, który ułatwia użytkownikom Przejdź "nawet" do ekranu głównego aplikacji. Gdy użytkownik wybierze **się** przycisku, użytkownik zostanie przesunięty do wyższego poziomu w hierarchii aplikacji &ndash; oznacza to, że aplikacja POP użytkownik ponownie wielu działań w stosie przechodzenia wstecz zamiast odwiedzonych z powrotem wyświetlanie Działanie. 

Aby włączyć **się** przycisk w drugim działanie, które używa `Toolbar` jako jego pasku akcji wywołać `SetDisplayHomeAsUpEnabled` i `SetHomeButtonEnabled` metod w działaniu drugi `OnCreate` — metoda:

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

[Obsługuje narzędzi w wersji 7](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/) przykładowy kod przedstawia **się** przycisk w akcji. W tym przykładzie (który korzysta z biblioteki AppCompat opisem) implementuje drugi działanie, które korzysta z narzędzi **się** przycisk hierarchiczna nawigacji Wstecz do poprzedniego działania. W tym przykładzie `DetailActivity` włącza przycisk Strona główna **się** przycisku, wprowadzając następujące `SupportActionBar` wywołania metody: 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

Gdy użytkownik nawiguje między `MainActivity` do `DetailActivity`, `DetailActivity` Wyświetla **się** przycisk (Strzałka w lewo wskazującego), jak pokazano na zrzucie ekranu:

[![Przykład zrzut ekranu przycisku po lewej stronie Strzałka w górę na pasku narzędzi](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png)

Naciskając to **się** przycisku powoduje, że aplikacja powrócić do `MainActivity`. W bardziej złożonych aplikacji z różnych poziomów hierarchii naciskając przycisk ten zwróci użytkownika do następnego najwyższego poziomu w aplikacji, a nie do poprzedniego ekranu. 



## <a name="related-links"></a>Linki pokrewne

- [Interfejs typu lizak paska narzędzi (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat paska narzędzi (przykład)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
