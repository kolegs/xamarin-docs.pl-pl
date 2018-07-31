---
title: Interfejsy API wyszukiwania w rozszerzeniu Xamarin.iOS
description: W tym artykule opisano, aby umożliwić użytkownikom wyszukiwanie informacji i funkcji w aplikacji platformy Xamarin.iOS przy użyciu nowych aplikacji wyszukiwania interfejsów API dostarczonych przez system iOS 9.
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4e73e1bc34df8628790a3734e5b3b32a687fdf14
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351655"
---
# <a name="search-apis-in-xamarinios"></a>Interfejsy API wyszukiwania w rozszerzeniu Xamarin.iOS

_W tym artykule opisano, aby umożliwić użytkownikom wyszukiwanie informacji i funkcji w aplikacji platformy Xamarin.iOS przy użyciu interfejsów API wyszukiwania aplikacji, które zostały dostarczone przez system iOS 9._

Wyszukiwania została rozszerzona w systemie iOS 9, aby zapewnić doskonałe nowe sposoby dostępu do informacji i funkcji w aplikacji platformy Xamarin.iOS. Przy użyciu nowych interfejsów API wyszukiwania aplikacji, zawartość aplikacji składa się można wyszukiwać za pośrednictwem funkcji w centrum uwagi oraz Safari wyników wyszukiwania, przekazywanie i przypomnienia Siri i sugestie. Dzięki temu użytkownicy mogą szybko uzyskać dostęp do działań i informacje szczegółowe w aplikacji.

Ponadto nowe interfejsy API wyszukiwania ułatwiają integrowanie wyszukiwania w aplikacji bez wcześniejszego wykonania wyszukiwania. W związku z tym Apple oświadczeń, zazwyczaj zajmuje kilka godzin, aby zawartość aplikacji systemu iOS 9 powszechnie można wyszukiwać za pomocą wyszukiwania aplikacji.

[![](images/intro01.png "Przykładem powszechnie można wyszukiwać za pomocą wyszukiwania aplikacji zawartości aplikacji systemu iOS 9")](images/intro01.png#lightbox)

Wyszukiwania aplikacji składa się z trzech oddzielnych interfejsów API:

1. [**NSUserActivity** ](nsuseractivity.md) — jest to rozszerzenie przekazywanie Apple ogólnie dostępnych w systemie iOS 8 dla interfejsu API. Służy do Dodaj Historia interakcji aplikacji multimedialnej możliwość wyszukiwania publiczne i prywatne) przez użytkownika.

2. [**Podstawowych funkcji Spotlight** ](corespotlight.md) — dzięki czemu aplikacja może indeksowania zawartości będą widoczne w wynikach wyszukiwania. To działa, takich jak bazy danych interfejsu API, gdzie elementy, które mogą być dodawane lub usuwane i jest najlepszym sposobem na indeks prywatnej zawartości w aplikacji.

3. [**WebMarkup** ](web-markup.md) — dla aplikacji, które zapewniają dostęp do jego zawartości za pośrednictwem interfejsu sieci web (nie tylko z poziomu aplikacji). Zawartość sieci Web może być oznaczony specjalne łącza, zostaną przeszukane przez firmę Apple, która zapewnia tworzenie linku do aplikacji na urządzeniu z systemem iOS 9 użytkownika.

## <a name="selecting-an-app-search-approach"></a>Wybieranie podejścia wyszukiwania aplikacji

Przy wyborze rozwiązania, które z tych metod w celu zaimplementowania zależy od tego, typy interakcji, dostarczone przez aplikację i typ zawartości, który stanowi.

Skorzystaj z poniższych wskazówek:

- [**NSUserActivity** ](nsuseractivity.md) — umożliwia ta struktura zapewnia możliwość wyszukiwania dla zawartości publicznej i prywatnej i możliwość wyszukiwania punktów nawigacji w aplikacji.

- [**Podstawowych funkcji Spotlight** ](corespotlight.md) — umożliwia ta struktura zapewnia możliwość wyszukiwania dla prywatnych danych przechowywanych na urządzeniu.

- [**Web Markup** ](web-markup.md) — umożliwia ta struktura zapewnia możliwość wyszukiwania dla aplikacji, są one ich zawartość nie tylko z poziomu aplikacji, ale również w witrynie aplikacji.

Wyszukiwanie aplikacji podejścia różnią się i mogą być używane pojedynczo, jednak Apple zaprojektowane do wspólnej pracy. Korzystając z więcej niż jedno z podejść do indeksowania określonego elementu, upewnij się, używać tego samego **identyfikator elementu** na każde podejście, więc tą osobą łączy pracy ze sobą.

