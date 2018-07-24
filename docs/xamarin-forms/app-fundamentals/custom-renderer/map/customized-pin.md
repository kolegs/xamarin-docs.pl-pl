---
title: Dostosowywanie pinezki mapy
description: W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla formantu mapy, zawierające mapę natywnych przy użyciu dostosowanych numeru pin i dostosowany widok danych numeru pin na każdej platformie.
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 351119a8b0089f78d4ce98729a1516c3cd7bae7b
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203088"
---
# <a name="customizing-a-map-pin"></a>Dostosowywanie pinezki mapy

_W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla formantu mapy, zawierające mapę natywnych przy użyciu dostosowanych numeru pin i dostosowany widok danych numeru pin na każdej platformie._

Każdego widoku interfejsu Xamarin.Forms ma towarzyszący modułu renderowania dla każdej platformy, która tworzy wystąpienie kontrolki natywne. Gdy [ `Map` ](xref:Xamarin.Forms.Maps.Map) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS, `MapRenderer` tworzenia wystąpienia klasy, która z kolei tworzy macierzystej `MKMapView` kontroli. Na platformie Android `MapRenderer` klasy tworzy macierzystej `MapView` kontroli. Na Universal Windows Platform (platformy UWP), `MapRenderer` klasy tworzy macierzystej `MapControl`. Aby uzyskać więcej informacji na temat renderowania i klasy natywnych kontrolek, mapowane kontrolek zestawu narzędzi Xamarin.Forms, zobacz [natywne kontrolki i klasy podstawowej renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono relację między [ `Map` ](xref:Xamarin.Forms.Maps.Map) odpowiedniego natywne kontrolki, które ją implementują i:

![](customized-pin-images/map-classes.png "Relacja między kontrolki mapy i implementowanie natywne kontrolki")

Proces renderowania może służyć do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `Map` ](xref:Xamarin.Forms.Maps.Map) na każdej platformie. Proces ten jest w następujący sposób:

