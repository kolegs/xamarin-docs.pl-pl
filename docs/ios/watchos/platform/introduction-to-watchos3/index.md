---
title: Wprowadzenie do watchOS 3
description: W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w watchOS 3 dla deweloperów programu Xamarin.
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2017
ms.openlocfilehash: 506e3795538ceffc77301a608c504fc6ec2045a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-watchos-3"></a>Wprowadzenie do watchOS 3

_W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w watchOS 3 dla deweloperów programu Xamarin._

Tym dokumencie omówiono następujące tematy:

- [Nowości w watchOS 3](#Whats-New-in-watchOS-3)
    - [Ulepszenia zwrócić Apple](#Apple-Pay-Enhancements) na Apple Watch powoduje dodanie obsługi płatności w aplikacji.
    - [Zadania w tle](#Background-Tasks) umożliwiają aplikacji zaktualizować informacje w tle, więc jest gotowy, gdy użytkownik musi on.
    - [Ulepszenia komplikacji](#Complications-Enhancements) dokonano watchOS 3, który udostępnia nowe funkcje aplikacji.
    - [Nowo dostępnych platform](#Newly-Available-Frameworks) zostały uwidocznione dla aplikacji watchOS.
    - [Aktywne sugestie](#Proactive-Suggestions) zezwala aplikacji na aktywne zawierają informacje dla użytkownika.
    * Kilka [zabezpieczeń i prywatności rozszerzenia](#Security-and-Privacy-Enhancements) wprowadzono watchOS 3.
    - [Migawki i dokowania](#Snapshots-and-Dock) zapewniają szybki dostęp do aplikacji watchOS aplikacji użytkownika.
    - [Powiadomienia użytkowników](#User-Notifications) udostępnia lokalne i zdalne powiadomień dla użytkownika.
    * Kilka [czujki łączności Framework ulepszenia](#Watch-Connectivity-Framework-Enhancements) zostały wprowadzone w programie watchOS 3.
    * Kilka [WatchKit Framework ulepszenia](#WatchKit-Framework-Enhancements) zostały wprowadzone w programie watchOS 3.
    - [Ulepszenia aplikacji ćwiczeń](#Workout-App-Enhancements) oferuje nowe możliwości ćwiczeń powiązane Apple Watch aplikacji.
- [Dodatkowe zmiany Framework](#Additional-Framework-Changes) zostały wprowadzone w całym watchOS 3.
- [Przestarzałe interfejsy API](#Deprecated-APIs) w watchOS 3.

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>Nowości w watchOS 3

Apple został dodany kilka nowych interfejsów API i usługi watchOS 3 oraz wiele ulepszenia istniejących funkcji, w tym:

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Ulepszenia płatności firmy Apple

W watchOS 3 Aby umożliwić obsługę bezpiecznego, w aplikacji płatności (towarów fizycznych i usługi) dla aplikacji uruchomionych na Apple Watch została rozszerzona PassKit framework.

Użyj nowych [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) i [PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) klasy reagowania na interfejs, gdy użytkownik może autoryzować żądania płatności i stanowi.

Aby dowiedzieć się więcej, zobacz nasze [firmy Apple należy zwrócić ulepszenia](~/ios/watchos/platform/apple-pay.md) przewodnik.

<a name="Background-Tasks" />

## <a name="background-tasks"></a>Zadania w tle

watchOS 3 wprowadzono kilka zadań w tle, które aplikacji można użyć do zaktualizowania jej informacje o sprawdzeniu, czy ma ona zawartość musi przez użytkownika, zanim zostaną one otwarte.

Dostępne są następujące nowe zadania tła:

- **Aplikacja odświeżania w tle** - [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) zadanie pozwala aplikacji na aktualizowanie stanu w tle. Zwykle będzie to obejmowało innego zadania, takie jak pobieranie nowej zawartości z Internetu za pomocą [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Migawki odświeżania w tle** - [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) zadanie pozwala aplikacji na aktualizowanie jej zawartości i interfejsu użytkownika, zanim system tworzy migawkę, który będzie używany do wypełniania doku.
- **Obejrzyj łączność w tle** - [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) zadanie zostało uruchomione dla aplikacji, gdy odbiera dane w tle z sparowanego iPhone.
- **Adres URL sesja w tle** - [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) zadanie zostało uruchomione dla aplikacji podczas transferu w tle wymaga autoryzacji lub zakończeniu (pomyślnie lub błędów).

Aby dowiedzieć się więcej, zobacz nasze [zadania w tle](~/ios/watchos/platform/background-tasks.md) przewodnik.

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>Ulepszenia komplikacji

Komplikacji są małe elementy wizualne, dostarczające przydatnych informacji na pierwszy rzut oka. W zależności od kroju czujki zaznaczone użytkownik ma możliwość dostosowania krój czujki z co najmniej jeden Complication.

watchOS 3 daje aplikacji możliwość tworzenia Complication jeden lub więcej aplikacji czujki tak, aby użytkownik ma dostęp jego informacji w ułatwia z krój czujki.

Ponadto komplikacji zapewniają następujące korzyści:

- Użytkownik może szybko uruchomić aplikację, naciskając pozycję na Complication bezpośrednio z krój czujki.
- Mających jedno komplikacji aplikacji na przyczyny krój czujki systemu do zachowania aplikacji w stanie gotowe do uruchomienia, gdy próbuje uruchomić aplikację w tle, przechowuj go w pamięci i zapewnia bardzo czasu aktualizacji.
- Komplikacji dotrą co najmniej 50 wypychaną aktualizacji na dzień.
- Jeśli aplikacja zawiera komplikacji, zostanie on zostać umieszczony w galerii krój Apple Watch (zobacz firmy Apple [Dodawanie komplikacji do galerii](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery) dokumentacji, aby uzyskać więcej informacji).

W watchOS 3 ClockKit framework teraz obejmuje kilka nowych szablonów dla bardzo dużych komplikacji takich jak [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) i [CLKComplicationTemplateExtraLargeRingImage ](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). Ponadto, aby utworzyć Lokalizowalny tekst, użyj nowych metod [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider) klasy.

Aby dowiedzieć się więcej, zobacz nasze [szybkie techniki interakcji watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) przewodnik.

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>Nowo dostępne platformy

watchOS 3 zawiera kilka istniejących struktur firmy Apple, które były wcześniej dostępne, takich jak:

- **SceneKit** -SceneKit Użyj, aby uwzględnić 3D modeli w Interfejsie użytkownika aplikacji czujki większość systemów cząstki i funkcje dostępne na innych platformach, takich jak fizyka oświetlenia cieniowania animacji, w tym. 3D audio przestrzennych, niestandardowych programów do cieniowania systemu operacyjnego lub OpenGL, filtry obrazów Core i fizycznie podstawie materiałów nie są obsługiwane.
- **SpriteKit** -SpriteKit używany do renderowania i animować ikony w interfejsie użytkownika aplikacji czujki aplikacji tym większość funkcji dostępne na innych platformach, np. akcji, fizycznych, oświetlenia i cząstki systemów. 3D przestrzennych audio, odtwarzanie wideo i filtry obrazów podstawowych nie są obsługiwane.
- **AVFoundation** — do zarządzania i odtwarzania dźwięku.
- **CloudKit** — aby przenoszenie danych między czujki kontenery aplikacji i usługi iCloud.
- **Podstawowe Audio** — do zarządzania typami danych reprezentujący strumieni audio, buforów złożone i wartości czasu.
- **GameKit** — do tworzenia gier społecznych.

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>Sugestie aktywne

watchOS 3 zezwala aplikacji na aktywne są wyświetlane informacje użytkownika w ramach danego kontekstach. Do obsługi tej funkcji [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) zawiera teraz `MapItem` właściwość, która umożliwia aplikacji, podaj informacje o lokalizacji do późniejszego użytku przez inne aplikacje.

Aby dowiedzieć się więcej, zobacz nasze [wprowadzenie do aktywnego sugestie](~/ios/watchos/platform/proactive-suggestions.md) przewodnik.

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>Bezpieczeństwo i prywatność ulepszenia

Apple wprowadziła kilka ulepszeń zarówno prywatności i bezpieczeństwa w watchOS 3 pomoże developer poprawy zabezpieczeń aplikacji i zapewnienia zachowania poufności użytkownika końcowego.

W związku z tym aplikacje działające na watchOS 3 (lub nowszy) statycznie musi deklarować ich próba dostępu do określonych funkcji lub informacje użytkownika, wprowadzając jeden lub więcej określonych kluczy prywatności w ich `Info.plist` pliki, które wyjaśnić użytkownikowi przyczynę aplikacja chce uzyskać dostęp.

Ponieważ watchOS 3 współużytkuje te zmiany z systemem iOS 10, zobacz nasze iOS 10 [zabezpieczeń i prywatności rozszerzenia](~/ios/app-fundamentals/security-privacy.md) przewodnika, aby uzyskać więcej informacji.

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>Migawki i doku.

W watchOS 3 Apple został dodany dokowania, w którym użytkownicy mogą przypiąć ich ulubionych aplikacji i szybko uzyskiwać do nich dostęp. Gdy użytkownik naciśnie przycisk po stronie na Apple Watch, pojawi się galerii migawek przypiętych aplikacji. Użytkownik może szybko przesuń od lewej lub prawej strony, aby znaleźć odpowiednią aplikację, a następnie wybierz aplikację, aby uruchomić go zamianę migawki interfejsu uruchomionej aplikacji.

System okresowo przyjmuje migawki w Interfejsie użytkownika aplikacji i użyje tych migawek, aby wypełnić z dokumentami. watchOS daje aplikacji możliwość jego zawartość i interfejsu użytkownika aktualizacji przed podjęciem tej migawki.

Aby uzyskać więcej informacji, zobacz nasze [zadania w tle](~/ios/watchos/platform/background-tasks.md) przewodnik i firmy Apple [odwołania WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) .

<a name="User-Notifications" />

## <a name="user-notifications"></a>Powiadomienia użytkowników

Struktura powiadomienie użytkownika, wprowadzone w watchOS 3 obsługuje dostarczanie powiadomień lokalnych i zdalnych do Apple Watch. Za pomocą tej architektury można zaplanować na podstawie określonych warunków, takich jak czas dzień lub lokalizacji powiadomienia i odbierają i obsługi powiadomień.

Aby dowiedzieć się więcej, zobacz nasze [szybkie techniki interakcji watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) przewodnik.

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>Obejrzyj ulepszenia Framework łączności

Nowy `HasContentPending` właściwość [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession) klasy wskazuje, że sesja odebrał danych w tle, który ma zostać przetworzony. I `RemainingComplicationUserInfoTransfers` właściwość zwraca pozostałe godziny, o których aplikacja systemu iOS można aktualizować jego watchOS Complication.

Aby dowiedzieć się więcej, zobacz nasze [zadania w tle](~/ios/watchos/platform/background-tasks.md) przewodnik.

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>Ulepszenia WatchKit Framework

watchOS 3 zawiera kilka ulepszeń framework WatchKit, takie jak następujące:

- Aplikację można uzyskać stanu cyfrowe wierzchołek przy użyciu nowego [WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) klasy i otrzymywać aktualizacje, gdy użytkownik obraca przy użyciu wierzchołek [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate) klasy.
- [WKExtension](https://developer.apple.com/reference/watchkit/wkextension) zawiera teraz klasy `ApplicationState` — metoda i [WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate) stałą, której aplikacja może służyć do śledzenia stanu środowiska uruchomieniowego aplikacji. `WKExtension` dostępne są także dwa nowych metod, które mogą służyć do zaplanowania zadania w tle.
- [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) zawiera teraz nowe `ApplicationWillEnterForeground`, `ApplicationDidEnterBackground` i `HandleBackgroundTasks` metody służące do monitorowania zmian stanu aplikacji i obsługiwać aktualizacje zadań w tle.
- Nowy [WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) klasy został dodany do określ następujące typy rozpoznawania gestów w aplikacjach watch: [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [WKPanGestureRecognizer ](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) i [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer).
- Nowy [WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) klasy udostępnia interfejs dla aparatu IP dołączyć żadnych HomeKit.
- Nowy [WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) klasy zezwala aplikacji na wyświetlanie filmów "plakat" zastępowany przez filmu uruchomione po naciśnięciu go.
- Nowy [WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) klasa umożliwia aplikacji przedstawia przycisk Apple Pay w jego interfejsie użytkownika, który inicjuje żądanie płatności wybrany.
- Nowy [WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) klasa przedstawia interfejs do wyświetlania na Apple Watch SceneKit sceny.
- Nowy [WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) klasa przedstawia interfejs do wyświetlania na Apple Watch SpriteKit sceny.

Aby dowiedzieć się więcej, zobacz nasze [szybkie techniki interakcji watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) przewodnik.

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>Ulepszenia aplikacji ćwiczeń

Nowe watchOS 3 ćwiczeń związane z, aplikacje mają możliwość uruchamiania w tle na Apple Watch. Aby włączyć tę funkcję (i uzyskać dostęp do danych HealthKit), aplikacja musi zawierać `WKBackgroundModes` klucza w `Info.plist` z wartością `workout-processing`.

Ponadto dewelopera ma teraz możliwość Uruchom aplikację ćwiczeń watchOS od wersji aplikacji dla systemu iOS na sparowanego iPhone.

Aby dowiedzieć się więcej, zobacz nasze [ulepszenia aplikacji ćwiczeń](~/ios/watchos/platform/workout-apps.md) przewodnik.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Zmiany dodatkowe Framework

Oprócz framework najważniejszych zmian i dodatków wymienionych powyżej Apple wprowadził wiele dodatkowych framework drobne zmiany w watchOS 3.

Aby dowiedzieć się więcej, zobacz nasze [dodatkowych zmian Framework](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) przewodnik.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>Przestarzałe interfejsy API

Następujące interfejsy API są przestarzałe w watchOS 3:

- `UILocalNotification` Klasy UIKit jest przestarzała i powinna zostać zastąpiona w ramach powiadomienia użytkownika.

Zobacz firmy Apple [watchOS 2.2 różnic interfejsu API watchOS 3.0](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) dokumentacji, aby uzyskać pełną listę deprecations i zmian.


## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu watchOS](https://developer.xamarin.com/samples/watchos/all/)
- [What's new in watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
