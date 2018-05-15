---
title: Kompas Xamarin.Essentials
description: Klasa kompas umożliwia monitorowanie nagłówek Północna magnetycznych urządzenia.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 39f141424ddd247458b9c8b35ae02ab29e2c2206
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-compass"></a>Kompas Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Kompas** klasa umożliwia monitorowanie nagłówek Północna magnetycznych urządzenia.

## <a name="using-compass"></a>Przy użyciu kompas

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Funkcja kompas działa przez wywołanie metody `Start` i `Stop` metody do nasłuchiwania zmian w kompas. Wszelkie zmiany są wysyłane za pośrednictwem `ReadingChanged` zdarzeń. Oto przykład:

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(CompassChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: {data.HeadingMagneticNorth} degrees");
        // Process Heading Magnetic North
    }

    public void ToggleCompass()
    {
        try
        {
            if (Compass.IsMonitoring)
              Compass.Stop();
            else
              Compass.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Some other exception has occured
        }
    }
}
```

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Czujnik szybkości](xref:Xamarin.Essentials.SensorSpeed)

- **Najszybszym** — pobieranie danych czujnika tak szybko jak to możliwe (nie ma gwarancji zwrotu z wątku interfejsu użytkownika).
- **Gry** — szybkości odpowiednie do gier (nie ma gwarancji zwrotu z wątku interfejsu użytkownika).
- **Normalny** — szybkość domyślna odpowiedni w przypadku zmiany orientacji ekranu.
- **Interfejs użytkownika** — szybkości nadające się do interfejsu użytkownika ogólne.

## <a name="platform-implementation-specifics"></a>Szczegóły implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android nie zapewnia interfejs API pobierania wzorem nagłówka. Możemy korzystać z przyspieszeniomierza i magnetometrów do obliczenia nagłówek magnetycznych Północna, co jest zalecane przez firmę Google. 

W rzadkich przypadkach może być widzisz niespójne wyniki ponieważ czujników muszą być kalibrować, który polega na przenoszeniu urządzenia w ruchu rysunek 8. Najlepszym sposobem zrobić to otwórz map programu Google, naciśnij na kropki (.) dla lokalizacji, a następnie wybierz **kompas Kalibruj**.

Należy pamiętać, systemem wielu czujników z aplikacji, w tym samym czasie może dopasowywać prędkość czujnika.

--------------

## <a name="api"></a>interfejs API

- [Kompas kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Wzorem dokumentacji interfejsu API](xref:Xamarin.Essentials.Compass)
