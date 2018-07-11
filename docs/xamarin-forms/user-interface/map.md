---
title: Mapa zestawu narzędzi Xamarin.Forms
description: W tym artykule opisano sposób użycia klasy Xamarin.Forms Map użyć mapy natywnych interfejsów API na każdej platformie, aby zapewnić powszechnie znane mapy środowisko dla użytkowników.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: d74ad52a2926fb30a528aeba29156259390c3edf
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947247"
---
# <a name="xamarinforms-map"></a>Mapa zestawu narzędzi Xamarin.Forms

_Zestaw narzędzi Xamarin.Forms używa mapy natywnych interfejsów API na każdej platformie._

Projekt xamarin.Forms.Maps dla używa natywnego mapy interfejsów API na każdej platformie. Zapewnia szybkie i dobrze znana mapy dla użytkowników, ale oznacza, że niektóre kroki konfiguracji są potrzebne stosować się do określonych wymagań API każdej platformy.
Po skonfigurowaniu `Map` działania, podobnie jak inny element zestawu narzędzi Xamarin.Forms w kodzie wspólnej kontroli.

* [Mapuje inicjowania](#Maps_Initialization) — przy użyciu `Map` wymaga dodatkowy kod inicjujący przy uruchamianiu.
* [Konfiguracja platformy](#Platform_Configuration) — każdej z platform wymaga konfiguracji dla map do pracy.
* [W języku C# przy użyciu map](#Using_Maps) — wyświetlanie mapy i Przypina przy użyciu języka C#.
* [Przy użyciu map w XAML](#Using_Xaml) — wyświetlanie mapy z XAML.

Formant mapy został użyty w [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) próbki, które znajdują się poniżej.

 [![Mapy w przykładzie MobileCRM](map-images/maps-zoom-sml.png "przykład kontrolki mapy")](map-images/maps-zoom.png#lightbox "przykład kontrolki mapy")

Funkcji mapy może dalszych Zwiększ swoje możliwości, tworząc [mapowanie niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>Inicjowanie mapy

Podczas dodawania mapy do aplikacji platformy Xamarin.Forms **projekt xamarin.Forms.Maps dla** jest oddzielny pakiet NuGet, który należy dodać do każdego projektu w rozwiązaniu.
W systemie Android to również zależny od GooglePlayServices (NuGet inny), który jest pobierany automatycznie, gdy dodajesz projekt xamarin.Forms.Maps dla.

Po zainstalowaniu pakietu NuGet, niektóre kod inicjujący jest wymagany w każdym projekcie aplikacji *po* `Xamarin.Forms.Forms.Init` wywołania metody. Użyj poniższego kodu, dla systemu iOS:

```csharp
Xamarin.FormsMaps.Init();
```

W systemie Android musisz przekazać te same parametry jako `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Dla uniwersalnej platformy Windows (UWP), użyj następującego kodu:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Dodaj to wywołanie w następujących plikach dla każdej platformy:

-  **iOS** — w pliku AppDelegate.cs `FinishedLaunching` metody.
-  **Android** — w pliku MainActivity.cs `OnCreate` metody.
-  **Platformy uniwersalnej systemu Windows** — w pliku MainPage.xaml.cs `MainPage` konstruktora.

Po został dodany pakiet NuGet i wywołana metoda inicjowania wewnątrz każdego applcation `Xamarin.Forms.Maps` interfejsy API mogą być używane w typowych projekt biblioteki .NET Standard lub kod projektu udostępnionego.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Konfiguracja platformy

Dodatkowe czynności konfiguracyjne są wymagane na niektórych platformach, zanim spowoduje wyświetlenie mapy.

### <a name="ios"></a>iOS

Dostęp do lokalizacji usług w systemie iOS, należy ustawić następujące klucze w **Info.plist**:

- System iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) — dotyczące korzystania z usług lokalizacyjnych, gdy aplikacja jest w użyciu
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) — dotyczące korzystania z usług lokalizacyjnych przez cały czas
- System iOS 10 i wcześniejszych
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) — dotyczące korzystania z usług lokalizacyjnych, gdy aplikacja jest w użyciu
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) — dotyczące korzystania z usług lokalizacyjnych przez cały czas    

Aby obsługiwać system iOS 11 lub starszej, mogą obejmować wszystkie trzy przyciski: `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription`, i `NSLocationAlwaysUsageDescription`.

Reprezentacja XML tych kluczy w **Info.plist** znajdują się poniżej. Należy zaktualizować `string` wartości, aby odzwierciedlić, jak Twoja aplikacja używa informacji o lokalizacji:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

**Info.plist** wpisy mogą być również dodawane w **źródła** widoku podczas edytowania **Info.plist** pliku:

![Plik info.plist dla systemu iOS 8](map-images/ios8-map-permissions.png "wpisy Info.plist wymagane systemy iOS 8")


### <a name="android"></a>Android

Aby użyć [v2 interfejsu API usługi mapy Google](https://developers.google.com/maps/documentation/android/) w systemie Android możesz wygenerować klucz interfejsu API i dodać go do projektu systemu Android.
Postępuj zgodnie z instrukcjami w dokumentacji platformy Xamarin na [uzyskiwanie klucza interfejsu API usługi mapy Google v2](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
Po wykonaniu tych instrukcji, Wklej klucz interfejsu API w **Properties/AndroidManifest.xml** pliku (Wyświetl źródło i Znajdź/Zaktualizuj następujący element):

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

Bez prawidłowego klucza interfejsu API kontrolki mapy będą wyświetlane jako szary pole w systemie Android.

> [!NOTE]
> Pamiętaj, że, aby APK usługi Google Maps dostęp do, możesz musi obejmują odcisków palców SHA-1 i pakietu nazwy dla każdego keystore (debug i release), którego używasz do logowania się użytkownika pliku APK. Na przykład jeśli jeden komputer jest używany do debugowania i inny komputer w celu generowania wersji pliku APK, możesz powinna zawierać odcisk palca certyfikatu SHA-1 z magazynu kluczy debugowania pierwszego komputera i odcisk palca certyfikatu SHA-1 z magazynu kluczy wersji programu drugi komputer. Pamiętaj również, edycji poświadczenia klucza, jeśli aplikacja **nazwy pakietu** zmiany. Zobacz [uzyskiwanie klucza interfejsu API usługi mapy Google v2](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

Należy także włączyć odpowiednie uprawnienia, kliknij prawym przyciskiem myszy na projekt systemu Android i wybierając **Opcje > kompilacji > Aplikacja dla systemu Android** i jak należy następujące czynności:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

W poniższym zrzucie ekranu przedstawiono niektóre z nich:

![Wymagane uprawnienia dla systemu Android](map-images/android-map-permissions.png "wymagane uprawnienia dla systemu Android")

Ostatnie dwa są wymagane, ponieważ aplikacji wymaga połączenia sieciowego, aby pobrać dane mapy. Przeczytaj o Android [uprawnienia](http://developer.android.com/reference/android/Manifest.permission.html) Aby dowiedzieć się więcej.

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Aby używać mapy na platformie Universal Windows należy wygenerować token autoryzacji. Aby uzyskać więcej informacji, zobacz [poprosić o klucz uwierzytelniania mapy](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) w witrynie MSDN.

Następnie należy określić token uwierzytelniania w `FormsMaps.Init("AUTHORIZATION_TOKEN")` wywołania metody, do uwierzytelniania aplikacji z usługą mapy Bing.

<a name="Using_Maps" />

## <a name="using-maps"></a>Przy użyciu map

Zobacz [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) w przykładzie MobileCRM, na przykład jak używać kontrolki mapy w kodzie. Prosty `MapPage` klasy może wyglądać jak ten — powiadomienie, nowy `MapSpan` utworzeniu pozycja widoku mapy do:

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

Można także zmienić zawartość mapy, ustawiając `MapType` właściwości, aby pokazać regularne mapa drogowa (ustawienie domyślne), obrazów satelitarnych lub jako kombinację obu tych.

```csharp
map.MapType == MapType.Street;
```

Nieprawidłowa `MapType` wartości to:

-  Hybrydowe
-  Satelity
-  Ulica (ustawienie domyślne)


### <a name="map-region-and-mapspan"></a>Mapa regionów i MapSpan

Jak pokazano w powyższym fragmencie kodu, podając `MapSpan` wystąpienia do konstruktora mapy Ustawia widok początkowy (Centrum punktu i poziom powiększenia) mapy podczas jego ładowania. `MoveToRegion` Metody w klasie mapy następnie można zmienić na poziomie pozycji lub powiększenia mapy. Istnieją dwa sposoby, aby utworzyć nowy `MapSpan` wystąpienie:

-  **MapSpan.FromCenterAndRadius()** — metody statycznej, aby utworzyć zakres z `Position` i określając `Distance` .
-  **nowe MapSpan ()** — Konstruktor, który używa `Position` i st. szerokości i długości geograficznej do wyświetlenia.


Aby zmienić poziom powiększenia mapy bez zmiany ich lokalizacji, Utwórz nowy `MapSpan` przy użyciu bieżącej lokalizacji z `VisibleRegion.Center` właściwości formantu mapy. Element `Slider` może służyć do kontrolowania powiększenia mapy następująco (jednak powiększanie bezpośrednio w formancie mapy aktualnie nie można zaktualizować wartość suwaka):

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![Mapy z powiększeniem](map-images/maps-zoom-sml.png "Powiększ formant mapy")](map-images/maps-zoom.png#lightbox "powiększenie kontrolki mapy")

### <a name="map-pins"></a>Mapy kodów PIN

Może być oznaczony lokalizacji na mapie za pomocą `Pin` obiektów.

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

 `PinType` można ustawić jedną z następujących wartości, które mogą mieć wpływ na sposób numer pin do renderowania (w zależności od platformy):

-  Ogólny
-  Miejsce
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>Przy użyciu języka Xaml

Mapy również może być umieszczony w Xaml układów, jak pokazano w tym fragmencie kodu.

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

`MapRegion` i `Pins` można ustawić przy użyciu kodu `MyMap` odwołania (lub niezależnie od rodzaju mapy o nazwie). Należy pamiętać, że dodatkowe `xmlns` definicję przestrzeni nazw jest wymagany do odwołują się do formantów projekt xamarin.Forms.Maps dla.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

Projekt xamarin.Forms.Maps dla jest oddzielnym NuGet, który musi zostać dodany do każdego projektu w rozwiązaniu Xamarin.Forms. Dodatkowy kod inicjujący jest wymagane, jak również niektórych czynności konfiguracyjnych dla systemów iOS, Android i platformy uniwersalnej systemu Windows.

Po skonfigurowanym interfejsu API map, może zostać użyty do renderowania mapy ze znacznikami numeru pin w kilku wierszach kodu. Mapy mogą być dodatkowo dołączane [niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## <a name="related-links"></a>Linki pokrewne

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Mapowanie niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
