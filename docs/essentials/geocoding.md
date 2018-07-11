---
title: 'Xamarin.Essentials: Geokodowania'
description: Klasa Geokodowania w Xamarin.Essentials udostępnia interfejsy API do obu geocode placemark pozycyjne współrzędnych i Cofnij współrzędne geocode na placemark.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 063adba82d96e7fcc64d7ec49a0c0133e1cef8ef
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831451"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: Geokodowania

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Geokodowania** klasa udostępnia interfejsy API do geokodowania placemark pozycyjne współrzędnych i Cofnij geocode coordincates na placemark.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **Geokodowania** następujące ustawienia określone platformy jest wymagane.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Żadna dodatkowa konfiguracja wymagana.

# <a name="iostabios"></a>[iOS](#tab/ios)

Żadna dodatkowa konfiguracja wymagana.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Klucz interfejsu API map Bing wymagane jest wprowadzenie funcationality geokodowania. Załóż bezpłatne [mapy Bing](https://www.bingmapsportal.com/) konta. W obszarze **Moje konto > Moje klucze** Utwórz nowy klucz i Wypełnij informacje w zależności od typu aplikacji (powinien być **publicznych aplikacji Windows (platformy uniwersalnej systemu Windows, 8.x i starszych)** dla aplikacji platformy UWP).

Na wczesnym etapie w życie aplikacji przed wywołaniem dowolnej **Geokodowania** metody Ustaw klucz interfejsu API:

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Za pomocą Geokodowania

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Wprowadzenie [lokalizacji](xref:Xamarin.Essentials.Location) współrzędnych dla adresu:

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occured in geocoding
}
```

Wysokość nie zawsze jest dostępny. Jeśli nie jest dostępna, `Altitude` właściwość może być `null` lub wartość może wynosić zero. Jeśli wysokości jest dostępny, wartość jest w metrach powyżej nad poziomem morza. 

Wprowadzenie [placemarks](xref:Xamarin.Essentials.Placemark) dla istniejącego zestawu współrzędnych:

```csharp
try
{
    var lat = 47.673988;
    var lon = -122.121513;

    var placemarks = await Geocoding.GetPlacemarksAsync(lat, lon);

    var placemark = placemarks?.FirstOrDefault();
    if (placemark != null)
    {
        var geocodeAddress =
            $"AdminArea:       {placemark.AdminArea}\n" +
            $"CountryCode:     {placemark.CountryCode}\n" +
            $"CountryName:     {placemark.CountryName}\n" +
            $"FeatureName:     {placemark.FeatureName}\n" +
            $"Locality:        {placemark.Locality}\n" +
            $"PostalCode:      {placemark.PostalCode}\n" +
            $"SubAdminArea:    {placemark.SubAdminArea}\n" +
            $"SubLocality:     {placemark.SubLocality}\n" +
            $"SubThoroughfare: {placemark.SubThoroughfare}\n" +
            $"Thoroughfare:    {placemark.Thoroughfare}\n";

        Console.WriteLine(geocodeAddress);
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

## <a name="distance-between-two-locations"></a>Odległość między dwiema lokalizacjami

[ `Location` ](xref:Xamarin.Essentials.Location) i [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) klasy definiują metody, aby obliczyć odległość między dwiema lokalizacjami. Zapoznaj się z artykułem [ **Xamarin.Essentials: używanie funkcji Geolokalizacji** ](geolocation.md#calculate-distance) przykład.

## <a name="api"></a>interfejs API

- [Kod źródłowy geokodowania](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Dokumentacja usługi geokodowania interfejsu API](xref:Xamarin.Essentials.Geocoding)
