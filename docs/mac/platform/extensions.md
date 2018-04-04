---
title: Obsługa rozszerzeń Xamarin.Mac
description: W tym artykule omówiono rozszerzenia obsłudze w Xamarin.Mac wersji 2.10 (lub nowszej).
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 03936c75d31bfd01e741ad2c09096c925dc9dbfc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-extension-support"></a>Obsługa rozszerzeń Xamarin.Mac

W wersji zapoznawczej Xamarin.Mac 2.10 została dodana obsługa wielu punktów rozszerzenia macOS:

- Wyszukiwanie
- Udostępnij
- Dzisiaj

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>Ograniczenia i znane problemy

Poniżej są ograniczenia i problemy, które mogą wystąpić podczas opracowywania rozszerzeń w Xamarin.Mac wiedzieć:

* Nie jest obecnie obsługiwane debugowania w programie Visual Studio dla komputerów Mac. Debugowanie wszystkich musi odbywać się za pośrednictwem **NSLog** i **konsoli**. Zobacz sekcję porady poniżej, aby uzyskać szczegółowe informacje.
* Rozszerzenia muszą być zawarte w aplikacji hosta, które po wybraniu uruchamiane raz rejestr w systemie. Następnie musi być włączony w **rozszerzenia** sekcji **preferencjach systemowych**. 
* Niektóre awarii rozszerzenia może zdestabilizować aplikacji hosta i wystąpienie nietypowego zachowania. W szczególności **wyszukiwania** i **dzisiaj** sekcji **Centrum powiadomień** może stać się "zakleszczeniu" i przestać odpowiadać. Został w projektach o rozszerzenia w środowisku Xcode również i obecnie pojawia się związana z Xamarin.Mac. Często można to zaobserwować w dzienniku systemowym (za pośrednictwem **konsoli**, szczegółowe informacje znajdują się wskazówki dotyczące) drukowanie komunikatów o błędzie powtórzony. Ponowne uruchomienie macOS może rozwiązać ten problem.

<a name="Tips" />

## <a name="tips"></a>Porady

Poniższe porady mogą być przydatne podczas pracy z rozszerzeń Xamarin.Mac:

- Jak Xamarin.Mac nie obsługuje obecnie Debugowanie rozszerzeń, działanie debugowania głównie będzie zależeć od wykonania i `printf` , takich jak instrukcje. Jednak rozszerzenia uruchomić w proces izolowanym, w związku z tym `Console.WriteLine` będą zachowywać się jak w innych aplikacjach Xamarin.Mac. Wywoływanie [ `NSLog` bezpośrednio](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) będą dane wyjściowe debugowania komunikaty w dzienniku systemu.
- Nieprzechwyconych wyjątków ulegnie awarii procesu rozszerzenia, zapewniając małej ilości użyteczne informacje w **dziennika systemu**. Zawijanie powodującymi kodu w `try/catch` (wyjątek) zablokować, który `NSLog`użytkownika przed ponownie zgłaszanie mogą być użyteczne.
- **Dziennika systemu** są dostępne z **konsoli** aplikacji w obszarze **aplikacji** > **narzędzia**:

    [![](extensions-images/extension02.png "Dziennika systemu")](extensions-images/extension02.png#lightbox)
- Jak wspomniano powyżej, uruchomienie aplikacji hosta rozszerzenia spowoduje zarejestrowanie go za pomocą systemu. Usuwanie pakietu aplikacji z jej wyrejestrowania. 
- Jeśli "stray" wersje rozszerzeń aplikacji są zarejestrowane, użyj następującego polecenia, aby zlokalizować nimi (tak, aby je usunąć): `plugin kit -mv`


<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>Wskazówki i przykładowa aplikacja

Ponieważ dewelopera utworzy i pracować z Xamarin.Mac rozszerzeń w taki sam sposób jak Xamarin.iOS rozszerzeń, zapoznaj się z naszym [wprowadzenie do rozszerzeń](~/ios/platform/extensions.md) dokumentację, aby uzyskać więcej szczegółowych informacji.

Przykładowy projekt Xamarin.Mac zawierające małe, przykłady pracy każdego typu rozszerzenia można znaleźć [tutaj](https://developer.xamarin.com/samples/mac/ExtensionSamples/).

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało krótki przegląd pracy z rozszerzeń w aplikacji Xamarin.Max wersji 2.10 (lub nowszej).

## <a name="related-links"></a>Linki pokrewne

- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://developer.xamarin.com/samples/mac/ExtensionSamples/)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
