---
title: 'Xamarin.Essentials: OrientationSensor'
description: Klasa OrientationSensor umożliwia monitorowanie orientacji urządzenia w przestrzeni trójwymiarowej.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c01fa28e495eb3eceec62885060dce8f096c4086
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947393"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**OrientationSensor** klasy umożliwia monitorowanie orientacji urządzenia w przestrzeni trójwymiarowej.

> [!NOTE]
> Ta klasa służy do oznaczania orientacji urządzenia w przestrzeni 3D. Jeśli zachodzi potrzeba określenia, jeśli urządzenie użytkownika wideo wyświetlana jest w trybie pionowej lub poziomej, należy użyć `Orientation` właściwość `ScreenMetrics` obiektu dostępnym [ `DeviceDisplay` ](device-display.md) klasy.

## <a name="using-orientationsensor"></a>Za pomocą OrientationSensor

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

`OrientationSensor` Jest włączona, wywołując `Start` metodę, aby monitorować zmiany orientacji urządzenia i wyłączona przez wywołanie metody `Stop` metody. Wszelkie zmiany są wysyłane za pośrednictwem `ReadingChanged` zdarzeń. Poniżej przedstawiono przykładowe zastosowanie:

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

`OrientationSensor` Odczyty są raportowane w formie [ `Quaternion` ](xref:System.Numerics.Quaternion) opisujący orientacji urządzenia, w oparciu dwoma układami współrzędnych 3D:

Urządzenia (zazwyczaj telefonie lub tablecie) ma 3D układ współrzędnych z osiami następujące:

- Dodatnie X wskazuje osi po prawej stronie ekranu w trybie pionowym.
- Dodatnia osi Y wskazuje na początku urządzenia w trybie pionowym.
- Dodatnia osi Z punktów z ekranu.

3D współrzędnych ziemi ma następujące osi:

- Pozytywny osi X stycznej na powierzchni ziemi i wskazuje Wschodnia.
- Dodatnia osi Y jest również stycznej na powierzchni ziemi i północ punktów.
- Prostopadłe do powierzchni ziemi i punkty się jest dodatnią osi Z.

`Quaternion` Opisuje obrót współrzędnych urządzenia względem współrzędnych na ziemi.

A `Quaternion` wartość jest ściśle powiązana z rotacji wokół osi. Jeśli oś obrotu znormalizowany wektor (<sub>x</sub>,<sub>y</sub>,<sub>z</sub>), a kąt obrotu Θ, a następnie (X, Y, Z, W) składniki quaternion są:

(<sub>x</sub>·sin(Θ/2),<sub>y</sub>·sin(Θ/2),<sub>z</sub>·sin(Θ/2), cos(Θ/2))

Są to po prawej stronie współrzędnych, więc przy użyciu przycisku po prawej stronie wskazywany w Dodatni kierunek osi obrotu, krzywej palcami wskazują kierunek dodatnią kąty obrotu.

Przykłady:

* Gdy urządzenie jest na tabelę ekran skierowany, z górną częścią urządzenia (w trybie portret) wskazuje Północne, są wyrównane dwoma układami współrzędnych. `Quaternion` Wartość reprezentuje quaternion tożsamości (0, 0, 0, 1). Wszystkie rotacji mogą być analizowane względem tej pozycji.

* Gdy urządzenie jest na tabelę ekran skierowany w, a górną krawędzią urządzenia (w trybie portret) wskazuje Zachodnia, `Quaternion` wartość (0, 0, 0.707, 0.707). Urządzenie ma zostać obróconą o 90 stopni wokół osi Z ziemi.

* Gdy urządzenia są przechowywane pionowo, aby górnej (w trybie portret) wskazuje na Niebie i ponownie urządzenia jest skierowana Północna, urządzeń, na których obracany o 90 stopni wokół osi X. `Quaternion` Wartość (0.707, 0, 0, 0.707).

* Jeśli urządzenie jest umieszczony w jego lewej krawędzi znajduje się na tabelę i u góry wskazuje Północne, urządzenia został obrócony &ndash;90 stopni wokół osi Y (czyli 90 stopni wokół ujemna osi Y). `Quaternion` Wartość (0,-0.707, 0, 0.707).

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>interfejs API

- [Kod źródłowy OrientationSensor](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [Dokumentacja interfejsu API OrientationSensor](xref:Xamarin.Essentials.OrientationSensor)
