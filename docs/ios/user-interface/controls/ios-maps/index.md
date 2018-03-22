---
title: Mapy
description: "iOS obejmuje MapKit mapuje struktury, która ułatwia dodawanie do aplikacji. Za pomocą zestawu mapy, aplikacji systemu iOS można dodać interakcyjne mapy, obsługujące funkcje, takie jak przesuwanie i powiększanie, przedstawiający lokalizacji użytkownika i nakładania sformatowanego grafiki na mapie. W tym artykule delves w szereg funkcji zestawu mapy, pokazujący sposób móc korzystać z nich do tworzenia geograficzne funkcji do aplikacji"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3fecf17a4f70e44ca169c825bf0dd34a5127cec8
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="maps"></a>Mapy

_iOS obejmuje MapKit mapuje struktury, która ułatwia dodawanie do aplikacji. Za pomocą zestawu mapy, aplikacji systemu iOS można dodać interakcyjne mapy, obsługujące funkcje, takie jak przesuwanie i powiększanie, przedstawiający lokalizacji użytkownika i nakładania sformatowanego grafiki na mapie. W tym artykule delves w szereg funkcji zestawu mapy, pokazujący sposób móc korzystać z nich do tworzenia geograficzne funkcji do aplikacji_

Mapy są typową funkcją w nowoczesnych przenośnych systemów operacyjnych. iOS oferuje obsługę mapowania natywnie przez strukturę zestawu mapy. Z zestawem mapy aplikacje można łatwo dodać rozbudowanych, interakcyjnych mapy. Mapy te można dostosować na różne sposoby, takie jak dodawanie adnotacji do oznaczania lokalizacji na mapie i nakładanie grafiki dowolnych kształtów. Zestaw mapy nawet ma wbudowaną obsługę przedstawiający bieżącej lokalizacji urządzenia.

## <a name="adding-a-map"></a>Dodawanie mapy

Dodawanie mapy do aplikacji jest osiągane przez dodanie `MKMapView` wystąpienie do hierarchii widoku, jak pokazano poniżej:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` jest `UIView` podklasy, który wyświetla mapy. Wystarczy dodać mapy, przy użyciu kodu powyżej tworzy mapy interakcyjnej:

 ![](images/00-map.png "Mapa próbki")

## <a name="map-style"></a>Styl mapy

 `MKMapView` obsługuje 3 różnych stylów mapy. Aby zastosować styl mapy, wystarczy ustawić `MapType` właściwości na wartość z `MKMapType` wyliczenie:
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  Poniższy zrzut ekranu Pokaż Mapuj różnych stylów, które są dostępne:

 ![](images/01-mapstyles.png "Mapuj różnych stylów, które są dostępne pokazuj tego zrzutu ekranu")

## <a name="panning-and-zooming"></a>Przesuwać i powiększać

 `MKMapView` obsługuje funkcje interaktywne mapy takich jak:

-  Powiększanie za pośrednictwem gestem uszczypnięcia
-  Przesuwanie za pomocą gestu przesuwanie


Te funkcje mogą być włączone lub wyłączone wystarczy wybrać ustawienie `ZoomEnabled` i `ScrollEnabled` właściwości `MKMapView` wystąpienia, gdzie wartość domyślna to true dla obu. Na przykład aby wyświetlić mapę statyczną, po prostu ustaw odpowiednie właściwości na wartość false:

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Lokalizacja użytkownika

Oprócz interakcji z użytkownikiem `MKMapView` również ma wbudowaną obsługę wyświetlania lokalizacji urządzenia. Robi to przy użyciu *lokalizacji Core* framework. Aby korzystać z lokalizacją użytkownika, musi Monituj użytkownika. W tym celu należy utworzyć wystąpienia `CLLocationManager` i Wywołaj `RequestWhenInUseAuthorization`.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Należy pamiętać, że w wersji systemu iOS przed 8.0, próba wywołania `RequestWhenInUseAuthorization` spowoduje błąd. Upewnij się sprawdzić wersję systemu IOS przed wykonaniem tego połączenia, jeśli zamierzasz obsługuje wersje starsze niż 8.

Uzyskiwanie dostępu do lokalizacji użytkownika wymaga również modyfikacje **Info.plist**. Należy ustawić następujące klucze odnoszące się do danych lokalizacji:

- **NSLocationWhenInUseUsageDescription** — w przypadku gdy uzyskujesz dostęp do lokalizacji użytkownika podczas, gdy są one interakcji z aplikacją.
- **NSLocationAlwaysUsageDescription** — w przypadku gdy aplikacja uzyskuje dostęp do lokalizacji użytkownika w tle.

Można dodać tych kluczy, otwierając **Info.plist** i wybierając *źródła* w dolnej części edytora.

Po zaktualizowaniu **Info.plist** i monituje użytkownika o zezwolenie na dostęp do ich lokalizacji, można wyświetlić lokalizacji użytkownika na mapie, ustawiając `ShowsUserLocation` właściwości na wartość true:

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "Zezwalaj alert dostępu do lokalizacji")
 
## <a name="annotations"></a>Adnotacje

 `MKMapView` Umożliwia wyświetlanie obrazów, znany jako adnotacje na mapie. Mogą to być niestandardowych obrazów lub zdefiniowane w systemie numerów PIN różnych kolorów. Na przykład poniższy zrzut ekranu przedstawia mapy za pomocą obu numeru pin i niestandardowego obrazu:

 ![](images/03-annotations.png "Ten zrzut ekranu przedstawia mapy za pomocą obu numeru pin i niestandardowego obrazu")

### <a name="adding-an-annotation"></a>Dodawanie adnotacji

Adnotacja sam ma dwie części:

-  `MKAnnotation` Obiektu, który zawiera model danych dotyczących adnotacji, takie jak tytuł i lokalizacja adnotacji.
-  `MKAnnotationView` , Który zawiera obraz do wyświetlenia i opcjonalnie objaśnienia, który jest wyświetlany, gdy użytkownik naciska adnotacji.


Zestaw mapy korzysta ze wzorca delegowanie systemu iOS na dodawanie adnotacji do mapy, gdy `Delegate` właściwość `MKMapView` jest ustawione na wystąpienie elementu `MKMapViewDelegate`. Ten delegat implementacji, który jest odpowiedzialny za zwrócenie `MKAnnotationView` dla adnotacji.

Dodawanie adnotacji, najpierw adnotacja została dodana przez wywołanie metody `AddAnnotations` na `MKMapView` wystąpienie:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Gdy lokalizacja adnotacji, staje się widoczne na mapie `MKMapView` wywoła swojego delegata `GetViewForAnnotation` metodę, aby pobrać `MKAnnotationView` do wyświetlenia.

Na przykład następujący kod zwraca dostarczane przez system `MKPinAnnotationView`:

```csharp
string pId = "PinAnnotation";

