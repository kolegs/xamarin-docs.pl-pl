---
title: Odtwarzanie dźwięku w systemu tvOS za pomocą programu AVAudioPlayer w środowisku Xamarin
description: W tym artykule pokazano, jak używać klasy pomocnika do kontrolowania odtwarzanie dźwięku za pomocą pomocą odtwarzacza AVAudioPlayer aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 2ce1d4b8564ef9599581aabd6a72ba3af12ec251
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241354"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Odtwarzanie dźwięku w systemu tvOS za pomocą programu AVAudioPlayer w środowisku Xamarin

## <a name="about-the-avaudioplayer"></a>Temat pomocą programu AVAudioPlayer

`AVAudioPlayer` Służy do odtwarzania dźwięku danych z pamięci lub pliku. Firma Apple zaleca używanie tej klasy do odtwarzania dźwięku w swojej aplikacji, chyba że robią sieci przesyłania strumieniowego lub wymagają audio małych opóźnień operacji We/Wy.

Możesz użyć `AVAudioPlayer` wykonać następujące czynności:

- Odtwarzanie dźwięków wszelkie czasu trwania z pętli opcjonalne.
- Odtwarzanie dźwięków wielu, w tym samym czasie, opcjonalny w przypadku synchronizacji.
- Sterowanie woluminu, szybkość odtwarzania i stereo pozycjonowanie dla każdego odtwarzanie dźwięku.
- Obsługuje funkcje, takie jak szybkie przewijanie do przodu lub do tyłu.
- Uzyskaj poziom odtwarzania pomiaru danych.

`AVAudioPlayer` obsługuje dźwięki w dowolnym formacie audio, dostarczone przez systemów iOS, tvOS i OS X, takich jak `.aif`, `.wav` lub `.mp3`.

## <a name="playing-sounds-in-tvos"></a>Odtwarzanie dźwięków w systemu tvOS

Ponieważ systemu tvOS obsługuje tej samej klasy przybornika Audio w postaci dla systemu iOS, zobacz nasz system iOS [odtwarzanie dźwięku za pomocą programu AVAudioPlayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) dokumentacji pełne szczegóły odtwarzanie audio w aplikacji Xamarin.tvOS.



## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja pomocą odtwarzacza AVAudioPlayer](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
