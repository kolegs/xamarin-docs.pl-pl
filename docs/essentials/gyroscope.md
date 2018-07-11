---
title: 'Xamarin.Essentials: Żyroskop'
description: Klasa Żyroskop w Xamarin.Essentials umożliwia monitorowanie czujnika Żyroskop urządzenia, czyli miary obrotu wokół urządzenia trzech osi głównej.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c83b3a9d8a7801e531006f50f8db2e1ad23e48c
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947218"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials: Żyroskop

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Żyroskop** klasy umożliwia monitorowanie czujnika Żyroskop urządzenia, czyli obrotu wokół urządzenia trzech osi głównej.

## <a name="using-gyroscope"></a>Za pomocą Żyroskop

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcja Żyroskop działa przez wywołanie metody `Start` i `Stop` metody do nasłuchiwania pod kątem zmian w Żyroskop. Wszelkie zmiany są wysyłane za pośrednictwem `ReadingChanged` zdarzeń. Poniżej przedstawiono przykładowe zastosowanie:

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(GyroscopeChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Angular Velocity X, Y, and Z
        Console.WriteLine($"Reading: X: {data.AngularVelocity.X}, Y: {data.AngularVelocity.Y}, Z: {data.AngularVelocity.Z}");
    }

    public void ToggleGyroscope()
    {
        try
        {
            if (Gyroscope.IsMonitoring)
              Gyroscope.Stop();
            else
              Gyroscope.Start(speed);
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

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>interfejs API

- [Kod źródłowy Żyroskop](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Dokumentacja interfejsu API Żyroskop](xref:Xamarin.Essentials.Gyroscope)
