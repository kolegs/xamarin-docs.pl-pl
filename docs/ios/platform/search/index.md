---
title: Interfejsy API wyszukiwania w Xamarin.iOS
description: W tym artykule omówiono, aby umożliwić użytkownikom wyszukiwanie informacji i funkcji w aplikacji platformy Xamarin.iOS przy użyciu nowych aplikacji wyszukiwania interfejsów API dostarczonych przez system iOS 9.
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bc62ad34af0b9b98f0475599a08946122badd21e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788179"
---
# <a name="search-apis-in-xamarinios"></a>Interfejsy API wyszukiwania w Xamarin.iOS

_W tym artykule omówiono, aby umożliwić użytkownikom wyszukiwanie informacji i funkcji w aplikacji platformy Xamarin.iOS przy użyciu interfejsów API wyszukiwania aplikacji, które zostały udostępnione przez system iOS 9._

Wyszukiwanie została rozszerzona w systemie iOS 9, aby zapewnić doskonałe nowe sposoby na dostęp do informacji i funkcji w aplikacji platformy Xamarin.iOS. Przy użyciu nowych interfejsów API aplikacji wyszukiwania, zawartość aplikacji jest nawiązywane za pośrednictwem Spotlight oraz Safari wyniki wyszukiwania, przekazaniem i używanie programu Siri przypomnienia i sugestie dotyczące wyszukiwania. Dzięki temu użytkownicy mogą szybko uzyskać dostęp do działań i informacji głęboko w aplikacji.

Ponadto nowe interfejsy API wyszukiwania ułatwiają integrowanie wyszukiwania w aplikacji bez obsługi wdrożenia poprzedniego wyszukiwania. W związku z tym Apple oświadczeń. zazwyczaj zajmuje kilka godzin, aby aplikacja systemu iOS 9 zawartość ogólnie można wyszukiwać w wyszukiwaniu aplikacji.

[![](images/intro01.png "Oto przykład zawartości aplikacji systemu iOS 9 powszechnie wyszukiwanie przy użyciu aplikacji wyszukiwania")](images/intro01.png#lightbox)

Wyszukiwanie aplikacji składa się z trzech osobnych interfejsów API:

1. [**NSUserActivity** ](nsuseractivity.md) — jest to rozszerzenie interfejsu API przekazaniem Apple publikowanych w systemie iOS 8. Służy do upewnij przeszukiwanie historii interakcji aplikacji publiczne i prywatne) przez użytkownika.

2. [**Podstawowe Spotlight** ](corespotlight.md) -, dzięki czemu aplikacja do indeksowania zawartości będą widoczne w wynikach wyszukiwania. Działania takie jak bazy danych interfejsu API, której elementy mogą być dodawane i usuwane i jest najlepszym sposobem indeksu prywatną zawartość w aplikacji.

3. [**WebMarkup** ](web-markup.md) — dla aplikacji, które zapewniają dostęp do zawartości za pośrednictwem interfejsu sieci web (nie tylko z poziomu aplikacji). Zawartość sieci Web może być oznaczony specjalne łącza, który będzie przeszukiwana przez firmę Apple oraz podaj połączeń bezpośrednich do aplikacji na urządzeniu z systemem iOS 9 użytkownika.

## <a name="selecting-an-app-search-approach"></a>Wybieranie metody wyszukiwania aplikacji

Przy wyborze, które z tych metod, aby zaimplementować zależy od tego, typy współdziałania udostępnione przez aplikację i typu zawartości, który stanowi.

Skorzystaj z poniższych wskazówek:

- [**NSUserActivity** ](nsuseractivity.md) — Użyj platforma, aby zapewnić możliwość wyszukiwania zawartości publiczne i prywatne i możliwość wyszukiwania punktów nawigacji w aplikacji.

- [**Podstawowe Spotlight** ](corespotlight.md) — Użyj tego framework, aby zapewnić możliwość wyszukiwania prywatnych danych przechowywanych na urządzeniu.

- [**Sieci Web znaczników** ](web-markup.md) — zapewnia możliwość wyszukiwania dla aplikacji, które stanowić jego zawartość nie tylko z poziomu aplikacji, ale także witryny sieci Web aplikacji za pomocą tej architektury.

Wyszukiwanie aplikacji podejścia są unikatowe i mogą być używane pojedynczo, jednak Apple zaprojektowane współdziałają ze sobą. Korzystając z więcej niż jednym z podejść do indeksu z określonym elementem, upewnij się, używać tego samego **identyfikator elementu** na każde podejście, więc osoba łączy pracy ze sobą.

