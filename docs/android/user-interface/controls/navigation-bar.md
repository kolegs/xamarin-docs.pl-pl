---
title: Pasek nawigacyjny
ms.topic: article
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 396ed31cba336976342a8dfb26f31eeda20cf494
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="navigation-bar"></a>Pasek nawigacyjny

Android 4 wprowadzono nową systemu użytkowników interface funkcję o nazwie *pasek nawigacyjny*, zapewniające formantów nawigacji na urządzeniach, które nie zawierają przyciski sprzętu **Home**, **Wstecz** , i **Menu**.
Poniższy zrzut ekranu przedstawia pasek nawigacyjny z głównego węzła urządzenia:

 [ ![Przykład paska nawigacji dla systemu Android](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png)

Kilka nowych flagi są dostępne umożliwiające sterowanie widoczność paska nawigacji i jego formantów, a także widoczność pasek System, który został wprowadzony w systemie Android 3. Flagi są zdefiniowane w `Android.View.View` klasy i są wymienione poniżej:

-   `SystemUiFlagVisible` &ndash; Pasek nawigacji staje się widoczny. 
-   `SystemUiFlagLowProfile` &ndash; DIMS układu kontrolek na pasku nawigacyjnym. 
-   `SystemUiFlagHideNavigation` &ndash; Ukrywa pasek nawigacji. 


Te flagi można zastosować do dowolnego widoku w hierarchii widoku przez ustawienie `SystemUiVisibility` właściwości. Jeśli wiele widoków ta właściwość ma wartość, system łączy je z operacji lub i zastosuje je tak długo, jak okna, w którym ustawiono flagi zachowuje fokus. Po usunięciu widoku zostaną także usunięte wszystkie flagi, które została ustawiona.

W poniższym przykładzie przedstawiono prostą aplikację, której, klikając jeden z przycisków zmienia `SystemUiVisibility`:

 [ ![Prezentacja Visible, niski profilu i ukryte SystemUiVisibility zrzuty ekranu](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png)

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
