---
title: Wprowadzenie do systemów iOS 12, 12 systemu tvOS i watchOS 5
description: W tym dokumencie opisano, jak uzyskać Ustaw maksymalnie kompilacji dla systemu iOS 12, 12 systemu tvOS i watchOS 5 aplikacji za pomocą platformy Xamarin. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac i Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/07/2018
ms.openlocfilehash: cb84ddc646933d253ca72fe8f9f581364aab698b
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615177"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>Wprowadzenie do systemów iOS 12, 12 systemu tvOS i watchOS 5

![Wersja zapoznawcza](~/media/shared/preview.png)

> [!WARNING]
> Obsługa platformy Xamarin iOS 12, 12 systemu tvOS i watchOS 5 zestawów SDK, rozpowszechniać za pomocą środowiska Xcode 10 jest obecnie w wersji zapoznawczej, co oznacza, że może ona zawierać błędy, nie jest funkcją ukończenia i mogą ulec zmianie. Na użytek go tylko do eksperymentowania.

W tym dokumencie opisano sposób Przygotuj się do tworzenia aplikacji platformy Xamarin, które wywołują interfejsy API w programie Xcode 10. Omówiono w nim sposób pobierania Xcode 10 i zaktualizować program Visual Studio dla komputerów Mac i Visual Studio 2017.

## <a name="download-and-install"></a>Pobierz i zainstaluj

1. **Zainstaluj najnowszą wersję beta Xcode 10** — deweloperzy zarejestrowany Apple można pobrać i zainstalować najnowszą wersję Xcode 10 z [portalu Apple Developer Portal](https://developer.apple.com/download/).

2. **Uruchom program Xcode 10** — Uruchom 10 Xcode, zanim aktualizacji i programem Visual Studio for Mac lub Visual Studio 2017, niektóre podczas instalacji narzędzia Xamarin tego wymaga.

3. **Aktualizacja programu Visual Studio dla komputerów Mac i Visual Studio 2017** — postępuj zgodnie z instrukcjami [wersji blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-5/) instalacji Xamarin w wersji zapoznawczej.

4. _(opcjonalnie)_  **Zainstaluj najnowszą wersję beta dla systemu iOS na urządzeniach z systemem iOS** — służy do testowania aplikacji korzystających z interfejsów API, wprowadzona w systemie 10 Xcode zarejestrowanych może deweloperów firmy Apple urządzenia [Pobierz](https://developer.apple.com/download) i zainstaluj najnowszą wersję wersje beta dla deweloperów na swoich urządzeniach.

   > [!TIP]
   > Nawet wtedy, gdy aplikacja nie używa żadnych nowych interfejsów API, należy ją skompilować przy użyciu najnowszych zestawów 10 SDK dla środowiska Xcode i go przetestować, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami. Jeśli aplikacja nie wywołać wszelkie nowe interfejsy API, możesz ponownie go skompilować przy użyciu tych nowych zestawów SDK i przetestować go na urządzeniach, które nie zostały jeszcze uaktualnione do nowego systemu operacyjnego.
   >
   > Zanim uaktualniania urządzeń do najnowszego systemu operacyjnego wydań z firmy Apple, aby testować aplikacje platformy Xamarin, należy koniecznie:
   >
   > - Odczyt [firmy Apple wersji](https://developer.apple.com/download/) aktualizacji systemu operacyjnego.
   > - Przeczytaj platformy Xamarin w wersji zapoznawczej [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-5/).

## <a name="related-links"></a>Linki pokrewne

- [Pobierz środowisko Xcode 10](https://developer.apple.com/download/)
- Xamarin (wersja zapoznawcza) [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
