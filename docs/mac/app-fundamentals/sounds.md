---
title: Odtwarzanie dźwięku z AVAudioPlayer w Xamarin.Mac
description: Ten dokument zawiera opis sposobu odtwarzanie dźwięku z AVAudioPlayer w aplikacji Xamarin.Mac. Zawarto informacje AVAudioPlayer na wysokim poziomie oraz linki do dokumentacji, która opisuje on dokładniejszego.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 9e5b9ec43189999f8a0aee29eb50221b494e2133
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791858"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Odtwarzanie dźwięku z AVAudioPlayer w Xamarin.Mac

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
