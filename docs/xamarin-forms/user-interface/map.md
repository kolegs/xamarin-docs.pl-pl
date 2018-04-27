---
title: mapy
description: Platformy Xamarin.Forms korzysta natywnych interfejsów API mapy na każdej z platform.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: e296ca79ee03e7fc61532758219b65946a8d4381
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="map"></a>mapy

_Platformy Xamarin.Forms korzysta natywnych interfejsów API mapy na każdej z platform._

Xamarin.Forms.Maps korzysta natywnych interfejsów API mapy na każdej z platform. Zapewnia szybkie i znanych mapy dla użytkowników, ale oznacza, że niektóre kroki konfiguracji są potrzebne do przestrzegania szczególne wymagania interfejsu API każdej platformy.
Po skonfigurowaniu `Map` kontrolować działa tak samo jak element platformy Xamarin.Forms typowy kod.

* [Mapuje inicjowania](#Maps_Initialization) — przy użyciu `Map` wymaga dodatkowy kod inicjujący podczas uruchamiania.
* [Konfiguracja platformy](#Platform_Configuration) -każdej z platform wymaga skonfigurowania map do pracy.
* [W języku C# przy użyciu map](#Using_Maps) — wyświetlanie mapuje i PIN przy użyciu języka C#.
* [Przy użyciu mapy w języku XAML](#Using_Xaml) — wyświetlanie mapy z XAML.

Użyto formantu mapy [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) próbki, które są wyświetlane poniżej.

 [![Mapy w przykładowym MobileCRM](map-images/maps-zoom-sml.png "przykład formantu mapy")](map-images/maps-zoom.png#lightbox "przykład formantu mapy")

Funkcje mapy może zostać poprawione dalsze tworząc [mapy niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>Inicjowanie mapy

Podczas dodawania mapy do aplikacji platformy Xamarin.Forms **Xamarin.Forms.Maps** jest oddzielnego pakietu NuGet, który należy dodać do każdego projektu w rozwiązaniu.
W systemie Android to również ma zależności na GooglePlayServices (NuGet inny) jest automatycznie pobierana podczas dodawania Xamarin.Forms.Maps.

Po zainstalowaniu pakietu NuGet, niektóre kod inicjujący jest wymagany w każdym projekcie aplikacji *po* `Xamarin.Forms.Forms.Init` wywołania metody. Dla systemu iOS można użyć poniższego kodu:

```csharp
Xamarin.FormsMaps.Init();
```

W systemie Android należy przekazać takich samych parametrach co `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Dla systemu Windows platformy Uniwersalnej użyć poniższego kodu:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Dodaj to wywołanie w następujących plikach dla każdej platformy:

-  **iOS** — w pliku AppDelegate.cs `FinishedLaunching` metody.
-  **Android** — w pliku MainActivity.cs `OnCreate` metody.
-  **Platformy uniwersalnej systemu Windows** — w pliku MainPage.xaml.cs `MainPage` konstruktora.

Gdy został dodany pakiet NuGet i wywołać metodę inicjowania wewnątrz każdego applcation `Xamarin.Forms.Maps` interfejsy API mogą być używane w typowy kod PCL lub projektu udostępnionego.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Konfiguracja platformy

Przed mapy będzie wyświetlana na niektórych platformach są wymagane dodatkowe czynności konfiguracyjne.

### <a name="ios"></a>iOS

Aby uzyskać dostęp do lokalizacji usługi w systemie iOS, należy ustawić następujące klucze **Info.plist**:

- iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) — dla przy użyciu usługi lokalizacji, gdy aplikacja jest w użyciu
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) — dla przy użyciu lokalizacji usług przez cały czas
- iOS 10 i wcześniejszych
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) — dla przy użyciu usługi lokalizacji, gdy aplikacja jest w użyciu
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) — dla przy użyciu lokalizacji usług przez cały czas    
    
Aby zapewnić obsługę systemu iOS 11 i starsze wersje, mogą obejmować wszystkie trzy przyciski: `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription`, i `NSLocationAlwaysUsageDescription`.

Reprezentacja XML tych kluczy w **Info.plist** są wyświetlane poniżej. Należy zaktualizować `string` wartości w celu odzwierciedlenia jak aplikacja używa informacji o lokalizacji:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

**Info.plist** wpisy można również dodać w **źródła** widoku w czasie edycji **Info.plist** pliku:

![Info.plist dla systemu iOS 8](map-images/ios8-map-permissions.png "wpisy Info.plist wymagane iOS 8")


### <a name="android"></a>Android

Aby użyć [v2 interfejsu API map Google](https://developers.google.com/maps/documentation/android/) w systemie Android możesz wygenerować klucz interfejsu API i dodaj go do projektu systemu Android.
Postępuj zgodnie z instrukcjami w dokumencie Xamarin [uzyskiwania klucza interfejsu API map Google v2](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
Po wykonaniu tych instrukcji, Wklej klucz interfejsu API w **Properties/AndroidManifest.xml** pliku (Wyświetl źródło i Znajdź lub zaktualizowania następujący element):

```xml
<meta-data
        android:name="com.google.android.geo.API_KEY"
        android:value="YOUR_API_KEY"/>
```

Bez prawidłowego klucza interfejsu API w formancie mapy zostaną wyświetlone jako szare pole w systemie Android.

> [!NOTE]
> Pamiętaj, aby wygenerować inny klucz przy użyciu pliku magazynu kluczy, który jest używany do podpisywania wydanej wersji programu dowolnej aplikacji, która została przekazana do sklepu Google Play. Klucz Generowanie dla rozwoju i debugowanie nie będzie działać i z aplikacji pobranej ze sklepu Google Play zostaną przerwane ekran mapy. Pamiętaj także można ponownie wygenerować klucza if aplikacji **nazwy pakietu** zmiany.

Należy również włączyć odpowiednie uprawnienia, kliknięcie prawym przyciskiem myszy projekt Android i wybierając **Opcje > kompilacji > aplikacji systemu Android** i zaznaczenie następujących czynności:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

Niektóre z nich są przedstawione na poniższym zrzucie ekranu:

![Wymagane uprawnienia dla systemu Android](map-images/android-map-permissions.png "wymagane uprawnienia dla systemu Android")

Ostatnie dwa są wymagane, ponieważ aplikacji wymaga połączenia sieciowego, aby pobrać dane mapy. Przeczytaj informacje o Android [uprawnienia](http://developer.android.com/reference/android/Manifest.permission.html) Aby dowiedzieć się więcej.

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Aby użyć mapy na platformy uniwersalnej systemu Windows należy wygenerować token autoryzacji. Aby uzyskać więcej informacji, zobacz [żądania klucz uwierzytelniania mapy](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) w witrynie MSDN.

Następnie należy określić token uwierzytelniania w `FormsMaps.Init("AUTHORIZATION_TOKEN")` wywołania metody, aby uwierzytelnić aplikację przy użyciu map Bing.

<a name="Using_Maps" />

## <a name="using-maps"></a>Przy użyciu map

Zobacz [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) w przykładowym MobileCRM przykład sposobu użycia formantu mapy w kodzie. Prosty `MapPage` klasy może wyglądać informacja ta — która nowy `MapSpan` utworzeniu pozycja widoku mapy do:

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### <a name="map-type"></a>Typ mapowania

Można także zmienić zawartości mapy przez ustawienie `MapType` właściwości do wyświetlenia mapy ulicy regularne (ustawienie domyślne), obrazów satelitarnych lub obie te grupy.

```csharp
map.MapType == MapType.Street;
```

Nieprawidłowa `MapType` wartości to:

-  Hybrydowe
-  Satelity
-  Ulica (ustawienie domyślne)


### <a name="map-region-and-mapspan"></a>Mapa regionu i MapSpan

Jak pokazano powyżej fragmentu kodu, podając `MapSpan` wystąpienie do konstruktora map Ustawia widok początkowy (center punktu i poziom powiększenia) mapy po załadowaniu go. `MoveToRegion` Metody w klasie mapy można zmienić poziomu pozycji lub powiększenia mapy. Istnieją dwa sposoby tworzenia nowego `MapSpan` wystąpienie:

-  **MapSpan.FromCenterAndRadius()** -statycznej metody do tworzenia zakresu z `Position` i określając `Distance` .
-  **nowe MapSpan ()** — Konstruktor, który używa `Position` i st. szerokości i długości geograficznej do wyświetlenia.


Aby zmienić poziom powiększenia mapy bez zmiany lokalizacji, Utwórz nową `MapSpan` przy użyciu bieżącej lokalizacji z `VisibleRegion.Center` właściwości formantu mapy. A `Slider` może posłużyć do kontroli powiększenia mapy następująco (jednak powiększanie bezpośrednio w formancie mapy obecnie nie można zaktualizować wartości suwaka):

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![Mapy z powiększenia](map-images/maps-zoom-sml.png "powiększenie formantu mapy")](map-images/maps-zoom.png#lightbox "powiększenie formantu mapy")

### <a name="map-pins"></a>Szpilki

Lokalizacje można oznaczyć na mapie z `Pin` obiektów.

```csharp
var position = new Position(37,-122); // Latitude, Longitude
var pin = new Pin {
            Type = PinType.Place,
            Position = position,
            Label = "custom pin",
            Address = "custom detail info"
        };
map.Pins.Add(pin);
```

 `PinType` mogą być ustawione na jedną z następujących wartości, co może negatywnie wpłynąć na sposób numer pin do renderowania (w zależności od platformy):

-  Ogólny
-  Miejsce
-  SavedPin
-  Właściwości SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>Przy użyciu kodu Xaml

Mapy również może być umieszczony w układów Xaml, jak pokazano w ta Wstawka kodu.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
    x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map WidthRequest="320" HeightRequest="200"
            x:Name="MyMap"
            IsShowingUser="true"
            MapType="Hybrid"
        />
    </StackLayout>
</ContentPage>
```

`MapRegion` i `Pins` można ustawić za pomocą kodu `MyMap` odwołania (lub niezależnie od mapy o nazwie). Należy pamiętać, że dodatkowe `xmlns` definicję przestrzeni nazw jest wymagany do odwołują się do formantów Xamarin.Forms.Maps.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

Xamarin.Forms.Maps jest oddzielny NuGet, które muszą zostać dodane do każdego projektu w rozwiązaniu platformy Xamarin.Forms. Dodatkowy kod inicjujący jest wymagana, jak również niektóre kroki konfiguracji dla systemu iOS, Android i platformy uniwersalnej systemu Windows.

Raz skonfigurowanego interfejsu API map może zostać użyty do renderowania map ze znacznikami numeru pin w kilku wierszach kodu. Mapy mogą być dodatkowo dołączane [niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## <a name="related-links"></a>Linki pokrewne

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Mapowanie niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
