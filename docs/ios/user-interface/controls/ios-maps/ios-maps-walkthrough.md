---
title: "Adnotacje i nakładki"
description: "W tym artykule przedstawiono wskazówki krok po kroku, przedstawiający sposób pracy z funkcjami adnotacji i nakładki zestawu mapy. Widoczny jest sposób dodawania mapy do aplikacji, która wyświetla adnotacji i nakładki w lokalizacji konferencji Xamarin rozwijać 2013."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7eea7231ec4300a368e4612cbed2ba4ebc044a26
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="annotations-and-overlays--walkthrough"></a>Adnotacje i nakładki — wskazówki

Aplikacja, którą spróbujemy kompilacji w tym przewodniku przedstawiono poniżej:

 [![](ios-maps-walkthrough-images/00-map-overlay.png "Przykładową aplikację MapKit")](ios-maps-walkthrough-images/00-map-overlay.png)
 
Kompletny kod w można znaleźć **MapsWalkthroughComplete** folder w [próbka Demo mapy](https://developer.xamarin.com/samples/monotouch/MapDemo/).

Zacznijmy od utworzenia nowego **iOS pusty projekt**i nadanie mu odpowiednią nazwę. Firma Microsoft rozpoczyna się przez dodanie kodu do kontrolera widoku do wyświetlenia MapView, a następnie utworzy nowe klasy dla naszych MapDelegate i adnotacje niestandardowych. Wykonaj poniższe kroki, aby to osiągnąć:

## <a name="viewcontroller"></a>ViewController


1. Dodaj następujące obszary nazw do `ViewController`:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. Dodaj `MKMapView` wystąpienie zmiennej do klasy, wraz z `MapDelegate` wystąpienia. Utworzymy `MapDelegate` wkrótce:

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. Na kontrolerze `LoadView` metody, Dodaj `MKMapView` i ustaw ją na `View` właściwości kontrolera:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    Następnie dodamy kod do zainicjowania mapy w "ViewDidLoad" metoda.

1. W `ViewDidLoad` Dodaj kod, aby ustawić typ mapy, Pokaż lokalizacji użytkownika i zezwolić powiększanie i przesuwanie:

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;
    
    ```

1. Następnie dodaj kod, Wyśrodkuj mapę i ustawić jej regionu:

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;
    
    ```

1. Utwórz nowe wystąpienie klasy `MapDelegate` i przypisz go do `Delegate` z `MKMapView`. Ponownie, wyślemy wiadomość implcodeent `MapDelegate` wkrótce:

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;     
    ```

1. Począwszy od systemu iOS 8 jest powinien żąda autoryzacji od użytkownika, aby użyć ich lokalizacji, więc warto dodać ten element do naszej próbki. Najpierw należy zdefiniować `CLLocationManager` zmiennej na poziomie klasy:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. W `ViewDidLoad` metody, jeśli chcesz Sprawdź, czy urządzenie, na którym uruchomiono aplikację korzysta z systemem iOS 8 i jeśli jest firma Microsoft zażąda autoryzacji, gdy aplikacja jest w użyciu:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
                    locationManager.RequestWhenInUseAuthorization ();
                }
    ```

1. Na koniec należy edytować **Info.plist** pliku w celu poinformowania użytkowników powód żądania ich lokalizacji. W **źródła** menu **Info.plist**, Dodaj następujący klucz:
    
    `NSLocationWhenInUseUsageDescription` 
    
    i parametry: 

    `Maps Demo`.


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs — klasa adnotacji niestandardowych


1. Chcemy używać niestandardowej klasy dla adnotacji o nazwie `ConferenceAnnotation`. Dodaj następujące klasy do projektu:

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;
    
    namespace MapDemo
    {
        public class ConferenceAnnotation : MKAnnotation
        {
            string title;
            CLLocationCoordinate2D coord;
    
            public ConferenceAnnotation (string title,
            CLLocationCoordinate2D coord)
            {
                this.title = title;
                this.coord = coord;
            }
    
            public override string Title {
                get {
                    return title;
                }
            }
    
            public override CLLocationCoordinate2D Coordinate {
                get {
                    return coord;
                }
            }
        }
    }   
    ```

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController — Dodawanie adnotacji i nakładki

1. Z `ConferenceAnnotation` w miejscu możemy dodać go do mapy. W `ViewDidLoad` metody `ViewController`, Dodaj adnotację współrzędną center mapy:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter)); 
    ```

1. Chcemy też mieć nakładką hoteli. Dodaj następujący kod, aby utworzyć `MKPolygon` za pomocą współrzędnych dla hoteli, pod warunkiem i dodać go do mapy przez wywołanie `AddOverlay`:

    ```csharp
    // add an overlay of the hotel
    MKPolygon hotelOverlay = MKPolygon.FromCoordinates(
        new CLLocationCoordinate2D[]{
        new CLLocationCoordinate2D(30.2649977168594, -97.73863627705),
        new CLLocationCoordinate2D(30.2648461170005, -97.7381627734755),
        new CLLocationCoordinate2D(30.2648355402574, -97.7381750192576),
        new CLLocationCoordinate2D(30.2647791309417, -97.7379872505988),
        new CLLocationCoordinate2D(30.2654525150319, -97.7377341711021),
        new CLLocationCoordinate2D(30.2654807195004, -97.7377994819399),
        new CLLocationCoordinate2D(30.2655089239607, -97.7377994819399),
        new CLLocationCoordinate2D(30.2656428950368, -97.738346460207),
        new CLLocationCoordinate2D(30.2650364981811, -97.7385709662122),
        new CLLocationCoordinate2D(30.2650470749025, -97.7386199493406)
    });
    
    map.AddOverlay (hotelOverlay);  
    ```
Na tym kończy się kod w `ViewDidLoad`. Teraz musimy zaimplementować naszych `MapDelegate` klasy do obsługi tworzenia adnotacji i odpowiednio nakładki widoków.


## <a name="mapdelegate"></a>MapDelegate

1. Tworzenie klasy o nazwie `MapDelegate` dziedziczący po `MKMapViewDelegate` i obejmują `annotationId` zmienna do użycia jako identyfikator ponownemu dla adnotacji:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```
    Tylko mamy jedna adnotacja tutaj tak, ponowne użycie kodu nie jest to niezbędne, ale jest dobrym rozwiązaniem będzie uwzględniony.

1. Implementowanie `GetViewForAnnotation` metodę, aby zwrócić widok dla `ConferenceAnnotation` przy użyciu **conference.png** obrazu uwzględnionych w tym przewodniku:

    ```csharp
    public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
    {
        MKAnnotationView annotationView = null;
    
        if (annotation is MKUserLocation)
            return null; 
    
        if (annotation is ConferenceAnnotation) {
    
            // show conference annotation
            annotationView = mapView.DequeueReusableAnnotation (annotationId);
    
            if (annotationView == null)
                annotationView = new MKAnnotationView (annotation, annotationId);
        
            annotationView.Image = UIImage.FromFile ("images/conference.png");
            annotationView.CanShowCallout = true;
        } 
    
        return annotationView;
    }
    ```

1. Po naciśnięciu adnotację, chcemy obraz przedstawiający Miasto Austin. Dodaj następujące zmienne, które mają `MapDelegate` dla obrazu i widok w celu wyświetlenia go:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. Dalej, aby pokazać obrazu adnotacji jest wybrany, należy zaimplementować `DidSelectAnnotation` metody w następujący sposób:

    ```csharp
    public override void DidSelectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // show an image view when the conference annotation view is selected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView = new UIImageView ();
            venueView.ContentMode = UIViewContentMode.ScaleAspectFit;
            venueImage = UIImage.FromFile ("image/venue.png");
            venueView.Image = venueImage;
            view.AddSubview (venueView);
    
            UIView.Animate (0.4, () => {
            venueView.Frame = new CGRect (-75, -75, 200, 200); });
        }
    }
    ```

1. Aby ukryć obrazu, gdy użytkownik odznacza adnotację, wybierając gdziekolwiek na mapie, zaimplementuj `DidSelectAnnotationView` metody w następujący sposób:

    ```csharp
    public override void DidDeselectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // remove the image view when the conference annotation is deselected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView.RemoveFromSuperview ();
            venueView.Dispose ();
            venueView = null;
        }
    }
    ```
    Mamy teraz kodu dla adnotacji w miejscu. Pozostało ma Dodaj kod, aby `MapDelegate` utworzyć widok do nakładki hoteli.

1. Dodaj następujące wykonania `GetViewForOverlay` do `MapDelegate`:

    ```csharp
    public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
    {
        // return a view for the polygon
        MKPolygon polygon = overlay as MKPolygon;
        MKPolygonView polygonView = new MKPolygonView (polygon);
        polygonView.FillColor = UIColor.Blue;
        polygonView.StrokeColor = UIColor.Red;
        return polygonView;
    }
    ```

Uruchom aplikację. Mamy teraz mapy interakcyjnej z adnotacją niestandardową i nakładki! Wybierz adnotację, a obraz Austin jest wyświetlany, jak pokazano poniżej:

 [![](ios-maps-walkthrough-images/01-map-image.png "Wybierz adnotację, i nie jest wyświetlany obraz Austin")](ios-maps-walkthrough-images/01-map-image.png)

## <a name="summary"></a>Podsumowanie

W tym artykule Analizujemy sposób dodawania adnotacji do mapy, jak również sposób dodawania nakładki określonego wielokąta. Firma Microsoft również przedstawiono sposób dodawania obsługi dotykowej do adnotacji do animowania obrazu na mapie.


## <a name="related-links"></a>Linki pokrewne

- [Wersja demonstracyjna mapy (przykład)](https://developer.xamarin.com/samples/monotouch/MapDemo/)
- [Mapy dla systemu iOS](~/ios/user-interface/controls/ios-maps/index.md)
