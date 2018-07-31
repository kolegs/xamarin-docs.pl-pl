---
title: 'Xamarin.Essentials: Informacje o urządzeniu'
description: W tym dokumencie opisano klasy DeviceInfo w Xamarin.Essentials, która zapewnia informacje o urządzeniu, że aplikacja jest uruchomiona na.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 18fe081372cc190e5ead2045f36d63652f8702c3
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353805"
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
// Device Model (SMG-950U, iPhone10,6)
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

`DeviceInfo.DeviceType` jest wyliczeniem, aby ustalić, czy aplikacja działa na urządzeniu fizycznym lub wirtualnym. Urządzenie wirtualne jest symulator lub w emulatorze.

## <a name="platform-implementation-specifics"></a>Funkcje specyficzne dla implementacji platformy

# <a name="iostabios"></a>[iOS](#tab/ios)

dla systemu iOS nie uwidacznia interfejs API dla deweloperów uzyskać nazwę określonych urządzeń z systemem iOS. Zamiast tego identyfikatora sprzętu jest zwracany takich jak _iPhone10, 6_ która odnosi się do telefonu iPhone X. Mapowanie identifers te nie są oferowane przez firmę Apple, ale można znaleźć na [iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) (— oficjalne źródło).

--------------

## <a name="api"></a>interfejs API

- [Kod źródłowy DeviceInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Dokumentacja interfejsu API DeviceInfo](xref:Xamarin.Essentials.DeviceInfo)
