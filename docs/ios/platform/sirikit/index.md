---
title: SiriKit
description: W tym artykule przedstawiono sposób użycia SiriKit w aplikacji platformy Xamarin.iOS do świadczenia usług, które są dostępne dla użytkownika za pomocą Siri na urządzeniu z systemem iOS.
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: ac68f249361fd5bee8af102a1418c9d5f1d2c0ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="sirikit"></a>SiriKit

_W tym artykule przedstawiono sposób użycia SiriKit w aplikacji platformy Xamarin.iOS do świadczenia usług, które są dostępne dla użytkownika za pomocą Siri na urządzeniu z systemem iOS._

Nowe w systemach iOS 10, SiriKit umożliwia aplikacji systemu iOS do świadczenia usług, które są dostępne dla użytkownika za pomocą aplikacja map i używanie programu Siri na urządzeniu z systemem iOS przy użyciu rozszerzeń aplikacji i nowych **intencje** i **interfejsu użytkownika intencje** struktury.

Używanie programu Siri działa z pojęciem **domen**, grup znać akcji dla zadania pokrewne. Każdy interakcji, która aplikację z Siri muszą należeć do jednej z jego domeny znanej usługi:

- Audio i wideo telefonicznej.
- Rezerwacji jazdy.
- Zarządzanie ćwiczeń.
- Do obsługi komunikatów.
- Wyszukiwanie zdjęcia.
- Wysyłanie lub odbieranie płatności.

Gdy użytkownik wysyła żądanie Siri odwołania rozszerzenia aplikacji usług, SiriKit wysyła rozszerzenia **zamiar** obiektu, który opisuje żądanie użytkownika wraz z danymi pomocniczych. Rozszerzenie aplikacji następnie generuje odpowiednie **odpowiedzi** obiektu dla danego **zamiar**, opisujące, jak rozszerzenie może obsłużyć żądania.

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[Informacje o pojęciach dotyczących zestawu SiriKit](~/ios/platform/sirikit/understanding-sirikit.md)

W tym artykule opisano podstawowe pojęcia, które będą wymagane do pracy z SiriKit w aplikacji platformy Xamarin.iOS. Obejmuje on nowej lokalizacji docelowych i punktów rozszerzenia interfejsu użytkownika intencje i jak działają z aplikacji i słownictwa użytkownika, aby otworzyć aplikację na używanie programu Siri.

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[Implementowanie zestawu SiriKit](~/ios/platform/sirikit/implementing-sirikit.md)

W tym artykule opisano kroki wymagane do zaimplementowania SiriKit pomocy technicznej w aplikacji platformy Xamarin.iOS. Deweloper powinien przeczytać Przewodnik zrozumienie pojęcia SiriKit powyżej przed podjęciem próby dodania SiriKit pomocy technicznej dla aplikacji jako klucz który opisano pojęcia, które będzie wymagane do pomyślnego wdrożenia.





## <a name="related-links"></a>Linki pokrewne

- [Przykładowe ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Przewodnik programowania w języku SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Odwołanie do lokalizacji docelowych platformy](https://developer.apple.com/reference/intents)
- [Odwołanie do lokalizacji docelowych interfejsu użytkownika platformy](https://developer.apple.com/reference/intentsui)
