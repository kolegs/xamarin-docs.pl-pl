---
title: "Wprowadzenie do zużycia dla systemu Android"
description: "Wraz z wprowadzeniem nosić systemu Android firmy Google nie jesteś już ograniczona do właśnie telefony i tablety po przejściu do tworzenia niezwykłych aplikacji dla systemu Android. Dla platformy Xamarin.Android obsługę systemu Android nosić umożliwia uruchomienie kodu C# na nadgarstka! To wprowadzenie zawiera ogólne omówienie nosić systemu Android, opisano najważniejsze funkcje i udostępnia przegląd funkcji dostępnych w systemie Android nosić 2.0. Przedstawiono najpopularniejszych urządzeń z systemem Android nosić i linki do podstawowych Google Android nosić dokumentacji, aby uzyskać więcej informacji."
ms.topic: article
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c334e78793f90b4f349f87e12e6b0093fe5cacf8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-android-wear"></a>Wprowadzenie do zużycia dla systemu Android

_Wraz z wprowadzeniem nosić systemu Android firmy Google nie jesteś już ograniczona do właśnie telefony i tablety po przejściu do tworzenia niezwykłych aplikacji dla systemu Android. Dla platformy Xamarin.Android obsługę systemu Android nosić umożliwia uruchomienie kodu C# na nadgarstka! To wprowadzenie zawiera ogólne omówienie nosić systemu Android, opisano najważniejsze funkcje i udostępnia przegląd funkcji dostępnych w systemie Android nosić 2.0. Przedstawiono najpopularniejszych urządzeń z systemem Android nosić i linki do podstawowych Google Android nosić dokumentacji, aby uzyskać więcej informacji._

<a name="overview" />

## <a name="overview"></a>Omówienie

Android zużycia działa na wielu urządzeniach, włącznie z pierwszej generacji 360 firmie, obejrzyj G LG przez i na żywo koło zębate Samsung. Drugiej generacji, jego Sony SmartWatch 3, w tym również została wydana dodatkowe funkcje, w tym wbudowane GPS i odtwarzanie muzyki w trybie offline. Dla systemu Android 2.0 nosić Google ma razem z LG dla dwóch nowych obserwowanie: Sport czujki LG i styl czujki LG.

![Urządzenia z systemem android nosić 2.0](intro-to-wear-images/hero-image.png "przykład nosić 2.0 urządzeń z systemem Android")

Obsługuje platformy Xamarin.Android 5.0 i nowszych obsługuje nosić Android za pośrednictwem naszego 4.4W systemu Android (interfejs API 20) i formanty interfejsu użytkownika dotyczące zużycia pakietu NuGet, który dodaje dodatkowe. Xamarin.Android 5.0 i nowszych także funkcje do pakowania aplikacji zużycia. Pakiety NuGet są również dostępne dla systemu Android 2.0 nosić zgodnie z opisem w dalszej części tego przewodnika.


<a name="basics" />

## <a name="android-wear-basics"></a>Podstawowe informacje dotyczące zużycia dla systemu android

Android zużycia ma modelu interfejsu użytkownika, który różni się od aplikacji urządzenia przenośnego systemu Android. Pierwszy wave zużycia aplikacji są przeznaczone do rozszerzenia pomocnika aplikacji urządzenia przenośnego w niektórych sposób, ale począwszy od systemu Android zużycia 2.0, zużycia aplikacji może być wykorzystywana samodzielnie. Podczas wdrażania aplikacji zużycia jest dostarczana z aplikacją handheld pomocnika. Ponieważ większość nosić aplikacji zależnych od aplikacji urządzenia przenośnego pomocnika, potrzebują niektórych sposób komunikowania się z aplikacji urządzenia przenośnego. W poniższych sekcjach opisano te scenariusze użycia i przedstawiają ważne funkcje nosić systemu Android. 


<a name="scenarios" />

### <a name="usage-scenarios"></a>Scenariusze użycia

