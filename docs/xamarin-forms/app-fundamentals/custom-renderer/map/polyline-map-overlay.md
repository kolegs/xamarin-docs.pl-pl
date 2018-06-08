---
title: Wyróżnianie trasy na mapie
description: W tym artykule opisano sposób dodawania nakładki linię łamaną na mapę. Nakładki łamanej jest serią połączonych segmentów, które zwykle są używane do wyświetlenia trasy na mapie lub z dowolnym kształcie, która jest wymagana.
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 8b80f9569e9377ca76798911cda64d8c0ee28d93
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846646"
---
# <a name="highlighting-a-route-on-a-map"></a>Wyróżnianie trasy na mapie

_W tym artykule opisano sposób dodawania nakładki linię łamaną na mapę. Nakładki łamanej jest serią połączonych segmentów, które zwykle są używane do wyświetlenia trasy na mapie lub z dowolnym kształcie, która jest wymagana._

## <a name="overview"></a>Omówienie

Nakładki jest warstwowego grafiki na mapie. Nakładki obsługuje rysowania graficznej zawartości, która może obsłużyć z planem, ponieważ jest on powiększony. Poniższe zrzuty ekranu pokazują wynikiem dodania nakładki łamanej do mapy:

![](polyline-map-overlay-images/screenshots.png)

Gdy [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) renderowania formantu przez aplikację platformy Xamarin.Forms w systemie iOS `MapRenderer` tworzenia wystąpienia klasy, która z kolei tworzy natywny `MKMapView` formantu. Na platformie Android `MapRenderer` natywny tworzy wystąpienie klasy `MapView` formantu. W systemie Windows platformy Uniwersalnej, `MapRenderer` natywny tworzy wystąpienie klasy `MapControl`. Proces renderowania można podjąć zaletą do zaimplementowania dostosowań mapy specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla `Map` na każdej z platform. Proces ten wygląda następująco:

1. [Utwórz](#Creating_the_Custom_Map) mapy niestandardowe platformy Xamarin.Forms.
1. [Korzystać z](#Consuming_the_Custom_Map) niestandardowe mapowanie z platformy Xamarin.Forms.
1. [Dostosowywanie](#Customizing_the_Map) mapy przez utworzenie niestandardowego modułu renderowania mapy na każdej z platform.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) musi być zainicjowana i skonfigurowana przed użyciem. Aby uzyskać więcej informacji, zobacz [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Informacje o dostosowywaniu mapy, przy użyciu niestandardowego modułu renderowania, zobacz [Dostosowywanie mapy numer Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Tworzenie niestandardowych mapy

Utwórz podklasę [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) klasy, który dodaje `RouteCoordinates` właściwości:

```csharp
public class CustomMap : Map
{
  public List<Position> RouteCoordinates { get; set; }

  public CustomMap ()
  {
    RouteCoordinates = new List<Position> ();
  }
}
```

`RouteCoordinates` Właściwości będzie przechowywać kolekcja współrzędnych definiujące trasy do wyróżnione.

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Korzystanie z niestandardowych mapy

Korzystać z `CustomMap` kontroli przez zadeklarowanie wystąpienia w wystąpieniu strony XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MapOverlay;assembly=MapOverlay"
             x:Class="MapOverlay.MapPage">
    <ContentPage.Content>
        <local:CustomMap x:Name="customMap" MapType="Street" WidthRequest="{x:Static local:App.ScreenWidth}" HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

Można również używać `CustomMap` kontroli przez zadeklarowanie wystąpienia w wystąpieniu strony C#:

```csharp
public class MapPageCS : ContentPage
{
    public MapPageCS ()
    {
        var customMap = new CustomMap {
            MapType = MapType.Street,
            WidthRequest = App.ScreenWidth,
            HeightRequest = App.ScreenHeight
        };
        ...
        Content = customMap;
    }
}
```

Inicjowanie `CustomMap` kontroli zgodnie z wymaganiami:

```csharp
public partial class MapPage : ContentPage
{
  public MapPage ()
  {
    ...
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

Ta inicjowania określa szereg współrzędne geograficzne współrzędnych w celu definiowania trasy na mapie, aby być wyróżniony. Następnie umieszcza widoku mapy z [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) metodę, która zmienia położenie i poziom powiększenia mapy, tworząc [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) z [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) i [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Dostosowywanie mapy

Niestandardowego modułu renderowania teraz należy dodać do każdego projektu aplikacji można dodać nakładki linię łamaną na mapie.

#### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie modułu renderowania niestandardowe w systemie iOS

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` metody w celu dodania nakładki łamanej:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolylineRenderer polylineRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polylineRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.RouteCoordinates.Count];
                int index = 0;
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var routeOverlay = MKPolyline.FromCoordinates(coords);
                nativeMap.AddOverlay(routeOverlay);
            }
        }
        ...
    }
}

