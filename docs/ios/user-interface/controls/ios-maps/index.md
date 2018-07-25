---
title: Mapy w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano strukturę systemu iOS w strukturze MapKit i sposobie ich użycia z rozszerzeniem Xamarin.iOS. Omówiono w nim sposób dodawania mapy stylów, Przesuń i powiększania, pracować z lokalizacji użytkownika, dodawać adnotacje, pracować z objaśnieniami i nakładki i wykonania lokalnego wyszukiwania.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 5343f53b77319b08424263103834ffcf10e261a0
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242069"
---
# <a name="maps-in-xamarinios"></a>Mapy w rozszerzeniu Xamarin.iOS

Mapy są typową funkcją w nowoczesnych mobilnych systemów operacyjnych. dla systemu iOS zapewnia obsługę mapowania natywnie za pośrednictwem struktura Mapkit. Za pomocą zestawu mapy aplikacji można łatwo dodawać bogate, interaktywne mapy. Mapy te można dostosować w taki sposób, w na różne sposoby, takie jak dodawanie adnotacji do oznaczania lokalizacji na mapie i nałożenie grafiki dowolnego kształtów. Struktura Mapkit nawet ma wbudowaną obsługę przedstawiający bieżącą lokalizację urządzenia.

## <a name="adding-a-map"></a>Dodawanie mapy

Dodawanie mapy do aplikacji to osiągnąć przez dodanie `MKMapView` wystąpienia do widoku hierarchii, jak pokazano poniżej:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` jest `UIView` podklasy, który wyświetla mapę. Dodanie mapy za pomocą powyższego kodu tworzy mapę interaktywną:

 ![](images/00-map.png "Mapy próbki")

## <a name="map-style"></a>Style mapy

 `MKMapView` obsługuje 3 różnych stylów mapy. Aby zastosować styl mapy, po prostu ustaw `MapType` właściwości na wartość z zakresu od `MKMapType` wyliczenia:
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  Poniższy zrzut ekranu Pokaż style różnych mapy, które są dostępne:

 ![](images/01-mapstyles.png "Ten zrzut ekranu Pokaż style różnych mapy, które są dostępne")

## <a name="panning-and-zooming"></a>Przesuwać i powiększać

 `MKMapView` obsługuje mapy interaktywne funkcje takie jak:

-  Powiększania przez gest uszczypnięcia
-  Przesuwanie za pomocą gestu pan


Te funkcje można włączać lub wyłączać, ustawiając po prostu `ZoomEnabled` i `ScrollEnabled` właściwości `MKMapView` wystąpienia, gdzie wartość domyślna to true dla obu. Na przykład aby wyświetlić mapę statyczną, po prostu ustaw odpowiednie właściwości na wartość false:

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Lokalizacja użytkownika

Oprócz interakcji z użytkownikiem `MKMapView` również ma wbudowaną obsługę wyświetlania lokalizacji urządzenia. Robi to przy użyciu *lokalizacji podstawowej* framework. Zanim można uzyskać dostęp do lokalizacji użytkownika, musi monitować użytkownika. Aby to zrobić, Utwórz wystąpienie `CLLocationManager` i wywołać `RequestWhenInUseAuthorization`.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Należy pamiętać, że w wersji systemu iOS przed 8.0, próba wywołania `RequestWhenInUseAuthorization` spowoduje wystąpienie błędu. Upewnij się sprawdzić wersję systemu iOS przed wprowadzeniem to wywołanie, jeśli zamierzasz obsługiwać wersjach starszych niż 8.

Uzyskiwanie dostępu do lokalizacji użytkownika wymaga również modyfikacje **Info.plist**. Należy ustawić następujące klucze odnoszące się do danych lokalizacji:

- **NSLocationWhenInUseUsageDescription** — w przypadku gdy uzyskujesz dostęp do lokalizacji użytkownika podczas ich wchodzą w interakcje z Twoją aplikacją.
- **NSLocationAlwaysUsageDescription** — w przypadku gdy aplikacja uzyskuje dostęp do lokalizacji użytkownika w tle.

Możesz dodać te klucze, otwierając **Info.plist** i wybierając polecenie *źródła* w dolnej części edytora.

Po zaktualizowaniu **Info.plist** i zostanie wyświetlony monit użytkownika o zezwolenie na dostęp do ich lokalizacji, możesz pokazać od lokalizacji użytkownika na mapie, ustawiając `ShowsUserLocation` właściwości na wartość true:

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "Zezwalaj na alert dostęp do lokalizacji")
 
## <a name="annotations"></a>Adnotacje

 `MKMapView` obsługuje również wyświetlania obrazów, znane jako adnotacje na mapie. Mogą to być obrazy niestandardowe lub zdefiniowane przez system PinY różnych kolorów. Na przykład, poniższy zrzut ekranu przedstawia mapy za pomocą obu numeru pin i obraz niestandardowy:

 ![](images/03-annotations.png "Ten zrzut ekranu przedstawia mapy za pomocą obu numeru pin i obraz niestandardowy")

### <a name="adding-an-annotation"></a>Dodawanie adnotacji

Adnotacja sam ma dwie części:

-  `MKAnnotation` Obiektu, który zawiera model danych dotyczących adnotacji, takie jak tytuł i lokalizacja adnotacji.
-  `MKAnnotationView` , Który zawiera obraz do wyświetlenia i opcjonalnie objaśnienia, który jest wyświetlany, gdy użytkownik naciśnie adnotacji.


Struktura Mapkit używa wzorca delegowania dla systemu iOS do dodawania adnotacji do mapy, gdzie `Delegate` właściwość `MKMapView` jest ustawiona na wystąpienie `MKMapViewDelegate`. Ten delegat implementacji, która jest odpowiedzialna za zwrócenie `MKAnnotationView` adnotacji.

Dodawanie adnotacji, czy najpierw adnotacja została dodana przez wywołanie metody `AddAnnotations` na `MKMapView` wystąpienie:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Gdy lokalizacja adnotacja staje się widoczny na mapie `MKMapView` wywoła jego delegata `GetViewForAnnotation` metodę, aby uzyskać `MKAnnotationView` do wyświetlenia.

Na przykład, poniższy kod zwraca dostarczane przez system `MKPinAnnotationView`:

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

W celu zachowywania pamięci, `MKMapView` umożliwia wyświetlanie adnotacji użytkownika do puli do ponownego wykorzystania, podobnie jak komórki tabeli są używane ponownie. Uzyskiwanie widoku adnotacji z puli jest przeprowadzane za pomocą wywołania `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Wyświetlanie objaśnienia

