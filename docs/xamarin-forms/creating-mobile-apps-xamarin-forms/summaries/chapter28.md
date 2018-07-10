---
title: Podsumowanie rozdziałów 28. Lokalizacja i mapy
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziałów 28. Lokalizacja i mapy'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c10a3c1a0ed2755734fe351df39caadc88dd61c4
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935132"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Podsumowanie rozdziałów 28. Lokalizacja i mapy

Obsługuje platformy Xamarin.Forms [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) element, który pochodzi od klasy `View`. Ze względu na wymagania specjalne platformy, związane z użyciem map, są one wykonywane w osobnym zestawie **projekt xamarin.Forms.Maps dla**i może dotyczyć innej przestrzeni nazw: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Współrzędnych geograficznych

System współrzędnych geograficznych identyfikuje pozycji w obiekcie kulistego (lub prawie sferyczną), takich jak ziemi. Współrzędna składa się z obu *latitude* i *długości geograficznej* wyrażone w kąty.

Wywoływana najkrótszej `equator` jest pośrodku dwóch biegunów za pomocą których koncepcyjnie rozszerza ziemi osi.

### <a name="parallels-and-latitude"></a>Równoleżniki i szerokości geograficznej

Kąt mierzony na północ lub południe od równika z Centrum znaczniki ziemi wierszy równą szerokości geograficznej, znane jako *parallels*. Te w zakresie od 0 stopni na równika do 90 stopni północ i południowo-biegunów. Zgodnie z Konwencją geograficznej na północ od równika są wartości dodatnich i na południe od równika są wartości ujemnych.

### <a name="longitude-and-meridians"></a>Długość geograficzna i południki

Połówki okręgów great od Bieguna Północnego do południowego wierszy równa długości geograficznej, są również nazywane *południków*. Są to względem południków Prime w Greenwich w Anglii. Zgodnie z Konwencją szerokości geograficzne wschód od południków iloczyn wartości dodatnich z 0 stopni do 180 stopni i szerokości geograficzne południka są wartości ujemnych z 0 stopni &ndash;180 stopni.

### <a name="the-equirectangular-projection"></a>Projekcja equirectangular

Wszelkie prostego mapy ziemi wprowadza zakłóceń. Jeśli wszystkie wiersze szerokości i długości geograficznej są proste, a jeśli równy różnice w kąty szerokości i długości geograficznej odpowiadają równe odległości na mapie, wynik jest *projekcji equirectangular*. To mapowanie zniekształci obszarów bliżej biegunów, ponieważ są one poziomo rozciągnięte.

### <a name="the-mercator-projection"></a>Projekcja Mercator

Popularne *projekcji Mercator* próbuje kompensuje poziomy rozciąganie rozciągając również tych obszarów w pionie. Skutkuje to gdzie obszary niemal biegunów wyświetlane są znacznie większe niż są rzeczywiście, ale wszystkie lokalne dość dobrze jest zgodny z bieżącego obszaru mapy.

### <a name="map-services-and-tiles"></a>Mapy usługi i Kafelki

Mapy usługi używają typu projekcji Mercator o nazwie `Web Mercator`. Mapy usług dostarczania kafelków mapy bitowej do klienta na podstawie lokalizacji i powiększenia poziomu.

## <a name="getting-the-users-location"></a>Uzyskiwanie lokalizacji użytkownika

Xamarin.Forms `Map` klasy nie obejmują funkcji można uzyskać lokalizacji geograficznej użytkownika, ale często jest pożądane podczas pracy z mapy, dlatego usługa zależności należy go obsłużyć.

### <a name="the-location-tracker-api"></a>Śledzący lokalizację interfejsu API

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) rozwiązanie zawiera kod śledzący lokalizację interfejsu API. [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) Struktury hermetyzuje szerokości i długości geograficznej. [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) Interfejs definiuje dwie metody uruchamianie i zatrzymywanie śledzący lokalizację i zdarzenie po udostępnieniu nowej lokalizacji.

#### <a name="the-ios-location-manager"></a>Przez program location manager dla systemu iOS

Implementacja systemu iOS `ILocationTracker` jest [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) klasę, która sprawia, że korzystanie z systemu iOS [ `CLLocationManager` ](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/).

#### <a name="the-android-location-manager"></a>Menedżer lokalizacji dla systemu Android

