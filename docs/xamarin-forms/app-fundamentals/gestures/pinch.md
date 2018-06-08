---
title: Dodawanie rozpoznawania gestów Uszczypnięcia
description: Gest uszczypnięcia służy do wykonywania interakcyjne powiększenia i jest realizowana za pomocą klasy PinchGestureRecognizer. Typowy scenariusz dla gestu uszczypnięcia jest przeprowadzenie interakcyjne powiększenia obrazu w lokalizacji uszczypnięcia. Odbywa się przez skalowanie zawartość okienka ekranu, a przedstawionej w tym artykule.
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: b2348a1f0dfacc4a7a0e37f5c9041a07217ff802
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846116"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Dodawanie rozpoznawania gestów Uszczypnięcia

_Gest uszczypnięcia służy do wykonywania interakcyjne powiększenia i jest realizowana za pomocą klasy PinchGestureRecognizer. Typowy scenariusz dla gestu uszczypnięcia jest przeprowadzenie interakcyjne powiększenia obrazu w lokalizacji uszczypnięcia. Odbywa się przez skalowanie zawartość okienka ekranu, a przedstawionej w tym artykule._

## <a name="overview"></a>Omówienie

Aby elementu interfejsu użytkownika którą można powiększać z gestów uszczypnięcia, Utwórz [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) wystąpienia i obsługiwać [ `PinchUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PinchGestureRecognizer.PinchUpdated/) zdarzeń, i Dodaj nowy aparat rozpoznawania gestów do [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) kolekcji dla elementu interfejsu użytkownika. Poniższy kod przedstawia przykład `PinchGestureRecognizer` dołączony do [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) elementu:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Ponadto można to osiągnąć w języku XAML, jak pokazano w poniższym przykładzie:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kod `OnPinchUpdated` program obsługi zdarzeń jest następnie dodawana do pliku CodeBehind:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>Tworzenie kontenera PinchToZoom

Obsługa gestu uszczypnięcia do wykonania operacji powiększenia wymaga niektórych matematyczne do przekształcania interfejsu użytkownika. Ta sekcja zawiera klasę pomocnika uogólniony przeprowadzić obliczenia, która może służyć do interaktywnego powiększenie dowolnego elementu interfejsu użytkownika. Poniższy kod przedstawia przykład `PinchToZoomContainer` klasy:

```csharp
public class PinchToZoomContainer : ContentView
{
  ...

  public PinchToZoomContainer ()
  {
    var pinchGesture = new PinchGestureRecognizer ();
    pinchGesture.PinchUpdated += OnPinchUpdated;
    GestureRecognizers.Add (pinchGesture);
  }

  void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
  {
    ...
  }
}
```

Ta klasa może otaczający element interfejsu użytkownika tak, aby gestu uszczypnięcia zostanie powiększony elementu interfejsu użytkownika opakowana. Przedstawia poniższy przykładowy kod XAML `PinchToZoomContainer` zawijanie [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) elementu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PinchGesture;assembly=PinchGesture"
             x:Class="PinchGesture.HomePage">
    <ContentPage.Content>
        <Grid Padding="20">
            <local:PinchToZoomContainer>
                <local:PinchToZoomContainer.Content>
                    <Image Source="waterfront.jpg" />
                </local:PinchToZoomContainer.Content>
            </local:PinchToZoomContainer>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Poniższy kod przedstawia przykład sposobu `PinchToZoomContainer` opakowuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) elementu na stronie C#:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new Grid {
      Padding = new Thickness (20),
      Children = {
        new PinchToZoomContainer {
          Content = new Image { Source = ImageSource.FromFile ("waterfront.jpg") }
        }
      }
    };
  }
}
```

Gdy [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element odbiera gestem uszczypnięcia, wyświetlany obraz będzie być powiększony w ani out. Wartość powiększenia jest wykonywane przez `PinchZoomContainer.OnPinchUpdated` metodę, która jest wyświetlana w poniższym przykładzie:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  if (e.Status == GestureStatus.Started) {
    // Store the current scale factor applied to the wrapped user interface element,
    // and zero the components for the center point of the translate transform.
    startScale = Content.Scale;
    Content.AnchorX = 0;
    Content.AnchorY = 0;
  }
  if (e.Status == GestureStatus.Running) {
    // Calculate the scale factor to be applied.
    currentScale += (e.Scale - 1) * startScale;
    currentScale = Math.Max (1, currentScale);

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the X pixel coordinate.
    double renderedX = Content.X + xOffset;
    double deltaX = renderedX / Width;
    double deltaWidth = Width / (Content.Width * startScale);
    double originX = (e.ScaleOrigin.X - deltaX) * deltaWidth;

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the Y pixel coordinate.
    double renderedY = Content.Y + yOffset;
    double deltaY = renderedY / Height;
    double deltaHeight = Height / (Content.Height * startScale);
    double originY = (e.ScaleOrigin.Y - deltaY) * deltaHeight;

    // Calculate the transformed element pixel coordinates.
    double targetX = xOffset - (originX * Content.Width) * (currentScale - startScale);
    double targetY = yOffset - (originY * Content.Height) * (currentScale - startScale);

    // Apply translation based on the change in origin.
    Content.TranslationX = targetX.Clamp (-Content.Width * (currentScale - 1), 0);
    Content.TranslationY = targetY.Clamp (-Content.Height * (currentScale - 1), 0);

    // Apply scale factor.
    Content.Scale = currentScale;
  }
  if (e.Status == GestureStatus.Completed) {
    // Store the translation delta's of the wrapped user interface element.
    xOffset = Content.TranslationX;
    yOffset = Content.TranslationY;
  }
}
```

Ta metoda aktualizacji poziom powiększenia elementu interfejsu użytkownika opakowana oparte na gestu uszczypnięcia użytkownika. Jest to osiągane przy użyciu wartości [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale/), [ `ScaleOrigin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin/) i [ `Status` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Status/) właściwości [ `PinchGestureUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureUpdatedEventArgs/) wystąpienia można obliczyć współczynnik skali, które mają być zastosowane na pochodzenie gestu uszczypnięcia. Element opakowana użytkownika następnie powiększone źródłem gestu uszczypnięcia przez ustawienie jej [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/), i [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) właściwości do obliczonych wartości.

## <a name="summary"></a>Podsumowanie

Gest uszczypnięcia służy do wykonywania interakcyjne powiększenia i jest realizowana za pomocą [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) klasy.


## <a name="related-links"></a>Linki pokrewne

- [PinchGesture (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PinchGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/)
