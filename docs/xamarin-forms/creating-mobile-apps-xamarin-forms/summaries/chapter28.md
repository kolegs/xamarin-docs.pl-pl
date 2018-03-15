---
title: "Podsumowanie działu 28. Lokalizacja i map"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d7a75ce0303030d53315b5e698fc604a910c969c
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Podsumowanie działu 28. Lokalizacja i map

Obsługuje platformy Xamarin.Forms [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) element pochodzący z `View`. Z powodu specjalne wymagania dotyczące platformy związane z użyciem map, zostały wdrożone w osobny zestaw **Xamarin.Forms.Maps**i może dotyczyć innej przestrzeni nazw: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>System współrzędnych geograficznych

Geograficzne współrzędnych identyfikuje pozycji na obiekt kulistego (lub prawie kulistego), takich jak ziemi. Uwzględniono współrzędnych *szerokości geograficznej* i *geograficzne* wyrażone w kątów.

Wywołuje koła `equator` jest pośrodku dwóch biegunów za pomocą których koncepcyjnie rozszerza ziemi osi.

### <a name="parallels-and-latitude"></a>Równoleżników i szerokości geograficznej

Kąt mierzony północ lub południe od równika z Centrum znaczniki ziemi wierszy równa szerokości geograficznej znany jako *równoleżników*. Te zakresu od 0 stopni w równika do 90 stopni na północ i południowo Kijki. Według Konwencji geograficznej na północ od równika są wartości dodatnie i południe od równika są wartości ujemnych.

### <a name="longitude-and-meridians"></a>Długość geograficzna i południków

Połowy okręgi Wielkiej od Bieguna Północnego do południowego są również nazywane wierszy równa długości *południków*. Są to względem głównego południka w Greenwich w Anglii. Według Konwencji szerokości geograficzne wschód od głównego południka są dodatnie wartości od 0 stopni do 180 stopni i szerokości geograficzne zachód od głównego południka ujemne wartości od 0 stopni &ndash;180 stopni.

### <a name="the-equirectangular-projection"></a>Projekcja equirectangular

Wszelkie płaskiej mapy ziemi wprowadza zakłóceń. Jeśli wszystkie wiersze współrzędne geograficzne są proste, a jeśli takie same różnice w kąty współrzędne geograficzne odpowiada równy odległości na mapie, wynik *projekcji equirectangular*. Ta mapa zakłóca obszarów bliżej biegunów, ponieważ są one poziomie tak.

### <a name="the-mercator-projection"></a>Merkatora

Popularne *merkatora* próbuje kompensacji poziome rozciąganie przez także rozciąganie tych obszarów w pionie. Powoduje to gdzie obszary niemal biegunów wyświetlane są znacznie większe niż są rzeczywiście, ale wszystkie lokalne dość ściśle zgodne z bieżącego obszaru mapy.

### <a name="map-services-and-tiles"></a>Mapy usług i kafelków

Mapy usług Użyj odmianą merkatora o nazwie `Web Mercator`. Mapy usług dostarczania kafelków mapy bitowej do klienta na podstawie lokalizacji i powiększenia poziomu.

## <a name="getting-the-users-location"></a>Uzyskiwanie lokalizacji użytkownika

Platformy Xamarin.Forms `Map` klasy nie obejmują możliwość uzyskania lokalizacji geograficznej użytkownika, ale jest to często pożądane podczas pracy z mapy, dlatego usługa zależności musi obsługiwać go.

### <a name="the-location-tracker-api"></a>Śledzący lokalizację interfejsu API

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) rozwiązanie zawiera kod śledzący lokalizację interfejsu API. [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) Struktury hermetyzuje współrzędne geograficzne. [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) Interfejs definiuje dwie metody uruchamiania i wstrzymywania śledzący lokalizację, a zdarzenie po udostępnieniu nowej lokalizacji.

#### <a name="the-ios-location-manager"></a>Menedżer lokalizacji systemu iOS

Implementacja systemu iOS `ILocationTracker` jest [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) klasy, która umożliwia korzystanie z systemu iOS [ `CLLocationManager` ](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/).

#### <a name="the-android-location-manager"></a>Menedżer lokalizacji systemu Android

Implementacja systemu Android `ILocationTracker` jest [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) klasy, która korzysta z Android [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) klasy.

