---
title: Ograniczenia aplikacji Xamarin Live Player
description: W tym dokumencie opisano ograniczenia aplikacji Xamarin Live Player. Go w tym artykule omówiono wymagania dotyczące urządzeń, ich funkcje współpracuje z typów projektów i innych dodatkowych tematów.
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: aff6990df1b710190f11c2d7fa09c8399e94f8af
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "43780638"
---
# <a name="limitations-of-xamarin-live-player"></a>Ograniczenia aplikacji Xamarin Live Player

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

## <a name="device-requirements"></a>Wymagania dotyczące urządzeń

Aplikacja Xamarin Live Player obsługuje następujące urządzenia z systemem Android:

- System android 4.2 lub nowszy.
- ARM-v7a, ARM v8a, ARM64-v8a x 86, x86_64 procesor lub.

## <a name="limitations"></a>Ograniczenia

Istnieją pewne ograniczenia na rzeczy, które można uruchomić aplikacji Xamarin Live Player, łącznie z poniższych elementów:

### <a name="ios"></a>iOS

Live Player nie jest dostępna dla systemu iOS.

### <a name="xamarinforms"></a>Xamarin.Forms

- Niestandardowe programy renderujące nie są obsługiwane.
- Efekty nie są obsługiwane.
- Kontrolki niestandardowe przy użyciu możliwej do wiązania właściwości niestandardowe nie są obsługiwane.
- Zasoby osadzone, nie są obsługiwane (tj. osadzenie obrazów i innych zasobów w aplikacji PCL).
- Struktury MVVM innych firm nie są obsługiwane (tj. Biblioteki PRISM między Mvvm, Mvvm Light, itp.).

### <a name="other-project-types"></a>Inne typy projektów

- Live Player nie jest przeznaczony dla natywnych projektów systemu Android (korzystające z systemem Android XML dla interfejsu użytkownika).

### <a name="misc"></a>Różne

- Ograniczona obsługa odbicie (obecnie ma wpływ na niektóre popularne rozszerzeń Nuget, takich jak bazy danych SQLite i Json.NET). Innych rozszerzeń Nuget może w dalszym ciągu będzie obsługiwana.
- Nie można zastąpić niektórych klas systemowych (na przykład, nie może implementować podklasę).
- Niektóre funkcje platformy, które wymagają aprowizacji nie może działać w aplikacji Xamarin Live Player (jednak została skonfigurowana dla typowych operacji, takich jak dostęp do galerii fotografii).
- Niestandardowe elementy docelowe i kroki kompilacji są ignorowane. Na przykład narzędzia, takie jak Fody, Refit, AutoFac i AutoMapper nie może zostać włączone.
- Projekty języka F # nie są obsługiwane.
- Zaawansowane scenariusze obejmujące niestandardowe ogólne klasy i interfejsy mogą nie być obsługiwane.

Użyj **Zgłoś Problem** w [programu Visual Studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017) lub [programu Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/report-a-problem) do zgłaszania problemów przy użyciu aplikacji Xamarin Live Player.

## <a name="related-links"></a>Linki pokrewne

- [Rozwiązywanie problemów](~/tools/live-player/troubleshooting.md)
- [Konfiguracja](~/tools/live-player/install.md)