Implementacja systemu Android `ILocationTracker` jest [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) klasę, która korzysta z programu Android [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) klasy.

#### <a name="the-windows-runtime-geo-locator"></a>Lokalizator geograficznie Windows Runtime

Implementacja Windows Runtime `ILocationTracker` jest [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) klasę, która korzysta z platformy UWP [ `Geolocator` ](https://msdn.microsoft.com/library/windows/apps/br225534).

### <a name="display-the-phones-location"></a>Wyświetlanie lokalizacji na telefonie

[ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) próbki do wyświetlania lokalizacji telefonu, zarówno w tekście, jak i na mapie equirectangular śledzący lokalizację.

### <a name="the-required-overhead"></a>Wymagane czynności

Pewne nadmiarowe obciążenie jest wymagany dla **WhereAmI** używać śledzący lokalizację. Pierwszy, wszystkie projekty w **WhereAmI** rozwiązanie musi mieć odwołania do odpowiednich projektów w **Xamarin.FormsBook.Platform**, a każdy **WhereAmI** projektu należy wywołać `Toolkit.Init` metody.

Niektóre dodatkowe obciążenie specyficzne dla platformy w formie uprawnienia do lokalizacji, jest wymagana.

#### <a name="location-permission-for-ios"></a>Uprawnienie lokalizacji dla systemu iOS

W przypadku systemu iOS **info.plist** plik musi zawierać elementy zawierające tekst zapytania z pytaniem do użytkownika i zezwolić na pobieranie lokalizacji tego użytkownika.

#### <a name="location-permissions-for-android"></a>Uprawnienia do lokalizacji dla systemu Android

Aplikacje systemu android, które uzyskiwanie lokalizacji użytkownika musi mieć uprawnienie ACCESS_FILE_LOCATION w pliku AndroidManifest.xml.

#### <a name="location-permissions-for-the-windows-runtime"></a>Uprawnienia do lokalizacji dla środowiska uruchomieniowego Windows

Aplikacja Windows lub Windows Phone, musi mieć `location` możliwość urządzenia jest oznaczona jako w plik manifestu Package.appx.

## <a name="working-with-xamarinformsmaps"></a>Praca z projekt xamarin.Forms.Maps dla

Kilka wymagań dotyczących wiążą się z pomocą `Map` klasy.

### <a name="the-nuget-package"></a>Pakiet NuGet

**Projekt xamarin.Forms.Maps dla** NuGet biblioteki musi zostać dodany do rozwiązania aplikacji. Numer wersji powinna być taka sama jak **Xamarin.Forms** aktualnie zainstalowany pakiet.

### <a name="initializing-the-maps-package"></a>Inicjowanie pakietu mapy

Projekty aplikacji musi wywołać `Xamarin.FormsMaps.Init` wywołania do metody `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>Włączanie mapy usług

Ponieważ `Map` można uzyskać od lokalizacji użytkownika, aplikacja musi uzyskać uprawnienia dla użytkownika w sposób opisany wcześniej w tym rozdziale:

#### <a name="enabling-ios-maps"></a>Włączanie systemu iOS mapy

Aplikacja systemu iOS przy użyciu `Map` wymaga dwóch wierszy w pliku info.plist.

#### <a name="enabling-android-maps"></a>Włączanie Android mapy

Klucz autoryzacji jest wymagane do używania usługi mapy Google. Ten klucz jest wstawiana w **AndroidManifest.xml** pliku. Ponadto **AndroidManifest.xml** plik wymaga `manifest` tagi zaangażowanych w uzyskiwaniu lokalizacji użytkownika.

#### <a name="enabling-windows-runtime-maps"></a>Włączanie środowiska uruchomieniowego Windows mapy

Aplikacja Windows Runtime wymaga klucza autoryzacji przy użyciu map Bing. Ten klucz jest przekazywany jako argument do `Xamarin.FormsMaps.Init` metody. Aplikacja musi być także włączona dla usługi lokalizacji.

### <a name="the-unadorned-map"></a>Unadorned mapy

[ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) próbka składa się z [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) pliku i [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) plik CodeBehind Umożliwia przechodzenie do różnych programów demonstracyjnych.

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) plik pokazuje sposób wyświetlania [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) widoku. Domyślnie wyświetla Miasto Rzym, ale mapy mogą być zmieniane przez użytkownika.

Aby wyłączyć poziome i pionowe przewijanie, ustaw [ `HasScrollEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasScrollEnabled/) właściwość `false`. Aby wyłączyć, powiększanie, ustaw [ `HasZoomEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasZoomEnabled/) do `false`. Te właściwości mogą nie działać na wszystkich platformach.

### <a name="streets-and-terrain"></a>Mapa Streets i terenu

Możesz wyświetlić różnego rodzaju mapy, ustawiając `Map` właściwość [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) typu [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/), wyliczenie z trzema elementami członkowskimi:

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street), wartość domyślna
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) plik pokazuje, jak i użyj przycisku radiowego, aby wybrać typ mapy. Jego sprawia, że użycie [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) na podstawie biblioteki i klasa [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) pliku.

### <a name="map-coordinates"></a>Współrzędne mapy

Program można uzyskać bieżącego obszaru, `Map` jest wyświetlana za pośrednictwem [ `VisibleRegion` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.VisibleRegion/) właściwości. Ta właściwość jest *nie* wspierana przez właściwości możliwej do wiązania, i nie ma żadnych mechanizm powiadomień, aby wskazać, kiedy został zmieniony, dlatego program, który chce monitorować właściwość prawdopodobnie może używać czasomierza do tego celu.

`VisibleRegion` Typ jest [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/), klasa z czterech właściwości tylko do odczytu:

- [`Center`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Center/) tego typu [`Position`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)
- [`LatitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LatitudeDegrees/) typu `double`, wskazując wysokość wyświetlenie obszaru mapy
- [`LongitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LongitudeDegrees/) typu `double`, wskazującą szerokość wyświetlanego obszaru mapy
- [`Radius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Radius/) typu [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/), wskazując rozmiaru największego obszaru cykliczne widoczne na mapie