#### <a name="the-windows-runtime-geo-locator"></a>Lokalizator geograficznie środowiska wykonawczego systemu Windows

Implementacja środowiska wykonawczego systemu Windows `ILocationTracker` jest [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) klasy, która korzysta z platformy UWP [ `Geolocator` ](https://msdn.microsoft.com/library/windows/apps/br225534).

### <a name="display-the-phones-location"></a>Wyświetlanie lokalizacji telefonu

[ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) w przykładzie użyto śledzący lokalizację do wyświetlania lokalizacji telefonu, zarówno w tekście, jak i na mapie equirectangular.

### <a name="the-required-overhead"></a>Wymagane koszty

Niektóre czynności jest wymagane dla **WhereAmI** do używania śledzący lokalizację. Pierwsza strona, wszystkie projekty w **WhereAmI** rozwiązanie musi mieć odwołania do odpowiednich projektów w **Xamarin.FormsBook.Platform**i każdego **WhereAmI** projektu należy wywołać `Toolkit.Init` metody.

Niektóre dodatkowe obciążenie specyficzne dla platformy w formie uprawnienia lokalizacji jest wymagany.

#### <a name="location-permission-for-ios"></a>Uprawnienie lokalizacji dla systemu iOS

Dla systemu iOS **info.plist** plik musi zawierać elementy zawierające tekst pytania prośba do użytkownika, aby umożliwić uzyskiwanie lokalizacji użytkownika.

#### <a name="location-permissions-for-android"></a>Uprawnienia lokalizacji dla systemu Android

Aplikacje systemu android, które uzyskują lokalizacji użytkownika musi mieć uprawnienie ACCESS_FILE_LOCATION w pliku AndroidManifest.xml.

#### <a name="location-permissions-for-the-windows-runtime"></a>Uprawnienia lokalizacji dla środowiska uruchomieniowego systemu Windows

Aplikacja systemu Windows lub Windows Phone musi mieć `location` możliwość urządzenia jest oznaczona jako w pliku Package.appxmanifest.

## <a name="working-with-xamarinformsmaps"></a>Praca z Xamarin.Forms.Maps

Kilka wymagań wiążą się z pomocą `Map` klasy.

### <a name="the-nuget-package"></a>Pakiet NuGet

**Xamarin.Forms.Maps** NuGet biblioteki musi zostać dodany do rozwiązania aplikacji. Numer wersji powinna być taka sama jak **platformy Xamarin.Forms** aktualnie zainstalowanym pakietem.

### <a name="initializing-the-maps-package"></a>Inicjowanie pakietu mapy

Projekty aplikacji należy wywołać `Xamarin.FormsMaps.Init` metody po wprowadzeniu wywołania `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>Włączanie mapy usług

Ponieważ `Map` można uzyskać lokalizacji użytkownika, aplikacja musi uzyskać uprawnienia w sposób opisany wcześniej w tym rozdziale:

#### <a name="enabling-ios-maps"></a>Włączanie iOS mapy

Aplikacja systemu iOS przy użyciu `Map` wymaga dwóch wierszy w pliku info.plist.

#### <a name="enabling-android-maps"></a>Włączanie Android mapy

Klucz autoryzacji jest wymagany dla przy użyciu usługi Google mapy. Ten klucz jest wstawiana **AndroidManifest.xml** pliku. Ponadto **AndroidManifest.xml** wymaga pliku `manifest` tagi objętego uzyskiwanie lokalizacji użytkownika.

#### <a name="enabling-windows-runtime-maps"></a>Mapuje Włączanie środowiska uruchomieniowego systemu Windows

Aplikacji środowiska wykonawczego systemu Windows wymaga klucza autoryzacji przy użyciu map Bing. Ten klucz jest przekazywany jako argument `Xamarin.FormsMaps.Init` metody. Aplikacja musi być również włączone dla usługi lokalizacji.

### <a name="the-unadorned-map"></a>Unadorned mapy

[ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) próbka składa się z [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) pliku i [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) plik CodeBehind Umożliwia przejście do różnych programów pokaz.

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) pliku przedstawia sposób wyświetlania [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) widoku. Domyślnie wyświetla Miasto Rzym, ale użytkownik może manipulować mapy.

Aby wyłączyć poziome i pionowe przewijanie, ustaw [ `HasScrollEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasScrollEnabled/) właściwości `false`. Aby wyłączyć powiększanie, ustaw [ `HasZoomEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasZoomEnabled/) do `false`. Te właściwości mogą nie działać na wszystkich platformach.

### <a name="streets-and-terrain"></a>Streets i terenu

Różne typy map można wyświetlić, ustawiając `Map` właściwości [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) typu [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/), wyliczenie z trzech elementów członkowskich:

- [`Street`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Street/), wartość domyślna
- [`Satellite`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Satellite/)
- [`Hybrid`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Hybrid/)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) pliku przedstawia sposób użycia przycisku radiowego, aby wybrać typ mapy. Go wykorzystuje [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) na podstawie biblioteki i klasę [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) pliku.

### <a name="map-coordinates"></a>Współrzędnych mapy

Program można uzyskać bieżącego obszaru który `Map` są wyświetlane przez [ `VisibleRegion` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.VisibleRegion/) właściwości. Ta właściwość jest *nie* obsługiwana przez właściwości możliwej do wiązania, a nie istnieje mechanizm powiadomień wskazująca, kiedy został zmieniony, więc program, który chce monitorować właściwość w tym celu należy używać prawdopodobnie czasomierza.

`VisibleRegion` Typ jest [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/), klasa z czterech właściwości tylko do odczytu:

- [`Center`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Center/) typu [`Position`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)
- [`LatitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LatitudeDegrees/) typu `double`, wskazując wysokość wyświetlenie obszaru mapy
- [`LongitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LongitudeDegrees/) typu `double`, wskazującą szerokość wyświetlenie obszaru mapy
- [`Radius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Radius/) typu [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/), wskazując rozmiar największego obszaru cykliczne widoczne na mapie

