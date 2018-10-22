---
title: Przy użyciu map Google interfejsu API w aplikacji
description: Sposób implementowania interfejsu API map Google v2 funkcji w aplikacji platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/25/2018
ms.openlocfilehash: a0e010a8300eb4b4452737e34d2f55a35ab95428
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935142"
---
# <a name="using-the-google-maps-api-in-your-application"></a>W aplikacji przy użyciu interfejsu API map Google

Przy użyciu aplikacji mapy stanowi doskonałe rozwiązanie, ale czasami chcesz dołączyć mapy bezpośrednio w aplikacji. Oprócz wbudowanej aplikacji, map Google oferuje również [natywne mapowanie interfejsu API dla systemu Android](https://developers.google.com/maps/documentation/android/).
Interfejsu API map jest odpowiednia dla przypadków, w której chcesz zachować większą kontrolę nad środowiskiem mapowania. Obejmuje to między innymi, które można wykonać przy użyciu interfejsu API map:

-  Programowo, zmieniając widzenia mapy.
-  Dodawanie i Dostosowywanie znaczników.
-  Dodawanie adnotacji do mapy z nakładki.

W odróżnieniu od v1 interfejsu API systemu Android map Google obecnie przestarzały interfejsu API Google Maps systemu Android w wersji 2 jest częścią [usług Google Play](http://developer.android.com/google/play-services/index.html).
W związku z tym jest konieczne do spełnienia niektóre obowiązkowe wymagania wstępne, zanim będzie go można użyć interfejsu API systemu Android map Google w aplikacji platformy Xamarin.Android.


## <a name="google-maps-api-prerequisites"></a>Wymagania wstępne dotyczące interfejsu API map Google

Kilka elementów, należy skonfigurować przed użyciem interfejsu API map, w tym:

-  Zainstaluj usługi Google Play zestawu SDK
-  Utwórz emulator z interfejsów API Google
-  Uzyskaj klucz interfejsu API map
-  Określ wymagane uprawnienia



### <a name="install-the-google-play-services-sdk"></a>Zainstaluj usługi Google Play zestawu SDK

Usług Google Play jest technologią z Google, który umożliwia aplikacji systemu Android skorzystać z różnych funkcji Google, takich jak Google + rozliczeń w aplikacji i mapy. Te funkcje są dostępne na urządzeniach z systemem Android jako usługi w tle, które są zawarte w [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en).

Aplikacje systemu android w interakcje z usług Google Play za pomocą biblioteki klienta usług Google Play. Ta biblioteka zawiera klasy dla poszczególnych usług, takich jak map i interfejsów. Na poniższym diagramie przedstawiono relację między Android aplikacji i usług Google Play:

![Diagram pokazujący aktualizowanie Google Play Services APK sklepu Google Play](maps-api-images/play-services-diagram.png)

Interfejs API systemu Android mapy znajduje się w ramach usług Google Play.
Zanim aplikacji platformy Xamarin.Android można użyć interfejsu API map, Google Play Services SDK musi być zainstalowana i powiązany. Google Play Services SDK jest zainstalowany za pomocą Menedżera zestawu SDK systemu Android. Poniższy zrzut ekranu przedstawia where w Android SDK Manager znajduje się klient usług Google Play:

![Usług Google Play jest wyświetlany w obszarze dodatki w Menedżerze zestawu SDK systemu Android](maps-api-images/image01.png)

> [!NOTE]
> Usługi Google Play APK jest licencjonowanego produktu, które nie mogą być obecne na wszystkich urządzeniach. Jeśli nie jest zainstalowany, map programu Google nie będzie działać na urządzeniu.


#### <a name="binding-google-play-services"></a>Powiązanie witryny Google Play Services

Po zainstalowaniu usług Google Play biblioteki klienta musi być powiązana przez bibliotekę powiązanie Java platformy Xamarin.Android. Istnieją dwa sposoby, w tym celu:

-  **Za pomocą pakietu Google Play Services mapy NuGet** — jest to najprostsza metoda (dostępne tylko w Xamarin.Android 4.8 lub nowszej).
   Zainstaluj [Xamarin Google Play Services mapy NuGet](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps); spowoduje to również zainstalowanie wszystkie pakiety zależności usług Google Play.
   W dalszej części tego podręcznika koncentruje się na tej metody.

-  **Ręczne powiązanie biblioteki klienta usług Google Play** — jest to bardziej złożonych podejście, jedynym sposobem dla platformy Xamarin.Android 4.4 lub Xamarin.Android 4.6 powiązania zestawu SDK usług Google Play.
   Ręcznie powiązanie biblioteki klienta usług Google Play wykracza poza zakres tego dokumentu, ale przykładem sposobu wykonania tego zadania można znaleźć w [mapy i pokaz lokalizacji próbki v3](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3) w witrynie Github.


#### <a name="adding-the-google-play-services-map-package"></a>Dodawanie pakietu mapy usług Google Play

Aby dodać pakiet Google Play Services mapy, kliknij prawym przyciskiem myszy **odwołania** folderu projektu w Eksploratorze rozwiązań i kliknij przycisk **Zarządzaj pakietami NuGet...** :

![Elementu menu kontekstowego Zarządzaj pakietami NuGet przedstawiający Eksploratora rozwiązań w obszarze odwołania](maps-api-images/image02.png)

Spowoduje to otwarcie **Menedżera pakietów NuGet**. Kliknij przycisk **Przeglądaj** , a następnie wprowadź **Xamarin Google Play usługi mapy** w polu wyszukiwania. Wybierz **Xamarin.GooglePlayServices.Maps** i kliknij przycisk **zainstalować**. (Jeśli wcześniej zainstalowano ten pakiet, kliknij przycisk **aktualizacji**.):

[![Menedżer pakietów NuGet z pakietem Xamarin.GooglePlayServices.Maps wybrane](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

Zwróć uwagę, również zainstalowania następujących pakietów dla zależności:

-   **Xamarin.GooglePlayServices.Base**
-   **Xamarin.GooglePlayServices.Basement**
-   **Xamarin.GooglePlayServices.Tasks**



### <a name="create-an-emulator-with-google-apis"></a>Utwórz Emulator z interfejsów API Google

Chociaż nie jest to zalecane, jest możliwość skonfigurowania emulatora do obsługi interfejsu API systemu Android mapy. Emulator musi być skonfigurowany pod kątem 17 poziom interfejsu API Google (Android 4.2.2) lub nowszej. Na poniższym zrzucie ekranu obraz emulatora jest skonfigurowany dla interfejsu API 19 poziomu: 

![Android Emulator Manager z AVD skonfigurowane dla interfejsu API 19 poziom](maps-api-images/image04.png)



### <a name="obtain-a-google-maps-api-key"></a>Uzyskaj klucz interfejsu API map Google

Ostatnim krokiem jest uzyskanie klucza interfejsu API map Google (należy pamiętać, że nie można użyć ponownie klucz interfejsu API ze starszej wersji v1 map programu Google). Aby uzyskać informacje o tym, jak uzyskać i korzystać z platformy Xamarin.Android klucz interfejsu API, zobacz [uzyskiwania klucz interfejsu API map Google A](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
 


### <a name="specify-the-required-permissions"></a>Określ wymagane uprawnienia

Następujące uprawnienia muszą być określone w **AndroidManifest.XML** dla interfejsu API systemu Android map Google:

-  **Dostęp do stanu sieci** &ndash; interfejsu API map musi być w stanie sprawdzić, czy można pobrać kafelków mapy.

-  **Dostęp do Internetu** &ndash; dostęp do Internetu jest niezbędne do pobrania Kafelki map i komunikować się z Google Play serwerów dostępu do interfejsu API.

-  **Interfejsy OpenGL ES v2** &ndash; aplikacji, muszą deklarować wymaganie OpenGL ES v2.

-  **Klucz interfejsu API map Google** &ndash; klucz interfejsu API jest używany, aby upewnić się, że aplikacja jest zarejestrowany i autoryzowane do używania usług Google Play. Zobacz [uzyskiwania klucza interfejsu API map Google](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) szczegółowe informacje na temat tego klucza.

-  **Zapisu w pamięci zewnętrznej,** &ndash; Google Maps interfejsu API systemu Android zostaną zbuforowane pobrany Kafelki w pamięci zewnętrznej.

-  **Dostęp do usług opartych na sieci Google Web** &ndash; aplikacji wymaga uprawnień dostępu do usług sieci web firmy Google, obsługujące interfejs API systemu Android mapy.

-  **Uprawnienia Google Play Services powiadomienia o** &ndash; aplikacji musi mieć uprawnienie do odbierania powiadomień zdalnego z usług Google Play.

-  **Dostęp do lokalizacji dostawców** &ndash; są opcjonalne uprawnienia.
   Umożliwiają one będą `GoogleMap` klasy do wyświetlenia lokalizację urządzenia na mapie.


Poniższy fragment jest przykładem ustawienia, które muszą zostać dodane do **AndroidManifest.XML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="14" android:targetSdkVersion="17" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- We need to be able to download map tiles and access Google Play Services-->
    <uses-permission android:name="android.permission.INTERNET" />

    <!-- Allow the application to access Google web-based services. -->
    <uses-permission android:name="com.google.android.providers.gsf.permission.READ_GSERVICES" />

    <!-- Google Maps for Android v2 will cache map tiles on external storage -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

    <!-- Google Maps for Android v2 needs this permission so that it may check the connection state as it must download data -->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <!-- Permission to receive remote notifications from Google Play Services -->
    <!-- Notice here that we have the package name of our application as a prefix on the permissions. -->
    <uses-permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" />
    <permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" android:protectionLevel="signature" />

    <!-- These are optional, but recommended. They will allow Maps to use the My Location provider. -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />


    <application android:label="@string/app_name">
        <!-- Put your Google Maps V2 API Key here. -->
        <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
        <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
    </application>
</manifest>
```


## <a name="the-googlemap-class"></a>Klasa GoogleMap

Gdy wymagania wstępne są podejmowane obsługę, nadszedł czas na rozpocząć tworzenie aplikacji i użyj interfejsu API systemu Android mapy. [GoogleMap](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap) klasa jest głównym interfejsu API, które aplikacji platformy Xamarin.Android będzie używane do wyświetlanie i używanie map Google dla systemu Android. Ta klasa ma następujące obowiązki:

-  Interakcja z usługi Google Play, aby przyznać aplikacji z usługą sieci web firmy Google.

-  Pobieranie, buforowanie i wyświetlanie kafelków mapy.

-  Wyświetlanie formantów interfejsu użytkownika, takich jak kadrowania i powiększenia do użytkownika.

-  Rysowanie znaczniki i kształtów geometrycznych na mapach.

`GoogleMap` Zostanie dodany do działania w jednej z dwóch sposobów:

-  **MapFragment** - [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) jest specjalne fragmentem, który działa jako host `GoogleMap` obiektu. `MapFragment` Wymaga poziom interfejsu API systemu Android, 12 lub nowszym.
   Starsze wersje systemu android można użyć [SupportMapFragment](http://developer.android.com/reference/com/google/android/gms/maps/SupportMapFragment.html).

-  **MapView** - [MapView](https://developer.xamarin.com/api/type/Android.GoogleMaps.MapView/) jest podklasą widoku specjalne, które mogą pełnić rolę hosta dla `GoogleMap` obiektu. Użytkownicy tej klasy musi przekazywać wszystkie metody cyklu życia działania do `MapView` klasy.

Każdy z tych kontenerów ujawnia `Map` właściwość, która zwraca wystąpienie klasy `GoogleMap`. Preferowany do [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) klasy, ponieważ jest prostsza interfejs API, który ogranicza kodu umożliwiającego kwota dewelopera ręcznie musi implementować.


### <a name="adding-a-mapfragment-to-an-activity"></a>Dodawanie MapFragment do działania

Poniższy zrzut ekranu jest bardzo prosty przykład `MapFragment`:

[![Zrzut ekranu przedstawiający urządzenia wyświetlającego fragment mapy](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

Podobnie jak inne klasy fragmentu, istnieją dwa sposoby dodawania to `MapFragment` do działania:

-   **Deklaratywnie** — `MapFragment` można dodać za pomocą pliku układu XML dla działania. Następujący fragment kodu XML przedstawiono przykład sposobu użycia `fragment` elementu:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

-   **Programowo** — `MapFragment` można dodać programistycznie, zgodnie z opisem.

Aby dodać programistycznie `MapFragment`, działanie musi implementować `IOnMapReadyCallback` interfejsu. Ponieważ inicjowanie `GoogleMap` obiektu może zająć trochę czasu zgodnie z interfejsu API komunikuje się z witryny Google Play, musisz podać wywołania zwrotnego, który powiadamia aplikację podczas `GoogleMap` jest gotowy.

Najpierw dodaj `IOnMapReadyCallback` do `Activity` deklaracji klasy.
Na przykład:

```csharp
public class MapWithMarkersActivity : Activity, IOnMapReadyCallback
```

Następnie w `OnCreate` metody, Dodaj `MapFragment` jak pokazano w poniższym przykładzie kodu ( `GoogleMapOptions` klasa znajduje się w dalszej części tego przewodnika):

```csharp
_mapFragment = FragmentManager.FindFragmentByTag("map") as MapFragment;
if (_mapFragment == null)
{
    GoogleMapOptions mapOptions = new GoogleMapOptions()
        .InvokeMapType(GoogleMap.MapTypeSatellite)
        .InvokeZoomControlsEnabled(false)
        .InvokeCompassEnabled(true);

    FragmentTransaction fragTx = FragmentManager.BeginTransaction();
    _mapFragment = MapFragment.NewInstance(mapOptions);
    fragTx.Add(Resource.Id.map, _mapFragment, "map");
    fragTx.Commit();
}
_mapFragment.GetMapAsync(this);
```

A `GoogleMap` musi zostać pobrany przy użyciu `GetMapAsync`, jak pokazano na końcu w poprzednim przykładzie kodu &ndash; to automatycznie inicjuje system map i widoku. (Należy pamiętać, że ta metoda nie korzysta z `await` / `async` semantyki &ndash; `Async` zachowanie jest zaimplementowana przez system Android.) Gdy `GoogleMap` obiekt jest gotowy, Android wymaga aplikacji `OnMapReady` — metoda (który musi zaimplementować jako część `IOnMapReadyCallback` interfejs). Na przykład:

```csharp
public void OnMapReady (GoogleMap map)
{
    _map = map;
}
```

W powyższym przykładzie kodu `OnMapReady` inicjuje wywołania zwrotnego `_map` zmienną utworzony `GoogleMap` obiektu.

Na przykład sposobu użycia tego wyniku Po `OnResume` jest wywoływana, go można sprawdzić, czy `_map` jest różna od null. Jeśli `_map` ustawiono `GoogleMap` obiektu `OnResume` można wywoływać metod w celu dodania znaczników i Przenieś jego aparatu do określonej długości i szerokości geograficznej. Na przykład kompletny kod, zobacz [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo).



### <a name="map-types"></a>Typy map

Brak dostępnych pięć różnych typów mapowania z interfejsu API map Google:

-  **Normalny** — jest to domyślny typ mapy. Przedstawia on drogi i ważnych funkcji fizycznych oraz niektóre syntetycznych punkty zainteresowań (na przykład budynków i mostków).

-  **Satelita** — ta mapa pokazuje fotografii satelity.

-  **Hybrydowe** — ta mapa pokazuje satelity fotografii, jak i mapy drogowej.

-  **Terenu** -oznacza to przede wszystkim z niektórych drogi topograficznych funkcji.

-  **Brak** — to mapowanie nie ładuje wszystkie Kafelki, jest on renderowany jako pustą siatkę.


Na poniższym obrazie pokazano trzy różne rodzaje map, od lewej do prawej (normalne, rozwiązanie hybrydowe, terenu):

[![Trzy mapy przykład zrzuty ekranu: Normalny, hybrydowej i terenu](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

`GoogleMap.MapType` Właściwość jest używana do ustaw lub zmień typ mapy jest wyświetlany. Poniższy fragment kodu przedstawia sposób wyświetlania mapy satelitarnej.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MapType = GoogleMap.MapTypeSatellite;
}
```


### <a name="googlemap-properties"></a>Właściwości GoogleMap

`GoogleMap` definiuje kilka właściwości określające funkcje i wyglądu mapy. Jednym ze sposobów konfigurowania początkowy stan `GoogleMap` jest przekazanie [GoogleMapOptions](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMapOptions.html) obiektu podczas tworzenia `MapFragment`. Poniższy fragment kodu jest jednym z przykładów przy użyciu `GoogleMapOptions` obiektu podczas tworzenia `MapFragment`:

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
_mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, _mapFragment, "map");
fragTx.Commit();
```

Innym sposobem skonfigurowania `GoogleMap` obiekt jest przez ustawienie wartości [UiSettings](http://developer.android.com/reference/com/google/android/gms/maps/UiSettings.html) właściwości obiektu mapy. Następny przykładowy kod przedstawia sposób konfigurowania `GoogleMap` do wyświetlania kontrolki powiększania i kompas:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.UiSettings.ZoomControlsEnabled = true;
    _map.UiSettings.CompassEnabled = true;
}
```


## <a name="interacting-with-the-map"></a>Interakcja z mapy

Interfejs API systemu Android społeczności Maps zawiera interfejsu API, który umożliwia działanie, aby zmienić z punktu widzenia, Dodaj znaczniki, umieść nakładki niestandardowych lub rysowanie kształtów geometrycznych. W tej sekcji będzie omawiać temat wykonać niektóre z tych zadań w platformy Xamarin.Android.

### <a name="changing-the-viewpoint"></a>Zmiana punktu obserwacji

Mapowania są sporządzony według wzoru jako płaska płaszczyzny na ekranie, oparte na merkatora. Widoku mapy, jest *aparatu* wyglądającej w dół na tym płaszczyzny. Pozycja kamery może być kontrolowane przez zmiana lokalizacji, powiększenia, pochylenie i wpływu. [CameraUpdate](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdate) klasa jest używana do przenieść lokalizację aparatu. `CameraUpdate` obiekty nie są bezpośrednio wystąpienia, zamiast tego interfejsu API map oferuje [CameraUpdateFactory](http://developer.android.com/reference/com/google/android/gms/maps/CameraUpdateFactory.html) klasy.

Raz `CameraUpdate` obiekt został utworzony, jest przekazywany jako parametr w jednej [GoogleMap.MoveCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#moveCamera%28com.google.maps.CameraUpdate%29) lub [GoogleMap.AnimateCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#animateCamera%28com.google.maps.CameraUpdate%29) metody. `MoveCamera` Metody aktualizacji mapy natychmiast podczas `AnimateCamera` metoda zapewnia płynne i animowany przejście.

Następujący fragment kodu jest prosty przykład sposobu użycia `CameraUpdateFactory` utworzyć `CameraUpdate` będą powodować zwiększenie poziom powiększenia mapy przez jedną:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Interfejs API map zawiera [CameraPosition](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) która będzie używana do agregowania wszystkie możliwe wartości pozycji kamery. Wystąpienie tej klasy może zostać dostarczona do [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) metodę, która zwróci `CameraUpdate` obiektu. Zawiera także interfejsu API map [CameraPosition.Builder](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) klasy, która zapewnia interfejs fluent API do tworzenia `CameraPosition` obiektów.
Poniższy fragment kodu przedstawia przykład tworzenia `CameraUpdate` z `CameraPosition` i używając jej do zmiany położenia kamery na `GoogleMap`:

```csharp
LatLng location = new LatLng(50.897778, 3.013333);
CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
builder.Target(location);
builder.Zoom(18);
builder.Bearing(155);
builder.Tilt(65);
CameraPosition cameraPosition = builder.Build();
CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(cameraUpdate);
}
```

W poprzednim fragmencie kodu, określonej lokalizacji na mapie jest reprezentowany przez [LatLng](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/LatLng) klasy. Poziom powiększenia jest ustawiony na 18. Wpływ jest wzorem pomiaru prawo od północy. Pochylenie właściwość kontroluje kąta i określa kąt 25 stopni w pionie. Poniższy zrzut ekranu przedstawia `GoogleMap` po wykonaniu powyższych kodu:

[![Przykładowa mapa Google przedstawiający w określonej lokalizacji z Wychylny wyświetlanie kąta](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)


### <a name="drawing-on-the-map"></a>Rysowanie na mapie

Interfejs API systemu Android mapy przewiduje rysowania następujące elementy na mapie interfejsu API:

-  **Znaczniki** — są to specjalne ikony, które są używane do identyfikowania w jednej lokalizacji na mapie.

-  **Nakładki** — jest to obraz, który może służyć do identyfikowania zbiór lokalizacji lub obszar na mapie.

-  **Linie, wielokątów i okręgi** — są to interfejsy API, który zezwala na dodawanie kształtów do mapy czynności.


#### <a name="markers"></a>Znaczniki

Interfejs API map zawiera [znacznika](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) klasy, który hermetyzuje wszystkie dane dotyczące jednej lokalizacji na mapie. Domyślnie używają standardowa ikona podał map programu Google. Istnieje możliwość dostosowywania wyglądu znacznika i odpowiadanie na kliknięcia użytkownika.


##### <a name="adding-a-marker"></a>Dodawanie znacznika

Aby dodać znacznik do mapy, konieczne jest Utwórz nową [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) obiekt, a następnie wywołać [AddMarker](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) metoda `GoogleMap` wystąpienia. Ta metoda zwróci [znacznika](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) obiektu.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    _map.AddMarker(markerOpt1);
}
```

Tytuł znacznika będzie wyświetlany w *okna informacje* po naciśnięciu na znacznika. Poniższy zrzut ekranu pokazuje, jak wygląda ten znacznik:

[![Przykładowa mapa Google ze znacznikiem i informacje o oknie pierścieniem Vimy](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)


##### <a name="customizing-a-marker"></a>Dostosowywanie znacznika

Można dostosować ikona przez znacznika przez wywołanie metody `MarkerOptions.InvokeIcon` metody podczas dodawania znacznika do mapy.
Ta metoda przyjmuje [BitmapDescriptor](http://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptor.html) obiekt zawierający dane wymagane do renderowania ikony. [BitmapDescriptorFactory](https://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory.html) klasy zawiera niektóre metody pomocnicze, aby uprościć tworzenie `BitmapDescriptor`. Poniżej przedstawiono niektóre z tych metod:

-   `DefaultMarker(float colour)` &ndash; Użyj domyślnego znacznika map programu Google, ale zmienić kolor.

-   `FromAsset(string assetName)` &ndash; Użyj ikon niestandardowych z określonego pliku w folderze Zasoby.

-   `FromBitmap(Bitmap image)` &ndash; Ikona, należy użyć określonej mapy bitowej.

-   `FromFile(string fileName)` &ndash; Tworzenie ikon niestandardowych z pliku w określonej ścieżce.

-   `FromResource(int resourceId)` &ndash; Tworzenie ikon niestandardowych z określonego zasobu.

Poniższy fragment kodu przedstawia przykład tworzenia znacznika błękitnego zabarwionych domyślne:

```csharp
mapFrag.GetMapAsync(this);
...
if (_map != null)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    markerOpt1.InvokeIcon(BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan));
    _map.AddMarker(markerOpt1);
}
```


#### <a name="info-windows"></a>Informacje o systemie Windows

*Informacje o windows* są specjalne windows tego menu podręczne do wyświetlenia informacji użytkownika podczas ich wybierz określonego znacznika. Domyślnie okno informacje zostanie wyświetlona zawartość znacznik tytułu. Jeśli tytuł nie został przypisany, zostanie wyświetlone okno nie informacji. Tylko jedno okno informacje mogą być wyświetlane w czasie.

Można dostosować okno informacje zaimplementowanie [GoogleMap.IInfoWindowAdapter](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) interfejsu. Istnieją dwie metody ważna w tym interfejsie:

-  `public View GetInfoWindow(Marker marker)` &ndash; Ta metoda jest wywoływana można pobrać okna informacje o niestandardowych na potrzeby znacznika. Jeśli zmienna zwraca `null` , zostanie użyta domyślna renderowanie okien. Jeśli ta metoda zwraca widok tego widoku zostaną umieszczone wewnątrz ramki okna informacje.

-  `public View GetInfoContents(Marker marker)` &ndash; Ta metoda zostanie wywołana tylko, jeśli zwróci GetInfoWindow `null` . Ta metoda może zwracać `null` wartość, gdy odwzorowanie domyślne informacje o zawartości okna, które ma być używany. W przeciwnym razie ta metoda powinna zwrócić widok o zawartość okna informacje.

Okno informacyjne nie jest widoku aktywnego — zamiast tego systemu Android zostanie przekonwertować widoku mapy bitowej statyczne i zostaną wyświetlone który na obrazie. Oznacza to okno informacyjne nie może odpowiadać na wszelkie zdarzenia touch lub gesty, ani nie jest on automatycznie aktualizowane automatycznie. Aby zaktualizować informacje o oknie, należy wywołać [GoogleMap.ShowInfoWindow](http://developer.android.com/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) metody.

Na poniższej ilustracji przedstawiono przykłady niektórych informacji o niestandardowych systemu Windows. Obraz po lewej stronie ma dostosowane, gdy obraz po prawej stronie ma okna i dostosować zawartość jego zawartość:

![Przykład znacznik systemu windows dla Melbourne, w tym ikonę i wypełniania. W oknie prawym ma zaokrąglone narożniki.](maps-api-images/marker-infowindows.png)


#### <a name="ground-overlays"></a>Podstaw nakładki

W przeciwieństwie do znaczników, które identyfikują konkretnej lokalizacji na mapie, [GroundOverlay](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlay.html) jest obrazem, który umożliwia identyfikowanie zbiór lokalizacji lub obszar na mapie.


##### <a name="adding-a-groundoverlay"></a>Dodawanie GroundOverlay

Dodawanie nakładki podstaw do mapy jest bardzo podobne do dodawania znacznika do mapy. Najpierw [GroundOverlayOptions](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlayOptions.html) tworzony jest obiekt. Ten obiekt jest następnie przekazany jako parametr `GoogleMap.AddGroundOverlay` metodę, która zwróci `GroundOverlay` obiektu. Następujący fragment kodu jest przykładem Dodawanie nakładki podstaw do mapy:

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = _map.AddGroundOverlay(groundOverlayOptions);
```

Poniższy zrzut ekranu przedstawia tej nakładki na mapie:

[![Przykładowa mapa z obrazem nakładany o opatrzone biegunowego](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)


#### <a name="lines-circles-and-polygons"></a>Linie, okręgi i wielokątów

Istnieją trzy typy proste dane geometryczne, które mogą zostać dodane do mapy:

-  **Łamanej** — jest to szereg segmenty linii połączonej. Możesz oznaczyć ścieżki na mapie lub tworzą dowolnym kształcie wymagane.

-  **Wielokąt** -to jest zamknięty kształt znakowania obszarów mapy.

-  **Koło** — spowoduje to narysować okrąg na mapie.



##### <a name="polylines"></a>Linię

A [linię łamaną](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Polyline) znajduje się lista kolejnych `LatLng` obiekty, które Określ wierzchołki każdy segment linii. Łamanej jest tworzony przez utworzenie `PolylineOptions` obiekt i dodanie do niej punkty. `PolylineOption` Obiektu są następnie przekazywane do `GoogleMap` obiektu przez wywołanie metody `AddPolyline` metody.

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.
myMap.AddPolyline(rectOptions);
```


##### <a name="polygons"></a>Wielokąty

`Polygon`s są bardzo podobne do `Polyline`s, ale nie są otwarte została zakończona. `Polygon`s są pętli zamkniętej i mają ich wnętrza wypełnione.
`Polygon`s są tworzone w dokładnie taki sam jak `Polyline`, z wyjątkiem [GoogleMap.AddPolygon](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) wywołana metoda.

W odróżnieniu od `Polyline`, `Polygon` własnym jest zamykany. Gdy `AddPolygon` jest wywołana zostanie metoda automatycznie zamknięte poza wielokąta za pomocą rysowania wiersza, który łączy punkty imię i nazwisko. Poniższy fragment kodu utworzy wypełnionego prostokąta w tym samym obszarze jako poprzedniego fragmentu kodu w `Polyline` przykład.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon
myMap.AddPolygon(rectOptions);
```


##### <a name="circles"></a>Okręgi

Okręgi są tworzone przy uruchamianiu pierwszy [CircleOption](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/CircleOptions) obiektu, który będzie określać Centrum i promień okręgu w m. Okręgu jest rysowana na mapie, wywołując [GoogleMap.AddCircle](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap#addCircle(com.google.android.gms.maps.model.CircleOptions)).
Poniższy fragment kodu przedstawia sposób rysowania koło:

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);
_map.AddCircle (circleOptions);
```


## <a name="responding-to-events"></a>Odpowiadanie na zdarzenia

Istnieją trzy typy interakcji, które użytkownik może mieć z mapą:

-  **Kliknij znacznik** — użytkownik kliknie znacznika.

-  **Przeciągnij znacznika** — użytkownik long kliknął mparger

-  **Informacje o oknie kliknij** — użytkownik kliknie okno informacyjne.

Każde z tych wydarzeń zostanie dokładnie omówione bardziej szczegółowo poniżej.


### <a name="marker-click-events"></a>Znacznik zdarzenia kliknięcia

Gdy użytkownik kliknie znacznik `MarkerClick` zdarzeń zostanie wygenerowany, a `GoogleMap.MarkerClickEventArgs` przekazany. Ta klasa zawiera dwie właściwości:

-  `GoogleMap.MarkerClickEventArgs.Handled` &ndash; Ta właściwość powinna być równa `true` wskazująca, czy program obsługi zdarzeń zużyła zdarzenia. Jeśli ta wartość jest równa `false` , a następnie nastąpi zachowanie domyślne oprócz niestandardowe zachowanie programu obsługi zdarzeń.

-  `P0` &ndash; Ten parametr nazwy nieprawidłowo jest odwołanie do znacznika, która wywołała `MarkerClick` zdarzeń.


Następujący fragment kodu przedstawia przykład `MarkerClick` spowoduje to zmianę położenia kamery do nowej lokalizacji na mapie:

```csharp
private void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;
    Marker marker = markerClickEventArgs.P0;
    if (marker.Id.Equals(MyMarkerId)) // The ID of a specific marker the user clicked on.
    {
        _map.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(new LatLng(20.72110, -156.44776), 13));
    }
    else
    {
        Toast.MakeText(this, String.Format("You clicked on Marker ID {0}", marker.Id), ToastLength.Short).Show();
    }
}
```


### <a name="marker-drag-events"></a>Zdarzenia przeciągania znacznika

To zdarzenie jest wywoływane, gdy użytkownik chce przeciągnij znacznika. Domyślnie znaczników nie są możliwością przeciągania. Znacznik można ustawić jako możliwością przeciągania przez ustawienie `Marker.Draggable` właściwości `true` lub wywołując `MarkerOptions.Draggable` metody z `true` jako parametr.

Najpierw przeciągnij znacznika, użytkownik musi long kliknij go i Zachowaj palca ich na mapie. Podczas przeciągania ich palcem po ekranie, zostanie przesunięty znacznika. Gdy palca użytkownika wind odniosło, znacznika pozostaną w miejscu.

Poniższa lista zawiera opis różnych zdarzeń, które będą wywoływane na potrzeby znacznika możliwością przeciągania:

-   `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; To zdarzenie jest wywoływane, gdy użytkownik przeciąga najpierw znacznika.

-   `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; To zdarzenie jest zgłaszane, ponieważ przeciąganie znacznika.

-   `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; To zdarzenie jest wywoływane podczas kiedy zostanie zakończone przeciąganie znacznika.

Każdy z `EventArgs` zawiera jedną właściwość o nazwie `P0` oznacza to odwołanie do `Marker` obiekt przeciągania.


### <a name="info-window-click-events"></a>Informacje o oknie zdarzenia kliknięcia

Tylko jedno okno informacje mogą być wyświetlane w czasie. Gdy użytkownik kliknie okno informacje na mapie, zgłosi obiektu mapy `InfoWindowClick` zdarzeń. Poniższy fragment kodu przedstawia sposób okablować się obsługi zdarzenia:

```csharp
private bool SetupMapIfNeeded()
{
    if (_map == null)
    {
        _map = _mapFragment.Map;
        if (_map != null)
        {
            _map.InfoWindowClick += MapOnInfoWindowClick;
            return true;
        }
        return false;
    }
    return true;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.P0;
    // Do something with marker.
}
```

Odwołaj, że okno informacje o jest statyczna `View` który jest renderowany jako obraz na mapie. Wszystkie elementy widget, takie jak przyciski, pola wyboru lub tekst widoki, które są umieszczone wewnątrz okno informacje będą pod kątem i nie może odpowiadać na dowolną ich zdarzeń integralną użytkownika.



## <a name="related-links"></a>Linki pokrewne

- [Usług Google Play](http://developer.android.com/google/play-services/index.html)
- [Google mapy interfejsu API systemu Android w wersji 2](https://developers.google.com/maps/documentation/android/)
- [APK usług Google Play](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Uzyskania klucza interfejsu API map Google](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
