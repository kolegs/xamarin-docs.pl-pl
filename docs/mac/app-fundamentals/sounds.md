---
title: Odtwarzanie dźwięku za pomocą programu AVAudioPlayer platformie Xamarin.Mac
description: W tym dokumencie opisano sposób odtwarzanie dźwięku za pomocą programu AVAudioPlayer w aplikacji platformy Xamarin.Mac. Omówiono w nim pomocą odtwarzacza AVAudioPlayer na wysokim poziomie oraz linki do dokumentacji, która analizuje je dokładniej.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 03a0207ce8c742f0ca98ab6c75ed3e7b2f37d09e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241361"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Odtwarzanie dźwięku za pomocą programu AVAudioPlayer platformie Xamarin.Mac

## <a name="about-the-avaudioplayer"></a>Temat pomocą programu AVAudioPlayer

`AVAudioPlayer` Klasa jest używana do odtwarzania dźwięku danych z pamięci lub pliku. Firma Apple zaleca używanie tej klasy do odtwarzania dźwięku w swojej aplikacji, chyba że robią sieci przesyłania strumieniowego lub wymagają audio małych opóźnień operacji We/Wy.

Możesz użyć `AVAudioPlayer` klasy, aby wykonać następujące czynności:

- Odtwarzanie dźwięków wszelkie czasu trwania z pętli opcjonalne.
- Odtwarzanie dźwięków wielu, w tym samym czasie, opcjonalny w przypadku synchronizacji.
- Sterowanie woluminu, szybkość odtwarzania i stereo pozycjonowanie dla każdego odtwarzanie dźwięku.
- Obsługuje funkcje, takie jak szybkie przewijanie do przodu lub do tyłu.
- Uzyskaj poziom odtwarzania pomiaru danych.

`AVAudioPlayer` obsługuje dźwięki w dowolnym formacie audio, dostarczone przez systemy iOS, tvOS i macOS, takie jak .aif, .wav lub .mp3.

## <a name="playing-sounds-in-macos"></a>Odtwarzanie dźwięków w systemie macOS

Ponieważ macOS obsługuje tej samej klasy przybornika Audio w postaci dla systemu iOS, zobacz nasz system iOS [odtwarzanie dźwięku za pomocą programu AVAudioPlayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) dokumentacji pełne szczegóły odtwarzanie audio w aplikacji platformy Xamarin.Mac.

## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja pomocą odtwarzacza AVAudioPlayer](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
