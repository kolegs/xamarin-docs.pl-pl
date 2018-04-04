---
title: Odtwarzanie dźwięku z AVAudioPlayer
description: W tym artykule pokazano, jak użyć Klasa pomocy do sterowania odtwarzanie dźwięku za pomocą AVAudioPlayer.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 25e3285a1da1b6a112629001d5412fdd0788c705
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="playing-sound-with-avaudioplayer"></a>Odtwarzanie dźwięku z AVAudioPlayer

_W tym artykule pokazano, jak użyć Klasa pomocy do sterowania odtwarzanie dźwięku za pomocą AVAudioPlayer._

## <a name="about-the-avaudioplayer"></a>O AVAudioPlayer

`AVAudioPlayer` Klasa służy do odtwarzania audio danych z pamięci lub pliku. Firma Apple zaleca do odtwarzania dźwięku w aplikacji, chyba że robią sieci przesyłania strumieniowego lub wymagają audio krótki czas oczekiwania operacji We/Wy przy użyciu tej klasy.

Można użyć `AVAudioPlayer` klasy wykonać następujące czynności:

- Odtwarzanie dźwięków z dowolnego czasu trwania z pętli opcjonalne.
- Odtwarzanie dźwięków wielu jednocześnie z synchronizacją opcjonalne.
- Sterowanie woluminu, szybkość odtwarzania i stereo rozmieszczania dla każdego odtwarzanie dźwięku.
- Obsługuje funkcje, takie jak szybkie przewijanie do przodu lub do tyłu.
- Uzyskiwanie poziomu odtwarzania pomiaru danych.

`AVAudioPlayer` obsługuje dźwięki format audio dostarczane przez system iOS, systemu tvOS i macOS, takich jak .aif, .wav lub .mp3.

## <a name="playing-sounds-in-macos"></a>Odtwarzanie dźwięków w macOS

Ponieważ system macOS obsługuje tej samej klasy przybornika Audio jako dla systemu iOS, zobacz nasze iOS [odtwarzanie dźwięku z AVAudioPlayer](https://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) dokumentację, aby uzyskać szczegółowe informacje dotyczące odtwarzania audio w aplikacji Xamarin.Mac.



## <a name="related-links"></a>Linki pokrewne

- [Odwołanie AVAudioPlayer](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