Jak wspomniano wcześniej, adnotacja też opcjonalnie wyświetlić objaśnienia. Aby wyświetlić objaśnienia po prostu ustaw `CanShowCallout` na wartość TRUE `MKAnnotationView`. Skutkuje to tytuł adnotacji wyświetlane adnotacja jest wybrany, jak pokazano:

 ![](images/04-callout.png "Tytuł adnotacji, są wyświetlane")

### <a name="customizing-the-callout"></a>Dostosowywanie objaśnienie

Objaśnienie można również dostosować do pokazywania widoków po lewej i prawej akcesoriów, jak pokazano poniżej:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Ten kod powoduje zwrócenie następujących objaśnienia:

 ![](images/05-callout-accessories.png "Objaśnienie przykład")

Do obsługi użytkowników, wybierając odpowiednie narzędzie, należy po prostu zaimplementować `CalloutAccessoryControlTapped` method in Class metoda `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Nakładki

Innym sposobem na warstwę grafiki na mapie używa nakładki. Nakładki obsługuje rysowania graficzny zawartości, która skaluje się z mapą, ponieważ jest on powiększony. dla systemu iOS zapewnia obsługę dla kilku typów nakładki, w tym:

-  Wielokąty — często używane, aby wyróżnić niektóre regionu na mapie.
-  Linie łamane — często występuje, gdy są wyświetlane trasy.
-  Kółka — umożliwia wyróżnianie okrągłego obszaru mapy.


Ponadto można utworzyć niestandardowe nakładki do wyświetlenia dowolnych geometrii za pomocą szczegółowe, dostosowany kod rysowania. Na przykład pogoda radarowy będzie dobrym kandydatem do niestandardowych nakładki.

#### <a name="adding-an-overlay"></a>Dodawanie nakładki

Podobnie jak w adnotacji, dodając nakładki obejmuje 2 części:

-  Tworzenie modelu obiektu dla nakładce, a następnie dodanie go do `MKMapView` .
-  Tworzenie widoku na nakładki w `MKMapViewDelegate` .


Model nakładki mogą sytuować się `MKShape` podklasę. Zawiera rozszerzenia Xamarin.iOS `MKShape` podklasy wielokąty, linie łamane i okręgów, za pośrednictwem `MKPolygon`, `MKPolyline` i `MKCircle` klasy odpowiednio.

Na przykład, poniższy kod służy do dodawania `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Widok do nakładki `MKOverlayView` wystąpienia, który jest zwracany przez `GetViewForOverlay` w `MKMapViewDelegate`. Każdy `MKShape` ma odpowiadające mu `MKOverlayView` wie, w sposób wyświetlania danego kształtu. Aby uzyskać `MKPolygon` ma `MKPolygonView`. Podobnie `MKPolyline` odpowiada `MKPolylineView`oraz `MKCircle` ma `MKCircleView`.

