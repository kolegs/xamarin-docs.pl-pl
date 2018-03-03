---
title: Funkcje platformy
description: "Apple Watch specyficzne funkcje do uwzględnienia w aplikacjach watchOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 16e779761aa164ea9890547e3baca527a3592021
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="platform-features"></a>Funkcje platformy

_Apple Watch specyficzne funkcje do uwzględnienia w aplikacjach watchOS._

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Ulepszenia płatności firmy Apple](~/ios/watchos/platform/apple-pay.md)

W watchOS 3 Aby umożliwić obsługę bezpiecznego, w aplikacji płatności (towarów fizycznych i usługi) dla aplikacji uruchomionych na Apple Watch została rozszerzona PassKit framework.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Zadania w tle](~/ios/watchos/platform/background-tasks.md)

watchOS 3 wprowadzono kilka zadań w tle, które aplikacji można użyć do zaktualizowania jej informacje o sprawdzeniu, czy ma ona zawartość musi przez użytkownika, zanim zostaną one otwarte.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Wprowadzenie do systemu watchOS 4](introduction-to-watchos4.md)

Nowe funkcje w watchOS 4.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Wprowadzenie do systemu watchOS 3](introduction-to-watchos3/index.md)

W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsami API dostępnymi w watchOS 3 dla deweloperów programu Xamarin.

##  <a name="notificationsnotificationsmd"></a>[Powiadomienia](notifications.md)

Dowiedz się, jak zapewnić niestandardowe powiadomienie Obsługa w aplikacji czujki.

##  <a name="complicationscomplicationsmd"></a>[Komplikacje](complications.md)

Dodawanie obsługi complication mają być wyświetlane aktualne dane na powierzchni czujki.


## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Sugestie proaktywne](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 zezwala aplikacji na aktywne są wyświetlane informacje użytkownika w ramach danego kontekstach. Do obsługi tej funkcji [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) zawiera teraz `MapItem` właściwość, która umożliwia aplikacji, podaj informacje o lokalizacji do późniejszego użytku przez inne aplikacje.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Techniki szybkiej interakcji](~/ios/watchos/platform/quick-interaction-techniques.md)

Udostępnienie interakcji użytkowników szybkiego są niezbędne do tworzenia atrakcyjnych aplikacji Apple Watch oraz komplikacje. Nowy do watchOS 3 Apple ma dodano obsługę aparatów rozpoznawania gestów, dostęp do cyfrowego wierzchołek i nowe techniki powiadomienie użytkownika i Nawigacja. To, oraz dodano obsługę zarówno SceneKit i SpriteKit, umożliwiają deweloperom łatwe tworzenie rozbudowanych, krótkie interfejsów, które są szybkie i elastyczny.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Ulepszenia aplikacji do ćwiczeń](~/ios/watchos/platform/workout-apps.md)

Nowe watchOS 3 ćwiczeń związane z, aplikacje mają możliwość uruchamiania w tle na Apple Watch. Aby włączyć tę funkcję (i uzyskać dostęp do danych HealthKit), aplikacja musi zawierać `WKBackgroundModes` klucza w `Info.plist` z wartością `workout-processing`.
