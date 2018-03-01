---
title: "Układy z kartami"
description: "Omówienie z kartami układów w systemie Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/23/2017
ms.openlocfilehash: f8680cde2e5536495f33d571adea9980020a72fa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="tabbed-layouts"></a>Układy z kartami

<a name="Overview" />

## <a name="overview"></a>Omówienie

Karty są wzorzec interfejsu użytkownika popularnych aplikacji dla urządzeń przenośnych z powodu ich prostoty i użyteczność. Udostępniają one spójny i łatwy sposób przechodzić między różnymi ekranów w aplikacji. Android ma kilka interfejsów API dla interfejsów z kartami: 

-   **TabHost** &ndash; jest oryginalnym interfejsu API do tworzenia interfejsów użytkownika z kartami. A `TabHost` element widget zostanie dodany do układu i działa jako kontener dla widoków z kartami. Ten interfejs API ponieważ jest przestarzała i jego użycie nie jest zalecane. 

-   **Elementów nadrzędnych** &ndash; jest to część nowy zestaw interfejsów API wprowadzonej w 3.0 dla systemu Android (interfejs API na poziomie 11), celem jest zapewnienie spójną nawigacji i przełączania widoku interfejsu. Ma ona ponownie przenoszone do 2.2 systemu Android (interfejs API na poziomie 8) z [biblioteki obsługi systemu Android w wersji 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; wskazuje bieżący, następny i poprzedni strony `ViewPager`. `ViewPager` jest dostępna tylko za pośrednictwem [biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Aby uzyskać więcej informacji na temat `PagerTabStrip`, zobacz [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Pasek narzędzi** &ndash; `Toolbar` to składnik paska akcji nowszych i bardziej elastyczne, który zastępuje `ActionBar`. `Toolbar` jest dostępna w Android interfejs typu lizak 5.0 lub nowszej, a jest również dostępny do starszych wersji systemu android za pośrednictwem [biblioteki obsługi systemu Android w wersji 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) pakietu NuGet. 
    `Toolbar` jest obecnie składnika pasek zalecaną akcję do użycia w aplikacji systemu Android.
    Aby uzyskać więcej informacji, zobacz [narzędzi](~/android/user-interface/controls/tool-bar/index.md). 


Tych interfejsów API wizualnie bardzo różnią się i nie są zgodne ze sobą. Następujące ekranu pokazuje obrazu `TabHost` i `ActionBar` side-by-side: 

![Zrzuty ekranu TabHost po lewej stronie i elementów nadrzędnych po prawej stronie](images/image01.png)

Te niezgodne API istnieje ze względu na istotne zmiany interfejsu użytkownika od momentu 3.0 dla systemu Android (interfejs API na poziomie 11). Jeden z tych zmian interfejsu użytkownika jest [akcji paska wzorca projektowego](http://www.androidpatterns.com/uap_pattern/action-bar), wzorzec mają na celu dostarczenie łatwy dostęp do funkcji nawigacji i klucz w aplikacji. `ActionBar` Wprowadzono interfejs API do obsługi tego wzorca. 

`ActionBar` Interfejsu API jest prostsze i raczej zapewnia lepsze środowisko pracy użytkownika. Ponownie systemie Android 2,2, a jest to preferowany wybór dla aplikacji platformy Xamarin.Android. 

`TabHost` Interfejsu API jest zgodne przez wszystkie wersje systemu android, ale wymaga więcej wysiłku, aby użyć i nie jest zgodny z bieżącym [wytyczne interfejsu użytkownika dla systemu Android](http://developer.android.com/design/index.html). Deweloperzy są odradzane ze przy użyciu tego interfejsu API i powinno sprzyjać nowszej podrzędnego dla swoich aplikacji platformy Xamarin.Android. 


<a name="Introducing_ActionBarSherlock" />

## <a name="actionbarsherlock"></a>ActionBarSherlock

Przed elementów nadrzędnych interfejsu API zostały backported Android 2,2 deweloperów, którzy chciał nowszej wygląd i działanie interfejsu API elementów nadrzędnych, ale można używać biblioteki innych firm, [ActionBarSherlock](http://actionbarsherlock.com). ActionBarSherlock jest rozszerzeniem biblioteki obsługi systemu Android są zaprojektowane do Poprawka usterki systemu wzorca projektowego paska akcji do systemu Android 2.x. Podczas uruchamiania systemu Android 3.0 lub nowszego, ActionBarSherlock będą automatycznie używać natywnego `ActionBar` interfejsu API dostarczonych przez system Android. Starsze wersje systemu android użyje implementacja niestandardowych, które będzie używane środowisko przypominało wygląd i działanie nowszej `ActionBar` interfejsu API. [Składnika ActionBarSherlock](https://www.nuget.org/packages/xamstore-XamarinActionBarSherlock/) ułatwia dodawanie ActionBarSherlock do aplikacji platformy Xamarin.Android. 



## <a name="related-links"></a>Linki pokrewne

- [Omówienie TabHost](tab-host.md)
- [TabHost Walkthrough](~/android/user-interface/layouts/tab-layout/creating-a-tabbed-ui.md)
- [Elementów nadrzędnych.](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Pakiet NuGet AppCompat w wersji 7 biblioteki obsługi systemu android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Biblioteka appcompat w wersji 7](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
