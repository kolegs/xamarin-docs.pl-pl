---
title: Menu podręczne
description: Jak dodać menu podręcznego, który jest zakotwiczona do określonego widoku.
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: d7cadde88e9ae7ee30815ee9323785038dbb1a39
ms.sourcegitcommit: ecdc031e9e26bbbf9572885531ee1f2e623203f5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393662"
---
# <a name="popup-menu"></a>Menu podręczne

[Elementu PopupMenu](https://developer.xamarin.com/api/type/Android.Widget.PopupMenu/) (nazywane również _menu skrótów_) jest menu, w którym jest zakotwiczona określonego widoku. W poniższym przykładzie jedno działanie zawiera przycisk. Kiedy użytkownik naciska przycisk, jest wyświetlane menu podręczne trzech elementów:

[![Przykład aplikacji za pomocą przycisku i trzech elementów menu podręcznego](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)


## <a name="creating-a-popup-menu"></a>Tworzenie Menu podręczne

Pierwszym krokiem jest utworzenie pliku zasobu menu do menu i umieść go w **zasobów/menu**. Na przykład, następujący kod XML jest kod menu trzech elementów wyświetlanych na poprzednim zrzucie ekranu, **Resources/menu/popup_menu.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/item1"
          android:title="item 1" />
    <item android:id="@+id/item1"
          android:title="item 2" />
    <item android:id="@+id/item1"
          android:title="item 3" />
</menu>
```

Następnie utwórz wystąpienie obiektu `PopupMenu` i zakotwiczyć go do jej widok. Po utworzeniu wystąpienia `PopupMenu`, przekazać odwołanie do jej konstruktora `Context` oraz widok, do którego zostanie dołączony menu. W związku z menu podręcznego jest zakotwiczona do tego widoku podczas jego tworzenia.

W poniższym przykładzie `PopupMenu` jest tworzony w obsługi zdarzeń kliknij przycisk (który nosi nazwę `showPopupMenu`). Ten przycisk jest również w celu `PopupMenu` jest zakotwiczona, jak pokazano w poniższym przykładzie kodu:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

Na koniec należy z menu podręcznego *nadmuchany* zasobu menu, który został utworzony wcześniej. W poniższym przykładzie, wywołanie do menu [Inflate](https://developer.xamarin.com/api/member/Android.Views.LayoutInflater.Inflate/p/System.Int32/Android.Views.ViewGroup/) metoda jest dodawana i jego [Pokaż](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Show%28%29/) metoda jest wywoływana w celu wyświetlenia go:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```


## <a name="handling-menu-events"></a>Obsługa zdarzeń Menu

Gdy użytkownik wybierze element menu [MenuItemClick](https://developer.xamarin.com/api/event/Android.Widget.PopupMenu.MenuItemClick/) kliknij zdarzenie zostanie wygenerowany i menu zostanie odrzucony. Klikając w dowolnym miejscu poza menu będzie po prostu je zamknąć. W obu przypadkach, gdy menu jest odrzucane jego [DismissEvent](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Dismiss%28%29/) zostanie wygenerowany. Poniższy kod dodaje obsługę zdarzeń dla obu `MenuItemClick` i `DismissEvent` zdarzenia:

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