`Position` i `Distance` są obu struktur. `Position` definiuje dwie właściwości tylko do odczytu za pomocą [ `Position` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Position.Position/p/System.Double/System.Double/):

- [`Latitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Latitude/)
- [`Longitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Longitude/)

`Distance` mają na celu dostarczenie odległość niezależne od jednostki za pomocą konwersji między metryk i jednostek w języku angielskim. A `Distance` wartości można tworzyć na kilka sposobów:

- [`Distance` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Distance.Distance/p/System.Double/) z odległość w metrach
- [`Distance.FromMeters`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMeters/p/System.Double/) Metoda statyczna
- [`Distance.FromKilometers`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromKilometers/p/System.Double/) Metoda statyczna
- [`Distance.FromMiles`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMiles/p/System.Double/) Metoda statyczna

Wartość jest dostępna w trzech właściwości:

- [`Meters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Meters/) tego typu `double`
- [`Kilometers`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Kilometers/) tego typu `double`
- [`Miles`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Miles/) tego typu `double`

[MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) plik zawiera kilka `Label` elementów do wyświetlania `MapSpan` informacji. [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) pliku związanego z kodem korzysta z czasomierza, aby zachować je zaktualizować, ponieważ użytkownik modyfikuje mapy.

### <a name="position-extensions"></a>Pozycja rozszerzenia

Nową biblioteką dla tę książkę o nazwie [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) zawiera typy specyficzne dla mapy, ale niezależne od platformy. [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Klasa ma `ToString` metodę `Position`i metodę, aby obliczyć odległość między dwoma `Position` wartości.

### <a name="setting-an-initial-location"></a>Ustawienie początkowa lokalizacja:

Możesz wywołać [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion/p/Xamarin.Forms.Maps.MapSpan/) metody `Map` Aby programowo ustawić poziom lokalizacji i powiększania na mapie. Argument jest typu `MapSpan`. Możesz utworzyć `MapSpan` przy użyciu jednej z następujących pozycji:

- [`MapSpan` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.MapSpan.MapSpan/p/Xamarin.Forms.Maps.Position/System.Double/System.Double/) z `Position`oraz zakres szerokości i długości geograficznej
- [`MapSpan.FromCenterAndRadius`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius/p/Xamarin.Forms.Maps.Position/Xamarin.Forms.Maps.Distance/) za pomocą `Position` i usługi radius

Istnieje również możliwość utworzenia nowego `MapSpan` z istniejącą przy użyciu metod [ `ClampLatitude` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.ClampLatitude/p/System.Double/System.Double/) lub [ `WithZoom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.WithZoom/p/System.Double/).

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) pliku i [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) pliku związanego z kodem pokazuje sposób użycia `MoveToRegion` metodę, aby wyświetlić stan Wyoming.

