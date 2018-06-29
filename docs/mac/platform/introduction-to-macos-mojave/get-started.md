---
title: Wprowadzenie do korzystania z macOS Mojave
description: Ten dokument zawiera opis sposobu uzyskać ustawione do kompilacji macOS Mojave aplikacji za pomocą Xamarin.Mac. Opisano, jak pobrać Xcode 10 i aktualizacji programu Visual Studio dla komputerów Mac.
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: ee5269bf314401328d0184631e817b37ce091479
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067181"
---
# <a name="getting-started-with-macos-mojave"></a>Wprowadzenie do korzystania z macOS Mojave

![Wersja zapoznawcza](~/media/shared/preview.png)

> [!WARNING]
> MacOS na platformie Xamarin Mojave pomocy technicznej jest obecnie w przeglądzie, co oznacza, że może zawierać usterki, nie jest zakończenie funkcji i mogą ulec zmianie.
> Używać go tylko eksperymenty.

> [!NOTE]
> Aby uzyskać więcej informacji, przeczytaj [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin wersja preview.

Ten dokument zawiera opis sposobu uzyskać ustawione do kompilacji macOS Mojave aplikacji za pomocą Xamarin.Mac. Opisano, jak pobrać Xcode 10 i aktualizacji programu Visual Studio dla komputerów Mac.

## <a name="download-and-install"></a>Pobierz i zainstaluj

1. **Zainstaluj najnowszą wersję beta Xcode 10** — deweloperzy zarejestrowany Apple można pobrać i zainstalować najnowszą wersję Xcode 10 z [portalu dla deweloperów firmy Apple](https://developer.apple.com/download/).

   > [!NOTE]
   > System macOS Mojave SDK jest dystrybuowane w programie Xcode 10.

2. **Uruchom Xcode 10** — Uruchom 10 Xcode przed aktualizacji i działanie programu Visual Studio dla komputerów Mac; instaluje pewne narzędzia, które wymaga Xamarin.

3. **Aktualizacja programu Visual Studio dla komputerów Mac** — postępuj zgodnie z instrukcjami [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) do zainstalowania platformy Xamarin w wersji zapoznawczej.

4. _(opcjonalnie)_  **Zainstalowania najnowszej wersji beta Mojave macOS na komputerze Mac** — do przetestowania Xamarin.Mac aplikacji, które używają nowo wdrożonych macOS Mojave interfejsów API, zarejestrowanych może deweloperów firmy Apple [Pobierz](https://developer.apple.com/download/) i zainstaluj najnowsze macOS Mojave deweloperów w wersji beta.

   > [!TIP]
   > Nawet w przypadku aplikacji nie korzysta z żadnych nowych macOS Mojave interfejsów API, pamiętaj skompilować go z macOS Mojave zainstalowany zestaw SDK (w programie Xcode 10) i przetestować, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami. Jeśli aplikacja nie wywołuje żadnych nowych interfejsów API, można ponownie go skompilować macOS Mojave SDK i przetestować go bez uaktualniania systemu operacyjnego komputera Mac.

   > [!IMPORTANT]
   > Przed rozpoczęciem uaktualniania komputerów Mac do macOS Mojave umożliwiający zbudowanie i przetestowanie Xamarin.Mac aplikacje, które wywołują macOS nowych interfejsów API Mojave:
   > - Odczyt [wersji firmy Apple](https://developer.apple.com/download/) aktualizacji systemu operacyjnego.
   > - Odczyt [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin wersja preview. Zwróć uwagę, że pierwszy Podgląd nie obejmuje powiązania macOS nowych interfejsów API AppKit Mojave (takim jak tryb ciemny).

## <a name="related-links"></a>Linki pokrewne

- [Pobierz Xcode 10](https://developer.apple.com/download/)
- Podgląd Xamarin [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/)
