---
title: Backgrounding
description: "Tło przetwarzania lub backgrounding jest proces umożliwienie aplikacji wykonać zadania w tle, gdy inna aplikacja jest uruchomiona na pierwszym planie. W tym przewodniku stanowi wprowadzenie do przetwarzania w systemie iOS w tle."
ms.topic: article
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 2bba7c0908fb78ca199cc654adad645afaf47a02
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="backgrounding"></a>Backgrounding

_Tło przetwarzania lub backgrounding jest proces umożliwienie aplikacji wykonać zadania w tle, gdy inna aplikacja jest uruchomiona na pierwszym planie. W tym przewodniku stanowi wprowadzenie do przetwarzania w systemie iOS w tle._

Backgrounding w aplikacjach mobilnych zasadniczo różni się od tradycyjnych pojęcie wielozadaniowości na pulpicie. Komputerach stacjonarnych mają różne zasoby dostępne dla aplikacji, w tym nieruchomości ekranu, moc i pamięci. Aplikacje są w stanie side-by-side pozostają wydajność i można go użyć. Na urządzeniu przenośnym zasoby są bardziej ograniczone. Jest trudne wyświetlić więcej niż jedną aplikację na małej ekranu, a z kilku aplikacji pełną prędkością czy wyczerpania baterii. Backgrounding jest stałą kompromis między dając aplikacji zasobów, aby uruchomić zadania w tle, które wymagają one również przeprowadzenie i zachowanie reakcji aplikacji foregrounded i urządzenia. Zarówno dla systemu iOS i Android mają postanowienia dotyczące backgrounding, ale ich obsługi w bardzo różne sposoby.

W systemie iOS backgrounding jest rozpoznawana jako stan aplikacji, a aplikacje zostaną przeniesione do i stan tła w zależności od zachowania użytkownika i aplikacji. iOS oferuje również kilka opcji dołączenie aplikacji do uruchamiania w tle, w tym prosząc OS czas do ukończenia ważnym zadaniem, działających jako typ aplikacji konieczne tła, i odświeżanie aplikacji zawartości na wyznaczonych odstępach czasu.

W tym przewodniku i towarzyszące wskazówki zamierzamy Dowiedz się, jak wykonać zadania aplikacji w tle. Firma Microsoft będzie obejmować podstawowych pojęć i najlepsze rozwiązania w zakresie, a następnie postępuj przez proces tworzenia aplikacji rzeczywistych, który odbiera aktualizacje lokalizacji w tle.

## <a name="contents"></a>Spis treści

1.  [Wprowadzenie do uruchamiania procesów w tle w systemie iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [Pokaz dotyczący cyklu życia aplikacji](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [Techniki uruchamiania procesów w tle w systemie iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [Przewodnik: uruchamianie procesów w tle w systemie iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [Wskazówki dotyczące uruchamiania procesów w tle w systemie iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Podsumowanie

W tym przewodniku wprowadzono różne sposoby podczas przetwarzania w tle w systemie iOS. Firma Microsoft objęte Stany aplikacji systemu iOS i zbadane roli backgrounding pełni w cyklu życia aplikacji dla systemu iOS. Ponadto dowiedzieliśmy się, jak firma Microsoft może się zarejestrować poszczególne zadania lub całej aplikacji do działania w tle w systemie iOS. Na koniec mamy wzmocnione naszych zrozumienia backgrounding w systemie iOS przez utworzenie aplikacji, które wykonują aktualizacje w tle.



## <a name="related-links"></a>Linki pokrewne

- [Backgrounding w systemie Android](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (przykład)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [W lokalizacji (przykład)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Proste transferu w tle (przykład)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS wykonywania tła](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
