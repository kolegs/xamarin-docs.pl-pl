---
title: Rozpoczynanie pracy z systemem macOS Mojave
description: W tym dokumencie opisano sposób uzyskiwania do kompilacji aplikacji Mojave z rozszerzeniem Xamarin.Mac systemu macOS. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac.
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: 1d219808acaab3c6db089d341d1d91f34398a622
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615369"
---
# <a name="get-started-with-macos-mojave"></a>Rozpoczynanie pracy z systemem macOS Mojave

W tym dokumencie opisano sposób uzyskiwania do kompilacji aplikacji Mojave z rozszerzeniem Xamarin.Mac systemu macOS. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac.

## <a name="download-and-install"></a>Pobierz i zainstaluj

1. **Zainstaluj najnowszą wersję beta Xcode 10** — deweloperzy zarejestrowany Apple można pobrać i zainstalować najnowszą wersję Xcode 10 z [portalu Apple Developer Portal](https://developer.apple.com/download/).

2. **Uruchom program Xcode 10** — Uruchom 10 Xcode przed aktualizacji i programem Visual Studio dla komputerów Mac; instaluje pewne narzędzia, które wymaga platformy Xamarin.

3. **Aktualizacja programu Visual Studio dla komputerów Mac** — za pomocą najnowszej stabilnej wersji programu Visual Studio dla komputerów Mac, [Xamarin.Mac 5.0](https://developer.xamarin.com/releases/mac/xamarin.mac_5/xamarin.mac_5.0/) lub nowszej.

4. _(opcjonalnie)_  **Instalowanie w macOS Mojave na komputerze Mac** —

   > [!TIP]
   > Nawet wtedy, gdy aplikacja nie korzysta z żadnych nowych Mojave interfejsów API w systemie macOS, należy ją skompilować przy użyciu zestawu SDK Mojave z systemem macOS i go przetestować, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami. Jeśli aplikacja nie wywołać wszelkie nowe interfejsy API, można ponownie go skompilować z systemem macOS Mojave SDK i jego przetestowanie bez uaktualniania systemu operacyjnego komputera Mac.
   >
   > Przed uaktualnieniem komputera Mac, do systemu macOS Mojave do tworzenia i testowania aplikacji platformy Xamarin.Mac, odwołujących się do nowego systemu macOS Mojave interfejsów API:
   >
   > - Odczyt [firmy Apple wersji](https://developer.apple.com/download/) dla aktualizacji systemu operacyjnego.

## <a name="related-links"></a>Linki pokrewne

- [Pobierz środowisko Xcode 10](https://developer.apple.com/download/)
- [Informacje o wersji platformy Xamarin.Mac 5.0](https://developer.xamarin.com/releases/mac/xamarin.mac_5/xamarin.mac_5.0/)