1. [Utwórz](#Creating_the_Custom_Map) mapę niestandardowego zestawu narzędzi Xamarin.Forms.
1. [Używanie](#Consuming_the_Custom_Map) Mapa niestandardowa z zestawu narzędzi Xamarin.Forms.
1. [Utwórz](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania dla mapy na każdej platformie.

Każdy element zostaną teraz dokładniej omówione w implementacji `CustomMap` renderowania wyświetlającą mapę natywnych przy użyciu dostosowanych numeru pin i dostosowany widok danych numeru pin na każdej platformie.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) musi być zainicjowana i skonfigurowana przed użyciem. Aby uzyskać więcej informacji, zobacz [`Maps Control`](~/xamarin-forms/user-interface/map.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Tworzenie niestandardowych Map

Kontrolki niestandardowe mapy mogą być tworzone przez podklasy [ `Map` ](xref:Xamarin.Forms.Maps.Map) klasy, jak pokazano w poniższym przykładzie kodu:

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap` Kontroli jest tworzony w projekcie biblioteki .NET Standard i definiuje interfejs API dla niestandardowych map. Mapa niestandardowa udostępnia `CustomPins` właściwość, która reprezentuje kolekcję `CustomPin` obiektów, które będzie renderowany przez formant mapy macierzysty na poszczególnych platformach. `CustomPin` Klasy przedstawiono w poniższym przykładzie kodu:

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

Ta klasa definiuje `CustomPin` jak dziedziczenie właściwości [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) klasy i dodanie `Url` właściwości.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Korzystanie z niestandardowych Map

`CustomMap` Kontroli może być przywoływany w XAML w projekcie biblioteki .NET Standard deklarowanie przestrzeni nazw dla lokalizacji i używając prefiksu przestrzeni nazw kontrolki niestandardowej mapy. Poniższy kod przedstawia przykładowy sposób, w jaki `CustomMap` kontrolki mogą być wykorzystane przez strony XAML:

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

`local` Prefiks przestrzeni nazw może mieć nazwę niczego. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły Mapa niestandardowa. Po zadeklarowaniu przestrzeń nazw prefiks jest używany do odwołania Mapa niestandardowa.

Poniższy kod przedstawia przykładowy sposób, w jaki `CustomMap` kontrolki mogą być wykorzystane przez strony C#:

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

`CustomMap` Wystąpienie zostanie użyte, aby wyświetlić mapę macierzysty na poszczególnych platformach. Ma ona [ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType) właściwość ustawia styl wyświetlania [ `Map` ](xref:Xamarin.Forms.Maps.Map), za pomocą możliwe wartości są zdefiniowane w [ `MapType` ](xref:Xamarin.Forms.Maps.MapType) wyliczenia. Dla systemów iOS i Android, szerokość i wysokość mapy jest ustawiony za pomocą właściwości `App` klasy, które są inicjowane w projektach specyficzne dla platformy.

Lokalizacja, mapy i numery PIN ją zawiera, są inicjowane, jak pokazano w poniższym przykładzie kodu:

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

Ten proces inicjowania dodaje niestandardowy numer pin i umieszcza widoku mapy z [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) metody, która zmienia położenie i poziom powiększenia mapy, tworząc [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) z [ `Position` ](xref:Xamarin.Forms.Maps.Position) i [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

Teraz można dodać do każdego projektu aplikacji, aby dostosować kontrolki mapy natywnych niestandardowego modułu renderowania.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania jest następująca:

1. Utwórz podklasę `MapRenderer` klasę, która renderuje Mapa niestandardowa.
1. Zastąp `OnElementChanged` metodę, która renderuje Mapa niestandardowa i pisania logiki zgodnie z własnymi. Ta metoda jest wywoływana, gdy zostanie utworzony odpowiedni Mapa niestandardowa zestawu narzędzi Xamarin.Forms.
1. Dodaj `ExportRenderer` atrybutów do klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania Mapa niestandardowa zestawu narzędzi Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania przy użyciu zestawu narzędzi Xamarin.Forms.

> [!NOTE]
> Jest to opcjonalne zapewnić niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej kontrolki będą używane.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](customized-pin-images/solution-structure.png "CustomMap niestandardowego modułu renderowania projektu obowiązki")

`CustomMap` Renderowania formantu przez klasy renderowania specyficzne dla platformy, które pochodzi od `MapRenderer` klasy dla każdej platformy. Skutkuje to każda `CustomMap` kontrolować renderowanego z formantami specyficzne dla platformy, jak pokazano na poniższych zrzutach ekranu:

![](customized-pin-images/screenshots.png "CustomMap na każdej platformie")

`MapRenderer` Klasy ujawnia `OnElementChanged` metody, która jest wywoływana podczas tworzenia mapy niestandardowego zestawu narzędzi Xamarin.Forms do renderowania odpowiedniej kontrolki natywne. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentują element zestawu narzędzi Xamarin.Forms, modułu renderowania *został* dołączyć i element zestawu narzędzi Xamarin.Forms, modułu renderowania *jest* dołączone do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie miała `null` i `NewElement` właściwość będzie zawierać odwołanie do `CustomMap` wystąpienia.

Zastąpione wersję `OnElementChanged` metody, w każdej klasie renderowania specyficzne dla platformy jest w tym miejscu Przeprowadź Dostosowywanie kontrolki natywne. Wpisane odwołania do kontrolki natywne używane na platformie jest możliwy za pośrednictwem `Control` właściwości. Ponadto można uzyskać odwołanie do formantu Xamarin.Forms, który jest renderowany przy użyciu `Element` właściwości.

Należy zachować ostrożność podczas subskrybowania obsługi zdarzeń w `OnElementChanged` metody, jak pokazano w poniższym przykładzie kodu:

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

Natywne kontrolki powinny być skonfigurowane i procedury obsługi zdarzeń zasubskrybować tylko wtedy, gdy niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Podobnie programy obsługi zdarzeń, które zostały zasubskrybowane przez powinna być usuwania subskrypcji tylko wtedy, gdy element, który modułu renderowania jest dołączony do zmieni. Przyjęcie tego podejścia pomoże utworzyć niestandardowego modułu renderowania, który nie odczuwają przecieków pamięci.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje modułu renderowania za pomocą zestawu narzędzi Xamarin.Forms. Ten atrybut przyjmuje dwa parametry — Nazwa typu formantu niestandardowego zestawu narzędzi Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono wykonania każdej klasy specyficzne dla platformy niestandardowego modułu renderowania.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie niestandardowego modułu renderowania w systemie iOS

Poniższe zrzuty ekranu Pokaż mapę przed dostosowaniem i po nim:

![](customized-pin-images/map-layout-ios.png "Kontrolki mapy przed dostosowaniem i po nim")

W systemie iOS numer pin jest nazywany *adnotacji*, i może być niestandardowego obrazu lub numeru pin zdefiniowaną przez system różnych kolorów. Adnotacje też opcjonalnie wyświetlić *objaśnienia*, który jest wyświetlany w odpowiedzi na użytkownika, zaznaczając adnotację. Zawiera objaśnienie `Label` i `Address` właściwości `Pin` wystąpienia przy użyciu widoków bezpośrednio akcesoriów i opcjonalnie po lewej stronie. Na powyższym zrzucie ekranu, po lewej stronie widoku akcesoriów jest obraz małp z są bezpośrednio akcesoriów widoku *informacji* przycisku.

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

`OnElementChanged` Metoda wykonuje następujący konfiguracji [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) wystąpienia, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms:

- [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) Właściwość jest ustawiona na `GetViewForAnnotation` metody. Ta metoda jest wywoływana, gdy [adnotacji stanie się widoczne na mapie](#Displaying_the_Annotation)i jest używany do dostosowywania przed adnotacji do wyświetlenia.
- Programy obsługi zdarzeń dla `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, i `DidDeselectAnnotationView` zdarzenia są rejestrowane. Wyzwolenie tych zdarzeń, gdy użytkownik [naciśnie odpowiednie akcesorium w objaśnieniu](#Tapping_on_the_Right_Callout_Accessory_View)i kiedy użytkownik [wybiera](#Selecting_the_Annotation) i [odznacza](#Deselecting_the_Annotation) adnotację, odpowiednio. Zdarzenia są usuwania subskrypcji tylko wtedy, gdy element renderer jest dołączony do zmiany.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Wyświetlanie adnotacji

`GetViewForAnnotation` Metoda jest wywoływana, gdy lokalizacja adnotacja staje się widoczny na mapie i jest używany do dostosowywania przed adnotacji, aby wyświetlić. Adnotacja ma dwie części:

- `MkAnnotation` — zawiera tytuł, podtytuł i lokalizacja adnotacji.
- `MkAnnotationView` — zawiera obraz do reprezentowania adnotacji i, opcjonalnie, objaśnienia, który jest wyświetlany, gdy użytkownik naciśnie adnotacji.

`GetViewForAnnotation` Metoda przyjmuje `IMKAnnotation` , zawiera dane, adnotacji i zwraca `MKAnnotationView` do wyświetlenia na mapie i przedstawiono w poniższym przykładzie kodu:

```csharp
protected override MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
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

Ta metoda zapewnia, że adnotacji, które będą wyświetlane jako obraz niestandardowy, a nie jako zdefiniowaną przez system numeru pin, a gdy dotknięcie adnotacji objaśnienia zostanie wyświetlony zawiera dodatkową zawartość w lewo i prawo do adnotacji tytuł i adres . Jest to realizowane w następujący sposób:

1. `GetCustomPin` Metoda jest wywoływana, aby zwrócić dane niestandardowego numeru pin dla adnotacji.
1. W celu zachowywania pamięci, wyświetlanie adnotacji jest puli do ponownego użycia z wywołaniem [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/).
1. `CustomMKAnnotationView` Klasa rozszerza `MKAnnotationView` klasy `Id` i `Url` właściwościami, które odpowiadają właściwości są takie same w `CustomPin` wystąpienia. Nowe wystąpienie klasy `CustomMKAnnotationView` zostanie utworzony, pod warunkiem, że adnotacja jest `null`:
    - `CustomMKAnnotationView.Image` Właściwość jest ustawiona na obraz, który będzie reprezentować adnotacji na mapie.
    - `CustomMKAnnotationView.CalloutOffset` Właściwość jest ustawiona na `CGPoint` określający, że objaśnienie będzie wyśrodkowana powyżej adnotacji.
    - `CustomMKAnnotationView.LeftCalloutAccessoryView` Właściwość jest ustawiona na obrazie małp, która będzie wyświetlana po lewej stronie adnotacji tytuł i adres.
    - `CustomMKAnnotationView.RightCalloutAccessoryView` Właściwość jest ustawiona na *informacji* przycisk, który będzie widoczny po prawej stronie adnotacji tytuł i adres.
    - `CustomMKAnnotationView.Id` Właściwość jest ustawiona na `CustomPin.Id` właściwości zwróconej przez `GetCustomPin` metody. Dzięki temu adnotacji identyfikację tak, aby miało [objaśnienia można dodatkowo dostosowywać](#Selecting_the_Annotation), jeśli to konieczne.
    - `CustomMKAnnotationView.Url` Właściwość jest ustawiona na `CustomPin.Url` właściwości zwróconej przez `GetCustomPin` metody. Adres URL nastąpi przejście, kiedy użytkownik [naciśnie przycisk wyświetlane w widoku akcesoriów prawo objaśnienia](#Tapping_on_the_Right_Callout_Accessory_View).
1. [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) Właściwość jest ustawiona na `true` tak, aby objaśnienie jest wyświetlane, gdy dotknięcie adnotacji.
1. Adnotacja są zwracane do wyświetlenia na mapie.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>Wybieranie adnotacji

Kiedy użytkownik naciska adnotację, `DidSelectAnnotationView` generowane zdarzenie, które z kolei wykonuje `OnDidSelectAnnotationView` metody:

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

Ta metoda jest rozszerzeniem istniejącej objaśnienia, (który zawiera widoki po lewej i prawej akcesoriów), dodając `UIView` wystąpienia do niego, które zawiera obraz logo platformy Xamarin, pod warunkiem, że została wybrana adnotacja jego `Id` właściwość ustawioną na `Xamarin`. Dzięki temu w scenariuszach, gdzie różne objaśnienia mogą być wyświetlane dla różnych adnotacji. `UIView` Wystąpienia będą wyświetlane, a ich tematyka ponad istniejących objaśnienia.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Naciskając widok prawej akcesorium objaśnienia

Po użytkownik naciska na *informacji* przycisk w widoku akcesoriów prawo objaśnienia `CalloutAccessoryControlTapped` generowane zdarzenie, które z kolei wykonuje `OnCalloutAccessoryControlTapped` metody:

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

Ta metoda otwiera przeglądarkę sieci web i przechodzi do adresem przechowywanym w `CustomMKAnnotationView.Url` właściwości. Należy pamiętać, że adres został zdefiniowany podczas tworzenia `CustomPin` kolekcji w projekcie biblioteki .NET Standard.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>Usuwanie adnotacji

Gdy adnotacja jest wyświetlany, a użytkownik naciska na mapie `DidDeselectAnnotationView` generowane zdarzenie, które z kolei wykonuje `OnDidDeselectAnnotationView` metody:

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

Ta metoda zapewnia, że objaśnienia istniejącego nie jest zaznaczone, rozszerzone część objaśnienia (obraz Xamarin logo) spowoduje również przerwanie, są wyświetlane, gdy jego zasoby zostaną zwolnione.

Aby uzyskać więcej informacji o dostosowywaniu `MKMapView` wystąpienia, zobacz [mapy iOS](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Tworzenie niestandardowego modułu renderowania w systemie Android

Poniższe zrzuty ekranu Pokaż mapę przed dostosowaniem i po nim:

![](customized-pin-images/map-layout-android.png "Kontrolki mapy przed dostosowaniem i po nim")

W systemie Android numer pin jest nazywany *znacznika*, i może być niestandardowego obrazu lub znacznik zdefiniowaną przez system różnych kolorów. Znaczniki można wyświetlić *okno informacyjne*, który jest wyświetlany w odpowiedzi na użytkownika, dotknięcie znacznika. Wyświetla okno informacyjne `Label` i `Address` właściwości `Pin` metodą wystąpienia oraz można dostosować tak, aby uwzględnić inną zawartość. Jednak tylko jedno okno informacje mogą być wyświetlane tylko raz.

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

Pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms `OnElementChanged` wywołania metody `MapView.GetMapAsync` metody, która pobiera bazowego `GoogleMap` , jest powiązany z widoku. Raz `GoogleMap` wystąpienie jest dostępne, `OnMapReady` zastąpienie zostanie wywołany. Ta metoda rejestruje zdarzenia obsługi dla `InfoWindowClick` zdarzenie, które są generowane, gdy [kliknięciu okno informacyjne](#Clicking_on_the_Info_Window)i jest Anulowano tylko wtedy, gdy element renderer jest dołączony do zmiany. `OnMapReady` Także Przesłoń wywołania `SetInfoWindowAdapter` metodę, aby określić, że `CustomMapRenderer` wystąpienia klasy zapewni metody służące do dostosowywania okno informacyjne.

`CustomMapRenderer` Klasy implementuje `GoogleMap.IInfoWindowAdapter` współpracować w celu [dostosować okno informacyjne](#Customizing_the_Info_Window). Ten interfejs określa, czy należy zaimplementować następujące metody:

- `public Android.Views.View GetInfoWindow(Marker marker)` — Ta metoda jest wywoływana w celu zwrócenia okna niestandardowych informacji dla znacznika. Jeśli zostanie zwrócona `null`, zostanie użyta domyślna renderowanie okien. Jeśli zostanie zwrócona `View`, który następnie `View` zostaną umieszczone wewnątrz ramki okna informacji.
- `public Android.Views.View GetInfoContents(Marker marker)` — Ta metoda jest wywoływana w celu zwrócenia `View` z zawartością okno informacyjne i zostanie wywołana tylko jeśli `GetInfoWindow` metoda zwraca `null`. Jeśli zostanie zwrócona `null`, zostaną użyte domyślne renderowanie informacji o zawartości okna.

W przykładowej aplikacji dostosowanych tylko informacje o zawartości okna, a więc `GetInfoWindow` metoda zwraca `null` umożliwiające wykonanie tej.

#### <a name="customizing-the-marker"></a>Dostosowywanie znacznika

Ikona używana do reprezentowania znacznik można dostosować przez wywołanie metody `MarkerOptions.SetIcon` metody. Można to osiągnąć przez zastąpienie `CreateMarker` metody, która jest wywoływana dla każdego `Pin` który został dodany do mapy:

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

Ta metoda tworzy nowy `MarkerOption` wystąpienia dla każdej `Pin` wystąpienia. Po ustawieniu pozycji, etykiety i adresu znacznika, jego ikonę została ustawiona za pomocą `SetIcon` metody. Ta metoda przyjmuje `BitmapDescriptor` obiekt zawierający dane wymagane do renderowania ikony, za pomocą `BitmapDescriptorFactory` klasy, zapewniając metody pomocnika, aby uprościć tworzenie `BitmapDescriptor`.

Aby uzyskać więcej informacji o korzystaniu z `BitmapDescriptorFactory` klasy, aby dostosować znacznik, zobacz [Dostosowywanie znacznik](~/android/platform/maps-and-location/maps/maps-api.md).

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Dostosowywanie okna informacji

Kiedy użytkownik naciska na znacznika, `GetInfoContents` metoda jest wykonywana, pod warunkiem, że `GetInfoWindow` metoda zwraca `null`. Poniższy kod przedstawia przykład `GetInfoContents` metody:

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

Ta metoda zwraca `View` z zawartością okna informacji. Jest to realizowane w następujący sposób:

- A `LayoutInflater` wystąpienia są pobierane. Służy to do utworzenia wystąpienia pliku XML układu do odpowiadającymi mu dostawcami `View`.
- `GetCustomPin` Metoda jest wywoływana, aby zwrócić dane niestandardowego numeru pin dla okna informacji.
- `XamarinMapInfoWindow` Układ jest zwiększony, jeśli `CustomPin.Id` właściwości jest równa `Xamarin`. W przeciwnym razie `MapInfoWindow` układ jest zwiększony. Dzięki temu w scenariuszach, gdzie mogą zostać wyświetlone informacje o różnych układów okna różnych znaczników.
- `InfoWindowTitle` i `InfoWindowSubtitle` zasobów są pobierane z nadmuchany układ i ich `Text` właściwości są ustawione na odpowiadające im dane z `Marker` wystąpienia, pod warunkiem, że zasoby są `null`.
- `View` Wystąpienia są zwracane do wyświetlenia na mapie.

> [!NOTE]
> Okno informacji nie jest na żywo `View`. Zamiast tego zostanie przekonwertowana Android `View` na statyczną mapy bitowej i wyświetlenie jej w formie obrazu. Oznacza to, że podczas okna informacji może odpowiadać na zdarzenie click, nie może ona odpowiedzieć na wszelkie zdarzenia dotykowe lub gestów i pojedynczych formantów w oknie informacji nie może odpowiadać na ich własnych zdarzeń kliknięcia.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>Kliknij okno informacje

Gdy użytkownik kliknie okno informacyjne `InfoWindowClick` generowane zdarzenie, które z kolei wykonuje `OnInfoWindowClick` metody:

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

Ta metoda otwiera przeglądarkę sieci web i przechodzi do adresem przechowywanym w `Url` właściwość pobrany `CustomPin` wystąpienie `Marker`. Należy pamiętać, że adres został zdefiniowany podczas tworzenia `CustomPin` kolekcji w projekcie biblioteki .NET Standard.

Aby uzyskać więcej informacji o dostosowywaniu `MapView` wystąpienia, zobacz [interfejsu API usługi mapy](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Tworzenie niestandardowego modułu renderowania na platformie Universal Windows

Poniższe zrzuty ekranu Pokaż mapę przed dostosowaniem i po nim:

![](customized-pin-images/map-layout-uwp.png "Kontrolki mapy przed dostosowaniem i po nim")

Na platformy uniwersalnej systemu Windows jest nazywany numeru pin *ikona mapy*, i może być niestandardowego obrazu lub zdefiniowaną przez system domyślnego obrazu. Ikona mapy może wyświetlić `UserControl`, który jest wyświetlany w odpowiedzi na użytkownika, naciskając ikonę mapy. `UserControl` Można wyświetlić dowolną zawartość, łącznie z `Label` i `Address` właściwości `Pin` wystąpienia.

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

`OnElementChanged` Metoda wykonuje następujące operacje, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms:

- To powoduje wyczyszczenie `MapControl.Children` kolekcji, aby usunąć wszystkie istniejące elementy interfejsu użytkownika z mapy, przed zarejestrowaniem program obsługi zdarzeń dla `MapElementClick` zdarzeń. To zdarzenie jest uruchamiana, gdy użytkownik naciśnie lub kliknie `MapElement` na `MapControl`i jest Anulowano tylko wtedy, gdy element renderer jest dołączony do zmiany.
- Każdy numer pin w `customPins` Kolekcja zostanie wyświetlona w poprawnej lokalizacji geograficznej na mapie w następujący sposób:
  - Lokalizacja o numer pin jest tworzona jako `Geopoint` wystąpienia.
  - A `MapIcon` wystąpienie jest tworzone do reprezentowania numeru pin.
  - Obraz używany do reprezentowania `MapIcon` jest określany przez ustawienie `MapIcon.Image` właściwości. Jednak obraz ikony mapy nie zawsze musi pokazano, jak mogą być zasłonięte przez inne elementy na mapie. W związku z tym, mapa ikony `CollisionBehaviorDesired` właściwość jest ustawiona na `MapElementCollisionBehavior.RemainVisible`, aby upewnić się, że pozostaje widoczna.
  - Lokalizacja `MapIcon` jest określany przez ustawienie `MapIcon.Location` właściwości.
  - `MapIcon.NormalizedAnchorPoint` Właściwość jest ustawiona na przybliżona lokalizacja wskaźnika w obrazie. Jeśli ta właściwość zachowuje wartość domyślną (0,0), który reprezentuje lewym górnym rogu obrazu, zmiany w poziom powiększenia mapy może spowodować obraz, który wskazuje do innej lokalizacji.
  - `MapIcon` Wystąpienia jest dodawany do `MapControl.MapElements` kolekcji. Skutkuje to ikona mapy są wyświetlane na `MapControl`.

> [!NOTE]
> W przypadku używania tego samego obrazu dla wielu ikon mapy, `RandomAccessStreamReference` wystąpienia powinien być zadeklarowany jako na poziomie strony lub aplikacji uzyskać najlepszą wydajność.

#### <a name="displaying-the-usercontrol"></a>Wyświetlanie UserControl

Gdy użytkownik wybiera ikonę mapy `OnMapElementClick` metoda jest wykonywana. Poniższy przykład kodu pokazuje tę metodę:

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

Ta metoda tworzy `UserControl` wystąpienie, które wyświetla informacje o numer pin. Jest to realizowane w następujący sposób:

- `MapIcon` Wystąpienia są pobierane.
- `GetCustomPin` Metoda jest wywoływana, aby zwrócić dane niestandardowego numeru pin, który będzie wyświetlany.
- A `XamarinMapOverlay` tworzone jest wystąpienie, aby wyświetlić dane niestandardowego numeru pin. Ta klasa jest kontrolki użytkownika.
- Lokalizacja geograficzna, od którego należy wyświetlić `XamarinMapOverlay` wystąpienia na `MapControl` jest tworzona jako `Geopoint` wystąpienia.
- `XamarinMapOverlay` Wystąpienia jest dodawany do `MapControl.Children` kolekcji. Ta kolekcja zawiera elementy interfejsu użytkownika XAML, które będą wyświetlane na mapie.
- Lokalizacja geograficzna `XamarinMapOverlay` wystąpienia na mapie jest ustawiony przez wywołanie metody `SetLocation` metody.
- Względna lokalizacja w `XamarinMapOverlay` wystąpienia, która odnosi się do określonej lokalizacji, jest ustawiony przez wywołanie metody `SetNormalizedAnchorPoint` metody. Daje to gwarancję, które zmieniają się poziom powiększenia mapy doprowadzić `XamarinMapOverlay` wystąpienie, zawsze wyświetlane we właściwych lokalizacjach.

Alternatywnie, jeśli już są wyświetlane informacje o numer pin na mapie, naciskając pozycję na mapie spowoduje usunięcie `XamarinMapOverlay` wystąpienia z `MapControl.Children` kolekcji.

#### <a name="tapping-on-the-information-button"></a>Naciskając przycisk informacje

Kiedy użytkownik naciska na *informacji* znajdujący się w `XamarinMapOverlay` kontrolki użytkownika `Tapped` generowane zdarzenie, które z kolei wykonuje `OnInfoButtonTapped` metody:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Ta metoda otwiera przeglądarkę sieci web i przechodzi do adresem przechowywanym w `Url` właściwość `CustomPin` wystąpienia. Należy pamiętać, że adres został zdefiniowany podczas tworzenia `CustomPin` kolekcji w projekcie biblioteki .NET Standard.

Aby uzyskać więcej informacji o dostosowywaniu `MapControl` wystąpienia, zobacz [Przegląd lokalizacja i mapy](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) w witrynie MSDN.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla `Map` kontrolki, dzięki czemu deweloperzy mogą zastąpić domyślne renderowanie natywnych, dostosowując swoje własne specyficzne dla platformy. Projekt xamarin.Forms.Maps dla zapewnia abstrakcję dla wielu platform, które wyświetlanie map, korzystających z mapy natywnych interfejsów API na każdej platformie zapewnienie mapę szybkie i dobrze znane środowisko dla użytkowników.


## <a name="related-links"></a>Linki pokrewne

- [Kontrolki mapy](~/xamarin-forms/user-interface/map.md)
- [Mapy dla systemu iOS](~/ios/user-interface/controls/ios-maps/index.md)
- [Interfejs API map](~/android/platform/maps-and-location/maps/maps-api.md)
- [Przypnij dostosowany (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
