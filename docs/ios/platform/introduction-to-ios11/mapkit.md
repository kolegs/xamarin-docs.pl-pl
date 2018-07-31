---
title: Nowe funkcje w strukturze MapKit w systemie iOS 11
description: 'W tym dokumencie opisano nowe funkcje systemu iOS 11 strukturze MapKit: grupowanie znaczniki, kompasu przycisku, widok skalowania i przycisk śledzenia użytkowników.'
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: c060a7bbc8d5968aeaca5f84743cdf22513dfbec
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350589"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>Nowe funkcje w strukturze MapKit w systemie iOS 11

System iOS 11 dodaje następujące nowe funkcje do strukturze MapKit:

- [Adnotacja klastrowania](#clustering)
- [Compass przycisku](#compass)
- [Wyświetl skalowania](#scale)
- [Przycisk śledzenia użytkownika](#user-tracking)

![Mapa przedstawiająca klastrowanych znaczniki i kompas przycisku](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>Automatycznie grupowania znaczników podczas powiększania

Przykład [przykładowe strukturze MapKit "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) przedstawia sposób implementowania Nowa adnotacja systemu iOS 11 funkcji klaster.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Utwórz `MKPointAnnotation` podklasy

Klasa adnotacji z punktu reprezentuje każdy znacznik na mapie. Można dodać oddzielnie przy użyciu `MapView.AddAnnotation()` lub z tablicy przy użyciu `MapView.AddAnnotations()`.

Punkt adnotacja klasy nie posiadają wizualnej reprezentacji, są wymagane tylko do reprezentowania danych skojarzony ze znacznikiem (co najważniejsze, `Coordinate` właściwość, która jest jego długość i szerokość geograficzną na mapie) oraz wszelkie właściwości niestandardowe:

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

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Utwórz `MKMarkerAnnotationView` podklasę dla pojedynczego znaczników

Widok znaczników adnotacji jest wizualną reprezentacją każdej adnotacji i wyglądzie przy użyciu właściwości, takich jak:

- **MarkerTintColor** — kolor znacznika.
- **GlyphText** — tekst wyświetlany w znacznika.
- **GlyphImage** — Ustawia obraz, który jest wyświetlany w znacznika.
- **DisplayPriority** — określa porządek osi z (zachowanie stosu) po zatłoczonym ze znacznikami mapy. Użyj jednej z `Required`, `DefaultHigh`, lub `DefaultLow`.

Aby umożliwić obsługę automatycznych klastrowania, należy także ustawić:

- **ClusteringIdentifier** — w ten sposób kontroluje znaczniki, które Pobierz klastrowanych ze sobą. Można użyć tego samego identyfikatora dla wszystkich znaczników sieci lub użyj różne identyfikatory, aby kontrolować sposób, są one zgrupowane razem.

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

Podczas gdy widok adnotacji, która reprezentuje klaster znaczników _można_ być proste obraz, Użytkownicy oczekują aplikacji, aby zapewnić podpowiedzi wizualne, o ile znaczniki, zostały zgrupowane razem.

[Przykładowego kodu](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) używa CoreGraphics do renderowania liczbę znaczników w klastrze, a także reprezentację wykresu okrąg część każdego typu znacznika.

Należy również ustawić:

- **DisplayPriority** — określa porządek osi z (zachowanie stosu) po zatłoczonym ze znacznikami mapy. Użyj jednej z `Required`, `DefaultHigh`, lub `DefaultLow`.
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

Przy kontrolki widoku mapy jest tworzony i dodawany do widoku, zarejestrować typy widoków adnotacji w celu umożliwienia automatycznego zachowania klastrowania i pomniejszać powiększone mapy:

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. Renderowanie mapy!

Po wyrenderowaniu mapy znaczników adnotacji klastra lub renderowane w zależności od poziomu powiększenia. Ponieważ zmienia poziom powiększenia, znaczniki animować i klastrów.

![Symulator Pokazywanie znaczników klastrowanego na mapie](mapkit-images/cyclemap-sml.png)

Zapoznaj się [mapuje sekcji](~/ios/user-interface/controls/ios-maps/index.md) Aby uzyskać więcej informacji na temat wyświetlania danych o strukturze MapKit.

<a name="compass" />

## <a name="compass-button"></a>Compass przycisku

System iOS 11 wprowadza możliwość pop compass poza mapy i renderować ją w innym miejscu w widoku. Zobacz [Tandm przykładową aplikację](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) przykład.

Utwórz przycisk, który wygląda jak kompas (w tym na żywo animacji po zmianie orientacji mapy) i renderuje je w innej kontrolce.

![Kompasu przycisk na pasku nawigacyjnym](mapkit-images/compass-sml.png)

Poniższy kod tworzy kompasu przycisk i renderuje je na pasku nawigacyjnym:

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass` Właściwość może służyć do sterowania widocznością compass domyślne wewnątrz widoku mapy.

<a name="scale" />

## <a name="scale-view"></a>Wyświetl skalowania

Dodaj skalę w którymkolwiek miejscu przy użyciu widoku `MKScaleView.FromMapView()` metodę, aby pobrać wystąpienie obiektu widoku skali, aby dodać innych miejscach, w widoku hierarchii.

![Wyświetl skalowania nałożony na mapie](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale` Właściwość może służyć do sterowania widocznością compass domyślne wewnątrz widoku mapy.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>Przycisk śledzenia użytkownika

Przycisk śledzenia użytkownika centra mapy w bieżącej lokalizacji użytkownika. Użyj `MKUserTrackingButton.FromMapView()` metodę, aby pobrać wystąpienie obiektu przycisku, Zastosuj zmiany formatowania i dodać w innym miejscu w Wyświetl hierarchię.

![Przycisk lokalizacji użytkownika nałożony na mapie](mapkit-images/user-location-sml.png)

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

- [Przykładowe strukturze MapKit "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [Nowości w strukturze MapKit (WWDC) (wideo)](https://developer.apple.com/videos/play/wwdc2017/237/)
