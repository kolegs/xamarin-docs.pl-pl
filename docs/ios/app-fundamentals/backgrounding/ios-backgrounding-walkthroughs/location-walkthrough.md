---
title: Wskazówki — za pomocą lokalizacji tła
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: e6c74d9ffba4f63682a905d6ebc06d02be81abf4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---using-background-location"></a>Wskazówki — za pomocą lokalizacji tła

W tym przykładzie zamierzamy utworzyć iOS aplikację lokalizacji, która wyświetla informacje o naszych bieżącej lokalizacji: szerokości, długości i inne parametry do ekranu. Ta aplikacja będzie pokazują, jak poprawnie przeprowadzania aktualizacji lokalizacji, podczas gdy aplikacja jest aktywna lub Backgrounded.

W tym przewodniku opisano niektóre klucza backgrounding koncepcji, w tym rejestrowanie aplikacji jako aplikacji konieczne tła, wstrzymywanie aktualizacje interfejsu użytkownika, gdy aplikacja jest backgrounded i Praca z `WillEnterBackground` i `WillEnterForeground` `AppDelegate` metody .

## <a name="application-set-up"></a>Konfigurowanie aplikacji


1. Najpierw utwórz nowy **systemu iOS > aplikacji > Aplikacja pojedynczego widoku (C#)**. Wywołać ją _lokalizacji_ i upewnij się, że wybrano zarówno urządzeń iPad i iPhone.

1. Jako aplikacji konieczne tła w systemie iOS kwalifikuje się do lokalizacji aplikacji. Zarejestrować aplikację jako aplikacja lokalizacji, edytując **Info.plist** w pliku projektu.

    W Eksploratorze rozwiązań kliknij dwukrotnie **Info.plist** plik, aby go otworzyć, a następnie przewiń w dół listy. Zaznacz zarówno **Włącz tryby tła** i **aktualizacje lokalizacji** pola wyboru.

    W programie Visual Studio dla komputerów Mac będzie wyglądać mniej więcej tak:

    [![](location-walkthrough-images/image7.png "Zaznacz trybów tła Włącz i pól wyboru lokalizacji aktualizacji")](location-walkthrough-images/image7.png#lightbox)

    W programie Visual Studio **Info.plist** należy zaktualizować ręcznie, dodając następujące parę klucza i wartości:

    ```xml
    <key>UIBackgroundModes</key>
    <array>
      <string>location</string>
    </array>
    ```

1. Teraz, gdy aplikacja jest zarejestrowany, on mógł pobrać lokalizacji danych z urządzenia. W systemie iOS `CLLocationManager` klasa umożliwia dostęp do informacji o lokalizacji i może wywoływać zdarzenia zapewniające aktualizacje lokalizacji.

1. W kodzie, Utwórz nową klasę o nazwie `LocationManager` który udostępnia jedno miejsce na różnych ekranach i kod, aby subskrybować aktualizacje lokalizacji. W `LocationManager` klasy, należy wystąpienie `CLLocationManager` o nazwie `LocMgr`:

    ```csharp
    public class LocationManager
    {
        protected CLLocationManager locMgr;

        public LocationManager () {
            this.locMgr = new CLLocationManager();
            this.locMgr.PausesLocationUpdatesAutomatically = false;

            // iOS 8 has additional permissions requirements
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                locMgr.RequestAlwaysAuthorization (); // works in background
                //locMgr.RequestWhenInUseAuthorization (); // only in foreground
            }

            if (UIDevice.CurrentDevice.CheckSystemVersion (9, 0)) {
                locMgr.AllowsBackgroundLocationUpdates = true;
            }
        }

        public CLLocationManager LocMgr {
            get { return this.locMgr; }
        }
    }
    ```

    Powyższy kod ustawia liczbę właściwości i uprawnień na [CLLocationManager](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/) klasy:

    - `PausesLocationUpdatesAutomatically` — To jest wartość logiczna, która może być ustawiona w zależności od tego, czy system może wstrzymać aktualizacje lokalizacji. Na niektórych urządzeniach domyślnie `true`, co może spowodować urządzenie, aby przestać otrzymywać tła lokalizacji aktualizacje po upływie około 15 minut.
    - `RequestAlwaysAuthorization` -Należy przekazać tę metodę, aby przyznać użytkownikowi aplikacji opcję umożliwiającą programowi lokalizacji będą dostępne w tle. `RequestWhenInUseAuthorization` również mogą być przekazywane, jeśli chcesz dać użytkownikowi opcję umożliwiającą programowi lokalizacji można uzyskać dostęp tylko wtedy, gdy aplikacja działa na pierwszym planie.
    - `AllowsBackgroundLocationUpdates` — To właściwości typu Boolean, wprowadzone w systemie iOS 9, który może być ustawiony na Zezwalaj aplikacji na odbieranie aktualizacji lokalizacji na zawieszone.

    > [!IMPORTANT]
    > System iOS 8 (lub większą) wymaga również wpis w **Info.plist** wyświetlane użytkownikowi jako część żądania autoryzacji pliku.

1. Dodaj klucz `NSLocationAlwaysUsageDescription` lub `NSLocationWhenInUseUsageDescription` z ciągiem, który będzie wyświetlany użytkownikowi w alertu, który żąda dostępu do danych lokalizacji.

1. System iOS 9 wymaga, aby przy użyciu `AllowsBackgroundLocationUpdates` **Info.plist** zawiera klucz `UIBackgroundModes` z wartością `location`. Jeśli po wykonaniu kroku 2 tego przewodnika, powinna już zostały w pliku Info.plist.


1. Wewnątrz `LocationManager` klasy, Utwórz metodę o nazwie `StartLocationUpdates` z następującym kodem. Ten kod pokazuje, jak zacząć otrzymywać aktualizacje lokalizacji z `CLLocationManager`:

    ```csharp
    if (CLLocationManager.LocationServicesEnabled) {
        //set the desired accuracy, in meters
        LocMgr.DesiredAccuracy = 1;
        LocMgr.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) =>
        {
            // fire our custom Location Updated event
            LocationUpdated (this, new LocationUpdatedEventArgs (e.Locations [e.Locations.Length - 1]));
        };
        LocMgr.StartUpdatingLocation();
    }
    ```

    Jest kilka ważnych rzeczy, wykonywane w ramach tej metody. Po pierwsze wykonujemy Sprawdź, czy aplikacja ma dostęp do lokalizacji danych na urządzeniu. Możemy zweryfikować to wywołując `LocationServicesEnabled` na `CLLocationManager`. Ta metoda zwróci **false** Jeśli użytkownik ma zabroniony dostęp aplikacji do informacji o lokalizacji.

1. Następnie wskaż przez program location manager częstotliwość aktualizacji. `CLLocationManager` udostępnia wiele opcji filtrowania i konfigurowanie danych lokalizacji, w tym o częstotliwości aktualizacji. W tym przykładzie ustawiony `DesiredAccuracy` do aktualizowania lokalizacji zmian przez licznika. Aby uzyskać więcej informacji na temat konfigurowania częstotliwości aktualizacji lokalizacji i inne preferencje dotyczą [odwołania do klasy CLLocationManager](http://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) w dokumentacji firmy Apple.

1. Na koniec wywołania `StartUpdatingLocation` na `CLLocationManager` wystąpienia. Ta wartość informuje przez program location manager można pobrać poprawkę początkowej w bieżącej lokalizacji i Rozpocznij wysyłanie aktualizacji

Do tej pory Menedżera lokalizacji został utworzony, skonfigurowano rodzajów danych, firma chce otrzymywać, i stwierdził, że położenie początkowe. Ten kod musi teraz renderowania danych lokalizacji do interfejsu użytkownika. Można to zrobić dzięki zdarzenie niestandardowe, który pobiera `CLLocation` jako argument:

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

Następnym krokiem jest subskrybować aktualizacje lokalizacji z `CLLocationManager`i zgłosi niestandardowego `LocationUpdated` zdarzenie, gdy nowe dane do lokalizacji staje się dostępny, przekazując lokalizacji jako argument. Aby to zrobić, Utwórz nową klasę **LocationUpdateEventArgs.cs**. Ten kod jest dostępny w głównym aplikacji i zwraca lokalizację urządzenia, gdy zdarzenie zostanie wywołane:

```csharp
public class LocationUpdatedEventArgs : EventArgs
{
    CLLocation location;

    public LocationUpdatedEventArgs(CLLocation location)
    {
       this.location = location;
    }

    public CLLocation Location
    {
       get { return location; }
    }
}
```

## <a name="user-interface"></a>Interfejs użytkownika

1. Tworzenie ekranu, która spowoduje wyświetlenie informacji o lokalizacji za pomocą projektanta dla systemu iOS. Kliknij dwukrotnie **Main.storyboard** plik, aby rozpocząć.

    Scenorysu przeciągnij kilka etykiet na ekranie, aby działać jako symbole zastępcze informacji o lokalizacji. W tym przykładzie Brak etykiety dla szerokości, długości, wysokość, kursu i szybkości.

    Układ powinien wyglądać w następujący sposób:

    ![](location-walkthrough-images/image8.png "Przykład układu interfejsu użytkownika w systemie iOS projektanta")

1. W konsoli do rozwiązania, kliknij dwukrotnie `ViewController.cs` plików i edytowanie go w celu utworzenia nowego wystąpienia LocationManager i wywołanie `StartLocationUpdates`na nim.
  Zmień kod, aby wyglądały jak następujące:

    ```csharp
    #region Computed Properties
    public static bool UserInterfaceIdiomIsPhone {
                get { return UIDevice.CurrentDevice.UserInterfaceIdiom == UIUserInterfaceIdiom.Phone; }
            }

    public static LocationManager Manager { get; set;}
    #endregion

    #region Constructors
    public ViewController (IntPtr handle) : base (handle)
    {
    // As soon as the app is done launching, begin generating location updates in the location manager
        Manager = new LocationManager();
        Manager.StartLocationUpdates();
    }

    #endregion
    ```

    Spowoduje to uruchomienie aktualizacje lokalizacji przy uruchamianiu aplikacji, mimo że żadne dane nie zostaną wyświetlone.

1. Teraz, gdy są odbierane aktualizacje lokalizacji, należy zaktualizować ekranie z informacjami o lokalizacji. Następująca metoda pobiera lokalizację, z naszych `LocationUpdated` zdarzeń i wyświetla go w interfejsie użytkownika:

    ```csharp
    #region Public Methods
    public void HandleLocationChanged (object sender, LocationUpdatedEventArgs e)
    {
        // Handle foreground updates
        CLLocation location = e.Location;

        LblAltitude.Text = location.Altitude + " meters";
        LblLongitude.Text = location.Coordinate.Longitude.ToString ();
        LblLatitude.Text = location.Coordinate.Latitude.ToString ();
        LblCourse.Text = location.Course.ToString ();
        LblSpeed.Text = location.Speed.ToString ();

        Console.WriteLine ("foreground updated");
    }
    #endregion
    ```

Nadal trzeba subskrybować `LocationUpdated` zdarzenia w naszych AppDelegate i wywołaj metodę nowych aktualizacji interfejsu użytkownika. Dodaj następujący kod w `ViewDidLoad,` bezpośrednio po `StartLocationUpdates` wywołania:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```


Teraz gdy aplikacja jest uruchamiana, powinien on wyglądać następująco:

[![](location-walkthrough-images/image5.png "Uruchom przykładową aplikację")](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>Obsługa stanów aktywnych i tła

1. Aplikacji jest generowanie lokalizacji aktualizacji, gdy znajduje się na pierwszym planie i aktywne. Aby zademonstrować, co się dzieje, gdy aplikacja przechodzi w tle, należy zastąpić `AppDelegate` zmianie stanu metody, które śledzą aplikacji tak, aby aplikacji podczas jej przechodzi między pierwszego planu i tła zapisuje dane do konsoli:

    ```csharp
    public override void DidEnterBackground (UIApplication application)
    {
        Console.WriteLine ("App entering background state.");
    }

    public override void WillEnterForeground (UIApplication application)
    {
        Console.WriteLine ("App will enter foreground");
    }
    ```

    Dodaj następujący kod w `LocationManager` stale drukowania zaktualizowane lokalizacji danych na dane wyjściowe aplikacji, aby sprawdzić informacje o lokalizacji są nadal dostępne w tle:

    ```csharp
    public class LocationManager
    {
        public LocationManager ()
        {
        ...
        LocationUpdated += PrintLocation;
        }
        ...

        //This will keep going in the background and the foreground
        public void PrintLocation (object sender, LocationUpdatedEventArgs e) {
        CLLocation location = e.Location;
        Console.WriteLine ("Altitude: " + location.Altitude + " meters");
        Console.WriteLine ("Longitude: " + location.Coordinate.Longitude);
        Console.WriteLine ("Latitude: " + location.Coordinate.Latitude);
        Console.WriteLine ("Course: " + location.Course);
        Console.WriteLine ("Speed: " + location.Speed);
        }
    }
    ```

1. Brak jednym pozostałych problemów z kodem: Podjęto próbę zaktualizowania interfejsu użytkownika, gdy aplikacja jest backgrounded iOS Przyczyna będzie zakończy go. Gdy aplikacja przechodzi w tle, kod musi zrezygnować z lokalizacji aktualizacji i Zatrzymaj aktualizowanie interfejsu użytkownika.

    System iOS Wyświetla nam powiadomienia w przypadku aplikacji nastąpi przejście do innej aplikacji stanów. W takim przypadku firma Microsoft może subskrybować `ObserveDidEnterBackground` powiadomień.

    Poniższy fragment kodu przedstawia sposób użycia powiadomienie, aby umożliwić wyświetlanie o tym, kiedy zatrzymanie aktualizacje interfejsu użytkownika. To zostanie umieszczona `ViewDidLoad`:

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    Gdy aplikacja jest uruchomiona, dane wyjściowe będą wyglądać mniej więcej tak:

    ![](location-walkthrough-images/image6.png "Przykład danych wyjściowych lokalizacji w konsoli programu")

1. Aplikacja wyświetla aktualizacje lokalizacji do ekranu działającego na pierwszym planie i drukowanie danych w oknie danych wyjściowych aplikacji podczas pracy w tle w dalszym ciągu.

Pozostaje tylko jeden problem oczekujących: ekranu rozpoczyna się aktualizacje interfejsu użytkownika, gdy aplikacja jest ładowana jako pierwsza, ale go nie ma możliwości wiedzy, gdy aplikacja ponowne wprowadzenie pierwszego planu. Jeśli backgrounded aplikacji jest dostarczana z powrotem na pierwszym planie, nie będzie wznowić aktualizacje interfejsu użytkownika.

Aby rozwiązać ten problem, zagnieździć wywołań uruchomić aktualizacje interfejsu użytkownika wewnątrz innego powiadomienie, które będą uruchamiane, gdy aplikacja przechodzi do stanu aktywnego:

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

Teraz interfejsu użytkownika zostanie rozpoczęta aktualizowania po pierwszym uruchomieniu aplikacji i aktualizowanie aplikacji w dowolnym momencie wznowienia wróci do pierwszego planu.

W tym przewodniku budujemy aplikacji iOS dobrze behaved, background-aware, która wyświetla lokalizację danych do ekranu i w oknie danych wyjściowych aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [Lokalizacja (część 4) (przykład)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Odwołanie do podstawowej lokalizacji platformy](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
