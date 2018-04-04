---
title: Nowe funkcje w MapKit w systemie iOS 11
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 3b1ac8015a86292f00f26133277ead2f211dca33
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="new-features-in-mapkit-on-ios-11"></a>Nowe funkcje w MapKit w systemie iOS 11

iOS 11 dodaje następujące nowe funkcje do MapKit:

- [Adnotacja klastra](#clustering)
- [Kompas przycisku](#compass)
- [Widok skali](#scale)
- [Przycisk śledzenia użytkownika](#user-tracking)

![Mapa przedstawiająca klastrowanych znaczniki i kompas przycisku](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>Automatycznie grupowania znaczników podczas powiększania

Przykład [MapKit próbki "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) pokazuje, jak wdrożyć nową adnotację iOS 11 funkcji klaster.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Utwórz `MKPointAnnotation` podklasy

Klasa adnotacji z punktu reprezentuje każdy znacznik na mapie. Można dodać je oddzielnie, używając `MapView.AddAnnotation()` lub używana tablica `MapView.AddAnnotations()`.

Punkt klas adnotacji nie ma wizualną reprezentację, są wymagane tylko do reprezentowania danych skojarzonych ze znacznikiem (przede wszystkim `Coordinate` właściwość, która jest jego współrzędne geograficzne na mapie) i wszystkie niestandardowe właściwości:

```csharp
public class Bike : MKPointAnnotation
{
  public BikeType Type { get; set; } = BikeType.Tricycle;
  public Bike(){}
  public Bike(NSNumber lat, NSNumber lgn, NSNumber type)
  {
    Coordinate = new CLLocationCoordinate2D(lat.NFloatValue, lgn.NFloatValue);
    switch(type.NUIntValue) {
      case 0:
        Type = BikeType.Unicycle;
        break;
      case 1:
        Type = BikeType.Tricycle;
        break;
    }
  }
}
```

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Utwórz `MKMarkerAnnotationView` podklasy dla pojedynczego znaczników

Widok adnotacji znacznika jest wizualną reprezentację każdego adnotacji i wyglądzie przy użyciu właściwości, takich jak:

- **MarkerTintColor** — kolor znacznika.
- **GlyphText** — tekst wyświetlany w znacznika.
- **GlyphImage** — Ustawia obraz, który jest wyświetlany w znacznika.
- **DisplayPriority** — określa porządek osi z (zachowanie stosu) Jeśli jest wiele znaczników mapy. Użyj jednej z `Required`, `DefaultHigh`, lub `DefaultLow`.

Do obsługi, automatyczne klastra, należy także ustawić:

- **ClusteringIdentifier** — Określa, które znaczników pobrać razem w klastrze. Możesz użyć tego samego identyfikatora dla wszystkich żądanych oznaczeń, lub różne identyfikatory do kontrolowania sposobu są pogrupowane.

```csharp
[Register("BikeView")]
public class BikeView : MKMarkerAnnotationView
{
  public static UIColor UnicycleColor = UIColor.FromRGB(254, 122, 36);
  public static UIColor TricycleColor = UIColor.FromRGB(153, 180, 44);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;

      var bike = value as Bike;
      if (bike != null){
        ClusteringIdentifier = "bike";
        switch(bike.Type){
          case BikeType.Unicycle:
            MarkerTintColor = UnicycleColor;
            GlyphImage = UIImage.FromBundle("Unicycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultLow;
            break;
          case BikeType.Tricycle:
            MarkerTintColor = TricycleColor;
            GlyphImage = UIImage.FromBundle("Tricycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
            break;
        }
      }
    }
  }
```

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. Utwórz `MKAnnotationView` do reprezentowania klastrów znaczników

Podczas gdy widok adnotacji, która reprezentuje klaster znaczników _można_ być proste obraz, użytkownicy będą aplikacji dostarczanie wizualnych, o ile znaczniki zostały zgrupowane razem.

[Przykładowy kod](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) używa CoreGraphics do renderowania liczbę znaczników w klastrze, a także wykres koło reprezentację część każdego typu znacznika.

Należy także ustawić:

- **DisplayPriority** — określa porządek osi z (zachowanie stosu) Jeśli jest wiele znaczników mapy. Użyj jednej z `Required`, `DefaultHigh`, lub `DefaultLow`.
- **CollisionMode** — `Circle` lub `Rectangle`.

```csharp
[Register("ClusterView")]
public class ClusterView : MKAnnotationView
{
  public static UIColor ClusterColor = UIColor.FromRGB(202, 150, 38);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;
      var cluster = MKAnnotationWrapperExtensions.UnwrapClusterAnnotation(value);
      if (cluster != null)
      {
        var renderer = new UIGraphicsImageRenderer(new CGSize(40, 40));
        var count = cluster.MemberAnnotations.Length;
        var unicycleCount = CountBikeType(cluster.MemberAnnotations, BikeType.Unicycle);

        Image = renderer.CreateImage((context) => {
          // Fill full circle with tricycle color
          BikeView.TricycleColor.SetFill();
          UIBezierPath.FromOval(new CGRect(0, 0, 40, 40)).Fill();
          // Fill pie with unicycle color
          BikeView.UnicycleColor.SetFill();
          var piePath = new UIBezierPath();
          piePath.AddArc(new CGPoint(20,20), 20, 0, (nfloat)(Math.PI * 2.0 * unicycleCount / count), true);
          piePath.AddLineTo(new CGPoint(20, 20));
          piePath.ClosePath();
          piePath.Fill();
          // Fill inner circle with white color
          UIColor.White.SetFill();
          UIBezierPath.FromOval(new CGRect(8, 8, 24, 24)).Fill();
          // Finally draw count text vertically and horizontally centered
          var attributes = new UIStringAttributes() {
            ForegroundColor = UIColor.Black,
            Font = UIFont.BoldSystemFontOfSize(20)
          };
          var text = new NSString($"{count}");
          var size = text.GetSizeUsingAttributes(attributes);
          var rect = new CGRect(20 - size.Width / 2, 20 - size.Height / 2, size.Width, size.Height);
          text.DrawString(rect, attributes);
        });
      }
    }
  }
  public ClusterView(){}
  public ClusterView(MKAnnotation annotation, string reuseIdentifier) : base(annotation, reuseIdentifier)
  {
    DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
    CollisionMode = MKAnnotationViewCollisionMode.Circle;
    // Offset center point to animate better with marker annotations
    CenterOffset = new CoreGraphics.CGPoint(0, -10);
  }
  private nuint CountBikeType(IMKAnnotation[] members, BikeType type) {
    nuint count = 0;
    foreach(Bike member in members){
      if (member.Type == type) ++count;
    }
    return count;
  }
}
```

### <a name="4-register-the-view-classes"></a>4. Rejestrowanie klasy widoku

Po utworzeniu formantu widoku mapy i dodanych do widoku, zarejestrować typy widoku adnotacji, aby włączyć automatyczne działanie klastra, jak i wylogowywanie powiększone mapy:

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. Renderowanie mapy!

Podczas renderowania mapy adnotacji znaczników w klastrze lub renderowane w zależności od poziomu powiększenia. Poziom powiększenia zmian znaczników animować i klastrów.

![Symulator przedstawiający znaczników klastrowanego na mapie](mapkit-images/cyclemap-sml.png)

Zapoznaj się [mapy sekcji](~/ios/user-interface/controls/ios-maps/index.md) Aby uzyskać więcej informacji na temat wyświetlania danych za pomocą MapKit.

<a name="compass" />

## <a name="compass-button"></a>Kompas przycisku

iOS 11 dodaje możliwość pop kompas poza mapy i renderować ją w innym miejscu w widoku. Zobacz [Tandm Przykładowa aplikacja](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) przykład.

Tworzenie przycisku, który wygląda jak kompas (łącznie animacji na żywo, gdy zostanie zmieniona orientacja mapy), i wyświetla go na inny formant.

![Przycisk wzorem na pasku nawigacyjnym](mapkit-images/compass-sml.png)

Poniższy kod tworzy przycisk wzorem i renderuje go na pasku nawigacyjnym:

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass` Właściwości można ustawić widoczność kompas domyślne w widoku mapy.

<a name="scale" />

## <a name="scale-view"></a>Widok skali

Dodaj skali w innym miejscu przy użyciu widoku `MKScaleView.FromMapView()` metodę, aby pobrać wystąpienia widoku skalowania, aby dodać w innym miejscu w hierarchii widoku.

![Widok skali umieszczenia na mapie](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale` Właściwości można ustawić widoczność kompas domyślne w widoku mapy.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>Przycisk śledzenia użytkownika

Przycisk śledzenia użytkownika centrach mapy na bieżącej lokalizacji użytkownika. Użyj `MKUserTrackingButton.FromMapView()` metodę, aby pobrać wystąpienia przycisku Zastosuj zmiany formatowania, a Dodaj w innym miejscu w hierarchii widoku.

![Przycisk lokalizacji użytkownika umieszczenia na mapie](mapkit-images/user-location-sml.png)

```csharp
var button = MKUserTrackingButton.FromMapView(MapView);
button.Layer.BackgroundColor = UIColor.FromRGBA(255,255,255,80).CGColor;
button.Layer.BorderColor = UIColor.White.CGColor;
button.Layer.BorderWidth = 1;
button.Layer.CornerRadius = 5;
button.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(button); // constraints omitted for simplicity
```


## <a name="related-links"></a>Linki pokrewne

- [Przykładowe MapKit "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [Nowości w MapKit (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/237/)
