---
title: Informacje o urządzeniu Xamarin.Essentials
description: Klasa DeviceInfo zawiera informacje o urządzeniu aplikacja jest uruchomiona na.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a398f2697b03cec26f2c786b11e7a3c332cb5f63
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-device-information"></a>Informacje o urządzeniu Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**DeviceInfo** klasa udostępnia informacje o urządzeniu, aplikacja jest uruchomiona.

## <a name="using-deviceinfo"></a>Przy użyciu DeviceInfo

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Następujące informacje są dostępne za pośrednictwem interfejsu API:

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

`DeviceInfo.Platform` są powiązane ze stałym ciągiem, który jest mapowany na system operacyjny. Można sprawdzić wartości z `Platforms` klasy:

- **DeviceInfo.Platforms.iOS** — systemu iOS
- **DeviceInfo.Platforms.Android** — systemu Android
- **DeviceInfo.Platforms.UWP** — platformy uniwersalnej systemu Windows
- **DeviceInfo.Platforms.Unsupported** — nieobsługiwana

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Idioms](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` odpowiada stałym ciągiem, który jest mapowany na typ urządzenia, aplikacji jest uruchomiony. Można sprawdzić wartości z `Idioms` klasy:

- **DeviceInfo.Idioms.Phone** — telefon
- **DeviceInfo.Idioms.Tablet** — Tablet
- **DeviceInfo.Idioms.Desktop** — pulpitu
- **DeviceInfo.Idioms.TV** – telewizji
- **DeviceInfo.Idioms.Unsupported** — nieobsługiwana

## <a name="device-type"></a>Typ urządzenia

`DeviceInfo.DeviceType` skorelowany wyliczeniu, aby określić, czy aplikacja jest uruchamianych na komputerze fizycznym lub urządzenia wirtualnego. Urządzenie wirtualne jest symulator lub emulator.

## <a name="api"></a>interfejs API

- [Kod źródłowy DeviceInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Dokumentacja interfejsu API DeviceInfo](xref:Xamarin.Essentials.DeviceInfo)
