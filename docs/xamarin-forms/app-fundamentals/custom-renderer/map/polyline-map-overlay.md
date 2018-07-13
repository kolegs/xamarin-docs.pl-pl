---
title: Wyróżnianie trasy na mapie
description: W tym artykule wyjaśniono, jak dodać nakładki linii łamanej na mapie. Nakładka linii łamanej to seria segmenty linii połączonej, które są zazwyczaj używane do wyświetlenia trasy na mapie lub z dowolnego kształtu, który jest wymagany.
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 786f050495d4682b719178f2723c482929544678
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998725"
---
# <a name="highlighting-a-route-on-a-map"></a>Wyróżnianie trasy na mapie

_W tym artykule wyjaśniono, jak dodać nakładki linii łamanej na mapie. Nakładka linii łamanej to seria segmenty linii połączonej, które są zazwyczaj używane do wyświetlenia trasy na mapie lub z dowolnego kształtu, który jest wymagany._

## <a name="overview"></a>Omówienie

Nakładki to warstwowej grafika na mapie. Nakładki obsługuje rysowania graficzny zawartości, która skaluje się z mapą, ponieważ jest on powiększony. Poniższych zrzutach ekranu przedstawiono wynikiem dodania nakładki linii łamanej do mapy:

![](polyline-map-overlay-images/screenshots.png)

Gdy [ `Map` ](xref:Xamarin.Forms.Maps.Map) renderowania formantu przez aplikację platformy Xamarin.Forms w systemie iOS `MapRenderer` tworzenia wystąpienia klasy, która z kolei tworzy macierzystej `MKMapView` kontroli. Na platformie Android `MapRenderer` klasy tworzy macierzystej `MapView` kontroli. Na Universal Windows Platform (platformy UWP), `MapRenderer` klasy tworzy macierzystej `MapControl`. Proces renderowania może podjąć zalet do zaimplementowania dostosowań mapy specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla `Map` na każdej platformie. Proces ten jest w następujący sposób:

1. [Utwórz](#Creating_the_Custom_Map) mapę niestandardowego zestawu narzędzi Xamarin.Forms.
1. [Używanie](#Consuming_the_Custom_Map) Mapa niestandardowa z zestawu narzędzi Xamarin.Forms.
1. [Dostosowywanie](#Customizing_the_Map) mapy przez utworzenie niestandardowego modułu renderowania dla mapy na każdej platformie.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) musi być zainicjowana i skonfigurowana przed użyciem. Aby uzyskać więcej informacji, zobacz [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Aby dowiedzieć się, jak dostosowywanie mapy za pomocą niestandardowego modułu renderowania, zobacz [Dostosowywanie pinezki mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Tworzenie niestandardowych Map

Utwórz podklasę [ `Map` ](xref:Xamarin.Forms.Maps.Map) klasy, która dodaje `RouteCoordinates` właściwości:

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

`RouteCoordinates` Właściwości będą przechowywane w kolekcji współrzędnych definiujące trasy do wyróżnione.

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Korzystanie z niestandardowych Map

Używanie `CustomMap` kontroli deklarując jej wystąpienie w wystąpieniu strony XAML:

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

Alternatywnie używanie `CustomMap` kontroli deklarując jej wystąpienie w wystąpieniu strony C#:

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

Ten proces inicjowania określa szereg współrzędne geograficzne, aby zdefiniować trasy na mapie, aby być wyróżniony. Następnie umieszcza widoku mapy z [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) metody, która zmienia położenie i poziom powiększenia mapy, tworząc [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) z [ `Position` ](xref:Xamarin.Forms.Maps.Position) i [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Dostosowywanie mapy

Niestandardowego modułu renderowania, teraz należy dodać do każdego projektu aplikacji, można dodać nakładki linii łamanej na mapie.

#### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie niestandardowego modułu renderowania w systemie iOS

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` metody w celu dodania nakładki linii łamanej:

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

Ta metoda wykonuje następującą konfigurację, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms:

- `MKMapView.OverlayRenderer` Właściwość jest ustawiona na odpowiednie delegata.
- Kolekcja współrzędne geograficzne są pobierane z `CustomMap.RouteCoordinates` właściwości i przechowywane jako tablicę `CLLocationCoordinate2D` wystąpień.
- Linii łamanej jest tworzony przez wywołanie statycznego `MKPolyline.FromCoordinates` metody, która określa współrzędne geograficzne w każdym punkcie.
- Linii łamanej zostanie dodany do mapy, wywołując `MKMapView.AddOverlay` metody.

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

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` i `OnMapReady` metod dodawania nakładki linii łamanej:

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

`OnElementChanged` Metoda pobiera kolekcję współrzędne geograficzne z `CustomMap.RouteCoordinates` właściwości i zapisuje je w zmiennej składowej. Następnie wywołuje `MapView.GetMapAsync` metody, która pobiera bazowego `GoogleMap` , jest powiązany z widoku, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Raz `GoogleMap` wystąpienie jest dostępne, `OnMapReady` można wywołać metody, gdzie linii łamanej jest tworzony przez utworzenie wystąpienia `PolylineOptions` obiekt określający współrzędne geograficzne w każdym punkcie. Linii łamanej jest dodawane do mapy, wywołując `NativeMap.AddPolyline` metody.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Tworzenie niestandardowego modułu renderowania na platformie Universal Windows

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` metody w celu dodania nakładki linii łamanej:

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

Pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms, ta metoda wykonuje następujące operacje:

- Kolekcja współrzędne geograficzne są pobierane z `CustomMap.RouteCoordinates` właściwości i zostanie przekonwertowana na `List` z `BasicGeoposition` współrzędnych.
- Linii łamanej jest tworzony przez utworzenie wystąpienia `MapPolyline` obiektu. `MapPolygon` Klasa jest używana do wyświetlania linię na mapie, ustawiając jego `Path` właściwość `Geopath` obiekt, który zawiera współrzędne wiersza.
- Linii łamanej jest renderowany na mapie, dodając ją do `MapControl.MapElements` kolekcji.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak dodać nakładki linii łamanej na mapie, aby wyświetlić trasy na mapie lub utworzenia dowolnego kształtu, który jest wymagany.


## <a name="related-links"></a>Linki pokrewne

- [Ovlerlay Mapa linii łamanej (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [Dostosowywanie pinezki mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