Pierwszej wersji Android nosić testowano przede wszystkim na synchronizację danych między aplikacją urządzenia przenośnego i wearable aplikacji i rozszerzanie bieżące aplikacje urządzenia przenośnego z rozszerzoną powiadomienia. Dlatego te scenariusze są stosunkowo prosta do wdrożenia.

<a name="notifications" />

#### <a name="wearable-notifications"></a>Wearable powiadomienia

Najprostszym sposobem obsługi systemu Android nosić jest mógł korzystać z udostępnionego rodzaj powiadomienia między urządzenia przenośnego i wearable urządzenia. Za pomocą powiadomień w wersji 4 obsługi interfejsu API i `WearableExtender` klasy (dostępne w [Xamarin Android obsługuje biblioteki](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)), można naciśnij do natywnej funkcji platformy, takie jak karty styl skrzynki odbiorczej lub głosu. [RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/) przykład zawiera przykładowy kod, który demonstruje sposób wysłania listy powiadomienia do urządzenia z systemem Android nosić. 


<a name="companion" />

#### <a name="companion-applications"></a>Pomocnik aplikacji

Jest kolejną strategią do utworzenia pełnej aplikacji, która działa w sposób macierzysty na urządzeniu wearable i współdziała z aplikacją handheld pomocnika. Dobrym przykładem tego podejścia jest [kwizu](https://developer.xamarin.com/samples/monodroid/wear/Quiz/) przykładowej aplikacji, która przedstawia sposób tworzenia testu, jest uruchamiany na urządzeniach przenośnych i zadaje pytania kwizu wearable urządzenia. 


<a name="ui" />

### <a name="user-interface"></a>Interfejs użytkownika

Wzorzec głównej nawigacji używanych jest szereg kart ułożone pionowo. Każdy z tych kart można skojarzyć akcje, które są warstwie się na tym samym wierszu. `GridViewPager` Przez klasę; zgodne karty synonim `ListView`. Zwykle skojarzenia `GridViewPager` z `FragmentGridPagerAdaptor` (lub `GridPagerAdaptor`), która umożliwia reprezentować każdy wiersz i kolumnę komórki jako `Fragment`: 

[ ![Nosić nawigacji](intro-to-wear-images/2d-picker-sml.png "nosić nawigacji")](intro-to-wear-images/2d-picker.png)

Nosić również sprawia, że użycie przycisków akcji, który składa się z dużą pokolorowane koło z małych opis podrzędne (jako ilustrowane powyżej).  [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/) przykładzie pokazano sposób użycia `GridViewPager` i `GridPagerAdapter` w aplikacji zużycia.

Android 2.0 nosić dodaje szuflady nawigacji, szuflady akcji i przyciski akcji wbudowanego interfejsu użytkownika zużycia. Aby uzyskać więcej informacji na temat elementów interfejsu użytkownika dla systemu Android nosić 2.0, zobacz Android [Anatomy](https://www.google.com/design/spec-wear/system-overview/anatomy.html) tematu. 


<a name="comm" />

### <a name="communications"></a>Komunikacja

Zużycie android zapewnia dwa komunikację różnych interfejsy API w celu ułatwienia komunikacji między aplikacjami wearable i pomocnika aplikacje urządzenia przenośnego: 

**Danych interfejsu API** &ndash; ten interfejs API jest podobny do magazynu danych synchronizowane między urządzeniem wearable i urządzeniach przenośnych. Zapewnia obsługę systemu android propagowanie zmian między wearable i urządzenia przenośnego po optymalny sposób. Gdy do noszenia jest poza zakresem, kolejek synchronizacji na później. Główny punkt wejścia dla tego interfejsu API jest `WearableClass.DataApi`. Aby uzyskać więcej informacji na temat tego interfejsu API, zobacz Android [synchronizowanie elementów danych](https://developer.android.com/training/wearables/data-layer/data-items.html) tematu. 

**Komunikat interfejsu API** &ndash; ten interfejs API umożliwia można użyć niższych ścieżki komunikacji poziomu: mały ładunku są wysyłane bez synchronizacji między aplikacjami urządzenia przenośnego i wearable jednokierunkowe.
Główny punkt wejścia dla tego interfejsu API jest `WearableClass.MessageApi`.
Aby uzyskać więcej informacji na temat tego interfejsu API, zobacz Android [wysyłanie i odbieranie wiadomości](https://developer.android.com/training/wearables/data-layer/messages.html) tematu.

Istnieje możliwość rejestrowania wywołań zwrotnych do otrzymywania tych wiadomości przez każdy z interfejsów API odbiornika, lub też implementację usługi w swojej aplikacji, która jest pochodną `WearableListenerService`.
Ta usługa zostanie automatycznie utworzona przez nosić systemu Android.
[FindMyPhone](https://developer.xamarin.com/samples/monodroid/wear/FindMyPhoneSample/) przykład ilustruje sposób implementowania `WearableListenerService`.


<a name="deploy" />

### <a name="deployment"></a>wdrażania

Każdy wearable aplikacja jest wdrażana z własnego pliku APK osadzone wewnątrz aplikacji głównej APK. Opakowaniem odbywa się automatycznie w Xamarin.Android 5.0 lub nowszej, ale musi być wykonywane ręcznie dla wersji platformy Xamarin.Android wcześniejszej niż wersja 5.0. 
[Praca z opakowania](~/android/wear/deploy-test/packaging.md) opisano wdrożenie bardziej szczegółowo. 


<a name="further" />

## <a name="going-further"></a>Kontynuowanie 

To najlepszy sposób, aby zapoznać się z systemem Android nosić umożliwiający zbudowanie i przetestowanie pierwszej aplikacji. Poniższa lista zawiera kolejności zalecane materiały do przeczytania, aby pomóc Ci szybko szybko:

1.  [Instalator i instalacja](~/android/wear/get-started/installation.md) zawiera szczegółowe instrukcje dotyczące instalowania i konfigurowania środowiska deweloperskiego do tworzenia aplikacji platformy Xamarin.Android zużycia. 

2.  Po zainstalowaniu wymaganych pakietów i skonfigurować emulatora albo urządzenia, zobacz [nosić Witaj,](~/android/wear/get-started/hello-wear.md) instrukcje krok po kroku, umożliwiające tworzenie małych projektu systemu Android nosić przycisku dojść kliknie i wyświetla kliknij licznik zużycia urządzenia. 

3.  [Wdrażanie i testowanie](~/android/wear/deploy-test/index.md) zawiera bardziej szczegółowe informacje o konfigurowaniu i wdrażaniu emulatorów i urządzeń, w tym instrukcje dotyczące sposobu wdrażania aplikacji do zużycia urządzenia za pośrednictwem połączenia Bluetooth.

4.  [Praca z rozmiaru ekranu](~/android/wear/screen-sizes.md) wyjaśniono, jak wyświetlić podgląd i optymalizowanie interfejsu użytkownika dla różnych rozmiarów ekranu dostępne na urządzenia zużycia. 

5.  [Praca z opakowania](~/android/wear/deploy-test/packaging.md) opisano kroki dotyczące ręcznie pakowania aplikacji zużycia dla dystrybucji w witrynie Google Play.

Po utworzeniu pierwszej aplikacji zużycia może chcesz spróbować budowania krój niestandardowych czujki nosić systemu Android. 
[Tworzenie krój czujki](~/android/wear/platform/creating-a-watchface.md) zawiera szczegółowe instrukcje oraz przykładowy kod umożliwiający projektowanie pozbawionego włókien dół usługi krój czujki cyfrowych, następuje więcej kod, który zwiększa go do powierzchni czujki analogowy stylu z dodatkowych funkcji. 


<a name="wear2" />

## <a name="android-wear-20"></a>Android Wear 2.0

Android 2.0 nosić wprowadzono wiele nowych funkcji i możliwości, takie jak *komplikacji*, krzywych układów szuflady nawigacji i akcji i rozszerzonych powiadomieniach. Ponadto nosić 2.0 umożliwia kompilowanie aplikacji autonomicznej, które działają niezależnie od aplikacji urządzenia przenośnego. Nowy *gestów nadgarstka* pozwala jednoręczny poruszanie się ograniczenia interakcji z aplikacją. W poniższych sekcjach zaznacz te funkcje i znajdują się linki ułatwiające rozpoczęcie pracy z ich użyciem w aplikacji.


<a name="install2" />

### <a name="install-wear-20-packages"></a>Zainstaluj nosić pakietów 2.0

Aby tworzenie aplikacji nosić 2.0 z platformy Xamarin.Android, należy dodać **Xamarin.Android.Wear v2.0** pakietu do projektu (kliknij **kartę Przeglądaj**):

[![Xamarin.Android.Wear v2.0](intro-to-wear-images/wear-nuget-2.0-sml.png "instalowania NuGet w wersji 2.0 Xamarin.Android.Wear")](intro-to-wear-images/wear-nuget-2.0.png)

Ten pakiet NuGet zawiera powiązań dla bibliotek Wearable Obsługa systemu Android i nosić Compat.

Oprócz **Xamarin.Android.Wear**, zaleca się zainstalowanie **Xamarin.GooglePlayServices.Wearable** NuGet: 

[![Xamarin.GooglePlayServices.Wearable](intro-to-wear-images/gpsw-nuget-sml.png "instalowania Xamarin.GooglePlayServices.Wearable NuGet")](intro-to-wear-images/gpsw-nuget.png)

<a name="wear2feat" />

### <a name="key-features-of-wear-20"></a>Najważniejsze funkcje zużycia 2.0

Android nosić 2.0 jest największych aktualizacji dla systemu Android nosić od momentu jego uruchomienia początkowego w 2014 r. W poniższych sekcjach omówiono najważniejsze funkcje Android nosić 2.0 i podano linki ułatwiające rozpoczęcie pracy z nowych funkcji w aplikacji. 

<a name="compl" />

#### <a name="complications"></a>Komplikacji

*Komplikacji* są małe czujki krój elementy widget, które widać w skrócie bez konieczności szybko przesuń kroju czujki. Komplikacji są podobne do elementów widget pulpitu nawigacyjnego stylu pulpitu; wyświetlane informacje, takie jak pogodzie, czas pracy baterii, zdarzenia kalendarza i przydatności aplikacji statystyki: 

![Przykład komplikacji](intro-to-wear-images/complications.png "przykład komplikacji")

Aby uzyskać więcej informacji o komplikacji, zobacz Android [komplikacji krój czujki](https://developer.android.com/wear/preview/features/complications.html) tematu. 


<a name="drawers" />

#### <a name="navigation-and-action-drawers"></a>Nawigacji i szuflady akcji 

Dwa nowe szuflady znajdują się w nosić 2.0. *Szuflady nawigacji*, która jest wyświetlana w górnej części ekranu, umożliwia użytkownikom przechodzenie między widokami aplikacji (jak pokazano poniżej po lewej stronie). *Szuflady akcji*, która jest wyświetlana w dolnej części ekranu (jak pokazano po prawej stronie) umożliwia użytkownikom wybór z listy akcji. 

![Nawigacji i szuflady akcji](intro-to-wear-images/drawers.png "nawigacji i szuflady akcji")

Aby uzyskać więcej informacji o tych dwóch nowych szuflady interaktywne, zobacz Android [nosić nawigacji i akcje](https://developer.android.com/wear/preview/features/ui-nav-actions.html) tematu. 


<a name="curved" />

#### <a name="curved-layouts"></a>Układy krzywych 

Zużycie 2.0 wprowadza nowe funkcje do wyświetlania zakrzywioną układów round zużycia urządzenia. W szczególności nowe `WearableRecyclerView` klasa jest zoptymalizowana do wyświetlania listy elementów w pionie na ekranach zaokrąglij: 

![Przykładowy układ zaokrąglona](intro-to-wear-images/curved-layout.png "przykład układu krzywych")

`WearableRecyclerView` Rozszerza `RecyclerView` klasy do obsługi układów krzywych i cykliczne gestów przewijania. Aby uzyskać więcej informacji, zobacz Android [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) dokumentacji interfejsu API. 


<a name="standalone" />

#### <a name="standalone-apps"></a>Aplikacje autonomiczne 

Aplikacje dla systemu android nosić 2.0 można pracować niezależnie od aplikacji urządzenia przenośnego. Oznacza to, że, na przykład czujki inteligentne można nadal oferują pełnej funkcjonalności, nawet wtedy, gdy w urządzeniu przenośnym pomocnika jest wyłączony lub daleko od wearable urządzenia. Aby uzyskać więcej informacji na temat tej funkcji, zobacz Android [aplikacje autonomiczne](https://developer.android.com/wear/preview/features/standalone-apps.html) tematu.


<a name="wrist" />

#### <a name="wrist-gestures"></a>Gestów nadgarstka 

Gestów nadgarstka pozwalają użytkownikom na interakcję z aplikacji bez użycia ekran dotykowy &ndash; umożliwić użytkownikom reagowanie na aplikację z jednej strony. Obsługiwane są dwa gestów nadgarstka: 

-   Nadgarstka szybkich limit
-   Nadgarstka szybkich w

Aby uzyskać więcej informacji, zobacz Android [gestów nadgarstka](https://developer.android.com/wear/preview/features/gestures.html) tematu. 


Istnieje wiele więcej funkcji nosić 2.0, takie jak akcje wbudowane, inteligentne odpowiedzi, zdalne danych wejściowych, rozszerzonych powiadomieniach i nowy tryb mostkowania powiadomień. Aby uzyskać więcej informacji o nowych funkcjach nosić 2.0, zobacz Android [Przegląd interfejsu API](https://developer.android.com/wear/preview/api-overview.html). 


<a name="devices" />

## <a name="devices"></a>Urządzenia

Oto kilka przykładów urządzeń, które można uruchomić nosić Android:

* [Motorola 360](https://moto360.motorola.com/)
* [LG G Watch](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
* [Obejrzyj G LG R](http://www.lg.com/us/smartwatch/g-watch-r)
* [Koło zębate Samsung na żywo](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
* [Sony SmartWatch 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
* [ASUS ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)


<a name="reading" />

## <a name="further-reading"></a>Dalsze informacje

Zapoznaj się dokumentacją nosić systemu Android firmy Google:

* [O zużycia dla systemu Android](http://www.android.com/wear/)
* [Projekt aplikacji systemu android zużycie](https://developer.android.com/design/wear/index.html)
* [Biblioteka android.support.wearable ](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
* [Android zużycia 2.0](https://developer.android.com/wear/preview/index.html)


<a name="summary" />

## <a name="summary"></a>Podsumowanie

To wprowadzenie podać omówienie nosić systemu Android. Go opisane podstawowych funkcji systemu Android nosić i uwzględnione omówienie funkcji wprowadzonych w systemie Android nosić 2.0. Zapewniała linki do podstawowych czytania, aby pomóc deweloperom szybkie wprowadzenie do rozwoju platformy Xamarin.Android zużycia, a jego wymienione przykłady niektórych urządzeń z systemem Android nosić obecnie na rynku.


## <a name="related-links"></a>Linki pokrewne

- [Instalacja i Konfiguracja](~/android/wear/get-started/installation.md)
- [Wprowadzenie](~/android/wear/get-started/index.md)
