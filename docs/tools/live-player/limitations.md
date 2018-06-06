---
title: Ograniczenia programu Xamarin Player na żywo
description: W tym dokumencie opisano ograniczenia Xamarin Live Player. Go w tym artykule omówiono wymagania dotyczące urządzeń, ich funkcje współpracuje z typów projektów i innych dodatkowych tematów.
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: ea71391382f9e1ecb80cbf5f2d5bf127e0d6d1be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793678"
---
# <a name="limitations-of-xamarin-live-player"></a>Ograniczenia programu Xamarin Player na żywo

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

## <a name="device-requirements"></a>Wymagania dotyczące urządzeń
Aplikacja odtwarzacza Live Xamarin obsługuje następujące urządzenia:

# <a name="androidtabandroid"></a>[Android](#tab/android)

- System android 4.2 lub nowszy.
- ARM-v7a, ARM v8a, ARM64 v8a, x 86 lub x86_64 procesora.

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 lub nowszej.
- Procesor ARM64.

-----

## <a name="limitations"></a>Ograniczenia

Istnieją pewne ograniczenia dotyczące czynności, które można uruchomić Xamarin Live Player, łącznie z poniższych elementów:

### <a name="xamarinforms"></a>Xamarin.Forms

- Niestandardowe moduły renderowania nie są obsługiwane.
- Efekty nie są obsługiwane.
- Niestandardowe formanty mających właściwości niestandardowe nie są obsługiwane.
- Zasobów osadzonych nie są obsługiwane (tj. osadzanie obrazów lub innych zasobów w PCL).
- Struktury MVVM innych firm nie są obsługiwane (tj. Biblioteki PRISM między Mvvm, Mvvm światło, itp.).
- Katalogi zasobów w systemie iOS nie są obsługiwane.

### <a name="other-project-types"></a>Inne typy projektów

- Player na żywo nie jest przeznaczony dla systemu Android native lub projektów dla systemu iOS, (korzystających z systemem Android XML lub Scenorys dla interfejsu użytkownika).

### <a name="misc"></a>Różne

- Ograniczona obsługa odbicia (aktualnie ma wpływ na niektóre popularnych NuGets, takich jak SQLite i Json.NET). Inne NuGets nadal może być obsługiwana.
- Nie można zastąpić niektóre klasy systemu (na przykład użytkownik nie może implementować podklasy).
- Niektóre funkcje platformy, które wymagają obsługi nie może działać w aplikacji platformy Xamarin Player na żywo (jednak została skonfigurowana dla typowe operacje, takie jak dostęp do galerii fotografii).
- Niestandardowe elementy docelowe i kroki kompilacji są ignorowane. Na przykład narzędzi, takich jak Fody, Refit AutoFac i AutoMapper nie może być włączona.
- Projekty języka F # nie są obsługiwane w systemie Android a ograniczony pomocy technicznej w systemie iOS
- Scenariusze Zaawansowane z niestandardowych ogólne klasy i interfejsy może nie być obsługiwana.

Zgłoś wszelkie dodatkowe problemy na [bugzilla](https://aka.ms/live-player-report-issue).

## <a name="related-links"></a>Linki pokrewne

- [Rozwiązywanie problemów](~/tools/live-player/troubleshooting.md)
- [Konfiguracja](~/tools/live-player/install.md)
