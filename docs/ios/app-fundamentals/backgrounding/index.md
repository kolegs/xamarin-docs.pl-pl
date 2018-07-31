---
title: Uruchamianie procesów w tle w rozszerzeniu Xamarin.iOS
description: Tło przetwarzania lub uruchamianie procesów w tle polega na umożliwieniu aplikacji wykonywania zadań w tle, gdy inna aplikacja jest uruchomiona na pierwszym planie. Ten przewodnik stanowi wprowadzenie do przetwarzania w systemie iOS w tle.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2018
ms.openlocfilehash: 73344b790bf6d4719d9a92cfa9146578dffe04e9
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350771"
---
# <a name="backgrounding-in-xamarinios"></a>Uruchamianie procesów w tle w rozszerzeniu Xamarin.iOS

_Tło przetwarzania lub uruchamianie procesów w tle polega na umożliwieniu aplikacji wykonywania zadań w tle, gdy inna aplikacja jest uruchomiona na pierwszym planie. Ten przewodnik stanowi wprowadzenie do przetwarzania w systemie iOS w tle._

Uruchamianie procesów w tle w aplikacjach mobilnych różni się zasadniczo od tradycyjnych pojęcia wielowątkowości na pulpicie. Komputerach stacjonarnych mają różne zasoby dostępne dla aplikacji, w tym powierzchnię ekranu, moc i pamięci. Aplikacje to możliwość uruchamiania side-by-side i pozostają wydajne i można używać. Na urządzeniu przenośnym zasoby są bardziej ograniczone. Jest trudne wyświetlić więcej niż jedną aplikację na małym ekranie, a korzystający z kilku aplikacji działać z pełną prędkością będzie wyczerpania baterii. Uruchamianie procesów w tle jest stałe kompromis między dając aplikacji zasobów, aby uruchomić zadania w tle, których potrzebują do wykonania oraz i utrzymywanie foregrounded aplikacji i urządzenie dynamiczne. Systemów iOS i Android mają postanowienia dotyczące uruchamianie procesów w tle, ale ich obsługa na różne sposoby.

W systemie iOS uruchamianie procesów w tle jest rozpoznawana jako stan aplikacji, a aplikacje zostaną przeniesione i stan tła w zależności od zachowania użytkownika i aplikacji. dla systemu iOS również oferuje kilka opcji dołączenie aplikacji do uruchamiania w tle, w tym prosząc systemu operacyjnego przez czas w celu wykonania ważne zadania, działające jako typ aplikacji konieczne tła, a odświeżenie zawartości aplikacji na określone odstępach czasu.

W tym przewodniku i towarzyszący wskazówki dotyczące zamierzamy Dowiedz się, jak wykonywać zadania aplikacji w tle. Firma Microsoft będzie obejmować kluczowe założenia i najlepsze rozwiązania, a następnie kroku proces tworzenia aplikacji świata rzeczywistego, który odbiera aktualizacje lokalizacji w tle.

## <a name="contents"></a>Spis treści

1.  [Wprowadzenie do uruchamiania procesów w tle w systemie iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [Pokaz dotyczący cyklu życia aplikacji](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [Techniki uruchamiania procesów w tle w systemie iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [Przewodnik: uruchamianie procesów w tle w systemie iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [Wskazówki dotyczące uruchamiania procesów w tle w systemie iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Podsumowanie

W tym przewodniku wprowadziliśmy różne sposoby wykonywania przetwarzania w tle w systemie iOS. Firma Microsoft objętych Stany aplikacji systemu iOS i zbadać rolę odgrywa w całym cyklu życia aplikacji dla systemu iOS uruchamianie procesów w tle. Ponadto dowiedzieliśmy się, jak firma Microsoft może zarejestrować poszczególne zadania i całej aplikacji do działania w tle w systemie iOS. Na koniec mamy wzmocnione zrozumienie uruchamianie procesów w tle w systemie iOS, tworząc aplikacje, które wykonują aktualizacje w tle.



## <a name="related-links"></a>Linki pokrewne

- [Uruchamianie procesów w tle w systemie Android](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (przykład)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [Lokalizacja (przykład)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Proste transferu w tle (przykład)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [Wykonywanie w tle systemu iOS](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
