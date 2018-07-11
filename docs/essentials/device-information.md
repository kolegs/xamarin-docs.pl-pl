---
title: 'Xamarin.Essentials: Informacje o urządzeniu'
description: W tym dokumencie opisano klasy DeviceInfo w Xamarin.Essentials, która zapewnia informacje o urządzeniu, że aplikacja jest uruchomiona na.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b7246afca19607ef2f70288d4643696f4ac35d52
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831490"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: Informacje o urządzeniu

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**DeviceInfo** klasa udostępnia informacje o urządzeniu, w której działa aplikacja.

## <a name="using-deviceinfo"></a>Za pomocą DeviceInfo

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Poniższe informacje są udostępniane za pośrednictwem interfejsu API:

```csharp
// Device Model (SMG-950U)
var device = DeviceInfo.Model;

// Manufacturer (Samsung)
var manufacturer = DeviceInfo.Manufacturer;

// Device Name (Motz's iPhone)
var deviceName = DeviceInfo.Name;

// Operating System Version Number (7.0)
var version = DeviceInfo.VersionString;

// Platform (Android)
var platform = DeviceInfo.Platform;

// Idiom (Phone)
var idiom = DeviceInfo.Idiom;

// Device Type (Physical)
var deviceType = DeviceInfo.DeviceType;
```

## <a name="platformsxrefxamarinessentialsdeviceinfoplatforms"></a>[Platformy](xref:Xamarin.Essentials.DeviceInfo.Platforms)

`DeviceInfo.Platform` jest skorelowane ze stałym ciągiem, który jest mapowany do systemu operacyjnego. Wartości można sprawdzić za pomocą `Platforms` klasy:

- **DeviceInfo.Platforms.iOS** — iOS
- **DeviceInfo.Platforms.Android** — Android
- **DeviceInfo.Platforms.UWP** — platformy uniwersalnej systemu Windows
- **DeviceInfo.Platforms.Unsupported** — nieobsługiwana

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Idiomy](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` odpowiada stałej ciąg, który mapuje do odpowiedniego typu urządzenia aplikacji jest uruchomiona na. Wartości można sprawdzić za pomocą `Idioms` klasy:

- **DeviceInfo.Idioms.Phone** — telefon
- **DeviceInfo.Idioms.Tablet** — Tablet
- **DeviceInfo.Idioms.Desktop** — pulpitu
- **DeviceInfo.Idioms.TV** — TV
- **DeviceInfo.Idioms.Unsupported** — nieobsługiwana

## <a name="device-type"></a>Typ urządzenia

`DeviceInfo.DeviceType` koreluje wyliczeniu, aby określić, czy aplikacja jest uruchamianych na komputerze fizycznym lub wirtualny urządzenia. Urządzenie wirtualne jest symulator lub w emulatorze.

## <a name="api"></a>interfejs API

- [Kod źródłowy DeviceInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Dokumentacja interfejsu API DeviceInfo](xref:Xamarin.Essentials.DeviceInfo)
