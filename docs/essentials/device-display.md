---
title: 'Xamarin.Essentials: Wyświetl informacje o urządzeniu'
description: W tym dokumencie opisano klasy DeviceDisplay w Xamarin.Essentials, który zawiera metryki ekranu urządzenia, na którym działa aplikacja.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3060d56e14fb0d3801a96ec0fe6e24c9efda4dac
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080316"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: Wyświetl informacje o urządzeniu

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**DeviceDisplay** klasy informacje na temat metryki ekranu urządzenia aplikacja jest uruchomiona.

## <a name="using-devicedisplay"></a>Przy użyciu DeviceDisplay

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Metryki ekranu

Oprócz informacji podstawowych urządzenia **DeviceDisplay** klasy zawiera informacje o ekranu i orientacji urządzenia.

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

**DeviceDisplay** klasy udostępnia również zdarzenie, które mogą mieć subskrypcję, która jest zawsze wyzwalane, gdy dowolne ekranu metryki zmiany:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
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
