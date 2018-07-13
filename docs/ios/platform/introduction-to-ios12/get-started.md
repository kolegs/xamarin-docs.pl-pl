---
title: Wprowadzenie do systemów iOS 12, 12 systemu tvOS i watchOS 5
description: W tym dokumencie opisano sposób uzyskiwania do kompilacji dla systemu iOS 12 i aplikacje dla systemu tvOS 12 za pomocą platformy Xamarin. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac i Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 70f67f934c2503e6f6fa0d3bae1f37bcc1f6f0a4
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030655"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>Wprowadzenie do systemów iOS 12, 12 systemu tvOS i watchOS 5

![Wersja zapoznawcza](~/media/shared/preview.png)

> [!WARNING]
> Xamarin dla systemu iOS 12, tvOS 12, i pomocy technicznej systemu watchOS 5 jest obecnie dostępny w wersji zapoznawczej, co oznacza, że może ona zawierać błędy, nie jest pełną funkcją, i mogą ulec zmianie. Na użytek go tylko do eksperymentowania.

> [!NOTE]
> Aby uzyskać więcej informacji, przeczytaj platformy Xamarin w wersji zapoznawczej [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

W tym dokumencie opisano, jak uzyskać Ustaw maksymalnie kompilacji dla systemu iOS 12, 12 systemu tvOS i watchOS 5 aplikacji za pomocą platformy Xamarin. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac i Visual Studio 2017.

## <a name="download-and-install"></a>Pobierz i zainstaluj

1. **Zainstaluj najnowszą wersję beta Xcode 10** — deweloperzy zarejestrowany Apple można pobrać i zainstalować najnowszą wersję Xcode 10 z [portalu Apple Developer Portal](https://developer.apple.com/download/).

   > [!NOTE]
   > 12 systemu iOS, tvOS 12 i zestawy SDK watchOS 5 są dystrybuowane za pomocą środowiska Xcode 10.

2. **Uruchom program Xcode 10** — Uruchom 10 Xcode przed aktualizacji i programem Visual Studio for Mac lub Visual Studio 2017; instaluje pewne narzędzia, które wymaga platformy Xamarin.

3. **Aktualizacja programu Visual Studio dla komputerów Mac i Visual Studio 2017** — postępuj zgodnie z instrukcjami [wersji blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-3/) instalacji Xamarin w wersji zapoznawczej.

4. _(opcjonalnie)_  **Zainstaluj najnowszą wersję beta dla systemu iOS na urządzeniach z systemem iOS** — w przypadku urządzeń testowania aplikacji, użyj nowo wprowadzonego dla systemu iOS 12, 12 systemu tvOS lub systemu watchOS 5 interfejsów API, zarejestrowanych deweloperów firmy Apple można [Pobierz](https://developer.apple.com/download) i Zainstaluj najnowsze 12 dla systemu iOS, tvOS 12 lub systemu watchOS 5 dla deweloperów w wersji beta na swoich urządzeniach.

   > [!TIP]
   > Nawet w przypadku aplikacji nie korzystać z nowych 12 dla systemu iOS, tvOS 12 lub systemu watchOS 5 interfejsów API, należy ją skompilować przy użyciu 12 systemu iOS, tvOS 12 lub systemu watchOS 5 zestawu SDK (zainstalowaną z 10 środowiska Xcode) i testowanie ją, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami. Jeśli aplikacja nie wywołać wszelkie nowe interfejsy API, można ponownie skompilować go z 12 systemu iOS, tvOS 12 lub systemu watchOS 5 zestawu SDK i przetestować go na urządzeniach, które nie zostały jeszcze uaktualnione do nowych systemów operacyjnych.

   > [!IMPORTANT]
   > Przed rozpoczęciem uaktualniania urządzeń do 12 systemu iOS, tvOS 12 lub systemu watchOS 5 Testowanie aplikacji platformy Xamarin, które wywołują nowe 12 dla systemu iOS, tvOS 12 lub systemu watchOS 5 interfejsów API:
   > - Odczyt [firmy Apple wersji](https://developer.apple.com/download/) dla aktualizacji systemu operacyjnego.
   > - Przeczytaj platformy Xamarin w wersji zapoznawczej [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

## <a name="related-links"></a>Linki pokrewne

- [Pobierz środowisko Xcode 10](https://developer.apple.com/download/)
- Xamarin (wersja zapoznawcza) [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)