public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
{
    if (annotation is MKUserLocation)
        return null;

    // create pin annotation view
    MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);

    if (pinView == null)
        pinView = new MKPinAnnotationView (annotation, pId);

    ((MKPinAnnotationView)pinView).PinColor = MKPinAnnotationColor.Red;
    pinView.CanShowCallout = true;

    return pinView;
}
```

### <a name="reusing-annotations"></a>Ponowne użycie adnotacji

W celu zachowywania pamięci, `MKMapView` umożliwia jego widoku adnotacji do grupowane w pulach ponowne, podobnie jak komórek tabeli są używane ponownie. Uzyskiwanie widoku adnotacji z puli wykonuje się za pomocą wywołania `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Wyświetlanie objaśnienia

Jak wspomniano wcześniej, adnotacja może być opcjonalnie wyświetlany objaśnienia. Aby pokazać objaśnienia wystarczy ustawić `CanShowCallout` true na `MKAnnotationView`. Powoduje to tytuł adnotacji wyświetlane adnotacja jest wybrany, jak pokazano:

 ![](images/04-callout.png "Wyświetlany tytuł adnotacji")

### <a name="customizing-the-callout"></a>Dostosowywanie objaśnienie

Objaśnienie również można dostosować, aby wyświetlić widoki po lewej i prawej akcesoriów, jak pokazano poniżej:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Ten kod powoduje zwrócenie następujących objaśnienia:

 ![](images/05-callout-accessories.png "Przykład objaśnienia")