`Position` i `Distance` są obu struktur. `Position` definiuje dwie właściwości tylko do odczytu za pomocą [ `Position` konstruktora](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Position.Position/p/System.Double/System.Double/):

- [`Latitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Latitude/)
- [`Longitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Longitude/)

`Distance` mają na celu dostarczenie odległość niezależny od jednostki poprzez konwersję między metryk i jednostki w języku angielskim. A `Distance` wartości można tworzyć na kilka sposobów:

- [`Distance` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Distance.Distance/p/System.Double/) o długości w liczników
- [`Distance.FromMeters`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMeters/p/System.Double/) Metoda statyczna
- [`Distance.FromKilometers`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromKilometers/p/System.Double/) Metoda statyczna
- [`Distance.FromMiles`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMiles/p/System.Double/) Metoda statyczna

Wartość jest dostępna z trzy właściwości:

- [`Meters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Meters/) typu `double`
- [`Kilometers`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Kilometers/) typu `double`
- [`Miles`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Miles/) typu `double`

[MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) plik zawiera kilka `Label` elementy do wyświetlania `MapSpan` informacji. [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) pliku CodeBehind używa czasomierza, aby zachować informacje aktualizowane, ponieważ użytkownik modyfikuje mapy.

### <a name="position-extensions"></a>Pozycja rozszerzeń

Nową bibliotekę książki o nazwie [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) zawiera typy specyficzne dla mapy, ale niezależne od platformy. [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Klasa ma `ToString` metodę `Position`i metodę obliczania odległość między dwoma `Position` wartości.

### <a name="setting-an-initial-location"></a>Ustawienie początkową lokalizację

Możesz wywołać [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion/p/Xamarin.Forms.Maps.MapSpan/) metody `Map` programowo ustawić poziom lokalizacji i powiększenia na mapie. Argument jest typu `MapSpan`. Można utworzyć `MapSpan` przy użyciu jednej z następujących pozycji:

- [`MapSpan` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.MapSpan.MapSpan/p/Xamarin.Forms.Maps.Position/System.Double/System.Double/) z `Position`, a zakres współrzędne geograficzne
- [`MapSpan.FromCenterAndRadius`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius/p/Xamarin.Forms.Maps.Position/Xamarin.Forms.Maps.Distance/) z `Position` i usługi radius

Istnieje również możliwość utworzenia nowego `MapSpan` z istniejącego przy użyciu metod [ `ClampLatitude` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.ClampLatitude/p/System.Double/System.Double/) lub [ `WithZoom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.WithZoom/p/System.Double/).

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) pliku i [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) pliku CodeBehind przedstawiono sposób użycia `MoveToRegion` metodę w celu wyświetlenia stanu Wyoming.

