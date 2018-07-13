---
title: funkcje platformy systemu watchOS
description: Ten dokument prowadzi różne przewodniki z instrukcjami, które opisują funkcjami platformy systemu watchOS, takimi jak Apple Pay, powiadomienia, kompilacji, sugestie proaktywne, aplikacji do ćwiczeń i inne.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: e915ec38ed29b6ef2a0710c9dad10cf339c3a3ab
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030681"
---
# <a name="watchos-platform-features"></a>funkcje platformy systemu watchOS

## <a name="introduction-to-watchos-5introduction-to-watchos5indexmd"></a>[Wprowadzenie do systemu watchOS 5](introduction-to-watchos5/index.md)

> [!WARNING]
> Obsługa systemu watchOS 5 platformy Xamarin jest obecnie dostępny w wersji zapoznawczej, co oznacza, że może ona zawierać błędy, nie jest pełną funkcją, i mogą ulec zmianie.
> Na użytek go tylko do eksperymentowania.

Ten dokument zawiera ogólne omówienie nowe i zaktualizowane funkcje w systemie watchOS 5 są dostępne do użycia podczas tworzenia aplikacji systemu watchOS za pomocą platformy Xamarin.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Wprowadzenie do systemu watchOS 4](introduction-to-watchos4.md)

Ten dokument zawiera ogólne omówienie funkcji dodane lub zaktualizowane w systemie watchOS 4.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Wprowadzenie do systemu watchOS 3](introduction-to-watchos3/index.md)

W tym artykule opisano nowe i zaktualizowane interfejsy API w systemu watchOS 3.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Ulepszenia Apple Pay](~/ios/watchos/platform/apple-pay.md)

W systemie watchOS 3 PassKit framework został rozszerzony w celu umożliwienia obsługi bezpiecznego, w aplikacji płatności (fizycznych towarów i usług) dla aplikacji uruchomionych na Apple Watch.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Zadania w tle](~/ios/watchos/platform/background-tasks.md)

systemu watchOS 3 wprowadza kilka zadań w tle, które aplikacji można użyć do zaktualizowania jej informacje o zapewnienie, że jego zawartość jest użytkownik potrzebuje przed zaproszenie zostanie otwarte.

## <a name="notificationsnotificationsmd"></a>[Powiadomienia](notifications.md)

Dowiedz się, jak zapewnić niestandardowe powiadomienie w aplikacji watch.

## <a name="complicationscomplicationsmd"></a>[Komplikacje](complications.md)

Dodanie obsługi kompilacji, aby wyświetlić aktualne dane na tarczy zegarka.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Sugestie proaktywne](~/ios/watchos/platform/proactive-suggestions.md)

systemu watchOS 3 zezwala aplikacji na aktywne są wyświetlane informacje użytkownika w ramach danego kontekstów. Aby obsługiwać tę funkcję [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) zawiera teraz `MapItem` właściwość, która umożliwia aplikacji, podaj informacje o lokalizacji w celu późniejszego użycia przez inne aplikacje.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Techniki szybkiej interakcji](~/ios/watchos/platform/quick-interaction-techniques.md)

Zapewniając interakcje użytkownika szybkiego są niezbędne do tworzenia atrakcyjnych aplikacji Apple Watch i komplikacji. Nowe do systemu watchOS 3 Apple dodano obsługę aparatów rozpoznawania gestów, dostęp do cyfrowego korony i nowych technik powiadomienie użytkownika i nawigacji. W efekcie oraz dodano obsługę zarówno SceneKit, jak i struktury SpriteKit, umożliwiają deweloperom łatwe tworzenie rozbudowanych, glanceable interfejsy, które są szybkie i dynamiczne.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Ulepszenia aplikacji do ćwiczeń](~/ios/watchos/platform/workout-apps.md)

Nowe do systemu watchOS 3 do ćwiczeń związanych z aplikacje mają możliwość uruchamiania w tle na Apple Watch. Aby włączyć tę funkcję (i uzyskania dostępu do danych HealthKit), aplikacja musi zawierać `WKBackgroundModes` w `Info.plist` z wartością `workout-processing`.
