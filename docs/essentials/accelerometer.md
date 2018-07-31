---
title: 'Xamarin.Essentials: przyspieszeniomierza'
description: Klasy przyspieszeniomierz w Xamarin.Essentials umożliwia monitorowanie czujnika przyspieszeniomierza urządzenia, co oznacza wartość przyspieszenia urządzenia w przestrzeni trójwymiarowej.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b5a24e214eb129b4d53b94586632791c8827447b
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353844"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials: przyspieszeniomierza

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Przyspieszeniomierza** klasy umożliwia monitorowanie czujnika przyspieszeniomierza urządzenia, co oznacza wartość przyspieszenia urządzenia w przestrzeni trójwymiarowej.

## <a name="using-accelerometer"></a>Za pomocą przyspieszeniomierza

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcja przyspieszeniomierza działa przez wywołanie metody `Start` i `Stop` metody służące do nasłuchiwania zmian w celu przyspieszenia. Wszelkie zmiany są wysyłane za pośrednictwem `ReadingChanged` zdarzeń. Poniżej przedstawiono przykładowe zastosowanie:

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(object sender, AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Acceleration.X}, Y: {data.Acceleration.Y}, Z: {data.Acceleration.Z}");
        // Process Acceleration X, Y, and Z
    }

    public void ToggleAcceleromter()
    {
        try
        {
            if (Accelerometer.IsMonitoring)
              Accelerometer.Stop();
            else
              Accelerometer.Start(speed);
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

Odczyty przyspieszeniomierza są raportowane w G. G jest jednostką gravitation wymusić równa wywierany przez pole grawitacyjne ziemi (równa 9,81 m/s ^ 2).

System współrzędnych zdefiniowano względem ekranu telefonie w orientacji domyślne. Nie zostały zamienione osie, gdy zmienia się orientacja ekranu urządzenia.

Oś X jest poziomy i punktów z prawej strony, oś Y pionowy i punktów osi Z punktów na zewnątrz czoła ekranu. W tym systemie współrzędne za ekranu mają wartości ujemnych Z.

Przykłady:

* Gdy urządzenie znajduje się na tabelę i są wypychane na bok po lewej stronie w prawo, wartość przyspieszenia x jest dodatni.

* Gdy urządzenie znajduje się na tabelę, wartość przyspieszenia jest +1.00 G lub (+ równa 9,81 m/s ^ 2), wartość przyspieszenia urządzenia, które odpowiadają (0, m/s ^ 2) minus siła grawitacji (-równa 9,81 m/s ^ 2) i jest to znormalizowane jak G.

* Gdy urządzenie znajduje się na tabelę i jest przesunięte sky Dzięki przyspieszeniu m/s ^ 2, wartość przyspieszenia jest równa 9.81 +, co odpowiada wartość przyspieszenia urządzenia (+ m/s ^ 2) minus siła grawitacji (-równa 9,81 m/s ^ 2) i jest to znormalizowane w G.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>interfejs API

- [Kod źródłowy przyspieszeniomierza](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Dokumentacja interfejsu API przyspieszeniomierza](xref:Xamarin.Essentials.Accelerometer)
