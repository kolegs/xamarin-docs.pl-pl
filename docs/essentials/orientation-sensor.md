---
title: 'Xamarin.Essentials: OrientationSensor'
description: Klasa OrientationSensor umożliwia monitorowanie orientacji urządzenia w przestrzeni trójwymiarowej.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c7bbc849e5fa0b901f5b54e77d548b28bc2a72c6
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080391"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**OrientationSensor** klasa umożliwia monitorowanie orientacji urządzenia w przestrzeni trójwymiarowej.

> [!NOTE]
> Ta klasa służy do oznaczania orientacji urządzenia w miejscu 3D. Jeśli musisz określić, czy urządzenie użytkownika wideo wyświetlana jest w trybie pionowa lub pozioma, użyj `Orientation` właściwość `ScreenMetrics` dostępnym dla obiekt [ `DeviceDisplay` ](device-display.md) klasy.

## <a name="using-orientationsensor"></a>Przy użyciu OrientationSensor

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

`OrientationSensor` Jest włączona, wywołując `Start` monitorować zmiany orientacji urządzenia i wyłączone przez wywołanie metody `Stop` metody. Wszelkie zmiany są wysyłane za pośrednictwem `ReadingChanged` zdarzeń. Oto przykładowe zastosowanie:

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Orientation.X}, Y: {data.Orientation.Y}, Z: {data.Orientation.Z}, W: {data.Orientation.W}");
        // Process Orientation quaternion (X, Y, Z, and W)
    }

    public void ToggleOrientationSensor()
    {
        try
        {
            if (OrientationSensor.IsMonitoring)
                OrientationSensor.Stop();
            else
                OrientationSensor.Start(speed);
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

`OrientationSensor` Odczyty są przekazywane w postaci [ `Quaternion` ](xref:System.Numerics.Quaternion) opisujący orientacji urządzenia oparte na dwóch systemów współrzędnych 3D:

Urządzenia (zazwyczaj telefon lub tablet) ma 3D układ współrzędnych z następujących osi:

- Dodatnie X punktów osi z prawej strony ekranu w trybie portret.
- Wskazuje dodatnią osi Y górnej części urządzenia w trybie portret.
- Dodatnia osi Z punktów poza ekranu.

3D współrzędnych ziemi ma następujące osie:

- Dodatnich osi X stycznej do powierzchni ziemi i wschód punktów.
- Dodatnia osi Y jest również stycznej na powierzchni ziemi i północ punktów.
- Prostopadłe do powierzchni ziemi i punkty górę jest dodatnią osi Z.

`Quaternion` Opisuje obrót współrzędnych urządzenia względem ziemi współrzędnych.

A `Quaternion` wartość jest bardzo ściśle powiązana z obrót wokół osi. Jeśli oś obrotu jest znormalizowany wektor (<sub>x</sub>,<sub>y</sub>,<sub>z</sub>), a kąt obrotu jest Θ, a następnie (X, Y, Z, W) składników quaternion:

(<sub>x</sub>·sin(Θ/2),<sub>y</sub>·sin(Θ/2),<sub>z</sub>·sin(Θ/2), cos(Θ/2))

System współrzędnych po prawej stronie, są tak stronie przycisku suwaka prawej wskazywana w kierunku dodatnią oś obrotu krzywej palcami wskazują kierunek dodatnią kąty obrotu.

Przykłady:

* Jeśli urządzenie znajduje się na tabelę z jego ekranu skierowany, do górnej krawędzi urządzenia (w trybie portret), wskazując północ, dwóch systemów współrzędnych są wyrównane. `Quaternion` Wartość reprezentuje quaternion tożsamości (0, 0, 0, 1). Wszystkie obrotów można analizować względem tej pozycji.

* Jeśli urządzenie znajduje się na tabelę z jego ekranu skierowany w, a górną krawędzią urządzenia (w trybie portret) Zachodnia, wskazując `Quaternion` wartość (0, 0, 0.707, 0.707). Urządzenia został obrócony 90 stopni wokół osi Z ziemi.

* Gdy urządzenie jest utrzymywana pionowo górnej (w trybie portret) wskazuje niebo, a wstecz urządzenia północ skierowany, urządzenie zostało 90 stopni obrócona wokół osi X. `Quaternion` Wartość to (0.707, 0, 0, 0.707).

* Jeśli urządzenie znajduje się więc jego lewej krawędzi znajduje się na tabelę i górnej wskazuje północ, urządzenia został obrócony &ndash;90 stopni wokół osi Y (lub 90 stopni wokół osi Y ujemna). `Quaternion` Wartość (0,-0.707, 0, 0.707).

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Czujnik szybkości](xref:Xamarin.Essentials.SensorSpeed)

- **Najszybszym** — pobieranie danych czujnika tak szybko jak to możliwe (nie ma gwarancji zwrotu z wątku interfejsu użytkownika).
- **Gry** — szybkości odpowiednie do gier (nie ma gwarancji zwrotu z wątku interfejsu użytkownika).
- **Normalny** — szybkość domyślna odpowiedni w przypadku zmiany orientacji ekranu.
- **Interfejs użytkownika** — szybkości nadające się do interfejsu użytkownika ogólne.

Jeśli nie jest gwarantowana obsługi zdarzenia do uruchamiania w wątku interfejsu użytkownika, a jeśli program obsługi zdarzeń musi dostęp do elementów interfejsu użytkownika, użyj [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) metodę, aby uruchomić ten kod w wątku interfejsu użytkownika.

## <a name="api"></a>interfejs API

- [Kod źródłowy OrientationSensor](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [Dokumentacja interfejsu API OrientationSensor](xref:Xamarin.Essentials.OrientationSensor)
