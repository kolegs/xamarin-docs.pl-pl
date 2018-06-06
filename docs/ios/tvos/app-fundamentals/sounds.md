---
title: Odtwarzanie dźwięku w systemu tvOS z AVAudioPlayer w Xamarin
description: W tym artykule pokazano, jak użyć Klasa pomocy do sterowania odtwarzanie dźwięku za pomocą AVAudioPlayer w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7d95a8ea6c22c0d897d8ccfe0c2ca401f6523783
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788637"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Odtwarzanie dźwięku w systemu tvOS z AVAudioPlayer w Xamarin

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
