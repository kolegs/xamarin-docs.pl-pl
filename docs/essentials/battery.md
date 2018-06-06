---
title: 'Xamarin.Essentials: baterii'
description: W tym dokumencie opisano klasy baterii w Xamarin.Essentials, który umożliwia sprawdzenie informacji baterii urządzenia i monitorować zmiany.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 35764b4c2270359a7c010e1186f882e236e17fd5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782089"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: baterii

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Baterii** klasa umożliwia sprawdzenie, należy monitorować zmiany i informacji o baterii urządzenia.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **baterii** następujące ustawienia określonych platform jest wymagane.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery` Uprawnienie jest wymagane i muszą być skonfigurowane w projekt systemu Android. Mogą to być dodawane w następujący sposób:

Otwórz **AssemblyInfo.cs** plików w obszarze **właściwości** folderu i dodać:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Battery)]
```

LUB zaktualizować manifestu systemu Android:

Otwórz **AndroidManifest.xml** plików w obszarze **właściwości** folderu i dodaj następującą wewnątrz **manifestu** węzła.

```xml
<uses-permission android:name="android.permission.BATTERY" />
```

Kliknij prawym przyciskiem myszy projekt Anroid i otwórz właściwości projektu. W obszarze **manifestu systemu Android** znaleźć **wymagane uprawnienia:** obszarów i wyboru **baterii** uprawnienia. Ta operacja spowoduje automatyczne zaktualizowanie **AndroidManifest.xml** pliku.

# <a name="iostabios"></a>[iOS](#tab/ios)

Nie dodatkowe ustawienia wymagane.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Nie dodatkowe ustawienia wymagane.

-----

## <a name="using-battery"></a>Przy użyciu baterii

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Sprawdzanie informacji o bieżącym poziomie naładowania baterii:

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
    case BatteryPowerSource.Ac:
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

Zdarzenie jest wyzwalane, właściwości baterii zmianie:

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

## <a name="platform-differences"></a>Różnice dotyczące platformy

| Platforma | Różnica |
| --- | --- |
| iOS | Urządzenie musi być stosowane do testowania interfejsów API. |
| iOS | Tylko zwróci ak lub baterii PowerSource. |
| Platforma UWP | Tylko zwróci ak lub baterii PowerSource. |

## <a name="api"></a>interfejs API

- [Kod źródłowy baterii](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Dokumentacja interfejsu API baterii](xref:Xamarin.Essentials.Battery)