Możesz także użyć [ `Map` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Map.Map/p/Xamarin.Forms.Maps.MapSpan/) z `MapSpan` obiektu zainicjować mapy. [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) pliku pokazano, jak to zrobić w całości w języku XAML do wyświetlenia w Warszawie Xamarin w centrali.

### <a name="dynamic-zooming"></a>Powiększanie dynamiczne

Można użyć `Slider` dynamicznie powiększenie mapy. [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) pliku i [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) pliku CodeBehind pokazują, jak zmienić promień mapy na podstawie `Slider` wartości.

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) pliku i [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) pliku CodeBehind Pokaż alternatywne metody, która działa lepiej w systemie Android, ale żadne rozwiązanie działa dobrze nadaje się do systemu Windows platform.

### <a name="the-phones-location"></a>Lokalizacja telefonu

[ `IsShowingUser` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.IsShowingUser/) Właściwość `Map` działa w sposób nieco inaczej na platformach trzy jako [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) plik demonstruje:

- W systemach iOS niebieska kropka wskazuje lokalizację przez telefon, ale musisz ręcznie przejść istnieje
- W systemie Android, ikona jest wyświetlana, że w przypadku wypychana przenosi mapy do lokalizacji telefonu
- Platformy uniwersalnej systemu Windows jest podobny do systemu iOS, ale czasami automatycznie powoduje przejście do lokalizacji

**MapDemos** projekt próbuje naśladować Android podejście, definiując pierwszego przycisku na podstawie ikony na podstawie [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) pliku i [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) pliku CodeBehind.

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) pliku i [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) pliku CodeBehind ten przycisk służy do przejdź do lokalizacji przez telefon.

### <a name="pins-and-science-museums"></a>Numery PIN i muzea nauki

Na koniec `Map` klasa definiuje [ `Pins` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.Pins/) właściwości typu `IList<Pin>`. [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) Klasy definiuje czterech właściwości:

- [`Label`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Label/) typu `string`, wymagana właściwość
- [`Address`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Address/) typu `string`, opcjonalne adres zrozumiałą dla użytkownika
- [`Position`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Position/) typu `Position`, wskazujące, gdzie numer pin jest wyświetlone na mapie
- [`Type`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Type/) typu [ `PinType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.PinType/), wyliczenie, który nie jest używany

**MapDemos** projekt zawiera plik [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), które wymieniono muzea nauki w Stanach Zjednoczonych i [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) i [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) klasy dla tych danych do deserializacji.

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) pliku i [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) numerów PIN wyświetlania pliku CodeBehind dla tych muzea nauki na mapie. Po naciśnięciu numeru pin, wyświetla adres i witryny sieci Web dla muzeum.

### <a name="the-distance-between-two-points"></a>Odległość między dwoma punktami

[ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Klasa zawiera [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) metody za pomocą uproszczonego obliczania odległość między dwoma lokalizacje geograficzne.

Jest on używany w [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) pliku i [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) pliku CodeBehind również widzieć odległość muzeum lokalizacji użytkownika:

[![Potrójna zrzut ekranu strony muzea lokalnego](images/ch28fg28-small.png "odległość do lokalizacji")](images/ch28fg28-large.png#lightbox "odległości w lokalizacji")

Program również pokazano, jak ograniczyć dynamicznie numer PIN na podstawie lokalizacji mapy.

## <a name="geocoding-and-back-again"></a>Kodowanie geograficzne i z powrotem

[ **Xamarin.Forms.Maps** ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) zawiera także zestaw [ `Geocoder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Geocoder/) klasy z [ `GetPositionsForAddressAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync/p/System.String/) metodę, która konwertuje adres tekst na zero lub więcej możliwości położenia geograficznego, a inna metoda [ `GetAddressesForPositionAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync/p/Xamarin.Forms.Maps.Position/) konwertująca w innym kierunku.

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) pliku i [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) pliku CodeBehind pokazują tej funkcji.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 28 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Przykłady działu 28](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Formant mapy](~/xamarin-forms/user-interface/map.md)