Do obsługi naciśnięcie akcesoriów prawo użytkownika, po prostu zaimplementować `CalloutAccessoryControlTapped` metody w `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Nakładki

Innym sposobem warstwy grafiki na mapie używa nakładki. Nakładki obsługuje rysowania graficznej zawartości, która może obsłużyć z planem, ponieważ jest on powiększony. iOS zapewnia obsługę wielu rodzajów nakładki, w tym:

-  Wielokąty — często używane do Wyróżnij niektórych region na mapie.
-  Linię — często występuje w przypadku przedstawiający trasy.
-  Okręgi - podświetlenie cykliczne obszaru mapy.


Ponadto można tworzyć niestandardowe nakładki pokazanie dowolnego mają geometrię szczegółowe, dostosowane kod rysowania. Na przykład radarowego pogody byłoby odpowiednimi kandydatami do nakładki niestandardowych.

#### <a name="adding-an-overlay"></a>Dodawanie nakładki

Podobnie jak adnotacje, Dodawanie nakładki obejmuje 2 części:

-  Tworzenie obiektu modelu do nakładki i dodać go do `MKMapView` .
-  Tworzenie widoku do nakładki w `MKMapViewDelegate` .


Wzór nakładki może być dowolną `MKShape` podklasy. Obejmuje Xamarin.iOS `MKShape` podklasy wielokąty, linię i okręgi, za pomocą `MKPolygon`, `MKPolyline` i `MKCircle` odpowiednio klasy.

Na przykład poniższy kod służy do dodawania `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Widok do nakładki `MKOverlayView` wystąpienia, który jest zwracany przez `GetViewForOverlay` w `MKMapViewDelegate`. Każdy `MKShape` ma odpowiadające mu `MKOverlayView` wie, w sposób wyświetlania danego kształtu. Aby uzyskać `MKPolygon` jest `MKPolygonView`. Podobnie `MKPolyline` odpowiada `MKPolylineView`oraz `MKCircle` jest `MKCircleView`.

Na przykład poniższy kod zwraca `MKCircleView` dla `MKCircle`:

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

Spowoduje to wyświetlenie okrąg na mapie, jak pokazano:

 ![](images/06-circle-overlay.png "Koło wyświetlone na mapie")

## <a name="local-search"></a>Wyszukiwanie lokalne

iOS obejmuje wyszukiwania lokalnego interfejsu API za pomocą zestawu mapy, który umożliwia asynchroniczne wyszukuje interesujących akcji w wybranym regionie geograficznym.

Aby wykonać wyszukiwanie lokalne, aplikacji, wykonaj następujące kroki:

1.  Utwórz `MKLocalSearchRequest` obiektu.
1.  Utwórz `MKLocalSearch` obiekt z `MKLocalSearchRequest` .
1.  Wywołanie `Start` metoda `MKLocalSearch` obiektu.
1.  Pobrać `MKLocalSearchResponse` obiektu w wywołaniu zwrotnym.


Wyszukiwanie lokalne API sam zapewnia bez interfejsu użytkownika. Nawet nie wymaga mapy do użycia. Jednak aby praktyczne wykorzystanie lokalnego wyszukiwania, aplikacja musi zapewnić sposób określ kwerendę wyszukiwania i wyświetlić wyniki. Ponadto wyniki będą zawierać danych lokalizacji, będzie często rozsądne się okazać widoczne na mapie.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Dodawanie wyszukiwania lokalnego interfejsu użytkownika

Jest jednym ze sposobów wyszukiwania danych wejściowych zaakceptować `UISearchBar`, które jest dostarczane przez `UISearchController` i wyświetli wyniki w tabeli.

Poniższy kod dodaje `UISearchController` (który ma właściwość pasek wyszukiwania) w `ViewDidLoad` metody `MapViewController`:

```csharp
//Creates an instance of a custom View Controller that holds the results
var searchResultsController = new SearchResultsViewController (map);

//Creates a search controller updater
var searchUpdater = new SearchResultsUpdator ();
searchUpdater.UpdateSearchResults += searchResultsController.Search;
            
//add the search controller
searchController = new UISearchController (searchResultsController) {
                SearchResultsUpdater = searchUpdater
            };

//format the search bar
searchController.SearchBar.SizeToFit ();
searchController.SearchBar.SearchBarStyle = UISearchBarStyle.Minimal;
searchController.SearchBar.Placeholder = "Enter a search query";

//the search bar is contained in the navigation bar, so it should be visible
searchController.HidesNavigationBarDuringPresentation = false;

//Ensure the searchResultsController is presented in the current View Controller 
DefinesPresentationContext = true;

//Set the search bar in the navigation bar
NavigationItem.TitleView = searchController.SearchBar;

```csharp
Note that you are responsible for incorporating the search bar object into the user interface. In this example, we assigned it to the TitleView of the navigation bar, but if you do not use a navigation controller in your application you will have to find another place to display it.

In this code snippet, we created another custom view controller – `searchResultsController` –  that displays the search results and then we used this object to create our search controller object. We also created a new search updater, which becomes active when the user interacts with the search bar. It receives notifications about searches with each keystroke and is responsible for updating the UI.
We will take a look at how to implement both the `searchResultsController` and the `searchResultsUpdater` later in this guide.

This results in a search bar displayed over the map as shown below:

 ![](images/07-searchbar.png "A search bar displayed over the map")
 


### Displaying the Search Results

To display search results, we need to create a custom View Controller; normally a `UITableViewController`. As shown above, the `searchResultsController` is passed to the constructor of the `searchController` when it is being created.
The following code is an example of how to create this custom View Controller:

