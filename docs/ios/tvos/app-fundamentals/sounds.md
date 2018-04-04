---
title: Odtwarzanie dźwięku z AVAudioPlayer
description: W tym artykule pokazano, jak użyć Klasa pomocy do sterowania odtwarzanie dźwięku za pomocą AVAudioPlayer.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c50aea9c4c35e91c2baa98c94db2fd7c61136d69
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="playing-sound-with-avaudioplayer"></a>Odtwarzanie dźwięku z AVAudioPlayer

_W tym artykule pokazano, jak użyć Klasa pomocy do sterowania odtwarzanie dźwięku za pomocą AVAudioPlayer._

## <a name="about-the-avaudioplayer"></a>O AVAudioPlayer

`AVAudioPlayer` Służy do odtwarzania audio danych z pamięci lub pliku. Firma Apple zaleca do odtwarzania dźwięku w aplikacji, chyba że robią sieci przesyłania strumieniowego lub wymagają audio krótki czas oczekiwania operacji We/Wy przy użyciu tej klasy.

Można użyć `AVAudioPlayer` wykonać następujące czynności:

- Odtwarzanie dźwięków z dowolnego czasu trwania z pętli opcjonalne.
- Odtwarzanie dźwięków wielu jednocześnie z synchronizacją opcjonalne.
- Sterowanie woluminu, szybkość odtwarzania i stereo rozmieszczania dla każdego odtwarzanie dźwięku.
- Obsługuje funkcje, takie jak szybkie przewijanie do przodu lub do tyłu.
- Uzyskiwanie poziomu odtwarzania pomiaru danych.

`AVAudioPlayer` obsługuje dźwięki w dowolnym formacie audio, takie jak pochodzącymi z systemem iOS, systemu tvOS i OS X `.aif`, `.wav` lub `.mp3`.

## <a name="playing-sounds-in-tvos"></a>Odtwarzanie dźwięków w systemu tvOS

Ponieważ systemu tvOS obsługuje tej samej klasy przybornika Audio jako dla systemu iOS, zobacz nasze iOS [odtwarzanie dźwięku z AVAudioPlayer](http://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) dokumentację, aby uzyskać szczegółowe informacje dotyczące odtwarzania audio w aplikacji Xamarin.tvOS.



## <a name="related-links"></a>Linki pokrewne

- [Odwołanie AVAudioPlayer](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
