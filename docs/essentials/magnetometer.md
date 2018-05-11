---
title: Magnetometrów Xamarin.Essentials
description: Klasa magnetometrów umożliwia monitorowanie czujnik magnetometrów urządzenia, co oznacza orientacji urządzenia względem pola magnetycznego ziemi.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: bb9bd656c809b05c49a27f7b3dab2a64ff7b7e94
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-magnetometer"></a>Magnetometrów Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Magnetometrów** klasa umożliwia monitorowanie czujnik magnetometrów urządzenia, co oznacza orientacji urządzenia względem pola magnetycznego ziemi.

## <a name="using-magnetometer"></a>Przy użyciu magnetometrów

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Funkcja magnetometrów działa przez wywołanie metody `Start` i `Stop` metody do nasłuchiwania zmian magnetometrów. Wszelkie zmiany są wysyłane za pośrednictwem `ReadingChanged` zdarzeń. Oto przykładowe zastosowanie:

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometerr_ReadingChanged(MagnetometerChangedEventArgs e)
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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Czujnik szybkości](xref:Xamarin.Essentials.SensorSpeed)

- **Najszybszym** — pobieranie danych czujnika tak szybko jak to możliwe (nie ma gwarancji zwrotu z wątku interfejsu użytkownika).
- **Gry** — szybkości odpowiednie do gier (nie ma gwarancji zwrotu z wątku interfejsu użytkownika).
- **Normalny** — szybkość domyślna odpowiedni w przypadku zmiany orientacji ekranu.
- **Interfejs użytkownika** — szybkości nadające się do interfejsu użytkownika ogólne.

## <a name="api"></a>interfejs API

- [Kod źródłowy magnetometrów](https://github.com/xamarin/Essentials/tree/master/Essentials/Magnetometer)
- [Dokumentacja interfejsu API magnetometrów](xref:Xamarin.Essentials.Magnetometer)
