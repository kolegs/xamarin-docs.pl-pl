---
title: ViewPager
description: "ViewPager jest Menedżer układu, umożliwiające wdrożenie gestural nawigacji. Gestural nawigacji umożliwia użytkownikowi Przejdź lewy i prawy kroków strony danych. Ten przewodnik opisuje sposób nadawania gestural nawigacja z użyciem ViewPager, z włączonymi i wyłączonymi fragmenty. Zawiera ona również opis sposobu dodawania wskaźniki strony przy użyciu PagerTitleStrip i PagerTabStrip."
ms.topic: article
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 5e2f93eea970a15df03b00cc962ca7482624973d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="viewpager"></a>ViewPager

_ViewPager jest Menedżer układu, umożliwiające wdrożenie gestural nawigacji. Gestural nawigacji umożliwia użytkownikowi Przejdź lewy i prawy kroków strony danych. Ten przewodnik opisuje sposób nadawania gestural nawigacja z użyciem ViewPager, z włączonymi i wyłączonymi fragmenty. Zawiera ona również opis sposobu dodawania wskaźniki strony przy użyciu PagerTitleStrip i PagerTabStrip._

 
## <a name="overview"></a>Omówienie

Typowy scenariusz w rozwoju aplikacji jest potrzeba użytkownikom gestural nawigacji między widokami tego samego poziomu. W tej metody użytkownik swipes lewej lub prawej strony dostępu do zawartości (na przykład Kreator konfiguracji lub pokazu slajdów). Widoki te Przejdź można tworzyć przy użyciu `ViewPager` widget, dostępne w [biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). `ViewPager` Jest element widget układu składają się z wielu widoków podrzędne, gdzie każdy widok podrzędny stanowi strony w układzie: 

[![Zrzuty ekranu TreePager aplikacji za pomocą przykład przejdź pozioma](images/01-intro-sml.png)](images/01-intro.png#lightbox)

Zazwyczaj `ViewPager` jest używany w połączeniu z [fragmenty](https://developer.xamarin.com/guides/android/platform_features/fragments/); jednak niektórych sytuacjach, w których warto użyć `ViewPager` bez dodano złożoność `Fragment`s.

`ViewPager` używa wzorca karty dostarczenia widoków do wyświetlenia. Karta została użyta w tym miejscu jest podobny do używanego przez [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash; dostarczyć implementację `PagerAdapter` do generowania strony który `ViewPager` wyświetla dla użytkownika. Stron wyświetlanych przez `ViewPager` może być `View`s lub `Fragment`s. Gdy `View`s są wyświetlane, karta podklasy Android przez `PagerAdapter` klasy podstawowej. Jeśli `Fragment`s są wyświetlane, karta podklasy Android przez `FragmentPagerAdapter`. Obejmuje także biblioteki obsługi systemu Android `FragmentPagerAdapter` (podklasą `PagerAdapter`) ułatwiające łączenie szczegóły `Fragment`s do danych. 

W tym przewodniku zademonstrowano obu podejść: 

-   W [Viewpager z widokami](~/android/user-interface/controls/view-pager/viewpager-and-views.md), [TreePager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) aplikacji zaprojektowano w celu zademonstrowania sposobu użycia `ViewPager` Aby wyświetlić widoki w drzewie katalogu (Galeria obrazów drzew liściaste i wiecznie zielone). 
    `PagerTabStrip`  i `PagerTitleStrip` służą do wyświetlania tytułów, które pomagają w nawigacji strony.

-   W [Viewpager z fragmenty](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md), nieco bardziej złożone [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) aplikacji zaprojektowano w celu zademonstrowania sposobu użycia `ViewPager` z `Fragment`s do tworzenia aplikacji, która stanowi problem matematyczne jako karty Flash i odpowiada na dane wejściowe użytkownika. 


## <a name="requirements"></a>Wymagania

Aby użyć `ViewPager` w projekcie aplikacji, musisz zainstalować [biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) pakietu. Aby uzyskać więcej informacji na temat instalowania pakietów NuGet, zobacz [wskazówki: w tym NuGet w projekcie](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 

 
## <a name="architecture"></a>Architektura

Trzy składniki są używane do wykonywania gestural nawigacja z użyciem `ViewPager`:

-   ViewPager
-   Adapter
-   Pager Indicator

Każdy z tych składników Podsumowanie poniżej.



### <a name="viewpager"></a>ViewPager

`ViewPager` Menedżer układu, wyświetlająca kolekcję `View`s jednym naraz. Jego zadaniem jest wykrywanie gestu Przejdź użytkownika i przejdź do następnej lub poprzedniej widoku zależnie od potrzeb. Na przykład na poniższym zrzucie ekranu przedstawiono `ViewPager` przechodzenia z jednego obrazu do następnego w odpowiedzi na gestu użytkownika: 

[![Wyświetlanie przejścia między widokami aplikacji Closeup TreePager](images/02-transition-sml.png)](images/02-transition.png#lightbox)


### <a name="adapter"></a>Adapter

`ViewPager` pobiera dane z *karty*. Karta zadania jest utworzenie `View`s wyświetlanych przez `ViewPager`, zapewnienie im zgodnie z potrzebami. Poniższy diagram przedstawia tę koncepcję &ndash; karta tworzy i wypełnia `View`s i udostępnia im `ViewPager`. Jako `ViewPager` wykrywa gestów Przejdź użytkownika, sprawdza, czy karty, aby zapewnić odpowiednią `View` do wyświetlenia: 

[![Diagram pokazujący sposób karty obrazy i nazwy łączy się z ViewPager](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

W tym przykładzie każdy `View` jest tworzony z obrazu drzewa i nazwa drzewa, zanim zostanie przekazany do `ViewPager`. 



### <a name="pager-indicator"></a>Pager Indicator

`ViewPager` może służyć do wyświetlania dużych zestawów danych (na przykład Galeria obrazów może zawierać setki obrazy). Aby pomóc użytkownikowi nawigowanie po dużych zestawach danych, `ViewPager` często towarzyszy *wskaźnik pager* wyświetlający ciąg. Ciąg może być tytuł obrazu, podpis lub po prostu pozycji widok bieżący w zestawie danych. 

Istnieją dwa widoki, które powodują tych informacji nawigacji: `PagerTabStrip` i `PagerTitleStrip.` każdego wyświetla ciąg w górnej części `ViewPager`, i każdego pobiera dane z `ViewPager`na karty, tak aby zawsze pozostaje w aktualnie wyświetlane `View`. Jest to różnica między nimi, że `PagerTabStrip` obejmuje wizualnej dla ciągu "bieżący" podczas `PagerTitleStrip` jest nie (jak pokazano w tych zrzuty ekranu): 

[![Zrzuty ekranu aplikacji TreePager PagerTitleStrip i PagerTabStrip](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

W tym przewodniku pokazano, jak immplement `ViewPager`, karty i wskaźnik składniki aplikacji i ich do obsługi nawigacji gestural integracji. 



## <a name="related-links"></a>Linki pokrewne

- [TreePager (przykład)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
- [FlashCardPager (przykład)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
