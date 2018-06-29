---
title: 'Xamarin.Essentials: przyspieszeniomierza'
description: Klasa przyspieszeniomierza w Xamarin.Essentials umożliwia monitorowanie czujnik przyspieszeniomierza urządzenia, co oznacza przyspieszenie urządzenia w przestrzeni trójwymiarowej.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8229a372659e7918457a9d2f358b871e1a3f5978
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080388"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials: przyspieszeniomierza

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Przyspieszeniomierza** klasa umożliwia monitorowanie czujnik przyspieszeniomierza urządzenia, co oznacza przyspieszenie urządzenia w przestrzeni trójwymiarowej.

## <a name="using-accelerometer"></a>Przy użyciu przyspieszeniomierza

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Funkcja przyspieszeniomierza działa przez wywołanie metody `Start` i `Stop` metody do nasłuchiwania zmian w celu przyspieszenia. Wszelkie zmiany są wysyłane za pośrednictwem `ReadingChanged` zdarzeń. Oto przykładowe zastosowanie:

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(AccelerometerChangedEventArgs e)
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

Odczyty przyspieszeniomierza są raportowane w G. G jest jednostka gravitation wymusić równą wywierany przez pole grawitacyjne ziemi (9.81 m/s ^ 2).

System współrzędnych zdefiniowano względem ekranu Telefon w orientacji domyślne. Nie są zamienione osie, gdy zmienia się orientacja urządzenia.

Poziomy jest na osi X i punktów z prawej strony, osi Y pionowy i punktów osi Z wskazuje na zewnątrz czoła ekranu. W tym systemie współrzędne za ekranu mieć wartości ujemnej.

Przykłady:

* Jeśli urządzenie znajduje się na tabelę i spoczywa na jego lewej do prawej, wartość przyspieszenia x jest dodatnia.

* Jeśli urządzenie znajduje się na tabelę, wartość przyspieszenia jest +1.00 G lub (+ m 9.81/s ^ 2), przyspieszenie urządzenia, które odpowiadają (0, m/s ^ 2) minus siły grawitacji (-9.81 m/s ^ 2) i znormalizowane jak G.

* Jeśli urządzenie znajduje się na tabelę i jest przesunięte niebo z przyspieszenie m/s ^ 2, wartość przyspieszenia jest równa 9.81 +, co odpowiada przyspieszenie urządzenia (+ m/s ^ 2) minus siły grawitacji (-9.81 m/s ^ 2) i znormalizowane w G. 

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Czujnik szybkości](xref:Xamarin.Essentials.SensorSpeed)

- **Najszybszym** — pobieranie danych czujnika tak szybko jak to możliwe (nie ma gwarancji zwrotu z wątku interfejsu użytkownika).
- **Gry** — szybkości odpowiednie do gier (nie ma gwarancji zwrotu z wątku interfejsu użytkownika).
- **Normalny** — szybkość domyślna odpowiedni w przypadku zmiany orientacji ekranu.
- **Interfejs użytkownika** — szybkości nadające się do interfejsu użytkownika ogólne.

Jeśli nie jest gwarantowana obsługi zdarzenia do uruchamiania w wątku interfejsu użytkownika, a jeśli program obsługi zdarzeń musi dostęp do elementów interfejsu użytkownika, użyj [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) metodę, aby uruchomić ten kod w wątku interfejsu użytkownika.

## <a name="api"></a>interfejs API

- [Kod źródłowy przyspieszeniomierza](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Dokumentacja interfejsu API przyspieszeniomierza](xref:Xamarin.Essentials.Accelerometer)
