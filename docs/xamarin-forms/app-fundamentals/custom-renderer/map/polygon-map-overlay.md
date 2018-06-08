---
title: Wyróżnianie Region na mapie
description: W tym artykule wyjaśniono, jak dodać nakładki wielokąta na mapę, aby wyróżnić region na mapie. Wielokąty są kształt zamknięty i ich wnętrza wypełnione.
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: e0ffa1948bb7dd0996dd21793237df550a32aa70
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848333"
---
# <a name="highlighting-a-region-on-a-map"></a>Wyróżnianie Region na mapie

_W tym artykule wyjaśniono, jak dodać nakładki wielokąta na mapę, aby wyróżnić region na mapie. Wielokąty są kształt zamknięty i ich wnętrza wypełnione._

## <a name="overview"></a>Omówienie

Nakładki jest warstwowego grafiki na mapie. Nakładki obsługuje rysowania graficznej zawartości, która może obsłużyć z planem, ponieważ jest on powiększony. Poniższe zrzuty ekranu pokazują wynikiem dodania nakładki wielokąta do mapy:

![](polygon-map-overlay-images/screenshots.png)

Gdy [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) renderowania formantu przez aplikację platformy Xamarin.Forms w systemie iOS `MapRenderer` tworzenia wystąpienia klasy, która z kolei tworzy natywny `MKMapView` formantu. Na platformie Android `MapRenderer` natywny tworzy wystąpienie klasy `MapView` formantu. W systemie Windows platformy Uniwersalnej, `MapRenderer` natywny tworzy wystąpienie klasy `MapControl`. Proces renderowania można podjąć zaletą do zaimplementowania dostosowań mapy specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla `Map` na każdej z platform. Proces ten wygląda następująco:

1. [Utwórz](#Creating_the_Custom_Map) mapy niestandardowe platformy Xamarin.Forms.
1. [Korzystać z](#Consuming_the_Custom_Map) niestandardowe mapowanie z platformy Xamarin.Forms.
1. [Dostosowywanie](#Customizing_the_Map) mapy przez utworzenie niestandardowego modułu renderowania mapy na każdej z platform.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) musi być zainicjowana i skonfigurowana przed użyciem. Aby uzyskać więcej informacji, zobacz [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Informacje o dostosowywaniu mapy, przy użyciu niestandardowego modułu renderowania, zobacz [Dostosowywanie mapy numer Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Tworzenie niestandardowych mapy

Utwórz podklasę [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) klasy, który dodaje `ShapeCoordinates` właściwości:

```csharp
public class CustomMap : Map
{
  public List<Position> ShapeCoordinates { get; set; }

  public CustomMap ()
  {
    ShapeCoordinates = new List<Position> ();
  }
}
```

`ShapeCoordinates` Właściwości będzie przechowywać kolekcja współrzędnych definiujące regionu do wyróżnione.

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

Ta inicjowania określa szereg współrzędnych współrzędne geograficzne, aby zdefiniować obszaru mapy być wyróżniane. Następnie umieszcza widoku mapy z [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) metodę, która zmienia położenie i poziom powiększenia mapy, tworząc [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) z [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) i [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Dostosowywanie mapy

Niestandardowego modułu renderowania teraz należy dodać do każdego projektu aplikacji, aby dodać nakładki wielokąta mapy.

#### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie modułu renderowania niestandardowe w systemie iOS

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` metody w celu dodania nakładki wielokąta:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolygonRenderer polygonRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polygonRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.ShapeCoordinates.Count];

                int index = 0;
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var blockOverlay = MKPolygon.FromCoordinates(coords);
                nativeMap.AddOverlay(blockOverlay);
            }
        }
        ...
    }
}

```

Ta metoda wykonuje następującą konfigurację, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms:

- `MKMapView.OverlayRenderer` Właściwości ustawiono odpowiedniego obiektu delegowanego.
- Kolekcja współrzędne geograficzne są pobierane z `CustomMap.ShapeCoordinates` właściwości i przechowywane jako tablica `CLLocationCoordinate2D` wystąpień.
- Wielokąt jest tworzony przez wywołanie metody statycznych `MKPolygon.FromCoordinates` metodę, która określa współrzędne geograficzne każdego punktu.
- Wielokąt jest dodawany do mapy przez wywołanie `MKMapView.AddOverlay` metody. Ta metoda jest automatycznie zamykany wielokąta przez rysowanie linii łączącej punkty imię i nazwisko.

Następnie należy zaimplementować `GetOverlayRenderer` metodę w celu dostosowania renderowania nakładki:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolygonRenderer polygonRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polygonRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polygonRenderer = new MKPolygonRenderer(overlay as MKPolygon) {
              FillColor = UIColor.Red,
              StrokeColor = UIColor.Blue,
              Alpha = 0.4f,
              LineWidth = 9
          };
      }
      return polygonRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Tworzenie niestandardowego modułu renderowania w systemie Android

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` i `OnMapReady` metody dodawania nakładki wielokąta:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> shapeCoordinates;

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
                shapeCoordinates = formsMap.ShapeCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polygonOptions = new PolygonOptions();
            polygonOptions.InvokeFillColor(0x66FF0000);
            polygonOptions.InvokeStrokeColor(0x660000FF);
            polygonOptions.InvokeStrokeWidth(30.0f);

            foreach (var position in shapeCoordinates)
            {
                polygonOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }
            NativeMap.AddPolygon(polygonOptions);
        }
    }
}
```

`OnElementChanged` Metoda pobiera zbiór współrzędne geograficzne współrzędnych z `CustomMap.ShapeCoordinates` właściwości i zapisuje je w zmiennej elementu członkowskiego. Następnie wywołuje `MapView.GetMapAsync` metodę, która pobiera odpowiadającego `GoogleMap` który jest powiązany z widoku, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Raz `GoogleMap` wystąpienie jest dostępne, `OnMapReady` można wywołać metody, gdzie wielokąta jest tworzony przez utworzenie wystąpienia `PolygonOptions` obiekt określający współrzędne geograficzne każdego punktu. Wielokąt jest następnie dodany do mapy przez wywołanie metody `NativeMap.AddPolygon` metody. Ta metoda jest automatycznie zamykany wielokąta przez rysowanie linii łączącej punkty imię i nazwisko.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Tworzenie niestandardowego modułu renderowania na platformę uniwersalną systemu Windows

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` metody w celu dodania nakładki wielokąta:

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
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 0, 0, 255);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
    }
}
```

Ta metoda wykonuje następujące operacje, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms:

- Kolekcja współrzędne geograficzne są pobierane z `CustomMap.ShapeCoordinates` właściwości i przekonwertowane na `List` z `BasicGeoposition` współrzędnych.
- Przy uruchamianiu utworzono wielokąta `MapPolygon` obiektu. `MapPolygon` Klasa jest używana do wyświetlania kształtu wielu punktów na mapie, ustawiając jego `Path` właściwości `Geopath` obiekt, który zawiera współrzędne kształtu.
- Wielokąta jest renderowany na mapie, dodając ją do `MapControl.MapElements` kolekcji. Należy pamiętać, że wielokąta zostaną automatycznie zamknięte za pomocą rysowania linii łączącej punkty imię i nazwisko.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak dodać nakładki wielokąta na mapę, aby wyróżnić obszaru mapy. Wielokąty są kształt zamknięty i ich wnętrza wypełnione.


## <a name="related-links"></a>Linki pokrewne

- [Wielokąta mapy nakładki (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [Dostosowywanie pinezki mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
