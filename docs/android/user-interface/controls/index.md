---
title: Formanty
description: Bloki konstrukcyjne do tworzenia interfejsów użytkownika platformy Xamarin.Android
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8994a8988c0e32e85aedcd9110e3583195843862
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="controls"></a>Formanty


Xamarin.Android przedstawia wszystkie kontrolki interfejsu użytkownika macierzysty (elementy widget) podane przez system Android. Tych kontrolek można łatwo dodać do aplikacji platformy Xamarin.Android przy użyciu narzędzia Projektant Android lub programowo przy użyciu plików układu XML. Niezależnie od wybranej metody Xamarin.Android przedstawia wszystkie właściwości obiektu interfejsu użytkownika i metody w języku C#. W poniższych sekcjach wprowadzenie najbardziej typowych formantów interfejsu użytkownika dla systemu Android i wyjaśniono, jak włączyć je do aplikacji platformy Xamarin.Android.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[Pasek akcji](~/android/user-interface/controls/action-bar.md) 

`ActionBar` jest narzędzi, która wyświetla tytuł działania, interfejsy nawigacji i inne elementy interakcyjne. Zazwyczaj w górnej części okna działania zostanie wyświetlony pasek akcji.

![Przykład elementów nadrzędnych.](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[Autouzupełnianie](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` jest element widoku można edytować tekst, który zawiera sugestie dotyczące uzupełniania automatycznie podczas wpisywania jest użytkownik. W menu, z którego użytkownik może wybrać element, aby zastąpić zawartość pola edycji z rozwijane zostanie wyświetlona lista sugestie.

![Przykład automatyczne uzupełnianie](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[Przyciski](~/android/user-interface/controls/buttons/index.md)

Przyciski są elementy interfejsu użytkownika, które użytkownik naciska do wykonania akcji.

![Przykład przycisków](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Kalendarz](~/android/user-interface/controls/calendar.md)

`Calendar` Klasa jest używana do konwertowania konkretnego wystąpienia w czas (milisekundy wartość, która jest przesunięcie od epoka) na wartości, takie jak roku, miesiąca, godziny, dnia miesiąca i dnia w następnym tygodniu.
`Calendar` obsługuje wiele opcji interakcji z danymi kalendarza, łącznie z możliwością odczytu i zapisu zdarzenia, uczestników i przypomnienia. Przy użyciu dostawcy kalendarza w aplikacji, dane, które można dodać za pomocą interfejsu API będą wyświetlane w aplikacji wbudowanych kalendarza dołączoną do systemu Android.

![Przykład kalendarza](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` to składnik interfejsu użytkownika, który przedstawia zawartość tekstowych i obrazów w widokach, które przypominają kart. `CardView` jest zaimplementowany jako `FrameLayout` elementu widget zaokrąglone narożniki i cienia. Zazwyczaj `CardView` służy do prezentowania elementu pojedynczy wiersz w `ListView` lub `GridView` grupy widoku.

![Przykładowy widok karty](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[Edytowanie tekstu](~/android/user-interface/controls/edit-text.md)

`EditText` jest elementem interfejsu użytkownika, który służy do wprowadzania i modyfikowanie tekstu.

![Przykład edycji tekstu](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[Galeria](~/android/user-interface/controls/gallery.md)

`Gallery` jest element widget układu, który jest używany do wyświetlania elementów w poziomie przewijanej listy; bieżące zaznaczenie go umieszcza środek widoku.

![Przykład galerii](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[Pasek nawigacji](~/android/user-interface/controls/navigation-bar.md)

*Pasek nawigacyjny* zawiera kontrolki na urządzeniach, które nie zawierają przyciski sprzętu **Home**, **ponownie**, i **Menu**.

![Przykład paska nawigacyjnego](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[Selektory](~/android/user-interface/controls/pickers/index.md)

*Selektorami* są elementy interfejsu użytkownika, które umożliwiają użytkownikom wybierz datę lub godzinę za pomocą okien dialogowych, które są dostarczane przez system Android.

![Przykład selektora](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[Menu podręczne](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` Służy do wyświetlania menu podręczne, które są dołączone do określonego widoku.

![Przykład Menu podręcznego](images/popup-menu.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[Pokrętło](~/android/user-interface/controls/spinner.md)

`Spinner` jest elementem interfejsu użytkownika, który umożliwia szybkie można wybrać jedną wartość z zestawu. Jest simmilar do listy rozwijanej. 

![Przykład pokrętła](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[Przełącznik](~/android/user-interface/controls/switch.md)

`Switch` jest elementem interfejsu użytkownika, który umożliwia użytkownikowi przełączanie się między dwoma stanami, jak na przykład lub wyłączone. `Switch` Wartość domyślna to OFF.

![Przykład przełącznika](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` jest to widok używa 2D renderowanie przyspieszane sprzętowo, aby umożliwić wideo lub OpenGL strumień zawartości, który będzie wyświetlany.

![Przykładowy widok tekstury](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

`Toolbar` Widget (wprowadzona w systemie Android 5.0 interfejs typu lizak) można traktować jako generalizacji interfejsu paska akcji &ndash; ma zastąpić na pasku akcji. `Toolbar` Można używać w dowolnym miejscu układ aplikacji i jest znacznie więcej można dostosowywać niż pasku akcji.

![Przykład paska narzędzi](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

`ViewPager` Jest manager układu, która pozwala użytkownikowi Przerzuć lewy i prawy za pośrednictwem strony danych.

![Przykład ViewPager](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` jest elementem interfejsu użytkownika, który pozwala utworzyć własne okna do wyświetlania stron sieci web (lub nawet utworzyć pełną przeglądarki).

![Przykładowy widok sieci Web](images/web-view.png)

