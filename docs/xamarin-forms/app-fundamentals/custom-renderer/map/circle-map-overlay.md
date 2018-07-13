---
title: Wyróżnianie okrągłego obszaru na mapie
description: W tym artykule wyjaśniono, jak dodać cykliczne nakładki na mapie, aby wyróżnić okrągłego obszaru mapy. Gdy systemów iOS i Android zapewniają interfejsy API umożliwiające dodawanie nakładce cykliczne do mapy, nakładki jest na platformy uniwersalnej systemu Windows renderowane jako wielokąta.
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 3064296d4c78a3342fb27afc971c37a029987e5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998560"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>Wyróżnianie okrągłego obszaru na mapie

_W tym artykule wyjaśniono, jak dodać cykliczne nakładki na mapie, aby wyróżnić okrągłego obszaru mapy._

## <a name="overview"></a>Omówienie

Nakładki to warstwowej grafika na mapie. Nakładki obsługuje rysowania graficzny zawartości, która skaluje się z mapą, ponieważ jest on powiększony. Poniższych zrzutach ekranu przedstawiono wynikiem dodania cykliczne nakładki do mapy:

![](circle-map-overlay-images/screenshots.png)

Gdy [ `Map` ](xref:Xamarin.Forms.Maps.Map) renderowania formantu przez aplikację platformy Xamarin.Forms w systemie iOS `MapRenderer` tworzenia wystąpienia klasy, która z kolei tworzy macierzystej `MKMapView` kontroli. Na platformie Android `MapRenderer` klasy tworzy macierzystej `MapView` kontroli. Na Universal Windows Platform (platformy UWP), `MapRenderer` klasy tworzy macierzystej `MapControl`. Proces renderowania może podjąć zalet do zaimplementowania dostosowań mapy specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla `Map` na każdej platformie. Proces ten jest w następujący sposób:

1. [Utwórz](#Creating_the_Custom_Map) mapę niestandardowego zestawu narzędzi Xamarin.Forms.
1. [Używanie](#Consuming_the_Custom_Map) Mapa niestandardowa z zestawu narzędzi Xamarin.Forms.
1. [Dostosowywanie](#Customizing_the_Map) mapy przez utworzenie niestandardowego modułu renderowania dla mapy na każdej platformie.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) musi być zainicjowana i skonfigurowana przed użyciem. Aby uzyskać więcej informacji zobacz [`Maps Control`](~/xamarin-forms/user-interface/map.md)

Aby dowiedzieć się, jak dostosowywanie mapy za pomocą niestandardowego modułu renderowania, zobacz [Dostosowywanie pinezki mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Tworzenie niestandardowych Map

Tworzenie `CustomCircle` klasy, która ma `Position` i `Radius` właściwości:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

Następnie Utwórz podklasę [ `Map` ](xref:Xamarin.Forms.Maps.Map) klasy, która dodaje właściwość typu `CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

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
    var pin = new Pin {
      Type = PinType.Place,
      Position = new Position (37.79752, -122.40183),
      Label = "Xamarin San Francisco Office",
      Address = "394 Pacific Ave, San Francisco CA"
    };

    var position = new Position (37.79752, -122.40183);
    customMap.Circle = new CustomCircle {
      Position = position,
      Radius = 1000
    };

    customMap.Pins.Add (pin);
    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (position, Distance.FromMiles (1.0)));
  }
}
```

Dodaje ten proces inicjowania [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) i `CustomCircle` wystąpień do niestandardowej mapy i umieszcza widoku mapy z [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) metody, która zmienia położenie i powiększenia poziom mapy, tworząc [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) z [ `Position` ](xref:Xamarin.Forms.Maps.Position) i [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Dostosowywanie mapy

Niestandardowego modułu renderowania, teraz należy dodać do każdego projektu aplikacji, można dodać nakładce cykliczne do mapy.

#### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie niestandardowego modułu renderowania w systemie iOS

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` metody w celu dodania cykliczne nakładki:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKCircleRenderer circleRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    circleRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                var circle = formsMap.Circle;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                var circleOverlay = MKCircle.Circle(new CoreLocation.CLLocationCoordinate2D(circle.Position.Latitude, circle.Position.Longitude), circle.Radius);
                nativeMap.AddOverlay(circleOverlay);
            }
        }
        ...
    }
}

```

Ta metoda wykonuje następującą konfigurację, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms:

- `MKMapView.OverlayRenderer` Właściwość jest ustawiona na odpowiednie delegata.
- Koło jest tworzony przez ustawienie statycznego `MKCircle` obiekt, który określa w metrach środka okręgu i promienia koła.
- Koło zostanie dodany do mapy, wywołując `MKMapView.AddOverlay` metody.

Następnie należy zaimplementować `GetOverlayRenderer` metodę w celu dostosowania renderowania nakładki:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKCircleRenderer circleRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (circleRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          circleRenderer = new MKCircleRenderer(overlay as MKCircle) {
              FillColor = UIColor.Red,
              Alpha = 0.4f
          };
      }
      return circleRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Tworzenie niestandardowego modułu renderowania w systemie Android

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` i `OnMapReady` metod dodawania cykliczne nakładki:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        CustomCircle circle;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Xamarin.Forms.Maps.Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                circle = formsMap.Circle;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var circleOptions = new CircleOptions();
            circleOptions.InvokeCenter(new LatLng(circle.Position.Latitude, circle.Position.Longitude));
            circleOptions.InvokeRadius(circle.Radius);
            circleOptions.InvokeFillColor(0X66FF0000);
            circleOptions.InvokeStrokeColor(0X66FF0000);
            circleOptions.InvokeStrokeWidth(0);

            NativeMap.AddCircle(circleOptions);
        }
    }
}
```

`OnElementChanged` Wywołania metody `MapView.GetMapAsync` metody, która pobiera bazowego `GoogleMap` , jest powiązany z widoku, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Raz `GoogleMap` wystąpienie jest dostępne, `OnMapReady` można wywołać metody, której koło jest tworzony przez utworzenie wystąpienia `CircleOptions` obiekt, który określa w metrach środka okręgu i promienia koła. Koło jest dodawane do mapy, wywołując `NativeMap.AddCircle` metody.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Tworzenie niestandardowego modułu renderowania na platformie Universal Windows

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` metody w celu dodania cykliczne nakładki:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        const int EarthRadiusInMeteres = 6371000;

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
                var circle = formsMap.Circle;

                var coordinates = new List<BasicGeoposition>();
                var positions = GenerateCircleCoordinates(circle.Position, circle.Radius);
                foreach (var position in positions)
                {
                    coordinates.Add(new BasicGeoposition { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
        // GenerateCircleCoordinates helper method (below)
    }
}
```

Pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms, ta metoda wykonuje następujące operacje:

- Położenie okręgu i promienia są pobierane z `CustomMap.Circle` właściwości i przekazywane do `GenerateCircleCoordinates` metody, która generuje szerokości i długości geograficznej współrzędne obwód koła. Poniżej przedstawiono kod dla tej metody pomocnika.
- Współrzędne obwód koła są konwertowane na `List` z `BasicGeoposition` współrzędnych.
- Koło jest tworzony przez utworzenie wystąpienia `MapPolygon` obiektu. `MapPolygon` Klasa jest używana do wyświetlania kształtów wielu punktów na mapie, ustawiając jego `Path` właściwość `Geopath` obiekt, który zawiera współrzędne kształtu.
- Wielokąt jest renderowany na mapie, dodając ją do `MapControl.MapElements` kolekcji.


```
List<Position> GenerateCircleCoordinates(Position position, double radius)
{
    double latitude = position.Latitude.ToRadians();
    double longitude = position.Longitude.ToRadians();
    double distance = radius / EarthRadiusInMeteres;
    var positions = new List<Position>();

    for (int angle = 0; angle <=360; angle++)
    {
        double angleInRadians = ((double)angle).ToRadians();
        double latitudeInRadians = Math.Asin(Math.Sin(latitude) * Math.Cos(distance) + Math.Cos(latitude) * Math.Sin(distance) * Math.Cos(angleInRadians));
        double longitudeInRadians = longitude + Math.Atan2(Math.Sin(angleInRadians) * Math.Sin(distance) * Math.Cos(latitude), Math.Cos(distance) - Math.Sin(latitude) * Math.Sin(latitudeInRadians));

        var pos = new Position(latitudeInRadians.ToDegrees(), longitudeInRadians.ToDegrees());
        positions.Add(pos);
    }

    return positions;
}
```

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak dodać cykliczne nakładki na mapie, aby wyróżnić okrągłego obszaru mapy.


## <a name="related-links"></a>Linki pokrewne

- [Cykliczne Ovlerlay mapy (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [Dostosowywanie pinezki mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
