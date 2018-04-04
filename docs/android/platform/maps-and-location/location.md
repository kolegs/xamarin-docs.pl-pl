---
title: Usługi lokalizacji
description: Ten przewodnik zawiera wprowadzenie Rozpoznawanie lokalizacji w aplikacjach systemu Android oraz pokazano, jak pobrać lokalizacji użytkownika przy użyciu interfejsu API systemu Android usługi lokalizacji, a także dostępnego dostawcę kolei lokalizacji przy użyciu interfejsu API Google lokalizacji usług.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 366c75db49a7e0f4f559b13c0871071dee2f08e3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="location-services"></a>Usługi lokalizacji

_Ten przewodnik zawiera wprowadzenie Rozpoznawanie lokalizacji w aplikacjach systemu Android oraz pokazano, jak pobrać lokalizacji użytkownika przy użyciu interfejsu API systemu Android usługi lokalizacji, a także dostępnego dostawcę kolei lokalizacji przy użyciu interfejsu API Google lokalizacji usług._

## <a name="location-services-overview"></a>Omówienie usług lokalizacji

Android zapewnia dostęp do różnych technologii lokalizacji, takie jak lokalizacja wieża komórki, sieci Wi-Fi i GPS. Szczegóły dotyczące poszczególnych technologii lokalizacji są pobieranej przez *dostawców lokalizacji*, dzięki czemu aplikacje, aby uzyskać lokalizacji w taki sam sposób, niezależnie od tego, Dostawca używany. W tym przewodniku przedstawiono części usług Google Play, która inteligentnie określa najlepszy sposób, aby uzyskać lokalizacji urządzenia na podstawie dostawców, które są dostępne i sposobu używania urządzenia dostawcy kolei lokalizacji. Interfejs API systemu android usługi lokalizacji oraz sposób komunikowania się z lokalizacji w systemie usługi przy użyciu `LocationManager`. Druga część przewodnika Eksploruje przy użyciu interfejsu API systemu Android usługi lokalizacji `LocationManager`.
 
Jako ogólne zasadą aplikacje powinny wolą używać dostawcy kolei lokalizacji, nastąpi powrót starsze lokalizacji usługi interfejsu API systemu Android tylko wtedy, gdy jest to konieczne.

## <a name="location-fundamentals"></a>Podstawowe informacje dotyczące lokalizacji

W systemie Android niezależnie od tego, jakie interfejsu API, możesz wybrać do pracy z danymi lokalizacji, kilka pojęć pozostają takie same. W tej sekcji przedstawiono lokalizację dostawców i uprawnienia dotyczące lokalizacji.

### <a name="location-providers"></a>Lokalizacja dostawcy

Kilka technologii jest używana wewnętrznie w celu określenia lokalizacji użytkownika. Typ zależy od sprzętu używanego *lokalizacji dostawcy* wybrane zadania zbierania danych. Android używa trzech dostawców lokalizacji:

