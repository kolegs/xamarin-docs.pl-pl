---
title: Formanty dla systemu android (Widgets)
description: Bloki konstrukcyjne do tworzenia interfejsu użytkownika platformy Xamarin.Android
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 842fb1df2c9cc1aaf1a106687179a3730c2503bd
ms.sourcegitcommit: 7e4070bc104d612b6754ea35dd5a49c5c3d45f4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "32436716"
---
# <a name="android-controls-widgets"></a>Formanty dla systemu android (Widgets)

Rozszerzenie Xamarin.Android uwidacznia wszystkie natywnych kontrolek interfejsu użytkownika (widgets) dostarczane przez system Android. Te formanty można łatwo dodawać do aplikacji platformy Xamarin.Android przy użyciu narzędzia Android Designer lub programowo przy użyciu plików układu XML. Niezależnie od tego, w jakiej metody, możesz wybrać rozszerzenie Xamarin.Android uwidacznia wszystkie właściwości obiektu interfejsu użytkownika i metod w języku C#. Poniższe sekcje wprowadzenie najbardziej typowych kontrolek interfejsu użytkownika systemu Android i wyjaśniono, jak włączyć je do aplikacji platformy Xamarin.Android.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[Pasek akcji](~/android/user-interface/controls/action-bar.md) 

`ActionBar` jest pasek narzędzi, który wyświetla tytuł działania, nawigacji interfejsów i inne elementy interaktywne. Zazwyczaj w górnej części okna działania zostanie wyświetlony pasek akcji.

![Przykład elementów nadrzędnych.](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[Autouzupełnianie](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` jest elementem widoku tekst do edycji, który pokazuje sugestie uzupełniania automatycznie, gdy wpisywany przez użytkownika. W rozwijanych menu, z którego użytkownik może wybrać element, aby zastąpić zawartość pole edycji z zostanie wyświetlona lista sugestii.

![Przykład Autouzupełnianie](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[Przyciski](~/android/user-interface/controls/buttons/index.md)

Przyciski są elementy interfejsu użytkownika, które użytkownik naciska do wykonania akcji.

![Przykład przycisków](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Kalendarz](~/android/user-interface/controls/calendar.md)

`Calendar` Klasa jest używana do konwersji określonego wystąpienia w czas (milisekund wartość, która jest przesunięcie od początku epoki) na wartości, takie jak rok, miesiąc, godzina i dzień miesiąca i dnia następnym tygodniu.
`Calendar` obsługuje wiele opcji interakcji z danymi kalendarza, łącznie z możliwością odczytu i zapisu zdarzeń uczestników i przypomnienia. Przy użyciu dostawcy kalendarza w Twojej aplikacji, danych, które możesz dodać za pomocą interfejsu API pojawi się w aplikacji wbudowanej kalendarza, która jest dostarczany z programem Android.

![Przykład kalendarza](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` jest składnikiem interfejsu użytkownika, który prezentuje zawartość tekstu i obrazów w widokach, które przypominają kart. `CardView` jest implementowany jako `FrameLayout` widżet zaokrąglone rogi i w tle. Zazwyczaj `CardView` służy do prezentowania element pojedynczy wiersz w `ListView` lub `GridView` Wyświetl grupę.

![Przykładowy widok karty](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[Edytowanie tekstu](~/android/user-interface/controls/edit-text.md)

`EditText` jest elementem interfejsu użytkownika, który służy do wprowadzania i modyfikowanie tekstu.

![Przykład edycji tekstu](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[Galeria](~/android/user-interface/controls/gallery.md)

`Gallery` jest widżetu układu, który służy do wyświetlania elementów w poziomie przewijanej listy; bieżące zaznaczenie jej umieszcza w środku widoku.

![Przykład galerii](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[Pasek nawigacji](~/android/user-interface/controls/navigation-bar.md)

*Pasek nawigacyjny* zawiera formanty nawigacji na urządzeniach, które nie zawierają przyciski sprzętu dla **Home**, **ponownie**, i **Menu**.

![Przykład paska nawigacyjnego](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[Selektory](~/android/user-interface/controls/pickers/index.md)

*Selektora* elementów interfejsu użytkownika, które umożliwiają użytkownikom wyboru daty lub godziny przy użyciu okien dialogowych, które są dostarczane przez system Android.

![Przykład selektora](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[Menu podręczne](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` Służy do wyświetlania menu podręcznych, które są dołączone do określonego widoku.

![Przykład Menu podręczne](images/popup-menu.png)


## <a name="ratingbarandroiduser-interfacecontrolsratingbarmd"></a>[RatingBar](~/android/user-interface/controls/ratingbar.md)

Element `RatingBar` jest element interfejsu użytkownika, który wyświetla ocenę w gwiazdek.

![Przykład RatingBar](ratingbar-images/01-ratingbar.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[Pokrętło](~/android/user-interface/controls/spinner.md)

`Spinner` jest elementem interfejsu użytkownika, który zapewnia szybki sposób wyboru jednej wartości z zestawu. Jest simmilar do listy rozwijanej. 

![Przykład pokrętła](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[Przełącznik](~/android/user-interface/controls/switch.md)

`Switch` jest element interfejsu użytkownika, który umożliwia użytkownikowi przełączanie między dwoma stanami, jak na przykład lub WYŁĄCZONY. `Switch` Wartość domyślna to OFF.

![Przykład przełącznika](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` jest to widok używa 2D renderowanie przyspieszane sprzętowo, aby umożliwić filmu wideo lub OpenGL strumień zawartości mają być wyświetlane.

![Przykładowy widok tekstury](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

`Toolbar` Widżet (wprowadzona w systemie Android 5.0 Lollipop) mogą być uważane za generalizacji interfejsu pasek akcji &ndash; zamierza się zastąpić na pasku akcji. `Toolbar` Mogą być używane w dowolnym miejscu w układu aplikacji i jest znacznie szersze niż pasku akcji.

![Przykład paska narzędzi](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

`ViewPager` Jest kierownikiem układu, umożliwiająca użytkownikowi przerzucić lewy i prawy za pośrednictwem strony danych.

![Obiekt ViewPager z przykładu](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` jest elementem interfejsu użytkownika, która pozwala na utworzenie okna w celu wyświetlania stron sieci web (lub nawet utworzyć pełną przeglądarki).

![Przykładowy widok sieci Web](images/web-view.png)

