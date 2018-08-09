---
title: Wprowadzenie do systemu macOS Mojave
description: W tym dokumencie opisano sposób uzyskiwania do kompilacji aplikacji Mojave z rozszerzeniem Xamarin.Mac systemu macOS. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac.
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/08/2018
ms.openlocfilehash: 213f1feb53abf4a4eb00ae0d65c312eaaec95614
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615369"
---
# <a name="getting-started-with-macos-mojave"></a>Wprowadzenie do systemu macOS Mojave

![Wersja zapoznawcza](~/media/shared/preview.png)

> [!WARNING]
> Xamarin z systemem macOS Mojave pomocy technicznej jest obecnie dostępna w wersji zapoznawczej, co oznacza, że może zawierać błędy, nie jest uzupełnianie przez funkcję i mogą ulec zmianie.
> Na użytek go tylko do eksperymentowania. 

W tym dokumencie opisano sposób uzyskiwania do kompilacji aplikacji Mojave z rozszerzeniem Xamarin.Mac systemu macOS. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac.

## <a name="download-and-install"></a>Pobierz i zainstaluj

1. **Zainstaluj najnowszą wersję beta Xcode 10** — deweloperzy zarejestrowany Apple można pobrać i zainstalować najnowszą wersję Xcode 10 z [portalu Apple Developer Portal](https://developer.apple.com/download/).

2. **Uruchom program Xcode 10** — Uruchom 10 Xcode przed aktualizacji i programem Visual Studio dla komputerów Mac; instaluje pewne narzędzia, które wymaga platformy Xamarin.

3. **Aktualizacja programu Visual Studio dla komputerów Mac** — postępuj zgodnie z instrukcjami [wersji blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-5/) instalacji Xamarin w wersji zapoznawczej.

4. _(opcjonalnie)_  **Zainstaluj najnowszą wersję beta Mojave systemu macOS na komputerze Mac** — w celu testowania aplikacji rozszerzenia Xamarin.Mac, korzystających z nowo wprowadzonego macOS Mojave interfejsów API zarejestrowanych może deweloperów firmy Apple [Pobierz](https://developer.apple.com/download/) i zainstaluj najnowsze macOS Mojave dla deweloperów w wersji beta.

   > [!TIP]
   > Nawet wtedy, gdy aplikacja nie korzysta z żadnych nowych Mojave interfejsów API w systemie macOS, należy ją skompilować przy użyciu zestawu SDK Mojave z systemem macOS i go przetestować, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami. Jeśli aplikacja nie wywołać wszelkie nowe interfejsy API, można ponownie go skompilować z systemem macOS Mojave SDK i jego przetestowanie bez uaktualniania systemu operacyjnego komputera Mac.
   >
   > Przed uaktualnieniem komputera Mac, do systemu macOS Mojave do tworzenia i testowania aplikacji platformy Xamarin.Mac, odwołujących się do nowego systemu macOS Mojave interfejsów API:
   >
   > - Odczyt [firmy Apple wersji](https://developer.apple.com/download/) dla aktualizacji systemu operacyjnego.
   > - Przeczytaj platformy Xamarin w wersji zapoznawczej [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-5/).

## <a name="related-links"></a>Linki pokrewne

- [Pobierz środowisko Xcode 10](https://developer.apple.com/download/)
- Xamarin (wersja zapoznawcza) [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