Na przykład, poniższy kod zwraca `MKCircleView` dla `MKCircle`:

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

Spowoduje to wyświetlenie koła na mapie, jak pokazano:

 ![](images/06-circle-overlay.png "Okrąg wyświetlanych na mapie")

## <a name="local-search"></a>Wyszukiwanie lokalne

dla systemu iOS zawiera lokalnego wyszukiwania interfejsu API za pomocą mapy Kit, który umożliwia asynchroniczne wyszukiwanie punktów orientacyjnych w określonym regionie geograficznym.

Aby przeprowadzić wyszukiwanie lokalne, aplikacja musi wykonaj następujące kroki:

1.  Utwórz `MKLocalSearchRequest` obiektu.
1.  Tworzenie `MKLocalSearch` obiektu z `MKLocalSearchRequest` .
1.  Wywołaj `Start` metody `MKLocalSearch` obiektu.
1.  Pobieranie `MKLocalSearchResponse` obiektu wywołania zwrotnego.


Wyszukiwanie lokalne sam interfejs API udostępnia bez interfejsu użytkownika. Jeszcze nie wymaga mapa ma być używany. Jednak aby praktyczne wykorzystanie lokalnego wyszukiwania, aplikacja musi zapewnić jakiś sposób, aby określić zapytanie wyszukiwania i wyświetlić wyniki. Ponadto ponieważ wyniki będą zawierać dane lokalizacji, będzie często sensowne Aby wyświetlić je na mapie.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Dodawanie wyszukiwania lokalnego interfejsu użytkownika

Jest jednym ze sposobów, aby akceptować dane wejściowe wyszukiwania `UISearchBar`, które jest dostarczane przez `UISearchController` i wyświetla wyniki w tabeli.

Poniższy kod dodaje `UISearchController` (która ma właściwość pasek wyszukiwania) w `ViewDidLoad` metody `MapViewController`:

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

### <a name="updating-the-search-results"></a>Aktualizowanie wyników wyszukiwania

`SearchResultsUpdater` Działa jako łącznika między wewnętrzną siecią `searchController`na pasku wyszukiwania i wyniki wyszukiwania. 

W tym przykładzie mamy najpierw utworzyć metody wyszukiwania w `SearchResultsViewController`. Firma Microsoft, należy utworzyć w tym celu `MKLocalSearch` obiektu i użyć go do wystawiania wyszukiwanie `MKLocalSearchRequest`, wyniki są pobierane w wywołanie zwrotne przekazane do `Start` metody `MKLocalSearch` obiektu. Wyniki są następnie zwracane w `MKLocalSearchResponse` obiekt zawierający tablicę `MKMapItem` obiektów:

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

Następnie w naszym `MapViewController` utworzymy niestandardową implementację `UISearchResultsUpdating`, która jest przypisana do `SearchResultsUpdater` właściwość naszych `searchController` w [Dodawanie lokalnego interfejsu użytkownika wyszukiwania](#Adding_a_Local_Search_UI) sekcji:

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

Wykonanie powyższych dodaje adnotacji do mapy po wybraniu elementu na liście wyników, jak pokazano poniżej:

 ![](images/08-search-results.png "Adnotacja dodany do mapy, gdy element jest zaznaczony na liście wyników")
 
> [!IMPORTANT]
> `UISearchController` został wdrożony w systemie iOS 8. Jeśli chcesz obsługiwać urządzenia wcześniejsze niż ten, a następnie będą musieli używać `UISearchDisplayController`.



## <a name="summary"></a>Podsumowanie

W tym artykule zbadane *mapy* *Kit* dla systemu iOS. Po pierwsze, wyglądał jak `MKMapView` klasa umożliwia interaktywne mapy, które mają zostać uwzględnione w aplikacji. Następnie pokazano jak dalej dostosowywać mapy za pomocą adnotacje i nakładki. Na koniec badania jej możliwości lokalnego wyszukiwania, które zostały dodane do mapy zestawu z systemem iOS 6.1, przedstawiający sposób użycia wykonywania zapytań na podstawie lokalizacji dla punktów orientacyjnych i dodać je do mapy.

## <a name="related-links"></a>Linki pokrewne

- [SearchController](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [MapDemo (przykład)](https://developer.xamarin.com/samples/monotouch/MapDemo)
