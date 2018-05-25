---
title: Wyświetl informacje o urządzeniu Xamarin.Essentials
description: Klasa DeviceDisplay zawiera informacje o metryki ekranu urządzenia aplikacja jest uruchomiona na.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 05701ff2bc9fceac8a0a490989e52d0327079d46
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-device-display-information"></a>Wyświetl informacje o urządzeniu Xamarin.Essentials

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

**DeviceDisplay** klasy również ujawnia i zdarzenia, które może być subskrybowana wyzwalający zdarzenia przy każdym ekranie wszelkie zmiany metryki:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChanagedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>interfejs API

- [Kod źródłowy DeviceDisplay](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [Dokumentacja interfejsu API DeviceDisplay](xref:Xamarin.Essentials.DeviceDisplay)