---
title: 'Xamarin.Essentials: Compass'
description: W tym dokumencie opisano klasy Compass w Xamarin.Essentials, który umożliwia monitorowanie nagłówek Północna magnetycznych urządzenia.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c3fe98c384a87bdc08ce94e7537d1a6343767561
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353886"
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
    SensorSpeed speed = SensorSpeed.UI;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(object sender, CompassChangedEventArgs e)
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
            // Some other exception has occurred
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

## <a name="low-pass-filter"></a>Filtr niska — dostęp próbny

Ze względu na sposób Android compass wartości są aktualizowane i obliczane, może być niezbędna do wygładzania nagłych wartości. A _filtr przekazać niski_ mogą być stosowane, średnie wartości funkcji sinus i cosinus kąta i może zostać włączona przez ustawienie `ApplyLowPassFilter` właściwość `Compass` klasy:

```csharp
Compass.ApplyLowPassFilter = true;
```

Ma wpływ tylko na platformy systemu Android. Więcej informacji, które mogą być odczytywane [tutaj](https://github.com/xamarin/Essentials/pull/354#issuecomment-405316860).

--------------

## <a name="api"></a>interfejs API

- [Compass kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Kompasu dokumentacji interfejsu API](xref:Xamarin.Essentials.Compass)