-   **Dostawca GPS** &ndash; GPS zapewnia najbardziej dokładna lokalizacja używa najwięcej zasilania i działa najlepiej na zewnątrz. Ten dostawca jest kombinacją GPS i asystowaną GPS ([aGPS](http://en.wikipedia.org/wiki/Assisted_GPS)), zwraca GPS danych zbieranych przez wieże komórkową.

-   **Dostawcy sieci** &ndash; zawiera kombinację sieci Wi-Fi i sieci komórkowej danych, w tym aGPS danych zbieranych przez wieże komórki. Zużywają mniej energii niż dostawcy GPS, ale zwraca dane dokładność różnych lokalizacji.

-   **Dostawca pasywnym** &ndash; opcji "nakładane", można wygenerować danych lokalizacji w aplikacji przy użyciu dostawców żądane przez inne aplikacje lub usługi. Jest to bardziej zawodne ale idealne opcję oszczędzania energii dla aplikacji, które nie wymagają aktualizacji stałej lokalizacji do pracy.

Lokalizacja dostawcy nie są zawsze dostępne. Na przykład chcemy używać GPS dla aplikacji, ale GPS może być wyłączona w ustawieniach lub urządzenie może nie mieć GPS na wszystkich. Jeśli nie ma określonego dostawcy, wybierając tego dostawcy mogą zwracać `null`.

### <a name="location-permissions"></a>Uprawnienia lokalizacji

Lokalizacji aplikacji wymagany jest dostęp urządzenia sprzętu czujników odbierać GPS, sieci Wi-Fi i danych komórkowych. Dostęp jest kontrolowany przez odpowiednie uprawnienia w aplikacji manifestu systemu Android.
Istnieją dwa uprawnienia &ndash; w zależności od wymagań aplikacji i wybór interfejsu API, można zezwolić jednemu:

-   `ACCESS_FINE_LOCATION` &ndash; Umożliwia aplikacji dostęp do GPS.
    Wymagany dla *dostawcy GPS* i *pasywnym dostawcy* opcje (*pasywnym dostawcy wymaga uprawnienia dostępu GPS danych zbieranych przez inną aplikację lub usługę*). Opcjonalne uprawnienie *dostawcy sieci*.

-   `ACCESS_COARSE_LOCATION` &ndash; Umożliwia aplikacji dostęp do lokalizacji w sieci komórkowej i sieci Wi-Fi. Wymagany dla *dostawcy sieci* Jeśli `ACCESS_FINE_LOCATION` nie jest ustawiona.

W przypadku aplikacji, które odnoszą się do interfejsu API w wersji 21 (Android 5.0 interfejs typu lizak) lub nowszym, można włączyć `ACCESS_FINE_LOCATION` i nadal jest uruchamiane na urządzeniach, które nie mają GPS sprzętu. Jeśli aplikacja wymaga sprzętu GPS, należy jawnie dodać `android.hardware.location.gps` `uses-feature` elementu do manifestu systemu Android. Aby uzyskać więcej informacji, zobacz Android [używa funkcji](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) odwołanie do elementu.

Aby ustawić uprawnienia, rozwiń węzeł **właściwości** folderu w **konsoli rozwiązania** i kliknij dwukrotnie **AndroidManifest.xml**. Uprawnienia, które będą wyświetlane w **wymagane uprawnienia**:

[![Zrzut ekranu przedstawiający ustawień systemu Android Manifest wymagane uprawnienia](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

Ustawienie dowolnej z tych uprawnień informuje Android czy aplikacja wymaga zgody użytkownika w celu uzyskania dostępu do lokalizacji dostawcy. Urządzenia uruchom interfejs API na poziomie 22 (Android 5.1) lub niższy będzie monitować użytkownika można udzielić uprawnienia zawsze, gdy aplikacja jest zainstalowana. Na urządzeniach z systemem interfejsu API na poziomie 23 (Android 6.0) lub wyższym, aplikacji należy sprawdzić uprawnienia wykonawcze przed wysłaniem żądania lokalizacji dostawcy. 

> [!NOTE]
>Uwaga: Ustawienie `ACCESS_FINE_LOCATION` wymaga dostępu do danych zarówno grubych i cienkich lokalizacji. Nigdy nie trzeba ustawić obie uprawnień, tylko *minimalnego* uprawnień aplikacji wymaga do pracy.

Ta Wstawka kodu jest przykładem sprawdzić, czy aplikacja ma uprawnienie do `ACCESS_FINE_LOCATION` uprawnień:

```csharp
 if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.AccessFineLocation) == Permission.Granted)
{
    StartRequestingLocationUpdates();
    isRequestingLocationUpdates = true;
}
else
{
    // The app does not have permission ACCESS_FINE_LOCATION 
}
```

Aplikacji musi być odporny na błędy scenariusza, gdy użytkownik nie będzie udzielać uprawnienia (lub odwołał uprawnienia) i mogą bezpiecznie rozwiązania tej sytuacji. Zobacz [przewodnik uprawnienia](~/android/app-fundamentals/permissions.md) więcej szczegółów na implementacji czasu wykonywania uprawnienia kontroli w platformy Xamarin.Android.


## <a name="using-the-fused-location-provider"></a>Korzystanie z kolei lokalizacji dostawcy

Dostawca kolei lokalizacji jest preferowany sposób dla aplikacji systemu Android w celu otrzymywania aktualizacji lokalizacji z urządzenia, ponieważ wydajnie wybierze dostawcy lokalizacji w czasie wykonywania, aby zapewnić najlepszą informacji o lokalizacji w sposób wydajny baterii. Na przykład użytkownik przejście wokół na zewnątrz pobiera optymalne miejsce odczytu z GPS. Jeśli użytkownik przeprowadza następnie pomieszczeniu, gdzie GPS działa nieprawidłowo (Jeśli w ogóle), dostawca kolei lokalizacji automatycznie może przełączyć się do sieci Wi-Fi, który działa lepiej w pomieszczeniu.
 
Dostawca kolei lokalizacji Interfejs API udostępnia różnych innych narzędzi dla aplikacji lokalizacji, w tym geofencing i monitorowanie działania. W tej sekcji zamierzamy skoncentrować się na podstawowe informacje dotyczące konfigurowania `LocationClient`ustanawiania dostawców i uzyskiwanie lokalizacji użytkownika.

Dostawca kolei lokalizacji jest częścią [usług Google Play](http://developer.android.com/google/play-services/index.html). Pakiet usług Google Play musi być zainstalowana i poprawnie skonfigurowane w aplikacji interfejsu API dostawcy kolei lokalizacji do pracy, a urządzenie musi mieć plik Google Play Services APK zainstalowane.

Przed Xamarin.Android aplikacji można użyć dostawcy kolei lokalizacji go należy dodać **Xamarin.GooglePlayServices.Maps** do projektu.

### <a name="checking-if-google-play-services-is-installed"></a>Sprawdzanie, czy jest zainstalowany usług Google Play

Operacja zostanie wykonana zostanie Xamarin.Android, awarii, jeśli klient próbuje użyć dostawcy kolei lokalizacji usług Google Play nie jest zainstalowany (lub nieaktualny), a następnie wyjątek czasu wykonywania.  Jeśli nie zainstalowano usług Google Play, aplikacja powinna może wrócić do systemu Android usługi lokalizacji opisanych wyżej. Jeśli usług Google Play jest nieaktualny, aplikację można wyświetlić wiadomości do użytkownika, prosząc ich zaktualizować zainstalowana wersja usług Google Play.

Ta Wstawka kodu przedstawiono przykładowy sposób działanie systemu Android można programowo Sprawdź, czy usług Google Play jest zainstalowany:

```csharp
bool IsGooglePlayServicesInstalled()
{
    var queryResult = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
    if (queryResult == ConnectionResult.Success)
    {
        Log.Info("MainActivity", "Google Play Services is installed on this device.");
        return true;
    }

    if (GoogleApiAvailability.Instance.IsUserResolvableError(queryResult))
    {
        // Check if there is a way the user can resolve the issue
        var errorString = GoogleApiAvailability.Instance.GetErrorString(queryResult);
        Log.Error("MainActivity", "There is a problem with Google Play Services on this device: {0} - {1}",
                  queryResult, errorString);
                  
        // Alternately, display the error to the user.
    }

    return false;
}
```

### <a name="fusedlocationproviderclient"></a>FusedLocationProviderClient

Wchodzić w interakcje z kolei lokalizacji dostawcy, aplikacji platformy Xamarin.Android musi mieć wystąpienia `FusedLocationProviderClient`. Ta klasa udostępnia metody niezbędne do subskrybowania aktualizacji lokalizacji i pobrać ostatniej znanej lokalizacji urządzenia.

`OnCreate` Metoda działania jest odpowiednim miejscu, aby pobrać odwołanie do `FusedLocationProviderClient`, jak pokazano w poniższy fragment kodu:

```csharp
public class MainActivity: AppCompatActivity
{
    FusedLocationProviderClient fusedLocationProviderClient;

    protected override void OnCreate(Bundle bundle) 
    {
        fusedLocationProviderClient = LocationServices.GetFusedLocationProviderClient(this);
    }
}
```

### <a name="getting-the-last-known-location"></a>Pobieranie ostatniej znanej lokalizacji

`FusedLocationProviderClient.GetLastLocationAsync()` — Metoda umożliwia proste, bez blokowania aplikacji platformy Xamarin.Android szybko uzyskać ostatniej znanej lokalizacji urządzenia z minimalnym narzut kodowania.

Następujący fragment kodu przedstawia sposób użycia `GetLastLocationAsync` metodę, aby pobrać lokalizację urządzenia:

```csharp
async Task GetLastLocationFromDevice()
{
    // This method assumes that the necessary run-time permission checks have succeeded.
    getLastLocationButton.SetText(Resource.String.getting_last_location);
    Android.Locations.Location location = await fusedLocationProviderClient.GetLastLocationAsync();

    if (location == null)
    {
        // Seldom happens, but should code that handles this scenario
    }
    else
    {
        // Do something with the location 
        Log.Debug("Sample", "The latitude is " + location.Latitude);
    }
}
```

### <a name="subscribing-to-location-updates"></a>Subskrybowanie lokalizacji aktualizacji

Aplikacji platformy Xamarin.Android może również subskrybować aktualizacje lokalizacji z dostawcy kolei lokalizacji przy użyciu `FusedLocationProviderClient.RequestLocationUpdatesAsync` metody, jak pokazano w poniższym przykładzie:

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```
Ta metoda przyjmuje dwa parametry:

-   **`Android.Gms.Location.LocationRequest`** &ndash; A `LocationRequest` obiekt jest sposób aplikacji platformy Xamarin.Android przekazuje parametry działania kolei lokalizacji dostawcy. `LocationRequest` Przechowuje informacje takie jak należy częste żądania lub jak ważna aktualizacja dokładne lokalizacji powinna być. Na przykład spowoduje, że żądania lokalizacji ważne urządzenia może korzystać GPS i w związku z tym więcej mocy, określając lokalizację. Następujący fragment kodu przedstawia sposób tworzenia `LocationRequest` dla lokalizacji z Wysoka dokładność, sprawdzanie mniej więcej co pięć minut dla aktualizacji lokalizacji (ale nie wcześniej niż dwie minuty między żądaniami). Dostawca lokalizacji kolei używa `LocationRequest` jako wskazówek, dla których dostawca lokalizacji do użycia podczas próby można określić lokalizacji urządzenia:

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```
                                          

-   **`Android.Gms.Location.LocationCallback`** &ndash; Aby otrzymywać aktualizacje z lokalizacji, do aplikacji platformy Xamarin.Android musi podklasy `LocationProvider` klasy abstrakcyjnej. Ta klasa widoczne dwie metody, które może być wywołany przez dostawcę kolei lokalizacji do zaktualizowania aplikacji informacji o lokalizacji. To będzie można szczegółowo poniżej.

Aby powiadomić aplikację platformy Xamarin.Android aktualizacji lokalizacji, wywoła dostawcy kolei lokalizacji `LocationCallBack.OnLocationResult(LocationResult result)`. `Android.Gms.Location.LocationResult` Parametr będzie zawierać informacje o lokalizacji aktualizacji.

Jeśli dostawca kolei lokalizacji wykryje zmianę dostępności danych lokalizacji, zostanie wywołany `LocationProvider.OnLocationAvaibility(LocationAvailability
locationAvailability)` metody. Jeśli `LocationAvailability.IsLocationAvailable` zwraca `true`, a następnie można założyć, że wyniki lokalizacji urządzenia zgłoszone przez `OnLocationResult` precyzyjne i jak aktualny zgodnie z wymaganiami `LocationRequest`. Jeśli `IsLocationAvailable` wynosi false, nie zwróciło żadnych wyników lokalizacji będzie zwracany przez `OnLocationResult`.

Następujący fragment kodu jest przykładowa implementacja `LocationCallback` obiektu:

```csharp
public class FusedLocationProviderCallback : LocationCallback
{
    readonly MainActivity activity;

    public FusedLocationProviderCallback(MainActivity activity)
    {
        this.activity = activity;
    }

    public override void OnLocationAvailability(LocationAvailability locationAvailability)
    {
        Log.Debug("FusedLocationProviderSample", "IsLocationAvailable: {0}",locationAvailability.IsLocationAvailable);
    }

    public override void OnLocationResult(LocationResult result)
    {
        if (result.Locations.Any())
        {
            var location = result.Locations.First();
            Log.Debug("Sample", "The latitude is :" + location.Latitude);
        }
        else
        {
            // No locations to work with.
        }
    }
}
```

## <a name="using-the-android-location-service-api"></a>Przy użyciu interfejsu API usługi lokalizacji dla systemu Android

Usługi lokalizacji dla systemu Android to interfejs API starszych dla przy użyciu informacji o lokalizacji w systemie Android. Lokalizacja dane są zbierane przez czujniki sprzętu i zbierane przez usługę system, który jest dostępny w aplikacji `LocationManager` klasy i `ILocationListener`.

Usługi lokalizacji jest najbardziej odpowiednie dla aplikacji, które muszą być uruchamiane na urządzeniach, które nie mają usług Google Play zainstalowane.

Usługi lokalizacji jest specjalnym rodzajem [usługi](http://developer.android.com/guide/components/services.html) zarządzane przez System. Usługa systemowa współdziała z urządzeń sprzętowych i jest zawsze uruchomiona. Naciśnięcie przycisku do lokalizacji aktualizacji w naszej aplikacji, firma Microsoft będzie subskrybować lokalizacji aktualizacje z usługi lokalizacji systemu przy użyciu `LocationManager` i `RequestLocationUpdates` wywołania.

Uzyskanie lokalizacji użytkownika przy użyciu usługi lokalizacji dla systemu Android obejmuje kilka kroków:

1.  Pobierz odwołanie do `LocationManager` usługi.
2.  Implementowanie `ILocationListener` interfejsu i uchwyt zdarzenia, gdy zmienia lokalizację.
3.  Użyj `LocationManager` żądanie aktualizacji lokalizacji dla określonego dostawcy. `ILocationListener` z poprzedniego kroku, będzie używany do odbierania wywołań zwrotnych z `LocationManager`.
4.  Zatrzymaj lokalizacji aktualizacje, gdy aplikacja nie jest już odpowiedniego do otrzymywania aktualizacji.

### <a name="location-manager"></a>Menedżer lokalizacji

Firma Microsoft może uzyskać dostępu do usługi lokalizacji systemu przy użyciu wystąpienia `LocationManager` klasy. `LocationManager` to specjalne klasa, która pozwoli na interakcję z lokalizacji w systemie usługi i wywoływać metod w nim. Aplikację można odwołać się do `LocationManager` przez wywołanie metody `GetSystemService` i przekazując typ usługi, jak pokazano poniżej:

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` jest to dobre miejsce, aby pobrać odwołanie do `LocationManager`.
Jest dobrym rozwiązaniem, aby zachować `LocationManager` jako zmienną z klasy, tak aby nazywamy je w różnych punktach w cyklu życia działania.

### <a name="request-location-updates-from-the-locationmanager"></a>Żądanie aktualizacji lokalizacji z LocationManager

Gdy aplikacja odwołuje się do `LocationManager`, należy go sprawdzić `LocationManager` jakiego rodzaju informacje o lokalizacji, które są wymagane, i jak często informacji ma zostać zaktualizowany. W tym celu wywoływania `RequestionLocationUpdates` na `LocationManager` obiekt i przekazywanie pewne kryteria aktualizacji i wywołanie zwrotne, które otrzymają aktualizacje lokalizacji. To wywołanie zwrotne jest typem, który musi zaimplementować `ILocationListener` interfejsu (opisany bardziej szczegółowo w dalszej części tego przewodnika).

`RequestionLocationUpdates` Metody informuje system lokalizacji usługi aplikacji czy chcesz rozpocząć odbieranie aktualizacji lokalizacji. Ta metoda służy do określenia dostawcy, jak progi czasu i odległość kontrolować częstotliwość aktualizacji. Na przykład aktualizacje metody poniżej poniżej lokalizacji żądań w od dostawcy lokalizacji GPS co milisekund 2000 i tylko wtedy, gdy lokalizacja zmieni więcej niż 1 metr:

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

Aplikacja powinna tylko tyle razy, co jest wymagane dla aplikacji do wykonywania również żądań aktualizacji lokalizacji. Zachowuje czas pracy baterii i tworzy lepsze środowisko pracy użytkownika.

### <a name="responding-to-updates-from-the-locationmanager"></a>Odpowiada na żądania aktualizacji z LocationManager

Gdy aplikacja zażądała aktualizacje z `LocationManager`, implementując może odbierać informacje z usługi [ `ILocationListener` ](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/) interfejsu. Ten interfejs zawiera cztery metody do lokalizacji usługi i dostawcy lokalizacji nasłuchiwania `OnLocationChanged`. System będzie wywoływać `OnLocationChanged` podczas zmiany wystarczająca liczba na kwalifikować się jako zmiana lokalizacji zgodnie z kryteriami ustawionymi podczas żądania aktualizacji lokalizacji lokalizacji użytkownika. 

Poniższy kod przedstawia metod w `ILocationListener` interfejsu:

```csharp
public class MainActivity : AppCompatActivity, ILocationListener
{
    TextView latitude;
    TextView longitude;
    
    public void OnLocationChanged (Location location)
    {
        // called when the location has been updated.
    }
    
    public OnProviderDisabled(string locationProvider)
    {
        // called when the user disables the provider
    }
    
    public OnProviderEnabled(string locationProvider)
    {
        // called when the user enables the provider
    }
    
    public OnStatusChanged(string locationProvider, Availability status, Bundle extras)
    {
        // called when the status of the provider changes (there are a variety of reasons for this)
    }
}
```

### <a name="unsubscribing-to-locationmanager-updates"></a>Anulowanie LocationManager aktualizacji

W celu zaoszczędzenia zasobów systemowych, aplikacji należy jak najszybciej anulować subskrypcję aktualizacje lokalizacji. `RemoveUpdates` Informuje metody `LocationManager` zatrzymania wysyłania aktualizacji do naszej aplikacji.  Na przykład działanie może wywołać `RemoveUpdates` w `OnPause` — metoda tak, że jesteśmy w stanie oszczędzania energii, jeśli aplikacja nie wymaga aktualizacji lokalizacji podczas jego działanie nie jest na ekranie:

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

Jeśli aplikacja wymaga pobrać aktualizacje lokalizacji znajduje się w tle, należy utworzyć niestandardowe usługi, która ją subskrybuje system usługi lokalizacji. Zapoznaj się [Backgrounding z usługami Android](~/android/app-fundamentals/services/index.md) przewodnika, aby uzyskać więcej informacji.

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>Określanie najlepsze dostawca lokalizacji LocationManager

Powyższe aplikacji ustawia GPS jako lokalizacji dostawcy. Jednak GPS nie można dostępne we wszystkich przypadkach, takich jak Jeśli urządzenie jest w pomieszczeniu lub nie ma odbiornika GPS. Jeśli tak jest, wynikiem jest `null` zwracany przez dostawcę.

Aby pobrać aplikację do pracy, gdy GPS nie jest dostępna, należy użyć `GetBestProvider` metodę, aby uzyskać najlepsze dostawcy dostępnej lokalizacji (obsługiwane urządzenia i włączenie użytkownika) podczas uruchamiania aplikacji. Przekazywanie określonego dostawcy, a nie można ustalić `GetBestProvider` wymagania dotyczące dostawcy — takiego jak dokładność i power - o [ `Criteria` obiektu](https://developer.xamarin.com/api/type/Android.Locations.Criteria/). `GetBestProvider` Zwraca dostawcę najlepsze dla danych kryteriów.

Poniższy kod przedstawia sposób uzyskać najlepiej dostawcy i użyć, jeśli żądanie aktualizacji lokalizacji:

```csharp
Criteria locationCriteria = new Criteria();   
locationCriteria.Accuracy = Accuracy.Coarse;
locationCriteria.PowerRequirement = Power.Medium;

locationProvider = locationManager.GetBestProvider(locationCriteria, true);

if(locationProvider != null)
{
    locationManager.RequestLocationUpdates (locationProvider, 2000, 1, this);
}
else
{
    Log.Info(tag, "No location providers available");
}
```

> [!NOTE]
>  Jeśli użytkownik wyłączył wszystkich dostawców lokalizacji, `GetBestProvider` zwróci `null`. Aby zobaczyć, jak ten kod działa na urządzeniu prawdziwe, należy włączyć GPS, sieci Wi-Fi i sieci komórkowej w obszarze **Google Ustawienia > lokalizacji > Tryb** opisane w tym zrzut ekranu:

[![Ustawienia trybu lokalizacji ekranu na telefonie z systemem Android](location-images/location-02.png)](location-images/location-02.png#lightbox)

Na poniższym zrzucie ekranu przedstawiono lokalizację aplikacji uruchomionej przy użyciu `GetBestProvider`:

[![Wyświetlanie szerokości, długości i dostawcy aplikacji GetBestProvider](location-images/location-03.png)](location-images/location-03.png#lightbox)

Należy pamiętać, że `GetBestProvider` nie ulega zmianie dostawcy dynamicznie. Zamiast określa najlepiej dostawcy raz podczas cyklu działania. Jeśli stan dostawcy zmieni się po jego ustawieniu, aplikacja będzie wymagać dodatkowego kodu w `ILocationListener` metody &ndash; `OnProviderEnabled`, `OnProviderDisabled`, i `OnStatusChanged` &ndash; do obsługi co możliwości związane z Przełącznik dostawcy.

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano uzyskiwanie lokalizacji użytkownika przy użyciu zarówno usługa lokalizacji systemu Android, jak i dostawcy kolei lokalizacji z interfejsu API Google lokalizacji usług.


## <a name="related-links"></a>Linki pokrewne

- [W lokalizacji (przykład)](https://developer.xamarin.com/samples/Location/)
- [FusedLocationProvider (przykład)](https://developer.xamarin.com/samples/FusedLocationProvider/)
- [Usług Google Play](http://developer.android.com/google/play-services/index.html)
- [Kryteria — klasa](https://developer.xamarin.com/api/type/Android.Locations.Criteria/)
- [Klasa LocationManager](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/)
- [Klasa LocationListener](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/)
- [LocationClient interfejsu API](http://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener interfejsu API](http://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest interfejsu API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
