---
title: 'Xamarin.Essentials: Compass'
description: W tym dokumencie opisano klasy Compass w Xamarin.Essentials, który umożliwia monitorowanie nagłówek Północna magnetycznych urządzenia.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cf41948c55c742140896bfb48d9bb4abf25c8d68
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947416"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: Compass

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Compass** klasy umożliwia monitorowanie nagłówek Północna magnetycznych urządzenia.

## <a name="using-compass"></a>Za pomocą Compass

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcja Compass działa przez wywołanie metody `Start` i `Stop` metody do nasłuchiwania pod kątem zmian w kompas. Wszelkie zmiany są wysyłane za pośrednictwem `ReadingChanged` zdarzeń. Oto przykład:

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

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>Funkcje specyficzne dla implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Program android nie udostępnia interfejs API pobierania kompas. Będziemy korzystać z przyspieszeniomierz i magnetometrów do obliczania nagłówka magnetycznych północnej, co jest zalecane przez firmę Google. 

W rzadkich przypadkach być może zobaczysz niespójne wyniki ponieważ czujników muszą być dostosowywany, który polega na przenoszeniu urządzenia w ruchu rysunek 8. Najlepszym sposobem zrobić to umożliwia otwieranie map Google, naciśnij kropki (.) dla Twojej lokalizacji i wybierz **compass Kalibruj**.

Należy pamiętać, systemem wiele czujników z aplikacji, w tym samym czasie może dostosować szybkość czujnika.

--------------

## <a name="api"></a>interfejs API

- [Compass kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Kompasu dokumentacji interfejsu API](xref:Xamarin.Essentials.Compass)
