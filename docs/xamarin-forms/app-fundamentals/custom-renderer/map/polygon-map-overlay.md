---
title: Wyróżnianie regionu na mapie
description: W tym artykule wyjaśniono, jak dodać nakładki wielokąta na mapie, aby zaznaczyć region na mapie. Wielokąty są kształt zamknięty, a ich wnętrza wypełnienie.
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0a11e9c25922531727ad2fee3bbed9c8d4e2b80c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998137"
---
# <a name="highlighting-a-region-on-a-map"></a>Wyróżnianie regionu na mapie

_W tym artykule wyjaśniono, jak dodać nakładki wielokąta na mapie, aby zaznaczyć region na mapie. Wielokąty są kształt zamknięty, a ich wnętrza wypełnienie._

## <a name="overview"></a>Omówienie

Nakładki to warstwowej grafika na mapie. Nakładki obsługuje rysowania graficzny zawartości, która skaluje się z mapą, ponieważ jest on powiększony. Poniższych zrzutach ekranu przedstawiono wynikiem dodania nakładki wielokąta do mapy:

![](polygon-map-overlay-images/screenshots.png)

Gdy [ `Map` ](xref:Xamarin.Forms.Maps.Map) renderowania formantu przez aplikację platformy Xamarin.Forms w systemie iOS `MapRenderer` tworzenia wystąpienia klasy, która z kolei tworzy macierzystej `MKMapView` kontroli. Na platformie Android `MapRenderer` klasy tworzy macierzystej `MapView` kontroli. Na Universal Windows Platform (platformy UWP), `MapRenderer` klasy tworzy macierzystej `MapControl`. Proces renderowania może podjąć zalet do zaimplementowania dostosowań mapy specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla `Map` na każdej platformie. Proces ten jest w następujący sposób:

1. [Utwórz](#Creating_the_Custom_Map) mapę niestandardowego zestawu narzędzi Xamarin.Forms.
1. [Używanie](#Consuming_the_Custom_Map) Mapa niestandardowa z zestawu narzędzi Xamarin.Forms.
1. [Dostosowywanie](#Customizing_the_Map) mapy przez utworzenie niestandardowego modułu renderowania dla mapy na każdej platformie.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) musi być zainicjowana i skonfigurowana przed użyciem. Aby uzyskać więcej informacji, zobacz [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Aby dowiedzieć się, jak dostosowywanie mapy za pomocą niestandardowego modułu renderowania, zobacz [Dostosowywanie pinezki mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Tworzenie niestandardowych Map

Utwórz podklasę [ `Map` ](xref:Xamarin.Forms.Maps.Map) klasy, która dodaje `ShapeCoordinates` właściwości:

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

`ShapeCoordinates` Właściwości będą przechowywane w kolekcji współrzędnych definiujące region być wyróżniane.

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

Ten proces inicjowania określa szereg współrzędne geograficzne, można zdefiniować region mapy, aby być wyróżniony. Następnie umieszcza widoku mapy z [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) metody, która zmienia położenie i poziom powiększenia mapy, tworząc [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) z [ `Position` ](xref:Xamarin.Forms.Maps.Position) i [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Dostosowywanie mapy

Niestandardowego modułu renderowania, teraz należy dodać do każdego projektu aplikacji, aby dodać nakładki wielokąta do mapy.

#### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie niestandardowego modułu renderowania w systemie iOS

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

Ta metoda wykonuje następującą konfigurację, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms:

- `MKMapView.OverlayRenderer` Właściwość jest ustawiona na odpowiednie delegata.
- Kolekcja współrzędne geograficzne są pobierane z `CustomMap.ShapeCoordinates` właściwości i przechowywane jako tablicę `CLLocationCoordinate2D` wystąpień.
- Wielokąt jest tworzony przez wywołanie statycznego `MKPolygon.FromCoordinates` metody, która określa współrzędne geograficzne w każdym punkcie.
- Wielokąt jest dodawana do mapy, przez wywołanie `MKMapView.AddOverlay` metody. Ta metoda automatycznie zamyka wielokąta za pomocą rysowania linii łączącej punkty imię i nazwisko.

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

Utwórz podklasę `MapRenderer` klasy i zastąp jego `OnElementChanged` i `OnMapReady` metod dodawania nakładki wielokąta:

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

`OnElementChanged` Metoda pobiera kolekcję współrzędne geograficzne z `CustomMap.ShapeCoordinates` właściwości i zapisuje je w zmiennej składowej. Następnie wywołuje `MapView.GetMapAsync` metody, która pobiera bazowego `GoogleMap` , jest powiązany z widoku, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Raz `GoogleMap` wystąpienie jest dostępne, `OnMapReady` można wywołać metody, gdzie Wielokąt jest tworzony przez utworzenie wystąpienia `PolygonOptions` obiekt określający współrzędne geograficzne w każdym punkcie. Wielokąt jest dodawane do mapy, wywołując `NativeMap.AddPolygon` metody. Ta metoda automatycznie zamyka wielokąta za pomocą rysowania linii łączącej punkty imię i nazwisko.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Tworzenie niestandardowego modułu renderowania na platformie Universal Windows

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

Pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms, ta metoda wykonuje następujące operacje:

- Kolekcja współrzędne geograficzne są pobierane z `CustomMap.ShapeCoordinates` właściwości i zostanie przekonwertowana na `List` z `BasicGeoposition` współrzędnych.
- Wielokąt jest tworzony przez utworzenie wystąpienia `MapPolygon` obiektu. `MapPolygon` Klasa jest używana do wyświetlania kształtów wielu punktów na mapie, ustawiając jego `Path` właściwość `Geopath` obiekt, który zawiera współrzędne kształtu.
- Wielokąt jest renderowany na mapie, dodając ją do `MapControl.MapElements` kolekcji. Należy pamiętać, że wielokąta zostaną automatycznie zamknięte za pomocą rysowania linii łączącej punkty imię i nazwisko.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak dodać nakładki wielokąta na mapie, aby wyróżnić obszaru mapy. Wielokąty są kształt zamknięty, a ich wnętrza wypełnienie.


## <a name="related-links"></a>Linki pokrewne

- [Wielokątów mapy nakładki (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [Dostosowywanie pinezki mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
