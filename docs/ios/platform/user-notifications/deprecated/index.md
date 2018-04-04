---
title: Technologie przestarzałe powiadomień
ms.prod: xamarin
ms.assetid: 20C4F6E5-56DF-4A85-BBF0-E38C88586307
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2016
ms.openlocfilehash: eff1d999e705aa493d0481e34ead3b9b81d434f9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="deprecated-notification-technologies"></a>Technologie przestarzałe powiadomień

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
