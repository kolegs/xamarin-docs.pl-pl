---
title: Xamarin.Essentials mapy
description: Klasa map w Xamarin.Essentials umożliwia aplikacji w celu otwarcia aplikacji zainstalowanych map do określonej lokalizacji lub placemark.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 445e2da84e9a9aaf1ce4d836af11cfba963b8cbb
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353916"
---
# <a name="xamarinessentials-maps"></a>Xamarin.Essentials: mapy

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Mapuje** klasa umożliwia aplikacji w celu otwarcia aplikacji zainstalowanych map do określonej lokalizacji lub placemark.

## <a name="using-maps"></a>Przy użyciu map

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcja mapy działa przez wywołanie metody `OpenAsync` metody z `Location` lub `Placemark` można otworzyć z opcjonalnymi `MapsLaunchOptions`.

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(location, options);
    }
}
```

Podczas otwierania z `Placemark` wymagane są następujące informacje:

* `CountryName`
* `AdminArea`
* `Thoroughfare`
* `Locality`

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var placemark = new Placemark
            {
                CountryName = "United States",
                AdminArea = "WA",
                Thoroughfare = "Microsoft Building 25",
                Locality = "Redmond"
            };
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(placemark, options);
    }
}
```

## <a name="extension-methods"></a>Metody rozszerzeń

Jeśli masz już odwołanie do `Location` lub `Placemark` metodą rozszerzenia wbudowanych `OpenMapsAsync` z opcjonalnymi `MapsLaunchOptions`:

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## <a name="platform-differences"></a>Różnice dotyczące platform

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `MapDirectionsMode` nie jest obsługiwane i nie ma wpływu.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `MapDirectionsMode` jest obsługiwane można ustawić domyślny tryb kierunku, po otwarciu aplikacji mapy.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

* `MapDirectionsMode` nie jest obsługiwane i nie ma wpływu.

--------------

## <a name="platform-implementation-specifics"></a>Funkcje specyficzne dla implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

System android używa `geo:` schemat identyfikatora Uri, aby uruchomić aplikację map na urządzeniu. To może monitować użytkownika o wybierz istniejącą aplikację, która obsługuje ten schemat identyfikatora Uri.  Xamarin.Essentials jest testowana przy użyciu map Google, który obsługuje ten schemat.

# <a name="iostabios"></a>[iOS](#tab/ios)

Brak szczegółów konkretnej implementacji platformy.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Brak szczegółów konkretnej implementacji platformy.

--------------

## <a name="api"></a>interfejs API

- [Mapy kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Dokumentacja interfejsu API usługi Maps](xref:Xamarin.Essentials.Maps)
