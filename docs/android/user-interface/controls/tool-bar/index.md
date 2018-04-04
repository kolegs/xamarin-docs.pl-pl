---
title: Pasek narzędzi
description: 'Pasek narzędzi jest składnik paska akcji, który zapewnia większą elastyczność niż na pasku akcji domyślnej: może być umieszczony w dowolnym miejscu aplikacji, można zmienić jego rozmiaru i może używać schemat kolorów, który różni się od aplikacji motywu. Ponadto każdy ekran aplikacji może mieć wiele pasków narzędzi.'
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 08fa00b539bd5baca4f5d61b04419a76a4a72ab1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="toolbar"></a>Pasek narzędzi

_Pasek narzędzi jest składnik paska akcji, który zapewnia większą elastyczność niż na pasku akcji domyślnej: może być umieszczony w dowolnym miejscu aplikacji, można zmienić jego rozmiaru i może używać schemat kolorów, który różni się od aplikacji motywu. Ponadto każdy ekran aplikacji może mieć wiele pasków narzędzi._

 
## <a name="overview"></a>Omówienie

Element projektu klucza działalności Android jest *pasku akcji*. Na pasku akcji jest składnik interfejsu użytkownika, który jest używany do nawigacji, wyszukiwania, menu i znakowania w aplikacji systemu Android. W wersjach systemu Android przed Android 5.0 interfejs typu lizak, na pasku akcji (znanej także jako *paska aplikacji*) została składnika zalecane dla udostępniać tę funkcjonalność. 

`Toolbar` Widget (wprowadzona w systemie Android 5.0 interfejs typu lizak) można traktować jako generalizacji interfejsu paska akcji &ndash; ma zastąpić na pasku akcji. `Toolbar` Można używać w dowolnym miejscu układ aplikacji i jest znacznie więcej można dostosowywać niż pasku akcji. Poniższy zrzut ekranu przedstawia dostosowywane `Toolbar` przykład utworzone w tym przewodniku: 

[![Zrzut ekranu paska narzędzi z edycji, Zapisz i przepełnienie elementów menu](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

Istnieje kilka istotnych różnic między `Toolbar` i na pasku akcji: 

-   A `Toolbar` można umieścić w dowolnym miejscu w interfejsie użytkownika.

-   Na tym samym ekranie można wyświetlić wiele pasków narzędzi.

-   Jeśli są używane fragmenty, każdy fragment może mieć własny `Toolbar`. 

-   A `Toolbar` można skonfigurować, aby obejmować tylko częściowe szerokości ekranu. 

-   Ponieważ `Toolbar` nie jest powiązany do schemat kolorów decor okna działania, może mieć schemat wizualnie odrębnych kolorów. 

-   W odróżnieniu od na pasku akcji `Toolbar` nie ma ikony po lewej stronie. Jego menu z prawej strony użyj mniej miejsca. 

-   `Toolbar` Wysokość jest regulowany. 

-   Inne widoki mogą znajdować się wewnątrz `Toolbar`. 

A `Toolbar` może zawierać jeden lub więcej z następujących elementów: 

-   Przycisk nawigacji

-   Obraz logo marką

-   Tytuł i pomocniczą

-   Widoki niestandardowe

-   Menu akcji

-   Menu przeciążenia

Firmy Google [zaleceń dotyczących projektowania materiałów](https://material.google.com/) zaleca się korzystanie z tych elementów, aby udzielić aplikacji różne wygląd (zamiast polegać wyłącznie na ikony aplikacji i tytuł). 

W tym przewodniku dotyczą najbardziej często używanych `Toolbar` scenariusze:

-   Zastępowanie pasku akcji domyślne działanie `Toolbar`. 

-   Dodanie drugiej `Toolbar` do działania.

-   Przy użyciu **biblioteki obsługi systemu Android w wersji 7 AppCompat** biblioteki (nazywane *AppCompat* w dalszej części tego przewodnika) do wdrożenia `Toolbar` we wcześniejszych wersjach systemu android. 

 
 
## <a name="requirements"></a>Wymagania

`Toolbar` jest dostępna w Android 5.0 interfejs typu lizak (interfejs API 21) lub nowszym. Gdy w wersjach wcześniejszych niż Android 5.0 przeznaczonych dla systemu Android, użyj [biblioteki obsługi systemu Android w wersji 7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/), zapewniające wstecznie zgodna `Toolbar` obsługuje w pakiecie NuGet. 
[Zgodność narzędzi](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) wyjaśniono, jak użyć tej biblioteki. 




## <a name="related-links"></a>Linki pokrewne

- [Interfejs typu lizak paska narzędzi (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat paska narzędzi (przykład)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
