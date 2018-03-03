---
title: "Wyróżnianie cykliczne obszar na mapie"
description: "W tym artykule opisano sposób dodawania cykliczne nakładki na mapę, aby wyróżnić cykliczne obszaru mapy."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0eef31c5b9a93154b1038ffa63ee560bd738fe6b
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="highlighting-a-circular-area-on-a-map"></a>Wyróżnianie cykliczne obszar na mapie

_W tym artykule opisano sposób dodawania cykliczne nakładki na mapę, aby wyróżnić cykliczne obszaru mapy._

## <a name="overview"></a>Omówienie

Nakładki jest warstwowego grafiki na mapie. Nakładki obsługuje rysowania graficznej zawartości, która może obsłużyć z planem, ponieważ jest on powiększony. Poniższe zrzuty ekranu pokazują wynikiem dodania nakładki cykliczne do mapy:

![](circle-map-overlay-images/screenshots.png)

Gdy [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) renderowania formantu przez aplikację platformy Xamarin.Forms w systemie iOS `MapRenderer` tworzenia wystąpienia klasy, która z kolei tworzy natywny `MKMapView` formantu. Na platformie Android `MapRenderer` natywny tworzy wystąpienie klasy `MapView` formantu. W systemie Windows platformy Uniwersalnej, `MapRenderer` natywny tworzy wystąpienie klasy `MapControl`. Proces renderowania można podjąć zaletą do zaimplementowania dostosowań mapy specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla `Map` na każdej z platform. Proces ten wygląda następująco:

1. [Utwórz](#Creating_the_Custom_Map) mapy niestandardowe platformy Xamarin.Forms.
1. [Korzystać z](#Consuming_the_Custom_Map) niestandardowe mapowanie z platformy Xamarin.Forms.
1. [Dostosowywanie](#Customizing_the_Map) mapy przez utworzenie niestandardowego modułu renderowania mapy na każdej z platform.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/") musi być zainicjowana i skonfigurowana przed użyciem. Aby uzyskać więcej informacji zobacz [`Maps Control`](~/xamarin-forms/user-interface/map.md)

Informacje o dostosowywaniu mapy, przy użyciu niestandardowego modułu renderowania, zobacz [Dostosowywanie mapy numer Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Tworzenie niestandardowych mapy

Utwórz `CustomCircle` klasy, która ma `Position` i `Radius` właściwości:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

Następnie Utwórz podklasę [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) klasy, który dodaje właściwość typu `CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

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

Dodaje ten inicjowania [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) i `CustomCircle` wystąpień do mapy niestandardowe i umieszcza widoku mapy z [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) metodę, która zmienia położenie i powiększenia poziom mapy, tworząc [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) z [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) i [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Dostosowywanie mapy

Niestandardowego modułu renderowania teraz należy dodać do każdego projektu aplikacji, aby dodać nakładce cykliczne do mapy.

#### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie modułu renderowania niestandardowe w systemie iOS

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` metody w celu dodania nakładce cykliczne:

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

Ta metoda wykonuje następującą konfigurację, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms:

- `MKMapView.OverlayRenderer` Właściwości ustawiono odpowiedniego obiektu delegowanego.
- Okręgu jest tworzony przez ustawienie statycznego `MKCircle` obiekt określający środka okręgu i promień okręgu liczników.
- Okręgu jest dodawany do mapy przez wywołanie `MKMapView.AddOverlay` metody.

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

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` i `OnMapReady` metody dodawania nakładce cykliczne:

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

`OnElementChanged` Wywołania metody `MapView.GetMapAsync` metodę, która pobiera odpowiadającego `GoogleMap` który jest powiązany z widoku, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Raz `GoogleMap` wystąpienie jest dostępne, `OnMapReady` można wywołać metody, gdzie okręgu jest tworzony przez utworzenie wystąpienia `CircleOptions` obiekt określający środka okręgu i promień okręgu liczników. Okręgu następnie dodany do mapy przez wywołanie metody `NativeMap.AddCircle` metody.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Tworzenie niestandardowego modułu renderowania na platformę uniwersalną systemu Windows

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` metody w celu dodania nakładce cykliczne:

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
        ...
    }
}
```

Ta metoda wykonuje następujące operacje, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms:

- Pozycja koło i radius są pobierane z `CustomMap.Circle` właściwości i przekazywane do `GenerateCircleCoordinates` metodę, która generuje współrzędne geograficzne współrzędne obwodowej okręgu.
- Współrzędne obwodowej koło są konwertowane na `List` z `BasicGeoposition` współrzędnych.
- Przy uruchamianiu utworzono okręgu `MapPolygon` obiektu. `MapPolygon` Klasa jest używana do wyświetlania kształtu wielu punktów na mapie, ustawiając jego `Path` właściwości `Geopath` obiekt, który zawiera współrzędne kształtu.
- Wielokąta jest renderowany na mapie, dodając ją do `MapControl.MapElements` kolekcji.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak dodać cykliczne nakładki na mapę, aby wyróżnić cykliczne obszaru mapy.


## <a name="related-links"></a>Linki pokrewne

- [Cykliczne Ovlerlay mapy (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [Dostosowywanie pinezki mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
