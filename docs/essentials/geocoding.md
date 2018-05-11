---
title: Geokodowanie Xamarin.Essentials
description: Klasa Geokodowanie udostępnia interfejsy API do geocode placemark na współrzędne pozycyjnych i cofnąć geocode coordincates do placemark.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 0bfc463ca22d4137eadc35c02375fe41b0b99b8a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-geocoding"></a>Geokodowanie Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Geokodowanie** klasy udostępnia interfejsy API do geocode placemark na współrzędne pozycyjnych i cofnąć geocode coordincates do placemark.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **Geokodowanie** następujące ustawienia określonych platform jest wymagane.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Nie dodatkowe ustawienia wymagane.

# <a name="iostabios"></a>[iOS](#tab/ios)

Nie dodatkowe ustawienia wymagane.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Do korzystania z funcationality geokodowanie jest wymagany klucz interfejsu API map Bing. Załóż bezpłatne [mapy Bing](https://www.bingmapsportal.com/) konta. W obszarze **Moje konto > klawisze** Utwórz nowy klucz i podaj informacje w zależności od typu aplikacji.

Na wczesnym etapie w cyklu życia aplikacji przed wywołaniem dowolnej **Geokodowanie** metody Ustaw klucz interfejsu API:

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Przy użyciu Geokodowanie

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Pobieranie [lokalizacji](xref:Xamarin.Essentials.Location) współrzędne adres:

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

Pobieranie [placemarks](xref:Xamarin.Essentials.Placemark) dla istniejącego zestawu współrzędnych:

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
    // Handle exception that may have occured in geocoding
}
```

## <a name="api"></a>interfejs API

- [Geokodowanie kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Essentials/Geocoding)
- [Dokumentacja interfejsu API geokodowanie](xref:Xamarin.Essentials.Geocoding)
