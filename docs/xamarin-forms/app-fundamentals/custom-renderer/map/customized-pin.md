---
title: Dostosowywanie mapy numer Pin
description: "W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla formantu mapy, wyświetlającego natywnego mapy z dostosowanych numeru pin i dostosowany widok danych numeru pin na każdej z platform."
ms.topic: article
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 312bda44a6b390c6ba486d5a3d60dfe4fb770a2e
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="customizing-a-map-pin"></a>Dostosowywanie mapy numer Pin

_W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla formantu mapy, wyświetlającego natywnego mapy z dostosowanych numeru pin i dostosowany widok danych numeru pin na każdej z platform._

Każdy widok platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. Gdy [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS, `MapRenderer` tworzenia wystąpienia klasy, która z kolei tworzy natywny `MKMapView` formantu. Na platformie Android `MapRenderer` natywny tworzy wystąpienie klasy `MapView` formantu. W systemie Windows platformy Uniwersalnej, `MapRenderer` natywny tworzy wystąpienie klasy `MapControl`. Aby uzyskać więcej informacji na temat klasy macierzystego formantu, mapowane na formanty platformy Xamarin.Forms i renderowania, zobacz [renderowania klasy podstawowej i kontrolki natywne](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono związek między [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) i odpowiednie natywnego formantów, które implementuje ona:

![](customized-pin-images/map-classes.png "Relacja między formantu mapy i implementujący kontrolki natywne")

Proces renderowania może służyć do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) na każdej z platform. Proces ten wygląda następująco:

1. [Utwórz](#Creating_the_Custom_Map) mapy niestandardowe platformy Xamarin.Forms.
1. [Korzystać z](#Consuming_the_Custom_Map) niestandardowe mapowanie z platformy Xamarin.Forms.
1. [Utwórz](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania mapy na każdej z platform.

Każdy element teraz omówione zostaną z kolei, aby zaimplementować `CustomMap` renderowania wyświetlający natywnego mapy z dostosowanych numeru pin i dostosowany widok danych numeru pin na każdej z platform.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) musi być zainicjowana i skonfigurowana przed użyciem. Aby uzyskać więcej informacji, zobacz [ `Maps Control` ](~/xamarin-forms/user-interface/map.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Tworzenie niestandardowych mapy

Kontrolki niestandardowe mapy mogą być tworzone przez podklasy [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) klasy, jak pokazano w poniższym przykładzie:

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap` Kontroli jest tworzony w projekcie (PCL) biblioteki klas przenośnych i definiuje niestandardowe mapy interfejsu API. Przedstawia niestandardowe mapy `CustomPins` właściwość, która reprezentuje kolekcję `CustomPin` obiektów, które są wyświetlane przez formant mapy natywnego na każdej z platform. `CustomPin` Klasy przedstawiono w poniższym przykładzie kodu:

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

Ta klasa definiuje `CustomPin` jako dziedziczenie właściwości [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) klasy i dodawanie `Url` właściwości.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Korzystanie z niestandardowych mapy

`CustomMap` Formant może być przywoływany w XAML w projekcie PCL deklarowanie przestrzeni nazw dla lokalizacji, a następnie użyć prefiksu przestrzeni nazw w formancie mapy niestandardowe. Poniższy kod przedstawia przykład sposobu `CustomMap` formant może być zużyte przez strony XAML:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer">
    <ContentPage.Content>
        <local:CustomMap x:Name="myMap" MapType="Street"
          WidthRequest="{x:Static local:App.ScreenWidth}"
          HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

`local` Prefiks przestrzeni nazw może mieć nazwę żadnych czynności. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły niestandardowe mapy. Po zadeklarowaniu obszaru nazw prefiks jest używany do odwołania niestandardowe mapy.

Poniższy kod przedstawia przykład sposobu `CustomMap` formant może być zużyte przez stronę C#:

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

`CustomMap` Wystąpienie zostanie użyte do wyświetlenia mapy natywnego na każdej z platform. Ma ona [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) właściwość ustawia styl wyświetlania [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/), wartościami możliwe jest zdefiniowany w [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/) wyliczenia. Systemy iOS i Android, szerokość i wysokość mapy ustawiana za pośrednictwem właściwości `App` klasy, które są inicjowane w projektach specyficzne dla platformy.

Lokalizacja mapy i numery PIN ją zawiera, są inicjowane, jak pokazano w poniższym przykładzie kodu:

```csharp
public MapPage ()
{
  ...
  var pin = new CustomPin {
    Type = PinType.Place,
    Position = new Position (37.79752, -122.40183),
    Label = "Xamarin San Francisco Office",
    Address = "394 Pacific Ave, San Francisco CA",
    Id = "Xamarin",
    Url = "http://xamarin.com/about/"
  };

  customMap.CustomPins = new List<CustomPin> { pin };
  customMap.Pins.Add (pin);
  customMap.MoveToRegion (MapSpan.FromCenterAndRadius (
    new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
}
```

Ta inicjowania dodaje niestandardowy numer pin i umieszcza widoku mapy z [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) metodę, która zmienia położenie i poziom powiększenia mapy, tworząc [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) z [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) i [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

Teraz można dodać do każdego projektu aplikacji, aby dostosować natywnego mapy formantów niestandardowego modułu renderowania.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania wygląda następująco:

1. Utwórz podklasę `MapRenderer` klasy, która renderuje niestandardowe mapy.
1. Zastąpienie `OnElementChanged` metodę, która renderuje mapy niestandardowe i pisania logiki, aby dostosować go. Ta metoda jest wywoływana po utworzeniu odpowiednich mapy niestandardowe platformy Xamarin.Forms.
1. Dodaj `ExportRenderer` atrybutu klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania mapy niestandardowe platformy Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania z platformy Xamarin.Forms.

> [!NOTE]
> Jest to pozycja opcjonalna zapewnienie niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej formantu będzie używany.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](customized-pin-images/solution-structure.png "Obowiązki CustomMap niestandardowe renderowania projektu:")

`CustomMap` Renderowania formantu przez klasy renderowania specyficzne dla platformy, które wynikają z `MapRenderer` klasy dla każdej platformy. Powoduje to w każdym `CustomMap` kontrolować renderowanego specyficzne dla platformy kontroli, jak pokazano na poniższych zrzutach ekranu:

![](customized-pin-images/screenshots.png "CustomMap na każdej platformie")

`MapRenderer` Klasy ujawnia `OnElementChanged` metodę, która jest wywoływana po utworzeniu niestandardowej mapy platformy Xamarin.Forms renderowanie odpowiednich macierzystego formantu. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentuje elementu platformy Xamarin.Forms który renderującego *został* dołączyć i elementu platformy Xamarin.Forms który renderującego *jest* dołączony do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie `null` i `NewElement` właściwości będzie zawierać odwołanie do `CustomMap` wystąpienia.

Zastąpiona wersja `OnElementChanged` metody w każdej klasie renderowania specyficzne dla platformy jest miejscem do wykonania dostosowanie macierzystego formantu. Typu odwołanie do macierzystego formantu używanego na platformie jest możliwy za pośrednictwem `Control` właściwości. Ponadto można uzyskać odwołanie do formantu platformy Xamarin.Forms, który jest renderowany przy użyciu `Element` właściwości.

Należy zachować ostrożność podczas subskrybowania obsługi zdarzeń w `OnElementChanged` metody, jak pokazano w poniższym przykładzie:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Macierzystego formantu powinna być skonfigurowana i procedury obsługi zdarzeń zasubskrybować tylko wtedy, gdy niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Podobnie programy obsługi zdarzeń, które zostały subskrybuje powinien można anulować w tylko element renderującego jest dołączony do zmiany. Przyjmowanie takie podejście pomoże utworzyć niestandardowego modułu renderowania nie boryka się z przecieki pamięci.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje mechanizm renderujący platformy Xamarin.Forms. Atrybut przyjmuje dwa parametry — Nazwa typu formantu niestandardowego platformy Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono implementacji każdej klasy niestandardowego modułu renderowania specyficzne dla platformy.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie modułu renderowania niestandardowe w systemie iOS

Poniższe zrzuty ekranu przedstawiają mapy, przed i po dostosowaniu:

![](customized-pin-images/map-layout-ios.png "Formant mapy przed i po dostosowaniu")

W systemie iOS numer pin jest nazywany *adnotacji*, i może być niestandardowy obraz lub numeru pin zdefiniowana w systemie różnych kolorów. Opcjonalnie można pokazać adnotacje *objaśnienia*, która jest wyświetlana w odpowiedzi na użytkownika, zaznaczając adnotację. Zawiera objaśnienie `Label` i `Address` właściwości `Pin` wystąpienia opcjonalne po lewej i prawej akcesoriów widoków. Na zrzucie ekranu powyżej, po lewej stronie akcesoriów widok nie zawiera obrazu małp z jest prawo akcesoriów widoku *informacji* przycisku.

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy iOS:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        UIView customPinView;
        List<CustomPin> customPins;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveAnnotations(nativeMap.Annotations);
                    nativeMap.GetViewForAnnotation = null;
                    nativeMap.CalloutAccessoryControlTapped -= OnCalloutAccessoryControlTapped;
                    nativeMap.DidSelectAnnotationView -= OnDidSelectAnnotationView;
                    nativeMap.DidDeselectAnnotationView -= OnDidDeselectAnnotationView;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                customPins = formsMap.CustomPins;

                nativeMap.GetViewForAnnotation = GetViewForAnnotation;
                nativeMap.CalloutAccessoryControlTapped += OnCalloutAccessoryControlTapped;
                nativeMap.DidSelectAnnotationView += OnDidSelectAnnotationView;
                nativeMap.DidDeselectAnnotationView += OnDidDeselectAnnotationView;
            }
        }
        ...
    }
}
```

`OnElementChanged` Metoda wykonuje następujące konfiguracji [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) wystąpienia, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms:

- [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) Właściwość jest ustawiona na `GetViewForAnnotation` metody. Ta metoda jest wywoływana, gdy [lokalizacji adnotacji, staje się widoczne na mapie](#Displaying_the_Annotation)i jest używany do dostosowania przed adnotacji do wyświetlenia.
- Programy obsługi zdarzeń dla `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, i `DidDeselectAnnotationView` są rejestrowane zdarzenia. Wyzwalać te zdarzenia, kiedy użytkownik [naciska prawo akcesoriów w objaśnienie](#Tapping_on_the_Right_Callout_Accessory_View)i kiedy użytkownik [wybiera](#Selecting_the_Annotation) i [odznacza](#Deselecting_the_Annotation) adnotację, odpowiednio. Zdarzenia są anulować w tylko wtedy, gdy element renderującego jest dołączony do zmiany.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Wyświetlanie adnotacji

`GetViewForAnnotation` Metoda jest wywoływana, gdy lokalizacji adnotacji, staje się widoczne na mapie i jest używany do dostosowania przed adnotacji, aby wyświetlić. Adnotacja ma dwie części:

- `MkAnnotation` — zawiera title, subtitle i lokalizacji adnotacji.
- `MkAnnotationView` — zawiera obraz do reprezentowania adnotację oraz opcjonalnie objaśnienia, który jest wyświetlany, gdy użytkownik naciska adnotacji.

`GetViewForAnnotation` Metoda przyjmuje `IMKAnnotation` który zawiera dane adnotacji i zwraca `MKAnnotationView` do wyświetlenia na mapie i jest wyświetlany w poniższym przykładzie:

```csharp
MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null) {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.Id.ToString());
    if (annotationView == null) {
        annotationView = new CustomMKAnnotationView(annotation, customPin.Id.ToString());
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).Id = customPin.Id.ToString();
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

Ta metoda gwarantuje, że adnotacja będzie wyświetlany jako obraz niestandardowy, a nie jako zdefiniowana w systemie numeru pin, a adnotacja jest wybrany objaśnienia będą wyświetlane zawierającą dodatkowej zawartości po lewej i prawej adnotacji tytuł i adres . Jest to zrobić w następujący sposób:

1. `GetCustomPin` Metoda jest wywoływana, aby zwrócić dane niestandardowe numeru pin dla adnotacji.
1. Aby zachowywania pamięci, widok adnotacji jest puli do ponownego użycia z wywołaniem do [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/).
1. `CustomMKAnnotationView` Rozszerza klasy `MKAnnotationView` klasy z `Id` i `Url` właściwości, które odpowiadają właściwościom identyczne `CustomPin` wystąpienia. Nowe wystąpienie klasy `CustomMKAnnotationView` jest tworzony, pod warunkiem, że jest adnotacja `null`:
  - `CustomMKAnnotationView.Image` Właściwość jest ustawiona na obraz, który będzie stanowić adnotacji na mapie.
  - `CustomMKAnnotationView.CalloutOffset` Ma ustawioną właściwość `CGPoint` Określa, że objaśnienie będzie wyśrodkowana powyżej adnotacji.
  - `CustomMKAnnotationView.LeftCalloutAccessoryView` Właściwość jest ustawiona na obrazie małp, która będzie wyświetlana po lewej adnotacji tytuł i adres.
  - `CustomMKAnnotationView.RightCalloutAccessoryView` Właściwość jest ustawiona na *informacji* przycisku, która będzie wyświetlana po prawej stronie adnotacji tytuł i adres.
  - `CustomMKAnnotationView.Id` Właściwość jest ustawiona na `CustomPin.Id` właściwości zwróconej przez `GetCustomPin` metody. Dzięki temu adnotacji identyfikację, aby [objaśnienia można dodatkowo dostosowywać](#Selecting_the_Annotation), jeśli to konieczne.
  - `CustomMKAnnotationView.Url` Właściwość jest ustawiona na `CustomPin.Url` właściwości zwróconej przez `GetCustomPin` metody. Adres URL nastąpi przejście, kiedy użytkownik [naciska wyświetlane w widoku akcesoriów objaśnienia prawym przycisku](#Tapping_on_the_Right_Callout_Accessory_View).
1. [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) Właściwość jest ustawiona na `true` pojawia się objaśnienia adnotacji jest wybrany.
1. Adnotacja są zwracane do wyświetlenia na mapie.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>Wybieranie adnotacji

Po naciśnięciu adnotację, `DidSelectAnnotationView` generowane zdarzenie, które z kolei wykonuje `OnDidSelectAnnotationView` metody:

```csharp
void OnDidSelectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  customPinView = new UIView ();

  if (customView.Id == "Xamarin") {
    customPinView.Frame = new CGRect (0, 0, 200, 84);
    var image = new UIImageView (new CGRect (0, 0, 200, 84));
    image.Image = UIImage.FromFile ("xamarin.png");
    customPinView.AddSubview (image);
    customPinView.Center = new CGPoint (0, -(e.View.Frame.Height + 75));
    e.View.AddSubview (customPinView);
  }
}
```

Ta metoda jest rozszerzeniem istniejącej objaśnienie (która zawiera widoki po lewej i prawej akcesoriów) przez dodanie `UIView` wystąpienie do niego, które zawiera obraz Xamarin logo, pod warunkiem, że wybrana adnotacja jego `Id` właściwość `Xamarin`. Dzięki temu w scenariuszach, w którym różnych objaśnienia mogą być wyświetlane dla różnych adnotacji. `UIView` Wystąpienie będzie wyświetlana wyśrodkowane powyżej istniejących objaśnienia.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Naciśnięcie przycisku w widoku prawo akcesoriów objaśnienia

Po naciśnięciu na *informacji* przycisku w widoku akcesoriów prawo objaśnienia `CalloutAccessoryControlTapped` generowane zdarzenie, które z kolei wykonuje `OnCalloutAccessoryControlTapped` metody:

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

Ta metoda otwiera w przeglądarce sieci web i powoduje przejście do adresu przechowywane w `CustomMKAnnotationView.Url` właściwości. Należy pamiętać, że adres został zdefiniowany podczas tworzenia `CustomPin` kolekcji w projekcie PCL.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>Usunięcie zaznaczenia adnotacji

Gdy jest wyświetlana adnotacja i naciśnięciu na mapie, `DidDeselectAnnotationView` generowane zdarzenie, które z kolei wykonuje `OnDidDeselectAnnotationView` metody:

```csharp
void OnDidDeselectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  if (!e.View.Selected) {
    customPinView.RemoveFromSuperview ();
    customPinView.Dispose ();
    customPinView = null;
  }
}
```

Ta metoda gwarantuje, że podczas istniejących objaśnienia nie jest zaznaczone, rozszerzone część objaśnienia (obraz Xamarin logo) spowoduje również przerwanie wyświetlane i jego zasoby, zostaną zwolnione.

Aby uzyskać więcej informacji dotyczących dostosowywania `MKMapView` wystąpienia, zobacz [mapy iOS](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Tworzenie niestandardowego modułu renderowania w systemie Android

Poniższe zrzuty ekranu przedstawiają mapy, przed i po dostosowaniu:

![](customized-pin-images/map-layout-android.png "Formant mapy przed i po dostosowaniu")

W systemie Android numer pin jest nazywany *znacznika*, i może być niestandardowy obraz lub znacznik zdefiniowana w systemie różnych kolorów. Znaczników można wyświetlić *okna informacje*, która jest wyświetlana w odpowiedzi na użytkownika, naciskając znacznika. Wyświetla okno informacje `Label` i `Address` właściwości `Pin` wystąpienia oraz można dostosować, aby dołączyć do innej zawartości. Jednak tylko jedno okno informacje można wyświetlić na raz.

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy systemu Android:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.Droid
{
    public class CustomMapRenderer : MapRenderer, GoogleMap.IInfoWindowAdapter
    {
        List<CustomPin> customPins;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                NativeMap.InfoWindowClick -= OnInfoWindowClick;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                customPins = formsMap.CustomPins;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(GoogleMap map)
        {
            base.OnMapReady(map);

            NativeMap.InfoWindowClick += OnInfoWindowClick;
            NativeMap.SetInfoWindowAdapter(this);
        }
        ...
    }
}
```

Pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms `OnElementChanged` wywołania metody `MapView.GetMapAsync` metodę, która pobiera odpowiadającego `GoogleMap` który jest powiązany z widoku. Raz `GoogleMap` wystąpienie jest dostępne, `OnMapReady` zastąpienie zostanie wywołany. Ta metoda rejestruje program obsługi zdarzeń dla `InfoWindowClick` zdarzenie, które są uruchamiane w przypadku [okna informacje o kliknięciu](#Clicking_on_the_Info_Window)i Anulowano subskrypcję, tylko wtedy, gdy element renderującego jest dołączony do zmiany. `OnMapReady` Również zastąpić wywołania `SetInfoWindowAdapter` metodę, aby określić, że `CustomMapRenderer` wystąpienie klasy zapewnia metody do dostosowywania okna informacje.

`CustomMapRenderer` Klasa implementuje `GoogleMap.IInfoWindowAdapter` interfejsu do [Dostosowywanie okna informacje](#Customizing_the_Info_Window). Ten interfejs określa musi być implementowana następujących metod:

- `public Android.Views.View GetInfoWindow(Marker marker)` — Ta metoda jest wywoływana, aby powrócić do okna informacje o niestandardowych na potrzeby znacznika. Jeśli zmienna zwraca `null`, zostanie użyta domyślna renderowanie okien. Jeśli zmienna zwraca `View`, który następnie `View` zostaną umieszczone wewnątrz ramki okna informacje.
- `public Android.Views.View GetInfoContents(Marker marker)` — Ta metoda jest wywoływana, aby zwrócić `View` z zawartością okna informacje i będzie można wywołać tylko, jeśli `GetInfoWindow` metoda zwraca `null`. Jeśli zmienna zwraca `null`, zostanie użyta odwzorowanie domyślne informacje o zawartości okna.

W przykładowej aplikacji zawartości okna informacje o dostosowaniu i dlatego element `GetInfoWindow` metoda zwraca `null` aby je włączyć.

#### <a name="customizing-the-marker"></a>Dostosowywanie znacznika

Ikona używany do reprezentowania znacznik można dostosować, wywołując `MarkerOptions.SetIcon` metody. Można to osiągnąć przez zastąpienie `CreateMarker` metodę, która jest wywoływana dla każdej `Pin` który został dodany do mapy:

```csharp
protected override MarkerOptions CreateMarker(Pin pin)
{
    var marker = new MarkerOptions();
    marker.SetPosition(new LatLng(pin.Position.Latitude, pin.Position.Longitude));
    marker.SetTitle(pin.Label);
    marker.SetSnippet(pin.Address);
    marker.SetIcon(BitmapDescriptorFactory.FromResource(Resource.Drawable.pin));
    return marker;
}
```

Ta metoda tworzy nowy `MarkerOption` wystąpienia dla każdego `Pin` wystąpienia. Po ustawieniu pozycji, etykiety i adres znacznika, jego ikonę została skonfigurowana z `SetIcon` metody. Ta metoda przyjmuje `BitmapDescriptor` obiekt zawierający dane wymagane do renderowania ikony, z `BitmapDescriptorFactory` udostępnia metody pomocnicze, aby uprościć tworzenie klasy `BitmapDescriptor`.

Aby uzyskać więcej informacji o korzystaniu z `BitmapDescriptorFactory` klasę, aby dostosować informacje, zobacz [Dostosowywanie znacznik](~/android/platform/maps-and-location/maps/maps-api.md).

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Dostosowywanie okna informacje

Gdy użytkownik naciska na znacznika, `GetInfoContents` metoda jest wykonywana, pod warunkiem, że `GetInfoWindow` metoda zwraca `null`. Poniższy kod przedstawia przykład `GetInfoContents` metody:

```csharp
public Android.Views.View GetInfoContents (Marker marker)
{
  var inflater = Android.App.Application.Context.GetSystemService (Context.LayoutInflaterService) as Android.Views.LayoutInflater;
  if (inflater != null) {
    Android.Views.View view;

    var customPin = GetCustomPin (marker);
    if (customPin == null) {
      throw new Exception ("Custom pin not found");
    }

    if (customPin.Id.ToString() == "Xamarin") {
      view = inflater.Inflate (Resource.Layout.XamarinMapInfoWindow, null);
    } else {
      view = inflater.Inflate (Resource.Layout.MapInfoWindow, null);
    }

    var infoTitle = view.FindViewById<TextView> (Resource.Id.InfoWindowTitle);
    var infoSubtitle = view.FindViewById<TextView> (Resource.Id.InfoWindowSubtitle);

    if (infoTitle != null) {
      infoTitle.Text = marker.Title;
    }
    if (infoSubtitle != null) {
      infoSubtitle.Text = marker.Snippet;
    }

    return view;
  }
  return null;
}
```

Ta metoda zwraca `View` okna informacje z zawartością. Jest to zrobić w następujący sposób:

- A `LayoutInflater` wystąpienia są pobierane. Jest on używany do utworzenia wystąpienia pliku XML układu w odpowiednie `View`.
- `GetCustomPin` Metoda jest wywoływana, aby zwrócić dane niestandardowe numeru pin dla okna informacji.
- `XamarinMapInfoWindow` Układ jest zwiększony, jeśli `CustomPin.Id` właściwości jest równa `Xamarin`. W przeciwnym razie `MapInfoWindow` jest zwiększony układu. Dzięki temu w scenariuszach, w którym można wyświetlać układów okien z użyciem innych informacji dla różnych znaczników.
- `InfoWindowTitle` i `InfoWindowSubtitle` zasoby są pobierane z nadmuchany układ i ich `Text` właściwości są ustawione na odpowiednie dane z `Marker` wystąpienia, pod warunkiem, że zasoby nie są `null`.
- `View` Wystąpienia są zwracane do wyświetlenia na mapie.

> [!NOTE]
> Informacje o okno nie jest na żywo `View`. Zamiast tego przekonwertuje Android `View` do statycznego bitmap i który wyświetlany jako obraz. To oznacza, że podczas okna informacje mogą odpowiadać na zdarzenie click, nie może odpowiadać touch zdarzenia lub gestów i pojedynczych formantów w oknie informacji nie może odpowiadać na ich własnych zdarzenia kliknięcia.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>Kliknięcie okno informacyjne

Gdy użytkownik kliknie okno informacje `InfoWindowClick` generowane zdarzenie, które z kolei wykonuje `OnInfoWindowClick` metody:

```csharp
void OnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
  var customPin = GetCustomPin (e.Marker);
  if (customPin == null) {
    throw new Exception ("Custom pin not found");
  }

  if (!string.IsNullOrWhiteSpace (customPin.Url)) {
    var url = Android.Net.Uri.Parse (customPin.Url);
    var intent = new Intent (Intent.ActionView, url);
    intent.AddFlags (ActivityFlags.NewTask);
    Android.App.Application.Context.StartActivity (intent);
  }
}
```

Ta metoda otwiera w przeglądarce sieci web i powoduje przejście do adresu przechowywane w `Url` właściwości pobranej `CustomPin` wystąpienie `Marker`. Należy pamiętać, że adres został zdefiniowany podczas tworzenia `CustomPin` kolekcji w projekcie PCL.

Aby uzyskać więcej informacji dotyczących dostosowywania `MapView` wystąpienia, zobacz [interfejsu API map](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Tworzenie niestandardowego modułu renderowania na platformę uniwersalną systemu Windows

Poniższe zrzuty ekranu przedstawiają mapy, przed i po dostosowaniu:

![](customized-pin-images/map-layout-uwp.png "Formant mapy przed i po dostosowaniu")

Na platformy uniwersalnej systemu Windows numer pin jest nazywany *ikonę mapy*, i może być niestandardowy obraz lub zdefiniowane w systemie domyślnego obrazu. Ikona mapy można wyświetlić `UserControl`, która jest wyświetlana w odpowiedzi na użytkownika, naciskając ikonę mapy. `UserControl` Można wyświetlać zawartość, łącznie z `Label` i `Address` właściwości `Pin` wystąpienia.

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania platformy uniwersalnej systemu Windows:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        MapControl nativeMap;
        List<CustomPin> customPins;
        XamarinMapOverlay mapOverlay;
        bool xamarinOverlayShown = false;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                nativeMap.MapElementClick -= OnMapElementClick;
                nativeMap.Children.Clear();
                mapOverlay = null;
                nativeMap = null;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                nativeMap = Control as MapControl;
                customPins = formsMap.CustomPins;

                nativeMap.Children.Clear();
                nativeMap.MapElementClick += OnMapElementClick;

                foreach (var pin in customPins)
                {
                    var snPosition = new BasicGeoposition { Latitude = pin.Pin.Position.Latitude, Longitude = pin.Pin.Position.Longitude };
                    var snPoint = new Geopoint(snPosition);

                    var mapIcon = new MapIcon();
                    mapIcon.Image = RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///pin.png"));
                    mapIcon.CollisionBehaviorDesired = MapElementCollisionBehavior.RemainVisible;
                    mapIcon.Location = snPoint;
                    mapIcon.NormalizedAnchorPoint = new Windows.Foundation.Point(0.5, 1.0);

                    nativeMap.MapElements.Add(mapIcon);                    
                }
            }
        }
        ...
    }
}
```

`OnElementChanged` Metoda wykonuje następujące operacje, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms:

- Czyści `MapControl.Children` kolekcji, aby usunąć wszystkie istniejące elementy interfejsu użytkownika z mapy, przed zarejestrowaniem programu obsługi zdarzeń dla `MapElementClick` zdarzeń. To zdarzenie generowane, gdy użytkownik naciska lub kliknie `MapElement` na `MapControl`i Anulowano subskrypcję, tylko wtedy, gdy element renderującego jest dołączony do zmiany.
- Każdy kod pin w `customPins` Kolekcja zostanie wyświetlona w poprawnej lokalizacji geograficznej na mapie w następujący sposób:
  - Lokalizacja numer pin jest tworzony jako `Geopoint` wystąpienia.
  - A `MapIcon` do reprezentowania numer pin jest tworzone wystąpienie.
  - Obraz używany do reprezentowania `MapIcon` jest określany przez ustawienie `MapIcon.Image` właściwości. Jednak obraz ikony mapy nie zawsze gwarantuje pokazano, jak zostanie przesłonięty przez inne elementy na mapie. W związku z tym mapy ikony `CollisionBehaviorDesired` właściwość jest ustawiona na `MapElementCollisionBehavior.RemainVisible`, aby upewnić się, że pozostaje widoczna.
  - Lokalizacja `MapIcon` jest określany przez ustawienie `MapIcon.Location` właściwości.
  - `MapIcon.NormalizedAnchorPoint` Właściwość jest ustawiona na przybliżonej lokalizacji wskaźnika na obrazie. Jeśli ta właściwość przechowuje wartość domyślną (0,0), co stanowi lewym górnym rogu obrazu, zmiany w poziom powiększenia mapy może skutkować obrazu wskazanie w innej lokalizacji.
  - `MapIcon` Wystąpienia jest dodawany do `MapControl.MapElements` kolekcji. Powoduje to ikonę mapy będzie wyświetlany na `MapControl`.

> [!NOTE]
> W przypadku używania tego samego obrazu dla wielu ikon mapy, `RandomAccessStreamReference` wystąpienia powinien być zadeklarowany jako na poziomie strony lub aplikacji Aby uzyskać najlepszą wydajność.

#### <a name="displaying-the-usercontrol"></a>Wyświetlanie UserControl

Gdy użytkownik naciska ikonę mapy `OnMapElementClick` metoda jest wykonywana. Poniższy przykład kodu pokazuje tej metody:

```csharp
private void OnMapElementClick(MapControl sender, MapElementClickEventArgs args)
{
    var mapIcon = args.MapElements.FirstOrDefault(x => x is MapIcon) as MapIcon;
    if (mapIcon != null)
    {
        if (!xamarinOverlayShown)
        {
            var customPin = GetCustomPin(mapIcon.Location.Position);
            if (customPin == null)
            {
                throw new Exception("Custom pin not found");
            }

            if (customPin.Id.ToString() == "Xamarin")
            {
                if (mapOverlay == null)
                {
                    mapOverlay = new XamarinMapOverlay(customPin);
                }

                var snPosition = new BasicGeoposition { Latitude = customPin.Pin.Position.Latitude, Longitude = customPin.Pin.Position.Longitude };
                var snPoint = new Geopoint(snPosition);

                nativeMap.Children.Add(mapOverlay);
                MapControl.SetLocation(mapOverlay, snPoint);
                MapControl.SetNormalizedAnchorPoint(mapOverlay, new Windows.Foundation.Point(0.5, 1.0));
                xamarinOverlayShown = true;
            }
        }
        else
        {
            nativeMap.Children.Remove(mapOverlay);
            xamarinOverlayShown = false;
        }
    }
}
```

Ta metoda tworzy `UserControl` wystąpienia, który zawiera informacje o numer pin. Jest to zrobić w następujący sposób:

- `MapIcon` Wystąpienia są pobierane.
- `GetCustomPin` Metoda jest wywoływana, aby zwrócić dane niestandardowe numeru pin, który będzie wyświetlany.
- A `XamarinMapOverlay` do wyświetlania danych niestandardowych numer pin jest tworzone wystąpienie. Ta klasa jest kontrolki użytkownika.
- Lokalizacji geograficznej, w którym można wyświetlić `XamarinMapOverlay` wystąpienie na `MapControl` zostanie utworzona jako `Geopoint` wystąpienia.
- `XamarinMapOverlay` Wystąpienia jest dodawany do `MapControl.Children` kolekcji. Ta kolekcja zawiera elementy interfejsu użytkownika XAML, które zostaną wyświetlone na mapie.
- Lokalizacja geograficzna `XamarinMapOverlay` wystąpienia na mapie jest ustawiony przez wywołanie metody `SetLocation` metody.
- Względne położenie na `XamarinMapOverlay` wystąpienia, która odpowiada podanej lokalizacji, jest ustawiony przez wywołanie metody `SetNormalizedAnchorPoint` metody. Dzięki temu, który zmienia poziom powiększenia mapy wynik w `XamarinMapOverlay` wystąpienie, zawsze wyświetlany w poprawnej lokalizacji.

Alternatywnie, jeśli informacji o numer pin jest już wyświetlana na mapie, naciskając pozycję na mapie usuwa `XamarinMapOverlay` wystąpienia z `MapControl.Children` kolekcji.

#### <a name="tapping-on-the-information-button"></a>Naciskając przycisk informacji

Po naciśnięciu na *informacji* przycisk w `XamarinMapOverlay` kontrolki użytkownika `Tapped` generowane zdarzenie, które z kolei wykonuje `OnInfoButtonTapped` metody:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Ta metoda otwiera w przeglądarce sieci web i powoduje przejście do adresu przechowywane w `Url` właściwość `CustomPin` wystąpienia. Należy pamiętać, że adres został zdefiniowany podczas tworzenia `CustomPin` kolekcji w projekcie PCL.

Aby uzyskać więcej informacji dotyczących dostosowywania `MapControl` wystąpienia, zobacz [mapy i informacje o lokalizacji](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) w witrynie MSDN.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla `Map` formantu umożliwiają deweloperom zastąpienie renderowania natywnego domyślne z własne dostosowania specyficzne dla platformy. Xamarin.Forms.Maps zapewnia abstrakcji między platformami, wyświetlania mapy używających mapy natywnych interfejsów API na każdej platformie, aby zapewnić szybkie i znanych mapy środowisko dla użytkowników.


## <a name="related-links"></a>Linki pokrewne

- [Formantu mapy](~/xamarin-forms/user-interface/map.md)
- [Mapy dla systemu iOS](~/ios/user-interface/controls/ios-maps/index.md)
- [Interfejs API map](~/android/platform/maps-and-location/maps/maps-api.md)
- [Dostosowane numeru Pin (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
