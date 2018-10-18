---
title: Rozpoczynanie pracy z usługą iOS 12, 12 systemu tvOS i watchOS 5
description: W tym dokumencie opisano, jak uzyskać Ustaw maksymalnie kompilacji dla systemu iOS 12, 12 systemu tvOS i watchOS 5 aplikacji za pomocą platformy Xamarin. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac i Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/19/2018
ms.openlocfilehash: 77589d0d644c366fc0feacd874929c7456b4ae30
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615177"
---
# <a name="get-started-with-ios-12-tvos-12-and-watchos-5"></a>Rozpoczynanie pracy z usługą iOS 12, 12 systemu tvOS i watchOS 5

W tym dokumencie opisano, jak rozpocząć Kompilowanie aplikacji platformy Xamarin, które wywołują interfejsy API za pomocą środowiska Xcode 10, iOS 12, 12 systemu tvOS i watchOS 5.

## <a name="download-and-install"></a>Pobierz i zainstaluj

1. **Zainstaluj program Xcode 10** — deweloperzy zarejestrowany Apple można pobrać i zainstalować najnowszą wersję Xcode 10 z [portalu Apple Developer Portal](https://developer.apple.com/download/) lub **App Store**.

2. **Uruchom program Xcode 10** — Uruchom 10 Xcode, zanim aktualizacji i programem Visual Studio for Mac lub Visual Studio 2017, niektóre podczas instalacji narzędzia Xamarin tego wymaga.

3. **Aktualizacja programu Visual Studio dla komputerów Mac i Visual Studio 2017** — upewnij się, że najnowsza stabilna wersja platformy Xamarin.

4. _(opcjonalnie)_  **Zainstaluj dla systemu iOS 12 na urządzeniach z systemem iOS** —

   W przypadku testowania aplikacji, które wprowadzono interfejsów API za pomocą Xcode 10 urządzenia zarejestrowanego deweloperów firmy Apple można [Pobierz](https://developer.apple.com/download) i zainstalować system operacyjny na swoich urządzeniach.

   > [!TIP]
   > Nawet wtedy, gdy aplikacja nie używa żadnych nowych interfejsów API, należy ją skompilować przy użyciu najnowszych zestawów 10 SDK dla środowiska Xcode i go przetestować, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami. Jeśli aplikacja nie wywołać wszelkie nowe interfejsy API, możesz ponownie go skompilować przy użyciu tych nowych zestawów SDK i przetestować go na urządzeniach, które nie zostały jeszcze uaktualnione do nowego systemu operacyjnego.
   >
   > Zanim uaktualniania urządzeń do najnowszego systemu operacyjnego wydań z firmy Apple, aby testować aplikacje platformy Xamarin, należy koniecznie:
   >
   > - Odczyt [firmy Apple wersji](https://developer.apple.com/download/) aktualizacji systemu operacyjnego.
   > - Przeczytaj platformy Xamarin w wersji zapoznawczej [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-6/).

## <a name="related-links"></a>Linki pokrewne

- [Pobierz środowisko Xcode 10](https://developer.apple.com/download/)
