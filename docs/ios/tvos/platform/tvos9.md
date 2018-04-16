---
title: Wprowadzenie do systemu tvOS 9
description: W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w systemu tvOS 9 dla deweloperów Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: e2e3843506061cc79ad911404468477bf49dfe56
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-tvos-9"></a>Wprowadzenie do systemu tvOS 9

_W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w systemu tvOS 9 dla deweloperów Xamarin.tvOS._


Apple wydała 4. Generowanie sprzętu Apple TV, oferujący funkcje zmienioną, zdalne Włączanie touch, nowego systemu operacyjnego systemu tvOS (oparte na systemu iOS 9).

Po raz pierwszy systemu tvOS otwiera platformy Apple TV, do deweloperów, dzięki czemu można tworzyć rozbudowane, bez ramek aplikacje i zwolnij telewizora Apple wbudowanych sklepu z aplikacjami w procesie podobne do środowiska, zapisywania i udostępnia aplikacje dla systemu iOS za pomocą programu iTunes App Magazyn.

Jeśli znasz rozwoju platformy Xamarin.iOS powinien znajdować się proces przechodzenia do systemu tvOS dość proste. Większość funkcji i interfejsy API są takie same, jednak wiele wspólnych interfejsów API są niedostępne (na przykład WebKit). Ponadto Praca z za pomocą zdalnego Siri stanowi niektóre wyzwania projektu, które nie są obecne w ekran dotykowy na podstawie urządzeń z systemem iOS.