Użycie więcej niż jeden z nich nie tylko gwarantuje, że zawartość zostaną znalezione przez użytkownika końcowego, ale także pomoże nam poprawić klasyfikacji tego elementu z w ciągu wyszukiwania.

Podczas procesu klasyfikacji w przezroczysty głównie do deweloperów, interakcji z użytkownikiem z danym elementem waga silnie po tym rangę (na przykład użytkownik Tynkowanie link).
Zapewniając elementów rozbudowane, informacyjny, możesz upewnij się, że użytkownik będzie można enticed wchodzić w interakcje z zawartości, w związku z tym wywołaniem klasyfikacji.

## <a name="what-content-to-index"></a>Zawartość do indeksu

Apple zawiera poniższe sugestie jakie zawartości i akcje zapewnienie indeksy wyszukiwania dla aplikacji:

 - Żadnej zawartości wyświetlać, tworzyć lub wyselekcjonowanych przez użytkownika, w Twojej aplikacji.
 - Nawigacji i funkcji w aplikacji.
 - Takie operacje, jak nowe wiadomości, zawartości lub innych typów elementów wyświetlanych przez aplikację, które ostatnio zostały pobrane na urządzeniu.

## <a name="app-search-enhancements"></a>Ulepszenia wyszukiwania aplikacji

Spotlight Core w systemie iOS 10 udostępnia kilka rozszerzeń takich jak do wyszukiwania aplikacji:

- **Popularne z Linkiem bezpośrednim Crowdsourced (o prywatności różnicowa)** — zapewnia sposób promowania zawartość aplikacji z linkiem bezpośrednim w wynikach wyszukiwania.
- **Wyszukiwanie w aplikacji** — Użyj nowych `CSSearchQuery` klasy, aby zapewnić możliwość wyszukiwania uwagi w aplikacji podobnie jak działają aplikacje poczty, wiadomości i notatki.
- **Wyszukaj kontynuacji** — umożliwia użytkownikowi rozpocząć wyszukiwanie Spotlight lub Safari, a następnie otwórz aplikację i kontynuować tego wyszukiwania.
- **Wizualizacja wyników weryfikacji** -firmy Apple [aplikacji wyszukiwania interfejsu API sprawdzania poprawności narzędzie](https://search.developer.apple.com/appsearch-validation-tool) podczas preforming testy są obecnie wyświetlane wizualną reprezentację znaczników witryny sieci Web, a następnie połączenie bezpośrednie.
- **Komunikat aplikacji do udostępniania obrazu** — umożliwia popularnych obrazów w aplikacji dostępne w celu udostępniania w komunikatach (za pośrednictwem rozszerzenia aplikacji wiadomości) do jego wyświetlenia na wyszukiwanie Spotlight.

Aby dowiedzieć się więcej, zobacz nasze [aplikacji wyszukiwania ulepszenia](~/ios/platform/search/app-search-enhancements.md) przewodnik.

### <a name="proactive-suggestions"></a>Sugestie aktywne

iOS 10 przedstawia nowe sposoby pobudzenie engagement dla aplikacji dzięki systemowi aktywnego wyświetlane przydatne informacje automatycznie dla użytkownika w odpowiednim czasie. Tak jak w przypadku systemu iOS 9 podać możliwość dodawania wyszukiwania bezpośrednich do aplikacji przy użyciu Spotlight, przekazaniem i sugestie Siri z systemem iOS 10 aplikacji mogą uwidaczniać funkcje, które są widoczne dla użytkownika przez system z w następujących lokalizacjach:

- Przełącznik aplikacji
- Ekran blokady
- CarPlay
- Mapy
- Używanie programu Siri interakcji
- Sugestie QuickType 

Aplikacja udostępnia tę funkcję do systemu przy użyciu kolekcji technologii, takich jak [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), znaczników sieci web, Core Spotlight, MapKit, Media Player i UIKit.

Aby dowiedzieć się więcej, zobacz nasze [aktywnego sugestie](~/ios/platform/search/proactive-suggestions.md) przewodnik.

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego nowe funkcje interfejsu API Search tego systemu iOS 9 udostępnia dla aplikacji platformy Xamarin.iOS. Ją [NSUserActivity](nsuseractivity.md), [Core Spotlight](corespotlight.md) i [znaczników sieci Web](web-markup.md) metody indeksowania zawartości. Zakończeniu krótkie omówienie stosowania podejścia podanego i jakie typy zawartości, powinny być indeksowane.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Przewodnik programowania w języku wyszukiwania aplikacji](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
