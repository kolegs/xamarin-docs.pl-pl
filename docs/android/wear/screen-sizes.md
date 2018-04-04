---
title: Praca z rozmiaru ekranu
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: 12272b6db10249fb3750edd853ebb01a99586427
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-screen-sizes"></a>Praca z rozmiaru ekranu

Urządzenia z systemem android zużycia może mieć prostokątne lub round wyświetlania, które również mogą mieć różne rozmiary.

![Wyświetla zrzuty ekranu prostokątne i zaokrąglanie zużycie](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>Identyfikowanie typu ekranu

Biblioteka obsługi zużycia zawiera niektóre formanty, które ułatwiają wykrywania i dostosowania do kształtów inny ekran, takich jak `WatchViewStub` i `BoxInsetLayout`.

Należy pamiętać, że niektóre z innych obsługuje biblioteki formantów (takie jak `GridViewPager`) *automatycznie* Wykryj kształt ekranu się i nie można dodać, zgodnie z poniższym opisem elementów formantów podrzędnych.

### <a name="watchviewstub"></a>WatchViewStub

Zobacz [WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/) przykład, aby zobaczyć sposób wykrywania typu ekranu i wyświetlania różnych układu dla każdego typu.

Plik główny układ zawiera `android.support.wearable.view.WatchViewStub` który odwołuje się do różnych układów prostokątne i zaokrąglanie ekrany przy użyciu `app:rectLayout` i `app:roundLayout` atrybuty:

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

Rozwiązanie zawiera różne układy dla każdego stylu, który będzie wybierany w czasie wykonywania:

![Pliki wyświetlany w obszarze zasobów/układu](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>BoxInsetLayout

Zamiast tworzyć układów dla każdego typu ekranu, może spowodować pojedynczego widoku, który dostosowuje się do ekranów prostokątne lub round.

To [przykład Google](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) przedstawia sposób użycia `BoxInsetLayout` do użycia na ekranach zarówno prostokątne i zaokrąglanie ten sam układ.


## <a name="wear-ui-designer"></a>Nosić projektanta interfejsu użytkownika

Projektant Android Xamarin obsługuje zarówno prostokątne i zaokrąglanie ekrany:

![Wybieranie ekranu kwadratowe zużycia systemu Android w Projektancie Xamarin Android](screen-sizes-images/design-screen-type.png)

Powierzchnię projektu w stylu prostokątne jest następujący:

![Powierzchni projektu w stylu prostokątne](screen-sizes-images/design-rect.png) 

Powierzchnię projektu w stylu Rundy jest następujący:

![Powierzchni projektu w stylu okrągłych](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>Symulator nosić

**Menedżera emulatora Google** zawiera definicje urządzenia, na ekranie obu typów. Można utworzyć prostokątne i zaokrąglanie emulatory do testowania aplikacji.

![Nosić definicje urządzenia wyświetlany w Menedżerze emulatorów Google](screen-sizes-images/emulator-devices.png)

Emulator spowoduje, że takie dla prostokątnego ekranu:

![Emulator renderowania prostokątne ekranu](screen-sizes-images/recipe-2.png) 

Będą zawierały takie round ekranu:

![Emulator renderowania round ekranu](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>Video

[Pełny ekran aplikacji dla systemu Android zużycia](https://www.youtube.com/watch?v=naf_WbtFAlY) z [developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw).

