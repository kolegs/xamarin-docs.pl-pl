---
title: Ograniczenia aplikacji Xamarin Live Player
description: W tym dokumencie opisano ograniczenia aplikacji Xamarin Live Player. Go w tym artykule omówiono wymagania dotyczące urządzeń, ich funkcje współpracuje z typów projektów i innych dodatkowych tematów.
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: ea71391382f9e1ecb80cbf5f2d5bf127e0d6d1be
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815285"
---
# <a name="limitations-of-xamarin-live-player"></a>Ograniczenia aplikacji Xamarin Live Player

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

## <a name="device-requirements"></a>Wymagania dotyczące urządzeń
Aplikacja Xamarin Live Player obsługuje następujące urządzenia:

# <a name="androidtabandroid"></a>[Android](#tab/android)

- System android 4.2 lub nowszy.
- ARM-v7a, ARM v8a, ARM64-v8a x 86, x86_64 procesor lub.

# <a name="iostabios"></a>[iOS](#tab/ios)

- System iOS 9.0 lub nowszym.
- Procesorów ARM64.

-----

## <a name="limitations"></a>Ograniczenia

Istnieją pewne ograniczenia na rzeczy, które można uruchomić aplikacji Xamarin Live Player, łącznie z poniższych elementów:

### <a name="xamarinforms"></a>Xamarin.Forms

- Niestandardowe programy renderujące nie są obsługiwane.
- Efekty nie są obsługiwane.
- Kontrolki niestandardowe przy użyciu możliwej do wiązania właściwości niestandardowe nie są obsługiwane.
- Zasoby osadzone, nie są obsługiwane (tj. osadzenie obrazów i innych zasobów w aplikacji PCL).
- Struktury MVVM innych firm nie są obsługiwane (tj. Biblioteki PRISM między Mvvm, Mvvm Light, itp.).
- Wykazy zasobów w systemie iOS nie są obsługiwane.

### <a name="other-project-types"></a>Inne typy projektów

- Live Player nie jest przeznaczona dla systemu Android native lub projektów systemu iOS (korzystających XML dla systemu Android lub Scenorysy dla interfejsu użytkownika).

### <a name="misc"></a>Różne

- Ograniczona obsługa odbicie (obecnie ma wpływ na niektóre popularne rozszerzeń Nuget, takich jak bazy danych SQLite i Json.NET). Innych rozszerzeń Nuget może w dalszym ciągu będzie obsługiwana.
- Nie można zastąpić niektórych klas systemowych (na przykład, nie może implementować podklasę).
- Niektóre funkcje platformy, które wymagają aprowizacji nie może działać w aplikacji Xamarin Live Player (jednak została skonfigurowana dla typowych operacji, takich jak dostęp do galerii fotografii).
- Niestandardowe elementy docelowe i kroki kompilacji są ignorowane. Na przykład narzędzia, takie jak Fody, Refit, AutoFac i AutoMapper nie może zostać włączone.
- Projekty języka F # nie są obsługiwane w systemie Android i ograniczoną obsługę w systemie iOS
- Zaawansowane scenariusze obejmujące niestandardowe ogólne klasy i interfejsy mogą nie być obsługiwane.

Zgłoś wszelkie dodatkowe problemy na [bugzilla](https://aka.ms/live-player-report-issue).

## <a name="related-links"></a>Linki pokrewne

- [Rozwiązywanie problemów](~/tools/live-player/troubleshooting.md)
- [Konfiguracja](~/tools/live-player/install.md)
