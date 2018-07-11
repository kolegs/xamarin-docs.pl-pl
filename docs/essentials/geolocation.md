---
title: 'Xamarin.Essentials: używanie funkcji Geolokalizacji'
description: W tym dokumencie opisano klasy Geolokalizacja w Xamarin.Essentials, która udostępnia interfejsy API, aby pobrać bieżące współrzędne geolokalizacja urządzenia.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 11749107403fc99e1d49b63ee3b50ff105abaa57
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848754"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials: używanie funkcji Geolokalizacji

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Geolokalizacja** klasa udostępnia interfejsy API, aby pobrać bieżące współrzędne geolokalizacja urządzenia.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **Geolokalizacja** , następujące konfiguracje specyficzne dla platformy jest wymagane:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Duże i małe lokalizacji uprawnienia są wymagane i muszą być skonfigurowane w projekt systemu Android. Ponadto jeśli aplikacja jest przeznaczona dla systemu Android 5.0 (poziom interfejsu API 21) lub nowszego, musisz zadeklarować, aplikacja używa funkcje sprzętowe w pliku manifestu. Mogą to być dodawane w następujący sposób:

Otwórz **AssemblyInfo.cs** plik **właściwości** folderze i Dodaj:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

Lub zaktualizuj manifest systemu Android:

Otwórz **AndroidManifest.xml** plik **właściwości** folderze i Dodaj następujący kod wewnątrz **manifestu** węzła:

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Lub kliknij prawym przyciskiem myszy na projekt systemu Android i otwórz właściwości projektu. W obszarze **manifestu systemu Android** znaleźć **wymagane uprawnienia:** obszaru i wyboru **ACCESS_COARSE_LOCATION** i **ACCESS_FINE_LOCATION**uprawnienia. Spowoduje to automatyczne zaktualizowanie **AndroidManifest.xml** pliku.

# <a name="iostabios"></a>[iOS](#tab/ios)

Twoja aplikacja **Info.plist** musi zawierać `NSLocationWhenInUseUsageDescription` klucza w celu uzyskania dostępu do lokalizacji urządzenia.

Otwórz Edytor plist i Dodaj **ochrona prywatności — gdy w użyciu opis użycia lokalizacji** właściwość i wprowadź wartość, aby wyświetlić użytkownika.

Lub ręcznie edytować plik i Dodaj następujący kod:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Należy ustawić `Location` uprawnień dla aplikacji. Można to zrobić, otwierając **Package.appxmanifest** i selecing **możliwości** kartę i sprawdzanie **lokalizacji**.

-----

## <a name="using-geolocation"></a>Za pomocą używanie funkcji Geolokalizacji

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Interfejs API Geoloation również będzie monitował użytkownika uprawnień, gdy jest to konieczne.

Możesz uzyskać ostatnią znaną [lokalizacji](xref:Xamarin.Essentials.Location) urządzenia przez wywołanie metody `GetLastKnownLocationAsync` metody. To jest często szybsza, następnie wykonując pełne zapytanie, ale może być mniej dokładne.

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

Wysokość nie zawsze jest dostępny. Jeśli nie jest dostępna, `Altitude` właściwość może być `null` lub wartość może wynosić zero. Jeśli wysokości jest dostępny, wartość jest w metrach powyżej nad poziomem morza. 

Aby wysłać zapytanie bieżące urządzenie [lokalizacji](xref:Xamarin.Essentials.Location) współrzędne, `GetLocationAsync` mogą być używane. Zaleca się przekazać pełną `GeolocationRequest` i `CancellationToken` ponieważ może potrwać pewien czas, aby uzyskać lokalizacji urządzenia.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

## <a name="geolocation-accuracy"></a>Używanie funkcji Geolokalizacji dokładności

W poniższej tabeli przedstawiono dokładność dla danej platformy:

### <a name="lowest"></a>Najniższy

| Platforma | Odległość (w m) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| Platforma UWP | 1000 - 5000 |

### <a name="low"></a>Niska

| Platforma | Odległość (w m) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| Platforma UWP | 300 – 3000 |

### <a name="medium-default"></a>Średni (domyślnie)

| Platforma | Odległość (w m) |
| --- | --- |
| Android | 100 – 500 |
| iOS | 100 |
| Platforma UWP | 30-500 |

### <a name="high"></a>Wysoka

| Platforma | Odległość (w m) |
| --- | --- |
| Android | 0 - 100 |
| iOS | 10 |
| Platforma UWP | < = 10 |

### <a name="best"></a>Najlepiej

| Platforma | Odległość (w m) |
| --- | --- |
| Android | 0 - 100 |
| iOS | ~0 |
| Platforma UWP | < = 10 |

<a name="calculate-distance" />

## <a name="distance-between-two-locations"></a>Odległość między dwiema lokalizacjami

[ `Location` ](xref:Xamarin.Essentials.Location) i [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) klasy definiują `CalculateDistance` metody, które umożliwiają obliczyć odległość między dwoma lokalizacjach geograficznych. To obliczyć odległość nie uwzględnia drogach lub innych ścieżek i jest również nazywany jedynie najkrótszą odległość między dwoma punktami wzdłuż powierzchni ziemi, _najkrótszej odległości_ lub potocznie, odległości "w linii prostej."

Oto przykład:

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

`Location` Konstruktor ma szerokość i długość geograficzną argumentów w tej kolejności. Dodatnie wartości szerokości geograficznej są na północ od równika i wartości długości geograficznej dodatnią wschód od południków Północnej. Użyj ostatni argument `CalculateDistance` do określenia mil lub kilometrów. `Location` Klasa definiuje również `KilometersToMiles` i `MilesToKilometers` metody konwersji między dwiema jednostkami.

## <a name="api"></a>interfejs API

- [Używanie funkcji Geolokalizacji kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [Dokumentacja interfejsu API Geolokalizacji](xref:Xamarin.Essentials.Geolocation)
