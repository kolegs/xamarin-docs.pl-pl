---
title: Wprowadzenie do korzystania z systemu iOS 12 i systemu tvOS 12
description: Ten dokument zawiera opis sposobu uzyskać ustawione do kompilacji systemu iOS 12 i systemu tvOS 12 aplikacji za pomocą platformy Xamarin. Zawarto informacje, jak pobrać Xcode 10 i aktualizacji programu Visual Studio for Mac i Visual Studio 2017 r.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 887a5cc72b951b2f115cdc6998a6cf6ca7a94df0
ms.sourcegitcommit: 4939748d537cbb1934a3b5b22add39ef3d9aa64d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37091561"
---
# <a name="getting-started-with-ios-12-and-tvos-12"></a>Wprowadzenie do korzystania z systemu iOS 12 i systemu tvOS 12

![Wersja zapoznawcza](~/media/shared/preview.png)

> [!WARNING]
> Obsługa platformy Xamarin dla systemu iOS 12 i systemu tvOS 12 jest obecnie w wersji zapoznawczej, co oznacza, że może ona zawierać usterki, nie jest zakończenie funkcji, i może ulec zmianie. Używać go tylko eksperymenty.

> [!NOTE]
> Aby uzyskać więcej informacji, przeczytaj [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin wersja preview.

Ten dokument zawiera opis sposobu uzyskać ustawione do kompilacji systemu iOS 12 i systemu tvOS 12 aplikacji za pomocą platformy Xamarin. Zawarto informacje, jak pobrać Xcode 10 i aktualizacji programu Visual Studio for Mac i Visual Studio 2017 r.

## <a name="download-and-install"></a>Pobierz i zainstaluj

1. **Zainstaluj najnowszą wersję beta Xcode 10** — deweloperzy zarejestrowany Apple można pobrać i zainstalować najnowszą wersję Xcode 10 z [portalu dla deweloperów firmy Apple](https://developer.apple.com/download/).

   > [!NOTE]
   > IOS 12 i zestawy SDK systemu tvOS 12 są dystrybuowane wraz z Xcode 10.

2. **Uruchom Xcode 10** — Uruchom 10 Xcode przed aktualizacji i działanie programu Visual Studio dla komputerów Mac lub Visual Studio 2017; instaluje pewne narzędzia, które wymaga Xamarin.

3. **Aktualizacja programu Visual Studio for Mac i Visual Studio 2017** — postępuj zgodnie z instrukcjami w [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) do zainstalowania platformy Xamarin w wersji zapoznawczej.

4. _(opcjonalnie)_  **Zainstalowania najnowszej wersji beta systemu iOS na urządzeniach z systemem iOS** — w przypadku urządzeń testowania aplikacji iOS nowo wdrożonych Użyj 12 lub systemu tvOS 12 interfejsów API, zarejestrowanych deweloperów firmy Apple mogących [Pobierz](https://developer.apple.com/download) i zainstaluj najnowsze iOS 12 lub na urządzeniach z systemem iOS i systemu tvOS, wersje beta dewelopera systemu tvOS 12.

   > [!TIP]
   > Nawet w przypadku aplikacji nie korzysta z żadnych nowych iOS 12 ani systemu tvOS 12 interfejsów API, pamiętaj go skompilować z systemem iOS 12 lub systemu tvOS 12 SDK (zainstalowaną z Xcode 10) i testowych aby upewnić się, że wszystko działa jako oczekuje. Jeśli aplikacja nie wywołuje żadnych nowych interfejsów API, użytkownik może ponownie go skompilować iOS 12 lub systemu tvOS 12 zestawu SDK i przetestować go na urządzeniach, które nie zostały jeszcze uaktualnione do nowych systemów operacyjnych.

   > [!IMPORTANT]
   > Przed rozpoczęciem uaktualniania urządzeń z systemem iOS 12 lub systemu tvOS 12, aby przetestować aplikacje platformy Xamarin, które wywołują nowe iOS 12 lub systemu tvOS 12 interfejsów API:
   > - Odczyt [wersji firmy Apple](https://developer.apple.com/download/) aktualizacji systemu operacyjnego.
   > - Odczyt [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin wersja preview.

## <a name="related-links"></a>Linki pokrewne

- [Pobierz Xcode 10](https://developer.apple.com/download/)
- Podgląd Xamarin [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/)