```csharp
public class SearchResultsViewController : UITableViewController
{
    static readonly string mapItemCellId = "mapItemCellId";
    MKMapView map;

    public List<MKMapItem> MapItems { get; set; }

    public SearchResultsViewController (MKMapView map)
    {
        this.map = map;

        MapItems = new List<MKMapItem> ();
    }

    public override nint RowsInSection (UITableView tableView, nint section)
    {
        return MapItems.Count;
    }

    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        var cell = tableView.DequeueReusableCell (mapItemCellId);

        if (cell == null)
            cell = new UITableViewCell ();

        cell.TextLabel.Text = MapItems [indexPath.Row].Name;
        return cell;
    }

    public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
    {
        // add item to map
        CLLocationCoordinate2D coord = MapItems [indexPath.Row].Placemark.Location.Coordinate;
        map.AddAnnotations (new MKPointAnnotation () {
            Title = MapItems [indexPath.Row].Name,
            Coordinate = coord
        });

        map.SetCenterCoordinate (coord, true);

        DismissViewController (false, null);
    }

    public void Search (string forSearchString)
    {
        // create search request
        var searchRequest = new MKLocalSearchRequest ();
        searchRequest.NaturalLanguageQuery = forSearchString;
        searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

        // perform search
        var localSearch = new MKLocalSearch (searchRequest);

        localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
            if (response != null && error == null) {
                this.MapItems = response.MapItems.ToList ();
                this.TableView.ReloadData ();
            } else {
                Console.WriteLine ("local search error: {0}", error);
            }
        });


    }
}
```

### <a name="updating-the-search-results"></a>Aktualizowanie wyniki wyszukiwania

`SearchResultsUpdater` Działa jako łącznika między wewnętrzną siecią `searchController`na pasku wyszukiwania i wyników wyszukiwania. 

W tym przykładzie mamy najpierw utworzyć metody wyszukiwania w `SearchResultsViewController`. Należy utworzyć w tym celu `MKLocalSearch` obiektu i użyć go w celu wystawienia wyszukiwanie `MKLocalSearchRequest`, pobierane są wyniki w wywołaniu zwrotnym przekazany do `Start` metody `MKLocalSearch` obiektu. Wyniki są następnie zwracane w `MKLocalSearchResponse` obiekt zawierający tablicę `MKMapItem` obiektów:

```csharp
public void Search (string forSearchString)
{
    // create search request
    var searchRequest = new MKLocalSearchRequest ();
    searchRequest.NaturalLanguageQuery = forSearchString;
    searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

    // perform search
    var localSearch = new MKLocalSearch (searchRequest);

    localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
        if (response != null && error == null) {
            this.MapItems = response.MapItems.ToList ();
            this.TableView.ReloadData ();
        } else {
            Console.WriteLine ("local search error: {0}", error);
        }
    });


}
```

Następnie w naszym `MapViewController` utworzymy niestandardowej implementacji `UISearchResultsUpdating`, przypisane do `SearchResultsUpdater` właściwość naszych `searchController` w [Dodawanie lokalnego interfejsu użytkownika wyszukiwania](#Adding_a_Local_Search_UI) sekcji:

```csharp
public class SearchResultsUpdator : UISearchResultsUpdating
{
    public event Action<string> UpdateSearchResults = delegate {};

    public override void UpdateSearchResultsForSearchController (UISearchController searchController)
    {
        this.UpdateSearchResults (searchController.SearchBar.Text);
    }
}
```

Wykonania powyższych dodaje adnotacji do mapy po wybraniu elementu w wynikach, jak pokazano poniżej:

 ![](images/08-search-results.png "Adnotacja dodany do mapy po wybraniu elementu w wynikach")
 
> [!IMPORTANT]
> `UISearchController` został wdrożony w systemie iOS 8. Jeśli chcesz wcześniej obsługi urządzeń, a następnie będą musieli używać `UISearchDisplayController`.



## <a name="summary"></a>Podsumowanie

W tym artykule zbadane *mapy* *Kit* dla systemu iOS. Po pierwsze, przeglądał go jak `MKMapView` klasa umożliwia interakcyjne mapy do uwzględnienia w aplikacji. Następnie konieczne wykazanie, jak dostosować za pomocą adnotacji i nakładki mapy. Na koniec ją zbadać możliwości lokalnego wyszukiwania, które zostały dodane do zestawu mapy z systemem iOS, 6.1, przedstawiająca sposób używania wykonywania zapytań na podstawie lokalizacji dla interesujących akcji i dodaj je do mapy.

## <a name="related-links"></a>Linki pokrewne

- [SearchController](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
- [MapDemo (przykład)](https://developer.xamarin.com/samples/monotouch/MapDemo)
