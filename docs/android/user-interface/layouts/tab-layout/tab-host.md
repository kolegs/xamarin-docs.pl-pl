---
title: "Układ karty z TabHost"
description: "W tym artykule zapewni wysokiego poziomu omówienie TabHost, starsze API używane do tworzenia układów z kartami w aplikacji platformy Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: ff61ca0a2bca466da3e33c93a17944915328b70c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="tab-layout-with-tabhost"></a>Układ karty z TabHost

_W tym artykule zapewni wysokiego poziomu omówienie TabHost, starsze API używane do tworzenia układów z kartami w aplikacji platformy Xamarin.Android._

<a name="Overview" />

## <a name="overview"></a>Omówienie

> [!NOTE]
> **Uwaga:** `TabHost` jest stary interfejsu API, która została zastąpiona przez firmę Google. Deweloperzy są zachęcani do kompilacji z kartami aplikacji przy użyciu [elementów nadrzędnych](~/android/user-interface/controls/action-bar.md). `ActionBar` Jest dostępna we wszystkich wersji systemu android. Została wprowadzona w 3.0 dla systemu Android (interfejs API na poziomie 11), a była powrotem przenoszone do 2.2 systemu Android (interfejs API na poziomie 8) i Android 2.3 (interfejs API na poziomie 10) w [w wersji 7 AppCompat biblioteki](http://developer.android.com/tools/support-library/features.html#v7-appcompat), które są dostępne dla platformy Xamarin.Android przy użyciu [Xamarin Biblioteka obsługi systemu android - 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) pakietu.

`TabHost` Jest starsze, oryginalnego interfejsu API tworzenia użytkownika z kartami interfacesIt jest najbardziej odpowiednie dla aplikacji platformy Xamarin.Android, który musi obsługiwać Android 2.2 i Android 2.3 i nie można użyć **ActionBarSherlock**.
Następujące składniki pięć są wszystkie związane z `TabHost` interfejsu API:

-  **TabActivity** &ndash; to specjalne widoku, który działa jako kontener dla `TabHost` (opisanych poniżej).

-  **TabHost** &ndash; to kontener dla z kartami interfejsu użytkownika. Zawiera dwa elementy podrzędne: listę etykiet i a `FrameLayout` który wyświetla zawartość karty.

-  **TabWidget** &ndash; tego formantu zostanie wyświetlona lista kartę etykiety, po jednej dla każdej karty w `TabHost` . Każda karta `TabHost` są reprezentowane przez `TabHost.TabSpec` obiektu. Ten obiekt zawiera metadanych dla każdej karty. Gdy użytkownik wybierze kartę `TabHost` odpowiada zaznaczenie, wyświetlając odpowiednią kartę.

-  **FrameLayout** &ndash; z kartami interfejsu użytkownika musi mieć `FrameLayout` zawartych wewnątrz `TabHost`. Służy do wyświetlenia zawartości karty.

-  **Działania lub widoki** &ndash; po wybraniu karty wyświetla działanie lub widok w `FrameLayout`.

Na poniższym diagramie przedstawiono, jak wszystkie te składniki są powiązane ze sobą:

![Diagram pokazujący układu ramki w ramach TabWidget w TabHost](tab-host-images/image03.png)

Karta zawartość może być działania lub widoki. Widoki są stosunkowo nieskomplikowane i prostego, ale może spowodować powstanie wielu niepowiązanych kodu ko habitating w działaniu. To spowoduje niską rozdzielenie problemy i przeglądarek klasy, która jest trudna do zachowania. Z kolei działania wymagają zasobów systemowych, ale pozwala na bardziej podejściu przy logiki dla każdej karty z własnym różne klasy.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono składniki wysokiego poziomu starszej `TabHost` interfejsu API dla systemów Android i ich wszystkie relacje ze sobą.



## <a name="related-links"></a>Linki pokrewne

- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabHost.TabSpec](https://developer.xamarin.com/api/type/Android.Widget.TabHost+TabSpec/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Pakiet NuGet AppCompat w wersji 7 biblioteki obsługi systemu android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Biblioteka appcompat w wersji 7](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
