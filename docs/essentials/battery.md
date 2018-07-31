---
title: 'Xamarin.Essentials: baterii'
description: W tym dokumencie opisano klasy baterii w Xamarin.Essentials, który umożliwia sprawdzenie informacji baterii urządzenia i należy monitorować zmiany.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1deafed85e9400bf7d4592fc06f71c22cc0015f0
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353457"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: baterii

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Baterii** klasy umożliwia sprawdzenie, należy monitorować zmiany i informacji o baterii urządzenia.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **baterii** następujące ustawienia określone platformy jest wymagane.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery` Uprawnienie jest wymagane i musi być skonfigurowany w projekcie dla systemu Android. Mogą to być dodawane w następujący sposób:

Otwórz **AssemblyInfo.cs** plik **właściwości** folderze i Dodaj:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

LUB zaktualizuj Manifest systemu Android:

Otwórz **AndroidManifest.xml** plik **właściwości** folderze i Dodaj następujący kod wewnątrz **manifestu** węzła.

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

Lub kliknij prawym przyciskiem myszy projekt Android i otwórz właściwości projektu. W obszarze **manifestu systemu Android** znaleźć **wymagane uprawnienia:** obszaru i wyboru **baterii** uprawnień. Spowoduje to automatyczne zaktualizowanie **AndroidManifest.xml** pliku.

# <a name="iostabios"></a>[iOS](#tab/ios)

Żadna dodatkowa konfiguracja wymagana.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Żadna dodatkowa konfiguracja wymagana.

-----

## <a name="using-battery"></a>Używa baterii

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Sprawdź bieżące informacje o baterii:

```csharp
var level = Battery.ChargeLevel; // returns 0.0 to 1.0 or -1.0 if unable to determine.

var state = Battery.State;

switch (state)
{
    case BatteryState.Charging:
        // Currently charging
        break;
    case BatteryState.Full:
        // Battery is full
        break;
    case BatteryState.Discharging:
    case BatteryState.NotCharging:
        // Currently discharging battery or not being charged
        break;
    case BatteryState.NotPresent:
        // Battery doesn't exist in device (desktop computer)
    case BatteryState.Unknown:
        // Unable to detect battery state
        break;
}

var source = Battery.PowerSource;

switch (source)
{
    case BatteryPowerSource.Battery:
        // Being powered by the battery
        break;
    case BatteryPowerSource.AC:
        // Being powered by A/C unit
        break;
    case BatteryPowerSource.Usb:
        // Being powered by USB cable
        break;
    case BatteryPowerSource.Wireless:
        // Powered via wireless charging
        break;
    case BatteryPowerSource.Unknown:
        // Unable to detect power source
        break;
}
```

Po każdym wprowadzeniu zmiany przez dowolne właściwości baterii jest wyzwalane zdarzenie:

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryChanged += Battery_BatteryChanged;
    }

    void Battery_BatteryChanged(BatteryChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

## <a name="platform-differences"></a>Różnice dotyczące platform

# <a name="androidtabandroid"></a>[Android](#tab/android)

Nie różnice dotyczące platform.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Urządzenie musi służyć do testowania interfejsów API. 
* Tylko zwróci `AC` lub `Battery` dla `PowerSource`.
* Nie można anulować wibracje.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

* Tylko zwróci `AC` lub `Battery` dla `PowerSource`.

-----

## <a name="api"></a>interfejs API

- [Kod źródłowy dla poziomu energii baterii](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Dokumentacja interfejsu API dla poziomu energii baterii](xref:Xamarin.Essentials.Battery)
