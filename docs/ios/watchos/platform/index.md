---
title: funkcje platformy watchOS
description: Ten dokument prowadzi do różnych prowadnice, opisujących watchOS platformy funkcje, takie jak Apple Pay, powiadomienia, komplikacji, aktywne sugestii, ćwiczeń aplikacje i inne.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 16d10dd69223f404aac7c933302992a1544461e9
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066617"
---
# <a name="watchos-platform-features"></a>funkcje platformy watchOS

Ten dokument prowadzi do różnych prowadnice, opisujących watchOS platformy funkcje, takie jak Apple Pay, powiadomienia, komplikacji, aktywne sugestii, ćwiczeń aplikacje i inne.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Wprowadzenie do systemu watchOS 4](introduction-to-watchos4.md)

Ten dokument zawiera omówienie funkcji dodane lub zaktualizowane w watchOS 4.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Wprowadzenie do systemu watchOS 3](introduction-to-watchos3/index.md)

W tym artykule opisano nowe i zaktualizowane interfejsów API w watchOS 3.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay ulepszenia](~/ios/watchos/platform/apple-pay.md)

W watchOS 3 Aby umożliwić obsługę bezpiecznego, w aplikacji płatności (towarów fizycznych i usługi) dla aplikacji uruchomionych na Apple Watch została rozszerzona PassKit framework.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Zadania w tle](~/ios/watchos/platform/background-tasks.md)

watchOS 3 wprowadzono kilka zadań w tle, które aplikacji można użyć do zaktualizowania jej informacje o sprawdzeniu, czy ma ona zawartość musi przez użytkownika, zanim zostaną one otwarte.

## <a name="notificationsnotificationsmd"></a>[Powiadomienia](notifications.md)

Dowiedz się, jak zapewnić niestandardowe powiadomienie Obsługa w aplikacji czujki.

## <a name="complicationscomplicationsmd"></a>[Komplikacje](complications.md)

Dodawanie obsługi complication mają być wyświetlane aktualne dane na powierzchni czujki.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Sugestie aktywne](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 zezwala aplikacji na aktywne są wyświetlane informacje użytkownika w ramach danego kontekstach. Do obsługi tej funkcji [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) zawiera teraz `MapItem` właściwość, która umożliwia aplikacji, podaj informacje o lokalizacji do późniejszego użytku przez inne aplikacje.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Techniki szybkie interakcji](~/ios/watchos/platform/quick-interaction-techniques.md)

Udostępnienie interakcji użytkowników szybkiego są niezbędne do tworzenia atrakcyjnych aplikacji Apple Watch oraz komplikacje. Nowy do watchOS 3 Apple ma dodano obsługę aparatów rozpoznawania gestów, dostęp do cyfrowego wierzchołek i nowe techniki powiadomienie użytkownika i Nawigacja. To, oraz dodano obsługę zarówno SceneKit i SpriteKit, umożliwiają deweloperom łatwe tworzenie rozbudowanych, krótkie interfejsów, które są szybkie i elastyczny.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Ulepszenia aplikacji ćwiczeń](~/ios/watchos/platform/workout-apps.md)

Nowe watchOS 3 ćwiczeń związane z, aplikacje mają możliwość uruchamiania w tle na Apple Watch. Aby włączyć tę funkcję (i uzyskać dostęp do danych HealthKit), aplikacja musi zawierać `WKBackgroundModes` klucza w `Info.plist` z wartością `workout-processing`.
