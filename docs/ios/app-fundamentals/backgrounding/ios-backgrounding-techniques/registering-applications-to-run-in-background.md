---
title: Rejestrowanie aplikacji do uruchamiania w tle
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F89BE63-DDB5-4740-A69D-F60AEB21150D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: dddd1ad4ae70b97f17ba71a7e96b553759e35695
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="registering-applications-to-run-in-the-background"></a>Rejestrowanie aplikacji do uruchamiania w tle

Rejestrowanie poszczególne zadania dla tła uprawnienia działa w przypadku niektórych aplikacji, ale co się stanie, jeśli aplikacja jest stale wywołana po w celu przeprowadzenia ważne, długotrwałe zadania, takie jak pobieranie wskazówki dotyczące użytkownika za pośrednictwem GPS? Aplikacje, takie jak te zamiast tego powinien zostać zarejestrowany jako znanych aplikacji konieczne tła.

Rejestrowanie aplikacji sygnalizuje w systemach iOS powinny być podane specjalne uprawnienia niezbędne do wykonywania zadań w tle aplikacji.

## <a name="application-registration-categories"></a>Kategorie rejestracji aplikacji

Zarejestrowane aplikacje można podzielić na kilka kategorii:

-  **Audio** -odtwarzaczy muzycznych i inne aplikacje, które współpracują z zawartością audio może być zarejestrowany do kontynuowania odtwarzania audio, nawet gdy aplikacja nie jest już na pierwszym planie. Próba robić nic innego niż Odtwórz dźwięk i pobieranie w tle przez aplikację w tej kategorii zostanie zakończona z systemem iOS.
-  **VoIP** — aplikacje głosowych przez Internet Protocol (VoIP) uzyskać te same uprawnienia przyznane aplikacji audio, aby zachować przetwarzania dźwięku w tle. Ponadto mogą zgodnie z potrzebami odpowiadają usługom VoIP zasilania je do podtrzymywania połączenia.
-  **Zewnętrzne Akcesoria i Bluetooth** -zarezerwowany dla aplikacji, które muszą komunikować się z urządzeniami Bluetooth i inne urządzenia zewnętrzne Akcesoria, rejestracji w tych kategoriach zezwala aplikacji na łączność sprzętu.
-  **Newsstand** -A Newsstand aplikacji mogą w dalszym ciągu zsynchronizować zawartości w tle.
-  **Lokalizacja** — aplikacje, które należy używać z GPS lub danych lokalizacji sieciowej można wysyłać i odbierać aktualizacje lokalizacji w tle.
-  **Pobieranie (iOS 7 +)** — aplikację w zarejestrowany do uprawnień pobieranie w tle można sprawdzić dostawcy dla nowej zawartości w regularnych odstępach czasu, przedstawiający użytkownika obejmujących zaktualizowaną zawartość, gdy zwracają do aplikacji.
-  **Powiadomienia zdalne (iOS 7 +)** -aplikacji można zarejestrować do odbierania powiadomień od dostawcy i użyj powiadomienia, aby rozpocząć poza aktualizacji, zanim użytkownik otwiera aplikację. Powiadomienia mogą pochodzić w formie powiadomień wypychanych lub opt wznawiania aplikacji w trybie dyskretnym.


Aplikacje mogą być rejestrowane przez ustawienie **wymagane trybów tła** właściwości w aplikacji *Info.plist*. Aplikację można zarejestrować dowolną liczbę kategorii, ponieważ wymaga ona:

 [ ![](registering-applications-to-run-in-background-images/bgmodes.png "Ustawianie trybów tła")](registering-applications-to-run-in-background-images/bgmodes.png)

Przewodnik krok po kroku do rejestrowania aplikacji w tle lokalizacji aktualizacji, zobacz [tła lokalizacji wskazówki](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/location-walkthrough.md).

## <a name="application-does-not-run-in-background-property"></a>Aplikacja nie działa w tle właściwości

Inna właściwość, która może być ustawiona w *Info.plist* jest *aplikacja nie działa w tle*, lub `UIApplicationExitsOnSuspend` właściwości:

 [ ![](registering-applications-to-run-in-background-images/plist.png "Wyłączanie uruchomione w tle")](registering-applications-to-run-in-background-images/plist.png)

Ma dokładnie sam efekt co ustawienie odświeżanie aplikacji w tle, wyłączona w systemie iOS 7 +, z wyjątkiem można zmienić tylko po stronie deweloperów i jest dostępna dla systemu iOS 4 lub nowszym. Aplikacja zostanie zawieszona natychmiast po wprowadzeniu tła i nie będzie można wykonać żadnych przetwarzania.

Jeśli aplikacja nie jest przeznaczony do obsługi przetwarzania w tle, ponieważ pomaga uniknąć nieoczekiwanego zachowania, należy użyć tej właściwości.

## <a name="background-fetch-and-remote-notifications"></a>Pobieranie w tle i zdalnego powiadomienia

Pobieranie w tle i zdalnego powiadomienia są specjalne rejestracji wprowadzone w systemie iOS 7. Kategorie umożliwiają aplikacjom odbierać nowej zawartości od dostawcy i zaktualizować w tle. Następna sekcja Eksploruje pobierania i zdalnego powiadomienia większej liczby szczegółów i wprowadza również Rozpoznawanie lokalizacji, ponieważ oznacza aktualizowania aplikacji w tle w systemie iOS 6.
