---
title: Wprowadzenie do systemu macOS Mojave
description: W tym dokumencie opisano sposób uzyskiwania do kompilacji aplikacji Mojave z rozszerzeniem Xamarin.Mac systemu macOS. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac.
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: ee5269bf314401328d0184631e817b37ce091479
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831348"
---
# <a name="getting-started-with-macos-mojave"></a>Wprowadzenie do systemu macOS Mojave

![Wersja zapoznawcza](~/media/shared/preview.png)

> [!WARNING]
> Xamarin z systemem macOS Mojave pomocy technicznej jest obecnie dostępna w wersji zapoznawczej, co oznacza, że może zawierać błędy, nie jest uzupełnianie przez funkcję i mogą ulec zmianie.
> Na użytek go tylko do eksperymentowania.

> [!NOTE]
> Aby uzyskać więcej informacji, przeczytaj [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) dla platformy Xamarin w wersji zapoznawczej.

W tym dokumencie opisano sposób uzyskiwania do kompilacji aplikacji Mojave z rozszerzeniem Xamarin.Mac systemu macOS. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac.

## <a name="download-and-install"></a>Pobierz i zainstaluj

1. **Zainstaluj najnowszą wersję beta Xcode 10** — deweloperzy zarejestrowany Apple można pobrać i zainstalować najnowszą wersję Xcode 10 z [portalu Apple Developer Portal](https://developer.apple.com/download/).

   > [!NOTE]
   > Zestaw SDK Mojave z systemem macOS jest rozpowszechniana z Xcode 10.

2. **Uruchom program Xcode 10** — Uruchom 10 Xcode przed aktualizacji i programem Visual Studio dla komputerów Mac; instaluje pewne narzędzia, które wymaga platformy Xamarin.

3. **Aktualizacja programu Visual Studio dla komputerów Mac** — postępuj zgodnie z instrukcjami w [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) instalacji Xamarin w wersji zapoznawczej.

4. _(opcjonalnie)_  **Zainstaluj najnowszą wersję beta Mojave systemu macOS na komputerze Mac** — w celu testowania aplikacji rozszerzenia Xamarin.Mac, korzystających z nowo wprowadzonego macOS Mojave interfejsów API zarejestrowanych może deweloperów firmy Apple [Pobierz](https://developer.apple.com/download/) i zainstaluj najnowsze macOS Mojave dla deweloperów w wersji beta.

   > [!TIP]
   > Nawet wtedy, gdy aplikacja nie korzysta z żadnych nowych Mojave interfejsów API w systemie macOS, należy ją skompilować z systemem macOS Mojave zainstalowany zestaw SDK (z Xcode 10) oraz go przetestować, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami. Jeśli aplikacja nie wywołać wszelkie nowe interfejsy API, można ponownie go skompilować z systemem macOS Mojave SDK i jego przetestowanie bez uaktualniania systemu operacyjnego komputera Mac.

   > [!IMPORTANT]
   > Przed uaktualnieniem komputera Mac, do systemu macOS Mojave do tworzenia i testowania aplikacji platformy Xamarin.Mac, odwołujących się do nowego systemu macOS Mojave interfejsów API:
   > - Odczyt [firmy Apple wersji](https://developer.apple.com/download/) dla aktualizacji systemu operacyjnego.
   > - Odczyt [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) dla platformy Xamarin w wersji zapoznawczej. Należy pamiętać, to pierwsza wersja zapoznawcza nie ma powiązań dla nowego systemu macOS Mojave AppKit API (takie jak tryb ciemny).

## <a name="related-links"></a>Linki pokrewne

- [Pobierz środowisko Xcode 10](https://developer.apple.com/download/)
- Xamarin (wersja zapoznawcza) [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/)
