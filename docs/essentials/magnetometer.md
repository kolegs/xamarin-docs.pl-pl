---
title: 'Xamarin.Essentials: magnetometrów'
description: Klasa magnetometrów w Xamarin.Essentials umożliwia monitorowanie czujnik magnetometrów urządzenia, co oznacza orientacji urządzenia względem pola magnetycznego ziemi.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2c02188b5282949559e0abc5fa1b61b6b451fc8e
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080389"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: magnetometrów

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

Jeśli nie jest gwarantowana obsługi zdarzenia do uruchamiania w wątku interfejsu użytkownika, a jeśli program obsługi zdarzeń musi dostęp do elementów interfejsu użytkownika, użyj [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) metodę, aby uruchomić ten kod w wątku interfejsu użytkownika.

## <a name="api"></a>interfejs API

- [Kod źródłowy magnetometrów](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [Dokumentacja interfejsu API magnetometrów](xref:Xamarin.Essentials.Magnetometer)
