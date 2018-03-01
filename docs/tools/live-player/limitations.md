---
title: Ograniczenia
description: "Niektóre ograniczenia Xamarin Live Player"
ms.topic: article
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 08/04/2017
ms.openlocfilehash: d551c0a82fb8f970ce5a00ec9e64ac7f49c81a44
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="limitations"></a>Ograniczenia

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

## <a name="device-requirements"></a>Wymagania dotyczące urządzeń
Aplikacja odtwarzacza Live Xamarin obsługuje następujące urządzenia:

### <a name="ios"></a>iOS
- iOS 9.0 lub nowszej.
- Procesor ARM64.
- Sprawdź [sklepu z aplikacjami](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?mt=8) listę obsługiwanych urządzeń.

### <a name="android"></a>Android
- System android 4.2 lub nowszy.
- ARM-v7a, ARM v8a, ARM64 v8a, x 86 lub x86_64 procesora.


## <a name="limitations"></a>Ograniczenia

Istnieją pewne ograniczenia dotyczące czynności, które można uruchomić Xamarin Live Player, łącznie z poniższych elementów:

### <a name="android"></a>Android
- Interfejsy użytkownika dla systemu android o AXML pliki nie są obecnie obsługiwane.

### <a name="ios"></a>iOS
- Niektóre funkcje scenorysu z systemem iOS nie są obsługiwane.
- iOS XIB pliki nie są obsługiwane.

### <a name="xamarinforms"></a>Xamarin.Forms
- Niestandardowe moduły renderowania nie są obsługiwane.
- Efekty nie są obsługiwane.
- Niestandardowe formanty mających właściwości niestandardowe nie są obsługiwane.
- Zasobów osadzonych nie są obsługiwane (tj. osadzanie obrazów lub innych zasobów w PCL).

### <a name="misc"></a>Różne
- Ograniczona obsługa odbicia (aktualnie ma wpływ na niektóre popularnych NuGets, takich jak SQLite i Json.NET). Inne NuGets nadal są obsługiwane.
- Nie można zastąpić niektóre klasy systemu (na przykład użytkownik nie może implementować podklasy).
- Niektóre funkcje platformy, które wymagają obsługi nie może działać w aplikacji platformy Xamarin Player na żywo (jednak została skonfigurowana dla typowe operacje, takie jak dostęp do galerii fotografii).
- Niestandardowe elementy docelowe i kroki kompilacji są ignorowane. Na przykład narzędzi, takich jak Fody nie może być włączona.

> [!WARNING]
> **Uwaga:** Xamarin Player na żywo nie działa w programie Xamarin Studio

Zgłoś wszelkie dodatkowe problemy na [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Linki pokrewne

- [Rozwiązywanie problemów](~/tools/live-player/troubleshooting.md)
- [Konfiguracja](~/tools/live-player/install.md)
