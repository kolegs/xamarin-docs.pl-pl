---
title: Żyroskop Xamarin.Essentials
description: Klasa Żyroskop umożliwia monitorowanie czujnik Żyroskop urządzenia, czyli obrót wokół trzy osie głównej urządzenia.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a987978882a928ad50578d3a0031bce07e60fb6e
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-gyroscope"></a>Żyroskop Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Żyroskop** klasa umożliwia monitorowanie czujnik Żyroskop urządzenia, czyli obrót wokół trzy osie głównej urządzenia.

## <a name="using-gyroscope"></a>Przy użyciu Żyroskop

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Funkcja Żyroskop działa przez wywołanie metody `Start` i `Stop` metody do nasłuchiwania zmian w Żyroskop. Wszelkie zmiany są wysyłane za pośrednictwem `ReadingChanged` zdarzeń. Oto przykładowe zastosowanie:

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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Czujnik szybkości](xref:Xamarin.Essentials.SensorSpeed)

- **Najszybszym** — pobieranie danych czujnika tak szybko jak to możliwe (nie ma gwarancji zwrotu z wątku interfejsu użytkownika).
- **Gry** — szybkości odpowiednie do gier (nie ma gwarancji zwrotu z wątku interfejsu użytkownika).
- **Normalny** — szybkość domyślna odpowiedni w przypadku zmiany orientacji ekranu.
- **Interfejs użytkownika** — szybkości nadające się do interfejsu użytkownika ogólne.

## <a name="api"></a>interfejs API

- [Żyroskop kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Dokumentacja interfejsu API Żyroskop](xref:Xamarin.Essentials.Gyroscope)