Możesz też użyć [ `Map` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Map.Map/p/Xamarin.Forms.Maps.MapSpan/) z `MapSpan` obiekt, aby zainicjować lokalizacji mapy. [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) plik pokazuje, jak to zrobić w XAML, aby wyświetlić centrali platformy Xamarin w San Francisco w całości.

### <a name="dynamic-zooming"></a>Dynamiczne powiększanie

Możesz użyć `Slider` można dynamicznie powiększać mapę. [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) pliku i [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) pliku związanego z kodem pokazują jak zmienić radius mapy na podstawie `Slider` wartości.

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) pliku i [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) pliku związanego z kodem zawierać alternatywne podejście, który sprawdzi się najlepiej w systemie Android, ale nie sprawdza się w Windows platform.

### <a name="the-phones-location"></a>Lokalizacja na telefonie

[ `IsShowingUser` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.IsShowingUser/) Właściwość `Map` nieco zmieniło się na trzech platformach, co [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) pokazuje pliku:

- W systemach iOS niebieska kropka wskazuje lokalizację przez telefon, ale musisz ręcznie przejść istnieje
- W systemie Android jest wyświetlana ikona że w przypadku wypychania przenosi mapę do lokalizacji na telefonie
- Platformy uniwersalnej systemu Windows jest podobny do systemu iOS, ale czasami automatycznie powoduje przejście do lokalizacji

**MapDemos** projekt próbuje naśladować Android podejście definiując pierwszego przycisku na podstawie ikony na podstawie [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) pliku i [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) pliku CodeBehind.

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) pliku i [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) pliku związanego z kodem Użyj tego przycisku, aby przejść do lokalizacji na telefonie.

### <a name="pins-and-science-museums"></a>Numery PIN i muzeów nauki

Na koniec `Map` klasa definiuje [ `Pins` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.Pins/) właściwości typu `IList<Pin>`. [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) Klasa definiuje cztery właściwości:

- [`Label`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Label/) typu `string`, wymagana właściwość
- [`Address`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Address/) typu `string`, opcjonalny adres czytelny dla człowieka
- [`Position`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Position/) typu `Position`, wskazujące, gdzie numer pin jest wyświetlany na mapie
- [`Type`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Type/) typu [ `PinType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.PinType/), wyliczenia, który nie jest używany

**MapDemos** projekt zawiera plik [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), który zawiera listę muzeów analizy w Stanach Zjednoczonych i [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) i [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) klasy dla tych danych do deserializacji.

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) pliku i [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) związanym z kodem plik wyświetlania numerów PIN te muzeów nauki na mapie. Kiedy użytkownik naciska numeru pin, wyświetla adres i witryny sieci Web do muzeów.

### <a name="the-distance-between-two-points"></a>Odległość między dwoma punktami

[ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Klasa zawiera [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) metody za pomocą uproszczonego obliczania odległość między dwoma lokalizacjach geograficznych.

Jest on używany w [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) pliku i [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) pliku związanego z kodem, które mają być również wyświetlane odległość muzeów lokalizacji użytkownika:

[![Potrójna zrzut ekranu strony muzeów lokalnego](images/ch28fg28-small.png "odległość do lokalizacji")](images/ch28fg28-large.png#lightbox "odległość do lokalizacji")

Program ilustruje też sposób dynamicznie ograniczyć liczbę kodów PIN, na podstawie lokalizacji mapy.

## <a name="geocoding-and-back-again"></a>Geokodowanie i z powrotem

[ **Projekt xamarin.Forms.Maps dla** ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) zawiera także zestaw [ `Geocoder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Geocoder/) klasy [ `GetPositionsForAddressAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync/p/System.String/) metodę, która konwertuje adres tekstu, zero lub więcej możliwych położenia geograficznego i inną metodę [ `GetAddressesForPositionAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync/p/Xamarin.Forms.Maps.Position/) która umożliwia przekształcenie w innym kierunku.

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) pliku i [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) pliku związanego z kodem zademonstrowania tej funkcji.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 28 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Przykłady działu 28](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Kontrolki mapy](~/xamarin-forms/user-interface/map.md)
