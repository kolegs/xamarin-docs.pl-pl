---
title: 'Xamarin.Essentials: Wyświetl informacje o urządzeniu'
description: W tym dokumencie opisano klasy DeviceDisplay w Xamarin.Essentials, który zawiera metryki ekranu urządzenia, na którym działa aplikacja.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 830b96bcc21397047cb5aaacb5c568bc2ee863c4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782375"
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