```

Ta metoda wykonuje następującą konfigurację, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms:

- `MKMapView.OverlayRenderer` Właściwości ustawiono odpowiedniego obiektu delegowanego.
- Kolekcja współrzędne geograficzne są pobierane z `CustomMap.RouteCoordinates` właściwości i przechowywane jako tablica `CLLocationCoordinate2D` wystąpień.
- Linię łamaną jest tworzony przez wywołanie metody statycznych `MKPolyline.FromCoordinates` metodę, która określa współrzędne geograficzne każdego punktu.
- Linię łamaną jest dodawany do mapy przez wywołanie `MKMapView.AddOverlay` metody.

Następnie należy zaimplementować `GetOverlayRenderer` metodę w celu dostosowania renderowania nakładki:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolylineRenderer polylineRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polylineRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polylineRenderer = new MKPolylineRenderer(overlay as MKPolyline) {
              FillColor = UIColor.Blue,
              StrokeColor = UIColor.Red,
              LineWidth = 3,
              Alpha = 0.4f
          };
      }
      return polylineRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Tworzenie niestandardowego modułu renderowania w systemie Android

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` i `OnMapReady` metody dodawania nakładki łamanej:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> routeCoordinates;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                routeCoordinates = formsMap.RouteCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polylineOptions = new PolylineOptions();
            polylineOptions.InvokeColor(0x66FF0000);

            foreach (var position in routeCoordinates)
            {
                polylineOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }

            NativeMap.AddPolyline(polylineOptions);
        }
    }
}
```

`OnElementChanged` Metoda pobiera zbiór współrzędne geograficzne współrzędnych z `CustomMap.RouteCoordinates` właściwości i zapisuje je w zmiennej elementu członkowskiego. Następnie wywołuje `MapView.GetMapAsync` metodę, która pobiera odpowiadającego `GoogleMap` który jest powiązany z widoku, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Raz `GoogleMap` wystąpienie jest dostępne, `OnMapReady` można wywołać metody, gdzie linii łamanej jest tworzony przez utworzenie wystąpienia `PolylineOptions` obiekt określający współrzędne geograficzne każdego punktu. Linię łamaną następnie dodany do mapy przez wywołanie metody `NativeMap.AddPolyline` metody.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Tworzenie niestandardowego modułu renderowania na platformę uniwersalną systemu Windows

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` metody w celu dodania nakładki łamanej:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MapControl;

                var coordinates = new List<BasicGeoposition>();
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polyline = new MapPolyline();
                polyline.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polyline.StrokeThickness = 5;
                polyline.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polyline);
            }
        }
    }
}
```

Ta metoda wykonuje następujące operacje, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms:

- Kolekcja współrzędne geograficzne są pobierane z `CustomMap.RouteCoordinates` właściwości i przekonwertowane na `List` z `BasicGeoposition` współrzędnych.
- Przy uruchamianiu utworzono linię łamaną `MapPolyline` obiektu. `MapPolygon` Klasa jest używana do wyświetlania wiersza na mapie, ustawiając jego `Path` właściwości `Geopath` obiekt, który zawiera współrzędne wiersza.
- Linię łamaną jest renderowany na mapie, dodając ją do `MapControl.MapElements` kolekcji.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób dodawania nakładki łamanej do mapy, w celu wyświetlenia trasy na mapie lub z dowolnym kształcie, która jest wymagana.


## <a name="related-links"></a>Linki pokrewne

- [Ovlerlay mapy łamanej (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [Dostosowywanie pinezki mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