Za pomocą więcej niż jedno z podejść nie tylko zapewnia, że zawartości zostaną znalezione przez użytkownika końcowego, ale także pomoże nam poprawić klasyfikacji Twojego elementu z w ramach wyszukiwania.

Podczas procesu klasyfikacji w przede wszystkim zrozumiałe dla dewelopera, interakcji z użytkownikiem z danym elementem zadowalająco uwzględni wagi intensywnie po tym rangi (na przykład użytkownik Tynkowanie link).
Dostarczając rozbudowane, i elementy, można zagwarantować, czy użytkownik będzie można enticed do interakcji z zawartości, wywoływanie w związku z tym klasyfikacji.

## <a name="what-content-to-index"></a>Zawartość do indeksu

Apple zapewnia poniższe sugestie, jakie zawartość i akcje zapewnienie indeksy wyszukiwania dla twojej aplikacji:

 - Żadnej zawartości wyświetlać, tworzyć lub nadzorowane przez użytkownika z poziomu aplikacji.
 - Punkty nawigacji i funkcji w aplikacji.
 - Elementy, takie jak nowe wiadomości, zawartości lub innych rodzajów elementów wyświetlana przez aplikację, ostatnio pobrane na urządzenie.

## <a name="app-search-enhancements"></a>Ulepszenia wyszukiwania aplikacji

Funkcja w centrum uwagi podstawowych w systemie iOS 10 zawiera kilka ulepszeń do wyszukiwania aplikacji, takich jak:

- **Popularność z Linkiem bezpośrednim dodawanych (z różnicowego prywatność)** — umożliwia podwyższenie poziomu aplikacja z linkiem bezpośrednim zawartość w wynikach wyszukiwania.
- **Wyszukiwanie w aplikacji** — Użyj nowych `CSSearchQuery` Aby klasa zapewniała podobne do działania aplikacji wiadomości E-mail, wiadomości i informacje o możliwości wyszukiwania funkcji Spotlight w aplikację.
- **Wyszukaj kontynuacji** — umożliwia użytkownikowi rozpoczęcie wyszukiwania w centrum uwagi lub przeglądarki Safari, a następnie otwórz aplikację i kontynuować tego wyszukiwania.
- **Wizualizacja wyników weryfikacji** -firmy Apple [narzędzia sprawdzającego poprawność interfejsu API wyszukiwania w aplikacji](https://search.developer.apple.com/appsearch-validation-tool) teraz wyświetla wizualną reprezentację witryny sieci Web markup i łączenie głębokie, gdy preforming testów.
- **Komunikat udostępniania obrazów aplikacji** — umożliwia popularnych obrazów w aplikacji przewidziane udostępniania w wiadomościach (za pośrednictwem rozszerzenia aplikacji wiadomości) są wyświetlane w wynikach wyszukiwania funkcji Spotlight.

Aby dowiedzieć się więcej, odwiedź nasz [ulepszenia wyszukiwania aplikacji](~/ios/platform/search/app-search-enhancements.md) przewodnik.

### <a name="proactive-suggestions"></a>Sugestie proaktywne

System iOS 10 przedstawia informacje o nowych sposobów opracowuje engagement do aplikacji, umożliwiając systemowi aktywnie wyświetlane przydatne informacje automatycznie dla użytkownika w odpowiednim czasie. Podobnie jak z systemem iOS 9, pod warunkiem możliwość dodawania głębokie wyszukiwanie do aplikacji przy użyciu funkcji Spotlight, przekazywanie i sugestie Siri z systemem iOS 10 aplikacji może narazić funkcje, które można przedstawić użytkownikowi przez system z w następujących lokalizacjach:

- Przełącznik aplikacji
- Na ekranie blokady
- CarPlay
- Mapy
- Interakcje Siri
- Sugestie QuickType 

Aplikacja ujawnia funkcjonalność systemu, przy użyciu kolekcji technologii, takich jak [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), język znacznikowy sieci web, podstawowe w centrum uwagi, strukturze MapKit, usługa Media Player i UIKit.

Aby dowiedzieć się więcej, odwiedź nasz [sugestie proaktywne](~/ios/platform/search/proactive-suggestions.md) przewodnik.

## <a name="summary"></a>Podsumowanie

W tym artykule zawiera opisano nowe funkcje interfejsu API wyszukiwania w tym z systemem iOS 9 udostępnia dla aplikacji platformy Xamarin.iOS. Ją [NSUserActivity](nsuseractivity.md), [podstawowych funkcji Spotlight](corespotlight.md) i [Web Markup](web-markup.md) metody indeksowania zawartości. Zakończeniu krótkie omówienie, kiedy należy użyć metody podanym wyszukiwaniem i jakie typy zawartości, powinny być indeksowane.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Przewodnik programowania w wyszukiwania aplikacji](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
