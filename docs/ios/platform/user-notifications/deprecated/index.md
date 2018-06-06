---
title: Technologie przestarzałe powiadomień platformy Xamarin.iOS
description: W tym dokumencie opisano technologie powiadomień dla systemu iOS, które została zastąpiona framework powiadomienia użytkownika, wprowadzone w systemie iOS 10.
ms.prod: xamarin
ms.assetid: 20C4F6E5-56DF-4A85-BBF0-E38C88586307
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2016
ms.openlocfilehash: 4becc5e296fb6e2496d9ffd863f7137419480262
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788556"
---
# <a name="deprecated-notification-technologies-in-xamarinios"></a>Technologie przestarzałe powiadomień platformy Xamarin.iOS

W tej sekcji przedstawiono sposób wdrożenia lokalnego i powiadomienia wypychane w platformy Xamarin.iOS. Zostanie wyjaśnić różnych elementów interfejsu użytkownika powiadomienie z systemem iOS i omówienia interfejsu API użytkownika związane z tworzenie i wyświetlanie powiadomienie.

> [!IMPORTANT]
> Informacje przedstawione w tej sekcji dotyczą systemu iOS 9 i wcześniejszych, pozostał tutaj do obsługi starszych wersji systemu iOS. Dla systemu iOS 10 i nowsze, zobacz [Framework powiadomienia użytkownika — przewodnik](~/ios/platform/user-notifications/index.md) do obsługi w pliku lokalnym i zdalnym powiadomienia na urządzeniu z systemem iOS.

## <a name="sections"></a>Sekcje

<a name="Local Notifications In iOS" />

##  <a name="local-notifications-in-ioslocal-notifications-in-iosmd"></a>[Lokalnego powiadomienia w systemie iOS](local-notifications-in-ios.md)

W tej sekcji będzie omawiać temat do zaimplementowania lokalnego powiadomienia w platformy Xamarin.iOS. Zostanie wyjaśnić różnych elementów interfejsu użytkownika powiadomienie z systemem iOS i omówienia interfejsu API użytkownika związane z tworzenie i wyświetlanie powiadomienie.

<a name="Local Notifications Walkthrough" />

##  <a name="walkthrough---using-local-notifications-in-xamarinioslocal-notifications-in-ios-walkthroughmd"></a>[Przewodnik: używanie powiadomień lokalnych w rozszerzeniu Xamarin.iOS](local-notifications-in-ios-walkthrough.md)

W tej sekcji zostanie omówiony sposób użycia lokalnego powiadomienia w aplikacji platformy Xamarin.iOS. Będzie on pokazują podstawy tworzenia i publikowania powiadomienie, które będą wyskakujące alert, gdy odebrana w aplikacji.

<a name="Remote Notifications In iOS" />

##  <a name="remote-notifications-in-iosremote-notifications-in-iosmd"></a>[Powiadomienia zdalnego w systemie iOS](remote-notifications-in-ios.md)

W tej sekcji opisano powiadomień wypychanych w systemie iOS. Podaj powiadomienia bramy Service (APNS, Apple Push) oraz roli, która jest odtwarzany w publikacji powiadomienia do aplikacji systemu iOS. Go objaśnia sposób tworzenia niezbędne do włączenia powiadomień wypychanych i omówiono w nim certyfikaty zabezpieczeń. Na koniec w tej sekcji będzie omawiać niektóre zadania celów, które serwery aplikacji należy wykonać, aby śledzić klienckie urządzenia przenośne.

## <a name="related-links"></a>Linki pokrewne

- [Powiadomienia (na przykład)](https://developer.xamarin.com/samples/monotouch/Notifications/)
