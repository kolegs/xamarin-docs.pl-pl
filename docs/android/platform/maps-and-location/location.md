---
title: Usługi lokalizacji
description: W tym przewodniku wprowadza Rozpoznawanie lokalizacji w aplikacjach dla systemu Android oraz pokazano, jak można pobrać lokalizacji użytkownika przy użyciu interfejsu API systemu Android usługi lokalizacji, a także dostępny dostawca lokalizacji z kolei za pomocą interfejsu API z usługi Google lokalizacji.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/22/2018
ms.openlocfilehash: 2cd49441ec9ba15cd7fd2647fa74b39b32ea8acd
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780637"
---
# <a name="location-services"></a>Usługi lokalizacji

_W tym przewodniku wprowadza Rozpoznawanie lokalizacji w aplikacjach dla systemu Android oraz pokazano, jak można pobrać lokalizacji użytkownika przy użyciu interfejsu API systemu Android usługi lokalizacji, a także dostępny dostawca lokalizacji z kolei za pomocą interfejsu API z usługi Google lokalizacji._

## <a name="location-services-overview"></a>Omówienie usług lokalizacji

Android umożliwia dostęp do różnych technologii lokalizacji, takich jak lokalizacja tower komórka, sieci Wi-Fi i GPS. Szczegóły dotyczące poszczególnych technologii lokalizacji zostały wyabstrahowane przez *dostawców lokalizacji*, dzięki czemu aplikacje, aby uzyskać lokalizacji w taki sam sposób niezależnie od tego, Dostawca używany. Ten przewodnik stanowi wprowadzenie dostawcy lokalizacji z kolei, część usług Google Play, określający inteligentnie najlepszym sposobem na uzyskiwanie lokalizacji, urządzeń, w oparciu o jakie dostawcy są dostępne i sposobu korzystania z urządzenia. Interfejs API usługi lokalizacji dla systemu android i pokazuje, jak komunikować się z lokalizacji w systemie usługi przy użyciu `LocationManager`. Druga sekcja przewodnika odkrywa, przy użyciu interfejsu API systemu Android usług lokalizacji `LocationManager`.
 
Ogólna zasada mówi aplikacje powinny wolą używać dostawcy lokalizacji z kolei, nastąpi powrót starsze lokalizacji usługi interfejsu API systemu Android tylko wtedy, gdy jest to konieczne.

## <a name="location-fundamentals"></a>Podstawowe informacje dotyczące lokalizacji

W systemie Android, niezależnie od tego, jakie interfejsu API możesz wybrać do pracy z danymi lokalizacji kilka koncepcji pozostają takie same. Ta sekcja wprowadza lokalizacji dostawcy i uprawnienia powiązane z lokalizacji.

### <a name="location-providers"></a>Lokalizacja dostawcy

Kilka technologie są używane wewnętrznie w celu określenia lokalizacji użytkownika. Typ zależy od sprzętu używanego *dostawy* wybrane zadania zbierania danych. System android używa trzech dostawców lokalizacji:

-   **Dostawca GPS** &ndash; GPS zapewnia najbardziej dokładna lokalizacja używa najbardziej zasilania i działa najlepiej na zewnątrz. Ten dostawca używa kombinacji GPS oraz asystowanej GPS ([aGPS](http://en.wikipedia.org/wiki/Assisted_GPS)), która zwraca dane GPS zebrane przez towers sieci komórkowej.

-   **Dostawcy sieci** &ndash; zawiera kombinację danych sieci Wi-Fi i sieci komórkowej, w tym aGPS danych zbieranych przez towers komórki. Używa mniej energii niż dostawca GPS, ale zwraca dane lokalizacji różnej dokładności.

-   **Dostawca pasywnym** &ndash; opcji "nakładane", przy użyciu dostawców żądane przez inne aplikacje lub usługi, aby wygenerować dane lokalizacji w aplikacji. Jest to mniej niezawodne ale idealna opcja oszczędzania energii dla aplikacji, które nie wymagają stałej lokalizacji aktualizacji do pracy.

Lokalizacja dostawcy nie są zawsze dostępne. Na przykład chcemy do użycia GPS dla naszej aplikacji, ale GPS, może być wyłączone w ustawieniach lub urządzenie może nie mieć GPS w ogóle. Jeśli określony dostawca nie jest dostępna, wybierając ten dostawca może zwrócić `null`.

### <a name="location-permissions"></a>Uprawnienia do lokalizacji

Aplikacji obsługujących lokalizację wymagany jest dostęp urządzenia sensorów do odbierania GPS, sieci Wi-Fi i danych komórkowych. Dostęp jest kontrolowany za pośrednictwem odpowiednich uprawnień w aplikacji manifestu systemu Android.
Istnieją dwa uprawnienia &ndash; w zależności od wymagań aplikacji i wybór interfejsu API, można zezwolić jednemu:

-   `ACCESS_FINE_LOCATION` &ndash; Umożliwia aplikacji dostęp do systemu GPS.
    Wymagane dla *dostawcy GPS* i *pasywnym dostawcy* opcje (*pasywnym dostawcy wymaga uprawnienia dostępu GPS danych zbieranych przez inną aplikację lub usługę*). Opcjonalne uprawnienia dla *dostawcy sieci*.

-   `ACCESS_COARSE_LOCATION` &ndash; Umożliwia aplikacji dostęp do lokalizacji w sieci komórkowej i sieci Wi-Fi. Wymagane dla *dostawcy sieci* Jeśli `ACCESS_FINE_LOCATION` nie jest ustawiona.

W przypadku aplikacji przeznaczonych dla interfejsu API w wersji 21 (Android 5.0 Lollipop) lub nowszej, aby umożliwić `ACCESS_FINE_LOCATION` i nadal działają na urządzeniach, które nie mają GPS sprzętu. Jeśli aplikacja wymaga GPS sprzętu, należy jawnie dodać `android.hardware.location.gps` `uses-feature` elementu manifestu systemu Android. Aby uzyskać więcej informacji, zobacz Android [używa funkcji](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) odwołanie do elementu.

Aby ustawić uprawnienia, rozwiń węzeł **właściwości** folderu w **konsoli rozwiązania** i kliknij dwukrotnie **AndroidManifest.xml**. Uprawnienia, które będą wyświetlane w **wymagane uprawnienia**:

[![Zrzut ekranu przedstawiający ustawienia systemu Android Manifest wymagane uprawnienia](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

Ustawienia jednej z tych uprawnień informuje systemu Android, czy Twoja aplikacja potrzebuje zgody użytkownika w celu uzyskania dostępu do dostawcy lokalizacji. Urządzenia uruchom poziom interfejsu API 22 (Android 5.1) lub niższych poprosi użytkownika, aby udzielić tych uprawnień za każdym razem, gdy aplikacja jest zainstalowana. Na urządzeniach z systemem interfejsu API na poziomie 23 (Android 6.0) lub wyższą od nich, aplikacja powinna przeprowadzić sprawdzanie uprawnienia środowiska wykonawczego przed wysłaniem żądania lokalizacji dostawcy programu. 

> [!NOTE]
>Uwaga: Ustawienie `ACCESS_FINE_LOCATION` wymaga dostępu do danych zarówno zgrubnym i poprawnie lokalizacji. Nigdy nie powinna zajść można ustawić zarówno uprawnień, tylko *minimalny* uprawnień wymaganych przez aplikację do pracy.

Ten fragment kodu jest przykładem sprawdzić, czy aplikacja ma uprawnienia do `ACCESS_FINE_LOCATION` uprawnień:

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

Aplikacje, musi być odporny na błędy scenariusza, w którym użytkownik nie będzie udzielać uprawnienia (lub odwołał uprawnienie) i sposób poprawnego działania rozwiązania tej sytuacji. Zobacz [Przewodnik po uprawnieniach](~/android/app-fundamentals/permissions.md) więcej informacji na temat implementowania uprawnień w czasie wykonywania sprawdza platformie Xamarin.Android.


## <a name="using-the-fused-location-provider"></a>Korzystanie z kolei lokalizacji dostawcy

Dostawca lokalizacji z kolei jest preferowany sposób dla aplikacji systemu Android otrzymywać aktualizacje lokalizacji z urządzenia, ponieważ będzie efektywnie wybrać dostawcę lokalizacji w czasie wykonywania, aby zapewnić najlepsze informacji o lokalizacji, w sposób wydajny baterii. Na przykład użytkownik zalet wokół na zewnątrz pobiera najlepszą lokalizację odczytu z GPS. Jeśli użytkownik przegląda następnie pomieszczeniu, gdzie GPS działa źle (Jeśli w ogóle), dostawca lokalizacji z kolei może automatycznie Przełącz się do sieci Wi-Fi, który działa lepiej w pomieszczeniu.
 
Interfejs API dostawcy lokalizacji z kolei zawiera szeroką gamą innych narzędzi przeznaczonych dla aplikacji obsługujących lokalizację, w tym geofencing i monitorowania aktywności. W tej sekcji użyjemy koncentracji uwagi na temat konfigurowania `LocationClient`, ustanawiania dostawców i uzyskiwanie lokalizacji użytkownika.

Dostawca lokalizacji z kolei jest częścią [usług Google Play](http://developer.android.com/google/play-services/index.html).
Pakiet usług Google Play, musi być zainstalowane i poprawnie skonfigurowane w aplikacji dostawcy lokalizacji z kolei interfejsu API do pracy, a urządzenie musi mieć Google Play Services zestawu APK zainstalowane.

Przed platformy Xamarin.Android aplikacja może używać dostawcy lokalizacji z kolei go należy dodać **Xamarin.GooglePlayServices.Maps** pakietu do projektu. Ponadto następujące `using` instrukcji powinny zostać dodane do wszelkich plikach źródłowych, odwołujące się do klasy opisane poniżej:

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Sprawdzanie, czy zainstalowano usług Google Play

Operacja zostanie wykonana zostanie platformy Xamarin.Android, awarii, jeśli próbuje użyć dostawcy lokalizacji z kolei jeśli nie zainstalowano usługi Google Play (lub nieaktualny), a następnie wyjątek czasu wykonywania.  Jeśli nie zainstalowano usługi Google Play, aplikacja powinna spowodować powrót do usługi lokalizacji dla systemu Android, omówione powyżej. Jeśli usługi Google Play jest nieaktualna, aplikację można wyświetlić komunikatu użytkownika z pytaniem o nich można zaktualizować zainstalowana wersja usługi Google Play.

Ten fragment kodu znajduje się przykład jak działanie systemu Android można programowo Sprawdź, czy zainstalowano usług Google Play:

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

Do interakcji z kolei dostawy, aplikacji platformy Xamarin.Android musi mieć instancję `FusedLocationProviderClient`. Ta klasa udostępnia niezbędne metody, aby subskrybować aktualizacje lokalizacji i do pobrania ostatniej znanej lokalizacji urządzenia.

`OnCreate` Metoda działania jest odpowiednim miejscu, aby pobrać odwołanie do `FusedLocationProviderClient`, jak pokazano w poniższym fragmencie kodu:

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

### <a name="getting-the-last-known-location"></a>Wprowadzenie do ostatniej znanej lokalizacji

`FusedLocationProviderClient.GetLastLocationAsync()` Metoda zapewnia proste, nieblokującej na poziomie aplikacji platformy Xamarin.Android szybko uzyskać ostatniej znanej lokalizacji urządzenia przy użyciu minimalnego kodowania obciążenie.

Ten fragment kodu przedstawia sposób użycia `GetLastLocationAsync` metodę, aby pobrać lokalizacji urządzenia:

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

Aplikacji platformy Xamarin.Android można również subskrybować aktualizacje lokalizacji dostawcy lokalizacji z kolei za `FusedLocationProviderClient.RequestLocationUpdatesAsync` metodzie, jak pokazano w poniższym przykładzie:

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```
Ta metoda przyjmuje dwa parametry:

-   **`Android.Gms.Location.LocationRequest`** &ndash; A `LocationRequest` obiekt jest jak aplikacji platformy Xamarin.Android przekazuje parametry w działaniu dostawcy kolei lokalizacji. `LocationRequest` Przechowuje informacje takie jak ustanowić częste żądania lub jak ważne powinny być dokładne lokalizacji aktualizacji. Na przykład żądanie lokalizacji ważnych spowoduje, że korzystanie z GPS i w związku z tym większe możliwości, podczas określania lokalizacji na urządzeniu. Ten fragment kodu przedstawia sposób tworzenia `LocationRequest` lokalizacji o wysokiej dokładności, sprawdzanie mniej więcej co pięć minut dla aktualizacji lokalizacji (ale nie wcześniej niż dwie minuty między żądaniami). Dostawca lokalizacji z kolei użyje `LocationRequest` jako wskazówkę, dla której dostawca lokalizację do użycia podczas próby można określić lokalizacji urządzenia:

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```
                                          

-   **`Android.Gms.Location.LocationCallback`** &ndash; Aby otrzymywać aktualizacje lokalizacji, aplikacji platformy Xamarin.Android musisz podklasy `LocationProvider` klasy abstrakcyjnej. Ta klasa widoczne dwie metody, które może być wywoływany przez dostawcę lokalizacji z kolei aktualizowanie aplikacji przy użyciu informacji o lokalizacji. To zostanie dokładnie omówione bardziej szczegółowo poniżej.

Aby powiadomić aplikacji platformy Xamarin.Android aktualizacji lokalizacji, dostawca lokalizacji z kolei spowoduje wywołanie `LocationCallBack.OnLocationResult(LocationResult result)`. `Android.Gms.Location.LocationResult` Parametr będzie zawierać informacje o lokalizacji aktualizacji.

Gdy dostawca lokalizacji z kolei wykryje zmianę dostępności danych lokalizacji, wywoła `LocationProvider.OnLocationAvaibility(LocationAvailability
locationAvailability)` metody. Jeśli `LocationAvailability.IsLocationAvailable` właściwość zwraca `true`, a następnie można założyć, że wyniki lokalizacji urządzenia zgłoszone przez `OnLocationResult` są tak dokładne i możliwie aktualne zgodnie z wymogami `LocationRequest`. Jeśli `IsLocationAvailable` ma wartość FAŁSZ, żadne wyniki lokalizacji będą zwracane przez `OnLocationResult`.

Ten fragment kodu jest przykład implementacji `LocationCallback` obiektu:

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

## <a name="using-the-android-location-service-api"></a>Za pomocą interfejsu API usługi lokalizacji dla systemu Android

Usługi lokalizacji dla systemu Android to interfejs API starszych dotyczące korzystania z informacji o lokalizacji w systemie Android. Dane lokalizacji są zbierane przez sensorów i zebrane przez usługę system, który jest dostępny w aplikacji za pomocą `LocationManager` klasy i `ILocationListener`.

Usługa lokalizacji jest najbardziej odpowiednia dla aplikacji, które należy uruchomić na urządzeniach, które nie mają usług Google Play zainstalowane.

Usługa lokalizacji jest specjalnym typem [usługi](http://developer.android.com/guide/components/services.html) zarządzane przez System. Usługa systemowa wchodzi w interakcję z urządzeń sprzętowych i jest zawsze uruchomiona. Korzystać z aktualizacje lokalizacji w naszej aplikacji, firma Microsoft będzie subskrybować aktualizacje lokalizacji z przy użyciu usługi lokalizacji systemu `LocationManager` i `RequestLocationUpdates` wywołania.

Można uzyskać od lokalizacji użytkownika przy użyciu usługi lokalizacji dla systemu Android obejmuje kilka kroków:

1.  Pobierz odwołanie do `LocationManager` usługi.
2.  Implementowanie `ILocationListener` interfejsu i obsługują zdarzenia po zmianie lokalizacji.
3.  Użyj `LocationManager` żądanie lokalizacji aktualizacji dla określonego dostawcy. `ILocationListener` z poprzedniego kroku, będzie używany do odbierania wywołań zwrotnych z `LocationManager`.
4.  Zatrzymaj aktualizacje lokalizacji, gdy aplikacja nie jest już odpowiednie otrzymywać aktualizacje.

### <a name="location-manager"></a>Program Location Manager

Firma Microsoft dostęp do usługi lokalizacji systemu z wystąpieniem `LocationManager` klasy. `LocationManager` jest klasą specjalne, która pozwala nam interakcji z lokalizacji w systemie usługi i wywołania metody. Aplikacja może odwołać się do `LocationManager` przez wywołanie metody `GetSystemService` i przekazując typ usługi, jak pokazano poniżej:

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` jest dobrym miejscem, aby pobrać odwołanie do `LocationManager`.
To dobry pomysł, aby zachować `LocationManager` jako zmienną klasy tak, aby nazywamy je w różnych punktach cykl życia aktywności.

### <a name="request-location-updates-from-the-locationmanager"></a>Aktualizacje lokalizacji żądania z LocationManager

Gdy aplikacja ma odwołanie do `LocationManager`, należy go sprawdzić `LocationManager` jakiego rodzaju informacje o lokalizacji, które są wymagane, a częstotliwość tych informacji ma zostać zaktualizowany. To zrobić, wywołując `RequestionLocationUpdates` na `LocationManager` obiektu i przekazywanie w kryteriów, aktualizacji i wywołanie zwrotne, które będą otrzymywać aktualizacje lokalizacji. To wywołanie zwrotne jest typem, który muszą implementować `ILocationListener` interfejsu (opisany bardziej szczegółowo w dalszej części tego przewodnika).

`RequestionLocationUpdates` Metoda informuje lokalizacji w systemie usługi aplikacji czy chcesz zacząć otrzymywać aktualizacje lokalizacji. Ta metoda umożliwia określenie dostawcy, a także czas i odległość progi kontrolować częstotliwość aktualizacji. Na przykład metoda poniżej należących do żądań lokalizacji aktualizuje od dostawcy lokalizacji GPS co 2000 milisekund, a tylko, gdy lokalizacja zmienia więcej niż 1 metr:

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

Aplikacja powinna tylko tak często, zgodnie z potrzebami, aplikacja może działać dobrze zażądać aktualizacje lokalizacji. Zachowuje czas pracy baterii i tworzy lepsze środowisko użytkownika.

### <a name="responding-to-updates-from-the-locationmanager"></a>Odpowiadanie na aktualizacje z LocationManager

Gdy aplikacja zażądała aktualizacje z `LocationManager`, może ona odbierać informacje z usługi przez zaimplementowanie [ `ILocationListener` ](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/) interfejsu. Ten interfejs zapewnia cztery metody w celu nasłuchiwania na lokalizację usługi i dostawcę lokalizacji `OnLocationChanged`. System będzie wywoływać `OnLocationChanged` lokalizacji użytkownika zmiany wystarczająco dużo, aby zakwalifikować się jako zmianę lokalizacji zgodnie z kryteriami ustawiana, jeśli żądanie aktualizacje lokalizacji. 

Poniższy kod przedstawia metody w `ILocationListener` interfejsu:

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

### <a name="unsubscribing-to-locationmanager-updates"></a>Anulowanie subskrypcji LocationManager aktualizacji

W celu oszczędzania zasobów systemu, aplikacji należy jak najszybciej anulować subskrypcję aktualizacje lokalizacji. `RemoveUpdates` Informuje metoda `LocationManager` zatrzymania wysyłania aktualizacji do naszej aplikacji.  Na przykład działanie może wywołać `RemoveUpdates` w `OnPause` metody, aby można oszczędzać energię, jeśli aplikacja nie potrzebuje aktualizacje lokalizacji podczas jego działanie nie znajduje się na ekranie:

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

Jeśli aplikacja wymaga pobrać aktualizacje lokalizacji znajduje się w tle, należy utworzyć to usługa niestandardowa, która ją subskrybuje systemu usługi lokalizacji. Zapoznaj się [uruchamianie procesów w tle z usługami dla systemu Android](~/android/app-fundamentals/services/index.md) przewodnika, aby uzyskać więcej informacji.

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>Określanie lokalizacji najlepsze dostarczyciela LocationManager

Powyżej aplikacja ustawia GPS jako dostawca lokalizacji. Jednak GPS może nie być dostępne we wszystkich przypadkach, takich jak Jeśli urządzenie jest w pomieszczeniu, lub nie ma odbiornik GPS. Jeśli jest to możliwe, wynik jest `null` zwracaną dla dostawcy.

Aby uzyskać aplikację w przypadku niedostępności systemu GPS, należy użyć `GetBestProvider` metodę, aby poprosić o dostawcę najlepsze dostępnej lokalizacji (obsługiwane urządzenia i włączenie użytkownika), podczas uruchamiania aplikacji. Zamiast określonego dostawcy, będzie widnieć napis `GetBestProvider` wymagania dotyczące dostawcy — takiego jak dokładność i power - o [ `Criteria` obiektu](https://developer.xamarin.com/api/type/Android.Locations.Criteria/). `GetBestProvider` Zwraca dostawcę najlepsze dla podanych kryteriów.

Poniższy kod przedstawia sposób pobierania najlepiej dostawcy i używać go podczas żądania aktualizacje lokalizacji:

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
>  Jeśli użytkownik wyłączył wszystkich dostawców lokalizacji, `GetBestProvider` zwróci `null`. Aby zobaczyć, jak działa ten kod na rzeczywistym urządzeniu, upewnij się umożliwić GPS, sieci Wi-Fi i sieci komórkowe w obszarze **Google Ustawienia > lokalizacja > Tryb** pokazany na tym zrzucie ekranu:

[![Ustawienia trybu lokalizacji ekranu na telefonie z systemem Android](location-images/location-02.png)](location-images/location-02.png#lightbox)

Poniższy zrzut ekranu przedstawia lokalizację aplikacji uruchomionych za pomocą `GetBestProvider`:

[![Aplikacja GetBestProvider Wyświetlanie szerokości, długości i dostawcy](location-images/location-03.png)](location-images/location-03.png#lightbox)

Należy pamiętać, że `GetBestProvider` nie zmienia dostawcę dynamicznie. Przeciwnie określa najlepiej dostępnego dostawcę, raz w ciągu cyklu życia aktywności. Jeśli stan dostawcy zmieni się po jej ustawieniu, aplikacja będzie wymagać dodatkowego kodu w `ILocationListener` metody &ndash; `OnProviderEnabled`, `OnProviderDisabled`, i `OnStatusChanged` &ndash; do obsługi każdego możliwości związane z Przełącznik dostawcy.

## <a name="summary"></a>Podsumowanie

Ten przewodnik obejmuje uzyskiwanie lokalizacji użytkownika przy użyciu zarówno usługi lokalizacji dla systemu Android, jak i dostawcy lokalizacji z kolei z interfejsu API usługi lokalizacji Google.


## <a name="related-links"></a>Linki pokrewne

- [Lokalizacja (przykład)](https://developer.xamarin.com/samples/Location/)
- [FusedLocationProvider (przykład)](https://developer.xamarin.com/samples/FusedLocationProvider/)
- [Usługi Google Play](http://developer.android.com/google/play-services/index.html)
- [Kryteria, klasa](https://developer.xamarin.com/api/type/Android.Locations.Criteria/)
- [Klasa LocationManager](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/)
- [Klasa LocationListener](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/)
- [LocationClient interfejsu API](http://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener interfejsu API](http://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest interfejsu API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
