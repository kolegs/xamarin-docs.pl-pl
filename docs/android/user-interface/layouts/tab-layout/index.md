---
title: Układy z kartami
description: Omówienie z kartami układów w systemie Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/08/2017
ms.openlocfilehash: 53ed5f91583d43839e96388194aea8c0d6ac5315
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="tabbed-layouts"></a>Układy z kartami


## <a name="overview"></a>Omówienie

Karty są wzorzec interfejsu użytkownika popularnych aplikacji dla urządzeń przenośnych z powodu ich prostoty i użyteczność. Udostępniają one spójny i łatwy sposób przechodzić między różnymi ekranów w aplikacji. Android ma kilka interfejsów API dla interfejsów z kartami: 

-   **Elementów nadrzędnych** &ndash; jest to część nowy zestaw interfejsów API wprowadzonej w 3.0 dla systemu Android (interfejs API na poziomie 11), celem jest zapewnienie spójną nawigacji i przełączania widoku interfejsu. Ma ona ponownie przenoszone do 2.2 systemu Android (interfejs API na poziomie 8) z [biblioteki obsługi systemu Android w wersji 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; wskazuje bieżący, następny i poprzedni strony `ViewPager`. `ViewPager` jest dostępna tylko za pośrednictwem [biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Aby uzyskać więcej informacji na temat `PagerTabStrip`, zobacz [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Pasek narzędzi** &ndash; `Toolbar` to składnik paska akcji nowszych i bardziej elastyczne, który zastępuje `ActionBar`. `Toolbar` jest dostępna w Android interfejs typu lizak 5.0 lub nowszej, a jest również dostępny do starszych wersji systemu android za pośrednictwem [biblioteki obsługi systemu Android w wersji 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) pakietu NuGet. 
    `Toolbar` jest obecnie składnika pasek zalecaną akcję do użycia w aplikacji systemu Android.
    Aby uzyskać więcej informacji, zobacz [narzędzi](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="related-links"></a>Linki pokrewne

- [Karty materiału projektu -](https://material.io/guidelines/components/tabs.html)- [elementów nadrzędnych.](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Pakiet NuGet AppCompat w wersji 7 biblioteki obsługi systemu android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Biblioteka appcompat w wersji 7](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
