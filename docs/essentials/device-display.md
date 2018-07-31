---
title: 'Xamarin.Essentials: Wyświetl informacje o urządzeniu'
description: W tym dokumencie opisano klasy DeviceDisplay w Xamarin.Essentials, która generuje dane pomiarowe ekranu dla urządzenia, na którym działa aplikacja.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cb42da4c8c2d0e381a5b00f7e60da6f427d19c66
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353831"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: Wyświetl informacje o urządzeniu

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**DeviceDisplay** klasa udostępnia informacje o metrykach ekranu urządzenia, aplikacja jest uruchomiona.

## <a name="using-devicedisplay"></a>Za pomocą DeviceDisplay

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Metryki ekranu

Oprócz podstawowych informacji urządzenia **DeviceDisplay** klasy zawiera informacje o orientacji i ekranu urządzenia.

```csharp
// Get Metrics
var metrics = DeviceDisplay.ScreenMetrics;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = metrics.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = metrics.Rotation;

// Width (in pixels)
var width = metrics.Width;

// Height (in pixels)
var height = metrics.Height;

// Screen density
var density = metrics.Density;
```

**DeviceDisplay** klasa udostępnia także zdarzenie, które mogą być subskrybowane wyzwalającego zawsze wtedy, gdy dowolne ekranu zmiany metryki:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChangedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>interfejs API

- [Kod źródłowy DeviceDisplay](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [Dokumentacja interfejsu API DeviceDisplay](xref:Xamarin.Essentials.DeviceDisplay)
