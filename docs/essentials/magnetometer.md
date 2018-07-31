---
title: 'Xamarin.Essentials: magnetometrów'
description: Klasa magnetometrów w Xamarin.Essentials umożliwia monitorowanie czujnika magnetometrów urządzenia, co oznacza orientacji urządzenia względem pola magnetycznego na ziemi.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3827b9a57ec2667a8716f5b56bfb4631b979d43a
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353792"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: magnetometrów

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Magnetometrów** klasy umożliwia monitorowanie czujnika magnetometrów urządzenia, co oznacza orientacji urządzenia względem pola magnetycznego na ziemi.

## <a name="using-magnetometer"></a>Za pomocą magnetometrów

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcja magnetometrów działa przez wywołanie metody `Start` i `Stop` metody do nasłuchiwania pod kątem zmian magnetometrów. Wszelkie zmiany są wysyłane za pośrednictwem `ReadingChanged` zdarzeń. Poniżej przedstawiono przykładowe zastosowanie:

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometer_ReadingChanged(object sender, MagnetometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process MagneticField X, Y, and Z
        Console.WriteLine($"Reading: X: {data.MagneticField.X}, Y: {data.MagneticField.Y}, Z: {data.MagneticField.Z}");
    }

    public void ToggleMagnetometer()
    {
        try
        {
            if (Magnetometer.IsMonitoring)
              Magnetometer.Stop();
            else
              Magnetometer.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

Wszystkie dane są zwracane w microteslas.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>interfejs API

- [Kod źródłowy magnetometrów](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [Dokumentacja interfejsu API magnetometrów](xref:Xamarin.Essentials.Magnetometer)
