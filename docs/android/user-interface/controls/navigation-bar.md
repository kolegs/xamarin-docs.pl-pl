---
title: Pasek nawigacyjny
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 476e73906e4fbe01847740f90a8da701768b29db
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="navigation-bar"></a>Pasek nawigacyjny

Android 4 wprowadzono nową systemu użytkowników interface funkcję o nazwie *pasek nawigacyjny*, zapewniające formantów nawigacji na urządzeniach, które nie zawierają przyciski sprzętu **Home**, **Wstecz** , i **Menu**.
Poniższy zrzut ekranu przedstawia pasek nawigacyjny z głównego węzła urządzenia:

 [![Przykład paska nawigacji dla systemu Android](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

Kilka nowych flagi są dostępne umożliwiające sterowanie widoczność paska nawigacji i jego formantów, a także widoczność pasek System, który został wprowadzony w systemie Android 3. Flagi są zdefiniowane w `Android.View.View` klasy i są wymienione poniżej:

-   `SystemUiFlagVisible` &ndash; Pasek nawigacji staje się widoczny. 
-   `SystemUiFlagLowProfile` &ndash; DIMS układu kontrolek na pasku nawigacyjnym. 
-   `SystemUiFlagHideNavigation` &ndash; Ukrywa pasek nawigacji. 


Te flagi można zastosować do dowolnego widoku w hierarchii widoku przez ustawienie `SystemUiVisibility` właściwości. Jeśli wiele widoków ta właściwość ma wartość, system łączy je z operacji lub i zastosuje je tak długo, jak okna, w którym ustawiono flagi zachowuje fokus. Po usunięciu widoku zostaną także usunięte wszystkie flagi, które została ustawiona.

W poniższym przykładzie przedstawiono prostą aplikację, której, klikając jeden z przycisków zmienia `SystemUiVisibility`:

 [![Prezentacja Visible, niski profilu i ukryte SystemUiVisibility zrzuty ekranu](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

Kod, aby zmienić `SystemUiVisibility` ustawia właściwość `TextView` z każdego przycisku kliknij program obsługi zdarzeń, jak pokazano poniżej:

```csharp
var tv = FindViewById<TextView> (Resource.Id.systemUiFlagTextView);
var lowProfileButton = FindViewById<Button>(Resource.Id.lowProfileButton);
var hideNavButton = FindViewById<Button> (Resource.Id.hideNavigation);
var visibleButton = FindViewById<Button> (Resource.Id.visibleButton);
           
lowProfileButton.Click += delegate {
    tv.SystemUiVisibility =
        (StatusBarVisibility)View.SystemUiFlagLowProfile;
};
           
hideNavButton.Click += delegate {
    tv.SystemUiVisibility =
       (StatusBarVisibility)View.SystemUiFlagHideNavigation;        
};
           
visibleButton.Click += delegate {
    tv.SystemUiVisibility = (StatusBarVisibility)View.SystemUiFlagVisible;
}
```

Ponadto `SystemUiVisibility` zmienić zgłasza `SystemUiVisibilityChange` zdarzeń. Podobnie jak ustawienie `SystemUiVisibility` właściwości, program obsługi `SystemUiVisibilityChange` zdarzeń może być zarejestrowany dla każdego widoku w hierarchii. Na przykład poniższy kod używa `TextView` wystąpienia, aby zarejestrować dla zdarzenia:

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>Linki pokrewne

- [SystemUIVisibilityDemo (przykład)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [Wprowadzenie Sandwich lodów](http://www.android.com/about/ice-cream-sandwich/)
- [Platformy systemu android 4.0](http://developer.android.com/sdk/android-4.0.html)
