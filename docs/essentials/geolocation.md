---
title: 'Xamarin.Essentials: używanie funkcji Geolokalizacji'
description: W tym dokumencie opisano klasy używanie funkcji Geolokalizacji w Xamarin.Essentials, która udostępnia interfejsy API można pobrać bieżącego współrzędne używanie funkcji geolokalizacji urządzenia.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 11749107403fc99e1d49b63ee3b50ff105abaa57
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080290"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials: używanie funkcji Geolokalizacji

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Używanie funkcji Geolokalizacji** klasy udostępnia interfejsy API można pobrać bieżącego współrzędne używanie funkcji geolokalizacji urządzenia.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **Geolokalizacja** , następujące ustawienia specyficzne dla platformy jest wymagane:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Duże i małe lokalizacji uprawnienia są wymagane i muszą być skonfigurowane w projekt systemu Android. Ponadto jeśli aplikacja jest przeznaczony dla systemu Android 5.0 (poziom interfejsu API 21) lub nowszego, należy zadeklarować który Twoja aplikacja korzysta funkcje sprzętowe w pliku manifestu. Mogą to być dodawane w następujący sposób:

Otwórz **AssemblyInfo.cs** plików w obszarze **właściwości** folderu i dodać:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

Lub zaktualizować manifestu systemu Android:

Otwórz **AndroidManifest.xml** plików w obszarze **właściwości** folderu i dodaj następującą wewnątrz **manifestu** węzła:

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Lub kliknij prawym przyciskiem myszy projekt Android i otwórz właściwości projektu. W obszarze **manifestu systemu Android** znaleźć **wymagane uprawnienia:** obszarów i wyboru **ACCESS_COARSE_LOCATION** i **ACCESS_FINE_LOCATION**uprawnienia. Ta operacja spowoduje automatyczne zaktualizowanie **AndroidManifest.xml** pliku.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aplikacji **Info.plist** musi zawierać `NSLocationWhenInUseUsageDescription` klucza w celu uzyskania dostępu do lokalizacji urządzenia.

Otwórz Edytor właściwości i dodać **prywatność — lokalizacja podczas użycia użycia opis** właściwości i wartości do wyświetlenia użytkownika wypełniane.

Lub ręcznie edytować plik i dodaj następujące informacje:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Należy ustawić `Location` uprawnień dla aplikacji. Można to zrobić przez otwarcie **Package.appxmanifest** i selecing **możliwości** kartę i sprawdzanie **lokalizacji**.

-----

## <a name="using-geolocation"></a>Przy użyciu używanie funkcji Geolokalizacji

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Interfejs API Geoloation będzie również monit o podanie uprawnień w razie potrzeby.

Możesz uzyskać ostatnią znaną [lokalizacji](xref:Xamarin.Essentials.Location) urządzenia przez wywołanie metody `GetLastKnownLocationAsync` metody. Jest to często czas wykonywania następnie pełne zapytanie, ale może być mniej dokładne.

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

Wysokość nie zawsze jest dostępny. Jeśli nie jest dostępna, `Altitude` właściwość może być `null` lub wartość może być równy zero. Jeśli wysokości jest dostępny, wartość jest metry powyżej powyżej sea poziomu. 

Kwerenda bieżącego urządzenia [lokalizacji](xref:Xamarin.Essentials.Location) współrzędne, `GetLocationAsync` mogą być używane. Najlepiej do przekazania w pełni `GeolocationRequest` i `CancellationToken` ponieważ może upłynąć trochę czasu lokalizacji urządzenia.

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

## <a name="geolocation-accuracy"></a>Dokładność używanie funkcji Geolokalizacji

W poniższej tabeli przedstawiono dokładność dla każdej platformy:

### <a name="lowest"></a>Najniższa

| Platforma | Odległość (w m) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| Platforma UWP | 1000 - 5000 |

### <a name="low"></a>Niski

| Platforma | Odległość (w m) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| Platforma UWP | 300 - 3000 |

### <a name="medium-default"></a>Średni (domyślnie)

| Platforma | Odległość (w m) |
| --- | --- |
| Android | 100 - 500 |
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

[ `Location` ](xref:Xamarin.Essentials.Location) i [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) klasy definiują `CalculateDistance` metody, dzięki którym można obliczyć odległość między dwoma lokalizacji geograficznych. To obliczyć odległość uwzględnia drogi lub innych ścieżek i jest tylko najmniejszą odległość między dwoma punktami wzdłuż powierzchni ziemi, nazywane również _-koła wielkiego_ lub potocznie, odległość "w linii prostej."

Oto przykład:

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

`Location` Konstruktor ma argumenty współrzędne geograficzne w podanej kolejności. Dodatnie wartości szerokości geograficznej znajdują się na północ od równika, a wartości długości geograficznej dodatnią wschód od głównego południka. Użyj ostatni argument `CalculateDistance` do określenia milach lub kilometrach. `Location` Klasy definiuje również `KilometersToMiles` i `MilesToKilometers` metody do konwersji między dwiema jednostkami.

## <a name="api"></a>interfejs API

- [Kod źródłowy używanie funkcji Geolokalizacji](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [Dokumentacja interfejsu API używanie funkcji Geolokalizacji](xref:Xamarin.Essentials.Geolocation)
