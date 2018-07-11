---
title: Wprowadzenie do 12 systemu iOS i tvOS 12
description: W tym dokumencie opisano sposób uzyskiwania do kompilacji dla systemu iOS 12 i aplikacje dla systemu tvOS 12 za pomocą platformy Xamarin. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac i Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 887a5cc72b951b2f115cdc6998a6cf6ca7a94df0
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815634"
---
# <a name="getting-started-with-ios-12-and-tvos-12"></a>Wprowadzenie do 12 systemu iOS i tvOS 12

![Wersja zapoznawcza](~/media/shared/preview.png)

> [!WARNING]
> Xamarin dla systemu iOS 12 i tvOS 12 pomocy technicznej jest obecnie dostępny w wersji zapoznawczej, co oznacza, że może ona zawierać błędy, nie jest pełną funkcją, i mogą ulec zmianie. Na użytek go tylko do eksperymentowania.

> [!NOTE]
> Aby uzyskać więcej informacji, przeczytaj [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) dla platformy Xamarin w wersji zapoznawczej.

W tym dokumencie opisano sposób uzyskiwania do kompilacji dla systemu iOS 12 i aplikacje dla systemu tvOS 12 za pomocą platformy Xamarin. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac i Visual Studio 2017.

## <a name="download-and-install"></a>Pobierz i zainstaluj

1. **Zainstaluj najnowszą wersję beta Xcode 10** — deweloperzy zarejestrowany Apple można pobrać i zainstalować najnowszą wersję Xcode 10 z [portalu Apple Developer Portal](https://developer.apple.com/download/).

   > [!NOTE]
   > IOS 12 i zestawy SDK dla systemu tvOS 12 są dystrybuowane za pomocą środowiska Xcode 10.

2. **Uruchom program Xcode 10** — Uruchom 10 Xcode przed aktualizacji i programem Visual Studio for Mac lub Visual Studio 2017; instaluje pewne narzędzia, które wymaga platformy Xamarin.

3. **Aktualizacja programu Visual Studio dla komputerów Mac i Visual Studio 2017** — postępuj zgodnie z instrukcjami w [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) instalacji Xamarin w wersji zapoznawczej.

4. _(opcjonalnie)_  **Zainstaluj najnowszą wersję beta dla systemu iOS na urządzeniach z systemem iOS** — w przypadku urządzeń testowania aplikacji, użyj nowo wdrożonych aplikacji systemu iOS 12 lub tvOS 12 interfejsów API, zarejestrowanych deweloperów firmy Apple można [Pobierz](https://developer.apple.com/download) i zainstaluj iOS najnowsze 12 lub beta dla deweloperów systemu tvOS 12 na urządzeniach z systemem iOS i tvOS.

   > [!TIP]
   > Nawet jeśli aplikacja używa nowego systemu iOS 12 ani systemu tvOS 12 interfejsów API, należy ją skompilować przy użyciu systemu iOS 12 lub tvOS 12 zestawu SDK (zainstalowaną z 10 środowiska Xcode) i testowanie ją, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami. Jeśli aplikacja nie wywołać wszelkie nowe interfejsy API, można ponownie skompilować je przy użyciu systemu iOS 12 lub tvOS 12 zestawu SDK i przetestować go na urządzeniach, które nie zostały jeszcze uaktualnione do nowych systemów operacyjnych.

   > [!IMPORTANT]
   > Przed rozpoczęciem uaktualniania urządzeń z systemem iOS 12 lub tvOS 12, aby przetestować aplikacje platformy Xamarin, które wywołują nowego systemu iOS 12 lub tvOS 12 interfejsów API:
   > - Odczyt [firmy Apple wersji](https://developer.apple.com/download/) dla aktualizacji systemu operacyjnego.
   > - Odczyt [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) dla platformy Xamarin w wersji zapoznawczej.

## <a name="related-links"></a>Linki pokrewne

- [Pobierz środowisko Xcode 10](https://developer.apple.com/download/)
- Xamarin (wersja zapoznawcza) [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/)