W tym przewodniku zapewni wprowadzenie do wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w systemu tvOS 9 dla deweloperów Xamarin.tvOS. Aby uzyskać więcej informacji dotyczących systemu tvOS, zobacz firmy Apple [tworzenie aplikacji dla nowej Apple TV](https://developer.apple.com/tvos/) dokumentacji.

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>Obsługiwane i nieobsługiwane funkcje.

aplikacje systemu tvOS systemem Apple TV ma następujące obsługiwane funkcje i możliwości:

 - Grupy aplikacji
 - Tryby tła
 - Ochrona danych
 - Centrum gier
 - Kontrolery gier
 - iCloud
 - Zakupy w aplikacji
 - Udostępnianie łańcucha kluczy

Nie są obsługiwane następujące funkcje i możliwości:

 - Płatności firmy Apple
 - Piaskownica aplikacji
 - Skojarzone domen
 - HealthKit
 - HomeKit
 - Dźwięk Inter-App
 - Mapy
 - Osobista sieć VPN
 - Powiadomienia wypychane
 - Portfela
 - Konfiguracja zasobów bezprzewodowych

Zobacz nasze [obsługiwane zestawy](~/ios/tvos/internals/assemblies.md) i [obsługiwanych platform](~/ios/tvos/internals/frameworks.md) dokumentacji, aby uzyskać więcej informacji.

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV sprzętu

Nowe Apple TV ma następujące wymagania dotyczące sprzętu:

 - 64-bitowy procesor A8
 - 32GB lub 64GB miejsca do magazynowania
 - 2GB pamięci RAM
 - 10/100 MB/s Ethernet
 - WiFi 802.11a/b/g/n/ac
 - 1080p rozwiązania
 - HDMI
 - Port USB C (dla deweloperów i diagnostycznych tylko do użytku)
 - Nowe Siri zdalnego lub (na podstawie regionu) firmy Apple TV zdalnego

### <a name="siri-remote"></a>Używanie programu Siri zdalnego

Oparte na region, podany Apple TV zdalnego wejdzie w każdej konfiguracji jednego: używanie programu Siri zdalnego lub Apple TV zdalnego.

Używanie programu Siri zdalnego jest obecnie dostępny w następujących krajach:

 - Australia
 - Kanada
 - Francja
 - Niemcy
 - Japonia
 - Hiszpania
 - Zjednoczone Królestwo
 - Stany Zjednoczone

Innych krajach otrzymają Apple TV zdalnego zastępujący przycisk Siri z przycisku wyszukiwania, który powoduje wyświetlenie ekranu wyszukiwania domyślne wprowadzanie tekstu do wyszukiwania:

[![](tvos9-images/remote02.png "Używanie programu Siri zdalnego")](tvos9-images/remote02.png#lightbox)

Aby uzyskać więcej informacji, zobacz nasze [Siri zdalnego oraz kontrolerów Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) dokumentacji.

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV inicjowania obsługi administracyjnej.

Podobnie jak tworzenie dla systemu iOS, nowego systemu tvOS wymaga prawidłowego profilu inicjowania obsługi administracyjnej do rozwoju i dystrybucji na podstawie członkostwa w zespole i podpisywania tożsamości, które zostało już ustanowione przez firmę Apple.

Odpowiednie inicjowania obsługi administracyjnej jest również dostęp do funkcji systemu tvOS, takich jak iCloud magazynie wartości KLUCZY lub CloudKit magazynów danych. Zobacz nasze [zasobów i przechowywanie danych](~/ios/tvos/app-fundamentals/resources-data-storage.md) Aby uzyskać informacje dotyczące obsługi iCloud w aplikacjach Xamarin.tvOS.

Profile inicjowania obsługi administracyjnej są tworzone i zainstalować ten sam sposób jak praca z aplikacji platformy Xamarin.iOS. Tak, zobacz nasze iOS [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) dokumentację, aby uzyskać więcej szczegółowych informacji.

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV aplikacji

Nowy sprzęt Apple TV i systemu tvOS 9 obsługuje dwa typy aplikacji: tradycyjne i klient serwer aplikacji.

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>Tradycyjne aplikacje

Tradycyjne aplikacje są zakupione w sklepie Apple TV aplikacji i są instalowane bezpośrednio na urządzeniu. Te aplikacje mogą być gry, narzędzia lub aplikacje nośnika, które są tworzone przy użyciu tej samej struktury i techniki jako aplikacji platformy Xamarin.iOS.

Aplikacje Apple TV maksymalny rozmiar 200MB i pobrać dodatkowo 2GB zawartości przy użyciu zasobów na żądanie. Zobacz nasze [magazyn danych i zasoby](~/ios/tvos/app-fundamentals/resources-data-storage.md) Aby uzyskać więcej informacji.

Zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md) do zapoznania się z narzędziami i wymagane do opracowywania aplikacji systemu tvOS, przy użyciu Xamarin.tvOS pojęcia.

<a name="Summary" />

### <a name="client-server-apps"></a>Client-Server Apps

Oprócz tradycyjne aplikacje zainstalowane Apple TV ułatwia tworzenie nośników opartych na sieci web klient serwer przesyłania strumieniowego aplikacji przy użyciu technologii sieci web (HTTPS, XML i JavaScript). Zostanie projektowanie interfejsu użytkownika przy użyciu języka znaczników TVML firmy Apple i kod JavaScript jest używany do definiowania zachowania aplikacji przy użyciu TVMLKit.

Aby uzyskać więcej informacji, zobacz firmy Apple [Apple TV Markup Language Reference](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064), [TVJS Framework odwołanie](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076), [TVMLKit Framework odwołanie](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429), [— informacje HTTP transmisji strumieniowej na żywo](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978) i [HLS tworzenia specyfikacji dla Apple TV](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf) dokumentacji.

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>Wyzwania interfejs użytkownika

W przeciwieństwie do systemu iOS i OS X Apple TV nie ma ekranu dotykowego lub mysz, które umożliwiają użytkownikom bezpośrednio wybierz i interakcji z aplikacją lub jego zawartości. Zamiast tego one użytkownika nowe zdalnego Siri lub kontrolera gry Bluetooth przejdź interfejsu użytkownika aplikacji. Aby uzyskać więcej informacji, zobacz nasze [Siri zdalnego oraz kontrolerów Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) dokumentacji.

Ponadto ogólną środowisko użytkownika znacząco różni się od systemu iOS lub Mac aplikacje, które są doświadczenia jednego użytkownika. Z Apple TV możliwości użytkowników zwykle można bardziej społecznościowych charakter, gdzie kilka osób może oczekuje na kanapie interakcji z jednej aplikacji i siebie nawzajem. Projektowanie osiągnięcie środowiska aplikacji Apple TV (Nowa aplikacja lub przenoszenia istniejącego), należy uwzględnić te zmiany. 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>Praca z fokusem i paralaksy obrazów

Jak już wspomniano użytkowników aplikacji Xamarin.tvOS nie będzie interakcji z jego interfejsu bezpośrednio z systemem iOS gdzie one wybierz obrazy na ekranie urządzenia, ale pośrednio z w pomieszczeniu przy użyciu zdalnego Siri. Aby przedstawić i obsługują interakcję z użytkownikiem, Apple TV używa fokus na podstawie modelu. 

Jako zmiany fokusu niewielkie animacje i efekty (na przykład efekt paralaksy na obrazy) są używane w celu jednoznacznego zidentyfikowania elementu interfejsu użytkownika, który aktualnie ma fokus.

Jeśli użytkownik wysyła gestu wolny, cykliczne na komputerze zdalnym Siri, element fokus zostanie omija w czasie rzeczywistym w odpowiedzi na ten ruch. Po wystąpieniu kołysanie poprzeczne oświetlonej odcieniu jest zastosowany do obrazu, jego wprowadzania powierzchni wydają się widoczne. Po upływie podanego czasu nieaktywności wygasza żadnej zawartości poza fokusu i elementu Focused wzrośnie jeszcze większym.

Aby uzyskać więcej informacji, zobacz nasze [Praca z nawigacji i skoncentrować się](~/ios/tvos/app-fundamentals/navigation-focus.md) i [Praca z obrazów i ikon](~/ios/tvos/app-fundamentals/icons-images.md) dokumentacji.

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>Ekranu głównego

Na ekranie Apple TV Home zostaną wyświetlone wszystkie aplikacje, które są zainstalowane i umożliwia dostęp do preferencji użytkownika:

[![](tvos9-images/home01.png "Ekranu głównego")](tvos9-images/home01.png#lightbox)

Użytkownik nawiguje siatki ikon aplikacji za pomocą gestów dotykowych na komputerze zdalnym Siri, wybierz aplikację i uruchom ją przy użyciu fokus. Ikona aplikacji jest pierwszej szansy aby dużą wrażenie na potencjalne użytkownika i celu aplikacji w jednym rzutem oka skontaktować się.

Każda aplikacja podać zarówno małych i dużych wersji ikona aplikacji. Małych ikon będzie używany na ekranie Apple TV Narzędzia główne, gdy aplikacja jest zainstalowana. Duża wersja jest używana przez sklepu z aplikacjami. Dużych ikon aplikacji, powinien naśladować wygląd i działanie wersji małych ikon.

Aby uzyskać więcej informacji, zobacz nasze [Praca z obrazów i ikon](~/ios/tvos/app-fundamentals/icons-images.md) dokumentacji.

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>Górnej półki

Jeśli użytkownik ma dotyczącymi aplikacji Xamarin.tvOS górny wiersz na ekranie Apple TV Narzędzia główne, duży obraz górnej półki będą wyświetlane po wybraniu aplikacji przez użytkownika. Ten obraz powinien przedstawiające funkcje aplikacji lub podaj linki bezpośrednie do jego zawartości.

[![](tvos9-images/topshelf01.png "Górnej półki")](tvos9-images/topshelf01.png#lightbox)

Obraz górnej półki albo można podać jako pojedynczy statycznego `.png` lub `.lsr` plikiem lub mogą być tworzone dynamicznie w czasie wykonywania jako pojedynczy wiersz Focusable elementów.

Zamiast statyczny obraz górnej półki może zawierać dynamicznym wierszem lub elementy Focusable lub zestawu dynamicznego transparentach przewijania. Oba te style dynamiczne pozwalają Wyróżnij zawartość dostarczone przez aplikację lub skok do jego najczęściej używane funkcje.

Aby uzyskać więcej informacji, zobacz nasze [Praca z obrazów i ikon](~/ios/tvos/app-fundamentals/icons-images.md) dokumentacji i firmy Apple [odwołania Framework TVServices](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) Aby uzyskać więcej informacji o dodawaniu rozszerzenie górnej półki do aplikacji w celu zapewnienia zawartość dynamiczna górnej półki.






## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Tworzenie aplikacji dla systemu tvOS za pomocą platformy Xamarin (klip wideo)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
