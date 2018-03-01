---
title: "Menu podręczne"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/18/2017
ms.openlocfilehash: 54b6809b7e27dc87be6d510e4a4b6071e4ae22e7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="popup-menu"></a>Menu podręczne

`PopupMenu` Klasa dodaje obsługę wyświetlanie menu podręczne, które są dołączone do określonego widoku. Na poniższej ilustracji przedstawiono zaprezentowany drugiego elementu wyróżnione tak samo, jak jest zaznaczony przy użyciu przycisku menu podręcznego:

 [ ![Przykład PopopMenu, z których trzy są trzy elementy](popup-menu-images/20-popupmenu.png)](popup-menu-images/20-popupmenu.png)

Android 4 dodano kilka nowych funkcji zapewniających `PopupMenu` ułatwiające bit do pracy z, to znaczy:

-   **Inflate** &ndash; zwiększyć metody jest teraz dostępna bezpośrednio w klasie elementu PopupMenu.
-   **DismissEvent** &ndash; klasy elementu PopupMenu ma teraz DismissEvent.

Spójrzmy na te ulepszenia. W tym przykładzie mamy pojedyncze działanie, która zawiera przycisk. Po kliknięciu przycisku menu podręczne jest wyświetlane, jak pokazano poniżej:

 [ ![Przykład aplikacji uruchomionej w emulatorze z przyciskiem oraz 3 elementu menu podręcznego](popup-menu-images/06-popupmenu.png)](popup-menu-images/06-popupmenu.png)

<a name="Creating_a_Popup_Menu" />

## <a name="creating-a-popup-menu"></a>Tworzenie Menu podręcznego

Kiedy możemy utworzyć wystąpienia `PopupMenu`, należy przekazać odwołanie do jego konstruktora `Context`, oraz widok, do której jest dołączona menu. W takim przypadku utworzymy `PopupMenu` w kliknij program obsługi zdarzeń dla naszych przycisku, który ma nazwę `showPopupMenu`.
Ten przycisk jest również widoku, do której firma Microsoft będzie dołączać `PopupMenu`, jak pokazano w poniższym kodzie:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
}
```

W 3 dla systemu Android, kod, aby zwiększyć menu z zasobem XML musi najpierw pobrać odwołanie do `MenuInflator`, a następnie wywołać jej `Inflate` metody o identyfikatorze zasobu kod XML, który zawiera menu i wystąpienia menu, aby zwiększyć do. Takie podejście nadal działa w systemie Android 4 i nowszym jako kod poniżej przedstawia:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.MenuInflater.Inflate (Resource.Menu.popup_menu, menu.Menu);
};
```

Począwszy od systemu Android 4, można teraz wywołać `Inflate` bezpośrednio na wystąpienie `PopupMenu`. Dzięki temu kod więcej krótkie, jak pokazano poniżej:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```

W powyższym kodzie po pompowania menu po prostu nazywamy `menu.Show` do jego wyświetlenia na ekranie.

<a name="Handling_Menu_Events" />

## <a name="handling-menu-events"></a>Obsługa zdarzeń Menu

Po wybraniu elementu menu `MenuItemClick` zostanie wygenerowany, zdarzeń i menu zostanie zamknięty. Naciskając w dowolnym miejscu poza menu będzie po prostu je zamknąć. W obu przypadkach, począwszy od systemu Android 4 odrzucania menu jego `DismissEvent` zostanie wygenerowany. Poniższy kod dodaje obsługę zdarzeń dla obu `MenuItemClick` i `DismissEvent` zdarzenia:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);

    menu.MenuItemClick += (s1, arg1) => {
        Console.WriteLine ("{0} selected", arg1.Item.TitleFormatted);
    };

    menu.DismissEvent += (s2, arg2) => {
        Console.WriteLine ("menu dismissed");
    };
            menu.Show ();
};
```



## <a name="related-links"></a>Linki pokrewne

- [PopupMenuDemo (przykład)](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/)
- [Wprowadzenie Sandwich lodów](http://www.android.com/about/ice-cream-sandwich/)
- [Platformy systemu android 4.0](http://developer.android.com/sdk/android-4.0.html)
